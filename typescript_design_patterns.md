# Complete TypeScript Design Patterns Guide

A comprehensive guide combining TypeScript-specific patterns, type system features, and practical TypeScript code examples. This guide covers everything from basic TypeScript patterns to advanced type system patterns.

## Table of Contents

### TypeScript-Specific Patterns
- [1. Type System Patterns](#1-type-system-patterns)
- [2. Generic Patterns](#2-generic-patterns)
- [3. Advanced Type Patterns](#3-advanced-type-patterns)
- [4. Design Patterns with TypeScript](#4-design-patterns-with-typescript)
- [5. Utility Type Patterns](#5-utility-type-patterns)
- [6. Decorator Patterns](#6-decorator-patterns)
- [7. Module Patterns](#7-module-patterns)

### Advanced Topics
- [8. Type Guards & Narrowing](#8-type-guards--narrowing)
- [9. Conditional Types](#9-conditional-types)
- [10. Mapped Types](#10-mapped-types)
- [11. Template Literal Types](#11-template-literal-types)

### Decision Tools
- [TypeScript Pattern Decision Tree](#typescript-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## TypeScript Pattern Decision Tree

### When using TypeScript...

```
┌─────────────────────────────────────────────────────────────┐
│                  TYPESCRIPT CHALLENGE                        │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   TYPE SAFETY │ │ REUSABILITY│ │ TYPE         │
        │   PATTERNS    │ │ PATTERNS   │ │ MANIPULATION │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ TYPE GUARDS   │ │ GENERICS  │ │ UTILITY      │
        │ ASSERTIONS    │ │           │ │ TYPES        │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ CONDITIONAL   │ │ MAPPED     │ │ TEMPLATE    │
        │ TYPES         │ │ TYPES      │ │ LITERAL      │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Type System Patterns

### Interface Pattern
**Description:** Define contracts for object shapes.

```typescript
// Basic interface
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // Optional property
}

// Extending interfaces
interface Admin extends User {
  permissions: string[];
}

// Interface with methods
interface Repository<T> {
  findById(id: number): Promise<T | null>;
  findAll(): Promise<T[]>;
  save(entity: T): Promise<T>;
  delete(id: number): Promise<void>;
}

// Implementation
class UserRepository implements Repository<User> {
  async findById(id: number): Promise<User | null> {
    // Implementation
    return null;
  }

  async findAll(): Promise<User[]> {
    return [];
  }

  async save(entity: User): Promise<User> {
    return entity;
  }

  async delete(id: number): Promise<void> {
    // Implementation
  }
}
```

### Type Alias Pattern
**Description:** Create type aliases for complex types.

```typescript
// Type alias
type UserId = number;
type Email = string;
type UserRole = 'admin' | 'user' | 'guest';

// Complex type alias
type User = {
  id: UserId;
  email: Email;
  role: UserRole;
};

// Function type alias
type EventHandler<T> = (event: T) => void;
type AsyncFunction<T, R> = (param: T) => Promise<R>;

// Usage
const handleClick: EventHandler<MouseEvent> = (event) => {
  console.log('Clicked', event);
};

const fetchUser: AsyncFunction<number, User> = async (id) => {
  // Implementation
  return { id, email: 'test@example.com', role: 'user' };
};
```

### Union Types Pattern
**Description:** Represent values that can be one of several types.

```typescript
type Status = 'pending' | 'completed' | 'failed';
type ID = string | number;

function processId(id: ID): string {
  if (typeof id === 'string') {
    return id;
  }
  return id.toString();
}

// Discriminated unions
type Result<T> = 
  | { success: true; data: T }
  | { success: false; error: string };

function handleResult<T>(result: Result<T>): void {
  if (result.success) {
    console.log('Data:', result.data);
  } else {
    console.error('Error:', result.error);
  }
}
```

### Intersection Types Pattern
**Description:** Combine multiple types into one.

```typescript
type Serializable = {
  serialize(): string;
};

type Deserializable = {
  deserialize(data: string): void;
};

type SerializableEntity = Serializable & Deserializable;

class User implements SerializableEntity {
  name: string = '';

  serialize(): string {
    return JSON.stringify({ name: this.name });
  }

  deserialize(data: string): void {
    const parsed = JSON.parse(data);
    this.name = parsed.name;
  }
}
```

---

## 2. Generic Patterns

### Generic Functions Pattern
**Description:** Create reusable functions that work with multiple types.

```typescript
// Basic generic function
function identity<T>(value: T): T {
  return value;
}

// Generic with constraints
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Usage
const user = { id: 1, name: 'John', email: 'john@example.com' };
const name = getProperty(user, 'name'); // Type: string
const id = getProperty(user, 'id'); // Type: number

// Generic array operations
function map<T, U>(array: T[], fn: (item: T) => U): U[] {
  return array.map(fn);
}

function filter<T>(array: T[], predicate: (item: T) => boolean): T[] {
  return array.filter(predicate);
}

// Usage
const numbers = [1, 2, 3, 4, 5];
const doubled = map(numbers, n => n * 2); // number[]
const evens = filter(numbers, n => n % 2 === 0); // number[]
```

### Generic Classes Pattern
**Description:** Create classes that work with multiple types.

```typescript
class Container<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  get(index: number): T | undefined {
    return this.items[index];
  }

  getAll(): T[] {
    return [...this.items];
  }

  remove(item: T): void {
    const index = this.items.indexOf(item);
    if (index > -1) {
      this.items.splice(index, 1);
    }
  }
}

// Usage
const stringContainer = new Container<string>();
stringContainer.add('hello');
stringContainer.add('world');

const numberContainer = new Container<number>();
numberContainer.add(1);
numberContainer.add(2);
```

### Generic Constraints Pattern
**Description:** Constrain generic types to specific shapes.

```typescript
// Constraint with interface
interface HasId {
  id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}

// Constraint with keyof
function updateProperty<T, K extends keyof T>(
  obj: T,
  key: K,
  value: T[K]
): T {
  return { ...obj, [key]: value };
}

// Constraint with multiple types
interface Comparable<T> {
  compareTo(other: T): number;
}

function findMax<T extends Comparable<T>>(items: T[]): T | undefined {
  if (items.length === 0) return undefined;
  
  return items.reduce((max, item) => 
    item.compareTo(max) > 0 ? item : max
  );
}
```

---

## 3. Advanced Type Patterns

### Conditional Types Pattern
**Description:** Types that depend on conditions.

```typescript
// Basic conditional type
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false

// Extract array element type
type ArrayElementType<T> = T extends (infer U)[] ? U : never;

type Element = ArrayElementType<string[]>; // string
type NumberElement = ArrayElementType<number[]>; // number

// Non-nullable type
type NonNullable<T> = T extends null | undefined ? never : T;

type NonNullString = NonNullable<string | null>; // string
type NonNullNumber = NonNullable<number | undefined>; // number

// Function return type extractor
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

type StringReturn = ReturnType<() => string>; // string
type NumberReturn = ReturnType<(x: number) => number>; // number
```

### Mapped Types Pattern
**Description:** Create new types by transforming properties of existing types.

```typescript
// Make all properties optional
type Partial<T> = {
  [P in keyof T]?: T[P];
};

// Make all properties required
type Required<T> = {
  [P in keyof T]-?: T[P];
};

// Make all properties readonly
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// Pick specific properties
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};

// Omit specific properties
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;

// Usage
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

type PartialUser = Partial<User>; // All properties optional
type UserName = Pick<User, 'name' | 'email'>; // Only name and email
type UserWithoutId = Omit<User, 'id'>; // All properties except id
```

### Template Literal Types Pattern
**Description:** Create types from string literal templates.

```typescript
type EventName = 'click' | 'hover' | 'focus';
type EventHandlerName = `on${Capitalize<EventName>}`;
// Result: 'onClick' | 'onHover' | 'onFocus'

// API endpoint builder
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type ApiEndpoint<T extends string> = `/api/${T}`;

type UserEndpoint = ApiEndpoint<'users'>; // '/api/users'
type ProductEndpoint = ApiEndpoint<'products'>; // '/api/products'

// CSS property builder
type CssProperty = 'margin' | 'padding' | 'border';
type CssSide = 'Top' | 'Right' | 'Bottom' | 'Left';
type CssPropertyName = `${CssProperty}${CssSide}`;
// Result: 'marginTop' | 'marginRight' | 'paddingTop' | etc.
```

---

## 4. Design Patterns with TypeScript

### Singleton Pattern
**Description:** Ensure a class has only one instance.

```typescript
class Singleton {
  private static instance: Singleton;

  private constructor() {}

  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }
}

// Usage
const instance1 = Singleton.getInstance();
const instance2 = Singleton.getInstance();
console.log(instance1 === instance2); // true
```

### Factory Pattern
**Description:** Create objects without specifying exact classes.

```typescript
interface Product {
  name: string;
  price: number;
}

class ConcreteProductA implements Product {
  name = 'Product A';
  price = 100;
}

class ConcreteProductB implements Product {
  name = 'Product B';
  price = 200;
}

class ProductFactory {
  static createProduct(type: 'A' | 'B'): Product {
    switch (type) {
      case 'A':
        return new ConcreteProductA();
      case 'B':
        return new ConcreteProductB();
      default:
        throw new Error('Unknown product type');
    }
  }
}

// Usage
const productA = ProductFactory.createProduct('A');
const productB = ProductFactory.createProduct('B');
```

### Builder Pattern
**Description:** Construct complex objects step by step.

```typescript
class UserBuilder {
  private user: Partial<User> = {};

  setId(id: number): this {
    this.user.id = id;
    return this;
  }

  setName(name: string): this {
    this.user.name = name;
    return this;
  }

  setEmail(email: string): this {
    this.user.email = email;
    return this;
  }

  build(): User {
    if (!this.user.id || !this.user.name || !this.user.email) {
      throw new Error('Missing required fields');
    }
    return this.user as User;
  }
}

// Usage
const user = new UserBuilder()
  .setId(1)
  .setName('John Doe')
  .setEmail('john@example.com')
  .build();
```

### Strategy Pattern
**Description:** Encapsulate algorithms and make them interchangeable.

```typescript
interface PaymentStrategy {
  pay(amount: number): void;
}

class CreditCardStrategy implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid ${amount} using Credit Card`);
  }
}

class PayPalStrategy implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid ${amount} using PayPal`);
  }
}

class PaymentProcessor {
  private strategy: PaymentStrategy;

  constructor(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy: PaymentStrategy): void {
    this.strategy = strategy;
  }

  processPayment(amount: number): void {
    this.strategy.pay(amount);
  }
}

// Usage
const processor = new PaymentProcessor(new CreditCardStrategy());
processor.processPayment(100);
processor.setStrategy(new PayPalStrategy());
processor.processPayment(200);
```

---

## 5. Utility Type Patterns

### Built-in Utility Types
**Description:** Use TypeScript's built-in utility types.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age?: number;
}

// Partial - makes all properties optional
type PartialUser = Partial<User>;

// Required - makes all properties required
type RequiredUser = Required<User>;

// Pick - select specific properties
type UserName = Pick<User, 'name' | 'email'>;

// Omit - exclude specific properties
type UserWithoutId = Omit<User, 'id'>;

// Record - create object type from keys and value type
type UserRoles = Record<string, 'admin' | 'user' | 'guest'>;

// Readonly - make all properties readonly
type ReadonlyUser = Readonly<User>;

// Extract - extract types from union
type StringTypes = Extract<string | number | boolean, string | number>; // string | number

// Exclude - exclude types from union
type NonStringTypes = Exclude<string | number | boolean, string>; // number | boolean

// NonNullable - exclude null and undefined
type NonNullString = NonNullable<string | null | undefined>; // string

// ReturnType - extract return type of function
type StringReturn = ReturnType<() => string>; // string

// Parameters - extract parameter types
type StringParam = Parameters<(x: string) => void>; // [string]
```

### Custom Utility Types
**Description:** Create custom utility types for common patterns.

```typescript
// Deep partial
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Deep readonly
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};

// Nullable
type Nullable<T> = T | null;

// Optional
type Optional<T> = T | undefined;

// Function type extractor
type FunctionType<T> = T extends (...args: any[]) => infer R ? R : never;

// Array element type
type ArrayElement<T> = T extends (infer U)[] ? U : never;

// Promise unwrap
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;
```

---

## 6. Decorator Patterns

### Method Decorators Pattern
**Description:** Add behavior to methods using decorators.

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with args:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`${propertyKey} returned:`, result);
    return result;
  };

  return descriptor;
}

function measure(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    const start = performance.now();
    const result = originalMethod.apply(this, args);
    const end = performance.now();
    console.log(`${propertyKey} took ${end - start} milliseconds`);
    return result;
  };

  return descriptor;
}

class Calculator {
  @log
  @measure
  add(a: number, b: number): number {
    return a + b;
  }
}
```

---

## 7. Module Patterns

### ES Module Pattern
**Description:** Use ES modules for code organization.

```typescript
// user.ts
export interface User {
  id: number;
  name: string;
}

export class UserService {
  getUser(id: number): User {
    return { id, name: 'John' };
  }
}

// index.ts
export { User, UserService } from './user';
export { Product, ProductService } from './product';

// Usage
import { User, UserService } from './user';
import * as UserModule from './user';
```

### Namespace Pattern
**Description:** Organize code using namespaces.

```typescript
namespace Utils {
  export function formatDate(date: Date): string {
    return date.toISOString();
  }

  export function formatCurrency(amount: number): string {
    return `$${amount.toFixed(2)}`;
  }
}

// Usage
const formatted = Utils.formatDate(new Date());
const currency = Utils.formatCurrency(100);
```

---

## 8. Type Guards & Narrowing

### Type Guard Functions Pattern
**Description:** Narrow types using type guard functions.

```typescript
// Type guard function
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function isNumber(value: unknown): value is number {
  return typeof value === 'number';
}

function isUser(obj: any): obj is User {
  return obj && typeof obj.id === 'number' && typeof obj.name === 'string';
}

// Usage
function processValue(value: string | number) {
  if (isString(value)) {
    // TypeScript knows value is string here
    console.log(value.toUpperCase());
  } else {
    // TypeScript knows value is number here
    console.log(value.toFixed(2));
  }
}

// Discriminated union type guard
type Result<T> = 
  | { success: true; data: T }
  | { success: false; error: string };

function isSuccess<T>(result: Result<T>): result is { success: true; data: T } {
  return result.success === true;
}

function handleResult<T>(result: Result<T>): void {
  if (isSuccess(result)) {
    console.log('Data:', result.data); // TypeScript knows data exists
  } else {
    console.error('Error:', result.error); // TypeScript knows error exists
  }
}
```

### Assertion Functions Pattern
**Description:** Assert types using assertion functions.

```typescript
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== 'string') {
    throw new Error('Value is not a string');
  }
}

function assertIsNumber(value: unknown): asserts value is number {
  if (typeof value !== 'number') {
    throw new Error('Value is not a number');
  }
}

// Usage
function process(value: unknown) {
  assertIsString(value);
  // TypeScript knows value is string here
  console.log(value.toUpperCase());
}
```

---

## 9. Conditional Types

### Advanced Conditional Types Pattern
**Description:** Create complex conditional type logic.

```typescript
// Extract function parameters
type Parameters<T extends (...args: any) => any> = 
  T extends (...args: infer P) => any ? P : never;

// Extract function return type
type ReturnType<T extends (...args: any) => any> = 
  T extends (...args: any) => infer R ? R : any;

// Flatten array type
type Flatten<T> = T extends (infer U)[] ? U : T;

// Extract promise type
type Awaited<T> = T extends Promise<infer U> ? U : T;

// Check if type is never
type IsNever<T> = [T] extends [never] ? true : false;

// Check if type is any
type IsAny<T> = 0 extends 1 & T ? true : false;
```

---

## 10. Mapped Types

### Advanced Mapped Types Pattern
**Description:** Create complex mapped type transformations.

```typescript
// Make all properties nullable
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

// Make all properties functions
type Functions<T> = {
  [P in keyof T]: () => T[P];
};

// Add prefix to property names
type Prefixed<T, Prefix extends string> = {
  [P in keyof T as `${Prefix}${string & P}`]: T[P];
};

// Remove readonly modifier
type Writable<T> = {
  -readonly [P in keyof T]: T[P];
};

// Usage
interface User {
  readonly id: number;
  readonly name: string;
}

type WritableUser = Writable<User>; // id and name are no longer readonly
```

---

## 11. Template Literal Types

### Advanced Template Literal Types Pattern
**Description:** Create sophisticated string literal types.

```typescript
// API route builder
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type Resource = 'users' | 'products' | 'orders';

type ApiRoute<M extends HttpMethod, R extends Resource> = 
  `/${M.toLowerCase()}/${R}`;

type GetUsersRoute = ApiRoute<'GET', 'users'>; // '/get/users'

// CSS property builder
type CssProperty = 'margin' | 'padding';
type CssDirection = 'Top' | 'Right' | 'Bottom' | 'Left';

type CssPropertyName = `${CssProperty}${CssDirection}`;
// Result: 'marginTop' | 'marginRight' | 'paddingTop' | etc.

// Event name builder
type EventName = 'click' | 'hover' | 'focus';
type EventHandlerName = `on${Capitalize<EventName>}`;
// Result: 'onClick' | 'onHover' | 'onFocus'
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Type Safety** | Interface | Type Alias | Object shapes |
| **Reusability** | Generics | Utility Types | Generic functions/classes |
| **Type Manipulation** | Mapped Types | Conditional Types | Type transformations |
| **Type Narrowing** | Type Guards | Assertion Functions | Runtime type checking |
| **String Types** | Template Literal Types | Union Types | String literal combinations |

---

## Best Practices Checklist

### Type Safety
- [ ] Use interfaces for object shapes
- [ ] Use type aliases for unions and complex types
- [ ] Avoid using `any` type
- [ ] Use `unknown` instead of `any` for type-safe code
- [ ] Enable strict mode in tsconfig.json

### Generics
- [ ] Use generics for reusable code
- [ ] Apply constraints when needed
- [ ] Use descriptive generic parameter names
- [ ] Prefer type inference when possible

### Type System
- [ ] Use utility types for common transformations
- [ ] Create custom utility types for project-specific needs
- [ ] Use type guards for runtime type checking
- [ ] Leverage discriminated unions for type safety

---

## Conclusion

This comprehensive guide provides TypeScript-specific patterns for building type-safe applications. Remember:

- **Leverage the type system** - Use TypeScript's powerful type system to catch errors at compile time
- **Use generics** - Create reusable, type-safe code
- **Apply utility types** - Use built-in and custom utility types for type transformations
- **Write type guards** - Ensure runtime type safety
- **Follow TypeScript best practices** - Enable strict mode and avoid `any`

Good luck with your TypeScript development! 🚀

