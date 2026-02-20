# Planning Logic

## Flight Booking Application - Development Planning & Logic

---

## 1. PROJECT PLANNING LOGIC

### 1.1 Why MERN Stack?

The MERN stack was chosen based on the following logical considerations:

#### **JavaScript Everywhere**
- **Logic:** Single language (JavaScript) for both frontend and backend reduces context switching and learning curve
- **Benefit:** Faster development, easier maintenance, consistent codebase

#### **Component-Based Architecture**
- **Logic:** React's component model allows for reusable UI elements
- **Benefit:** Reduced code duplication, easier testing, faster development

#### **NoSQL Flexibility**
- **Logic:** Flight and booking data has varying attributes; NoSQL provides flexibility
- **Benefit:** Easy schema evolution, faster iterations, JSON-like data structure

#### **Performance**
- **Logic:** Node.js event-driven architecture handles concurrent requests efficiently
- **Benefit:** Good performance for I/O operations, scalability

#### **Rich Ecosystem**
- **Logic:** Large npm ecosystem provides ready-made solutions
- **Benefit:** Faster development, proven libraries, community support

---

## 2. FEATURE PRIORITIZATION LOGIC

### 2.1 MoSCoW Method Applied

#### **Must Have (P0 - Critical)**
1. **User Authentication**
   - **Logic:** Security foundation for entire application
   - **Impact:** Without it, no personalization or booking tracking
   - **Implementation Order:** First

2. **Flight Viewing**
   - **Logic:** Core functionality - users must see available flights
   - **Impact:** Primary user need
   - **Implementation Order:** Second

3. **Flight Booking**
   - **Logic:** Main application purpose
   - **Impact:** Without booking, application serves no purpose
   - **Implementation Order:** Third

4. **Admin Flight Management**
   - **Logic:** Without adding flights, users have nothing to book
   - **Impact:** Content management essential
   - **Implementation Order:** Fourth

#### **Should Have (P1 - Important)**
1. **Flight Search/Filter**
   - **Logic:** Improves user experience significantly
   - **Impact:** Users can find relevant flights faster
   - **Implementation Order:** Fifth

2. **Booking History**
   - **Logic:** Users need to track their bookings
   - **Impact:** User convenience and trust
   - **Implementation Order:** Sixth

3. **Admin User Management**
   - **Logic:** Admins need oversight of users
   - **Impact:** System management capability
   - **Implementation Order:** Seventh

#### **Could Have (P2 - Nice to Have)**
1. **Advanced Search Filters**
   - **Logic:** Enhanced user experience
   - **Impact:** Convenience but not critical
   - **Implementation Order:** If time permits

2. **Booking Statistics**
   - **Logic:** Business intelligence
   - **Impact:** Useful for admins but not essential for MVP
   - **Implementation Order:** If time permits

#### **Won't Have (P3 - Future)**
1. **Payment Integration**
   - **Logic:** Complex, time-consuming, requires compliance
   - **Impact:** Can be simulated in MVP
   - **Reason for Exclusion:** Adds significant complexity

2. **Email Notifications**
   - **Logic:** Requires external service integration
   - **Impact:** Nice to have but not critical for core functionality
   - **Reason for Exclusion:** Time constraint

3. **Real-time Updates**
   - **Logic:** Requires WebSocket implementation
   - **Impact:** Enhancement, not requirement
   - **Reason for Exclusion:** Adds complexity

---

## 3. DEVELOPMENT SEQUENCE LOGIC

### 3.1 Backend-First Approach

**Logic:** Build data layer and API before UI

**Reasoning:**
1. **Foundation First:** Backend provides data structure and rules
2. **Testing:** APIs can be tested independently with Postman
3. **Parallel Work:** Frontend can use mock data initially
4. **Stability:** Backend changes are more disruptive than frontend changes

**Sequence:**
```
Database Schema → Models → Routes → Controllers → Middleware → Testing
```

### 3.2 Frontend Development Sequence

**Logic:** Follow user journey from landing to booking

**Reasoning:**
1. **User-Centric:** Matches how users will interact
2. **Dependencies:** Each feature builds on previous
3. **MVP Path:** Creates functional path first

**Sequence:**
```
Landing Page → Auth (Login/Register) → Flights View → Booking → History → Admin
```

---

## 4. DATABASE DESIGN LOGIC

### 4.1 Collection Structure

#### **Users Collection**
**Logic:** Separate collection for authentication and profile data

**Reasoning:**
- Independent entity that doesn't change frequently
- Referenced by bookings
- Security: Passwords isolated
- Scalability: Can add user attributes without affecting other collections

**Fields Logic:**
- `email` (unique): Natural identifier for users
- `password` (hashed): Security requirement
- `role`: Simple RBAC implementation
- `name`: Personalization

#### **Flights Collection**
**Logic:** Independent collection for flight inventory

**Reasoning:**
- Flights exist independently of bookings
- Multiple bookings can reference same flight
- Admins manage flight data separately
- Scalability: Can add flight attributes easily

**Fields Logic:**
- `airline, origin, destination`: Search criteria
- `totalSeats, availableSeats`: Inventory management
- `price`: Booking calculation
- `departureDate, departureTime`: Scheduling

#### **Bookings Collection**
**Logic:** Junction collection linking users and flights

**Reasoning:**
- Many-to-many relationship (users can book multiple flights, flights can have multiple bookings)
- Stores transaction-specific data (date, seats, price at time of booking)
- Allows booking history tracking
- Enables analytics

**Fields Logic:**
- `userId, flightId`: Relationships
- `numberOfSeats`: Transaction detail
- `totalPrice`: Snapshot of price paid
- `bookingReference`: Unique identifier for customer support
- `bookingDate`: Timestamp for history

### 4.2 Referencing vs Embedding

**Decision:** Use referencing (ObjectId) instead of embedding

**Logic:**
- **Users ← Bookings:** Referenced because user data should remain consistent across all bookings
- **Flights ← Bookings:** Referenced because flight data should remain consistent and updateable
- **Benefit:** Data integrity, easier updates, no duplication

**Exception Consideration:**
- Could embed basic flight info in bookings for historical record
- **Decision:** Reference only for MVP; can enhance later with hybrid approach

---

## 5. API DESIGN LOGIC

### 5.1 RESTful Principles

**Logic:** Follow REST conventions for predictable API

**Reasoning:**
- Industry standard
- Easy to understand and document
- Clear resource mapping
- Standard HTTP methods

**Pattern:**
```
GET    /api/resource        → List all
GET    /api/resource/:id    → Get one
POST   /api/resource        → Create
PUT    /api/resource/:id    → Update
DELETE /api/resource/:id    → Delete
```

### 5.2 Authentication Strategy

**Decision:** JWT (JSON Web Tokens)

**Logic:**
- **Stateless:** Server doesn't store session data
- **Scalable:** No server-side session management
- **Cross-Platform:** Works with any client
- **Standard:** Industry-accepted solution

**Flow Logic:**
```
Login → Verify Credentials → Generate Token → Send to Client
Client Stores Token → Include in Requests → Server Validates → Grant Access
```

### 5.3 Endpoint Grouping

**Logic:** Group by resource type

**Reasoning:**
- Clear organization
- Easy to maintain
- Scalable structure
- Clear responsibility

**Groups:**
- `/api/auth/*` - Authentication operations
- `/api/flights/*` - Flight operations
- `/api/bookings/*` - Booking operations
- `/api/users/*` - User operations

---

## 6. SECURITY LOGIC

### 6.1 Authentication Security

#### **Password Hashing**
**Decision:** Use bcrypt

**Logic:**
- Industry standard
- Salt automatically included
- Configurable work factor
- Resistant to rainbow table attacks

**Implementation:**
```javascript
// Hash on registration
const hashedPassword = await bcrypt.hash(password, 10);

// Verify on login
const isValid = await bcrypt.compare(password, user.password);
```

#### **JWT Token**
**Decision:** 24-hour expiration

**Logic:**
- **Security:** Limited window if token compromised
- **Usability:** Users don't need to login too frequently
- **Balance:** Compromise between security and convenience

**Token Payload:**
```javascript
{
  userId: user._id,
  email: user.email,
  role: user.role
}
```
**Reasoning:** Minimal info to identify and authorize user

### 6.2 Authorization Logic

#### **Role-Based Access Control (RBAC)**
**Decision:** Two roles: 'user' and 'admin'

**Logic:**
- Simple to implement
- Covers all use cases for MVP
- Easy to extend later

**Permission Matrix:**
| Action | User | Admin |
|--------|------|-------|
| View Flights | ✓ | ✓ |
| Book Flight | ✓ | ✓ |
| View Own Bookings | ✓ | ✓ |
| Add/Edit/Delete Flights | ✗ | ✓ |
| View All Bookings | ✗ | ✓ |
| View All Users | ✗ | ✓ |

### 6.3 Input Validation Logic

**Decision:** Validate on both client and server

**Logic:**
- **Client-Side:** Immediate feedback, better UX
- **Server-Side:** Security, never trust client
- **Together:** Best of both worlds

**Validation Layers:**
1. **Frontend:** Real-time form validation
2. **Backend:** Schema validation (Mongoose)
3. **Backend:** Custom business logic validation

---

## 7. STATE MANAGEMENT LOGIC

### 7.1 React Context API

**Decision:** Use Context API instead of Redux

**Logic:**
- **Sufficient:** Application state is not overly complex
- **Built-in:** No additional library needed
- **Performance:** Good enough for this scale
- **Learning Curve:** Simpler than Redux

**State Structure:**
```javascript
{
  user: { /* user object */ },
  token: "JWT token",
  isAuthenticated: boolean,
  role: "user" or "admin"
}
```

**Reasoning:** Minimal state needed - primarily authentication

### 7.2 Where State Lives

**Decision:** Context for global state, Component state for local

**Logic:**
| Data Type | Storage | Reasoning |
|-----------|---------|-----------|
| Auth info | Context | Needed by many components |
| Flight list | Component state | Fetched when needed |
| Form inputs | Component state | Local to component |
| Booking data | Component state | Temporary, submitted then cleared |

---

## 8. UI/UX PLANNING LOGIC

### 8.1 Page Structure

**Decision:** Clear separation between user and admin interfaces

**Logic:**
- **Different Goals:** Users book, admins manage
- **Different Needs:** Different information display
- **Security:** Clear access boundaries

**Navigation Logic:**
```
Unauthenticated → Landing, Login, Register
User → Flights, Book Flight, My Bookings
Admin → All above + Admin Dashboard, Manage Flights, View All Bookings, View Users
```

### 8.2 Responsive Design

**Decision:** Mobile-first approach

**Logic:**
- **Mobile Traffic:** Increasing mobile usage
- **Progressive Enhancement:** Easier to scale up than down
- **Modern Practice:** Industry standard

**Breakpoints:**
- Mobile: 320px - 768px
- Tablet: 768px - 1024px
- Desktop: 1024px+

---

## 9. ERROR HANDLING LOGIC

### 9.1 Error Categories

**Decision:** Categorize errors by type

**Logic:** Different errors need different handling

**Categories:**
1. **Validation Errors (400):** User input issues
   - Show field-specific messages
   - Prevent form submission

2. **Authentication Errors (401):** Not logged in
   - Redirect to login
   - Show login prompt

3. **Authorization Errors (403):** Insufficient permissions
   - Show access denied message
   - Redirect to appropriate page

4. **Not Found Errors (404):** Resource doesn't exist
   - Show friendly "not found" message

5. **Server Errors (500):** System issues
   - Show generic error message
   - Log for debugging

### 9.2 Error Response Format

**Decision:** Consistent error structure

**Logic:** Frontend can predictably handle errors

**Format:**
```javascript
{
  success: false,
  message: "Human-readable error message",
  error: "Error code or type",
  details: { /* Optional additional info */ }
}
```

---

## 10. TESTING LOGIC

### 10.1 Testing Strategy Priority

**Decision:** Manual testing for MVP, automated later

**Logic:**
- **Time Constraint:** Automated tests take time to write
- **MVP Focus:** Need working product first
- **Future Investment:** Add automated tests in next phase

**Testing Priority:**
1. **Critical Path:** Register → Login → View Flights → Book → View Bookings
2. **Admin Path:** Login as Admin → Add Flight → Edit Flight → View Bookings
3. **Edge Cases:** Error handling, validation
4. **Cross-Browser:** Chrome, Firefox, Safari, Edge
5. **Responsive:** Mobile, tablet, desktop

### 10.2 Test Data Strategy

**Decision:** Create seed data script

**Logic:**
- **Consistency:** Same test data every time
- **Efficiency:** Quick database reset
- **Coverage:** Variety of scenarios

**Seed Data:**
- 5-10 sample flights (various routes, prices, dates)
- 2 admin users
- 5 regular users
- 10-15 sample bookings

---

## 11. DEPLOYMENT LOGIC

### 11.1 Deployment Strategy

**Decision:** Separate hosting for frontend and backend

**Logic:**
- **Specialization:** Each platform optimized for its purpose
- **Scaling:** Can scale independently
- **Cost:** Free tiers available
- **Reliability:** Platform-specific optimizations

**Architecture:**
```
Frontend (Netlify/Vercel) → API calls → Backend (Heroku/Render) → Database (MongoDB Atlas)
```

### 11.2 Environment Configuration

**Decision:** Different configs for development and production

**Logic:**
| Config | Development | Production |
|--------|-------------|------------|
| API URL | localhost:5000 | deployed backend URL |
| Database | Local or Atlas | MongoDB Atlas |
| CORS | Allow localhost | Allow specific frontend domain |
| Logging | Verbose | Minimal |
| Error Details | Full stack trace | Generic messages |

---

## 12. PERFORMANCE CONSIDERATIONS

### 12.1 Optimization Decisions

#### **Database Indexing**
**Decision:** Index frequently queried fields

**Logic:**
- `email` in Users: Used for login
- `flightNumber` in Flights: Used for lookup
- `bookingReference` in Bookings: Used for lookup
- **Impact:** Faster queries at minimal storage cost

#### **Response Pagination**
**Decision:** Not implemented in MVP

**Logic:**
- **Current Scale:** Expected < 100 flights initially
- **Performance:** Not an issue yet
- **Future:** Add when needed (>500 records)

#### **Image Optimization**
**Decision:** No flight images in MVP

**Logic:**
- **Scope:** Focuses on functionality
- **Performance:** Faster loading
- **Future Enhancement:** Add optimized images later

---

## 13. SCALABILITY PLANNING

### 13.1 Design for Scalability

**Decisions Made with Future Growth in Mind:**

1. **Stateless Backend**
   - Logic: Easy to add more servers
   - Benefit: Horizontal scaling ready

2. **Modular Code Structure**
   - Logic: Easy to separate into microservices
   - Benefit: Can split services as needed

3. **Cloud Database**
   - Logic: MongoDB Atlas can scale automatically
   - Benefit: No manual database scaling needed

4. **Indexed Database**
   - Logic: Prepared for larger datasets
   - Benefit: Performance maintained with growth

### 13.2 What Can Wait

**Not Implementing Now:**
- Caching layer (Redis)
- Load balancing
- Database sharding
- CDN for assets
- Microservices architecture

**Logic:** Premature optimization is wasteful; implement when needed

---

## 14. TIMELINE LOGIC

### 14.1 Why This Order?

**Week 1-2: Planning**
- **Logic:** Solid plan prevents rework
- **Critical:** Foundation for everything

**Week 3-5: Backend**
- **Logic:** Data layer must exist first
- **Benefit:** Can test independently

**Week 6-8: Frontend**
- **Logic:** Build on stable backend
- **Benefit:** Clear API contracts

**Week 9-10: Integration & Testing**
- **Logic:** Connect pieces and validate
- **Benefit:** Catch issues before production

**Week 10-11: Deployment**
- **Logic:** Final step when everything works
- **Benefit:** Smooth launch

### 14.2 Buffer Time

**Decision:** 1-2 weeks buffer included

**Logic:**
- **Reality:** Things take longer than estimated
- **Risks:** Unexpected issues arise
- **Quality:** Allows time for polish

---

## 15. SUCCESS METRICS LOGIC

### 15.1 How to Measure Success

**Technical Metrics:**
- **Load Time < 2s:** User expectation threshold
- **API Response < 500ms:** Feels instantaneous
- **99% Uptime:** Industry standard for reliability

**User Metrics:**
- **Booking Completion > 70%:** Indicates good UX
- **Return Rate > 40%:** Indicates satisfaction

**Logic:** Metrics are measurable and achievable

---

*Document prepared for: Flight Booking Application - MERN Stack*
*Version: 1.0*
*Date: February 2026*
*Purpose: Explaining the reasoning behind planning decisions*
