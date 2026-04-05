---
title: "Voice2Vault: Transcripción Automática de Clases a Apuntes de Obsidian"
tags:
  - python
  - fastapi
  - javascript
  - personal
date: 2026-03-28T12:00:00+01:00
draft: false
math: false
description: "Una aplicación web del tipo bring-your-own-keys (BYOK) que transforma el audio de clases universitarias en apuntes estructurados para Obsidian usando ASR y LLMs."
summary: "Aplicación web con FastAPI + HTMX que transcribe audio de clases y genera apuntes estructurados para Obsidian mediante un wrapper LLM personalizado."
cover:
  image: /projects/voice2vault/capture_interface.png
  alt: "Interfaz de captura de Voice2Vault mostrando las zonas de subida de audio y diapositivas"
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/voice2vault" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-voice2vault-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <img src="https://img.shields.io/badge/Hecho_con-Python_3.12-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Construido_con-FastAPI-009688?style=for-the-badge&logo=fastapi" style="margin: 0;" />
</div>

---

## Origen

Durante mi Trabajo de Fin de Grado (TFG) adquirí conocimientos sólidos en técnicas de procesamiento de audio como el Reconocimiento Automático de Voz (ASR) y en el uso de proveedores de APIs de LLM. Al mismo tiempo, caí en la cuenta de algo sobre mis propios hábitos de estudio: uso Obsidian para todos mis apuntes, y pasaba tanto tiempo tomando notas en clase que apenas prestaba atención a las explicaciones del profesor.

Esa contradicción me dio una idea — ¿y si pudiera grabar las clases, dejar que la tecnología hiciera el trabajo pesado, y tener apuntes estructurados directamente en mi vault de Obsidian? Voice2Vault es la respuesta a esa pregunta: un proyecto personal que cierra la brecha entre estar sentado en un aula y tener apuntes organizados y consultables listos para repasar.

## Qué Hace

Voice2Vault es una aplicación web donde subes grabaciones de audio de tus clases y genera automáticamente apuntes de estudio estructurados y formateados para Obsidian. También puedes adjuntar las diapositivas PDF de la clase para que los apuntes generados incorporen tanto lo que se dijo como lo que se mostró.

El flujo es sencillo:

1. **Sube** el audio de tu clase (MP3, WAV, M4A, etc.) y opcionalmente las diapositivas del profesor
2. **Transcribe** el audio usando un modelo local de voz a texto o uno basado en la nube
3. **Genera** apuntes estructurados usando un LLM, con prompts diseñados para producir material limpio y bien organizado
4. **Guarda** todo directamente en tu vault de Obsidian, organizado por asignatura y tema, con notas enlazadas entre sí

La aplicación también produce una transcripción literal corregida junto a los apuntes estructurados, de modo que tienes tanto el material de estudio pulido como una versión legible de todo lo que se dijo.

![Interfaz de captura de Voice2Vault](/projects/voice2vault/capture_interface.png)
*Interfaz con zonas de arrastrar y soltar para audio y diapositivas.*

## Decisiones Clave de Diseño

**Trae tus propias claves.** La aplicación no almacena ninguna clave API en un servidor — tú proporcionas tus propias credenciales para los servicios de voz a texto y LLM que quieras usar. Esto mantiene todo bajo tu control.

**Soporte multi-proveedor.** En lugar de depender de un único proveedor de IA, la aplicación soporta múltiples servicios de LLM a través de una interfaz unificada. Esta fue una decisión práctica — diferentes modelos destacan en distintas tareas — pero también de seguridad. Tras el incidente de la cadena de suministro de LiteLLM a principios de 2026, decidí construir una abstracción ligera directamente sobre los SDKs oficiales en lugar de depender de una biblioteca de enrutamiento de terceros.

**Sin framework de frontend.** Toda la interfaz está construida con HTMX y un poco de JavaScript vanilla — sin React, sin paso de compilación, sin dependencias de npm. El diseño es minimalista a propósito, usando tonos cálidos y tipografía limpia para mantener el foco en tu contenido.

**Salida organizada.** Los apuntes se escriben en tu vault de Obsidian siguiendo una taxonomía que tú defines — por asignatura, tema y tipo de clase. Cada nota incluye metadatos y enlaces internos de Obsidian, y un índice auto-generado mantiene todo navegable.

![Visor de clases con interfaz de pestañas](/projects/voice2vault/lecture_viewer.png)
*Historial de clases.*

## Enfoque Técnico

Por debajo, Voice2Vault usa FastAPI para el backend y una base de datos SQLite ligera para rastrear los trabajos de procesamiento. Los archivos de audio largos se dividen en segmentos solapados antes de la transcripción para mantener la calidad sin sobrecargar la memoria. Los prompts del LLM se almacenan como archivos de plantilla separados, lo que facilita iterar sobre el formato de salida sin tocar código.

Un desafío práctico fue manejar correctamente la ruta del vault de Obsidian entre sistemas operativos — dado que Obsidian normalmente se ejecuta en Windows y a veces desarrollo en WSL, la traducción de rutas entre los dos entornos requirió un manejo cuidadoso. La solución fue directa: ejecutar la aplicación de forma nativa en el mismo sistema operativo donde reside el vault.

## Stack Tecnológico

- **Backend:** FastAPI, Uvicorn, Jinja2
- **Base de datos:** SQLite (aiosqlite)
- **ASR:** faster-whisper (local), Google Gemini ASR (nube)
- **LLM:** openai, anthropic, google-genai
- **Frontend:** HTMX, JavaScript vanilla, CSS
- **Herramientas:** PyMuPDF (extracción de PDF), ffmpeg/ffprobe (procesamiento de audio)
