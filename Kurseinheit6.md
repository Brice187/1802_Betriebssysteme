# Kurseinheit 6

## 6.1 Was sind die Ziele der Sicherheitsmaﬂnahmen?

* **Vertraulichkeit**: Daten dürfen nur von Personen gelesen werden, die hierzu befugt sind
* **Integrität**: Unversehrtheit und Korrektheit von Daten
* **Verfügbarkeit:** Alle Programme und das BS müssen stets verfügbar sein => Gefahr z.B. durch
denial-of-service-Angriff
* **Schutz vor finanziellem Verlust**: Rechner wird von Personen benutzt, die hierfür nicht
zugelassen sind und die Kosten nicht tragen
* **Schutz vor Missbrauch**:  kriminellen Handlungen (Botnet / Spam etc.)

## 6.2 Was sind der Unterschied zwischen dem persistenten und transienten Rechtezustand?

### 6.2.1 Kann man Beispiele angeben?

## 6.3 Was versteht man unter Subjekten und Objekten?

## 6.4 Was ist der Unterschied zwischen Identifikation und Authentisierung?

## 6.5 Was ist das SETUID-Bit?

Setuid (Set User ID, manchmal auch suid) ist ein Zugriffsbit für Dateien oder Verzeichnisse des Unix-Betriebssystems.

* Ausführbare Programme bei denen dieses Bit gesetzt ist, werden mit den Rechten des Benutzers ausgeführt dem die Datei gehört, anstatt mit den Rechten desjenigen Benutzers, der die Datei ausführt.
* Auf Verzeichnissen bewirkt Setuid, dass Dateien, die innerhalb des Verzeichnisses angelegt werden, nicht dem Benutzer gehören, der sie anlegt, sondern dem Eigentümer des Verzeichnisses.

## 6.6 Können Sie ein Beispiel der Anwendung von SETUID-Bit angeben? Wie funktioniert die Änderung der passwd-Datei? Was ist mit der Sicherheit bei Benutzung vom SETUID-Bit?

(Die Änderung der passwd-Datei. Wie viele passwd-Dateien gibt es? Wie sehen die Zugriffsrechte auf sie aus? Bei welcher Datei wird das SETUID-Bit gesetzt?)

Die Datei, in der unter UNIX die Passwörter aller Benutzer gespeichert sind, kann nur von root beschrieben werden. Dennoch sollte jeder Benutzer die Möglichkeit haben, sein eigenes Passwort zu ändern. Dazu gibt es das Programm usr/bin/passwd, bei welchem das s-bit gesetzt ist. Der Prozess erhält beim Aufruf die Rechte des Besitzers, also root, und kann die Passwortdatei ändern. Das Programm wird aber nur die Änderung des eigenen Passwortes erlauben.

## 6.7 Wie funktioniert die Spooling-Technik?

Ein Spooling ist eine Warteschlange oder ein Puffer, um die Dateien aufzunehmen, die z.B. ausgedruckt werden sollen. Diese Warteschlange muss geschützt werden, es kann nicht sein, dass jeder Benutzer-Prozess einfach seine Datei in die Warteschlange schreiben darf.
Also braucht man ein Druckerprogramm, bei der Ausführung des Programms wird ein spezieller Prozess erzeugt, der das Schreiben in die Warteschlange übernimmt. Dieser Prozess gehört einem speziellen Benutzer `daemon`, nur er darf das Programm schreiben und lesen, die anderen können das Programm nur ausführen(mit Schutzbits: `rwxs--x--x)`. Wenn das s-bit des Programm gesetzt wird, kann auch jeder Benutzer mit den Rechten des Besitzers das Druckerprogramms ausführen. Das Schreiben in die Warteschlange wird durch das Programm kontrolliert.

## 6.8 Was ist eine Zugriffskontrollliste?

Ganz allgemein ermöglichen Zugriffskontrollen, einzelnen Subjekten den Zugriff auf einzelne Objekte zu erlauben oder zu verbieten.

Die Zugriffskontrollliste (access control list, ACL) gehört dabei zu den granulatorientieren(objektorientierten) Implementierungen, da die Rechtefestlegungen **beim Objekt** gespeichert werden (im Gegensatz zur subjektorientierten Implementierung).

Eine ACL ist ein Record, wobei jeder Eintrag für jeden zu einer Domäne gehörenden Prozess angibt, für welche Modi der Zugriff erlaubt ist, siehe Abb. 6.3., Skript S. 228.
Es gibt ferner die "benannten ACLs": Man kann ACLs als eigenständige Einheit auffassen, ihnen einen Namen geben und mehreren Objekten die gleiche benannte ACL zuweisen.

## 6.8.1 Was ist der Vorteil?
(Ein Benutzer kann die Zugriffsrechte selbst vergeben)
* bei ACL **allgemein**: Default-Wert ist "verboten", d.h. alles erlaubte muss explizit erlaubt sein
* bei **benannten ACLs**: Wenn mehrere Objekte die gleiche ACL haben, spart man Platz
* Der Besitzer einer Datei kann bei ACLs selbst festlegen kann, wer und welche Zugriffsrechte
auf eine Datei hat.

## 6.9 Wie sehen die Schutzbits bei UNIX aus?

Bei Schutzbits werden zu jeder Datei drei Gruppen zu je drei Bits angegeben. Zu jeder Gruppe
1. Datei-Eigentümer
2. Gruppe (Eigentümer muss nicht unbedingt Mitglied in der Gruppe sein)
3. Sonstige

legt jeweils ein Bit fest, ob Leserecht, Schreibrecht oder Ausführungsrecht erteilt ist.
Bsp. `rwxrw-r--`

### 6.9.1 Wie werden die Rechte einer Datei für einen Benutzer ausgewertet?

Die Schutzbits werden immer der Reihe nach geprüft. Wenn ich Mitglied in einer Gruppe bin und diese Gruppe keinen Zugriff hat, wird mir der Zugriff verweigert, auch wenn alle übrigen Zugriff bekommen.

### 6.9.2 Wie kann man mit Schutzbits realisieren, dass eine Datei allen bis auf eine Gruppe von bestimmten Personen zugänglich ist?

Rechtevergabe: `rwx---rwx`

## 6.11 Was ist eine Capability?

Capabilities gehören zu den subjektorientierten Implementierungen, die Rechte sind folglich beim Subjekt gespeichert. Nun kann man leicht herausfinden, welche Rechte ein Subjekt hat, es ist jedoch schwerer herauszufinden, welche Rechte für ein Objekt vorhanden sind.

Capability ist äquivalent zu einem Profil -> Unterschied liegt in der Adressierung der Objekte.

Eine Capability besteht aus einem Objektverweis plus die Operationen auf dem Objekt. Dieser Verweis muss geschützt werden, z.B. das Betriebssystem bewahrt die Capabilities auf.

### 6.11.1 Kann man ein Beispiel geben, wie es funktioniert?

Wenn ein Prozess eine Datei mit open() öffnen will, sucht das Dateisystem zuerst die Lokalisierung der Datei auf der Festplatte. Danach erzeugt das Betriebssystem einen Dateikontrollblock (ein Dateikontrollblock heißt bei UNIX auch i-node) für die Datei, in dem der Besitzer der Datei, Gruppe, Opreationen auf der Datei (read, write, append), i-node usw. stehen. Das Dateikontrollblock wird in eine Tabelle von allen offenen Dateien (system-wide open-file table) eingetragen, die zur Verwaltung der offenen Dateien vom Betriebssystem gebraucht wird. Die Stelle des Dateikontrollblocks in der Tabelle ist eine ganze Zahl, also eine Nummer. Diese Nummer wird wieder in die Tabelle der offenen Dateien des Prozesses (per-process open-file table) eingetragen. Die Tabelle per-process open-file table heißt in Kurseinheit 7 Umsetztabelle des Prozesses, der Nummer des Dateikontrollblocks heißt Dateiidentifizierer, siehe
Abbildung 7.3 im Kurseinheit 7.
Jetzt werden die Rechte des Prozesses (Subjektes) auf die Datei im Betriebssystem aufbewahrt. Der Prozess hat auch nur die Rechte auf die Datei, die bei der Erzeugung des Dateikontrollblocks übergeben wurden. Wenn der Prozess die Datei lesen oder schreiben möchte, muss er den Dateiidentifizierer als Index zum system-wide open-file table als Parameter übergeben. Dieser Dateiidentifizierer ist der Objektverweis oder das Ticket.

## 6.12 Was ist die Schwäche diskretionärer Zugriffskontrollen?

## 6.13 Was ist die Idee der Informationskontrollen?

### 6.13.1 Wie kann man mit dem Bell-La Padula-Modell das Sicherheitsziel der Vertraulichkeit realisieren?

### 6.13.2 Wie kann man mit dem Biba-Modell die Integrität erreichen?
