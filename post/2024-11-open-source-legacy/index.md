---
title: "Why I Build Comprehensively in Open Source"
date: 2024-11-15
tags: ["open-source", "legacy", "reproducible-science", "philosophy", "software-design"]
draft: false
series: ["the-long-echo"]
series_weight: 6
---

I maintain 50+ open source repositories. Every one has documentation, tests, examples, and clear architecture.

People ask: "Why spend so much time on free software when you have stage 4 cancer?"

The question misunderstands what I'm doing. This isn't charity. It's **value imprinting at scale**.

## First: Reproducible Science

Science requires reproducibility. If you publish computational results without releasing code, you've made **unverifiable claims**.

Every paper I publish includes:
- Open source implementation
- Documented dependencies
- Reproducible examples
- Test suites
- Clear installation instructions

This isn't optional. It's **methodological rigor**.

When someone reads my reliability analysis paper and wants to verify my results, they can:
1. Clone the repo
2. Install dependencies
3. Run the code
4. Reproduce the figures

That's how science should work.

## Second: Continuability

With stage 4 cancer, I think explicitly: **what if I'm not here to maintain this?**

For work to be continuable:

**Clear architecture**: Someone else must understand design decisions

**Comprehensive docs**: Explain not just *what* but *why*

**Test suites**: Demonstrate usage and validate correctness

**Clean dependencies**: No fragile, undocumented requirements

**Examples**: Show intent through working code

**Open license**: Remove legal barriers to continuation

I'm not building for myself. I'm building for **whoever comes next**.

## Third: Value Imprinting

Code embodies values. Every API decision reflects beliefs:

- What should be easy vs. hard?
- What should be explicit vs. implicit?
- What should be safe by default?
- What complexity is worth exposing?

My libraries encode:
- **Transparency over magic**: No hidden behavior
- **Composition over monoliths**: Small pieces, flexibly combined
- **Clarity over cleverness**: Obvious beats impressive
- **Safety by default**: The easy path is the safe path

These values propagate. Someone uses my library → internalizes its principles → applies them elsewhere.

**Code is how values scale beyond individual lifetimes.**

## Fourth: Usefulness to Others

I've benefited enormously from open source:

- Libraries that solved problems I couldn't
- Documentation that taught me techniques
- Examples that clarified concepts
- Tools that accelerated my research

Contributing back isn't obligation—it's **participating in a gift economy**.

When you solve a problem well and publish it, thousands don't have to solve it again. That's massive leverage.

## Fifth: Aesthetic Satisfaction

Well-crafted code is intrinsically beautiful:

- Clean abstractions that feel inevitable
- APIs that make complex operations simple
- Documentation explaining *why* not just *what*
- Tests that serve as executable specifications
- Code structured for human comprehension

I maintain libraries obsessively because **craftsmanship is rewarding**.

The cancer doesn't change this. If anything, it intensifies it—do work worth doing **while you can**.

## Why "Comprehensive"?

Why not just throw code over the wall?

Because **incomplete open source often wastes more time than it saves**:

- Undocumented code: Users spend hours figuring out what you could have explained in minutes
- Untested code: Breaks in production, creates technical debt
- Unclear dependencies: Installation becomes archaeological excavation
- No examples: Users can't tell if they're using it correctly
- Ambiguous licensing: Legal uncertainty blocks adoption

Comprehensive open source **respects users' time**.

When I publish:
1. Installation works first try
2. Examples run without modification
3. API is documented with clear examples
4. Tests demonstrate usage patterns
5. Architecture is explained
6. License is unambiguous
7. Dependencies are specified
8. Contribution guidelines are clear

This takes effort. But it's the difference between **publishing code and publishing tools**.

## The Cost

Comprehensive open source is expensive:

**Time**: Documenting takes longer than coding
**Energy**: Examples and tests require effort
**Focus**: Polish competes with new features

But these costs buy:
- Reproducibility (scientific rigor)
- Continuability (legacy beyond yourself)
- Usefulness (others can actually use it)
- Value propagation (principles spread)
- Quality (craftsmanship matters)

For work I want to outlive me, **this is the right tradeoff**.

## Stage 4 Changes the Calculation

With uncertain time horizons:

**Every project gets filtered**: "If this is my last contribution to this domain, is it worth it?"

**Documentation becomes critical**: I might not be here to explain

**Completeness matters more**: Half-finished work helps no one

**Open source is essential**: Proprietary work dies with me

Cancer doesn't make me publish less. It makes me **publish more carefully**.

## Examples From My Work

**Statistical libraries in R**: Every function documented with mathematical background, examples, and tests. Someone can pick this up and understand both the code and the theory.

**Cryptographic tools**: Type systems enforce security properties. Documentation explains threat models. Tests demonstrate correct usage. Someone can audit and continue this work.

**Network analysis code**: Abstracts over graph representations. Works on any structure satisfying the graph interface. Examples show how to extend it. Someone can build on this foundation.

All structured for **continuation without me**.

## The Long View

Well-documented, clearly-architected, thoroughly-tested open source has **staying power**.

Someone in 2030 might use a library I published in 2024—if it's:
- Well-designed enough to remain relevant
- Well-documented enough to understand
- Well-tested enough to trust
- Well-licensed enough to modify

That's the goal: **work that remains useful after I'm gone**.

## What This Means Practically

Every commit I make asks:

- Is this design decision explained?
- Are the tests comprehensive?
- Is the documentation current?
- Can someone else continue this?
- Does this encode the values I want to propagate?

This discipline is expensive. But **legacy is about what persists**, not what's easiest.

## The Paradox

Cancer gives you less time but **clarifies what that time is for**.

I'm more productive now than before diagnosis. Not because I work longer hours (I don't—treatment limits that).

Because I **waste less time on things that don't matter**:

- Code without docs? Not worth publishing
- Projects I'll abandon? Don't start them
- Work I can't explain? Figure it out or skip it
- Tools no one can use? Not worth building

The filter is simple: **will this remain useful and usable after I'm gone?**

Most things fail that test. The few that pass get everything I have.

## Open Source as Archive

My comprehensive open source work is fundamentally an **archival project**:

- Document my reasoning
- Encode my values
- Share my tools
- Make my work reproducible
- Enable continuation

Not memorial. **Functional archive**.

If someone in 2030 wants to:
- Reproduce my reliability analysis results
- Understand my cryptographic designs
- Build on my network analysis methods
- Continue my statistical computing work

They can. The code exists. The documentation explains it. The tests validate it. The license permits it.

**That's what I'm building toward.**

## For Other Researchers

If you're doing computational work:

**Publish your code**. Not someday. Now.

**Document it properly**. Explain why, not just what.

**Test it thoroughly**. Tests are specification and validation.

**License it clearly**. MIT or Apache 2.0 removes barriers.

**Make it reproducible**. Someone should be able to run your code and get your results.

This isn't extra work. It's **the minimum standard for computational science**.

## The Bottom Line

I build comprehensively in open source because:

1. **Science demands reproducibility**
2. **Legacy requires continuability**
3. **Values propagate through code**
4. **Others benefit from shared tools**
5. **Craftsmanship is intrinsically rewarding**
6. **Cancer makes priorities clear**

Every repository is an attempt to build something that **outlasts me**.

Not for immortality. For **usefulness**.

---

*Check the repos. Every commit encodes: this is how I think problems should be solved.*
