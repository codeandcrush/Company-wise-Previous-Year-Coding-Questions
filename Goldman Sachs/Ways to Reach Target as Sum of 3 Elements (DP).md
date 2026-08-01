# Ways to Reach Target as Sum of Exactly 3 Elements
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![OA](https://img.shields.io/badge/Source-OA%20%C2%B7%20Frequently%20Reported-blue?style=for-the-badge)
![Arrays](https://img.shields.io/badge/Topic-Arrays%20%26%20DP-purple?style=for-the-badge)
![Dynamic Programming](https://img.shields.io/badge/Technique-Dynamic%20Programming-orange?style=for-the-badge)

---

## Problem Description

You are given an integer array `arr`.

For every index `i`, determine the number of ways to express

```text
arr[i]
```

as the sum of **exactly 3 integers** selected from

```text
arr[0...i-1]
```

### Rules

- Each chosen number must come from the prefix `arr[0...i-1]`.
- **Infinite supply** of every available number.
- **Repetition is allowed.**
- Order **does not matter** (i.e., combinations are counted, not permutations).

Return the number of ways for every index.

---

## Example

### Input

```text
arr = [1, 2, 3, 4]
```

### Output

```text
[0, 0, 1, 1]
```

### Explanation

### Index 0

```text
Target = 1
No previous numbers exist.

Ways = 0
```

---

### Index 1

```text
Target = 2
Available numbers = {1}

Need exactly 3 numbers.

Impossible.

Ways = 0
```

---

### Index 2

```text
Target = 3
Available = {1,2}

Possible combinations:

1 + 1 + 1 = 3
```

Ways = **1**

---

### Index 3

```text
Target = 4
Available = {1,2,3}

Possible combinations:

1 + 1 + 2 = 4
```

Ways = **1**

---

## Approach & Intuition

This is a variation of the classic **Coin Change (Count Ways)** problem.

The differences are:

- We need **exactly 3 coins**.
- Coins can be reused infinitely.
- Order does **not** matter.

For every index `i`:

```text
Coins = arr[0...i-1]
Target = arr[i]
```

We solve an independent DP problem.

---

## DP State

Let

```text
dp[c][s]
```

represent

> Number of ways to make sum `s`
> using **exactly `c` coins**.

Since we only need **3 coins**, the number of rows is fixed.

---

## State Transition

For every coin:

```text
coin
```

For every number of chosen coins:

```text
c = 1 ... 3
```

For every achievable amount:

```text
s >= coin
```

Transition:

```text
dp[c][s] += dp[c-1][s-coin]
```

This is identical to Coin Change counting, except we also track how many coins have been used.

---

## Algorithm

For every index `i`:

1. Available coins:

```text
arr[0...i-1]
```

2. Initialize

```text
dp[0][0] = 1
```

3. Process every available coin.

4. Update DP for

```text
1 coin
2 coins
3 coins
```

5. Answer:

```text
dp[3][arr[i]]
```

Repeat for every index.

---

## Complexity Analysis

Let

- `n` = number of elements
- `M` = maximum value in the array

- **Time Complexity:** `O(n² × M)`
- **Space Complexity:** `O(3 × M)` ≈ `O(M)`

(The DP table has only 4 rows: 0, 1, 2, and 3 coins.)

---

## Code Implementation (Python)

```python
def countWays(arr):
    n = len(arr)
    answer = []

    for i in range(n):

        target = arr[i]
        coins = arr[:i]

        dp = [[0] * (target + 1) for _ in range(4)]
        dp[0][0] = 1

        for coin in coins:
            for used in range(1, 4):
                for amount in range(coin, target + 1):
                    dp[used][amount] += dp[used - 1][amount - coin]

        answer.append(dp[3][target])

    return answer


# Example
arr = [1,2,3,4]

print(countWays(arr))
```

Output

```text
[0,0,1,1]
```

---

## Dry Run

### Input

```text
arr = [1,2,3,4]
```

### Index = 2

Target:

```text
3
```

Available coins:

```text
1,2
```

DP discovers:

```text
1 + 1 + 1 = 3
```

Answer:

```text
1
```

---

### Index = 3

Target:

```text
4
```

Coins:

```text
1,2,3
```

Valid combination:

```text
1 + 1 + 2 = 4
```

Answer:

```text
1
```

---

## Edge Cases

- Empty prefix → `0` ways.
- Target smaller than the minimum possible sum using 3 coins → `0`.
- Duplicate values in the prefix are treated as the same denomination because the supply is infinite.
- If no combination exists → `0`.

---

## Key Takeaways

- ✅ This is a **Coin Change (Count Ways)** variation.
- ✅ The additional DP dimension tracks **how many coins have been chosen**.
- ✅ Since only **3 coins** are required, the DP remains compact.
- ✅ Repetition is allowed, so each available value behaves like an **unbounded coin denomination**.
- ✅ Overall complexity is `O(n² × maxValue)`.

---

## ⚠️ Interview Discussion

The statement **"repetition allowed"** is critical.

If repetition **is allowed** (as stated):

- This is an **Unbounded Coin Change** problem with an extra dimension for the number of coins.

If repetition **is not allowed**:

- The DP transition changes (0/1 Knapsack style), and iteration over the number of coins and amounts must proceed in reverse to avoid reusing the same element.

A good interview clarification question is:

> **"Should different orders like (1,1,2), (1,2,1), and (2,1,1) be counted separately, or only once?"**

The solution above assumes **combinations**, not permutations, which matches the examples provided.
