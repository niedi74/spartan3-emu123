# spartan3-emu123

123ignition-**Emulator** für den [Spartan 3 v2 Motorraum-Hub](https://github.com/niedi74/spartan3v2-can-adapter) — spielt einer echten 123TUNE+-Instanz ein 123ignition auf einem separaten ESP32-S3 als BLE-Peripheral nach, damit der Hub am Schreibtisch getestet werden kann, ohne dass ein echtes 123ignition am Bus hängt.

## Zweck

Der Hub verbindet sich normalerweise als BLE-**Client** mit einem echten 123ignition (Nordic UART Service, NUS). Dieser Emulator dreht das um: er tritt als BLE-**Peripheral** auf und gibt sich als 123ignition aus (NUS-Service, Gerätename, Antworten auf die bekannten Telegramme), sodass der Hub sich ganz normal verbinden und live Daten empfangen kann — RPM, Vorverstellung, MAP, Temperatur, Spulenstrom, Spannung — plus das Live-Zündwinkel-Tuning (T/A/R).

## Protokoll (BLE NUS)

- **RPM-Sweep oder feste RPM** (per WebGUI einstellbar) treibt einen simulierten Zündwinkel-/MAP-Verlauf.
- **Live-Tuning T/A/R**: der Hub kann wie an einem echten 123ignition den Zündwinkel-Offset live verstellen (Tuning-Modus an/aus, Auf/Ab, Reset) — der Emulator wendet den Offset auf die simulierte Kurve an.
- Gemeinsamer Decoder (`tune123Decode`) mit dem Hub-Repo, damit Emulator und Hub garantiert dasselbe Frameformat sprechen.

## Aufbau (Tischtest)

- Board: ESP32-S3, PlatformIO-Env **`emu_com17`** (Port COM17, `ENABLE_EMU123=1`)
- Läuft als eigenständiger Hub-Build mit `hubFeatEmu123=true` — im Emulator-Modus sind CAN/UART/Speed-Reed deaktiviert, es bleibt nur BLE + WebGUI.
- WebGUI zum Einstellen von RPM-Sweep/feste RPM, Tuning-Live-Werten und WLAN (inkl. Static-IP/DHCP je Profil, portiert vom Hub-Repo).
- Der `emu123`-Modus wird bewusst **per Reboot** umgeschaltet (kein Live-Toggle), damit BLE-Peripheral- und -Client-Rolle nie gleichzeitig aktiv sind.

## Verwandte Repos

- [spartan3v2-can-adapter](https://github.com/niedi74/spartan3v2-can-adapter) — die eigentliche Hub-Firmware, die sich gegen diesen Emulator testen lässt.
- [spartan3-hub-app](https://github.com/niedi74/spartan3-hub-app) — Android-Cockpit-Apps für den Hub.
