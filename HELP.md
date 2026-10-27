
## Overview

- There are four tracks each with up to two MIDI processors/generators, one source (instrument, device input, Link Audio or audio file) and up to three audio effects.

- Device input uses the system default input. It runs through the track's FX and is included in recordings and History Machine.

- Three pages: **Rack** hosts the selected track's slots, **Mixer** has the faders, **Pads** plays the destination track (through its MIDI processors or bypassing them). The destination is set in **MIDI input** and defaults to track 1.

- Sends A/B and the master inserts are under **Sends A/B & master** on the **Mixer** page. The send and master slots are shared; the send levels belong to the selected track.

- Each slot hosts a compatible AUv3 or AUv2 plug-in of its type (MIDI processor, instrument, effect).

- **Assign** below the pages maps any loaded plug-in parameter to one of the four macro sliders.

- Tap the session name (top left) to rename, save, load or delete sessions. A saved session includes all plug-ins, their state and presets, and macro assignments.

- The current session is saved automatically at regular ~30 seconds intervals and restored on relaunch.

- **Saved sessions have open format**: each is a folder in **Documents/Sessions** (next to Audio Files) with a readable `Session.json` and one standard `.aupreset` per plug-in, so your work can stay usable even without the app. The folder name is the session name; rename, copy or back up folders in Files/Finder. Format: [SESSION_FORMAT.md](https://github.com/r618/HostSonicMicro-Docs/blob/master/SESSION_FORMAT.md)

- The small light at the bottom left shows approximate RMS of the final output, before it is played or recorded.

## Transport

**Play / Pause / Resume** starts or pauses playback; Pause keeps the position. **Return to start** beside it stops playback and rewinds - next **Play** restarts MIDI files from their first event. Return to start keeps the audio engine, plug-ins and settings; audio files keep their Start position.

With external MIDI sync, rewind on the sending device (local button is also disabled while under external sync..).

## Audio settings

- The app runs at the current output device rate. Override it in the gear menu → **Settings**.

The host requests a 256-frame I/O buffer. Processing and recording are stereo.

## MIDI input

- **External source** selects one CoreMIDI source, or **All sources**. Sources include network RTP sessions and, on macOS, IAC buses.
- Select one **Destination** track; pads use the same destination. Other apps see the host as **HostSonic MIDI**.
- The track's two MIDI processors run in order. Their output goes to the instrument and to every MIDI-capable Audio FX (`aumf`) with its **MIDI** checkbox on (off by default). All enabled destinations receive the same events.
- Device input and Link Audio tracks can also use MIDI processors and MIDI-enabled FX, without an instrument. Example: select device input as the source, load a MIDI-capable FX, enable its **MIDI** checkbox, then load a MIDI processor.
- Pads keep their **Through MIDI** / **Direct** choice. Direct skips the MIDI processors for pads only and sends to the instrument and MIDI-enabled FX.

## MIDI files in processor slots

Tap the notes icon on either MIDI processor slot and choose **Choose MIDI File…**. The file takes the place of that slot's plug-in. It sends notes and channel messages to the next processor, to the instrument and MIDI-enabled FX. MIDI from an earlier processor or MIDI input passes through.

Playback follows the host transport and tempo; the file's tempo map is ignored. **Loop** is on by default. The file's ellipsis menu turns Loop off (the current pass finishes), bypasses playback, picks another file or removes it. The file and its Loop setting are saved with the session. Files share the **Documents/Audio Files** library with audio files. No sound bank is used here; load an instrument to hear the notes.

See below for MIDI audio playback.

## MPE pads

Turn on **MPE** on the Pads page to play an MPE instrument. Off by default, saved with the session. With MPE off, pads behave as normal single notes..

- Each held pad uses its own member channel in the lower MPE zone: manager channel 1, member channels 2–7. One finger per pad; several pads can play at once.
- Slide sideways for pitch bend. **Pitch bend width** (beside the MPE toggle) sets 2, 12, 24 or 48 semitones per pad width (default 12).
- Slide up/down for CC74 (timbre), which starts at 64 on touch-down.
- Touch force sends channel pressure, on force-capable displays or with Apple Pencil only.
- Lifting sends note-off and frees the channel.

The host sends the MPE zone and pitch range configuration. If the instrument needs manual setup, use the lower zone, channels 2–7 and ±48 semitones member pitch bend. MIDI processors on the pad route must preserve member channels and expression; use **Direct** to bypass them. External MIDI uses the same instrument, so avoid channel conflicts while playing pads.

## MIDI sync in/out

- Set **Sync input** and **Sync output** in the Tempo dialog.
- MIDI sync and Ableton Link are mutually exclusive.
- Sync input lists all discovered senders. With **Auto**, the first sender to start wins and keeps priority over later ones.
- Sync output goes to a specific app or device, or is published as **HostSonic Clock** for any app to subscribe.

## Ableton Link sync and audio

- Enable Link in the Tempo dialog, in the audio source picker, or under Mixer → **Sends A/B & master** → **Link Audio Output**.
- Tempo syncs once Link is enabled.
- To receive audio, pick a discovered Link Audio stream in the track's audio source picker.
- To send the final mix, turn on **Share Main Mix** under **Link Audio Output**.

## Audio files

- To use an audio file as a track source: tap the waveform icon → **Audio File** → **Choose Audio File…**. Any format the system can decode works (WAV, AIFF, CAF, MP3, AAC/M4A, ALAC, FLAC).
- The file is copied to **Documents/Audio Files** and decoded to memory, up to 256 MB decoded (about 12 min stereo at 44.1 kHz). Other rates are resampled to the session rate; mono plays on both channels; only the first two of more channels are used.
- **MIDI files** (Standard MIDI File `.mid`/`.midi`/`.kar`) are picked the same way and play through a General MIDI sound bank: all 16 channels, program and tempo changes, drums on channel 10.
  - The sound bank is GeneralUser GS by S. Christian Collins. It ships with the app, sounds the same on all platforms, and is free to use, private or commercial.
  - The file is rendered to stereo once (44.1 kHz) when chosen or when a session loads it, then behaves like any audio file: waveform, playhead, Start, Loop and transport.
  - The render is normalized to −1 dBFS peak (at most +24 dB); use the track faders to balance MIDI files.
  - The audio runs to the end of the music plus up to 3 s of release tail (cut once silent). With Loop on, the tail is part of the loop.
  - Same ~12 min limit as audio files. File info shows MIDI tracks, sound bank and render rate instead of sample rate and channels.
- Playback follows the host transport (internal, MIDI sync or Link): Play from stop starts at the playhead; Pause/Resume continues from the pause point.
- Tap the loaded file to open its waveform:
  - Drag the waveform to set the playhead, which is also where the next Play from stop begins.
  - The **Start** field takes `SS`, `MM:SS` or `H:MM:SS`, with decimals (`1:23.5`).
  - File info: name, title/artist/album when present, format/container, sample rate, channels, bit depth (PCM) or estimated bit rate, duration and size.
- Playhead/start position and Loop are saved with the session; the session stores only the file name.
- **File location:** iOS: Files → On My iPhone/iPad → HostSonic μ → Audio Files. macOS: `~/Library/Containers/<app>/Data/Documents/Audio Files`. Picking the same file again reuses its copy; a file picked from this folder is used in place; a different file with an existing name gets a numbered name. The app never deletes copies; remove them in Files/Finder. A session whose file was deleted shows it as missing; other tracks keep playing.


## Recording

- **Record** (bottom right) records the final output immediately. Press again to stop and save.

- **History Machine** continuously keeps the last few seconds of the final output; set the length in **Settings**. Pressing it saves that history and keeps recording. Press **History Machine** or **Record** again to stop and save.

- Choose the save location in the standard Files/Finder dialog. The default file name is the session name plus date and time.

- Set the format in **Settings** → **Recording**: AAC (256 kbps .m4a), FLAC (default, lossless 24-bit) or WAV (24-bit). AAC is saved at 44.1 or 48 kHz when the processing rate is higher.

- Nothing is captured while the device is suspended or the audio engine is stopped. Changing the sample rate (for example, by switching outputs) clears the history.

- If the output rate changes during recording (for example, by switching outputs) or an error occurs, the save dialog opens so you can keep the audio up to that point.
