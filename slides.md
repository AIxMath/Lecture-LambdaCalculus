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

If you've learnt or heard of Turing Machine, you probably know the Chomsky Hierarchy.

- Type 0: Recursively Enumerable (Turing Machine)
- Type 1: Context-Sensitive (Linear-Bounded Automaton)
- Type 2: Context-Free (Pushdown Automaton)
- Type 3: Regular (Finite Automaton)

There are two other models as powerful as Turing Machine -- the Lambda Calculus and Primitive Recursion.

Today we are going to discuss about the Lambda Calculus. We will explore how it originated from mathematics, crossed the bridge between mathematics and computer science, and achieved significant breakthroughs in the field of computing.

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
\lambda x.\lambda y.x^2+y^2\;\;\text{or}\;\;\lambda xy.x^2+y^2
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

<div h-35 />

<div text-center text-5xl>Let's Create the World</div>

<div h-8 />

<div text-center text-3xl color-gray>According to the Church-Turing thesis,</div>
<div h-1 />
<div text-center text-3xl color-gray>Lambda Calculus is equally powerful as Turing Machine.</div>

---

# Church Booleans

In pure lambda calculus, we need to represent boolean values using only functions.

### Definition

$$\text{TRUE} = \lambda xy.x$$
$$\text{FALSE} = \lambda xy.y$$

### Intuition

- `TRUE` takes two arguments and returns the first one
$$\text{TRUE} \; a \; b = (\lambda xy.x) \; a \; b \rightarrow_\beta a$$

- `FALSE` takes two arguments and returns the second one

$$\text{FALSE} \; a \; b = (\lambda xy.y) \; a \; b \rightarrow_\beta b$$

---

# Boolean Operations

Now we can define logical operations using Church booleans.

### AND Operation

$$\text{AND} = \lambda pq.pqp$$

### OR Operation  

$$\text{OR} = \lambda pq.ppq$$

### NOT Operation

$$\text{NOT} = \lambda p.p \; \text{FALSE} \; \text{TRUE}$$

### Example

$$\text{AND} \; \text{TRUE} \; \text{TRUE} = (\lambda pq.pqp) \; \text{TRUE} \; \text{TRUE} \rightarrow_\beta \text{TRUE} \; \text{TRUE} \; \text{TRUE} \rightarrow_\beta \text{TRUE}$$

$$\text{NOT} \; \text{TRUE} = (\lambda p.p \; \text{FALSE} \; \text{TRUE}) \; \text{TRUE} \rightarrow_\beta \text{TRUE} \; \text{FALSE} \; \text{TRUE} \rightarrow_\beta \text{FALSE}$$

---

# Conditional Logic: IF-THEN-ELSE

### Definition

$$\text{IF} = \lambda pab.pab$$

### Usage

$$\text{IF} \; \text{condition} \; \text{then-branch} \; \text{else-branch}$$

### Examples

$$
\begin{align}
\text{IF} \; \text{TRUE} \; x \; y &= (\lambda pab.pab) \; \text{TRUE} \; x \; y \\
&\rightarrow_\beta \text{TRUE} \; x \; y \\
&\rightarrow_\beta x
\end{align}
$$

$$
\begin{align}
\text{IF} \; \text{FALSE} \; x \; y &= (\lambda pab.pab) \; \text{FALSE} \; x \; y \\
&\rightarrow_\beta \text{FALSE} \; x \; y \\
&\rightarrow_\beta y
\end{align}
$$

---

# Natural Numbers: Church Numerals

### Definition

$$0 = \lambda fx.x$$
$$1 = \lambda fx.f \; x$$  
$$2 = \lambda fx.f \; (f \; x)$$
$$3 = \lambda fx.f \; (f \; (f \; x))$$
$$\vdots$$
$$n = \lambda fx.\underbrace{f \; (f \; (\cdots (f}_{n \text{ times}} \; x) \cdots))$$

### Intuition

A Church numeral $n$ is a function that applies its first argument $f$ exactly $n$ times to its second argument $x$.

---

# Church Numeral Operations

<div h-1 />

### Definition

$$\text{SUCC} = \lambda nfx.f \; (n \; f \; x)$$

$$\text{ADD} = \lambda mnfx.m \; f \; (n \; f \; x)$$

$$\text{MULT} = \lambda mnf.m \; (n \; f)$$

$$\text{EXP} = \lambda bn.n \; b$$

### Examples

$$\text{SUCC} \; 0 = (\lambda nfx.f \; (n \; f \; x)) \; (\lambda fx.x) \rightarrow_\beta \lambda fx.f \; x = 1$$

$$
\begin{align}
\text{ADD} \; 2 \; 3 &\rightarrow_\beta \lambda fx.2 \; f \; (3 \; f \; x) \\
&= \lambda fx.(\lambda fx.f(f(x)))\; f \; (\lambda fx.f(f(f(x))) \; f \; x) \\
&= \lambda fx.(\lambda fx.f(f(x)))\; f \; f(f(f(x))) \\
&= \lambda fx.f \; (f \; (f \; (f \; (f \; x)))) = 5
\end{align}$$

---

# Predecessor and Zero Test

These operations are more complex and showcase the power of lambda calculus.

### Predecessor

$$\text{PRED} = \lambda nfx.n \; (\lambda gh.h \; (g \; f)) \; (\lambda u.x) \; (\lambda u.u)$$

Noticed that $\text{PRED} \; 0 = 0$.

### Zero Test

$$\text{ISZERO} = \lambda n.n \; (\lambda x.\text{FALSE}) \; \text{TRUE}$$

With predecessor and zero test, we can run many complicated loops.

---

# We can do anything... Wait?

<div h-5 />

## Wait... Do we really need variables?

<div h-2 />

Consider the identity function:

<div text-3xl>
$$
\lambda x.x
$$
</div>

<div border="2 green rounded" v-drag="[459,229,40,48]" />

<div border="2 purple rounded" v-drag="[503,229,40,48]" />

<div v-drag="[405,278,143,26]" color-green>placeholder</div>

<div v-drag="[505,278,229,26]" color-purple>refers to placeholder</div>

<div h-3 />

Can we express functions without any variables at all?

<div h-3 />

<div text-center text-4xl>
Yes! Using <span color-blue>Combinators</span>
</div>

---

# What are Combinators?

A combinator is a lambda expression with no free variables.

<div h-2 />

### Examples

<v-clicks>

- $\mathbf{I} = \lambda x.x$ <span color-gray>(Identity)</span>
- $\mathbf{K} = \lambda xy.x$ <span color-gray>(Constant)</span>
- $\mathbf{S} = \lambda xyz.xz(yz)$ <span color-gray>(Substitution)</span>

</v-clicks>

<div h-3 />

<v-click>

### Why are they important?

Combinators can express any lambda calculus expression without using variable names!

</v-click>

---

# The SKI Combinator System

Three simple combinators can express all of lambda calculus:

<div h-2 />

| Combinator | Definition | Behavior |
|------------|------------|----------|
| $\mathbf{I}$ | $\lambda x.x$ | Identity: returns its argument |
| $\mathbf{K}$ | $\lambda xy.x$ | Constant: returns the first argument, ignores the second |
| $\mathbf{S}$ | $\lambda xyz.xz(yz)$ | Substitution: applies $x$ to $z$ and applies $y$ to $z$, then applies the results |

<div h-3 />

### Fun Fact

The $\mathbf{I}$ combinator is actually redundant! We can derive it from $\mathbf{S}$ and $\mathbf{K}$:

$$\mathbf{I} = \mathbf{SKK}$$

Let's verify: $\mathbf{SKK} \; a = \mathbf{S} \; \mathbf{K} \; \mathbf{K} \; a \rightarrow_\beta \mathbf{K} \; a \; (\mathbf{K} \; a) \rightarrow_\beta a$

---

# More Famous Combinators

Beyond SKI, there are many other useful combinators with special purposes:

<div h-2 />

| Name | Definition | Purpose |
|------|------------|---------|
| $\mathbf{B}$ | $\lambda xyz.x(yz)$ | Function composition |
| $\mathbf{C}$ | $\lambda xyz.xzy$ | Argument flip |
| $\mathbf{W}$ | $\lambda xy.xyy$ | Duplicate argument |
| $\mathbf{Y}$ | $\lambda f.(\lambda x.f(xx))(\lambda x.f(xx))$ | Fixed-point (recursion!) |
| $\omega$ | $\lambda x.xx$ | Self-application |
| $\Omega$ | $(\lambda x.xx)(\lambda x.xx)$ | Non-terminating computation |

---

# From Lambda Calculus to Combinators

Elimination Algorithm: Any lambda expression can be systematically converted to pure combinators.

<div h-2 />

### Rules

1. $T[x] = x$ <span color-gray text-sm>(variable)</span>
2. $T[E_1 \; E_2] = T[E_1] \; T[E_2]$ <span color-gray text-sm>(application)</span>
3. $T[\lambda x.x] = \mathbf{I}$ <span color-gray text-sm>(identity)</span>
4. $T[\lambda x.E] = \mathbf{K} \; T[E]$ if $x$ not free in $E$ <span color-gray text-sm>(constant)</span>
5. $T[\lambda x.E_1 \; E_2] = \mathbf{S} \; T[\lambda x.E_1] \; T[\lambda x.E_2]$ <span color-gray text-sm>(substitution)</span>

<div h-2 />

### Example: Converting $\lambda xy.x$

$$
\begin{align}
T[\lambda xy.x] &= T[\lambda x.\lambda y.x] \\
&= T[\lambda x.(\mathbf{K} \; x)] \\
&= \mathbf{S} \; T[\lambda x.\mathbf{K}] \; T[\lambda x.x] \\
&= \mathbf{S} \; (\mathbf{K} \; \mathbf{K}) \; \mathbf{I} \\
&= \mathbf{K}
\end{align}
$$

---

