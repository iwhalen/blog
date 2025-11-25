---
draft: false
date: 2025-11-24
categories:
  - Code
  - Python
authors:
  - ianwhalen
slug: ibis-compatibility
---

# Ibis compatibility checker

I'm a fan of the [Ibis framework](https://ibis-project.org/). To me, dataFrame APIs feel great and leaving pure Python to write a SQL query in a nasty triple-quoted string does not. 

In this post, I motivate and give a proof of concept for an Ibis "compatibility checker" where, given an Ibis expression, the checker will tell you which backends it can be run on.

This post isn't meant to sell Ibis, just motivate why I think it could be useful for a specific set of people.

Code for this post can be found [here](https://github.com/ianwhale/ibis-compatibility).

<!-- more -->

## Motivation

Let's say I want to write a reusable feature engineering pipeline. Say, I have a good idea for building features for churn modeling. I want to roll this out to all my customers on their infrastructure. My customers all have different levels of maturity and (of course) different infrastructure.

Not an exhaustive list, but some options are:

- Pandas for everything
- Rely on PySpark
- Write everything in SQL
- Use a fancy tool like DBT

These all have positives and negatives, but if my requirements are: 

- Pythonic API
- No vendor lock in
- Scalable to realistic datasets[^realistic-dataset]
- Few assumptions about infrastructure

Now, I'm definitely not a data engineer. If you are, you're likely scoffing and saying _"tool X would solve all of this. What a fool."_ You're probably right! I'm no expert here.

However, you are the one reading this post! And I have been struck by how it seems like Ibis helps solve most of these points.

Ibis lets us assume we can at least get our data into a particular data model and then run things like feature engineering in a portable way. It won't matter if we have flat files or an OLAP database. We can write some code to fit the raw data into our model[^raw-to-model-note] and then run our feature engineering or analytics or whatever. 

However, a question that I could never answer was: _given an Ibis expression, which backends will it run on?_

Ibis has a great documentation page [here](https://ibis-project.org/backends/support/matrix). This answers this exact question, but one would have to search every single operation iteratively. If you're trying to maximize compatibility, this would be cumbersome.

So, I set out to try to do this automatically.

## Checking compatibility

Out of the box, each `Backend` class in Ibis has a `has_operation` method. This does some complex delegation out to a `Backend`'s internals. What matters is, this code will return `False`:

``` python 
from ibis.backends.duckdb import Backend
import ibis.expr.operations as ops

Backend().has_operation(ops.HashBytes)
```

and this code will return `True`:

``` python
from ibis.backends.duckdb import Backend
import ibis.expr.operations as ops

Backend().has_operation(ops.Filter)
```

because the `duckdb` backend does not support the `HashBytes` operation and does support the `Filter` operation.

Given this, it seems the job is done! We can just run this in a loop across an expression's operations and return which backends will play nicely.

We could get inspired by [the code](https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/docs/backends/support/matrix.qmd#L74) that builds the operation support matrix and call it a day!

## Dependency hell

Of course, it's not that easy. At the time of writing, Ibis has $20$ backends. In order to import all $20$ backends, I have to install all Python packages and system-level dependencies for those backends.

Ibis has [lots of options](https://ibis-project.org/contribute/01_environment) for developing their tool with all these dependencies. However, none of them really suit the tool I'm trying to build.

To use a hypothetical "ibis compatibility checker", one would also have to have every single dependency installed. This renders the tool useless in my opinion.

## The checker

The Ibis documentation has to be built at some regular interval. So, the operation support matrix is updated at some regular interval. Why not just scrape this big table at runtime?

So that's what I did! You can see my implementation details [here](https://github.com/ianwhale/ibis-compatibility). 

This way, the only requirements for the library are:

- Ibis itself (with no backend dependencies)
- Beautiful soup
- `httpx`

This isn't perfect as it relies on the docs having the same format in perpetuity. However, it's good enough for what I wanted.

Most importantly, we rely on the Ibis team to maintain their developer environment and build the docs for us. Which minimizes our dependencies.


[^realistic-dataset]: For more on what a realistic dataset size is for enterprise use cases, see [here](https://motherduck.com/blog/big-data-is-dead/).

[^raw-to-model-note]: Certainly this is easier said than done.