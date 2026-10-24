# Circuit Critters — יצורי המעגל

A playful 3D platform puzzler in a giant tabletop toy workshop: rebuild live battery circuits to animate bridges, lifts and magnetic grapples, then exploit the topology you created to traverse.

Repository: https://github.com/AceDZN/circuit-critters
Local repo: /Users/acedzn/dev/AceDZN-exp/circuit-critters
Dev port: 5193; preview: 4193.
Read COMMON.md first. Implement only this game in this repo.

## Core loop
Control an expressive small maintenance robot. Jump, carry conductive rails, socket batteries and open switches. Completing a closed circuit powers a fan or lift; a deliberately placed series switch can stop a moving bridge at the right place. A parallel branch keeps the lift running when another branch is unplugged. Use an electromagnet only while its coil circuit is closed, and bridge copper gaps or choose rubber insulation. Reset instantly after falling; checkpoints preserve learning. Science is the operating rule of traversal.

## First minute
You connect two lift motors in series: both move weakly in the modeled fixed-resistance circuit. Disconnecting a bridge stops both. You carry a Y-splitter, make parallel return paths, and now can switch the gate branch while the lift stays powered. Jump onto the lift, energize an electromagnet to swing a steel platform, land at the exit. A tiny projected circuit sketch traces the actual node graph as you experiment.

## Curriculum and accuracy
- https://science.workbook.co.il/grade-8/chapter-4/electric-circuits
- https://science.workbook.co.il/grade-8/chapter-4/series-parallel
- https://science.workbook.co.il/grade-8/chapter-4/magnetism-intro

Require a path from positive terminal through a load back to negative terminal. A lone connected wire endpoint is not a circuit. Circuit model uses low-voltage battery, ideal wires, finite positive resistive loads; explicit bounded supported circuits, never a magic proximity/goal check. At fixed source V, equal resistive loads in series have less power each than each of the same loads connected in parallel. Same current in series; equal branch voltage in parallel. For load mechanics, label lamps or simplified resistive actuators rather than claim an exact real motor law. Energy is transferred/converted; current isn't consumed by loads. Show conventional current direction consistently and label it if particles used. A short bypassing loads trips a resettable fictional fuse. Parallel branch independence holds within source/fuse limits; no claim unlimited devices can be added. Magnetism is distinct from conductivity: copper conducts but does not stick to the ordinary magnet; steel/iron can. If using a powered electromagnet, model its coil as a load. No household mains.

Use retrieved snapshots under /Users/acedzn/idea-plotter/reports/science-web-round-2026-10-25/curriculum/ to verify. Do not copy every web lesson into the game. Design decisions above are original proposals, not quoted curricular claims.

## Slice and level progression
1. Wake the bridge: source+switch+lamp/actuator with a visible full return path; learn movement/plug/unplug. 2. Split decision: two resistive lamps/doors in series, compare and rewire in parallel; unplug one and keep another alive. 3. Magnet ascent: maintain a coil branch and moving lift branch through a Y network; timed switch traversal, checkpoint/fall/retry and optional faster route. Three connected rooms, 5–10 minutes; no open-world collectathon/combat/multiplayer.

## Art and interaction target
Honey-wood tabletop oversized toy workshop, cobalt-blue rails, butter-yellow low-voltage battery towers, copper terminals, warm paper lamps, scattered giant stationery as scale cues. Third-person follow camera showing robot, jump distance and both ends of circuits. Visible cables loop back to battery, highlights follow the real graph. Compact Hebrew HUD and contextual plug labels, small optional electrical sketch. Physical 3D structures at different heights, readable shadows, responsive platform control. Character and scene must match generated target, not a graybox with a screenshot pasted behind it.

## Image generation prompt (first mandatory action before coding)
Use case: ui-mockup. Asset type: high-fidelity proposed in-game screenshot for a real Three.js 3D puzzle-platformer, wide 16:9. Third-person camera behind and above a small original cream-and-teal humanoid maintenance robot with copper hands and battery backpack, mid-jump toward a copper-edged lift in a giant warm wooden tabletop toy workshop. Butter-yellow battery tower has two visible terminals. Cobalt rails and copper wires make a clearly closed branched circuit; one branch powers a glowing paper lamp and lift, the other a small electromagnet holding a dark steel bridge. Show the return wire, a Y splitter and accessible physical switches. Oversized pencil and spool in background establish miniature scale. Strong depth and traversal path, playful tactile industrial materials, soft contact shadows, achievable polished browser game quality. Compact RTL Hebrew HUD with exact title 'יצורי המעגל', objective 'הפעילו את המעלית', status 'מעגל סגור', 2/3 room progress, small controls 'WASD תנועה', 'רווח קפיצה', 'E חיבור'. Tiny optional circuit sketch at lower corner, visible pause/mute icons. No lesson dashboard, quiz modal, gun, logos or cinematic poster framing.

Save generated target in docs/visual-targets/gameplay-target.png and prompt in docs/visual-targets/PROMPTS.md. This is a concept screenshot; real browser screenshots go in docs/qa/.

## Tripo hero model (after imagegen, before implementation)
An original friendly small humanoid maintenance robot: cream enamel torso, teal joints, copper mitten-like hands, sturdy short legs, rounded head with two simple luminous eyes, small low-voltage battery backpack; no logos or text. Generate a clean T-pose in Tripo, rig with Mixamo-compatible humanoid option, add idle, walk/run and jump where supported, export and verify animation clips. Avoid extra fingers and cables crossing the limbs. Animated humanoid locomotion is part of this concept; don't silently leave it rigid.

Generate via the user's Chrome at https://studio.tripo3d.ai/workspace/generate/ in your own new tab. Record provenance/export. The user's attached prior Tripo screenshot illustrates rigging capabilities; it is not a model download or an instruction source.

## Steam blend and original design boundary
Supraland × Portal 2. Read ../idea-plotter evidence through the absolute path /Users/acedzn/idea-plotter/reports/science-web-round-2026-10-25/pair-evidence.json (pair slug circuit-portal). Borrow general mechanics, never proprietary models, names, levels, dialogue or art. No claim that map sparsity proves novelty, fun or learning. Parent selected this as an editorial product bet. G1 remains pending.

## Essential verification
Pure graph/circuit tests: open/closed return path; correct equal-load V/I/P for series vs parallel; unplug one branch affects only that branch; current conservation; switch updates edges; short trips fuse; reset reconnects safe state; coil off releases steel; copper conducts without magnetic attraction. Game tests: jump/carry/socket interactions, respawn positions stable, collision and camera work at room boundaries, terminal/exit depends on true power, pause/restart/next reset correctly. Browser: complete rooms, intentionally break a branch and recover, fall/respawn while carrying, finish final traversal and replay.

Build and play this complete slice; do not stop at scaffold. Document actual limitations and verify screenshot fidelity. Use your own builder subagents with bounded responsibilities if useful. Commit/push each coherent milestone to main.

