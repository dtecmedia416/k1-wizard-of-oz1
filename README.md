# K1 Wizard-of-Oz Dashboard

**A multi-provider AI voice pipeline and educator control dashboard for the Booster Robotics K1 humanoid robot.**

Developed by the **Hillsborough College AI Innovation Center**
*Deshjuana Bagley, Associate Dean, A.S. Degree Programs*

Part of the **AI PREP4WORK Initiative** — funded by the U.S. Department of Education
FIPSE Grant Program.

---

> **This project is built for educators.** Any college with a Booster K1 EDU robot and a
> laptop can clone this repo and have a working AI voice pipeline and browser-based
> Wizard-of-Oz control dashboard running in under two hours.

---

## What this is

The K1 Wizard-of-Oz Dashboard replaces the K1's default Doubao/ByteDance LLM with a
fully customizable, FERPA-safe AI pipeline. An educator controls the robot from a
browser dashboard while students interact with it naturally — asking questions, watching
it respond with voice and movement.

**Three LLM backends are supported out of the box:**

| Provider | Requires | Internet? |
|---|---|---|
| Ollama (Llama 3) | Local inference server | No |
| Anthropic Claude | Session API key | Yes |
| OpenAI GPT-4o | Session API key | Yes |

**The pipeline:**
```
K1 mic array → Whisper STT → LLM (your choice) → Piper TTS → K1 speaker + movement
```

**The dashboard provides:**
- Live chat conversation panel with LLM provider selector
- Movement controls (walk forward/backward, turn left/right, stop)
- Gesture buttons (wave, nod, thumbs up) with robot silhouette icons
- Real-time camera feed toggle — switch between live K1 camera and NVIDIA Isaac Sim
- Robot status strip (mode, battery, latency, last action)
- Voice selector (Piper TTS local voices + OpenAI TTS cloud voices)
- Session-scoped API keys — never stored, cleared on tab close
- Admin-only configuration console (K1 IP, network, provider defaults)

---

## Hardware requirements

| Component | Minimum | Notes |
|---|---|---|
| Robot | Booster K1 EDU | Firmware v1.6+ recommended |
| Inference server | 16 GB RAM, NVIDIA GPU | Your laptop works; Dell R770 recommended |
| Network | Local WiFi | Isolated hotspot recommended for security |
| Browser | Chrome 120+ or Firefox 120+ | For dashboard |

---

***

# K1 Wizard-of-Oz Dashboard — Windows Setup Guide

## Windows Environment Notes

* Recommended: **Windows 11 (64-bit)**
* Use **PowerShell** (preferred) or Command Prompt
* Optional but helpful: **Windows Terminal**
* You may need **Administrator rights** for some installs

***

#  Software Requirements (Windows)

Install the following:

### 1. Python

* Install **Python 3.10+** from: <https://www.python.org/downloads/>
* During install, check: `Add Python to PATH`

Verify:

```powershell
python --version
pip --version
```

***

### 2. Git

Download: <https://git-scm.com/download/win>

Verify:

```powershell
git --version
```

***

### 3. FFmpeg (Required for Whisper)

1. Download from: <https://ffmpeg.org/download.html>  
   (Use Windows builds like gyan.dev)

2. Extract to:

```
C:\ffmpeg
```

3. Add to PATH:

* Search **“Environment Variables”**
* Edit `Path`
* Add:

```
C:\ffmpeg\bin
```

Verify:

```powershell
ffmpeg -version
```

***

### 4. Ollama (Local LLM option)

Download: <https://ollama.com/download/windows>

Verify:

```powershell
ollama --version
```

Pull model:

```powershell
ollama pull llama3
```

***

### 5. Piper TTS (Windows)

* Download Windows release from:
  <https://github.com/rhasspy/piper/releases>

* Extract to something like:

```
C:\piper
```

* Download at least one voice model (e.g., `en_US-lessac-medium`)

Example structure:

```
C:\piper\
  piper.exe
  en_US-lessac-medium.onnx
  en_US-lessac-medium.onnx.json
```

***

### 6. Python Dependencies

These will be installed from `requirements.txt`, but include:

* Flask
* Whisper
* Booster Robotics SDK
* etc.

***

# Quick Start (Windows)

## 1. Clone Repo

```powershell
git clone https://github.com/TechPlayzone/k1-wizard-of-oz.git
cd k1-wizard-of-oz
```

***

## 2. Create Virtual Environment (Recommended)

```powershell
python -m venv venv
.\venv\Scripts\activate
```

***

## 3. Install Dependencies

```powershell
pip install -r requirements.txt
```

***

## 4. Create Environment Config

Windows doesn’t support `cp`, so use:

```powershell
copy .env.example .env
```

OR:

```powershell
copy-item .env.example .env
```

Edit `.env` in Notepad or VS Code:

```powershell
notepad .env
```

Set values:

```
K1_IP=192.168.X.X
LLM_PROVIDER=ollama
OLLAMA_URL=http://localhost:11434
```

***

## 5. Start Ollama (if using local LLM)

```powershell
ollama serve
```

(Leave this running in a separate terminal)

***

## 6. Run Backend

```powershell
python backend/app.py
```

***

## 7. Open Dashboard

Open browser:

```
http://localhost:5000
```

***

# Windows Replacements for Scripts

The repo includes `.sh` scripts that won’t run natively on Windows. Use these equivalents:

### `install.sh` → Manual (already covered above)

### `run.sh` → PowerShell alternative

Create a file `run.ps1`:

```powershell
# Activate venv
.\venv\Scripts\activate

# Start backend
python backend/app.py

# Open browser
Start-Process "http://localhost:5000"
```

Run:

```powershell
.\run.ps1
```

***

### `test_connection.py`

Run normally:

```powershell
python scripts\test_connection.py
```

***

# Networking (Windows Tips)

* Ensure **Windows Firewall allows Python**
* Use **Private Network** setting for your WiFi
* Confirm K1 is reachable:

```powershell
ping 192.168.X.X
```

***

# Optional: NVIDIA Isaac Sim (Windows)

If running locally:

* Ensure GPU drivers (NVIDIA) are installed
* WebRTC stream default:

```
http://<your-ip>:8211/streaming/webrtc-demo/
```

***

# Windows-Specific Gotchas

### 1. Path Issues

* Use double backslashes in config if needed:

```
C:\\piper\\piper.exe
```

***

### 2. Microphone Permissions

* Windows Settings → Privacy → Microphone → Allow access

***

### 3. GPU Support for Whisper (Optional)

Install PyTorch with CUDA if you want acceleration:
<https://pytorch.org/>

***

### 4. Long Path Errors

Enable long paths (if needed):

```powershell
git config --system core.longpaths true
```

***

# Summary (Windows Flow)

1. Install Python, Git, FFmpeg
2. Install Ollama + Piper
3. Clone repo
4. Create `.env`
5. Install dependencies
6. Run backend
7. Open browser UI

***



---

## License

This project is licensed under the
[Creative Commons Attribution-NonCommercial 4.0 International License](LICENSE).

You are free to use, adapt, and share this work for **educational, non-commercial
purposes** with attribution to Hillsborough College AI Innovation Center.

---

## Acknowledgment

*This project was supported by the U.S. Department of Education under the Fund for the
Improvement of Postsecondary Education (FIPSE), Grant Award No. [GRANT NUMBER].
The contents of this publication do not necessarily represent the policy of the
U.S. Department of Education, and you should not assume endorsement by the Federal
Government.*

---

## About

**Hillsborough College AI Innovation Center**
Dale Mabry Campus · Tampa, Florida

**Deshjuana Bagley**
Associate Dean, A.S. Degree Programs
AI PREP4WORK Initiative — FIPSE Grant Program

**TechPlayzone, Inc.**
https://github.com/TechPlayzone
