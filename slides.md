---
theme: seriph
background: asset/background.avif
title: Theory of Computation
info: From Lambda Calculus to Computer Application Practice
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Theory of Computation

## From Lambda Calculus to Computer Application Practice

<div h-20 />

<div class="abs-b m-18">
  <div class="text-6">Zecyel</div>
  <div>2025.10.12 at Fudan University</div>
</div>

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/AIxMath/Lecture-LambdaCalculus" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---

# What is the universal model of computation?

<div h-5 />

If you've learnt or heard of "Turing Machine", you probably know the "Chomsky Hierarchy".

- Type 0: Recursively Enumerable (Turing Machine)
- Type 1: Context-Sensitive (Linear-Bounded Automaton)
- Type 2: Context-Free (Pushdown Automaton)
- Type 3: Regular (Finite Automaton)

There are two other models as powerful as "Turing Machine" -- the "Lambda Calculus" and "Primitive Recursion".

Today we are going to discuss about the "Lambda Calculus". We will explore how it originated from mathematics, crossed the bridge between mathematics and computer science, and achieved significant breakthroughs in the field of computing.

---

# Table of Content

<!-- TOC Here -->

* Basics of Lambda Calculus
* Boolean Algebra and Natural Number
* Combinators
* Appliances in Lean
* Curry-Howard Correspondence
* Type Systems in Programming Languages

---

# The Abstraction Problem in Mathematics

<div h-3 />

What Does $f(x) = x + 1$ Really Mean?

<div text-3xl>
$$
f(x)=x+1
$$
</div>

<div border="2 red rounded" v-drag="[376,181,40,48]" />

<div border="2 blue rounded" v-drag="[418,181,40,48]" />

<div border="2 yellow rounded" v-drag="[501,181,96,48]" />

<div v-drag="[295,234,143,26]" color-red>arbitrary name</div>

<div v-drag="[417,234,121,26]" color-blue>placeholder</div>

<div v-drag="[515,234,143,26]" color-yellow>computation</div>

<div h-10 />

Thus, we need a simpler way to express "the function that takes an input and adds 1 to it".

Can we represent the "essence" of this function without relying on names?

---

# Lambda Notation: The Solution

<div h-3 />

In Lambda Calculus, $f(x) = x + 1$ is quite simple:

<div border="2 blue rounded" v-drag="[428,181,40,48]" />

<div border="2 yellow rounded" v-drag="[471,181,91,48]" />

<div v-drag="[396,234,121,26]" color-blue>variables</div>

<div v-drag="[473,234,143,26]" color-yellow>computation</div>

<div text-3xl>
$$
\lambda x.x+1
$$
</div>

<div h-2 />

What about a function of two variables?

<div text-3xl>
$$
\lambda x.\lambda y.x^2+y^2\;\;\mathbf{or}\;\;\lambda xy.x^2+y^2
$$
</div>

Simple. Also Effective!

---

# Formal Definition of Lambda Calculus

<div h-4 />

| Syntax | Semantics |
| ------ | --------- |
| $x, y, z, \cdots$ | variables |
| $\lambda x.E$ | function definition |
| $E_1 E_2$ | function call |

## Examples

- $\lambda x. x$ $\rightarrow$ identity function
- $\lambda x. \lambda y. x$ $\rightarrow$ constant function (also written as $\lambda xy. x$)
- $(\lambda x. x + 1) 5$ $\rightarrow$ function application

---

# Basics of Lambda Calculus -- Beta Reduction

<div h-3 />

Beta reduction is the fundamental computation rule in lambda calculus. It replaces bound variables with their arguments.

### Rule

$$(\lambda x.E) M \rightarrow_\beta E[x := M]$$

### Examples

$$
(\lambda x.x) \; 5 \rightarrow_\beta 5
$$

$$
(\lambda x.x + 1) \; 3 \rightarrow_\beta 3 + 1 = 4
$$

$$
(\lambda x.\lambda y.x + y) \; 2 \; 3 \rightarrow_\beta (\lambda y.2 + y) \; 3 \rightarrow_\beta 2 + 3 = 5
$$

---

# Basics of Lambda Calculus -- Alpha Equivalence

<div h-3 />

Alpha equivalence allows renaming bound variables without changing the function's meaning.

### Rule

$$\lambda x.E \equiv_\alpha \lambda y.E[x := y] \quad\text{(if} \;y\; \text{is not free in}\; E \text{)}$$


### Examples

$$
\lambda x.x \equiv_\alpha \lambda y.y
$$

$$
\lambda x.\lambda y.x + y \equiv_\alpha \lambda a.\lambda b.a + b
$$

$$
\lambda x.x + z \equiv_\alpha \lambda y.y + z \quad \text{(z remains free)}
$$

**Important**: Free variables cannot be renamed arbitrarily.

---

# Basics of Lambda Calculus -- Currying

<div h-1 />

Currying transforms multi-argument functions into sequences of single-argument functions.

### Principle

$$f(x, y) = f(x)(y)$$

### Examples

$$
\text{Uncurried:} \; \lambda xy.x + y
$$

$$
\text{Curried:} \; \lambda x.\lambda y.x + y
$$

### Application

$$
\begin{align}
(\lambda xy.x + y) \; 3 \; 2 &= (\lambda x.\lambda y.x + y) \; 3 \; 2 \\
&\rightarrow_\beta (\lambda y.3 + y) \; 2 \\
&\rightarrow_\beta 3 + 2 = 5
\end{align}
$$

---

# One More Example

<div />

Now $f(x)=x+1$, $g(x)=x\times2$, let's calculate $(f\circ g)(3)$ in Lambda Calculas.

$$\text{compose} = \lambda fgx.f(g(x))$$
$$\text{double} = \lambda y.y \times 2$$
$$\text{increment} = \lambda z.z + 1$$

$$
\begin{align}
&\text{compose} \; \text{increment} \; \text{double} \; 3 \\
&= (\lambda fgx.f(g(x))) \; (\lambda z.z + 1) \; (\lambda y.y \times 2) \; 3 \\
&\rightarrow_\beta (\lambda gx.(\lambda z.z + 1)(g(x))) \; (\lambda y.y \times 2) \; 3 \\
&\rightarrow_\beta (\lambda x.(\lambda z.z + 1)((\lambda y.y \times 2)(x))) \; 3 \\
&\rightarrow_\beta (\lambda z.z + 1)((\lambda y.y \times 2)(3)) \\
&\rightarrow_\beta (\lambda z.z + 1)(3 \times 2) \\
&\rightarrow_\beta (\lambda z.z + 1)(6) \\
&\rightarrow_\beta 6 + 1 = 7
\end{align}
$$

---
