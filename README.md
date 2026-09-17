# 🔒 Secured Message — Cipherline Secure Messaging Platform

![Next.js](https://img.shields.io/badge/Next.js-14-black?style=for-the-badge&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![WebSockets](https://img.shields.io/badge/WebSockets-Realtime-blueviolet?style=for-the-badge)

A full-stack, pixel-accurate **Signal Messenger Clone** built with **Next.js 14 (App Router)**, **TypeScript**, **FastAPI**, and **WebSockets**. Features real-time 1-on-1 and group messaging, per-recipient status ticks (`sent` → `delivered` → `read`), typing indicators, dark mode, and a responsive desktop 3-pane layout.

---

## ✨ Features

- 💬 **Real-time Messaging**: Instant bi-directional messaging over WebSockets with automatic fallback synchronization.
- 👥 **Group Chats & Admin Controls**: Create groups, manage members, assign admins, and broadcast group updates.
- ✅ **Multi-Recipient Status Ticks**:
  - 🕓 Sending
  - ✓ Sent
  - ✓✓ Delivered
  - ✓✓ Read (Blue ticks)
- ✍️ **Typing Indicators & Presence**: Live visual feedback when users are typing or online/offline.
- 💬 **Reply Threads & Reactions**: Quote specific messages and react with emojis.
- 🎨 **Signal Design System**: Clean typography (`Inter`), custom color tokens, dark mode toggle, and micro-animations using Framer Motion & Lottie.
- 🔐 **Custom Auth Flow**: JWT token authentication with phone/username registration and session persistence.
- 📱 **Fully Responsive Layout**: 3-pane layout for desktop and dynamic stack-navigation for mobile viewports.

---

## 🛠️ Tech Stack

| Domain | Technology | Description |
|---|---|---|
| **Frontend** | Next.js 14 (App Router) | React Framework with File-based Routing |
| **Language** | TypeScript | Type safety for message payloads & store states |
| **Styling** | Tailwind CSS + shadcn/ui | Signal-inspired UI design & theme system |
| **State Management** | Zustand | Lightweight store for active chat, UI state, & auth |
| **Backend** | FastAPI (Python 3.10+) | High-performance async Python backend framework |
| **Real-time Engine** | Native WebSockets (`WebSocketManager`) | Live socket channel management & broadcast |
| **Database & ORM** | SQLite / PostgreSQL + SQLAlchemy | Relational schema with async pool support |
| **Security** | PyJWT + Passlib (Bcrypt) | JWT authentication & password hashing |

---

## 🗄️ Database Schema Overview

```
users
├── id (PK, UUID)
├── username / phone_number (unique)
├── display_name, avatar_url, about_status
├── password_hash
└── created_at

conversations
├── id (PK, UUID)
├── type ('direct' | 'group')
├── group_name, group_avatar_url
├── created_by (FK -> users.id)
└── created_at

conversation_members
├── id (PK)
├── conversation_id (FK -> conversations.id)
├── user_id (FK -> users.id)
├── role ('member' | 'admin')
└── UNIQUE(conversation_id, user_id)

messages
├── id (PK, UUID)
├── conversation_id (FK -> conversations.id)
├── sender_id (FK -> users.id)
├── content, attachment_url
├── reply_to_message_id (FK -> messages.id)
└── created_at

message_status
├── id (PK)
├── message_id (FK -> messages.id)
├── user_id (FK -> users.id)          # Per-recipient status tracking
├── status ('sent' | 'delivered' | 'read')
└── UNIQUE(message_id, user_id)
```

---

## 📁 Directory Structure

```
Secured-Message/
├── frontend/                  # Next.js 14 Frontend Application
│   ├── app/                   # App Router Pages ((auth), chats, settings)
│   ├── components/            # Chat Bubbles, Sidebar, Composer, UI Primitives
│   ├── lib/                   # API Client, WebSocket Client Hook, Utilities
│   ├── store/                 # Zustand Global State Stores
│   └── public/                # Static Assets & Lottie Animations
├── backend/                   # FastAPI Backend Server
│   ├── app/
│   │   ├── models/            # SQLAlchemy Database ORM Models
│   │   ├── routers/           # REST API Routes (Auth, Users, Conversations)
│   │   ├── schemas/           # Pydantic Request/Response Models
│   │   ├── websocket/         # WebSocket Connection Manager & Events
│   │   └── seed/              # Database Seeder Script
│   ├── database.py            # SQLAlchemy Engine & Session Configuration
│   └── main.py                # FastAPI Application Entry Point
└── README.md
```

---

## 🚀 Quick Start Guide

### Prerequisites
- Python 3.10+
- Node.js 18+ and npm

### 1. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
pip install -r requirements.txt

# Seed the database with demo users & conversations
python -m app.seed.seed_data

# Start the FastAPI server (runs on http://127.0.0.1:8000)
python -m uvicorn main:app --port 8000 --reload
```

### 2. Frontend Setup

```bash
# Open a new terminal tab and navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start Next.js development server (runs on http://localhost:3000)
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to test the application!

---

## 🔑 Demo Credentials

The database comes pre-seeded with test accounts. Password for all demo accounts is **`demo123`**:

| Account | Username | Phone Number | Password |
|---|---|---|---|
| **Primary Demo** | `demo` | `+1 (555) 019-2831` | `demo123` |
| **Alice** | `alice` | `+1 (555) 012-3456` | `demo123` |
| **Bob** | `bob` | `+1 (555) 012-7890` | `demo123` |

*Note: For OTP authentication screens, use the fixed verification code: **`123456`**.*

---

## 📜 Environment Variables

### Backend (`backend/.env`)
```env
DATABASE_URL=sqlite:///./signal_clone.db
JWT_SECRET=your_jwt_secret_key
JWT_ALGORITHM=HS256
FRONTEND_URL=http://localhost:3000
```

### Frontend (`frontend/.env.local`)
```env
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_WS_URL=ws://localhost:8000
```

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
