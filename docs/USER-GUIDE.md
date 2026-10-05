# PatchPro 2 User Guide

PatchPro 2 is a patchbay for your Linux desktop's audio. It shows every source of sound (microphones, apps)
and every destination (headphones, speakers, HDMI), and lets you connect them with cables, mix them in
virtual sinks, set volumes, and save the whole setup as scenes.

**Contents**

1. [Before you start](#1-before-you-start)
2. [Installing](#2-installing)
3. [First start: what happens to your audio](#3-first-start-what-happens-to-your-audio)
4. [The window](#4-the-window)
5. [Devices](#5-devices)
6. [Routing audio](#6-routing-audio)
7. [Volume, mute and meters](#7-volume-mute-and-meters)
8. [The three views](#8-the-three-views)
9. [The inspector](#9-the-inspector)
10. [Virtual sinks](#10-virtual-sinks)
11. [Apps and browser tabs](#11-apps-and-browser-tabs)
12. [Default devices](#12-default-devices)
13. [Scenes](#13-scenes)
14. [The tray icon](#14-the-tray-icon)
15. [The mute hotkey](#15-the-mute-hotkey)
16. [Settings reference](#16-settings-reference)
17. [Keyboard and mouse reference](#17-keyboard-and-mouse-reference)
18. [Where PatchPro keeps its files](#18-where-patchpro-keeps-its-files)
19. [Desktop support](#desktop-support)
20. [Troubleshooting](#20-troubleshooting)
21. [Uninstalling](#21-uninstalling)

---

## 1. Before you start

PatchPro needs:

- **64-bit Linux** with **PipeWire** (the sound server) and **WirePlumber** (its session manager) running. They
  are the default on most current distributions. To check, run:
  ```sh
  systemctl --user status pipewire wireplumber
  ```
  Both should say `active (running)`.
- **Tested setup:** KDE Plasma 6 on Wayland, PipeWire 1.6, WirePlumber 0.5. Routing works the same everywhere;
  the tray icon and global hotkey depend on your desktop ([Desktop support](#desktop-support)).

PatchPro **controls the routing** of your system's audio. It does not add effects or process the sound itself.

## 2. Installing

Download the latest release from the [Releases page](https://github.com/nelsonbernard/patchpro2-releases/releases/latest).

### AppImage (any distribution)

1. Make it executable and run it:
   ```sh
   chmod +x PatchPro2-*-x86_64.AppImage
   ./PatchPro2-*-x86_64.AppImage
   ```
2. AppImages need FUSE 2. If it doesn't start, install it:

   | Distribution | Command |
   |---|---|
   | Ubuntu 24.04+ / Debian 13+ | `sudo apt install libfuse2t64` |
   | Older Ubuntu / Debian | `sudo apt install libfuse2` |
   | Fedora | `sudo dnf install fuse-libs` |
   | Arch | `sudo pacman -S fuse2` |

Keep the AppImage in a fixed place (for example `~/Applications/`). "Start at login" and the mute hotkey remember
where it is; if you move it, start it once from the new place.

### Debian / Ubuntu (.deb)

```sh
sudo apt install ./PatchPro2-*-amd64.deb
```

This installs PatchPro 2 into your application menu, along with its one extra requirement, the PipeWire client
library (`libpipewire-0.3`).

## 3. First start: what happens to your audio

When PatchPro starts, it **takes over routing**:

- Whatever is playing keeps playing where it is. PatchPro adopts your current routing, so nothing changes
  audibly.
- From then on, PatchPro decides where each app's audio goes. New apps start on your **default output**
  (recording apps on your **default input**). You can then route them anywhere.
- If you move an app in your desktop's sound settings (for example KDE's "play this app on…"), PatchPro follows
  that change and shows the new route.

When PatchPro **quits** (or crashes), your system's normal routing comes back on its own: every app returns to
the default device. PatchPro's virtual sinks only exist while it runs; they come back next time from your saved
setup ([Scenes](#13-scenes)).

> A brand-new app may be heard on the default device for a split second before PatchPro routes it. That's
> expected.

## 4. The window

![The main window](images/graph.png)

| Area | What it shows |
|---|---|
| **Title bar** | <ul><li>The **scenes menu** (shows the active scene, or "Current setup").</li><li>The **view switcher**: Graph, Console, Matrix.</li><li>Live audio stats: **Rate** (sample rate), **Buffer** (samples per cycle), **Latency** (buffer time).</li><li>The **engine status** (Running, Starting, Restarting, Waiting, Failed; hover for details).</li><li>The **Settings** gear and the window buttons.</li></ul> |
| **Device list** (left) | Every device grouped as Inputs, Virtual sinks and Outputs, with a live level bar. "DEFAULT" marks the system default output and input; a red speaker icon marks a muted device. Click a device to select it. **New virtual sink** at the bottom. |
| **Main area** | The current view: Graph, Console or Matrix ([The three views](#8-the-three-views)). |
| **Inspector** (right) | Details and controls for the selected device or cable ([The inspector](#9-the-inspector)). With nothing selected: a color legend and the keyboard shortcuts. |
| **Status bar** (bottom) | How many devices and routes there are, how many routes are carrying sound right now, and a hint for the current view. |

If the audio engine is not running, a banner under the title bar says why and what to do
([Troubleshooting](#20-troubleshooting)). Double-click empty space in the title bar to maximize or restore the
window.

## 5. Devices

PatchPro shows three kinds of devices. Their colors follow you through every view:

| Kind | Color | What it is | Ports |
|---|---|---|---|
| **Input** (orange) | 🟠 | Something that produces sound: a microphone, a capture card, or an **app** that plays sound (browser, game, music player). | One output port (right side). |
| **Virtual sink** (white) | ⚪ | A software device you create: it receives sound like an output and sends it on like an input. Use it to combine sources. | Input port (left) and output port (right). |
| **Output** (teal) | 🔵 | Somewhere sound goes: headphones, speakers, HDMI, or an **app** that records (Discord's microphone, OBS, a recorder). | One input port (left side). |

Apps appear and disappear as they start and stop. PatchPro remembers their routes and volume, so an app that
restarts goes back where it was.

## 6. Routing audio

A **route** is a cable from a source to a destination: sound flows from the left card's right port into the right
card's left port.

- **Connect:** drag from a source's port (right side of an input or virtual sink) to a destination's port (left
  side of an output or virtual sink). In the Console and Matrix views, use the send buttons or crosspoints instead.
- **Fan out:** one source can go to several destinations (music to headphones *and* the stream mix).
- **Mix:** several sources can go into one destination.
- **Disconnect:**
  - click a cable to select it, then press **Delete** (or **Backspace**);
  - or click the **×** next to a route in the inspector;
  - or click a lit button or crosspoint again in Console or Matrix.
- **Mono and stereo** are matched automatically (a mono mic plays on both sides of stereo headphones).
- **Feedback loops** are refused (e.g. A → B → A). PatchPro tells you "Would create a feedback loop".

An app with no route at all is silent. That can be what you want, but if something seems to be playing and you
hear nothing, check that it has a cable.

## 7. Volume, mute and meters

- **Volume:** every device card, console strip and matrix row has a fader. The readout is in dB: 0 dB is full
  volume, and the scale matches your desktop's volume sliders.
  - Hardware with its own volume control (many USB headsets and microphones) is set on the device itself, so the
    change also shows in your system sound settings.
- **Mute:** click the speaker button next to a fader, or select a device and press **M**. Muted devices show a
  red icon.
- **Meters:** level bars and a **spectrum** (32 bands from 20 Hz to 20 kHz) show the sound actually flowing
  through each device. The level bars run from −60 dB to 0 dB.
  - Meters run only while the window is visible, so a hidden PatchPro uses almost no CPU.
  - To show levels, PatchPro records from each device while the window is visible, microphones included. These
    recordings are listed in your desktop's sound settings under **"PatchPro 2"** as **"Meter: *device*"**, and
    they can make the "microphone in use" indicator appear. Settings › Privacy can hide them
    ([Settings](#16-settings-reference)).

## 8. The three views

Switch views with **Graph / Console / Matrix** in the title bar. All three show and change the same setup.

### Graph

![Graph view](images/graph.png)

Device cards in three columns (inputs, virtual sinks, outputs) with cables between them.

- Drag a card by its header to arrange your layout; PatchPro remembers where you put each card.
- Each card has a spectrum, a fader, a mute button, and the device's format (sample rate, mono/stereo).
- Cables carrying sound are bright, with dots moving along them; silent routes are faint and dashed.

### Console

![Console view](images/console.png)

A mixing desk: one strip per device, grouped as inputs, virtual sinks and outputs.

- **SEND TO** (inputs and virtual sinks) and **RECEIVE FROM** (outputs) list every possible route as a button.
  Click a button to connect or disconnect; lit buttons are active routes.
- Tall faders with a dB scale, a live level bar, and a **MUTE** button.
- Faders work with the mouse or the arrow keys (click a fader, then press ↑/↓).
- **+ New sink** (between the groups) creates a virtual sink.

### Matrix

![Matrix view](images/matrix.png)

A grid with sources as rows and destinations as columns. Each crosspoint is a possible route: click to connect
or disconnect. Filled crosspoints are active routes; a slash marks combinations that cannot be routed (a virtual
sink into itself). Rows and columns have their own faders and mute buttons. **+ Sink** adds a virtual sink.

## 9. The inspector

![The inspector with a virtual sink selected](images/inspector.png)

Select a device (click its card, its name in the device list, or its row/strip) to see and change:

| Section | What it does |
|---|---|
| **Device name** | The name. Only virtual sinks can be renamed: click the name, type, press **Enter** (**Esc** cancels). |
| **Device / Format / Level** | What it is (hardware name, "Application", "Virtual sink"), sample rate and channels, and the current level. |
| **Spectrum** | A larger live spectrum (20 Hz to 20 kHz). |
| **Gain** | Volume fader and mute button. |
| **Default output / Default input** | For outputs, virtual sinks and inputs: **Set as default** makes it the system default (see [Default devices](#12-default-devices)). |
| **Streams** | For apps: the **One device per stream** switch (see [Apps and browser tabs](#11-apps-and-browser-tabs)). |
| **Receives from / Sends to** | Every route into and out of the device. Click **×** to remove one. |
| **Delete virtual sink** | For virtual sinks: removes it and its routes. |

Click a cable to select a route; press **Delete** to remove it. **Esc** clears the selection.

## 10. Virtual sinks

A virtual sink is a software device: sources go in, and its output can be routed anywhere. Use one to:

- build a **stream mix** (mic + game + music) and send it to OBS or a recording app;
- make a **voice chat bus** that Discord listens to, separate from what you hear;
- give a group of apps one shared volume fader.

How to:

- **Create:**
  - **New virtual sink** in the device list (or **+ New sink** in Console, **+ Sink** in Matrix);
  - new sinks are stereo, named "Virtual Sink 1", "Virtual Sink 2"…
- **Rename:** select it and edit the name in the inspector. Names must be unique.
- **Route** into it like an output and out of it like an input. Its own fader sets the level of the whole mix.
- **Use it as a microphone in other apps:**
  - virtual sinks are visible to every app on your system;
  - in OBS, Discord, a browser or a recorder, choose **"Monitor of *sink name*"** (or the sink's name in the
    input list, depending on the app);
  - or route the virtual sink to the app directly in PatchPro (the app appears as an output while it records).
- **Delete:** select it and click **Delete virtual sink** (or press **Delete**). Apps that played only into it
  go silent (they are not paused) until you route them somewhere else.

Virtual sinks exist only while PatchPro runs. When PatchPro starts, it recreates them from your saved setup.

## 11. Apps and browser tabs

Every app that plays sound appears as an **input**, and every app that records appears as an **output**. An app
is usually one device, even if it has several sound streams (a browser with several tabs playing).

**One device per stream:** select the app and turn on **One device per stream** in the inspector. Each stream (for
example each browser tab) then appears as its own device, so you can send one tab to your headphones and another
to your stream mix.

- When you split an app, every stream starts with the app's routes.
- New streams of a split app (a newly opened tab) also get the app's routes.
- Turning the switch off brings the app back to the routes it had before you split it.

**New apps** start on your default output (recording apps listen to your default input). Change the default to
change where new apps go.

## 12. Default devices

The **default output** is where new apps play; the **default input** is what new recording apps listen to. These
are your system's defaults: changing them in PatchPro also changes them for your desktop, and the other way
around.

Set them in the inspector (**Set as default output** / **Set as default input**). A virtual sink can be the
default output. "DEFAULT" in the device list marks the current ones.

## 13. Scenes

![The scenes menu](images/scenes.png)

A **scene** is a complete setup: your virtual sinks, every route, the volume and mute of every device (hardware
included), which apps are split per stream, and the default devices.

### The current setup is always saved

PatchPro saves your setup automatically about a second after every change, and restores it whenever it starts
(and after an engine restart). Turn that off in Settings › Startup if you prefer starting from the system's
current routing.

### Named scenes

Open the scenes menu in the title bar (it shows the active scene's name, or "Current setup"):

- **Save:** type a name in "Save current setup as…" and click **Save**. Saving under an existing name replaces
  that scene.
- **Load:** click a scene. PatchPro creates or removes virtual sinks, changes routes and sets volumes to match. A
  checkmark shows the active scene. You can also switch scenes from the tray menu.
- **Hover a scene for more:**
  - **update** it with the current setup (save icon);
  - **rename** it (pencil);
  - **delete** it (trash, then confirm).

What to expect when loading:

- **Apps the scene doesn't mention keep their routes.** A scene saved before you installed Discord won't silence
  Discord.
- **Devices that aren't there now are not forgotten.** An app that isn't running, or an unplugged headset, gets
  its routes and volume from the scene when it appears.
- **Playing audio isn't interrupted** by switching scenes, even when a scene removes a virtual sink an app was
  playing into.
- If part of a scene can't be applied (for example a route that would make a loop), the rest is applied and a
  message says what was skipped.

## 14. The tray icon

PatchPro puts an icon (orange waveform) in your system tray:

- **Left-click:** show or hide the window.
- **Right-click** opens the menu:
  - **Show PatchPro / Hide PatchPro**;
  - **Scenes:** load a scene;
  - **Mute *device*:** toggles the mute device chosen in Settings › Mute;
  - **Update available…** (when there is one): opens the release page;
  - **Quit PatchPro:** quits for real; routing goes back to the system.
- The icon turns **grey with a red slash** while the mute device is muted.

With **Close button hides to the tray** on (the default), the window's ✕ hides PatchPro instead of quitting, so
your routing keeps working. The first time, a notification reminds you that it's still running. Use **Quit
PatchPro** in the tray menu to quit.

## 15. The mute hotkey

Set a global shortcut in **Settings › Mute** that mutes and unmutes a device from anywhere, even while PatchPro
is hidden. It's handy as a push-to-mute for your microphone.

1. Choose the device under **Device the tray and the mute hotkey toggle**. "Default input" follows your system's
   default microphone.
2. Click **Set shortcut** and press the key combination: a modifier (Ctrl, Alt, Super) with a key, or an F-key on
   its own. **Esc** cancels; **Clear** removes the shortcut.
3. On Wayland, your desktop asks once to confirm the shortcut. Accept it.

Each press toggles the device and shows a short notification ("Yeti Mic muted"); the tray icon changes too. The
status line under the shortcut says whether it's **Active**.

On Wayland the **desktop owns global shortcuts**. If you change the key in your desktop's settings (KDE: System
Settings › Shortcuts › PatchPro 2), that key wins, and PatchPro's Settings shows the key the desktop actually
uses.

## 16. Settings reference

Open Settings with the gear in the title bar. Changes apply and are saved immediately.

![Settings](images/settings.png)

| Setting | Default | What it does |
|---|---|---|
| **Startup › Restore the last setup when PatchPro starts** | On | Virtual sinks, routes and volumes come back as you left them. Off: PatchPro starts from the system's current routing (your saved scenes are kept). |
| **Startup › Start PatchPro when you log in** | Off | Adds PatchPro to your desktop's autostart (`~/.config/autostart/app.patchpro2.desktop`). Only in the installed app; a development build shows it disabled. |
| **Startup › Start minimized to the tray** | Off | PatchPro starts with the window hidden; open it from the tray icon or by launching PatchPro again. Ignored if there is no tray. |
| **Window › Close button hides to the tray** | On | ✕ hides the window and routing keeps running; quit from the tray. Off: ✕ quits and the system takes routing back. |
| **Mute › Device the tray and the mute hotkey toggle** | Default input | The device for the tray's Mute item and the hotkey. App streams are not offered (they come and go). |
| **Mute › Mute hotkey** | None | A global shortcut for that device ([The mute hotkey](#15-the-mute-hotkey)). |
| **Privacy › Hide meter streams from system sound settings** | Off | Marks PatchPro's meter recordings the way system mixers mark theirs, so KDE's and GNOME's sound settings don't list them. This also hides PatchPro from the "microphone in use" indicator. |
| **Updates › Tell me when a new version is available** | On | Asks GitHub once a day for the latest release. Nothing about you or your setup is sent. PatchPro never updates itself; a notice and a tray item link to the download. |
| **About** | | The version, the license, **Copy diagnostics** (a report for bug reports, copied to the clipboard) and **Open log folder**. |

## 17. Keyboard and mouse reference

| Action | How |
|---|---|
| Remove the selected route or virtual sink | **Delete** or **Backspace** |
| Mute or unmute the selected device | **M** |
| Clear the selection | **Esc** |
| Move a fader in Console | Click it, then **↑ / ↓** |
| Rename a virtual sink | Edit the name in the inspector, **Enter** to save, **Esc** to cancel |
| Connect | Drag from a source port to a destination port (Graph); click a send button (Console) or crosspoint (Matrix) |
| Move a card | Drag its header (Graph) |
| Maximize / restore | Double-click empty title bar space |
| Mute hotkey | Your shortcut from Settings › Mute, from anywhere |

## 18. Where PatchPro keeps its files

Everything is in `~/.config/PatchPro 2/`:

| File | Contents |
|---|---|
| `settings.json` | Your settings. |
| `scenes.json` | Your current setup and named scenes. |
| `project.json` | The layout: where cards are, which view is open. |
| `logs/patchpro.log` | The log (rotated at 1 MB, 3 old files kept). |

PatchPro may also create, depending on your settings and desktop:

- `~/.config/autostart/app.patchpro2.desktop`: only with "Start at login" on.
- `~/.local/share/applications/app.patchpro2.desktop`: a hidden entry that identifies the AppImage to your
  desktop, needed for the global hotkey. It's removed again automatically if you install the `.deb`.
- **KDE only:**
  - a window rule "PatchPro 2: remember window position" (System Settings › Window Rules), so the window comes
    back from the tray where you left it;
  - the shortcut "Mute or unmute (PatchPro 2)" in System Settings › Shortcuts.

## Desktop support

Routing, volume, meters and scenes work on any desktop with PipeWire and WirePlumber. Desktop integration varies:

| Feature | KDE Plasma 6 | GNOME | Others |
|---|---|---|---|
| Tray icon | Yes | Needs the "AppIndicator and KStatusNotifierItem Support" extension | Needs a StatusNotifierItem tray (most panels have one) |
| Close to tray / start minimized | Yes | With the extension; otherwise ✕ quits | With a tray |
| Global mute hotkey (Wayland) | Yes | Needs the GlobalShortcuts portal (recent GNOME, 48 or newer) | Needs a GlobalShortcuts portal |
| Global mute hotkey (X11) | Yes | Yes | Yes |
| Window returns to its last position | Yes (window rule) | Placed by the desktop | Placed by the desktop |

Tested on KDE Plasma 6 (Wayland) with PipeWire 1.6 and WirePlumber 0.5. Systems with WirePlumber 0.4 (e.g.
Ubuntu 24.04's default) have not been tested yet.

## 20. Troubleshooting

When something goes wrong, **Settings › About › Copy diagnostics** puts a report on your clipboard. It contains
versions, your settings and recent log lines; device and app names may appear in it. Paste it into a bug report.
The full log is under **Open log folder**.

### "Waiting for PipeWire: the sound server is not running."

PatchPro can't reach PipeWire. It keeps trying and connects as soon as PipeWire runs (at login PipeWire sometimes
starts a moment after PatchPro).

- Check: `systemctl --user status pipewire wireplumber`
- Start them: `systemctl --user enable --now pipewire pipewire-pulse wireplumber`
- If your system still uses PulseAudio, switch to PipeWire (your distribution's documentation explains how).
  PatchPro requires PipeWire.

### "PipeWire's client library (libpipewire 0.3) is not installed."

Install it, then click **Retry** in the banner:

| Distribution | Command |
|---|---|
| Ubuntu 24.04+ / Debian 13+ | `sudo apt install libpipewire-0.3-0t64` |
| Older Ubuntu / Debian | `sudo apt install libpipewire-0.3-0` |
| Fedora | `sudo dnf install pipewire-libs` |
| Arch | `sudo pacman -S libpipewire` |

### "This build of PatchPro needs a newer system (glibc …)."

Your distribution is older than the build supports. Use the official release build (made to run on Ubuntu 24.04 /
Debian 13 and newer), or update your system.

### "The audio engine is missing or cannot be run."

The installation is damaged. Reinstall PatchPro (download the AppImage again, or reinstall the `.deb`).

### The AppImage doesn't start

- Make it executable: `chmod +x PatchPro2-*.AppImage`.
- Install FUSE 2 (see [Installing](#appimage-any-distribution)), or run it without FUSE:
  `./PatchPro2-*.AppImage --appimage-extract-and-run`.
- Start it from a terminal to see error messages.

### An app is silent

- Look at the app in the Graph view: does it have a cable? An app without routes is silent. Drag a cable to where
  you want to hear it.
- Is the app, the device, or a virtual sink in between muted, or its fader all the way down?
- If the route goes through a virtual sink, does the sink have a cable to an output?
- Still nothing: quit PatchPro from the tray. Your system's normal routing comes back, which tells you whether the
  problem is in PatchPro's setup.

### Sound plays in two places / louder than expected

The app has two routes, for example straight to your headphones *and* through a virtual sink that also goes to
your headphones. Check its routes in the inspector and remove the one you don't want.

### A new app plays on the wrong device

New apps start on the default output. Change it with **Set as default output** in the inspector. Apps PatchPro has
seen before go where they were routed last time.

### The mute hotkey does nothing

- Settings › Mute should say **Active**. If it shows an error, read it: on Wayland your desktop needs the
  GlobalShortcuts portal ([Desktop support](#desktop-support)).
- **KDE:**
  - open System Settings › Shortcuts and find **PatchPro 2**;
  - the shortcut must be assigned, and no other action should use the same keys;
  - if you changed the key there, use the new key.
- If you moved the AppImage, start PatchPro once from its new place so your desktop learns where it is.
- Remember it toggles: one press mutes, the next unmutes.

### No tray icon

Your desktop has no system tray that supports StatusNotifierItem (GNOME needs the "AppIndicator and
KStatusNotifierItem Support" extension). Without a tray, the close button quits PatchPro and "Start minimized" is
ignored.

### The window doesn't come back where I left it

On Wayland, apps can't place their own windows; the desktop does. On KDE, PatchPro adds a window rule that
remembers the position (the first time, it may need one more close/open to learn it). On other desktops the
window is placed by the desktop's rules.

### "Meter: …" entries in my sound settings, or the microphone indicator is on

Those are PatchPro's level meters (they run while the window is visible). Turn on **Settings › Privacy › Hide meter
streams**, or hide the window to stop them.

### A video paused when PatchPro quit or crashed

PatchPro avoids this on a normal quit, a restart and when switching scenes. If PatchPro crashes, your sound system
moves apps back to the default device, and some media players (browsers) pause when their output disappears.
Press play again.

### The setup that comes back at startup isn't what I want

- Load a named scene, or change things and let the autosave pick them up.
- To start from the system's routing instead, turn off **Settings › Startup › Restore the last setup**.

### Start completely fresh

Quit PatchPro, then delete `~/.config/PatchPro 2/`. This removes your settings, scenes and layout. Your system's
audio settings are not affected.

### Reporting a bug

Open an issue on [GitHub](https://github.com/nelsonbernard/patchpro2-releases/issues). Describe what you did and what
happened, and paste **Copy diagnostics** from Settings › About.

## 21. Uninstalling

1. Turn off **Start at login** in Settings (if you turned it on), then quit PatchPro from the tray.
2. Remove the app:
   - **AppImage:** delete the file.
   - **.deb:** `sudo apt remove patchpro-2`.
3. Optionally remove what it left behind:
   - your settings, scenes and logs: `~/.config/PatchPro 2/`;
   - `~/.local/share/applications/app.patchpro2.desktop`;
   - `~/.config/autostart/app.patchpro2.desktop`;
   - **KDE:** the window rule "PatchPro 2: remember window position" (System Settings › Window Rules) and the
     shortcut under System Settings › Shortcuts › PatchPro 2.

Your system's own audio routing is not changed by uninstalling: once PatchPro isn't running, the system routes
audio as it normally does.
