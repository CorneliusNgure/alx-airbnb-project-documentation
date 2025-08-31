# 🏡 Airbnb Clone – Backend

A robust, scalable backend for an **Airbnb-style marketplace** built with **PostgreSQL**.  
This project implements core features like **user authentication, property management, bookings, payments, reviews, and messaging** — structured for correctness, performance, and developer productivity.

---

## Contents
- [Overview](#-overview)
- [Features](#-features)
- [System Workflow](#-system-workflow)
- [Database Schema](#-database-schema)
- [Setup](#-setup)
- [Seeding the Database](#-seeding-the-database)
- [Contributing](#-contributing)
- [License](#-license)

---

## Overview
This backend powers a simplified Airbnb-style application where:
- Guests can **search properties** and **make bookings**.
- Hosts can **list properties** and **manage bookings**.
- Secure **payments** are handled with audit history.
- Guests can **leave reviews**, and all users can **exchange messages**.

---

## Features

### User Management
- Register & login with JWT authentication
- Role-based access: `guest`, `host`, `admin`
- Profile management (update phone, email, etc.)

### Property Management
- Hosts can create, update, and delete listings
- Property details: name, description, location, nightly price
- Guests can search and filter properties

### Booking System
- Guests can request bookings
- Hosts approve/reject requests
- Booking lifecycle: `pending → confirmed → canceled`

### Payments
- Payments tied to bookings
- Supports multiple methods (`credit_card`, `paypal`, `stripe`)
- Payment confirmations recorded for audit/history

### Reviews
- Guests leave reviews after completed bookings
- Ratings (1–5) with comments
- Reviews visible on property listings

### Messaging
- Guests and hosts can communicate directly
- Messages stored with sender/recipient references
- Supports conversation history