# AI Research & Development Assignment

## Problem Statement

The objective of this assignment is to estimate the unknown parameters of the following parametric curve.

\[
x(t)=t\cos(\theta)-e^{M|t|}\sin(0.3t)\sin(\theta)+X
\]

\[
y(t)=42+t\sin(\theta)+e^{M|t|}\sin(0.3t)\cos(\theta)
\]

Unknown Parameters:

- θ
- M
- X

Parameter Range:

- 0° < θ < 50°
- -0.05 < M < 0.05
- 0 < X < 100
- 6 < t < 60

The dataset (`xy_data.csv`) contains points sampled from the above curve.

---

# My Approach

Initially, I considered estimating four unknowns:

- θ
- M
- X
- t

However, every point has its own value of `t`, making it computationally expensive to search for all of them simultaneously.

To simplify the problem, I derived an analytical expression for `t`.

Starting from the original equations,

\[
x=t\cos(\theta)-e^{M|t|}\sin(0.3t)\sin(\theta)+X
\]

\[
y=42+t\sin(\theta)+e^{M|t|}\sin(0.3t)\cos(\theta)
\]

I eliminated the exponential term and obtained

\[
t=(x-X)\cos(\theta)+(y-42)\sin(\theta)
\]

Using this equation, the value of `t` can be computed directly for every point.

This reduced the optimization problem to only three unknown parameters:

- θ
- M
- X

---

# Brute Force Search

A brute force search was performed over the parameter space.

For every candidate combination of

- θ
- M
- X

the following steps were performed:

1. Compute `t` using the derived equation.
2. Substitute the computed `t` back into the original parametric equations.
3. Generate the predicted `(x,y)` values.
4. Compute the L1 distance between the predicted points and the original dataset.
5. Store the parameters that produce the minimum error.

The L1 error used is

\[
\sum_{i=1}^{N}
\left(
|x_i-x_i^{pred}|
+
|y_i-y_i^{pred}|
\right)
\]

---

# Estimated Parameters

| Parameter | Value |
|-----------|-------|
| θ | **30°** |
| M | **0.03** |
| X | **55** |

Total L1 Error

```
0.030834
```

Average Error Per Point

```
0.0000205
```

The predicted curve almost perfectly overlaps the original curve.

---

# Final Parametric Equation

\[
x(t)=t\cos(30^\circ)-e^{0.03|t|}\sin(0.3t)\sin(30^\circ)+55
\]

\[
y(t)=42+t\sin(30^\circ)+e^{0.03|t|}\sin(0.3t)\cos(30^\circ)
\]

---



# Notes

- The solution uses a brute force search.
- Instead of estimating `t` independently, an analytical expression for `t` was derived from the original equations.
- This significantly reduced the search space while preserving the original formulation.
- The final parameters were selected based on the minimum L1 distance between the original and predicted points.

---

# Author

Hruthik Palivela