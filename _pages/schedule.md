---
title: Schedule
nav_order: 20
---

{% include toc.html %}

## Changelog

- **2/11/25**: Changed recitation location on TR from 32-124 to 32-155.

## Calendar

The lectures and recitations are one hour long and take place:
- In **32-123** at 11 a.m. on Mondays, Wednesdays, and Fridays.
- In **32-155** at 12 p.m. (noon) on Tuesdays and Thursdays. (While these are listed officially as "recitations," we treat them the same as lectures.)

[bldg]: http://whereis.mit.edu/map-jpg?mapterms=32

|     | Monday | Tuesday | Wednesday | Thursday | Friday |
| :-: | :----: | :-----: | :-------: | :------: | :----: |
| 02/03 - 02/07 | _First day of classes_ <br/> [L1: Intro][lecs], [R0: Info][recs] | [L2: Regex][lecs] | [L2: Regex][lecs] | [L3: Parsing][lecs] | Phase 1 released <br/> [R1: Phase 1][recs] |
| 02/10 - 02/14 |  | [R2: Hand-Written Parser][recs] | [L4: IR][lecs] | [L4: IR][lecs] | [R3: Parser Generators][recs] |
| 02/17 - 02/21 | _Presidents' Day Holiday_ | _Monday schedule_<br />[L5: Semantics][lecs] | **Team submission**<br />[L5: Semantics][lecs] | | **Phase 1 due** <br/> Phase 2 released <br/> [R4: Phase 2][recs] |
| 02/24 - 02/28 | [L6: Codegen][lecs] | [L6: Codegen][lecs] | [L6: Codegen][lecs] |  | [R5: SSA][recs] |
| 03/03 - 03/07 |  |  |  | [R6: Control Flow Graphs][recs] | _Add date_ <br/> **Phase 2 due** <br/> Phase 3 released <br/> [R7: Phase 3][recs] |
| 03/10 - 03/14 | | | [Quiz 1 Review][reviews] | | **Quiz 1** |
| 03/17 - 03/21 |                                                              | [L7: Optimization][lecs] | [L8: Dataflow][lecs] | [L8: Dataflow][lecs] | |
| 03/24 - 03/28 | _Spring Break_ | _Spring Break_ | _Spring Break_ | _Spring Break_ | _Spring Break_ |
| 03/31 - 04/04 | [L9: Loop Optimizations][lecs] | [L10: Register Allocation][lecs] | [L11: Parallelization][lecs] | [L12: Dataflow Theory][lecs] | **Phase 3 due** <br/> Phase 4 released <br/> [R9: Phase 4][recs] |
| 04/07 - 04/11 |                                                              |                                            |                          |                     |                       [R10: Register Allocation][recs]                       |
| 04/14 - 04/18 | [L12: Dataflow Theory][lecs] | [L12: Dataflow Theory][lecs] | [L11: Parallelization][lecs]  | L13: Memory Optimization <br /> *CPW* | **Phase 4 due** <br/> Phase 5 released <br />*CPW*<br/> [R11: Phase 5][recs] |
| 04/21 - 04/25 | _Patriots' Day Holiday_ | _Drop Date_ |  | | |
| 04/28 - 05/02 | | | [Quiz 2 Review][reviews] | **Guest Lecture** <br/> Yaron Minsky <br/> (Jane Street) | **Quiz 2** |
| 05/05 - 05/09 | | | | | |
| 05/12 - 05/16 | **Phase 5 due** | _Last day of classes_ <br/> Complier Derby | | | |

[lecs]: {% link _pages/lecture-notes.md %}#lectures
[recs]: {% link _pages/lecture-notes.md %}#recitations
[relecs]: {% link _pages/lecture-notes.md %}#re-lectures
[reviews]: {% link _pages/lecture-notes.md %}#quiz-reviews

## List of topics

- Parsing
  - Regular expressions
  - Deterministic and non-deterministic finite automata
  - Context-free grammars
  - Parse trees
  - Recursive descent parser
  - Parser construction
- Intermediate representations
  - Object-oriented programming
  - Symbol tables
  - Semantic analysis
- Unoptimized code generation
  - Control flow graph
  - Linearizing
  - Short circuiting
  - Assembly code
  - Procedure calls and stack management
  - Peephole optimizations
- Program analysis and optimizations
  - Value numbering
  - Common sub-expression elimination
  - Copy propagation
  - Constant propagation
  - Dead code elimination
  - Strength reduction
  - Algebraic simplification
- Register allocation
  - Def-use chains
  - Web-based register allocation
  - Interference graphs
  - Liveness analysis
  - Graph coloring with Chaitin's Algorithm
  - Spilling and splitting
- Parallelization
  - Data dependence analysis
  - Induction variables
- Data-flow analysis foundations
  - Order theory and lattices
  - Fixed point iteration
