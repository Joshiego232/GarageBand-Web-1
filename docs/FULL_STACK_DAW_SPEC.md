# Full-Stack DAW Product Specification

## Product direction

Build an original browser-first digital audio workstation with the approachable session workflow of GarageBand and the deeper arrangement, editing, and mixing concepts of Logic Pro. It must be an actual full-stack application, not a static UI or frontend-only prototype.

**Target platforms:** Windows, ChromeOS/Chromebook, and macOS through modern browsers and an installable Progressive Web App (PWA). Desktop/laptop is the primary experience, with responsive tablet layouts.

Do not use Apple logos, proprietary GarageBand/Logic artwork, copied sound packs, or imply affiliation with Apple. Do not promise native VST/AU/AAX, ASIO, CoreAudio drivers, or other capabilities that browser APIs cannot provide.

## Required architecture

### Client application
- React/TypeScript DAW workspace with modular arrangement, transport, sound browser, piano roll, step sequencer, audio editor, mixer, inspector, command palette, project dashboard, account screens, and settings.
- Web Audio API audio engine with a look-ahead scheduler, stable transport clock, track routing, effects, metering, recording, and WAV rendering.
- IndexedDB local-first project and media storage.
- PWA installability and offline app shell; core creation/editing/export workflows must remain available offline.
- Capability detection and clear explanations for microphone permission, secure context, codecs, Web MIDI, memory, and browser limitations.

### Backend (Supabase; enabled for the existing Lovable project)
- Real authentication: sign-up, sign-in, sign-out, password reset, session restoration, and profile management.
- PostgreSQL persistence for user profiles, projects, version snapshots, asset metadata, preferences, and optional share/invitation records.
- Private object storage for audio assets, project media, and backup objects.
- Secure server-side operations for project sync, asset authorization, version creation, and share links where implemented.
- Row Level Security on all user-owned tables and private storage policies. A user can access only their own projects/assets unless an explicit share grants access.
- Never expose service-role credentials in browser code.
- Real loading, empty, upload, download, error, offline, sync, conflict, and permission-denied states. No fake login, fake cloud save, or inert account controls.

### Data and sync model
- Local projects are usable without an account or internet connection.
- Authenticated users can upload or sync local projects to cloud, browse cloud projects, download them to another device, rename/delete, and manage snapshots.
- Keep a stable project ID, schema version, content revision, local modified timestamp, cloud modified timestamp, owner ID, and asset references.
- Cloud sync must detect stale revisions and conflicts. Never silently overwrite newer work. Offer a clear choice to keep local, keep cloud, or save a separate copy.
- Autosave locally. Cloud sync should be explicit or have a visible, configurable policy.
- Audio blobs should be stored in private object storage, not embedded as large base64 database fields. Keep metadata, ownership, checksum, MIME type, byte size, duration, sample rate, and storage path in Postgres.
- Provide backup/restore and project version history. Document retention and storage behavior.

## Core DAW workflows

### Transport and arrangement
- Multitrack instrument, drum, sampler, and audio tracks.
- Tempo 40–240 BPM, time signatures, musical key/scale, swing, count-in, metronome, cycle range, markers, bars/beats/ticks and elapsed-time display.
- Reliable play, pause, stop, return-to-start, record, loop/cycle, metronome, and count-in.
- Timeline zoom, horizontal scroll, snap grids, track height, region move/resize/trim/split, duplicate, loop, delete, multi-select, copy/cut/paste, track reorder/rename/duplicate/delete.
- Project autosave, crash recovery, undo/redo history, and clear dirty/saved state.

### Instruments and MIDI
- Original procedural piano, electric keys, bass, synth, pad, strings, pluck, and drum instruments.
- Sound browser with categories, search, preset audition, and drag/drop.
- Piano roll with note draw/select/move/resize, velocity, duration, quantize, grid, scale highlighting, and undo/redo.
- Step sequencer with pattern editing, per-step enablement and velocity.
- MIDI file import/export independent of live Web MIDI device support.
- Musical typing keyboard and optional browser-supported Web MIDI controller access.

### Audio recording and editing
- Microphone capture with explicit permission and HTTPS/secure-context handling.
- Input monitoring where browser capabilities permit.
- Audio import and drag/drop, waveform display, trim, split, fades, region gain, loop, and audio editor.
- Recording should place new takes on the armed track and preserve previous takes.
- Clearly identify unsupported or approximate time-stretch/pitch-shift operations rather than presenting fake controls.

### Mixer and effects
- Channel strips with gain, pan, mute, solo, meters, master output, EQ, filter, compression, saturation/drive, reverb, and delay.
- Effects must be wired into the actual audio graph and saved with the project.
- Show peak/clipping feedback and avoid runaway feedback or unexpected volume spikes.

### Export and interchange
- Stereo WAV mix, track stems, MIDI export, and complete project backup/restore with media.
- Progress and errors for long render/export operations.
- Preserve media identity, sample rate/channel metadata, track settings, and tempo/signature information where supported.

## Command system
- Full searchable command palette with categories and shortcut hints.
- Complete File, Edit, Track, Region, Record, Mix, View, Window, and Help menus.
- Platform-aware modifiers: Ctrl on Windows/Chromebook, Command on macOS.
- Shortcuts for transport, save/open/new, undo/redo, import/export, select/deselect, cut/copy/paste, duplicate/delete, split, quantize, zoom/fit, nudge, track navigation, panel toggles, piano roll, step sequencer, mixer, musical typing, metronome, cycle, command search, and help.
- Allow shortcut remapping and reset-to-defaults.
- Never trigger global shortcuts while a user is typing in an input or editable field. Avoid common browser/system shortcut collisions.
- Menus, buttons, context menus, and shortcuts must invoke real functionality. Do not leave decorative/inert controls.

## Product UI
- Refined professional studio visual language: dark charcoal surfaces, restrained teal/mint accent, clean typography, subtle dividers, crisp waveforms, coherent iconography, meaningful hover/focus/pressed states.
- Main workspace: project/system bar, menus, transport, sound library, arrangement, contextual inspector, lower editor, and status bar.
- Project dashboard: local projects, cloud projects, recent sessions, templates/demo sessions, account/storage status, and settings. It should feel like part of a music product, not a generic SaaS admin template.
- Responsive panel sizing, show/hide, context menus, right-click, drag/drop, keyboard navigation, tooltips, and accessible labels.

## Quality gates
- Preserve existing functional code and project data compatibility; do not replace the DAW with a toy demo.
- Modular typed architecture: project model/migrations, transport/scheduler, audio graph, recording, persistence, sync, commands, export, and UI.
- Tests for scheduler timing, clip edits, project validation/migration, local persistence, backend ownership/RLS, sync conflicts, shortcut mapping, MIDI import/export, and WAV rendering as practical.
- Run typecheck/lint/tests/production build and fix errors. State clearly when any test could not be run.
- Document setup, architecture, database schema, RLS/storage security, offline and sync behavior, browser capability matrix, keyboard shortcuts, and release process.
- Keep a clear working-vs-planned feature matrix. Never describe a roadmap item as implemented until tested.

## Build order
1. Audit existing Pocket Studio code and protect working features.
2. Establish auth, schema, RLS, private storage, and safe cloud project operations.
3. Complete local project save/recovery and tested cloud/local sync.
4. Stabilize audio engine, transport, arrangement, MIDI, recording, effects, and export.
5. Complete the workspace, command palette, dashboard, account settings, and responsive polish.
6. Add tests, docs, PWA/offline validation, and CI.

## Current implementation note
This document is a product specification, not proof of completion. The Supabase database has been enabled in the existing Lovable Pocket Studio project. The Lovable workspace currently reported that it had no credits available when asked to run the implementation; app changes must be validated after the workspace can accept a build request.
