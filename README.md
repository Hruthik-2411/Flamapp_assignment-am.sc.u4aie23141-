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

The file **xy_data.csv** contains 1501 points sampled from the above parametric curve.

---

# Approach

Initially, I treated the problem as estimating four unknowns:

- θ
- M
- X
- t

Since every point on the curve has its own value of **t**, directly searching for all of them would make the brute-force search computationally expensive.

After analyzing the given equations, I derived an analytical expression for **t** by eliminating the exponential term.

Starting from

```text
x = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X

y = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

the following expression was obtained:

```text
t = (x - X)*cos(θ) + (y - 42)*sin(θ)
```

This observation is important because the dataset already provides the values of **x** and **y**.

Therefore, once a candidate value of **θ** and **X** is selected during the brute-force search, the corresponding value of **t** for every point can be computed directly from the dataset.

As a result, there is no need to search for **t** separately.

This reduces the optimization problem to only three unknown parameters:

- θ
- M
- X

---

# Brute Force Algorithm

The algorithm performs an exhaustive search over the possible values of:

- θ
- M
- X

For every parameter combination, the following steps are executed:

1. Select a candidate value of θ.
2. Select a candidate value of X.
3. Compute the value of **t** for every point in the dataset using

```text
t = (x - X)*cos(θ) + (y - 42)*sin(θ)
```

4. Substitute the computed values of **t** back into the original parametric equations.
5. Generate the predicted values of **x** and **y**.
6. Compute the L1 distance between the predicted points and the original dataset.
7. Store the parameter combination with the minimum error.

---

# Error Function

The objective function used is the L1 distance.

```text
L1 Error = Σ ( |x_actual - x_predicted| + |y_actual - y_predicted| )
```

The parameter combination producing the minimum L1 error is selected as the final solution.

---

# Estimated Parameters

| Parameter | Estimated Value |
|-----------|----------------:|
| θ | **30°** |
| M | **0.03** |
| X | **55** |

---

# Error

Total L1 Error

```text
0.030834
```

Average Error Per Point

```text
0.0000205
```

The obtained error is extremely small, indicating that the predicted curve almost perfectly overlaps the original curve.

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

```
bruteforce_approach.ipynb   -> Complete implementation
xy_data.csv                 -> Dataset
README.md                   -> Documentation
```

---

# Observations

- A brute-force search was used to estimate the unknown parameters.
- Instead of treating **t** as an independent unknown, an analytical expression was derived from the original equations.
- Since the dataset already contains the values of **x** and **y**, once **θ** and **X** are selected, the corresponding **t** values can be computed directly.
- This significantly reduces the search space because only **θ**, **M**, and **X** need to be optimized.
- The estimated parameters reconstruct the given curve with a very small L1 error.

---

# Author

**Hruthik Palivela**