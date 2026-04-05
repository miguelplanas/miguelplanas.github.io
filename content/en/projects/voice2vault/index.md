---
title: "Voice2Vault: Automated Lecture Transcription to Obsidian Study Notes"
tags:
  - python
  - fastapi
  - javascript
  - personal
date: 2026-03-28T12:00:00+01:00
draft: false
math: false
description: "A bring-your-own-keys (BYOK) web application that transforms university lecture audio into structured Obsidian study notes using ASR and LLMs."
summary: "FastAPI + HTMX web app that transcribes lecture audio and generates structured Obsidian study notes via a custom LLM wrapper."
cover:
  image: /projects/voice2vault/capture_interface.png
  alt: "Voice2Vault capture interface showing audio and slide upload zones"
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/voice2vault" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-voice2vault-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <img src="https://img.shields.io/badge/Made_with-Python_3.12-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Built_with-FastAPI-009688?style=for-the-badge&logo=fastapi" style="margin: 0;" />
</div>

---

## Origin

During my Final Year Project (TFG) I gained solid experience in audio processing techniques like Automatic Speech Recognition (ASR) and in working with LLM API providers. Around the same time, I realized something about my own study habits: I use Obsidian for all my notes, and I spent so much time writing things down in class that I was barely paying attention to the professor's explanations.

That contradiction sparked an idea — what if I could record the lectures, let the technology do the heavy lifting, and have structured study notes land directly in my Obsidian vault? Voice2Vault is the answer to that question: a personal project that bridges the gap between sitting in a classroom and having organized, searchable notes ready to review.

## What It Does

Voice2Vault is a web application where you upload lecture audio recordings, and it automatically produces structured study notes formatted for Obsidian. You can also attach the lecture's PDF slides so the generated notes incorporate both what was said and what was shown.

The workflow is simple:

1. **Upload** your lecture audio (MP3, WAV, M4A, etc.) and optionally the professor's slides
2. **Transcribe** the audio using either a local speech-to-text model or a cloud-based one
3. **Generate** structured study notes using an LLM, with prompts designed to produce clean, well-organized material
4. **Save** everything directly into your Obsidian vault, organized by subject and topic, with cross-linked notes

The app also produces a cleaned-up verbatim transcript alongside the structured notes, so you have both the polished study material and a readable version of everything that was said.

![Voice2Vault capture interface](/projects/voice2vault/capture_interface.png)
*Interface with drag-and-drop zones for audio and slides.*

## Key Design Decisions

**Bring your own keys.** The app doesn't store any API keys on a server — you provide your own credentials for the speech-to-text and LLM services you want to use. This keeps everything under your control.

**Multi-provider support.** Rather than locking into a single AI provider, the app supports multiple LLM services through a unified interface. This was partly a practical decision — different models excel at different tasks — and partly a security one. After the LiteLLM supply-chain incident in early 2026, I decided to build a lightweight abstraction directly over the official SDKs rather than depend on a third-party routing library.

**No frontend framework.** The entire UI is built with HTMX and a bit of vanilla JavaScript — no React, no build step, no npm dependencies. The design is intentionally minimal, using warm tones and clean typography to keep the focus on your content.

**Organized output.** Notes are written into your Obsidian vault following a taxonomy you define — by subject, topic, and class type. Each note includes metadata and internal Obsidian links, and an auto-generated index keeps everything navigable.

![Lecture viewer with tabbed interface](/projects/voice2vault/lecture_viewer.png)
*Lecture history.*

## Technical Approach

Under the hood, Voice2Vault uses FastAPI for the backend and a lightweight SQLite database to track processing jobs. Long audio files are split into overlapping segments before transcription to maintain quality without overwhelming memory. The LLM prompts are stored as separate template files, making it easy to iterate on the output format without touching code.

One practical challenge was handling the Obsidian vault path correctly across operating systems — since Obsidian typically runs on Windows and I sometimes develop in WSL, path translation between the two environments required careful handling. The solution was straightforward: run the app natively on the same OS where the vault lives.

## Tech Stack

- **Backend:** FastAPI, Uvicorn, Jinja2
- **Database:** SQLite (aiosqlite)
- **ASR:** faster-whisper (local), Google Gemini ASR (cloud)
- **LLM:** openai, anthropic, google-genai
- **Frontend:** HTMX, vanilla JavaScript, CSS
- **Tools:** PyMuPDF (PDF extraction), ffmpeg/ffprobe (audio processing)
