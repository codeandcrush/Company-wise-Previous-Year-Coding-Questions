# Binary Search with Hidden Array + Trie Follow-up
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![Phone Screen 2024](https://img.shields.io/badge/Source-Phone%20Screen%202024-blue?style=for-the-badge)
![Binary Search](https://img.shields.io/badge/Topic-Binary%20Search-purple?style=for-the-badge)
![Trie](https://img.shields.io/badge/Follow--up-Trie-orange?style=for-the-badge)

---

# Part 1: Count 1s in a Hidden Binary Array

## Problem Description

You are given access to a **hidden binary array** of length `n`.

Properties:

- The array is sorted in **descending order**.
- All `1`s appear before all `0`s.

Example:

```text
[1,1,1,1,0,0,0,0,0,0]
```

You **cannot access the array directly**.

Instead, the only available API is:

```python
query(i)
```

which returns the value at index `i`.

Your task is to determine the **number of 1s** using the **minimum number of queries**.

---

## Example

**Input**

```text
n = 10

Hidden Array:
[1,1,1,1,0,0,0,0,0,0]
```

**Output**

```text
4
```

**Explanation**

The last occurrence of `1` is at index `3`.

Therefore,

```text
Number of 1s = 3 + 1 = 4
```

---

## Approach & Intuition

Since the array is already sorted,

```text
111111100000
```

there is exactly **one transition point** where `1` changes to `0`.

We need to locate this boundary.

Binary Search is ideal because:

- If `query(mid) == 1`
  - The boundary lies to the **right**.
- If `query(mid) == 0`
  - The boundary lies to the **left**.

The last position containing `1` determines the answer.

---

## Algorithm

1. Set

```text
left = 0
right = n-1
lastOne = -1
```

2. While `left <= right`

- Compute `mid`.
- If `query(mid) == 1`
  - Store `mid`
  - Search the right half.
- Otherwise
  - Search the left half.

3. Return

```text
lastOne + 1
```

---

## Complexity Analysis

- **Time Complexity:** `O(log n)` queries
- **Space Complexity:** `O(1)`

---

## Code Implementation (Python)

```python
def countOnes(n, query):
    left, right = 0, n - 1
    last_one = -1

    while left <= right:
        mid = (left + right) // 2

        if query(mid) == 1:
            last_one = mid
            left = mid + 1
        else:
            right = mid - 1

    return last_one + 1
```

---

## Dry Run

Hidden array:

```text
[1,1,1,1,0,0,0,0,0,0]
```

| Left | Right | Mid | query(mid) | Action |
|------|-------|-----|------------|--------|
| 0 | 9 | 4 | 0 | Search Left |
| 0 | 3 | 1 | 1 | Search Right |
| 2 | 3 | 2 | 1 | Search Right |
| 3 | 3 | 3 | 1 | Search Right |

Last `1` found at index `3`.

```text
Answer = 3 + 1 = 4
```

---

# Follow-up: Prefix Queries using a Trie

## Problem Description

You are given a dictionary of words.

Support efficient prefix queries:

```python
countWithPrefix(prefix)
```

which returns the number of words that begin with the given prefix.

---

## Example

**Dictionary**

```text
[
    "apple",
    "app",
    "ape",
    "banana",
    "band",
    "bat"
]
```

**Queries**

```text
countWithPrefix("ap")  → 3

countWithPrefix("app") → 2

countWithPrefix("ban") → 2

countWithPrefix("cat") → 0
```

---

## Approach & Intuition

A **Trie (Prefix Tree)** stores words character by character.

Each node maintains:

- Children
- End-of-word flag
- A counter indicating how many words pass through the node

During insertion:

- Increment the counter for every visited node.

During querying:

- Traverse the prefix.
- If traversal succeeds, return the stored counter.
- Otherwise return `0`.

This avoids scanning every word.

---

## Algorithm

### Insert

For every word:

- Traverse/create nodes.
- Increment each node's prefix count.

### Query

- Walk through the prefix.
- If any character is missing

```text
Return 0
```

Otherwise

```text
Return node.count
```

---

## Complexity Analysis

### Build Trie

- **Time:** `O(total characters)`
- **Space:** `O(total characters)`

### Prefix Query

- **Time:** `O(length of prefix)`
- **Space:** `O(1)`

---

## Code Implementation (Python)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.count = 0
        self.is_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root

        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()

            node = node.children[ch]
            node.count += 1

        node.is_word = True

    def countWithPrefix(self, prefix):
        node = self.root

        for ch in prefix:
            if ch not in node.children:
                return 0

            node = node.children[ch]

        return node.count


# Example
words = ["apple", "app", "ape", "banana", "band", "bat"]

trie = Trie()

for word in words:
    trie.insert(word)

print(trie.countWithPrefix("ap"))    # 3
print(trie.countWithPrefix("app"))   # 2
print(trie.countWithPrefix("ban"))   # 2
print(trie.countWithPrefix("cat"))   # 0
```

---

## Key Takeaways

### Binary Search Problem

- ✅ Hidden arrays can only be accessed through an API.
- ✅ Binary Search minimizes the number of queries to **O(log n)**.
- ✅ Find the **last occurrence of `1`** to compute the answer.

### Trie Follow-up

- ✅ Trie is the standard data structure for prefix searching.
- ✅ Store a prefix count at every node for fast queries.
- ✅ Building the Trie takes **O(total characters)**.
- ✅ Each prefix query runs in **O(length of prefix)**.

---

## Interview Tips

- 🔹 Hidden-array problems test your ability to optimize **API calls**, not array traversal.
- 🔹 Mention that binary search minimizes expensive queries.
- 🔹 In the Trie follow-up, explain why storing a **prefix count** at each node avoids traversing the entire subtree during every query.
- 🔹 These two questions together commonly assess **Binary Search**, **Data Structures**, and **API-based problem solving** in phone screens.
