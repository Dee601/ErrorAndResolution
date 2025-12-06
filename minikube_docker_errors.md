# Minikube Setup Errors & Resolutions (macOS M1/M2/M3/M4)

This is the **README.md** version documenting errors and fixes during Minikube + Docker setup on Apple Silicon.

---

## 1. ❌ Error: `zsh: bad CPU type in executable: kubectl`
### **Cause:**
Installed wrong binary (Intel/amd64) on an ARM Mac.

### **Fix:**
Install ARM version:
```bash
brew install kubectl
```
Or manual:
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

---

## 2. ❌ Error: Minikube cannot find a driver
```
Unable to pick a default driver
No possible driver was detected
```
### **Cause:**
Minikube needs Docker/vfkit/VirtualBox, none installed.

### **Fix:**
Install Docker Desktop:
```bash
brew install --cask docker
open /Applications/Docker.app
```
Start Minikube:
```bash
minikube start --driver=docker
```

---

## 3. ❌ Error: `zsh: command not found: brew`
### **Cause:**
Homebrew not installed.

### **Fix:**
Install Homebrew:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Add to PATH:
```bash
eval "$($(brew --prefix)/bin/brew shellenv)"
```

---

## 4. ℹ️ Xcode Command Line Tools Installation
If you see:
```
Installing Command Line Tools for Xcode
```
This is normal. Wait for completion.

---

## 5. ❌ Error: Docker not running
```
failed to connect to the docker API at unix:///var/run/docker.sock
```
### **Cause:**
Docker Desktop daemon not active.

### **Fix:**
Start Docker:
```bash
open /Applications/Docker.app
```
Check:
```bash
docker info
```
Then:
```bash
minikube start --driver=docker
```
---

## ✅ Final Summary
- Use ARM binaries
- Install Homebrew
- Install Docker Desktop
- Start Docker daemon before Minikube
- Use: `--driver=docker`

---

If you want, I can export this README.md as a **PDF**, **md file**, or **GitHub-ready format**.