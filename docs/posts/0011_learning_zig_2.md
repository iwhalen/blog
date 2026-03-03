---
draft: false
date: 2026-03-02
categories:
  - Code
  - Zig
authors:
  - ianwhalen
slug: learning-zig-2
---

# Learning Zig part 2

Here is my second entry in my "learning Zig" series. Likely the final.

I feel I have seen enough syntax and am ready to build something.

For my first post on this topic, see [here](./0007_learning_zig_1.md).

<!-- more -->

## Ziglings good

[Ziglings](https://codeberg.org/ziglings/exercises) was a joy to go through. This was exactly what I was looking for when I was originally just following along through the [_Introduction to Zig_](https://pedropark99.github.io/zig-book/) book. 

I know this idea came from Rust. I hope other language communities follow suit and introduce high quality *lings of their own. Maybe these already exist and I'm unaware!

Of course, I do have a couple gripes.

The "quiz 8 trench" of exercises between quiz 8 and quiz 9. Excluding the defunct async exercises, there were 26 total exercises in the trench. This felt like a lot. Having regular quizzes as checkpoints make it feel easier to set goals and move between sections.

The quality of Zigling exercises also seems to decline after the [string formatting](https://codeberg.org/ziglings/exercises/src/branch/main/exercises/099_formatting.zig) exercise. The final quiz also felt like it belonged after the bit manipulation section rather than at the end of the whole thing.

Regardless, I learned a lot, loved the format, and enjoyed the humor. More power to Ziglings.

## Revisiting OOP

In part 1 of this series, I made some comments on being a little confused with structs pretending to interfaces. I continue this object oriented confusion here.

[Exercise 55](https://codeberg.org/ziglings/exercises/src/branch/main/exercises/055_unions.zig) in Ziglings introduces unions. This exercise refers to them as a sort of "primitive polymorphism".

I guess I haven't done enough systems programming to know why these are useful. Because it seems like Zig can sort of do OOP but doesn't let scrubs have it. 

You must roll with the cool kids, implement your own VTable, and toss in a union to get similar functionality.

My confusion continues with [Exercise 92](https://codeberg.org/ziglings/exercises/src/branch/main/exercises/092_interfaces.zig) on interfaces (again). 

``` zig
const Insect = union(enum) {
    ant: Ant,
    bee: Bee,
    grasshopper: Grasshopper,

    pub fn print(self: Insect) void {
        switch (self) {
            inline else => |case| return case.print(),
        }
    }
};

pub fn main() !void {
    const my_insects = [_]Insect{
        Insect{ .ant = Ant{ .still_alive = true } },
        Insect{ .bee = Bee{ .flowers_visited = 17 } },
        Insect{ .grasshopper = Grasshopper{ .distance_hopped = 32 } },
    };

    std.debug.print("Daily Insect Report:\n", .{});
    for (my_insects) |insect| {
        // Almost done! We want to print() each insect with a
        // single method call here.
        insect.print();
    }
}
```

The lesson goes on to say that the `Insect` union here is basically an abstract data type. 

We basically have everything we need for OOP. The only hurdle is stuffing in every "class" that "extends" the `Insect` "base class". It's just down to how bad the user wants it!

This isn't a dig at Zig. I don't even really know what I'm talking about here. I'm still excited to use Zig and implement projects. I'm just a little lost in how these things are presented.

This all feels like some tongue-in-cheek, elbow nudge, inside joke of "this is OOP, _not_."

## Current state of parallelism

The next piece that I went down a few rabbit holes on was the state of parallelism in Zig.

Given that Zig is an evolving language, the information on this topic is quite hard to digest. Searching "parallelism in Zig" is a nightmare.

Ziglings currently has some vestigial exercises on async functions. As I understand it, these were removed in `0.15.2`. Which is fine.

But then later on there's a quaint two exercises using threads! Just what I was looking for.

Then, on further investigation of the standard library, there's also a [`process`](https://ziglang.org/documentation/0.15.2/std/#std.process) namespace.

Until I saw the threading exercise I was under the impression I would have to use some external library to get any parallelism at all. Which, in retrospect, is insane to assume.

Maybe this is more of a comment on the state of information dissemination for the Ziglang.

## Up next

The next step on my Zig journey is a mix of a few of the projects I mused about in [my 2026 project goals](./0006_projects_for_2026.md). 

I originally stated that I wanted to try reimplementing a symbolic regression project from my grad school days. However, I thought more and this seemed a little trite. I would prefer something I haven't done.

Instead, I'll be implementing the _classic_ artificial life simulation [Tierra](https://en.wikipedia.org/wiki/Tierra_(computer_simulation)). 

I also wanted to approach this in a different way. I just got done vibe coding a [big project](./0010_rogomatic_llm.md) over the weekend. This was fun and accomplished my goal. However, it left me feeling a little disconnected from my implementation. I wish I knew the intricacies better. This project was Python though, which I know quite well. So, if motivated, I could easily figure it out.

If I fired up the vibe coding machine for a Zig project, I would be completely lost.

This time, I'm going to take a new approach. The goal is:

- Define the overall plan and phases of implementation with Opus 4.6.
- For each phase, ask the model to give me tasks and starting points to fill in. Kind of like how Ziglings does. But, much less boilerplate.

This will be _much_ slower. But my goal here is learning. So, my coding assistant will take a teaching role rather than an engineering role.

Thanks for reading!
