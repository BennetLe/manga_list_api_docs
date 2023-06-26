# Endpoints

Diese Seite beinhaltet die Dokumentation für alle API Endpoints. Dazu gehört die URL, die Request art, die zu übergebene JSON und der Rückgabewert. Das Format für jeden der API Endpoints ist
127.0.0.1:8080/api/`Link`

---

### GET `Manga/all`

Dieser Endpoint gibt alle Mangas, die in der Datenbank sind als JSON zurück. Es werden alle Attribute der Manga in der 
Reihenfolge id, id, name, chapters, finished, no_updates zurückgegeben.
    


??? "Beispiel Ergebnis"

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

Dieser Endpoint erstellt einen neuen Manga in der Datenbank. Dazu benötigt der ausführende Benutzer administrator Rechte. Dies wird über den Session-Cookie überprüft.