# Authentication and Authorization

Authentication and authorization are two fundamental concepts in software security. They are closely related, but they answer two different questions.

- **Authentication** answers the question: **Who are you?**
- **Authorization** answers the question: **What are you allowed to do?**

Authentication is the process of verifying the identity of a user, application, or system. When you log into a website using your email and password, the system checks whether you are really the person you claim to be.

Authorization happens after authentication. Once the system knows your identity, it determines what resources and actions you are permitted to access. For example, a normal user may be able to view their profile, while an administrator may be allowed to delete accounts and manage settings.

Together, authentication and authorization form the foundation of secure software systems.

## Historical Evolution of Authentication

Authentication has evolved significantly over time. Understanding this history helps explain why modern systems use the techniques they do today.

### Trust-Based Authentication in Early Societies

In pre-industrial societies, authentication was based on personal trust and recognition.

People usually interacted within small communities where everyone knew one another. If a trusted village elder vouched for someone, that person's identity was accepted. Agreements were often confirmed through handshakes, which symbolized mutual trust.

This form of authentication was:

- Implicit
- Human-based
- Dependent on reputation

The main limitation was that it did not scale. A person trusted in one village would not necessarily be known or trusted in another.

### Wax Seals and Physical Tokens

As societies grew and communication extended across larger regions, more formal methods were needed.

Wax seals became an early authentication mechanism. A document bearing a unique seal indicated that it came from a specific person or authority.

These seals acted as early **authentication tokens**, based on the principle of **something you have**.

However, seals could be forged. This introduced one of the earliest examples of authentication bypass attacks.

### Passphrases and Shared Secrets

During the Industrial Revolution, communication systems such as the telegraph became widely used.

Operators used pre-agreed passphrases to verify identities. This introduced the concept of **shared secrets**, where both parties knew a secret phrase.

This method is the direct ancestor of today's password systems.

It relied on the principle of **something you know**.

### Passwords in Early Computer Systems

In the 1960s, multi-user computer systems were developed.

Researchers at the created the Compatible Time-Sharing System (CTSS), one of the first systems to use passwords.

Initially, passwords were stored in plain text. This was a major security flaw. In one famous incident, the password file was printed and exposed to users.

This led to the realization that passwords should never be stored directly.

### Hashing and Secure Password Storage

To protect passwords, systems began using hashing.

Hashing transforms a password into a fixed-length string that cannot easily be reversed.

For example:

- Input: `mypassword123`
- Output: `5f4dcc3b5aa765d61d8327deb882cf99`

Properties of hashing:

1. The same input always produces the same output.
2. Small changes in input create completely different outputs.
3. The original password cannot be easily recovered.
4. Output length remains fixed.

Modern systems store password hashes rather than actual passwords.

### Asymmetric Cryptography

In the 1970s, cryptographic research advanced dramatically.

Whitfield Diffie and Martin Hellman introduced the Diffie-Hellman key exchange.

This allowed two parties to establish a shared secret over an untrusted network.

This breakthrough became the foundation for:

- Public key cryptography
- Digital signatures
- TLS/SSL
- Modern authentication protocols

### Kerberos and Ticket-Based Authentication

 introduced the idea of issuing tickets from a trusted third party.

Instead of sending passwords repeatedly, users received tickets proving their identity.

This was an early form of token-based authentication.

### Multi-Factor Authentication (MFA)

In the 1990s, attackers began using brute-force and dictionary attacks against passwords.

To improve security, systems adopted Multi-Factor Authentication.

MFA combines multiple categories of evidence:

#### Something You Know

Examples:

- Passwords
- PINs
- Passphrases

#### Something You Have

Examples:

- Smart cards
- Security tokens
- OTP generators

#### Something You Are

Examples:

- Fingerprints
- Face recognition
- Retina scans

Even if one factor is compromised, the attacker still needs the other factors.

### Modern Authentication Technologies

In the 21st century, cloud computing, APIs, and mobile applications required more scalable solutions.

Important modern technologies include:

- OAuth 2.0
- OpenID Connect
- JSON Web Tokens (JWTs)
- Zero Trust Architecture
- WebAuthn
- Passwordless authentication

### Emerging Trends

Potential future technologies include:

- Decentralized identity using blockchain
- Behavioral biometrics
- Post-quantum cryptography

Post-quantum cryptography focuses on algorithms resistant to attacks from quantum computers.

## Core Components of Modern Authentication

Three essential components appear repeatedly in authentication systems:

1. Sessions
2. JSON Web Tokens (JWTs)
3. Cookies

Understanding these components is critical.

# Sessions

## Why Sessions Were Needed

HTTP is a stateless protocol.

This means each request is treated independently. The server does not automatically remember previous requests.

For static websites, this was acceptable. But dynamic applications required continuity.

Examples:

- Shopping carts
- Logged-in users
- User preferences

Sessions were introduced to provide server-side memory.

# How Sessions Work

## Step 1: User Logs In

The authentication process begins when the user submits their credentials, such as an email and password, to the server.

At this stage, the server verifies whether the credentials are valid by comparing them with the stored user data. If the credentials are correct, the server proceeds to create a session for the user.

## Step 2: Server Creates a Session

Once the user's identity is verified, the server generates a unique session ID.

Example:

abc123xyz789

This session ID acts as a reference key that identifies the user's session. The ID itself usually contains no meaningful information. It is simply a unique identifier.

## Step 3: Session Data Is Stored

The server stores the session ID along with all the relevant information about the user.

Typical session data may include:

- User ID
- User roles and permissions
- Shopping cart contents
- Authentication status
- Preferences or temporary state

This data is stored in a persistent storage system such as a file, database, or in-memory store.

## Step 4: Session ID Is Sent to the Browser

After storing the session, the server sends the session ID to the client's browser.

This is usually done by setting a cookie using the `Set-Cookie` HTTP header.

## Step 5: Browser Sends Session ID Automatically

The browser stores the cookie and automatically includes it in every future request sent to the same server.

This means the user does not need to log in again for every page request.

## Step 6: Server Retrieves Session Data

When the server receives a request containing the session ID, it uses that ID to look up the corresponding session data.

This allows the server to remember who the user is and what state they are in.

# Session Storage Evolution

As applications became larger and more complex, session storage mechanisms evolved.

## File-Based Sessions

In early systems, session data was stored in files on the server's disk.

This approach was simple to implement, but it became inefficient as the number of users increased.

## Database-Backed Sessions

To improve scalability and persistence, applications began storing sessions in relational databases.

This allowed session data to survive server restarts and made it easier to manage large numbers of sessions.

## In-Memory Stores

Modern applications often use high-performance in-memory data stores such as:

- Redis
- Memcached

These systems store data in RAM, which makes lookups extremely fast.

They are widely used in production systems where low latency is critical.
 

# Advantages of Sessions

Sessions offer several important benefits.

## Simple to Understand

The server has full control over all session data, which makes the architecture straightforward.

## Easy to Revoke

The server can invalidate a session at any time by deleting it from storage.

## Full Server Control

Because the state is stored on the server, administrators can modify or inspect it whenever necessary.

 

# Disadvantages of Sessions

Despite their usefulness, sessions have some drawbacks.

## Requires Server-Side Storage

Each active user consumes storage resources on the server.

## Difficult to Scale

In distributed systems, multiple servers must share access to the same session store.

## Additional Infrastructure

Large applications often need tools such as Redis to manage sessions efficiently.

 

# JSON Web Tokens (JWTs)

JSON Web Tokens, commonly known as JWTs, provide a stateless method for transferring identity and authorization information.

Instead of storing session data on the server, all relevant information is embedded directly inside the token.

 

# Structure of a JWT

A JWT consists of three parts separated by dots.

header.payload.signature

Each part is Base64URL encoded.

## Header

The header contains metadata about the token.

Example:

{
  "alg": "HS256",
  "typ": "JWT"
}

In this example:

- `alg` specifies the signing algorithm.
- `typ` indicates that the token type is JWT.

## Payload

The payload contains claims, which are statements about the user.

Example:

```json
{
  "sub": "12345",
  "iat": 1710000000,
  "role": "admin",
  "name": "Alice"
}
```

### Common Claims

- `sub` — Subject, usually the user ID.
- `iat` — Issued At timestamp.
- `exp` — Expiration time.
- `iss` — Issuer.
- `aud` — Audience.

## Signature

The signature is created by combining the header and payload and signing them with a secret key or private key.

The signature ensures:

- Integrity
- Authenticity
- Tamper detection

If the token is modified, the signature validation fails.
 

# JWT Authentication Flow

## Step 1: Login

The user sends their credentials to the server.

## Step 2: Token Generation

The server verifies the credentials and creates a signed JWT.

## Step 3: Token Storage

The client stores the token, typically in an HttpOnly cookie or other secure storage.

## Step 4: Subsequent Requests

The client sends the JWT with every future request.

## Step 5: Token Verification

The server validates the token's signature and checks claims such as expiration.


# Advantages of JWTs

## Statelessness

The server does not need to store session data.

## Scalability

JWTs are well suited for microservices and distributed architectures.

## Portability

JWTs can be transmitted through:

- Authorization headers
- Cookies
- URLs (though URLs are generally discouraged)

 
# Disadvantages of JWTs

## Token Theft

Anyone who obtains the token can use it until it expires.

## Difficult Revocation

Since the server does not track tokens, invalidating them early is challenging.

## Larger Request Size

JWTs are often much larger than session IDs.


# Hybrid JWT Approach

Many systems combine JWTs with server-side state to support token revocation.

## Process

1. The server verifies the JWT signature.
2. The server checks a blacklist or revocation list.
3. If the token is listed, access is denied.

This approach improves security but partially removes the benefits of statelessness.

# When to Use JWTs

JWTs are especially useful for:

- APIs
- Mobile applications
- Microservices
- Single sign-on (SSO)

# Cookies

Cookies are small pieces of data stored in the user's browser.

They are created by the server and automatically sent back with future requests to the same domain.

# Purpose of Cookies

Cookies allow servers to persist information on the client side.

They are commonly used to store:

- Session IDs
- JWTs
- User preferences
- Tracking identifiers


# Authentication Flow Using Cookies

## Step 1: User Logs In

The user submits their credentials.

## Step 2: Server Sets a Cookie

The server sends a `Set-Cookie` header containing the authentication token or session ID.

## Step 3: Browser Stores the Cookie

The browser saves the cookie according to its attributes.

## Step 4: Browser Sends Cookie Automatically

The browser includes the cookie in every subsequent request to the same domain.

## Step 5: Server Reads the Cookie

The server extracts the cookie and authenticates the user.

# Cookie Security Attributes

## HttpOnly

Prevents JavaScript from reading the cookie, reducing the risk of token theft through XSS attacks.

## Secure

Ensures the cookie is sent only over HTTPS.

## SameSite

Controls whether cookies are included in cross-site requests.

Possible values:

- `Strict`
- `Lax`
- `None`

## Expires and Max-Age

Define how long the cookie remains valid.

# Why Cookies Are Useful

Cookies automate the transmission of authentication data.

The client does not need to manually attach tokens to each request.

# Sessions vs JWTs

## Sessions

In a session-based system:

- The server stores all state.
- The client stores only a session ID.
- Sessions are easy to revoke.
- Shared storage is required.

## JWTs

In a JWT-based system:

- The client stores the complete token.
- The server remains stateless.
- Revocation is more difficult.
- JWTs work well in distributed systems.


# Cookies vs Local Storage

## Cookies

Cookies:

- Are automatically sent with requests.
- Can be protected with `HttpOnly`.
- Are generally preferred for authentication.

## Local Storage

Local storage:

- Is accessible via JavaScript.
- Is vulnerable to XSS attacks.
- Requires manual inclusion in requests.

For security-sensitive applications, storing tokens in secure cookies is usually the better choice.

# Authentication Providers

Building authentication systems securely is difficult.

Many organizations rely on specialized identity providers to handle this complexity.

These providers typically offer:

- Secure password hashing
- Multi-factor authentication
- Social login
- Session management
- Compliance features
- Monitoring and audit logs

For production applications, using a trusted authentication provider is often safer and faster than building a custom authentication system from scratch.

## Authentication vs Authorization

### Authentication

Authentication is the process of verifying the identity of a user, application, or system.

When a user provides credentials such as an email and password, the backend checks whether those credentials match a valid account.

If the credentials are correct, the user is considered authenticated.

### Authorization

Authorization is the process of determining what an authenticated user is allowed to do.

For example:

- A regular user may only access their own notes.
- A moderator may review user content.
- An administrator may manage all users and system settings.

A user can be authenticated but still not be authorized to perform certain actions.

## Major Types of Authentication

Modern backend systems commonly use four major authentication approaches:

1. Stateful Authentication
2. Stateless Authentication
3. API Key Authentication
4. OAuth 2.0 and OpenID Connect

Each approach serves different use cases and has its own advantages and disadvantages.

# Stateful Authentication

Stateful authentication stores session information on the server.

When a user logs in, the server creates a session and saves it in a persistent storage system such as Redis or a database.

The client only stores a session identifier.

## How Stateful Authentication Works

### Step 1: User Submits Credentials

The client sends credentials such as:

- Email and password
- Username and password

### Step 2: Server Validates Credentials

The server verifies that:

- The user exists.
- The password is correct.
- The account is active.

### Step 3: Server Creates a Session

If authentication succeeds, the server generates a unique session ID.

The server stores session data such as:

- User ID
- Role
- Login time
- Expiration time

### Step 4: Session Stored in Redis or Database

The session information is stored in a centralized session store.

Redis is commonly used because it offers extremely fast reads and writes.

### Step 5: Session ID Sent to Client

The server sends the session ID to the browser in an HTTP-only cookie.

An HTTP-only cookie cannot be accessed by JavaScript, which helps reduce the risk of token theft through cross-site scripting (XSS).

### Step 6: Browser Sends Cookie Automatically

The browser automatically includes the cookie in every subsequent request.

### Step 7: Server Retrieves Session Data

The server extracts the session ID from the cookie and looks up the session in Redis or the database.

If the session is valid, the server identifies the user and processes the request.

## Why It Is Called Stateful

The server stores state about the user's session.

If the server deletes the session, the user is immediately logged out.

## Advantages of Stateful Authentication

### Centralized Session Control

All sessions are stored on the server, making them easy to manage.

### Immediate Revocation

The server can instantly invalidate a session.

### Real-Time Session Visibility

Administrators can inspect active sessions.

### Strong Security

The server retains full control over authentication state.

## Disadvantages of Stateful Authentication

### Requires Session Storage

A dedicated store such as Redis is needed.

### More Infrastructure Complexity

Session replication and synchronization may be required.

### Potential Scalability Challenges

Large distributed systems may experience latency when accessing the session store.

## Common Use Cases

Stateful authentication is ideal for:

- Traditional web applications
- SaaS platforms
- Applications requiring immediate logout or session revocation

# Stateless Authentication

Stateless authentication stores all required user information inside a token, typically a JSON Web Token (JWT).

The server does not store session data.

## JSON Web Token (JWT)

A JWT is a signed token that contains claims about the user.

Typical claims include:

- User ID
- Role
- Expiration time
- Issuer
- Issued-at time

The token is signed using a secret key so the server can verify that it has not been tampered with.

## How Stateless Authentication Works

### Step 1: User Logs In

The client sends credentials.

### Step 2: Server Validates Credentials

The server checks the credentials.

### Step 3: Server Creates a JWT

The server signs a JWT containing user information.

### Step 4: Token Sent to Client

The token is returned to the client.

### Step 5: Client Sends Token in Every Request

The token is usually included in the `Authorization` header in this format:

`Authorization: Bearer <token>`

### Step 6: Server Verifies Token

The server verifies the token signature using its secret key.

If valid, the server extracts user information from the token.

## Why It Is Called Stateless

The server does not need to store session data.

All necessary information is included in the token itself.

## Advantages of Stateless Authentication

### Highly Scalable

Any server that has the signing secret can validate tokens.

### No Session Store Dependency

No Redis or database lookup is required for authentication.

### Mobile-Friendly

Well suited for mobile apps and APIs.

## Disadvantages of Stateless Authentication

### Difficult Token Revocation

A token remains valid until it expires.

### Stolen Tokens Remain Valid

An attacker can use a stolen token until expiration.

### Secret Rotation Can Log Out Everyone

Changing the signing secret invalidates all tokens.

## Common Use Cases

Stateless authentication is ideal for:

- REST APIs
- Mobile applications
- Microservices
- Distributed systems

# Stateful vs Stateless Authentication

## Stateful Authentication

Characteristics:

- Session data stored on server
- Easy logout and revocation
- Requires session store
- Excellent control over sessions

## Stateless Authentication

Characteristics:

- Session data stored inside token
- No server-side session storage
- Harder to revoke tokens
- Better horizontal scalability

# Hybrid Authentication Approach

Many systems combine both approaches.

For example:

- Web browsers use stateful authentication.
- Mobile apps use JWT-based stateless authentication.
- Internal services use API keys or client credentials.

This approach balances security, scalability, and usability.

# API Key Authentication

API keys are long, random strings used to authenticate programmatic clients.

They are most commonly used for server-to-server communication.

## How API Keys Work

1. A user generates an API key from a dashboard.
2. The server stores metadata about the key.
3. The client includes the key in each request.
4. The server validates the key and associated permissions.

## Metadata Associated with API Keys

An API key may be associated with:

- Owner
- Permissions
- Expiration date
- Usage limits
- Billing information

## Example: OpenAI API Keys

Developers can generate API keys to call OpenAI models programmatically instead of using the ChatGPT interface.

## Machine-to-Machine Communication

API keys are ideal when one server needs to call another server without any user interaction.

### Example Workflow

1. A user sends a request to your application.
2. Your server calls another API using an API key.
3. The external service returns a response.
4. Your server returns the processed result to the user.

## Advantages of API Keys

- Easy to generate
- Easy to use
- Ideal for automation
- Suitable for server-to-server communication

## Limitations of API Keys

- Must be stored securely
- May provide broad access if leaked
- Less suitable for end-user authentication

# OAuth 2.0

OAuth 2.0 is an authorization framework that allows one application to access resources from another application without sharing passwords.

## The Delegation Problem

Users often want one application to access data from another application.

Examples:

- A travel application reading flight confirmations from Gmail.
- A social application importing Google contacts.

## Why Password Sharing Was Problematic

Sharing passwords grants full account access and makes revocation extremely difficult.

OAuth solves this by issuing tokens with limited permissions.

## OAuth 2.0 Roles

### Resource Owner

The user who owns the data.

### Client

The application requesting access.

### Authorization Server

The server that authenticates the user and issues tokens.

### Resource Server

The server that hosts the protected resources.

## OAuth 2.0 Authorization Code Flow

### Step 1: Client Redirects User

The client redirects the user to the authorization server.

### Step 2: User Authenticates and Grants Permission

The user logs in and approves requested scopes.

### Step 3: Authorization Code Returned

The authorization server sends an authorization code to the client.

### Step 4: Code Exchanged for Tokens

The client exchanges the authorization code for an access token.

### Step 5: Access Token Used

The client uses the access token to access protected resources.

## Scopes

Scopes define what access is being requested.

Examples:

- Read contacts
- Read email
- View profile

## Common OAuth 2.0 Flows

### Authorization Code Flow

Used by server-side applications.

### Client Credentials Flow

Used for server-to-server communication.

### Device Code Flow

Used for devices with limited input, such as smart TVs.

### Implicit Flow

Historically used for browser apps but now discouraged.

# OpenID Connect (OIDC)

OAuth 2.0 handles authorization but does not standardize authentication.

OpenID Connect adds authentication on top of OAuth 2.0.

## ID Token

OIDC introduces an ID token, which is typically a JWT containing user identity information.

Typical claims include:

- Subject (`sub`)
- Name
- Email
- Profile picture
- Issuer (`iss`)
- Issued-at time (`iat`)

## Sign in with Google

When a user clicks "Sign in with Google":

1. The application redirects the user to Google.
2. The user logs in to Google.
3. Google returns an ID token.
4. The application verifies the token.
5. The application extracts user profile information.

## Benefits of OpenID Connect

- Standardized authentication
- Simplified social login
- Verified identity information
- Reduced password management burden

# Choosing the Right Authentication Method

## Use Stateful Authentication When

- Building traditional web applications
- You need immediate logout
- You need centralized session control

## Use Stateless Authentication When

- Building APIs
- Supporting mobile applications
- Running globally distributed services

## Use API Keys When

- Providing programmatic API access
- Supporting machine-to-machine communication

## Use OAuth 2.0 When

- Allowing third-party applications to access user resources

## Use OpenID Connect When

- Implementing "Sign in with Google" or similar features

# Authorization

After authentication, the server must determine what actions the user can perform.

This process is called authorization.

## Example Scenario

In a note-taking platform:

- Users can create and edit their own notes.
- Moderators can review content.
- Admins can access deleted notes and manage the system.

# Role-Based Access Control (RBAC)

RBAC is the most common authorization model.

Users are assigned roles, and roles are associated with permissions.

## Core Concepts

### Roles

Examples:

- User
- Moderator
- Admin

### Permissions

Examples:

- Read notes
- Create notes
- Update notes
- Delete notes
- Access archived or deleted notes

### Role Assignment

Each user is assigned one or more roles.

## RBAC Workflow

### Step 1: User Authenticates

The server identifies the user.

### Step 2: Server Retrieves Roles

Roles are loaded from a database or token.

### Step 3: Permission Check

The server checks whether the role grants the required permission.

### Step 4: Access Granted or Denied

If permission is missing, the server returns `403 Forbidden`.

# Secure Error Messages

Authentication responses should not reveal unnecessary information.

## Unsafe Error Messages

Examples:

- User not found
- Incorrect password
- Account locked

These messages help attackers understand which part of authentication failed.

## Safe Error Message

Use a generic message such as:

`Authentication failed.`

This prevents attackers from learning whether the username or password was incorrect.

# Timing Attacks

Timing attacks exploit differences in response times to infer sensitive information.

## Example Scenario

### Case 1: Username Does Not Exist

The server fails immediately.

### Case 2: Username Exists but Password Is Wrong

The server performs password hashing before failing.

Because hashing takes time, the second case responds more slowly.

Attackers can measure this difference to identify valid usernames.

## Defenses Against Timing Attacks

### Constant-Time Comparisons

Use cryptographic comparison functions that take the same amount of time regardless of input.

### Artificial Delays

Add a fixed delay to all authentication failures.

This ensures that all failures take approximately the same amount of time.

# Password Storage

Passwords must never be stored in plain text.

## Secure Password Storage Process

### During Registration

1. Receive the password.
2. Hash it using a secure algorithm.
3. Store only the hash.

### During Login

1. Receive the password.
2. Hash it using the same algorithm.
3. Compare the new hash with the stored hash.

## Recommended Hashing Algorithms

- Argon2
- bcrypt
- scrypt

# HTTP Status Codes

## 401 Unauthorized

Returned when authentication fails or credentials are missing.

## 403 Forbidden

Returned when the user is authenticated but lacks required permissions.

# Practical Rules of Thumb

## Authentication

- Use sessions for browser-based applications.
- Use JWTs for APIs and mobile apps.
- Use API keys for server-to-server communication.
- Use OAuth 2.0 for delegated authorization.
- Use OpenID Connect for social login.

## Authorization

- Start with RBAC.
- Assign permissions to roles.
- Check permissions for every protected action.

## Security Best Practices

- Hash passwords securely.
- Use generic authentication error messages.
- Defend against timing attacks.
- Rotate API keys and secrets regularly.
- Revoke sessions when needed.

