# HDCVT HDMI Matrix - Home Assistant Custom Integration

Based on https://github.com/taysuus/hass-orei-matrix

The original repo works on other HDCVT rebranded HDMI Matrix such as Monoprice.

This fork is made to support other HDCVT OEM branded HDMI Matrix.

Control your **HDCVT HDMI Matrix** switch directly from **Home Assistant** via Telnet.

Supports power control, input/output switching, live state updates, and manual refresh.  
Compatible with multiple HDCVT models such as **UHD48-EX230-K**, etc.

---

## ✨ Features

- 🧠 **Automatic model detection** (`r type!`)
- 🔌 **Global power control** (on/off)
- 🎛 **Per-zone source selection** as media players
- 🔄 **Manual refresh service** (`hdcvt_matrix.refresh`)
- 🧩 **Dynamic device grouping** (all entities under one device)
- 🪄 **Config Flow setup** (no YAML required)
- 🧰 **Support for 4x4, 8x8, and other HDCVT matrix models**

---

## 🖼 Example UI

When configured, you’ll see a single device in Home Assistant:

> **HDCVT UHD48-EX230-K**
>
> - 🔌 `switch.hdcvt_matrix_power`
> - 🎚 `media_player.living_room`
> - 🎚 `media_player.bedroom`
> - 🎚 `media_player.office`
> - 🎚 `media_player.patio`

---

## ⚙️ Installation

### 🧩 HACS (Recommended)

1. Go to **HACS → Integrations → Custom Repositories**
2. Add this repository URL https://github.com/warheat1990/hass-hdcvt-matrix as type **Integration**
3. Search for **HDCVT HDMI Matrix** and install it.
4. Restart Home Assistant.

### 📦 Manual

1. Copy the `custom_components/hdcvt_matrix` folder into: <config>/custom_components/hdcvt_matrix/
2. Restart Home Assistant.

---

## 🧠 Configuration

Set up via the **Home Assistant UI**:

1. Go to **Settings → Devices & Services → Add Integration**
2. Search for **HDCVT HDMI Matrix**
3. Enter:

- **Host** (IP of your HDCVT Matrix)
- **Port** (default: 23)
- **Source Names** (e.g. `"Apple TV"`, `"Blu-ray"`, `"PC"`, `"Game Console"`)
- **Zone Names** (e.g. `"Living Room"`, `"Bedroom"`, `"Patio"`, `"Office"`)

That’s it — entities will be created automatically.

---

## 🧩 Entities

| Entity                     | Description                                          |
| -------------------------- | ---------------------------------------------------- |
| `switch.hdcvt_matrix_power` | Controls main matrix power                           |
| `media_player.<zone>`      | Represents each output zone (allows input selection) |

Each media player exposes:

- **Current source**
- **Source selection list** (using configured names)
- **Availability** (grayed out when matrix power is off)

---

## 🧰 Services

### `hdcvt_matrix.refresh`

Manually refreshes all matrix states immediately — power, model, and routing.

#### Example usage (Developer Tools → Services)

```yaml
service: hdcvt_matrix.refresh
```
