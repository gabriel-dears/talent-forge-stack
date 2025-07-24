# 🚀 Talent Forge Stack

**Talent Forge Stack** is a full-stack AI-powered recruitment platform designed to streamline the hiring process with intelligent matching, resume analysis, centralized monitoring, and automated notifications. This repository orchestrates all backend services, AI matcher logic, and observability tools using Docker Compose.

---

## 📆 Project Structure

```bash
talent_forge_stack/
├── .env                        # Environment configuration
├── docker-compose.yml         # Orchestrates all services
├── uploads/                   # Mounted directory for uploaded resumes
├── talent_forge/              # Spring Boot REST API for job/candidate management
├── talent_forge_ai_matcher/   # FastAPI AI Matching service
├── prometheus/                # Prometheus configuration
├── tempo/                     # Grafana Tempo configuration
```

---

## 🧩 Submodules

| Submodule                                                                             | Description                                                                       |
| ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| [`talent_forge`](https://github.com/gabriel-dears/talent_forge)                       | Spring Boot backend managing candidates, jobs, resumes, and Kafka events          |
| [`talent_forge_ai_matcher`](https://github.com/gabriel-dears/talent_forge_ai_matcher) | FastAPI service performing AI-powered profile matching and consuming Kafka events |

> ⚠️ Use `git clone --recurse-submodules` to automatically fetch all submodules.

---

## 🐳 Service Overview

| Service      | Description                         | Port        |
| ------------ | ----------------------------------- | ----------- |
| `postgres`   | PostgreSQL database                 | 5432        |
| `redis`      | In-memory cache and messaging       | 6379        |
| `zookeeper`  | Coordination for Kafka              | 2181        |
| `kafka`      | Kafka event broker                  | 9092        |
| `kafka-ui`   | Kafka monitoring UI                 | 8081        |
| `app`        | Spring Boot REST API                | 8080        |
| `ai-matcher` | AI-powered FastAPI matching service | 8000        |
| `prometheus` | Metrics collection                  | 9090        |
| `grafana`    | Dashboard for metrics and traces    | 3000        |
| `tempo`      | Distributed tracing backend         | 3200 / 4317 |

All services are connected through the shared Docker network `tf-net`.

---

## 📁 Upload Directory

The `./uploads/` directory is mounted into the Spring Boot container for handling resume uploads.

Example `.env` configuration:

```env
APP_UPLOAD_DIR=/app/uploads
```

---

## 🔐 Environment Variables

Create a `.env` file in the root directory with the following example values:

```env
POSTGRES_DB=talent_forge
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_password

SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/talent_forge
SPRING_JPA_HIBERNATE_DDL_AUTO=update
SPRING_JPA_SHOW_SQL=true
SERVER_PORT=8080
APP_UPLOAD_DIR=/app/uploads
SPRING_PROFILES_ACTIVE=dev
JAVA_OPTS=-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005

AI_MATCHER_URL=http://ai-matcher:8000
SPRING_API_URL=http://app:8080
APP_HEALTHCHECK_URL=http://localhost:8080/actuator/health
```

---

## ✅ Getting Started

### 1. Clone with Submodules

```bash
git clone --recurse-submodules https://github.com/your-username/talent_forge_stack.git
cd talent_forge_stack
```

If you forgot `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

### 2. Build & Start the Stack

```bash
docker-compose up --build
```

### 3. Access Services

| Service         | URL                                                                            |
| --------------- | ------------------------------------------------------------------------------ |
| Spring Boot API | [http://localhost:8080](http://localhost:8080)                                 |
| Swagger UI      | [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html) |
| AI Matcher API  | [http://localhost:8000/docs](http://localhost:8000/docs)                       |
| Kafka UI        | [http://localhost:8081](http://localhost:8081)                                 |
| Prometheus      | [http://localhost:9090](http://localhost:9090)                                 |
| Grafana         | [http://localhost:3000](http://localhost:3000)                                 |

Default Grafana login: `admin` / `admin`

---

## 🤔 Health Checks

Each container defines health checks:

* **PostgreSQL**: `pg_isready`
* **Spring Boot**: `/actuator/health`
* **AI Matcher**: `/health`
* **Kafka**: via `kafka-topics.sh`
* **Redis**: `redis-cli ping`

---

## 🔐 Authentication
Talent Forge now includes JWT-based authentication using Spring Security.
Each request to the protected endpoints must include a valid token in the Authorization header:

```
Authorization: Bearer <your_token>
The /auth/login endpoint issues the JWT after verifying user credentials.
```

---

## 📊 Observability

| Tool                  | Purpose                                   |
| --------------------- | ----------------------------------------- |
| **Prometheus**        | Collects metrics from the Spring Boot app |
| **Grafana**           | Visualizes metrics and traces             |
| **Tempo**             | Distributed tracing backend               |
| **Micrometer + OTEL** | Full tracing integration with Tempo       |

You can monitor HTTP calls, Kafka events, and execution time per request.

---

## 💡 Features

* ✅ Full CRUD for jobs and candidates
* ✅ Resume file upload and parsing
* ✅ AI-powered semantic matching (embeddings)
* ✅ Email and SMS notifications
* ✅ Event-driven communication (Kafka)
* ✅ Distributed tracing and metrics via OTEL, Tempo, Prometheus, Grafana
* ✅ Modular microservice architecture with Docker Compose

---

## 🚲 Planned Enhancements

* 🔐 OAuth2/JWT authentication
* 📈 Public UI in React or Angular
* ⚙️ API Gateway and Service Discovery
* 🧠 Integration with HuggingFace or AWS Comprehend for advanced NLP

---

Contributions and feedback are welcome!
