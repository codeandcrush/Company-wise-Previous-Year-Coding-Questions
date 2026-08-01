# Four Indices Where `a + b = c + d`
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge)
![OA](https://img.shields.io/badge/Source-OA%20%7C%20GFG%20Reported-blue?style=for-the-badge)
![Arrays](https://img.shields.io/badge/Topic-Arrays%20%26%20Hashing-purple?style=for-the-badge)
![Hash Map](https://img.shields.io/badge/Technique-Hash%20Map-orange?style=for-the-badge)

---

## Problem Description

Given an array `arr` of `n` integers, find **four distinct indices**

```text
(i, j, k, l)
```

such that

```text
arr[i] + arr[j] = arr[k] + arr[l]
```

where all four indices are **distinct**.

Return **any valid set of indices**.

If no such indices exist, return `-1`.

---

## Example

### Example 1

**Input**

```text
arr = [3, 4, 7, 1, 2, 9, 8]
```

**Output**

```text
(1, 3, 0, 4)
```

**Explanation**

```text
arr[1] + arr[3] = 4 + 1 = 5

arr[0] + arr[4] = 3 + 2 = 5
```

Therefore,

```text
4 + 1 = 3 + 2
```

All indices are distinct.

---

### Example 2

**Input**

```text
arr = [1, 2, 3]
```

**Output**

```text
-1
```

No four distinct indices satisfy the condition.

---

## Approach & Intuition

A brute-force solution would examine every pair of pairs:

```text
Choose first pair
Choose second pair
Compare sums
```

This takes **O(n⁴)** time.

### Better Observation

For every pair of indices:

```text
(i, j)
```

compute

```text
sum = arr[i] + arr[j]
```

Store the **first pair** producing each sum inside a hash map.

Later, if another pair has the same sum:

```text
arr[i] + arr[j] = arr[k] + arr[l]
```

simply verify that the four indices are all distinct.

If they are, we've found the answer.

---

## Algorithm

1. Create a hash map:

```text
sum → (first_index, second_index)
```

2. Iterate over every pair `(i, j)`.

3. Compute:

```text
pairSum = arr[i] + arr[j]
```

4. If `pairSum` is not in the map:

- Store `(i, j)`.

5. Otherwise:

- Retrieve the previous pair.
- Check whether all four indices are distinct.
- If yes, return the indices.

6. If no valid pair exists, return `-1`.

---

## Complexity Analysis

- **Time Complexity:** `O(n²)`
- **Space Complexity:** `O(n²)`

---

## Code Implementation (Python)

```python
def findFourIndices(arr):
    pair_sum = {}

    n = len(arr)

    for i in range(n):
        for j in range(i + 1, n):

            s = arr[i] + arr[j]

            if s not in pair_sum:
                pair_sum[s] = (i, j)
            else:
                x, y = pair_sum[s]

                # Ensure all four indices are distinct
                if len({x, y, i, j}) == 4:
                    return (x, y, i, j)

    return -1


# Example
arr = [3, 4, 7, 1, 2, 9, 8]

print(findFourIndices(arr))
```

---

## Dry Run

### Input

```text
arr = [3,4,7,1,2,9,8]
```

### Pair Processing

| Pair | Sum | Hash Map |
|------|-----|----------|
| (0,1) | 7 | Store |
| (0,2) | 10 | Store |
| (0,3) | 4 | Store |
| (0,4) | 5 | Store |
| (1,3) | 5 | Already exists ✅ |

Stored pair:

```text
(0,4)
```

Current pair:

```text
(1,3)
```

All indices are distinct:

```text
0,4,1,3
```

Result:

```text
arr[0] + arr[4] = 3 + 2 = 5

arr[1] + arr[3] = 4 + 1 = 5
```

Answer found.

---

## Edge Cases

- No matching pair sums → Return `-1`.
- Duplicate values are allowed.
- Multiple valid answers may exist; returning **any one** is acceptable.
- Ensure all four indices are **distinct**.

---

## Key Takeaways

- ✅ Store the **first occurrence** of every pair sum.
- ✅ Hashing reduces the search from **O(n⁴)** to **O(n²)**.
- ✅ Always verify that the four indices are different.
- ✅ Multiple correct answers are possible.

---

## Interview Tips

- 🔹 Mention that there are **O(n²)** possible index pairs.
- 🔹 A hash map lets us instantly check whether the same sum has already appeared.
- 🔹 The distinct-index check is essential because two pairs may share an index.
- 🔹 This is a classic interview problem combining **Arrays**, **Pair Sums**, and **Hash Maps**.

---

## Related Problems

- Two Sum
- Four Sum
- Count Pairs with Given Sum
- Equal Pair Sums
- Subarray Sum Equals K
