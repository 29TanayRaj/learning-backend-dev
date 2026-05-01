## Introduction to Routing 

Routing is a fundamental concept in web development that determines how a server responds to different client requests. It works closely with HTTP methods to define both **what action is being performed** and **where that action is applied**.

HTTP methods such as GET, POST, PUT, and DELETE describe the **intent** of a request. For example, a GET request indicates that the client wants to retrieve data, while a POST request indicates that the client wants to create new data.

Routing, on the other hand, defines the **destination** of that request. It specifies which resource the client is trying to access and helps the server decide how to handle the request.

In simple terms:
- HTTP methods describe **what you want to do**
- Routes describe **where you want to do it**

The combination of an HTTP method and a route allows the server to map the request to a specific handler that performs the required operations, such as database queries or business logic processing. 
## Understanding the Role of Routes

A route is essentially a URL path that represents a resource on the server. For example:

- `GET /api/books` → Fetch a list of books
- `POST /api/books` → Add a new book

Even though both requests use the same route (`/api/books`), the difference in HTTP methods ensures that they are handled differently. The server treats each combination of method and route as a unique mapping.

This mapping allows the server to:
1. Identify the request type (GET, POST, etc.)
2. Match the route path
3. Execute the appropriate handler

## Static Routes

### Definition

Static routes are routes that do not change. They contain fixed URL paths with no dynamic values.

### Example

GET /api/books
POST /api/books

In both cases:
- The route `/api/books` remains constant
- There are no variable parameters

### Characteristics

- Easy to understand and maintain
- Always return predictable responses
- No dynamic input is required from the URL

Static routes are ideal for endpoints where the resource does not depend on user-specific or variable input.

## Dynamic Routes (Path Parameters)

### Definition

Dynamic routes include variable parts in the URL that allow the server to handle different values dynamically. These variables are known as **path parameters** or **route parameters**.

### Example

GET /api/users/123

Here:
- `/api/users` is the static part
- `123` is a dynamic parameter representing a user ID

### How It Works

On the server side, the route might be defined as:

/api/users/:id

Step-by-step process:
1. The server receives a request (e.g., `/api/users/123`)
2. It matches the route pattern `/api/users/:id`
3. It extracts the value `123` and assigns it to `id`
4. The handler uses this value to fetch the corresponding user data

### Key Points

- Path parameters are part of the URL structure
- They provide a semantic meaning (e.g., "user with ID 123")
- They are always treated as strings

## Query Parameters

### Definition

Query parameters are key-value pairs appended to the URL after a question mark (`?`). They are commonly used to send additional data in GET requests.

### Example

GET /api/search?query=some+value

### Structure

- `?` marks the beginning of query parameters
- `key=value` format is used
- Multiple parameters can be added using `&`

Example:

/api/books?page=2&limit=20

### Purpose

Query parameters are used when:
- You need to send optional data
- You want to filter, sort, or paginate results
- You cannot use a request body (as in GET requests)

### Example: Pagination

1. Initial request:

GET /api/books

Response includes:
- Total items
- Current page
- Total pages

2. Next page request:

GET /api/books?page=2


### Key Differences from Path Parameters

| Path Parameters | Query Parameters |
|----------------|----------------|
| Part of the route | Appended after `?` |
| Used for resource identification | Used for filtering or metadata |
| Mandatory for route matching | Optional |

## Nested Routes

### Definition

Nested routes are used to represent relationships between resources by structuring them hierarchically.

### Example

GET /api/users/123/posts/456

### Breakdown

- `/api/users` → All users
- `/api/users/123` → Specific user
- `/api/users/123/posts` → All posts by that user
- `/api/users/123/posts/456` → Specific post by that user

### Step-by-Step Interpretation

1. Identify the user (`123`)
2. Navigate to that user’s posts
3. Fetch a specific post (`456`)

### Purpose

Nested routes improve clarity by:
- Representing relationships between resources
- Making APIs more readable and intuitive
- Providing a logical hierarchy

## Route Versioning and Deprecation

### Definition

Route versioning is a technique used to manage changes in API structure without breaking existing clients.

### Example

GET /api/v1/products
GET /api/v2/products

### Why Versioning is Needed

Over time, APIs evolve. Changes may include:
- Renaming fields
- Changing response formats
- Adding or removing data

### Example Difference

- Version 1 response:

{ id, name, price }

- Version 2 response:

{ id, title, price }

### Benefits

1. Allows backward compatibility
2. Gives developers time to migrate
3. Avoids breaking existing applications

### Deprecation Process

1. Introduce a new version (e.g., v2)
2. Notify developers that v1 will be deprecated
3. Allow a transition period
4. Eventually remove v1

This ensures a smooth transition without disrupting users.

## Catch-All Routes

### Definition

A catch-all route handles requests that do not match any defined routes.

### Example

/*

### Purpose

When a user requests an undefined route (e.g., `/api/v3/products`), the server:
1. Fails to match any existing route
2. Falls back to the catch-all route
3. Returns an error message (e.g., "Route not found")

### Benefits

- Prevents undefined behavior
- Provides user-friendly error messages
- Improves debugging and usability

## Summary of Routing Concepts

Routing is a combination of:
- HTTP methods (defining intent)
- URL paths (defining location)

Key types of routing include:
- Static routes for fixed paths
- Dynamic routes for variable data
- Query parameters for optional inputs
- Nested routes for hierarchical relationships
- Versioned routes for API evolution
- Catch-all routes for error handling

Understanding these concepts is essential for working with backend systems and designing scalable APIs.