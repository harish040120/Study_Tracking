# Java OOP Interview Handbook

A complete, interview-ready guide to Object-Oriented Programming in Java. Every topic follows the same structure:

**Concept → Why it exists (the problem it solves) → Java code → Interview Q&A**

---

## Table of Contents

1. [What is OOP & Why it exists](#1-what-is-oop--why-it-exists)
2. [Class & Object](#2-class--object)
3. [The 4 Pillars of OOP](#3-the-4-pillars-of-oop)
   - [Encapsulation](#31-encapsulation)
   - [Abstraction](#32-abstraction)
   - [Inheritance](#33-inheritance)
   - [Polymorphism](#34-polymorphism)
4. [Constructors](#4-constructors)
5. [`this` and `super`](#5-this-and-super-keywords)
6. [Access Modifiers](#6-access-modifiers)
7. [`static` keyword](#7-static-keyword)
8. [`final` keyword](#8-final-keyword)
9. [Abstract Classes vs Interfaces](#9-abstract-classes-vs-interfaces)
10. [Method Overloading vs Overriding](#10-method-overloading-vs-overriding)
11. [Constructor Chaining & Init Order](#11-constructor-chaining--initialization-order)
12. [Object Class Methods (equals, hashCode, toString)](#12-object-class-methods)
13. [Composition vs Inheritance (HAS-A vs IS-A)](#13-composition-vs-inheritance)
14. [Association, Aggregation, Composition](#14-association-aggregation-composition)
15. [Coupling & Cohesion](#15-coupling--cohesion)
16. [SOLID Principles](#16-solid-principles)
17. [Design Patterns (OOP-relevant)](#17-design-patterns)
18. [Common OOP Interview Coding Problems](#18-common-oop-interview-coding-problems)
19. [Rapid-Fire Q&A Cheat Sheet](#19-rapid-fire-qa-cheat-sheet)

---

## 1. What is OOP & Why it exists

**Definition:** OOP is a programming paradigm that organizes software design around **data (objects)** rather than **functions and logic**. An object bundles data (fields) and behavior (methods) together.

**Why it exists (the problem before OOP):**
Before OOP, procedural languages (like C) separated data and functions. This caused:
- **Tight coupling** between data and the many functions that touched it — one data structure change broke code everywhere.
- **No data protection** — any function could freely modify any global data, causing bugs that were hard to trace.
- **Poor reusability** — logic tied to one program's data structures couldn't be reused elsewhere.
- **Hard to model real-world systems** — real world is made of objects (Car, Account, User) with their own state and behavior, not just flat functions.

OOP solves this by modeling software as interacting objects, giving us **modularity, reusability, security, and maintainability**.

```java
// Procedural style (data and behavior separate — fragile)
class ProceduralBank {
    static double[] balances = new double[10];

    static void withdraw(int accId, double amt) {
        balances[accId] -= amt; // anyone, anywhere can corrupt this array
    }
}

// OOP style (data + behavior bundled, protected)
class Account {
    private double balance; // protected data

    public void withdraw(double amt) {
        if (amt > balance) throw new IllegalArgumentException("Insufficient funds");
        balance -= amt; // only this class controls how balance changes
    }
}
```

---

## 2. Class & Object

**Class:** A blueprint/template that defines fields (state) and methods (behavior).
**Object:** A runtime instance of a class, created in heap memory.

**Why they exist:** We need a way to describe a *type* of entity once (Class) and then create many concrete instances of it (Objects) without rewriting the structure every time — similar to how a "house blueprint" is designed once and many houses are built from it.

```java
class Car {
    // fields = state
    String model;
    int speed;

    // method = behavior
    void accelerate() {
        speed += 10;
        System.out.println(model + " speed is now " + speed);
    }
}

public class Main {
    public static void main(String[] args) {
        Car car1 = new Car(); // object 1 - own memory
        car1.model = "Tesla";
        car1.accelerate();

        Car car2 = new Car(); // object 2 - independent state
        car2.model = "BMW";
        car2.accelerate();
    }
}
```

**Interview Q&A:**
- Q: Where are objects stored in memory? → **Heap**. Reference variables live on the **stack**.
- Q: Can a class exist without any object? → Yes, but it's useless unless it has `static` members.
- Q: Is Java "purely" object-oriented? → No — it has primitives (`int`, `char`, etc.) that aren't objects.

---

## 3. The 4 Pillars of OOP

### 3.1 Encapsulation

**Definition:** Wrapping data (fields) and code (methods) together into a single unit (class), and **restricting direct access** to the internal state using access modifiers, exposing controlled access via getters/setters.

**Problem it solves:** Without encapsulation, any part of the program could set an object's fields to invalid states (e.g., negative age, negative balance). This causes **data corruption** and makes debugging extremely hard because you can't control *who* changes *what*.

**Benefit:** Data validation, controlled access, ability to change internal implementation without breaking external code (loose coupling), improved security.

```java
class BankAccount {
    private double balance; // hidden from outside — cannot be accessed directly

    public BankAccount(double initialBalance) {
        if (initialBalance < 0) throw new IllegalArgumentException("Balance cannot be negative");
        this.balance = initialBalance;
    }

    // Controlled read access
    public double getBalance() {
        return balance;
    }

    // Controlled write access with validation logic
    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Deposit must be positive");
        balance += amount;
    }

    public void withdraw(double amount) {
        if (amount > balance) throw new IllegalStateException("Insufficient funds");
        balance -= amount;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount(1000);
        acc.deposit(500);
        acc.withdraw(200);
        // acc.balance = -99999;  // COMPILE ERROR — cannot access private field directly
        System.out.println(acc.getBalance()); // 1300.0
    }
}
```

**Interview Q&A:**
- Q: Is encapsulation just about `private` fields + getters/setters? → No — it's about **information hiding + controlled access**, even if that means NOT providing a setter at all (immutability).
- Q: Real-world example? → A capsule/pill — the medicine (data) is hidden inside a shell (class); you interact with it only through defined interfaces.

---

### 3.2 Abstraction

**Definition:** Hiding **implementation complexity** and exposing only the essential/relevant features to the user.

**Problem it solves:** Users of a class/API shouldn't need to know *how* something works internally — only *what* it does. Without abstraction, every consumer of your code is tightly coupled to your internal implementation, so any internal change breaks everyone.

**Benefit:** Reduces complexity for the user, allows implementation to change freely, improves security (internal logic hidden), enables designing against contracts.

```java
// Abstraction via abstract class
abstract class PaymentProcessor {
    // exposed essential operation - the "what"
    public final void pay(double amount) {
        validate(amount);
        processPayment(amount); // the "how" is hidden, implemented differently by subclasses
        sendReceipt();
    }

    private void validate(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Invalid amount");
    }

    protected abstract void processPayment(double amount); // subclass MUST define "how"

    private void sendReceipt() {
        System.out.println("Receipt sent.");
    }
}

class CreditCardProcessor extends PaymentProcessor {
    protected void processPayment(double amount) {
        System.out.println("Charging $" + amount + " to credit card via bank API...");
    }
}

class UpiProcessor extends PaymentProcessor {
    protected void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via UPI gateway...");
    }
}

public class Main {
    public static void main(String[] args) {
        PaymentProcessor p = new UpiProcessor();
        p.pay(250.0); // user just calls pay() — doesn't care HOW it works
    }
}
```

**Interview Q&A:**
- Q: Abstraction vs Encapsulation? → Abstraction hides **complexity/design** (focus: "what to show"), Encapsulation hides **data** (focus: "how to protect/restrict access"). Abstraction is achieved via interfaces/abstract classes; encapsulation via access modifiers.
- Q: Real-world example? → A car's steering wheel/pedals (interface) hide the engine's internal combustion complexity (implementation).

---

### 3.3 Inheritance

**Definition:** A mechanism where a new class (**subclass/child**) acquires the fields and methods of an existing class (**superclass/parent**), using the `extends` keyword.

**Problem it solves:** Without inheritance, common code across related classes (e.g., `Dog`, `Cat`, `Bird` all being `Animal`s) gets **duplicated**. Duplicated code means duplicated bugs and higher maintenance cost.

**Benefit:** Code reusability, establishes an IS-A relationship, supports polymorphism, allows extending behavior without modifying existing tested code (Open/Closed Principle).

```java
class Animal {
    protected String name;

    Animal(String name) {
        this.name = name;
    }

    void eat() {
        System.out.println(name + " is eating.");
    }

    void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// Dog IS-A Animal — reuses eat()/sleep(), adds its own behavior
class Dog extends Animal {
    Dog(String name) {
        super(name); // calling parent constructor
    }

    void bark() {
        System.out.println(name + " is barking.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog("Rex");
        d.eat();   // inherited from Animal
        d.bark();  // Dog's own method
    }
}
```

**Java supports:**
- Single inheritance (class extends one class)
- Multilevel inheritance (A → B → C)
- Hierarchical inheritance (one parent, many children)
- **NOT multiple inheritance of classes** (diamond problem) — but multiple interfaces are allowed.

**Interview Q&A:**
- Q: Why doesn't Java support multiple class inheritance? → To avoid the **Diamond Problem** — ambiguity when two parent classes have the same method signature and the compiler can't decide which one to inherit.
- Q: Can constructors be inherited? → No. Constructors are not inherited, but the child's constructor implicitly/explicitly calls the parent's via `super()`.
- Q: Is "favor composition over inheritance" a rule? → Yes, a well-known design guideline (see [Section 13](#13-composition-vs-inheritance)) — inheritance creates tight coupling to a parent's implementation.

---

### 3.4 Polymorphism

**Definition:** "Many forms" — the ability of an object/method to behave differently based on context. Two types:
1. **Compile-time (Static) Polymorphism** → Method Overloading
2. **Runtime (Dynamic) Polymorphism** → Method Overriding

**Problem it solves:** Without polymorphism, calling code would need giant `if-else`/`switch` blocks checking object type to decide what to do — brittle and requires editing every time a new type is added.

**Benefit:** Enables writing generic code that works with a family of related types, follows Open/Closed Principle (add new subclasses without changing existing calling code).

```java
class Shape {
    double area() {
        return 0;
    }
}

class Circle extends Shape {
    double radius;
    Circle(double radius) { this.radius = radius; }
    @Override
    double area() { return Math.PI * radius * radius; }
}

class Rectangle extends Shape {
    double length, width;
    Rectangle(double l, double w) { length = l; width = w; }
    @Override
    double area() { return length * width; }
}

public class Main {
    // Works with ANY current or future Shape subtype — no if/else needed
    static void printArea(Shape s) {
        System.out.println("Area: " + s.area()); // runtime polymorphism (dynamic dispatch)
    }

    public static void main(String[] args) {
        Shape s1 = new Circle(5);
        Shape s2 = new Rectangle(4, 6);

        printArea(s1); // calls Circle's area()
        printArea(s2); // calls Rectangle's area()

        // Compile-time polymorphism example (overloading) below
        Calculator calc = new Calculator();
        System.out.println(calc.add(2, 3));       // int version
        System.out.println(calc.add(2.5, 3.5));   // double version
    }
}

class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
}
```

**How runtime polymorphism actually works (VTable / Dynamic Method Dispatch):**
The reference type is `Shape`, but JVM looks at the **actual object type at runtime** to decide which `area()` to call. This is called **dynamic method dispatch**.

**Interview Q&A:**
- Q: Overloading vs Overriding — which is polymorphism? → Both are, but overloading = compile-time, overriding = runtime.
- Q: Can `static` methods be overridden? → No, they can only be **hidden** (method hiding), since static methods are resolved at compile-time based on reference type, not runtime object type.
- Q: What is "upcasting"? → Assigning a child object to a parent reference (`Shape s = new Circle();`) — necessary for runtime polymorphism to work.

---

## 4. Constructors

**Definition:** A special block of code, same name as the class, no return type, used to **initialize an object's state** at the moment of creation.

**Why it exists:** Objects often need mandatory initial state (e.g., an `Account` MUST have an owner and starting balance). Without constructors, objects could exist in an incomplete/invalid state, and you'd need a separate `init()` call every time (easy to forget).

```java
class Employee {
    String name;
    double salary;

    // Default constructor
    Employee() {
        this("Unknown", 0.0); // constructor chaining, see Section 11
    }

    // Parameterized constructor
    Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e1 = new Employee();               // uses default
        Employee e2 = new Employee("Asha", 75000);   // uses parameterized
        System.out.println(e1.name + " " + e1.salary);
        System.out.println(e2.name + " " + e2.salary);
    }
}
```

**Interview Q&A:**
- Q: Does Java provide a default constructor automatically? → Yes, ONLY if you don't define any constructor yourself.
- Q: Can constructors be `private`? → Yes — used in **Singleton pattern** to prevent external instantiation.
- Q: Can constructors be inherited or overridden? → No to both.

---

## 5. `this` and `super` Keywords

**Why they exist:** `this` resolves ambiguity between instance fields and parameters/local variables with the same name, and refers to the current object. `super` gives explicit access to the immediate parent class's members/constructor, needed because a subclass can override/hide parent members.

```java
class Vehicle {
    String type = "Generic Vehicle";

    void showType() {
        System.out.println("Vehicle type: " + type);
    }
}

class Car extends Vehicle {
    String type = "Car"; // shadows parent's field

    Car(String type) {
        this.type = type; // 'this' -> refers to THIS object's field, disambiguates from param
    }

    void showBoth() {
        System.out.println("Child type (this): " + this.type);
        System.out.println("Parent type (super): " + super.type); // accesses hidden parent field
        super.showType(); // calls parent's method version explicitly
    }
}

public class Main {
    public static void main(String[] args) {
        Car c = new Car("Sedan");
        c.showBoth();
    }
}
```

**Interview Q&A:**
- Q: Can `this()` and `super()` be used in the same constructor? → No — both must be the FIRST statement, so only one can be used.
- Q: What happens if you don't call `super()` explicitly? → Java inserts an implicit no-arg `super()` call automatically.

---

## 6. Access Modifiers

**Why they exist:** To control the **visibility/scope** of classes, fields, and methods — a core enabler of encapsulation. Without them, everything would be globally accessible, breaking data hiding entirely.

| Modifier | Same Class | Same Package | Subclass (diff package) | Everywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| default (no modifier) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

```java
package bank;

public class Account {
    private double balance;      // only within Account class
    protected String accType;    // package + subclasses
    public String owner;         // accessible everywhere
    String branch;                // default: only within 'bank' package
}
```

**Interview Q&A:**
- Q: Why is `private` the "safest default" for fields? → Principle of least privilege — expose only what's necessary; you can always widen access later, but narrowing it later breaks existing callers.

---

## 7. `static` Keyword

**Definition:** A member that belongs to the **class itself**, not to any individual object — shared across all instances, and can be accessed without creating an object.

**Why it exists:** Some data/behavior is inherently **shared** and not tied to a specific object's state — e.g., counting total objects created, utility/helper methods (`Math.sqrt()`), or constants. Making every instance carry its own copy would waste memory and be logically wrong.

```java
class Counter {
    static int totalObjectsCreated = 0; // shared across ALL instances, one copy in memory

    Counter() {
        totalObjectsCreated++; // every new object increments the SAME variable
    }

    static void printCount() { // static method: can be called without an object
        System.out.println("Total created: " + totalObjectsCreated);
    }
}

public class Main {
    public static void main(String[] args) {
        new Counter();
        new Counter();
        new Counter();
        Counter.printCount(); // called on class, not on an instance -> "Total created: 3"
    }
}
```

**Interview Q&A:**
- Q: Can static methods access instance (non-static) variables? → No — static methods belong to the class and run before any object may exist, so they can't reference per-object state directly.
- Q: What's a static block used for? → One-time class-level initialization logic that runs when the class is loaded (e.g., loading config/constants).
- Q: Can we override static methods? → No, only **hide** them (see Section 3.4).

---

## 8. `final` Keyword

**Why it exists:** To create **immutability and guarantees** — sometimes you WANT to prevent a variable from being reassigned, a method from being overridden, or a class from being extended (for safety, security, or design intent).

```java
final class ImmutablePoint {  // final class -> cannot be subclassed (e.g., like String)
    private final int x;      // final field -> must be assigned once (in constructor), never changed
    private final int y;

    ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    int getX() { return x; }
    int getY() { return y; }

    // final method -> subclasses (if allowed) cannot override this, guaranteeing behavior
    final void printCoordinates() {
        System.out.println("(" + x + ", " + y + ")");
    }
}
```

**Why immutability matters (real interview follow-up):** Immutable objects are **thread-safe by default** (no synchronization needed since state can't change), easier to reason about, and safe to share/cache (like `String` and `Integer` in Java).

**Interview Q&A:**
- Q: Why is `String` immutable in Java? → Security (used in class loading, network connections), thread-safety, hashcode caching (used heavily as `HashMap` keys), and String pool optimization.
- Q: Can a `final` reference variable's object be modified? → Yes! `final` only prevents *reassigning the reference*, not mutating the object it points to (unless the object itself is immutable).

---

## 9. Abstract Classes vs Interfaces

**Why abstract classes exist:** To provide a **partial implementation** — some common code shared by subclasses, plus a "contract" of methods each subclass MUST implement differently. Prevents instantiating a class that only makes sense as a base/template (e.g., you shouldn't be able to create a generic `Shape` object — only concrete shapes).

**Why interfaces exist:** To define a **pure contract** (what a class CAN do) without dictating HOW, allowing unrelated classes to share capability (e.g., `Bird` and `Airplane` both `Flyable`, despite having nothing else in common) — solving the "multiple inheritance of type" problem since a class can implement many interfaces.

```java
// Abstract class: partial implementation + shared state
abstract class Employee {
    String name;
    Employee(String name) { this.name = name; }

    void checkIn() { System.out.println(name + " checked in."); } // shared, concrete
    abstract double calculateSalary(); // must be implemented by each subclass
}

class FullTimeEmployee extends Employee {
    double monthlySalary;
    FullTimeEmployee(String name, double s) { super(name); monthlySalary = s; }
    double calculateSalary() { return monthlySalary; }
}

// Interface: pure contract, can be implemented by unrelated classes
interface Payable {
    void processPayment(double amount); // implicitly public abstract
}

interface Taxable {
    double calculateTax(double amount);
}

// A class can implement MULTIPLE interfaces (solves multiple inheritance problem)
class Contractor extends Employee implements Payable, Taxable {
    Contractor(String name) { super(name); }
    double calculateSalary() { return 5000; }
    public void processPayment(double amount) {
        System.out.println("Paid $" + amount + " to contractor " + name);
    }
    public double calculateTax(double amount) {
        return amount * 0.1;
    }
}
```

| Feature | Abstract Class | Interface |
|---|---|---|
| Method bodies | Can have concrete + abstract methods | Only abstract (until Java 8: `default`/`static` methods allowed) |
| Fields | Any type (instance state allowed) | Only `public static final` constants |
| Multiple inheritance | ❌ (extend only 1) | ✅ (implement many) |
| Constructors | ✅ Yes | ❌ No |
| When to use | Shared code + "IS-A" strong relationship | Shared capability across unrelated types |

**Interview Q&A:**
- Q: Since Java 8 added `default` methods to interfaces, are abstract classes obsolete? → No — abstract classes still allow instance state (fields) and constructors, which interfaces cannot have.
- Q: Real-world analogy? → Interface = a job posting's requirements list ("must know Java, must know SQL") — anyone qualifying can apply regardless of background. Abstract class = a family template — you inherit shared traits AND must define your own unique ones.

---

## 10. Method Overloading vs Overriding

**Why overloading exists:** Lets you use the **same, intuitive method name** for conceptually the same operation with different input types/counts, instead of forcing awkward names like `addInt()`, `addDouble()`.

**Why overriding exists:** Lets a subclass **redefine inherited behavior** to be more specific, which is the foundation of runtime polymorphism.

```java
class OverloadDemo {
    // Overloading: same name, different parameter list, resolved at COMPILE time
    void print(int i)    { System.out.println("int: " + i); }
    void print(String s) { System.out.println("String: " + s); }
    void print(int i, int j) { System.out.println("two ints: " + i + "," + j); }
}

class Bird {
    void sound() { System.out.println("Some generic bird sound"); }
}

class Parrot extends Bird {
    @Override // Overriding: same signature, resolved at RUNTIME based on actual object
    void sound() { System.out.println("Parrot says: Hello!"); }
}
```

| Aspect | Overloading | Overriding |
|---|---|---|
| Binding | Compile-time (static) | Runtime (dynamic) |
| Location | Same class (or subclass adding new variant) | Parent-child relationship |
| Signature | Must differ (params) | Must be IDENTICAL |
| Return type | Can differ freely | Must be same or covariant |
| Access modifier | No restriction | Cannot reduce visibility |

**Interview Q&A:**
- Q: Can you overload by changing only the return type? → No — return type alone doesn't make a valid overload; the compiler can't tell them apart from the call site.
- Q: Can a private/static method be overridden? → No — only public/protected instance methods participate in dynamic dispatch.
- Q: What is "covariant return type"? → An overriding method may return a **subtype** of the original return type.

---

## 11. Constructor Chaining & Initialization Order

**Why it exists:** To avoid duplicating initialization logic across multiple constructors — one constructor can delegate to another (`this(...)`) or to the parent's (`super(...)`).

```java
class A {
    A() {
        System.out.println("1. A's constructor");
    }
}

class B extends A {
    static { System.out.println("0. B's static block (runs ONCE, at class load)"); }
    { System.out.println("2. B's instance initializer block"); }

    B() {
        // implicit super() call happens FIRST, even though not written
        System.out.println("3. B's constructor body");
    }
}

public class Main {
    public static void main(String[] args) {
        new B();
        // Order: static block -> parent constructor -> instance init block -> constructor body
    }
}
```

**Actual object creation order (very common interview question):**
1. Static blocks/variables of parent, then child (only once per class, at class loading)
2. Parent class constructor (via implicit/explicit `super()`)
3. Instance variable initializers & instance blocks of current class (top to bottom)
4. Constructor body of current class

**Interview Q&A:**
- Q: Why must `super()`/`this()` be the first line? → To guarantee the parent object is fully constructed before the child adds its own state — you can't build a house's second floor before the first.

---

## 12. Object Class Methods

Every class in Java implicitly extends `Object`. The most important overridable methods:

**Why `equals()` and `hashCode()` exist:** By default, `equals()` checks reference equality (same memory address) via `==`. But often we want **logical/value equality** (two different `Point(1,2)` objects should be "equal"). `hashCode()` must be consistent with `equals()` because hash-based collections (`HashMap`, `HashSet`) use the hash code to locate a bucket, then `equals()` to confirm the exact match — breaking this contract causes silent bugs (duplicates in a `Set`, lookups failing).

```java
import java.util.Objects;

class Point {
    int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Point)) return false;
        Point p = (Point) obj;
        return x == p.x && y == p.y; // value-based comparison
    }

    @Override
    public int hashCode() {
        return Objects.hash(x, y); // MUST be equal for equal objects (the contract)
    }

    @Override
    public String toString() { // default Object.toString() prints unreadable "Point@1a2b3c"
        return "Point(" + x + ", " + y + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        Point p1 = new Point(1, 2);
        Point p2 = new Point(1, 2);

        System.out.println(p1 == p2);       // false -> different objects in memory
        System.out.println(p1.equals(p2));  // true  -> same logical value
        System.out.println(p1);             // Point(1, 2) -> readable, thanks to toString()

        java.util.Set<Point> set = new java.util.HashSet<>();
        set.add(p1);
        set.add(p2);
        System.out.println(set.size()); // 1 -> because equals()+hashCode() both overridden correctly
    }
}
```

**Interview Q&A:**
- Q: The `equals`/`hashCode` contract? → (1) if `a.equals(b)` is true, `a.hashCode() == b.hashCode()` MUST be true. (2) The reverse is NOT required — different objects CAN share a hash code (collision), `equals()` then breaks the tie.
- Q: What breaks if you override `equals()` but not `hashCode()`? → The object may not be found in a `HashSet`/`HashMap` even though a logically-equal object was inserted, because it lands in the wrong bucket.

---

## 13. Composition vs Inheritance

**The problem inheritance can cause:** Inheritance is the **tightest form of coupling** in OOP — a subclass depends on its parent's internal implementation, and any change to the parent can silently break every subclass ("fragile base class problem"). It also only allows a single IS-A hierarchy.

**Why composition exists:** "HAS-A" relationship — build complex objects by combining simpler ones as fields, exposing only what's needed. More flexible: behavior can be swapped at runtime, and you avoid deep, fragile hierarchies.

```java
// BAD: inheritance misuse — a Stack "IS-A" ArrayList? Not really conceptually correct,
// and it exposes ArrayList methods (add(index,...), remove(index)) that break stack invariants.
class BadStack extends java.util.ArrayList<Integer> {
    void push(int val) { add(val); }
    int pop() { return remove(size() - 1); }
}

// GOOD: composition — Stack HAS-A List internally, exposes ONLY stack operations
class GoodStack {
    private java.util.List<Integer> items = new java.util.ArrayList<>(); // composed, hidden

    void push(int val) { items.add(val); }
    int pop() {
        if (items.isEmpty()) throw new RuntimeException("Stack empty");
        return items.remove(items.size() - 1);
    }
    boolean isEmpty() { return items.isEmpty(); }
    // NOTE: caller cannot do stack.add(5, x) — that operation simply doesn't exist here
}

public class Main {
    public static void main(String[] args) {
        GoodStack s = new GoodStack();
        s.push(10);
        s.push(20);
        System.out.println(s.pop()); // 20
    }
}
```

**Interview Q&A:**
- Q: "Favor composition over inheritance" — why? → Composition gives loose coupling, better encapsulation (internal object fully hidden), flexibility to change behavior at runtime (e.g., swap `ArrayList` for `LinkedList` internally without callers noticing), and avoids fragile base-class issues.
- Q: When SHOULD you use inheritance? → When there's a genuine, stable IS-A relationship and you want polymorphic substitutability (Liskov Substitution Principle holds).

---

## 14. Association, Aggregation, Composition

**Why these distinctions exist:** To precisely describe **how strongly** two objects' lifecycles are tied together when modeling relationships — critical for good class design.

- **Association:** General "uses-a" relationship. Objects are independent; e.g., `Teacher` and `Student` — related, but neither owns the other.
- **Aggregation:** "HAS-A" but **weak ownership** — the child can exist independently of the parent. (e.g., `Department` has `Employees`, but Employees exist even if the Department is dissolved.)
- **Composition:** "HAS-A" with **strong ownership** — the child's lifecycle is bound to the parent; if parent is destroyed, so is the child. (e.g., a `House` has `Rooms` — a Room cannot exist without its House.)

```java
// Aggregation: Employee can exist without Department (weak ownership)
class Employee {
    String name;
    Employee(String name) { this.name = name; }
}

class Department {
    String deptName;
    java.util.List<Employee> employees; // passed in from outside, NOT created here

    Department(String name, java.util.List<Employee> employees) {
        this.deptName = name;
        this.employees = employees; // Department doesn't "own" their lifecycle
    }
}

// Composition: Room CANNOT exist without House (strong ownership)
class Room {
    String type;
    Room(String type) { this.type = type; }
}

class House {
    private final java.util.List<Room> rooms = new java.util.ArrayList<>();

    House() {
        rooms.add(new Room("Living Room")); // Room is created INSIDE House
        rooms.add(new Room("Bedroom"));      // if House object dies, Rooms die with it
    }
}
```

**Interview Q&A:**
- Q: How to identify composition vs aggregation in code? → Ask: "Is the child object created and destroyed inside the parent's constructor/lifecycle, with no external reference?" If yes → composition. If the child is passed in externally and can outlive the parent → aggregation.

---

## 15. Coupling & Cohesion

**Why this matters:** These are the two metrics that determine how maintainable your OOP design is.

- **Coupling** = how much one class depends/knows about another. **LOW coupling is the goal** — changes in one class shouldn't ripple through the whole system.
- **Cohesion** = how focused a single class's responsibilities are. **HIGH cohesion is the goal** — a class should do ONE well-defined job (Single Responsibility Principle).

```java
// LOW cohesion, HIGH coupling (BAD): one class does everything, tightly tied to DB/Email specifics
class UserManagerBad {
    void createUser(String name) { /* insert into MySQL directly */ }
    void sendWelcomeEmail(String email) { /* SMTP code directly here */ }
    void generateInvoicePdf(String userId) { /* PDF generation logic here too */ }
    // One class doing user mgmt + email + billing = LOW cohesion. Hard to change one without risk to others.
}

// HIGH cohesion, LOW coupling (GOOD): each class has ONE job, connected via interfaces
interface EmailService { void send(String to, String msg); }
interface UserRepository { void save(String name); }

class UserService { // depends on ABSTRACTIONS, not concrete implementations -> low coupling
    private final UserRepository repo;
    private final EmailService emailService;

    UserService(UserRepository repo, EmailService emailService) {
        this.repo = repo;
        this.emailService = emailService;
    }

    void createUser(String name, String email) {
        repo.save(name);
        emailService.send(email, "Welcome, " + name + "!");
    }
}
```

**Interview Q&A:**
- Q: How do you achieve low coupling in practice? → Program to interfaces, use Dependency Injection, avoid classes reaching into other classes' internals.

---

## 16. SOLID Principles

**Why SOLID exists:** A set of 5 guidelines (Robert C. Martin) that, applied together, produce OOP code that is easy to **extend, maintain, and test** — directly combats the most common causes of "legacy code that nobody wants to touch."

### S — Single Responsibility Principle
*A class should have only ONE reason to change.*
```java
// BAD: Invoice does both business logic AND printing (2 reasons to change)
class InvoiceBad {
    double calculateTotal() { return 100.0; }
    void printInvoice() { System.out.println("Printing..."); }
}

// GOOD: split responsibilities
class Invoice {
    double calculateTotal() { return 100.0; }
}
class InvoicePrinter {
    void print(Invoice invoice) { System.out.println("Total: " + invoice.calculateTotal()); }
}
```

### O — Open/Closed Principle
*Open for extension, closed for modification.*
```java
// BAD: must edit this method every time a new shape is added
double areaBad(Object shape) {
    if (shape instanceof Circle) { /* ... */ }
    else if (shape instanceof Rectangle) { /* ... */ } // keeps growing, risky edits
    return 0;
}

// GOOD: new shapes extend Shape; no existing code touched (see Section 3.4 example)
abstract class Shape { abstract double area(); }
```

### L — Liskov Substitution Principle
*Subtypes must be substitutable for their base type without breaking correctness.*
```java
// BAD: classic violation — Square "IS-A" Rectangle mathematically, but not behaviorally
class Rectangle {
    protected int width, height;
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    int area() { return width * height; }
}
class Square extends Rectangle {
    @Override void setWidth(int w) { width = w; height = w; }  // breaks caller's expectations!
    @Override void setHeight(int h) { width = h; height = h; }
}
// A method that does rect.setWidth(5); rect.setHeight(10); expects area()==50 — Square breaks this.
```

### I — Interface Segregation Principle
*Don't force classes to implement methods they don't need.*
```java
// BAD: fat interface forces unrelated implementations
interface WorkerBad { void work(); void eat(); } // a Robot shouldn't need eat()!

// GOOD: split into focused interfaces
interface Workable { void work(); }
interface Eatable { void eat(); }
class Robot implements Workable { public void work() { System.out.println("Working..."); } }
class Human implements Workable, Eatable {
    public void work() { System.out.println("Working..."); }
    public void eat() { System.out.println("Eating..."); }
}
```

### D — Dependency Inversion Principle
*Depend on abstractions, not concrete implementations.*
```java
// BAD: high-level UserService directly depends on a concrete low-level MySQLDatabase
class MySQLDatabase { void save(String data) { /* ... */ } }
class UserServiceBad {
    MySQLDatabase db = new MySQLDatabase(); // tightly coupled — can't swap DB easily, hard to test
}

// GOOD: depend on abstraction, inject the implementation (same pattern as Section 15)
interface Database { void save(String data); }
class MySQLDatabase2 implements Database { public void save(String data) { /* ... */ } }
class UserServiceGood {
    private final Database db; // depends on interface
    UserServiceGood(Database db) { this.db = db; } // implementation injected from outside
}
```

**Interview Q&A:**
- Q: Which SOLID principle do people violate most in interviews' "design a system" questions? → SRP and OCP — candidates cram too much logic into one class or use long if-else chains for type-based behavior instead of polymorphism.

---

## 17. Design Patterns

*(OOP interviews almost always test these — they are direct applications of the 4 pillars + SOLID.)*

### Singleton — ensure only ONE instance of a class exists
**Why:** Some resources should have exactly one shared instance (e.g., a config manager, logger, connection pool) to avoid conflicting state or wasted resources.
```java
class ConfigManager {
    private static ConfigManager instance; // single shared instance
    private ConfigManager() {} // private constructor -> blocks external "new"

    public static synchronized ConfigManager getInstance() {
        if (instance == null) {
            instance = new ConfigManager(); // created lazily, only once
        }
        return instance;
    }
}
```

### Factory — delegate object creation logic to a dedicated method/class
**Why:** Decouples the client code from concrete class names, so adding new types doesn't require touching every place that creates objects (supports OCP).
```java
interface Notification { void notifyUser(); }
class EmailNotification implements Notification {
    public void notifyUser() { System.out.println("Sending Email"); }
}
class SmsNotification implements Notification {
    public void notifyUser() { System.out.println("Sending SMS"); }
}

class NotificationFactory {
    static Notification create(String type) {
        return switch (type) {
            case "EMAIL" -> new EmailNotification();
            case "SMS" -> new SmsNotification();
            default -> throw new IllegalArgumentException("Unknown type");
        };
    }
}
// Client: NotificationFactory.create("EMAIL").notifyUser();  -- doesn't know concrete class
```

### Strategy — swap algorithms/behavior at runtime via composition
**Why:** Avoids giant if-else chains for behavior selection; each algorithm is isolated, testable, and interchangeable — a direct application of "favor composition + program to interfaces."
```java
interface PaymentStrategy { void pay(int amount); }
class CardPayment implements PaymentStrategy {
    public void pay(int amount) { System.out.println("Paid " + amount + " via Card"); }
}
class PayPalPayment implements PaymentStrategy {
    public void pay(int amount) { System.out.println("Paid " + amount + " via PayPal"); }
}
class ShoppingCart {
    private PaymentStrategy strategy; // composed, injected -> swappable at runtime
    ShoppingCart(PaymentStrategy strategy) { this.strategy = strategy; }
    void checkout(int amount) { strategy.pay(amount); }
}
```

### Observer — notify dependents automatically when state changes
**Why:** Decouples the "subject" from its "listeners" — the subject doesn't need to know concrete listener types, just that they implement a common interface. Basis of GUI event handling, pub-sub systems.
```java
import java.util.*;
interface Observer { void update(String event); }

class EventPublisher {
    private List<Observer> observers = new ArrayList<>();
    void subscribe(Observer o) { observers.add(o); }
    void publish(String event) {
        for (Observer o : observers) o.update(event); // notify all, without knowing their type
    }
}
class LoggerObserver implements Observer {
    public void update(String event) { System.out.println("Logged: " + event); }
}
```

**Interview Q&A:**
- Q: Which pillar does Strategy pattern demonstrate best? → Polymorphism + composition working together.
- Q: Singleton — thread safety concern? → The naive lazy version above has a race condition without `synchronized`; better production approaches use the **Bill Pugh Singleton (static inner class)** or an `enum` singleton.

---

## 18. Common OOP Interview Coding Problems

### Problem 1: Design a Parking Lot (classic OOD question)
**Ask:** Model a parking lot with multiple spot types (Car, Bike) that assigns spots and frees them.

```java
enum VehicleType { CAR, BIKE }

abstract class Vehicle {
    String licensePlate;
    VehicleType type;
    Vehicle(String plate, VehicleType type) { this.licensePlate = plate; this.type = type; }
}
class Car extends Vehicle { Car(String p) { super(p, VehicleType.CAR); } }
class Bike extends Vehicle { Bike(String p) { super(p, VehicleType.BIKE); } }

class ParkingSpot {
    int spotNumber;
    VehicleType allowedType;
    boolean isFree = true;
    Vehicle parkedVehicle;

    ParkingSpot(int number, VehicleType type) { spotNumber = number; allowedType = type; }

    boolean canFit(Vehicle v) { return isFree && v.type == allowedType; }

    void park(Vehicle v) {
        if (!canFit(v)) throw new IllegalStateException("Spot cannot fit this vehicle");
        parkedVehicle = v;
        isFree = false;
    }
    void vacate() { parkedVehicle = null; isFree = true; }
}

class ParkingLot {
    private java.util.List<ParkingSpot> spots = new java.util.ArrayList<>();

    ParkingLot(java.util.List<ParkingSpot> spots) { this.spots = spots; }

    // Encapsulation: caller doesn't know HOW we find the spot
    ParkingSpot parkVehicle(Vehicle v) {
        for (ParkingSpot spot : spots) {
            if (spot.canFit(v)) {
                spot.park(v);
                System.out.println(v.licensePlate + " parked at spot " + spot.spotNumber);
                return spot;
            }
        }
        throw new RuntimeException("No available spot for " + v.type);
    }
}

public class Main {
    public static void main(String[] args) {
        java.util.List<ParkingSpot> spots = java.util.List.of(
            new ParkingSpot(1, VehicleType.CAR),
            new ParkingSpot(2, VehicleType.BIKE)
        );
        ParkingLot lot = new ParkingLot(spots);
        lot.parkVehicle(new Car("KA-01-1234"));
        lot.parkVehicle(new Bike("KA-02-5678"));
    }
}
```
**Concepts tested:** abstraction (`Vehicle`), encapsulation (`ParkingSpot` protects its own state), polymorphism (works for any `Vehicle` subtype), SRP (each class one job).

---

### Problem 2: Immutable class design
**Ask:** Design a fully immutable `Employee` class (common "explain immutability" interview task).

```java
final class ImmutableEmployee { // final -> cannot be subclassed to add mutability
    private final String name;
    private final java.util.List<String> skills; // mutable field type - needs defensive copy!

    ImmutableEmployee(String name, java.util.List<String> skills) {
        this.name = name;
        this.skills = new java.util.ArrayList<>(skills); // DEFENSIVE COPY on the way in
    }

    String getName() { return name; }

    java.util.List<String> getSkills() {
        return java.util.Collections.unmodifiableList(skills); // defensive copy/wrap on the way out
    }
}
```
**Why the defensive copy matters:** Without it, an outside caller who holds a reference to the original list (or the returned list) could mutate the "immutable" object's internal state — a very common interview trick question.

---

### Problem 3: Diamond Problem demonstration with interfaces
```java
interface Amphibious { default void move() { System.out.println("Amphibious move"); } }
interface Fast { default void move() { System.out.println("Fast move"); } }

// Class implementing 2 interfaces with the SAME default method -> MUST resolve manually
class AmphibiousCar implements Amphibious, Fast {
    @Override
    public void move() {
        Amphibious.super.move(); // explicitly choosing which one to defer to
        Fast.super.move();
        System.out.println("AmphibiousCar's own custom move");
    }
}
```
**Why this matters:** Shows Java DOES have a mini diamond-problem with default interface methods (Java 8+), and forces the implementing class to resolve the conflict explicitly — the compiler will NOT guess for you.

---

## 19. Rapid-Fire Q&A Cheat Sheet

| Q | A |
|---|---|
| 4 pillars of OOP? | Encapsulation, Abstraction, Inheritance, Polymorphism |
| Encapsulation vs Abstraction? | Encapsulation hides DATA; Abstraction hides IMPLEMENTATION COMPLEXITY |
| Can we instantiate an abstract class? | No, but you can hold a reference to it pointing to a subclass object |
| Can an interface have a constructor? | No |
| Default value of object references? | `null` |
| IS-A vs HAS-A? | Inheritance vs Composition/Aggregation |
| Method overloading resolved when? | Compile-time |
| Method overriding resolved when? | Runtime |
| Can constructors be `final`, `static`, or `abstract`? | No to all three |
| What is the Diamond Problem? | Ambiguity from multiple inheritance of the SAME method from 2 parents; Java avoids it for classes, mitigates for interface default methods via explicit resolution |
| Why favor interfaces for API design? | Loose coupling, multiple implementation, easier mocking/testing |
| What is "tight coupling"? | High interdependency between classes — a change in one forces changes in another |
| Why is `Object` the root of all classes? | Provides a common baseline (equals, hashCode, toString, getClass) so ANY object can be used generically (e.g., in collections) |
| Difference between abstract method and interface method (pre-Java 8)? | None functionally — both implicitly abstract, but abstract class methods can be `protected`/`private`+concrete too |
| What does `instanceof` check? | Whether an object is an instance of a given class/interface at runtime — used carefully to avoid violating polymorphism (relying on it too much is a code smell) |
| Deep copy vs Shallow copy? | Shallow copies references to nested objects (shared mutable state); Deep copy recursively duplicates nested objects too |

---

### Final Interview Tips
1. **Always explain the "why"** before the "what" — interviewers value understanding the *problem being solved*, not memorized definitions.
2. **Draw the connection between pillars and SOLID** — e.g., "This uses polymorphism, which is what makes the Open/Closed Principle possible here."
3. **When asked to design something (Parking Lot, Elevator, Library System)** — start with nouns → classes, verbs → methods, then apply encapsulation + polymorphism naturally.
4. **Always mention trade-offs** — e.g., "I used inheritance here because X, but I considered composition since it would reduce coupling."
