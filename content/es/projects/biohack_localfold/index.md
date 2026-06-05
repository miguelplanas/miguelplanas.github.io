---
title: "BioHack LocalFold: Democratizando el Plegamiento de Proteínas"
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
description: "Portal web tipo AlphaFold que conecta a biólogos sin conocimientos de HPC con el superordenador CESGA Finis Terrae III, sumando Google VertexAI (Gemini), Cloud TTS y un sistema multiagente sobre la visualización 3D. Desarrollado para el reto BioHack del Impacthon 2026."
summary: "Portal web tipo AlphaFold que permite a cualquier biólogo plegar una proteína en un superordenador desde una interfaz clara, con explicaciones de Gemini, narración por voz y agentes de IA especializados. Desarrollado para el Impacthon 2026."
cover:
  image: /projects/biohack_localfold/portada.png
  alt: "Portada de LocalFold para el Impacthon 2026 con los logos de CAMELIA, GDG y USC"
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: wrap;">
  <a href="https://github.com/miguelplanas/impacthon2026-MXPNJD" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-impacthon2026-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <img src="https://img.shields.io/badge/Hecho_con-Python_3.14-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Frontend-React_19-61DAFB?style=for-the-badge&logo=react" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Cloud-Google_VertexAI-4285F4?style=for-the-badge&logo=googlecloud" style="margin: 0;" />
</div>

## De qué va esto

LocalFold salió de un fin de semana de hackathon: el **Impacthon 2026** (GDG Santiago de Compostela, en la ETSE/USC), concretamente el **reto BioHack** que patrocinaba la *Cátedra CAMELIA de Medicina Personalizada - Plexus*.

El problema que nos pusieron encima de la mesa es de los que dan rabia precisamente porque es muy real. Modelos como **AlphaFold2** han cambiado la biología para siempre, pero para ejecutarlos de verdad necesitas Linux, GPUs NVIDIA A100, unos 3 TB de bases de datos genéticas y pelearte con colas de SLURM. Un biólogo de a pie no tiene nada de eso, ni tiene por qué saber qué es.

Y lo curioso es que la infraestructura ya está ahí: el superordenador **CESGA Finis Terrae III** está en Santiago, a un paseo. Lo que falta es la última milla, la parte aburrida que nadie quiere construir: una web decente por delante. Eso es LocalFold. Pegas una secuencia FASTA, el job se manda al superordenador (en nuestro caso a una **API mock** que imita su comportamiento bastante fielmente) y al rato te vuelve una estructura 3D que puedes girar, con métricas de confianza, propiedades biológicas, una explicación escrita en cristiano y hasta narrada en voz alta.

Íbamos a por el **premio especial de Google Cloud** al mejor uso de VertexAI, así que la parte de IA no la metimos de adorno: Gemini, Cloud TTS, Firestore y una capa de agentes están metidas de lleno en todo el recorrido.

![Participantes en el podium del Impacthon 2026](/projects/biohack_localfold/premios.jpeg)
*El podium de clausura en la ETSE, bajo los estandartes de CAMELIA, la USC y GDG.*

---

## El ciclo de vida de un job

Queríamos que se notara que detrás hay un clúster de verdad, no una llamada instantánea. El trabajo en HPC es asíncrono: lo encolas y esperas. Así que la interfaz va enseñando cada cambio de estado para que el biólogo nunca se quede mirando una pantalla muerta sin saber qué pasa.

Básicamente hay tres momentos: `PENDING` mientras el job está en cola en el CESGA, `RUNNING` cuando se está prediciendo la estructura (y ahí van saliendo logs simulados de Apptainer/SLURM en directo, que quedan bastante resultones) y `COMPLETED` o `FAILED` al final, según salga la cosa.

Por debajo hay un poller multipestaña (`useMultiJobPoller`) que sigue varias predicciones a la vez, así que puedes lanzar ubiquitina, GFP y p53 de golpe y verlas avanzar cada una por su lado.

![Ciclo de vida del job con logs en directo y explicación](/projects/biohack_localfold/lifecycle.png)
*El badge pasando de PENDING a RUNNING a COMPLETED, los logs estilo SLURM en directo y el panel de Gemini con el chat al lado.*

---

## De FASTA al superordenador

No quisimos obligar a nadie a llegar con la secuencia ya preparada. Puedes pegar un FASTA en bruto, tirar de un catálogo de 22 proteínas conocidas que dejamos preparado (ubiquitina, insulina, GFP, p53, hemoglobina, lisozima y unas cuantas más) o, si no tienes ni idea de qué buscar, describir lo que quieres en lenguaje natural y dejar que un agente identifique la proteína y te recomiende cuántos recursos pedir.

Antes de mandar el job puedes tocar la petición al CESGA: GPUs (0-4), CPUs (1-64), memoria y tiempo máximo. Esos campos no son inventados, replican el contrato real de `POST /jobs/submit` de la API.

![Entrada FASTA y catálogo de proteínas](/projects/biohack_localfold/fasta.png)
*Entrada por FASTA, catálogo o lenguaje natural, con los controles de recursos justo antes de enviar.*

---

## Ver y entender el resultado

Aquí decidimos no reinventar la rueda. Cuando una predicción termina, la pintamos con las mismas herramientas que usa la AlphaFold DB oficial, para que a quien sepa del tema le resulte familiar desde el primer vistazo.

Tienes el visor 3D **Mol\*** con la estructura PDB coloreada residuo a residuo según la confianza **pLDDT** (azul oscuro por encima de 90, azul claro entre 70 y 90, amarillo 50-70 y naranja por debajo de 50 para las zonas más desordenadas). Al lado, un heatmap del **PAE** (*Predicted Aligned Error*) hecho con Plotly, donde los bloques azules en la diagonal te chivan los dominios que se han plegado con confianza. Y luego dos paneles más: uno de confianza con la pLDDT media y su histograma, y otro biológico con solubilidad, índice de inestabilidad, estructura secundaria y avisos de toxicidad.

La idea de fondo era convertir un PDB en bruto en algo sobre lo que un biólogo pueda razonar sin abrir una sola terminal.

![Estructura de p53 en el visor de LocalFold](/projects/biohack_localfold/visor.png)
*La p53 en el visor Mol\*, coloreada por pLDDT, con la confianza, el heatmap PAE y el asistente al lado.*

---

## La parte de IA (Google VertexAI)

Para nosotros lo más interesante era esto: usar la IA generativa para bajar la barrera de interpretación, que es donde de verdad se atasca la gente. Cada número que sale viene acompañado de su significado.

Gemini 2.5 sobre VertexAI (`/api/explain`) coge todas las métricas estructurales y biológicas y las convierte en un texto claro pensado para biólogos, con una plantilla de reserva por si un día no hay credenciales a mano. Ese mismo texto se puede escuchar narrado con una voz neural Chirp3-HD (`/api/tts`), que añadimos sobre todo por accesibilidad. Gemini Vision (`/api/analyze-image`) es capaz de mirar el heatmap PAE o la propia estructura y comentarlos. Y hay un chat contextual (`/api/chat`) que ya sabe qué proteína tienes delante, así que no le tienes que repetir el contexto cada vez.

Por encima de todo eso montamos una **capa A2A (Agent-to-Agent)** con cuatro agentes especializados, cada uno con su Agent Card y su streaming SSE, hablando por el protocolo de agentes de Google:

| Agente | Rol |
|---|---|
| **Mutation Analyst** | Estima el efecto de una mutación puntual sobre la estabilidad y la función antes de ir al laboratorio |
| **Lab Conditions Advisor** | Recomienda sistemas de expresión y estrategias de purificación a partir de propiedades fisicoquímicas |
| **Clinical Relevance Analyst** | Destaca dianas farmacológicas, enfermedades asociadas y aplicaciones biomédicas |
| **Next Steps** | Propone la hoja de ruta experimental de seguimiento |

Y como guinda, un **bot de Telegram** para lanzar plegamientos y mirar el estado de los jobs desde el móvil, sin abrir el portal.

---

## Cómo está montado por dentro

Por debajo es un sistema bastante sencillo de dos capas, y eso fue una decisión consciente: un front en React 19 (TypeScript, Tailwind 4) hablando por HTTP con un backend en FastAPI sobre Python 3.14. El backend se reparte en módulos por responsabilidad: autenticación, la parte de IA (explicaciones, TTS y visión), el agente que parsea FASTA y compara proteínas, la capa A2A de los cuatro agentes, y luego chat, historial, generación de informes, ajustes y Telegram.

Hacia fuera solo habla con dos sitios: Google Cloud (VertexAI Gemini 2.5, Cloud TTS y Firestore) y la API mock del CESGA. Lo hicimos así a propósito para que, el día que se quiera enchufar al clúster de verdad, sea cuestión de cambiar una URL y poco más. Era uno de los criterios del reto: que aquello pudiera llegar a producción algún día, no quedarse en demo.

La autenticación es JWT (hashes bcrypt, tokens de 72h) con un modo de sesión anónima por si quieres trastear sin registrarte, y cada run que termina se guarda en **Firestore**. Así tienes un historial al que volver, que puedes exportar a PDF con ReportLab o poner dos predicciones lado a lado para compararlas.

---

## Stack Tecnológico

- **Frontend:** React 19, TypeScript, Vite, Tailwind CSS 4, Mol\* (3D), Plotly.js (heatmap PAE)
- **Backend:** FastAPI, Python 3.14, Uvicorn
- **IA / Cloud:** Google GenAI SDK (VertexAI Gemini 2.5), Google Cloud Text-to-Speech, protocolo de agentes A2A
- **Datos / Auth:** Google Cloud Firestore, JWT (python-jose), bcrypt (passlib)
- **Reporting:** ReportLab (PDF), Telegram Bot API

---

**Enlace al código:** [impacthon2026-MXPNJD en GitHub](https://github.com/miguelplanas/impacthon2026-MXPNJD)
