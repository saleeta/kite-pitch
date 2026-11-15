# Demo video script (3:00)

Legend for the last column: **REAL** = ran on real hardware or a real service. **SIM** = simulator, fake server or screen recording. **NEVER RUN** = no real-hardware run exists; rehearse first. Sources: HANDOFF.md (first real flights, 14 Nov), bridge/unoq/README.md, ASSUMPTIONS.md, kite-studio/ASSUMPTIONS.md.

Known gap to settle before filming: the repo's `web/glasses.html` (master) has Ball, Auto (shots), Wrist and Result modes. I found no draw-in-the-air mode in it. If the draw mode lives on another branch, merge it; otherwise the "draw" beat is shot on the console air canvas (REAL in a browser, `docs/screens/03-console-drawn-path.png`) and must not be described as glasses-drawn. Likewise the amber hold indicator exists on the console (`web/console.html`, edge obstacle warms) but I found none on the glasses page.

## Timed shot list

| Time | On screen | What the wearer does | Voiceover | Component | Real vs simulated |
|---|---|---|---|---|---|
| 0:00-0:12 | Wide outdoor or hall shot, Aino with glasses, drone on a mat. Title "Kite". | Walks into frame, puts on glasses. | "Small places need good footage and cannot afford a crew. This is Kite." | Glasses, Tello | REAL (filmed). Staged. |
| 0:12-0:30 | Over-the-shoulder, then glasses view (screen recording of the 600x600 page): ring, "Ready". | Index pinch on the big ring: take off. | "One pinch and the drone is up." | `web/glasses.html`, governor, ESP32 bridge or UNO Q | Take-off from glasses: REAL via ESP32 bridge (HANDOFF). Via UNO Q: NEVER RUN. Open issue: Tello auto-landed ~13 s into a pinch flight. |
| 0:30-0:48 | Glasses view: ball moves; drone drifts the same way. Governor line says what it allows. | Press and hold the ball, move, release: drone stops. | "Hold the ball to fly. Let go and it stops." | Glasses page, governor | Pinch-grab flight: REAL. Wrist (Neural Band) axis mapping: NEVER RUN on a real band. |
| 0:48-1:10 | Glasses or console: a trail appears as the ball moves, then a smooth arc, then "Allowed" (or a refusal with its reason). | Pinch and move the ball to draw an arc, release. | "Draw the move in the air. Kite smooths it, and the governor checks it before anything flies." | `kite/planning`, governor | Planner and governor: SIM (tested, 185 tests). Draw on glasses: not in master; NEVER RUN on glasses. Console draw: REAL in browser. |
| 1:10-1:30 | Drone camera feed beside the glasses status line. A box or plant ahead. Status turns amber: "something ahead". Drone hovers. | Keeps the ball pushed forward. | "Something is ahead. The drone holds, and says why. This is judged from the video on the edge, no cloud." | Looming estimator, OBSTACLE rule (0.65), UNO Q video | Estimator: SIM and synthetic benchmark only. Live Tello video through board: NEVER RUN. Hold on real flight: NEVER RUN. |
| 1:30-1:52 | Drone circles a teammate (holding a plant or a sign). Subject box on feed. | Selects "orbit"; teammate stands still. | "Orbit a person: a shot from the library, flown at their position." | `shots/orbit.json`, `kite/subject` | Library orbit shot: flightlog shows it flown (confirm real vs sim). Person-anchored orbit and follow: SIM only, NEVER RUN. |
| 1:52-2:02 | Glasses: Land button. Drone settles. | Thumb-slide to Land, pinch. | "Land. Everything so far ran with no internet." | Governor, bridge | REAL (landing). |
| 2:02-2:14 | Glasses Result view: "Publish", then "Uploading 2 of 3". | Pinch Publish. | "Publish: Kite asks Studio to sign each upload, then sends it to Cloudinary." | `kite/publish.py`, Studio signing | SIM (fake Studio and Cloudinary servers). Live Cloudinary: NEVER RUN. |
| 2:14-2:30 | Studio glasses screens: clips, look Noir, trim, caption on (`glasses-01` to `glasses-05`). | Thumb slide to pick, pinch to apply. | "Edit from the glasses: a named Cloudinary look, trim, a caption." | Studio `/glasses`, named transformations | Screens REAL in fixture mode; renders against live Cloudinary NEVER RUN. |
| 2:30-2:48 | Glasses QR (`glasses-06`), cut to phone camera scanning it, share page playing the clip with the path dot. | Pinch Share; friend scans with phone. | "A QR code on the lens. She scans it and the film plays. The move itself is a file she can fly." | Studio share page | Fixture: REAL. Live phone scan: NEVER RUN. |
| 2:48-3:00 | Title card: three logos area, one sentence. | None. | "Flown, edited and shared from the glasses. What we have not done yet is on the next slide." | - | Say it plainly. |

## Minimum-risk version (simulator and screen recordings only)

Film this version first. It is complete and honest if hardware fails.

1. Glasses page in the Meta Display simulator (Select toggles the ball; `/sim/<code>` link): take-off, ball, land. Label "Display simulator".
2. Console air canvas: draw a move, ghost path with governor verdict, fly it in the simulated room (`make demo`).
3. Obstacle: replay of a synthetic closing sequence from `tools/make_edge_bench.py` with the score crossing 0.65 and the hold line. Label "synthetic footage".
4. Orbit shot in the simulator with the person-shaped obstacle.
5. Kite exports; Studio in fixture mode (`STUDIO_FIXTURES=1`) imports it, Noir look, QR on `/glasses`, phone scan of the fixture share page.
6. On-screen label on every simulated beat: "Simulator". Do not mix real and simulated footage in one shot without a label.

## Pre-flight checklist

- Glasses: charged, Developer Mode on (Meta AI app), `/g/<code>` loads over the tunnel, token set (`KITE_ACCESS_TOKEN`), `/sim/<code>` ready as fallback.
- Tello charged, props checked, flying area approved by the venue, tether and spotter, e-stop (Stop button) reachable.
- UNO Q: USB-C data cable, `adb devices` shows the board, `.\tools\unoq_up.ps1` run, `adb forward --list` shows 9100 and 9101, `.env` set (bridge/unoq/README.md). Back-out: ESP32 bridge on COM24.
- Laptop: venv, `pytest` green, ffmpeg on PATH, `KITE_STUDIO_URL` and `KITE_STUDIO_CODE` set, internet on a different interface than the Tello.
- Studio: `CLOUDINARY_URL` set, `npm run seed-looks -- --apply` done, one real upload and one real render tested (credits: 25 per month, check the dashboard first).
- Phone: can open the share URL; QR scans at lens size (test on the glasses, not a monitor).
- Obstacle: textured object ahead (poster, plant), good light; a blank wall will not trigger it [ASSUMPTIONS.md section 6].
- Recording: screen capture of the glasses view, a second camera on the wearer, spare cards.

## Fallback for each beat

| Beat | If it fails |
|---|---|
| Take-off from glasses | Take off from the console, say so; or use simulator footage. |
| Hold the ball | Arrow-key mode in `/sim/<code>`; wrist is a bonus, never the plan. |
| Draw in the air | Console air canvas; or Auto mode picks a saved shot. Never claim glasses drawing without a take. |
| Obstacle hold | Synthetic benchmark replay plus the governor log line (`flightlog.jsonl` shows the rule name). Do not trigger it by flying at a person. |
| Orbit a person | Orbit library shot in the simulator with the person obstacle. |
| Land | Console `L`; Stop button is always allowed. |
| Publish | Show fixture Studio; or Export for Studio and import by hand, both REAL in fixture mode. |
| Edit | `glasses-01` to `glasses-05` clips from fixture mode. |
| QR and phone | Pre-publish a clip, use its known share URL, film the phone scanning a QR shown on a monitor. |

## Must rehearse before filming (never run on real hardware)

UNO Q gateway with the Tello; video through the board; looming estimator on real Tello video; amber hold in flight; orbit around a person; follow; drawing on the glasses (and on the real band); publish to live Cloudinary; named looks rendered on live Cloudinary; QR scanned by a phone from the glasses. Rehearse each three times; film only what works three times.
