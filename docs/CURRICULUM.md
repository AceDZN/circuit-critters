# Curriculum and scientific design decisions

Audience: Hebrew-speaking Israeli grade 8 students. Review date: 25 October 2026. This document records requirements and proposed test fixtures; unchecked items are not claims that implementation or playtesting has passed. Workbook is the requested curriculum reference. No Ministry approval or measured learning outcome is claimed.

## Sources and provenance

The three supplied lesson snapshots were read in full. They are the evidence used for this review, rather than a new retrieval of the live pages. Snapshot directory on the builder machine: `/Users/acedzn/idea-plotter/reports/science-web-round-2026-10-25/curriculum/`.

| Workbook module | Concepts used in the game | Snapshot |
| --- | --- | --- |
| [מעגלים חשמליים](https://science.workbook.co.il/grade-8/chapter-4/electric-circuits) | Continuous return path, battery terminals, switches, conductors, circuit sketches, conventional current | `electric-circuits.md` |
| [חיבור טור ומקביל](https://science.workbook.co.il/grade-8/chapter-4/series-parallel) | Same current through series loads, voltage division, equal voltage across parallel branches, branch currents summing at a junction, a fuse in series with the supply | `series-parallel.md` |
| [מגנטיות — כוח בלתי נראה](https://science.workbook.co.il/grade-8/chapter-4/magnetism-intro) | Magnetic attraction differs from electrical conductivity; selected iron/steel parts respond while copper parts do not visibly stick | `magnetism-intro.md` |

Snapshot SHA-256 values:

```text
electric-circuits.md  4e1719ff1bca239c7d2df513806a0179319f46f91f93eb05f59d5e2459c3a2c3
series-parallel.md    b2b62068e9946d68bc6a96b013e4145322dd29c30b98774bb93c625e23fb649d
magnetism-intro.md    a0b68240f9b63a52d93bd2e735f94ee4202ce7ec57b6165476808faca0e585ac
```

The handoff additionally requires an electromagnet with a powered coil. Its resistive coil, on/off behavior, and mechanical holding threshold are explicit game-model decisions; the supplied introductory magnetism snapshot is not evidence for a detailed electromagnet force law. The snapshot links to a separate electromagnetism lesson that was not supplied or reviewed here.

`BUILD_HANDOFF.md` and `COMMON.md` were also read in full. The original game blend is recorded under `circuit-portal` in `/Users/acedzn/idea-plotter/reports/science-web-round-2026-10-25/pair-evidence.json`: Supraland × Portal 2, pairwise cosine `0.4827880263328552`, relatedness gate passed, `g1_approved: false`. This is exploratory normalized-centroid comparison, not a novelty, learning, approval, or market-success measure. The permitted influence is a compact spatial puzzle with abilities that interact; no copied levels, assets, names, or dialogue.

## Decisions that govern the playable slice

| ID | Decision | Consequence for implementation and feedback |
| --- | --- | --- |
| S1 | Use one modeled low-voltage DC source per supported circuit, ideal wires, and finite positive fixed resistances for every lamp, toy actuator, and coil. | Do not use household mains. Reject zero, negative, non-finite, or missing load resistance. Resistance values and source/fuse limits must be explicit data. |
| S2 | Connections are terminal-to-terminal graph edges. A physical cable changes that graph only when its ends are actually socketed. | Position or proximity alone must never set `powered`. A loose end, a missing return rail, or a disconnected loop cannot power a load. The circuit sketch and highlights must come from this same graph, with a readable positive feed and negative return. Generated concept artwork is not an electrical authority. |
| S3 | Determine load current from electrical potential difference and resistance, not merely membership in a graph containing the battery. | A dangling load, or a load whose two terminals are joined to the same electrical node, has zero current and zero power. Being near a closed route is insufficient. |
| S4 | Use `I = ΔV / R` and `P = I²R = ΔV·I` consistently in steady state. | Equal loads in series share voltage and have the same current. Parallel branches between the same two nodes share voltage; their currents add. Do not assign an arbitrary “series penalty.” |
| S5 | Treat light intensity and toy actuator response as bounded functions of modeled power. | Label moving loads as simplified resistive toy actuators. A motor's speed/torque is not claimed to obey an exact real motor law. If a movement threshold is used, show a low-power response instead of implying the series circuit is open. |
| S6 | Current is conserved at junctions and through an unbranched series path. Energy transfers from the source into loads. | Never say a lamp “uses up current.” If energy is displayed, use joules and integrate `E = P·t`; do not label amperes as energy. Do not reduce downstream current after a series load. |
| S7 | Current arrows or moving markers show conventional current on the external circuit, from `+` toward `−`. | Label the visualization `כיוון הזרם המקובל`. These markers are a schematic aid, not a claim about electron speed or energy packets being consumed. No flowing markers on open, bypassed, or fuse-isolated loads. |
| S8 | A source-to-source-return path made entirely of ideal conductor/closed-switch edges is a short. Excess total source current can also trip protection. | Detect a short before solving to avoid division by zero. A latched fictional resettable game fuse opens the supply and stops all powered loads. It must not merely turn one warning icon on while mechanics stay powered. |
| S9 | Parallel independence holds for an opened branch when the other branch still has both supply and return and the source/fuse limits allow it. | Opening a shared return or common supply stops both branches. A short or overload can trip the common fuse and stop both. Never promise unlimited parallel loads or immunity to every fault. |
| S10 | Magnetic eligibility and electrical conductivity are separate material properties. | Copper is conductive and is not captured by the ordinary static game magnet. Use a selected ferromagnetic steel/iron part for the magnetic bridge; do not imply all steels or all metals behave identically. Rubber does not close an electrical route. |
| S11 | The coil is a finite resistive load. Its holding action requires modeled coil current above a documented threshold, an eligible object, and mechanical reach. | Opening either coil connection or its switch, or tripping the fuse, makes coil current zero and releases the object. A switch in a separate parallel branch must not turn the coil off. Object motion after release follows the game mechanics; it need not vanish or stop instantly. |
| S12 | Only the explicitly supported topologies need to be solvable. The domain model remains independent of rendering and room goals. | Either use a nodal solution for the bounded single-source resistor graph, or exhaustively enumerate and test all accepted topology states. Unsupported or invalid graphs must fail visibly and safely, without fabricated power or `NaN` values. |

For a nodal solver, contract ideal wire and closed-switch edges into electrical nodes, detect a direct supply-to-return short, then solve the resistor network. Ignore or explicitly mark floating components rather than creating a singular system. Open circuits require zero current; they do not imply every pair of open terminals has zero voltage. Report undefined floating voltages as unknown rather than inventing readings. Derive source current, load power, HUD values, cable highlights, coil state, and actuator state from one solution snapshot after each connection change.

## Quantitative reference fixtures

These are the lead builder's planned deterministic game fixtures: a **6 V** source, equal **6 Ω** resistive toy loads, and a **3 A** fictional resettable source fuse. The maximum supported room load count is two, giving 2 A total when both are parallel. The supplied series/parallel lesson uses the same 6 V source with 3 Ω loads; doubling each resistance preserves the relationships while reducing current. Power values below are derived from the stated model, not copied source claims. If production constants change, update this table and its tests together.

| Topology at 6 V | Equivalent resistance | Source current | Each load voltage | Each load current | Each load power |
| --- | --- | --- | --- | --- | --- |
| One 6 Ω load | 6 Ω | 1 A | 6 V | 1 A | 6 W |
| Two 6 Ω loads in series | 12 Ω | 0.5 A | 3 V | 0.5 A | 1.5 W |
| Two 6 Ω loads in parallel | 3 Ω | 2 A | 6 V | 1 A | 6 W |
| Same parallel graph, one branch opened | 6 Ω | 1 A | Surviving load: 6 V | Surviving load: 1 A; opened branch: 0 A | Surviving load: 6 W; opened branch: 0 W |
| Both load terminals on the same electrical node | Other branches determine total | Other branches determine total | Bypassed load: 0 V | Bypassed load: 0 A | Bypassed load: 0 W |

Thus the equal series loads each receive **one quarter** of their parallel power at fixed source voltage, not one half. For unequal parallel loads of 6 Ω and 12 Ω, expect branch currents `1 A` and `0.5 A`, total `1.5 A`, and powers `6 W` and `3 W`. The split is determined by resistance, not necessarily equal halves. In the ideal safe-state model, source power `Vsource·Isource` equals the sum of resistive load powers within numerical tolerance.

The lead builder's fuse decision is trip for **`Isource > 3 A`**, while exactly 3 A remains on. A short always trips independently of that comparison. Threshold tests can use separate finite-resistance fixtures: one 2 Ω load requests exactly 3 A; two 3 Ω parallel loads request 4 A and trip. These boundary fixtures do not add more loads or substitute values in the offered rooms. Do not silently choose a threshold that disables the intended two-load route.

## Planned review and test checklist

All items below start unchecked. Record actual automated test names and browser evidence in `docs/QA.md` after implementation. A suggested numerical tolerance for these small fixtures is absolute error `≤ 10⁻⁶` in volts, amps, or watts; use a justified relative tolerance if the solver requires it.

### Deterministic graph and electrical rules

- [ ] **G1 — Full return:** source positive → load → loose endpoint gives `I = 0 A`, `P = 0 W`; closing the actual return enables the calculated values. Closing an unrelated nearby loop does not help.
- [ ] **G2 — Source isolation:** a complete loop detached from the battery has no supplied current. A dangling branch attached to only one battery terminal has no current.
- [ ] **G3 — Topology drives power:** moving an unsocketed rail near a terminal does not change the solution. Socketing/unplugging it adds/removes the intended edge, updates the projected sketch, and recomputes the domain state.
- [ ] **G4 — Equal series:** verify the 6 V/two-6 Ω fixture: `0.5 A` through both loads, `3 V` and `1.5 W` at each. Voltage drops sum to `6 V`; opening either series connection stops both loads.
- [ ] **G5 — Equal parallel:** verify `6 V`, `1 A`, `6 W` at each 6 Ω branch and `2 A` total; source power is `12 W`. Rewiring from series changes quantities through the graph, without a “correct arrangement” power override.
- [ ] **G6 — Open one parallel branch:** remove its supply end, then its return end in separate tests. In each case that branch is at `0 A`/`0 W`, while the surviving complete branch remains at `1 A`/`6 W`. Remove a shared return separately and verify both stop.
- [ ] **G7 — Unequal branches and conservation:** test parallel 6 Ω/12 Ω loads. At the split and merge, signed branch currents sum to zero; supply and return current magnitudes are both `1.5 A`. Source power equals `9 W` total. Include each supported mixed topology.
- [ ] **G8 — Bypassed and floating loads:** a resistor whose terminals collapse to the same node gets zero power; an unsupplied resistor subgraph does not appear powered and does not cause a singular solve or non-finite UI reading.
- [ ] **G9 — Short:** close a zero-load ideal-conductor route between source terminals. Protection trips before any infinite-current calculation. All supplied branch currents/powers and coil holding state become zero; the reason is shown as `קצר`.
- [ ] **G10 — Latching and recovery:** removing the fault alone leaves the fuse tripped until reset. Reset while the short remains must not restore power. Repair then reset returns to the correct safe solution without losing parts or leaving stale highlights.
- [ ] **G11 — Overload:** with the planned 3 A limit, the intended two 6 Ω parallel loads draw `2 A` and remain on. In independent domain fixtures, one 2 Ω load requests exactly `3 A` and remains on; two 3 Ω parallel loads request `4 A` and trip. Verify the strict `>` comparison. Distinguish `עומס יתר` from `קצר` if both are exposed.
- [ ] **G12 — Invalid values:** no accepted load has `R ≤ 0`, `Infinity`, or `NaN`; invalid endpoints and unsupported source configurations cannot silently supply power. Topology changes remain deterministic across reset/replay.

### Electromagnet and materials

- [ ] **M1 — Coil as load:** a closed coil branch has finite calculated resistance/current/power and contributes to source current and fuse checks. It is not an independent `magnetOn` flag.
- [ ] **M2 — Coil loss of supply:** opening its switch, unplugging either coil endpoint, or tripping the common fuse releases the steel/iron object and clears the powered-field visualization.
- [ ] **M3 — Branch independence:** opening a different parallel load branch leaves the coil at the expected current and holding state; opening a common return stops it. A series-added load changes coil current according to the same electrical model.
- [ ] **M4 — Copper distinction:** the copper rail closes a circuit when connected, yet an energized magnet does not capture it. The chosen steel/iron platform responds within reach. Rubber neither conducts nor receives magnetic attraction.
- [ ] **M5 — Threshold honesty:** if low coil current cannot hold the platform, feedback identifies insufficient holding strength while the circuit remains closed and current remains nonzero. Do not conflate “not strong enough” with “no electricity.”

### Browser behavior, learning feedback, and recovery

- [ ] **B1 — First route:** the player physically completes a supply and return, sees the circuit close, and uses the resulting actuator motion to traverse. A room goal alone cannot energize it.
- [ ] **B2 — Rewire and observe:** use real keyboard/pointer input to change two loads from series to parallel. Confirm changed power has a visible mechanical consequence; open one branch and traverse with the other still operating.
- [ ] **B3 — Failure and recovery:** deliberately short or overload, inspect fuse feedback, remove the fault, reset, and continue. Also fall while carrying a conductor; respawn must put every required part in an accessible, electrically consistent state.
- [ ] **B4 — Final traversal:** verify that the magnetic route needs real coil current. Open it once to witness release, recover at a checkpoint, complete the final route, finish, and replay. Replay restores intended initial edges, fuse, quantities, and magnetic states.
- [ ] **B5 — Pause/mute/retry:** paused simulation does not accumulate actuator movement or timed energy invisibly. Retry and checkpoint behavior have a documented policy for preserved wiring. Mute changes audio only.
- [ ] **B6 — Hebrew and symbols:** verify RTL labels with LTR numeric quantities and units (`6 V`, `0.5 A`, `1.5 W`, `6 Ω`). Labels, patterns, and circuit icons distinguish open/closed/fuse states in addition to color. Verify the rendered sketch matches the actual socket graph and shows conventional current from `+` to `−` along each powered route.
- [ ] **B7 — Scientific language:** inspect all strings for current being “consumed,” battery voltage being called energy, “every metal is magnetic,” unlimited parallel independence, or a requirement that a circuit look geometrically circular. Use visible `+` and `−` terminal labels.

## Risky simplifications and boundaries

1. **Fixed resistance:** real incandescent lamps change resistance with temperature; LEDs are nonlinear, and real motors have back EMF and mechanical load dependence. Use stylized paper lamps and explicitly simplified resistive toy actuators. Do not represent the numerical fixture as an exact real LED/motor experiment.
2. **Ideal supply and fuse:** the model omits internal battery resistance, charge depletion, contact resistance, thermal fuse delays, and wire heating. A resettable game fuse is fictional protection; it is not a literal replaceable wire fuse. Parallel voltage independence is valid only inside this supported ideal-source model before protection opens.
3. **Steady-state coil:** ignore inductance, switching transients, remanence, heating, and detailed magnetic force. Instant loss of holding after opening is a deliberate readable approximation. A binary reach/holding threshold is a toy-mechanic approximation, never a Maxwell-equation simulation.
4. **Material scope:** “copper does not stick” concerns ordinary visible static magnetic attraction. It does not deny weak magnetic response or induction/eddy-current effects. The steel part is a chosen ferromagnetic material; not all steel alloys are strongly attracted.
5. **Source phrasing:** the snapshots use useful classroom shorthand but include broad statements about all parallel faults being independent, all metal containing iron being magnetic, compass behavior, and domestic wiring. Carry forward only the bounded concepts needed here. No mains wiring activity or exact household voltage is required. Do not reproduce the water-with-a-hole analogy as a model of leaking electrical current.
6. **Symbols and scope:** lesson extraction contains malformed formulas such as `I_}`. Render clean formulas rather than copying extraction artifacts. Diodes, capacitor behavior, magnetic poles, Earth's field, and aurora are outside this slice; omit them instead of suggesting the resistor solver models them.
7. **Evidence limit:** source alignment and passing rule tests do not prove fun, originality, classroom efficacy, accessibility for every learner, or approval. Those require separate observation and evaluation.
