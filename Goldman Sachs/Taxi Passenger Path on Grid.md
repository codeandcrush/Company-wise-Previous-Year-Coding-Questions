# Taxi Passenger Path on Grid
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge)
![OA](https://img.shields.io/badge/Source-OA%20%C2%B7%20GS%20Reported-blue?style=for-the-badge)
![DP](https://img.shields.io/badge/Topic-DP%20%26%20Grid-purple?style=for-the-badge)
![Dynamic Programming](https://img.shields.io/badge/Technique-Grid%20DP-orange?style=for-the-badge)

---

## Problem Description

A taxi starts at the **top-left corner** of an `n × n` grid:

```text
(0, 0)
```

and must reach the **bottom-right corner**:

```text
(n-1, n-1)
```

The taxi:

- Can move **only Right or Down**.
- Can carry **multiple passengers**.
- Picks up every passenger located on the cells it visits.

The grid contains:

- `1` → A passenger is waiting.
- `0` → Empty cell.

Return the **maximum number of passengers** the taxi can pick up while reaching the destination.

---

## Example

### Input

```text
grid = [
    [0,1,0],
    [0,0,1],
    [1,0,0]
]
```

### Output

```text
2
```

### Explanation

One optimal path is:

```text
(0,0)
   ↓
(0,1)  ✅ Passenger
   ↓
(1,1)
   ↓
(1,2)  ✅ Passenger
   ↓
(2,2)
```

Passengers collected:

```text
(0,1)
(1,2)
```

Total passengers:

```text
2
```

---

## Approach & Intuition

Since the taxi can move **only right or down**, every cell can be reached from only two possible directions:

- From the **top**
- From the **left**

Let

```text
dp[i][j]
```

represent:

> The maximum number of passengers that can be collected upon reaching cell `(i, j)`.

The taxi chooses whichever previous path yields more passengers.

---

## State Transition

For every cell:

```text
dp[i][j] =
grid[i][j] +
max(
    dp[i-1][j],
    dp[i][j-1]
)
```

where unavailable directions are ignored.

---

## Algorithm

1. Create an `n × n` DP table.
2. Initialize:

```text
dp[0][0] = grid[0][0]
```

3. Fill the first row.
4. Fill the first column.
5. For every remaining cell:

```text
dp[i][j] =
grid[i][j] +
max(dp[i-1][j], dp[i][j-1])
```

6. The answer is

```text
dp[n-1][n-1]
```

---

## Complexity Analysis

- **Time Complexity:** `O(n²)`
- **Space Complexity:** `O(n²)`

> **Optimization:** Space can be reduced to **`O(n)`** using a rolling DP array since only the previous row is needed.

---

## Code Implementation (Python)

```python
def maxPassengers(grid):
    if not grid:
        return 0

    n = len(grid)

    dp = [[0] * n for _ in range(n)]

    dp[0][0] = grid[0][0]

    # First row
    for j in range(1, n):
        dp[0][j] = dp[0][j - 1] + grid[0][j]

    # First column
    for i in range(1, n):
        dp[i][0] = dp[i - 1][0] + grid[i][0]

    # Remaining cells
    for i in range(1, n):
        for j in range(1, n):
            dp[i][j] = grid[i][j] + max(dp[i - 1][j], dp[i][j - 1])

    return dp[n - 1][n - 1]


# Example
grid = [
    [0,1,0],
    [0,0,1],
    [1,0,0]
]

print(maxPassengers(grid))   # Output: 2
```

---

## Dry Run

### Input

```text
0 1 0
0 0 1
1 0 0
```

### DP Table Construction

| Cell | DP Value | Explanation |
|------|----------|-------------|
| (0,0) | 0 | Start |
| (0,1) | 1 | Collect passenger |
| (0,2) | 1 | Continue right |
| (1,0) | 0 | Move down |
| (1,1) | 1 | Max(top,left) |
| (1,2) | 2 | Collect passenger |
| (2,0) | 1 | Collect passenger |
| (2,1) | 1 | Max(top,left) |
| (2,2) | 2 | Destination |

Final DP table:

```text
0 1 1
0 1 2
1 1 2
```

Answer:

```text
2
```

---

## Edge Cases

- Empty grid → `0`
- Single cell:
  - `0` → `0`
  - `1` → `1`
- No passengers → `0`
- Every cell has a passenger → Maximum passengers along any valid right/down path.

---

## Key Takeaways

- ✅ This is a classic **Grid Dynamic Programming** problem.
- ✅ Each state depends only on the **top** and **left** cells.
- ✅ DP guarantees the optimal passenger count for every cell.
- ✅ Can be optimized from **O(n²)** space to **O(n)**.
- ✅ Similar to problems like **Maximum Path Sum** and **Minimum Path Sum**.

---

## Follow-up

### 1. What if some cells are blocked?

Represent blocked cells with `-1`.

- Ignore blocked cells during DP transitions.
- If both top and left are unreachable, the current cell is also unreachable.

---

### 2. What if the taxi can move in all four directions?

The problem is no longer a simple Grid DP because cycles become possible.

Possible approaches include:

- **BFS** (unweighted shortest path)
- **Dijkstra's Algorithm** (weighted movement)
- **Graph Search + State DP** (if passenger collection constraints are added)

---

### 3. What if the taxi must return to `(0,0)` after reaching the destination?

This becomes the famous **Cherry Pickup (LeetCode 741)** problem, where two synchronized paths are modeled using **3D Dynamic Programming**.
