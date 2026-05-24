# Note Formula — User Manual

The **Formula** tab in the **Note Generator** lets you type notes directly using a compact text notation instead of drawing them in the piano roll. It is ideal for quickly sketching melodic ideas, entering chord progressions, or setting up automation ramps alongside the notes — all in one pass.

---

## Opening the Formula Tab

1. Open a clip in the **Clip Editor**.
2. Click the **Gen** (Generator) button in the toolbar.
3. Click the **Formula** tab (fourth tab, next to Bass Lines).
4. Type your formulas in the text box.
5. Click **Insert into Clip**.

Notes and automation are appended to the clip (they do not erase existing content).

---

## Formula Syntax

Each line describes one note event (or chord) and optional automation ramps.

```
Start.Step-End.Step=Note [param=value ...]
```

| Part | Description |
|---|---|
| `Start.Step` | Start position — bar number, dot, step number (1–16) |
| `End.Step` | End position — the note plays until this position |
| `Note` | MIDI note name or number; use `+` to stack a chord |
| `[param=value]` | Optional automation parameters (space-separated) |

---

## Bar.Step Notation

Positions are written as **Bar.Step**, where bar and step are both **1-based**.

- **Bar** — bar number (1, 2, 3, …)
- **Step** — 1/16th note position within the bar (1–16)

| You write | Meaning | Absolute step |
|---|---|---|
| `1.1` | Bar 1, Step 1 (downbeat) | 0 |
| `1.2` | Bar 1, Step 2 | 1 |
| `1.3` | Bar 1, Step 3 | 2 |
| `1.5` | Bar 1, Step 5 (beat 2) | 4 |
| `1.9` | Bar 1, Step 9 (beat 3) | 8 |
| `1.13` | Bar 1, Step 13 (beat 4) | 12 |
| `2.1` | Bar 2, Step 1 (downbeat) | 16 |
| `3.1` | Bar 3, Step 1 | 32 |

**Time units:**
- 1 step = 1/16th note
- 4 steps = 1 beat (quarter note)
- 16 steps = 1 bar

**Step range:** steps are **1–16** per bar. `1.17` is invalid — use `2.1` instead.

**Beat landmarks** (useful reference for 4/4 time):

| Beat | Step notation |
|---|---|
| Beat 1 | `B.1` |
| Beat 2 | `B.5` |
| Beat 3 | `B.9` |
| Beat 4 | `B.13` |

**Note duration reference:**

| Duration | Steps | How to write | Example |
|---|---|---|---|
| 1/16 note | 1 | `B.N - B.(N+1)` | `1.1-1.2=C4` |
| 1/8 note | 2 | `B.N - B.(N+2)` | `1.1-1.3=C4` |
| Dotted 1/8 | 3 | `B.N - B.(N+3)` | `1.1-1.4=C4` |
| 1/4 note | 4 | `B.N - B.(N+4)` | `1.1-1.5=C4` |
| Dotted 1/4 | 6 | `B.N - B.(N+6)` | `1.1-1.7=C4` |
| 1/2 note | 8 | `B.N - B.(N+8)` | `1.1-1.9=C4` |
| Dotted 1/2 | 12 | `B.N - B.(N+12)` | `1.1-1.13=C4` |
| Whole note (1 bar) | 16 | `B.1 - (B+1).1` | `1.1-2.1=C4` |
| 2 bars | 32 | `B.1 - (B+2).1` | `1.1-3.1=C4` |
| 4 bars | 64 | `B.1 - (B+4).1` | `1.1-5.1=C4` |

**Example — a whole note filling bar 1:**
```
1.1-2.1=C3
```

**Example — a quarter note starting on beat 2 of bar 2:**
```
2.5-2.9=G4
```

**Example — an 8th note on beat 3 of bar 1:**
```
1.9-1.11=E4
```

**Example — a 16th note run across one beat:**
```
1.1-1.2=C4
1.2-1.3=D4
1.3-1.4=E4
1.4-1.5=F4
```

---

## Note Names

Notes can be written as letter names or plain MIDI numbers.

### Letter Names

```
[Letter][Accidental][Octave]
```

| Part | Options |
|---|---|
| Letter | `A` `B` `C` `D` `E` `F` `G` (case-insensitive) |
| Accidental | `#` for sharp, `b` for flat (lowercase b), or omit |
| Octave | Integer; C4 = middle C (MIDI 60) |

**Examples:**

| You write | Note | MIDI |
|---|---|---|
| `C4` | Middle C | 60 |
| `C3` | C one octave below middle C | 48 |
| `D#4` | D sharp, octave 4 | 63 |
| `Eb4` | E flat, octave 4 | 63 |
| `Bb2` | B flat, octave 2 | 46 |
| `G5` | G, octave 5 | 79 |

### MIDI Numbers

You can also write a plain integer (0–127) instead of a note name.

```
1.1-2.1=60
```

**Valid MIDI range:** 0 (`C-1`) to 127 (`G9`). Practical music sits in roughly 24 (`C1`) to 108 (`C8`). Notes outside 0–127 are rejected.

**Octave reference:**

| Note | MIDI |
|---|---|
| C1 | 24 |
| C2 | 36 |
| C3 | 48 |
| C4 (middle C) | 60 |
| C5 | 72 |
| C6 | 84 |

---

## Chords

Stack multiple notes on the same timing by separating them with `+` (no spaces around `+`).

```
1.1-2.1=C3+E3+G3
```

This inserts three notes — C3, E3, and G3 — all starting at bar 1 beat 1 and ending at bar 2 beat 1.

**Examples:**

```
1.1-2.1=C4+E4+G4         C major
1.1-2.1=C4+Eb4+G4        C minor
2.1-3.1=F3+A3+C4+E4      Fmaj7
3.1-4.1=G3+B3+D4+F4      G7
```

---

## Velocity

Control note velocity with `vel=` (0–127, default 100).

```
1.1-2.1=C4 vel=80
```

Velocity applies to **all notes** in a chord on that line.

```
2.1-3.1=G3+B3+D4 vel=110
```

---

## Automation Parameters

Add optional automation ramps to a note line using named parameters. Each parameter creates a linear automation segment covering the same time range as the note.

### Flat Value

Sets a constant automation value across the note's time range.

```
param=VALUE
```

`VALUE` is a percentage from **0** to **100**.

```
1.1-2.1=C4 vol=80
```

### Ramp

Linearly fades an automation parameter from one value to another over the note's duration.

```
param=FROM>TO
```

```
1.1-5.1=C3 mod=0>100
```

### Recognised Parameters

| You write | Automation lane | Value meaning |
|---|---|---|
| `vol` or `volume` | Volume | 0 = silence, 100 = full volume |
| `pan` | Pan | 0 = hard left, 50 = centre, 100 = hard right |
| `pitch` or `pitchbend` | Pitch Bend | 0 = max bend down, 50 = no bend (centre), 100 = max bend up |
| `mod` or `modulation` | Modulation (CC1) | 0 = none, 100 = maximum modulation |

---

## Combining Notes and Automation

Any number of automation parameters can follow the note on the same line, separated by spaces.

```
1.1-3.1=C4 vel=90 mod=0>80 vol=70>100
```

This inserts note C4 (velocity 90) and simultaneously writes:
- A modulation ramp from 0% to 80%
- A volume ramp from 70% to 100%

all spanning bars 1–3 (steps 0–32).

---

## Comments

Any text after `#` on a line is ignored. Use comments to annotate your formulas.

```
# Verse melody
1.1-2.1=C4 vel=80      # root
1.2-2.2=E4 vel=75      # third
2.1-3.1=G4 vel=85      # fifth
```

Blank lines are also ignored.

---

## Error Handling

If a line contains a mistake, a red error message appears below the text box and nothing is inserted. The dialog stays open so you can correct the line and try again.

**Common errors:**

| Error message | Likely cause |
|---|---|
| `expected Start.Step-End.Step=Note format` | Missing `-` or `=`, or they appear in the wrong order |
| `end position must be after start` | The end Bar.Step is earlier than or equal to the start |
| `missing note` | Nothing written after the `=` |
| `note must come before parameters` | A `param=value` token appeared before the note name |
| `unrecognized note 'X'` | The note name is malformed (check letter, accidental, octave) |
| `unknown parameter 'X'` | The parameter name is not one of the four recognised names |

---

## Multiple Lines

Each line is processed independently. Use multiple lines to build a full sequence.

```
1.1-2.1=C4 vel=90          # whole note on beat 1
1.5-2.5=E4 vel=85          # whole note offset by 1 beat (creates overlap/layer)
1.9-2.9=G4 vel=80          # whole note offset by 2 beats
2.1-3.1=F3+A3+C4 vel=95 mod=20>60
3.1-4.1=G3+B3+D4 vel=100
4.1-5.1=C3+E3+G3 vel=90 mod=60>0
```

Formulas are **appended** to the clip — clicking Insert does not erase existing notes.

---

## Complete Examples

### Example 1 — C major scale

**Quarter notes (one note per beat, 2 bars):**
```
1.1-1.5=C4
1.5-1.9=D4
1.9-1.13=E4
1.13-2.1=F4
2.1-2.5=G4
2.5-2.9=A4
2.9-2.13=B4
2.13-3.1=C5
```

**Eighth notes (scale in one bar):**
```
1.1-1.3=C4
1.3-1.5=D4
1.5-1.7=E4
1.7-1.9=F4
1.9-1.11=G4
1.11-1.13=A4
1.13-1.15=B4
1.15-2.1=C5
```

**16th notes (scale in half a bar):**
```
1.1-1.2=C4
1.2-1.3=D4
1.3-1.4=E4
1.4-1.5=F4
1.5-1.6=G4
1.6-1.7=A4
1.7-1.8=B4
1.8-1.9=C5
```

---

### Example 2 — Chord progression with velocity shaping

```
# I - IV - V - I in C major
1.1-2.1=C3+E3+G3 vel=95
2.1-3.1=F3+A3+C4 vel=90
3.1-4.1=G3+B3+D4 vel=100
4.1-5.1=C3+E3+G3 vel=85
```

---

### Example 3 — Melody with modulation swell

```
# Melody with mod wheel opening up
1.1-3.1=C4 vel=80 mod=0>60
3.1-5.1=E4 vel=85 mod=60>100
5.1-7.1=G4 vel=90 mod=100>40
7.1-9.1=C5 vel=95 mod=40>0
```

---

### Example 4 — Bass line with pitch ramp effect

```
1.1-2.1=C2 vel=110 pitch=50>52
2.1-3.1=G2 vel=105 pitch=50>52
3.1-4.1=A2 vel=100 pitch=50>52
4.1-5.1=F2 vel=105 pitch=50>52
```

---

### Example 5 — Chords with volume fade-in

```
# Pad chords fading in over 4 bars
1.1-5.1=C3+E3+G3+B3 vel=70 vol=0>100 mod=20>60
```

---

### Example 6 — Using MIDI numbers and inline comment

```
1.1-2.1=60 vel=100    # C4 by MIDI number
2.1-3.1=64+67 vel=90  # E4 + G4
```

---

## Invalid Syntax — What to Avoid

These are common mistakes that will produce an error and prevent insertion:

| What you wrote | Problem | Correct form |
|---|---|---|
| `1.1-1.1=C4` | Zero duration — start equals end | `1.1-1.2=C4` |
| `2.1-1.13=C4` | End is before start | `1.13-2.1=C4` |
| `1.17-2.1=C4` | Step 17 does not exist; steps are 1–16 | `2.1-3.1=C4` |
| `1.1-2.1=C 4` | Space inside a note name | `1.1-2.1=C4` |
| `1.1-2.1=C4 + E4` | Spaces around `+` in chord | `1.1-2.1=C4+E4` |
| `1.1-2.1=CB4` | Uppercase B used as flat | `1.1-2.1=Cb4` |
| `1.1-2.1=vel=80` | Parameter before note | `1.1-2.1=C4 vel=80` |
| `1.1-2.1=C4 filter=50` | Unknown parameter name | `1.1-2.1=C4 mod=50` |
| `1.1-2.1=C4+E4 vel=80 100` | Value without a `param=` key | `1.1-2.1=C4+E4 vel=80 vol=100` |

---

## LLM Prompt Rules

The following compact rule set is designed to be included in an LLM system prompt so the model can generate valid Note Formula text.

```
NOTE FORMULA RULES — GrooviXBeat

Format (one line per note):
  Start.Step-End.Step=Note [param=value ...]

Positions (Bar.Step):
- Both bar and step are integers, 1-based.
- Steps are 1–16 per bar (1 step = 1/16th note). Step 17+ is invalid — use next bar.
- Absolute offset = (bar-1)*16 + (step-1).
- Beat landmarks: beat 1 = B.1, beat 2 = B.5, beat 3 = B.9, beat 4 = B.13.
- Minimum duration is 1 step (1/16th note).
- End must be strictly after start. Same position = error.

Duration quick-ref (N = step number):
  1/16  = 1 step   → B.N - B.(N+1)   e.g. 1.1-1.2
  1/8   = 2 steps  → B.N - B.(N+2)   e.g. 1.1-1.3
  1/4   = 4 steps  → B.N - B.(N+4)   e.g. 1.1-1.5
  1/2   = 8 steps  → B.N - B.(N+8)   e.g. 1.1-1.9
  whole = 16 steps → B.1 - (B+1).1   e.g. 1.1-2.1

Note names:
- Letter A-G (case-insensitive) + optional accidental + octave integer.
- Sharp: # (e.g. D#4). Flat: lowercase b only (e.g. Bb3, Eb4).
- Uppercase B is always the note B, never a flat.
- C4 = MIDI 60 (middle C). C3=48, C5=72.
- Or use plain MIDI number 0-127.
- Valid practical range: C1 (24) to C7 (84).

Chords:
- Join notes with + and NO spaces: C4+E4+G4

Parameters (all optional, space-separated after the note):
- vel=0-127          note velocity, default 100
- vol=0-100          volume (flat) or vol=FROM>TO (ramp)
- pan=0-100          pan; 50=centre, 0=left, 100=right
- pitch=0-100        pitch bend; 50=centre (no bend), 0=max down, 100=max up
- mod=0-100          modulation; 0=none, 100=maximum

Ramps: param=FROM>TO interpolates linearly over the note's time range.
Comments: # to end of line. Blank lines ignored.

NEVER:
- Use step > 16 (e.g. 1.17 is invalid — write 2.1 instead).
- Write zero-duration notes (start == end).
- Put end before start.
- Put spaces inside note names or around + in chords.
- Use uppercase B as a flat symbol.
- Use param names other than vel, vol, pan, pitch/pitchbend, mod/modulation.
```

---

## Quick Reference

```
# Basic note (bar.step notation; step = 1/16th note, 16 steps per bar)
1.1-2.1=C4                    C4, whole note bar 1

# Note with velocity
1.1-2.1=C4 vel=80

# Chord
1.1-2.1=C4+E4+G4

# Note with automation flat value (0-100)
1.1-2.1=C4 vol=75

# Note with automation ramp
1.1-5.1=C4 mod=0>100

# Multiple automation params
1.1-3.1=G3 vel=90 mod=0>80 pitch=50>55

# MIDI number instead of note name
1.1-2.1=60

# Chord with velocity and modulation ramp
2.1-4.1=F3+A3+C4 vel=95 mod=20>80

# Comment
# this line is ignored

# Flat pan (50 = centre)
1.1-5.1=C4 pan=50
```

---

## Notes and Limitations

- **Bar and step numbers are 1-based.** Bar 1 Step 1 is the very first 1/16th note of the clip. Steps run from 1 to 16 per bar; step 17+ is invalid.
- **The note duration equals** `End − Start` in absolute steps. A duration of zero is not allowed.
- **Automation values are 0–100%** and are clamped to that range. Values outside it are treated as 0 or 100.
- **Inserting automation replaces** any existing breakpoints in the note's step range for that parameter. Breakpoints outside the range are untouched.
- **Inserting notes is always additive.** Existing notes are never removed; Insert appends to whatever is already in the clip.
- **The formula text is saved** between opens of the Generator dialog, so you can refine and re-insert without retyping.
- **Octave convention:** C4 = MIDI 60 (middle C). C3 = MIDI 48, C5 = MIDI 72.
- **Flat accidentals** must be written as lowercase `b` (e.g. `Bb3`, `Eb4`). Uppercase `B` is always the note B.
