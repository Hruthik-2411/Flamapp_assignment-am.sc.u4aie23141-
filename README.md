# AI Research & Development Assignment

## Problem Statement

The objective of this assignment is to estimate the unknown parameters of the following parametric curve.

```text
x(t) = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X

y(t) = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

### Unknown Parameters

- θ
- M
- X

### Parameter Range

- 0° < θ < 50°
- -0.05 < M < 0.05
- 0 < X < 100
- 6 < t < 60

The file **xy_data.csv** contains **1501 points** sampled from the above parametric curve. The objective is to estimate the unknown values of **θ**, **M**, and **X** such that the generated curve matches the given dataset as closely as possible.

---

# Thought Process

At the beginning, I considered all the unknown quantities in the problem.

The unknowns are:

- θ
- M
- X
- t

Since the dataset contains **1501 points**, every point has its own value of **t**. Therefore, directly searching for all values of **t** together with **θ**, **M**, and **X** would make the search space extremely large.

Instead of brute-forcing **t**, I tried to determine whether it could be expressed using the given equations. If that was possible, the optimization problem would become much simpler.

---

# Mathematical Derivation

The original equations are

```text
x = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X

y = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

First, move the constant terms to the left.

```text
x - X = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ)

y - 42 = t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

Now multiply the first equation by **cos(θ)**

```text
(x - X)cos(θ)

=

t*cos²(θ)

-

e^(M|t|)sin(0.3t)sin(θ)cos(θ)
```

Multiply the second equation by **sin(θ)**

```text
(y - 42)sin(θ)

=

t*sin²(θ)

+

e^(M|t|)sin(0.3t)sin(θ)cos(θ)
```

Adding the two equations,

the exponential terms cancel each other.

```text
(x - X)cos(θ)

+

(y - 42)sin(θ)

=

t(cos²(θ)+sin²(θ))
```

Using the trigonometric identity

```text
cos²(θ)+sin²(θ)=1
```

the equation becomes

```text
t = (x - X)cos(θ) + (y - 42)sin(θ)
```

This is the key result.

The dataset already provides **x** and **y** values.

Therefore, whenever the brute-force algorithm selects a candidate value of **θ** and **X**, the corresponding **t** value for every point can be computed directly.

As a result, **t no longer needs to be treated as an independent unknown.**

The optimization problem is therefore reduced to only

- θ
- M
- X

instead of

- θ
- M
- X
- t₁
- t₂
- ...
- t₁₅₀₁

This significantly reduces the search space.

---

# Brute Force Algorithm

After deriving the expression for **t**, the brute-force algorithm was implemented as follows.

### Step 1

Read all the points from **xy_data.csv**.

Store the x-coordinates and y-coordinates separately.

---

### Step 2

Generate candidate values for

- θ
- M
- X

using the predefined search ranges.

---

### Step 3

For every candidate value of **θ**

- compute sin(θ)
- compute cos(θ)

These values are reused throughout the iteration.

---

### Step 4

For every candidate value of **X**

compute the value of **t** for all dataset points using

```text
t = (x - X)cos(θ) + (y - 42)sin(θ)
```

If any value of **t** lies outside the valid range

```text
6 < t < 60
```

that parameter combination is rejected.

---

### Step 5

For every candidate value of **M**

substitute the computed **t** values into the original parametric equations.

```text
x(t)

=

t*cos(θ)

-

e^(M|t|)sin(0.3t)sin(θ)

+

X
```

```text
y(t)

=

42

+

t*sin(θ)

+

e^(M|t|)sin(0.3t)cos(θ)
```

This generates the predicted curve.

---

### Step 6

Compute the L1 distance between the predicted points and the original dataset.

```text
L1 Error

=

Σ

(

|x_actual-x_predicted|

+

|y_actual-y_predicted|

)
```

---

### Step 7

If the computed error is smaller than the best error found so far,

store

- θ
- M
- X

as the current best solution.

---

### Step 8

Repeat the above process until every parameter combination has been evaluated.

The parameter combination with the minimum L1 error is selected as the final solution.

---

# Estimated Parameters

| Parameter | Estimated Value |
|-----------|----------------:|
| θ | **30°** |
| M | **0.03** |
| X | **55** |

---

# Error Analysis

**Total L1 Error**

```text
0.030834
```

**Average Error Per Point**

```text
0.0000205
```

The obtained error is very small, indicating that the predicted curve closely matches the given dataset.

---

# Final Estimated Equation

```text
x(t) = t*cos(30°) - e^(0.03|t|)*sin(0.3t)*sin(30°) + 55

y(t) = 42 + t*sin(30°) + e^(0.03|t|)*sin(0.3t)*cos(30°)
```

Parameter Range

```text
6 ≤ t ≤ 60
```

---

# Repository Contents

```text
bruteforce_approach.ipynb   -> Complete implementation

xy_data.csv                 -> Input dataset

README.md                   -> Project documentation
```

---

# Conclusion

Instead of treating **t** as an additional unknown parameter, an analytical expression for **t** was derived from the original equations. Since the dataset already contains the values of **x** and **y**, the corresponding value of **t** can be computed directly once **θ** and **X** are selected.

This reduced the optimization problem from estimating thousands of unknown values to estimating only three unknown parameters (**θ**, **M**, and **X**). A brute-force search over these parameters successfully reconstructed the original curve with a very small L1 error.

---

# Author

**Hruthik Palivela**