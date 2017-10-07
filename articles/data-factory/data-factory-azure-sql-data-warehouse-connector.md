---
title: az Azure SQL Data Warehouse aaaCopy adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatokat az Azure SQL Data Warehouse Azure Data Factory használatával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 75bfcf3c99844fc1297ca500107da23cf875e41f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Adatok tooand másolása az Azure SQL Data Warehouse Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure SQL Data Warehouse és a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.  

> [!TIP]
> tooachieve a legjobb teljesítményt, az Azure SQL Data Warehouse PolyBase tooload adatok felhasználásával. Hello [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakasz részleteket tartalmaz. A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **az Azure SQL Data Warehouse** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooAzure SQL Data Warehouse**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> Ha az adatok másolása az SQL Server vagy az Azure SQL Database tooAzure SQL Data Warehouse, ha hello tábla nem létezik a hello céltár, adat-előállító automatikusan segítségével hozhat létre hello tábla az SQL Data Warehouse hello séma hello tábla hello forrás adattároló. Lásd: [tábla létrehozásához automatikus](#auto-table-creation) részleteiről.

## <a name="supported-authentication-type"></a>Támogatott hitelesítési típushoz
Az Azure SQL Data Warehouse összekötő alapszintű hitelesítés támogatása.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat az Azure SQL Data Warehouse és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate egy folyamatot, amely másolja az adatokat az Azure SQL Data Warehouse és a rendszer toouse hello másolása varázsló. Lásd: [oktatóanyag: adatok betöltése az SQL Data Warehouse Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az Azure blob storage tooan Azure SQL adatraktárban, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL data warehouse tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure SQL Data Warehouse, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 
3. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát. És egy másik dataset toospecify hello tábla hello Azure SQL data warehouse hello blob-tároló átmásolva hello adatokat tartalmazó hozzon létre. Adatkészlet tulajdonságai, amelyek adott tooAzure SQL Data Warehouse, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
4. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában BlobSource forrás-és SqlDWSink akár használhatja a fogadó hello másolási tevékenységhez. Hasonlóképpen a Blob Storage Azure SQL Data Warehouse tooAzure másolása, használható SqlDWSource és BlobSink hello másolási tevékenység. A másolási tevékenység tulajdonságait, amelyek adott tooAzure SQL Data Warehouse, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az Azure SQL Data Warehouse használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure SQL Data Warehouse részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON elemek adott tooAzure SQL Data Warehouse kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **AzureSqlDW** |Igen |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz. Csak az alapszintű hitelesítést is támogatja. |Igen |

> [!IMPORTANT]
> Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) és adatbázis-kiszolgáló túl hello[engedélyezése az Azure-szolgáltatások tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Ezenkívül az adatok tooAzure SQL Data Warehouse másolása a külső Azure többek között a helyszíni adatforrásokból a data factory átjáróval, az IP-címtartományt, amely az SQL Data Warehouse adatok tooAzure küld hello gép konfigurálni.

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** típusú hello adatkészlet szakasz **AzureSqlDWTable** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello Azure SQL Data Warehouse-adatbázis, amely a társított szolgáltatás hello nevére hivatkozik. |Igen |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

> [!NOTE]
> hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.

Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

### <a name="sqldwsource"></a>SqlDWSource
Ha a forrás típusa van **SqlDWSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

Ha hello **sqlReaderQuery** megadott hello SqlDWSource, hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Data Warehouse forrásadatok tooget hello.

Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok használt toobuild egy lekérdezés toorun elleni hello Azure SQL Data Warehouse. Példa: `select column1, column2 from mytable`. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

#### <a name="sqldwsource-example"></a>SqlDWSource – példa

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**hello tárolt eljárás definíciója:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. További információkért lásd: [ismételhetőség szakasz](#repeatability-during-copy). |A lekérdezési utasítást. |Nem |
| allowPolyBase |Azt jelzi, hogy (ha alkalmazható) PolyBase toouse BULKINSERT mechanizmus helyett. <br/><br/> **A PolyBase használata javasolt módja tooload adatokat az SQL Data Warehouse hello.** Lásd: [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakaszban a korlátozások és részleteit. |True (Igaz) <br/>Hamis (alapértelmezés) |Nem |
| kapcsolódó polyBaseSettings |Egy csoport lehet megadni, ha hello tulajdonságok **allowPolybase** tulajdonsága túl**igaz**. |&nbsp; |Nem |
| rejectValue |Megadja a hello számát vagy a sorok, amelyekre el lehet utasítani, mielőtt hello lekérdezés nem sikerült százalékát. <br/><br/>Bővebben a hello PolyBase elutasítása hello lehetőségeit további **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör. |0 (alapértelmezés), 1, 2... |Nem |
| rejectType |Meghatározza, hogy hello rejectValue beállítás konstans értéket vagy százalékában van megadva. |Érték (alapértelmezett), százalékos aránya |Nem |
| rejectSampleValue |Határozza meg, hogy hello sorok tooretrieve előtt hello PolyBase újraszámítja a visszautasított sorok hello százalékát. |1, 2, … |Igen, ha **rejectType** van **százalékos aránya** |
| useTypeDefault |Itt adhatja meg, hogyan értékek a hiányzó toohandle tagolt-e szövegfájlok amikor PolyBase hello szövegfájlból kér le adatokat.<br/><br/>Ezt a tulajdonságot hello argumentumok című szakaszában olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |IGAZ, hamis (alapértelmezés) |Nem |
| WriteBatchSize |Adatok beszúrása hello SQL táblázatba, amikor hello puffer mérete eléri writeBatchSize |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |

#### <a name="sqldwsink-example"></a>SqlDWSink – példa

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-tooload-data-into-azure-sql-data-warehouse"></a>Az Azure SQL Data Warehouse PolyBase tooload adatok használata
Használatával  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  egy hatékony módszer a nagy mennyiségű adatok betöltését az Azure SQL Data Warehouse nagy átviteli sebességgel. A nagy nyereség hello átviteli sebességének hello alapértelmezett BULKINSERT mechanizmus helyett a PolyBase használatával tekintheti meg. Lásd: [teljesítmény hivatkozási szám másolása](data-factory-copy-activity-performance.md#performance-reference) a részletes összehasonlítását. A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).

* Ha a forrás adatok **Azure Blob vagy az Azure Data Lake Store**, és hello formátum kompatibilis a PolyBase, tooAzure SQL Data Warehouse PolyBase segítségével közvetlenül másolhatja. Lásd:  **[közvetlen másolása a PolyBase használatával](#direct-copy-using-polybase)**  adatokkal.
* Ha a forrás-tárolót és formátum eredetileg nem támogatott a PolyBase által, használhatja a hello  **[előkészített másolása a PolyBase használatával](#staged-copy-using-polybase)**  inkább a beállítást. Is biztosít, nagyobb átviteli sebesség automatikusan hello adatok PolyBase-kompatibilis formátumra konvertálása és hello adattárolás az Azure Blob Storage tárolóban. Majd betölti az SQL Data Warehouse-adatok.

Set hello `allowPolyBase` tulajdonság túl**igaz** a következő példa az Azure Data Factory toouse PolyBase toocopy adatokat az Azure SQL Data Warehouse hello látható módon. Ha úgy állítja be a allowPolyBase tootrue, PolyBase tulajdonságokat hello segítségével megadhatja `polyBaseSettings` tulajdonságcsoport. Lásd: hello [SqlDWSink](#SqlDWSink) szakasz kapcsolódó polyBaseSettings használható tulajdonságokról vonatkozó további információért.

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a>A PolyBase használatával közvetlen másolása
SQL Data Warehouse PolyBase közvetlenül támogatja Azure-Blob és az Azure Data Lake Store (egyszerű szolgáltatásnév) forrásként, és az adott fájl formátum követelményeinek. Ha a forrásadatok megfelel a jelen szakaszban ismertetett hello feltételeknek, közvetlenül átmásolhatja forrás adatokat tároló tooAzure SQL Data Warehouse PolyBase használatával. Ellenkező esetben használhatja [előkészített másolása a PolyBase használatával](#staged-copy-using-polybase).

> [!TIP]
> Data Lake Store tooSQL adatraktár toocopy adatait hatékonyan, további információhoz [Azure Data Factory teszi egyszerűbbé és kényelmes toouncover információkat kaphat a adatok akkor is igaz, Data Lake Store használata az SQL Data Warehouse szolgáltatással](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Hello feltételeknek nem felel meg, ha az Azure Data Factory hello beállítások ellenőrzi, és automatikusan visszaáll toohello BULKINSERT mechanizmus hello adatátvitelt jelölik.

1. **Forrás társított szolgáltatás** típusa: **AzureStorage** vagy **szolgáltatás egyszerű hitelesítéssel AzureDataLakeStore**.  
2. Hello **bemeneti adatkészlet** típusa: **AzureBlob** vagy **AzureDataLakeStore**, és a formátum típusa hello `type` tulajdonságai **OrcFormat** , vagy **szöveges** a következő konfigurációk hello:

   1. `rowDelimiter`kell  **\n** .
   2. `nullValue`értéke túl**üres karakterlánc** (""), vagy `treatEmptyAsNull` értéke túl**igaz**.
   3. `encodingName`értéke túl**utf-8**, amely **alapértelmezett** érték.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, és `skipLineCount` nincs megadva.
   5. `compression`lehet **tömörítés**, **GZip**, vagy **Deflate**.

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. Nincs nincs `skipHeaderLineCount` beállítás alatt **BlobSource** vagy **AzureDataLakeStore** hello másolási tevékenység hello-feldolgozási folyamat számára.
4. Nincs nincs `sliceIdentifierColumnName` beállítás alatt **SqlDWSink** hello másolási tevékenység hello-feldolgozási folyamat számára. (A PolyBase garantálja, hogy minden adat frissül, vagy nem frissül, az egyszeri futtatás. tooachieve **ismételhetőség**, használhat `sqlWriterCleanupScript`).
5. Nincs nincs `columnMapping` a másolási tevékenység társított hello használja.

### <a name="staged-copy-using-polybase"></a>A PolyBase használatával előkészített másolása
A forrásadatok hello előző szakaszban bemutatott hello feltételeknek nem felel meg, amikor az adatok másolását az átmeneti átmeneti Azure Blob Storage (nem lehet a prémium szintű Storage) keresztül is engedélyezheti. Ebben az esetben Azure Data Factory automatikusan átalakítások végez hello toomeet adatok formátuma vonatkozó követelmények a PolyBase, majd használja a PolyBase tooload adatokat az SQL Data Warehouse, és a legutóbbi karbantartás hello blobtárolóból ideiglenes adatait. Lásd: [előkészített másolási](data-factory-copy-activity-performance.md#staged-copy) átmeneti Azure Blob keresztül az adatok másolásának működéséről, általában a részletekért.

> [!NOTE]
> Ha az Azure SQL Data Warehouse PolyBase használatával tárolja az adatok másolását egy helyszíni adatokból, és átmeneti, ha az adatkezelési átjáró verziója nem éri el 2.4, JRE (Java Runtime Environment) megadása szükséges. az átjáró számítógépre, amely használt tootransform a forrásadatok van megfelelő formátumba. Javasoljuk, hogy az átjáró toohello legújabb tooavoid ilyen függőségi frissíti.
>

toouse ezt a beállítást, hozzon létre egy [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) toohello Azure Storage-fiókot, amely rendelkezik hello ideiglenes blob-tároló hivatkozik, amely, majd adja meg a hello `enableStaging` és `stagingSettings` hello másolási tevékenység tulajdonságai ahogy az a következő kód hello:

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server tooSQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>A PolyBase használata esetén ajánlott eljárások
hello következő szakaszokban további ajánlott eljárások toohello azokat, a említett [ajánlott eljárások az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Adatbázis szükséges engedéllyel
toouse PolyBase, igényel hello felhasználó éppen használt tooload adatokat az SQL Data Warehouse rendelkezik hello ["Vezérlő" engedély](https://msdn.microsoft.com/library/ms191291.aspx) hello cél adatbázison. Egyirányú tooachieve, amely tooadd "db_owner" szerepkör tagjaként felhasználó. Megtudhatja, hogyan toodo, amelyek a következő [ebben a szakaszban](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Írja be korlátozás sorméret és adatok
A Polybase terhelések korlátozódnak tooloading sort is kisebb, mint **1 MB** és nem tölthető be a tooVARCHR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX). Tekintse meg a túl[Itt](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Ha 1 MB-nál nagyobb méretű sorokat tartalmazó forrásadatok, előfordulhat, hogy a kívánt toosplit hello forrástáblákból függőleges több kis állók közül. Ha hello legnagyobb sorainak méretéhez azok nem haladja meg a hello korlátot. hello kisebb táblák majd tölthetők be a PolyBase használatával, és egyesíti az Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>Az SQL Data Warehouse erőforrás osztály
tooachieve legjobb lehetséges átviteli, fontolja meg a tooassign nagyobb erőforrás osztály toohello felhasználó éppen használt tooload adatokat az SQL Data Warehouse polybase. Megtudhatja, hogyan toodo, amelyek a következő [módosíthatja a felhasználói erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

### <a name="tablename-in-azure-sql-data-warehouse"></a>az Azure SQL Data Warehouse táblanév
hello alábbi táblázat példákat hogyan toospecify hello **tableName** tulajdonságot az adatkészlet JSON-séma-és táblanevet különböző kombinációjához.

| Megadott adatbázissémát | Tábla neve | tableName JSON tulajdonsága |
| --- | --- | --- |
| dbo |Táblanév |MyTable vagy dbo. MyTable vagy [dbo]. [MyTable] |
| dbo1 |Táblanév |dbo1. MyTable vagy [dbo1]. [MyTable] |
| dbo |My.Table |[My.Table] vagy [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Ha megjelenik a következő hiba hello, problémát hello tableName tulajdonsága megadott hello érték lehet. Hello megfelelő módon toospecify hello tableName JSON tulajdonság értékei a hello táblázatban találja.  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Az alapértelmezett értékekkel oszlopok
Jelenleg a PolyBase szolgáltatás adat-előállítóban csak fogad hello azonos számú oszlopot hello céltábla hasonlóan. Tegyük fel például, négy oszlopokkal rendelkező táblát rendelkezik, és az egyik legyen alapértelmezett értékkel van definiálva. hello bemeneti adatok továbbra is tartalmaznia kell a négy oszlopot. A 3-oszlop a bemeneti adatkészletet biztosító eredményezné egy hasonló toohello hiba, a következő üzenetet:

```
All columns of hello table must be specified in hello INSERT BULK statement.
```
NULL érték az alapértelmezett érték egy speciális formája, amely. Ha hello oszlop nullázható, hello bemeneti adatait (blob) az adott oszlop lehet üres (nem hiányzik hello bemeneti adatkészlet). A PolyBase számukra NULL hello Azure SQL Data Warehouse szúrja be.  

## <a name="auto-table-creation"></a>Automatikus tábla létrehozásához
Ha az SQL Server adatainak másolása varázsló toocopy használ, vagy az Azure SQL Database tooAzure SQL Data Warehouse és hello tartozó tábla toohello forrástábla nem létezik a hello céltár, adat-előállító automatikusan létrehozhat hello tábla hello az adatraktár hello forrás táblaséma használatával.

Adat-előállító hello céltár hello táblát hoz létre a hello ugyanaz a tábla neve hello forrás adattár. hello adattípusok oszlopok a következő típusleképezéshez hello alapján választják ki. Szükség esetén elvégez típus átalakítások toofix között a forrás és cél tárolja az azonosított inkompatibilitásokat. Ciklikus multiplexelés elosztása is használja.

| Forrás SQL-adatbázis oszlop típusa | Cél SQL DW oszlop típusa (méretének korlátozása) |
| --- | --- |
| int | int |
| BigInt | BigInt |
| SmallInt | SmallInt |
| TinyInt | TinyInt |
| bit | bit |
| Decimális | Decimális |
| Numerikus | Decimális |
| Lebegőpontos | Lebegőpontos |
| pénz | pénz |
| Real | Real |
| Kis pénz típusú értéknél | Kis pénz típusú értéknél |
| Bináris | Bináris |
| varbinary | Varbinary (felfelé too8000) |
| Dátum | Dátum |
| Dátum és idő | Dátum és idő |
| DateTime2 | DateTime2 |
| Time | Time |
| DateTimeOffset | DateTimeOffset |
| SmallDateTime | SmallDateTime |
| Szöveg | Varchar (felfelé too8000) |
| NText | NVarChar (felfelé too4000) |
| Kép | VarBinary (felfelé too8000) |
| Egyedi azonosító | Egyedi azonosító |
| Karakter | Karakter |
| NChar | NChar |
| VarChar | VarChar (felfelé too8000) |
| NVarChar | NVarChar (felfelé too4000) |
| XML | Varchar (felfelé too8000) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a>Írja be az Azure SQL Data Warehouse leképezése
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Ha túl & Azure SQL Data Warehouse az adatok áthelyezése, hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz.

hello leképezési legyen, mint hello [SQL Server adattípus-hozzárendelése az ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).

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

Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti. További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-tooand-from-sql-data-warehouse"></a>Az adatok tooand másolását az SQL Data Warehouse JSON példák
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand az Azure SQL Data Warehouse és az Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

### <a name="example-copy-data-from-azure-sql-data-warehouse-tooazure-blob"></a>Példa: Adatok másolása az Azure SQL Data Warehouse tooAzure Blob
hello minta meghatározza, hogy a következő adat-előállító entitások hello:

1. A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlDWSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta idősorozat (óránkénti, napi stb) adatainak másolása az Azure SQL Data Warehouse adatbázis tooa blob egy táblázatban minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**A társított szolgáltatásnak Azure SQL Data Warehouse:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
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
**Az SQL Data Warehouse bemeneti adatkészletet:**

hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" az Azure SQL Data Warehouse és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
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

**Másolási tevékenység során a folyamat SqlDWSource és BlobSink:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlDWSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> [!NOTE]
> Hello példában **sqlReaderQuery** hello SqlDWSource van megadva. hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Data Warehouse forrásadatok tooget hello.
>
> Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).
>
> Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok-e a használt toobuild (válassza Oszlop1, column2 from tábla) lekérdezés toorun hello Azure SQL Data Warehouse ellen. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.
>
>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-data-warehouse"></a>Példa: Adatok másolása az Azure Blob tooAzure SQL Data Warehouse
hello minta meghatározza, hogy a következő adat-előállító entitások hello:

1. A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlDWSink](#copy-activity-properties).

hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL Data Warehouse-adatbázis az Azure blob tooa táblából óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**A társított szolgáltatásnak Azure SQL Data Warehouse:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
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
**Az Azure Blob bemeneti adatkészletet:**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.

```JSON
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
**Az SQL Data Warehouse kimeneti adatkészlet:**

hello minta másolja át az Azure SQL Data Warehouse "MyTable" nevű tooa adattábla. Hello tábla létrehozása az Azure SQL Data Warehouse azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
**Másolási tevékenység során a folyamat BlobSource és SqlDWSink:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlDWSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
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
Útmutatást lásd: lásd: hello [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md) és [adatok betöltése az Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) hello Azure SQL Data Warehouse cikk dokumentációját.

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
