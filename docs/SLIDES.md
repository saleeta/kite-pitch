# Slides: 10-slide outline

Screenshot paths are relative to `H:\junction\kite` unless marked Studio (`H:\junction\kite-studio`).

| # | Title | Speaker note (max 3 lines) | Visual | Repo asset |
|---|---|---|---|---|
| 1 | Kite: a drone studio you wear | One person, a drone and glasses make a film of a local place. No crew. Everything from the lens. | Photo of the glasses next to the drone; one-line pitch. | Your own photo; fallback `docs/screens/10-glasses-flying.png` |
| 2 | The problem | Small local projects need footage and cannot pay for crews. Piloting, composing and editing live on separate devices. | Aino persona, three device icons crossed out. | Illustration only; mark persona as illustrative. |
| 3 | Draw the move, fly the shot | A shot is a named file of relative moves. The library is the product; flight is playback. You draw it, Kite smooths it. | Console drawn path with its ghost arc. | `docs/screens/03-console-drawn-path.png`, `04-console-flying-the-move.png` |
| 4 | Safe by construction | Every command passes the governor; it refuses with a reason and never clips. A test proves no path bypasses it. | Screenshot of a refusal with its rule. | `docs/screens/05-console-refused.png` |
| 5 | Challenge 1: Meta Ray-Ban Display | 600x600 additive web app: pinch, thumb slide, wrist ball. Tested on the real glasses for about an hour; band mapping not verified. | Glasses status screen and Result view at 600 px. | `docs/screens/10-glasses-flying.png`, `result-view-600.jpg` |
| 6 | Challenge 2: Cloudinary | Studio built from the Next.js Starter Kit. Signed uploads, named looks, trim and caption, publish from the glasses. The share page carries the move. | Studio playing with the path dot; glasses look screen. | Studio: `docs/screens/02-studio-playing.png`, `glasses-03-look-noir.jpg`, `04-shared-page.png` |
| 7 | Challenge 3: Edge AI on the Arduino UNO Q | The board is the drone's gateway, so the laptop keeps the internet. Obstacle judgement from video, locally, holds forward motion. Not autonomous avoidance. | Architecture strip with the board highlighted; amber hold ring. | `bridge/unoq/README.md` diagram; Studio hold ring (fixture); `docs/screens/02-console-flying.png` |
| 8 | Why it is green and local | A framing, not a measured saving: small projects show change in a place over seasons without a crew; the move repeats; processing stays local. | Three short lines, then a "repeat this move" visual. | Studio `docs/screens/04-shared-page.png` plus two clips of the same shot |
| 9 | What works today, what is next | Works: simulator end to end, real Tello flights and glasses use, signed uploads on fakes. Next: UNO Q live, live Cloudinary run, drawing and follow in flight. | Two-column table, green and amber, no hype. | HANDOFF.md lists |
| 10 | Try it, read it, thanks | Repo, Studio, Project Hub page tagged `Junction Finland 2026`, team. One line to close. | QR to the repo and Project Hub. | Generated QR; links to be filled in |

## Slide 9 content (copy exactly, keep it honest)

Works today: simulator flight, governor and refusals (185 tests); real Tello take-off, hover, a sweep shot and landing through the ESP32 bridge; real glasses page for about an hour; Studio in fixture mode (189 tests); publish against fake servers; obstacle estimator on synthetic footage.

Next: UNO Q gateway with a real drone and live video; estimator on real footage and on the board; a live Cloudinary run; drawing on the glasses in flight; person follow and orbit around a person in flight; wrist band verification; Modulino emergency stop.

## Rules for the deck

- Every screenshot from fixture or simulator mode carries a small "simulator" tag.
- No number that is not in a repo file. No CO2 figure anywhere.
- One idea per slide, at most 20 words on a slide besides the table above.
