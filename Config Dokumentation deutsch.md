# Dokumentation: Konfigurationsparameter für die Krypto-Trading-Bot-Strategie

## Inhaltsverzeichnis
1. [Allgemeine Einstellungen](#allgemeine-einstellungen)
2. [Signal-Gewichtungen](#signal-gewichtungen)
3. [Positionsgrößen-Parameter](#positionsgrößen-parameter)
4. [Signal-Schwellenwerte](#signal-schwellenwerte)
5. [RSI-Parameter](#rsi-parameter)
6. [Momentum-Parameter](#momentum-parameter)
7. [Volumen-Parameter](#volumen-parameter)
8. [Gleitende-Durchschnitt-Parameter](#gleitende-durchschnitt-parameter)
9. [Indikatoren-Gewichtungen](#indikatoren-gewichtungen)
10. [Vertrauensniveau-Parameter](#vertrauensniveau-parameter)
11. [Volatilitäts-Parameter](#volatilitäts-parameter)
12. [Trend-Parameter](#trend-parameter)
13. [Kühlperioden-Parameter](#kühlperioden-parameter)
14. [Positions-Management-Parameter](#positions-management-parameter)
15. [Trailing-Stop-Parameter](#trailing-stop-parameter)
16. [Gewinnmitnahme-Parameter](#gewinnmitnahme-parameter)
17. [Gewinnschutz-Parameter](#gewinnschutz-parameter)
18. [Divergenz-Parameter](#divergenz-parameter)
19. [Markt-Timing-Parameter](#markt-timing-parameter)
20. [Zeitfilter-Parameter](#zeitfilter-parameter)
21. [Verlust-Begrenzungs-Parameter](#verlust-begrenzungs-parameter)

---

## Allgemeine Einstellungen

### UPDATE_INTERVAL
**Beschreibung:** Zeitintervall in Sekunden, nach dem die Strategie den Markt erneut analysiert und Handelssignale generiert.  
**Werte:** Angabe in Sekunden (z.B. 3600 = 1 Stunde, 1800 = 30 Minuten)  
**Auswirkung:** Kleinere Werte ermöglichen schnellere Reaktionen auf Marktbewegungen, verbrauchen aber mehr Ressourcen und können zu mehr Rauschen führen. Höhere Werte reduzieren Rauschen und API-Aufrufe, aber können wichtige Handelsmomente verpassen.  
**Optimale Verwendung:** Sollte mit dem gewählten Timeframe abgestimmt sein. Für einen 1h-Chart ist ein Intervall von 30-60 Minuten sinnvoll.

### LOOKBACK_WINDOW
**Beschreibung:** Anzahl der historischen Datenpunkte, die für die Analyse verwendet werden.  
**Werte:** Ganzzahl (z.B. 300 Kerzen)  
**Auswirkung:** Bestimmt, wie weit die Strategie in die Vergangenheit schaut, um Muster zu erkennen und Indikatoren zu berechnen.  
**Optimale Verwendung:** Sollte groß genug sein, um langfristige Trends zu erkennen, aber nicht so groß, dass veraltete Daten die Analyse beeinflussen.

### PREDICTION_HORIZON
**Beschreibung:** Anzahl der zukünftigen Zeiteinheiten, für die eine Prognose erstellt wird.  
**Werte:** Ganzzahl (z.B. 12 Perioden)  
**Auswirkung:** Beeinflusst, wie weit in die Zukunft die Strategie zu prognostizieren versucht.  
**Optimale Verwendung:** Kürzere Horizonte sind in der Regel genauer, aber längere können für strategische Entscheidungen hilfreich sein.

## Signal-Gewichtungen

### SIGNAL_WEIGHTS
**Beschreibung:** Gewichtungen für verschiedene Signalquellen, die zum Gesamtsignal beitragen.  
**Komponenten:**
- **hybrid_weight:** Gewichtung des eigenen hybriden Signals, das verschiedene technische Indikatoren kombiniert.
- **technical_weight:** Gewichtung der reinen technischen Analyse.
- **finrl_weight:** Gewichtung des FinRL-Modells (Reinforcement Learning).
- **sentiment_weight:** Gewichtung der Sentiment-Analyse.
- **lstm_weight:** Gewichtung des LSTM-Modells für Preisprognosen.
- **rl_weight:** Gewichtung des allgemeinen Reinforcement Learning-Modells.

**Auswirkung:** Bestimmt, wie stark jede Signalquelle das endgültige Handelssignal beeinflusst.  
**Optimale Verwendung:** Höhere Gewichtungen sollten den zuverlässigeren Signalquellen gegeben werden. In volatilen Märkten sollte das hybrid_weight erhöht werden.

## Positionsgrößen-Parameter

### MAX_POSITION_SIZE
**Beschreibung:** Maximaler Anteil des verfügbaren Kapitals, der für eine Position verwendet werden kann.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.7 = 70% des Kapitals)  
**Auswirkung:** Begrenzt das Risiko durch Begrenzung der Positionsgröße.  
**Optimale Verwendung:** Konservativere Werte (0.5-0.7) für risikobewusste Strategien, höhere Werte für aggressivere Strategien.

### BUY_POSITION_SIZE
**Beschreibung:** Anteil des verfügbaren Kapitals, der für Kauf-Positionen verwendet wird.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Bestimmt, wie viel Kapital bei Kaufsignalen eingesetzt wird.  
**Optimale Verwendung:** Sollte an die Marktsituation angepasst werden - höher in Bullenmärkten, niedriger in unsicheren Märkten.

### SELL_POSITION_SIZE
**Beschreibung:** Anteil der bestehenden Position, der bei Verkaufssignalen liquidiert wird.  
**Werte:** Dezimalzahl zwischen 0 und 1 (1.0 = 100% verkaufen)  
**Auswirkung:** Bestimmt, ob Positionen vollständig oder teilweise geschlossen werden.  
**Optimale Verwendung:** Typischerweise 1.0, um Positionen vollständig zu schließen, aber kann für Teil-Gewinnmitnahmen reduziert werden.

## Signal-Schwellenwerte

### BUY_THRESHOLD
**Beschreibung:** Mindestwert des Signals, ab dem ein Kaufsignal generiert wird.  
**Werte:** Dezimalzahl zwischen 0 und 1 (höhere Werte = selektivere Signale)  
**Auswirkung:** Bestimmt, wie stark ein Signal sein muss, um einen Kauf auszulösen.  
**Optimale Verwendung:** Höhere Werte (0.4-0.6) für selektivere und qualitativ hochwertigere Kaufsignale.

### STRONG_BUY_THRESHOLD
**Beschreibung:** Signalwert, ab dem ein starkes Kaufsignal generiert wird.  
**Werte:** Dezimalzahl zwischen 0 und 1 (höher als BUY_THRESHOLD)  
**Auswirkung:** Kann verwendet werden, um die Positionsgröße bei besonders starken Signalen anzupassen.  
**Optimale Verwendung:** Sollte deutlich höher sein als BUY_THRESHOLD (z.B. 0.7-0.85).

### SELL_THRESHOLD
**Beschreibung:** Maximalwert des Signals, unter dem ein Verkaufssignal generiert wird.  
**Werte:** Dezimalzahl zwischen -1 und 0 (niedrigere Werte = selektivere Signale)  
**Auswirkung:** Bestimmt, wie stark ein negatives Signal sein muss, um einen Verkauf auszulösen.  
**Optimale Verwendung:** Niedrigere Werte (-0.4 bis -0.6) für selektivere und qualitativ hochwertigere Verkaufssignale.

### STRONG_SELL_THRESHOLD
**Beschreibung:** Signalwert, unter dem ein starkes Verkaufssignal generiert wird.  
**Werte:** Dezimalzahl zwischen -1 und 0 (niedriger als SELL_THRESHOLD)  
**Auswirkung:** Kann verwendet werden, um die Positionsgröße bei besonders starken Verkaufssignalen anzupassen.  
**Optimale Verwendung:** Sollte deutlich niedriger sein als SELL_THRESHOLD (z.B. -0.7 bis -0.85).

### CONFIDENCE_THRESHOLD
**Beschreibung:** Mindestvertrauensniveau, das ein Signal haben muss, um zu handeln.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Ein höherer Wert bedeutet, dass nur Signale mit hohem Vertrauen zu Trades führen.  
**Optimale Verwendung:** Höhere Werte (0.4-0.65) für eine konservativere Strategie mit weniger, aber qualitativ besseren Trades.

## RSI-Parameter

### RSI_OVERSOLD_THRESHOLD
**Beschreibung:** RSI-Wert, unter dem ein Asset als stark überverkauft gilt.  
**Werte:** Ganzzahl zwischen 0 und 30 (typischerweise 20-30)  
**Auswirkung:** Niedrigere Werte führen zu selteneren, aber stärkeren Kaufsignalen.  
**Optimale Verwendung:** In Bärenmärkten sollten niedrigere Werte (18-20) verwendet werden, in Bullenmärkten können höhere Werte (25-30) effektiv sein.

### RSI_MODERATE_OVERSOLD
**Beschreibung:** RSI-Wert, unter dem ein Asset als moderat überverkauft gilt.  
**Werte:** Ganzzahl zwischen RSI_OVERSOLD_THRESHOLD und 50 (typischerweise 30-40)  
**Auswirkung:** Erzeugt schwächere Kaufsignale als starke Überverkauft-Bedingungen.  
**Optimale Verwendung:** Kann für frühere Einstiege mit kleineren Positionsgrößen verwendet werden.

### RSI_OVERBOUGHT_THRESHOLD
**Beschreibung:** RSI-Wert, über dem ein Asset als stark überkauft gilt.  
**Werte:** Ganzzahl zwischen 70 und 100 (typischerweise 70-80)  
**Auswirkung:** Höhere Werte führen zu selteneren, aber stärkeren Verkaufssignalen.  
**Optimale Verwendung:** In Bullenmärkten sollten höhere Werte (75-80) verwendet werden, in Bärenmärkten können niedrigere Werte (70-75) effektiv sein.

### RSI_MODERATE_OVERBOUGHT
**Beschreibung:** RSI-Wert, über dem ein Asset als moderat überkauft gilt.  
**Werte:** Ganzzahl zwischen 50 und RSI_OVERBOUGHT_THRESHOLD (typischerweise 60-70)  
**Auswirkung:** Erzeugt schwächere Verkaufssignale als starke Überkauft-Bedingungen.  
**Optimale Verwendung:** Kann für frühere Ausstiege mit teilweisen Positionsschließungen verwendet werden.

### RSI_NEUTRAL_LOW
**Beschreibung:** Untere Grenze des neutralen RSI-Bereichs.  
**Werte:** Ganzzahl zwischen 30 und 50 (typischerweise 40)  
**Auswirkung:** Beeinflusst das Vertrauensniveau für Signale im neutralen Bereich.  
**Optimale Verwendung:** Kann angepasst werden, um die "Neutralzone" zu definieren, in der Signale mit Vorsicht betrachtet werden sollten.

### RSI_NEUTRAL_HIGH
**Beschreibung:** Obere Grenze des neutralen RSI-Bereichs.  
**Werte:** Ganzzahl zwischen 50 und 70 (typischerweise 60)  
**Auswirkung:** Beeinflusst das Vertrauensniveau für Signale im neutralen Bereich.  
**Optimale Verwendung:** Kann angepasst werden, um die "Neutralzone" zu definieren, in der Signale mit Vorsicht betrachtet werden sollten.

## Momentum-Parameter

### MOMENTUM_12H_THRESHOLD
**Beschreibung:** Schwellenwert für die Preisbewegung über 12 Stunden, der als signifikant gilt.  
**Werte:** Dezimalzahl (z.B. 0.045 = 4.5% Preisänderung)  
**Auswirkung:** Niedrigere Werte erzeugen mehr, höhere Werte selektivere Momentum-Signale.  
**Optimale Verwendung:** Sollte an die typische Volatilität des Assets angepasst werden - höher für volatilere Assets.

### MOMENTUM_24H_THRESHOLD
**Beschreibung:** Schwellenwert für die Preisbewegung über 24 Stunden, der als signifikant gilt.  
**Werte:** Dezimalzahl (typischerweise höher als MOMENTUM_12H_THRESHOLD)  
**Auswirkung:** Niedrigere Werte erzeugen mehr, höhere Werte selektivere Momentum-Signale.  
**Optimale Verwendung:** Sollte an die typische Volatilität des Assets angepasst werden - höher für volatilere Assets.

### MOMENTUM_12H_MULTIPLIER
**Beschreibung:** Faktor, mit dem 12-Stunden-Preisänderungen multipliziert werden, um die Signalstärke zu berechnen.  
**Werte:** Ganzzahl (typischerweise 4-6)  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Kurzzeit-Momentum auf das Gesamtsignal.  
**Optimale Verwendung:** Höhere Werte für kurzfristige Strategien, niedrigere für längerfristige Ansätze.

### MOMENTUM_24H_MULTIPLIER
**Beschreibung:** Faktor, mit dem 24-Stunden-Preisänderungen multipliziert werden, um die Signalstärke zu berechnen.  
**Werte:** Ganzzahl (typischerweise 3-5)  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Mittelfrist-Momentum auf das Gesamtsignal.  
**Optimale Verwendung:** Sollte etwas niedriger sein als der 12H-Multiplier, da längerfristige Bewegungen bereits größer sind.

### MOMENTUM_12H_WEIGHT
**Beschreibung:** Gewichtung des 12-Stunden-Momentums im kombinierten Momentum-Signal.  
**Werte:** Dezimalzahl zwischen 0 und 1 (Summe mit MOMENTUM_24H_WEIGHT sollte 1 ergeben)  
**Auswirkung:** Höhere Werte betonen kurzfristige Preisbewegungen stärker.  
**Optimale Verwendung:** Höhere Werte (0.6-0.7) für reaktionsschnellere Strategien, niedrigere für stabilere Signale.

### MOMENTUM_24H_WEIGHT
**Beschreibung:** Gewichtung des 24-Stunden-Momentums im kombinierten Momentum-Signal.  
**Werte:** Dezimalzahl zwischen 0 und 1 (Summe mit MOMENTUM_12H_WEIGHT sollte 1 ergeben)  
**Auswirkung:** Höhere Werte betonen mittelfristige Preisbewegungen stärker.  
**Optimale Verwendung:** Höhere Werte (0.4-0.5) für stabilere Signale, niedrigere für reaktionsschnellere Strategien.

## Volumen-Parameter

### VOLUME_RATIO_THRESHOLD
**Beschreibung:** Verhältnis zwischen aktuellem Volumen und Durchschnittsvolumen, das als signifikant gilt.  
**Werte:** Dezimalzahl (typischerweise 1.5-2.5, wobei 1.0 das durchschnittliche Volumen ist)  
**Auswirkung:** Niedrigere Werte führen zu mehr Volumen-basierten Signalen, höhere zu selektiveren.  
**Optimale Verwendung:** Sollte basierend auf der typischen Volumenvolatilität des Assets angepasst werden.

## Gleitende-Durchschnitt-Parameter

### MA_PRICE_UPPER_THRESHOLD
**Beschreibung:** Verhältnis zwischen aktuellem Preis und 50-Perioden-Durchschnitt, das als signifikant überkauft gilt.  
**Werte:** Dezimalzahl größer als 1 (z.B. 1.06 = 6% über dem MA50)  
**Auswirkung:** Niedrigere Werte erzeugen häufigere Verkaufssignale basierend auf Preisabstand zu MAs.  
**Optimale Verwendung:** In weniger volatilen Märkten sollten niedrigere Werte verwendet werden, in volatileren höhere.

### MA_PRICE_LOWER_THRESHOLD
**Beschreibung:** Verhältnis zwischen aktuellem Preis und 50-Perioden-Durchschnitt, das als signifikant überverkauft gilt.  
**Werte:** Dezimalzahl kleiner als 1 (z.B. 0.94 = 6% unter dem MA50)  
**Auswirkung:** Höhere Werte erzeugen häufigere Kaufsignale basierend auf Preisabstand zu MAs.  
**Optimale Verwendung:** In weniger volatilen Märkten sollten höhere Werte verwendet werden, in volatileren niedrigere.

### MA_TREND_UPPER_THRESHOLD
**Beschreibung:** Verhältnis zwischen kürzeren und längeren gleitenden Durchschnitten, das einen starken Aufwärtstrend anzeigt.  
**Werte:** Dezimalzahl größer als 1 (z.B. 1.02 = kurzfristiger MA ist 2% über dem längerfristigen MA)  
**Auswirkung:** Niedrigere Werte erkennen Aufwärtstrends früher, höhere Werte erst bei stärkeren Trends.  
**Optimale Verwendung:** Sollte basierend auf der typischen Trendstärke des Assets angepasst werden.

### MA_TREND_LOWER_THRESHOLD
**Beschreibung:** Verhältnis zwischen kürzeren und längeren gleitenden Durchschnitten, das einen starken Abwärtstrend anzeigt.  
**Werte:** Dezimalzahl kleiner als 1 (z.B. 0.98 = kurzfristiger MA ist 2% unter dem längerfristigen MA)  
**Auswirkung:** Höhere Werte erkennen Abwärtstrends früher, niedrigere Werte erst bei stärkeren Trends.  
**Optimale Verwendung:** Sollte basierend auf der typischen Trendstärke des Assets angepasst werden.

## Indikatoren-Gewichtungen

### RSI_WEIGHT
**Beschreibung:** Gewichtung des RSI-Signals im kombinierten Gesamtsignal.  
**Werte:** Dezimalzahl zwischen 0 und 1 (Summe aller Gewichte sollte 1 ergeben)  
**Auswirkung:** Höhere Werte verstärken den Einfluss des RSI auf das Gesamtsignal.  
**Optimale Verwendung:** In seitwärts gerichteten Märkten sollte dieser Wert höher sein, da RSI dort besser funktioniert.

### MOMENTUM_WEIGHT
**Beschreibung:** Gewichtung des Momentum-Signals im kombinierten Gesamtsignal.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Preisbewegungen auf das Gesamtsignal.  
**Optimale Verwendung:** In trendenden Märkten sollte dieser Wert höher sein, da Momentum dort besser funktioniert.

### VOLUME_WEIGHT
**Beschreibung:** Gewichtung des Volumen-Signals im kombinierten Gesamtsignal.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Volumenanalysen auf das Gesamtsignal.  
**Optimale Verwendung:** Bei Assets mit starken Volumen-Preis-Korrelationen sollte dieser Wert höher sein.

### MA_WEIGHT
**Beschreibung:** Gewichtung des gleitenden-Durchschnitt-Signals im kombinierten Gesamtsignal.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Trendanalysen auf das Gesamtsignal.  
**Optimale Verwendung:** In stark trendenden Märkten sollte dieser Wert höher sein.

## Vertrauensniveau-Parameter

### MIN_CONFIDENCE
**Beschreibung:** Minimales Vertrauensniveau, das einem Signal zugewiesen wird, wenn keine starken Signale vorliegen.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.1-0.2)  
**Auswirkung:** Höhere Werte führen dazu, dass auch schwächere Signale ein gewisses Mindestvertrauen erhalten.  
**Optimale Verwendung:** Sollte niedrig sein, um zu verhindern, dass schwache Signale zu Trades führen.

### BASE_CONFIDENCE
**Beschreibung:** Basiswert für das Vertrauensniveau, bevor Anpassungen basierend auf Signalübereinstimmung vorgenommen werden.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.2-0.3)  
**Auswirkung:** Höhere Werte führen generell zu mehr Trades, da das Basisvertrauen höher ist.  
**Optimale Verwendung:** Sollte in Märkten mit klaren Trends höher sein, in unsicheren Märkten niedriger.

### SIGNAL_AGREEMENT_WEIGHT
**Beschreibung:** Gewichtungsfaktor für die Anpassung des Vertrauens basierend auf der Übereinstimmung verschiedener Signale.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.6-0.8)  
**Auswirkung:** Höhere Werte verstärken den Einfluss der Signalübereinstimmung auf das Vertrauensniveau.  
**Optimale Verwendung:** Höhere Werte führen zu mehr Vertrauen in Signale, bei denen mehrere Indikatoren übereinstimmen.

### SIGNAL_STRENGTH_WEIGHT
**Beschreibung:** Gewichtungsfaktor für die Anpassung des Vertrauens basierend auf der absoluten Stärke des Signals.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.3-0.6)  
**Auswirkung:** Höhere Werte verstärken den Einfluss der Signalstärke auf das Vertrauensniveau.  
**Optimale Verwendung:** Höhere Werte führen zu mehr Vertrauen in sehr starke Signale, selbst wenn nicht alle Indikatoren übereinstimmen.

### NEUTRAL_CONFIDENCE_FACTOR
**Beschreibung:** Faktor, mit dem das Vertrauensniveau multipliziert wird, wenn der RSI im neutralen Bereich liegt.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.6-0.8)  
**Auswirkung:** Niedrigere Werte reduzieren das Vertrauen stärker, wenn RSI im neutralen Bereich ist.  
**Optimale Verwendung:** Sollte in volatilen Märkten niedriger sein, da neutrale RSI-Werte dort weniger aussagekräftig sind.

## Volatilitäts-Parameter

### VOLATILITY_THRESHOLD
**Beschreibung:** Standardabweichung der Preisänderungen, ab der ein Markt als volatil betrachtet wird.  
**Werte:** Dezimalzahl (z.B. 1.8 = 1.8% durchschnittliche Abweichung von der mittleren Preisänderung)  
**Auswirkung:** Niedrigere Werte führen dazu, dass mehr Marktbedingungen als volatil eingestuft werden.  
**Optimale Verwendung:** Sollte an die typische Volatilität des gehandelten Assets angepasst werden.

### VOLATILITY_POSITION_MODIFIER
**Beschreibung:** Faktor, mit dem die Positionsgröße multipliziert wird, wenn der Markt als volatil gilt.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.5 = Halbierung der Positionsgröße)  
**Auswirkung:** Niedrigere Werte führen zu stärkeren Positionsgrößenreduzierungen in volatilen Märkten.  
**Optimale Verwendung:** Sollte basierend auf der Risikobereitschaft angepasst werden - konservativere Strategien nutzen niedrigere Werte.

### EXTREME_VOLATILITY_THRESHOLD
**Beschreibung:** Standardabweichung der Preisänderungen, ab der ein Markt als extrem volatil betrachtet wird.  
**Werte:** Dezimalzahl (höher als VOLATILITY_THRESHOLD)  
**Auswirkung:** Niedrigere Werte führen dazu, dass mehr Situationen als extrem volatil eingestuft werden.  
**Optimale Verwendung:** Sollte deutlich höher sein als der normale Volatilitätsschwellenwert.

### MIN_PROFIT_FOR_VOL_EXIT
**Beschreibung:** Mindestgewinn in Prozent, bevor eine Position aufgrund extremer Volatilität geschlossen wird.  
**Werte:** Dezimalzahl (z.B. 0.2 = 0.2% Gewinn)  
**Auswirkung:** Niedrigere Werte führen zu früheren Ausstiegen bei Volatilität.  
**Optimale Verwendung:** Sollte basierend auf der typischen Volatilität des Assets angepasst werden.

## Trend-Parameter

### UPTREND_THRESHOLD
**Beschreibung:** Verhältnis zwischen kurzfristigem (MA5) und mittelfristigem (MA20) Durchschnitt, das einen Aufwärtstrend anzeigt.  
**Werte:** Dezimalzahl größer als 1 (z.B. 1.01 = MA5 ist 1% über MA20)  
**Auswirkung:** Niedrigere Werte identifizieren Aufwärtstrends früher, können aber mehr Fehlsignale generieren.  
**Optimale Verwendung:** In stark trendenden Märkten sollten höhere Werte verwendet werden, um vorübergehende Rücksetzter zu ignorieren.

### DOWNTREND_THRESHOLD
**Beschreibung:** Verhältnis zwischen kurzfristigem (MA5) und mittelfristigem (MA20) Durchschnitt, das einen Abwärtstrend anzeigt.  
**Werte:** Dezimalzahl kleiner als 1 (z.B. 0.99 = MA5 ist 1% unter MA20)  
**Auswirkung:** Höhere Werte identifizieren Abwärtstrends früher, können aber mehr Fehlsignale generieren.  
**Optimale Verwendung:** In stark trendenden Märkten sollten niedrigere Werte verwendet werden, um vorübergehende Erholungen zu ignorieren.

### TREND_SIGNAL_REDUCTION
**Beschreibung:** Faktor, mit dem ein Signal multipliziert wird, wenn es gegen den aktuellen Markttrend läuft.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.5 = Signal wird um 50% reduziert)  
**Auswirkung:** Niedrigere Werte schwächen Signale stärker ab, die gegen den vorherrschenden Trend laufen.  
**Optimale Verwendung:** In stark trendenden Märkten sollten niedrigere Werte verwendet werden, um trendfremde Signale stärker zu unterdrücken.

### TREND_CONFIDENCE_REDUCTION
**Beschreibung:** Faktor, mit dem das Vertrauensniveau eines Signals multipliziert wird, wenn es gegen den aktuellen Markttrend läuft.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.7 = Confidence wird um 30% reduziert)  
**Auswirkung:** Niedrigere Werte reduzieren das Vertrauen in Signale stärker, die gegen den vorherrschenden Trend laufen.  
**Optimale Verwendung:** In stark trendenden Märkten sollten niedrigere Werte verwendet werden, um das Vertrauen in trendfremde Signale zu verringern.

### EXPANSION_TREND_STRENGTH
**Beschreibung:** Mindesttrendstärke, die erforderlich ist, um eine Marktphase als Expansion zu identifizieren.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.6-0.8)  
**Auswirkung:** Höhere Werte erfordern stärkere Aufwärtstrends, um die Expansionsphase zu erkennen.  
**Optimale Verwendung:** In volatilen Märkten sollten höhere Werte verwendet werden, um Fehlinterpretationen zu vermeiden.

### EXPANSION_RECENT_RANGE_RATIO
**Beschreibung:** Mindestanteil des Preisbereichs der letzten 20 Perioden am Gesamtpreisbereich der letzten 50 Perioden, um eine Expansionsphase zu identifizieren.  
**Werte:** Dezimalzahl zwischen 0 und 1 (typischerweise 0.5-0.7)  
**Auswirkung:** Höhere Werte erfordern eine stärkere jüngste Preisbewegung im Verhältnis zur mittelfristigen Bewegung.  
**Optimale Verwendung:** In Märkten mit langsamen Trends sollten niedrigere Werte verwendet werden.

### EXPANSION_VOLATILITY
**Beschreibung:** Mindestvolatilität (Standardabweichung der Preisänderungen), die erforderlich ist, um eine Marktphase als Expansion zu identifizieren.  
**Werte:** Dezimalzahl (z.B. 1.5 = 1.5% Standardabweichung)  
**Auswirkung:** Höhere Werte erfordern mehr Preisvolatilität, um die Expansionsphase zu erkennen.  
**Optimale Verwendung:** Sollte an die typische Volatilität des gehandelten Assets angepasst werden.

## Kühlperioden-Parameter

### COOLING_PERIOD_HOURS
**Beschreibung:** Zeit in Stunden, die nach einem Verlust gewartet wird, bevor neue Positionen eröffnet werden.  
**Werte:** Ganzzahl (z.B. 8 = 8 Stunden Wartezeit)  
**Auswirkung:** Längere Perioden reduzieren die Handelsfrequenz nach Verlusten.  
**Optimale Verwendung:** Sollte länger sein für konservativere Strategien und in volatilen Märkten.

## Positions-Management-Parameter

### MAX_UNPROFITABLE_HOLD_HOURS
**Beschreibung:** Maximale Zeit in Stunden, die eine unrentable Position gehalten wird, bevor sie geschlossen wird.  
**Werte:** Ganzzahl (z.B. 12 = 12 Stunden maximale Haltezeit für unrentable Positionen)  
**Auswirkung:** Niedrigere Werte führen zu schnelleren Ausstiegen aus unprofitablen Trades.  
**Optimale Verwendung:** Sollte kürzer sein in volatilen Märkten und für kurzfristige Strategien.

### UNPROFITABLE_LONG_THRESHOLD
**Beschreibung:** Preisschwellenwert in Relation zum Einstiegspreis, unter dem eine Long-Position als unrentabel gilt.  
**Werte:** Dezimalzahl größer als 1 (z.B. 1.002 = Preis muss 0.2% über dem Einstiegspreis sein)  
**Auswirkung:** Werte näher an 1 stellen strengere Anforderungen an die Profitabilität.  
**Optimale Verwendung:** Strengere Werte (näher an 1) für aggressivere Gewinnmitnahmen, großzügigere für geduldige Strategien.

### UNPROFITABLE_SHORT_THRESHOLD
**Beschreibung:** Preisschwellenwert in Relation zum Einstiegspreis, über dem eine Short-Position als unrentabel gilt.  
**Werte:** Dezimalzahl kleiner als 1 (z.B. 0.998 = Preis muss 0.2% unter dem Einstiegspreis sein)  
**Auswirkung:** Werte näher an 1 stellen strengere Anforderungen an die Profitabilität.  
**Optimale Verwendung:** Strengere Werte (näher an 1) für aggressivere Gewinnmitnahmen, großzügigere für geduldige Strategien.

## Trailing-Stop-Parameter

### TRAILING_STOP_ACTIVATION
**Beschreibung:** Mindestgewinn in Prozent, bevor der Trailing-Stop aktiviert wird.  
**Werte:** Dezimalzahl (z.B. 0.008 = 0.8% Gewinn)  
**Auswirkung:** Niedrigere Werte aktivieren Trailing-Stops früher.  
**Optimale Verwendung:** In volatilen Märkten sollte dieser Wert niedriger sein, um Gewinne schneller zu sichern.

### TRAILING_STOP_DISTANCE
**Beschreibung:** Abstand zwischen aktuellem Preis und Trailing-Stop-Preis.  
**Werte:** Dezimalzahl (z.B. 0.005 = 0.5% Abstand)  
**Auswirkung:** Niedrigere Werte führen zu engeren Stops, die früher ausgelöst werden können.  
**Optimale Verwendung:** Sollte an die typische Volatilität des Assets angepasst werden - enger für weniger volatile Assets.

### TRAILING_ADJUST_8H, TRAILING_ADJUST_24H, TRAILING_ADJUST_48H
**Beschreibung:** Faktoren, mit denen der Trailing-Stop-Abstand nach bestimmten Haltezeiten multipliziert wird.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.9 = 10% Reduzierung des Abstands)  
**Auswirkung:** Niedrigere Werte führen zu engeren Stops nach längeren Haltezeiten.  
**Optimale Verwendung:** Sollte basierend auf der erwarteten Haltezeit der Strategie angepasst werden.

### TRAILING_MIN_8H, TRAILING_MIN_24H, TRAILING_MIN_48H
**Beschreibung:** Minimaler Trailing-Stop-Abstand nach bestimmten Haltezeiten.  
**Werte:** Dezimalzahl (z.B. 0.006 = 0.6% minimaler Abstand)  
**Auswirkung:** Niedrigere Werte ermöglichen engere Stops nach längeren Haltezeiten.  
**Optimale Verwendung:** Sollte an die typische Volatilität des Assets angepasst werden.

## Gewinnmitnahme-Parameter

### QUICK_PROFIT_6H, QUICK_PROFIT_12H
**Beschreibung:** Gewinnschwellenwerte in Prozent für schnelle Gewinnmitnahmen innerhalb bestimmter Zeitfenster.  
**Werte:** Dezimalzahl (z.B. 1.5 = 1.5% Gewinn)  
**Auswirkung:** Niedrigere Werte führen zu früheren Gewinnmitnahmen.  
**Optimale Verwendung:** In volatilen Märkten sollten diese Werte niedriger sein, um Gewinne schneller zu sichern.

## Gewinnschutz-Parameter

### PROFIT_PROTECTION_MIN
**Beschreibung:** Mindestgewinn in Prozent, bevor der Gewinnschutz aktiviert wird.  
**Werte:** Dezimalzahl (z.B. 1.5 = 1.5% Gewinn)  
**Auswirkung:** Niedrigere Werte aktivieren den Gewinnschutz früher.  
**Optimale Verwendung:** In volatilen Märkten sollte dieser Wert niedriger sein, um Gewinne schneller zu schützen.

### PROFIT_PROTECTION_FACTOR
**Beschreibung:** Faktor, der bestimmt, wie viel Gewinnrückgang toleriert wird, bevor eine Position geschlossen wird.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.8 = Position wird geschlossen, wenn 20% des maximalen Gewinns verloren werden)  
**Auswirkung:** Höhere Werte führen zu schnelleren Ausstiegen, wenn Gewinne schrumpfen.  
**Optimale Verwendung:** In volatilen Märkten sollte dieser Wert höher sein, um Gewinne aggressiver zu schützen.

## Divergenz-Parameter

### DIVERGENCE_WEIGHT
**Beschreibung:** Gewichtung des Divergenz-Signals (Unterschied zwischen Preis- und RSI-Trend) im Gesamtsignal.  
**Werte:** Dezimalzahl zwischen 0 und 1  
**Auswirkung:** Höhere Werte verstärken den Einfluss von Divergenzen auf das Gesamtsignal.  
**Optimale Verwendung:** Sollte höher sein für Strategien, die auf Trendwenden abzielen.

## Markt-Timing-Parameter

### LOCAL_MIN_CONFIDENCE_BOOST, LOCAL_MAX_CONFIDENCE_BOOST
**Beschreibung:** Faktoren, mit denen das Vertrauensniveau multipliziert wird, wenn der aktuelle Preis ein lokales Minimum/Maximum darstellt.  
**Werte:** Dezimalzahl größer als 1 (z.B. 1.25 = 25% Erhöhung des Vertrauens)  
**Auswirkung:** Höhere Werte verstärken den Einfluss des Markt-Timings auf das Vertrauensniveau.  
**Optimale Verwendung:** Höhere Werte für Strategien, die auf präzises Timing angewiesen sind.

### NON_EXTREME_CONFIDENCE_PENALTY
**Beschreibung:** Faktor, mit dem das Vertrauensniveau multipliziert wird, wenn der aktuelle Preis kein lokales Extrem darstellt.  
**Werte:** Dezimalzahl zwischen 0 und 1 (z.B. 0.8 = 20% Reduzierung des Vertrauens)  
**Auswirkung:** Niedrigere Werte reduzieren das Vertrauen stärker, wenn das Timing nicht optimal ist.  
**Optimale Verwendung:** Niedrigere Werte für Strategien, die auf präzises Timing angewiesen sind.

## Zeitfilter-Parameter

### AVOID_TRADING_HOURS
**Beschreibung:** Liste von Stunden (UTC), in denen kein Handel stattfinden soll.  
**Werte:** Liste von Ganzzahlen zwischen 0 und 23  
**Auswirkung:** Verhindert den Handel während bestimmter Stunden.  
**Optimale Verwendung:** Kann verwendet werden, um Zeiten mit geringer Liquidität oder historisch schlechter Performance zu vermeiden.

## Verlust-Begrenzungs-Parameter

### MAX_CONSECUTIVE_LOSSES
**Beschreibung:** Maximale Anzahl aufeinanderfolgender Verluste, bevor der Handel pausiert wird.  
**Werte:** Ganzzahl (z.B. 2 = Pause nach 2 Verlusten in Folge)  
**Auswirkung:** Niedrigere Werte führen zu häufigeren Handelspausen nach Verlusten.  
**Optimale Verwendung:** Sollte basierend auf der erwarteten Gewinnrate der Strategie angepasst werden.

### CONSECUTIVE_LOSSES_COOLDOWN
**Beschreibung:** Zeit in Stunden, die nach Erreichen der maximalen Anzahl aufeinanderfolgender Verluste gewartet wird, bevor der Handel fortgesetzt wird.  
**Werte:** Ganzzahl (z.B. 24 = 24 Stunden Wartezeit)  
**Auswirkung:** Längere Perioden reduzieren die Handelsfrequenz nach aufeinanderfolgenden Verlusten.  
**Optimale Verwendung:** Sollte länger sein für konservativere Strategien und in volatilen Märkten.

---
