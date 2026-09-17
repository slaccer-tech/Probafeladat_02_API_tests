# ReqRes API Tests

Automatizált tesztek a [reqres.in](https://reqres.in) publikus teszt-API
`GET /api/users` végpontjához, **Postman**-ben írva és **Newman** CLI-vel
futtatva (CI-kompatibilis).

## Mit ellenőriznek a tesztek?

| Kategória | Tesztek |
|---|---|
| HTTP válasz és teljesítmény | TC01–TC03: státuszkód, válaszidő (<3000 ms), Content-Type |
| JSON struktúra | TC04: érvényes JSON body |
| Lapozási mezők | TC05–TC06: `page`, `per_page`, `total`, `total_pages` léteznek és integerek |
| `data` tömb struktúrája | TC07–TC08: létezik, tömb típusú, elemei objektumok |
| Felhasználó objektum struktúrája | TC09–TC10: kötelező mezők léteznek és típusuk helyes |
| Adatok helyessége | TC11–TC14: egyedi ID-k, kitöltött mezők, valid email formátum, valid avatar URL |
| Lapozási konzisztencia | TC15: `data.length` megfelel a `per_page`-nek (utolsó oldalon a maradéknak) |

A teljes tervezési dokumentációt lásd: [`docs/test-plan.md`](docs/test-plan.md).

## Futtatás Postman-ben

1. Nyisd meg a Postman-t.
2. **Import** → válaszd a [`collection.json`](collection.json) fájlt.
3. Futtasd a `GET /api/users - page 1` requestet, vagy futtasd a teljes
   collectiont a **Collection Runner**-rel.

## Futtatás parancssorból (Newman)

Előfeltétel: [Node.js](https://nodejs.org/) telepítve.

```bash
npm install
npm test
```

Ez a `newman run collection.json` parancsot futtatja, és a terminálban
megjeleníti mind a 15 teszt eredményét.

## Folyamatos integráció (CI)

A repo tartalmaz egy GitHub Actions workflow-t
([`.github/workflows/tests.yml`](.github/workflows/tests.yml)), ami minden
`push` és `pull_request` eseménynél automatikusan lefuttatja a teszteket.

## Projekt struktúra

```
.
├── collection.json              # Postman Collection a teszt-szkripttel
├── docs/
│   └── test-plan.md             # Teszt terv / dokumentáció
├── package.json                 # Newman futtatáshoz
├── .github/
│   └── workflows/
│       └── tests.yml            # CI workflow
└── README.md
```
