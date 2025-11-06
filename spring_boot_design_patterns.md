# Complete Spring Boot Enterprise Java Design Patterns Guide

A comprehensive guide combining Spring Boot-specific patterns, enterprise architectural decisions, and practical Java code examples. This guide covers everything from basic Spring Boot patterns to advanced enterprise-level architectures.

## Table of Contents

### Spring Boot-Specific Patterns
- [1. Dependency Injection Patterns](#1-dependency-injection-patterns)
- [2. Controller Patterns](#2-controller-patterns)
- [3. Service Layer Patterns](#3-service-layer-patterns)
- [4. Repository Patterns](#4-repository-patterns)
- [5. Configuration Patterns](#5-configuration-patterns)
- [6. Testing Patterns](#6-testing-patterns)
- [7. Security Patterns](#7-security-patterns)
- [8. Transaction Management Patterns](#8-transaction-management-patterns)

### Advanced Patterns
- [9. AOP Patterns](#9-aop-patterns)
- [10. Event-Driven Patterns](#10-event-driven-patterns)
- [11. Caching Patterns](#11-caching-patterns)
- [12. Microservices Patterns](#12-microservices-patterns)

### Decision Tools
- [Spring Boot Pattern Decision Tree](#spring-boot-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## Spring Boot Pattern Decision Tree

### When building Spring Boot applications...

```
┌─────────────────────────────────────────────────────────────┐
│            SPRING BOOT ENTERPRISE CHALLENGE                  │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ DEPENDENCY     │ │ SERVICE   │ │ DATA ACCESS │
        │ INJECTION      │ │ LAYER     │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ @COMPONENT     │ │ SERVICE   │ │ REPOSITORY  │
        │ @SERVICE       │ │ INTERFACE │ │ JPA/HIBERNATE│
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ @REPOSITORY    │ │ TRANSACTION│ │ CACHING    │
        │ SPRING DATA    │ │ MANAGEMENT │ │ REDIS      │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Dependency Injection Patterns

### Component Scanning Pattern
**Description:** Use Spring's component scanning for automatic bean registration.

```java
package com.example.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User findById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
    }
}

package com.example.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

### Constructor Injection Pattern
**Description:** Use constructor injection for required dependencies.

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final EmailService emailService;

    // Constructor injection - recommended
    public OrderService(
            OrderRepository orderRepository,
            PaymentService paymentService,
            EmailService emailService) {
        this.orderRepository = orderRepository;
        this.paymentService = paymentService;
        this.emailService = emailService;
    }

    public Order createOrder(Order order) {
        // Process payment
        paymentService.processPayment(order);
        
        // Save order
        Order savedOrder = orderRepository.save(order);
        
        // Send email
        emailService.sendOrderConfirmation(savedOrder);
        
        return savedOrder;
    }
}
```

### Bean Configuration Pattern
**Description:** Configure beans using @Configuration classes.

```java
@Configuration
public class AppConfig {
    
    @Bean
    @Primary
    public PaymentService paymentService() {
        return new PaymentServiceImpl();
    }

    @Bean
    @ConditionalOnProperty(name = "payment.provider", havingValue = "stripe")
    public PaymentService stripePaymentService() {
        return new StripePaymentService();
    }

    @Bean
    public RestTemplate restTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.setMessageConverters(Collections.singletonList(
            new MappingJackson2HttpMessageConverter()
        ));
        return restTemplate;
    }
}
```

---

## 2. Controller Patterns

### REST Controller Pattern
**Description:** Create RESTful API controllers.

```java
package com.example.controller;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public ResponseEntity<List<UserDto>> getAllUsers() {
        List<UserDto> users = userService.findAll();
        return ResponseEntity.ok(users);
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        UserDto user = userService.findById(id);
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(@RequestBody CreateUserDto dto) {
        UserDto createdUser = userService.create(dto);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserDto> updateUser(
            @PathVariable Long id,
            @RequestBody UpdateUserDto dto) {
        UserDto updatedUser = userService.update(id, dto);
        return ResponseEntity.ok(updatedUser);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

### Response Entity Pattern
**Description:** Use ResponseEntity for fine-grained HTTP response control.

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    private final OrderService orderService;

    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<OrderDto> getOrder(@PathVariable Long id) {
        try {
            OrderDto order = orderService.findById(id);
            return ResponseEntity.ok(order);
        } catch (OrderNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }

    @PostMapping
    public ResponseEntity<OrderDto> createOrder(@RequestBody CreateOrderDto dto) {
        OrderDto createdOrder = orderService.create(dto);
        return ResponseEntity
            .status(HttpStatus.CREATED)
            .header("Location", "/api/orders/" + createdOrder.getId())
            .body(createdOrder);
    }
}
```

---

## 3. Service Layer Patterns

### Service Interface Pattern
**Description:** Define service interfaces for loose coupling.

```java
package com.example.service;

public interface UserService {
    UserDto findById(Long id);
    List<UserDto> findAll();
    UserDto create(CreateUserDto dto);
    UserDto update(Long id, UpdateUserDto dto);
    void delete(Long id);
}

@Service
public class UserServiceImpl implements UserService {
    private final UserRepository userRepository;
    private final UserMapper userMapper;

    public UserServiceImpl(UserRepository userRepository, UserMapper userMapper) {
        this.userRepository = userRepository;
        this.userMapper = userMapper;
    }

    @Override
    public UserDto findById(Long id) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
        return userMapper.toDto(user);
    }

    @Override
    public List<UserDto> findAll() {
        return userRepository.findAll().stream()
            .map(userMapper::toDto)
            .collect(Collectors.toList());
    }

    @Override
    public UserDto create(CreateUserDto dto) {
        User user = userMapper.toEntity(dto);
        User savedUser = userRepository.save(user);
        return userMapper.toDto(savedUser);
    }

    @Override
    public UserDto update(Long id, UpdateUserDto dto) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
        userMapper.updateEntity(dto, user);
        User updatedUser = userRepository.save(user);
        return userMapper.toDto(updatedUser);
    }

    @Override
    public void delete(Long id) {
        if (!userRepository.existsById(id)) {
            throw new UserNotFoundException(id);
        }
        userRepository.deleteById(id);
    }
}
```

---

## 4. Repository Patterns

### Spring Data JPA Pattern
**Description:** Use Spring Data JPA for data access.

```java
package com.example.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    
    List<User> findByAgeGreaterThan(int age);
    
    @Query("SELECT u FROM User u WHERE u.status = :status")
    List<User> findByStatus(@Param("status") UserStatus status);
    
    @Query("SELECT u FROM User u WHERE u.name LIKE %:name%")
    List<User> findByNameContaining(@Param("name") String name);
    
    boolean existsByEmail(String email);
    
    long countByStatus(UserStatus status);
}
```

### Custom Repository Pattern
**Description:** Extend repositories with custom methods.

```java
public interface UserRepositoryCustom {
    List<User> findActiveUsersWithOrders();
}

@Repository
public class UserRepositoryImpl implements UserRepositoryCustom {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    public List<User> findActiveUsersWithOrders() {
        String jpql = "SELECT DISTINCT u FROM User u " +
                      "JOIN FETCH u.orders o " +
                      "WHERE u.status = :status";
        return entityManager.createQuery(jpql, User.class)
            .setParameter("status", UserStatus.ACTIVE)
            .getResultList();
    }
}

// Extend both interfaces
public interface UserRepository extends JpaRepository<User, Long>, UserRepositoryCustom {
    // Spring Data JPA methods
}
```

---

## 5. Configuration Patterns

### Application Properties Pattern
**Description:** Use application.properties or application.yml for configuration.

```yaml
# application.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: ${DB_USERNAME:admin}
    password: ${DB_PASSWORD:password}
    driver-class-name: org.postgresql.Driver
  
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect

server:
  port: 8080

app:
  name: MyApplication
  version: 1.0.0
```

### Configuration Properties Pattern
**Description:** Use @ConfigurationProperties for type-safe configuration.

```java
@ConfigurationProperties(prefix = "app")
@Data
public class AppProperties {
    private String name;
    private String version;
    private Database database = new Database();
    private Security security = new Security();

    @Data
    public static class Database {
        private String url;
        private String username;
        private String password;
    }

    @Data
    public static class Security {
        private String secretKey;
        private int tokenExpirationMinutes;
    }
}

@Configuration
@EnableConfigurationProperties(AppProperties.class)
public class AppConfig {
}
```

---

## 6. Testing Patterns

### Unit Testing Pattern
**Description:** Test Spring components in isolation.

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @Mock
    private UserMapper userMapper;
    
    @InjectMocks
    private UserServiceImpl userService;

    @Test
    void findById_WhenUserExists_ReturnsUserDto() {
        // Given
        Long id = 1L;
        User user = new User();
        user.setId(id);
        UserDto expectedDto = new UserDto();
        expectedDto.setId(id);

        when(userRepository.findById(id)).thenReturn(Optional.of(user));
        when(userMapper.toDto(user)).thenReturn(expectedDto);

        // When
        UserDto result = userService.findById(id);

        // Then
        assertThat(result).isNotNull();
        assertThat(result.getId()).isEqualTo(id);
        verify(userRepository).findById(id);
        verify(userMapper).toDto(user);
    }

    @Test
    void findById_WhenUserNotFound_ThrowsException() {
        // Given
        Long id = 999L;
        when(userRepository.findById(id)).thenReturn(Optional.empty());

        // When & Then
        assertThatThrownBy(() -> userService.findById(id))
            .isInstanceOf(UserNotFoundException.class);
    }
}
```

### Integration Testing Pattern
**Description:** Test Spring Boot applications with Spring context.

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerIntegrationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private ObjectMapper objectMapper;

    @Test
    void createUser_WhenValidRequest_ReturnsCreated() throws Exception {
        CreateUserDto dto = new CreateUserDto();
        dto.setName("John Doe");
        dto.setEmail("john@example.com");

        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(dto)))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John Doe"))
            .andExpect(jsonPath("$.email").value("john@example.com"));
    }

    @Test
    void getUser_WhenUserExists_ReturnsUser() throws Exception {
        User user = new User();
        user.setName("John Doe");
        user.setEmail("john@example.com");
        user = userRepository.save(user);

        mockMvc.perform(get("/api/users/{id}", user.getId()))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John Doe"));
    }
}
```

---

## 7. Security Patterns

### Spring Security Pattern
**Description:** Implement security with Spring Security.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .decoder(jwtDecoder())
                )
            );
        return http.build();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri("https://auth.example.com/.well-known/jwks.json")
            .build();
    }
}

// Method-level security
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    @PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        // Implementation
    }

    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        // Implementation
    }
}
```

---

## 8. Transaction Management Patterns

### Declarative Transaction Pattern
**Description:** Use @Transactional for transaction management.

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final InventoryService inventoryService;

    public OrderService(
            OrderRepository orderRepository,
            PaymentService paymentService,
            InventoryService inventoryService) {
        this.orderRepository = orderRepository;
        this.paymentService = paymentService;
        this.inventoryService = inventoryService;
    }

    @Transactional
    public Order createOrder(Order order) {
        // All operations in this method are part of one transaction
        Order savedOrder = orderRepository.save(order);
        
        paymentService.processPayment(order);
        
        inventoryService.updateInventory(order);
        
        return savedOrder;
    }

    @Transactional(readOnly = true)
    public Order findById(Long id) {
        return orderRepository.findById(id)
            .orElseThrow(() -> new OrderNotFoundException(id));
    }

    @Transactional(rollbackFor = PaymentException.class)
    public Order processOrderWithPayment(Order order) {
        // Transaction will rollback if PaymentException is thrown
        return paymentService.processPayment(order);
    }
}
```

---

## 9. AOP Patterns

### Aspect-Oriented Programming Pattern
**Description:** Use AOP for cross-cutting concerns.

```java
@Aspect
@Component
public class LoggingAspect {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        logger.info("Executing method: {}", joinPoint.getSignature().getName());
    }

    @AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        logger.info("Method {} returned: {}", joinPoint.getSignature().getName(), result);
    }

    @AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "exception")
    public void logAfterThrowing(JoinPoint joinPoint, Exception exception) {
        logger.error("Exception in method {}: {}", joinPoint.getSignature().getName(), exception.getMessage());
    }

    @Around("execution(* com.example.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        logger.info("Method {} executed in {} ms", joinPoint.getSignature().getName(), executionTime);
        return result;
    }
}
```

---

## 10. Event-Driven Patterns

### Spring Events Pattern
**Description:** Use Spring's event mechanism for loose coupling.

```java
// Event class
public class UserCreatedEvent extends ApplicationEvent {
    private final UserDto user;

    public UserCreatedEvent(Object source, UserDto user) {
        super(source);
        this.user = user;
    }

    public UserDto getUser() {
        return user;
    }
}

// Event publisher
@Service
public class UserService {
    private final ApplicationEventPublisher eventPublisher;
    private final UserRepository userRepository;

    public UserService(ApplicationEventPublisher eventPublisher, UserRepository userRepository) {
        this.eventPublisher = eventPublisher;
        this.userRepository = userRepository;
    }

    public User createUser(CreateUserDto dto) {
        User user = userRepository.save(userMapper.toEntity(dto));
        
        // Publish event
        eventPublisher.publishEvent(new UserCreatedEvent(this, userMapper.toDto(user)));
        
        return user;
    }
}

// Event listener
@Component
public class UserCreatedEventListener {
    
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        // Send welcome email
        emailService.sendWelcomeEmail(event.getUser());
    }

    @Async
    @EventListener
    public void handleUserCreatedAsync(UserCreatedEvent event) {
        // Asynchronous processing
        analyticsService.trackUserCreation(event.getUser());
    }
}
```

---

## 11. Caching Patterns

### Spring Cache Pattern
**Description:** Use Spring Cache abstraction for caching.

```java
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        RedisCacheManager.Builder builder = RedisCacheManager
            .RedisCacheManagerBuilder
            .fromConnectionFactory(redisConnectionFactory())
            .cacheDefaults(cacheConfiguration());
        return builder.build();
    }

    private RedisCacheConfiguration cacheConfiguration() {
        return RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .serializeKeysWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new GenericJackson2JsonRedisSerializer()));
    }
}

@Service
public class UserService {
    private final UserRepository userRepository;

    @Cacheable(value = "users", key = "#id")
    public UserDto findById(Long id) {
        return userMapper.toDto(userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id)));
    }

    @CacheEvict(value = "users", key = "#id")
    public void delete(Long id) {
        userRepository.deleteById(id);
    }

    @CachePut(value = "users", key = "#result.id")
    public UserDto update(Long id, UpdateUserDto dto) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
        userMapper.updateEntity(dto, user);
        return userMapper.toDto(userRepository.save(user));
    }
}
```

---

## 12. Microservices Patterns

### Feign Client Pattern
**Description:** Use Feign for service-to-service communication.

```java
@FeignClient(name = "user-service", url = "${user.service.url}")
public interface UserServiceClient {
    
    @GetMapping("/api/users/{id}")
    UserDto getUser(@PathVariable Long id);
    
    @PostMapping("/api/users")
    UserDto createUser(@RequestBody CreateUserDto dto);
}

@Service
public class OrderService {
    private final UserServiceClient userServiceClient;

    public OrderService(UserServiceClient userServiceClient) {
        this.userServiceClient = userServiceClient;
    }

    public Order createOrder(CreateOrderDto dto) {
        UserDto user = userServiceClient.getUser(dto.getUserId());
        // Create order logic
    }
}
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Dependency Injection** | Constructor Injection | @Autowired | Service dependencies |
| **Data Access** | Spring Data JPA | Custom Repository | Database operations |
| **API Layer** | REST Controller | Service Layer | RESTful APIs |
| **Transaction Management** | @Transactional | Programmatic | Database transactions |
| **Caching** | Spring Cache | Redis Cache | Performance optimization |

---

## Best Practices Checklist

### Spring Boot
- [ ] Use constructor injection for required dependencies
- [ ] Implement service interfaces for loose coupling
- [ ] Use Spring Data JPA for data access
- [ ] Implement proper exception handling
- [ ] Use @Transactional for transaction management
- [ ] Write comprehensive tests
- [ ] Use @ConfigurationProperties for configuration
- [ ] Implement proper security with Spring Security
- [ ] Use caching for performance optimization
- [ ] Follow RESTful conventions

---

## Conclusion

This comprehensive guide provides Spring Boot Enterprise Java-specific patterns for building scalable applications. Remember:

- **Use dependency injection** - Leverage Spring's DI container
- **Follow layered architecture** - Separate concerns (Controller, Service, Repository)
- **Use Spring Data JPA** - Simplify data access
- **Implement proper transactions** - Use @Transactional
- **Test thoroughly** - Write unit and integration tests
- **Follow Spring Boot best practices** - Use auto-configuration and conventions

Good luck with your Spring Boot Enterprise Java development! 🚀

