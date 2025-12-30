# Ausarbeitungen

## Auftraggeber

Auftraggeber dieser Diplomarbeit ist die **HTL Leoben**. Der humanoide Serviceroboter Pepper ist an der Schule bereits vorhanden und wurde in der Vergangenheit vereinzelt für Demonstrationszwecke eingesetzt. Aufgrund fehlender Erweiterbarkeit, veralteter Softwarekomponenten sowie unzureichender Dokumentation war ein dauerhafter und sinnvoller Einsatz jedoch nur eingeschränkt möglich.

Die Diplomarbeit wurde in Zusammenarbeit mit den betreuenden Lehrkräften initiiert, um Pepper technisch zu überarbeiten und für schulische Veranstaltungen wie den Tag der offenen Tür, Schulführungen oder Präsentationen nutzbar zu machen. Der Auftraggeber erwartet sich eine stabile, verständliche und langfristig wartbare Lösung, die ohne tiefgehendes technisches Vorwissen bedient werden kann.

---

## Aufgabenstellung

Ziel der Ausarbeitungen ist die Entwicklung einer interaktiven Softwarelösung für Pepper, die Besucher selbstständig nutzen können. Dabei sollen Informationen über die Schule vermittelt und gleichzeitig eine spielerische Interaktion ermöglicht werden.

Die Aufgabenstellung umfasst unter anderem:

- Konzeption und Umsetzung einer tabletbasierten Benutzeroberfläche  
- Entwicklung interaktiver Spiele für Besucher  
- Präsentation der Ausbildungszweige der HTL Leoben  
- Anbindung der Tablet-Anwendung an Pepper über eine ereignisbasierte Kommunikation  
- Sicherstellung eines stabilen Offline-Betriebs  
- Erstellung eines verständlichen Tutorials zur Inbetriebnahme und Anpassung  

---

## Ausgangssituation

Zu Beginn der Diplomarbeit befand sich Pepper in einem technisch eingeschränkten Zustand. Die vorhandene Software war nicht einheitlich aufgebaut und nur teilweise dokumentiert. Interaktionen mit Besuchern waren nur eingeschränkt möglich und erforderten meist eine aktive Betreuung.

Eine zentrale Tablet-Anwendung, über welche Besucher selbstständig mit Pepper interagieren können, war nicht vorhanden. Ebenso fehlte eine klare Trennung zwischen Benutzeroberfläche, Spiellogik und Roboterverhalten. Änderungen am System waren daher zeitaufwendig und fehleranfällig.

---

## Allgemeine Systembeschreibung

Die entwickelte Lösung basiert auf einer klaren Trennung der einzelnen Systemkomponenten. Die Benutzerinteraktion erfolgt über eine Weboberfläche, die direkt auf dem Tablet von Pepper angezeigt wird. Diese Weboberfläche dient als zentrale Schnittstelle für alle Inhalte und Spiele.

Benutzereingaben auf dem Tablet lösen definierte Ereignisse aus, die an Pepper weitergegeben werden. In der Entwicklungsumgebung werden diese Ereignisse verarbeitet und mit entsprechenden Aktionen wie Sprachausgabe, Animationen oder Videowiedergabe verknüpft.

Die Architektur ist bewusst einfach gehalten, um Stabilität, Wartbarkeit und Erweiterbarkeit sicherzustellen.

---

## Gliederung der Teilaufgaben

Die konkreten technischen Ausarbeitungen dieser Diplomarbeit sind nach Schülern gegliedert. Jeder Schüler bearbeitet einen klar abgegrenzten Teilbereich des Gesamtsystems.

- Die Ausarbeitung von **Sommer** umfasst die Entwicklung der interaktiven Spiele, Beiträge zum Projektmanagement sowie die Erstellung des Tutorials.  
- Die Ausarbeitung von **Zorn** behandelt die Repräsentationsseiten, die Gestaltung der Tablet-Oberfläche sowie die Anbindung an Pepper.  
- Die Ausarbeitung von **Leitner** beschäftigt sich mit KI-gestützter Interaktion und der Verwendung lokaler Sprachmodelle.  

Die einzelnen Teilaufgaben werden in den folgenden Kapiteln detailliert beschrieben.
