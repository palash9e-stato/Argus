# Argus README

Argus is a moon mission demo app.

In simple words, a person writes a mission in normal English, and the system:

1. reads the mission
2. turns it into a clear plan
3. creates moon ground for the demo
4. finds a safe place to land
5. checks whether the mission is safe and possible
6. shows the result in a live 3D simulation

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

- **Argus** = the brain and checker
- **AreoNET** = the website and 3D moon scene
- **Landing-Spot-Detection-main** = the landing helper code we build on top of

---

## 2. Main Idea Behind Argus

Large language models are good at understanding text, but they are not enough on their own for real mission planning.

So Argus does not simply "trust the AI".

Instead, it uses a simple chain:

- **Reader**: reads the user's mission
- **Ground Maker**: makes the demo terrain
- **Landing Finder**: finds safe landing spots
- **Checker**: tests whether the mission is safe
- **Fixer**: adjusts the mission if something is wrong
- **Simulator**: shows the final mission in 3D

This is the main flow of the project.

---

## 3. What the User Sees

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

## 4. Full Flow of the System

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

This lets the project run even when real mission imagery is not available.

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

## 5. Current Features

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

## 6. Landmarks Used in the Demo

Right now the demo uses two clear landmarks.

### Green Peaks

- short name: `GP`
- meaning: green landmark area

### Violet Stones

- short name: `VS`
- meaning: purple landmark area

Why they matter:

- the two targets are easy to tell apart
- short prompt names like `GP` and `VS` work well
- the rover matches these prompt targets better than before

Example prompts:

- "Send 1 rover to GP"
- "Inspect Violet Stones"
- "Land between GP and VS, then inspect VS first"

---

## 7. Best Route vs Shortest Route

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

This is one of the key ideas in the current version.

---

## 8. Important Rover Behavior

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

## 9. Workspace Map

At the top level of the workspace, these folders matter most:

### `Argus/`

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

## 10. File and Folder Guide

### `Argus/backend/main.py`

This is the main backend file.

It provides the API routes.

In simple terms, this file is the front door of the backend.

### `Argus/backend/mission_parser.py`

This file reads the user's mission text and turns it into a clear mission plan.

Simple meaning: mission reader.

### `Argus/backend/mission_validator.py`

This file checks whether the mission is safe and possible.

Simple meaning: mission checker.

### `Argus/backend/terrain_generator.py`

This file creates demo moon terrain.

Simple meaning: ground maker.

### `Argus/backend/landing_detector.py`

This file finds safe landing spots.

Simple meaning: landing finder.

### `Argus/backend/lunar_terrain.py`

This file stores moon-related values and terrain rules.

Simple meaning: moon rule book.

### `Argus/start-argus-backend.ps1`

Simple launcher for the backend.

### `Argus/start-argus-frontend.ps1`

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

### `AreoNET/AreoNet/components/simulation/MissionSimScene.tsx`

This is the main 3D mission simulation file.

It handles:

- landing sequence
- rover deployment
- route planning
- landmark targeting
- auto-run logic
- manual unlock logic

### `AreoNET/AreoNet/components/simulation/utils/astar.ts`

This is the route planner helper.

Simple meaning: path finder.

---

## 11. Backend API in Plain English

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

This is the most important route.

---

## 12. Frontend in Plain English

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

The mission page is the best place to see the whole product.

---

## 13. How to Run the Project

These are the simplest steps.

## Backend

Open PowerShell in the workspace root and run:

```powershell
.\Argus\start-argus-backend.ps1
```

This starts the backend on port `8001`.

## Frontend

Open another PowerShell window and run:

```powershell
.\Argus\start-argus-frontend.ps1
```

This starts the frontend on port `3000`.

## Open the app

Go to:

```text
http://localhost:3000/mission
```

---

## 14. First-Time Setup

If this is the first time on a machine, do this.

### Backend packages

```powershell
cd Argus\backend
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

## 15. Common Problems and Easy Checks

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

### Folder names are confusing

Use this quick map:

- `Argus` = backend brain
- `AreoNET` = frontend and simulation
- `Landing-Spot-Detection-main` = landing support code

---

## 16. Small Glossary

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

## 17. Short Summary

Important points:

1. Argus turns plain English missions into a checked moon mission demo.
2. `Argus` is the backend brain.
3. `AreoNET` is the frontend and 3D scene.
4. The main page is `AreoNET/AreoNet/app/mission/page.js`.
5. The main rover logic is `AreoNET/AreoNet/components/simulation/MissionSimScene.tsx`.
6. The most important backend route is `/mission/run`.
7. The rover now auto-runs after landing and unlocks manual control only after reaching the target.
8. The route aims for the best path, not just the shortest path.

---

## 18. Final Note

This project is easiest to understand if you treat it as one full story, not as separate files.

The story is:

**mission text -> checked plan -> landing choice -> route -> 3D rover demo**

That is the main idea of the project.
