# HostSonic session format, version 1

A HostSonic session is an ordinary folder of text files. You can read it, back it up, put it under version control, or convert it with any tool, without the app. This document is the complete specification.

## Where sessions are

| | Location |
|---|---|
| Saved sessions, iOS | Files app → On My iPhone/iPad → HostSonic μ → **Sessions** |
| Saved sessions, macOS | `~/Library/Containers/<app>/Data/Documents/Sessions` |
| Audio files used by sessions | The **Audio Files** folder next to **Sessions** |

The app lists every folder in **Sessions** that holds a valid `Session.json`. **The folder name is the session's name**, so renaming the folder in Files or Finder renames the session. To duplicate a session, copy its folder. Moving a session to another device means copying its folder into that device's **Sessions** folder, plus any audio files it uses into **Audio Files**.

The current, unsaved session is autosaved in this same format inside the app's private storage.

## Folder contents

```
Sessions/
  Night Drive/
    Session.json                          the session
    Track 1 Slot 3 - Model D.aupreset     state of one plug-in
    Send A - ValhallaSupermassive.aupreset
```

- `Session.json` is UTF-8 JSON with sorted keys, so diffs between saves stay small.
- Each loaded plug-in that has state gets one **`.aupreset`** file. This is Apple's standard Audio Unit preset format: an XML property list holding the plug-in's `fullState`, with the `type`, `subtype`, `manufacturer`, `version` and `name` keys other hosts look for. Apart from adding any of those keys that are missing, the file holds the plug-in's state unchanged, so hosts that import `.aupreset` files can load it directly.
- The app writes `Session.json` last on every save. Afterwards it removes only the `.aupreset` files that the previous `Session.json` referenced and the new one no longer uses. Other files you put in the folder, such as notes, renders or stems, are left alone.

## Session.json

```json
{
  "app" : { … },
  "createdWith" : "HostSonic μ 1.0.8",
  "format" : "hostsonic.session",
  "macros" : [
    { "parameterAddress" : "4", "target" : "track1.slot3", "value" : 0.42 },
    { "value" : 0 }, { "value" : 0 }, { "value" : 0 }
  ],
  "master" : [ { "bypassed" : false, "kind" : "audio" }, { "bypassed" : false, "kind" : "audio" } ],
  "name" : "Night Drive",
  "sends" : [ { … }, { … } ],
  "tempo" : 112,
  "tracks" : [
    {
      "level" : 0.9, "muted" : false, "solo" : false, "sendA" : 0.25, "sendB" : 0,
      "name" : "Bass",
      "source" : "instrument",
      "slots" : [
        { "bypassed" : false, "kind" : "midi" },
        { "bypassed" : false, "kind" : "midi" },
        {
          "bypassed" : false,
          "kind" : "instrument",
          "plugin" : {
            "component" : { "flags" : 2, "flagsMask" : 0, "manufacturer" : "Moog", "subtype" : "mdD1", "type" : "aumu" },
            "manufacturer" : "Moog",
            "name" : "Model D",
            "preset" : { "name" : "Fat Bass", "number" : 12 },
            "stateFile" : "Track 1 Slot 3 - Model D.aupreset"
          }
        },
        { "bypassed" : false, "kind" : "audio" },
        { "bypassed" : false, "kind" : "audio" },
        { "bypassed" : false, "kind" : "audio" }
      ]
    }
  ],
  "transport" : { "beat" : 32, "paused" : false, "playing" : false },
  "version" : 1
}
```

### Top level

| Key | Type | Meaning |
|---|---|---|
| `format` | string | Always `"hostsonic.session"`. |
| `version` | integer | Format version, `1`. A change that older readers cannot handle increases it. |
| `name` | string | The name as typed. The app uses the folder name instead. |
| `createdWith` | string | App name and version that wrote the file. Informational. |
| `tempo` | number | Beats per minute, 20–999. |
| `transport` | object | `playing` and `paused` (booleans), `beat` (number, playhead position in beats). |
| `tracks` | array | Exactly 4 tracks. |
| `sends` | array | Exactly 2 slots: send A and send B effects. |
| `master` | array | Exactly 2 slots: master insert effects, in processing order. |
| `macros` | array | Exactly 4 macros. |
| `app` | object | Optional. View, device and routing state of this app. Other readers can ignore it. |

The rack layout is fixed. The app rejects a file with a different number of tracks, slots, sends, master inserts or macros.

### Track

| Key | Type | Meaning |
|---|---|---|
| `name` | string | Track name, may be empty. |
| `source` | string | `instrument`, `audioInput` (system audio input), `linkAudio` or `audioFile`. An audio source replaces the instrument slot, so slot 3 is then empty. |
| `linkAudio` | object | When `source` is `linkAudio`: Ableton Link Audio channel, as `peer` and `channel` names plus `peerID` and `channelID`. |
| `audioFile` | object | When `source` is `audioFile`: `file` (file name in the **Audio Files** folder; an audio file or a Standard MIDI File such as `.mid`), `startSeconds` (where playback starts from a stopped transport), `loops` (boolean). |
| `level` | number | Fader position, 0–1. Gain in dB is `level × 60 − 60`; `0` is silence. |
| `muted`, `solo` | boolean | |
| `sendA`, `sendB` | number | Send amount as linear gain, 0–1. |
| `slots` | array | Exactly 6 slots, in signal order: two MIDI processors, the instrument, then three audio effects. |

### Slot

| Key | Type | Meaning |
|---|---|---|
| `kind` | string | `midi`, `instrument` or `audio`. Fixed by position; written for readers. |
| `bypassed` | boolean | |
| `receivesMIDI` | boolean | Optional. An audio effect that also receives the track's MIDI. |
| `plugin` | object | Absent for an empty slot or a MIDI file slot. |
| `midiFile` | object | Optional, for either MIDI slot instead of `plugin`: `fileName` (name in **Audio Files**) and `loops` (boolean, defaults to `true`). Events follow the host tempo and transport and enter the next MIDI processor or instrument/MIDI-enabled FX. Incoming MIDI passes through. |

Slots are addressed by **slot paths**: `track1.slot1` … `track4.slot6`, `sendA`, `sendB`, `master1`, `master2`. Positions are 1-based.

### Plugin

| Key | Type | Meaning |
|---|---|---|
| `name`, `manufacturer` | string | Display names, for people and for finding a replacement. |
| `component` | object | The Audio Unit's `AudioComponentDescription`. `type`, `subtype` and `manufacturer` are four-character codes as text (`"aumu"`), or `"0x"` plus 8 hex digits when a code is not printable. `flags` and `flagsMask` are integers. |
| `preset` | object | Optional. The preset selected in the host's preset menu: `number` and `name`. The actual sound is in the state file. |
| `stateFile` | string | Optional. The `.aupreset` file in this folder. When it is missing, the plug-in loads with its defaults. |

### Macro

| Key | Type | Meaning |
|---|---|---|
| `value` | number | Slider position, 0–1, mapped linearly to the parameter's minimum–maximum range. |
| `target` | string | Optional slot path of the assigned plug-in. |
| `parameterAddress` | string | Optional. The Audio Unit parameter address, as a decimal string. |

Values that can exceed 2^53 (`parameterAddress`, Link `peerID`/`channelID`) are decimal strings so JavaScript readers keep them exact.

### app

The app reads these keys and supplies defaults when they are missing. They describe this device rather than the music, so tools that convert sessions can drop them.

`savedSession` (autosave only: the folder the current session belongs to), `sampleRate`, `transportSamplePosition`, `transportSampleRate`, `selectedTrack` (0-based), `page` (`Rack`, `Mixer`, `Pads`), `pageScrollOffsets`, `padsThroughMIDI`, `padsMPE`, `padPitchBendRange` (semitones), `midiInputTrack` (0-based), `midiInputSourceID`, `allNoteSources`, `syncSourceID`, `syncDestinationID`, `autoSyncInput`, `publicSyncOutput`, `shareLinkMainMix`, and `pluginWindows` (macOS plug-in window frames keyed by slot path).

## Recovering a session without the app

1. Open `Session.json` to see each track's plug-ins, presets, levels, sends and the tempo.
2. In any Audio Unit host, load the same plug-in, then import the slot's `.aupreset` file, or copy it to `~/Library/Audio/Presets/<manufacturer>/<plug-in name>/` on macOS.
3. Audio file tracks name their file in **Audio Files**.
