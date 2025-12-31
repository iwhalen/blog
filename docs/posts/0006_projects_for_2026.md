---
draft: false
date: 2025-12-31
categories:
  - Words
authors:
  - ianwhalen
slug: projects-for-2026
---

# Project ideas for 2026

This is a collection of ideas for side projects.

I'm trying to learn Zig. So, there will be some projects appropriate for that. 

But I'll also put in some ideas for Python-appropriate projects (my usual language).

<!-- more -->

## Zig projects

These have varying levels of appropriateness for Zig. 

### Existing Zig guides

I'm already working my way through the [Zig book](https://pedropark99.github.io/zig-book/). There are four projects in there that I'll work my way through eventually. 

On top of this, I will also most likely do some of the [Ziglings](https://codeberg.org/ziglings/exercises/) exercises.

If all of these don't satisfy my "starters" for Zig, I could also look at cloning a command line tool like `grep` or dive into blog posts like [this one](https://austinhenley.com/blog/challengingalgorithms.html).

### Symbolic regression

In grad school, I implemented a tree-based genetic programming approach to symbolic regression for a final course project. See [here](https://github.com/iwhalen/symbolic_regression).

The purpose of that course was to demonstrate different types of parallelism (strong scaling and weak scaling).

Re-implementing in Zig would focus more on making one approach work well rather than comparing scaling approaches.

### TUI game

Zig already has a TUI library being developed: [libvaxis](https://github.com/rockorager/libvaxis). The first step of this will be deciding if I want to use it. The answer is probably yes. A TUI itself would be its whole own project.

The only reason to implement my own version would be if the library does not have the features I want. Even then, using it as a starting point would probably be better than starting from scratch.

A (potentially) simple idea would be to implement a text-based version of something like a tower defense or base defender game.

If this proves too difficult, I could just fall back on a text based adventure game or just do Conway's Game of Life.

### Linear program solver

I am by no means an operations research PhD. Not even close. But, writing a small module for linear-only programs would be fun.

There already exist a ton of implementations in other languages I could draw from if I am stuck.

The goal here would be to see how far I could get just looking at the math of the simplex method.

### Vector database with Python bindings

Adding another log to the fire. Implementing a vector database will be interesting. Properly coding up the indexing strategy will be a challenge.

The real fun here will be making it usable from Python through the [ziggy-pydust](https://github.com/spiraldb/ziggy-pydust) framework.

## Python projects

### Kedro proofs of concept

Over the years of using Kedro and discussing with the team, I've had lots of ideas on how to extend the library.

These are some of them.

#### Conditional node running

For a very long time, people have wanted to run nodes conditionally. See [here](https://github.com/kedro-org/kedro/discussions/1181) for a discussion.

For most use cases, this breaks Kedro's principles. The best way to accomplish it would be conditionally generating pipelines and then running those.

But, it would still be interesting to:

1. Figure out a reasonable use case for conditional execution that isn't solved by datasets or pipeline generation
2. Determine the best way to implement conditional execution

#### Node filtering in run command

I can't find the discussion, but basically the idea here is filtering nodes through a command line syntax like regex. 

When a project gets really big and you're hammering namespaced pipelines, it often becomes cumbersome to run a specific set of nodes or pipelines.

For example, imagine I'm creating pipelines based on some product of sets like:

``` python
from itertools import product

regions = ["us-east", "us-west", "eu-east", "eu-west"]
models = ["logistic-regression", "random-forest", "xgboost"]

# ...generate pipelines with product(regions, models)...
```

So for a given node, we may have two layers of namespaces, e.g. `us-east.logistic-regression.preprocess`.

How could I easily run all the nodes for training `us`-based models? Or only rerun the preprocessing for `xgboost`-models?

If I remember correctly, someone suggested a similar syntax to dbt's node selection. See [here](https://docs.getdbt.com/reference/node-selection/syntax).

So, if I wanted to run a subset of nodes, I could do something like:

``` bash
kedro run --select-nodes "us-*.logistic-regression.*"
```

To run all of the nodes relating to `us`-based logistic regression (e.g., preprocessing, training, reporting, etc.)

Then we could do the same thing with excluding nodes:

``` bash
kedro run --exclude-nodes "eu-*"
```

This would exclude all nodes prefixed with `"eu-"`.

#### Python-first configuration

A common point of contention is Kedro's yaml-first approach to its data catalog and parameters configuration. There are some good reasons to keep it like this. Allowing Python in configuration lets users introduce runtime dependencies and that breaks Kedro's whole "it runs the same way, every time" ethos.

But, allowing configuration as code lets people use things like `pydantic-settings` and inheritance to remove redundancy in large projects.

The initial PoC here is just making it work with OmegaConf (or writing a `pydantic`-based configuration reader).

A possible extension could be making sure users don't do bad things in their configs (e.g. only allow a class definition).

### Extend AlphaEvolve

I love algorithms like [AlphaEvolve](https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/) and Sakana AI's [ShinkaEvolve](https://sakana.ai/shinka-evolve/).

As I've said in the past, artificial evolution is what got me into computing and AI. It's a backward route, but I wouldn't trade it!

The main thing stopping me from doing a project building on (or using) these methods is cost. I have tried to set up a model locally, but my GPU chokes and dies on anything interesting.

Still, this kind of project excites me.

### Investigating Google's Lighthouse tool

For work, I used the [Lighthouse](https://github.com/GoogleChrome/lighthouse) CLI for some SEO related junk. Actual use case aside, I found an interesting result that one of the Lighthouse scores has no correlation with SERP rank.

I thought this was very surprising given that Google offers this tool as a way to improve SERP rank. Sort of. Explicitly, they advertise it as:

> Lighthouse analyzes web apps and web pages, collecting modern performance metrics and insights on developer best practices.

But the things they check are also advertised as best practices in their developer guide. So one would assume if I have a high lighthouse score, I would also have a high SERP result.

So, my questions are (on a set of diverse queries):

- Which metrics correlate well with SERP rank?
- Do other search engines (e.g., DuckDuckGo) respect Lighthouse ranks more than Google?
- Am I just loading the pages incorrectly and getting bad readings?

### Transformers for recommendations

This is a two part idea. First, I'd like to write an Ibis-based pipeline for parsing the [2023 Amazon reviews dataset](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) into an easy to use format for model training.

Then, I'd like to try out a method like [HSTU](https://arxiv.org/pdf/2402.17152) on this data. I'd be surprised if my GPU can handle a small generative recommender. But the data processing part is the real work here.

## Conclusion

I would be happy if I actually completed 2 or 3 of these. This full list is definitely aspirational.

There will also be some non-technical things to write about. Those will be fun little pieces about whatever comes to mind. 

Finally, I have also been kicking around the idea of learning game dev in Godot. But, that seems the most involved of all since I'll also have to learn art.
