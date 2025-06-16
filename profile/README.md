<p><a target="_blank" href="https://app.eraser.io/workspace/AT46KPSmysGG3uqr7lVk" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

# 🛒 Hivespace Project
> [🇻🇳 Xem bản Tiếng Việt tại đây](./README.vi.md) 

## 📖 Table of Contents
1. [Overview](#overview) 
2. [Architecture](#architecture) 
3. [Project Structure](#project-structure) 
    - [Backend](#backend) 
    - [Frontend](#frontend) 
4. [DevOps](#devops) 
5. [Technology](#technology) 
6. [Popular Libraries & Design Patterns](#popular-libraries--design-patterns) 
7. [Inspiration & References](#inspiration--references) 
8. [Contributions & Feedback](#contributions--feedback) 

This project is a side side project designed to apply and deepen knowledge of software architecture, backend development, cloud services, and DevOps principles. It simulates a simplified but realistic e-commerce platform inspired by real-world systems like Shopee or TiktokShop.

Although e-commerce systems are common, they are far from trivial to build well. A robust e-commerce platform touches on many real-world concerns such as:

- User authentication and profile management
- Product catalog with search and filters
- Shopping cart and order placement

We chose e-commerce because it provides a rich learning ground that can evolve over time. Starting with the core buyer experience, I aim to incrementally extend the system to include more advanced domains like:

- **Seller Center** (multi-vendor marketplace features)
- **Logistics Management** (order tracking, delivery scheduling)
- **Customer Engagement** (reviews, ratings, recommendations)
- **Social Commerce** (user feeds, livestream shopping, chat support)


This long-term vision allows me to:

- Practice building modular, maintainable codebases
- Apply patterns like **Domain-Driven Design (DDD)** and **Clean Architecture**
- Transition from a monolithic structure to **microservices**
- Implement cloud-native practices using tools like **Docker**, **Kubernetes**, and **Azure services**
- Explore **scalability**, **observability**, **CI/CD**, and **system resilience** in production-like environments
Ultimately, this project is a **sandbox for continuous learning**, built around a real-world problem space with broad application across industries.

## Architecture
We combine multiple architectural patterns and styles, including:

- [Domain-Driven Design](../architecture/domain-driven-design.md) 
- Event-Driven Architecture
- Command Query Responsibility Segregation (CQRS)
### Why Start as a Monolith?
1. **Faster Development**: Build and test features quickly without worrying about service boundaries or communication layers.
2. **Simpler Infrastructure**: No need to set up Kubernetes, service discovery, API gateways, or distributed tracing right away.
3. **Easier Debugging**: Logs, stack traces, and errors are in one place, making it easier to understand what's happening.
4. **Lower Cognitive Overhead**: Focus more on the business logic and feature development rather than system design complexity.
Below is the architecture of current monolith backend application which include 3 layer and the details of each layer will be presented in [Backend](#backend).

### Transition to Microservices
Once stable, we will transition to microservices for:

- Modular deployment
- Independent scaling
- Better separation of concerns
#### Migration Stages:
1. **Service Extraction Plan:** Identify domain boundaries and extract services.
2. **Communication:** Implement REST, gRPC, or messaging protocols.
3. **Deployment:** Use Docker and Kubernetes for containerization and orchestration.
## Project Structure
### Frontend
The frontend is built using Vue.js and Vite for a fast and modern development experience.

### Backend
The backend project is hosted in [this repo](https://github.com/HiveSpaceTeam/hivespace.backend), include 3 projects corresponding to 3 layered:

1. **Application Layer:** Handles user interactions and orchestrates business logic.
2. **Domain Layer:** Contains core business logic and domain models.
3. **Infrastructure Layer:** Manages database, messaging, and external integrations.
### Backend microservices
## Migration Plan: Monolith ➜ Microservices
## DevOps
- **CI/CD:** Jenkins pipelines for automated builds and deployments.
- **Infrastructure:** Hosted on Azure (VMs, Storage, etc.).
- **Deployment Tools:** Docker and Kubernetes for containerization and orchestration.
- **Local Emulation:** Azurite for Azure services.
## Technology
| Layer | Technology |
| ----- | ----- |
| Frontend | Vue.js, Vite |
| Backend | .NET |
| Messaging | RabbitMQ |
| Database | PostgreSQL, SQL Server, Redis |
| Auth | IdentityServer (OIDC) |
| DevOps | Jenkins, Docker, Kubernetes |
| Cloud | Azure |
## Popular Libraries & Design Patterns
| Tool/Lib | Purpose | Why We Use It |
| ----- | ----- | ----- |
|  | Input validation (C#) | Clean, testable validation logic |
|  | OIDC client for Vue.js | Integration with IdentityServer |
|  | HTTP requests | Lightweight and widely supported |
|  | Object mapping in C# | Reduces boilerplate |
|  | CQRS and clean architecture in .NET | Promotes separation of concerns |
---

## Inspiration & References
- [Sairyss/domain-driven-hexagon](https://github.com/Sairyss/domain-driven-hexagon)  – Architecture inspiration
- [MichaelCade/90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps)  – Language toggle structure
- [Awesome .NET Microservices](https://github.com/thangchung/awesome-dotnet-core#microservices) 
## Contributions & Feedback
> Have suggestions or ideas? Feel free to open an issue or submit a PR.





<!--- Eraser file: https://app.eraser.io/workspace/AT46KPSmysGG3uqr7lVk --->