# Docker Networking Demo (Browser → Web → DB)

This is a **simple, reliable Docker networking demo** designed for **beginners**.
This demo shows how a browser talks to a web app, and the web app securely talks to a database.

---

## 🎯 Goal

**Browser → Web container → DB container**

Data is fetched from the database and shown in the browser.

---

## 🏗 Architecture

```
Browser
   ↓
http://localhost:8080
   ↓
Web Container (Python Flask)
   ↓
Docker Network
   ↓
DB Container (MySQL)
```

---

## ✅ Prerequisites

- Docker installed
- Docker running
- Internet access (to pull images)

---

## Step 1️⃣: Create Docker Network

```bash
docker network create app-net
```

This network allows containers to talk to each other securely using container names.

---

## Step 2️⃣: Run Database Container (MySQL)

```bash
docker run -d   --name db   --network app-net   -e MYSQL_ROOT_PASSWORD=root   -e MYSQL_DATABASE=demo   mysql:8.0
```

### Add Sample Data

```bash
docker exec -it db mysql -uroot -proot demo
```

```sql
CREATE TABLE users (id INT, name VARCHAR(20));
INSERT INTO users VALUES (1,'Alice'),(2,'Bob');
EXIT;
```

---

## Step 3️⃣: Create Web App (Flask)

Create a file named **app.py**

```python
from flask import Flask
import mysql.connector

app = Flask(__name__)

@app.route("/")
def home():
    db = mysql.connector.connect(
        host="db",
        user="root",
        password="root",
        database="demo"
    )
    cursor = db.cursor()
    cursor.execute("SELECT * FROM users")
    data = cursor.fetchall()
    return str(data)

app.run(host="0.0.0.0", port=5000)
```

📌 **Important:**  
The database hostname is **db**, not `localhost`.  
Docker provides DNS automatically inside the network.

---

## Step 4️⃣: Run Web Container

```bash
docker run -d   --name web   --network app-net   -p 8080:5000   -v $(pwd)/app.py:/app.py   python:3.9   sh -c "pip install flask mysql-connector-python && python /app.py"
```

---

## Step 5️⃣: Open Browser 🌐

Open:

```
http://localhost:8080
```

---

## 👀 What you will See

```text
[(1, 'Alice'), (2, 'Bob')]
```

🎉 **LIVE DATA from the database shown in the browser**

---

## 🧠 Learning Points (Very Important)

- Browser **never** talks to the database
- Database is **not exposed** to the outside world
- Web container talks to DB using container name `db`
- Docker network provides:
  - DNS resolution
  - Isolation
  - Secure communication

---

## 🔐 Security Lesson

> Only the web container is public.  
> The database stays private.

This is **exactly how production systems work**.

---

## 📝 One-Line Summary

**Docker networking lets web apps securely fetch data from databases and show it in the browser.**

---


