Diese Seite beinhaltet die Dokumentation für alle API Endpoints. Dazu gehört die URL, die Art der Request, die zu übergebende JSON und der Rückgabewert. Das Format für jeden der API Endpoints ist
`127.0.0.1:8080/api/[Link]`

---

### GET `Manga/all`

Dieser Endpoint gibt alle Mangas, die in der Datenbank sind als JSON zurück. Es werden alle Attribute der Mangas in der 
Reihenfolge `id, name, chapters, finished, no_updates` zurückgegeben.
    


??? success "Beispiel Rückgabewert"

    ```json
    [
        [
            1,
            "test",
            123,
            false,
            false
        ]
    ]
    ```

---

### POST `Manga`

Dieser Endpoint erstellt einen neuen Manga in der Datenbank. Dazu benötigt der ausführende Benutzer Administrator-Rechte. Diese werden bei der Anfrage durch den Session-Cookie
überprüft.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "name": "example",
      "chapters": 1
    }
    ```

??? success "Beispiel Rückgabewert"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das Erstellen jedoch nicht funktioniert haben, ist der Rückgabewert `0`.   
    Dies kann daran liegen, dass der Benutzer nicht angemeldet ist oder
    nicht über Administrator-Rechte verfügt.

---

### GET `UserList/all`

Dieser Endpoint gibt alle Listen, die der User besitzt als JSON zurück. Alle Attribute der Liste werden in der Reihenfolge `id, name, private, user_id` zurückgegeben.

??? success "Beispiel Rückgabewerte"

    ```json
    [
        [
            1,
            "test list",
            true,
            1
        ],
        [
            4,
            "test list 12",
            true,
            1
        ],
        [
            10,
            "test list 123",
            true,
            1
        ]
    ]
    ```

---

### POST `UserList`

Dieser Endpunkt erstellt eine neue Liste für den Benutzer. Der aktuelle Benutzer wird über den Session-Cookie übergeben. Des Weiteren muss ein Name für die Liste in JSON Format
übergeben werden.

??? question "Beispiel Übergabewerte"
    
    ```json
    {
      "name": "example"
    }
    ```

??? success "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das erstellen jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist oder
    bereits eine Liste mit dem Namen existiert.

---

### GET `MangaList/all`

Dieser Endpunkt gibt alle Mangas, die der Benutzer in sämtlichen Listen hat zurück, wobei das Format der JSON Antwort `manga_list_id, manga_name, manga_current_chapter` ist.
Der Benutzer wird über den Session-Cookie ermittelt.

??? success "Beispiel Rückgabewerte"

    ```json
    [
        [
            1,
            "test",
            100
        ],
        [
            2,
            "test",
            0
        ]
    ]
    ```

---

### POST `/User/login`

Dieser Endpoint setzt den Session-Cookie des Benutzers, wenn dieser passende Anmeldedaten übergibt.

??? question "Beispiel Übergabewerte"
    
    ```json
    {
    "name" : "root",
    "password": "root"
    }
    ```
??? success "Beispiel Rückgabewerte"

    ```json
    {
        "success": true
    }
    ```

---

### POST `User/logout`

Dieser Endpoint entfernt den Session-Cookie des Nutzers. Es gibt keinen Rückgabewert.

---

### POST `UserList/add`

Dieser Endpoint fügt einer Liste des Benutzers einen Manga hinzu.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "manga_id": 1,
      "user_list_id": 1
    }
    ```

??? success "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das Hinzufügen jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist, ihm
    die Liste nicht gehört oder der Manga oder die Liste nicht existiert.

---

### POST `UserList/update`

Dieser Endpoint kann Informationen über das zuletzt gelesene Kapitel eines Mangas in einer Liste bearbeiten.  
Dies funktioniert nur bei Listen, die dem Benutzer auch gehören.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "manga_list_id": 1,
      "current_chapter": 1
    }
    ```

??? success "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das bearbeiten jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist, ihm
    die Liste nicht gehört oder dieser Manga nicht auf der Liste existiert.

---

### GET `UserList`

Dieser Endpunkt gibt alle Mangas aus eine spezifische Benutzerliste mit dem Format `id, name, private, current_chapter` zurück.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "id": 1
    }
    ```
??? success "Beispiel Rückgabewerte"
    
    ```json
    [
        [
            1,
            "test",
            1,
            100
        ],
        [
            1,
            "test",
            1,
            0
        ]
    ]
    ```

---

### POST `Manga/update/chapter`

Dieser Endpunkt ist zum Ändern der Anzahl der Kapitel eines Mangas gedacht.  
Für diese Aktion werden Administrator-Rechte benötigt.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "chapter": 100,
      "id": 1
    }
    ```

??? success "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das Bearbeiten jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist oder
    keine Administrator-Rechte besitzt.

---

### Post `User/create`

Dieser Endpunkt dient der Erstellung eines neuen Accounts in der Datenbank.

??? question "Beispiel Übergabewerte"

    ```json
    {
      "name": "abc",
      "password": "password"
    }
    ```

??? success "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das Erstellen eines neuen Accounts jedoch nicht funktioniert haben, ist der Rückgabewert `0`.  
    Dies kann daran liegen, dass bereits ein Nutzer mit dem Name existiert.