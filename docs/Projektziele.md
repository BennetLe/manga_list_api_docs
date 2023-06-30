Unter diesem Abschnitt sind die ursprünglichen Ziele bei der Erstellung des Programms zu finden.

---

Ich möchte ein Programm schreiben, bei dem sich Benutzer anmelden können. Nutzer können den Admin Status besitzen. Jeder Benutzer kann für sich Listen erstellen. 
Diese Listen haben einen Namen. Der Nutzer kann seiner Liste Mangas (Comics) hinzufügen, entfernen und das aktuelle Kapitel ändern. Die Anzahl der Kapitel können nicht vom 
Benutzer geändert werden. Ein Admin kann neue Mangas anlegen. Der Benutzer soll sich alle, auch die noch nicht fertig gelesenen Mangas ausgeben können. Das Programm wird über eine API gesteuert. 
Die Passwörter des Users sollen als Hash mit Salt in der DB 
gespeichert werden. Die Datenbank wird auf einem Raspberry Pi aufgesetzt, damit sie von verschiedenen Geräten erreichbar ist.  
Später soll ein Discord Bot Benutzern die Funktion geben, Benachrichtigungen zu erhalten, wenn ein neues Kapitel zu einem Manga auf ihrer Liste erscheint. Dazu wird der Benutzer 
mit einem Discord Namen verknüpft. Über Discord sollen später auch Mangas der Benutzerliste hinzugefügt und Listen erstellt werden können.   
Danach möchte ich eine Weboberfläche erstellen. Diese soll auch auf dem Raspberry Pi laufen und auch für Mobile Geräte funktionieren.