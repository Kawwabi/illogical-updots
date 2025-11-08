# GTK Python app that launches Zenity/Zenify

A minimal Python app using GTK (PyGObject) that shows a window with a single button. Clicking the button launches a simple Zenity/Zenify info dialog to verify everything works end-to-end.

This project primarily targets Linux desktops with GTK installed.

## What it does

- Creates a GTK window with a button labeled “Open Zenity”.
- When clicked, it runs the command `zenity --info --text="Hello from GTK!"`.
- You can override the command (e.g., use `zenify`) via an environment variable.

By default the app tries to run `zenity`. If your system has a binary called `zenify` instead, see the “Configuration” section below.

## Prerequisites

- Python 3.8+
- GTK 3 and PyGObject (gi)
- Zenity (or an alternative binary you want to run, e.g., zenify)

### Install system packages (recommended)

Ubuntu/Debian:

    sudo apt update
    sudo apt install -y python3 python3-venv python3-gi gir1.2-gtk-3.0 zenity

Fedora:

    sudo dnf install -y python3 gobject-introspection gtk3 zenity

Arch Linux:

    sudo pacman -S python gobject-introspection gtk3 zenity

Notes:
- Installing `PyGObject` via OS packages is the most reliable route on Linux.
- If you don’t want Zenity and intend to use a different binary (e.g., zenify), install that instead and configure the app as described below.

### Optional: create and activate a virtual environment

    python3 -m venv .venv
    . .venv/bin/activate

If you installed `python3-gi` via your package manager, you typically do not need to `pip install` anything. If you prefer pip, you can try:

    pip install --upgrade pip
    pip install PyGObject

(Be aware that PyGObject wheels are not always available for every platform; the system package approach is generally easier.)

## Files

- app.py — the GTK launcher script (you will create this)
- README.md — this file

A minimal `app.py` should:
- Import `gi` and require GTK 3
- Create a `Gtk.ApplicationWindow` with a `Gtk.Button`
- On click, run `zenity --info --text="Hello from GTK!"` (or your configured command) using `subprocess.run`
- Show an error dialog if the command fails or is not found

## Run

From the repository root:

    python3 app.py

You should see a window with a button labeled “Open Zenity”. Click it and you should get a Zenity info dialog with the test message.

## Configuration

You can override the command the app launches by setting the `ZEN_CMD` environment variable. This is useful if your binary is named `zenify`, or if you want to run a different tool.

Examples:

Use zenify instead of zenity:

    ZEN_CMD=zenify python3 app.py

Use zenity with a custom message (the app sets a default message, but you can adjust the command in code if desired):

    ZEN_CMD='zenity --info --title="Test" --text="Custom message from GTK"' python3 app.py

Tip: In the code, the default might be something like `ZEN_CMD=os.environ.get("ZEN_CMD","zenity --info --text=\"Hello from GTK!\"")`. Adjust as you like.

## Troubleshooting

- ModuleNotFoundError: No module named 'gi'
  - Install PyGObject via your OS packages:
    - Ubuntu/Debian: `sudo apt install python3-gi gir1.2-gtk-3.0`
    - Fedora: `sudo dnf install gobject-introspection gtk3`
    - Arch: `sudo pacman -S gobject-introspection gtk3`
  - If using a virtualenv, remember that `python3-gi` is a system package; keep using the system Python or ensure GI is visible to your venv.

- Command not found: zenity
  - Install zenity (e.g., `sudo apt install zenity`) or set `ZEN_CMD=zenify` if you have a different binary.

- Gtk-Message: Failed to load module or cannot open display
  - Ensure you’re running inside a graphical session with a valid DISPLAY (e.g., not over a headless SSH session without X forwarding).

- Nothing happens when clicking the button
  - Print logs to the console and verify the command:
    - Temporarily log `ZEN_CMD` and `subprocess.run` return codes.
    - Try running the command directly in your terminal to confirm it works.

## Example enhancements (optional)

- Add a text entry to customize the message before launching Zenity.
- Provide a dropdown to choose between zenity and zenify.
- Capture stdout/stderr from the launched process and show it in a GTK dialog.

## License

MIT (or your preferred license).
