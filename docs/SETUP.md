# Setup Guide for Partner Colleges

**K1 Wizard-of-Oz Dashboard**
Hillsborough College AI Innovation Center · AI PREP4WORK Initiative

---

This guide walks a new college through setting up the K1 Wizard-of-Oz Dashboard
from scratch. Estimated time: **1.5 to 2.5 hours** on first install.

---



***

# K1 Wizard-of-Oz Dashboard

## Windows Setup Guide with Technical Explanation

Hillsborough College AI Innovation Center · AI PREP4WORK Initiative

***

## Overview

This guide explains not only how to install the K1 Wizard-of-Oz Dashboard on a Windows system, but also why each step is necessary and what processes are occurring within the system.

The completed system integrates multiple components into a real-time pipeline:

* Speech recognition (Whisper)
* Language model processing (Ollama or cloud LLMs)
* Speech synthesis (Piper)
* Robot control (Booster SDK)
* Web-based control interface (Flask dashboard)

***

## System Architecture (Conceptual Model)

The system operates as a sequential pipeline:

```
Audio Input → Speech-to-Text → Language Model → Text-to-Speech → Robot Output
```

Each setup step enables one or more components of this pipeline.

***

## Step 1 — Clone the Repository

```powershell
git clone https://github.com/TechPlayzone/k1-wizard-of-oz.git
cd k1-wizard-of-oz
```

### Purpose

This step downloads the complete application source code from the remote GitHub repository to the local machine.

### Explanation

The repository contains:

* Backend application (Flask server)
* Frontend interface (dashboard)
* Integration modules for audio, AI processing, and robot control
* Configuration templates and scripts

The `git clone` operation creates a local copy of this project so it can be executed and modified. Without this step, the system cannot run because no executable code exists locally.

***

## Step 2 — Create the Configuration File

```powershell
copy .env.example .env
notepad .env
```

### Purpose

The `.env` file provides runtime configuration parameters required by the application.

### Explanation

This file defines critical system variables such as:

```
K1_IP=192.168.X.X
LLM_PROVIDER=ollama
ADMIN_PASSWORD=your_password
```

* `K1_IP`: Enables network communication with the robot
* `LLM_PROVIDER`: Determines which AI model is used
* `ADMIN_PASSWORD`: Secures system configuration access

At startup, the backend reads this file and configures its behavior accordingly. This approach separates configuration from code, which improves maintainability and security.

Without a valid `.env` file, the application cannot correctly locate or control the robot.

***

## Step 3 — Install FFmpeg

### Purpose

FFmpeg is required for audio preprocessing used by the Whisper speech recognition model.

### Explanation

Whisper does not directly accept raw microphone input. Audio must be:

* Decoded
* Reformatted
* Converted into a compatible waveform structure

FFmpeg performs these transformations.

For example:

* Converts compressed audio into PCM format
* Ensures consistent sampling rates
* Normalizes audio streams

Without FFmpeg, speech-to-text processing will fail, and the system will not be able to interpret spoken input.

***

## Step 4 — Install Python Dependencies

```powershell
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
```

### Purpose

This step installs all required Python libraries needed for the system to function.

### Explanation

The application depends on several libraries, including:

* Flask: Handles the web server and API endpoints
* Whisper: Performs speech recognition
* Piper: Generates synthesized speech
* Booster SDK: Interfaces with robot hardware
* LLM SDKs: Enable communication with AI models

The use of a virtual environment (`venv`) ensures that dependencies are isolated from the global Python installation. This prevents version conflicts and improves reproducibility.

Without installing these dependencies, the application will fail to start due to missing modules.

***

## Step 5 — Install Ollama and Pull Llama 3

```powershell
ollama pull llama3
```

### Purpose

This step installs the local language model used for generating responses.

### Explanation

Ollama is a runtime that hosts large language models locally. The `llama3` model provides the system with natural language understanding and generation capabilities.

When a user speaks, the transcribed text is passed to the language model, which produces a contextual response.

Advantages of using a local model include:

* No dependency on internet connectivity after installation
* No transmission of user data to external services
* Reduced latency for responses

Without this step, the system lacks the ability to generate intelligent responses and cannot complete the interaction loop.

***

## Step 6 — Download a Piper Voice Model

```powershell
mkdir voices
cd voices
```

(then download `.onnx` and `.json` files)

### Purpose

This step installs the speech synthesis model used to convert text into audible output.

### Explanation

After the language model generates a text response, it must be converted into audio. Piper performs this conversion using neural voice models.

The `.onnx` file represents the trained voice model, while the `.json` file contains configuration parameters such as phoneme mappings and synthesis settings.

The system uses the path defined in:

```
PIPER_VOICE_PATH
```

to locate and execute the model during runtime.

Without Piper, the system can generate text responses but cannot produce speech output.

***

## Step 7 — Connect to the K1 Robot

```powershell
python scripts\test_connection.py
```

### Purpose

This step verifies network connectivity between the local machine and the robot.

### Explanation

The robot is controlled over a TCP/IP network using its assigned IP address. The test script validates:

* Reachability of the K1 device
* Availability of the Ollama service
* Readiness of the dashboard server

If the robot is not reachable, commands such as movement or audio playback cannot be executed.

Reliable connectivity is essential because all control commands and audio streams are transmitted in real time.

***

## Step 8 — Start the Backend Application

```powershell
python backend\app.py
```

### Purpose

This starts the central orchestration service for the entire system.

### Explanation

The backend application performs multiple roles:

* Hosts the web dashboard
* Manages API endpoints
* Connects all subsystems (audio, AI, robot)
* Maintains session state and configuration

Internally, it routes data through the pipeline:

1. Receives audio input
2. Sends it to Whisper
3. Passes transcription to the language model
4. Sends generated text to Piper
5. Dispatches audio and commands to the robot

Without the backend running, none of the system components are connected or operational.

***

## Step 9 — Access the Dashboard

Navigate to:

```
http://localhost:5000
```

### Purpose

The dashboard provides a graphical interface for interacting with the system.

### Explanation

The dashboard allows users to:

* View conversation history
* Control robot movement
* Trigger gestures
* Monitor system status
* Select AI providers

The term `localhost` refers to the local machine, indicating that the server is running on the same system.

Without the dashboard, interaction would require direct API calls, which is impractical for most users.

***

## Integrated System Behavior

Once all steps are complete, the system operates as follows:

1. Audio is captured from the robot or microphone
2. FFmpeg processes the audio stream
3. Whisper converts audio into text
4. The language model generates a response
5. Piper synthesizes speech from the response
6. The robot plays the audio and executes any movement commands
7. The dashboard displays the interaction in real time

***

## Summary

Each step in the setup process enables a specific functional layer:

| Step                | Function                             |
| ------------------- | ------------------------------------ |
| Clone repository    | Provides application code            |
| Configuration file  | Defines runtime parameters           |
| FFmpeg              | Enables audio processing             |
| Python dependencies | Supports application execution       |
| Ollama              | Provides language model capabilities |
| Piper               | Enables speech synthesis             |
| Network connection  | Enables robot control                |
| Backend launch      | Integrates system components         |
| Dashboard access    | Provides user interface              |



***



---

*Hillsborough College AI Innovation Center · AI PREP4WORK Initiative*
*Deshjuana Bagley, Associate Dean, A.S. Degree Programs*
