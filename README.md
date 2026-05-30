# MeetingMind — Whisper Speech-To-Text Transcription System

Audio transcription system built with OpenAI Whisper API. Implements chunking for long recordings, timestamp extraction, and comparison between prompted and unprompted transcription approaches — applied to a real business scenario: transcribing meeting recordings for a client.

---

## Business Context

Companies record hours of meetings that sit untouched because manual transcription is too slow. This lab builds a system that:

- Transcribes audio files accurately using OpenAI Whisper
- Handles long recordings (2+ hours) via audio chunking
- Extracts timestamps for navigation and searchability
- Uses prompts to improve accuracy for technical content
- Exports in multiple formats (text, JSON, SRT subtitles)

---

## Repository Structure

```
Ironhack_Day5/
├── audio/
│   └── chunks/                          # Audio chunks from long recordings
├── recordings/                          # Source audio files
├── output/                              # Transcription outputs (text, JSON, SRT)
├── stt.ipynb                            # Starting notebook — basic STT setup
├── Whisper_STT_Implementation.ipynb     # Full implementation: chunking, timestamps, export
├── speech_recognition_breaking_points.ipynb  # Analysis of chunking edge cases
├── transformer_audio.ipynb              # Transformer-based audio processing exploration
├── tts.ipynb                            # Text-to-speech (TTS) experiments
├── Whisper_STT_Report.pdf               # Lab report: findings and recommendations
├── Arthur.mp3                           # Sample audio file 1
├── CA138clip.mp3                        # Sample audio file 2
└── .gitignore
```

---

## What Was Implemented

**Step 1 — Environment setup**
OpenAI client initialisation, directory structure, ffmpeg for audio processing via pydub.

**Step 2 — Basic transcription**
Short audio file transcription to understand Whisper API response format.

**Step 3 — Prompted transcription (guided)**
Prompts with context (technical terms, names) passed to Whisper to improve accuracy for domain-specific content.

**Step 4 — Unprompted transcription (unguided)**
Same audio transcribed without prompts — results compared against guided approach to measure accuracy delta.

**Step 5 — Audio chunking**
Long recordings split into manageable segments (10-minute chunks). Each chunk processed separately with offset-adjusted timestamps then recombined into a single transcript.

**Step 6 — Timestamp extraction**
Segment-level timestamps extracted and adjusted per chunk offset for accurate navigation across the full recording.

**Step 7 — Multi-format export**
Transcriptions exported in three formats:
- `.txt` — human-readable with timestamps
- `.json` — structured for downstream processing
- `.srt` — subtitle format for video/accessibility use

---

## Key Findings

- **Prompted vs unprompted:** Prompts meaningfully improve accuracy for technical terms, names, and domain-specific vocabulary. For general speech the difference is smaller.
- **Chunking:** Essential for files over Whisper's 25MB API limit. Timestamp offset adjustment is the critical implementation detail — without it, all chunk timestamps restart from zero.
- **Breaking points:** Chunking at fixed time intervals can split mid-sentence. Silence detection or semantic boundaries improve chunk quality.

Full findings and recommendations in `Whisper_STT_Report.pdf`.

---

## Setup

**Requirements:** Python 3.10+, OpenAI API key, ffmpeg

```bash
pip install openai pydub
```

**ffmpeg installation:**
```bash
# Mac
brew install ffmpeg

# Linux
sudo apt-get install ffmpeg

# Windows
choco install ffmpeg
```

**API key:**
```bash
export OPENAI_API_KEY="your_api_key_here"
# or add to .env file
```

---

## Usage

Open notebooks in order:

1. `stt.ipynb` — basic transcription setup
2. `Whisper_STT_Implementation.ipynb` — full implementation with chunking and export
3. `speech_recognition_breaking_points.ipynb` — edge case analysis
4. `transformer_audio.ipynb` — alternative transformer-based approach

Audio files go in `recordings/`. Outputs are saved to `output/`.

---

## Troubleshooting

| Issue | Solution |
|---|---|
| `File size too large` | Use chunking — Whisper API limit is 25MB |
| `ffmpeg not found` | Install ffmpeg and add to system PATH |
| `API rate limit exceeded` | Add delays between chunk API calls |
| `Timestamps incorrect after chunking` | Verify chunk offset is added to each segment timestamp |
| `Poor transcription quality` | Use prompts with domain context; check audio quality and format |

---

## Built With

- [OpenAI Whisper API](https://platform.openai.com/docs/guides/speech-to-text) — speech-to-text transcription
- [pydub](https://github.com/jiaaro/pydub) — audio chunking and processing
- [ffmpeg](https://ffmpeg.org) — audio format handling
- Python 3.10+
