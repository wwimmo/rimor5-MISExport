# MIS Export IAZI

<!--ts-->
Table of contents
   * [Ausgangslage](#Ausgangslage)
   * [Anforderungen](#Anforderungen)
   * [Konzept Rimo](#Konzept-Rimo)
      * [Format](#Format)
      * [Tabellendefinition](#Tabellendefinition)
   * [Datenzusammenhänge](#Datenzusammenhänge)
   * [Exportmechanismus](#Exportmechanismus)
     * [FTP Verbindung](#FTP-Verbindung)
       * [Exportkonfiguration im Rimo](#Exportkonfiguration-im-Rimo)
       * [Exportpfad](#Exportpfad)
       * [Exportjob](#Exportjob)
   * [Hinweis: Change-Management Schnittstelle MIS-Export](#Hinweis-Change-Management-Schnittstelle-MIS-Export)
<!--te-->
# Ausgangslage
Aus Rimo sollen Daten in das System von IAZI exportiert werden. Die Rimo Daten kommen als ganze Tabellensätze und sind grösstenteils nicht aufbereitet (Ausnahme Finanzdaten).

# Anforderungen
Export von Datentabellen aus Rimo in Reamis. Die Daten sind im Format CSV aufbereitet. Die CSV Dateien sollen automatisiert exportiert und an Reamis geschickt werden.
Die Rimo Daten kommen als ganze Tabellensätze und sind grösstenteils nicht aufbereitet (Ausnahme Finanzdaten wegen Einschränkung auf die letzten drei Jahre).
Der Datenumfang und Anforderungsbeispiele sind gemäss Mail vom 29.03.2016 zusammengestellt.

# Konzept Rimo
Die IAZI Schnittstelle soll über die im Rimo bereits existierende Standardschnittstelle „MIS-Export“ gelöst werden. Die Schnittstelle MIS-Export exportiert Rohtabellen oder Views aus Rimo in CSV-Files.

## Format
Das Schnittstellenfile wird im Format CSV (comma separated value) erstellt.
Das Trennzeichen ist ein Komma **`,`** 
Die Spaltentitel werden immer angezeigt, heisst der erste Record enthält die Spaltenüberschrift (HDR).

## Tabellendefinition
Folgende Tabellen werden für den Export definiert.

**Liegenschaft und Mieterdaten:**
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
- mzbpromiet
- mzbest
- mzbart
- mieteroption
- verwaltung
- verwgebiet

**Finanzdaten:**
- fibu
- fibujahr1
- bilanztx
- fibukonto1
- bilanzkat
- fibubudget
- budgetprokonto
- vfibubuchungiazi

Die View vfibubuchungiazi wird speziell auf der Datenbank erstellt und ist keine standardisierte Tabelle/View auf der Rimo R4 Datenbank.
Sie enthält folgende Daten:
fibunr, jahr, monat, kontonr, kontobezeichnung, betrag
Der **Betrag (Saldo)** wird in dieser View **pro Monat** (vom ersten Tag des Monats bis zum letzten Tag des Monats) **pro Konto** kumuliert.

Die benötigten View können per Script 
- [CreateViewVfibubuchungiazi_ANY.sql](CreateViewVfibubuchungiazi_ANY.sql) 

hinzugefügt werden.

# Datenzusammenhänge
**Liegenschaft- und Mieterdaten**<br>
![DataRelationship](/_grafiken/DataRelationship_iazi_Liegenschaft_Mieterdaten.png)

**Gebäudeversicheru
**Finanzdaten:**<br>
![DataRelationship](/_grafiken/DataRelationship_iazi_Finanzdaten.png)

# Exportmechanismus
Der Exportmechanismus kann manuell gesteuert werden. Die Selektionsmaske ist von der Standardschnittstelle MIS-Export vorgegeben:
![ExportMechanism](/_grafiken/Exportmechanismus_iazi.png)
Die Selektion des Exports kann nur mittels Liegenschaftsnummer eingegrenzt werden. Eine periodische Eingrenzung ist nicht möglich – es werden somit immer sämtliche Datensätze einer Tabelle (inkl. Historydaten) exportiert.

Der Export kann ebenfalls als Job geplant werden.

## Exportkonfiguration im Rimo
Im Rimo wird folgende Exportkonfiguration (SYSTEM\MISEXP) benötigt:
- Name der Tabellen, welche exportiert werden sollen.
Hier werden die Daten definiert, welche exportiert werden (siehe vorherige Kapitel). 

Die Konfiguration der zu exportierenden Tabellen/Views kann während der Laufzeit von Rimo ohne spezifische Programmanpassung erweitert werden.

Somit sind zukünftige Datenerweiterungen derselben Schnittstelle auf einfachste Art und Weise realisierbar.

Der Export der Daten muss einmalig eingerichtet werden.

### FTP Verbindung
Im Rimo kann eine FTP Verbindung (Protokolle wie FTP oder FTPs mit TLS/SSL v3) eingerichtet werden.
Die exportierten Daten werden nach dem Export auf diesen Server hochgeladen.

### Exportpfad
Es muss ein Exportpfad für die Dateien definiert werden.

### Exportjob
Der Export wird als automatisierter Job eingerichtet. Zeitpunkt und Häufigkeit dieses Jobs kann frei definiert werden – z.B.
- jede Nacht um 03:00
- jeweils am 5. Tag jeden Monats um 10:00
- alle 4 Stunden
Der Job wird zum gewünschten Zeitpunkt wiederkehrend durch den Rimo R5 Server ausgeführt. Dazu ist keine Anwen-derinteraktion mehr erforderlich.

<br><br><br>

# Hinweis: Change-Management Schnittstelle MIS-Export
Aufgrund zukünftig geplanter Systemerweiterungen im Rimo kann es sein, dass die für diesen Export konfigurierten Tabellen mit einer zukünftigen Rimo Version ändern. Meist handelt es sich hier um Datenbankerweiterungen wie z.B. neue Felder auf einer der Exporttabellen. Die Datenbankerweiterungen werden automatisch nach einem entsprechenden Versionsupdate auch für diese Schnittstelle verwendet.

Grundsätzlich werden solche Datenbankänderungen im Updatejournal zum entsprechenden Versionsupdate vermerkt, jedoch werden nicht automatisch technische Dokumentationen dazu verfasst. Sofern die laufenden Datenbankerweiterungen keinen zwingenden Änderungseinfluss auf die Schnittstelle haben, werden sie von der W&W Immo Informatik nicht aktiv kommuniziert. Änderungen mit zwingendem Einfluss auf die Schnittstelle (z.B. Grundlegende Datenstrukturänderungen) werden dem Kunden vor der Auslieferung des Updates kommuniziert.

Änderungen an bestehenden Datenstrukturen, wie z.B. Formatänderungen eines bestimmten Feldes, Änderungen des Feldinhaltes bzw. der Businesslogik von Rimo sind der W&W Immo Informatik vorbehalten.