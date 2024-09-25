
## Resource Management System

### Overview:

This project draws inspiration from my research on High-Performance Computing (HPC) resource managers like Slurm, known for their efficient job scheduling and management capabilities.

The Resource Manager is a job management system built with Java Spring Boot (version 11), utilizing Maven for build automation and PostgreSQL for data persistence. 

### Features

-  Layered architecture, separating the persistence, web, and service layers for better modularity. 
- Users interact with the application through REST APIs, making it easy to manage and monitor jobs. 
- Testing:
	- For APIs, Postman or Swagger
	- Repository-level JUnit testing. 

----------

### Architecture:

#### **1. Model Package:**

-   **Job Entity:** Represents the core job structure with all the necessary properties.
-   **Status Enumeration:** Represents different job statuses (e.g., RUNNING, COMPLETED, FAILED).

#### **2. Repository Package:**

-   **JobRepository:** JPA repository responsible for interacting with the `Job` entity and handling CRUD operations.

#### **3. Service Package:**

-   **JobService:** A Spring Bean Service containing asynchronous methods for parallel job execution without blocking the application.
    -   **Key Methods:**
        -   **runScheduledJob(Long id, Long delay):** Schedules a job with a dynamic delay using a `ScheduledTask` (extending `TimerTask`) and a `Timer` to manage execution.
        -   **runScheduledJobPeriodically(Long id, Long delay, Long repeatPeriod):** Schedules recurring jobs with a dynamic delay and repeat period.
        -   **runJob(Long id):** Executes a job immediately by calling an external API. In case of failure, the job status is updated to `FAILED`. Error handling is implemented using try/catch blocks or if statements.

#### **4. Controller Package:**

-   **JobController:** Provides REST endpoints for job management, leveraging dependency injection for `JobService`.
    -   **Available Routes:**
        -   **POST /jobs/addJob:** Adds a new job via JSON body.
        -   **POST /jobs/startJob/{id}:** Starts a job immediately.
        -   **POST /jobs/scheduleJob/{id}:** Schedules a job with a specified delay.
        -   **GET /jobs/checkStatus/{id}:** Retrieves the status of a job.
        -   **GET /jobs/getAllJobs:** Retrieves a list of all jobs.
        -   **POST /jobs/scheduleJobPeriodically/{id}:** Schedules a job to run periodically.

#### **5. Configuration Package:**

-   Contains configurations for asynchronous job execution in `JobService`.

#### **6. application.properties File:**

-   Stores database credentials and other configuration settings.

#### **7. Testing:**

-   **JobRepositoryTests:** Unit tests for JPA repository functionality, utilizing the H2 in-memory database for testing purposes.

#### **8. Resource Package:**

-   Contains resources like `StatusResource` to create structured responses (e.g., JSON) for the client.

#### **9. Factory Package:**

-   Implements a **Factory Design Pattern** to map job entities to status resources, simplifying the process of transforming entities for client consumption.

---------
