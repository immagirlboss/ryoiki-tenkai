# 領域展開 Ryoiki Tenkai - real-time gesture recognition

> Show your hand. Unleash the domain.

A real-time computer vision system that recognizes hand signs inspired by Jujutsu Kaisen and triggers cinematic WebGL animations. Built end-to-end: data collection -> ML training -> live inference -> browser rendering.

---

## Demo
(need to add gif soon)


---

## Gestures & Animations

| Sign | Trigger | Animation |
|---|---|---|
| Infinite Void (無量空処) | Hook middle finger behind index | Thousands of particles stream from screen edges into a consuming vortex |
| Blue · Red (蒼赫) | Index + middle pinch with thumb, pinky and ring finger out | Two neon orbs orbit each other and fuse with lightning arcs |
| Hollow Purple (虚式「茈」) | Extend thumb, index and middle finger, pinky and ring finger folded | White flash -> expanding shockwave rings -> particle explosion through screen |

---

## Tech Stack

| Component | Technology |
|---|---|
| Hand tracking | MediaPipe (21 landmarks per hand) |
| ML classifier | Scikit-learn (MLP / Random Forest) |
| Backend | Python, Flask, Flask-SocketIO |
| Frontend | Vanilla JS, Canvas 2D, WebGL |
| Real-time comm | WebSocket (Socket.IO) |

---

## Architecture

```
Webcam → MediaPipe → 21 hand landmarks (x,y per joint)
                ↓
        ML Classifier → gesture label + confidence
                ↓
        Flask → Socket.IO → Browser
                ↓
        Canvas 2D / WebGL → cinematic animation
```

---

## How to Run

```bash
# Clone and set up
git clone https://github.com/immagirlboss/ryoiki-tenkai
cd ryoiki-tenkai
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Collect training data (or use my trained model) (50+ samples per sign)
python collect.py

# Train the model (or use my trained model)
python train.py

# Run the app (or use my trained model)
python server.py
```

Open `http://127.0.0.1:5001` — press **1, 2, 3** to test animations without camera.

---

## Model Performance

- 4 gesture classes: `infinite_void`, `hollow_purple_charge`, `hollow_purple_release`, `idle`
- 100% validation accuracy on held-out test set
- Real-time inference at 30fps with <50ms socket latency

---

## Key Technical Decisions

- **Typed arrays (Float32Array/Uint8Array)** for particle system — eliminates GC pauses during animation
- **Majority vote smoothing** over 3-frame buffer — prevents false gesture triggers
- **WebSocket transport only** (no HTTP polling) — minimizes socket latency
- **Canvas 2D over WebGL** — more reliable cross-platform circle rendering for particle system

---

## Author

Akbota Kengeskhan — [LinkedIn](https://linkedin.com/in/akbota-kengeskhan)
