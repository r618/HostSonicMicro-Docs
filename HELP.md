
## Quick overview

- There are four lanes/tracks, each can have up to two MIDI processors/generators, one instrument, and up to three effects
- Three main sections below: **Rack** where lanes are hosted, **Mixer**, and **Pads** for testing the sound on track #1 (either with or bypassing currently loaded MIDi processor)
- Sends A/B and Master inserts are below mixer on **Mixer** pane - UI is the same, but they are currently selected track specific -
- all slots can host compatible AUv3, AUv2 plugins according to their type (MIDI processor, instrument, effect)
- **Assign** button below panes allows assigning any currently slotted plugin's parameter to one of the four macros/sliders
- top left is current session name, tapping it allows renaming, saving, and loading previously saved session; session save includes everything currently setup in the host - all plugins, their settings/loaded patches, and macros assignments,...
- Current session is also saved automatically and at regular intervals and automatically restored on relaunch.
- an RMS based green diode indicator's intensity at bottom left measures approximately final audio before its output/recorded


## Recording

- Final output can be recorded immediately by tapping **Record** button at the bottom right, tap/press again to finish and save
- **History Machine** runs continuosly and stores last 30 seconds of final output. Tapping it keeps the history and appends to it the current output, press either **History Machine** or **Record** again to finish and save
- save location for final WAV file is selected using the standard Files/Finder picker (file name contains the session name and capture date/time by default)
- device suspension or a stopped audio engine cannot supply audio, changing the sample rate (like switching outputs..) also clears the history
- if output rate changes during recording (like switching outputs..), or any error is encountered, save dialog is presented and audio up to that point can be saved 

Internal processsing and recorded audio is in stereo.
