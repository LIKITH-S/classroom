# SPEC.md — Project Specification

> **Status**: FINALIZED

## Vision
To build an offline-first MVP classroom analytics platform for MakerGhat that processes audio recordings to extract actionable insights on classroom engagement, demonstrating system design and pragmatic tool usage over perfect accuracy.

## Goals
1. Accurately transcribe Hindi/English mixed classroom audio (even with lower clarity on some samples).
2. Peform basic classroom interaction analysis (Teacher vs. Student utterances, question counting).
3. Extract simple engagement metrics (Teacher vs. Student talk time, Participation Indicator).
4. Present pre-processed results within a clean, intuitive Streamlit Cloud dashboard.
5. Provide a rigorous, transparent deployment architecture and README detailing assumptions and logic.

## Non-Goals (Out of Scope)
- Building a full production-ready, real-time pipeline.
- Training custom speech-to-text models from scratch.
- Achieving 100% transcription accuracy, especially for low-clarity audio.
- Implementing an enterprise user management or backend.

## Users
- MakerGhat evaluators who will test and review the prototype.
- Educators aiming to get rapid insights from their classroom audio.

## Constraints
- **Timeline**: 10 days.
- **Audio quality**: 2 out of 5 audio files have low voice clarity; mixed language (Hindi/English).
- **Tooling**: Heavy bias towards free, open-source models (Whisper, etc.) and free hosting (Streamlit Cloud).
- **Format**: Pre-processed backend (batch processing audio -> saving to JSON) to avoid timeouts in Streamlit Cloud.

## Success Criteria
- [ ] Working offline batch-processor script for audio -> text + metrics.
- [ ] Deployed Streamlit Cloud frontend showcasing results of the 5 audio files.
- [ ] Readme clearly documents assumptions, models used, and engagement metric formulas.
