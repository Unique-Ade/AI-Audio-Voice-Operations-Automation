# 🎙️ AI-Powered Media Transcription & Job Tracking Pipeline (n8n)

## 📌 Project Overview

This project is an end-to-end asynchronous media processing pipeline built with **n8n**, **OpenAI (Whisper)**, **Supabase**, and advanced webhook routing.

It simulates a high-traffic production environment where large media files are ingested, validated, and transcribed. The system provides immediate feedback to the client while handling long-running AI processing tasks in the background.

---

## 🎯 Business Problem

Processing large media files for transcription often leads to:

- Hanging API connections  
- Server timeouts  
- Lost files if a process crashes  
- No tracking visibility while AI processing runs  

Without a structured asynchronous pipeline, users are left waiting without a tracking ID while the AI processes their request.

---

## ✅ Solution

This automation workflow implements a **non-blocking asynchronous architecture**:

- **Gateway Validation** – Filters inbound requests based on payload size (25MB limit)
- **Immediate Acknowledgement** – Returns `202 Accepted` with a unique `job_id` before heavy processing begins
- **Persistent Logging** – Stores job metadata in Supabase to track status from `pending` to `completed`
- **AI Processing** – Uses OpenAI Whisper for high-accuracy speech-to-text conversion
- **Final Persistence** – Updates the database record with the completed transcript
- **Error Resilience** – Handles oversized files using proper `413 Payload Too Large` responses

---

## 🛠️ Tech Stack

- **n8n** – Workflow orchestration and logic branching  
- **OpenAI API (Whisper)** – Neural speech-to-text processing  
- **Supabase** – PostgreSQL database for job state persistence  
- **HTTP / REST** – Custom webhook responses and binary data handling  
- **Postman** – API stress testing and payload simulation  
- **GitHub** – Version control and documentation  

---

## 🔄 Workflow Architecture

The system is divided into four architectural tiers:

### 1️⃣ The Gateway (Validation)
Authenticates and filters incoming data.

### 2️⃣ The Intake (Logging)
Records the job and issues a receipt (`job_id`) to the user.

### 3️⃣ The Engine (Processing)
Handles resource-heavy AI transcription.

### 4️⃣ The Persistence (Completion)
Finalizes the data lifecycle in the database.

---

## 🤖 AI Transcription Logic

The **Engine** layer uses the OpenAI Whisper model.

Because AI transcription can take seconds or minutes, the workflow implements a **parallel fork pattern**:

- The user receives a response in **< 200ms**
- AI processing continues in the background
- No connection timeouts occur

This ensures scalability and a production-ready user experience.

---

## 🗄️ Data Storage (Supabase)

Each transaction follows a strict schema for auditability:

- `id` – Unique UUID for job tracking  
- `file_name` – Original file metadata  
- `status` – Transitions from `processing` to `completed`  
- `transcript` – Final AI-generated text output  

---

## ⚠️ Error Handling & API Standards

This project follows RESTful API standards:

- **413 Request Entity Too Large** – Triggered if the file exceeds the 25MB Whisper limit  
- **202 Accepted** – Used for asynchronous processing acknowledgment  
- **200 OK** – Used for successful final responses  
- **JSON Standard** – All responses include `Content-Type: application/json` headers  

---

## 🚀 What This Project Demonstrates

- **System Thinking** – Transitioning from simple scripts to multi-tier architecture  
- **Async Processing** – Handling long-running tasks without blocking the UI  
- **Data Integrity** – Logging state before processing to prevent job loss  
- **Professional Documentation** – Clear separation of concerns and naming conventions  

---

## 📚 What I Learned

Building this automation provided hands-on experience with production-grade workflow design:

- **Advanced Webhook Management** – Differentiating between automatic and response-node modes to control client-facing output  
- **Parallel Execution (Forking)** – Using Merge nodes and parallel paths to run background tasks while closing HTTP requests  
- **API Status Codes** – Applying correct HTTP codes (`413`, `202`, `200`) to build predictable APIs  
- **Payload Handling** – Managing binary data buffers to move files from webhook to cloud AI engines efficiently  

---

## 👤 Author

**Adeola Olagbenro**  
Automation Engineer / Workflow Developer
