# ROADMAP.md

> **Current Phase**: Not started
> **Milestone**: Prototype MVP

## Must-Haves (from SPEC)
- [ ] Audio -> Text processing pipeline (Hinglish support).
- [ ] Basic speaker diarization and dialogue classification.
- [ ] Engagement metrics logic (Talk time ratio, Participation Indicator).
- [ ] Demo UI (Streamlit) using pre-processed data.
- [ ] Public GitHub Repo & Streamlit Cloud deployment + README.

## Phases

### Phase 1: Batch Processing Pipeline (Data Extraction)
**Status**: ⬜ Not Started
**Objective**: Build local Python scripts to process the 5 audio files, utilizing Whisper for transcription and Pyannote for diarization. Output results as structured JSON.
**Requirements**: REQ-01, REQ-02

### Phase 2: NLP Analysis & Engagement Metrics
**Status**: ⬜ Not Started
**Objective**: Parse the JSON transcripts to calculate Teacher Dominance Ratio, Student Participation Indicator, and Interaction Count. Update JSON with metrics.
**Requirements**: REQ-03, REQ-04

### Phase 3: Streamlit Demo Interface
**Status**: ⬜ Not Started
**Objective**: Create a pp.py Streamlit dashboard that visualizes the pre-processed JSON data with transcripts, metrics, and summaries.
**Requirements**: REQ-05

### Phase 4: Documentation & Deployment
**Status**: ⬜ Not Started
**Objective**: Finalize the README, clean up the GitHub repository, and deploy the Streamlit Dashboard to Streamlit Cloud.
**Requirements**: REQ-06
