# 🚀 Hivespace Project

> A side project to learn and apply acquired knowledge in architecture, design, cloud, and DevOps principles, including microservices, domain-driven design, and Azure. We plan to built an e-commerce system, designed for scalability and adaptability to address key challenges in the e-commerce space.  
> [🇻🇳 Xem bản Tiếng Việt tại đây](./README.vi.md)

---

## 📖 Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Project Structure](#project-structure)
    - [Backend](#backend)
    - [Frontend](#frontend)
4. [Migration Plan: Monolith ➜ Microservices](#migration-plan)
5. [DevOps](#devops)
6. [Technology](#technology)
7. [Popular Libraries & Design Patterns](#popular-libraries--design-patterns)
8. [Inspiration & References](#inspiration--references)


## 📘 Overview

Describe the purpose of the project. What problem does it solve?  
Who is the target user?  
How is it deployed or used?


## 🏗️ Architecture
We attempt to learn and combine multiple architectural patterns and styles together, such as:
- Domain driven design
- Event driven architecture
- Command Query Responsibility Segregation (CQRS)

As this is a side project, starting with a monolith makes development simpler and faster. It helps validate core business logic and domain boundaries before the complexity of microservices, Key reasons:
- Faster iteration: Single codebase means less setup and faster prototyping.
- Easier debugging: Everything is in one place — no need to manage distributed logging or tracing.
- Domain clarity: Helps you better understand and model business domains before splitting them.
- Avoid premature complexity: Microservices introduce networking, data consistency, and deployment challenges — overkill for an early stage or solo project.

Once stable, we can confidently split into microservices using clear, tested domain boundaries.

We're transitioning to a microservices-based architecture to enable modular deployment and independent scaling with better separation of concern.


Migration stages:
1. Service extraction plan
2. Communication (e.g., REST, gRPC, messaging)
3. Deployment considerations (e.g., Docker, Kubernetes)


## 🧱 Project Structure

### 📦 Backend

- Frameworks: (.NET, Go, Python, etc.)
- Architecture style: (Layered, Hexagonal, DDD, CQRS)
- Key components:
  - User Service
  - Product Catalog
  - Order, Payment, etc.
- Storage: PostgreSQL, MongoDB, Redis
- Auth: IdentityServer (custom UI integration)

### 🖼️ Frontend

- Framework: Vue.js + Vite
- Authentication integration (OIDC with IdentityServer)
- Shared UI components & feature flags


## 🛠️ Migration Plan: Monolith ➜ Microservices


## 🔧 DevOps

- CI/CD with **Jenkins**
- Infrastructure on **Azure** (VMs, Storage, etc.)
- Deployment tools: Docker, Kubernetes (planned/used)
- Local emulation: Azurite


## 📚 Technology

| Layer        | Technology                    |
|--------------|-------------------------------|
| Frontend     | Vue.js, Vite                  |
| Backend      | .NET                          |
| Messaging    | RabbitMQ                      |
| Database     | PostgreSQL, SQL server, Redis |
| Auth         | IdentityServer (OIDC)         |
| DevOps       | Jenkins, Docker, Kubernetes   |
| Cloud        | Azure                         |


## 🔍 Popular Libraries & Design Patterns

| Tool/Lib               | Purpose                               | Why We Use It                            |
|------------------------|----------------------------------------|------------------------------------------|
| `FluentValidation`     | Input validation (C#)                  | Clean, testable validation logic         |
| `oidc-client-ts`       | OIDC client for Vue.js                 | Integration with IdentityServer          |
| `axios`                | HTTP requests                          | Lightweight and widely supported         |
| `AutoMapper`           | Object mapping in C#                   | Reduces boilerplate                      |
| `MediatR`              | CQRS and clean architecture in .NET   | Promotes separation of concerns          |

(You can extend with diagrams like [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon))


## 🧭 Inspiration & References

- [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon) – Architecture inspiration
- [MichaelCade/90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps) – Language toggle structure
- [Awesome .NET Microservices](https://github.com/thangchung/awesome-dotnet-core#microservices)

## 🗣️ Contributions & Feedback

> Have suggestions or ideas? Feel free to open an issue or submit a PR.

