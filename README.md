# rimor5-MISExport

<!--ts-->
   * [Usage](#usage)
      * [Format](#Format)
      * [Tabellendefinition](#Tabellendefinition)
   * [Tests](#tests)
     * [Local](#local)
     * [Public](#public)
<!--te-->

# Konzept Rimo R4
Die Schnittstelle MIS-Export exportiert Daten aus Tabellen oder Views, aus Rimo in CSV-Files ins lokale Dateisystem.

## Format
Das Schnittstellenfile wird im Format CSV (comma separated value) erstellt.
Das Trennzeichen ist ein Komma **`,`** 
Die Spaltentitel werden immer angezeigt, heisst der erste Record enthält die Spaltenüberschrift (HDR).

## Tabellendefinition
Es kann jede Tabelle und View der Rimo Datenbank exportiert werden. Die folgende Dokumentation beschränkt sich auf einige auserwählte Daten.
Liegenschaft und Mieterdaten:
- lieg
- gebaeude
- haus
- etage
- objekt
- objlage
- objart
- mieter
- person
- mietfall
- mietzins
- verwaltung
- verwgebiet
***
<br><br><br>


# Datenzusammenhänge
![image.png](/.attachments/image-e42cfd58-1d2a-4cfe-8d3a-c2848d285951.png)
***
<br><br><br>

# Exportmechanismus
Der Exportmechanismus kann manuell gesteuert werden. Die Selektionsmaske ist von der Standardschnittstelle MIS-Export vorgegeben:
![image.png](/.attachments/image-a2a8f785-bf2a-44b2-9e05-c9b8381a8691.png)
Die Selektion des Exports kann nur mittels Liegenschaftsnummer eingegrenzt werden. Eine periodische Eingrenzung ist nicht möglich – es werden somit immer sämtliche Datensätze einer Tabelle (inkl. Historydaten) exportiert.

Der Export kann ebenfalls als Job geplant werden.
***

## Exportkonfiguration im Rimo
Im Rimo wird folgende Exportkonfiguration (SYSTEM\MISEXP) benötigt:
- Name der Tabellen, welche exportiert werden sollen.

Die Konfiguration der zu exportierenden Tabellen/Views kann während der Laufzeit von Rimo ohne spezifische Programmanpassung erweitert werden.
***
<br><br><br>
# Hinweis: Change-Management Schnittstelle MIS-Export
Aufgrund zukünftig geplanter Systemerweiterungen im Rimo kann es sein, dass die für diesen Export konfigurierten Tabellen mit einer zukünftigen Rimo Version ändern. Meist handelt es sich hier um Datenbankerweiterungen wie z.B. neue Felder auf einer der Exporttabellen. Die Datenbankerweiterungen werden automatisch nach einem entsprechenden Versionsupdate auch für diese Schnittstelle verwendet.

Grundsätzlich werden solche Datenbankänderungen im Updatejournal zum entsprechenden Versionsupdate vermerkt, jedoch werden nicht automatisch technische Dokumentationen dazu verfasst. Sofern die laufenden Datenbankerweiterungen keinen zwingenden Änderungseinfluss auf die Schnittstelle haben, werden sie von der W&W Immo Informatik nicht aktiv kommuniziert. Änderungen mit zwingendem Einfluss auf die Schnittstelle (z.B. Grundlegende Datenstrukturänderungen) werden dem Kunden vor der Auslieferung des Updates kommuniziert.

Änderungen an bestehenden Datenstrukturen, wie z.B. Formatänderungen eines bestimmten Feldes, Änderungen des Feldinhaltes bzw. der Businesslogik von Rimo sind der W&W Immo Informatik vorbehalten.
***
