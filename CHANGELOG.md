## TestFlight Beta 1.0.4
- compatibility fixes for cold plug-in restoration and CoreMIDI startup
- isolation fixes for failing plugins which could affect entire session
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
