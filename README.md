# MindBridge — Healthcare Patient Scheduling Chatbot

A full-stack healthcare scheduling application built with **React + TypeScript**, **Supabase**, and **AI (OpenAI/Claude/Gemini)**. Patients chat with an AI to describe their mental health needs, get matched with therapists, and book appointments — which are synced to Google Calendar.

---

## ✨ Features

### Patient-Facing Chat
- **Natural language intake** — describe your problem conversationally
- **AI-powered matching** — matches symptoms/needs to therapist specialties
- **Insurance verification** — filters by accepted insurance
- **Interactive booking calendar** — pick date & time, confirm with email

### Admin Dashboard
- **Secure login** (Supabase Auth)
- **Inquiry management** with status tracking (pending → matched → scheduled)
- **Appointment list** with Google Calendar sync status
- **Therapist directory** with specialty and insurance info
- **Real-time stats** overview

### Backend (Supabase Edge Functions)
- `handle-chat` — AI message processing + inquiry storage
- `find-therapist` — AI-powered specialty extraction + DB matching
- `book-appointment` — Creates appointment + Google Calendar event
- `get-admin-data` — Admin-authenticated data fetch

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Supabase CLI (`npm install -g supabase`)
- Supabase account at [supabase.com](https://supabase.com)
- OpenAI API key (or Claude/Gemini)
- Google Cloud Platform project (for Calendar)

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Environment

```bash
cp .env.example .env
```

Edit `.env`:
```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

### 3. Set Up Supabase

```bash
# Initialize and link your project
supabase init
supabase link --project-ref YOUR_PROJECT_REF

# Apply the database schema
supabase db push
# Or run the SQL manually in Supabase SQL Editor:
# supabase/migrations/001_init_schema.sql
```

### 4. Set Edge Function Secrets

```bash
supabase secrets set OPENAI_API_KEY=sk-...
supabase secrets set GOOGLE_CLIENT_ID=your-google-client-id
supabase secrets set GOOGLE_CLIENT_SECRET=your-google-client-secret
```

### 5. Deploy Edge Functions

```bash
supabase functions deploy handle-chat
supabase functions deploy find-therapist
supabase functions deploy book-appointment
supabase functions deploy get-admin-data
```

### 6. Start Development Server

```bash
npm run dev
```

Visit:
- Patient chat: `http://localhost:5173/chat`
- Admin portal: `http://localhost:5173/admin`

---

## 📁 Project Structure

```
healthcare-scheduler/
├── src/
│   ├── pages/
│   │   ├── LandingPage.tsx      # Home/marketing page
│   │   ├── ChatPage.tsx         # Patient chat interface
│   │   └── AdminPage.tsx        # Admin dashboard
│   ├── components/
│   │   ├── TherapistCard.tsx    # Therapist match card
│   │   └── BookingModal.tsx     # Date/time booking UI
│   ├── lib/
│   │   ├── supabase.ts          # Supabase client
│   │   └── mockData.ts          # Demo data + matching logic
│   └── types/
│       └── index.ts             # TypeScript types
├── supabase/
│   ├── functions/
│   │   ├── handle-chat/         # AI chat + inquiry saving
│   │   ├── find-therapist/      # Therapist matching
│   │   ├── book-appointment/    # Booking + Google Calendar
│   │   └── get-admin-data/      # Admin data fetch
│   └── migrations/
│       └── 001_init_schema.sql  # Full DB schema + seed data
└── package.json
```

---

## 🔑 Admin Portal Demo

The app includes a **fully functional demo mode** using mock data (no Supabase required):

- URL: `/admin`
- Email: `admin@mindbridge.health`
- Password: `demo1234`

---

## 🗓️ Google Calendar Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a project → Enable **Google Calendar API**
3. Create **OAuth 2.0 credentials** (Web Application type)
4. Set authorized redirect URIs (e.g., `https://your-project.supabase.co/functions/v1/oauth-callback`)
5. For each therapist, run the OAuth flow to get a refresh token
6. Store the refresh token in `therapists.google_refresh_token`

**Prototype shortcut**: Use the [Google OAuth Playground](https://developers.google.com/oauthplayground) to manually generate a refresh token for a test account.

---

## 🏗️ Architecture

```
React Frontend
    │
    ├── /chat → ChatPage (AI chat, booking)
    ├── /admin → AdminPage (dashboard, auth)
    │
    ▼
Supabase Edge Functions (Deno/TypeScript)
    │
    ├── handle-chat → OpenAI API → Supabase DB
    ├── find-therapist → OpenAI API → therapists table
    ├── book-appointment → Google Calendar API → appointments table
    └── get-admin-data → Supabase Auth → inquiries + appointments
    │
    ▼
PostgreSQL (Supabase)
    ├── therapists
    ├── inquiries
    └── appointments
```

---

## 🔒 Security Notes

- **RLS enabled** on all tables
- **Service role key** only used in Edge Functions (never exposed to frontend)
- **Anon key** in frontend is safe (RLS + Edge Functions gate all writes)
- **Google refresh tokens** should be encrypted at rest in production
- **Patient data** uses hashed identifiers — never raw PII
- Edge functions verify **JWT tokens** for admin access

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Vite |
| Styling | CSS Modules (no CSS framework) |
| Routing | React Router v6 |
| Backend | Supabase Edge Functions (Deno) |
| Database | PostgreSQL (Supabase) |
| Auth | Supabase Auth |
| AI | OpenAI GPT-4o-mini (swap for Claude/Gemini) |
| Calendar | Google Calendar API v3 |

---

## 📋 Next Steps

- [ ] Replace mock data with live Supabase queries
- [ ] Add patient email/SMS confirmation
- [ ] Implement therapist availability checking
- [ ] Add multi-turn conversation history
- [ ] Build Google OAuth flow for therapist onboarding
- [ ] Add HIPAA-compliant audit logging
- [ ] Implement real-time admin notifications (Supabase Realtime)

# LINK TO THE PROJECT
https://69e8dec35ecc5979d2220fc0--helpful-khapse-def140.netlify.app/


