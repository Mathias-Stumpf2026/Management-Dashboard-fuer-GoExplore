# Management-Dashboard-fuer-GoExplore
# GoExplore Markterschließung — Umsatzprognose neuer Länder mittels BIP-Skalierung (BigQuery & Looker Studio)
<img width="861" height="435" alt="image" src="https://github.com/user-attachments/assets/c3523dd8-65fa-4686-9a5b-e7030bb8f9b0" />

[Finance/CEO Dashboard]([Images 4/finance_ceo_dashboard.png](https://github.com/Mathias-Stumpf2026/Management-Dashboard-fuer-GoExplore/blob/main/Images%204/finance_ceo_dashboard.png))

Management-Dashboard für **GoExplore** (internationaler Einzelhändler für Camping-, Golf- und Outdoor-Ausrüstung) zur zentralen Frage der Geschäftsleitung: **Lohnt sich der Markteintritt in Polen, Norwegen, Tschechien und Portugal — und wie stellen wir sicher, dass CEO und Fachbereiche das jederzeit selbst nachvollziehen können, ohne für jede Frage eine neue Excel-Auswertung zu brauchen?**

📊 ![Präsentation]([Projekt/GoExplore_Presentation.pptx](https://github.com/Mathias-Stumpf2026/Management-Dashboard-fuer-GoExplore/blob/main/Projekt/GoExplore_Praesentation.pptx) · 📄 [Pflichtenheft](4.Pflichtenheft/) · 🧩 [SQL-Views](3.SQL/) · 🖥️ [Interaktives Mockup](6.Mockup/)

Dieses Projekt entstand als Gruppenarbeit (Team: Zubaida, Tim, Mathias) im Rahmen des WBS-Kurses Data Science & AI; dieses Repository dokumentiert die gemeinsame Ausarbeitung.

## Projektübersicht

GoExplore plant den Markteintritt in vier neue europäische Länder und braucht dafür belastbare Umsatzprognosen statt Bauchgefühl. Mit einem BIP-skalierten Benchmark-Modell (je Zielland ein Vergleichsland, Umsatz × BIP-pro-Kopf-Verhältnis × gestaffeltes 3-Jahres-Ziel) haben wir in BigQuery 24 Views gebaut und daraus ein rollenbasiertes Looker-Studio-Dashboard mit 5 Fachbereichsseiten entwickelt, das CEO, Marketing, Retailer Connections und Administration jeweils genau die Kennzahlen zeigt, die ihre Kernfragen beantworten.

## Datensatz & Quellen

- **Quelle:** vier interne GoExplore-Datensätze (`retailers`, `products`, `daily_sales`, `methods`), bereitgestellt im Rahmen des Kurses, unbereinigt
- **Größe:** 149.257 Verkaufszeilen, Zeitraum 12.01.2015–20.07.2018 (~3,5 Jahre), 289 aktive Händler in 21 Ländern, 144 Produkte in 5 Produktlinien
- **Schlüsselmerkmale:** Retailer code/type/country, Product number/line/type, Order method, Quantity, Unit price, Unit sale price
- **Datenbereinigung geprüft, bewusst nicht angewendet:** 802 Duplikate (0,09 Prozentpunkte Effekt auf den Nettoumsatz) und 524 Nullpreis-Zeilen (0,14 Prozentpunkte Effekt auf die Rabatt-Quote) — Effekt war marginal, Entscheidung inkl. Begründung im Pflichtenheft dokumentiert
- **Bekannte Datenlücke:** keine Personal-/Headcount-Daten, keine Kreditzins- oder Kundenzufriedenheits-/NPS-Daten vorhanden

## Wichtige Erkenntnisse & Ergebnisse

- **Rabattpolitik ist nicht einheitlich:** Die Rabatt-Quote liegt gesamt bei 5,04 %, schwankt aber je Händlertyp stark zwischen 0,99 % (Direct Marketing) und 6,91 % (Golf Shop) — eine pauschale Rabattregel würde die profitabelsten Kanäle unnötig verwässern.
- **Fachgeschäfte sind trotz höherer Rabatte wertvoller:** Fachgeschäfte (Golf-/Brillengeschäfte) haben eine höhere Rabatt-Quote (6,0 % vs. 4,8 %), erzielen aber einen um rund 30 % höheren Warenkorbwert (10.350 € vs. 7.960 €) und mehr Umsatz je Händler als allgemeine Geschäfte.
- **Web dominiert den Vertrieb:** 83,2 % des Nettoumsatzes laufen über den Web-Kanal — Marketing-Budget und Kampagnenplanung sollten sich daran orientieren, nicht an den Nebenkanälen.
- **Markterschließung mit klarer Prognose:** Das BIP-skalierte 3-Jahres-Modell zeigt für Polen das größte Potenzial (13,3 Mio. € Zielumsatz in Jahr 3), gefolgt von Norwegen (9,0 Mio. €); die Zielerreichung liegt aktuell erwartungsgemäß bei 0 %, da der reale Markteintritt in den vier Ländern noch nicht erfolgt ist.
- **Vollständige GuV noch nicht möglich:** Rohertrag (527,7 Mio. €, 42,2 % Marge) lässt sich sauber berechnen, aber ohne Personal-, Miet- und Zinskosten sind Rohertrag und EBIT im Modell rechnerisch identisch — eine dokumentierte Vereinfachung, keine vollständige Gewinn- und Verlustrechnung.

## Verwendete Technologien

- **Data Warehouse:** Google BigQuery (24 Views)
- **Abfragesprache:** SQL (BigQuery Standard SQL)
- **Visualisierung:** Looker Studio, 5 rollenbasierte Fachbereichsseiten (Grundseite, Finance/CEO, Marketing, Retailer Connections, Administration)
- **Konzept & Prototyping:** HTML/CSS/JS-Mockup mit Rollen- und Sprachumschalter (DE/EN)
- **Dokumentation:** Pflichtenheft (Word) inkl. SQL-Anlage mit kommentierten Abfragen

## Wichtige Erkenntnisse in Bildern

![Markterschließung: BIP-skalierte 3-Jahres-Prognose](2.Images/markterschliessung_prognose.png)
*Prognostizierter Zielumsatz je Land für die Jahre 1–3 (30/60/100 % der Lücke zum Benchmark-Land) — aktueller Ist-Umsatz noch bei 0, da der Markteintritt noch nicht erfolgt ist.*

![Retailer Connections: Fachgeschäfte vs. allgemeine Geschäfte](2.Images/retailer_connections.png)
*Top-5-Händler, Umsatz nach Land und Rabatt-/Warenkorbwert je Händlertyp — Basis für die Fachgeschäft-vs-allgemein-Analyse.*

## Repository-Struktur

```
README.md         # diese Datei (Root-Ebene)
1.Data/            # Original-CSV (retailers, products, daily_sales, methods) bzw. Download-Link
2.Images/          # Dashboard- und Mockup-Screenshots für README & Präsentation
3.SQL/             # alle 24 BigQuery-Views, Startpunkt: v_filter_basis
4.Pflichtenheft/   # Pflichtenheft inkl. SQL-Anlage, Formeln, Entscheidungen, KPI-Definitionen
5.Praesentation/   # PowerPoint für die Dashboard-Demo
6.Mockup/          # interaktive HTML-Mockups (V1 Umsetzungsstand, V2 Ausblick "Business Operations")
```

## Wie man dieses Projekt nutzt

1. **Konzept verstehen:** zuerst `6.Mockup/` öffnen (klickbarer Prototyp, zeigt Seitenstruktur und Rollen-/Berechtigungsmodell)
2. **Formeln & Entscheidungen:** `4.Pflichtenheft/` — dort stehen alle KPI-Definitionen, die Markterschließungs-Formel und die Begründung für die Datenbereinigungs-Entscheidung
3. **SQL nachvollziehen:** `3.SQL/` — jede View liegt zweimal vor: einmal ohne Kommentare (copy-paste-fertig), einmal zeilenweise mit `#`-Kommentaren erklärt
4. **Dashboard:** Zugriff auf das Live-Dashboard in Looker Studio nur mit Berechtigung — Screenshots in `2.Images/` und in der Präsentation
5. Keine Einrichtung nötig außer BigQuery-Zugriff auf die vier Rohdaten-Tabellen

## Zukünftige Arbeiten

- **Business-Operations-Seite:** Rohertrag/EBIT, Gewinnmarge, Aufschlagsfaktor, Umsatzeffizienz als eigene Dashboard-Seite (Mockup V2 bereits vorbereitet)
- **API-/ERP-Anbindung:** automatischer Datenimport statt manuellem CSV-Upload
- **Kundenzufriedenheitsprogramm:** Anbindung für NPS-Daten, aktuell keine Datenquelle vorhanden
- **Rollenbasierte Zugriffssteuerung:** im Mockup konzeptionell definiert, in Looker Studio noch nicht produktiv umgesetzt
- **Weitere Abteilungsseiten** (z. B. Vertrieb) mit eigener Unterseiten-Logik (Kurzüberblick + Detail-Drilldown, Anbindung an Quellsysteme)

## 📧 Kontakt

- **Team:** Zubaida, Tim, Mathias — WBS Coding School, Data Science & AI Kurs

---

## English Summary

This project builds a management dashboard for **GoExplore**, an international retailer of camping, golf, and outdoor equipment, to answer the executive board's central question: is entering four new markets (Poland, Norway, Czech Republic, Portugal) worthwhile, and can the CEO and department heads track this themselves without a new spreadsheet for every question?

**Key findings**, based on 149,257 sales records (Jan 2015–Jul 2018) across 289 retailers in 21 countries:
- Discount policy varies sharply by retailer type (0.99%–6.91%) — a flat discount rule would hurt the most profitable channels.
- Specialty stores discount more (6.0% vs. 4.8%) but generate ~30% higher average order value (€10,350 vs. €7,960) than general stores.
- The web channel drives 83.2% of net revenue — marketing spend should follow this concentration.
- A GDP-scaled 3-year forecast model projects the largest opportunity in Poland (€13.3M year-3 target), followed by Norway (€9.0M); actual achievement is currently 0% since market entry has not yet started.
- A full P&L isn't possible without payroll, rent, and interest cost data — gross profit and EBIT are numerically identical in this simplified model, a documented assumption rather than a complete income statement.

Full documentation: see the German sections above, the [Pflichtenheft](4.Pflichtenheft/), and the [interactive mockup](6.Mockup/).
