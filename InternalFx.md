# Internal Effects — User Manual

GrooviXBeat ships a set of built-in audio effects you can insert on any mixer
track or on the master bus. They appear in the FX chain alongside any external
VST/AU plugins you've scanned.

- **Source:** `Source/Plugins/Effects/` (one file per effect)
- **Parametric EQ details:** see [ParametricEQ](#parametric-eq) (and the visualiser
  notes below)

---

## Adding & managing effects

1. Open a track's **FX chain** (or the master bus FX chain) from the mixer.
2. The left list shows everything available — built-in effects first, then your
   scanned plugins. Use the search box to filter.
3. **Add →** inserts the selected effect at the end of the chain. Double-click also
   adds it.
4. The right list is the **ordered chain**: signal flows top → bottom
   (`Source → FX1 → FX2 → … → Mixer`). Use **↑/↓** to reorder, **← Remove** to
   delete.
5. **Open UI** shows the selected effect's controls.
6. **Apply** rebuilds the chain; each effect keeps its own settings, saved with
   the project.

You can place several effects in any order, including more than one of the same
effect (e.g. two EQs, or a Delay into a Reverb).

### The control panel

Most effects use an auto-generated panel:

- **Rotary knobs** for continuous values, with the value shown below (2 decimals).
- **Combo boxes** for type/mode choices and **toggles** for on/off switches, laid
  out on a top row above the knobs.

Three effects have custom panels: **EQ** and **Parametric EQ** (graph + curve)
and **Sidechain Compressor** (live visualiser).

> **Time note:** delay/modulation times are in milliseconds (ms) or seconds (s)
> as labelled; LFO rates are in Hz (cycles per second).

---

## Dynamics

### Compressor
Evens out level differences (or, in expander mode, increases them / gates noise).

| Control | Range (default) | What it does |
|---|---|---|
| Mode | Compressor / Limiter · Expander / Noise gate (Comp) | Reduce loud peaks, or attenuate quiet signal. |
| Threshold | −60…0 dB (−24) | Level where processing starts. |
| Ratio | 1…100 (50) | How hard the level is reduced past the threshold (∞-ish at 100 = limiter). |
| Attack | 0.1…100 ms (2) | How fast it reacts to peaks. |
| Release | 10…1000 ms (300) | How fast it recovers. |
| Makeup | −12…12 dB (0) | Gain added back after compression. |
| Bypass | off | Pass audio through untouched. |

*Tip:* lower Threshold + higher Ratio = more squashing. Fast Attack tames
transients; slow Attack lets them through.

### Sidechain Compressor
A compressor whose **trigger** is another track — classic "pumping/ducking"
(e.g. duck a pad under a kick). If no sidechain source is connected it ducks from
its own signal.

| Control | Range (default) | What it does |
|---|---|---|
| Source Track | 0…15 (0) | Which track's signal triggers the ducking. |
| Threshold | −60…0 dB (−24) | Trigger level at which ducking begins. |
| Ratio | 1…20 (4) | Depth of ducking. |
| Attack | 0.1…100 ms (10) | How fast the duck engages. |
| Release | 10…1000 ms (100) | How fast the level returns. |
| Makeup | 0…24 dB (0) | Output gain. |

Its panel includes a **live visualiser**: the sidechain (trigger) signal, the
input before compression, the output after, and the transfer curve with a moving
operating-point dot. Select the effect in a slot and play audio to see it move.

---

## EQ & Filtering

### EQ
A single-band filter for quick tone shaping.

| Control | Range (default) | What it does |
|---|---|---|
| Frequency | 10…20000 Hz (1500) | Centre/corner frequency. |
| Q | 0.1…20 (≈1.41) | Bandwidth/resonance (higher = narrower/sharper). |
| Gain | −12…12 dB (0) | Boost/cut (used by Shelf and Peaking types). |
| Filter Type | Low Pass · High Pass · Band Pass · Low Shelf · High Shelf · Peaking/Notch | Filter shape. |

*Note:* with **Peaking/Notch** at ~0 dB gain the band is effectively inactive —
dial in some Gain to hear it.

### Parametric EQ
A 7-band parametric EQ with a live spectrum analyser and draggable curve.
Summary:

- 7 bands, each with **On**, **Type** (Bell / Low Shelf / High Shelf / Low Cut /
  High Cut / Notch), **Freq**, **Gain**, **Q**.
- The graph overlays the **input spectrum** (grey) and **output spectrum** (gold)
  with the combined EQ curve.
- **Drag a node** to set its frequency (x) and gain (y); **mouse-wheel** over a
  node changes its Q; **double-click** toggles the band on/off; the band buttons
  (1–7) above the graph select a band for the knob strip below.

### Wah-Wah
A sweeping resonant filter — manual, LFO-driven, or envelope-following (auto-wah).

| Control | Range (default) | What it does |
|---|---|---|
| Mode | Manual · LFO · Envelope · LFO + Envelope (Manual) | What moves the filter. |
| Mix | 0…1 (0.5) | Dry/wet blend. |
| Frequency | 200…1300 Hz (300) | Base/manual filter frequency. |
| Q | 0.1…20 (10) | Resonance of the sweep. |
| Filter Type | Resonant Low/High/Band Pass | Filter shape that sweeps. |
| LFO Freq | 0…5 Hz (2) | Sweep rate in LFO modes. |
| LFO/Env Mix | 0…1 (0.8) | Blend of LFO vs envelope in the combined mode. |
| Env Attack | 0.1…100 ms (2) | Envelope follower response (Envelope modes). |
| Env Release | 10…1000 ms (300) | Envelope follower recovery. |

---

## Modulation

All modulation effects share an **LFO Waveform** (Sine / Triangle / Sawtooth /
Inverse Sawtooth) where applicable; delay-line ones share **Interpolation**
(Nearest / Linear / Cubic — higher = smoother, slightly more CPU).

### Chorus
Thickens a sound with detuned, delayed copies ("voices").

| Control | Range (default) | What it does |
|---|---|---|
| Delay | 10…50 ms (30) | Base delay of the voices. |
| Width | 10…50 ms (20) | Depth of the delay modulation. |
| Depth | 0…1 (1) | Amount of LFO movement. |
| Voices | 2 / 3 / 4 / 5 (2) | Number of layered copies. |
| LFO Freq | 0.05…2 Hz (0.2) | Modulation rate. |
| LFO Waveform | (Sine) | Shape of the movement. |
| Interpolation | (Linear) | Delay-line read quality. |
| Stereo | on | Spread voices across L/R. |

### Flanger
A short modulated delay creating a sweeping "jet" comb-filter.

| Control | Range (default) | What it does |
|---|---|---|
| Delay | 1…20 ms (2.5) | Base comb delay. |
| Width | 1…20 ms (10) | Sweep depth. |
| Depth | 0…1 (1) | LFO amount. |
| Feedback | 0…0.5 (0) | Resonant intensity of the sweep. |
| Inverted | off | Flip the wet polarity (hollower tone). |
| LFO Freq | 0.05…2 Hz (0.2) | Sweep rate. |
| LFO Waveform / Interpolation / Stereo | | As Chorus. |

### Phaser
A series of all-pass filters sweeping notches through the spectrum.

| Control | Range (default) | What it does |
|---|---|---|
| Depth | 0…1 (1) | Sweep amount. |
| Feedback | 0…0.9 (0.7) | Resonance/emphasis. |
| Filters | 2 / 4 / 6 / 8 / 10 (4) | Number of all-pass stages (more = richer). |
| Min Freq | 50…1000 Hz (80) | Lower bound of the sweep. |
| Sweep Width | 50…3000 Hz (1000) | How far the notches travel. |
| LFO Freq | 0…2 Hz (0.05) | Sweep rate. |
| LFO Waveform / Stereo | | Movement shape / stereo spread. |

### Tremolo
Rhythmic volume modulation.

| Control | Range (default) | What it does |
|---|---|---|
| Depth | 0…1 (0.5) | How deep the volume dips. |
| Frequency | 0…10 Hz (2) | Modulation rate. |
| Waveform | (Sine) | Shape of the amplitude movement. |

### Vibrato
Rhythmic **pitch** modulation via a modulated delay line.

| Control | Range (default) | What it does |
|---|---|---|
| Width | 1…50 ms (10) | Pitch-modulation depth. |
| Frequency | 0…10 Hz (2) | Modulation rate. |
| Waveform / Interpolation | | Movement shape / read quality. |

### Ring Modulation
Multiplies the signal by a carrier oscillator for metallic/bell/robotic tones.

| Control | Range (default) | What it does |
|---|---|---|
| Depth | 0…1 (0.5) | Dry/effect blend. |
| Frequency | 10…1000 Hz (200) | Carrier frequency (the "metallic pitch"). |
| Waveform | (Sine) | Carrier shape. |

### Panning
Positions a mono/stereo signal in the stereo field. (Needs a stereo track.)

| Control | Range (default) | What it does |
|---|---|---|
| Method | ITD/ILD · Stereo Balance · Constant Power (ITD/ILD) | How the pan is computed. |
| Pan | −1 (left) … 1 (right) (0) | Stereo position. |

- **ITD/ILD** simulates real spatial cues (level + tiny inter-ear time delay).
- **Stereo Balance** is a simple L/R level balance.
- **Constant Power** keeps perceived loudness even across the sweep.

---

## Delay & Reverb

### Delay
A single echo line with feedback.

| Control | Range (default) | What it does |
|---|---|---|
| Time | 0…5 s (0.1) | Echo time. |
| Feedback | 0…0.9 (0.7) | How many repeats (higher = longer tail). |
| Mix | 0…1 (1) | Level of the echoes added to the dry signal. |

### Ping-Pong Delay
A stereo delay that bounces echoes between left and right. (Needs a stereo track.)

| Control | Range (default) | What it does |
|---|---|---|
| Balance | 0…1 (0.25) | Cross-feed/placement of the bounces. |
| Time | 0…5 s (0.1) | Echo time. |
| Feedback | 0…0.9 (0.7) | Number of repeats. |
| Mix | 0…1 (1) | Echo level. |

### Reverb
Adds room/space ambience.

| Control | Range (default) | What it does |
|---|---|---|
| Room Size | 0…1 (0.5) | Apparent size of the space. |
| Damping | 0…1 (0.5) | High-frequency absorption (higher = darker tail). |
| Wet Level | 0…1 (0.33) | Reverb amount. |
| Dry Level | 0…1 (0.4) | Direct signal amount. |
| Width | 0…1 (1) | Stereo spread of the reverb. |

---

## Distortion & Character

### Distortion
Multiple saturation/clipping curves with input drive, output trim and a tone tilt.

| Control | Range (default) | What it does |
|---|---|---|
| Type | Full Wave Rectifier · Half Wave Rectifier · Soft Clipping · Hard Clipping · Fuzz · Overdrive (Full Wave) | Shape of the distortion. |
| Input Gain | −24…24 dB (12) | Drive into the curve (more = more distortion). |
| Output Gain | −24…24 dB (−24) | Level trim after distortion (compensate for added gain). |
| Tone | −24…24 dB (12) | High-shelf tilt to brighten/darken the result. |

*Tip:* push **Input Gain** for more grit, then pull **Output Gain** down to match
levels. **Soft Clipping** / **Overdrive** are smoother; **Hard Clipping** /
**Fuzz** are more aggressive.

---

## Pitch & Special

### Pitch Shifter
Shifts pitch up or down (grain-based) without changing tempo.

| Control | Range (default) | What it does |
|---|---|---|
| Semitones | −12…12 (0) | Pitch shift amount. At 0 the effect is bypassed. |

*Note:* large shifts and very transient material will show grain artefacts —
this is a real-time time-domain shifter, best for moderate shifts and pads.

### Robotization
Frequency-domain (FFT) voice effects.

| Control | Options (default) | What it does |
|---|---|---|
| Type | Pass-through · Robotization · Whisperization (Pass-through) | Pass-through does nothing; **Robotization** flattens phase for a monotone/robot timbre; **Whisperization** randomises phase for a breathy whisper. |

---

## Quick reference (signal-flow tips)

- **Order matters.** Common chains: *EQ → Compressor → Delay → Reverb*, or
  *Distortion → EQ → Delay*.
- **Stereo-only effects** (Ping-Pong Delay, Panning) need a stereo track to do
  anything useful.
- **Sidechain Compressor** needs its **Source Track** set to the track you want to
  duck from (e.g. the kick).
- Every effect's settings are stored per-slot and saved with the project.
