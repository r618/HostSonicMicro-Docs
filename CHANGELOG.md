## 1.1.0
- + Reset to Start button beside Play/Pause/Resume
- + support for MIDI files:
- in MIDI processor slot: right-side notes menu, host tempo and transport, Loop on by default. File notes continue through the downstream MIDI graph, incoming MIDI passes through the file slot.
- in Audio File track source: Standard MIDI Files play through the bundled GeneralUser GS bank on iOS and macOS (all 16 channels, program changes, drums on channel 10), rendered once to audio, then waveform/playhead/Loop work as for audio files
- hardened final output physical protection & Panic recovery

## TestFlight Beta 1.0.8
- + open text session format: saved sessions are folders in Documents/Sessions (Files app/Finder) with a readable Session.json and standard .aupreset files for plug-in states; see SESSION_FORMAT.md. Sessions saved by earlier betas are not loaded
- + Audio File track source: waveform (Audio Input) menu -> Audio File, standard Files/Finder picker; any format the OS decodes (WAV, AIFF, CAF, MP3, AAC/M4A, ALAC, FLAC)
- UI: better support for small screens & larger OS text sizes (should be identical on normal common screens)
- : better layout for tracks switching
- added MIDI In indication and switch directly to track view
- macOS: plugin windows improvements
- + saved recording formats: AAC, FLAC(default) and WAV, set in Settings -> Recording

## TestFlight Beta 1.0.7
- + Settings window with fixed/updated app's default samplerate and History Machine length
- + Ableton Link Sync and Audio
- receive per track audio via Link, main mix can be sent via Mixer/Sends (see HELP for more)
- + Pads have optional MIDI MPE: per-note pitch, CC74 timbre, and touch pressure
- + enhanced support for Audio FX units which support MIDI:
- MIDI reception checkbox for compatible Audio FX - track's MIDI is sent to both Instrument and Audio FX
- MIDI processors and pads can control FX on Audio Input and Link Audio tracks w/o main Instrument present
- more bugfixes for AUs operations

## TestFlight Beta 1.0.6
- + MIDI Sync In/Out via CoreMIDI on device, + Network RTP, + IAC on macOS
- tempo sync attempts for drift corrections
- + MIDI In (target is single selected track)

## TestFlight Beta 1.0.5
- fixed samplerate mismatch
- less audio disruptions when app is changing background/foreground states

## TestFlight Beta 1.0.4
- more plugins loading/restoration and MIDI startup fixes
- currently selected patch id/name in plugin's patches menu (if it is provided) is also saved in session now (actual settings/content was saved also before already)
- added a "Reload" action to plugin's menu which tries to populate the same plugin without having to browse for it again
- UI/UX updates

## TestFlight Beta 1.0.3
- Audio Input can be used in Instrument slot
- there are small icons on each track's button indicating what plugin(s) are populated on it
- plugin's windows are more compact and should be more accommodating/compatible with how plugins display their content in host
- improvements for plugins/FXs connections (disconnected FXs now restore reliably)

## TestFlight Alpha 1.0.2
- fixed volume sliders travels
- several UI/UX improvements/updates: mixers scrolling/sliders, MIDI Input config, MIDI Input/Sends configs removed from scrolling,...
- refuse to load incompatible plugins instead of crashing - a warning suggesting samplerate change for relevant error when loading is shown
- added option to change samplerate for the session to cogwheel menu which should help with loading previously incompatible plugins
- improved session's plugins restoration/loading

## TestFlight Alpha 1.0.1
Sep 2026
- skip loading of non supported extensions like Apple's ttsp AUSpeechSynthesizer since they don't fully support audio graph embedding
- updated app icon
- added 'History Machine' - a time machine like audio history recording

## TestFlight Alpha 0.1
Sep 15 2026
- and accidental macOS release
