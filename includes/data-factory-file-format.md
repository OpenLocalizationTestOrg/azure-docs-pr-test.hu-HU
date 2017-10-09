## <a name="specifying-formats"></a>Formátumok meghatározása
Az Azure Data Factory támogatja a következő formátumban típusok hello:

* [Szöveges formátum](#specifying-textformat)
* [JSON formátum](#specifying-jsonformat)
* [Avro formátum](#specifying-avroformat)
* [ORC formátum](#specifying-orcformat)
* [Parquet formátum](#specifying-parquetformat)

### <a name="specifying-textformat"></a>TextFormat megadása
Ha szeretné, hogy tooparse hello szövegfájlok vagy hello adatírás szöveges formátumú, állítsa be a hello `format` `type` tulajdonság túl**szöveges**. Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz. Lásd: [szöveges példa](#textformat-example) hogyan szakasz tooconfigure.

| Tulajdonság | Leírás | Megengedett értékek | Kötelező |
| --- | --- | --- | --- |
| columnDelimiter |hello karakter egy fájlban használt tooseparate oszlopok. Érdemes lehet toouse ritka nem nyomtatható karakter, amely valószínűleg nem létezik az adatokon: például adja meg a "\u0001" által képviselt indítsa el a fejléc (SOH). |Csak egy karakter használata engedélyezett. Hello **alapértelmezett** értéke **vesszővel (",")**. <br/><br/>toouse Unicode-karakter, tekintse meg a túl[Unicode-karaktereket](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget megfelelő kód hello azt. |Nem |
| rowDelimiter |hello karakter egy fájlban használt tooseparate sorokat. |Csak egy karakter használata engedélyezett. Hello **alapértelmezett** értéke bármelyik hello követő értékek olvasáskor: **["\r\n", "\r", "\n"]** és **"\r\n"** írás. |Nem |
| escapeChar |hello speciális karaktert használt oszlop elválasztó tooescape hello bemeneti fájl tartalmát. <br/><br/>Egy táblához nem határozható meg az escapeChar és a quoteChar is. |Csak egy karakter használata engedélyezett. Nincs alapértelmezett érték. <br/><br/>Példa: Ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave hello vesszővel karaktert hello szövegként (Példa: "Hello, world"), adja meg a "$" hello escape-karakter, és a karakterlánc "Hello$, world" hello forrásból. |Nem |
| quoteChar |hello kitöltéshez használandó tooquote karakterlánc-érték. hello oszlop- és sorfejlécek határolójelek belül hello idézőjel karakterek hello karakterláncértéket részeként kezelik. Ez a tulajdonság akkor alkalmazható tooboth bemeneti és kimeneti adathalmazokat.<br/><br/>Egy táblához nem határozható meg az escapeChar és a quoteChar is. |Csak egy karakter használata engedélyezett. Nincs alapértelmezett érték. <br/><br/>Például, ha vesszővel (', '), hello oszlop elválasztó, de szeretné toohave vesszővel karaktert hello szöveges (Példa: < Hello, world >), definiálhat "(idézőjelek nélkül), hello idézőjel, és használja a hello karakterlánc"Hello, world"hello forrás. |Nem |
| nullValue |Egy vagy több karaktert használt toorepresent null értékű. |Egy vagy több karakter. Hello **alapértelmezett** értékei **"\N" és "NULL"** olvasáskor és **"\N"** írás. |Nem |
| encodingName |Adja meg a hello kódolási név. |Egy érvényes kódolási név. Lásd az [Encoding.EncodingName tulajdonságot](https://msdn.microsoft.com/library/system.text.encoding.aspx). Például: windows-1250 vagy shift_jis. Hello **alapértelmezett** értéke **UTF-8**. |Nem |
| firstRowAsHeader |Meghatározza, hogy tooconsider hello első sor adatai alkossák fejléc. A bemeneti adatkészletek első sorát a Data Factory fejlécként olvassa be. A kimeneti adatkészletek első sorát a Data Factory fejlécként írja ki. <br/><br/>[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket. |True (Igaz)<br/>**False (alapértelmezett)** |Nem |
| skipLineCount |Meghatározza a sorok tooskip hello számát, bemeneti fájlok adatainak olvasásakor. Ha skipLineCount és firstRowAsHeader is meg van adva, hello sorok először kimarad, és majd hello fejléc-információ a hello bemeneti fájl olvasásakor. <br/><br/>[A `firstRowAsHeader` és a `skipLineCount` használatára vonatkozó forgatókönyvekben](#scenarios-for-using-firstrowasheader-and-skiplinecount) tekinthet meg minta-forgatókönyveket. |Egész szám |Nem |
| treatEmptyAsNull |Megadja, hogy a tootreat null vagy üres karakterlánc, a rendszer null értéket, amikor a bemeneti fájl adatainak olvasása. |**True (alapértelmezett)**<br/>False (Hamis) |Nem |

#### <a name="textformat-example"></a>A TextFormat használatát bemutató példa
hello következő példa bemutatja hello formátum tulajdonságok a szöveges.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

toouse egy `escapeChar` helyett `quoteChar`, cserélje le a hello sor `quoteChar` a következő escapeChar hello:

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>A firstRowAsHeader és a skipLineCount használatára vonatkozó forgatókönyvek
* Nem fájl forrás tooa szöveges fájlból másolja át, és szeretné tooadd hello séma metaadatokat tartalmazó fejléc sor (pl.: SQL-séma). Adja meg `firstRowAsHeader` hello kimeneti adatkészlet ebben a forgatókönyvben a true.
* A fejléc sor tooa nem fájl fogadó tartalmazó szöveges fájlból másolja át, és szeretné, hogy a sor toodrop. Adja meg `firstRowAsHeader` hello bemeneti adatkészletet, igaz.
* Másolja át egy szövegfájlból, és szeretné, hogy tooskip néhány sor hello elején, adatok vagy a fejléc adatokat tartalmazó. Adja meg `skipLineCount` tooindicate hello száma sorok toobe kihagyva. Ha hello rest hello fájl tartalmazza a fejléc, azt is megadhatja, `firstRowAsHeader`. Ha mindkét `skipLineCount` és `firstRowAsHeader` megadott, hello sorok először kimarad, és ezután hello fejléc-információ a hello bemeneti fájl olvasásának

### <a name="specifying-jsonformat"></a>JsonFormat megadása
túl**importálási/exportálási JSON-fájlok használata-be/Azure Cosmos DB van**, lásd: [importálási/exportálási JSON-dokumentumok](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) szakasz hello Azure Cosmos DB Connector adatokkal.

Ha szeretné, hogy tooparse hello JSON-fájlokat vagy hello adatírás JSON formátumban, állítsa be a hello `format` `type` tulajdonság túl**JsonFormat**. Azt is megadhatja, hello következő **választható** hello tulajdonságok `format` szakasz. Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure.

| Tulajdonság | Leírás | Kötelező |
| --- | --- | --- |
| filePattern |Minden JSON-fájlban tárolt adatok hello mintát jelzi. Az engedélyezett értékek a következők: **setOfObjects** és **arrayOfObjects**. Hello **alapértelmezett** értéke **setOfObjects**. A mintákkal kapcsolatban lásd a [JSON-fájlminták](#json-file-patterns) című szakaszt. |Nem |
| jsonNodeReference | Ha tooiterate és az adatok kinyerése egy tömb belül hello objektumok mezőben a hello azonos mintát, adja meg, hogy tömb hello JSON elérési útját. Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat. | Nem |
| jsonPathDefinition | Adja meg a hello JSON elérési út minden oszlop leképezése egyéni oszlopnévvel (kisbetűvel kezdődik). Ez a tulajdonság csak akkor támogatott, ha JSON-fájlokból másol adatokat, és ki tud nyerni adatokat objektumokból vagy tömbökből. <br/><br/> A mezők a gyökérszintű objektum esetében indítsa el a legfelső szintű $; belül hello tömb által kiválasztott mező `jsonNodeReference` tulajdonság, hello tömbelem elindítását. Lásd: [JsonFormat példa](#jsonformat-example) hogyan szakasz tooconfigure. | Nem |
| encodingName |Adja meg a hello kódolási név. Érvényes kódolási nevek hello listájáért lásd: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonság. Például: windows-1250 vagy shift_jis. Hello **alapértelmezett** értéke: **UTF-8**. |Nem |
| nestingSeparator |Az karakter, amely használt tooseparate beágyazási szinttel. hello alapértelmezett érték "." (pont). |Nem |

#### <a name="json-file-patterns"></a>JSON-fájlminták

A másolási tevékenység a JSON-fájlok következő mintáit tudja elemezni:

- **I. típus: setOfObjects**

    Minden fájl egyetlen objektumot, illetve több, sorokkal határolt/összefűzött objektumot tartalmaz. Ha ezt a lehetőséget választja egy kimeneti adatkészletben, a másolási tevékenység egyetlen JSON-fájlt állít elő, soronként egy objektummal (sorokkal határolt).

    * **példa egy objektumot tartalmazó JSON-fájlra**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **példa sorokkal határolt JSON-fájlra**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **példa összefűzött JSON-fájlra**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **II. típus: arrayOfObjects**

    Minden fájl objektumok egy tömbjét tartalmazza.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

#### <a name="jsonformat-example"></a>A JsonFormat használatát bemutató példa

**1. eset: Adatok másolása JSON-fájlokból**

Lásd az alábbi minták két típusú adatok másolásakor a JSON-fájlokat, és általános pontok toonote hello:

**1. példa: adatok kigyűjtése objektumból és tömbből**

Ez a minta elvárt egy legfelső szintű JSON-objektum leképezi a táblázatos eredmény toosingle rekord. Ha egy JSON-fájl a következő tartalmat hello:  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
és azt szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, kinyeri az adatokat a objektumok és a tömb toocopy:

| id | deviceType | targetResourceType | resourceManagmentProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban). Pontosabban:

- `structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor. Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége. A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.
- `jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása toocopy adatokat egy olyan tömbből, használhat **tömb [x] .property** hello hello x-edik objektumot, vagy a tulajdonság megadott értéke tooextract használható  **tömb [*] .property** toofind hello értéket az összes ilyen tulajdonságot tartalmazó objektumtól.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

**2. példa: közötti több objektumnak azonos mintát egy olyan tömbből, hello alkalmazása**

Ez a minta elvárt tootransform egy legfelső szintű JSON-objektum táblázatos eredményben több bejegyzésekbe. Ha egy JSON-fájl a következő tartalmat hello:  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
és szeretné, hogy azt egy Azure SQL táblázatba hello alábbi formázása, hello hello tömbben található adatok közül egybesimítását toocopy Keresztillesztés információval hello közös legfelső szintű:

| ordernumber | orderdate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P2 | 13 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P3 | 231 | [{"sanmateo":"No 1"}] |

bemeneti adatkészlet hello **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban). Pontosabban:

- `structure`szakasz határozza meg a testreszabott hello oszlopneveket és a megfelelő adattípus hello tootabular adatok konvertálásakor. Ez a szakasz **választható** kivéve toodo oszlopleképezés van szüksége. A további részletekkel kapcsolatban lásd a [Struktúrameghatározások négyszögletes adatkészletek esetén](#specifying-structure-definition-for-rectangular-datasets) című szakaszt.
- `jsonNodeReference`azt jelzi, ugyanaz a mintát hello hello objektumok tooiterate és hogyan nyerhet ki adatokat **tömb** orderlines.
- `jsonPathDefinition`az egyes oszlopok, ahol tooextract hello adatait jelző hello JSON elérési út megadása Ebben a példában a "ordernumber", "orderdate oszlopra" és "város" vannak a legfelső szintű objektum kezdve "$.", amíg a "order_pd" és "order_price" meg van határozva, származó hello tömbelem nélkül "$." elérési úttal rendelkező JSON-útvonalhoz.

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

**Vegye figyelembe a következő pontok hello:**

* Ha hello `structure` és `jsonPathDefinition` nincsenek megadva itt: hello adat-előállító adatkészlet hello másolási tevékenység észleli hello hello első objektum sémáját, és egybesimítására hello teljes objektum.
* Ha hello JSON-bevitelben tömb, alapértelmezés szerint hello másolási tevékenység alakítja hello teljes tömb érték karakterlánc. Választhat tooextract adatok használatával `jsonNodeReference` és/vagy `jsonPathDefinition`, vagy hagyja ki a nem ad meg a legyen `jsonPathDefinition`.
* Ha duplikált nevek: hello ugyanazon a szinten, szerzi hello legutóbb hello másolási tevékenység.
* A tulajdonságnevek megkülönböztetik a kis- és nagybetűket. A rendszer két azonos nevű, de eltérő kis- és nagybetűket tartalmazó tulajdonságot két különálló tulajdonságként kezel.

**2. eset: Adatok tooJSON fájl írása**

Ha a következő táblával rendelkezik az SQL Database-ben:

| id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

és az egyes rekordokhoz, várt toowrite tooa JSON-objektumból az alábbi formátumban:
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

hello kimeneti adatkészlet **JsonFormat** típusa a következők: (csak hello vonatkozó alkotórészek részleges definícióban). Pontosabban `structure` szakaszban definiálja a testre szabott hello tulajdonságnevek célfájlt, `nestingSeparator` (alapértelmezett érték ".") használt tooidentify hello nest réteg hello neve lesz. Ez a szakasz **választható** kivéve, ha szeretné, hogy toochange hello tulajdonságnév összehasonlítva az forrásoszlop neve, vagy beágyazni hello tulajdonságok.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

### <a name="specifying-avroformat"></a>Az AvroFormat megadása
Ha szeretné, hogy tooparse hello az Avro-fájlok vagy az Avro formátum hello adatírás, állítsa be a hello `format` `type` tulajdonság túl**AvroFormat**. Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell. Példa:

```json
"format":
{
    "type": "AvroFormat",
}
```

az Avro-formátumban toouse Hive táblákat, olvassa el a túl[Apache Hive oktatóanyag](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Vegye figyelembe a következő pontok hello:  

* Az [összetett adattípusok](http://avro.apache.org/docs/current/spec.html#schema_complex) nem támogatottak (rekordok, felsorolt típusok, tömbök, leképezések, egyesítések és rögzített típusok).

### <a name="specifying-orcformat"></a>OrcFormat megadása
Ha szeretné, hogy tooparse hello ORC fájlokat vagy hello adatírás ORC formátumban, állítsa be a hello `format` `type` tulajdonság túl**OrcFormat**. Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell. Példa:

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> Nem másolása ORC fájlokat **,-van** között a helyszíni és felhőalapú adattároló, az átjáró gépen tooinstall hello JRE 8 (Java Runtime Environment) van szüksége. A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges. Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605). Válassza ki a hello megfelelő egy.
>
>

Vegye figyelembe a következő pontok hello:

* Az összetett adattípusok nem támogatottak (STRUCT, MAP, LIST, UNION)
* Az ORC-fájlok három, [tömörítéshez kapcsolódó beállítással](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) rendelkeznek: NONE, ZLIB, SNAPPY. A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását. Használja hello tömörítési kodek hello metaadatok tooread hello adatok van. Azonban tooan ORC fájl írásakor, adat-előállító úgy dönt, ZLIB, ez utóbbi érték az ORC hello alapértelmezett. Jelenleg nincs lehetőség toooverride ezt a viselkedést.

### <a name="specifying-parquetformat"></a>ParquetFormat megadása
Ha szeretné, hogy tooparse hello Parquet fájlok vagy hello adatírás Parquet formátumban, állítsa be a hello `format` `type` tulajdonság túl**ParquetFormat**. Toospecify hello formátum szakasz hello typeProperties szakaszon belül bármely tulajdonságok nem kell. Példa:

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> Nem Parquet fájlok másolása **,-van** között a helyszíni és felhőalapú adattároló, tooinstall hello JRE 8 (Java Runtime Environment) a átjáró számítógépre van szüksége. A 64 bites átjáróhoz 64 bites JRE, a 32 bites átjáróhoz 32 bites JRE szükséges. Mindkét verziót megtalálja [itt](http://go.microsoft.com/fwlink/?LinkId=808605). Válassza ki a hello megfelelő egy.
>
>

Vegye figyelembe a következő pontok hello:

* Az összetett adattípusok nem támogatottak (MAP, LIST)
* Parquet fájl rendelkezik-e hello alábbi tömörítési kapcsolatos beállítások: NONE, SNAPPY GZIP és LZO. A Data Factory a három tömörített formátum bármelyikében lévő ORC-fájlokból támogatja az adatok olvasását. Hello tömörítési kodek hello metaadatok tooread hello adatokat használ. Azonban tooa Parquet fájl írásakor, adat-előállító úgy dönt, SNAPPY, hello alapértelmezett Parquet formátum. Jelenleg nincs lehetőség toooverride ezt a viselkedést.
