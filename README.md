# Not Bilbanan

A browser homage to *Bilbanan*, the Swedish slot-car Shockwave game from the early 2000s (once hosted on TV4 / Blip.se).

**Play:** https://www.alecsharpie.me/bilbanan/

- Drag track pieces from the top bar onto the board; drop them on the bin to remove them. Click a placed piece to rotate it.
- Close the loop through the start piece, then click the checkered flag (or press Enter), or tap **Start the race** on a phone.
- **Hold to join:** everyone holds their button; the race starts once nobody joins or lets go for a moment.
- Hold to speed up, let go to brake. Too fast in a curve and you fly off.
- Click the cockatoo to race the computer; set its difficulty with **Nivå** 1–9. After a race, click the board (or tap **Race again** on a phone) for a rematch, or the [x] to edit the track.

## Phones as controllers

Put the game on a laptop or TV. Under the track is a QR code and a 5-letter room code. Each friend scans it (or opens
`pad.html` and types the code) and their phone becomes their car's controller: one big hold-to-go button in the car's
colour, with their lap, time, place and the start lights, and a buzz on a crash (Android). Up to four cars:
phones fill orange, blue and green first and take red from the cockatoo only when a fourth joins. The keys
(**D** red, **K** orange, **Z** blue, **M** green) and the on-screen controllers still work alongside them.
Click the join card to shrink it.

How it fits together: the same pattern as `~/me/finger-frolic`, and the same relay.

- `index.html` is the TV (host) and runs the race. `pad.html` is the phone; it sends `{on}` while its button is held
  (re-sent every 400 ms; the TV lets go of a phone it hasn't heard from in 1.5 s) and `{go}` to start a race.
- `shared/net.js` and `shared/lobby.js` are copied from Finger Frolic. They connect through its relay (`frolic-relay`
  on Cloudflare), which already allows `https://www.alecsharpie.me`. Phones keep their seat across screen locks.
- The TV broadcasts one message with every car's numbers ~8 times a second while racing, otherwise only on a change,
  and nothing while no phone is connected.
- Opened straight from disk (`file://`) the networking is skipped and the game plays as before with keys.

**Dev:** `dev.html` shows the TV and 1–4 phones in one tab over BroadcastChannel (`?net=local`), no relay needed.
Serve the folder first (e.g. `python3 -m http.server`). Served from localhost, the pages look for a local relay on
:8787 (`npm run relay` in finger-frolic); add `?relay=wss://frolic-relay.alecsharpie.workers.dev` to both the
TV and pad URLs to use the live one instead.

No build step.
