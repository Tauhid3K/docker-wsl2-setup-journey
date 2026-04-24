# 🐧 WSL2 + Docker Desktop Setup & Troubleshooting Journey (Windows 11)

A practical, step-by-step record of my **real debugging journey** while setting up **WSL2 (Windows Subsystem for Linux)** and **Docker Desktop** on Windows 11.

This is **not just a clean “how-to”**—it includes the messy parts: broken services, missing features, confusing errors, and the fixes that finally worked.

---

## 📌 What’s Inside

- ✅ A **setup checklist** for WSL2 + Docker Desktop (WSL backend)
- 🧩 Common **error messages** and what they actually mean
- 🛠️ The **fixes I tried** (including what *didn’t* work)
- 🧠 Lessons learned + final working setup

---

## 🎯 Goals

- Install and run **WSL2** (Ubuntu + Kali Linux)
- Run **Docker Desktop** with the **WSL2 backend**
- Understand critical **Windows service & feature dependencies**
- Document the **real troubleshooting process** for future reference

---

## 🧾 Environment

| Item | Details |
| --- | --- |
| **OS** | Windows 11 Home |
| **WSL** | WSL2 (Ubuntu + Kali Linux attempted) |
| **Docker** | Docker Desktop (latest) |
| **Shell** | PowerShell / CMD (Admin) |
| **Hardware** | Virtualization enabled (Intel VT-x / AMD SVM) |

---

## ❌ Problems Faced (Real Errors)

1. **WSL not starting**
   - Error: `Wsl/0x80070422` → *The service cannot be started…*
   - Cause: Disabled/missing Windows services

2. **Missing WSL service**
   - `LxssManager` service not found
   - WSL commands failing completely

3. **Docker dependency issue**
   - Docker Desktop requires WSL2 backend
   - Docker failed to start because WSL was broken

4. **Installation failures**
   - `wsl --install` → *The system cannot find the path specified*
   - Partial installs caused a corrupted/broken WSL state

5. **Broken system state**
   - Inconsistent Windows feature registration
   - Missing services (VM Compute, Windows Update, etc.)
   - Hyper‑V confusion on Windows 11 Home

6. **Kali Linux WSL crash**
   - `ERROR_FILE_NOT_FOUND` (`ext4.vhdx` missing)
   - Corrupted/missing WSL virtual disk

7. **Docker + WSL integration failure**
   - Docker couldn’t connect to Kali Linux
   - Integration toggles missing / not showing distros

---

## 🧪 What I Tried

### Windows features (manual enable)

- `Microsoft-Windows-Subsystem-Linux`
- `VirtualMachinePlatform`

### Recovery attempts

- DISM repairs / restore health
- Multiple restarts
- `wsl --install` and `winget install Microsoft.WSL`
- Service checks (`LxssManager`, `vmcompute`, Windows Update)
- Docker reinstall
- Reset/unregister broken distros

---

## 🛠️ Fixes That Helped (Checklist)

### 1) Enable required Windows features

```powershell name=README.md
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### 2) Ensure required services are enabled

These mattered a lot in my case:

- **Windows Update**
- **Background Intelligent Transfer Service (BITS)**
- **VM Compute Service** (`vmcompute`)

### 3) Enable BIOS virtualization

Make sure this is enabled in BIOS/UEFI:

- **Intel VT-x** or **AMD SVM**

### 4) Install / repair WSL properly

```powershell name=README.md
wsl --install
wsl --set-default-version 2
```

If WSL is broken and needs a reset:

```powershell name=README.md
wsl --shutdown
wsl --unregister <distro-name>
```

### 5) Install Ubuntu (stable baseline distro)

```powershell name=README.md
wsl --install -d Ubuntu
```

### 6) Fix Docker integration

In **Docker Desktop**:

- **Settings → Resources → WSL Integration**
- Enable integration for your distro(s) (Ubuntu/Kali)

---

## 🧠 Key Findings

- WSL is not “just an app” → it’s a **Windows system feature**.
- Docker Desktop depends heavily on the **WSL2 backend**.
- If **`LxssManager` is missing**, WSL is **not properly registered**.
- Error **`0x80070422`** usually means a **required service is disabled/not created**.
- Partial installs can leave Windows in a **half-configured state**.

---

## 💡 Lessons Learned

- Check Windows **services and features** before blaming Docker.
- Avoid disabling “unnecessary” services without understanding dependencies.
- Many WSL failures are **system-level**, not distro-level.
- Sometimes a **repair install** is faster than days of debugging.

---

## 🐳 Final Setup Architecture (Working)

```text name=README.md
Windows 11
  ├─ WSL2 (Ubuntu + Kali Linux)
  ├─ Docker Desktop (WSL2 backend)
  └─ Tools (n8n, dev containers, VS Code)
```

### ✅ Result

- ✔ WSL2 restored and functional
- ✔ Ubuntu + Kali Linux working properly
- ✔ Docker Desktop integrated with WSL2
- ✔ Stable development environment achieved

---

## 🚀 Future Plan

- Restore Kali Linux environment cleanly
- Reinstall Docker Desktop with proper integration
- Document the final stable setup with screenshots

---


## 🏷️ Tags

`WSL` `Docker` `Windows 11` `Kali Linux` `Ubuntu` `DevOps` `Troubleshooting` `System Administration`

---

## ⭐ Purpose

This repository exists to:

- Document real-world setup problems
- Help beginners fix WSL2 + Docker issues
- Share troubleshooting experience
- Save time for others facing the same errors

If you found this helpful, consider starring the repo ⭐ or opening an issue if you’re stuck.
