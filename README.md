# GrooviXBeat — User Manual

GrooviXBeat is a native desktop groovebox and sequencer built in JUCE C++. It combines the clip-launching workflow of Ableton Live with a built-in piano roll, waveform editor, VST3 support, and a live performance grid — all in a single self-contained application with no browser or web layer.

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
4. [Arrangement Mode & Timeline](#4-arrangement-mode--timeline)
   - [4a. Switching to Arrangement Mode](#4a-switching-to-arrangement-mode)
   - [4b. Timeline View](#4b-timeline-view)
   - [4c. Placing Clips on the Timeline](#4c-placing-clips-on-the-timeline)
   - [4d. Section Markers](#4d-section-markers)
   - [4e. Arrangement Wizard](#4e-arrangement-wizard)
5. [Piano Roll Editor](#5-piano-roll-editor)
   - [5a. Toolbar](#5a-toolbar)
   - [5b. Note Editing](#5b-note-editing)
   - [5c. Automation Lane](#5c-automation-lane)
   - [5d. Scale Filter](#5d-scale-filter)
   - [5e. Info Bar](#5e-info-bar)
6. [Sample Editor](#6-sample-editor)
   - [6a. Toolbar](#6a-toolbar)
   - [6b. Waveform View](#6b-waveform-view)
   - [6c. Selection & Editing](#6c-selection--editing)
   - [6d. BPM Detection & Stretch](#6d-bpm-detection--stretch)
   - [6e. Info Bar](#6e-info-bar)
7. [Note Generator](#7-note-generator)
   - [7a. Chord / Arp Tab](#7a-chord--arp-tab)
   - [7b. Drum Patterns Tab](#7b-drum-patterns-tab)
   - [7c. Bass Lines Tab](#7c-bass-lines-tab)
   - [7d. Formula Tab](#7d-formula-tab)
8. [FX Chain Editor](#8-fx-chain-editor)
9. [VST3 Plugins](#9-vst3-plugins)
10. [Project Management](#10-project-management)
11. [Tips & Keyboard Shortcuts](#11-tips--keyboard-shortcuts)

---

## 1. Quick Start

1. Launch GrooviXBeat.
2. Set the project tempo in the toolbar (BPM field).
3. Click a track header to open Track Properties and choose the track type (Sample, Melody, Sampled Instrument, or Drum Kit).
4. Drop an audio file onto a clip cell, or click a clip cell to open the Piano Roll / Sample Editor and add content.
5. Click the Play button (or press **Space**) to start playback.
6. In **Performance Mode**, click clip cells to trigger individual clips live.
7. Switch to **Arrangement Mode** to lay out clips on a linear timeline.

---

## 2. Main Interface

The interface is divided into two areas:

**Top Toolbar**
Contains transport controls (Play / Stop / Record), tempo, time signature, quantize setting, and mode buttons (**Performance Mode** / **Arrangement Mode**). The **Auto-Q** button toggles automatic BPM stretch on file load.

**Live Performance Grid / Arrangement Timeline**
The main working area. In Performance Mode this is a scrollable grid of clip cells organised by scenes (rows) and tracks (columns), with the mixer panel below. In Arrangement Mode the same clips are laid out on a horizontal timeline with a playhead and section markers.

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

When Auto-Q is **ON** and a file with a detectable BPM is loaded (BPM from filename such as `loop_120bpm.wav`, or embedded metadata), GrooviXBeat will:

1. Time-stretch the audio to match the project tempo.
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


> See **[DrumKitSetup.md](DrumKitSetup.md)** for the steps to setup and use drumkit
samples from https://github.com/geikha/tidal-drum-machines.


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
| **Repeat** | How many times the scene loops before advancing (Arrangement Mode). |
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

## 4. Arrangement Mode & Timeline

Arrangement Mode is the linear timeline view. Use it to compose a full song by placing clip instances at specific bar positions across all tracks, with section labels to mark song structure.

### 4a. Switching to Arrangement Mode

Click the **Arrangement** mode button in the top toolbar. The grid switches to a horizontal timeline view. Click **Performance** to switch back. Transport (Play / Stop) works in both modes.

---

### 4b. Timeline View

The timeline shows all tracks as horizontal lanes. Time runs left to right, measured in bars.

| Element | Description |
|---------|-------------|
| Bar ruler | Horizontal ruler across the top showing bar numbers |
| Track lanes | One horizontal lane per track |
| Clip instances | Rectangles placed at specific bar positions; coloured by scene |
| Playhead | Vertical line showing the current playback position |
| Section markers | Coloured label bands above the ruler marking song sections (Intro, Verse, Chorus, etc.) |

Scroll the timeline horizontally with the scrollbar or mouse wheel. Zoom with **Ctrl + mouse wheel**.

---

### 4c. Placing Clips on the Timeline

**Click to place** — click an empty area in a track lane to place the clip from the scene that occupies that row at the click position.

**Drag to move** — drag an existing clip instance to a different bar position on the same track.

**Right-click to remove** — right-click a clip instance to remove it from the timeline.

Clip instances tile automatically: if you place a clip in a slot longer than the clip's own length, the clip repeats to fill the space.

---

### 4d. Section Markers

Section markers label ranges of the timeline (e.g. "Intro", "Verse", "Bridge"). They appear as coloured bands above the bar ruler and are visible during both arrangement editing and playback.

**Adding a marker**
1. Click on the bar ruler to position the arrangement cursor.
2. Click the **Label** button in the toolbar.
3. Type the section name and set the duration.
4. Click **Add**.

**Editing a marker**
Click the **Label** button when the cursor is exactly on an existing marker's start bar to edit its name or duration.

**Splitting a section**
Click the **Label** button when the cursor is inside (but not at the start of) an existing section to split it and insert a new named section at the cursor position.

---

### 4e. Arrangement Wizard

The **Wizard** button (wand icon) opens the Arrangement Wizard — a text formula editor that lets you describe an entire arrangement in a few lines of text and apply it all at once.

```
TrackName: StartBar-DurationBars=SceneName, DurationBars=SceneName, ...
```

**Example:**
```
Lead:  1-8=Intro, 8=Verse, 8=Verse, 8=Chorus, 8=Outro
Bass:  1-8=Intro, 8=Verse, 8=Verse, 8=Chorus, 8=Outro
Drums: 1-8=Intro, 8=Verse, 16=Chorus, 8=Outro
```

If any track or scene name cannot be resolved, a red error message appears inside the dialog and nothing is placed until all lines are valid.

> See **[ArrangementFormula.md](ArrangementFormula.md)** for the full syntax reference, token formats, naming shortcuts, and complete examples.

> See **[ArrangementView.md](ArrangementView.md)** for the complete Arrangement View & Live Performance reference — all toolbar buttons, edit and select modes, section markers, clip placement, zoom controls, keyboard shortcuts, and tips.

---

## 5. Piano Roll Editor

Open by clicking a clip cell on a melody, drum-kit, or sampled-instrument track.

### 5a. Toolbar

**Left side — icon buttons:**

| Button | Description |
|--------|-------------|
| **Filter** (eye icon) | Toggle: hide notes outside the selected scale. |
| **Select** | Toggle: switch to selection mode. Click and drag to select a range of notes; move, copy, or delete as a group. |
| **Automation** | Toggle: show/hide the automation lane below the note grid. |
| **VST UI** | Open the VST instrument's own editor window *(melody tracks only)*. |
| **MIDI Routing** | Open the MIDI routing dialog to configure MIDI input and output for the track. |
| **Load MIDI** | Import notes from a `.mid` file into the current clip. |
| **Clear** | Remove all notes from the current clip. |
| **Generate** | Open the Note Generator to create chord progressions, drum patterns, bass lines, or formula-based notes. |

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

### 5b. Note Editing

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

### 5c. Automation Lane

Click the **Automation** button in the toolbar to reveal the automation lane below the note grid.

**Selecting a parameter**
Use the parameter dropdown at the left of the lane to choose which value to automate:

- Volume
- Pan
- Pitch Bend
- Modulation (CC1)
- Any VST parameter exposed by the loaded instrument

**Drawing automation**
Click and drag in the lane to draw breakpoints. The value at any step is linearly interpolated between adjacent breakpoints.

**Renaming a parameter — Ren button**
Click **Ren** to give the currently selected parameter a custom display name (e.g. rename "Modulation" to "Filter Sweep"). The custom name is saved with the project and can be used in the Automation Formula dialog.

**Formula entry — Fn button**
Click **Fn** to open the Automation Formula dialog. Type bar-range formulas to set or ramp automation values across multiple bars in one step without drawing. If a formula contains an error, a red message appears inside the dialog and nothing is applied until all lines are valid.

```
Volume:1-8=0>100, 9-24=100, 25-32=100>0
Modulation:1-32=0>80
Filter Sweep:1-16=100>20
```

> See **[AutomationFormula.md](AutomationFormula.md)** for the full syntax reference including ramps, named parameters, renamed labels, and multiple parameters at once.

---

### 5d. Scale Filter

Set a **Scale Root** and **Scale Type** from the toolbar dropdowns. Toggle the **Filter** (eye) button to hide all notes outside the scale — remaining rows are highlighted, making it easy to stay in key.

The **Remap C Major to target scale** option in Track Properties transposes incoming keyboard notes from C Major to the selected scale in real time.

---

### 5e. Info Bar

| Side | Content |
|------|---------|
| Left | Track type and loaded VST instrument name (dim text) |
| Right | Hint text for the button or dropdown under the mouse (gold) |

> See **[ClipEditor.md](ClipEditor.md)** for the complete Piano Roll / Clip Editor reference — all toolbar buttons, note editing, automation lane, scale filter, drum kit mode, sampled instrument mode, keyboard shortcuts, and tips.

---

## 6. Sample Editor

Open by clicking a clip cell on a sample track, or by clicking a waveform thumbnail in an existing cell.

### 6a. Toolbar

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

### 6b. Waveform View

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

### 6c. Selection & Editing

Most edit operations act on the current selection. If no selection exists, the operation acts on the entire audio file. After each edit, the waveform updates automatically. The undo/redo stack is per-session and clears when a new file is loaded.

---

### 6d. BPM Detection & Stretch

Click the **BPM** button to open the BPM dialog. GrooviXBeat shows:
- Detected BPM (from filename, embedded metadata, or automatic analysis)
- Detection source

Click **Stretch to project BPM** to time-stretch the audio to the project tempo. The stretched file is saved to disk and the clip length is snapped to the nearest bar count.

> The BPM button is enabled whenever a file is loaded, regardless of whether a BPM was detected.

---

### 6e. Info Bar

| Side | Content |
|------|---------|
| Left | Filename (gold, bold) + duration, offset, detected BPM, and selection range (dim text) |
| Right | Hint text for the button or dropdown under the mouse (gold) |

---

## 7. Note Generator

Click the **Generate** button in the Piano Roll toolbar to open the Note Generator. It has four tabs:

> See **[NoteGenerator.md](NoteGenerator.md)** for the complete Note Generator reference covering all four tabs — Chord / Arp, Drum Patterns, Bass Lines, and Formula.

---

### 7a. Chord / Arp Tab

Build chord progressions and arpeggios from a palette of scale degrees.

| Control | Description |
|---------|-------------|
| **Root Key** | Root note of the key (C – B). |
| **Scale** | Scale type used to derive chord notes (Major, Minor, Dorian, etc.). |
| **Chord Type** | Voicing quality: triad, maj7, min7, dom7, sus4, etc. |
| **Arp Pattern** | Arpeggio direction / order: Up, Down, Up-Down, Random, etc. |
| **Chord Oct.** | Octave for the chord block. |
| **Note Duration** | Length of each note: 1/16, 1/8, 1/4, 1/2, 1 bar. |
| **Repeat** | How many times the full chord sequence is repeated. |
| **Chord block** | Toggle: include a simultaneous chord block in the output. |
| **Arpeggio** | Toggle: include an arpeggiated pattern in the output. |

**Chord Sequence palette**
Click the Roman numeral buttons (I – VII) to append scale degrees to your progression. Each click adds one chord to the sequence strip. Click the **×** on any chip in the strip to remove it. Click **Clear** to reset the sequence.

Click **Insert into Clip** to write the generated notes to the current clip.

> See **[ChordArpGenerator.md](ChordArpGenerator.md)** for the complete Chord / Arp Generator reference — chord types, voicings, arpeggio patterns, the scale-degree palette, and worked examples.

---

### 7b. Drum Patterns Tab

Choose from a built-in library of drum patterns organised by genre and style.

| Column | Description |
|--------|-------------|
| **Pattern** (left list) | Genre family: Rock, Hip-Hop, Electronic, Jazz, Latin, etc. |
| **Style** (right list) | Named variation within the family. |

Select a family row, then select a style. Click **Insert into Clip** to write the drum pattern to the current clip. The pattern tiles to fill the clip length.

---

### 7c. Bass Lines Tab

Choose from a built-in library of bass patterns organised by genre and style. Each pattern defines up to 16 steps of interval, velocity, and duration data relative to a root note.

| Control | Description |
|---------|-------------|
| **Pattern** (left list) | Genre family: Galloping, Walking, Funk Slap, Reggae, House, etc. |
| **Style** (right list) | Named variation (e.g. Standard, Root+Fifth, Approach Note). |
| **Root** | Root note for the bass line (C – B). |
| **Oct.** | Octave for the root note (1 – 4). |

The pattern description is shown below the style list. Click **Insert into Clip** to write the bass line to the current clip. The pattern tiles to fill the clip length.

---

### 7d. Formula Tab

Enter notes and automation directly as text using a compact bar/step notation. Positions are written as `Bar.Step` where step is a 1/16th note number (1–16). This is the fastest way to enter specific melodies, chord voicings, or note sequences from scratch.

```
Start.Step-End.Step=Note [param=value ...]
```

**Examples:**
```
1.1-2.1=C4 vel=90          # whole note, bar 1
1.1-1.5=C4                 # quarter note (steps 1-4)
1.5-1.9=E4                 # quarter note on beat 2
2.1-3.1=E4+G4 vel=85
3.1-5.1=F3+A3+C4 vel=95 mod=0>80
4.1-5.1=G3 vel=100 pitch=50>52
```

- **Positions**: `Bar.Step` — step 1–16 per bar, where step 5 = beat 2, step 9 = beat 3, step 13 = beat 4
- **Notes**: letter names (`C4`, `Bb3`, `D#5`) or plain MIDI numbers (`60`)
- **Chords**: stack notes with `+` (`C4+E4+G4`)
- **Velocity**: `vel=0–127`
- **Automation ramps**: `vol=0>100`, `mod=20>80`, `pitch=48>52`, `pan=50`
- **Comments**: `# text`

If a line contains an error, a red message appears below the text box and nothing is inserted until all lines are valid.

> See **[NoteFormula.md](NoteFormula.md)** for the full syntax reference including bar/step notation, all parameter names, chord syntax, and complete examples.

---

## 8. FX Chain Editor

Right-click a track header and choose **FX Chain** to open the FX Chain Editor.

Add VST3 effect plugins (compressors, reverbs, EQs, etc.) in series for that track. The signal flows through each plugin in order before reaching the master output. Each plugin slot shows the plugin name and a bypass toggle. Drag slots to reorder the chain.

> See **[InternalFx.md](InternalFx.md)** for the complete reference on GrooviXBeat's built-in internal effects — parameters, routing, and usage.

> See **[SideChain.md](SideChain.md)** for how to set up sidechain routing (e.g. ducking a pad with the kick).

---

## 9. VST3 Plugins

**Instruments**
Assigned per track via Track Properties → VST Instrument. The search box filters by name or manufacturer. Click **VST UI** to open the plugin's graphical editor.

**Effects**
Added per track via the FX Chain Editor (see [Section 8](#8-fx-chain-editor)).

**Plugin Scanning**
GrooviXBeat scans for installed VST3 plugins on startup. Plugins must be installed in the standard VST3 folder for your system.

**Automation**
VST parameters can be automated per clip in the Piano Roll automation lane. Select the parameter from the dropdown in the automation lane. Parameters can be renamed with the **Ren** button and referenced by their custom name in the Automation Formula dialog.

---

## 10. Project Management

| Action | How |
|--------|-----|
| **Save** | File → Save or **Ctrl+S**. Saves all clip data, MIDI notes, mixer settings, sample edits, plugin assignments, scene/track layout, and arrangement timeline. |
| **Load** | File → Open or **Ctrl+O**. Restores the complete session including all plugin states. |
| **New** | File → New. Clears all data and resets to the default layout. |

**Audio files**
When you drop or load audio files, GrooviXBeat copies them into the project's samples folder so the project remains self-contained and portable. Edited samples (trimmed, stretched, etc.) are also saved to disk within the project folder.

---

## 11. Tips & Keyboard Shortcuts

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

### Arrangement Tips

- Use the **Arrangement Wizard** to lay out a complete song structure in seconds — type one line per track, then click Apply.
- Track and scene names in wizard formulas are matched case-insensitively; you can also use `T1`/`S1` shortcuts or plain numbers.
- Clip labels (set with the **Label** button on a clip cell) can be used as scene names in wizard formulas, making it easy to reference specific clip variants by name.
- Section markers created with the **Label** toolbar button appear during both editing and playback, giving you a visual map of the song structure.

---

## Reference Documents

| Topic | Document |
|-------|----------|
| Live Performance & Arrangement View (full reference) | [ArrangementView.md](ArrangementView.md) |
| Piano Roll / Clip Editor (full reference) | [ClipEditor.md](ClipEditor.md) |
| Sample Editor (full reference) | [SampleEditor.md](SampleEditor.md) |
| Internal FX plugins (full reference) | [InternalFx.md](InternalFx.md) |
| Sidechain setup | [SideChain.md](SideChain.md) |
| Note Generator (full reference) | [NoteGenerator.md](NoteGenerator.md) |
| Chord / Arp Generator (full reference) | [ChordArpGenerator.md](ChordArpGenerator.md) |
| Note Formula syntax (Generator) | [NoteFormula.md](NoteFormula.md) |
| Arrangement Wizard formula syntax | [ArrangementFormula.md](ArrangementFormula.md) |
| Automation Formula syntax | [AutomationFormula.md](AutomationFormula.md) |
