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

In Phase 4, you will implement several dataflow optimizations. We will provide more information here in the future.

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