---
title: What is API?
date: 2025-01-01
categories: [api]
---

Application Programming Interface is a tool that allows different software applications to communicate with each other. Think of it as a bridge that connects systems, enabling them to share data and perform tasks without knowing the details of how the other system works.

<!--more-->

For example, when you book a flight online, the travel website uses an API to get flight details from the airline’s database.

```mermaid
sequenceDiagram
    participant Client
    participant API_Server
    Client->>API_Server: HTTP Request (GET/POST)
    API_Server-->Client: HTTP Response (200 OK / 404 Not Found)
```

## Types of APIs

### 1. Web APIs

These are the most common and allow communication over the internet using protocols like HTTP. Examples include REST APIs and GraphQL APIs.

```mermaid
graph TD;
    A[Client] -->|HTTP Request| B[Web API]
    B -->|HTTP Response| A
    B --> C[Database]
    B --> D[External Service]
```

### 2. Library APIs

These let developers use functions from a library (e.g., a math or graphics library) within their application.

```mermaid
flowchart LR
    A[Application] --> B[Library API]
    B --> C[Function 1]
    B --> D[Function 2]
    B --> E[Function 3]
```

### 3. Operating System APIs

These allow applications to interact with the operating system, like accessing files or managing memory.

```mermaid
sequenceDiagram
    participant Application
    participant OS_API
    participant FileSystem
    Application->>OS_API: Request file access
    OS_API->>FileSystem: Fetch file
    FileSystem-->>OS_API: File data
    OS_API-->>Application: Return file content
```

### 4. Database APIs

These allow applications to query and update databases.

```mermaid
graph TD;
    A[Application] -->|SQL Query| B[Database API]
    B --> C[Database]
    C -->|Query Result| B
    B --> A
```

## API Implementations

### 1. REST (Representational State Transfer)

REST APIs are simple, stateless, and work over HTTP. They use methods like GET, POST, PUT, and DELETE for operations.
Example: A weather app fetching data from a weather API using a GET request.

```mermaid
sequenceDiagram
    participant Client
    participant REST_API
    Client->>REST_API: GET /weather
    REST_API-->>Client: JSON Response { "temp": 25°C }
```

### 2. GraphQL

GraphQL is a query language for APIs that allows clients to request specific data, making it flexible and efficient.
Example: A shopping app requesting only product names and prices from an API.

```mermaid
sequenceDiagram
    participant Client
    participant GraphQL_Server
    Client->>GraphQL_Server: Query { product { name, price } }
    GraphQL_Server-->>Client: { "name": "Laptop", "price": 1200 }
```

### 3. SOAP (Simple Object Access Protocol)

SOAP APIs use XML and have strict standards, often used in enterprise applications like payment gateways.

```mermaid
graph TD;
    A[Client] -->|SOAP Request| B[SOAP API]
    B -->|XML Response| A
```

### 4. gRPC

A modern API framework that uses Protocol Buffers for faster communication, often used in microservices.

```mermaid
sequenceDiagram
    participant Service_A
    participant gRPC
    participant Service_B
    Service_A->>gRPC: Request serialized data
    gRPC->>Service_B: Transfer binary data
    Service_B-->>gRPC: Return response
    gRPC-->>Service_A: Decode response
```

## API Lifecycle

Understanding the **API Lifecycle** helps in managing APIs effectively from inception to retirement.

**Stages:**

- **API Design:** Planning the structure and functionality of the API.
- **Development:** Writing the code and building the API.
- **Testing:** Ensuring the API works as intended and is free of bugs.
- **Deployment:** Making the API available for use.
- **Monitoring:** Tracking the performance and usage of the API.
- **Versioning:** Updating the API to add features or fix issues without disrupting existing users.

```mermaid
graph LR
    A[API Design] --> B[Development]
    B --> C[Testing]
    C --> D[Deployment]
    D --> E[Monitoring]
    E --> F[Versioning]
    F --> B
```

## API Security

APIs are vulnerable to attacks like SQL injection, DDoS, and data breaches. Security measures like authentication, authorization, and encryption are crucial to protect APIs.

**Stages:**

- **Client:** The user or application making the API request.
- **API Gateway:** Manages incoming requests and routes them appropriately.
- **Authentication Service:** Validates the security token provided by the client.
- **API Server:** Processes the request if authentication is successful.
- **Database:** Stores and retrieves data as needed.

```mermaid
graph TD
    A[Client] -->|Request with Token| B[API Gateway]
    B -->|Validate Token| C[Authentication Service]
    C -->|Token Valid| D[API Server]
    D -->|Process Request| E[Database]
    E -->|Send Data| D
    D -->|Response| B
    B -->|Forward Response| A
```

## API Rate Limiting

Rate limiting restricts the number of requests a client can make to an API within a specific time frame, preventing abuse and ensuring fair usage.

```mermaid
sequenceDiagram
    participant Client
    participant API_Gateway
    participant Rate_Limiter
    participant API_Server
    
    Client->>API_Gateway: API Request
    API_Gateway->>Rate_Limiter: Check Rate Limit
    alt Within Limit
        Rate_Limiter-->>API_Gateway: Allow Request
        API_Gateway->>API_Server: Forward Request
        API_Server-->>API_Gateway: Response
        API_Gateway-->>Client: Send Response
    else Exceeded Limit
        Rate_Limiter-->>API_Gateway: Reject Request
        API_Gateway-->>Client: 429 Too Many Requests
    end
```

## API Versioning

API versioning is crucial to maintain backward compatibility and allow for changes without breaking existing client applications.

```mermaid
flowchart LR
    A[API v1] -->|Deprecated| B[Client]
    A --> C[API v2]
    C --> D[New Features]
    D --> B
    C --> E[Old Features]
    E --> B
```

**Explanation:**

- **API v1:** The original version of the API.
- **API v2:** An updated version introducing new features while maintaining old functionalities.
- **Client:** Can continue using the deprecated v1 or migrate to v2.

## API Gateway Architecture

API gateways act as a single entry point for multiple APIs, providing security, monitoring, and routing capabilities.

```mermaid
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Authentication Service]
    B --> D[Rate Limiter]
    B --> E[REST API]
    B --> F[GraphQL API]
    B --> G[gRPC API]
    E --> H[Service A]
    F --> I[Service B]
    G --> J[Service C]
```

**Explanation:**

- **API Gateway:** Acts as a single entry point for all API requests.
- **Authentication Service:** Handles user authentication.
- **Rate Limiter:** Manages request rates to prevent abuse.
- **REST, GraphQL, gRPC APIs:** Different types of APIs managed by the gateway.
- **Services A, B, C:** Backend services handling specific functionalities.

## Real-World API Integration Example

Demonstrate how multiple APIs work together in a real-world scenario.

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Auth_API
    participant Data_API
    participant Database
    
    User->>Frontend: Login Request
    Frontend->>Auth_API: Authenticate User
    Auth_API-->>Frontend: Auth Token
    Frontend->>Data_API: Request Data with Token
    Data_API->>Database: Fetch Data
    Database-->>Data_API: Data Response
    Data_API-->>Frontend: Send Data
    Frontend-->>User: Display Data
```

In this flow:

1. The User sends a login request to the Frontend.
2. The Frontend communicates with the Auth_API to authenticate the user.
3. Upon successful authentication, the Auth_API returns an authentication token.
4. The Frontend uses this token to request data from the Data_API.
5. The Data_API fetches the required data from the Database and returns it to the Frontend.
6. Finally, the Frontend displays the data to the User.

## Why APIs Matter

APIs are essential because they:

- Enable software systems to work together seamlessly.
- Simplify complex tasks by hiding implementation details.
- Allow businesses to build integrations and create better user experiences.

APIs power everything from social media integrations to payment systems, making them a cornerstone of modern technology.
