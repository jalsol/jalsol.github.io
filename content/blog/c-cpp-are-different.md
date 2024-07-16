---
title: "C and C++ are different languages"
date: 2023-07-04
description: "As a C++ programmer, C is an entirely different kind of beast."
taxonomies:
    tags: ["english", "cpp", "project", "coursework"]
---

**UPDATE** (2023-09-03)

I am reimplementing [jaledit](/blog/jaledit) in C.

It is 95% complete compared to the C++ version. The source code is available in the `c-rereimpl` branch of the original repository.

With C, I have to manually allocate/deallocate memory, implement reference counting from scratch, find workarounds to compensate for the lack of OOP ("pimpl" for encapsulation, unions with enums for inheritance and polymorphism), and so much more.

I really learned a lot from that. Developing a decent project in C made me appreciate higher-level languages and how many modern features are just "syntactic sugar" in one way or another.

At least I can say that I have fulfilled a goal I set for myself by working with C at the beginning of the project: to push my boundaries even further.

{{hr()}}

Programming with C is tremendously difficult. It's an entirely different kind of beast. I usually tell people that C and C++ are completely different languages, and proficiency in one language doesn't necessarily translate well to the other.

I am currently working on my lab project: [a text editor](/blog/jaledit). At first, I thought C would be a nice idea: I am somewhat comfortable with C++ already, and I want to push my boundaries even further. And I started working with C.

However, it turns out, as the project is evolving, there is just too much work. I am already missing C++ features, including (but not limited to) its standard containers, smart pointers, RAII, and OOP features. The time is limited, and the development speed is painfully slow with all the manual work.

I can definitely continue to work with C, but not under the current constraints. I like the simplicity of C's expressions, but at the same time, it is not the most suitable choice for more complex models.

I may, therefore, accept the defeat, and go back to C++.
