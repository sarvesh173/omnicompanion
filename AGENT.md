# AGENT.md — OmniCompanion Agent Integration & Linking Guide

> **Target Audience:** Autonomous AI Agents (Claude Code, Hermes, AutoGPT, Codex, Cursor) and Headless Shell Scripts.  
> **Purpose:** Standard operating procedure for establishing connectivity, bootstrapping permissions, and controlling Android automation primitives via OmniCompanion.

---

## 1. Connectivity Topology

OmniCompanion runs an internal HTTP & WebSocket automation server on port `8788`.
Agents can communicate with the phone through any of the following transports:

### Transport A: USB ADB Reverse Forward (Preferred / Zero-Config)
```bash
adb forward tcp:8788 tcp:8788
# The device is now reachable on host loopback:
curl -s http://127.0.0.1:8788/api/status
```

### Transport B: Tailscale Mesh (Zero-IP / Carrier NAT Bypass)
When the device is connected to your Tailscale mesh:
```bash
curl -s http://<magicdns-hostname>:8788/api/status
```

### Transport C: Local Wi-Fi / LAN
```bash
curl -s http://<device-lan-ip>:8788/api/status
```

---

## 2. One-Shot Agent Bootstrap via Shell

When you have elevated ADB or Shizuku shell access, grant the complete permission envelope in one batch:

```bash
PKG="com.personal.omnicompanion"

# 1. Standard Runtime Permissions
pm grant $PKG android.permission.ACCESS_FINE_LOCATION
pm grant $PKG android.permission.ACCESS_COARSE_LOCATION
pm grant $PKG android.permission.ACCESS_BACKGROUND_LOCATION
pm grant $PKG android.permission.CAMERA
pm grant $PKG android.permission.RECORD_AUDIO
pm grant $PKG android.permission.BODY_SENSORS
pm grant $PKG android.permission.POST_NOTIFICATIONS
pm grant $PKG android.permission.READ_PHONE_STATE
pm grant $PKG android.permission.READ_SMS
pm grant $PKG android.permission.SEND_SMS
pm grant $PKG android.permission.READ_CONTACTS
pm grant $PKG android.permission.READ_MEDIA_IMAGES
pm grant $PKG android.permission.READ_MEDIA_VIDEO
pm grant $PKG android.permission.READ_MEDIA_AUDIO

# 2. Privileged AppOps
appops set $PKG MANAGE_EXTERNAL_STORAGE allow
appops set $PKG SYSTEM_ALERT_WINDOW allow
appops set $PKG GET_USAGE_STATS allow
appops set $PKG SCHEDULE_EXACT_ALARM allow

# 3. Vivo iManager Battery & Autostart Exemption
dumpsys deviceidle whitelist +$PKG
```

---

## 3. Automation API Endpoints (:8788)

### Inspect System State & Battery
```bash
curl -s http://127.0.0.1:8788/api/status
```
**Output Schema:**
```json
{
  "status": "online",
  "version": "5.4.0",
  "battery": { "level": 56, "temp_c": 41.1, "is_charging": false },
  "network": { "lan_ip": "192.168.31.132", "tailscale_status": "disconnected" },
  "permissions": { "runtime_granted": 28, "special_granted": 9 }
}
```

### Silent Native Screenshot (Zero Popups)
Captures screen via native API 30+ Accessibility without MediaProjection dialogs:
```bash
curl -s http://127.0.0.1:8788/api/screenshot -o screen.png
```

### Instant Text Typing (Companion Virtual Keyboard IME)
Types Unicode text without touching clipboard or sending raw keyevents:
```bash
curl -s -X POST http://127.0.0.1:8788/api/action/type \\
  -H "Content-Type: application/json" \\
  -d '{"text": "git commit -m \\"auto commit\\""}'
```

### Touch Gestures (Accessibility Service)
```bash
# Tap
curl -s -X POST http://127.0.0.1:8788/api/action/tap \\
  -H "Content-Type: application/json" \\
  -d '{"x": 540, "y": 1200}'

# Swipe
curl -s -X POST http://127.0.0.1:8788/api/action/swipe \\
  -H "Content-Type: application/json" \\
  -d '{"fromX": 540, "fromY": 1500, "toX": 540, "toY": 500, "durationMs": 300}'
```

### Hardware Flashlight Toggle
```bash
curl -s -X POST http://127.0.0.1:8788/api/action/torch \\
  -H "Content-Type: application/json" \\
  -d '{"state": "toggle"}'
```

### Elevated Shell Execution (Shizuku)
```bash
curl -s -X POST http://127.0.0.1:8788/api/shizuku/exec \\
  -H "Content-Type: application/json" \\
  -d '{"command": "pm list packages -3"}'
```

### Termux RUN_COMMAND Bridge
```bash
curl -s -X POST http://127.0.0.1:8788/api/termux/run \
  -H "Content-Type: application/json" \
  -d '{"command": "python app.py", "background": true}'
```

---

## 4. Autonomous Agent Execution Loop

When automating UI workflows, follow the closed-loop cycle:

```
[Capture Screenshot] ──> [LLM Vision / OCR] ──> [Compute Coordinates]
        ▲                                              │
        │                                              ▼
[Verify UI Change]  <── [Inject Text (IME)] <── [Execute Tap/Swipe]
```

1. **Observe:** Call `GET /api/screenshot` to fetch current viewport buffer.
2. **Decide:** Compute target `(x, y)` pixel coordinates from vision analysis.
3. **Act:** Issue `POST /api/action/tap` to activate UI target.
4. **Type:** If input focus is required, dispatch `POST /api/action/type` with payload.
5. **Confirm:** Re-fetch screenshot after 200 ms to confirm DOM transition.

