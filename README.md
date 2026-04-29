# 🏡 Airbnb Database System

A fully normalized relational database schema simulating the core backend of an Airbnb-like rental marketplace — built with **MySQL 8.0**.

\---

## 📌 Project Overview

This project models a real-world property rental platform with **20 interconnected tables**, covering everything from user identity verification and property listings to bookings, payments, reviews, and host earnings. It demonstrates strong database design principles including normalization, referential integrity, and constraint enforcement.

> \*\*Tech Stack:\*\* MySQL Server 8.0 · MySQL Workbench 8.0 · MySQL Shell 8.0

\---

## 🗂️ Database Schema

The schema is organized around six core domains:

|Domain|Tables|
|-|-|
|**Users \& Identity**|`users`, `guest\_identity`, `host\_identity`, `language`|
|**Location**|`country`, `city`, `address`|
|**Property**|`property`, `property\_image`, `property\_amenity`, `amenity`, `house\_rules`|
|**Bookings**|`booking`, `booking\_cancellation`|
|**Financials**|`payment`, `host\_earnings`, `promotion`, `discount`|
|**Reviews**|`review`, `property\_review`|

\---

## 🗺️ Entity Relationship Diagram (ERD)

!\[Airbnb Database ERD](airbnb\_erd.png)

\---

## 🔗 Key Relationships \& Cardinality

```
Users        ──< Property         (1 user can list many properties)
Users        ──< Booking          (1 user can make many bookings)
Property     ──< Booking          (1 property can have many bookings)
Booking      ──  Payment          (each booking has exactly 1 payment)
Booking      ──  Booking\_Cancel   (each booking has at most 1 cancellation)
Property     ──  Address          (1:1)
Country      ──< City             ──< Address
Property     ──< Property\_Review
Review       ──< Property\_Review
Host\_Identity──< Host\_Earnings
```

All foreign keys use `ON DELETE CASCADE ON UPDATE CASCADE` to maintain referential integrity automatically.

\---

## 📐 Schema Highlights

### Sample Table: `Booking`

|Column|Type|Constraint|
|-|-|-|
|`Booking\_ID`|INT|PRIMARY KEY, AUTO\_INCREMENT|
|`Check\_in\_date`|DATE|NOT NULL|
|`Check\_out\_date`|DATE|NOT NULL|
|`Total\_price`|DECIMAL(10,2)|NOT NULL|
|`State`|VARCHAR(15)|NOT NULL|
|`Property\_ID`|INT|FK → Property|
|`User\_ID`|INT|FK → Users|

### Sample Table: `Payment`

|Column|Type|Constraint|
|-|-|-|
|`Payment\_ID`|INT|PRIMARY KEY, AUTO\_INCREMENT|
|`Amount`|DECIMAL(10,2)|NOT NULL|
|`Payment\_method`|VARCHAR(10)|NOT NULL|
|`Payment\_date`|DATE|NOT NULL|
|`Booking\_ID`|INT|FK → Booking|
|`User\_ID`|INT|FK → Users|

\---

## 🚀 Getting Started

### Prerequisites

* MySQL Server 8.0+
* MySQL Workbench (recommended)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/airbnb-database.git
cd airbnb-database

# 2. Open MySQL Workbench and connect to your server

# 3. Run the schema script first
# File > Open SQL Script > airbnb\_db.sql > Execute (⚡)

# 4. Verify the database was created
SHOW DATABASES;
USE airbnb\_db;
SHOW TABLES;
```

> ⚠️ \*\*Important:\*\* Run table creation scripts before insertion scripts. Insert into parent/independent tables before dependent ones to avoid foreign key constraint errors.

\---

## ✅ Testing the Setup

```sql
-- Verify the database exists
SHOW DATABASES;

-- Check all tables are created
USE airbnb\_db;
SHOW TABLES;

-- Test a sample query
SELECT \* FROM users LIMIT 5;
```

\---

## 📊 Database Info

|Metric|Value|
|-|-|
|Total Tables|20|
|Storage Engine|InnoDB|
|Schema Size|\~0.72 MB|
|Database|`airbnb\_db`|

\---

## 🧩 Use Case Flow

```
Guest searches → filters by location, price, amenities
    → selects property → booking created
    → payment processed → confirmation sent
    → post-stay: guest leaves review
    → host receives payout via host\_earnings
    → cancellation? → refund logged in booking\_cancellation
```

\---

## 📁 Repository Structure

```
airbnb-database/
│
├── airbnb\_db.sql          # Full schema (table creation)
├── inserts/               # Data insertion scripts (ordered by dependency)
├── README.md              # This file
└── docs/
    ├── metadata.pdf       # Table structures \& cardinality
    ├── abstract.pdf       # Project overview
    └── installation.pdf   # Setup guide
```

\---

## 👤 Author

**Your Name**
[LinkedIn](https://linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username)

