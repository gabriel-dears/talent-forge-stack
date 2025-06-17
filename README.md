# 🚀 Talent Forge Stack

Talent Forge Stack is a full-stack AI-driven recruitment platform designed to streamline the hiring process through candidate/job matching, resume parsing, analytics, and notifications. This repository acts as the root orchestrator for the system, bringing together backend services, AI matcher logic, and infrastructure via Docker Compose.

## 📦 Project Structure

```bash
talent_forge_stack/
├── .env                        # Environment configuration
├── docker-compose.yml         # Orchestration for all services
├── uploads/                   # Mounted directory for uploaded resumes
├── talent_forge/              # Spring Boot REST API for job/candidate management
├── talent_forge_ai_matcher/   # Python FastAPI AI Matching service
```

## 🧩 Submodules

This project integrates two main submodules:

talent_forge – Java Spring Boot backend for managing candidates, jobs, resumes, and notifications.

talent_forge_ai_matcher – Python-based microservice that performs AI-powered candidate/job matching.

## 🐳 Services Overview

Service	Description	Port
postgres	PostgreSQL database for persistent storage	5432
app	Spring Boot backend application (talent_forge)	8080
ai-matcher	Python FastAPI service for candidate-job matching (ai_matcher)	8000

All services are connected via a shared Docker network tf-net.

## 📁 Upload Directory

./uploads/ is mounted into the Spring Boot container to handle resume file uploads. It is configurable through the .env variable APP_UPLOAD_DIR.

## 🔐 Environment Variables (.env)

Create a .env file in the root directory with the following values (example):

```bash
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

Be sure to adjust credentials and URLs as needed.

## ✅ How to Run

### 1. Clone with Submodules

```bash
git clone --recurse-submodules https://github.com/your-username/talent_forge_stack.git
cd talent_forge_stack
If you forgot --recurse-submodules:

bash
Copy
Edit
git submodule update --init --recursive
```

### 2. Build & Start

```bash
docker-compose up --build
```

### 3. Access Services

Spring Boot API: http://localhost:8080

Swagger UI: http://localhost:8080/swagger-ui.html

AI Matcher API: http://localhost:8000/docs

## 🧪 Health Checks

Each service includes Docker health checks:

PostgreSQL: pg_isready

Spring Boot: /actuator/health

AI Matcher: Uvicorn self-monitoring

## 🧠 Features (High-Level)

✅ CRUD for jobs and candidates

✅ Resume file upload + optional parsing

✅ AI-powered match scoring

✅ Email/SMS notification system

✅ Observability via Spring Boot Actuator

✅ Modular microservice design

## 🛠️ Future Enhancements

Add service discovery or API Gateway

Integrate external NLP APIs (e.g., AWS Comprehend)

Add authentication (OAuth2/JWT)

Connect Prometheus + Grafana for monitoring