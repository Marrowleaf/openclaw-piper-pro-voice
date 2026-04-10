# 🎙️ Piper Pro Voice

**High-Fidelity Local Text-to-Speech for OpenClaw**

Piper Pro Voice is a refined implementation of the Piper TTS engine designed specifically for OpenClaw agents. It moves beyond simple "text-to-audio" conversion by introducing a **Pronunciation Layer** and a **Voice Management system** to eliminate robotic artifacts and provide a natural, human-like experience.

## ✨ Why Piper Pro Voice?

Standard TTS often struggles with specific words, blending them together or mispronouncing technical terms. Piper Pro Voice solves this by:

- **Phonetic Mapping**: Using a curated `pronunciation.md` file to translate problematic words into phonetic spellings (e.g., `winds` $\rightarrow$ `windz`) before they hit the engine.
- **Multi-Fidelity Support**: Optimized for various ONNX voice models, allowing users to switch between "Low", "Medium", and "High" fidelity voices based on their hardware and preference.
- **Local-First**: Zero cloud dependency. No API keys, no monthly subscriptions, and zero latency.

## 📱 Integrated Messaging Delivery

A voice is only as good as its delivery. Piper Pro Voice is engineered for seamless integration with modern messaging surfaces, particularly **Telegram, Discord, and WhatsApp**.

By leveraging a specialized synthesis-to-delivery pipeline, the system ensures:
- **Native Audio Format**: Automatic conversion to MP3/OGG to ensure native playback on mobile devices.
- **Asynchronous Delivery**: Synthesis happens in the background, delivering "voice notes" that feel like a natural part of the conversation rather than a static file attachment.
- **Contextual Voice Responses**: Ability to trigger voice replies based on specific agent intents, transforming a text-based assistant into a truly interactive companion.

## 🚀 Quick Start

### 1. Prerequisites
Ensure you have the following installed on your host:
- `piper` (The ONNX TTS engine)
- `ffmpeg` (For audio conversion)

### 2. Voice Models
Download your preferred Piper ONNX voices and place them in your workspace voices directory. We recommend the `en_US-amy-medium` model for a great balance of clarity and speed.

### 3. Usage
You can generate high-quality voice notes using the included helper script:

```bash
echo "Hello James, your weather update is ready." | ./scripts/speak.sh
```

## 🛠️ The Pronunciation Layer

The "secret sauce" of this skill is the `pronunciation.md` file. To make your agent sound more human, add mappings for words that sound robotic.

**Example mappings included:**
- `degrees` $\rightarrow$ `de-grees`
- `AI` $\rightarrow$ `A-I`
- `winds` $\rightarrow$ `windz`

## 📦 Project Structure

```text
piper-pro-voice/
├── SKILL.md           # OpenClaw skill documentation
├── README.md          # Project overview
├── pronunciation.md   # Phonetic mapping guide
└── scripts/
    └── speak.sh       # Core synthesis & MP3 conversion pipeline
```

---
*Built with ❤️ for the OpenClaw community.*
