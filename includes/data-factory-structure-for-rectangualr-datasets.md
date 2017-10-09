## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Adja meg a struktúra definíciójának téglalap alakú adatkészletek
hello hello adatkészletek JSON struktúrában szakaszban van egy **választható** téglalap alakú táblákhoz (a sorok és oszlopok) szakaszt, és hello tábla oszlopait gyűjteményét tartalmazza. Hello struktúra szakasz típuskonverziók vagy olyan típussal kapcsolatos információk, vagy ez az oszlop-hozzárendelések fogja használni. a következő szakaszok hello ezeket a szolgáltatásokat részletesen ismertetik. 

Mindegyik oszlop tartalmaz hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello oszlop neve. |Igen |
| type |Hello oszlop adattípusát. Tekintse meg a típus átalakítások című szakaszt több részletek kapcsolatban, hogy mikor kell hogy típussal kapcsolatos információk megadása |Nem |
| Kulturális környezet |.NET-alapú kulturális környezet toobe használható, ha a típus meg van adva, de Datetime vagy Datetimeoffset .NET-típus. Alapértelmezett érték "en-us". |Nem |
| Formátumban |Formázó karakterlánc toobe használható, ha a típus meg van adva, de Datetime vagy Datetimeoffset .NET-típus. |Nem |

hello következő példában három oszlopok felhasználói azonosítóját, nevét és lastlogindate táblának hello struktúra szakasz JSON.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Használja a következő irányelveket a hello tooinclude "structure" információkat és a hello milyen tooinclude **struktúra** szakasz.

* **A strukturált adatforrások** , amelyek tárolása adatok séma- és típus magát (forrás például SQL Server, Oracle, az Azure tábla stb.), a hello "szerkezeti" szakasz olyan formában adja meg, csak akkor, ha azt szeretné, hogy egyes tegye oszlopleképezés hello adatokkal együtt a forrásoszlopokat fogadó és a nevek toospecific oszlopai nem hello azonos (lásd az alábbi oszlop leképezése részben). 
  
    Fent említett hello típus információ megadása nem kötelező "structure" szakaszában. A strukturált források típusinformációt már elérhető adatkészlet-definícióban hello adattár részeként, akkor nem tartalmazhat típussal kapcsolatos információk Ha hello "structure" szakasz.
* **Az olvasási adatforrások (kifejezetten az Azure blob) séma** választhat toostore adatok nélkül hello adatokkal bármely séma vagy típusú információk tárolására. Az ilyen típusú adatforrások hello követő 2 esetekben a "structure" kell tartalmaznia:
  * Azt szeretné, hogy toodo oszlop leképezése.
  * A másolási tevékenység forrást hello dataset esetén megadhatja a "structure" írja be az adatokat, és adat-előállító fogja használni a típus adatainak átalakítás toonative típusok hello fogadó számára. Lásd: [tooand adatok áthelyezése az Azure Blob](../articles/data-factory/data-factory-azure-blob-connector.md) cikkében találja.

### <a name="supported-net-based-types"></a>Támogatott. A NET-alapú típusok
Data factory támogatja hello CLS-kompatibilis .NET a következő típusú értékek a "structure" típusú adatokat ad meg olvasási adatforrások, például az Azure blob-sémáját alapján.

* Int16
* Int32 
* Int64
* Egyetlen
* Dupla
* Decimális
* Byte]
* logikai érték
* Karakterlánc 
* GUID
* Dátum és idő
* datetimeoffset
* Időtartomány 

A dátum és idő & Datetimeoffset opcionálisan kiegészítheti "nyelv" & "formátum" karakterlánc toofacilitate elemzése a egyéni dátum/idő karakterláncot. Tekintse meg a típus átalakítás az alábbi minta.

