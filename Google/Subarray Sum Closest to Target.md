# Subarray Sum Closest to Target
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge)
![Phone Screen](https://img.shields.io/badge/Source-Phone%20Screen-blue?style=for-the-badge)
![Arrays](https://img.shields.io/badge/Topic-Arrays%20%26%20Prefix%20Sum-purple?style=for-the-badge)
![Binary Search](https://img.shields.io/badge/Technique-Binary%20Search-orange?style=for-the-badge)

---

## Problem Description

Given an integer array `nums` (which **may contain negative numbers**) and an integer `k`, find the **contiguous subarray** whose sum is **closest** to `k`.

Return the **subarray sum** that is closest to the target.

---

## Example

**Input**

```text
nums = [1, 3, -2, 5, 4]
k = 6
```

**Output**

```text
6
```

**Explanation**

One optimal subarray is:

```text
[3, -2, 5]
```

Its sum is:

```text
3 + (-2) + 5 = 6
```

Since the subarray sum equals the target, it is the closest possible answer.

---

## Approach & Intuition

A brute-force solution would check every possible subarray, resulting in **O(n²)** time.

We can optimize using **Prefix Sums** and **Binary Search**.

### Key Observation

Let:

```text
prefix[i] = sum(nums[0...i-1])
```

Then any subarray sum is:

```text
prefix[j] - prefix[i]
```

We want this value to be as close as possible to `k`.

Rearranging:

```text
prefix[i] ≈ prefix[j] - k
```

For every current prefix sum `prefix[j]`:

1. Compute the target value:

```text
target = prefix[j] - k
```

2. Among all previous prefix sums, find the one closest to `target`.
3. This gives the subarray whose sum is closest to `k`.

To efficiently find the closest prefix sum, maintain previous prefix sums in a **sorted list** and use **binary search**.

---

## Algorithm

1. Initialize:
   - `prefix = 0`
   - Sorted list containing the initial prefix sum `0`.
2. Traverse the array:
   - Update the current prefix sum.
   - Compute:

```text
target = prefix - k
```

- Binary search to find the closest prefix sum to `target`.
- Update the best answer if a closer subarray sum is found.
- Insert the current prefix sum into the sorted list.
3. Return the closest subarray sum.

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)`
- **Space Complexity:** `O(n)`

---

## Code Implementation (Python)

```python
from bisect import bisect_left, insort

def subarraySumClosest(nums, k):
    prefix = 0
    prefixes = [0]

    closest_sum = 0
    min_diff = float("inf")

    for num in nums:
        prefix += num
        target = prefix - k

        idx = bisect_left(prefixes, target)

        # Check the closest prefix on the right
        if idx < len(prefixes):
            curr_sum = prefix - prefixes[idx]
            diff = abs(curr_sum - k)

            if diff < min_diff:
                min_diff = diff
                closest_sum = curr_sum

        # Check the closest prefix on the left
        if idx > 0:
            curr_sum = prefix - prefixes[idx - 1]
            diff = abs(curr_sum - k)

            if diff < min_diff:
                min_diff = diff
                closest_sum = curr_sum

        insort(prefixes, prefix)

    return closest_sum


# Example
nums = [1, 3, -2, 5, 4]
k = 6

print(subarraySumClosest(nums, k))   # Output: 6
```

---

## Dry Run

### Input

```text
nums = [1, 3, -2, 5, 4]
k = 6
```

| Current Number | Prefix Sum | Target (`prefix-k`) | Closest Prefix | Subarray Sum |
|---------------|------------|---------------------|----------------|--------------|
| 1 | 1 | -5 | 0 | 1 |
| 3 | 4 | -2 | 0 | 4 |
| -2 | 2 | -4 | 0 | 2 |
| 5 | 7 | 1 | 1 | 6 ✅ |
| 4 | 11 | 5 | 4 | 7 |

Best subarray sum:

```text
6
```

---

## Why Binary Search?

Without binary search:

- Searching for the closest prefix would take **O(n)**.
- Overall complexity becomes **O(n²)**.

Using a sorted prefix list:

- Searching takes **O(log n)**.
- Inserting into the sorted list takes **O(log n)** (using balanced BST; Python's `insort` is `O(n)` due to list shifting, but the intended interview solution assumes a balanced ordered set).
- Overall interview complexity: **O(n log n)**.

---

## Key Takeaways

- ✅ Prefix sums convert subarray sums into differences of prefix sums.
- ✅ We search for a previous prefix closest to `prefix - k`.
- ✅ Binary search efficiently finds the nearest candidate.
- ✅ Handles **negative numbers**, unlike the sliding window technique.
- ✅ Classic interview problem combining **Prefix Sum**, **Binary Search**, and **Ordered Data Structures**.
