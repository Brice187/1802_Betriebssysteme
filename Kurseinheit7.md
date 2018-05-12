# Kurseinheit 7

## 7.1 Was ist eine Shell?

Eine Shell ist eine Kommandozeilen-Schnittstelle, bzw. ein Kommandointerpreter. Es ist der Teil des Betriebssystems, welcher Kommandos einliest, interpretiert und die weiteren erforderlichen Schritte zur Ausführung der Aktivität veranlasst. Sollte nicht Teil des BS-Kerns sein.

## 7.2 Wo kann man Shell am besten einsetzen?

In jedem Betriebssystem. Eine Shell (*Schale/Muschel*) ist auch eine Art Benutzungsoberfläche/UI für den Betriebssystem-Kern. Shell ist sehr mächtig für die z.B. administrativen Aufgaben.

## 7.3 Was ist ein Kommandoprozedur?

Ein Programm, welches in einer Kommandosprache geschrieben ist. Eine Kommandoprozedur enthält im einfachsten Fall eine Folge von Kommandos, die bei Ausführung der Kommandoprozedur der Reihe nach ausgeführt werden.

## 7.4 Warum ist manchmal vorteilhafter mit einer Kommandoprozedur als mit einem z.B. C-Programm zu arbeiten?

Bei der Programmiersprache Shell gibt es Anweisungen, Schleifen, Variablen und in gewisser Weise auch Unterprogramme. Eine kleine Kommandoprozedur kann "mal eben schnell" geschrieben werden, während ein C- Programm erst kompiliert und gelinkt werden muss.

### 7.4.1 Warum wird Shell in der Praxis nicht wie C, C++ und Java als Programmiersprache eingesetzt?

## 7.5 Was ist das Kommando make? Wie funktioniert das Kommando make? Was passiert, wenn man make eintippt?

Das Kommando `make` sorgt dafür, bei einer Programm-Aktualisierung nur die von der Veränderung betroffenen Programmteile zu aktualisieren, um das gesamte Programm auf den neuesten Stand zu bringen.

Dafür benötigt `make` eine Beschreibung der Abhängigkeiten zwischen den Dateien und die Definition der auszuführenden Aktion.

### 7.5.1 Was ist eine Makefile? Was steht in einer Makefile? Wie funktioniert eine Makefile?

Diese Angaben werden in einer Datei `Makefile` abgelegt. `make` prüft das Modifikationsdatum jeder Datei, ermittelt damit, welche Dateien geändert wurden, leitet aus den im `Makefile` beschriebenen Abhängigkeiten die notwendigen Aktionen ab und führt sie aus. Das heißt, dass das letzte Veränderungsdatum in den Attributen einer Datei eine wichtige Rolle bei `make` spielt.

![Funktionsprinzip des Kommandos `make`](img/makefile.png)

### 7.5.6 Wie wird entschieden, welche abhängigen Dateien neu übersetzt werden müssen?

### 7.6 wie funktioniert der Systemaufruf fork()?
