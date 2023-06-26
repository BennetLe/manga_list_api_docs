Das folgende Programm ist im Rahmen eines schulischen Projektes entstanden. Das Ziel dieses Projektes war es, ein Programm mit einer Datenbank zu verknüpfen.
Da ich seit längerem mit der Programmiersprache Rust arbeiten wollte, habe ich dieses Projekt als API in Rust programmiert. Dafür habe ich hauptsächlich das Rust Web Framework Rocket
benutzt und dies mit einer mysql Datenbank verknüpft. Das Projekt soll später um eine Weboberfläche erweitert werden. Das Projekt ist auf [Github](https://github.com/BennetLe/rust_manga_readlist_api "Der Github Link zu meinem Projekt")
zu finden. Die folgende Dokumentation soll meine Arbeitsweise zeigen und die nutzung der API erklären. Ideen, die ich noch nicht implementiert habe, aber für die Zukunft Plane werden 
auch in dieser Dokumentation vorhanden sein genauso wie der Fortschritt an diesen.
---

## Projektbeschreibung 
Bei dem erstellten Programm können Nutzer einen Account erstellen und diesen nutzten, um Listen zu erstellen, in denen sie ihr zuletzt gelesenes Manga Kapitel speichern können. Wenn 
der Benutzer ein Admin ist, kann dieser zusätzlich neue Mangas anlegen. Zum Speichern der Anmeldung wird bei jeder neuen Anmeldung eine UUID-v4 erstellt die als Session Cookie 
gespeichert wird. Das hier erstellte Backend verfügt noch nicht über eine für den Benutzer einfach zugängliche Schnittstelle, kann aber über den Browser und Programme wie zum Beispiel
[Postman](https://www.postman.com/) genutzt werden.