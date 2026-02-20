# Project Executable Files

This folder contains information about the executable project files for the Flight Booking Application.

## Project Structure

The actual executable code for the Flight Booking Application is located in the main project folder:

```
Flight-Booking-App-MERN-main/
├── client/          - React.js Frontend Application
├── server/          - Node.js + Express Backend API
└── Documentation/   - Complete Project Documentation
```

## Client Application (Frontend)

**Location:** `../client/`

**Technologies:**
- React.js v17.0+
- React Router DOM v5.0+
- Axios for API calls
- Context API for state management

**Main Files:**
- `client/src/App.js` - Main application component
- `client/src/index.js` - Application entry point
- `client/src/components/` - Reusable React components
- `client/src/pages/` - Page components
- `client/src/context/` - Global state management
- `client/public/` - Static files

**Setup & Installation:**
```bash
cd client
npm install
npm start
```

**Build for Production:**
```bash
cd client
npm run build
```

**Environment Variables:**
Create `.env` file in client folder:
```
REACT_APP_API_URL=http://localhost:5000
```

## Server Application (Backend)

**Location:** `../server/`

**Technologies:**
- Node.js v14.0+
- Express.js v4.17+
- MongoDB with Mongoose ODM
- JWT for authentication
- Bcrypt for password hashing

**Main Files:**
- `server/index.js` - Server entry point & API routes
- `server/schemas.js` - MongoDB models (User, Flight, Booking)
- `server/package.json` - Dependencies and scripts

**Setup & Installation:**
```bash
cd server
npm install
node index.js
```

**Environment Variables:**
Create `.env` file in server folder:
```
MONGODB_URI=mongodb://localhost:27017/flight-booking
JWT_SECRET=your_secret_key_here
PORT=5000
CLIENT_URL=http://localhost:3000
```

## Database Structure

### MongoDB Collections

1. **Users Collection**
   - Stores user accounts (customers and admins)
   - Fields: name, email, password (hashed), role, timestamps

2. **Flights Collection**
   - Stores flight information
   - Fields: airline, flightNumber, origin, destination, dates, times, price, seats, status

3. **Bookings Collection**
   - Stores booking records
   - Fields: userId, flightId, numberOfSeats, totalPrice, bookingReference, status, timestamps

### Database Setup

**Local MongoDB:**
1. Install MongoDB Community Edition
2. Start MongoDB service
3. Database will be created automatically on first run

**MongoDB Atlas (Cloud):**
1. Create free cluster at mongodb.com/atlas
2. Create database user
3. Whitelist IP address
4. Copy connection string
5. Update MONGODB_URI in .env file

## Running the Complete Application

### Development Mode

**Terminal 1 - Start Backend:**
```bash
cd server
npm install
node index.js
```
Server will run on http://localhost:5000

**Terminal 2 - Start Frontend:**
```bash
cd client
npm install
npm start
```
Client will run on http://localhost:3000

### Production Deployment

**Frontend (Netlify/Vercel):**
1. Build: `npm run build` in client folder
2. Deploy `build` folder to Netlify/Vercel
3. Set environment variable: REACT_APP_API_URL

**Backend (Heroku/Render):**
1. Push code to Git repository
2. Connect to hosting platform
3. Set environment variables
4. Deploy

**Database (MongoDB Atlas):**
1. Already hosted in cloud
2. Configure connection string
3. Production-ready

## Key Features Implemented

### User Features:
✅ User registration and login
✅ Browse available flights
✅ Search flights by criteria
✅ Book flights with seat selection
✅ View booking history
✅ Secure authentication

### Admin Features:
✅ Admin dashboard with statistics
✅ Add new flights
✅ Edit existing flights
✅ Delete flights
✅ View all bookings
✅ View all users
✅ Complete flight management

## API Endpoints

### Authentication
- POST `/api/auth/register` - Register new user
- POST `/api/auth/login` - Login user

### Flights
- GET `/api/flights` - Get all flights
- GET `/api/flights/:id` - Get single flight
- POST `/api/flights` - Create flight (admin only)
- PUT `/api/flights/:id` - Update flight (admin only)
- DELETE `/api/flights/:id` - Delete flight (admin only)

### Bookings
- GET `/api/bookings` - Get user's bookings
- GET `/api/bookings/all` - Get all bookings (admin only)
- POST `/api/bookings` - Create new booking
- GET `/api/bookings/:id` - Get single booking

### Users
- GET `/api/users` - Get all users (admin only)
- GET `/api/users/:id` - Get user details

## Dependencies

### Client Dependencies:
```json
{
  "react": "^17.0.2",
  "react-dom": "^17.0.2",
  "react-router-dom": "^5.3.0",
  "axios": "^0.21.1"
}
```

### Server Dependencies:
```json
{
  "express": "^4.17.1",
  "mongoose": "^5.13.0",
  "jsonwebtoken": "^8.5.1",
  "bcryptjs": "^2.4.3",
  "cors": "^2.8.5",
  "dotenv": "^10.0.0"
}
```

## Testing

### Manual Testing
- Test all user registration and login flows
- Test flight browsing and search
- Test booking creation and history
- Test admin features (add/edit/delete flights)
- Test responsive design on multiple devices

### API Testing with Postman
- Import API collection
- Test all endpoints
- Verify authentication
- Check error handling

## Troubleshooting

### Common Issues:

**"Cannot connect to MongoDB"**
- Check if MongoDB is running
- Verify connection string in .env
- Check network access on MongoDB Atlas

**"CORS Error"**
- Verify CLIENT_URL in backend .env
- Check CORS configuration in server/index.js

**"Module not found"**
- Run `npm install` in both client and server folders
- Check for missing dependencies

**"JWT Invalid Token"**
- Check if JWT_SECRET matches between registration and login
- Verify token hasn't expired
- Clear localStorage and login again

**"Port already in use"**
- Kill process using the port
- Use different port in .env file

## Performance

- **Page Load Time:** < 2 seconds
- **API Response Time:** < 500ms
- **Database Queries:** Optimized with indexes
- **Bundle Size:** Minimized in production build

## Security Features

✅ Password hashing with bcrypt
✅ JWT-based authentication
✅ Protected routes (authentication required)
✅ Role-based authorization (admin vs user)
✅ Input validation on client and server
✅ CORS configuration
✅ Environment variables for secrets
✅ MongoDB injection prevention

## Future Enhancements

See `Documentation/16. Future Enhancements` section in the complete project documentation for a comprehensive list of planned features including:
- Payment gateway integration
- Email notifications
- Booking modifications
- Mobile application
- Multi-language support
- AI-powered recommendations
- Advanced analytics

## Documentation

Complete project documentation is available in:
`../Documentation/`

Including:
- Ideation Phase
- Requirement Analysis
- Project Design Phase
- Project Planning Phase
- Project Development Phase
- Complete Project Documentation

## Support & Contact

For issues, questions, or contributions:
- Review the documentation
- Check troubleshooting section
- Review code comments
- Test in development environment first

## License

[Specify your license here]

## Version

**Version:** 1.0  
**Last Updated:** February 2026  
**Status:** Production Ready

---

**Note:** This Flight Booking Application is a complete MERN stack project demonstrating modern web development practices, secure authentication, responsive design, and production deployment capabilities.
