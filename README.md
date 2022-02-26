# Alchemy: **A**ttribute-**L**evel **C**aching in **He**terogeneous In-**M**emor**y** DBMS

![caching](https://github.com/XiangpengHao/clock-cache/workflows/caching/badge.svg)
[![codecov](https://codecov.io/gh/XiangpengHao/clock-cache/branch/master/graph/badge.svg?token=YS06KZ1RYS)](https://codecov.io/gh/XiangpengHao/clock-cache)


Scalable persistent memory (PM) can potentially lower the cost and increase the capacity of main-memory database systems by replacing or complementing DRAM. However, modern main-memory database systems often mandate memory bandwidth beyond what commercial PM devices can offer, yet existing DRAM-based caching strategies do not fully utilize PM’s byte-addressability, leading to suboptimal performance.

Alchemy is a DRAM-PM hybrid database engine built from scratch to achieve high performance at a low cost. The key is to leverages PM’s byte-addressability to cache only hot attributes in DRAM, reducing requirements on PM bandwidth. To mitigate the higher latency of PM, Alchemy leverages lightweight coroutines to overlap data fetching and computation, effectively hiding CPU stalls. Experiments using Intel Optane DCPMM show that Alchemy is up to 2×faster and 2.17×more cost-effective than highly-optimized, traditional DRAM-based designs.


### Build
Alchemy require nightly rust to build.
```bash
cargo build . --release
```

### Test

Run unit test
```bash
cargo test
```

To test with sanitizers:
```bash
env ASAN_OPTIONS="detect_odr_violation=0" RUSTFLAGS="-Z sanitizer=address" cargo test -Zbuild-std --target x86_64-unknown-linux-gnu --features asans -- --skip test_alloc_oom
```


### Benchmark
Alchemy has microbenchmarks and TPC-C benchmark.
To run microbenchmark
```bash
cargo run --bin table --release "test-name"
```

To run TPC-C benchmark
```bash
cargo run --bin tpcc --release "new-order"
```

Alchemy collects comprehensive runtime statistics.
To enable flamegraph support, add `flamegraph` feature;
To enable Alchemy internal metrics, add `metric` feature;
To enable PCM counter support, add `pcm` feature.
For example:
```bash
cargo run --bin table --release "benchmark-name" --features "metric pcm flamegraph"
```

All benchmark results will be redirected to a json file.

More details please check out our paper.


