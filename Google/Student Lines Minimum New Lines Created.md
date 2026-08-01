# Student Lines - Minimum New Lines Created
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge)
![OA 2026](https://img.shields.io/badge/Source-OA%202026-blue?style=for-the-badge)

![Arrays](https://img.shields.io/badge/Topic-Arrays%20%26%20Greedy-purple?style=for-the-badge)
![Patience Sorting](https://img.shields.io/badge/Technique-Patience%20Sorting-orange?style=for-the-badge)

---

## Problem Description

Students arrive one at a time in the given order.

Each arriving student:

- Joins an **existing line** if **every student already in that line is taller** than them.
- Otherwise, they **start a new line**.

Given an array `heights` representing students' heights in arrival order, return the **minimum number of lines** that will be formed.

---

## Example

**Input**

```text
heights = [5, 3, 6, 2, 4]
```

**Output**

```text
3
```

**Explanation**

One optimal arrangement is:

```text
Line 1: [5, 3, 2]
Line 2: [6, 4]
Line 3: []
```

Process:

| Student | Action | Line Tails |
|---------|--------|------------|
| 5 | Start Line 1 | [5] |
| 3 | Join Line 1 | [3] |
| 6 | Start Line 2 | [3,6] |
| 2 | Join Line 1 | [2,6] |
| 4 | Join Line 2 | [2,4] |

Total lines = **2**.

> The greedy algorithm always places a student into the valid line whose current tail is the **smallest height that is still greater** than the student's height.

---

## Approach & Intuition

The key observation is that **only the last (shortest) student in each line matters**.

Suppose a line currently ends with height `tail`.

A new student of height `h` can join that line **only if**

```text
tail > h
```

To minimize the number of lines:

- Find the line with the **smallest tail greater than `h`**.
- Replace that tail with `h`.
- If no such tail exists, create a new line.

This is exactly the **Patience Sorting** technique used in the **Longest Increasing Subsequence (LIS)** algorithm.

Using binary search allows us to efficiently find the correct line for every student.

---

## Algorithm

For each height:

1. Maintain a sorted list `tails`.
2. Binary search for the first tail that is **strictly greater** than the current height.
3. If found:
   - Replace that tail with the current height.
4. Otherwise:
   - Append a new tail (create a new line).
5. The final size of `tails` is the minimum number of lines.

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)`
- **Space Complexity:** `O(n)`

---

## Code Implementation (Python)

```python
from bisect import bisect_right

def minimumLines(heights):
    tails = []

    for h in heights:
        # Find the first tail strictly greater than h
        idx = bisect_right(tails, h)

        if idx == len(tails):
            # No valid line, create a new one
            tails.append(h)
        else:
            # Place student into the best existing line
            tails[idx] = h

    return len(tails)


# Example
heights = [5, 3, 6, 2, 4]
print(minimumLines(heights))   # Output: 2
```

---

## Dry Run

For:

```text
heights = [5, 3, 6, 2, 4]
```

| Student | Tails Before | Action | Tails After |
|---------|--------------|--------|-------------|
| 5 | [] | New line | [5] |
| 3 | [5] | Replace 5 | [3] |
| 6 | [3] | New line | [3,6] |
| 2 | [3,6] | Replace 3 | [2,6] |
| 4 | [2,6] | Replace 6 | [2,4] |

Final:

```text
Number of lines = 2
```

---

## Key Takeaways

- ✅ Greedy placement always gives the minimum number of lines.
- ✅ Only the **tail** of each line needs to be tracked.
- ✅ Binary search finds the optimal line in `O(log n)`.
- ✅ This is a classic application of **Patience Sorting**, closely related to the **Longest Increasing Subsequence (LIS)** algorithm.
