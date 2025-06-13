# 🚀 Hivespace Project

> A brief, one-liner description of your app's purpose or domain.  
> [🇻🇳 Xem bản Tiếng Việt tại đây](./README.vi.md)

---

## 📖 Table of Contents

1. [Overview](#overview)
2. [Current Architecture](#current-architecture)
3. [Migration Plan: Monolith ➜ Microservices](#migration-plan-monolith--microservices)
4. [Project Structure](#project-structure)
    - [Backend](#backend)
    - [Frontend](#frontend)
5. [DevOps](#devops)
6. [Tech Stack](#tech-stack)
7. [Popular Libraries & Design Patterns](#popular-libraries--design-patterns)
8. [Inspiration & References](#inspiration--references)

---

## 📘 Overview

Describe the purpose of the project. What problem does it solve?  
Who is the target user?  
How is it deployed or used?

---

## 🏗️ Current Architecture

Currently implemented as a **monolithic application**.

- Technologies used in the monolith
- Basic diagram (optional)
- Challenges faced with current structure

---

## 🛠️ Migration Plan: Monolith ➜ Microservices

We aim to transition to a **microservice architecture**.  
Here’s why:

- Scalability
- Independent deployment
- Better separation of concerns

Migration stages:

1. Service extraction plan
2. Communication (e.g., REST, gRPC, messaging)
3. Deployment considerations (e.g., Docker, Kubernetes)

---

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

---

## 🔧 DevOps

- CI/CD with **Jenkins**
- Infrastructure on **Azure** (VMs, Storage, etc.)
- Deployment tools: Docker, Kubernetes (planned/used)
- Local emulation: Azurite

---

## 📚 Tech Stack

| Layer        | Technology                    |
|--------------|-------------------------------|
| Frontend     | Vue.js, Vite                  |
| Backend      | .NET                          |
| Messaging    | RabbitMQ                      |
| Database     | PostgreSQL, SQL server, Redis |
| Auth         | IdentityServer (OIDC)         |
| DevOps       | Jenkins, Docker, Kubernetes   |
| Cloud        | Azure                         |

---

## 🔍 Popular Libraries & Design Patterns

| Tool/Lib               | Purpose                               | Why We Use It                            |
|------------------------|----------------------------------------|------------------------------------------|
| `FluentValidation`     | Input validation (C#)                  | Clean, testable validation logic         |
| `oidc-client-ts`       | OIDC client for Vue.js                 | Integration with IdentityServer          |
| `axios`                | HTTP requests                          | Lightweight and widely supported         |
| `AutoMapper`           | Object mapping in C#                   | Reduces boilerplate                      |
| `MediatR`              | CQRS and clean architecture in .NET   | Promotes separation of concerns          |

(You can extend with diagrams like [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon))

---

## 🧭 Inspiration & References

- [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon) – Architecture inspiration
- [MichaelCade/90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps) – Language toggle structure
- [Awesome .NET Microservices](https://github.com/thangchung/awesome-dotnet-core#microservices)

---

## 🗣️ Contributions & Feedback

> Have suggestions or ideas? Feel free to open an issue or submit a PR.

