# Object-Oriented Programming (OOP) in Python

A practical guide to understanding Object-Oriented Programming (OOP) concepts, with definitions, Python examples, real-world analogies, and best practices.

## Table of Contents

1. [Introduction to OOP](#1-introduction-to-oop)
2. [Classes](#2-classes)
3. [Objects](#3-objects)
4. [Attributes and Properties](#4-attributes-and-properties)
5. [Methods](#5-methods)
6. [Constructors](#6-constructors)
7. [Encapsulation](#7-encapsulation)
8. [Abstraction](#8-abstraction)
9. [Inheritance](#9-inheritance)
10. [Polymorphism](#10-polymorphism)
11. [Method Overloading](#11-method-overloading)
12. [Method Overriding](#12-method-overriding)
13. [Interfaces](#13-interfaces)
14. [Abstract Classes](#14-abstract-classes)
15. [Composition](#15-composition)
16. [Aggregation](#16-aggregation)
17. [Association](#17-association)
18. [Dependency](#18-dependency)
19. [Coupling](#19-coupling)
20. [Cohesion](#20-cohesion)
21. [SOLID Principles](#21-solid-principles)
22. [Quick Comparison](#22-quick-comparison)
23. [Best Practices](#23-best-practices)
24. [Summary](#24-summary)

---

## 1. Introduction to OOP

Object-Oriented Programming is a programming paradigm that organizes software around **objects**, which combine data and behavior.

An object generally contains:
- **State:** Data stored in attributes.
- **Behavior:** Operations provided by methods.
- **Identity:** What distinguishes one object from another.

### Real-world example

Consider a banking application.

A bank account has:
- Data: Account holder, account number, balance.
- Behavior: Deposit money, withdraw money, check balance.

In OOP, we can represent a bank account using a class and create individual account objects from it.

### Why use OOP?

- Code reuse through inheritance and composition.
- Better organization of complex applications.
- Controlled access to data.
- Easier testing and maintenance.
- Clear separation of responsibilities.

Python supports OOP, procedural programming, and functional programming. OOP is useful when a problem can be modeled naturally as interacting objects.

---

## 2. Classes

### Definition

A **class** is a blueprint that defines the attributes and methods its objects can have.

A class describes how objects should be structured and what operations they can perform.

### Python example

```python
class Car:
    def start(self):
        print("Car started")

    def stop(self):
        print("Car stopped")
```

Here, `Car` is a class. It defines two methods: `start()` and `stop()`.

### Real-world analogy

A car design is like a class. It describes the structure and functionality of cars without representing one specific car.

### When to use

Use classes when multiple entities share common data and behavior.

Examples:
- `Customer`
- `BankAccount`
- `Employee`
- `Order`
- `Payment`

**Key point:** Defining a class does not automatically create a specific car object.

---

## 3. Objects

### Definition

An **object** is an instance of a class. It is a concrete entity that can hold its own state and use the behavior defined by its class.

### Python example

```python
class Car:
    def __init__(self, brand):
        self.brand = brand

    def start(self):
        print(f"{self.brand} started")


car1 = Car("Toyota")
car2 = Car("Honda")

car1.start()
car2.start()
```

Output:

```text
Toyota started
Honda started
```

`car1` and `car2` are separate instances of the same class, with different brand values.

### Important distinction

| Class | Object |
|---|---|
| Defines a type and behavior | Is an instance of that type |
| Acts as a blueprint | Represents a particular entity |
| Example: `Car` | Example: `Car("Toyota")` |

---

## 4. Attributes and Properties

### Definition

**Attributes** store information associated with an object or class.

Python also provides the `@property` decorator, which allows a method to be accessed like an attribute while implementing validation or computed behavior.

### Types of attributes

1. Instance attributes
2. Class attributes
3. Properties

### Python example

```python
class Employee:
    company = "ABC Technologies"  # Class attribute

    def __init__(self, name, salary):
        self.name = name           # Instance attribute
        self._salary = salary      # Internal-use convention

    @property
    def salary(self):
        return self._salary

    @salary.setter
    def salary(self, value):
        if value < 0:
            raise ValueError("Salary cannot be negative")
        self._salary = value


employee = Employee("Amit", 50000)

print(employee.name)
print(employee.company)
print(employee.salary)

employee.salary = 60000
print(employee.salary)
```

Output:

```text
Amit
ABC Technologies
50000
60000
```

### Explanation

- `name` belongs to each employee object.
- `company` is defined on the class and shared by instances unless overridden.
- `salary` is exposed through a property that validates changes.
- `_salary` indicates that the attribute is intended for internal use; it is not strictly private.

### Why properties matter

Properties let you keep a simple attribute-style interface while adding validation or calculated values without changing how callers use the object.

---

## 5. Methods

### Definition

A **method** is a function defined inside a class. Methods describe behavior associated with a class or its instances.

### Main types of methods

- Instance methods
- Class methods
- Static methods

### Python example

```python
class Employee:
    company = "ABC Technologies"

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def display(self):  # Instance method
        print(self.name, self.salary)

    @classmethod
    def change_company(cls, new_name):
        cls.company = new_name

    @staticmethod
    def is_valid_salary(amount):
        return amount >= 0


employee = Employee("Amit", 50000)

employee.display()

Employee.change_company("XYZ Technologies")

print(Employee.company)
print(Employee.is_valid_salary(50000))
```

### Explanation

| Method type | First parameter | Purpose |
|---|---|---|
| Instance method | `self` | Works with a particular object |
| Class method | `cls` | Works with class-level state |
| Static method | None required | Groups a utility operation with a class |

`self` and `cls` are conventional names, not reserved keywords.

---

## 6. Constructors

### Definition

A constructor is commonly described as the mechanism used to initialize an object when it is created.

In Python, `__init__()` initializes an already-created instance. Technically, `__new__()` creates the instance, while `__init__()` initializes it.

### Python example

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def display(self):
        print(f"Name: {self.name}, Age: {self.age}")


student = Student("Priya", 22)
student.display()
```

Output:

```text
Name: Priya, Age: 22
```

### Constructor with default values

```python
class Student:
    def __init__(self, name, age=18):
        self.name = name
        self.age = age


student1 = Student("Priya", 22)
student2 = Student("Rahul")

print(student1.age)  # 22
print(student2.age)  # 18
```

### Key points

- `__init__()` is called automatically during normal object initialization.
- It commonly initializes instance attributes.
- It can validate input and establish initial state.
- Python does not support multiple `__init__()` definitions in the same class as traditional constructor overloading.

---

## 7. Encapsulation

### Definition

**Encapsulation** combines data and related behavior within a class and provides controlled ways to interact with the object's state.

Python does not enforce private instance attributes in the same way as some languages. It uses naming conventions and name mangling.

### Python example

```python
class BankAccount:
    def __init__(self, balance=0):
        if balance < 0:
            raise ValueError("Initial balance cannot be negative")
        self.__balance = balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self.__balance:
            raise ValueError("Insufficient balance")
        self.__balance -= amount

    def get_balance(self):
        return self.__balance


account = BankAccount(1000)
account.deposit(500)
account.withdraw(200)

print(account.get_balance())
```

Output:

```text
1300
```

### Explanation

The balance is stored in `__balance`. Users interact with it through methods that enforce business rules.

The double underscore triggers name mangling; it is not a security boundary. Python code can still access the mangled attribute.

### Benefits

- Protects object invariants from accidental misuse.
- Centralizes validation.
- Reduces unintended changes to internal state.
- Makes future implementation changes easier.

---

## 8. Abstraction

### Definition

**Abstraction** exposes the essential operations of a component while hiding unnecessary implementation details.

Users need to understand what an operation does, not every internal step required to perform it.

### Python example

```python
class PaymentService:
    def pay(self, amount):
        raise NotImplementedError


class CardPayment(PaymentService):
    def pay(self, amount):
        print(f"Processing card payment of {amount}")


class UpiPayment(PaymentService):
    def pay(self, amount):
        print(f"Processing UPI payment of {amount}")


def checkout(payment_service, amount):
    payment_service.pay(amount)


checkout(CardPayment(), 1000)
checkout(UpiPayment(), 500)
```

The checkout function uses a common operation, `pay()`, without needing to know the internal details of card or UPI payment processing.

For a formal contract enforced by Python, use an abstract base class or a protocol, as explained later.

### Benefits

- Simplifies complex systems.
- Separates interface from implementation.
- Allows implementations to change without changing their callers.

---

## 9. Inheritance

### Definition

**Inheritance** allows a class to derive behavior and attributes from another class.

The original class is the parent or base class. The derived class is the child or subclass.

### Python example

```python
class Employee:
    def __init__(self, name):
        self.name = name

    def work(self):
        print(f"{self.name} is working")


class Developer(Employee):
    def write_code(self):
        print(f"{self.name} is writing code")


developer = Developer("Amit")

developer.work()
developer.write_code()
```

Output:

```text
Amit is working
Amit is writing code
```

### Types of inheritance

- Single inheritance: One parent class.
- Multiple inheritance: More than one parent class.
- Multilevel inheritance: A subclass extends another subclass.
- Hierarchical inheritance: Multiple subclasses share a parent.
- Hybrid inheritance: A combination of inheritance patterns.

### Example of method reuse

A `Developer` is an `Employee`, so inheritance can be appropriate when that relationship is genuine.

Do not use inheritance merely to reuse a few lines of code. Composition is often a better choice when the relationship is not truly an "is-a" relationship.

---

## 10. Polymorphism

### Definition

**Polymorphism** means that the same operation or interface can work with different object types, with each type providing its own behavior.

### Python example

```python
class Dog:
    def speak(self):
        print("Woof")


class Cat:
    def speak(self):
        print("Meow")


class Cow:
    def speak(self):
        print("Moo")


def make_sound(animal):
    animal.speak()


animals = [Dog(), Cat(), Cow()]

for animal in animals:
    make_sound(animal)
```

Output:

```text
Woof
Meow
Moo
```

The `make_sound()` function does not need separate branches for each animal type. It calls the operation supported by each object.

### Forms of polymorphism

- Subtype polymorphism: Different subclasses implement a common interface.
- Duck typing: Python relies on supported behavior rather than requiring a particular declared type.
- Operator overloading: Operators such as `+` can behave differently for different types.

### Why it matters

Polymorphism makes code extensible. A new object type can often be introduced without modifying existing code that uses the shared interface.

---

## 11. Method Overloading

### Definition

**Method overloading** traditionally means defining multiple methods with the same name but different parameter lists.

Python does not support traditional signature-based method overloading by defining multiple methods with the same name in one class. The later definition replaces the earlier one.

### Python approach 1: Default parameters

```python
class Calculator:
    def add(self, a, b=0, c=0):
        return a + b + c


calculator = Calculator()

print(calculator.add(5))
print(calculator.add(5, 10))
print(calculator.add(5, 10, 15))
```

Output:

```text
5
15
30
```

### Python approach 2: Variable-length arguments

```python
class Calculator:
    def add(self, *numbers):
        return sum(numbers)


calculator = Calculator()

print(calculator.add(1, 2))
print(calculator.add(1, 2, 3, 4))
```

Output:

```text
3
10
```

### Python approach 3: `singledispatchmethod`

When behavior must depend on the type of the first non-`self` argument, Python provides `functools.singledispatchmethod`.

```python
from functools import singledispatchmethod


class Formatter:
    @singledispatchmethod
    def format_value(self, value):
        return str(value)

    @format_value.register
    def _(self, value: int):
        return f"Integer: {value}"

    @format_value.register
    def _(self, value: list):
        return f"List with {len(value)} items"


formatter = Formatter()

print(formatter.format_value(10))
print(formatter.format_value([1, 2, 3]))
```

Output:

```text
Integer: 10
List with 3 items
```

### Important distinction

Default parameters and `*args` provide flexible argument handling; they do not automatically create traditional compile-time method overloads.

---

## 12. Method Overriding

### Definition

**Method overriding** occurs when a subclass provides its own implementation of a method inherited from its parent class.

### Python example

```python
class Notification:
    def send(self):
        print("Sending notification")


class EmailNotification(Notification):
    def send(self):
        print("Sending email notification")


class SMSNotification(Notification):
    def send(self):
        print("Sending SMS notification")


notifications = [
    EmailNotification(),
    SMSNotification()
]

for notification in notifications:
    notification.send()
```

Output:

```text
Sending email notification
Sending SMS notification
```

### Using `super()`

```python
class Employee:
    def display(self):
        print("Employee details")


class Manager(Employee):
    def display(self):
        super().display()
        print("Manager details")


Manager().display()
```

Output:

```text
Employee details
Manager details
```

`super()` calls the next implementation in Python's method resolution order. It is particularly useful when extending parent behavior.

### Overloading vs overriding

| Method overloading | Method overriding |
|---|---|
| Same method name with different signatures in languages that support it | Subclass replaces or extends inherited behavior |
| Python uses alternatives such as defaults and `*args` | Supported directly in Python |
| Often discussed as compile-time polymorphism | Commonly associated with runtime polymorphism |

---

## 13. Interfaces

### Definition

An **interface** specifies the operations that an implementation must provide, without requiring callers to know how those operations are implemented.

Python has no dedicated `interface` keyword like Java. Common alternatives include abstract base classes and `typing.Protocol`.

### Python example using `Protocol`

```python
from typing import Protocol


class PaymentProcessor(Protocol):
    def pay(self, amount: float) -> None:
        ...


class CardPayment:
    def pay(self, amount: float) -> None:
        print(f"Paid {amount} using card")


class UpiPayment:
    def pay(self, amount: float) -> None:
        print(f"Paid {amount} using UPI")


def checkout(processor: PaymentProcessor, amount: float) -> None:
    processor.pay(amount)


checkout(CardPayment(), 1000)
checkout(UpiPayment(), 500)
```

The implementations do not need to inherit from `PaymentProcessor`. They simply need to provide compatible behavior.

`Protocol` is primarily a static typing mechanism. Python does not automatically enforce it at runtime.

### When to use interfaces or protocols

- Multiple implementations must support the same operations.
- Components need to be interchangeable.
- You want to reduce dependencies on concrete classes.
- You need clear contracts between modules.

### Java comparison

Java declares interfaces explicitly:

```java
interface PaymentProcessor {
    void pay(double amount);
}

class CardPayment implements PaymentProcessor {
    public void pay(double amount) {
        System.out.println("Card payment: " + amount);
    }
}
```

Use Python's `Protocol` for structural typing or `ABC` when you want explicit inheritance and runtime enforcement of abstract methods.

---

## 14. Abstract Classes

### Definition

An **abstract class** is a class intended to serve as a base for other classes. It can define required methods and provide shared implementation.

Python provides the `abc` module for abstract base classes.

### Python example

```python
from abc import ABC, abstractmethod


class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    def describe(self):
        print("This is a shape")


class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height


rectangle = Rectangle(5, 4)

rectangle.describe()
print(rectangle.area())
```

Output:

```text
This is a shape
20
```

You cannot instantiate `Shape` while its abstract method remains unimplemented.

### Abstract class vs interface

| Abstract class | Interface-like contract |
|---|---|
| Can contain shared implementation and state | Primarily specifies required behavior |
| Can define abstract and concrete methods | Depends on the mechanism used |
| Python `ABC` enforces abstract methods during instantiation | Python `Protocol` supports structural type checking |
| Useful for closely related classes | Useful for interchangeable implementations |

These concepts overlap. The distinction depends on the language and the specific mechanism being used.

---

## 15. Composition

### Definition

**Composition** is a strong whole-part relationship in which an object owns or manages another object as one of its internal components.

It is often used to build complex objects from smaller, reusable objects.

### Python example

```python
class Engine:
    def start(self):
        print("Engine started")


class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car started")


car = Car()
car.start()
```

Output:

```text
Engine started
Car started
```

Here, the `Car` creates and manages its `Engine` in its constructor. This is an illustrative ownership pattern, not a language-enforced lifetime rule.

### Benefits

- Promotes modular design.
- Makes components easier to replace.
- Avoids deep inheritance hierarchies.
- Supports the principle of favoring composition over inheritance.

### When to use

Use composition when an object is built from components that form part of its implementation.

Examples:
- A car has an engine.
- An order has line items.
- A report generator has a formatter.

---

## 16. Aggregation

### Definition

**Aggregation** is a whole-part relationship where the contained object can exist independently of the object that references it.

It is often modeled using shared references to objects created elsewhere.

### Python example

```python
class Teacher:
    def __init__(self, name):
        self.name = name


class Department:
    def __init__(self, name, teachers):
        self.name = name
        self.teachers = teachers


teacher1 = Teacher("Amit")
teacher2 = Teacher("Priya")

department = Department("Computer Science", [teacher1, teacher2])

print(department.name)

for teacher in department.teachers:
    print(teacher.name)
```

The teachers are created independently of the department and can be referenced elsewhere.

### Composition vs aggregation

| Composition | Aggregation |
|---|---|
| Strong ownership relationship | Weaker whole-part relationship |
| Component lifecycle is commonly managed by the whole | Component can exist independently |
| Example: internally owned component | Example: department referencing existing teachers |

**Python does not have special syntax that enforces composition or aggregation.** These are design relationships expressed through how objects are created, shared, and managed.

---

## 17. Association

### Definition

**Association** is a general relationship in which one object knows about or interacts with another object.

Association may be one-to-one, one-to-many, or many-to-many. It does not necessarily imply ownership.

### Python example

```python
class Student:
    def __init__(self, name):
        self.name = name


class Course:
    def __init__(self, title):
        self.title = title

    def enroll(self, student):
        print(f"{student.name} enrolled in {self.title}")


student = Student("Amit")
course = Course("Python Programming")

course.enroll(student)
```

Output:

```text
Amit enrolled in Python Programming
```

The course interacts with the student, but it does not create or own the student's lifecycle.

### Examples

- A doctor treats a patient.
- A customer places an order.
- A student enrolls in a course.
- A manager supervises employees.

Association is a broad relationship. Aggregation and composition are often treated as more specific whole-part forms of association.

---

## 18. Dependency

### Definition

**Dependency** exists when one component temporarily uses another component to perform a task.

A dependency can appear through a function parameter, a local variable, or a call to another component's method.

### Python example

```python
class EmailService:
    def send(self, message):
        print(f"Email sent: {message}")


class OrderService:
    def place_order(self, email_service):
        print("Order placed")
        email_service.send("Your order has been confirmed")


email_service = EmailService()
order_service = OrderService()

order_service.place_order(email_service)
```

`OrderService` depends on an email service to send the notification, but it does not construct that service internally.

### Why dependency matters

- Changes in a dependency can affect the code that uses it.
- Dependencies should be easy to replace during testing.
- External services should generally be accessed through clear contracts.

Passing a dependency into a method or constructor is one way to reduce tight coupling.

---

## 19. Coupling

### Definition

**Coupling** describes how strongly one module depends on another module's implementation or behavior.

Lower coupling generally makes software easier to modify and test.

### Example of tight coupling

```python
class MySQLDatabase:
    def save(self, data):
        print("Saved to MySQL:", data)


class UserService:
    def __init__(self):
        self.database = MySQLDatabase()

    def register_user(self, name):
        self.database.save(name)
```

`UserService` creates a specific database implementation, making replacement and isolated testing more difficult.

### Improved design

```python
class UserService:
    def __init__(self, database):
        self.database = database

    def register_user(self, name):
        self.database.save(name)


class MySQLDatabase:
    def save(self, data):
        print("Saved to MySQL:", data)


class FakeDatabase:
    def save(self, data):
        print("Saved to fake database:", data)


service = UserService(FakeDatabase())
service.register_user("Amit")
```

Output:

```text
Saved to fake database: Amit
```

The service receives its dependency instead of constructing a specific implementation.

### Types of coupling

Commonly discussed forms include:

- Content coupling: One module relies on another module's internal implementation.
- Common coupling: Modules share global state.
- Control coupling: One module passes flags that control another module's internal behavior.
- Data coupling: Modules communicate through the data they need.
- Interface-based dependency: Modules interact through stable contracts rather than concrete implementation details.

These classifications vary across software-design references.

### Best practices

- Depend on clear abstractions where they provide value.
- Avoid unnecessary global state.
- Keep module boundaries explicit.
- Use dependency injection when it improves replaceability and testing.

Low coupling does not mean eliminating all dependencies. Software components must interact to do useful work.

---

## 20. Cohesion

### Definition

**Cohesion** describes how closely the responsibilities of a module or class relate to one another.

High cohesion means a class has a focused, understandable purpose.

### Example of low cohesion

```python
class ApplicationManager:
    def save_user(self):
        print("Saving user")

    def generate_invoice(self):
        print("Generating invoice")

    def send_email(self):
        print("Sending email")

    def create_chart(self):
        print("Creating chart")
```

This class handles unrelated responsibilities, making changes and testing harder.

### Improved design with high cohesion

```python
class UserRepository:
    def save_user(self):
        print("Saving user")


class InvoiceService:
    def generate_invoice(self):
        print("Generating invoice")


class EmailService:
    def send_email(self):
        print("Sending email")


class ChartService:
    def create_chart(self):
        print("Creating chart")
```

Each class now has a clearer responsibility.

### Coupling vs cohesion

| Coupling | Cohesion |
|---|---|
| Measures dependencies between modules | Measures relatedness within a module |
| Prefer fewer unnecessary dependencies | Prefer focused responsibilities |
| High coupling often makes changes risky | High cohesion usually improves maintainability |

A common design goal is **low unnecessary coupling and high cohesion**.

---

## 21. SOLID Principles

SOLID is a set of five object-oriented design principles intended to make software easier to maintain, extend, and test.

- S — Single Responsibility Principle
- O — Open/Closed Principle
- L — Liskov Substitution Principle
- I — Interface Segregation Principle
- D — Dependency Inversion Principle

### 21.1 Single Responsibility Principle (SRP)

**Definition:** A class should have one focused responsibility and one primary reason to change.

Poor design:

```python
class Invoice:
    def calculate_total(self):
        return 1000

    def save_to_database(self):
        print("Saving invoice")

    def send_email(self):
        print("Sending invoice email")
```

Improved design:

```python
class Invoice:
    def calculate_total(self):
        return 1000


class InvoiceRepository:
    def save(self, invoice):
        print("Saving invoice")


class InvoiceNotifier:
    def send(self, invoice):
        print("Sending invoice email")
```

Calculation, persistence, and notification are separated.

Do not split every method into its own class. The objective is to separate distinct responsibilities, not maximize the number of classes.

### 21.2 Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for unnecessary modification.

The idea is to add new behavior without repeatedly rewriting stable existing logic.

```python
from abc import ABC, abstractmethod


class Discount(ABC):
    @abstractmethod
    def calculate(self, amount):
        pass


class RegularDiscount(Discount):
    def calculate(self, amount):
        return amount * 0.05


class PremiumDiscount(Discount):
    def calculate(self, amount):
        return amount * 0.10


def final_price(amount, discount):
    return amount - discount.calculate(amount)


print(final_price(1000, RegularDiscount()))
print(final_price(1000, PremiumDiscount()))
```

A new discount type can be added by implementing `Discount`, without changing `final_price()`.

### 21.3 Liskov Substitution Principle (LSP)

**Definition:** An instance of a subtype should be usable wherever its base type is expected without breaking the program's expected behavior.

Problematic design:

```python
class Bird:
    def fly(self):
        print("Flying")


class Penguin(Bird):
    def fly(self):
        raise NotImplementedError("Penguins cannot fly")
```

A function expecting every `Bird` to fly will fail for a penguin.

A better design separates general bird behavior from flying capability:

```python
class Bird:
    def eat(self):
        print("Eating")


class FlyingBird(Bird):
    def fly(self):
        print("Flying")


class Sparrow(FlyingBird):
    pass


class Penguin(Bird):
    def swim(self):
        print("Swimming")
```

Now, code that requires `fly()` can depend on `FlyingBird` rather than assuming every bird can fly.

LSP is about behavioral compatibility, not merely matching method names.

### 21.4 Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on methods they do not use.

Poor design:

```python
class Machine:
    def print_document(self):
        pass

    def scan_document(self):
        pass

    def fax_document(self):
        pass
```

A basic printer should not be forced to implement scanning and faxing.

Improved design:

```python
from abc import ABC, abstractmethod


class Printer(ABC):
    @abstractmethod
    def print_document(self):
        pass


class Scanner(ABC):
    @abstractmethod
    def scan_document(self):
        pass


class BasicPrinter(Printer):
    def print_document(self):
        print("Printing document")


class MultiFunctionPrinter(Printer, Scanner):
    def print_document(self):
        print("Printing document")

    def scan_document(self):
        print("Scanning document")
```

The interfaces are split according to capability.

### 21.5 Dependency Inversion Principle (DIP)

**Definition:** High-level business logic should depend on abstractions rather than directly depending on low-level implementation details.

```python
from typing import Protocol


class Database(Protocol):
    def save(self, data: str) -> None:
        ...


class UserRepository:
    def __init__(self, database: Database):
        self.database = database

    def save_user(self, name):
        self.database.save(name)


class MySQLDatabase:
    def save(self, data):
        print(f"MySQL saved: {data}")


class InMemoryDatabase:
    def save(self, data):
        print(f"In-memory saved: {data}")


repository = UserRepository(InMemoryDatabase())
repository.save_user("Amit")
```

`UserRepository` depends on the behavior described by `Database`, rather than constructing a concrete database itself.

The example uses structural typing for clarity; Python does not enforce the protocol at runtime by default.

### SOLID summary

| Principle | Main idea |
|---|---|
| SRP | One focused responsibility per class |
| OCP | Extend behavior without repeatedly modifying stable code |
| LSP | Subtypes preserve expected behavior |
| ISP | Keep interfaces focused on required capabilities |
| DIP | Depend on abstractions rather than concrete details |

SOLID principles are guidelines, not absolute laws. Apply them where they reduce complexity rather than adding abstractions without a practical need.

---

## 22. Quick Comparison

| Concept | Main question it answers |
|---|---|
| Class | What structure and behavior do these objects share? |
| Object | What specific instance are we working with? |
| Attributes | What data does this object or class hold? |
| Methods | What can this object or class do? |
| Constructor | How is an object initialized? |
| Encapsulation | How is internal state accessed and protected from misuse? |
| Abstraction | What details can be hidden behind a simpler interface? |
| Inheritance | What behavior can a subclass inherit? |
| Polymorphism | How can the same operation work with different object types? |
| Overloading | How can similar operations accept different argument forms? |
| Overriding | How can a subclass replace inherited behavior? |
| Interface | What behavior must an implementation provide? |
| Abstract class | What common base behavior and requirements should subclasses share? |
| Composition | What components form part of an object? |
| Aggregation | What independent objects does a larger object reference? |
| Association | Which objects know about or interact with each other? |
| Dependency | Which component does another component use? |
| Coupling | How strongly are modules dependent on each other? |
| Cohesion | How closely related are a module's responsibilities? |
| SOLID | How can object-oriented designs be made easier to maintain and extend? |

---

## 23. Best Practices

1. **Use meaningful names.** Prefer `BankAccount` over `DataClass`.
2. **Keep classes focused.** Avoid combining unrelated business responsibilities.
3. **Favor composition when inheritance is not a genuine "is-a" relationship.**
4. **Validate important inputs.** Protect business rules such as non-negative balances.
5. **Avoid unnecessary abstraction.** Add interfaces and abstract classes when they clarify real variation or contracts.
6. **Use properties when controlled attribute access is needed.**
7. **Write small, testable methods.** Each method should have a clear purpose.
8. **Use dependency injection when replaceability or testing benefits from it.**
9. **Follow SOLID pragmatically.** Do not introduce needless complexity just to follow a principle mechanically.
10. **Test behavior, not just implementation details.** Verify what objects do and which rules they enforce.
11. **Understand Python's conventions.** Double underscores provide name mangling, not absolute privacy; protocols usually provide static typing rather than runtime enforcement.
12. **Prefer readable code.** A simple design is better than a complicated design with unnecessary patterns.

---

## 24. Summary

Object-Oriented Programming provides a way to organize code around objects that combine state and behavior.

The most important concepts to understand first are:

1. Classes and objects
2. Attributes, methods, and constructors
3. Encapsulation and abstraction
4. Inheritance and polymorphism
5. Overriding and Python's alternatives to traditional overloading
6. Interfaces and abstract classes
7. Composition, aggregation, association, and dependency
8. Coupling and cohesion
9. SOLID design principles
