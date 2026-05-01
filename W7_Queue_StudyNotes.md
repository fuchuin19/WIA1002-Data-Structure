# 📚 WIA1002 Data Structure — Queue (Complete Study Notes)
> **Course:** WIA1002 Data Structure | Universiti Malaya | Sem 2, 2025/2026  
> **Topic:** Queue — Introduction, Implementation, Priority Queue  
> **Reference:** Chapter 19 & 24, Liang — *Introduction to Java Programming*, 10th Ed.

---

## 📋 Table of Contents
1. [Quick Recap — Java Collection Framework](#1-quick-recap--java-collection-framework-hierarchy)
2. [What is a Queue?](#2-what-is-a-queue)
3. [Queue Operations (Enqueue & Dequeue)](#3-queue-operations)
4. [How a Queue Works — Step by Step](#4-how-a-queue-works--step-by-step-visual)
5. [Implementing a Queue in Java](#5-implementing-a-queue-in-java)
6. [GenericQueue — Full Code & Walkthrough](#6-genericqueue--full-code--walkthrough)
7. [Priority Queue](#7-priority-queue)
8. [Java's Built-in PriorityQueue Class](#8-javas-built-in-priorityqueue-class)
9. [PriorityQueue Code Examples](#9-priorityqueue-code-examples)
10. [Array-Based Queue (Exercise)](#10-array-based-queue-exercise)
11. [Common Mistakes & Tips](#11-common-mistakes--exam-tips)
12. [Practice Questions with Full Explanations](#12-practice-questions-with-full-explanations)

---

## 1. Quick Recap — Java Collection Framework Hierarchy

Before diving into Queue, you need to understand where Queue sits in Java's structure. Think of it like a family tree:

```
Iterable  (great-grandparent — everything that can be looped)
    └── Collection  (grandparent — holds groups of objects)
            ├── List        → ArrayList, LinkedList, Vector → Stack
            ├── Queue       → PriorityQueue, Deque → ArrayDeque
            └── Set         → HashSet, LinkedHashSet, SortedSet → TreeSet
```

### 🔑 Key Point
- **Orange boxes = Interfaces** (blueprints, rules only, no actual code)
- **Blue boxes = Classes** (actual working code you can use)
- `Queue` is an **interface** that sits under `Collection`
- `PriorityQueue` and `ArrayDeque` are **classes** that implement `Queue`

### The Collection Interface — What Every Collection Can Do

Every collection in Java (List, Queue, Set) inherits these core methods from `Collection`:

| Method | What it does |
|--------|-------------|
| `add(o)` | Adds an element |
| `remove(o)` | Removes a specific element |
| `size()` | Returns number of elements |
| `isEmpty()` | Returns true if nothing is in it |
| `contains(o)` | Checks if element exists |
| `clear()` | Removes everything |
| `toArray()` | Converts to plain array |

> 💡 **Human analogy:** The `Collection` interface is like the general rules for any "container" — you can add things, remove things, and check how many things are inside. Queue, List, and Set are all just specialised containers with extra rules on top.

---

## 2. What is a Queue?

### Real-World Analogy
Imagine you're at a KFC counter. Person A arrived first, then B, then C, then D.

```
[COUNTER/CASHIER] ←←← A ←←← B ←←← C ←←← D [NEW ARRIVALS]
     (front/head)                           (back/tail/rear)
```

- **Person A** is served **first** (they arrived first)
- **Person D** has to wait the longest (they arrived last)
- New customers join at the **back**
- Customers are served from the **front**

This is exactly how a **Queue** data structure works.

### Formal Definition

> A **Queue** is a linear data structure where:
> - Elements are **inserted** at the **back (tail/rear)**
> - Elements are **accessed and deleted** from the **front (head)**
> - It follows **FIFO** order — **First In, First Out**

```
INSERT HERE                          DELETE/ACCESS HERE
     ↓                                      ↑
  [ BACK ]  ←  [4th]  [3rd]  [2nd]  [1st]  [ FRONT ]
   (tail/rear)                              (head)
```

### FIFO — First In, First Out
The item that was added **first** will be removed **first**.

| Inserted Order | Removed Order |
|---------------|---------------|
| 1st: "Tom"    | 1st removed: "Tom" |
| 2nd: "Susan"  | 2nd removed: "Susan" |
| 3rd: "Kim"    | 3rd removed: "Kim" |
| 4th: "Michael"| 4th removed: "Michael" |

> ⚠️ **Do NOT confuse with Stack (LIFO)!**
> - **Stack** = Last In, First Out (like a pile of plates — you take from the top)
> - **Queue** = First In, First Out (like a queue at a bank — first come, first served)

---

## 3. Queue Operations

A Queue has exactly **2 core operations**:

### ① Enqueue — Adding to the Queue
- Adds an element to the **back/tail** of the queue
- Also called: `add`, `offer`, `push` (depends on context)

```
Before:  [ Tom ] [ Susan ]
                            ↑
Enqueue("Kim")    →    adds "Kim" here (at the back)

After:   [ Tom ] [ Susan ] [ Kim ]
```

### ② Dequeue — Removing from the Queue
- Removes and returns the element from the **front/head** of the queue
- Also called: `remove`, `poll`

```
Before:  [ Tom ] [ Susan ] [ Kim ]
    ↑
Dequeue()    →    removes "Tom" from here (from the front)

After:   [ Susan ] [ Kim ]
Returns: "Tom"
```

### Summary Table

| Operation | Where it happens | What it does |
|-----------|-----------------|--------------|
| `enqueue(e)` | Back / Tail | Adds element `e` |
| `dequeue()` | Front / Head | Removes & returns element |
| `getSize()` | — | Returns number of elements |
| `isEmpty()` | — | Returns true if queue is empty |

---

## 4. How a Queue Works — Step by Step Visual

Let's trace through adding and removing elements manually:

```
Start:       (empty queue)

enqueue("Tom")    →   [ Tom ]
                       front/back

enqueue("Susan")  →   [ Tom ][ Susan ]
                       front    back

enqueue("Kim")    →   [ Tom ][ Susan ][ Kim ]
                       front              back

enqueue("Michael")→   [ Tom ][ Susan ][ Kim ][ Michael ]
                       front                      back

dequeue()         →   returns "Tom"
                       [ Susan ][ Kim ][ Michael ]
                        front              back

dequeue()         →   returns "Susan"
                       [ Kim ][ Michael ]
                        front    back

Queue now:  [ Kim ][ Michael ]
```

> 🎯 **Interactive Animation:** Visit [https://yongdanielliang.github.io/animation/web/Queue.html](https://yongdanielliang.github.io/animation/web/Queue.html) to see Queue operations animated live!

---

## 5. Implementing a Queue in Java

### Why Use LinkedList (Not ArrayList)?

You have two choices to back a queue with a list:

| | LinkedList | ArrayList |
|--|-----------|-----------|
| **Deletion at front** | O(1) — just move the head pointer | O(n) — must shift ALL elements left |
| **Insertion at back** | O(1) | O(1) amortised |
| **Best for Queue?** | ✅ YES | ❌ No (slow dequeue) |

**Why is ArrayList bad for Queue?**

Imagine an ArrayList with 1000 elements. When you remove index 0 (front), Java must shift ALL 999 remaining elements one position to the left. That's 999 operations!

With LinkedList, removing from the front just means updating a pointer — 1 operation regardless of size.

```
ArrayList dequeue (slow):
[ A ][ B ][ C ][ D ][ E ]...[ Z ]
Remove A → shift everything left:
[ B ][ C ][ D ][ E ]...[ Z ][ _ ]
(999 shifts for 1000 elements!)

LinkedList dequeue (fast):
head → [A] → [B] → [C] → [D] → ...
Remove A → just move head pointer:
head → [B] → [C] → [D] → ...
(1 operation always!)
```

### Two Ways to Design a Queue Class

#### (a) Using Inheritance — GenericQueue EXTENDS LinkedList

```
LinkedList ←——— GenericQueue
(parent)         (child inherits everything from LinkedList)
```

```java
// Inheritance approach
public class GenericQueue<E> extends java.util.LinkedList<E> {
    public void enqueue(E e) {
        addLast(e);  // inherited from LinkedList
    }
    public E dequeue() {
        return removeFirst();  // inherited from LinkedList
    }
}
```

**Problem with inheritance:** GenericQueue also inherits methods that shouldn't be in a pure Queue (like `add(index, element)` — Queues shouldn't allow inserting in the middle!). This breaks the Queue concept.

#### (b) Using Composition — GenericQueue HAS-A LinkedList (Recommended ✅)

```
GenericQueue ◇——— LinkedList
(contains a LinkedList as an internal field, not a child of it)
```

```java
// Composition approach — the LinkedList is HIDDEN inside
public class GenericQueue<E> {
    private java.util.LinkedList<E> list = new java.util.LinkedList<>();
    // only exposes Queue methods — enqueue, dequeue, getSize
}
```

**Why composition is better:** You control exactly which methods are exposed. Users of GenericQueue can ONLY enqueue and dequeue — they can't accidentally insert in the middle.

> 💡 **Analogy:** 
> - **Inheritance** = A sports car that inherited everything from a truck (including the cargo bed — weird!)
> - **Composition** = A sports car that has an engine inside — it uses the engine, but you only see the steering wheel and pedals

---

## 6. GenericQueue — Full Code & Walkthrough

### The GenericQueue Class (Composition Approach)

```java
public class GenericQueue<E> {
    
    // Step 1: The hidden LinkedList — this is where data is actually stored
    private java.util.LinkedList<E> list = new java.util.LinkedList<>();
    
    // Step 2: enqueue — adds element to the BACK of the queue
    public void enqueue(E e) {
        list.addLast(e);   // addLast() adds to the tail of the LinkedList
    }
    
    // Step 3: dequeue — removes and returns element from the FRONT
    public E dequeue() {
        return list.removeFirst();  // removeFirst() removes from head of LinkedList
    }
    
    // Step 4: getSize — how many elements are in the queue?
    public int getSize() {
        return list.size();
    }
    
    // Step 5: toString — for printing the queue nicely
    @Override
    public String toString() {
        return "Queue: " + list.toString();
    }
}
```

### Line-by-Line Explanation

| Line | Code | What it means in plain English |
|------|------|-------------------------------|
| `private java.util.LinkedList<E> list` | The actual storage | Think of it as the physical "queue lane" |
| `list.addLast(e)` | Adds to the back | New person joins at the END of the queue |
| `list.removeFirst()` | Removes from front | First person in line is served and leaves |
| `list.size()` | Count elements | How many people are in the queue? |

### Testing the GenericQueue

```java
public class TestGenericQueue {
    public static void main(String[] args) {
        
        // Create a new empty queue
        GenericQueue<String> queue = new GenericQueue<>();
        
        // Add elements (enqueue)
        queue.enqueue("Tom");
        System.out.println(queue);    // Queue: [Tom]
        
        queue.enqueue("Susan");
        System.out.println(queue);    // Queue: [Tom, Susan]
        
        queue.enqueue("Kim");
        queue.enqueue("Michael");
        System.out.println(queue);    // Queue: [Tom, Susan, Kim, Michael]
        
        // Remove elements (dequeue)
        System.out.println(queue.dequeue());  // Tom    ← first in, first out
        System.out.println(queue.dequeue());  // Susan
        System.out.println(queue);            // Queue: [Kim, Michael]
    }
}
```

### Output Trace

```
(7)  Queue: [Tom]
(8)  Queue: [Tom, Susan]
(9)  Queue: [Tom, Susan, Kim, Michael]
(10) Tom
(11) Susan
(12) Queue: [Kim, Michael]
```

> 🔑 Notice: "Tom" was added first → "Tom" was removed first. Pure FIFO!

---

## 7. Priority Queue

### What's the Problem with a Regular Queue?

In a regular queue, order is purely based on **arrival time** (FIFO). But in real life, some tasks are more urgent than others.

**Example — Hospital Emergency Room:**
- Patient A arrives: sprained ankle (low urgency)
- Patient B arrives: heart attack (HIGH urgency)
- Patient C arrives: broken arm (medium urgency)

In a regular FIFO queue, Patient A is treated first (they arrived first). But that's dangerous! Patient B with the heart attack should be treated immediately regardless of arrival order.

**This is exactly what a Priority Queue solves.**

### Priority Queue Definition

> A **Priority Queue** is a collection where each element has an assigned **priority**. When removing an element, the one with the **highest priority** is always removed first — regardless of when it was added.

### FIFO vs Priority Queue Comparison

| | Regular Queue | Priority Queue |
|--|--------------|----------------|
| **Order** | Arrival time (FIFO) | Priority value |
| **Removal** | Always removes the FIRST element added | Always removes the HIGHEST priority element |
| **Behaviour** | First In, First Out | Largest-In, First-Out (or smallest, depending on config) |
| **Use case** | Print queue, customer service | Task scheduling, hospital ER, Dijkstra's algorithm |

### Priority Queue Visualised

```
Adding 8 to a priority queue containing {3, 13, 15, 18}:

        add 8
   8 ──────────▶  [ 8, 3, 15, 13, 18 ]
                          ↓
                   (internally sorted)
                   [ 3, 8, 13, 15, 18 ]  ← min-heap internally
                                   ↓
                         remove() → removes 18 (highest = 18)
                   [ 3, 8, 13, 15 ]
```

> ⚠️ **Important Caution:** Whether the **higher number** = higher priority OR **lower number** = higher priority depends on the problem context. Always read the question carefully!
>
> - Hospital: Higher severity number = Higher priority → remove MAX first
> - Job scheduling: Lower job ID = Higher priority → remove MIN first
> - **Java's default `PriorityQueue`: removes the SMALLEST element first (min-heap)**

### Real-World Examples of Priority Queues

| Scenario | Priority = ? | Who goes first |
|---------|-------------|----------------|
| Hospital ER | Severity score (1-10) | Highest severity |
| CPU Scheduling | Process priority | Highest priority process |
| Dijkstra's algorithm | Shortest distance | Smallest distance node |
| Network packets | QoS level | Highest QoS level |
| Print queue | Document urgency | Most urgent document |

---

## 8. Java's Built-in PriorityQueue Class

Java provides `java.util.PriorityQueue` — you don't have to build it from scratch.

```java
import java.util.PriorityQueue;
```

It implements the `java.util.Queue<E>` interface.

**⚠️ Java's default PriorityQueue = MIN priority queue (smallest element comes out first)**

To make it MAX (largest comes out first), use `Collections.reverseOrder()`.

### Constructors

| Constructor | What it creates |
|------------|----------------|
| `new PriorityQueue<>()` | Default min-heap, initial capacity 11 |
| `new PriorityQueue<>(int initialCapacity)` | Min-heap with custom capacity |
| `new PriorityQueue<>(Comparator c)` | Custom ordering (e.g., max-heap) |
| `new PriorityQueue<>(Collection c)` | Build from existing collection |

### Methods Reference Table

| Method | Returns | What it does |
|--------|---------|-------------|
| `offer(E e)` | `boolean` | **Adds** element to the queue |
| `remove(Object o)` | `boolean` | Removes a **specific** object |
| `peek()` | `E` | **Views** the highest-priority element (does NOT remove it) |
| `poll()` | `E` | **Retrieves AND removes** the highest-priority element |
| `clear()` | `void` | Removes **all** elements |
| `size()` | `int` | Returns number of elements |
| `contains(Object o)` | `boolean` | Checks if element exists |

### peek() vs poll() — Critical Difference!

```
Queue: [3, 8, 13, 15, 18]  (min-heap, so 3 is the "head")

peek()  →  returns 3,  Queue is STILL: [3, 8, 13, 15, 18]  (unchanged!)
poll()  →  returns 3,  Queue becomes:  [8, 13, 15, 18]      (3 is GONE!)
```

> 💡 **Memory trick:**
> - `peek()` = peeking through a window — you SEE it but don't touch it
> - `poll()` = polling/taking — you TAKE it away

---

## 9. PriorityQueue Code Examples

### Example 1 — Strings with Natural Ordering (Alphabetical)

```java
import java.util.*;

public class PriorityQueueDemo {
    public static void main(String[] args) {
        
        // ── EXAMPLE A: Default (min) ordering = alphabetical for Strings ──
        PriorityQueue<String> queue1 = new PriorityQueue<>();
        queue1.offer("Oklahoma");
        queue1.offer("Indiana");
        queue1.offer("Georgia");
        queue1.offer("Texas");
        
        System.out.println("Priority queue using Comparable:");
        while (queue1.size() > 0) {
            System.out.print(queue1.poll() + " ");
        }
        // Output: Georgia Indiana Oklahoma Texas  ← alphabetical (G < I < O < T)
        
        System.out.println();
        
        // ── EXAMPLE B: Reverse ordering = reverse alphabetical (Z first) ──
        PriorityQueue<String> queue2 = new PriorityQueue<>(4, Collections.reverseOrder());
        queue2.offer("Oklahoma");
        queue2.offer("Indiana");
        queue2.offer("Georgia");
        queue2.offer("Texas");
        
        System.out.println("Priority queue using Comparator:");
        while (queue2.size() > 0) {
            System.out.print(queue2.poll() + " ");
        }
        // Output: Texas Oklahoma Indiana Georgia  ← reverse alphabetical
    }
}
```

**Output:**
```
Priority queue using Comparable:
Georgia Indiana Oklahoma Texas
Priority queue using Comparator:
Texas Oklahoma Indiana Georgia
```

**Explanation:**
- `queue1` uses **natural ordering** → for Strings, that's alphabetical → "G" comes before "I" before "O" before "T"
- `queue2` uses `Collections.reverseOrder()` → reverses it → "T" comes first

---

### Example 2 — Custom Objects with Comparable

When you have your own class (like `Customer`), Java doesn't know how to compare them. You need to tell Java how — by implementing the `Comparable` interface.

**Step 1 — Create the Customer class:**

```java
public class Customer implements Comparable<Customer> {
    
    private Integer id;    // We'll use id as the priority
    private String name;
    
    // Constructor
    public Customer(Integer id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // Getters and Setters
    public Integer getID()          { return id; }
    public void setID(Integer id)   { this.id = id; }
    public String getName()         { return name; }
    public void setName(String name){ this.name = name; }
    
    // compareTo — tells Java how to COMPARE two Customer objects
    // Returns negative if this < other, 0 if equal, positive if this > other
    @Override
    public int compareTo(Customer c) {
        return this.getID().compareTo(c.getID());
        // This uses Integer's compareTo → smaller ID = higher priority (min-heap default)
    }
    
    // toString — for printing
    @Override
    public String toString() {
        return "Customer [ id=" + id + ", name=" + name + " ]";
    }
}
```

**Step 2 — Use it with PriorityQueue:**

```java
import java.util.*;

public class PriorityQueue2 {
    public static void main(String[] args) {
        
        // Using reverseOrder() → LARGER id = higher priority = removed first
        PriorityQueue<Customer> customerQueue
            = new PriorityQueue<>(Collections.reverseOrder());
        
        customerQueue.add(new Customer(3, "Donald"));
        customerQueue.add(new Customer(1, "Chong"));
        customerQueue.add(new Customer(2, "Ali"));
        customerQueue.add(new Customer(4, "Bala"));
        
        // peek() — see who's at the front WITHOUT removing them
        Customer c = customerQueue.peek();
        if (c != null) {
            System.out.println(c.getName() + " is in the queue");
            
            // poll() — remove and print everyone in priority order
            while ((c = customerQueue.poll()) != null) {
                System.out.println(c);
            }
        }
    }
}
```

**Output:**
```
Bala is in the queue
Customer [ id=4, name=Bala ]
Customer [ id=3, name=Donald ]
Customer [ id=2, name=Ali ]
Customer [ id=1, name=Chong ]
```

**Why does Bala (id=4) come first?**
Because we used `Collections.reverseOrder()` which reverses the natural order. Natural order = smaller id first. Reversed = larger id first. Bala has id=4, which is the largest → Bala comes out first.

---

### Example 3 — Integers (Simple Number Priority)

```java
import java.util.*;

public class IntegerPriorityQueue {
    public static void main(String[] args) {
        
        // Default: min-heap (smallest number = highest priority)
        PriorityQueue<Integer> minQueue = new PriorityQueue<>();
        minQueue.offer(30);
        minQueue.offer(10);
        minQueue.offer(50);
        minQueue.offer(20);
        
        System.out.print("Min-heap order: ");
        while (!minQueue.isEmpty()) {
            System.out.print(minQueue.poll() + " ");
        }
        // Output: 10 20 30 50   ← smallest first
        
        System.out.println();
        
        // Max-heap (largest number = highest priority)
        PriorityQueue<Integer> maxQueue = new PriorityQueue<>(Collections.reverseOrder());
        maxQueue.offer(30);
        maxQueue.offer(10);
        maxQueue.offer(50);
        maxQueue.offer(20);
        
        System.out.print("Max-heap order: ");
        while (!maxQueue.isEmpty()) {
            System.out.print(maxQueue.poll() + " ");
        }
        // Output: 50 30 20 10   ← largest first
    }
}
```

---

## 10. Array-Based Queue (Exercise)

This is the exercise from your slides. Here is the full implementation with explanations:

### Part A — Using Plain Array

```java
public class ArrayQueue<E> {
    
    private Object[] data;   // the array that holds elements
    private int front;       // index of the front element
    private int back;        // index where next element will be added
    private int size;        // current number of elements
    
    // Default constructor — creates array with capacity 10
    public ArrayQueue() {
        data = new Object[10];
        front = 0;
        back = 0;
        size = 0;
    }
    
    // Constructor with custom initial capacity
    public ArrayQueue(int initial) {
        data = new Object[initial];
        front = 0;
        back = 0;
        size = 0;
    }
    
    // enqueue — add element to the back
    public void enqueue(E e) {
        if (size == data.length) {
            resize();  // if array is full, make it bigger
        }
        data[back] = e;
        back = (back + 1) % data.length;  // circular wrap-around
        size++;
    }
    
    // dequeue — remove and return element from the front
    public E dequeue() {
        if (isEmpty()) {
            throw new RuntimeException("Queue is empty!");
        }
        E element = (E) data[front];
        data[front] = null;               // help garbage collection
        front = (front + 1) % data.length; // circular wrap-around
        size--;
        return element;
    }
    
    // getElement — peek at the front without removing
    public E getElement() {
        if (isEmpty()) {
            throw new RuntimeException("Queue is empty!");
        }
        return (E) data[front];
    }
    
    // isEmpty — is the queue empty?
    public boolean isEmpty() {
        return size == 0;
    }
    
    // size — how many elements?
    public int size() {
        return size;
    }
    
    // resize — double the array capacity when full
    public void resize() {
        Object[] newData = new Object[data.length * 2];
        for (int i = 0; i < size; i++) {
            newData[i] = data[(front + i) % data.length];
        }
        data = newData;
        front = 0;
        back = size;
    }
    
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("Queue: [");
        for (int i = 0; i < size; i++) {
            sb.append(data[(front + i) % data.length]);
            if (i < size - 1) sb.append(", ");
        }
        sb.append("]");
        return sb.toString();
    }
}
```

### Part B — Using ArrayList

```java
import java.util.ArrayList;

public class ArrayListQueue<E> {
    
    private ArrayList<E> list = new ArrayList<>();
    
    public ArrayListQueue() {}
    
    public ArrayListQueue(int initial) {
        list = new ArrayList<>(initial);
    }
    
    // enqueue — add to back (ArrayList index = end)
    public void enqueue(E e) {
        list.add(e);  // adds to the end of ArrayList
    }
    
    // dequeue — remove from front (ArrayList index 0)
    // NOTE: This is O(n) — less efficient than LinkedList!
    public E dequeue() {
        if (isEmpty()) throw new RuntimeException("Queue is empty!");
        return list.remove(0);  // removes index 0 and shifts everything left
    }
    
    // getElement — peek at front
    public E getElement() {
        if (isEmpty()) throw new RuntimeException("Queue is empty!");
        return list.get(0);
    }
    
    public boolean isEmpty() { return list.isEmpty(); }
    
    public int size() { return list.size(); }
    
    // resize() is handled automatically by ArrayList internally
    public void resize() {
        // ArrayList auto-resizes; no action needed
        // but we implement it to satisfy the interface contract
    }
    
    @Override
    public String toString() {
        return "Queue: " + list.toString();
    }
}
```

> 💡 **Why `resize()` in ArrayListQueue is empty:** `ArrayList` automatically resizes itself. You still declare the method to honour the class design contract, but there's nothing to do inside.

---

## 11. Common Mistakes & Exam Tips

### ❌ Common Mistakes

**Mistake 1 — Confusing front and back:**
- Elements are ADDED at the **BACK** (tail/rear)
- Elements are REMOVED from the **FRONT** (head)
- Many students get this backwards!

**Mistake 2 — Confusing Queue with Stack:**
- Queue = FIFO (bank queue)
- Stack = LIFO (plate stack)

**Mistake 3 — Assuming Java's PriorityQueue is a max-heap:**
- Java's default `PriorityQueue` is a **MIN-HEAP** (smallest = highest priority)
- To get max-heap behaviour, you MUST pass `Collections.reverseOrder()`

**Mistake 4 — Calling `peek()` when you meant `poll()`:**
- `peek()` = look but DON'T remove
- `poll()` = look AND remove

**Mistake 5 — Not handling empty queue:**
- Always check `isEmpty()` before calling `dequeue()` or `poll()`
- `poll()` returns `null` if empty (safe)
- `remove()` throws `NoSuchElementException` if empty (dangerous if not handled)

### ✅ Exam Tips

1. **Know which end is which:** BACK = insert, FRONT = delete
2. **Know the implementation:** LinkedList is preferred over ArrayList for queue (O(1) vs O(n) deletion)
3. **Know the two design approaches:** Inheritance vs Composition — be able to draw the UML
4. **Know priority queue behaviour:** Default Java = min-heap (smallest out first)
5. **Trace through code:** If given code with enqueue/dequeue operations, trace step by step showing the queue state after EACH operation
6. **compareTo():** If `compareTo` returns `this.id - other.id`, smaller id = higher priority (min-heap). If wrapped in `reverseOrder()`, it flips.

---

## 12. Practice Questions with Full Explanations

---

### 🟢 Question 1 (Concept)

**Where is an element inserted and deleted in a Queue?**

**A.** Inserted at front, deleted at back  
**B.** Inserted at back, deleted at front ✅  
**C.** Inserted at front, deleted at front  
**D.** Inserted at back, deleted at back  

**Explanation:**
- **A is WRONG:** This is backwards. If you insert at the front and delete at the back, the first item you insert would be the LAST to be removed — that's LIFO (Stack behaviour), not FIFO.
- **B is CORRECT:** Queue inserts at the BACK (new elements join at the end of the line) and deletes from the FRONT (the person who waited longest is served first). This gives FIFO ordering.
- **C is WRONG:** Both at front means it would behave like a stack — the last thing you put in is the first thing you take out.
- **D is WRONG:** Both at back also results in LIFO behaviour.

---

### 🟢 Question 2 (Concept)

**What ordering does a Queue follow?**

**A.** LIFO — Last In, First Out  
**B.** LILO — Last In, Last Out  
**C.** FIFO — First In, First Out ✅  
**D.** FILO — First In, Last Out  

**Explanation:**
- **A (LIFO)** — This is how a **Stack** works. Imagine a stack of trays at a cafeteria — you take the TOP tray (last one placed), not the bottom one.
- **B (LILO)** — This doesn't describe any standard data structure. Not a real ordering.
- **C (FIFO) ✅** — This is correct for Queue. The FIRST element inserted is the FIRST one removed. Like a queue at a bus stop — the person who waited the longest boards first.
- **D (FILO)** — FILO is actually the same as LIFO (just described differently) and describes a Stack, not a Queue.

---

### 🟢 Question 3 (Implementation)

**Which data structure is BEST for implementing a Queue efficiently?**

**A.** Array  
**B.** ArrayList  
**C.** LinkedList ✅  
**D.** TreeSet  

**Explanation:**
- **A (Array)** — Arrays have fixed size and require shifting elements during deletion from front. Possible, but requires careful circular index management and manual resizing. Not the most practical.
- **B (ArrayList)** — ArrayList is dynamic, but removing from index 0 (front of queue) requires shifting ALL subsequent elements left — O(n) time complexity for every dequeue. Not efficient.
- **C (LinkedList) ✅** — Removing from the head of a LinkedList is O(1) — just update the head pointer. Adding to the tail is also O(1). This makes both enqueue and dequeue O(1) — perfect for a queue.
- **D (TreeSet)** — A TreeSet is a sorted set. It doesn't support duplicates and doesn't maintain insertion order. Completely wrong for a standard queue.

---

### 🟢 Question 4 (Design)

**What is the difference between using Inheritance vs Composition to implement a Queue?**

**A.** Inheritance is faster at runtime; Composition is slower  
**B.** Inheritance exposes only queue methods; Composition exposes all LinkedList methods  
**C.** Inheritance exposes all LinkedList methods; Composition exposes only queue methods ✅  
**D.** There is no practical difference  

**Explanation:**
- **A** — Wrong. There is no meaningful runtime speed difference between the two approaches.
- **B** — This is backwards! With inheritance (GenericQueue extends LinkedList), the child class inherits ALL methods from the parent (LinkedList), so users can call methods like `add(index, element)` that violate the Queue concept.
- **C ✅** — Correct! With composition (GenericQueue HAS-A LinkedList as a private field), only the methods you explicitly define (`enqueue`, `dequeue`, `getSize`) are visible to the outside. The LinkedList is hidden — encapsulation is preserved.
- **D** — Wrong. The difference is significant in terms of encapsulation and API design. Composition leads to a cleaner, safer Queue interface.

---

### 🟡 Question 5 (Tracing)

**What is the output of the following code?**

```java
GenericQueue<Integer> q = new GenericQueue<>();
q.enqueue(5);
q.enqueue(10);
q.enqueue(3);
System.out.println(q.dequeue());
q.enqueue(7);
System.out.println(q.dequeue());
System.out.println(q);
```

**A.** 5, 10, Queue: [3, 7]  ✅  
**B.** 3, 5, Queue: [10, 7]  
**C.** 10, 5, Queue: [3, 7]  
**D.** 5, 3, Queue: [10, 7]  

**Explanation — trace step by step:**

```
Step 1: enqueue(5)   →  Queue: [5]
Step 2: enqueue(10)  →  Queue: [5, 10]
Step 3: enqueue(3)   →  Queue: [5, 10, 3]
Step 4: dequeue()    →  removes FRONT = 5, prints "5"
                         Queue: [10, 3]
Step 5: enqueue(7)   →  Queue: [10, 3, 7]
Step 6: dequeue()    →  removes FRONT = 10, prints "10"
                         Queue: [3, 7]
Step 7: println(q)   →  "Queue: [3, 7]"
```

- **A ✅** — Output: 5, 10, Queue: [3, 7] — Correct!
- **B** — Wrong. 3 would only be dequeued if both 5 and 10 were already removed before it.
- **C** — Wrong. 10 is the second element, it's not removed until after 5 is gone.
- **D** — Wrong. The second dequeue removes 10 (not 3), because 10 is at the front after 5 is removed.

---

### 🟡 Question 6 (Priority Queue)

**What does Java's default `PriorityQueue` do when you call `poll()`?**

**A.** Removes the element that was added LAST  
**B.** Removes the element that was added FIRST  
**C.** Removes the element with the SMALLEST value ✅  
**D.** Removes the element with the LARGEST value  

**Explanation:**
- **A** — Wrong. That's LIFO/Stack behaviour, not PriorityQueue.
- **B** — Wrong. That's regular FIFO Queue behaviour. Java's PriorityQueue is NOT a simple FIFO queue.
- **C ✅** — Correct! Java's `PriorityQueue` by default is a **min-heap**, meaning it always gives you the element with the SMALLEST value when you call `poll()` or `peek()`.
- **D** — Wrong. To get the LARGEST value first, you must explicitly pass `Collections.reverseOrder()` to the constructor.

---

### 🟡 Question 7 (Priority Queue Code)

**Given this code, what is printed?**

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(40);
pq.offer(10);
pq.offer(30);
pq.offer(20);

System.out.println(pq.peek());
System.out.println(pq.poll());
System.out.println(pq.poll());
System.out.println(pq.size());
```

**A.** 40, 40, 20, 2  
**B.** 10, 10, 20, 2 ✅  
**C.** 10, 10, 20, 3  
**D.** 40, 10, 20, 2  

**Explanation — trace step by step:**

```
pq.offer(40) → PQ: {40}
pq.offer(10) → PQ: {10, 40}       ← 10 floats to top (smaller)
pq.offer(30) → PQ: {10, 40, 30}
pq.offer(20) → PQ: {10, 20, 30, 40}  ← internal min-heap order

pq.peek()   → returns 10, PQ UNCHANGED: {10, 20, 30, 40}  → prints 10
pq.poll()   → returns 10, removes it: {20, 30, 40}          → prints 10
pq.poll()   → returns 20, removes it: {30, 40}              → prints 20
pq.size()   → 2 elements remain                             → prints 2
```

- **A** — Wrong. 40 is the largest, but default PriorityQueue gives SMALLEST first. 40 would only come out last.
- **B ✅** — Correct! peek() gives 10 (smallest, doesn't remove), first poll() removes 10, second poll() removes 20, size is now 2.
- **C** — Wrong. The size after removing two elements from four is 2, not 3.
- **D** — Wrong. peek() and poll() both operate on the SMALLEST element (10), not 40.

---

### 🔴 Question 8 (Advanced — compareTo)

**Consider this compareTo method in a Patient class:**

```java
@Override
public int compareTo(Patient other) {
    return this.severity - other.severity;
}
```

**If used in a default PriorityQueue, what happens when you poll()?**

**A.** Patient with the HIGHEST severity is removed first  
**B.** Patient with the LOWEST severity is removed first ✅  
**C.** Patients are removed in the order they were added  
**D.** This code will not compile  

**Explanation:**

`compareTo` returning `this.severity - other.severity` means:
- If `this.severity < other.severity` → returns **negative** → `this` is "smaller" → comes out FIRST in a min-heap
- So patients with LOWER severity come out first.

To make higher severity patients come out first (correct for ER), you'd flip it:
```java
return other.severity - this.severity;  // or use Collections.reverseOrder()
```

- **A** — Wrong. This compareTo makes it a min-heap on severity. Lower severity = out first (opposite of what an ER needs).
- **B ✅** — Correct. `this - other` = natural/ascending order = smallest severity out first. This is actually the WRONG design for an ER (you'd want highest severity first), but it's what this compareTo produces.
- **C** — Wrong. PriorityQueue never preserves insertion order — that's what regular Queue does.
- **D** — Wrong. The code compiles perfectly fine. Both classes would need to implement `Comparable<Patient>`, but `compareTo` itself is valid.

---

### 🔴 Question 9 (Tracing with PriorityQueue + Custom Object)

**Trace this code (using the Customer class from the lecture with `reverseOrder()`):**

```java
PriorityQueue<Customer> q = new PriorityQueue<>(Collections.reverseOrder());
q.add(new Customer(3, "Donald"));
q.add(new Customer(1, "Chong"));
q.add(new Customer(4, "Bala"));
q.add(new Customer(2, "Ali"));

while (!q.isEmpty()) {
    System.out.println(q.poll().getName());
}
```

**A.** Donald, Chong, Bala, Ali  
**B.** Chong, Ali, Donald, Bala  
**C.** Bala, Donald, Ali, Chong ✅  
**D.** Donald, Bala, Ali, Chong  

**Explanation:**

The Customer class `compareTo` does `this.id.compareTo(other.id)` = natural (ascending) order.

`Collections.reverseOrder()` **reverses** this → descending order → LARGEST id comes out first.

```
IDs in queue: 3, 1, 4, 2
With reverseOrder (descending):
  poll() → id=4 → "Bala"
  poll() → id=3 → "Donald"
  poll() → id=2 → "Ali"
  poll() → id=1 → "Chong"
```

- **A** — Wrong. This would be the order if no reverseOrder was applied starting from id=3 first (but that's not how a heap works either).
- **B** — Wrong. This represents ascending order without reversal but starting from the smallest.
- **C ✅** — Correct! Descending order by id: 4→Bala, 3→Donald, 2→Ali, 1→Chong.
- **D** — Wrong. This mixes up the id ordering.

---

## 📌 Final Summary Cheat Sheet

```
┌─────────────────────────────────────────────────────────────┐
│                    QUEUE QUICK REFERENCE                    │
├─────────────────────────────────────────────────────────────┤
│ Type      │ Order │ Insert │ Delete │ Use Case              │
├───────────┼───────┼────────┼────────┼───────────────────────┤
│ Queue     │ FIFO  │ Back   │ Front  │ Customer service      │
│ Stack     │ LIFO  │ Top    │ Top    │ Undo operations       │
│ PQ (min)  │ Min   │ Any    │ Min    │ Dijkstra's algorithm  │
│ PQ (max)  │ Max   │ Any    │ Max    │ Hospital ER           │
├─────────────────────────────────────────────────────────────┤
│                   KEY METHODS (Regular Queue)               │
├─────────────────────────────────────────────────────────────┤
│ enqueue(e) → adds to BACK                                   │
│ dequeue()  → removes from FRONT, returns element           │
│ getSize()  → number of elements                            │
├─────────────────────────────────────────────────────────────┤
│                KEY METHODS (Java PriorityQueue)             │
├─────────────────────────────────────────────────────────────┤
│ offer(e)   → add element                                    │
│ poll()     → remove & return highest priority (min default) │
│ peek()     → view highest priority WITHOUT removing         │
│ remove(o)  → remove specific object                        │
│ size()     → count elements                                 │
│ contains() → check existence                               │
│ clear()    → remove all                                    │
├─────────────────────────────────────────────────────────────┤
│               IMPLEMENTATION DECISION GUIDE                 │
├─────────────────────────────────────────────────────────────┤
│ Need FIFO? → Use GenericQueue (LinkedList + composition)    │
│ Need priority? → Use Java's PriorityQueue                   │
│ Default PQ → min-heap (smallest out first)                 │
│ Max-heap → new PriorityQueue<>(Collections.reverseOrder())  │
│ Custom priority → implement Comparable.compareTo()          │
└─────────────────────────────────────────────────────────────┘
```

---

> 📝 **Study tip:** The best way to master Queue is to trace through code by hand — draw the queue state after EVERY single operation (enqueue/dequeue). Do this for all the examples above until you can predict the output without running the code.

> 🔗 **Animation tool:** [https://yongdanielliang.github.io/animation/web/Queue.html](https://yongdanielliang.github.io/animation/web/Queue.html)
