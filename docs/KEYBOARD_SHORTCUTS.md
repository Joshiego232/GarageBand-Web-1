# Keyboard shortcuts

This is the target shortcut map for GarageBand Web Studio. The app should display the correct modifier for the current platform: **Command (⌘)** on macOS and **Ctrl** on Windows/ChromeOS. Avoid triggering global shortcuts while focus is inside a text field, number field, editable name, or other control that is consuming keystrokes.

| Action | Windows / ChromeOS | macOS |
|---|---|---|
| Play / Stop | Space | Space |
| Record | R | R |
| Toggle cycle / loop | L | L |
| Toggle metronome | M | M |
| Save project | Ctrl + S | ⌘ + S |
| Save As / duplicate project | Ctrl + Shift + S | ⌘ + Shift + S |
| Undo | Ctrl + Z | ⌘ + Z |
| Redo | Ctrl + Shift + Z | ⌘ + Shift + Z |
| Cut | Ctrl + X | ⌘ + X |
| Copy | Ctrl + C | ⌘ + C |
| Paste | Ctrl + V | ⌘ + V |
| Select All | Ctrl + A | ⌘ + A |
| Import audio | Ctrl + I | ⌘ + I |
| Export / Bounce | Ctrl + E | ⌘ + E |
| Open command palette | Ctrl + Shift + P | ⌘ + Shift + P |
| Toggle piano keyboard | Ctrl + K | ⌘ + K |
| Duplicate selected region/track | D | D |
| Delete selection | Delete / Backspace | Delete / Backspace |
| Zoom in / out | + / - | + / - |
| Nudge selection | Arrow keys | Arrow keys |
| Return playhead to start | Home | Home |
| Toggle lower editor | E | E |
| Toggle mixer | X | X |
| Close dialog / cancel tool | Esc | Esc |

## Shortcut behavior

- Shortcuts should call the same central commands exposed by menus and the command palette.
- Space should not insert a space into a focused text control.
- Undo/redo must operate on project-edit history, not browser navigation.
- Delete should remove selected clips/notes/tracks only when an editable studio selection is active.
- Platform detection should choose the appropriate modifier without changing the underlying command.
- Show shortcuts beside menu commands and in the Help panel.
