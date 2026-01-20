---
title: "API Design as Value Imprinting"
date: 2019-04-22
tags: ["api-design", "software-design", "philosophy", "ethics"]
draft: false
series: ["the-long-echo"]
series_weight: 50
---

I've been thinking about how **API design encodes values**—not just technical decisions, but philosophical ones.

Every interface you create is a constraint on future behavior. Every abstraction emphasizes certain patterns and discourages others. You're not just building tools—you're shaping how people think about problems.

## What Your API Communicates

Consider these design choices:

**Mutability vs Immutability**: Do you encourage stateful modification or pure functions? This isn't just performance—it's a philosophy about side effects and reasoning.

**Explicit vs Implicit**: Do you make users specify parameters or infer from context? This trades convenience for transparency.

**Fail Fast vs Fail Safe**: Do you throw exceptions or return error codes? This encodes beliefs about error handling and control flow.

## My Design Values

When I build libraries, I try to encode:

**Explicitness over magic**: I'd rather make users type more than hide behavior
**Composition over inheritance**: Prefer small pieces that combine flexibly
**Clarity over cleverness**: Code should be obvious, not impressive
**Safety by default**: The easy path should be the safe path

## Why This Matters

Your API is a value statement. It says:
- What you think is important
- What you think is dangerous
- How you think about the problem domain

This is why I spend so long on interface design. I'm not just making technical decisions—I'm imprinting values into artifacts that might outlive me.

**The APIs we create shape future thought.**

---

*Every library I publish is an attempt to encode certain values: transparency, composability, and respect for the user's intelligence.*
