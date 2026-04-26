# GrooviXBeat — User Manual

GrooviXBeat is a native desktop groovebox and sequencer built in JUCE C++. It combines the clip-launching workflow with a built-in piano roll, waveform editor, VST3 support, and a live performance grid — all in a single self-contained application with no browser or web layer.

---

## Contents

1. [Quick Start](#1-quick-start)
2. [Main Interface](#2-main-interface)
3. [Live Performance Grid](#3-live-performance-grid)
   - [3a. Clip Grid](#3a-clip-grid)
   - [3b. Dropping Audio & MIDI Files](#3b-dropping-audio--midi-files)
   - [3c. Auto-Q (Auto-Stretch)](#3c-auto-q-auto-stretch)
   - [3d. Clip Drag & Drop](#3d-clip-drag--drop)
   - [3e. Track Types](#3e-track-types)
   - [3f. Track Properties Dialog](#3f-track-properties-dialog)
   - [3g. Scene Properties Dialog](#3g-scene-properties-dialog)
   - [3h. Mixer](#3h-mixer)
   - [3i. Status Bar](#3i-status-bar)
4. [Piano Roll Editor](#4-piano-roll-editor)
   - [4a. Toolbar](#4a-toolbar)
   - [4b. Note Editing](#4b-note-editing)
   - [4c. Automation Lane](#4c-automation-lane)
   - [4d. Scale Filter](#4d-scale-filter)
   - [4e. Info Bar](#4e-info-bar)
5. [Sample Editor](#5-sample-editor)
   - [5a. Toolbar](#5a-toolbar)
   - [5b. Waveform View](#5b-waveform-view)
   - [5c. Selection & Editing](#5c-selection--editing)
   - [5d. BPM Detection & Stretch](#5d-bpm-detection--stretch)
   - [5e. Info Bar](#5e-info-bar)
6. [Generator](#6-generator)
7. [FX Chain Editor](#7-fx-chain-editor)
8. [VST3 Plugins](#8-vst3-plugins)
9. [Project Management](#9-project-management)
10. [Tips & Keyboard Shortcuts](#10-tips--keyboard-shortcuts)

---

## 1. Quick Start

1. Launch GrooviXBeat.
2. Set the project tempo in the toolbar (BPM field).
3. Click a track header to open Track Properties and choose the track type (Sample, Melody, Sampled Instrument, or Drum Kit).
4. Drop an audio file onto a clip cell, or click a clip cell to open the Piano Roll / Sample Editor and add content.
5. Click the Play button (or press **Space**) to start playback.
6. In Performance Mode, click clip cells to trigger individual clips live.

---

## 2. Main Interface

The interface is divided into two areas:

**Top Toolbar**
Contains transport controls (Play / Stop / Record), tempo, time signature, quantize setting, and mode buttons (Song Mode / Performance Mode). The **Auto-Q** button toggles automatic BPM stretch on file load.

**Live Performance Grid**
The main working area — a scrollable grid of clip cells organised by scenes (rows) and tracks (columns). The mixer panel sits below the grid.

---

## 3. Live Performance Grid

### 3a. Clip Grid

| Axis | Label |
|------|-------|
| Rows | Scenes (S1, S2, …) |
| Columns | Tracks (T1, T2, …) |

Each cell is a clip. A clip can hold:
- **Audio sample data** — sample / sampled-instrument / drum-kit tracks
- **MIDI note data** — melody tracks

**Clip states shown visually:**

| State | Visual |
|-------|--------|
| Empty | Dark cell, no content |
| Has content | Waveform thumbnail (audio) or note bars (MIDI) |
| Playing | Highlighted border, moving playhead |
| Queued | Pulsing indicator (waiting for next quantize boundary) |
| Muted | Dimmed |

**Song Mode**
Click the scene play button (left of each row) to trigger that scene. Scenes play in sequence from top to bottom. Repeat count and other per-scene settings are controlled via the Scene Properties dialog.

**Performance Mode**
Click any clip cell to trigger it independently. Clips launch at the next quantize boundary set in the toolbar. Multiple tracks can play different clips simultaneously.

---

### 3b. Dropping Audio & MIDI Files

**Audio files** — `.wav` `.aif` `.aiff` `.mp3` `.ogg` `.flac` `.m4a` `.caf`

Drop one or more audio files directly onto a clip cell.

| Drop | Behaviour |
|------|-----------|
| Single file | Loads into the target cell. Status bar shows **Loading…** in yellow. Wait cursor shown. |
| Multiple files | Each file fills the next scene down in the same track (scenes created automatically if needed). Status bar shows **Processing N files…** in red. Wait cursor shown. |

**MIDI files** — `.mid` `.midi`

Drop a MIDI file onto a melody-type track cell. Notes are imported and the clip length is extended to fit.

---

### 3c. Auto-Q (Auto-Stretch)

The **Auto-Q** button in the top toolbar toggles automatic tempo matching.

When Auto-Q is **ON** and a file with a detectable BPM is loaded (BPM from filename such as `loop_120bpm.wav`, or embedded ACID metadata), GrooviXBeat will:

1. WSOLA time-stretch the audio to match the project tempo.
2. Snap the clip length to the nearest bar count.
3. Save the stretched audio to disk alongside the project.

This works for both drag-and-drop onto the grid and loading via the Sample Editor's Load button. Files without a detectable BPM are loaded as-is.

---

### 3d. Clip Drag & Drop

**Copy a clip**
Click and drag a clip cell to another cell to copy it. The source clip remains unchanged. The destination receives a full copy of the clip data including sample path and MIDI notes.

**Clear a clip — Trash Zone**
During a drag, a **CLEAR** trash zone appears above the mixer. Drop the clip onto this zone to clear it. A wait cursor is shown while the operation runs. The status bar confirms the cleared cell.

---

### 3e. Track Types

| Type | Description |
|------|-------------|
| **Sample** | Plays an audio file. Drop audio onto a cell or use the Sample Editor. |
| **Melody (MIDI)** | Sends MIDI to a VST3 instrument or external MIDI device. Use the Piano Roll to program notes. Supports live MIDI input. |
| **Sampled Instrument** | Multi-sample instrument player. Select via Track Properties VST selector. |
| **Drum Kit** | MIDI-triggered drum kit. Each pitch maps to a different drum sound. Use the Piano Roll to program patterns. |

---

### 3f. Track Properties Dialog

Click a track header to open the Track Properties dialog.

| Field | Description |
|-------|-------------|
| **Name** | Rename the track. |
| **Track Type** | Sample / Melody (MIDI) / Sampled Instrument / Drum Kit. Changing type resets clip content. |
| **Playback Mode** | Loop (repeats continuously) or One Shot (plays once). |
| **VST Instrument** *(Melody only)* | Search and select a loaded VST3 instrument. Click **VST UI** to open the plugin editor. |
| **MIDI Input** *(Melody only)* | Select an external MIDI input device and channel. **Remap C Major to target scale** transposes incoming notes to the scale set in the Piano Roll. |

**Action buttons:**

| Button | Action |
|--------|--------|
| Move Left / Move Right | Reorder the track in the grid. |
| Clone | Duplicate the track and all its clips. |
| Delete | Permanently remove the track. |
| Clear All Clips in Track | Empty every clip cell in the track. |
| Save / Cancel | Apply or discard changes. |

---

### 3g. Scene Properties Dialog

Click a scene button (left of each row) to open the Scene Properties dialog.

| Field | Description |
|-------|-------------|
| **Name** | Rename the scene. |
| **Signature** | Time signature: 4/4, 3/4, 6/8, 2/4. |
| **Repeat** | How many times the scene loops before advancing (Song Mode). |
| **Quantize** | Launch quantize override: Inherit / 1/16 / 1/4 / 1/2 / 1 Bar. |
| **Fade In / Fade Out** | Apply a volume fade at scene start or end. |

**Action buttons:**

| Button | Action |
|--------|--------|
| Move Up / Move Down | Reorder the scene in the grid. |
| Clone | Duplicate the scene and all its clips. |
| Delete | Permanently remove the scene. |
| Clear All Clips in Scene | Empty every clip cell in the scene. |
| Save / Cancel | Apply or discard changes. |

---

### 3h. Mixer

The mixer panel at the bottom of the Live Performance window has one channel strip per track plus a Master channel.

Each channel strip has:
- **Volume fader**
- **Pan knob**
- **M** (Mute) — silences the track
- **S** (Solo) — solos the track; all others are muted
- **Level meter**

The Master channel controls the overall output volume.

---

### 3i. Status Bar

A thin bar at the very bottom of the Live Performance window shows status messages colour-coded by state:

| Colour | Meaning |
|--------|---------|
| Dim | Idle / ready |
| Yellow | Busy / loading a single file |
| Red | Processing multiple files simultaneously |

---

## 4. Piano Roll Editor

Open by clicking a clip cell on a melody, drum-kit, or sampled-instrument track.

### 4a. Toolbar

**Left side — icon buttons:**

| Button | Description |
|--------|-------------|
| **Filter** (eye icon) | Toggle: hide notes outside the selected scale. |
| **Select** | Toggle: switch to selection mode. Click and drag to select a range of notes; move, copy, or delete as a group. |
| **Automation** | Toggle: show/hide the automation lane below the note grid. |
| **VST UI** | Open the VST instrument's own editor window *(melody tracks only)*. |
| **Load MIDI** | Import notes from a `.mid` file into the current clip. |
| **Clear** | Remove all notes from the current clip. |
| **Generate** | Open the Generator to create chord progressions, arpeggios, or melody patterns. |

**Right side — dropdowns:**

| Control | Description |
|---------|-------------|
| Play Mode | Loop or One Shot. |
| Quantize | Note quantize resolution for playback. |
| Scale Root | Root note of the active scale (C – B). |
| Scale Type | Scale type (Major, Minor, Pentatonic, etc.). |
| Length | Clip length in bars (1 – 16 bars). |
| **Close** | Close the Piano Roll and return to the grid. |

---

### 4b. Note Editing

The piano roll displays pitches vertically against time steps horizontally.

| Action | How |
|--------|-----|
| Add note | Click on an empty cell. |
| Move note | Click and drag the note body. |
| Resize note | Drag the right edge of a note. |
| Delete note | Right-click a note. |
| Copy / Paste | Select notes → **Ctrl+C** / **Ctrl+V**. |
| Duplicate | Select notes → **Ctrl+D** (offset by combined length). |
| Select all | **Ctrl+A**. |
| Delete selected | **Delete** or **Backspace**. |

---

### 4c. Automation Lane

Click the **Automation** button to reveal the automation lane below the note grid. Automatable parameters:

- Pitch Bend
- Modulation (CC1)
- Pan (CC10)
- Any VST parameter exposed by the loaded instrument

Draw automation values by clicking and dragging in the lane.

---

### 4d. Scale Filter

Set a **Scale Root** and **Scale Type** from the toolbar dropdowns. Toggle the **Filter** (eye) button to hide all notes outside the scale — remaining rows are highlighted, making it easy to stay in key.

The **Remap C Major to target scale** option in Track Properties transposes incoming keyboard notes from C Major to the selected scale in real time.

---

### 4e. Info Bar

| Side | Content |
|------|---------|
| Left | Track type and loaded VST instrument name (dim text) |
| Right | Hint text for the button or dropdown under the mouse (gold) |

---

## 5. Sample Editor

Open by clicking a clip cell on a sample track, or by clicking a waveform thumbnail in an existing cell.

### 5a. Toolbar

**Left side — icon buttons:**

| Button | Description |
|--------|-------------|
| **Load** | Open a file browser to load an audio file. |
| **Nudge Left** | Shift the sample start point left. Hold **Shift** for 1/16-note steps; hold **Ctrl** for 1-bar jumps. |
| **Nudge Right** | Shift the sample start point right. |
| **Trim** | Crop the audio to the current selection (or entire file if no selection). |
| **Fade In** | Apply a linear fade-in over the selection. |
| **Fade Out** | Apply a linear fade-out over the selection. |
| **Silence** | Replace the selection with silence. |
| **Cut** | Remove the selection and close the gap. |
| **Copy** | Copy the selection to the clipboard. |
| **Paste** | Paste clipboard audio at the paste cursor position. |
| **Duplicate** | Append a copy of the selection to the end. |
| **Undo** | Undo the last edit. |
| **Redo** | Redo the last undone edit. |
| **Reset** | Revert all edits to the original loaded file. |
| **Zoom to Fit** | Zoom the waveform view to show the entire audio file. |
| **BPM** | Open the BPM detection / stretch dialog. |
| **Clear Clip** | Remove this clip entirely from the grid. |

**Right side — dropdowns:**

| Control | Description |
|---------|-------------|
| Play Mode | Loop or One Shot. |
| Quantize | Launch quantize override for this clip. |
| Length | Clip length in bars. |
| **Close** | Close the Sample Editor and return to the grid. |

---

### 5b. Waveform View

The waveform is displayed below the toolbar:

| Element | Description |
|---------|-------------|
| Blue waveform | Left channel |
| Green waveform | Right channel |
| Time ruler | Across the top of the waveform area |
| Hatched grey region | Silence beyond the audio end (within clip boundary) |
| Green line | Playhead tracking current playback position |

| Interaction | How |
|-------------|-----|
| Zoom | Mouse wheel in/out around the cursor position |
| Pan | Horizontal scrollbar at the bottom |
| Selection | Click and drag (shown in gold) |
| Paste cursor | Single click without dragging (teal line) |

---

### 5c. Selection & Editing

Most edit operations act on the current selection. If no selection exists, the operation acts on the entire audio file. After each edit, the waveform reloads automatically and peaks are recalculated. The undo/redo stack is per-session and clears when a new file is loaded.

---

### 5d. BPM Detection & Stretch

Click the **BPM** button to open the BPM dialog. GrooviXBeat shows:
- Detected BPM (from filename, ACID metadata, or DSP analysis)
- Detection source

Click **Stretch to project BPM** to WSOLA time-stretch the audio to the project tempo. The stretched file is saved to disk and the clip length is snapped to the nearest bar count.

> The BPM button is enabled whenever a file is loaded, regardless of whether a BPM was detected.

---

### 5e. Info Bar

| Side | Content |
|------|---------|
| Left | Filename (gold, bold) + duration, offset, detected BPM, and selection range (dim text) |
| Right | Hint text for the button or dropdown under the mouse (gold) |

---

## 6. Generator

Click the **Generate** button (import-wizard icon) in the Piano Roll toolbar to open the Generator. It can produce:

| Mode | Description |
|------|-------------|
| **Chords** | Choose a root, quality, and voicing to insert a chord. |
| **Arpeggios** | Generate arpeggiated patterns from a chord selection. |
| **Melody** | Algorithmic melody generation based on scale and rhythm parameters. |

Generated notes are inserted into the current clip at the playhead position and can be edited normally in the Piano Roll.

---

## 7. FX Chain Editor

Right-click a track header and choose **FX Chain** to open the FX Chain Editor.

Add VST3 effect plugins (compressors, reverbs, EQs, etc.) in series for that track. The signal flows through each plugin in order before reaching the master output. Each plugin slot shows the plugin name and a bypass toggle. Drag slots to reorder the chain.

---

## 8. VST3 Plugins

**Instruments**
Assigned per track via Track Properties → VST Instrument. The search box filters by name or manufacturer. Click **VST UI** to open the plugin's graphical editor.

**Effects**
Added per track via the FX Chain Editor (see [Section 7](#7-fx-chain-editor)).

**Plugin Scanning**
GrooviXBeat scans for installed VST3 plugins on startup. Plugins must be installed in the standard VST3 folder for your system.

**Automation**
VST parameters can be automated per note in the Piano Roll automation lane. Select the parameter from the dropdown in the automation lane.

---

## 9. Project Management

| Action | How |
|--------|-----|
| **Save** | File → Save or **Ctrl+S**. Saves all clip data, MIDI notes, mixer settings, sample edits, plugin assignments, and scene/track layout. |
| **Load** | File → Open or **Ctrl+O**. Restores the complete session including all plugin states. |
| **New** | File → New. Clears all data and resets to the default layout. |

**Audio files**
When you drop or load audio files, GrooviXBeat copies them into the project's samples folder so the project remains self-contained and portable. Edited samples (trimmed, stretched, etc.) are also saved to disk within the project folder.

---

## 10. Tips & Keyboard Shortcuts

### Transport

| Shortcut | Action |
|----------|--------|
| `Space` | Play / Stop |

### Piano Roll

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Select all notes |
| `Ctrl+C` | Copy selected notes |
| `Ctrl+V` | Paste notes |
| `Ctrl+D` | Duplicate selected notes |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Delete` / `Backspace` | Delete selected notes |

### Sample Editor

| Shortcut | Action |
|----------|--------|
| `Ctrl+Z` | Undo last edit |
| `Ctrl+Y` | Redo |
| `Mouse wheel` | Zoom waveform in / out |

### Live Performance Tips

- Hold **Shift** while nudging a sample to move by 1/16-note steps.
- Hold **Ctrl** while nudging to move by 1 bar at a time.
- Drop multiple audio files at once onto a track to fill consecutive scenes in one operation.
- Enable **Auto-Q** before dropping loops with BPM in the filename — they will stretch to your project tempo automatically.
- Drag a clip cell to the **CLEAR** zone above the mixer to delete it quickly.
- The status bar colour tells you what the app is doing:

| Colour | Meaning |
|--------|---------|
| Dim | Idle |
| Yellow | Loading a file |
| Red | Processing multiple files (hourglass cursor shown) |
