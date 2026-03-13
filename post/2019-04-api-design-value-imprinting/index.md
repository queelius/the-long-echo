---
title: "API Design as Value Imprinting"
date: 2019-04-22
tags: ["api-design", "software-design", "philosophy", "ethics"]
draft: false
series: ["the-long-echo"]
series_weight: 50
description: "API design encodes philosophical values: mutability, explicitness, error handling. Your interface shapes how people think about problems."
---

Every interface you create is a constraint on future behavior. Every abstraction emphasizes certain patterns and discourages others. You are not just building tools. You are shaping how people think about problems.

I have been paying attention to how API design encodes values, not just technical decisions, but philosophical ones.

## What Your API Communicates

Consider these design choices:

**Mutability vs Immutability.** Do you encourage stateful modification or pure functions? This is not just about performance. It is a philosophy about side effects and reasoning. If your default is mutable state, you are telling users that local mutation is fine, that they can reason locally. If your default is immutability, you are telling them to think about data flow.

**Explicit vs Implicit.** Do you make users specify parameters or infer from context? This trades convenience for transparency. I lean toward explicitness. Magic is convenient until you need to debug it.

**Fail Fast vs Fail Safe.** Do you throw exceptions or return error codes? This encodes beliefs about who should handle errors and when. Fail-fast says "don't let bad state propagate." Fail-safe says "keep running if you can." Both are defensible, but they lead to very different code.

## My Design Values

When I build libraries, I try to encode:

- **Explicitness over magic.** I would rather make users type more than hide behavior behind conventions they have to discover.
- **Composition over inheritance.** Small pieces that combine flexibly beat deep class hierarchies.
- **Clarity over cleverness.** Code should be obvious, not impressive.
- **Safety by default.** The easy path should be the safe path.

## Why This Matters

Your API is a value statement. It says what you think is important, what you think is dangerous, and how you think about the problem domain.

This is why I spend so long on interface design. The APIs we create shape future thought. They outlast the code that implements them, because the patterns they teach persist in the minds of the people who use them.
