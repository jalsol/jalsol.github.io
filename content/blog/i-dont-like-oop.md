---
title: "[en] I don't like OOP and the OOP delusion"
date: 2025-04-16
extra:
description: "I don't like OOP. The abuse of OOP can do more harm than good. I hate the OOP and design patterns cultists."
taxonomies:
    tags: ["english", "oop", "programming", "software engineering"]
---

[Đọc bản tiếng Việt tại đây.](/blog/toi-khong-thich-oop/)

Today I encountered a talk (in Vietnamese) by Dinh Ngoc Thanh - Chief Architect at Holistics, my (former) company. Since it's a talk I really like, I'm sharing it here and adding a few words of my own.

TL;DR: I don't like OOP. The abuse of OOP can do more harm than good. I hate the OOP and design patterns cultists. Anh Thanh has analyzed a different perspective on OOP in the video. I like this perspective and I think everyone should too.

<div style="display: flex;">
  <iframe style="aspect-ratio: 16 / 9; width: 100% !important;" src="https://www.youtube.com/embed/7PGCvpJl_0o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

Key takeaways from the video:
- Alan Kay's definition of OOP is about object communication. Class and inheritance are not the core of OOP.
- Software design is about cracking complexity. OOP **does not** reduce global complexity, it just packages complexity into isolated, smaller boxes.
- 5 aspects of complexity: Shared mutable state, Side effects, Dependencies, Control flow, and Code size.
- Adaptation of "Functional Shell, Imperative Core" at Holistics.

{{hr()}}

OOP is extremely popular. OOP is taught right during the first or second year of college. Popular languages and frameworks have featured OOP for decades. People say that using OOP makes code cleaner, more flexible, and easier to develop. People learn OOP, learn design patterns, and some even worship them mindlessly.

I don't like OOP (or at least the kind of OOP people are used to). I hate the fact that there are people arguing on the internet (very dumb of me, I know) and even professors who consider OOP to be the best (I was disappointed with Computer Science education in Vietnam).

There is a paradox is that although OOP has existed for a very, very long time alongside the development of software engineering, it does not have a solid theoretical foundation. Functional Programming (the paradigm I currently like the most) is based on lambda calculus, while Imperative Programming is based on Turing machine. OOP is the opposite: people created OOP from decades ago, and only recently have they devised [a theory for it](https://arxiv.org/abs/2111.13384).

A huge problem in OOP and software engineering in general is too much abstraction. The CM101 course taught me that abstraction is important. However, too much abstraction increases the complexity of the system: poor performance, difficult development, difficult debugging, complex control flow... Sometimes abstraction does not reduce complexity, but only divides it into smaller parts or hides it. Meanwhile, if you wrap raw data with structs and process it directly, everything becomes simpler. It is no coincidence that modern languages like Go and Rust do not have classes or inheritance but only structs.

There are design patterns in OOP created just because people wanted to "bypass" things that could be done very easily in other paradigms, such as Singleton or Facade. Bundling state and behavior together into a class makes it difficult to test behaviors (because you have to achieve the right state and the right dependency to replicate the behavior).

I prefer to view OOP as a method for objects to communicate data with each other through a contract/interface. This perspective on OOP is not bound by classes and inheritance, is more flexible, more direct, and allows for better integration of other paradigms (like Functional Programming). You can watch the video to understand more.
