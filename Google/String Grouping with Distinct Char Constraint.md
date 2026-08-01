# String Grouping with Distinct Character Constraint
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![Onsite 2024](https://img.shields.io/badge/Source-Onsite%202024-blue?style=for-the-badge)
![Actual Reported](https://img.shields.io/badge/Interview-Actual%20Reported-green?style=for-the-badge)
![DP](https://img.shields.io/badge/Topic-DP%20%26%20Greedy-purple?style=for-the-badge)
![Strings](https://img.shields.io/badge/Topic-Strings-orange?style=for-the-badge)

---

## Problem Description

You are given:

- A string `s`
- An integer `k`

You must divide the characters of `s` into groups such that:

- Each group contains **at most `k` characters**.
- **All occurrences of the same character must belong to exactly one group.**
  - A character cannot be split across multiple groups.

Return the **minimum number of groups** required.

If any character appears more than `k` times, return **`-1`** because that character cannot fit into any valid group.

---

## Example

### Example 1

**Input**

```text
s = "aabbcc"
k = 4
```

**Output**

```text
2
```

**Explanation**

Character frequencies:

```text
a → 2
b → 2
c → 2
```

One optimal grouping is:

```text
Group 1 : aa + bb = 4
Group 2 : cc = 2
```

Minimum groups = **2**

---

### Example 2

**Input**

```text
s = "aaaabb"
k = 3
```

**Output**

```text
-1
```

**Explanation**

```text
'a' appears 4 times
```

Since

```text
4 > 3
```

it is impossible to place all `'a'` characters in a single group.

---

## Approach & Intuition

Each **distinct character behaves like one indivisible block**.

For example,

```text
"aaabbcccc"
```

becomes

```text
[3,2,4]
```

where each number represents the size of a block.

The problem now becomes:

> Pack these blocks into bins of capacity `k` while minimizing the number of bins.

### Step 1

Count the frequency of every character.

If any frequency exceeds `k`:

```text
Return -1
```

### Step 2

Sort the block sizes in descending order.

### Step 3

Greedily place each block into the first group with enough remaining capacity.

If no existing group can accommodate it:

- Create a new group.

This is the classic **Bin Packing (First-Fit Decreasing)** greedy strategy.

---

## Algorithm

1. Count character frequencies.
2. If any frequency > `k`, return `-1`.
3. Sort frequencies in descending order.
4. Maintain the remaining capacity of each group.
5. For every frequency:
   - Place it into the first group with enough remaining capacity.
   - Otherwise create a new group.
6. Return the total number of groups.

---

## Complexity Analysis

Let

- `n` = length of the string
- `C` = number of distinct characters

- **Time Complexity:** `O(n + C log C)`
- **Space Complexity:** `O(C)`

---

## Code Implementation (Python)

```python
from collections import Counter

def minimumGroups(s, k):
    freq = Counter(s)

    # Impossible if any character block exceeds group size
    for count in freq.values():
        if count > k:
            return -1

    # Sort blocks from largest to smallest
    blocks = sorted(freq.values(), reverse=True)

    # Remaining capacity of each group
    remaining = []

    for block in blocks:
        placed = False

        for i in range(len(remaining)):
            if remaining[i] >= block:
                remaining[i] -= block
                placed = True
                break

        if not placed:
            remaining.append(k - block)

    return len(remaining)


# Example
print(minimumGroups("aabbcc", 4))   # 2
print(minimumGroups("aaaabb", 3))   # -1
```

---

## Dry Run

### Input

```text
s = "aabbcc"
k = 4
```

Frequency table:

```text
a → 2
b → 2
c → 2
```

Sorted blocks:

```text
[2,2,2]
```

| Block | Remaining Capacity Before | Action | Remaining Capacity After |
|--------|---------------------------|--------|--------------------------|
| 2 | [] | Create Group 1 | [2] |
| 2 | [2] | Place in Group 1 | [0] |
| 2 | [0] | Create Group 2 | [0,2] |

Final groups:

```text
Group 1 = aa + bb
Group 2 = cc
```

Answer:

```text
2
```

---

## Edge Cases

- Empty string → `0`
- Single distinct character:
  - Frequency ≤ `k` → `1`
  - Frequency > `k` → `-1`
- `k = 1` → Every distinct character must appear exactly once, otherwise impossible.
- Large frequencies close to `k` → Naturally handled by the greedy packing.

---

## Key Takeaways

- ✅ Treat each distinct character as an **indivisible block**.
- ✅ The problem reduces to a **Bin Packing** problem.
- ✅ First-Fit Decreasing (FFD) is a common greedy heuristic.
- ✅ Frequency counting determines feasibility in `O(n)` time.
- ✅ Overall complexity is `O(n + C log C)`.

---

## ⚠️ Interview Discussion

The greedy solution above is a **heuristic**, not an exact algorithm.

After converting character frequencies into block sizes, the problem becomes the classic **Bin Packing Problem**, which is **NP-hard**.

- **First-Fit Decreasing (FFD)** is widely used in interviews because it is simple and efficient.
- However, it **does not always produce the minimum number of groups**.

For example:

```text
k = 10
Blocks = [6, 5, 5, 4, 4]
```

FFD may use more groups than the optimal packing.

If an interviewer asks for the **guaranteed minimum** number of groups, discuss:
- Dynamic Programming (for small `k` or constrained inputs),
- Backtracking with pruning,
- Integer Linear Programming (ILP),
- Exact Bin Packing algorithms.

Recognizing that this reduces to **Bin Packing** is often more important than memorizing a specific implementation.
