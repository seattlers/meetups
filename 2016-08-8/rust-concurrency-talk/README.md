# Rust Concurrency Talk

by: Chris Griffing

Slides available via my personal repo:
(https://cmgriffing.github.io/rust-concurrency-talk/)[https://cmgriffing.github.io/rust-concurrency-talk]

I ended up benchmarking things:

    test slide_01_setup::benchmark_the_operation         ... bench:   5,890,521 ns/iter (+/- 440,129)
    test slide_02_standard_library::benchmark_channels   ... bench:   3,839,737 ns/iter (+/- 285,787)
    test slide_02_standard_library::benchmark_join       ... bench:   3,816,445 ns/iter (+/- 279,266)
    test slide_03_coroutines::benchmark_mioco_coroutines ... bench:  16,135,118 ns/iter (+/- 2,343,957)
    test slide_04_futures::benchmark_futures             ... bench:   3,822,760 ns/iter (+/- 314,886)
    test slide_04_futures::benchmark_promises            ... bench:   5,964,280 ns/iter (+/- 456,764)
    test slide_05_async_await::benchmark_async_await     ... bench:   3,828,543 ns/iter (+/- 276,901)

Looking at these benchmarks, it appears I did something terribly wrong in my mioco code.
Also keep in mind that the promises benchmark is using sleep like the rest of them, but it is on top of a single threaded event loop which causes the function to run in a similar amount of time to the raw operation benchmark.
