## Phase 2 Decisions

**Date:** 2026-04-08

### Scope
- **Metric Refinement**: Filter out segments with high duration and low word count (Ghost Segments). 
- **Core Metrics**: Stuck to Teacher Dominance, Student Participation, Interaction Switches, and Questions Asked.
- **Detailed Reports**: Chose **Option B** (Per-Class Detailed Reports) instead of a single central summary.
- **Transparency Flags**: Mark files with low transcription density as "Incomplete Data".

### Approach
- **Ghost Segment Rule**: Exclude segments with `duration > 60s` AND `word_count < 5`.
- **Quality Metric**: Calculate `words_per_minute`. If < 2, flag as `quality="Incomplete"`.

### Constraints
- No additional NLP (Topic detection/Sentiment) to keep MVP focus.
