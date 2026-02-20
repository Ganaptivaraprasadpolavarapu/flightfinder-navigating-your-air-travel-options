# Solution Architecture

## Flight Booking Application - MERN Stack

---

## 1. ARCHITECTURE OVERVIEW

The Flight Booking Application follows a three-tier architecture pattern with clear separation of concerns:

1. **Presentation Layer** (Client): React.js-based SPA
2. **Application Layer** (Server): Node.js + Express.js REST API
3. **Data Layer** (Database): MongoDB NoSQL Database

```
┌─────────────────────────────────────────────────────────────────┐
│                     PRESENTATION LAYER                          │
│                       (React Client)                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │  Pages   │ │Components│ │ Context  │ │ Services │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
└────────────────────────────┬────────────────────────────────────┘
                             │ HTTP/HTTPS (REST API)
                             │ JSON Data Exchange
┌────────────────────────────▼────────────────────────────────────┐
│                     APPLICATION LAYER                           │
│                   (Node.js + Express.js)                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │  Routes  │ │Controllers│ │Middleware│ │  Models  │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
└────────────────────────────┬────────────────────────────────────┘
                             │ MongoDB Protocol
                             │ BSON Data Exchange
┌────────────────────────────▼────────────────────────────────────┐
│                        DATA LAYER                               │
│                      (MongoDB Database)                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                        │
│  │  Users   │ │ Flights  │ │ Bookings │                        │
│  └──────────┘ └──────────┘ └──────────┘                        │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2. SYSTEM ARCHITECTURE DIAGRAM

### 2.1 High-Level Architecture

```
                           Internet
                              │
                              │
                    ┌─────────▼──────────┐
                    │                    │
                    │   Load Balancer    │
                    │   (Cloud Provider) │
                    │                    │
                    └─────────┬──────────┘
                              │
              ┌───────────────┴───────────────┐
              │                               │
    ┌─────────▼──────────┐         ┌─────────▼──────────┐
    │                    │         │                    │
    │  Frontend Server   │         │  Backend Server    │
    │   (Netlify/Vercel) │         │   (Heroku/Render)  │
    │                    │         │                    │
    │   Static Files     │         │   REST API         │
    │   React SPA        │         │   Express.js       │
    │                    │         │                    │
    └────────────────────┘         └─────────┬──────────┘
                                             │
                                             │
                                   ┌─────────▼──────────┐
                                   │                    │
                                   │  Database Server   │
                                   │  (MongoDB Atlas)   │
                                   │                    │
                                   │  Cloud Database    │
                                   │                    │
                                   └────────────────────┘
```

---

## 3. FRONTEND ARCHITECTURE

### 3.1 React Application Structure

```
client/
│
├── public/
│   ├── index.html          # HTML template
│   ├── manifest.json       # PWA manifest
│   └── robots.txt          # SEO robots file
│
├── src/
│   ├── index.js            # Application entry point
│   ├── App.js              # Root component
│   ├── App.css             # Global styles
│   │
│   ├── components/         # Reusable components
│   │   ├── Navbar.jsx      # Navigation bar
│   │   ├── Login.jsx       # Login form
│   │   └── Register.jsx    # Registration form
│   │
│   ├── pages/              # Page components
│   │   ├── LandingPage.jsx      # Home/Landing
│   │   ├── Authenticate.jsx     # Auth page
│   │   ├── Flights.jsx          # View flights
│   │   ├── BookFlight.jsx       # Booking page
│   │   ├── Bookings.jsx         # User bookings
│   │   ├── Admin.jsx            # Admin dashboard
│   │   ├── AllFlights.jsx       # Admin flights list
│   │   ├── NewFlight.jsx        # Add flight
│   │   ├── EditFlight.jsx       # Edit flight
│   │   ├── AllBookings.jsx      # All bookings (admin)
│   │   ├── AllUsers.jsx         # All users (admin)
│   │   ├── FlightAdmin.jsx      # Flight management
│   │   ├── FlightBookings.jsx   # Flight bookings
│   │   └── FlightRequests.jsx   # Flight requests
│   │
│   ├── context/            # Context API for state
│   │   └── GeneralContext.jsx   # Global state
│   │
│   ├── RouteProtectors/    # Route protection
│   │   ├── AuthProtector.jsx    # Protected routes
│   │   └── LoginProtector.jsx   # Login redirect
│   │
│   ├── styles/             # Component-specific styles
│   │   ├── Navbar.css
│   │   ├── LandingPage.css
│   │   ├── Authenticate.css
│   │   ├── AllFlights.css
│   │   ├── BookFlight.css
│   │   ├── Bookings.css
│   │   ├── Admin.css
│   │   ├── allUsers.css
│   │   ├── FlightAdmin.css
│   │   └── NewFlight.css
│   │
│   └── assets/             # Static assets
│       ├── images/
│       └── icons/
│
└── package.json            # Dependencies
```

### 3.2 Component Hierarchy

```
App
│
├── Navbar (Always visible)
│
├── Router
│   │
│   ├── LandingPage (Public)
│   │
│   ├── Authenticate (Public/Login Protector)
│   │   ├── Login
│   │   └── Register
│   │
│   ├── Flights (Protected)
│   │
│   ├── BookFlight (Protected)
│   │
│   ├── Bookings (Protected)
│   │
│   └── Admin Routes (Protected + Admin Only)
│       ├── Admin Dashboard
│       ├── AllFlights
│       ├── NewFlight
│       ├── EditFlight
│       ├── AllBookings
│       ├── AllUsers
│       ├── FlightAdmin
│       ├── FlightBookings
│       └── FlightRequests
```

### 3.3 State Management Flow

```
┌─────────────────────────────────────────┐
│         GeneralContext Provider         │
│                                         │
│  State:                                 │
│  - user (current user object)           │
│  - token (JWT token)                    │
│  - isAuthenticated (boolean)            │
│  - role (user/admin)                    │
│                                         │
│  Actions:                               │
│  - login(userData)                      │
│  - logout()                             │
│  - register(userData)                   │
│  - updateUser(userData)                 │
│                                         │
└───────────────┬─────────────────────────┘
                │
                │ Context consumed by:
                │
    ┌───────────┴───────────┐
    │                       │
    ▼                       ▼
┌─────────┐           ┌──────────┐
│ Navbar  │           │  Pages   │
│         │           │          │
│ - Shows │           │ - Access │
│   user  │           │   user   │
│ - Logout│           │   data   │
│   button│           │ - Auth   │
└─────────┘           │   checks │
                      └──────────┘
```

---

## 4. BACKEND ARCHITECTURE

### 4.1 Server Application Structure

```
server/
│
├── index.js              # Server entry point & configuration
├── schemas.js            # Mongoose schemas & models
├── .env                  # Environment variables
├── package.json          # Dependencies
│
├── routes/               # API routes (future organization)
│   ├── authRoutes.js     # Authentication routes
│   ├── flightRoutes.js   # Flight routes
│   ├── bookingRoutes.js  # Booking routes
│   └── userRoutes.js     # User routes
│
├── controllers/          # Business logic (future organization)
│   ├── authController.js
│   ├── flightController.js
│   ├── bookingController.js
│   └── userController.js
│
├── middleware/           # Custom middleware (future organization)
│   ├── auth.js           # Authentication middleware
│   ├── admin.js          # Admin authorization
│   └── errorHandler.js   # Error handling
│
└── models/               # Database models (future organization)
    ├── User.js
    ├── Flight.js
    └── Booking.js
```

### 4.2 Request-Response Flow

```
Client Request
     │
     ▼
┌──────────────────┐
│  Express Server  │
│  (Port 5000)     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  CORS Middleware │  ◄── Cross-origin handling
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Body Parser     │  ◄── Parse JSON body
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Route Handler   │  ◄── Match route
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Auth Middleware │  ◄── Verify JWT (if protected)
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Controller      │  ◄── Business logic
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Mongoose Model  │  ◄── Database operations
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  MongoDB         │  ◄── Data persistence
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Response        │  ◄── JSON response to client
└──────────────────┘
```

### 4.3 API Endpoint Architecture

```
/api
│
├── /auth
│   ├── POST   /register          # Register new user
│   ├── POST   /login             # User login
│   └── GET    /verify             # Verify token
│
├── /flights
│   ├── GET    /                  # Get all flights
│   ├── GET    /:id               # Get single flight
│   ├── POST   /                  # Create flight (admin)
│   ├── PUT    /:id               # Update flight (admin)
│   └── DELETE /:id               # Delete flight (admin)
│
├── /bookings
│   ├── GET    /                  # Get user's bookings
│   ├── GET    /all               # Get all bookings (admin)
│   ├── GET    /:id               # Get single booking
│   ├── POST   /                  # Create booking
│   └── DELETE /:id               # Cancel booking
│
└── /users
    ├── GET    /                  # Get all users (admin)
    ├── GET    /:id               # Get user profile
    └── PUT    /:id               # Update user profile
```

---

## 5. DATABASE ARCHITECTURE

### 5.1 Database Schema Design

#### Users Collection
```javascript
{
  _id: ObjectId,
  name: String,                    // Required
  email: String,                   // Required, Unique, Indexed
  password: String,                // Required, Hashed
  role: String,                    // 'user' or 'admin', Default: 'user'
  createdAt: Date,                 // Timestamp
  updatedAt: Date                  // Timestamp
}
```

#### Flights Collection
```javascript
{
  _id: ObjectId,
  airline: String,                 // Required
  flightNumber: String,            // Required, Indexed
  origin: String,                  // Required
  destination: String,             // Required
  departureDate: Date,             // Required
  departureTime: String,           // Required
  arrivalTime: String,             // Required
  duration: String,                // Calculated or provided
  price: Number,                   // Required, Min: 0
  totalSeats: Number,              // Required, Min: 1
  availableSeats: Number,          // Required, Min: 0
  status: String,                  // 'active', 'cancelled'
  createdAt: Date,                 // Timestamp
  updatedAt: Date                  // Timestamp
}
```

#### Bookings Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,                // Ref: 'User', Required
  flightId: ObjectId,              // Ref: 'Flight', Required
  numberOfSeats: Number,           // Required, Min: 1, Max: 10
  totalPrice: Number,              // Required, Calculated
  bookingReference: String,        // Required, Unique, Indexed
  status: String,                  // 'confirmed', 'cancelled'
  bookingDate: Date,               // Timestamp of booking
  createdAt: Date,                 // Timestamp
  updatedAt: Date                  // Timestamp
}
```

### 5.2 Database Relationships

```
┌─────────────┐
│    Users    │
│             │
│  _id (PK)   │◄─────────┐
│  name       │          │
│  email      │          │
│  password   │          │
│  role       │          │
└─────────────┘          │
                         │ 1:N
                         │
                  ┌──────┴────────┐
                  │   Bookings    │
                  │               │
                  │  _id (PK)     │
                  │  userId (FK)  │
                  │  flightId (FK)│──────┐
                  │  numberOfSeats│      │
                  │  totalPrice   │      │
                  │  reference    │      │
                  └───────────────┘      │ N:1
                                         │
                                   ┌─────▼──────┐
                                   │  Flights   │
                                   │            │
                                   │  _id (PK)  │
                                   │  airline   │
                                   │  origin    │
                                   │  destination│
                                   │  price     │
                                   │  seats     │
                                   └────────────┘
```

### 5.3 Database Indexes

**Performance Optimization Indexes:**
```javascript
// Users Collection Indexes
{
  email: 1                         // Unique index for login
}

// Flights Collection Indexes
{
  flightNumber: 1,                 // For flight lookup
  departureDate: 1,                // For date-based queries
  origin: 1,                       // For search queries
  destination: 1                   // For search queries
}

// Bookings Collection Indexes
{
  bookingReference: 1,             // Unique index for lookup
  userId: 1,                       // For user bookings query
  flightId: 1,                     // For flight bookings query
  bookingDate: -1                  // For sorting by date
}

// Compound Indexes (Optional)
{
  origin: 1, destination: 1        // For route-based searches
}
```

---

## 6. SECURITY ARCHITECTURE

### 6.1 Authentication Flow

```
User Login Request
    │
    ▼
┌─────────────────────┐
│ Validate Input      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Query User by Email │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ User Exists?        │
└──────────┬──────────┘
           │ Yes
           ▼
┌─────────────────────┐
│ Compare Password    │
│ using Bcrypt        │
└──────────┬──────────┘
           │ Match
           ▼
┌─────────────────────┐
│ Generate JWT Token  │
│ - Payload: user ID, │
│   email, role       │
│ - Secret: from ENV  │
│ - Expires: 24h      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Send Token to Client│
└─────────────────────┘
           │
           ▼
Client Stores Token
(localStorage/cookie)
```

### 6.2 Authorization Flow

```
Protected Route Request
    │
    ▼
┌─────────────────────┐
│ Extract Token from  │
│ Authorization Header│
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Token Exists?       │
└──────────┬──────────┘
           │ Yes
           ▼
┌─────────────────────┐
│ Verify Token        │
│ using JWT Secret    │
└──────────┬──────────┘
           │ Valid
           ▼
┌─────────────────────┐
│ Decode Token        │
│ Extract User Data   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Check Role          │
│ (if admin required) │
└──────────┬──────────┘
           │ Authorized
           ▼
┌─────────────────────┐
│ Proceed to Route    │
│ Handler             │
└─────────────────────┘
```

### 6.3 Security Layers

```
┌─────────────────────────────────────────┐
│         Application Security            │
│                                         │
│  Layer 1: Input Validation              │
│  - Email format validation              │
│  - Password strength requirements       │
│  - Data type validation                 │
│  - Sanitization of inputs               │
│                                         │
│  Layer 2: Authentication                │
│  - Password hashing (bcrypt)            │
│  - JWT token generation                 │
│  - Token expiration                     │
│                                         │
│  Layer 3: Authorization                 │
│  - Role-based access control            │
│  - Route protection middleware          │
│  - Resource ownership verification      │
│                                         │
│  Layer 4: Data Protection               │
│  - HTTPS encryption                     │
│  - Environment variables                │
│  - Secure headers                       │
│  - CORS configuration                   │
│                                         │
│  Layer 5: Error Handling                │
│  - Graceful error responses             │
│  - No sensitive data in errors          │
│  - Error logging                        │
│                                         │
└─────────────────────────────────────────┘
```

---

## 7. DEPLOYMENT ARCHITECTURE

### 7.1 Development Environment

```
Developer Machine
│
├── Frontend (localhost:3000)
│   └── React Development Server
│
├── Backend (localhost:5000)
│   └── Node/Express Server
│
└── Database
    └── MongoDB Local or Atlas
```

### 7.2 Production Environment

```
                        CDN
                         │
                         │ Static Assets
                         │
┌────────────────────────▼──────────────────┐
│         Frontend Hosting                  │
│         (Netlify/Vercel)                  │
│                                           │
│  - React Build Files                      │
│  - Environment Variables                  │
│  - Automatic HTTPS                        │
│  - CDN Distribution                       │
└───────────────────────────────────────────┘
                         │
                         │ API Calls
                         │
┌────────────────────────▼──────────────────┐
│         Backend Hosting                   │
│         (Heroku/Render/Railway)           │
│                                           │
│  - Node.js Application                    │
│  - Environment Variables                  │
│  - Auto-scaling                           │
│  - SSL/TLS                                │
└─────────────────────┬─────────────────────┘
                      │
                      │ Database Connection
                      │
┌─────────────────────▼─────────────────────┐
│         Database Hosting                  │
│         (MongoDB Atlas)                   │
│                                           │
│  - Cloud Database Cluster                 │
│  - Automatic Backups                      │
│  - Replication                            │
│  - Monitoring                             │
└───────────────────────────────────────────┘
```

### 7.3 CI/CD Pipeline (Optional)

```
Code Push to GitHub
        │
        ▼
┌────────────────┐
│ GitHub Actions │ or ┌─────────────┐
│ Trigger        │    │ Automated   │
└────────┬───────┘    │ Deployment  │
         │            └─────────────┘
         ▼
┌────────────────┐
│ Run Tests      │
└────────┬───────┘
         │ Pass
         ▼
┌────────────────┐
│ Build Frontend │
└────────┬───────┘
         │
         ▼
┌────────────────────┐
│ Deploy Frontend to │
│ Netlify/Vercel     │
└────────────────────┘
         │
         ▼
┌────────────────────┐
│ Deploy Backend to  │
│ Heroku/Render      │
└────────────────────┘
```

---

## 8. SCALABILITY CONSIDERATIONS

### 8.1 Horizontal Scaling

```
                Load Balancer
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
   Server 1      Server 2      Server 3
   (Node.js)     (Node.js)     (Node.js)
        │             │             │
        └─────────────┼─────────────┘
                      │
                      ▼
                 MongoDB Cluster
              (Replica Set/Sharding)
```

### 8.2 Caching Strategy (Future)

```
Client Request
     │
     ▼
Cache Check (Redis)
     │
     ├─── Hit ──────────► Return Cached Data
     │
     └─── Miss
          │
          ▼
     Database Query
          │
          ▼
     Cache Result
          │
          ▼
     Return Data
```

---

## 9. MONITORING & LOGGING

### 9.1 Application Monitoring

```
┌─────────────────────────────────┐
│   Application Monitoring        │
│                                 │
│  - Error Tracking (Sentry)      │
│  - Performance Monitoring       │
│  - Uptime Monitoring            │
│  - API Response Times           │
│  - User Activity Tracking       │
└─────────────────────────────────┘
```

### 9.2 Logging Strategy

```
Application Logs
     │
     ├─── Error Logs
     │     │
     │     ├─── Server Errors (500+)
     │     ├─── Database Errors
     │     └─── Authentication Failures
     │
     ├─── Access Logs
     │     │
     │     ├─── API Requests
     │     ├─── Response Times
     │     └─── User Actions
     │
     └─── System Logs
           │
           ├─── Server Start/Stop
           ├─── Database Connections
           └─── Deployment Events
```

---

## 10. DISASTER RECOVERY

### 10.1 Backup Strategy

```
Database Backups
     │
     ├─── Automated Daily Backups (MongoDB Atlas)
     ├─── Point-in-Time Recovery
     ├─── Backup Retention: 7 days
     └─── Offsite Backup Storage
```

### 10.2 Recovery Plan

```
System Failure Detected
     │
     ▼
Assess Failure Type
     │
     ├─── Frontend Down
     │     └─── Redeploy from Git
     │
     ├─── Backend Down
     │     └─── Restart Server/Redeploy
     │
     └─── Database Issue
           └─── Restore from Backup
```

---

*Document prepared for: Flight Booking Application - MERN Stack*
*Version: 1.0*
*Date: February 2026*
*Status: Approved*
*Architecture Type: Three-Tier Web Application*
