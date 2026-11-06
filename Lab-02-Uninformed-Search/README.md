# Lab 2: Uninformed Search


This folder contains the answers of the questions that are required after attempting the BFS &amp; DFS, and it contains the change for understanding.

*The Help of AI is also used, since the entire LAB2 was first executed to understand how it works*
# Lab 02: Uninformed Search — BFS and DFS on a 2D Maze


---

# Part 1: Implementation and Execution

### Task 1: Creating the Maze

**Description:**

A 2D maze was created using a Python list containing rows and columns.

- `0` represents an open cell where movement is allowed.
- `1` represents a wall where movement is not allowed.
- The starting position is `(0, 0)`.
- The goal position is `(4, 4)`.

**Code:** Provided in the original lab.

**Result:** The maze was successfully created and displayed.

### Task 2: Defining Movements

**Description:**

Four possible movements were defined for the agent: UP, DOWN, LEFT, and RIGHT. Each movement is represented by a pair of row and column changes.

**Code:** Provided in the original lab.

**Result:** All four movement directions were successfully defined.

### Task 3: Checking Valid Moves

**Description:**

The `valid_move()` function checks whether a position is inside the maze and whether the cell is open.

Three test cases were executed:

| Position | Expected Result | Explanation |
|---|---|---|
| `(0, 0)` | True | Open cell |
| `(1, 1)` | False | Wall |
| `(-1, 0)` | False | Outside the maze |

**Result:** The function successfully identified valid and invalid movements.

### Task 4: Implementing Breadth-First Search (BFS)

**Description:**

BFS was implemented using a Queue (FIFO). It explores neighboring positions level by level and finds the shortest path when all movements have equal costs.

The important operation is:

```python
current, path = frontier.popleft()
```

**Result:** BFS successfully found a path from the starting position to the goal.

### Task 5: Implementing Depth-First Search (DFS)

**Description:**

DFS was implemented using a Stack (LIFO). It explores one branch deeply before backtracking to explore another branch.

The important operation is:

```python
current, path = frontier.pop()
```

**Result:** DFS successfully found a path from the starting position to the goal.

### Task 6: Visualizing the Paths

**Description:**

The `print_maze()` function was used to display the solution paths of BFS and DFS.

The following symbols were used:

| Symbol | Meaning |
|---|---|
| `*` | Solution path |
| `#` | Wall |
| `.` | Open cell |

**BFS Solution:**

```text
* . . . .
* # # # .
* * * # .
. # * * *
. # # # *
```

**DFS Solution:**

```text
* * * * *
. # # # *
. . . # *
. # . . *
. # # # *
```

**Result:** Both algorithms successfully reached the goal, but they followed different routes.

---

# Part 2: Comparison of BFS and DFS

## Task 7: Comparing BFS and DFS

The original maze, starting position, goal position, and direction order were used to compare both algorithms.

### Comparison Table

| Algorithm | Path Length | Maximum Frontier |
|---|---:|---:|
| BFS | 8 | 3 |
| DFS | 8 | 3 |

### Questions and Answers

**Q1. Which algorithm found the shorter path?**

Neither algorithm found a shorter path in this particular maze. Both BFS and DFS found paths of length 8, although their routes were different.

**Q2. Which algorithm had the larger maximum frontier?**

Neither algorithm had a larger maximum frontier. Both algorithms had a maximum frontier size of 3.

**Q3. Why did the two algorithms produce different paths on the same maze?**

BFS uses a Queue (FIFO), which explores the oldest discovered position first. DFS uses a Stack (LIFO), which explores the most recently discovered position first.

Because they follow different exploration strategies, they can reach the goal through different routes.

**Q4. Does DFS always produce the shortest path? Why or why not?**

No. DFS follows one branch as deeply as possible before exploring other branches. It may reach the goal through a longer route without checking whether a shorter route exists.

**Q5. Does BFS always produce the shortest path when every movement costs 1? Why?**

Yes. BFS explores positions level by level. It explores all positions requiring fewer movements before exploring positions requiring more movements. Therefore, the first goal it reaches has the minimum number of movements.

---

# Part 3: Experiments

## Task 8: Experiment 1 — Changing the Neighbor Order

### Objective

To observe how changing the order of neighboring directions affects the paths produced by BFS and DFS.

### Original Direction Order

```python
directions = [
    (-1, 0),   # up
    (1, 0),    # down
    (0, -1),   # left
    (0, 1)     # right
]
```

### Modified Direction Order

The following lines replace the original direction list:

```python
directions = [
    (0, 1),    # right
    (1, 0),    # down
    (0, -1),   # left
    (-1, 0)    # up
]
```

After changing the directions, the search algorithms were executed again:

```python
dfs_path2, dfs_frontier2 = dfs(maze, start, goal)

print("DFS path (new order):", dfs_path2)
print("DFS path length:", len(dfs_path2) - 1)
print("DFS maximum frontier:", dfs_frontier2)

# Also rerun BFS to observe the effect of neighbor order
bfs_path2, bfs_frontier2 = bfs(maze, start, goal)

print("BFS path (new order):", bfs_path2)
print("BFS path length:", len(bfs_path2) - 1)
print("BFS maximum frontier:", bfs_frontier2)
```

### Results

| Algorithm | Original Path | New Path | Path Length |
|---|---|---|---:|
| BFS | Bottom route | Top route | 8 |
| DFS | Top route | Bottom route | 8 |

Both algorithms had a maximum frontier size of 3 in this experiment.

### Questions and Answers

**Q1. Did the DFS path change?**

Yes. The DFS path changed because changing the neighbor order changed which position was added to the stack last and explored first.

**Q2. Did BFS's path depend on this ordering the same way? Why or why not?**

Yes. BFS's selected path can also depend on neighbor order when multiple shortest paths are available.

However, BFS still guarantees a shortest path when all movements have equal costs. Changing the neighbor order can change which shortest route it selects, but not the minimum path length.

**Result:** Changing the neighbor order affected the paths selected by both algorithms, demonstrating the importance of exploration order.

### Resetting Directions

Before starting Experiment 2, the original direction order was restored:

```python
directions = [
    (-1, 0),   # up
    (1, 0),    # down
    (0, -1),   # left
    (0, 1)     # right
]
```

---

## Task 9: Experiment 2 — Changing Start and Goal

### Objective

To observe how changing the starting and goal positions affects the path lengths and maximum frontier sizes of BFS and DFS.

Three different start/goal pairs were tested using open cells from the original maze.

### Code Used

The following code runs both algorithms on three different test cases:

```python
# Experiment 2: Changing Start and Goal

directions = [
    (-1, 0),
    (1, 0),
    (0, -1),
    (0, 1)
]

test_cases = [
    ((0, 0), (2, 2)),
    ((0, 4), (4, 4)),
    ((2, 0), (4, 4))
]

for start, goal in test_cases:

    bfs_path, bfs_frontier = bfs(maze, start, goal)
    dfs_path, dfs_frontier = dfs(maze, start, goal)

    print("Start:", start, "| Goal:", goal)

    print("BFS Length:", len(bfs_path) - 1,
          "| BFS Max Frontier:", bfs_frontier)

    print("DFS Length:", len(dfs_path) - 1,
          "| DFS Max Frontier:", dfs_frontier)

    print("--------------------------------")
```

### Experimental Results

| Start | Goal | BFS Length | BFS Max Frontier | DFS Length | DFS Max Frontier |
|---|---|---:|---:|---:|---:|
| `(0, 0)` | `(2, 2)` | 4 | 3 | 10 | 3 |
| `(0, 4)` | `(4, 4)` | 4 | 3 | 12 | 4 |
| `(2, 0)` | `(4, 4)` | 6 | 3 | 6 | 4 |

### Questions and Answers

**Q1. Does BFS's path length ever change if you re-run it with the same start/goal? Does DFS's?**

With the same maze, start, goal, and direction order, both algorithms produce the same results on repeated runs.

BFS always finds the same minimum path length for those fixed conditions. DFS also produces the same path length with a fixed exploration order, but its path and length may change if the neighbor order changes.

**Q2. As the distance between start and goal grows, what happens to the maximum frontier size for each algorithm?**

Generally, BFS may maintain a larger frontier because it explores multiple positions level by level. DFS often maintains a smaller frontier because it follows one branch deeply.

However, the maximum frontier size does not necessarily increase with distance. It also depends on the maze structure, available paths, and neighbor order.

**Q3. Can you find a start/goal pair where BFS and DFS return the same path? What does that tell you about the maze structure between those two points?**

Yes. For example:

```python
start = (2, 0)
goal = (4, 4)
```

Both algorithms return the following path:

```python
[
    (2, 0),
    (2, 1),
    (2, 2),
    (3, 2),
    (3, 3),
    (3, 4),
    (4, 4)
]
```

This demonstrates that both algorithms can select the same route for particular starting and goal positions. It does not necessarily mean that only one possible route exists.

**Q4. What happens if the goal is unreachable from the start? What do bfs() and dfs() return?**

If the goal is unreachable, both algorithms continue exploring all reachable positions until their frontier becomes empty.

They return:

```python
(None, max_frontier)
```

`None` indicates that no solution path was found.

### Additional Test: Unreachable Goal

The following separate maze was used to test an unreachable goal:

```python
test_maze = [
    [0, 1, 1],
    [1, 0, 1],
    [1, 1, 0]
]

test_start = (0, 0)
test_goal = (2, 2)

print(bfs(test_maze, test_start, test_goal))
print(dfs(test_maze, test_start, test_goal))
```

**Expected Output:**

```text
(None, 1)
(None, 1)
```

**Explanation:**

The starting and goal cells are both open, but they are disconnected by walls. Since diagonal movements are not allowed, neither algorithm can reach the goal.

**Result:** Both BFS and DFS correctly identified that no solution path exists.

---


---

# Part 5: 

## Task 11: Understanding the Main Difference

The main behavioral difference between BFS and DFS comes from how they remove elements from the frontier.

### BFS

```python
current, path = frontier.popleft()
```

BFS uses FIFO (First In, First Out), which makes it explore positions level by level.

### DFS

```python
current, path = frontier.pop()
```

DFS uses LIFO (Last In, First Out), which makes it explore the most recently discovered position first.

### Final Comparison

| Feature | BFS | DFS |
|---|---|---|
| Full Form | Breadth-First Search | Depth-First Search |
| Data Structure | Queue | Stack |
| Principle | FIFO | LIFO |
| Exploration Strategy | Level by level | Deep into a branch |
| Shortest Path Guarantee (Equal Costs) | Yes | No |
| Memory Usage | Generally higher | Often lower in typical search trees |

---

# Conclusion

In this lab, I implemented Breadth-First Search (BFS) and Depth-First Search (DFS) and executed on a 2D maze using Python language.

Both algorithms successfully searched for the goal state, but the exploration strategies produced different paths.

I used BFS a Queue (FIFO) to explore the maze level by level which guaranteed the shortest path because every movement had equal cost.

I used DFS a Stack (LIFO) to explore one branch deeply before back-tracking. Its path depend on order in which neighboring positions were explored, and it did not guaranteed the shortest path.

The experiments demonstrated how changing neighbor order and start/goal positions affected the search results and frontier sizes.

**Overall, this lab demonstrated how two different frontier-management strategies can produce different search behaviors even when they use the same maze and movement rules.**
