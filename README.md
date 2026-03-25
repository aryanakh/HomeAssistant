# HomeAssistant

A personal collection of Home Assistant blueprints, scripts, and integrations.

---

## Contents

- [Blueprints](#blueprints)
  - [Arya's Ultimate Room Climate Control](#aryas-ultimate-room-climate-control)
  - [iOS Multi-Action Notifications](#ios-multi-action-notifications)

---

## Blueprints

Blueprints live in the [`blueprints/`](./blueprints) directory. To use them, copy the relevant `.yaml` file into your Home Assistant `config/blueprints/automation/` (or `config/blueprints/script/`) folder and reload blueprints.

---

### Arya's Ultimate Room Climate Control

**Files:**
- `blueprints/aryas_ultimate_room_climate_control.yaml` — Stable (v0.1.4)
- `blueprints/aryas_ultimate_room_climate_control_beta.yaml` — Beta (v0.1.5b)

**Minimum HA version:** 2023.1.0 (stable) · 2024.11.0 (beta)

An automation blueprint that drives your central thermostat based on a dedicated room temperature sensor rather than the thermostat's built-in sensor. Instead of a simple on/off threshold, it applies proportional control — the further the room is from your target range, the more aggressively it adjusts the thermostat setpoint.

#### How it works

1. Runs on a configurable polling interval (default: every 10 minutes).
2. Reads your chosen room sensor and compares it to your min/max comfort range.
3. **Outside range:** Calculates a corrective setpoint using `target − (multiplier × deviation)` and forces the appropriate heat or cool mode.
4. **Inside range:** Applies a wide comfort band (`min − buffer` to `max + buffer`) to prevent the system from cycling unnecessarily.

#### Inputs

| Input | Description | Default |
|---|---|---|
| Main Thermostat | The thermostat entity to control | — |
| Room Temperature Sensor | Sensor with `temperature` device class | — |
| Min Temperature | Lower bound of comfort range | 68 °F |
| Max Temperature | Upper bound of comfort range | 70 °F |
| Control Mode | `heat_cool`, `auto`, `heat`, or `cool` | `heat_cool` |
| Aggressiveness Multiplier | How forcefully to correct (0.5 – 3.0) | 1.0 |
| Neutral Range Buffer | Extra padding when room is in range (°F) | 1.0 |
| Poll Interval *(beta only)* | Minutes between automation runs (1 – 360) | 10 |
| Enable Toggle *(beta only)* | `input_boolean` to pause the automation | — |

#### Beta additions (v0.1.5b)

- Configurable poll interval instead of hardcoded 10 minutes.
- Enable/disable toggle via an `input_boolean` entity — disabling it will also turn the thermostat off cleanly.

---

### iOS Multi-Action Notifications

**File:** `blueprints/iOS_multi_action_notifications.yaml` — v0.4

**Type:** Script blueprint

A script blueprint for sending rich, actionable push notifications to iOS devices through the Home Assistant mobile app. Supports 2–4 custom action buttons, each with Apple SF Symbols icons and advanced iOS-specific options. After the notification is sent, the script waits for a button tap and runs the corresponding action sequence you define.

#### Features

- 2 required buttons + 2 optional buttons (up to 4 total)
- Per-button configuration:
  - Custom label text
  - SF Symbols icon name
  - Destructive styling (renders the button in red)
  - `authenticationRequired` — prompts Face ID / Touch ID / Passcode before firing
  - `foreground` — brings the Home Assistant app to the foreground on tap
- Critical alert mode — bypasses Silent and Do Not Disturb
  - Custom sound name
  - Volume control (0.0 – 1.0)
  - Requires iOS Critical Alerts permission granted to the HA app
- Send to a single device or override with a notify group service

#### Inputs

| Input | Description |
|---|---|
| Device | iOS device to notify |
| Notify Service Override | Optional group notify service (e.g. `notify.all_phones`) |
| Title / Message | Notification title and body text |
| Critical Alert | Enable critical mode, sound name, and volume |
| Action 1–4 | Text, icon, flags, and action sequence per button |

---

## Requirements

- [Home Assistant](https://www.home-assistant.io/) (see per-blueprint minimum versions above)
- iOS app: [Home Assistant for iOS](https://apps.apple.com/app/home-assistant/id1099568401) (for the notification blueprint)

---

## License

Personal use — no license applied. Feel free to adapt for your own setup.
