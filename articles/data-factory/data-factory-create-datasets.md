---
title: Azure Data Factory aaaCreate adathalmazok |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate adatkészletek az Azure Data Factoryben, például használó példákkal eltolás és a anchorDateTime."
keywords: "a dataset adatkészlet példa létrehozása, eltolás: Példa"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: shlo
ms.openlocfilehash: 181859ed250595d756df73e9ebcac08d9e7184c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="datasets-in-azure-data-factory"></a>Az Azure Data Factory adathalmazok
Ez a cikk ismerteti, milyen adatkészletek, hogyan vannak definiálva JSON formátumú, és hogy ezek hogyan használhatók az Azure Data Factory folyamatok. Az adatkészlet JSON-definícióban hello biztosít az egyes szakaszokon (például struktúra, rendelkezésre állási és házirend) adatait. hello cikket is példákat hello segítségével **eltolás**, **anchorDateTime**, és **stílus** tulajdonságok az adatkészlet JSON-definícióban.

> [!NOTE]
> Ha új tooData gyári, lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md) áttekintése. Ha nem rendelkezik adat-előállítók létrehozása a gyakorlati tapasztalatok, akkor is értelmezésében által olvasása hello [data transformation oktatóanyag](data-factory-build-your-first-pipeline.md) és hello [adatok mozgása oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="overview"></a>Áttekintés
A data factory egy vagy több folyamattal rendelkezhet. A **csővezeték** logikai csoportosítása **tevékenységek** , amelyek együtt a feladat elvégzésére. Egy adatcsatorna tevékenységeinek hello műveletek tooperform határozza meg az adatokat. Például előfordulhat, hogy használja a másolási tevékenység toocopy adatokat egy helyi SQL Server tooAzure Blob-tároló. Ezután használhatja a egy Hive tevékenységet, amely egy parancsprogramot futtathat Hive egy Azure HDInsight fürt tooprocess adatokon a Blob storage tooproduce kimeneti adatokat. Végül használhat egy második másolási tevékenység toocopy hello kimeneti adatok tooAzure SQL Data Warehouse, mely üzleti intelligenciával reporting megoldások beépített felett. Adatcsatornák és tevékenységgel kapcsolatos további információkért lásd: [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md).

Egy tevékenység is igénybe vehet a nulla vagy több bemeneti **adatkészletek**, majd előállítanak egy vagy több kimeneti adatkészletek. Egy bemeneti adatkészlet hello bemeneti hello feldolgozási soros tevékenység, és egy kimeneti adatkészlet hello kimeneti hello tevékenység jelenti. Az adatkészletek adatokat határoznak meg a különböző adattárakban, például táblákban, fájlokban, mappákban és dokumentumokban. Például egy Azure Blob-adathalmazra megadja hello blob tároló és mappa mely hello a feldolgozási sor olvassa el a hello adatok Blob Storage tárolóban. 

A DataSet adatkészlet létrehozása előtt hozzon létre egy **társított szolgáltatás** toolink az adatok tárolásához toohello adat-előállítóban. Társított szolgáltatások sokkal hasonlóak a kapcsolati karakterláncok, amelyek a Data Factory tooconnect tooexternal erőforrások szükséges hello kapcsolati adatainak megadása. Adatkészletek azonosíthatja az adatokat belül hello kapcsolódó adatokat tárolja, például az SQL-táblák, fájlok, mappák és dokumentumok. Például egy Azure Storage társított szolgáltatás hivatkozások a tárolási fiók toohello adat-előállítóban. Egy Azure Blob-adathalmazra hello blobtárolót és hello bemeneti BLOB toobe feldolgozott tartalmazó hello mappát jelöli. 

Íme egy példa. Blob storage tooa SQL-adatbázis adatait toocopy, két társított szolgáltatások létrehozásához: Azure Storage és az Azure SQL Database. Ezután hozzon létre két adatkészletet: Azure Blob-adathalmazra (amely a toohello Azure tárolás társított szolgáltatásának) és (amely a toohello csatolt Azure SQL Database szolgáltatás) Azure SQL-tábla a dataset. hello Azure Storage és az Azure SQL Database összekapcsolt szolgáltatások használó Data Factory futásidejű tooconnect tooyour Azure Storage és az Azure SQL Database, illetve kapcsolati karakterláncokat tartalmazhat. hello Azure Blob-adathalmazra hello blobtárolót és hello bemeneti BLOB a Blob Storage tárolóban tartalmazó blob mappát adja meg. hello Azure SQL-tábla a dataset határozza meg, az SQL adatbázis toowhich hello adatokon hello SQL táblázat toobe másolja.

hello következő diagramon láthatók hello folyamat, a tevékenység, a dataset és a társított szolgáltatás közötti kapcsolatok szerepelnek az adat-előállító: 

![Adatcsatorna, adatkészlet, tevékenység társított szolgáltatások közötti kapcsolat](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>Adatkészlet JSON
A Data Factory dataset az alábbiak szerint definiáltuk JSON formátumban:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

a következő táblázat hello hello fent JSON-tulajdonságokat ismerteti:   

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| név |Hello DataSet adatkészlet neve. Lásd: [Azure Data Factory - elnevezési szabályait](data-factory-naming-rules.md) elnevezési szabályait. |Igen |NA |
| type |Hello dataset típusa. Adjon meg egy adat-előállító által támogatott hello típusok (például: AzureBlob, AzureSqlTable). <br/><br/>További információkért lásd: [adathalmaztípushoz](#Type). |Igen |NA |
| struktúra |Hello adatkészlet sémája.<br/><br/>További információkért lásd: [adatkészlet-szerkezetekben](#Structure). |Nem |NA |
| typeProperties | hello típustulajdonságokat eltérőek az egyes (például: Azure Blob, Azure SQL-tábla). További részletek az azok tulajdonságait és a támogatott hello esetében: [adathalmaztípushoz](#Type). |Igen |NA |
| external | Logikai jelző toospecify, hogy a DataSet adatkészlet explicit módon hozzák a data factory-folyamat vagy nem. Hello bemeneti adatkészlet a tevékenység nem hozzák hello aktuális folyamatot, ha a jelző tootrue beállítása Állítsa be a jelző tootrue hello bemeneti adatkészlet hello első tevékenység hello folyamat.  |Nem |hamis |
| rendelkezésre állás | Hello feldolgozása ablak (például óránként vagy naponta) vagy a felosztás hello dataset üzemi modell hello határoz meg. Felhasznált és egy tevékenység futott által előállított adatok tárolóegységekhez egy adatszelet nevezik. Ha egy kimeneti adatkészlet rendelkezésre állását hello beállítása toodaily (gyakoriság -, időköz - 1 nap), a szelet naponta elő. <br/><br/>További információkért lásd: [adatkészlet rendelkezésre állási](#Availability). <br/><br/>További tudnivalók hello dataset modell felosztás: hello [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk. |Igen |NA |
| szabályzat |Hello feltételek vagy hello dataset szeletek kell néhány előfeltételnek hello feltételt határoz meg. <br/><br/>További információkért lásd: hello [Dataset házirend](#Policy) szakasz. |Nem |NA |

## <a name="dataset-example"></a>A DataSet – példa
A következő példa hello, hello dataset nevű táblázatot jelölő **MyTable** SQL-adatbázisban.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Vegye figyelembe a következő pontok hello:

* **típus** tooAzureSqlTable van beállítva.
* **tableName** típusa (típus: adott tooAzureSqlTable) tulajdonsága tooMyTable.
* **linkedServiceName** tooa társított szolgáltatás típusa AzureSqlDatabase, amelyet a következő kódrészletet hello a JSON hivatkozik. 
* **rendelkezésre állási gyakoriság** értéke tooDay, és **időköz** too1 van beállítva. Ez azt jelenti, hogy hello dataset szelet naponta jön létre.  

**AzureSqlLinkedService** az alábbiak szerint definiáltuk:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

A JSON-részlet megelőző hello:

* **típus** tooAzureSqlDatabase van beállítva.
* **connectionString** típusú tulajdonság határozza meg az információkat tooconnect tooa SQL-adatbázis.  

Ahogy látja, hello társított szolgáltatás meghatározása hogyan tooconnect tooa SQL-adatbázis. hello dataset határozza meg, milyen tábla bemenetként használja, ezért egy folyamat hello tevékenység kimeneti.   

> [!IMPORTANT]
> Kivéve, ha a DataSet adatkészlet hello folyamat alatt keletkezik, azt kell megjelölni **külső**. Ez a beállítás általában az adatcsatorna első tevékenység tooinputs vonatkozik.   


## <a name="Type"></a>A DataSet típusa
hello hello adatkészlet függ hello tárolót használ. Tekintse meg a következő táblázat felsorolja a Data Factory által támogatott adattároló hello. Kattintson egy adatokat tároló toolearn hogyan toocreate összekapcsolt szolgáltatás és az adatok dataset tárolja.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Adatok tárolja a * lehet helyszíni vagy Azure-infrastruktúra (IaaS) szolgáltatás. Ezen adatok áruház használatához tooinstall [az adatkezelési átjáró](data-factory-data-management-gateway.md).

Az előző szakaszban hello hello példában hello típusú hello adatkészlet túl van beállítva**AzureSqlTable**. Ehhez hasonlóan egy Azure-Blob adatkészlet hello típusú hello dataset értéke túl**AzureBlob**, ahogy az alábbi JSON hello:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <a name="Structure"></a>Adatkészlet-szerkezetekben
Hello **struktúra** szakasz nem kötelező megadni. Hello séma hello adatkészlet nevét és az oszlopok adattípusát gyűjteményét tartalmazó szerint határozza meg. Hello struktúra szakasz tooprovide típus használt tooconvert típusok és információk hello forrás toohello cél térkép oszlopokat használja. A következő példa hello, hello dataset három oszlopot tartalmaz: `slicetimestamp`, `projectname`, és `pageviews`. Vannak típusú karakterlánc, karakterlánc, és tizedes, illetve.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Minden egyes oszlopának hello struktúra tartalmaz hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello oszlop neve. |Igen |
| type |Hello oszlop adattípusát.  |Nem |
| Kulturális környezet |. A NET-alapú kulturális környezet toobe használható, ha hello típusú .NET-típus: `Datetime` vagy `Datetimeoffset`. hello alapértelmezett érték a `en-us`. |Nem |
| Formátumban |Formázza használható, ha hello típus a .NET-típus: karakterlánc toobe: `Datetime` vagy `Datetimeoffset`. |Nem |

hello következő irányelvek segítségével meghatározhatja, mikor tooinclude szerkezeti információkat, és milyen tooinclude a hello **struktúra** szakasz.

* **A strukturált adatforrások**, adja meg a hello struktúra szakaszban csak akkor, ha azt szeretné, hogy a forrás oszlopok toosink oszlop leképezése, és vannak a nevek nem hello azonos. Strukturált adatforrás az ilyen adatok séma- és tároló típus maga hello adatokkal együtt. Strukturált adatforrások például SQL Server, az Oracle és az Azure-tábla. 
  
    Típusra vonatkozó adat már áll rendelkezésre a strukturált adatforrásokhoz, akkor nem tartalmazhat típusinformációt amikor hello struktúra szakasz tartalmazza.
* **Olvassa el az adatforrások (kifejezetten Blob-tároló) a séma**, dönthet úgy, toostore adatok nélkül hello adatokkal bármely séma vagy típusú információk tárolására. Az ilyen típusú adatforrások tartalmazhat struktúra, ha azt szeretné, hogy toomap forrásoszlopokat oszlopok toosink. Amennyiben hello adatkészletet másolási tevékenységhez bemeneti, és adattípusok forrás adatkészlet hello fogadó konvertált toonative típust kell terjednie az struktúra. 
    
    Adat-előállító támogatja a következő struktúrában típusú adatokat ad értékeinek hello: **Int16, Int32, Int64, egyetlen, Double, Decimal, Byte [], logikai érték, karakterlánc, Guid, Datetime, Datetimeoffset és Timespan**. Ezek az értékek közös nyelvi specifikáció (CLS)-kompatibilis. A NET-alapú típusú értékeket.

Adat-előállító automatikusan típuskonverziók hajt végre, a forrásadatok az adattároló tooa fogadó adattár áthelyezésekor. 
  

## <a name="dataset-availability"></a>Adatkészlet rendelkezésre állása
Hello **rendelkezésre állási** hello feldolgozási időszakában (például óránként, naponta, vagy hetente) hello adatkészlet rész DataSet adatkészletben. Tevékenység windows kapcsolatos további információkért lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md).

hello rendelkezésre állással kapcsolatos szakaszának alábbi határozza meg, hogy hello kimeneti adathalmazt vagy előállított óránként vagy hello bemeneti adatkészlet érhető el Óránként:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Ha hello folyamat a következő kezdési és befejezési időpontjai hello:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

hello kimeneti adatkészlet hozzák óránkénti belül hello folyamat kezdési és befejezési időpontja. Ezért nincsenek öt dataset szeletek által az adatcsatornát, egy az egyes tevékenységek ablakát (12 óra - 13 AM, 13: 00 – 2 óra, Reggel 2 - 3 AM, hajnali 3 Órakor - 4 AM, hajnali 4 Órakor – 5: 00). 

hello következő táblázat segítségével is hello rendelkezésre állással kapcsolatos szakaszának tulajdonságok:

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| frequency |Megadja a dataset szelet üzemi hello időegységét.<br/><br/><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap |Igen |NA |
| interval |Megadja a gyakoriság egy szorzóval.<br/><br/>"X időköz" határozza meg, milyen gyakran hello szelet jön létre. Például, ha a dataset toobe szeletelhetők óránként kell hello, beállíthatja <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.<br/><br/>Vegye figyelembe, hogy ha megad **gyakoriság** , **perc**, 15-nál kisebb hello időköz toono kell beállítani. |Igen |NA |
| stílus |Meghatározza, hogy hello szelet akkor a rendszer hello kezdő vagy hello időköz végén.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Ha **gyakoriság** értéke túl**hónap**, és **stílus** értéke túl**EndOfInterval**, hello szelet a hónap utolsó napján hello jön létre. Ha **stílus** értéke túl**StartOfInterval**, hello szelet hozzák hello a hónap első napján.<br/><br/>Ha **gyakoriság** értéke túl**nap**, és **stílus** értéke túl**EndOfInterval**, hello szelet előállított hello hello naponta az elmúlt egy órában.<br/><br/>Ha **gyakoriság** értéke túl**óra**, és **stílus** értéke túl**EndOfInterval**, hello szelet hello végének hello keletkezik. Például a hello du. 1-2 PM időszak adatszelethez, hello szelet hozzák 2 du.. |Nem |EndOfInterval |
| anchorDateTime |Hello abszolút pozíciója a ennyi másodpercig használta hello Feladatütemező toocompute dataset szelet határok meghatározása. <br/><br/>Vegye figyelembe, hogy a propoerty részekből dátum, amelyek részletesebb hello megadottnál gyakorisága, ha hello részletesebb részek figyelmen kívül lesznek hagyva. Például, ha hello **időköz** van **óránkénti** (gyakoriság: óra és időköz: 1), és hello **anchorDateTime** tartalmaz **percet és másodpercet**, majd a perc és a másodperc részét hello **anchorDateTime** figyelmen kívül lesznek hagyva. |Nem |01/01/0001 |
| Az offset |TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette. <br/><br/>Ne feledje, ha mindkét **anchorDateTime** és **eltolás** megadott, hello eredménye kombinált hello shift. |Nem |NA |

### <a name="offset-example"></a>az eltolási – példa
Alapértelmezés szerint naponta (`"frequency": "Day", "interval": 1`) szeletek start: 00 (éjfél) egyezményes világidő (UTC). Ha ehelyett hello kezdési idő toobe reggel 6 óra UTC idő szerint, be eltolásnál, ahogy az alábbi részlet hello hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime – példa
A következő példa hello hello dataset 23 óránként jön létre. hello első szelet elindítja időpontban hello által megadott **anchorDateTime**, melynek értéke túl`2017-04-19T08:00:00` (idő szerint UTC).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Az offset/style – példa
hello következő dataset van havi, és a hello jön létre havonta, de a 3. (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>A DataSet házirend
Hello **házirend** hello feltételek rész az hello adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell.

### <a name="validation-policies"></a>Házirendek érvényesítése
| Házirend neve | Leírás | Alkalmazott túl| Szükséges | Alapértelmezett |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Ellenőrzi, hogy hello adatok **Azure Blob Storage tárolóban** megfelel-e hello (megabájtban) a minimális méret követelményeinek. |Azure Blob Storage |Nem |NA |
| minimumRows |Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza. |<ul><li>Az Azure SQL-adatbázis</li><li>Azure-tábla</li></ul> |Nem |NA |

#### <a name="examples"></a>Példák
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Külső adatkészletek
Külső adatkészletek hello gazdarendszerhez egy futó folyamatot hello adat-előállítóban nem hozzák létre. Ha hello dataset van megjelölve **külső**, hello **ExternalData** szabályzata lehet hello dataset szelet rendelkezésre állási meghatározott tooinfluence hello viselkedését.

Kivéve, ha a Data Factory hozzák alatt álló adatkészlet, azt kell megjelölni **külső**. Ez a beállítás általában egy folyamatot, az első tevékenység toohello bemenetek vonatkozik, kivéve, ha a tevékenység vagy csővezeték-láncolás használatban van.

| Név | Leírás | Szükséges | Alapértelmezett érték |
| --- | --- | --- | --- |
| dataDelay |a adva szelet hello idő toodelay hello hello rendelkezésre állását hello hello külső adatok ellenőrzése. Például egy óránkénti ellenőrzést késleltetheti a beállítás használatával.<br/><br/>hello beállítás csak akkor érvényes toohello jelenlegi idő.  Például ha 1:00 PM azonnal, és az értéke 10 perc hello érvényesítési kezdődik, 1:10 óra.<br/><br/>Vegye figyelembe, hogy ez a beállítás nem befolyásolja az elmúlt hello szeletek. A szeletek **szelet befejezésének** + **dataDelay** < **most** dolgoznak fel késedelem nélkül.<br/><br/>Időpontokban nagyobb, mint 23:59 óra meg kell határozni hello segítségével `day.hours:minutes:seconds` formátumban. Például toospecify 24 órát, ne használjon 24:00:00. Ehelyett használjon 1.00:00:00. Ha 24:00:00 használja, akkor a rendszer 24 napos (24.00:00:00). 1 nap és 4 óra adja meg 1:04:00:00. |Nem |0 |
| RetryInterval |hello várakozási idő hibákhoz és hello következő kísérlet között. Ez a beállítás toopresent idő vonatkozik. Ha hello előző próbálja sikertelen volt, hello következő kísérlet után hello van **retryInterval** időszak. <br/><br/>Ha 1:00 PM most, az első lépések hello első próbálkozás. Hello időtartama toocomplete hello első eredetiség ellenőrzésének 1 perc és hello művelet végrehajtása sikertelen volt, hello legközelebbi újrapróbálkozás akkor 1:00 + 1 perc (időtartam) + (újrapróbálkozási időköz) 1 perc = 1:02 PM. <br/><br/>Az elmúlt hello szeletek nincs késleltetés. hello újrapróbálkozási azonnal történik. |Nem |00:01:00 (1 perc) |
| retryTimeout |hello időkorlátjának újrapróbálkozási kísérletei.<br/><br/>Ha ez a tulajdonság értéke too10 perc hello érvényesítési 10 percen belül kell végrehajtani. Ha hosszabb ideig tart, mint 10 percig tooperform hello érvényesítési, hello időtúllépést próbálkozzon újra.<br/><br/>Ha bármilyen kísérlet a hello érvényesítési túllépi az időkorlátot, hello szelet jelölésű **időtúllépésbe került**. |Nem |00:10:00 (10 perc) |
| maximumRetry |hello számú alkalommal fordult elő az toocheck a hello hello külső adatok rendelkezésre állását. hello megengedett maximális érték: 10. |Nem |3 |


## <a name="create-datasets"></a>Adatkészletek létrehozása
Az ezen eszközök vagy az SDK-k adatkészletek hozhat létre: 

- Másolás varázsló 
- Azure Portal
- Visual Studio
- PowerShell
- Azure Resource Manager-sablon
- REST API
- .NET API

Tekintse meg a lépésenkénti útmutatók adatcsatornákat és adathalmazokat egyikével a következő eszközök, illetve az SDK-k létrehozása a következő hello:
 
- [Adatátalakítási tevékenységgel rendelkező folyamat létrehozása](data-factory-build-your-first-pipeline.md)
- [Adatok mozgása tevékenységgel folyamat létrehozása](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Miután egy folyamat létrehozása és telepítése, felügyelheti és figyelheti a folyamatok használatával hello Azure portál paneleken, vagy hello figyelés és a felügyeleti alkalmazás. Tekintse meg a következő témakörök részletes utasításokat hello: 

- [Figyelheti és folyamatok kezelése az Azure portál paneleken használatával](data-factory-monitor-manage-pipelines.md)
- [Figyelheti és kezelheti a folyamatok hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Hatókört használó adatkészletek
Létrehozhat hello segítségével hatókörön belüli tooa csővezeték adatkészletek **adatkészletek** tulajdonság. Ezek az adatkészletek csak akkor használható, ez az adatcsatorna tevékenységét, nem pedig más folyamatok tevékenységek. a következő példa hello folyamat két adatkészletet (InputDataset-rdc és OutputDataset-rdc) toobe hello csővezeték történik az határozza meg.  

> [!IMPORTANT]
> Egyszeri adatcsatornák csak a hatókörön belüli adatkészletek támogatottak (ahol **pipelineMode** értéke túl**OneTime**). Lásd: [Onetime csővezeték](data-factory-create-pipelines.md#onetime-pipeline) részleteiről.
>
>

```json
{
    "name": "CopyPipeline-rdc",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Következő lépések
- Folyamatok kapcsolatos további információkért lásd: [hozzon létre adatcsatornák](data-factory-create-pipelines.md). 
- Hogyan adatcsatornák ütemezett és végrehajtott kapcsolatos további információkért lásd: [ütemezés és a végrehajtása az Azure Data Factory](data-factory-scheduling-and-execution.md). 
