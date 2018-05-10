# Kurseinheit 4

## 4.1 Was sind Race Conditions? 

## 4.1.1 Was ist ein kritischer Abschnitt eines Prozesses? 

## 4.2 Warum ist der wechselseitige Ausschlusses notwendig? (Warum ist die Synchronisation von Prozessen notwendig?)

### 4.2.1 Welche Anforderung gibt es an einen wechselseitigen Ausschluss?

## 4.3 Was sind globale Synchronisationsvariablen? 

### 4.3.1 Welche Probleme leiden sie darunter? 
(busy waiting, bestimmte Ausführungsreihenfolge, was kann passieren, wenn Prozessen bestimmte Prioritäten zugewiesen werden? )

## 4.4 Was ist ein Semaphor?

### 4.4.1 Welche Operationen sind auf einem Semaphor definiert?

### 4.4.2 Was macht ein Prozess, wenn er nicht in den kritischen Abschnitt eintreten kann?

### 4.4.3 Wie kommt ein Prozess wieder aus der Warteschlange des Semaphors?

### 4.4.4 Warum leidet das Semaphor-Verfahren nicht unter basy waiting?

### 4.4.5 Wie funktioniert die down- und up-Operation?

### 4.4.6 Wie können die atomaren down- und up-Operationen realisiert werden?

### 4.4.7 Welche Vor- und Nachteile hat ein Semaphor? 

## 4.5 Was ist ein Monitor?

### 4.5.1 Welche Vor- und Nachteile hat ein Monitor?

## 4.6 Was ist das Erzeuger/Verbraucher-Problem?

### 4.6.1 Wie kann man das Problem mit Semaphor und Monitor lösen?

## 4.7 Was ist ein Deadlock?

## 4.8 Welche vier Deadlock-Bedingungen müssen erfüllt sein, wenn ein Deadlock vorlegt? 
(Die vier Bedingungen sind notwendig, d.h. wenn ein Deadlock vorliegt,
dann müssen die vier Bedingungen erfüllt sein.
Wenn die vier Bedingungen erfüllt sind und keine zusätzliche externe Ressourcen angeschlossen werden dürfen, dann sind die vier Bedingungen auch hinreichend.)

## 4.9 Wie kann man ein Deadlock erkennen?

## 4.10. Wie kann man ein Deadlock vermeiden? 

### 4.10.1 Wie funktioniert der Bankier-Algorithmus? 

### 4.10.2 Welche Nachteile hat der Bankier-Algorithmus?

## 4.11 Wie kann man ein Deadlock verhindern? 

### 4.11.1 Sind die Methoden praktisch?

### 4.11.2 Was wird in der Praxis gemacht?
