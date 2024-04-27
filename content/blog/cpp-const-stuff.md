---
title: "C++'s const, constexpr, consteval"
date: 2024-04-27
extra:
    add_toc: true
---

## The `const` keywords in C++

C++ is highly regarded for its control over performance. The `const` keywords
are undoubtedly important in achieving optimized applications. These keywords
are some of the fundamentals every C++ developer has to know.

In this blog, we are we considering these keywords:
- `const`;
- `constexpr`;
- `consteval`.

I have yet to come up with a good use for `constinit` (even [cppreference](https://en.cppreference.com/w/cpp/language/constinit)
has no examples). That is why we are not discussing it right now. Probably I
will have a part 2, or even update this very own blog as well for it.

## Immutable vs. Immediate

Before any further explanation, I want to define some terminologies first.

> **Immutable**: after creation, value cannot be changed during and after
running.

> **Immediate**: **immutable**, but value is also known before running.

From the above definition, we can also see that:

> An **immutable** variable can be initialized during either compile time or run
time.

> An **immediate** variable can **only** be initialized during compile time.

It can be seen that **immediate** variables have stricter conditions than
**immutable** ones. Thus, it allows greater (compile-time) optimizations.

## `const` vs. `constexpr` variable

Okay, I hope we are settled on the definitions. Now to the actual C++ part.

### `const`

> A `const` variable is **immutable**.

What this means is that this code is **valid**:

```cpp
void valid() {
    int a;
    std::cin >> a;

    const int b = a + 1; // OK

    b = 69; // not OK, b is immutable
}
```

Let's do a checklist:

- Can the value of `b` be changed after its creation? **No**
- Does the program know the value of `b` before running? **No**

The first condition satisfies the definition of an **immutable** variable.

The second condition doesn't affect its immutability at all. However, it will be
important in the next part.

### `constexpr`

> A `constexpr` variable is **immediate**.

A **valid** example:

```cpp
void valid() {
    constexpr int a = 6; // OK, a = 6
    constexpr int b = a + 9; // OK, b = 15

    b = 7; // not OK, b is immediate/immutable
}
```

- Can the value of `b` be changed after its creation? **No**
- Does the program know the value of `b` before running? **Yes**

The first condition satisfies the definition of an **immediate** (which is also
immutable) variable.

The second condition satisfies the definition of an **immediate**. The program
knows the value `a` and `b` before running, and they won't change during and
after running.

And an **invalid** example:

```cpp
void invalid() {
    int a;
    std::cin >> a;

    constexpr int b = a + 1; // not OK, b is not known before running
}
```

- Does the program know the value of `b` before running? **No**

Thus, it fails to become a `constexpr` variable.

### `const` number == `constexpr`?

But, what about this example:

```cpp
int main() {
    const int b = 10;
}
```

- Can the value of `b` be changed after its creation? **No**
- Does the program know the value of `b` before running? **Yes**

`b` now satisfies both conditions of an **immediate** variable, but it is
declared as `const`. It is actually okay. A good compiler may be able to detect
this is effectively the same as a `constexpr` variable and optimize it.

However, you are still *betting* on the compiler to do the magic work for you. If
you want to be certain, just use `constexpr`.

## What can `constexpr` variables do?

I'm [running the examples in Compiler Explorer](https://godbolt.org/z/hMa8b9b9P)
for the assembly.

To avoid other sorts of optimizations from the compiler, I'm not using any
optimization flags for these examples.

### No `constexpr`

Let's consider an example with **no `constexpr`**:

```cpp
int func1() {
    int a = 17;
    int b = 9;
    return a / b;
}
```

A simple function. Let's look at the assembly:

```asm
func1():
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 17
        mov     DWORD PTR [rbp-8], 9
        mov     eax, DWORD PTR [rbp-4] # load dividend
        cdq
        idiv    DWORD PTR [rbp-8]      # load divisor -> divide
        pop     rbp
        ret
```

Both the values of `a` and `b` are loaded to the memory at `[rbp-4]` and
`[rbp-8]`, respectively.

> Note: Due to keeping the stack as is (because no optimization flags are
enabled), these two `mov` instructions are kept throughout all examples. They
are not really relevant to our investigation.

For this example:
- `eax` (the dividend) is loaded from the *memory* (slow);
- `idiv` is used (slow);
- The argument of `idiv` (the divisor) is loaded from the *memory* (slow).

Full of slow stuff. We can do better.

### `constexpr` dividend

```cpp
int func2() {
    constexpr int a = 17;
    int b = 9;
    return a / b;
}
```

```asm
func2():
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 17
        mov     DWORD PTR [rbp-8], 9
        mov     eax, 17 # load dividend as 17,
                        # no memory access
        cdq
        idiv    DWORD PTR [rbp-8] # load divisor -> divide
        pop     rbp
        ret
```

Mostly the same as the previous example. However, there is a difference: `eax`
is now loaded with an *immediate* value of `17`, instead of loading from the
memory.

The equivalent C++ code will be:

```cpp
int func2p() {
    int b = 9;
    return 17 / b;
}
```

Notice how `a` is gone. Using `constexpr` like in `func2()`, we can have the
code with the same performance as `func2p()` without replacing the variable
with its value by hand.

### `constexpr` divisor

```cpp
int func3() {
    int a = 17;
    constexpr int b = 9;
    return a / b;
}
```

```asm
func3():
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 17
        mov     DWORD PTR [rbp-8], 9
        mov     eax, DWORD PTR [rbp-4] # load dividend
        movsx   rdx, eax
        imul    rdx, rdx, 954437177    # multiply the dividend
                                       # with (1/9 mod 2^32)
        shr     rdx, 32     # these lines of code are
        sar     edx         # normalizing the result
        sar     eax, 31     # of the division
        sub     edx, eax    # to account for inaccuracy
        mov     eax, edx
        pop     rbp
        ret
```

Notice we are not using `idiv` anymore. We are using another trick instead.

This seemingly "magic" value of `954437177` is actually (1/9 mod 2^32). Instead
of doing `17/9`, we are doing `17 * 1/9` with the faster `imul` instruction. The
compiler calculates 1/9 beforehand.

This technique is called "Division by Invariant Multiplication". A bunch of
arithmetic instructions in the second half of the function are normalizing the
result to compensate for the inaccuracy that may happen.

The equivalent C++ code will be:

```cpp
int func3p() {
    int a = 17;
    return a / 9;
}
```

### Both `constexpr`

```cpp
int func4() {
    constexpr int a = 17;
    constexpr int b = 9;
    return a / b;
}
```

```asm
func4():
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 17
        mov     DWORD PTR [rbp-8], 9
        mov     eax, 1    # return 1
        pop     rbp
        ret
```

With both sides being `constexpr`, the compiler just calculates `17/9 == 1`, and
return `1`. That's it.

The equivalent C++ code will be:

```cpp
int func4p() {
    return 17 / 9; // or: return 1;
}
```

## `constexpr` function

You have seen what magic those C++ compilers can do with `constexpr` variables.
Let's make a step further and apply `constexpr` to functions.

However, there is a rather "weird" trait:

> A `constexpr` function can evaluate either during compile time or run time.

The next example can be viewed on Compiler Explorer [here](https://godbolt.org/z/dcK653q3T).

```cpp
constexpr int add_five(int n) {
    return n + 5;
}

int main() {
    int n = 5;
    const int a = add_five(n);
    constexpr int b = add_five(10);
}
```

```asm
add_five(int):
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], edi
        mov     eax, DWORD PTR [rbp-4]
        add     eax, 5
        pop     rbp
        ret
main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     DWORD PTR [rbp-4], 5    # n := 5
        mov     eax, DWORD PTR [rbp-4]  # eax := n
        mov     edi, eax                # edi := eax
        call    add_five(int)           # eax := add_five(edi)
        mov     DWORD PTR [rbp-8], eax  # a := eax
        mov     DWORD PTR [rbp-12], 15  # b := 15
        mov     eax, 0
        leave
        ret
```

`add_five` is a `constexpr` function. It is kept in the assembly, because later
on, it will be used in run time.

It can be seen that `a` is an **immutable** variable as its value will be
determined during run time. There is indeed a call to `add_five`, with `n` as
the argument.

However, `b` is an **immediate** variable, as it is `constexpr`.
`add_five(10) == 15`, and `b` is assigned with the value `15` without any call
to the function.

We can see that for this `constexpr` function, if it can evaluate during compile
time, it will. Otherwise, it will evaluate during run time for other cases.

## `consteval` function

C++20 introduces a new keyword: `consteval`. It is a keyword used with functions
(and not with variables). It aims to provide another tool for compile-time
control.

> A `consteval` function can **only** evaluate during compile time. Otherwise,
the program fails to compile.

We have a similar [example](https://godbolt.org/z/P3K591zxa), but this time
using `consteval`:

```cpp
consteval int add_five(int n) {
    return n + 5;
}

int main() {
    // the following 2 lines lead to compilation errors:
    // int n = 5;
    // const int a = add_five(n);

    constexpr int b = add_five(10);
}
```

```asm
main:
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 15   # b := 15
        mov     eax, 0
        pop     rbp
        ret
```

Pretty much the same effect. However, it will fail to compile if it can't
evaluate during compile time.

The function `add_five` is also gone in the assembly as well: it can only be
executed in compile time, and it should not exist after compilation is done.

## `if constexpr`

I was watching Carl Cook's talk at CppCon 2017. He showed an example of
[eliminating the branching away with template specialization](https://youtu.be/NH1Tta7purM?t=1246).

Scrolling down the comments, there was one mentioning `if constexpr`. And I
thought it was brilliant.

### Branching approach

This is the "branching approach" in Cook's talk:

```cpp
enum class Side { Buy, Sell };

float CalcPrice(Side side, float value, float credit) {
    return side == Side::Buy
         ? value - credit
         : value + credit;
}
```

It can be pretty fast, but we can go further.

### Template specialization approach

Cook's proposal was:

```cpp
enum class Side { Buy, Sell };

template<Side side>
float CalcPrice(float value, float credit);

template<>
float CalcPrice<Side::Buy>(float value, float credit) {
    return value - credit;
}

template<>
float CalcPrice<Side::Sell>(float value, float credit) {
    return value + credit;
}
```

There is absolutely no branching. The code is deterministic. It's either on the
buy side or the sell side. No other sides.

Let's see what we can do with `if constexpr`.

### `if constexpr` approach

```cpp
enum class Side { Buy, Sell };

template<Side side>
float CalcPrice(float value, float credit) {
    if constexpr (side == Side::Buy) {
        return value - credit;
    } else {
        return value + credit;
    }
}
```

It looks like there is branching with `if`. However, the branching is done in
*compile time*. The generated assembly will either return `value - credit` or
`value + credit`. There will be no branch at all. Effectively, we have achieved
the same code as the template specialization method, with a centralized, more
compact and readable code.

### Any other examples?

[This StackOverflow answer](https://stackoverflow.com/a/43434771) demonstrates
how `if constexpr` can be different from `if`. Their example is more on the
*functionality* side, while Carl Cook's example is on the *performance* side.

## `if consteval`

Do you remember that a `constexpr` function may execute either during compile
time or run time?

`if consteval` in C++23 will allow us to branch, depending on whether that
`constexpr` function is evaluating during compile time or run time.

```cpp
constexpr int f() {
    if consteval {
        return 69;
    } else {
        return 420;
    }
}

int main() {
    int a = f(); // a = 420
    constexpr int b = f(); // b = 69
}
```

