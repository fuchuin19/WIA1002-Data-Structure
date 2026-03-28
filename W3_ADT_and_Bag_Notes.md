# WIA 1002 Data Structure — Complete Study Notes
## Topic: Abstract Data Type (ADT) & Bag
> **Course:** WIA 1002 Data Structure | Universiti Malaya | Sem 2, 2025/2026  
> **Coverage:** ADT concept, Collection vs Container, Data Structure, Bag ADT, UML, Interface (Java), ArrayBag, LinkedBag

---

## Table of Contents
1. [Why Do We Organize Data?](#1-why-do-we-organize-data)
2. [Abstract Data Type (ADT)](#2-abstract-data-type-adt)
3. [Collection vs Container](#3-collection-vs-container)
4. [Data Structure — The Implementation Side](#4-data-structure--the-implementation-side)
5. [ADT vs Data Structure — The Big Picture](#5-adt-vs-data-structure--the-big-picture)
6. [Advantages of ADT](#6-advantages-of-adt)
7. [The ADT Bag](#7-the-adt-bag)
8. [CRC Card (Class-Responsibility-Collaboration)](#8-crc-card)
9. [UML Notation for Bag](#9-uml-notation-for-bag)
10. [Design Decisions for Unusual Conditions](#10-design-decisions-for-unusual-conditions)
11. [Java Interface for Bag](#11-java-interface-for-bag)
12. [Using the ADT Bag — Real Examples](#12-using-the-adt-bag--real-examples)
13. [ADT Bag vs Vending Machine Analogy](#13-adt-bag-vs-vending-machine-analogy)
14. [Bag vs Set — Java Class Library](#14-bag-vs-set--java-class-library)
15. [Implementations: ArrayBag & LinkedBag](#15-implementations-arraybag--linkedbag)
16. [Practice Questions with Explanations](#16-practice-questions-with-explanations)
17. [Quick Reference Summary](#17-quick-reference-summary)

---

## 1. Why Do We Organize Data?

In everyday life, we already organize data without thinking about it:

| Real-World Object | What it does |
|---|---|
| To-Do List | Keeps tasks in order |
| Dictionary | Lets you look up words by key |
| Waiting Queue (Ticket Line) | First person in = first person served |
| Stack of Books | Last book placed = first book you pick up |
| Road Map | Connects locations with paths |
| Organizational Chart | Hierarchy of relationships |

**Key Insight:** These real-world organization patterns map directly to data structures in programming. When we say "stack" in CS, it literally behaves like a stack of books. When we say "queue", it behaves like a queue at a ticket counter.

---

## 2. Abstract Data Type (ADT)

### Definition
An **Abstract Data Type (ADT)** is a **conceptual model** (a blueprint, a specification) that defines:
1. **What type of data** it stores
2. **What operations** can be performed on that data

It does **NOT** say:
- How the data is stored in memory
- How the operations are coded/implemented

> **"Abstract"** means we are ignoring the irrelevant details (like storage method) and only focusing on *what* the data type does, not *how* it does it.

### Simple Analogy — A TV Remote Control
- You know the remote has a **Volume Up** button (operation).
- You know it **controls the TV volume** (what it does).
- You have **no idea** what circuit inside the remote processes your button press (implementation is hidden).
- You don't need to know — you just use it!

That's exactly what an ADT does: it gives you a clean, simple interface to use without worrying about internal complexity.

### The Two Sides of an ADT

```
┌──────────────────────────────────────────────────────┐
│                      ADT                             │
│                                                      │
│  PUBLIC (Client sees this):                          │
│    - What data is stored                             │
│    - What operations are available                   │
│                                                      │
│  PRIVATE (Hidden from client):                       │
│    - HOW data is stored internally                   │
│    - HOW operations are coded                        │
└──────────────────────────────────────────────────────┘
```

### Real CS Example — Stack ADT
- **Data stored:** A collection of elements (integers, strings, objects...)
- **Operations:** `push()` (add to top), `pop()` (remove from top), `peek()` (look at top), `isEmpty()`
- **Hidden:** Whether it's stored in an array or a linked list internally — doesn't matter to the user

---

## 3. Collection vs Container

These two terms are often confused. Here's the clear distinction:

### Collection
A **collection** is the **abstract idea** (the ADT) of grouping objects together. It defines *what* operations you can do (add, remove, search), but NOT how.

> Think of "collection" as the **recipe** — it tells you what ingredients and steps exist, but not which brand of pan to use.

### Container
A **container** is the **actual Java class** (or code) that **implements** a collection. It brings the abstract idea to life in a specific programming language.

> Think of "container" as the **actual cooked dish** — real, usable, made with a specific method.

### Side-by-side Comparison

| Concept | Collection (ADT) | Container (Class) |
|---|---|---|
| Nature | Abstract / Conceptual | Concrete / Real code |
| Language | Language-independent | Language-specific (e.g., Java) |
| Example | "A Bag that stores objects" | `ArrayBag<T>` class in Java |
| Defines | WHAT operations exist | HOW operations work |

---

## 4. Data Structure — The Implementation Side

A **data structure** is the **actual implementation** of an ADT in a programming language.

- It decides exactly how data is stored in memory.
- It decides exactly how each operation works step-by-step.

### Example
| ADT (what) | Data Structure (how) |
|---|---|
| List ADT | Array, or Linked List |
| Stack ADT | Array-based stack, or Linked-node stack |
| Queue ADT | Circular array, or Linked-node queue |
| Bag ADT | Fixed-size array (`ArrayBag`), or Linked nodes (`LinkedBag`) |

---

## 5. ADT vs Data Structure — The Big Picture

```
         ADT Layer (What)
    ┌─────────────────────────┐
    │  List ADT               │  → "I support add, remove, get"
    │  Stack ADT              │  → "I support push, pop (LIFO)"
    │  Queue ADT              │  → "I support enqueue, dequeue (FIFO)"
    │  Bag ADT                │  → "I support add, remove, contains..."
    └────────────┬────────────┘
                 │
          implementation
                 │
                 ▼
         Data Structure Layer (How)
    ┌─────────────────────────┐
    │  Array                  │  → Fixed memory slots
    │  Linked List            │  → Nodes connected by pointers
    └─────────────────────────┘
```

### Cinema Reservation System Example
Applying ADT thinking to a real system:

| ADT Concept | Cinema System |
|---|---|
| **Data** | Seats, whether each seat is reserved or available |
| **Operations** | Check availability, reserve a seat, cancel a reservation, find a block of seats |
| **Implementation** | IGNORED at ADT level — could be array, database, etc. |

---

## 6. Advantages of ADT

### Advantage 1: Information Hiding
The internal details (how data is stored, how operations are coded) are hidden from the user. The user only sees and interacts with the **interface** — the list of available operations.

**Why this matters:** If the developer improves the implementation (e.g., switches from array to linked list for better performance), users of the ADT don't need to change their code at all. The interface stays the same.

### Advantage 2: Shared Data and Operations
Multiple programmers can build systems using the same ADT without knowing each other's code. Everyone agrees on the *interface*, not the implementation.

### Advantage 3: Modularity and Reusability
An ADT like `Bag` can be reused in completely different applications — a shopping cart, a piggy bank, a playlist — without rewriting it.

### Advantage 4: Abstraction Simplifies Thinking
When you're designing a system at a high level, you can say "I'll use a Stack here" without worrying yet about whether it will be an array-stack or linked-stack. You separate the *design* from the *implementation*.

---

## 7. The ADT Bag

### What is a Bag?
A **Bag** is one of the simplest and most fundamental ADTs. It is modeled after a real bag (like a shopping bag or backpack).

### Key Properties of a Bag:
| Property | Value |
|---|---|
| Order | **No specific order** — items are not sorted or indexed |
| Duplicates | **Allowed** — you can put two identical items in |
| Size limit | Depends on implementation — generally considered finite |
| Access pattern | You can't "peek" at a specific position — you grab randomly |

### Answers to the "Yes or No?" Questions from the Slides:
- **Should items be stored in a specific order?** → **NO** — a bag has no order
- **Can you keep repetitive/duplicate items?** → **YES** — duplicates are allowed
- **Is there a standard size limit?** → **NO** — no standard limit defined at ADT level

### Possible Behaviors (Operations):
1. **Add** an object to the bag
2. **Remove** an unspecified (random) object from the bag
3. **Remove a specific** object from the bag
4. **Clear** — remove everything
5. **Get the current size** — how many items are in the bag
6. **Check if empty**
7. **Get frequency of** — how many times does an item appear?
8. **Contains** — does the bag have this item?
9. **toArray** — get all items as an array

---

## 8. CRC Card

A **CRC Card** (Class-Responsibility-Collaboration) is a design tool used to describe a class before writing any code.

| Section | Description |
|---|---|
| **Class Name** | The name of the ADT (e.g., `Bag`) |
| **Responsibilities** | All the things this class can do (its operations/methods) |
| **Collaborations** | Other classes this class works with |

### CRC Card for Bag

```
┌─────────────────────────────────────────────────────────────────┐
│                            Bag                                  │
├────────────────────────────────────────────────────────────────┤
│ Responsibilities                                                │
│   Get the number of items currently in the bag                  │
│   See whether the bag is empty                                  │
│   Add a given object to the bag                                 │
│   Remove an unspecified object from the bag                     │
│   Remove one occurrence of a particular object from the bag     │
│   Remove all objects from the bag                               │
│   Count how many times a certain object occurs in the bag       │
│   Test whether the bag contains a particular object             │
│   Look at all objects that are in the bag                       │
├────────────────────────────────────────────────────────────────┤
│ Collaborations                                                  │
│   The class of objects that the bag can contain                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. UML Notation for Bag

**UML (Unified Modeling Language)** is a standard way to visually represent a class before writing code.

### UML Diagram

```
┌──────────────────────────────────────┐
│                 Bag                  │
├──────────────────────────────────────┤
│  (no data fields shown at ADT level) │
├──────────────────────────────────────┤
│ +getCurrentSize(): integer           │
│ +isEmpty(): boolean                  │
│ +add(newEntry: T): boolean           │
│ +remove(): T                         │
│ +remove(anEntry: T): boolean         │
│ +clear(): void                       │
│ +getFrequencyOf(anEntry: T): integer │
│ +contains(anEntry: T): boolean       │
│ +toArray(): T[]                      │
└──────────────────────────────────────┘
```

> **Note:** The `+` prefix means the method is **public** (accessible by the client/user of the ADT). The `T` is a **generic type** — it means the Bag can hold any type of object (String, Integer, custom class, etc.).

### Full Method Specification Table

| Pseudocode | UML Notation | Input | Output | What it does |
|---|---|---|---|---|
| `getCurrentSize()` | `+getCurrentSize(): integer` | None | int | Returns how many items are currently in the bag |
| `isEmpty()` | `+isEmpty(): boolean` | None | boolean | Returns `true` if bag has zero items, `false` otherwise |
| `add(newEntry)` | `+add(newEntry: T): boolean` | An object T | boolean | Adds the object; returns `true` if successful |
| `remove()` | `+remove(): T` | None | T (the removed object) or null | Removes and returns an **unspecified** (random) item; null if bag is empty |
| `remove(anEntry)` | `+remove(anEntry: T): boolean` | An object T | boolean | Removes one occurrence of the specified item; `true` if found and removed |
| `clear()` | `+clear(): void` | None | Nothing | Empties the entire bag |
| `getFrequencyOf(anEntry)` | `+getFrequencyOf(anEntry: T): integer` | An object T | int | Returns how many times that item appears in the bag |
| `contains(anEntry)` | `+contains(anEntry: T): boolean` | An object T | boolean | Returns `true` if the item is in the bag, `false` otherwise |
| `toArray()` | `+toArray(): T[]` | None | Array of T | Returns a new array containing all items in the bag |

---

## 10. Design Decisions for Unusual Conditions

When writing ADTs, you must decide: **what happens when something goes wrong?**

For example: What if you try to `remove()` from an **empty bag**?

Here are 6 strategies, with pros and cons:

| Strategy | Description | Example | Drawback |
|---|---|---|---|
| **Assume it won't happen** | Ignore edge cases entirely | Just code the happy path | Crashes in production |
| **Ignore invalid situations** | Do nothing if invalid | `remove()` on empty bag → just does nothing | User gets no feedback |
| **Guess at client's intention** | Try to figure out what they meant | If empty, maybe return a default value | Leads to unexpected behavior |
| **Return a value that signals a problem** | Return `null` or `-1` | `remove()` on empty → returns `null` | User must check for null always |
| **Return a boolean** | Return `true`/`false` to indicate success | `remove(anEntry)` returns `false` if not found | ✅ Clean and simple |
| **Throw an exception** | Raise an error the caller must handle | `throw new EmptyBagException()` | ✅ Explicit, forces handling |

> **Best practice in Java:** For methods that return objects, return `null` on failure (like `remove()` returning `null` if empty). For methods that return booleans, that boolean IS the signal (`remove(anEntry)` returns `false` if not found). For critical errors, throw exceptions.

---

## 11. Java Interface for Bag

In Java, an **interface** is the code version of an ADT specification. It lists all the method signatures (name, parameters, return type) but no implementation.

```java
/**
 * An interface that describes the operations of a bag of objects.
 * This is the ADT specification — no implementation here.
 */
public interface BagInterface<T> {

    /** Returns the number of entries currently in the bag.
     *  @return The integer number of entries in the bag. */
    public int getCurrentSize();

    /** Checks whether the bag is empty.
     *  @return True if the bag is empty, false if not. */
    public boolean isEmpty();

    /** Adds a new entry to this bag.
     *  @param newEntry The object to be added as a new entry.
     *  @return True if the addition is successful, false if not. */
    public boolean add(T newEntry);

    /** Removes one unspecified entry from this bag, if possible.
     *  @return The removed entry if successful, otherwise null. */
    public T remove();

    /** Removes one occurrence of a given entry from this bag, if possible.
     *  @param anEntry The entry to be removed.
     *  @return True if the removal was successful, false if not. */
    public boolean remove(T anEntry);

    /** Removes all entries from this bag. */
    public void clear();

    /** Counts the number of times a given entry appears in this bag.
     *  @param anEntry The entry to be counted.
     *  @return The number of times anEntry appears in the bag. */
    public int getFrequencyOf(T anEntry);

    /** Tests whether this bag contains a given entry.
     *  @param anEntry The entry to locate.
     *  @return True if the bag contains anEntry, false if not. */
    public boolean contains(T anEntry);

    /** Retrieves all entries that are in this bag.
     *  @return A newly allocated array of all the entries in the bag.
     *          Note: If the bag is empty, the returned array is empty. */
    public T[] toArray();

} // end BagInterface
```

### Why Use an Interface?
- Multiple different classes can implement the same interface (`ArrayBag`, `LinkedBag`).
- Code that uses `BagInterface` works with ANY implementation — you can swap the implementation without changing client code.
- Forces all implementations to provide ALL the required methods.

---

## 12. Using the ADT Bag — Real Examples

### Example 1: Online Shopping Cart

```java
public class OnlineShopper {
    public static void main(String[] args) {

        // Define items available
        Item[] items = {
            new Item("Bird feeder", 2050),
            new Item("Squirrel guard", 1547),
            new Item("Bird bath", 4499),
            new Item("Sunflower seeds", 1295)
        };

        // Create a Bag to act as the shopping cart
        // Note: We use BagInterface (the ADT type), not ArrayBag directly
        // This means we could swap ArrayBag for LinkedBag and nothing else changes!
        BagInterface<Item> shoppingCart = new Bag<>();
        int totalCost = 0;

        // Add selected items to cart
        for (int index = 0; index < items.length; index++) {
            Item nextItem = items[index];
            shoppingCart.add(nextItem);
            totalCost = totalCost + nextItem.getPrice();
        }

        // Simulate checkout: remove items one by one
        while (!shoppingCart.isEmpty()) {
            System.out.println(shoppingCart.remove());
        }
        System.out.println("Total cost: $" + totalCost / 100 + "." + totalCost % 100);
    }
}
```

**Output:**
```
Sunflower seeds  $12.95
Bird bath        $44.99
Squirrel guard   $15.47
Bird feeder      $20.50
Total cost:      $93.91
```

> **Notice:** Items come out in no particular order — that's a key Bag property (no guaranteed order).

---

### Example 2: Piggy Bank

This shows how the Bag ADT can be **reused** inside a completely different concept — a piggy bank.

```java
public class PiggyBank {

    // The piggy bank USES a Bag internally — the coins ARE a bag of coins
    private BagInterface<Coin> coins;

    // Constructor: create an empty bag of coins
    public PiggyBank() {
        coins = new Bag<>();
    }

    // Add a coin to the piggy bank
    public boolean add(Coin aCoin) {
        return coins.add(aCoin);
    }

    // Remove (shake out) a random coin
    public Coin remove() {
        return coins.remove();
    }

    // Check if piggy bank is empty
    public boolean isEmpty() {
        return coins.isEmpty();
    }
}
```

**Demo usage:**
```java
public class PiggyBankExample {
    public static void main(String[] args) {

        PiggyBank myBank = new PiggyBank();

        addCoin(new Coin(1, 2010), myBank);   // PENNY
        addCoin(new Coin(5, 2011), myBank);   // NICKEL
        addCoin(new Coin(10, 2000), myBank);  // DIME
        addCoin(new Coin(25, 2012), myBank);  // QUARTER

        System.out.println("Removing all the coins:");
        int amountRemoved = 0;

        while (!myBank.isEmpty()) {
            Coin removedCoin = myBank.remove();
            System.out.println("Removed a " + removedCoin.getCoinName() + ".");
            amountRemoved = amountRemoved + removedCoin.getValue();
        }

        System.out.println("All done. Removed " + amountRemoved + " cents.");
    }

    private static void addCoin(Coin aCoin, PiggyBank aBank) {
        if (aBank.add(aCoin))
            System.out.println("Added a " + aCoin.getCoinName() + ".");
        else
            System.out.println("Tried to add a " + aCoin.getCoinName() + ", but couldn't.");
    }
}
```

**Output:**
```
Added a PENNY.
Added a NICKEL.
Added a DIME.
Added a QUARTER.
Removing all the coins:
Removed a QUARTER.
Removed a DIME.
Removed a NICKEL.
Removed a PENNY.
All done. Removed 41 cents.
```

---

## 13. ADT Bag vs Vending Machine Analogy

This is one of the best ways to understand ADTs:

| Vending Machine | ADT Bag |
|---|---|
| You can only use the buttons/interface the machine provides | You can only call the methods defined in the ADT interface |
| You must understand what each button does | You must understand what each method does (read the specification) |
| You cannot reach inside the machine | You cannot directly access the internal data of the ADT |
| You can use the machine without knowing its internal wiring | You can use the ADT without knowing how it's implemented |
| The machine works even if its internal parts are replaced | The ADT works even if the underlying implementation changes (array → linked list) |

---

## 14. Bag vs Set — Java Class Library

The Java standard library has a `Set` interface, which is similar to Bag but with one critical difference:

| Feature | Bag | Set |
|---|---|---|
| Duplicates | **Allowed** | **NOT allowed** |
| Order | No specific order | No specific order |
| `add()` behavior | Always adds (if space) | Only adds if item doesn't already exist |

```java
// Set interface (simplified):
public interface SetInterface<T> {
    public int getCurrentSize();
    public boolean isEmpty();
    public boolean add(T newEntry);      // Returns FALSE if item already in set
    public boolean remove(T anEntry);
    public T remove();
    public void clear();
    public boolean contains(T anEntry);
    public T[] toArray();
}
```

> **Key difference:** In a Bag, `add("apple")` twice → bag has two "apple" entries. In a Set, `add("apple")` twice → set still has only ONE "apple" entry.

---

## 15. Implementations: ArrayBag & LinkedBag

The Bag ADT can be implemented in two main ways. The interface stays identical — only the internal mechanism changes.

---

### 15A. ArrayBag (Fixed-Size Array Implementation)

**Idea:** Store all bag entries in a regular array. Track how many items are currently in the bag with `numberOfEntries`.

```
bag array:   [ "apple" | "banana" | "apple" | "cherry" |  null  |  null  ]
index:             0         1          2          3          4        5
numberOfEntries = 4
DEFAULT_CAPACITY = 25
```

#### UML for ArrayBag

```
┌──────────────────────────────────────┐
│              ArrayBag                │
├──────────────────────────────────────┤
│  -bag: T[]                           │  ← The actual array storing items
│  -numberOfEntries: integer           │  ← How many items are currently stored
│  -DEFAULT_CAPACITY: integer          │  ← Default max size (e.g., 25)
├──────────────────────────────────────┤
│ +getCurrentSize(): integer           │
│ +isEmpty(): boolean                  │
│ +add(newEntry: T): boolean           │
│ +remove(): T                         │
│ +remove(anEntry: T): boolean         │
│ +clear(): void                       │
│ +getFrequencyOf(anEntry: T): integer │
│ +contains(anEntry: T): boolean       │
│ +toArray(): T[]                      │
│ -isArrayFull(): boolean              │  ← Private helper, not in interface
└──────────────────────────────────────┘
```

#### Key Code Outline

```java
public final class ArrayBag<T> implements BagInterface<T> {

    private final T[] bag;
    private int numberOfEntries;
    private static final int DEFAULT_CAPACITY = 25;

    // Default constructor — creates bag with default size of 25
    public ArrayBag() {
        this(DEFAULT_CAPACITY);
    }

    // Constructor with custom capacity
    @SuppressWarnings("unchecked")
    public ArrayBag(int capacity) {
        // Java doesn't allow creating generic arrays directly, so we cast
        T[] tempBag = (T[]) new Object[capacity];
        bag = tempBag;
        numberOfEntries = 0;
    }

    // Add an item to the bag
    public boolean add(T newEntry) {
        boolean result = true;
        if (isArrayFull()) {
            result = false;   // Can't add — bag is full
        } else {
            bag[numberOfEntries] = newEntry;
            numberOfEntries++;
        }
        return result;
    }

    // Get current number of items
    public int getCurrentSize() {
        return numberOfEntries;
    }

    // Check if bag is empty
    public boolean isEmpty() {
        return numberOfEntries == 0;
    }

    // Private helper: is the array completely full?
    private boolean isArrayFull() {
        return numberOfEntries >= bag.length;
    }

    // Get all items as an array
    public T[] toArray() {
        @SuppressWarnings("unchecked")
        T[] result = (T[]) new Object[numberOfEntries];
        for (int index = 0; index < numberOfEntries; index++) {
            result[index] = bag[index];
        }
        return result;
    }

    // Remove an unspecified item (removes from last filled slot)
    public T remove() {
        T result = null;
        if (!isEmpty()) {
            result = bag[numberOfEntries - 1];
            bag[numberOfEntries - 1] = null;  // Help garbage collection
            numberOfEntries--;
        }
        return result;
    }

    // Remove a specific item
    public boolean remove(T anEntry) {
        int index = getIndexOf(anEntry);   // Find where it is
        T result = removeEntry(index);     // Remove from that position
        return anEntry.equals(result);
    }

    // Private helper: find index of an item (-1 if not found)
    private int getIndexOf(T anEntry) {
        int where = -1;
        boolean found = false;
        int index = 0;
        while (!found && index < numberOfEntries) {
            if (anEntry.equals(bag[index])) {
                found = true;
                where = index;
            }
            index++;
        }
        return where;
    }

    // Check if item exists
    public boolean contains(T anEntry) {
        return getIndexOf(anEntry) >= 0;
    }

    // Count how many times item appears
    public int getFrequencyOf(T anEntry) {
        int counter = 0;
        for (int index = 0; index < numberOfEntries; index++) {
            if (anEntry.equals(bag[index])) {
                counter++;
            }
        }
        return counter;
    }

    // Remove all items
    public void clear() {
        while (!isEmpty()) {
            remove();
        }
    }

} // end ArrayBag
```

#### ArrayBag — Pros and Cons

| Pros | Cons |
|---|---|
| Simple to implement | Fixed maximum size (can't grow beyond capacity) |
| Fast access by index | If you declare size 25 but only use 3, you waste memory |
| Memory-efficient for full bags | Adding when full → returns false (fails silently if not handled) |

---

### 15B. LinkedBag (Linked-Node Implementation)

**Idea:** Instead of an array, store each item in a "node" that points to the next node. The bag grows dynamically — no fixed size limit!

```
firstNode → [Node: "apple" | next→] → [Node: "banana" | next→] → [Node: "cherry" | next→] → null
```

#### Key Code Outline

```java
public final class LinkedBag<T> implements BagInterface<T> {

    private Node firstNode;      // Reference to first node in the chain
    private int numberOfEntries; // How many nodes exist

    // Constructor: empty bag
    public LinkedBag() {
        firstNode = null;
        numberOfEntries = 0;
    }

    // Private inner class — each "box" that holds one item
    private class Node {
        private T data;    // The actual item stored
        private Node next; // Pointer to the next node

        private Node(T dataPortion) {
            this(dataPortion, null);
        }

        private Node(T dataPortion, Node nextNode) {
            data = dataPortion;
            next = nextNode;
        }
    }

    // Add item — adds to the FRONT of the chain (most efficient)
    public boolean add(T newEntry) {
        Node newNode = new Node(newEntry);
        newNode.next = firstNode;  // New node points to old first
        firstNode = newNode;       // New node is now the first
        numberOfEntries++;
        return true;               // LinkedBag NEVER gets full!
    }

    public int getCurrentSize() {
        return numberOfEntries;
    }

    public boolean isEmpty() {
        return numberOfEntries == 0;
    }

    // Remove from front (most efficient for linked list)
    public T remove() {
        T result = null;
        if (firstNode != null) {
            result = firstNode.data;
            firstNode = firstNode.next; // Move firstNode pointer to next node
            numberOfEntries--;
        }
        return result;
    }

} // end LinkedBag
```

#### ArrayBag vs LinkedBag — Comparison

| Feature | ArrayBag | LinkedBag |
|---|---|---|
| Storage | Fixed-size array | Chain of nodes (dynamic) |
| Max size | Fixed (e.g., 25) | **No limit** — grows as needed |
| Memory usage | Can waste memory | Uses only what's needed |
| `add()` performance | O(1) if not full | O(1) — always adds to front |
| Implementation complexity | Simpler | Slightly more complex (nodes) |
| Can it ever be full? | YES — `isArrayFull()` check needed | **NO** — always returns true on add |

---

## 16. Practice Questions with Explanations

---

### Question 1 — Basic Concept

**Which of the following best describes an Abstract Data Type (ADT)?**

A) A specific data storage method like an array or linked list  
B) A conceptual model that defines what data is stored and what operations can be performed  
C) A Java class that implements a collection  
D) A programming language construct for defining variables  

**✅ Correct Answer: B**

**Explanations:**
- **A** — Incorrect. Arrays and linked lists are *data structures* — they are implementations, not ADTs.
- **B** — ✅ Correct. An ADT is a *conceptual model* (blueprint) that specifies WHAT data is stored and WHAT operations exist, without specifying HOW.
- **C** — Incorrect. A Java class that implements a collection is a *container* (the implementation layer, not the abstraction layer).
- **D** — Incorrect. ADT is not a basic language construct for variables. It's a design concept.

---

### Question 2 — Collection vs Container

**What is the difference between a "collection" and a "container" in data structures?**

A) A collection is a data structure; a container is an ADT  
B) A collection defines what operations exist (ADT); a container is a class that implements it  
C) There is no difference — the terms are interchangeable  
D) A collection can only hold primitive types; a container can hold objects  

**✅ Correct Answer: B**

**Explanations:**
- **A** — Incorrect. This has the relationship backwards. Collection = ADT concept; Container = implementation.
- **B** — ✅ Correct. Collection (ADT level) = defines what operations are possible. Container (class level) = provides the actual code/implementation.
- **C** — Incorrect. They are related but distinct concepts with important differences.
- **D** — Incorrect. This distinction is not what differentiates collections from containers.

---

### Question 3 — Bag Properties

**Which of the following is TRUE about the ADT Bag?**

A) Items in a Bag are stored in alphabetical order  
B) A Bag cannot contain duplicate items  
C) A Bag is a finite collection with no particular order and can contain duplicates  
D) A Bag can only store integer values  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. A Bag does NOT maintain any order. Items are stored with no guaranteed ordering.
- **B** — Incorrect. A Bag CAN contain duplicates (e.g., you can add "apple" twice and the bag will have two "apple" entries). This is what distinguishes it from a Set.
- **C** — ✅ Correct. A Bag is defined as a finite collection in no particular order, where duplicates are permitted.
- **D** — Incorrect. Bag is a generic ADT — it can hold any type of object (T is a generic type parameter).

---

### Question 4 — Method Behavior

**What does `remove()` (no parameters) return when the bag is empty?**

A) 0  
B) false  
C) An empty array  
D) null  

**✅ Correct Answer: D**

**Explanations:**
- **A** — Incorrect. `0` is an integer. `remove()` returns type `T` (an object), not an int.
- **B** — Incorrect. `false` is what `remove(anEntry)` might return. The no-argument `remove()` returns the removed object itself, or `null` on failure.
- **C** — Incorrect. `toArray()` returns an array. `remove()` returns a single object.
- **D** — ✅ Correct. When the bag is empty and `remove()` is called, the method returns `null` to signal that no object was removed.

---

### Question 5 — Method Selection

**A client wants to check if the string "hello" appears more than once in a Bag. Which method should they use?**

A) `contains("hello")`  
B) `remove("hello")`  
C) `getFrequencyOf("hello")`  
D) `isEmpty()`  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. `contains("hello")` only tells you whether "hello" is in the bag at all (true/false). It doesn't tell you how many times.
- **B** — Incorrect. `remove("hello")` would delete one occurrence. We don't want to delete anything, just count.
- **C** — ✅ Correct. `getFrequencyOf("hello")` returns an integer — exactly how many times "hello" appears. If it returns > 1, it appears more than once.
- **D** — Incorrect. `isEmpty()` just checks if the bag has zero items. It has nothing to do with counting a specific item.

---

### Question 6 — ADT vs Data Structure

**Which statement correctly distinguishes an ADT from a data structure?**

A) An ADT specifies HOW data is stored; a data structure specifies WHAT operations exist  
B) An ADT specifies WHAT operations exist; a data structure specifies HOW those operations are implemented  
C) ADTs are used in Java; data structures are used in Python  
D) ADTs are faster than data structures  

**✅ Correct Answer: B**

**Explanations:**
- **A** — Incorrect. This is completely backwards. ADT = WHAT; Data structure = HOW.
- **B** — ✅ Correct. The ADT is the specification (WHAT), and the data structure is the implementation (HOW). Example: Bag ADT says "you can add and remove items." ArrayBag says "I implement add by storing in slot `bag[numberOfEntries]`."
- **C** — Incorrect. Both ADTs and data structures exist in all programming languages. This is a conceptual distinction, not a language one.
- **D** — Incorrect. ADTs are abstract concepts — they don't have speed. Speed/performance belongs to the data structure implementation.

---

### Question 7 — UML Reading

**In UML notation, what does the `+` prefix before a method name indicate?**

A) The method is private  
B) The method is protected  
C) The method is public  
D) The method is static  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. Private methods in UML use the `-` prefix (e.g., `-isArrayFull(): boolean`).
- **B** — Incorrect. Protected methods in UML use the `#` prefix.
- **C** — ✅ Correct. `+` means public in UML. All the Bag interface methods (getCurrentSize, isEmpty, add, etc.) are `+` because they are part of the public interface accessible to clients.
- **D** — Incorrect. UML does not use `+` for static. Static is usually indicated by underlining in UML.

---

### Question 8 — ArrayBag vs LinkedBag

**Which of the following is an advantage of LinkedBag over ArrayBag?**

A) LinkedBag is simpler to implement  
B) LinkedBag has faster index-based access  
C) LinkedBag can never become full — it grows dynamically  
D) LinkedBag uses less memory than ArrayBag  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. ArrayBag is actually simpler to implement because arrays are straightforward. LinkedBag requires managing nodes and pointers.
- **B** — Incorrect. LinkedBag has NO index-based access. To find an item, you must traverse the chain node by node. Arrays are much faster for index access.
- **C** — ✅ Correct. LinkedBag stores items as linked nodes — it just creates a new node every time you add something. There is no fixed capacity, so `add()` always returns `true` (unlike ArrayBag which returns `false` when full).
- **D** — Incorrect. This is context-dependent. If the bag is nearly full, LinkedBag and ArrayBag use similar memory. If the bag is sparse, ArrayBag might actually waste less memory because linked nodes carry extra overhead (the `next` pointer in each node).

---

### Question 9 — Interface Design

**Why do we use a Java `interface` (like `BagInterface<T>`) instead of just writing a class directly?**

A) Java requires all ADTs to be written as interfaces  
B) Interfaces make code run faster  
C) An interface allows multiple different implementations (ArrayBag, LinkedBag) to be used interchangeably  
D) Interfaces can store data, unlike classes  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. Java does not require ADTs to be interfaces. This is a design choice.
- **B** — Incorrect. Interfaces have no direct impact on runtime speed.
- **C** — ✅ Correct. By coding against `BagInterface<T>`, client code like `BagInterface<Item> cart = new ArrayBag<>()` can be changed to `BagInterface<Item> cart = new LinkedBag<>()` without changing ANY other code. The client only knows about the interface, not the specific implementation.
- **D** — Incorrect. Interfaces in Java (before Java 8 defaults) cannot store instance data. Classes store data.

---

### Question 10 — Bag vs Set

**A student adds the string "Java" to a Bag 3 times. What does `getFrequencyOf("Java")` return?**

A) 0  
B) 1  
C) 3  
D) It throws an exception  

**✅ Correct Answer: C**

**Explanations:**
- **A** — Incorrect. 0 would mean "Java" was never added, but it was added 3 times.
- **B** — Incorrect. 1 would be the result for a Set (no duplicates), not a Bag. A Bag allows duplicates.
- **C** — ✅ Correct. The Bag allows duplicates. Each `add("Java")` call adds another occurrence. After 3 adds, `getFrequencyOf("Java")` returns 3.
- **D** — Incorrect. `getFrequencyOf` does not throw an exception for this scenario. It simply counts and returns the count.

---

### Question 11 — Code Trace

**Given the following code, what is printed?**

```java
BagInterface<String> myBag = new Bag<>();
myBag.add("cat");
myBag.add("dog");
myBag.add("cat");
System.out.println(myBag.getCurrentSize());
System.out.println(myBag.contains("cat"));
System.out.println(myBag.getFrequencyOf("cat"));
```

A) `2`, `false`, `1`  
B) `3`, `true`, `2`  
C) `3`, `true`, `1`  
D) `2`, `true`, `2`  

**✅ Correct Answer: B**

**Explanations:**
- We added 3 items: "cat", "dog", "cat" → `getCurrentSize()` = **3**
- "cat" is in the bag → `contains("cat")` = **true**
- "cat" was added twice → `getFrequencyOf("cat")` = **2**
- **A** — Wrong (size is 3, not 2; frequency is 2, not 1)
- **B** — ✅ Correct. 3, true, 2
- **C** — Wrong (frequency is 2, not 1)
- **D** — Wrong (size is 3, not 2)

---

## 17. Quick Reference Summary

### ADT Core Concepts Cheat Sheet

```
ADT (Abstract Data Type)
│
├── Defines: WHAT data is stored + WHAT operations exist
├── Does NOT define: HOW data is stored / HOW operations are coded
├── Language-independent
└── Focuses on "what" not "how"

Data Structure
│
├── The actual implementation of an ADT
├── Defines: HOW data is stored + HOW operations run
└── Language-specific (Java, Python, C++, etc.)

Collection  →  The concept (ADT)
Container   →  The class that implements it
```

---

### Bag ADT Cheat Sheet

| Property | Value |
|---|---|
| Order | None |
| Duplicates | Allowed |
| Generic | Yes — `T` type |

| Method | Returns | Purpose |
|---|---|---|
| `getCurrentSize()` | int | Number of items in bag |
| `isEmpty()` | boolean | Is bag empty? |
| `add(T)` | boolean | Add an item |
| `remove()` | T or null | Remove any item |
| `remove(T)` | boolean | Remove a specific item |
| `clear()` | void | Empty the bag |
| `getFrequencyOf(T)` | int | How many times does item appear? |
| `contains(T)` | boolean | Is this item in the bag? |
| `toArray()` | T[] | Get all items as array |

---

### ArrayBag vs LinkedBag Cheat Sheet

| Feature | ArrayBag | LinkedBag |
|---|---|---|
| Internal storage | `T[] bag` (fixed array) | Chain of `Node` objects |
| Max size | Fixed (DEFAULT_CAPACITY = 25) | Unlimited — always grows |
| `add()` can fail? | YES (if full) | NO (always returns true) |
| Implementation complexity | Simple | Moderate |

---

### Key Vocabulary

| Term | Definition |
|---|---|
| **ADT** | Abstract Data Type — a conceptual model of data + operations |
| **Data Structure** | The actual implementation of an ADT |
| **Collection** | An ADT that stores a group of objects |
| **Container** | A class that implements a collection |
| **Bag** | An unordered ADT that allows duplicates |
| **Set** | Like a Bag but no duplicates allowed |
| **Interface** | Java keyword for declaring ADT specifications |
| **Generic (T)** | A placeholder type that makes a class work with any data type |
| **UML** | Unified Modeling Language — visual notation for class design |
| **CRC Card** | Class-Responsibility-Collaboration design card |
| **Information Hiding** | Keeping implementation details private from clients |
| **Abstraction** | Focusing on WHAT something does, ignoring HOW |

---

*End of Notes — WIA 1002 ADT & Bag*  
*Prepared for exam reference. Study all code examples and practice questions thoroughly.*
