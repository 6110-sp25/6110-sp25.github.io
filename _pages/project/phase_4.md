---
title: Phase 4
parent: Compiler Project
nav_order: 45
---

In this phase, you will add dataflow optimizations to your compiler.

{: .announcement }
> There are two deliverables for this phase.
> 1. A [Project Design Document](#design-document) explaining your implementation of dataflow optimizations. This part is due **11:59 PM on Saturday, April 19**.
> 2. Your Decaf compiler, with at least one dataflow optimization implemented. This part is due **11:59 PM on Friday, April 18**.
>
> See the [Evaluation section](#evaluation) for details on grading.
>
> Only one submission is needed for each team --- please use Gradescope's feature to add multiple people to a single submission.

This due date is also posted on the [Class Schedule]({% link _pages/project.md %}).

{% include toc.html %}

## Overview

For this phase, you will add dataflow optimizations to your compiler. **At the very least, you must implement one of the three global dataflow optimizations**:

1. __Global CSE (Common Subexpression Elimination)__: Identification and elimination of redundant expressions using the algorithm described in lecture (based on available-expression analysis). See §8.3 and §13.1 of the Whale book, §10.6 and §10.7 in the Dragon book, and §17.2 in the Tiger book.
2. __Global Copy Propagation__: Given a `copy` assignment like `x = y`, replace uses of `x` by `y` when legal (the use must be reached by only this def, and there must be no modification of `y` on any path from the def to the use).  See §12.5 of the Whale book and §10.7 of the Dragon book.
3. __Dead Code Elimination__: Dead code elimination is the detection and elimination of code which computes values that are not subsequently used. This is especially useful as a post-pass to constant propagation, CSE, and copy propagation. See the §18.10 of the Whale book and §10.2 of the Dragon book.

Your dataflow optimizations should work on **all local (non-array) variables**, and you can optionally extend the scope of these optimizations to cover global variables and array variables.

All other optimizations listed below are optional. We list them here as suggestions since past winners of the compiler derby typically implement each of these optimizations in some form. You are free to implement any other optimizations you wish. Note that you will be implementing register allocation and other assembly level optimizations for the next phase, so you don't need to concern yourself with it now.

The following are other dataflow optimizations that are useful to implement, but not required for this phase.

1. __Global Constant Propagation and Folding__: Compile-time interpretation of expressions whose operands are compile time constants. See the algorithm described in §12.1 of the Whale book.
2. __Loop-invariant Code Motion (code hoisting)__: Moving invariant code from within a loop to a block prior to that loop. See §13.2 of the Whale book and §10.7 of the Dragon book.
3. __Unreachable Code Elimination__: Unreachable code elimination is the conversion of constant conditional branches to equivalent unconditional branches, followed by elimination of unreachable code. See §18.1 of the Whale book and §9.9 of the Dragon book.

The following are not generally considered dataflow optimizations. However, they can increase the effectiveness for the transformations listed above.

1. __Algebraic Simplification and Reassociation__: Basic algebraic simplifications described in class. This includes simplifying the following rules:
    ```hs
    a + 0      -> a
    a - 0      -> a
    a * 1      -> a
    b == true  -> b
    b != false -> b
    ...
    ```
    You may find it useful to canonicalize expression orders, especially if you choose to implement CSE. See 12.3 of the Whale book and 10.3 of the Dragon book.
1. __Strength reductions__: Algebraic manipulation of expressions to use less expensive operations. This includes the following transformations that convert multiplying constants into bit shifts i.e.: `a * 4 -> a << 2` and include turning multiplication operations from within a loop into sum operations.
1. __Inline Expansion of Calls__: Replacement of a call to procedure `P` by inline code derived from the body of `P`. This can also include the conversion of tail-recursive calls to iterative loops. See 15.1 and 15.2 of the Whale book.

You will want to think carefully about the order in which optimizations are performed. You may want to perform some optimizations more than once.

All optimizations (except inline expansion of calls) should be done at the level of a single method. Be careful that your optimizations do not introduce bugs in the generated code or break any previously-implemented phase of your compiler. Needless to say, it would be foolish not to do regression testing using your existing test cases. __Do not underestimate the difficulty of debugging this phase__.

As in phase 3, your generated code must include instructions to perform the runtime checks listed in the language specification. Note that the optimized program must report a runtime error for exactly those program inputs for which the corresponding unoptimized program would report a runtime error (and the runtime error message must be the same in both cases). However, we allow the optimized program to report a runtime error earlier than it might have been reported in the unoptimized program.

## Specifications

Your compiler's command line interface must provide an interface similar to the following for turning on each optimization individually. Something similar should be provided for every optimization you implement. Document the names given to each optimization not specified here.

- `-O cse` turns on common subexpression elimination only
- `-O cp` turns on copy propagation optimization only
- `-O cp,cse` turns on copy propagation optimization and common subexpression elimination only
- `-O all` turns on all optimizations
- `-O all,-cse` turns on all optimizations except common subexpression elimination

This is the interface provided by the skeleton repositories. For the full command-line specification, see the project overview handout.

You should be able to run your compiler from the command line with:

```
./run.sh -t assembly <INPUT FILE> -o <OUTPUT FILE>          # no optimizations
./run.sh -t assembly -O all <INPUT FILE> -o <OUTPUT FILE>   # all optimizations
```

Your compiler should then write a x86-64 assembly listing to standard output, or to the file specified by the `-o` argument.

## Design Document

In this phase you will also write a design document that explains the technical details of your phase 4 implementation.

The TAs will review your design document and provide feedback. Please make sure all code has been committed to your team’s repository. It must include the following sections:

1. **Design (70 points):** An overview of your design, an analysis of design alternatives you considered, and key design decisions. This section should help us understand your code, convince us that it is sufficiently general, and let us know anything that might not be reflected in your code. In particular, you should discuss the design of the following components:
   1. **(30 points)** Your general framework for dataflow optimizations
   2. **(40 points)** Details of each dataflow optimization you implemented, including:
     - the scope of the optimization you implemented (did you take into account global variables and/or array variables?)
     - the dataflow equations you used
     - a sample test case, with generated code before and after, included under `doc/phase4-code/` in your repository
     - a brief explanation of how your dataflow optimization worked
2. **Extras (4 points):** A list of any clarifications, assumptions, or additions to the problem assigned. This includes any interesting debugging techniques/logging, additional build scripts, or approved libraries used in designing the compiler.
3. **Difficulties (3 points):** A list of known problems with your project, and as much as you know about the cause. If there are any phase 3 tests that you were not passing before the phase 3 deadline, but fixed for phase 4, you should also include this information in the write up.
4. **Contribution (3 points):** A brief description of how your group divided the work. This will not affect your grade; it will be used to alert the TAs to any possible problems.

## Evaluation

This phase is worth 10% of the overall grade in this class. Your grade in this phase (10% total) is allocated as follows:
- **3%:** Description of your general framework for dataflow optimizations (§1a [in your design document](#design-document))
- **4%:** **At least one** dataflow optimization implemented and discussed in detail (§1b)
- **1%:** The Extras, Difficulties, and Contribution sections of your design document (§2--4)
- **2%:** Autograded test cases, including phase 3 test cases
  - your compiler should output correct code both without optimizations and with optimizations enabled (`-O all`).
  - Note that the autograder only checks for correctness of the output code and not whether optimizations were made.
  - All test cases are public, and are available at the [`6110-sp25/public-tests` repository](https://github.com/6110-sp25/public-tests).

## Submission

### Design Document

Please submit your design document as a PDF in the [Phase 4 Report assignment](https://www.gradescope.com/courses/931853/assignments/5976225) on Gradescope, and remember to add your team members to the submission.

{: .note }
Please also include both the design document _and_ your sample test case under the `doc` subdirectory in your team GitHub repository.

### Code

Please submit your phase 4 code on Gradescope via GitHub, following the steps below:

1. Push your code to your team GitHub repository (`6110-sp25/<TEAM NAME>`). We suggest making a separate branch for the submission, say, `phase4-submission`.
2. Go to the [Phase 4 assignment](https://www.gradescope.com/courses/931853/assignments/5965407/) on Gradescope, and select your GitHub repository and branch.
3. Add your team members to the submission on Gradescope.

Submitted repositories should have the following structure:
```txt
<repo name>
├── build.sh
├── run.sh
├── doc/
│   ├── phase4.pdf    # phase 4 design document
│   └── phase4-code/  # phase 4 sample test case
└── ...
```

{: .warning }
Make sure the `./build.sh` and `./run.sh` scripts are located at the **root** of your repository, otherwise the autograder will fail.

[design-doc]: {% link _pages/project.md %}#project-design-document
