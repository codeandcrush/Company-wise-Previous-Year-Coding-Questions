# Burning Rope — Measure 45 Minutes
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![Puzzle Round](https://img.shields.io/badge/Source-Puzzle%20Round-blue?style=for-the-badge)
![GS Reported](https://img.shields.io/badge/Interview-GS%20Reported-green?style=for-the-badge)
![Math](https://img.shields.io/badge/Topic-Math%20%26%20Logic-purple?style=for-the-badge)
![Logic Puzzle](https://img.shields.io/badge/Technique-Logic%20Puzzle-orange?style=for-the-badge)

---

## Problem Description

You have **two ropes**.

Each rope takes exactly:

```text
60 minutes
```

to burn completely.

However, the ropes **do not burn at a uniform rate**.

For example, one half of a rope may take 10 minutes to burn while the other half takes 50 minutes.

Using only:

- 🔥 Matches
- 🪢 The two ropes

measure **exactly 45 minutes**.

---

## Solution

The key is to use the fact that a rope burning from **both ends** finishes in half of its total burn time, regardless of how unevenly it burns.

### Step 1 — Start Both Ropes

At the same time:

- Light **Rope 1 from both ends**.
- Light **Rope 2 from one end**.

```text
Rope 1: 🔥 ===== 🔥
Rope 2: 🔥 =========
```

---

### Step 2 — Wait 30 Minutes

Rope 1 is burning from both ends.

Since it normally takes 60 minutes from one end:

```text
60 / 2 = 30 minutes
```

Therefore, Rope 1 completely burns out after exactly **30 minutes**.

At this moment:

```text
Elapsed time = 30 minutes
```

Rope 2 has been burning from one end for 30 minutes.

Therefore, its **remaining burn time** is:

```text
30 minutes
```

---

### Step 3 — Light the Other End of Rope 2

At the exact moment Rope 1 burns out:

```text
🔥 ===== 🔥
    ↓
Rope 1 → Finished
```

Immediately light the **other end** of Rope 2.

Now Rope 2 is burning from **both ends**.

It had 30 minutes of burn time remaining from one end.

Therefore, burning it from both ends makes the remaining time:

```text
30 / 2 = 15 minutes
```

---

### Step 4 — Total Time

We already measured:

```text
30 minutes
```

Then Rope 2 takes another:

```text
15 minutes
```

Therefore:

```text
30 + 15 = 45 minutes
```

---

## Final Answer

```text
⏱️ Exactly 45 minutes
```

Timeline:

```text
0 min                         30 min                 45 min
  |-----------------------------|----------------------|
  🔥 Rope 1 both ends           🔥 Rope 2 both ends    🔥
                                                    
  Rope 2 starts one end         Light other end       Done
  ↓                              ↓
  └────────────── 30 min ───────┴──── 15 min ───────┘
```

---

## Key Insight

The rope burns at a **non-uniform rate**, so you cannot simply divide its physical length into halves.

But burning from **both ends simultaneously** is different.

If a rope has `T` minutes of total burn time from one end, then lighting both ends makes it finish in:

```text
T / 2
```

This remains true even when the burning rate is highly non-uniform.

The crucial trick is therefore:

> **Use the first rope to create a precise 30-minute checkpoint, then use that checkpoint to split the remaining 30-minute burn time of the second rope into 15 minutes.**

---

## Why Non-Uniform Burning Doesn't Matter

Suppose Rope 2 has an uneven burn pattern:

```text
Part A → burns in 5 minutes
Part B → burns in 25 minutes
Part C → burns in 30 minutes
```

The total is still:

```text
5 + 25 + 30 = 60 minutes
```

After it has burned for 30 minutes from one end, exactly **30 minutes of burn-time** remains.

When the other end is also ignited, the two flames consume that remaining burn-time together, causing the rope to finish after:

```text
30 / 2 = 15 minutes
```

The physical midpoint of the rope is irrelevant.

---

## Key Takeaways

- ✅ A 60-minute rope burning from both ends takes exactly **30 minutes**.
- ✅ Non-uniform burning does not affect this property.
- ✅ The first rope creates a reliable **30-minute checkpoint**.
- ✅ At 30 minutes, ignite the second end of Rope 2.
- ✅ The remaining 30-minute burn time becomes **15 minutes**.
- ✅ Total measured time = **45 minutes**.

---

## Interview Tip

When explaining this puzzle, emphasize:

> **"We are measuring burn time, not physical length."**

That single distinction explains why the solution works even though the ropes burn **non-uniformly**.
