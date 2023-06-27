Das folgende Programm ist im Rahmen eines schulischen Projektes entstanden. Das Ziel dieses Projektes war es, ein Programm mit einer Datenbank zu verknüpfen.
Da ich seit längerem mit der Programmiersprache Rust arbeiten wollte, habe ich dieses Projekt als API in Rust programmiert. Dafür habe ich hauptsächlich das Rust Web Framework Rocket
benutzt und dies mit einer mysql Datenbank verknüpft. Das Projekt soll später um eine Weboberfläche erweitert werden. Das [Projekt](https://github.com/BennetLe/rust_manga_readlist_api "Der Github Link zu meinem Projekt")
ist auf Github zu finden. Die folgende Dokumentation soll meine Arbeitsweise zeigen und die Nutzung der API erklären.  
Ideen, die ich noch nicht implementiert habe, aber für die Zukunft plane, werden, genauso wie der Fortschritt an diesen, auch in dieser Dokumentation vorhanden sein.

---

## Projektbeschreibung
Bei dem erstellten Programm können Manga-Leser einen Account anlegen und diesen nutzten, um Listen zu erstellen, in denen sie speichern können, welches Kapitel eines Mangas sie zuletzt gelesen
haben. Wenn der Nutzer ein Admin ist, kann dieser zusätzlich neue Mangas anlegen, die in einer vorhandenen Datenbank gespeichert werden.   
Zum Speichern einer Anmeldung wird jedes Mal eine UUID-v4 erstellt, die als Session-Cookie gespeichert wird.  
Das hier erstellte Backend verfügt noch nicht über 
eine für den Benutzer einfach zugängliche Schnittstelle, kann aber über den Browser und Programme wie zum Beispiel [Postman](https://www.postman.com/) genutzt werden.

---

## Zukünftige Funktionen

Die geplanten Funktionen sind ihrer Priorität nach sortiert:

- Passwörter als Hash speichern (für Testversion nicht erforderlich)
- DELETE Endpoints erstellen (aus Zeitmangel noch nicht erstellt)
- Rückgaben sind derzeit nicht einheitlich und sollen standardisiert werden
- Ein Python-Script soll jede Stunde die Mangas aus der Datenbank auf neue Kapitel überprüfen und aktualisieren 
- Ein Discord-Bot mit folgenden Features soll erstellt werden:
    * Über neue Kapitel benachrichtigen 
    * Generelle Funktionen der API (Liste erstellen, Manga hinzufügen, neustes gelesenes Kapitel ändern u.s.w.)
    * spezifische Discord-Rollen sollen Zugriff auf die Admin-Funktionen der API regeln 
- Weboberfläche, um die API von verschiedenen Geräten aus nutzen zu können

---

## Fazit

Die Arbeit an diesem Projekt hat mir viel Spaß bereitet, da ich zum ersten Mal richtig mit Rust gearbeitet habe.   
Ich bin an Python gewöhnt, weshalb Rust zu lernen eine neue Herausforderung
für mich war. Zusätzlich habe ich noch nie eine API erstellt oder vorher mit Datenbanken gearbeitet. Daher konnte ich in diesem Projekt viel Neues lernen. 

Die meisten Probleme hatte
ich bei der Arbeit mit der mysql-library und einer grafischen Anwendung in Rust, weshalb ich meine Anwendung neu schreiben musste, damit diese nur als Schnittstelle zu einer anderen 
Anwendung dient.

Alles in allem hatte ich an der Arbeit an diesem Projekt viel Spaß und bin mit meinem Endergebnis zufrieden.