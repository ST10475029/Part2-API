# RaceDay API

RaceDay API is a .NET 8 ASP.NET Core Web API for managing race events, categories, participant enrolments, and results.

## Overview

This project provides a secure, role-based API for two user types:

- Organiser: create and manage events, categories, and race results
- Participant: enrol in events and view their own race results

## Tech Stack

- .NET 8
- ASP.NET Core Web API
- Entity Framework Core
- SQL Server LocalDB / SQL Server
- Cookie-based authentication
- Swagger / OpenAPI
- PBKDF2 password hashing

## Project Structure

- `RaceDay.Api/` – main application
- `docs/RaceDay_Database_Script.sql` – database creation and seed script
- `.vscode/` – VS Code launch and task configuration

## Prerequisites

- .NET 8 SDK
- SQL Server LocalDB or another SQL Server instance
- VS Code (recommended)

## Configuration

The API reads the database connection string from `RaceDay.Api/appsettings.json`.

```json
"ConnectionStrings": {
  "RaceDayDb": "Server=(localdb)\\MSSQLLocalDB;Database=RaceDayDb;Trusted_Connection=True;TrustServerCertificate=True;"
}
```

Update this value if you are using a different SQL Server instance.

## Database Setup

You can either:

1. Run the application and let EF Core create the schema automatically, or
2. Use the SQL script at `docs/RaceDay_Database_Script.sql` to create the database and seed sample data.

## Run the API

From the repository root:

```bash
dotnet restore
dotnet build
dotnet run --project RaceDay.Api
```

The API will be available on:

- http://localhost:5080
- Swagger UI: http://localhost:5080/swagger

## Authentication

Authentication is handled with cookies. The API supports:

- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`

### Register

```http
POST /api/auth/register
Content-Type: application/json

{
  "fullName": "Jane Doe",
  "email": "jane@example.com",
  "password": "Password123!",
  "role": "Participant"
}
```

### Login

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "jane@example.com",
  "password": "Password123!"
}
```

## Main API Endpoints

### Users

- `GET /api/users/me`
- `PUT /api/users/me`
- `GET /api/users/{id}`
- `GET /api/users/me/enrolments`

### Events

- `GET /api/events`
- `GET /api/events/{id}`
- `POST /api/events`
- `PUT /api/events/{id}`
- `DELETE /api/events/{id}`

### Categories

- `GET /api/events/{eventId}/categories`
- `POST /api/events/{eventId}/categories`
- `PUT /api/categories/{id}`
- `DELETE /api/categories/{id}`

### Enrolments

- `POST /api/categories/{id}/enrol`
- `GET /api/categories/{id}/enrolments`
- `DELETE /api/enrolments/{id}`

### Results

- `POST /api/enrolments/{id}/result`
- `GET /api/categories/{id}/results`
- `GET /api/enrolments/{id}/result`

## Typical Workflow

1. Register as an organiser or participant
2. Log in using the auth endpoint
3. Create an event (organiser)
4. Add categories to the event (organiser)
5. Enrol in a category as a participant
6. Record a result for an enrolment (organiser)
7. View the leaderboard and event results

## Notes

- The application uses role-based authorization.
- Session data is stored in a cookie and is sent automatically with authenticated requests.
- Passwords are hashed using PBKDF2 with a unique random salt per user.

## License

No explicit license has been added to this repository yet.
