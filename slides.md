---
theme: seriph
background: asset/background.avif
title: Theory of Computation
info: From Lambda Calculus to Computer Application Practice
drawings:
  persist: false
transition: slide-left
mdc: true
routerMode: hash
---

# Theory of Computation

## From Lambda Calculus to Computer Application Practice

<div h-20 />

<div class="abs-b m-18">
  <div class="text-6">Zecyel</div>
  <div>2025.10.12 at Fudan University</div>
</div>

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/AIxMath/Lecture-TOC" target="_blank" class="slidev-icon-btn">
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

Today we are going to discuss the Lambda Calculus. We will explore how it originated from mathematics, crossed the bridge between mathematics and computer science, and achieved significant breakthroughs in the field of computing.

---

# Table of Contents

<!-- TOC Here -->

* Basics of Lambda Calculus
* Boolean Algebra and Natural Numbers
* Combinators and Fixed Points
* Type-Safe Lambda Calculus
* Type Systems and Safety Theorems
* Curry-Howard Correspondence

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

Important: Free variables cannot be renamed arbitrarily.

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

Now $f(x)=x+1$, $g(x)=x\times2$, let's calculate $(f\circ g)(3)$ in Lambda Calculus.

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

Note that $\text{PRED} \; 0 = 0$.

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

- $\mathbf{I} = \lambda x.x$ <span color-gray>(Identity)</span>
- $\mathbf{K} = \lambda xy.x$ <span color-gray>(Constant)</span>
- $\mathbf{S} = \lambda xyz.xz(yz)$ <span color-gray>(Substitution)</span>

<div h-3 />

### Why are they important?

Combinators can express any lambda calculus expression without using variable names!

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

# The Problem: How Do We Implement Recursion?

In pure lambda calculus, functions have no names. How can a function call itself?

<div h-2 />

### Consider the factorial function:

$$\text{fact}(n) = \begin{cases} 1 & \text{if } n = 0 \\ n \times \text{fact}(n-1) & \text{otherwise} \end{cases}$$

<div h-2 />

### In lambda calculus:

$$\text{FACT} = \lambda n. \text{IF} \; (\text{ISZERO} \; n) \; 1 \; (\text{MULT} \; n \; (\text{FACT} \; (\text{PRED} \; n)))$$

<div h-3 />

<div text-center text-2xl color-red>
But wait! We cannot reference FACT inside its own definition!
</div>

---

# Fixed Points: The Mathematical Foundation

A fixed point of a function $f$ is a value $x$ such that $f(x) = x$.

<div h-2 />

### Examples in ordinary mathematics:

- $f(x) = x^2$ has fixed points: $x = 0$ and $x = 1$
- $f(x) = \cos(x)$ has a fixed point around $x \approx 0.739$

<div h-3 />

### In lambda calculus context:

If $F$ is a function that takes a function as input, then $Y \; F$ should be a fixed point of $F$:

$$F \; (Y \; F) = Y \; F$$

<div h-2 />

This means $Y \; F$ calls $F$ with itself as an argument, enabling recursion!

We define $\mathbf{Y} = \lambda f.(\lambda x.f \; (x \; x)) \; (\lambda x.f \; (x \; x))$ without providing a proof here.

---

# Implementing Factorial with Y Combinator

<div h-2 />

### Step 1: Define the factorial template

$$\text{FACT-TEMPLATE} = \lambda \text{fact}. \lambda n. \text{IF} \; (\text{ISZERO} \; n) \; 1 \; (\text{MULT} \; n \; (\text{fact} \; (\text{PRED} \; n)))$$

### Step 2: Apply Y combinator

$$\text{FACTORIAL} = \mathbf{Y} \; \text{FACT-TEMPLATE}$$

### Step 3: Compute factorial of 3

$$
\begin{align}
\text{FACTORIAL} \; 3 &= \mathbf{Y} \; \text{FACT-TEMPLATE} \; 3 \\
&= \text{FACT-TEMPLATE} \; (\mathbf{Y} \; \text{FACT-TEMPLATE}) \; 3 \\
&= \text{IF} \; (\text{ISZERO} \; 3) \; 1 \; (\text{MULT} \; 3 \; ((\mathbf{Y} \; \text{FACT-TEMPLATE}) \; 2)) \\
&= \text{MULT} \; 3 \; ((\mathbf{Y} \; \text{FACT-TEMPLATE}) \; 2) \\
&= \text{MULT} \; 3 \; (\text{MULT} \; 2 \; ((\mathbf{Y} \; \text{FACT-TEMPLATE}) \; 1)) \\
&= \text{MULT} \; 3 \; (\text{MULT} \; 2 \; (\text{MULT} \; 1 \; 1)) = 6
\end{align}
$$

---

# The Problem with Untyped Lambda Calculus

While pure lambda calculus is powerful, it allows nonsensical computations.

### Examples of problematic expressions:

$$(\lambda x.x \; x) \; (\lambda x.x \; x)$$

This creates infinite loops that never terminate.

$$\text{TRUE} \; 3 \; (\lambda y.y)$$

This applies a boolean to a number and a function, which is meaningless.

### The core issues:

- No way to prevent ill-formed expressions
- Functions can be applied to inappropriate arguments
- No static guarantees about program behavior

<div h-3 />

<div text-center text-3xl color-blue>
Type Systems provide the solution!
</div>

---

# What is a Type System?

A type system is a formal framework that assigns types to expressions and enforces constraints on how they can be combined.

<div h-3 />

### Goals of Type Systems:

- Safety: Prevent runtime errors by catching type mismatches at compile time
- Documentation: Types serve as machine-checked documentation
- Optimization: Enable compiler optimizations based on type information
- Abstraction: Provide interfaces that hide implementation details

<div h-3 />

### Types as Logical Propositions:

The Curry-Howard correspondence shows that:
- Types correspond to logical propositions
- Programs correspond to proofs
- Type checking corresponds to proof verification

---

# Simple Types in Lambda Calculus

<div h-2 />

### Type Syntax:

$$
\begin{align}
\tau ::= \;&\alpha & \text{(type variable)} \\
     |  \;&\tau_1 \to \tau_2 & \text{(function type)}
\end{align}
$$

### Examples:

$$
\begin{align}
\alpha &\quad \text{some base type} \\
\alpha \to \alpha &\quad \text{identity function type} \\
\alpha \to \beta \to \alpha &\quad \text{constant function type} \\
(\alpha \to \beta) \to \alpha \to \beta &\quad \text{function application type}
\end{align}
$$

### Interpretation:

- $\tau_1 \to \tau_2$ represents functions that take inputs of type $\tau_1$ and return outputs of type $\tau_2$
- Function types are right-associative: $\alpha \to \beta \to \gamma$ means $\alpha \to (\beta \to \gamma)$

---

# Typed Lambda Calculus Syntax

<div h-1 />

### Term Syntax (with type annotations):

$$
\begin{align}
M ::= \;&x & \text{(variable)} \\
     |  \;&\lambda x:\tau.M & \text{(typed abstraction)} \\
     |  \;&M \; N & \text{(application)}
\end{align}
$$

### Type Environments:

A type environment $\Gamma$ is a finite mapping from variables to types:

$$\Gamma ::= \emptyset \;|\; \Gamma, x:\tau$$

<div h-1 />

### Examples:

$$
\begin{align}
\lambda x:\alpha.x &\quad \text{identity function with explicit type} \\
\lambda f:(\alpha \to \beta).\lambda x:\alpha.f \; x &\quad \text{application function} \\
\lambda x:\alpha.\lambda y:\beta.x &\quad \text{constant function (K combinator)}
\end{align}
$$

---

# Type Inference Rules

The typing judgment $\Gamma \vdash M : \tau$ means "in environment $\Gamma$, term $M$ has type $\tau$".

<div h-2 />

### Variable Rule:
$$\frac{x:\tau \in \Gamma}{\Gamma \vdash x : \tau} \;\text{(VAR)}$$

### Abstraction Rule:
$$\frac{\Gamma, x:\tau_1 \vdash M : \tau_2}{\Gamma \vdash \lambda x:\tau_1.M : \tau_1 \to \tau_2} \;\text{(ABS)}$$

### Application Rule:
$$\frac{\Gamma \vdash M : \tau_1 \to \tau_2 \quad \Gamma \vdash N : \tau_1}{\Gamma \vdash M \; N : \tau_2} \;\text{(APP)}$$

<div h-2 />

These rules form a complete type system for the simply typed lambda calculus.

---

# Type Derivation Examples

<div h-1 />

### Example 1: Identity Function

$$
\frac{x:\alpha \in x:\alpha}{x:\alpha \vdash x : \alpha} \;\text{(VAR)}
$$

$$
\frac{x:\alpha \vdash x : \alpha}{\emptyset \vdash \lambda x:\alpha.x : \alpha \to \alpha} \;\text{(ABS)}
$$

<div h-2 />

### Example 2: Function Application

$$
\frac{
  \frac{f:\alpha \to \beta \in f:\alpha \to \beta, x:\alpha}{f:\alpha \to \beta, x:\alpha \vdash f : \alpha \to \beta} \;\text{(VAR)}
  \quad
  \frac{x:\alpha \in f:\alpha \to \beta, x:\alpha}{f:\alpha \to \beta, x:\alpha \vdash x : \alpha} \;\text{(VAR)}
}{f:\alpha \to \beta, x:\alpha \vdash f \; x : \beta} \;\text{(APP)}
$$

$$
\frac{f:\alpha \to \beta, x:\alpha \vdash f \; x : \beta}{f:\alpha \to \beta \vdash \lambda x:\alpha.f \; x : \alpha \to \beta} \;\text{(ABS)}
$$

$$
\frac{f:\alpha \to \beta \vdash \lambda x:\alpha.f \; x : \alpha \to \beta}{\emptyset \vdash \lambda f:(\alpha \to \beta).\lambda x:\alpha.f \; x : (\alpha \to \beta) \to \alpha \to \beta} \;\text{(ABS)}
$$

---

# Typing Church Encodings

Now we can assign proper types to our Church encodings.

### Church Booleans:

$$
\begin{align}
\text{TRUE} &: \alpha \to \beta \to \alpha \\
\text{FALSE} &: \alpha \to \beta \to \beta \\
\text{IF} &: (\alpha \to \beta \to \gamma) \to \alpha \to \beta \to \gamma \\
\text{AND} &: (\alpha \to \beta \to \alpha) \to (\alpha \to \beta \to \alpha) \to (\alpha \to \beta \to \alpha)
\end{align}
$$

### Church Numerals:

$$
\begin{align}
0, 1, 2, \ldots &: (\alpha \to \alpha) \to \alpha \to \alpha \\
\text{SUCC} &: ((\alpha \to \alpha) \to \alpha \to \alpha) \to (\alpha \to \alpha) \to \alpha \to \alpha \\
\text{ADD} &: ((\alpha \to \alpha) \to \alpha \to \alpha) \to ((\alpha \to \alpha) \to \alpha \to \alpha) \to (\alpha \to \alpha) \to \alpha \to \alpha
\end{align}
$$

Notice that Church numerals all have the same type structure!

---

# What Cannot Be Typed?

### Self-Application

$$\lambda x.x \; x$$

This would require $x$ to have type $\tau \to \tau'$ and simultaneously type $\tau$, which is impossible unless we allow infinite types.

### Fixed-Point Combinator

$$\mathbf{Y} = \lambda f.(\lambda x.f \; (x \; x)) \; (\lambda x.f \; (x \; x))$$

The $\mathbf{Y}$ combinator cannot be typed because it also involves self-application.

### Implications

- Simply typed lambda calculus is strongly normalizing: all well-typed programs terminate
- We cannot express general recursion in simply typed lambda calculus
- This is both a limitation (less expressiveness) and a feature (guaranteed termination)

---

# Type Safety Theorems

The simply typed lambda calculus satisfies two fundamental safety properties.

### Preservation (Subject Reduction): Evaluation preserves types.

If $\Gamma \vdash M : \tau$ and $M \rightarrow_\beta N$, then $\Gamma \vdash N : \tau$.

### Progress: Well-typed closed terms either are values or can take a step.

If $\emptyset \vdash M : \tau$, then either:
1. $M$ is a value (normal form), or  
2. There exists $N$ such that $M \rightarrow_\beta N$.

<div h-3 />

### Combined Type Safety

Together, these theorems guarantee that well-typed programs cannot "go wrong" - they either terminate with a value of the expected type or diverge, but never encounter runtime type errors.

---

# Extensions to Simply Typed Lambda Calculus

The simply typed lambda calculus forms the foundation for many extensions:

### Polymorphism (System F):

$$\Lambda \alpha.\lambda x:\alpha.x \quad : \quad \forall \alpha.\alpha \to \alpha$$

Allows functions that work with multiple types.

### Dependent Types:

$$\lambda n:\text{Nat}.\lambda v:\text{Vec}(\alpha, n).\text{head}(v) \quad : \quad \Pi n:\text{Nat}.\text{Vec}(\alpha, n) \to \alpha$$

Types that depend on values.

### Recursive Types:

$$\mu \alpha.\alpha \to \alpha$$

Allows self-referential types, enabling recursion while maintaining type safety.

---

<div h-45 />

<div text-center text-5xl>Go Across the Math-Computer Bridge!</div>

---

# The Curry-Howard Correspondence

One of the most profound discoveries in logic and computer science.

<div h-1 />

<div text-3xl text-blue text-center>Types are propositions, programs are proofs.</div>

<div h-4 />

### Historical Background

- Haskell Curry (1934): Observed similarity between combinatory logic and propositional logic

- William Howard (1969): Made the connection precise for lambda calculus and natural deduction

- Also known as the Propositions-as-Types Interpretation

<div h-3 />

### The Core Insight

There is a perfect correspondence between constructive proofs in logic and programs in typed lambda calculus.

---

# The Basic Correspondence

<div h-1 />

### Fundamental Mappings:

| Logic | Type Theory | Lambda Calculus |
|-------|-------------|-----------------|
| Proposition $A$ | Type $\tau$ | Type $\tau$ |
| Proof of $A$ | Term of type $\tau$ | $M : \tau$ |
| Assumption | Variable declaration | $x : \tau$ |
| Implication $A \to B$ | Function type $\tau_1 \to \tau_2$ | $\lambda x:\tau_1.M:\tau_2$ |

### Modus Ponens: From $A$ and $A \to B$, derive $B$

$$
\frac{\Gamma \vdash M : A \quad \Gamma \vdash N : A \to B}{\Gamma \vdash N \; M : B}
$$

The logical rule and the typing rule have identical structure!

---

# Proofs as Programs: Examples

<div h-1 />

### Identity: $A \to A$

Logical proof: "If we assume $A$, then we have $A$"

Program: $\lambda x:A.x \quad : \quad A \to A$

### Composition: $(A \to B) \to (B \to C) \to (A \to C)$

Logical proof: "If we can prove $A \to B$ and $B \to C$, then we can prove $A \to C$"

Program: $\lambda f:(A \to B).\lambda g:(B \to C).\lambda x:A.g \; (f \; x)$

### Constant Function: $A \to B \to A$

Logical proof: "If we have $A$ and $B$, then we still have $A$"

Program: $\lambda x:A.\lambda y:B.x \quad : \quad A \to B \to A$

---

# Extending the Correspondence

<div h-5 />

### Logical AND $A \land B$ $\Rightarrow$ Product Types $A \times B$

- Constructor: $\langle M, N \rangle : A \times B$
- Eliminators: $\pi_1 : A \times B \to A$ and $\pi_2 : A \times B \to B$

<div h-5 />

### Logical OR $A \lor B$ $\Rightarrow$ Sum Types $A + B$

- Constructors: $\text{inl} : A \to A + B$ and $\text{inr} : B \to A + B$  
- Eliminator: Case analysis

<div h-5 />

### Truth and Falsehood

- True $\top$ corresponds to unit type $()$ (exactly one value)
- False $\bot$ corresponds to empty type $\mathbf{None}$ (no values)

---

# Logical Theorems as Typed Programs

<div h-5 />

### Simple Example: $(A \land B) \to (A \lor B)$

$$\lambda x:(A \times B).\text{inl}(\pi_1 \; x)$$

<div h-5 />

### Distributivity: $A \land (B \lor C) \to (A \land B) \lor (A \land C)$

$$\lambda p:A \times (B + C).\text{case} \; (\pi_2 \; p) \; \text{of} \; (\text{inl} \; b \Rightarrow \text{inl} \; \langle \pi_1 \; p, b \rangle \;|\; \text{inr} \; c \Rightarrow \text{inr} \; \langle \pi_1 \; p, c \rangle)$$

<div h-5 />

### Extractivity: $(A \land B) \lor (A \land C) \to A \land (B \lor C)$

$$\lambda x: (A \times B) + (A \times C).\text{case } x \text{ of } (\text{inl } p \Rightarrow \langle \pi_1 \; p, \text{inl}(\pi_2 \; p) \rangle \; | \; \text{inr } q \Rightarrow \langle \pi_1 \; q, \text{inr}(\pi_2 \;q) \rangle)$$

---

# The Curry-Howard Correspondence Revisited

The connection between types and logic becomes even more profound.

| Logic | Programming | Type Theory |
|-------|-------------|-------------|
| Proposition | Type | Type |
| Proof | Program | Term |
| Implication $(A \to B)$ | Function type | $\tau_1 \to \tau_2$ |
| Conjunction $(A \land B)$ | Product type | $\tau_1 \times \tau_2$ |
| Disjunction $(A \lor B)$ | Sum type | $\tau_1 + \tau_2$ |
| True proposition | Unit type | $()$ |
| False proposition | Empty type | $\mathbf{None}$ |

<div h-2 />

### Modern Applications:

- Proof assistants: Coq, Lean, Agda use this correspondence for formal verification
- Program synthesis: Generate programs from logical specifications
- Dependent types: Types that depend on values, enabling rich specifications

---

# Conclusion: From Mathematics to Computation

<div h-3 />

We have seen the journey of lambda calculus from pure mathematics to practical computing:

1. Pure Lambda Calculus: Captures the essence of computation through function abstraction and application

2. Church Encodings: Shows computational universality - anything computable can be expressed

3. Combinators: Demonstrate that even variable names are not essential

4. Type Systems: Add safety and structure while maintaining expressiveness

5. Modern Impact: Forms the theoretical foundation for programming language design and formal verification

<div h-3 />

<div text-center text-3xl color-blue>
Lambda calculus remains a powerful bridge between mathematical theory and computational practice.
</div>

---

<div h-44 />

<div text-7xl text-center>Thanks for listening.</div>
