# 🐧 WSL2 + Kali (Learning) + Docker Desktop (Windows) — Setup & Troubleshooting Journey

This repo documents my **real troubleshooting journey** on Windows 11 while:

- Installing **WSL2** to **learn Kali Linux**
- Installing **Docker Desktop on Windows** to learn Docker (and later tools like automation platforms)
- Fixing the **conflicts** that happened when WSL wasn’t healthy, which caused Docker Desktop to fail or behave strangely

It’s written as both a **guide** (clean steps) and **notes** (what went wrong + what fixed it).

---

## 📌 Quick Summary (How I Use This Setup)

- ✅ **Kali Linux** runs inside **WSL2** for learning/practice
- ✅ **Docker runs on Windows via Docker Desktop**
- ⚙️ Docker Desktop can use the **WSL2 backend**; when WSL is broken, Docker can also break
- 🔌 WSL Integration in Docker Desktop is **optional** unless you want Docker CLI/tools inside a distro

---

## 🎯 Goals

- Get **WSL2 stable** on Windows 11
- Install **Kali from Microsoft Store** and keep it working reliably
- Install **Docker Desktop** and avoid WSL/Docker conflicts
- Document the **real errors** and the fixes that worked

---

## 🧾 Environment

| Item | Details |
| --- | --- |
| **OS** | Windows 11 Home |
| **WSL** | WSL2 (Kali Linux via Microsoft Store) |
| **Docker** | Docker Desktop (Windows) |
| **Shell** | PowerShell / CMD (Admin) |
| **Hardware** | Virtualization enabled (Intel VT-x / AMD SVM) |

---

## ✅ Clean Setup Guide (Recommended Order)

### 1) Enable required Windows features

```powershell
# Run in an elevated PowerShell / Terminal
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Restart Windows after enabling features.

### 2) Install / update WSL

```powershell
wsl --install
wsl --set-default-version 2
wsl --update
```

### 3) Install Kali Linux (Microsoft Store)

- Install **Kali Linux** from Microsoft Store
- Launch it once to complete initial setup

Useful checks:

```powershell
wsl -l -v
```

### 4) Install Docker Desktop (Windows)

- Install Docker Desktop normally on Windows
- Choose/keep **Use WSL 2 based engine** (recommended)

### 5) (Optional) Enable Docker → WSL Integration

Only needed if you want Docker tools inside a distro:

- Docker Desktop → **Settings** → **Resources** → **WSL Integration**
- Enable integration for the distro(s) you want

---

## ❌ What Went Wrong (Conflicts I Hit)

These are the main conflict patterns I ran into:

1. **WSL not starting**
   - Error: `Wsl/0x80070422` → *The service cannot be started…*
   - Usually means a required Windows service is disabled

2. **Missing WSL service / WSL not properly registered**
   - `LxssManager` service not found
   - WSL commands failing completely

3. **Docker Desktop depends on WSL2 being healthy**
   - When WSL is broken, Docker Desktop (WSL2 backend) may fail to start or behave inconsistently

4. **Install/repair failures**
   - `wsl --install` → *The system cannot find the path specified*
   - Partial installs can leave Windows in a half-configured state

5. **Kali Linux WSL crash**
   - `ERROR_FILE_NOT_FOUND` (often `ext4.vhdx` missing)
   - Can happen if the distro storage is corrupted or removed

---

## 🛠️ Fixes That Helped (My Checklist)

### A) Confirm WSL status

```powershell
wsl --status
wsl -l -v
```

### B) Restart WSL

```powershell
wsl --shutdown
```

### C) Ensure required services are enabled

These mattered a lot in my case:

- **Windows Update**
- **Background Intelligent Transfer Service (BITS)**
- **VM Compute Service** (`vmcompute`)

### D) Repair WSL

```powershell
wsl --update
```

If things are badly broken, re-check Windows features (Step 1) and restart.

### E) Reset a broken distro (last resort)

If a distro is corrupted (example: missing `ext4.vhdx`), unregistering it resets it completely:

```powershell
wsl --unregister <distro-name>
```

Then reinstall Kali from Microsoft Store.

---

## 🧠 Key Findings (What I Learned)

- WSL is not “just an app” → it relies on **Windows features + services**.
- Docker Desktop with the WSL2 backend will often fail if WSL is broken.
- Error **`0x80070422`** usually points to a **disabled Windows service**.
- If **`LxssManager` is missing**, WSL is likely **not installed/registered correctly**.
- Kali is great for learning, but it’s best to keep **Docker Desktop as the Docker runtime on Windows** to reduce complexity.

---

## 🐳 Final Setup (Working)

```text
Windows 11
  ├─ WSL2: Kali Linux (learning)
  └─ Docker Desktop: Docker runtime on Windows (WSL2 backend)
```

---

## 🏷️ Tags

`WSL` `Docker Desktop` `Windows 11` `Kali Linux` `Troubleshooting`

---

## ⭐ Purpose

This repository exists to:

- Document real-world setup problems
- Keep a personal troubleshooting log
- Help others fix WSL2 + Docker Desktop conflicts faster

If you found this helpful, consider starring the repo.
