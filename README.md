# Phone Number Portability System 📱

Complete microservices system to manage phone number portability, product sales, payment processing, and customer service with artificial intelligence.

**FOR SECURITY REASONS, THE COMPLETE PROJECT IS PRIVATE, BUT SELECTED SERVICES ARE SHOWCASED HERE AS PART OF MY PROFESSIONAL PORTFOLIO FOR JOB INTERVIEWS.**\
- **[BOT SERVICE LINK](https://github.com/Shadowmataj/bot-service)**
- **[PORTABILITIES SERVICE LINK](https://github.com/Shadowmataj/portability-portabilities-service)**
- **[USERS SERVICE LINK](https://github.com/Shadowmataj/portability-user-service)**
- **[ADDRESSES SERVICE LINK](https://github.com/Shadowmataj/portability-addresses-service)**
---

## 📑 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Microservices](#-microservices)
  - [Service Registry (Eureka)](#1-service-registry-eureka)
  - [API Gateway](#2-api-gateway)
  - [Auth Service](#3-auth-service)
  - [Users Service](#4-users-service)
  - [Products Service](#5-products-service)
  - [Portabilities Service](#6-portabilities-service)
  - [Addresses Service](#7-addresses-service)
  - [Payments Service](#8-payments-service)
  - [Bot Service](#9-bot-service)
  - [Scraper Service](#10-scraper-service)
  - [Frontend](#11-frontend)
- [Technology Stack](#-technology-stack)
- [Installation and Configuration](#-installation-and-configuration)
- [Utility Scripts](#-utility-scripts)
- [Security](#-security)

---

## 🎯 Overview

This project implements a **complete microservices ecosystem** to manage the end-to-end phone number portability process, from device compatibility verification to payment processing and shipment tracking.

### Main Features

✅ **Microservices Architecture** - Independent and scalable services  
✅ **Service Discovery** - Automatic registration with Eureka  
✅ **API Gateway** - Single entry point with JWT authentication  
✅ **Conversational AI** - Intelligent chatbot with OpenAI GPT-4o  
✅ **Payment Processing** - Stripe integration  
✅ **Automated Scraping** - IMEI and portability verification  
✅ **Shipment Management** - Integration with Envia (Mexico)  
✅ **Modern Frontend** - React Dashboard with Vite

---

## 🏗 Architecture

```
                    ┌─────────────────────┐
                    │     Frontend        │
                    │   React + Vite      │
                    │   Port: 5173        │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │    API Gateway      │
                    │   Spring Cloud      │
                    │   Port: 8765        │
                    │   (JWT Security)    │
                    └──────────┬──────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
┌───────▼────────┐   ┌─────────▼─────────┐   ┌──────▼───────┐
│  Auth Service  │   │  Users Service    │   │   Products   │
│  Port: 8080    │   │  Port: 8081       │   │  Service     │
└────────────────┘   └───────────────────┘   └──────────────┘
        │                      │                      │
        └──────────────────────┼──────────────────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
┌───────▼────────┐   ┌─────────▼─────────┐   ┌──────▼───────┐
│  Portabilities │   │   Addresses       │   │   Payments   │
│    Service     │   │   Service         │   │   Service    │
│                │   │   Port: 8084      │   │  Port: 3000  │
└────────────────┘   └───────────────────┘   └──────────────┘
        │                      │                      │
        └──────────────────────┼──────────────────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
┌───────▼────────┐   ┌─────────▼─────────┐   ┌──────▼───────┐
│  Bot Service   │   │ Scraper Service   │   │   Eureka     │
│  (AI Chat)     │   │  Flask + Selenium │   │   Server     │
│                │   │  Port: 5000       │   │ Port: 8761   │
└────────────────┘   └───────────────────┘   └──────────────┘
        │                      │
        ▼                      ▼
┌────────────────┐   ┌───────────────────┐
│  PostgreSQL    │   │  Selenium Grid    │
│  + pgvector    │   │  (Chrome nodes)   │
└────────────────┘   └───────────────────┘
```

**Communication Flow:**
1. The **Frontend** communicates only with the **API Gateway**
2. The **API Gateway** validates JWT tokens and routes requests
3. The **microservices** communicate with each other using **OpenFeign**
4. All services register with **Eureka** for automatic discovery
5. **PostgreSQL** databases are independent per service

---

## 🔧 Microservices

### 1. Service Registry (Eureka)

**Port:** `8761`  
**Technology:** Spring Cloud Netflix Eureka

#### Description
Service discovery server that enables automatic registration and load balancing between microservices.

#### Features
- ✅ Automatic service registration
- ✅ Periodic health checks
- ✅ Web dashboard for monitoring
- ✅ Client-side load balancing

#### Endpoints
- **Dashboard:** `http://localhost:8761`

---

### 2. API Gateway

**Port:** `8765`  
**Technology:** Spring Cloud Gateway, JWT  
**Language:** Java 25

#### Description
Single entry point for all microservices. Handles JWT authentication, dynamic routing, and CORS.

#### Features
- ✅ JWT authentication on all protected routes
- ✅ Service discovery-based routing
- ✅ Automatic load balancing with `lb://`
- ✅ Globally configured CORS
- ✅ Public routes for login and health checks

#### Main Routes
```yaml
/api/auth/**          → auth-service
/api/users/**         → users-service
/api/products/**      → products-service
/api/portabilities/** → portabilities-service
/api/addresses/**     → addresses-service
/api/payments/**      → payments-service
/api/bot/**           → bot-service
/api/scrape/**        → scraper-service
```

#### Configuration
```properties
JWT_SECRET=your-secret-key
EUREKA_SERVER_URL=http://localhost:8761/eureka
```

---

### 3. Auth Service

**Port:** `8080`  
**Technology:** Spring Boot, Spring Security, JWT  
**Database:** PostgreSQL  
**Language:** Java 25

#### Description
Authentication and authorization service with internal system user management (employees, administrators, analysts).

#### Features
- ✅ JWT authentication (access + refresh tokens)
- ✅ Granular roles and permissions system (RBAC)
- ✅ Login attempt auditing
- ✅ Automatic account lockout after failures
- ✅ System user CRUD management
- ✅ Advanced filtering with pagination
- ✅ Automatic logging with AOP

#### Predefined Roles
- **SUPER_ADMIN** - Full system access
- **ADMIN** - User and configuration management
- **ANALYST** - Data query and analysis
- **CUSTOMER_SERVICE** - Customer service

#### Main Endpoints
```http
POST   /api/auth/login          # Login
POST   /api/auth/refresh        # Refresh token
POST   /api/auth/logout         # Logout
GET    /api/users               # List users
POST   /api/users               # Create user
GET    /api/users/{id}          # Get user
PUT    /api/users/{id}          # Update user
DELETE /api/users/{id}          # Delete user
GET    /api/roles               # List roles
POST   /api/roles               # Create role
```

#### Database Structure
- `system_users` - System users
- `roles` - Available roles
- `permissions` - Granular permissions
- `user_roles` - Users ↔ roles relation
- `role_permissions` - Roles ↔ permissions relation
- `refresh_tokens` - Refresh tokens
- `login_audit` - Login auditing

---

### 4. Users Service

**Port:** `8081`  
**Technology:** Spring Boot, Spring Data JPA  
**Database:** PostgreSQL  
**Language:** Java 25

#### Description
Manages **customer** information (customers) who request portability or purchase products. NOT to be confused with system users (auth-service).

#### Features
- ✅ Customer registration and management
- ✅ Search by email, phone, or ID
- ✅ Advanced filtering with multiple criteria
- ✅ Pagination and sorting
- ✅ Data validation (email, phone)
- ✅ Automatic timestamps
- ✅ JPA specifications for dynamic queries

#### Main Endpoints
```http
POST /api/customer/register        # Register customer
GET  /api/customers/{id}           # Get by ID
POST /api/customers/by-email       # Search by email
POST /api/customers/by-phone       # Search by phone
POST /api/customers/filter         # Filter with pagination
```

#### Data Model
```json
{
  "id": 1,
  "firstName": "Juan",
  "lastName": "Pérez",
  "email": "juan.perez@example.com",
  "phoneNumber": "+525512345678",
  "createdAt": "2024-01-15T10:30:00",
  "updatedAt": "2024-01-15T10:30:00"
}
```

#### Documentation
- **Swagger UI:** `http://localhost:8081/swagger-ui.html`

---

### 5. Products Service

**Port:** `8082`  
**Technology:** Spring Boot, Spring Data JPA  
**Database:** PostgreSQL  
**Language:** Java 25

#### Description
Manages the product catalog (SIM cards, plans, phones) and phone companies.

#### Features
- ✅ Product and company CRUD
- ✅ Inventory management
- ✅ Product categorization
- ✅ Products ↔ companies relationship
- ✅ Prices and descriptions
- ✅ Advanced filtering and search
- ✅ Data validation

#### Main Endpoints
```http
GET  /api/products              # List products
POST /api/products              # Create product
GET  /api/products/{id}         # Get product
PUT  /api/products/{id}         # Update product
DELETE /api/products/{id}       # Delete product
GET  /api/companies             # List companies
POST /api/companies             # Create company
GET  /api/companies/{id}        # Get company
```

#### Product Model
```json
{
  "id": 1,
  "name": "SIM Card Bait",
  "description": "Prepaid SIM card",
  "price": 150.00,
  "category": "SIM_CARD",
  "stock": 100,
  "companyId": 1
}
```

---

### 6. Portabilities Service

**Port:** `8083`  
**Technology:** Spring Boot, Spring Data JPA, OpenFeign  
**Database:** PostgreSQL  
**Language:** Java 25

#### Description
Manages the complete phone number portability request process, coordinating with other services.

#### Features
- ✅ Portability request CRUD
- ✅ Process states (PENDING, IN_PROCESS, COMPLETED, CANCELLED)
- ✅ Integration with users-service for customers
- ✅ Integration with products-service for products
- ✅ Integration with scraper-service for validation
- ✅ Portability status tracking
- ✅ IMEI and NIP validation

#### Main Endpoints
```http
GET  /api/portabilities           # List requests
POST /api/portabilities           # Create request
GET  /api/portabilities/{id}      # Get request
PUT  /api/portabilities/{id}      # Update status
DELETE /api/portabilities/{id}    # Cancel request
```

#### Portability Model
```json
{
  "id": 1,
  "customerId": 123,
  "phoneNumber": "5512345678",
  "imei": "123456789012345",
  "nip": "12345",
  "icc": "8952020123456789",
  "status": "IN_PROCESS",
  "createdAt": "2024-01-15T10:30:00"
}
```

---

### 7. Addresses Service

**Port:** `8084`  
**Technology:** Spring Boot, Spring Data JPA, OpenFeign  
**Database:** PostgreSQL  
**Language:** Java 25

#### Description
Manages customer addresses and integration with **Envia** (Mexican shipping platform) for label generation and tracking.

#### Features
- ✅ Customer address CRUD
- ✅ Address types (SHIPPING, BILLING)
- ✅ Mexican address validation by postal code
- ✅ Envia API integration
- ✅ Shipping label generation
- ✅ Shipping quotes with multiple carriers
- ✅ Shipment tracking
- ✅ Guide cancellation
- ✅ Webhook for Envia updates
- ✅ Filter by type, city, state, postal code

#### Main Endpoints

**Address Management:**
```http
GET  /api/addresses               # List addresses
POST /api/addresses               # Create address
GET  /api/addresses/{id}          # Get address
PUT  /api/addresses/{id}          # Update address
DELETE /api/addresses/{id}        # Delete address
POST /api/addresses/filter        # Filter addresses
```

**Envia Integration:**
```http
POST /api/envia/validate-address  # Validate address by postal code
POST /api/envia/get-carriers      # Get quotes
POST /api/envia/ship              # Generate label
POST /api/envia/cancel/{id}       # Cancel guide
GET  /api/envia/track/{orderId}   # Track shipment
POST /api/envia/webhook           # Envia webhook
```

#### Address Model
```json
{
  "id": 1,
  "customerId": 123,
  "type": "SHIPPING",
  "street": "Av. Insurgentes Sur 123",
  "city": "Mexico City",
  "state": "CDMX",
  "postalCode": "03100",
  "country": "Mexico",
  "isDefault": true
}
```

#### Envia Configuration
```properties
envia.api.token=your_envia_token
envia.api.url-address-validation=https://queries.envia.com/address
envia.api.url-get-carriers=https://api.envia.com/ship/rate
envia.api.url-ship=https://api.envia.com/ship/generate
```

---

### 8. Payments Service

**Port:** `3000`  
**Technology:** Node.js, Express.js, Stripe API  
**Database:** PostgreSQL  
**Language:** JavaScript (ES Modules)

#### Description
Payment processing service integrated with **Stripe** for checkout sessions, payment links, and webhooks.

#### Features
- ✅ Complete Stripe API integration
- ✅ Reusable Payment Links generation
- ✅ Individual Checkout Sessions
- ✅ Webhook handling for automatic confirmation
- ✅ Complete payment history
- ✅ Payment-order linking
- ✅ Structured logging with Winston
- ✅ Eureka registration for service discovery
- ✅ Swagger/OpenAPI documentation

#### Architecture
Follows **Repository-Service-Controller** pattern:
- **Repositories** - Database access
- **Services** - Business logic and Stripe
- **Controllers** - HTTP route handling
- **Routes** - Endpoint definitions

#### Main Endpoints

**Payment Links:**
```http
POST /api/payment-links           # Create payment link
GET  /api/payment-links           # List payment links
GET  /api/payment-links/{id}      # Get payment link
```

**Checkout Sessions:**
```http
POST /api/checkout-sessions       # Create checkout session
GET  /api/checkout-sessions       # List sessions
GET  /api/checkout-sessions/{id}  # Get session
```

**Payments:**
```http
GET /api/payments                 # List payments
GET /api/payments/{id}            # Get payment
GET /api/payments/order/{orderId} # Get by order
```

**Webhooks:**
```http
POST /api/webhooks/stripe         # Stripe webhook
```

#### Payment Model
```json
{
  "id": 1,
  "orderId": "ORD-123",
  "amount": 1500.00,
  "currency": "MXN",
  "status": "succeeded",
  "stripePaymentId": "pi_xxx",
  "customerEmail": "cliente@example.com",
  "createdAt": "2024-01-15T10:30:00"
}
```

#### Configuration
```env
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
DB_HOST=localhost
DB_PORT=5432
DB_NAME=payments
EUREKA_HOST=localhost
EUREKA_PORT=8761
```

#### Documentation
- **Swagger UI:** `http://localhost:3000/api-docs`

---

### 9. Bot Service

**Port:** `8086`  
**Technology:** Spring Boot, Spring AI, OpenAI GPT-4o  
**Database:** PostgreSQL + pgvector  
**Language:** Java 25

#### Description
Intelligent chatbot with **conversational AI** to manage the complete portability process and SIM card sales through natural conversation.

#### Key Features
- 🤖 **GPT-4o** - Advanced OpenAI language model
- 🎭 **State Management** - State machine for conversational flow
- 💾 **Persistent Memory** - Complete history in PostgreSQL
- 🧠 **RAG (Retrieval-Augmented Generation)** - Vector Store with pgvector
- 🔧 **Function Calling Tools** - 5 specialized tool categories
- 📱 **WhatsApp Integration** - Intelligent message buffer
- 📊 **Context Management** - Automatically stores user data
- 🔐 **Security** - Log sanitization and encryption

#### Spring AI Architecture
```
User Message → Orchestrator → ChatClient → [GPT-4o + Tools + Memory + RAG]
                                             ↓
                                      Response Generation
                                             ↓
                              [State Management + Context Storage]
                                             ↓
                                        Response to User
```

#### Tools (Function Calling)

1. **Customer Tools** - Customer management
   - Register new customer
   - Search customer by email/phone
   - Update information

2. **Address Tools** - Address management
   - Create shipping address
   - Validate Mexican address
   - List customer addresses

3. **Order Tools** - Order management
   - Create portability order
   - Check order status
   - Update order

4. **Payment Tools** - Payment processing
   - Generate payment link (Stripe)
   - Check payment status
   - Create checkout session

5. **Scraper Tools** - External validation
   - Verify IMEI compatibility
   - Validate number portability
   - Check availability

#### Chatbot States
- `GREETING` - Initial greeting
- `COLLECTING_INFO` - Collecting customer data
- `VALIDATING` - Validating IMEI and portability
- `CONFIRMING_ORDER` - Order confirmation
- `PROCESSING_PAYMENT` - Processing payment
- `COMPLETED` - Process completed
- `ERROR` - Error handling

#### Main Endpoints
```http
POST /api/chat/message            # Send message to bot
GET  /api/conversations           # List conversations
GET  /api/conversations/{id}      # Get conversation
POST /api/conversations/clear     # Clear history
GET  /api/documents               # List vector documents
POST /api/documents               # Add document to RAG
```

#### Conversation Examples
```
User: "Hi, I want to port to Bait"
Bot: "Hello! I'd be happy to help you with your portability. Could you 
      please provide me with your full name?"

User: "Juan Pérez"
Bot: "Perfect Juan. What is your email address?"

User: "juan@example.com"
Bot: "Excellent. What number would you like to port?"

[Bot continues collecting: IMEI, NIP, ICC, address]
[Automatically validates with scraper-service]
[Generates order and payment link]
[Sends confirmation]
```

#### Configuration
```properties
# OpenAI
spring.ai.openai.api-key=sk-xxx
spring.ai.openai.chat.options.model=gpt-4o
spring.ai.openai.chat.options.temperature=0.7

# PostgreSQL + pgvector
spring.datasource.url=jdbc:postgresql://localhost:5432/bot_db
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Vector Store
spring.ai.vectorstore.pgvector.dimensions=1536
spring.ai.vectorstore.pgvector.distance-type=COSINE_DISTANCE
```

#### Advanced Capabilities
- ✅ Maintains context between messages
- ✅ Retrieves information from previous conversations
- ✅ Performs operations across multiple services
- ✅ Handles errors gracefully
- ✅ Suggests alternatives when there are problems
- ✅ Generates conversation summaries

---

### 10. Scraper Service

**Port:** `5000`  
**Technology:** Flask, Selenium, Selenium Grid  
**Language:** Python 3.8+

#### Description
**Automated web scraping** service to verify device compatibility (IMEI) and validate phone number portability on carrier websites.

#### Features
- ✅ IMEI compatibility verification with Bait network
- ✅ Portability form automation
- ✅ Selenium Grid with load distribution
- ✅ Multiple Chrome nodes in parallel
- ✅ Eureka registration for service discovery
- ✅ RESTful API with Flask-Smorest
- ✅ OpenAPI/Swagger documentation
- ✅ Health checks and monitoring
- ✅ Robust error handling
- ✅ CORS enabled

#### Architecture
```
Flask App → Selenium Hub → Chrome Node 1
                         → Chrome Node 2
                         → Chrome Node 3
```

#### Technology Stack
- **Flask 3.1.2** - Web framework
- **Selenium 4.39.0** - Browser automation
- **Flask-Smorest** - REST API with OpenAPI
- **py-eureka-client** - Service discovery
- **Marshmallow** - Schema validation
- **Gunicorn** - WSGI server

#### Main Endpoints

**IMEI Verification:**
```http
POST /api/scrape-compatibility
Content-Type: application/json

{
  "imei": "123456789012345"
}

Response:
{
  "compatibility": true,
  "message": "Scraping completed successfully."
}
```

**Portability:**
```http
POST /api/scrape-portability
Content-Type: application/json

{
  "phone_number": "52XXXXXXXXXX",
  "imei": "123456789012345",
  "portability_nip": "12345",
  "icc": "8952020123456789",
  "first_name": "Juan",
  "last_name": "Pérez",
  "email": "juan@example.com"
}

Response:
{
  "portability_status": true,
  "message": "Portability scraping completed successfully."
}
```

**Health Check:**
```http
GET /api/health

Response:
{
  "status": "UP",
  "service": "scraper-service",
  "message": "Service is running"
}
```

#### Docker Compose Configuration
```yaml
services:
  selenium-hub:
    image: selenium/hub:latest
    ports:
      - "4444:4444"
  
  chrome-1:
    image: selenium/node-chrome:latest
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
  
  scraper-service:
    build: .
    ports:
      - "5000:5000"
    environment:
      - SELENIUM_HUB_URL=http://selenium-hub:4444/wd/hub
```

#### Environment Variables
```env
SELENIUM_HUB_URL=http://selenium-hub:4444/wd/hub
BAIT_URL=https://example-bait-url.com
SERVICE_NAME=scraper-service
SERVICE_PORT=5000
EUREKA_SERVER_URL=http://localhost:8761/eureka
```

#### Documentation
- **Swagger UI:** `http://localhost:5000/swagger-ui`

---

### 11. Frontend

**Port:** `5173`  
**Technology:** React 18, Vite, Tailwind CSS

#### Description
Modern and responsive web dashboard to manage all microservices in the portability system.

#### Features
- ✅ JWT authentication (access + refresh tokens)
- ✅ Protected routes with React Router
- ✅ Sidebar with navigation to all services
- ✅ Responsive (desktop, tablet, mobile)
- ✅ Axios interceptors for automatic tokens
- ✅ Context API for state management
- ✅ Forms with validation
- ✅ Centralized error handling
- ✅ Loading states on all buttons
- ✅ Modern design with Tailwind CSS

#### Technology Stack
- **Vite** - Ultra-fast build tool
- **React 18** - UI framework
- **Tailwind CSS** - Utility-first CSS
- **React Router** - SPA navigation
- **Axios** - HTTP client
- **Lucide React** - Modern icons

#### View Structure
```
Dashboard
├── Home View          → System status overview
├── Auth Service       → Login, users, roles
├── Users Service      → Customer management
├── Products Service   → Products and companies
├── Portabilities      → Portability requests
├── Addresses Service  → Addresses and shipments
├── Payments Service   → Payments and transactions
└── Bot Service        → Chat and AI
```

#### Main Components
- **Login.jsx** - Authentication page
- **Dashboard.jsx** - Main layout with sidebar
- **Header.jsx** - Top bar with user info
- **Sidebar.jsx** - Navigation side menu
- **ProtectedRoute.jsx** - HOC for protected routes
- **AuthContext.jsx** - Context for authentication state

#### Routes
```
/login                    # Login page
/dashboard                # Main dashboard
/dashboard/home           # Home view
/dashboard/auth           # Auth Service
/dashboard/users          # Users Service
/dashboard/products       # Products Service
/dashboard/portabilities  # Portabilities Service
/dashboard/addresses      # Addresses Service
/dashboard/payments       # Payments Service
/dashboard/bot            # Bot Service
```

#### API Configuration
```
// src/services/api.ts
const API_BASE_URL = 'http://localhost:8765/api';

// Interceptor to add JWT
api.interceptors.request.use(config => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

#### Available Scripts
```bash
npm run dev       # Development (http://localhost:5173)
npm run build     # Production build
npm run preview   # Build preview
npm run lint      # Linting with ESLint
```

#### Installation
```bash
cd frontend
npm install
npm run dev
```

---

## 💻 Technology Stack

### Backend (Java Services)
| Technology | Version | Use |
|------------|---------|-----|
| **Java** | 25 | Main language |
| **Spring Boot** | 4.0.1 - 4.0.2 | Backend framework |
| **Spring Cloud** | 2025.1.0 | Microservices |
| **Spring Security** | Latest | Authentication/Authorization |
| **Spring Data JPA** | Latest | Persistence |
| **PostgreSQL** | 13+ | Database |
| **JWT** | 0.12.6 | Authentication tokens |
| **OpenFeign** | Latest | HTTP client between services |
| **Eureka** | Latest | Service discovery |
| **Lombok** | Latest | Boilerplate reduction |
| **SpringDoc OpenAPI** | 2.3.0 - 2.8.4 | API documentation |

### Backend (Node.js Services)
| Technology | Version | Service |
|------------|---------|----------|
| **Node.js** | 16+ | Payments Service |
| **Express.js** | 4.x | Web framework |
| **Stripe** | Latest | Payment processing |
| **PostgreSQL** | 13+ | Database |
| **Winston** | Latest | Logging |
| **Swagger** | Latest | API documentation |

### Backend (Python Services)
| Technology | Version | Service |
|------------|---------|----------|
| **Python** | 3.8+ | Scraper Service |
| **Flask** | 3.1.2 | Web framework |
| **Selenium** | 4.39.0 | Web scraping |
| **Flask-Smorest** | Latest | REST API |
| **py-eureka-client** | 0.13.2 | Service discovery |

### Frontend
| Technology | Version | Use |
|------------|---------|-----|
| **React** | 18 | UI framework |
| **Vite** | Latest | Build tool |
| **Tailwind CSS** | Latest | Styles |
| **React Router** | 6 | Navigation |
| **Axios** | Latest | HTTP client |

### Database
| Technology | Use |
|------------|-----|
| **PostgreSQL 13+** | Main database |
| **pgvector** | Vector storage (Bot Service) |
| **HikariCP** | Connection pooling |

### DevOps & Tools
| Technology | Use |
|------------|-----|
| **Docker** | Containerization |
| **Docker Compose** | Local orchestration |
| **Maven** | Build tool (Java) |
| **npm** | Package manager (Node/React) |
| **Git** | Version control |

---

## 🚀 Installation and Configuration

### Global Prerequisites
```bash
# Java 25
java --version

# Node.js 16+
node --version

# Python 3.8+
python --version

# PostgreSQL 13+
psql --version

# Docker & Docker Compose
docker --version
docker-compose --version

# Maven 3.8+
mvn --version
```

### 1. Clone the Repository
```bash
git clone <repository-url>
cd portabilidad
```

### 2. Configure Databases

Create the necessary databases:
```sql
CREATE DATABASE service_registry_db;
CREATE DATABASE auth_db;
CREATE DATABASE users_db;
CREATE DATABASE products_db;
CREATE DATABASE portabilities_db;
CREATE DATABASE addresses_db;
CREATE DATABASE payments;
CREATE DATABASE bot_db;

-- For bot-service, enable pgvector
\c bot_db
CREATE EXTENSION IF NOT EXISTS vector;
CREATE EXTENSION IF NOT EXISTS hstore;
```

### 3. Environment Variables

Create `.env` files in each service:

**auth-service, users-service, etc. (Java services):**
```properties
# PostgreSQL
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/<db_name>
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=your_password

# Eureka
EUREKA_CLIENT_SERVICE_URL_DEFAULTZONE=http://localhost:8761/eureka

# JWT (auth-service and api-gateway only)
JWT_SECRET=your-super-secret-key-min-512-bits
```

**payments-service (Node.js):**
```env
PORT=3000
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
DB_HOST=localhost
DB_PORT=5432
DB_NAME=payments
DB_USER=postgres
DB_PASSWORD=your_password
EUREKA_HOST=localhost
EUREKA_PORT=8761
```

**scraper-service (Python):**
```env
SELENIUM_HUB_URL=http://selenium-hub:4444/wd/hub
BAIT_URL=https://your-bait-url.com
SERVICE_NAME=scraper-service
SERVICE_PORT=5000
EUREKA_SERVER_URL=http://localhost:8761/eureka
```

**bot-service:**
```properties
spring.ai.openai.api-key=sk-xxx
spring.ai.openai.chat.options.model=gpt-4o
```

### 4. Service Startup Order

**IMPORTANT:** Services must be started in this order:

#### Step 1: Service Registry (Eureka)
```bash
cd service-registry
./mvnw spring-boot:run
# Wait for it to start at http://localhost:8761
```

#### Step 2: API Gateway
```bash
cd api-gateway
./mvnw spring-boot:run
# Wait for it to register with Eureka
```

#### Step 3: Core Services (in parallel)
```bash
# Terminal 1
cd auth-service
./mvnw spring-boot:run

# Terminal 2
cd users-service
./mvnw spring-boot:run

# Terminal 3
cd products-service
./mvnw spring-boot:run

# Terminal 4
cd portabilities-service
./mvnw spring-boot:run

# Terminal 5
cd addresses-service
./mvnw spring-boot:run
```

#### Step 4: Payments Service (Node.js)
```bash
cd payments-service
npm install
npm start
```

#### Step 5: Scraper Service (Python + Docker)
```bash
cd scraper
docker-compose up -d    # Start Selenium Grid
pip install -r requirements.txt
flask run --host=0.0.0.0 --port=5000
```

#### Step 6: Bot Service
```bash
cd bot-service
./mvnw spring-boot:run
```

#### Step 7: Frontend
```bash
cd frontend
npm install
npm run dev
# Access http://localhost:5173
```

### 5. Verification

Verify that all services are registered with Eureka:
```
http://localhost:8761
```

You should see:
- API-GATEWAY
- AUTH-SERVICE
- USERS-SERVICE
- PRODUCTS-SERVICE
- PORTABILITIES-SERVICE
- ADDRESSES-SERVICE
- STRIPE-SERVICE (payments)
- SCRAPER-SERVICE
- BOT-SERVICE

### 6. Access the System

**Frontend:**
```
http://localhost:5173
```

**API Gateway:**
```
http://localhost:8765/api/
```

**Swagger Documentation:**
- Auth: `http://localhost:8080/swagger-ui.html`
- Users: `http://localhost:8081/swagger-ui.html`
- Products: `http://localhost:8082/swagger-ui.html`
- Addresses: `http://localhost:8084/swagger-ui.html`
- Payments: `http://localhost:3000/api-docs`
- Scraper: `http://localhost:5000/swagger-ui`
- Bot: `http://localhost:8086/swagger-ui.html`

---

## 🛠 Utility Scripts

The project includes bash scripts to facilitate service management:

### start-services.sh
Starts all Java services (Spring Boot) in the background.
```bash
./start-services.sh
```

### stop-services.sh
Stops all Java services.
```bash
./stop-services.sh
```

### status-services.sh
Shows the status of all services.
```bash
./status-services.sh
```

### restart-service.sh
Restarts a specific service.
```bash
./restart-service.sh auth-service
```

### start-frontend.sh
Starts the frontend in development mode.
```bash
./start-frontend.sh
```

---

## 🔒 Security

### Authentication and Authorization
- **JWT Tokens** - Access tokens (15min) + Refresh tokens (7 days)
- **RBAC** - Role-based access control
- **API Gateway** - Single entry point with JWT validation
- **Password Hashing** - BCrypt for passwords
- **Account Lockout** - Lockout after failed attempts

### Data Protection
- **HTTPS** - Recommended for production
- **CORS** - Globally configured in API Gateway
- **Input Validation** - Validation on all endpoints
- **SQL Injection** - Protection with JPA/Hibernate
- **XSS Protection** - Input sanitization

### Secrets Management
- **Environment Variables** - Don't hardcode secrets
- **Stripe Webhook Signature** - Webhook validation
- **JWT Secret** - Minimum 512 bits
- **Database Credentials** - Environment variables

### Logging and Auditing
- **Login Audit** - Log all login attempts
- **AOP Logging** - Automatic logging in services
- **Winston Logs** - Structured logs (payments-service)
- **Error Tracking** - Centralized exception handling

---

## 📊 Service Ports

| Service | Port | URL |
|----------|--------|-----|
| **Frontend** | 5173 | http://localhost:5173 |
| **API Gateway** | 8765 | http://localhost:8765 |
| **Service Registry** | 8761 | http://localhost:8761 |
| **Auth Service** | 8080 | http://localhost:8080 |
| **Users Service** | 8081 | http://localhost:8081 |
| **Products Service** | 8082 | http://localhost:8082 |
| **Portabilities Service** | 8083 | http://localhost:8083 |
| **Addresses Service** | 8084 | http://localhost:8084 |
| **Bot Service** | 8086 | http://localhost:8086 |
| **Payments Service** | 3000 | http://localhost:3000 |
| **Scraper Service** | 5000 | http://localhost:5000 |
| **Selenium Hub** | 4444 | http://localhost:4444 |
| **PostgreSQL** | 5432 | localhost:5432 |

---

### Upcoming Improvements
- [ ] Unit and integration tests

**Version:** 1.0.0  
**Last updated:** February 2026  
**License:** Private
