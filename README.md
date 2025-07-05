Library Management System
Overview
The Library Management System is a web-based application built with Flask and SQLite to manage library operations such as book issuance, returns, and user management. It supports two user roles: Admins (who manage books and issue them) and Students (who can search and issue books). The system incorporates robust security features to ensure safe and reliable operation, making it suitable for small to medium-sized libraries.
This project was developed as a secure, user-friendly solution for library management, with a focus on modern web development practices and security best practices.
Features
Admin Features

Login: Admins can log in with secure credentials (default: username: shaheer, password: 231330@shaheer).
Create Admins: Authorized admins can create new admin accounts.
Add Books: Add books with title, author, and ISBN.
View Books: List all books with their status (Available/Issued).
Issue Books: Issue books to students using their username and book ISBN via dropdown menus.
View Issued Books: View details of issued books, including student username, issue date, expiry date, and return status.
View Students: List all registered students.
Search Books: Search books by title, author, or ISBN.

Student Features

Register: Students can create accounts with a username and password.
Login: Secure login for students to access their dashboard.
View Issued Books: See a list of books they have issued, including issue and expiry dates.
Search and Issue Books: Search for books and issue available ones directly from search results.
Return Books: Return issued books, updating their status to Available.

Security Features
The application incorporates the following security measures to protect against common web vulnerabilities:

Secure Error Handling:

Custom error pages for 404 (Page Not Found) and 500 (Internal Server Error) prevent leakage of sensitive information like stack traces.
Generic error messages are used in production to avoid exposing system details.


Secure Input Handling:

All user inputs (username, password, ISBN, book title, author, search queries) are validated using regular expressions and length checks to prevent invalid or malicious input.
Inputs are sanitized and escaped in templates using Jinja2's | e filter to mitigate Cross-Site Scripting (XSS) attacks.


Parameterized Queries:

All database queries use parameterized statements with SQLite to prevent SQL injection attacks.
Example: cursor.execute('SELECT * FROM students WHERE username = ?', (username,)).


Session Management:

Secure session cookies are configured with HttpOnly and SameSite=Strict attributes to prevent JavaScript access and cross-site request issues.
Sessions are set to expire after 30 minutes of inactivity (PERMANENT_SESSION_LIFETIME).
In production, SESSION_COOKIE_SECURE should be enabled for HTTPS-only cookies.


CSRF Protection:

Flask-WTF is used to implement Cross-Site Request Forgery (CSRF) protection.
All POST forms include a CSRF token (<input type="hidden" name="csrf_token" value="{{ csrf_token() }}">) to validate requests.


Password Security:

Passwords are hashed using werkzeug.security.generate_password_hash with a secure hashing algorithm (PBKDF2 by default).
Password validation enforces a minimum length of 8 characters and allows alphanumeric characters plus specific symbols.


Role-Based Access Control:

A custom decorator (@role_required) restricts access to routes based on user roles (admin or student).
Unauthorized access attempts redirect to the homepage with an error message.



Technologies Used

Backend: Flask (Python web framework)
Database: SQLite (lightweight, serverless database)
Frontend: HTML, Tailwind CSS (via CDN for styling)
Security: Flask-WTF (CSRF protection), Werkzeug (password hashing)
Python Version: Compatible with Python 3.13

Project Structure
Library-Management-System/
├── library_management.py      # Main Flask application
├── templates/
│   ├── index.html             # Landing page
│   ├── admin_login.html       # Admin login page
│   ├── admin_dashboard.html   # Admin dashboard
│   ├── create_admin.html      # Create new admin
│   ├── add_book.html          # Add a new book
│   ├── view_books.html        # List all books
│   ├── issue_book.html        # Issue book to a student
│   ├── view_issued_books.html # List issued books
│   ├── view_students.html     # List all students
│   ├── student_register.html  # Student registration
│   ├── student_login.html     # Student login
│   ├── student_dashboard.html # Student dashboard
│   ├── search_books.html      # Search books page
│   ├── error.html             # Custom error page
├── library.db                 # SQLite database (generated on first run)
├── README.md                  # This file

Setup Instructions
Follow these steps to set up and run the project locally:

Clone the Repository:
git clone https://github.com/your-username/Library-Management-System.git
cd Library-Management-System


Create a Virtual Environment (recommended):
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate


Install Dependencies:
pip install flask flask-wtf


Run the Application:
python library_management.py


The app will run in debug mode by default (http://localhost:5000).
The SQLite database (library.db) is created automatically on first run.


Access the Application:

Open http://localhost:5000 in your browser.
Admin Login: Use username: shaheer, password: 231330@shaheer to log in as an admin.
Student Register/Login: Register a new student account or log in with existing credentials.


Production Notes:

Set app.run(debug=False) in library_management.py for production.
Enable HTTPS and set app.config['SESSION_COOKIE_SECURE'] = True.
Use a WSGI server like Gunicorn for deployment:pip install gunicorn
gunicorn -w 4 library_management:app





Usage

Admin Workflow:

Log in as an admin (shaheer/231330@shaheer).
Create additional admins if needed (/create_admin).
Add books with title, author, and ISBN (/add_book).
Issue books to students using dropdowns for ISBN and username (/issue_book).
View all books (/view_books), issued books (/view_issued_books), or students (/view_students).
Search books by title, author, or ISBN (/search_books).


Student Workflow:

Register a new account (/student_register) with a username (3-20 characters, alphanumeric) and password (8+ characters).
Log in (/student_login) to access the student dashboard.
View issued books, search for books, and issue available books (/search_books).
Return issued books from the dashboard (/student_dashboard).


Security Notes:

Ensure inputs meet validation criteria (e.g., usernames are alphanumeric, passwords are 8+ characters).
CSRF tokens are required for all form submissions.
Session timeouts occur after 30 minutes of inactivity.



Testing
To verify the application works as expected:

Admin Login:
Log in with shaheer/231330@shaheer and test all admin features (add books, issue books, view lists).


Student Registration and Login:
Register a student (e.g., username: teststudent, password: Test@123).
Log in with the same credentials and verify access to the student dashboard.


Search and Issue:
As a student, search for a book and issue it if available.
Return the book from the dashboard.


Error Handling:
Test invalid inputs (e.g., short passwords, invalid ISBNs) to ensure validation errors are displayed.
Access a nonexistent page (e.g., /invalid) to verify the 404 error page.



Troubleshooting

"Invalid credentials" on login: Ensure the username and password match exactly what was registered. Check library.db for stored credentials using an SQLite viewer.
"Database error": Verify library.db is in the project root and is writable.
"Method Not Allowed": Ensure forms use the correct method (POST) and action URLs.
CSRF Errors: Ensure Flask-WTF is installed and CSRF tokens are present in forms.
Console Errors: Check the Flask console for detailed error messages and share them for further debugging.

Contributing
Contributions are welcome! To contribute:

Fork the repository.
Create a new branch (git checkout -b feature/your-feature).
Make changes and commit (git commit -m "Add your feature").
Push to your fork (git push origin feature/your-feature).
Open a pull request with a detailed description of your changes.

License
This project is licensed under the MIT License. See the LICENSE file for details.
Acknowledgments

Built with Flask and Tailwind CSS for a lightweight and responsive design.
Security features inspired by OWASP best practices.
Developed with a focus on simplicity and security for educational purposes.
