Ich habe zunächst damit begonnen, grob festzulegen, was für das Projekt erforderlich ist.  
Mein Ziel war es, die Liste von verschiedenen Endgeräten wie z.B. einem Handy, Laptop und PC erreichbar zu machen. Daher habe ich mich für die Nutzung einer Weboberfläche mit einer API entschieden.  
Anschließend habe ich begonnen, eine simple Verbindung zwischen Rust und meiner Datenbank herzustellen.
Dazu habe ich zunächst eine Demo geschrieben, mit der es nur möglich war, eine einzige Tabelle aus- und Daten einzugeben. Die Eingabemöglichkeiten waren vordefiniert und der Code alles in allem unübersichtlich.

??? example "Demo Ausgeben"

    ```rust
    pub fn get(
        conn: &mut PooledConn,
        input_type: &str,
        input_value: &str
    ) -> Vec<(u32, String, u32, bool, bool)> {
        let res:Vec<(u32, String, u32, bool, bool)> = conn.query(
            "SELECT id, name, chapters, finished, no_updates FROM mangas WHERE ".to_owned() + &input_type + " = \"" + &input_value + "\";"
        )
            .unwrap();
    
        return res;
    }
    ```

??? example "Demo Einfügen"

    ```rust
    pub fn create(
        conn: &mut PooledConn,
        user: &User,
    ) -> std::result::Result<String, Error> {
        conn.exec_drop(r"INSERT INTO user (name, password)
        VALUES (:name, :password)",
                       params! {
                            "name" => user.name.clone(),
                            "password" => user.password.clone(),
                       },
        )
            .and_then(|_| Ok(user.name.clone()))
    }
    ```

Zu diesem Zeitpunkt habe ich noch nicht die Platzhalter von mysql genutzt, was meinen Code sehr unübersichtlich gemacht hat. Dies habe ich jedoch in der aktuellsten Version der
API bereits eingebaut.

??? example "Ausgeben"
    
    ```rust
    pub fn get_all_user_mangas(
        mut cookie: String
    ) -> Vec<(u32, String, u32)> {
        let mut conn = db_layer::connection::connect();
        let query = "SELECT ml.id, m.name, ml.current_chapter
    FROM user
        JOIN user_list ul on user.id = ul.user_id
        JOIN manga_list ml on ul.id = ml.user_list_id
        JOIN mangas m on m.id = ml.manga_id
        JOIN user_auth_cookie uac on user.id = uac.user_id
    where uac.cookie = ?";
        let result = conn.exec(query, (cookie, )).unwrap();
        return result;
    }
    ```
??? example "Einfügen"

    ```rust
    pub fn add(
        mut new_manga: Json<CreateManga>
    ) -> u64 {
        let mut conn = db_layer::connection::connect();
        let query = "INSERT INTO mangas (name, chapters) VALUES (?, ?)";
        let name = new_manga.name.to_owned();
        let chapters:u32= new_manga.chapters;
        let result = conn.exec_iter(query, (name, chapters)).unwrap();
        return result.affected_rows();
    }
    ```

Vor Beginn der eigentlichen Programmierung habe ich ein Use-Case-Diagramm erstellt, dass Funktionen, oder in meinem Fall, API-Endpoints erfasst, die für ein funktionstüchtiges Programm erforderlich sind.

---

### Use-Case-Diagramm

![Use Case](img/MangaListAktuell.png)

Bei diesem Use-Case-Diagramm gibt es 3 Akteure. Die Person ist nicht angemeldet und besitzt deshalb kine Funktionen außer der Anmeldung und der erstellung eines Accounts. Wenn sich die Person erfolgreich anmeldet, wird diese zu einem Benutzer.  
Der Benutzer kann sich abmelden, Listen erstellen, diese bearbeiten, sich alle Mangas der Datenbank anzeigen und diese seinen Listen hinzufügen, Mangas aus seinen Listen entfernen, sich alle seine Listen anzeigen und sich den Inhalt seiner Listen Anzeigen. Hierbei erbt Manga hinzufügen von Manga anzeigen, 
da der Benutzer ohne zu wissen welche Manga in der DB existieren keinen seiner Liste hinzufügen kann.  
Zuletzt gibt es einen Admin welcher alle Funktionen des Benutzers besitzt. Zusätzlich kann dieser jedoch einen Manga erstellen und die Kapitel Anzahl dieser bearbeiten.

---

### SQL Befehle

Zu den Funktionen im Use-Case-Diagramm habe ich die passenden SQL Befehle erstellt. Diese habe ich anschließend in meiner API implementiert und mit Postman getestet. Dazu habe ich 
einige Testfälle durchgeführt. Alle SQL Befehle des Use-Case-Diagramms sind in dem folgenden Abschnitt aufgelistet.

??? example "Anmelden"

    ```mysql
    SELECT user.id FROM user WHERE name = ? AND password = ?;
    ```

??? example "Benutzer erstellen"

    ```mysql
    INSERT INTO user (name, password) VALUES (?, ?);
    ```

??? example "Liste erstellen"

    ```mysql
    INSERT INTO user_list (name, user_id) VALUES (?, ?);
    ```

??? example "Manga anzeigen"

    ```mysql
    SELECT id, name, chapters, finished, no_updates FROM mangas;
    ```

??? example "Manga hinzufügen"

    ```mysql
    INSERT INTO manga_list (manga_id, user_list_id, current_chapter) VALUES (?, ?, 0);
    ```

??? example "Manga entfernen"
    ```mysql
    
    ```

??? example "Listen anzeigen"

    ```mysql
    SELECT id, name, private, user_id FROM user_list WHERE user_id = ?;
    ```
    

??? example "Inhalt von Liste anzeigen"

    ```mysql
    SELECT mangas.id, mangas.name, ul.private, ml.current_chapter 
    FROM mangas 
    JOIN manga_list ml on mangas.id = ml.manga_id 
    JOIN user_list ul on ul.id = ml.user_list_id 
    JOIN user u on u.id = ul.user_id 
    WHERE ul.id = ? AND u.id = ?;
    ```

??? example "Manga erstellen"
    
    ```mysql
    INSERT INTO mangas (name, chapters) VALUES (?, ?);
    ```

??? example "Kapitelanzahl ändern"

    ```mysql
    UPDATE mangas SET chapters = ? WHERE id = ?;
    ```

---

### Testfälle

| API Request                      | Übergebene Daten                      | Antwort                                                  | Andere entscheidende Faktoren                   |
|----------------------------------|---------------------------------------|----------------------------------------------------------|-------------------------------------------------|
| 127.0.0.1:8080/api/User/create   | "name": "abc", "password": "password" | 0                                                        | Nicht als Admin angemeldet                      |
| 127.0.0.1:8080/api/User/login    | "name" : "test", "password": "test"   | {"success":true}                                         | Account existiert                               |
| 127.0.0.1:8080/api/User/logout   |                                       | 1                                                        |                                                 |
| 127.0.0.1:8080/api/Manga/all     |                                       | [[1,"test",100,false,false],[2,"example",1,false,false]] |                                                 |
| 127.0.0.1:8080/api/MangaList/all |                                       | []                                                       | Keine Listen auf auf dem Account test vorhanden |


Solche Testfälle habe ich für jeden API-Endpoint mehrfach durchgeführt, um sicherzustellen, dass alles funktioniert und auch bei z.B. einem fehlenden Cookie kein Fehler auftritt. Für 
fehlende Attribute in der Request habe ich jedoch noch keine Rückgabe eingerichtet. Die library Rocket gibt dabei die Standard-Webseite für den Error-Code 400 oder 422 zurück.

---

### Projekttagebuch der Aktuellen version

Das Projekttagebuch ist für die gesamte Arbeit an der API. Das bedeutet, dass alles an Recherche und Erarbeitung von Wissen hier nicht enthalten ist und sich diese Auflistung nur mit reinem Fortschritt am Projekt beschäftigt.


| Datum   | Beschreibung                                                           | Status        |
|---------|------------------------------------------------------------------------|---------------|
| 14.6.23 | Grundstruktur des Projektes wurde erstellt                             | Abgeschlossen |
| 14.6.23 | API Endpoints für Manga erstellt                                       | Abgeschlossen |
| 14.6.23 | Struktur für Rocket erstellt                                           | Abgeschlossen |
| 14.6.23 | API Endpoint für User List erstellt                                    | Abgeschlossen |
| 14.6.23 | API Endpoint zum erstellen von Mangas erstellt                         | Abgeschlossen |
| 14.6.23 | Structs für Manga, User und User List erstellt                         | Abgeschlossen |
| 20.6.23 | API Endpoint um alle Mangas eines Benutzers zu erhalten erstellt       | Abgeschlossen |
| 20.6.23 | UUID-v4 für die erstellung des Session-Cookie hinzugefügt              | Abgeschlossen |
| 20.6.23 | API Endpoint für Login erstellt                                        | Begonnen      |
| 21.6.23 | API Endpoint für Login erstellt                                        | Abgeschlossen |
| 22.6.23 | API Endpoint zum erstellen eines Mangas bearbeitet                     | Abgeschlossen |
| 22.6.23 | Das Erstellen einer Liste nutzt nun den Session-Cookie für die User ID | Abgeschlossen |
| 22.6.23 | Text Datei mit den SQL Befehlen zum erstellen der Datenbank erstellt   | Abgeschlossen |
| 22.6.23 | Möglichen Fehlerquelle beim erstellen eines Mangas beseitigt           | Abgeschlossen |
| 23.6.23 | API Endpoint zum erstellen von Listen bearbeitet                       | Abgeschlossen |
| 23.6.23 | API Endpoint erstellt, der einer Liste einen Manga hinzufügen kann     | Abgeschlossen |
| 23.6.23 | Funktion erstellt, die Prüft ob einer User ID eine Liste gehört        | Abgeschlossen |
| 23.6.23 | API Endpoint zum Aktualisieren des aktuellen Kapitel hinzugefügt       | Abgeschlossen |
| 23.6.23 | Alle API Endpoints bekommen die User ID jetzt über den Session Cookie  | Abgeschlossen |
| 23.6.23 | API Endpoint um eine spezifische Liste zu erhallten erstellt           | Abgeschlossen |
| 23.6.23 | API Endpoint zum ändern der Kapitel anzahl eines Mangas erstellt       | Abgeschlossen |
| 23.6.23 | API Endpoint zum registrieren erstellt                                 | Abgeschlossen |
| 23.6.23 | Rocket config Datei erstellt                                           | Abgeschlossen |

Mit [MD Table Generator](https://www.tablesgenerator.com/markdown_tables) erstellt.