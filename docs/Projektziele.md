Unter diesem Abschnitt sind die ursprünglichen Ziele bei der Erstellung des Programms zu finden.

---

Ich möchte ein Programm schreiben, bei dem sich Benutzer anmelden können. Nutzer können den Admin Status besitzen. Jeder Benutzer kann für sich Listen erstellen. 
Diese Listen haben einen Namen. Der Nutzer kann seiner Liste Mangas (Comics) hinzufügen, entfernen und das Aktuelle Kapitel ändern. Die Anzahl der Kapitel können nicht vom 
Benutzer geändert werden. Ein Admin kann neue Mangas anlegen. Jedem Manga werden die Seiten für diesen zum online lesen hinterlegt. Ein Python-Script welches in einem Crontab 
läuft überprüft die Mangas alle 30 min und aktualisiert die Anzahl der Kapitel in der DB. Der Benutzer soll sich alle und die noch nicht fertig gelassenen Mangas anzeigen lassen 
können und sich für die Mangas die Links ausgeben lassen können. Das Programm soll über ein TUI gesteuert werden. Die Passwörter des Users sollen als Hash mit Salt in der DB 
gespeichert werden. Die Datenbank soll deshalb auf einem Raspberry Pi aufgesetzt werden, damit sie von verschiedenen Geräten erreichbar ist.  
Später soll ein Discord Bot Benutzern die Funktion geben Benachrichtigungen zu erhalten, wenn ein neues Kapitel zu einem Manga auf ihrer Liste erscheint. Dazu wird der Benutzer 
mit einem Discord Namen und Discriminator verknüpft. Über Discord sollen später auch Mangas der Liste des Benutzers hinzugefügt werden und Listen erstellt werden können.   
Danach möchte ich eine Weboberfläche erstellen. Diese soll auch auf dem Raspberry Pi laufen und auch für Mobile Geräte funktionieren.