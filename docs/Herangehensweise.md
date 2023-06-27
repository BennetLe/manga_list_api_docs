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

![Use Case](img/MangaList%20Use%20Case.png)

Einige der Features sind in der fertigen Version nicht exakt wie im Diagramm dargestellt implementiert (keine System-Console, Admins können noch keine Benutzer löschen). Dies wurde aus Zeitmangel nicht 
priorisiert, soll aber noch implementiert werden.

Zu den Funktionen im Use-Case-Diagramm habe ich die passenden SQL Befehle erstellt. Diese habe ich anschließend in meiner API implementiert und mit Postman getestet. Dazu habe ich 
einige Testfälle durchgeführt.

| API Request                      | Übergebene Daten                      | Antwort                                                  | Andere entscheidende Faktoren                   |
|----------------------------------|---------------------------------------|----------------------------------------------------------|-------------------------------------------------|
| 127.0.0.1:8080/api/User/create   | "name": "abc", "password": "password" | 0                                                        | Nicht als Admin angemeldet                      |
| 127.0.0.1:8080/api/User/login    | "name" : "test", "password": "test"   | {"success":true}                                         | Account existiert                               |
| 127.0.0.1:8080/api/User/logout   |                                       | 1                                                        |                                                 |
| 127.0.0.1:8080/api/Manga/all     |                                       | [[1,"test",100,false,false],[2,"example",1,false,false]] |                                                 |
| 127.0.0.1:8080/api/MangaList/all |                                       | []                                                       | Keine Listen auf auf dem Account test vorhanden |


Solche Testfälle habe ich für jeden API-Endpoint mehrfach durchgeführt, um sicherzustellen, dass alles funktioniert und auch bei z.B. einem fehlenden Cookie kein Fehler auftritt. Für 
fehlende Attribute in der Request habe ich jedoch noch keine Rückgabe eingerichtet. Die library Rocket gibt dabei die Standard-Webseite für den Error-Code 400 oder 422 zurück.