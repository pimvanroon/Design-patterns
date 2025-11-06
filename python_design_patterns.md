# Complete Design Patterns Guide - Python Edition

A comprehensive guide combining decision flow charts, pattern descriptions, and practical Python code examples. I've compiled this based on years of experience and common mistakes I've seen developers make.

## Table of Contents

### Decision Flow Charts
- [Quick Start Decision Tree](#quick-start-decision-tree)
- [Creational Patterns Decision Tree](#creational-patterns-decision-tree)
- [Structural Patterns Decision Tree](#structural-patterns-decision-tree)
- [Behavioral Patterns Decision Tree](#behavioral-patterns-decision-tree)
- [Architectural Patterns Decision Tree](#architectural-patterns-decision-tree)
- [Modern Patterns Decision Tree](#modern-patterns-decision-tree)

### Pattern Categories
- [1. Creational Patterns (GoF)](#1-creational-patterns-gof)
- [2. Structural Patterns (GoF)](#2-structural-patterns-gof)
- [3. Behavioral Patterns (GoF)](#3-behavioral-patterns-gof)
- [4. Modern Extensions](#4-modern-extensions)
- [5. Additional Patterns](#5-additional-patterns)
- [6. Functional Programming Patterns](#6-functional-programming-patterns)
- [7. Concurrency & Parallelism Patterns](#7-concurrency--parallelism-patterns)
- [8. Reactive Programming Patterns](#8-reactive-programming-patterns)
- [9. Cloud & Distributed Patterns](#9-cloud--distributed-patterns)
- [10. Data Access Patterns](#10-data-access-patterns)
- [11. Security Patterns](#11-security-patterns)
- [12. Performance Patterns](#12-performance-patterns)
- [13. Testing Patterns](#13-testing-patterns)
- [14. Modern Architectural Patterns](#14-modern-architectural-patterns)

### Advanced Topics
- [15. Pattern Anti-Patterns](#15-pattern-anti-patterns)
- [16. Pattern Combinations & Recipes](#16-pattern-combinations--recipes)
- [17. Performance Impact Analysis](#17-performance-impact-analysis)
- [18. Common Implementation Mistakes](#18-common-implementation-mistakes)
- [19. Refactoring Guide](#19-refactoring-guide)
- [20. Language-Specific Considerations](#20-language-specific-considerations)

### Decision Tools
- [Quick Decision Guide](#quick-decision-guide)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Final Decision Checklist](#final-decision-checklist)

---

## Quick Start Decision Tree

### 1. What's Your Main Challenge?

```
┌─────────────────────────────────────────────────────────────┐
│                    MAIN CHALLENGE                            │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   CREATION    │ │ STRUCTURE │ │ BEHAVIOR    │
        │   PROBLEMS    │ │ PROBLEMS  │ │ PROBLEMS   │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## CREATIONAL PATTERNS Decision Tree

### When you need to create objects...

This is probably the most common scenario. Most of the time, you'll want to start with Factory Method or Builder. Singleton should be your last resort - I've seen too many projects ruined by overusing it.

```
┌─────────────────────────────────────────────────────────────┐
│                    OBJECT CREATION                           │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   SINGLE      │ │ FAMILY    │ │ COMPLEX     │
        │  INSTANCE     │ │ OBJECTS   │ │ OBJECTS     │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   SINGLETON    │ │ ABSTRACT  │ │   BUILDER   │
        │               │ │ FACTORY   │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   FACTORY     │ │           │ │  PROTOTYPE │
        │   METHOD      │ │           │ │            │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## STRUCTURAL PATTERNS Decision Tree

### When you need to organize objects...

These patterns are about making incompatible things work together or optimizing how objects are structured. Adapter is probably the most useful one here - you'll use it constantly when integrating with third-party libraries.

```
┌─────────────────────────────────────────────────────────────┐
│                   OBJECT ORGANIZATION                       │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │  INTERFACE    │ │ COMPOSITE │ │ PERFORMANCE │
        │  COMPATIBILITY│ │ STRUCTURE │ │ OPTIMIZATION│
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   ADAPTER     │ │ COMPOSITE │ │  FLYWEIGHT  │
        │               │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   BRIDGE      │ │  FACADE   │ │   PROXY     │
        │               │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │  DECORATOR    │ │           │ │             │
        │               │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## BEHAVIORAL PATTERNS Decision Tree

### When you need to manage object interactions...

This is where things get interesting. Observer is probably the most important pattern here - it's the foundation of event-driven programming. Strategy is also incredibly useful for making your code more flexible.

```
┌─────────────────────────────────────────────────────────────┐
│                  OBJECT INTERACTIONS                         │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │  COMMUNICATION│ │ ALGORITHM │ │ STATE       │
        │  BETWEEN      │ │ SELECTION │ │ MANAGEMENT  │
        │  OBJECTS      │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   OBSERVER    │ │ STRATEGY  │ │    STATE    │
        │               │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   MEDIATOR    │ │ COMMAND   │ │ TEMPLATE    │
        │               │ │           │ │ METHOD      │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ CHAIN OF      │ │ ITERATOR  │ │   VISITOR   │
        │ RESPONSIBILITY│ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   MEMENTO     │ │           │ │             │
        │               │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## ARCHITECTURAL PATTERNS Decision Tree

### When you need to structure entire applications...

These are the big picture patterns. MVC is probably what you'll use most often, but don't be afraid to mix and match. I've seen some great applications that combine MVC with Repository and Unit of Work patterns.

```
┌─────────────────────────────────────────────────────────────┐
│                   APPLICATION STRUCTURE                     │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   SEPARATION  │ │ SCALABILITY│ │ DISTRIBUTED │
        │   OF CONCERNS │ │ & PERFORMANCE│ │ SYSTEMS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │     MVC       │ │   CQRS    │ │ MICROSERVICES│
        │   MVP/MVVM    │ │           │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   HEXAGONAL   │ │ EVENT     │ │ API GATEWAY │
        │   ARCHITECTURE│ │ SOURCING  │ │             │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## MODERN PATTERNS Decision Tree

### When you need modern solutions...

These are the patterns that have become essential in modern development. Circuit Breaker is a lifesaver when dealing with external services, and Actor Model is great for concurrent programming. Don't try to implement these from scratch - use existing libraries.

```
┌─────────────────────────────────────────────────────────────┐
│                    MODERN CHALLENGES                        │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ CONCURRENCY   │ │ REACTIVE  │ │ CLOUD       │
        │ & PARALLELISM │ │ PROGRAMMING│ │ COMPUTING   │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ ACTOR MODEL   │ │ REACTIVE  │ │ SERVERLESS  │
        │ THREAD POOL   │ │ STREAMS   │ │ MICROSERVICES│
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ PRODUCER-     │ │ CIRCUIT   │ │ SERVICE     │
        │ CONSUMER      │ │ BREAKER   │ │ MESH        │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Creational Patterns (GoF)

### Singleton
**Description:** Ensures a class has only one instance; provides global access. Useful for configuration, logging, or caches. Use sparingly - it can make testing difficult and create hidden dependencies.

```python
import threading

class Singleton:
    _instance = None
    _lock = threading.Lock()
    
    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
        return cls._instance
```

### Factory Method
**Description:** Defines an interface for creating objects, letting subclasses decide which to instantiate. Decouples object creation from usage. Great for when you don't know the exact type at compile time.

```python
from abc import ABC, abstractmethod

# Product interface
class Product(ABC):
    @abstractmethod
    def do_something(self):
        pass

# Concrete products
class ConcreteProductA(Product):
    def do_something(self):
        print("Product A")

class ConcreteProductB(Product):
    def do_something(self):
        print("Product B")

# Creator abstract class
class Creator(ABC):
    @abstractmethod
    def factory_method(self):
        pass
    
    def some_operation(self):
        product = self.factory_method()
        product.do_something()

# Concrete creators
class ConcreteCreatorA(Creator):
    def factory_method(self):
        return ConcreteProductA()

class ConcreteCreatorB(Creator):
    def factory_method(self):
        return ConcreteProductB()
```

### Abstract Factory
**Description:** Creates families of related objects without specifying concrete classes. Helps ensure compatible product families. Perfect for UI frameworks where you need consistent themes.

```python
from abc import ABC, abstractmethod

# Abstract products
class Button(ABC):
    @abstractmethod
    def render(self):
        pass

class Checkbox(ABC):
    @abstractmethod
    def render(self):
        pass

# Concrete products for Windows
class WinButton(Button):
    def render(self):
        print("Windows Button")

class WinCheckbox(Checkbox):
    def render(self):
        print("Windows Checkbox")

# Concrete products for Mac
class MacButton(Button):
    def render(self):
        print("Mac Button")

class MacCheckbox(Checkbox):
    def render(self):
        print("Mac Checkbox")

# Abstract factory
class UIFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass
    
    @abstractmethod
    def create_checkbox(self):
        pass

# Concrete factories
class WinFactory(UIFactory):
    def create_button(self):
        return WinButton()
    
    def create_checkbox(self):
        return WinCheckbox()

class MacFactory(UIFactory):
    def create_button(self):
        return MacButton()
    
    def create_checkbox(self):
        return MacCheckbox()
```

### Builder
**Description:** Separates construction of complex objects from representation. Allows step-by-step creation of objects with optional parts. Excellent for objects with many optional parameters.

```python
# Product to be built
class Car:
    def __init__(self):
        self.engine = None
        self.wheels = None
        self.color = None
        self.has_gps = False
        self.has_sunroof = False

# Builder interface
class CarBuilder(ABC):
    @abstractmethod
    def set_engine(self, engine):
        pass
    
    @abstractmethod
    def set_wheels(self, wheels):
        pass
    
    @abstractmethod
    def set_color(self, color):
        pass
    
    @abstractmethod
    def set_gps(self, has_gps):
        pass
    
    @abstractmethod
    def set_sunroof(self, has_sunroof):
        pass
    
    @abstractmethod
    def build(self):
        pass

# Concrete builder
class CarBuilderImpl(CarBuilder):
    def __init__(self):
        self.car = Car()
    
    def set_engine(self, engine):
        self.car.engine = engine
        return self
    
    def set_wheels(self, wheels):
        self.car.wheels = wheels
        return self
    
    def set_color(self, color):
        self.car.color = color
        return self
    
    def set_gps(self, has_gps):
        self.car.has_gps = has_gps
        return self
    
    def set_sunroof(self, has_sunroof):
        self.car.has_sunroof = has_sunroof
        return self
    
    def build(self):
        return self.car

# Director (optional)
class CarDirector:
    def build_sports_car(self, builder):
        return (builder
                .set_engine("V8")
                .set_wheels("Sports")
                .set_color("Red")
                .set_gps(True)
                .set_sunroof(False)
                .build())
```

### Prototype
**Description:** Create new objects by cloning existing ones. Useful when object creation is costly. Great for game development where you need many similar objects.

```python
import copy

class Shape(ABC):
    @abstractmethod
    def clone(self):
        pass

class Circle(Shape):
    def __init__(self, radius=0):
        self.radius = radius
    
    def clone(self):
        return copy.deepcopy(self)
```

---

## 2. Structural Patterns (GoF)

### Adapter
**Description:** Converts one interface to another expected by clients. Allows incompatible interfaces to work together. You'll use this constantly when integrating with third-party libraries.

```python
from abc import ABC, abstractmethod

# Target interface (what client expects)
class Target(ABC):
    @abstractmethod
    def request(self):
        pass

# Adaptee (existing incompatible class)
class Adaptee:
    def specific_request(self):
        print("Specific request from Adaptee")

# Adapter (makes Adaptee compatible with Target)
class Adapter(Target):
    def __init__(self):
        self.adaptee = Adaptee()
    
    def request(self):
        self.adaptee.specific_request()

# Client code
class Client:
    def use_target(self, target):
        target.request()
```

### Bridge
**Description:** Decouples abstraction from implementation so both can vary independently. Useful for UI/platform independence. Helps avoid the "explosion of classes" problem.

```python
from abc import ABC, abstractmethod

# Implementation interface
class Device(ABC):
    @abstractmethod
    def turn_on(self):
        pass
    
    @abstractmethod
    def turn_off(self):
        pass

# Concrete implementations
class TV(Device):
    def turn_on(self):
        print("TV is ON")
    
    def turn_off(self):
        print("TV is OFF")

class Radio(Device):
    def turn_on(self):
        print("Radio is ON")
    
    def turn_off(self):
        print("Radio is OFF")

# Abstraction
class Remote(ABC):
    def __init__(self, device):
        self.device = device
    
    @abstractmethod
    def toggle(self):
        pass

# Refined abstraction
class AdvancedRemote(Remote):
    def toggle(self):
        self.device.turn_on()
    
    def mute(self):
        print("Device muted")
```

### Composite
**Description:** Treat individual objects and compositions uniformly. Ideal for tree-like structures (folders, UI elements). Makes complex hierarchies easy to work with.

```python
from abc import ABC, abstractmethod

class Component(ABC):
    @abstractmethod
    def display(self):
        pass

class Leaf(Component):
    def display(self):
        print("Leaf")

class Composite(Component):
    def __init__(self):
        self.children = []
    
    def add(self, component):
        self.children.append(component)
    
    def display(self):
        for child in self.children:
            child.display()
```

### Decorator
**Description:** Adds responsibilities to objects dynamically. Supports flexible extension without modifying original class. Great for adding features like logging, caching, or validation.

```python
from abc import ABC, abstractmethod

class Notifier(ABC):
    @abstractmethod
    def send(self, msg):
        pass

class EmailNotifier(Notifier):
    def send(self, msg):
        print(f"Email: {msg}")

class SMSDecorator(Notifier):
    def __init__(self, notifier):
        self.wrappee = notifier
    
    def send(self, msg):
        self.wrappee.send(msg)
        print(f"SMS: {msg}")
```

### Facade
**Description:** Provides a unified, simple interface to a set of interfaces in a subsystem. Simplifies complex subsystems for clients. Perfect for wrapping complex APIs.

```python
class CPU:
    def freeze(self):
        print("CPU frozen")
    
    def execute(self):
        print("CPU executing")

class Memory:
    def load(self):
        print("Memory loaded")

class ComputerFacade:
    def __init__(self):
        self.cpu = CPU()
        self.memory = Memory()
    
    def start(self):
        self.cpu.freeze()
        self.memory.load()
        self.cpu.execute()
```

### Flyweight
**Description:** Uses shared objects to reduce memory usage for large numbers of similar objects. Great for text editors, games, or any scenario with many similar objects.

```python
class FlyweightFactory:
    def __init__(self):
        self.flyweights = {}
    
    def get(self, key):
        if key not in self.flyweights:
            self.flyweights[key] = Flyweight(key)
        return self.flyweights[key]

class Flyweight:
    def __init__(self, key):
        self.key = key
```

### Proxy
**Description:** Provides a placeholder for another object to control access, add lazy loading, caching, or security. Essential for performance optimization and access control.

```python
from abc import ABC, abstractmethod

class Image(ABC):
    @abstractmethod
    def display(self):
        pass

class RealImage(Image):
    def __init__(self, filename):
        self.filename = filename
        self.load_from_disk()
    
    def load_from_disk(self):
        print(f"Loading {self.filename}")
    
    def display(self):
        print(f"Displaying {self.filename}")

class ProxyImage(Image):
    def __init__(self, filename):
        self.filename = filename
        self.real_image = None
    
    def display(self):
        if self.real_image is None:
            self.real_image = RealImage(self.filename)
        self.real_image.display()
```

---

## 3. Behavioral Patterns (GoF)

### Chain of Responsibility
**Description:** Pass requests along a chain until one handler processes it. Useful for event processing. Great for middleware pipelines and request handling.

```python
from abc import ABC, abstractmethod

class Handler(ABC):
    def __init__(self):
        self.next = None
    
    def set_next(self, handler):
        self.next = handler
        return handler
    
    @abstractmethod
    def handle(self, request):
        pass

class ConcreteHandler(Handler):
    def handle(self, request):
        if request < 10:
            print("Handled")
        elif self.next:
            self.next.handle(request)
```

### Command
**Description:** Encapsulates requests as objects. Enables queuing, logging, and undo/redo operations. Perfect for implementing undo functionality and macro commands.

```python
from abc import ABC, abstractmethod

# Receiver
class Light:
    def on(self):
        print("Light is ON")
    
    def off(self):
        print("Light is OFF")

# Command interface
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass
    
    @abstractmethod
    def undo(self):
        pass

# Concrete commands
class LightOnCommand(Command):
    def __init__(self, light):
        self.light = light
    
    def execute(self):
        self.light.on()
    
    def undo(self):
        self.light.off()

class LightOffCommand(Command):
    def __init__(self, light):
        self.light = light
    
    def execute(self):
        self.light.off()
    
    def undo(self):
        self.light.on()

# Invoker
class RemoteControl:
    def __init__(self):
        self.command = None
    
    def set_command(self, command):
        self.command = command
    
    def press_button(self):
        if self.command:
            self.command.execute()
    
    def press_undo(self):
        if self.command:
            self.command.undo()
```

### Interpreter
**Description:** Defines a grammar and interprets sentences in that language. Useful for DSLs and expression evaluation. Great for rule engines and configuration languages.

```python
from abc import ABC, abstractmethod

class Expression(ABC):
    @abstractmethod
    def interpret(self, context):
        pass

class Number(Expression):
    def __init__(self, value):
        self.value = value
    
    def interpret(self, context):
        return self.value
```

### Iterator
**Description:** Provides sequential access to elements without exposing underlying structure. Essential for collections and data structures. Makes traversal algorithms reusable.

```python
class MyCollection:
    def __init__(self):
        self.items = [1, 2, 3]
    
    def __iter__(self):
        return iter(self.items)
```

### Mediator
**Description:** Centralizes communication between objects, reducing direct dependencies. Great for complex UI interactions and chat systems. Reduces coupling between components.

```python
from abc import ABC, abstractmethod

# Mediator interface
class ChatMediator(ABC):
    @abstractmethod
    def register(self, user):
        pass
    
    @abstractmethod
    def send(self, message, sender):
        pass

# Concrete mediator
class ChatMediatorImpl(ChatMediator):
    def __init__(self):
        self.users = []
    
    def register(self, user):
        self.users.append(user)
    
    def send(self, message, sender):
        for user in self.users:
            if user != sender:
                user.receive(message)

# Colleague
class User:
    def __init__(self, name, mediator):
        self.name = name
        self.mediator = mediator
    
    def send(self, message):
        print(f"{self.name} sends: {message}")
        self.mediator.send(message, self)
    
    def receive(self, message):
        print(f"{self.name} receives: {message}")
```

### Memento
**Description:** Captures and restores object state without violating encapsulation. Supports undo functionality. Perfect for implementing save/restore features.

```python
class Memento:
    def __init__(self, state):
        self.state = state
    
    def get_state(self):
        return self.state

class Originator:
    def __init__(self):
        self.state = None
    
    def set_state(self, state):
        self.state = state
    
    def save(self):
        return Memento(self.state)
    
    def restore(self, memento):
        self.state = memento.get_state()
```

### Observer
**Description:** Defines a one-to-many dependency: when one object changes state, all dependents are notified. Foundation of event-driven programming. Essential for MVC and reactive systems.

```python
from abc import ABC, abstractmethod

class Observer(ABC):
    @abstractmethod
    def update(self):
        pass

class Subject:
    def __init__(self):
        self.observers = []
    
    def attach(self, observer):
        self.observers.append(observer)
    
    def notify(self):
        for observer in self.observers:
            observer.update()
```

### State
**Description:** Allows an object to alter behavior when its internal state changes. Perfect for state machines and complex conditional logic. Makes state transitions explicit.

```python
from abc import ABC, abstractmethod

class State(ABC):
    @abstractmethod
    def handle(self, context):
        pass

class Context:
    def __init__(self):
        self.state = None
    
    def request(self):
        self.state.handle(self)
```

### Strategy
**Description:** Encapsulates interchangeable algorithms. Clients can switch strategies at runtime. Great for sorting algorithms, payment methods, or any interchangeable behavior.

```python
from abc import ABC, abstractmethod

class Strategy(ABC):
    @abstractmethod
    def execute(self):
        pass

class QuickSort(Strategy):
    def execute(self):
        print("QuickSort")

class Sorter:
    def __init__(self, strategy):
        self.strategy = strategy
    
    def sort(self):
        self.strategy.execute()
```

### Template Method
**Description:** Defines skeleton of algorithm, deferring certain steps to subclasses. Great for frameworks and libraries. Ensures consistent algorithm structure.

```python
from abc import ABC, abstractmethod

class Game(ABC):
    def play(self):
        self.init()
        self.start()
        self.end()
    
    @abstractmethod
    def init(self):
        pass
    
    @abstractmethod
    def start(self):
        pass
    
    @abstractmethod
    def end(self):
        pass
```

### Visitor
**Description:** Represents an operation to be performed on object structures without modifying the objects themselves. Great for adding operations to existing class hierarchies.

```python
from abc import ABC, abstractmethod

class Visitor(ABC):
    @abstractmethod
    def visit(self, element):
        pass

class Element(ABC):
    @abstractmethod
    def accept(self, visitor):
        pass

class ConcreteElement(Element):
    def accept(self, visitor):
        visitor.visit(self)
```

---

## 4. Modern Extensions

### Null Object
**Description:** Provides a do-nothing implementation instead of null, avoiding null checks. Eliminates null pointer exceptions and makes code more robust.

```python
from abc import ABC, abstractmethod

class Logger(ABC):
    @abstractmethod
    def log(self, msg):
        pass

class ConsoleLogger(Logger):
    def log(self, msg):
        print(msg)

class NullLogger(Logger):
    def log(self, msg):
        pass  # Do nothing
```

### MVC (Model-View-Controller)
**Description:** Separates application logic into Model (data), View (UI), Controller (input). Promotes separation of concerns. The foundation of most web frameworks.

```python
# Model
class User:
    def __init__(self, name=None):
        self.name = name

# View
class UserView:
    def render(self, user):
        print(f"Hello {user.name}")

# Controller
class UserController:
    def __init__(self, model, view):
        self.model = model
        self.view = view
    
    def update_view(self):
        self.view.render(self.model)
```

### Dependency Injection / Service Locator
**Description:** Externalizes object creation and dependency resolution. Improves flexibility and testability. Essential for modern application architecture.

```python
class ServiceLocator:
    _services = {}
    
    @classmethod
    def register(cls, service_type, service):
        cls._services[service_type] = service
    
    @classmethod
    def get(cls, service_type):
        return cls._services.get(service_type)
```

### Repository / DAO
**Description:** Abstracts data layer, providing a clean interface for CRUD operations. Makes data access testable and allows easy switching between data sources.

```python
from abc import ABC, abstractmethod

# Domain entity
class User:
    def __init__(self, id=None, name=None, email=None):
        self.id = id
        self.name = name
        self.email = email

# Repository interface
class UserRepository(ABC):
    @abstractmethod
    def get_by_id(self, id):
        pass
    
    @abstractmethod
    def get_all(self):
        pass
    
    @abstractmethod
    def save(self, user):
        pass
    
    @abstractmethod
    def delete(self, id):
        pass

# Concrete repository
class UserRepositoryImpl(UserRepository):
    def __init__(self):
        self.users = []
    
    def get_by_id(self, id):
        for user in self.users:
            if user.id == id:
                return user
        return None
    
    def get_all(self):
        return list(self.users)
    
    def save(self, user):
        existing = self.get_by_id(user.id)
        if existing:
            self.users.remove(existing)
        self.users.append(user)
    
    def delete(self, id):
        user = self.get_by_id(id)
        if user:
            self.users.remove(user)
```

### MVP (Model-View-Presenter)
**Description:** Similar to MVC but with tighter coupling between View and Presenter. The Presenter handles all UI logic. Great for desktop applications.

```python
from abc import ABC, abstractmethod

# Model
class UserModel:
    def __init__(self):
        self.name = None
        self.email = None

# View interface
class UserView(ABC):
    @abstractmethod
    def get_user_name(self):
        pass
    
    @abstractmethod
    def set_user_name(self, name):
        pass
    
    @abstractmethod
    def get_user_email(self):
        pass
    
    @abstractmethod
    def set_user_email(self, email):
        pass
    
    @abstractmethod
    def show_message(self, message):
        pass

# Presenter
class UserPresenter:
    def __init__(self, view, model):
        self.view = view
        self.model = model
    
    def load_user(self):
        self.view.set_user_name(self.model.name)
        self.view.set_user_email(self.model.email)
    
    def save_user(self):
        self.model.name = self.view.get_user_name()
        self.model.email = self.view.get_user_email()
        self.view.show_message("User saved successfully!")
```

### MVVM (Model-View-ViewModel)
**Description:** Designed for modern UI frameworks. Uses data binding and commands. The ViewModel acts as a data converter and command handler.

```python
from abc import ABC, abstractmethod

# Model
class User:
    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email

# ViewModel
class UserViewModel:
    def __init__(self, user):
        self.user = user
        self._name = user.name
        self._email = user.email
        self.property_changed_callbacks = []
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, value):
        old_name = self._name
        self._name = value
        self._notify_property_changed('name', old_name, value)
    
    @property
    def email(self):
        return self._email
    
    @email.setter
    def email(self, value):
        old_email = self._email
        self._email = value
        self._notify_property_changed('email', old_email, value)
    
    def save(self):
        self.user.name = self.name
        self.user.email = self.email
        # Save logic here
    
    def add_property_changed_listener(self, callback):
        self.property_changed_callbacks.append(callback)
    
    def _notify_property_changed(self, property_name, old_value, new_value):
        for callback in self.property_changed_callbacks:
            callback(property_name, old_value, new_value)
```

### CQRS (Command Query Responsibility Segregation)
**Description:** Separates read and write operations. Uses different models for commands and queries. Great for high-performance systems with complex business logic.

```python
import asyncio
from abc import ABC, abstractmethod

# Command side
class Command(ABC):
    pass

class CreateUserCommand(Command):
    def __init__(self, name, email):
        self.name = name
        self.email = email

class CommandHandler(ABC):
    @abstractmethod
    async def handle(self, command):
        pass

class CreateUserCommandHandler(CommandHandler):
    async def handle(self, command):
        # Write to database
        print(f"Creating user: {command.name}")
        await asyncio.sleep(0.1)

# Query side
class Query(ABC):
    pass

class GetUserQuery(Query):
    def __init__(self, user_id):
        self.user_id = user_id

class QueryHandler(ABC):
    @abstractmethod
    async def handle(self, query):
        pass

class GetUserQueryHandler(QueryHandler):
    async def handle(self, query):
        # Read from database
        await asyncio.sleep(0.05)
        return UserDto(query.user_id, "User Name")

class UserDto:
    def __init__(self, id, name):
        self.id = id
        self.name = name
```

### Event Sourcing
**Description:** Stores changes as a sequence of events rather than current state. Enables audit trails and time travel. Perfect for financial systems and audit requirements.

```python
import uuid
from datetime import datetime
from abc import ABC, abstractmethod

# Domain event
class DomainEvent(ABC):
    def __init__(self):
        self.occurred_at = datetime.now()
        self.id = uuid.uuid4()

class UserCreatedEvent(DomainEvent):
    def __init__(self, name, email):
        super().__init__()
        self.name = name
        self.email = email

class UserEmailChangedEvent(DomainEvent):
    def __init__(self, old_email, new_email):
        super().__init__()
        self.old_email = old_email
        self.new_email = new_email

# Aggregate root
class User:
    def __init__(self):
        self.events = []
        self.id = None
        self.name = None
        self.email = None
    
    @classmethod
    def create(cls, name, email):
        user = cls()
        user.apply(UserCreatedEvent(name, email))
        return user
    
    def change_email(self, new_email):
        old_email = self.email
        self.apply(UserEmailChangedEvent(old_email, new_email))
    
    def apply(self, event):
        self.events.append(event)
        # Apply event to current state
        if isinstance(event, UserCreatedEvent):
            self.id = event.id
            self.name = event.name
            self.email = event.email
        elif isinstance(event, UserEmailChangedEvent):
            self.email = event.new_email
    
    def get_uncommitted_events(self):
        return list(self.events)
    
    def mark_events_as_committed(self):
        self.events.clear()
```

---

## 5. Additional Patterns

### Object Pool
**Description:** Reuses objects that are expensive to create (e.g., database connections). Reduces creation overhead. Essential for high-performance applications.

```python
from queue import Queue
from typing import Callable, TypeVar

T = TypeVar('T')

class ObjectPool:
    def __init__(self, factory: Callable[[], T], max_size=10):
        self.items = Queue(maxsize=max_size)
        self.factory = factory
    
    def get(self):
        if self.items.empty():
            return self.factory()
        return self.items.get()
    
    def return_item(self, obj):
        if not self.items.full():
            self.items.put(obj)
```

### Lazy Initialization
**Description:** Delays object creation until actually needed. Saves memory and computation. Great for expensive resources that might not be used.

```python
class Lazy:
    def __init__(self, factory):
        self._value = None
        self.factory = factory
    
    def get(self):
        if self._value is None:
            self._value = self.factory()
        return self._value
```

### Multiton
**Description:** Like Singleton, but allows a limited number of instances keyed by a value. Useful for resource management with multiple instances.

```python
import threading

class Multiton:
    _instances = {}
    _lock = threading.Lock()
    
    def __new__(cls, key):
        if key not in cls._instances:
            with cls._lock:
                if key not in cls._instances:
                    cls._instances[key] = super().__new__(cls)
        return cls._instances[key]
```

### Pipeline
**Description:** Processes data sequentially through stages. Often used in streaming or functional pipelines. Great for data transformation workflows.

```python
class Pipeline:
    def __init__(self):
        self.stages = []
    
    def add_stage(self, stage):
        self.stages.append(stage)
        return self
    
    def process(self, input_data):
        result = input_data
        for stage in self.stages:
            result = stage(result)
        return result

# Usage
pipeline = Pipeline().add_stage(lambda x: x + 1).add_stage(lambda x: x * 2)
result = pipeline.process(1)
```

### Event Bus / Event Aggregator
**Description:** Decouples publishers and subscribers. Enables scalable event-driven architecture. Perfect for microservices communication.

```python
from abc import ABC, abstractmethod

class EventListener(ABC):
    @abstractmethod
    def on_message(self, message):
        pass

class EventBus:
    def __init__(self):
        self.listeners = []
    
    def subscribe(self, listener):
        self.listeners.append(listener)
    
    def publish(self, message):
        for listener in self.listeners:
            listener.on_message(message)
```

### Specification
**Description:** Encapsulates business rules into reusable predicates. Allows combining rules using logical operators. Great for complex business logic.

```python
from abc import ABC, abstractmethod

class Specification(ABC):
    @abstractmethod
    def is_satisfied(self, item):
        pass

class EvenSpecification(Specification):
    def is_satisfied(self, item):
        return item % 2 == 0
```

---

## 6. Functional Programming Patterns

### Monad
**Description:** Encapsulates computations with side effects in a composable way. Essential for functional programming and error handling. Makes error handling explicit and composable.

```python
# Maybe/Option monad
class Maybe:
    def __init__(self, value, has_value):
        self._value = value
        self._has_value = has_value
    
    @classmethod
    def some(cls, value):
        return cls(value, True)
    
    @classmethod
    def none(cls):
        return cls(None, False)
    
    def bind(self, f):
        if self._has_value:
            return f(self._value)
        return Maybe.none()
    
    def map(self, f):
        if self._has_value:
            return Maybe.some(f(self._value))
        return Maybe.none()
```

### Functor
**Description:** Provides a way to apply functions to values wrapped in a context (like Option, List). Enables functional composition. Foundation of functional programming.

```python
class ListFunctor:
    def __init__(self, items):
        self.items = items
    
    def map(self, f):
        return ListFunctor([f(item) for item in self.items])
```

### Fold/Reduce
**Description:** Aggregates collections into single values. Fundamental for functional data processing. Essential for data analysis and transformation.

```python
def fold(source, seed, func):
    result = seed
    for item in source:
        result = func(result, item)
    return result

# Usage: sum = fold(numbers, 0, lambda acc, x: acc + x)
```

---

## 7. Concurrency & Parallelism Patterns

### Actor Model
**Description:** Objects communicate through message passing. Excellent for concurrent systems and distributed computing. Eliminates shared state issues.

```python
import asyncio
from abc import ABC, abstractmethod

class Actor(ABC):
    @abstractmethod
    async def receive(self, message):
        pass

class SimpleActor(Actor):
    async def receive(self, message):
        if isinstance(message, str):
            print(f"Received: {message}")
        elif isinstance(message, int):
            print(f"Number: {message}")

class ActorSystem:
    def __init__(self):
        self.actors = {}
    
    def register(self, name, actor):
        self.actors[name] = actor
    
    async def send(self, actor_name, message):
        actor = self.actors.get(actor_name)
        if actor:
            await actor.receive(message)
```

### Producer-Consumer
**Description:** Decouples data production from consumption using queues. Essential for asynchronous processing. Great for background processing and data pipelines.

```python
import asyncio
from asyncio import Queue

class ProducerConsumer:
    def __init__(self):
        self.queue = Queue()
    
    async def produce(self, item):
        await self.queue.put(item)
    
    async def consume(self):
        return await self.queue.get()
```

### Thread Pool
**Description:** Manages a pool of worker threads to execute tasks. Improves performance and resource management. Essential for scalable server applications.

```python
from concurrent.futures import ThreadPoolExecutor
import threading

class ThreadPool:
    def __init__(self, worker_count):
        self.executor = ThreadPoolExecutor(max_workers=worker_count)
        self.shutdown = False
    
    def queue_task(self, task):
        if not self.shutdown:
            self.executor.submit(task)
    
    def shutdown_pool(self):
        self.shutdown = True
        self.executor.shutdown(wait=True)
```

---

## 8. Reactive Programming Patterns

### Circuit Breaker
**Description:** Prevents cascading failures by temporarily stopping calls to failing services. Critical for microservices. Essential for resilient distributed systems.

```python
from datetime import datetime, timedelta
from enum import Enum

class CircuitState(Enum):
    CLOSED = "closed"
    OPEN = "open"
    HALF_OPEN = "half_open"

class CircuitBreaker:
    def __init__(self, threshold=5, timeout_seconds=60):
        self.state = CircuitState.CLOSED
        self.failure_count = 0
        self.last_failure_time = None
        self.threshold = threshold
        self.timeout = timedelta(seconds=timeout_seconds)
    
    def execute(self, operation):
        if self.state == CircuitState.OPEN:
            if self.last_failure_time and datetime.now() - self.last_failure_time > self.timeout:
                self.state = CircuitState.HALF_OPEN
            else:
                raise Exception("Circuit breaker is open")
        
        try:
            result = operation()
            self.on_success()
            return result
        except Exception as ex:
            self.on_failure()
            raise ex
    
    def on_success(self):
        self.failure_count = 0
        self.state = CircuitState.CLOSED
    
    def on_failure(self):
        self.failure_count += 1
        self.last_failure_time = datetime.now()
        if self.failure_count >= self.threshold:
            self.state = CircuitState.OPEN
```

### Saga
**Description:** Manages distributed transactions across multiple services. Essential for microservices architectures. Provides eventual consistency in distributed systems.

```python
import asyncio
from abc import ABC, abstractmethod

class SagaStep(ABC):
    @abstractmethod
    async def execute(self):
        pass
    
    @abstractmethod
    async def compensate(self):
        pass

class OrderSaga:
    def __init__(self):
        self.steps = []
        self.completed_steps = []
    
    def add_step(self, step):
        self.steps.append(step)
    
    async def execute(self):
        try:
            for step in self.steps:
                await step.execute()
                self.completed_steps.append(step)
        except Exception as e:
            await self.compensate()
            raise e
    
    async def compensate(self):
        for step in reversed(self.completed_steps):
            await step.compensate()
```

---

## 9. Cloud & Distributed Patterns

### API Gateway
**Description:** Single entry point for client requests to microservices. Simplifies client-server communication. Essential for microservices architecture.

```python
import asyncio
from abc import ABC, abstractmethod

class Microservice(ABC):
    @abstractmethod
    async def process_request(self, request):
        pass

class APIGateway:
    def __init__(self):
        self.services = {}
    
    def register_service(self, path, service):
        self.services[path] = service
    
    async def route_request(self, path, request):
        service = self.services.get(path)
        if service:
            return await service.process_request(request)
        raise ValueError(f"No service found for path: {path}")
```

### Service Mesh
**Description:** Handles service-to-service communication in microservices. Provides observability and security. Essential for production microservices deployments.

```python
import asyncio
from abc import ABC, abstractmethod

class ServiceMesh(ABC):
    @abstractmethod
    async def call_service(self, service_name, endpoint, data):
        pass

class ServiceMeshImpl(ServiceMesh):
    def __init__(self):
        self.service_urls = {}
    
    def register_service(self, name, url):
        self.service_urls[name] = url
    
    async def call_service(self, service_name, endpoint, data):
        url = self.service_urls.get(service_name)
        if not url:
            raise ValueError(f"Service {service_name} not found")
        
        full_url = f"{url}/{endpoint}"
        print(f"Calling {full_url} with data: {data}")
        
        # Return mock response
        return None
```

---

## 10. Data Access Patterns

### Unit of Work
**Description:** Maintains a list of objects affected by a business transaction. Ensures data consistency. Essential for maintaining transactional integrity.

```python
import asyncio

class UnitOfWork:
    def __init__(self):
        self.new_entities = []
        self.dirty_entities = []
        self.deleted_entities = []
    
    def register_new(self, entity):
        self.new_entities.append(entity)
    
    def register_dirty(self, entity):
        self.dirty_entities.append(entity)
    
    def register_deleted(self, entity):
        self.deleted_entities.append(entity)
    
    async def commit(self):
        # Save all changes atomically
        print("Committing all changes...")
        await asyncio.sleep(0.1)
    
    def rollback(self):
        self.new_entities.clear()
        self.dirty_entities.clear()
        self.deleted_entities.clear()
        print("Rolled back all changes")
```

### Identity Map
**Description:** Ensures each object is loaded only once per session. Prevents duplicate objects in memory. Essential for ORM frameworks and data consistency.

```python
class IdentityMap:
    def __init__(self):
        self.map = {}
    
    def get(self, key):
        return self.map.get(key)
    
    def put(self, key, entity):
        self.map[key] = entity
    
    def contains(self, key):
        return key in self.map
    
    def remove(self, key):
        self.map.pop(key, None)
    
    def clear(self):
        self.map.clear()
```

---

## 11. Security Patterns

### Authentication
**Description:** Verifies user identity. Fundamental security requirement. Essential for any system that needs to identify users.

```python
import asyncio

class AuthenticationService:
    async def authenticate_async(self, username, password):
        # Simulate authentication logic
        await asyncio.sleep(0.1)
        return username == "admin" and password == "password"
    
    async def generate_token_async(self, username):
        await asyncio.sleep(0.05)
        return f"token_{username}_{asyncio.get_event_loop().time()}"
```

### Authorization
**Description:** Controls access to resources based on user permissions. Essential for secure systems. Implements the principle of least privilege.

```python
import asyncio

class AuthorizationService:
    def __init__(self):
        self.permissions = {
            "admin": ["read", "write", "delete"],
            "user": ["read"]
        }
    
    async def is_authorized_async(self, user, resource, action):
        await asyncio.sleep(0.01)
        user_permissions = self.permissions.get(user, [])
        return action in user_permissions
```

---

## 12. Performance Patterns

### Caching
**Description:** Stores frequently accessed data in fast storage. Improves application performance. Essential for high-performance applications.

```python
from datetime import datetime, timedelta

class CacheItem:
    def __init__(self, value, expiry):
        self.value = value
        self.expiry = expiry
    
    def is_expired(self):
        return datetime.now() > self.expiry

class MemoryCache:
    def __init__(self):
        self.cache = {}
    
    def get(self, key):
        item = self.cache.get(key)
        if item and not item.is_expired():
            return item.value
        return None
    
    def set(self, key, value, expiry_seconds=300):
        expiry = datetime.now() + timedelta(seconds=expiry_seconds)
        self.cache[key] = CacheItem(value, expiry)
    
    def remove(self, key):
        self.cache.pop(key, None)
    
    def clear(self):
        self.cache.clear()
```

### Connection Pooling
**Description:** Reuses database connections. Improves performance and resource utilization. Essential for database-intensive applications.

```python
from queue import Queue
import threading

class ConnectionPool:
    def __init__(self, max_connections, connection_factory):
        self.available_connections = Queue(maxsize=max_connections)
        self.all_connections = []
        self.max_connections = max_connections
        self.connection_factory = connection_factory
        self.lock = threading.Lock()
    
    def get_connection(self):
        if not self.available_connections.empty():
            return self.available_connections.get()
        
        with self.lock:
            if len(self.all_connections) < self.max_connections:
                connection = self.connection_factory()
                self.all_connections.append(connection)
                return connection
        
        raise Exception("No connections available")
    
    def return_connection(self, connection):
        self.available_connections.put(connection)
```

---

## 13. Testing Patterns

### Test Double
**Description:** Replaces real objects with test-specific objects. Essential for unit testing. Enables isolated testing of components.

```python
import asyncio

class UserService:
    async def get_user_async(self, id):
        # Real implementation
        await asyncio.sleep(0.1)
        user = User(id, "Real User")
        return user

class TestUserService:
    def __init__(self):
        self.users = {}
    
    def add_user(self, user):
        self.users[user.id] = user
    
    async def get_user_async(self, id):
        return self.users.get(id)

class User:
    def __init__(self, id, name):
        self.id = id
        self.name = name
```

### Mock Object
**Description:** Simulates behavior of real objects for testing. Enables isolated testing. Essential for testing interactions between objects.

```python
import asyncio

class EmailService:
    async def send_email_async(self, to, subject, body):
        pass

class MockEmailService(EmailService):
    def __init__(self):
        self.sent_emails = []
        self.should_throw_exception = False
    
    async def send_email_async(self, to, subject, body):
        if self.should_throw_exception:
            raise Exception("Email service unavailable")
        
        self.sent_emails.append(f"To: {to}, Subject: {subject}")
```

---

## 14. Modern Architectural Patterns

### Clean Architecture
**Description:** Separates concerns into layers with dependency inversion. Promotes maintainable code. Essential for large, complex applications.

```python
import asyncio
from abc import ABC, abstractmethod

# Domain Layer (Core)
class User:
    def __init__(self, id=None, name=None, email=None):
        self.id = id
        self.name = name
        self.email = email

class UserRepository(ABC):
    @abstractmethod
    async def get_by_id_async(self, id):
        pass
    
    @abstractmethod
    async def save_async(self, user):
        pass

# Application Layer
class UserService:
    def __init__(self, user_repository):
        self.user_repository = user_repository
    
    async def get_user_async(self, id):
        return await self.user_repository.get_by_id_async(id)

# Infrastructure Layer
class SqlUserRepository(UserRepository):
    async def get_by_id_async(self, id):
        # Database implementation
        await asyncio.sleep(0.1)
        return User(id, "User from DB")
    
    async def save_async(self, user):
        # Database save implementation
        await asyncio.sleep(0.05)
```

### Domain-Driven Design (DDD)
**Description:** Focuses on business domain modeling. Improves software design. Essential for complex business applications.

```python
from enum import Enum

class OrderStatus(Enum):
    DRAFT = "draft"
    CONFIRMED = "confirmed"

class OrderItem:
    def __init__(self, product, quantity):
        self.product = product
        self.quantity = quantity
        self.price = product.price
    
    @property
    def total(self):
        return self.price * self.quantity

class Order:
    def __init__(self):
        self.items = []
        self.id = None
        self.customer = None
        self.status = OrderStatus.DRAFT
    
    @property
    def total_amount(self):
        return sum(item.total for item in self.items)
    
    def add_item(self, product, quantity):
        if self.status != OrderStatus.DRAFT:
            raise ValueError("Cannot modify confirmed order")
        self.items.append(OrderItem(product, quantity))
    
    def confirm(self):
        if not self.items:
            raise ValueError("Cannot confirm empty order")
        self.status = OrderStatus.CONFIRMED

class OrderPricingService:
    def calculate_discount(self, order):
        if order.total_amount > 1000:
            return order.total_amount * 0.1  # 10% discount
        return 0
```

### Hexagonal Architecture (Ports and Adapters)
**Description:** Separates business logic from external concerns using ports and adapters. Makes applications testable and technology-agnostic. Essential for maintainable enterprise applications.

```python
import asyncio
from abc import ABC, abstractmethod
from enum import Enum

class OrderStatus(Enum):
    PROCESSING = "processing"
    COMPLETED = "completed"

# Domain (Core) - Business Logic
class Order:
    def __init__(self):
        self.id = None
        self.amount = None
        self.status = None
    
    def process(self):
        if self.amount <= 0:
            raise ValueError("Invalid amount")
        self.status = OrderStatus.PROCESSING
    
    def complete(self):
        self.status = OrderStatus.COMPLETED

# Ports (Interfaces) - Define contracts
class OrderRepository(ABC):
    @abstractmethod
    async def get_by_id_async(self, id):
        pass
    
    @abstractmethod
    async def save_async(self, order):
        pass

class PaymentService(ABC):
    @abstractmethod
    async def process_payment_async(self, amount, card_number):
        pass

class NotificationService(ABC):
    @abstractmethod
    async def send_notification_async(self, message, recipient):
        pass

# Application Services - Orchestrate business logic
class OrderService:
    def __init__(self, order_repository, payment_service, notification_service):
        self.order_repository = order_repository
        self.payment_service = payment_service
        self.notification_service = notification_service
    
    async def process_order_async(self, order_id, card_number):
        order = await self.order_repository.get_by_id_async(order_id)
        
        order.process()
        
        success = await self.payment_service.process_payment_async(order.amount, card_number)
        
        if success:
            order.complete()
            await self.notification_service.send_notification_async(
                "Order completed", "customer@email.com"
            )
        
        await self.order_repository.save_async(order)

# Adapters (Infrastructure) - Implement ports
class SqlOrderRepository(OrderRepository):
    async def get_by_id_async(self, id):
        order = Order()
        order.id = id
        order.amount = 100
        return order
    
    async def save_async(self, order):
        # Database save implementation
        pass

class StripePaymentService(PaymentService):
    async def process_payment_async(self, amount, card_number):
        # Stripe API implementation
        await asyncio.sleep(0.2)
        return True  # Simulate successful payment

class EmailNotificationService(NotificationService):
    async def send_notification_async(self, message, recipient):
        print(f"Email sent to {recipient}: {message}")
```

---

## Quick Decision Guide

### By Problem Type:

#### **Object Creation Issues**
- **Need single instance?** → Singleton
- **Complex object building?** → Builder
- **Family of related objects?** → Abstract Factory
- **Expensive object creation?** → Object Pool / Prototype

#### **Structure & Organization**
- **Incompatible interfaces?** → Adapter
- **Tree-like structures?** → Composite
- **Add features dynamically?** → Decorator
- **Simplify complex system?** → Facade
- **Memory optimization?** → Flyweight
- **Control access?** → Proxy

#### **Behavior & Communication**
- **Loose coupling needed?** → Observer
- **Multiple algorithms?** → Strategy
- **Undo/Redo functionality?** → Command
- **State-dependent behavior?** → State
- **Request processing chain?** → Chain of Responsibility
- **Sequential data access?** → Iterator
- **External operations?** → Visitor

#### **Architecture & Scale**
- **Web application?** → MVC/MVP/MVVM
- **High performance reads?** → CQRS
- **Audit requirements?** → Event Sourcing
- **Testability?** → Hexagonal Architecture
- **Scalability?** → Microservices
- **API management?** → API Gateway

#### **Modern Challenges**
- **Concurrency?** → Actor Model / Thread Pool
- **Async processing?** → Producer-Consumer
- **Fault tolerance?** → Circuit Breaker
- **Reactive data?** → Reactive Streams
- **Service communication?** → Service Mesh
- **Serverless?** → Function-based patterns

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Web App** | MVC | Repository | Standard web applications |
| **API Service** | API Gateway | Circuit Breaker | Microservices architecture |
| **Data Processing** | Pipeline | Producer-Consumer | Stream processing |
| **Game Development** | State | Observer | Game state management |
| **E-commerce** | CQRS | Event Sourcing | High-performance systems |
| **Mobile App** | MVVM | Repository | Cross-platform development |
| **Legacy Integration** | Adapter | Facade | System integration |
| **Real-time System** | Observer | Reactive Streams | Live data updates |

---

## Final Decision Checklist

### Before Choosing a Pattern:

1. **Identify the core problem**
   - Creation, Structure, or Behavior?

2. **Consider the context**
   - Web app, API, Desktop, Mobile?

3. **Evaluate complexity**
   - Simple solution vs. Over-engineering?

4. **Think about future changes**
   - Will this pattern help or hinder?

5. **Consider team familiarity**
   - Can the team maintain this?

6. **Check performance implications**
   - Does it add overhead?

### Red Flags:
- **Over-engineering** simple problems
- **Using patterns** just because they exist
- **Ignoring** simpler solutions
- **Not considering** maintenance costs
- **Forgetting** about team expertise

---

## 15. Pattern Anti-Patterns

### God Object
**Problem:** When Singleton becomes a monster that knows and does everything.
**Symptoms:** Single class with 1000+ lines, handles multiple responsibilities.
**Solution:** Break into smaller, focused classes using Single Responsibility Principle.

```python
# BAD: God Object
class ApplicationManager:
    def handle_user_login(self): pass
    def process_payment(self): pass
    def send_email(self): pass
    def log_activity(self): pass
    def generate_report(self): pass
    # ... 50 more methods

# GOOD: Separated concerns
class UserService:
    def login(self): pass

class PaymentService:
    def process(self): pass

class EmailService:
    def send(self): pass

class LoggingService:
    def log(self): pass

class ReportService:
    def generate(self): pass
```

### Anemic Domain Model
**Problem:** When Repository pattern goes wrong - entities become just data containers.
**Symptoms:** Business logic scattered in services, entities with only getters/setters.
**Solution:** Move business logic back into domain entities.

```python
# BAD: Anemic model
class Order:
    def __init__(self):
        self.id = None
        self.amount = None
        self.status = None

class OrderService:
    def confirm_order(self, order):
        if order.amount > 0:
            order.status = "Confirmed"

# GOOD: Rich domain model
class Order:
    def __init__(self):
        self.id = None
        self.amount = None
        self.status = None
    
    def confirm(self):
        if self.amount <= 0:
            raise ValueError("Invalid amount")
        self.status = OrderStatus.CONFIRMED
```

---

## 20. Language-Specific Considerations

### Python Specific Implementations

#### **Async/Await Patterns**
```python
import asyncio

# Async Factory
class AsyncFactory:
    @staticmethod
    async def create_async(factory_func):
        return await factory_func()

# Async Observer
class AsyncSubject:
    def __init__(self):
        self.observers = []
    
    def attach(self, observer):
        self.observers.append(observer)
    
    async def notify_async(self):
        tasks = [observer() for observer in self.observers]
        await asyncio.gather(*tasks)
```

#### **Decorator Pattern (Python Decorators)**
```python
# Using Python's built-in decorator syntax
def timing_decorator(func):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start} seconds")
        return result
    return wrapper

@timing_decorator
def expensive_operation():
    time.sleep(1)
    return "done"
```

#### **Context Managers**
```python
from contextlib import contextmanager

@contextmanager
def database_connection():
    conn = create_connection()
    try:
        yield conn
    finally:
        conn.close()

# Usage
with database_connection() as conn:
    conn.execute("SELECT * FROM users")
```

#### **Dependency Injection Integration**
```python
# Using dependency injection frameworks like dependency-injector
from dependency_injector import containers, providers
from dependency_injector.wiring import inject, Provide

class ServiceContainer(containers.DeclarativeContainer):
    logger = providers.Singleton(ConsoleLogger)
    user_repository = providers.Factory(UserRepositoryImpl)
    user_service = providers.Factory(
        UserService,
        user_repository=user_repository
    )

# Pattern with DI
class UserController:
    @inject
    def __init__(self, user_service: UserService = Provide[ServiceContainer.user_service]):
        self.user_service = user_service
```

#### **Pythonic Patterns**
```python
# Using Python's duck typing
class DuckTypedStrategy:
    def execute(self):
        pass

# Using Python's magic methods
class Comparable:
    def __init__(self, value):
        self.value = value
    
    def __lt__(self, other):
        return self.value < other.value
    
    def __eq__(self, other):
        return self.value == other.value

# Using generators for lazy evaluation
def fibonacci_generator():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Using list comprehensions for functional patterns
even_numbers = [x for x in range(10) if x % 2 == 0]
squared = [x**2 for x in range(10)]
```

---

## Conclusion

This comprehensive guide provides a systematic approach to choosing design patterns. Remember:

- **Start simple** - Don't use patterns unless you have a real problem
- **Consider context** - The same problem might need different solutions in different contexts  
- **Think ahead** - Choose patterns that will help with future changes
- **Keep learning** - Patterns are tools, not rules
- **Avoid anti-patterns** - Learn from common mistakes
- **Combine wisely** - Use pattern combinations for complex scenarios
- **Consider performance** - Understand the trade-offs
- **Refactor gradually** - Don't try to fix everything at once

The biggest mistake I see developers make is trying to use every pattern they know. Pick the simplest solution that solves your problem, and refactor later if needed. Most of the time, a simple Factory Method or Strategy pattern will do the job just fine.

Good luck with your pattern matching!

