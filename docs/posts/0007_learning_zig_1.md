---
draft: false
date: 2026-01-04
categories:
  - Code
  - Zig
authors:
  - ianwhalen
slug: learning-zig-1
---

# Learning Zig part 1

As promised in my [project goals for 2026](./0006_projects_for_2026.md), I'm trying to learn Zig.

The first step here has been going through the [_Introduction to Zig_](https://pedropark99.github.io/zig-book/) book.

I paused my (slow) progress to reflect on the first 4 chapters of the book and Zig as a language.

<!-- more -->

## Reflections on Zig

Overall, I am happy to be using a compiled, low-level language again. 

My first language was Visual Basic (classic). Thankfully, time has wiped this from my memory.

In undergrad, languages I remember are:

- Python: the first language I learned (for real).
- C++11: the first compiled language I learned for real and the language OOP was taught in.
- PHP: the language used in my student programming job (specifically, the Laravel framework).
- SPARC assembly: for my lowest level programming class, we learned this relic of an instruction set.
- SQL: for all things data across all courses.

In grad school, this boiled down to almost exclusively Python, save a [small project](https://github.com/iwhalen/symbolic_regression) in C++ for a parallel computing course.

For the past 7+ years at work, I have exclusively used Python and (barely) SQL. So, there is a compiled language-sized hole in my heart.

### General comments

I love `zig init [-m]`. Knowing that most projects will all look the same due to this is great. It makes me feel the same way that `uv init` does in Python. It removes the need to make a decision on how my project will be laid out.

The build system is intuitive and I also really like that it's all just batteries included. As the book mentions, lots of languages have whole ecosystems of tools to enable compiling an executable. Running a quick `zig run src/main.zig` or `zig test src/mycode.zig` feels really nice.

Certainly I have not even scratched the surface of how building works yet. But, the new user experience has been great.

Speaking of testing, I also have enjoyed Zig's built-in testing library. For some reason I remember unit tests in C++ being a _hassle_ last time I tried. This was almost a decade ago though.

I feel like I am struggling with allocators still. I generally understand what they're doing any why. 

However, I have only ever done manual memory management (or not thought about memory at all in Python). So, I'm still getting my head around them. I'm sure this will come with time.

### Struct strangeness

The main gripe I have at the moment is with struct definitions. They seem stuck in limbo between just being a C struct like I remember and being a class.

We can define a struct with methods like this:

``` zig
const Point = struct {
  x: f64,
  y: f64,

  pub fn print(self: Point) void {
    std.debug.print("Point(x={d:.4}), y={d:.4})", .{ self.x, self.y });
  }
}
```

Which is fine. 

However, I was struck with the tension between "no OOP" and "ok, some OOP" when learning about allocators.

In _Introduction to Zig_, the author says, 

> ... every allocator object is built on top of the `Allocator` interface in Zig. This means that, every allocator object you find in Zig must have the methods `alloc()`, `create()`, `free()` and `destroy()`.

In Chapter 2, there was plenty of discussion on how (paraphrasing) "Zig doesn't really do OOP like you expect in Java or C++." After reading this passage, it made it seem like Zig doesn't do OOP, except interfaces. Which is fine! Abstract classes get messy quickly. 

So, I was expecting an "interfaces only" approach to OOP. What I got was even worse.

In Zig `0.15.2`, we have the "standard memory allocation interface" in the `Allocator` struct:

``` zig
// ... code omitted ...

const Allocator = @This();

ptr: *anyopaque,
vtable: *const VTable,

pub const VTable = struct {
    alloc: *const fn (*anyopaque, len: usize, alignment: Alignment, ret_addr: usize) ?[*]u8,
    resize: *const fn (*anyopaque, memory: []u8, alignment: Alignment, new_len: usize, ret_addr: usize) bool,
    remap: *const fn (*anyopaque, memory: []u8, alignment: Alignment, new_len: usize, ret_addr: usize) ?[*]u8,
    free: *const fn (*anyopaque, memory: []u8, alignment: Alignment, ret_addr: usize) void,
};

// ... code omitted ...
```

See [here](https://codeberg.org/ziglang/zig/src/commit/e4cbd752c8c05f131051f8c873cff7823177d7d3/lib/std/mem/Allocator.zig) for the full code.

For the uninitiated, `VTable` is a reference to a [virtual method table](https://en.wikipedia.org/wiki/Virtual_method_table). This is (among other things) how C++ accomplishes fun OOP things like dynamic dispatch and inheritance.

This just strikes me as confusing since we're not supposed to have OOP in the traditional sense. Unless you do it yourself?

And even then, I assume it will be at runtime when my user finds out that they forgot to implement a given method in my `VTable`?

Would life be any rosier if we had a special `interface` construct that checks method implementation at compile time (and doesn't do inheritance)?

First idea off the hip, brainless implementation from me:

``` zig

const Allocator = interface {
    pub fn alloc(self: Allocator, len: usize, alignment: Alignment, ret_addr: usize) ?[*]u8;
    pub fn resize(self: Allocator, memory: []u8, alignment: Alignment, new_len: usize, ret_addr: usize) bool;
    pub fn remap(self: Allocator, memory: []u8, alignment: Alignment, new_len: usize, ret_addr: usize) ?[*]u8;
    pub fn free(self: Allocator, memory: []u8, alignment: Alignment, ret_addr: usize) void;
};
```

No more function pointers, no more `VTable` smoke and mirrors, no more `anyopaque` pointer-to-who-knows-what.

The Zig creators are smarter than me though. So, there's a good reason for not having this.

Searching around [Ziggit](https://ziggit.dev/search?q=interface), I'm (of course) not the first person to think about this.

So maybe this is more of a gripe on _Introduction to Zig_ than the language itself. My first impression was "no OOP".

## Reflections on the book

First off, writing a full-length book is a Herculean feat. Pedro is certainly eloquent and has done a lot for the community by publishing this book for free.

People learn in different ways. I'm just reflecting on things that would help me learn better.

Overall, the book is good and should be used.

### Check your understanding

Maybe a personal failing, but I found myself becoming quite passive while reading.

Often in textbooks, you'll find exercises or "check your understanding" subsections sprinkled throughout a chapter. I think this keeps readers engaged and reinforces key concepts on the learning journey.

For example, in the [section on structs](https://pedropark99.github.io/zig-book/Chapters/03-structs.html#sec-structs-and-oop), the reader could easily be asked to implement the entire `Vec3` struct before continuing. One could even put the answers behind a details tag so readers don't accidentally see them.

Just a preference thing, I guess.

### Convention confusion

A piece I would love more context on is convention. 

I want to know the answer to the question "is this how the author would implement this, or is this how the majority of the Zig community would implement this?".

For example, tests living alongside source code. Lots of examples in the book have them living together, but big projects break these up into separate directories (e.g. [bun](https://github.com/oven-sh/bun)). Maybe this is an obvious one, but some commentary on convention would be nice.

## Moving forward

I think, before I jump back into the book, I'll try out some [Ziglings](https://ziglings.org). Even the _Introduction to Zig_ book recommends doing them in addition to the book. This will probably satisfy the feeling that I'm following along too much and not doing things enough.

Then, after finishing off the book, comes the part where I try to make good on some [project goals](./0006_projects_for_2026.md).
