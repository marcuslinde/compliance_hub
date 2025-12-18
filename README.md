# Compliance Hub

### Automated Violation Detection for Data Processing Agreements

![Status](https://img.shields.io/badge/Status-MVP_Complete-success) ![Stack](https://img.shields.io/badge/Stack-Java_Spring_Boot_|_PostgreSQL_|_Docker-blue)

> **Note:** This is a fork of a group project developed for **UNIwise**. This repository specifically highlights my contributions to the **Backend Architecture, Database Design, and DevOps infrastructure**.

## 📖 Project Overview
Managing GDPR compliance across multiple products and data processors is complex. **UNIwise**, a Danish EdTech company, faced a critical challenge: knowing instantly if adding a new Data Processor would violate existing Data Processing Agreements (DPAs).

**Compliance Hub** solves this by providing a real-time validation engine. It transitions the workflow from manual document inspection to algorithmic logic, linking DPAs and Data Processors to detect violations *before* they happen.

## 🏗 Architecture Note
This project follows a strict **3-Tier Architecture** to ensure Separation of Concerns:
* **Frontend:** React with TypeScript (Hosted on Vercel)
* **Backend:** Java Spring Boot REST API (Hosted on Scalingo)
* **Database:** PostgreSQL (Containerized for Dev, Scalingo for Prod)

*This repository contains only the Backend API logic.*

## 👨‍💻 My Key Contributions
While the frontend was a team effort, my primary responsibility was **architecting the Backend, Database, and Developer Experience (DX)**.

### 1. Backend Architecture (Java & Spring Boot)
I architected the backend service using a **Layered Architecture** (Controller-Service-Repository).
* **Strategy Pattern:** Implemented the Strategy Pattern to handle variable evaluation logic (e.g., checking location restrictions vs. notification periods). This allows new legal rules to be added without modifying core service logic.
* **REST API:** Built a robust API handling complex relationships between products, requirements, and safeguards.
* **Type Safety:** Moved the data access layer from raw JDBC to **Spring Data JPA/Hibernate** to ensure object-oriented consistency.

### 2. Advanced Database Design
I designed a highly normalized relational schema in **PostgreSQL**.
* **JPA Optimization:** Utilized best-practice mappings (`@OneToMany` with `orphanRemoval`) and **Lazy Fetching** to prevent N+1 query issues.
* **PostgreSQL Specifics:** Leveraged native PostgreSQL array types (`text[]`) via `@JdbcTypeCode` to efficiently store processing locations without unnecessary join tables.

### 3. DevOps & Testing Infrastructure
To ensure my team could develop fast without "it works on my machine" issues, I containerized our environment.
* **Dockerized Development:** Set up **PostgreSQL via Docker Compose**, allowing the team to spin up a production-like database locally with a single command.
* **Testing Strategy:**
    * **Unit Testing:** Configured **H2 in-memory database** for fast, isolated logic tests using JUnit 5 and Mockito.
    * **Integration Testing:** Automated API verification using **Postman** against the containerized database.
    * **CI Pipeline:** Configured GitHub Actions to block merges unless the test suite passes.

## ✨ MVP Features (Current Status)
* **Entity Management:** CRUD capabilities for Data Processors and DPAs.
* **Automated Violation Engine:** Automatically flags conflicts (e.g., a Processor hosting in "USA" vs. a DPA requiring "EU/EEA").
* **Violations Dashboard:** Consolidated view of all current conflicts.

## 🔜 Roadmap (Future Work)
While the MVP successfully handles the core compliance loop, the following features are planned for the next iteration:

* [ ] **Adding Actions to Violations-dahsboard:** Implementing actions specific to the contract can be viewed and resolved.
* [ ] **Soft Delete / Archiving:** Implementing an audit trail for deleted contracts to meet legal record-keeping requirements.
* [ ] **NLP Integration:** Automating the extraction of metadata (e.g., notice periods) from uploaded PDF contracts using Natural Language Processing.
* [ ] **Role-Based Access Control (RBAC):** Differentiating between "Read-Only" employees and "Admin" users for sensitive deletion tasks.

Nb: since this was a semester project further development will not happen.

## 🛠 Tech Stack (My Focus)

* **Language:** Java 17+
* **Framework:** Spring Boot 3 (Web, Data JPA, Security)
* **Database:** PostgreSQL (Production/Dev), H2 (Test)
* **Infrastructure:** Docker, Docker Compose
* **Tools:** Postman, GitHub Actions (CI), Maven

## 🚀 How to Run (Local API)

**Prerequisites:** Docker Desktop & Java 17 SDK.

Since this is a headless backend service, running it locally exposes the **REST API endpoints**, not a GUI.

1.  **Clone the repository**
    ```bash
    git clone [https://github.com/marcuslinde/compliance_hub.git](https://github.com/marcuslinde/compliance_hub.git)
    cd compliance_hub
    ```

2.  **Configure Environment Variables**
    The application `application.yaml` relies on environment variables for DB connections and Security.
    * Duplicate the `.env` file in the root directory.
    * Modify the configuration
    
    > **⚠️ Important:** Spring Boot does not automatically read root `.env` files. You must ensure your IDE (e.g., **IntelliJ with the "EnvFile" plugin**) is configured to load this file, or manually set these variables in your Run Configuration.

3.  **Start the Local Database**
    This spins up a clean PostgreSQL instance in a Docker container (credentials match the `.env` above).
    ```bash
    docker-compose up -d
    ```

4.  **Run the Server**
    ```bash
    ./mvnw spring-boot:run
    ```

5.  **Verify Connectivity**
    The server will start on port `8080`. You can verify it is running by checking the health/root endpoint via terminal:
    ```bash
    curl http://localhost:8080/api/v1/
    ```
    *(Note: Without the frontend running locally, you will interact purely with JSON responses via Postman).*

---

## 🔒 Privacy & Data Note
**Important:** As this system was built to handle sensitive GDPR contracts for a real company, **no production data is included in this repository**. The instructions above set up a **local development environment** with an empty database.

---

## 🎓 Academic Context
* **Institution:** Aalborg University, Copenhagen (AAU CPH)
* **Course:** Software Engineering (3rd Semester)
* **Focus:** Object-Oriented Programming, System Architecture, and Software Engineering methodologies.
