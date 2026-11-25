# Home Assistant einrichten

[English](home-assistant.en.md) | **Deutsch**

## Zugriffstoken

1. In Home Assistant unten links auf deinen Benutzernamen klicken → Reiter **Sicherheit**.
2. Ganz unten **Langlebige Zugriffstoken** → **Token erstellen**, Name z. B. „Cockpit Display“.
3. Token kopieren (er wird nur einmal angezeigt) und in die `secrets.yaml` eintragen, **mit** „Bearer “ davor:

```yaml
cockpit_ha_bearer_token: "Bearer eyJhbGciOi..."
```

Der Benutzer muss Administrator sein, weil das Display Werte über `/api/template` abfragt.

## Entitäten

Alle Einträge gehören in die Gerätedatei unter `substitutions`. Die Entitäts-IDs findest du in HA unter Entwicklerwerkzeuge → Zustände.

### Energie

| Eintrag | Art | Anzeige |
|---|---|---|
| `entity_battery_soc` | Sensor, % | Batterie-Ladezustand |
| `entity_booster` | Schalter* | Ladebooster ein/aus |
| `entity_dcdc_in_current` | Sensor, A | Eingangsstrom DC-DC |
| `entity_dcdc_in_power` | Sensor, W | Eingangsleistung DC-DC |
| `entity_dcdc_out_current` | Sensor, A | Ausgangsstrom DC-DC |
| `entity_dcdc_out_power` | Sensor, W | Ausgangsleistung DC-DC |
| `entity_dcdc_state_bulk` | Binärsensor | Ladephase Bulk |
| `entity_dcdc_state_absorption` | Binärsensor | Ladephase Absorption |
| `entity_dcdc_state_float` | Binärsensor | Ladephase Float |
| `entity_dcdc_state_fault` | Binärsensor | Fehler |
| `entity_dcdc_state_off` | Binärsensor | Aus |
| `entity_production_current` | Sensor | Gesamtproduktion Strom |
| `entity_production_power` | Sensor | Gesamtproduktion Leistung |
| `entity_outside_temp` | Sensor | Außentemperatur |
| `entity_inside_temp` | Sensor | Innentemperatur |

Die fünf Zustands-Binärsensoren entsprechen dem, was die Victron-Integration für Orion-DC-DC-Wandler liefert. Ein Wert ist „on“, die anderen „off“.

### Kamera

| Eintrag | Art | Anzeige |
|---|---|---|
| `entity_camera` | Kamera (`camera.…`) | Livebild |
| `entity_camera_power` | Schalter* | Kamera ein/aus |
| `entity_camera_rssi` | Sensor, dBm | WLAN-Signal der Kamera |
| `entity_floodlight` | Schalter* | Flutlicht ein/aus |

### Wasserwaage

| Eintrag | Art | Anzeige |
|---|---|---|
| `entity_level_pitch` | Sensor, Grad | Längsneigung, **positiv = vorn hoch** |
| `entity_level_roll` | Sensor, Grad | Querneigung, **positiv = rechts hoch** (in Fahrtrichtung) |
| `entity_level_battery` | Sensor, % | Batterie des Neigungssensors |
| `entity_level_voltage` | Sensor, V | Batteriespannung des Neigungssensors |
| `entity_level_live` | Schalter*, optional | Live-Modus, siehe unten |

### Live-Schalter (optional)

Manche Neigungssensoren senden zum Batteriesparen nur selten und haben einen Schalter für einen schnellen Live-Modus. Für solche Sensoren gibt es in der Gerätedatei:

```yaml
level_live_switch: "true"
entity_level_live: switch.wasserwaage_live_modus
```

Das Display schaltet den Live-Modus dann beim Öffnen der Wasserwaage ein und beim Verlassen wieder aus und zeigt oben „LIVE EIN“ oder „LIVE AUS“. Sendet dein Sensor ohnehin laufend, lass beide Zeilen weg: Dann wird nichts geschaltet und die Anzeige bleibt leer.

### Vorzeichen

Zeigt dein Sensor die Vorzeichen andersherum, stimmen Libelle und Keilplan nicht. Dann am besten in HA einen Template-Sensor mit umgedrehtem Vorzeichen anlegen und diesen eintragen.

Die Wasserwaagen-Werte kommen per Push über die native ESPHome-API. Dafür muss das Display in HA als ESPHome-Gerät hinzugefügt sein.

\* „Schalter“ heißt: alles, was sich mit `homeassistant.turn_on`/`turn_off` schalten lässt, also `switch`, `light`, `input_boolean` usw.
