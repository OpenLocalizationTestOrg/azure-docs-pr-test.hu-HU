---
title: "aaaScheduling és a Data Factory végrehajtási |} Microsoft Docs"
description: "Ismerje meg az Azure Data Factory alkalmazásmodell ütemezés és a végrehajtási aspektusait."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory ütemezés és a végrehajtás
Ez a cikk ismerteti a hello ütemezés és a végrehajtási aspektusainak hello Azure Data Factory alkalmazásmodellt. Ez a cikk feltételezi, hogy tudomásul veszi a Data Factory alkalmazás modell fogalmakat, beleértve a tevékenység, a folyamatok, a társított szolgáltatások és a adatkészletek alapjait. Azure Data Factory alapvető fogalmait tekintse meg a következő cikkek hello:

* [Bevezetés tooData gyári](data-factory-introduction.md)
* [Folyamatok](data-factory-create-pipelines.md)
* [Adatkészletek](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Feldolgozási folyamat kezdő és befejező időpontja
Egy folyamat aktív csak között a **start** idő és **end** idő. Nem hajtotta végre, hello kezdete előtt vagy után hello befejezési időpontja. Hello csővezeték fel van függesztve, ha nem végre függetlenül a kezdő és záró idő. Az a folyamat toorun azt kell nem lehet szüneteltetni. Ezek a beállítások (kezdő, célból szünetel) található hello csővezeték definíciója: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

További információ: ezek a Tulajdonságok [hozzon létre adatcsatornák](data-factory-create-pipelines.md) cikk. 


## <a name="specify-schedule-for-an-activity"></a>Egy tevékenység ütemezése
Hello folyamat, amely végrehajtja a rendszer nincs. Hello hello adatcsatorna tevékenységeinek, amelyek hello hello folyamat teljes környezetében. Hello segítségével is megadhat egy ismétlődő ütemezés megadása egy tevékenység **Feladatütemező** tevékenység JSON szakasza. Például ütemezheti egy tevékenység toorun óránkénti az alábbiak szerint:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Hello a következő ábrán az látható, annak ütemezését, hogy egy tevékenység hoz létre egy sorozatát átfedésmentes windows hello-feldolgozási folyamat kezdési és befejezési időpontja. Átfedésmentes windows rendszer rögzített méretű mozaikként, átfedés nélkül, összefüggő időközök sorozata. A logikai átfedésmentes egy tevékenység hívjuk **tevékenység windows**.

![Tevékenység Feladatütemező – példa](media/data-factory-scheduling-and-execution/scheduler-example.png)

Hello **Feladatütemező** tevékenység tulajdonsága nem kötelező. Ha megadja ezt a tulajdonságot, akkor meg kell egyeznie a hello tevékenység kimeneti adatkészlet hello definíciójában megadott hello ütemben történik. Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ezért egy kimeneti adatkészlet kell létrehoznia, még akkor is, ha hello tevékenység nem ad kimenetet. 

## <a name="specify-schedule-for-a-dataset"></a>Adja meg a DataSet adatkészlet ütemezését
A Data Factory-folyamat egy tevékenységének is igénybe vehet a nulla vagy több bemeneti **adatkészletek** , majd előállítanak egy vagy több kimeneti adatkészletek. Egy tevékenység megadhat hello ütemben történik, mely hello érhetők el a bemeneti adatok vagy hello kimeneti adatok hozzák használó hello **rendelkezésre állási** hello dataset definíciók című szakasza. 

**Gyakoriság** a hello **rendelkezésre állási** szakasz hello időegység határozza meg. hello két érték engedélyezett: a gyakoriság: perc, óra, nap, hét és hónap. Hello **időköz** hello rendelkezésre állással kapcsolatos szakaszának a tulajdonság határozza meg a gyakoriság egy szorzóval. Példa: Ha hello gyakoriságának beállítása tooDay, és egy kimeneti adatkészlet too1 időköz, hello kimeneti adatok naponta jön létre. Perc gyakoriság hello ad meg, azt javasoljuk, hogy állítsa a hello időköz toono kisebb, mint 15. 

A következő példa hello, hello bemeneti adatok elérhető óránkénti és hello kimeneti adatok rendszer óránként készít adatszeletet (`"frequency": "Hour", "interval": 1`). 

**Bemeneti adatkészletet:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


**Kimeneti adatkészlet**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Jelenleg **kimeneti adatkészlet meghajtók hello ütemezés**. Más szóval hello kimeneti adatkészlet megadott hello ütemezése használt toorun futásidőben tevékenységgel. Ezért egy kimeneti adatkészlet kell létrehoznia, még akkor is, ha hello tevékenység nem ad kimenetet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet. 

Hello alábbi csővezeték-definíció, hello **Feladatütemező** tulajdonsága hello tevékenység használt toospecify ütemezését. Ez a tulajdonság nem kötelező. Hello ütemezés hello tevékenység jelenleg hello kimeneti adatkészlet megadott hello ütemezése egyeznie kell.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

Ebben a példában a hello tevékenység fut óránkénti hello közötti kezdési, és a befejezési időpontja hello folyamatának. hello kimeneti adatok óránkénti állítanak elő háromórás windows (8 de - de, Reggel 9-10 Reggel 9, és 10 de - de). 

Felhasznált vagy egy futtatása tevékenység által létrehozott adatok tárolóegységekhez nevezik egy **adatszelet**. a következő diagram hello egy bemeneti adatkészlet és egy kimeneti adatkészlet a tevékenység példáját mutatja be: 

![Rendelkezésre állási Feladatütemező](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

hello ábrán az látható hello óránkénti adatszeletek hello a bemeneti és kimeneti adatkészlet. hello ábrán látható, amely készen áll a feldolgozás három bemeneti szeletek. hello 10-11 AM tevékenység van folyamatban, hello 10-11 AM kimeneti szeletet előállító. 

Hello alatt az időtartam alatt hello aktuális szelet hello adatkészlet JSON társított változók használatával végezheti el: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) és [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Hasonló módon érheti el egy tevékenység ablakban társított hello WindowStart és WindowEnd hello időintervallumban. hello ütemezés tevékenység hello ütemezés hello kimeneti adatkészlet hello tevékenységhez meg kell egyeznie. Ezért hello SliceStart és a SliceEnd értékek vannak hello azonos WindowStart és WindowEnd értékként kulcsattribútumokkal. Ezek a változók további információkért lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables) cikkeket.  

Ezek a változók többféle célra használhatja a a tevékenység JSON-NÁ. Például használhatja őket adatsorozat időadatok képviselő bemeneti és kimeneti adatkészletek tooselect adatait (például: 8 óra too9 VAGYOK). Ez a példa **WindowStart** és **WindowEnd** tooselect vonatkozó adatokat egy tevékenységhez futtatni, és másolja a megfelelő hello tooa blob **folderPath**. Hello **folderPath** óránként paraméteres toohave külön mappába van.  

Példa megelőző hello, a megadott bemeneti és kimeneti adatkészletek hello ütemezése van hello azonos (óránként). Ha hello bemeneti adatkészlet hello tevékenység elérhető különböző gyakorisággal mondja ki 15 percenként, a hello tevékenység, amely létrehozza a kimeneti adatkészletet továbbra is fut óránként, mert hello kimeneti adatkészlet milyen meghajtók hello tevékenységütemezést. További információkért lásd: [eltérő gyakorisággal adatkészletek modell](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>Adatkészlet rendelkezésre állását és házirendek
Amint láthatta hello használat gyakorisága és időköz tulajdonságokat hello rendelkezésre állással kapcsolatos szakaszának adatkészlet-definícióban. Nincsenek néhány más tulajdonságok, amelyek hello ütemezés és a tevékenység végrehajtása. 

### <a name="dataset-availability"></a>Adatkészlet rendelkezésre állása 
hello következő táblázat ismerteti a hello használható tulajdonságok **rendelkezésre állási** szakasz:

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| frequency |Megadja a dataset szelet üzemi hello időegységét.<br/><br/><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap |Igen |NA |
| interval |Megadja egy szorzóval gyakoriság<br/><br/>"X időköz" határozza meg, milyen gyakran hello szelet jön létre.<br/><br/>Ha dataset toobe szeletelhetők óránként kell hello, akkor be <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.<br/><br/><b>Megjegyzés:</b>: perces gyakoriságot ad meg, ha azt javasoljuk, hogy állítsa a 15-nál kisebb hello időköz toono |Igen |NA |
| stílus |Meghatározza, hogy kell-e hello szelet előállított hello kezdő/záró hello időköz.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Ha gyakoriságának beállítása tooMonth és stílus tooEndOfInterval van beállítva, hello szelet a hónap utolsó napján hello elő. Ha hello stílus be van állítva tooStartOfInterval, hello szelet elő hello a hónap első napján.<br/><br/>Ha gyakoriságának beállítása tooDay és stílus tooEndOfInterval van beállítva, hello szelet elő az elmúlt egy órában hello nap hello.<br/><br/>Ha gyakoriság tooHour és stílus tooEndOfInterval van beállítva, hello szelet hello végének hello keletkezik. Például a szelet du. 1 – 2 óra időszakban, a hello szelet hozzák 2 du.. |Nem |EndOfInterval |
| anchorDateTime |Hello abszolút pozíciója a ennyi másodpercig használta a Feladatütemező toocompute dataset szelet határok meghatározása. <br/><br/><b>Megjegyzés:</b>: Ha hello AnchorDateTime részekből dátum, amelyek részletesebben, mint a hello gyakorisága, akkor hello részletesebb részek figyelmen kívül lesznek hagyva. <br/><br/>Például, ha hello <b>időköz</b> van <b>óránkénti</b> (gyakoriság: óra és időköz: 1) és hello <b>AnchorDateTime</b> tartalmaz <b>percet és másodpercet</b>, majd hello <b>percet és másodpercet</b> hello AnchorDateTime részei a rendszer figyelmen kívül hagyja. |Nem |01/01/0001 |
| Az offset |TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette. <br/><br/><b>Megjegyzés:</b>: Ha anchorDateTime és az offset is meg van adva, hello eredménye kombinált hello shift. |Nem |NA |

### <a name="offset-example"></a>az eltolási – példa
Alapértelmezés szerint naponta (`"frequency": "Day", "interval": 1`) szeletek kezdjék 12 óra UTC idő szerint (éjfél). Ha ehelyett hello kezdési idő toobe reggel 6 óra UTC idő szerint, be eltolásnál, ahogy az alábbi részlet hello hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime – példa
A következő példa hello hello dataset 23 óránként jön létre. hello első szelet elindítja időpontban hello hello anchorDateTime, melynek értéke túl által megadott`2017-04-19T08:00:00` (UTC idő).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Az offset/stílus – példa
hello következő dataset havi adatkészlet és a 3. minden hónap 8:00 órakor hozzák (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>A DataSet házirend
Egy adatkészlet tartozhat egy meghatározott érvényesség-ellenőrzési házirend, amely meghatározza, hogyan olyan szelet végrehajtással által létrehozott hello adatok is ellenőrizni kell, mielőtt azt készen áll a felhasználásra. Ilyen esetekben hello szelet végrehajtás befejezését követően hello kimeneti szelet állapota túl**Várakozás** rendelkező, a részállapot **érvényesítési**. Hello szeletek ellenőrzését követően hello szelet állapota túl**készen**. Ha egy adatszelet készült, de nem felelt meg a hello érvényesítési, alsóbb rétegbeli szeletek a szelet függő tevékenység fut nincsenek feldolgozva. [Megfigyelés és kezelés folyamatok](data-factory-monitor-manage-pipelines.md) magában foglalja az adat-előállítóban adatszeletek különböző állapotok hello.

Hello **házirend** hello feltételek rész az adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell. hello következő táblázat ismerteti a hello használható tulajdonságok **házirend** szakasz:

| Házirend neve | Leírás | Alkalmazott túl| Szükséges | Alapértelmezett |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Ellenőrzi, hogy hello adatokat egy **Azure blob** megfelel-e hello (megabájtban) a minimális méret követelményeinek. |Azure-blob |Nem |NA |
| minimumRows | Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza. |<ul><li>Azure SQL Database</li><li>Azure-tábla</li></ul> |Nem |NA |

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

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Ezek a tulajdonságok és példák kapcsolatos további információkért lásd: [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. 

## <a name="activity-policies"></a>A tevékenység-szabályzatok
A házirendek milyen hatással hello futtatási viselkedés tevékenység, kifejezetten egy tábla hello szelet feldolgozásakor. a következő táblázat hello hello részletes adatokat biztosít.

| Tulajdonság | Megengedett értékek | Alapértelmezett érték | Leírás |
| --- | --- | --- | --- |
| Egyidejűségi |Egész szám <br/><br/>A maximális érték: 10 |1 |Egyidejű végrehajtások hello tevékenység száma.<br/><br/>Meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek. Például ha egy tevékenység toogo keresztül elérhető adatokat, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Meghatározza, hogy hello sorrendje feldolgozott adatszeletek.<br/><br/>Például ha 2 szeletek (du. 4: egy azonban és délután 5 óra egy másik tulajdonságnak), és mindkét végrehajtási függőben van. Ha hello executionPriorityOrder toobe NewestFirst, hello szelet, délután 5 óra lesz elsőként feldolgozva. Hasonlóképpen ha hello executionPriorityORder toobe OldestFIrst, majd du. 4: hello szelet feldolgozása. |
| retry |Egész szám<br/><br/>A maximális érték 10 is lehet. |0 |Hello adatfeldolgozási hello adatszelethez előtt újrapróbálkozások száma hiba van megjelölve. Egy adatszelet tevékenység végrehajtása a rendszer ismét megkísérli megadott toohello mentése újrapróbálkozások száma. hello újrapróbálkozási minél hamarabb hello meghibásodás után történik. |
| timeout |A TimeSpan |00:00:00 |Hello tevékenység időkorlátját. . Példa: 00:10:00 (azt jelenti, időtúllépés 10 perc)<br/><br/>Ha az érték nincs megadva vagy 0, hello időtúllépési érték végtelen.<br/><br/>Szelet hello adatok feldolgozási ideje meghaladja a hello időtúllépési értéket, ha azt megszakadt, és hello rendszer próbál tooretry hello feldolgozása. Az újrapróbálkozások számát hello hello újrapróbálkozási tulajdonság függ. Időtúllépés esetén hello beállítás tooTimedOut. |
| Késleltetés |A TimeSpan |00:00:00 |Adja meg a hello késleltetés hello szelet elindítja az adatok feldolgozása előtt.<br/><br/>egy adatszelet tevékenységet hello végrehajtása után hello késleltetés hello várt végrehajtási ideje elmúlt elindult.<br/><br/>. Példa: 00:10:00 (magában foglalja a késleltetést a 10 perc) |
| hosszú újrapróbálkozás |Egész szám<br/><br/>A maximális érték: 10 |1 |hosszú újrapróbálkozások számát hello tett kísérletet, mielőtt hello szelet végrehajtása sikertelen volt.<br/><br/>hosszú újrapróbálkozás kísérletek által longRetryInterval távolságban helyezkednek el. Ha toospecify kell egy újrapróbálkozási kísérletek között eltelt idő, így hosszú újrapróbálkozás használja. Ha mind az újra gombra, és a hosszú újrapróbálkozás meg van adva, minden hosszú újrapróbálkozás kísérlet tartalmazza az ismételt kísérletek számát, és hello kísérletek maximális száma újrapróbálkozási * hosszú újrapróbálkozás.<br/><br/>Ha például tudunk hello hello tevékenység házirend beállításai a következő:<br/>Próbálkozzon újra: 3<br/>hosszú újrapróbálkozás: 2. régiója<br/>longRetryInterval: 01:00:00<br/><br/>Feltételezik, hogy csak egy szelet tooexecute (állapot vár) hello tevékenység végrehajtási minden olyan alkalommal sikertelen lesz. Eredetileg nem lenne 3 egymást követő végrehajtási kísérletek. Minden kísérlet után hello szelet állapota lenne az újra gombra. Miután először 3 kísérletet: keresztül, hello szelet állapota hosszú újrapróbálkozás lesz.<br/><br/>Egy óra (Ez azt jelenti, hogy longRetryInteval tartozó érték) nem lenne a 3 egymást követő végrehajtási kísérletek egy másik készlet. Ezt követően hello szelet állapota akkor sikertelen, és nincs további újrapróbálkozások volna kísérli meg. Ezért a teljes 6 történt kísérlet.<br/><br/>Ha bármely végrehajtása sikeres, hello szelet állapota Kész és nincs további újrapróbálkozások próbált vannak.<br/><br/>hosszú újrapróbálkozás függő adatok nem determinisztikus időpontokban érkeznek vagy hello teljes környezete alapján akkor következik be, mely az adatfeldolgozás flaky használható. Ezekben az esetekben nem újrapróbálkozások egymás után ez segíthet, és kimeneti így időt eredményez hello időköz után szükséges.<br/><br/>Járjon el a Word: nincs beállítva hosszú újrapróbálkozás vagy longRetryInterval magas értékeit. Általában a magasabb értékkel rendszeres problémákkal utalnak. |
| longRetryInterval |A TimeSpan |00:00:00 |hello kísérletek hosszú újrapróbálkozási kísérletek között eltelt idő |

További információkért lásd: [folyamatok](data-factory-create-pipelines.md) cikk. 

## <a name="parallel-processing-of-data-slices"></a>Párhuzamos feldolgozás adatszeletek
Hello adatcsatorna hello kezdő dátuma múltbeli hello állíthatja be. Ha így tesz, a Data Factory automatikusan kiszámítja (hátsó kitöltés) múltbeli hello összes adatszeletek, és elkezdi őket. Példa: Ha a kezdő dátum 2017-04-01 hoz létre egy folyamatot, és hello aktuális dátum 2017-04-10. Ha hello ütemben történik a hello kimeneti adatkészlet naponta, majd a Data Factory indítása 2017-04-01-től az összes hello szelet feldolgozása too2017-04-09 azonnal mert hello kezdő dátum van hello múltbeli. 2017-04-10-hello szelet nem lett feldolgozva még mert hello hello rendelkezésre állással kapcsolatos szakaszának a style tulajdonságának értéke EndOfInterval alapértelmezés szerint. hello legrégebbi szelet feldolgozása először hello alapértelmezett executionPriorityOrder értéke OldestFirst. Hello style tulajdonságának ismertetését lásd: [adatkészlet rendelkezésre állási](#dataset-availability) szakasz. Hello executionPriorityOrder szakasz ismertetését lásd: hello [tevékenység szabályzatai](#activity-policies) szakasz. 

Adatok biztonsági kitöltött szeletek toobe által hello beállítása párhuzamosan is beállíthat **egyidejűségi** hello tulajdonság **házirend** hello tevékenység JSON szakasza. Ez a tulajdonság meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek. hello alapértelmezett hello párhuzamossági tulajdonság értéke 1. Ezért egy szelet feldolgozása egyszerre alapértelmezés szerint. hello maximális érték: 10. Ha egy folyamatot kell toogo keresztül a rendelkezésre álló adatok, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása. 

## <a name="rerun-a-failed-data-slice"></a>Futtassa újra a sikertelen adatszelet
Adatok szelet feldolgozása során hiba lép fel, ha azt megtudhatja, miért szelet hello feldolgozása nem sikerült, az Azure portál paneleken vagy a figyelő és App kezelése. Lásd: [figyelése és kezelése az Azure portál paneleken használatával folyamatok](data-factory-monitor-manage-pipelines.md) vagy [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md) részleteiről.

Vegye figyelembe a következő példának, amely mutatja a két tevékenység hello. Activity1 és 2 tevékenység. Activity1 Dataset1 szelet használ fel, és Dataset2, amely runbook az Activity2 tooproduce hello végső Dataset szelet által felhasznált bemeneti adatokként a szelet eredményez.

![Hibás szeletet](./media/data-factory-scheduling-and-execution/failed-slice.png)

hello ábra azt mutatja, hogy három legutóbbi szeletek, kívül történt hiba előállító hello 9-10 de szelet Dataset2 a. Adat-előállító automatikusan nyomon követi a függőségi hello idő adatsorozat adatkészlethez. Emiatt nem indul el hello tevékenységfuttatási hello Reggel 9-10 alárendelt adatszelethez.

Data Factory figyelése és a felügyeleti eszközöket oszthatja toodrill hello diagnosztikai naplók hello hibás szeletet tooeasily keresés hello legfelső szintű hello probléma okozza, és javítsa ki azt. Hello a probléma kijavítása után futtassa tooproduce hello hibás szeletet hello tevékenység könnyen megkezdheti. További információt a toorerun és állapotváltozási adat áramlik az adatok megismeréséhez, lásd: [figyelése és kezelése az Azure portál paneleken használatával folyamatok](data-factory-monitor-manage-pipelines.md) vagy [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md).

Után futtassa újra a hello 9-10 AM a szelet **Dataset2**, adat-előállító hello 9-10 de függő adatszelethez futtatnak hello végső dataset hello kezdődik.

![Futtassa újra a sikertelen szelet](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Több tevékenység egy adott folyamatban
Egy folyamathoz azonban több tevékenység is tartozhat. Ha több tevékenység rendelkezik egy folyamatot, és a hello tevékenység kimenete nem egy másik tevékenység bemeneti, hello tevékenységek párhuzamos futtathatja, ha készen áll a bemeneti adatok szeletek hello tevékenységekhez.

Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után). hello tevékenységek hello lehetnek azonos vagy különböző kimenetátirányítási. hello második tevékenység végrehajtása csak akkor, ha hello először egy futtatása sikeresen befejeződött.

Vegyük példaként a következő esetet, ahol a folyamat két tevékenység rendelkezik hello:

1. Külső bemeneti adatkészlet D1, és az előállított kimeneti adatkészlet D2 igénylő tevékenységek A1.
2. A kimeneti adatkészlet D3 tevékenység A2 D2 adatkészletből beavatkozást igényel, és hozza létre.

Ez a forgatókönyv, tevékenységek A1 és A2 vannak a hello azonos a következő feldolgozási sorban. hello tevékenység A1 fut, amikor hello külső adatok érhető el, és ütemezett hello rendelkezésre állási gyakoriság éri el. hello tevékenység A2 fut, amikor hello a D2 ütemezett szeletek rendelkezésre állására, és hello ütemezett rendelkezésre állása gyakoriság éri el. Ha nincs hiba fordult elő egy adatkészlet D2 hello szeletek, A2 nem fut a, hogy a szelet amíg elérhetővé válik.

Diagram nézet hello mindkét tevékenységeinek azonos jelenne meg a következő diagram hello hello:

![Láncolás hello tevékenységek ugyanazt a következő feldolgozási sorban](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

A korábban említett hello tevékenységek a különböző folyamatok lehet. Ilyen esetben a következő diagram hello hello diagramnézet jelenne meg:

![Két kimenetátirányítási titkosításblokkoló tevékenységek](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Lásd: hello [egymás után másolja](#copy-sequentially) szakasz hello függelékben példát.

## <a name="model-datasets-with-different-frequencies"></a>Eltérő gyakorisággal modell adatkészletek
Hello minták, a bemeneti és kimeneti adatkészletek és hello tevékenység ütemezési ablak a hello gyakoriságot volt hello azonos. Egyes esetekben szükséges hello képességét tooproduce kimeneti egy vagy több bemeneti hello gyakoriságát eltérő gyakorisággal. Adat-előállító támogatja a modellezési forgatókönyvekben.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>1. példa: A bemeneti adatok óránként napi kimeneti jelentést készít.
Fontolja meg egy olyan forgatókönyvet, amelyben van megadott mérési adatokat az érzékelők érhető el az Azure Blob storage óránként. Hello nap tooproduce statisztikákról, például átlag, maximális és minimális napi összesítő jelentés kívánt [adat-előállító hive tevékenység](data-factory-hive-activity.md).

Hogyan modellezhető ebben a forgatókönyvben a Data Factory itt található:

**Bemeneti adatkészlet**

hello óránkénti bemeneti fájlok dobja a megadott nap hello hello mappájában. Bemeneti rendelkezésre állásra van beállítva **óra** (gyakoriság: óra, időköz: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Kimeneti adatkészlet**

Egy kimeneti fájl hello nap mappában jön létre naponta. Kimeneti rendelkezésre állását nem állítják **nap** (gyakoriság: nap és időköz: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Tevékenység: struktúra egy feldolgozási soros tevékenység**

hello hive parancsfájl kap megfelelő hello *DateTime* hello használó paraméterek adatként **WindowStart** változó, ahogy az alábbi részlet hello. hello hive parancsfájlokat használ a változó tooload hello adatok hello megfelelő mappából hello napon, és futtassa a hello összesítési toogenerate hello kimeneti.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

hello alábbi ábrán látható hello forgatókönyv egy adat-függőség szempontjából.

![Függőség](./media/data-factory-scheduling-and-execution/data-dependency.png)

hello kimeneti szelet esetén minden nap 24 óránként szeletek a egy bemeneti adatkészlet függ. Data Factory kiszámítja ezeket a függőségeket, hogy tudja által automatikusan hello bemeneti adatszeletek, amely az hello azonos időszak szerint előállított kimeneti szelet toobe hello. Ha hello 24 bemeneti szeletek bármelyike nem érhető el, adat-előállító megvárja-e a hello bemeneti szelet toobe készen áll a hello napi tevékenységfuttatási megkezdése előtt.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>2. példa: Adja meg a függőségi kifejezések és a Data Factory-funkciók
Mérlegeljük, egy másik helyzet. Tegyük fel, a hive tevékenység, amely feldolgozza a két bemeneti adatkészletek. Egyik új adatokat naponta, de egyikük kap új adatokat minden héten. Tegyük toodo illesztés hello két bemenet közötti, majd előállít egy kimenő minden nap.

hello egyszerű módszert, amelyben Data Factory automatikusan adatok kimenő hello jobb bemeneti tooprocess szeletek egymáshoz igazításával toohello kimeneti adatok szelet időszak nem működik.

Meg kell adnia, hogy minden tevékenységfuttatási hello adat-előállító érdemes használni a múlt héten adatszelet hello heti bemeneti adatkészlet. Használhatja az Azure Data Factory funkciók, ahogy az alábbi részlet tooimplement hello ezt a viselkedést.

**Input1: Az Azure blob**

hello első bemenet hello Azure blob alatt naponta frissül.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Input2: Az Azure blob**

Input2 hello Azure blob hetente frissítése folyamatban.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Kimenete: Az Azure blob**

Egy kimeneti fájl létrejön naponta hello mappában hello nap. Kimeneti rendelkezésre állását értéke túl**nap** (gyakoriság: nap, időköz: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Tevékenység: struktúra egy feldolgozási soros tevékenység**

hello hive tevékenység hello két bemeneti fogad, és egy kimeneti szelet naponta eredményez. Minden nap kimeneti szelet toodepend a következőképpen hello az előző hét bemeneti szelet heti bemeneti adhatja meg.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

Lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md) funkciók és a Data Factory támogató rendszerváltozók listáját.

## <a name="appendix"></a>Függelék:

### <a name="example-copy-sequentially"></a>Példa: egymás után másolása
Azt az lehetséges toorun több másolási műveletek egymás után szekvenciális/rendezett módon. Például lehetséges, hogy két másolási folyamat (CopyActivity1 és CopyActivity2) a következő hello tevékenységek bemeneti adatok kimeneti adatkészletek:   

CopyActivity1

Bemenet: Dataset. Kimenet: Dataset2.

CopyActivity2

Bemenet: Dataset2.  Kimenet: Dataset3.

CopyActivity2 fog futni, ha hello CopyActivity1 végrehajtása sikeresen befejeződött, és Dataset2 érhető el.

Hello minta adatcsatorna JSON itt található:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Figyelje meg, hogy hello példában hello kimeneti adatkészlet a hello első másolási tevékenység (Dataset2) van megadva hello második tevékenység bemeneti adatként. Hello második tevékenység fut, ezért csak akkor, amikor készen áll a hello kimeneti adatkészlet hello első tevékenységből.  

Hello példában CopyActivity2 egy másik bemeneti Dataset3, például rendelkezhet, de ad meg Dataset2 egy bemeneti tooCopyActivity2, így hello tevékenység CopyActivity1 befejezéséig nem működik. Példa:

CopyActivity1

Bemenet: Dataset1. Kimenet: Dataset2.

CopyActivity2

Bemenetek: Dataset3, Dataset2. Kimenet: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

Figyelje meg, hogy hello példában két bemeneti adatkészletek vannak megadva hello második másolási tevékenységhez. Ha több bemeneti adatok meg vannak adva, csak hello első bemeneti adatkészletet szolgál az adatok másolásának, de más adatkészletek függőségek használatosak. CopyActivity2 kezdenie csak azt követően hello következő feltételek teljesülnek:

* CopyActivity1 sikeresen befejeződött, és Dataset2 érhető el. Ez az adatkészlet nem használt adatok tooDataset4 másolásakor. Csak működik ütemezési függőségei a CopyActivity2.   
* Dataset3 érhető el. Ez az adatkészlet másolt toohello cél hello adatot jelöli. 
