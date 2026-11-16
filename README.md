<p align="center">
  <img src="https://img.shields.io/badge/MindfulChat-AI%20Mental%20Health%20Companion-8B5CF6?style=for-the-badge&logo=brain&logoColor=white" alt="MindfulChat Badge"/>
</p>

<h1 align="center">🧠 MindfulChat — AI-Powered Mental Health Support Chatbot</h1>

<p align="center">
  <em>An intelligent, full-stack mental health support system combining BERT-based sentiment analysis with Google Gemini AI for empathetic, context-aware conversations.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white"/>
  <img src="https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white"/>
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white"/>
  <img src="https://img.shields.io/badge/BERT-FF6F00?style=flat-square&logo=tensorflow&logoColor=white"/>
  <img src="https://img.shields.io/badge/Gemini%202.0-4285F4?style=flat-square&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white"/>
</p>

---

## 📌 Problem Statement & Objective

### The Problem
Mental health is a growing global crisis. According to the WHO, **1 in every 8 people** lives with a mental health condition, yet **75% of people in low-and-middle-income countries receive no treatment**. Key barriers include:

- **Stigma** — People hesitate to seek help due to social judgment
- **Accessibility** — Limited availability of trained professionals, especially in rural/underserved areas
- **Cost** — Therapy can be prohibitively expensive ($100–$300/session in many countries)
- **Wait times** — Months-long waitlists for professional help
- **24/7 availability** — Mental health crises don't follow business hours

### The Objective
Build an **AI-powered mental health companion** that provides:
1. **Instant, empathetic support** — Available 24/7, no appointment needed
2. **Emotion-aware responses** — Uses NLP to understand the user's emotional state
3. **Evidence-based coping strategies** — Offers breathing exercises, grounding techniques, and journaling prompts
4. **Crisis detection** — Automatically identifies suicidal ideation and provides immediate helpline resources
5. **Conversation memory** — Tracks chat history and sentiment trends over time

> ⚠️ **Disclaimer**: MindfulChat is NOT a replacement for professional therapy. It serves as a supplementary support tool that bridges the gap between recognizing a problem and seeking professional help.

---

## 🏗️ System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT (Browser)                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────────┐   │
│  │Login Page│  │Register  │  │ Chat UI  │  │  Admin Dashboard │   │
│  │          │  │   Page   │  │          │  │                  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────────────┘   │
│       HTML + CSS + Vanilla JavaScript (Fetch API)                  │
└────────────────────────────┬────────────────────────────────────────┘
                             │ HTTP/REST (JSON)
                             ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    NODE.JS EXPRESS SERVER (:3000)                   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                    MIDDLEWARE LAYER                          │   │
│  │  Helmet (Security) → CORS → Rate Limiter → Body Parser     │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────────┐   │
│  │ Auth Routes │  │ Chat Routes  │  │    Admin Routes         │   │
│  │ /api/auth/* │  │ /api/chat/*  │  │    /api/admin/*         │   │
│  └──────┬──────┘  └──────┬───────┘  └───────────┬─────────────┘   │
│         │                │                       │                 │
│  ┌──────▼──────┐  ┌──────▼───────┐  ┌───────────▼─────────────┐   │
│  │    Auth     │  │    Chat      │  │      Admin              │   │
│  │ Controller  │  │ Controller   │  │    Controller            │   │
│  │             │  │              │  │                          │   │
│  │ • Register  │  │ • Message    │  │ • List Users/Chats      │   │
│  │ • Login     │  │   Pipeline   │  │ • Edit/Delete Users     │   │
│  │ • Profile   │  │ • Crisis     │  │ • Dashboard Stats       │   │
│  │ • JWT Issue │  │   Detection  │  │ • Role Management       │   │
│  └─────────────┘  └──────┬───────┘  └─────────────────────────┘   │
│                          │                                         │
│         ┌────────────────┼────────────────┐                        │
│         ▼                ▼                ▼                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐               │
│  │  Sentiment  │  │   Gemini    │  │  MongoDB    │               │
│  │  Service    │  │ Controller  │  │  (Mongoose) │               │
│  │  Client     │  │             │  │             │               │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘               │
└─────────┼────────────────┼────────────────┼──────────────────────┘
          │                │                │
          ▼                ▼                ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────────┐
│ Python Flask │  │ Google       │  │   MongoDB Atlas  │
│ Service      │  │ Gemini 2.0   │  │   (Cloud DB)     │
│ (:5001)      │  │ Flash API    │  │                  │
│              │  │              │  │  • Users         │
│ DistilBERT   │  │ Generative   │  │  • Chats         │
│ Emotion      │  │ AI           │  │  • Messages      │
│ Classifier   │  │              │  │  • Sentiments    │
└──────────────┘  └──────────────┘  └──────────────────┘
```

### Application Layers

| Layer | Technology | Responsibility |
|-------|-----------|----------------|
| **Presentation** | HTML, CSS, Vanilla JS | UI rendering, user interaction, API calls |
| **API Gateway** | Express.js + Middleware | Routing, authentication, rate limiting, CORS |
| **Business Logic** | Controllers | Request processing, crisis detection, response orchestration |
| **AI/ML Layer** | BERT (Python) + Gemini (API) | Sentiment analysis + empathetic response generation |
| **Data Layer** | MongoDB + Mongoose ODM | User data, chat history, sentiment tracking |
| **Security Layer** | JWT, Helmet, bcrypt | Authentication, encryption, input validation |

---

## 🧠 AI/ML Layer — The Intelligence Behind MindfulChat

### 1. Sentiment Analysis — DistilBERT Emotion Classifier

| Attribute | Details |
|-----------|---------|
| **Model** | `bhadresh-savani/distilbert-base-uncased-emotion` |
| **Base Architecture** | DistilBERT (distilled version of BERT-base) |
| **Parameters** | 66 million |
| **Training Data** | Emotion dataset (~416K text samples labeled with 6 emotions) |
| **Source** | [HuggingFace Model Hub](https://huggingface.co/bhadresh-savani/distilbert-base-uncased-emotion) |
| **Input** | Raw text (max 512 tokens) |
| **Output** | Emotion classification with confidence scores |

**How it works:**

```
User Message → Tokenization → DistilBERT (12 layers, 768 hidden) → Classification Head → Emotion Label
                                                                                          + Confidence
```

**Detected Emotions (mapped to mental health context):**

| Model Output | MindfulChat Label | Example Trigger |
|-------------|-------------------|-----------------|
| sadness | 😢 depressed | "I feel hopeless and empty" |
| joy | 😊 happy | "I'm feeling great today!" |
| anger | 😡 angry | "I'm so frustrated with everything" |
| fear | 😰 anxious | "I can't stop worrying about tomorrow" |
| love | 😊 happy | "I'm grateful for my friends" |
| surprise | 😐 neutral | "I wasn't expecting that" |

**Enhanced with keyword-based analysis** for mental health-specific emotions (stressed, lonely) that the base BERT model doesn't cover.

### 2. Response Generation — Google Gemini 2.0 Flash

| Attribute | Details |
|-----------|---------|
| **Model** | `gemini-2.0-flash` |
| **Provider** | Google DeepMind |
| **API** | `@google/generative-ai` npm package |
| **Context Window** | 1 million tokens |
| **Role** | Generate empathetic, context-aware therapeutic responses |

**Prompt Engineering Strategy:**
- System instructions define MindfulChat's personality as a compassionate counselor
- Sentiment label and confidence are injected into the prompt for emotional context
- Last 6 messages of conversation history are included for continuity
- Guidelines enforce: no diagnosing, no prescribing, active listening, evidence-based coping strategies

### 3. Crisis Detection System

A rule-based safety layer that operates **before** any AI processing:

```
User Message → Crisis Keyword Scan → If detected:
                                       → Bypass Gemini
                                       → Return immediate helpline info
                                       → Flag message in database
                                       → Set isCrisis: true
```

**Crisis keywords monitored:** suicide, kill myself, self-harm, end my life, want to die, etc.


---

## 🛠️ Tools & Techniques

### Backend
| Tool | Version | Purpose |
|------|---------|---------|
| Node.js | 22.x | Server runtime |
| Express.js | 4.x | REST API framework |
| Mongoose | 8.x | MongoDB ODM |
| bcryptjs | 2.x | Password hashing (10 salt rounds) |
| jsonwebtoken | 9.x | JWT authentication (7-day expiry) |
| helmet | 8.x | HTTP security headers |
| express-rate-limit | 7.x | API rate limiting (100 req/15 min) |
| morgan | 1.x | HTTP request logging |
| axios | 1.x | HTTP client (sentiment service calls) |
| @google/generative-ai | latest | Gemini API SDK |

### AI/ML Service
| Tool | Purpose |
|------|---------|
| Python 3.12 | ML service runtime |
| Flask + Flask-CORS | REST API for model serving |
| HuggingFace Transformers | Model loading & inference |
| PyTorch | Deep learning backend |
| DistilBERT | Pre-trained emotion classifier |

### Frontend
| Tool | Purpose |
|------|---------|
| HTML5 + CSS3 | Structure & styling |
| Vanilla JavaScript | Client-side logic (no framework overhead) |
| CSS Custom Properties | Dark theme design system |
| Fetch API | Async HTTP requests |

### Infrastructure
| Tool | Purpose |
|------|---------|
| MongoDB Atlas | Cloud-hosted database (free tier) |
| Docker | Containerized deployment |
| Render | Cloud hosting (free tier) |
| Git + GitHub | Version control |

---

## 📁 Project Structure

```
mental_health_chatbot/
├── app.js                          # Express server entry point
├── Dockerfile                      # Docker deployment config
├── render.yaml                     # Render hosting config
├── package.json                    # Node.js dependencies
│
├── config/
│   └── db.js                       # MongoDB Atlas connection
│
├── controllers/
│   ├── authController.js           # Register, login, profile (JWT)
│   ├── chatController.js           # Message pipeline + crisis detection
│   ├── geminiController.js         # Gemini prompt engineering + retries
│   └── adminController.js          # User/chat CRUD + dashboard stats
│
├── middleware/
│   ├── auth.js                     # JWT verification middleware
│   └── admin.js                    # Admin role authorization
│
├── models/
│   ├── User.js                     # User schema (bcrypt pre-save hook)
│   └── Chat.js                     # Chat + messages with sentiment data
│
├── routes/
│   ├── authRoutes.js               # POST /api/auth/register|login
│   ├── chatRoutes.js               # POST /api/chat/message, GET /history
│   └── adminRoutes.js              # GET/PATCH/DELETE /api/admin/*
│
├── public/                         # Static frontend
│   ├── index.html                  # Chat interface
│   ├── login.html                  # Login page
│   ├── register.html               # Registration page
│   ├── admin.html                  # Admin dashboard
│   ├── css/style.css               # Complete design system
│   └── js/
│       ├── app.js                  # Chat logic + sentiment display
│       ├── auth.js                 # Login/register form handling
│       └── admin.js                # Admin panel logic
│
└── sentiment_service/              # Python microservice
    ├── app.py                      # Flask + BERT emotion classifier
    └── requirements.txt            # Python dependencies
```

---

## ⚡ Step-by-Step: How to Use

### Prerequisites
- Node.js 18+ installed
- Python 3.10+ installed
- MongoDB Atlas account (free) — [signup here](https://www.mongodb.com/cloud/atlas/register)
- Google Gemini API key — [get one here](https://aistudio.google.com/apikey)

### 1. Clone the Repository
```bash
git clone https://github.com/MohdAnas1234/VoiceMonitoringSystem
cd VoiceMntrSys
```

### 2. Install Dependencies
```bash
# Node.js dependencies
npm install

# Python dependencies
cd sentiment_service
pip install -r requirements.txt
cd ..
```

### 3. Configure Environment
Create a `.env` file in the root directory:
```env
MONGO_URI=mongodb+srv://your_user:your_password@cluster.mongodb.net/mindfulchat?retryWrites=true&w=majority
JWT_SECRET=your_secret_key_here
GOOGLE_API_KEY=your_gemini_api_key
PORT=3000
SENTIMENT_SERVICE_URL=http://localhost:5001
```

### 4. Start the Services
```bash
# Terminal 1 — Start the sentiment analysis service
cd sentiment_service
python app.py
# ✅ Running on http://localhost:5001

# Terminal 2 — Start the main server
node app.js
# ✅ Running on http://localhost:3000
```

### 5. Use the Application
1. Open **http://localhost:3000** → Register a new account
2. Start chatting — the bot detects your emotions and responds empathetically
3. Observe sentiment badges (😢 depressed, 😰 anxious, etc.) on your messages
4. Admin panel at **http://localhost:3000/admin.html** (requires admin role)

### 6. Docker Deployment
```bash
docker build -t mindfulchat .
docker run -p 3000:3000 --env-file .env mindfulchat
```

---

## 🔌 API Reference

### Authentication
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|:---:|
| POST | `/api/auth/register` | Register new user | ❌ |
| POST | `/api/auth/login` | Login & get JWT | ❌ |
| GET | `/api/auth/profile` | Get user profile | ✅ |

### Chat
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|:---:|
| POST | `/api/chat/message` | Send message & get AI response | ✅ |
| GET | `/api/chat/history` | Get all chat sessions | ✅ |

### Admin
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|:---:|
| GET | `/api/admin/users` | List all users | ✅ Admin |
| PATCH | `/api/admin/user/:id` | Update user role | ✅ Admin |
| DELETE | `/api/admin/user/:id` | Delete user | ✅ Admin |
| GET | `/api/admin/chats` | View all chats | ✅ Admin |
| GET | `/api/admin/dashboard` | Platform statistics | ✅ Admin |

### Sentiment Service
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/predict` | Analyze text sentiment |
| GET | `/health` | Service health check |

---

## 🔒 Security Features

- **Password hashing** — bcrypt with 10 salt rounds
- **JWT authentication** — 7-day token expiry, Bearer scheme
- **Helmet** — Sets secure HTTP headers (XSS, clickjacking protection)
- **Rate limiting** — 100 requests per 15 minutes per IP
- **CORS** — Cross-origin resource sharing configured
- **Input validation** — All endpoints validate request body
- **Environment secrets** — All sensitive data in `.env` (never committed)

---

## 🧩 Challenges & Learnings

### Challenges Faced

| Challenge | How We Solved It |
|-----------|-----------------|
| **BERT model download failures** | Built a keyword-based sentiment fallback that works instantly without network access. BERT loads asynchronously in background. |
| **Gemini API rate limiting (429 errors)** | Implemented exponential backoff retry logic (2s → 4s → 8s) and 21 sentiment-aware fallback responses so the bot never appears broken. |
| **Running Python + Node.js together** | Created a Docker container that runs both services, with the Python service launching first and Node.js starting after a 3-second delay. |
| **MongoDB connection failures** | Decoupled server startup from DB connection — the Express server starts immediately and serves the frontend even if MongoDB isn't ready yet. |
| **Generic AI responses** | Enhanced Gemini prompts with specific instructions to reference user's exact words, vary responses, and avoid repetitive patterns. |
| **Crisis safety concerns** | Built a rule-based crisis detection layer that bypasses AI entirely and provides immediate helpline resources — no AI hallucination risk for life-threatening situations. |

### Key Learnings

1. **Microservice architecture is powerful** — Separating the ML model (Python) from the web server (Node.js) means each can fail/restart independently without taking down the other.

2. **Always design for failure** — Every external dependency (MongoDB, Gemini API, BERT model) has a graceful fallback. The app never shows an error screen.

3. **Prompt engineering matters enormously** — The difference between generic and empathetic AI responses came down to prompt specificity: injecting sentiment context, conversation history, and explicit instructions to avoid repetition.

4. **BERT is surprisingly accessible** — HuggingFace Transformers makes it possible to deploy a 66-million parameter model with just 5 lines of Python code.

5. **Mental health AI requires extra safety** — Unlike a general chatbot, mental health applications need crisis detection, helpline resources, and clear disclaimers about not replacing professional help.

---

## 📊 Message Processing Pipeline

```
User types message
       │
       ▼
┌─────────────────┐
│ Crisis Keyword   │──── Yes ──→ Return helpline resources immediately
│ Detection        │             (bypass all AI processing)
└────────┬────────┘
         │ No
         ▼
┌─────────────────┐
│ Python Flask     │
│ /predict         │
│                  │
│ DistilBERT       │──→ { sentiment: "anxious", confidence: 0.87 }
│ Emotion Model    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Gemini 2.0 Flash │
│                  │
│ Prompt includes: │
│ • User message   │──→ Empathetic, context-aware response
│ • Sentiment      │
│ • Chat history   │
│ • Guidelines     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ MongoDB          │
│                  │
│ Save:            │
│ • User message   │
│ • Bot response   │
│ • Sentiment data │
│ • Timestamp      │
└────────┬────────┘
         │
         ▼
Response sent to user with sentiment badge
```

---


---



---

## 🙏 Acknowledgments

- **[Google Gemini](https://ai.google.dev/)** — Generative AI for response generation
- **[HuggingFace](https://huggingface.co/)** — Transformers library and pre-trained BERT models
- **[MongoDB Atlas](https://www.mongodb.com/atlas)** — Cloud database hosting
- **[Render](https://render.com)** — Application deployment

---

<p align="center">
  <strong>Built with 💙 for mental health awareness</strong><br/>
  <em>Because everyone deserves to be heard.</em>
</p>
