# Limitless — AI-Powered Meeting Assistant
 
A cross-platform mobile application built with Flutter that acts as an intelligent digital assistant for meeting management, audio transcription, calendar sync, and team communication.
 
Built for the Advanced User Interfaces course — Politecnico di Milano, 2025–2026.
 
---
 
## Features
 
- **Audio Recording & Transcription** — Record meetings and get automatic transcriptions via Azure Whisper
- **AI Briefing** — GPT-4o generates a concise spoken briefing from each transcript
- **Digital Twin Avatar** — Animated Rive avatar reads briefings aloud via ElevenLabs TTS
- **Calendar Integration** — Sync with Google Calendar; auto-detect and create events from transcripts
- **Team Chat** — Private and group messaging with real-time updates
- **Lifelog** — Archive of all recordings, transcriptions, and summaries
---
 
## Tech Stack
 
| Layer | Technology |
|---|---|
| Framework | Flutter 3.9.2 / Dart |
| Backend & DB | Supabase (PostgreSQL, Auth, Storage, Realtime) |
| Speech-to-Text | Azure OpenAI — Whisper |
| Language Model | Azure OpenAI — GPT-4o |
| Text-to-Speech | ElevenLabs |
| Calendar | Google Calendar API |
| Auth | Supabase Auth + Google OAuth 2.0 |
| Animation | Rive |
| State Management | Provider |
| Local Cache | Hive |
 
---
 
## Project Structure
 
```
lib/
├── config/
│   └── keys.dart                  # API keys and endpoints (see setup)
├── core/
│   └── services/
│       ├── ai_service.dart
│       ├── audio_recording_service.dart
│       ├── auth_service.dart
│       ├── briefing_service.dart
│       ├── calendar_service.dart
│       ├── chat_service.dart
│       ├── meeting_repository.dart
│       └── openai_service.dart
├── models/
│   ├── calendar_event_model.dart
│   ├── chat_models.dart
│   ├── conversation_model.dart
│   ├── lifelog_model.dart
│   └── meeting_model.dart
├── ui/
│   ├── auth/
│   ├── calendar/
│   ├── chat/
│   ├── home/
│   ├── messages/
│   ├── profile/
│   ├── transcript/
│   └── main_layout.dart
├── widgets/
│   ├── briefing_avatar.dart
│   └── custom_buttom_nav.dart
└── main.dart
```
 
---
 
## Recording → Briefing Pipeline
 
```
Record Audio
    ↓
Upload to Supabase Storage
    ↓
Transcribe with Azure Whisper
    ↓
Extract calendar events (GPT-4o)  →  Sync to Google Calendar
    ↓
Generate briefing (GPT-4o)
    ↓
Save meeting to database
    ↓
Read briefing aloud (ElevenLabs + Rive avatar)
```
 
---
 
## Setup
 
### Prerequisites
- Flutter SDK 3.9.2+
- Supabase account
- Azure OpenAI account (Whisper + GPT-4o deployments)
- ElevenLabs account
- Google Cloud project with Calendar API enabled
### Configuration
 
Create `lib/config/keys.dart` and fill in your credentials:
 
```dart
class SupabaseConfig {
  static const supabaseUrl = 'YOUR_SUPABASE_URL';
  static const supabaseAnonKey = 'YOUR_SUPABASE_ANON_KEY';
}
 
class AzureConfig {
  static const apiKey = 'YOUR_AZURE_API_KEY';
  static const endpoint = 'YOUR_AZURE_ENDPOINT';
  static const whisperDeploymentName = 'YOUR_WHISPER_DEPLOYMENT';
  static const gptDeploymentName = 'YOUR_GPT_DEPLOYMENT';
}
 
class ElevenLabsConfig {
  static const apiKey = 'YOUR_ELEVENLABS_API_KEY';
  static const voiceId = 'YOUR_VOICE_ID';
}
```
 
> **Never commit `keys.dart` to version control.** Add it to `.gitignore`.
 
### Supabase Setup
 
1. Create tables: `profiles`, `meetings`, `calendar_events`, `chats`, `chat_participants`, `messages`
2. Configure Row Level Security (RLS) policies
3. Create storage buckets: `meeting_recordings`, `avatars`
4. Enable Google OAuth provider in Supabase Auth
### Run
 
```bash
flutter pub get
flutter run
```
 
---
 
## Known Limitations
 
- Transcription currently English-only
- Google Calendar sync is one-directional (app → Google)
- ElevenLabs voice is fixed (not selectable from UI)
---
 
## Possible Future Improvements
 
- Multi-language transcription support
- Voice selection from UI
- Offline mode with local caching
- PDF/Word export for meeting summaries
- Push notifications for calendar events
- Full-text search across transcriptions
- Slack / Teams integration
---
 
## Authors
 
Developed for the Advanced User Interfaces course at Politecnico di Milano.
Greta Severi, Arianna Balducci, Ernesto Minghini.