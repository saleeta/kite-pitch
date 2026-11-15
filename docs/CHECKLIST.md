# Checklist: next 24 hours

Owners are roles; the team assigns names. Files go in `H:\junction\kite\_private\story\media\` (create it; private). Naming: `NN-scene-take.ext`, e.g. `03-draw-arc-t2.mp4`. Record 1080p or higher, landscape, 3 takes each, keep the best.

## 12 clips and screenshots

| # | Capture | Exact scene | Duration | File name | Owner |
|---|---|---|---|---|---|
| 1 | Hook, wide | Aino puts on glasses, drone on mat, no talking | 10 s | `01-hook-wide.mp4` | Camera |
| 2 | Take-off from glasses | Over-the-shoulder, then screen recording of `/g/<code>` ring pinch | 15 s | `02-takeoff-glasses.mp4` | Pilot + screen capture |
| 3 | Ball flight | Hold ball, move, release, drone stops; screen and drone together | 20 s | `03-ball-flight.mp4` | Pilot |
| 4 | Draw an arc | Draw, smoothing, governor verdict; one allowed, one refused with reason | 25 s | `04-draw-arc.mp4` | Software |
| 5 | Obstacle hold, amber | Textured poster or plant ahead; amber and "something ahead" on screen; drone hovers | 20 s | `05-obstacle-hold.mp4` | Edge |
| 6 | Orbit a teammate | Teammate stands still holding a plant; orbit shot | 25 s | `06-orbit-person.mp4` | Pilot |
| 7 | Land and Stop | Land button; separately show Stop is always allowed (in sim) | 10 s | `07-land-stop.mp4` | Pilot |
| 8 | Publish | Result view, Publish, "Uploading 2 of 3" | 15 s | `08-publish.mp4` | Software |
| 9 | Board on the desk | Close-up of UNO Q, USB-C cable, gateway log scrolling, Tello on the table; plus the bench run on the board if done | 15 s | `09-board-gateway.mp4` | Edge |
| 10 | Studio edit on glasses | `/glasses`: clip, Noir, trim, caption on | 20 s | `10-studio-edit.mp4` | Cloudinary |
| 11 | QR and phone | Glasses QR, phone scans, share page plays, path dot moves | 20 s | `11-qr-phone.mp4` | Cloudinary |
| 12 | Still screenshots | Studio hold ring, console refusal, architecture, glasses status, Result view at 600 px (see SLIDES.md for repo files) | 6 stills | `12-still-NN.png` | Slides |

## Work split (parallel)

| Track | Tasks | Done when |
|---|---|---|
| Hardware | Run `unoq_up.ps1`; control and video through the board; flight with the board gateway; measure whether the Tello auto-lands (open issue from HANDOFF.md) | A recorded flight through the board, or a decision to demo with the ESP32 bridge |
| Edge | Run `tools/make_edge_bench.py` then `tools/bench_edge.py` on the laptop and on the board; record the output in a file; try a real hold on real video | Numbers written down with the command used |
| Glasses | Confirm where draw-in-the-air lives; add or show the amber hold line on the glasses; rehearse the full pinch flow | A take of clips 2, 3 and 4 |
| Cloudinary | One real upload, one real render of each look, a phone scan; check credits (25 per month, 100 MB per video); deploy Studio | A public share URL that plays on a phone |
| Story | Record voiceover, edit video, build the slides, write the Project Hub page and Junction submission | Drafts out for review at hour 12 |
| Safety | Spotter, props, venue approval, battery check, kill switch rehearsal | Written go or no-go before every flight |

## Risks and fallbacks

| Risk | Fallback |
|---|---|
| UNO Q gateway fails with a real Tello (Wi-Fi join, ath10k, video) | Use the ESP32 bridge for flights, show the board gateway on the desk with logs, and describe the gateway as built, not proven |
| Obstacle estimator misses or false-alarms on real video | Show the synthetic benchmark and the governor rule; say it is being improved |
| Tello auto-lands mid-demo (cause unknown) | Short flights, fresh battery, take what works from three tries; simulator as backup |
| Neural Band wrist control unreliable | Use pinch-and-hold ball; wrist is optional |
| Glasses tunnel or Wi-Fi drops | `/sim/<code>` in the Display simulator; screen recordings |
| Live Cloudinary upload or render fails, or credits are used | Fixture mode footage; keep two spare renders prerendered |
| Draw-in-the-air not in master | Console air canvas; do not claim it was drawn on the glasses |
| Venue rules or weather stop flying | Simulator version in DEMO-SCRIPT.md |

## Claims we must NOT make

- No CO2, energy, travel or cost savings numbers. The green case is a framing, not a measured saving.
- Not "autonomous obstacle avoidance". The OBSTACLE rule only turns forward motion (`vy > 0`) into a hover when the score is at least 0.65 [kite/governor.py]. Nothing steers around anything. Back, sideways, up, down, land and stop are never held.
- Not "runs on the UNO Q" for the estimator until the benchmark has run on the board. Today it is wired into Kite's server on the laptop [kite/server.py].
- The estimator was checked on synthetic footage only (`tools/make_edge_bench.py`; scenarios named by time-to-contact of 17, 9, 5, 3 and 2 seconds, plus static, pan, retreat, off-centre and blank wall). No result is recorded in the repo, so we say: it is being improved, and here is what it cannot see (plain walls, glass, thin objects, off-centre objects, fast approach) [ASSUMPTIONS.md section 6].
- Person follow and orbit around a person are built and simulator-tested, not flown, and not yet wired into the flight loop (`kite/subject/README.md`: "Integration (not done here)"). It is "not a safety-certified follower".
- Drawing in the air on the glasses: not in master as far as I could see; call it built only if the team confirms it. Never claim it flew.
- The UNO Q gateway has not run on the board against a drone [bridge/unoq/README.md]. Say "written and tested with fakes".
- Cloudinary: no live-account run has been recorded; no claim that renders or looks work live until done.
- The glasses were used for about an hour; the Neural Band mapping is unverified [HANDOFF.md]. Do not say "tested with the band".
- No latency, range or accuracy figures; none were measured [HANDOFF.md].
- The MCU side of the UNO Q does nothing of ours today. Modulino parts are ideas until added.
- Repeat flights are similar, not identical: position is dead-reckoned from takeoff, not georeferenced.
- "Real" footage only if flown; label simulator and fixture footage on screen.
- Do not call the Tello's 13 s auto-land a solved problem; its cause is open [HANDOFF.md].
