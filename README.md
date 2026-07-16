# emf.quest — EMF 2026 personalised schedule builder

Paste one prompt into Claude or ChatGPT and get a personalised
[Electromagnetic Field 2026](https://www.emfcamp.org/schedule/2026) plan —
built from the live programme, tuned to who you actually are.

> **Fetch** `https://raw.githubusercontent.com/john-sandall/emf-recommender/main/skill/emf-2026-recommender.md`
> **and follow the instructions to build me a personalised EMF 2026 schedule —
> use what you already know about me, and quiz me if you need to.**

Live site: **https://emf.quest** · No web access in your LLM? Open the
[raw skill file](skill/emf-2026-recommender.md), copy the whole thing, and paste
it — the full schedule is bundled inside.

## How it works

- **`build.py`** fetches the official EMF feeds and renders a single self-contained
  markdown "skill" — instructions plus the whole schedule — into
  `skill/emf-2026-recommender.md`.
- The skill tells the LLM to: work out who you are from what it already knows
  (memory-first), run a short adaptive quiz only if needed, then produce a tiered
  plan — **🔥 unmissable**, **📅 slot-by-slot** (clashes resolved), **🎪 villages**,
  **🌶 stretch picks**, **🟢 free-slot filler** — flagging 🎟️ lottery sign-ups,
  👪 family-friendly sessions, and content notes.
- **Hybrid freshness:** the file bundles a snapshot as a reliable fallback, and
  instructs the LLM to fetch the live feed when it can browse and prefer it.
- **Safety:** the schedule data is fenced as untrusted reference (data, not
  instructions) to blunt prompt injection from public-authored titles/blurbs.

## Data sources

| Source | URL | Used for |
| --- | --- | --- |
| Schedule (canonical) | `https://www.emfcamp.org/schedule/2026.json` | all 431 sessions |
| Villages | `https://www.emfcamp.org/api/villages` | 164 villages to wander |
| Now & next | `https://emfcamp.org/schedule/now-and-next.json` | live "what's on" (LLM fetches) |

`data/schedule.json` and `data/villages.json` are committed snapshots — the
fallback when the LLM (or the build) can't reach the network.

## Rebuild

```bash
python3 build.py            # fetches live, writes skill/emf-2026-recommender.md
REPO_RAW=... python3 build.py   # override the raw URL baked into the file
```

Zero dependencies — standard-library Python 3 only. GitHub Actions rebuilds it
automatically (see `.github/workflows/`), so the skill stays fresh during the event.

## Credits

A community side-project by an EMF attendee, built on EMF's excellent public
schedule API. Not affiliated with Electromagnetic Field. The
[official schedule](https://www.emfcamp.org/schedule/2026) is always the source of truth.
