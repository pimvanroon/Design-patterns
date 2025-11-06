# Complete Golang Design Patterns Guide

A comprehensive guide combining Go-specific patterns, idiomatic Go practices, and practical Go code examples. This guide covers everything from basic Go patterns to advanced concurrent programming patterns.

## Table of Contents

### Go-Specific Patterns
- [1. Interface Patterns](#1-interface-patterns)
- [2. Error Handling Patterns](#2-error-handling-patterns)
- [3. Concurrency Patterns](#3-concurrency-patterns)
- [4. Design Patterns in Go](#4-design-patterns-in-go)
- [5. Package Patterns](#5-package-patterns)
- [6. Testing Patterns](#6-testing-patterns)
- [7. Context Patterns](#7-context-patterns)

### Advanced Patterns
- [8. Channel Patterns](#8-channel-patterns)
- [9. Goroutine Patterns](#9-goroutine-patterns)
- [10. Reflection Patterns](#10-reflection-patterns)
- [11. Dependency Injection Patterns](#11-dependency-injection-patterns)

### Decision Tools
- [Go Pattern Decision Tree](#go-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## Go Pattern Decision Tree

### When building Go applications...

```
┌─────────────────────────────────────────────────────────────┐
│                     GO CHALLENGE                              │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │  INTERFACE    │ │ ERROR     │ │ CONCURRENCY │
        │  PATTERNS     │ │ HANDLING  │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ SMALL         │ │ ERROR     │ │ GOROUTINES  │
        │ INTERFACES    │ │ VALUES    │ │ CHANNELS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ COMPOSITION   │ │ CUSTOM    │ │ CONTEXT     │
        │               │ │ ERRORS    │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Interface Patterns

### Small Interface Pattern
**Description:** Define small, focused interfaces. "Accept interfaces, return structs."

```go
package main

import (
    "fmt"
    "io"
)

// Small, focused interfaces
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// Compose interfaces
type ReadWriter interface {
    Reader
    Writer
}

// Implementation
type File struct {
    name string
}

func (f *File) Read(p []byte) (n int, err error) {
    // Implementation
    return 0, nil
}

func (f *File) Write(p []byte) (n int, err error) {
    // Implementation
    return 0, nil
}

// Function accepts interface
func CopyData(dst Writer, src Reader) error {
    buffer := make([]byte, 4096)
    _, err := src.Read(buffer)
    if err != nil {
        return err
    }
    _, err = dst.Write(buffer)
    return err
}

// Usage
func main() {
    file := &File{name: "example.txt"}
    CopyData(file, file)
}
```

### Interface Composition Pattern
**Description:** Compose interfaces from smaller interfaces.

```go
package main

type Closer interface {
    Close() error
}

type ReadCloser interface {
    Reader
    Closer
}

type WriteCloser interface {
    Writer
    Closer
}

type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

---

## 2. Error Handling Patterns

### Error Interface Pattern
**Description:** Use Go's error interface for error handling.

```go
package main

import (
    "errors"
    "fmt"
)

// Custom error type
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error on %s: %s", e.Field, e.Message)
}

// Error wrapping
func wrapError(err error, msg string) error {
    return fmt.Errorf("%s: %w", msg, err)
}

// Error checking
func processUser(user User) error {
    if user.Name == "" {
        return &ValidationError{Field: "name", Message: "name is required"}
    }
    
    if err := validateEmail(user.Email); err != nil {
        return wrapError(err, "email validation failed")
    }
    
    return nil
}

// Error handling
func main() {
    user := User{Name: "", Email: "invalid"}
    err := processUser(user)
    if err != nil {
        var validationErr *ValidationError
        if errors.As(err, &validationErr) {
            fmt.Printf("Validation error: %s\n", validationErr.Message)
        } else {
            fmt.Printf("Error: %v\n", err)
        }
    }
}
```

### Sentinel Errors Pattern
**Description:** Use exported error variables for common errors.

```go
package main

import "errors"

var (
    ErrNotFound     = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrInvalidInput = errors.New("invalid input")
)

func findUser(id int) (*User, error) {
    if id <= 0 {
        return nil, ErrInvalidInput
    }
    
    // Search logic
    user := searchUser(id)
    if user == nil {
        return nil, ErrNotFound
    }
    
    return user, nil
}

// Usage
func main() {
    user, err := findUser(0)
    if err != nil {
        if errors.Is(err, ErrInvalidInput) {
            fmt.Println("Invalid input provided")
        } else if errors.Is(err, ErrNotFound) {
            fmt.Println("User not found")
        }
    }
}
```

---

## 3. Concurrency Patterns

### Goroutine Pattern
**Description:** Use goroutines for concurrent execution.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second) // Simulate work
        results <- job * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // Start workers
    var wg sync.WaitGroup
    for w := 1; w <= 3; w++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            worker(id, jobs, results)
        }(w)
    }

    // Send jobs
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)

    // Wait for workers to finish
    go func() {
        wg.Wait()
        close(results)
    }()

    // Collect results
    for result := range results {
        fmt.Printf("Result: %d\n", result)
    }
}
```

### Channel Pattern
**Description:** Use channels for communication between goroutines.

```go
package main

import (
    "fmt"
    "time"
)

// Producer-consumer pattern
func producer(ch chan<- int) {
    for i := 0; i < 5; i++ {
        ch <- i
        time.Sleep(time.Second)
    }
    close(ch)
}

func consumer(ch <-chan int, done chan<- bool) {
    for value := range ch {
        fmt.Printf("Consumed: %d\n", value)
    }
    done <- true
}

func main() {
    ch := make(chan int)
    done := make(chan bool)

    go producer(ch)
    go consumer(ch, done)

    <-done
}

// Fan-out pattern
func fanOut(input <-chan int, outputs []chan<- int) {
    defer func() {
        for _, out := range outputs {
            close(out)
        }
    }()

    for value := range input {
        for _, out := range outputs {
            select {
            case out <- value:
            default:
            }
        }
    }
}

// Fan-in pattern
func fanIn(inputs []<-chan int, output chan<- int) {
    var wg sync.WaitGroup

    for _, input := range inputs {
        wg.Add(1)
        go func(ch <-chan int) {
            defer wg.Done()
            for value := range ch {
                output <- value
            }
        }(input)
    }

    go func() {
        wg.Wait()
        close(output)
    }()
}
```

### Select Pattern
**Description:** Use select statement for non-blocking operations.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "message from ch1"
    }()

    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "message from ch2"
    }()

    select {
    case msg1 := <-ch1:
        fmt.Println("Received:", msg1)
    case msg2 := <-ch2:
        fmt.Println("Received:", msg2)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout")
    }
}

// Default case for non-blocking
func nonBlockingSend(ch chan<- int, value int) bool {
    select {
    case ch <- value:
        return true
    default:
        return false
    }
}
```

---

## 4. Design Patterns in Go

### Singleton Pattern
**Description:** Ensure a class has only one instance.

```go
package main

import (
    "sync"
)

type Singleton struct {
    value int
}

var (
    instance *Singleton
    once     sync.Once
)

func GetInstance() *Singleton {
    once.Do(func() {
        instance = &Singleton{value: 0}
    })
    return instance
}

// Usage
func main() {
    s1 := GetInstance()
    s2 := GetInstance()
    fmt.Println(s1 == s2) // true
}
```

### Factory Pattern
**Description:** Create objects without specifying exact classes.

```go
package main

import "fmt"

type Product interface {
    Use() string
}

type ConcreteProductA struct{}

func (p *ConcreteProductA) Use() string {
    return "Product A"
}

type ConcreteProductB struct{}

func (p *ConcreteProductB) Use() string {
    return "Product B"
}

type ProductFactory struct{}

func (f *ProductFactory) CreateProduct(productType string) (Product, error) {
    switch productType {
    case "A":
        return &ConcreteProductA{}, nil
    case "B":
        return &ConcreteProductB{}, nil
    default:
        return nil, fmt.Errorf("unknown product type: %s", productType)
    }
}

// Usage
func main() {
    factory := &ProductFactory{}
    product, _ := factory.CreateProduct("A")
    fmt.Println(product.Use())
}
```

### Builder Pattern
**Description:** Construct complex objects step by step.

```go
package main

type User struct {
    ID    int
    Name  string
    Email string
    Age   int
}

type UserBuilder struct {
    user User
}

func NewUserBuilder() *UserBuilder {
    return &UserBuilder{}
}

func (b *UserBuilder) SetID(id int) *UserBuilder {
    b.user.ID = id
    return b
}

func (b *UserBuilder) SetName(name string) *UserBuilder {
    b.user.Name = name
    return b
}

func (b *UserBuilder) SetEmail(email string) *UserBuilder {
    b.user.Email = email
    return b
}

func (b *UserBuilder) SetAge(age int) *UserBuilder {
    b.user.Age = age
    return b
}

func (b *UserBuilder) Build() User {
    return b.user
}

// Usage
func main() {
    user := NewUserBuilder().
        SetID(1).
        SetName("John Doe").
        SetEmail("john@example.com").
        SetAge(30).
        Build()
}
```

---

## 5. Package Patterns

### Package Organization Pattern
**Description:** Organize code into logical packages.

```go
// cmd/api/main.go - Application entry point
package main

import "myapp/internal/api"

func main() {
    api.Start()
}

// internal/api/server.go - API layer
package api

type Server struct {
    userService user.Service
}

func Start() {
    // Start server
}

// internal/user/service.go - Service layer
package user

type Service struct {
    repo Repository
}

// internal/user/repository.go - Repository layer
package user

type Repository interface {
    FindByID(id int) (*User, error)
}

type userRepository struct {
    db *sql.DB
}
```

---

## 6. Testing Patterns

### Table-Driven Tests Pattern
**Description:** Write tests using table-driven approach.

```go
package main

import (
    "testing"
)

func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a        int
        b        int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -2, -3, -5},
        {"zero", 0, 5, 5},
        {"mixed", -2, 3, 1},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; expected %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### Test Helpers Pattern
**Description:** Create helper functions for tests.

```go
package main

import (
    "testing"
    "time"
)

func setupTestDB(t *testing.T) *sql.DB {
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("Failed to open database: %v", err)
    }
    return db
}

func teardownTestDB(db *sql.DB) {
    db.Close()
}

func TestUserRepository(t *testing.T) {
    db := setupTestDB(t)
    defer teardownTestDB(db)

    repo := NewUserRepository(db)
    // Test logic
}
```

---

## 7. Context Patterns

### Context Pattern
**Description:** Use context for cancellation and timeout.

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func longRunningTask(ctx context.Context) error {
    for {
        select {
        case <-ctx.Done():
            return ctx.Err()
        default:
            // Do work
            time.Sleep(100 * time.Millisecond)
        }
    }
}

func main() {
    // With timeout
    ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
    defer cancel()

    go func() {
        if err := longRunningTask(ctx); err != nil {
            fmt.Printf("Task failed: %v\n", err)
        }
    }()

    <-ctx.Done()
    fmt.Println("Context cancelled")
}

// With cancellation
func main() {
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    go func() {
        time.Sleep(2 * time.Second)
        cancel() // Cancel after 2 seconds
    }()

    <-ctx.Done()
    fmt.Println("Cancelled")
}
```

---

## 8. Channel Patterns

### Worker Pool Pattern
**Description:** Use worker pool for concurrent task processing.

```go
package main

import (
    "sync"
)

type WorkerPool struct {
    workers    int
    jobQueue   chan Job
    resultQueue chan Result
    wg         sync.WaitGroup
}

func NewWorkerPool(workers int, jobQueue chan Job, resultQueue chan Result) *WorkerPool {
    return &WorkerPool{
        workers:     workers,
        jobQueue:    jobQueue,
        resultQueue: resultQueue,
    }
}

func (wp *WorkerPool) Start() {
    for i := 0; i < wp.workers; i++ {
        wp.wg.Add(1)
        go wp.worker(i)
    }
}

func (wp *WorkerPool) worker(id int) {
    defer wp.wg.Done()
    for job := range wp.jobQueue {
        result := processJob(job)
        wp.resultQueue <- result
    }
}

func (wp *WorkerPool) Wait() {
    close(wp.jobQueue)
    wp.wg.Wait()
    close(wp.resultQueue)
}
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Concurrency** | Goroutines | Channels | Parallel processing |
| **Error Handling** | Error Interface | Sentinel Errors | Error propagation |
| **Interface Design** | Small Interfaces | Interface Composition | Type abstraction |
| **State Management** | Channels | Mutex | Shared state |
| **Testing** | Table-Driven Tests | Test Helpers | Unit testing |

---

## Best Practices Checklist

### Go Idioms
- [ ] Use small interfaces
- [ ] Accept interfaces, return structs
- [ ] Handle errors explicitly
- [ ] Use meaningful error messages
- [ ] Follow package naming conventions

### Concurrency
- [ ] Use goroutines for concurrent tasks
- [ ] Use channels for communication
- [ ] Avoid shared mutable state
- [ ] Use sync package for synchronization
- [ ] Use context for cancellation

### Code Organization
- [ ] Keep packages focused
- [ ] Use internal packages for private code
- [ ] Follow Go naming conventions
- [ ] Write table-driven tests
- [ ] Document exported functions

---

## Conclusion

This comprehensive guide provides Go-specific patterns for building scalable applications. Remember:

- **Keep interfaces small** - Follow the "accept interfaces, return structs" principle
- **Handle errors explicitly** - Don't ignore errors
- **Use goroutines wisely** - Leverage Go's concurrency features
- **Follow Go idioms** - Write idiomatic Go code
- **Test thoroughly** - Use table-driven tests

Good luck with your Go development! 🚀

