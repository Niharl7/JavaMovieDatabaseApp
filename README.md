# JavaMovieDatabaseApp

## Project Description
This is a full-stack application for managing and reviewing movies. It features a Spring Boot backend providing a RESTful API and a JavaFX client for user interaction. The system supports user authentication, movie cataloging, and user-based reviews.

## Table of Contents
1.  [Prerequisites](#prerequisites)
2.  [Project Setup](#project-setup)
3.  [Running the Application](#running-the-application)
4.  [Running Tests](#running-tests)
5.  [Features](#features)
6.  [Technologies Used](#technologies-used)
7.  [Application Architecture](#application-architecture)
8.  [API Specification](#api-specification)
9.  [Security Model](#security-model)
10. [Test Strategy](#test-strategy)
11. [Design Decisions & Patterns](#design-decisions--patterns)
12. [Design Rationale](#design-rationale)
<!-- Optional: Add sections for License and Contact -->
<!-- 13. [License](#license) -->
<!-- 14. [Contact](#contact) -->

## Prerequisites
*   Java JDK 21
*   Apache Maven 3.6.x or newer
*   MySQL Server 8.x

## Project Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Niharl7/JavaMovieDatabaseApp.git
    cd JavaMovieDatabaseApp
    ```
2.  **Database Setup:**
    *   Ensure your MySQL server is running.
    *   The application is configured to connect to a MySQL database named `movie_db_javafx`.
    *   The database will be created automatically if it does not exist when the backend server starts, due to the `createDatabaseIfNotExist=true` setting in the connection URL.
    *   The application will also attempt to create tables on startup if they don't exist, based on `schema.sql` (if `spring.sql.init.mode=always`) and `spring.jpa.hibernate.ddl-auto` settings.

## Running the Application

### Backend (Server)
The backend is a Spring Boot application.
*   Open in CMD (Command Prompt).

1.  Navigate to the server directory:
    ```bash
    cd server
    ```
2.  Run the application using Maven, providing your MySQL credentials:
    ```bash
    mvn spring-boot:run -Dspring-boot.run.arguments="--DB_USERNAME=your_mysql_username --DB_PASSWORD=your_mysql_password"
    ```
    Replace `your_mysql_username` and `your_mysql_password` with your actual MySQL credentials.
    The server will start, typically on port `8080`.

### Frontend (Client)
The frontend is a JavaFX application.
*   Open in CMD (Command Prompt).

1.  Navigate to the client directory:
    ```bash
    cd client
    ```
2.  Run the application using Maven:
    ```bash
    mvn javafx:run
    ```
3.  **Login Details:**
    Default admin username: `admin`
    Default admin password: `admin123`

## Running Tests
*   Open in CMD (Command Prompt).

1.  Navigate to the server directory:
    ```bash
    cd server
    ```
2.  Run the backend tests using Maven. If your tests are configured to run against a live MySQL database, provide your MySQL credentials:
    ```bash
    mvn clean test -DDB_USERNAME=your_mysql_username -DDB_PASSWORD=your_mysql_password
    ```
    Replace `your_mysql_username` and `your_mysql_password` with your actual MySQL credentials.

## Features
*   User registration and login system.
*   Browse a comprehensive catalog of movies.
*   Add, edit, and delete movies (Admin functionality).
*   Submit, view, and manage movie reviews.
*   Role-based access control (User, Admin).
*   RESTful API for backend services.
*   Interactive JavaFX client for user interface.

## Technologies Used
*   **Backend:** Java, Spring Boot, Spring MVC, Spring Security, Spring Data JPA, Hibernate
*   **Frontend:** JavaFX
*   **Database:** MySQL
*   **Build Tool:** Apache Maven
*   **Testing:** JUnit, Mockito, Spring Boot Test

## Application Architecture

The application follows a classic client-server architecture:

*   **Frontend (Client):**
    *   Built with JavaFX.
    *   Handles user interface and interaction.
    *   Communicates with the backend via RESTful API calls.
    *   Implements the Model-View-Controller (MVC) pattern for UI organization.
*   **Backend (Server):**
    *   Built with Spring Boot.
    *   Provides a RESTful API for data management and business logic.
    *   Uses Spring MVC for handling web requests.
    *   Employs a layered architecture:
        *   **Controller Layer (`com.example.controller`):** Handles incoming HTTP requests, delegates to services, and returns responses.
        *   **Service Layer (`com.example.service`):** Contains business logic, orchestrates calls to repositories.
        *   **Repository Layer (`com.example.repository`):** Manages data persistence using Spring Data JPA and interacts with the database.
        *   **Model Layer (`com.example.model`):** Defines JPA entities (e.g., `User`, `Movie`, `Review`).
*   **Database:**
    *   MySQL is used for persistent data storage.

## API Specification

The backend exposes the following primary REST endpoints. All paths are prefixed with `/api`.

*   **Authentication (`/api/auth`):**
    *   `POST /api/auth/register`: User registration.
    *   `POST /api/auth/login`: User login and session creation.
    *   `POST /api/auth/logout`: User logout.
*   **Movies (`/api/movies`):**
    *   `GET /api/movies`: Retrieve all movies.
    *   `GET /api/movies/{id}`: Retrieve a specific movie by ID.
    *   `POST /api/movies`: Create a new movie (Admin only).
    *   `PUT /api/movies/{id}`: Update an existing movie (Admin only).
    *   `DELETE /api/movies/{id}`: Delete a movie (Admin only).
*   **Reviews (`/api/reviews` and `/api/movies/{movieId}/reviews`):**
    *   `GET /api/movies/{movieId}/reviews`: Get all reviews for a specific movie.
    *   `POST /api/movies/{movieId}/reviews`: Create a new review for a movie.
    *   `PUT /api/reviews/{reviewId}`: Update a review (Owner or Admin).
    *   `DELETE /api/reviews/{reviewId}`: Delete a review (Owner or Admin).
*   **Admin (`/api/admin`):**
    *   `GET /api/admin/users`: Get all users (Admin only).
    *   `DELETE /api/admin/users/{userId}`: Delete a user (Admin only).
    *   `PUT /api/admin/users/{userId}/roles`: Update user roles (Admin only).

## Security Model

*   **Framework:** Spring Security.
*   **Authentication:** Session-based authentication. Upon successful login, a session is established, and subsequent requests are authenticated using the session cookie.
*   **Authorization:** Role-Based Access Control (RBAC).
    *   **Roles:** `ROLE_USER`, `ROLE_ADMIN`.
    *   Endpoints are secured using `@PreAuthorize` annotations and security configurations in `SecurityConfig.java`.
*   **Password Management:** Passwords are hashed using `PasswordEncoder` (e.g., BCrypt) before being stored in the database.
*   **CSRF Protection:** Enabled by default in Spring Security for stateful applications.
*   **Session Management:** Configured with timeouts and HttpOnly cookies.

## Test Strategy

*   **Backend:**
    *   **Unit Tests:** Focus on individual components (services, utility classes). Mockito is used for mocking dependencies.
    *   **Integration Tests:** Test interactions between components, including controller-service-repository layers and API endpoints. `@SpringBootTest` is used to load the application context, and `MockMvc` is used for testing REST controllers.
*   **Frontend:**
    *   *(Manual testing was primarily employed. Consider adding UI test frameworks like TestFX for future enhancements.)*

## Design Decisions & Patterns

Several design patterns and principles have been applied:

1.  **Model-View-Controller (MVC):**
    *   **Backend (Spring MVC):** Controllers handle HTTP requests, Models represent data (JPA entities), and Views are typically JSON responses.
    *   **Frontend (JavaFX):** FXML files define the View, Controller classes handle UI logic, and Model classes represent data structures.
2.  **Repository Pattern (Spring Data JPA):**
    *   Interfaces extending `JpaRepository` abstract data access logic.
3.  **Service Layer Pattern:**
    *   Services encapsulate business logic, acting as an intermediary between controllers and repositories.
4.  **Dependency Injection (DI):**
    *   Leveraged extensively by Spring Boot for loose coupling and testability.
5.  **Singleton Pattern:**
    *   Spring beans are singletons by default.
6.  **Data Transfer Object (DTO):**
    *   DTOs are used to transfer data between layers, decoupling API contracts from internal domain models.

## Design Rationale

The architecture aims to balance:

*   **Cohesion:** Functionalities are grouped into logical modules.
*   **Modularity:** Separation of frontend and backend allows for independent development and deployment. The backend's layered architecture ensures clear separation of concerns.
*   **Extensibility:** Using interfaces and DI makes it easier to add new components or modify existing ones. The REST API allows for different types of clients. DTOs help in evolving the API.

<!--
## License
Consider adding an MIT License or another open-source license.
Example:
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
-->

<!--
## Contact
Your Name – [Your Portfolio Website/LinkedIn Profile URL] – your.email@example.com
-->
