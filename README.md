# Data Engineering & ETL Pipeline: PV-Anlagen Validierung (ZEE)

Ein automatisiertes Data-Engineering-Projekt zur Extraktion, Bereinigung, relationalen Modellierung und Validierung von Leistungsdaten verschiedener Photovoltaik-Technologien (PERC, HIT, CIGS). Inspiriert von realen Validierungsprozessen im Forschungszentrum für Erneuerbare Energien (CER / ZEE).

## 📊 Projektübersicht

Dieses Projekt demonstriert eine vollständige **ETL-Pipeline (Extract, Transform, Load)**, die in einem einzigen interaktiven **Jupyter Notebook** unifiziert ist. Ziel des Projekts ist es, fehlerhafte Zeitreihen-Rohdaten von PV-Anlagen automatisiert zu bereinigen, statistische Imputationen durchzuführen und die Daten kontrolliert in ein relationales **PostgreSQL-Datenbankmodell** zu überführen.

### Kernfeatures:
* **Extraction & Simulation:** Generierung von realistischen 15-Minuten-Intervall-Rohdaten für drei führende Solartechnologien.
* **Data Transformation (Pandas & NumPy):** Fortgeschrittene Bereinigung und Imputation von fehlenden Leistungswerten (`NaN`) basierend auf technologischen Clustern sowie präzise Performance-Modellierung (Festlegung der *Performance Ratio* für HIT auf exakt **82%**).
* **Relationales Datenbank-Schema (SQL):** Entwurf einer normalisierten Struktur in PostgreSQL zur Gewährleistung der Datenintegrität.
* **Automated Bulk Load (psycopg2):** Effiziente Übertragung der prozessierten In-Memory-Daten direkt in die lokale PostgreSQL-Instanz.
* **Data Validation (SQL-Abfragen):** Automatisierte Validierung der KPIs direkt aus der SQL-Datenbank zur Qualitätssicherung.

---

## 🛠️ Technologie-Stack

* **Programmiersprache:** Python 3.12
* **Bibliotheken & Frameworks:** Pandas, NumPy, Psycopg2 (Massenimport)
* **Datenbanksystem:** PostgreSQL / pgAdmin 4
* **Entwicklungsumgebung:** Jupyter Notebook

---

## 📐 Relationales Datenbankmodell (SQL)

Die Pipeline transformiert flache Tabellenstrukturen in ein sauberes, relationales Schema, um Datenredundanzen zu minimieren:

```sql
-- 1. Tabelle für die Solartechnologien (Standardisierung)
CREATE TABLE technologien (
    technologie_id SERIAL PRIMARY KEY,
    name VARCHAR(20) NOT NULL UNIQUE,
    beschreibung TEXT
);

-- 2. Tabelle für historische Messdaten (Optimierte relationale Struktur)
CREATE TABLE messwerte_anlagen (
    messung_id SERIAL PRIMARY KEY,
    zeitstempel TIMESTAMP NOT NULL,
    technologie_id INT REFERENCES technologien(technologie_id),
    einstrahlung_wm2 NUMERIC(6,2),
    leistung_kw NUMERIC(5,2),
    performance_ratio NUMERIC(4,3)
);

🚀 Pipeline-Architektur & Workflow
Das Projekt ist im Notebook Data_Engineering_PV_Anlagen.ipynb in vier logische Phasen unterteilt:

Datenbereinigung & Imputation (Transform): Rohdaten enthalten häufig Lücken durch Sensorfehler oder nächtliche Abschaltungen (Einstrahlung < 250 W/m²). Fehlwerte in leistung_kw werden nicht gelöscht, sondern präzise über den technologischen Mittelwert imputiert, um statistische Verzerrungen zu vermeiden.

Performance-Modellierung: Implementierung mathematischer Schwellenwerte mithilfe von numpy.where(). Die hocheffiziente HIT-Technologie wird präzise auf ein Qualitäts-KPI von 0.820 (82% Performance Ratio) kalibriert.

Automatisierter Daten-Load via Python: Die Verbindung zur Datenbank erfolgt über psycopg2. Die transformierten Pandas-DataFrames werden performant mittels extras.execute_batch() in einem einzigen Block (Bulk Insert) in PostgreSQL hochgeladen.

KPI-Validierung: Ein direkter SQL-Join aggregiert die Daten direkt auf dem Datenbankserver und validiert die korrekte Modellausführung.

📈 Validierungsergebnisse (Beispiel-Ausgabe)
Nach erfolgreichem Durchlauf della Pipeline liefert die integrierte SQL-Validierungszelle folgendes verifiziertes Ergebnis direkt aus PostgreSQL:

technologie,durchschnittliche_pr,mittlere_leistung_kw
CIGS,0.749,26.63
HIT,0.820,26.21
PERC,0.745,31.58

Hinweis: Das Ergebnis bestätigt die fehlerfreie Validierung des HIT-Performance-Ratios von exakt 82% (0.820).

📂 Repository-Struktur
Bash
├── Data_Engineering_PV_Anlagen.ipynb  # Haupt-Notebook mit dem gesamten ETL- & SQL-Code
└── README.md                           # Projektdokumentation (Deutsch)

🔧 Installation & Ausführung
Voraussetzungen
Stellen Sie sicher, dass PostgreSQL und eine Python-Umgebung (z.B. Anaconda) installiert sind. Erstellen Sie eine Datenbank namens zee_solardaten_db in pgAdmin vor der Ausführung.

1.- Repository klonen oder Notebook herunterladen:

Bash
git clone [https://github.com/DEIN_USER/DEIN_REPOSITORIO.git](https://github.com/DE

2.- Erforderliche Python-Bibliotheken installieren:

Bash
pip install pandas numpy psycopg2-binary

3.- Jupyter Notebook starten:

Bash
jupyter Notebook

4.- Notebook ausführen:
Öffnen Sie Data_Engineering_PV_Anlagen.ipynb, tragen Sie Ihr pgAdmin-Passwort ein und führen Sie alle Zellen aus.