---
title: "Cool Resources on Compilers and Interpreters"
date: 2022-11-19T20:12:51+01:00
draft: false
showTOC: false
cover:
  image: /images/compiler_resources.png
---

Full disclosure, I'm no expert I just really like programming languages.
I use Rust and so a lot of resources
here will be geared towards that but hopefully the knowledge is general enough to be translated to your language of preference.

## How to use this guide

I tried my best to arrange them into categories that made sense to me.
I'm personally horrible at following rigid curriculums and prefer jumping around to whatever interests me.

### 1. **[Crafting Interpreters](http://craftinginterpreters.com/contents.html)**

This book is the best introduction to language development, I've seen.
Most programming books are either:

- A short blog post on writing a [fancy calculator](https://github.com/Ch1n3du/ziggy), or.
- A 600 page compiler textbook, that assumes you're already a master of Programming Language theory.

Crafting Interpreters is a mix of the best parts of both, it focuses mainly on applied stuff
but also makes sure to do justice to the theory associated with it.
Throughout the book, you implement an inter

All in all my only complaint with it is that the first part is written in Java, I personally don't like Java
and feel it's not well suited for writing languages.

### 2. Whatever you want

### Parsers

- **[Simple but Powerful Pratt Parsing](https://matklad.github.io/2020/04/13/simple-but-powerful-pratt-parsing.html)**

  Great article on how to write pratt parsers in Rust.

- **[Learning Parser Combinators With Rust](https://bodil.lol/parser-combinators/)**

  Tutorial on implementing Parser Combinators in Rust.

### Interpreters

- **[Crafting Interpreters](http://craftinginterpreters.com/contents.html)**

  The best.

- **[Writing a cheap fast interpreter](https://ndmitchell.com/downloads/slides-cheaply_writing_a_fast_interpreter-23_feb_2021.pdf)**

  Great article by Facebook engineers on writing fast interpreters and the known tradeoffs.

### Compilers

- **[How I wrote my own "proper" programming language](https://mukulrathi.com/create-your-own-programming-language/intro-to-compiler/)**

  Great tutorial on writing a compiler in Ocaml.

- **[Writing a WASM compiler in Rust](https://www.bitfalter.com/webassembly-compiler-text-format-and-ast)**

  Covers implementing a compiler for a subset of the WebAssembly spec in Rust.

- **[Cranelift JIT Demo](https://github.com/bytecodealliance/cranelift-jit-demo)**

  Example repo for using Cranelift for writing a JIT-compiled language.

### Miscellaneous

- **[An Incremental Approach to Compiler Construction](http://scheme2006.cs.uchicago.edu/11-ghuloum.pdf)**
- **[Risp (in (Rust) (Lisp))](https://stopa.io/post/222)**
- **[WebAssembly Specification](https://webassembly.github.io/spec/core/_download/WebAssembly.pdf)**
- **[WebAssembly Troubles](http://troubles.md/wasm-is-not-a-stack-machine/)**
