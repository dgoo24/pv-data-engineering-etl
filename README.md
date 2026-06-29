# Data Engineering & ETL-Pipeline: PV-Anlagen Langzeit-Degradationsanalyse

Automatisierte Python-Pipeline zur Extraktion, Bereinigung (Data Cleaning) und mathematischen Modellierung historischer Leistungsdaten von Photovoltaikanlagen (Zeitraum 2020–2024). Inspiriert von den Forschungsarbeiten am Zentrum für Erneuerbare Energien (CER).

## 📊 Projektübersicht

Dieses Projekt simuliert und verarbeitet die hochfrequenten Minuten-Messdaten eines Solarparks, um die Degradation verschiedener PV-Technologien (**PERC, HIT, CIGS**) unter realen Klimabedingungen zu vergleichen. Es demonstriert einen vollständigen **ETL-Prozess (Extract, Transform, Load)**, der fehlerhafte Sensordaten automatisch bereinigt und geschäftskritische KPIs berechnet.

### Kernfeatures:
* **Data Engineering & Cleaning (Pandas):** Automatisierte Filterung von extremen Ausreißern und fehlerhaften Sensorwerten (Werte unter Null).
* **Daten-Imputation:** Lineare Interpolation (`.interpolate(method='linear')`) zur sauberen Behandlung von fehlenden Messwerten (NaNs), um Verzerrungen in der Langzeitanalyse zu verhindern.
* **Performance-Modellierung (KPIs):** Automatisierte Berechnung der *Performance Ratio (PR)* zum Soll-Ist-Abgleich der Energieerträge.
* **Wichtigste Erkenntnis:** Die **HIT-Technologie (Heterojunction)** wurde mit einer **Performance Ratio von ca. 82%** als die effizienteste und stabilste Option identifiziert.

---

## 🛠️ Technologie-Stack

* **Programmiersprache:** Python 3.12
* **Bibliotheken:** Pandas (ETL & Datenmanipulation), NumPy (Numerische Arrays), Matplotlib (Visual Analytics)
* **Datenbank-Integration:** Konzeptioniert für relationale SQL-Strukturen (PostgreSQL/MySQL) für historische Großdaten.

---

## 📈 Visual Analytics (Output)

Die Pipeline aggregiert die bereinigten Tagesdaten automatisch zu Monatszeitreihen und generiert einen aussagekräftigen Report (`pv_degradation_analysis.png`), der den exakten Degradationstrend über die 5 Jahre fehlerfrei visualisiert.

---

## 📂 Repository-Struktur

```bash
├── optical_pipeline.ipynb          # Alternativer Pipeline-Pfad (falls zutreffend)
├── pv_data_pipeline.ipynb          # Haupt-Jupyter-Notebook mit der ETL-Pipeline
├── pv_degradation_analysis.png    # Automatisierte Grafik der Langzeitanalyse
└── README.md                       # Projektdokumentation (Deutsch)

🔧 Installation & Ausführung
1.- Erforderliche Bibliotheken installieren:
Bash
pip install pandas numpy matplotlib

2.- Pipeline ausführen:
Starten Sie Jupyter Notebook und führen Sie alle Zellen der Datei pv_data_pipeline.ipynb aus.
