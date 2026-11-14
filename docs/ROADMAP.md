# Product roadmap

This roadmap describes the intended progression of GarageBand Web Studio. Items are not considered shipped solely because they appear here; release notes and tests should identify what is actually available.

## Foundation

- Local-first project model with schema versioning, IndexedDB persistence, autosave, backup, and restore.
- One shared audio context, reliable transport, look-ahead scheduling, and resource cleanup.
- Track/channel routing with gain, pan, mute, solo, master bus, and meters.
- Original demo project and procedurally generated instrument/drum sounds.

## Arrange and create

- Multitrack audio, instrument/MIDI, drummer, and sampler tracks.
- Timeline ruler, bar/beat grid, playhead, loop region, markers, selection, zoom, and snapping.
- Clip creation, drag/move, trim, split, duplicate, delete, and undo/redo.
- Piano roll with note editing and velocity.
- Drum step sequencer with velocity and swing.
- Searchable sound library, sample import, and audition.

## Record and mix

- Microphone recording to armed audio tracks with clear permission states.
- Waveform editing and clip controls.
- Mixer channel strips, inserts/sends, and master.
- Functional EQ, compressor, reverb, tempo-synced delay, filter, and saturation.
- Stereo WAV bounce and per-track stem export.

## Interchange and portability

- Audio import using formats supported by the browser decoder.
- MIDI file import/export with editable MIDI regions.
- Portable, versioned project backup/restore.
- Installable PWA, offline app shell, and update lifecycle.
- Cross-platform command map for Windows, macOS, and ChromeOS.

## Later enhancements

- Optional live MIDI input/output where browser capability and permission allow it.
- Shortcut customization and advanced automation lanes.
- Additional original instruments, sound packs, and effect presets.
- More complete audio editing, quantization, comping, and advanced routing.
- Performance profiling for large projects, long audio files, and low-memory devices.

## Release discipline

A release should publish a concise changelog, note browser/device constraints, and distinguish implemented behavior from planned behavior. Never describe a control as functional until its action has been exercised in the app and covered by an appropriate test.
