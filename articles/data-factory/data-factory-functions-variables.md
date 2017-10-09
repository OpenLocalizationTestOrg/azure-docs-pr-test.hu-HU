---
title: "aaaData gyári funkciók és rendszerváltozók |} Microsoft Docs"
description: "Azure Data Factory funkciók és rendszerváltozók listája"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: d2936c2821797947bb37d9775226a6c19c4b8ab9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Az Azure Data Factory - funkciók és a rendszer változói
Ez a cikk tájékoztatást ad azokról a funkciók és az Azure Data Factory által támogatott változók.

## <a name="data-factory-system-variables"></a>Data Factory rendszerváltozók
| Változó neve | Leírás | Objektum hatóköre | JSON-hatókör és a használati esetek |
| --- | --- | --- | --- |
| WindowStart |Időtartam a jelenlegi művelet ablakban futtassa elindítása |Tevékenység |<ol><li>Adja meg a kijelölés lekérdezések. Olvassa el a hello összekötő cikk [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk.</li> |
| WindowEnd |Időtartam a jelenlegi művelet ablakban futtassa vége |Tevékenység |ugyanaz, mint WindowStart. |
| SliceStart |Az adatszelet előállítását időintervallumát elindítása |Tevékenység<br/>Adatkészlet |<ol><li>Adja meg a dinamikus mappák elérési útjaiban és fájlneveket dolgozva [Azure Blob](data-factory-azure-blob-connector.md) és [fájlrendszer adatkészletek](data-factory-onprem-file-system-connector.md).</li><li>Adja meg a bemeneti függőségek a data factory funkciók a tevékenység bemenetei gyűjteményben.</li></ol> |
| SliceEnd |Aktuális adatszelet időintervallumát végét. |Tevékenység<br/>Adatkészlet |ugyanaz, mint SliceStart. |

> [!NOTE]
> Jelenleg adat-előállító megköveteli, hogy hello ütemezhet a megadott hello tevékenység pontosan egyezik a megadott hello kimeneti adatkészlet rendelkezésre állását hello ütemezése. Ezért WindowStart, a WindowEnd, és a SliceStart és a SliceEnd mindig leképezése toohello ugyanaz az időszak és egy kimeneti szelet időpont.
> 

### <a name="example-for-using-a-system-variable"></a>Példa a rendszer változó segítségével
A következő példa, év, hónap, nap és idején hello a **SliceStart** ki kell olvasni a által használt külön változók **folderPath** és **Fájlnév** tulajdonságait.

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a>Data Factory-funkciók
Használható funkciók mellett rendszerváltozók adat-előállítóban hello a következő célra:

1. Kijelölés lekérdezések megadása (hello által hivatkozott összekötő cikkekben talál [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk.
   
   a data factory függvény szintaxis tooinvoke hello:  **$$ <function>**  kijelölés lekérdezések és egyéb tulajdonságok hello tevékenység és adathalmazokat.  
2. Bemeneti függőségek megadása a data factory funkciók a tevékenység bemenetei gyűjteményben.
   
    $$ nem szükséges bemeneti függőségi kifejezés megadásával.     

A következő minta hello **sqlReaderQuery** tulajdonság egy JSON-fájlban hello által visszaadott tooa értéket kapja `Text.Format` függvény. Ez a minta is használja a rendszer nevű változó **WindowStart**, amely hello tevékenységfuttatási ablak hello kezdési időt jelenti.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Lásd: [egyéni dátum és idő formátumú karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx) foglalkozó témakör használható különböző formázási beállítások (például: éééé és pi). 

### <a name="functions"></a>Functions
a következő táblák hello Azure Data Factory összes hello függvény listában:

| Kategória | Függvény | Paraméterek | Leírás |
| --- | --- | --- | --- |
| Time |AddHours(X,Y) |X: dátum és idő <br/><br/>Y int |Hozzáadja a megadott idő X Y óra toohello. <br/><br/>Példa:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Time |AddMinutes(X,Y) |X: dátum és idő <br/><br/>Y int |Y perc tooX hozzáadja.<br/><br/>Példa:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Time |StartOfHour(X) |X: dátum és idő |Lekérdezi a hello kezdési idő hello óráig hello óra összetevőjét X képviseli. <br/><br/>Példa:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Dátum |AddDays(X,Y) |X: dátum és idő<br/><br/>Y int |Y nap tooX hozzáadja. <br/><br/>Példa: 9/15/2013 12:00:00 PM + 2 nap = 9/17/2013 12:00:00 PM.<br/><br/>Nap túl kivonás Y megadása egy negatív számot.<br/><br/>Példa: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Dátum |AddMonths(X,Y) |X: dátum és idő<br/><br/>Y int |Y hónap tooX hozzáadja.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>Hónap túl kivonás Y megadása egy negatív számot.<br/><br/>Példa: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Dátum |AddQuarters(X,Y) |X: dátum és idő <br/><br/>Y int |Hozzáadja a Y * 3 hónapos tooX.<br/><br/>Példa:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Dátum |AddWeeks(X,Y) |X: dátum és idő<br/><br/>Y int |Hozzáadja a Y * tooX 7 nap<br/><br/>Példa: 9/15/2013 12:00:00 PM + 1 hét = 9/22-es/2013 12:00:00 PM<br/><br/>Hét túl kivonás Y megadása egy negatív számot.<br/><br/>Példa: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Dátum |AddYears(X,Y) |X: dátum és idő<br/><br/>Y int |Y év tooX hozzáadja.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>Év túl kivonás Y megadása egy negatív számot.<br/><br/>Példa: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Dátum |Day(X) |X: dátum és idő |Lekérdezi a hello nap összetevőjének megállapítása X.<br/><br/>Példa: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Dátum |DayOfWeek(X) |X: dátum és idő |Lekérdezi a hello nap, hét összetevőjének X.<br/><br/>Példa: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Dátum |DayOfYear(X) |X: dátum és idő |Hello év összetevőjét X által képviselt hello évben hello nap lekérdezi.<br/><br/>Példák:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Dátum |DaysInMonth(X) |X: dátum és idő |Hello hónap összetevőjét X paraméter által képviselt hello hónap napjainak hello lekérdezi.<br/><br/>Példa: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`. |
| Dátum |EndOfDay(X) |X: dátum és idő |Hello dátum idejű hello napja (nap összetevőjének), X hello végét jelölő lekérdezi.<br/><br/>Példa: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Dátum |EndOfMonth(X) |X: dátum és idő |Lekérdezi a hónap összetevőjét X paraméter által képviselt hello hónap hello végét. <br/><br/>Példa: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (dátuma és időpontja hello szeptember a hónap végét jelölő) |
| Dátum |StartOfDay(X) |X: dátum és idő |Hello start hello nap összetevőjének megállapítása X paraméter által képviselt hello nap lekérdezi.<br/><br/>Példa: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| Dátum és idő |FROM(X) |X: karakterlánc |X tooa karakterlánc elemzése időpontja. |
| Dátum és idő |Ticks(X) |X: dátum és idő |Lekérdezi a hello ticks X hello paraméter tulajdonsága. Egy osztásjelek 100 nanoszekundumban egyenlő. Ez a tulajdonság értékének hello ticks, 1 januárt, 12:00:00 éjfél óta eltelt 0001 hello számát jelöli. |
| Szöveg |Format(X) |X: karakterlánc-változóvá |Formátumok hello szöveg (használata `\\'` kombinációja tooescape `'` karakter).|

> [!IMPORTANT]
> Egy másik függvényen belül függvény használatakor nem kell toouse  **$$**  hello belső függvény előtag. Például: $$Text.Format ("PartitionKey eq \\" my_pkey_filter_value\\"és a RowKey ge \\" {0: éééé-hh-nn óó: pp:}\\", Time.AddHours (SliceStart, -6)). Figyelje meg, hogy a jelen példában  **$$**  előtag nem használják-e hello **Time.AddHours** függvény. 

#### <a name="example"></a>Példa
Hello a következő példa, valamint bemeneti és kimeneti paraméterek hello Hive tevékenység határozzák meg hello segítségével `Text.Format` függvény és SliceStart rendszer változó. 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a>2. példa

A következő példa hello hello DateTime paraméter hello tárolt eljárási tevékenység szöveg hello segítségével határozza meg. Függvény formázása és hello SliceStart változó. 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>3. példa
tooread adatok előző nap hello SliceStart, által jelölt nap helyett használja a hello napokHozzaadasa függvényt, ahogy az alábbi példa hello: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
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

Lásd: [egyéni dátum és idő formátumú karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx) foglalkozó témakör használható különböző formázási beállítások (például: nn és éééé). 

