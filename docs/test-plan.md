# Teszt terv – GET https://reqres.in/api/users

## 1. Cél
Annak biztosítása, hogy a végpont stabilan, helyes struktúrájú és tartalmú
adatot ad vissza, elfogadható válaszidővel.

## 2. Tesztelt végpont
- **Metódus:** GET
- **URL:** `https://reqres.in/api/users`
- **Eszköz:** Postman (Tests script) / Newman (CI)

## 3. Tesztkategóriák

### 3.1 Adatstruktúra (Schema validáció)
| # | Ellenőrzés | Elvárt eredmény |
|---|------------|------------------|
| 1 | HTTP státuszkód | `200 OK` |
| 2 | Content-Type header | `application/json` |
| 3 | Válasz JSON-ként parse-olható | igen |
| 4 | Top-level mezők léteznek | `page`, `per_page`, `total`, `total_pages`, `data` |
| 5 | `page`, `per_page`, `total`, `total_pages` típusa | integer |
| 6 | `data` mező típusa | tömb (array) |
| 7 | `data` tömb elemei objektumok | igen |
| 8 | Minden `data` elem tartalmazza a mezőket | `id`, `email`, `first_name`, `last_name`, `avatar` |

### 3.2 Adatok helyessége és konzisztenciája
| # | Ellenőrzés | Elvárt eredmény |
|---|------------|------------------|
| 1 | `id` mezők egyedisége | nincs duplikáció a `data` tömbön belül |
| 2 | `id` típusa | integer |
| 3 | `email` formátum validáció | `xxx@yyy.zzz` mintának megfelel (regex) |
| 4 | `first_name`, `last_name` típusa | string |
| 5 | Kötelező mezők kitöltöttsége | egyik mező sem `null` vagy üres string |
| 6 | `avatar` mező formátuma | valid URL |
| 7 | Lista elemszáma egyezik-e a szerződéssel | `data.length === per_page` (kivéve utolsó oldal) |

### 3.3 Teljesítmény
| # | Ellenőrzés | Elvárt eredmény |
|---|------------|------------------|
| 1 | Válaszidő | < 3000 ms |

## 4. Tesztesetek megfeleltetése (collection.json)

| Teszteset | Kategória |
|---|---|
| TC01–TC03 | HTTP válasz és teljesítmény |
| TC04 | JSON struktúra |
| TC05–TC06 | Lapozási mezők |
| TC07–TC08 | `data` tömb struktúrája |
| TC09–TC10 | Felhasználó objektum struktúrája |
| TC11–TC14 | Adatok helyessége |
| TC15 | Lapozási konzisztencia |

## 5. Nem funkcionális megjegyzések
- A teljesítmény-teszt nem volt explicit elvárás, de a fejlesztői
  biztonságérzet növelése miatt bekerült.
- Az `id` egyediség és a lapozási elemszám inkább *konzisztencia*, mint
  szűken vett *adathelyesség* teszt.

## 6. Kimaradó / opcionális bővítési pontok
- Hibás lapozási paraméterek tesztelése (pl. `?page=999`)
- Response séma validálása JSON Schema-val (pl. `pm.response.to.have.jsonSchema()`)
