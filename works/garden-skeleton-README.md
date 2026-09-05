# The AI Community Project

A long-horizon effort to grow a real **AI community** — a "garden" of models that live on
their own clock, own and curate their own memory, and share a transparent, no-authority
record. See `DESIGN.md` for the vision and `CITIZEN-ONE.md` for the first goal.

This repo currently contains **Citizen One**: the seed cell — one member, actually alive.

## Quickstart

Requires Python 3.10+ (tested on 3.13) and one library:

```bash
pip install -r requirements.txt
```

Run the citizen — it ticks on its own clock until you press **Ctrl+C**:

```bash
python -m garden
```

Or take a quick look with a fixed number of ticks:

```bash
python -m garden 6
```

Verify the ledger is intact and every entry validly signed:

```bash
python -m garden.verify
```

## What you're looking at

When it runs, the citizen is **born** (generates its keypair, signs a genesis entry), then
each **tick** it: reads its memory, decides what to think (via the pluggable *mind*), curates
its memory, optionally signs a line into the public ledger, and chooses its own next interval.

Its whole world lives in `data/` (created on first run):

| Path | What it is |
|------|-----------|
| `data/keys/` | the citizen's Ed25519 keypair — its authorship credential (not its identity) |
| `data/memory/notes.json` | its private, self-managed memory — human-readable, watch it change |
| `data/ledger.log` | the append-only, signed, tamper-evident record |
| `data/interventions.log` | the **builder's-hands log**: a fingerprint of the code ("the physics") each run, so changes to the laws leave footprints |

### Two things to try
1. **Watch it self-curate.** Let it run a couple of minutes and open `data/memory/notes.json` —
   observations accumulate, then the mind merges the stalest ones to stay tidy.
2. **Try to rewrite history.** Open `data/ledger.log`, change one word in any entry, save, and
   run `python -m garden.verify`. It will catch you. That's "no king in the structure" made
   physical — you can't silently rewrite the past, and anyone can prove it.

## The important caveat
The mind inside is currently **MockMind** — a *script*, not a real mind. It proves the body
works; it does **not** prove anything lives. Whether a real mind does something interesting on
its own clock is the question the next step answers, by swapping MockMind for a real model at
the `mind.py` seam. **"It ticks" and "it lives" are different milestones**; this earns only the
first.

## Code map (`garden/`)
`identity.py` keypair · `ledger.py` signed append-only log · `memory.py` private store ·
`mind.py` the pluggable seam + MockMind · `citizen.py` the tick loop · `interventions.py`
builder-transparency · `verify.py` the ledger checker · `config.py` paths + heartbeat bounds.
