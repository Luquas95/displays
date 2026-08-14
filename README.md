# displays

Terminal UI for managing external monitor layout in Hyprland — extend/mirror, side, scale, on/off, all with live preview and a keypress.

[![Platform](https://img.shields.io/badge/platform-Hyprland%20%2F%20Omarchy-blue?logo=linux&logoColor=white)](https://hyprland.org/)
[![Language](https://img.shields.io/badge/language-Bash-green)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/badge/license-MIT-yellow)](https://opensource.org/license/MIT)
[![Stars](https://img.shields.io/github/stars/Luquas95/displays?style=social)](https://github.com/Luquas95/displays)

## What it does

- **Extend / Mirror** — toggle between one big desktop and a mirrored image
- **Placement** — put the external monitor left, right, above or below the laptop panel
- **Scale** — cycle 1 / 1.25 / 1.5 / 2, auto-snapped to the nearest value Hyprland will actually accept for that resolution
- **Outputs** — both screens, laptop only, or external only
- **Multiple externals** — cycle between them with `e` if more than one is connected
- **Persistence** — the last layout you pick survives a Hyprland restart or the laptop being unplugged and replugged, instead of resetting to the default `monitors.conf` rule every time

Nothing is applied until you press a key — starting the program just shows the current state.

## Requirements

- Hyprland
- `hyprctl`
- `jq`

## Installation

```bash
cp displays ~/.local/bin/
chmod +x ~/.local/bin/displays
```

(assumes `~/.local/bin` is on `$PATH`, the default on Omarchy)

## Usage

```bash
displays
```

| Key | Action |
|-----|--------|
| `Tab` | Switch mode — Mirror ↔ Extend |
| `←` `→` `↑` `↓` | In Extend: side of the external monitor relative to the laptop |
| `s` | Cycle external monitor scale (1 / 1.25 / 1.5 / 2) |
| `o` | Which screens are on — both / laptop only / external only |
| `e` | Switch to the next external monitor (when more than one is connected) |
| `r` | Reset to the defaults in `~/.config/hypr/monitors.conf` (forgets the saved layout below) |
| `q` | Quit |

## How persistence works

Every applied change is written to `~/.local/state/omarchy/displays/monitors.conf`, which `hyprland.conf` sources right after `~/.config/hypr/monitors.conf`. That means your last choice always wins on the next Hyprland start — without it, reconnecting a monitor falls back to the static default rule (typically "place it to the right"), no matter what you had set before.

## Why not just edit monitors.conf by hand?

You can, but you're guessing coordinates and scale values that Hyprland will silently adjust if they don't divide evenly into whole pixels. `displays` reads the real state back from `hyprctl`, computes positions itself so the layout is always anchored with no gaps, and snaps scale to a value Hyprland will actually keep — all with instant visual feedback instead of a reload-and-check loop.
