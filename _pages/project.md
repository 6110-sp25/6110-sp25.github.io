---
title: Compiler Project
has_children: true
nav_order: 30
---

{% include toc.html %}

## Introduction

This is an overview of the course project and how we'll grade it. You should not expect to understand all the technical terms, since we haven't yet covered them in class. We are only providing the overview here to give you some idea of the kind of project we're assigning. Additional instructions for each phase will be released to provide more technical details of the project.

By the end of the course, you will have built a compiler that takes Decaf programs, a subset of C, as input and generates executable x86 code as output.

## Organization

The first phase (lexing and parsing) will be done individually. For subsequent phases, you will be working in groups of three or four students. You will be allowed to choose your own partners as much as possible. Each group collaborate to write one compiler for the Decaf language. We expect all groups to complete all phases successfully. The start of the class is very fast-paced: do not fall behind!

## Project Segments

### Phase 1: Lexing and Parsing

A *scanner* takes a Decaf source file as an input and ouputs a sequence of *tokens* in a process known as *lexing*. Tokens area sequences of characters which form the basic unit of Decaf programs.

A token can be:

- An operator (``*`` or ``[``)
- A keyword (``if``, or ``while``)
- An identifier (`foo`)
- A literal (`42`, `'a'`, `"hello"`)

Non-tokens (such as whitespace or comments) are discarded. Invalid tokens (unterminated string literals) must be reported.

A *parser* takes a sequence of tokens as input and checks to make sure they conform to the language specification. In order to pass this check, the input must have all matching braces, semicolons, etc. Types, variable names, and function names are not verified. A parser outputs a *syntax tree*, a tree representation of the program.

The [*Decaf Specification*]({% link _pages/project/decaf-spec.md %}) is the grammar of the language, which you will need to separate into a scanner specification and a parser specification. You will then implement the scanner and parser, either from scratch in the programming language of your choice, or by using a scanner generator (such as `lex`, `flex`, `jflex`, `antlr`) and parser generator (such as `yacc`, `bison`, `cup`, `antlr`) of your choice.

There will be a *short* report that will help us understand the approach (i.e. hand-written, parser generator, programming language choice) you decided to take.

### Phase 2: Intermediate Representation (IR) and Semantic Checking

A *semantic checker* takes a syntax tree as input and checks to make sure that the program is well-formed, performing checks that cannot be done by the scanner and parser.

For example, this checks that variables are declared before they are used, that variables have the right types, and that functions are called with the right number and types of arguments.

The semantic checker will also build a symbol table in which the type and location of each identifier is kept. We'll supply a complete list of the checks in the [*Decaf Specification*]({% link _pages/project/decaf-spec.md %}).

Experience from past years suggests that many groups underestimate the time required to complete the static semantic checker, so you should pay special attention to this deadline.

It is important that you build the symbol table, since you won't be able to build the code generator without it. However, the completeness of the checking will not have a major impact on subsequent stages of the project.

At the end of this phase the front-end of your compiler is complete and you have designed the intermediate representation (IR) that will be used by the rest of the compiler.

You will also turn in a short report that details how your team plans on splitting the work associated with the project, any questions, and the status of your implementation.

### Phase 3: Code Generation

In this phase you will create a working compiler by generating unoptimized x86-64 assembly code from the IR you generated in the previous phase. Because you have relatively little time for this phase you should concentrate on correctness and leave any optimization hacks out, no matter how simple.

The steps of code generation are as follows: first, the rich semantics of Decaf are broken-down into a simple low-level intermediate representation. For example, constructs such as loops and conditionals are expanded to code segments with simple comparison and branch instructions. Next, the intermediate representation is matched with the Application Binary Interface (ABI), i.e., the calling convention and register/stack usage.

Then, the corresponding x86-64 machine code is generated. Finally, the code, data structures, and storage are laid-out in the assembly format. We will provide a description of the object language. The object code created using this interface will then be run on a testing machine.

You will turn in a Project Design Document that explains the technical details of phase 1, 2, and 3. This will be reviewed by the TAs and feedback will be provided. This document will also count towards the project grade.

### Phase 4: Dataflow Optimization

This phase phase consists of building a data-flow framework to help optimize the code generated by your compiler.
For this phase, you are required to implement the data-flow framework and data-flow optimization passes to test the framework. This framework will be used in the final phase to build data-flow optimization passes.

We will provide a description of the framework and the required optimizations to be implemented in a later handout.

You will turn in a Project Design Document that explains the technical details of phase 4.

### Phase 5: Other Optimizations

The final phase is a substantial open-ended phase. In this phase your team's task is to generate optimized code for programs so that they will be correctly executed in the shortest possible time.

There are a multitude of different optimizations you can implement to improve the generated code. You can perform data-flow optimizations such as constant propagation, common sub-expression elimination, copy propagation, loop invariant code motion, unreachable code elimination, dead code elimination and many others using the framework created in the previous segment.

You can also implement instruction scheduling, register allocation, peephole optimizations and even parallelization across multiple cores of the target architecture.

In order to identify and prioritize optimizations, you will be provided with a benchmark suite of a few simple applications.
Your task is to analyze these programs, perhaps hand optimizing these programs, to identify which optimizations will have the highest performance impact. Your write-up needs to clearly describe the process you went through to identify the optimizations you implemented and justify them.

You will turn in a Project Design Document that details the technical details of phase 5.

### Compiler Derby

The last class will be the *Compiler Derby* at which your group will compete against other groups to identify the compiler that produces the fastest code.
<!-- The application used for the Derby will be provided to the groups one day before the Derby. This is done in order for your group to debug the compiler and get it working on this program. However, you are forbidden from adding any application-specific hacks to make this specific program run faster. -->

## Grading Details

For each phase, you are required to submit your *project design documents* and *complete sources* (including all files needed to build your project). Your projects will be submitted via GitHub. Do not include compiled files. Instead, you repo should contain an executable file called `build.sh` in the top-level directory which will compile your code. These files are provided for you in the skeleton code; you may modify them if you need to.

Phases 2 through 5 will be done in groups. Each group will be given access to a repository for their project on Github.

There are few restrictions on how the project should be structured, except that it should be self-contained (apart from the allowed libraries and programming environment), and contain executables `build.sh` and `run.sh` in the top-level directory.

We will release public tests associated with each phase, which will be released with the phase instructions. There are also private tests, which will be released **after** the due date of each phase. We will grade your submission based on both the public and private tests after each phase.

Your project design document will make up a portion of your grade for each phase. Please make sure not to neglect turning it in.

---

## Command Line Reference

Your compiler should have the following command line interface.

```
./run.sh [options] filename
```

| **Option**                      | **Description**                                              |
| :------------------------------ | ------------------------------------------------------------ |
| `-t | --target <stage>`         | `stage` is one of scan, parse, inter, or assembly. <br />Compilation should proceed through the given stage. |
| `-o | --output <outname>`       | Write output to `<outname>`                                  |
| `-O | --opt [optimization,...]` | Perform the (comma-separated) listed optimizations.<br /> `all` stands for all supported optimizations. `-<optimization>` removes optimizations from the list |
| `-d | --debug`                  | Print debugging information. <br />If this option is not given, there should be no output to the screen on successful compilation |

The command line arguments you must implement are listed in table above. Exactly one filename should be provided, which will not begin with a dash.The filename must not be listed after the ``-O/--opt`` flag, since it will be assumed to be an optimization.

The default behavior is to compile as far as the current phase of the project and print the output to standard output unless different output is specified with `-o/--output`. All error messages should be printed to standard error.

By default, no optimizations are performed. The list of optimization names will be provided in the optimization phases.

For each provided language skeleton, we have provided code which is sufficient to implement this interface. It also returns a list of arguments it did not understand which can be used to add features. The TAs will not use any extra features you add for grading. However, you can tell us which, if any, to use for the compiler derby. You may wish to provide a flag which turns on only the optimizations you like.

---

## Project Design Document

From Phase 3 onwards, you should include a Project Design Document in the `doc` folder of your GitHub repository as a PDF file. Documentation should be clear, concise and readable.

Your documentation must include the following parts, which could be described as Design, Extras, Difficulties, and Contribution.
Not every question or point of each part need to be addressed, just enough information to describe each portion effectively:

### Design
An overview of your design, an analysis of design alternatives you considered, and key design decisions. Be sure to document and justify all design decisions you make. Any decision accompanied by a convincing argument will be accepted.

If you realize there are flaws or deficiencies in your design late in the implementation process, discuss those flaws and how you would have done things differently. Also include any changes you made to previous parts and why they were necessary.

This section should aid the TA in being able to read and give feedback on the code written.

### Extras
A list of any clarifications, assumptions, or additions to the problem assigned. This include any interesting debugging techniques/logging, additional build scripts, or approved libraries used in designing the compiler. The project specifications are fairly broad and leave many of the decisions to you. This is an aspect of real software engineering. If you think major clarifications are necessary, consult the TA.

### Difficulties
A list of known problems with your project, and as much as you know about the cause.

- If your project fails a provided test case, but you are unable to fix the problem, describe your understanding of the problem
- If you discover problems in your project in your own testing that you are unable to fix, but are not exposed by the provided test cases, describe the problem as specifically as possible and as much as you can about its cause
- If you were failing any test cases for a previous phase and managed to fix them by the current phase, give a short description of what the problem was and how you fixed it
- Describe any section of your project that you would like to highlight for more feedback on/had questions on

### Contribution
A brief description of how your group divided the work. This will not affect your grade; it will be used to alert the TAs to any possible problems.

