# EchoBrief — Real-Time Audio Transcription & AI Summaries (McHacks 12)

EchoBrief is a React + Express web app that records live speech, streams transcripts in real time, and generates structured summaries (action items, key points, follow-ups).  
It supports both **live recording** and **uploading pre-recorded files** (.txt / audio).

## ✨ Features
- 🎙️ Start/stop live recording + transcription
- 📝 Real-time transcript streaming to the frontend via **Server-Sent Events (SSE)**
- 📂 Upload support:
  - `.txt` transcripts → summarize directly
  - `.mp3`, `.wav`, `.flac` → transcribe then summarize
- 🧠 AI summarization into **structured Markdown**:
  - ✅ Action Points
  - 📅 Scheduled Dates (YYYY-MM-DD)
  - 🔄 Follow-ups
  - 📝 General Notes

## 🧱 Tech Stack
**Frontend**
- React

**Backend**
- Node.js + Express
- SSE (`/stream`) for real-time updates
- Multer for file uploads (`/upload`)

**AI / Speech**
- Google Cloud Speech-to-Text
  - Streaming transcription for live audio
  - Batch transcription for uploaded audio
- OpenAI Chat Completions for summaries (structured Markdown)

## 🏗️ Architecture Overview
- **Live mode**
  1. Backend starts recording (`GET /start`)
  2. Audio is captured server-side and piped into Google streamingRecognize
  3. Final transcript chunks are broadcast to all connected SSE clients (`GET /stream`)
  4. On stop (`GET /stop`), backend summarizes the full transcript buffer and returns the summary

- **Upload mode**
  1. Upload `.txt` or audio file (`POST /upload`)
  2. If audio: backend transcribes using Google Speech-to-Text `recognize`
  3. Backend summarizes the transcript using OpenAI and returns `{ transcript, summary }`

## ✅ Requirements
- Node.js (18+ recommended)
- A Google Cloud project with Speech-to-Text enabled
- OpenAI API key

## 🔐 Environment Variables
Create a `.env` file in the backend folder:

```env
OPENAI_API_KEY=your_openai_key_here
