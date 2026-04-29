# 🏡 Airbnb Database System

A fully normalized relational database schema simulating the core backend of an Airbnb-like rental marketplace — built with **MySQL 8.0**.

---

## 📌 Project Overview

This project models a real-world property rental platform with **20 interconnected tables**, covering everything from user identity verification and property listings to bookings, payments, reviews, and host earnings. It demonstrates strong database design principles including normalization, referential integrity, and constraint enforcement.

> **Tech Stack:** MySQL Server 8.0 · MySQL Workbench 8.0 · MySQL Shell 8.0

---

## 🗂️ Database Schema

The schema is organized around six core domains:

| Domain | Tables |
|---|---|
| **Users & Identity** | `users`, `guest_identity`, `host_identity`, `language` |
| **Location** | `country`, `city`, `address` |
| **Property** | `property`, `property_image`, `property_amenity`, `amenity`, `house_rules` |
| **Bookings** | `booking`, `booking_cancellation` |
| **Financials** | `payment`, `host_earnings`, `promotion`, `discount` |
| **Reviews** | `review`, `property_review` |

---

## 🗺️ Entity Relationship Diagram (ERD)

![Airbnb Database ERD](docs/airbnb_erd.png)

---

## 🔗 Key Relationships & Cardinality

```
Users        ──< Property         (1 user can list many properties)
Users        ──< Booking          (1 user can make many bookings)
Property     ──< Booking          (1 property can have many bookings)
Booking      ──  Payment          (each booking has exactly 1 payment)
Booking      ──  Booking_Cancel   (each booking has at most 1 cancellation)
Property     ──  Address          (1:1)
Country      ──< City             ──< Address
Property     ──< Property_Review
Review       ──< Property_Review
Host_Identity──< Host_Earnings
```

All foreign keys use `ON DELETE CASCADE ON UPDATE CASCADE` to maintain referential integrity automatically.

---

## 📐 Schema Highlights

### Sample Table: `Booking`
| Column | Type | Constraint |
|---|---|---|
| `Booking_ID` | INT | PRIMARY KEY, AUTO_INCREMENT |
| `Check_in_date` | DATE | NOT NULL |
| `Check_out_date` | DATE | NOT NULL |
| `Total_price` | DECIMAL(10,2) | NOT NULL |
| `State` | VARCHAR(15) | NOT NULL |
| `Property_ID` | INT | FK → Property |
| `User_ID` | INT | FK → Users |

### Sample Table: `Payment`
| Column | Type | Constraint |
|---|---|---|
| `Payment_ID` | INT | PRIMARY KEY, AUTO_INCREMENT |
| `Amount` | DECIMAL(10,2) | NOT NULL |
| `Payment_method` | VARCHAR(10) | NOT NULL |
| `Payment_date` | DATE | NOT NULL |
| `Booking_ID` | INT | FK → Booking |
| `User_ID` | INT | FK → Users |

---

## 🚀 Getting Started

### Prerequisites
- MySQL Server 8.0+
- MySQL Workbench (recommended)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Alfredo4651/airbnb-database.git
cd airbnb-database

# 2. Open MySQL Workbench and connect to your server

# 3. Run the schema script first
# File > Open SQL Script > airbnb_db.sql > Execute (⚡)

# 4. Verify the database was created
SHOW DATABASES;
USE airbnb_db;
SHOW TABLES;
```

> ⚠️ **Important:** Run table creation scripts before insertion scripts. Insert into parent/independent tables before dependent ones to avoid foreign key constraint errors.

---

## ✅ Testing the Setup

```sql
-- Verify the database exists
SHOW DATABASES;

-- Check all tables are created
USE airbnb_db;
SHOW TABLES;

-- Test a sample query
SELECT * FROM users LIMIT 5;
```

---

## 📊 Database Info

| Metric | Value |
|---|---|
| Total Tables | 20 |
| Storage Engine | InnoDB |
| Schema Size | ~0.72 MB |
| Database | `airbnb_db` |

---

## 🧩 Use Case Flow

```
Guest searches → filters by location, price, amenities
    → selects property → booking created
    → payment processed → confirmation sent
    → post-stay: guest leaves review
    → host receives payout via host_earnings
    → cancellation? → refund logged in booking_cancellation
```


---

## 👤 Author

**Your Name**
[LinkedIn](https://www.linkedin.com/in/alfredmichael01/) · [GitHub](https://github.com/Alfredo4651/airbnb-db-schema)
