## Introduction to HTTP Protocol

Backend development is a vast field that includes many components and concepts. Instead of trying to cover everything, it is more practical to focus on the most commonly used concepts that appear in the majority of real world applications. One of the most fundamental concepts in backend systems is the **HTTP protocol**, which is the primary medium through which clients (such as browsers or apps) communicate with servers.

HTTP is used to send and receive data between a client and a server. Although there are other communication protocols, HTTP remains the most widely used and essential for web development.


## Core Ideas Behind HTTP

There are two key concepts that form the foundation of HTTP:

### Statelessness

HTTP is a **stateless protocol**, which means that it does not retain any memory of past interactions. Each request sent from the client to the server is treated as an independent event.

#### What Statelessness Means

- Every HTTP request must include all the necessary information required for the server to process it.
- Once the server processes the request and sends a response, it does not remember anything about that interaction.
- If another request is sent, it is treated as a completely new request.

For example, when accessing a user profile, the client must send authentication details (such as cookies or tokens) with every request. The server does not remember previous authentication.

#### Benefits of Statelessness

1. **Simplicity**  
   The server does not need to store session data, which reduces complexity in server design.

2. **Scalability**  
   Requests can be handled by any server in a distributed system since no server needs to maintain session state.

3. **Fault Tolerance**  
   If a server crashes, no session data is lost because no state is stored.

#### Managing State in a Stateless System

Even though HTTP is stateless, developers often need to maintain continuity (such as user login sessions or shopping carts). This is achieved using techniques like:

- Cookies
- Sessions
- Tokens (e.g., JWT)

These methods allow state to be maintained externally while still using a stateless protocol.

### Client-Server Model

HTTP follows a **client-server architecture**.

#### Roles in the Model

- **Client**  
  The client is typically a web browser or application. It initiates communication by sending requests.

- **Server**  
  The server hosts resources such as websites, APIs, or files. It processes incoming requests and returns responses.

#### Key Principle

Communication is always initiated by the client. The server only responds it does not initiate communication on its own.

## HTTP vs HTTPS

HTTP and HTTPS follow the same principles. HTTPS is simply a secure version of HTTP that includes:

- Encryption
- Security certificates
- TLS (Transport Layer Security)

## Underlying Communication: TCP

Before communication can happen, a connection must be established between the client and server.

- HTTP relies on **TCP (Transmission Control Protocol)** for reliable communication.
- TCP ensures that data is delivered correctly and in order.
- It uses mechanisms like the **3-way handshake** to establish connections.

Although HTTP does not strictly require TCP, it uses it because of its reliability compared to alternatives like UDP.

## OSI Model Context

In networking, communication is often described using the **OSI model**. Backend engineers primarily work at:

- **Layer 7: Application Layer**

This is where HTTP operates. Lower layers (like TCP and TLS) handle transport and security but are generally considered networking concepts.

## Evolution of HTTP

HTTP has evolved over time to improve performance and efficiency.

### HTTP 1.0

- Each request required a new connection.
- This caused inefficiency due to repeated connection setup and teardown.

### HTTP 1.1

- Introduced **persistent connections**, allowing multiple requests over a single connection.
- Improved performance significantly.
- Added features like caching and chunked transfer encoding.

### HTTP 2.0

- Introduced **multiplexing**, allowing multiple requests and responses simultaneously over one connection.
- Used **binary framing** instead of text.
- Supported **header compression**.
- Introduced **server push**, where the server can send resources before being requested.

### HTTP 3.0

- Built on **QUIC protocol** over UDP instead of TCP.
- Improved connection speed and reduced latency.
- Better handling of packet loss.
- Eliminated issues like head-of-line blocking seen in HTTP/2.

## HTTP Messages

HTTP communication consists of two types of messages:

### Request Message (Client → Server)

A request message includes:

1. **Request Method**: Specifies the action (e.g., GET, POST).

2. **Resource URL**: Indicates the resource being requested.

3. **HTTP Version**: Example: HTTP/1.1

4. **Headers**: Metadata about the request.

5. **Blank Line**: Separates headers from the body.

6. **Request Body**: Contains data sent to the server (optional).

### Response Message (Server → Client)

A response message includes:

1. **HTTP Version**

2. **Status Code**  
   Example: 200 (OK)

3. **Status Message**

4. **Headers**

5. **Blank Line**

6. **Response Body**  
   Contains the actual data returned by the server.


| REQUEST | RESPONSE |
|--------|----------|
| PUT /api/users/12345 HTTP/1.1<br>Host: example.com<br>User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)<br>AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36<br>Content-Type: application/json<br>Content-Length: 123<br>Authorization: Bearer eyJhbGciOiJIUzI1NiIsInT5cCI6IkpXVCJ9...<br>Accept: application/json<br>Accept-Encoding: gzip, deflate, br<br>Connection: keep-alive<br>Referer: https://example.com/dashboard<br>Cookie: sessionId=abc123xyz456; lang=en-US<br>// a blank line here means, everything has been sent<br>{<br>"firstName": "John",<br>"lastName": "Doe",<br>"email": "john.doe@example.com",<br>"age": 30<br>} | HTTP/1.1 200 OK<br>Date: Fri, 20 Sep 2024 12:00:00 GMT<br>Content-Type: application/json<br>Content-Length: 85<br>Server: Apache/2.4.41 (Ubuntu)<br>Cache-Control: no-store<br>X-Request-ID: abcdef123456<br>Strict-Transport-Security: max-age=31536000; includeSubDomains; preload<br>Set-Cookie: sessionId=abc123xyz456; Path=/; Secure; HttpOnly<br>Vary: Accept-Encoding<br>Connection: keep-alive<br>// a blank line here means, everything has been sent<br>{<br>"message": "User updated successfully",<br>"userId": 12345,<br>"status": "success"<br>} |


## HTTP Headers

Headers are a critical part of HTTP communication.

### What Are Headers?

Headers are **key-value pairs** that provide metadata about the request or response.

### Why Headers Are Needed

Headers act like labels on a package. Instead of placing important information inside the body (which would require opening it), metadata is placed in headers for quick access.

This allows intermediaries (like servers, proxies, or browsers) to process requests efficiently without needing to inspect the entire body.

### Types of HTTP Headers

HTTP headers can be categorized based on their purpose and usage.

#### 1. Request Headers

Request headers are sent from the client to the server. They provide information about the client and the nature of the request.

For example

- `User-Agent` header identifies the type of client making the request. This could be a web browser, a mobile application, or a tool like Postman.

- `Authorization`, which is used to send credentials such as tokens. These credentials help the server identify and authenticate the user.

- `Accept` header informs the server about the type of response the client expects. For instance, the client may request JSON, HTML, or plain text.

Overall, request headers help the server understand the client’s environment, preferences, and capabilities.

#### 2. General Headers

General headers are used in both requests and responses. They provide metadata about the message itself rather than the content.

Examples include:

- `Date`: Indicates when the message was created.
- `Cache-Control`: Defines caching behavior (e.g., `no-cache`, `max-age`).
- `Connection`: Specifies whether the connection should remain open (`keep-alive`) or be closed.

These headers help manage how messages are transmitted and stored.

#### 3. Representation Headers

Representation headers describe the body of the request or response. They help the client and server understand how to interpret the data being transferred.

Key examples include:

- `Content-Type`: Specifies the format of the data (e.g., JSON, HTML).
- `Content-Length`: Indicates the size of the content in bytes.
- `Content-Encoding`: Describes any encoding applied (e.g., gzip).
- `ETag`: A unique identifier used mainly for caching and validation.

These headers ensure that both parties correctly process the data being exchanged.

#### 4. Security Headers

Security headers are used to enhance the safety of web applications by controlling browser behavior and preventing common attacks.

Examples include:

- `Strict-Transport-Security (HSTS)`: Forces communication over HTTPS, preventing downgrade attacks.
- `Content-Security-Policy (CSP)`: Restricts sources for scripts, styles, and other resources, reducing XSS risks.
- `X-Frame-Options`: Prevents embedding in iframes, protecting against clickjacking.
- `X-Content-Type-Options`: Stops browsers from guessing MIME types, preventing MIME sniffing attacks.
- Secure cookies (`HttpOnly`, `Secure` flags): Protect cookies from JavaScript access and ensure they are sent only over HTTPS.

These headers play a critical role in securing both client and server interactions. 

## Key Concepts of HTTP Headers

### Extensibility

HTTP is highly extensible because new headers can be added without changing the core protocol.

Developers can define custom headers (e.g., `X-Custom-Header`) for specific use cases. This flexibility allows HTTP to adapt to new technologies and requirements, such as enhanced security mechanisms.

Another important use case is content negotiation. Headers like `Accept`, `Accept-Language`, and `Accept-Encoding` allow the client to specify preferences, and the server can respond accordingly.

### Headers as Remote Control

HTTP headers act like a remote control for the server. They allow the client to influence how the server processes requests.

For example:

- A client can request a specific data format using the `Accept` header.
- The server can control caching behavior using `Cache-Control`.
- Authentication is handled through the `Authorization` header.

These capabilities make HTTP communication more dynamic and customizable.

## HTTP Methods

HTTP methods define the type of action a client wants to perform on a server resource. They provide semantic meaning to requests.

### Common HTTP Methods

#### GET: The `GET` method is used to retrieve data from the server. It should not modify any data.

#### POST: The `POST` method is used to create new resources. It typically includes a request body containing the data to be created.

#### PATCH: The `PATCH` method is used to partially update a resource. It modifies only specific fields.

#### PUT: The `PUT` method is also used to update resources. However, it replaces the entire resource with the new data provided.

A general guideline is to use `PATCH` for partial updates and `PUT` only when a full replacement is required.

#### DELETE: The `DELETE` method removes a resource from the server.

## Idempotent vs Non-Idempotent Methods

### Idempotent Methods

An HTTP method is idempotent if making the same request multiple times produces the same result.

Examples include:

- `GET`: Fetching data repeatedly does not change it.
- `PUT`: Replacing a resource multiple times results in the same final state.
- `DELETE`: Deleting a resource multiple times still results in it being absent.

### Non-Idempotent Methods

A method is non-idempotent if repeated requests produce different results.

- `POST` is the primary example. Sending the same request multiple times may create multiple resources.

## OPTIONS Method and CORS

### What is CORS?

CORS (Cross-Origin Resource Sharing) is a security mechanism enforced by browsers. It controls how web applications interact with resources from different domains.

Browsers follow the Same-Origin Policy, which restricts requests to the same domain unless explicitly allowed.

### Simple Request Flow

A request is considered simple if it meets certain conditions:

- Uses methods like GET, POST, or HEAD.
- Uses standard headers.
- Uses simple content types (e.g., text/plain, form data).

#### Step-by-Step Flow

1. The client sends a request to the server.
2. The browser automatically adds an `Origin` header.
3. The server checks whether the origin is allowed.
4. If allowed, the server responds with `Access-Control-Allow-Origin`.
5. The browser verifies this header.
6. If valid, the response is passed to the client; otherwise, it is blocked.

If the required CORS headers are missing, the browser blocks the response and raises a CORS error. 

### Preflight Request Flow

A preflight request is required when:

- The method is not GET, POST, or HEAD (e.g., PUT, DELETE).
- Custom headers (e.g., Authorization) are used.
- The content type is not simple (e.g., application/json).

#### Step-by-Step Flow

1. The browser sends an `OPTIONS` request before the actual request.
2. This request includes:
   - `Origin`
   - `Access-Control-Request-Method`
   - `Access-Control-Request-Headers`

3. The server responds with:
   - `Access-Control-Allow-Origin`
   - `Access-Control-Allow-Methods`
   - `Access-Control-Allow-Headers`
   - `Access-Control-Max-Age`

4. The browser checks if the server allows the request.
5. If allowed, the browser sends the actual request.
6. The server processes it and returns the response.

The preflight request does not contain a body. It only checks server capabilities.

## Important Notes on CORS Behavior

- If the server does not include the required CORS headers, the browser blocks the response.
- The `Access-Control-Allow-Origin` header can specify a specific domain or use `*` to allow all origins.
- The `Access-Control-Max-Age` header reduces the number of preflight requests by caching permissions.



## HTTP Response Status Codes

HTTP response status codes are standardized three-digit numbers sent by a server to indicate the result of a client’s request. These codes allow the client (such as a browser or application) to quickly understand whether the request was successful, failed, or requires further action—without needing to inspect the response body.

### Why Status Codes Are Important

Before the introduction of HTTP status codes, clients had to interpret the response content manually to determine success or failure. This led to inconsistency and inefficiency. Status codes solve this problem by:

- Providing a universal and standardized way of communication between client and server.
- Allowing quick identification of request outcomes.
- Enabling automated error handling (e.g., prompting login on authentication failure).
- Ensuring consistency across different programming languages and platforms.

### Categories of Status Codes

Status codes are grouped based on their first digit:

- **1xx (Informational)**: Request received, continue processing.
- **2xx (Success)**: Request successfully processed.
- **3xx (Redirection)**: Further action needed to complete the request.
- **4xx (Client Errors)**: Problem with the client’s request.
- **5xx (Server Errors)**: Problem on the server side.

## Informational Responses (1xx)

These codes indicate that the server has received the request headers and the client may proceed.

### Common Examples

- **100 Continue**: The server confirms it is ready to receive the request body.
- **101 Switching Protocols**: The server agrees to switch protocols (e.g., HTTP to WebSocket).

These are less commonly encountered in everyday development but are useful in specific scenarios like large uploads or protocol upgrades.

## Success Responses (2xx)

These indicate that the request was successfully processed.

### Common Status Codes

#### 200 OK
This is the most common success response. It means the request was successful and the server is returning the requested data.

Example: Fetching user data with a GET request.

#### 201 Created
This indicates that a new resource has been successfully created.

Example: Submitting a form or creating a new database entry.

#### 204 No Content
The request was successful, but there is no content to return.

Example:
- Deleting a resource.
- Responding to preflight (OPTIONS) requests.

## Redirection Responses (3xx)

These codes tell the client that the requested resource is located elsewhere.

### Common Status Codes

#### 301 Moved Permanently
The resource has been permanently moved to a new URL. Future requests should use the new URL.

Example: Redirecting `/user` to `/person`.

#### 302 Found (Temporary Redirect)
The resource is temporarily available at another URL. The client should continue using the original URL in the future.

Example: Temporary campaign redirects.

#### 304 Not Modified
The resource has not changed since the last request. The client should use its cached version.

This is heavily used in caching mechanisms.

## Client Error Responses (4xx)

These errors occur due to issues in the client’s request.

### Common Status Codes

#### 400 Bad Request
The request is invalid due to incorrect data or format.

Example: Sending a string instead of a number.

#### 401 Unauthorized
The request requires authentication, but credentials are missing or invalid.

Example: Missing or expired JWT token.

#### 403 Forbidden
The client is authenticated but does not have permission to access the resource.

Example: Trying to delete another user’s data.

#### 404 Not Found
The requested resource does not exist.

Example: Incorrect URL or deleted resource.

#### 405 Method Not Allowed
The HTTP method used is not supported for the resource.

Example: Using PUT instead of POST.

#### 409 Conflict
The request conflicts with the current state of the resource.

Example: Creating a folder with a duplicate name.

#### 429 Too Many Requests
The client has exceeded the allowed request rate.

Example: Rate limiting to prevent abuse.

## Server Error Responses (5xx)

These indicate that the server encountered an issue while processing the request.

### Common Status Codes

#### 500 Internal Server Error
A generic error indicating something unexpected went wrong on the server.

#### 501 Not Implemented
The server does not support the requested functionality.

#### 502 Bad Gateway
A proxy server received an invalid response from an upstream server.

#### 503 Service Unavailable
The server is temporarily unavailable (e.g., maintenance or high load).

#### 504 Gateway Timeout
The upstream server did not respond within the expected time.

## HTTP Caching

HTTP caching is a technique used to store copies of server responses so that future requests can reuse them instead of fetching new data.

### Benefits of Caching

- Reduces server load.
- Decreases bandwidth usage.
- Improves response time and performance.

### Key Headers in Caching

#### Cache-Control
Specifies how long a response can be cached.

Example: `max-age=10` means cache for 10 seconds.

#### ETag (Entity Tag)
A unique identifier (usually a hash) representing the version of a resource.

#### Last-Modified
Indicates the last time the resource was updated.

## How Caching Works Step-by-Step

1. **Initial Request**
   - Client sends a request.
   - Server responds with data, ETag, and caching headers.
   - Client stores the response.

2. **Subsequent Request**
   - Client sends:
     - `If-None-Match` (ETag)
     - `If-Modified-Since` (timestamp)

3. **Server Decision**
   - If resource unchanged → responds with **304 Not Modified**.
   - If changed → responds with **200 OK** and new data.

4. **Client Action**
   - Uses cached data if 304.
   - Updates cache if new data is received.

## Content Negotiation

Content negotiation allows the client and server to agree on the format of the response.

### Types of Content Negotiation

#### Media Type Negotiation
The client specifies preferred format using the `Accept` header.

Example:
- JSON (`application/json`)
- XML (`application/xml`)

#### Language Negotiation
The client specifies preferred language using `Accept-Language`.

Example:
- English (`en`)
- Spanish (`es`)

#### Encoding Negotiation
The client specifies supported compression formats using `Accept-Encoding`.

Example:
- gzip
- deflate

### How It Works

1. Client sends preferences via headers.
2. Server selects the best matching format.
3. Server responds accordingly.

## HTTP Compression

Compression reduces the size of responses sent over the network.

### Why Compression Is Needed

Large responses can consume significant bandwidth. Compression minimizes data transfer size.

Example:
- Compressed response: 3.8 MB
- Uncompressed response: 26 MB

### Common Compression Types

- gzip
- deflate

### Process

1. Client indicates supported encodings.
2. Server compresses response.
3. Client decompresses data automatically.

## Persistent Connections and Keep-Alive

In earlier HTTP versions, each request required a new TCP connection, which was inefficient.

### Improvements in HTTP/1.1

- Connections are persistent by default.
- Multiple requests can reuse the same connection.

### Keep-Alive Header

This header allows control over connection reuse.

Options include:
- Timeout duration
- Maximum number of requests

### Benefits

- Reduced latency
- Lower resource usage
- Faster communication

## Handling Large Data Transfers

### Multipart Requests (Uploading Files)

Used when sending large files such as images or videos.

#### Key Concept: Boundary

- Data is split into parts.
- A boundary string separates each part.

#### Process

1. Client sends file in chunks.
2. Server reconstructs the file.
3. Server confirms upload.

### Chunked Transfer (Streaming Responses)

Used when sending large responses from server to client.

#### How It Works

1. Server sends data in small chunks.
2. Client receives and appends chunks.
3. Connection remains open until complete.

#### Important Headers

- `Content-Type: text/event-stream`
- `Connection: keep-alive`

## SSL, TLS, and HTTPS

### SSL (Secure Sockets Layer)

- Original encryption protocol.
- Now deprecated due to vulnerabilities.

### TLS (Transport Layer Security)

- Modern replacement for SSL.
- Provides secure communication through encryption.
- Uses certificates to verify server identity.

### HTTPS

HTTPS is HTTP combined with TLS encryption.

#### How It Works

1. Client connects to server using HTTPS.
2. TLS encrypts all data in transit.
3. Sensitive information (e.g., passwords) is protected.

### Key Benefits

- Prevents data interception.
- Ensures secure communication.
- Protects against tampering.