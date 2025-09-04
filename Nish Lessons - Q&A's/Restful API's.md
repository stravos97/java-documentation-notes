
## RESTful APIs: Understanding from Scratch


***
tags: [java, rest, api, http, spring-boot, cheatsheet]
date: 2025-09-04
topic: RESTful API Fundamentals
***

### What is a REST API?

**REST** (Representational State Transfer) is an architectural style for building web services that allows different applications to communicate over the internet using standard HTTP protocols. A **RESTful API** follows these principles and provides a standardized way for clients (web browsers, mobile apps, other services) to interact with server resources.[^1][^2][^3]

> [!INFO] Key Concept
> REST APIs treat data as **resources** that can be manipulated using standard HTTP methods, making them predictable and easy to use.

### The Six Core Principles of REST

REST architecture is built on six fundamental constraints that ensure scalability, reliability, and maintainability:[^2][^4][^1]

#### 1. **Uniform Interface**

All resources follow consistent naming conventions and interaction patterns. This includes:

- **Resource Identification**: Each resource has a unique URI (e.g., `/users/123`)
- **Resource Manipulation**: Use standard HTTP methods for operations
- **Self-Descriptive Messages**: Responses contain enough information for processing
- **HATEOAS**: Include links to related resources[^4][^1]


#### 2. **Statelessness**

Each request must contain all necessary information - the server doesn't store client session data between requests. This improves scalability and fault tolerance.[^5][^1][^2]

#### 3. **Client-Server Separation**

Clear separation allows both sides to evolve independently as long as the interface remains consistent.[^2][^5]

#### 4. **Cacheability**

Responses should be cacheable when possible to improve performance and reduce server load.[^1][^2]

#### 5. **Layered System**

Architecture can include multiple layers (load balancers, proxies, gateways) without affecting client-server communication.[^2]

#### 6. **Code on Demand** (Optional)

Servers can send executable code to extend client functionality, though this is rarely used.[^2]

### HTTP Methods: The CRUD Operations

REST APIs use standard HTTP methods to perform operations on resources. These map to CRUD (Create, Read, Update, Delete) operations:[^6][^7][^8][^9]


| HTTP Method | CRUD Operation | Purpose | Idempotent |
| :-- | :-- | :-- | :-- |
| **GET** | Read | Retrieve resource(s) | Yes |
| **POST** | Create | Create new resource | No |
| **PUT** | Update/Replace | Update entire resource | Yes |
| **PATCH** | Update | Partial update | No |
| **DELETE** | Delete | Remove resource | Yes |

> [!TIP] Idempotency
> An **idempotent** operation produces the same result when called multiple times. GET, PUT, and DELETE are idempotent, while POST typically isn't.

#### GET - Retrieving Data

```java
@GetMapping("/users")
public List<User> getAllUsers() {
    return userService.findAll();
}

@GetMapping("/users/{id}")
public ResponseEntity<User> getUserById(@PathVariable Long id) {
    User user = userService.findById(id);
    return ResponseEntity.ok(user);
}
```


#### POST - Creating Resources

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    User savedUser = userService.save(user);
    return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
}
```


#### PUT - Updating Resources

```java
@PutMapping("/users/{id}")
public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
    User updatedUser = userService.update(id, user);
    return ResponseEntity.ok(updatedUser);
}
```


#### DELETE - Removing Resources

```java
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
    userService.delete(id);
    return ResponseEntity.noContent().build();
}
```


### HTTP Status Codes

Status codes communicate the outcome of API requests:[^10][^11][^12][^13]

#### Success Codes (2xx)

- **200 OK**: Request successful, response includes data
- **201 Created**: Resource successfully created
- **204 No Content**: Request successful, no response body needed


#### Client Error Codes (4xx)

- **400 Bad Request**: Invalid request format or data
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Valid request but insufficient permissions
- **404 Not Found**: Resource doesn't exist
- **405 Method Not Allowed**: HTTP method not supported for this resource


#### Server Error Codes (5xx)

- **500 Internal Server Error**: Unexpected server error
- **502 Bad Gateway**: Invalid response from upstream server
- **503 Service Unavailable**: Server temporarily unavailable


### Data Format: JSON

REST APIs commonly use **JSON** (JavaScript Object Notation) for data exchange because it's lightweight, human-readable, and language-independent.[^14][^15][^16][^17]

**Example JSON Response:**

```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "createdAt": "2025-01-15T10:30:00Z",
  "addresses": [
    {
      "type": "home",
      "street": "123 Main St",
      "city": "Springfield"
    }
  ]
}
```


### Authentication Methods

REST APIs use various authentication mechanisms to secure endpoints:[^18][^19][^20]

#### 1. **API Keys**

Simple token-based authentication for internal services:

```java
@GetMapping("/users")
public List<User> getUsers(@RequestHeader("X-API-Key") String apiKey) {
    // Validate API key
    return userService.findAll();
}
```


#### 2. **JWT (JSON Web Tokens)**

Self-contained tokens carrying user information:

```java
@GetMapping("/users")
public List<User> getUsers(@RequestHeader("Authorization") String token) {
    // Extract "Bearer <jwt-token>" and validate
    return userService.findAll();
}
```


#### 3. **OAuth 2.0**

Industry standard for secure authorization, especially for third-party integrations.[^19][^18]

### REST API Naming Conventions

Following consistent naming conventions improves API usability and maintainability:[^21][^22][^23]

#### Best Practices

1. **Use Nouns, Not Verbs**
    - ✅ `GET /users`
    - ❌ `GET /getUsers`
2. **Use Plural Nouns for Collections**
    - ✅ `/users`, `/products`, `/orders`
    - ❌ `/user`, `/product`, `/order`
3. **Use Lowercase with Hyphens**
    - ✅ `/user-profiles`
    - ❌ `/userProfiles` or `/user_profiles`
4. **Create Logical Hierarchies**
    - ✅ `/users/{id}/addresses/{addressId}`
    - ❌ Deeply nested URLs
5. **Use Query Parameters for Filtering**
    - ✅ `/users?status=active&page=1&limit=10`
    - ❌ `/users/active/page/1/limit/10`

### Complete Spring Boot Example

Here's a practical REST API implementation using Spring Boot:[^24][^25][^26][^27]

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserController {
    
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    // GET all users with pagination
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size) {
        List<User> users = userService.findAll(page, size);
        return ResponseEntity.ok(users);
    }
    
    // GET single user
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        Optional<User> user = userService.findById(id);
        return user.map(ResponseEntity::ok)
                  .orElse(ResponseEntity.notFound().build());
    }
    
    // POST create new user
    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
    
    // PUT update user
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, 
                                         @Valid @RequestBody User user) {
        User updatedUser = userService.update(id, user);
        return ResponseEntity.ok(updatedUser);
    }
    
    // DELETE user
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```


### REST API Best Practices

> [!TIP] Performance Tips
> - Use appropriate **caching headers** (ETag, Cache-Control)[^28][^29]
> - Implement **pagination** for large datasets
> - Use **compression** (gzip) for responses
> - Validate input using **@Valid** annotations[^28]

> [!WARNING] Security Considerations
> - Always use **HTTPS** in production
> - Implement proper **authentication and authorization**
> - Validate and sanitize all inputs
> - Use **rate limiting** to prevent abuse
> - Never expose sensitive data in error messages[^29][^28]

#### Error Handling

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        ErrorResponse error = new ErrorResponse("USER_NOT_FOUND", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException ex) {
        ErrorResponse error = new ErrorResponse("VALIDATION_ERROR", ex.getMessage());
        return ResponseEntity.badRequest().body(error);
    }
}
```


#### API Versioning

```java
// Option 1: URL versioning
@RequestMapping("/api/v1/users")

// Option 2: Header versioning
@GetMapping(value = "/users", headers = "API-Version=1")
```


### Real-World Example: E-commerce API

Here's how REST principles apply to a practical e-commerce scenario:

```java
// Products
GET    /api/v1/products              // List all products
GET    /api/v1/products/{id}         // Get specific product
POST   /api/v1/products              // Create new product
PUT    /api/v1/products/{id}         // Update product
DELETE /api/v1/products/{id}         // Delete product

// Orders
GET    /api/v1/orders                // List user's orders
POST   /api/v1/orders                // Create new order
GET    /api/v1/orders/{id}           // Get specific order
PUT    /api/v1/orders/{id}/status    // Update order status

// Nested resources
GET    /api/v1/users/{id}/orders     // Get orders for specific user
GET    /api/v1/orders/{id}/items     // Get items in specific order
```


### Quick Reference Summary

> [!NOTE] REST API Checklist
> - ✅ Use appropriate HTTP methods (GET, POST, PUT, DELETE)
> - ✅ Return meaningful HTTP status codes
> - ✅ Use JSON for data exchange
> - ✅ Follow consistent naming conventions (nouns, plural, lowercase)
> - ✅ Implement proper error handling
> - ✅ Use authentication and authorization
> - ✅ Version your APIs
> - ✅ Document your endpoints
> - ✅ Test thoroughly with tools like Postman or REST Assured

RESTful APIs provide a powerful, standardized way to build scalable web services. By following these principles and best practices, you'll create APIs that are intuitive, maintainable, and easy for other developers to consume.
<span style="display:none">[^30][^31][^32]</span>

<div style="text-align: center">⁂</div>

[^1]: https://amplication.com/blog/rest-apis-what-why-and-how

[^2]: https://www.moesif.com/blog/technical/api-development/Rest-API-Tutorial-A-Complete-Beginners-Guide/

[^3]: https://aws.amazon.com/what-is/restful-api/

[^4]: https://restfulapi.net

[^5]: https://blog.dreamfactory.com/rest-apis-an-overview-of-basic-principles

[^6]: https://www.linkedin.com/pulse/http-methods-explained-understanding-get-post-put-delete-shaaban-nqyyf

[^7]: https://apidog.com/blog/http-methods/

[^8]: https://api7.ai/learning-center/api-101/http-methods-in-apis

[^9]: https://www.reddit.com/r/learnprogramming/comments/zrzyyu/understanding_crud_rest_api_get_put_post_delete/

[^10]: https://umbraco.com/knowledge-base/http-status-codes/

[^11]: https://www.moesif.com/blog/technical/api-design/Which-HTTP-Status-Code-To-Use-For-Every-CRUD-App/

[^12]: https://restfulapi.net/http-status-codes/

[^13]: https://openapi.com/blog/status-codes-rest-api

[^14]: https://help.hcl-software.com/onedb/2.0.1/rest/rest_011.html

[^15]: https://jsonapi.org

[^16]: https://stackoverflow.blog/2022/06/02/a-beginners-guide-to-json-the-data-format-for-the-internet/

[^17]: https://jsonapi.org/examples/

[^18]: https://www.frugaltesting.com/blog/handling-authentication-in-rest-assured-oauth-jwt-and-more

[^19]: https://zuplo.com/learning-center/top-7-api-authentication-methods-compared

[^20]: https://www.knowi.com/blog/4-ways-of-rest-api-authentication-methods/

[^21]: https://blog.dreamfactory.com/best-practices-for-naming-rest-api-endpoints

[^22]: https://www.moesif.com/blog/technical/api-development/The-Ultimate-Guide-to-REST-API-Naming-Convention/

[^23]: https://www.theserverside.com/video/Top-REST-API-URL-naming-convention-standards

[^24]: https://www.geeksforgeeks.org/advance-java/easiest-way-to-create-rest-api-using-spring-boot/

[^25]: https://www.geeksforgeeks.org/springboot/spring-boot-rest-example/

[^26]: https://spring.io/guides/gs/rest-service

[^27]: https://pwskills.com/blog/spring-boot-rest-api-tutorial-best-practices-and-examples/

[^28]: https://dev.to/mohamed_el_laithy/key-best-practices-for-api-design-in-java-44le

[^29]: https://javapro.io/2025/06/04/best-practices-for-api-design-in-java/

[^30]: https://www.youtube.com/watch?v=pJ83mmqcvoQ

[^31]: https://www.lonti.com/blog/understanding-http-methods-in-rest-api-development

[^32]: https://restfulapi.net/http-methods/



