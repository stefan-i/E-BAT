# E-Book Analytics Tool

## Funktionsweise des Tools
Bei dem Tool handelt es sich um ein R-Skript, das in einem R Markdown (.rmd) Dokument eingebettet ist. Mit Hilfe von R Markdown ist es möglich Code und weitere Elemente (z.B. Text, Tabellen, Grafiken) in einem einzigen Dokument zu kombinieren. 

Die zu analysierenden Nutzungsdaten von E-Book-Paketen werden in vorher definierten Verzeichnissen abgelegt. Das R-Skript liest automatisch alle Dateien dort ein, bereinigt diese und führt sie zusammen. Danach werden verschiedene Kennzahlen zu den Paketen berechnet und als interaktive Tabelle mit Farbcodierung ausgegeben. Zusätzlich werden Boxplots zur grafischen Darstellung der Verteilung der Einzeltitelnutzung innerhalb der Pakete erstellt.

## Datenimport
Das Tool liest die zu analysierenden Daten aus den im Skript angegebenen Verzeichnissen. Diese können in den Zeilen mit "setwd" beliebig angepasst werden. 

Das Skript erwartet im "Stammverzeichnis" die Datei "Pakete.csv". In diese Tabelle werden vor Ausführung des Skripts die Paketmetadaten (Spalten "Paketname", "Paket_ID", "Paketpreis") hinterlegt. 

Im Unterverzeichnis "CSV-G" befinden sich die Gesamttitellisten der Pakete der einzelnen Verlage als CSV-Dateien. Diese müssen zwingend die Spalten "ISBN" (Zahl ohne Bindestriche) und "Paket_ID" (z.B. "utb_Politik 2019") enthalten. Über die Paket_ID identifizieren die Verlage in ihren Listen die Zugehörigkeit der Titel zu einem bestimmten Paket. 

Im Unterverzeichnis "CSV-N" befinden sich die Listen mit den Nutzungszahlen als CSV-Dateien. Diese müssen zwingend die Spalten "ISBN" und "Nutzung" (Zahl der Nutzungen pro Titel) enthalten. Die Dateien dürfen jeweils beliebige weitere Spalten enthalten.

Die Gesamttitellisten und Nutzungslisten werden jeweils zu einer Liste zusammengeführt und die nicht benötigten Spalten entfernt. Anschliessend werden beide Listen anhand der gemeinsamen Variable "ISBN" verknüpft. Die so entstehende Liste enthält nun alle für die weiteren Berechnungen notwendigen Informationen. Schliesslich werden noch alle Pakete entfernt, deren Paket_ID nicht in der Tabelle "Pakete.csv" aufgeführt ist.

## Berechnung der Kennzahlen
In den nächsten Schritten werden nun verschiedene Kennzahlen berechnet:

 - Gesamtnutzungszahlen pro Einzelpaket
 - Zahl enthaltener Titel
 - Durchschnittspreis pro Buch
 - Prozentsatz genutzter Titel
 - Cost-per-use (CPU)

Die Kennzahlen werden mit der Funktion "Reactable" als interaktive Tabelle ausgegeben und können durch Anklicken der Spalten neu sortiert werden. Die Zellen werden mit einer Farbcodierung (grün=beste Werte, gelb= mittlere Werte, rot=schlechteste Werte) hinterlegt, die automatisch auf Basis der Bandbreite der Daten einer Variable generiert wird. Die Farbpalette kann mit der Funktion "colpal" beliebig angepasst werden. 

## Boxplot der Einzeltitelnutzung
Mit Hilfe der Funktion ggplot2 werden Boxplots zur grafischen Darstellung der Verteilung der Einzeltitelnutzung innerhalb der Pakete erstellt. Zur besseren Sichtbarkeit auch wenig genutzter Titel werden die Ergebnisse auf einer logarithmischen Skala dargestellt.

## Liste aller erworbenen Titel
Zusätzlich wird eine Liste alles erworbenen Titel generiert. Die Sortierung kann durch Anklicken der Spaltenüberschriften geändert werden. Die Spalten können durch Eingabe von Text in die leeren Felder gefiltert werden.

## Ausgabe der Ergebnisse
Durch Ausführen des R Markdown Dokuments in R Studio wird standardmässig eine HTML-Datei generiert in der alle Ergebnisse zusammengeführt werden. Durch Erweiterung des R Markdown Dokuments können noch beliebige weitere Analysen und Textabschnitte hinzugefügt werden. Es ist auch eine Ausgabe als PDF oder Word (.docx) möglich, allerdings ist die Ergebnistabelle dann statisch und kann nicht mehr durch Anklicken der Spalten neu sortiert werden.


