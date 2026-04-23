# VividHead: The ISL Interpreter

VividHead is a full-stack AI system that converts Indian Sign Language (ISL) non-manual head movements into text-level grammatical modifiers.

# 🌊 Project Philosophy: Atmospheric Intelligence

### The Fluid Kinetic Experience
VividHead replaces rigid, traditional interfaces with an **Atmospheric Intelligence** design system. The UI is engineered to be weightless and transparent, ensuring that the technology never obstructs the human connection of sign language. 

By utilizing **Fluid Kinetic** principles, the interface mimics the natural flow of human motion through:

* **Glassmorphic Depth:** Translucent, frosted-glass panels that prioritize content while maintaining a sense of layered depth.
* **Drifting Orbs:** Subtly animated Gaussian blur orbs that create a living, breathing background environment.
* **Inertial Transitions:** Leveraging **Framer Motion**, UI elements respond with natural physics—entering and exiting the screen with a smoothness that reflects the grace of ISL.
* **Luminous Overlays:** Real-time neon mesh visualizations that bridge the gap between biological movement and machine intelligence.
  
### Semantic Bridge Logic
The application acts as a "Semantic Bridge" between raw kinematic data and linguistic meaning. By continuously assessing Euler angles (Pitch, Yaw, Roll) via a PnP geometry solver, it maps dynamic motion thresholds directly to grammatical modifiers (e.g., Affirmative, Negative, Question/Uncertainty). It bridges computer vision metrics with ISL conversational norms without requiring explicit hand gestures.

## Architecture Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend UI
    participant Backend API
    participant Frame Processor
    participant Landmark Extractor
    participant Geometry Solver (PnP)
    participant Motion Analyzer

    User->>Frontend UI: Uploads .mp4 Video
    Frontend UI->>Backend API: POST /analyze (multipart/form-data)
    Backend API->>Frame Processor: Extract frames from video
    Frame Processor->>Landmark Extractor: Process MediaPipe (468 points)
    Landmark Extractor->>Geometry Solver (PnP): Provide 2D-3D point mapping
    Geometry Solver (PnP)->>Motion Analyzer: Yield Euler Angles (Pitch, Yaw, Roll)
    Motion Analyzer->>Backend API: Temporal Sliding Window analysis & thresholds
    Backend API->>Frontend UI: JSON Payload with modifiers & confidence
    Frontend UI->>User: Update UX with floating cards & neon mesh feedback
```

## Folder Structure Map
```text
vividhead/
├── backend/
│   ├── app.py                 # FastAPI application and endpoints
│   ├── evaluate_dataset.py    # Metric evaluation against datasets
│   ├── head_logic.py          # Core PnP Euler angle calculations & thresholds
│   ├── hybrid_model.py        # ML hybrid model logic (optional)
│   ├── requirements.txt       # Python dependencies
│   ├── train_hybrid.py        # Script to train hybrid classifier
│   └── Dockerfile             # Container configuration for Hugging Face Spaces
├── frontend/
│   ├── app/                   # Next.js 14 App Router layout & pages
│   ├── components/            # Reusable UI components (VividLogo, UI cards)
│   ├── logic/                 # Frontend state and API interaction
│   ├── public/                # Static assets
│   ├── next-env.d.ts          # TypeScript environment declaration
│   ├── next.config.mjs        # Next.js configuration
│   ├── package.json           # Node.js dependencies
│   ├── postcss.config.mjs     # PostCSS styling config
│   └── tailwind.config.ts     # Tailwind CSS configuration framework
├── dataset/                   # Local categorized ISL phrase datasets
├── README.md                  # Global documentation
└── .gitignore                 # Git ignore patterns
```

## Quick Start

### 1) Backend setup
```bash
cd backend
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
# source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
uvicorn app:app --reload --host 0.0.0.0 --port 7860
```

### 2) Frontend setup
```bash
cd frontend
npm install --legacy-peer-deps
cp .env.example .env.local
npm run dev
```
## 🚀 Run with Docker

```bash
docker pull 01mayank/vividhead-backend
docker run -p 7860:7860 01mayank/vividhead-backend

Open `http://localhost:3000` and upload an `.mp4` to test the full pipeline.
```
## Deployment Targets
- Backend Space: <https://huggingface.co/spaces/01mayankk/computervision-backend>
- Frontend Platform: Vercel (Edge Runtime where possible)
