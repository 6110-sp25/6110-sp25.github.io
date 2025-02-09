---
title: Autograder
parent: Class Tools
nav_order: 30
---

We use Gradescope's autograding system to build and run your submissions.

## Software configuration

{: .note }
If you are planning to use a different language, please let the course staff know on Piazza and we'll add support to the autograder.

The autograder is running Ubuntu 22.04, with the following software:
- JDK: openjdk 17.0.13
- Scala: 2.13.16
  - sbt: 1.10.7
- Python: 3.10.12
- Rust: 1.84.1
  - rustc: 1.84.1
  - cargo: 1.84.1
  - rustup: 1.27.1
- Typescript: 5.7.3
  - npm: 10.9.2
  - node: 22.13.1
- GNU C/Cpp Compiler: 11.4.0
  - CMake: 3.22.1
- GHC (Haskell): 8.8.4
  - cabal: 3.0.0.0
- Ocaml: 5.3.0
  - opam: 2.3.0

Please make sure your submission is compatible with these versions, and that all other dependencies are self-contained in the project. If you are planning to use a different language, or need a dependency not listed where which is not feasible to package with your project, please contact the course staff.

The grading server will have network access when running tests, so you can download and install packages while running `./build.sh`. However, please do this responsibly, and try to avoid using network access in `./run.sh`.

## Hardware configuration

The autograder will have at least one CPU core and 1.5 GB of memory.
