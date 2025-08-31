# Airbnb Clone – Backend Requirements Specifications

This document details the requirements specifications for three core backend features: **User Authentication**, **Property Management**, and **Booking System**.
It outlines API endpoints, input/output specifications, validation rules, and performance criteria.

---

## 1. User Authentication

### Overview

Handles user account creation, login, authentication, and session management using JWT tokens.

### API Endpoints

* `POST /api/v1/auth/register` – Register a new user.
* `POST /api/v1/auth/login` – Authenticate user and issue JWT token.
* `GET /api/v1/auth/profile` – Retrieve current user profile (requires JWT).
* `PUT /api/v1/auth/profile` – Update profile details.

### Input Specifications

* **Register**:

  ```json
  {
    "first_name": "Alice",
    "last_name": "Johnson",
    "email": "alice@example.com",
    "password": "StrongPass123!",
    "phone_number": "+254700111111"
  }
  ```
* **Login**:

  ```json
  {
    "email": "alice@example.com",
    "password": "StrongPass123!"
  }
  ```

### Output Specifications

* **Register/Login (success)**:

  ```json
  {
    "message": "Registration successful",
    "token": "<JWT_TOKEN>"
  }
  ```
* **Profile (GET)**:

  ```json
  {
    "user_id": "uuid",
    "first_name": "Alice",
    "last_name": "Johnson",
    "email": "alice@example.com",
    "role": "guest",
    "created_at": "2025-08-31T12:00:00Z"
  }
  ```

### Validation Rules

* Email must be unique and valid format.
* Password: min 8 chars, at least one uppercase, one lowercase, one digit, one special character.
* Phone number must follow E.164 format.
* Role ∈ {guest, host, admin}.

### Performance Criteria

* Authentication requests must respond within **500ms** under normal load.
* Passwords stored using **bcrypt hashing** with a minimum cost factor of 12.
* JWT tokens expire after 24 hours.

---

## 2. Property Management

### Overview

Allows hosts to create, update, and manage property listings. Supports property search and filtering for guests.

### API Endpoints

* `POST /api/v1/properties` – Create a new property (host only).
* `PUT /api/v1/properties/:id` – Update property details.
* `DELETE /api/v1/properties/:id` – Remove a property listing.
* `GET /api/v1/properties/:id` – Retrieve details of a property.
* `GET /api/v1/properties?location=Nairobi&price_min=50&price_max=200` – Search & filter properties.

### Input Specifications

* **Create property**:

  ```json
  {
    "name": "Cozy Apartment Nairobi",
    "description": "2-bedroom furnished apartment in Westlands",
    "location": "Nairobi, Kenya",
    "price_per_night": 50.00
  }
  ```

### Output Specifications

* **Success**:

  ```json
  {
    "property_id": "uuid",
    "name": "Cozy Apartment Nairobi",
    "location": "Nairobi, Kenya",
    "price_per_night": 50.00,
    "host_id": "uuid",
    "created_at": "2025-08-31T12:00:00Z"
  }
  ```

### Validation Rules

* Name: required, max 100 characters.
* Price per night: numeric, > 0.
* Description: required, max 500 characters.
* Location: required.
* Only **hosts** can create or modify properties.

### Performance Criteria

* Property search must return results in **< 1 second** for up to 10,000 listings.
* Must support **pagination** (default: 20 per page, max: 100).

---

## 3. Booking System

### Overview

Manages property bookings by guests, including availability checks, approvals, and status updates.

### API Endpoints

* `POST /api/v1/bookings` – Create a booking request.
* `GET /api/v1/bookings/:id` – Retrieve booking details.
* `PUT /api/v1/bookings/:id/status` – Update booking status (host only).
* `GET /api/v1/bookings/user/:id` – View bookings for a user.

### Input Specifications

* **Create booking**:

  ```json
  {
    "property_id": "uuid",
    "start_date": "2025-09-15",
    "end_date": "2025-09-20"
  }
  ```

### Output Specifications

* **Booking created**:

  ```json
  {
    "booking_id": "uuid",
    "property_id": "uuid",
    "user_id": "uuid",
    "start_date": "2025-09-15",
    "end_date": "2025-09-20",
    "status": "pending",
    "total_price": 250.00
  }
  ```

### Validation Rules

* Dates must be valid and `end_date > start_date`.
* Property must not be already booked for requested dates.
* Only **guests** can create bookings; only **hosts** can approve/reject.
* Status ∈ {pending, confirmed, rejected, canceled}.

### Performance Criteria

* Booking availability check must complete in **< 500ms**.
* Must prevent **double bookings** using transaction-level locking.
* System should scale to handle **10,000 concurrent bookings/day**.

---

## Notes

* All endpoints require **JWT authentication** unless otherwise specified.
* Responses use **ISO 8601 timestamps** (`YYYY-MM-DDTHH:MM:SSZ`).
* Errors follow a standardized format:

  ```json
  {
    "error": "ValidationError",
    "message": "Email already exists"
  }
  ```

---
