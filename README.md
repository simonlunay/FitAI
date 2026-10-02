# FitAI

**An AI fitness coach that scans your body in the browser with TensorFlow.js pose detection, then uses Gemini to generate a personalized workout and diet plan built around Ohio State's gyms and dining halls.**

Built at a hackathon to take the fear out of starting at the gym, beginning with OSU students.

[Demo](https://www.youtube.com/watch?v=K5WOeHgslAE)
<!-- Replace with a 10 to 15 second GIF: run the body scan, then show the generated workout and meal plan -->

> **No hosted demo.** The app runs on a personal Gemini API key, so there's no public instance. You can run it locally with your own key (see [Run it yourself](#run-it-yourself)).

---

## How it works

1. **Body scan.** A MoveNet pose detection model runs entirely in the browser via TensorFlow.js, reading body keypoints from the user's camera with no server round trip.
2. **Personalization.** Scan results and the user's goals are sent to the Gemini API, which generates a tailored workout and diet plan.
3. **Campus grounding.** Plans pull from a PostgreSQL database of Ohio State gym locations and dining options, so every suggestion is something a student can actually do this week.
4. **Coaching chat.** A built-in chatbot answers fitness and nutrition questions with beginner-friendly guidance.

## Architecture

```
┌────────────────────────────────┐
│  React frontend                │
│  TensorFlow.js + MoveNet       │  ← pose detection runs client-side
└───────────────┬────────────────┘
                │ scan results + goals
┌───────────────▼────────────────┐
│  Node.js backend               │
└───────┬────────────────┬───────┘
        │                │
┌───────▼───────┐  ┌─────▼────────────────────┐
│  Gemini API   │  │  PostgreSQL              │
│  plan + chat  │  │  OSU gyms · dining · users│
└───────────────┘  └──────────────────────────┘
```

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React, React Router, TypeScript, Vite, Tailwind CSS |
| Computer vision | TensorFlow.js with MoveNet pose detection |
| AI | Gemini API for plan generation and chat |
| Backend | Node.js |
| Database | PostgreSQL with OSU gym and dining data |
| Deployment | Docker |

## My role

I built the computer vision body scan, from camera to AI analysis:

- **Live camera pipeline** that streams the user's webcam into a MoveNet pose detection model running fully in the browser with TensorFlow.js
- **Skeleton overlay** that draws detected keypoints and connecting lines on the video in real time, so users can see exactly what the model is tracking
- **Metric extraction** that turns raw keypoint coordinates into body measurements like [e.g. shoulder-to-hip ratio, limb proportions, posture alignment]
- **AI handoff** that packages those metrics into a structured prompt for Gemini, which uses them to personalize the workout and diet plan

## Roadmap

- Expansion beyond Ohio State to other universities and cities
- Dynamic local meal recommendations
- AI-powered form correction and progress tracking
- Achievement system to build consistency

---

## Run it yourself

**Requirements:** Node.js, PostgreSQL, and a Gemini API key.

```bash
git clone https://github.com/simonlunay/FitAI.git
cd FitAI
npm install
```

Create a `.env` in the project root:

```env
GEMINI_API_KEY=your_api_key_here
DATABASE_URL=postgresql://postgres:password@localhost:5432/fitai
```

Then start the dev server:

```bash
npm run dev
```

Open the local URL printed in your terminal.

---

Team project, originally developed at [ChuckyT15/FitAI](https://github.com/ChuckyT15/FitAI). README by [Simon Lunay](https://www.simonlunay.com) · [LinkedIn](https://www.linkedin.com/in/simonlunay)
