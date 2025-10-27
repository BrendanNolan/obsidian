# Allocating and Freeing Memory

To allocate memory on the devide, use `cudaMalloc`:

```cpp
cudaError_t cudaMalloc(void** devPtr, size_t size);

```

The `cudaError_t` type has a `cudaSuccess` enumerator to indicate (obviously) that the allocation
succeeded. Call it like this:

```cpp
void* device_memory;
const auto success_status = cudaMalloc(&device_memory, 100);
```

# Cuda Kernels

A `kernel` is a function that runs on the device; if it is marked as `__global__`, then it can be
called from the device or from the host. You provide a `launch configuration` when calling a CUDA
kernel:

```cpp
add<<<1, 1>>>(N, sum, x, y);
```

The host will not wait until the kernel executes; to force the host to wait until all previously
issued CUDA calls (kernels, asynchronous memory copies, etc.), call `cudaDeviceSynchronize` from the
host.

# Grids, Blocks, Threads, Warps

Threads are grouped into 3D structures called `blocks` and blocks are grouped into 3D structures
called `grids`. There is only ever one active grid per kernel launch. CUDA GPUs have many parallel
processors grouped into Streaming Multiprocessors, or SMs. Each SM can run multiple concurrent
thread blocks, but each thread block runs on a single SM.

You launch a kernel with a `launch configuration` specifying the grid dim and the block dim
(specified with CUDA's `dim3` data structure), e.g.:

```cpp
const auto grid_dim = dim3{2, 2, 2};
const auto block_dim = dim3{2, 2, 2};
add<<<grid_dim, block_dim>>>(N, sum, x, y);
```

If your grid or block dimensions don't use all three axes, you can omit them and they will default
to `1`; so that `const auto grid_dim = dim3{2, 1, 1}` can be shortened to
`const auto grid_dim = dim3{2}` and you can even do stuff like

```cpp
add<<<1, 1>>>(N, sum, x, y);
```

Finally, a `warp` is a set of threads (all belonging to one block) that execute in lock-step (SIMD
style). If threads in a warp suffer `warp divergence` (meaning that there is a conditional where not
all threads of the warp follow the same branch), then the warp slows down because it has to execute
the threads that followed one branch in lock step, then the threads that followd the other branch in
lock step.

# Unit-Stride Addressing

If consecutive threads access consecutive memory locations, this is called `unit-stride` addressing,
and is good for caching reasons (it is particularly important to make addressing within a warp unit
stride because the device coalesces global memory loads and stores issued by threads of a warp into
as few transactions as possible to minimize DRAM bandwidth).

# Profiling

There is a simple script on my path called `nsys_easy` that is worth a look.

# Grid-Stride Loops

**N.B. This discussion of grid-stride loops assumes that the kernel will be launched with a launch
configuration of a `1`-dimensional grid of `1`-dimensional blocks.**

(Taken from the nvidia developer website). Consider the simple vector operation known as "SAXPY":
`y = a.x + y`, where `x,y` are vectors and `a` is a scalar. Common CUDA guidance is to launch one
thread per data element, which means to parallelize the above SAXPY loop we write a kernel that
assumes we have enough threads to more than cover the array size.

```cpp
__global__
void saxpy(const int n, const float a, const float* x, float* y) {
    const int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n)
        y[i] = a * x[i] + y[i];
}
```

This is called a `monolithic kernel` because it assumes a single grid of threads large enough to
process the entire array in a single pass; you might launch it on a large array like this:

```cpp
saxpy<<<4096,256>>>(1<<20, 2.0, x, y);
```

Rather than eliminating (in comparison to the plain `cpp` implementation) the loop, it is
recommended to use a `grid-stride loop`:

```cpp
__global__
void saxpy(const int n, const float a, const float* x, float* y) {
    const auto thread_count = blockDim.x * gridDim.x; // Notice that this will be our stride length
    for (auto i = blockIdx.x * blockDim.x + threadIdx.x; i < n; i += thread_count)
        y[i] = a * x[i] + y[i];
}
```

By using a loop, you can support any problem size even if it exceeds the largest grid size your
device supports. Moreover, you can limit the number of blocks you use to tune performance. For
example, it’s often useful to launch a number of blocks that is a multiple of the number of
multiprocessors on the device, to balance utilization. Moreover, by using a loop instead of a
monolithic kernel, you can easily switch to serial processing by launching one block with one
thread.

**Remark** In both the monolithic and grid-stride case, all addressing within warps is unit-stride
(see [[#Unit-Stride Addressing]]).
