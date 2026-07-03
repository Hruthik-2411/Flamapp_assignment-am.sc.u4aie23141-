# AI Research & Development Assignment

## Problem Statement

The goal of this assignment is to estimate the unknown parameters of the following parametric curve.

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

The file **xy_data.csv** contains points sampled from this parametric curve.

---

# Approach

At first, I considered estimating four unknowns:

- θ
- M
- X
- t

Since every point has its own value of **t**, directly searching for all of them would make the brute-force search extremely expensive.

To reduce the search space, I derived an analytical expression for **t** from the given equations.

Starting from

```text
x = t*cos(θ) - e^(M|t|)*sin(0.3t)*sin(θ) + X

y = 42 + t*sin(θ) + e^(M|t|)*sin(0.3t)*cos(θ)
```

I obtained

```text
t = (x - X)*cos(θ) + (y - 42)*sin(θ)
```

Using this equation, the value of **t** can be computed directly for every point.

This reduces the optimization problem to only three unknown parameters:

- θ
- M
- X

---

# Brute Force Algorithm

For every possible combination of:

- θ
- M
- X

the following steps are performed:

1. Compute `t` using the derived equation.
2. Substitute `t` back into the original parametric equations.
3. Compute the predicted `(x, y)` values.
4. Calculate the L1 distance between the predicted points and the original points.
5. Store the parameter set with the minimum error.

The L1 error is calculated as

```text
Σ ( |x_actual - x_predicted| + |y_actual - y_predicted| )
```

---

# Estimated Parameters

| Parameter | Estimated Value |
|-----------|----------------:|
| θ | **30°** |
| M | **0.03** |
| X | **55** |

### Total L1 Error

```text
0.030834
```

### Average Error Per Point

```text
0.0000205
```

The predicted curve almost perfectly overlaps the original curve.

---

# Final Parametric Equation

```text
x(t) = t*cos(30°) - e^(0.03|t|)*sin(0.3t)*sin(30°) + 55

y(t) = 42 + t*sin(30°) + e^(0.03|t|)*sin(0.3t)*cos(30°)
```

---

# Desmos Equation

Copy the following into Desmos:

```text
(
t*cos(30°)-e^(0.03*abs(t))*sin(0.3*t)*sin(30°)+55,
42+t*sin(30°)+e^(0.03*abs(t))*sin(0.3*t)*cos(30°)
)

6 ≤ t ≤ 60
```

---

# Files

```
bruteforce_approach.ipynb   -> Implementation
xy_data.csv                 -> Input dataset
README.md                   -> Documentation
```

---

# Observations

- A brute-force search was used to estimate the unknown parameters.
- Instead of searching for `t`, an analytical expression was derived from the original equations.
- This significantly reduced the search space and improved computational efficiency.
- The estimated parameters produced an almost perfect reconstruction of the given curve.

---

# Author

**Hruthik Palivela**