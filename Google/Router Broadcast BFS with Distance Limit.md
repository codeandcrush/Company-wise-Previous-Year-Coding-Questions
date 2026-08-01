# Router Broadcast BFS with Distance Limit
![Medium](https://img.shields.io/badge/Level-Medium-yellow?style=for-the-badge)
![Phone Screen 2024](https://img.shields.io/badge/Source-Phone%20Screen%202024-blue?style=for-the-badge)
![Graphs](https://img.shields.io/badge/Topic-Graphs%20%26%20BFS-purple?style=for-the-badge)
![BFS](https://img.shields.io/badge/Technique-Breadth%20First%20Search-orange?style=for-the-badge)

---

## Problem Description

A network consists of routers connected as an **undirected graph**.

A source router broadcasts a message to every router that is reachable within **`d` hops**.

Given:

- An undirected graph represented by `edges`
- A source router `source`
- A destination router `dest`
- A maximum hop limit `d`

Determine whether the destination router receives the broadcast.

Return:

- `True` if `dest` can be reached from `source` in **at most `d` hops**
- `False` otherwise

---

## Example 1

**Input**

```text
edges = [[0,1],[1,2],[2,3]]
source = 0
dest = 2
d = 2
```

**Output**

```text
True
```

**Explanation**

```text
0 → 1 → 2

Total hops = 2
Allowed hops = 2

Destination receives the broadcast.
```

---

## Example 2

**Input**

```text
edges = [[0,1],[1,2],[2,3]]
source = 0
dest = 3
d = 2
```

**Output**

```text
False
```

**Explanation**

```text
0 → 1 → 2 → 3

Total hops = 3
Allowed hops = 2

Destination is beyond the broadcast range.
```

---

## Approach & Intuition

Since every edge represents **one hop**, this is a classic **Breadth-First Search (BFS)** problem.

BFS explores nodes **level by level**, where:

- Level 0 → Source router
- Level 1 → Routers one hop away
- Level 2 → Routers two hops away
- ...

During traversal:

- Track the current hop count.
- Stop exploring paths once the hop count exceeds `d`.
- If the destination is reached within `d` hops, return `True`.

Because BFS always discovers the shortest path in an unweighted graph, the first time we visit `dest` is guaranteed to be its minimum hop distance.

---

## Algorithm

1. Build an adjacency list from the given edges.
2. Initialize a BFS queue with `(source, 0)` where `0` is the hop count.
3. Maintain a `visited` set to avoid revisiting routers.
4. While the queue is not empty:
   - Pop the current router and its hop count.
   - If it is the destination, return `True`.
   - If the hop count equals `d`, do not expand its neighbors.
   - Otherwise, enqueue all unvisited neighbors with hop count `+1`.
5. If BFS finishes without reaching `dest`, return `False`.

---

## Complexity Analysis

- **Time Complexity:** `O(V + E)`
- **Space Complexity:** `O(V)`

Where:

- `V` = Number of routers (vertices)
- `E` = Number of connections (edges)

---

## Code Implementation (Python)

```python
from collections import defaultdict, deque

def receivesBroadcast(edges, source, dest, d):
    graph = defaultdict(list)

    # Build graph
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    queue = deque([(source, 0)])
    visited = {source}

    while queue:
        node, hops = queue.popleft()

        if node == dest:
            return True

        if hops == d:
            continue

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, hops + 1))

    return False


# Example Usage
edges = [[0,1],[1,2],[2,3]]

print(receivesBroadcast(edges, 0, 2, 2))   # True
print(receivesBroadcast(edges, 0, 3, 2))   # False
```

---

## Dry Run

### Input

```text
edges = [[0,1],[1,2],[2,3]]
source = 0
dest = 2
d = 2
```

### BFS Traversal

| Queue | Current Router | Hops | Action |
|--------|----------------|------|--------|
| [(0,0)] | 0 | 0 | Visit neighbors |
| [(1,1)] | 1 | 1 | Visit neighbors |
| [(2,2)] | 2 | 2 | Destination reached ✅ |

Result:

```text
True
```

---

## Follow-up

### What if edges have weights?

If each connection has a weight (latency, cost, distance, etc.), BFS is no longer valid because the shortest path is no longer determined by hop count.

Instead:

- Use **Dijkstra's Algorithm**.
- Compute the minimum weighted distance from `source`.
- Return:

```text
dist[dest] ≤ d
```

### Complexity (Weighted Graph)

- **Time Complexity:** `O((V + E) log V)`
- **Space Complexity:** `O(V)`

---

## Key Takeaways

- ✅ BFS gives the shortest path in terms of **number of hops**.
- ✅ Level order traversal naturally represents broadcast distance.
- ✅ Stop expanding nodes once the hop limit is reached.
- ✅ For weighted graphs, replace BFS with **Dijkstra's Algorithm**.
- ✅ This is a common interview problem involving **Graphs**, **BFS**, and **Shortest Path**.
