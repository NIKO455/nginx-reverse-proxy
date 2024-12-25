# NGINX Reverse Proxy and Load Balancer

## What is NGINX?

NGINX is a high-performance web server that is also used as a reverse proxy, load balancer, and HTTP cache. It is designed to efficiently serve static content and handle a high volume of concurrent connections, making it one of the most popular web servers in production environments.

### What is a Proxy?

A **proxy** server acts as an intermediary between a client and a destination server. It forwards requests from the client to the server and returns the response back to the client. 

#### Example:
When you use a **VPN** (Virtual Private Network), the connection is routed through the VPN server, which acts as a proxy. The client (your device) connects to the VPN server, and then the VPN server forwards your request to the desired website or service.

### What is a Reverse Proxy?

A **reverse proxy** is the opposite of a traditional proxy. While a proxy forwards requests from clients to the internet, a reverse proxy forwards requests from the multiple clients to a backend server. 

With NGINX as a reverse proxy:
- Users send requests to a single domain (e.g., `api.example.dev`).
- NGINX routes these requests to multiple backend servers (e.g., `Node.js servers, PHP servers etc`) based on load balancing rules or specific URL paths.
- The response from the backend server is then returned to the user.

### Why Use a Reverse Proxy?

In a scenario with multiple backend servers (e.g., multiple Node.js servers), a reverse proxy ensures that:
- Users do not have direct access to individual backend servers.
- Requests are distributed evenly across available servers (load balancing).
- If one server fails, others are still available to handle requests (high availability).
- The backend servers can communicate with a single shared database, ensuring data consistency.

### Example Setup:

Suppose you have a backend domain `api.example.dev`, and you have multiple Node.js servers running on different ports: 
- `8000`, `8001`, `8002`, `8003`.

The reverse proxy (NGINX) handles requests as follows:
- Users send requests to `api.example.dev`.
- NGINX forwards requests to one of the available backend servers (e.g., `8000`, `8001`, etc.).
- The backend server processes the request and accesses the database.
- NGINX sends the response back to the user.

#### NGINX as Load Balancer:
NGINX can also act as a **load balancer** by distributing incoming traffic evenly across multiple backend servers to prevent any single server from becoming overloaded. This ensures high availability and fault tolerance.

### Proxy vs Reverse Proxy

| **Proxy** | **Reverse Proxy** |
| --- | --- |
| The client connects to the proxy to access multiple websites or servers. | Multiple clients connect to a single server through the reverse proxy. |
| Example: A user accesses a website via a proxy server. | Example: Users access a service (like a Node.js app) through a reverse proxy like NGINX. |

### Load Balancer vs Reverse Proxy

- **Load Balancer:** A load balancer distributes incoming traffic across multiple servers to ensure no single server is overloaded, improving scalability and fault tolerance.
- **Reverse Proxy:** A reverse proxy forwards requests from clients to backend servers. It may or may not include load balancing, depending on configuration.

However, in most cases, NGINX can perform both functions—acting as a reverse proxy **and** a load balancer.

### How Does NGINX Handle Requests?

1. A user sends a request to `api.example.dev`.
2. The request is directed to the NGINX reverse proxy server.
3. NGINX uses rules (like load balancing, URL routing, etc.) to decide which backend server should handle the request.
4. The selected backend server processes the request, communicates with the database if necessary, and sends the response back to NGINX.
5. NGINX sends the response back to the user.

This allows users to interact with multiple backend servers without knowing how many servers exist, ensuring high availability and scalability.

### What Happens If the Reverse Proxy Fails?

NGINX is a lightweight, high-performance service, and its failure is unlikely unless the server itself encounters an issue. Since NGINX does not contain production code (i.e., it does not handle the actual business logic of the application), the risk of failure is minimal. However, it’s a good practice to run NGINX in a highly available setup, often with multiple NGINX instances to prevent single points of failure.

### Why is NGINX So Scalable?

NGINX is built with an **event-driven architecture**, which enables it to handle hundreds of thousands of concurrent connections efficiently. This makes NGINX highly scalable and well-suited for high-traffic websites and applications.

### What is Event-Driven Architecture?

**Event-driven architecture (EDA)** is a design pattern where components of an application communicate by producing and reacting to events. When an event occurs (e.g., a user request), it triggers actions in other components of the system. This allows for loosely coupled, asynchronous communication, making it ideal for handling real-time updates, complex workflows, and microservices environments.

### Use Case Example

In a microservices architecture, NGINX can act as a reverse proxy that directs requests to various services based on the URL or other rules. For example, requests to `api.example.dev/admin` might be directed to a specific backend server (`8002`), while requests to `api.example.dev/user` go to a different backend server.

---

## Visual Diagram: Reverse Proxy Architecture

Here’s a basic diagram illustrating how NGINX works as a reverse proxy in a typical setup:

```
        +------------------+              +-----------------------+
User --->|  NGINX Reverse  |------------->|   Backend Servers     |
        |      Proxy       |              | (e.g., Node.js apps)  |
        +------------------+              +-----------------------+
                 |                                    |
                 |                                    |
           +-----v-----+                      +-------v-------+
           | Server 1  |                      |  Server 2     |
           | (Port 8000)|                      |  (Port 8001)  |
           +-----------+                      +---------------+
```

- The **user** sends a request to `api.example.dev`, which is routed through the **NGINX Reverse Proxy**.
- NGINX decides which **backend server** (e.g., `8000`, `8001`, etc.) will handle the request.
- The backend server processes the request and sends the response back to NGINX.
- NGINX forwards the response to the user.

This setup ensures that traffic is efficiently routed to the appropriate backend server, providing load balancing and high availability.

---

## Resources

For more detailed information on how NGINX is designed for performance and scalability, check out [NGINX Blog](https://blog.nginx.org/blog/inside-nginx-how-we-designed-for-performance-scale).
