# Changelog

What changed in each PatchPro 2 release. The newest version is at the top. Download releases from the
[Releases page](https://github.com/nelsonbernard/patchpro2-releases/releases); the
[User Guide](docs/USER-GUIDE.md) explains every feature.

<!-- Release notes: CI uses the section for the tagged version as the release's description. Add changes under
"Unreleased" as they are made; at release time rename it to "## [x.y.z] - YYYY-MM-DD". -->

## [0.5.0] - 2026-10-06

Voice chat apps such as Discord are now one violet card with a microphone side and a sound side, with a
step-by-step setup guide; Windows supports every VB-Audio cable as its own virtual sink; and cables no longer blink
while you talk.

### New

- **Windows: more virtual sinks.** Every VB-Audio virtual cable you install becomes its own virtual sink: the free
  VB-CABLE plus the A+B and C+D packs ("Virtual Cable", "Virtual Cable A", …), each with its own name and volume.
  Scenes never install or remove cables; a scene that uses a cable this PC doesn't have says so.
- **Apps that play and record are one card:** a voice chat app such as Discord appears in the Graph as one
  violet card instead of two: connect what it should hear (your microphone, a virtual sink) to its left port and
  its sound to an output from its right port. Each side keeps its own level and volume. While the app plays or
  records nothing, that side shows as idle (the card stays together); a side with nothing connected says so and
  what to connect. New cards start in the middle column, and the **?** legend lists the new color.
- **Voice chat setup guide:** a new User Guide section walks through setting up Discord (and Teams, Zoom) step by
  step, on Windows and Linux.
- **Windows: voice chat device notice.** When Windows' communications device (what Discord and other voice apps
  use on "Default") differs from the default device, a notice says so; **Match** makes them the same and keeps it
  after PatchPro quits.

### Changed

- **One inspector for a voice chat app:** clicking a violet card shows the app by its name with both sides
  (Microphone and Sound: levels, faders, routes, hints), the clicked side highlighted; **Hide this app** hides both.
- **Clearer names for app recordings:** "Discord (capture)" is now "Discord · recording"; the two sides of a violet
  card are listed as "Discord · microphone" and "Discord · sound", in violet like the card: the Console gives them
  their own **Apps (play & record)** group (microphone, then sound), and the Matrix marks them "receives ↓" (a
  column) and "sends →" (a row).
- **Voice chat apps in the device list:** a new violet **Apps (play & record)** section lists each one once (with its
  microphone and sound levels), and the **Hidden** list shows a hidden app once; showing it brings both sides back.
- **Steady cables:** a cable (and a Matrix crosspoint) stays lit for 1.5 seconds after its sound stops, so a
  microphone route no longer blinks between words.
- **The inspector opens on a click only:** moving a card or drawing a cable no longer opens it over the area you
  are working in.
- **Windows:** recording apps (such as "OBS · recording") show a moving level bar where the spectrum would be,
  so you can see the audio they receive.
- **Windows:** the hint for an app that did not follow its route now says what to do: set the app's device to
  **Default** in its own settings, or restart it (restarting alone does not help when the app is set to a specific
  device, such as Discord's input set to your microphone).

## [0.4.1] - 2026-10-06

Voice apps on Windows now follow your routes.

### Fixed

- **Windows:** voice apps such as Discord now follow PatchPro's routes right away, for their microphone and their
  sound (PatchPro also sets the app's communications device, which they use): for example Discord recording a
  mix from the virtual cable.
- **Windows:** recording apps that only switch input when they start now get the "Restart the app to apply" hint.

## [0.4.0] - 2026-10-06

Every scene now keeps its own look, hiding a device no longer loses its routes, and the window makes better use
of its space.

### New

- **Each scene keeps its own look:** hidden devices, card positions and the view (Graph, Console, Matrix) are
  saved with each scene and switch when you load it, along with its routes, volumes and mutes.
- **Unsaved changes are visible:** a dot (•) next to the scene name in the title bar shows the setup differs from
  the active scene; the **save icon** beside it saves the changes in one click (with no scene active, it asks for
  a name). Saving as a new scene leaves the scene you started from as it was.
- **More room:** the inspector is hidden when nothing is selected and slides in over the right side when you
  select a device or cable (close it with ✕, Esc or a click on empty space). The color legend and shortcuts moved
  to the **?** in the status bar.

### Changed

- **Hiding a device pauses its routes** instead of removing them: they stay in your setup and scenes, carry no
  sound while the device is hidden, and play again when you show it.
- **Matrix view:** every destination column has the same width; a long device name wraps to two lines instead of
  widening its column, so the crosspoints line up under their device.
- **Windows:** the "One device per process" switch is gone (Windows routes an app's processes together, and
  browsers use one audio process); an app split earlier still shows it so you can turn it off.

### Fixed

- **Narrow windows:** the title bar no longer wraps or overflows; below 1280 px it hides Rate, Buffer and
  Latency, below 1100 px the view buttons show only their icons, and the window buttons always stay visible.
- **Windows:** hiding an app, or every device it plays to, now silences it (it kept playing on the default
  output), in the patchbay and when loading a scene; showing it again brings the sound back.
- **Windows:** an app PatchPro silences because it has no route is no longer reported, autosaved or saved in a
  scene as muted, so it is not left muted once it has a route again.

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
