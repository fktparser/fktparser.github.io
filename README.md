# FKT — Write Workouts Like a Human

**FKT** (*Format for Kinetic Transcripts*)is a plain-text format for structured training sessions and plans. Describe a workout the way you'd write it on a whiteboard, and let tooling push it to Garmin, Coros, TrainingPeaks, or anywhere else.

No apps. No drag-and-drop. No clicking through ten screens to build an interval set.

```fkt
8x400m Intervals
  warmup 10min @zone2
  8x
    400m @zone5
    200m @zone1
  cooldown 10min @zone2
```

---

## Who It's For

**Coaches** — Write a full week's training plan in minutes. Change one pace definition and every workout updates. Version-control your athletes' plans with Git.

**Serious athletes** — Describe your workouts in a format that matches how you already think about them. Build a personal library of sessions and plans you can search and reuse.

**Occasional runners** — Write a simple 30-minute run or a weekend long run. No account needed. No app to open.

---

## Examples

### A Typical Tempo Run

Set a target pace, warm up, hit the work, come home:

```fkt
Tempo Run
  15min @zone2
  25min @4:45/km
  10min @zone2
```

Duration formats: `30s`, `10min`, `1h`, `400m`, `5km`. Intensity: zones, pace, heart rate, or a name you define yourself.

---

### Define Your Paces Once

Stop hardcoding numbers in every workout. Define your key paces at the top and reference them anywhere:

```fkt
race pace: 5:10/km
threshold: 4:45/km
easy: zone2

Pre-Race Shakeout
  20min @easy
  4x
    1min @race pace
    2min @easy

Threshold Cruise
  15min @easy
  3x
    8min @threshold
    3min @easy
  10min @easy
```

When you hit a new PR, update `race pace` in one place. Every workout reflects it.

---

### A Full Training Week

Build an entire block in a single file — sessions, days, the lot:

```fkt
race pace: 5:10/km
easy: zone2

plan: 5K Build — Week 3
  days: Tuesday, Thursday, Saturday, Sunday

  Speed Work
    10min @easy
    8x
      400m @zone5
      90s @zone1
    10min @easy

  Tempo Intervals
    10min @easy
    4x
      5min @race pace
      2min @easy
    10min @easy

  Easy Run
    45min @easy

  Long Run
    75min @easy
```

Days cycle through sessions automatically. If you train on Tuesday and Friday, and define 5 sessions, you'll have a plan for 3 weeks..

---

### Multi-Sport: Brick Session

FKT handles transitions and mixed activities. Works for triathletes out of the box:

```fkt
ftp: 240w
run pace: 5:00/km

Olympic Brick
  warmup 15min @zone2
  45min @ftp
  transition
  30min @run pace
  cooldown 5min @zone2
```

Built-in step types — `rest`, `recovery`, `walk`, `transition` — can stand alone or carry a duration and intensity.

---

## The Full Format at a Glance

```fkt
# Definitions (optional, must come first)
race pace: 5:30/km
max hr: 185bpm
easy: zone2

# Standalone session
Easy Day
  45min @easy

# Plan
plan: Build Week
  days: Tuesday, Thursday, Saturday

  Speed Work
    10min @zone2
    6x
      200m @zone5
      200m @zone1
    10min @zone2

  Tempo
    10min @zone2
    20min @race pace
    10min @zone2

  Long Run
    90min @easy
```

| Intensity format | Example           |
|------------------|-------------------|
| Heart rate zone  | `@zone2`          |
| Pace             | `@4:45/km`        |
| Pace range       | `@4:30–5:00/km`   |
| Heart rate       | `@155bpm`         |
| HR range         | `@140–160bpm`     |
| HR percent       | `@85% max hr`     |
| Named            | `@race pace`      |

| Duration format | Example                     |
|-----------------|-----------------------------|
| Seconds         | `30s`, `90sec`              |
| Minutes         | `10min`, `45min`            |
| Hours           | `1h`, `2h`                  |
| MM:SS           | `2:30`                      |
| Distance        | `400m`, `1km`, `5km`        |

---

## Get Started

```bash
pip install fktparser
```

```python
from fktparser import parse

result = parse(open("my_plan.fkt").read())

for session in result.sessions:
    print(session.name, "—", len(session.steps), "steps")

for plan in result.plans:
    print(plan.name, "—", len(plan.sessions), "sessions")
```

Parse a file, get structured Python objects. Plug the output into whatever platform or tool you're building on top of.

---

## Why Not Just Use the App?

Most platforms make you click through a wizard for every single step. Drag reps up and down. Export, re-import, lose your formatting. Switch platforms, start over.

FKT is a text file. It lives in a folder. You can copy it, share it, diff it, store it in Git. One file can hold an entire season of training. A coach can send it as a plain text message.

---

[View on GitHub](https://github.com/fktparser/fktparser) · [Open an issue](https://github.com/fktparser/fktparser/issues) · [MIT License](https://github.com/fktparser/fktparser/blob/main/LICENSE)
