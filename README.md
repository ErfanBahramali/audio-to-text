# 🎧 audio-to-text

A free and robust backend package for transcribing audio files to text using the [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API).

---

## Features

- ✅ Convert audio files to text
- 🎤 Supports multiple languages
- 🧠 Uses **Web Speech API** inside a headless browser (via Puppeteer)
- 🔊 Streams audio using a virtual microphone
- 💾 Supports all audio file formats supported by ffmpeg (e.g., .mp3, .wav, .ogg, .m4a, etc.)
- 🪄 Automatically sets up required audio routing using `pactl` and `paplay`
- ⚙️ Works in Linux environments with PipeWire or PulseAudio

---

## 🛠 Requirements

Before installing and using this package, please ensure the following dependencies are installed and properly configured on your system:

- [**ffmpeg**](https://ffmpeg.org/) — for audio format conversion and processing
- [**ffprobe**](https://ffmpeg.org/) — for audio validation (comes with ffmpeg)
- [**PipeWire**](https://pipewire.org/) — **RECOMMENDED** modern audio server
- [**PulseAudio**](https://www.freedesktop.org/wiki/Software/PulseAudio/) — alternative audio server (older systems)
- [**pactl**](https://docs.pipewire.org/) — Audio control tool
- [**paplay**](https://docs.pipewire.org/) — Audio playback utility
- [**Microsoft Edge**](https://www.microsoft.com/en-us/edge) — Microsoft Edge Browser
- [**Google Chrome**](https://www.google.com/chrome/) or [**Chromium**](https://www.chromium.org/) — browsers
- [**Node.js**](https://nodejs.org/) — version 18 or higher is recommended
- [**bun**](https://bun.sh/) — optional, recommended for development and build tasks
- **Internet connection** (required for browser-based speech recognition)

## Install on Ubuntu/Debian:

### PipeWire (Recommended - Modern Audio Server):

```zsh
sudo apt update
sudo apt install ffmpeg pipewire pipewire-pulse wireplumber
```

### PulseAudio (Alternative - Older Systems):

```zsh
sudo apt update
sudo apt install ffmpeg pulseaudio-utils pulseaudio
```

---

## 🔐 Permissions

- Make sure Node.js has permission to run `pactl` and `paplay`
- Puppeteer will launch a headless browser and use your virtual audio devices

---


## 📦 Installation

To install with Bun:

```bash
bun add audio-to-text-node
```

Or with npm:

```bash
npm install audio-to-text-node
```

---

## 🧼 Cleanup

The package creates temporary folders in `/tmp/audio-to-text` and cleans them up automatically after use.

---

## ✨ Usage

```typescript
import { transcribeFromFile } from "audio-to-text-node";

async function main() {
  const transcript = await transcribeFromFile("/path/to/audio.wav", {
    language: "en-US",
    executablePath: "/usr/bin/microsoft-edge",
    speakerDevice: "virtual_speaker",
    microphoneDevice: "virtual_microphone",
  });

  console.log(transcript);
}

main();
```

---

## Tested Distributions

| Distribution | Version | Status           |
| ------------ | ------- | ---------------- |
| Ubuntu       | 24.10   | ✅ Fully Tested  |
| MacOS        | -       | ❌ Not Supported |
| Windows      | -       | ❌ Not Supported |

> **Note:** This package is designed for Linux environments.

---

## 📚 API Reference

### 🧠 `transcribeFromFile(filePath: string, options?: { language?: string; executablePath?: string; speakerDevice?: string; microphoneDevice?: string }): Promise<string>`

| 🧩 Parameter               | 📝 Type   | 📖 Description                                          | 🧵 Default             |
| -------------------------- | -------- | ------------------------------------------------------ | ---------------------- |
| `filePath`                 | `string` | Path to the audio file (`.wav`, `.mp3`, `.ogg`, etc.) | —                      |
| `options.language`         | `string` | Language code for transcription                       | `'en-US'`              |
| `options.executablePath`   | `string` | Path to browser executable                            | Auto-detected          |
| `options.speakerDevice`    | `string` | Virtual speaker device name (PipeWire/PulseAudio)     | `'virtual_speaker'`    |
| `options.microphoneDevice` | `string` | Virtual microphone device name (PipeWire/PulseAudio)  | `'virtual_microphone'` |

#### Browser Detection Priority:
1. **Microsoft Edge** - `/usr/bin/microsoft-edge`
2. **Google Chrome** - `/usr/bin/google-chrome`
3. **Chromium** - `/usr/bin/chromium-browser`

🔁 **Returns:** `Promise<string>` — The transcribed text.

---

### ⚙️ How it works:

1. ✅ Validates and splits the audio file into 5-second chunks
2. 🎛 Sets up virtual audio devices for routing (PipeWire/PulseAudio)
3. 🧭 Launches a headless browser and uses Web Speech API for transcription
4. 🧹 Cleans up temporary files and restores audio routing

---

## 🎵 Supported Audio Formats

This package supports all audio formats supported by ffmpeg. For a full list, see:

- [FFmpeg Supported File Formats](https://ffmpeg.org/general.html#File-Formats)

Common formats include: `.wav`, `.mp3`, `.ogg`, `.flac`, `.aac`, `.m4a`, and more.

---

## 🌐 Supported Languages

You can use any language supported by the Web Speech API and Google Speech-to-Text. For a full list, see:

- [Google Speech-to-Text Supported Languages](https://cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages)

Specify the language code (e.g., `en-US`, `fa-IR`, `fr-FR`, etc.) in the `language` option.

---


## 🛠️ Troubleshooting

- Ensure all prerequisites are installed and available in your PATH (`which ffmpeg`, `which ffprobe`, `which pactl`, `which paplay`)
- For best audio performance: Use PipeWire (modern) over PulseAudio (legacy)
- For long audio files, ensure enough disk space in `/tmp`
- If you get permission errors, run with appropriate user rights
- For best results, use high-quality audio files (16kHz mono recommended)
- Make sure your connection is stable and not interrupted during transcription
- Only Linux with PipeWire or PulseAudio is supported
- If browser detection fails, explicitly set `executablePath` to your browser location

### Common Browser Paths:
```bash
# Check if browsers are installed
which microsoft-edge
which google-chrome
which chromium-browser
```

### Audio System Check:
```bash
# Check if PipeWire is running (recommended)
systemctl --user status pipewire pipewire-pulse

# Check if PulseAudio is running (alternative)
systemctl --user status pulseaudio

# Test audio commands
which pactl paplay # Should work with both PipeWire and PulseAudio
```

---

## 📝 Changelog

### Version 0.2.0 (Latest)
- 🚀 **BREAKING**: Switched from `puppeteer` to `puppeteer-core` for better control
- ✨ Added **multi-browser support** with automatic detection (Edge, Chrome, Chromium)
- ⚙️ Added `executablePath` option to specify custom browser location
- 🎵 Added **PipeWire support** (recommended audio server)

---

## 💬 Contributing

Pull requests and issues are welcome!<br>
Please open issues for any bugs or feature requests.
When contributing, please:

- Use clear commit messages
- Follow TypeScript best practices

---

## 📋 License

MIT © 2025 [ErfanBahramali](https://github.com/ErfanBahramali)

---
