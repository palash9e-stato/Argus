# Argus Team README

Argus is a moon mission demo app.

In simple words, a person writes a mission in normal English, and the system:

1. reads the mission
2. turns it into a clear plan
3. creates moon ground for the demo
4. finds a safe place to land
5. checks whether the mission is safe and possible
6. shows the result in a live 3D simulation

This README is written for teammates who are new to the project.

---

## 1. What This Project Is

Argus is a "prompt to simulation" mission tool.

That means:

- the user types a mission in plain language
- the backend understands that mission
- the system checks if it makes sense
- the frontend shows the mission in a visual way
- the rover then follows the planned route in the 3D scene

You can think of Argus as 3 parts working together:

- **MissionTwin** = the brain and checker
- **AreoNET** = the website and 3D moon scene
- **Landing-Spot-Detection-main** = the landing helper code we build on top of

---

## 2. One-Line Team Explanation

If you need to explain Argus very quickly:

> Argus takes a mission written in normal English, checks whether it is safe on lunar terrain, chooses a landing area, and shows the rover carrying out the mission in a 3D simulation.

---

## 3. Main Idea Behind Argus

Large language models are good at understanding text, but they are not enough on their own for real mission planning.

So Argus does not simply "trust the AI".

Instead, it uses a simple chain:

- **Reader**: reads the user's mission
- **Ground Maker**: makes the demo terrain
- **Landing Finder**: finds safe landing spots
- **Checker**: tests whether the mission is safe
- **Fixer**: adjusts the mission if something is wrong
- **Simulator**: shows the final mission in 3D

This is the main story of the project.

---

## 4. What the User Sees

The main page is the mission page in the AreoNET app.

The user can:

- type a mission in plain English
- try ready-made example prompts
- draw a box on the small map to choose a region
- run the mission
- see the mission status step by step
- view the 3D rover simulation
- open the 3D simulation in full screen
- see mission events in a timeline
- replay earlier mission runs
- compare route choices
- inspect landing and terrain images

So the app is not only a form and a result. It is a full demo flow.

---

## 5. Full Flow of the System

Here is the full path from user input to rover action:

### Step 1. User writes a mission

Example:

"Send 1 rover to the Green Peaks landmark. Avoid steep slopes. Return safely."

### Step 2. Backend reads the mission

The backend turns the sentence into a structured plan.

It tries to understand things like:

- how many rovers
- where to go
- slope limit
- distance limit
- battery reserve
- whether return is needed
- whether science is important

### Step 3. Terrain is prepared

For demo use, the backend creates moon-like terrain.

This lets the team run the project even when we do not have real mission imagery.

### Step 4. Landing spots are found

The system checks the terrain image and ranks possible landing spots.

It picks a best spot and also keeps some backup spots.

### Step 5. Mission is checked

The system checks if the mission is reasonable.

Examples:

- Is the slope too high?
- Is the route too long?
- Is the battery reserve too low?
- Is the mission trying to do too much?

### Step 6. Mission is fixed if needed

If the mission fails the checks, the system tries to repair it.

It may reduce risk by changing values such as:

- distance
- slope limit
- route choice
- safety margin

### Step 7. Frontend shows the mission package

The website shows:

- parsed mission result
- landing result
- terrain images
- mission status
- route reason
- replay data
- 3D rover simulation

### Step 8. Rover runs in the 3D scene

After the lander finishes landing, the rover starts automatically.

Important current behavior:

- the rover auto-runs after landing
- it follows the prompt target
- it uses the best route, not only the shortest route
- it avoids real obstacles
- manual driving unlocks only after the rover reaches the target

---

## 6. Current Demo Features

These are the main visible features in the current version.

### Mission input

- plain English prompt box
- example prompts
- altitude slider
- region selection box

### Mission results

- step progress bar
- event timeline
- session replay list
- mission objects panel
- route comparison panel
- terrain and landing analysis
- landing zone list
- final mission status

### 3D simulation

- live moon terrain scene
- satellite landing sequence
- rover auto-deploy after landing
- automatic drive to target
- prompt-based target choice
- battery and state feedback
- manual control only after arrival
- full screen 3D mode

---

## 7. Landmarks Used in the Demo

Right now the demo uses two clear landmarks so it is easier to present.

### Green Peaks

- short name: `GP`
- meaning: green landmark area

### Violet Stones

- short name: `VS`
- meaning: purple landmark area

Why this matters:

- judges can clearly see two separate targets
- teammates can use short prompt names like `GP` and `VS`
- the rover now matches these prompt targets better than before

Example prompts:

- "Send 1 rover to GP"
- "Inspect Violet Stones"
- "Land between GP and VS, then inspect VS first"

---

## 8. Best Route vs Shortest Route

This is one of the most important changes in the project.

Before, a route could look good mainly because it was short.

Now the route is chosen more carefully.

The system tries to choose the **best route**, which means a route that balances:

- power use
- safety
- slope
- obstacle avoidance
- science value

So if a short path is risky, the rover may take a slightly longer path that is safer and smarter.

That is a key point to tell your teammates.

---

## 9. Important Rover Behavior

The rover simulation has some specific rules now.

### Auto start after landing

The rover does not wait for a separate manual launch.

Once the lander reaches the surface and the route is ready, the rover starts automatically.

### Prompt target matching

The rover tries to go to the place named in the prompt.

This includes:

- demo landmarks like `GP` and `VS`
- region selected by the user on the map
- mission intent from the mission text

### Obstacle-aware motion

The rover should not cut through real obstacles in the scene.

The route planner now treats major obstacles more seriously.

### Manual unlock only after arrival

Manual movement is locked during the main mission run.

It unlocks only when the rover reaches the final target.

This helps the demo feel more mission-like and less game-like.

---

## 10. Workspace Map

At the top level of the workspace, these folders matter most:

### `MissionTwin/`

This is the Argus control layer.

It contains:

- backend API
- startup scripts
- project README

### `AreoNET/`

This is the main web app and simulation side.

It contains:

- Next.js frontend
- 3D simulation code
- terrain and scene logic
- other existing AreoNET pages

### `Landing-Spot-Detection-main/`

This is an older landing detection source area used as reference and support for landing logic.

---

## 11. File and Folder Guide

This section is for teammates who need to know where to look.

### `MissionTwin/backend/main.py`

This is the main backend file.

It provides the API routes.

In simple terms, this file is the front door of the backend.

### `MissionTwin/backend/mission_parser.py`

This file reads the user's mission text and turns it into a clear mission plan.

Simple meaning: mission reader.

### `MissionTwin/backend/mission_validator.py`

This file checks whether the mission is safe and possible.

Simple meaning: mission checker.

### `MissionTwin/backend/terrain_generator.py`

This file creates demo moon terrain.

Simple meaning: ground maker.

### `MissionTwin/backend/landing_detector.py`

This file finds safe landing spots.

Simple meaning: landing finder.

### `MissionTwin/backend/lunar_terrain.py`

This file stores moon-related values and terrain rules.

Simple meaning: moon rule book.

### `MissionTwin/start-argus-backend.ps1`

Simple launcher for the backend.

### `MissionTwin/start-argus-frontend.ps1`

Simple launcher for the frontend.

### `AreoNET/AreoNet/app/mission/page.js`

This is the main Argus mission page.

It contains:

- mission input UI
- example prompts
- timeline
- replay list
- route comparison
- landing and terrain panels
- 3D simulation area

If the team asks "where is the main page logic?" this is the answer.

### `AreoNET/AreoNet/components/simulation/MissionSimScene.tsx`

This is the main 3D mission simulation file.

It handles:

- landing sequence
- rover deployment
- route planning
- landmark targeting
- auto-run logic
- manual unlock logic

If the team asks "where does the rover behavior live?" this is the answer.

### `AreoNET/AreoNet/components/simulation/utils/astar.ts`

This is the route planner helper.

Simple meaning: path finder.

---

## 12. Backend API in Plain English

The backend runs on:

- `http://localhost:8001`

Useful routes:

### `GET /`

Basic info about the backend.

### `GET /health`

Simple health check.

### `POST /mission/parse`

Reads a text mission and returns a structured plan.

### `POST /landing/detect`

Checks an image and returns landing spots.

### `GET /landing/synthetic`

Creates demo terrain and landing spots.

### `POST /mission/validate`

Checks if a mission is safe and possible.

### `POST /mission/replan`

Repairs a mission if needed.

### `POST /mission/run`

This is the main all-in-one route.

It does the full flow in one call:

- read mission
- create terrain
- find landing spot
- check mission
- repair if needed
- return full result

For teammates, this is the most important route.

---

## 13. Frontend in Plain English

The frontend runs on:

- `http://localhost:3000/mission`

This page is where most demo activity happens.

The mission page includes:

- prompt box
- sample prompts
- region picker
- run button
- progress tracker
- 3D simulation
- full screen simulation button
- mission timeline
- replay panel
- mission object summary
- route comparison
- terrain analysis
- landing zone analysis

So when someone says "show me the whole product", the mission page is the best place to start.

---

## 14. How to Run the Project

These are the simplest steps.

## Backend

Open PowerShell in the workspace root and run:

```powershell
.\MissionTwin\start-argus-backend.ps1
```

This starts the backend on port `8001`.

## Frontend

Open another PowerShell window and run:

```powershell
.\MissionTwin\start-argus-frontend.ps1
```

This starts the frontend on port `3000`.

## Open the app

Go to:

```text
http://localhost:3000/mission
```

---

## 15. First-Time Setup

If this is the first time on a machine, do this.

### Backend packages

```powershell
cd MissionTwin\backend
pip install -r requirements.txt
```

### Frontend packages

```powershell
cd AreoNET\AreoNet
npm install
```

### Environment file

The backend uses a `.env` file.

If it does not exist, the startup script copies it from `.env.example`.

Important:

- use your own Gemini key
- do not share private keys in commits or screenshots

---

## 16. Simple Demo Walkthrough

If you want to show the project to someone in 2 to 3 minutes, use this flow.

1. Open the mission page.
2. Explain that the user writes a mission in normal English.
3. Pick an example like `GP` or `VS`.
4. Optionally draw a region box.
5. Click `Run Mission`.
6. Show the progress steps.
7. Show the landing analysis.
8. Open the 3D simulation.
9. Explain that the rover starts on its own after landing.
10. Explain that the route is chosen for safety and power, not only distance.
11. Show the event timeline and replay panel.
12. Mention that manual drive unlocks only after the rover reaches the target.

This is usually enough for a teammate or judge to understand the full story.

---

## 17. Good Points to Say in a Team Discussion

If your teammates ask what is special about Argus, these are good short answers.

### "What makes this more than a chatbot?"

Because it does not stop at text.

It turns text into:

- a checked mission
- a landing choice
- a route
- a simulation result

### "Why is this useful?"

Because it helps bridge the gap between a written mission idea and a grounded visual mission plan.

### "What is the strongest demo feature?"

The strongest part is the full chain:

- write mission
- check mission
- show mission
- watch rover act on it

### "What is a recent improvement?"

Recent important improvements include:

- rover auto-start after landing
- better target matching from prompt text
- better route choice based on power and safety
- cleaner demo landmarks
- replay and mission timeline
- full screen 3D simulation

---

## 18. Common Team Questions

### "Where should I edit the mission page UI?"

Go to `AreoNET/AreoNet/app/mission/page.js`.

### "Where should I edit rover behavior?"

Go to `AreoNET/AreoNet/components/simulation/MissionSimScene.tsx`.

### "Where should I edit backend mission logic?"

Start with:

- `MissionTwin/backend/main.py`
- `MissionTwin/backend/mission_parser.py`
- `MissionTwin/backend/mission_validator.py`

### "Which backend route matters most?"

`/mission/run`

### "Can we demo without real moon images?"

Yes.

That is why the synthetic terrain step exists.

---

## 19. Common Problems and Easy Checks

### Frontend opens but mission run fails

Check if backend is running on `8001`.

### Backend starts but parsing fails

Check the Gemini key in `.env`.

### Mission page opens but looks empty or broken

Check if frontend dependencies were installed with `npm install`.

### Rover does not behave as expected

Check:

- the prompt text
- selected region
- landmark name used
- mission simulation logic in `MissionSimScene.tsx`

### A teammate is confused by the folder names

Use this memory trick:

- `MissionTwin` = backend brain
- `AreoNET` = frontend and simulation
- `Landing-Spot-Detection-main` = landing support code

---

## 20. Small Glossary

This glossary avoids heavy terms.

- **Prompt** = the mission text written by the user
- **Parser** = reader
- **Validator** = checker
- **Replanner** = fixer
- **Terrain** = ground
- **Landing spot** = safe place to land
- **Simulation** = visual demo of the mission
- **Route planner** = path chooser
- **API** = backend connection point

---

## 21. Short Summary for New Teammates

If you remember only a few things, remember these:

1. Argus turns plain English missions into a checked moon mission demo.
2. `MissionTwin` is the backend brain.
3. `AreoNET` is the frontend and 3D scene.
4. The main page is `AreoNET/AreoNet/app/mission/page.js`.
5. The main rover logic is `AreoNET/AreoNet/components/simulation/MissionSimScene.tsx`.
6. The most important backend route is `/mission/run`.
7. The rover now auto-runs after landing and unlocks manual control only after reaching the target.
8. The route aims for the best path, not just the shortest path.

---

## 22. Suggested Team Explanation Script

If you want a very simple script for speaking to teammates:

> We built a system called Argus. A user writes a moon mission in plain English. The backend reads that mission, creates demo terrain, finds a landing area, checks whether the mission is safe, fixes it if needed, and sends the final result to the frontend. The frontend then shows the mission in a full 3D rover simulation. The rover automatically moves to the prompt target using a safer and smarter route, and manual driving unlocks only after the mission target is reached.

---

## 23. Final Note

This project is easiest to understand if you treat it as one full story, not as separate files.

The story is:

**mission text -> checked plan -> landing choice -> route -> 3D rover demo**

That is the main idea your teammates should remember.
