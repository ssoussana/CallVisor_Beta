# CallVisor_Beta

See who's calling and read your texts without leaving VR. CallVisor pairs a small Android app with a PC tray app to bring caller ID, call control, and text message previews into your headset while you sim race or fly — controlled entirely from your wheel, stick, or keyboard, with your phone left alone in your pocket.

## Features

- **Automatic Bluetooth connect** — turns your PC's Bluetooth on the moment your sim launches, off when it closes. No more staying paired all day just in case.
- **Caller ID in VR** — see who's calling (by name, if they're in your contacts) as an overlay in your headset via [OpenKneeboard](https://github.com/OpenKneeboard/OpenKneeboard).
- **Hands-free call control** — Answer, Ignore, Hang Up, and Mute, each bindable to a physical controller button *or* a keyboard combo (including modifier keys like Ctrl+Alt).
- **Mute that actually makes sense** — mutes only your outgoing call audio, not your sim/Discord microphone. The two are handled independently on purpose.
- **Text message previews** — see who texted, with the choice of sender-only or the full message, auto-dismissing after a timer you control (or instantly, via the Hang Up button).
- **Fully customizable overlay** — pick your own colors for ringing/active/muted/text states, and choose what shows when nothing's happening: a small dot, custom text, or nothing at all.
- **Live settings** — nearly every setting takes effect immediately. No restarting the app to see a color change.

## How it works

Your phone and PC talk over your local Wi-Fi network. The Android app watches for calls and texts and reports them to a small program running in your Windows system tray, which serves a simple status page that OpenKneeboard displays inside your headset. Button presses on your PC-side hardware are sent back to the phone, which acts on them (answering, hanging up, muting). Nothing goes through the internet — this is entirely local-network communication between two devices you own.

## Requirements

- **PC:** Windows 10 or 11, with Bluetooth, paired with your phone for call audio.
- **Phone:** Android 9 (Pie) or newer. Hang Up specifically requires Android 9+; everything else works on Android 8+.
- **[OpenKneeboard](https://github.com/OpenKneeboard/OpenKneeboard)** installed, with a VR-capable sim (iRacing, MSFS, or any program you add to the watch list).
- A game controller, wheel, or keyboard for button bindings (a controller is optional — keyboard combos work too).

## Installation

### 1. Link your phone with Phone Link, then pair for calls

CallVisor's Bluetooth automation only turns an existing pairing on and off — it doesn't create one, and Windows needs a one-time setup before it's even capable of carrying phone call audio at all.

1. Install **Phone Link** on your PC (Microsoft Store) if it isn't already there, and the **Link to Windows** companion app on your phone (built into Samsung phones; available on the Play Store for others).
2. Sign into the *same Microsoft account* on both, and complete Phone Link's own **Calls** setup, accepting the prompts on both sides. This one-time step is what enables Windows to act as a Hands-Free-Profile device for phone calls in the first place — without it, Windows won't offer call audio as a Bluetooth capability no matter how you pair.
3. With that done, pair your phone the normal way: **Settings → Bluetooth & devices → Add a device** on the PC — not through the Phone Link app's own window, which is a separate pairing flow that doesn't carry calls as reliably. Complete pairing on both devices (you may need to confirm a matching code on each).
4. Check your phone's Bluetooth quick panel — it should show **"Connected for calls"** to your PC.
5. Place a quick test call to your phone and confirm the audio plays through your PC/headset rather than the phone's own earpiece or speaker.

Phone Link itself doesn't need to stay open after this — once linked, the actual connection is handled by standard Windows Bluetooth, which is what CallVisor automates.

### 2. PC setup

1. Download `CallVisorSetup.exe` from the [Releases](../../releases) page.
2. Run it. It'll ask for administrator rights once — that's needed for a one-time network configuration step, handled automatically during install.
3. During setup you can optionally check **"Start CallVisor automatically when Windows starts"** and **"Create a desktop shortcut."** Both can also be toggled later from CallVisor's own Settings window.
4. When it finishes, look for the CallVisor icon in your system tray. It's **blue** when idle and turns **green** once your phone connects and starts watching for calls.

That's the whole PC install — no manual file downloads, no extra configuration steps.

### 3. Android setup

1. Download the `.apk` from the same [Releases](../../releases) page onto your phone, and install it (you may need to allow "install from unknown sources" for your browser or file manager the first time — this is normal for an app installed outside the Play Store).
2. Open the app and tap **Grant Permissions**. You'll be asked for:
   - **Phone state, Call log, Answer calls** — to detect and answer incoming calls.
   - **Contacts** — so callers show up by name instead of just a number.
   - **Receive SMS** — for text message previews (optional feature, can be turned off in PC Settings).
   - **Notifications** — required for the persistent "watching for calls" notification Android needs for a background service like this.
3. Find your PC's local IP address (Windows: `ipconfig`, look for IPv4 Address) and enter it in the app as `<ip>:8787`, e.g. `192.168.0.58:8787`. Tap **Save PC Address**.
4. Tap **Start Watching for Calls**. Your PC's tray icon should turn green within a few seconds, confirming the connection.

### 4. OpenKneeboard setup

1. In OpenKneeboard, add a new tab of type **Web Dashboard**.
2. Set its URL to:
   ```
   http://localhost:8787/call-status-page
   ```
3. The page has a transparent background by design — no extra configuration needed for that.
4. Position it in VR (Settings → VR → Position) wherever's comfortable for peripheral vision. A reasonable starting point: vertical 0.0m, left-right 0.25m, forward 0.5m, pitch 90° — adjust to taste.
5. **After any CallVisor software update** that changes the status page itself, remove and re-add this tab (or use OpenKneeboard's reload option) so it picks up the new version. Routine changes made in CallVisor's own Settings window — colors, idle text, etc. — apply live and do *not* need a tab reload.

## Configuring CallVisor

Right-click the tray icon → **Settings**. Four tabs:

- **Buttons** — bind Answer, Ignore, Hang Up, and Mute to a controller button or keyboard combo. Click Capture, then press your button (or hold a key combo and release it). Each binding must be unique — CallVisor blocks assigning the same button to two different actions.
- **Sim Programs** — the list of `.exe` names that trigger automatic Bluetooth on/off, plus the auto-start-with-Windows checkbox. Add any program via a file picker; comes pre-loaded with iRacing and MSFS 2024.
- **Display** — colors for the ringing, active-call, and muted states, plus what to show when idle: a small dot, custom text (e.g. "Ready"), or nothing at all.
- **Text Messages** — turn message previews on/off, choose sender-only vs. full message, and set the auto-dismiss timer. Hang Up also dismisses a visible text immediately, regardless of the timer.

## Troubleshooting

- **A button doesn't respond, but shows as bound in Settings:** Some controllers have buttons (certain paddles, rotary encoders) that don't report as standard digital buttons through DirectInput. Try binding a different button on the same device.
- **A bound button does something unexpected in OpenKneeboard itself** (like the whole overlay toggling on/off): Check OpenKneeboard's own Input bindings (Settings → Input) — it's possible the same physical button is also bound to an OpenKneeboard function like "toggle visibility," and both are responding to the same press.
- **Settings changes aren't showing up in VR:** Routine settings apply live. If you've just installed a CallVisor *update*, reload the OpenKneeboard tab once (see OpenKneeboard setup, step 5).
- **Hang Up doesn't work:** Requires Android 9 (Pie) or newer.
- **Reinstalling or moving CallVisor to a new PC:** Uninstalling removes the one-time network reservation automatically; a fresh install on a new machine handles that setup again on its own.

## Known limitations

- **iOS is not supported.** Apple provides no public API for third-party apps to answer or end cellular calls — only call *state* (ringing/active) could theoretically be observed, not controlled.
- **One phone per PC at a time.** CallVisor is built around a single sim rig with a single paired phone.
- Windows' own master volume also affects your phone call's audio (they share the same output device), so automatic volume-ducking during calls isn't offered — there's no clean way to lower your sim's volume without also lowering the call you're on.

## License

CallVisor is proprietary software — see [LICENSE](LICENSE) for full terms. In short: free to evaluate for a trial period, with a purchase required for continued use of most features afterward. This is not open-source software; repository access does not grant rights to copy, modify, or redistribute it.
# CallVisor_Beta
See who's calling and read your texts without leaving VR. CallVisor pairs a small Android app with a PC tray app to bring caller ID, call control, and text message previews into your headset while you sim race or fly — controlled entirely from your wheel, stick, or keyboard, with your phone left alone in your pocket.
