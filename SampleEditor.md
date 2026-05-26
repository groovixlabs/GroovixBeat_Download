# Sample Editor — User Manual

The **Sample Editor** is a full-overlay waveform editor that opens when you click a sample clip cell in the Live Performance grid. It lets you load, trim, fade, silence, slice, pitch-shift, and time-stretch audio directly inside GrooviXBeat without leaving the session.

---

## Opening and Closing

- **Open**: Click any occupied (or empty) sample clip cell in the Live Performance grid.
- **Close**: Click the **X** button at the far right of the toolbar, or press **Esc** (if the dialog is focused).
- Changes are applied immediately and in memory. The edited audio is flushed to disk when the project is saved.

---

## Layout

```
+------------------------------------------------------------------+
| Toolbar  [buttons left]               [Len: - N + | Q combo | X] |
+------------------------------------------------------------------+
| Ruler (time, click/drag to set cursor)                           |
+------------------------------------------------------------------+
|                                                                  |
|   Waveform area  (scroll, zoom, select, split)                   |
|                                                                  |
+------------------------------------------------------------------+
| Scrollbar (horizontal pan / zoom range indicator)                |
+------------------------------------------------------------------+
| Info bar  filename | duration | offset | BPM | selection         |
+------------------------------------------------------------------+
```

| Zone | Height | Purpose |
|---|---|---|
| Toolbar | 36 px | All edit buttons, playback controls, clip settings |
| Ruler | 18 px | Time axis; click or drag to set the paste/seek cursor |
| Waveform | remaining | Waveform display; selection and split interaction |
| Scrollbar | 12 px | Horizontal pan; thumb width reflects the current zoom level |
| Info bar | 24 px | Filename (left, gold), stats (dim), hover hint (right, gold) |

---

## Toolbar Buttons — Left Side

Buttons appear left to right in this order.

---

### Load

Opens a file browser to replace the current clip with a new audio file. Supported formats: WAV, AIF/AIFF, MP3, OGG, FLAC, M4A, CAF.

- If **Auto-Q** is on in the Live Performance toolbar and the file's BPM is reliably detected (from the filename or ACID metadata), the file is automatically WSOLA-stretched to the project tempo and the clip length is snapped to the nearest bar count.

---

### Reset

Discards all edits and restores the original audio file (the file on disk before any edits were made in this session). A confirmation dialog is shown before the reset is applied.

---

### Undo / Redo

Step backward and forward through the edit history for this clip. The buttons are dimmed when no undo or redo states are available.

- Keyboard: **Ctrl+Z** (undo), **Ctrl+Y** (redo).

---

### Duplicate

Copies the current selection (or the whole sample if there is no selection) and pastes the copy immediately after the original, extending the sample length. Useful for repeating a phrase or doubling a loop.

---

### Nudge Left / Nudge Right

Shifts the **playback start offset** earlier or later without modifying the audio file. The sample waveform moves visually, and the darkened pre-roll region before the offset shows how much lead-in space exists.

| Modifier | Step size |
|---|---|
| (none) | 1/64 step |
| Shift | 1/16 step |
| Ctrl | 1 bar (16 steps) |

The offset is shown in the info bar as `Offset: +0.12s`. Negative offset moves the sample earlier than the clip start.

---

### Trim

Removes all audio **outside** the current selection, keeping only the selected region.

- If there is no selection, Trim does nothing (the whole sample is already the full range).
- Split markers that fall within the kept region are preserved and shifted to match the new start offset. Markers outside the kept range are discarded.

---

### Fade In

Opens the **Fade In Level** dialog. A horizontal slider lets you set the start level (0–100%). At 0% the audio fades from silence up to full volume over the selection range. At 50% the audio fades from half volume to full.

- Fade applies to the current selection, or the whole sample if nothing is selected.
- The fade is linear (applied to amplitude, not dB).

---

### Fade Out

Opens the **Fade Out Level** dialog. The slider sets the end level. At 0% the audio fades to silence at the end of the selection. At 50% it fades to half volume.

- Same range rules as Fade In.

---

### Silence

Replaces the current selection (or the whole sample if nothing is selected) with digital silence (all zeros). This is a destructive edit — use Undo if you apply it by mistake.

---

### Split (Manual Slicer)

Places a **split marker** at the current paste cursor position, dividing the sample into independently selectable sections.

- A paste cursor must be positioned first (click in the waveform or ruler). The split button is inactive if no cursor exists.
- Split markers appear as gold vertical lines with downward triangles at the top and upward triangles at the bottom.
- Each section can be independently focused (click within it), labelled, deleted, or operated on.
- Keyboard shortcut: **S** (when a paste cursor is active).
- To remove all splits, undo with **Ctrl+Z** or press **Reset**.
- Split data is saved as a `.splits` sidecar file next to the audio file and restored automatically when the editor reopens.

---

### Auto-Split (Auto Slicer)

Opens the **Auto-Split** dialog which uses spectral analysis to detect structural boundaries automatically and divides the sample into named sections.

**Dialog options:**

| Option | Behaviour |
|---|---|
| **Auto-detect** | Analyser decides how many boundaries to place based on the material |
| **2 / 4 / 8 / 16 sections** | Forces exactly that many equally or structurally spaced splits |

Click **Analyze & Split**. A wait cursor is shown during analysis (which runs on the audio thread). When complete:

- Split markers are placed at the detected boundaries.
- Each section is automatically labelled from the set: **Intro**, **Verse**, **Buildup**, **Drop**, **Break**, **Outro**.
  - The first section is always re-classified as **Intro** if it is quiet or ambient.
  - The last section is always re-classified as **Outro** under the same conditions.
- Labels are shown in gold in the top-left corner of each section.

If no clear boundaries are found, a message dialog appears. Try a longer sample or choose a fixed section count.

---

### Tape Slow

Slows the sample down by a tape-stretch factor: **-1 semitone pitch**, **+5.9% duration**. This simulates the effect of slowing analogue tape — pitch drops and the sample gets slightly longer.

Each click applies one step. Use Undo to reverse.

---

### Tape Fast

Speeds the sample up: **+1 semitone pitch**, **-5.6% duration**. Simulates speeding tape — pitch rises and the sample gets slightly shorter.

---

### Pitch Up

Raises pitch by **+1 semitone** using WSOLA time-domain pitch shifting. Duration is unchanged. Unlike Tape Fast, this does not alter the timing of the sample.

---

### Pitch Down

Lowers pitch by **-1 semitone**, duration unchanged.

---

### BPM / Stretch

Opens the **BPM / Stretch** dialog for time-stretching the sample to a target BPM.

**Dialog fields:**

| Field | Description |
|---|---|
| **Sample BPM** | The BPM of the sample. Pre-filled if detected from the filename or audio metadata. |
| **Analyze** | Runs DSP beat analysis in a background thread to detect the sample BPM. Result is filled into the BPM field when complete. |
| **Algorithm** | The time-stretch algorithm to use. |
| **Source** | Shows where the current BPM value came from. |
| **Project BPM** | The current project tempo (read-only). |
| **Stretch to Project BPM** | Applies WSOLA (or selected algorithm) to warp the sample duration so it plays at the project tempo. |

**BPM detection priority:**

1. Parsed from the filename (e.g. `loop_120bpm.wav`, `funk_128_groove.wav`).
2. ACID / metadata embedded in the audio file.
3. DSP analysis (click **Analyze**).
4. Manual entry.

**Stretch algorithms:**

| Name | Quality | Speed |
|---|---|---|
| **Medium - WSOLA** | Good; transient-safe | Fast |
| **High - Phase Vocoder** | Best for tonal/melodic content | Slower |
| **Low - Linear** | Minimal CPU; audible artefacts | Fastest |

---

### Loop

Toggles the clip's playback mode between **Loop** (repeating) and **One-shot** (plays once and stops). When active (gold background), the clip loops continuously. When inactive, it plays once from the start position and stops.

This setting is stored per clip and persists when the editor is closed.

---

### Snap (Magnet)

Toggles quantise snapping for the paste cursor and selection edges. When **on** (gold background), click and drag positions snap to the global quantise grid (set by the Q combo in the main Live Performance toolbar).

Available snap resolutions (set by the main toolbar Q): 1/16, 1/4, 1/2, 1 bar.

---

## Toolbar Controls — Right Side

These controls appear at the right end of the toolbar.

---

### Len: — N — + (Clip Length)

Sets the **clip length in bars** — how many bars of the arrangement timeline this clip occupies when triggered.

| Control | Action |
|---|---|
| **–** button | Decrease length by 1 bar |
| **N** (value label) | Shows current bar count. Double-click to type a value directly (1–256 bars). |
| **+** button | Increase length by 1 bar |

The clip-end boundary is shown in the waveform as a dashed gold vertical line labelled **clip end**. Audio to the right of this line is dimmed — it is loaded but will not play.

---

### Q (Quantize Combo)

Sets the **launch quantise** for this clip — how precisely the clip start aligns to the beat when triggered from the Live Performance grid.

| Option | Behaviour |
|---|---|
| **Inherit** | Uses the global song quantise from the main toolbar |
| **1/16** | Clip starts on the next 1/16th note boundary |
| **1/4** | Clip starts on the next beat |
| **1/2** | Clip starts on the next half-bar |
| **1 Bar** | Clip starts on the next bar boundary |

---

### X (Close)

Closes the Sample Editor overlay and returns to the Live Performance grid. Any edits made during the session are retained in memory and written to disk on the next project save.

---

## Waveform Area

### Visual indicators

| Element | Colour | Meaning |
|---|---|---|
| Waveform peaks | Blue | Audio content (L channel or mono) |
| Paste cursor | Teal dashed line + arrow | Current edit / paste / split position |
| Playhead | Green line | Real-time audio playback position |
| Selection | Gold translucent fill | The range that operations will affect |
| Split markers | Gold lines with triangles | Section boundaries |
| Focused section border | Purple rectangle | The section currently selected for editing |
| Clip-end line | Dashed gold | Where the clip loop ends; audio to the right is dimmed |
| Silence region | Dark hatching | Gap between audio end and clip-end boundary |
| Pre-roll region | Dark blue hatching | Lead-in before the sample's offset start point |
| Bar lines | Grey vertical lines | Bar numbers shown above each line |
| Beat lines | Darker grey lines | Beat subdivisions (only drawn when zoomed in enough) |

---

## Modes

### Default (Edit) Mode

When the **SEL** button in the Live Performance toolbar is **off**:

- **Click** anywhere in the waveform — places the paste cursor (teal). This is the position used for Split, Paste, and BPM-referenced playback.
- Clicking inside a split section also **focuses** that section (purple border). Edit operations (Trim, Fade, Silence, Duplicate, Delete) will target that section's range.
- No drag-selection is possible — dragging does nothing.

### Select Mode

When the **SEL** button in the Live Performance toolbar is **on**:

- **Click and drag** in the waveform draws a **rubber-band selection** (gold fill). The selection is shown in the info bar with its start time, end time, and duration.
- **Click without dragging** places the paste cursor (same as Edit mode).
- Within a split section, you can rubber-band a sub-range to operate on just part of that section.
- All edit operations (Trim, Fade In, Fade Out, Silence, Duplicate) apply to the selection rather than the whole sample.
- **Ctrl+A** selects the entire sample.

When a selection is active, its duration is shown both in the waveform overlay and in the info bar.

### Effective Range

All edit operations determine their target range in this priority order:

1. **Rubber-band selection** (drag in SEL mode) — highest priority.
2. **Focused split section** (click in split mode) — used if no rubber-band selection exists.
3. **Whole sample** — used when neither a selection nor a focused section is active.

---

## Ruler

The ruler strip (below the toolbar) shows time in seconds and minutes. Click or drag in the ruler to position the paste cursor. The teal triangle and time label in the ruler always show the current cursor position.

Ruler interaction respects the **Snap** button — if Snap is on, the cursor jumps to the nearest grid line.

---

## Navigation and Zoom

| Action | Result |
|---|---|
| **Ctrl + scroll wheel** | Zoom in / out around the cursor position |
| **Scroll wheel** | Scroll left / right |
| **Horizontal scrollbar** drag | Pan the visible window |
| **Zoom In / Zoom Out** buttons (main toolbar) | Halve / double the visible time span |
| **Zoom Fit** button (main toolbar) | Show the entire sample |

---

## Playback

- **Space** — Play from the paste cursor (or from the selection / focused section start if SEL mode is on). Press again to stop.
- **Home** — Return the cursor to position 0 and stop playback.
- **Loop button** — When on, the clip loops; when off, it plays once and stops.
- When a **selection** or **focused split section** is active in SEL mode, Space plays only that region sample-accurately (no timer race — the audio thread enforces the stop via a sample counter).
- The green **playhead** line moves in real time during playback and wraps back on loop.

---

## Slicing in Detail

Slicing divides the sample into independently addressable sections without modifying the audio data.

### Manual Split workflow

1. Click in the waveform (or ruler) to position the teal paste cursor.
2. Click **Split** (or press **S**) to place a split marker at that position.
3. Repeat to add more markers. You can have any number of splits.
4. Click inside any section to **focus** it (purple border). Its label, duration, and index appear in the status text.
5. With a section focused, all edit operations target that section only:
   - **Trim** — keeps only the focused section, discards the rest.
   - **Silence** — silences the section.
   - **Delete / Backspace** — removes the section and closes the gap (adjusts adjacent split markers automatically).
   - **Fade In / Fade Out** — applies the fade within the section boundaries.
   - **Duplicate** — copies the section and appends it immediately after.
   - **Ctrl+X / Ctrl+C / Ctrl+V** — cut, copy, paste within the context of the section.
6. Split data is saved to `<audiofile>.splits` next to the audio. It is loaded automatically when the editor reopens.

### Auto-Split workflow

1. Click **Auto-Split**.
2. Choose **Auto-detect** or a fixed section count (2, 4, 8, or 16).
3. Click **Analyze & Split**. Analysis runs on the audio thread (wait cursor shown).
4. Sections are labelled automatically: **Intro**, **Verse**, **Buildup**, **Drop**, **Break**, **Outro**.
5. The first section is always labelled **Intro** and the last **Outro** when the analysis finds them to be quiet or ambient.
6. Click any section to focus it and use it as the target for edit operations.

### Paste and Split interaction

When you paste audio (**Ctrl+V**), the pasted region is automatically fenced by two new split markers so it can be selected and acted on independently. The pasted section is pre-focused after the paste.

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| **Space** | Play / Stop |
| **Home** | Return cursor to position 0, stop playback |
| **S** | Split at paste cursor |
| **Delete** or **Backspace** | Delete focused split section (if section focused), or delete selection (if selection active) |
| **Ctrl+A** | Select all (entire sample) |
| **Ctrl+C** | Copy selection (or whole sample) |
| **Ctrl+X** | Cut selection (or whole sample) |
| **Ctrl+V** | Paste at paste cursor |
| **Ctrl+Z** | Undo last edit |
| **Ctrl+Y** | Redo |

---

## Info Bar

The info bar at the bottom shows (left to right):

| Item | Colour | Shown when |
|---|---|---|
| Filename (truncated to 35 chars) | Gold, bold | A file is loaded |
| `Duration: Xs` | Dim | Always |
| `Offset: +Xs` | Dim | Sample offset != 0 |
| `BPM: NNN` | Dim | BPM was detected or entered |
| `Sel: Xs - Ys  (Zs)` | Dim | A selection is active |
| Hover hint | Gold | Mouse is over a toolbar button |

---

## Tips

- Use **Trim** after a rubber-band selection to destructively crop the file to just what you want — faster than re-loading a different file.
- Use **Auto-Split** on a long DJ mix or stem export to automatically get labelled sections you can then trigger individually from the clip grid.
- **Tape Slow / Tape Fast** applied repeatedly produce lo-fi pitch artefacts useful for effects. Undo chain lets you compare multiple steps.
- The **Nudge** buttons are non-destructive — they shift the playback offset without touching the file. Use them to align a loop's first transient with the bar grid when the file has a few milliseconds of pre-roll silence.
- **Snap** + a coarse Q setting (1 Bar) makes it easy to place split markers exactly on bar boundaries when the BPM is set correctly.
- **WSOLA** (the default stretch algorithm) is good for drums and percussion. Use **Phase Vocoder** for sustained pads, strings, or vocals where transient sharpness matters less than pitch smoothness.
- After a **Paste**, the pasted region is automatically wrapped in split markers so you can immediately Trim, Delete, or re-examine it without losing the surrounding audio.

---

> See **[docs/Manual.md](Manual.md)** for the full application manual including the Sample Editor's place in the broader workflow.
