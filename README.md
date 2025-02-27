﻿# Vollmed API

A robust and modern RESTful API for managing patients, doctors, and appointments in a cutting-edge clinic application. Built with the latest Spring Boot technologies, this project demonstrates best practices in enterprise Java development.

## Features

- **Patient Management**
    - CRUD operations for patient records (name, contact details, medical history, etc.)

- **Doctor Management**
    - Manage doctor profiles, specialties, availability, and contact information

- **Appointment & Enquiry System**
    - Create and manage appointments and medical enquiries
    - Track appointment status (e.g., OPEN, IN_PROGRESS, CLOSED, CANCELLED)

- **Security**
    - JWT-based authentication and authorization using Spring Security

- **Database Management**
    - Schema migrations handled by Flyway
    - MySQL for production and H2 for in-memory testing

- **API Documentation**
    - Automatically generated Swagger UI documentation powered by Springdoc OpenAPI

## Technologies Used

- **Framework**: Spring Boot 3.4.2-SNAPSHOT (built on Spring Framework 6.x)
- **Language**: Java 23
- **Database**: MySQL (production) & H2 (in-memory for testing)
- **Persistence**: Spring Data JPA with Hibernate
- **Validation**: Jakarta Bean Validation (Spring Boot Starter Validation)
- **Security**: Spring Security with JWT (using Auth0's java-jwt 4.2.1)
- **Documentation**: Springdoc OpenAPI Starter WebMVC UI (version 2.7.0)
- **Build Tool**: Maven 3.8+
- **Utilities**: Lombok for reducing boilerplate code

## Installation & Usage

### Prerequisites

- **Java**: Java 23 (or later)
- **Maven**: 3.8+
- **Database**: MySQL (or H2 for development/testing)

### Clone the Repository

```bash
git clone https://github.com/yourusername/clinic-management-api.git
cd clinic-management-api
```
### Build the Project
```bash
mvn clean install
```
### Run the Application
```bash
java -jar target/api-0.0.1-SNAPSHOT.jar
```

### Access API
- Base URL: http://localhost:8080/api/v1

### API Endpoints
If you're already running the application, you can access the http://localhost:8080/swagger-ui.html to have a better look.

---

#### **Patients**

| Method | Endpoint              | Description                      | Request Body                 | Response  |
|--------|------------------------|----------------------------------|------------------------------|-----------|
| POST   | `/patients`            | Register a new patient          | `PatientRegistrationData`    | 201 Created (Patient details) |
| GET    | `/patients`            | List all active patients        | -                            | 200 OK (Paginated list) |
| GET    | `/patients/{id}`       | Get patient details by ID       | -                            | 200 OK (Patient details) |
| PUT    | `/patients`            | Update patient information      | `PatientUpdateData`          | 200 OK (Updated patient details) |
| DELETE | `/patients/{id}`       | Logically delete a patient      | -                            | 204 No Content |

---

### **Doctors**

| Method | Endpoint              | Description                      | Request Body              | Response  |
|--------|------------------------|----------------------------------|---------------------------|-----------|
| POST   | `/doctors`             | Register a new doctor           | `DoctorRegisterData`      | 201 Created (Doctor details) |
| GET    | `/doctors`             | List all active doctors         | -                         | 200 OK (Paginated list) |
| GET    | `/doctors/{id}`        | Get doctor details by ID        | -                         | 200 OK (Doctor details) |
| PUT    | `/doctors`             | Update doctor information       | `DoctorUpdateData`        | 200 OK (Updated doctor details) |
| DELETE | `/doctors/{id}`        | Delete doctor (physical delete) | -                         | 204 No Content |
| DELETE | `/doctors/Logically/{id}` | Logically delete doctor      | -                         | 204 No Content |

---

#### **Appointments**

| Method | Endpoint       | Description                          | Request Body                   | Response  |
|--------|---------------|--------------------------------------|--------------------------------|-----------|
| POST   | `/appointments` | Book a new appointment             | `BookingAppointmentData`       | 200 OK    |
| DELETE | `/appointments` | Cancel an existing appointment     | `AppointmentCancellationData`  | 204 No Content |

---

#### **Authentication**

| Method | Endpoint  | Description           | Request Body         | Response  |
|--------|-----------|-----------------------|----------------------|-----------|
| POST   | `/login`  | Authenticate user and return JWT token | `AuthenticationData` | 200 OK (JWT Token) |

---

# **Database Configuration Guide**

## **1. Database Connection**

This project uses **MySQL** as its database. Below are the connection details:

- **Database URL:** `jdbc:mysql://localhost:3306/vollmed_api_database?createDatabaseIfNotExist=true`
- **Username:** `root`
- **Password:** `root`
- **Driver:** `com.mysql.cj.jdbc.Driver`

### **Automatic Database Creation**
The `createDatabaseIfNotExist=true` flag ensures that MySQL automatically creates the database if it doesn’t exist.

---

## **2. Configuration in `application.properties`**

Your database settings are defined in `application.properties`:

```properties
# Application Name
spring.application.name=vollmed_api

# MySQL Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/vollmed_api_database?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Flyway (Database Migrations)
spring.flyway.enabled=true
spring.flyway.baseline-on-migrate=true
spring.flyway.locations=classpath:db/migration

# Hibernate Configuration
spring.jpa.hibernate.ddl-auto=none
spring.jpa.properties.hibernate.hbm2ddl.auto=update
spring.jpa.properties.hibernate.hbm2ddl.export=true

# Show SQL queries in logs
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Server Configuration
server.error.include-stacktrace=never
server.port=8080

# JWT Security
api.security.token.secret=${JWT_SECRET:123456789}
```

---

## **3. Understanding the Configuration**

### **Database Settings**
- **`spring.datasource.url`** → Connects to the MySQL database.
- **`spring.datasource.username` & `spring.datasource.password`** → Sets database credentials.
- **`spring.datasource.driver-class-name`** → Specifies MySQL JDBC driver.

### **Flyway (Database Migrations)**
- **`spring.flyway.enabled=true`** → Enables Flyway for managing schema migrations.
- **`spring.flyway.baseline-on-migrate=true`** → Baselines the database on first migration.
- **`spring.flyway.locations=classpath:db/migration`** → Defines where Flyway migration scripts are stored.

### **Hibernate (JPA ORM Configuration)**
- **`spring.jpa.hibernate.ddl-auto=none`** → Disables automatic schema updates (Flyway is used instead).
- **`spring.jpa.properties.hibernate.hbm2ddl.auto=update`** → Allows schema updates without dropping tables.
- **`spring.jpa.show-sql=true`** → Logs SQL queries in the console.
- **`spring.jpa.properties.hibernate.format_sql=true`** → Formats SQL output for better readability.

### **Security and Server Settings**
- **`server.error.include-stacktrace=never`** → Hides stack traces in API error responses for security.
- **`server.port=8080`** → Runs the application on port 8080.
- **`api.security.token.secret=${JWT_SECRET:123456789}`** → Configures JWT authentication with a secret key.

---

## **4. Managing Migrations with Flyway**

To manage database schema updates, Flyway migration scripts should be placed in:

:open_file_folder: `src/main/resources/db/migration/`

### **Example Migration File (`V1__create_table.sql`)**
```sql
CREATE TABLE doctors (
    id BIGINT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(20) NOT NULL,
    crm VARCHAR(6) NOT NULL UNIQUE,
    expertise VARCHAR(50) NOT NULL,
    street VARCHAR(100) NOT NULL,
    neighbourhood VARCHAR(100) NOT NULL,
    address_code VARCHAR(9) NOT NULL,
    city VARCHAR(100) NOT NULL,
    fu CHAR(2) NOT NULL,
    number VARCHAR(20),
    complement VARCHAR(100),
    active BOOLEAN NOT NULL DEFAULT TRUE,

    PRIMARY KEY (id)
);

```

### **Applying Migrations**
Flyway will run scripts in order (e.g., `V1__create_table.sql`, `V2__add_column.sql`).  
To manually trigger migrations, run:
```sh
mvn flyway:migrate
```

---
