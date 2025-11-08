### **API Design: [[APP NAME]]

**1. Overview**

  * **Base URL**: `/api/v1`
  * **Authentication**: All endpoints, except for `/auth/signup` and `/auth/login`, will require a JSON Web Token (JWT) sent in the `Authorization` header as a Bearer token.
  * **Data Format**: All request and response bodies will be in JSON format.

-----

**2. Data Models**

  * **User Model**
      * `id`: string (UUID)
      * `email`: string
      * `createdAt`: timestamp
  * **Task Model**
      * `id`: string (UUID)
      * `title`: string
      * `description`: string (optional)
      * `isCompleted`: boolean
      * `dueDate`: timestamp (optional)
      * `ownerId`: string (foreign key to User)
      * `createdAt`: timestamp

-----

**3. Error Responses**

  * Errors will be returned with a standard structure:
    ```json
    {
      "statusCode": 404,
      "message": "Task with ID 123 not found",
      "error": "Not Found"
    }
    ```

-----

**4. Endpoints**

#### **Authentication (`/auth`)**

  * **`POST /auth/signup`**

      * **Description**: Creates a new user account.
      * **Request Body**: `{ "email": "user@example.com", "password": "password123" }`
      * **Success Response (201)**: `{ "id": "uuid", "email": "user@example.com" }`
      * **Error Response (400)**: If email is invalid or password is too weak.
      * **Error Response (409)**: If email already exists.

  * **`POST /auth/login`**

      * **Description**: Logs in a user and returns a JWT.
      * **Request Body**: `{ "email": "user@example.com", "password": "password123" }`
      * **Success Response (200)**: `{ "accessToken": "jwt.token.here" }`
      * **Error Response (401)**: If credentials are invalid.

#### **Tasks (`/tasks`)**

  * **`POST /tasks`**

      * **Description**: Creates a new task for the authenticated user.
      * **Request Body**: `{ "title": "Finish API Design", "description": "Complete all endpoints" }`
      * **Success Response (201)**: Returns the full Task object.
      * **Error Response (400)**: If title is missing.

  * **`GET /tasks`**

      * **Description**: Retrieves all tasks for the authenticated user.
      * **Success Response (200)**: Returns an array of Task objects `[ { ...task1 }, { ...task2 } ]`.

  * **`GET /tasks/:id`**

      * **Description**: Retrieves a single task by its ID.
      * **Success Response (200)**: Returns the full Task object.
      * **Error Response (404)**: If the task is not found or does not belong to the user.

  * **`PATCH /tasks/:id`**

      * **Description**: Updates a task (e.g., changes title, marks as complete).
      * **Request Body**: `{ "title": "Updated Title", "isCompleted": true }` (only include fields to be updated).
      * **Success Response (200)**: Returns the updated Task object.
      * **Error Response (404)**: If the task is not found.

  * **`DELETE /tasks/:id`**

      * **Description**: Deletes a task by its ID.
      * **Success Response (204)**: No content.
      * **Error Response (404)**: If the task is not found.