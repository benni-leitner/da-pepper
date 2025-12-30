# Ausarbeitung Sommer

## Aufgabenbereich und Verantwortlichkeiten

Der Teilbereich von Sommer umfasst die Konzeption, Umsetzung und Dokumentation interaktiver Spiele für den humanoiden Roboter Pepper. Ziel ist es, Besuchern der HTL Leoben eine spielerische, intuitive und technisch stabile Möglichkeit zur Interaktion mit dem Roboter zu bieten.

Die Spiele werden vollständig über das Tablet von Pepper bedient und sind so gestaltet, dass sie ohne zusätzliche Erklärung verständlich sind. Dadurch eignen sie sich besonders für den Einsatz bei Schulführungen, im Empfangsbereich sowie am Tag der offenen Tür.

Zusätzlich umfasst die Teilaufgabe Beiträge zum Projektmanagement sowie die Mitwirkung an der Erstellung eines ausführlichen Tutorials zur Inbetriebnahme, Nutzung und Fehlerbehebung.

## Zielsetzung der Teilaufgabe

Ziel dieser Teilaufgabe ist die Entwicklung eines stabilen, wartbaren und benutzerfreundlichen Spielsystems für Pepper. Dabei stehen folgende Punkte im Vordergrund:

- intuitive Touch-Bedienung
- zuverlässiger Dauerbetrieb
- klare Benutzerführung
- ereignisbasierte Kommunikation
- einfache Erweiterbarkeit
- vollständige Dokumentation

## Abgrenzung der Teilaufgabe

Nicht Bestandteil dieser Teilaufgabe ist die Entwicklung autonomer Entscheidungslogik oder fortgeschrittener künstlicher Intelligenz. Pepper reagiert ausschließlich auf definierte Ereignisse. Ebenso wird bewusst auf automatische Spracherkennung verzichtet, da diese im Schulumfeld nur eingeschränkt zuverlässig ist.

## Tabletbasierte Benutzerinteraktion

Die Interaktion mit Pepper erfolgt primär über das integrierte Tablet. Diese Entscheidung wurde getroffen, da eine rein sprachbasierte Interaktion in schulischen Umgebungen durch Umgebungsgeräusche, Dialekte und wechselnde Sprecher unzuverlässig ist.

Durch Touch-Eingaben können Benutzer gezielt Aktionen auslösen. Pepper reagiert anschließend durch Sprachausgaben oder Animationen. Diese Trennung von Eingabe und Ausgabe erhöht die Stabilität und Wartbarkeit des Systems.

## Gestaltungsprinzipien

Die Benutzeroberfläche folgt klaren Prinzipien:

- große Buttons
- wenig Text
- klare Farben
- selbsterklärende Icons
- jederzeitige Rückkehr zur Hauptseite

Diese Gestaltung ist besonders für den Einsatz bei hohem Besucheraufkommen geeignet.

## Ereignisbasierte Systemarchitektur

Die Systemarchitektur basiert auf einem ereignisgesteuerten Ansatz. Benutzeraktionen auf dem Tablet erzeugen Ereignisse, die über QiMessaging an Pepper gesendet werden. In der Entwicklungsumgebung Choregraphe werden diese Ereignisse verarbeitet und mit Aktionen verknüpft.

Durch diese Architektur entsteht eine lose Kopplung zwischen Benutzeroberfläche und Roboterverhalten. Änderungen an der Tablet-Oberfläche erfordern keine Anpassung der Roboterlogik, solange die Eventnamen unverändert bleiben.


```javascript
var session = new QiSession();
```
Diese Zeile initialisiert eine QiMessaging-Sitzung. Über diese Sitzung kann die Tablet-Webanwendung auf NAOqi-Services zugreifen. Ohne diese Sitzung ist keine Kommunikation mit Pepper möglich. Die Session wird einmal erstellt und anschließend wiederverwendet, um Verbindungsprobleme zu vermeiden.


```javascript
session.service("ALMemory").done(function (ALMemory) {
  ALMemory.raiseEvent("Games", "Such dir ein Spiel aus!");
});
```
Dieser Code greift über die bestehende QiMessaging-Sitzung auf den NAOqi-Service ALMemory zu. Sobald der Service verfügbar ist, wird mit raiseEvent ein Ereignis ausgelöst. Der Eventname lautet hier „Games“, die Nutzlast ist ein einfacher Text.

In der Entwicklungsumgebung Choregraphe wird dieses Event abgefangen und typischerweise an eine Sprachbox weitergeleitet. Dadurch spricht Pepper den übergebenen Text. Die Tablet-Anwendung selbst kennt keine Details über die Umsetzung der Sprachausgabe, sondern sendet ausschließlich das Ereignis. Diese lose Kopplung erhöht die Wartbarkeit erheblich.

```javascript
setInterval(function () {
  session.service("ALMemory").done(function (ALMemory) {
    ALMemory.raiseEvent("Games", "Such dir ein Spiel aus!");
  });
}, 20000);
```
Dieses Code-Snippet sorgt für eine periodische Sprachausgabe von Pepper. Mithilfe von setInterval wird die enthaltene Funktion alle 20 Sekunden ausgeführt. Dadurch spricht Pepper Besucher regelmäßig aktiv an, auch wenn keine direkte Benutzereingabe erfolgt.

Gerade im Empfangsbereich oder am Tag der offenen Tür ist dies wichtig, da Pepper dadurch präsenter wirkt und Besucher zur Interaktion eingeladen werden. Die Umsetzung ist ressourcenschonend und stabil, da lediglich in fixen Zeitabständen ein einzelnes Ereignis gesendet wird.

## Zentrale Spieleübersicht

Die zentrale Spieleübersicht dient als Einstiegspunkt für alle interaktiven Spiele. Sie ist so gestaltet, dass Besucher ohne Erklärung sofort erkennen, welche Möglichkeiten zur Interaktion zur Verfügung stehen.

Die Spiele werden in Form von großen Kacheln dargestellt. Jede Kachel repräsentiert ein Spiel und ist eindeutig beschriftet. Durch Antippen einer Kachel wird das jeweilige Spiel gestartet. Zusätzlich ist jederzeit eine Rückkehr zur Hauptübersicht möglich.

Pepper unterstützt die Benutzerführung aktiv durch regelmäßige Sprachausgaben. Dadurch wird die Hemmschwelle zur Nutzung gesenkt und der Roboter wirkt interaktiv und einladend.

Gestaltungsprinzipien der Übersicht sind:
- große Touch-Flächen
- wenig Text
- klare Farben
- visuelle Icons
- Orientierung am Farbschema der HTL Leoben

[Abbildung: Zentrale Spieleübersicht auf dem Pepper-Tablet]

## Spiel „Pepper sagt“ – Konzept

Das Spiel „Pepper sagt“ basiert auf dem bekannten Prinzip „Simon Says“. Pepper gibt Anweisungen, denen der Benutzer nur dann folgen darf, wenn die Anweisung mit der Phrase „Pepper sagt“ beginnt.

Das Spiel ist rundenbasiert aufgebaut und eignet sich besonders für kurze Interaktionen. Durch die zufällige Abfolge von korrekten und irreführenden Anweisungen bleibt das Spiel abwechslungsreich und fordert die Aufmerksamkeit der Spieler.

Zusätzlich werden visuelle Elemente wie geometrische Formen eingesetzt, um die Aufgaben auch ohne Zuhören verständlich zu machen. Dadurch ist das Spiel für unterschiedliche Altersgruppen geeignet.

[Abbildung: Spiel „Pepper sagt“ – Startansicht]

```javascript
pepperSays = Math.random() < 0.5;
```
Diese Zeile erzeugt die Zufallskomponente des Spiels. Math.random liefert eine Zufallszahl zwischen 0 und 1. Ist diese kleiner als 0,5, wird pepperSays auf wahr gesetzt, andernfalls auf falsch.

Damit wird zufällig entschieden, ob Pepper in der aktuellen Runde tatsächlich „Pepper sagt“ verwendet oder nicht. Diese Zufälligkeit verhindert, dass Spieler das Muster vorhersagen können, und sorgt für einen langfristig hohen Spielreiz.

```javascript
var text = pepperSays
  ? "Pepper sagt: Tippe auf " + task.text
  : "Tippe auf " + task.text;
```
Hier wird der gesprochene Text für die aktuelle Runde erzeugt. Mithilfe eines ternären Operators wird abhängig vom Wert von pepperSays entschieden, ob die Phrase „Pepper sagt“ im Text enthalten ist oder nicht.

task.text enthält den konkreten Aufgabentext, beispielsweise den Namen einer Form. Dadurch entstehen dynamische Anweisungen, die sich pro Runde unterscheiden und automatisch generiert werden.

```javascript
session.service("ALMemory").done(function (ALMemory) {
  ALMemory.raiseEvent("Tap", text);
});
```
Dieses Code-Snippet sendet die zuvor erzeugte Anweisung an Pepper. Das Event trägt den Namen „Tap“ und enthält den gesprochenen Text als Nutzlast.

In Choregraphe wird dieses Event empfangen und zur Sprachausgabe verwendet. Zusätzlich können dort Animationen oder Gesten ergänzt werden, ohne den HTML- oder JavaScript-Code ändern zu müssen.

```javascript
if (!pepperSays) {
  setTimeout(function () {
    if (waiting) {
      waiting = false;
      session.service("ALMemory").done(function (ALMemory) {
        ALMemory.raiseEvent("Tap", "Richtig! Ich habe nicht 'Pepper sagt' gesagt.");
      });
      setTimeout(nextRound, 1500);
    }
  }, 3000);
}
```
Dieses Snippet implementiert die Zeitfenster-Logik des Spiels. Wenn Pepper nicht „Pepper sagt“ gesagt hat, ist die korrekte Spielerreaktion, nicht zu tippen.

Nach drei Sekunden wird geprüft, ob der Benutzer korrekt nicht reagiert hat. Ist dies der Fall, gibt Pepper ein positives Feedback aus. Anschließend startet nach einer kurzen Pause automatisch die nächste Runde.

Die Logik verwendet bewusst einfache JavaScript-Konstrukte wie setTimeout und Zustandsvariablen, um eine stabile Ausführung auf dem Pepper-Tablet zu gewährleisten.

## Quiz – Konzept und Ablauf

Das Quiz ist ein klassisches Multiple-Choice-Spiel. Besucher beantworten Fragen durch Antippen einer Antwort. Das System bewertet die Antwort sofort und gibt Feedback.

Anforderungen im Besucherbetrieb sind:
- kurze, verständliche Fragen
- große Antwortbuttons
- sofortige Rückmeldung
- linearer Ablauf
- akustisches Feedback durch Pepper

[Abbildung: Quiz – Frage mit Antwortbuttons]

```javascript
var questions = [
  { q: "Wofür steht HTL?", a: ["Höhere Technische Lehranstalt", "Hochschule"], correct: 0 },
  { q: "Wie bedient man Pepper?", a: ["Über das Tablet", "Nur per Sprache"], correct: 0 }
];
```
Der Fragenpool wird als Array von Objekten umgesetzt. Jedes Objekt enthält den Fragetext, ein Array mit Antwortmöglichkeiten sowie den Index der korrekten Antwort.

Diese Struktur ist einfach, gut lesbar und leicht erweiterbar. Neue Fragen können durch Hinzufügen weiterer Objekte ergänzt werden, ohne bestehende Logik anzupassen.

```javascript
var current = 0;

function renderQuestion() {
  document.getElementById("question").textContent = questions[current].q;
  document.getElementById("a0").textContent = questions[current].a[0];
  document.getElementById("a1").textContent = questions[current].a[1];
}
```
Diese Funktion stellt die aktuelle Frage und die dazugehörigen Antwortmöglichkeiten dar. Über die Variable current wird gesteuert, welche Frage aktiv ist.

Die Inhalte werden direkt in die entsprechenden HTML-Elemente geschrieben. Diese DOM-basierte Lösung wurde gewählt, da moderne Frameworks auf dem Pepper-Tablet nicht zuverlässig funktionieren.

```javascript
function chooseAnswer(idx) {
  var ok = (idx === questions[current].correct);

  session.service("ALMemory").done(function (ALMemory) {
    ALMemory.raiseEvent("quiz", ok ? "Richtig!" : "Leider falsch!");
  });

  current++;
  if (current >= questions.length) current = 0;
  setTimeout(renderQuestion, 800);
}
```
Dieses Snippet bewertet die ausgewählte Antwort. Der Index der gedrückten Antwort wird mit dem gespeicherten correct-Index verglichen.

Das Ergebnis wird über ein Event an Pepper gesendet, sodass er akustisches Feedback geben kann. Danach wird zur nächsten Frage gewechselt. Nach der letzten Frage beginnt das Quiz wieder von vorne, was einen kontinuierlichen Spielbetrieb ermöglicht.

## Memory – Konzept

Memory ist ein bekanntes Spielprinzip, bei dem Kartenpaare aufgedeckt und gefunden werden müssen. Das Spiel ist selbsterklärend und eignet sich besonders für Besucher unterschiedlicher Altersgruppen.

Die Kartenbilder wurden im Comic-Stil mittels KI generiert, um eine freundliche und einheitliche Optik zu erzielen. Technisch wurde darauf geachtet, Bildgrößen und Dateiformate zu optimieren, um Ladezeiten auf dem Pepper-Tablet gering zu halten.

[Abbildung: Memory-Spielfeld mit verdeckten Karten]

```javascript
var cards = ["a","a","b","b","c","c","d","d"];
for (var i = cards.length - 1; i > 0; i--) {
  var j = Math.floor(Math.random() * (i + 1));
  var tmp = cards[i];
  cards[i] = cards[j];
  cards[j] = tmp;
}
```
Dieses Snippet zeigt die Datenbasis für das Memory-Spiel sowie die Zufallsdurchmischung der Karten. Die Karten sind paarweise im Array enthalten und werden mithilfe des Fisher-Yates-Algorithmus gemischt.

Diese Methode ist effizient, leicht verständlich und funktioniert zuverlässig auf dem Pepper-Tablet. Durch das Mischen wird sichergestellt, dass jede Spielrunde unterschiedlich ist.

## Entwicklungsworkflow und Tools

Die HTML-, CSS- und JavaScript-Dateien wurden mit Notepad++ bearbeitet. Der Editor ist ressourcenschonend und eignet sich gut für einfache Webprojekte ohne Build-Systeme.

Getestet wurde zweistufig:
- Logiktests am PC
- Kompatibilitäts- und Performancetests auf dem Pepper-Tablet

Dabei zeigte sich, dass moderne JavaScript-Features auf dem Tablet problematisch sind. Deshalb wurde konsequent auf einfache, kompatible Logik gesetzt.

## Tests und Validierung

Die Spiele wurden durch das Projektteam sowie durch Mitschüler getestet. Der geplante Einsatz erfolgt im Empfangsbereich, bei Schulführungen, am Tag der offenen Tür sowie bei Präsentationen.

Als erfolgreich validiert gelten:
- zuverlässiger Start der Tablet-Anwendung
- vollständige Spielbarkeit
- korrektes Layout bei 1280×800
- stabile QiMessaging-Kommunikation
- verständliche Sprachausgabe

Bekannte Probleme sind gelegentliche Performance-Einbußen des Tablets und vereinzelte Abstürze. Diese können in der Praxis meist durch einen Neustart der App oder einen vollständigen Neustart von Pepper behoben werden.

## Projektmanagement-Beitrag

Sommer war am Projektmanagement beteiligt, insbesondere an der Zeitplanung, Meilensteinverwaltung und internen Abstimmung. Für die Planung wurde Microsoft Project verwendet.

Der Projektplan umfasst Analyse, Entwicklung, Test und Dokumentation. Regelmäßige Zwischenmeetings dienten der Fortschrittskontrolle und Koordination der Teammitglieder.

[Abbildung: Projektplan in Microsoft Project]

## Tutorial-Beitrag

Das Tutorial wurde mit Microsoft PowerPoint erstellt und dient als Schritt-für-Schritt-Anleitung zur Inbetriebnahme von Pepper. Inhalte sind unter anderem Auspacken, Netzwerkanbindung, Verbindung über Choregraphe, Start der Tablet-App sowie Troubleshooting.

Das Tutorial wird sowohl gedruckt als auch als PDF bereitgestellt und ermöglicht eine einfache Nutzung des Systems ohne tiefgehende technische Vorkenntnisse.

[Abbildung: Screenshot aus dem Tutorial]

## Zusammenfassung

Die Teilaufgabe Sommer umfasst die Entwicklung interaktiver Tablet-Spiele, die ereignisbasierte Anbindung an Pepper, Beiträge zum Projektmanagement sowie die Erstellung eines praxisnahen Tutorials.

Durch den Einsatz einfacher Webtechnologien und einer klaren Systemarchitektur konnte eine stabile, selbsterklärende Lösung geschaffen werden, die sich ideal für den Einsatz an der HTL Leoben eignet.






