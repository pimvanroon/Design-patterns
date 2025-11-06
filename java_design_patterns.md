# Complete Design Patterns Guide - Java Edition

A comprehensive guide combining decision flow charts, pattern descriptions, and practical Java code examples. I've compiled this based on years of experience and common mistakes I've seen developers make.

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

```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() { }
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### Factory Method
**Description:** Defines an interface for creating objects, letting subclasses decide which to instantiate. Decouples object creation from usage. Great for when you don't know the exact type at compile time.

```java
// Product interface
interface Product {
    void doSomething();
}

// Concrete products
class ConcreteProductA implements Product {
    public void doSomething() {
        System.out.println("Product A");
    }
}

class ConcreteProductB implements Product {
    public void doSomething() {
        System.out.println("Product B");
    }
}

// Creator abstract class
abstract class Creator {
    public abstract Product factoryMethod();
    
    public void someOperation() {
        Product product = factoryMethod();
        product.doSomething();
    }
}

// Concrete creators
class ConcreteCreatorA extends Creator {
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}
```

### Abstract Factory
**Description:** Creates families of related objects without specifying concrete classes. Helps ensure compatible product families. Perfect for UI frameworks where you need consistent themes.

```java
// Abstract products
interface Button {
    void render();
}

interface Checkbox {
    void render();
}

// Concrete products for Windows
class WinButton implements Button {
    public void render() {
        System.out.println("Windows Button");
    }
}

class WinCheckbox implements Checkbox {
    public void render() {
        System.out.println("Windows Checkbox");
    }
}

// Concrete products for Mac
class MacButton implements Button {
    public void render() {
        System.out.println("Mac Button");
    }
}

class MacCheckbox implements Checkbox {
    public void render() {
        System.out.println("Mac Checkbox");
    }
}

// Abstract factory
interface UIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete factories
class WinFactory implements UIFactory {
    public Button createButton() {
        return new WinButton();
    }
    
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

class MacFactory implements UIFactory {
    public Button createButton() {
        return new MacButton();
    }
    
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

### Builder
**Description:** Separates construction of complex objects from representation. Allows step-by-step creation of objects with optional parts. Excellent for objects with many optional parameters.

```java
// Product to be built
class Car {
    private String engine;
    private String wheels;
    private String color;
    private boolean hasGPS;
    private boolean hasSunroof;
    
    // Getters and setters
    public void setEngine(String engine) { this.engine = engine; }
    public void setWheels(String wheels) { this.wheels = wheels; }
    public void setColor(String color) { this.color = color; }
    public void setGPS(boolean hasGPS) { this.hasGPS = hasGPS; }
    public void setSunroof(boolean hasSunroof) { this.hasSunroof = hasSunroof; }
}

// Builder interface
interface CarBuilder {
    CarBuilder setEngine(String engine);
    CarBuilder setWheels(String wheels);
    CarBuilder setColor(String color);
    CarBuilder setGPS(boolean hasGPS);
    CarBuilder setSunroof(boolean hasSunroof);
    Car build();
}

// Concrete builder
class CarBuilderImpl implements CarBuilder {
    private Car car = new Car();
    
    public CarBuilder setEngine(String engine) {
        car.setEngine(engine);
        return this;
    }
    
    public CarBuilder setWheels(String wheels) {
        car.setWheels(wheels);
        return this;
    }
    
    public CarBuilder setColor(String color) {
        car.setColor(color);
        return this;
    }
    
    public CarBuilder setGPS(boolean hasGPS) {
        car.setGPS(hasGPS);
        return this;
    }
    
    public CarBuilder setSunroof(boolean hasSunroof) {
        car.setSunroof(hasSunroof);
        return this;
    }
    
    public Car build() {
        return car;
    }
}

// Director (optional)
class CarDirector {
    public Car buildSportsCar(CarBuilder builder) {
        return builder
            .setEngine("V8")
            .setWheels("Sports")
            .setColor("Red")
            .setGPS(true)
            .setSunroof(false)
            .build();
    }
}
```

### Prototype
**Description:** Create new objects by cloning existing ones. Useful when object creation is costly. Great for game development where you need many similar objects.

```java
abstract class Shape implements Cloneable {
    public abstract Shape clone();
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class Circle extends Shape {
    public int radius;
    
    public Shape clone() {
        try {
            return (Circle) super.clone();
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }
}
```

---

## 2. Structural Patterns (GoF)

### Adapter
**Description:** Converts one interface to another expected by clients. Allows incompatible interfaces to work together. You'll use this constantly when integrating with third-party libraries.

```java
// Target interface (what client expects)
interface Target {
    void request();
}

// Adaptee (existing incompatible class)
class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request from Adaptee");
    }
}

// Adapter (makes Adaptee compatible with Target)
class Adapter implements Target {
    private final Adaptee adaptee = new Adaptee();
    
    public void request() {
        adaptee.specificRequest();
    }
}

// Client code
class Client {
    public void useTarget(Target target) {
        target.request();
    }
}
```

### Bridge
**Description:** Decouples abstraction from implementation so both can vary independently. Useful for UI/platform independence. Helps avoid the "explosion of classes" problem.

```java
// Implementation interface
interface Device {
    void turnOn();
    void turnOff();
}

// Concrete implementations
class TV implements Device {
    public void turnOn() {
        System.out.println("TV is ON");
    }
    
    public void turnOff() {
        System.out.println("TV is OFF");
    }
}

class Radio implements Device {
    public void turnOn() {
        System.out.println("Radio is ON");
    }
    
    public void turnOff() {
        System.out.println("Radio is OFF");
    }
}

// Abstraction
abstract class Remote {
    protected Device device;
    
    public Remote(Device device) {
        this.device = device;
    }
    
    public abstract void toggle();
}

// Refined abstraction
class AdvancedRemote extends Remote {
    public AdvancedRemote(Device device) {
        super(device);
    }
    
    public void toggle() {
        device.turnOn();
    }
    
    public void mute() {
        System.out.println("Device muted");
    }
}
```

### Composite
**Description:** Treat individual objects and compositions uniformly. Ideal for tree-like structures (folders, UI elements). Makes complex hierarchies easy to work with.

```java
interface Component {
    void display();
}

class Leaf implements Component {
    public void display() {
        System.out.println("Leaf");
    }
}

class Composite implements Component {
    private List<Component> children = new ArrayList<>();
    
    public void add(Component component) {
        children.add(component);
    }
    
    public void display() {
        for (Component child : children) {
            child.display();
        }
    }
}
```

### Decorator
**Description:** Adds responsibilities to objects dynamically. Supports flexible extension without modifying original class. Great for adding features like logging, caching, or validation.

```java
interface Notifier {
    void send(String msg);
}

class EmailNotifier implements Notifier {
    public void send(String msg) {
        System.out.println("Email: " + msg);
    }
}

class SMSDecorator implements Notifier {
    private final Notifier wrappee;
    
    public SMSDecorator(Notifier notifier) {
        this.wrappee = notifier;
    }
    
    public void send(String msg) {
        wrappee.send(msg);
        System.out.println("SMS: " + msg);
    }
}
```

### Facade
**Description:** Provides a unified, simple interface to a set of interfaces in a subsystem. Simplifies complex subsystems for clients. Perfect for wrapping complex APIs.

```java
class CPU {
    public void freeze() { System.out.println("CPU frozen"); }
    public void execute() { System.out.println("CPU executing"); }
}

class Memory {
    public void load() { System.out.println("Memory loaded"); }
}

class ComputerFacade {
    private CPU cpu = new CPU();
    private Memory memory = new Memory();
    
    public void start() {
        cpu.freeze();
        memory.load();
        cpu.execute();
    }
}
```

### Flyweight
**Description:** Uses shared objects to reduce memory usage for large numbers of similar objects. Great for text editors, games, or any scenario with many similar objects.

```java
class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();
    
    public Flyweight get(String key) {
        Flyweight flyweight = flyweights.get(key);
        if (flyweight == null) {
            flyweight = new Flyweight(key);
            flyweights.put(key, flyweight);
        }
        return flyweight;
    }
}

class Flyweight {
    private String key;
    
    public Flyweight(String key) {
        this.key = key;
    }
}
```

### Proxy
**Description:** Provides a placeholder for another object to control access, add lazy loading, caching, or security. Essential for performance optimization and access control.

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    
    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }
    
    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }
    
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;
    
    public ProxyImage(String filename) {
        this.filename = filename;
    }
    
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

---

## 3. Behavioral Patterns (GoF)

### Chain of Responsibility
**Description:** Pass requests along a chain until one handler processes it. Useful for event processing. Great for middleware pipelines and request handling.

```java
abstract class Handler {
    protected Handler next;
    
    public void setNext(Handler handler) {
        this.next = handler;
    }
    
    public abstract void handle(int request);
}

class ConcreteHandler extends Handler {
    public void handle(int request) {
        if (request < 10) {
            System.out.println("Handled");
        } else if (next != null) {
            next.handle(request);
        }
    }
}
```

### Command
**Description:** Encapsulates requests as objects. Enables queuing, logging, and undo/redo operations. Perfect for implementing undo functionality and macro commands.

```java
// Receiver
class Light {
    public void on() {
        System.out.println("Light is ON");
    }
    
    public void off() {
        System.out.println("Light is OFF");
    }
}

// Command interface
interface Command {
    void execute();
    void undo();
}

// Concrete commands
class LightOnCommand implements Command {
    private Light light;
    
    public LightOnCommand(Light light) {
        this.light = light;
    }
    
    public void execute() {
        light.on();
    }
    
    public void undo() {
        light.off();
    }
}

class LightOffCommand implements Command {
    private Light light;
    
    public LightOffCommand(Light light) {
        this.light = light;
    }
    
    public void execute() {
        light.off();
    }
    
    public void undo() {
        light.on();
    }
}

// Invoker
class RemoteControl {
    private Command command;
    
    public void setCommand(Command command) {
        this.command = command;
    }
    
    public void pressButton() {
        if (command != null) {
            command.execute();
        }
    }
    
    public void pressUndo() {
        if (command != null) {
            command.undo();
        }
    }
}
```

### Interpreter
**Description:** Defines a grammar and interprets sentences in that language. Useful for DSLs and expression evaluation. Great for rule engines and configuration languages.

```java
interface Expression {
    int interpret(Map<String, Integer> context);
}

class Number implements Expression {
    private int value;
    
    public Number(int value) {
        this.value = value;
    }
    
    public int interpret(Map<String, Integer> context) {
        return value;
    }
}
```

### Iterator
**Description:** Provides sequential access to elements without exposing underlying structure. Essential for collections and data structures. Makes traversal algorithms reusable.

```java
import java.util.*;

class MyCollection implements Iterable<Integer> {
    private List<Integer> items = Arrays.asList(1, 2, 3);
    
    public Iterator<Integer> iterator() {
        return items.iterator();
    }
}
```

### Mediator
**Description:** Centralizes communication between objects, reducing direct dependencies. Great for complex UI interactions and chat systems. Reduces coupling between components.

```java
// Mediator interface
interface ChatMediator {
    void register(User user);
    void send(String message, User sender);
}

// Concrete mediator
class ChatMediatorImpl implements ChatMediator {
    private List<User> users = new ArrayList<>();
    
    public void register(User user) {
        users.add(user);
    }
    
    public void send(String message, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(message);
            }
        }
    }
}

// Colleague
class User {
    private String name;
    private ChatMediator mediator;
    
    public User(String name, ChatMediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }
    
    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.send(message, this);
    }
    
    public void receive(String message) {
        System.out.println(name + " receives: " + message);
    }
}
```

### Memento
**Description:** Captures and restores object state without violating encapsulation. Supports undo functionality. Perfect for implementing save/restore features.

```java
class Memento {
    private final String state;
    
    public Memento(String state) {
        this.state = state;
    }
    
    public String getState() {
        return state;
    }
}

class Originator {
    private String state;
    
    public void setState(String state) {
        this.state = state;
    }
    
    public Memento save() {
        return new Memento(state);
    }
    
    public void restore(Memento memento) {
        state = memento.getState();
    }
}
```

### Observer
**Description:** Defines a one-to-many dependency: when one object changes state, all dependents are notified. Foundation of event-driven programming. Essential for MVC and reactive systems.

```java
import java.util.*;

interface Observer {
    void update();
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}
```

### State
**Description:** Allows an object to alter behavior when its internal state changes. Perfect for state machines and complex conditional logic. Makes state transitions explicit.

```java
interface State {
    void handle(Context context);
}

class Context {
    public State state;
    
    public void request() {
        state.handle(this);
    }
}
```

### Strategy
**Description:** Encapsulates interchangeable algorithms. Clients can switch strategies at runtime. Great for sorting algorithms, payment methods, or any interchangeable behavior.

```java
interface Strategy {
    void execute();
}

class QuickSort implements Strategy {
    public void execute() {
        System.out.println("QuickSort");
    }
}

class Sorter {
    private Strategy strategy;
    
    public Sorter(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public void sort() {
        strategy.execute();
    }
}
```

### Template Method
**Description:** Defines skeleton of algorithm, deferring certain steps to subclasses. Great for frameworks and libraries. Ensures consistent algorithm structure.

```java
abstract class Game {
    public void play() {
        init();
        start();
        end();
    }
    
    protected abstract void init();
    protected abstract void start();
    protected abstract void end();
}
```

### Visitor
**Description:** Represents an operation to be performed on object structures without modifying the objects themselves. Great for adding operations to existing class hierarchies.

```java
interface Visitor {
    void visit(Element element);
}

abstract class Element {
    public abstract void accept(Visitor visitor);
}

class ConcreteElement extends Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

---

## 4. Modern Extensions

### Null Object
**Description:** Provides a do-nothing implementation instead of null, avoiding null checks. Eliminates null pointer exceptions and makes code more robust.

```java
interface Logger {
    void log(String msg);
}

class ConsoleLogger implements Logger {
    public void log(String msg) {
        System.out.println(msg);
    }
}

class NullLogger implements Logger {
    public void log(String msg) {
        // Do nothing
    }
}
```

### MVC (Model-View-Controller)
**Description:** Separates application logic into Model (data), View (UI), Controller (input). Promotes separation of concerns. The foundation of most web frameworks.

```java
// Model
class User {
    private String name;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}

// View
class UserView {
    public void render(User user) {
        System.out.println("Hello " + user.getName());
    }
}

// Controller
class UserController {
    private User model;
    private UserView view;
    
    public UserController(User model, UserView view) {
        this.model = model;
        this.view = view;
    }
    
    public void updateView() {
        view.render(model);
    }
}
```

### Dependency Injection / Service Locator
**Description:** Externalizes object creation and dependency resolution. Improves flexibility and testability. Essential for modern application architecture.

```java
import java.util.*;

class ServiceLocator {
    private static Map<Class<?>, Object> services = new HashMap<>();
    
    public static <T> void register(Class<T> type, T service) {
        services.put(type, service);
    }
    
    @SuppressWarnings("unchecked")
    public static <T> T get(Class<T> type) {
        return (T) services.get(type);
    }
}
```

### Repository / DAO
**Description:** Abstracts data layer, providing a clean interface for CRUD operations. Makes data access testable and allows easy switching between data sources.

```java
import java.util.*;

// Domain entity
class User {
    private int id;
    private String name;
    private String email;
    
    // Getters and setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

// Repository interface
interface UserRepository {
    User getById(int id);
    List<User> getAll();
    void save(User user);
    void delete(int id);
}

// Concrete repository
class UserRepositoryImpl implements UserRepository {
    private List<User> users = new ArrayList<>();
    
    public User getById(int id) {
        return users.stream()
            .filter(u -> u.getId() == id)
            .findFirst()
            .orElse(null);
    }
    
    public List<User> getAll() {
        return new ArrayList<>(users);
    }
    
    public void save(User user) {
        User existing = getById(user.getId());
        if (existing != null) {
            users.remove(existing);
        }
        users.add(user);
    }
    
    public void delete(int id) {
        User user = getById(id);
        if (user != null) {
            users.remove(user);
        }
    }
}
```

### MVP (Model-View-Presenter)
**Description:** Similar to MVC but with tighter coupling between View and Presenter. The Presenter handles all UI logic. Great for desktop applications.

```java
// Model
class UserModel {
    private String name;
    private String email;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

// View interface
interface UserView {
    String getUserName();
    void setUserName(String name);
    String getUserEmail();
    void setUserEmail(String email);
    void showMessage(String message);
}

// Presenter
class UserPresenter {
    private final UserView view;
    private final UserModel model;
    
    public UserPresenter(UserView view, UserModel model) {
        this.view = view;
        this.model = model;
    }
    
    public void loadUser() {
        view.setUserName(model.getName());
        view.setUserEmail(model.getEmail());
    }
    
    public void saveUser() {
        model.setName(view.getUserName());
        model.setEmail(view.getUserEmail());
        view.showMessage("User saved successfully!");
    }
}
```

### MVVM (Model-View-ViewModel)
**Description:** Designed for modern UI frameworks. Uses data binding and commands. The ViewModel acts as a data converter and command handler.

```java
import java.beans.PropertyChangeListener;
import java.beans.PropertyChangeSupport;

// Model
class User {
    private String name;
    private String email;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

// ViewModel
class UserViewModel {
    private User user;
    private String name;
    private String email;
    private PropertyChangeSupport propertyChangeSupport = new PropertyChangeSupport(this);
    
    public UserViewModel(User user) {
        this.user = user;
        this.name = user.getName();
        this.email = user.getEmail();
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        String oldName = this.name;
        this.name = name;
        propertyChangeSupport.firePropertyChange("name", oldName, name);
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        String oldEmail = this.email;
        this.email = email;
        propertyChangeSupport.firePropertyChange("email", oldEmail, email);
    }
    
    public void save() {
        user.setName(name);
        user.setEmail(email);
        // Save logic here
    }
    
    public void addPropertyChangeListener(PropertyChangeListener listener) {
        propertyChangeSupport.addPropertyChangeListener(listener);
    }
}
```

### CQRS (Command Query Responsibility Segregation)
**Description:** Separates read and write operations. Uses different models for commands and queries. Great for high-performance systems with complex business logic.

```java
import java.util.concurrent.CompletableFuture;

// Command side
interface Command { }

class CreateUserCommand implements Command {
    private String name;
    private String email;
    
    public CreateUserCommand(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    public String getName() { return name; }
    public String getEmail() { return email; }
}

interface CommandHandler<T extends Command> {
    CompletableFuture<Void> handle(T command);
}

class CreateUserCommandHandler implements CommandHandler<CreateUserCommand> {
    public CompletableFuture<Void> handle(CreateUserCommand command) {
        // Write to database
        System.out.println("Creating user: " + command.getName());
        return CompletableFuture.completedFuture(null);
    }
}

// Query side
interface Query<TResult> { }

class GetUserQuery implements Query<UserDto> {
    private int userId;
    
    public GetUserQuery(int userId) {
        this.userId = userId;
    }
    
    public int getUserId() { return userId; }
}

interface QueryHandler<TQuery extends Query<TResult>, TResult> {
    CompletableFuture<TResult> handle(TQuery query);
}

class GetUserQueryHandler implements QueryHandler<GetUserQuery, UserDto> {
    public CompletableFuture<UserDto> handle(GetUserQuery query) {
        // Read from database
        UserDto dto = new UserDto();
        dto.setId(query.getUserId());
        dto.setName("User Name");
        return CompletableFuture.completedFuture(dto);
    }
}

class UserDto {
    private int id;
    private String name;
    
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

### Event Sourcing
**Description:** Stores changes as a sequence of events rather than current state. Enables audit trails and time travel. Perfect for financial systems and audit requirements.

```java
import java.util.*;
import java.time.Instant;
import java.util.UUID;

// Domain event
abstract class DomainEvent {
    private Instant occurredAt;
    private UUID id;
    
    public DomainEvent() {
        this.occurredAt = Instant.now();
        this.id = UUID.randomUUID();
    }
    
    public Instant getOccurredAt() { return occurredAt; }
    public UUID getId() { return id; }
}

class UserCreatedEvent extends DomainEvent {
    private String name;
    private String email;
    
    public UserCreatedEvent(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    public String getName() { return name; }
    public String getEmail() { return email; }
}

class UserEmailChangedEvent extends DomainEvent {
    private String oldEmail;
    private String newEmail;
    
    public UserEmailChangedEvent(String oldEmail, String newEmail) {
        this.oldEmail = oldEmail;
        this.newEmail = newEmail;
    }
    
    public String getOldEmail() { return oldEmail; }
    public String getNewEmail() { return newEmail; }
}

// Aggregate root
class User {
    private List<DomainEvent> events = new ArrayList<>();
    private UUID id;
    private String name;
    private String email;
    
    public static User create(String name, String email) {
        User user = new User();
        user.apply(new UserCreatedEvent(name, email));
        return user;
    }
    
    public void changeEmail(String newEmail) {
        String oldEmail = this.email;
        apply(new UserEmailChangedEvent(oldEmail, newEmail));
    }
    
    private void apply(DomainEvent event) {
        events.add(event);
        // Apply event to current state
        if (event instanceof UserCreatedEvent) {
            UserCreatedEvent e = (UserCreatedEvent) event;
            this.id = e.getId();
            this.name = e.getName();
            this.email = e.getEmail();
        } else if (event instanceof UserEmailChangedEvent) {
            UserEmailChangedEvent e = (UserEmailChangedEvent) event;
            this.email = e.getNewEmail();
        }
    }
    
    public List<DomainEvent> getUncommittedEvents() {
        return new ArrayList<>(events);
    }
    
    public void markEventsAsCommitted() {
        events.clear();
    }
    
    public UUID getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
}
```

---

## 5. Additional Patterns

### Object Pool
**Description:** Reuses objects that are expensive to create (e.g., database connections). Reduces creation overhead. Essential for high-performance applications.

```java
import java.util.*;

class ObjectPool<T> {
    private Stack<T> items = new Stack<>();
    private Supplier<T> factory;
    
    public ObjectPool(Supplier<T> factory) {
        this.factory = factory;
    }
    
    public T get() {
        if (items.isEmpty()) {
            return factory.get();
        }
        return items.pop();
    }
    
    public void returnItem(T obj) {
        items.push(obj);
    }
}

interface Supplier<T> {
    T get();
}
```

### Lazy Initialization
**Description:** Delays object creation until actually needed. Saves memory and computation. Great for expensive resources that might not be used.

```java
class Lazy<T> {
    private T value;
    private Supplier<T> supplier;
    
    public Lazy(Supplier<T> supplier) {
        this.supplier = supplier;
    }
    
    public T get() {
        if (value == null) {
            value = supplier.get();
        }
        return value;
    }
}
```

### Multiton
**Description:** Like Singleton, but allows a limited number of instances keyed by a value. Useful for resource management with multiple instances.

```java
import java.util.*;

class Multiton {
    private static Map<String, Multiton> instances = new HashMap<>();
    
    private Multiton() { }
    
    public static synchronized Multiton get(String key) {
        Multiton instance = instances.get(key);
        if (instance == null) {
            instance = new Multiton();
            instances.put(key, instance);
        }
        return instance;
    }
}
```

### Pipeline
**Description:** Processes data sequentially through stages. Often used in streaming or functional pipelines. Great for data transformation workflows.

```java
import java.util.*;
import java.util.function.Function;

class Pipeline<T> {
    private List<Function<T, T>> stages = new ArrayList<>();
    
    public Pipeline<T> addStage(Function<T, T> stage) {
        stages.add(stage);
        return this;
    }
    
    public T process(T input) {
        T result = input;
        for (Function<T, T> stage : stages) {
            result = stage.apply(result);
        }
        return result;
    }
}

// Usage
Pipeline<Integer> pipeline = new Pipeline<Integer>()
    .addStage(x -> x + 1)
    .addStage(x -> x * 2);
int result = pipeline.process(1);
```

### Event Bus / Event Aggregator
**Description:** Decouples publishers and subscribers. Enables scalable event-driven architecture. Perfect for microservices communication.

```java
import java.util.*;

class EventBus {
    private List<EventListener> listeners = new ArrayList<>();
    
    public void subscribe(EventListener listener) {
        listeners.add(listener);
    }
    
    public void publish(String message) {
        for (EventListener listener : listeners) {
            listener.onMessage(message);
        }
    }
}

interface EventListener {
    void onMessage(String message);
}
```

### Specification
**Description:** Encapsulates business rules into reusable predicates. Allows combining rules using logical operators. Great for complex business logic.

```java
interface Specification<T> {
    boolean isSatisfied(T item);
}

class EvenSpecification implements Specification<Integer> {
    public boolean isSatisfied(Integer item) {
        return item % 2 == 0;
    }
}
```

---

## 6. Functional Programming Patterns

### Monad
**Description:** Encapsulates computations with side effects in a composable way. Essential for functional programming and error handling. Makes error handling explicit and composable.

```java
// Maybe/Option monad
class Maybe<T> {
    private final T value;
    private final boolean hasValue;
    
    private Maybe(T value, boolean hasValue) {
        this.value = value;
        this.hasValue = hasValue;
    }
    
    public static <T> Maybe<T> some(T value) {
        return new Maybe<>(value, true);
    }
    
    public static <T> Maybe<T> none() {
        return new Maybe<>(null, false);
    }
    
    public <U> Maybe<U> bind(Function<T, Maybe<U>> f) {
        return hasValue ? f.apply(value) : Maybe.none();
    }
    
    public <U> Maybe<U> map(Function<T, U> f) {
        return hasValue ? Maybe.some(f.apply(value)) : Maybe.none();
    }
}

interface Function<T, R> {
    R apply(T t);
}
```

### Functor
**Description:** Provides a way to apply functions to values wrapped in a context (like Option, List). Enables functional composition. Foundation of functional programming.

```java
import java.util.*;
import java.util.function.Function;

interface Functor<T> {
    <U> Functor<U> map(Function<T, U> f);
}

class ListFunctor<T> implements Functor<T> {
    private final List<T> items;
    
    public ListFunctor(List<T> items) {
        this.items = items;
    }
    
    public <U> Functor<U> map(Function<T, U> f) {
        List<U> result = new ArrayList<>();
        for (T item : items) {
            result.add(f.apply(item));
        }
        return new ListFunctor<>(result);
    }
}
```

### Fold/Reduce
**Description:** Aggregates collections into single values. Fundamental for functional data processing. Essential for data analysis and transformation.

```java
import java.util.*;
import java.util.function.BinaryOperator;

class FunctionalExtensions {
    public static <T, U> U fold(List<T> source, U seed, BinaryOperator<U> func) {
        U result = seed;
        for (T item : source) {
            result = func.apply(result, (U) item);
        }
        return result;
    }
}

// Usage: Integer sum = FunctionalExtensions.fold(numbers, 0, (acc, x) -> acc + x);
```

---

## 7. Concurrency & Parallelism Patterns

### Actor Model
**Description:** Objects communicate through message passing. Excellent for concurrent systems and distributed computing. Eliminates shared state issues.

```java
import java.util.*;

interface Actor {
    void receive(Object message);
}

class SimpleActor implements Actor {
    public void receive(Object message) {
        if (message instanceof String) {
            System.out.println("Received: " + message);
        } else if (message instanceof Integer) {
            System.out.println("Number: " + message);
        }
    }
}

class ActorSystem {
    private Map<String, Actor> actors = new HashMap<>();
    
    public void register(String name, Actor actor) {
        actors.put(name, actor);
    }
    
    public void send(String actorName, Object message) {
        Actor actor = actors.get(actorName);
        if (actor != null) {
            actor.receive(message);
        }
    }
}
```

### Producer-Consumer
**Description:** Decouples data production from consumption using queues. Essential for asynchronous processing. Great for background processing and data pipelines.

```java
import java.util.*;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

class ProducerConsumer<T> {
    private BlockingQueue<T> queue = new LinkedBlockingQueue<>();
    
    public void produce(T item) throws InterruptedException {
        queue.put(item);
    }
    
    public T consume() throws InterruptedException {
        return queue.take();
    }
}
```

### Thread Pool
**Description:** Manages a pool of worker threads to execute tasks. Improves performance and resource management. Essential for scalable server applications.

```java
import java.util.*;
import java.util.concurrent.*;

class ThreadPool {
    private BlockingQueue<Runnable> tasks = new LinkedBlockingQueue<>();
    private List<Thread> workers = new ArrayList<>();
    private volatile boolean shutdown = false;
    
    public ThreadPool(int workerCount) {
        for (int i = 0; i < workerCount; i++) {
            Thread worker = new Thread(this::workerLoop);
            worker.setDaemon(true);
            worker.start();
            workers.add(worker);
        }
    }
    
    public void queueTask(Runnable task) {
        if (!shutdown) {
            tasks.offer(task);
        }
    }
    
    private void workerLoop() {
        while (!shutdown) {
            try {
                Runnable task = tasks.take();
                task.run();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
    
    public void shutdown() {
        shutdown = true;
        for (Thread worker : workers) {
            worker.interrupt();
        }
    }
}
```

---

## 8. Reactive Programming Patterns

### Circuit Breaker
**Description:** Prevents cascading failures by temporarily stopping calls to failing services. Critical for microservices. Essential for resilient distributed systems.

```java
import java.time.Instant;
import java.time.Duration;
import java.util.function.Supplier;

enum CircuitState {
    CLOSED, OPEN, HALF_OPEN
}

class CircuitBreaker {
    private CircuitState state = CircuitState.CLOSED;
    private int failureCount = 0;
    private Instant lastFailureTime;
    private final int threshold;
    private final Duration timeout;
    
    public CircuitBreaker(int threshold, Duration timeout) {
        this.threshold = threshold;
        this.timeout = timeout;
    }
    
    public <T> T execute(Supplier<T> operation) {
        if (state == CircuitState.OPEN) {
            if (Duration.between(lastFailureTime, Instant.now()).compareTo(timeout) > 0) {
                state = CircuitState.HALF_OPEN;
            } else {
                throw new IllegalStateException("Circuit breaker is open");
            }
        }
        
        try {
            T result = operation.get();
            onSuccess();
            return result;
        } catch (Exception ex) {
            onFailure();
            throw ex;
        }
    }
    
    private void onSuccess() {
        failureCount = 0;
        state = CircuitState.CLOSED;
    }
    
    private void onFailure() {
        failureCount++;
        lastFailureTime = Instant.now();
        if (failureCount >= threshold) {
            state = CircuitState.OPEN;
        }
    }
}
```

### Saga
**Description:** Manages distributed transactions across multiple services. Essential for microservices architectures. Provides eventual consistency in distributed systems.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface SagaStep {
    CompletableFuture<Void> execute();
    CompletableFuture<Void> compensate();
}

class OrderSaga {
    private List<SagaStep> steps = new ArrayList<>();
    private List<SagaStep> completedSteps = new ArrayList<>();
    
    public void addStep(SagaStep step) {
        steps.add(step);
    }
    
    public CompletableFuture<Void> execute() {
        try {
            for (SagaStep step : steps) {
                step.execute().join();
                completedSteps.add(step);
            }
            return CompletableFuture.completedFuture(null);
        } catch (Exception e) {
            return compensate().thenCompose(v -> CompletableFuture.failedFuture(e));
        }
    }
    
    private CompletableFuture<Void> compensate() {
        CompletableFuture<Void> future = CompletableFuture.completedFuture(null);
        Collections.reverse(completedSteps);
        for (SagaStep step : completedSteps) {
            future = future.thenCompose(v -> step.compensate());
        }
        return future;
    }
}
```

---

## 9. Cloud & Distributed Patterns

### API Gateway
**Description:** Single entry point for client requests to microservices. Simplifies client-server communication. Essential for microservices architecture.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface Microservice {
    CompletableFuture<String> processRequest(String request);
}

class APIGateway {
    private Map<String, Microservice> services = new HashMap<>();
    
    public void registerService(String path, Microservice service) {
        services.put(path, service);
    }
    
    public CompletableFuture<String> routeRequest(String path, String request) {
        Microservice service = services.get(path);
        if (service != null) {
            return service.processRequest(request);
        }
        throw new IllegalArgumentException("No service found for path: " + path);
    }
}
```

### Service Mesh
**Description:** Handles service-to-service communication in microservices. Provides observability and security. Essential for production microservices deployments.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface ServiceMesh {
    <T> CompletableFuture<T> callService(String serviceName, String endpoint, Object data);
}

class ServiceMeshImpl implements ServiceMesh {
    private Map<String, String> serviceUrls = new HashMap<>();
    
    public void registerService(String name, String url) {
        serviceUrls.put(name, url);
    }
    
    public <T> CompletableFuture<T> callService(String serviceName, String endpoint, Object data) {
        String url = serviceUrls.get(serviceName);
        if (url == null) {
            return CompletableFuture.failedFuture(
                new IllegalArgumentException("Service " + serviceName + " not found")
            );
        }
        
        String fullUrl = url + "/" + endpoint;
        System.out.println("Calling " + fullUrl + " with data: " + data);
        
        // Return mock response
        return CompletableFuture.completedFuture(null);
    }
}
```

---

## 10. Data Access Patterns

### Unit of Work
**Description:** Maintains a list of objects affected by a business transaction. Ensures data consistency. Essential for maintaining transactional integrity.

```java
import java.util.*;

interface UnitOfWork {
    <T> void registerNew(T entity);
    <T> void registerDirty(T entity);
    <T> void registerDeleted(T entity);
    CompletableFuture<Void> commit();
    void rollback();
}

class UnitOfWorkImpl implements UnitOfWork {
    private List<Object> newEntities = new ArrayList<>();
    private List<Object> dirtyEntities = new ArrayList<>();
    private List<Object> deletedEntities = new ArrayList<>();
    
    public <T> void registerNew(T entity) {
        newEntities.add(entity);
    }
    
    public <T> void registerDirty(T entity) {
        dirtyEntities.add(entity);
    }
    
    public <T> void registerDeleted(T entity) {
        deletedEntities.add(entity);
    }
    
    public CompletableFuture<Void> commit() {
        // Save all changes atomically
        System.out.println("Committing all changes...");
        return CompletableFuture.completedFuture(null);
    }
    
    public void rollback() {
        newEntities.clear();
        dirtyEntities.clear();
        deletedEntities.clear();
        System.out.println("Rolled back all changes");
    }
}
```

### Identity Map
**Description:** Ensures each object is loaded only once per session. Prevents duplicate objects in memory. Essential for ORM frameworks and data consistency.

```java
import java.util.*;

class IdentityMap<T> {
    private Map<Object, T> map = new HashMap<>();
    
    public T get(Object key) {
        return map.get(key);
    }
    
    public void put(Object key, T entity) {
        map.put(key, entity);
    }
    
    public boolean contains(Object key) {
        return map.containsKey(key);
    }
    
    public void remove(Object key) {
        map.remove(key);
    }
    
    public void clear() {
        map.clear();
    }
}
```

---

## 11. Security Patterns

### Authentication
**Description:** Verifies user identity. Fundamental security requirement. Essential for any system that needs to identify users.

```java
import java.util.concurrent.CompletableFuture;

interface AuthenticationService {
    CompletableFuture<Boolean> authenticateAsync(String username, String password);
    CompletableFuture<String> generateTokenAsync(String username);
}

class AuthenticationServiceImpl implements AuthenticationService {
    public CompletableFuture<Boolean> authenticateAsync(String username, String password) {
        // Simulate authentication logic
        return CompletableFuture.completedFuture(
            "admin".equals(username) && "password".equals(password)
        );
    }
    
    public CompletableFuture<String> generateTokenAsync(String username) {
        return CompletableFuture.completedFuture(
            "token_" + username + "_" + System.currentTimeMillis()
        );
    }
}
```

### Authorization
**Description:** Controls access to resources based on user permissions. Essential for secure systems. Implements the principle of least privilege.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface AuthorizationService {
    CompletableFuture<Boolean> isAuthorizedAsync(String user, String resource, String action);
}

class AuthorizationServiceImpl implements AuthorizationService {
    private Map<String, List<String>> permissions = new HashMap<>();
    
    public AuthorizationServiceImpl() {
        permissions.put("admin", Arrays.asList("read", "write", "delete"));
        permissions.put("user", Arrays.asList("read"));
    }
    
    public CompletableFuture<Boolean> isAuthorizedAsync(String user, String resource, String action) {
        List<String> userPermissions = permissions.get(user);
        return CompletableFuture.completedFuture(
            userPermissions != null && userPermissions.contains(action)
        );
    }
}
```

---

## 12. Performance Patterns

### Caching
**Description:** Stores frequently accessed data in fast storage. Improves application performance. Essential for high-performance applications.

```java
import java.util.*;
import java.time.Instant;
import java.time.Duration;

interface Cache<T> {
    T get(String key);
    void set(String key, T value, Duration expiry);
    void remove(String key);
    void clear();
}

class CacheItem<T> {
    private T value;
    private Instant expiry;
    
    public CacheItem(T value, Instant expiry) {
        this.value = value;
        this.expiry = expiry;
    }
    
    public T getValue() { return value; }
    public boolean isExpired() { return Instant.now().isAfter(expiry); }
}

class MemoryCache<T> implements Cache<T> {
    private Map<String, CacheItem<T>> cache = new HashMap<>();
    
    public T get(String key) {
        CacheItem<T> item = cache.get(key);
        if (item != null && !item.isExpired()) {
            return item.getValue();
        }
        return null;
    }
    
    public void set(String key, T value, Duration expiry) {
        Instant expiryTime = Instant.now().plus(expiry);
        cache.put(key, new CacheItem<>(value, expiryTime));
    }
    
    public void remove(String key) {
        cache.remove(key);
    }
    
    public void clear() {
        cache.clear();
    }
}
```

### Connection Pooling
**Description:** Reuses database connections. Improves performance and resource utilization. Essential for database-intensive applications.

```java
import java.util.*;
import java.sql.Connection;

class ConnectionPool {
    private Queue<Connection> availableConnections = new LinkedList<>();
    private List<Connection> allConnections = new ArrayList<>();
    private final int maxConnections;
    
    public ConnectionPool(int maxConnections) {
        this.maxConnections = maxConnections;
    }
    
    public synchronized Connection getConnection() {
        if (!availableConnections.isEmpty()) {
            return availableConnections.poll();
        }
        
        if (allConnections.size() < maxConnections) {
            Connection connection = createConnection();
            allConnections.add(connection);
            return connection;
        }
        
        throw new IllegalStateException("No connections available");
    }
    
    public synchronized void returnConnection(Connection connection) {
        availableConnections.offer(connection);
    }
    
    private Connection createConnection() {
        // Create actual database connection
        return null; // Placeholder
    }
}
```

---

## 13. Testing Patterns

### Test Double
**Description:** Replaces real objects with test-specific objects. Essential for unit testing. Enables isolated testing of components.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface UserService {
    CompletableFuture<User> getUserAsync(int id);
}

class UserServiceImpl implements UserService {
    public CompletableFuture<User> getUserAsync(int id) {
        // Real implementation
        User user = new User();
        user.setId(id);
        user.setName("Real User");
        return CompletableFuture.completedFuture(user);
    }
}

class TestUserService implements UserService {
    private Map<Integer, User> users = new HashMap<>();
    
    public void addUser(User user) {
        users.put(user.getId(), user);
    }
    
    public CompletableFuture<User> getUserAsync(int id) {
        return CompletableFuture.completedFuture(users.get(id));
    }
}
```

### Mock Object
**Description:** Simulates behavior of real objects for testing. Enables isolated testing. Essential for testing interactions between objects.

```java
import java.util.*;
import java.util.concurrent.CompletableFuture;

interface EmailService {
    CompletableFuture<Void> sendEmailAsync(String to, String subject, String body);
}

class MockEmailService implements EmailService {
    private List<String> sentEmails = new ArrayList<>();
    private boolean shouldThrowException = false;
    
    public List<String> getSentEmails() {
        return sentEmails;
    }
    
    public void setShouldThrowException(boolean shouldThrowException) {
        this.shouldThrowException = shouldThrowException;
    }
    
    public CompletableFuture<Void> sendEmailAsync(String to, String subject, String body) {
        if (shouldThrowException) {
            return CompletableFuture.failedFuture(
                new IllegalStateException("Email service unavailable")
            );
        }
        
        sentEmails.add("To: " + to + ", Subject: " + subject);
        return CompletableFuture.completedFuture(null);
    }
}
```

---

## 14. Modern Architectural Patterns

### Clean Architecture
**Description:** Separates concerns into layers with dependency inversion. Promotes maintainable code. Essential for large, complex applications.

```java
import java.util.concurrent.CompletableFuture;

// Domain Layer (Core)
class User {
    private int id;
    private String name;
    private String email;
    
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

interface UserRepository {
    CompletableFuture<User> getByIdAsync(int id);
    CompletableFuture<Void> saveAsync(User user);
}

// Application Layer
class UserService {
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public CompletableFuture<User> getUserAsync(int id) {
        return userRepository.getByIdAsync(id);
    }
}

// Infrastructure Layer
class SqlUserRepository implements UserRepository {
    public CompletableFuture<User> getByIdAsync(int id) {
        // Database implementation
        User user = new User();
        user.setId(id);
        user.setName("User from DB");
        return CompletableFuture.completedFuture(user);
    }
    
    public CompletableFuture<Void> saveAsync(User user) {
        // Database save implementation
        return CompletableFuture.completedFuture(null);
    }
}
```

### Domain-Driven Design (DDD)
**Description:** Focuses on business domain modeling. Improves software design. Essential for complex business applications.

```java
import java.util.*;

// Domain Entity
enum OrderStatus {
    DRAFT, CONFIRMED
}

class OrderItem {
    private Product product;
    private int quantity;
    private double price;
    
    public OrderItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
        this.price = product.getPrice();
    }
    
    public double getPrice() { return price; }
    public int getQuantity() { return quantity; }
}

class Order {
    private List<OrderItem> items = new ArrayList<>();
    private int id;
    private Customer customer;
    private OrderStatus status;
    
    public double getTotalAmount() {
        return items.stream()
            .mapToDouble(item -> item.getPrice() * item.getQuantity())
            .sum();
    }
    
    public void addItem(Product product, int quantity) {
        if (status != OrderStatus.DRAFT) {
            throw new IllegalStateException("Cannot modify confirmed order");
        }
        items.add(new OrderItem(product, quantity));
    }
    
    public void confirm() {
        if (items.isEmpty()) {
            throw new IllegalStateException("Cannot confirm empty order");
        }
        status = OrderStatus.CONFIRMED;
    }
    
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public OrderStatus getStatus() { return status; }
}

// Domain Service
class OrderPricingService {
    public double calculateDiscount(Order order) {
        if (order.getTotalAmount() > 1000) {
            return order.getTotalAmount() * 0.1; // 10% discount
        }
        return 0;
    }
}
```

### Hexagonal Architecture (Ports and Adapters)
**Description:** Separates business logic from external concerns using ports and adapters. Makes applications testable and technology-agnostic. Essential for maintainable enterprise applications.

```java
import java.util.concurrent.CompletableFuture;

// Domain (Core) - Business Logic
enum OrderStatus {
    PROCESSING, COMPLETED
}

class Order {
    private int id;
    private double amount;
    private OrderStatus status;
    
    public void process() {
        if (amount <= 0) {
            throw new IllegalStateException("Invalid amount");
        }
        status = OrderStatus.PROCESSING;
    }
    
    public void complete() {
        status = OrderStatus.COMPLETED;
    }
    
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public double getAmount() { return amount; }
    public void setAmount(double amount) { this.amount = amount; }
    public OrderStatus getStatus() { return status; }
}

// Ports (Interfaces) - Define contracts
interface OrderRepository {
    CompletableFuture<Order> getByIdAsync(int id);
    CompletableFuture<Void> saveAsync(Order order);
}

interface PaymentService {
    CompletableFuture<Boolean> processPaymentAsync(double amount, String cardNumber);
}

interface NotificationService {
    CompletableFuture<Void> sendNotificationAsync(String message, String recipient);
}

// Application Services - Orchestrate business logic
class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final NotificationService notificationService;
    
    public OrderService(
            OrderRepository orderRepository,
            PaymentService paymentService,
            NotificationService notificationService) {
        this.orderRepository = orderRepository;
        this.paymentService = paymentService;
        this.notificationService = notificationService;
    }
    
    public CompletableFuture<Void> processOrderAsync(int orderId, String cardNumber) {
        return orderRepository.getByIdAsync(orderId)
            .thenCompose(order -> {
                order.process();
                return paymentService.processPaymentAsync(order.getAmount(), cardNumber)
                    .thenCompose(success -> {
                        if (success) {
                            order.complete();
                            return notificationService.sendNotificationAsync(
                                "Order completed", "customer@email.com"
                            ).thenCompose(v -> orderRepository.saveAsync(order));
                        }
                        return orderRepository.saveAsync(order);
                    });
            });
    }
}

// Adapters (Infrastructure) - Implement ports
class SqlOrderRepository implements OrderRepository {
    public CompletableFuture<Order> getByIdAsync(int id) {
        Order order = new Order();
        order.setId(id);
        order.setAmount(100);
        return CompletableFuture.completedFuture(order);
    }
    
    public CompletableFuture<Void> saveAsync(Order order) {
        // Database save implementation
        return CompletableFuture.completedFuture(null);
    }
}

class StripePaymentService implements PaymentService {
    public CompletableFuture<Boolean> processPaymentAsync(double amount, String cardNumber) {
        // Stripe API implementation
        return CompletableFuture.completedFuture(true); // Simulate successful payment
    }
}

class EmailNotificationService implements NotificationService {
    public CompletableFuture<Void> sendNotificationAsync(String message, String recipient) {
        System.out.println("Email sent to " + recipient + ": " + message);
        return CompletableFuture.completedFuture(null);
    }
}
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

```java
// BAD: God Object
class ApplicationManager {
    public void handleUserLogin() { }
    public void processPayment() { }
    public void sendEmail() { }
    public void logActivity() { }
    public void generateReport() { }
    // ... 50 more methods
}

// GOOD: Separated concerns
class UserService { public void login() { } }
class PaymentService { public void process() { } }
class EmailService { public void send() { } }
class LoggingService { public void log() { } }
class ReportService { public void generate() { } }
```

### Anemic Domain Model
**Problem:** When Repository pattern goes wrong - entities become just data containers.
**Symptoms:** Business logic scattered in services, entities with only getters/setters.
**Solution:** Move business logic back into domain entities.

```java
// BAD: Anemic model
class Order {
    private int id;
    private double amount;
    private String status;
    
    // Only getters and setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public double getAmount() { return amount; }
    public void setAmount(double amount) { this.amount = amount; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
}

class OrderService {
    public void confirmOrder(Order order) {
        if (order.getAmount() > 0) {
            order.setStatus("Confirmed");
        }
    }
}

// GOOD: Rich domain model
class Order {
    private int id;
    private double amount;
    private OrderStatus status;
    
    public void confirm() {
        if (amount <= 0) {
            throw new IllegalStateException("Invalid amount");
        }
        status = OrderStatus.CONFIRMED;
    }
    
    public int getId() { return id; }
    public double getAmount() { return amount; }
    public OrderStatus getStatus() { return status; }
}
```

---

## 16. Pattern Combinations & Recipes

### MVC + Repository + Unit of Work
**Use Case:** Classic web application stack
**Benefits:** Clean separation, testable, maintainable

```java
import java.util.concurrent.CompletableFuture;

// Model
class User {
    private int id;
    private String name;
}

// Repository
interface UserRepository {
    User getById(int id);
    void save(User user);
}

// Unit of Work
interface UnitOfWork {
    UserRepository getUsers();
    CompletableFuture<Void> commit();
}

// Controller
class UserController {
    private final UnitOfWork unitOfWork;
    
    public UserController(UnitOfWork unitOfWork) {
        this.unitOfWork = unitOfWork;
    }
    
    public User getUser(int id) {
        return unitOfWork.getUsers().getById(id);
    }
}
```

### Observer + Command + Memento
**Use Case:** Undo/Redo system with notifications
**Benefits:** Flexible, extensible, maintains history

```java
import java.util.*;

// Command
interface Command {
    void execute();
    void undo();
}

// Memento
class EditorMemento {
    private final String content;
    
    public EditorMemento(String content) {
        this.content = content;
    }
    
    public String getContent() { return content; }
}

// Observer
class EditorHistory {
    private List<EditorMemento> history = new ArrayList<>();
    private List<EditorMementoListener> listeners = new ArrayList<>();
    
    public void saveState(EditorMemento memento) {
        history.add(memento);
        for (EditorMementoListener listener : listeners) {
            listener.onStateChanged(memento);
        }
    }
    
    public void addListener(EditorMementoListener listener) {
        listeners.add(listener);
    }
}

interface EditorMementoListener {
    void onStateChanged(EditorMemento memento);
}
```

---

## 17. Performance Impact Analysis

### Memory Usage Comparison

| Pattern | Memory Overhead | When to Avoid |
|---------|----------------|---------------|
| Singleton | Low | When you need multiple instances |
| Factory | Low | For simple object creation |
| Observer | Medium | With many subscribers |
| Decorator | Medium | Deep decoration chains |
| Proxy | Low-Medium | For lightweight objects |
| Flyweight | Very Low | When objects aren't similar |
| Command | Medium | For simple operations |
| Strategy | Low | When algorithms are simple |

### Thread Safety Considerations

```java
// Thread-safe Singleton
class ThreadSafeSingleton {
    private static volatile ThreadSafeSingleton instance;
    
    private ThreadSafeSingleton() { }
    
    public static ThreadSafeSingleton getInstance() {
        if (instance == null) {
            synchronized (ThreadSafeSingleton.class) {
                if (instance == null) {
                    instance = new ThreadSafeSingleton();
                }
            }
        }
        return instance;
    }
}

// Thread-safe Observer
import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;

class ThreadSafeSubject {
    private final List<Observer> observers = new CopyOnWriteArrayList<>();
    
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}
```

---

## 18. Common Implementation Mistakes

### Singleton Pitfalls
**Mistake:** Not thread-safe, hard to test
```java
// BAD
class BadSingleton {
    private static BadSingleton instance;
    
    public static BadSingleton getInstance() {
        if (instance == null) {
            instance = new BadSingleton(); // Race condition!
        }
        return instance;
    }
}

// GOOD
class GoodSingleton {
    private static volatile GoodSingleton instance;
    
    public static GoodSingleton getInstance() {
        if (instance == null) {
            synchronized (GoodSingleton.class) {
                if (instance == null) {
                    instance = new GoodSingleton();
                }
            }
        }
        return instance;
    }
    
    private GoodSingleton() { }
}
```

### Observer Memory Leaks
**Mistake:** Forgetting to unsubscribe
```java
// BAD: Memory leak
class BadSubscriber {
    public BadSubscriber(Subject subject) {
        subject.attach(this); // Never unsubscribes!
    }
}

// GOOD: Proper cleanup
class GoodSubscriber implements AutoCloseable {
    private final Subject subject;
    
    public GoodSubscriber(Subject subject) {
        this.subject = subject;
        subject.attach(this);
    }
    
    public void close() {
        subject.detach(this);
    }
}
```

### Factory Overuse
**Mistake:** Creating unnecessary abstractions
```java
// BAD: Over-engineered
interface StringFactory {
    String createString(String value);
}

// GOOD: Simple and direct
class StringHelper {
    public static String createString(String value) {
        return value;
    }
}
```

---

## 19. Refactoring Guide

### Code Smells That Indicate Pattern Need

#### **Creation Smells**

##### **Long Parameter Lists**
**Problem:** Methods with too many parameters are hard to use and maintain.
**Symptoms:** 5+ parameters, optional parameters, complex constructors.
**Solution:** Builder pattern for complex object creation.

```java
// BAD: Long parameter list
class Person {
    public Person(String firstName, String lastName, int age, String email, 
                  String phone, String address, String city, String country, 
                  boolean isVip, java.util.Date birthDate, String occupation) {
        // Constructor with 11 parameters!
    }
}

// GOOD: Builder pattern
class PersonBuilder {
    private Person person = new Person();
    
    public PersonBuilder setName(String firstName, String lastName) {
        person.setFirstName(firstName);
        person.setLastName(lastName);
        return this;
    }
    
    public PersonBuilder setContact(String email, String phone) {
        person.setEmail(email);
        person.setPhone(phone);
        return this;
    }
    
    public PersonBuilder setAddress(String address, String city, String country) {
        person.setAddress(address);
        person.setCity(city);
        person.setCountry(country);
        return this;
    }
    
    public PersonBuilder setVip(boolean isVip) {
        person.setVip(isVip);
        return this;
    }
    
    public Person build() {
        return person;
    }
}

// Usage
Person person = new PersonBuilder()
    .setName("John", "Doe")
    .setContact("john@email.com", "123-456-7890")
    .setAddress("123 Main St", "New York", "USA")
    .setVip(true)
    .build();
```

---

## 20. Language-Specific Considerations

### Java Specific Implementations

#### **CompletableFuture Patterns**
```java
import java.util.concurrent.CompletableFuture;

// Async Factory
class AsyncFactory {
    public static <T> CompletableFuture<T> createAsync(
            java.util.function.Supplier<CompletableFuture<T>> factory) {
        return factory.get();
    }
}

// Async Observer
import java.util.*;
import java.util.concurrent.CompletableFuture;

class AsyncSubject {
    private final List<java.util.function.Supplier<CompletableFuture<Void>>> observers = new ArrayList<>();
    
    public void attach(java.util.function.Supplier<CompletableFuture<Void>> observer) {
        observers.add(observer);
    }
    
    public CompletableFuture<Void> notifyAsync() {
        List<CompletableFuture<Void>> futures = new ArrayList<>();
        for (var observer : observers) {
            futures.add(observer.get());
        }
        return CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]));
    }
}
```

#### **Stream API Functional Patterns**
```java
import java.util.*;
import java.util.stream.Collectors;

// Functional composition with Streams
class FunctionalExtensions {
    public static <T> List<T> filter(List<T> source, java.util.function.Predicate<T> predicate) {
        return source.stream().filter(predicate).collect(Collectors.toList());
    }
    
    public static <T, U> List<U> map(List<T> source, java.util.function.Function<T, U> mapper) {
        return source.stream().map(mapper).collect(Collectors.toList());
    }
    
    public static <T> Optional<T> reduce(List<T> source, java.util.function.BinaryOperator<T> reducer) {
        return source.stream().reduce(reducer);
    }
}
```

#### **Dependency Injection Integration**
```java
// Service registration (Spring Framework example)
// @Configuration
class ServiceConfiguration {
    // @Bean
    public Logger logger() {
        return new ConsoleLogger();
    }
    
    // @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }
    
    // @Bean
    public UserService userService(UserRepository userRepository) {
        return new UserService(userRepository);
    }
}

// Pattern with DI
class UserController {
    private final UserService userService;
    private final Logger logger;
    
    // @Autowired
    public UserController(UserService userService, Logger logger) {
        this.userService = userService;
        this.logger = logger;
    }
}
```

#### **Java 8+ Features**
```java
import java.util.*;
import java.util.function.*;
import java.util.stream.Collectors;

// Using Optional for null safety
class OptionalPattern {
    public Optional<String> findName(int id) {
        // Instead of returning null, return Optional
        return Optional.ofNullable(getNameById(id));
    }
    
    private String getNameById(int id) {
        // Implementation
        return null;
    }
}

// Using Streams for functional operations
class StreamPattern {
    public List<String> processNames(List<String> names) {
        return names.stream()
            .filter(name -> name.length() > 5)
            .map(String::toUpperCase)
            .collect(Collectors.toList());
    }
}

// Using Lambda expressions for Strategy pattern
class LambdaStrategy {
    public void process(List<Integer> numbers, Function<List<Integer>, List<Integer>> strategy) {
        List<Integer> result = strategy.apply(numbers);
        // Process result
    }
    
    // Usage:
    // process(numbers, list -> list.stream().sorted().collect(Collectors.toList()));
}
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

