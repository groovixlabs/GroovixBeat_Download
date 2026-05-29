# Automation Formula — User Manual

The **Fn** button in the automation lane gives you a fast way to set automation values across bar ranges without dragging. Open the automation lane, click **Fn**, type your formulas, and press **Apply**.

---

## Opening the Dialog

1. Open a clip in the **Clip Editor**.
2. Click the **Auto** button in the toolbar to show the automation lane.
3. Select the parameter you want to automate (e.g. Volume) using the quick-access toolbar.
4. Click **Fn** (below the **Ren** button on the left panel).
5. Type your formulas in the text box.
6. Click **Apply**.

---

## Formula Syntax

### Flat Value

Sets a constant value across a range of bars. Two equivalent notations are accepted:

```
BARSTART+LENGTH=VALUE
BARSTART-ENDBAR=VALUE
```

| Part | Description |
|---|---|
| `BARSTART` | First bar of the range (1-based) |
| `LENGTH` | Number of bars to cover (`+` form) |
| `ENDBAR` | Last bar of the range, inclusive (`-` form) |
| `VALUE` | Target value as a percentage: **0** (minimum) to **100** (maximum) |

`1+8=100` and `1-8=100` are identical — both cover bars 1 through 8.

**Example — set 8 bars starting at bar 1 to full volume:**
```
1+8=100
1-8=100
```

---

### Ramp (Fade)

Linearly fades from one value to another across a range of bars.

```
BARSTART+LENGTH=FROM>TO
BARSTART-ENDBAR=FROM>TO
```

| Part | Description |
|---|---|
| `FROM` | Value at the start of the range (0–100) |
| `TO` | Value at the end of the range (0–100) |

**Example — fade out over 8 bars starting at bar 9:**
```
9+8=100>0
9-16=100>0
```

**Example — fade in over 8 bars starting at bar 1:**
```
1+8=0>100
1-8=0>100
```

---

### Continuation Token

When a token starts with `+` and has no leading bar number, it continues from where the previous range on the same line ended.

```
BARSTART+LENGTH=VALUE, +LENGTH2=VALUE2, +LENGTH3=VALUE3
```

The cursor advances automatically after each token — you only need to specify the starting bar once.

**Example — flat then fade out, 4 bars each:**
```
1+4=100, +4=100>0
```

**Example — three consecutive segments:**
```
1+4=100>50, +4=50>0, +8=0
```

Continuation resets at the start of each line, so each line is independent.

---

### Parameter Prefix

To write automation for a specific parameter, prefix the line with the parameter name followed by a colon.

```
ParamName:BARSTART+LENGTH=VALUE
ParamName:BARSTART+LENGTH=FROM>TO
```

If no prefix is given, the **currently selected parameter** in the automation lane is used.

**Recognised built-in parameter names:**

| You type | Parameter |
|---|---|
| `Volume` or `Vol` | Volume |
| `Pan` | Pan |
| `PitchBend`, `Pitch Bend`, or `Pitch` | Pitch Bend |
| `Modulation` or `Mod` | Modulation |
| Any VST parameter name | Matched by display name (case-insensitive) |
| Any custom renamed label | Matched by the name set with the **Ren** button (case-insensitive) |

#### Using Renamed Parameter Labels

If you have renamed an automation parameter using the **Ren** button, you can use your custom name directly as the prefix — it will be recognised just like a built-in name.

**Example:** if you renamed parameter 1 from "Volume" to "Lead Vol":
```
Lead Vol:1+8=0>100
Lead Vol:9+8=100>0
```

If a prefix is not recognised (not a built-in name, not a VST parameter, and not a renamed label), an error is shown and nothing is applied.

---

### Multiple Ranges on One Line

Separate multiple ranges with commas. They all apply to the same parameter prefix on that line. Continuation tokens (`+LENGTH=VALUE`) are resolved left to right.

```
ParamName:RANGE1, RANGE2, RANGE3
```

---

## Multiple Parameters at Once

Use a separate line for each parameter. The dialog applies all lines when you click Apply.

```
Volume:1+8=100, +8=100>0
Pan:1+16=50
Modulation:1+8=0>100, +8=100>0
```

---

## Error Handling

If any line contains a mistake, a **red error message** appears inside the dialog below the text box. The dialog stays open so you can correct the problem and try again — nothing is applied until all lines are valid.

**Common errors:**

| Error message | Likely cause |
|---|---|
| `Unrecognised parameter: X` | The prefix before `:` is not a built-in name, a VST parameter name, or a renamed label |
| `Missing '='` | A range is malformed — check the `BARSTART+LENGTH=VALUE` format |
| Invalid value | A value after `=` is not a number or a valid `FROM>TO` ramp |

Fix the highlighted line and click Apply again.

---

## Complete Examples

### Example 1 — Simple flat volume

Set 16 bars to full volume on the currently selected parameter:
```
1+16=100
```

---

### Example 2 — Fade out over 8 bars

Volume at full for bars 1–8, then fade to silence over bars 9–16:
```
Volume:1+8=100
Volume:9+8=100>0
```

Or on one line using continuation:
```
Volume:1+8=100, +8=100>0
```

---

### Example 3 — Fade in, sustain, fade out (32-bar clip)

```
Volume:1+8=0>100, +16=100, +8=100>0
```

---

### Example 4 — Multiple parameters in one session

```
Volume:1+8=0>100, +16=100, +8=100>0
Pan:1+32=50
Modulation:1+32=0>80
```

---

### Example 5 — Automate a VST parameter by name

If your VST exposes a parameter called "Cutoff":
```
Cutoff:1+8=100>30, +8=30>100
```

---

### Example 6 — Using a renamed parameter label

If you renamed parameter 2 (Pan) to "Width" using the Ren button:
```
Width:1+8=50, +8=50>80, +16=80>50
```

---

## Notes

- **Applying a formula replaces** any existing automation breakpoints in the specified bar range. Points outside the range are untouched.
- **Values are clamped** to 0–100. Values outside this range are treated as 0 or 100.
- **Bars beyond the clip length** are silently clamped to the last step of the clip.
- **Unrecognised parameter prefixes cause an error** — the dialog stays open and nothing is applied.
- **Renamed labels** set with the **Ren** button are matched case-insensitively and work everywhere a built-in name works.
- Lines starting with `#` are treated as comments and ignored.
- The dialog can be re-opened and formulas re-applied at any time; each apply is additive on top of whatever is currently in the lane outside the specified ranges.

---

## Quick Reference

```
# Flat
1+8=100                        set 8 bars from bar 1 to 100%

# Ramp
9+8=100>0                      fade from 100% to 0% over 8 bars from bar 9

# Continuation (no leading bar number)
1+4=100, +4=100>0              bars 1-4 at 100%, bars 5-8 fade to 0%

# Named parameter (built-in)
Volume:1+8=100                 set Volume, 8 bars from bar 1

# Named parameter (renamed label)
Lead Vol:1+8=0>100             use custom name set with Ren button

# Multiple ranges with continuation
Volume:1+8=0>100, +16=100, +8=100>0

# Multiple parameters, multiple lines
Volume:1+32=100
Pan:1+16=50
Modulation:1+8=0>100

# VST parameter by display name
Cutoff:1+8=100>30, +8=30>100

# Comment line (ignored)
# this line is ignored
```
