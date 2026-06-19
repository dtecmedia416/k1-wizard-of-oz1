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

## Software requirements

- Python 3.10+
- [Ollama](https://ollama.com) with `llama3` model pulled
- [Piper TTS](https://github.com/rhasspy/piper) + at least one voice model
- [OpenAI Whisper](https://github.com/openai/whisper) (`pip install openai-whisper`)
- Booster Robotics Python SDK (`pip install booster_robotics_sdk_python`)
- Flask (`pip install flask`)
- ffmpeg (required by Whisper)

---


***

# Quick Start (Windows)

## 1. Check Python

Open **PowerShell** and run:

```powershell
python --version
```

**Use:**

* Python 3.10.x (recommended)
* Python 3.11.x

**Avoid:**

* Python 3.12+

If Python is not installed, download it:  
<https://www.python.org/downloads/windows/>

Restart PowerShell and check again.

***

## Fix: Python Not Found (Windows)

If you see:

```
Python was not found...
```

Go to:

```
Settings → Apps → Advanced app settings → App execution aliases
```

Turn OFF:

* Python.exe
* Python3.exe

Restart PowerShell.

***

# Setup

## 2. Clone the Repository

```powershell
git clone https://github.com/TechPlayzone/k1-wizard-of-oz.git
cd k1-wizard-of-oz
```

***

## 3. Install Dependencies

```powershell
pip install -r requirements.txt
```

***

## 4. Create Config File

```powershell
copy .env.example .env
notepad .env
```

Update:

```
K1_IP=<your robot IP>
LLM_PROVIDER=ollama
OLLAMA_URL=http://localhost:11434
```

***

## 5. Install AI Model

```powershell
ollama pull llama3
```

***

## 6. Start the App

```powershell
python backend\app.py
```

***

## 7. Open Dashboard

Go to:

```
http://localhost:5000
```

***

# Notes

* K1 and your computer must be on the same network
* Run `ollama serve` if needed
* Check `.env` if something does not work

***


Full setup guide for partner colleges: [`docs/SETUP.md`](docs/SETUP.md)

---

## Repository structure

```
k1-wizard-of-oz/
├── README.md                  ← You are here
├── LICENSE                    ← CC BY-NC 4.0
├── .env.example               ← Config template
├── requirements.txt           ← Python dependencies
│
├── frontend/
│   └── index.html             ← Wizard-of-Oz dashboard (single file)
│
├── backend/
│   ├── app.py                 ← Flask server — main entry point
│   ├── config.py              ← Load and validate .env config
│   ├── llm_router.py          ← Route to Ollama / Anthropic / OpenAI
│   ├── k1_handler.py          ← Booster SDK — movement + audio
│   ├── stt.py                 ← Whisper speech-to-text
│   ├── tts.py                 ← Piper TTS synthesis
│   └── session_manager.py     ← In-memory session API key store
│
├── scripts/
│   ├── install.sh             ← One-command dependency installer
│   ├── run.sh                 ← Start Flask + open dashboard
│   └── test_connection.py     ← Verify K1 reachability before demo
│
└── docs/
    ├── SETUP.md               ← Step-by-step for partner colleges
    ├── API_REFERENCE.md       ← Flask endpoint documentation
    ├── TROUBLESHOOTING.md     ← Common issues and fixes
    └── FIPSE_ACKNOWLEDGMENT.md← Required grant acknowledgment
```

---

## Security note

The K1 EDU runs firmware with ByteDance/Doubao as the default LLM. This pipeline
**completely bypasses Doubao** — no student voice data ever reaches ByteDance servers.

For additional isolation, we recommend connecting the K1 and your inference server to a
**dedicated hotspot** (e.g. Franklin T10 or Inseego 5G) that is separate from your
institution's campus network. See [`docs/SETUP.md`](docs/SETUP.md) for details.

---

## NVIDIA Isaac Sim integration

The dashboard includes a toggle to switch the Live panel between the K1's real camera
feed and an embedded NVIDIA Isaac Sim view. Isaac Sim must be running on your inference
server with WebRTC streaming enabled.

Default stream URL: `http://<your-server-ip>:8211/streaming/webrtc-demo/`

See [`docs/SETUP.md`](docs/SETUP.md) → *Isaac Sim setup* for configuration steps.

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
