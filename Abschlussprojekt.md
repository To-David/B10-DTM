# B10-DTM Abschlussaufgabe SoSe25
## Impressum
Autor: Toni David  
Herausgeber: BHT  
Kontakt: toda5240@bht-berlin.de  

<br><br><br>
# Inhaltsverzeichnis
[EP.01 Dasymetrische Choroplethenkarte](#EP.01)<br>
[EP.02 Gitterchoroplethenkarte](#EP.02)<br>
[EP.03 Punktrasterkarte](#EP.03)<br>
[EP.04 Value-by-alpha Mapping](#EP.04)<br>
[EP.05 Tilemaps](#EP.05)<br>
[EP.06 Flowmaps](#EP.06)<br>
[EP.07 Mesh-Daten](#EP.07)<br>
[EP.08 Animationen in QGIS](#EP.08)<br>
[EP.09 3D-Gebäudemodelle](#EP.09)<br>

<br><br>
<a id="EP.01"></a>
<br>
# EP.01 | Dasymetrische Choroplethenkarte (Bevölkerungsdichte Berlins)
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
* Höherer Aufwand – Filter- , Auflöse- , Verschneidungs-  und Join-Schritte kosten Zeit und Rechenleistung; jede Aktualisierung der Einwohnerzahlen erfordert ein erneutes Durchlaufen des Workflows.

<br><br>
<a id="EP.02"></a>
<br>
# EP.02 | Gitterchoroplethenkarte (Kirschbäume Berlins)
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-10_David_%C3%9C2_Kirschbaeume-Berlins.png)
## Ergebnis
Die Hexagon Choroplethenkarte (Seitenlänge = 500 m) zeigt deutlich, dass Kirschbäume in Berlin nicht gleichmäßig verteilt sind. Vor allem im Osten und Südosten Köpenicks und Teilen des Westens Berlins sind kaum oder keine Kirschbäume aufzufinden. Eine solche Analyse ist nur mittels geometrischer Auswertemethoden (z.B. wie hier mittels Hexagone) möglich.
## Arbeitsschritte
1.	Datenbeschaffung – Bezirksgrenzen und Baumbestand als WFS-Layer aus dem Geoportal Berlin einfügen
2.	Erstellen des Hexagongitters – Menü Vektor → Recherchewerkzeuge → Gitter erzeugen
* Typ = „Hexagon (Polygone)“, Ausdehnung = Bezirksgrenzen Layer
* Seitenlänge 500 m → kurzer Diagonal¬abstand ≈ 866,025 m als horizontale & vertikale Schrittweite eingetragen. 
3.	Aggregation (räumlicher Join) – Verarbeitung → Werkzeugkiste → „Attribute nach Position verknüpfen (Zusammenfassung)“
* Ziel Layer = Hexagon Grid, Join Layer = Kirschbaumpunkte
* Operation = Summe der Baumanzahl pro Zelle
4.	Symbolisierung
* Layer Eigenschaften → Symbolisierung → „Abgestuft“
* Spalte = Baumanzahl, Klassifizierung nach Jenks, 6 Farben von Grau (0 Bäume) bis Dunkel¬pink (462 Bäume)
## Vorteile der Methode
* Unabhängig von Verwaltungsgrenzen – räumliche Muster werden nicht durch Bezirks- oder Ortsteilgrenzen verzerrt
* Gleich große Flächeneinheiten erleichtern statistische Vergleiche (Dichte ↔ Anzahl)
* Lücken sichtbar – Zellen ohne Punkte (grau) heben baumfreie Räume sofort hervor
* Flexibel skalierbar – durch Ändern der Zellgröße lässt sich der Detailgrad anpassen
## Nachteile der Methode
* Wahl der Zellgröße beeinflusst das Ergebnis – zu grob ⇒ Details gehen verloren, zu fein ⇒ Karte wird unruhig
* Künstliche Zellgrenzen können reale räumliche Strukturen zerschneiden oder Hot Spots „aufspalten“
* Weniger vertraut für Laien als bekannte Bezirkskarten; erfordert Erläuterung im Bericht

<br><br>
<a id="EP.03"></a>
<br>
# EP.03 | Punktrasterkarten (AirBnB Berlin)
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-17_David_%C3%9C3_AirBnB-Berlin.png)
## Ergebnis
Die Punkt-Rasterkarte zeigt, dass sich AirBnB-Angebote in Berlin stark auf die innerstädtischen Bezirke konzentrieren. In den 2x2 km-Zellen von Mitte, Kreuzberg, Friedrichshain und Prenzlauer Berg treten sowohl die höchsten Angebotszahlen (bis > 350 Wohnungen/Häuser pro Raster) als auch die höchsten Durchschnittspreise (> 200 EUR/Nacht) auf. In den Außenbezirken nimmt die Zahl der Inserate deutlich ab, und die Preise liegen überwiegend unter 100 EUR/Nacht. Hotelzimmer häufen sich erwartungsgemäß im touristischen Zentrum innerhalb der S-Bahn-Ringbahn, während geteilte und private Zimmer häufiger in angrenzenden Wohngebieten erscheinen. Insgesamt erlaubt das gleichmäßige Raster, Hot-Spots schnell zu erkennen.
## Arbeitsschritte
1.	Datenbeschaffung – InsideAirBnB-Portal -> listings.csv (Quelldaten aufbereiten -> auf Extremwerte achten, NULL-Werte entfernen)
2.	Datenimport in QGIS – CSV als Punktgeometrie importieren
3.	Attributaufbereitung und Join – Preis (price) und Objekt-ID zusammenfassen; Anzahl und Durchschnitt pro Typ berechnen.
4.	Rastererstellung – 2x2 km Grid über das Stadtgebiet generieren; Spatial Join -> Zuteilung jeder Anzeige zur entsprechenden Rasterzelle
5.	Kategorisierung – Vier Objektarten: Wohnungen/Häuser, Geteilte Zimmer, Private Zimmer, Hotelzimmer
6.	Symbolisierung – zentrierte Füllung für jede Objektart
* Punktgrösse = Durchschnittspreis pro Nacht
* Farbe = Anzahl der Objekte je Kategorie
* Offset = Objektart
## Vorteile der Methode
* Vergleichbarkeit – Jede Rasterzelle besitzt identische Fläche - statistische Kennzahlen (Dichte, Mittelwerte) sind direkt vergleichbar und nicht von Verwaltungsgränzen abhängig
* Flexibilität der Zellengrösse – Raster lässt sich je nach Untersuchungsziel (z. B. 500 m, 1 km, 2 km) anpassen.
* Visualisierung – Gleichmässiges Raster erzeugt ein regelmässig strukturiertes Kartenbild, das Hot-Spots sofort erkennbar macht.
## Nachteile der Methode
* Verlust lokaler Details – Innerhalb einer 2 km-Zelle können starke Unterschiede verborgen bleiben (z. B. Kiezzentren vs. Nebenstrassen); eine bessere Auflösung war bei dem geforderten A3 Format nicht möglich
* Interpretationshürde – Nicht-fachliche Leserinnen und Leser sind an Bezirksgrenzen gewöhnt; Rasterzellen sind weniger intuitiv verortbar.
* Parameter-Abhängigkeit – Ergebnisse hängen stark von Rastergrösse und -ausrichtung ab; andere Gittergrössen können andere Muster suggerieren.
* Symbolüberladung – Mehrere Variablen (Grösse + Farbe + Kategorie) in dicht bebauten Gebieten führen leicht zu visuellem Clutter und erschweren das Kartenlesen.

<br><br>
<a id="EP.04"></a>
<br>
# EP.04 | Alpha-by-Value ( Bundestagswahl 2021/25)
![image](https://github.com/To-David/B10-DTM/blob/9d39f689f44bff8a86cf113991f89a6287f4e36b/files/25-04-24_David_%C3%9C4_Bundestagswahl_Zweitstimmen.png)


## Ergebnis
Die Auswertung liefert je Wahljahr eine thematische Karte, in der jeder Wahlkreis in der Parteifarbe der siegreichen Zweitstimme eingefärbt ist. Die Deckkraft der Fläche steigt proportional zum Stimmenanteil der Gewinnerpartei. So werden auf einen Blick sowohl der Wahlsieger als auch die Stärke seines Vorsprungs sichtbar, was den Vergleich zwischen 2021 und 2025 erleichtert. 
Anmerkung: Beim Export wurden die Farben verzerrt. Besonders deutlich wird dies beim Rot der SPD.
<br><br>
Eine Weitere Karte wurde für den Zuwachs der Afd zwischen 2021 und 2025 erstellt: 
[Zuwachs der AfD](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-04-24_David_%C3%9C4_Bundestagswahl_Zweitstimmen_AfD.png)
## Arbeitsschritte
1. Datenbeschaffung – Wahlkreis Shapefile sowie finale Zweitstimmenergebnisse 2025 und 2021 von der Bundeswahlleiterin herunterladen. 
2. Datenaufbereitung – In einer Tabellenkalkulation alle irrelevanten Spalten/Zeilen löschen, nur Zweitstimmen der Parteien SPD, CDU/CSU, Grüne, Linke und AfD behalten, CSU Werte in Bayern zur CDU addieren, anschließend als CSV speichern. 
3. Import – Shapefile und CSV in QGIS laden; Wahlkreisnummer als Integer definieren und korrektes Trennzeichen wählen.
4. Verknüpfung – CSV mit Geometrien per Wahlkreisnummer („WKR_NR“) joinen.
5. Attributberechnung – Neue Felder erzeugen
* g_value (Stimmenzahl der Gewinnerpartei) array_max( array( "SPD" , "CDU" , "Gruenen" , "AFD" , "Linke"))
* g_name (Name der Gewinnerpartei - ) with_variable( 'maxVal', array_max( array( "SPD" , "CDU" , "Gruenen" , "AFD" , "Linke")), CASE WHEN "SPD" = @maxVal THEN 'SPD' WHEN "CDU" = @maxVal THEN 'CDU' WHEN "Gruenen" = @maxVal THEN 'Gruenen' WHEN "AFD" = @maxVal THEN 'AFD' WHEN "Linke" = @maxVal THEN 'Linke' END)
* g_proz_gueltig (Anteil an gültigen Stimmen) "G_value" / "gueltig" *100
6. Symbolisierung – Regelbasierte Darstellung pro Partei (z.B. Regel: „g_name“ = ‚SPD‘ und Beschriftung: ‚SPD‘)
* anschließend eine Regel für Alpha anlegen
* Intensität des Kanals wird über den folgenden Ausdruck geregelt: set_color_part( 'black','alpha',scale_linear( "g_percent", 19,47,255,0))
## Vorteile der Methode
* Informationsdichte – Parteifarben zeigen den Sieger, die Transparenz dessen Stimmenanteil; zwei Kennzahlen in einer Kartenebene.
* Intuitive Lesbarkeit – Bekanntes Farbschema der Parteien macht die Karte ohne umfangreiche Legende verständlich.
* Homogene Datenquelle – Alle Ausgangsdaten stammen von derselben Behörde, wodurch Kompatibilitätsprobleme minimiert werden.
## Nachteile der Methode
* Farbüberschneidungen – Ähnliche Farbtöne (z. B. helles Rot vs. Rosa) können die Siegerpartei verschleiern und reduzieren die Anzahl unterscheidbarer Kategorien.
* Manueller Aufwand – Das händische Säubern und Umstrukturieren der Wahldaten ist zeitintensiv und fehleranfällig.
* Lesbarkeit bei vielen Parteien – Je mehr Kategorien eingefärbt werden müssen, desto eher leidet die Klarheit der Darstellung.
* Alpha Skalierung – Bei knappen Ergebnissen kann der Transparenzeffekt zu schwach sein, bei Erdrutschsiegen zu dominant. 

<br><br>
<a id="EP.05"></a>
<br>
# EP.05 | Tile-Mapping (Anteil der Zulassungsbeschränkte Studiengänge je Bundesland)
![image](https://github.com/To-David/B10-DTM/blob/ce815194d411cfa91d97f0d35069a25f3b9d8d23/files/25-05-15_David_%C3%9C5_NC.png)
## Ergebnis
Die erstellte Tile Map Karte macht auf einen Blick sichtbar, wie stark der Anteil zulassungsbeschränkter Studiengänge zwischen den Bundesländern variiert. Stadtstaaten und hochschulstarke westdeutsche Länder weisen die höchsten Quoten auf, während mehrere Flächenländer im Osten deutlich unter dem Bundesdurchschnitt liegen. 
## Arbeitsschritte
1. Datenvorbereitung – CSV Tabelle
* Liste der Länderkürzel und Namen anlegen
* Spalte mit Prozentwerten der zulassungsbeschränkten Studiengänge ergänzen
* Datei als CSV speichern
2. Grundlage – Gitter erzeugen
* Rechteckiges Raster über Deutschland skizzieren
* Sechzehn Zellen auswählen, die der Lage der Bundesländer möglichst gut entsprechen
3. Geometrie anpassen – Verschieben & Skalieren
* Kacheln gemäß Aufgabenstellung in einem 4x4 Raster anordnen, sodass die relative geographische Lage weitestgehend erhalten bleibt.
4. Attributpflege – Kürzel zuweisen
* Neue Spalte im Kachel Layer erstellen
* Jede Zelle mit passendem Länderkürzel versehen
5. Tabellenverknüpfung – Join nach Feldwert
* CSV per Länderkürzel mit dem Kachel Layer verbinden
6. Symbolisierung – Abgestufte Darstellung
* Farbverlauf auf die Prozentspalte anwenden
## Vorteile der Methode
* Tile Mapping – klare Lesbarkeit: Gleich große Felder eliminieren Flächenverzerrung und erleichtern den Vergleich
* Bearbeitung in QGIS – geringe Softwarehürde: Alle Schritte lassen sich mit Bordmitteln ohne Zusatzplugins ausführen
## Nachteile der Methode
* Tile Mapping – begrenzte Genauigkeit: Stadtstaaten wie Berlin oder Bremen lassen sich nur annähernd platzieren
* Einheitsgröße – Informationsverlust: Flächenbezogene Aspekte (z. B. Hochschuldichte) gehen verloren

<br><br>
<a id="EP.06"></a>
<br>
# EP.06 | 
![image](https://github.com/To-David/B10-DTM/blob/85770d4b2ec99e0dfab818f12af690dba4be69fa/files/25-06-09_David_%C3%9C6_Syria-refugees.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


<br><br>
<a id="EP.07"></a>
<br>
# EP.07 | 
![image](https://github.com/To-David/B10-DTM/blob/ccf2f138f1fb91aedf499ebe413b8016ca8b6b05/files/25-06-12_David_%C3%9C7_orkan-kyrill.gif)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


<br><br>
<a id="EP.08"></a>
<br>
# EP.08 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-06-26_David_%C3%9C8_meteor-shower_quadrantids.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode


<br><br>
<a id="EP.09"></a>
<br>
# EP.09 | 
![image](https://github.com/To-David/B10-DTM/blob/07fc2314d888f1e9093d5af6cf99fb3469437fb2/files/25-06-26_David_%C3%9C9_2.5d-stadtmodelle_dresen.png)
## Ergebnis

## Arbeitsschritte

## Vorteile der Methode

## Nachteile der Methode
