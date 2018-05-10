# Kurseinheit 5

## 5.1 Wie kommuniziert die CPU mit Geräten? 

### 5.1.2 Wie kann die Kommunikation zwischen CPU und Controllers realisiert werden?

## 5.2 Wie funktioniert die Ein- und Ausgabe?

### 5.2.1 Welche drei Techniken gibt es für die Ein-/Ausgabe?

### 5.2.2 Wie funktioniert sie? Welche Vor- und Nachteile haben sie?

## 5.3 Wie sieht das Schichtmodell für E/A-Software aus?

### 5.3.1 Welche Aufgaben hat jede Schicht?

## 5.4 Wie verwaltet das Betriebssystem die E/A-Aufträgen und Geräten?

## 5.5 Warum braucht man Pufferung im Hauptspeicher bei der Ein-/Ausgabe?

## 5.6 Welche Techniken gibt es, um exklusive Geräte zu reservieren? Wie funktionieren sie? 
(Siehe auch Kunrseinheit 4)

## 5.7 Wie wird eine E/A-Operation durchgeführt?

### 5.7.1 Warum muss ein Benutzerprozess das Betriebssystem beauftragen, eine E/A-Operation auszuführen?

## 5.8 Welche Aufgaben haben der E/A-Teil des Betriebssystems, der Gerätetreiber, der Interrupt-Handler und Controller bei einer z.B. read()-Operation? Wie arbeiten sie zusammen?

## 5.9 Welche Ziele hat der E/A-Softwareentwurf?

## 5.10 Wie ist eine Festplatte aufgebaut? 

### 5.10.1 Welche Fähigkeit hat eine Festplatte ohne Dateisystem?

## 5.11 Wie wird die Zugriffszeit auf eine Festplatte definiert?

### 5.11.1 Welche Maﬂnahmen im Betriebssystem und im Controller können die Suchzeit, die Latenzzeit und die Übertragungszeit verkürzen oder einsparen?

## 5.12 Wie funktionieren die Strategien SSTF und SCAN?

### 5.12.1 Welche Vorteile und Nachteile haben die Strategien?

### 5.12.2 Was ist das Interleaving und der Interleaving-Faktor?

## 5.13 Wazu ist ein Dateisystem gut? Was ist ein Dateisystem? Was ist ein hierarchisches Dateisystem?

## 5.14 Welche Verfahren gibt es, um die Sektoren einer Datei zu verwalten?

## 5.15 Was ist eine FAT beim MS-DOS Betriebssystem?

### 5.15.1 Wie groﬂ ist eine FAT?
(Man sollte nicht sagen, dass eine FAT so groﬂ wie die Anzahl der Dateien ist, die auf einer Festplatte gespeichert werden. 
Eine FAT hat für jeden Block auf der Festplatte einen Eintrag.)

### 5.15.2 Wie werden die Sektoren einer Datei mit einer FAT verwaltet?
(Man sollte die Abbildung 5.14 zeichnen können.)

### 5.15.3 Wie kann ein Block einer Datei in FAT gefunden werden?

### 5.15.4 Welche Vor- und Nachteile hat die FAT? Was kann die FAT gut unterstützen?

## 5.16 Was ist eine Sektoradresstabelle?

## 5.17 Was ist die Idee bei i-nodes? Was ist ein i-node unter UNIX? Was steht in einem i-node?

### 5.17.1 Wo steht der Name de Datei unter UNIX?

### 5.17.2 Welche Attribute gibt es im i-node?

### 5.17.3 Wie groﬂ ist ein i-node bei UNIX? 
(64 Byte relativ klein)

### 5.17.4 Was steht genau in der 10 direkten Sektoradressen? 
(Was zeigt eine direkte Sektoradresse?)

### 5.17.5 Was zeigt eine indirekte Sektoradresse? 
(sie zeigt genau auf eine Adresse eines Blocks, in dem genau $X$ Adressen stehen.)

### 5.17.6 Was sind die doppelt und dreifach indirekten Sektoradressen?

### 5.17.7 Wie groﬂ kann eine Datei sein, die mit i-node verwaltet wird?
(10+X+X^2+X^3, Was ist das X? 
Wie kann man das X berechnen? Wie groﬂ kann das X sein? 
man sollte unbedingt die Abbildung 5.15 zeichnen und erklären können.)

### 5.17.8 Wie viele Zugriffe auf die Indexblöcke bei i-node werden maximal benötigt?

### 5.17.9 Wie kann eine Datei unter UNIX mit Hilfe der i-nodes gefunden werden?

## 5.18 Wie sieht ein klassisches UNIX-Dateisystem aus?

## 5.19 Wie funktioniert das Sektorfolgen-Verfahren zur Verwaltung der Sektoren einer Datei? 

### 5.19.1 Wie funktioniert das NTFS-Dateisystem? 

### 5.19.2 Was bedeutet ein Jounaling-Dateisystem?

## 5.20 Welche Verfahren gibt es, um die freien Sektoren zu verwalten?

### 5.20.1 Wie funktionieren sie und welche Vor- und Nachteile haben sie?

## 5.21 Wie werden bei UNIX die Zugriffsrechte einer Datei realisiert?
(Siehe Schutzbits in Kurseinheit 6)


Viele Grüﬂe
Lihong Ma  