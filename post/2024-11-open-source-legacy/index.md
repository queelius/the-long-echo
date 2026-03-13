---
title: "Why I Build Comprehensively in Open Source"
date: 2024-11-15
tags: ["open-source", "legacy", "reproducible-science", "philosophy", "software-design"]
draft: false
description: "On building comprehensive open source software as value imprinting at scale, reproducible science, and leaving intellectual legacy under terminal constraints."
series: ["the-long-echo"]
series_weight: 6
---

I maintain 50+ open source repositories. Every one has documentation, tests, examples, and clear architecture.

People ask: "Why spend so much time on free software when you have stage 4 cancer?"

The question misunderstands what I'm doing. This isn't charity. It's value imprinting at scale.

## Reproducible Science

Science requires reproducibility. If you publish computational results without releasing code, you've made unverifiable claims. Full stop.

Every paper I publish includes an open source implementation, documented dependencies, reproducible examples, test suites, and clear installation instructions. This isn't optional. It's methodological rigor. When someone reads my reliability analysis paper and wants to verify my results, they can clone the repo, install dependencies, run the code, and reproduce the figures. That's how science should work.

## Continuability

With stage 4 cancer, I think explicitly about what happens if I'm not here to maintain this.

For work to be continuable, it needs clear architecture (someone else must understand the design decisions), comprehensive docs (explain not just what but why), test suites (demonstrate usage and validate correctness), clean dependencies (no fragile undocumented requirements), examples (show intent through working code), and open licensing (remove legal barriers to continuation).

I'm not building for myself. I'm building for whoever comes next.

## Value Imprinting

Code embodies values. Every API decision reflects beliefs about what should be easy vs. hard, what should be explicit vs. implicit, what should be safe by default, what complexity is worth exposing.

My libraries encode transparency over magic, composition over monoliths, clarity over cleverness, safety by default. These values propagate. Someone uses my library, internalizes its principles, applies them elsewhere.

Code is how values scale beyond individual lifetimes.

## Usefulness to Others

I've benefited enormously from open source. Libraries that solved problems I couldn't. Documentation that taught me techniques. Examples that clarified concepts. Tools that accelerated my research.

Contributing back isn't obligation. It's participating in a gift economy. When you solve a problem well and publish it, thousands of people don't have to solve it again.

## Craftsmanship

Well-crafted code is intrinsically beautiful. Clean abstractions that feel inevitable. APIs that make complex operations simple. Documentation explaining why, not just what. Tests that serve as executable specifications. Code structured for human comprehension.

I maintain libraries obsessively because craftsmanship is rewarding. The cancer doesn't change this. If anything, it intensifies it. Do work worth doing while you can.

## Why "Comprehensive"?

Why not just throw code over the wall?

Because incomplete open source often wastes more time than it saves. Undocumented code: users spend hours figuring out what you could have explained in minutes. Untested code: breaks in production, creates technical debt. Unclear dependencies: installation becomes archaeological excavation. No examples: users can't tell if they're using it correctly. Ambiguous licensing: legal uncertainty blocks adoption.

Comprehensive open source respects users' time.

When I publish, installation works first try, examples run without modification, the API is documented with clear examples, tests demonstrate usage patterns, architecture is explained, license is unambiguous, dependencies are specified, and contribution guidelines are clear.

This takes effort. But it's the difference between publishing code and publishing tools.

## The Cost

Comprehensive open source is expensive. Documenting takes longer than coding. Examples and tests require effort. Polish competes with new features.

But these costs buy reproducibility, continuability, usefulness, value propagation, and quality. For work I want to outlive me, this is the right tradeoff.

## Stage 4 Changes the Calculation

With uncertain time horizons, every project gets filtered: "If this is my last contribution to this domain, is it worth it?" Documentation becomes critical because I might not be here to explain. Completeness matters more because half-finished work helps no one. Open source is essential because proprietary work dies with me.

Cancer doesn't make me publish less. It makes me publish more carefully.

## Examples From My Work

My statistical libraries in R: every function documented with mathematical background, examples, and tests. Someone can pick this up and understand both the code and the theory.

My cryptographic tools: type systems enforce security properties. Documentation explains threat models. Tests demonstrate correct usage. Someone can audit and continue this work.

My network analysis code: abstracts over graph representations. Works on any structure satisfying the graph interface. Examples show how to extend it. Someone can build on this foundation.

All structured for continuation without me.

## The Long View

Well-documented, clearly-architected, thoroughly-tested open source has staying power.

Someone in 2030 might use a library I published in 2024, if it's well-designed enough to remain relevant, well-documented enough to understand, well-tested enough to trust, and well-licensed enough to modify.

That's the goal: work that remains useful after I'm gone.

## What This Means Practically

Every commit I make asks: Is this design decision explained? Are the tests comprehensive? Is the documentation current? Can someone else continue this? Does this encode the values I want to propagate?

This discipline is expensive. But legacy is about what persists, not what's easiest.

## The Paradox

Cancer gives you less time but clarifies what that time is for.

I'm more productive now than before diagnosis. Not because I work longer hours (I don't, treatment limits that). Because I waste less time on things that don't matter. Code without docs? Not worth publishing. Projects I'll abandon? Don't start them. Work I can't explain? Figure it out or skip it. Tools no one can use? Not worth building.

The filter is simple: will this remain useful and usable after I'm gone?

Most things fail that test. The few that pass get everything I have.

## Open Source as Archive

My comprehensive open source work is fundamentally an archival project. Document my reasoning. Encode my values. Share my tools. Make my work reproducible. Enable continuation.

Not memorial. Functional archive.

If someone in 2030 wants to reproduce my reliability analysis results, understand my cryptographic designs, build on my network analysis methods, or continue my statistical computing work, they can. The code exists. The documentation explains it. The tests validate it. The license permits it.

That's what I'm building toward.

## For Other Researchers

If you're doing computational work: publish your code. Not someday. Now. Document it properly. Explain why, not just what. Test it thoroughly (tests are specification and validation). License it clearly (MIT or Apache 2.0 removes barriers). Make it reproducible (someone should be able to run your code and get your results).

This isn't extra work. It's the minimum standard for computational science.

## The Bottom Line

I build comprehensively in open source because science demands reproducibility, legacy requires continuability, values propagate through code, others benefit from shared tools, craftsmanship is intrinsically rewarding, and cancer makes priorities clear.

Every repository is an attempt to build something that outlasts me. Not for immortality. For usefulness.
