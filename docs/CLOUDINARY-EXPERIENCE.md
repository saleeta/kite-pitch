# Cloudinary: tell us about your experience (draft, about 310 words)

Before submitting: replace the bracketed sentence with what the first live run actually showed. As of writing, Studio has never been run against a live account (kite-studio/ASSUMPTIONS.md), so this text must not claim more.

---

We built Kite Studio from the Next.js Starter Kit with the AI Skills Pack installed, and used Cloudinary as the second half of a drone workflow: the drone records offline, then Cloudinary stores, grades, trims, captions and shares the footage, with the whole flow driven from smart glasses.

What we liked. Named transformations suit our idea: a shot carries a look by name (kite-noir, kite-spotlight, kite-steady), so the same name travels from the flight file to the share page. The starter kit's signed-upload pattern let the browser, and later the glasses' host, send a 100 MB video straight to Cloudinary while the secret stays in Studio. Deterministic public IDs made a repeat publish cost nothing. The documentation's .md pages and llms.txt were easy for our tooling to read.

Friction. Signing is strict: the signature covers every parameter except the file and API key, so any mismatch between what we sign and what the client posts fails, and a signature is also permission to spend credits, so we had to build a deny-list (no eager transformations or add-ons) and a rate limit around it. Text overlays need the whole text segment double-encoded, which we pinned with character-exact tests because a wrong encoding looks like a broken video. Contextual metadata is capped at 255 characters, so flight paths live in a small stored manifest instead. Credit accounting shaped the design: one upload is one transformation, a derived video costs two per output second, so we preview trims and captions in the browser and render only on a click, with an estimate and a 60-second cap. Some behaviours we could only assume from the docs, such as a look inside a splice layer and the cost of e_deshake. [Live-run findings go here.]

Feature request. A way to ask "what would this URL cost?" before it is generated, for example a dry-run or estimate parameter that returns the transformation count. For apps that hand out signed URLs, especially on a free plan, that would replace our hand-written estimator and make cost-safe sharing a first-class feature.
