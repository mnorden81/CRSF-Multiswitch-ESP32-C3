# CRSF-Multiswitch-ESP32-C3

CRSF-Multiswitch mit 8 Schaltkanaelen auf einem ESP32-C3 SuperMini.

Kleiner, guenstiger Multiswitch fuer RC-Modelle: Der ESP32-C3 empfaengt CRSF vom Empfaenger und schaltet ueber einen ULN2803A bis zu 8 Verbraucher (Licht, Relais, Funktionen). Das Layout folgt dem CRSF Multiswitch THT v1.1, alle Bauteile ausser den JST-GH-Buchsen sind bedrahtet.

Ausfuehrliche Projektbeschreibung: [docs/Projektbeschreibung.docx](docs/Projektbeschreibung.docx)

## Merkmale
- 8 Schaltkanaele ueber ULN2803A, Kontroll-LED je Kanal
- UART A (Eingang vom Empfaenger) und UART B (Weiterleitung zum naechsten Modul)
- Versorgung des ESP32 aus +5 V des Empfaengers, Last getrennt mit 12-24 V
- Eigener THT-Footprint fuer den SuperMini im Projekt

## Hardware (v0.3 (Entwurf))
Board: ESP32-C3 SuperMini

- Lastspannung 12-24 V (VLOAD) an CN2 (JST PH 2), TVS-Diode P6KE33A als Schutz.
- Treiber ULN2803A, 8 Kanaele, max. 500 mA je Kanal (Verlustleistung des ULN2803A bei mehreren Kanaelen beachten).
- R9-R16 (THT 1/4 W, Standard 150 R) zwischen ULN-Ausgang und Stiftleiste J1, je nach Verbraucher waehlen.
- R1-R8 und LED1-LED8 (3 mm) als Kontroll-LED je Kanal, vom Steuersignal Sn gegen GND.
- Keine Pulldowns und keine Widerstaende in den UART-Leitungen. Der ESP32 wird aus +5 V von CN1 (BEC/Empfaenger) versorgt.

### Belegung
| Funktion | GPIO |
|---|---|
| S0..S7 | IO0..IO7 |
| UART A (CN1, Eingang) | RX IO20, TX IO21 |
| UART B (J5) | TX IO10, RX IO8 |

### Anschluesse
- CN1 UART A (JST-GH 4): 1 GND, 2 +5 V, 3 RX, 4 TX
- J5 UART B (JST-GH 4): 1 GND, 2 +5 V, 3 TX, 4 RX (1:1-Kabel zum naechsten Modul)
- CN2 Last 12-24 V (JST PH 2, max. ca. 2 A je Kontakt)
- J1 Ausgaenge S0..S7

### Hinweise
- IO8 haengt beim SuperMini an der Onboard-LED, IO9 am BOOT-Taster. UART B RX liegt auf IO8.
- Footprint ESP32-C3-SuperMini_THT ist eine eigene Zeichnung (2x8, 2,54 mm, Reihenabstand 15,24 mm). Vor der Fertigung 1:1 am echten Board pruefen.

## Firmware (geplant)
- CRSF als einziges Protokoll, busfaehig ueber die UART-Anschluesse (Eingang plus Weiterleitung zum naechsten Modul).
- Gemeinsame CRSF-Bibliothek als Kopie im Projekt (firmware/lib), keine externe Abhaengigkeit.
- Einstellung der Adresse und Parameter ueber CRSF-Parameter (Lua am Sender) und Web-UI.
- Build mit PlatformIO, Bauweise wie bei den Sound-Mini-Repos.

## Ordner
- `hardware/`: KiCad-Projekt (Schaltplan, PCB, PDF, eigene Footprints, fp-lib-table). Entwurfsnotizen in [hardware/README-hardware.md](hardware/README-hardware.md).
- `firmware/`: Firmware, noch nicht begonnen.
- `docs/`: Projektbeschreibung als DOCX.

## Stand
- Hardware: Schaltplan und PCB vorhanden. ERC/DRC noch in KiCad ausfuehren.
- Firmware: noch nicht begonnen.

## Offene Punkte
- Pad-Namen und Footprint am realen Board pruefen
- Verlustleistung des ULN2803A bei mehreren Kanaelen mit 500 mA pruefen
- Firmware schreiben
