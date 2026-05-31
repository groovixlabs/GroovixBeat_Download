# Note Formula — User Manual

The **Formula** tab in the **Note Generator** lets you type notes directly using a compact text notation instead of drawing them in the piano roll. It is ideal for quickly sketching melodic ideas, entering chord progressions, or setting up automation ramps alongside the notes — all in one pass.

---

## Opening the Formula Tab

1. Open a clip in the **Clip Editor**.
2. Click the **Gen** (Generator) button in the toolbar.
3. Click the **Formula** tab (fourth tab, next to Bass Lines).
4. Type your formulas in the text box.
5. Click **Insert into Clip**.

Inserting replaces any existing notes that overlap the specified ranges. Notes outside those ranges are untouched.

---

## Formula Syntax

Each line describes one or more note events separated by commas. Multiple lines are also supported — commas and newlines are interchangeable as note separators. Four token formats are recognised:

```
Start.Step - End.Step  = Note [param=value ...]   (explicit range)
Start.Step + N         = Note [param=value ...]   (start + step count)
           + N         = Note [param=value ...]   (continuation)
                 Note [param=value ...]            (bare note — reuse last step count)
```

| Part | Description |
|---|---|
| `Start.Step` | Start position — bar number, dot, step (1–16) within the bar |
| `End.Step` | End position for the range format |
| `N` | Step count (1/16th notes) for the `+` formats |
| `Note` | MIDI note name or number; stack a chord with `+` between notes |
| `[param=value]` | Optional automation parameters (space-separated) |

---

## Separators — Commas and Newlines

**Commas and newlines are interchangeable note separators.** The continuation cursor advances after every note token and carries over regardless of whether tokens are split by commas or line breaks. Blank lines and comment-only lines are ignored.

An explicit `Start.Step` always resets the cursor to that position.

All four of the following are identical:

```
# All commas on one line
1.1+4=C4, +4=D4, +4=E4, +4=F4

# Commas with bare notes
1.1+4=C4, D4, E4, F4

# One per line
1.1+4=C4
+4=D4
+4=E4
+4=F4

# Mixed — split for visual grouping
1.1+4=C4, D4
E4, F4
```

`#` comments strip the rest of the **line** (not just up to the next comma), so a comment silences all tokens that follow it on the same line.

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

---

## Position Formats

### Range: `Start.Step - End.Step`

The note plays from the start position up to (but not including) the end position.

```
1.1-2.1=C4        # whole note — 16 steps
1.1-1.5=C4        # quarter note — 4 steps
```

### Start + Count: `Start.Step + N`

The note starts at the given position and lasts exactly N steps.

```
1.1+16=C4         # whole note
1.1+4=C4          # quarter note
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

The note starts exactly where the previous note ended and lasts N steps.

```
1.1+4=C4          # step 0–4
+4=D4             # step 4–8
+4=E4             # step 8–12
+4=F4             # step 12–16
```

### Bare Note (Repeat Last Length)

A line with **no position prefix and no `=`** reuses the step count from the most recent `+N` token and continues from where the previous note ended. The entire line is treated as the note and optional parameters.

```
1.1+4=C4          # establishes step count = 4
D4                # same length, next position
E4                # same length, next position
F4                # same length, next position
```

This is identical to writing `+4=` before each note:
```
1.1+4=C4
+4=D4
+4=E4
+4=F4
```

Bare notes work with chords, velocity, and automation parameters just like any other line:
```
1.1+4=C3+E3+G3 vel=90
F3+A3+C4 vel=85
G3+B3+D4 vel=100
C3+E3+G3 vel=90
```

The bare note shorthand also works after `start-end` and `start+N` tokens — the equivalent step count is remembered in all cases.

**Notes:**
- A bare note with no preceding step count produces an error.
- Velocity and automation parameters on bare note lines are separated by spaces after the note name, exactly as in the `=`-prefixed format.

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

---

## Chords

Stack multiple notes on the same timing by separating note names with `+` (no spaces). The `+` inside the note part is always chord notation and is never confused with the `+` duration separator (which appears before `=`).

```
1.1+16=C3+E3+G3        # C major triad, whole note
+8=F3+A3+C4            # F major, continuation, half note
F3+A3+C4               # same as above, bare note form
```

---

## Velocity

Control note velocity with `vel=` (0–127, default 100). Applies to all notes in a chord on that line.

```
1.1+4=C4 vel=80
D4 vel=75              # bare note with velocity
```

---

## Automation Parameters

Add optional automation ramps using named parameters after the note. Each parameter creates a linear segment covering the same time range as the note.

### Flat Value

```
param=VALUE
```

### Ramp

```
param=FROM>TO
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

Any text after `#` on a line is ignored, including any comma-separated tokens that follow on the same line. Blank lines are also ignored and do not break the continuation cursor.

```
# Verse melody — quarter notes
1.1+4=C4 vel=80, D4 vel=75, E4 vel=85   # three notes, one line

# Bar 2 — new phrase (explicit reset)
2.1+4=F4 vel=80
A4, C5
```

---

## Error Handling

If a line contains a mistake, a red error message appears below the text box and nothing is inserted. The dialog stays open so you can correct the line and try again.

**Common errors:**

| Error message | Likely cause |
|---|---|
| `bare note has no preceding step count to inherit` | A bare note line appeared before any `+N` token established a length |
| `expected start-end, start+steps, +steps, or bare note` | Position format not recognised |
| `step count must be >= 1` | The N in `+N` is zero or missing |
| `end must be after start` | End position is at or before start (range format) |
| `missing note` | Nothing written after `=` |
| `note must come before parameters` | A `param=value` token appeared before the note name |
| `unrecognized note 'X'` | Note name is malformed — check letter, accidental, octave |
| `unknown parameter 'X'` | Parameter name not recognised — use `vol`, `pan`, `pitch`, `mod` |

---

## Complete Examples

### Example 1 — C major scale, four ways

**Using continuation (`+N=`):**
```
1.1+4=C4, +4=D4, +4=E4, +4=F4, +4=G4, +4=A4, +4=B4, +4=C5
```

**Using bare notes, all on one line:**
```
1.1+4=C4, D4, E4, F4, G4, A4, B4, C5
```

**Bare notes across lines for readability:**
```
1.1+4=C4
D4, E4, F4
G4, A4, B4, C5
```

**Eighth notes in one bar:**
```
1.1+2=C4, D4, E4, F4, G4, A4, B4, C5
```

---

### Example 2 — Chord progression, bare note style

```
# I - IV - V - I in C major, whole notes
1.1+16=C3+E3+G3 vel=95, F3+A3+C4 vel=90, G3+B3+D4 vel=100, C3+E3+G3 vel=85
```

---

### Example 3 — Melody with modulation swell

```
# Phrase 1
1.1+8=C4 vel=80 mod=0>60
E4 vel=85 mod=60>100

# Phrase 2 (explicit reset to bar 3)
3.1+8=G4 vel=90 mod=100>40
C5 vel=95 mod=40>0
```

---

### Example 4 — Mixing all four formats

```
# Anchor, then fill with bare notes
1.1-2.1=C3 vel=110 pitch=50>52    # whole note, range form
2.1+8=G2 vel=105                   # half note, count form
A2 vel=100                         # bare: same 8-step length
F2 vel=105                         # bare: continues
```

---

### Example 5 — Pad chord with long fade-in

```
1.1+64=C3+E3+G3+B3 vel=70 vol=0>100 mod=20>60
```

---

### Example 6 — 16th-note bass riff using bare notes

```
# Compact: one line sets length, rest are bare values separated by commas
1.1+2=C2 vel=110, C2 vel=80, G2 vel=105, G2 vel=75, A2 vel=100, A2 vel=70, F2 vel=105, F2 vel=75
```

Or split across lines for readability:
```
1.1+2=C2 vel=110, C2 vel=80
G2 vel=105, G2 vel=75
A2 vel=100, A2 vel=70
F2 vel=105, F2 vel=75
```

---

## Quick Reference

```
# Range form
1.1-2.1=C4                         whole note, bar 1

# Count form
1.1+16=C4                           whole note, bar 1
1.1+4=C4                            quarter note

# Continuation
+4=D4                               quarter note, follows previous

# Bare note (reuse last step count, continue)
D4                                  same length as previous +N, next position
D4 vel=80                           bare note with velocity
D3+F3+A3                            bare chord

# Comma-separated on one line (equivalent to separate lines)
1.1+4=C4, D4, E4, F4               four quarter notes
1.1+4=C4, +4=D4, 3.1+4=G4, A4     mixed formats on one line

# Chord
1.1+16=C4+E4+G4

# Velocity
1.1+4=C4 vel=80

# Automation
1.1+32=C4 mod=0>100
1.1+16=G3 vel=90 mod=0>80 pitch=50>55

# Explicit reset mid-sequence
1.1+4=C4, D4
3.1+4=G4                            jumps to bar 3, resets cursor
A4                                  continues from bar 3 beat 2

# Comment strips rest of line including any following commas
1.1+4=C4, D4   # these two notes are inserted, rest of line is ignored
E4, F4

# This line is fully ignored
# this line is ignored
```

---

## Notes and Limitations

- **Commas and newlines are equivalent** separators — use whichever is clearest.
- **`#` comments** strip the rest of the line, including any comma-separated tokens that follow on the same line.
- **Bar and step numbers are 1-based.** Steps run 1–16 per bar.
- **Continuation cursor starts at step 0** at the beginning of each Insert operation.
- **Blank lines and comment-only lines** do not advance the cursor.
- **An explicit `Start.Step`** always overrides the cursor.
- **Bare notes** reuse the last step count set by any `+N`, `start+N`, or `start-end` token. A bare note before any such token causes an error.
- **Inserting replaces** existing notes that overlap the placed range. Notes outside the range are untouched.
- **The formula text is saved** between opens of the Generator dialog.
- **Chord `+` vs duration `+`** — `+` before `=` is always a duration separator; `+` after `=` is always a chord separator.
- **Flat accidentals** must be written as lowercase `b` (e.g. `Bb3`, `Eb4`). Uppercase `B` is always the note B.
- **Octave convention:** C4 = MIDI 60 (middle C). C3 = MIDI 48, C5 = MIDI 72.
