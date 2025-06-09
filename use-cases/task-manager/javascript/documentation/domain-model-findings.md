# Domain Model Findings: Task Manager

## 1. Core Entity Classes and Concepts:

Based on the codebase, the core entities and concepts of the Task Manager application are:

*   **<mcsymbol name="Task" filename="models.js" path="models.js" startline="18" type="class"></mcsymbol>**: Represents a single task in the system. It encapsulates all the properties and behaviors associated with a task.
    *   **Properties**: `id` (unique identifier, likely UUID), `title`, `description`, `priority`, `status`, `createdAt`, `updatedAt`, `dueDate`, `completedAt`, `tags`.
    *   **Behaviors**: `update` (to modify properties), `markAsDone` (to change status and set `completedAt`), `isOverdue` (to check if the task is past its due date and not completed).

*   **<mcsymbol name="TaskPriority" filename="models.js" path="models.js" startline="4" type="class"></mcsymbol>**: An enumeration defining the possible priority levels for a task.
    *   **Values**: `LOW`, `MEDIUM`, `HIGH`, `URGENT` (mapped to numerical values 1-4).

*   **<mcsymbol name="TaskStatus" filename="models.js" path="models.js" startline="11" type="class"></mcsymbol>**: An enumeration defining the possible completion statuses for a task.
    *   **Values**: `TODO`, `IN_PROGRESS`, `REVIEW`, `DONE`.

*   **<mcsymbol name="TaskManager" filename="app.js" path="app.js" startline="5" type="class"></mcsymbol>**: This class acts as the central orchestrator for task-related business logic. It manages the lifecycle of tasks and interacts with the storage layer.

*   **<mcsymbol name="TaskStorage" filename="storage.js" path="storage.js" startline="6" type="class"></mcsymbol>**: Handles the persistence of tasks, abstracting away the details of how tasks are saved to and loaded from the `tasks.json` file.

*   **`tasks.json`**: The persistent storage for all task data.

## 2. Business Logic Related to Tasks:

The business logic is primarily found within the <mcsymbol name="TaskManager" filename="app.js" path="app.js" startline="5" type="class"></mcsymbol> class, which utilizes <mcsymbol name="Task" filename="models.js" path="models.js" startline="18" type="class"></mcsymbol> objects and interacts with <mcsymbol name="TaskStorage" filename="storage.js" path="storage.js" startline="6" type="class"></mcsymbol>. Key business operations include:

*   **Task Creation**: `createTask` (in `TaskManager`) generates a new <mcsymbol name="Task" filename="models.js" path="models.js" startline="18" type="class"></mcsymbol> instance and adds it to storage.
*   **Task Listing/Filtering**: `listTasks` (in `TaskManager`) retrieves tasks based on status, priority, or overdue status, delegating to `TaskStorage` for data retrieval.
*   **Task Updates**: `updateTaskStatus`, `updateTaskPriority`, `updateTaskDueDate`, `addTagToTask`, `removeTagFromTask` (in `TaskManager`) modify specific properties of a <mcsymbol name="Task" filename="models.js" path="models.js" startline="18" type="class"></mcsymbol> object, which then gets persisted via `TaskStorage`.
*   **Task Deletion**: `deleteTask` (in `TaskManager`) removes a task from storage.
*   **Task Statistics**: `getStatistics` (in `TaskManager`) calculates and aggregates task counts by status, priority, and overdue status.

## 3. Terminology and Concepts:

*   **CLI-based**: The application is primarily interacted with via command-line interface, defined in <mcfile name="cli.js" path="cli.js"></mcfile>.
*   **Modular, Class-based**: The codebase is structured using classes (<mcsymbol name="Task" filename="models.js" path="models.js" startline="18" type="class"></mcsymbol>, <mcsymbol name="TaskManager" filename="app.js" path="app.js" startline="5" type="class"></mcsymbol>, <mcsymbol name="TaskStorage" filename="storage.js" path="storage.js" startline="6" type="class"></mcsymbol>) and organized into separate files for clear separation of concerns.
*   **Persistence Layer**: The `TaskStorage` class and `tasks.json` file form the persistence layer, handling how data is saved and loaded.
*   **UUID**: Used for generating unique identifiers for tasks, ensuring each task has a distinct ID.

## 4. Initial Understanding and Questions:

### Simple Diagram of Entity Relationships:

```mermaid
classDiagram
    Task --o TaskPriority : has
    Task --o TaskStatus : has
    TaskManager --o TaskStorage : uses
    TaskManager --o Task : manages
    TaskStorage --o Task : stores
    cli --o TaskManager : interacts with

    class Task {
        +String id
        +String title
        +String description
        +TaskPriority priority
        +TaskStatus status
        +Date createdAt
        +Date updatedAt
        +Date dueDate
        +Date completedAt
        +String[] tags
        +update()
        +markAsDone()
        +isOverdue()
    }
    class TaskPriority {
        <<enumeration>>
        LOW
        MEDIUM
        HIGH
        URGENT
    }
    class TaskStatus {
        <<enumeration>>
        TODO
        IN_PROGRESS
        REVIEW
        DONE
    }
    class TaskManager {
        +createTask()
        +listTasks()
        +updateTaskStatus()
        +updateTaskPriority()
        +updateTaskDueDate()
        +deleteTask()
        +getTaskDetails()
        +addTagToTask()
        +removeTagFromTask()
        +getStatistics()
    }
    class TaskStorage {
        +addTask()
        +getTask()
        +updateTask()
        +deleteTask()
        +getAllTasks()
        +getTasksByStatus()
        +getTasksByPriority()
        +getOverdueTasks()
    }