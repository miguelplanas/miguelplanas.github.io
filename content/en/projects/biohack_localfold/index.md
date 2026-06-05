---
title: "BioHack LocalFold: Democratizing Protein Folding"
tags:
  - python
  - fastapi
  - javascript
  - deep-learning
  - personal
  - awards
date: 2026-04-12T12:00:00+01:00
draft: false
math: false
description: "An AlphaFold-like web portal that connects biologists with no HPC background to the CESGA Finis Terrae III supercomputer, layering Google VertexAI (Gemini), Cloud TTS and a multi-agent system on top of 3D structure visualization. Built for the Impacthon 2026 BioHack challenge."
summary: "An AlphaFold-like web portal that lets any biologist fold a protein on a supercomputer through a clean UI, with Gemini explanations, voice narration and specialized AI agents. Built for Impacthon 2026."
cover:
  image: /projects/biohack_localfold/portada.png
  alt: "LocalFold title card for Impacthon 2026 with CAMELIA, GDG and USC logos"
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: wrap;">
  <a href="https://github.com/miguelplanas/impacthon2026-MXPNJD" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-impacthon2026-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <img src="https://img.shields.io/badge/Made_with-Python_3.14-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Frontend-React_19-61DAFB?style=for-the-badge&logo=react" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Cloud-Google_VertexAI-4285F4?style=for-the-badge&logo=googlecloud" style="margin: 0;" />
</div>

## What this is about

LocalFold came out of a hackathon weekend: **Impacthon 2026** (GDG Santiago de Compostela, at the ETSE/USC), specifically the **BioHack challenge** sponsored by the *CAMELIA Personalized Medicine Chair - Plexus*.

The problem they put in front of us is one of those that bug you precisely because it's so real. Models like **AlphaFold2** have changed biology for good, but to actually run them you need Linux, NVIDIA A100 GPUs, around 3 TB of genetic databases and a willingness to fight with SLURM queues. Your average biologist has none of that, and frankly shouldn't have to know what any of it is.

The funny part is that the infrastructure is already there: the **CESGA Finis Terrae III** supercomputer sits in Santiago, a short walk away. What's missing is the last mile, the unglamorous bit nobody wants to build: a decent web frontend. That's LocalFold. You paste a FASTA sequence, the job goes off to the supercomputer (in our case to a **mock API** that imitates its behavior fairly faithfully), and a while later you get back a 3D structure you can spin around, with confidence metrics, biological properties, an explanation written in plain language and even narrated out loud.

We were going for the **Google Cloud special prize** for the best use of VertexAI, so the AI side wasn't there for decoration: Gemini, Cloud TTS, Firestore and an agent layer run through the whole thing.

![Participants on the podium at Impacthon 2026](/projects/biohack_localfold/premios.jpeg)
*The closing podium at the ETSE, under the CAMELIA, USC and GDG banners.*

---

## The lifecycle of a job

We wanted it to feel like there's a real cluster behind it, not an instant call. HPC work is asynchronous: you queue it and you wait. So the UI keeps showing every state change, so the biologist is never left staring at a dead screen wondering what's going on.

There are basically three moments: `PENDING` while the job sits in the queue at CESGA, `RUNNING` while the structure is being predicted (with simulated Apptainer/SLURM logs streaming live, which honestly look pretty cool), and `COMPLETED` or `FAILED` at the end, depending on how it goes.

Underneath there's a multi-tab poller (`useMultiJobPoller`) following several predictions at once, so you can fire off ubiquitin, GFP and p53 together and watch each one move on its own.

![Job lifecycle with live logs and explanation](/projects/biohack_localfold/lifecycle.png)
*The badge moving from PENDING to RUNNING to COMPLETED, the live SLURM-style logs and the Gemini panel with the chat next to it.*

---

## From FASTA to supercomputer

We didn't want to force anyone to show up with their sequence already prepared. You can paste a raw FASTA, reach for a catalogue of 22 well-known proteins we left ready (ubiquitin, insulin, GFP, p53, hemoglobin, lysozyme and a few more), or, if you have no idea what to look for, describe what you want in plain language and let an agent identify the protein and suggest how many resources to ask for.

Before sending the job you can tweak the request to CESGA: GPUs (0-4), CPUs (1-64), memory and max runtime. Those fields aren't made up, they mirror the real `POST /jobs/submit` contract of the API.

![FASTA input and protein catalogue](/projects/biohack_localfold/fasta.png)
*Input by FASTA, catalogue or natural language, with the resource controls right before submitting.*

---

## Seeing and understanding the result

Here we decided not to reinvent the wheel. When a prediction finishes, we draw it with the same tools the official AlphaFold DB uses, so anyone who knows the field feels at home from the first glance.

You get the **Mol\*** 3D viewer with the PDB structure colored residue by residue according to **pLDDT** confidence (dark blue above 90, light blue between 70 and 90, yellow 50-70, and orange below 50 for the more disordered regions). Next to it, a **PAE** heatmap (*Predicted Aligned Error*) built with Plotly, where the blue diagonal blocks tip you off to the domains that folded confidently. And then two more panels: a confidence one with the mean pLDDT and its histogram, and a biological one with solubility, instability index, secondary structure and toxicity warnings.

The whole point was to turn a raw PDB file into something a biologist can actually reason about without opening a single terminal.

![p53 structure in the LocalFold viewer](/projects/biohack_localfold/visor.png)
*p53 in the Mol\* viewer, colored by pLDDT, with confidence, the PAE heatmap and the assistant alongside.*

---

## The AI side (Google VertexAI)

This was the part we found most interesting: using generative AI to lower the interpretation barrier, which is where people actually get stuck. Every number that comes out comes with its meaning attached.

Gemini 2.5 on VertexAI (`/api/explain`) takes all the structural and biological metrics and turns them into clear text aimed at biologists, with a fallback template in case there are no credentials around some day. That same text can be heard narrated with a neural Chirp3-HD voice (`/api/tts`), which we added mostly for accessibility. Gemini Vision (`/api/analyze-image`) can look at the PAE heatmap or the structure itself and comment on them. And there's a contextual chat (`/api/chat`) that already knows which protein you have in front of you, so you don't have to repeat the context every time.

On top of all that we built an **A2A (Agent-to-Agent) layer** with four specialized agents, each with its own Agent Card and SSE streaming, talking over Google's agent protocol:

| Agent | Role |
|---|---|
| **Mutation Analyst** | Estimates the effect of a point mutation on stability and function before you go to the lab |
| **Lab Conditions Advisor** | Recommends expression systems and purification strategies from physicochemical properties |
| **Clinical Relevance Analyst** | Surfaces drug targets, associated diseases and biomedical applications |
| **Next Steps** | Proposes the follow-up experimental roadmap |

And as the cherry on top, a **Telegram bot** to trigger folds and check job status from your phone, without opening the portal.

---

## How it's put together inside

Underneath it's a fairly simple two-tier system, and that was a deliberate call: a React 19 front (TypeScript, Tailwind 4) talking over HTTP to a FastAPI backend on Python 3.14. The backend is split into modules by responsibility: authentication, the AI side (explanations, TTS and vision), the agent that parses FASTA and compares proteins, the A2A layer with the four agents, and then chat, history, report generation, settings and Telegram.

Outward, it only talks to two places: Google Cloud (VertexAI Gemini 2.5, Cloud TTS and Firestore) and the CESGA mock API. We did it that way on purpose so that, the day someone wants to plug it into the real cluster, it's a matter of changing a URL and little else. That was one of the challenge criteria: that the thing could reach production some day, not stay a demo.

Authentication is JWT (bcrypt hashes, 72h tokens) with an anonymous-session mode in case you want to poke around without registering, and every run that finishes is saved to **Firestore**. So you get a history to come back to, which you can export to PDF with ReportLab or put two predictions side by side to compare them.

---

## Tech Stack

- **Frontend:** React 19, TypeScript, Vite, Tailwind CSS 4, Mol\* (3D), Plotly.js (PAE heatmap)
- **Backend:** FastAPI, Python 3.14, Uvicorn
- **AI / Cloud:** Google GenAI SDK (VertexAI Gemini 2.5), Google Cloud Text-to-Speech, A2A agent protocol
- **Data / Auth:** Google Cloud Firestore, JWT (python-jose), bcrypt (passlib)
- **Reporting:** ReportLab (PDF), Telegram Bot API

---

**Link to code:** [impacthon2026-MXPNJD on GitHub](https://github.com/miguelplanas/impacthon2026-MXPNJD)
