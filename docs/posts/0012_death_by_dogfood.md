---
draft: false
date: 2026-04-11
categories:
  - Words
authors:
  - ianwhalen
slug: death-by-dog-food
---

# Death by dog food

I've been slacking on blog posts. So this one is a filler episode at best.

I learned the term [dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) recently and it really resonated.

I'm overloading the term a little bit in this blog but, I hope the story still works.

<!-- more -->

## Augmenting our generations 

The main themes I see out in the wild are (1) people grappling with information retrieval and (2) people grappling with an agent loop where it probably shouldn't be.

Regardless, they're making the computer talk and people are excited. Documents come in, something happens, the outputs are somehow economically useful[^pretend-economist].

At some point a stakeholder who will decide the fate of a product is brought in to test things out (dogfooding). They try it, a thumb goes up or down, and the technical team's life becomes easier or harder respectively. 

Not everyone does this of course, but look up some blogs on RAG systems and usually evaluations are a footnote or not mentioned at all[^blog-examples]. The culture of vibey evaluations is strong.

## Dog food

Of course, this is maybe the _most risky_ way to evaluate a system. If the thumb goes down (the dog food is just meant for dogs after all) due to a fluke in retrieval or answer generation, life gets hard. Stakeholders lose trust, the team backpedals, rehearsed workflows are fallen back on.

I've never liked this "it works when I try it" approach. Especially in a particularly stochastic era of AI.

Dogfooding cannot be the answer for even the simplest RAG system.

## One can't survive on dog food alone

I think I'm missing regular old machine learning more and more these days. Imagine the days before ChatGPT came out. You're building your favorite model[^favorite-model]. You understand your data, clean it up, engineer and select some features, do hyperparameter tuning, and save off your model artifact to model heaven (wherever you'll be deploying it). 

You'd never get away with this! We have no idea what our model's performance is on some "generally agreed upon method for evaluation." In other words, performance on some hold out dataset[^hamel].

In my day to day, I've been focusing more on the least """agentic""" piece of the puzzle: information retrieval. There's a rich history of evaluating these types of systems, and I think there's lots more we can do in the current era of AI. Specifically things that are focused on numbers and facts instead of vibes based evaluations.

I hope to be able to share more on this publicly soon!

[^pretend-economist]: Soon, I'll pretend to be an economist and talk more about this.

[^blog-examples]: Without being a hater, here are some examples [link](https://www.outerport.com/blog/agentic-search), [link](https://en.andros.dev/blog/aa31d744/from-zero-to-a-rag-system-successes-and-failures/), [link](https://blogs.oracle.com/ai-and-datascience/ai-rag-solution), [link](https://www.pinecone.io/learn/retrieval-augmented-generation/). Yes, the focus of these are not evaluations. Just illustrating a point from a few top results in a "blog on rag" Google search.

[^favorite-model]: Let me guess... XGBoost. No! LightGBM. It's CatBoost definitely. Logistic regression who?

[^hamel]: If you're reading my blog, you've probably also seen [this one](https://hamel.dev/blog/posts/revenge/). Similar idea there.