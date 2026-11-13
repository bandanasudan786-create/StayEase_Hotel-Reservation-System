# StayEase – Hotel Reservation Management System

StayEase is a DBMS project developed using **React.js, Node.js (Express.js), and PostgreSQL**. It demonstrates relational database normalization (3NF), database constraints, triggers, views, ACID transactions, and temporal exclusion-based concurrency control through real-world hotel reservation workflows.

## 1. Project Structure

```text
DBMS PROJECT/
├── frontend/
│   ├── src/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── style.css
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   └── .gitignore
│
├── backend/
│   ├── server.js
│   ├── db.js
│   ├── package.json
│   ├── .env.example
│   └── .gitignore
│
├── database/
│   ├── stayease_schema.sql
│   ├── sample_data.sql
│   └── ER_DIAGRAM.drawio
│
└── README.md
```

## 2. Technology Stack

- **Frontend:** React.js (v18+) with Vite
- **Backend:** Node.js with Express.js
- **Database:** PostgreSQL (v14+)
- **Database Driver:** pg (node-postgres)
- **Styling:** CSS3

## 3. Database Setup

### Step 1: Create Database

```sql
CREATE DATABASE stayease_db;
```

### Step 2: Execute Schema

```bash
psql -U postgres -d stayease_db -f database/stayease_schema.sql
```

### Step 3: Load Sample Data

```bash
psql -U postgres -d stayease_db -f database/sample_data.sql
```

## 4. How to Run the Application

### Step 1: Start Backend

```bash
cd backend
npm install
npm run dev
```

Backend runs at: `http://localhost:5000`

- Health Check: `/api/health`
- Database Check: `/api/db-health`

### Step 2: Start Frontend

Open another terminal:

```bash
cd frontend
npm install
npm run dev
```

Frontend runs at: `http://localhost:5173`

All `/api` requests are proxied to the backend using Vite configuration.

**Note:** Configure your PostgreSQL credentials and JWT settings in the backend `.env` file using `.env.example` as a reference. Never upload your actual `.env` file to GitHub.

## 5. Backend API Endpoints

| Method | Endpoint | Access | Description |
|---|---|---|---|
| GET | `/api/health` | Public | Server health status |
| GET | `/api/db-health` | Public | PostgreSQL connectivity check |
| GET | `/api/hotels` | Public | Retrieve active hotels |
| GET | `/api/hotels/:id` | Public | Retrieve hotel details |
| GET | `/api/hotels/:id/rooms` | Public | Retrieve room categories and amenities |
| POST | `/api/availability` | Public | Check room availability |
| POST | `/api/auth/login` | Public | Authenticate users and generate JWT |
| GET | `/api/auth/me` | Authenticated | Retrieve current user profile |
| POST | `/api/auth/logout` | Authenticated | Log out current session |
| POST | `/api/reservations` | Authenticated | Create reservation using ACID transaction |
| GET | `/api/reservations/:id` | Authenticated | Retrieve reservation details |
| GET | `/api/reservations/user/:userId` | Authenticated | Retrieve customer bookings |
| GET | `/api/admin/stats` | Admin Only | View system statistics and revenue |
| GET | `/api/admin/reservations` | Admin Only | View all reservations |
| PATCH | `/api/admin/reservations/:id/status` | Admin Only | Update reservation status |

## 6. Demo Accounts

All demo accounts use the password `Password@123`.

| Role | Email |
|---|---|
| Administrator | `admin@stayease.com` |
| Customer 1 | `aarav.patel@example.com` |
| Customer 2 | `priya.nair@example.com` |
| Customer 3 | `rohan.gupta@example.com` |
| Mumbai Manager | `manager.mumbai@stayease.com` |
| Goa Manager | `manager.goa@stayease.com` |

## 7. Key DBMS Concepts Implemented

### 1. Relational Database Normalization (3NF)
- Nine normalized relational tables.
- Reduces data redundancy and update anomalies.
- Composite foreign key `(hotel_id, room_type_id)` maintains hotel-wide room number uniqueness.

### 2. Temporal Concurrency Control
- Uses PostgreSQL `btree_gist` extension.
- Implements `EXCLUDE USING gist` constraints.
- Prevents overlapping reservations for the same room.

### 3. Automated Database Triggers

- `trg_check_guest_capacity`: Prevents reservations exceeding room type capacity.
- `trg_check_review_eligibility`: Allows reviews only from guests with `checked_out` reservations.

### 4. Independent View Aggregation

- `v_hotel_ratings_summary` uses a Common Table Expression (CTE).
- Aggregates hotel reviews independently.
- Prevents row multiplication when combining review and room data.

### 5. ACID Transactions

- Atomicity: Reservation operations complete fully or roll back.
- Consistency: Database constraints maintain valid data.
- Isolation: Concurrent reservation conflicts are controlled.
- Durability: Committed reservations are permanently stored.

### 6. Database Constraints

- Primary Key and Foreign Key constraints.
- Unique constraints.
- NOT NULL constraints.
- CHECK constraints.
- Composite foreign keys.
- Exclusion constraints.

---

## 8. Project Highlights

- Complete hotel reservation management workflow.
- Role-based access for administrators, customers, and hotel managers.
- Hotel and room search functionality.
- Real-time room availability checking.
- Secure authentication using JWT.
- Reservation management and status updates.
- PostgreSQL-based concurrency control.
- Database triggers, views, and normalized schema.

## 9. Project Objective

The main objective of StayEase is to demonstrate how database management system concepts can be applied to a practical hotel reservation application while maintaining data integrity, consistency, security, and reliable transaction processing.
