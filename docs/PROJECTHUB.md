# Arduino Project Hub draft (paste and edit)

**Tag to add: `Junction Finland 2026`.** Publish on the Project Hub and on the Junction platform.

## Title

Kite: an edge brain for a camera drone, flown and edited from smart glasses

## Tagline

An Arduino UNO Q rides beside a drone: it relays control and video, so the laptop keeps its internet, and it is where the drone's obstacle awareness is meant to live, locally, with no cloud.

## Story

Small local projects (restoration sites, farms, community groups, local shops) need footage of their place and rarely have money for a crew, vehicles or a helicopter. Kite lets one person fly a light drone and make the film from Meta Ray-Ban Display glasses: draw a camera move, fly it, publish to Cloudinary, edit and share by QR code.

The part for this challenge is the edge. A Tello makes its own Wi-Fi network, and a laptop that joins it loses the internet. We put the UNO Q on that network instead. The laptop talks to the board over USB (adb), the board talks to the drone, and the video comes back through the board. On top of that video, a small estimator watches for things growing in the middle of the frame (looming, a cue for time-to-contact) and tells a safety governor to hold forward motion. It needs numpy and no network.

Green framing, stated honestly: this is a framing, not a measured saving. One drone, a reusable library of repeatable camera moves and local processing let small nature and community projects show how a place changes over seasons without a crew, and without sending video of people to a cloud. We have not measured energy, travel or emissions and make no such claim.

## How the UNO Q is used

**Linux side (MPU, Debian 13):** runs `bridge/unoq/gateway.py`, standard library Python only. It joins the drone's Wi-Fi with `nmcli`, relays the drone control protocol on a local TCP port, forwards the Tello's H.264 video on a second port, and enforces a command whitelist, rc clamps and a dead-man watchdog (hover after 1.5 s, land after 6 s without the laptop). Both ports bind to 127.0.0.1 and reach the laptop through `adb forward`. Status today: written and tested with fakes; **not yet run on the board against a drone**.

**Obstacle estimator:** numpy only, on JPEG frames at most twice a second. Today it is wired into Kite's server on the laptop; `tools/bench_edge.py` is written to run the same code on the board in a container and report speed and memory. Say "runs on the board" only after that run. Synthetic benchmark (`tools/make_edge_bench.py`) covers approaches at several times-to-contact, static, sideways, retreat, off-centre and blank wall; results are not yet recorded in the repo.

**STM32 side (MCU):** honestly, nothing of ours runs on it yet beyond the board's own services. Quick additions that fit the story and that Modulino parts from the Hardware Lab (5th floor) make easy:
- Modulino Buttons: a physical emergency stop that sends stop through the same governor path.
- Modulino Pixels: the amber light for "holding, something ahead", visible to bystanders.
- Modulino Buzzer: a short alarm on hold or low battery.
If we add any of these, move it from "idea" to "done" in this text only after it has been tested.

## Architecture

See the diagram in `ARCHITECTURE.md`. In short: glasses to Kite on the laptop, through the safety governor, to the UNO Q gateway, to the Tello; video back through the board; estimator score into the governor; after landing, publish to Cloudinary through Studio's signing endpoint.

## Bill of materials

| Part | Notes |
|---|---|
| Arduino UNO Q | Gateway; USB-C data cable to the laptop |
| DJI Tello (or RMTT) | Camera drone, open Wi-Fi access point |
| Laptop | Runs Kite; stays on the internet |
| Meta Ray-Ban Display and Neural Band | Control and editing surface (web app) |
| Optional: Modulino Buttons, Pixels, Buzzer | Not built yet |
| Backup: ESP32 bridge | Control only, used for the first real flights |

Costs: add real prices if you want them; we did not list any.

## Build steps

1. Plug the UNO Q into the laptop with a USB-C data cable; `adb devices`.
2. `.\tools\unoq_up.ps1` pushes and starts the gateway and forwards ports 9100 and 9101.
3. Set `KITE_DRONE=bridge`, `KITE_BRIDGE_PORT=socket://127.0.0.1:9100`, `KITE_VIDEO_URL=tcp://127.0.0.1:9101`.
4. Switch the Tello on, start `python -m kite.server`, open the console or the glasses link.
5. After flying: `.\tools\unoq_up.ps1 -Restore` returns the board to the venue Wi-Fi.

## Code

- Kite: <REPO URL>
- Gateway: <REPO URL>/bridge/unoq
- Studio: <STUDIO REPO URL>
- Demo video: <VIDEO URL>

## What we learned

- One radio means the board leaves the venue Wi-Fi while it flies; plan for adb-only access.
- On a Tello, rc velocity barely moves the drone at small values, so bounded relative moves flew better [HANDOFF.md].
- Looming is a cheap, local cue but blind to blank walls, glass, thin objects and off-centre things; it is an assist that holds forward motion, not autonomous avoidance [ASSUMPTIONS.md].
- Hardware bugs found on day one (Wi-Fi join timing, watchdog landing a drone that was simply slow to reply) each got a test.

## Limits

Nothing here is autonomous avoidance. Person follow and drawing-on-glasses are built and simulator-tested, not flown. No latency, range or accuracy figures are claimed.

Tag: **Junction Finland 2026**
