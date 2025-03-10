---
title: Phase 2
parent: Compiler Project
nav_order: 30
---

In this phase, you will extend your compiler to find, report, and recover from semantic errors in Decaf programs.

{: .note }
> Starting with this phase, you will work in teams of 1-4 people.
> Please submit the [Team Preference Form](https://www.gradescope.com/courses/931853/assignments/5788932/) on Gradescope by **11:59PM on Wednesday, February 19**.
>
> See the [team formation](#team-formation) section below for more information.

{: .announcement }
> There are two deliverables for this phase. The semantic checker is due at **11:59 PM on Friday, March 7**. The short report is due at **11:59 PM on Saturday, March 8**.
> 1. Your semantic checker
> 2. A short report, including an overview of your IR design and your team dynamics
>
> Only one submission is needed for each team --- please use Gradescope's feature to add multiple people to a single submission.

These due dates are also posted on the [Class Schedule]({% link _pages/schedule.md %}).

{% include toc.html %}

## Overview

This part of the project includes the following tasks:

1. Create a high-level intermediate representation (IR) tree. If you used a parser generator, you can do this either by adding actions to your grammar to build a tree, or by generating a generic tree with the parser generator and walking that to construct a new tree. If you implemented your recursive-descent parser from scratch, you will have to convert your parse tree/AST to the IR tree. The problem of designing an IR will be discussed in the lectures; some hints are given in the final section of this handout.

	When running in debug mode, your compiler should pretty-print the constructed IR tree in some easily-readable form suitable for debugging. This will not be graded, but is highly recommended by the course staff.
2. Build symbol tables for the methods.  (A symbol table is an environment, i.e. a mapping from identifiers to semantic objects such as variable declarations.  Environments are structured hierarchically, in parallel with source-level constructs, such as method-bodies, loop-bodies, etc.)
3. Perform all semantic checks by traversing the IR and accessing the symbol tables. __Note:__ the run-time checks are not relevant for this phase.

  Most semantic errors can be checked by testing the rules enumerated in the "Semantic Rules" section of the [Decaf spec]({%link _pages/project/decaf-spec.md%}). However, you should read the spec in its entirety to make sure that your compiler catches all semantic errors implied by the language definition. We have attempted to provide a precise statement of the semantic rules. Please ask the course staff if you have any questions about the semantic rules.

Please do not hesitate to reach out to the TAs if you're not sure what to do, find yourself struggling to make progress, or encounter problems relating to team dynamics.

## Team formation

Starting in this phase, you will work in teams of up to 4 students.
We will use the [Team Preference Form](https://www.gradescope.com/courses/931853/assignments/5788932/) on Gradescope by  on Gradescope to assign you to teams. Please submit this form by **11:59PM on Wednesday, February 19**.

On this form you will be able to indicate your programming language of choice, your preferred teammates, and whether you'd like us to match you up with other people. Matching will be done based on your preferred language.

Group submission is enabled on this form. **If you already know who you want to team up with, please submit this form as a group by adding your preferred teammates to your submission.**

{: .note }
We heavily recommend you to work in groups of at least 3 students, as the amount of work in the project will still be the same even if you have fewer people.

## Getting started

Once teams have been formed, each team will be given their own private GitHub repository, which will be used for phases 2-5. Teams should take care to avoid accidentally (or intentionally) sharing their code with other teams. Such actions will constitute cheating.

Use the procedure described in [Class Tools][class-tools] to initialize a new local Git repository, but this time, set the `origin` remote to your team repository instead of your personal phase 1 repository. You can also start with one of your team member's phase 1 code, or a combination of your team members' phase 1 code.

## Specifications

{: .note}
An earlier version of this page mistakenly stated that the command being run is `./run.sh --target=inter <filename>`. The autograder actually runs `./run.sh -t inter <filename>`, with a space between `-t` and `inter`.

You should be able to run your compiler from the command line with:

```
./run.sh -t inter <filename>
```

The resulting output to the terminal should be a report of all errors (printed to stderr) encountered while compiling the file. Your compiler should give reasonable and specific error messages (with line numbers, column numbers and identifier names) for all errors detected. It should avoid reporting multiple error messages for the same error. For example, if `y` has not been declared in the assignment statement `x=y+1;`, the compiler should report only one error message for `y`, rather than one error message for `y`, another error message for the `+`, and yet another error message for the assignment.

After you implement the static semantic checker, your compiler should be able to detect and report _all_ static (i.e., compile-time) errors in any input Decaf program, including lexical and syntax errors detected by previous phases of the compiler. In addition, your compiler should not report any messages for valid Decaf programs.

As suggested, when run in debug mode, your compiler should print the constructed IR and symbol tables in some form. You can run in debug mode using the command:

```
./run.sh -t inter --debug <filename>
```

The public test cases for this phase have been added to the [`6110-sp25/public-tests` repository](https://github.com/6110-sp25/public-tests) on Github. Please read the comments in the test cases to see what we expect your compiler to do.

## Report

Your report for this phase should follow the structure below.

### 1. Implementation

Provide a high-level description of your approach in this phase, including:

- An overview of your intermediate representations (IRs) beyond the parse tree (e.g., abstract syntax trees, symbol tables, tree IRs, graph IRs, etc.).
- Any nontrivial data structures you used and why they were necessary.
- Your strategy for handling and reporting multiple errors for each Decaf program.

### 2. Testing and Debugging

Describe how you evaluated the correctness of your implementation:

- Did you write any additional test cases? How did you ensure adequate coverage?
- What methods or tools (e.g., assertions, visualizations, fuzzers, sanitizers) helped you debug?

### 3. Reflection and Project Status

Provide an honest evaluation of your progress and teamwork:

- Is your implementation complete? Are there failing tests or known issues?
- If you were to redo this phase, what would you do differently?
- How did your team divide the work? Did you encounter any collaboration challenges, and how do you plan to resolve them?
- Do you have anything specific you would like TAs to review or help with?

## Evaluation

This phase is worth 5% of the overall grade in this class.
Your grade in this phase (5% total) is allocated as follows:

- Your semantic checker: 3.5% (2.5% from autograded public and private test cases, 1% from error messages, checked manually)
- A short report, including an overview of your IR design and your team dynamics: 1.5%

The public test cases are available at the [`6110-sp25/public-tests` repository](https://github.com/6110-sp25/public-tests).
You should also make sure that your phase 2 compiler also passes all phase 1 test cases.

## Submission

### Code

Please submit your phase 2 code on Gradescope via GitHub, following the steps below:

1. Push your code to your team GitHub repository (`6110-sp25/<TEAM NAME>`). We suggest making a separate branch for the submission, say, `phase2-submission`.
2. Go to the [Phase 2 assignment](https://www.gradescope.com/courses/931853/assignments/5819180/) on Gradescope, and select your GitHub repository and branch.
3. Add your team members to the submission on Gradescope.

{: .warning }
Make sure the `./build.sh` and `./run.sh` scripts are located at the **root** of your repository, otherwise the autograder will fail.

{: .note }
Please make sure that your compiler doesn't get caught in an infinite loop when running any of the tests.

### Report

Please submit your report in the [Phase 2 Report assignment](https://www.gradescope.com/courses/931853/assignments/5819193/) on Gradescope, and remember to add your team members to the submission.

## Implementation Suggestions

- You'll probably need to declare classes for each of the nodes in your IR. In many places, the hierarchy of IR node classes will resemble the language grammar. For example, a part of your inheritance tree might look like this (where indentation represents inheritance):

	```
	abstract class Ir
	abstract class     IrExpression
	abstract class         IrLiteral
	         class             IrIntLiteral
	         class             IrBooleanLiteral
	         class         IrCallExpr
	         class             IrMethodCallExpr
	         class         IrBinopExpr
	abstract class     IrStatement
	         class         IrAssignStmt
	         class         IrPlusAssignStmt
	         class         IrBreakStmt
	         class         IrContinueStmt
	         class         IrIfStmt
	         .
	         .
	         .
	abstract class     IrMemberDecl
	         class         IrMethodDecl
	         class         IrFieldDecl
	         .
	         .
	         .
	         class     IrVarDecl
	         class     IrType
	```

	Classes such as these implement the _abstract syntax tree_ of the input program. In its simplest form, each class is simply a tuple of its subtrees, for example:

	```
	public class IrBinopExpr extends IrExpression
	{
	    private final int          operator;                      |
	    private final IrExpression lhs;                           +
	    private final IrExpression rhs;                          / \
	}                                                         lhs   rhs
	```

	or:

	```
	public class IrAssignStmt extends IrStatement
	{                                                             :=
	    private final IrLocation   lhs;                          /  \
	    private final IrExpression rhs;                       lhs    rhs
	}
	```

	In addition, you probably want to define classes for the semantic entities of the program, which represent abstract properties (e.g. expression types, method signatures, etc.) and to establish the correspondences between them.  Some examples: every expression has a type; every variable declaration introduces a variable; every block defines a scope.  Many of these properties are derived by recursive traversals over the tree.

- **Don't be afraid to refactor "working" code.** As you progress through the project, you will gain a better understanding of what you need to do and how you want your data structures to look like. Sometimes it makes sense to stick with early design decisions; sometimes refactoring them would save you time in the long run.

	If you're worried about losing progress, save your working state with Git. If you don't know how to do this or how to recover a previous state once you've saved it, reach out on Piazza. Being able to use Git is one of the most important skills you can have as a software developer.

- All error messages should be accompanied by the filename, line number and column number of the token most relevant to the error message (use your judgement here). This means that, when building your abstract-syntax tree (or AST), you must ensure that each IR node contains sufficient information for you to determine its line number at some later time.

	It is _not_ appropriate to throw an exception when encountering an error in the input: doing so would lead to a design in which at most one error message is reported for each run of the compiler.
    A good front-end saves the user time by reporting multiple errors before stopping, allowing the programmer to make several corrections before having to restart compilation.

- Semantic checking should be done __top-down__. While the type-checking component of semantic checking can be done in bottom-up fashion, other kinds of checks (for example, detecting uses of undefined variables) can not.

	There are two ways of achieving this. The first is to perform the checks at the same time as parsing, e.g. using parser actions in the middle of productions. This approach may require less code but can be more complex, because more work needs to be done directly within the parser/ parser generator.

	A cleaner approach is to invoke your semantic checker on a complete AST after parsing has finished.  The pseudocode for `block` in this approach would resemble this:

	```
	   void checkBlock(EnvStack envs, Block b) {
	       envs.push(new Env());
	       for each statement in b.statements
	           checkStatement(envs, statement);
	       envs.pop();
	   }
	```

	In this pseudocode, a new environment is created and pushed on the environment stack, the body of the block is checked in the context of this environment stack, and then the new environment is discarded when the end of the block is reached.

	The semantic check, just like code generation and certain optimizations, can often be expressed as a _visitor_ (which should be familiar from _Design Patterns_ or 6.031) over the AST. However, the visitor pattern may be most useful in imperative programming, and is just one design. For languages that support first-class functions, there are certainly many other interesting designs. Whatever your language and design, we strongly recommend you separate your logic for performing generic AST operations (traversing, searching, etc) from the logic of your compiler functions (e.g. semantic checks). Think carefully about what designs will work well for performing many analyses and operations on an AST!

- One last note: the treatment of negative integer literals requires some care. Recall from the previous handout that negative integer literals are in fact two separate tokens: the positive integer literal and a preceding `-`.  Whenever your top-down semantic checker finds a unary minus operator, it should check if its operand is a positive integer literal, and if so, replace the subtree (both nodes) by a single, negative integer literal.

[class-tools]: {% link _pages/class-tools.md %}
