---

# Deterministic Simulated Annealing for TSP

## Overview

This project implements several **deterministic variants of Simulated Annealing (SA)** to solve the **Traveling Salesman Problem (TSP)** using the standard **TSPLIB** dataset `kroB200`.

Unlike classical SA, which relies on randomness in initialization, neighbor selection, and acceptance probability, these implementations progressively **remove stochastic components** to analyze their effect on solution quality and search behavior.

---

## Dataset

* **Dataset:** `kroB200`
* **Problem Type:** Traveling Salesman Problem (TSP)
* **Number of Cities:** 200
* **Coordinates:** 2D Euclidean
* **Distance Metric:** Euclidean distance

Each city is defined by an ID and its `(x, y)` coordinates. A tour is a permutation of all cities, forming a closed loop.

---

## Common Components

### Distance Calculation

The total tour length is computed as the sum of Euclidean distances between consecutive cities, including the return from the last city to the first.

### Neighborhood Structure

All variants use the **2-opt operator**, which reverses a segment of the tour between two selected indices `(i, j)` to generate a neighboring solution.

---

## Implemented Variants

### 1. Deterministic Initial Tour

* **Initial Solution:** Fixed, ordered tour `[0, 1, 2, ..., n-1]`
* **Goal:** Remove randomness from initialization
* **Neighbor Selection:** Random
* **Acceptance Rule:** Standard SA (accept worse solutions probabilistically)

This variant isolates the impact of a deterministic starting point.

---

### 2. Deterministic City Selection

* **Initial Solution:** Random
* **Neighbor Selection:** Deterministic

  * Cities selected as:

    ```
    i = iteration % (n - 1)
    j = i + 1
    ```
* **Acceptance Rule:** Standard SA

This version removes randomness from neighborhood exploration.

---

### 3. Deterministic Acceptance (Greedy)

* **Initial Solution:** Random
* **Neighbor Selection:** Random
* **Acceptance Rule:**

  * Only strictly better solutions are accepted
  * No probabilistic acceptance of worse solutions

This effectively transforms SA into a greedy local search.

---

### 4. Fully Deterministic Simulated Annealing

This final version removes **all stochastic elements**:

* **Initial Tour:** Deterministic
* **City Selection:** Deterministic
* **Acceptance Rule:** Only better solutions accepted
* **Cooling Schedule:** Retained but has no practical effect on acceptance

This algorithm behaves as a **deterministic local search with 2-opt moves**, allowing a direct comparison with stochastic SA.

---

## Purpose of the Study

The goal of these implementations is to:

* Analyze the role of randomness in Simulated Annealing
* Compare stochastic and deterministic behaviors
* Observe convergence, stagnation, and solution quality
* Understand how SA degrades into local search when randomness is removed

---

## Visualization

The final tour is visualized using Matplotlib, showing:

* City positions
* Tour connections
* Closed-loop path

---

## Execution

Each variant:

* Loads the `kroB200.txt` dataset
* Runs for a fixed maximum number of iterations
* Reports:

  * Final tour length
  * Execution time
  * Visual plot of the tour
