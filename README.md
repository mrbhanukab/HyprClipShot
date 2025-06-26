# HyprClipShot

HyprClipShot is a modified fork of Hyprshot, an utility to easily take screenshots in Hyprland using your mouse.

It allows taking screenshots of windows, regions, and monitors, which are **copied to your clipboard** and also offers an option to be **saved to a folder via notification**. By default, it will use screen freezing during selection and utilize a default folder for saving with auto-generated filenames. This also displays the screenshot in a notification.

This simplified version focuses on:

1.  Taking a screenshot and copying it to the clipboard.
2.  Displaying a notification with a "Save to Disk" action.
3.  If the user clicks "Save to Disk", the screenshot is saved to a default folder with an auto-generated filename, and a second notification confirms the save.

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

First create your local bin directory if it doesn't exist:

```bash
mkdir -p "$HOME/.local/bin"
```

Download and make `hyprclipshot` executable using `curl`:

```bash
curl -o "$HOME/.local/bin/hyprclipshot" "https://raw.githubusercontent.com/mrbhanukab/HyprClipShot/refs/heads/main/hyprclipshot"
chmod +x "$HOME/.local/bin/hyprclipshot"
```

Alternatively, use `wget`:

```bash
wget -O "$HOME/.local/bin/hyprclipshot" "https://raw.githubusercontent.com/mrbhanukab/HyprClipShot/refs/heads/main/hyprclipshot"
chmod +x "$HOME/.local/bin/hyprclipshot"
```

## Usage

You can get help on how to use `hyprclipshot` by executing:

```bash
$ hyprclipshot -h
```

The simplest usage of HyprClipShot is executing it with one of the available modes. The screenshot will be copied to your clipboard, and a notification will appear offering to save it to disk.

For example, to screenshot an open window:

```bash
$ hyprclipshot -m window
```

To capture the currently active window:

```bash
$ hyprclipshot -m window -m active
```

To capture a specific monitor, e.g., `DP-1`:

```bash
$ hyprclipshot -m output -m DP-1
```

You can also specify a command to open the screenshot with after it's taken (this will apply to the temporary file before it's saved via notification):

```bash
$ hyprclipshot -m window -- mirage
```

### Options:

  - `-h, --help`: Show the help message.
  - `-m, --mode`: Specify one or more modes for screenshot capture. Valid modes include: `output`, `window`, `region`, `active`, or a specific `OUTPUT_NAME` (e.g., `DP-1`).
  - `-D, --delay`: How long to delay taking the screenshot after selection (in seconds).
  - `-d, --debug`: Print debug information to the console.
  - `-t, --notif-timeout`: Set the notification timeout in milliseconds (default is 5000ms).
  - `-- [command]`: Open the screenshot with a command of your choosing.

### Modes:

  - `output`: Take a screenshot of an entire monitor.
  - `window`: Take a screenshot of an open window (you will be prompted to select one with your mouse).
  - `region`: Take a screenshot of a selected rectangular region (you will be prompted to select one with your mouse).
  - `active`: Use in conjunction with `window` or `output` mode (e.g., `-m window -m active`) to take a screenshot of the currently focused window or active monitor without manual selection.
  - `OUTPUT_NAME`: Take a screenshot of the monitor with the specified `OUTPUT_NAME` (e.g., `DP-1`). You can get monitor names from `hyprctl monitors`.

## Configuration

You can add the various modes as keybindings in your Hyprland config like so:

```
# ~/.config/hypr/hyprland.conf

...

# Screenshot a window (and copy to clipboard, prompt to save)
bind = $mainMod, PRINT, exec, hyprclipshot -m window
# Screenshot a monitor (and copy to clipboard, prompt to save)
bind = , PRINT, exec, hyprclipshot -m output
# Screenshot a region (and copy to clipboard, prompt to save)
bind = $shiftMod, PRINT, exec, hyprclipshot -m region
# Screenshot active window (and copy to clipboard, prompt to save)
bind = $mainMod SHIFT, PRINT, exec, hyprclipshot -m window -m active
```

This would allow you to:

  * Take a screenshot of a window by using `MOD + PrintScr`.
  * Take a screenshot of a monitor by using `PrintScr`.
  * Take a screenshot of a region by using `MOD + Shift + PrintScr`.
  * Take a screenshot of the active window by using `MOD + Shift + PrintScr`.

## Save location

When you click "Save to Disk" on the notification, HyprClipShot will save the screenshot to an auto-generated filename in a default location.

To change this default save directory, you need to open the `hyprclipshot` script file (e.g., located at `~/.local/bin/hyprclipshot`) with a text editor. Find the line that sets the `SAVEDIR` variable and modify it to your desired path.

For example, to save screenshots directly to your `~/Downloads` folder, you would change:

```bash
SAVEDIR="${HOME}/Pictures/screenshots"
```

to:

```bash
SAVEDIR="${HOME}/Downloads"
```
