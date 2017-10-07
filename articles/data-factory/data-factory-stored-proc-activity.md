---
title: "aaaSQL kiszolgálón tárolt eljárási tevékenység"
description: "Ismerje meg, hogyan használhatja a hello SQL Server tárolt eljárási tevékenység tooinvoke a Data Factory-folyamat az Azure SQL Database vagy az Azure SQL Data Warehouse tárolt eljárást."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server tárolt eljárási tevékenység
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md) 
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

## <a name="overview"></a>Áttekintés
Adatok átalakítása tevékenységek használata egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) előrejelzéseket és elemzések tootransform és a folyamat nyers adatok. hello tárolt eljárási tevékenység egyike, amely támogatja a Data Factory hello átalakítása tevékenységeket. Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek adat-előállítóban általános áttekintést.

Hello tárolt eljárási tevékenység tooinvoke valamelyik hello adatokat a következő tárolt eljárás tárolja a vállalati vagy egy Azure virtuális gépen (VM) is használhatja: 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server-adatbázis.  Ha SQL Server használ, telepítse az adatkezelési átjáró hello azonos számítógépre, hogy a gazdagépek hello adatbázis, vagy egy külön számítógépen, amely rendelkezik hozzáférési toohello adatbázis. Az adatkezelési átjáró egy összetevő, amely összeköti az adatok a helyszínen vagy a Azure VM a cloud serviceshez adatforrásokat felügyelt és biztonságos módon. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikkben alább.

> [!IMPORTANT]
> Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke hello segítségével tárolt eljárás **sqlWriterStoredProcedureName** tulajdonság. További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md). További hello tulajdonságra vonatkozó további információkért lásd a következő összekötő cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Adatok másolása az Azure SQL Data Warehouse a másolási tevékenység során a tárolt eljárás meghívása nem támogatott. De az SQL Data Warehouse hello tárolt eljárás tevékenység tooinvoke tárolt eljárást használhatja. 
>  
> Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység tooinvoke egy tárolt eljárás tooread adatok forrásadatbázisból hello hello segítségével  **sqlReaderStoredProcedureName** tulajdonság. További információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


forgatókönyv használja a következő hello egy folyamat tooinvoke Azure SQL-adatbázisban tárolt eljárás a tárolt eljárási tevékenység hello. 

## <a name="walkthrough"></a>Útmutatás
### <a name="sample-table-and-stored-procedure"></a>A minta-tábla és tárolt eljárás
1. Hozza létre a következőket hello **tábla** az Azure SQL adatbázis SQL Server Management Studio vagy bármilyen más ismeri a Feladatkezelő segítségével. hello datetimestamp oszlop hello dátum és idő hello megfelelő azonosító létrehozásakor.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    Azonosító egyedi hello azonosított pedig hello datetimestamp oszlop hello dátum és idő hello megfelelő azonosító létrehozásakor.
    
    ![Mintaadatok](./media/data-factory-stored-proc-activity/sample-data.png)

    Ez a példa Azure SQL-adatbázisban egy hello tárolt eljárás. Hello tárolt eljárás egy Azure SQL Data warehouse-bA és az SQL Server-adatbázis, hello megközelítést akkor hasonló. SQL Server-adatbázis, telepítenie kell egy [az adatkezelési átjáró](data-factory-data-management-gateway.md).
2. Hozza létre a következőket hello **tárolt eljárás** , amely szúr be adatokat toohello **sampletable**.

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Név** és **kis-és** hello a paraméter (ebben a példában szereplő DateTime) meg kell egyeznie hello csővezeték/tevékenység JSON-ban megadott paramétert. A tárolt eljárás definition hello, ügyeljen arra, hogy  **@**  hello paraméter előtagjaként szolgál.

### <a name="create-a-data-factory"></a>Data factory létrehozása
1. Jelentkezzen be túl[Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** hello bal oldali menüben kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.

    ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. A hello **új adat-előállító** panelen adja meg **SProcDF** a hello nevét. Az Azure Data Factory neve **globálisan egyedi**. A névvel, tooenable hello sikeres létrehozása hello gyári hello adat-előállító tooprefix hello neve van szüksége.

   ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Válassza ki a **Azure-előfizetés**.
5. A **erőforráscsoport**, hajtsa végre az alábbi lépésekkel hello:
   1. Kattintson a **hozzon létre új** , és írja be a hello erőforráscsoport nevét.
   2. Kattintson a **meglévő** , és válasszon ki egy meglévő erőforráscsoportot.  
6. Jelölje be hello **hely** hello adat-előállító esetében.
7. Válassza ki **PIN-kód toodashboard** , hogy láthatóvá hello adat-előállító hello irányítópult jelentkezik be a következő alkalommal.
8. Kattintson a **létrehozása** a hello **új adat-előállító** panelen.
9. Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon. Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.

   ![Data Factory kezdőlap](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Azure SQL társított szolgáltatás létrehozása
Miután létrehozta a hello adat-előállítót, hozzon létre, amely az Azure SQL adatbázis, amely tartalmazza a hello sampletable tábla és sp_sample tárolt eljárás, tooyour adat-előállító Azure SQL társított szolgáltatásnak.

1. Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** paneljén **SProcDF** toolaunch hello Data Factory Editor.
2. Kattintson a **az új adattároló** a hello parancssávon, és válassza a **Azure SQL Database**. Meg kell jelennie a hello JSON-parancsfájl létrehozásához egy Azure SQL társított szolgáltatásnak hello-szerkesztőben.

   ![Új adattár](media/data-factory-stored-proc-activity/new-data-store.png)
3. Hello JSON-parancsfájl ellenőrizze a következő módosításokat hello:

   1. Cserélje le `<servername>` hello nevet, az Azure SQL Database-kiszolgálóhoz.
   2. Cserélje le `<databasename>` létrehozására használt tábla hello és hello hello adatbázissal tárolt eljárást.
   3. Cserélje le `<username@servername>` hello felhasználói fiókkal, amely rendelkezik toohello adatbázist.
   4. Cserélje le `<password>` a hello hello felhasználói fiók jelszavát.

      ![Új adattár](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon. Győződjön meg arról, hogy megjelenik-e hello AzureSqlLinkedService hello fában hello bal oldali megtekintése.

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Kimeneti adatkészlet létrehozása
Adjon meg egy kimeneti adatkészlet egy tárolt eljárás tevékenység még akkor is, ha hello tárolt eljárás nem hozhatók létre adatokat. Ennek oka az, az hello kimeneti adatkészlet, amely az hello ütemezés hello tevékenység (milyen gyakran hello tevékenység fut - óránként, naponta, stb.). hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis. hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) hello folyamat. Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia. Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat. Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset** (a DataSet adatkészlet mutat, tooa tábla hello kimenete valóban nem rendelkező tárolt eljárás). Az üres adatkészlet csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység szolgál. 

1. Ha nem látja ezt a gombot, kattintson a három pontot  **További** hello eszköztáron kattintson **új adatkészlet**, és kattintson a **Azure SQL**. **Új adatkészlet** hello parancs menüsoron, majd válassza a **Azure SQL**.

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/new-dataset.png)
2. Másolja és illessze be a következő JSON-parancsfájlok JSON-szerkesztőben toohello hello.

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy hello dataset, kattintson a **telepítés** hello parancssávon. Győződjön meg arról, hogy megjelenik-e hello dataset hello faszerkezetes nézetben.

    ![Fanézet, a társított szolgáltatások](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Hozzon létre egy folyamatot SqlServerStoredProcedure tevékenység
Most hozzon létre egy folyamatot egy tárolt eljárás tevékenységet. 

Figyelje meg a következő tulajdonságai hello: 

- Hello **típus** tulajdonsága túl**SqlServerStoredProcedure**. 
- Hello **storedProcedureName** típus tulajdonságainak túl van beállítva**sp_sample** (hello neve tárolt eljárás).
- Hello **storedProcedureParameters** szakasz nevű egy paraméter **/időből**. Név és a kis-és a JSON-ban hello paraméter meg kell egyeznie hello nevét és a kis-és nagybetűk hello paraméter hello tárolt eljárás definícióban. Ha az egyik paraméter null értékű kell átadni, szintaxissal hello: `"param1": null` (kisbetűket).
 
1. Ha nem látja ezt a gombot, kattintson a három pontot  **További** a hello parancssávon, és kattintson a **új adatcsatorna**.
2. Másolja és illessze be a következő JSON részlet hello:   

    ```JSON
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
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. toodeploy hello sorban, kattintson a **telepítés** hello eszköztáron.  

### <a name="monitor-hello-pipeline"></a>A figyelő hello folyamat
1. Kattintson a **X** tooclose Data Factory Editor paneleken toonavigate biztonsági toohello adat-előállító panelt, és kattintson **Diagram**.

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. A hello **diagramnézet**, az hello adatcsatornák áttekintés és adatkészletek szerepel ez az oktatóanyag.

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. A Diagram nézet hello, kattintson duplán a hello dataset `sprocsampleout`. Megjelenik a hello szeletek üzemkész állapotban. Lehetnek öt szeletek mert szelet állítanak elő minden órában hello kezdési és befejezési időpontot hello JSON között.

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. A szelet esetén a **készen áll a** állapot, futtassa a `select * from sampletable` hello Azure SQL adatbázis tooverify hello adatok lekérdezése toohello tábla, hello tárolt eljárás által beszúrt.

   ![kimeneti adatok](./media/data-factory-stored-proc-activity/output.png)

   Lásd: [figyelő hello adatcsatorna](data-factory-monitor-manage-pipelines.md) Azure Data Factory folyamatok figyelésével kapcsolatos részletes információk.  


## <a name="specify-an-input-dataset"></a>Adjon meg egy bemeneti adatkészlet
Hello forgatókönyv tárolt eljárási tevékenység nem rendelkezik a bemeneti adatkészletek. Ha megad egy bemeneti adatkészlet, hello tárolt eljárási tevékenység befejezéséig nem fut le hello szelet bemeneti adatkészlet érhető el (üzemkész állapotban). hello dataset lehet egy külső adatkészletet (nem egy másik tevékenysége hello által előállított azonos) vagy egy belső adatkészlet (a előtt ezt a tevékenységet futtató hello tevékenység) felsőbb szintű tevékenység által létrehozott. Hello tárolt eljárás tevékenység több bemeneti adatkészletet is megadhat. Ha így tesz, hello tárolt eljárás tevékenység fut csak akkor, ha minden hello bemeneti adatkészlet szeletek érhetők el (üzemkész állapotban). hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként. Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt.

## <a name="chaining-with-other-activities"></a>Más tevékenységek láncolás
Ha azt szeretné, hogy ehhez a tevékenységhez egy felsőbb szintű tevékenység toochain, adja meg a hello felsőbb szintű tevékenység kimenete hello tevékenység bemenetként. Ha így tesz, hello tárolt eljárási tevékenység befejezéséig nem fut hello felsőbb szintű tevékenység befejezése és a felsőbb szintű tevékenység hello hello kimeneti adatkészlet érhető el (a kész állapot). Megadhatja a több felsőbb szintű tevékenység kimeneti adatkészletek hello tárolt eljárás tevékenység bemeneti adatkészletek. Ha így tesz, hello tárolt eljárási tevékenység fut, csak akkor, ha minden hello bemeneti adatkészlet szeletek nem érhető el.  

A következő példa hello, hello hello másolási tevékenység eredménye: OutputDataset, ami bemenete hello a tárolt eljárási tevékenység. Ezért hello tárolt eljárási tevékenység befejezéséig nem fut le hello másolási tevékenység befejezése és hello OutputDataset szelet érhető el (üzemkész állapotban). Ha több bemeneti adatkészletek ad meg, hello tárolt eljárási tevékenység nem működik addig, amíg az összes hello bemeneti adatkészlet szeletek nem érhető el (üzemkész állapotban). hello bemeneti adatkészletek közvetlenül paraméterek toohello tárolt eljárási tevékenység nem használható. 

A láncolás tevékenységek további információkért lásd: [több tevékenységet egy folyamaton belül](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
        "activities": [
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
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

Ehhez hasonlóan toolink hello tároló eljárás tevékenység **alárendelt tevékenységek** (hello futtató tevékenységek hello tárolt eljárási tevékenység befejezése után), adja meg a hello kimeneti adatkészlet hello tárolt eljárás tevékenység, egy Adjon meg hello alárendelt tevékenység hello folyamat.

> [!IMPORTANT]
> Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke hello segítségével tárolt eljárás **sqlWriterStoredProcedureName** tulajdonság. További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md). Hello tulajdonságra vonatkozó további információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység tooinvoke egy tárolt eljárás tooread adatok forrásadatbázisból hello hello segítségével  **sqlReaderStoredProcedureName** tulajdonság. További információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>JSON formátumban
Íme hello JSON formátumban tárolt eljárási tevékenység meghatározásához:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

hello a következő táblázat ismerteti ezeket a JSON-tulajdonságokat:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név | Hello tevékenység neve. |Igen |
| leírás |Milyen hello tevékenységgel a leíró szöveg |Nem |
| type | Értékre kell állítani: **SqlServerStoredProcedure** | Igen |
| Bemenetek | Választható. Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) hello a tárolt eljárás tevékenység toorun. hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként. Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt. |Nem |
| kimenetek | Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet. Kimeneti adatkészlet megadja hello **ütemezés** hello a tárolt eljárási tevékenység (óránként, heti, havi, stb.). <br/><br/>hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis. <br/><br/>hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) hello folyamat. Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia. Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat. <br/><br/>Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység. |Igen |
| storedProcedureName |Hello Azure SQL database vagy az Azure SQL Data Warehouse vagy SQL Server adatbázis hello kapcsolódó szolgáltatás, amely a kimeneti tábla által használt hello által képviselt hello hello tárolt eljárás nevét adja meg. |Igen |
| storedProcedureParameters |Adja meg a tárolt eljárás paraméter értékét. Ha toopass null egy paraméter, szintaxissal hello: "param1": (összes kisbetű) NULL értékű. Tekintse meg a következő minta toolearn e tulajdonság használatával kapcsolatos hello. |Nem |

## <a name="passing-a-static-value"></a>Egy statikus értékre továbbításához
Most tegyük vegyen fel egy másik oszlop, "A forgatókönyv" nevű "Dokumentum minta" nevű statikus értéket tartalmazó hello táblában.

![Mintaadatokat 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tábla:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Tárolt eljárást:**

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Ezután továbbítja a hello **forgatókönyv** hello paraméter és hello értéket a tárolt eljárási tevékenység. Hello **typeProperties** című szakasza a következő kódrészletet hello minta tűnik megelőző hello:

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Data Factory adatkészlet:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Data Factory-folyamat**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```