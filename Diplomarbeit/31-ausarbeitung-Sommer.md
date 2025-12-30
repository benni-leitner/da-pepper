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

## Entwicklungsworkflow und verwendete Tools

Für die Umsetzung der Tablet-Anwendungen sowie der dazugehörigen Logik wurde bewusst auf einen einfachen und transparenten Entwicklungsworkflow gesetzt. Ziel war es, ein System zu schaffen, das ohne komplexe Build-Prozesse oder zusätzliche Abhängigkeiten auskommt und direkt auf dem Pepper-Tablet lauffähig ist.

### Webentwicklung (HTML, CSS, JavaScript)

Die Benutzeroberflächen der Spiele sowie der zentralen Spieleübersicht wurden mit den Webtechnologien HTML, CSS und JavaScript umgesetzt. Diese Technologien eignen sich besonders gut für die Entwicklung einfacher, interaktiver Benutzeroberflächen und werden vom Pepper-Tablet grundsätzlich unterstützt.

Bei der Entwicklung wurde darauf geachtet, ausschließlich grundlegende und kompatible JavaScript-Konstrukte zu verwenden. Moderne JavaScript-Features oder Frameworks wurden bewusst vermieden, da das Tablet von Pepper auf einer älteren Android-Version mit eingeschränkter WebView-Unterstützung basiert. Funktionen, die in modernen Desktop-Browsern problemlos laufen, können auf dem Pepper-Tablet zu Darstellungsfehlern, Performanceproblemen oder Abstürzen führen.

Die Gestaltung der Benutzeroberflächen erfolgte mit einfachem CSS. Dabei wurden große Buttons, klare Farben und übersichtliche Layouts verwendet, um eine sichere Touch-Bedienung zu gewährleisten. Das Farbschema orientiert sich am Erscheinungsbild der HTL Leoben, um einen einheitlichen Gesamteindruck zu erzeugen.

### Bearbeitung der Dateien mit Notepad++

Die HTML-, CSS- und JavaScript-Dateien wurden mit dem Texteditor Notepad++ bearbeitet. Dieser Editor wurde gewählt, da er ressourcenschonend ist, keine zusätzlichen Abhängigkeiten benötigt und sich gut für einfache Webprojekte eignet.

Notepad++ ermöglicht eine direkte Bearbeitung der Projektdateien im HTML-Ordner der Choregraphe-Anwendung. Dadurch konnten Änderungen schnell getestet werden, ohne einen zusätzlichen Build- oder Deployment-Prozess durchführen zu müssen. Besonders im Zusammenspiel mit der eingeschränkten Hardware von Pepper erwies sich dieser einfache Workflow als vorteilhaft.

### Choregraphe als Entwicklungsumgebung

Für die Steuerung des Roboters und die Verarbeitung der vom Tablet gesendeten Ereignisse wurde die Entwicklungsumgebung Choregraphe verwendet. Choregraphe ist die offizielle grafische Entwicklungsumgebung für Pepper und basiert auf dem NAOqi-Framework.

In Choregraphe wurden die sogenannten Show-Apps erstellt, die beim Start der Anwendung automatisch ausgeführt werden. Diese Show-Apps übernehmen unter anderem folgende Aufgaben:

- Laden der Tablet-Webanwendung
- Anzeigen der Weboberfläche auf dem Tablet
- Entgegennehmen von QiMessaging-Ereignissen
- Weiterleitung der Ereignisse an Sprach- oder Animationsboxen

Zusätzlich wurden in Choregraphe vordefinierte Animationen, Gesten und Augen-Emotes verwendet. Diese sind bereits in der Entwicklungsumgebung enthalten und können ohne zusätzliche Programmierung eingebunden werden.

### Python-Skripte innerhalb von Choregraphe

Innerhalb von Choregraphe wurden Python-Skripte eingesetzt, um bestimmte Abläufe zu steuern. Dazu zählen unter anderem:

- Starten der Tablet-Webanwendung
- Verarbeiten von Ereignissen aus ALMemory
- Steuern der Sprachausgabe über ALTextToSpeech

Python eignet sich besonders gut für diese Aufgaben, da es die native Programmiersprache von NAOqi ist und direkt auf die internen Dienste von Pepper zugreifen kann. Die verwendeten Skripte sind bewusst einfach gehalten und verzichten auf komplexe Logik, um die Stabilität im Dauerbetrieb zu gewährleisten.

### QiMessaging als Kommunikationsschnittstelle

Die Kommunikation zwischen der Tablet-Webanwendung und Pepper erfolgt über QiMessaging. QiMessaging stellt eine Schnittstelle bereit, über die JavaScript-Code auf dem Tablet mit den NAOqi-Diensten des Roboters kommunizieren kann.

Über QiMessaging wird eine Sitzung aufgebaut, über die anschließend auf Dienste wie ALMemory zugegriffen wird. Benutzeraktionen auf dem Tablet lösen Ereignisse aus, die über ALMemory an Choregraphe übermittelt werden. Dort werden sie weiterverarbeitet, beispielsweise für Sprachausgaben oder Animationen.

Dieser Ansatz ermöglicht eine klare Trennung zwischen Benutzeroberfläche und Roboterlogik. Die Tablet-Seite kennt lediglich die Namen der Ereignisse, nicht jedoch die konkrete Umsetzung der Reaktion von Pepper.

### Teststrategie

Die Tests wurden bewusst zweistufig durchgeführt:

- Logiktests am PC  
  Die grundlegende Spiellogik, Navigation und Darstellung wurden zunächst in einem Desktop-Browser getestet. Dies ermöglichte ein schnelles Debugging und eine effiziente Weiterentwicklung.

- Kompatibilitäts- und Performancetests auf dem Pepper-Tablet  
  Anschließend wurden alle Funktionen direkt auf dem Pepper-Tablet getestet. Diese Tests waren entscheidend, da sich hier Einschränkungen der WebView und der Hardware deutlich bemerkbar machten.

Ein zentrales Ergebnis dieser Tests war die Erkenntnis, dass moderne JavaScript-Konstrukte und komplexe Animationen auf dem Tablet problematisch sind. Daher wurde konsequent auf einfache, robuste Lösungen gesetzt.

## Tests und Validierung

Die entwickelten Spiele wurden sowohl vom Projektteam als auch von Mitschülerinnen und Mitschülern getestet. Ziel dieser Tests war es, die Alltagstauglichkeit des Systems unter realistischen Bedingungen zu überprüfen.

Der geplante Einsatzbereich umfasst:

- den Empfangsbereich der Schule
- Schulführungen
- den Tag der offenen Tür
- Messen und Präsentationen

### Abnahmekriterien

Als erfolgreich validiert gelten folgende Kriterien:

- die Tablet-Anwendung startet zuverlässig
- alle Spiele sind vollständig spielbar
- das Layout ist an die Tablet-Auflösung von 1280×800 angepasst
- die QiMessaging-Kommunikation funktioniert stabil
- Pepper reagiert korrekt auf Tablet-Eingaben
- die Sprachausgabe ist gut verständlich

### Bekannte Probleme und Umgang damit

Im praktischen Betrieb traten vereinzelt folgende Probleme auf:

- verzögerte Reaktion des Tablets
- eingeschränkte Performance bei längerer Laufzeit
- vereinzelte Abstürze der Tablet-Anwendung
- zeitweise unzuverlässige Sensorik

Diese Probleme sind größtenteils hardwarebedingt. In der Praxis konnten sie meist durch folgende Maßnahmen behoben werden:

- Neustart der Tablet-Anwendung
- erneutes Verbinden über Choregraphe
- vollständiger Neustart von Pepper

Ein vollständiger Neustart von Pepper behebt in der Praxis den Großteil der auftretenden Probleme. Wichtig ist, dass dabei kein Datenverlust auftritt und das System nach dem Neustart wieder vollständig einsatzbereit ist.

## Projektmanagement-Beitrag

Sommer war aktiv am Projektmanagement beteiligt. Zu den Aufgaben zählten insbesondere die Zeitplanung, die Verwaltung von Meilensteinen sowie die Koordination innerhalb des Projektteams.

Für die Planung wurde Microsoft Project verwendet. Mit diesem Werkzeug konnten Aufgaben, Abhängigkeiten und Meilensteine übersichtlich in Form eines Gantt-Diagramms dargestellt werden. Der Projektplan gliedert sich in mehrere Phasen, darunter Analyse, Entwicklung, Test und Dokumentation.

Regelmäßige interne Zwischenmeetings dienten der Abstimmung des Projektfortschritts und der frühzeitigen Identifikation von Problemen. Gerade bei der Arbeit mit Hardware wie Pepper erwies sich eine strukturierte Planung als besonders wichtig, da Tests und Neustarts zeitaufwendig sein können.

[Abbildung: Projektplan und Meilensteine in Microsoft Project]

## Tutorial-Beitrag

Ein weiterer wesentlicher Bestandteil der Teilaufgabe war die Mitwirkung an der Erstellung eines Tutorials. Das Tutorial wurde mit Microsoft PowerPoint erstellt, da dieses Werkzeug eine schnelle und flexible Gestaltung von Anleitungen ermöglicht.

Das Tutorial dient als Schritt-für-Schritt-Anleitung zur Inbetriebnahme und Nutzung von Pepper. Es richtet sich auch an Personen ohne tiefgehende technische Vorkenntnisse.

Behandelte Inhalte des Tutorials sind unter anderem:

- Auspacken und grundlegende Inbetriebnahme
- Netzwerkanbindung über den Pepper-Router
- Verbindung zu Pepper über Choregraphe
- Starten der Tablet-Anwendung
- Hinweise zur Anpassung bei aktiver Entwicklung
- Troubleshooting bei typischen Fehlern

Das Tutorial wird sowohl in gedruckter Form als auch als PDF bereitgestellt. Dadurch ist es flexibel einsetzbar und kann bei Bedarf leicht aktualisiert werden.

[Abbildung: Screenshot aus dem Tutorial – Verbindung mit Pepper]


## Zusammenfassung

Die Teilaufgabe Sommer umfasst die Entwicklung interaktiver Tablet-Spiele, die ereignisbasierte Anbindung an Pepper, Beiträge zum Projektmanagement sowie die Erstellung eines praxisnahen Tutorials.

Durch den Einsatz einfacher Webtechnologien und einer klaren Systemarchitektur konnte eine stabile, selbsterklärende Lösung geschaffen werden, die sich ideal für den Einsatz an der HTL Leoben eignet.






