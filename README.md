
# Portabilidad Demo Project

This repository contains a demo version of a microservices-based system, intended as a sample for employers. It demonstrates a modular architecture with several independent services, a modern frontend, and robust security. Some features are stubbed or simplified for demonstration purposes only.

---

## 1. Project Structure

The project is organized as a set of independent services and tools:

- **addresses-service/**: Address management (Java/Spring Boot)
- **api-gateway/**: API routing and JWT validation (Java/Spring Cloud Gateway)
- **auth-service/**: Authentication and authorization (Java/Spring Boot)
- **bot-service/**: Chatbot and document services
- **frontend/**: User interface (React, Vite, TypeScript, Tailwind CSS)
- **payments-service/**: Payment processing (Node.js)
- **portabilities-service/**: Portability operations
- **products-service/**: Product management
- **scraper/**: Data scraping (Python/Flask)
- **service-registry/**: Service discovery (Eureka)
- **users-service/**: User management
- **docker-compose.yaml**: Orchestrates databases and Selenium Grid
- **start/stop scripts**: Shell scripts for service lifecycle

**Technologies:** Java 17+, Spring Boot, Node.js 18+, React, Python 3.10+, Docker, PostgreSQL, PGVector, Selenium

---

## 2. Frontend

The frontend is a complete, responsive web application built with Vite, React, TypeScript, and Tailwind CSS. It provides:

- **Login page** with JWT integration and form validation
- **Dashboard** with sidebar navigation for all microservices
- **Views for each service** (Users, Products, Portabilities, Payments, Addresses, Bot)
- **CRUD operations**: List, view, create, and manage records for each service
- **Error handling and loading states**

### How to Run the Frontend

1. From the project root, run:
	 ```bash
	 ./start-frontend.sh
	 ```
	 or manually:
	 ```bash
	 cd frontend
	 npm install
	 npm run dev
	 ```
2. Access the app at [http://localhost:5173](http://localhost:5173)

---

## 3. Service Management and Security

### Service Management

#### Requirements
- Java 17+, Maven 3.6+, Node.js 18+, Python 3.10+, Docker & Docker Compose

#### Main Scripts
- **Start all services:**
	```bash
	./start-services.sh
	```
- **Stop all services:**
	```bash
	./stop-services.sh
	```
- **Stop a single service:**
	```bash
	./stop-services.sh <service-name>
	```
- **Restart services (one, several, or all):**
	```bash
	./restart-service.sh <service-name>
	# To restart all services:
	./restart-service.sh all
	```
- **Check status:**
	```bash
	./status-services.sh
	```

#### Manual Compilation
- Java services: `./mvnw clean package -DskipTests`
- Payments: `npm install` in payments-service
- Scraper: Python venv + `pip install -r requirements.txt`

#### Logs
Each service writes logs to its `logs/` directory. Use `tail -f` to monitor.

#### Service URLs
- **Eureka:** http://localhost:8761
- **API Gateway:** http://localhost:8080
- **Users:** http://localhost:8081
- **Addresses:** http://localhost:8082
- **Bot:** http://localhost:8083
- **Products:** http://localhost:8084
- **Portabilities:** http://localhost:8085
- **Payments:** http://localhost:3000
- **Scraper:** http://localhost:5000

#### Databases
- **PostgreSQL:** localhost:5432 (user: postgres, password: 1234)
- **PGVector:** localhost:5433 (user: postgres, password: 1234)

#### Environment Variables
Set Stripe keys, database configs, and Eureka configs as needed.

#### Troubleshooting
- Check logs for errors
- Ensure Docker is running
- Use `lsof -i :<port>` for port conflicts

### Security Overview

#### Authentication & Authorization
- JWT-based authentication via `auth-service`
- API Gateway validates and relays tokens to microservices
- Role-based and permission-based access control using Spring Security

#### Token Management
- Access tokens (1h), refresh tokens (24h)
- Refresh tokens stored in DB, support for revocation and rotation

#### Security Features
- Stateless authentication (no server sessions)
- Account lockout after failed attempts
- Passwords hashed with BCrypt
- Audit logging for logins
- Token validation: signature, expiration, issuer

#### Best Practices (Production)
- Change default passwords
- Use strong, environment-based JWT secrets
- Enforce HTTPS
- Store tokens in httpOnly cookies
- Rate limit login endpoints
- Monitor and audit logs
- Restrict CORS origins

#### Testing Security
Use curl or Postman to test login, token refresh, and protected endpoints. See SECURITY.md for detailed examples.

---

**This is a demo version for employer review. For more details, see the documentation in each service folder.**
