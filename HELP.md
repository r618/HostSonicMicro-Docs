
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

- **MIDI input** selects one CoreMIDI source or **All sources** for the app. Available sources include Network RTP sessions and IAC on macOS.
- Select one **Destination** track, pads use this same destination. MIDI In is visible as **HostSonic MIDI** in system/other apps.
- The track's two MIDI processors run in order, final output goes to the Instrument and every MIDI-capable Audio FX (`aumf`) with its **MIDI** checkbox enabled (off by default). All enabled destinations receive the same events in parallel.
- Audio Input and Link Audio tracks can also use MIDI processors and send MIDI to enabled FX, without an instrument. For example, select Microphone as the audio source, load MIDI capable FX in FX slot, enable it's MIDI checkbox and load MIDI processor.
- Pads use the same destination and keep their Through MIDI / Direct choice. Direct bypasses the MIDI processors for pads only and sends to the instrument and enabled FX.

## MPE pads

Enable **MPE expression** on the Pads page for an MPE instrument. Off by default, saved with the session. Ordinary pads keep their original behavior.

- Each held pad uses a separate member channel in the lower MPE zone: manager channel 1, member channels 2–7. One finger per pad is supported, pads can be played simultaneously.
- Pitch bend is left to right **Pitch bend width**, beside the MPE toggle, selects 2, 12, 24, or 48 semitones per pad width (default: 12).
- Slide up / down sends tCC74 (timbre). CC74 starts at 64 at touch-down.
- Touch force sends channel pressure only for force-capable direct touch or Apple Pencil.
- Lift sends note-off and frees the channel.

The host sends MPE zone and pitch range configuration. If the instrument needs manual setup, select the lower zone, channels 2–7, and ±48 semitones for member pitch bend. MIDI processors on the pad route must preserve the member channels and expression messages; use **Direct** to bypass them. External MIDI notes use the same instrument, so avoid channel conflicts while playing pads.

## MIDI Sync In/Out

- **Input clock** can be synced to clock selected in BPM/Tempo dialog:
- MIDI In/Out and Ableton Link sync are mutually exclusive
- choose from all discovered sync points; when *Auto* is selected, first Start 'wins' and will be kept/prioritized over any other sync which might arrive later
- **Output clock** can be advertised as public, with any subsriber(s) or to a specific existing app/destination

## Ableton Link Sync and Audio

- enable Link in BPM/Tempo dialog, or when adding/opening Audio Input instrument on a track
- tempo is synced after Link is enabled
- select from Link audio streams on Audio Input dialog once they're discovered/available

## Recording

- Final output can be recorded immediately by tapping **Record** button at the bottom right, tap/press again to finish and save

- **History Machine** runs continuosly and stores the last few seconds of the final output. You can set the amount in **Settings**. Tapping it keeps the history and appends currently playing output, press either **History Machine** or **Record** again to finish and save

- save location for final WAV file is selected using the standard Files/Finder picker, file name contains the session name and capture date/time by default

- device suspension or a stopped audio engine cannot supply audio, changing the sample rate (like switching outputs..) also clears the history

- if output rate changes during recording (like switching outputs..), or any error is encountered, save dialog is presented and audio up to that point can be saved
