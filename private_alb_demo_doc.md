# Private ALB Demo in Private VPC

This README explains the setup, issue encountered, and resolution for demonstrating **Private ALB → Private Target Group → Private EC2** in a classroom environment without internet access.

## 1. Setup
- **VPC:** Private subnets only
- **EC2 Instances:** Private, no public IP
- **Internal ALB:** Target group pointing to private EC2
- **Health check path:** `/`
- **Security Groups:** ALB → EC2 on port 80

## 2. Problem
After adding EC2 to the target group, the target showed **unhealthy**.

Local test on EC2:
```bash
curl localhost:80
curl: (7) Failed to connect to localhost port 80 after 0 ms: Could not connect to server
```

### Analysis
- No service running on port 80
- Private subnet → cannot install httpd without NAT Gateway
- ALB health check failed → target unhealthy

## 3. Solution
### Step 1: Start Python Web Server (No Installation Needed)
```bash
sudo nohup python3 -m http.server 80 &
```

### Step 2: Test Server Locally
```bash
curl localhost:80
```
Output: directory listing → confirms server running.

### Step 3: Verify Security Groups
- EC2 SG inbound: ALB-SG → port 80
- ALB SG inbound: internal clients → port 80

### Step 4: Test ALB from Private EC2
```bash
curl http://<ALB-DNS-Name>
```
- Directory listing confirms traffic is reaching EC2
- Multiple curls → different responses → load balancing working

## 4. Root Cause
- EC2 had no service on port 80
- Health check failed → target unhealthy
- Private subnet → no internet → cannot install httpd

## 5. Resolution Summary
| Issue | Fix |
|-------|-----|
| Target unhealthy | Start Python server on port 80 |
| Cannot install httpd | Use built-in Python server |
| Health check failing | Ensure path `/` served |
| No NAT / Internet | Python server works without NAT |

## 6. Teaching Points
1. Internal ALB is reachable only from inside VPC
2. Python `http.server` allows quick demos in private subnets
3. Target health depends on server running on correct port/path
4. Curl responses differ per EC2 → demonstrates load balancing
5. Security groups must allow ALB → EC2 traffic

## 7. Demo Validation Steps
1. Start Python server on all private EC2s
2. Test `curl localhost:80` → should return 200
3. ALB automatically marks targets healthy
4. Test ALB from EC2:
```bash
curl http://<ALB-DNS-name>
```
5. Curl multiple times → observe load balancing

**Result:** Successfully demonstrates **Private ALB → Private Target Group → Private EC2** without internet or package installations.