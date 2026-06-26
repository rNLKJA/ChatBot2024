<div align="center">

# ChatBot2024 — "Echo" Voice Companion (Fork)

**A fork of [AlbertLiDesign/ChatBot2024](https://github.com/AlbertLiDesign/ChatBot2024) — a PyQt5 desktop voice chatbot ("Echo") with wake-word detection and OpenAI-powered conversation, extended with a Flask API backend.**

[![Upstream](https://img.shields.io/badge/upstream-AlbertLiDesign-1f6feb?logo=github)](https://github.com/AlbertLiDesign/ChatBot2024)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyQt5](https://img.shields.io/badge/GUI-PyQt5-41CD52?logo=qt&logoColor=white)](https://www.riverbankcomputing.com/software/pyqt/)
[![OpenAI](https://img.shields.io/badge/AI-OpenAI-412991?logo=openai&logoColor=white)](https://platform.openai.com/)
[![Wake word](https://img.shields.io/badge/wake--word-Porcupine-FFD000)](https://picovoice.ai/platform/porcupine/)

</div>

> **This is a fork, not original work.** The base desktop application is by **[@AlbertLiDesign](https://github.com/AlbertLiDesign)**. I forked it to learn from it and to add a separate Flask API backend (see [my contribution](#my-contribution)).

## Overview

**Echo** is a desktop voice companion designed as a warm, concise conversational partner with a clinical-psychology framing — a listener for everyday stress, emotions and personal reflection. The flow is fully spoken:

1. **Wake word** — [Porcupine](https://picovoice.ai/platform/porcupine/) listens for _"Hey, Echo"_ (`.ppn` keyword models under `wake-up-word/`) and wakes the app.
2. **Speech → text** — the user's recorded question is transcribed via OpenAI.
3. **Dialogue** — the transcript is sent to OpenAI (GPT-4o) under a role/system prompt that defines Echo's persona and conversational style.
4. **Text → speech** — the reply is synthesised back to audio (OpenAI TTS) and played aloud.

A PyQt5 GUI (`source/gui/`) provides the desktop window; conversation history is logged to `history.txt` / `data/history.json`.

## My contribution

My own work lives on the **[`flaskApi`](../../tree/flaskApi)** branch: a small **Flask API backend** that re-implements the conversation layer as a web service —

- `flask/app/openai_client.py` — OpenAI client, persona dialogue, and personal-info collection.
- `flask/app/db.py` — SQLite (`conversations.db`) persistence for users and conversation history.
- `flask/app/routes.py`, `config.py`, `app.py` — Flask routes and configuration (`OPENAI_API_KEY` read from a `.env`).

```bash
# on the flaskApi branch
conda env create -f flask/environment.yml
conda activate flask
python flask/app.py
```

## Tech Stack

| Layer          | Tools                                       |
| -------------- | ------------------------------------------- |
| Desktop GUI    | PyQt5                                       |
| Wake word      | Picovoice Porcupine (`pvporcupine`)         |
| Speech / audio | `pyaudio`, `SpeechRecognition`              |
| AI             | OpenAI (Whisper transcription, GPT-4o, TTS) |
| Backend (fork) | Flask + SQLite (`flaskApi` branch)          |

## Getting Started (desktop app)

```bash
pip install -r requirements.txt

# API keys are read from environment variables (none are committed):
export OPENAI_API_KEY="sk-..."
export PORCUPINE="<your Picovoice access key>"

python app.py        # PyQt5 GUI
# or
python main.py       # voice-loop entry point
```

## Attribution & licence

Base application © **[@AlbertLiDesign](https://github.com/AlbertLiDesign)**, under the repository's [GPL-3.0 licence](LICENSE). This fork is kept for personal learning and to host my Flask-backend experiment; refer to the [upstream repository](https://github.com/AlbertLiDesign/ChatBot2024) as the source of truth.

<sub>API keys are loaded from environment variables — none are committed. The original README is preserved in <a href="_archive/README.original.md"><code>\_archive/README.original.md</code></a>.</sub>
