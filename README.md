# Device-Specific AHK Shortcut Manager

A lightweight, portable AutoHotkey v2 application that transforms a single key on a **specific** secondary keyboard, numpad, or foot pedal into a powerful multi-action macro button. 

Unlike standard AutoHotkey scripts that trigger globally across all keyboards, this app uses the Windows Raw Input API to listen to a specific hardware device. It allows you to use cheap secondary keyboards as dedicated macro pads *without* needing to install kernel-level drivers (like AutoHotInterception) or disable Secure Boot.

## ✨ Features

* **Device Isolation:** Triggers macros *only* when the target key is pressed on your designated hardware (filtered via Vendor ID and Product ID). The same key on your main keyboard acts normally.
* **Hardware State Machine:** Reliably distinguishes between **Single Clicks**, **Double Clicks**, and **Long Holds** on a single physical key.
* **Custom Configurable Actions:**
  * 🚀 **Launch App:** Open any `.exe`, document, or file.
  * 🌐 **Open URL:** Launch a website in your default browser.
  * ⌨️ **Macro Recording:** Record and playback complex keystroke combinations (with modifiers like Ctrl/Shift/Alt).
  * 🔊 **Change Output Device:** Instantly switch Windows audio outputs (requires `SoundVolumeView.exe`).
* **Dynamic GUI:** A built-in graphical interface accessible from the System Tray to edit shortcuts, change timing delays, and manage admin/startup settings.

## ⚠️ How it Works & Limitations

Because this script operates in "User Mode" without custom drivers, it intercepts the hardware signal at the moment it reaches Windows. 

When you press the designated macro key (default: `Numpad *`), Windows will briefly type the character. The script instantly detects the hardware ID, uses `SendInput` to fire a `{BackSpace}` to erase the character, and then triggers your macro. You may occasionally see a split-second flicker of the character if you are typing rapidly, which is normal for driverless user-mode interception.

## 🚀 Installation & Setup

To make the script listen *only* to your custom foot switch and run without needing AHK installed on every PC, follow these hardware and software steps.

### Prerequisites
* [AutoHotkey v2](https://www.autohotkey.com/) installed on your PC.
* [Python](https://www.python.org/downloads/) installed on your PC.

### Step 1: Hardware Build (DIY Foot Switch)
You can build a dedicated macro switch by recycling an old keyboard and wiring it to an external foot pedal (like [this one on Amazon](https://a.co/d/0exTAZ0x)).
1. Take the printed circuit board (PCB) from an old, cheap membrane keyboard.
2. Plug the bare PCB into your computer via USB. Use a wire to manually short any two pins on the board to see which key it registers on your computer.
3. Solder two wires to those specific pins and connect them to your external foot switch.

### Step 2: Configure the Target Key
1. Open `final.ahk` in a text editor.
2. Find the following line near the top:
   ```autohotkey
   global targetKey := "NumpadMult"
