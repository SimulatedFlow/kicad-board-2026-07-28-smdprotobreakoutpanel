```json
{
  "board_name": "SMDProtoBreakoutPanel",
  "one_liner": "Ein universelles, per V-Cut ausbrechbares SMD-zu-THT Adapterpanel für 8 gängige SMD-Footprints auf 2,54mm-Breadboard-Raster.",
  "market_gap": "Bastler scheuen oft SMD-Bauteile wegen der schwierigen Prototypisierung auf Breadboards. Dieses günstige, kombinierte All-in-One-Panel löst das Problem elegant und kostengünstig.",
  "confidence": "high",
  "price_eur": 12,
  "target_enclosure": "Keines (Direkte Breadboard-Nutzung / 100x100mm Panel)",
  "injection_notes": "keine"
}
```

## BUILD-PROMPT

### 1. Allgemeine Systemvorgaben (DFM & PCB-Eigenschaften)
- **Abmessungen:** Gesamt-Platine 100.0 mm × 100.0 mm. Vier Ecken bei (0,0), (100,0), (100,100), (0,100).
- **Lagen:** 4-Lagen-Standard-Stackup (F.Cu, In1.Cu, In2.Cu, B.Cu). Die internen Lagen `In1.Cu` (GND) und `In2.Cu` (VCC) werden als isolierte, segmentierte Shielding-Planes für jedes Segment ausgelegt (keine globale Verbindung, da alle Segmente elektrisch autark sind).
- **Leiterbahnen:** Signal-Clearance ≥ 0.3 mm, Bahnenbreite ≥ 0.4 mm.
- **V-Cut-Schnittlinien (Mausbisse/V-Scoring):**
  - Vertikale Score-Lines auf Layer `User.Drawings` (mit Silk-Text "V-CUT"):
    - X = 25.0 mm (von Y=0 bis Y=100)
    - X = 50.0 mm (von Y=0 bis Y=100)
    - X = 75.0 mm (von Y=0 bis Y=100)
  - Horizontale Score-Line auf Layer `User.Drawings`:
    - Y = 50.0 mm (von X=0 bis X=100)
  - *Dies teilt das Panel in exakt 8 identische Segmente von 25 mm × 50 mm.*
- **Montagelöcher:** 4× M3-Bohrungen (Durchmesser 3.2 mm, Pad-Durchmesser 6.0 mm) an den Ecken des Gesamtpanels:
  - Loch 1: (5.0, 5.0) mm
  - Loch 2: (95.0, 5.0) mm
  - Loch 3: (95.0, 95.0) mm
  - Loch 4: (5.0, 95.0) mm

---

### 2. Segment-Spezifikation (Schaltplan & Platzierung)

Jedes Segment ist elektrisch vollständig isoliert. Die Pins des SMD-Footprints werden 1:1 parallel auf die zugehörigen THT-Pinheader (RM 2.54 mm) herausgeführt, die an den Rändern des jeweiligen Segments platziert sind.

#### REIHE 1 (Y: 0.0 mm bis 50.0 mm)

- **Segment 1 (X: 0.0 bis 25.0 mm): TQFP-32 Adapter**
  - **SMD-Bauteil:** U1 (`Package_QFP:TQFP-32_7x7mm_P0.8mm`) platziert im Zentrum bei (12.5, 25.0).
  - **THT-Anschlüsse:** J1 & J2 (je `Connector_PinHeader_2.54mm:PinHeader_1x16_P2.54mm_Vertical`).
    - J1 platziert links bei X = 2.54 mm (Pins 1-16)
    - J2 platziert rechts bei X = 22.46 mm (Pins 17-32)
  - **Verbindung:** U1 Pins 1–16 zu J1 Pins 1–16; U1 Pins 17–32 zu J2 Pins 1–16.
  - **Silk-Beschriftung:** "TQFP-32 to DIP-32"

- **Segment 2 (X: 25.0 bis 50.0 mm): SOIC-8 Adapter**
  - **SMD-Bauteil:** U2 (`Package_SO:SOIC-8_3.9x4.9mm_P1.27mm`) bei (37.5, 25.0).
  - **THT-Anschlüsse:** J3 & J4 (je `Connector_PinHeader_2.54mm:PinHeader_1x04_P2.54mm_Vertical`).
    - J3 platziert links bei X = 33.68 mm (Pins 1-4)
    - J4 platziert rechts bei X = 41.32 mm (Pins 5-8)
  - **Verbindung:** U2 Pins 1–4 zu J3; U2 Pins 5–8 zu J4.
  - **Silk-Beschriftung:** "SOIC-8 to DIP-8"

- **Segment 3 (X: 50.0 bis 75.0 mm): SOT-23-6/5/3 Adapter**
  - **SMD-Bauteil:** U3 (`Package_TO_SOT_SMD:SOT-23-6_Handsoldering`) bei (62.5, 25.0).
  - **THT-Anschlüsse:** J5 & J6 (je `Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical`).
    - J5 links bei X = 58.68 mm, J6 rechts bei X = 66.32 mm.
  - **Verbindung:** U3 Pins 1–3 zu J5; U3 Pins 4–6 zu J6.
  - **Silk-Beschriftung:** "SOT-23-6 to DIP-6"

- **Segment 4 (X: 75.0 bis 100.0 mm): SOT-223 Adapter**
  - **SMD-Bauteil:** U4 (`Package_TO_SOT_SMD:SOT-223-3_TabPin2`) bei (87.5, 25.0).
  - **THT-Anschlüsse:** J7 (`Connector_PinHeader_2.54mm:PinHeader_1x04_P2.54mm_Vertical`) bei X = 93.0 mm.
  - **Verbindung:** U4 Pin 1 zu J7-1, Pin 2 (Tab) zu J7-2, Pin 3 zu J7-3, Pin 4 (Zusatz-Tab/Heatsink) zu J7-4.
  - **Silk-Beschriftung:** "SOT-223 to DIP-4"

---

#### REIHE 2 (Y: 50.0 mm bis 100.0 mm)

- **Segment 5 (X: 0.0 bis 25.0 mm): SC-70-6 Adapter**
  - **SMD-Bauteil:** U5 (`Package_TO_SOT_SMD:SOT-363_SC-70-6_Handsoldering`) bei (12.5, 75.0).
  - **THT-Anschlüsse:** J8 & J9 (je `Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical`).
    - J8 links bei X = 8.68 mm, J9 rechts bei X = 16.32 mm.
  - **Verbindung:** U5 Pins 1–3 zu J8; U5 Pins 4–6 zu J9.
  - **Silk-Beschriftung:** "SC-70 to DIP-6"

- **Segment 6 (X: 25.0 bis 50.0 mm): 0805 & 0603 Passives Breakout**
  - **SMD-Bauteile:** 
    - R1, R2 (`Resistor_SMD:R_0805_2012Metric_Pad1.15x1.40mm_HandSolder`) bei Y = 65.0 mm.
    - R3, R4 (`Resistor_SMD:R_0603_1608Metric_Pad1.05x0.95mm_HandSolder`) bei Y = 85.0 mm.
  - **THT-Anschlüsse:** Vier 2-Pin-Header J10, J11, J12, J13 (`Connector_PinHeader_2.54mm:PinHeader_1x02_P2.54mm_Vertical`).
  - **Verbindung:**
    - R1 Pins 1-2 an J10 Pins 1-2.
    - R2 Pins 1-2 an J11 Pins 1-2.
    - R3 Pins 1-2 an J12 Pins 1-2.
    - R4 Pins 1-2 an J13 Pins 1-2.
  - **Silk-Beschriftung:** "0805 / 0603 Passive Breakout"

- **Segment 7 (X: 50.0 bis 75.0 mm): SOD-123 & SOD-323 Dioden**
  - **SMD-Bauteile:**
    - D1, D2 (`Diode_SMD:D_SOD-123_HandSoldering`) bei Y = 65.0 mm.
    - D3, D4 (`Diode_SMD:D_SOD-323_HandSoldering`) bei Y = 85.0 mm.
  - **THT-Anschlüsse:** Vier 2-Pin-Header J14, J15, J16, J17 (`Connector_PinHeader_2.54mm:PinHeader_1x02_P2.54mm_Vertical`).
  - **Verbindung:**
    - D1 Anode/Kathode an J14.
    - D2 Anode/Kathode an J15.
    - D3 Anode/Kathode an J16.
    - D4 Anode/Kathode an J17.
  - **Silk-Beschriftung:** "Diodes SOD-123 / SOD-323" (Kathodenstrich auf Silk deutlich markieren!)

- **Segment 8 (X: 75.0 bis 100.0 mm): SOIC-16 Adapter**
  - **SMD-Bauteil:** U6 (`Package_SO:SOIC-16_3.9x9.9mm_P1.27mm`) bei (87.5, 75.0).
  - **THT-Anschlüsse:** J18 & J19 (je `Connector_PinHeader_2.54mm:PinHeader_1x08_P2.54mm_Vertical`).
    - J18 links bei X = 81.14 mm (Pins 1-8)
    - J19 rechts bei X = 93.86 mm (Pins 9-16)
  - **Verbindung:** U6 Pins 1–8 zu J18; U6 Pins 9–16 zu J19.
  - **Silk-Beschriftung:** "SOIC-16 to DIP-16"

---

### 3. Auszuführende Design-Schritte (KiCad-Automatisierungs-Pipeline)
1. **Projekt-Setup:** Lege ein neues KiCad-Projekt namens `SMDProtoBreakoutPanel` an.
2. **Schaltplan (Eeschema):** Erstelle die 8 isolierten Schaltplanabschnitte mit den oben definierten Bauteilen, Anschlüssen und Netzverbindungen.
3. **Platinen-Layout (Pcbnew):**
   - Definiere die Edge.Cuts als quadratischen Umriss von 100.0 mm × 100.0 mm.
   - Setze die 4 M3-Befestigungslöcher in die Ecken.
   - Platziere die SMD-Footprints und THT-Pinheader exakt innerhalb der 8 Segmentbereiche (jeweils 25×50 mm) laut Koordinatenvorgaben.
   - Zeichne die V-Cut-Trennlinien auf den Layer `User.Drawings`.
   - Bringe die Text-Schilder auf dem Silk-Layer (`F.SilkS`) an, sodass jedes Segment und jede Pinnummerierung gut ablesbar ist.
4. **Routing:**
   - Verbinde die Netze lokal auf den Segmenten. Da es sich um simple Punkt-zu-Punkt-Verbindungen handelt, ist dies extrem stabil autoroutbar.
   - Generiere Kupfer-Flächen für die Innenlagen als Schirmflächen (je Segment isoliert).
5. **DRC-Prüfung:** Führe den Design Rule Check (DRC) durch und behebe etwaige Fehler.
6. **Export:** Exportiere die Gerber-Dateien und Excellon-Bohrdateien strukturiert in das Verzeichnis `./gerbers`.

*Bitte fass am Ende des Prozesses kurz und ehrlich zusammen, was erfolgreich abgeschlossen wurde.*
