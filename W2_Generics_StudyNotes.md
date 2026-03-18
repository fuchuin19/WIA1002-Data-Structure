# WIA 1002 Data Structure — Generics: Complete Study Notes

> **Course:** WIA 1002 Data Structure | Universiti Malaya  
> **Topic:** Generics (Chapter 19)  
> **Reference:** Liang, *Introduction to Java Programming*, 10th Edition

---

## Table of Contents

1. [What is Generic?](#1-what-is-generic)
2. [Why Use Generics?](#2-why-use-generics)
3. [The ArrayList Class](#3-the-arraylist-class)
4. [Array vs ArrayList](#4-array-vs-arraylist)
5. [Naming Conventions for Type Parameters](#5-naming-conventions-for-type-parameters)
6. [Generic Types — Rules & Constraints](#6-generic-types--rules--constraints)
7. [Declaring Generic Classes](#7-declaring-generic-classes)
8. [Declaring Generic Interfaces](#8-declaring-generic-interfaces)
9. [Generic Methods](#9-generic-methods)
10. [Bounded Generic Types](#10-bounded-generic-types)
11. [Raw Types and Backward Compatibility](#11-raw-types-and-backward-compatibility)
12. [Wildcard Generic Types](#12-wildcard-generic-types)
13. [Type Erasure and Compile-Time Checking](#13-type-erasure-and-compile-time-checking)
14. [Restrictions on Generics](#14-restrictions-on-generics)
15. [Practice Questions with Full Explanations](#15-practice-questions-with-full-explanations)

---

## 1. What is Generic?

### Simple Definition

**Generic** is Java's capability to **parameterize types**. Instead of hardcoding a specific type (like `String` or `Integer`) into a class or method, you use a **placeholder** (like `T`, `E`, `K`) that gets replaced by a real type when the class or method is actually used.

Think of it like a **template** or a **mould** — the mould's shape is fixed, but you can pour in different materials (types).

### Formal Definition

> A **generic** class or method can work with objects of various types while providing **compile-time type safety**.

### Analogy

Imagine a **box**. Without generics, you have a box that can hold *anything*, but you never know what's inside — you might expect an apple and get a hammer. With generics, you label the box: **"This box holds ONLY apples"**. Now if someone tries to put a hammer in, you're warned immediately.

### Key Idea: Values vs Types

| Regular Parameters (Methods) | Type Parameters (Generics) |
|---|---|
| Inputs are **values** | Inputs are **types** |
| `void print(int x)` — x is a value | `class Box<T>` — T is a type |
| Called with `print(5)` | Used as `Box<String>` |

### Quick Example

```java
// WITHOUT generics — accepts anything, no safety
public class Box {
    private Object item;

    public void store(Object a) {
        this.item = a;
    }

    public Object retrieve() {
        return item;
    }
}

// WITH generics — type-safe
public class GenericBox<T> {
    private T item;

    public void store(T a) {
        this.item = a;
    }

    public T retrieve() {
        return item;
    }
}
```

Usage:
```java
GenericBox<String> box1 = new GenericBox<>();
box1.store("Hello World");
String s = box1.retrieve(); // No casting needed, guaranteed String!

GenericBox<Integer> box2 = new GenericBox<>();
box2.store(100);
Integer n = box2.retrieve(); // Guaranteed Integer!

// This would cause a COMPILE ERROR — caught early!
// box1.store(100); // Error: 100 is not a String
```

---

## 2. Why Use Generics?

### Problem Without Generics

Without generics, to make a class work with different types, you'd use `Object` (the parent of all Java classes). But this leads to **runtime errors** that are very hard to debug.

```java
// Non-generic Box using Comparable (the old approach)
public class Box {
    private Comparable item;

    public void store(Comparable a) { this.item = a; }
    public Comparable retrieve() { return item; }
}

// Usage — DANGEROUS
Box box1 = new Box();
Box box2 = new Box();

box1.store("Hello World");  // stores a String
box2.store(100);            // stores an Integer

// This COMPILES fine, but CRASHES at RUNTIME!
int c = box1.retrieve().compareTo(box2.retrieve());
// RuntimeError: Cannot compare String with Integer!
```

**The problem:** The compiler has no idea that box1 holds a String and box2 holds an Integer. The bug only appears when the program actually runs — which could be in production.

### Benefits of Generics

#### Benefit 1: Stronger Type Checks at Compile Time

With generics, the **compiler** catches type mismatches **before** the program runs.

```java
GenericBox<String> box1 = new GenericBox<>();
GenericBox<Integer> box2 = new GenericBox<>();

box1.store("Hello World");
box2.store(100);

// COMPILE ERROR — caught immediately, before running!
// int c = box1.retrieve().compareTo(box2.retrieve());
// Error: method compareTo(Integer) cannot be applied to String
```

This is far better — you fix the bug at your desk, not in production.

#### Benefit 2: Elimination of Explicit Casting

Without generics, every time you retrieve something from a collection, you must manually cast it. This is clunky and error-prone.

```java
// WITHOUT generics — manual casting required
ArrayList list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);  // Must cast explicitly

// WITH generics — no casting needed
ArrayList<String> list = new ArrayList<>();
list.add("hello");
String s = list.get(0);  // Compiler knows it's a String!
```

#### Benefit 3: Code Reusability

Write a class or method **once**, use it with **any type**:

```java
// One generic stack works for ALL types
GenericStack<String> stringStack = new GenericStack<>();
GenericStack<Integer> intStack = new GenericStack<>();
GenericStack<Double> doubleStack = new GenericStack<>();
```

---

## 3. The ArrayList Class

### What is ArrayList?

`ArrayList` is Java's built-in **dynamic array** — it grows and shrinks automatically as you add or remove elements. Unlike a regular array whose size is **fixed** at creation, an ArrayList can hold an **unlimited** number of objects.

```java
// Regular array — size FIXED at 10, cannot grow
String[] arr = new String[10];

// ArrayList — size grows AUTOMATICALLY
ArrayList<String> list = new ArrayList<>();
```

### ArrayList is a Generic Class

`ArrayList` itself is defined as:

```java
public class ArrayList<E> { ... }
```

Here `E` stands for **Element** — a placeholder for whatever type you want to store. When you use it, you replace `E` with a real type:

```java
ArrayList<String>  cities   = new ArrayList<>();  // E = String
ArrayList<Integer> numbers  = new ArrayList<>();  // E = Integer
ArrayList<Double>  prices   = new ArrayList<>();  // E = Double
```

Both of these are equivalent (diamond operator `<>` infers the type):
```java
ArrayList<String> cities = new ArrayList<String>();  // explicit
ArrayList<String> cities = new ArrayList<>();        // using diamond operator (preferred)
```

### Full ArrayList API Reference

| Method Signature | What It Does | Example |
|---|---|---|
| `ArrayList()` | Creates an empty list | `new ArrayList<>()` |
| `add(E o)` | Appends element to **end** | `list.add("London")` |
| `add(int index, E o)` | Inserts at **specific position** | `list.add(2, "Paris")` |
| `get(int index)` | Returns element at index | `list.get(0)` → `"London"` |
| `set(int index, E o)` | **Replaces** element at index | `list.set(0, "Tokyo")` |
| `remove(int index)` | Removes by **position** | `list.remove(1)` |
| `remove(Object o)` | Removes by **value** | `list.remove("Paris")` |
| `size()` | Returns **count** of elements | `list.size()` → `3` |
| `contains(Object o)` | Returns `true` if element exists | `list.contains("Rome")` |
| `indexOf(Object o)` | Returns index of **first** match | `list.indexOf("Paris")` |
| `lastIndexOf(Object o)` | Returns index of **last** match | `list.lastIndexOf("Paris")` |
| `isEmpty()` | Returns `true` if no elements | `list.isEmpty()` |
| `clear()` | **Removes all** elements | `list.clear()` |

### Complete ArrayList Example

```java
import java.util.ArrayList;

public class TestArrayList {
    public static void main(String[] args) {

        // Create an ArrayList of Strings
        ArrayList<String> cityList = new ArrayList<>();

        // Add elements — appended to the end
        cityList.add("London");   // index 0
        cityList.add("Denver");   // index 1
        cityList.add("Paris");    // index 2
        cityList.add("Miami");    // index 3
        cityList.add("Seoul");    // index 4
        cityList.add("Tokyo");    // index 5

        // Query methods
        System.out.println("List size: " + cityList.size());            // 6
        System.out.println("Contains Miami? " + cityList.contains("Miami")); // true
        System.out.println("Index of Denver: " + cityList.indexOf("Denver")); // 1
        System.out.println("Is empty? " + cityList.isEmpty());          // false

        // Insert at a specific position
        cityList.add(2, "Xian");
        // List is now: [London, Denver, Xian, Paris, Miami, Seoul, Tokyo]

        // Remove by value
        cityList.remove("Miami");
        // List is now: [London, Denver, Xian, Paris, Seoul, Tokyo]

        // Remove by index (removes element at index 1 = "Denver")
        cityList.remove(1);
        // List is now: [London, Xian, Paris, Seoul, Tokyo]

        // Print list (toString)
        System.out.println(cityList.toString());
        // Output: [London, Xian, Paris, Seoul, Tokyo]

        // Traverse in REVERSE using a for loop
        for (int i = cityList.size() - 1; i >= 0; i--) {
            System.out.print(cityList.get(i) + " ");
        }
        // Output: Tokyo Seoul Paris Xian London

        System.out.println();

        // ArrayList also works with custom objects!
        ArrayList<Circle> circleList = new ArrayList<>();
        circleList.add(new Circle(2));
        circleList.add(new Circle(3));

        System.out.println("Area of circle 0: " + circleList.get(0).getArea());
        // Output: 12.566370614359172
    }
}
```

---

## 4. Array vs ArrayList

This comparison is crucial for understanding when to use each:

| Operation | Regular Array | ArrayList |
|---|---|---|
| **Declaration** | `String[] a = new String[10]` | `ArrayList<String> list = new ArrayList<>()` |
| **Access element** | `a[index]` | `list.get(index)` |
| **Update element** | `a[index] = "London"` | `list.set(index, "London")` |
| **Get size** | `a.length` | `list.size()` |
| **Add new element** | ❌ Not possible | `list.add("London")` |
| **Insert at position** | ❌ Not possible | `list.add(index, "London")` |
| **Remove by index** | ❌ Not possible | `list.remove(index)` |
| **Remove by value** | ❌ Not possible | `list.remove(Object)` |
| **Remove all** | ❌ Not possible | `list.clear()` |
| **Size is fixed?** | ✅ Yes — fixed at creation | ❌ No — grows dynamically |
| **Stores primitives?** | ✅ Yes (`int[]`, `double[]`) | ❌ No — only reference types |

### Key Takeaway

Use a **regular array** when the size is known and fixed. Use an **ArrayList** when you need to add/remove elements dynamically.

---

## 5. Naming Conventions for Type Parameters

When writing generic code, type parameters should be **single uppercase letters**. This is an agreed-upon Java convention — without it, it would be impossible to tell whether `E` is a type parameter or a class name.

| Type Parameter | Conventional Meaning | Typical Usage |
|---|---|---|
| `E` | **E**lement | Collections: `List<E>`, `ArrayList<E>` |
| `K` | **K**ey | Maps: `Map<K, V>` |
| `V` | **V**alue | Maps: `Map<K, V>` |
| `N` | **N**umber | Numeric types |
| `T` | **T**ype | General-purpose placeholder |
| `S`, `U`, `V` | 2nd, 3rd, 4th type params | When multiple are needed |

### Examples

```java
// E for Element in a stack
public class Stack<E> { ... }

// T for general Type in a box
public class Box<T> { ... }

// K and V for Key-Value pairs (like a dictionary)
public class Pair<K, V> {
    private K key;
    private V value;
}

// Multiple type parameters
public class Triple<A, B, C> {
    private A first;
    private B second;
    private C third;
}
```

---

## 6. Generic Types — Rules & Constraints

### Rule 1: Only Reference Types Allowed (No Primitives)

The type parameter `E` must be a **reference type** (objects). Primitive types (`int`, `double`, `char`, `boolean`, etc.) are NOT allowed.

```java
ArrayList<Integer> list1 = new ArrayList<>();  // ✅ Integer (reference type)
ArrayList<Double>  list2 = new ArrayList<>();  // ✅ Double  (reference type)
ArrayList<String>  list3 = new ArrayList<>();  // ✅ String  (reference type)

ArrayList<int>    list4 = new ArrayList<>();  // ❌ COMPILE ERROR — int is primitive
ArrayList<double> list5 = new ArrayList<>();  // ❌ COMPILE ERROR — double is primitive
```

**Why?** Because Java generics work with objects in memory, and primitives are not objects — they're stored differently.

**Workaround:** Use wrapper classes:
- `int` → `Integer`
- `double` → `Double`
- `char` → `Character`
- `boolean` → `Boolean`
- `long` → `Long`

Java automatically converts between primitives and their wrappers (**autoboxing/unboxing**):

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(5);      // autoboxing: int 5 → Integer(5) automatically
int x = numbers.get(0); // unboxing: Integer(5) → int 5 automatically
```

### Rule 2: Type Safety is Enforced

Once you specify a type, only that type (or its subtypes) can be used:

```java
ArrayList<String> list = new ArrayList<>();
list.add("Red");           // ✅ String — OK
list.add("Blue");          // ✅ String — OK
list.add(new Integer(1));  // ❌ COMPILE ERROR — Integer is not a String!
```

---

## 7. Declaring Generic Classes

### Syntax

```java
public class ClassName<E> {
    // use E anywhere inside the class as a type
}
```

You can use any letter (following conventions), not just `E`:

```java
public class Box<T> { ... }
public class Pair<K, V> { ... }
public class Triple<A, B, C> { ... }
```

### Full Example: GenericStack

```java
public class GenericStack<E> {
    private ArrayList<E> list = new ArrayList<>();

    // Push: add to top
    public void push(E o) {
        list.add(o);
    }

    // Pop: remove from top
    public E pop() {
        E o = list.get(getSize() - 1);
        list.remove(getSize() - 1);
        return o;
    }

    // Peek: look at top without removing
    public E peek() {
        return list.get(getSize() - 1);
    }

    public boolean isEmpty() {
        return list.isEmpty();
    }

    public int getSize() {
        return list.size();
    }

    @Override
    public String toString() {
        return "stack: " + list.toString();
    }
}
```

Usage:
```java
// A stack that holds Strings
GenericStack<String> stringStack = new GenericStack<>();
stringStack.push("First");
stringStack.push("Second");
stringStack.push("Third");
System.out.println(stringStack.peek());  // Third
System.out.println(stringStack.pop());   // Third (removes it)

// A stack that holds Integers
GenericStack<Integer> intStack = new GenericStack<>();
intStack.push(10);
intStack.push(20);
intStack.push(30);
System.out.println(intStack.pop()); // 30
```

### Full Example: GenericBox (from lecture)

```java
public class GenericBox<T> {
    private T item;
    boolean full;

    public GenericBox() {
        full = false;
    }

    public void store(T a) {
        this.item = a;
        full = true;
    }

    public void remove() {
        item = null;
        full = false;
    }

    @Override
    public String toString() {
        if (full)
            return item.toString();
        else
            return "nothing";
    }
}
```

```java
public class UseGenericBox {
    public static void main(String[] args) {
        GenericBox<String>  box1 = new GenericBox<>();
        GenericBox<Integer> box2 = new GenericBox<>();

        box1.store("Hello World");
        box2.store(100);

        System.out.println("Box 1 has " + box1); // Box 1 has Hello World
        System.out.println("Box 2 has " + box2); // Box 2 has 100

        box1.remove();
        box2.remove();

        System.out.println("After removal, box 1 has " + box1); // After removal, box 1 has nothing
        System.out.println("After removal, box 2 has " + box2); // After removal, box 2 has nothing

        // These lines would cause COMPILE ERRORS — type safety!
        // box1.store(100);           // Error: 100 is not a String
        // box2.store("Hello World"); // Error: String is not an Integer
    }
}
```

### Important Notes About Generic Classes

1. **Constructor does NOT include the type parameter:**
   ```java
   // CORRECT
   public GenericStack() { ... }
   
   // WRONG — don't write GenericStack<E>() as constructor name
   ```

2. **A class can have MULTIPLE type parameters:**
   ```java
   public class Pair<K, V> {
       private K key;
       private V value;
       
       public Pair(K key, V value) {
           this.key = key;
           this.value = value;
       }
   }
   
   // Usage
   Pair<String, Integer> person = new Pair<>("Alice", 30);
   Pair<String, String>  coord  = new Pair<>("lat", "lng");
   ```

3. **A class/interface can be a subtype of a generic class/interface:**
   ```java
   // String implements Comparable<String>
   public class String implements Comparable<String> { ... }
   
   // Integer extends Number and implements Comparable<Integer>
   public class Integer extends Number implements Comparable<Integer> { ... }
   ```

---

## 8. Declaring Generic Interfaces

### Syntax

```java
public interface InterfaceName<E> {
    // abstract methods using E
}
```

### Examples

```java
// Java's built-in Comparable — already generic!
public interface Comparable<E> {
    int compareTo(E o);
}

// Custom generic interface
public interface Edible<E> {
    boolean canEat(E food);
}

// A class implementing a generic interface
public class Rectangle implements Comparable<Rectangle> {
    double width, height;

    @Override
    public int compareTo(Rectangle other) {
        double thisArea  = this.width * this.height;
        double otherArea = other.width * other.height;
        if (thisArea > otherArea) return 1;
        if (thisArea < otherArea) return -1;
        return 0;
    }
}
```

---

## 9. Generic Methods

A **generic method** is a method that introduces its own type parameter — independent of any generic class it may or may not be inside.

### Syntax

```java
// Static generic method
public static <E> returnType methodName(E parameter) { ... }

// Instance generic method
public <E> returnType methodName(E parameter) { ... }
```

**Key point:** The `<E>` appears **before** the return type.

### Simple Example

```java
public class GenericMethodDemo {
    public static void main(String[] args) {
        Integer[] integers = {1, 2, 3, 4, 5};
        String[]  strings  = {"London", "Paris", "New York", "Austin"};

        // Explicit type specification
        GenericMethodDemo.<Integer>print(integers);

        // Type inferred by compiler (more common)
        GenericMethodDemo.<String>print(strings);

        // Even simpler — compiler infers the type automatically
        print(integers);
        print(strings);
    }

    // ONE method that works for ANY array type
    public static <E> void print(E[] list) {
        for (int i = 0; i < list.length; i++) {
            System.out.print(list[i] + " ");
        }
        System.out.println();
    }
}
```

Output:
```
1 2 3 4 5
London Paris New York Austin
```

### More Generic Method Examples

```java
public class GenericUtils {

    // Find max of two comparable items
    public static <E extends Comparable<E>> E max(E a, E b) {
        return a.compareTo(b) > 0 ? a : b;
    }

    // Swap two elements in an array
    public static <E> void swap(E[] arr, int i, int j) {
        E temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    // Check if an array contains a value
    public static <E> boolean contains(E[] arr, E value) {
        for (E item : arr) {
            if (item.equals(value)) return true;
        }
        return false;
    }
}

// Usage
System.out.println(GenericUtils.max("Apple", "Mango"));  // Mango
System.out.println(GenericUtils.max(10, 25));             // 25

String[] fruits = {"Apple", "Banana", "Cherry"};
System.out.println(GenericUtils.contains(fruits, "Banana")); // true

GenericUtils.swap(fruits, 0, 2);
// fruits is now: ["Cherry", "Banana", "Apple"]
```

### How to Call a Generic Method

```java
// Method: public static <E> void print(E[] list)

// Option 1: Specify type explicitly
GenericMethodDemo.<Integer>print(integers);
GenericMethodDemo.<String>print(strings);

// Option 2: Let the compiler infer the type (preferred)
print(integers);   // compiler sees Integer[] → infers E = Integer
print(strings);    // compiler sees String[]  → infers E = String
```

---

## 10. Bounded Generic Types

### The Problem

Sometimes you want to restrict what types can be used with your generic class or method. For example, a method that calculates areas only makes sense for `GeometricObject` subclasses — you don't want someone passing in a `String`.

### Solution: Bounded Type Parameters

```java
// E must be GeometricObject or a subclass of it
<E extends GeometricObject>

// U must be Number or a subclass (Integer, Double, etc.)
<U extends Number>

// T must implement the Comparable interface
<T extends Comparable<T>>
```

**Syntax:** `<E extends UpperBoundType>`

Note: In generics, `extends` is used for both **class inheritance** AND **interface implementation** (unlike regular Java where you'd use `implements`).

### Unbounded vs Bounded

```java
// Unbounded — E can be ANY object type
// Same as: <E extends Object>
public static <E> void method(E item) { ... }

// Bounded — E must be Number or its subclass (Integer, Double, Long...)
public static <E extends Number> void method(E item) { ... }
```

### Bounded Class Example

```java
public class BoundedGeneric<T extends Number> {
    T data;

    public BoundedGeneric(T t) {
        data = t;
    }

    public void display() {
        System.out.println("Value is: " + data);
        System.out.println("  and type is: " + data.getClass().getName());
    }
}
```

```java
BoundedGeneric<Integer> b1 = new BoundedGeneric<>(3);
b1.display();
// Value is: 3
//   and type is: java.lang.Integer

BoundedGeneric<Double> b2 = new BoundedGeneric<>(3.14);
b2.display();
// Value is: 3.14
//   and type is: java.lang.Double

// COMPILE ERROR — String is NOT a subclass of Number
// BoundedGeneric<String> b3 = new BoundedGeneric<>("Hello");
// Error: type argument String is not within bounds of type-variable T
```

### Bounded Method Example

```java
// Compare areas of any two GeometricObjects
public static <E extends GeometricObject> boolean equalArea(E obj1, E obj2) {
    return obj1.getArea() == obj2.getArea();
}

// Usage — only GeometricObject subclasses allowed
Circle    c = new Circle(5);
Rectangle r = new Rectangle(3, 4);
System.out.println(equalArea(c, r)); // compares areas

// COMPILE ERROR — String is not a GeometricObject
// equalArea("hello", "world"); // Error!
```

### Type Erasure with Bounded Types

The compiler replaces the bounded type parameter with the **bound type** itself:

```java
// Before compilation (what you write):
public static <E extends GeometricObject> boolean equalArea(E o1, E o2) {
    return o1.getArea() == o2.getArea();
}

// After type erasure (what actually runs):
public static boolean equalArea(GeometricObject o1, GeometricObject o2) {
    return o1.getArea() == o2.getArea();
}
```

---

## 11. Raw Types and Backward Compatibility

### What is a Raw Type?

A **raw type** is when you use a generic class **without** specifying a type parameter.

```java
// Raw type — no type specified, can hold ANYTHING
ArrayList list = new ArrayList();

// This is roughly equivalent to:
ArrayList<Object> list = new ArrayList<Object>();
```

Raw types exist for **backward compatibility** — Java code written before JDK 1.5 (when generics were introduced) didn't use type parameters. Raw types let that old code still compile.

### Why Raw Types are Dangerous

```java
public class Max {
    // Raw type — o1 and o2 can be ANYTHING
    public static Comparable max(Comparable o1, Comparable o2) {
        if (o1.compareTo(o2) > 0) return o1;
        else return o2;
    }
}

// This COMPILES but CRASHES at runtime!
Max.max("Welcome", 23);
// RuntimeError: String cannot be compared to Integer
// Because "Welcome".compareTo(23) is meaningless
```

The compiler gives a **warning** (unchecked call) but doesn't stop you. The error only shows up when the program runs.

### Making it Safe with Generics

```java
public class Max {
    // E must be Comparable to itself — type-safe!
    public static <E extends Comparable<E>> E max(E o1, E o2) {
        if (o1.compareTo(o2) > 0) return o1;
        else return o2;
    }
}

// Now this is a COMPILE ERROR — caught immediately!
// Max.max("Welcome", 23);
// Error: no suitable method found for max(String, int)
// Because String and Integer are different types

// These work correctly:
System.out.println(Max.max("Apple", "Mango")); // Mango
System.out.println(Max.max(10, 25));           // 25
```

### Best Practice

```java
// ✅ DO THIS — always specify a concrete type
new ArrayList<String>();
new ArrayList<Integer>();

// ❌ AVOID THIS — raw type, unsafe
new ArrayList();
```

---

## 12. Wildcard Generic Types

### The Problem Wildcards Solve

Even though `Integer` is a subtype of `Number`, **`ArrayList<Integer>` is NOT a subtype of `ArrayList<Number>`**. This surprises many people.

```java
public static void display(ArrayList<Number> list) {
    for (int i = 0; i <= 2; i++) {
        System.out.println(list.get(i));
    }
}

ArrayList<Integer> list1 = new ArrayList<>();
list1.add(3); list1.add(6); list1.add(9);

// COMPILE ERROR — even though Integer is a Number!
display(list1);
// Error: ArrayList<Integer> cannot be converted to ArrayList<Number>
```

### Solution: Wildcards `?`

A **wildcard** `?` represents an **unknown type**. It allows the method to accept lists of any type.

```java
// Using wildcard — now accepts ANY ArrayList
public static void display(ArrayList<?> list) {
    for (int i = 0; i <= 2; i++) {
        if (list.get(i).equals(6.0))
            System.out.println("yes");
        else
            System.out.println("no");
    }
}

ArrayList<Integer> list1 = new ArrayList<>();
list1.add(3); list1.add(6); list1.add(9);

ArrayList<Double> list2 = new ArrayList<>();
list2.add(3.0); list2.add(6.0); list2.add(9.0);

display(list1); // Works! Output: no, no, no (6 ≠ 6.0 because Integer ≠ Double)
display(list2); // Works! Output: no, yes, no
```

### Three Forms of Wildcards

#### Form 1: `<?>` — Unbounded Wildcard

**Meaning:** Any type whatsoever.  
**Equivalent to:** `<? extends Object>`

```java
// Accepts ArrayList of ANY type
public static void printList(ArrayList<?> list) {
    for (Object item : list) {
        System.out.print(item + " ");
    }
    System.out.println();
}

printList(new ArrayList<String>());   // ✅
printList(new ArrayList<Integer>());  // ✅
printList(new ArrayList<Double>());   // ✅
```

#### Form 2: `<? extends T>` — Upper Bounded Wildcard

**Meaning:** Any **subtype** of T (T itself or its children).  
**Use case:** You want to READ from a collection of T or its subtypes.

```java
// Accepts ArrayList of Number or any subtype (Integer, Double, Long...)
public static double sumList(ArrayList<? extends Number> list) {
    double sum = 0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    return sum;
}

ArrayList<Integer> ints    = new ArrayList<>(Arrays.asList(1, 2, 3));
ArrayList<Double>  doubles = new ArrayList<>(Arrays.asList(1.1, 2.2, 3.3));

System.out.println(sumList(ints));    // 6.0  ✅
System.out.println(sumList(doubles)); // 6.6  ✅
```

#### Form 3: `<? super T>` — Lower Bounded Wildcard

**Meaning:** Any **supertype** of T (T itself or its parents).  
**Use case:** You want to WRITE/ADD to a collection — the collection must be at least as general as T.

```java
// Accepts ArrayList of Integer or any supertype (Number, Object...)
public static void addNumbers(ArrayList<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

ArrayList<Integer> ints    = new ArrayList<>();
ArrayList<Number>  numbers = new ArrayList<>();
ArrayList<Object>  objects = new ArrayList<>();

addNumbers(ints);    // ✅ Integer is Integer
addNumbers(numbers); // ✅ Integer is a subtype of Number
addNumbers(objects); // ✅ Integer is a subtype of Object
```

### Summary Table

| Wildcard | Name | Accepts | Read? | Write? |
|---|---|---|---|---|
| `<?>` | Unbounded | Any type | ✅ (as Object) | ❌ |
| `<? extends T>` | Upper bounded | T or its subtypes | ✅ (as T) | ❌ |
| `<? super T>` | Lower bounded | T or its supertypes | ⚠️ (as Object) | ✅ |

---

## 13. Type Erasure and Compile-Time Checking

### What is Type Erasure?

Java implements generics using **type erasure**:

1. The compiler **uses** generic type information to check correctness
2. The compiler then **erases** all generic type information
3. At **runtime**, there is NO generic type information — the JVM only sees raw types

This design decision makes generic code **backward-compatible** with pre-generics Java code.

### How Erasure Works: Unbounded Types → Object

```java
// What YOU write (before compilation):
public static <E> void print(E[] list) {
    for (int i = 0; i < list.length; i++) {
        System.out.print(list[i] + " ");
    }
    System.out.println();
}

// What actually RUNS (after type erasure):
public static void print(Object[] list) {
    for (int i = 0; i < list.length; i++) {
        System.out.print(list[i] + " ");
    }
    System.out.println();
}
```

### How Erasure Works: Bounded Types → Bound Type

```java
// What YOU write:
public static <E extends GeometricObject> boolean equalArea(E o1, E o2) {
    return o1.getArea() == o2.getArea();
}

// What actually RUNS (E replaced by GeometricObject, the bound):
public static boolean equalArea(GeometricObject o1, GeometricObject o2) {
    return o1.getArea() == o2.getArea();
}
```

### Compile-Time vs Runtime Casting

The compiler automatically inserts casts where needed:

```java
// What YOU write:
ArrayList<String> list = new ArrayList<>();
list.add("Oklahoma");
String state = list.get(0);  // No cast needed in your code!

// What actually RUNS (compiler adds the cast):
ArrayList list = new ArrayList();
list.add("Oklahoma");
String state = (String) list.get(0);  // Cast inserted by compiler
```

### One Class in the JVM

Because of type erasure, **there is only ONE `ArrayList` class** loaded into the JVM regardless of how many different generic versions you use:

```java
ArrayList<String>  list1 = new ArrayList<>();
ArrayList<Integer> list2 = new ArrayList<>();

// Both are instances of the same ArrayList class!
System.out.println(list1 instanceof ArrayList); // true
System.out.println(list2 instanceof ArrayList); // true

// They are the SAME class at runtime
System.out.println(list1.getClass() == list2.getClass()); // true
```

---

## 14. Restrictions on Generics

Because of type erasure (generic info is gone at runtime), there are **4 important restrictions**.

### Restriction 1: Cannot Use `new E()`

You **cannot instantiate** a generic type parameter:

```java
public class Container<E> {
    private E item;

    public void createItem() {
        item = new E(); // ❌ COMPILE ERROR!
        // Error: Cannot instantiate the type E
    }
}
```

**Why?** `new E()` is executed at **runtime**, but `E` is erased — the JVM doesn't know what type E is at runtime.

**Workaround:** Pass a pre-created object in:
```java
public void setItem(E item) {
    this.item = item; // ✅ OK — object already exists
}
```

### Restriction 2: Cannot Use `new E[]`

You **cannot create an array** using a generic type parameter:

```java
public class GenericList<E> {
    // ❌ COMPILE ERROR — cannot create generic array
    private E[] data = new E[100];
}
```

**Why?** Same reason — array creation (`new E[100]`) happens at runtime, but `E` is erased.

**Workaround:** Cast from Object array (with unchecked warning):

```java
public class GenericList<E> {
    @SuppressWarnings("unchecked")
    private E[] data = (E[]) new Object[100]; // ✅ Works but gives warning
}
```

**Important caveat:** This workaround can cause a `ClassCastException` at runtime if misused. For example, if `E = String`, casting an `Object[]` to `String[]` is fine — but if you later assign that array to a `String[]` variable, and the array actually contains Integers, it will crash.

### Restriction 3: Generic Type Parameter NOT Allowed in Static Context

The class-level type parameter cannot be used in `static` methods, `static` fields, or `static` blocks:

```java
public class Test<E> {
    // ❌ ILLEGAL — static method using class-level E
    public static void m(E o1) { ... }

    // ❌ ILLEGAL — static field using class-level E
    public static E o1;

    // ❌ ILLEGAL — static block using class-level E
    static {
        E o2;
    }
}
```

**Why?** Static members belong to the **class**, not to any instance. But the generic type `E` is associated with a **specific instance** (`new Test<String>()` vs `new Test<Integer>()`). Static members exist before any instance is created, so they can't know what `E` is.

**Workaround:** Use a separate type parameter in a static generic method:

```java
public class Test<E> {
    // ✅ LEGAL — declares its OWN type parameter <T>
    public static <T> void m(T o1) { ... }
}
```

### Restriction 4: Exception Classes Cannot Be Generic

You cannot define a custom exception that is generic:

```java
// ❌ ILLEGAL — generic exception class
public class MyException<T> extends Exception {
    private T info;
    // ...
}

// ❌ This catch clause would also be illegal
try {
    // ...
} catch (MyException<T> ex) {  // Cannot use parameterized type in catch
    // ...
}
```

**Why?** When you `catch` an exception, Java checks the **runtime type**. But generic type info is erased at runtime — so Java cannot distinguish `MyException<String>` from `MyException<Integer>` at runtime.

### Quick Reference: Restrictions

| Restriction | What's NOT Allowed | Why |
|---|---|---|
| 1 | `new E()` | E is erased at runtime |
| 2 | `new E[100]` | E is erased at runtime |
| 3 | `static E field` or `static method(E x)` | Static context exists before instance creation |
| 4 | `class MyEx<T> extends Exception` | Cannot catch by parameterized type at runtime |

---

## 15. Practice Questions with Full Explanations

---

### Question Set 1: Basics

**Q1.** What is the primary advantage of using generic types in Java?

- A) They make the program run faster at runtime
- B) They provide stronger type checks at compile time and eliminate the need for casting
- C) They allow the use of primitive types like `int` and `double` directly in collections
- D) They allow a class to inherit from multiple classes

**✅ Correct Answer: B**

**Explanation of each option:**

- **A — Incorrect.** Generics do NOT improve runtime performance. In fact, type erasure means generics compile down to the same bytecode as raw types. The benefit is developer safety, not speed.
- **B — Correct.** The two main benefits are: (1) type mismatches are caught at **compile time** before the program runs, and (2) you no longer need to manually cast retrieved objects.
- **C — Incorrect.** This is actually a **restriction** of generics — primitive types are NOT allowed. You must use wrapper types (`Integer`, `Double`, etc.).
- **D — Incorrect.** Java does not support multiple inheritance through generics. Multiple inheritance of interfaces is a separate concept.

---

**Q2.** Which of the following statements correctly creates a generic ArrayList?

```java
// (a)
ArrayList<int> list = new ArrayList<>();

// (b)
ArrayList<Integer> list = new ArrayList<>();

// (c)
ArrayList<String> list = new ArrayList<String>();

// (d)
ArrayList list = new ArrayList();
```

**✅ Correct Answers: B and C**

**Explanation:**

- **A — Incorrect.** `int` is a **primitive type**. Generic type parameters must be reference types. Use `Integer` instead.
- **B — Correct.** `Integer` is a reference type (wrapper class for `int`). This is the proper way to store integers in an ArrayList.
- **C — Correct.** Fully explicit form — specifying `String` on both sides. This is valid but verbose. Modern Java prefers `new ArrayList<>()` (diamond operator).
- **D — Incorrect (but compiles).** This is a **raw type** — no type parameter specified. It compiles with a warning, but it's unsafe because no type checking is enforced.

---

### Question Set 2: Compile Errors

**Q3.** For the code below, is there a compile error?

```java
// (a) Prior to JDK 1.5
ArrayList dates = new ArrayList();
dates.add(new Date());
dates.add(new String());

// (b) Since JDK 1.5
ArrayList<Date> dates = new ArrayList<>();
dates.add(new Date());
dates.add(new String());
```

**Answer: (a) No error. (b) Compile error.**

**Explanation:**

- **A — No compile error.** This uses a **raw type** ArrayList (no type parameter). Raw types accept anything — `Date`, `String`, `Integer` — there's no type checking. It will compile without errors. However, it's unsafe and may cause runtime issues.
- **B — Compile error.** This uses `ArrayList<Date>` — a typed ArrayList that only accepts `Date` objects. When `dates.add(new String())` is called, the compiler sees that `String` is NOT a `Date`, and **refuses to compile** the code. Error: "argument mismatch; String cannot be converted to Date".

---

**Q4.** What is wrong with code (a) below? Is code (b) correct?

```java
// (a) Prior to JDK 1.5
ArrayList dates = new ArrayList();
dates.add(new Date());
Date date = dates.get(0);   // Problem here!

// (b) Since JDK 1.5
ArrayList<Date> dates = new ArrayList<>();
dates.add(new Date());
Date date = dates.get(0);   // Is this OK?
```

**Answer: (a) needs explicit cast. (b) is correct, no cast needed.**

**Explanation:**

- **A — Compile error without cast.** Raw type `ArrayList` stores everything as `Object`. So `dates.get(0)` returns type `Object`, NOT `Date`. You **must** cast: `Date date = (Date) dates.get(0)`. Without the cast, the compiler reports: "incompatible types: Object cannot be converted to Date".
- **B — Correct.** Because the ArrayList is typed as `ArrayList<Date>`, the compiler **knows** that `dates.get(0)` returns a `Date`. No casting is needed, and the code compiles cleanly.

---

### Question Set 3: Generic Classes & Interfaces

**Q5.** Which of the following correctly declares a generic class?

- A) `public class MyClass { }`
- B) `public class MyClass<E> { }`
- C) `public class MyClass<> { }`
- D) `public class <E> MyClass { }`

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** This declares a normal (non-generic) class. No type parameter.
- **B — Correct.** The type parameter `<E>` is placed **after the class name**. This is the correct syntax for a generic class.
- **C — Incorrect.** `<>` (diamond operator) is used when CREATING instances (`new ArrayList<>()`), NOT when declaring a class.
- **D — Incorrect.** The `<E>` must come **after** the class name, not before it.

---

**Q6.** Can a generic class have multiple type parameters?

- A) No, only one type parameter is allowed
- B) Yes, using `<E1, E2, E3>` separated by commas
- C) Yes, but only using `T` and `E` as names
- D) Only interfaces can have multiple type parameters

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** Java generics support multiple type parameters. For example, `Map<K, V>` uses two.
- **B — Correct.** You can have as many type parameters as needed, separated by commas inside `<>`. Example: `public class Triple<A, B, C> { ... }`
- **C — Incorrect.** You can use ANY letter or meaningful short name for type parameters, not just `T` and `E`. The convention is to use uppercase letters, but the names are flexible.
- **D — Incorrect.** Both classes AND interfaces support multiple type parameters.

---

### Question Set 4: Generic Methods

**Q7.** How do you declare a static generic method named `myMethod` that returns `void` and accepts one parameter called `a`?

- A) `public static void myMethod(E a) { }`
- B) `public static <E> void myMethod(E a) { }`
- C) `public <E> static void myMethod(E a) { }`
- D) `public static void <E> myMethod(E a) { }`

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** `E` is used but never declared. The compiler doesn't know what `E` is. This would cause: "cannot find symbol: class E".
- **B — Correct.** `<E>` is declared **before the return type** `void`. This tells the compiler "this method introduces a type parameter E". Full signature: `public static <E> void myMethod(E a)`.
- **C — Incorrect.** The `<E>` declaration must come **before** the return type, not after `static`. The order of modifiers matters.
- **D — Incorrect.** `<E>` cannot go between the return type and the method name. It must be before the return type.

---

**Q8.** What does this code print?

```java
public class Demo {
    public static <E> void print(E[] list) {
        for (int i = 0; i < list.length; i++) {
            System.out.print(list[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] nums = {10, 20, 30};
        String[] words = {"Hi", "Bye"};
        print(nums);
        print(words);
    }
}
```

- A) Compile error — can't pass Integer[] and String[] to same method
- B) `10 20 30` then `Hi Bye`
- C) `[10, 20, 30]` then `[Hi, Bye]`
- D) Runtime error — type mismatch

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** This is exactly WHY generic methods exist — one method can handle multiple types. The compiler infers `E = Integer` for the first call and `E = String` for the second call.
- **B — Correct.** The generic method prints each element followed by a space, then a newline. Output: `10 20 30 ` then `Hi Bye `.
- **C — Incorrect.** The method uses `list[i]` which gives individual elements, not `list.toString()` which would give `[10, 20, 30]`.
- **D — Incorrect.** There is no type mismatch. Generics handle this cleanly with compile-time type checking.

---

### Question Set 5: Bounded Types

**Q9.** What is a bounded generic type?

- A) A generic type that can only store exactly one element
- B) A generic type specified as a subtype of another type
- C) A generic type that has a maximum size limit
- D) A generic type that can only be used in bounded loops

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** "Bounded" in generics has nothing to do with storing one element. It's about restricting which types are allowed.
- **B — Correct.** A bounded generic type constrains E to be a specific type or one of its subtypes. For example, `<E extends Number>` means E can be `Number`, `Integer`, `Double`, `Long`, etc., but NOT `String` or `Object`.
- **C — Incorrect.** Bounded types are about **type constraints**, not size or capacity.
- **D — Incorrect.** "Bounded" here is a type-theory term, not a loop-related term.

---

**Q10.** Given `public class BoundedGeneric<T extends Number>`, which of the following will cause a compile error?

- A) `BoundedGeneric<Integer> b = new BoundedGeneric<>(3);`
- B) `BoundedGeneric<Double> b = new BoundedGeneric<>(3.14);`
- C) `BoundedGeneric<Number> b = new BoundedGeneric<>(5);`
- D) `BoundedGeneric<String> b = new BoundedGeneric<>("Hello");`

**✅ Correct Answer: D**

**Explanation:**

- **A — No error.** `Integer` is a subclass of `Number`. ✅
- **B — No error.** `Double` is a subclass of `Number`. ✅
- **C — No error.** `Number` itself satisfies `extends Number`. ✅
- **D — Compile error.** `String` does NOT extend `Number`. It's a subclass of `Object`, but not `Number`. The constraint `<T extends Number>` prohibits `String`. Error: "type argument String is not within bounds of type-variable T".

---

### Question Set 6: Raw Types & Wildcards

**Q11.** What does the `?` wildcard represent in generics?

- A) Any primitive type
- B) An unknown reference type
- C) A null value
- D) A wildcard that only matches String types

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** Wildcards represent reference types. Primitive types cannot be used with generics at all.
- **B — Correct.** `?` represents an **unknown** reference type. `ArrayList<?>` means "an ArrayList of some unknown type". It accepts `ArrayList<String>`, `ArrayList<Integer>`, `ArrayList<Double>`, etc.
- **C — Incorrect.** Wildcards have nothing to do with `null` values. `null` is a special reference value, not a type.
- **D — Incorrect.** A wildcard matches ANY type, not just `String`.

---

**Q12.** What is the difference between `<? extends Number>` and `<? super Number>`?

- A) `<? extends Number>` accepts supertypes; `<? super Number>` accepts subtypes
- B) `<? extends Number>` accepts Number or its subtypes; `<? super Number>` accepts Number or its supertypes
- C) They are equivalent — both accept the same types
- D) `<? extends Number>` is for reading; `<? super Number>` is for writing but they accept the same types

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** This has the definitions swapped.
- **B — Correct.** `<? extends Number>` = upper bound → `Number`, `Integer`, `Double`, `Long`, etc. (Number or anything below it in the hierarchy). `<? super Number>` = lower bound → `Number`, `Object` (Number or anything above it).
- **C — Incorrect.** They accept completely different sets of types.
- **D — Incorrect.** While the "Producer Extends, Consumer Super" (PECS) principle does relate to reading/writing, the two wildcards accept **different** types. The statement "they accept the same types" is wrong.

---

### Question Set 7: Erasure & Restrictions

**Q13.** If a program uses `ArrayList<String>` and `ArrayList<Date>`, how many ArrayList classes does the JVM load?

- A) Two — one for String and one for Date
- B) One — only the raw ArrayList class
- C) Zero — generic classes are inlined at compile time
- D) Depends on how many objects are created

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** Due to **type erasure**, all the type information (`<String>`, `<Date>`) is removed at compile time. At runtime, both `ArrayList<String>` and `ArrayList<Date>` are treated as plain `ArrayList`.
- **B — Correct.** The JVM loads exactly ONE `ArrayList` class. Both `ArrayList<String>` and `ArrayList<Date>` are the same `ArrayList` class at runtime. You can verify: `new ArrayList<String>().getClass() == new ArrayList<Date>().getClass()` is `true`.
- **C — Incorrect.** Generic classes are not inlined. They compile to normal bytecode with the raw type.
- **D — Incorrect.** The number of objects created is irrelevant. There's always one class loaded regardless.

---

**Q14.** Which of the following is NOT a valid restriction on generics?

- A) You cannot instantiate a generic type with `new E()`
- B) You cannot create a generic array with `new E[10]`
- C) You cannot use a generic type parameter in a static context
- D) You cannot use generics with interfaces

**✅ Correct Answer: D**

**Explanation:**

- **A — Valid restriction.** `new E()` is illegal because `E` is not available at runtime (erased). This is restriction 1.
- **B — Valid restriction.** `new E[10]` is illegal for the same reason. This is restriction 2.
- **C — Valid restriction.** Class-level generic parameters like `E` in `class Foo<E>` cannot be used in static methods or fields. This is restriction 3.
- **D — NOT a restriction.** You absolutely CAN use generics with interfaces. Examples include: `Comparable<E>`, `List<E>`, `Map<K,V>`. Declaring and implementing generic interfaces is completely valid.

---

**Q15.** Why can exception classes not be generic?

- A) Because exception classes are final and cannot be extended
- B) Because the type parameter information is not available at runtime, making it impossible to match a specific generic exception in a catch block
- C) Because exceptions must always use raw types for compatibility
- D) Because generics are only for data structures, not exceptions

**✅ Correct Answer: B**

**Explanation:**

- **A — Incorrect.** `Exception` is not final — you can extend it. The issue is with using generics, not with inheritance.
- **B — Correct.** When Java encounters a `catch (MyException<String> ex)` block, it needs to check at **runtime** whether the thrown exception matches `MyException<String>`. But due to type erasure, `MyException<String>` and `MyException<Integer>` are both just `MyException` at runtime — Java cannot tell them apart, making specific catching impossible.
- **C — Incorrect.** Raw types exist for backward compatibility, not as a rule for exceptions specifically.
- **D — Incorrect.** Generics can be used in many contexts beyond data structures — methods, classes, and interfaces — but exceptions are specifically excluded due to the runtime type erasure issue.

---

## Summary Cheat Sheet

```
┌─────────────────────────────────────────────────────────────┐
│                    JAVA GENERICS SUMMARY                     │
├─────────────────────────────────────────────────────────────┤
│ DEFINITION: Parameterize types to write reusable,           │
│             type-safe code                                   │
├─────────────────────────────────────────────────────────────┤
│ BENEFITS:                                                    │
│  ✅ Stronger type checks at compile time                    │
│  ✅ No explicit casting needed                              │
│  ✅ Reusable code for multiple types                        │
├─────────────────────────────────────────────────────────────┤
│ SYNTAX:                                                      │
│  Class:      public class Name<E> { }                       │
│  Interface:  public interface Name<E> { }                   │
│  Method:     public static <E> void method(E param) { }     │
│  Bounded:    <E extends SomeType>                           │
│  Wildcard:   <?>, <? extends T>, <? super T>                │
├─────────────────────────────────────────────────────────────┤
│ TYPE PARAMETER CONVENTIONS:                                  │
│  E = Element   K = Key     V = Value                        │
│  N = Number    T = Type    S,U,V = 2nd, 3rd, 4th params     │
├─────────────────────────────────────────────────────────────┤
│ RULES:                                                       │
│  ❌ No primitives (int, double) — use Integer, Double       │
│  ❌ No new E() — can't instantiate generic type             │
│  ❌ No new E[] — can't create generic array                 │
│  ❌ No static use of class-level type params                │
│  ❌ No generic exception classes                            │
├─────────────────────────────────────────────────────────────┤
│ TYPE ERASURE:                                                │
│  • Compiler uses generics to check correctness              │
│  • Generic info erased AFTER compilation                    │
│  • At runtime: <E> → Object, <E extends T> → T             │
│  • Only ONE class loaded per generic class (e.g. ArrayList) │
└─────────────────────────────────────────────────────────────┘
```

---

*End of Study Notes — WIA 1002 Generics*  
*References: Liang, Introduction to Java Programming, 10th Ed. | Oracle Java Docs*
