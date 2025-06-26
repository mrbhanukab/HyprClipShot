# Hyprshot

Hyprshot is an utility to easily take screenshot in Hyprland using your mouse.

It allows taking screenshots of windows, regions and monitors which are saved to a folder of your choosing and copied to your clipboard.

## Installation
### Dependencies

- hyprland (this one should be obvious)
- jq (to parse and manipulate json)
- grim (to take the screenshot)
- slurp (to select what to screenshot)
- wl-clipboard (to copy screenshot to clipboard)
- libnotify (to get notified when a screenshot is saved)

#### Optional Dependencies

- hyprpicker (to freeze the screen contents with the `--freeze` flag)

First create 
```bash
mkdir -p "$HOME/.local/bin"
```

curl
```bash
curl -o "$HOME/.local/bin/hyprclipshot" "https://raw.githubusercontent.com/mrbhanukab/HyprClipShot/refs/heads/main/hyprclipshot"
chmod +x "$HOME/.local/bin/hyprclipshot"
```

wget
```
wget -O "$HOME/.local/bin/hyprclipshot" "https://raw.githubusercontent.com/mrbhanukab/HyprClipShot/refs/heads/main/hyprclipshot"
chmod +x "$HOME/.local/bin/hyprclipshot"
```

## Usage

You can get help on how to use hyprshot by executing:

```bash
$ hyprshot -h
```

The simplest usage of Hyprshot is executing it with one of the available modes.

For example, to screenshot an open window:

```bash
$ hyprshot -m window
```

You can also skip saving the screenshot to a file, copying it only to the clipboard:

```bash
$ hyprshot -m output --clipboard-only
```

## Configuration

You can add the various modes as keybindings in your Hyprland config like so:

```
# ~/.config/hypr/hyprland.conf

...

# Screenshot a window
bind = $mainMod, PRINT, exec, hyprshot -m window
# Screenshot a monitor
bind = , PRINT, exec, hyprshot -m output
# Screenshot a region
bind = $shiftMod, PRINT, exec, hyprshot -m region
```

This would allow you to:

Take a screenshot of a window by using `MOD + PrintScr`

Take a screenshot of a monitor by using `PrintScr`

Take a screenshot of a region by using `MOD + Shift + PrintScr`

## Save location

You can choose which directory Hyprshot will save screenshots in by setting an `HYPRSHOT_DIR` environment variable to your preferred location.

If `HYPRSHOT_DIR` is not set, Hyprshot will attempt to save to `XDG_PICTURES_DIR` and will further fallback to your home directory if this is also not available.
