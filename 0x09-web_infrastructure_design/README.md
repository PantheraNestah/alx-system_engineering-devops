# Web Infrastructure Design

This project explores the evolution of web infrastructure design, from simple single-server setups to complex, scalable, and secure distributed systems. Each task builds upon the previous one, demonstrating how to address common challenges in web infrastructure.

## Table of Contents

- [0. Simple Web Stack](#0-simple-web-stack)
- [1. Distributed Web Infrastructure](#1-distributed-web-infrastructure)
- [2. Secured and Monitored Web Infrastructure](#2-secured-and-monitored-web-infrastructure)
- [3. Scale Up](#3-scale-up)

---

## 0. Simple Web Stack

**File**: `0-simple_web_stack`

A basic single-server web infrastructure hosting the website `www.foobar.com`.

### Key Components:
- **1 Server**: Single physical/virtual machine hosting all components
- **1 Web Server (Nginx)**: Handles HTTP requests and serves static content
- **1 Application Server**: Executes dynamic application code
- **1 Database (MySQL)**: Stores persistent application data
- **1 Domain Name**: `foobar.com` with `www` record pointing to server IP

### Issues with This Design:
- **Single Point of Failure (SPOF)**: If any component fails, entire website goes down
- **Downtime During Maintenance**: Website unavailable during updates/patches
- **Cannot Scale**: Limited resources cannot handle traffic spikes

---

## 1. Distributed Web Infrastructure

**File**: `1-distributed_web_infrastructure`

A three-server distributed infrastructure that addresses scalability and reliability concerns.

### Key Components:
- **Load Balancer (HAProxy)**: Distributes incoming requests using Round Robin algorithm
- **2 Web/Application Servers**: Redundant servers handling web requests
- **Database Primary-Replica Cluster**: Primary handles writes, Replica handles reads

### Improvements:
- **Redundancy**: Multiple servers provide backup if one fails
- **Load Distribution**: Traffic spread across multiple servers
- **Database Optimization**: Read/write separation improves performance

### Remaining Issues:
- **Load Balancer SPOF**: Single load balancer is still a failure point
- **Primary Database SPOF**: Write operations depend on single database
- **No Security**: Missing HTTPS, firewalls, and monitoring

---

## 2. Secured and Monitored Web Infrastructure

**File**: `2-secured_and_monitored_web_infrastructure`

Enhanced infrastructure with security measures and monitoring capabilities.

### Security Enhancements:
- **3 Firewalls**: Network security for each server
- **SSL Certificate**: HTTPS encryption for secure communication
- **Traffic Control**: Restricted access on specific ports

### Monitoring Features:
- **3 Monitoring Clients**: Real-time system health tracking
- **Performance Metrics**: CPU, memory, disk, and network monitoring
- **Log Analysis**: Web server access logs and application logs
- **Alert System**: Immediate notification of system issues

### Benefits:
- **Data Encryption**: HTTPS protects user data in transit
- **Access Control**: Firewalls prevent unauthorized access
- **Proactive Management**: Monitoring enables early problem detection

### Trade-offs:
- **SSL Termination**: Internal traffic between load balancer and servers is unencrypted
- **Resource Contention**: All components on same servers compete for resources
- **Complex Management**: More components require more maintenance

---

## 3. Scale Up

**File**: `3-scale_up`

Advanced infrastructure with component separation and load balancer clustering.

### Architecture:
- **2 Load Balancers**: Clustered HAProxy for high availability
- **Dedicated Web Server**: Nginx optimized for static content
- **Dedicated Application Server**: Separate server for dynamic processing
- **Dedicated Database Server**: MySQL on its own hardware

### Key Improvements:
- **Component Separation**: Each tier runs on dedicated hardware
- **Load Balancer Redundancy**: Eliminates load balancer SPOF
- **Independent Scaling**: Scale each tier based on specific needs
- **Resource Optimization**: Each server optimized for its specific role

### Benefits:
- **Performance**: No resource competition between components
- **Scalability**: Add capacity where needed without over-provisioning
- **Reliability**: Failure in one tier doesn't affect others
- **Maintainability**: Update/maintain components independently

---

## Evolution Summary

| Aspect | Task 0 | Task 1 | Task 2 | Task 3 |
|--------|--------|--------|--------|---------|
| Servers | 1 | 3 | 3 | 5+ |
| Redundancy | None | Web/App | Web/App | All tiers |
| Security | None | None | Full | Full |
| Monitoring | None | None | Full | Full |
| Scalability | Vertical only | Horizontal | Horizontal | Independent |
| Complexity | Low | Medium | High | Very High |

## Key Concepts Learned

- **Load Balancing**: Distribution algorithms (Round Robin, Least Connections)
- **Database Clustering**: Primary-Replica setups for read/write separation
- **Security Layers**: Firewalls, SSL/TLS, HTTPS implementation
- **Monitoring**: System metrics, log analysis, alerting
- **High Availability**: Redundancy, failover, clustering
- **Scalability**: Horizontal vs vertical scaling, component separation
