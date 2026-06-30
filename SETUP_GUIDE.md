# 🎓 MakerGhat Classroom Analytics: Setup & Execution Guide

This guide provides step-by-step instructions to set up your local development environment, configure the Python virtual environment (`venv`), install all required dependencies, and run the MakerGhat Classroom Analytics application.

---

## 📋 Prerequisites

Before starting, ensure you have the following installed on your system:

1. **Python 3.10 or 3.11** (Highly recommended. Python 3.12+ might have compatibility issues with older PyTorch/CUDA libraries).
2. **Git** (For version control and cloning the repository).
3. **NVIDIA GPU with CUDA Support** (Optional: Only required if you intend to re-run the `faster-whisper` audio transcription pipeline locally).

---

## 🛠️ Step-by-Step Setup

### Step 1: Clone the Repository
Open your terminal (PowerShell/CMD on Windows, or Terminal on macOS/Linux) and clone the project:
```bash
git clone https://github.com/LIKITH-S/classroom.git
cd classroom
```

### Step 2: Create a Virtual Environment (`venv`)
Create an isolated Python virtual environment named `.venv` in the project root:
```bash
python -m venv .venv
```
*Note: Creating a virtual environment ensures that the project's dependencies do not conflict with other Python projects on your system.*

### Step 3: Activate the Virtual Environment
Activate the environment depending on your operating system and shell:

#### **Windows (PowerShell - Recommended)**
```powershell
.\.venv\Scripts\Activate.ps1
```
*Note: If you get an execution policy error, run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process` first, then run the activation script.*

#### **Windows (Command Prompt / CMD)**
```cmd
.\.venv\Scripts\activate.bat
```

#### **macOS / Linux (Bash / Zsh)**
```bash
source .venv/bin/activate
```

*(You will know activation was successful when you see `(.venv)` prepended to your command line prompt.)*

### Step 4: Upgrade Pip & Install Dependencies
With the virtual environment active, run the following commands to upgrade package managers and install all the project dependencies:
```bash
python -m pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
```
*Note: The `requirements.txt` file is updated to contain all dependencies currently in use, including Streamlit for the dashboard, and `faster-whisper`, `torch`, and `sympy` for the transcription and analysis pipeline.*

### Step 5: Fix NVIDIA DLLs (Windows Only)
On Windows, pip-installed NVIDIA libraries (installed as dependencies of PyTorch and `faster-whisper`) often fail to load because they are not placed in the system search path. Run the included script to copy the required DLLs to the correct location:
```powershell
python scripts/fix_dlls.py
```

---

## 🚀 Running the Application

### 1. Launch the Streamlit Dashboard
To run the interactive analytics dashboard locally, execute:
```bash
streamlit run app.py
```
This will automatically open the dashboard in your default web browser (usually at `http://localhost:8501`).

### 2. Run the Processing Pipeline (Optional)
If you wish to download the raw classroom audio and run the transcription/metrics extraction locally:

1. **Download the Audio & Model Files** using the links in the main [README.md](file:///d:/AI%20classroom/README.md).
2. **Organize the Folders** in the project root as follows:
   ```text
   classroom/
   ├── audio/          <-- Place raw .mp3 files here
   ├── models/         <-- Place the downloaded "whisper-medium" folder here
   └── processed_data/
   ```
3. **Run the transcription pipeline**:
   ```bash
   python scripts/process_audio.py
   ```
4. **Run the metrics calculation**:
   ```bash
   python scripts/calculate_metrics.py
   ```
5. Re-run `streamlit run app.py` to see the new local outputs on the dashboard.

---

## 🔍 Troubleshooting

### ❌ Command `python -m venv .venv` fails
- Make sure Python is added to your system `PATH`.
- If you have multiple versions of Python installed, use `py -3.10 -m venv .venv` on Windows or `python3 -m venv .venv` on macOS/Linux.

### ❌ DLL load failed: The specified module could not be found (`cublas`, `cudnn`, etc.)
- This is common on Windows. Make sure you ran the `python scripts/fix_dlls.py` script.
- Ensure your system has the appropriate NVIDIA graphics driver installed.

### ❌ Out of Memory (OOM) on GPU
- The audio transcription script defaults to CUDA float16. If your GPU has less than 4GB VRAM, edit `scripts/process_audio.py` and set `DEVICE = "cpu"` or `COMPUTE_TYPE = "int8"`.
