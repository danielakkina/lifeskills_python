


# SOLID Principles in Object-Oriented Programming

SOLID is a set of **five design principles** in object-oriented programming that help developers write maintainable, flexible, and scalable software.  
They were introduced by **Robert C. Martin (Uncle Bob)** and are widely used in modern software development.

---

## 1. **Single Responsibility Principle (SRP)**

**Definition:**  
A class should have **only one reason to change**.  
In other words, a class should do only one thing (single responsibility).

### Example ( Wrong - violates SRP)
```java
class Report {
    public String generateReport() {
        return "Report Data";
    }

    public void saveToFile(String data) {
        // Save data to file
    }
}
````

The class is handling both **report generation** and **file saving** (two responsibilities).

### Example (Correct - follows SRP)

```java
class Report {
    public String generateReport() {
        return "Report Data";
    }
}

class FileSaver {
    public void saveToFile(String data) {
        // Save data to file
    }
}
```

 Each class now has a **single responsibility**.

---

## 2. **Open/Closed Principle (OCP)**

**Definition:**
Software entities (classes, modules, functions) should be **open for extension but closed for modification**.
This means you should be able to add new functionality without changing existing code.

### Example ( Wrong - violates OCP)

```java
class AreaCalculator {
    public double calculate(Object shape) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return Math.PI * c.radius * c.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.length * r.breadth;
        }
        return 0;
    }
}
```

Adding a new shape requires modifying the `AreaCalculator` class.

### Example ( Correct - follows OCP)

```java
interface Shape {
    double area();
}

class Circle implements Shape {
    double radius;
    Circle(double r) { radius = r; }
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle implements Shape {
    double length, breadth;
    Rectangle(double l, double b) { length = l; breadth = b; }
    public double area() {
        return length * breadth;
    }
}

class AreaCalculator {
    public double calculate(Shape shape) {
        return shape.area();
    }
}
```

New shapes can be added without modifying `AreaCalculator`.

---

## 3. **Liskov Substitution Principle (LSP)**

**Definition:**
Subtypes must be substitutable for their base types.
That means, if `S` is a subclass of `T`, objects of type `T` should be replaceable with objects of type `S` without breaking the program.

### Example (Wrong - violates LSP)

```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches cannot fly!");
    }
}
```

`Ostrich` is a `Bird`, but cannot behave like one (violates LSP).

### Example (Correct - follows LSP)

```java
abstract class Bird { }

class FlyingBird extends Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Ostrich extends Bird {
    public void run() {
        System.out.println("Running");
    }
}
```

Now `FlyingBird` and `Ostrich` behave correctly without violating expectations.

---

## 4. **Interface Segregation Principle (ISP)**

**Definition:**
Clients should not be forced to implement interfaces they don’t use.
It’s better to have many small, specific interfaces than one large, general-purpose one.

### Example (Wrong - violates ISP)

```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {
        System.out.println("Robot working");
    }
    public void eat() {
        // Robots don’t eat – bad design
    }
}
```

### Example (Correct - follows ISP)

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() {
        System.out.println("Human working");
    }
    public void eat() {
        System.out.println("Human eating");
    }
}

class Robot implements Workable {
    public void work() {
        System.out.println("Robot working");
    }
}
```

Now classes implement only the interfaces they actually need.

---

## 5. **Dependency Inversion Principle (DIP)**

**Definition:**
High-level modules should not depend on low-level modules.
Both should depend on **abstractions (interfaces)**.
Abstractions should not depend on details; details should depend on abstractions.

### Example (Wrong - violates DIP)

```java
class MySQLDatabase {
    public void connect() {
        System.out.println("Connected to MySQL");
    }
}

class Application {
    private MySQLDatabase db = new MySQLDatabase();
    public void start() {
        db.connect();
    }
}
```

`Application` is tightly coupled to `MySQLDatabase`.

### Example (Correct - follows DIP)

```java
interface Database {
    void connect();
}

class MySQLDatabase implements Database {
    public void connect() {
        System.out.println("Connected to MySQL");
    }
}

class MongoDBDatabase implements Database {
    public void connect() {
        System.out.println("Connected to MongoDB");
    }
}

class Application {
    private Database db;
    Application(Database db) {
        this.db = db;
    }
    public void start() {
        db.connect();
    }
}
```

`Application` depends on abstraction (`Database`), not on a concrete class.

---

# Summary of SOLID Principles

* **S** → Single Responsibility Principle: One class = one responsibility.
* **O** → Open/Closed Principle: Open for extension, closed for modification.
* **L** → Liskov Substitution Principle: Subtypes must be substitutable.
* **I** → Interface Segregation Principle: No forcing unused methods.
* **D** → Dependency Inversion Principle: Depend on abstractions, not concrete implementations.

---

Following SOLID principles makes code:

* Easier to **maintain**
* Easier to **extend**
* More **flexible**
* Less **bug-prone**

```

---


