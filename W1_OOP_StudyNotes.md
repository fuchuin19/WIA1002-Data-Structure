# WIA1002 Data Structure — OOP Revision Study Notes
### Universiti Malaya | Sem 2, Session 2025/2026

> **Complete Study Notes** covering all 4 parts of the OOP Revision lecture.  
> Includes explanations, code examples, and MCQ practice questions with detailed answer explanations.

---

## Table of Contents

1. [Part 1: Objects and Classes](#part-1-objects-and-classes)
   - [What is an Object?](#what-is-an-object)
   - [What is a Class?](#what-is-a-class)
   - [UML Class Diagrams](#uml-class-diagrams)
   - [Constructors](#constructors)
   - [Default Constructor](#default-constructor)
   - [Object Reference Variables](#object-reference-variables)
   - [Primitive vs Object Types](#primitive-vs-object-types)
   - [Visibility Modifiers](#visibility-modifiers)
   - [Why Make Data Fields Private?](#why-make-data-fields-private)

2. [Part 2: Thinking in Objects](#part-2-thinking-in-objects)
   - [Class Abstraction and Encapsulation](#class-abstraction-and-encapsulation)

3. [Part 3: Inheritance and Polymorphism](#part-3-inheritance-and-polymorphism)
   - [Inheritance](#inheritance)
   - [Superclasses and Subclasses](#superclasses-and-subclasses)
   - [Calling Superclass Constructor](#calling-superclass-constructor)
   - [Superclass Constructor is Always Invoked](#superclass-constructor-is-always-invoked)
   - [Overriding vs Overloading](#overriding-vs-overloading)
   - [Polymorphism](#polymorphism)
   - [Dynamic Binding](#dynamic-binding)
   - [Visibility/Accessibility Modifiers Summary](#visibilityaccessibility-modifiers-summary)

4. [Part 4: Abstract Classes and Interfaces](#part-4-abstract-classes-and-interfaces)
   - [Abstract Classes](#abstract-classes)
   - [6 Key Rules About Abstract Classes](#6-key-rules-about-abstract-classes)
   - [Interfaces](#interfaces)
   - [Defining an Interface](#defining-an-interface)
   - [Implementing an Interface](#implementing-an-interface)
   - [Extend vs Implement](#extend-vs-implement)
   - [Interface Inheritance](#interface-inheritance)
   - [Omitting Modifiers in Interfaces](#omitting-modifiers-in-interfaces)
   - [Interfaces vs Abstract Classes](#interfaces-vs-abstract-classes)

5. [Practice MCQs with Explanations](#practice-mcqs-with-explanations)

---

## Part 1: Objects and Classes

### What is an Object?

An **object** is a software representation of a real-world entity that can be distinctly identified. Think of real things: a car, a chair, a student, a bank account. Every object has three core properties:

| Property | What it means | Example (Car) |
|---|---|---|
| **Identity** | Each object is unique | VIN number / license plate |
| **State** | Set of data fields with current values | colour="red", speed=80, fuel=40 |
| **Behaviour** | Set of methods (what it can do) | accelerate(), brake(), refuel() |

> **Simple analogy:** Think of your phone. Its **state** includes battery level, screen brightness, and installed apps. Its **behaviour** includes making calls, taking photos, and sending messages. Every phone is a unique **object** even if two phones are the same model.

---

### What is a Class?

A **class** is a blueprint or template used to create objects. Just like an architectural blueprint defines what a house will look like, a class defines what an object will contain and how it will behave. You can create **many objects** from one class.

```
Class  (Blueprint)   →   Objects (Actual houses built from the blueprint)
-----------------         ------------------------------------------
Circle class         →   circle1 (radius=10), circle2 (radius=25), circle3 (radius=125)
Student class        →   student1 (Ali), student2 (Siti), student3 (Kumar)
```

**Key Points:**
- A class is a *construct* that defines objects of the same type.
- It is a *template*, *blueprint*, or *contract* defining data fields and methods.
- An **object** is an **instance** of a class.
- You can create as many objects/instances from a single class as needed.

---

### UML Class Diagrams

UML (Unified Modeling Language) diagrams are visual tools used to represent classes and their relationships. Here is how to read and write one:

```
+----------------------+
|       Circle         |   ← Class name (top section)
+----------------------+
|  radius: double      |   ← Data fields (middle section)
+----------------------+
|  Circle()            |   ← Constructors and methods (bottom section)
|  Circle(r: double)   |
|  getArea(): double   |
+----------------------+
```

**Notation used in UML:**
- `+` before a member means **public**
- `-` before a member means **private**
- `#` before a member means **protected**
- Underlined member names mean **static**
- *Italicised* method names mean **abstract**

**UML Object notation** (when showing a specific instance):
```
+------------------+
|  circle1: Circle |   ← objectName: ClassName (underlined = it's an object)
+------------------+
|  radius = 1.0    |   ← actual values stored
+------------------+
```

#### Question from lecture: Declare the class from the UML diagram

```java
class Circle {
    // Data field
    double radius = 1.0;

    // No-arg constructor
    Circle() {
    }

    // Constructor with parameter
    Circle(double newRadius) {
        radius = newRadius;
    }

    // Method
    double getArea() {
        return radius * radius * 3.14159;
    }
}
```

---

### Constructors

A **constructor** is a special type of method that is called automatically when you create an object using the `new` keyword. Its job is to **initialise** the object's data fields.

```java
Circle()  {                        // no-arg constructor
}

Circle(double newRadius) {         // parameterised constructor
    radius = newRadius;
}
```

**Rules about constructors (very important for exams!):**

| Rule | Explanation |
|---|---|
| Same name as the class | `class Circle` → constructor must be `Circle()` |
| No return type | Not even `void`. If you write `void Circle()`, it becomes a regular method, NOT a constructor! |
| Invoked with `new` | `Circle c = new Circle();` |
| Multiple constructors allowed | This is called *constructor overloading* |
| Purpose | To initialise objects — set initial values for data fields |

#### Full Example with Multiple Constructors

```java
public class BankAccount {
    private String owner;
    private double balance;

    // Constructor 1: no-arg constructor — sets defaults
    BankAccount() {
        owner = "Unknown";
        balance = 0.0;
    }

    // Constructor 2: set only owner, balance defaults to 0
    BankAccount(String ownerName) {
        owner = ownerName;
        balance = 0.0;
    }

    // Constructor 3: set both owner and opening balance
    BankAccount(String ownerName, double openingBalance) {
        owner = ownerName;
        balance = openingBalance;
    }

    double getBalance() {
        return balance;
    }
}

// Usage:
BankAccount acc1 = new BankAccount();                        // uses Constructor 1
BankAccount acc2 = new BankAccount("Ali");                   // uses Constructor 2
BankAccount acc3 = new BankAccount("Siti", 500.00);          // uses Constructor 3
```

---

### Default Constructor

- If you write a class **without any constructor**, Java automatically provides a **no-arg constructor with an empty body**. This is called the **default constructor**.
- The default constructor is **only provided automatically if you have NOT written any constructor yourself**.
- The moment you define even ONE constructor, Java will NOT provide the default constructor anymore.

```java
// Case 1: No constructor defined → Java provides default constructor automatically
class Dog {
    String name;
    // Java auto-generates: Dog() {}
}
Dog d = new Dog();   // ✅ Works fine


// Case 2: One parameterised constructor defined → Java does NOT provide default constructor
class Cat {
    String name;
    Cat(String n) {
        name = n;
    }
    // Java does NOT generate Cat() {} anymore
}
Cat c1 = new Cat("Whiskers");   // ✅ Works fine
Cat c2 = new Cat();              // ❌ ERROR! No no-arg constructor exists


// Case 3: Fix — explicitly define your own no-arg constructor
class Fish {
    String name;
    Fish() {
        name = "Unknown";
    }
    Fish(String n) {
        name = n;
    }
}
Fish f1 = new Fish();           // ✅ Works
Fish f2 = new Fish("Nemo");     // ✅ Works
```

---

### Object Reference Variables

In Java, when you work with objects, you work with **references** (like an address/pointer to where the object lives in memory), not the object itself.

**Step 1: Declare a reference variable**
```java
Circle myCircle;       // declares a variable that CAN hold a reference to a Circle object
                       // at this point, myCircle holds 'null' — it points to nothing
```

**Step 2: Create the object and assign the reference**
```java
myCircle = new Circle(5.0);   // creates a Circle object in memory and stores its address in myCircle
```

**One-step declaration + creation:**
```java
Circle myCircle = new Circle(5.0);
```

**Accessing object members using the dot operator:**
```java
// Access data field
System.out.println(myCircle.radius);    // prints the radius value

// Call a method
double area = myCircle.getArea();       // invokes the getArea() method
System.out.println(area);
```

---

### Primitive vs Object Types

This is a very important distinction that affects how assignment (`=`) works.

#### Primitive Types (e.g., int, double, boolean)
- The variable **directly stores the value**.
- Assignment **copies the value**.

```java
int i = 1;
int j = 2;
i = j;       // i now holds 2 (a copy of j's value)
             // changing j later will NOT affect i
j = 99;
System.out.println(i);  // prints 2 — unchanged
```

#### Object Types (e.g., Circle, String, any class)
- The variable stores a **reference (address/pointer)** to the object.
- Assignment **copies the reference**, NOT the object itself.

```java
Circle c1 = new Circle(5);   // c1 points to an object with radius=5
Circle c2 = new Circle(9);   // c2 points to a different object with radius=9

c1 = c2;     // c1 now points to the SAME object as c2 (radius=9)
             // The original object c1 was pointing to (radius=5) is now ORPHANED
             // It becomes GARBAGE and will be collected by Java's garbage collector
```

**Memory diagram:**

```
BEFORE c1 = c2:
c1 ──→ [Circle: radius=5]
c2 ──→ [Circle: radius=9]

AFTER c1 = c2:
c1 ──→ [Circle: radius=9]  ← both point to same object
c2 ──→ [Circle: radius=9]
       [Circle: radius=5]  ← orphaned / garbage (no reference points to it)
```

> **Key insight:** Assigning one object variable to another does NOT create a new copy of the object. Both variables end up pointing to the **same** object. If you modify through one reference, the change is visible through the other reference too.

---

### Visibility Modifiers

Java uses **access modifiers** to control who can access (read/write) data fields and methods of a class.

| Modifier | Who can access it? |
|---|---|
| `public` | Anyone, from any class in any package |
| (default / no modifier) | Only classes in the **same package** |
| `private` | **Only the class itself** — not even subclasses |
| `protected` | Same class, same package, AND subclasses in other packages |

**Code example:**
```java
package p1;

public class Person {
    public String name;        // accessible by everyone
    int age;                   // accessible only within package p1 (default)
    private double salary;     // accessible ONLY inside Person class

    public void greet() { }    // accessible by everyone
    void work() { }            // accessible only within package p1
    private void sleep() { }   // accessible ONLY inside Person class
}
```

```java
// In same package p1:
class Colleague {
    void test() {
        Person p = new Person();
        p.name = "Ali";       // ✅ public — fine
        p.age = 25;            // ✅ default — same package, fine
        p.salary = 5000;       // ❌ private — ERROR!
        p.greet();             // ✅ public — fine
        p.work();              // ✅ default — same package, fine
        p.sleep();             // ❌ private — ERROR!
    }
}
```

```java
// In different package p2:
class Manager {
    void test() {
        Person p = new Person();
        p.name = "Siti";       // ✅ public — fine
        p.age = 30;             // ❌ default — different package, ERROR!
        p.salary = 6000;        // ❌ private — ERROR!
        p.greet();              // ✅ public — fine
        p.work();               // ❌ default — different package, ERROR!
        p.sleep();              // ❌ private — ERROR!
    }
}
```

---

### Why Make Data Fields Private?

**Reason 1: To protect data from invalid values**

If `radius` is public, anyone can set it to an invalid value:
```java
myCircle.radius = -5;   // negative radius doesn't make sense!
```

If `radius` is private, you control access via getter/setter methods where you can validate:
```java
public void setRadius(double r) {
    if (r >= 0) {
        radius = r;
    } else {
        System.out.println("Error: radius cannot be negative!");
    }
}
```

**Reason 2: To make the class easy to maintain**

If you need to change how radius is stored internally (e.g., from `double` to `float`), with private fields you only change the class internals. All external code using `getRadius()` / `setRadius()` continues to work unchanged.

**Getter and Setter pattern:**
```java
public class Circle {
    private double radius;          // private data field
    private static int count = 0;  // counts how many Circle objects exist

    public Circle() {
        radius = 1.0;
        count++;
    }

    public Circle(double r) {
        radius = r;
        count++;
    }

    // Getter (accessor) — reads the value
    public double getRadius() {
        return radius;
    }

    // Setter (mutator) — modifies the value with validation
    public void setRadius(double r) {
        if (r >= 0) {
            radius = r;
        }
    }

    // Static getter — returns class-level data
    public static int getCount() {
        return count;
    }

    public double getArea() {
        return Math.PI * radius * radius;
    }
}
```

---

## Part 2: Thinking in Objects

### Class Abstraction and Encapsulation

**Class abstraction** means separating *what a class does* (its interface/contract) from *how it does it* (its implementation). Users of a class only need to know what methods are available and what they do — they don't need to know the internal implementation details.

Think of it like driving a car:
- You know how to **use** the steering wheel, accelerator, and brakes (the interface).
- You don't need to understand how the engine, transmission, and hydraulics actually work (the implementation).

**Encapsulation** is the mechanism that achieves abstraction — by making data fields `private` and providing `public` methods, the internal details are **hidden** from outside users.

```
┌─────────────────────────────────────┐
│          Circle Class               │
│  ┌──────────────────────────────┐   │
│  │  (Hidden Implementation)     │   │
│  │  - radius (private)          │   │  ←── Encapsulated internals
│  │  - calculation logic         │   │
│  └──────────────────────────────┘   │
│                                     │
│  Public Contract (Interface):        │
│  + getRadius()                      │  ←── What users can access
│  + setRadius(r)                     │
│  + getArea()                        │
└─────────────────────────────────────┘
```

> **Analogy:** When you use `System.out.println("Hello")` in Java, you don't care about how Java internally converts "Hello" to characters and sends them to the console. You just use the public method. That's abstraction at work!

---

## Part 3: Inheritance and Polymorphism

### Inheritance

**Inheritance** is a mechanism where a new class (subclass) acquires the properties and behaviours of an existing class (superclass). It promotes **code reuse** and reduces redundancy.

**Real-world analogy:** A `Student` IS-A `Person`. A student has all properties of a person (name, age, address) PLUS additional properties specific to being a student (studentID, course, GPA).

**The IS-A relationship:**
```
Person    (superclass / parent class)
  ↑
  |  "is-a"
Student   (subclass / child class)  — Student IS-A Person ✅
```

**Syntax:**
```java
class SubclassName extends SuperclassName {
    // additional data fields
    // additional/overridden methods
}
```

**Why use inheritance?**
- Avoids repeating the same code in multiple classes
- Creates a logical hierarchy that mirrors real-world relationships
- Makes it easier to maintain and extend code

---

### Superclasses and Subclasses

```java
// Superclass — common features shared by all geometric shapes
public class GeometricObject {
    private String color = "white";
    private boolean filled = false;

    // Constructors
    protected GeometricObject() { }

    protected GeometricObject(String color, boolean filled) {
        this.color = color;
        this.filled = filled;
    }

    // Getters and setters
    public String getColor() { return color; }
    public void setColor(String color) { this.color = color; }
    public boolean isFilled() { return filled; }
    public void setFilled(boolean filled) { this.filled = filled; }

    @Override
    public String toString() {
        return "Color: " + color + ", Filled: " + filled;
    }
}

// Subclass 1 — inherits from GeometricObject
public class Circle extends GeometricObject {
    private double radius;

    public Circle() { }

    public Circle(double radius) {
        this.radius = radius;
    }

    public Circle(double radius, String color, boolean filled) {
        super(color, filled);      // calls the superclass constructor
        this.radius = radius;
    }

    public double getRadius() { return radius; }
    public void setRadius(double radius) { this.radius = radius; }
    public double getArea() { return Math.PI * radius * radius; }
    public double getPerimeter() { return 2 * Math.PI * radius; }
    public double getDiameter() { return 2 * radius; }
}

// Subclass 2 — also inherits from GeometricObject
public class Rectangle extends GeometricObject {
    private double width;
    private double height;

    public Rectangle() { }

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public Rectangle(double width, double height, String color, boolean filled) {
        super(color, filled);
        this.width = width;
        this.height = height;
    }

    public double getArea() { return width * height; }
    public double getPerimeter() { return 2 * (width + height); }
}
```

**Important questions answered:**

| Question | Answer |
|---|---|
| Is a subclass a subset of superclass? | Yes — every subclass object IS-A superclass object, but not vice versa |
| Is private accessible in subclass? | No — private members are NOT inherited; subclass cannot directly access them |
| Can you extend a class? | Yes — but only ONE class (single inheritance) |
| Multiple inheritance? | Not for classes — a class can only `extend` one class |
| Is superclass constructor inherited? | No — constructors are NOT inherited, but they ARE invoked via `super()` |

---

### Calling Superclass Constructor

Use the `super()` keyword to explicitly call a superclass constructor from a subclass constructor.

```java
public class Circle extends GeometricObject {
    private double radius;

    public Circle(double radius, String color, boolean filled) {
        super(color, filled);    // MUST be the FIRST statement in the constructor
        this.radius = radius;
    }
}
```

**Rules for `super()`:**
1. `super()` or `super(arguments)` must be the **very first statement** in the constructor
2. If you don't call `super()` explicitly, Java automatically inserts `super()` (no-arg) as the first line

---

### Superclass Constructor is Always Invoked

**Critical concept:** When a subclass object is created, the **superclass constructor is always called first**, before the subclass constructor body runs.

```java
public class A {
    public A() {
        System.out.println("A's no-arg constructor");
    }
}

public class B extends A {
    public B() {
        // Java automatically inserts super() here if you don't write it
        System.out.println("B's no-arg constructor");
    }
}

public class C extends B {
    public C() {
        // Java automatically inserts super() here
        System.out.println("C's no-arg constructor");
    }
}

// What happens when you write: C obj = new C();
// Output:
// A's no-arg constructor    ← superclass of superclass runs first
// B's no-arg constructor    ← superclass runs second
// C's no-arg constructor    ← subclass runs last
```

**The "no A() constructor" error trap:**
```java
// (a) This works fine:
class A {
    public A() {
        System.out.println("A's no-arg constructor is invoked");
    }
}
class B extends A { }
// B b = new B();  → Java inserts super() in B's default constructor → calls A() → WORKS ✅

// (b) This causes a compile ERROR:
class A {
    public A(int x) { }    // Only parameterised constructor, no no-arg constructor
}
class B extends A {
    public B() { }         // Java tries to insert super() here, but A() doesn't exist!
}
// B b = new B();  → ERROR: constructor A() is undefined ❌
// Fix: either add A() to class A, or write super(someValue) in B's constructor
```

---

### Overriding vs Overloading

These two terms are commonly confused. Here's the clear distinction:

#### Method Overloading
- **Within the same class** (or across related classes)
- **Same method name**, but **different parameter list** (different number, type, or order of parameters)
- The return type may or may not differ
- Resolved at **compile time** (static binding)

```java
public class Calculator {
    // Overloaded methods — same name, different parameters
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {    // different parameter types
        return a + b;
    }

    public int add(int a, int b, int c) {      // different number of parameters
        return a + b + c;
    }
}
```

#### Method Overriding
- **In a subclass**, redefining a method that already exists in the superclass
- **Same method name**, **same parameter list**, **same return type**
- Provides a **new implementation** specific to the subclass
- Resolved at **runtime** (dynamic binding)

```java
class Animal {
    public String makeSound() {
        return "Some generic sound";
    }
}

class Dog extends Animal {
    @Override                           // @Override annotation — good practice!
    public String makeSound() {         // same signature as superclass
        return "Woof!";                 // NEW implementation
    }
}

class Cat extends Animal {
    @Override
    public String makeSound() {
        return "Meow!";
    }
}
```

#### Side-by-side comparison (from lecture slides):

```java
// Example (a) — OVERRIDING (parameter type is the same: double)
class B {
    public void p(double i) { System.out.println(i * 2); }
}
class A extends B {
    public void p(double i) { System.out.println(i); }   // overrides B's p(double)
}
A a = new A();
a.p(10);     // Output: 10.0  (A's version is called for BOTH int and double input)
a.p(10.0);   // Output: 10.0  (A's version is called)

// Example (b) — OVERLOADING (parameter type is different: int vs double)
class B {
    public void p(double i) { System.out.println(i * 2); }
}
class A extends B {
    public void p(int i) { System.out.println(i); }   // overloads — different parameter type!
}
A a = new A();
a.p(10);     // Output: 10    (A's p(int) is called because 10 is an int)
a.p(10.0);   // Output: 20.0  (B's p(double) is called because 10.0 is a double)
```

| Feature | Overloading | Overriding |
|---|---|---|
| Class | Same or related classes | Only in subclass |
| Method name | Same | Same |
| Parameters | **Different** | Same |
| Return type | Can differ | Same (or covariant) |
| Binding | Compile time (static) | Runtime (dynamic) |
| Purpose | Convenience (multiple ways to call) | Change inherited behaviour |

---

### Polymorphism

**Polymorphism** (from Greek: "many forms") means that a variable of a supertype can refer to a subtype object. An object of a subtype can be used wherever the supertype is expected.

```java
// A GeometricObject variable can hold a Circle or Rectangle
GeometricObject shape1 = new Circle(5.0);
GeometricObject shape2 = new Rectangle(3.0, 4.0);

// A Person variable can hold a Student
Person p = new Student("Ali", "CS101");

// Fruit variable can hold Apple or Orange
Fruit f = new Apple();
Fruit g = new Orange();
```

**Why is this powerful?**
```java
// You can write ONE method that works for ALL geometric shapes
public static void printShapeInfo(GeometricObject shape) {
    System.out.println("Color: " + shape.getColor());
    System.out.println("Filled: " + shape.isFilled());
    // getArea() will call the RIGHT implementation depending on actual object type
    System.out.println("Area: " + shape.getArea());
}

// Call it with any GeometricObject subtype
printShapeInfo(new Circle(5.0));        // works!
printShapeInfo(new Rectangle(3, 4));    // works!
printShapeInfo(new Triangle(3, 4, 5));  // works!
```

---

### Dynamic Binding

When an overridden method is called through a supertype reference, Java decides at **runtime** which version of the method to call, based on the actual type of the object. This is called **dynamic binding** or **late binding**.

```java
public class PolymorphismDemo {
    public static void main(String[] args) {
        m(new GraduateStudent());   // passes GraduateStudent as Object
        m(new Student());
        m(new Person());
        m(new Object());
    }

    // This ONE method accepts ANY object (Object is supertype of everything in Java)
    public static void m(Object x) {
        System.out.println(x.toString());   // which toString() is called? Decided at runtime!
    }
}

class Person extends Object {
    @Override
    public String toString() { return "Person"; }
}

class Student extends Person {
    @Override
    public String toString() { return "Student"; }
}

class GraduateStudent extends Student {
    // No toString() override — inherits Student's toString()
}

/*
Output:
Student          ← GraduateStudent has no toString(), inherits Student's
Student          ← Student's toString()
Person           ← Person's toString()
java.lang.Object@<hashcode>  ← Object's default toString()
*/
```

---

### Visibility/Accessibility Modifiers Summary

| Modifier | Same Class | Same Package | Subclass (different package) | Different Package |
|---|:---:|:---:|:---:|:---:|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| default (no modifier) | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

**Full visibility modifier example (from lecture):**

```java
package p1;

public class C1 {
    public int x;        // accessible everywhere
    protected int y;     // accessible in p1 + subclasses
    int z;               // accessible only in p1 (default)
    private int u;       // accessible only in C1

    protected void m() { }
}

// In package p1 (same package):
class C2 {
    void test() {
        C1 o = new C1();
        o.x;    // ✅ public
        o.y;    // ✅ protected — same package
        o.z;    // ✅ default — same package
        o.u;    // ❌ private
        o.m();  // ✅ protected — same package
    }
}

// In package p1 (subclass, same package):
class C3 extends C1 {
    // can access: x ✅, y ✅, z ✅, u ❌, m() ✅
}

// In package p2 (subclass, DIFFERENT package):
class C4 extends C1 {
    // can access: x ✅, y ✅ (protected allows subclass), z ❌, u ❌, m() ✅
}

// In package p2 (NOT a subclass, different package):
class C5 {
    void test() {
        C1 o = new C1();
        o.x;    // ✅ public
        o.y;    // ❌ protected — not a subclass
        o.z;    // ❌ default — different package
        o.u;    // ❌ private
        o.m();  // ❌ protected — not a subclass
    }
}
```

---

## Part 4: Abstract Classes and Interfaces

### Abstract Classes

An **abstract class** is a class that is marked with the `abstract` keyword. It is used as a **base class** and represents an incomplete concept that needs to be specialised by subclasses.

**Example:** `GeometricObject` is abstract because you don't typically create a "generic geometric object" — you create specific shapes like circles and rectangles. However, all shapes share common properties like color and fill, and they all must have an area and perimeter.

```java
public abstract class GeometricObject {
    private String color = "white";
    private boolean filled;

    protected GeometricObject() { }

    protected GeometricObject(String color, boolean filled) {
        this.color = color;
        this.filled = filled;
    }

    public String getColor() { return color; }
    public void setColor(String color) { this.color = color; }
    public boolean isFilled() { return filled; }
    public void setFilled(boolean filled) { this.filled = filled; }

    // Abstract methods — no implementation here, subclasses MUST implement
    public abstract double getArea();
    public abstract double getPerimeter();
}
```

```java
// Concrete subclass MUST implement all abstract methods
public class Circle extends GeometricObject {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;    // MUST implement this
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;         // MUST implement this
    }
}
```

---

### 6 Key Rules About Abstract Classes

#### Rule 1: Abstract methods MUST be in abstract classes
An abstract method (no body) cannot exist in a regular (non-abstract) class.

```java
// ❌ WRONG — cannot have abstract method in non-abstract class
public class GeometricObject {
    public abstract double getArea();   // ERROR!
}

// ✅ CORRECT
public abstract class GeometricObject {
    public abstract double getArea();   // Fine in abstract class
}
```

#### Rule 2: Cannot create objects (instances) from abstract classes
```java
// ❌ WRONG
GeometricObject g = new GeometricObject();   // ERROR! Cannot instantiate abstract class

// ✅ CORRECT — use a concrete subclass
GeometricObject g = new Circle(5.0);   // Fine — Circle is concrete (non-abstract)
```

But you CAN still define constructors in abstract classes — they are called from subclass constructors via `super()`.

#### Rule 3: Abstract class CAN have zero abstract methods
You can declare a class as `abstract` even if it has no abstract methods. This prevents instantiation while still serving as a base class.
```java
public abstract class Vehicle {
    // No abstract methods, but...
    public void start() { System.out.println("Starting..."); }
    // ...marking it abstract prevents: new Vehicle() — which may not make sense
}
```

#### Rule 4: Superclass of an abstract class may be concrete
```java
public class Object { }                              // Object is concrete
public abstract class GeometricObject extends Object { }  // abstract extends concrete — VALID ✅
```

#### Rule 5: Subclass can override a concrete method as abstract
This is rare but possible. If a subclass makes a method abstract, the subclass MUST also be abstract.
```java
public abstract class GeometricObject {
    public String someMethod() { return "GeometricObject"; }  // concrete method
}

// A subclass can make it abstract again:
public abstract class Circle extends GeometricObject {
    @Override
    public abstract String someMethod();    // now abstract — Circle must be abstract too
}
```

#### Rule 6: Abstract class CAN be used as a data type (reference type)
```java
// Cannot create GeometricObject instances, but CAN use it as a type:
GeometricObject[] shapes = new GeometricObject[10];   // ✅ creating array is fine

shapes[0] = new Circle(5.0);        // ✅ assign concrete subclass
shapes[1] = new Rectangle(3, 4);    // ✅ assign concrete subclass

for (GeometricObject shape : shapes) {
    if (shape != null) {
        System.out.println("Area: " + shape.getArea());   // polymorphism at work!
    }
}
```

---

### Interfaces

An **interface** is a contract that specifies what a class must do, without specifying how it does it. It is like an abstract class but stricter:
- All data fields must be `public static final` (constants)
- All methods must be `public abstract` (no concrete methods — in older Java versions)
- No constructors
- Cannot be instantiated with `new`

**When to use interface vs abstract class?**
- Use **interface** when you want to define a **capability** that unrelated classes can share (e.g., `Comparable`, `Serializable`, `Edible`)
- Use **abstract class** when you have a **is-a hierarchy** with shared state/implementation (e.g., `GeometricObject` → `Circle`, `Rectangle`)

> **Real-world analogy:** An interface is like a job requirement. "Must be able to drive" (`Driveable` interface) can apply to taxi drivers, truck drivers, and racing car drivers — they're all very different, but they all satisfy the `Driveable` contract.

---

### Defining an Interface

```java
public interface InterfaceName {
    // Constants (implicitly public static final)
    int CONSTANT = 100;

    // Abstract methods (implicitly public abstract)
    returnType methodName(parameters);
}
```

**Example:**
```java
public interface Edible {
    /** Describe how to eat */
    public abstract String howToEat();   // explicitly written with modifiers
}

// Equivalent — modifiers can be omitted because they're implicit:
public interface Edible {
    String howToEat();
}
```

---

### Implementing an Interface

A class uses the `implements` keyword to implement an interface. It **must** provide a concrete implementation for **all** methods declared in the interface (or the class must be declared abstract).

```java
public interface Edible {
    String howToEat();
}

// Animal class hierarchy (does NOT implement Edible — tigers aren't food!)
abstract class Animal {
    public abstract String sound();
}

class Tiger extends Animal {
    @Override
    public String sound() { return "Tiger: RROOAARR"; }
}

// Chicken IS an Animal AND IS Edible
class Chicken extends Animal implements Edible {
    @Override
    public String howToEat() { return "Chicken: Fry it"; }

    @Override
    public String sound() { return "Chicken: cock-a-doodle-doo"; }
}

// Fruit is Edible but is NOT an Animal
abstract class Fruit implements Edible {
    // Fruit doesn't implement howToEat() → must be abstract
    // Subclasses of Fruit must implement howToEat()
}

class Apple extends Fruit {
    @Override
    public String howToEat() { return "Apple: Make apple cider"; }
}

class Orange extends Fruit {
    @Override
    public String howToEat() { return "Orange: Make orange juice"; }
}
```

**The TestEdible code from lecture:**
```java
public class TestEdible {
    public static void main(String[] args) {
        Object[] objects = {new Tiger(), new Chicken(), new Apple()};

        for (int i = 0; i < objects.length; i++) {
            if (objects[i] instanceof Edible) {
                System.out.println(((Edible)objects[i]).howToEat());
            }
            if (objects[i] instanceof Animal) {
                System.out.println(((Animal)objects[i]).sound());
            }
        }
    }
}

/*
Output:
Tiger: RROOAARR           ← Tiger IS Animal → sound() called
Chicken: Fry it           ← Chicken IS Edible → howToEat() called
Chicken: cock-a-doodle-doo ← Chicken IS Animal → sound() called
Apple: Make apple cider   ← Apple IS Edible → howToEat() called
*/
```

---

### Extend vs Implement

| | `extends` | `implements` |
|---|---|---|
| Used by | Classes and Interfaces | Classes only |
| Inherits from | One class OR multiple interfaces | One or more interfaces |
| Gets concrete code? | Yes (from class) | No (must write own implementation) |
| Multiple allowed? | One class only (`extends` ONE class) | Multiple interfaces allowed |

```java
// A class extending a class and implementing interfaces:
public class Penguin extends Bird implements Swimmable, Walkable {
    // Must implement all methods from Swimmable AND Walkable
    // Inherits all non-private members from Bird
}

// An interface extending another interface:
public interface NewInterface extends Interface1, Interface2, Interface3 {
    // adds more abstract methods on top of inherited ones
}
```

---

### Interface Inheritance

Unlike classes (which can only `extend` one class), **interfaces can extend multiple interfaces**.

```java
public interface Flyable {
    void fly();
}

public interface Swimmable {
    void swim();
}

// Interface extending multiple interfaces
public interface Duck extends Flyable, Swimmable {
    void quack();
    // A class implementing Duck must implement: fly(), swim(), AND quack()
}
```

**Important:** An interface **cannot implement** another interface — it can only `extend` it.
```java
interface A { void methodA(); }
interface B extends A { void methodB(); }   // ✅ interface extends interface
interface C implements A { }               // ❌ ERROR — interfaces cannot implement
```

---

### Omitting Modifiers in Interfaces

In an interface, all members have implied modifiers that you don't need to write explicitly:

- All data fields are implicitly `public static final`
- All methods are implicitly `public abstract`

```java
// Verbose form (with explicit modifiers):
public interface T1 {
    public static final int K = 1;
    public abstract void p();
}

// Compact form (omitting implicit modifiers — exactly equivalent):
public interface T1 {
    int K = 1;
    void p();
}
```

**Accessing interface constants:**
```java
System.out.println(T1.K);    // access using InterfaceName.CONSTANT_NAME
```

---

### Interfaces vs Abstract Classes

| Feature | Abstract Class | Interface |
|---|---|---|
| **Variables** | No restrictions (any type, any modifier) | Must be `public static final` (constants only) |
| **Constructors** | Can have constructors (called via `super()`) | No constructors at all |
| **Methods** | Can have both abstract and concrete methods | All methods are `public abstract` (Java 8+ allows `default` methods) |
| **Inheritance** | A class can extend only **one** abstract class | A class can implement **multiple** interfaces |
| **Instantiation** | Cannot use `new` directly | Cannot use `new` directly |
| **`extends`/`implements`** | Subclass uses `extends` | Implementing class uses `implements` |

**Side-by-side code comparison:**

```java
// Using ABSTRACT CLASS:
abstract class Animal {
    public abstract String howToEat();
}
class Chicken extends Animal {
    @Override public String howToEat() { return "Fry it"; }
}
class Duck extends Animal {
    @Override public String howToEat() { return "Roast it"; }
}
// Main:
Animal animal = new Chicken();
eat(animal);                    // calls Chicken's howToEat()
animal = new Duck();
eat(animal);                    // calls Duck's howToEat()

public static void eat(Animal a) { a.howToEat(); }


// Using INTERFACE:
interface Edible {
    String howToEat();
}
class Chicken implements Edible {
    @Override public String howToEat() { return "Fry it"; }
}
class Duck implements Edible {
    @Override public String howToEat() { return "Roast it"; }
}
class Broccoli implements Edible {    // Broccoli is NOT an Animal but IS Edible!
    @Override public String howToEat() { return "Stir-fry it"; }
}
// Main:
Edible stuff = new Chicken();
eat(stuff);                      // calls Chicken's howToEat()
stuff = new Broccoli();
eat(stuff);                      // works! Broccoli isn't an Animal but implements Edible

public static void eat(Edible e) { e.howToEat(); }
```

> **Key insight:** The interface approach is more **flexible** — it allows unrelated classes (`Chicken` and `Broccoli`) to share the same behaviour contract (`Edible`), whereas with abstract classes, `Broccoli` would have to extend `Animal`, which doesn't make logical sense.

---

## Practice MCQs with Explanations

### Question 1: Constructors

**Which of the following is true about constructors?**

- A) A constructor can have a return type of `void`
- B) A constructor must have a different name than the class
- C) A constructor is invoked using the `new` operator
- D) A constructor can only have one version per class

**✅ Correct Answer: C**

**Explanation of each option:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | Constructors have **no return type at all** — not even `void`. If you write `void Circle()`, it becomes a regular method, not a constructor! |
| B | ❌ WRONG | Constructors **must** have the **same name** as the class. |
| C | ✅ CORRECT | Constructors are invoked using the `new` operator, e.g. `new Circle()` |
| D | ❌ WRONG | A class can have **multiple constructors** (constructor overloading) — they just need different parameter lists. |

---

### Question 2: Default Constructor

**When is a default (no-arg) constructor automatically provided by Java?**

- A) Always, regardless of what constructors you define
- B) Only if the class has no data fields
- C) Only if no constructors are explicitly defined in the class
- D) Only if the class is declared as `public`

**✅ Correct Answer: C**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | Java does NOT always provide one. If you define your own constructor, no default is generated. |
| B | ❌ WRONG | The presence/absence of data fields is irrelevant. |
| C | ✅ CORRECT | The default constructor is only auto-generated when you haven't written any constructor yourself. |
| D | ❌ WRONG | The `public` keyword on the class has nothing to do with it. |

```java
// Proof:
class A { int x; }                 // no constructors → Java provides A() automatically
class B { B(int x) { } }           // one constructor defined → Java provides NOTHING
A a = new A();   // ✅ works
B b = new B();   // ❌ ERROR — no no-arg constructor!
B b2 = new B(5); // ✅ works
```

---

### Question 3: Object Reference Assignment

**What happens when you execute `c1 = c2` where both are object reference variables?**

- A) A copy of the object referenced by c2 is created and assigned to c1
- B) c1 now references the same object as c2, and the old c1 object may be garbage collected
- C) The values in c1's object are updated with the values from c2's object
- D) A compile error occurs because you cannot assign object references

**✅ Correct Answer: B**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | Assignment does NOT copy the object itself — it only copies the **reference** (address). |
| B | ✅ CORRECT | Both `c1` and `c2` now point to the **same object**. The object previously referenced by `c1` has no more references pointing to it, so it becomes garbage. |
| C | ❌ WRONG | The field values are NOT copied — no deep copy happens. |
| D | ❌ WRONG | This is completely valid Java syntax. |

```java
Circle c1 = new Circle(5);   // c1 → [radius=5]
Circle c2 = new Circle(9);   // c2 → [radius=9]
c1 = c2;
// Now: c1 → [radius=9] (same as c2)
//      [radius=5] — orphaned, awaiting garbage collection
System.out.println(c1.radius);   // 9 (not 5!)
```

---

### Question 4: Overriding vs Overloading

**Which of the following correctly describes method overriding?**

- A) Defining multiple methods with the same name but different parameter lists in the same class
- B) Providing a new implementation for a method in a subclass, with the same name and parameter list as the superclass method
- C) Changing the return type of a method in a subclass
- D) Making a public method private in a subclass

**✅ Correct Answer: B**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | This describes **overloading**, not overriding. Overloading = same name + different parameters. |
| B | ✅ CORRECT | Overriding = subclass provides a new implementation with the **exact same signature** (name + parameters + return type) as the superclass method. |
| C | ❌ WRONG | You generally cannot change the return type arbitrarily when overriding (though covariant return types are allowed in Java — returning a more specific type). |
| D | ❌ WRONG | When overriding, you cannot reduce the visibility (e.g., you cannot override a `public` method as `private`). You can increase visibility but not reduce it. |

---

### Question 5: Abstract Classes

**Which of the following statements about abstract classes is CORRECT?**

- A) An abstract class can be instantiated using the `new` operator
- B) An abstract class must contain at least one abstract method
- C) An abstract class can contain both abstract and concrete (non-abstract) methods
- D) A subclass of an abstract class is always abstract

**✅ Correct Answer: C**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | You **cannot** use `new` to create an instance of an abstract class directly. |
| B | ❌ WRONG | An abstract class can have **zero abstract methods**. You can mark a class as abstract purely to prevent direct instantiation. |
| C | ✅ CORRECT | Abstract classes can have a **mix** of abstract methods (no body) and concrete methods (with body). This is actually one of the key advantages over interfaces. |
| D | ❌ WRONG | A subclass of an abstract class becomes **concrete** (non-abstract) as long as it implements ALL the abstract methods from the superclass. |

---

### Question 6: Interfaces

**Which of the following interface declarations is CORRECT?**

```
(a) interface A { void print() { }; }
(b) abstract interface A extends I1, I2 { abstract void print() { }; }
(c) abstract interface A { print(); }
(d) interface A { void print(); }
```

- A) Option (a)
- B) Option (b)
- C) Option (c)
- D) Option (d)

**✅ Correct Answer: D**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | `void print() { }` has a method **body** `{ }`. Interface methods cannot have a body (in standard Java). |
| B | ❌ WRONG | Two issues: (1) `abstract` keyword before `interface` is redundant/wrong. (2) `abstract void print() { }` gives an abstract method a body — contradictory! |
| C | ❌ WRONG | `print()` has no return type declared. Also `abstract` before `interface` is not standard. |
| D | ✅ CORRECT | `void print();` — correct syntax: return type declared, no body (ends with `;`), no explicit `abstract` keyword needed (it's implicit). |

---

### Question 7: Polymorphism and Dynamic Binding

**What is the output of the following code?**

```java
class A {
    public String toString() { return "A"; }
}
class B extends A {
    public String toString() { return "B"; }
}
class C extends B { }

public class Test {
    public static void main(String[] args) {
        A obj = new C();
        System.out.println(obj.toString());
    }
}
```

- A) `A`
- B) `B`
- C) `C`
- D) Compile error

**✅ Correct Answer: B**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | `A`'s `toString()` would only be called if the actual object was of type `A` or a subclass that doesn't override it. `B` overrides it. |
| B | ✅ CORRECT | Dynamic binding: `obj` is declared as type `A`, but the actual object is `new C()`. C doesn't override `toString()`, so Java looks up the inheritance chain and finds `B`'s `toString()`, which returns `"B"`. |
| C | ❌ WRONG | `C` doesn't define a `toString()` method. |
| D | ❌ WRONG | The code is syntactically valid — `A obj = new C()` is valid because `C` IS-A `A` (polymorphism). |

**Key concept:** Java always calls the method of the **actual object type** at runtime (dynamic binding), not the declared reference type.

---

### Question 8: Superclass Constructor Chaining

**What is the output of the following code?**

```java
class X {
    X() { System.out.println("X"); }
}
class Y extends X {
    Y() { System.out.println("Y"); }
}
class Z extends Y {
    Z() { System.out.println("Z"); }
}
public class Test {
    public static void main(String[] args) {
        new Z();
    }
}
```

- A) `Z`
- B) `Z Y X`
- C) `X Y Z`
- D) `Y Z`

**✅ Correct Answer: C**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | Only printing "Z" would mean the superclass constructors were skipped, which never happens. |
| B | ❌ WRONG | Z→Y→X order would mean subclass runs before superclass. The opposite is true. |
| C | ✅ CORRECT | When `new Z()` is called: Z's constructor has `super()` (implicit) → calls Y's constructor → Y has `super()` (implicit) → calls X's constructor → X prints "X" → returns to Y which prints "Y" → returns to Z which prints "Z". **Superclass always runs first.** |
| D | ❌ WRONG | X is also in the chain and should be printed. |

---

### Question 9: Interface vs Abstract Class

**Which statement correctly differentiates interfaces from abstract classes?**

- A) An interface can have constructors but an abstract class cannot
- B) A class can extend multiple abstract classes but can only implement one interface
- C) All variables in an interface are implicitly `public static final`, but an abstract class has no such restriction
- D) An abstract class cannot have any concrete methods, but an interface can

**✅ Correct Answer: C**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | It is the opposite — **neither** can have instances created with `new`, but abstract classes CAN have constructors (called via `super()`). Interfaces cannot have constructors. |
| B | ❌ WRONG | Completely backwards. A class can extend only **ONE** class (abstract or not), but can implement **multiple** interfaces. |
| C | ✅ CORRECT | Interface variables are always `public static final` (constants). Abstract class variables have no restrictions — they can be any type with any modifier. |
| D | ❌ WRONG | Also backwards. **Abstract classes** can have concrete (non-abstract) methods. Standard **interfaces** cannot (all methods are implicitly abstract — though Java 8 introduced `default` methods). |

---

### Question 10: `extends` vs `implements`

**Which of the following is valid Java code?**

- A) `interface A implements B { }`
- B) `class A extends B, C { }` (where B and C are both classes)
- C) `class A implements B, C { }` (where B and C are both interfaces)
- D) `interface A extends B implements C { }` (where B and C are interfaces)

**✅ Correct Answer: C**

**Explanation:**

| Option | Verdict | Reason |
|---|---|---|
| A | ❌ WRONG | An **interface cannot `implement`** another interface. Interfaces use `extends` to inherit from other interfaces. |
| B | ❌ WRONG | Java supports **single class inheritance only**. A class can only `extend` ONE class. `extends B, C` is not valid. |
| C | ✅ CORRECT | A class can `implement` **multiple interfaces** separated by commas. This is how Java achieves a form of multiple inheritance of type. |
| D | ❌ WRONG | An interface uses `extends` (not `implements`) to inherit from other interfaces, and `implements` is a keyword for classes, not interfaces. |

---

## Quick Reference Summary

### OOP Pillars

| Pillar | Description | Key Keyword/Mechanism |
|---|---|---|
| **Encapsulation** | Hiding implementation details; exposing only what's needed | `private` + getter/setter |
| **Inheritance** | A subclass acquires properties and behaviours of a superclass | `extends` |
| **Polymorphism** | A supertype variable can refer to subtype objects; methods behave differently based on actual type | Overriding + dynamic binding |
| **Abstraction** | Separating interface from implementation | `abstract` class/method, `interface` |

### Constructor Chaining Rules
1. If a constructor doesn't call `super()` explicitly, Java inserts `super()` (no-arg) as the first line.
2. Superclass constructors always run **before** subclass constructors.
3. If superclass only has a parameterised constructor (no no-arg), subclass MUST explicitly call `super(args)`.

### Method Override Rules
1. Same method name, same parameter list, same return type.
2. Cannot reduce visibility (can't override `public` as `private`).
3. Use `@Override` annotation — it helps catch mistakes at compile time.

### Interface Rules
1. All variables = `public static final`
2. All methods = `public abstract`
3. No constructors
4. Cannot be instantiated
5. A class `implements` interfaces; an interface `extends` interfaces
6. A class can implement multiple interfaces
7. A class implementing an interface must implement ALL methods, or be declared `abstract`

### Abstract Class Rules
1. Cannot be instantiated with `new`
2. CAN have constructors (used by subclasses)
3. CAN have both abstract and concrete methods
4. A class with ANY abstract method must be declared abstract
5. Subclass must implement ALL abstract methods, or itself be declared abstract
6. CAN be used as a reference type (variable type)

---

*Notes compiled from WIA1002 Data Structure OOP Revision Lecture Slides, Universiti Malaya.*  
*Reference: Liang, Introduction to Java Programming, 10th Edition, Global Edition, Pearson, 2015 — Chapters 9, 10, 11, 13.*
