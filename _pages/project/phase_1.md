---
title: Phase 1
parent: Compiler Project
nav_order: 20
---

This phase consists of two segments: lexical analysis (scanning, aka lexing) and syntactic analysis (parsing).

{: .announcement }
> There are two things you need to submit for this phase:
> 1. Your scanner and parser code, due at **11:59 PM on Friday, February 21**.
> 2. A short report, due at **11:59 PM on Saturday, February 22**.

These due dates are also posted on the [Class Schedule]({% link _pages/project.md %}).
Also see [Recitation 1 slides](assets/documents/recitations/r01-project-overview-phase1.pdf) for some advice regarding this phase.

{% include toc.html %}

## Getting Started

1. Read the [project specification]({% link _pages/project.md %}).
2. Follow [these instructions]({% link _pages/class-tools/setup.md %}) to set up a programming environment.
3. Follow [these instructions]({% link _pages/class-tools/skeletons.md %}) to set up and understand the project skeletons.
4. Read the [Decaf spec]({% link _pages/project/decaf-spec.md %}). Phase 1 will only reference the first two sections, but we suggest reading the entire specification before starting.

You will need to refer to the [Decaf spec]({%link _pages/project/decaf-spec.md%}) when implementing the scanner and parser. Note that grammar in the Decaf spec does not specify what goes into the scanner and what goes into the parser; you will have to determine this split yourself.

You are welcome to use scanner/parser generators, or to write this code by hand.



## Scanner

Your scanner must be able to identify tokens of the Decaf language, the simple imperative language we will be compiling in 6.110. The language is described in the [Decaf spec]({%link _pages/project/decaf-spec.md%}). Your scanner should note illegal characters, missing quotation marks, and other lexical errors with reasonable error messages. The scanner should find as many lexical errors as possible, and should be able to continue scanning after errors are found. The scanner should also filter out comments and whitespace not in string and character literals.

When `-t scan` is specified, the output of your compiler should be a scanned listing of the program with one row for each token in the input. Each line will contain the following information: the line number (starting at 1) on which the token appears, the type of the token (if applicable), and the token's text. Emit int casts as `int`, `(`, `)`, and long casts as `long`, `(`, `)` on separate lines. Please print only the following strings as token types: `CHARLITERAL`, `INTLITERAL`, `LONGLITERAL`, `BOOLEANLITERAL`, `STRINGLITERAL` and `IDENTIFIER`.

For `STRINGLITERAL` and `CHARLITERAL`, the text of the token should be the text, as appears in the original program, including the quotes and any escaped characters.

Each error message should be printed on its own line, _before_ the erroneous token, if any. Such messages should include the file name, line and column number on which the erroneous token appears.

Here is an example table corresponding to `print("Hello, World!");`:

```
1 IDENTIFIER print
1 (
1 STRINGLITERAL "Hello, World!"
1 )
1 ;
```

The `public-tests` repository contains both a set of test files on which to test your scanner and the expected output for these files. The output of your scanner should match the provided output exactly on all valid files. For invalid files, the autograder will only check that your compiler returns a non-zero exit code. We have provided a sample of what you may want your errors to look like, but the output does not need to match the provided output exactly.



## Parser

Your parser must be able to correctly parse programs that conform to the grammar of the Decaf language. Any program that does not conform to the language grammar must be flagged with at least one error message. As with the scanner, all error messages must be printed to standard error.

Your parser does not have to, and should not, recognize context-sensitive errors, e.g. using an integer identifier where an array identifier is expected. Such errors will be detected by the static semantic checker. _Do not_ try to detect context-sensitive errors in your parser. However, you might need to create syntactic actions in your parser to check for some context-free errors.

You might want to look at Section 3 in the Tiger book or Sections 4.3 and 4.8 in the Dragon book for tips on getting rid of shift/reduce and reduce/reduce conflicts from your grammar.

When `-t parse` is specified, any syntactically incorrect program should be flagged with at least one error message, and the program should exit with a non-zero exit code. Multiple error messages may be printed for programs with multiple syntax errors that are amenable to error recovery. Given a syntactically valid program, your parser should produce no output, and exit with the value zero (success). The exact format for parse error messages is not stipulated.



## Evaluation

This phase is worth 5% of the overall grade in this class. Each student will be required to write and evaluate their own scanner and parser. There are several reasons for this shape of the phase:

- Writing (and debugging!) a front-end for a non-trivial language is a useful skill. Think about being able to quickly prototype a domain specific language in a few hours once you master the parsing techniques and tools.
- The following phases in this class will require a front-end for the Decaf language.  Typically, this front-end will be constructed by combining the parsers written by the individual students.

Your grade in this phase (5% total) is allocated as follows:

- The public and private tests of your scanner and parser: 4% (2% for scanner, 2% for parser)
- A *short* (~1-2 paragraphs) report on your chosen implementation approach: 1%

The public test cases are available at the [`6110-sp25/public-tests` repository](https://github.com/6110-sp25/public-tests).

However, copying code outside of your team is strictly forbidden. While the students are allowed to inspect and discuss other students' solutions, copying code will be considered cheating. We will be strict in enforcing this policy!


## Submission

### Code

Please submit your Phase 1 code on Gradescope via GitHub, following the steps below:

1. Push your code to your phase 1 GitHub repository (`6110-sp25/<YOUR KERB>_phase1`). We suggest making a separate branch for the submission, say, `phase1-submission`. _(An earlier version of this page and the recitation slides mistakenly used `-phase1` instead of `_phase1`. If you ran into an issue with this, please try again with `_phase1`.)_
2. Go to the Phase 1 assignment on Gradescope, and select your GitHub repository and branch.

We have set up an autograder for the Gradescope assignment, and you should be able to see the number of test cases you passed when the autograder finishes running. Note that the autograder is slow, and might take up to 40 minutes to run. Please see the [Autograder][autograder] page for more information about the installed software on the autograder.

**We suggest making an early submission once you finish setting up your build system just to check that the autograder can correctly build your compiler.**
You can resubmit your assignment as many times as you want before the due date, but please do not do this excessively.

Per course policy, you should make sure to submit your code on time. For extensions under extenuating circumstances (e.g., a member of your team is sick, family emergencies), we require a letter from one of the student deans at [Student Support Services (S<sup>3</sup>)][s3].

We reserve the right to review your code on GitHub and may, for example, give a lower grade (than dictated by the number of tests passed) for bad-faith code (e.g. writing code specific to particular tests in the test suites).

{: .warning }
Make sure the `./build.sh` and `./run.sh` scripts are located at the **root** of your repository, otherwise the autograder will fail.

### Report

Please submit your report in the Phase 1 Report assignment on Gradescope. The report should chosen implementation approach (parser and scanner)


## Appendix: Why we defer integer range checking until the next phase

When considering the problem of checking the legality of the input program, there is no fundamental separation between the responsibilities of the scanner, the parser and the semantic checker. Often, the compiler designer has the choice of checking a certain constraint in a particular phase, or even of dividing the checking across multiple phases. However, for pragmatic reasons, we have had to divide the complete scan/parse/check unit into two parts simply to fit the course schedule better.

As a result, we will have to mandate certain details about your implementations that are not necessarily issues of correctness. For example, one cannot completely check whether integer literals are within range without constructing a parse tree.

Consider the input:

```
x+-9223372036854775808
```

This corresponds to a parse tree of:

```
    +
   / \
  x   -
      |
  INT(9223372036854775808)
```

We cannot confirm in the scanner that the integer literal -9223372036854775808 is within range, since it is not a single token. Nor can we do this within the parser, since at this stage we are not constructing an abstract syntax tree. Only in the semantic checking phase, when we have an AST, are we able to perform this check, since it requires the unary minus operator to modify its argument if it is an integer literal, as follows:

```
    +                         +
   / \                       / \
  x   -           ---->     x   INT(-9223372036854775808)
      |
  INT(9223372036854775808)
```

Of course, if the integer token was clearly out of range (e.g. 999999999999999999999) the scanner could have rejected it, but this check is not required since the semantic phase will need to perform it later anyway.

Therefore, rather than do some checking earlier and some later, we have decided that ALL integer range checking must be deferred until the semantic phase. So, your scanner/parser must not try to interpret the strings of decimal or hex digits in an integer token; the token must simply retain the string until the semantic phase.

When printing out the token table from your scanner, do not print the value of an `INTLITERAL` token in decimal. Print it exactly as it appears in the source program, whether decimal or hex.

[s3]: https://studentlife.mit.edu/s3
[autograder]: {% link _pages/class-tools/autograder.md %}
