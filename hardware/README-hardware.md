# RC-Multiswitch-ESP32-C3 (v0.3, Entwurf)
Ein Multiswitch nach CRSF Multiswitch THT v1.1, ULN2803A, 8 Kanaele.

- Last: 12-24V (VLOAD, JST PH 2 an CN2 - Achtung: max. ca. 2A je Kontakt; TVS P6KE33A), max. 500mA je Kanal.
- R9-R16 (THT 1/4W, Standard 150R) zwischen ULN-Ausgang und Stiftleiste J1: je Verbraucher waehlen, Leistung beachten.
- R1-R8 (150R) + LED1-8 (3mm): Kontroll-LED je Kanal, vom Steuersignal Sn gegen GND.
- Keine Pulldowns, keine Widerstaende in den UART-Leitungen.
- ESP32 wird aus +5V von CN1 (BEC/Empfaenger) versorgt.
- S0..S7 = IO0..IO7; UART A (Eingang): RX=IO20, TX=IO21; UART B (zusaetzlich): TX=IO10, RX=IO8. Adresse per Firmware/Web-UI/Lua.
- UART-Buchsen CN1, J5 (und J6 beim S3) als JST-GH (SMD-only, kein THT-Typ; Footprint BM04B-GHS-TBT). CN1 (UART A): 1 GND, 2 +5V, 3 RX, 4 TX. J5 (UART B): 1 GND, 2 +5V, 3 TX, 4 RX (1:1-Kabel zum naechsten Modul).

Zu pruefen: Pad-Namen am realen Board, ULN2803A-Verlustleistung bei mehreren Kanaelen mit 500mA. Kein ERC, keine Firmware getestet.

## Board: ESP32-C3 SuperMini (v0.3)
- Symbol-Pads = Pad-Nummern des Boards laut mrtnvgr/KiCad_ESP32-C3-SuperMini (rechte Reihe 1..8 von unten: IO0-IO4, 3V3, GND, 5V; linke Reihe 9..16 von oben: IO5-IO10, IO20, IO21).
- Footprint `footprints.pretty/ESP32-C3-SuperMini_THT` (eigene Zeichnung, 2x8, Raster 2,54 mm, Reihenabstand 15,24 mm, USB oben). Bindung per `fp-lib-table` im Projekt. Vor dem Layout 1:1 am echten Board pruefen.
- IO8 haengt beim SuperMini an der Onboard-LED, IO9 am BOOT-Taster (UART B RX = IO8 beachten).
