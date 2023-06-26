Diese Seite beinhaltet die Dokumentation für alle API Endpoints. Dazu gehört die URL, die Request art, die zu übergebene JSON und der Rückgabewert. Das Format für jeden der API Endpoints ist
127.0.0.1:8080/api/`Link`

---

### GET `Manga/all`

Dieser Endpoint gibt alle Mangas, die in der Datenbank sind als JSON zurück. Es werden alle Attribute der Manga in der 
Reihenfolge `id, name, chapters, finished, no_updates` zurückgegeben.
    


??? example "Beispiel Rückgabewert"

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

Dieser Endpoint erstellt einen neuen Manga in der Datenbank. Dazu benötigt der ausführende Benutzer administrator Rechte. Diese werden bei der Anfrage durch den Session-Cookie
überprüft.

??? example "Beispiel Übergabewerte"

    ```json
    {
      "name": "example",
      "chapters": 1
    }
    ```

??? example "Beispiel Rückgabewert"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das erstellen jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist oder
    nicht über administrator Rechte verfügt.

---

### GET `UserList/all`

Dieser Endpoint gibt alle Listen, die der User besitzt als JSON zurück. Alle Attribute der Liste werden in der Reihenfolge `id, name, private, user_id` zurückgegeben.

??? example "Beispiel Rückgabewerte"

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

### POST `/UserList`

Dieser Endpunkt erstellt eine neue List für den Benutzer. Der aktuelle Benutzer wird über den Session-Cookie übergeben. De weiteren muss ein Name für die Liste in JSON Format
übergeben werden.

??? example "Beispiel Übergabewerte"
    
    ```json
    {
      "name": "example"
    }
    ```

??? example "Beispiel Rückgabewerte"

    Bei Erfolg ist der Rückgabewert `1`. Sollte das erstellen jedoch nicht funktioniert haben, ist der Rückgabewert `0`. Dies kann daran liegen, dass der Benutzer nicht angemeldet ist oder
    bereits eine Liste mit dem Namen existiert.