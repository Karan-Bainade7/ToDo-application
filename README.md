# ToDo-applicationProject Structure:
css
Copy code
ToDoApplication
│
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.todoapp
│   │   │       ├── controller
│   │   │       ├── model
│   │   │       ├── repository
│   │   │       └── service
│   │   ├── resources
│   │       └── application.properties
│   └── test
└── pom.xml
Step-by-Step Explanation:
1. Create the Spring Boot Project
Use Spring Initializr to set up a project with the following dependencies:

Spring Web
Spring Data JPA
MySQL Driver
2. Add Dependencies in pom.xml
xml
Copy code
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
</dependencies>
3. Configure application.properties
properties
Copy code
spring.datasource.url=jdbc:mysql://localhost:3306/todo_db
spring.datasource.username=root
spring.datasource.password=root_password
spring.jpa.hibernate.ddl-auto=update
4. Create the Model Class
src/main/java/com/todoapp/model/Task.java

java
Copy code
@Entity
public class Task {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String description;
    private boolean completed;

    // Getters and Setters
}
5. Create the Repository Interface
src/main/java/com/todoapp/repository/TaskRepository.java

java
Copy code
public interface TaskRepository extends JpaRepository<Task, Long> {
}
6. Create the Service Layer
src/main/java/com/todoapp/service/TaskService.java

java
Copy code
@Service
public class TaskService {
    @Autowired
    private TaskRepository taskRepository;

    public List<Task> getAllTasks() {
        return taskRepository.findAll();
    }

    public Task createTask(Task task) {
        return taskRepository.save(task);
    }

    public void deleteTask(Long id) {
        taskRepository.deleteById(id);
    }
}
7. Create the Controller
src/main/java/com/todoapp/controller/TaskController.java

java
Copy code
@RestController
@RequestMapping("/api/tasks")
public class TaskController {
    @Autowired
    private TaskService taskService;

    @GetMapping
    public List<Task> getAllTasks() {
        return taskService.getAllTasks();
    }

    @PostMapping
    public Task createTask(@RequestBody Task task) {
        return taskService.createTask(task);
    }

    @DeleteMapping("/{id}")
    public void deleteTask(@PathVariable Long id) {
        taskService.deleteTask(id);
    }
}
8. MySQL Table Structure
Create a todo_db database and a table for tasks:

sql
Copy code
CREATE DATABASE todo_db;
USE todo_db;

CREATE TABLE task (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    description VARCHAR(255),
    completed BOOLEAN
);
How it Works:
CRUD Operations: The TaskController exposes endpoints for task creation, retrieval, and deletion:

GET /api/tasks to fetch all tasks.
POST /api/tasks to add a new task.
DELETE /api/tasks/{id} to remove a task.
Data Persistence: The TaskService uses TaskRepository (based on Spring Data JPA) to interact with the MySQL database.

Application Properties: You configure the database connection in application.properties, ensuring seamless integration with MySQL.

Example Test API Calls:
Get All Tasks:
bash
Copy code
GET http://localhost:8080/api/tasks
Create a Task:
bash
Copy code
POST http://localhost:8080/api/tasks
Body:
{
  "description": "Complete Spring Boot Project",
  "completed": false
}










