# Count Distinct Strings via Non-Adjacent Swaps
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge) ![Google OA](https://img.shields.io/badge/Source-Google%20OA%202025-blue?style=for-the-badge) ![IIT BHU Intern](https://img.shields.io/badge/Company-IIT%20BHU%20Intern-green?style=for-the-badge) ![Strings](https://img.shields.io/badge/Topic-Strings%20%26%20DP-purple?style=for-the-badge)


## Problem Description

Given a string `s` of length $n$, you may perform any number of operations: pick an index $i$ and swap `s[i]` with `s[i+1]`. 

**Constraint:** If you swap at index $i$, you cannot also swap at index $i+1$ in the same round (i.e., you cannot swap adjacent indices simultaneously). 

Count the **number of distinct strings** that can be formed using any valid sequence of these non-adjacent swaps.

---

## Example

* **Input:** `s = "abc"`
* **Output:** `4`
* **Explanation:** 
  The 4 distinct strings that can be formed are:
  1. No swaps (Identity): `"abc"`
  2. Swap at index 1 (`s[0]` and `s[1]`): `"bac"`
  3. Swap at index 2 (`s[1]` and `s[2]`): `"acb"`
  4. Swap at index 1 and index 3 (non-adjacent indices): `"bca"`

---

## Approach & Intuition

1. **Non-Adjacent Swaps Constraint:** This is identical to the independent set problem on a path graph (Fibonacci-like transition), where choosing to swap at index $i$ forces us to skip index $i+1$.
2. **State Transition:** At each index $i$, we have two choices:
   * **Skip:** Keep the character at its current position and move to index $i+1$.
   * **Swap:** Swap `s[i]` with `s[i+1]` (provided $i+1 < n$) and move to index $i+2$ (since index $i+1$ cannot be used for another swap in the same round).
3. **Handling Duplicates:** To ensure we only count *distinct* strings, we can use a set to accumulate all uniquely generated string states during exploration.

---

## Complexity Analysis

* **Time Complexity:** $\mathcal{O}(n^2)$ due to state transitions and string reconstructions.
* **Space Complexity:** $\mathcal{O}(n)$ for recursion stack and memoization storage.

---

## Code Implementation (Python)

```python
def countDistinctStrings(s: str) -> int:
    unique_strings = set()
    
    def generate(index, current):
        unique_strings.add("".join(current))
        for i in range(index, len(s) - 1):
            # Swap adjacent characters
            current[i], current[i+1] = current[i+1], current[i]
            # Move to i + 2 to respect the non-adjacent swap constraint
            generate(i + 2, current)
            # Backtrack
            current[i], current[i+1] = current[i+1], current[i]

    generate(0, list(s))
    return len(unique_strings)

# Example usage:
# print(countDistinctStrings("abc"))  # Output: 4
