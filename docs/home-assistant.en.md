# Setting up Home Assistant

**English** | [Deutsch](home-assistant.md)

## Access token

1. In Home Assistant, click your user name at the bottom left → **Security** tab.
2. At the very bottom, **Long-lived access tokens** → **Create token**, name it e.g. “Cockpit Display”.
3. Copy the token (it is only shown once) and add it to your `secrets.yaml`, **with** “Bearer ” in front of it:

```yaml
cockpit_ha_bearer_token: "Bearer eyJhbGciOi..."
```

The user must be an administrator, because the display reads values through `/api/template`.

## Entities

All entries go into the device file under `substitutions`. You can find the entity IDs in HA under Developer tools → States.

### Energy

| Entry | Kind | Shows |
|---|---|---|
| `entity_battery_soc` | Sensor, % | Battery state of charge |
| `entity_booster` | Switch* | Charge booster on/off |
| `entity_dcdc_in_current` | Sensor, A | DC-DC input current |
| `entity_dcdc_in_power` | Sensor, W | DC-DC input power |
| `entity_dcdc_out_current` | Sensor, A | DC-DC output current |
| `entity_dcdc_out_power` | Sensor, W | DC-DC output power |
| `entity_dcdc_state_bulk` | Binary sensor | Charge state bulk |
| `entity_dcdc_state_absorption` | Binary sensor | Charge state absorption |
| `entity_dcdc_state_float` | Binary sensor | Charge state float |
| `entity_dcdc_state_fault` | Binary sensor | Fault |
| `entity_dcdc_state_off` | Binary sensor | Off |
| `entity_production_current` | Sensor | Total production, current |
| `entity_production_power` | Sensor | Total production, power |
| `entity_outside_temp` | Sensor | Outside temperature |
| `entity_inside_temp` | Sensor | Inside temperature |

The five state binary sensors match what the Victron integration provides for Orion DC-DC chargers. One of them is “on”, the others are “off”.

### Camera

| Entry | Kind | Shows |
|---|---|---|
| `entity_camera` | Camera (`camera.…`) | Live image |
| `entity_camera_power` | Switch* | Camera on/off |
| `entity_camera_rssi` | Sensor, dBm | Camera Wi-Fi signal |
| `entity_floodlight` | Switch* | Floodlight on/off |

### Spirit level

| Entry | Kind | Shows |
|---|---|---|
| `entity_level_pitch` | Sensor, degrees | Pitch, **positive = front up** |
| `entity_level_roll` | Sensor, degrees | Roll, **positive = right side up** (in driving direction) |
| `entity_level_battery` | Sensor, % | Battery of the tilt sensor |
| `entity_level_voltage` | Sensor, V | Battery voltage of the tilt sensor |
| `entity_level_live` | Switch*, optional | Live mode, see below |

### Live switch (optional)

Some tilt sensors only report rarely to save battery and have a switch for a fast live mode. For those sensors, add to the device file:

```yaml
level_live_switch: "true"
entity_level_live: switch.your_live_mode_switch
```

The display then turns live mode on when you open the spirit level page and off when you leave it, and shows “LIVE EIN” (on) or “LIVE AUS” (off) at the top. If your sensor reports continuously anyway, leave both lines out: nothing is switched and that label stays empty.

### Signs

If your sensor reports the signs the other way round, the bubble level and the wedge plan will be wrong. The easiest fix is a template sensor in HA with the sign inverted, and entering that one instead.

The spirit level values are pushed through the native ESPHome API. For this, the display must be added to HA as an ESPHome device.

\* “Switch” means anything that can be switched with `homeassistant.turn_on`/`turn_off`, such as `switch`, `light`, `input_boolean` and so on.
