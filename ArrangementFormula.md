# Arrangement Wizard Formula — User Manual

The **Arrangement Wizard** gives you a fast way to lay out clips on the Arrangement timeline by typing a compact text formula instead of dragging clips one by one. It works in **Arrangement mode** only.

---

## Opening the Wizard

1. Switch to **Arrangement mode** using the mode button in the toolbar.
2. Click the **Wizard** (wand) button in the toolbar.
3. Type your formulas in the text box.
4. Click **Apply** (or press **Ctrl+Enter**).

---

## Formula Syntax

Each line describes one track's arrangement. Tokens on the same line are comma-separated.

```
TrackName: token1, token2, token3, ...
```

### Token Formats

There are two ways to write each token:

#### Explicit — fixed start bar

```
StartBar-DurationBars=SceneName
```

Places the scene clip starting at `StartBar` for `DurationBars` bars, regardless of where the previous token ended.

| Part | Description |
|---|---|
| `StartBar` | Bar number where the clip slot begins (1-based) |
| `DurationBars` | How many bars the slot occupies |
| `SceneName` | Which scene's clip to place (see Scene Names below) |

#### Chain — follows the previous token

```
DurationBars=SceneName
```

Places the scene clip immediately after the end of the previous token on the same line. The first chain token on a line starts at **bar 1** if no explicit token has appeared before it.

---

## Naming Tracks

| You write | Resolves to |
|---|---|
| `T1`, `T2`, … | Track 1, Track 2, … (shortcut) |
| `Track1`, `Track2`, … | Track 1, Track 2, … (full form) |
| `Bass`, `Lead`, … | Track matched by name (case-insensitive) |
| `1`, `2`, … | Track 1, Track 2, … (plain number) |

---

## Naming Scenes

| You write | Resolves to |
|---|---|
| `S1`, `S2`, … | Scene 1, Scene 2, … (shortcut) |
| `Scene1`, `Scene2`, … | Scene 1, Scene 2, … (full form) |
| `Intro`, `Verse`, … | Scene matched by its row name (case-insensitive) |
| `My Riff`, `Fill A`, … | Scene matched by the **clip label** on that track (case-insensitive) |
| `1`, `2`, … | Scene 1, Scene 2, … (plain number) |

**Clip labels** are set with the **Label** button in the clip grid. When a track is known, the wizard checks that track's clip labels first before searching all tracks, so track-specific labels take priority.

---

## How Clip Placement Works

Each token defines a *slot* — a start bar and a duration in bars. The wizard fills that slot by repeating the source clip:

- If the slot is exactly one clip length, one instance is placed.
- If the slot is longer than the clip, the clip is tiled (repeated) to fill the duration.
- If the slot ends mid-way through a clip, the last instance is trimmed to fit.
- If a placed instance would overlap an existing arranged clip on the same track, that instance is skipped (existing content is never overwritten).

**Time units:**
- 1 step = 1/16th note
- 16 steps = 1 bar
- Bar numbers are 1-based throughout.

---

## Multiple Tracks

Use one line per track. All lines are applied together when you click Apply.

```
T1: 1-8=Intro, 8=Verse, 8=Chorus
T2: 1-8=Intro, 16=Verse, 32-8=Bridge
```

---

## Comments

Lines starting with `#` are ignored. Use them to annotate your formula.

```
# Full arrangement draft
T1: 1-8=Intro, 8=Verse, 8=Verse, 8=Chorus, 8=Outro
T2: 1-8=Intro, 8=Verse, 16=Chorus, 8=Outro
# T3: placeholder — add drums later
```

Blank lines are also ignored.

---

## Error Handling

If any track or scene name cannot be resolved, a **red error message** appears inside the dialog below the text box. The dialog stays open so you can correct the problem — **nothing is placed until all lines are valid**.

**Common errors:**

| Error message | Likely cause |
|---|---|
| `Unknown track: "X"` | The name before `:` doesn't match any track name, shortcut, or number |
| `Unknown scene: "X"` | The name after `=` doesn't match any scene name, clip label, shortcut, or number |

Fix the highlighted names and click Apply again.

---

## Complete Examples

### Example 1 — Simple two-track layout

Two tracks, each with Intro, Verse, and Chorus, using 8-bar slots chained together:

```
T1: 1-8=Intro, 8=Verse, 8=Chorus
T2: 1-8=Intro, 8=Verse, 8=Chorus
```

---

### Example 2 — Different section lengths per track

```
Lead:  1-4=Intro, 8=Verse, 16=Chorus, 4=Outro
Bass:  1-4=Intro, 8=Verse, 16=Chorus, 4=Outro
Drums: 1-8=Intro, 8=Verse, 16=Chorus, 8=Outro
```

---

### Example 3 — Explicit positioning (non-sequential)

Place clips at fixed bar positions regardless of chaining:

```
T1: 1-8=Intro, 17-8=Bridge, 33-8=Outro
T2: 1-8=Intro, 17-8=Bridge, 33-8=Outro
```

Bars 9–16 are left empty on both tracks.

---

### Example 4 — Mix of explicit and chain tokens

Start explicitly at bar 1, then chain the rest:

```
T1: 1-8=Intro, 8=Verse, 8=Verse, 8=Chorus, 4=Outro
```

This places:
- Intro at bars 1–8
- Verse at bars 9–16
- Verse at bars 17–24
- Chorus at bars 25–32
- Outro at bars 33–36

---

### Example 5 — Using scene names

If your scene rows are named "Intro", "Verse", "Chorus", and "Bridge":

```
Track1: 1-8=Intro, 8=Verse, 8=Chorus, 8=Bridge, 8=Chorus
Track2: 1-8=Intro, 16=Verse, 8=Chorus, 8=Outro
```

---

### Example 6 — Using clip labels

If the clips on Track 1 in each scene have been labelled "A", "B", "C":

```
T1: 1-8=A, 8=B, 8=C, 8=B, 8=A
```

The wizard matches the label against that specific track's clips, so labels unique to T1 won't clash with same-named labels on other tracks.

---

### Example 7 — Full arrangement with comments

```
# 32-bar arrangement: Intro 8, Verse 8, Chorus 8, Outro 8
Lead:  1-8=Intro, 8=Verse, 8=Chorus, 8=Outro
Bass:  1-8=Intro, 8=Verse, 8=Chorus, 8=Outro
Drums: 1-8=Intro, 8=Verse, 8=Chorus, 8=Outro
Keys:  1-8=Intro, 16=Chorus, 8=Outro
# Pad only plays on Chorus and Outro
Pad:   17-8=Chorus, 8=Outro
```

---

## Quick Reference

```
# Explicit token (fixed bar, fixed duration)
T1: 1-8=Intro

# Chain token (follows previous, or starts at bar 1)
T1: 8=Verse

# Combined on one line
T1: 1-8=Intro, 8=Verse, 8=Chorus

# Multiple tracks
T1: 1-8=Intro, 8=Verse
T2: 1-8=Intro, 8=Verse

# Scene by shortcut
T1: 1-8=S1, 8=S2

# Track by shortcut
T1: 1-8=Intro
Track1: 1-8=Intro    # same track, both forms work

# Named scene
T1: 1-8=Intro, 8=Verse, 8=Chorus, 8=Outro

# Clip label as scene name
T1: 1-8=Riff A, 8=Riff B

# Comment
# this line is ignored
```

---

## Notes

- **Only Arrangement mode** — the Wizard button is only active when the grid is in Arrangement view.
- **Bars are 1-based.** Bar 1 is the start of the timeline.
- **Duration is in whole bars** — fractional bar durations are not supported.
- **Clips are never overwritten.** If a generated instance would overlap existing content, that instance is silently skipped.
- **Partial fills** — if a slot duration is not a multiple of the source clip's length, the last instance is trimmed to fit exactly.
- **Track and scene names** are matched case-insensitively.
- **Clip labels** (set with the Label button in the grid) can be used as scene names. When the track is known, only that track's labels are searched first; the label check then falls back to all tracks.
- **Nothing is placed if any error exists** — the entire formula is validated before any clip is written to the timeline.
