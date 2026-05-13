<div align="center">

<img src="https://img.shields.io/badge/DubWiz-AI%20Video%20Dubbing-blueviolet?style=for-the-badge&logo=youtube&logoColor=white" alt="DubWiz Banner"/>

# 🎬 DubWiz — AI-Powered Video Dubbing System

**Automatically dub any English video into Indian languages with synchronized audio using state-of-the-art AI models.**

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=flat-square&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Vite](https://img.shields.io/badge/Vite-4.x-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

</div>

---

## 📖 Overview

**DubWiz** is a full-stack AI system that takes an English video and automatically produces a fully dubbed version in your chosen Indian language — complete with a synthesized voice track synced to the original video. No manual transcription, no human voiceover artists needed.

### How it works (under the hood):

```
English Video
     │
     ▼
🎵  Audio Extraction     (MoviePy)
     │
     ▼
📝  Speech-to-Text       (OpenAI Whisper)
     │
     ▼
🌍  Translation          (Facebook MBart-50)
     │
     ▼
🗣️  Text-to-Speech       (Silero TTS + Aksharamukha)
     │
     ▼
🎬  Video Reconstruction (MoviePy)
     │
     ▼
✅  Dubbed Video Output
```

---

## 🌐 Supported Languages

| Language | Code | Voice |
|----------|------|-------|
| Hindi | `hin` | Male |
| Marathi | `mar` | Female |
| Gujarati | `guj` | Female |
| Kannada | `kan` | Female |
| Telugu | `tel` | Male |

---

## 🏗️ Project Structure

```
DubWiz/
├── DubWiz/
│   └── VideoDubbingSystem/
│       ├── Backend/
│       │   ├── server.py            # Flask API server
│       │   ├── ML_Model.py          # Core AI pipeline (ASR → Translate → TTS)
│       │   ├── extract_audio.py     # Audio/video separation via MoviePy
│       │   ├── final_video.py       # Merges dubbed audio back into video
│       │   ├── uploader.py          # File upload handler
│       │   ├── Input/               # Uploaded videos go here
│       │   ├── extracted/           # Intermediate audio/video files
│       │   ├── Output/              # Subtitles & translated audio
│       │   ├── Final/               # Final dubbed video output
│       │   └── Requirements/
│       │       └── requirements.txt
│       └── Frontend/
│           ├── src/                 # React components
│           ├── index.html
│           ├── package.json
│           └── vite.config.js
└── README.md
```

---

## 🤖 AI Models Used

| Model | Provider | Purpose |
|-------|----------|---------|
| `whisper-tiny.en` | OpenAI | Speech recognition — converts English audio to text |
| `mbart-large-50-one-to-many-mmt` | Facebook/Meta | Neural machine translation — English → Indian languages |
| `silero-tts` (v4_indic) | Snakers4 | Text-to-Speech synthesis for Indian languages |
| Aksharamukha | Open Source | Script transliteration (Devanagari/Gujarati/etc → ISO Roman) |

---

## ⚙️ Tech Stack

**Backend**
- Python 3.9+
- Flask + Flask-CORS
- PyTorch (CPU/CUDA)
- HuggingFace Transformers & Datasets
- MoviePy
- SoundFile, Librosa

**Frontend**
- React 18
- Vite 4
- Tailwind CSS
- Axios
- React Router DOM

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:
- [Python 3.9+](https://www.python.org/downloads/)
- [Node.js 18+](https://nodejs.org/)
- [Git](https://git-scm.com/)
- `ffmpeg` (required by MoviePy)

```bash
# Install ffmpeg (Mac)
brew install ffmpeg

# Install ffmpeg (Ubuntu/Debian)
sudo apt install ffmpeg
```

---

### 1. Clone the Repository

```bash
git clone https://github.com/LaveenChordia/DubWiz.git
cd DubWiz
```

---

### 2. Backend Setup (Python / Flask)

```bash
# Navigate to the backend
cd DubWiz/VideoDubbingSystem/Backend

# Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate        # Mac/Linux
# venv\Scripts\activate         # Windows

# Install dependencies
pip install torch torchaudio transformers datasets soundfile \
            aksharamukha flask flask-cors moviepy sentencepiece \
            protobuf omegaconf librosa

# Start the Flask server
python3 server.py
```

> ✅ The backend will start at **http://127.0.0.1:5000**

---

### 3. Frontend Setup (React / Vite)

Open a **new terminal window**:

```bash
# Navigate to the frontend
cd DubWiz/VideoDubbingSystem/Frontend

# Install dependencies
npm install

# Start the development server
npm run dev
```

> ✅ The frontend will be live at **http://localhost:5173**

---

### 4. Using DubWiz

1. Open **http://localhost:5173** in your browser
2. Upload an English `.mp4` video
3. Select your target language from the dropdown
4. Click **"Dub Video"** and wait for processing (~2–5 min depending on video length)
5. Download your dubbed video from the output section 🎉

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/upload/<language>` | Upload a video and start the dubbing pipeline |
| `GET` | `/subtitle` | Fetch original English subtitle text |
| `GET` | `/trans_sub` | Fetch translated subtitle text |
| `GET` | `/audio` | Fetch extracted original audio |
| `GET` | `/trans_audio` | Fetch translated dubbed audio |
| `GET` | `/dub` | Download the final dubbed video |

---

## 💡 Notes & Limitations

- 🖥️ **GPU Acceleration**: The system auto-detects CUDA. If no NVIDIA GPU is found, it falls back to CPU (processing will be slower).
- 📏 **Video Length**: Longer videos take more time. Recommended: under 5 minutes for testing.
- 🗣️ **Voice Sync**: The dubbed voice may not be perfectly lip-synced to the original speaker — true lip-sync requires additional models not included here.
- 🌐 **Languages**: Currently supports 5 Indian languages. More can be added by extending `ML_Model.py`.
- 🪟 **Windows Note**: File paths in the backend use backslashes (`\\`). On Mac/Linux, update paths in `ML_Model.py` and `server.py` to use forward slashes (`/`).

---

## 🛠️ Future Improvements

- [ ] Add lip-sync alignment
- [ ] Expand to more languages (Tamil, Bengali, Malayalam)
- [ ] Support longer videos with chunked processing
- [ ] Add subtitle overlay (burned-in captions) to output video
- [ ] Deploy to cloud (AWS/GCP) for scalable processing
- [ ] Add user authentication and video history

---

## 👨‍💻 Author

**Laveen Chordia**

[![GitHub](https://img.shields.io/badge/GitHub-LaveenChordia-181717?style=flat-square&logo=github)](https://github.com/LaveenChordia)

---

## 📄 License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

---

<div align="center">
  <i>Built with ❤️ using OpenAI Whisper, Facebook MBart, and Silero TTS</i>
</div>
