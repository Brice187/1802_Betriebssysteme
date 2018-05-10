# Kurseinheit 4

## 4.1 Was sind Race Conditions?

## 4.1.1 Was ist ein kritischer Abschnitt eines Prozesses?

Kritische Abschnitte sind Abschnitte, die lesend und/oder schreibend gemeinsame Daten verarbeiten, die von mehreren konkurrierenden Prozessen genutzt werden. Damit kritische Abschnitte determinierte Resultate zur Folge haben, müssen solche Abschnitte ohne Störung durch die konkurrierenden Prozesse in einer Sequenz ausgeführt werden.
Innerhalb eines kritischen Abschnitts greift ein Prozess exklusiv auf gemeinsam benutze Betriebsmittel zu.
Nicht gemeinsam benutzbare Betriebsmittel (z.B. Drucker, Diskettenlaufwerk) können von konkurrierenden Prozessen nur in kritischen Abschnitten belegt werden.

## 4.2 Warum ist der wechselseitige Ausschlusses notwendig? (Warum ist die Synchronisation von Prozessen notwendig?)

Damit die Resultate von Abläufen konkurrierender Prozesse determiniert bleiben, müssen deren kritische Abschnitte exklusiv ausgeführt werden, d.h. EIN kritischer Abschnitt zu EINER Zeit. Das Betreten und Verlassen eines kritischen Abschnittes muss abgestimmt (*synchronisiert*) sein. Es muss folglich zwei Funktionen *enter_critical_region* und *exit_critical_region* geben, die das Betreten und Verlassen des kritischen Abschnittes regeln (=>wechselseitiger Ausschluss).

### 4.2.1 Welche Anforderung gibt es an einen wechselseitigen Ausschluss?

1. **EIN** Prozess zu **EINER** Zeit in einem kritischen Abschnitt.
2. Falls mehrere Prozesse gleichzeitig versuchen, den kritischen Abschnitt zu betreten, gibt es in **endlicher Zeit** eine Entscheidung, wer zuerst darf.
3. Ein Prozess, der eintreten möchte, darf nicht beliebig oft von anderen überholt werden, so dass er verhungert. Die Wartezeit eines Prozesses sollte **fair** sein.
4. Kein Prozess darf **außerhalb** eines kritischen Abschnittes einen anderen Prozess **blockieren**.
5. Ein Algorithmus zur Realisierung des wechselseitigen Ausschlusses darf nicht auf irgendwelchen Annahmen über die **relative oder absolute Ausführungsgeschwindigkeit** der Prozesse beruhen.

## 4.3 Was sind globale Synchronisationsvariablen?

Eine globale Variable **s**, mit welcher ein Prozess den kritischen Abschnitt für den jeweils anderen freigibt.

Nachteil:
* Die kritischen Abschnitte können nur abwechselnd 1,2,1,2,... ablaufen -> kein Überrunden möglich
* Ein Prozess kann nicht selbst zweimal hintereinander den gleichen kritischen Abschnitt aufrufen.

Lösungsmöglichkeit:
* zwei Synchronisationsvariablen verwenden (Neuer Nachteil: Gefahr eines Deadlock)

### 4.3.1 Welche Probleme leiden sie darunter?

Wenn sich ein Prozess in seinem kritischen Abschnitt befindet, sollte kein anderer Prozess ständig nachfragen, ob er in den kritischen Abschnitt eintreten darf (busy waiting -> unwirtschaftlich, da unproduktive Prozessorzeit benötigt wird)

(busy waiting, bestimmte Ausführungsreihenfolge, was kann passieren, wenn Prozessen bestimmte Prioritäten zugewiesen werden? )

## 4.4 Was ist ein Semaphor?

Ein Prozess, der in krit. Abschnitt eintreten möchte, wird deaktiviert und wieder aufgeweckt, wenn der derzeitige aktive Prozess seinen kritischen Abschnitt verlässt.

Eine Semaphor *s* ist eine Variable, die einen ganzzahligen Wert hat. Mit Hilfe dieses Semaphores wird eine bestimmte kritische Ressource überwacht.
Initialisiert man *s* mit *k*, so werden höchstens *k* gleichzeitige Zugriffe auf diese Ressource zugelassen => meistens ist **k=1**.

### 4.4.1 Welche Operationen sind auf einem Semaphor _s_ definiert?

* **down(s)**: bedeutet, dass der aktuelle Wert der Semaphore *s* um 1 vermindert wird. Wird der Wert negativ, blockiert der Prozess, der down ausführt. (_wird **vor** krit. Abschnitt ausgeführt_)
* **up(s)** erhöht den Wert von s um 1. Ist der neue Wert nicht positiv, wird ein durch eine down-Operation blockierter Prozess freigegeben, er geht folglich vom Zustand blockiert in bereit über. (_wird **nach** krit. Abschnitt ausgeführt_)

### 4.4.2 Was macht ein Prozess, wenn er nicht in den kritischen Abschnitt eintreten kann?

### 4.4.3 Wie kommt ein Prozess wieder aus der Warteschlange des Semaphors?

### 4.4.4 Warum leidet das Semaphor-Verfahren nicht unter basy waiting?

### 4.4.5 Wie funktioniert die down- und up-Operation?

### 4.4.6 Wie können die atomaren down- und up-Operationen realisiert werden?

### 4.4.7 Welche Vor- und Nachteile hat ein Semaphor?

## 4.5 Was ist ein Monitor?

Synchronisation kann auch herbeigeführt werden, wenn ein spezieller Prozess damit beauftragt wird, den Zutritt zu kritischen Abschnitten zu kontrollieren. Dies ist das Konzept des Monitors:

*Unter einem Monitor versteht man eine Menge von Prozeduren, Variablen und Datenstrukturen, die als Betriebsmittel betrachtet und mehreren Prozessen zugänglich gemacht sind. Die gemeinsam genutzten Betriebsmittel können geschützt werden, indem sie im Monitor platziert werden.*

Synchronisation durch Bedingungsvariablen, auf die zwei Funktionen wirken:
* **wait(c)**: Prozess wird blockiert, wenn c nicht erfüllt ist; Prozess kommt in die Warteschlange der Bedingung c
* **signal(c)**: Wenn in einer Monitorprozedur die Bedingung c wahr wird, dann wird die signal-Operation aufgerufen. Ein wartender Prozess wird ein aktiver.

Analogie: Ein Raum, zu dem es nur einen einzigen bewachten Eingang gibt, so dass sich immer nur ein Prozess im Raum befinden kann. Andere Prozesse geraten in eine Warteschlange für blockierte Prozesse, die auf die Verfügbarkeit des Monitors (Änderung der Bedingung c) warten

### 4.5.1 Welche Vor- und Nachteile hat ein Monitor?

## 4.6 Was ist das Erzeuger/Verbraucher-Problem?

Zwei zyklische Prozesse *Erzeuger* und *Verbraucher*(z.B. E/A-Vorgänge) produzieren, bzw. konsumieren Ware. Es ist eine Einweg-Kommunikation vorgesehen, wobei der Austausch der Ware über einen Puffer stattfindet.

Der gleichzeitige Zugriff von Erzeuger und Verbraucher auf den Puffer kann jedoch zu Störungen führen.

### 4.6.1 Wie kann man das Problem mit Semaphor und Monitor lösen?

## 4.7 Was ist ein Deadlock?

Ein Deadlock ist eine **Systemverklemmung**.

Allgemein: ein Zustand von Prozessen, bei dem mindestens zwei Prozesse auf Betriebsmittel warten, die einem jeweils anderen beteiligten Prozess zugeteilt sind.
Beispiele: Buchausleihe (A liest a und benötigt b; B liest b und benötigt a)

## 4.8 Welche vier Deadlock-Bedingungen müssen erfüllt sein, wenn ein Deadlock vorliegt?
(Die vier Bedingungen sind notwendig, d.h. wenn ein Deadlock vorliegt,
dann müssen die vier Bedingungen erfüllt sein.
Wenn die vier Bedingungen erfüllt sind und keine zusätzliche externe Ressourcen angeschlossen werden dürfen, dann sind die vier Bedingungen auch hinreichend.)

1. **Wechselseitiger Ausschluss** (Es kann zu einem Zeitpunkt nur ein Prozess ein Betriebsmittel nutzen)
2. **Nicht-Unterbrechbarkeit (no preemption)**: Die Betriebsmittel können temporär nicht zurückgegeben werden, sondern bleiben dem Prozess bis zum Ende der Anforderung zugeordnet.
3. **Halte-und-Warte (Hold-and-wait)**: Die Prozesse belegen exklusiv ein Betriebsmittel während sie noch auf weitere Betriebsmittel warten.
4. **Zyklische Warte-Bedingung (circular wait)**: Es besteht eine geschlossene Kette aus Prozessen und Betriebsmitteln in der Weise, dass jeder Prozess ein oder mehrere Betriebsmittel belegt, die vom nächsten Prozess in der Kette benötigt werden.

## 4.9 Wie kann man ein Deadlock erkennen?

Das Betriebssystem führt regelmäßig einen Algorithmus zur Erkennung von Deadlocks aus, der das zyklische Warten erkennt.

Dieser sucht nach einem Prozess, dessen Anforderungen durch die momentan verfügbaren Betriebsmittel erfüllt werden können. Im vorderen Teil des Algorithmus werden zunächst die Variablen initialisiert. Der wesentliche Teil des Algorithmus ist die *repeat-Schleife*. Bei jedem Durchlauf werden alle Prozesse, die potentiell noch im Deadlock enthalten sind, daraufhin untersucht, ob ihre Restanforderung durch die noch vorhandenen Betriebsmittel erfüllt werden kann. Falls ein solcher Prozess gefunden wird, wird dieser als beendbar markiert und die von ihm belegten Betriebsmittel können freigegeben werden. Die repeat-Schleife wird so lange wiederholt, wie weitere beendbare Prozesse gefunden werden. Nur wenn alle Prozesse als beendbar markiert sind, dann liegt keine Verklemmung vor, ansonsten bilden die nicht beendbaren Prozesse einen Deadlock-Zyklus.

## 4.10. Wie kann man ein Deadlock vermeiden?

Vermeidung von Deadlocks bedeutet, dass Informationen über zukünftige Betriebsmittel-Anforderungen genutzt werden, um zu versuchen, nur solche Betriebsmittel-Vergaben zuzulassen, bei denen nicht die Gefahr einer Verklemmung besteht.

### 4.10.1 Wie funktioniert der Bankier-Algorithmus?

Wie einem Bankier nur eine begrenzte Anzahl an Geld zur Verfügung steht, um die Kreditwünsche seiner Kunden zu erfüllen, so steht einem Betriebssystem auch nur eine begrenzte Anzahl von Betriebsmitteln zur Verfügung. Der Bankier hält deswegen immer noch so viel Geld in seinem Tresor zurück, damit er noch von mindestens einem Kunden das komplette Kreditlimit erfüllen kann. Dieser eine Kunde (Prozess) kann dann sein Geschäft erfolgreich zum Abschluss bringen und das verwendete Geld wieder zurück auf die Bank bringen. Nun kann es ein anderer Kunde haben.

### 4.10.2 Welche Nachteile hat der Bankier-Algorithmus?

Keine endgültige Lösung des Deadlock-Problems, da die maximalen Anforderungen eines Prozesses kaum im Voraus verfügbar sind.

## 4.11 Wie kann man ein Deadlock verhindern?

Bei der Verhinderung von Deadlocks wird versucht, zumindest eine der vier Deadlock-Bedingungen niemals erfüllen zu lassen.

### 4.11.1 Sind die Methoden praktisch?

1. **Bedingung** scheidet aus, da die wechselseitiger Ausschluss i.d.R. erforderlich ist
2. **Bedingung** Möglich durch Ressourcenentzug: Prozess müsste alle Betriebsmittel zurückgeben und diese wieder zusammen mit dem neuen Betriebsmittel anfordern -> würde funktionieren, ist aber nicht immer praktikabel, da z.B. einem Prozess ein Ausgabegerät nicht entzogen werden kann, auf das er gerade etwas ausgibt.
3. **Bedingung** Prozess müsste alle in Zukunft benötigten Betriebsmittel von Anfang an anfordern (alle oder keines).
   * Nachteil: Lösung ist nicht effizient und man kann nicht immer im Voraus sehen, wie viele Ressourcen benötigt werden.
4. **Bedingung** lineare Ordnung der Betriebsmittel ("wichtiger als") -> nur weniger wichtige Betriebsmittel können nachträglich angefordert werden; funktioniert, da die weniger wichtigen Betriebsmittel nur von Prozessen blockiert werden können, die auf noch weniger wichtige Betriebsmittel warten.
   * Nachteil: Eine vernünftige Ordnung für alle Ressourcen kann man kaum finden.

### 4.11.2 Was wird in der Praxis gemacht?
