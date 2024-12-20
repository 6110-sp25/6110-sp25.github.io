---
title: Resources
nav_order: 40
---

This page contains a number of useful and/or interesting references selected by the staff. You are not expected to know most of the material on this page for the class; however, you may find it interesting and helpful. We had a lot of fun curating this!

{% include toc.html %}

## References

You will find these references useful in writing your compiler.

1. The complete [Intel x64 manual](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) --- official PDFs with all the gory details
1. [x86 and amd64 instruction reference](https://www.felixcloutier.com/x86/) --- navigable reference by Félix Cloutier derived from the Intel manual
1. [x86 wiki](https://en.wikibooks.org/wiki/X86_Assembly/X86_Instructions) --- good primer on how x86 instructions work
1. [x64 cheat sheet](https://cs.brown.edu/courses/cs033/docs/guides/x64_cheatsheet.pdf) --- lists and tables detailing registers and assembly commands from Brown University's CS033
1. [Agner Fog's optimization page](https://agner.org/optimize/) --- this is a very useful reference page with manuals on how to optimize code for x86-64. In particular, take a look at the [latency tables](https://agner.org/optimize/instruction_tables.pdf).
1. [Godbolt](https://godbolt.org) --- allows you to input C code and gives you line-by-line assembly output of various compilers (such as gcc and clang), very useful for figuring out how to translate certain operations to assembly.

If you find interesting resources you think other students could benefit from, please consider sharing them on Piazza!

## Related compiler courses

You might find lecture slides/notes from other publicly available courses useful, especially as you get into the later phases of the project:

1. [Carnegie Mellon's 15-411 Compiler Design course](https://www.cs.cmu.edu/~janh/courses/411/23/schedule.html).
1. [Harvard's CS153 Compilers class](https://groups.seas.harvard.edu/courses/cs153/2019fa/schedule.html).

If you are hungry for more advanced material, take a look at this:

{:style="counter-reset:step-counter 2"}
1. [Cornell's self-guided, online Advanced Compilers course](https://www.cs.cornell.edu/courses/cs6120/2020fa/self-guided/).

Of course, do not forget this class's cousin in the fall semester: [6.1120<sub>[6.818]</sub> Dynamic Computer Language Engineering][dynlang].

For more theoretical approaches to programming language design, theory, and implementation, look into these classes:
1. [6.S050 Programming Language Design (Spring 2023)][pld]
1. [6.5110<sub>\[6.820\]</sub> Foundations of Program Analysis][6820]
1. [6.5120<sub>\[6.822\]</sub> Formal Reasoning About Programs][frap]

## Toolings

Although we would like to focus on ideas, not specific tools, it is important for you to gain fluency in whatever it is that you choose to use. Minutes spent familiarizing yourself with common tools and frameworks will save you hours or days in the long run.

1. Shell
    1. [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/2020/) --- Learn about the shell and the terminal, then you look like a pro.
1. Scala
    1. [Tour of Scala](https://docs.scala-lang.org/tour/tour-of-scala.html)
    1. [Scala Book](https://docs.scala-lang.org/overviews/scala-book/introduction.html)
    1. [Scala Patterns for Compiler Design](https://gist.github.com/rcoh/4992969)


## Recommended textbooks

There is no required textbook for this class, but if you insist, we think this is a good reference textbook to have:

1. **Cooper, Keith D., and Torczon, Linda, [_Engineering a Compiler_][cooper], 3rd ed., Morgan Kaufmann, 2022.**
  - A new reference textbook for understanding modern compilers. Covers many optimizations seen in this course (e.g. data-flow, instruction scheduling, register allocation) plus a few advanced ones found in modern compilers (e.g. [static single-assignment (SSA)][ssa]).
  - The 3rd edition is more up to date, but the 2nd edition (2012) is [available online through MIT Libraries][cooper-mit]. In supplemental readings, we will list the pages for both editions.

Other compiler textbooks you may be interested in:

{:style="counter-reset:step-counter 1"}
1. **Robert Nystrom, [_Crafting Interpreters_](https://craftinginterpreters.com/). Genever Benning, 2021.**
  - Our default recommendation for people who want to learn about compilers in general (i.e. outside of this course).
  - This is an _amazing_ introduction to implementing a parser, an interpreter, and a bytecode virtual machine in Java and C.
  - The section on parsers may be useful for phase 1 of the project.
  - The rest of the book is arguably more relevant to [6.1120<sub>\[6.818\]</sub> Dynamic Computer Language Engineering][dynlang].
1. **A. W. Appel and J. Palsberg, [_Modern Compiler Implementation in Java_][appel-java]. Cambridge University Press, 2002.**
  - A classic book. Guides you through a compiler project with thoughtful code organization. Otherwise similar to _Cooper et al._ in content.
  - Called the "tiger book". Also has a C version and ML version.
1. **Aho, Alfred V., et al., [_Compilers: Principles, Techniques, & Tools_][dragon], 2nd ed. Pearson Addison-Wesley, 2007.**
  - A very, very, thick, classic reference on writing an optimizing compiler for C-like languages (e.g. Decaf).
  - Also called the "dragon book." Everyone has heard of this.
1. **Steven Muchnick, [_Advanced Compiler Design and Implementation_][appel-java]. Morgan Kaufmann, 1997.**
  - The "whale book." A comprehensive coverage of advanced compiler optimizations.
1. **Queinnec, C., and Callaway K., [_Lisp in Small Pieces_][lisp-book]. Cambridge University Press, 1996.**
  - A very cool book that builds _multiple_ interpreters from scratch to demonstrate how language design choices interact.

<!-- These textbooks are a lot more theoretical, and cover very specific topics:

{:style="counter-reset:step-counter 5"}
1.
  - The mathematical foundations of data-flow analysis.
  - Matt Might wrote [a short summary of order theory][might-lattice] on his blog. -->

Many of these textbooks are [available online through MIT Libraries][mitlib]. (For some books, you can borrow physical copies!) You can purchase the textbooks through traditional means (e.g. Amazon) or ask the TAs for advice on acquiring the textbooks.

## Other sources of inspiration

1. Overviews
    1. [LLVM compiler architecture](http://www.aosabook.org/en/llvm.html)
    1. [GCC compiler architecture](http://en.wikibooks.org/wiki/GNU_C_Compiler_Internals/GNU_C_Compiler_Architecture)
1. Blogs
    1. [Russ Cox's Blog](http://research.swtch.com/) --- Russ is one of the developers of Go, a popular language.
    1. [Matt Might's Blog](http://matt.might.net/articles/) --- Matt is a professor at the University of Utah and has written some very interesting articles (e.g. "Yacc is dead")
    1. [Ralf's Ramblings](https://www.ralfj.de/blog/) --- Ralf wrote a lot of popular blog posts exploring the complexity behind systems languages like C++ and Rust.
    1. [Embedded in Academia](https://blog.regehr.org/) --- Ditto.
1. Papers
    1. [Register Allocation & Spilling via Graph Coloring](http://dl.acm.org/citation.cfm?id=806984) --- G.J. Chaitin / 1982. Great (short) paper on simple register allocation.
    1. [Linear Scan Register Allocation](https://dl.acm.org/citation.cfm?id=330250)
    1. [Iterated Register Coalescing](http://dl.acm.org/citation.cfm?id=229546) --- Lal George / 1996. Presents improvements/alternative to Chaitin's design. If Chaitin-style (+/-Briggs) register allocation isn't enough for you, this paper is a good read - actually, it's a good read anyway, to understand the tradeoffs
    1. [Superword Level Parallelism](http://dl.acm.org/citation.cfm?id=358438) combined with loop unrolling, a simple way to implement a vectorizing compiler
1. Theory
    1. [Order Theory for Computer Scientists][might-lattice] --- Matt Might's summary on the foundations of data-flow analysis.
    2. Davey, B. A., and H. A. Priestley, [_Introduction to Lattices and Order_][lattice], 2nd ed. Cambridge University Press, 2002.

[cooper]: https://shop.elsevier.com/books/engineering-a-compiler/cooper/978-0-12-815412-0
[cooper-mit]: https://mit.primo.exlibrisgroup.com/permalink/01MIT_INST/jp08pj/alma9935028392606761
[appel-java]: https://www.cs.princeton.edu/~appel/modern/java/
[ssa]: https://en.wikipedia.org/wiki/Static_single-assignment_form
[lisp-book]: https://doi.org/10.1017/CBO9781139172974
[mitlib]: https://libraries.mit.edu/
[dragon]: https://suif.stanford.edu/dragonbook/
[might-lattice]: https://matt.might.net/articles/partial-orders/
[lattice]: https://doi.org/10.1017/CBO9780511809088
[pld]: https://people.csail.mit.edu/feser/pld-s23/index.html
[dynlang]: https://student.mit.edu/catalog/m6a.html#6.1120
[6820]: https://student.mit.edu/catalog/m6a.html#6.5110
[frap]: https://frap.csail.mit.edu/main
