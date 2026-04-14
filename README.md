# 🎙️ LocalTTS

**Free Voice Messages for AI Agents**

LocalTTS lets your OpenClaw agent send voice messages **without paying for cloud TTS APIs**. Instead of routing text to expensive cloud services (OpenAI, ElevenLabs, etc.), this runs locally on your host—completely free.

## The Problem It Solves

Most AI platforms charge extra for voice:
- OpenAI's TTS: $0.015 per 1K characters
- ElevenLabs: Limited free tier, then paid
- Cloud providers: Monthly subscriptions, usage caps

**LocalTTS is the free alternative.** Run synthesis locally, pay nothing, speak unlimited.

## What This Actually Is

This isn't about "pro audio quality"—it's about **enabling voice communication without the paywall**.

- **Local synthesis**: Piper TTS runs on your machine
- **Zero API costs**: No cloud calls, no usage limits
- **Good enough quality**: Fine for Telegram voice notes, quick replies, notifications
- **Messaging ready**: Generates MP3s that play natively in Telegram/Discord

## How It Works

1. **Text comes in** → Agent decides to reply with voice
2. **Piper synthesizes** → Local ONNX model generates audio
3. **ffmpeg converts** → WAV → MP3 for delivery
4. **Message sent** → Voice note delivered via Telegram/Discord

## Quick Start

### Prerequisites
- `piper` binary installed
- `ffmpeg` for MP3 conversion
- Voice models in `/voices/` directory

### Generate a Voice Note
```bash
echo "Hey James, here's your update." | ./scripts/speak.sh
```

The output goes to `/root/.openclaw/workspace/media/voice.mp3`, ready to send.

## The Pronunciation Layer

Piper can struggle with some words. The `pronunciation.md` file maps problematic terms to phonetic spellings before synthesis:

- `degrees` → `de-grees`
- `winds` → `windz`
- `AI` → `A-I`

This fixes robotic artifacts without needing expensive cloud voices.

## Cost Comparison

| Method | Cost Per Message | Monthly (1K msgs) |
|--------|------------------|-------------------|
| OpenAI TTS | ~$0.0015 | ~$45 |
| ElevenLabs | Limited free, then paid | $5-50+ |
| **LocalTTS** | **$0** | **$0** |

## Project Structure

```
localtts/
├── README.md          # This file
├── SKILL.md           # OpenClaw skill documentation
├── pronunciation.md   # Word → phonetic mappings
└── scripts/
    └── speak.sh       # Synthesis pipeline
```

---

*Free voice for agents. No subscriptions. No limits.*
