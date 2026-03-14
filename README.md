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
* **Fully Portable:** Can be compiled into a standalone `.exe` and moved between computers.

## ⚠️ How it Works & Limitations

Because this script operates in "User Mode" without custom drivers, it intercepts the hardware signal at the moment it reaches Windows. 

When you press the designated macro key (default: `Numpad *`), Windows will briefly type the character. The script instantly detects the hardware ID, uses `SendInput` to fire a `{BackSpace}` to erase the character, and then triggers your macro. You may occasionally see a split-second flicker of the character if you are typing rapidly, which is normal for driverless user-mode interception.

## 🚀 Installation & Setup

### 1. Prerequisites
* **AutoHotkey v2** (if running the raw `.ahk` script).
* If you are using the compiled `.exe` version, no installation is required! Just run it on any Windows PC.

### 2. Configure Your Hardware ID (Crucial Step)
To make the script listen only to your specific device, you must find its Hardware ID (`VID` and `PID`) and paste it into the code.

1. Find your device's Hardware ID. You can find this in Windows Device Manager (under Human Interface Devices -> Properties -> Details -> Hardware Ids), or by using an AHK/Python Raw Input scanner.
2. Open `final.ahk` in a text editor.
3. Locate this line near the top of the script:
   ```autohotkey
   global targetDeviceID := "\\?\HID#VID_04F3&PID_152E"
