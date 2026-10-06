# Changelog

What changed in each PatchPro 2 release. The newest version is at the top. Download releases from the
[Releases page](https://github.com/nelsonbernard/patchpro2-releases/releases); the
[User Guide](docs/USER-GUIDE.md) explains every feature.

<!-- Release notes: CI uses the section for the tagged version as the release's description. Add changes under
"Unreleased" as they are made; at release time rename it to "## [x.y.z] - YYYY-MM-DD". -->

## [Unreleased]

## [0.3.0] - 2026-10-06

PatchPro now runs on **Windows 10 (22H2) and Windows 11** as well as Linux.

### New: Windows

- **Installer and portable version:** `PatchPro2-Setup-0.3.0-x64.exe` installs PatchPro for your user (no
  administrator rights) with Start menu and desktop shortcuts; `PatchPro2-0.3.0-x64-portable.zip` runs from any
  folder. Running a newer setup updates in place and keeps your settings. The files are not signed yet, so
  Windows SmartScreen may warn ("More info › Run anyway").
- **Routing on Windows:** an app routed to one output is moved by Windows itself (no added delay); fan-out to
  several outputs, microphones to outputs and the virtual sink to outputs are copied by PatchPro (about 60 ms later
  than the app's own output). An app with no routes is muted.
- **Virtual sink:** [VB-Audio Virtual Cable](https://vb-audio.com/Cable/) (free, installed separately) appears as
  one virtual sink you can rename, set the volume of and route through.
- **Default devices** include the **communications** output and input used by voice chat apps.
- **Apps that pick a fixed device** (some games and voice apps) are marked "Restart the app to apply".
- **Safe quit:** quitting PatchPro, a crash, an update or an uninstall puts back each app's own device choice, the
  mutes PatchPro made, and your default devices.
- Volumes in percent, like Windows' own sliders; apps appear once they play or record; Voicemeeter's many devices
  start hidden.
- **Voicemeeter is not needed.** PatchPro avoids conflicts with it, but we recommend uninstalling Voicemeeter and
  keeping VB-Audio Virtual Cable (a separate product).
- Tray, mute hotkey, scenes, meters and spectrum, start at login, start minimized, update notices and diagnostics
  work as on Linux.

### New: all platforms

- **Hide devices** you don't use: **Hide this device** in the inspector; hidden devices are listed under
  **Hidden** at the bottom of the device list, where you can show them again. Hiding a device also removes its
  routes.

### Fixed

- Settings: the mute shortcut field and the mute device menu could lose focus while you used them (seen on
  Windows).
- Typing in PatchPro no longer triggers the window's hidden default menu shortcuts (Ctrl+M minimized the window,
  Ctrl+R reloaded it).
- **Start at login** is restored when PatchPro starts if its entry went missing (for example after reinstalling),
  and follows an AppImage or portable copy you moved.
- Quitting no longer waits forever if the audio engine's shutdown stalls (it gives up after 3 seconds).

## [0.1.0] - 2026-10-05

First release, for Linux with PipeWire: patchbay with Graph, Console and Matrix views; routing of apps, devices and
virtual sinks; volume and mute; live meters and spectrum; scenes; tray icon; global mute hotkey; start at login;
update notices; diagnostics. AppImage and `.deb`.

0.2.0 was not released.
