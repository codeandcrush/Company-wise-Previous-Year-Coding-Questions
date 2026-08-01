# Minimum Weight to Disconnect All Leaf Nodes
![Hard](https://img.shields.io/badge/Level-Hard-red?style=for-the-badge)
![Onsite 2024](https://img.shields.io/badge/Source-Onsite%202024-blue?style=for-the-badge)
![Actual Reported](https://img.shields.io/badge/Interview-Actual%20Reported-green?style=for-the-badge)
![Trees](https://img.shields.io/badge/Topic-Trees%20%26%20DFS-purple?style=for-the-badge)
![Greedy](https://img.shields.io/badge/Technique-DFS%20%2B%20Greedy-orange?style=for-the-badge)

---

## Problem Description

You are given a **weighted binary tree** where every edge has a positive weight.

Your task is to remove a set of edges such that **every leaf node becomes unreachable from the root**.

Return the **minimum total weight** of the removed edges.

---

## Example

**Input**

```text
           Root
          /    \
      (5)/      \(7)
        A       Leaf2
        |
      (2)
        |
      Leaf1
```

Edge weights:

```text
Root → A      = 5
A → Leaf1     = 2
Root → Leaf2  = 7
```

**Output**

```text
9
```

**Explanation**

There are two root-to-leaf paths:

```text
Root → A → Leaf1
Root → Leaf2
```

For the first path, removing the edge with weight **2** is cheaper than removing the edge with weight **5**.

For the second path, the only available edge has weight **7**.

Therefore,

```text
2 + 7 = 9
```

---

## Approach & Intuition

Every leaf must be disconnected from the root.

For a **single root-to-leaf path**, disconnecting the leaf only requires removing **one edge** from that path.

To minimize the cost for that path:

> Remove the **minimum-weight edge** on the path.

During DFS:

- Maintain the smallest edge weight seen from the root to the current node.
- Whenever a leaf is reached, add that minimum edge weight to the answer.

This greedy choice is optimal **only when root-to-leaf paths are edge-disjoint**.

---

## Algorithm

1. Start a DFS from the root.
2. Pass along the minimum edge weight encountered so far.
3. If the current node is a leaf:
   - Add the minimum edge weight on its path to the answer.
4. Continue recursively for the left and right subtrees.
5. Return the accumulated cost.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(h)`

Where:

- `n` = Number of nodes
- `h` = Height of the tree

---

## Code Implementation (Python)

```python
class TreeNode:
    def __init__(self):
        self.left = None
        self.right = None
        self.leftWeight = None
        self.rightWeight = None


def minimumDisconnectCost(root):
    if root is None:
        return 0

    answer = 0

    def dfs(node, min_edge):
        nonlocal answer

        if node.left is None and node.right is None:
            answer += min_edge
            return

        if node.left:
            dfs(node.left, min(min_edge, node.leftWeight))

        if node.right:
            dfs(node.right, min(min_edge, node.rightWeight))

    INF = float("inf")
    dfs(root, INF)

    return answer
```

---

## Dry Run

Tree:

```text
           Root
          /    \
      (5)/      \(7)
        A       Leaf2
        |
      (2)
        |
      Leaf1
```

### DFS Traversal

| Current Path | Minimum Edge So Far | Leaf? | Cost Added |
|--------------|---------------------|--------|------------|
| Root → A | 5 | No | — |
| Root → A → Leaf1 | 2 | Yes | 2 |
| Root → Leaf2 | 7 | Yes | 7 |

Final answer:

```text
2 + 7 = 9
```

---

## Edge Cases

- Single-node tree (root is also a leaf) → No edges need to be removed, answer = `0`.
- Tree with only one root-to-leaf path → Remove the minimum-weight edge on that path.
- Balanced or skewed trees → DFS naturally handles both structures.

---

## Key Takeaways

- ✅ Every leaf must be separated from the root.
- ✅ For an individual root-to-leaf path, removing the **cheapest edge** disconnects that leaf at minimum cost.
- ✅ DFS efficiently tracks the minimum edge weight on the current path.
- ✅ Time complexity is linear, making it suitable for large trees.

---

## ⚠️ Interview Discussion

The greedy DFS solution above is **correct only if cutting an edge affects exactly one leaf path** (i.e., the root-to-leaf paths do not share useful cut edges).

For the **general version** of this problem on arbitrary trees, the greedy approach is **not always optimal**.

Example:

```text
        Root
          |
        (1)
          |
          A
         / \
      (100) (100)
      L1     L2
```

- Greedy per leaf would choose:

```text
100 + 100 = 200
```

- But cutting the shared edge:

```text
Root → A (weight = 1)
```

disconnects **both leaves** for a total cost of **1**.

Therefore, the general problem requires a **Tree Dynamic Programming (DP)** solution rather than a simple greedy DFS.

This distinction is a common follow-up in onsite interviews.
