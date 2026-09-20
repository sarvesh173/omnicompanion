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

## 🥊 Architectural Comparison Matrix

Most Android automation approaches force an impractical tradeoff: either they require a computer physically tethered via ADB, or they rely on rigid visual scripting tools with no REST/JSON-RPC server for autonomous LLM agents.

| Architecture / Metric | **OmniCompanion** ⚡ | **`android-remote-control-mcp`** | **`scrcpy-mcp` / `adb-mcp`** | **Appium / UIAutomator2** | **Tasker + AutoInput** |
|:---|:---:|:---:|:---:|:---:|:---:|
| **Deployment Model** | **Standalone On-Device** | Standalone On-Device | Host-Dependent (Requires PC) | Host-Dependent (Heavy PC Server) | On-Device Human Tool |
| **Action Latency** | **<30 ms** (Direct Ktor IPC) | 10–100 ms | 1–4 s (ADB roundtrip) | 500 ms–2 s | 200–800 ms |
| **Virtual IME Text Input** | **Yes** (Native IME, zero mangling) | ❌ Accessibility Replace only | ⚠️ ADB input (mangles Unicode) | ⚠️ Custom test IME | ⚠️ Emulated Keystrokes |
| **Silent Screen Capture** | **Yes** (API 30+ A11y, 0 popups) | ⚠️ MediaProjection popup | ⚠️ Requires desktop frame grab | ⚠️ Screen capture session | ⚠️ Root or popup required |
| **Privileged Shell Access** | **Yes** (Shizuku Binder UID 2000) | ❌ Standard sandbox only | ⚠️ Host ADB shell only | ⚠️ Limited adb shell | ⚠️ Root only |
| **Termux `RUN_COMMAND`** | **Yes** (Local CLI/Python runner) | ❌ No | ❌ No | ❌ No | ⚠️ Plugin setup |
| **CGNAT Reverse Relay** | **Yes** (Outbound WSS / Tailscale) | ⚠️ Cloudflare/ngrok tunnels | ❌ Localhost only | ❌ Local network only | ❌ No |
| **Open Source & License** | **Yes (MIT)** | Yes (MIT) | Yes (MIT) | Yes (Apache 2.0) | ❌ Closed Source / Paid |

### Key Architectural Advantages:
1. **Zero-Tether Autonomous Operation:** Unlike `scrcpy-mcp` or `adb-mcp`, OmniCompanion does not require a desktop machine running `adb server` next to the phone. The phone operates as an autonomous agent node.
2. **Dedicated Virtual IME:** Ordinary accessibility tools use `ACTION_SET_TEXT` which fails on modern web views, terminal emulators, and custom text engines. OmniCompanion's `CompanionInputMethodService` commits arbitrary Unicode and terminal control sequences instantly.
3. **Triple-Layer Privilege Model:** Combines user-level accessibility gestures, Shizuku ADB shell execution (UID 2000), and Termux environment execution into a unified JSON-RPC plane.

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
