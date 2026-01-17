# dw — Deep Work Logger (CLI)

A frictionless CLI to log deep work sessions by **project**, **mode**, and **output** — with optional **blocker** and **next_step** capture at the end of a session.

---

## What it records (mental model)

Each deep work session produces a single CSV row with:

- **project**: what you’re working on (slug)
- **mode**: what kind of work it was (small fixed set)
- **output** (required): what you produced (one-line artifact)

Plus lightweight quality signals:

- **quality**: subjective quality rating (1–5)
- **distractions**: how distracted you were (0 / 1-2 / 3+)
- **interruption**: whether something externally interrupted the block (none / soft / hard)
- **blocker** (optional): internal stall reason (why progress stopped from the inside)
- **next_step** (optional): resume breadcrumb (smallest true next action)

---

## Install

Copy `dw` somewhere on your PATH (example: `~/.local/bin/dw`) and make it executable:

```bash
chmod +x dw
```

---

## Usage

```bash
dw start [--min N] [-p PROJECT] [-m MODE] [--force]
dw end
dw status
dw cancel [--yes]
dw review --today|--week
```

### Modes

Modes are intentionally few, to keep mental overhead low:

- `build` — shipping artifacts (code, specs, designs, implementations)
- `sell` — outreach, calls, demos, proposals, negotiating, closing
- `discover` — user research, wedge selection, positioning tests, problem validation
- `study` — targeted learning to unblock the next rep (reading, courses, tutorials)
- `debug` — unblocking: reproduce → diagnose → fix
- `ops` — deploy, infra, maintenance, admin ops
- `write` — drafting docs/specs/memos/notes that are the output

> If you pass an invalid mode, `dw` will nudge you by showing the allowed modes.

---

## End prompts explained (what the fields mean)

When you run `dw end`, you’ll be prompted in a consistent order:

### Output (required)

**What it means:** the one-line artifact you produced.  
**How to use:** write a concrete thing, not “worked on X”.

Examples:

- `implemented dw end prompts + csv write`
- `wrote 12 outreach messages`
- `designed schema for bookings + notes`
- `read ch3; wrote summary + 10 flashcards`

### Quality (1–5)

**What it means:** your subjective session quality.

Suggested anchors:

- **1** = basically a miss (couldn’t focus / no meaningful progress)
- **2** = struggled, partial progress
- **3** = solid, got something done
- **4** = strong focus, good progress
- **5** = exceptional, flow / high-leverage progress

### Distractions (0 / 1-2 / 3+)

**What it means:** how often you self-distracted (checking phone, tabs, random tasks).

- **0** = clean block
- **1-2** = a couple slips
- **3+** = frequent distractions

### Interruption (none / soft / hard)

**What it means:** external disruption (not your choice).

- **none** = no external interruption
- **soft** = minor interruption (quick question, brief disruption)
- **hard** = major interruption (call, urgent issue, forced context switch)

If you answer **hard**, the next prompt lets you backdate the session end:

- enter an **end time** (`HH:MM`), or
- enter **minutes to subtract** (`N`) from “now”

This is for the real-world case where you get pulled away hard and don’t run `dw end` until later.

### Blocker (optional)

**What it means:** *internal* stall reason — why progress stopped from the inside.

Use when you hit a real bottleneck like:

- `unclear requirement; need decision on X`
- `missing repro; cannot isolate bug`
- `don’t know next step; too many options`
- `waiting on data/credential; can’t proceed`

Leave blank if the session ended normally.

### Next_step (optional)

**What it means:** a **resume breadcrumb** — the smallest true next action that would let future-you restart fast.

Examples:

- `add unit test for cancel flow; then rerun review`
- `draft 5-line pitch; send to 3 leads`
- `write repro script; capture logs; isolate failing input`

Rule of thumb: if you read `next_step` tomorrow, you should be able to start in <60 seconds.

---

## Data files

- CSV log: `~/.local/share/deepwork/deepwork.csv`
- Active session state: `~/.local/state/deepwork/active_session.json`

The CSV header is strict (clean-break). If the header doesn’t match the current version, `dw` will error and ask you to rename/move the old CSV.

---

## Quick examples

Start a default session (uses last project/mode defaults if present):

```bash
dw start
```

Start a specific project + mode:

```bash
dw start -p caaster -m build --min 120
```

Check status:

```bash
dw status
```

End session (prompts you):

```bash
dw end
```

Review today:

```bash
dw review --today
```

Review last 7 days:

```bash
dw review --week
```
