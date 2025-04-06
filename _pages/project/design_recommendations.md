---
title: Design Recommendations
parent: Compiler Project
nav_order: 10
---

The Decaf Compiler project is one of the few MIT software engineering experiences that will start from a specification and end with a large software product. There are many design decisions that you must make along the way, and there are an infinite number of ways to go about designing your compiler.

However, we also understand if teams find the number of decisions that must be made overwhelming. The TAs have compiled the following resources, divided up by phase, that will hopefully help you make these design decisions.

## Phase 3

{% include svg/phase3.svg %}

At the end of Phase 2, you should have a high-level intermediate representation of your program that is (ostensibly) semantically valid. The goal of Phase 3 is to generate *correct* x86 code. It is expected that your code may be very slow, but we will spend the second half of the class optimizing the generated x86 code.

###  Linearizer

This is the part of your compiler that takes complex expressions in your high-level IR and turn it into simpler, linear three-address instructions. At this point, the control flow also must be reduced to jumps. There are many ways to go about doing this, and it depends on how your high-level IR is structured. We may recommend some of the following resources:

- Copper et al, *Engineering a Compiler*, **Chapter 4**, has a detailed description of how you may go about designing your CFG, along with suggested data structures
- Aho et al, (Dragon Book) *Compilers Principles Techniques and Tools*, **Chapter 6**, has a detailed description of how to represent many syntax structures in a CFG, including some common language features that are not present in Decaf. A simplified version of the CFG in the book can be instructive for your design.

### SSA (Static Single Assignment)

If you have not encountered SSA before, please review the recitation video and slides on SSA.

One of the most important decisions for your team in Phases 3-5 is whether or not to implement a compiler with static single assignment (SSA) form. It is not necessary to implement SSA to get an A in the class, but in general, the top performing compilers have used SSA.

The main reason why SSA is useful is because it allows for more efficient dataflow analysis, which allows your Phase 4 and 5 optimizations to run more optimally. However, it does add additional implementation complexity to your code. In particular, you must write a CFG pass to convert your CFG to SSA-form, and then another CFG pass to convert it out of SSA-form (known as de-SSA). 

In terms of workflow, teams that use SSA will have more work during Phase 3, and less during Phase 4, because SSA makes dataflow optimizations much easier to implement. On the flip side, teams that do not use SSA will have less work in Phase 3 and more during Phase 4.

### Converting to SSA Form

The conversion of a CFG to SSA-form involves converting all variables to only be assigned once. As mentioned in the recitation, due to control flow, we also must use phi nodes to resolve certain variables. The key issue that must be resolved in converting to SSA-form is the *optimal placement of phi nodes*. There are many research papers on this topic, as well as textbooks that give a canonical version of this. We would recommend the following resources:

- Copper et al, *Engineering a Compiler*, **Section 9.3**, has a very good explanation of how to construct SSA form
- [*SSA-based Compiler Design* (The SSA Book)](https://pfalcon.github.io/ssabook/latest/book-full.pdf): **Section 3.1**, also has a similar explanation on how to construct SSA form

The canonical academic paper that details how to convert into SSA form from a *non-SSA CFG* would be:

- Cytron et al (1991), [Efficiently Computing Static Single Assignment Form and the Control Dependence Graph](https://www.cs.utexas.edu/~pingali/CS380C/2010/papers/ssaCytron.pdf). This uses the idea of *dominance frontiers*, but does implicitly require that you already have a non-SSA CFG already.

There are also ways to go directly from a high-level IR to SSA form. The canonical paper in this would be:

- Braun et al (2013), [Simple and Efficient Construction of Static Single Assignment Form](https://c9x.me/compile/bib/braun13cc.pdf), which is used in the Go Compiler and OpenJDK

You may also find this paper on Dominance Frontiers to be instructive:

- Copper et al (2006), [A Simple, Fast Dominance Algorithm](https://repository.rice.edu/items/99a574c3-90fe-4a00-adf9-ce73a21df2ed)

### Converting out of SSA Form (de-SSA)

Conversion out of SSA form is required prior to code generation because phi nodes have no analogue in machine code. Therefore, the phi nodes must be converted to copy operations before it is able to be translated into x86. The key issue with this phase is how to deal with the *lost copy* and *swap* problems, which arise when you have cycles (i.e. loops) in your CFG. We recommend the following resources:

- Copper et al, *Engineering a Compiler*, **Section 9.3.5**, gives a clear explanation of the lost copy and swap problems. It also provides an algorithm that solves both of these problems which you can directly implement in theory.
- [*SSA-based Compiler Design* (The SSA Book)](https://pfalcon.github.io/ssabook/latest/book-full.pdf): **Section 3.2**, offers psuedocode for algorithms that correctly destructs SSA, but doesn't explain the problems in detail; you may want to read the Copper book before analyzing the code in the SSA book.

The canonical acadamic paper for de-SSA is:

- Boissinot et al (2009), [Revisiting Out-of-SSA Translation for Correctness, Code Quality, and Efficiency](https://ieeexplore.ieee.org/document/4907656)

### Hybrid CFG Representations

You may also decide that you want to merge your non-SSA CFG and SSA CFG representations. You can do this by having a CFG representation that contains a phi nodes. When you linearize your high level IR, you can insert these phi nodes, and before converting to x86, another CFG pass removes the phi nodes from the graph before handing it off to the code generator.

Some advantages of using a hybrid IR:

- Less redundant code, as much of your non-SSA and SSA CFG representations will be the same
- Similar in architecture to LLVM
- Feel smart :)

Some disadvantages:

- Much harder to debug, because you've coupled SSA form to your CFG implementation
  - It's harder to tell if your SSA passes or CFG is broken
- Have to keep track of which state your CFG is in (whether it contains phi nodes), and run optimizations accordingly

### Code Generator

The code generator takes a **non-SSA** CFG and converts it into x86 assembly code. You should design your CFG to be conducive to code generation. Some things you may want to consider:

- Consider making your assembly code easy to manipulate programmatically (i.e. *do not directly emit strings*), because you may be performing optimizations on assembly in later phases.
- If you do choose to implement SSA, make sure your code generator is correct *regardless of* whether or not SSA translation is happening in your CFG. This will help with debugging.
- Consult the [macOS Infrastructure page](macos) for differences in assembly between Linux and macOS, as well as instructions on how to cross-compile to x86 on Apple Silicon. Consider adding a macOS specific flag for easier testing.
- In Phase 3, focus on **correctness**, and forget about speed for right now. The easiest way to do this is to make extensive use of the stack for variable storage.

Some additional resources you may want to consult:

- Aho et al, (Dragon Book) *Compilers Principles Techniques and Tools*, **Chapter 8**, describes the code generation step in great detail. There are some optimizations in the chapter, such as peephole and instruction selection, that may want to defer to later phases.
- The [x86 references](resources#references) on the Resources page
- [Godbolt](godbolt.org), where you can explore how various compilers produce assembly and take inspiration from them

## Phase 4

{% include svg/phase4.svg %}

In Phase 4, you will implement several dataflow optimizations. 

### Global Copy Propagation

Copy propagation aims to reduce variable copies, and more importantly, **uncover opportunities for other optimizations** (for example, `x = a + b; c = b; y = a + c` would become `x = a + b; c = b; y = a + b` which enables common subexpression elimination).

* If you are using a non-SSA IR, refer to Martin's slides, §12.5 of the Whale book, and §10.7 of the Dragon book. Basically, you can only propagate a copy `x = y` if `y` **cannot have been modified between the copy and the use of `x`.**
* If you are using a SSA IR, the legality check above is trivially true thanks to the single-assignment property. There are, however, a few extra things you need to be careful about:
  * **Phi nodes with identical arguments are copies in disguise.** After some copies have been propagated, you may see phi nodes where all arguments are the same, such as `x_2 = phi(x_1, x_1, x_1)`. That's a new copy! Your pass should ideally move on and replace all uses of `x_2` with `x_1`.
  * **The lost copy problem.** Copy propagation creates extra complication when converting out of SSA. See [these slides](https://www.inf.ed.ac.uk/teaching/courses/copt/lecture-4-from-ssa.pdf) for an example of the problem. TL,DR: watch out for **critical edges** (edges A->B where A has mulitple successors and B has multiple predecessors), and eliminate them by inserting empty blocks (replace A->B with A->C->B where C is an empty block). **You must be aware of this problem or you risk banging your head against the wall for days.**
  * Your algorithm may first find a copy `x = y` and replace all uses of `x` with `y`, but that requires a full CFG traversal per copy! Alternatively, you can maintain a map `orig` initialized to `orig[x] = x`. Then, on seeing `x = y`, update `orig[x] = orig[y]` and replace all use of `x` with `orig[x]` as you go. This way, one traversal propagates many copies and it handles transitivity quite well. We recommend traversing the blocks in **dominator tree pre-order,** as it ensures all uses of a variable `x` is visited after its definition (where `orig[x]` might be updated).
* You may find that one full CFG traversal may not suffice for propagating all copies however your try --- that's totally fine. In this case, just do multiple traversals until the last traversal did not introduce any changes. 
* Copy propagation can leave many dead copies behind. There's no need to clean them up as part of the copy propagation pass --- leave it to dead code elimination!

### Global Common Subexpression Elimination

* If you are using a non-SSA IR, refer to Martin's slides, §8.3 and §13.1 of the Whale book, §10.6 and §10.7 in the Dragon book, and §17.2 in the Tiger book.
* SSA again makes things simplier here because we don't have to worry about variable reassignments making an expression unavailable. 
* In this pass, you will need a map that is keyed on expressions. Assuming a hash map, this means you will need to define hash and equality functions for an expression tree. Isn't it tedious? While this is the proper way to go, a shortcut is to write a function that maps an expression to **a stringified canonical representation,** and make you map keyed on these strings instead. This remains correct as long as you do it carefully.
* You may be tempted to write your pass in a way that handles commutativity and associativity, or even some algebraic identities. **Resist this high IQ move for a moment and think twice about the cost-benefit tradeoff before you go down this rabbit hole.** Determine whether two expressions are equivalent is hard (see [Word problem](https://en.wikipedia.org/wiki/Word_problem_(mathematics))). Doing so efficiently (e.g., by mapping equivalent expressions to the same hash or canonical expression) is even harder. The reality is that many eliminatable common expressions are written the same verbatim in the source code, so even a naive approach can work quite well.

### Global Dead Code Elimination

* If you are using a non-SSA IR, refer to Martin's slides, §18.10 of the Whale book, and §10.2 of the Dragon book.
* SSA makes things simpler here as well. DCE over non-SSA IR is done backwards to detect useless (re)-assignments to a variable. With SSA, re-assignments of source program variables are already broken into multiple SSA variable definitions. Hence, DCE over SSA need not be done in strictly backwards order. Instead, consider a worklist algorithm: 
  1. Initialize the worklist with all variables used in block terminators (such as conditional jumps or returns); 
  2. For every variable in the worklist, 
      1. mark it as alive, and 
      2. add all variables referenced in its definition to the worklist (if not already); 
  3. Remove all variable definitions not marked alive.
* Whether you are using SSA or not, **make sure not to eliminate instructions with side effects.** This includes **function calls and array stores**. Of course, there are exceptions where these can be eliminated after analysis, but they count as advanced optimizations and are best placed in their own passes.
* You may also write optimizations that **eliminate dead blocks that are provably unreachable.** These include 
  1. any block without a path to it from the entry block in the CFG (could be leftover from previous optimizations), or, 
  2. any block with a path to it from the entry block in the CFG, but **there's some edge that's provably untaken.** For example, a jump conditioned on a constant value will make one of the target branches never taken.
  That said, dead block are much rarer in general than dead instructions, and you don't need to implement them to be considered to have implemented DCE for the purpose of this course. See §18.1 of the Whale book and §9.9 of the Dragon book.

### How Should I Order My Optimizations?

You may realize at this point that some ordering of optimizations makes more sense than others. For example, it's better to arrange DCE to run last rather than first so it can also eliminate junk generated by other optimizations. However, things are not always this simple. Consider:

* Copy propagation uncovers more opportunities for Common Subexpression Elimination, but
* CSE can replace complex expressions to copies, inviting another run of copy propagation.

Which should go first? And the ordering gets more complicated as you add more passes to your compiler. The optimal scheduling of optimization passes remains an actively researched topic. For the purpose of our compiler project, a reasonable strategy is to

1. Come up with some ordering of passes that somewhat makes sense, but try not to overthink it.
2. Run passes in that order X times, where X is a small integer (say 10).

This way, every pass gets to run before or after every other pass for a sufficient number of times and hopefully that should exploit every optimization opportunity. Real production compliers don't adopt this strategy because it may unnecessarily slow down compilation, but compile time is not a big concern for this class. 

The downside of such blindly throwing passes to your IR is that it makes debugging harder. An incorrect transformation anywhere can cause your compiler to miscompile. Therefore, we make the following advice:

### General Principles When Implementing an Optimization

To make your lives easier, we highly recommend making your pass **pure, idempotent, and deterministic.**
1. **Purity** means that your optimization pass should be an IR-to-IR transformation with no global side effects. This makes it easier to analyze, compose, and test in isolation. 
2. **Idempotence** means that your optimization pass **should not modify anything in the IR** if it doesn't find any optimization opportunity it's designed to exploit. This way, if you find out during your debugging that a certain pass does not contribute to the bug you are working on, you can confidently disable that pass without worrying possible cascade effects that could complicate debugging.
3. **Determinism** means that your optimization pass should produce identical output IRs from run to run given the same input IR. In some languages, hash map iteration order is not determinisic --- be careful! Determinism makes reproducing bugs (and isolating them into regression tests) much easier.

### Other Optimizations

* **Constant Propagation:** Compile-time interpretation of expressions whose operands are compile time constants. See the algorithm described in §12.1 of the Whale book. SSA makes constant propagation easier as well (unsurprisingly).
* **Loop-invariant Code Motion (code hoisting):** Moving invariant code from within a loop to a block prior to that loop. See §13.2 of the Whale book and §10.7 of the Dragon book, or Martin's slides. SSA does not necessarily make loop optimizations easier --- it's just different. You may be interested in knowing that LLVM uses a special form of SSA (LCSSA) for loop optimizations. See [here](https://llvm.org/docs/LoopTerminology.html). 
* **[Loop inversion](https://en.wikipedia.org/wiki/Loop_inversion):** Converts a while loop into a do-while loop.
```c
  void pre_inversion() {
      while (/* condition */) {
          /* loop body */
      }
  }
  // Gets converted to  
  void post_inversion() {
      if (/* condition */) {
          do {
              /* loop body */
          } while (/* condition */);
      }
  }
```
Recall that an argument against aggressive hoisting is that it makes the
performance worse if the loop is never executed. With loop inversion, the
hoisted code is contained in the if branch, so it only executed if the loop runs
at least once.
* **Global Value Numbering-Partial Redundancy Elimination (GVN-PRE):** This is a very powerful optimization that entails Common Subexpression Elimination, Loop-invariant Code Motion (after loop inversion), and more. It is conceptually elegant, but significantly more complicated to implement. See Thomas VanDrunen's [PhD thesis](https://www.cs.purdue.edu/homes/hosking/papers/vandrunen.pdf), or [the conference paper](https://link.springer.com/chapter/10.1007/978-3-540-24723-4_12).

## Phase 5

{% include svg/phase5.svg %}

In Phase 5, you will implement register allocation, as well as other optimizations (such as peephole optimizations) of your choosing. We will provide more information here in the future.

## Just show me the code: IR designs in the wild

A great way to learn compiler design is to see how experienced compiler devs do it. Below we have curated a list of open source compilers that could serve as valuable references to your design. Keep in mind, however, that many of these compilers have greater scopes and deal with more complex languages. **Don't copy their design as is and use your best judgement to avoid overengineering.**

* **LLVM** is a compiler backend framework used by Clang (C/C++/Fortan), Rust, Swift, and many more languages and a gold mine of SSA-based optimizations.
  * Skim [the Language Reference Manual](https://llvm.org/docs/LangRef.html) to get a sense of SSA IR.
  * If you want to learn how a particular optimization is implemented in LLVM, start by reading [the Programmer's Manual](https://llvm.org/docs/ProgrammersManual.html). You may also ask LLMs to help you navigate the codebase.
* **Cranelift** is a Rust-based compiler backend used in many WebAssembly implementations. If your team is using Rust, Cranelift's codebase is a valuable reference of how to represent your IR with Rust constructs.
  * Read [the IR Reference](https://github.com/bytecodealliance/wasmtime/blob/main/cranelift/docs/ir.md).
  * For more context, Cranelift uses a variation of SSA called **basic blocks with parameters** that's equivalent to phi nodes. It also uses a **data flow graph** to explicitly track dependencies between instructions.
  * Cranelift's codebase can be intimidating. We recommend starting with 
    [`InstructionData`](https://docs.rs/cranelift-codegen/latest/cranelift_codegen/ir/instructions/enum.InstructionData.html), [`Layout`](https://docs.rs/cranelift-codegen/latest/cranelift_codegen/ir/layout/struct.Layout.html), and [`DataFlowGraph`](https://docs.rs/cranelift-codegen/latest/cranelift_codegen/ir/dfg/struct.DataFlowGraph.html).
* **The Go compiler** is written in Go and also uses an SSA-based IR. Notably, it implements a **linear scan register allocator** directly over SSA. If your team is considering a similar approach, Go's register allocator may be a good read.
  * Red Hat has a [blog post](https://developers.redhat.com/articles/2024/09/24/go-compiler-register-allocation#conclusion) that gives a high-level overview of Go's register allocator.
  * The code can be found [here](https://github.com/golang/go/blob/master/src/cmd/compile/internal/ssa/regalloc.go), roughly 3000 lines including extensive comments, and should be understandable with effort.
* **libFirm** is a graph based SSA compiler backend originally developed by Sebastian Hack and contains the reference implementation of his **graph coloring register allocation allocator directly over SSA.** As many of you will learn later in the course the hard way, direct register allocation over SSA is deep rabbit hole and his paper doesn't give out enough details. This is when the libFirm code comes in handy.
  * Start by reading [`bespill.c`](https://github.com/libfirm/libfirm/blob/master/ir/be/bespill.c) and [`bespillbelady.c`](https://github.com/libfirm/libfirm/blob/master/ir/be/bespillbelady.c).
* [**QBE**](https://c9x.me/compile/) is a compiler backend in C that aims to "provide 70% of the performance of industrial compilers with 10% of the code." It implements many of the key optimizations and the author deliberately keeps the codebase at hobby project scale so it's more understandable and hackable. If you find the code of all compilers above intimidating, try this!

Finally, we want to stress that while these open source compilers can be a valuable source of inspiration, you should **NOT** copy code from them directly or call them as libraries for your compiler, as it will trivialize the project and is forbidden per course policy.