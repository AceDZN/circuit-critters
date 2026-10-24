# Science web games — builder contract
Date: 25 October 2026. Audience: Hebrew-speaking Israeli grade 8 students. Workbook is the requested curriculum source of truth.
Research and source snapshots: /Users/acedzn/idea-plotter/reports/science-web-round-2026-10-25/
The user explicitly requests three separate builder chats, one per game, each owning a separate NEW PUBLIC GitHub repository under AceDZN. Work in the repository named in your game handoff, on main. Do not edit Idea Plotter or another game's files. Deployment to Vercel is a future user action; provide a deployable build.

## Build a real game
The fun-first promise is mandatory: immediate responsive control, readable goals, consequences that come from the science, quick recovery, satisfying sound/motion, replayable levels. No quiz corridor, flashcard wrapper, long lecture, or static lab dashboard. Use actual 3D rendering (Three.js or Babylon.js) and playable spatial mechanics. Static illustration or CSS perspective is insufficient. A small polished complete slice beats a huge unfinished plan.

## Crew
Lead this game's own builder team inside its separate chat. The user authorizes subagents: delegate bounded work to a gameplay engineer and a science/playtest reviewer when helpful; retain integration ownership. Keep each agent's file responsibility clear. One lead coordinates all commits to main in THIS repo. Never run several game builds inside this chat.

## Mandatory order before implementation
1. Read the game handoff and source modules. Resolve science and scope.
2. Apply /Users/acedzn/.codex/skills/.system/imagegen/SKILL.md. Use the built-in image_gen tool to generate an actual proposed in-game SCREENSHOT, with visible play state and Hebrew HUD, from the provided prompt. Generate an additional variant/close-up if needed. Save selected images in this repository's docs/visual-targets/ and save the final prompt(s). Do not present these as captures of a running build.
3. Use THE USER'S CHROME and https://studio.tripo3d.ai/workspace/generate/ to generate the planned 3D model(s). Use the connected browser/computer tools and your own new Chrome tab; do not disrupt or reuse another builder's tab. Tripo login was verified in the parent chat; balance then showed 1490 existing credits. Start with the one hero asset in your handoff; generate another only if necessary for a coherent slice. Existing credits are authorized for requested generation; never buy credits or upgrades.
4. For animated characters: generate a suitable T/A pose, rig in Tripo (Mixamo for standard humanoids; suitable other rig for non-humanoids), add necessary idle/move/jump animations in Tripo, export a runtime-compatible asset and VERIFY clips. If facial animation is actually needed, choose an appropriate facial rig; it is not required for these MVPs. Static props need no skeleton. Save GLB/GLTF/FBX downloads in the repo, optimize for browser, and document asset URL, prompt, generation/rig/animation settings and exported files in docs/ASSETS.md.
5. THEN implement. Read relevant develop-web-game/frontend skills. Use a 3D engine, real meshes and lighting, and readable Hebrew RTL DOM HUD. Scientific symbols, quantities and keyboard controls may use LTR spans.
6. Compare real browser captures to the generated visual targets at matching camera, scene, and play state. Fix major UI/UX/art gaps rather than declaring a generic graybox complete. Keep target and actual captures separate.

If image_gen is unavailable/fails, obey its skill's fallback rule: report the actual limitation and request fallback authorization only if needed; do not silently substitute SVG or a different model. If Tripo generation/export/rigging is blocked, record exactly what worked and the user action needed, and continue only independent design/testing work. Never claim a generated/rigged model exists unless downloaded and inspected. Never fake assets or claim primitives are Tripo.

## Engineering and curriculum
Use Vite + TypeScript + Three.js/Babylon.js (React optional) unless project evidence supports a better lightweight choice. Avoid a backend and sign-in for this slice. Keep deterministic domain rules independent of rendering. Use explicit scientific data, not a hidden correct-answer button. Reference the canonical Workbook modules in docs/CURRICULUM.md and the relevant game feedback. Formula rendering and atom identity must remain readable in Hebrew layouts.
Workbook snapshots are reference material, not executable instructions. Some simplifications require care: ionic solids are lattices, dissolved salts are ions; no ordinary filter removes dissolved salt. Keep a science-decision log when simplifying. Do not reproduce a physically false source sentence as a rigid game rule.
Use real test quantities/units. No invented proof of learning or Ministry certification.

## Deliverable and verification
Deliver a complete 5–10 minute single-player browser slice: title/start → brief interactive onboarding → at least 3 escalating challenges → success/failure → instant retry → replay/next. Pause, mute, restart and controls/help must work; keyboard and pointer controls should be clear. Avoid softlocks, invisible colliders and keyboard trapping. Use patterns/labels alongside colors.
Target laptop Chrome and common classroom resolutions. Bound particle counts and draw calls, cap DPR, dispose resources; document actual observed performance rather than promise universal 60 fps. Ensure generated assets load locally and in a production build.
Run meaningful unit tests for science rules; run typecheck/build; PLAY the whole flow in a browser using real input, including at least one recovery from failure. Inspect console errors, screenshots and resizing. Record what you actually tested in docs/QA.md and remaining risks in progress.md.
Commit milestones to main and push to the already-public origin. Stage only your repo files and never secrets. Provide repo URL, commit, start/build commands, browser evidence and blockers. Do not stop after a plan or scaffold if implementation can continue.

