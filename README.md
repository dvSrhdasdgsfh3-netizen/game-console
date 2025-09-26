# Retro Spielebox

Dieses Projekt ist eine kleine Spielkonsole für Arduino-kompatible Boards mit ILI9341-TFT-Display und mehreren klassischen Spielen (Snake, Pong, Pac-Man, Donkey Kong). Die Steuerung erfolgt über sechs Taster.

## Features

- Auswahlmenü für verschiedene Spiele
- Grafische Darstellung auf ILI9341-TFT
- Steuerung über Taster (UP, DOWN, LEFT, RIGHT, A, B)

## Verkabelung

| Display Pin | Arduino Pin |
|-------------|-------------|
| CS          | 5           |
| RST         | 4           |
| DC          | 6           |
| MOSI        | 11 (HW SPI) |
| SCK         | 13 (HW SPI) |
| MISO        | 12 (HW SPI, falls benötigt) |
| VCC         | 5V          |
| GND         | GND         |

| Button      | Arduino Pin |
|-------------|-------------|
| UP          | 22          |
| DOWN        | 23          |
| LEFT        | 24          |
| RIGHT       | 25          |
| A           | 26          |
| B           | 27          |

Alle Buttons werden gegen GND geschaltet und mit `INPUT_PULLUP` konfiguriert.

## Beispiel-Schaltplan

```
   +-----------------------------+
   |         ILI9341 TFT         |
   |   +---------------------+   |
   |   |      Arduino        |   |
   |   |   +-------------+   |   |
   |   |   |           5 |---+---CS
   |   |   |           4 |---+---RST
   |   |   |           6 |---+---DC
   |   |   |          11 |---+---MOSI
   |   |   |          13 |---+---SCK
   |   |   |          12 |---+---MISO (optional)
   |   |   |          5V |---+---VCC
   |   |   |         GND |---+---GND
   |   +-------------+   |   |
   |                     |   |
   |   Buttons:          |   |
   |   22 --[Taster]-- GND   |
   |   23 --[Taster]-- GND   |
   |   24 --[Taster]-- GND   |
   |   25 --[Taster]-- GND   |
   |   26 --[Taster]-- GND   |
   |   27 --[Taster]-- GND   |
   +-------------------------+---+
```

## Hinweise

- Die Pinbelegung kann je nach Board angepasst werden.
- Die Spiele starten nach Auswahl im Menü.
- Zum Kompilieren werden die Bibliotheken `Adafruit_GFX` und `Adafruit_ILI9341` benötigt.

## Simulation in Wokwi

Du kannst das Projekt auch in der Online-Simulation [Wokwi](https://wokwi.com/) testen. Kopiere dazu den Inhalt der Datei `wokwi.json` aus diesem Repository und füge ihn im Wokwi-Editor unter "Projekt → Importieren → Aus JSON einfügen" ein.

Viel Spaß beim Nachbauen!
