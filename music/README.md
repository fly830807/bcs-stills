# BCS music pack (for S5G4+; not yet wired in)
11 mono 64 kbps MP3 loops, 60–75 s each, ~5.7 MB total. Built by mk.py from src/ (originals not for upload).
Loops: 3 s tail→head crossfade baked in; seam jump checked (manifest seam_jump).

## Licence
10 tracks: Kevin MacLeod (incompetech.com), CC BY 4.0 — attribution REQUIRED in game credits:
"<Title>" Kevin MacLeod (incompetech.com), Licensed under Creative Commons: By Attribution 4.0
(per-track lines in manifest.json). bcs_drone.mp3 is original (generated), no licence issue.
No Better Call Saul score used.

## Design note (game side, not built)
- Upload to bcs-stills/music/; one `const MB='…/music/'` string (the build's http count goes to 2, update the asserts).
- iPhone autoplay: nothing plays until a "🎵 開音樂" tap; that tap creates/resumes the AudioContext (or calls audio.play()).
- Two <audio> elements (A/B), loop=true, preload="none", volume ≤0.25; crossfade ~1.5 s via requestAnimationFrame ramp (iOS ignores audio.volume → use WebAudio GainNode for volume/fades; fallback = hard switch).
- Each node gets an optional `mu` (mood key) like `im`; show() calls MUS(mu); same mood = keep playing; no mood = keep current.
- Mute toggle 🔇 in top bar; state in memory + URL hash flag (e.g. `&m=0`), never localStorage; not part of S, so save codes are unchanged (no new fields, v stays, old/new codes import identically).
- Failure (404, offline, decode error, play() rejected) → catch silently, mark that track dead, no UI message, game continues.
- Pause on visibilitychange hidden; resume only if user had music on.
- Credits line on title/end screen for CC BY.
- Loop gap: MP3 encoder padding can give a tiny click at loop; WebAudio decodeAudioData + BufferSource.loop with loopStart/End trims it if needed.
