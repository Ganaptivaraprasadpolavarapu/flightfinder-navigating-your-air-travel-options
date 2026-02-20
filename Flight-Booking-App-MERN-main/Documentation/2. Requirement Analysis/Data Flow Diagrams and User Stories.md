# Data Flow Diagrams and User Stories

## Flight Booking Application - MERN Stack

---

## PART 1: DATA FLOW DIAGRAMS

### 1.1 System Context Diagram (Level 0 DFD)

```
                                  ┌─────────────────────┐
                                  │                     │
                    ┌────────────►│   Regular User      │──────────┐
                    │             │                     │          │
                    │             └─────────────────────┘          │
                    │                                              │
                    │             ┌─────────────────────┐          │
                    │             │                     │          │
                    │    ┌───────►│   Administrator     │────┐     │
                    │    │        │                     │    │     │
                    │    │        └─────────────────────┘    │     │
                    │    │                                   │     │
                    │    │                                   │     │
                    │    │                                   │     │
           ┌────────┴────┴───────────────────────────────────┴─────┴────┐
           │                                                             │
           │         FLIGHT BOOKING APPLICATION SYSTEM                  │
           │                  (MERN Stack)                              │
           │                                                             │
           └────────────────────────────┬────────────────────────────────┘
                                       │
                                       │
                              ┌────────▼────────┐
                              │                 │
                              │  MongoDB        │
                              │  Database       │
                              │                 │
                              └─────────────────┘
```

**External Entities:**
- Regular User: Books flights, views bookings
- Administrator: Manages flights, users, and bookings
- Database: Stores all application data

---

### 1.2 Level 1 DFD - Main Processes

```
┌──────────┐                                                    ┌──────────┐
│          │    Registration/Login Data                         │          │
│  User    │───────────────────────────────────────────────────►│  1.0     │
│          │                                                     │  User    │
│          │◄───────────────────────────────────────────────────│  Auth    │
└──────────┘    Auth Token/Status                               │          │
                                                                 └────┬─────┘
                                                                      │
                                                                      │User Data
                                                                      │
      ┌───────────────────────────────────────────────────────────────▼──────┐
      │                         MongoDB Database                              │
      │  ┌──────────┐          ┌──────────┐          ┌──────────┐           │
      │  │  Users   │          │ Flights  │          │ Bookings │           │
      │  └──────────┘          └──────────┘          └──────────┘           │
      └───────▲──────────────────────▲─────────────────────▲─────────────────┘
              │                      │                     │
              │                      │                     │
┌──────────┐  │    User Profile      │  Flight List       │  Booking Data
│          │  │                      │                     │
│  User    │──┼──────────────────────┼─────────────────────┼────────────┐
│          │  │                      │                     │            │
│          │  │    ┌─────────────────▼────┐     ┌─────────▼────────┐   │
│          │  │    │  2.0                  │     │  3.0             │   │
└──────────┘  │    │  Flight               │     │  Booking         │   │
              │    │  Management           │     │  Management      │   │
              │    │                       │     │                  │   │
              │    └────────────┬──────────┘     └──────┬───────────┘   │
              │                 │                       │               │
              │                 │Flight Details         │Booking Info   │
              │                 │                       │               │
   ┌──────────┴─────┐          │                       │               │
   │  4.0           │◄─────────┴───────────────────────┴───────────────┘
   │  Admin         │
   │  Dashboard     │
   │                │
   └────────────────┘
        ▲
        │
        │Admin Actions
        │
   ┌────┴─────┐
   │  Admin   │
   │  User    │
   └──────────┘
```

---

### 1.3 Level 2 DFD - User Authentication Process (Process 1.0)

```
┌──────────┐
│  User    │
└────┬─────┘
     │
     │ Registration Data
     │ (email, password, name)
     │
     ▼
┌─────────────────────┐
│  1.1                │
│  Validate           │───────► Validation Errors
│  Input Data         │
└─────┬───────────────┘
      │ Valid Data
      │
      ▼
┌─────────────────────┐
│  1.2                │         ┌──────────────┐
│  Hash Password      │───────►│  bcrypt      │
│                     │         └──────────────┘
└─────┬───────────────┘
      │ Hashed Password
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  1.3                │────────►│  Users       │
│  Store User         │         │  Collection  │
│                     │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ User Created
      │
      ▼
┌─────────────────────┐
│  1.4                │
│  Login              │
│  Authenticate       │
└─────┬───────────────┘
      │ Credentials
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  1.5                │────────►│  Users       │
│  Verify Password    │         │  Collection  │
│                     │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Valid User
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  1.6                │────────►│  JWT         │
│  Generate Token     │         │  Library     │
│                     │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Token
      │
      ▼
┌─────────────────────┐
│  User               │
│  (Authenticated)    │
└─────────────────────┘
```

---

### 1.4 Level 2 DFD - Flight Management Process (Process 2.0)

```
┌──────────┐
│  User    │
└────┬─────┘
     │ Search Criteria
     │ (origin, destination, date)
     │
     ▼
┌─────────────────────┐         ┌──────────────┐
│  2.1                │────────►│  Flights     │
│  Search Flights     │         │  Collection  │
│                     │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Flight List
      │
      ▼
┌─────────────────────┐
│  2.2                │
│  Display Flights    │───────► User sees available flights
│                     │
└─────────────────────┘


┌──────────┐
│  Admin   │
└────┬─────┘
     │ Flight Data
     │ (airline, origin, destination, etc.)
     │
     ▼
┌─────────────────────┐
│  2.3                │
│  Validate Flight    │───────► Validation Errors
│  Data               │
└─────┬───────────────┘
      │ Valid Data
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  2.4                │────────►│  Flights     │
│  Add/Update Flight  │         │  Collection  │
│                     │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Confirmation
      │
      ▼
┌─────────────────────┐
│  Admin              │
│  (Success Message)  │
└─────────────────────┘
```

---

### 1.5 Level 2 DFD - Booking Management Process (Process 3.0)

```
┌──────────┐
│  User    │
└────┬─────┘
     │ Flight Selection
     │ Number of Seats
     │
     ▼
┌─────────────────────┐         ┌──────────────┐
│  3.1                │────────►│  Flights     │
│  Check              │         │  Collection  │
│  Availability       │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Available Seats
      │
      ▼
┌─────────────────────┐
│  3.2                │
│  Calculate Total    │───────► Display Price
│  Price              │
└─────┬───────────────┘
      │ Total Amount
      │
      ▼
┌─────────────────────┐
│  3.3                │
│  Confirm Booking    │
│                     │
└─────┬───────────────┘
      │ Booking Data
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  3.4                │────────►│  Bookings    │
│  Create Booking     │         │  Collection  │
│  Record             │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Booking Created
      │
      ▼
┌─────────────────────┐         ┌──────────────┐
│  3.5                │────────►│  Flights     │
│  Update Seat        │         │  Collection  │
│  Availability       │◄────────│              │
└─────┬───────────────┘         └──────────────┘
      │ Confirmation
      │
      ▼
┌─────────────────────┐
│  3.6                │
│  Generate Booking   │───────► Booking Reference Number
│  Reference          │
└─────┬───────────────┘
      │
      ▼
┌─────────────────────┐
│  User               │
│  (Booking           │
│   Confirmation)     │
└─────────────────────┘
```

---

## PART 2: USER STORIES

### 2.1 User Registration and Authentication

#### Story 1: User Registration
**As a** new user  
**I want to** register with my email and password  
**So that** I can create an account and book flights

**Acceptance Criteria:**
- User can enter email, password, and name
- System validates email format
- System validates password strength (minimum 6 characters)
- System prevents duplicate email registration
- User receives success confirmation
- User is redirected to login page

**Priority:** HIGH  
**Story Points:** 5

---

#### Story 2: User Login
**As a** registered user  
**I want to** login with my credentials  
**So that** I can access my account and book flights

**Acceptance Criteria:**
- User can enter email and password
- System validates credentials
- User receives JWT token upon successful login
- User is redirected to flights page
- Error message shown for invalid credentials
- Session is maintained across page navigation

**Priority:** HIGH  
**Story Points:** 5

---

#### Story 3: User Logout
**As a** logged-in user  
**I want to** logout from my account  
**So that** I can securely end my session

**Acceptance Criteria:**
- Logout button is visible when logged in
- Clicking logout clears session
- User is redirected to landing page
- Protected pages are no longer accessible

**Priority:** MEDIUM  
**Story Points:** 2

---

### 2.2 Flight Search and Viewing

#### Story 4: View All Flights
**As a** user  
**I want to** view all available flights  
**So that** I can see what options are available

**Acceptance Criteria:**
- All flights are displayed in a list/card format
- Each flight shows: airline, origin, destination, date, time, price, available seats
- Flights are loaded from database
- Empty state is shown if no flights available
- Loading indicator is shown while fetching data

**Priority:** HIGH  
**Story Points:** 5

---

#### Story 5: Search Flights
**As a** user  
**I want to** search flights by origin, destination, and date  
**So that** I can find flights that match my travel plans

**Acceptance Criteria:**
- Search form has fields for origin, destination, and date
- Search updates results in real-time
- Results show only matching flights
- Clear indication when no flights match criteria
- Search is case-insensitive

**Priority:** MEDIUM  
**Story Points:** 8

---

### 2.3 Flight Booking

#### Story 6: Book a Flight
**As a** logged-in user  
**I want to** book a flight by selecting number of seats  
**So that** I can reserve my travel

**Acceptance Criteria:**
- User can select number of seats (1-10)
- System checks seat availability before booking
- Total price is calculated and displayed
- Booking is saved to database
- Available seats are updated after booking
- User receives booking confirmation with reference number
- Error shown if not enough seats available

**Priority:** HIGH  
**Story Points:** 8

---

#### Story 7: View My Bookings
**As a** logged-in user  
**I want to** view all my past and current bookings  
**So that** I can keep track of my travel plans

**Acceptance Criteria:**
- User can see list of all their bookings
- Each booking shows: flight details, booking date, number of seats, total price, booking reference
- Bookings are sorted by date (newest first)
- Empty state shown if no bookings exist
- Booking details are clickable for more information

**Priority:** MEDIUM  
**Story Points:** 5

---

### 2.4 Admin - Flight Management

#### Story 8: Add New Flight (Admin)
**As an** administrator  
**I want to** add new flights to the system  
**So that** users can book them

**Acceptance Criteria:**
- Admin can access flight management page
- Form includes: airline, flight number, origin, destination, date, time, price, total seats
- All fields are validated
- Flight is saved to database
- Success message is displayed
- Admin is redirected to flights list

**Priority:** HIGH  
**Story Points:** 8

---

#### Story 9: Edit Flight (Admin)
**As an** administrator  
**I want to** edit existing flight details  
**So that** I can correct errors or update information

**Acceptance Criteria:**
- Admin can click edit on any flight
- Form is pre-filled with existing data
- Admin can modify any field
- Changes are validated
- Updated flight is saved to database
- Existing bookings are not affected negatively
- Success message is displayed

**Priority:** MEDIUM  
**Story Points:** 8

---

#### Story 10: Delete Flight (Admin)
**As an** administrator  
**I want to** delete flights from the system  
**So that** I can remove cancelled or outdated flights

**Acceptance Criteria:**
- Admin can click delete on any flight
- Confirmation dialog is shown before deletion
- System checks if flight has bookings
- Warning shown if bookings exist
- Flight is removed from database
- Success message is displayed
- Flights list is updated

**Priority:** MEDIUM  
**Story Points:** 5

---

### 2.5 Admin - User and Booking Management

#### Story 11: View All Users (Admin)
**As an** administrator  
**I want to** view all registered users  
**So that** I can manage the user base

**Acceptance Criteria:**
- Admin can access users page
- All users are listed with: name, email, registration date
- Users can be filtered or searched
- User count is displayed
- Admin cannot see passwords

**Priority:** LOW  
**Story Points:** 5

---

#### Story 12: View All Bookings (Admin)
**As an** administrator  
**I want to** view all bookings in the system  
**So that** I can monitor booking activity

**Acceptance Criteria:**
- Admin can access all bookings page
- All bookings are listed with: user, flight, date, seats, price
- Bookings can be filtered by user or flight
- Total bookings count is displayed
- Booking statistics are shown (total revenue, total bookings)

**Priority:** MEDIUM  
**Story Points:** 8

---

### 2.6 Navigation and UI

#### Story 13: Responsive Navigation
**As a** user  
**I want to** easily navigate the application  
**So that** I can access different features quickly

**Acceptance Criteria:**
- Navigation bar is visible on all pages
- Navigation shows different options based on login status
- Navigation shows different options based on user role (admin/user)
- Active page is highlighted in navigation
- Navigation is responsive on mobile devices
- Logo/brand name links to home page

**Priority:** HIGH  
**Story Points:** 5

---

#### Story 14: Landing Page
**As a** visitor  
**I want to** see an attractive landing page  
**So that** I understand what the application offers

**Acceptance Criteria:**
- Landing page has welcoming design
- Clear calls-to-action for login/register
- Brief description of features
- Responsive design for all devices
- Fast loading time

**Priority:** MEDIUM  
**Story Points:** 5

---

### 2.7 Error Handling and Validation

#### Story 15: Form Validation
**As a** user  
**I want to** see clear error messages for invalid inputs  
**So that** I can correct my mistakes easily

**Acceptance Criteria:**
- All forms have real-time validation
- Error messages are clear and specific
- Invalid fields are highlighted
- Submit button is disabled during processing
- Success feedback is provided after successful submission

**Priority:** HIGH  
**Story Points:** 8

---

## 3. USER STORY SUMMARY

### 3.1 Priority Breakdown
- **HIGH Priority:** 8 stories
- **MEDIUM Priority:** 6 stories
- **LOW Priority:** 1 story

### 3.2 Total Story Points: 94

### 3.3 Feature Categories
- **Authentication:** 3 stories (12 points)
- **Flight Viewing:** 2 stories (13 points)
- **Booking:** 2 stories (13 points)
- **Admin - Flights:** 3 stories (21 points)
- **Admin - Management:** 2 stories (13 points)
- **UI/Navigation:** 2 stories (10 points)
- **Validation:** 1 story (8 points)

---

## 4. USER JOURNEY MAPS

### 4.1 User Journey: Booking a Flight

```
Step 1: Discover
- User visits landing page
- Sees flight booking features
- Decides to register/login

Step 2: Register/Login
- Fills registration form
- Receives confirmation
- Logs in with credentials

Step 3: Search
- Views available flights
- Searches by criteria
- Compares options

Step 4: Select
- Chooses preferred flight
- Selects number of seats
- Reviews total price

Step 5: Book
- Confirms booking
- Receives booking reference
- Sees confirmation message

Step 6: Verify
- Views booking history
- Confirms booking details
- Saves booking information
```

### 4.2 Admin Journey: Managing Flights

```
Step 1: Login
- Admin logs in with credentials
- Accesses admin dashboard

Step 2: Navigate
- Views admin menu options
- Selects flight management

Step 3: Manage
- Views all flights
- Adds new flight
- Edits existing flight
- Deletes outdated flight

Step 4: Monitor
- Checks bookings
- Reviews user activity
- Views statistics

Step 5: Maintain
- Updates flight information
- Manages flight requests
- Ensures data accuracy
```

---

*Document prepared for: Flight Booking Application - MERN Stack*
*Version: 1.0*
*Date: February 2026*
*Status: Approved*
