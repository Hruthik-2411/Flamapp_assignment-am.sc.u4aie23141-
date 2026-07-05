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

The file **xy_data.csv** contains **1501 points** sampled from the above parametric curve. The objective is to estimate the values of **θ**, **M**, and **X** such that the generated curve best matches the given dataset.

---

# Thought Process

Initially, I considered estimating four unknowns:

- θ
- M
- X
- t

Since every point on the curve has its own value of **t**, directly searching for all values of **t** along with **θ**, **M**, and **X** would make the brute-force search computationally very expensive.

Instead of searching for **t**, I tried to derive an equation that could compute **t** directly from the given dataset. This would reduce the number of unknown parameters and make the search much more efficient.

---

# Mathematical Derivation

The original equations are

```text
x = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X

y = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

Move the constants to the left side.

```text
x - X = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ)

y - 42 = t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

Multiply the first equation by **cos(θ)**

```text
(x - X)cos(θ) = tcos²(θ) - e^(M|t|)sin(0.3t)sin(θ)cos(θ)
```

Multiply the second equation by **sin(θ)**

```text
(y - 42)sin(θ) = tsin²(θ) + e^(M|t|)sin(0.3t)sin(θ)cos(θ)
```

Now add both equations.

The exponential terms cancel each other.

```text
(x - X)cos(θ) + (y - 42)sin(θ) = t(cos²(θ) + sin²(θ))
```

Using the identity

```text
cos²(θ) + sin²(θ) = 1
```

we obtain

```text
t = (x - X)cos(θ) + (y - 42)sin(θ)
```

This is the key observation in the solution.

Since the dataset already contains the values of **x** and **y**, once a candidate value of **θ** and **X** is selected, the value of **t** for every point can be computed directly.

Therefore, there is no need to search for **t** separately.

The optimization problem is reduced from

- θ
- M
- X
- t₁
- t₂
- ...
- t₁₅₀₁

to only

- θ
- M
- X

which significantly reduces the search space.

---

# Brute Force Algorithm

After deriving the expression for **t**, the brute-force algorithm was implemented using the following steps.

### Step 1

Read the dataset from **xy_data.csv**.

Store all x-coordinates and y-coordinates separately.

---

### Step 2

Generate all possible values of

- θ
- M
- X

using the specified search ranges.

---

### Step 3

For every candidate value of **θ**

- Compute sin(θ)
- Compute cos(θ)

These values are reused throughout the iteration.

---

### Step 4

For every candidate value of **X**

Compute **t** for every point in the dataset.

```text
t = (x - X)cos(θ) + (y - 42)sin(θ)
```

If any computed value of **t** lies outside the valid range

```text
6 < t < 60
```

the current parameter combination is discarded.

---

### Step 5

For every candidate value of **M**

Substitute the computed **t** values into the original equations.

```text
x(t) = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X
```

```text
y(t) = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

This generates the predicted curve.

---

### Step 6

Compute the L1 distance between the predicted points and the original dataset.

```text
L1 Error = Σ ( |x_actual - x_predicted| + |y_actual - y_predicted| )
```

---

### Step 7

If the computed error is smaller than the previous best error,

store

- θ
- M
- X

as the current best solution.

---

### Step 8

Repeat the above process until every possible parameter combination has been evaluated.

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

The obtained error is extremely small, indicating that the predicted curve closely matches the given dataset.

---

# Final Estimated Equation

```text
x(t) = t*cos(30°) - e^(0.03|t|)*sin(0.3t)*sin(30°) + 55
```

```text
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

xy_data.csv                 -> Dataset

README.md                   -> Project documentation
```

---

# Conclusion

Instead of treating **t** as another unknown parameter, an analytical expression for **t** was derived from the given equations. Since the dataset already provides **x** and **y**, the value of **t** can be computed directly once **θ** and **X** are chosen.

This reduced the optimization problem from estimating thousands of unknown values to estimating only three unknown parameters (**θ**, **M**, and **X**). A brute-force search over these parameters successfully reconstructed the original curve with a very small L1 error.

---

# Author

**Hruthik Palivela**