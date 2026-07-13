# Master Thesis Project Plan: Event-Driven MLOps for HPFC Forecasting

Dieses Dokument fasst die strategische Ausrichtung, die softwareseitige Architektur und das theoretische Fundament der geplanten Diplomarbeit im Studiengang Angewandte Informatik an der AAU Klagenfurt zusammen.

---

## 1. Metadaten & Titelstruktur

* **Voraussichtlicher englischer Arbeitstitel:** `An Event-Driven MLOps Architecture for Hourly Price Forward Curve Forecasting: Benchmarking Supply Function Equilibria against Machine Learning`
* **Fokus:** 50% High-Level Software-Engineering & Data Pipelines / 50% Angewandte Wirtschaftstheorie, mathematische Optimierung und Zeitreihenprognose.
* **Vorteil des Setups:** Volle akademische Unabhängigkeit durch die exklusive Verwendung von Open-Source-Software und öffentlich zugänglichen Marktdaten (z. B. ENTSO-E, EXAA, EPEX Spot).

---

## 2. Der inhaltliche rote Faden (Theorie & Experiment)

Die Arbeit bricht mit dem klassischen „Entweder-oder“ aus reiner Wirtschaftstheorie und reiner Datenanalyse. Sie implementiert stattdessen ein kontrolliertes, empirisches Benchmarking (*Race*) zwischen strukturellen Marktmodellen und modernen Machine-Learning-Algorithmen, um eine Hourly Price Forward Curve (HPFC) zu generieren.

### Der theoretische Brückenbau (Wirtschafts- & Markttheorie)
1. **Das Fundament (Cournot):** Herleitung des klassischen Mengenwettbewerbs und des Nash-Gleichgewichts als Ausgangspunkt für strategisches Verhalten.
2. **Die fundamentale Kritik:** Aufzeigen, warum Cournot an modernen Strombörsen unvollständig ist (Händler bieten keine fixen Mengen, sondern Preis-Mengen-Funktionen).
3. **Die Evolution (Supply Function Equilibrium - SFE):** Einführung des SFE-Modells als mathematisch realistische Abbildung von Gebotskurven.
4. **Die Disruption (Der PV- und Speicher-Twist):** Erweiterung des klassischen SFE um hochaktuelle Phänomene des Jahres 2026:
   * Einpreisung von unelastischem Angebot mit Grenzkosten von Null (solare Preiskannibalisierung / Negativpreisphasen).
   * Integration von zeitgekoppelten Großspeichern (Pumpspeicher/Batterien) als rein opportunistische Arbitrage-Akteure.

### Das experimentelle Benchmark (Data Science)
Das modifizierte, mathematische SFE-Modell tritt im Backtesting gegen zwei reine Machine-Learning-Architekturen an:
* **Modell A (Tree-Based):** LightGBM oder XGBoost für die tabellarische Mustererkennung.
* **Modell B (Deep Learning):** LSTM oder Transformer-basierte Netze für zeitliche Sequenzabhängigkeiten.
* **Die Synthese (Hybrid):** Untersuchung, ob die Nutzung des SFE-Gleichgewichtspreises als *Feature* für die ML-Modelle die geringsten Fehlermaße (RMSE, MAPE) erzielt.

---

## 3. Technologische Enterprise-Architektur (Informatik-Kern)

Um dem Anspruch der Angewandten Informatik gerecht zu werden, wird das System als **Event-Driven Machine Learning Architecture** über Microservices realisiert. Die gesamte Infrastruktur ist **kostenlos (Open Source)** und läuft lokal über Docker.