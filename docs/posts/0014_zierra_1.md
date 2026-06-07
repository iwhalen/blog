---
draft: false
date: 2026-06-07
categories:
  - Code
  - Zig
authors:
  - ianwhalen
slug: zierra_1
---

# Zierra part 1

Back with some non-Python content. This is the first installment of my journey to implement Tierra in Zig (Zierra) and to use LLMs for learning.

<!-- more -->

## Tierra

A while ago, I wrote a [post](0008_alife_to_ai.md) about my journey in computing. The short of it is that I started out doing some light artificial life (alife) research. Part of that journey was taking a class on evolutionary computation. One of the first things I learned in the section on alife was about the simulation [Tierra](https://en.wikipedia.org/wiki/Tierra_(computer_simulation)). 

Tierra's general idea is meant to resemble a game called [Core War](https://en.wikipedia.org/wiki/Core_War)[^darwin]. The basic idea is that programmers use a limited machine code instruction set to try to make their opponents execute invalid programs (die). Core War and the idea of competition in a program inspired lots of alife research into the current day. Sakana AI enjoyers will recognize Core War from one of their [recent publications](https://sakana.ai/drq/)[^david-ha].

Enough history though! Tierra is relatively simple. It has three main components: programs (or creatures, organisms, Tierrans, genomes), the CPU that executes the machine code that makes up a program, and the "soup" or block of RAM that the programs are fighting over. The [original paper](https://sfi-edu.s3.amazonaws.com/sfi-edu/production/uploads/sfi-com/dev/uploads/filer/f3/9c/f39c1307-606e-44d8-961a-c16c5c1cfbef/92-08-042.pdf) gives lots of ecological / biological interpretations for each of these. I'll wave them all away as they're not imperative for this blog post. But if you want to have a heady experience, check out the paper and contemplate evolution.

The Tierra simulation loop looks sort of like this:

```
soup = get_soup();

while (we_want_to_keep_going):
  organism = get_next_organism(soup);
  execute(organism);

  while (memory_full_enough):
    reaper();

write_soup(soup);
```

The soup is a block of memory where the organisms exist. In the original paper, this was 60,000 bytes. Organisms are a composition of 32 different machine code instructions each of which fits nicely into one byte[^bytes]. An example organism is:

``` title="Example Tierran"
nop_1  
nop_0  
nop_1  
nop_0  
mov_iab
dec_c  
if_cz  
jmp    
nop_0  
nop_1  
nop_0  
nop_0  
inc_a  
inc_b  
jmp    
nop_0  
nop_1  
nop_0  
nop_1  
shl    
mal    
nop_0  
mov_iab
dec_c  
dec_c  
jmpb   
dec_c  
inc_a  
inc_b  
mov_iab
dec_c  
inc_a  
inc_b  
mov_iab
dec_c  
or1    
if_cz  
ret    
inc_a  
inc_b  
jmpb   
nop_1  
```

A huge hand wave in this pseudocode is `execute(organism)`. This `execute` is doing all the reading from the soup, writing to the soup, and executing of internal logic in a virtual CPU. Most importantly, execution is how organisms self-replicate. Organisms can "divide" and get a new block of memory to write a "daughter" organism. This is an imperfect process which (along with mutations) causes the small modifications we see in real biological systems. Finally comes the selection pressure through the `reaper`. Death comes for Tierrans quicker if execution of certain instructions fail with an error and slower if those executions do not generate an error.

All this when combined creates an analogy for real evolution. Importantly, Tierra has no "fitness function" that assigns organisms "points" based on efficacy on some task. More "fit" individuals are those who can reproduce more effectively. This gives rise to very interesting results like parasitism and organisms that develop immunity to parasites.

## Goals for Zierra

Zierra is my name for "Tierra in Zig." The main goal is to get Zierra working end to end and observe some results shown in the original paper (e.g. parasitism). I also want to get as much of the Tierra algorithm in `comptime` as possible. Additionally, to learn the C ABI, I'll be including a visualizer using [notcurses](https://github.com/dankamongmen/notcurses).

I'm expecting this to take quite a while. To account for this, I want to create incremental posts like this one to track my progress. In my last project, [Rogue-Bench](https://iwhalen.github.io/rogue-bench/), I waited until the very end to post about it. This was a less enjoyable experience as it felt like a "big bang" at the end rather than an incremental project that I could chip away at.

Finally, I'm also not using LLMs to write any Zig code[^zig-and-llms]. This doesn't mean I'm not using them at all though.

## Learning with LLMs

Of course, your favorite coding assistant could easily dump out "Tierra written in Zig" if you point it at the [C implementation](https://github.com/bioerrorlog/Tierra) or give it the paper. Doing this would result in zero learning, but would result in working code. Since my goal is learning, I'll opt for a different route.

My learnings[^learning-zig] on Zig have _mostly_ left my brain. Instead of going through Ziglings again, I opted to have my coding assistant teach me Zig while implementing Zierra. In my local directory, the important pieces for learning are:

```
.
├── AGENTS.md
├── plan
│   └── PLAN.md
├── reference
│   ├── langref.html.in       // Zig `0.16.0` language reference
│   ├── std                   // Zig `0.16.0` standard library
│   ├── tierra_paper.pdf      // For me to read
│   └── tierra_paper.tex      // For an LLM to read
└── ziglings -> ../ziglings/
```

The first important piece is my `AGENTS.md`. This makes specific references to _not write any code_ unless I explicitly say so. For the most part, it obeys this. So, its been entirely on my discipline to not allow my coding assistant to write any Zig. 

Next is the `PLAN.md`. This is where the LLM has done the most heavy lifting[^plan-with-llm]. I've gone back and forth with GPT-5.5 quite a few times to get things in a good spot. It made quite a few mistakes on things like data contracts which required a little more forward thinking. Finally, the plan is split into "phases". These will act as my milestones and likely where I'll pause to write blog posts about progress.

The `reference` folder contains information about the Zig language that my assistant hasn't necessarily been trained on. After all, `0.16.0` is relatively new. The directory also contains the Tierra paper itself for me and the LLM to read. When interacting with this folder, the LLM is more or less searching for me and answering questions. 

I also have a symlink to my completed Ziglings project. In my `AGENTS.md`, I ask the LLM point to Ziglings exercises when appropriate. So if I forget how to write a struct (or something) it will point me to the exercises on structs.

Finally, I've also been using the LLM for quality control. I write all the code and unit tests. But then I also ask for a review to understand what else I might be missing. This has been nice as well to pick up on low level programming ideas that have gone stale in my brain.

## Progress 

Now down to brass tacks. What have I actually done? For the full implementations, see the [repo](https://github.com/iwhalen/zierra).

I'm sure most of this will have to change as I get into the nitty gritty of the project. But this is the first pass.

### Instructions

The first step was of course to list out all the instructions as an enum. 

``` zig
pub const Instruction = enum(u8) {
    nop_0, // No operation
    nop_1, // No operation
    or1, // Flip low order bit of cx, cx ^= 1
    shl, // Shift cx register left, cx <<= 1
    zero, // Set cx register to zero, cx = 0
    if_cz, // If cx == 0, execute next instruction
    sub_ab, // Subtraction, cx = ax - bx
    sub_ac, // Subtraction, ax = ax - cx
    inc_a, // Increment ax, ax++
    inc_b, // Increment bx, bx++
    dec_c, // Decrement cx, cx--
    inc_c, // Increment cx, cx++
    push_ax, // Push ax on stack
    push_bx, // Push bx on stack
    push_cx, // Push cx on stack
    push_dx, // Push dx on stack
    pop_ax, // Pop top of stack into ax
    pop_bx, // Pop top of stack into bx
    pop_cx, // Pop top of stack into cx
    pop_dx, // Pop top of stack into dx
    jmp, // Move IP forward to template
    jmpb, // Move IP backward to template
    call, // Call a procedure
    ret, // Return from procedure
    mov_cd, // Move cx to dx, dx = cx
    mov_ab, // Move ax to bx, bx = ax
    mov_iab, // Move instruction at address in bx to address in ax
    adr, // Address of nearest template to ax
    adrb, // Search backward for template
    adrf, // Search forward for template
    mal, // Allocate memory for daughter cell
    divide, // Cell division
};
```

Nothing really crazy happening here. I just know I'll need to write a huge `switch` statement down the line somewhere and it will use this `enum`.

### Soup

Next was writing the soup. Since they're so fancy, I wanted to use the "module-level" structs Zig allows like this:

``` zig
pub const Soup = @This();

memory: [size]?Instruction = .{null} ** size,

// ... more soup stuff...
```

This immediately had a wrench thrown in it since I need to know `size` at compile time. The only way I could figure out was to instead do:

``` zig
pub fn Soup(comptime size: u16) type {
    // ... error checking...

    return struct {
        const Self = @This();

        memory: [size]?Instruction = .{null} ** size,

        // ... more soup stuff...
    };
```

Which doesn't seem too crazy since there are examples ([e.g.](https://ziglang.org/documentation/0.16.0/#Examples)) that do this sort of thing in the language reference. 

There's nothing out of the ordinary happening in the soup. It handles ownership of blocks of memory and allocating / deallocating slices. The most interesting part was a classic HackerRank style algorithm that finds the first unallocated block of memory of a certain size

``` zig
        pub fn allocate(self: *Self, request_size: u16, requester: CreatureId) SoupError!Allocation {
            var run_start: u16 = 0;
            var run_len: u16 = 0;

            // Find a contiguous "run" of unallocated memory.
            for (self.owner, 0..) |owner, i| {
                if (owner == null) {
                    if (run_len == 0) {
                        run_start = @intCast(i);
                    }

                    if (run_len == request_size) {

                        // Assign memory in the allocation range.
                        for (run_start..run_start + request_size) |j| {
                            self.owner[j] = requester;
                        }

                        return Allocation{ .start = run_start, .len = request_size };
                    }
                    run_len += 1;
                } else {
                    run_len = 0;
                }
            }
            return SoupError.NoFreeSoup;
        }
```

Ground breaking stuff!

Of course, I also wrote a bunch of unit tests. I find testing in Zig to be very nice overall. I struggled a bit with the `testing.expectEqualSlices` syntax at first. Other than that it was smooth sailing here and in the CPU tests.

In the original paper, the soup is a circle. At least for allocations, I decided to skip implementing this as it would only result in at most a single additional individual being stored in the soup. Maybe this will come back to haunt me... who knows.

### CPU

Next I did the virtual CPU class. This was a start and is by no means anywhere near done:

``` zig 
pub fn CPU(comptime stack_depth: u16) type {
    // ... error checking...

    return struct {
        const Self = @This();

        // Address registers.
        ax: u16 = 0x000,
        bx: u16 = 0x000,

        // Numerical registers.
        cx: u16 = 0x000,
        dx: u16 = 0x000,

        // Flags
        fl: u8 = 0x00,

        // Stack pointer
        sp: u8 = 0x00,

        stack: [stack_depth]?u16 = .{null} ** stack_depth,

        // Instruction pointer
        ip: u16 = 0x000,

        pub fn push(self: *Self, value: u16) StackError!void { // ...}

        pub fn pop(self: *Self) StackError!?u16 { // ... }
    };
}
```

Right now, the stack works and registers get initialized. But that's it!

The main learning here was just a friendly reminder on the absolute basics of low level computing. CPUs are truly magic and revisiting this kind of information renews my respect for early computer scientists that had to figure all this stuff out.

### Config

Finally, I had to dive into the `.zon` file type to store all my configs:

``` zon
.{
    // The size (in bytes) of the "soup" where evolution takes place.
    .soup_size = 60000,
    // Per-creature stack depth.
    .stack_depth = 10,
    // Maximum distance for template searches.
    .search_limit = 500,
    // `true` to enable linage tracing through evolution.
    .lineage_enabled = true,
    // `true` to enable soup visualizer.
    .display_enabled = true,
    // Start reaping when memory soup above this ratio.
    .reaper_threshold = 0.8,
    // Number of instructions between "cosmic ray" mutations (default 10,000).
    .cosmic_rate = 10000,
    // Number of instructions between "copy" mutations (usually 1,000 - 2,500).
    .copy_error_rate = 1000,
    // Flaw rate multiplier (this number x copy_error_rate).
    .flaw_rate = 1.5,
    // Exponent for time-slice calculation.
    .slicer_power = 1.0,
    // How often to output population metadata
    .snapshot_interval = 100000,
    // Directory to output population metadata.
    .output_dir = "output",
    // Random seed.
    .rng_seed = 8675309,
}
```

At first, I questioned a custom file format for configs like this. But I really like it. One just imports the file and gets access to all this information. Better yet, it is compile time available! 

## Conclusion

That's it for phase one of the _master plan_. Up next, I'll be implementing all 32 instruction handlers in the CPU! Stay tuned...

[^darwin]: Apparently, Core War is not the first of its kind. It seems that the earliest competitive programming game was [Darwin](https://en.wikipedia.org/wiki/Darwin_(programming_game)). The concept was basically the same but, the input was probably written on tapes or punch cards. 

[^david-ha]: I can even remember people discussing [David Ha](https://scholar.google.com/citations?hl=en&user=N7X-kbUAAAAJ)'s work while I was first learning about evolutionary computation. Sometimes I feel like I missed the boat and should have kept down the path of combining evolution and neural networks. Alas...

[^bytes]: Of course, 32 different instructions would fit into 5 bits. But the original paper uses 8 bits and, to make the math work out the same, I do here as well.

[^zig-and-llms]: The Zig development [code of conduct](https://ziglang.org/code-of-conduct/#strict-no-llm-no-ai-policy) is well known for banning LLMs / AI assistance. The VP of Community at the Zig Software Foundation, Loris Cro, has written about this in two different contexts: [contributor poker](https://kristoff.it/blog/contributor-poker-and-ai/) and their distributed community meetup [Zig Days](https://kristoff.it/blog/llms-at-zig-days/). In the first case, the ban seems to be more about building a healthy community of contributors who _really_ know Zig. In the second, it also seems to be more about focusing on learning Zig and community building. There's probably a blog post worth of discussion analyzing these two posts. But, the general theme seems to be less that "AI bad" and more "people good". Which is heartening. In a [recent interview](https://youtu.be/iqddnwKF8HQ?si=2ffFa9G7zpDHHKnv&t=1595), Andrew Kelley himself explains a little more on the policy, echoing Loris's major themes and adding a point on Zig being about learning. I hope Mr. Kelley would forgive my transgressions in using LLMs. Even in the limited way I do for this project. There's only so much time in the day for learning, and I have found that the workflow described here is useful and I do feel I am getting better.

[^learning-zig]: See my previous posts [here](./0007_learning_zig_1.md) and [here](./0011_learning_zig_2.md). 

[^plan-with-llm]: Arguably, this was the worst place to use LLMs. The overall architecting of Zierra is pretty important for learning Zig. However, my interests were a little more mechanical. As in, I wanted to learn how to literally write Zig code well. In my next project (if it ever comes), I'll probably limit coding assistance even more to get this more architectural piece too. 