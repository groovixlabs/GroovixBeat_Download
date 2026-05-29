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

Each line describes one note event (or chord) and optional automation ramps. Three position formats are supported:

```
Start.Step - End.Step  = Note [param=value ...]   (explicit range)
Start.Step + N         = Note [param=value ...]   (start + step count)
           + N         = Note [param=value ...]   (continuation from previous note's end)
```

| Part | Description |
|---|---|
| `Start.Step` | Start position — bar number, dot, step (1–16) within the bar |
| `End.Step` | End position for the range format |
| `N` | Step count (1/16th notes) for the `+` formats |
| `Note` | MIDI note name or number; stack a chord with `+` between notes |
| `[param=value]` | Optional automation parameters (space-separated) |

---

## Line Continuity

**Lines are treated as a continuous sequence.** The continuation cursor advances to the end of each note and carries over to the next line. This means you can split a phrase across lines for readability — blank lines and comment-only lines are ignored and do not break continuity.

An explicit `Start.Step` always resets the cursor to that position and then advances it to `Start.Step + duration` for the next line.

```
# These two phrases produce exactly the same result:

# Written as one phrase:
1.1+4=C4  1.5+4=D4  1.9+4=E4  1.13+4=F4

# Written across lines for readability:
1.1+4=C4
+4=D4
+4=E4
+4=F4
```

---

## Bar.Step Notation

Positions are written as **Bar.Step**, where both bar and step are **1-based**.

- **Bar** — bar number (1, 2, 3, …)
- **Step** — 1/16th note position within the bar (1–16)

| You write | Meaning |
|---|---|
| `1.1` | Bar 1, Step 1 (downbeat) |
| `1.5` | Bar 1, Step 5 (beat 2) |
| `1.9` | Bar 1, Step 9 (beat 3) |
| `1.13` | Bar 1, Step 13 (beat 4) |
| `2.1` | Bar 2, Step 1 (downbeat) |

**Time units:**
- 1 step = 1/16th note
- 4 steps = 1 beat (quarter note)
- 16 steps = 1 bar

**Beat landmarks (4/4 reference):**

| Beat | Step notation |
|---|---|
| Beat 1 | `B.1` |
| Beat 2 | `B.5` |
| Beat 3 | `B.9` |
| Beat 4 | `B.13` |

---

## Position Formats

### Range: `Start.Step - End.Step`

The note plays from the start position up to (but not including) the end position. Duration = end step − start step.

```
1.1-2.1=C4        # whole note — 16 steps
1.1-1.5=C4        # quarter note — 4 steps
2.5-2.9=G4        # quarter note starting on beat 2 of bar 2
```

### Start + Count: `Start.Step + N`

The note starts at the given position and lasts exactly N steps. Equivalent to writing `Start.Step - (Start + N steps)`.

```
1.1+16=C4         # whole note (same as 1.1-2.1=C4)
1.1+4=C4          # quarter note
2.5+4=G4          # quarter note on beat 2 of bar 2
```

**Duration reference:**

| Duration | Steps | Range form | Count form |
|---|---|---|---|
| 1/16 note | 1 | `1.1-1.2=C4` | `1.1+1=C4` |
| 1/8 note | 2 | `1.1-1.3=C4` | `1.1+2=C4` |
| Dotted 1/8 | 3 | `1.1-1.4=C4` | `1.1+3=C4` |
| 1/4 note | 4 | `1.1-1.5=C4` | `1.1+4=C4` |
| Dotted 1/4 | 6 | `1.1-1.7=C4` | `1.1+6=C4` |
| 1/2 note | 8 | `1.1-1.9=C4` | `1.1+8=C4` |
| Whole note | 16 | `1.1-2.1=C4` | `1.1+16=C4` |
| 2 bars | 32 | `1.1-3.1=C4` | `1.1+32=C4` |

### Continuation: `+ N`

The note starts exactly where the previous note ended and lasts N steps. No start position is needed.

```
1.1+4=C4          # starts at step 0, ends at step 4
+4=D4             # starts at step 4, ends at step 8
+4=E4             # starts at step 8, ends at step 12
+4=F4             # starts at step 12, ends at step 16
```

The continuation cursor resets to 0 at the beginning of each Insert operation. An explicit `Start.Step` mid-sequence overrides the cursor for that line and all lines that follow.

```
1.1+4=C4          # cursor -> step 4
+4=D4             # cursor -> step 8
3.1+4=G4          # explicit jump to bar 3 beat 1; cursor -> step 36
+4=A4             # continues from step 36; cursor -> step 40
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
| Accidental | `#` for sharp, `b` (lowercase) for flat, or omit |
| Octave | Integer; C4 = middle C (MIDI 60) |

| You write | Note | MIDI |
|---|---|---|
| `C4` | Middle C | 60 |
| `D#4` | D sharp, octave 4 | 63 |
| `Eb4` | E flat, octave 4 | 63 |
| `Bb2` | B flat, octave 2 | 46 |
| `G5` | G, octave 5 | 79 |

### MIDI Numbers

Plain integers 0–127 are accepted in place of a note name.

```
1.1+16=60         # C4 by MIDI number
```

**Octave reference:**

| Note | MIDI |
|---|---|
| C2 | 36 |
| C3 | 48 |
| C4 (middle C) | 60 |
| C5 | 72 |
| C6 | 84 |

---

## Chords

Stack multiple notes on the same timing by separating note names with `+` (no spaces). The `+` inside the note part (after `=`) is always chord notation and is never confused with the `+` duration separator (which appears before `=`).

```
1.1-2.1=C3+E3+G3          # C major triad, whole note
1.1+16=C4+E4+G4+B4        # Cmaj7, whole note, count form
+8=F3+A3+C4               # F major, continuation, half note
```

---

## Velocity

Control note velocity with `vel=` (0–127, default 100). Applies to all notes in a chord on that line.

```
1.1+4=C4 vel=80
2.1-3.1=G3+B3+D4 vel=110
```

---

## Automation Parameters

Add optional automation ramps using named parameters after the note. Each parameter creates a linear segment covering the same time range as the note.

### Flat Value

```
param=VALUE
```

`VALUE` is a percentage from 0 to 100.

```
1.1+16=C4 vol=80
```

### Ramp

```
param=FROM>TO
```

```
1.1+32=C3 mod=0>100
```

### Recognised Parameters

| You write | Automation lane | Value meaning |
|---|---|---|
| `vol` or `volume` | Volume | 0 = silence, 100 = full |
| `pan` | Pan | 0 = hard left, 50 = centre, 100 = hard right |
| `pitch` or `pitchbend` | Pitch Bend | 0 = max down, 50 = centre, 100 = max up |
| `mod` or `modulation` | Modulation (CC1) | 0 = none, 100 = maximum |

---

## Comments

Any text after `#` on a line is ignored. Blank lines are also ignored and do not break the continuation cursor.

```
# Verse melody
1.1+4=C4 vel=80      # root
+4=E4 vel=75         # third — continues from step 4
+4=G4 vel=85         # fifth — continues from step 8

# Bar 2 — new phrase (explicit reset)
2.1+4=F4 vel=80
+4=A4
+4=C5
```

---

## Error Handling

If a line contains a mistake, a red error message appears below the text box and nothing is inserted. The dialog stays open so you can correct the line and try again.

**Common errors:**

| Error message | Likely cause |
|---|---|
| `missing '='` | No `=` found on the line |
| `expected start-end, start+steps, or +steps` | Position format not recognised |
| `step count must be >= 1` | The N in `+N` is zero or missing |
| `end must be after start` | End position is at or before start (range format) |
| `missing note` | Nothing written after `=` |
| `note must come before parameters` | A `param=value` token appeared before the note name |
| `unrecognized note 'X'` | Note name is malformed — check letter, accidental, octave |
| `unknown parameter 'X'` | Parameter name not recognised — use `vol`, `pan`, `pitch`, `mod` |

---

## Complete Examples

### Example 1 — C major scale, continuation style

**Quarter notes using continuation (most concise):**
```
1.1+4=C4
+4=D4
+4=E4
+4=F4
+4=G4
+4=A4
+4=B4
+4=C5
```

**Eighth notes in one bar:**
```
1.1+2=C4
+2=D4
+2=E4
+2=F4
+2=G4
+2=A4
+2=B4
+2=C5
```

**16th note run, half bar:**
```
1.1+1=C4
+1=D4
+1=E4
+1=F4
+1=G4
+1=A4
+1=B4
+1=C5
```

---

### Example 2 — Chord progression, count form

```
# I - IV - V - I in C major
1.1+16=C3+E3+G3 vel=95
+16=F3+A3+C4 vel=90
+16=G3+B3+D4 vel=100
+16=C3+E3+G3 vel=85
```

---

### Example 3 — Melody with modulation swell

```
# Phrases split across lines for readability

# Phrase 1
1.1+8=C4 vel=80 mod=0>60
+8=E4 vel=85 mod=60>100

# Phrase 2 (explicit reset to bar 3)
3.1+8=G4 vel=90 mod=100>40
+8=C5 vel=95 mod=40>0
```

---

### Example 4 — Mixing range and count notation

```
# Anchor with explicit positions, fill gaps with continuation
1.1-2.1=C3 vel=110 pitch=50>52    # whole note, range form
2.1+8=G2 vel=105                   # half note, count form
+4=A2 vel=100                      # quarter note, continuation
+4=F2 vel=105                      # quarter note, continuation
```

---

### Example 5 — Pad chord with long fade-in

```
# Single long chord, volume and mod ramp
1.1+64=C3+E3+G3+B3 vel=70 vol=0>100 mod=20>60
```

---

### Example 6 — 16th-note bass riff with chord stabs

```
# Bass line (continuation)
1.1+2=C2 vel=110
+2=C2 vel=80
+2=G2 vel=105
+2=G2 vel=75
+2=A2 vel=100
+2=A2 vel=70
+2=F2 vel=105
+2=F2 vel=75

# Chord stabs on beats 1 and 3 (explicit positions)
1.1+4=C3+E3+G3 vel=90
1.9+4=A2+C3+E3 vel=85
```

---

## Quick Reference

```
# Range form (start - end)
1.1-2.1=C4                    whole note, bar 1

# Count form (start + steps)
1.1+16=C4                     whole note, bar 1 (same as above)
1.1+4=C4                      quarter note

# Continuation (no start — follows previous note)
+4=D4                         quarter note from where last note ended
+2=E4                         eighth note continuing on

# Chord (+ inside note part, after =)
1.1+16=C4+E4+G4

# Velocity
1.1+4=C4 vel=80

# Automation flat
1.1+16=C4 vol=75

# Automation ramp
1.1+32=C4 mod=0>100

# Multiple params
1.1+16=G3 vel=90 mod=0>80 pitch=50>55

# MIDI number
1.1+16=60

# Explicit reset mid-sequence
1.1+4=C4
+4=D4
3.1+4=G4                      jumps to bar 3 -- resets cursor
+4=A4                         continues from bar 3 beat 2

# Comment
# this line is ignored
```

---

## Notes and Limitations

- **Bar and step numbers are 1-based.** Bar 1 Step 1 is the very first 1/16th note of the clip. Steps run 1–16 per bar.
- **Continuation cursor starts at step 0** (bar 1, step 1) at the beginning of each Insert operation.
- **Blank lines and comment-only lines** do not advance the cursor — they are skipped entirely.
- **An explicit `Start.Step`** always overrides the cursor and sets it to `Start.Step + duration` for subsequent continuation lines.
- **Duration must be at least 1 step.** Zero-duration notes are not allowed.
- **Automation values are 0–100%** and are clamped to that range.
- **Inserting is always additive.** Existing notes are never removed; Insert appends to whatever is already in the clip.
- **The formula text is saved** between opens of the Generator dialog, so you can refine and re-insert without retyping.
- **Chord `+` vs duration `+`** — `+` before `=` is always a duration separator; `+` after `=` is always a chord separator. They cannot be confused.
- **Flat accidentals** must be written as lowercase `b` (e.g. `Bb3`, `Eb4`). Uppercase `B` is always the note B.
- **Octave convention:** C4 = MIDI 60 (middle C). C3 = MIDI 48, C5 = MIDI 72.
