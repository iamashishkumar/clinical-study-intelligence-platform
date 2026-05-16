# Clinical-Study-Intelligence-Platform
Built a RAG system over 500+ clinical studies to stop teams  reinventing the wheel. IRT knowledge, finally searchable. LangGraph · DuckDB · FastAPI · Ollama

# IRT Intelligence Platform

Clinical trials generate a lot of institutional knowledge. 
Most of it goes nowhere.

Every study we close has validated configurations, resolved defects, 
effort data, risk patterns — sitting in SPIRA, JIRA, and folders 
that nobody searches systematically. When a new study comes in, 
the team starts fresh. Same mistakes, same research, same wheel.

This platform is an attempt to change that.

---

## What it does

Four things, dashboard-first:

**Executive Portfolio Health**  
Global → Client → Study drill-down in one view. Instant risk 
concentration across the portfolio without pulling five reports.

**New Study Similarity**  
Upload a URS. Get the top-2 closest studies from history. 
Reuse their known risks, mitigations, and configurations 
instead of starting from scratch.

**Risk Linking (Evidence)**  
Requirements ↔ SPIRA ↔ JIRA ↔ Effort — connected, not siloed. 
Component-level risk badges so you know where problems 
are concentrated before they surface.

**Role Dashboards**  
Different views for different people. Suggested studies 
surfaced from JIRA activity, not manual lookup.

ROI: faster study start-up · fewer repeat defects · 
better delivery predictability · reduced reporting overhead

---

## Architecture

Local-first. No study data leaves the environment.

```
Streamlit UI ──► FastAPI (APIs + Background Jobs) ──► DuckDB Master
(Dashboards)                                      ──► DuckDB Shards
                      │                               (per client)
                      ▼
                 Local LLM (Ollama)
```


Design principles that drove these choices:

- **SQL-first, vector-second, LLM-last** — dashboards and KPIs run 
  on DuckDB. Similarity matching uses vectors. LLM is the last layer, 
  only when needed. Keeps it fast and auditable.
- **Non-blocking UI** — background jobs handle ingestion. 
  The dashboard doesn't freeze while processing.
- **Local-only data sovereignty** — Ollama runs on-premise. 
  No clinical data touches an external API. This matters in 
  a regulated environment.

---

## How ingestion works

Upload URS + files
│
▼
Extract client / study metadata
│
▼
Confirm & version
│
▼
Build KB folders  ──►  Ingest SPIRA / JIRA / Effort
│
▼
Compute KPIs
│
▼
Dashboards (drill-down)
│
▼
Similarity — Top-2 match

---

## Where things stand

**Pilot: 20 studies**  
Currently proving ingestion + dashboards + similarity on 20 studies. 
Goal is to confirm KPI usefulness and whether teams actually adopt it 
before scaling.

**Scale: 500+ studies**  
Same pipeline, client shards, federated metrics. 
AI search layer added at scale.

This was presented to the steering committee as a snapshot. 
The pilot is the gate before full rollout.

---

## Stack

FastAPI · DuckDB · Streamlit · Ollama · LangGraph · Python

---

## Why local LLM

The obvious question. External APIs are faster and cheaper. 
But this system processes URS documents and JIRA data from 
live clinical studies. Sending that to a cloud API is not 
an option in our environment. Ollama running locally on the 
server solves this cleanly — same LLM capability, 
zero data exposure.

---

## Folder structure
├── ingestion/        URS parsing, JIRA/SPIRA connectors
├── kb/               Knowledge base folder management
├── similarity/       Study similarity matching
├── kpi/              KPI computation logic
├── api/              FastAPI backend + background jobs
├── frontend/         Streamlit dashboards
├── llm/              Ollama integration layer
└── data/sample/      Synthetic studies for local testing

---

## Background

Built this because I got tired of watching teams spend the first 
two weeks of every study rediscovering what the last team already 
figured out. The knowledge was always there. The access wasn't.

The steering committee presentation was the first checkpoint — 
getting alignment on whether this was worth piloting before 
investing in the full 500-study build.

---

Ashish Kumar 
Bengaluru  


