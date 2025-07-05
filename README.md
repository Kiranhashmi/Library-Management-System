# Library Management System

## Overview
The Library Management System is a web-based application built with Flask and SQLite to manage library operations such as book issuance, returns, and user management. It supports two user roles: Admins (who manage books and issue them) and Students (who can search and issue books). The system incorporates robust security features to ensure safe and reliable operation, making it suitable for small to medium-sized libraries.
This project was developed as a secure, user-friendly solution for library management, with a focus on modern web development practices and security best practices.

## Features

### Admin Features
- Login: Admins can log in with secure credentials (default: username: shaheer, password: 231330@shaheer).
- Create Admins: Authorized admins can create new admin accounts.
- Add Books: Add books with title, author, and ISBN.
- View Books: List all books with their status (Available/Issued).
- Issue Books: Issue books to students using their username and book ISBN via dropdown menus.
- View Issued Books: View details of issued books, including student username, issue date, expiry date, and return status.
- View Students: List all registered students.
- Search Books: Search books by title, author, or ISBN.

### Student Features
- Register: Students can create accounts with a username and password.
- Login: Secure login for students to access their dashboard.
- View Issued Books: See a list of books they have issued, including issue and expiry dates.
- Search and Issue Books: Search for books and issue available ones directly from search results.
- Return Books: Return issued books, updating their status to Available.

## Security Features
The application incorporates the following security measures to protect against common web vulnerabilities:

### Secure Error Handling:
- Custom error pages for 404 (Page Not Found) and 500 (Internal Server Error) prevent leakage of sensitive information like stack traces.
- Generic error messages are used in production to avoid exposing system details.

### Secure Input Handling:
- All user inputs (username, password, ISBN, book title, author, search queries) are validated using regular expressions and length checks to prevent invalid or malicious input.
- Inputs are sanitized and escaped in templates using Jinja2's | e filter to mitigate Cross-Site Scripting (XSS) attacks.

### Parameterized Queries:
- All database queries use parameterized statements with SQLite to prevent SQL injection attacks.
- Example: cursor.execute('SELECT * FROM students WHERE username = ?', (username,)).

### Session Management:
- Secure session cookies are configured with HttpOnly and SameSite=Strict attributes to prevent JavaScript access and cross-site request issues.
- Sessions are set to expire after 30 minutes of inactivity (PERMANENT_SESSION_LIFETIME).
- In production, SESSION_COOKIE_SECURE should be enabled for HTTPS-only cookies.

### CSRF Protection:
- Flask-WTF is used to implement Cross-Site Request Forgery (CSRF) protection.
- All POST forms include a CSRF token (<input type="hidden" name="csrf_token" value="{{ csrf_token() }}">) to validate requests.

### Password Security:
- Passwords are hashed using werkzeug.security.generate_password_hash with a secure hashing algorithm (PBKDF2 by default).
- Password validation enforces a minimum length of 8 characters and allows alphanumeric characters plus specific symbols.

### Role-Based Access Control:
- A custom decorator (@role_required) restricts access to routes based on user roles (admin or student).
- Unauthorized access attempts redirect to the homepage with an error message.

## Technologies Used
- Backend: Flask (Python web framework)
- Database: SQLite (lightweight, serverless database)
- Frontend: HTML, Tailwind CSS (via CDN for styling)
- Security: Flask-WTF (CSRF protection), Werkzeug (password hashing)
- Python Version: Compatible with Python 3.13

## Installation Guide
Follow these steps to set up and run the Library Management System locally:

1. **Clone the Repository**:
```bash
git clone https://github.com/your-username/Library-Management-System.git
cd Library-Management-System
```
2. **Create Virtual Environment**:
```
python -m venv venv
```
# Linux/macOS:
```
source venv/bin/activate
```
# Windows:
```
venv\Scripts\activate
```
3. **Install Dependencies:**
```
pip install -r requirements.txt
```
4. **Run the Application:**
```
python library_management.py
```
5. **Access the Application:**

- Open http://localhost:5000 in your browser
- Admin Login: username: shaheer, password: 231330@shaheer
- Student: Register at /student_register, then login at /student_login
