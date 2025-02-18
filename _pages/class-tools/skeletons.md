---
title: Project Skeletons
parent: Class Tools
nav_order: 20
---

Choose a language and its corresponding skeleton repo. Though the choice of language is up to you and your group, we encourage you to default to Java:

- [Java](https://github.com/6110-sp25/decaf-skeleton-java)
- [Scala](https://github.com/6110-sp25/decaf-skeleton-scala)
- [Rust](https://github.com/6110-sp25/decaf-skeleton-rust)
- [Typescript](https://github.com/6110-sp25/decaf-skeleton-typescript)
- Get in touch with the TAs if you want to use any other language

Create and navigate to a directory where you'd like your project files to be stored (`~/Documents/6.110-compiler-phase1` or `~/Desktop/6.110-compiler-phase1` is a good choice) and run the following commands.

```bash
git init

git remote add skeleton git@github.com:6110-sp25/decaf-skeleton-<LANGUAGE>.git
git pull skeleton main

git remote add origin git@github.com:6110-sp25/<YOUR KERB>_phase1.git
git push -u origin main
```

This should initialize your phase 1 project directory with the skeleton code for your chosen language. If you get `Permission denied (publickey)`, make sure you have set up an SSH key with GitHub.

Make sure that your environment is set up correctly by running `./build.sh` and `./run.sh`.

While you are encouraged to use this infrastructure, you may also choose to modify it however you like, or even ignore it and design your own infrastructure from scratch. If you choose to do so, you will still need to replicate all command line options and the functionality required for the scripts that build and execute the compiler, as detailed in the project overview.

# Provided Infrastructure

## Java

The Java skeleton relies on Gradle for build automation. Gradle is configured by the `build.gradle.kts` file using Kotlin DSL. The file is not particularly long, so you are encouraged to read and understand the provided `build.gradle.kts`.

The build server uses Java 17, so you may wish to install this version of Java in your local development environment.

The Java skeleton has the following structure:

```
.
├── src/
│   ├── main/java/decaf/
│   │   ├── parallel/
│   │   │   └── Analyze.java
│   │   ├── utils/
│   │   │   └── CommandLineInterface.java
│   │   └── DecafCompiler.java
│   └── test/java/decaf/
│       └── ExampleTest.java
├── build.sh
├── run.sh
├── build.gradle.kts
└── settings.gradle.kts
```

The program entry point is located in `DecafCompiler.java`. `CommandLineInterface.java` implements the command-line interface described in the Compiler Project overview; you can modify it to add new command-line flags as needed. `Analyze.java` will only be used in phase 5.

We have declared JUnit as a test dependency for unit testing, and an example test is provided in `ExampleTest.java`

## Scala

The Scala skeleton relies on Scala Build Tool (SBT) 1.9.8 for build automation. SBT is configured by the `build.sbt` file. You are encouraged to read over this file to understand what it is doing.

As Scala is a JVM language, it has interoperability with Java. Therefore, we have provided a folder structure that allows you to use both Java and Scala source code. The Scala skeleton has the following structure:

```
.
├── src/
│   ├── main/java/decaf/
│   │   ├── parallel/
│   │   │   └── Analyze.java
│   │   └── utils/
│   │       └── CommandLineInterface.java
│   ├── main/scala/decaf/
│   │   └── DecafCompiler.scala
│   └── test/scala/decaf/
│       ├── ExampleTestFlatSpec.scala
│       └── ExampleTestFunSuite.scala
├── build.sh
├── run.sh
└── build.sbt
```

The program entry point is located in `DecafCompiler.scala`. `CommandLineInterface.java` implements the command-line interface described in the Compiler Project Overview; you can modify it to add new command-line flags as needed. `Analyze.java` will only be used in phase 5.

We have declared ScalaTest as a test dependency for unit testing. Two examples of unit testing are provided in `test/scala/decaf`

## Rust

The program entry point is located in `src/main.rs`. `src/utils/cli.rs` implements the command-line interface described in the Compiler Project Overview; you can modify it to add new command-line flags as needed.
```
.
├── src/
│   ├── utils/
│   │   └── cli.rs
│   └── main.rs
├── build.sh
├── run.sh
├── test.sh
└── Cargo.toml
```

The Rust skeleton uses Cargo for builds. An example of a test is provided in `test.sh`

## Typescript

The program entry point is located in `src/main.ts`. `src/utils/cli.ts` implements the command-line interface described in the Compiler Project Overview; you can modify it to add new command-line flags as needed.

```
.
├── src/
│   ├── utils/
│   │   └── cli.ts
│   └── main.ts
├── test/
│   └── exampleTest.ts
├── build.sh
├── run.sh
└── package.json
```

The Typescript skeleton uses Node for builds, and Mocha for unit tests. An example test file is given in `test/exampleTest.ts`.

# Running your compiler

All skeleton projects come with a `build.sh` and `run.sh` in the project root. Use these to build and run your compiler respectively. They will also be used for grading, so if you make any changes to the build or run process, make sure to modify these files to reflect them. In general, these two files are the only restriction we impose on the structure of your projects. If you decide to change the repository structure, or even use a language for which we have no template, you just need to modify `build.sh` and `run.sh` to correctly build and run your system.

Arguments passed to `run.sh` are passed straight to your compiler. Read [Command Line Reference](/project#command-line-reference) for more information about the command-line arguments that your compiler should accept.
