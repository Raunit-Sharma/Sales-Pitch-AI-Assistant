# Sales Pitch AI Assistant

A production-ready AI-powered web application for generating structured sales pitches and handling customer Q&A using FastAPI, LangChain, and Groq API.

**Version:** 2.0 | **Status:** ✅ Production Ready  
**Demo:** http://localhost:3000 (Frontend) | http://localhost:8000 (Backend API)

---

## Table of Contents

1. [Features](#features)
2. [Getting Started](#getting-started)
3. [API Documentation](#api-documentation)
4. [Project Structure](#project-structure)
5. [Enhanced Capabilities](#enhanced-capabilities)
6. [Configuration](#configuration)
7. [Development & Deployment](#development--deployment)
8. [Troubleshooting](#troubleshooting)

---

## Features

### 1. **Advanced Pitch Generation** (`POST /api/v1/generate-pitch`)

Generate compelling sales pitches with:
- **6 Formats**: Problem-Solution, Story-Based, Question-Led, Data-Driven, Bold Statement, Consultative
- **9 Tones**: Confident, Enthusiastic, Professional, Friendly, Empathetic, Authoritative, Urgent, Casual, Aggressive
- **4 Audiences**: Investor, Customer, B2B, Partner (with deep customization)
- **3 Durations**: 30s (70-85 words), 60s (140-160 words), 120s (280-320 words)
- **3 Languages**: English, Hindi, Hinglish (Hindi-English mix for Indian market)

### 2. **Q&A Chat** (`POST /api/v1/chat`)
Answer customer questions like a top-tier salesperson—concisely and persuasively.

### 3. **Self-Assessment System**
- `POST /api/v1/pitch-feedback` - Track accepted/rejected pitches
- `GET /api/v1/pitch-assessment` - Monitor acceptance rates and improvement

### 4. **Health & API Docs**
- `GET /health` - API availability check
- `GET /docs` - Interactive Swagger documentation

---

## Getting Started

### Prerequisites
- Python 3.8+
- Node.js 16+ (for frontend)
- Groq API Key from https://console.groq.com

### 1. Setup Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Environment

Create `.env` in project root:
```env
GROQ_API_KEY=your_api_key_here
LLM_MODEL=llama-3.3-70b-versatile
LLM_TEMPERATURE=0.7
LLM_MAX_TOKENS=2048
DEBUG=False
```

### 4. Run Application

**Terminal 1 - Backend (Port 8000):**
```bash
python app.py
```

**Terminal 2 - Frontend (Port 3000):**
```bash
cd frontend
npm install  # first time only
npm run dev
```

### 5. Access Application

- **Frontend:** http://localhost:3000
- **API Docs:** http://localhost:8000/docs
- **API ReDoc:** http://localhost:8000/redoc
- **Health Check:** http://localhost:8000/health

---

## API Documentation

### Base URL: `http://localhost:8000/api/v1`

#### Generate Pitch
```
POST /generate-pitch
Content-Type: application/json

{
  "product": "string (1-500 chars)",
  "audience": "investor | customer | b2b | partner",
  "time_limit": "30s | 60s | 120s",
  "tone": "confident | enthusiastic | professional | friendly | empathetic | authoritative | urgent | casual | aggressive",
  "language": "english | hinglish"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "pitch": {
    "script": "Generated pitch text..."
  },
  "message": "Pitch generated successfully"
}
```

#### Regenerate Pitch
```
POST /regenerate-pitch
```
Same parameters as Generate Pitch

#### Chat/Q&A
```
POST /chat
{
  "question": "string (1-1000 chars)",
  "context": "string (optional, max 2000 chars)"
}
```

#### Pitch Feedback
```
POST /pitch-feedback
{
  "accepted": true | false
}
```

#### Assessment Statistics
```
GET /pitch-assessment

Response:
{
  "total_pitches": 10,
  "accepted_pitches": 7,
  "rejected_pitches": 3,
  "acceptance_rate": 70.0
}
```

#### Health Check
```
GET /health
```

---

## Project Structure

```
Sales AI Assistant/
├── app.py                      # Single consolidated backend
├── requirements.txt            # Python dependencies
├── .env                        # Environment config
├── run.bat / run.sh           # Quick start scripts
└── frontend/                   # React frontend
    ├── package.json
    ├── vite.config.js
    ├── src/
    │   ├── App.jsx
    │   ├── index.jsx
    │   └── components/
    │       ├── PitchGenerator.jsx
    │       └── ChatAssistant.jsx
    └── index.html
```

---

## Enhanced Capabilities

### Multiple Pitch Formats (v2.0)

Every generation picks a random format for variety:

1. **Problem-Solution** - Critical problems → solution → transformation
2. **Story-Based** - Relatable narrative with natural product integration  
3. **Question-Led** - Thought-provoking questions leading to solution
4. **Data-Driven** - Statistics and facts establishing credibility
5. **Bold Statement** - Attention-grabbing claim backed by evidence
6. **Consultative** - Trusted advisor approach with insights

### Audience Customization

| Audience | Focus |
|----------|-------|
| **Investor** | Market size, competitive moat, scalability, team, ROI timeline, traction |
| **Customer** | Pain points, ease of use, price/value, support, social proof, quick wins |
| **B2B** | Integration, enterprise security, scalability, TCO, SLA, implementation timeline |
| **Partner** | Mutual value, shared goals, revenue sharing, brand alignment, collaboration |

### Language Support

- **English**: Professional business English
- **Hinglish**: Natural Hindi-English mix (Roman script) for Indian market

Example: *"Yeh product bahut innovative hai aur aapke business ko grow karne mein help karega..."*

### Self-Assessment System

Track pitch performance with real-time statistics:
- Total pitches generated
- Acceptance vs rejection rates
- Acceptance rate percentage
- Data-driven optimization insights

---

## Configuration

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `GROQ_API_KEY` | (required) | Your Groq API key |
| `LLM_MODEL` | llama-3.3-70b-versatile | LLM to use |
| `LLM_TEMPERATURE` | 0.7 | Creativity (0.0-1.0) |
| `LLM_MAX_TOKENS` | 2048 | Max response length |
| `DEBUG` | False | Debug logging |

### Temperature Guide

- **0.0-0.3**: Deterministic, consistent (Q&A)
- **0.4-0.7**: Balanced creativity (default)
- **0.8-1.0**: High creativity, varied

### LLM Models

- **llama-3.3-70b-versatile** (default) - Best quality & speed balance
- **llama-3.1-8b-instant** - Fastest inference
- **mixtral-8x7b-32768** - Highly capable alternative

---

## Development & Deployment

### Add New Features

1. Create Pydantic model for validation
2. Implement endpoint in `app.py`
3. Test via `/docs`
4. Update frontend components

### Testing Endpoints

```bash
# Test pitch generation
curl -X POST "http://localhost:8000/api/v1/generate-pitch" \
  -H "Content-Type: application/json" \
  -d '{
    "product": "AI invoice automation",
    "audience": "investor",
    "time_limit": "60s",
    "tone": "confident"
  }'

# Check health
curl "http://localhost:8000/health"

# Get stats
curl "http://localhost:8000/api/v1/pitch-assessment"
```

### Production Deployment

#### Using Gunicorn
```bash
pip install gunicorn
gunicorn app:app --workers 4 --worker-class uvicorn.workers.UvicornWorker \
  --bind 0.0.0.0:8000
```

#### Docker
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build & Run:
```bash
docker build -t sales-ai .
docker run -e GROQ_API_KEY=your_key -p 8000:8000 sales-ai
```

#### Cloud Options
- **Heroku**: `git push heroku main`
- **AWS Lambda**: Serverless with Zappa
- **Google Cloud Run**: Containerized
- **Azure App Service**: Managed service

---

## Troubleshooting

### Backend Won't Start
```bash
# Check Python version (requires 3.8+)
python --version

# Verify venv is activated (should see (venv) in prompt)

# Reinstall dependencies
pip install --upgrade -r requirements.txt
```

### GROQ_API_KEY Error
```bash
# Verify .env exists in project root
# Check key is correct: https://console.groq.com/keys
# Windows: echo %GROQ_API_KEY%
# Mac/Linux: echo $GROQ_API_KEY
```

### Slow Responses
```bash
# Try faster model: llama-3.1-8b-instant
# Check Groq API status
# Verify internet connection
```

### API Returns 500 Error
- Check backend logs for detailed errors
- Verify GROQ_API_KEY is valid
- Ensure LLM model is available

### Frontend Build Issues
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
npm run dev
```

### API Calls Failing
- Verify backend running: `curl http://localhost:8000/health`
- Check browser console for CORS errors
- Verify API endpoint URLs in frontend code

### Port Already in Use
```bash
# Backend (8000):
uvicorn app:app --port 9000

# Frontend (3000):
cd frontend && npm run dev -- --port 5173
```

---

## What's New in v2.0

| Feature | Before | After | Impact |
|---------|--------|-------|--------|
| Pitch Formats | 1 (repetitive) | 6 (random variety) | 600% ✨ |
| Tone Options | 3 limited | 9 diverse | 300% 🎭 |
| Languages | English only | English + Hinglish | 100% 🇮🇳 |
| Length Scaling | Generic | Proportional (30s:60s:120s = 1:2:4) | 📏 Accurate |
| Quality Tracking | ❌ None | ✅ Self-assessment | 📊 Measurable |
| Audience Context | Vague | Deep customization | 🎯 Precise |

---

## Technology Stack

- **Backend**: FastAPI, LangChain, Groq API, Pydantic v2
- **Frontend**: React 18, Vite, Tailwind CSS, Axios
- **LLM Models**: llama-3.3-70b-versatile, llama-3.1-8b-instant, mixtral-8x7b-32768

---

## Backend Readiness Status

✅ **Fully Production-Ready**

Complete API implementation with:
- Pitch generation, Q&A, feedback tracking, statistics
- Input validation, error handling, CORS enabled
- Environment variables, flexible model selection
- Async/await for high concurrency
- Smart token allocation and prompt engineering
- Ready for Docker, AWS, GCP, Azure, serverless functions

---

## Roadmap

- [ ] Response caching (Redis)
- [ ] Rate limiting per client
- [ ] User authentication & API keys
- [ ] Pitch history database
- [ ] Advanced analytics & reporting
- [ ] Batch pitch generation
- [ ] Custom model fine-tuning
- [ ] Team collaboration features
- [ ] Pitch templates library

---

## Support

For issues:
- Review **Troubleshooting** above
- Check **API Docs** at http://localhost:8000/docs
- Review backend logs
- Inspect browser console for frontend errors

---

## License

Available for personal and commercial use.

---

**Built with ❤️ using FastAPI, LangChain, React, and Groq API**  
**Version:** 2.0 (Production Ready) | **Last Updated:** February 23, 2026

