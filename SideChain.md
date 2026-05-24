
> How do i test the sidechain FX

● Here's the step-by-step workflow:

  Setup

  1. Load your clips
  - In Performance mode, place your kick drum on one track (e.g. Track 1) and your bass on another (e.g. Track 2)

  2. Open the FX Chain on the bass track
  - In the mixer strip, click the Fx button on Track 2 (the bass)

  3. Add the Multi-Effect
  - In the left panel of the FX Chain dialog, select Multi-Effect
  - Click Add → to move it to the chain
  - Click Apply (this creates the graph node and enables the Open UI button)

  4. Open the Multi-Effect UI
  - Click Open UI — the Multi-Effect editor opens

  5. Configure the Sidechain Compressor
  - In the Effect 1 dropdown, select Sidechain Compressor
  - The Sidechain Compressor tab appears automatically
  - Set your parameters:
    - Threshold: start around -24 dB (where your kick peaks)
    - Ratio: 4:1 to 8:1 for a noticeable duck
    - Attack: 5–10 ms (fast enough to catch the kick transient)
    - Release: 80–150 ms (controls how quickly the bass comes back up)
    - Makeup: add a few dB if the bass level drops too much
    - Source Track: set to Track 1 (the kick)

  6. Apply and close
  - Close the Multi-Effect UI
  - Re-open FX Chain on Track 2 → click Apply again to rewire the sidechain bus

  ▎ Why Apply twice? The graph wiring in setTrackFxChain reads the scSourceTrack parameter at Apply time. The first Apply creates the
  ▎ node (parameter defaults to Track 1). If you changed the source track in the UI, you need a second Apply to rewire the sidechain
  ▎ bus connection.

  ---
  Verifying it works

  - Press Play in Arrangement or Performance mode
  - The bass should audibly duck every time the kick hits
  - If nothing happens, the sidechain bus may not be connected — check that the source track has an active audio clip playing

  ---
  Shortcut for testing without the full wiring

  If you just want to hear the compressor responding without wiring the sidechain bus, the processor automatically self-triggers
  (reads from channels 0–1) when no sidechain connection is present. So you can hear the compressor working on the bass track on its
  own before the kick wiring is in place.

  ---
  Known limitation to be aware of

  The sidechain bus is wired at Apply time based on the scSourceTrack parameter value. If you change the source track combo in the UI,
   you must close the editor and Apply again from the FX Chain dialog to update the graph connection. A future improvement would be to
   listen for parameter changes and rewire automatically.
   