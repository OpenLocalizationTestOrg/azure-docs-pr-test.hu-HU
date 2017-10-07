---
title: az SQL Server aaaMove adatok tooand |} Microsoft Docs
description: "Ismerje meg, hogyan toomove adatokat az SQL Server-adatbázis, amely a helyszíni vagy egy Azure virtuális gép Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Helyezze át az adatokat tooand a helyszíni SQL Server vagy az infrastruktúra-szolgáltatási (Azure virtuális gép), Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok belőle egy helyi SQL Server-adatbázis a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során. 

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **egy SQL Server-adatbázisból** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooa SQL Server-adatbázis**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Támogatott SQL Server-verziók
Az SQL Server connector támogatása az adatok másolásának / toohello következő helyszínen üzemeltetett példányt, vagy az SQL-hitelesítést és a Windows-hitelesítést használó Azure IaaS verziók: SQL Server 2016, az SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005

## <a name="enabling-connectivity"></a>Kapcsolat engedélyezése
hello fogalmakat és a helyszíni SQL Server által futtatott vagy az Azure infrastruktúra-szolgáltatási (infrastruktúra-,--szolgáltatás) virtuális gépek csatlakoztatásához szükséges lépéseket hello azonos történik. Mindkét esetben szükséges toouse az adatkezelési átjáró kapcsolat.

Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat. Kapcsolódás az SQL Server olyan előfeltételt egy átjárópéldányt beállítása.

Amíg átjárót telepítheti hello megegyezik a helyszíni gépen vagy a felhő Virtuálisgép-példány SQL Server hello a jobb teljesítmény, javasoljuk, hogy külön gépeken telepíteni. Hello átjáró és az SQL Server különböző gépeken csökkenti a Erőforrásverseny.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely a különböző eszközök/API-k használatával helyezi át az adatokat belőle egy helyi SQL Server-adatbázis egy folyamat hozhatja létre.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Például egy SQL Server adatbázis tooan Azure blob-tároló az adatok másolása, létrehozhat két összekapcsolt szolgáltatások toolink az SQL Server-adatbázis és az Azure storage tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooSQL Server-adatbázis, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 
3. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello példában dataset toospecify hello SQL tábla hello bemeneti adatokat tartalmazó az SQL Server-adatbázis létrehozása. Egy másik dataset toospecify hello blob tároló létrehozása és a hello adatokat tartalmazó hello mappába másolta át hello SQL Server-adatbázist. Adatkészlet tulajdonságai, amelyek adott tooSQL Server-adatbázis, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
4. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában SqlSource forrás-és BlobSink akár használhatja a fogadó hello másolási tevékenységhez. Ehhez hasonlóan az Azure Blob Storage tooSQL Server-adatbázis másolása, használható BlobSource és SqlSink hello másolási tevékenység. A másolási tevékenység tulajdonságait, amelyek adott tooSQL Server-adatbázis, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Minták használt toocopy adatok egy helyi SQL Server-adatbázis az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples-for-copying-data-from-and-to-sql-server) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooSQL Server részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban. a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.

a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello tulajdonságra kell megadni: **OnPremisesSqlServer**. |Igen |
| connectionString |Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges. |Igen |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja. |Igen |
| felhasználónév |Adja meg a felhasználónevet, ha a Windows-hitelesítést használ. Példa: **tartománynév\\felhasználónév**. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |

Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Példák
**Az SQL-hitelesítéssel JSON**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**A Windows-hitelesítést használó JSON**

Az adatkezelési átjáró fog megszemélyesíteni hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist. 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
A hello mintában használt típusú dataset **SqlServerTable** toorepresent egy SQL Server adatbázis egyik táblája.  

Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (SQL Server, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** típusú hello adatkészlet szakasz **SqlServerTable** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello SQL Server-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik. |Igen |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Ha adatokat egy SQL Server-adatbázisból, beállítása hello forrástípus hello másolási tevékenység túl**SqlSource**. Hasonlóképpen, ha az tooa SQL Server-adatbázist, beállítása hello a fogadó típusa hello másolási tevékenység túl**SqlSink**. Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.

Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

> [!NOTE]
> hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

### <a name="sqlsource"></a>SqlSource
Ha a másolási tevékenység során a forrás típusa nem **SqlSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. Hivatkozhatnak hello bemeneti adatkészlet által hivatkozott hello adatbázisból táblákat. Ha nincs megadva, az SQL-utasítás végrehajtása hello: táblanév kiválaszthatja. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello SQL Server-adatbázis forrás tooget hello adatok alapján.

Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

> [!NOTE]
> Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság. Nincs érvényesítést hajt végre ezt a táblázatot, ha van.

### <a name="sqlsink"></a>SqlSink
**SqlSink** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute lekérdezést, úgy, hogy egy adott szelet adatait. További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz. |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean. További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz. |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |
| sqlWriterStoredProcedureName |Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |
| sqlWriterTableType |Adja meg a tábla Típus neve toobe hello tárolt eljárásban használt. Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus. Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat. |Egy tábla környezettípus nevét. |Nem |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a>Az adatok és a kiszolgáló tooSQL másolására JSON példák
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). a következő minták megjelenítése hogyan hello toocopy adatok tooand az SQL Server és az Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a>Példa: Adatok másolása az SQL Server tooAzure Blob
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol idősorozat adatokat egy SQL Server tábla tooan Azure blob minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**Kapcsolódó SQL Server szolgáltatás**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Az Azure Blob storage társított szolgáltatás**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**SQL Server bemeneti adatkészlet**

hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz. Több tábla belül azonos adatbázist egyetlen dataset, de egy táblát kell használni az hello dataset tableName typeProperty hello keresztül kérdezheti le.

"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Az Azure Blob kimeneti adatkészlet**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**A másolási tevékenység-feldolgozási folyamat**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
Ebben a példában **sqlReaderQuery** hello SqlSource van megadva. hello másolási tevékenység fut ez a lekérdezés hello hello tooget SQL Server-adatbázis forrásadatot. Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el). hello sqlReaderQuery hello bemeneti adatkészlet által hivatkozott hello adatbázison belül több táblát is hivatkozik. Már nem korlátozott tooonly hello tábla adatkészlet tableName typeProperty hello beállítani.

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

Lásd: hello [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságokról hello listáját.

## <a name="example-copy-data-from-azure-blob-toosql-server"></a>Példa: Adatok másolása az Azure Blob tooSQL kiszolgáló
a következő példa azt mutatja be hello:

1. hello társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).
2. hello társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#sql-server-copy-activity-type-properties).

hello minta idősorozat adatainak másolása az Azure blob tooa SQL Server táblából óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Kapcsolódó SQL Server szolgáltatás**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Az Azure Blob storage társított szolgáltatás**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Az Azure Blob bemeneti adatkészlet**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az adott hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**SQL Server kimeneti adatkészlet**

hello minta másolja át az SQL Server "MyTable" nevű tooa adattábla. Hozzon létre hello táblát az SQL Server azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**A másolási tevékenység-feldolgozási folyamat**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="troubleshooting-connection-issues"></a>Kapcsolati problémák elhárítása
1. Az SQL Server tooaccept távoli kapcsolatok konfigurálása. Indítsa el **SQL Server Management Studio**, kattintson a jobb gombbal **server**, és kattintson a **tulajdonságok**. Válassza ki **kapcsolatok** hello listája és ellenőrzés **engedélyezés távoli kapcsolatok toohello server**.

    ![Távoli kapcsolatok engedélyezése](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Lásd: [hello távelérési kiszolgáló konfigurációs beállítás konfigurálása](https://msdn.microsoft.com/library/ms191464.aspx) a részletes lépéseket.
2. Indítsa el **SQL Server Konfigurációkezelő**. Bontsa ki a **SQL Server hálózati konfigurációja** hello a példány akkor kívánja, majd válassza **MSSQLSERVER protokolljai**. Meg kell jelennie protokollok hello jobb oldali ablaktáblában. Engedélyezze a TCP/IP protokollt kattintson a jobb gombbal **TCP/IP** elemre kattintva **engedélyezése**.

    ![Engedélyezze a TCP/IP protokollt](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    Lásd: [engedélyezheti vagy tilthatja le a hálózati protokoll](https://msdn.microsoft.com/library/ms191294.aspx) részleteit és más módon, hogy a TCP/IP-protokoll.
3. A hello azonos ablak, kattintson duplán a **TCP/IP** toolaunch **TCP/IP-tulajdonságok** ablak.
4. Váltás toohello **IP-címek** fülre. Görgessen lefelé toosee **IPAll** szakasz. Jegyezze fel a hello ** TCP-Port ** (alapértelmezett érték a **1433**).
5. Hozzon létre egy **szabály a Windows tűzfal hello** hello gép tooallow érkező forgalmat ezen a porton keresztül.  
6. **Ellenőrizze a kapcsolat**: tooconnect toohello teljesen minősített nevet használó SQL Server SQL Server Management Studio használja egy másik gépről. Például: "<machine>.<domain>. Corp.<company>.com, 1433. "

   > [!IMPORTANT]

   > Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md) részletes információkat.
   >
   > Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.
   >
   >


## <a name="identity-columns-in-hello-target-database"></a>Azonosító oszlop hello céladatbázis
Ez a szakasz azt mutatja, hogy az adatok átmásolja a forrás nincs azonosító oszlop tooa céltábla azonosító oszlopot tartalmazó tábla.

**Forrástábla:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Céltábla:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Figyelje meg, hogy hello céltábla tartalmaz azonosító oszlopot.

**Forrás adatkészlet JSON-definícióból**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
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
**Cél adatkészlet JSON-definícióból**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású). Ebben az esetben szüksége toospecify **struktúra** hello tároló adatkészlet-definícióban, amely nem tartalmazza a hello azonosító oszlop tulajdonsága.

## <a name="invoke-stored-procedure-from-sql-sink"></a>A fogadó SQL tárolt eljárás meghívása
Lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk SQL fogadó a folyamat a másolási tevékenység a tárolt eljárás meghívása példát.

## <a name="type-mapping-for-sql-server"></a>Az SQL server leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Ha áthelyezése adatok túl & SQL server, a hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz.

hello ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET hello lesz.

| SQL Server adatbázismotor típusa | .NET-keretrendszer típusa |
| --- | --- |
| bigint |Int64 |
| Bináris |Byte] |
| bit |Logikai érték |
| Karakter |Karakterlánc, Char] |
| Dátum |Dátum és idő |
| Dátum és idő |Dátum és idő |
| datetime2 |Dátum és idő |
| datetimeoffset |DateTimeOffset |
| Decimális |Decimális |
| A FILESTREAM attribútum (varbinary(max)) |Byte] |
| Lebegőpontos |Dupla |
| Kép |Byte] |
| int |Int32 |
| pénz |Decimális |
| nchar |Karakterlánc, Char] |
| ntext |Karakterlánc, Char] |
| Numerikus |Decimális |
| nvarchar |Karakterlánc, Char] |
| valós |Egyetlen |
| ROWVERSION |Byte] |
| smalldatetime |Dátum és idő |
| smallint |Int16 |
| kis pénz típusú értéknél |Decimális |
| sql_variant |Objektum * |
| Szöveg |Karakterlánc, Char] |
| time |A TimeSpan |
| időbélyeg |Byte] |
| tinyint |Bájt |
| egyedi azonosító |GUID |
| varbinary |Byte] |
| varchar |Karakterlánc, Char] |
| xml |XML |

## <a name="mapping-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Ismételhető másolása
Adatok tooSQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint. egy UPSERT tooperform helyett, lásd: [ismételhető írási tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk. 

Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
