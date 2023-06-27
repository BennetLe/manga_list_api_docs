Ich habe zuerst damit begonnen, mir grob zu überlegen, was ich für das Projekt brauche. Meine Ziele waren es die Liste von verschiedenen Endgeräten wie z.B. einem Handy, Laptop
und PC erreichbar zu machen, weshalb ich eine Weboberfläche mit einer API nutzen wollte. Danach habe ich begonnen, eine simple Verbindung zwischen Rust und meiner Datenbank herzustellen.
Dazu habe ich zu nächst eine Demo geschrieben, mit der es nur möglich war eine Tabelle auszugeben und Daten einzugeben. Diese waren vordefiniert und der Code war alles in allem 
nicht übersichtlich.

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

Zu diesem Zeitpunkt habe ich noch nicht die Platzhalter von mysql genutzt, was meinen Code sehr unübersichtlich gemacht hat. Dies habe ich jedoch in der Aktuellsten version der
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

Danach habe ich mir mit einem Use Case Diagramm überlegt, welche Funktionen, oder in meinem Fall API Endpoints ich benötige, um alle Anforderung die ich an das Programm hatte zu
erfüllen.

![Use Case](img/MangaList%20Use%20Case.png)

Einige der Features sind in der fertigen Version nicht genau so implementiert (keine System Console, Admins können noch keine benutzer Löschen). Dies wurde aus Zeitmangel nicht 
Priorisiert, soll aber noch implementiert werden.

Zu den Funktionen im Use Case Diagramm habe ich dann die passenden SQL Befehle erstellt. Diese habe ich dann in meiner API implementiert und mit Postman getestet. Dazu habe ich 
ein paar testfälle durchgeführt

| API Request                      | Data provided                         | Antwort                                                  | Andere entscheidende Faktoren                   |
|----------------------------------|---------------------------------------|----------------------------------------------------------|-------------------------------------------------|
| 127.0.0.1:8080/api/User/create   | "name": "abc", "password": "password" | 0                                                        | Nicht als Admin angemeldet                      |
| 127.0.0.1:8080/api/User/login    | "name" : "test", "password": "test"   | {"success":true}                                         | Account existiert                               |
| 127.0.0.1:8080/api/User/logout   |                                       | 1                                                        |                                                 |
| 127.0.0.1:8080/api/Manga/all     |                                       | [[1,"test",100,false,false],[2,"example",1,false,false]] |                                                 |
| 127.0.0.1:8080/api/MangaList/all |                                       | []                                                       | Keine Listen auf auf dem Account test vorhanden |


Solche Testfälle habe ich für jeden API Endpoint mehrfach durchgeführt, um sicherzustellen, dass alles funktioniert und auch bei z.B. einem fehlenden Cookie kein Fehler kommt. Führ 
fehlende Attribute in der Request habe ich jedoch noch nichts eingerichtet und Rocket gibt dabei die standard Seite für den Error Code 400 oder 422 zurück.