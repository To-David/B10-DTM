# B10-DTM Abschlussaufgabe SoSe25
## Impressum
Autor: Toni David
Herausgeber: BHT
Kontakt: toda5240@bht-berlin.de

# EP.01 | Dasymetrische Choroplethenkarte
![image](https://github.com/To-David/B10-DTM/blob/cf572afbcd849946e0471e8133b370fedbe4090b/files/25-04-03_David_%C3%9C1_Planungsr%C3%A4ume-Berlins.png)
## Ergebnis
Die fertige A3 Karte umfasst zwei eigenständige Darstellungen der Bevölkerungsdichte in Berlins Planungsräumen:
* Einfache Choroplethenkarte – Hier wird die Bevölkerungszahl jedes Planungsraums auf dessen Gesamtfläche bezogen. Die Dichte wurde in QGIS aus dem Ausdruck EW / ($area / 10 000) berechnet und anschließend mit dem Klassifikationsmodus Natürliche Unterbrechungen (Jenks) in fünf Farbstufen visualisiert.
* Dasymetrische Choroplethenkarte – Zunächst wurde das Corine Land Cover Shapefile gefiltert, sodass nur die urbanen Klassen 111 und 112 (Urban Fabric) übrigblieben. Nach dem Auflösen dieser Polygone zu einem einzigen bewohnten Flächenlayer wurde er mit den Planungsräumen verschnitten. Alle nicht bebauten Teilflächen erscheinen grau und gehen nicht in die Flächen- oder Dichteberechnung ein. Dadurch liegen die Dichtewerte deutlich höher als in der einfachen Karte.
## Arbeitsschritte
1.	Datenbeschaffung – Die Shapefile der Planungsräume Berlins (LOR 2021) sowie die Einwohnerzahlen (CSV, Stand 1/2023) wurden von der Berliner Senatsverwaltung heruntergeladen.
2.	Aufbereitung der CSV – Überflüssige Kopfzeilen und Spalten wurden in Excel entfernt; 1000er Trennpunkte deaktiviert; beim Import nach QGIS erhielten sowohl PLR Nummer als auch Einwohnerzahl den Datentyp Ganzzahl.
3.	Join – Mit „Attribute nach Feldwert verbinden“ wurden die Einwohnerzahlen an die PLR Geometrien angefügt. Damit war der Layer für die einfache Choroplethenkarte vollständig.
4.	Klassifizierung & Layout – In den Layereigenschaften wurde EW / ($area / 10 000) als Ausdruck hinterlegt, Jenks Klassen gewählt und Farben angepasst; Anzahl, Grenzwerte und Legende wurden manuell verfeinert.
5.	Dasymetrische Schritte:
* Filtern der Corine Daten auf Code_18 = 111 OR 112 mittels „Nach Ausdruck extrahieren“.
* Auflösen zu einer einzigen Urban Fabric Fläche.
* Verschneidung dieser Fläche mit dem mit Einwohnerzahlen angereicherten PLR Layer, sodass jeder bebaute Polygonteil seine Einwohnerzahl erbte.
* Neuberechnung von Fläche und Dichte in einer neuen Spalte; zwei Flächenschnipsel mit extrem kleinen Polygonen und unrealistisch hohen Dichtewerten wurden gelöscht, um Ausreißer zu vermeiden (Schritt aus Praktikum).
* Symbolisierung: unbewohnte Teile grau, bewohnte Teile als abgestufte Choroplethenkarte; Stil des einfachen Layers wurde kopiert und neu klassifiziert.
## Vorteile der Methode
* Realistischere Kennzahlen – Dichte wird nur auf bebaute Flächen bezogen, unbewohnte Areale verzerren das Ergebnis nicht.
* Klarere räumliche Kontraste – Hot‑Spots hoher oder niedriger Dichte treten deutlicher hervor, was die Interpretation erleichtert.
* Bessere Planungsgrundlage – Präzisere Werte unterstützen Entscheidungen zu Infrastruktur, Freiraum‑ und Wohnraumbedarf.
* Verwaltungseinheiten bleiben erhalten – Ergebnisse sind weiterhin Planungsraum‑scharf und lassen sich politisch leicht kommunizieren.
## Nachteile der Methode
* Datenabhängigkeit – Das Ergebnis ist so zuverlässig wie die Aktualität und Auflösung der Corine Land Cover Daten; Diskrepanzen an den Schnittstellen der Planungsräume können zu Ausreißern führen, die manuell bereinigt werden sollten (hier am Beispiel des Maximalwertes zu sehen, welcher bewusst beibehalten wurde).
* Annahme „keine Bewohner außerhalb Urban Fabric“ – In Großstädten kaum problematisch, in suburbanen oder ländlichen Kontexten aber eine zu grobe Vereinfachung.
* Höherer Aufwand – Filter , Auflöse , Verschneidungs  und Join Schritte kosten Zeit und Rechenleistung; jede Aktualisierung der Einwohnerzahlen erfordert ein erneutes Durchlaufen des Workflows.

# EP.02 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-10_David_%C3%9C2_Kirschbaeume-Berlins.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.03 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-17_David_%C3%9C3_AirBnB-Berlin.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.04 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-24_David_%C3%9C4_Bundestagswahl_Zweitstimmen_AfD.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.05 | 
![image](***)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.06 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-06-09_David_%C3%9C6_Syria-refugees.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.07 | 
[Gif-Link](https://cloud.acarin.de/s/DTM_EP07_Meshdaten)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.08 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-06-26_David_%C3%9C8_meteor-shower_quadrantids.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


# EP.09 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-06-26_David_%C3%9C9_2.5d-stadtmodelle_dresen.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode
