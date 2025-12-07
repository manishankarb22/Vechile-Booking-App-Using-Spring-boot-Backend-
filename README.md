🚗 VBooking — Vehicle Booking Application
Spring Boot (Java 17) + React (Vite + TypeScript) Full-Stack System
📌 Overview
VBooking is a full-stack Vehicle Booking Application built using:

Spring Boot (Java 17) for backend REST APIs

React + TypeScript + Vite for the frontend

MySQL for persistence

JWT Authentication, Email OTP Verification, and Google OAuth Login

Clean modular structure with Booking, Users, Authentication, and Notification modules

This is a final-year ready project with real-world scalable architecture.

✨ Key Features
🔐 Authentication & Authorization
Email signup with OTP verification

Secure login using JWT

Google OAuth login support

Route protection on frontend

🚗 Booking Management
Create a booking

List user-specific bookings

Cancel bookings

View upcoming / current / past bookings

Booking statistics dashboard

📧 Email Notifications
OTP emails

Booking confirmation and cancellation emails

Asynchronous email sending

💸 Payment Flow (Mock)
User-friendly client-side payment simulation

Logging & booking confirmation

🛠 Additional Features
CORS configured for dev

Clean layered Spring architecture

Auto schema generation

Environment-based config support

🏗 System Architecture
vbnet
Copy code
            ┌──────────────────────────────────────┐
            │               Frontend                │
            │     React + TypeScript + Vite        │
            │  Login | Signup | Booking | Payments │
            └──────────────────────────────────────┘
                           │   REST API Calls
                           ▼
┌────────────────────────────────────────────────────────────────┐
│                           Backend (Spring Boot)                │
│                                                                │
│   ┌──────────────┐   ┌────────────────────┐   ┌────────────┐  │
│   │ Auth Module  │   │ Booking Module     │   │ Email OTP  │  │
│   │ JWT + OAuth  │   │ CRUD Operations    │   │ Async Mail │  │
│   └──────────────┘   └────────────────────┘   └────────────┘  │
│                     │ JPA / Hibernate │                         │
└─────────────────────┴─────────────────┴──────────────────────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │      MySQL DB       │
                │ Users | Bookings    │
                └─────────────────────┘
🗄 Entity Relationship Diagram (ERD)
bash
Copy code
┌──────────────┐         1 ──── n         ┌──────────────┐
│    USERS     │──────────────────────────│   BOOKINGS    │
└──────────────┘                          └──────────────┘
| id (PK)      |                          | id (PK)       |
| name         |                          | user_id (FK)  |
| email        |                          | vehicleType   |
| password     |                          | startDate     |
| verifiedOTP  |                          | endDate       |
└──────────────┘                          | status        |
                                          └──────────────┘
🔄 Authentication Flow
pgsql
Copy code
User → Signup → Receive OTP → Verify OTP → Account Active
User → Login → Backend Issues JWT → Access Protected APIs
Google OAuth Flow:
mathematica
Copy code
Google Token → Backend → Verify Token → Generate JWT → Login Success
📁 Project Structure
bash
Copy code
VBooking/
│
├── FinalProject/                 # Backend (Spring Boot)
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── entity/
│   ├── security/
│   └── application.properties
│
└── Vbooking/                     # Frontend (React + Vite)
    ├── src/components/
    ├── src/services/api.ts
    ├── src/utils/auth.ts
    └── vite.config.ts
⚙️ Backend Setup (Spring Boot)
1️⃣ Run MySQL
bash
Copy code
docker run --name vbooking-mysql `
  -e MYSQL_ROOT_PASSWORD=1234 `
  -e MYSQL_DATABASE=auth_system_db `
  -p 3306:3306 -d mysql:8.0
2️⃣ Configure application.properties
properties
Copy code
spring.datasource.url=jdbc:mysql://localhost:3306/auth_system_db
spring.datasource.username=root
spring.datasource.password=1234

jwt.secret=CHANGE_THIS_TO_A_256BIT_SECRET
jwt.expiration=86400000

spring.mail.username=your-email@example.com
spring.mail.password=your-app-password
3️⃣ Run Project
bash
Copy code
cd FinalProject
./mvnw spring-boot:run
💻 Frontend Setup (React + Vite)
bash
Copy code
cd Vbooking
npm install
npm run dev
App opens at:

👉 http://localhost:5173

🔌 API Summary
AuthController — /api/auth
Method	Endpoint	Description
POST	/signup	Start signup + email OTP
POST	/verify-otp	Verify OTP
POST	/login	Login with JWT
POST	/google-login	Google OAuth login
GET	/check-user/{email}	Check if user exists
DELETE	/delete-user/{email}	Remove user

BookingController — /api/bookings
Method	Endpoint	Description
POST	/create	Create booking
GET	/my-bookings	View user bookings
GET	/upcoming	Upcoming bookings
GET	/current	Today's bookings
GET	/past	Past bookings
GET	/stats	Booking analytics
PUT	/{id}/cancel	Cancel booking

🧪 Testing Backend
bash
Copy code
cd FinalProject
./mvnw test
🔒 Security Highlights
JWT token signing + filters

Spring Security config

Authenticated user context extraction

CORS enabled for frontend dev

🌍 Environment Variables (Production Recommended)
makefile
Copy code
SPRING_DATASOURCE_URL=
SPRING_DATASOURCE_USERNAME=
SPRING_DATASOURCE_PASSWORD=
JWT_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
MAIL_USERNAME=
MAIL_PASSWORD=
📸 Screenshots (Add your images later)
bash
Copy code
/screenshots
 ├── login.png
 ├── signup.png
 ├── dashboard.png
 └── booking-flow.png
🤝 Contributing
Fork the repo

Create a branch

Commit changes

Open a pull request

📄 License
MIT License — free to use and modify.

📧 Contact
For any queries or improvements:
📨 manieerr@gmail.com

