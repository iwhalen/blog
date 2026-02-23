---
draft: false
date: 2026-02-23
categories:
  - Words
authors:
  - ianwhalen
slug: sqa-as-ai-enablement
---

# Software quality assurance as AI enablement

I recently gave this idea a whirl to good effect. Instead of saying "we should use static type analysis", we now can say "my coding agent needs to use static analysis."

> _Set up your dream codebase with this one simple trick. Product managers hate him!_

<!-- more -->

## Software quality assurance 

I do Python at work. I really enjoy a codebase that has lots of bells and whistles to keep me from shooting myself (and my fellow developers) in the foot.

Bells and whistles like:

- `ruff` for linting and formatting
- `ty` for static type checking
- Tools like `bandit` for security checks
- A reliable unit test suite
- Integration tests that reflect real usage
- The list goes on!

Automations like this remove the need to choose conventions and catch bugs before users do.

However, none of these automations are things users see, which can cause conflict when developing in a _business_. 

## Corporate software development realities

SQA tooling is something that most technology people would agree should be done. This is well trod ground, people have come up with all the good ideas for shipping good code. Why not use them?

A product manager wants new features and to address user needs. There are only so many sprints in a quarter.

You, as a developer, may come to an existing, old project with fresh eyes and think "oh we should be linting", "static type checking would help catch bugs earlier", or "testing this all manually is a terrible way to develop, we should automate". Codebases have historical baggage, and this will bump into reality quite quickly. `ruff` thinks your code smells like day old sewage. `ty` chokes on your lack of annotations. Tests take dev time to write.

So, now you've created a mountain of tech debt. But you are intrepid, and still want your dream SQA tooling. Your product manager is juggling many priorities (and you should be too, really). What now?

## You know who else likes SQA

Certainly I'm not the first person to say this. Coding agents like feedback. Sweet sweet deterministic feedback. 

Remember this old thing from intro to artificial intelligence?

``` mermaid

flowchart LR
    A[Agent] --action--> E[Environment]
    E --feedback--> A

```

Any agent thrives on a healthy feedback signal. The more info we can give, the better chance for a good action. 

Out of all the hype you may see on agents in enterprise, coding agents seem to be real. However, they're still like driving a car. Good drivers have licenses, follow traffic laws, wear their seat belts, drive sober, etc.

Coding agents need these same safety checks to maximize success. The more that your SQA suite can quickly check, the more feedback the agent gets and can quickly fix if need be. And needs will often be! LLMs are great at coding, but one can't let them drive without a seatbelt.

Maybe all this is obvious.

## Selling the dream

Like it or not, C-suites, boardrooms, and other various senior business people are excited about artificial intelligence. Usually the "LLM-powered agents automating things" kind of artificial intelligence. This, in turn, trickles down to all levels of management. 

So, now better tooling and tests will help your dev team. How can you get your product manager on board? 

This type of work is no longer "tech debt". No longer "best practices". This is "AI enablement".

You'll get better tooling that legitimately does work better with coding assistants. Your product manager will get a great story to tell. Everyone wins.

Furthermore, the next time your senior leadership hears about your dev team, they won't hear that you're "taking time to address tech debt". No, you'll be embracing coding agents with open arms. Unabashedly improving the interface between your codebase and the coding agents making your life easier.

Give it a whirl at your next one-on-one!
