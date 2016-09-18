llvm-cbe
========

The resurrected LLVM "C Backend", with improvements.

GOAL OF THIS FORK
=================

My primary goal is to have a backend I can use for this project:

Github: https://github.com/Ace17/dscripten
Blogpost: http://code.alaiwan.org/wp/?p=103
Demo: http://code.alaiwan.org/dscripten/full.html

In one word, it's a compiler from the D programming language to Javascript (asmjs).

The LLVM frontend for the D programming language, aka LDC, generates valid bitcode that JuliaComputing/llvm-cbe can't process (it generates uncompilable C code).
(It seems that the JuliaComputing/llvm-cbe was only tested with bitcode inputs that were generated by clang)

Moreover, the project in its current state needs a lot of cleaning, its merely consists of one huge source file and uses heavy C++ syntax that isn't required anymore, and the test suite is hard to use.

So my goal here is to have a codebase:
- that has no bugs blocking the dscripten project.
- that works with a recent version of LLVM (at the moment 3.8 vs JuliaComputing/llvm-cbe which requires LLVM 3.7)
- that can be compiled out of the LLVM tree (as long as there's one llvm-config in the PATH).
- that has a trustable suite of tests which directly feed llvm-cbe with deterministic LLVM bitcode (instead of relying on clang code generation, as JuliaComputing/llvm-cbe does).
- that don't require compromises on code cleanliness.
- whose output is standard ISO C99 code, instead of relying on the specifics of some compilers.

INSTALLATION INSTRUCTIONS
=========================

This version of the LLVM-CBE library works with LLVM 3.8. You will have to
compile this version of LLVM before you try to use LLVM-CBE. This
guide will walk you through the compilation and installation of both
tools and show usage statements to verify that the LLVM-CBE library is
compiled correctly.

The library is known to compile on various Linux versions (Redhat,
Mageia, Ubuntu, Debian), Mac OS X, and Windows (Mingw-w64).

Step 1a: Installing LLVM manually
=======================

LLVM-CBE currently requires LLVM 3.8 to be installed somewhere on your system,
and that the corresponding "llvm-config" be in your PATH.
(only LLVM is needed, not clang).

The first step is to compile LLVM on your machine:

```
$ cd $HOME
$ git clone https://github.com/llvm-mirror/llvm
$ cd llvm
$ git checkout release_38
$ ./configure
$ make
$ make install
```

At this point, you should have llvm-config in your path:
```
$ llvm-config --version
3.8.0svn
```

Step 1b: Install LLVM from repositories
=======================

Alternatively, a LLVM installed some other way can be used, e.g. installing the Debian packages.
Be aware that some distributions will suffix the 'llvm-config' program with LLVM version, e.g 'llvm-config-3.8'.
In this case, you will need to set the environment variable LLVM_CONFIG so the makefile knows which program to call.
Example:
```
$ export LLVM_CONFIG=llvm-config-3.8
```

Step 2: Compiling LLVM-CBE
==========================

Next, download and compile llvm-cbe:
```
$ cd $HOME
$ git clone https://github.com/JuliaComputing/llvm-cbe.git llvm-cbe
$ cd llvm-cbe
```

```
$ make
```

Step 3: Usage Examples
======================

Once llvm-cbe is compiled, you can run it with the following command.
```
$ bin/llvm-cbe main.ll -o=main.cbe.c
$ gcc -w main.cbe.c -o main.exe
$ ./main.exe
```

Run the test suite
==================
```
$ ./check
```

This will trigger the build and run the tests.

