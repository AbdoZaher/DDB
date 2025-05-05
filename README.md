# Distributed Database Replication System (Master-Slave Model)

This project simulates a distributed database system using a **Master-Slave architecture** written in **Go (Golang)**. It allows executing SQL queries from the Master and automatically replicates them to connected Slave nodes. The system supports **synchronous replication** for data-level operations and maintains a local copy of the database at each Slave.

---

## 🧠 Project Idea

In distributed systems, data replication helps in high availability, fault tolerance, and scalability. This project demonstrates a simplified version of a **synchronous replication system**, where:

- All write operations (INSERT, UPDATE, DELETE) sent to the Master are forwarded to all Slaves.
- Slaves execute these operations on their local copy of the database immediately.
- SELECT queries are executed locally on the Slave.
- If the Master goes offline, Slaves keep running and reconnect automatically when it's back.

---

## 🔧 Technologies Used

- **Go (Golang)** — for backend logic and networking
- **MySQL** — relational database
- **TCP sockets** — for communication between Master and Slaves
- **godotenv** — for environment variable management

---

## 📁 Project Structure

+-------------+ +-------------+ +-------------+
| Client | ---> | Master | ---> | Slave |
| (query) | | (DB + TCP) | | (Local DB) |
+-------------+ +-------------+ +-------------+

* Master handles input, executes queries, and forwards to all connected Slaves.

* Slaves execute all queries locally (except restricted ones).

* If Master goes down, Slaves keep running and reconnect when it's back.


---

## ⚙️ Setup & Installation

### 1. Requirements

- Go installed (v1.16+ recommended)
- MySQL installed and running
- A MySQL database named `school` created

### 2. Clone the Project

```bash
git clone https://github.com/AbdoZaher/DDB/tree/main

```
------
3. Create .env File
Inside the root directory, create a .env file with your DB credentials:

DB_USER=root
DB_PASS=rootroot

4. Initialize Go Modules and Install Dependencies

go mod init distributed-db-replication
go get github.com/go-sql-driver/mysql
go get github.com/joho/godotenv

🚀 Running the Project
✅ Run the Master
On the Master machine or terminal:
go run Master.go
The Master listens on TCP port 8080 and waits for Slaves to connect.

✅ Run a Slave
On each Slave machine or terminal (can be the same or different machines):
go run Slave.go
Each Slave connects to the Master and listens for queries.

💡 Usage
You input SQL queries into the Master terminal.

The Master:

Executes the query on its own local DB.

Sends the query to all connected Slaves.

Slaves:

Receive the query.

Validate it's not a critical query (e.g., DROP TABLE is blocked).

Execute it on their local copy.

Only SELECT, INSERT, UPDATE, DELETE are allowed to replicate.

🔐 Query Restrictions
To prevent structural conflicts, the following queries are blocked on Slaves:

CREATE TABLE

ALTER TABLE

DROP TABLE

CREATE DATABASE

DROP DATABASE

These are allowed only on the Master.

🔁 Auto-Reconnect (Slave)
If the Master goes offline:

Each Slave detects the disconnection.

It will retry connecting every 5 seconds.

Once the Master is available again, the Slave reconnects automatically.

📌 Example
On Master:
Enter SQL query: INSERT INTO students(name, age) VALUES ('Ali', 22)
Master: Success
On Slave:
Received from master: INSERT INTO students(name, age) VALUES ('Ali', 22)

👨‍💻 Authors
Abdelrahman Ali Zaher
Ahmed Mohamed Ahmed

