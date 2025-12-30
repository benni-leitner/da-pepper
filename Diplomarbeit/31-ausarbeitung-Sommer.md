# Ausarbeitung Sommer

## Aufgabenbereich und Verantwortlichkeiten

Der Schwerpunkt der Teilaufgabe von Sommer liegt auf der Konzeption und Umsetzung interaktiver Spiele für den humanoiden Roboter Pepper. Ziel dieser Teilaufgabe ist es, Besuchern der HTL Leoben eine spielerische, intuitive und stabile Möglichkeit zur Interaktion mit Pepper zu bieten. Die Spiele werden ausschließlich über das Tablet des Roboters bedient und lösen über eine ereignisbasierte Kommunikation entsprechende Reaktionen von Pepper aus.

Zusätzlich umfasst die Teilaufgabe Beiträge zum Projektmanagement sowie die Mitwirkung an der Erstellung eines ausführlichen Tutorials, das als Anleitung zur Inbetriebnahme, Nutzung und Wartung des Systems dient. Alle entwickelten Komponenten sind so gestaltet, dass sie ohne tiefgehendes technisches Vorwissen genutzt werden können und sich für den Einsatz bei Schulführungen, im Empfangsbereich sowie am Tag der offenen Tür eignen.

---

## Theoretische Grundlagen

### Tabletbasierte Benutzerinteraktion

In öffentlich zugänglichen Umgebungen wie Schulen, Messen oder Präsentationen ist eine rein sprachbasierte Interaktion mit Robotern nur eingeschränkt zuverlässig. Gründe dafür sind unter anderem Umgebungsgeräusche, unterschiedliche Dialekte sowie die begrenzte Leistungsfähigkeit der integrierten Spracherkennung. Aus diesem Grund wurde bewusst eine tabletbasierte Benutzeroberfläche als primäre Interaktionsform gewählt.

Durch die Bedienung über Touch-Eingaben können Benutzer gezielt Aktionen auslösen, ohne komplexe Sprachbefehle verwenden zu müssen. Pepper reagiert anschließend auf diese Eingaben durch Sprachausgabe, Animationen oder weitere Aktionen. Diese Form der Interaktion ist stabil, selbsterklärend und besonders für Besuchergruppen geeignet.

---

### Ereignisbasierte Kommunikation mit Pepper

Die Kommunikation zwischen der Tablet-Anwendung und Pepper erfolgt ereignisbasiert. Benutzeraktionen auf dem Tablet lösen Ereignisse aus, die über QiMessaging an den Roboter übermittelt werden. Diese Ereignisse werden im Choregraphe-Programm verarbeitet und lösen dort definierte Aktionen aus.

Technisch erfolgt dies über eine QiSession, welche die Verbindung zur Pepper-Umgebung herstellt. Über den Dienst `ALMemory` werden Ereignisse mit Nutzdaten (z.B. Text) gesendet. Diese Nutzdaten werden anschließend in Choregraphe weiterverarbeitet, beispielsweise für Sprachausgaben oder Animationen.

Dieser Ansatz bietet mehrere Vorteile:
- klare Trennung zwischen Benutzeroberfläche und Roboterlogik
- hohe Stabilität durch den Verzicht auf dauerhafte Hintergrundprozesse
- einfache Erweiterbarkeit durch zusätzliche Ereignisse

---

### Technische Einschränkungen des Pepper-Tablets

Das Tablet von Pepper basiert auf einer älteren Android-Version mit eingeschränkter WebView-Unterstützung. Moderne JavaScript-Features und Frameworks werden nur teilweise oder gar nicht unterstützt. Code, der in aktuellen Desktop-Browsern problemlos funktioniert, kann auf dem Pepper-Tablet instabil sein oder Fehler verursachen.

Aus diesem Grund wurde bewusst auf moderne Frameworks wie React, Vue oder Angular verzichtet. Stattdessen kommen einfache, kompatible JavaScript-Konstrukte zum Einsatz, die zuverlässig auf der vorhandenen Hardware ausgeführt werden können. Diese Entscheidung erhöht die Stabilität und Wartbarkeit des Systems.

---

## Überblick über die entwickelten Spiele

Im Rahmen dieser Teilaufgabe wurden mehrere interaktive Spiele umgesetzt, die vollständig über das Tablet von Pepper bedient werden können:

- Pepper sagt
- Schere Stein Papier
- Quiz
- Memory

Alle Spiele sind vollständig funktionsfähig und über eine zentrale Spieleübersicht erreichbar. Sie sind so gestaltet, dass sie ohne zusätzliche Erklärung verständlich sind und Besucher aktiv zur Interaktion einladen.

---

## Spieleübersicht

Die Datei `pepper_games.html` dient als zentrale Einstiegsseite für alle Spiele. Sie stellt ein übersichtliches Kachelmenü bereit, über welches Besucher ein Spiel auswählen können. Zusätzlich wird Pepper aktiv genutzt, um Besucher regelmäßig zur Interaktion zu animieren.

```javascript
setInterval(function () {
  session.service("ALMemory").done(function (ALMemory) {
    ALMemory.raiseEvent("Games", "Such dir ein Spiel aus!");
  });
}, 20000);
```
Dieser Code sorgt dafür, dass Pepper alle 20 Sekunden eine Sprachausgabe ausführt. Über den Dienst ALMemory wird ein Ereignis mit dem Namen Games ausgelöst. Der übergebene Text wird in Choregraphe verarbeitet und als Sprachausgabe wiedergegeben.

Diese Umsetzung hat mehrere Vorteile:
- Besucher werden aktiv angesprochen
- Pepper wirkt lebendiger und interaktiver
- keine permanente Benutzeraktion notwendig

Bei der Gestaltung der Übersicht wurde auf große Buttons, wenig Text und eine klare Farbgestaltung geachtet. Das Farbschema orientiert sich am Erscheinungsbild der HTL Leoben und sorgt für eine einheitliche Darstellung.

---

## Spiel „Pepper sagt“

### Spielidee
„Pepper sagt“ ist ein Reaktionsspiel nach dem bekannten Prinzip Simon Says. Pepper gibt Anweisungen, wobei der Spieler nur reagieren darf, wenn die Anweisung mit „Pepper sagt“ beginnt. In einer erweiterten Version werden zusätzlich visuelle Objekte wie geometrische Formen verwendet.

Das Spiel ist rundenbasiert aufgebaut und eignet sich besonders gut für kurze Interaktionen mit Besuchern.

### Umsetzung der Spiellogik

```javascript
pepperSays = Math.random() < 0.5;

var text = pepperSays
  ? "Pepper sagt: Tippe auf " + task.text
  : "Tippe auf " + task.text;

session.service("ALMemory").done(function (ALMemory) {
  ALMemory.raiseEvent("Tap", text);
});
```
Mit den obigen Zeilen wird zufällig entschieden, ob Pepper „Pepper sagt“ verwendet oder nicht und der entsprechende Text an den Roboter übermittelt.

### Zeitfenster und Fehlererkennung

```javascript
if (!pepperSays) {
  setTimeout(function () {
    if (waiting) {
      waiting = false;
      session.service("ALMemory").done(function (ALMemory) {
        ALMemory.raiseEvent(
          "Tap",
          "Richtig! Ich habe nicht 'Pepper sagt' gesagt."
        );
      });
      setTimeout(nextRound, 1500);
    }
  }, 3000);
}
```
Dieser Code überprüft, ob der Benutzer korrekt nicht reagiert hat.
## Spiel „Quiz“

Das Quiz besteht aus einem Fragenpool, der als Array im JavaScript-Code hinterlegt ist. Jede Frage enthält den Fragetext, mehrere Antwortmöglichkeiten sowie die korrekte Lösung.

Der typische Ablauf ist:
1. Anzeige einer Frage
2. Auswahl einer Antwort durch den Benutzer
3. Bewertung der Antwort
4. Rückmeldung durch Pepper
5. Laden der nächsten Frage

Durch die klare Struktur ist das Quiz leicht erweiterbar und für Besucher intuitiv nutzbar.

---

## Spiel „Memory“

Memory ist ein bekanntes Spielprinzip, bei dem Kartenpaare aufgedeckt und gefunden werden müssen. Das Spiel ist selbsterklärend und eignet sich besonders für jüngere Besucher.

Die Kartenbilder wurden im Comic-Stil mittels KI generiert, um eine freundliche und einheitliche Optik zu erreichen. Technisch wurde auf optimierte Bildgrößen und effiziente Bildformate geachtet, um Ladezeiten auf dem Pepper-Tablet gering zu halten.

---

## Entwicklungsworkflow und Werkzeuge

Die HTML-, CSS- und JavaScript-Dateien wurden mit **Notepad++** bearbeitet. Dieser Editor eignet sich besonders für einfache Webprojekte ohne zusätzliche Build-Tools und ermöglicht eine direkte Bearbeitung der Projektdateien.

Die Tests erfolgten zweistufig:
1. Logiktests in einem Desktop-Browser
2. finale Validierung auf dem Pepper-Tablet

Dabei zeigte sich, dass moderne JavaScript-Features auf dem Tablet problematisch sind, weshalb konsequent auf einfache und kompatible Lösungen gesetzt wurde.

---

## Tests und Validierung

Die entwickelten Spiele wurden vom Projektteam sowie von Mitschülern getestet. Alle Spiele sind vollständig spielbar und reagieren zuverlässig auf Benutzereingaben.

Als erfolgreich validiert gelten:
- zuverlässiger Start der Tablet-Anwendung
- fehlerfreie Ausführung der Spiele
- korrekte Darstellung auf der Tablet-Auflösung
- funktionierende Kommunikation zwischen Tablet und Pepper

Bei auftretenden Problemen konnte durch einen Neustart der Anwendung oder einen vollständigen Neustart von Pepper in den meisten Fällen eine stabile Funktion wiederhergestellt werden.

---

## Projektmanagement-Beitrag

Für die zeitliche Planung und Verwaltung der Meilensteine wurde **Microsoft Project** verwendet. Der Projektplan umfasst mehrere Phasen, darunter Analyse, Entwicklung, Test und Dokumentation.

Regelmäßige interne Zwischenmeetings dienten der Abstimmung im Team und der Kontrolle des Projektfortschritts.

---

## Tutorial-Beitrag

Ein weiterer Bestandteil der Teilaufgabe war die Mitwirkung an der Erstellung eines Tutorials. Das Tutorial wurde mit **Microsoft PowerPoint** erstellt und dient als Schritt-für-Schritt-Anleitung zur Inbetriebnahme und Anpassung von Pepper.

Behandelte Inhalte sind unter anderem:
- Auspacken und Inbetriebnahme
- Verbindung mit dem Pepper-Router
- Verbindung zu Choregraphe
- Starten der Tablet-Anwendung
- typische Fehlerquellen und deren Behebung

Das Tutorial wird sowohl gedruckt als auch als PDF bereitgestellt.

---

