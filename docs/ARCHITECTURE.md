# Architecture

This document describes the intended architecture for GarageBand Web Studio. Keep it aligned with the actual implementation as the product evolves; do not document a feature as complete until its code and tests exist.

## Product shape

GarageBand Web Studio is a local-first, installable browser DAW for desktop browsers on Windows, macOS, and ChromeOS. The browser/PWA is the cross-platform delivery target; this repository does not claim to produce native Windows or macOS binaries.

Core creation and playback should not require an account, server database, or network connection after the app shell is available. Project metadata and imported media belong in browser-managed local storage. GitHub stores source code, tests, documentation, and build tooling—not user recordings or private project audio.

## Main subsystems

- **Studio shell:** menus, transport, library, arrangement, inspector, lower editors, dialogs, preferences, and command palette.
- **Project model:** versioned, validated document schema for tempo, meter, tracks, clips, automation, regions, notes, markers, mixer state, and media references. Add explicit migrations when the schema changes.
- **Audio engine:** one coordinated `AudioContext`; track/channel graph; instrument and sample voices; effects; metering; transport clock; look-ahead scheduler driven by `AudioContext.currentTime`. UI state must not be the audio clock.
- **Arrangement engine:** musical-time conversion, snapping, clip geometry, selection, split/trim/move/duplicate, loop regions, markers, and track ordering.
- **Media store:** IndexedDB for project documents and imported media Blobs. Use object URLs only as temporary presentation resources and release them when no longer needed.
- **Recording:** permission-gated microphone capture, record-arm and monitoring state, stream lifecycle cleanup, and error handling.
- **Instrument and sample library:** original procedural instruments and generated demo content, plus user-imported samples and loops. Do not bundle proprietary GarageBand sounds or UI assets.
- **Editors:** piano roll/MIDI, drum step editor, audio waveform editor, sampler controls, and mixer.
- **Import/export:** browser-decoded audio import, MIDI file parsing/writing, project backup/restore, offline stereo WAV rendering, and stem rendering where supported.
- **Command system:** central command registry used by menus, shortcut manager, and command palette so each command has one implementation.
- **PWA shell:** manifest, icons, offline shell cache, update lifecycle, and install affordance with graceful fallback.

## Audio-engine principles

1. Create and reuse a single active audio context per studio session where practical.
2. Schedule events ahead against audio-context time; use timers only to wake the scheduler, never as the musical clock.
3. Keep engine nodes and media streams outside transient component render lifecycles.
4. Make graph changes incremental. Do not tear down the master graph for every UI state update.
5. Respect autoplay restrictions and explicitly resume the context from a user gesture.
6. Make stop, dispose, route changes, and unmount paths release nodes, timers, streams, and object URLs.
7. Render exports from the same project data and routing model used by live playback.

## Persistence and file formats

- Store large media as IndexedDB Blobs rather than base64 strings in localStorage.
- Version project backup files and validate data before opening them.
- Autosave without blocking the audio thread; show save status and provide recovery when a prior save was interrupted.
- Never upload private audio to a remote service implicitly.
- Use WAV as the dependable uncompressed export target. Offer other encoded formats only when a real encoder is present and tested.

## Browser capability boundaries

AudioWorklet is broadly available in modern browsers but requires a secure context for worklet processing. Microphone access through `getUserMedia()` requires a secure context and user permission. Web MIDI is not available consistently across major browsers, so MIDI file import/export must not depend on live device access. Detect capabilities at runtime and make unsupported features explicit rather than presenting inactive controls.

## Quality gates

Before a release, run type checking, lint, unit tests, and a production build. Test the most important real workflows: create/open a project, add tracks, import audio, arrange clips, record where permitted, play multiple tracks together, mix, save/reopen, and render WAV. Record browser-specific limitations in the release notes.
