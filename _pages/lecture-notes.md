---
title: Lecture Slides
nav_order: 60
---

## Lectures

Supplemental readings are listed under each lecture. You may find them useful. You are not responsible for the contents not covered in lecture.

1. [Introduction][l01] ([6 slides/page][l01-6])

   - *Cooper et al*., Ch. 1 Overview of Compilation, p.p. 1-23 (2nd ed.), p.p. 1-26 (3rd ed.)

2. [Specifying Languages with Regular Expressions and Context-Free Grammars][l02] ([6 slides/page][l02-6])

   - *Cooper et al*., Ch. 2 Scanners, p.p. 25-82 (2nd ed.), p.p. 27-84 (3rd ed.).
   - *Cooper et al*., Ch. 3 Parsers, § 3.1-3.2, p.p. 83-95 (2nd ed.), p.p. 85-98 (3rd ed.).

3. [Top-down Parsing][l03] ([6 slides/page][l03-6])

   - *Cooper et al*., Ch. 3 Parsers, § 3.3 Top-Down-Parsing, p.p. 96-116 (2nd ed.), p.p. 99-118 (3rd ed.).

4. [Intermediate Formats][l04] ([6 slides/page][l04-6])
    - *Cooper et al.*, Ch. 5 Intermediate Representations, p.p. 221-268 (2nd ed.), Ch. 4, p.p. 159-207 (3rd ed.).
    - *Cooper et al.*, Ch. 6 The Procedure Abstraction, § 6.3 Name Spaces, p.p. 276-297 (2nd ed.).
    - *Cooper et al.*, Ch. 5 Syntax Driven Translation, § 5.4 Modeling the Naming Environment, p.p. 227-239 (3rd ed.).
5. [Semantic Analysis][l05] ([6 slides/page][l05-6])
    - *Cooper et al*., Ch. 4 Context-Sensitive Analysis, § 4.2 Introduction to Type Systems, p.p. 164-181 (2nd ed.).
    - *Cooper et al*., Ch. 5 Syntax Driven Translation, § 5.5 Type Information, p.p. 239-251 (3rd ed.).
6. [Code Generation][l06] ([6 slides/page][l06-6])
    - _Cooper et al_., Ch. 6 The Procedure Abstraction, p.p. 269-330 (2nd ed.).
    - _Cooper et al_., Ch. 6 Implementing Procedures, p.p. 275-326 (3rd ed.).
    - _Cooper et al_., Ch. 7 Code shape, p.p. 331-404 (2nd ed.), p.p. 327-378 (3rd ed.).
1. [Program Analysis and Optimization][l07] ([6 slides/page][l07-6])
1. [Dataflow Analysis][l08] ([6 slides/page][l08-6])
1. [Loop Optimizations][l09] ([6 slides/page][l09-6])
1. [Register Allocation][l10] ([6 slides/page][l10-6])
1. [Parallelization][l11] ([6 slides/page][l11-6])
1. [Foundations of Dataflow Analysis][l12] ([6 slides/page][l12-6])

### Optional content

1. [Shift-Reduce Parsing][lshift] ([6 slides/page][lshift-6])
    - This used to be covered in past semesters. The material is useful for understanding the internals of some parser generators.
    - _Cooper et al_., Ch. 3 Parsers, § 3.4 Bottom-Up Parsing, p.p. 116-140 (2nd ed.), p.p. 118-142 (3rd ed.).

[l01]: assets/documents/lectures/L01-Introduction.pdf
[l01-6]: assets/documents/lectures/L01-Introduction-6pages.pdf
[l02]: assets/documents/lectures/L02-RegularExpressionsAndGrammars.pdf
[l02-6]: assets/documents/lectures/L02-RegularExpressionsAndGrammars-6pages.pdf
[l03]: assets/documents/lectures/L03-TopDownParsing.pdf
[l03-6]: assets/documents/lectures/L03-TopDownParsing-6pages.pdf
[l04]: assets/documents/lectures/L04-IntermediateFormats.pdf
[l04-6]: assets/documents/lectures/L04-IntermediateFormats-6pages.pdf
[l05]: assets/documents/lectures/L05-SemanticAnalysis.pdf
[l05-6]: assets/documents/lectures/L05-SemanticAnalysis-6pages.pdf
[l06]: assets/documents/lectures/L06-CodeGeneration.pdf
[l06-6]: assets/documents/lectures/L06-CodeGeneration-6pages.pdf
[l07]: assets/documents/lectures/L07-ProgramAnalysisOptimization.pdf
[l07-6]: assets/documents/lectures/L07-ProgramAnalysisOptimization-6pages.pdf
[l08]: assets/documents/lectures/L08-DataflowAnalysis.pdf
[l08-6]: assets/documents/lectures/L08-DataflowAnalysis-6pages.pdf
[l09]: assets/documents/lectures/L09-LoopOptimizations.pdf
[l09-6]: assets/documents/lectures/L09-LoopOptimizations-6pages.pdf
[l10]: assets/documents/lectures/L10-RegisterAllocation.pdf
[l10-6]: assets/documents/lectures/L10-RegisterAllocation-6pages.pdf
[l11]: assets/documents/lectures/L11-Parallelization.pdf
[l11-6]: assets/documents/lectures/L11-Parallelization-6pages.pdf
[l12]: assets/documents/lectures/L12-FoundationsOfDataflowAnalysis.pdf
[l12-6]: assets/documents/lectures/L12-FoundationsOfDataflowAnalysis-6pages.pdf

[lshift]: assets/documents/lectures/L-ShiftReduceParsing.pdf
[lshift-6]: assets/documents/lectures/L-ShiftReduceParsing-6pages.pdf
[cooper]: https://mit.primo.exlibrisgroup.com/permalink/01MIT_INST/jp08pj/alma9935028392606761

## Recitations

{:style="counter-reset:step-counter -1"}
1. Course Information ([Slides][r00])
1. Phase 1 ([Slides][r01])
1. Recursive Descent Parser ([Demo](https://github.com/6110-sp25/recitation2/))
1. Parser Generator ([Slides][r03], [Demo](https://github.com/6110-sp25/recitation3))
1. Phase 2 ([Slides][r04], [Demo](https://github.com/6110-sp25/recitation4))
1. SSA ([Slides][r05])
1. Control Flow Graphs ([Slides][r06], [Demo](https://github.com/6110-sp25/recitation6))
1. x86 Assembly ([Slides][r07])
1. Phase 4 and GDB crash course ([Slides][r09])
1. Register Allocation and Peephole Optimization ([Slides][r10])

[r00]: assets/documents/recitations/r0-course-information.pdf
[r01]: assets/documents/recitations/r01-project-overview-phase1.pdf
[r03]: assets/documents/recitations/r03-parser-generator.pdf
[r04]: assets/documents/recitations/r04-phase2.pdf
[r05]: assets/documents/recitations/r05-ssa.pdf
[r06]: assets/documents/recitations/r06-cfg.pdf
[r07]: assets/documents/recitations/r07-x86-asm.pdf
[r09]: assets/documents/recitations/r09-phase4.pdf
[r10]: assets/documents/recitations/r10-hack-peephole.pdf

## Re-lectures from 2024

1. [Regular expressions, automata, grammars, parse trees][rl-1] ([6 pages/slide][rl-1-6pages])
2. [High-level IR, semantic checking][rl-2] ([6 pages/slide][rl-2-6pages])
3. [Code generation][rl-3] ([6 pages/slide][rl-3-6pages])
4. [Program analysis and optimization][rl-4] ([6 pages/slide][rl-4-6pages])
5. [Foundations of dataflow analysis][rl-5] ([6 pages/slide][rl-5-6pages])
6. [Register allocation, loop optimizations, parallelization][rl-6]: see lecture slides L09, L10, and L11.

[rl-1]: assets/documents/relectures/relecture-1.pdf
[rl-1-6pages]: assets/documents/relectures/relecture-1-6pages.pdf
[rl-2]: assets/documents/relectures/relecture-2.pdf
[rl-2-6pages]: assets/documents/relectures/relecture-2-6pages.pdf
[rl-3]: assets/documents/relectures/relecture-3.pdf
[rl-3-6pages]: assets/documents/relectures/relecture-3-6pages.pdf
[rl-4]: assets/documents/relectures/relecture-4.pdf
[rl-4-6pages]: assets/documents/relectures/relecture-4-6pages.pdf
[rl-5]: assets/documents/relectures/relecture-5.pdf
[rl-5-6pages]: assets/documents/relectures/relecture-5-6pages.pdf
[rl-6]: assets/documents/relectures/relecture-6.pdf

## Recordings
All recitations and re-lectures are recorded and [published on Panopto](https://mit.hosted.panopto.com/Panopto/Pages/Sessions/List.aspx?folderID=d1b52dac-7f75-4148-ae95-b27f000ebb7e).
