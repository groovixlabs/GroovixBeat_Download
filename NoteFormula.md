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
Start.Beat-End.Beat=Note [param=value ...]
```

| Part | Description |
|---|---|
| `Start.Beat` | Start position — bar number, dot, beat number |
| `End.Beat` | End position — the note plays until this position |
| `Note` | MIDI note name or number; use `+` to stack a chord |
| `[param=value]` | Optional automation parameters (space-separated) |

---

## Bar.Beat Notation

Positions are written as **Bar.Beat**, where both bar and beat are **1-based**.

| You write | Meaning | Step number |
|---|---|---|
| `1.1` | Bar 1, Beat 1 | Step 0 |
| `1.2` | Bar 1, Beat 2 | Step 4 |
| `1.3` | Bar 1, Beat 3 | Step 8 |
| `1.4` | Bar 1, Beat 4 | Step 12 |
| `2.1` | Bar 2, Beat 1 | Step 16 |
| `3.1` | Bar 3, Beat 1 | Step 32 |

**Time units:**
- 1 step = 1/16th note
- 4 steps = 1 beat
- 16 steps = 1 bar

**Example — a note filling bar 1:**
```
1.1-2.1=C3
```

**Example — a note on the second beat of bar 2, lasting one beat:**
```
2.2-2.3=G4
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

| You write | Automation lane |
|---|---|
| `vol` or `volume` | Volume |
| `pan` | Pan |
| `pitch` or `pitchbend` | Pitch Bend |
| `mod` or `modulation` | Modulation |

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
| `expected Start.Beat-End.Beat=Note format` | Missing `-` or `=`, or they appear in the wrong order |
| `end position must be after start` | The end Bar.Beat is earlier than or equal to the start |
| `missing note` | Nothing written after the `=` |
| `note must come before parameters` | A `param=value` token appeared before the note name |
| `unrecognized note 'X'` | The note name is malformed (check letter, accidental, octave) |
| `unknown parameter 'X'` | The parameter name is not one of the four recognised names |

---

## Multiple Lines

Each line is processed independently. Use multiple lines to build a full sequence.

```
1.1-2.1=C4 vel=90
1.2-2.2=E4 vel=85
1.3-2.3=G4 vel=80
2.1-3.1=F3+A3+C4 vel=95 mod=20>60
3.1-4.1=G3+B3+D4 vel=100
4.1-5.1=C3+E3+G3 vel=90 mod=60>0
```

Formulas are **appended** to the clip — clicking Insert does not erase existing notes.

---

## Complete Examples

### Example 1 — C major scale, one note per beat

```
1.1-1.2=C4
1.2-1.3=D4
1.3-1.4=E4
1.4-2.1=F4
2.1-2.2=G4
2.2-2.3=A4
2.3-2.4=B4
2.4-3.1=C5
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

## Quick Reference

```
# Basic note (bar.beat notation)
1.1-2.1=C4                    C4, bars 1-2

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

- **Bar and beat numbers are 1-based.** Bar 1 Beat 1 is the very first step of the clip.
- **The note duration equals** `End.Beat − Start.Beat` in steps. A duration of zero is not allowed.
- **Automation values are 0–100%** and are clamped to that range. Values outside it are treated as 0 or 100.
- **Inserting automation replaces** any existing breakpoints in the note's step range for that parameter. Breakpoints outside the range are untouched.
- **Inserting notes is always additive.** Existing notes are never removed; Insert appends to whatever is already in the clip.
- **The formula text is saved** between opens of the Generator dialog, so you can refine and re-insert without retyping.
- **Octave convention:** C4 = MIDI 60 (middle C). C3 = MIDI 48, C5 = MIDI 72.
- **Flat accidentals** must be written as lowercase `b` (e.g. `Bb3`, `Eb4`). Uppercase `B` is always the note B.
