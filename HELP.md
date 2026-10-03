
## Overview

- There are four lanes/tracks, each can have up to two MIDI processors/generators, one mutually exclusive Instrument or Audio Input source, and up to three audio effects

- Audio Input uses the system default audio input. It is processed by that track's Audio FX and is included in final recording and History Machine audio.

- Three main sections below: **Rack** where lanes are hosted, **Mixer**, and **Pads** for testing the sound on single track (either with or bypassing currently loaded MIDi processor, track destination is set in MIDI Input, defaults to track #1)

- Sends A/B and Master inserts are below mixer on **Mixer** pane - UI is the same, but they are specific for currently selected track only

- all slots can host compatible AUv3, AUv2 plugins according to their type (MIDI processor, instrument, effect)

- **Assign** button below panes allows assigning any currently slotted plugin's parameter to one of the four macros/sliders

- top left is current session name, tapping it allows renaming, saving, removing and restoring previously saved session; session save includes everything currently setup in the host - all plugins, their settings/loaded patches, and macros assignments,...

- Current session is also saved automatically and at regular intervals and automatically restored on relaunch.

- an RMS based green diode indicator's intensity at bottom left measures approximately final audio before its output/recorded

## Audio settings

- app runs at current default output rate, change in cogwheel menu, **Settings**

Internal processsing requests 256 frames for audio I/O buffer and (recorded) audio is in stereo.

## MIDI In

- **MIDI input** - pick from any available CoreMIDI sources on the device or as aggregate of all of them
- after discovered sources include Network RTP sessions and configured IAC on macOS
- pick app's track as MIDI In destination

## MIDI Sync In/Out & Ableton Link sync

- **Input clock** can be synced to clock selected in BPM/Tempo dialog:
- Ableton Link sync and MIDI In/Out shoudl be mutually exclusive
- choose from all discovered sync points; when *Auto* is selected, first Start 'wins' and will be kept/prioritized over any other sync which might arrive later
- **Output clock** can be advertised as public, with any subsriber(s) or to a specific existing app/destination

## Recording

- Final output can be recorded immediately by tapping **Record** button at the bottom right, tap/press again to finish and save

- **History Machine** runs continuosly and stores the last few seconds of the final output. You can set the amount in **Settings**. Tapping it keeps the history and appends currently playing output, press either **History Machine** or **Record** again to finish and save

- save location for final WAV file is selected using the standard Files/Finder picker, file name contains the session name and capture date/time by default

- device suspension or a stopped audio engine cannot supply audio, changing the sample rate (like switching outputs..) also clears the history

- if output rate changes during recording (like switching outputs..), or any error is encountered, save dialog is presented and audio up to that point can be saved
