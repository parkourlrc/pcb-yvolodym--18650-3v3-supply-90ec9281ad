# Spannungsmessungen

Sammlung von Soll- und Messwerten an der Schaltung. Pro Messsituation ein Abschnitt:
Ausgangssituation beschreiben, Sollwerte eintragen, Messwerte nachtragen.

Alle Spannungen gegen **GND = P−** (Systemmasse hinter dem FS8205A), sofern nicht anders angegeben.

---

## 1. TP4056 (U1) – USB angeschlossen, Akku eingelegt, Schalter geschlossen

### Ausgangssituation

| Punkt | Zustand |
|---|---|
| USB-C (P1) | angeschlossen, 5-V-Netzteil |
| Akku (BT1) | 18650 eingelegt, Zellspannung: _____ V (vor Messung) |
| Schalter SW2 | geschlossen (BT1+ ↔ BAT+) |
| Last an J1 | _____ (keine / Widerstand / Gerät, Strom: _____ mA) |
| Datum / Messgerät | _____ |

### Schaltung um U1 (laut Netzliste)

- **VCC (Pin 4)** und **CE (Pin 8)** an VBUS → Lader immer aktiv, sobald USB anliegt.
- **BAT (Pin 5)** am Netz BAT+ – dort hängen auch C2/C4/C6, MT3608 (IN + EN), R4 (DW01A-VCC) und
  Teiler R8.
- **GND (Pin 3/EP)** = Systemmasse P−, also **hinter** den Schutz-MOSFETs.
- Ladestrompfad: TP4056 BAT → SW2 → Zelle → BT− → Q1 (beide FETs) → GND.

### Spannungen an U1

| Pin | Signal | Sollwert | Messwert |
|---|---|---|---|
| 4 VCC | VBUS | ≈ 5,0 V (4,75 … 5,25 V), unter Last je nach Kabel etwas weniger | |
| 8 CE | VBUS | wie VCC, ≈ 5 V | |
| 3 GND / EP | GND | 0 V | |
| 1 TEMP | GND | 0 V (NTC-Überwachung deaktiviert) | |
| 5 BAT | BAT+ | Zellspannung + ≈ 50 … 100 mV, max. 4,20 V ± 1 % | |
| 2 PROG | R1 = 1,2 kΩ | 1,0 V bei vollem Ladestrom (CC), ≈ 0,1 V beim Vorladen, in CV fallend gegen 0 V | |
| 7 CHRG | LED D1 rot | lädt: ≈ 0,1 … 0,3 V (D1 an, ca. 3 mA); voll: hochohmig, D1 aus | |
| 6 STDBY | LED D2 grün | lädt: hochohmig, D2 aus (Messgerät zeigt undefinierten Wert von einigen Volt); voll: ≈ 0,1 … 0,3 V, D2 an | |

Zusätzlich:

| Messpunkt | Sollwert | Messwert |
|---|---|---|
| Zellspannung direkt an BT1 (+ gegen −) | 2,9 … 4,2 V je nach Ladezustand | |
| Ladestrom aus PROG: I ≈ V_PROG × 1000 mA/V | ≈ 1,0 A in CC (siehe Hinweis unten) | |

### Ladephasen (Pin 5, BAT)

1. **Vorladen** (Zelle < 2,9 V): Strom ≈ 1/10, also ≈ 100 mA; PROG ≈ 0,1 V.
2. **Konstantstrom (CC)** (2,9 … 4,2 V): voller Ladestrom, PROG = 1,0 V. BAT steigt mit der Zelle.
   Pin 5 liegt um I × (Schalter + Q1 + Leiterbahnen) über der echten Zellspannung, bei 1 A etwa
   50–100 mV – die TP4056-Masse liegt hinter den MOSFETs.
3. **Konstantspannung (CV)**: BAT bleibt bei 4,20 V, der Strom sinkt. Unter ≈ 1/10 Ladestrom →
   grüne LED. Die Zelle endet bei ≈ 4,19 V.

Der DW01A schaltet erst bei 4,3 V wegen Überladung ab und greift beim normalen Laden nicht ein.

### Hinweise

- **Laststrom geht vom Ladestrom ab.** Boost und LDO hängen direkt an BAT+. Zieht der Ausgang
  dauerhaft mehr als ≈ 100 mA, wird das Ladeende nie erkannt: rote LED bleibt an, BAT bleibt bei 4,2 V.
- **Wärme:** Bei leerer Zelle verheizt der TP4056 (5 V − 3 V) × 1 A ≈ 2 W. Ab ca. 120 °C regelt er
  den Strom selbst herunter – PROG unter 1 V ist dann normal.
- **Ladestrom:** Laut Datenblattformel (I = 1200 V / R_PROG) ergibt R1 = 1,2 kΩ **1,0 A**, nicht
  0,9 A wie in `README.md` angegeben – gegen Datenblatt prüfen.
- **Beim Umschalten:** Steckt USB schon, bevor SW2 schließt, liegt BAT+ auf ≈ 4,2 V. Beim Schließen
  entladen sich C2/C4/C6 kurz in die Zelle (harmlos), danach CC-Phase.
- **Offener Schalter + USB:** Der TP4056 versorgt BAT+ trotzdem – Boost, LDO und die 3,3 V an J1
  laufen weiter, nur die Zelle ist abgetrennt. Der TP4056 sieht keinen Akku, die LEDs können flackern.

### Beobachtungen

_(hier Auffälligkeiten beim Messen notieren)_

---

## 2. _(nächste Messsituation)_

### Ausgangssituation

| Punkt | Zustand |
|---|---|
| USB-C (P1) | |
| Akku (BT1) | |
| Schalter SW2 | |
| Last an J1 | |
| Datum / Messgerät | |

### Spannungen

| Messpunkt | Sollwert | Messwert |
|---|---|---|
| | | |
