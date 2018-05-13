# Kurseinheit 5

## 5.1 Wie kommuniziert die CPU mit Geräten

Die allermeisten Geräte werden über einen Controller mit mit der CPU verbunden. Controller sind entweder auf der Hauptplatine des Rechners oder in Form einer Steckkarte mit CPU und Hauptspeicher zusammen im gleichen Gehäuse untergebracht.

Vorteile:

* CPU wird von elementaren Aufgaben (z. B. Checksums zur Fehlererkennung) entlastet. Controller hat spezialisierten Prozessor hierfür
* neuartige Geräte mit Controller, der bekannten Gerätetyp simuliert, kann ohne Änderung des BS an einem Rechner betrieben werden

### 5.1.2 Wie kann die Kommunikation zwischen CPU und Controllern realisiert werden

CPU und Controller benutzen spezielle Register im Controller für Daten und Befehle, um miteinander zu kommunizieren. Die das Betriebssystem aus- führende CPU übermittelt den Controllern Aufträge und ggf. Parameter, und die Controller geben Abschlussmeldungen, Fehlercodes, gelesene Daten u.ä. zurück. Es gibt zwei Techniken:

* **I/O-Ports**: Jedem Register eines Controllers wird eine Port-Nummer zugewiesen. Die CPU kann durch den Assembler-Befehle diesen Port lesen/schreiben
* **memory-mapped I/O**: Alle Geräte können einheitlich im Adressraum des Hauptspeichers angesprochen werden
    - Vorteil: Man kann mit Hochsprache statt Assembler auf Hardware zugreifen

![Speicherabgebildete Ein-/Ausgabe (memory-mapped I/O).](img/memorymappedio.png)

### 5.2.1 Welche drei Techniken gibt es für die Ein-/Ausgabe? Wie funktioniert die Ein- und Ausgabe? Welche Vor- und Nachteile haben diese Techniken?

#### 1. Programm-gesteuerte Ein-/Ausgabe

Applikationen wollen unabhängig voneinander Ein- oder Ausgaben durchführen: Systemaufruf wird synchron von CPU abgearbeitet (aktives Pollen der Controller-Register). Einfach, aber CPU wird komplett belegt.

#### 2. Interrupt-gesteuerte Ein-/Ausgabe

E/A-Operationen sind asynchron und arbeiten mit Hilfe von Unterbrechungen.

Phasen:

1. Startphase: E/A-Auftragsübermittlung mit Parametern von CPU an Gerät. Hierdurch wird das Gerät gestartet und ein Datentransport zwischen Gerät und Puffern bzw. Registern initiiert.
2. Wartephase: Warten auf Ende des Transports. Das Ende der Wartephase wird durch eine Unterbrechung angezeigt.
3. Nachbearbeitungsphase: aufgetretene Fehler analysiert, Puffer reorganisiert und ähnliches Aufgaben erledigt werden.

Nachteil: Auch hier ist die CPU noch involviert, Daten werden wortweise zwischen Hauptspeicher und Controller hin und her geschaufelt

#### 3. Ein-/Ausgabe mit DMA

Bestimmte Controller dürfen Daten in den Hauptspeicher schreiben. Die CPU wird erst wieder involviert, wenn die gewünschten Daten bereits im Hauptspeicher sind. Der DMA-Controller besitzt mindestens vier Register, die vom Prozessor gelesen und geschrieben werden können, und kann unabhängig von der CPU auf das Bus-System zugreifen.

![DMA benutzt denselben Systembus wie die anderen Komponenten im System.](img/dma.png)

Beispiel:

1. Um die HDD-Übertragung zu initialisieren, schreibt die CPU die Quelle (Festplatte), das Ziel (die Anfangsadresse eines Hauptspeicherbereichs, in den die Daten geschrieben werden sollen) und die Menge der zu übertragenden Daten in die DMA-Register und gibt zusätzlich ein Lese-Kommando mit der Nummer des zu lesenden Sektors an den Controller der Festplatte ab.
2. Die CPU mit anderen Arbeiten fort.
3. Der Festplatten-Controller liest inzwischen die Daten von der Festplatte in seinen Puffer und führt die Fehlerprüfung durch.
4. Sobald die gültigen Daten im Puffer des Festplatten-Controllers vorliegen, sendet er ein Signal an den DMA-Controller.
5. Der DMA-Controller veranlasst nun den Festplatten-Controller, die Daten aus dem Puffer wortweise über den Systembus in den vorgegebenen Bereich im Hauptspeicher zu schreiben.
6. Danach erzeugt der DMA-Controller eine Unterbrechung und teilt der CPU so mit, dass die Daten jetzt im Hauptspeicher vorliegen.

## 5.3 Wie sieht das Schichtmodell für E/A-Software aus

![Ein-/Ausgabe-System](img/easystem.png)

### 5.3.1 Welche Aufgaben hat jede Schicht

## 5.4 Wie verwaltet das Betriebssystem die E/A-Aufträgen und Geräten

### E/A-Aufträge

Mehrere Ein-/Ausgaben können für das gleiche Gerät gelten; z.B. können mehrere Prozesse Sektoren auf derselben Platte lesen oder schreiben. Daher muss für jedes Gerät eine E/A-Auftragsliste vom Betriebssystem geführt werden. Bei Geräten, die mehrere Aufträge parallel behandeln können und bei denen die Aufträge nicht notwendigerweise in der Reihenfolge beendet werden, in der sie angekommen sind (z. B. bei manchen Platten/Controllern), muss bei jeder Fertigmeldung des Geräts zugleich angegeben werden, welcher Auftrag gemeint ist.

### Geräte

Ein Drucker kann einen Papierstau haben oder gerade mit dem Ausdrucken einer Seite beschäftigt sein oder eine Kommunikationsverbindung kann gerade gestört sein. Die Gerätezustandstabelle enthält für jedes angeschlossene Gerät alle derartigen Angaben sowie die E/A-Auftragsliste.

![Gerätezustandstabelle und E/A-Auftragslisten](img/geraete_ea.png)

## 5.5 Warum braucht man Pufferung im Hauptspeicher bei der Ein-/Ausgabe

Wenn ein Benutzerprozess einen Datenblock von einer Platte in einen Datenbereich in seinem Adressraum lesen will, führt er einen read-Systemaufruf aus und blockiert dann und wartet, bis die relativ langsame Übertragung abgeschlossen ist. Danach wird der Block verarbeitet und der nächste Block gelesen, nun blockiert der Prozess wieder. Dieser Vorgang ist sehr ineffizient. Außerdem könnte ein Benutzerprozess aus irgendeinem Grund gezwungen sein, seinen Adressraum zeitweilig auszulagern.

Effizienzsteigerung:

* Double Buffering: Während das Betriebssystem einen Puffer leert bzw. füllt, werden Daten an den zweiten Puffer übertragen
* Ringpuffer: Mehr als zwei Puffer werden verwendet

## 5.6 Welche Techniken gibt es, um exklusive Geräte zu reservieren? Wie funktionieren sie

Spooling: Drucker sind langsam und werden von vielen benutzt, müssen aber pro Ausdruck exclusiv belegt werden. Das Betriebssystem simuliert für jeden Prozess einen eigenen privaten Drucker. Druckjobs werden in Datei gesammelt. Ein `daemon` fügt die Datei in eine Warteschlange (Spooling-Verzeichnis) ein und leitet gibt diese nach und nach direkt auf dem Drucker aus.

Allgemein: Versucht ein Prozess, ein Gerät zu belegen, das nicht frei ist, blockiert er und wird in eine Warteschlange gelegt. Wenn das Gerät wieder frei ist, darf der erste Prozess in der Warteschlange es belegen und erhält den Zugriff

## 5.7 Wie wird eine E/A-Operation durchgeführt

### 5.7.1 Warum muss ein Benutzerprozess das Betriebssystem beauftragen, eine E/A-Operation auszuführen

## 5.8 Welche Aufgaben haben der E/A-Teil des Betriebssystems, der Gerätetreiber, der Interrupt-Handler und Controller bei einer z.B. read()-Operation? Wie arbeiten sie zusammen

## 5.9 Welche Ziele hat der E/A-Softwareentwurf

## 5.10 Wie ist eine Festplatte aufgebaut

### 5.10.1 Welche Fähigkeit hat eine Festplatte ohne Dateisystem

## 5.11 Wie wird die Zugriffszeit auf eine Festplatte definiert

### 5.11.1 Welche Maﬂnahmen im Betriebssystem und im Controller können die Suchzeit, die Latenzzeit und die Übertragungszeit verkürzen oder einsparen

## 5.12 Wie funktionieren die Strategien SSTF und SCAN

### 5.12.1 Welche Vorteile und Nachteile haben die Strategien

### 5.12.2 Was ist das Interleaving und der Interleaving-Faktor

## 5.13 Wazu ist ein Dateisystem gut? Was ist ein Dateisystem? Was ist ein hierarchisches Dateisystem

## 5.14 Welche Verfahren gibt es, um die Sektoren einer Datei zu verwalten

## 5.15 Was ist eine FAT beim MS-DOS Betriebssystem

### 5.15.1 Wie groﬂ ist eine FAT
(Man sollte nicht sagen, dass eine FAT so groﬂ wie die Anzahl der Dateien ist, die auf einer Festplatte gespeichert werden.
Eine FAT hat für jeden Block auf der Festplatte einen Eintrag.)

### 5.15.2 Wie werden die Sektoren einer Datei mit einer FAT verwaltet
(Man sollte die Abbildung 5.14 zeichnen können.)

### 5.15.3 Wie kann ein Block einer Datei in FAT gefunden werden

### 5.15.4 Welche Vor- und Nachteile hat die FAT? Was kann die FAT gut unterstützen

## 5.16 Was ist eine Sektoradresstabelle

## 5.17 Was ist die Idee bei i-nodes? Was ist ein i-node unter UNIX? Was steht in einem i-node

### 5.17.1 Wo steht der Name de Datei unter UNIX

### 5.17.2 Welche Attribute gibt es im i-node

### 5.17.3 Wie groﬂ ist ein i-node bei UNIX
(64 Byte relativ klein)

### 5.17.4 Was steht genau in der 10 direkten Sektoradressen
(Was zeigt eine direkte Sektoradresse?)

### 5.17.5 Was zeigt eine indirekte Sektoradresse
(sie zeigt genau auf eine Adresse eines Blocks, in dem genau $X$ Adressen stehen.)

### 5.17.6 Was sind die doppelt und dreifach indirekten Sektoradressen

### 5.17.7 Wie groß kann eine Datei sein, die mit i-node verwaltet wird
(10+X+X^2+X^3, Was ist das X?
Wie kann man das X berechnen? Wie groﬂ kann das X sein?
man sollte unbedingt die Abbildung 5.15 zeichnen und erklären können.)

### 5.17.8 Wie viele Zugriffe auf die Indexblöcke bei i-node werden maximal benötigt

### 5.17.9 Wie kann eine Datei unter UNIX mit Hilfe der i-nodes gefunden werden

## 5.18 Wie sieht ein klassisches UNIX-Dateisystem aus

## 5.19 Wie funktioniert das Sektorfolgen-Verfahren zur Verwaltung der Sektoren einer Datei

### 5.19.1 Wie funktioniert das NTFS-Dateisystem

### 5.19.2 Was bedeutet ein Jounaling-Dateisystem

## 5.20 Welche Verfahren gibt es, um die freien Sektoren zu verwalten

### 5.20.1 Wie funktionieren sie und welche Vor- und Nachteile haben sie

## 5.21 Wie werden bei UNIX die Zugriffsrechte einer Datei realisiert
(Siehe Schutzbits in Kurseinheit 6)