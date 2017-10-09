---
title: ".NET API-módosítási napló gyár - aaaData |} Microsoft Docs"
description: "Ismerteti, akár jelentős változásokat, szolgáltatás kiegészítéseit, hibajavításokat tartalmaz stb... egy adott verziójában .NET API-t az Azure Data Factory hello."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: spelluru
ms.openlocfilehash: 1d44b45c3dc8f9d483d1f1cef7068edacc610932
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---net-api-change-log"></a>Az Azure Data Factory - .NET API Változásnapló
Ez a cikk ismerteti a módosítások tooAzure Data Factory SDK egy adott verziójában. Az Azure Data Factory hello legújabb NuGet-csomag található [Itt](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>4.11.0 verzió
A szolgáltatás kiegészítéseket:

* hello következő kapcsolódószolgáltatás-típusok bővült:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* a következő adatkészlet típusok hello bővült:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* hello következő átmásolhatja a forrást típusok lettek hozzáadva:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>4.10.0 verzió
* a következő választható tulajdonságok hello tooTextFormat lettek hozzáadva:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* hello következő kapcsolódószolgáltatás-típusok bővült:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* a következő adatkészlet típusok hello bővült:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* hello következő átmásolhatja a forrást típusok lettek hozzáadva:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Adja hozzá [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) tulajdonság tooAzureMLBatchExecutionActivity
  * A webszolgáltatás bemenetei több tooan Azure Machine Learning kísérlet sikeres engedélyezése

## <a name="version-491"></a>Verzió 4.9.1.
### <a name="bug-fix"></a>Hibajavítás
* WebApi-alapú hitelesítés érvényteleníthető [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>4.9.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* Adja hozzá [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) és [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) tulajdonságok tooCopyActivity. Lásd: [másolási előkészített](data-factory-copy-activity-performance.md#staged-copy) hello szolgáltatás leírását.

### <a name="bug-fix"></a>Hibajavítás
* Egy túlterhelése bevezetni [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metódus, amely fogad egy [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) példány.
* Megjelölés [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) és [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) CopySink, nem kötelező.

## <a name="version-480"></a>4.8.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* a következő választható tulajdonságok hello tooCopy tevékenység lettek hozzáadva írja be a másolási teljesítményének hangolása tooenable:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>4.7.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* Hozzáadott új StorageFormat típust [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) toocopy fájlok adja meg a optimalizált sor oszlopos (ORC) formátumban.
* Adja hozzá [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) és a kapcsolódó PolyBaseSettings tulajdonságok tooSqlDWSink.
  * Az SQL Data Warehouse PolyBase toocopy adatok hello használata lehetővé teszi, hogy.

## <a name="version-461"></a>Verzió 4.6.1.
### <a name="bug-fixes"></a>Hibajavítások
* Kijavítja a felsoroló tevékenység windows HTTP-kérelem.
  * Hello erőforráscsoport-név és hello adat-előállító eltávolítása hello-kérések forgalma.

## <a name="version-460"></a>4.6.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* hello következő tulajdonságokkal is bővült túl[PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Adatkészletek](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* hello következő tulajdonságokkal is bővült túl[PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Új hozzáadott [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) típus [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) írja be a toodefine adatkészleteket, amelyek esetén az JSON formátumban van.

## <a name="version-450"></a>4.5.0 verziója
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* Hozzáadott [tevékenység ablakban műveleteinek listázása](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * A hozzáadott metódusok tooretrieve tevékenység windows hello entitástípusok (Ez azt jelenti, hogy adat-előállítók, adathalmazokat, adatcsatornákat és tevékenységek) alapján szűri.
* hello következő kapcsolódószolgáltatás-típusok bővült:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* a következő adatkészlet típusok hello bővült:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* hello következő átmásolhatja a forrást típusok lettek hozzáadva:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>4.4.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* hello következő kapcsolódószolgáltatás-típus hozzá lett adva adatforrásaként és a másolási tevékenység fogadók esetében:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Lásd: [Azure Storage SAS társított szolgáltatás](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) kapcsolatos információkat és példákat.

## <a name="version-430"></a>4.3.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* a kapcsolódószolgáltatás-típusok régen következő hello lett fel, mert a másolási tevékenység adatforrások:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Lásd: [tárolt adatok mozgatása használatával a Data Factory HDFS](data-factory-hdfs-connector.md) kapcsolatos információkat és példákat.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Lásd: [helyezze át az adatok Azure Data Factory használatával az ODBC-adattároló](data-factory-odbc-connector.md) kapcsolatos információkat és példákat.

## <a name="version-420"></a>4.2.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* a következő új tevékenységtípus hello hozzá lett adva: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). További hello tevékenységgel kapcsolatos információkért lásd: [frissítése Azure ML modellek segítségével hello Update-Erőforrástevékenység](data-factory-azure-ml-batch-execution-activity.md).
* Egy új kötelező tulajdonság [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) toohello bővült [AzureMLLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) és [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) tulajdonságokkal is bővült toohello [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) osztály.
* Az ügyfél hívások toohello Data Factory szolgáltatásnak hello időtúllépések konfigurálását teszik lehetővé.

## <a name="version-410"></a>4.1.0 verzió
### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* hello következő kapcsolódószolgáltatás-típusok bővült:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* a következő típusú tevékenységek hello bővült:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* a következő adatkészlet típusok hello bővült:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* hello következő forrás és a fogadó típusa másolási tevékenységhez bővült:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>4.0.1 verziója
### <a name="breaking-changes"></a>Módosítások megszakítása
a következő osztályok hello átnevezték. hello új nevek hello eredeti nevek osztályok előtti 4.0.0 kiadási.

| Neve a 4.0.0 | Neve a 4.0.1 |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>4.0.0-s verzió
### <a name="breaking-changes"></a>Módosítások megszakítása
* a következő osztályok/felületek hello átnevezték.

| Régi név | Új neve |
|:--- |:--- |
| ITableOperations |[IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Tábla |[Adatkészlet](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| TableProperties |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TableTypeProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters |[DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| TableCreateOrUpdateResponse |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| TableListResponse |[DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* Hello **lista** módszerek most lapozható eredményeket adjon. Ha hello válasz tartalmaz egy nem üres **NextLink** , hello ügyfélalkalmazás kell tulajdonságot toocontinue lekérdezésekor hello következő oldal mindaddig, amíg az összes ad vissza.  Például:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Lista** csővezeték API beolvasása csak hello összefoglaló helyett részletes folyamat. Például csővezeték összefoglaló tevékenységet csak tartalmazhat nevét és típusát.

### <a name="feature-additions"></a>A szolgáltatás elemek felvétele
* Hello [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) az osztály támogatja a két új tulajdonságok, **SliceIdentifierColumnName** és **SqlWriterCleanupScript**, toosupport idempotent másolási tooAzure SQL-adatok Adatraktár. Lásd: hello [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) szóló cikkben olvashat ezeket a tulajdonságokat.
* Most már támogatott a tárolt eljárás futtatott Azure SQL Database és az Azure SQL Data Warehouse források hello másolási tevékenység részeként. Hello [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) és [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) osztály rendelkezik hello következő tulajdonságai: **SqlReaderStoredProcedureName** és **StoredProcedureParameters** . Lásd: hello [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) és [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) cikkek az Azure.com-on, ezek a Tulajdonságok vonatkozó további információért.  
