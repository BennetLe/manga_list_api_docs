In diesem Abschnitt werde ich wichtige Code-Abschnitte aus meinem Quellcode erklären und ihre nutzung erläutern.

---

```rust
pub fn connect() -> PooledConn {
    let pool = Pool::new(URL).unwrap();
    let conn = pool.get_conn().unwrap();
    return conn;
}
```

Die funktion `connect` stellt die Verbindung zu der Datenbank zurück und gibt dann diese Verbindung als Objekt zurück. Diese Methode wird in jeder Methode benutzt, die in irgendeiner Weise mit 
der Datenbank interagiert. Die URL die dieser Methode übergeben wird, ist statisch und beinhaltet alles um sich mit der Datenbank zu verbinden (url, port, username, password).

---

```rust
pub fn get_id_by_session(
    cookie: String
) -> u32 {
    let mut conn = db_layer::connection::connect();

    println!("{}", cookie);

    let query = "SELECT user.id FROM user JOIN user_auth_cookie uac on user.id = uac.user_id WHERE uac.cookie = \"".to_owned()+&cookie.to_owned()+"\"";

    let result = conn.query(query).unwrap();

    println!("GetIdBySession: {:?}", result);
    
    if result.len() >= 1 {
        return result[0];
    }

    return 0;
}
```

Diese Methode bekommt den Session-Cookie übergeben und gibt die zugehörige User-ID zurück. Sollte der Session Cookie nicht in der Datenbank existieren, so gibt diese Funktion eine `0` zurück.
Diese User-ID ist keinem User zugewiesen, wodurch erkennbar ist, dass der Cookie ungültig ist.

---

```rust
pub fn is_owner_of_manga_list(
    cookie: &CookieJar<'_>,
    user_list_id: u32
) -> bool{
    let mut conn = db_layer::connection::connect();
    let cookie_session = cookie.get("session").unwrap().value();
    let user_id = db_layer::user::get_id_by_session(cookie_session.to_string());
    let query = "SELECT user_id FROM user_list WHERE user_id = ? AND id = ?";
    let result: Vec<u32> = conn.exec(query, (user_id, user_list_id)).unwrap();

    if result.len() == 0 {
        return false
    }
    if result[0] >= 1 {
        return true
    }
    return false
}
```

Diese Methode nutzt die Methode `get_id_by_session` um aus dem Session-Cookie eine User-ID zu erhalten. Danach überprüfe ich, ob die übergebene Listen-ID der erhaltenen User-ID gehört. Um Fehler
zu vermeiden, überprüfe ich zuerst die länge des Ergebnisses, da es sonst zu einem `Index out of bounds` Fehler kommen kann. Wenn dies geschehen ist, gebe ich abhängig des Ergebnisses `true` oder
`false` zurück.