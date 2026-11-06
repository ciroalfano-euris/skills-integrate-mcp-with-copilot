# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Teacher-only student registration and unregistration

## Teacher access

Set `TEACHER_USERNAME` and `TEACHER_PASSWORD` in the server environment before
starting the application. Do not commit credentials to the repository. Without
these settings, activity viewing remains available, but teacher login and
registration changes are disabled.

Teacher login is required for both signup and unregister API requests. The
browser keeps the short-lived login token in memory only, and the server
invalidates it after eight hours or when the teacher logs out. Use HTTPS when
hosting the application outside a trusted local development environment.

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| POST   | `/auth/login`                                                     | Authenticate a teacher and return a temporary access token          |
| POST   | `/auth/logout`                                                    | Revoke the current teacher access token                             |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | (Teacher only) Sign up a student                                    |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | (Teacher only) Unregister a student                               |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
