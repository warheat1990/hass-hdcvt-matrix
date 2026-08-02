Based on https://github.com/Dresdn/hass-orei_matrix (fork of https://github.com/taysuus/hass-orei_matrix)

The original repo works on other HDCVT rebranded HDMI Matrix such as Monoprice.

This fork is made to support other HDCVT OEM branded HDMI Matrix but it will be focused on HDP-MXB44P as that is what I have.

# Orei HDMI Matrix - Home Assistant Custom Integration

Control your **Orei HDMI Matrix** switch directly from **Home Assistant** via Telnet.

Supports power control, input/output switching, live state updates, and manual refresh.
Compatible with multiple Orei models such as **UHD48-EX230-K**, etc.

---

## ✨ Features

- 🧠 **Automatic model detection** (`r type!`)
- 🔌 **Global power control** (on/off)
- 🎛 **Per-output source selection** as media players
- ⚡ **Comprehensive services** for power, routing, and CEC control
- 🔄 **Manual refresh service** (`orei_matrix.refresh`)
- 🧩 **Dynamic device grouping** (all entities under one device)
- 🪄 **Config Flow setup** (no YAML required)
- 🔍 **Auto-discovery** of inputs and outputs
- 🧰 **Support for 4x4, 8x8, and other Orei matrix models**

---

## 🖼 Example UI

When configured, you’ll see a single device in Home Assistant:

> **Orei UHD48-EX230-K**
>
> - 🔌 `switch.orei_matrix_power`
> - 🎚 `media_player.living_room`
> - 🎚 `media_player.bedroom`
> - 🎚 `media_player.office`
> - 🎚 `media_player.patio`

---

## ⚙️ Installation

### 🧩 HACS (Recommended)

1. Go to **HACS → Integrations → Custom Repositories**
2. Add this repository URL https://github.com/taysuus/hass-orei-matrix as type **Integration**
3. Search for **Orei HDMI Matrix** and install it.
4. Restart Home Assistant.

### 📦 Manual

1. Copy the `custom_components/orei_matrix` folder into: <config>/custom_components/orei_matrix/
2. Restart Home Assistant.

---

## 🧠 Configuration

Set up via the **Home Assistant UI**:

1. Go to **Settings → Devices & Services → Add Integration**
2. Search for **Orei HDMI Matrix**
3. Enter:

- **Host** (IP of your Orei Matrix)
- **Port** (default: 23)
- **Source Names** (e.g. `"Apple TV"`, `"Blu-ray"`, `"PC"`, `"Game Console"`)
- **Zone Names** (e.g. `"Living Room"`, `"Bedroom"`, `"Patio"`, `"Office"`)

That’s it — entities will be created automatically.

---

## 🧩 Entities

| Entity Type                | Count | Description                                          |
| -------------------------- | ----- | ---------------------------------------------------- |
| `switch.orei_matrix_power` | 1     | Controls main matrix power                           |
| `switch.<input_name>`      | 8     | CEC control for each input device                    |
| `media_player.<output>`    | 8     | Represents each output (allows source selection)     |

**Total: 17 entities** for an 8x8 matrix (vs 33 with old button-based approach)

Each media player exposes:

- **Current source**
- **Source selection list** (using configured names)
- **Availability** (grayed out when matrix power is off)

Each input switch shows:
- **Routed outputs** (which outputs display this input)
- **CEC control** (power on/off source devices)

---

## 🧰 Services

### Power Control

- `orei_matrix.power_on_output` - Power on a TV/display and set as active source
- `orei_matrix.power_off_output` - Power off a TV/display
- `orei_matrix.set_output_active` - Tell TV to switch to matrix input
- `orei_matrix.power_on_input` - Power on a source device (Apple TV, Xbox, etc)
- `orei_matrix.power_off_input` - Power off a source device
- `orei_matrix.power_on_all_outputs` - Power on all displays
- `orei_matrix.power_off_all_outputs` - Power off all displays

### Routing Control

- `orei_matrix.route_input_to_output` - Route specific input to specific output
- `orei_matrix.route_input_to_outputs` - Route input to multiple outputs

### System

- `orei_matrix.refresh` - Manually refresh matrix state

### Example: Movie Night Automation

```yaml
automation:
  - alias: "Movie Night"
    trigger:
      - platform: time
        at: "20:00:00"
    action:
      # Power on living room TV
      - service: orei_matrix.power_on_output
        data:
          output: 1

      # Wait for TV to boot
      - delay:
          seconds: 5

      # Switch to Apple TV
      - service: media_player.select_source
        target:
          entity_id: media_player.av_matrix_living_room
        data:
          source: "Apple TV"
```

See [SERVICE_EXAMPLES.md](SERVICE_EXAMPLES.md) for more automation examples.