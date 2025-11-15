# Running CUDA Kernels From Rust (Using Rust's FFI)

- Create a `shared library` of CUDA kernels (decorating public kernels with `extern "C"`):

```cuda
extern "C" __global__ void add_arrays(const float* a, const float* b, float* c, const int len) {
  // whatever ...
}
```

- Declare in rust the kernels that you want to use:

```rust
#[link(name = "my_kernel")] // Assuming the shared lib is libmy_kernel.so
extern "C" {
    fn add_arrays(a: *const f32, b: *const f32, c: *mut f32, n: i32);
}
```

- Call your rust functions in `unsafe` blocks:

```rust
fn my_rust_func() {
  // whatever ...
  unsafe {
    add_arrays(a.as_ptr(), b.as_ptr(), c.as_mut_ptr(), n as i32);
  }
  // whatever ...
}
```
