# Docker Spring 2026

A comprehensive hands-on containerization project exploring custom image builds, multi-container orchestration, caching, load balancing, and production Docker best practices.

---

##  Overview

This repository documents my practical journey learning Docker—moving from writing individual `Dockerfiles` to orchestrating a complete microservices architecture using **Docker Compose** and **Nginx**. 

The project uses example frontend and backend applications (cloned from an external repo, link placed at the bottom) to simulate a full production stack featuring in-memory caching, database management, traffic routing, and project tracking tools.

---

## 🏗 System Architecture

<!-- Insert your architecture diagram image below -->
![System Architecture Diagram](./architecture-diagram.png)

### Stack Components

| Component | Technology | Role / Purpose |
| :--- | :--- | :--- |
| **Frontend** | React | Client-facing web application |
| **Backend** | REST API | Core business logic & API handling |
| **Database** | PostgreSQL | Primary relational database |
| **DB Management** | Adminer | Lightweight database administration interface |
| **Caching Layer** | Redis | In-memory key-value store for caching API data |
| **Reverse Proxy / LB** | Nginx | Web server, entry point, and load balancer |
| **Project Mgmt** | Redmine | Integrated project management tool |

---

## Docker Best Practices Applied

When containerizing the frontend and backend applications, I focused on three main pillars of image creation:

### 1. Image Size Optimization
* **Minimal Base Images:** Selected slim/alpine base images to reduce attack surface and download size.
* **Leveraging `.dockerignore`:** Excluded unnecessary build context files (e.g., `node_modules`, logs, `.git`) to keep builds clean.
* **Temporary File Cleanup:** Removed cached package files immediately after installation steps.
* **Multi-Stage Builds:** Separated compilation environments from runtime environments to ensure production images contain only runtime necessities.

### 2. Build Speed & Layer Caching
* **Optimized Layer Ordering:** Arranged Dockerfile instructions from least-frequently changed to most-frequently changed.
* **Command Chaining:** Grouped `RUN` commands (using `&&`) to minimize total layer creation.
* **Dependency Isolation:** Copied package manifests and installed dependencies *before* copying application code, preventing unnecessary dependency re-installs on minor code changes.

### 3. Container Security & Quality
* **Single Responsibility:** Adhered to the "one process per container" principle.
* **Controlled Port Exposure:** Exposed only strictly necessary internal ports.
* **Version Pinning:** Used specific tags instead of `latest` for predictable, reproducible builds.
* **Instruction Hygiene:** Favored `COPY` over `ADD` except where automatic archive extraction was required.

---

## Key Milestones & Implementation

### 1. Single Container Operations & Storage
* Built standalone images using optimized `Dockerfiles`.
* Tested port mappings to the host, configured **Docker-managed volumes** for data persistence, and used **bind mounts** for live code editing.
* Debugged running containers interactively using `docker exec`.

### 2. Caching Integration with Redis
* Integrated an official Redis container from Docker Hub.
* Configured backend communication with Redis to handle key-value caching and reduce database read loads.

### 3. Orchestration with Docker Compose
* Consolidated multi-container operations into a single `docker-compose.yml` file, replacing complex, individual `docker run` commands.
* Handled volume persistence, custom network bridges, and service initialization order (`depends_on`).
* **Key Learning:** *Docker networking was one of the most rewarding concepts here—understanding how containers discover and communicate with each other isolated from the host machine.*

### 4. Scaling, Reverse Proxying & Load Balancing with Nginx
* Implemented Nginx as a reverse proxy and entry point to route incoming web traffic.
* Configured load balancing across scaled instances of the backend container.
* Locked down network exposure: **No container ports are directly exposed to the host machine except through the Nginx proxy.**
* Configured Nginx to serve static frontend assets efficiently.

---

## How to Run

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/docker-spring-2026.git](https://github.com/YOUR_USERNAME/docker-spring-2026.git)
   cd docker-spring-2026

MATERIALS I USED FOR THIS PROJECT CAN BE FOUND HERE -> https://github.com/docker-hy/material-applications/tree/main/example-backend
