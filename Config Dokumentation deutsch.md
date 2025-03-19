# ** Dokumentation: Konfigurationsparameter für die Krypto-Trading-Bot-Strategie**  

## **📌 Inhaltsverzeichnis**  
1. [Allgemeine Einstellungen](#allgemeine-einstellungen)  
2. [Signal-Gewichtungen](#signal-gewichtungen)  
3. [Positionsgrößen-Parameter](#positionsgrößen-parameter)  
4. [Signal-Schwellenwerte](#signal-schwellenwerte)  
5. [Volatilitäts-Parameter](#volatilitäts-parameter)  
6. [Trend-Parameter](#trend-parameter)  
7. [Marktphasen-Parameter](#marktphasen-parameter)  
8. [Positions-Management-Parameter](#positions-management-parameter)  
9. [Trailing-Stop-Parameter](#trailing-stop-parameter)  
10. [Gewinnschutz-Parameter](#gewinnschutz-parameter)  
11. [Verlust-Begrenzungs-Parameter](#verlust-begrenzungs-parameter)  

---

## **📌 6. Trend-Parameter**  

### **TREND_SIGNAL_REDUCTION**  
**Beschreibung:** Faktor, um den das Handelssignal reduziert wird, wenn es gegen den aktuellen Markttrend läuft.  
**Werte:** Dezimalzahl zwischen `0` und `1` (z. B. `0.2` bedeutet, dass das Signal um **80% reduziert** wird).  
**Auswirkung:** Je niedriger dieser Wert ist, desto stärker werden Signale gegen den Trend abgeschwächt.  
**Empfohlene Werte:**  
- **Konservative Strategie:** `0.1` (90% Reduktion)  
- **Moderate Strategie:** `0.2` (80% Reduktion)  
- **Aggressive Strategie:** `0.4` (60% Reduktion)  

### **TREND_CONFIDENCE_REDUCTION**  
**Beschreibung:** Faktor, um den das Vertrauensniveau (`confidence`) reduziert wird, wenn das Signal gegen den Trend läuft.  
**Werte:** Dezimalzahl zwischen `0` und `1` (z. B. `0.5` bedeutet, dass die Confidence um **50% reduziert** wird).  
**Auswirkung:** Senkt die Wahrscheinlichkeit, dass Trades gegen den Trend ausgeführt werden.  
**Empfohlene Werte:**  
- **Sichere Strategie:** `0.3` (70% Reduktion)  
- **Ausgewogene Strategie:** `0.5` (50% Reduktion)  
- **Riskante Strategie:** `0.7` (30% Reduktion)  

---

## **📌 7. Marktphasen-Parameter**  

### **EXPANSION_TREND_STRENGTH**  
**Beschreibung:** Mindest-Trendstärke, die erforderlich ist, um eine Expansion-Phase zu erkennen.  
**Werte:** Dezimalzahl zwischen `0` und `1` (z. B. `0.7` bedeutet, dass mindestens **70% der Indikatoren einen Aufwärtstrend zeigen müssen**).  
**Auswirkung:** Höhere Werte führen zu weniger, aber präziseren Expansion-Phasen.  
**Empfohlene Werte:**  
- **Strenge Erkennung:** `0.8` (nur sehr starke Trends gelten als Expansion)  
- **Standard:** `0.7`  
- **Lockerere Erkennung:** `0.6` (mehr Expansion-Phasen, aber mit höherem Fehlsignal-Risiko)  

### **EXPANSION_RECENT_RANGE_RATIO**  
**Beschreibung:** Mindestveränderung des Preisbereichs relativ zum 50-Kerzen-Bereich, um eine Expansion-Phase zu bestätigen.  
**Werte:** Dezimalzahl zwischen `0` und `1` (z. B. `0.5` bedeutet, dass sich der Preisbereich um **mindestens 50%** verändert haben muss).  
**Auswirkung:** Höhere Werte sorgen für genauere Expansion-Erkennung, niedrigere Werte führen zu häufigeren Expansion-Trades.  
**Empfohlene Werte:**  
- **Strenge Erkennung:** `0.6`  
- **Standard:** `0.5`  
- **Lockerere Erkennung:** `0.4`  

### **EXPANSION_VOLATILITY**  
**Beschreibung:** Mindestvolatilität (%), die erforderlich ist, um eine Expansion-Phase zu erkennen.  
**Werte:** Dezimalzahl zwischen `0` und `5` (z. B. `1.5` bedeutet, dass die Standardabweichung der Preisbewegungen **mindestens 1.5% beträgt**).  
**Auswirkung:** Höhere Werte reduzieren Fehlsignale, niedrigere Werte erkennen mehr Expansion-Phasen.  
**Empfohlene Werte:**  
- **Strenge Erkennung:** `2.0`  
- **Standard:** `1.5`  
- **Lockerere Erkennung:** `1.2`  

---

## **📌 8. Positions-Management-Parameter**  

### **MAX_UNPROFITABLE_HOLD_HOURS**  
**Beschreibung:** Maximale Anzahl an Stunden, die eine **unprofitable Position** gehalten wird, bevor sie zwangsweise geschlossen wird.  
**Werte:** Ganzzahl (z. B. `3` bedeutet, dass eine unprofitable Position **maximal 3 Stunden gehalten wird**).  
**Empfohlene Werte:**  
- **Kurzfristiger Handel:** `2-4 Stunden`  
- **Mittelfristiger Handel:** `6-12 Stunden`  
- **Langfristiger Handel:** `24+ Stunden`  

---

## **📌 9. Trailing-Stop-Parameter**  

### **TRAILING_STOP_ACTIVATION**  
**Beschreibung:** Mindestgewinn (%), bevor ein Trailing-Stop aktiviert wird.  
**Werte:** Dezimalzahl zwischen `0` und `5` (z. B. `2.5` bedeutet, dass ein Trailing-Stop erst aktiviert wird, wenn die Position **2.5% im Gewinn ist**).  
**Empfohlene Werte:**  
- **Sichere Strategie:** `1.5%`  
- **Standard:** `2.5%`  
- **Aggressive Strategie:** `3.5%`  

### **TRAILING_STOP_DISTANCE**  
**Beschreibung:** Abstand des Trailing-Stops zum aktuellen Preis nach Aktivierung.  
**Werte:** Dezimalzahl zwischen `0` und `5` (z. B. `1.0` bedeutet, dass der Trailing-Stop **1% unter dem Höchstpreis** liegt).  
**Empfohlene Werte:**  
- **Enger Stop:** `0.5%`  
- **Standard:** `1.0%`  
- **Weiter Stop:** `1.5%`  

---

## **📌 10. Gewinnschutz-Parameter**  

### **PROFIT_PROTECTION_MIN**  
**Beschreibung:** Mindestgewinn (%), bevor der Gewinnschutz aktiviert wird.  
**Werte:** Dezimalzahl zwischen `0` und `5` (z. B. `1.2` bedeutet, dass der Gewinnschutz ab **1.2% Gewinn** aktiviert wird).  
**Empfohlene Werte:**  
- **Sichere Strategie:** `1.0%`  
- **Standard:** `1.2%`  
- **Aggressive Strategie:** `1.5%`  

### **PROFIT_PROTECTION_FACTOR**  
**Beschreibung:** Faktor, der bestimmt, wie viel Gewinnrückgang toleriert wird, bevor eine Position geschlossen wird.  
**Werte:** Dezimalzahl zwischen `0` und `1` (z. B. `0.8` bedeutet, dass eine Position geschlossen wird, wenn **20% des maximalen Gewinns verloren gehen**).  
**Empfohlene Werte:**  
- **Konservativ:** `0.9` (10% Verlust wird toleriert)  
- **Standard:** `0.8` (20% Verlust wird toleriert)  
- **Riskanter:** `0.7` (30% Verlust wird toleriert)  

---

## **📌 11. Verlust-Begrenzungs-Parameter**  

### **MAX_CONSECUTIVE_LOSSES**  
**Beschreibung:** Maximale Anzahl an **aufeinanderfolgenden Verlust-Trades**, bevor der Handel pausiert wird.  
**Werte:** Ganzzahl (z. B. `2` bedeutet, dass der Bot nach **2 Verlust-Trades** pausiert).  
**Empfohlene Werte:**  
- **Sichere Strategie:** `1`  
- **Standard:** `2`  
- **Riskanter:** `3`  

---

### ** Fazit & Vorteile der neuen Parameter**  
 **Mehr Kontrolle über Trend- & Marktphasenanalyse**  
 **Bessere Anpassung an verschiedene Marktbedingungen**  
 **Keine Code-Änderungen mehr nötig – alles über `config.json` steuerbar**  
