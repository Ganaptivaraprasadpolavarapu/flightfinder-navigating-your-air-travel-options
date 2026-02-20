# flightfinder-navigating-your-air-travel-options
# ✈️ FlightFinder – Navigating Your Air Travel Options

FlightFinder is a full-stack **Flight Booking Web Application** developed using the **MERN stack (MongoDB, Express.js, React.js, and Node.js)**.  
The application provides a smooth and user-friendly platform for users to search, book, and manage flight tickets, while admins can manage flights and monitor bookings.

This project was developed as part of the **SmartBridge Internship Program** to gain real-world experience in full-stack web development.

---

## 📌 Project Description

FlightFinder simplifies the flight booking process by allowing users to easily search for flights based on their preferences such as source, destination, date, and travel class.  
Users can book flights, view booking history, and cancel bookings.  
Admins have access to manage flights, update flight details, and monitor all user bookings.

The project follows a clean **frontend–backend separation** with REST APIs and a NoSQL database.

---

## 🚀 Features

### 👤 User Features
- User Registration and Login
- Search flights by source, destination, and date
- View available flight details
- Book flight tickets
- View booking history
- Cancel booked flights

### 🛠️ Admin Features
- Admin authentication
- Add new flights
- Update flight details
- View all bookings
- Manage users and flight availability

---

## 🧱 Tech Stack

### Frontend
- React.js
- Axios
- Bootstrap
- HTML, CSS, JavaScript

### Backend
- Node.js
- Express.js
- JWT Authentication

### Database
- MongoDB
- Mongoose ODM

### Tools
- MongoDB Compass
- Postman
- Git & GitHub
- Visual Studio Code

---

## 📂 Project Structure
FlightFinder
├── client # React frontend
├── server # Node.js & Express backend
├── README.md

## ⚙️ Installation & Setup

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/saikrishnavankayala/VANKAYALA-GANESH-SIVA-SAI-KRISHNA-flightfinder-navigating-your-air-travel-options.git
cd VANKAYALA-GANESH-SIVA-SAI-KRISHNA-flightfinder-navigating-your-air-travel-options
2️⃣ Start MongoDB

Make sure MongoDB is running locally:

mongod


You can verify using MongoDB Compass:

mongodb://127.0.0.1:27017

3️⃣ Run Backend Server
cd server
npm install
node index.js


Backend runs on:

http://localhost:6001

4️⃣ Run Frontend
cd client
npm install
npm start


Frontend runs on:

http://localhost:3000

🔁 Application Flow

User registers or logs in

User searches for flights

Backend fetches flight data from MongoDB

User selects and books a flight

Booking details are stored in the database

User can view or cancel bookings

Admin can add/update flights and monitor bookings

🗄️ Database Design
Collections Used:

Users – stores user and admin details

Flights – stores flight information

Bookings – stores booking details linking users and flights

Relationships:

One user can have multiple bookings

One flight can be booked by multiple users

Booking acts as a link between user and flight

📈 Learning Outcomes

Hands-on experience with MERN stack

REST API development

MongoDB schema design

Frontend and backend integration

Authentication and authorization

Real-world project structure and flow

👨‍💻 Developed By

Sai Krishna Vankayala
Computer Science Engineering Student
SmartBridge Internship Project

📜 Acknowledgement

I would like to thank SmartBridge for providing this internship opportunity and guidance throughout the project development.


---

## ✅ NEXT STEPS (IMPORTANT)

After pasting this README:

```bash
git add README.md
git commit -m "Added detailed project README"
git push
