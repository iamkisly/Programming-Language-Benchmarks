# [Programming Language Benchmarks](https://programming-language-benchmarks.vercel.app/)

[![bench](https://github.com/hanabi1224/Programming-Language-Benchmarks/actions/workflows/bench.yml/badge.svg)](https://github.com/hanabi1224/Programming-Language-Benchmarks/actions/workflows/bench.yml)
[![MIT License](https://img.shields.io/github/license/hanabi1224/Programming-Language-Benchmarks.svg)](https://github.com/hanabi1224/Programming-Language-Benchmarks/blob/master/LICENSE)

<!-- [![Build status](https://img.shields.io/appveyor/ci/hanabi1224/Programming-Language-Benchmarks/main.svg)](https://ci.appveyor.com/project/hanabi1224/Programming-Language-Benchmarks) -->

# Why Build This

The idea is to build an automated process for benchmark generation and publishing.

### Comparable numbers

_It currently use CI to generate benchmark results to guarantee all the numbers are generated from the same environment at nearly the same time. All benchmark tests are executed in a single CI job_

### Automatic publishing

_Once a change is merged into main branch, the CI job will re-generate and publish the static website_

## Main Goals

- Compare performance differences between different languages. Note that implementations might be using different optimizations, e.g. with or w/o multithreading, please do read the source code to check if it's a fair comparison or not.
- Compare performance differences between different compilers or runtimes of the same language with the same source code.
- Facilitate benchmarking on real server environments as nowadays more and more applications are deployed with docker/k8s. It's likely to get a very different result from what you get on your dev machine.
- A reference for CI setup / Dev environment setup / package management setup for different languages. Refer to [Github action](https://github.com/hanabi1224/Programming-Language-Benchmarks/blob/main/.github/workflows/bench.yml)
- It focuses more on new programming languages, classic
  programming languages that are covered by [CLBG](https://benchmarksgame-team.pages.debian.net/benchmarksgame/index.html) receive limited or no maintenance, based on their popularity.

# [Website](https://programming-language-benchmarks.vercel.app/)

### Build

To achieve better SEO, the published site is static and prerendered, powered by [nuxt.js](https://nuxtjs.org/).

### Host

The website is hosted on [Vercel](https://vercel.com/)

### Development

```
git clone https://github.com/hanabi1224/Programming-Language-Benchmarks.git

cd website
pnpm i
pnpm build
pnpm dev
```

# Benchmarks

_All benchmarks are defined in [bench.yaml](https://github.com/hanabi1224/Programming-Language-Benchmarks/blob/main/bench/bench.yaml)_

_Current benchmarks problems and their implementations are from [The Computer Language Benchmarks Game](https://benchmarksgame-team.pages.debian.net/benchmarksgame/) ([ Repo](https://salsa.debian.org/benchmarksgame-team/benchmarksgame/))_

# Local development

## Prerequisites

[net9](https://dotnet.microsoft.com/)

[nodejs 14](https://nodejs.org/)

[pnpm](https://pnpm.io/installation)

[podman](https://podman.io/getting-started/installation) (or [docker](https://www.docker.com/) by changing `docker_cmd: podman` to `docker_cmd: docker` in `bench/bench.yaml`)

## Build

_The 1st step is to build source code from various of languages_

```bash
cd bench
# To build a subset
dotnet run -p tool -- --task build --langs lisp go --problems nbody helloworld --force-rebuild
# To build all
dotnet run -p tool -- --task build
```

## Test

_The 2nd step is to test built binaries to ensure the correctness of their implementation_

```bash
cd bench
# To test a subset
dotnet run -p tool -- --task test --langs lisp go --problems nbody helloworld
# To test all
dotnet run -p tool -- --task test
```

## Bench

_The 3rd step is to generate benchmarks_

```bash
cd bench
# To bench a subset
dotnet run -p tool -- --task bench --langs lisp go --problems nbody helloworld
# To bench all
dotnet run -p tool -- --task bench
```

_For usage_

```bash
cd bench
dotnet run -p tool -- -h

BenchTool
  Main function

Usage:
  BenchTool [options]

Options:
  --config <config>              Path to benchmark config file [default: bench.yaml]
  --algorithm <algorithm>        Root path that contains all algorithm code [default: algorithm]
  --include <include>            Root path that contains all include project templates [default: include]
  --build-output <build-output>  Output folder of build step [default: build]
  --task <task>                  Benchmark task to run, valid values: build, test, bench [default: build]
  --force-pull-docker            A flag that indicates whether to force pull docker image even when it exists [default: False]
  --force-rebuild                A flag that indicates whether to force rebuild [default: False]
  --fail-fast                    A Flag that indicates whether to fail fast when error occurs [default: False]
  --build-pool                   A flag that indicates whether builds that can run in parallel [default: False]
  --verbose                      A Flag that indicates whether to print verbose information [default: False]
  --no-docker                    A Flag that forces disabling docker [default: False]
  --langs <langs>                Languages to include, e.g. --langs go csharp [default: ]
  --problems <problems>          Problems to include, e.g. --problems binarytrees nbody [default: ]
  --environments <environments>  OS environments to include, e.g. --environments linux windows [default: ]
  --version                      Show version information
  -?, -h, --help                 Show help and usage information
```

## Refresh website

_Lastly you can re-generate website with latest benchmark numbers_

```
cd website
pnpm i
pnpm content
pnpm build
serve dist
```

# TODOs

Integrate test environment info into website

Integrate build / test / benchmark information into website

...

# How to contribute

TODO

# Thanks

_This is inspired by [The Computer Language Benchmarks Game](https://benchmarksgame-team.pages.debian.net/benchmarksgame/), thanks to the curator._

# LICENSES

Code of problem implementation from [The Computer Language Benchmarks Game](https://salsa.debian.org/benchmarksgame-team/benchmarksgame/) is under their [Revised BSD](https://benchmarksgame-team.pages.debian.net/benchmarksgame/license.html)

Other code in this repo is under MIT.
