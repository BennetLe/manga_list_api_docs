In Rust werden statt Klassen Structs verwendet. Dabei handelt es sich um Klassen, die keine Methoden besitzen.  
Das bedeutet, dass ein Struct der Bauplan für das Speichern von Daten
ist. Deshalb habe ich für jede der Tabellen in der Datenbank und alle Rückgabewerte der API ein Struct erstellt.

??? note "Implementierung eines Struct für `user`"

    ```rust
    pub struct User {
        pub id: u32,
        pub name: String,
        pub password: String,
        pub admin: bool
    }
    ```

Bei einem Struct kann genau wie in einer Java-Klasse angegeben werden, ob dieser `public` oder `private`.  
Da ich für meine Structs eigene Dateien angelegt habe, sind bei mir alle Structs und zugehörige Attribute public.  
Einige der von mir implementierten Structs sind sehr kurz und könnten daher auch weggelassen werden. Ich wollte jedoch der besseren Übersicht wegen auch
für einfache Datensätze Vorlagen erstellen.

---

### User

??? info "user structs"

    ![user structs](img/user_struct.png)

--- 

### Logic

??? info "logic structs"

    ![logic structs](img/logic_struct.png)

---

### Manga

??? info "manga structs"
    
    ![manga structs](img/manga_struct.png)

---

### UserList

??? info "user_list structs"

    ![user_list structs](img/user_list_struct.png)