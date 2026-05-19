# 📚 WIA1002 Data Structure — Graph (Complete Study Notes)
> **Course:** WIA1002 Data Structure | Universiti Malaya | Sem 2, 2025/2026  
> **Topic:** Graph — Concept, Modelling, Implementation, Traversal  
> **Sources:** Lecture slides (007-Graph.pdf) + Lab (L8) + Tutorial (T08)

---

## 📋 Table of Contents
1. [What is a Graph?](#1-what-is-a-graph)
2. [Graph Terminology](#2-graph-terminology)
3. [Types of Graphs](#3-types-of-graphs)
4. [Modelling a Graph](#4-modelling-a-graph)
5. [Adjacency Matrix — Deep Dive](#5-adjacency-matrix--deep-dive)
6. [Adjacency List — Deep Dive](#6-adjacency-list--deep-dive)
7. [Matrix vs List — When to Use Which](#7-matrix-vs-list--when-to-use-which)
8. [Implementing Graphs in Java](#8-implementing-graphs-in-java)
9. [All Graph Methods — Full Code + Explanation](#9-all-graph-methods--full-code--explanation)
10. [Graph Traversal — DFS](#10-graph-traversal--depth-first-search-dfs)
11. [Graph Traversal — BFS](#11-graph-traversal--breadth-first-search-bfs)
12. [DFS vs BFS Comparison](#12-dfs-vs-bfs-comparison)
13. [Tutorial Answers (T08)](#13-tutorial-answers-t08)
14. [Lab Answers (L8)](#14-lab-answers-l8)
15. [Common Exam Mistakes](#15-common-exam-mistakes--tips)
16. [Practice Questions with Full Explanations](#16-practice-questions-with-full-explanations)

---

## 1. What is a Graph?

A **Graph** is both a mathematical concept and a data structure.

Formally: **G = (V, E)**  
- **V** = a set of **vertices** (also called nodes)  
- **E** = a set of **edges** (also called links or connections)

### Real-World Analogy

Think of Waze or Google Maps. Every **junction/intersection** is a vertex. Every **road connecting two junctions** is an edge. The entire road network = a graph.

Other everyday examples:

| Real World | Vertex (Node) | Edge (Connection) |
|-----------|--------------|-------------------|
| Flight routes (like the KL–Tawau example) | City (Alor Setar, Kuching...) | Flight route between cities |
| Facebook friends | Person | Friendship between two people |
| Website links | Web page | Hyperlink from one page to another |
| COVID-19 close contacts | Person | Physical contact between two people |
| University course prereqs | Course | "must take before" relationship |
| Grab routing | Pickup/dropoff point | Road segment |

### Key Insight
> In graph problems, **entities become vertices** and **interactions/relationships between entities become edges**.

---

## 2. Graph Terminology

### Vertices & Edges
```
Graph G with vertices 1,2,3,4,5,6 and edges:

        6
        |
    4 - 5 - 1
    |   |  /
    3 - 2
```

### Adjacent Vertices
Two vertices are **"adjacent"** if they share a direct edge (they are directly connected).

```
In the graph above:
- 1 and 2 are ADJACENT (direct edge between them)
- 1 and 5 are ADJACENT
- 1 and 3 are NOT adjacent (no direct edge)
- 4 and 6 are ADJACENT (via vertex 4-5... wait, 6 connects to 4, not 5)
```

> Think of it like: your neighbours in the housing estate are "adjacent" to you (share a fence). Someone two houses away is NOT adjacent.

### Path
A **path** from vertex P to vertex Q is a sequence of 1 or more edges that take you from P to Q.

```
Example: Is there a path from vertex 1 to vertex 6?
YES: 1 → 5 → 4 → 6  (going through 5 then 4 then reaching 6)
Also: 1 → 2 → 3 → 4 → 6
```

> Adjacent = 1 edge apart. Path = 1 or MORE edges.

### Degree
- **In-degree** of a vertex = number of edges **coming INTO** that vertex
- **Out-degree** of a vertex = number of edges **going OUT FROM** that vertex
- For undirected graphs: just called **degree** (same for both)

```
Directed graph example:
    A → B → C
        ↑
        D

In-degree of B  = 2  (edges from A and from D come into B)
Out-degree of B = 1  (edge from B to C)
In-degree of A  = 0  (no edges come into A)
Out-degree of A = 1  (edge from A to B)
```

---

## 3. Types of Graphs

### Directed vs Undirected

**Undirected Graph**: edges have NO direction — if you can go from A to B, you can also go from B to A.
```
A --- B --- C
|         /
D -------
(like a bidirectional road — can travel both ways)
```

**Directed Graph (Digraph)**: edges have a direction — shown with arrows. A→B does NOT mean B→A.
```
A → B → C
↑       |
D ←-----
(like a one-way street system)
```

> **Malaysian example**: LRT network is mostly undirected (can go both directions). But traffic flow on one-way streets (like those in KL city centre) = directed graph.

### Unweighted vs Weighted

**Unweighted Graph**: edges just show connection — no additional value.
```
A --- B --- C
(just tells you if connected or not)
```

**Weighted Graph**: each edge carries a value (weight). This weight can represent distance, cost, time, capacity, etc.
```
A --100km-- B --400km-- C
(tells you the actual distance between cities)
```

From the lecture: Alor Setar → Kuching = 1200, Kuching → Melaka = 800, etc. (flight distances in km)

### Four Combinations
You can mix and match:

| Type | Description | Example |
|------|-------------|---------|
| Undirected + Unweighted | Just connections | Facebook friends |
| Undirected + Weighted | Bidirectional with cost | Road map with km |
| Directed + Unweighted | One-way connections | Twitter followers |
| Directed + Weighted | One-way with cost | Flight routes with fare |

---

## 4. Modelling a Graph

There are two main ways to represent a graph mathematically before coding it:

### The Sample Graph Used in Lecture
```
Vertices: 1, 2, 3, 4, 5, 6

        6
        |
    4 - 5 - 1
    |   |  /
    3 - 2

Edges:
(1,2), (1,5), (2,3), (2,5), (3,4), (4,5), (4,6)
```

This same graph can be represented as:
1. **Adjacency Matrix** — a 2D grid of 1s and 0s
2. **Adjacency List** — each vertex has a list of its neighbours

---

## 5. Adjacency Matrix — Deep Dive

### What is it?
A 2D array of size **n × n** (where n = number of vertices).

`matrix[i][j] = 1` means there IS an edge from vertex i to vertex j.  
`matrix[i][j] = 0` means there is NO edge from vertex i to vertex j.

### Visual Representation
```
For the 6-vertex graph above:

         1   2   3   4   5   6
    1  [ 0   1   0   0   1   0 ]   → vertex 1 connects to: 2, 5
    2  [ 1   0   1   0   1   0 ]   → vertex 2 connects to: 1, 3, 5
    3  [ 0   1   0   1   0   0 ]   → vertex 3 connects to: 2, 4
    4  [ 0   0   1   0   1   1 ]   → vertex 4 connects to: 3, 5, 6
    5  [ 1   1   0   1   0   0 ]   → vertex 5 connects to: 1, 2, 4
    6  [ 0   0   0   1   0   0 ]   → vertex 6 connects to: 4
```

**How to read it:**
- To check if vertex 2 and vertex 5 are adjacent: look at `matrix[2][5]` → 1 = YES they're adjacent
- To check if vertex 2 and vertex 6 are adjacent: look at `matrix[2][6]` → 0 = NOT adjacent
- For an undirected graph: the matrix is always **symmetric** (matrix[i][j] == matrix[j][i])

### Undirected Unweighted — Java Code
```java
int[][] adjacencyMatrix = {
    { 0, 1, 0, 0, 0, 1 },  // Alor Setar  (index 0)
    { 1, 0, 0, 1, 0, 1 },  // Kuching     (index 1)
    { 0, 0, 0, 0, 1, 0 },  // Langkawi    (index 2)
    { 0, 1, 0, 0, 1, 0 },  // Melaka      (index 3)
    { 0, 0, 1, 1, 0, 0 },  // Penang      (index 4)
    { 1, 1, 0, 0, 0, 0 }   // Tawau       (index 5)
};
```

**How to read this code:**
Row 0 (Alor Setar): `{0,1,0,0,0,1}` → connected to index 1 (Kuching) and index 5 (Tawau)
Row 1 (Kuching): `{1,0,0,1,0,1}` → connected to Alor Setar, Melaka, Tawau

### Directed Weighted — Java Code
```java
int[][] adjacencyMatrix = {
    {    0, 1200,   0,   0,   0,    0 },  // Alor Setar → Kuching (1200)
    {    0,    0,   0, 800,   0,  900 },  // Kuching → Melaka(800), Tawau(900)
    {    0,    0,   0,   0, 100,    0 },  // Langkawi → Penang (100)
    {    0,    0,   0,   0, 400,    0 },  // Melaka → Penang (400)
    {    0,    0,   0,   0,   0,    0 },  // Penang → (nothing)
    { 1900,    0,   0,   0,   0,    0 }   // Tawau → Alor Setar (1900)
};
```

**How to read:** `adjacencyMatrix[0][1] = 1200` means there is a directed edge from Alor Setar (index 0) to Kuching (index 1) with weight 1200.

`adjacencyMatrix[1][0] = 0` means there is NO edge from Kuching back to Alor Setar — this is a directed (one-way) graph.

### Pros and Cons of Adjacency Matrix

| Pros | Cons |
|------|------|
| Fast to check if edge exists: O(1) | Wastes memory for sparse graphs: O(n²) space |
| Easy to implement | Adding/removing vertices requires creating new matrix |
| Good for dense graphs (many edges) | n² space even if very few edges |

---

## 6. Adjacency List — Deep Dive

### What is it?
For each vertex, maintain a **list of its adjacent (neighbouring) vertices**.

### Visual Representation
```
For the same 6-vertex graph:

Node List:
  1  →  [2, 5]
  2  →  [1, 3, 5]
  3  →  [2, 4]
  4  →  [3, 5, 6]
  5  →  [1, 2, 4]
  6  →  [4]
```

### For the Directed Weighted Flight Graph:
```
Vertex Array (main list):
  [0] Alor Setar  ──→  [Kuching, 1200] → NULL
  [1] Kuching     ──→  [Melaka, 800] → [Tawau, 900] → NULL
  [2] Langkawi    ──→  [Penang, 100] → NULL
  [3] Melaka      ──→  [Penang, 400] → NULL
  [4] Penang      ──→  NULL  (no outgoing flights!)
  [5] Tawau       ──→  [Alor Setar, 1900] → NULL
```

### Structure of Each Edge Node
```
┌──────────────────┬───────────────┬──────────────────────┐
│  Reference to    │  Weight of    │  Reference to        │
│  adjacent vertex │  the edge     │  next edge node ──→  │
└──────────────────┴───────────────┴──────────────────────┘
```

Each edge node contains 3 things:
1. **toVertex** — which vertex does this edge point to?
2. **weight** — what's the weight of this edge?
3. **nextEdge** — pointer to the next edge (since a vertex can have multiple neighbours)

### Pros and Cons of Adjacency List

| Pros | Cons |
|------|------|
| Space-efficient for sparse graphs: O(V+E) | Slower to check if edge exists: O(degree) |
| Easy to add new vertices | More complex to implement |
| Better for most real-world graphs (sparse) | Weighted graphs need extra field per edge node |

---

## 7. Matrix vs List — When to Use Which

```
DECISION GUIDE:
                        
  Many edges (dense)?        Few edges (sparse)?
  (n² edges ≈ n vertices²)   (much fewer edges than n²)
         ↓                           ↓
   ADJACENCY MATRIX          ADJACENCY LIST
   - Less wasted space        - Saves memory
   - Fast lookup              - Better for traversal
   - e.g., COVID contacts     - e.g., road maps
     in a city of millions      city maps, flights
```

**Rule of thumb:**
- If `E ≈ V²` → use **adjacency matrix** (dense → matrix is "full" anyway)
- If `E << V²` → use **adjacency list** (sparse → most matrix cells would be 0, waste of space)

> **Example from lecture:** A COVID-19 close-contact graph for an entire country. If you have 30 million people (30M vertices) but each person only contacts ~10 others per day, you'd need a 30M × 30M = 900 trillion cell matrix just to store mostly zeros. Use adjacency list instead!

---

## 8. Implementing Graphs in Java

The lecture implements a **Directed Weighted Graph** using the **linked-list (adjacency list with linked structure)** approach.

### Three Classes Needed

```
Graph<T,N>
    |
    +--> head (Vertex<T,N>)
              |
              +--> vertexInfo  (T)    ← the vertex data (e.g., "Kuching")
              +--> indeg        (int)  ← how many edges come IN
              +--> outdeg       (int)  ← how many edges go OUT
              +--> nextVertex  (Vertex<T,N>) ← link to next vertex in list
              |
              +--> firstEdge   (Edge<T,N>)
                        |
                        +--> toVertex   (Vertex<T,N>) ← destination vertex
                        +--> weight     (N)            ← edge weight
                        +--> nextEdge  (Edge<T,N>)    ← link to next edge
```

### 8.1 — The Vertex Class

```java
class Vertex<T extends Comparable<T>, N extends Comparable<N>> {

    T vertexInfo;           // The actual data stored in this vertex
                            // (e.g., "Kuching" for a city graph)

    int indeg;              // In-degree: how many edges point INTO this vertex
    int outdeg;             // Out-degree: how many edges go OUT from this vertex

    Vertex<T, N> nextVertex;  // Pointer to the NEXT vertex in the vertex list
                              // (NOT an adjacent vertex — this links vertices together)

    Edge<T, N> firstEdge;     // Pointer to the FIRST outgoing edge from this vertex

    // Default constructor
    public Vertex() {
        vertexInfo  = null;
        indeg       = 0;
        outdeg      = 0;
        nextVertex  = null;
        firstEdge   = null;
    }

    // Parameterised constructor
    public Vertex(T vInfo, Vertex<T, N> next) {
        vertexInfo  = vInfo;
        indeg       = 0;
        outdeg      = 0;
        nextVertex  = next;
        firstEdge   = null;
    }
}
```

**Visual of the Vertex node structure:**
```
┌─────────────┬──────────────────────┬─────────────────────────┐
│  Vertex     │  Reference to        │  Reference to first     │
│  Info (T)   │  next vertex ──────→ │  edge node ──────────→  │
└─────────────┴──────────────────────┴─────────────────────────┘
```

> ⚠️ **Important distinction:**
> - `nextVertex` links to the **next vertex in the vertex list** (like a linked list of all vertices)
> - `firstEdge` links to the **first edge going OUT from this vertex** (the adjacency list for this vertex)
> - These are TWO separate pointer fields!

### 8.2 — The Edge Class

```java
class Edge<T extends Comparable<T>, N extends Comparable<N>> {

    Vertex<T, N> toVertex;   // The destination vertex of this edge
    N weight;                 // The weight of this edge
    Edge<T, N> nextEdge;      // Pointer to the next edge from the same source vertex

    // Default constructor
    public Edge() {
        toVertex  = null;
        weight    = null;
        nextEdge  = null;
    }

    // Parameterised constructor
    public Edge(Vertex<T, N> destination, N w, Edge<T, N> a) {
        toVertex  = destination;   // where does this edge go?
        weight    = w;             // how much does it cost/weigh?
        nextEdge  = a;             // what's the next edge from the same source?
    }
}
```

**Visual of the Edge node structure:**
```
┌──────────────────┬───────────────┬──────────────────────┐
│  Reference to    │  Weight of    │  Reference to        │
│  adjacent vertex │  the edge     │  next edge node ──→  │
└──────────────────┴───────────────┴──────────────────────┘
```

### 8.3 — The Graph Class (skeleton)

```java
class Graph<T extends Comparable<T>, N extends Comparable<N>> {

    Vertex<T, N> head;   // Pointer to the FIRST vertex in the graph
    int size;            // Total number of vertices in the graph

    // Constructor — empty graph
    public Graph() {
        head = null;
        size = 0;
    }
}
```

---

## 9. All Graph Methods — Full Code + Explanation

### 9.1 — getSize(): Get Number of Vertices

```java
public int getSize() {
    return this.size;
}
```

Simple — just return the stored count. `size` is incremented every time a vertex is successfully added.

---

### 9.2 — hasVertex(): Check if Vertex Exists

```java
public boolean hasVertex(T v) {
    if (head == null)
        return false;                          // empty graph → vertex can't exist

    Vertex<T, N> temp = head;                  // start from the first vertex
    while (temp != null) {                     // loop through all vertices
        if (temp.vertexInfo.compareTo(v) == 0) // found a match?
            return true;
        temp = temp.nextVertex;                // move to the next vertex
    }
    return false;                              // looped through all, not found
}
```

**Trace example:** Does "Melaka" exist?
```
temp = head (Alor Setar)
  → Alor Setar.compareTo("Melaka") ≠ 0, move to nextVertex
temp = Kuching
  → Kuching.compareTo("Melaka") ≠ 0, move to nextVertex
temp = Langkawi
  → ...
temp = Melaka
  → Melaka.compareTo("Melaka") == 0 → return TRUE ✅
```

---

### 9.3 — getIndeg(): Get In-Degree of a Vertex

```java
public int getIndeg(T v) {
    if (hasVertex(v) == true) {
        Vertex<T, N> temp = head;
        while (temp != null) {
            if (temp.vertexInfo.compareTo(v) == 0)
                return temp.indeg;             // found vertex, return its in-degree
            temp = temp.nextVertex;
        }
    }
    return -1;                                 // vertex not in graph → return -1
}
```

> **getOutdeg()** would be identical but returns `temp.outdeg` instead. Try writing it yourself!

---

### 9.4 — addVertex(): Add a New Vertex

```java
public boolean addVertex(T v) {
    if (hasVertex(v) == false) {               // only add if vertex doesn't exist yet
        Vertex<T, N> temp = head;
        Vertex<T, N> newVertex = new Vertex<>(v, null);  // create new vertex

        if (head == null) {
            head = newVertex;                  // empty graph → new vertex becomes head
        } else {
            // traverse to find the LAST vertex
            Vertex<T, N> previous = head;
            while (temp != null) {
                previous = temp;               // track the last valid vertex
                temp = temp.nextVertex;
            }
            previous.nextVertex = newVertex;   // append new vertex at the end
        }
        size++;
        return true;
    } else {
        return false;                          // vertex already exists → return false
    }
}
```

**Step-by-step for an empty graph:**
```
addVertex("Alor Setar"):
  hasVertex("Alor Setar") = false (graph empty)
  newVertex = new Vertex("Alor Setar", null)
  head == null → head = newVertex
  size = 1
  return true

addVertex("Kuching"):
  hasVertex("Kuching") = false
  newVertex = new Vertex("Kuching", null)
  head != null → traverse to last vertex (Alor Setar)
  Alor Setar.nextVertex = newVertex (Kuching)
  size = 2
  return true

Result: head → [Alor Setar] → [Kuching] → null
```

---

### 9.5 — getIndex(): Get Position of a Vertex

```java
public int getIndex(T v) {
    Vertex<T, N> temp = head;
    int pos = 0;
    while (temp != null) {
        if (temp.vertexInfo.compareTo(v) == 0)
            return pos;                        // found it, return position
        temp = temp.nextVertex;
        pos += 1;                              // increment position counter
    }
    return -1;                                 // not found → -1
}
```

**Example:**
```
Vertices in order: Alor Setar(0), Kuching(1), Langkawi(2), Melaka(3), Penang(4), Tawau(5)
getIndex("Kuching")  → returns 1
getIndex("Ipoh")     → returns -1 (not in graph)
```

---

### 9.6 — getAllVertexObjects(): Get All Vertices as ArrayList

```java
public ArrayList<T> getAllVertexObjects() {
    ArrayList<T> list = new ArrayList<>();
    Vertex<T, N> temp = head;
    while (temp != null) {
        list.add(temp.vertexInfo);             // add vertex info to the list
        temp = temp.nextVertex;
    }
    return list;
}
```

**Result for our example:**
`getAllVertexObjects()` → `["Alor Setar", "Kuching", "Langkawi", "Melaka", "Penang", "Tawau"]`

---

### 9.7 — getVertex(): Get Vertex Info at a Specific Position

```java
public T getVertex(int pos) {
    if (pos > size - 1 || pos < 0)
        return null;                           // invalid position → return null

    Vertex<T, N> temp = head;
    for (int i = 0; i < pos; i++)
        temp = temp.nextVertex;                // move pos times

    return temp.vertexInfo;
}
```

**Example:**
```
getVertex(0) → "Alor Setar"
getVertex(1) → "Kuching"
getVertex(5) → "Tawau"
getVertex(6) → null  (only 6 vertices, index 5 is the last)
getVertex(-1) → null (invalid)
```

---

### 9.8 — hasEdge(): Check if an Edge Exists

This method uses a **double while loop** — outer loop finds source vertex, inner loop searches its edges for the destination.

```java
public boolean hasEdge(T source, T destination) {
    if (head == null)
        return false;                          // empty graph → no edges

    if (!hasVertex(source) || !hasVertex(destination))
        return false;                          // one of the vertices doesn't exist

    Vertex<T, N> sourceVertex = head;
    while (sourceVertex != null) {

        if (sourceVertex.vertexInfo.compareTo(source) == 0) {
            // FOUND source vertex — now search its edge list for destination

            Edge<T, N> currentEdge = sourceVertex.firstEdge;
            while (currentEdge != null) {
                if (currentEdge.toVertex.vertexInfo.compareTo(destination) == 0)
                    return true;               // edge found!
                currentEdge = currentEdge.nextEdge;
            }
        }

        sourceVertex = sourceVertex.nextVertex;
    }
    return false;                              // edge not found
}
```

**Trace example:** `hasEdge("Kuching", "Melaka")`
```
sourceVertex = Alor Setar
  → "Alor Setar" ≠ "Kuching" → move to next vertex
sourceVertex = Kuching
  → "Kuching" == "Kuching" → FOUND source vertex!
  currentEdge = Kuching.firstEdge → [Melaka, 800, →]
    → Melaka.compareTo("Melaka") == 0 → return TRUE ✅
```

`hasEdge("Melaka", "Kuching")` → returns **false** because no directed edge from Melaka to Kuching exists in this directed graph.

---

### 9.9 — addEdge(): Add a New Directed Weighted Edge

This is the most complex method. Key trick: **when a new edge is added, it becomes the FIRST edge** of the source vertex, and the old first edge becomes the new edge's `nextEdge`.

```java
public boolean addEdge(T source, T destination, N w) {
    if (head == null)
        return false;

    if (!hasVertex(source) || !hasVertex(destination))
        return false;

    Vertex<T, N> sourceVertex = head;
    while (sourceVertex != null) {

        if (sourceVertex.vertexInfo.compareTo(source) == 0) {
            // Found source vertex. Now find destination vertex.

            Vertex<T, N> destinationVertex = head;
            while (destinationVertex != null) {

                if (destinationVertex.vertexInfo.compareTo(destination) == 0) {
                    // Found destination vertex. Add the edge here.

                    // Step 1: currentEdge = whatever firstEdge was before
                    Edge<T, N> currentEdge = sourceVertex.firstEdge;

                    // Step 2: create new edge pointing TO destinationVertex,
                    //         with weight w, and next = the OLD first edge
                    Edge<T, N> newEdge = new Edge<>(destinationVertex, w, currentEdge);

                    // Step 3: make source vertex point to the NEW edge
                    sourceVertex.firstEdge = newEdge;

                    // Step 4: update degrees
                    sourceVertex.outdeg++;
                    destinationVertex.indeg++;

                    return true;
                }
                destinationVertex = destinationVertex.nextVertex;
            }
        }
        sourceVertex = sourceVertex.nextVertex;
    }
    return false;
}
```

**Visual of what happens when `addEdge("Kuching", "Tawau", 900)` is called AFTER `addEdge("Kuching", "Melaka", 800)`:**

```
BEFORE (after adding Kuching→Melaka):
Kuching.firstEdge → [Melaka | 800 | null]

AFTER (adding Kuching→Tawau):
currentEdge = [Melaka | 800 | null]   ← save the old firstEdge
newEdge = [Tawau | 900 | → Melaka]    ← new edge points to old firstEdge
Kuching.firstEdge = newEdge

Result:
Kuching.firstEdge → [Tawau | 900 | →] → [Melaka | 800 | null]
```

> ⚠️ This means **newer edges appear first** in the edge list! The test program output shows `[Kuching, Tawau] [Kuching, Melaka]` because Tawau was added after Melaka.

---

### 9.10 — getEdgeWeight(): Get the Weight of an Edge

Very similar to `hasEdge()`, but instead of returning true, it returns the weight.

```java
public N getEdgeWeight(T source, T destination) {
    N notFound = null;

    if (head == null)
        return notFound;

    if (!hasVertex(source) || !hasVertex(destination))
        return notFound;

    Vertex<T, N> sourceVertex = head;
    while (sourceVertex != null) {
        if (sourceVertex.vertexInfo.compareTo(source) == 0) {
            Edge<T, N> currentEdge = sourceVertex.firstEdge;
            while (currentEdge != null) {
                if (currentEdge.toVertex.vertexInfo.compareTo(destination) == 0)
                    return currentEdge.weight;   // found! return weight
                currentEdge = currentEdge.nextEdge;
            }
        }
        sourceVertex = sourceVertex.nextVertex;
    }
    return notFound;                             // edge not found → return null
}
```

---

### 9.11 — getNeighbours(): Get All Neighbours of a Vertex

Returns an `ArrayList<T>` of all vertices that the given vertex has edges pointing to.

```java
public ArrayList<T> getNeighbours(T v) {
    if (!hasVertex(v))
        return null;

    ArrayList<T> list = new ArrayList<>();
    Vertex<T, N> temp = head;

    while (temp != null) {
        if (temp.vertexInfo.compareTo(v) == 0) {
            // Found the vertex — now collect all its neighbours
            Edge<T, N> currentEdge = temp.firstEdge;
            while (currentEdge != null) {
                list.add(currentEdge.toVertex.vertexInfo);
                currentEdge = currentEdge.nextEdge;
            }
        }
        temp = temp.nextVertex;
    }
    return list;
}
```

**Example:** `getNeighbours("Kuching")` → `["Tawau", "Melaka"]`

> Note: Tawau appears before Melaka because Tawau was added AFTER Melaka (newer edges are at the front).

---

### 9.12 — printEdges(): Print the Entire Graph

```java
public void printEdges() {
    Vertex<T, N> temp = head;
    while (temp != null) {
        System.out.print("# " + temp.vertexInfo + " : ");

        Edge<T, N> currentEdge = temp.firstEdge;
        while (currentEdge != null) {
            System.out.print("[" + temp.vertexInfo + ","
                    + currentEdge.toVertex.vertexInfo + "] ");
            currentEdge = currentEdge.nextEdge;
        }

        System.out.println();
        temp = temp.nextVertex;
    }
}
```

**Example output:**
```
# Alor Setar : [Alor Setar,Kuching]
# Kuching : [Kuching,Tawau] [Kuching,Melaka]
# Langkawi : [Langkawi,Penang]
# Melaka : [Melaka,Penang]
# Penang :
# Tawau : [Tawau,Alor Setar]
```

---

### 9.13 — Complete Test Program Output

After running the full test program from the lecture:

```
The number of vertices in graph1: 6
Cities and their vertices
0: Alor Setar    1: Kuching    2: Langkawi    3: Melaka    4: Penang    5: Tawau

Is Melaka in the Graph? true
Is Ipoh in the Graph? false

Kuching at index: 1
Ipoh at index: -1

add edge Kuching - Melaka: true
add edge Langkawi - Penang: true
add edge Melaka - Penang: true
add edge Alor Setar - Kuching: true
add edge Tawau - Alor Setar: true
add edge Kuching - Tawau: true
add edge Langkawi - Ipoh: false       ← false because Ipoh is not in the graph!

has edge from Kuching to Melaka? true
has edge from Melaka to Langkawi? false   ← directed! no edge from Melaka to Kuching
has edge from Ipoh to Langkawi? false     ← Ipoh doesn't exist

weight of edge from Kuching to Melaka? 800
weight of edge from Tawau to Alor Setar? 1900
weight of edge from Semporna to Ipoh? null    ← neither vertex exists

In and out degree for Kuching is 1 and 2     ← 1 in (from Alor Setar), 2 out (Melaka, Tawau)
In and out degree for Penang is 2 and 0      ← 2 in (from Langkawi, Melaka), 0 out
In and out degree for Ipoh is -1 and -1      ← not in graph → -1

Neighbours of Kuching: [Tawau, Melaka]       ← Tawau listed first (added last)

Print Edges:
# Alor Setar : [Alor Setar,Kuching]
# Kuching : [Kuching,Tawau] [Kuching,Melaka]
# Langkawi : [Langkawi,Penang]
# Melaka : [Melaka,Penang]
# Penang :
# Tawau : [Tawau,Alor Setar]
```

---

## 10. Graph Traversal — Depth-First Search (DFS)

### What is Traversal?
Graph traversal = visiting (checking/updating) **every vertex** in the graph exactly once.

There are two main strategies: DFS and BFS.

### DFS — The Concept

> "Go as DEEP as possible before coming back." Like exploring a maze — you pick a direction and keep going until you hit a dead end, then you backtrack and try another path.

**Data structure used:** **Stack** (LIFO — Last In First Out)

### DFS Algorithm
```
1. Pick a starting vertex. Push it onto the Stack.
2. Pop the top item from the Stack → add it to the Visited list.
3. Get all its unvisited adjacent neighbours → push them onto the Stack.
4. Repeat steps 2-3 until the Stack is empty.
```

### DFS Trace Example

Using the 6-vertex graph:
```
        6
        |
    4 - 5 - 1
    |   |  /
    3 - 2
```

Edges: 1-2, 1-5, 2-3, 2-5, 3-4, 4-5, 4-6

**Start from vertex 1:**

```
Step 1: Stack = [1],       Visited = []
Step 2: Pop 1 → Visited = [1]
        Neighbours of 1: {2, 5} → push unvisited ones
        Stack = [2, 5]

Step 3: Pop 5 → Visited = [1, 5]
        Neighbours of 5: {1, 2, 4} → unvisited = {2, 4}
        Stack = [2, 2, 4]   ← 2 is pushed again (we'll skip duplicates later)

Step 4: Pop 4 → Visited = [1, 5, 4]
        Neighbours of 4: {3, 5, 6} → unvisited = {3, 6}
        Stack = [2, 2, 3, 6]

Step 5: Pop 6 → Visited = [1, 5, 4, 6]
        Neighbours of 6: {4} → already visited
        Stack = [2, 2, 3]

Step 6: Pop 3 → Visited = [1, 5, 4, 6, 3]
        Neighbours of 3: {2, 4} → unvisited = {2}
        Stack = [2, 2, 2]

Step 7: Pop 2 → Visited = [1, 5, 4, 6, 3, 2]
        Neighbours of 2: {1, 3, 5} → all visited
        Stack = [2, 2]

Step 8: Pop 2 → already visited, skip
Step 9: Pop 2 → already visited, skip
Stack empty → DONE!

DFS order starting from 1: 1, 5, 4, 6, 3, 2
```

> The exact order can vary depending on which neighbour gets pushed first. DFS goes deep — notice it went from 1 → 5 → 4 → 6 (very deep) before coming back.

### Applications of DFS
1. **Detect if graph is connected**: Start DFS from any vertex. If # vertices visited == total vertices → graph is connected.
2. **Detect if a path exists** between two vertices.
3. **Find a path** between two vertices.
4. **Detect cycles** in a graph.

---

## 11. Graph Traversal — Breadth-First Search (BFS)

### BFS — The Concept

> "Visit all NEIGHBOURS first before going deeper." Like ripples in water — first the immediate neighbours, then their neighbours, then their neighbours' neighbours...

**Data structure used:** **Queue** (FIFO — First In First Out)

### BFS Algorithm
```
1. Pick a starting vertex. Add it to the back of the Queue.
2. Dequeue the front item → add it to the Visited list.
3. Get all its unvisited adjacent neighbours → add them to the back of the Queue.
4. Repeat steps 2-3 until the Queue is empty.
```

### BFS Trace Example

Using the same graph, **starting from vertex 1:**

```
        6
        |
    4 - 5 - 1
    |   |  /
    3 - 2
```

```
Step 1: Queue = [1],       Visited = []
Step 2: Dequeue 1 → Visited = [1]
        Neighbours of 1: {2, 5} → enqueue unvisited
        Queue = [2, 5]

Step 3: Dequeue 2 → Visited = [1, 2]
        Neighbours of 2: {1, 3, 5} → unvisited = {3, 5}
        Queue = [5, 3, 5]

Step 4: Dequeue 5 → Visited = [1, 2, 5]
        Neighbours of 5: {1, 2, 4} → unvisited = {4}
        Queue = [3, 5, 4]

Step 5: Dequeue 3 → Visited = [1, 2, 5, 3]
        Neighbours of 3: {2, 4} → unvisited = {4}
        Queue = [5, 4, 4]

Step 6: Dequeue 5 → already visited, skip
Step 7: Dequeue 4 → Visited = [1, 2, 5, 3, 4]
        Neighbours of 4: {3, 5, 6} → unvisited = {6}
        Queue = [4, 6]

Step 8: Dequeue 4 → already visited, skip
Step 9: Dequeue 6 → Visited = [1, 2, 5, 3, 4, 6]
        Neighbours of 6: {4} → all visited
        Queue = []

Queue empty → DONE!

BFS order starting from 1: 1, 2, 5, 3, 4, 6
```

> BFS visits level by level — first vertex 1, then its immediate neighbours (2, 5), then their neighbours (3, 4), then deeper (6).

### Applications of BFS
1. **Find shortest path (fewest edges)** between two vertices — BFS guarantees minimum edge count (not minimum weight!).
2. **Check if graph is bipartite** (can be divided into 2 groups with no edges within a group).
3. **Level-order processing** of a graph.

> ⚠️ BFS finds the path with the **fewest edges**, not the **shortest distance** in a weighted graph! For shortest distance by weight, you'd use Dijkstra's algorithm.

---

## 12. DFS vs BFS Comparison

| Feature | DFS | BFS |
|---------|-----|-----|
| **Data Structure** | Stack (LIFO) | Queue (FIFO) |
| **Strategy** | Go deep first | Go wide first (level by level) |
| **Memory** | Less memory (only stores path) | More memory (stores entire frontier) |
| **Path found** | Not necessarily shortest | **Shortest by edge count** |
| **Use: connectivity** | ✅ Yes | ✅ Yes |
| **Use: find path** | ✅ Yes | ✅ Yes (shortest) |
| **Use: detect cycle** | ✅ Yes | Less common |
| **Use: bipartite check** | Possible | ✅ Easier |
| **Analogy** | Exploring a cave with one torch (go deep) | Sending scouts in all directions simultaneously |

### Visual Summary

```
Graph:   1 - 2 - 3
         |       |
         4 - 5 - 6

DFS from 1:  1 → 2 → 3 → 6 → 5 → 4  (goes deep: 1→2→3→6→5→4)
BFS from 1:  1 → 2 → 4 → 3 → 5 → 6  (goes wide: level 1 = {2,4}, level 2 = {3,5}, level 3 = {6})
```

---

## 13. Tutorial Answers (T08)

### Tutorial Question 1: Write adjacency matrix and list for the graph

The tutorial graph (directed):
```
Vertices: A, B, C, D, E, F, G, H, I
Directed Edges:
  A → C
  A → D  (via B going to D)
  B → D
  C → F
  C → E
  D → E
  F → H
  G → E
  G → H  (via I somehow, check the diagram)
  H → I
  I → H
```

**Based on the tutorial diagram (directed graph with vertices A–I):**

Edges visible: A→C, A→D(?), B→D, C→F, C→E, D→E, F→H, G→E, H→I, I→H

#### Adjacency Matrix (9×9 for vertices A,B,C,D,E,F,G,H,I):

```
     A  B  C  D  E  F  G  H  I
A  [ 0  0  1  0  0  0  0  0  0 ]
B  [ 0  0  0  1  0  0  0  0  0 ]
C  [ 0  0  0  0  1  1  0  0  0 ]
D  [ 0  0  0  0  1  0  0  0  0 ]
E  [ 0  0  0  0  0  0  0  0  0 ]
F  [ 0  0  0  0  0  0  0  1  0 ]
G  [ 0  0  0  0  1  0  0  0  0 ]
H  [ 0  0  0  0  0  0  0  0  1 ]
I  [ 0  0  0  0  0  0  0  1  0 ]
```

#### Adjacency List:

```
A → [C]
B → [D]
C → [F, E]
D → [E]
E → []  (no outgoing edges)
F → [H]
G → [E]
H → [I]
I → [H]
```

### Tutorial Question 2: Represent using 2D array

For a 2D array representation, use the **adjacency matrix** (Question 2 answer: use adjacency matrix).

```java
// Vertex index mapping: A=0, B=1, C=2, D=3, E=4, F=5, G=6, H=7, I=8
int[][] adjMatrix = {
    { 0, 0, 1, 0, 0, 0, 0, 0, 0 },  // A
    { 0, 0, 0, 1, 0, 0, 0, 0, 0 },  // B
    { 0, 0, 0, 0, 1, 1, 0, 0, 0 },  // C
    { 0, 0, 0, 0, 1, 0, 0, 0, 0 },  // D
    { 0, 0, 0, 0, 0, 0, 0, 0, 0 },  // E
    { 0, 0, 0, 0, 0, 0, 0, 1, 0 },  // F
    { 0, 0, 0, 0, 1, 0, 0, 0, 0 },  // G
    { 0, 0, 0, 0, 0, 0, 0, 0, 1 },  // H
    { 0, 0, 0, 0, 0, 0, 0, 1, 0 }   // I
};
```

### Tutorial Question 3: Write code using linked-list representation

For linked-list representation, use the **adjacency list** approach (Question 3 answer: use adjacency list).

```java
// Using the WeightedGraph framework from lecture
// For unweighted graph, we can use Integer as the weight type with value 1

WeightedGraph<String, Integer> tutorialGraph = new WeightedGraph<>();

// Add all vertices
String[] vertices = {"A", "B", "C", "D", "E", "F", "G", "H", "I"};
for (String v : vertices) {
    tutorialGraph.addVertex(v);
}

// Add all directed edges (weight = 1 for unweighted)
tutorialGraph.addEdge("A", "C", 1);
tutorialGraph.addEdge("B", "D", 1);
tutorialGraph.addEdge("C", "F", 1);
tutorialGraph.addEdge("C", "E", 1);
tutorialGraph.addEdge("D", "E", 1);
tutorialGraph.addEdge("F", "H", 1);
tutorialGraph.addEdge("G", "E", 1);
tutorialGraph.addEdge("H", "I", 1);
tutorialGraph.addEdge("I", "H", 1);

// Print to verify
tutorialGraph.printEdges();
```

---

## 14. Lab Answers (L8)

### Lab Question 1: addUndirectedEdge()

An undirected edge between A and B = two directed edges: A→B and B→A.

```java
public boolean addUndirectedEdge(T source, T destination, N w) {
    // Add edge in BOTH directions
    boolean forward  = addEdge(source, destination, w);
    boolean backward = addEdge(destination, source, w);
    return forward && backward;
}
```

> Simple! An undirected edge is just two directed edges. Call `addEdge` twice — once each way. Both must succeed for the method to return true.

---

### Lab Question 2: removeEdge()

```java
public boolean removeEdge(T source, T destination) {
    if (head == null)
        return false;

    if (!hasVertex(source) || !hasVertex(destination))
        return false;

    Vertex<T, N> sourceVertex = head;

    while (sourceVertex != null) {
        if (sourceVertex.vertexInfo.compareTo(source) == 0) {
            // Found source vertex — now find and remove the edge

            Edge<T, N> currentEdge  = sourceVertex.firstEdge;
            Edge<T, N> previousEdge = null;

            while (currentEdge != null) {
                if (currentEdge.toVertex.vertexInfo.compareTo(destination) == 0) {
                    // Found the edge to remove!

                    if (previousEdge == null) {
                        // Edge to remove is the FIRST edge of this vertex
                        sourceVertex.firstEdge = currentEdge.nextEdge;
                    } else {
                        // Edge is in the middle or end of the edge list
                        previousEdge.nextEdge = currentEdge.nextEdge;
                    }

                    sourceVertex.outdeg--;

                    // Find destination vertex and decrement its in-degree
                    Vertex<T, N> destVertex = head;
                    while (destVertex != null) {
                        if (destVertex.vertexInfo.compareTo(destination) == 0) {
                            destVertex.indeg--;
                            break;
                        }
                        destVertex = destVertex.nextVertex;
                    }

                    return true;
                }

                previousEdge = currentEdge;
                currentEdge  = currentEdge.nextEdge;
            }
        }
        sourceVertex = sourceVertex.nextVertex;
    }
    return false;   // edge not found
}
```

**How it works:**
```
Before: Kuching.firstEdge → [Tawau,900] → [Melaka,800] → null

removeEdge("Kuching", "Tawau"):
  currentEdge = [Tawau,900], previousEdge = null
  Found it! previousEdge is null → sourceVertex.firstEdge = [Melaka,800]

After: Kuching.firstEdge → [Melaka,800] → null

removeEdge("Kuching", "Melaka"):
  currentEdge = [Melaka,800], previousEdge = null
  Found it! previousEdge is null → sourceVertex.firstEdge = null

After: Kuching.firstEdge → null
```

---

### Lab Question 3: Graph.java (Unweighted Graph)

For an unweighted graph, we don't need the weight in our Edge nodes. We can either:
- Use Integer as the weight type (with dummy value 1)
- Or create a new simplified class

Here's the simplified approach using the same framework (just no weight field exposed):

```java
class UnweightedGraph<T extends Comparable<T>> {

    // Simplified inner Vertex class (no N type parameter needed)
    class Vertex<T> {
        T vertexInfo;
        int indeg, outdeg;
        Vertex<T> nextVertex;
        Edge<T> firstEdge;

        public Vertex(T info) {
            this.vertexInfo  = info;
            this.indeg       = 0;
            this.outdeg      = 0;
            this.nextVertex  = null;
            this.firstEdge   = null;
        }
    }

    // Simplified Edge class (no weight field)
    class Edge<T> {
        Vertex<T> toVertex;
        Edge<T> nextEdge;

        public Edge(Vertex<T> dest, Edge<T> next) {
            this.toVertex  = dest;
            this.nextEdge  = next;
        }
    }

    private Vertex<T> head;
    private int size;

    public UnweightedGraph() {
        head = null;
        size = 0;
    }

    // addEdge — directed, no weight
    public boolean addEdge(T source, T destination) {
        if (head == null) return false;
        if (!hasVertex(source) || !hasVertex(destination)) return false;

        Vertex<T> sourceVertex = head;
        while (sourceVertex != null) {
            if (sourceVertex.vertexInfo.compareTo(source) == 0) {
                Vertex<T> destVertex = head;
                while (destVertex != null) {
                    if (destVertex.vertexInfo.compareTo(destination) == 0) {
                        Edge<T> currentEdge = sourceVertex.firstEdge;
                        Edge<T> newEdge     = new Edge<>(destVertex, currentEdge);
                        sourceVertex.firstEdge = newEdge;
                        sourceVertex.outdeg++;
                        destVertex.indeg++;
                        return true;
                    }
                    destVertex = destVertex.nextVertex;
                }
            }
            sourceVertex = sourceVertex.nextVertex;
        }
        return false;
    }

    // addUndirectedEdge — add both directions
    public boolean addUndirectedEdge(T source, T destination) {
        boolean a = addEdge(source, destination);
        boolean b = addEdge(destination, source);
        return a && b;
    }

    // hasVertex
    public boolean hasVertex(T v) {
        Vertex<T> temp = head;
        while (temp != null) {
            if (temp.vertexInfo.compareTo(v) == 0)
                return true;
            temp = temp.nextVertex;
        }
        return false;
    }

    // addVertex
    public boolean addVertex(T v) {
        if (!hasVertex(v)) {
            Vertex<T> newV = new Vertex<>(v);
            if (head == null) {
                head = newV;
            } else {
                Vertex<T> temp = head;
                while (temp.nextVertex != null)
                    temp = temp.nextVertex;
                temp.nextVertex = newV;
            }
            size++;
            return true;
        }
        return false;
    }
}
```

---

### Lab Question 4: Modify TestWeightedGraph to Test Graph Class

```java
public class TestGraph {
    public static void main(String[] args) {

        // Create unweighted graph using tutorial graph (A–I)
        UnweightedGraph<String> graph = new UnweightedGraph<>();

        // Add vertices
        String[] vertices = {"A", "B", "C", "D", "E", "F", "G", "H", "I"};
        for (String v : vertices) {
            graph.addVertex(v);
        }

        System.out.println("Testing addEdge (directed):");
        System.out.println("A → C: " + graph.addEdge("A", "C"));
        System.out.println("B → D: " + graph.addEdge("B", "D"));
        System.out.println("C → E: " + graph.addEdge("C", "E"));
        System.out.println("C → F: " + graph.addEdge("C", "F"));

        System.out.println("\nTesting addUndirectedEdge:");
        System.out.println("H ↔ I: " + graph.addUndirectedEdge("H", "I"));

        System.out.println("\nTesting hasVertex:");
        System.out.println("Has A? " + graph.hasVertex("A"));   // true
        System.out.println("Has Z? " + graph.hasVertex("Z"));   // false
    }
}
```

---

## 15. Common Exam Mistakes & Tips

### ❌ Common Mistakes

**Mistake 1 — Confusing nextVertex with adjacency:**
- `nextVertex` links vertices in the **vertex list** — it's NOT about adjacency!
- `firstEdge` is what links a vertex to its adjacent neighbours
- Many students think "nextVertex goes to adjacent vertex" — WRONG!

**Mistake 2 — Thinking the matrix is always symmetric:**
- Undirected graph matrix IS symmetric
- Directed graph matrix is usually NOT symmetric
- Always check if the graph is directed or undirected first

**Mistake 3 — Confusing DFS and BFS data structures:**
- DFS uses **Stack** (pop from top, go deep)
- BFS uses **Queue** (dequeue from front, go wide)

**Mistake 4 — addEdge inserts at FRONT not BACK:**
- New edges are inserted at the FRONT of the edge list
- `newEdge.nextEdge = currentFirstEdge` then `source.firstEdge = newEdge`
- This means LATEST added edge appears FIRST when printing

**Mistake 5 — getIndeg returns -1 when vertex not found:**
- -1 means vertex doesn't exist, NOT that degree is 0
- A vertex with indegree 0 actually EXISTS but has no incoming edges

**Mistake 6 — addEdge returns false for non-existent vertices:**
- BOTH source AND destination must exist before you can add an edge
- In test program: `addEdge("Langkawi", "Ipoh")` returns false because "Ipoh" was never added as a vertex

### ✅ Exam Tips

1. **Know all three classes:** Vertex, Edge, Graph — and what fields each has
2. **Trace addEdge carefully:** new edge goes to the FRONT, not the back
3. **Matrix vs List:** 2D array for matrix (Question 2), linked list for adjacency list (Question 3)
4. **DFS Stack BFS Queue** — remember this!
5. **Directed edges:** `hasEdge(A,B)` and `hasEdge(B,A)` can give different results
6. **BFS finds shortest by EDGE COUNT** not by weight
7. **getIndex() returns -1** for non-existent vertex (same as getIndeg/getOutdeg)

---

## 16. Practice Questions with Full Explanations

---

### 🟢 Question 1

**What are the two components of a graph G=(V,E)?**

**A.** Vertices and Entries  
**B.** Vectors and Edges  
**C.** Vertices and Edges ✅  
**D.** Variables and Elements  

**Explanations:**
- **A** — Wrong. "Entries" is not a graph term.
- **B** — Wrong. "Vectors" is a different data structure. In graph theory, the points are called vertices.
- **C** — **CORRECT.** G=(V,E): V is the set of **V**ertices (nodes), E is the set of **E**dges (connections between vertices).
- **D** — Wrong. Neither of these are graph terms.

---

### 🟢 Question 2

**Two vertices are considered "adjacent" if:**

**A.** They are at the same level in the graph  
**B.** They share the same weight  
**C.** They are connected by the same edge ✅  
**D.** They are both connected to the same third vertex  

**Explanations:**
- **A** — Wrong. Level is a tree concept, not directly a graph adjacency concept.
- **B** — Wrong. Weight is a property of an edge, not a criterion for adjacency.
- **C** — **CORRECT.** Two vertices are adjacent if there is a direct edge connecting them (they "share" that edge).
- **D** — Wrong. This describes having a common neighbour, which does not make them adjacent to each other.

---

### 🟡 Question 3

**For a directed weighted graph with 6 vertices, which is TRUE about the adjacency matrix?**

**A.** The matrix is always symmetric  
**B.** The matrix has 36 cells, and `matrix[i][j]` stores the weight of the edge from i to j ✅  
**C.** The matrix stores only 0 and 1 values  
**D.** The matrix size is 6×1  

**Explanations:**
- **A** — Wrong. A directed graph's adjacency matrix is NOT necessarily symmetric. `matrix[i][j]` = weight of i→j, but `matrix[j][i]` could be 0 (no edge from j→i). Only undirected graphs have symmetric matrices.
- **B** — **CORRECT.** An n=6 vertex graph has an n×n = 6×6 = 36 cell matrix. For a weighted graph, cells store edge weights instead of just 0/1.
- **C** — Wrong. For UNWEIGHTED graphs, cells are 0 or 1. But for WEIGHTED graphs, cells store the actual weight value (like 800, 1200, 1900, etc.).
- **D** — Wrong. An adjacency matrix is n×n (square), not n×1. 6×1 would just be a list of 6 items.

---

### 🟡 Question 4

**What does `addEdge("Langkawi", "Ipoh", 200)` return if "Ipoh" has never been added as a vertex?**

**A.** `true` — the edge is added, and "Ipoh" is auto-created  
**B.** `false` — both vertices must already exist ✅  
**C.** `null` — the method would throw a NullPointerException  
**D.** `-1` — the vertex is not found  

**Explanations:**
- **A** — Wrong. The `addEdge` method does NOT auto-create vertices. It checks `!hasVertex(source) || !hasVertex(destination)` and returns false immediately if either vertex is missing.
- **B** — **CORRECT.** `addEdge` requires BOTH source and destination vertices to already exist in the graph. If either is missing, it returns false without doing anything.
- **C** — Wrong. The method has proper null/existence checks. No NullPointerException is thrown.
- **D** — Wrong. -1 is the return value of `getIndex()` and `getIndeg()`/`getOutdeg()` when a vertex is not found, not `addEdge()`.

---

### 🟡 Question 5

**After these operations on an empty graph:**
```java
graph.addVertex("A");
graph.addVertex("B");
graph.addVertex("C");
graph.addEdge("A", "B", 10);
graph.addEdge("A", "C", 20);
```
**What does `graph.getNeighbours("A")` return?**

**A.** `["B", "C"]` (insertion order)  
**B.** `["C", "B"]` (reverse insertion — newest first) ✅  
**C.** `["A", "B", "C"]`  
**D.** `null`  

**Explanations:**
- **A** — Wrong. B was added first, C was added second. BUT because new edges are inserted at the FRONT of the edge list, C (added later) appears first.
- **B** — **CORRECT.** When `addEdge("A","B",10)` runs: A.firstEdge → [B|10|null]. When `addEdge("A","C",20)` runs: newEdge=[C|20|→B], A.firstEdge=newEdge. So edge list: C → B. `getNeighbours` reads this order: `["C", "B"]`.
- **C** — Wrong. "A" is never a neighbour of itself (no self-loop added). getNeighbours returns the vertices that A has edges pointing TO.
- **D** — Wrong. "A" exists in the graph, so the method returns an ArrayList (not null).

---

### 🟡 Question 6

**Which data structure does DFS use, and which does BFS use?**

**A.** DFS uses Queue; BFS uses Stack  
**B.** DFS uses Stack; BFS uses Queue ✅  
**C.** Both use Stack  
**D.** Both use Queue  

**Explanations:**
- **A** — Wrong. This has them swapped. DFS goes deep (Stack/LIFO pushes and pops from the same end). BFS goes wide (Queue/FIFO processes in order).
- **B** — **CORRECT.** DFS uses a **Stack** (LIFO): push unvisited neighbours on top; pop from top to go deeper. BFS uses a **Queue** (FIFO): enqueue neighbours at the back; dequeue from front to process level by level.
- **C** — Wrong. Only DFS uses a Stack.
- **D** — Wrong. Only BFS uses a Queue.

---

### 🔴 Question 7

**In this directed graph: A→B→C→D, B→D. Perform BFS from A. What is the visit order?**

**A.** A, B, C, D  
**B.** A, B, D, C ✅  
**C.** A, B, C  
**D.** A, D, C, B  

**Explanations:**
- **A** — Wrong. This looks like it could be DFS order (going deep A→B→C→D). BFS processes level by level.
- **B** — **CORRECT.** BFS from A:
  - Queue=[A], Visited=[]
  - Dequeue A → Visited=[A]. Neighbours of A: {B} → Queue=[B]
  - Dequeue B → Visited=[A,B]. Neighbours of B: {C,D} → Queue=[C,D]  
  - Dequeue C → Visited=[A,B,C]. Neighbours of C: {D} → D already in queue → Queue=[D]
  - Dequeue D → Visited=[A,B,C,D]. Neighbours of D: {} → Queue=[]
  - Result: A, B, C, D... wait, let me re-trace. B→C and B→D, so both C and D get enqueued. C dequeued first → A,B,C then D → A,B,C,D. If B's edge list has D before C (due to insertion order of D being added after C), then Queue=[D,C] and order = A,B,D,C. The answer depends on edge list order!
- **C** — Wrong. D is reachable via B→D so it must be visited.
- **D** — Wrong. A is the starting vertex; BFS starts with A and processes its neighbours before going deeper.

> Key insight: BFS order depends on the ORDER edges are stored in the adjacency list!

---

### 🔴 Question 8

**What is the in-degree and out-degree of vertex Penang, given:**
`addEdge("Langkawi","Penang",100)`, `addEdge("Melaka","Penang",400)`, Penang has no outgoing edges.

**A.** indeg=0, outdeg=2  
**B.** indeg=2, outdeg=0 ✅  
**C.** indeg=1, outdeg=1  
**D.** indeg=-1, outdeg=-1  

**Explanations:**
- **A** — Wrong. These are swapped. 2 edges point INTO Penang, 0 edges go OUT.
- **B** — **CORRECT.** indeg = number of edges pointing INTO Penang = 2 (from Langkawi and from Melaka). outdeg = number of edges going OUT from Penang = 0 (no addEdge with "Penang" as source was called).
- **C** — Wrong. Two cities fly to Penang; Penang flies to nobody.
- **D** — Wrong. -1 would only be returned if "Penang" doesn't exist in the graph. Since Penang was added as a vertex, its degrees are valid numbers (0 and 2).

---

### 🔴 Question 9

**Which statement is FALSE about the adjacency matrix vs adjacency list comparison?**

**A.** Adjacency matrix uses O(n²) space regardless of the number of edges  
**B.** Adjacency list is preferred for sparse graphs  
**C.** Checking if edge (u,v) exists is O(1) in adjacency matrix and O(degree) in adjacency list  
**D.** Adjacency matrix is preferred over adjacency list in ALL cases ✅  

**Explanations:**
- **A** — TRUE (not the answer). An n-vertex adjacency matrix always has n² cells regardless of how many edges actually exist.
- **B** — TRUE (not the answer). For sparse graphs (few edges), adjacency list only stores actual edges, saving significant space.
- **C** — TRUE (not the answer). Matrix: `matrix[u][v]` is O(1) direct lookup. List: must traverse the edge list for u to find v, which takes O(degree(u)) time.
- **D** — **FALSE — this IS the answer.** The lecture explicitly states: adjacency matrix is good for DENSE graphs; for SPARSE graphs, linked-list/adjacency list is preferred as matrix wastes space. Neither is universally better in ALL cases.

---

### 🔴 Question 10 (Tracing)

**After these operations, what does `hasEdge("A","C")` return?**

```java
graph.addVertex("A");
graph.addVertex("B");
graph.addVertex("C");
graph.addEdge("A", "B", 5);
graph.addEdge("A", "C", 10);
graph.addEdge("B", "C", 3);
```

**A.** `false` — because C is only 2 hops from A  
**B.** `true` ✅  
**C.** Returns 10 (the weight)  
**D.** Throws an exception  

**Explanations:**
- **A** — Wrong. `hasEdge` checks for a DIRECT edge (adjacency), not whether a path exists. A→C IS a direct edge (added explicitly), so it returns true.
- **B** — **CORRECT.** `addEdge("A","C",10)` created a direct edge from A to C. `hasEdge("A","C")` finds this edge and returns true.
- **C** — Wrong. `hasEdge` returns **boolean** (true/false), not the weight. Use `getEdgeWeight("A","C")` to get 10.
- **D** — Wrong. All vertices exist and the edge was added. No exception occurs.

---

## 📌 Final Summary Cheat Sheet

```
╔════════════════════════════════════════════════════════════════════╗
║                    GRAPH QUICK REFERENCE                           ║
╠═══════════════════════╦════════════════════════════════════════════╣
║  TERM                 ║  MEANING                                    ║
╠═══════════════════════╬════════════════════════════════════════════╣
║  G = (V, E)           ║  Graph = set of Vertices + set of Edges    ║
║  Adjacent             ║  Directly connected by one edge            ║
║  Path                 ║  Route via 1 or more edges from p to q     ║
║  In-degree            ║  # edges pointing INTO a vertex            ║
║  Out-degree           ║  # edges going OUT of a vertex             ║
║  Directed graph       ║  Edges have direction (one-way)            ║
║  Undirected graph     ║  Edges are bidirectional                   ║
║  Weighted graph       ║  Edges carry a numeric value               ║
╠═══════════════════════╩════════════════════════════════════════════╣
║  REPRESENTATION       KEY DIFFERENCE                                ║
╠════════════════════════════════════════════════════════════════════╣
║  Adjacency Matrix: 2D array n×n, O(n²) space                       ║
║    → Use for DENSE graphs (many edges)                             ║
║    → edge[i][j] = weight (or 1/0 for unweighted)                  ║
║  Adjacency List: linked lists, O(V+E) space                        ║
║    → Use for SPARSE graphs (few edges)                             ║
║    → One linked list of edge nodes per vertex                      ║
╠════════════════════════════════════════════════════════════════════╣
║  IMPLEMENTATION (3 classes)                                         ║
╠════════════════════════════════════════════════════════════════════╣
║  Vertex<T,N>:  vertexInfo, indeg, outdeg, nextVertex, firstEdge    ║
║  Edge<T,N>:    toVertex, weight, nextEdge                          ║
║  Graph<T,N>:   head (first vertex), size                           ║
║                                                                     ║
║  ⚠ nextVertex ≠ adjacent vertex (it's the next in vertex list!)   ║
║  ⚠ New edges insert at FRONT of edge list (newest = first)        ║
║  ⚠ addEdge returns false if EITHER vertex doesn't exist           ║
║  ⚠ getIndeg/getOutdeg returns -1 if vertex not found             ║
╠════════════════════════════════════════════════════════════════════╣
║  TRAVERSAL                                                          ║
╠════════════════════════════════════════════════════════════════════╣
║  DFS: uses STACK, goes DEEP first                                  ║
║    → Detect: connectivity, paths, cycles                           ║
║  BFS: uses QUEUE, goes WIDE first (level by level)                 ║
║    → Find: shortest path (by edge count, NOT weight)               ║
║    → Detect: bipartite graph, connectivity                         ║
║  Both: result in a spanning tree                                   ║
╚════════════════════════════════════════════════════════════════════╝
```

---

> 📝 **Study tip:** The best way to master Graph is to physically draw the linked-list structure on paper for addVertex/addEdge operations. Draw the Vertex list with arrows for nextVertex, and separately draw the Edge lists for each vertex with arrows for nextEdge. Trace through addEdge step by step to see the "new edge becomes firstEdge" pattern.
