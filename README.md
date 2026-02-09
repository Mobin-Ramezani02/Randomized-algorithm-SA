
# Simulated Annealing Variants for the Traveling Salesman Problem (TSP)

## Overview

This project implements several variants of **Simulated Annealing (SA)** to solve the **Traveling Salesman Problem (TSP)** using the standard **TSPLIB** dataset `kroB200`.

The implementations range from a **classical stochastic Simulated Annealing** algorithm to **partially and fully deterministic variants**, allowing a structured comparison of randomness, exploration capability, and convergence behavior.

---

## Dataset

* **Dataset:** `kroB200`
* **Problem Type:** Traveling Salesman Problem (TSP)
* **Number of Cities:** 200
* **Coordinates:** 2D Euclidean
* **Distance Metric:** Euclidean distance
* **Tour Type:** Closed tour (returns to starting city)

Each city is represented by an ID and its `(x, y)` coordinates. A solution is a permutation of all cities forming a Hamiltonian cycle.

---

## Core Components

### Distance Evaluation

The objective function computes the total length of a tour by summing Euclidean distances between consecutive cities, including the return from the last city to the first.

### Neighborhood Operator

All implementations use the **2-opt neighborhood**, where a segment of the tour between indices `(i, j)` is reversed to generate a neighboring solution.

---

## 1. Classical Simulated Annealing (Stochastic SA)

### Description

This is the **baseline implementation** of Simulated Annealing and corresponds to the first code provided.

### Characteristics

* **Initial Tour:** Random permutation of cities
* **Neighbor Selection:** Random selection of two indices `(i, j)`
* **Acceptance Rule:**

  * Always accept better solutions
  * Accept worse solutions with probability:
    [
    P = \exp\left(-\frac{\Delta}{T}\right)
    ]
* **Cooling Schedule:** Exponential cooling
* **Stochastic Elements:**

  * Initial solution
  * Neighbor selection
  * Acceptance decision

### Purpose

This version provides:

* Strong exploration capability
* Ability to escape local minima
* A reference point for comparing deterministic variants

---

## 2. Deterministic Initial Tour

### Description

This variant removes randomness from the **initial solution**.

### Characteristics

* **Initial Tour:** Fixed ordered tour `[0, 1, 2, ..., n-1]`
* **Neighbor Selection:** Random
* **Acceptance Rule:** Standard SA probabilistic acceptance

### Purpose

To isolate and study the impact of a deterministic starting point on convergence and final solution quality.

---

## 3. Deterministic City Selection

### Description

This variant removes randomness from the **neighbor selection process**.

### Characteristics

* **Initial Tour:** Random
* **Neighbor Selection:** Deterministic

  ```
  i = iteration % (n - 1)
  j = i + 1
  ```
* **Acceptance Rule:** Standard SA

### Purpose

To analyze how systematic neighborhood traversal affects exploration and performance.

---

## 4. Deterministic Acceptance (Greedy SA)

### Description

This variant removes stochasticity from the **acceptance mechanism**.

### Characteristics

* **Initial Tour:** Random
* **Neighbor Selection:** Random
* **Acceptance Rule:**

  * Only strictly improving solutions are accepted
  * Worse solutions are always rejected

### Interpretation

This effectively converts Simulated Annealing into a **greedy local search** algorithm using 2-opt moves.

---

## 5. Fully Deterministic Simulated Annealing

### Description

This is the most restrictive variant, where **all sources of randomness are removed**.

### Characteristics

* **Initial Tour:** Deterministic
* **Neighbor Selection:** Deterministic
* **Acceptance Rule:** Greedy (only better solutions accepted)
* **Cooling Schedule:** Retained but has no effect on acceptance

### Interpretation

Despite the name, this algorithm behaves as a **deterministic 2-opt local search**, useful for direct comparison with stochastic SA.

---

## Experimental Purpose

The collection of implementations is designed to:

* Study the role of randomness in Simulated Annealing
* Observe how SA degrades into local search when stochastic components are removed
* Compare exploration, convergence speed, and solution quality
* Provide educational insight into metaheuristic design choices

---

## Visualization

Each run produces:

* A 2D plot of the final tour using Matplotlib
* Printed output including:

  * City visit order
  * Total tour length
  * Execution time
