# Changelog

## v1.0.0 – erste öffentliche Version

- Vier Seiten: Energie, Grauwasser, Wasserwaage mit Keilplan, Kamera-Livebild
- Konfiguration als ESPHome-Paket: alle Entitäten und Zugangsdaten in der eigenen Gerätedatei
- Wasserwaage per Push über die native API (kein Polling), Libelle mit einstellbaren Toleranzringen
- Keilplan: Keilhöhe je Rad aus Spurbreite und Radstand
- Nicht verfügbare Werte werden als „--“ angezeigt
- OTA-Updates verschlüsselt mit dem API-Schlüssel
- Schaltbefehle domänenunabhängig (switch, light, input_boolean …)
