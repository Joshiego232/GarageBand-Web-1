# GarageBand Web Studio

GarageBand Web Studio is an original, browser-based digital audio workstation inspired by the approachable workflow of GarageBand and the deeper editing and mixing concepts found in professional DAWs. It targets Windows, macOS, and ChromeOS through modern desktop browsers and an installable Progressive Web App.

The project aims to combine an inviting music-making workflow with a real multitrack arrangement, recording, MIDI, sound design, mixing, and export environment. It is not affiliated with Apple and does not include proprietary GarageBand or Logic assets or sound packs.

> **Development status:** the DAW rebuild is in progress. The GitHub repository contains architecture, roadmap, shortcut documentation, and the full-stack product specification. The Lovable Pocket Studio prototype is being upgraded separately. Do not treat the target feature list below as a claim that every feature is already implemented.

## Product goals

- **Arrangement:** multitrack audio, instrument/MIDI, drum, and sampler tracks; musical timeline; waveform and MIDI clips; loop regions; markers; snapping; clip editing.
- **Instruments and sounds:** original procedural instruments and drum kits, playable presets, step sequencing, user-imported samples and loops.
- **Recording and editing:** audio-file import, microphone recording where supported, waveform editing, MIDI note editing, and project recovery.
- **Mixing:** channel strips, track gain and pan, mute/solo, master output, effects, and metering.
- **Interchange:** project backup/restore, MIDI file import/export, stereo WAV bounce, and track stems where supported.
- **Commands:** contextual menus, command search, undo/redo, and platform-aware keyboard shortcuts.
- **Full-stack services:** real authentication, cloud project metadata and versions, private audio-asset storage, and secure local/cloud synchronization.
- **Portability:** local-first project storage and an installable offline-capable PWA. Cloud services should extend local work, not be required to make music.

## Full-stack architecture

The intended application uses a Supabase-backed service layer for authentication, PostgreSQL project metadata/version history, and private object storage for user audio assets. User-owned data must be protected with Row Level Security and private storage policies. Client code must never contain service-role credentials.

The core DAW remains local-first using browser storage so anonymous/offline users can create, edit, save, and export projects. Signed-in users can sync/backup projects across devices, with visible conflict detection and no silent overwrites.

**Backend provisioning note:** the Supabase database has been enabled in the existing Lovable Pocket Studio project. Authentication flows, database schema, RLS policies, object storage integration, and cloud sync remain implementation work until they are actually built and tested.

See [Full-Stack DAW Specification](docs/FULL_STACK_DAW_SPEC.md) for the detailed product requirements and implementation sequence.

## Target platforms and browser requirements

The application is a web app/PWA, not currently a native Windows executable or macOS application.

- **Windows:** current Chromium-based browsers and other browsers supported by the app.
- **macOS:** current Safari and Chromium-based browsers, with capability checks for browser-specific audio/MIDI features.
- **ChromeOS:** current Chrome browser on Chromebook.

Microphone recording requires a secure context (normally HTTPS or localhost), explicit microphone permission, and a compatible input device. Live Web MIDI device access is optional and browser-dependent; MIDI file import/export should not depend on live MIDI device support. Audio file decoding depends on the codecs supported by the current browser. Native VST/AU/AAX plugin hosting and low-level audio-driver integrations are not implied by the browser target.

## Local development

The source tree is being brought into this repository from the Lovable project. Once the application source and package scripts are synced, the intended commands are:

    npm install
    npm run dev
    npm test
    npm run lint
    npm run build

Use the scripts actually defined in package.json; do not assume a command is available before the source sync is complete.

## Documentation

- [Full-Stack DAW Specification](docs/FULL_STACK_DAW_SPEC.md) — full-stack product requirements, backend security, sync, DAW workflows, and build order.
- [Architecture](docs/ARCHITECTURE.md) — intended application modules, audio-engine principles, persistence, and browser capability boundaries.
- [Keyboard shortcuts](docs/KEYBOARD_SHORTCUTS.md) — cross-platform command reference.
- [Roadmap](docs/ROADMAP.md) — staged product development goals.

## Data and privacy

The intended core workflow is local-first: user projects, imported audio, and recordings remain in browser-managed storage unless the user explicitly exports or syncs them. Cloud sync should be opt-in or clearly configurable. GitHub is used for application source and developer collaboration, not for uploading private recordings or project audio.

## Licensing

No open-source license has been selected for this repository. Until one is chosen and added, do not assume the code is licensed for redistribution or commercial reuse.

## Contributions and releases

Changes should preserve project data compatibility, include tests for critical editing/audio behavior, and update documentation to match what is genuinely implemented. Release notes should record browser constraints and distinguish completed behavior from planned work.
