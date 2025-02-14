---
title: macOS Infrastructure
parent: Class Tools
nav_order: 40
---
There are unique challenges presented with using macOS for the compiler project. However, because many students use macOS as their primary work machine, we present this page to explain some workarounds that can be used to enable you to efficiently work on the compiler project natively on macOS.
## x86 Differences between macOS and Linux
- The entry point to the program must have an underscore (e.g. _main)
- All function names must start with an underscore (e.g. _func1)
- All external function calls must start with an underscore (e.g. _printf)
- `gcc` does not exist on macOS, it is symlinked to `clang`
- The syscall to exit on macOS is different:

```
movl $0x2000001, %eax
movl $<error_code>, %edi
syscall
```
## Cross Compiling on Apple Silicon

- If your Apple computer is using an x86 Intel chip, this section will not apply to you
- If you are using a computer with Apple Silicon (your chip is M1, M2, M3, etc.), you will need to cross compile your code as these chips are based on an ARM architecture

On Apple Silicon, pass `-arch x86_64` to cross compile to x86. 
You must also have Rosetta (translation layer between ARM and x86) installed (see this [Apple Support article](https://support.apple.com/en-us/102527)):

```
gcc -O0 -arch x86_64 compile.S -o compile.o
```
where compile.S is your assembly file and compiler.o is the output executable object file.
## Consider Adding a macOS CLI Flag

If your team has at least one macOS user, you may want to consider adding a command line flag (e.g. `-m, --mac`) to toggle the code generator between Linux and macOS mode. This is because the autograder will be running on Linux, but you may want to debug on macOS.
## LLDB vs GDB
It is often quite difficult to get `gdb` to work properly on macOS, and impossible on Apple Silicon natively. You may want to consider using a virtual machine to run x86 Linux.

Another alternative is to sync your files with the [Athena dialup](https://web.mit.edu/dialup/www/ssh.html) and do your debugging on their x86 CPUs.
