# 3 Aufgabenstellung

## 3.1 Auftraggeber und Aufgabenstellung

### Auftraggeber

Auftraggeber dieser Diplomarbeit ist die **HTL Leoben**. Der humanoide Serviceroboter Pepper ist an der Schule bereits vorhanden und wurde in der Vergangenheit vereinzelt für Demonstrationszwecke eingesetzt. Aufgrund fehlender Erweiterbarkeit, veralteter Softwarekomponenten sowie unzureichender Dokumentation war ein dauerhafter und sinnvoller Einsatz jedoch nicht möglich.

Die Initiative zu dieser Diplomarbeit entstand aus dem Wunsch, Pepper neu aufzusetzen und für schulische Veranstaltungen wie den Tag der offenen Tür, Schulführungen oder Präsentationen nutzbar zu machen. Der Auftraggeber verspricht sich von dieser Arbeit eine moderne, stabile und selbsterklärende Lösung, die ohne tiefgehendes technisches Vorwissen bedient werden kann und langfristig an der Schule eingesetzt wird.

---

### Ausgangssituation

Zu Beginn der Diplomarbeit befand sich Pepper in einem technisch eingeschränkten Zustand. Die vorhandene Software war nicht einheitlich strukturiert und nur teilweise dokumentiert. Interaktionen mit Besuchern waren nur eingeschränkt möglich und für Außenstehende nicht selbsterklärend.

Eine zentrale Tablet-Anwendung, über welche Inhalte übersichtlich dargestellt und Interaktionen ausgelöst werden können, war nicht vorhanden. Ebenso fehlte eine klare Trennung zwischen Benutzeroberfläche, Spiellogik und Roboterverhalten. Änderungen am System erforderten umfangreiche Kenntnisse der Pepper-Softwareumgebung und waren daher nur eingeschränkt durchführbar.

Der derzeitige Arbeitsablauf war stark manuell geprägt. Für Präsentationen musste Pepper aktiv über Choregraphe gesteuert werden, was einen betreuten Betrieb erforderte. Ein teilautonomer Einsatz, beispielsweise über einen längeren Zeitraum ohne permanente Aufsicht, war nicht möglich.

---

## 3.2 Allgemeine Systembeschreibung

Ziel der entwickelten Lösung ist ein ereignisbasiertes System, bei dem die Benutzerinteraktion über eine tabletbasierte Weboberfläche erfolgt. Diese Weboberfläche wird direkt auf dem Tablet von Pepper angezeigt und dient als zentrale Schnittstelle für Besucher.

Benutzereingaben auf dem Tablet lösen definierte Ereignisse aus, welche über eine Kommunikationsschnittstelle an Pepper weitergegeben werden. In der Entwicklungsumgebung werden diese Ereignisse verarbeitet und mit entsprechenden Aktionen wie Sprachausgabe, Animationen oder Videowiedergabe verknüpft.

Die Systemarchitektur ist bewusst einfach gehalten und besteht aus klar getrennten Komponenten. Dadurch wird eine hohe Wartbarkeit sowie eine einfache Erweiterbarkeit ermöglicht. Die detaillierte technische Umsetzung und die einzelnen Teilbereiche werden in den nachfolgenden Kapiteln je Schüler ausführlich beschrieben.

---

## 3.3 Struktur der weiteren Ausarbeitungen

Die detaillierten technischen Ausarbeitungen dieser Diplomarbeit sind in eigenen Unterkapiteln je Schüler gegliedert. In diesen Kapiteln werden die jeweiligen Teilbereiche, verwendeten Technologien sowie die konkrete Umsetzung ausführlich beschrieben.

- Kapitel 3.1 behandelt die Ausarbeitung von Sommer.
- Kapitel 3.2 behandelt die Ausarbeitung von Zorn.
- Kapitel 3.3 behandelt die Ausarbeitung von Leitner.
