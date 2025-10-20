
# Task Manager Application

This project is a **Java Spring Boot** backend with a **React 19 + TypeScript frontend** using Ant Design. The application allows you to create, search, delete, and execute shell command tasks, with data stored in MongoDB.

## **VIDEO DEMO**
[Watch the demo video](https://aseblr-my.sharepoint.com/:v:/g/personal/bl_en_u4aie22020_bl_students_amrita_edu/EeuQNoDmGxRFphVMVjeTfLcBVsuKpQATyG8_OQA-g4CszA?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=l8Yk4J)

---

## **File Structure**

```
.
├── .gitignore
├── pom.xml
├── README.md
└── src
    └── main
        ├── java
        │   └── com
        │       └── kaiburr
        │           └── taskmanager
        │               ├── TaskManagerApplication.java
        │               ├── controller
        │               │   └── TaskController.java
        │               ├── model
        │               │   ├── Task.java
        │               │   └── TaskExecution.java
        │               ├── repository
        │               │   └── TaskRepository.java
        │               └── service
        │                   └── TaskService.java
        └── resources
            └── application.properties
frontend/
└── (React 19 + TypeScript + Ant Design project powered by Vite)
```

---

## **Prerequisites**

- Java 17  
- Maven  
- Node.js (>=18) + npm  
- Docker  

---

## **Backend Setup (Spring Boot)**

### **Step 1: Start MongoDB using Docker**
If you don’t have a local MongoDB instance, run:

```bash
docker run -d -p 27017:27017 --name mongodb mongo
```
- MongoDB will be available at: `mongodb://localhost:27017`

### **Step 2: Configure Spring Boot**
In `src/main/resources/application.properties`:

```properties
spring.data.mongodb.uri=mongodb://localhost:27017/your_database_name
server.port=8081
```
Replace `your_database_name` with your preferred database name.

### **Step 3: Run the backend**
From the project root:

```bash
mvn spring-boot:run
```

The backend API will start on:  
```
http://localhost:8081
```

---

## **Frontend Setup (React + Ant Design)**

1. Navigate to the frontend folder:

```bash
cd frontend
```

2. Install dependencies:

```bash
npm install
```

3. Start the dev server:

```bash
npm run dev
```

- Open [http://localhost:3000](http://localhost:3000) to view the UI.  
- The dev server proxies `/tasks` requests to `http://localhost:8081`.

---

## **API Endpoints**

### **Create or Update a Task**
- **Endpoint:** `PUT /tasks`  
- **Request Body:**
```json
{
  "id": "101",
  "name": "My Test Task",
  "owner": "Balaji",
  "command": "echo Hello World"
}
```

**Example using curl:**
```bash
curl -X PUT http://localhost:8081/tasks -H "Content-Type: application/json" -d '{"id": "101", "name": "My Test Task", "owner": "Balaji", "command": "echo Hello World"}'
```

---

### **Get Tasks**
- **Endpoint:** `GET /tasks`  
- Returns all tasks, or filter by ID using `?id={taskId}`

**Example:**
```bash
curl http://localhost:8081/tasks
curl http://localhost:8081/tasks?id=101
```

---

### **Search Tasks by Name**
- **Endpoint:** `GET /tasks/name/{name}`  
- Finds tasks containing the given string.

**Example:**
```bash
curl http://localhost:8081/tasks/name/Test
```

---

### **Execute a Task**
- **Endpoint:** `PUT /tasks/execute/{id}`  
- Executes the shell command and records the output.

**Example:**
```bash
curl -X PUT http://localhost:8081/tasks/execute/101
```
- Check the execution result:
```bash
curl http://localhost:8081/tasks?id=101
```

---

### **Delete a Task**
- **Endpoint:** `DELETE /tasks/{id}`

**Example:**
```bash
curl -X DELETE http://localhost:8081/tasks/101
```

---

## **Frontend Features**

- **Task Table:** View all tasks, search by name, sort columns.  
- **Create Task:** Drawer-based form with validation.  
- **Execute Task:** Run shell commands with a single click.  
- **Execution Timeline:** View all command executions per task.  
- **Delete Task:** Delete tasks directly from the UI.  
- **Usability & Accessibility:** Fully navigable forms and keyboard accessible components.

---

## **CI/CD Pipeline (GitHub Actions)**

- Automatically builds backend and frontend on `push` or `pull_request` to `main`.  
- **Steps:**
  1. Checkout code
  2. Setup JDK 17 & Node.js
  3. Build frontend (`npm install` + `npm build`)
  4. Build backend with Maven
  5. Build Docker image containing backend + frontend
  6. Push Docker image to Docker Hub

- **Secrets Required:**
  - `DOCKER_HUB_USERNAME`
  - `DOCKER_HUB_ACCESS_TOKEN`

---




## **Notes**

- Always use `mongodb://` for the connection string, not `http://`  
- The backend runs on port `8081`, frontend runs on `3000`  
- Dev server automatically proxies `/tasks` requests to the backend
