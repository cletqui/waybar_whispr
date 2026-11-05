# waybar_whispr

Push-to-talk dictation for Wayland with [whisper.cpp](https://github.com/ggerganov/whisper.cpp). One toggle command: the first call records, the second transcribes, copies the text to the clipboard and types it into the focused window.

## Requirements

`whisper-cli`, `pw-record` (pipewire), `wpctl`, `wl-copy`, `wtype`, `notify-send`, and a ggml model at `~/.local/share/whisper/ggml-small.bin` (override with `DICTATE_MODEL`).

## Install

```bash
ln -s "$PWD/bin/dictate" ~/.local/bin/dictate
ln -s "$PWD/waybar" ~/.config/waybar/dictate
```

Bind `dictate` to a key in your compositor, for example niri:

```kdl
XF86AudioMedia repeat=false { spawn "/home/you/.local/bin/dictate"; }
```

## Waybar

The module is hidden while idle, shows a blinking red dot while recording and shows a spinner while transcribing. Clicking it while recording stops and transcribes, same as the key. Clicks while transcribing are ignored.

```jsonc
"include": ["~/.config/waybar/dictate/modules.json"],
"modules-left": ["custom/dictate"]
```

```css
@import "dictate/style.css";
```

The module is refreshed with `SIGRTMIN+12`; change `DICTATE_WAYBAR_SIGNAL` and the `signal` field in `modules.json` together if 12 is taken. The stylesheet expects the matugen colour names (`@error`, `@primary`, `@surface_container`).
