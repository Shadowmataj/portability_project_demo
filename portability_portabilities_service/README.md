# Portabilities Service

**[LINK TO THE PROJECT](https://github.com/Shadowmataj/portability-portabilities-service)**

REST API microservice for managing phone number portabilities and orders, built with Spring Boot and designed to work in a microservices ecosystem with Stripe integration for payments.

## 📋 Description

This service is part of a microservices architecture and provides complete functionality for:
- Phone portability lifecycle management
- Order administration related to products, SIM cards, addresses, and payments
- Integration with external services via OpenFeign (Stripe Service)
- Registration with Eureka server for service discovery
- Data validation (IMEI, NIP, phone numbers)

## ✨ Key Features

- **Portabilities CRUD**: Create, read, update, and delete portability requests
- **Order Management**: Complete order control with references to customers, products, SIM cards, addresses, and payments
- **Advanced Search**: Dynamic filters and pagination for portabilities and orders
- **Validations**: Business rules for phone numbers, IMEI (15 digits), NIP (4 digits)
- **Interactive Documentation**: API documented with OpenAPI 3.0 (Swagger UI)
- **Microservices**: Integration with Eureka for service discovery
- **Persistence**: PostgreSQL database with JPA/Hibernate
- **Logging**: AOP aspects for automatic method logging
- **Exception Handling**: Centralized error control with appropriate HTTP responses

## 🛠️ Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 25 | Programming language |
| Spring Boot | 4.0.1 | Main framework |
| Spring Cloud | 2025.1.0 | Microservices ecosystem |
| Spring Data JPA | - | Data persistence |
| PostgreSQL | - | Relational database |
| Lombok | - | Boilerplate code reduction |
| SpringDoc OpenAPI | 2.7.0 | API documentation |
| Netflix Eureka Client | - | Service discovery |
| OpenFeign | - | Declarative HTTP client |
| Spring AOP | 4.0.0-M2 | Aspect-oriented programming |
| Maven | - | Dependency management |

## 📦 Prerequisites

- Java Development Kit (JDK) 25
- Maven 3.x
- PostgreSQL 12+
- Running Eureka Server (for service discovery)
- Stripe Service (optional, for payment features)

## 🚀 Installation and Configuration

### 1. Clone the Repository

```bash
git clone <repository-url>
cd portabilities-service
```

### 2. Configure Environment Variables (Optional)

Edit `src/main/resources/application.properties`:

```properties
# Service name
spring.application.name=portabilities-service

# Server port
server.port=8082

# Database configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/portability_db
spring.datasource.username=postgres
spring.datasource.password=1234

# JPA/Hibernate
spring.jpa.hibernate.ddl-auto=update
```

### 3. Build the Project

```bash
mvn clean install
```

### 4. Run the Application

```bash
mvn spring-boot:run
```

Or run the compiled JAR:

```bash
java -jar target/portabilities-service-0.0.1-SNAPSHOT.jar
```

The application will be available at: `http://localhost:8082`

## 📁 Project Structure

```
src/main/java/com/portability/portabilities_service/
├── PortabilitiesServiceApplication.java    # Main class
├── aspect/
│   └── LoggingAspect.java                  # AOP Logging
├── config/
│   ├── AppConfig.java                       # General configuration
│   ├── OpenApiConfig.java                   # Swagger configuration
│   └── WebConfig.java                       # Web configuration
├── controller/
│   ├── OrderController.java                 # Order endpoints
│   └── PortabilitiesController.java         # Portability endpoints
├── exception/
│   ├── DuplicatePhoneNumberException.java   # Duplicate number exception
│   ├── GlobalExceptionHandler.java          # Global error handler
│   ├── OrderNotFoundException.java          # Order not found exception
│   └── PortabilityNotFoundException.java    # Portability not found exception
├── feign/
│   └── StripeInterface.java                 # Feign client for Stripe
├── model/
│   ├── Order.java                           # Order entity
│   ├── Portability.java                     # Portability entity
│   ├── dto/                                 # Data Transfer Objects
│   │   ├── ByPhoneNumberRequest.java
│   │   ├── Imei.java
│   │   ├── OrderFilterRequest.java
│   │   ├── OrderRequest.java
│   │   ├── OrderResponse.java
│   │   ├── PagedResponse.java
│   │   ├── PortabilityFilterRequest.java
│   │   ├── PortabilityNip.java
│   │   ├── PortabilityRequest.java
│   │   └── PortabilityResponse.java
│   └── enm/                                 # Enumerations
├── repo/
│   ├── OrderRepo.java                       # Order JPA repository
│   ├── OrderSpecification.java              # Order filtering specifications
│   ├── PortabilitiesRepo.java               # Portability JPA repository
│   └── PortabilitySpecification.java        # Portability filtering specifications
└── service/
    ├── OrderService.java                    # Order business logic
    └── PortabilityService.java              # Portability business logic
```

## 🔌 API Endpoints

### Portabilities

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/portabilities` | Create new portability |
| `GET` | `/api/portabilities/{id}` | Get portability by ID |
| `POST` | `/api/portabilities/filter` | Search portabilities with filters |
| `POST` | `/api/portabilities/by-phone` | Search by phone number |
| `PUT` | `/api/portabilities/{id}` | Update portability |
| `PATCH` | `/api/portabilities/{id}/imei` | Update IMEI |
| `PATCH` | `/api/portabilities/{id}/nip` | Update NIP |
| `DELETE` | `/api/portabilities/{id}` | Delete portability |

### Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/orders` | Create new order |
| `GET` | `/api/orders/{id}` | Get order by ID |
| `GET` | `/api/orders` | List orders with filters |
| `GET` | `/api/orders/product/{productId}` | Orders by product |
| `GET` | `/api/orders/customer/{customerId}` | Orders by customer |
| `GET` | `/api/orders/sim/{simId}` | Orders by SIM |
| `GET` | `/api/orders/address/{addressId}` | Orders by address |
| `GET` | `/api/orders/checkout/{checkoutId}` | Orders by checkout |
| `GET` | `/api/orders/payment/{paymentId}` | Orders by payment |
| `PUT` | `/api/orders/{id}` | Update order |
| `PATCH` | `/api/orders/{id}/product` | Update product |
| `PATCH` | `/api/orders/{id}/sim` | Update SIM |
| `PATCH` | `/api/orders/{id}/address` | Update address |
| `PATCH` | `/api/orders/{id}/checkout` | Update checkout |
| `PATCH` | `/api/orders/{id}/payment` | Update payment |
| `DELETE` | `/api/orders/{id}` | Delete order |

## 📊 Data Models

### Portability

```java
{
  "id": Long,                        // Auto-generated ID
  "phoneNumber": String,             // 10-15 digits (unique)
  "imei": String,                    // 15 digits (optional)
  "portabilityNip": String,          // 4 digits (optional)
  "portabilityStatus": String,       // Status (enum)
  "order": Order,                    // 1-1 relationship with Order
  "createdAt": DateTime,             // Creation timestamp
  "updatedAt": DateTime              // Update timestamp
}
```

### Order

```java
{
  "id": String,                      // Format "P{timestamp}{random}"
  "customerId": Long,                // Customer ID
  "productId": Long,                 // Product ID
  "simId": Long,                     // SIM ID (optional)
  "addressId": Long,                 // Address ID (optional)
  "checkoutId": Long,                // Checkout ID (optional)
  "paymentId": Long,                 // Payment ID (optional)
  "portability": Portability,        // 1-1 relationship with Portability
  "createdAt": DateTime,             // Creation timestamp
  "updatedAt": DateTime              // Update timestamp
}
```

## 📖 Swagger Documentation

Once the application is running, access the interactive API documentation:

- **Swagger UI**: http://localhost:8082/swagger-ui.html
- **OpenAPI JSON**: http://localhost:8082/v3/api-docs

From Swagger UI you can:
- Explore all available endpoints
- View request/response schemas
- Test APIs directly from the browser
- View usage examples

## 🏗️ Microservices Architecture

This service integrates into a microservices ecosystem:

- **Eureka Server**: Service registration and discovery
- **Stripe Service**: Payment processing (integration via Feign)
- **API Gateway**: Single entry point (handles authentication)

### Eureka Configuration

The service automatically registers with Eureka on startup. Make sure the Eureka server is available.

### Stripe Service Integration

The service is ready to communicate with Stripe Service via OpenFeign:

```java
@FeignClient(name = "STRIPE-SERVICE")
public interface StripeInterface {
    // Stripe integration methods
}
```

## 🔒 Security

**Note**: Authentication and authorization are handled at the API Gateway level.

## 🧪 Testing

Run tests:

```bash
mvn test
```

## 📝 Logging

The project uses AOP (Aspect-Oriented Programming) for automatic logging:
- `LoggingAspect.java`: Intercepts method calls in the service package

Logs are generated in the `logs/` directory.
'--

**Version**: 0.0.1-SNAPSHOT  
**Java**: 25  
**Spring Boot**: 4.0.1
