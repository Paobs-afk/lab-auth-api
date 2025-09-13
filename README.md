Project Overview

This project, named lab-auth-api, is an Express.js application that implements a secure authentication system. It uses JSON Web Tokens (JWTs) for session management, bcrypt for secure password hashing, 
and a MySQL database to store user information and revoked tokens. The application includes endpoints for user signup, login, logout, and a protected profile route, all tested using Postman. 
The code is organized into a clean, modular structure with separate directories for controllers, routes, middleware, and configuration.


Setup

1. Pre-Flight Checklist
>Install Node.js and npm.
>Install and run XAMPP, ensuring Apache and MySQL are active.
>Create a new, empty database in MySQL for the project.

2. Project Structure
Create the following directory structure and files:

lab-auth-api/
├─ controllers/
│  └─ authController.js
├─ routes/
│  └─ authRoutes.js
├─ middleware/
│  └─ auth.js
├─ config/
│  └─ db.js
├─ .env
├─ .gitignore
├─ package.json
└─ server.js
controllers/authController.js: Handles the logic for authentication and user-related operations.

routes/authRoutes.js: Defines the API endpoints for authentication.

middleware/auth.js: Contains middleware to protect routes by verifying JWTs.

config/db.js: Manages the database connection.

.env: Stores environment variables like database credentials.

.gitignore: Prevents sensitive files from being committed to version control.

server.js: The main entry point of the application, sets up the server and routes.

3. Dependencies
In the project root, initialize a new Node.js project and install the necessary packages.

Bash

npm init -y
npm install express mysql2 jsonwebtoken bcryptjs uuid dotenv
npm install -D nodemon
Add the following scripts to your package.json file to enable easy running:

JSON

"scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
}
.gitignore
Create a .gitignore file to protect sensitive information and local files.

node_modules/
.env
.env

Create a .env file and add your database credentials.

DB_HOST=localhost
DB_USER=root
DB_PASS=
DB_NAME=<your_db_name>
JWT_SECRET=<a_strong_secret_key>
Remember to replace the placeholders with your actual information. 🔑

Database Schema
You will need to create two tables in your MySQL database to support this application. The users table will store user information, including the hashed password, and the revoked_tokens table will be used for logging out and revoking JWTs.

users table

SQL

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(50) NOT NULL
);
revoked_tokens table

SQL

CREATE TABLE revoked_tokens (
    id INT AUTO_INCREMENT PRIMARY KEY,
    jti VARCHAR(255) UNIQUE NOT NULL,
    expires_at DATETIME NOT NULL
);

How to Run

>Start MySQL: Ensure the MySQL server is running via your XAMPP Control Panel.
>Navigate to Project Directory: Open a terminal and go to the lab-auth-api folder.
>Run the Server: Execute the development script.

Bash

npm run dev
The server will start and listen on port 3000 (or the port specified in your server.js). You'll see a confirmation message in the terminal.


Endpoints

Below is a list of the main API endpoints for the authentication system, along with their purpose and expected payload.

1. Signup
Method: POST

URL: /auth/signup

Description: Creates a new user account by hashing the provided password and saving the user's details to the database.

Request Body:

JSON

{
  "email": "user1@example.com",
  "password": "Pass@1234",
  "full_name": "User One",
  "role": "student"
}
2. Login
Method: POST

URL: /auth/login

Description: Authenticates a user. If the email and password are correct, it generates and returns a JWT. This token should be saved for subsequent requests.

Request Body:

JSON

{
  "email": "user1@example.com",
  "password": "Pass@1234"
}
Response Body:

JSON

{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
Postman Tip: In Postman's "Tests" tab for the login request, you can add a script to automatically save the received token to an environment variable named token.

3. Protected Profile
Method: GET

URL: /profile

Description: A protected endpoint that requires a valid JWT. The middleware verifies the token and, if valid, returns the user's profile information.

Headers:

Authorization: Bearer <your_saved_token>
4. Logout
Method: POST

URL: /auth/logout

Description: Revokes the current user's JWT by adding its unique identifier (jti) to the revoked_tokens table.

Headers:

