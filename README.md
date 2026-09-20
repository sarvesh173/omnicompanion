<p align="center">
  <h1 align="center">OmniCompanion</h1>
  <p align="center">
    <strong>Universal Android Automation Companion Bridge & Virtual IME Service for Autonomous AI Agents and Developers.</strong><br>
    Material 3 Expressive • Shizuku Privileged Shell • Accessibility Gestures • Termux RUN_COMMAND Bridge • Silent Screenshots
  </p>
  <p align="center">
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge" alt="License"></a>
    <img src="https://img.shields.io/badge/Target_SDK-35_(Android_14/15)-blue?style=for-the-badge&logo=android" alt="Target SDK">
    <img src="https://img.shields.io/badge/Architecture-Kotlin_%2B_Compose-4285F4?style=for-the-badge&logo=kotlin" alt="Kotlin">
    <a href="AGENT.md"><img src="https://img.shields.io/badge/Agent_Ready-JSON_RPC_&_REST-orange?style=for-the-badge&logo=probot" alt="Agent Ready"></a>
  </p>
</p>

---

## 🎯 What is OmniCompanion?

**OmniCompanion** is a high-performance, background Android service application designed to bridge physical mobile devices with autonomous AI agents, developer desktop scripts, and remote terminals.

Rather than relying on brittle ADB keyevents or heavyweight cloud emulators, OmniCompanion runs directly on the device as a persistent automation hub:

- **Companion Virtual Keyboard (IME):** Injects high-speed, arbitrary Unicode text directly into focused Android input fields without clipboard pollution.
- **Silent Screenshot Engine:** Captures full-resolution frames natively via Android 11+ AccessibilityService (`takeScreenshot`) with zero user permission popups.
- **Gesture Injection Engine:** Performs sub-millisecond precision taps, continuous swipes, drag-and-drop, and hardware navigation key events.
- **Privileged Shell Integration:** Integrates with **Shizuku** (Binder UID 2000) and **Termux** (`RUN_COMMAND`) for privileged system administration and offline CLI execution.
- **Local HTTP & WebSocket Control Plane (:8788):** Standard REST and JSON-RPC 2.0 endpoints reachable over USB forward, LAN, or Tailscale MagicDNS.
- **Official Material 3 Expressive UI:** Built strictly adhering to [Google Material 3](https://m3.material.io/) with deep matte surfaces, ergonomic bottom thumb controls, and live device telemetry.

---

## 🚀 Quick Agent Link

Connect over ADB reverse tunnel in seconds:

```bash
# Forward port 8788 from phone to host
adb forward tcp:8788 tcp:8788

# Check device telemetry
curl -s http://127.0.0.1:8788/api/status
```

For complete machine instructions, JSON schemas, and bootstrap commands, read [**AGENT.md**](AGENT.md).

---

## 🛡️ Permission Envelope

OmniCompanion manages a comprehensive 40+ permission envelope required for complete automation:
- **Foreground & Background:** Location, Camera, Audio, Health Sensors, Telephony, SMS, Contacts.
- **Special App Access:** All Files Access (`MANAGE_EXTERNAL_STORAGE`), Battery Optimization Exemption, Display Over Other Apps.
- **System Automation:** Accessibility Service, Companion Virtual Keyboard (IME), Notification Listener, Device Administrator.
- **Privileged Bridges:** Shizuku Binder, Termux RUN_COMMAND.

---

## 📄 License

MIT License — Copyright (c) 2026 Sarvesh. See [LICENSE](LICENSE) for details.

---

<p align="center">
  <strong>Engineered for zero-friction human & machine coexistence.</strong>
</p>
