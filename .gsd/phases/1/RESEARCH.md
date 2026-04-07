---
phase: 1
level: 2
researched_at: 2026-04-07
---

# Phase 1 Research: Audio Processing & Data Extraction

## Questions Investigated
1. How to optimize aster-whisper for Hinglish (mixed Hindi/English) classroom audio?
2. What are effective text-based heuristics for identifying "Teacher" vs "Student" without full diarization?
3. How to mitigate hallucinations and errors in low-quality audio files?

## Findings

### 1. Hinglish Optimization
aster-whisper performs best on Hinglish when guided by an initial_prompt. 
- **Recommendation**: Provide a prompt containing 2-3 sentences of mixed Hindi-English (e.g., "Students, please open your books. Aaj hum naya chapter start karenge.").
- **Language Code**: Setting language="hi" (Hindi) is safer than None to ensure the character set is correct, as Whisper handles English mixed into Hindi better than the reverse.
- **VAD Filter**: Enable ad_filter=True to prevent repetitions and hallucinations in silent/noisy gaps.

### 2. Teacher vs Student Heuristics
Since we are skipping pyannote.audio (diarization), we will use a multi-signal heuristic:
- **Dominance**: The speaker with the most cumulative talk time is likely the **Teacher**.
- **Interaction Patterns**: Identify "Questions" (sentences ending in ? or starting with "Kya", "Why", "How"). The person asking questions and then receiving a short response (< 3s) is likely the **Teacher**.
- **Keywords**: Search for "Pedagogical moves" like "Listen", "Class", "Problem", "Chapter", "Homework".

### 3. Low-Quality Audio Mitigation
- **VAD Threshold**: Adjust ad_parameters to be more sensitive to speech frequencies.
- **Filtering**: Drop segments with very low average log-probability (confidence) or segments that are essentially pure noise/silence (< 0.5s).

## Decisions Made
| Decision | Choice | Rationale |
|----------|--------|-----------|
| Model Size | small or medium | medium offers significantly better Hinglish accuracy for complex classroom discourse if local RAM allows (~5GB). Fallback to small. |
| Diarization | Segment-based Heuristic | Avoids dependency on gated HF models and heavy compute while meeting "approximation" goal. |
| Language | hi with initial prompt | Best consistency for Hinglish according to community benchmarks. |

## Dependencies Identified
| Package | Version | Purpose |
|---------|---------|---------|
| aster-whisper | Latest | High-performance CTranslate2 based transcription. |
| scipy | Latest | For basic audio intensity/VAD checks if needed. |
| 	qdm | Latest | Progress tracking during batch processing. |

## Risks
- **Hallucination**: Whisper might generate repetitive text in low-clarity files. Mitigation: ad_filter=True and 
o_speech_threshold.
- **Diarization Error**: Heuristics might mislabel a very active student as the teacher. Mitigation: Use "Most prominent speaker" as a fallback and allow manual overrides in metadata if needed.

## Ready for Planning
- [x] Questions answered
- [x] Approach selected
- [x] Dependencies identified
