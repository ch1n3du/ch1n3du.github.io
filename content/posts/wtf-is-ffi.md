---
title: "wtf Is ffi?"
date: 2023-07-22T19:00:57+01:00
draft: true
---

## The Need 😒

You're a CS student who hungers endlessly for a challenge, you find fleeting solace the in the joy of making electrons dance and give life to your ideas.
In need of another fix, you decide on your next project, a GUI packet monitoring app because you wish to see the packets flow within the wires and pick them apart as you please.
You're *"not like the other devs"™* so you're doing it in Rust (🦀𝖙𝖍𝖎𝖘 𝖕𝖑𝖊𝖆𝖘𝖊𝖘 𝖙𝖍𝖊 𝖈𝖗𝖆𝖇 𝖌𝖔𝖉🦀).

You need a library for handling the packet, you search far and wide but alas nothing on crates.io matches your need.
There exists one library that fits the bill, but it's...it's in C++ (🦀𝖆𝖇𝖔𝖒𝖎𝖓𝖆𝖙𝖎𝖔𝖓!🦀).
If only there was a way to use the library from in Rust, (mostly) never leaving the loving embrace of the Borrow Checker.
You're ready to unknowingly reinvent FFI, *"how bad can it be?"* (🦀𝕴'𝖑𝖑 𝖆𝖑𝖑𝖔𝖜 𝖎𝖙🦀).

## The Challenges 😩

You recently finished the *Crafting Interpreters* and wrote some HDL one time, and now you see the world for what it truly is, that the line between Compilers and Interpreters is mirage, binaries are just interpreted *in silico*.
Rust and C++ compile to the same assembly at the lowest level, so we should be it should be possible ideally to just call a C function in Rust and vice-versa.

### Example

You open your editor and try to remember what little `C` you know to write plausible looking code:

```C
typedef {
  *char srcPath,
  unsigned long sizeLimit 
} struct Config

bool isWithinLimits(*Config config) {
  // Magic ✨
}
```

Now to connect to it in Rust, ideally you would be being able to call `isWithinLimits` directly in Rust:

```rust
// We define this type to have rust 
// representation of the same value.
struct Config {
  src_path: String,
  size_limit: u64,
}

extern "C" fn isWithinLimits(config: &Config) -> bool;
```

You see through the facade of abstraction, you're definitely *"not like the other devs"™*  (●'◡'●).
But alas there are several things wrong with the Rust code:

1. The `src_path` field is a Rust `String` which is basically a character pointer and a length value.
But the `C` string is a pointer to multiple `char` values in memory that we read till we reach a `null` value which signifies the end of the string.

2. In C if you define a `struct` all the fields are laid out in memory in the order you declared them, this is simple, but it can lead to poor performance as the ordering of the fields affects how much data you can pack into the cache at once and if you don't lay out fields the right way it can lead to *huge* performance differences.
In Rust the compiler helps you by finding the optimal memory layout for the struct fields based on the target architecture.
This causes *serious pain* because if Rust switches the places of `src_path` and `size_limit` the `C` compiler can't tell that and all the `C` compiler knows is that when it sees code to dereference `srcPath` all it knows is `srcPath` is meant to be first, so it generates assembly that just reads the first 8 bytes from the section of memory the struct is stored at, and then it tries to treat that as a memory address and now Satan has entered your system. *╰(\*°▽°\*)╯*

![[5xjo6808s5db1.jpg|300]]

The crux of the issue is that different languages have different languages use different ways of representing language constructs like functions, classes, structs, etc.

## The Solution 🕊️

All might seem lost but fear (mostly) not things can work, but we need to get some stuff straight first, a way to tell the other languages *"please don't fuck my code (>w<)"*. We need a way for defining an *interface* between the *binary* representations of *applications*, let's call it an `ABI` (Application Binary Interface) you can think of it like a natural language Dictionary, it describes the building blocks of the language and how they're represented using the lowest-level building blocks, how words are pronounced for human words (I'm really stretching this analogy).

So now languages describe themselves using their `ABI`s, so we can be sure of how they represent stuff on the machine. But now we need a way to specifically map a representation from one language to an equivalent representation in another a sort of Translation Dictionary, this is because representations are usually not equivalent for all cases. We need yet another *interface,* this time for calling *foreign functions* (functions written in other languages), let's call it `FFI`. Let's try and rewrite our previous `Rust` code and make sure we abide by `C`'s `ABI`:
```rust
// This is Rust type has the exact same representation as a string in C.
use std::ffi::CString; 

// This tells the Rust compiler to follow C and order
// fields in memory in the order they're defined.
#[repr(C)]
struct Config {
	src_path: CString,
	size_limit: u64,
}

extern "C" fn isWithinLimits(config: &Config) -> bool;
```