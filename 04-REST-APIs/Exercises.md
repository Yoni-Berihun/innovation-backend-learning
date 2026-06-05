# Basic REST API interview questions

## 1. How do you define REST?
REST is an architectural style for building scalable, reliable web services. It is not a protocol like SOAP; instead, it relies on standard HTTP methods such as GET, POST, PUT, PATCH, and DELETE to manipulate server resources.

RESTful systems use a client-server model. The client sends an HTTP request, and the server returns an HTTP response with the requested data or the result of an operation.

Resources are uniquely identified by URIs, and responses typically include a body, headers, and an HTTP status code.

## 2. What do the letters in REST stand for?
- R: Representational — the resource is represented in a format like JSON or XML.
- S: State — the representation reflects the resource’s state at a given time.
- T: Transfer — the state is transferred between client and server over HTTP.

## 3. What are REST APIs and why use them?
REST APIs are web services that expose resources via URIs and use HTTP methods to operate on those resources.

They are popular because they are simple, scalable, stateless, and language-agnostic. REST allows different systems—web, mobile, or server clients—to communicate without using a custom protocol.

## 4. What is the difference between HTTP and REST?
HTTP is a communication protocol that defines methods, headers, status codes, and message formats.
REST is an architectural style that uses HTTP to organize APIs around resources and operations, while following principles like statelessness and uniform interfaces.

Example: HTTP allows `GET /users`, but REST defines it as a way to retrieve the `users` resource.

## 5. What does “resource” mean in REST API?
A resource is a data entity managed by the API, such as a user, product, or order.

Each resource is identified by a URI like `/users/123`. Resources are manipulated using HTTP methods:
- GET: retrieve
- POST: create
- PUT: update or replace
- DELETE: remove

Responses include headers, request body data, and HTTP status codes like `200 OK`, `201 Created`, or `404 Not Found`.

## 6. What is an HTTP request, and what are its key parts?
An HTTP request has three main parts:
- Request line: method, URI, and HTTP version.
- Headers: metadata such as `Content-Type`, `Authorization`, and caching rules.
- Body: optional data payload for methods like POST or PUT.

Example request body: `{ "name": "Alice" }`.

## 7. What does an HTTP response consist of?
An HTTP response includes:
- Status line: protocol version and status code (`200 OK`, `404 Not Found`, etc.).
- Headers: metadata like `Content-Type` or `Cache-Control`.
- Body: the returned data, usually in JSON for REST APIs.

Example response body: `{ "id": 1, "name": "Alice" }`.

## 8. What is a URI in REST?
A URI (Uniform Resource Identifier) uniquely identifies a resource in a REST API.

Example: `/api/orders/45` points to order number 45.

## 9. What is the difference between query parameters and path parameters?
- Path parameters are part of the route and identify a specific resource, e.g. `/users/123`.
- Query parameters appear after `?` and are used for filtering, sorting, or pagination, e.g. `/users?age=30&sort=asc`.

Path parameters define what to fetch; query parameters define how or which subset to fetch.

## 10. What are the standard or core HTTP methods in REST?
- GET: fetch data
- POST: create resources
- PUT: update or replace resources
- PATCH: partially update resources
- DELETE: remove resources

These map closely to CRUD operations.

## 11. What is statelessness in a REST API?
Statelessness means every request must contain all information needed for processing. The server does not store session data between requests.

This improves scalability because servers can handle each request independently.

## 12. What is the concept of idempotency?
An operation is idempotent if repeating it produces the same result.

In REST:
- GET, PUT, DELETE are idempotent.
- POST is not, because it usually creates new resources.

## 13. What is cross-origin resource sharing (CORS)?
CORS is a browser security mechanism that controls access from one origin to another.

Servers use headers like `Access-Control-Allow-Origin` to permit requests from specific domains.

## 14. What is the purpose of the HTTP status code?
HTTP status codes indicate the result of a request:
- 2xx for success
- 4xx for client errors
- 5xx for server errors

They make it easier for clients to handle responses correctly.

## 15. Which HTTP status codes indicate success vs. errors?
- Success: `200 OK`, `201 Created`, `204 No Content`
- Client error: `400 Bad Request`, `401 Unauthorized`, `404 Not Found`
- Server error: `500 Internal Server Error`, `503 Service Unavailable`

## 16. Which HTTP status code fits a successful PATCH?
Use `200 OK` when the updated resource is returned.
Use `204 No Content` when the server successfully updates the resource and returns no body.

## 17. When should you use PUT vs. POST?
- PUT is idempotent and updates or replaces a resource at a known URI.
- POST creates a new resource under a collection URI and is not idempotent.

Example:
- `PUT /users/123` updates user 123.
- `POST /users` creates a new user.

## 18. How does HTTP basic authentication work?
The client sends credentials in the `Authorization` header as Base64-encoded `username:password`.

Because the credentials are easily decoded, Basic Auth should only be used over HTTPS.

## 19. What is the purpose of TLS for APIs?
TLS encrypts data in transit between client and server.

It prevents eavesdropping and tampering, which is essential when APIs send sensitive data like tokens or personal user information.

---

# Intermediate REST API interview questions

## 20. What are the constraints of REST architecture?
REST has six constraints:
- Client-server separation
- Statelessness
- Cacheable responses
- Uniform interface
- Layered system
- Code on demand (optional)

## 21. How do you handle versioning in RESTful APIs?
Common strategies:
- URI versioning: `/api/v1/users`
- Header versioning: `Accept: application/vnd.myapp.v2+json`
- Query versioning: `/users?version=2`

## 22. How do you maintain backward compatibility in REST APIs?
- Keep old versions alive while introducing new ones.
- Add optional fields instead of removing existing ones.
- Deprecate endpoints before removing them.
- Provide defaults for new required fields.

## 23. What is HATEOAS?
HATEOAS means the API response includes hyperlinks to related actions.

Example response for `GET /users/123`:
```
{
  "id": 123,
  "name": "Alice",
  "links": {
    "update": "/users/123",
    "delete": "/users/123",
    "orders": "/users/123/orders"
  }
}
```

This helps clients discover available operations dynamically.

## 24. Which HTTP methods are safe vs. idempotent?
- Safe methods do not modify state: GET, HEAD, OPTIONS.
- Idempotent methods produce the same effect when repeated: GET, PUT, DELETE.
- POST is neither safe nor idempotent.

## 25. How do you choose appropriate HTTP status codes?
Use:
- `200 OK` for successful reads
- `201 Created` when a resource is created
- `204 No Content` for successful updates or deletes with no body
- `400 Bad Request` for invalid input
- `401 Unauthorized` for missing authentication
- `403 Forbidden` for insufficient permissions
- `404 Not Found` for missing resources
- `500 Internal Server Error` for unexpected failures

## 26. How does the Cache-Control header work with ETags?
- `Cache-Control` tells clients how long they can reuse a response.
- `ETag` is a version token for a resource.

Clients send `If-None-Match` with the last ETag. If the resource hasn’t changed, the server returns `304 Not Modified`.

## 27. How does pagination work in REST APIs?
Common approaches:
- Page + limit: `/users?page=2&limit=20`
- Offset + limit: `/users?offset=40&limit=20`
- Cursor-based pagination: return a token for the next page

## 28. How do you handle multiple identical requests safely?
Use idempotency keys for operations like payments.

If the same request is retried with the same key, the server returns the same outcome instead of creating duplicates.

## 29. What is content negotiation, and how does it work?
Content negotiation uses the `Accept` header to ask the server for a preferred format.

Example:
- `Accept: application/json`
- `Accept: application/xml`

If the format cannot be produced, the API may return `406 Not Acceptable`.

## 30. How does REST compare to GraphQL and gRPC?
- REST is resource-based and simple with HTTP/JSON.
- GraphQL lets clients request exactly the fields they need.
- gRPC is a high-performance binary protocol using HTTP/2 and is ideal for microservices.

## 31. What is the difference between REST and SOAP protocols?
- REST is lightweight, flexible, and usually JSON-based.
- SOAP is rigid, XML-based, and relies on a WSDL contract.

SOAP includes built-in standards for security and error handling, but it is heavier than REST.

## 32. How do you enforce TLS for REST APIs?
- Serve only HTTPS endpoints.
- Redirect HTTP traffic to HTTPS.
- Reject plain HTTP requests.
- Use strong TLS versions and trusted certificates.
- Consider HSTS to prevent downgrades.

## 33. What are the key aspects to consider when implementing RESTful web services?
- Use clear, consistent URIs.
- Map HTTP methods to actions.
- Keep APIs stateless.
- Secure endpoints with authentication and authorization.
- Use caching for performance.
- Support pagination for large results.

## 34. What core REST API concepts should everyone know?
- Resource identification via URIs
- HTTP methods: GET, POST, PUT, PATCH, DELETE
- Status codes and error handling
- Statelessness and idempotency
- CORS and HTTPS
- Caching and pagination

## 35. How do you implement webhooks in RESTful web services?
Webhooks let a server notify a client when an event occurs.

Clients register a callback URL, and the server sends an HTTP POST request to that URL for each event.

## 36. What are the differences between JSON and XML in RESTful web services?
- JSON is lightweight, readable, and the default for modern APIs.
- XML is more verbose and supports namespaces and schemas.

JSON is usually preferred for performance and simplicity, while XML is still useful in enterprise systems that require strict validation.

## 37. What is the difference between Swagger and OpenAPI?
- OpenAPI is the specification for documenting REST APIs.
- Swagger is the toolset for working with OpenAPI, including editor and UI tools.

OpenAPI defines the contract; Swagger provides tooling around that contract.

---

# API design tips

- Use plural resource names: `/articles`, `/users`
- Keep endpoint names consistent with HTTP methods
- Return standard HTTP status codes
- Support filter, sort, and pagination for large collections
- Use nested routes for hierarchical resources: `/students/:studentId/subjects`
- Enforce good security practices: HTTPS, auth, role checks
- Cache data carefully, but avoid stale responses
- Version your API using `/v1/`, `/v2/`, etc.

## Conclusion
JSON, TLS, and HTTP status codes are the building blocks of modern APIs. Good REST design is not just about making features work — it is about making APIs easy to use, maintain, and extend.
