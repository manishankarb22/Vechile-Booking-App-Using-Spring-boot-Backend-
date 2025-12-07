# VBooking — Spring Boot + React (Vite) Vehicle Booking Application

✅ Project overview

VBooking is a full-stack vehicle booking application comprised of:
- Backend: Java 17 + Spring Boot (REST API) with MySQL persistence, JWT authentication, email OTP verification, Google OAuth login, and scheduling/async tasks.
- Frontend: React + TypeScript + Vite (single page application) for signup/login, booking flow, payments (mock), and booking management.

This repository contains two main folders:
- `FinalProject/` — Spring Boot backend
- `Vbooking/` — Frontend (React + Vite)

---

## Key features
- User signup with email OTP verification
- Google OAuth sign-in
- JWT-based authentication and protected APIs
- CRUD booking endpoints (create, list, details, cancel)
- Booking statistics (per user)
- Email notifications for OTP, booking confirmation & cancellation
- Simple client-side payment flow (mock) and logging
- CORS set up to allow frontend on localhost

---

## Tech stack
- Backend
  - Java 17, Spring Boot 3.5.7
  - Spring Security (JWT)
  - Spring Data JPA, MySQL
  - Spring Mail + JavaMailSender
  - Jackson for JSON
  - Asynchronous tasks & scheduling for email and cleanup
- Frontend
  - React 19 + TypeScript
  - Vite dev server (port 5173 by default)
  - axios for HTTP client
  - react-router for routing

---

## Quick start

Prerequisites:
- Java 17
- Maven (or use the included Maven wrapper)
- Node.js (v18+) and npm
- MySQL database or Docker

### Backend (Spring Boot)
Windows (PowerShell) commands:

```powershell
cd "FinalProject"
# Run via Maven wrapper
.\mvnw.cmd spring-boot:run
# or build and run
.\mvnw.cmd clean package
java -jar target/FinalProject-0.0.1-SNAPSHOT.war
```

If you're using Docker for the database, create a local MySQL container:

```powershell
# Example (change password and DB name as needed):
docker run --name vbooking-mysql -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=auth_system_db -p 3306:3306 -d mysql:8.0
```

Things to check in `FinalProject/src/main/resources/application.properties`:
- `spring.datasource.url` — DB host/port
- `spring.datasource.username`, `spring.datasource.password`
- `jwt.secret` — set a secure value for JWT signing
- `jwt.expiration` — expiration in milliseconds
- `spring.mail.username`, `spring.mail.password` — setup credentials for sending OTP / booking mails
- `google.client.id`, `google.client.secret` — if enabling Google OAuth

Note: The repo includes some default dev values (like `root`/`1234`) that should be updated in production.

### Frontend (Vite + React)

```powershell
cd "Vbooking"
npm install
npm run dev
# or build for production
npm run build
npm run preview  # preview built app
```

Frontend default dev server: `http://localhost:5173`. The frontend communicates with the backend API at `http://localhost:8080/api` (set in `Vbooking/src/services/api.ts`).

---

## API Endpoints (summary)
All endpoints are prefixed by `/api` (base path), see controllers below.

AuthController (`/api/auth`)
- POST /auth/signup — Register and receive OTP via email
- POST /auth/verify-otp — Verify the OTP sent to email
- POST /auth/login — Login and receive JWT token
- POST /auth/google-login — Login/Signup via Google OAuth (exchanges Google token)
- GET /auth/check-user/{email} — Check if user exists & verified
- DELETE /auth/delete-user/{email} — Delete user by email
- GET /auth/home — User dashboard (requires authentication)
- GET /auth/test — Test endpoint
- GET /auth/test-db — DB connectivity + stats
- POST /auth/bookings/create — Create a booking for the authenticated user
- GET /auth/bookings/my-bookings — Get all bookings for user
- GET /auth/bookings/upcoming — Get upcoming bookings
- GET /auth/bookings/current — Get current bookings
- GET /auth/bookings/past — Get past bookings
- GET /auth/bookings/{bookingId} — Get a booking by ID
- GET /auth/bookings/stats — Get booking stats for the user
- PUT /auth/bookings/{bookingId}/cancel — Cancel a booking

BookingController (`/api/bookings`) — duplicates some booking endpoints (structured to separate concerns)
- POST /bookings/create
- GET /bookings/my-bookings
- GET /bookings/upcoming
- GET /bookings/current
- GET /bookings/past
- GET /bookings/{bookingId}
- PUT /bookings/{bookingId}/cancel
- GET /bookings/stats

Security note: JWT authentication uses the Authorization header `Authorization: Bearer <token>` and default CORS is permissive for dev origins.

---

## Configuration & Environment
Important configuration properties live in `FinalProject/src/main/resources/application.properties`. You should not commit secrets to version control. Replace placeholder values with real ones in your environment.

Recommended env changes:
- Use env vars or a secrets manager in production (e.g., `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, `SPRING_DATASOURCE_PASSWORD`).
- Rotate the `jwt.secret` value and use strong, random secrets (256-bit key).
- Set mail credentials and/or use a transactional mail provider (e.g., SendGrid, Mailgun) for production site notifications.

---

## Database
- Default MySQL DB: `jdbc:mysql://localhost:3306/auth_system_db` (see `application.properties`)
- The application uses `spring.jpa.hibernate.ddl-auto=update`. This will create/update schema automatically in dev, but in production prefer managed migrations (Flyway or Liquibase).

---

## Testing
- Backend: `cd FinalProject` and run:

```powershell
.\mvnw.cmd test
```

- Frontend: Vite app has no test runner in the repository by default; you can add one if needed (e.g., Vitest).

---

## Developer notes (observations)
- Booking endpoints are exposed in both an `AuthController` under `/api/auth/bookings/*` and in a separate `BookingController` under `/api/bookings/*`. The frontend currently uses `/api/auth/bookings/*` paths. It might be cleaner to keep booking endpoints in a single controller.
- OTP confirmation is handled by an in-memory PendingRegistration map for signup verification. This is ephemeral and will be cleared on app restarts; consider implementing a DB-backed pending registrations or modifying the flow to save and verify OTP in DB for production.
- Some methods make use of raw strings for statuses (e.g., `CONFIRMED`, `IN_PROGRESS`), which can be converted to enums for better type safety.

---

## Important files & structure
- `FinalProject/` (Backend):
  - `src/main/java/com/example/FinalProject/controller/` – controllers
  - `src/main/java/com/example/FinalProject/service/` – core business logic
  - `src/main/java/com/example/FinalProject/entity/` – JPA entities: `User`, `Booking`
  - `src/main/java/com/example/FinalProject/repository/` – Spring Data JPA repositories
  - `src/main/resources/application.properties` – configuration
  - `pom.xml` – Maven build

- `Vbooking/` (Frontend):
  - `src/components/` – UI components (Login, Signup, BookingForm, Payment, BookingSuccess)
  - `src/services/api.ts` – Axios wrapper & api functions
  - `src/utils/auth.ts` – token storage and client-side helpers
  - `package.json` – scripts and deps

---

## How to contribute
- Clone the repository
- Create a feature branch and push PRs with descriptive titles
- Update this README if adding features or changing config

---

## License & Contact
- This repository includes placeholder values for sample deployments. Confirm licenses you’d like to use in each sub-project.
- Contact (from project config): manieerr@gmail.com (used for email sending example)

---

If you'd like, I can also:
- Add a sample `.env` / `.properties` file template for secure local setup
- Add a small `docker-compose.yml` to start MySQL + backend for easier local development
- Convert in-memory pending registration flow to DB-based

💡 Next: I'll create this README file in the repository now and make sure key configuration info is included.# Vechile-Booking-App-Using-Spring-boot-Backend-
