# Piper Pro Voice - Free Voice Messages for OpenClaw

Piper Pro Voice lets your OpenClaw agent send voice messages **without paying for cloud TTS APIs** (OpenAI, ElevenLabs, etc.). It uses local Piper TTS synthesis to generate MP3 voice notes for Telegram, Discord, and other messaging platforms—completely free, with no usage limits.

## Why Use This?

Most AI platforms charge extra for voice features:
- **OpenAI TTS**: $0.015 per 1K characters
- **ElevenLabs**: Limited free tier, then paid subscriptions
- **Google/Amazon**: Monthly fees, usage quotas

**Piper Pro Voice: $0 forever.** Local synthesis, unlimited messages.

## What It Actually Does

This isn't about "pro audio quality"—it's about **enabling voice communication without the paywall**.

- **Local synthesis**: Piper TTS runs on your machine
- **Zero API costs**: No cloud calls, no usage limits
- **Good enough quality**: Fine for Telegram voice notes, quick replies, notifications
- **Messaging ready**: Generates MP3s that play natively in Telegram/Discord

## How It Works

1. Agent decides to reply with voice
2. **Piper synthesizes** locally using ONNX models
3. **ffmpeg converts** WAV → MP3
4. **Voice note sent** via Telegram/Discord

## Installation

1. Install `piper` binary and `ffmpeg` on your host
2. Download ONNX voice models to `voices/` directory
3. Configure paths in `TOOLS.md`

## The Pronunciation Layer

Piper can struggle with some words. The `pronunciation.md` file maps problematic terms to phonetic spellings before synthesis:

- `degrees` → `de-grees`
- `winds` → `windz`
- `AI` → `A-I`

This fixes robotic artifacts without needing expensive cloud voices.

## Usage

Generate a voice message:
```bash
echo "Your text here" | ./scripts/speak.sh
```

Output goes to `/root/.openclaw/workspace/media/voice.mp3`, ready to send.

## Cost Comparison

| Method | Cost Per 1K Messages | Notes |
|--------|-------------------|-------|
| OpenAI TTS | ~$45/month | Per-character billing |
| ElevenLabs | $5-50+/month | Tiered plans |
| **Piper Pro Voice** | **$0** | **Unlimited, local** |

## Structure

```
piper-pro-voice/
├── SKILL.md           # This file
├── README.md          # Project documentation
├── pronunciation.md   # Word → phonetic mappings
└── scripts/
    └── speak.sh       # Synthesis pipeline
```

---
*Free voice for agents. No subscriptions. No limits.*
