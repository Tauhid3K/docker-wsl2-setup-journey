# 🐧 WSL2 + Docker Desktop Setup & Troubleshooting Journey (Windows 11)

This repository documents my **real-world debugging journey** setting up and fixing **WSL2 (Windows Subsystem for Linux)** and **Docker Desktop** on Windows 11.  
Instead of a smooth setup, I faced multiple system-level issues, broken services, and installation failures. This repo is a log of those problems and how I solved them.

---

## 👋 About This Repository

- Not just a setup guide → a **debugging log** of Windows system-level issues.  
- Documents **errors, fixes, and lessons learned** while building a dev environment.  
- Useful for anyone struggling with WSL2 + Docker Desktop integration on Windows 11.

---

## 🎯 Goal

- Install and run **WSL2 (Ubuntu + Kali Linux)**  
- Run **Docker Desktop** with WSL2 backend  
- Understand **Windows service dependencies**  
- Document the **real debugging process**  

---

## ⚙️ Environment

- **OS:** Windows 11 Home  
- **Docker Desktop:** Latest version  
- **WSL2:** Ubuntu + Kali Linux (attempted)  
- **Shells:** PowerShell / CMD (Admin mode)  
- **Hardware:** Virtualization enabled (Intel VT-x / AMD SVM)  

---

## ❌ Problems Faced

1. **WSL not starting**  
   - Error: `Wsl/0x80070422` → *The service cannot be started...*  
   - Cause: Disabled/missing Windows services  

2. **Missing WSL service**  
   - `LxssManager` service not found  
   - WSL commands failing completely  

3. **Docker dependency issue**  
   - Docker Desktop required WSL2 backend  
   - Docker failed to start due to broken WSL  

4. **Installation failures**  
   - `wsl --install` → *The system cannot find the path specified*  
   - Partial installs breaking WSL  

5. **Broken system state**  
   - Inconsistent Windows feature registration  
   - Missing services (VM Compute, Update)  
   - Hyper-V confusion on Windows 11 Home  

6. **Kali Linux WSL crash**  
   - `ERROR_FILE_NOT_FOUND (ext4.vhdx missing)`  
   - Corrupted/missing WSL virtual disk  

7. **Docker + WSL integration failure**  
   - Docker failed to connect to Kali Linux  
   - Integration settings missing  

---

## 🧪 What I Tried

- Enabled Windows features manually:  
  - `Microsoft-Windows-Subsystem-Linux`  
  - `VirtualMachinePlatform`  

- Used **DISM restore commands**  
- Restarted system multiple times  
- Tried `wsl --install` and `winget install Microsoft.WSL`  
- Checked Windows services (`LxssManager`, `vmcompute`)  
- Attempted Docker reinstall  
- Reset/unregistered broken distros  

---

## 🛠️ Debugging & Fixes

### ✔ Enable Required Windows Services
- Windows Update  
- Background Intelligent Transfer Service (BITS)  
- VM Compute Service  

### ✔ Enable Windows Features
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
✔ Enable BIOS Virtualization
Intel VT-x / AMD SVM enabled in BIOS

✔ Install WSL Properly
powershell
wsl --install
wsl --set-default-version 2
✔ Reset/Reinstall WSL (if broken)
powershell
wsl --shutdown
wsl --unregister <distro-name>
✔ Install Ubuntu (stable choice)
powershell
wsl --install -d Ubuntu
✔ Fix Docker Integration
Docker Desktop → Settings → Resources → WSL Integration

Enable integration with Ubuntu/Kali Linux

📌 Key Findings
WSL is not just an app → it’s a Windows system feature

Docker Desktop depends heavily on WSL2 backend

If LxssManager is missing → WSL is not properly registered

Error 0x80070422 → usually means service disabled/not created

Partial installs can break WSL completely

💡 Lessons Learned
Always check Windows services before installing WSL/Docker

Avoid disabling system features unless necessary

WSL issues are often system-level, not app-level

Docker + WSL integration is tightly coupled on Windows 11

Sometimes a full repair install is the fastest fix

🐳 Final Setup Architecture
Code
Windows 11
   ├── WSL2 (Ubuntu + Kali Linux)
   ├── Docker Desktop (WSL2 backend)
   └── Tools (n8n, dev containers, VS Code)
🎯 Result
✔ WSL2 restored and functional

✔ Ubuntu + Kali Linux working properly

✔ Docker Desktop integrated with WSL2

✔ Stable development environment achieved

🚀 Future Plan
Successfully restore Kali Linux environment

Reinstall Docker Desktop with proper integration

Build a stable dev environment (WSL + Docker + VS Code)

Document final working setup with screenshots

🧑‍💻 Author
Tauhid Shahriar  
CSE Student | Developer | IoT & Android Enthusiast

🧠 Tags
WSL Docker Windows 11 Kali Linux DevOps Troubleshooting System Administration

⭐ Purpose
This repository is created to:

Document real-world setup problems

Help beginners fix WSL2 + Docker issues

Share troubleshooting experience

Save time for others facing the same errors

If you found this helpful, feel free to star ⭐ the repo or open an issue if you’re stuck — I’ll try to help.
