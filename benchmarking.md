# Google Benchmark

Google Benchmark reports wall time, CPU time, and iterations per benchmark.

```bash
-----------------------------------------------------
Benchmark           Time             CPU   Iterations
-----------------------------------------------------
BM_StringCreation  5.09 ns         5.07 ns  136987124
```

## Basic Usage

`benchmark::State` controls the measurement loop. Iterating over it (`for (auto _ : state)`) runs
the loop body as many times as the framework needs to get a statistically stable measurement. Google
Benchmark dynamically adjusts the iteration count, scaling up until the total runtime meets a
minimum threshold.

```cpp
#include <benchmark/benchmark.h>

static void BM_StringCreation(benchmark::State& state) {
    for (auto _ : state) {
        std::string empty;
    }
}
BENCHMARK(BM_StringCreation);

BENCHMARK_MAIN();
```

`BENCHMARK(fn)` registers a function as a benchmark. It creates a static object that adds the
function to an internal registry so the framework knows to run it.

`BENCHMARK_MAIN()` expands to a `main()` function that initialises the library, parses command-line
flags (like `--benchmark_filter`), runs all registered benchmarks, and reports results.

## Preventing Optimisation

Use `benchmark::DoNotOptimize()` to stop the compiler from eliding the work you are trying to
measure:

```cpp
static void BM_VectorPush(benchmark::State& state) {
    for (auto _ : state) {
        std::vector<int> v;
        v.push_back(42);
        benchmark::DoNotOptimize(v.data());
    }
}
```
