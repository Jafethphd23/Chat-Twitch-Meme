# replit.md

## Overview

Twitch Chat Translator Bot (Chibi) - A real-time chat translation tool for Twitch streams. The application connects to any Twitch channel via IRC, translates incoming chat messages using OpenAI's GPT models, and includes a ChatGPT-powered chat assistant with a kawaii personality (triggered by !chibi command). Features multi-language profanity filtering and protection against AI/programming topic discussions.

**Creator**: ostian  
**Target Channel**: https://www.twitch.tv/nagayama_meme

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend (React + TypeScript)

- **Build Tool**: Vite with React plugin
- **UI Framework**: Shadcn/ui components built on Radix UI primitives
- **Styling**: Tailwind CSS with custom kawaii pink-blue gradient theme (not the original Twitch purple theme mentioned in design docs)
- **Routing**: Wouter for lightweight client-side routing
- **State Management**: React Query for server state, custom WebSocket hook for real-time chat
- **Entry Point**: `client/src/main.tsx` → `client/src/App.tsx`

### Backend (Express + TypeScript)

- **Framework**: Express.js with TypeScript, compiled via esbuild
- **Real-time**: WebSocket server (`ws` library) at `/ws` path for dashboard communication
- **Twitch Integration**: `tmi.js` library for Twitch IRC chat connection
- **AI Services**: OpenAI API (gpt-4.1-mini) for translation and chat responses
- **Entry Point**: `server/index.ts`

### Key Server Modules

- `server/bot.ts` - Twitch bot controller with event system
- `server/translate.ts` - OpenAI-powered translation with Unicode script detection and caching
- `server/chat.ts` - ChatGPT assistant with profanity filtering and topic protection
- `server/twitch.ts` - Low-level Twitch IRC client wrapper
- `server/routes.ts` - API endpoints and WebSocket handler

### Data Flow

1. Frontend connects via WebSocket to `/ws`
2. User specifies Twitch channel and target language
3. Backend connects to Twitch IRC via tmi.js
4. Incoming messages are translated via OpenAI API
5. Events broadcast to all connected dashboard clients via WebSocket

### Database

- **ORM**: Drizzle ORM with PostgreSQL dialect configured
- **Schema**: `shared/schema.ts` - Zod schemas for chat messages and WebSocket protocol
- **Note**: Database is configured but not actively used for core chat functionality; available for future preference persistence

## External Dependencies

### APIs & Services

- **OpenAI API** (`OPENAI_API_KEY` env var) - Powers translation and chat assistant features
- **Twitch IRC** - Read-only connection to chat channels via tmi.js (no OAuth required for reading)
- **Neon Database** (`DATABASE_URL` env var) - PostgreSQL serverless database via @neondatabase/serverless

### Key NPM Packages

- `tmi.js` - Twitch Messaging Interface for IRC connections
- `openai` - Official OpenAI SDK
- `ws` - WebSocket server implementation
- `drizzle-orm` + `drizzle-kit` - Database ORM and migrations
- `@tanstack/react-query` - Server state management
- `@radix-ui/*` - Accessible UI primitives
- `tailwindcss` - Utility-first CSS framework

### Path Aliases

- `@/*` → `./client/src/*`
- `@shared/*` → `./shared/*`
- `@assets` → `./attached_assets`