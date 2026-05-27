# Ad-Free YouTube on LG webOS TV using Homebrew

## 📌 Overview

This project documents the complete process of enabling Developer Mode on an LG webOS Smart TV, connecting it to a laptop using LG webOS CLI tools, installing Homebrew Channel, sideloading unofficial `.ipk` applications, and attempting to run an ad-free YouTube client on newer LG webOS firmware.

The project explores:

* LG webOS Developer Mode
* SSH-based TV authentication
* webOS CLI tooling
* App sideloading
* Homebrew ecosystem
* Firmware limitations on webOS 2024 TVs

---

# 🖥️ Device Information

| Component   | Details                     |
| ----------- | --------------------------- |
| TV Model    | LG 43UA82006LA              |
| OS          | webOS 2024                  |
| SDK Version | 10.3.0                      |
| Firmware    | 33.30.91                    |
| Laptop OS   | Windows                     |
| Tools Used  | Node.js, VS Code, webOS CLI |

---

# 🎯 Objective

Enable ad-free YouTube playback on an LG webOS TV using unofficial webOS applications and Homebrew tools.

---

# ⚠️ Important Limitation

LG Smart TVs run **webOS**, NOT Android TV.

Therefore:

* `.apk` Android apps ❌ DO NOT WORK
* `.ipk` webOS packages ✅ REQUIRED

---

# 🛠️ Technologies & Tools Used

* Node.js
* VS Code
* webOS Studio Extension
* `@webos-tools/cli`
* LG Developer Mode
* Homebrew Channel
* Luna Service Commands
* SSH Authentication

---

# 📶 Phase 1 — TV Preparation

## 1. Create LG Developer Account

Created a new India-region LG developer account.

Official portal:
[https://webostv.developer.lge.com/](https://webostv.developer.lge.com/)

---

## 2. Install Developer Mode App

On TV:

* Open LG Content Store
* Search:
  `Developer Mode`
* Install the app

---

## 3. Enable Developer Features

Inside Developer Mode app:

* Login using LG Developer account
* Enable:

  * Dev Mode Status
  * Key Server
* Restart TV

---

# 💻 Phase 2 — Laptop Setup

## 1. Install Node.js

Download:
[https://nodejs.org/](https://nodejs.org/)

Verify:

```bash
node -v
npm -v
```

---

## 2. Install webOS CLI Tools

Open Command Prompt as Administrator:

```bash
npm install -g @webos-tools/cli
```

---

## 3. Install webOS Studio Extension

Inside VS Code:

* Extensions
* Search:
  `webOS Studio`
* Install

---

# 🔗 Phase 3 — Connect TV to Laptop

## 1. Ensure Same Wi-Fi Network

Both TV and laptop must be on the same network.

---

## 2. Add TV Device

Run:

```bash
ares-setup-device
```

Enter:

* Device Name:
  `LGTV`
* IP Address:
  `192.168.1.12`
* Port:
  `9922`
* SSH User:
  `prisoner`

Set:

* Default Device → YES

---

## 3. Authenticate TV

Run:

```bash
ares-novacom --getkey -d LGTV
```

Enter TV passphrase shown inside Developer Mode app.

---

## 4. Verify Connection

```bash
ares-device -i -d LGTV
```

Successful output example:

```bash
modelName : 43UA73806LA
sdkVersion : 10.3.0
firmwareVersion : 33.30.91
```

---

# 📦 Phase 4 — Installing Homebrew Channel

## 1. Download Homebrew `.ipk`

Repository:
[https://github.com/webosbrew/webos-homebrew-channel/releases](https://github.com/webosbrew/webos-homebrew-channel/releases)

Download:

```text
org.webosbrew.hbchannel_0.7.3_all.ipk
```

---

## 2. Install Homebrew Channel

```bash
ares-install -d LGTV "FULL_PATH_TO_IPK"
```

Example:

```bash
ares-install -d LGTV "C:\Users\PBYAS\Desktop\projects\LG\org.webosbrew.hbchannel_0.7.3_all.ipk"
```

Expected output:

```bash
Success
```

---

# ▶️ Phase 5 — Installing YouTube AdFree

## 1. Remove Official YouTube App

VERY IMPORTANT:

* Uninstall stock YouTube from LG Content Store
* Remove from home screen
* Power off TV completely

This prevents webOS from redirecting to the official YouTube app.

---

## 2. Install YouTube AdFree

Using Homebrew Channel:

* Open Homebrew Channel
* Install:
  `youtube.leanback.v4`

OR manually install `.ipk`.

---

# 🧠 Phase 6 — App Registration (Critical Step)

Run:

```bash
ares-novacom --device LGTV --run "luna-send-pub -n 1 'luna://com.webos.service.eim/addDevice' '{\"appId\":\"youtube.leanback.v4\",\"pigImage\":\"\",\"mvpdIcon\":\"\"}'"
```

Expected output:

```json
{"returnValue":true}
```

This manually registers the sideloaded app inside webOS launcher services.

---

# 🔌 Phase 7 — Final Power Cycle

Completely unplug TV power for:

* 2 full minutes

Then:

* reconnect power
* turn TV ON

---

# 🚀 Launching the App

Possible methods:

## Method 1

Open:

* Homebrew Channel
* Launch YouTube AdFree

## Method 2

Use CLI:

```bash
ares-launch youtube.leanback.v4 -d LGTV
```

---

# ⚠️ Observed Firmware Limitation

On newer webOS firmware:

* sideloaded YouTube apps may redirect to stock YouTube
* launcher visibility may be hidden
* some ad-block functionality may be restricted

This appears to be caused by newer LG firmware security behavior.

---

# ✅ Achievements

Successfully:

* Enabled Developer Mode
* Connected TV via SSH
* Installed Homebrew Channel
* Sideloaded `.ipk` applications
* Registered apps manually
* Explored webOS internals and launcher behavior

---

# 📚 Key Commands Summary

## Add Device

```bash
ares-setup-device
```

## Authenticate TV

```bash
ares-novacom --getkey -d LGTV
```

## Device Info

```bash
ares-device -i -d LGTV
```

## Install App

```bash
ares-install -d LGTV "path_to_app.ipk"
```

## Launch App

```bash
ares-launch youtube.leanback.v4 -d LGTV
```

## Register App

```bash
ares-novacom --device LGTV --run "luna-send-pub -n 1 'luna://com.webos.service.eim/addDevice' '{\"appId\":\"youtube.leanback.v4\",\"pigImage\":\"\",\"mvpdIcon\":\"\"}'"
```

---

# 🔮 Future Improvements

* Investigate launcher persistence
* Explore deeper Homebrew repositories
* Reverse engineer webOS app registration
* Test older firmware versions
* Explore permanent rooting methods

---

# 📷 Screenshots to Include

Recommended screenshots:

* Developer Mode enabled
* CLI authentication
* Homebrew installation success
* Luna-send registration success
* VS Code webOS Studio connection
* Homebrew Channel UI

---

# 🏁 Conclusion

This project demonstrates advanced experimentation with LG webOS TVs including:

* embedded OS tooling
* sideloading workflows
* SSH communication
* unofficial package installation
* reverse engineering platform restrictions

It also highlights the increasing security restrictions present in modern smart TV ecosystems.
