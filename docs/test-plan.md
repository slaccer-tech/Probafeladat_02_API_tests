# Teszt terv – GET https://reqres.in/api/users

## Cél
Annak biztosítása, hogy a végpont stabilan, helyes struktúrájú és tartalmú
adatot ad vissza, elfogadható válaszidővel.

## Tesztelt végpont
- **Metódus:** GET
- **URL:** `https://reqres.in/api/users`
- **Eszköz:** Postman (Tests script) / Newman (CI)

## Tesztkategóriák és tesztesetek
3 teszt helyett több tesztet írok kategóriákba sorolva: Teljesítmény, Adatstruktúra, Adatok helyessége.


### 1. HTTP válasz és teljesítmény
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC01 | HTTP státuszkód | `200 OK` |
| TC02 | Válaszidő | < 3000 ms |
| TC03 | Content-Type header | `application/json` |

### 2. JSON struktúra
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC04 | Válasz JSON-ként parse-olható | igen |

### 3. Lapozási mezők struktúrája
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC05 | Top-level lapozási mezők léteznek (`page`, `per_page`, `total`, `total_pages`) | igen |
| TC06 | Lapozási mezők típusa | integer |

### 4. `data` tömb struktúrája
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC07 | `data` mező létezik és tömb típusú | igen |
| TC08 | `data` tömb elemei objektumok | igen |

### 5. Felhasználó objektum struktúrája
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC09 | Minden user tartalmazza a mezőket: `id`, `email`, `first_name`, `last_name`, `avatar` | igen |
| TC10 | User mezők típusa helyes | `id`: integer, a többi: string |

### 6. Adatok helyessége
| Teszteset | Ellenőrzés | Elvárt eredmény |
|---|---|---|
| TC11 | Kötelező mezők kitöltöttsége | egyik mező sem `null` vagy üres string |
| TC12 | `email` formátum validáció | `xxx@yyy.zzz` mintának megfelel (regex) |
| TC13 | `avatar` mező formátuma | valid URL |

## Nem funkcionális megjegyzések
- A teljesítmény-teszt (TC02) nem volt explicit elvárás, de a fejlesztői
  biztonságérzet növelése miatt bekerült.

## Kimaradó / opcionális bővítési pontok
- ID-egyediség ellenőrzése a `data` tömbön belül
- Lapozási elemszám és `total_pages` konzisztencia-ellenőrzése
- Hibás lapozási paraméterek tesztelése (pl. `?page=999`)
- Response séma validálása JSON Schema-val (pl. `pm.response.to.have.jsonSchema()`)
