# Poisoned Wine — Minimum Test Strips
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![Puzzle Round](https://img.shields.io/badge/Source-Puzzle%20Round-blue?style=for-the-badge)
![GS Classic](https://img.shields.io/badge/Interview-GS%20Classic-green?style=for-the-badge)
![Math](https://img.shields.io/badge/Topic-Math%20%26%20Logic-purple?style=for-the-badge)
![Binary](https://img.shields.io/badge/Technique-Binary%20Representation-orange?style=for-the-badge)

---

## Problem Description

You have:

```text
1000 wine bottles
```

Exactly **one bottle contains poison**.

You also have:

```text
10 test strips
```

Each test strip:

- Can receive drops from multiple bottles.
- Turns **positive after 30 minutes** if it comes into contact with poison.
- Otherwise remains negative.

You have **1 hour** and need to determine **which bottle is poisoned**.

What is the **minimum number of test strips** required?

---

## Answer

```text
10 strips
```

### Why?

Each test strip has two possible outcomes:

```text
Negative → 0
Positive → 1
```

Therefore, with `n` strips, we can create:

```text
2^n
```

different result patterns.

For 10 strips:

```text
2^10 = 1024
```

Since:

```text
1024 ≥ 1000
```

10 strips are sufficient to uniquely identify any of the 1000 bottles.

---

## Core Idea

Instead of testing bottles individually, assign each bottle a unique **10-bit binary number**.

For example:

```text
Bottle 1    → 0000000001
Bottle 2    → 0000000010
Bottle 3    → 0000000011
...
Bottle 1000 → unique 10-bit code
```

Each bit corresponds to one test strip.

```text
Bit 0 → Strip 1
Bit 1 → Strip 2
Bit 2 → Strip 3
...
Bit 9 → Strip 10
```

---

## Strategy

For every bottle:

1. Convert its bottle number into a 10-bit binary representation.
2. If bit `i` is `1`, put a drop from that bottle onto **Strip `i`**.
3. If bit `i` is `0`, don't put that bottle on Strip `i`.

After 30 minutes, observe which strips are positive.

The positive/negative pattern forms the binary representation of the poisoned bottle.

---

## Example

Suppose the poisoned bottle is:

```text
Bottle = 13
```

Binary representation:

```text
13 = 0000001101
```

Therefore, the corresponding strips receive:

```text
Strip 1 → Positive
Strip 2 → Negative
Strip 3 → Positive
Strip 4 → Positive
...
```

The observed pattern:

```text
0000001101
```

decodes back to:

```text
13
```

Therefore, Bottle 13 is poisoned.

---

## How the Encoding Works

Consider a smaller example with **4 strips**.

Four strips can distinguish:

```text
2^4 = 16
```

different bottles.

| Bottle | Binary Code | Strip 1 | Strip 2 | Strip 3 | Strip 4 |
|--------:|-------------|----------|----------|----------|----------|
| 0 | 0000 | ❌ | ❌ | ❌ | ❌ |
| 1 | 0001 | ✅ | ❌ | ❌ | ❌ |
| 2 | 0010 | ❌ | ✅ | ❌ | ❌ |
| 3 | 0011 | ✅ | ✅ | ❌ | ❌ |
| 4 | 0100 | ❌ | ❌ | ✅ | ❌ |
| 5 | 0101 | ✅ | ❌ | ✅ | ❌ |
| ... | ... | ... | ... | ... | ... |
| 15 | 1111 | ✅ | ✅ | ✅ | ✅ |

Every bottle gets a **unique result pattern**.

---

## Why 9 Strips Are Not Enough

With 9 strips:

```text
2^9 = 512
```

possible patterns are available.

But there are:

```text
1000 bottles
```

Since:

```text
512 < 1000
```

9 strips cannot uniquely identify every bottle.

Therefore:

```text
Minimum = 10 strips
```

---

## Mathematical Proof

With `n` binary test strips, the maximum number of distinguishable outcomes is:

```text
2^n
```

We need:

```text
2^n ≥ 1000
```

Taking logarithm:

```text
n ≥ log₂(1000)
```

Since:

```text
log₂(1000) ≈ 9.97
```

we need:

```text
n = 10
```

---

## Code Implementation (Python)

The following function demonstrates how to generate the strip assignment for every bottle:

```python
def generate_test_plan(num_bottles, num_strips):
    plan = {}

    for bottle in range(1, num_bottles + 1):
        binary_code = format(bottle - 1, f"0{num_strips}b")

        strips = []

        for i, bit in enumerate(binary_code):
            if bit == "1":
                strips.append(i + 1)

        plan[bottle] = strips

    return plan


# Example
plan = generate_test_plan(1000, 10)

print("Bottle 1:", plan[1])
print("Bottle 13:", plan[13])
```

---

## Decoding the Result

Suppose after 30 minutes the strips show:

```text
Strip 1 → Positive
Strip 2 → Negative
Strip 3 → Positive
Strip 4 → Positive
```

and the remaining strips are negative.

The positive pattern represents a binary number.

Convert that binary number to decimal to identify the poisoned bottle.

```python
def decode_result(results):
    """
    results:
        List of 0/1 values representing
        negative/positive strips.
    """

    binary_code = "".join(map(str, results))

    bottle_index = int(binary_code, 2)

    return bottle_index + 1


# Example:
# 0000001101 = 13
print(decode_result([0,0,0,0,0,0,1,1,0,1]))
```

---

## Complexity Analysis

For assigning bottles to strips:

- **Time Complexity:** `O(B × S)`
- **Space Complexity:** `O(B × S)`

Where:

- `B` = Number of bottles
- `S` = Number of strips

For this problem:

```text
B = 1000
S = 10
```

---

## Key Insight

The important idea is **information encoding**.

Each strip provides one binary piece of information:

```text
Positive = 1
Negative = 0
```

Therefore:

```text
10 strips
→ 10 bits
→ 2^10 possible patterns
→ 1024 possible bottles
```

Since 1000 bottles fit inside 1024 possible patterns, **10 strips are sufficient and optimal**.

---

## Key Takeaways

- ✅ Each test strip represents **one binary bit**.
- ✅ `n` strips can distinguish up to `2^n` possibilities.
- ✅ `2^10 = 1024`, which is enough for 1000 bottles.
- ✅ `2^9 = 512`, which is not enough.
- ✅ The positive-strip pattern directly identifies the poisoned bottle.
- ✅ This is a classic application of **Binary Representation + Information Theory**.

---

## Interview Tip

When asked for the minimum number of test strips, don't start by designing the testing procedure.

First ask:

> **"How many different outcomes can `n` strips produce?"**

Since each strip has two states:

```text
2^n
```

Then solve:

```text
2^n ≥ number of bottles
```

For 1000 bottles:

```text
2^10 = 1024
```

Therefore:

```text
🎯 Answer = 10 strips
```
