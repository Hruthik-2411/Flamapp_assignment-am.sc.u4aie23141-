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

The file **xy_data.csv** contains 1501 points sampled from the above curve.

---

# Approach

Initially, I considered estimating four unknowns: **θ, M, X and t**. Since every point has its own value of **t**, searching for all of them together would make the brute-force search computationally expensive.

To simplify the problem, I combined the two given equations by multiplying the first equation with **cos(θ)** and the second equation with **sin(θ)**. After adding them, the exponential terms cancel out, giving the following expression:

```text
t = (x - X)*cos(θ) + (y - 42)*sin(θ)
```

Since the dataset already contains the values of **x** and **y**, and the brute-force search provides candidate values of **θ** and **X**, the corresponding **t** value for every point can be computed directly. Therefore, **t** no longer needs to be searched independently, reducing the optimization to only three unknown parameters:

- θ
- M
- X

---

# Brute Force Algorithm

The algorithm performs an exhaustive search over all possible values of:

- θ
- M
- X

For every parameter combination:

1. Compute **t** using the derived equation.
2. Substitute the computed **t** values back into the original parametric equations.
3. Generate the predicted `(x, y)` values.
4. Compute the L1 distance between the predicted points and the original dataset.
5. Store the parameter combination with the minimum error.

---

# Error Function

The objective function used is the L1 distance.

```text
L1 Error = Σ ( |x_actual - x_predicted| + |y_actual - y_predicted| )
```

The parameter combination with the minimum error is selected as the final solution.

---

# Estimated Parameters

| Parameter | Estimated Value |
|-----------|----------------:|
| θ | **30°** |
| M | **0.03** |
| X | **55** |

---

# Error

**Total L1 Error**

```text
0.030834
```

**Average Error Per Point**

```text
0.0000205
```

The obtained parameters reconstruct the given dataset with a very small L1 error.

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

# Observations

- A brute-force search was used to estimate the unknown parameters.
- An analytical expression for **t** was derived, eliminating the need to search for **t** separately.
- Since **x** and **y** are available in the dataset, the corresponding **t** values can be computed directly using the derived equation for every candidate value of **θ** and **X**.
- This significantly reduces the search space while preserving the original mathematical model.
- The estimated parameters reconstruct the original curve with a very small reconstruction error.

---

# Author

**Hruthik Palivela**