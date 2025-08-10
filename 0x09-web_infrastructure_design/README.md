web design
# 4. Advanced — Application Server vs Web Server

## Scenario
We are upgrading our web infrastructure to:
- Add **one more server** for scalability.
- Configure **HAProxy** load balancers in a **cluster** for redundancy.
- Split the **Web Server**, **Application Server**, and **Database** into their **own dedicated servers**.

This design increases **performance**, **reliability**, and **scalability**.

---

## Whiteboard Diagram

User (Browser)
|
| 1. Requests www.foobar.com
v
+--------------------------+
| DNS System |
| (A record: www -> LB VIP)|
+--------------------------+
|
v
+-----------------------------------+
| Load Balancer #1 (HAProxy) |
| Clustered with Load Balancer #2 |
+-----------------------------------+
|
v
+-------------+
| Web Server |
| (Nginx) |
+-------------+
|
v
+-----------------+
| App Server |
| (Gunicorn, PHP) |
+-----------------+
|
v
+-----------------+
| Database Server |
| (MySQL) |
+-----------------+

markdown
Copy
Edit

---

## Components

### 1. **Additional Server**
- **Why**: Increases infrastructure capacity, allowing us to dedicate resources to specific roles.
- **Role**: Can host either the application server or database server depending on scaling needs.

### 2. **Load Balancer Cluster (HAProxy)**
- **Why**: Ensures high availability by removing the load balancer as a single point of failure.
- **Function**: Distributes incoming traffic between multiple web servers.
- **Cluster Mode**: Active-Passive or Active-Active using a **shared Virtual IP (VIP)**.

### 3. **Web Server (Nginx)**
- **Why**: Handles **static content** (HTML, CSS, JavaScript, images) and forwards dynamic requests to the application server.
- **Benefit**: Reduces load on the application server by offloading static file serving.

### 4. **Application Server**
- **Why**: Handles **dynamic content processing** (PHP, Python, Ruby code execution).
- **Benefit**: Separating from the web server allows independent scaling for CPU-intensive tasks.

### 5. **Database Server (MySQL)**
- **Why**: Stores and retrieves **persistent application data**.
- **Benefit**: Dedicated server improves data security and query performance.

---

## Why We Split Components

- **Performance**: Each server handles a specialized workload, optimizing efficiency.
- **Scalability**: We can scale web, application, and database tiers independently.
- **Reliability**: One tier failing doesn’t always bring down the entire system.

---

## Summary
By **adding a dedicated server** and **splitting web, app, and DB tiers**, this design:
- Improves **system performance** by reducing resource contention.
- Enhances **scalability** by allowing targeted upgrades.
- Increases **reliability** by isolating failures to specific components.
- Uses **HAProxy load balancer clustering** to prevent downtime from load balancer failure.