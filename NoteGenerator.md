# Note Generator

The Note Generator creates MIDI note patterns and inserts them directly into the currently open clip. Open it from the **Generate** button in the Piano Roll toolbar. It has four independent tabs — **Chord / Arp**, **Drum Patterns**, **Bass Lines**, and **Formula** — and remembers all settings between sessions.

Notes are always **appended** to the clip; existing notes are not replaced. Use the Piano Roll's undo (**Ctrl+Z**) to reverse an insert.

---

## Chord / Arp

Generates chord blocks, arpeggios, or both together, built from a harmonic progression you assemble with the chord palette.

> See **[ChordArpGenerator.md](ChordArpGenerator.md)** for the complete reference.

### Controls

| Control | Description |
|---|---|
| **Root Key** | The tonic note of the key (C – B). |
| **Scale** | The scale used to compute chord tones and degree roots. |
| **Chord Type** | Triad or extended chord voicing (Major, Minor, Dom7, Maj7, sus4, etc.). |
| **Arp Pattern** | The note-ordering pattern applied to each chord when arpeggiating (Up, Down, Up-Down, Random, etc.). |
| **Chord Oct.** | Octave (1–6) for the chord block notes. |
| **Note Duration** | How many steps each chord occupies (1/16 through 1 bar). |
| **Repeat** | How many times the full progression repeats before the insert stops. |
| **Chord block** | When checked, all chord tones are inserted simultaneously at each position. |
| **Arp Oct.** | Octave (1–6) for the arpeggio notes, independent of the chord octave. |
| **Arpeggio** | When checked, the arp pattern is inserted as sequential single notes at each position. |

Both **Chord block** and **Arpeggio** can be active at the same time — the chord block plays as a pad while the arp runs on top, each at its own octave.

### Building the chord progression

The **Chord Palette** (buttons I – VII) lets you build a progression one degree at a time:

- Click any Roman numeral button to **append** that degree to the end of the progression.
- The sequence strip shows the current progression as labelled chips.
- Click the **× on a chip** to remove that step from the progression.
- Click **Clear** to reset the entire progression.

The progression can hold any number of steps in any order. Each step occupies one *Note Duration* worth of time in the clip.

### How notes are generated

For each step in the progression (repeated N times):

1. The scale degree root is computed from the Root Key, Scale, and degree number.
2. If **Chord block** is on, the chord tones (computed from the chord root at **Chord Oct.**) are all inserted at the current cursor position with the full Note Duration.
3. If **Arpeggio** is on, the chord tones are rebuilt from the arp root at **Arp Oct.**, ordered by the Arp Pattern, and inserted as sequential 1-step notes starting at the cursor.
4. The cursor advances by one Note Duration, ready for the next degree.

---

## Drum Patterns

Inserts a rhythmic drum pattern from a built-in library of grooves organised by genre and style.

### Using the lists

- The left **Pattern** list selects the genre/family (Rock, Hip-Hop, Electronic, Jazz, Latin, R&B / Funk, Pop).
- The right **Style** list selects the specific groove within that family.
- Selecting a Pattern immediately updates the Style list to show the available grooves for that family.

### Drum instruments

Each pattern is a 16-step grid across several instruments:

| Abbrev. | Instrument |
|---|---|
| BD | Bass Drum |
| SD | Snare Drum |
| CH | Closed Hi-Hat |
| OH | Open Hi-Hat |
| RC | Ride Cymbal |
| HH | Hi-Hat (foot) |

### Clip length

Each pattern is 16 steps (one bar). When inserted, only the first 16 steps of the clip are filled. Steps beyond step 16 are left untouched. To tile the pattern across a longer clip, click **Insert into Clip** multiple times — notes are appended each time, so a second insert fills steps 17–32, and so on. Alternatively, use the Piano Roll's **Ctrl+C / Ctrl+V** to copy the bar and paste it into later positions.

### Available patterns

| Family | Styles |
|---|---|
| Rock 1 | Standard, Measure A, Measure B, Break |
| Rock 2 | Standard, Measure A |
| Hip-Hop | Boom Bap, Trap, Lo-Fi |
| Electronic | 4-on-Floor, House, Techno, Drum n Bass |
| Jazz | Swing, Bossa Nova |
| Latin | Samba, Beguine |
| R&B / Funk | Groove, Funk |
| Pop | 8th-note, Shuffle |

---

## Bass Lines

Inserts a melodic bass pattern built around a chosen root note and octave. Patterns are 16 steps long and automatically tile to fill the full clip length.

### Controls

| Control | Description |
|---|---|
| **Pattern** (left list) | Bass style family (Galloping Bass, Walking Bass, Funk Slap, Reggae Bass, House / Disco, Hip-Hop / Trap, R&B / Soul, Latin Bass, Rock Pump, Arpeggio Bass). |
| **Style** (right list) | Specific groove within the family. A short description is shown below the list. |
| **Root** | The root note the pattern is transposed to (C – B). All interval steps in the pattern are relative to this root. |
| **Octave** | Base octave (1–4) for the root note. The default is 2, placing the root in the lower register appropriate for bass. |

### How patterns repeat

One pattern cycle is always 16 steps. The generator calculates how many full and partial cycles are needed to fill the clip length and inserts them end-to-end. A 32-step clip gets two cycles; a 48-step clip gets three full cycles; a 24-step clip gets one full cycle plus an 8-step partial.

### Step structure

Each step inside a pattern carries:

| Field | Description |
|---|---|
| **Active / Rest** | Whether a note plays on that step. |
| **Interval** | Semitones above the root (0 = root, 7 = fifth, 12 = octave, etc.). |
| **Velocity** | Attack strength, giving dynamics within the pattern. |
| **Length** | How many steps the note sustains. Notes longer than 1 step advance the internal cursor by their length, so the next note does not overlap. |

### Available patterns

| Family | Styles |
|---|---|
| Galloping Bass | Standard, Root + Fifth, Double Gallop |
| Walking Bass | Up Arpeggio, Root-5th Walk, Approach Note |
| Funk Slap | Classic Funk, Thumb Pop, Groove Lock |
| Reggae Bass | Rockers, One Drop, Steppers |
| House / Disco | Disco Driver, Chicago House, Deep House |
| Hip-Hop / Trap | 808 Thump, Trap Slide, Boom Bap |
| R&B / Soul | Motown Groove, Neo Soul, Gospel Run |
| Latin Bass | Tumbao, Bossa Nova, Samba Drive |
| Rock Pump | Power Quarter, Root + Fifth Pump, Eighth Blitz |
| Arpeggio Bass | Minor Climb, Major Pop, Pentatonic Run |

---

## Formula

Enter notes and optional automation ramps as text using a compact bar/step notation. Every line describes one note event; multiple lines build up a full phrase. This is the fastest way to enter specific melodies, chords, or note sequences without drawing them step by step.

### Syntax

```
Start.Step-End.Step=Note [param=value ...]
```

| Part | Description |
|---|---|
| `Start.Step` | Bar and step where the note starts (both 1-based). |
| `End.Step` | Bar and step where the note ends (exclusive — the note plays up to but not including this position). |
| `Note` | A note name (`C4`, `Bb3`, `D#5`) or a plain MIDI number (`60`). Separate multiple notes with `+` for chords (`C4+E4+G4`). |
| `[param=value]` | Optional space-separated parameters (see below). |

### Bar.Step notation

Steps are **1–16** per bar (1 step = 1/16th note).

| Position | Meaning | Steps from clip start |
|---|---|---|
| `1.1` | Bar 1, step 1 (downbeat) | 0 |
| `1.5` | Bar 1, step 5 (beat 2) | 4 |
| `1.9` | Bar 1, step 9 (beat 3) | 8 |
| `1.13` | Bar 1, step 13 (beat 4) | 12 |
| `2.1` | Bar 2, step 1 | 16 |
| `3.1` | Bar 3, step 1 | 32 |

1 step = 1/16th note; 4 steps = 1 beat; 16 steps = 1 bar. Steps beyond 16 are invalid — use the next bar number.

### Note names

| You write | Result |
|---|---|
| `C4` | Middle C (MIDI 60) |
| `D#4` / `Eb4` | D sharp / E flat, octave 4 |
| `Bb3` | B flat, octave 3 |
| `60` | Plain MIDI number |

Accidentals: `#` for sharp, lowercase `b` for flat.

### Parameters

| Parameter | Range | Description |
|---|---|---|
| `vel` | 0–127 | Note velocity. Default is 100. Applies to all notes in a chord on that line. |
| `vol` | 0–100 | Volume automation. Flat value or ramp (`0>100`). |
| `pan` | 0–100 | Pan automation. 50 = centre. |
| `pitch` or `pitchbend` | 0–100 | Pitch bend automation. |
| `mod` or `modulation` | 0–100 | Modulation (CC1) automation. |

**Flat value:** `param=50` — holds the value constant over the note's time range.  
**Ramp:** `param=0>100` — linearly ramps from the start value to the end value.

Automation segments cover exactly the same bar/step range as the note on that line. They are written to the clip's automation lane and erase any existing breakpoints in that range for that parameter.

### Comments

Any text after `#` on a line is ignored.

```
# Chorus hook
1.1-3.1=C4+E4+G4 vel=95     # Cmaj
3.1-5.1=F3+A3+C4 vel=90     # Fmaj
5.1-7.1=G3+B3+D4 vel=100    # Gmaj
```

### Error handling

If any line contains a mistake, a **red error message** appears below the text box and nothing is inserted. The dialog stays open so you can correct the line and click Insert again.

### Quick examples

```
# Single note
1.1-2.1=C4 vel=80

# Chord
1.1-2.1=C4+E4+G4 vel=90

# Melody with mod swell
1.1-3.1=C4 vel=80 mod=0>80
3.1-5.1=E4 vel=85 mod=80>100
5.1-7.1=G4 vel=90 mod=100>40

# Bass line with volume and pan
1.1-2.1=C2 vel=110 vol=80 pan=45
2.1-3.1=G2 vel=105 vol=80 pan=45
```

> See **[NoteFormula.md](NoteFormula.md)** for the complete reference including all note names, every parameter, chord syntax, and a full set of worked examples.

---

## Tips

- Generate a **chord block only** (Arpeggio off) to lay down a pad track, then switch the same chord sequence to **arpeggio only** on a second track for a melodic line.
- Set **Chord Oct.** low (2–3) and **Arp Oct.** high (4–5) for a common bass + lead arp split.
- Increase **Repeat** to fill a 32- or 64-step clip in one insert without having to insert multiple times.
- The generator **appends** notes, so you can insert a chord block and then an arpeggio in two separate clicks to layer them, even changing settings between inserts.
- Use the **Drum Patterns** tab to lay down a groove quickly, then open the Piano Roll to fine-tune individual hits.
- For **Bass Lines**, setting the octave to 1 or 2 keeps the root well below the melody. Octave 3 works well for synth bass or when layering with chords.
- Use the **Formula tab** when you already know the notes you want — typing `1.1-2.1=C4+E4+G4` is faster than clicking four steps into the piano roll for a chord.
- In the Formula tab, add automation ramps alongside the notes to sketch filter sweeps, pitch bends, or volume shapes at the same time as entering the melody.
- All settings in all four tabs persist when you close and reopen the generator, including the chord progression sequence and formula text.
