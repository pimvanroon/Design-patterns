# Complete ASP.NET Core Design Patterns Guide

A comprehensive guide combining ASP.NET Core-specific patterns, architectural decisions, and practical C# code examples. This guide covers everything from basic ASP.NET Core patterns to advanced enterprise-level architectures.

## Table of Contents

### ASP.NET Core-Specific Patterns
- [1. Controller Patterns](#1-controller-patterns)
- [2. Middleware Patterns](#2-middleware-patterns)
- [3. Dependency Injection Patterns](#3-dependency-injection-patterns)
- [4. Configuration Patterns](#4-configuration-patterns)
- [5. Routing Patterns](#5-routing-patterns)
- [6. API Patterns](#6-api-patterns)
- [7. Authentication & Authorization Patterns](#7-authentication--authorization-patterns)
- [8. Testing Patterns](#8-testing-patterns)

### Advanced Patterns
- [9. Performance Patterns](#9-performance-patterns)
- [10. Caching Patterns](#10-caching-patterns)
- [11. Background Service Patterns](#11-background-service-patterns)
- [12. Logging Patterns](#12-logging-patterns)

### Decision Tools
- [ASP.NET Core Pattern Decision Tree](#aspnet-core-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## ASP.NET Core Pattern Decision Tree

### When building ASP.NET Core applications...

```
┌─────────────────────────────────────────────────────────────┐
│                ASP.NET CORE CHALLENGE                        │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   CONTROLLER  │ │ MIDDLEWARE │ │ DEPENDENCY  │
        │   PATTERNS    │ │ PATTERNS   │ │ INJECTION   │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ API           │ │ PIPELINE   │ │ BUILT-IN    │
        │ CONTROLLER    │ │ MIDDLEWARE │ │ DI CONTAINER│
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ MINIMAL API   │ │ CUSTOM     │ │ THIRD-PARTY │
        │               │ │ MIDDLEWARE │ │ CONTAINERS  │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Controller Patterns

### API Controller Pattern
**Description:** Create RESTful API controllers.

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly IUserService _userService;
    private readonly ILogger<UsersController> _logger;

    public UsersController(IUserService userService, ILogger<UsersController> logger)
    {
        _userService = userService;
        _logger = logger;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<UserDto>>> GetUsers()
    {
        var users = await _userService.GetAllUsersAsync();
        return Ok(users);
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<UserDto>> GetUser(int id)
    {
        var user = await _userService.GetUserByIdAsync(id);
        if (user == null)
        {
            return NotFound();
        }
        return Ok(user);
    }

    [HttpPost]
    public async Task<ActionResult<UserDto>> CreateUser(CreateUserDto createUserDto)
    {
        var user = await _userService.CreateUserAsync(createUserDto);
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> UpdateUser(int id, UpdateUserDto updateUserDto)
    {
        await _userService.UpdateUserAsync(id, updateUserDto);
        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteUser(int id)
    {
        await _userService.DeleteUserAsync(id);
        return NoContent();
    }
}
```

### Base Controller Pattern
**Description:** Create base controller for common functionality.

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
public abstract class BaseController : ControllerBase
{
    protected IActionResult HandleResult<T>(Result<T> result)
    {
        if (result.IsSuccess)
        {
            return Ok(result.Value);
        }

        return result.Error switch
        {
            NotFoundError => NotFound(result.Error.Message),
            ValidationError => BadRequest(result.Error.Message),
            _ => StatusCode(500, result.Error.Message)
        };
    }

    protected IActionResult HandleResult(Result result)
    {
        if (result.IsSuccess)
        {
            return NoContent();
        }

        return result.Error switch
        {
            NotFoundError => NotFound(result.Error.Message),
            ValidationError => BadRequest(result.Error.Message),
            _ => StatusCode(500, result.Error.Message)
        };
    }
}

// Usage
[Route("api/[controller]")]
public class UsersController : BaseController
{
    private readonly IUserService _userService;

    public UsersController(IUserService userService)
    {
        _userService = userService;
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetUser(int id)
    {
        var result = await _userService.GetUserByIdAsync(id);
        return HandleResult(result);
    }
}
```

---

## 2. Middleware Patterns

### Custom Middleware Pattern
**Description:** Create custom middleware for cross-cutting concerns.

```csharp
using Microsoft.AspNetCore.Http;

public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<RequestLoggingMiddleware> _logger;

    public RequestLoggingMiddleware(RequestDelegate next, ILogger<RequestLoggingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        _logger.LogInformation("Request: {Method} {Path}", context.Request.Method, context.Request.Path);

        await _next(context);

        _logger.LogInformation("Response: {StatusCode}", context.Response.StatusCode);
    }
}

// Extension method for registration
public static class RequestLoggingMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestLogging(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<RequestLoggingMiddleware>();
    }
}

// Usage in Program.cs or Startup.cs
app.UseRequestLogging();
```

### Exception Handling Middleware Pattern
**Description:** Global exception handling middleware.

```csharp
public class ExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ExceptionHandlingMiddleware> _logger;

    public ExceptionHandlingMiddleware(RequestDelegate next, ILogger<ExceptionHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred");
            await HandleExceptionAsync(context, ex);
        }
    }

    private static Task HandleExceptionAsync(HttpContext context, Exception exception)
    {
        context.Response.ContentType = "application/json";
        context.Response.StatusCode = exception switch
        {
            NotFoundException => StatusCodes.Status404NotFound,
            ValidationException => StatusCodes.Status400BadRequest,
            UnauthorizedException => StatusCodes.Status401Unauthorized,
            _ => StatusCodes.Status500InternalServerError
        };

        var response = new
        {
            error = exception.Message,
            statusCode = context.Response.StatusCode
        };

        return context.Response.WriteAsJsonAsync(response);
    }
}
```

---

## 3. Dependency Injection Patterns

### Service Registration Pattern
**Description:** Register services with dependency injection container.

```csharp
// Program.cs or Startup.cs
var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddScoped<IUserService, UserService>();
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddSingleton<ICacheService, CacheService>();
builder.Services.AddTransient<IEmailService, EmailService>();

// Register with options
builder.Services.Configure<DatabaseOptions>(builder.Configuration.GetSection("Database"));

// Register with factory
builder.Services.AddScoped<IHttpClientFactory, HttpClientFactory>();
builder.Services.AddHttpClient<IUserService, UserService>();

var app = builder.Build();
```

### Service Lifetime Patterns
**Description:** Choose appropriate service lifetime.

```csharp
// Singleton - One instance for entire application lifetime
builder.Services.AddSingleton<ICacheService, CacheService>();

// Scoped - One instance per HTTP request
builder.Services.AddScoped<IUserRepository, UserRepository>();

// Transient - New instance every time
builder.Services.AddTransient<IEmailService, EmailService>();

// Scoped service example
public class UserService : IUserService
{
    private readonly IUserRepository _repository;
    private readonly ILogger<UserService> _logger;

    public UserService(IUserRepository repository, ILogger<UserService> logger)
    {
        _repository = repository;
        _logger = logger;
    }

    public async Task<UserDto> GetUserByIdAsync(int id)
    {
        _logger.LogInformation("Getting user {UserId}", id);
        return await _repository.GetByIdAsync(id);
    }
}
```

---

## 4. Configuration Patterns

### Options Pattern
**Description:** Use options pattern for configuration.

```csharp
// appsettings.json
{
  "Database": {
    "ConnectionString": "Server=localhost;Database=MyDB;...",
    "Timeout": 30
  }
}

// Options class
public class DatabaseOptions
{
    public string ConnectionString { get; set; }
    public int Timeout { get; set; }
}

// Register options
builder.Services.Configure<DatabaseOptions>(
    builder.Configuration.GetSection("Database"));

// Inject options
public class DatabaseService
{
    private readonly DatabaseOptions _options;

    public DatabaseService(IOptions<DatabaseOptions> options)
    {
        _options = options.Value;
    }
}

// Or use IOptionsSnapshot for scoped access
public class DatabaseService
{
    private readonly DatabaseOptions _options;

    public DatabaseService(IOptionsSnapshot<DatabaseOptions> options)
    {
        _options = options.Value;
    }
}
```

### Configuration Provider Pattern
**Description:** Use multiple configuration sources.

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Configuration
    .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
    .AddJsonFile($"appsettings.{builder.Environment.EnvironmentName}.json", optional: true)
    .AddEnvironmentVariables()
    .AddCommandLine(args)
    .AddUserSecrets<Program>()
    .AddAzureKeyVault(/* configuration */);
```

---

## 5. Routing Patterns

### Attribute Routing Pattern
**Description:** Use attribute routing for API endpoints.

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetProducts()
    {
        // Implementation
    }

    [HttpGet("{id:int}")]
    public async Task<ActionResult<ProductDto>> GetProduct(int id)
    {
        // Implementation
    }

    [HttpPost]
    [Route("create")]
    public async Task<ActionResult<ProductDto>> CreateProduct(CreateProductDto dto)
    {
        // Implementation
    }

    [HttpPut("{id:int}")]
    public async Task<IActionResult> UpdateProduct(int id, UpdateProductDto dto)
    {
        // Implementation
    }
}
```

### Minimal API Pattern
**Description:** Use minimal APIs for simple endpoints.

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/users", async (IUserService userService) =>
{
    var users = await userService.GetAllUsersAsync();
    return Results.Ok(users);
});

app.MapGet("/users/{id:int}", async (int id, IUserService userService) =>
{
    var user = await userService.GetUserByIdAsync(id);
    return user != null ? Results.Ok(user) : Results.NotFound();
});

app.MapPost("/users", async (CreateUserDto dto, IUserService userService) =>
{
    var user = await userService.CreateUserAsync(dto);
    return Results.Created($"/users/{user.Id}", user);
});

app.Run();
```

---

## 6. API Patterns

### Result Pattern
**Description:** Use Result pattern for error handling.

```csharp
public class Result<T>
{
    public bool IsSuccess { get; private set; }
    public T Value { get; private set; }
    public Error Error { get; private set; }

    private Result(bool isSuccess, T value, Error error)
    {
        IsSuccess = isSuccess;
        Value = value;
        Error = error;
    }

    public static Result<T> Success(T value) => new(true, value, null);
    public static Result<T> Failure(Error error) => new(false, default, error);
}

public abstract class Error
{
    public string Message { get; protected set; }
}

public class NotFoundError : Error
{
    public NotFoundError(string message) => Message = message;
}

public class ValidationError : Error
{
    public ValidationError(string message) => Message = message;
}

// Usage
public async Task<Result<UserDto>> GetUserByIdAsync(int id)
{
    var user = await _repository.GetByIdAsync(id);
    if (user == null)
    {
        return Result<UserDto>.Failure(new NotFoundError($"User {id} not found"));
    }
    return Result<UserDto>.Success(user);
}
```

### DTO Pattern
**Description:** Use Data Transfer Objects for API contracts.

```csharp
public class UserDto
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class CreateUserDto
{
    [Required]
    [StringLength(100)]
    public string Name { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }
}

public class UpdateUserDto
{
    [StringLength(100)]
    public string Name { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}

// Mapping
public class UserMappingProfile : Profile
{
    public UserMappingProfile()
    {
        CreateMap<User, UserDto>();
        CreateMap<CreateUserDto, User>();
        CreateMap<UpdateUserDto, User>();
    }
}
```

---

## 7. Authentication & Authorization Patterns

### JWT Authentication Pattern
**Description:** Implement JWT authentication.

```csharp
// Program.cs
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidAudience = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]))
        };
    });

builder.Services.AddAuthorization();

// Controller
[ApiController]
[Route("api/[controller]")]
[Authorize]
public class UsersController : ControllerBase
{
    [HttpGet]
    [Authorize(Roles = "Admin")]
    public async Task<ActionResult<IEnumerable<UserDto>>> GetUsers()
    {
        // Implementation
    }
}

// Login endpoint
[HttpPost("login")]
[AllowAnonymous]
public async Task<IActionResult> Login(LoginDto loginDto)
{
    var user = await _userService.AuthenticateAsync(loginDto);
    if (user == null)
    {
        return Unauthorized();
    }

    var token = _tokenService.GenerateToken(user);
    return Ok(new { token });
}
```

---

## 8. Testing Patterns

### Integration Testing Pattern
**Description:** Test ASP.NET Core applications.

```csharp
using Microsoft.AspNetCore.Mvc.Testing;
using Xunit;

public class UsersControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly WebApplicationFactory<Program> _factory;
    private readonly HttpClient _client;

    public UsersControllerTests(WebApplicationFactory<Program> factory)
    {
        _factory = factory;
        _client = _factory.CreateClient();
    }

    [Fact]
    public async Task GetUsers_ReturnsSuccessStatusCode()
    {
        var response = await _client.GetAsync("/api/users");
        response.EnsureSuccessStatusCode();
    }

    [Fact]
    public async Task GetUser_ReturnsUser()
    {
        var response = await _client.GetAsync("/api/users/1");
        response.EnsureSuccessStatusCode();
        
        var user = await response.Content.ReadFromJsonAsync<UserDto>();
        Assert.NotNull(user);
        Assert.Equal(1, user.Id);
    }
}
```

---

## 9. Performance Patterns

### Response Caching Pattern
**Description:** Cache API responses.

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    [ResponseCache(Duration = 60, Location = ResponseCacheLocation.Any)]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetProducts()
    {
        // Implementation
    }
}

// Or use in-memory cache
public class ProductsController : ControllerBase
{
    private readonly IMemoryCache _cache;

    public ProductsController(IMemoryCache cache)
    {
        _cache = cache;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetProducts()
    {
        if (!_cache.TryGetValue("products", out IEnumerable<ProductDto> products))
        {
            products = await _productService.GetAllProductsAsync();
            _cache.Set("products", products, TimeSpan.FromMinutes(5));
        }
        return Ok(products);
    }
}
```

### Async/Await Pattern
**Description:** Use async/await for asynchronous operations.

```csharp
public class UserService : IUserService
{
    private readonly IUserRepository _repository;

    public UserService(IUserRepository repository)
    {
        _repository = repository;
    }

    public async Task<UserDto> GetUserByIdAsync(int id)
    {
        return await _repository.GetByIdAsync(id);
    }

    public async Task<IEnumerable<UserDto>> GetAllUsersAsync()
    {
        return await _repository.GetAllAsync();
    }

    public async Task<UserDto> CreateUserAsync(CreateUserDto dto)
    {
        var user = new User
        {
            Name = dto.Name,
            Email = dto.Email
        };
        return await _repository.AddAsync(user);
    }
}
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **API Endpoints** | API Controller | Minimal API | RESTful APIs |
| **Cross-Cutting** | Middleware | Filters | Request/Response processing |
| **Dependency Management** | Built-in DI | Third-party Containers | Service registration |
| **Configuration** | Options Pattern | Configuration Providers | App settings |
| **Authentication** | JWT | Identity | User authentication |

---

## Best Practices Checklist

### ASP.NET Core
- [ ] Use dependency injection for all services
- [ ] Implement proper error handling middleware
- [ ] Use async/await for I/O operations
- [ ] Implement proper logging
- [ ] Use DTOs for API contracts
- [ ] Implement proper validation
- [ ] Use response caching where appropriate
- [ ] Follow RESTful conventions

---

## Conclusion

This comprehensive guide provides ASP.NET Core-specific patterns for building scalable web applications. Remember:

- **Use dependency injection** - Leverage built-in DI container
- **Implement middleware** - Handle cross-cutting concerns
- **Use async/await** - For all I/O operations
- **Follow RESTful conventions** - Design clean APIs
- **Handle errors properly** - Use middleware and Result pattern
- **Test thoroughly** - Write integration and unit tests

Good luck with your ASP.NET Core development! 🚀

