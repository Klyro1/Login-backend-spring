# Spring Boot Authentication System

![Java](https://img.shields.io/badge/Java-17%2B-blue)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1.5-green)
![Security](https://img.shields.io/badge/Security-JWT-orange)
![Database](https://img.shields.io/badge/Database-MySQL-lightgrey)

A robust authentication system built with Spring Boot featuring JWT authentication, email verification, and role-based access control.

## Table of Contents
- [System Architecture](#system-architecture)
- [Core Components](#core-components)
- [Key Features](#key-features)
- [Workflows](#workflows)
- [Usage Examples](#usageexamples)
- [MailDev Installation](#installation)
- [Database Schema](#database)
- [Configuration](#configuration)
- [Testing](#testing)
- [Dependencies](#dependencies)
- [License](#license)


## System Architecture
Project

├── auth/               # Authentication endpoints

├── config/             # Application configuration

├── email/              # Email services

├── handler/            # Exception handling

├── role/               # Role management

├── security/           # JWT and security config

└── user/               # User and token management

## Core Components

### 1. User Management
- **User Entity**: Implements Spring Security interfaces 
  - Implements UserDetails and Principal for Spring Security integration
  - Contains user details (firstname, lastname, email, password, etc.)
  - Many-to-many relationship with Role entity
  - Includes auditing fields (createdDate, lastModifiedDate)
  - Implements Spring Security's required methods (getAuthorities(), isAccountNonLocked(), etc.)
  - Automatic auditing (created/modified timestamps)
  
- **Token Entity**: Manages email verification tokens
  - Stores activation tokens for email verification
  - Contains token string, creation/expiration dates
  - Many-to-one relationship with User

- **Repositories**:
  - UserRepository: Basic CRUD + custom findByEmail() method
  - TokenRepository: Basic CRUD + custom findByToken() method
  - RoleRepository: Basic CRUD + custom findByName() method

### 2. Role Management
- **Role Entity**: Defines application roles with:
  - Unique role names
  - User relationships
  - Automatic auditing

### 3. Security Components
- **JWT Service**: 
  - Generates and validates JWT tokens
  - Extracts claims from tokens
  - Uses HS256 algorithm with secret key from configuration
- **JWT Filter**: 
  - Validates tokens on each request
  - Sets authentication in SecurityContext if token is valid
  - Skips authentication for public endpoints
- **Security Config**:
  - Defines protected/public endpoints
  - Sets up JWT filter chain
- **UserDetails Service**:
  - Loads user details by email for authentication 

### 4. Authentication Flow
- **Controller**: 
  - /register: Handles user registration
  - /authenticate: Handles login (returns JWT)
  - /activate-account: Handles email verification
- **Service**: 
  - Core logic for registration, login, and activation
  - Generates activation tokens
  - Sends verification emails
  - Uses AuthenticationManager for credential validation

### 5. Email Service
- Sends HTML emails using Thymeleaf templates
- Supports async sending with @Async
- Configurable templates via EmailTemplateName

### 6. Configuration
- **Beans Config**: Sets up:
  - Authentication providers
  - Password encoder
  - Authentication manager
- **Application Properties**: Configures:
  - JWT settings
  - Database connection
  - Email server
  - Frontend URLs

### 7. Error Handling
- **Global Exception Handler**: Handles common exceptions:
  - Locked/disabled accounts
  - Bad credentials
  - Validation errors
  - Messaging exceptions
- **Business Error Codes**:
  - Standardized error codes and messages
- **Example error response:**:
    {
      "businessErrorCode": 304,
      "businessErrorDescription": "Login and / or Password is incorrect",
      "error": "Bad credentials"
    }

## Key Features

- **Secure JWT Authentication**:
  - Stateless token-based security
  - Custom claims support
  - Token validation middleware

- **Email Verification**:
  - Account activation flow
  - Token expiration (15 minutes)
  - Resend capability

- **Role-Based Access Control**:
  - Flexible role definitions
  - Easy to extend for admin roles

- **Comprehensive Validation**:
  - Request payload validation
  - Password complexity requirements
  - Custom error messages

## Workflows

### 1. Registration Flow
1. User submits registration form
2. System:
   - Encodes password
   - Assigns USER role
   - Saves disabled account
   - Generates activation token
   - Sends verification email
3. User clicks activation link
4. System validates token and enables account

### 2. Login Flow
1. User submits credentials
2. System:
   - Validates credentials
   - Generates JWT with claims
   - Returns token to client
3. Client uses token for authenticated requests

## Usage Examples
### 1. Register a new user
- **Request**:
  - POST /api/v1/auth/register
    Content-Type: application/json
    
    {
      "firstname": "John",
      "lastname": "Doe",
      "email": "john.doe@example.com",
      "password": "securePassword123"
    }

- **Response**:
  - HTTP/1.1 202 Accepted
  
### 2. Authenticate (Login)
- **Request**:
  - POST /api/v1/auth/authenticate
  Content-Type: application/json
  
  {
    "email": "john.doe@example.com",
    "password": "securePassword123"
  }

- **Response**:
  - {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }

### 3. Activate Account
- **Request**:
  GET /api/v1/auth/activate-account?token=123456

- **Response**:
  HTTP/1.1 202 Accepted

## Installation
1. open your intelij terminal
2. npm i maildev
3. maildev 

## Database Schema
The system automatically creates these tables:
- **user**: Stores user information
- **role**: Contains application roles
- **token**: Manages account activation tokens
- **user_roles**: Join table for user-role relationship

## Configuration
### application.properties 
- **Database configuration**
  - spring.datasource.url=jdbc:mysql://localhost:3306/Project?&createDatabaseIfNotExist=true&useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
  - spring.datasource.username=root
  - spring.datasource.password=
- **JWT**
  - application.security.jwt.secret-key=1fd8e1606256f52b0860dac2adcc08631fd8e1606256f52b0860dac2adcc0863
  - application.security.jwt.expiration=3600000
- **Email**
  - spring.mail.host=localhost
  - spring.mail.port=1025
  - spring.mail.username=admin
  - spring.mail.password=admin
  - spring.mail.properties.mail.smtp.trust=*
  - spring.mail.properties.mail.smtp.auth=true
  - spring.mail.properties.mail.smtp.starttls.enabled=true

## Testing
### Recommended test cases:
1. Registration with valid/invalid data
2. Email verification flow
3. Login scenarios:
  - Correct credentials
  - Incorrect credentials
  - Disabled/locked accounts
4. Protected endpoint access
**For email testing in development, use MailHog.**

## Dependencies
- Spring Boot 3.x
- Spring Security
- Spring Data JPA
- JJWT (JWT library)
- Lombok
- Thymeleaf (email templates)
- Swagger (API docs)

## License
*This README provides:*
1. Clear structure with table of contents
2. Visual badges for key technologies
3. Detailed component explanations
4. Visual architecture overview
5. Step-by-step workflows
6. Practical configuration examples
7. Testing recommendations
8. Dependency information
