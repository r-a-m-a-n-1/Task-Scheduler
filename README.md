<div align="center">

# ⚙️ **TaskMaster**
### 🟦 A Distributed Task Scheduler Written in Go

<img src="https://img.shields.io/badge/Go-1.21-blue?style=for-the-badge">
<img src="https://img.shields.io/badge/Distributed%20Systems-Enabled-green?style=for-the-badge">
<img src="https://img.shields.io/badge/Docker-Ready-orange?style=for-the-badge">

A robust, educational, and scalable **task scheduling system** designed in Go.  
Built to demonstrate distributed system design, worker coordination, and scalable task execution.

</div>

---

## 📌 **Overview**

**TaskMaster** is a distributed task scheduler built using **Go**, **gRPC**, and **PostgreSQL**.  
It efficiently handles high task loads by distributing execution across multiple workers while ensuring coordination, scheduling, and system reliability.

---

## 🧩 **System Components**

### 🔹 **Scheduler**
- Receives incoming tasks via HTTP.
- Stores task & scheduling metadata in PostgreSQL.

### 🔹 **Coordinator**
- Periodically scans DB for due tasks.
- Selects tasks to execute.
- Handles worker registration & removal.
- Distributes tasks to workers for execution.

### 🔹 **Worker**
- Executes tasks assigned by the coordinator.
- Maintains in-memory execution queue.
- Sends heartbeats to coordinator.
- Returns execution status.

### 🔹 **Client**
- Sends tasks to the Scheduler via HTTP.
- Queries task execution status.

### 🔹 **Database (PostgreSQL)**
- Stores task metadata:
  - Task ID  
  - Command / Data  
  - Schedule time  
  - Completion status  

All communication between services is done via **gRPC**, enabling scalability and reliability.

---

## 🔄 **Life of a Task**

### 🕒 **1. Scheduling**
1. A client sends an HTTP request → Scheduler  
2. Scheduler persists task into DB

### ⚙️ **2. Execution**
1. Coordinator scans DB for due tasks  
2. Coordinator assigns tasks to available workers  
3. Worker executes tasks using an internal queue  

### 🔁 **3. Retries & Failure**
- Coordinator retries **only** if it failed to assign a task to a worker  
- Tasks that fail during worker execution are **not retried**  

---

## ⚠️ **Limitations**
- This is an educational project, not production-ready  
- Task payloads are strings only  
- Coordinator push-based execution may overload workers  

---

## 📁 **Directory Structure**

TaskMaster/
│── cmd/
│ ├── scheduler/
│ ├── coordinator/
│ └── worker/
│
│── pkg/
│ ├── scheduler/
│ ├── coordinator/
│ └── worker/
│
│── data/
│ └── *.sql # DB initialization scripts
│
│── tests/
│── docker-compose.yml
│── *-dockerfile

yaml
Copy code

---

## 🐳 **Running a Cluster (Docker Compose)**

Start the entire cluster (1 coordinator, 1 scheduler, 3 workers):

```sh
docker-compose up --build --scale worker=3
Requirements:

Docker installed

Docker Compose installed

🌐 Interacting with the Cluster
📌 1. Scheduling a Task
Send a POST request:

sh
Copy code
curl -X POST localhost:8081/schedule \
-d '{"command":"<your-command>", "scheduled_at":"2023-12-25T22:34:00+05:30"}'
Response:

json
Copy code
{
  "task_id": "<generated-task-id>"
}
📌 2. Query Task Status
sh
Copy code
curl localhost:8081/status?task_id=<task-id>
📜 About the Project
TaskMaster is a simple distributed task scheduler written in Go.
Its purpose is to teach:

Distributed systems

gRPC communication

Worker coordination

Task queues & schedulers

⭐ Resources
Readme

Activity

Releases

Docker setup

🧑‍💻 Technologies Used
Language	%
Go	98.3%
Shell	1.7%

<div align="center">
⭐ If you found this project useful, please give it a star! ⭐

</div> ```
