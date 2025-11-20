---
draft: false
date: 2025-11-02
categories:
  - General
authors:
  - ianwhalen
slug: hello-world
---

# Baby's first blog post

This one started as a test post. Originally, I wanted to ensure all the bells and whistles are working. And, I guess that includes being able to see the test online as well. So, for now, it will stay.

<!-- more -->

If you're looking for more about why I decided a blog would be a good idea, see [here](../about.md).

I have very aggressively named this file `0001_hello_world.md`. Needing over 1000 numbers to order my blog posts seems wildly unlikely, but it is at least future proofed.


## Icons and emoji

Let's fire off some emoji and material-included icons:

- Emojis: :tada: :skull: :apple:
- Icons: :fontawesome-regular-face-laugh-wink: :material-bolt: :octicons-gift-16: :simple-nvidia:

I had no idea that so many icons were included with material! Moving on...

## Code

Code formatting `inline` and block:

``` python
from pathlib import Path

print("This is just some code...")
```

## Latex

Some inline latex $\sigma > 0, \exists \delta$ and block:

$$
\text{If} \quad 0 \leq \lvert{} x - 1 \rvert{} \leq \delta \quad  \text{then} \quad \lvert{} f(x) - 5 \rvert{} < \sigma
$$

## Mermaid :simple-mermaid:

A little mermaid diagram:

``` mermaid
flowchart TD
    A[Make blog] --> B[Make people happy]
    B --> C[Retire early]
```

## Admonitions

A couple admonitions:

!!! quote "This is an admonition title"

    _This is an admonition. Pay close attention!_

!!! success ""

    I'm going to try to nest them now, wish me luck.

    !!! question "Did it work?"

        I sure hope it worked. I'm not sure why I would want to do this though.

## Footnotes

I have much more to say, but I don't want to say it all here[^1]^,^[^2].

## Javascript

Likely never to be used, but lets dump in some javascript.

<div id="counter-widget" style="display: inline-block;">
  <button id="counter-btn" style="padding: 10px 20px; background-color: #ff764d; border-radius: 8px;">
    Click me!
  </button>
  <span id="counter-display">Count: 0</span>
</div>

<script>
  (function() {
    let count = 0;
    const btn = document.getElementById('counter-btn');
    const display = document.getElementById('counter-display');
    
    btn.addEventListener('click', function() {
      count++;
      display.textContent = 'Count: ' + count;
    });
  })();
</script>


[^1]: Just kidding, I don't have anything else to say.
[^2]: How do multiple footnotes work?
