Das folgende Programm ist im Rahmen eines schulischen Projektes entstanden. Das Ziel dieses Projektes war es, ein Programm mit einer Datenbank zu verknüpfen.
Da ich seit längerem mit der Programmiersprache Rust arbeiten wollte, habe ich dieses Projekt als API in Rust programmiert. Dafür habe ich hauptsächlich das Rust Web Framework Rocket
benutzt und dies mit einer mysql Datenbank verknüpft. Das Projekt soll später um eine Weboberfläche erweitert werden. Das [Projekt](https://github.com/BennetLe/rust_manga_readlist_api "Der Github Link zu meinem Projekt")
ist auf Github zu finden. Die folgende Dokumentation soll meine Arbeitsweise zeigen und die nutzung der API erklären. Ideen, die ich noch nicht implementiert habe, aber für die 
Zukunft Plane werden auch in dieser Dokumentation vorhanden sein genauso wie der Fortschritt an diesen.
---

## Projektbeschreibung 
Bei dem erstellten Programm können Nutzer einen Account erstellen und diesen nutzten, um Listen zu erstellen, in denen sie ihr zuletzt gelesenes Manga Kapitel speichern können. Wenn 
der Benutzer ein Admin ist, kann dieser zusätzlich neue Mangas anlegen. Zum Speichern der Anmeldung wird bei jeder neuen Anmeldung eine UUID-v4 erstellt die als Session Cookie 
gespeichert wird. Das hier erstellte Backend verfügt noch nicht über eine für den Benutzer einfach zugängliche Schnittstelle, kann aber über den Browser und Programme wie zum Beispiel
[Postman](https://www.postman.com/) genutzt werden.

---

## Zukünftige Funktionen

Die folgenden Punkte sind ihrer derzeitigen Priorität nach sortiert 

- Passwörter als Hash speichern
- Die DELETE Endpoints wurden aus Zeitmangel noch nicht erstellt
- Alle Rückgaben sollen standardisiert werden umd den Umgang mit der API zu vereinfachen
- Ein Python Script soll jede Stunde die Manga auf neue Kapitel überprüfen und in der Datenbank aktualisieren
- Ein Discord Bot mit folgenden Features soll erstellt werden:
    * ÜBer neue Kapitel benachrichtigen 
    * Generelle Funktionen der API (Liste erstellen, Manga hinzufügen, neustes gelesenes Kapitel ändern u.s.w.)
    * Admin Funktionen Discord Rollen basiert machen
- Eine Weboberfläche um die API von verschiedenen Geräten aus nutzen zu können

---

## Fazit

Die arbeit an diesem Projekt hat mir viel Spaß bereitet, da ich zum ersten Mal richtig mit Rust gearbeitet habe. Ich bin an Python gewöhnt, weshalb Rust zu lernen eine neue Herausforderung
für mich war. Zusätzlich habe ich noch nie eine API erstellt oder vorher mit Datenbanken gearbeitet, weshalb ich in diesem Projekt vieles neues lernen konnte. Die meisten Probleme hatte
ich bei der Arbeit mit der mysql library und einer grafischen Anwendung in Rust weshalb ich meine Anwendung neu geschrieben habe, so das diese nur als Schnittstelle zu einer anderen 
Anwendung dient. Alles in allem hatte ich an der Arbeit an diesem Projekt viel Spaß und bin mit meinem Endergebnis zufrieden.