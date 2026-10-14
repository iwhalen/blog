---
draft: false
date: 2026-10-14
categories:
  - Code
  - Python
authors:
  - ianwhalen
slug: relevance-bench
---

# Relevance-bench

This is a follow up to my [post](./0018_jev_pseudo_relevance.md) on using Jev for pseudo-relevance judgments on TREC DL 2023.

This experiment felt more useful as a little benchmark that I can extend.

Check out the code for this post [here](https://github.com/iwhalen/relevance-bench). For the data used, see my dataset on Hugging Face [here](https://huggingface.co/datasets/iwhalen/trec-dl-2023-judged).

<!-- more -->

## Recap

Last week I posted about using Jev as a judge for pseudo-relevance.

The takeaways from [that work](./0018_jev_pseudo_relevance.md) were:

- Jev is more efficient than GPT-6 luna with no reasoning effort.
- GPT-6 luna is better at matching human judgments when measured through Cohen's kappa.
- There is no difference between the approaches when considering ranking retrieval systems (measured with Kendall's tau).

Overall, Jev was the better choice for pseudo-relevance. In most cases, such an approach would be used to evaluate a retrieval system. 

For this post, I wanted to add a couple new models in the mix to see what comes out.

## A small benchmark

The code used in my original post was pretty bad. It would require a lot of duplicate code to add a new model provider. Also, the CLI was a little clunky.

To improve this a bit, I standardized the provider API a little bit to make extensions easier. To make the CLI nicer, I used good ol' [Typer](https://typer.tiangolo.com/).

There's not much more to say about this. The code is [here](https://github.com/iwhalen/relevance-bench) if you'd like to see.

As far as development goes, I typed out the initial pattern I wanted by hand and had Codex fill it in. All the results are the same and outputs seem correct. So, I was fine to let this one be a little bit vibe coded.

## Decisions, decisions

Very interestingly, OpenAI released the decisions API as a response to Jev[^jev-likes].

Given OpenAI's comms and the pricing around the model, it seems that their decision model is GPT-6 luna with the generative piece swapped out for a classifier.

Jumping into some of the other model providers, [Microsoft's Decision-1](https://commandline.microsoft.com/microsoft-decision-1-model-foundry/) model caught my eye as well. From their benchmarks, it seems faster and better than Jev! I also had my eyes on [Perplexity's Decider v1.1](https://huggingface.co/perplexity-ai/pplx-decider-v1.1-27b). 

OpenRouter has even released their own [`/decisions`](https://openrouter.ai/docs/api/api-reference/alphadecisions/submit-a-decisions-request) endpoint. Given that these models are both available there, I added them to the evaluation.

I think the explosion of these types of models is quite interesting. On one hand, it demonstrates a clear market fit. Lots of people were using beefy LLMs for tasks like metadata tagging, structured extraction, and simple classification. By no means does one need a frontier, flag ship model to do this type of task. Decision models are cheap and good enough at this so they can easily scale to the data sizes such tasks are often carried out on.

On the other hand, this type of model is great for smaller labs to demonstrate competency. There's no need to spend millions on a pre-training run. Just take your favorite open source, instruction-tuned language model, create a dataset, replace the next-token predictor head with a decision head, train the decision head and a LoRA adapter on your dataset, and (optionally) calibrate the output probabilities[^releasing-jev].

Enough talk! Let's look at some results.

## Results

Here is a summary of the agreement metrics:

| Provider | Model | Judgment coverage | nDCG@10 tau-b | Tau CI (95%) | Pooled Cohen’s kappa | Kappa CI (95%) |
| --- | --- | --- | --- | --- | --- | --- |
| jev | jev-latest | 22327/22327 | 0.9352 | [0.8545, 0.9453] | 0.1907 | [0.1539, 0.2279] |
| openrouter | microsoft/microsoft-decision-1 | partial (22256/22327) | 0.9429 | [0.8780, 0.9529] | **0.2696** | [0.2275, 0.3106] |
| openai | gpt-6-luna | 22327/22327 | 0.9328 | [0.8655, 0.9496] | 0.2391 | [0.2000, 0.2767] |
| openai-decisions | gpt-6-luna | partial (22324/22327) | 0.9294 | [0.8622, 0.9462] | 0.1775 | [0.1427, 0.2124] |
| openrouter | perplexity/pplx-decider-v1.1-27b | partial (22320/22327) | **0.9453** | [0.8780, 0.9554] | 0.2419 | [0.2015, 0.2825] |

And again the efficiency metrics:

| Provider | Model | Judgment coverage | Estimated total cost (USD) | P50 latency (seconds) |
| --- | --- | --- | --- | --- |
| jev | jev-latest | 22327/22327 | 0.678263 | **0.1357** |
| openrouter | microsoft/microsoft-decision-1 | partial (22256/22327) | 0.338348 | 0.2578 |
| openai | gpt-6-luna | 22327/22327 | 1.328348 | 1.7142 |
| openai-decisions | gpt-6-luna | partial (22324/22327) | 1.236627 | 0.1578 |
| openrouter | perplexity/pplx-decider-v1.1-27b | partial (22320/22327) | **0.246585** | 0.2602 |

For partial results:

- OpenAI's decision API had an interesting refusal (see below for more).
- Decision-1 and Decider v1.1 had a few examples that were too long to be judged at my fixed truncation length[^truncation].

Let's discuss.

### Interchangeable rankers

Similarly to my previous results, there's not really a significant difference between the Kendall's tau values across the board. Meaning, if you wanted to use decision models for comparing information retrieval systems, any of these models are appropriate.

This result is also consistent with [academic findings](https://arxiv.org/pdf/2502.13908) on relevance judges. Most have quite high Kendall's tau, regardless of Cohen's kappa.

### Human agreement

Now the more interesting part. We seem to have a significant winner for Cohen's kappa. Namely, Decision-1.

With some work on the prompts, I suspect this model would surpass all the approaches used in our [reference](https://arxiv.org/pdf/2502.13908).

That said, this model was also produced by Microsoft. The same Microsoft whose researchers put together MS MARCO dataset which is used in TREC competitions. I would wager that some TREC-style relevance grading tasks made it into this model's training data[^ms-marco].

### M-O-O-N that spells luna

When OpenAI announced that the underlying model for the decisions API was GPT-6 luna, I was a little suspicious. I was expecting a custom, fine-tuned model. Or at least a version of luna that was tuned for the decision generation task.

However, two pieces of evidence from these results point to the conclusion that this is the same old luna in both cases.

- Cohen's kappa shows that decisions luna isn't as good at generating decisions compared to its generative brother[^luna]. A drop of ~0.06 points of performance is quite large.
- Efficiency metrics are almost identical for cost, but latency is much lower in the decisions API.

This is just a hypothesis, of course. However, after seeing this, I think it may be worthwhile to hold off on using OpenAI's approach to decisions for now. Especially when so many other, better models are easily accessible.

## Refusal of service

One interesting side note is GPT-6 luna's refusal to decide on a particular question. In total, three (query, passage) pairs were refused.

As it turns out, these were all the same passage[^dupes].

Query:

> what causes eye blackouts

Passage:

> Aim for the temple. The temple is the area on the side of a person's face, between the hairline and the eyebrows, at eye level. If the temple is accurately targeted, the punch will cause the brain to swing violently against the skull lining, causing a blackout.

Violent indeed!

The strange part here is that the responses API did not refuse this and the decisions API did. Very odd...

## Conclusion

I may update this from time to time if a new, interesting decision model comes out. For the latest, be sure to check out the [repo](https://github.com/iwhalen/relevance-bench).

Thanks for reading!

[^jev-likes]: In response to Jev _and_ all the Jev-likes that are popping up. Too many to list! But, you can see some people who tried [here](https://huggingface.co/blog/a2aprotocol/openai-decisions-api-vs-jev-a-practical-guide) and [here](https://www.forbes.com/sites/ronschmelzer/2026/10/12/decisions-decisions-a-guide-to-the-new-wave-of-ai-decision-models-besides-jev/). Also, check out [this](https://huggingface.co/spaces/multimodalart/jev-decision-index) leaderboard on Hugging Face.

[^releasing-jev]: For more, see [Releasing the Inner Jev: A Simple Recipe for Open Decision Models](https://arxiv.org/pdf/2610.17205). I'm no expert here, but this is the basic outline. Of course, this is a SFT approach. Jev claimed to use [RLCD](https://arxiv.org/pdf/2307.12950) but I feel like that might be a little bit of marketing. 

[^truncation]: Rather than regenerate all my results with a variable, model-specific truncation, I decided to use the same one for everything. I think this original choice was to make Jev work. Now everyone gets the same.

[^luna]: With the caveat, of course, that I did no tuning for the prompts going into the decision API.

[^ms-marco]: For more, see [MS MARCO: A Human Generated MAchine Reading COmprehension Dataset](https://arxiv.org/pdf/1611.09268). Nick Craswell, Daniel Campos, and Bhaskar Mitra are also authors on the [Overview of the TREC 2023 deep learning track](https://arxiv.org/abs/2507.08890). We don't know exactly who worked on Decision-1. But, given Microsoft's history with information retrieval, it is not out of the realm of possibility to say that they trained on this task.

[^dupes]: I am using the TREC DL 2023 data that contains duplicates.