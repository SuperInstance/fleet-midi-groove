# fleet-midi-groove

_Swing and groove — the feel of time._

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for groove**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → groove (port 2170)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-groove",
  "port": 2170,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | swung | pulse/basic | straight |

## Educational Supplement

Groove is the subtle timing variation that makes music feel "in the pocket."
It's the difference between a metronome and a drummer.

### Straight vs. Swung
- **Straight (-1)**: Eighth notes are exactly even. Machine-like, precise.
- **Swung (+1)**: Eighth notes are uneven — first note longer, second shorter. The
  ratio can be 2:1 (triplet feel), 3:2 (light swing), or anything in between.
- **Pulse (0)**: Bare minimum — just downbeats. No subdivision.

### Swing Ratio
A swing ratio of 55% (55% of the beat on the first half, 45% on second) reads as
"light swing." 66%/33% reads as "hard swing" or "shuffle."

## Fleet Integration

- **Port**: 2170
- **Roles**: tempo
- **Conductor ID**: `groove`
- **Protocol**: HTTP POST to `/2170/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2170
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
