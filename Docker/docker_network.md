# Docker Networking – Beginner (1 Hour)

This README is a **trainer-ready guide** to teach Docker Networking to **beginner-level students** in **1 hour**.  
You can **read this content directly** while teaching and run the demos live.

---

## Session Objective

By the end of this session, students will be able to:
- Understand why Docker networking is needed
- Create Docker networks
- Connect containers using container names
- Use port mapping to access containers
- Troubleshoot basic networking issues

---

## 0–5 Minutes | Why Docker Networking?

### Problem Statement
- Containers are **isolated by default**
- An application container must talk to:
  - Database container
  - Other services
  - Host machine
  - Internet

### Key Message
> Docker networking allows containers to discover and communicate with each other.

---

## 5–15 Minutes | Docker Networking Basics

### What is a Docker Network?
- A **virtual network** created and managed by Docker
- Containers on the same network can communicate
- Docker handles:
  - IP assignment
  - Routing
  - DNS

### List Docker Networks
```bash
docker network ls
```

### Common Networks
- **bridge** → default network  
- **host** → uses host network  
- **none** → no networking  

---

## Default Bridge Network (Important)
- Created automatically
- Containers get private IP addresses
- ❌ No automatic DNS resolution

### Problem
- Containers cannot communicate using **names**

---

## Container IP vs Container Name

### ❌ Bad Practice
- Communicating using IP addresses
- IP changes when container restarts

### ✅ Best Practice
- Use **container names**
- Docker provides **built-in DNS** on custom networks

> Always use container names, not IPs.

---

## 15–35 Minutes | Live Demo (Core Teaching)

### Step 1: Create Custom Network
```bash
docker network create demo-net
```

### Step 2: Run First Container
```bash
docker run -dit --name app --network demo-net busybox
```

### Step 3: Run Second Container
```bash
docker run -dit --name db --network demo-net busybox
```

### Step 4: Test Connectivity
```bash
docker exec -it app ping db
```

---

## Port Mapping (Container ↔ Host)

### Solution
```bash
docker run -d -p 8080:80 nginx
```

---

## Hands-on Lab

```bash
docker run -dit --name test1 busybox
docker run -dit --name test2 busybox
docker exec -it test1 ping test2
```

```bash
docker network connect demo-net test1
docker network connect demo-net test2
```

---

## Final Takeaway
> Always create a custom Docker network and communicate using container names.

---

## End of Session ✅
