# WIA 1002 Data Structure — Stack: Complete Study Notes
> University of Malaya | Sem 2, 2025/2026  
> Topics: Recap → Introduction → Implementation → Postfix Evaluation

---

## Table of Contents
1. [Recap: Java Collection Framework](#1-recap-java-collection-framework)
2. [Recap: Lists — Array vs Linked List](#2-recap-lists--array-vs-linked-list)
3. [Introduction to Stack](#3-introduction-to-stack)
4. [Stack Methods: push, pop, peek](#4-stack-methods-push-pop-peek)
5. [Implementing a Stack](#5-implementing-a-stack)
6. [GenericStack Class (Full Code)](#6-genericstack-class-full-code)
7. [Postfix Expressions (RPN)](#7-postfix-expressions-rpn)
8. [Postfix Evaluation Algorithm](#8-postfix-evaluation-algorithm)
9. [Postfix Error Handling](#9-postfix-error-handling)
10. [PostfixEvaluation.java (Full Code)](#10-postfixevaluationjava-full-code)
11. [Practice Questions with Full Explanations](#11-practice-questions-with-full-explanations)
12. [Quick Reference Cheat Sheet](#12-quick-reference-cheat-sheet)

---

## 1. Recap: Java Collection Framework

### What is the Java Collection Framework?

It is a **set of classes and interfaces** built into Java that helps you store and manage groups of objects (data). Think of it like a toolbox — each tool is designed for a specific job.

### The Hierarchy (Top → Down)

```
Iterable  (Interface — can be looped through)
    └── Collection  (Interface — root of all collections)
            ├── List  (Interface — ordered, allows duplicates)
            │       ├── ArrayList   [Class]
            │       ├── LinkedList  [Class]
            │       └── Vector      [Class]
            │               └── Stack  [Class]   ← OUR FOCUS TODAY
            │
            ├── Queue  (Interface — first in, first out)
            │       ├── PriorityQueue  [Class]
            │       └── Deque          (Interface)
            │               └── ArrayDeque  [Class]
            │
            └── Set  (Interface — no duplicates)
                    ├── HashSet        [Class]
                    ├── LinkedHashSet  [Class]
                    └── SortedSet      (Interface)
                            └── TreeSet  [Class]
```

> **Key Point:** Stack in Java inherits from `Vector`, which inherits from `List`. This means Stack IS a List, but with restrictions — you can only touch the top.

### The Collection Interface — Key Methods

| Method | What it does |
|---|---|
| `add(o)` | Adds element `o` to the collection |
| `remove(o)` | Removes element `o` |
| `contains(o)` | Returns `true` if `o` is in the collection |
| `size()` | Returns the count of elements |
| `isEmpty()` | Returns `true` if empty |
| `clear()` | Removes everything |
| `iterator()` | Returns an iterator to loop through |

The `Iterator` interface methods:
- `hasNext()` → Is there a next element?
- `next()` → Get the next element
- `remove()` → Remove the last element returned

---

## 2. Recap: Lists — Array vs Linked List

### What is a List?

A List is an **Abstract Data Type (ADT)** that stores data in **sequential order**. Think of it like a numbered line of lockers. Common operations:

- Retrieve an element
- Insert a new element
- Delete an element
- Find how many elements exist
- Check if it is empty
- Check if a specific element is in the list

### Two Ways to Implement a List

#### Way 1 — Using an Array

```
Index:  [0]   [1]   [2]   [3]   [4]   ... [N-1]
Data:  [Cat] [Dog] [Rat] [   ] [   ] ...  [   ]
```

- The array is created dynamically
- If the array gets full, a new BIGGER array is created and all data is copied over
- **Fast for:** Reading/writing at a known position (`get(index)`, `set(index, value)`, `add` at the end)
- **Slow for:** Inserting or deleting in the middle — every element after that point needs to shift

#### Way 2 — Using a Linked List

```
[Cat] ──→ [Dog] ──→ [Rat] ──→ [null]
 Node       Node      Node
```

- Each "node" is a box containing: the data + a pointer to the next node
- Nodes are created dynamically (one at a time, as needed)
- **Fast for:** Inserting or deleting anywhere — just rearrange pointers
- **Slow for:** Random access — you must walk through from the start

### When to Use Which?

| Use Case | Best Choice | Why |
|---|---|---|
| Frequent random access (`get(5)`) | Array | Direct indexing = O(1) |
| Add/remove at the END | Array | Just update last position |
| Add/remove in the MIDDLE | Linked List | No shifting needed |
| Unknown final size | Linked List | Grows node by node |

---

## 3. Introduction to Stack

### What is a Stack?

A Stack is a **special type of List** where:
- You can **only access, insert, or delete from ONE end** — called the **TOP**
- It follows the **LIFO** principle: **Last In, First Out**

### Real World Analogy

Think of a **stack of plates** in a cafeteria:
- You put new plates on the TOP (push)
- You take plates from the TOP (pop)
- You can peek at the TOP plate without removing it (peek)
- You CANNOT grab a plate from the bottom without removing all the ones above it

Another analogy from the slides: a **vegetable skewer** (satay stick):
- Vegetables are threaded from the top
- You remove them from the top in the reverse order they were added

### LIFO Visualised Step by Step

```
Start     Push A    Push B    Push C    Pop C     Pop B     Push D
           
 ┌───┐     ┌───┐     ┌───┐     ┌───┐     ┌───┐     ┌───┐     ┌───┐
 │   │     │ A │     │ B │←top │ C │←top │   │     │   │     │ D │←top
 │   │     └───┘     │ A │     │ B │     │ B │←top │   │     │ A │
 │   │     ↑push     └───┘     │ A │     │ A │     │ A │←top └───┘
 └───┘               ↑push     └───┘     └───┘     └───┘     ↑push
(empty)                         ↑push     ↑pop C    ↑pop B
```

> **Result after all operations:** Stack contains [A (bottom), D (top)]

---

## 4. Stack Methods: push, pop, peek

| Method | What it does | Returns | Requires |
|---|---|---|---|
| `push(element)` | Adds element to the TOP | `void` | Nothing special |
| `pop()` | Removes AND returns the TOP element | The removed element | Stack must NOT be empty |
| `peek()` | Returns the TOP element WITHOUT removing it | The top element | Stack must NOT be empty |
| `isEmpty()` | Checks if stack has no elements | `boolean` | — |
| `getSize()` | Returns number of elements | `int` | — |

### Critical Rule for pop() and peek()
If the stack is **empty** and you call `pop()` or `peek()`, Java throws an `EmptyStackException`:

```java
if (isEmpty()) {
    throw new EmptyStackException();
}
```

Always check `isEmpty()` before calling these methods if you are unsure!

### Code Demonstration

```java
GenericStack<String> stack = new GenericStack<>();

stack.push("Tom");       // Stack: [Tom]        size=1
stack.push("Susan");     // Stack: [Tom, Susan]  size=2
stack.push("Kim");       // Stack: [Tom, Susan, Kim]  size=3
stack.push("Michael");   // Stack: [Tom, Susan, Kim, Michael]  size=4

System.out.println(stack.pop());   // Prints: Michael   Stack: [Tom, Susan, Kim]
System.out.println(stack.pop());   // Prints: Kim        Stack: [Tom, Susan]
System.out.println(stack.peek());  // Prints: Susan      Stack: [Tom, Susan] (unchanged!)
System.out.println(stack.getSize()); // Prints: 2
```

---

## 5. Implementing a Stack

### Why Use an ArrayList (Array-based)?

For a stack, all operations happen at ONE end — the top. In an ArrayList, the "top" is mapped to the **LAST position (index = size - 1)**. This means:
- `push` = `list.add(element)` → adds to the end → **O(1) — very fast**
- `pop` = `list.remove(lastIndex)` → removes from the end → **O(1) — very fast**
- `peek` = `list.get(lastIndex)` → reads the end → **O(1) — very fast**

This is why ArrayList is **preferred over LinkedList** for Stack implementation.

### Visual: Stack as an ArrayList

```
CONCEPTUAL VIEW          ARRAYLIST INTERNAL STORAGE
(Stack — vertical)       (Array — horizontal)

 top →  ┌───┐           [0]   [1]   [2]   ← back = top
        │ C │            A  |  B  |  C  |
        │ B │           ↑               ↑
        │ A │          index 0         index 2 (size-1)
        └───┘           = bottom        = top
       bottom
```

### Two Design Approaches

#### Approach 1 — Inheritance
```
ArrayList ◄──── GenericStack
```
GenericStack EXTENDS ArrayList. It inherits all ArrayList methods, but restricts how they are used.

**Problem:** The user can still call ArrayList methods like `add(0, element)` to insert in the middle — which violates stack rules!

#### Approach 2 — Composition (Recommended ✅)
```
GenericStack ◇──── ArrayList
```
GenericStack CONTAINS an ArrayList as a private field. It only exposes the Stack-appropriate methods (push, pop, peek, etc.).

This is **safer and cleaner** because the internal ArrayList is hidden — the user cannot bypass the stack rules.

```
GenericStack<E>
───────────────────────────────────────
- list: java.util.ArrayList<E>         ← private! hidden inside
───────────────────────────────────────
+ GenericStack()                       ← creates empty stack
+ getSize(): int                       ← how many elements
+ peek(): E                            ← see top (no remove)
+ pop(): E                             ← remove and return top
+ push(o: E): void                     ← add to top
+ isEmpty(): boolean                   ← is it empty?
───────────────────────────────────────
```

---

## 6. GenericStack Class (Full Code)

```java
public class GenericStack<E> {
    // The ArrayList stores our stack data internally
    // It is PRIVATE — users cannot access it directly
    private java.util.ArrayList<E> list = new java.util.ArrayList<>();

    // Returns how many elements are in the stack
    public int getSize() {
        return list.size();
    }

    // Returns the top element WITHOUT removing it
    // Top = last element in the ArrayList
    public E peek() {
        return list.get(getSize() - 1);
    }

    // Adds a new element to the top of the stack
    // list.add() adds to the end of ArrayList = top of stack
    public void push(E o) {
        list.add(o);
    }

    // Removes and returns the top element
    // Step 1: save the top element
    // Step 2: remove it from the ArrayList
    // Step 3: return the saved element
    public E pop() {
        E o = list.get(getSize() - 1);
        list.remove(getSize() - 1);
        return o;
    }

    // Returns true if no elements in the stack
    public boolean isEmpty() {
        return list.isEmpty();
    }

    // A nice way to print the stack (used with System.out.println)
    @Override
    public String toString() {
        return "stack: " + list.toString();
    }
}
```

### How each method maps to ArrayList operations

| Stack Method | ArrayList Operation | Index |
|---|---|---|
| `push(o)` | `list.add(o)` | Adds to end |
| `pop()` | `list.remove(size-1)` | Removes last |
| `peek()` | `list.get(size-1)` | Reads last |
| `getSize()` | `list.size()` | — |
| `isEmpty()` | `list.isEmpty()` | — |

### Safe Version with Exception Handling

Always add this check inside `peek()` and `pop()` in production code:

```java
public E peek() {
    if (isEmpty()) {
        throw new java.util.EmptyStackException();
    }
    return list.get(getSize() - 1);
}

public E pop() {
    if (isEmpty()) {
        throw new java.util.EmptyStackException();
    }
    E o = list.get(getSize() - 1);
    list.remove(getSize() - 1);
    return o;
}
```

---

## 7. Postfix Expressions (RPN)

### The Problem with Normal Math Notation (Infix)

In normal math, the operator sits **between** the operands:

```
3 + 4
```

This is called **infix notation**. The problem is that computers need **rules** to figure out which operation to do first:
1. Operator precedence (`*` before `+`)
2. Parentheses to override precedence
3. Left-to-right evaluation

These rules add complexity. **Postfix notation solves this.**

### What is Postfix (Reverse Polish Notation — RPN)?

In postfix, the operator comes **AFTER** its two operands:

```
3 4 +       ← means: 3 + 4 = 7
```

**Advantages:**
- No need for precedence rules
- No need for parentheses
- Easier for computers to evaluate (uses a stack)
- Reduces memory access

### Converting Infix to Postfix — Examples

| Infix Expression | Postfix (RPN) | Explanation |
|---|---|---|
| `a + b` | `a b +` | Simple: just move `+` to end |
| `a + b * c` | `a b c * +` | `*` has higher precedence, so `b * c` groups first |
| `(a + b) * c` | `a b + c *` | Parentheses force `a + b` to compute first |
| `(a*b + c) / d + e` | `a b * c + d / e +` | Work inside-out: `a*b` → add `c` → divide by `d` → add `e` |
| `2 + 3` | `2 3 +` | Result = 5 |
| `4 + 3 * 5` | `4 3 5 * +` | `3*5=15`, then `4+15=19` |

### Step-by-step: Why `a + b * c` becomes `a b c * +`

In infix: `a + b * c`  
Precedence says `*` happens first:  
→ First compute `b * c`, call it X  
→ Then compute `a + X`  

In postfix, write operands in the order you encounter them, and the operator IMMEDIATELY after its two operands:
- `b * c` → write `b`, then `c`, then `*` → gives `b c *`
- Then `a + (b c *)` → write `a`, then the already-computed group `b c *`, then `+`
- Final: `a b c * +`

---

## 8. Postfix Evaluation Algorithm

### The Rule (3 Steps)

```
Scan the postfix expression left-to-right:
  
  Step 1: If the token is a NUMBER → PUSH it onto the stack
  
  Step 2: If the token is an OPERATOR → 
              pop the top element  → call it x (second operand)
              pop the next element → call it y (first operand)
              compute (y OPERATOR x)
              push the RESULT back onto the stack
  
  Step 3: When expression ends → POP the stack → that's your FINAL ANSWER
```

> **WARNING:** Order matters! The FIRST pop gives `x` (second operand), the SECOND pop gives `y` (first operand). So the operation is `y OPERATOR x`, NOT `x OPERATOR y`.

### Example 1: Evaluate `4 3 5 * +`

Expected result: `4 + 3 * 5 = 4 + 15 = 19`

```
Token: 4     → it's a number → PUSH
Stack: [4]

Token: 3     → it's a number → PUSH  
Stack: [4, 3]

Token: 5     → it's a number → PUSH
Stack: [4, 3, 5]

Token: *     → it's an operator →
                  pop x = 5
                  pop y = 3
                  compute: 3 * 5 = 15
                  PUSH 15
Stack: [4, 15]

Token: +     → it's an operator →
                  pop x = 15
                  pop y = 4
                  compute: 4 + 15 = 19
                  PUSH 19
Stack: [19]

End of expression → POP → ANSWER = 19 ✓
```

### Example 2: Evaluate `2 3 + 4 * 5 -`

This means: `(2 + 3) * 4 - 5 = 5 * 4 - 5 = 20 - 5 = 15`

```
Token: 2   → PUSH    Stack: [2]
Token: 3   → PUSH    Stack: [2, 3]
Token: +   → pop x=3, pop y=2, push (2+3)=5    Stack: [5]
Token: 4   → PUSH    Stack: [5, 4]
Token: *   → pop x=4, pop y=5, push (5*4)=20   Stack: [20]
Token: 5   → PUSH    Stack: [20, 5]
Token: -   → pop x=5, pop y=20, push (20-5)=15  Stack: [15]

ANSWER = 15 ✓
```

### Example 3: Evaluate `2 3 * 4 2 - / 5 6 * +`

This means: `(2*3) / (4-2) + (5*6) = 6/2 + 30 = 3 + 30 = 33`

```
Token: 2   → PUSH    Stack: [2]
Token: 3   → PUSH    Stack: [2, 3]
Token: *   → pop x=3, pop y=2, push 6         Stack: [6]
Token: 4   → PUSH    Stack: [6, 4]
Token: 2   → PUSH    Stack: [6, 4, 2]
Token: -   → pop x=2, pop y=4, push (4-2)=2   Stack: [6, 2]
Token: /   → pop x=2, pop y=6, push (6/2)=3   Stack: [3]
Token: 5   → PUSH    Stack: [3, 5]
Token: 6   → PUSH    Stack: [3, 5, 6]
Token: *   → pop x=6, pop y=5, push (5*6)=30  Stack: [3, 30]
Token: +   → pop x=30, pop y=3, push (3+30)=33 Stack: [33]

ANSWER = 33 ✓
```

### Example 4: Evaluate `2 4 - 3 ^ 5 +`

This means: `(2-4)^3 + 5 = (-2)^3 + 5 = -8 + 5 = -3`

```
Token: 2   → PUSH    Stack: [2]
Token: 4   → PUSH    Stack: [2, 4]
Token: -   → pop x=4, pop y=2, push (2-4)=-2  Stack: [-2]
Token: 3   → PUSH    Stack: [-2, 3]
Token: ^   → pop x=3, pop y=-2, push (-2)^3=-8  Stack: [-8]
Token: 5   → PUSH    Stack: [-8, 5]
Token: +   → pop x=5, pop y=-8, push (-8+5)=-3  Stack: [-3]

ANSWER = -3 ✓
```

---

## 9. Postfix Error Handling

The stack naturally tells you **when something is wrong** with the expression.

### Error Type 1 — Too Many Operators

**Example:** `3 8 + * 9`  
Problem: The `*` operator needs TWO operands, but after evaluating `3 8 +` there is only ONE element (11) on the stack. `*` can't find its second operand.

```
Token: 3   → PUSH    Stack: [3]
Token: 8   → PUSH    Stack: [3, 8]
Token: +   → pop x=8, pop y=3, push 11  Stack: [11]

Token: *   → try to pop x → gets 11
             try to pop y → STACK IS EMPTY → ERROR!
```

**Detection:** When an operator tries to pop but the stack is empty (or has only one element when it needs two).

### Error Type 2 — Too Many Operands

**Example:** `9 8 + 7`  
Problem: The `7` is pushed after evaluation is done. At the end, the stack has MORE than one element — meaning some numbers were never used.

```
Token: 9   → PUSH    Stack: [9]
Token: 8   → PUSH    Stack: [9, 8]
Token: +   → pop x=8, pop y=9, push 17  Stack: [17]
Token: 7   → PUSH    Stack: [17, 7]

End of expression → Stack has [17, 7] → MORE THAN ONE ELEMENT → ERROR!
```

**Detection:** After processing the entire expression, if the stack has **more than one element**, there were too many operands.

### Summary of Error Detection

| When does the error occur? | What error? |
|---|---|
| During evaluation — can't pop enough elements | Too many operators |
| After full evaluation — stack has >1 element | Too many operands |

---

## 10. PostfixEvaluation.java (Full Code)

### Main Test Class

```java
public class PostfixEvaluation {
    public static void main(String[] args) {
        System.out.println("Testing PostfixEvaluation:\n");
        
        // Test case 1: 2 + 3 * 4 - 5 = 2 + 12 - 5 = 9... 
        // But postfix "2 3 + 4 * 5 -" means (2+3)*4-5 = 15
        System.out.println("2 3 + 4 * 5 - : " 
            + evaluatePostfix("2 3 + 4 * 5 -") + "\n");
        
        // Test case 2: (2*3)/(4-2) + 5*6 = 6/2 + 30 = 33
        System.out.println("2 3 * 4 2 - / 5 6 * + : " 
            + PostfixEvaluation.evaluatePostfix("2 3 * 4 2 - / 5 6 * +") + "\n");
        
        // Test case 3: (2-4)^3 + 5 = -8 + 5 = -3
        System.out.println("2 4 - 3 ^ 5 + : " 
            + PostfixEvaluation.evaluatePostfix("2 4 - 3 ^ 5 +") + "\n");
        
        System.out.println("\nDone.");
    }
}
```

### Core Evaluation Method

```java
/**
 * Evaluates a postfix expression.
 * @param postfix  a string that is a valid postfix expression (tokens separated by spaces)
 * @return         the value of the postfix expression as a double
 */
public static double evaluatePostfix(String postfix) {
    // Create a stack to hold operands (numbers)
    GenericStack<Double> valueStack = new GenericStack<>();
    
    // Split the expression into individual tokens by space
    // e.g. "4 3 5 * +" → ["4", "3", "5", "*", "+"]
    String[] tokens = postfix.split(" ");
    
    for (String token : tokens) {
        if (isNumeric(token)) {
            // It's a number → push it
            valueStack.push(new Double(token));
        }
        else if (token.equals("+") || token.equals("-") || 
                 token.equals("*") || token.equals("/") || 
                 token.equals("^")) {
            // It's an operator → pop two operands and compute
            Double operandTwo = valueStack.pop();  // x (SECOND, popped first)
            Double operandOne = valueStack.pop();  // y (FIRST, popped second)
            Double result = compute(operandOne, operandTwo, token);
            valueStack.push(result);
        }
    }
    
    // The final answer is the only element left in the stack
    return (valueStack.peek());
}
```

### Helper: isNumeric

```java
public static boolean isNumeric(String str) {
    try {
        // Try to parse it as a decimal number
        double d = Double.parseDouble(str);
    }
    catch (NumberFormatException nfe) {
        return false;  // Not a number → it must be an operator
    }
    return true;
}
```

### Helper: compute

```java
private static Double compute(Double operandOne, Double operandTwo, String operator) {
    double result;
    
    switch (operator) {
        case "+":
            result = operandOne + operandTwo;
            break;
        case "-":
            result = operandOne - operandTwo;  // IMPORTANT: operandOne - operandTwo (not reversed!)
            break;
        case "*":
            result = operandOne * operandTwo;
            break;
        case "/":
            result = operandOne / operandTwo;
            break;
        case "^":
            result = Math.pow(operandOne, operandTwo);  // Uses Java's Math.pow
            break;
        default:  // Unknown character
            result = 0;
            break;
    }
    
    return result;
}
```

> **Why operandOne - operandTwo and NOT operandTwo - operandOne?**  
> Because we pop `x` first (operandTwo) and then `y` (operandOne). But the ORIGINAL expression had `y - x`. So we compute `operandOne - operandTwo` which correctly gives `y - x`.  
> Example: for `5 3 -`, we want `5 - 3 = 2`. We pop x=3 first (operandTwo), then y=5 (operandOne). Result = `operandOne - operandTwo = 5 - 3 = 2`. ✓

---

## 11. Practice Questions with Full Explanations

---

### Question 1 — Stack Trace (Checkpoint)

> Draw the stack after these operations:
> 1. Push A, 2. Push B, 3. Push C, 4. Pop, 5. Pop, 6. Push D

**Answer:**

```
After Push A:     After Push B:     After Push C:
┌───┐             ┌───┐             ┌───┐
│ A │←top         │ B │←top         │ C │←top
└───┘             │ A │             │ B │
                  └───┘             │ A │
                                    └───┘

After Pop (removes C):   After Pop (removes B):   After Push D:
┌───┐                    ┌───┐                     ┌───┐
│ B │←top                │ A │←top                 │ D │←top
│ A │                    └───┘                     │ A │
└───┘                                              └───┘
```

**Final stack (bottom to top):** A, D

---

### Question 2 — Multiple Choice

> What does `pop()` do?

**(A)** Adds an element to the top of the stack  
**(B)** Removes and returns the top element  
**(C)** Returns the top element without removing it  
**(D)** Removes the bottom element

**Answer: B**

- **(A) Wrong** — That is `push()`. `push` ADDS, `pop` REMOVES.
- **(B) Correct** — `pop()` removes the top element AND returns it to you. After calling `pop()`, the next element below becomes the new top.
- **(C) Wrong** — That describes `peek()`. `peek()` only looks, it does not remove anything.
- **(D) Wrong** — Stacks NEVER access the bottom. That is the fundamental rule. Bottom = inaccessible.

---

### Question 3 — Multiple Choice

> What does LIFO mean?

**(A)** Last In, First Over  
**(B)** Last Item, First Object  
**(C)** Last In, First Out  
**(D)** Linear Input, Fixed Output

**Answer: C**

- **(A) Wrong** — "First Over" is not a real computer science term. Nonsense option.
- **(B) Wrong** — "Last Item, First Object" — Object vs Out is the distraction. Also not a real term.
- **(C) Correct** — LIFO = Last In, First Out. The LAST element you pushed in is the FIRST one you get when you pop. Like a stack of pancakes — you eat from the top.
- **(D) Wrong** — Completely made up. "Linear Input, Fixed Output" has nothing to do with stacks.

---

### Question 4 — Multiple Choice

> Which implementation is preferred for a stack and why?

**(A)** LinkedList, because stacks need dynamic memory  
**(B)** ArrayList, because all stack operations occur at the end  
**(C)** Array (fixed size), because stacks never grow  
**(D)** LinkedList, because insertion in the middle is fast

**Answer: B**

- **(A) Misleading** — LinkedList does allow dynamic memory, but that's not the key reason for stack preference. LinkedList adds overhead per node (pointer storage).
- **(B) Correct** — ArrayList is preferred because stack operations (push, pop, peek) all happen at the END of the list, and ArrayList's `add()`/`remove(lastIndex)` are O(1) — very fast. LinkedList's end operations are also fast but have pointer overhead.
- **(C) Wrong** — Stacks DO grow dynamically. A fixed-size array would cause `StackOverflowError` when full.
- **(D) Wrong** — Stack NEVER inserts in the middle. Insertion-in-middle speed is irrelevant for a stack.

---

### Question 5 — Multiple Choice

> What is the output of this code?

```java
GenericStack<Integer> s = new GenericStack<>();
s.push(10);
s.push(20);
s.push(30);
System.out.println(s.pop());
System.out.println(s.peek());
System.out.println(s.getSize());
```

**(A)** 10, 20, 1  
**(B)** 30, 20, 2  
**(C)** 30, 20, 1  
**(D)** 30, 10, 2

**Answer: B**

Step-by-step trace:

```
push(10)    → Stack: [10]
push(20)    → Stack: [10, 20]
push(30)    → Stack: [10, 20, 30]
pop()       → removes 30, returns 30 → Stack: [10, 20]  → Prints: 30
peek()      → returns 20 (new top), does NOT remove → Stack: [10, 20]  → Prints: 20
getSize()   → Stack still has 10 and 20 → size = 2  → Prints: 2
```

- **(A) Wrong** — 10 is the BOTTOM element, not the top. Stack is LIFO.
- **(B) Correct** — As traced above.
- **(C) Wrong** — After `pop()` and `peek()`, the stack still has [10, 20] = size 2, NOT 1. `peek()` doesn't remove.
- **(D) Wrong** — 10 is at the bottom, `peek()` returns the TOP which is 20 after the pop.

---

### Question 6 — Postfix Conversion

> Convert `(a + b) * (c - d)` to postfix.

**Step-by-step reasoning:**

1. There are two groups in parentheses: `(a + b)` and `(c - d)`
2. Each group has two operands and one operator
3. For `(a + b)` in postfix: `a b +` (operator goes after both operands)
4. For `(c - d)` in postfix: `c d -`
5. These two groups are then multiplied: `(group1) * (group2)`
6. In postfix: put both groups, then `*`

**Answer: `a b + c d - *`**

---

### Question 7 — Postfix Evaluation

> Evaluate the postfix expression: `6 2 / 3 + 4 2 ^ *`
> 
> (Note: `^` means power/exponent)

**Step-by-step:**

```
Token: 6   → PUSH    Stack: [6]
Token: 2   → PUSH    Stack: [6, 2]
Token: /   → pop x=2, pop y=6, push (6/2)=3    Stack: [3]
Token: 3   → PUSH    Stack: [3, 3]
Token: +   → pop x=3, pop y=3, push (3+3)=6    Stack: [6]
Token: 4   → PUSH    Stack: [6, 4]
Token: 2   → PUSH    Stack: [6, 4, 2]
Token: ^   → pop x=2, pop y=4, push (4^2)=16   Stack: [6, 16]
Token: *   → pop x=16, pop y=6, push (6*16)=96 Stack: [96]

ANSWER = 96
```

**Verification:** Infix equivalent is `(6/2 + 3) * 4^2 = (3+3) * 16 = 6 * 16 = 96` ✓

---

### Question 8 — Error Detection

> Which of the following postfix expressions has an error, and what kind?

**(A)** `5 3 + 2 *` — expected result = (5+3)*2 = 16  
**(B)** `5 3 + *` — ?  
**(C)** `5 3 2 + *` — expected result = 5*(3+2) = 25  
**(D)** `5 3 + 2 * 4` — ?

**Answers:**

- **(A) Valid** — `5 3 +` → 8, then `8 2 *` → 16. Stack ends with [16]. ✓
- **(B) Error: Too many operators** — `5 3 +` → 8. Stack: [8]. Then `*` tries to pop TWO operands but only finds ONE (8). Stack is empty on second pop → EmptyStackException.
- **(C) Valid** — `5` pushed, `3 2 +` → 5, then `5 5 *` → 25. ✓
- **(D) Error: Too many operands** — `5 3 + 2 *` → 16. Then `4` is pushed. End of expression. Stack = [16, 4] → more than one element → error.

---

### Question 9 — Implementation Detail

> In `GenericStack`, why is `pop()` implemented as:
> ```java
> E o = list.get(getSize() - 1);
> list.remove(getSize() - 1);
> return o;
> ```
> instead of just:
> ```java
> return list.remove(getSize() - 1);
> ```

**(A)** Both are equivalent; the first version is just clearer  
**(B)** `list.remove()` returns the index, not the element  
**(C)** The element must be saved before the list shrinks, in case an exception occurs  
**(D)** `list.get()` is faster than `list.remove()`

**Answer: A** (though C has a valid software engineering reasoning)

- **(A) Mostly correct** — In practice, `list.remove(index)` on an `ArrayList<E>` returns the removed element of type `E`, so both versions produce the same result. The verbose version is written for clarity.
- **(B) Wrong** — `ArrayList.remove(int index)` returns the element that was removed, NOT the index.
- **(C) Partially valid** — It's a good defensive programming practice to save first, but not strictly necessary here since `ArrayList.remove()` is atomic.
- **(D) Wrong** — Both `get()` and `remove()` are O(1) for the last element. Speed is not the reason.

---

### Question 10 — Inheritance vs Composition

> Why is composition preferred over inheritance when implementing GenericStack?

**(A)** Composition is always faster at runtime  
**(B)** Inheritance exposes all ArrayList methods to the user, breaking encapsulation  
**(C)** Composition allows the stack to grow beyond ArrayList's capacity  
**(D)** Java does not support inheriting from ArrayList

**Answer: B**

- **(A) Wrong** — There is no significant runtime speed difference between the two approaches for this case.
- **(B) Correct** — If `GenericStack extends ArrayList`, then a user can call `stack.add(0, element)` or `stack.set(2, value)` — bypassing the LIFO restriction entirely. With composition, `list` is `private`, so only the methods YOU define are accessible. The stack rule is enforced.
- **(C) Wrong** — Both approaches use ArrayList internally and benefit from ArrayList's dynamic resizing.
- **(D) Wrong** — Java absolutely allows you to extend ArrayList. The issue is about good design, not Java limitations.

---

## 12. Quick Reference Cheat Sheet

### Stack Basics

```
LIFO = Last In, First Out
Top = the accessible end
Bottom = the inaccessible end (you cannot touch it directly)
```

### Stack Methods (in one line each)

```java
push(x)     → Add x to top
pop()       → Remove + return top (throws exception if empty)
peek()      → Return top without removing (throws exception if empty)
isEmpty()   → true if no elements
getSize()   → number of elements
```

### Postfix Evaluation (quick algorithm)

```
For each token left-to-right:
  NUMBER → push
  OPERATOR (+ - * / ^) → pop x, pop y, push (y OP x)
End → pop = answer
```

### Postfix Error Detection

```
Too many operators → stack empty when trying to pop for an operator
Too many operands  → stack has >1 element after full evaluation
```

### Infix to Postfix Quick Rules

```
No parentheses: operands keep their order, operators move to AFTER their two operands
Parentheses: solve the inner group first, that group's operator goes after the group
Precedence: higher-precedence operators bind tighter (write their result first)
```

### Common Postfix Conversions

```
a + b             →  a b +
a - b             →  a b -
a + b * c         →  a b c * +      (* binds b and c first)
(a + b) * c       →  a b + c *      (parentheses force a+b first)
a * b + c * d     →  a b * c d * +  (two separate * groups, then +)
(a + b) * (c - d) →  a b + c d - *
```

---

> **Study Tip:** The most exam-common questions are:
> 1. Tracing a series of push/pop operations and drawing the resulting stack
> 2. Evaluating a given postfix expression step-by-step
> 3. Converting an infix expression to postfix
> 4. Spotting errors in a postfix expression
>
> Practice each of these manually — do NOT rely on your calculator or code to do it for you.
> 
> Good luck! 🎯
