

# Project Collaboration API with Google OAuth

![Django](https://img.shields.io/badge/Django-4.2-brightgreen.svg) ![DRF](https://img.shields.io/badge/DRF-3.15-blue.svg) ![Python](https://img.shields.io/badge/Python-3.12.3-yellow.svg) ![License](https://img.shields.io/badge/license-MIT-green.svg)

![Django](https://github.com/kihuni/google-auth-django/blob/main/Dev.to%20-%20100px%20x%20430px.png) 

A Django REST API for project collaboration, featuring secure Google OAuth authentication and JWT-based API access. This project allows users to sign in with their Google accounts, obtain a JWT, and interact with project-related endpoints. It’s designed to be a scalable foundation for building a collaborative platform where users can list projects, find contributors, or offer support.

This project is part of a detailed tutorial on integrating Google OAuth with Django REST Framework. Check out the full guide here: [How to Add Google OAuth to Django REST API](https://dev.to/kihuni/how-to-add-google-oauth-to-django-rest-api-29h9).

---

## Features

- **Google OAuth Sign-In**: Seamless authentication using Google accounts.
- **JWT Authentication**: Secure API access with JSON Web Tokens (JWT) via `djangorestframework-simplejwt`.
- **Protected Endpoints**: Access project-related endpoints with JWT authentication.
- **Custom Adapter**: Redirects users to a token endpoint after login for API-first workflows.
- **Minimal Frontend**: A simple homepage to initiate Google OAuth login.
- **Scalable Structure**: Ready to extend with additional endpoints for project collaboration.

---

## Project Structure
```
ProjectAPI/
├── core/
│   ├── migrations/
│   ├── templates/
│   │   └── core/
│   │       └── home.html
│   ├── init.py
│   ├── adapters.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── ProjectAPI/
│   ├── init.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── .env
├── manage.py
├── Pipfile
├── Pipfile.lock
└── README.md
```
---

## Prerequisites

- **Python 3.12.3**: Ensure Python is installed. You can download it from [python.org](https://www.python.org/downloads/).
- **Pipenv**: Used for dependency management. Install it with:
  ```
  pip install pipenv
  ```
Google Developer Console Account: Required to set up Google OAuth credentials.

Setup Instructions

1. Clone the Repository

```
git clone https://github.com/your-username/project-collaboration-api.git
cd project-collaboration-api

```
2. Set Up the Virtual Environment
   
Use Pipenv to create a virtual environment and install dependencies:

```
pipenv --python 3.12.3
pipenv install
```
4. Configure Google OAuth Credentials
   
- Go to the [Google Cloud Console](https://console.cloud.google.com/welcome?inv=1&invt=Abs7rg&project=projectcollabapi).

- Create a new project (e.g., "ProjectCollabAPI").

- Enable the "Google+ API" or "People API" under "APIs & Services" > "Library."

**Configure the OAuth consent screen:**

- Choose "External" user type.

- Add profile and email scopes.

- Add your email as a test user.

**Create OAuth 2.0 Client IDs under "Credentials":**

- Select "Web application."

**Add authorized redirect URIs:**
- http://localhost:8000/auth/google/login/callback/

- http://127.0.0.1:8000/auth/google/login/callback/

Copy the Client ID and Client Secret.

5. Set Up Environment Variables
   
Create a .env file in the project root:

```
GOOGLE_CLIENT_ID=your-client-id-here
GOOGLE_CLIENT_SECRET=your-client-secret-here
```

6. Apply Migrations

Run the following commands to set up the database:

```
pipenv run python manage.py makemigrations
pipenv run python manage.py migrate
```
7. Create a Superuser

Create a superuser to access the Django admin interface:

```
pipenv run python manage.py createsuperuser
```
8. Configure the Google SocialApp

Start the server:

```
pipenv run python manage.py runserver
```

- Visit http://localhost:8000/admin/ and log in with your superuser credentials.

- Under Social Accounts > Social applications, add a new application:
Provider: Google

- Name: Google OAuth

- Client ID: Your GOOGLE_CLIENT_ID

- Secret key: Your GOOGLE_CLIENT_SECRET

- Sites: Move the site with SITE_ID=1 to "Chosen sites"

- Save the application.

9. Run the Development Server

```
pipenv run python manage.py runserver
```

- Visit http://localhost:8000/api/ to see the homepage.

**Usage**

**Sign In with Google:**

- On the homepage (/api/), click "Sign In with Google."

- You’ll be redirected to Google’s OAuth consent screen.

- After signing in, you’ll be redirected to /api/token/ to obtain a JWT.

**Obtain a JWT:**

The /api/token/ endpoint returns a JSON response with refresh and access tokens:
```

{
    "refresh": "your-refresh-token",
    "access": "your-access-token"
}
```
**Access Protected Endpoints:**

- Use the access token to access protected endpoints like /api/projects/:
bash

- curl -H "Authorization: Bearer <access_token>" http://localhost:8000/api/projects/

**Example response:**
```

{
    "message": "Welcome to the project collaboration API!",
    "user": "user@example.com",
    "projects": ["Sample Project 1", "Sample Project 2"]
}
```
API Endpoints

```
Endpoint

Method

Description

Authentication Required

/api/

GET

Homepage with login link

No

/api/token/

GET

Get JWT tokens after login

Yes (session-based)

/api/projects/

GET

List sample projects

Yes (JWT)

```
This project is part of a detailed tutorial on integrating Google OAuth with Django REST Framework. Check out the full guide here: [How to Add Google OAuth to Django REST API](https://dev.to/kihuni/how-to-add-google-oauth-to-django-rest-api-29h9)

