# fcp

`fcp` is a [significantly faster](#benchmarks) alternative to the classic Unix [`cp(1)`](https://man7.org/linux/man-pages/man1/cp.1.html) command.

`fcp` aims to handle the most common use-cases for `cp` with much higher performance.

`fcp` does _not_ aim to completely replace `cp` with its myriad options.

## Installation

### Pre-built binaries

Pre-built binaries for some systems can be found under [this repository's releases](https://github.com/Svetlitski/fcp/releases).

### Via [`cargo`](https://github.com/rust-lang/cargo)

`fcp` can be installed using `cargo` by running the following:

```sh
cargo install fcp
```

## Usage

Usage information can be found by running `fcp --help`, and has been reproduced below:

```
fcp

USAGE:
    fcp SOURCE DESTINATION_FILE
    Copy SOURCE to DESTINATION_FILE, overwriting DESTINATION_FILE if it exists

    fcp SOURCE ... DESTINATION_DIRECTORY
    Copy each SOURCE into DESTINATION_DIRECTORY
```

## Benchmarks

`fcp` doesn't just _claim_ to be faster than `cp`, it _is_ faster than `cp`. As different operating systems display
different performance characteristics, the same benchmarks were run on both macOS and Linux.

### macOS

The following benchmarks were run on a 2018 MacBook Pro<sup><a href="#footnote-2">2</a></sup> (2.9 GHz 6-Core Intel Core i9, 16 GiB RAM, SSD)

#### Large Files

The following shows the result of a benchmark which copies a directory containing 13 different 512 MB files using `cp` and `fcp`, with `fcp` being approximately **822x faster** on average (note the units of the x-axis for each plot)<sup><a href="#footnote-3">3</a></sup>:

![`fcp` is approximately 822x faster than `cp`, with `fcp`'s average time to copy being approximately 4.5 milliseconds, while `cp`'s average time to copy is approximately 3.7 seconds](https://user-images.githubusercontent.com/35482043/122131973-a3990080-cdff-11eb-92dc-3e0d5f47ac07.png)

#### Linux Kernel Source

The following shows the result of a benchmark which copies the source tree of the Linux kernel using `cp` and `fcp`, with `fcp` being approximately 6x faster on average:

![`fcp` is approximately 6x faster than `cp`, with `fcp`'s average time to copy being approximately 5.1 seconds, while `cp`'s average time to copy is approximately 30 seconds](https://user-images.githubusercontent.com/35482043/122131983-a7c51e00-cdff-11eb-8bbb-8c768998de56.png)

### Linux

The following benchmarks were run on a bare-metal AWS EC2 instance (a1.metal, 16 CPUs, 32 GiB RAM, SSD)

#### Linux Kernel Source

The following shows the result of a benchmark which copies the source tree of the Linux kernel using `cp` and `fcp`, with `fcp` being approximately 10x faster on average:

![`fcp` is nearly 10x faster than `cp`, with `fcp`'s average time to copy being approximately 675 milliseconds, while `cp`'s average time to copy is approximately 6.02 seconds](https://user-images.githubusercontent.com/35482043/122125946-ae9b6300-cdf6-11eb-97dd-0e0bfb916ede.png)

#### Large Files

The following shows the result of a benchmark which copies a directory containing 13 different 512 MB files using `cp` and `fcp`, with `fcp` being approximately 1.4x faster on average:

![`fcp` is approximately 1.4x faster than `cp, with `fcp`'s average time to copy being approximately 8 seconds, while `cp`'s average time to copy is approximately 11.3 seconds](https://user-images.githubusercontent.com/35482043/122125941-ae02cc80-cdf6-11eb-9899-a93ed0442f6f.png)



[1] See the [benchmarks](#benchmarks).

<span id="footnote-2">[2]</span> While in general [you should avoid benchmarking on
laptops](https://lemire.me/blog/my-sayings/), `fcp` is a developer tool and
many developers work primarily on laptops. Also unlike with Linux where you can
rent by the second, the minimum tenancy for AWS EC2 macOS instances is 24
hours, and these benchmarks took less than an hour.

<span id="footnote-3">[3]</span> The massive difference in performance in this case is due
to `fcp` using `fclonefileat` and `fcopyfile` under the hood.