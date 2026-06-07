Basic REST API interview questions
1. How do you define REST?
<details>
<summary>Answer</summary>

REST is an architectural style used to design scalable and reliable web services or build distributed applications. Unlike protocols such as SOAP (Simple Object Access Protocol), it isn’t tied to a specific technology. Instead, it relies on standard HTTP methods like GET, POST, PUT, PATCH, and DELETE to interact with server resources.

RESTful systems are built on the client-server architecture, where the client sends an HTTP request and the server responds with an HTTP response containing the requested resource or details of the operation performed.

In RESTful APIs, every resource requested is uniquely identified using a Uniform Resource Identifier (URI). A request usually consists of a request line, request headers, and an optional request body, while the server sends back a response body along with response headers and an appropriate HTTP status code.
</details>

2. What do the letters in REST stand for?
<details>
<summary>Answer</summary>
REST stands for:

R (Representational): Refers to different formats like JSON or XML that represent a resource.
S (State): Shows the resource’s data at a given time.
T (Transfer): Reflects that the representation of the state is sent between the client and server over HTTP.

Each letter in the term describes how data is represented, managed, and transferred between client and server.
</details>

3. What are REST APIs and why use them?
<details>
<summary>Answer</summary>
REST APIs are web services that follow REST principles, exposing resources via URI (Uniform Resource Identifier) and allowing clients (like browsers, mobile apps, or other servers) to interact with backend services using HTTP.

Most developers prefer it because they are simple, scalable, stateless, and language-agnostic. REST APIs enable seamless communication between different systems, whether mobile, web, or microservices, without requiring custom protocols.
</details>

4. What is the difference between HTTP and REST?
<details>
<summary>Answer</summary>
HTTP is a protocol that governs the transmission of data between clients and servers on the web. It defines request methods like GET, POST, PUT, and DELETE, along with headers, response codes, and message structures.

REST, on the other hand, is an architectural style for designing APIs that leverages HTTP to structure APIs around resources and operations. It applies principles such as statelessness, resource identification via URIs, and uniform use of HTTP request methods to represent operations on resources.

For example, HTTP tells you that GET /users is a valid request method and URI, but REST defines how to use that request to represent the retrieval of a “users” resource.
</details>

5. What does “resource” mean in REST API?
<details>
<summary>Answer</summary>
According to the REST architectural pattern (Representational State Transfer), resources are the central building blocks of REST services. Examples of resources include users, products, or orders.

Each requested resource is uniquely identified by a Uniform Resource Identifier (URI), such as /users/123. This URI allows the REST client to send an HTTP request to the REST server and receive an HTTP response with the response body that contains the data in a format like JSON (JavaScript Object Notation) or XML.

Resources are manipulated using standard HTTP methods. For example, GET retrieves them, POST creates new ones, PUT updates them, and DELETE removes them. Along with the request body and request headers, the server includes response headers and appropriate HTTP status codes (200 OK, 201 Created, 404 Not Found) to indicate success or failure.
</details>

6. What is an HTTP request, and what are its key parts?
<details>
<summary>Answer</summary>

An HTTP request is how a client communicates with a server in REST. It typically has three parts: a request line, headers, and an optional body. Together, these parts tell the server what resource is being requested and how to process it.

Request line: This includes the HTTP method (like GET, POST, PUT, DELETE), the URI (which identifies the resource, such as /users/123), and the HTTP version.

Headers: These are additional details that help the server understand the HTTP request, such as the format of the data (Content-Type: application/json), authentication info (Authorization: Bearer <token>), or caching rules.

Body: This is optional and only present in some HTTP requests (like POST or PUT). It carries the actual data the client wants to send, usually in JSON format, for example, { "name": "Alice" }.

In simple terms, the HTTP request tells the server what resource you want, what you want to do with it, and any extra information needed to process it properly.
</details>

7. What does an HTTP response consist of?
<details>
<summary>Answer</summary>

An HTTP response is the server's response sent back to the client after it makes an HTTP request. In RESTful web services, there are usually three main parts:

First is the status line, which includes the HTTP protocol version and a status code (e.g., 200 OK, 404 Not Found, or 500 Internal Server Error). These codes are essential for error handling, since they help the client understand whether the request was successful, whether the user is unauthorized, or if there were server errors.

The second part is the response headers, which contain metadata about the response. These headers can define things like cache control, the content type (JSON or XML), or security measures such as authentication requirements. For example, APIs may use API keys in headers to ensure only authorized users can access certain resources, or they may enforce role-based access control.

The final part is the response body, which holds the actual dynamic data being returned, commonly in JSON format. For instance, when retrieving user information, the body might return { "id": 1, "name": "Alice" }.
</details>

8. What is a URI in REST?
<details>
<summary>Answer</summary>

URI stands for Uniform Resource Identifier; it uniquely identifies a resource in a REST API. It tells the client where the resource is located and how to access it. For example, /api/orders/45 might point to order #45.
</details>

9. What is the difference between query parameters and path parameters?
<details>
<summary>Answer</summary>

Path parameters are part of the resource path and used to identify specific resources, such as /users/123. Query parameters are appended after a "?" in the URI and majorly used for filtering, sorting, or pagination, such as /users?age=30&sort=asc.

In short, path parameters identify “what” to fetch, while query parameters define “how” to fetch it.
</details>

10. What are the standard or core HTTP methods in REST?
<details>
<summary>Answer</summary>

The core HTTP methods in REST are:

GET: Fetch data

POST: Create new resources

PUT: Update or replace existing resources

PATCH: Make partial updates

DELETE: Remove resources

These methods map directly to CRUD operations, making REST APIs easy to design and consume consistently across different systems.
</details>

11. What is statelessness in a REST API?
<details>
<summary>Answer</summary>

Statelessness means each client request must contain all necessary information for the server to process it, and the server does not store client session state. This makes REST APIs scalable, as servers don’t need to remember client context between requests.
</details>

12. What is the concept of idempotency?
<details>
<summary>Answer</summary>

Idempotency refers to an operation that produces the same result no matter how many times it is repeated. This is an important REST API concept because it ensures reliability, consistency, and safety in distributed systems where network issues may cause clients to retry requests.

In REST, GET, PUT, and DELETE are idempotent HTTP methods. Even if a developer calls them multiple times, it has no additional effect. The POST method is not idempotent because it creates new resources each time.
</details>

13. What is cross-origin resource sharing (CORS)?
<details>
<summary>Answer</summary>

CORS is a browser security mechanism that controls how resources in REST services are shared across different origins. By default, browsers block requests from one domain to another. Servers use response headers like Access-Control-Allow-Origin to specify which domains are allowed. CORS is essential when web clients consume APIs hosted on different domains.
</details>

14. What is the purpose of the HTTP status code?
<details>
<summary>Answer</summary>

HTTP status codes communicate the outcome of a request. They indicate whether the request was successful, failed due to client error, or failed due to server error.

For example, 200 OK means success; if you receive 404 Not Found, it indicates a missing resource. If any user receives 500 Internal Server Error, it signals a server issue. They standardize communication between client and server.
</details>

15. Which HTTP status codes indicate success vs. errors?
<details>
<summary>Answer</summary>

Success codes fall under the 2xx range, such as 200 OK, 201 Created, and 204 No Content. Client errors fall under the 4xx range, like 400 Bad Request or 401 Unauthorized. Server errors are in the 5xx range, such as 500 Internal Server Error or 503 Service Unavailable. These categories help quickly classify responses.
</details>

16. Which HTTP status code fits a successful patch?
<details>
<summary>Answer</summary>

For a successful PATCH request, the server usually returns 200 OK if the updated resource is included in the response, or 204 No Content if no body is returned. Both codes indicate success, but 200 OK is preferred when the client needs confirmation of the changes. It is important to check that the appropriate HTTP status code is implemented.
</details>

17. When should you use put vs. post?
<details>
<summary>Answer</summary>

The choice between PUT and POST in RESTful APIs depends on how you want to handle the request data and the resource being targeted. The PUT is idempotent, which means that sending the same request multiple times will always result in the same state of the resource.

For example, a PUT /users/123 with updated user details will replace or update that specific user. Even if the same request is sent again, the server processes it without creating duplicates.

On the other hand, POST is not idempotent. It is used to create new resources, often at a collection endpoint like POST /users. Every call creates a new entry, even if the request body contains the same request data. That’s why POST methods are best for operations where you expect a new identifier (like id) to be generated.

When working with secure REST APIs, you should also consider security measures. Both PUT and POST requests may carry sensitive data, so using custom request headers (for example, tokens, API keys, or encryption details) helps enforce stronger authentication and authorization.

Organizations can even implement their own security measures, like validating user roles, before allowing POST or PUT operations.
</details>

18. How does HTTP basic authentication work?
<details>
<summary>Answer</summary>

In HTTP basic authentication, the client sends credentials in the Authorization header as a Base64-encoded string in the format username:password. The server decodes and validates them. Since credentials are easily decodable, Basic Auth should always be used over HTTPS to ensure they are transmitted securely.
</details>

19. What is the purpose of TLS for APIs?
<details>
<summary>Answer</summary>

TLS (Transport Layer Security) ensures secure communication between client and server by encrypting the data exchanged. This prevents eavesdropping and tampering. For REST APIs, TLS is critical because sensitive data like authentication tokens or user details often travels over the network. Hence, it is best always to prefer HTTPS over plain HTTP.
</details>

When you move beyond the basics, the interviewers usually change their tactics and ask you to demonstrate your skills through the creation, building, and usage of REST APIs in real-world situations. Such questions often inquire about your theoretical understanding of REST along with its application in various practical cases, e.g., managing large volumes of data or taking care of the API's security and maintainability. These are some of the common intermediate-level REST API questions.

Intermediate REST API interview questions
20. What are the constraints of REST architecture?
<details>
<summary>Answer</summary>

There are six specific architectural constraints of REST architecture:

Client-server separation

The client (frontend) and server (backend) are independent, which allows them to evolve separately. UI changes won't break the API, and backend improvements can happen without affecting the client. For example, you can update your mobile app UI without touching the API.

Statelessness

Each API call must stand on its own. The server doesn’t retain any information between requests. Instead, every request carries everything the server needs: authentication, data context, etc. This simplifies scaling and reduces server complexity because there’s no session state to manage.

Cacheable

REST responses must explicitly state if they can be cached (and for how long). Caching improves performance and reduces server load by allowing clients or intermediaries to reuse recent data rather than requesting it repeatedly.

Uniform interface

REST uses a consistent set of standards, like URIs to identify resources, HTTP request methods (GET, POST, PUT, DELETE) to manipulate them, and clear media types (JSON, XML). This consistency makes APIs predictable and easier to use.

Layered system

API architecture can include layers like proxies, gateways, or load balancers, without the client knowing. This helps with scalability, security, and manageability while keeping the client-server interaction the same.

Code on demand (optional)

Sometimes the server may send executable code (like JavaScript) for the client to run. This is optional and allows for flexible client behavior, but it isn’t common in traditional REST APIs.
</details>

21. How do you handle versioning in RESTful APIs?
<details>
<summary>Answer</summary>

API versioning is necessary because APIs evolve over time. Without versioning, existing clients may break when changes are introduced, i.e., new fields are added or old endpoints are replaced. The common strategies are:

URI versioning: Include the version in the path, e.g., /api/v1/users. This is simple and widely used.

Header versioning: Specify the version in request headers, e.g., Accept: application/vnd.myapp.v2+json. This keeps the URI clean but requires more client configuration.

Query parameters: Append version to query string, e.g., /users?version=2. This is less common but sometimes used for quick testing.
</details>

22. How do you maintain backward compatibility in REST APIs?
<details>
<summary>Answer</summary>

Backward compatibility means old clients should continue working even when the API evolves. You can use the following strategies to maintain backward compatibility in REST APIs:

Keep old versions alive while introducing new ones (e.g., /v1/ and /v2/).

Instead of removing fields, add new optional fields so old clients don’t break.

Clearly mark old endpoints as deprecated and give clients time to migrate before shutting them down.

When adding new required fields, provide defaults so older clients don’t fail.

For example, if your /users API adds a new phoneNumber field, don’t remove existing email or name fields. Just add the new field in the response so older clients can ignore it.
</details>

23. What is HATEOAS?
<details>
<summary>Answer</summary>

HATEOAS stands for Hypermedia as the Engine of Application State and is part of the uniform interface constraint in REST architecture. It is one of the key, yet often overlooked, constraints of the REST architecture. In simple terms, it means that a REST server should provide not only the requested resource but also hyperlinks (or hypermedia controls) in the HTTP response that guide the client about possible next actions.

For example, if a client requests details of a user with GET /users/123, the response body might not only include the user data in JSON format but also links such as "update": "/users/123", "delete": "/users/123", "orders": "/users/123/orders". This way, the REST client can dynamically discover what it can do next without hardcoding URIs.

The real benefit of HATEOAS is decoupling. Existing clients don’t need to know all endpoints beforehand; they can follow links provided by the server. This supports backward compatibility when new features are added, helps improve error handling by pointing to recovery actions, and aligns with the client-server architecture principle of RESTful web services.
</details>

24. Which HTTP methods are safe vs. idempotent?
<details>
<summary>Answer</summary>

In REST, safe methods don’t modify server state. These include GET, HEAD, and OPTIONS. For example, GET /users only retrieves data; it does not change anything. Because it always returns the same result without side effects, GET is also idempotent.

Idempotent methods produce the same result no matter how many times you call them. PUT and DELETE are idempotent because repeating them doesn’t create additional changes.

For instance, DELETE /users/5 deletes the user once, and calling it again has no further effect. POST is neither safe nor idempotent; it always creates a new resource. Distinguishing safe vs. idempotent helps interviewers see if you understand reliability in REST design.
</details>

25. How do you choose appropriate HTTP status codes?
<details>
<summary>Answer</summary>

Developers must follow the simple guidelines when choosing a status code.

2xx success codes: Use 200 OK for successful GET, 201 Created when a resource is added, and 204 No Content when an update or delete succeeds without returning data.

4xx client errors: Use 400 Bad Request for invalid input, 401 Unauthorized for missing authentication, 403 Forbidden for insufficient permissions, and 404 Not Found for missing resources.

5xx server errors: Use 500 Internal Server Error for generic failures or 503 Service Unavailable when a service is down.

For example, if a client sends an invalid email in a signup API, return 400 Bad Request, not 500, since it’s the client’s fault.
</details>

26. How does the cache-control header work with ETags?
<details>
<summary>Answer</summary>

Caching is important for performance. The Cache-Control header tells clients how long they can reuse a response. For example, Cache-Control: max-age=3600 means the response is valid for one hour.

ETags (Entity Tags), on the other hand, are unique identifiers for resource versions. Clients can send If-None-Match with the last ETag to check if the resource has changed. If it hasn’t, the server replies with 304 Not Modified, saving bandwidth.

Together, Cache-Control headers and ETags reduce server load and speed up responses, which is an important optimization for high-traffic APIs.
</details>

27. How does pagination work in REST APIs?
<details>
<summary>Answer</summary>

Pagination helps handle large datasets by splitting results into smaller chunks. Instead of returning thousands of records at once, APIs return a subset and let clients request more.

Common methods include:

Page & limit: /users?page=2&limit=20 → returns 20 users on page 2

Offset & limit: /users?offset=40&limit=20 → skips 40 users, then returns the next 20

Cursor-based: returns a “next” cursor token with each response, used to fetch subsequent results
</details>

28. How do you handle multiple identical requests safely?
<details>
<summary>Answer</summary>

Duplicate requests are common, especially in unreliable networks where clients retry operations. To prevent duplicate resource creation, APIs use idempotency keys.

For example, when creating a payment, the client includes a unique key like Idempotency-Key: abc123. If the same request is sent again with the same key, the server returns the same result instead of creating another payment.
</details>

29. What is content negotiation, and how does it work?
<details>
<summary>Answer</summary>

Content negotiation allows a client to tell the server what data format it prefers. This is usually done using the Accept header.

For example:

javascript

Accept: application/json: server responds in JSON.
Accept: application/xml: server responds in XML.
The server decides which representation to return based on the request. If it can’t provide the requested format, it may return 406 Not Acceptable.
</details>

30. How does REST compare to GraphQL and gRPC?
<details>
<summary>Answer</summary>

REST is resource-based, widely adopted, and simple to use with HTTP. However, it may lead to over-fetching or under-fetching data.

GraphQL gives clients the flexibility to request only the fields they need in a single query. This reduces round-trip time but requires more setup.

gRPC is a high-performance, binary protocol based on HTTP/2. It supports streaming and is great for microservices, but less human-readable than REST.
</details>

31. What is the difference between REST and SOAP protocols?
<details>
<summary>Answer</summary>

REST is lightweight, flexible, and usually uses JSON over HTTP. It’s easy to use and widely adopted in modern web applications.

SOAP (Simple Object Access Protocol) is more rigid, relies on XML, and requires a WSDL (Web Services Description Language). It has built-in error handling and security, but is heavy compared to REST.
</details>

32. How do you enforce TLS for REST APIs?
<details>
<summary>Answer</summary>

TLS ensures all data is encrypted between the client and the server. To enforce it:

Configure the API server to accept only HTTPS.

Redirect all HTTP traffic to HTTPS.

Reject requests that attempt plain HTTP.

In addition, use strong TLS versions (e.g., TLS 1.2 or higher), rotate certificates regularly, and consider enforcing HSTS (HTTP Strict Transport Security) headers to prevent downgrade attacks. It’s also important to obtain certificates from trusted certificate authorities (CAs) and to turn off weak ciphers or outdated protocols (like SSLv3 or TLS 1.0) to maintain security. This is essential for protecting sensitive data like login credentials or payments.
</details>

33. What are the key aspects to consider when implementing RESTful web services?
<details>
<summary>Answer</summary>

When building REST APIs, make them simple and reliable. Use clear and consistent paths (URIs) for your resources and match each HTTP method to the correct action, i.e, (GET to read, POST to add). Keep the API stateless so each request is standalone.

Key practices to follow are:

Security: Implement login or token checks to protect resources.

Performance: Use caching to speed up responses.

Handling large data: Apply pagination to avoid sending too much data at once.

Server stability: Set limits or monitoring to prevent overload.
</details>

34. What core REST API concepts should everyone know?
<details>
<summary>Answer</summary>

Everyone working with REST should know how resources are identified with URIs and how to use the main HTTP methods (GET, POST, PUT, DELETE, PATCH) on them. It is also important to understand the status codes and idempotency, statelessness and authentication. Other key ideas include caching to improve speed, pagination for large data sets, and common security measures like using HTTPS (TLS) and handling cross-origin requests (CORS).
</details>

35. How do you implement webhooks in RESTful web services?
<details>
<summary>Answer</summary>

Webhooks enable servers to notify clients of events, eliminating the need for clients to constantly poll. To implement, the client registers a callback URL with the API provider.

When an event occurs (e.g., a payment succeeds), the server makes an HTTP POST request to that callback. The client processes the event and sends back a response (like 200 OK).

For example, Stripe uses webhooks to notify your system about payment events.
</details>

36. What are the differences between JSON and XML in RESTful web services?
<details>
<summary>Answer</summary>

In RESTful web services, the two most common data formats for request and response bodies are JSON (JavaScript Object Notation) and XML (eXtensible Markup Language). Both serve the same purpose, i.e., structuring data, but they differ in style and use cases.

JSON is lightweight, less verbose, and very close to how data structures are represented in most programming languages, especially JavaScript. It uses key-value pairs, arrays, and objects, which makes it faster to parse and easier for developers to work with. Because of its simplicity and efficiency, JSON has become the default choice for most modern REST APIs. For example, { "id": 1, "name": "Alice" }.

XML, on the other hand, is more verbose and uses opening/closing tags, which increases payload size. However, XML is more powerful in certain scenarios; it supports attributes, namespaces, and strict schemas (XSD), making it useful where data validation and complex hierarchical structures are important. For example:

xml

<user id="1"> 
<name>Alice</name> 
</user>

JSON is preferred mostly for performance and readability, but XML is still valuable in enterprise systems or when schema validation and document-like structures are required.
</details>

37. What is the difference between Swagger and OpenAPI?
<details>
<summary>Answer</summary>

The terms Swagger and OpenAPI are closely related. The key point is that OpenAPI is the specification, while Swagger is a set of tools that help you use that specification.

OpenAPI is a standard way to describe REST APIs. It defines how you can document details such as available endpoints, request methods, query parameters, authentication types, and expected responses. For example, an OpenAPI document (usually written in YAML or JSON) acts like a blueprint of the API that both humans and machines can understand. This makes it easier to generate documentation, client SDKs, and tests automatically.

Swagger, on the other hand, started as both a specification and a toolset. Later, the specification part was donated to the Linux Foundation and renamed OpenAPI Specification (OAS). Since then, “Swagger” refers mainly to the tools that support OpenAPI. These include:

Swagger Editor to design APIs.

Swagger UI to visualize and interact with API docs in the browser.

Swagger Codegen to generate server stubs or client SDKs.

In short, OpenAPI is the specification (the rules and format), and Swagger is the toolset that implements and works with that specification.
</details>

1. What is a REST API and why is it important?
<details>
<summary>Answer</summary>

A REST API (Representational State Transfer Application Programming Interface) is a set of rules and conventions for building and interacting with web services. It relies on stateless, client-server communication and uses HTTP methods such as GET, POST, PUT, and DELETE for operations.

REST APIs are important because they promote scalability, simplicity, and flexibility in web services development. They enable different systems to communicate and exchange data seamlessly, regardless of the underlying architecture.

Look for candidates who can explain the basic principles of REST and why it’s widely adopted. An ideal response should mention scalability, simplicity, and flexibility.
</details>

2. Can you explain the difference between PUT and POST methods in REST API?
<details>
<summary>Answer</summary>

The PUT method is used to update a resource or create a resource if it doesn’t exist. It’s idempotent, meaning multiple identical requests should have the same effect as a single request. Essentially, PUT replaces the resource at the given URL with the payload provided in the request.

On the other hand, the POST method is used to create a new resource. It’s not idempotent, meaning multiple identical requests may result in multiple new resources. POST typically adds a new item to a collection of resources.

Candidates should highlight the idempotent nature of PUT and the non-idempotent behavior of POST. This shows their understanding of how these methods are used effectively in RESTful services.
</details>

3. What are the key components of a RESTful web service?
<details>
<summary>Answer</summary>

The key components of a RESTful web service include:

Resources: Entities or objects that the service manages, represented by URIs (Uniform Resource Identifiers).
HTTP Methods: The actions that can be performed on resources, such as GET, POST, PUT, and DELETE.
Representation: The format in which resources are represented, often using JSON or XML.
Statelessness: Each request from a client contains all the information needed to process the request, with no reliance on previous interactions.
Hypermedia: Links to related resources, allowing clients to navigate the API.

Look for candidates who can clearly identify and explain these components, demonstrating a comprehensive understanding of RESTful web services.
</details>

4. What is statelessness in REST APIs and why is it important?
<details>
<summary>Answer</summary>

Statelessness means that each request from a client to a server must contain all the information needed to understand and process the request. The server does not store any context or state information between requests.

Statelessness is important because it simplifies the server design, improves scalability, and makes it easier to handle and route requests. Each request is independent, which means that servers can be added or removed without affecting the overall system.

Candidates should emphasize the benefits of statelessness, such as simplification of server design and improved scalability. This shows their understanding of why REST APIs are built this way.
</details>

5. How do you handle errors in a RESTful web service?
<details>
<summary>Answer</summary>

Error handling in a RESTful web service typically involves using standard HTTP status codes to indicate the result of a request. Common status codes include 200 (OK), 201 (Created), 400 (Bad Request), 401 (Unauthorized), 404 (Not Found), and 500 (Internal Server Error).

In addition to status codes, the response body can include more detailed error messages or codes to provide additional context to the client. This helps clients understand what went wrong and how to address the issue.

Look for candidates who can explain the use of HTTP status codes and detailed error messages in response bodies. This indicates their ability to design robust and user-friendly APIs.
</details>

6. Can you explain the concept of resource representation in REST?
<details>
<summary>Answer</summary>

Resource representation in REST refers to the format in which resources are presented to clients. This is typically done using standard data formats like JSON or XML. The representation includes the data of the resource as well as metadata about the resource.

The representation allows clients to understand the structure and properties of the resource. Clients can request specific representations through content negotiation by specifying the desired format in the Accept header of the HTTP request.

Candidates should be able to explain the importance of resource representation and how content negotiation works. This shows their understanding of delivering data in a flexible and client-friendly manner.
</details>

7. What is HATEOAS, and why is it significant in RESTful APIs?
<details>
<summary>Answer</summary>

HATEOAS (Hypermedia As The Engine Of Application State) is a constraint of RESTful APIs that allows clients to interact with the application entirely through hypermedia provided dynamically by the server. This means that the server includes links in its responses to guide clients through the available actions.

HATEOAS is significant because it decouples clients from server implementation details, allowing for more flexible and maintainable client-server interactions. Clients do not need to know the URI structure upfront; they can discover available actions via the links provided.

Look for candidates who understand HATEOAS and can explain how it enhances the flexibility and maintainability of RESTful APIs. This indicates their knowledge of advanced REST concepts.
</details>

8. Describe the role of HTTP headers in RESTful web services.
<details>
<summary>Answer</summary>

HTTP headers play a crucial role in RESTful web services by providing meta-information about the request or response. They can include details such as content type, content length, authentication credentials, caching directives, and more.

For example, the Content-Type header indicates the media type of the resource (e.g., JSON, XML), while the Authorization header is used to pass authentication credentials. Headers like Cache-Control and ETag help manage caching and resource versioning.

Candidates should highlight the importance of HTTP headers in conveying essential information and controlling aspects of communication. This shows their understanding of how to effectively use headers in RESTful APIs.
</details>

9. What are some common security considerations for REST APIs?
<details>
<summary>Answer</summary>

Common security considerations for REST APIs include:

Authentication and Authorization: Ensuring that only authenticated users can access the API and that they have the appropriate permissions.
Encryption: Using HTTPS to encrypt data in transit, protecting it from eavesdropping and tampering.
Rate Limiting: Preventing abuse by limiting the number of requests a client can make within a certain period.
Input Validation: Validating and sanitizing input to prevent injection attacks and other malicious activities.
Error Handling: Avoiding the exposure of sensitive information in error messages.

Look for candidates who can discuss these considerations and understand the importance of securing RESTful APIs. This indicates their awareness of potential vulnerabilities and their ability to implement best practices.
</details>

10. How do you version a REST API, and why is it necessary?
<details>
<summary>Answer</summary>

Versioning a REST API involves assigning version numbers to different iterations of the API to manage changes and ensure backward compatibility. Common methods include:

URI Versioning: Including the version number in the URI (e.g., /api/v1/resource).
Header Versioning: Specifying the version number in a custom HTTP header (e.g., X-API-Version: 1).
Query Parameter Versioning: Adding the version number as a query parameter (e.g., /api/resource?version=1).

Versioning is necessary to manage changes without disrupting existing clients. It allows developers to introduce new features and improvements while maintaining support for older versions.
</details>

1. Name of the endpoint should be accompanied by the HTTP method
Being a backend developer you might have been familiar with the various HTTP request methods to perform CRUD actions in your application especially the ones, which are common such as GET, POST, PUT, DELETE, and PATCH. In case you are not familiar with these methods then read the blog HTTP Request Methods. 

When you're implementing an API make sure that the name of the endpoints linked with the resources go along with the HTTP method related to the actions you're using in your application. Look at the example given below for reference...

- GET /get_customers
- POST /insert_customers
- PUT /modify_customers
- DELETE /delete_customers
+ GET /customers
+ POST /customers
+ PUT /customers
+ DELETE /customers
2. Return standard error codes according to the result of our API
While implementing an API the endpoints return the result that whether the action is successful or not. The result is always associated with some status code. For example: if you get the status 200 (ok) then it means the result is successful and if you get the status code 400 (bad request) then the result is failed (You can check the response using some tools like Postman to get to know the status code for the actions you perform in your application).

Handle the errors gracefully and return the HTTP response code to indicate what kind of error is generated. This is helpful for API maintainers to understand the issues and troubleshoot them. 

Make sure that you know the existing status code to identify the case you need to apply each one of them. Sometimes it happens that the return message does not match with the status code and that creates confusion for the developers as well as for the consumers who are using that REST API. This is really harmful to the application. So it's good to take care of the status code for the actions you perform in your application. Below is one of the examples...

// Bad, status code 200 (Success)
// associated with an error object
{
    "status": 200,
    "error": {...}
}// Good
{
    "status": 200,
    "data": [...]
}
Some common error codes are given below...

400 Bad Request – Client-side input fails validation.
401 Unauthorized – User isn’t authorized to access a resource. It usually returns when the user isn’t authenticated.
403 Forbidden – User is authenticated, but it’s not allowed to access a resource.
404 Not Found – Resource is not found.
500 Internal server error – Generic server error. It probably shouldn’t be thrown explicitly.
502 Bad Gateway – This indicates an invalid response from an upstream server.
503 Service Unavailable – Something unexpected happened on the server-side (It can be anything like server overload, some parts of the system failed, etc.).
3. Support for the Filter, Sort, and Pagination
How would your server react if you implement an API in your application and the endpoints return the results in millions....???

Your server will be down and it's really going to cry in front of you...(jokes apart...)

Many times it happens that we only need to consume some specific or fewer resources from our server. The reason could be anything. It can be the performance of the system, search system, or the massive amount of information that is not needed to return all at once. You can use a filter to return some specific item based on the condition. If you want to return a few results at a time then use pagination in your API. 

These concepts will help you to display only a specific type of information and it will increase the performance of your system by consuming fewer resources. Examples are given below...

Filter: filter the customer with the following properties... the last name is Smith and the age is 30.
GET /customers?last_name=Smith&age=30
Pagination: Return 20 rows starting from 0
GET /customers?limit=20&offset=0
Sort: Return rows sorted by email in the ascendant.
GET /customers?sort_by=asc(email)
4. Endpoints name should be plural
If you have implemented any kind of API in your application then you might have come across a question that whether the endpoint name should be singular or plural. Remember that you need to maintain consistency throughout your application. So it's good to build the endpoints in the plural to be consistent in your database. 

It's not necessary that you will always get a single item. You can have many items in a table but even if you consider the scenario of getting the result only one and you place it singular everywhere then you won't be able to maintain the consistency in the name of your routes. 

- GET /article
- GET /article/:id
+ GET /articles
+ GET /articles/:id
5. Nesting resources for hierarchical objects
While implementing an API you need to take care of the path for the endpoints. The path of the endpoint deal with the nested resources. To create this path treat the nested resource as the name of the path and append it after the parent resource. Make sure that the nested resource matches the table you have stored in your database else it will create confusion for you. 

If you don't get the above point then understand this in a way that you have a list of students in your database. Each one of them has chosen the subjects they are interested in. Treat the 'subject' table as a child of a 'student' table in your database. 

Now, if you want to create the endpoint for the subjects associated with the student then append the /subjects path to the end of the /student path. If you're using the GET method then an example of the endpoint path will look something like the given below...

'/students/:studentId/subjects'

We get subjects on the students identified by studentId and then the response will be returned. Here, students are the parent's resources and the subject is the child's resources of the student.  So as discussed, we are adding subjects after the '/students/:studentId'. Each student has their own subject. The same kind of nesting structure will be applied to other methods as well such as POST, PUT and DELETE endpoints.

6. Follow good security practices
When you're implementing an API make sure that the communication between the client and the server is private because you often send and receive private information. For security purposes, you can use SSL/TLS.

Using the SSL certificate isn't too difficult. You can easily load it onto the server and the cost of an SSL certificate is also free and very low. Don't make your REST API open. It should communicate over secure channels.

When a user is accessing the information, they shouldn't be able to access more data they have requested. Being a user you are not allowed to access the information of another user or the data of admins. 

To implement the principle of the least privilege, add role checks for a single role or more granular roles for each user. If you want to group users into a few roles then they should be allowed to cover all they need and no more. 

For each feature that users have access to if you have more granular permission then make sure that the admins can easily add and remove those features for each user accordingly. Add some preset roles that can be applied to group users. You won't have to do this for every user manually. 

7. Cache data to increase the performance
You might have used caching during the implementation of some features in your application. Caching is really a powerful tool to speed up the performance of your application. Using caching you will get faster results and you won't have to extract the data from the database multiple times for the same query. 

Use caching during the implementation of your API. It will speed up the performance of your application and it will reduce the consumption of the resources. It's good to cache the data instead of running the same query and asking the database to give the same result (your database will start crying in front of you....lolzzz). 

One of the precautions you need to take care is that you don't get outdated data. Due to the old data, something wrong can happen and your system can generate a lot of bugs in a production environment. Do not keep the information for a long period of time in the cache. It is good to keep the data for a short period of time in the cache. 

Depending on the needs you can change the way data is cached. One of the great services to implement caching is Redis

8. Versioning
Keeping the different versions of your API will help you to track the changes and it will help you to restore the previous version in case if something goes wrong with the latest one. Consider a scenario that you implemented an API, deploy it and a lot of clients start using it. Now at some point, you want to make some changes and you added or removed the data from a resource. 

Chances are there that it will generate bugs on the external services that consume the interface. This is the reason you should keep the different versions of your API. From the previous version, you can get the backup and work on it further. 

Versioning can be done according to the semantic version. Don't force everyone to work on the same version at the same time, you can gradually remove the old versions of your API once you see that it's not required anymore. Most of the time versioning is done with /v1/, /v2/, etc. added at the start of the API path.

GET /v1/customers
GET /v2/students
Conclusion
JSON, SSL/TLS, HTTP Status codes are the standard building blocks of the modern web app API. TO design a high-quality Restful API follow the best conventions we have discussed above. 

Being a backend developer your job is not just to implement the features you are asked to do. You also need to take care of doing it in the best possible way. Apply the best principle when you're implementing an API so that people who consume and work on it as it.