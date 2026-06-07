# Chord / Arp Generator

The **Chord / Arp** generator is the first tab of the Note Generator overlay
(`GeneratorComponent`). It builds a chord progression and/or an arpeggio from a
root key, a scale, a chord size and an ordered list of scale degrees, then
inserts the resulting notes into the currently open clip.

- **UI / logic:** `Source/UI/GeneratorComponent.{h,cpp}` (tab `ChordArp`)
- **Music theory:** `Source/Sequencer/MusicTheory.h`
- **Opened from:** the Clip (piano-roll) editor; generated notes are returned to
  it through the `InsertFn` callback.
- **Time unit:** 1 step = 1/16 note (4 steps = 1 beat, 16 steps = 1 bar).

---

## Controls

| Control | Range / options | Meaning |
|---|---|---|
| **Root Key** | C … B (12) | Pitch class the scale/chords are built from. |
| **Scale** | Ionian, Dorian, Phrygian, Lydian, Mixolydian, Aeolian, Locrian, Melodic Minor, Harmonic Minor, Pentatonic Major/Minor, Blues | Chooses the interval set used to place each scale degree. |
| **Chord Size** | Triad, 7th, 9th | How many scale-thirds are stacked. The chord *quality* is derived diatonically from the scale (see below). |
| **Arp Pattern** | Up, Down, Up‑Down, Random, In‑Out, Out‑In | Order in which the chord's notes are played when the arpeggio is enabled. |
| **Chord Octave** | 1–6 (default 4) | Octave for the chord block. Octave 4 → C4 = MIDI 60. |
| **Arp Octave** | 1–6 (default 4) | Octave for the arpeggio (independent of the chord octave). |
| **Note Duration** | 1/16, 1/8, 1/4, 1/2, 1 bar | Step length of each degree slot. See mapping below. |
| **Repeat** | 1–8 (default 1) | How many times the whole degree sequence is repeated. |
| **Chord block** (toggle) | on/off (default on) | Emit a sustained chord for each degree. |
| **Arpeggio** (toggle) | on/off (default off) | Emit an arpeggio for each degree. |
| **Degree palette** | I II III IV V VI VII | Click a degree to **append** it to the sequence. Buttons show a **gold suggestion ring** for chords that would sound good next (see *Next-chord suggestions*). |
| **Sequence strip** | — | Shows the ordered degrees as chips; click a chip's right‑hand ✕ to remove it. |
| **Clear** | — | Empties the degree sequence. |
| **Insert into Clip** | — | Runs the generator and inserts the notes. |
| **Clear Notes** (toggle) | on/off | If on, the clip is cleared before the new notes are inserted. |

The default sequence is **I – IV – V** (`chordSequence = {0, 3, 4}`). All settings
persist across open/close via `GeneratorComponent::s_last`.

`Note Duration` maps to steps in `noteDurToSteps()`:

| Selection | Steps |
|---|---|
| 1/16 | 1 |
| 1/8  | 2 |
| 1/4  | 4 (default) |
| 1/2  | 8 |
| 1 bar | 16 |

---

## How generation works

The algorithm lives in `GeneratorComponent::generateChordArp()`. In pseudocode:

```
chordRoot = midiFromPcOctave(rootPC, chordOctave)   // (octave+1)*12 + pc
arpRoot   = midiFromPcOctave(rootPC, arpOctave)
degrees   = chordSequence (or [I] if empty)

numTones = {Triad:3, 7th:4, 9th:5}[chordSize]
cursor   = 0                     // position in steps
repeat 'Repeat' times:
    for each degree d in degrees:
        if Chord block:
            for pitch in diatonicChord(chordRoot, scale, d, numTones):
                emit note { pitch, start=cursor, duration=noteDur, velocity=100 }

        if Arpeggio:
            pitches = diatonicChord(arpRoot, scale, d, numTones)
            seq     = arpSequence(pitches, arpPattern, noteDur)  // noteDur entries
            for s, pitch in enumerate(seq):
                emit note { pitch, start=cursor+s, duration=1, velocity=90 }

        cursor += noteDur        // each degree occupies one noteDur slot
```

Key points:

1. **Diatonic harmony.** `diatonicChord(tonic, scale, d, numTones)` stacks thirds
   *within the scale*: it takes scale tones at positions `d, d+2, d+4 (, d+6, d+8)`,
   wrapping up octaves. The chord **quality emerges from the scale automatically**,
   so each degree gets the correct chord for the key. In C Ionian that yields
   I = C major, ii = D minor, iii = E minor, IV = F major, V = G major,
   vi = A minor, vii = B diminished — and the right qualities for every other
   scale too. `Chord Size` only changes how many tones are stacked (triad / 7th /
   9th).

2. **Each degree fills one `noteDur` slot.** The cursor advances by `noteDur`
   steps per degree, whether or not chord/arp is enabled.

3. **Chord block notes** are the raw chord intervals stacked above the degree
   root (e.g. Major = root, +4, +7 semitones), each held for the full `noteDur`,
   velocity 100.

4. **Arpeggio notes** are 1 step long, velocity 90, laid out across the
   `noteDur` slot in the chosen pattern (the pattern wraps/repeats to fill all
   `noteDur` steps).

5. **Chord + Arp together** overlap: the sustained chord and the arpeggio occupy
   the same step span for each degree.

6. **Clip-length clamping (quirk).** After generation every note is clamped to
   the clip:
   ```
   start    = min(start, clipLength - 1)
   duration = clamp(min(duration, clipLength - start), 1, …)
   ```
   Notes whose start lands beyond the clip end are *not dropped* — they pile up
   on the last step. Keep the sequence length (`degrees × repeat × noteDur`)
   within the clip length to avoid this.

### Arpeggio patterns

`arpSequence()` sorts the chord pitches ascending, then orders them:

- **Up** — low → high, repeating.
- **Down** — high → low, repeating.
- **Up‑Down** — up then back down without repeating the endpoints.
- **In‑Out** — alternates inward-bound ends: `lo, hi, lo+1, hi-1, …`.
- **Out‑In** — same but starting from the top: `hi, lo, hi-1, lo+1, …`.
- **Random** — random pick each step (uses `juce::Random`, so not reproducible).

The sequence is always exactly `noteDur` entries long (wrapping the pattern as
needed).

---

## Next-chord suggestions

As you build the sequence, the palette highlights which degree(s) tend to follow
the **last** chord, using `MusicTheory::suggestNextDegrees(scaleIndex, sequence)`.

- **Model (order-1):** each candidate degree is scored by diatonic root motion —
  the diatonic circle of fifths is "+3 scale degrees" per step, so a down-a-fifth
  move scores highest, then up-a-fifth (I→V) and step-up (IV→V). Bonuses are added
  for authentic cadences (dominant→I) and pre-dominant→dominant (ii/IV→V); the
  weak V→IV retrogression and chord repeats are penalised. An empty sequence
  suggests common openings (I, then IV / vi / V).
- **Display:** scores are normalised so the best = 1.0. The generator highlights
  **1 strong** ring (the top score; up to 2 if tied) plus **up to 2 "good"** rings
  for any other degree scoring **≥ 75%** of the best. Strong = bright thick ring,
  good = dim thin ring. Rings are drawn as a halo *around* the button so they read
  as a hint rather than a selection.
- **Recomputed** whenever you append/remove a degree, clear the sequence, or
  change the scale. It is advisory only — it never alters what is generated.
- Degrees outside the scale's note count (pentatonic/blues) are never suggested.

## Reference tables

### Scales (`MusicTheory::SCALES`, semitones from root)

| Scale | Intervals | Notes |
|---|---|---|
| Ionian (Major) | 0 2 4 5 7 9 11 | 7 |
| Dorian | 0 2 3 5 7 9 10 | 7 |
| Phrygian | 0 1 3 5 7 8 10 | 7 |
| Lydian | 0 2 4 6 7 9 11 | 7 |
| Mixolydian | 0 2 4 5 7 9 10 | 7 |
| Aeolian (Minor) | 0 2 3 5 7 8 10 | 7 |
| Locrian | 0 1 3 5 6 8 10 | 7 |
| Melodic Minor | 0 2 3 5 7 9 11 | 7 |
| Harmonic Minor | 0 2 3 5 7 8 11 | 7 |
| Pentatonic Major | 0 2 4 7 9 | 5 |
| Pentatonic Minor | 0 3 5 7 10 | 5 |
| Blues | 0 3 5 6 7 10 | 6 |

> Degrees are taken as `intervals[degree % 7]`. For the 5- and 6-note scales the
> unused trailing interval slots are 0, so high degrees (VI/VII) on a pentatonic
> scale fold back to the root — prefer degrees within the scale's note count.

### Chord size (`Chord Size` dropdown → stacked scale tones)

| Selection | Tones | Scale positions stacked |
|---|---|---|
| Triad | 3 | d, d+2, d+4 |
| 7th | 4 | d, d+2, d+4, d+6 |
| 9th | 5 | d, d+2, d+4, d+6, d+8 |

Chords are built diatonically via `MusicTheory::diatonicChord()`, so the quality
is always correct for the chosen scale. (The fixed `chordTypes()` table still
exists in `MusicTheory.h` for other callers but is no longer used by the chord
generator.)

---

## Output

Each emitted item is a `NoteEvent` (`Source/Sequencer/NativeAppState.h`):

```cpp
struct NoteEvent { int pitch; int start; int duration; int velocity; };
//                 MIDI 0-127  steps     steps         0-127
```

The full vector is handed back to the clip editor via the `onInsert` callback
(optionally preceded by `onClearNotes()` when the *Clear Notes* toggle is on).

---

## Worked example

Settings: Root = **C**, Scale = **Ionian**, Chord Size = **Triad**,
Sequence = **I – ii – iii – IV**, Chord Octave = **4**,
Note Duration = **1/4 (4 steps)**, Repeat = **1**, Chord block **on**,
Arpeggio **off**.

- chordRoot = C4 = MIDI 60.
- **I** → scale tones 0,2,4 → `{60, 64, 67}` = **C major** at steps 0–3.
- **ii** → scale tones 1,3,5 → `{62, 65, 69}` = **D minor** at steps 4–7.
- **iii** → scale tones 2,4,6 → `{64, 67, 71}` = **E minor** at steps 8–11.
- **IV** → scale tones 3,5,7 → `{65, 69, 72}` = **F major** at steps 12–15.

Each degree now gets the musically-correct quality (C, Dm, Em, F) instead of
four major chords. Choosing Chord Size = **7th** would add the next scale third
to each (e.g. I → Cmaj7 `{60,64,67,71}`). Enabling **Arpeggio** lays those same
chord tones out as 1‑step notes across each degree's slot in the chosen pattern.
