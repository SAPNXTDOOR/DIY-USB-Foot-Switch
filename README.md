# Device-Specific AHK Shortcut Manager

A lightweight, portable **AutoHotkey v2** application that transforms a single key on a **specific secondary keyboard, numpad, or foot pedal** into a powerful multi-action macro button.

Unlike standard AutoHotkey scripts that trigger globally across all keyboards, this app uses the **Windows Raw Input API** to listen to a **specific hardware device**. This allows you to use cheap secondary keyboards as dedicated macro pads **without installing kernel-level drivers** (such as AutoHotInterception) or disabling **Secure Boot**.

---

# ✨ Features

### 🔒 Device Isolation

Triggers macros **only when the target key is pressed on your designated hardware** (filtered via Vendor ID and Product ID).
The same key on your main keyboard continues to work normally.

### 🧠 Hardware State Machine

Reliably detects:

* **Single Press**
* **Double Press**
* **Long Hold**

All from **one physical key**.

### ⚡ Custom Configurable Actions

#### 🚀 Launch App

Open any executable, file, or document.

#### 🌐 Open URL

Launch websites instantly in your default browser.

#### ⌨️ Macro Recording

Record and replay complex keyboard combinations including:

* `Ctrl`
* `Shift`
* `Alt`
* `Win`

#### 🔊 Change Output Device

Instantly switch Windows audio output devices using NirSoft's **SoundVolumeView**.

### 🖥 Dynamic GUI

A built-in graphical interface accessible from the **System Tray** to:

* Edit shortcuts
* Configure press types
* Adjust timing delays
* Toggle admin/startup settings

---

# ⚠️ How it Works & Limitations

Because this script runs in **User Mode** (without custom drivers), it intercepts the keyboard signal **after Windows receives it**.

When the macro key is pressed:

1. Windows briefly types the character.
2. The script detects the **device hardware ID**.
3. It instantly sends **Backspace** to erase the character.
4. Your macro action is executed.

You may occasionally see a **very brief flicker** of the character if you are typing extremely fast. This is normal behavior for **driverless interception**.

---

# 🚀 Installation & Setup

To make the script listen **only to your custom foot switch** and run on **any PC without installing AutoHotkey**, follow these steps.

---

# Prerequisites

Install the following tools:

* **AutoHotkey v2**
  https://www.autohotkey.com/

* **Python**
  https://www.python.org/downloads/

---

# Step 1: Hardware Build (DIY Foot Switch)

You can build a dedicated macro switch using an old keyboard PCB and a cheap foot pedal switch (like [this one on Amazon](https://a.co/d/0exTAZ0x)).

1. Take the PCB from a **cheap membrane keyboard**.
2. Plug the bare PCB into your computer via USB.
3. Use a wire to short different pin pairs to see which key gets registered.
4. Once you find the key you want (example: `Numpad *`), solder two wires to those pins.
5. Connect the wires to your **external foot pedal switch**.


---

# Step 2: Configure the Target Key

Open **`final.ahk`** and locate this line near the top:

```ahk
global targetKey := "NumpadMult"
```

Replace `"NumpadMult"` with the name of the key your PCB produced when you shorted the pins.

You can find all AutoHotkey key names here: https://www.autohotkey.com/docs/v2/KeyList.htm

---

# Step 3: Set the Hardware ID (VID & PID)

To ensure the shortcut **only triggers on your foot switch**, you must identify the hardware device ID.

## 1️⃣ Run the Python Identifier

Download and run this script: https://github.com/SAPNXTDOOR/Keyboard-VID-and-PID-identifier/blob/main/keyboard_detector.py


## 2️⃣ Press the Foot Switch

The script will print a device string similar to:

```
\\?\HID#VID_04F3&PID_152E
```

## 3️⃣ Update the AHK Script

Open `final.ahk` and find:

```ahk
global targetDeviceID := "\\?\HID#VID_04F3&PID_152E"
```

Replace the value with the **exact string returned by the Python script**.

---

# Optional: Enable Audio Switching

To use the **Change Output Device** action:

1. Download **SoundVolumeView** from NirSoft
2. Create a folder named:

```
audio
```

3. Place this file inside it:

```
SoundVolumeView.exe
```

Directory structure example:

```
ShortcutManager
 ├── ShortcutManager.exe
 ├── config.txt
 ├── settings.ini
 └── audio
     └── SoundVolumeView.exe
```

---

# Step 4: Compile to Portable `.exe`

To run the app on **any PC without installing AutoHotkey**:

1. Open **Start Menu**
2. Launch **AutoHotkey Dash**
3. Click **Compile**

Then:

1. Select your file:

```
final.ahk
```

2. Choose the correct base file:

```
AutoHotkey64.exe
```

3. Click **Convert**

This will generate a standalone executable.

---

# Cleanup

Once compiled, you can delete:

```
*.ahk
*.py
```

Only these files are required to run the app:

```
ShortcutManager.exe
config.txt
settings.ini
audio/SoundVolumeView.exe
```


# 🖱 Usage

1. Run the compiled:

```
ShortcutManager.exe
```

2. Locate the **green "H" icon** in the Windows system tray.

3. Open the GUI:

* Double click the tray icon
  or
* Right click → **Edit Shortcut**

4. Choose your **Press Type**

```
Single
Double
Hold
```

5. Select the **Action Type**

Examples:

```
Launch App
Open URL
Macro
Change Output Device
```

6. Enter the action value and click **Save**.

---

# 📦 Example Config

```
NumpadMult|single|app|notepad.exe
NumpadMult|double|url|https://google.com
NumpadMult|hold|audio|Headphones
```

---

# 💡 Use Cases

* Foot pedal for **Push-to-Talk**
* Dedicated **streaming macro button**
* **Audio device switching**
* **Video editing shortcuts**
* **Accessibility tools**
* Cheap **DIY Stream Deck alternative**

---

# 🛠 Built With

* **AutoHotkey v2**
* **Python**
* **Windows Raw Input API**
* **SoundVolumeView (NirSoft)**

