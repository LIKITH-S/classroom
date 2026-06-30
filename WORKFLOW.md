# Project Workflow: MakerGhat Classroom Analytics

This document details the step-by-step implementation of the four major phases required to build the Classroom Analytics MVP.

---

## 🏗 Phase 1: Batch Processing Pipeline (Transcription)

**Objective**: Extract high-fidelity Hinglish transcripts from raw classroom MP3 files using local GPU resources.

### **Technical Workflow:**
1.  **Environment Setup**: Configured a Python 3.9+ virtual environment and installed `faster-whisper`.
2.  **Windows/CUDA Fix**: Implemented `scripts/fix_dlls.py` to resolve `cublas64_12.dll` loading errors by injecting the required NVIDIA DLL paths into the runtime environment.
3.  **Hinglish Optimization**: 
    - Used the **Whisper-Medium** model for a balance between speed and quality.
    - Set `language="hi"` with an `initial_prompt` containing common Indian classroom phrases to bias the model toward Hinglish.
4.  **Batch Execution**: Created `scripts/process_audio.py` to iterate through the `audio/` folder and output structured JSON files for each session.
5.  **VAD Implementation**: Enabled `vad_filter=True` to automatically remove non-speech audio segments (ambient noise/silence).

---

## 📊 Phase 2: NLP Analysis & Engagement Metrics

**Objective**: Convert raw text segments into pedagogical insights and engagement scoring.

### **Technical Workflow:**
1.  **Data Ingestion**: Parsed the `utterances` from Phase 1 JSONs into **Pandas DataFrames**.
2.  **Hybrid Cleaning Engine**: 
    - Implemented a rule to detect **"Ghost Segments"**: `duration > 60s` AND `word_count < 5`.
    - These segments typically represent muffled noise and were excluded to prevent skewing Teacher Dominance metrics.
3.  **Metric Calculation**:
    - **Teacher Dominance**: Sum of teacher segment durations / Total session duration.
    - **Student Participation**: Ratio of student utterances to total utterances.
    - **Interaction Switches**: Count of speaker-label transitions (back-and-forth).
4.  **Quality Flagging**: Implemented **Transcription Density** (Words Per Minute). Sessions with < 5 WPM are tagged as "Incomplete Data."
5.  **Structure**: Metrics are saved both in the individual session JSONs and a global `summary_metrics.json`.

---

## 🎨 Phase 3: Streamlit Demo Interface (UI)

**Objective**: Create a professional, read-only dashboard for educators to visualize classroom dynamics.

### **Technical Workflow:**
1.  **Branding**: Styled the application with **MakerGhat official colors** (Green #00A859 & Yellow #FFD200) using custom CSS injection.
2.  **State Management**: Used `st.cache_data` to ensure the dashboard loads instantly from the pre-processed JSON artifacts.
3.  **Visualization Layer**:
    - **Plotly Express**: Used for dynamic talk-time distribution pie charts.
    - **Progress Bars**: Used for session health/density visualization.
4.  **Searchable Transcripts**: Implemented a "Chat-Style" UI for the transcripts, allowing users to filter by speaker ("Teacher only" or "Student only") and search for specific keywords.
5.  **Quality Badges**: Added dynamic banners that warn users if a session has the "Incomplete Data" tag.

---

## 🚀 Phase 4: Documentation & Deployment

**Objective**: Finalize the package for public handover and deploy it to the cloud.

### **Technical Workflow:**
1.  **Dependency Optimization**:
    - Created a lightweight `requirements.txt` for the Web (no heavy torch/whisper libs).
    - Created `requirements-dev.txt` for the full local AI pipeline.
2.  **Repository Hygiene**:
    - Configured a comprehensive `.gitignore` to exclude 2GB+ of models and audio files.
    - Manually cleared the Git cache to ensure no private agent metadata (`.gsd`, `.agent`) was tracked.
3.  **Documentation**: Authored a professional `README.md` including approach, assumptions, and Google Drive download links for assets.
4.  **Deployment**: 
    - Synced local code to **GitHub**.
    - Connected the repo to **Streamlit Cloud** with a custom `runtime.txt` (Python 3.11.2) to ensure a stable build environment.

---
*Created as a project lifecycle record for MakerGhat.*
