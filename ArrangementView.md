# Live Performance & Arrangement View -- User Manual

The **Live Performance / Arrangement** view is the main session window. It has two modes that share the same toolbar and mixer strip:

- **Performance Mode** -- a scene/clip grid for live triggering and composition.
- **Arrangement Mode** -- a time-based timeline for arranging scenes into a linear song.

Switch between modes using the **PERF | ARR** toggle at the left of the toolbar.

---

## Layout Overview

![A mushroom-head robot drinking bubble tea](GroovixBeat_01.png)

```
+--------------------------------------------------------------+
| Toolbar  [mode][AUTO][Play][Stop][Click][BPM][Q][AutoQ]...  |
+------+--------+--------+--------+------------------------+--+
|      | T1     | T2     | T3     |      ...               |  |
| Scn  +--------+--------+--------+                        |V |
|  1   | clip   | clip   | clip   |  (tracks scroll right) |S |
+------+--------+--------+--------+                        |B |
| Scn  | clip   | clip   | clip   |                        |  |
|  2   |        |        |        |                        |  |
+------+--------+--------+--------+------------------------+--+
|  [+] |                                 H Scrollbar          |
+------+------ Mixer strip -------------------------+----------+
|      | Vol Pan M S Fx | Vol Pan M S Fx | ...      | Master  |
+------+-------------------------------------------------+----+
| Status bar                                              |    |
+----------------------------------------------------------+---+
```

In **Arrangement Mode** the grid is replaced by a horizontal time axis:

```
+------+------- Time ruler (bar numbers, beat ticks) ----------+
| T1   | [waveform clip][   clip   ]     [clip]                |
| T2   |      [clip]                  [clip][clip]             |
| T3   |                                                       |
+------+-------------------------------------------------------+
```

---

## Toolbar

The toolbar is a single 36 px row at the top. Buttons appear left to right.

---

### PERF | ARR (Mode Toggle)

An 80 px button split into two halves:

| Half | Action |
|---|---|
| **PERF** (left) | Switch to Performance Mode (clip grid) |
| **ARR** (right) | Switch to Arrangement Mode (timeline) |

The active mode half is highlighted. Switching modes stops any current playback and resets the scroll position.

---

### AUTO (Auto-Advance)

Available in **Performance Mode** only. When active (gold background), the scene sequencer plays all scenes in order from top to bottom, advancing at each scene's natural length boundary. When auto-advance is active:

- The **Play** button (or **Space**) starts playback from Scene 1.
- Clicking a scene button jumps to and starts that scene.
- Individual clip cell triggers are disabled.

When AUTO is off, clips are triggered freely per track.

---

### Play

Starts playback. Behavior depends on the current state:

| State | Action |
|---|---|
| Performance Mode, AUTO off | Starts the live transport; clips play when triggered |
| Performance Mode, AUTO on | Starts scene auto-advance from Scene 1 |
| Arrangement Mode | Pre-renders all sample tracks and starts the arrangement timeline |
| Editor overlay open | Starts single-clip preview from the seek cursor |

If transport is already running, Play does nothing.

---

### Stop

Stops all playback immediately. Clears the playhead. Queued and playing clips are stopped.

---

### Metronome (Click)

Toggles the click track on/off. When active (gold background), the metronome plugin generates a beat on every quarter note at the current project tempo.

---

### BPM (Tempo)

A small editable label showing the current project BPM (20-300). Double-click to edit; press Enter or click away to commit. Changes are applied immediately to the MIDI scheduler. Integer values only.

---

### Q (Global Quantize)

Sets the **launch quantise** for all clips that use "Inherit":

| Option | Steps | Meaning |
|---|---|---|
| **1/16** | 1 | Clips start on the next 1/16th note |
| **1/4** | 4 | Clips start on the next beat |
| **1/2** | 8 | Clips start on the next half-bar |
| **1 Bar** | 16 | Clips start on the next bar (default) |

Also determines the snap grid in **Arrangement Mode** for clip placement and paste cursor positioning.

---

### Auto-Q (Auto-Quantize)

When active (gold background), audio files dropped onto the grid are automatically:

1. BPM-detected from the filename or ACID metadata.
2. WSOLA time-stretched to the project tempo.
3. Clip length snapped to the nearest bar count.

When off, files are loaded as-is at their original tempo.

---

### Pencil (Draw Mode)

Activates **Draw/Pencil mode** (the default). When clicked, it deactivates any active Select mode in the current context:

- Deactivates SEL mode on the clip grid or the Clip/Sample Editor overlay.
- Pencil is active (gold background) when SEL is not.

---

### SEL (Select Mode)

Activates **Select Mode**. Behavior depends on context:

| Context | Effect |
|---|---|
| **Clip Editor overlay** | Activates rubber-band note selection in the piano roll |
| **Sample Editor overlay** | Activates waveform range selection |
| **Performance grid** | Click a cell, scene button, or track header to select it (purple highlight) |
| **Arrangement timeline** | Click a clip to select it (purple highlight); Ctrl+click for multi-selection |

When SEL is active, Cut / Copy / Paste / Clear / Label operate on the current selection.

---

### Cut (Scissors icon)

Cuts the current selection to the clipboard. Context-aware:

- **Piano Roll**: cuts selected notes.
- **Sample Editor**: cuts the selected waveform region.
- **Arrangement** (SEL + clips selected): removes selected arranged clips and stores them for paste.
- **Performance grid** (SEL + selection): cuts the selected clip, scene, or track.

---

### Copy

Copies the current selection to the clipboard without removing it. Same context rules as Cut.

---

### Paste

Pastes the clipboard contents. In **Arrangement Mode**, clips are placed starting at the paste cursor (white dashed line), offset so the earliest clip in the clipboard lands at the cursor.

---

### Clear (Trash icon)

Removes the current selection. Context-aware:

| Context | Action |
|---|---|
| Clip Editor, notes selected | Deletes selected notes |
| Sample Editor, region selected | Silences the selected region |
| Arrangement, clips selected | Deletes selected arranged clips |
| Arrangement, track selected (no clips) | Confirmation dialog: removes all clips from that track's timeline |
| Performance grid, selection | Confirmation dialog or immediate delete |

---

### LBL (Label)

Opens a text dialog to label the selected item. Context-aware:

| Context | What is labelled |
|---|---|
| Clip / Sample Editor open | Labels the clip currently open in the editor |
| Arrangement SEL mode | If the paste cursor is inside a section marker: edits that marker's label. Otherwise creates a new section marker at the cursor spanning one bar. |
| Performance SEL + clip selected | Labels the selected clip cell |

Labels appear in **gold** on the clip thumbnail. Unlabelled clips show the scene name in white.

---

### WIZ (Arrangement Wizard)

Available in **Arrangement Mode** only. Opens the **Arrangement Wizard** dialog, which helps build an arrangement automatically from the available scenes.

---

### Zoom In / Zoom Out / Zoom Fit

Horizontal zoom controls. Behavior depends on context:

| Context | Action |
|---|---|
| **Clip Editor overlay** | Zooms the piano roll |
| **Sample Editor overlay** | Zooms the waveform |
| **Arrangement Mode** | Zooms the timeline (0.125× to 8×, pivoting around the cursor) |
| **Performance Mode** | Buttons are present but not functional (the grid has fixed cell sizes) |

**Zoom Fit** in Arrangement Mode calculates the zoom needed to show all scenes within the visible width and resets the scroll to position 0.

---

### Refresh (far right)

Forces a full re-sync of the live performance data from `NativeAppState`. Use after making changes in a sub-dialog that doesn't auto-refresh the grid.

---

## Performance Mode

### Grid Structure

```
+------+--------+--------+--------+
|      | T1     | T2     | T3     |  ← Track headers (30 px tall)
+------+--------+--------+--------+
| S1 ▲ | clip   | clip   | clip   |  ← Scene 1
+------+--------+--------+--------+
| S2 ▲ |        | clip   |        |  ← Scene 2
+------+--------+--------+--------+
| [+]  |                           |  ← Add Scene button
```

- **Track headers** (top row, one column per track): show track name with a coloured left-edge stripe.
- **Scene buttons** (left column, 36 px wide): each scene row has a play/props button.
- **Clip cells** (96×48 px): show a waveform preview (audio clips) or a note preview (MIDI clips).
- **+ Track** button appears in the header row above the master fader column. Maximum 12 tracks.
- **+ Scene** button appears below the last scene row. Maximum 20 scenes.

---

### Scene Buttons

Each scene button is split vertically:

| Half | Action |
|---|---|
| **Top half** (play triangle) | Launches all clips in this scene (stops clips from other scenes first). Starts transport if not running. |
| **Bottom half** (properties) | Opens the Scene Properties dialog. |
| **Right-click** | Opens the scene context menu. |

**Auto-advance**: when AUTO is on, clicking the top half starts auto-advance from that scene.

---

### Clip Cells

| Interaction | Transport off | Transport on |
|---|---|---|
| **Single click** | Opens the Clip Editor (MIDI/drum) or Sample Editor (audio) | Triggers / toggles the clip (queued then confirmed at the next quantise boundary) |
| **Double-click** | Opens the editor | Opens the editor |
| **Right-click** | Context menu | Context menu |
| **Drag to another cell** | Copies the clip to the destination (cross-type drags rejected) | -- |
| **Drag to trash zone** (above master fader) | Clears the clip | -- |

**Clip state colours:**

| State | Visual |
|---|---|
| Empty | Dark, dim |
| Has content | Coloured (waveform or note preview) |
| Playing | Bright with a green playhead line |
| Queued to start | Dimmer gold ring |
| Queued to stop | Red border |

---

### Track Headers

| Interaction | Action |
|---|---|
| **Left-click** | Opens Track Properties dialog |
| **Right-click** | Opens track context menu (rename, type, clone, delete, clear track) |
| **Double-click** (SEL mode) | Opens Track Properties dialog |

---

### Scene Properties Dialog

Right-click a scene button and choose Properties, or click its bottom half.

| Field | Description |
|---|---|
| Scene Name | Text name for this scene |
| Quantize | Per-scene launch quantise (Inherit / 1/16 / 1/4 / 1/2 / 1 Bar) |
| Fade In | Cross-fades this scene in from the previous one |
| Fade Out | Cross-fades this scene out into the next one |
| Move Up / Move Down | Reorders scenes in the grid |
| Clone | Duplicates this scene (all clips, metadata) below it |
| Delete | Removes the scene (confirmation required) |
| Clear All Clips | Empties every clip cell in this scene (confirmation required) |

---

### Track Properties Dialog

Left-click a track header.

| Field | Description |
|---|---|
| Name | Text name for the track |
| Track Type | Sample, Melody (VST), Drum Kit, Sampled Instrument |
| Playback Mode | Loop or One-shot |
| VST Instrument | (Melody tracks only) searchable list of loaded plugins |
| MIDI Device / Channel | (Melody tracks) MIDI input routing |
| Remap C Major | (Melody tracks) maps MIDI input from C major to the track's target scale |
| Move Left / Move Right | Reorders the track |
| Clone | Duplicates the track |
| Delete | Removes the track and all its clips (confirmation required) |
| Clear Track | Clears all clip cells for this track (confirmation required) |

---

### File Drag & Drop (Performance Mode)

Drag audio files (WAV, AIF, AIFF, MP3, OGG, FLAC) from Explorer onto clip cells:

- **Single file**: loads into the target cell. A wait cursor is shown during load.
- **Multiple files**: the first file goes into the target cell; subsequent files fill consecutive scenes downward in the same track column.
- **Auto-Q**: if enabled and the file has a detectable BPM (from filename or ACID metadata), the file is WSOLA-stretched to the project tempo and the clip length snapped to the nearest bar.
- **Cross-type rejection**: audio files on MIDI tracks (and vice versa) are rejected; a status message appears.

---

## Arrangement Mode

### Timeline Layout

```
+------+---- Time ruler ----+------------------------------------+
|      | 1    2   3    4    | 5  6    7    8                     |
| T1   | [Verse clip.....] | [Chorus clip....]                  |
| T2   | [..bass line.....] |                 [break...]         |
| T3   |         [pad...]   |                                    |
+------+----------------------------------------------------+----+
```

- **Track labels** (left column, 36 px): show the track name with a coloured stripe. Click to select the track for clip painting.
- **Time ruler** (30 px): bar numbers + beat ticks. Click or drag to set the paste cursor.
- **Section marker bands**: coloured regions in the ruler for named sections (Intro, Verse, Chorus, etc.).
- **Clip blocks**: placed clips from the Performance grid, showing waveform or note preview.

---

### Visual Indicators

| Element | Colour | Meaning |
|---|---|---|
| Playhead | Green line + downward triangle | Current playback position |
| Paste cursor | White dashed line + upward triangle | Where the next clip placement or paste will land |
| Cursor position label | White, small, beside triangle | Bar.Beat position (e.g. "4.3") |
| Selected clip | Purple fill + purple border (2px) | Clip is in the current selection |
| Section markers | Steel blue / olive / muted red / amber / purple / teal | Named song sections in the ruler |
| Q snap highlight | Purple band in ruler | Snap zone for the current quantize grid (SEL mode) |
| Resize handle | Right 8 px of a clip | Drag to shorten the clip's length override |

---

### Placing Clips (Draw / Pencil Mode)

1. Click a **track label** on the left to select that track. The **Scenes** combo in the toolbar updates to show all non-empty clips for that track.
2. Use the **Scenes** combo to choose which scene's clip to paint.
3. **Left-click** anywhere in that track's timeline row to place a clip at the clicked position, snapped to the current Q grid.
4. **Left-click and drag** across the row to paint multiple clip instances (each snapped bar that is empty gets a copy).
5. **Right-click** a placed clip to erase it.

Clip placement is rejected if it would overlap an existing clip on the same track.

---

### Moving and Resizing Clips

| Interaction | Action |
|---|---|
| **Drag clip body** | Moves the clip to a new bar position (snapped to Q) |
| **Drag right edge** (last 8 px) | Shortens the clip's active length (clipped at source length, capped before the next clip) |
| **Right-click** | Removes the clip from the timeline |
| **Double-click** | Opens the Clip Editor or Sample Editor for the source scene/track |

---

### Paste Cursor

The white dashed line is the paste cursor. It determines where **Paste** (Ctrl+V) places copied clips, and also where the **LBL** button creates or moves a section marker.

- **Click** anywhere in the grid or ruler: repositions the cursor (snapped to Q).
- **Drag** in the ruler: scrubs the cursor while dragging.
- The cursor always snaps to the nearest Q-grid boundary.

---

### Section Markers

Section markers are labelled bands in the ruler that span one or more bars.

| Action | How to do it |
|---|---|
| Add marker | Position the paste cursor, then click **LBL** (or press the LBL button). If no existing marker contains the cursor, a new 1-bar marker is created. |
| Edit label | Double-click an existing marker band in the ruler, or position the cursor inside it and click **LBL**. |
| Remove marker | Edit the label to blank and save |

**Marker colours** (assigned in sequence):

| Colour | Typical sections |
|---|---|
| Steel blue | Intro, Outro |
| Olive green | Verse, Break |
| Muted red | Chorus, Drop |
| Amber | Buildup, Bridge |
| Purple | Pre-chorus, Transition |
| Teal | Outro B |

---

### Selecting Clips (Arrangement SEL Mode)

When **SEL** is active in Arrangement Mode:

- **Click** a clip: selects it (purple highlight). Previous selection cleared.
- **Ctrl+click**: adds or removes the clip from the selection without clearing others.
- **Click empty space**: clears the selection.
- **Click track label**: selects the track (clears individual clip selection). Delete/Clear will then target all clips in that track's timeline.

---

### Arrangement Playback

1. Click **Play** (or press **Space**). The view shows a wait cursor briefly while sample tracks are pre-rendered.
2. The green playhead advances left to right across the timeline.
3. The arrangement ends automatically when the playhead passes the last placed clip.
4. **Stop** (or **Space**) halts the playhead immediately.

Sample tracks are pre-rendered into a per-track audio buffer before playback starts. MIDI tracks are scheduled directly by the MIDI engine using merged note streams.

---

### Zoom in Arrangement Mode

| Action | Result |
|---|---|
| **Ctrl + scroll wheel** | Zoom in / out, pivoting around the mouse X position |
| **Scroll wheel** | Scroll the timeline left / right (~3 bars per notch) |
| **Zoom In button** | ×1.5 zoom in |
| **Zoom Out button** | ÷1.5 zoom out |
| **Zoom Fit button** | Fit all content into the visible width |

Zoom range: **0.125×** (most zoomed out) to **8×** (most zoomed in).
Timeline total length: **128 bars**.
Bar numbers are drawn in the ruler when a bar is at least 14 px wide; beat ticks when at least 6 px wide.

---

## Mixer Strip

The mixer strip is always visible below the clip grid, 142 px tall.

### Per-Track Channel Strip

Each track has (left to right within its column):

| Control | Description |
|---|---|
| **Level meters** (L/R) | Real-time peak meters with ~0.85 decay per 60 Hz tick |
| **Volume fader** | Vertical fader, 0-100% |
| **Pan slider** | Centre-out slider. Drag left/right. Double-click to reset to centre. |
| **M (Mute)** | Mutes the track |
| **S (Solo)** | Solos the track (all others are muted) |
| **Fx** | Opens the FX Chain editor for this track |

### Master Channel

The rightmost strip (76 px wide):

| Control | Description |
|---|---|
| **Level meters** (L/R) | Master output meters |
| **Master Fader** | Master output volume |
| **Master Pan** | Master output pan |
| **M (Mute)** | Mutes master output |
| **Fx** | Opens the master FX chain |

The trash drop zone (drag clip here to clear it) appears above the master fader during a clip drag operation.

In **Arrangement Mode**, the mixer strip scrolls horizontally independently of the timeline — use Shift+Scroll or drag the mixer scrollbar.

---

## Status Bar

The 20 px status bar at the bottom of the view shows the current state:

| State | Colour | Example |
|---|---|---|
| Idle / ready | Dim | "Ready" |
| Loading | Yellow/gold | "Preloading 5 sample(s)..." |
| Processing multiple files | Red | "Processing 3 files..." |
| Playing (performance) | Dim | "Playing" or "Scene 3" |
| Syncing clip to boundary | Dim | "Syncing..." |

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| **Space** | Play / Stop (same as toolbar Play/Stop) |
| **Ctrl+X** | Cut (applies to current selection context) |
| **Ctrl+C** | Copy |
| **Ctrl+V** | Paste (at paste cursor in arrangement; after last note in piano roll) |
| **Ctrl+D** | Duplicate selected clips (Arrangement SEL mode only) |
| **Delete** or **Backspace** | Delete selection (notes, waveform region, or arranged clips) |

---

## Clip Thumbnails

Clips display a preview inside their cell:

| Content | Preview type |
|---|---|
| Audio clip | Waveform (200-point peak graph, track colour) |
| MIDI clip | Note bars (mini piano roll, track colour) |
| Automation | Thin coloured lines per parameter, sampled at every beat |

**Automation overlay in Arrangement Mode**: each parameter's breakpoint curve is drawn as a 1.5 px line over the clip using the same 8-colour palette as the Clip Editor (sky blue, light green, deep orange, lavender, yellow, teal, pink, blue grey).

---

## Tips

- Use **Auto-Advance** to rehearse a full song in order without manual intervention. Click any scene button during auto-advance to skip to that scene.
- In **Arrangement Mode**, **Zoom Fit** is the fastest way to get an overview of the full arrangement after placing clips.
- Use **LBL** to mark Intro / Verse / Chorus / Drop etc. in the ruler; the coloured bands make navigation much easier during editing and live performance.
- The **Q combo** is the single most important control in Arrangement Mode: a coarser grid (1 Bar) makes rough layout fast; switch to 1/4 or 1/16 for fine-tuned transitions.
- When placing clips with **Auto-Q on**, files detected at the same BPM as the project land exactly on the bar grid. Files at different BPMs are stretched first.
- Drag the **right edge** of an arranged clip to shorten it, creating a natural loop-end that doesn't modify the source clip. The maximum length is always the source clip's natural length.
- **Ctrl+D** in SEL mode duplicates selected clips into the next available gap, preserving relative offsets -- faster than Paste for extending a repeating section.
- The **Scenes** combo in Arrangement Mode remembers the last-used scene per track so you can quickly switch between a verse and a chorus clip on the same track.

---

> See **[docs/Manual.md](Manual.md)** for the full application manual.
> See **[docs/ClipEditor.md](ClipEditor.md)** for the Piano Roll editor reference.
> See **[docs/SampleEditor.md](SampleEditor.md)** for the Sample/Waveform editor reference.
