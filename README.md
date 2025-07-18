# Spring Boot Authentication System

![Java](https://img.shields.io/badge/Java-17%2B-blue)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1.5-green)
![Security](https://img.shields.io/badge/Security-JWT-orange)
![Database](https://img.shields.io/badge/Database-MySQL-lightgrey)

A robust authentication system built with Spring Boot featuring JWT authentication, email verification, and role-based access control.

## Table of Contents
- [Features](#features)
- [System Architecture](#system-architecture)
- [Core Components](#core-components)
- [Key Features](#key-features)
- [Workflows](#workflows)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [Dependencies](#dependencies)
- [License](#license)

## Features
✅ JWT Authentication - Secure stateless authentication

✉️ Email Verification - Account activation via email

🔐 Role-Based Access - Ready for USER/ADMIN roles

📝 Validation - Robust request validation

📚 API Documentation - Integrated Swagger UI

🛡️ Security - Password encryption, account locking

📈 Auditing - Automatic tracking of create/update times

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
- **User Entity**: Implements Spring Security interfaces with:
  - Personal details (name, email, password)
  - Role relationships
  - Account status fields
  - Automatic auditing (created/modified timestamps)
  
- **Token Entity**: Manages email verification tokens with:
  - Token strings
  - Expiration timestamps
  - User relationships

- **Repositories**:
  - Custom queries for email and token lookup

### 2. Role Management
- **Role Entity**: Defines application roles with:
  - Unique role names
  - User relationships
  - Automatic auditing

### 3. Security Components
- **JWT Service**: Handles token generation/validation
- **JWT Filter**: Validates tokens on each request
- **Security Config**: Defines protected/public endpoints
- **UserDetails Service**: Loads user credentials

### 4. Authentication Flow
- **Controller**: REST endpoints for:
  - Registration
  - Login
  - Account activation
- **Service**: Core business logic for:
  - User registration
  - Credential validation
  - Email verification

### 5. Email Service
- Sends HTML emails asynchronously
- Supports template-based emails
- Configurable through properties

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
- Global exception handler for:
  - Authentication failures
  - Validation errors
  - System exceptions
- Standardized error codes and messages

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

- **API Documentation**:
  - Integrated Swagger UI
  - Automatic endpoint documentation

## Workflows

### Registration Flow
1. User submits registration form
2. System:
   - Encodes password
   - Assigns USER role
   - Saves disabled account
   - Generates activation token
   - Sends verification email
3. User clicks activation link
4. System validates token and enables account

### Login Flow
1. User submits credentials
2. System:
   - Validates credentials
   - Generates JWT with claims
   - Returns token to client
3. Client uses token for authenticated requests

## Installation


