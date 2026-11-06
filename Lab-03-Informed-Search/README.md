# Lab 3: Informed Search


Implementation of A* Search for the 8-Puzzle using Misplaced Tiles and Manhattan Distance heuristics, with node expansion comparison and performance analysis.

# Lab 03: A* Search for the 8-Puzzle

## 1. Objective

The objective of this lab is to implement the A* search algorithm and apply it to the 8-Puzzle problem.

The lab implements and compares two admissible heuristics:

1. Misplaced Tiles
2. Manhattan Distance

The comparison measures how many nodes A* expands when solving puzzles generated at different scrambling depths.

## 2. Problem Representation

The 8-Puzzle consists of a 3×3 board containing eight numbered tiles and one blank space.

The puzzle is represented using a tuple of nine integers. The value `0` represents the blank space.

**Goal State:**

```python
GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)
```

The search problem consists of:

- **State:** The current arrangement of the nine positions.
- **Initial State:** The starting arrangement of the puzzle.
- **Actions:** Up, Down, Left, and Right movements of the blank.
- **Transition Model:** Swaps the blank with an adjacent numbered tile.
- **Goal Test:** Checks whether the current state equals `GOAL_STATE`.
- **Path Cost:** Each move has a cost of 1.

## 3. Tasks Completed

### Task 1: State Helper Functions

Implemented and tested:

- `get_blank_position(state)` to find the blank's index.
- `get_neighbors(state)` to generate valid neighboring states.
- `is_goal(state)` to check whether the puzzle is solved.

The Cell 4 checkpoint confirmed that the goal state has exactly two valid neighboring moves: Up and Left.

### Task 2: Misplaced Tiles Heuristic

Implemented `misplaced_tiles(state)`.

The function counts numbered tiles that are not in their correct goal positions. The blank space is excluded.

```python
def misplaced_tiles(state):
    misplaced = 0

    for i in range(len(state)):
        if state[i] != 0 and state[i] != GOAL_STATE[i]:
            misplaced += 1

    return misplaced
```

### Task 3: Manhattan Distance Heuristic

Implemented `manhattan_distance(state)`.

The function calculates the horizontal and vertical distance of every numbered tile from its correct goal position.

```python
def manhattan_distance(state):
    total_distance = 0

    for current_index, tile in enumerate(state):

        if tile == 0:
            continue

        current_row, current_col = divmod(current_index, 3)

        goal_index = tile - 1
        goal_row, goal_col = divmod(goal_index, 3)

        distance = (
            abs(current_row - goal_row)
            + abs(current_col - goal_col)
        )

        total_distance += distance

    return total_distance
```

Both heuristics return zero for the goal state and are admissible.

### Task 4: A* Search Implementation

Implemented `a_star_search(start_state, heuristic_fn)` using Python's `heapq` priority queue.

A* calculates:

\[
f(n)=g(n)+h(n)
\]

Where:

- `g(n)` is the actual cost from the starting state.
- `h(n)` is the estimated remaining cost.
- `f(n)` is the total estimated cost.

The implementation also uses:

- `best_g` to store the lowest known path cost for each state.
- A tie-breaker counter to keep priority queue entries ordered safely.
- A stale-entry check to skip outdated entries.
- `nodes_expanded` to count entries removed from the frontier.

### Task 5: Solvability Check

Implemented the inversion-count method in `is_solvable(state)`.

For a 3×3 8-Puzzle, a state is solvable when its inversion count is even.

An additional assertion was used during testing:

```python
assert is_solvable(start), "Generated puzzle is not solvable"
```

### Task 6: Scramble Generator

Used `generate_scramble(num_moves, seed=None)` to create puzzle states by making random legal moves from the goal state.

Different scrambling steps were tested to compare the behavior of both heuristics.

Note: Scrambling steps do not necessarily equal the optimal solution distance because random moves can undo previous moves.

### Task 7: Heuristic Comparison

Tested both heuristics using four scrambling-step values:

- 5
- 10
- 15
- 20

Five trials were performed at each level, resulting in 20 puzzle trials.

An additional assertion checked that both heuristics returned the same optimal solution cost:

```python
assert result_misplaced["cost"] == result_manhattan["cost"], \
    "The two heuristics returned different solution costs"
```

## 4. Experimental Results

The following table shows the average number of nodes expanded by each heuristic in five trials per scrambling level.

| Scramble Steps | Misplaced Tiles | Manhattan Distance |
|---:|---:|---:|
| 5 | 3.6 | 3.6 |
| 10 | 4.4 | 4.2 |
| 15 | 5.6 | 5.4 |
| 20 | 13.6 | 10.4 |

The comparison shows that Manhattan Distance expanded fewer nodes at scrambling levels 10, 15, and 20 in this experiment.

The largest difference appeared at 20 scrambling steps.

The actual Colab notebook contains the generated results table and chart.

## 5. Analysis Questions and Answers

### Q1. Why must h(n) = 0 whenever state == GOAL_STATE, for both heuristics? What would break in A* if this were not true?

When the goal state is reached, the actual remaining cost is zero because no additional moves are needed.

Both heuristics must therefore return zero for the goal state.

If a heuristic returned a positive value at the goal, it would overestimate the actual remaining cost. This would violate admissibility and could cause A* to lose its optimality guarantee.

### Q2. At what scramble depth does the gap between the two heuristics become clearly visible? Why does the gap widen as depth increases rather than staying constant?

In this experiment, the difference becomes more noticeable at 20 scrambling steps.

Manhattan Distance provides more detailed information because it measures how far each tile is from its goal position. Misplaced Tiles only counts whether a tile is incorrectly positioned.

As puzzles become harder, the additional information provided by Manhattan Distance can help A* avoid expanding unnecessary nodes.

The exact point where the difference becomes visible depends on the generated puzzle states.

### Q3. Manhattan Distance is provably greater than or equal to Misplaced Tiles for every state. Why does a higher admissible heuristic not violate admissibility, and why does it lead to fewer node expansions?

Admissibility means that a heuristic must never overestimate the actual minimum remaining cost.

A higher heuristic value is not automatically inadmissible.

Every misplaced tile must move at least one step to reach its goal position. Misplaced Tiles contributes one for each incorrect tile, while Manhattan Distance measures the actual horizontal and vertical distance.

Therefore:

\[
h_{\text{Manhattan}}(n)\geq h_{\text{Misplaced}}(n)
\]

Manhattan Distance is still admissible because it does not exceed the true minimum remaining cost.

It generally provides a more accurate estimate, helping A* prioritize more promising states and expand fewer nodes.

### Q4. Does Manhattan Distance hit the same performance limit as Misplaced Tiles?

Not necessarily.

Manhattan Distance generally performs better because it provides a stronger estimate of the remaining cost and often expands fewer nodes.

However, A* can still become expensive for difficult puzzles because it may need to store and explore a large number of states.

## 6. Conclusion

In this lab, I implemented A* search and tested on the 8-Puzzle problem using two admissible heuristics.

Both Misplaced Tiles and Manhattan Distance found optimal solutions with identical solution costs in the experiment.

However, Manhattan Distance generally expanded fewer nodes because it provides a more informative estimate of the remaining distance to the goal.

This experiment demonstrates that a stronger admissible heuristic can improve search efficiency without sacrificing optimality.
