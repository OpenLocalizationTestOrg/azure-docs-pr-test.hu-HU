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
# <a name="datasets-in-azure-data-factory"></a><span data-ttu-id="ad78f-104">Az Azure Data Factory adathalmazok</span><span class="sxs-lookup"><span data-stu-id="ad78f-104">Datasets in Azure Data Factory</span></span>
<span data-ttu-id="ad78f-105">Ez a cikk ismerteti, milyen adatkészletek, hogyan vannak definiálva JSON formátumú, és hogy ezek hogyan használhatók az Azure Data Factory folyamatok.</span><span class="sxs-lookup"><span data-stu-id="ad78f-105">This article describes what datasets are, how they are defined in JSON format, and how they are used in Azure Data Factory pipelines.</span></span> <span data-ttu-id="ad78f-106">Az adatkészlet JSON-definícióban hello biztosít az egyes szakaszokon (például struktúra, rendelkezésre állási és házirend) adatait.</span><span class="sxs-lookup"><span data-stu-id="ad78f-106">It provides details about each section (for example, structure, availability, and policy) in hello dataset JSON definition.</span></span> <span data-ttu-id="ad78f-107">hello cikket is példákat hello segítségével **eltolás**, **anchorDateTime**, és **stílus** tulajdonságok az adatkészlet JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-107">hello article also provides examples for using hello **offset**, **anchorDateTime**, and **style** properties in a dataset JSON definition.</span></span>

> [!NOTE]
> <span data-ttu-id="ad78f-108">Ha új tooData gyári, lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md) áttekintése.</span><span class="sxs-lookup"><span data-stu-id="ad78f-108">If you are new tooData Factory, see [Introduction tooAzure Data Factory](data-factory-introduction.md) for an overview.</span></span> <span data-ttu-id="ad78f-109">Ha nem rendelkezik adat-előállítók létrehozása a gyakorlati tapasztalatok, akkor is értelmezésében által olvasása hello [data transformation oktatóanyag](data-factory-build-your-first-pipeline.md) és hello [adatok mozgása oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-109">If you do not have hands-on experience with creating data factories, you can gain a better understanding by reading hello [data transformation tutorial](data-factory-build-your-first-pipeline.md) and hello [data movement tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="ad78f-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ad78f-110">Overview</span></span>
<span data-ttu-id="ad78f-111">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="ad78f-111">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="ad78f-112">A **csővezeték** logikai csoportosítása **tevékenységek** , amelyek együtt a feladat elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="ad78f-112">A **pipeline** is a logical grouping of **activities** that together perform a task.</span></span> <span data-ttu-id="ad78f-113">Egy adatcsatorna tevékenységeinek hello műveletek tooperform határozza meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ad78f-113">hello activities in a pipeline define actions tooperform on your data.</span></span> <span data-ttu-id="ad78f-114">Például előfordulhat, hogy használja a másolási tevékenység toocopy adatokat egy helyi SQL Server tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="ad78f-114">For example, you might use a copy activity toocopy data from an on-premises SQL Server tooAzure Blob storage.</span></span> <span data-ttu-id="ad78f-115">Ezután használhatja a egy Hive tevékenységet, amely egy parancsprogramot futtathat Hive egy Azure HDInsight fürt tooprocess adatokon a Blob storage tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="ad78f-115">Then, you might use a Hive activity that runs a Hive script on an Azure HDInsight cluster tooprocess data from Blob storage tooproduce output data.</span></span> <span data-ttu-id="ad78f-116">Végül használhat egy második másolási tevékenység toocopy hello kimeneti adatok tooAzure SQL Data Warehouse, mely üzleti intelligenciával reporting megoldások beépített felett.</span><span class="sxs-lookup"><span data-stu-id="ad78f-116">Finally, you might use a second copy activity toocopy hello output data tooAzure SQL Data Warehouse, on top of which business intelligence (BI) reporting solutions are built.</span></span> <span data-ttu-id="ad78f-117">Adatcsatornák és tevékenységgel kapcsolatos további információkért lásd: [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-117">For more information about pipelines and activities, see [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md).</span></span>

<span data-ttu-id="ad78f-118">Egy tevékenység is igénybe vehet a nulla vagy több bemeneti **adatkészletek**, majd előállítanak egy vagy több kimeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="ad78f-118">An activity can take zero or more input **datasets**, and produce one or more output datasets.</span></span> <span data-ttu-id="ad78f-119">Egy bemeneti adatkészlet hello bemeneti hello feldolgozási soros tevékenység, és egy kimeneti adatkészlet hello kimeneti hello tevékenység jelenti.</span><span class="sxs-lookup"><span data-stu-id="ad78f-119">An input dataset represents hello input for an activity in hello pipeline, and an output dataset represents hello output for hello activity.</span></span> <span data-ttu-id="ad78f-120">Az adatkészletek adatokat határoznak meg a különböző adattárakban, például táblákban, fájlokban, mappákban és dokumentumokban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-120">Datasets identify data within different data stores, such as tables, files, folders, and documents.</span></span> <span data-ttu-id="ad78f-121">Például egy Azure Blob-adathalmazra megadja hello blob tároló és mappa mely hello a feldolgozási sor olvassa el a hello adatok Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-121">For example, an Azure Blob dataset specifies hello blob container and folder in Blob storage from which hello pipeline should read hello data.</span></span> 

<span data-ttu-id="ad78f-122">A DataSet adatkészlet létrehozása előtt hozzon létre egy **társított szolgáltatás** toolink az adatok tárolásához toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-122">Before you create a dataset, create a **linked service** toolink your data store toohello data factory.</span></span> <span data-ttu-id="ad78f-123">Társított szolgáltatások sokkal hasonlóak a kapcsolati karakterláncok, amelyek a Data Factory tooconnect tooexternal erőforrások szükséges hello kapcsolati adatainak megadása.</span><span class="sxs-lookup"><span data-stu-id="ad78f-123">Linked services are much like connection strings, which define hello connection information needed for Data Factory tooconnect tooexternal resources.</span></span> <span data-ttu-id="ad78f-124">Adatkészletek azonosíthatja az adatokat belül hello kapcsolódó adatokat tárolja, például az SQL-táblák, fájlok, mappák és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="ad78f-124">Datasets identify data within hello linked data stores, such as SQL tables, files, folders, and documents.</span></span> <span data-ttu-id="ad78f-125">Például egy Azure Storage társított szolgáltatás hivatkozások a tárolási fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-125">For example, an Azure Storage linked service links a storage account toohello data factory.</span></span> <span data-ttu-id="ad78f-126">Egy Azure Blob-adathalmazra hello blobtárolót és hello bemeneti BLOB toobe feldolgozott tartalmazó hello mappát jelöli.</span><span class="sxs-lookup"><span data-stu-id="ad78f-126">An Azure Blob dataset represents hello blob container and hello folder that contains hello input blobs toobe processed.</span></span> 

<span data-ttu-id="ad78f-127">Íme egy példa.</span><span class="sxs-lookup"><span data-stu-id="ad78f-127">Here is a sample scenario.</span></span> <span data-ttu-id="ad78f-128">Blob storage tooa SQL-adatbázis adatait toocopy, két társított szolgáltatások létrehozásához: Azure Storage és az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ad78f-128">toocopy data from Blob storage tooa SQL database, you create two linked services: Azure Storage and Azure SQL Database.</span></span> <span data-ttu-id="ad78f-129">Ezután hozzon létre két adatkészletet: Azure Blob-adathalmazra (amely a toohello Azure tárolás társított szolgáltatásának) és (amely a toohello csatolt Azure SQL Database szolgáltatás) Azure SQL-tábla a dataset.</span><span class="sxs-lookup"><span data-stu-id="ad78f-129">Then, create two datasets: Azure Blob dataset (which refers toohello Azure Storage linked service) and Azure SQL Table dataset (which refers toohello Azure SQL Database linked service).</span></span> <span data-ttu-id="ad78f-130">hello Azure Storage és az Azure SQL Database összekapcsolt szolgáltatások használó Data Factory futásidejű tooconnect tooyour Azure Storage és az Azure SQL Database, illetve kapcsolati karakterláncokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ad78f-130">hello Azure Storage and Azure SQL Database linked services contain connection strings that Data Factory uses at runtime tooconnect tooyour Azure Storage and Azure SQL Database, respectively.</span></span> <span data-ttu-id="ad78f-131">hello Azure Blob-adathalmazra hello blobtárolót és hello bemeneti BLOB a Blob Storage tárolóban tartalmazó blob mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="ad78f-131">hello Azure Blob dataset specifies hello blob container and blob folder that contains hello input blobs in your Blob storage.</span></span> <span data-ttu-id="ad78f-132">hello Azure SQL-tábla a dataset határozza meg, az SQL adatbázis toowhich hello adatokon hello SQL táblázat toobe másolja.</span><span class="sxs-lookup"><span data-stu-id="ad78f-132">hello Azure SQL Table dataset specifies hello SQL table in your SQL database toowhich hello data is toobe copied.</span></span>

<span data-ttu-id="ad78f-133">hello következő diagramon láthatók hello folyamat, a tevékenység, a dataset és a társított szolgáltatás közötti kapcsolatok szerepelnek az adat-előállító:</span><span class="sxs-lookup"><span data-stu-id="ad78f-133">hello following diagram shows hello relationships among pipeline, activity, dataset, and linked service in Data Factory:</span></span> 

![Adatcsatorna, adatkészlet, tevékenység társított szolgáltatások közötti kapcsolat](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a><span data-ttu-id="ad78f-135">Adatkészlet JSON</span><span class="sxs-lookup"><span data-stu-id="ad78f-135">Dataset JSON</span></span>
<span data-ttu-id="ad78f-136">A Data Factory dataset az alábbiak szerint definiáltuk JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="ad78f-136">A dataset in Data Factory is defined in JSON format as follows:</span></span>

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

<span data-ttu-id="ad78f-137">a következő táblázat hello hello fent JSON-tulajdonságokat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="ad78f-137">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="ad78f-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ad78f-138">Property</span></span> | <span data-ttu-id="ad78f-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad78f-139">Description</span></span> | <span data-ttu-id="ad78f-140">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ad78f-140">Required</span></span> | <span data-ttu-id="ad78f-141">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="ad78f-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ad78f-142">név</span><span class="sxs-lookup"><span data-stu-id="ad78f-142">name</span></span> |<span data-ttu-id="ad78f-143">Hello DataSet adatkészlet neve.</span><span class="sxs-lookup"><span data-stu-id="ad78f-143">Name of hello dataset.</span></span> <span data-ttu-id="ad78f-144">Lásd: [Azure Data Factory - elnevezési szabályait](data-factory-naming-rules.md) elnevezési szabályait.</span><span class="sxs-lookup"><span data-stu-id="ad78f-144">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="ad78f-145">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-145">Yes</span></span> |<span data-ttu-id="ad78f-146">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-146">NA</span></span> |
| <span data-ttu-id="ad78f-147">type</span><span class="sxs-lookup"><span data-stu-id="ad78f-147">type</span></span> |<span data-ttu-id="ad78f-148">Hello dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="ad78f-148">Type of hello dataset.</span></span> <span data-ttu-id="ad78f-149">Adjon meg egy adat-előállító által támogatott hello típusok (például: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="ad78f-149">Specify one of hello types supported by Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <br/><br/><span data-ttu-id="ad78f-150">További információkért lásd: [adathalmaztípushoz](#Type).</span><span class="sxs-lookup"><span data-stu-id="ad78f-150">For details, see [Dataset type](#Type).</span></span> |<span data-ttu-id="ad78f-151">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-151">Yes</span></span> |<span data-ttu-id="ad78f-152">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-152">NA</span></span> |
| <span data-ttu-id="ad78f-153">struktúra</span><span class="sxs-lookup"><span data-stu-id="ad78f-153">structure</span></span> |<span data-ttu-id="ad78f-154">Hello adatkészlet sémája.</span><span class="sxs-lookup"><span data-stu-id="ad78f-154">Schema of hello dataset.</span></span><br/><br/><span data-ttu-id="ad78f-155">További információkért lásd: [adatkészlet-szerkezetekben](#Structure).</span><span class="sxs-lookup"><span data-stu-id="ad78f-155">For details, see [Dataset structure](#Structure).</span></span> |<span data-ttu-id="ad78f-156">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-156">No</span></span> |<span data-ttu-id="ad78f-157">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-157">NA</span></span> |
| <span data-ttu-id="ad78f-158">typeProperties</span><span class="sxs-lookup"><span data-stu-id="ad78f-158">typeProperties</span></span> | <span data-ttu-id="ad78f-159">hello típustulajdonságokat eltérőek az egyes (például: Azure Blob, Azure SQL-tábla).</span><span class="sxs-lookup"><span data-stu-id="ad78f-159">hello type properties are different for each type (for example: Azure Blob, Azure SQL table).</span></span> <span data-ttu-id="ad78f-160">További részletek az azok tulajdonságait és a támogatott hello esetében: [adathalmaztípushoz](#Type).</span><span class="sxs-lookup"><span data-stu-id="ad78f-160">For details on hello supported types and their properties, see [Dataset type](#Type).</span></span> |<span data-ttu-id="ad78f-161">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-161">Yes</span></span> |<span data-ttu-id="ad78f-162">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-162">NA</span></span> |
| <span data-ttu-id="ad78f-163">external</span><span class="sxs-lookup"><span data-stu-id="ad78f-163">external</span></span> | <span data-ttu-id="ad78f-164">Logikai jelző toospecify, hogy a DataSet adatkészlet explicit módon hozzák a data factory-folyamat vagy nem.</span><span class="sxs-lookup"><span data-stu-id="ad78f-164">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> <span data-ttu-id="ad78f-165">Hello bemeneti adatkészlet a tevékenység nem hozzák hello aktuális folyamatot, ha a jelző tootrue beállítása</span><span class="sxs-lookup"><span data-stu-id="ad78f-165">If hello input dataset for an activity is not produced by hello current pipeline, set this flag tootrue.</span></span> <span data-ttu-id="ad78f-166">Állítsa be a jelző tootrue hello bemeneti adatkészlet hello első tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="ad78f-166">Set this flag tootrue for hello input dataset of hello first activity in hello pipeline.</span></span>  |<span data-ttu-id="ad78f-167">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-167">No</span></span> |<span data-ttu-id="ad78f-168">hamis</span><span class="sxs-lookup"><span data-stu-id="ad78f-168">false</span></span> |
| <span data-ttu-id="ad78f-169">rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="ad78f-169">availability</span></span> | <span data-ttu-id="ad78f-170">Hello feldolgozása ablak (például óránként vagy naponta) vagy a felosztás hello dataset üzemi modell hello határoz meg.</span><span class="sxs-lookup"><span data-stu-id="ad78f-170">Defines hello processing window (for example, hourly or daily) or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="ad78f-171">Felhasznált és egy tevékenység futott által előállított adatok tárolóegységekhez egy adatszelet nevezik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-171">Each unit of data consumed and produced by an activity run is called a data slice.</span></span> <span data-ttu-id="ad78f-172">Ha egy kimeneti adatkészlet rendelkezésre állását hello beállítása toodaily (gyakoriság -, időköz - 1 nap), a szelet naponta elő.</span><span class="sxs-lookup"><span data-stu-id="ad78f-172">If hello availability of an output dataset is set toodaily (frequency - Day, interval - 1), a slice is produced daily.</span></span> <br/><br/><span data-ttu-id="ad78f-173">További információkért lásd: [adatkészlet rendelkezésre állási](#Availability).</span><span class="sxs-lookup"><span data-stu-id="ad78f-173">For details, see [Dataset availability](#Availability).</span></span> <br/><br/><span data-ttu-id="ad78f-174">További tudnivalók hello dataset modell felosztás: hello [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ad78f-174">For details on hello dataset slicing model, see hello [Scheduling and execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="ad78f-175">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-175">Yes</span></span> |<span data-ttu-id="ad78f-176">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-176">NA</span></span> |
| <span data-ttu-id="ad78f-177">szabályzat</span><span class="sxs-lookup"><span data-stu-id="ad78f-177">policy</span></span> |<span data-ttu-id="ad78f-178">Hello feltételek vagy hello dataset szeletek kell néhány előfeltételnek hello feltételt határoz meg.</span><span class="sxs-lookup"><span data-stu-id="ad78f-178">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="ad78f-179">További információkért lásd: hello [Dataset házirend](#Policy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad78f-179">For details, see hello [Dataset policy](#Policy) section.</span></span> |<span data-ttu-id="ad78f-180">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-180">No</span></span> |<span data-ttu-id="ad78f-181">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-181">NA</span></span> |

## <a name="dataset-example"></a><span data-ttu-id="ad78f-182">A DataSet – példa</span><span class="sxs-lookup"><span data-stu-id="ad78f-182">Dataset example</span></span>
<span data-ttu-id="ad78f-183">A következő példa hello, hello dataset nevű táblázatot jelölő **MyTable** SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-183">In hello following example, hello dataset represents a table named **MyTable** in a SQL database.</span></span>

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

<span data-ttu-id="ad78f-184">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-184">Note hello following points:</span></span>

* <span data-ttu-id="ad78f-185">**típus** tooAzureSqlTable van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ad78f-185">**type** is set tooAzureSqlTable.</span></span>
* <span data-ttu-id="ad78f-186">**tableName** típusa (típus: adott tooAzureSqlTable) tulajdonsága tooMyTable.</span><span class="sxs-lookup"><span data-stu-id="ad78f-186">**tableName** type property (specific tooAzureSqlTable type) is set tooMyTable.</span></span>
* <span data-ttu-id="ad78f-187">**linkedServiceName** tooa társított szolgáltatás típusa AzureSqlDatabase, amelyet a következő kódrészletet hello a JSON hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-187">**linkedServiceName** refers tooa linked service of type AzureSqlDatabase, which is defined in hello next JSON snippet.</span></span> 
* <span data-ttu-id="ad78f-188">**rendelkezésre állási gyakoriság** értéke tooDay, és **időköz** too1 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ad78f-188">**availability frequency** is set tooDay, and **interval** is set too1.</span></span> <span data-ttu-id="ad78f-189">Ez azt jelenti, hogy hello dataset szelet naponta jön létre.</span><span class="sxs-lookup"><span data-stu-id="ad78f-189">This means that hello dataset slice is produced daily.</span></span>  

<span data-ttu-id="ad78f-190">**AzureSqlLinkedService** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="ad78f-190">**AzureSqlLinkedService** is defined as follows:</span></span>

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

<span data-ttu-id="ad78f-191">A JSON-részlet megelőző hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-191">In hello preceding JSON snippet:</span></span>

* <span data-ttu-id="ad78f-192">**típus** tooAzureSqlDatabase van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ad78f-192">**type** is set tooAzureSqlDatabase.</span></span>
* <span data-ttu-id="ad78f-193">**connectionString** típusú tulajdonság határozza meg az információkat tooconnect tooa SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ad78f-193">**connectionString** type property specifies information tooconnect tooa SQL database.</span></span>  

<span data-ttu-id="ad78f-194">Ahogy látja, hello társított szolgáltatás meghatározása hogyan tooconnect tooa SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ad78f-194">As you can see, hello linked service defines how tooconnect tooa SQL database.</span></span> <span data-ttu-id="ad78f-195">hello dataset határozza meg, milyen tábla bemenetként használja, ezért egy folyamat hello tevékenység kimeneti.</span><span class="sxs-lookup"><span data-stu-id="ad78f-195">hello dataset defines what table is used as an input and output for hello activity in a pipeline.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="ad78f-196">Kivéve, ha a DataSet adatkészlet hello folyamat alatt keletkezik, azt kell megjelölni **külső**.</span><span class="sxs-lookup"><span data-stu-id="ad78f-196">Unless a dataset is being produced by hello pipeline, it should be marked as **external**.</span></span> <span data-ttu-id="ad78f-197">Ez a beállítás általában az adatcsatorna első tevékenység tooinputs vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-197">This setting generally applies tooinputs of first activity in a pipeline.</span></span>   


## <span data-ttu-id="ad78f-198"><a name="Type"></a>A DataSet típusa</span><span class="sxs-lookup"><span data-stu-id="ad78f-198"><a name="Type"></a> Dataset type</span></span>
<span data-ttu-id="ad78f-199">hello hello adatkészlet függ hello tárolót használ.</span><span class="sxs-lookup"><span data-stu-id="ad78f-199">hello type of hello dataset depends on hello data store you use.</span></span> <span data-ttu-id="ad78f-200">Tekintse meg a következő táblázat felsorolja a Data Factory által támogatott adattároló hello.</span><span class="sxs-lookup"><span data-stu-id="ad78f-200">See hello following table for a list of data stores supported by Data Factory.</span></span> <span data-ttu-id="ad78f-201">Kattintson egy adatokat tároló toolearn hogyan toocreate összekapcsolt szolgáltatás és az adatok dataset tárolja.</span><span class="sxs-lookup"><span data-stu-id="ad78f-201">Click a data store toolearn how toocreate a linked service and a dataset for that data store.</span></span>

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> <span data-ttu-id="ad78f-202">Adatok tárolja a * lehet helyszíni vagy Azure-infrastruktúra (IaaS) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ad78f-202">Data stores with * can be on-premises or on Azure infrastructure as a service (IaaS).</span></span> <span data-ttu-id="ad78f-203">Ezen adatok áruház használatához tooinstall [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-203">These data stores require you tooinstall [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="ad78f-204">Az előző szakaszban hello hello példában hello típusú hello adatkészlet túl van beállítva**AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="ad78f-204">In hello example in hello previous section, hello type of hello dataset is set too**AzureSqlTable**.</span></span> <span data-ttu-id="ad78f-205">Ehhez hasonlóan egy Azure-Blob adatkészlet hello típusú hello dataset értéke túl**AzureBlob**, ahogy az alábbi JSON hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-205">Similarly, for an Azure Blob dataset, hello type of hello dataset is set too**AzureBlob**, as shown in hello following JSON:</span></span>

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

## <span data-ttu-id="ad78f-206"><a name="Structure"></a>Adatkészlet-szerkezetekben</span><span class="sxs-lookup"><span data-stu-id="ad78f-206"><a name="Structure"></a>Dataset structure</span></span>
<span data-ttu-id="ad78f-207">Hello **struktúra** szakasz nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="ad78f-207">hello **structure** section is optional.</span></span> <span data-ttu-id="ad78f-208">Hello séma hello adatkészlet nevét és az oszlopok adattípusát gyűjteményét tartalmazó szerint határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ad78f-208">It defines hello schema of hello dataset by containing a collection of names and data types of columns.</span></span> <span data-ttu-id="ad78f-209">Hello struktúra szakasz tooprovide típus használt tooconvert típusok és információk hello forrás toohello cél térkép oszlopokat használja.</span><span class="sxs-lookup"><span data-stu-id="ad78f-209">You use hello structure section tooprovide type information that is used tooconvert types and map columns from hello source toohello destination.</span></span> <span data-ttu-id="ad78f-210">A következő példa hello, hello dataset három oszlopot tartalmaz: `slicetimestamp`, `projectname`, és `pageviews`.</span><span class="sxs-lookup"><span data-stu-id="ad78f-210">In hello following example, hello dataset has three columns: `slicetimestamp`, `projectname`, and `pageviews`.</span></span> <span data-ttu-id="ad78f-211">Vannak típusú karakterlánc, karakterlánc, és tizedes, illetve.</span><span class="sxs-lookup"><span data-stu-id="ad78f-211">They are of type String, String, and Decimal, respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="ad78f-212">Minden egyes oszlopának hello struktúra tartalmaz hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="ad78f-212">Each column in hello structure contains hello following properties:</span></span>

| <span data-ttu-id="ad78f-213">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ad78f-213">Property</span></span> | <span data-ttu-id="ad78f-214">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad78f-214">Description</span></span> | <span data-ttu-id="ad78f-215">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ad78f-215">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad78f-216">név</span><span class="sxs-lookup"><span data-stu-id="ad78f-216">name</span></span> |<span data-ttu-id="ad78f-217">Hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="ad78f-217">Name of hello column.</span></span> |<span data-ttu-id="ad78f-218">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-218">Yes</span></span> |
| <span data-ttu-id="ad78f-219">type</span><span class="sxs-lookup"><span data-stu-id="ad78f-219">type</span></span> |<span data-ttu-id="ad78f-220">Hello oszlop adattípusát.</span><span class="sxs-lookup"><span data-stu-id="ad78f-220">Data type of hello column.</span></span>  |<span data-ttu-id="ad78f-221">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-221">No</span></span> |
| <span data-ttu-id="ad78f-222">Kulturális környezet</span><span class="sxs-lookup"><span data-stu-id="ad78f-222">culture</span></span> |<span data-ttu-id="ad78f-223">. A NET-alapú kulturális környezet toobe használható, ha hello típusú .NET-típus: `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="ad78f-223">.NET-based culture toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="ad78f-224">hello alapértelmezett érték a `en-us`.</span><span class="sxs-lookup"><span data-stu-id="ad78f-224">hello default is `en-us`.</span></span> |<span data-ttu-id="ad78f-225">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-225">No</span></span> |
| <span data-ttu-id="ad78f-226">Formátumban</span><span class="sxs-lookup"><span data-stu-id="ad78f-226">format</span></span> |<span data-ttu-id="ad78f-227">Formázza használható, ha hello típus a .NET-típus: karakterlánc toobe: `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="ad78f-227">Format string toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="ad78f-228">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-228">No</span></span> |

<span data-ttu-id="ad78f-229">hello következő irányelvek segítségével meghatározhatja, mikor tooinclude szerkezeti információkat, és milyen tooinclude a hello **struktúra** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad78f-229">hello following guidelines help you determine when tooinclude structure information, and what tooinclude in hello **structure** section.</span></span>

* <span data-ttu-id="ad78f-230">**A strukturált adatforrások**, adja meg a hello struktúra szakaszban csak akkor, ha azt szeretné, hogy a forrás oszlopok toosink oszlop leképezése, és vannak a nevek nem hello azonos.</span><span class="sxs-lookup"><span data-stu-id="ad78f-230">**For structured data sources**, specify hello structure section only if you want map source columns toosink columns, and their names are not hello same.</span></span> <span data-ttu-id="ad78f-231">Strukturált adatforrás az ilyen adatok séma- és tároló típus maga hello adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="ad78f-231">This kind of structured data source stores data schema and type information along with hello data itself.</span></span> <span data-ttu-id="ad78f-232">Strukturált adatforrások például SQL Server, az Oracle és az Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="ad78f-232">Examples of structured data sources include SQL Server, Oracle, and Azure table.</span></span> 
  
    <span data-ttu-id="ad78f-233">Típusra vonatkozó adat már áll rendelkezésre a strukturált adatforrásokhoz, akkor nem tartalmazhat típusinformációt amikor hello struktúra szakasz tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ad78f-233">As type information is already available for structured data sources, you should not include type information when you do include hello structure section.</span></span>
* <span data-ttu-id="ad78f-234">**Olvassa el az adatforrások (kifejezetten Blob-tároló) a séma**, dönthet úgy, toostore adatok nélkül hello adatokkal bármely séma vagy típusú információk tárolására.</span><span class="sxs-lookup"><span data-stu-id="ad78f-234">**For schema on read data sources (specifically Blob storage)**, you can choose toostore data without storing any schema or type information with hello data.</span></span> <span data-ttu-id="ad78f-235">Az ilyen típusú adatforrások tartalmazhat struktúra, ha azt szeretné, hogy toomap forrásoszlopokat oszlopok toosink.</span><span class="sxs-lookup"><span data-stu-id="ad78f-235">For these types of data sources, include structure when you want toomap source columns toosink columns.</span></span> <span data-ttu-id="ad78f-236">Amennyiben hello adatkészletet másolási tevékenységhez bemeneti, és adattípusok forrás adatkészlet hello fogadó konvertált toonative típust kell terjednie az struktúra.</span><span class="sxs-lookup"><span data-stu-id="ad78f-236">Also include structure when hello dataset is an input for a copy activity, and data types of source dataset should be converted toonative types for hello sink.</span></span> 
    
    <span data-ttu-id="ad78f-237">Adat-előállító támogatja a következő struktúrában típusú adatokat ad értékeinek hello: **Int16, Int32, Int64, egyetlen, Double, Decimal, Byte [], logikai érték, karakterlánc, Guid, Datetime, Datetimeoffset és Timespan**.</span><span class="sxs-lookup"><span data-stu-id="ad78f-237">Data Factory supports hello following values for providing type information in structure: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan**.</span></span> <span data-ttu-id="ad78f-238">Ezek az értékek közös nyelvi specifikáció (CLS)-kompatibilis. A NET-alapú típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="ad78f-238">These values are Common Language Specification (CLS)-compliant, .NET-based type values.</span></span>

<span data-ttu-id="ad78f-239">Adat-előállító automatikusan típuskonverziók hajt végre, a forrásadatok az adattároló tooa fogadó adattár áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="ad78f-239">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span> 
  

## <a name="dataset-availability"></a><span data-ttu-id="ad78f-240">Adatkészlet rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="ad78f-240">Dataset availability</span></span>
<span data-ttu-id="ad78f-241">Hello **rendelkezésre állási** hello feldolgozási időszakában (például óránként, naponta, vagy hetente) hello adatkészlet rész DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="ad78f-241">hello **availability** section in a dataset defines hello processing window (for example, hourly, daily, or weekly) for hello dataset.</span></span> <span data-ttu-id="ad78f-242">Tevékenység windows kapcsolatos további információkért lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-242">For more information about activity windows, see [Scheduling and execution](data-factory-scheduling-and-execution.md).</span></span>

<span data-ttu-id="ad78f-243">hello rendelkezésre állással kapcsolatos szakaszának alábbi határozza meg, hogy hello kimeneti adathalmazt vagy előállított óránként vagy hello bemeneti adatkészlet érhető el Óránként:</span><span class="sxs-lookup"><span data-stu-id="ad78f-243">hello following availability section specifies that hello output dataset is either produced hourly, or hello input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="ad78f-244">Ha hello folyamat a következő kezdési és befejezési időpontjai hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-244">If hello pipeline has hello following start and end times:</span></span>  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

<span data-ttu-id="ad78f-245">hello kimeneti adatkészlet hozzák óránkénti belül hello folyamat kezdési és befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="ad78f-245">hello output dataset is produced hourly within hello pipeline start and end times.</span></span> <span data-ttu-id="ad78f-246">Ezért nincsenek öt dataset szeletek által az adatcsatornát, egy az egyes tevékenységek ablakát (12 óra - 13 AM, 13: 00 – 2 óra, Reggel 2 - 3 AM, hajnali 3 Órakor - 4 AM, hajnali 4 Órakor – 5: 00).</span><span class="sxs-lookup"><span data-stu-id="ad78f-246">Therefore, there are five dataset slices produced by this pipeline, one for each activity window (12 AM - 1 AM, 1 AM - 2 AM, 2 AM - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM).</span></span> 

<span data-ttu-id="ad78f-247">hello következő táblázat segítségével is hello rendelkezésre állással kapcsolatos szakaszának tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="ad78f-247">hello following table describes properties you can use in hello availability section:</span></span>

| <span data-ttu-id="ad78f-248">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ad78f-248">Property</span></span> | <span data-ttu-id="ad78f-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad78f-249">Description</span></span> | <span data-ttu-id="ad78f-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ad78f-250">Required</span></span> | <span data-ttu-id="ad78f-251">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="ad78f-251">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ad78f-252">frequency</span><span class="sxs-lookup"><span data-stu-id="ad78f-252">frequency</span></span> |<span data-ttu-id="ad78f-253">Megadja a dataset szelet üzemi hello időegységét.</span><span class="sxs-lookup"><span data-stu-id="ad78f-253">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="ad78f-254"><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap</span><span class="sxs-lookup"><span data-stu-id="ad78f-254"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="ad78f-255">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-255">Yes</span></span> |<span data-ttu-id="ad78f-256">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-256">NA</span></span> |
| <span data-ttu-id="ad78f-257">interval</span><span class="sxs-lookup"><span data-stu-id="ad78f-257">interval</span></span> |<span data-ttu-id="ad78f-258">Megadja a gyakoriság egy szorzóval.</span><span class="sxs-lookup"><span data-stu-id="ad78f-258">Specifies a multiplier for frequency.</span></span><br/><br/><span data-ttu-id="ad78f-259">"X időköz" határozza meg, milyen gyakran hello szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="ad78f-259">"Frequency x interval" determines how often hello slice is produced.</span></span> <span data-ttu-id="ad78f-260">Például, ha a dataset toobe szeletelhetők óránként kell hello, beállíthatja <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="ad78f-260">For example, if you need hello dataset toobe sliced on an hourly basis, you set <b>frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="ad78f-261">Vegye figyelembe, hogy ha megad **gyakoriság** , **perc**, 15-nál kisebb hello időköz toono kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="ad78f-261">Note that if you specify **frequency** as **Minute**, you should set hello interval toono less than 15.</span></span> |<span data-ttu-id="ad78f-262">Igen</span><span class="sxs-lookup"><span data-stu-id="ad78f-262">Yes</span></span> |<span data-ttu-id="ad78f-263">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-263">NA</span></span> |
| <span data-ttu-id="ad78f-264">stílus</span><span class="sxs-lookup"><span data-stu-id="ad78f-264">style</span></span> |<span data-ttu-id="ad78f-265">Meghatározza, hogy hello szelet akkor a rendszer hello kezdő vagy hello időköz végén.</span><span class="sxs-lookup"><span data-stu-id="ad78f-265">Specifies whether hello slice should be produced at hello start or end of hello interval.</span></span><ul><li><span data-ttu-id="ad78f-266">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="ad78f-266">StartOfInterval</span></span></li><li><span data-ttu-id="ad78f-267">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="ad78f-267">EndOfInterval</span></span></li></ul><span data-ttu-id="ad78f-268">Ha **gyakoriság** értéke túl**hónap**, és **stílus** értéke túl**EndOfInterval**, hello szelet a hónap utolsó napján hello jön létre.</span><span class="sxs-lookup"><span data-stu-id="ad78f-268">If **frequency** is set too**Month**, and **style** is set too**EndOfInterval**, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="ad78f-269">Ha **stílus** értéke túl**StartOfInterval**, hello szelet hozzák hello a hónap első napján.</span><span class="sxs-lookup"><span data-stu-id="ad78f-269">If **style** is set too**StartOfInterval**, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="ad78f-270">Ha **gyakoriság** értéke túl**nap**, és **stílus** értéke túl**EndOfInterval**, hello szelet előállított hello hello naponta az elmúlt egy órában.</span><span class="sxs-lookup"><span data-stu-id="ad78f-270">If **frequency** is set too**Day**, and **style** is set too**EndOfInterval**, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="ad78f-271">Ha **gyakoriság** értéke túl**óra**, és **stílus** értéke túl**EndOfInterval**, hello szelet hello végének hello keletkezik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-271">If **frequency** is set too**Hour**, and **style** is set too**EndOfInterval**, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="ad78f-272">Például a hello du. 1-2 PM időszak adatszelethez, hello szelet hozzák 2 du..</span><span class="sxs-lookup"><span data-stu-id="ad78f-272">For example, for a slice for hello 1 PM - 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="ad78f-273">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-273">No</span></span> |<span data-ttu-id="ad78f-274">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="ad78f-274">EndOfInterval</span></span> |
| <span data-ttu-id="ad78f-275">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="ad78f-275">anchorDateTime</span></span> |<span data-ttu-id="ad78f-276">Hello abszolút pozíciója a ennyi másodpercig használta hello Feladatütemező toocompute dataset szelet határok meghatározása.</span><span class="sxs-lookup"><span data-stu-id="ad78f-276">Defines hello absolute position in time used by hello scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="ad78f-277">Vegye figyelembe, hogy a propoerty részekből dátum, amelyek részletesebb hello megadottnál gyakorisága, ha hello részletesebb részek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="ad78f-277">Note that if this propoerty has date parts that are more granular than hello specified frequency, hello more granular parts are ignored.</span></span> <span data-ttu-id="ad78f-278">Például, ha hello **időköz** van **óránkénti** (gyakoriság: óra és időköz: 1), és hello **anchorDateTime** tartalmaz **percet és másodpercet**, majd a perc és a másodperc részét hello **anchorDateTime** figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="ad78f-278">For example, if hello **interval** is **hourly** (frequency: hour and interval: 1), and hello **anchorDateTime** contains **minutes and seconds**, then hello minutes and seconds parts of **anchorDateTime** are ignored.</span></span> |<span data-ttu-id="ad78f-279">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-279">No</span></span> |<span data-ttu-id="ad78f-280">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="ad78f-280">01/01/0001</span></span> |
| <span data-ttu-id="ad78f-281">Az offset</span><span class="sxs-lookup"><span data-stu-id="ad78f-281">offset</span></span> |<span data-ttu-id="ad78f-282">TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette.</span><span class="sxs-lookup"><span data-stu-id="ad78f-282">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="ad78f-283">Ne feledje, ha mindkét **anchorDateTime** és **eltolás** megadott, hello eredménye kombinált hello shift.</span><span class="sxs-lookup"><span data-stu-id="ad78f-283">Note that if both **anchorDateTime** and **offset** are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="ad78f-284">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-284">No</span></span> |<span data-ttu-id="ad78f-285">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-285">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="ad78f-286">az eltolási – példa</span><span class="sxs-lookup"><span data-stu-id="ad78f-286">offset example</span></span>
<span data-ttu-id="ad78f-287">Alapértelmezés szerint naponta (`"frequency": "Day", "interval": 1`) szeletek start: 00 (éjfél) egyezményes világidő (UTC).</span><span class="sxs-lookup"><span data-stu-id="ad78f-287">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM (midnight) Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="ad78f-288">Ha ehelyett hello kezdési idő toobe reggel 6 óra UTC idő szerint, be eltolásnál, ahogy az alábbi részlet hello hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-288">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="ad78f-289">anchorDateTime – példa</span><span class="sxs-lookup"><span data-stu-id="ad78f-289">anchorDateTime example</span></span>
<span data-ttu-id="ad78f-290">A következő példa hello hello dataset 23 óránként jön létre.</span><span class="sxs-lookup"><span data-stu-id="ad78f-290">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="ad78f-291">hello első szelet elindítja időpontban hello által megadott **anchorDateTime**, melynek értéke túl`2017-04-19T08:00:00` (idő szerint UTC).</span><span class="sxs-lookup"><span data-stu-id="ad78f-291">hello first slice starts at hello time specified by **anchorDateTime**, which is set too`2017-04-19T08:00:00` (UTC).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="ad78f-292">Az offset/style – példa</span><span class="sxs-lookup"><span data-stu-id="ad78f-292">offset/style example</span></span>
<span data-ttu-id="ad78f-293">hello következő dataset van havi, és a hello jön létre havonta, de a 3. (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="ad78f-293">hello following dataset is monthly, and is produced on hello 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <span data-ttu-id="ad78f-294"><a name="Policy"></a>A DataSet házirend</span><span class="sxs-lookup"><span data-stu-id="ad78f-294"><a name="Policy"></a>Dataset policy</span></span>
<span data-ttu-id="ad78f-295">Hello **házirend** hello feltételek rész az hello adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="ad78f-295">hello **policy** section in hello dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

### <a name="validation-policies"></a><span data-ttu-id="ad78f-296">Házirendek érvényesítése</span><span class="sxs-lookup"><span data-stu-id="ad78f-296">Validation policies</span></span>
| <span data-ttu-id="ad78f-297">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="ad78f-297">Policy name</span></span> | <span data-ttu-id="ad78f-298">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad78f-298">Description</span></span> | <span data-ttu-id="ad78f-299">Alkalmazott túl</span><span class="sxs-lookup"><span data-stu-id="ad78f-299">Applied too</span></span>| <span data-ttu-id="ad78f-300">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ad78f-300">Required</span></span> | <span data-ttu-id="ad78f-301">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="ad78f-301">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="ad78f-302">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="ad78f-302">minimumSizeMB</span></span> |<span data-ttu-id="ad78f-303">Ellenőrzi, hogy hello adatok **Azure Blob Storage tárolóban** megfelel-e hello (megabájtban) a minimális méret követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="ad78f-303">Validates that hello data in **Azure Blob storage** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="ad78f-304">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="ad78f-304">Azure Blob storage</span></span> |<span data-ttu-id="ad78f-305">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-305">No</span></span> |<span data-ttu-id="ad78f-306">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-306">NA</span></span> |
| <span data-ttu-id="ad78f-307">minimumRows</span><span class="sxs-lookup"><span data-stu-id="ad78f-307">minimumRows</span></span> |<span data-ttu-id="ad78f-308">Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ad78f-308">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="ad78f-309">Az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="ad78f-309">Azure SQL database</span></span></li><li><span data-ttu-id="ad78f-310">Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="ad78f-310">Azure table</span></span></li></ul> |<span data-ttu-id="ad78f-311">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-311">No</span></span> |<span data-ttu-id="ad78f-312">NA</span><span class="sxs-lookup"><span data-stu-id="ad78f-312">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="ad78f-313">Példák</span><span class="sxs-lookup"><span data-stu-id="ad78f-313">Examples</span></span>
<span data-ttu-id="ad78f-314">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="ad78f-314">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="ad78f-315">**minimumRows:**</span><span class="sxs-lookup"><span data-stu-id="ad78f-315">**minimumRows:**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a><span data-ttu-id="ad78f-316">Külső adatkészletek</span><span class="sxs-lookup"><span data-stu-id="ad78f-316">External datasets</span></span>
<span data-ttu-id="ad78f-317">Külső adatkészletek hello gazdarendszerhez egy futó folyamatot hello adat-előállítóban nem hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="ad78f-317">External datasets are hello ones that are not produced by a running pipeline in hello data factory.</span></span> <span data-ttu-id="ad78f-318">Ha hello dataset van megjelölve **külső**, hello **ExternalData** szabályzata lehet hello dataset szelet rendelkezésre állási meghatározott tooinfluence hello viselkedését.</span><span class="sxs-lookup"><span data-stu-id="ad78f-318">If hello dataset is marked as **external**, hello **ExternalData** policy may be defined tooinfluence hello behavior of hello dataset slice availability.</span></span>

<span data-ttu-id="ad78f-319">Kivéve, ha a Data Factory hozzák alatt álló adatkészlet, azt kell megjelölni **külső**.</span><span class="sxs-lookup"><span data-stu-id="ad78f-319">Unless a dataset is being produced by Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="ad78f-320">Ez a beállítás általában egy folyamatot, az első tevékenység toohello bemenetek vonatkozik, kivéve, ha a tevékenység vagy csővezeték-láncolás használatban van.</span><span class="sxs-lookup"><span data-stu-id="ad78f-320">This setting generally applies toohello inputs of first activity in a pipeline, unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="ad78f-321">Név</span><span class="sxs-lookup"><span data-stu-id="ad78f-321">Name</span></span> | <span data-ttu-id="ad78f-322">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad78f-322">Description</span></span> | <span data-ttu-id="ad78f-323">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ad78f-323">Required</span></span> | <span data-ttu-id="ad78f-324">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ad78f-324">Default value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ad78f-325">dataDelay</span><span class="sxs-lookup"><span data-stu-id="ad78f-325">dataDelay</span></span> |<span data-ttu-id="ad78f-326">a adva szelet hello idő toodelay hello hello rendelkezésre állását hello hello külső adatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ad78f-326">hello time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="ad78f-327">Például egy óránkénti ellenőrzést késleltetheti a beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="ad78f-327">For example, you can delay an hourly check by using this setting.</span></span><br/><br/><span data-ttu-id="ad78f-328">hello beállítás csak akkor érvényes toohello jelenlegi idő.</span><span class="sxs-lookup"><span data-stu-id="ad78f-328">hello setting only applies toohello present time.</span></span>  <span data-ttu-id="ad78f-329">Például ha 1:00 PM azonnal, és az értéke 10 perc hello érvényesítési kezdődik, 1:10 óra.</span><span class="sxs-lookup"><span data-stu-id="ad78f-329">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="ad78f-330">Vegye figyelembe, hogy ez a beállítás nem befolyásolja az elmúlt hello szeletek.</span><span class="sxs-lookup"><span data-stu-id="ad78f-330">Note that this setting does not affect slices in hello past.</span></span> <span data-ttu-id="ad78f-331">A szeletek **szelet befejezésének** + **dataDelay** < **most** dolgoznak fel késedelem nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad78f-331">Slices with **Slice End Time** + **dataDelay** < **Now** are processed without any delay.</span></span><br/><br/><span data-ttu-id="ad78f-332">Időpontokban nagyobb, mint 23:59 óra meg kell határozni hello segítségével `day.hours:minutes:seconds` formátumban.</span><span class="sxs-lookup"><span data-stu-id="ad78f-332">Times greater than 23:59 hours should be specified by using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="ad78f-333">Például toospecify 24 órát, ne használjon 24:00:00.</span><span class="sxs-lookup"><span data-stu-id="ad78f-333">For example, toospecify 24 hours, don't use 24:00:00.</span></span> <span data-ttu-id="ad78f-334">Ehelyett használjon 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="ad78f-334">Instead, use 1.00:00:00.</span></span> <span data-ttu-id="ad78f-335">Ha 24:00:00 használja, akkor a rendszer 24 napos (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="ad78f-335">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="ad78f-336">1 nap és 4 óra adja meg 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="ad78f-336">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="ad78f-337">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-337">No</span></span> |<span data-ttu-id="ad78f-338">0</span><span class="sxs-lookup"><span data-stu-id="ad78f-338">0</span></span> |
| <span data-ttu-id="ad78f-339">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="ad78f-339">retryInterval</span></span> |<span data-ttu-id="ad78f-340">hello várakozási idő hibákhoz és hello következő kísérlet között.</span><span class="sxs-lookup"><span data-stu-id="ad78f-340">hello wait time between a failure and hello next attempt.</span></span> <span data-ttu-id="ad78f-341">Ez a beállítás toopresent idő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-341">This setting applies toopresent time.</span></span> <span data-ttu-id="ad78f-342">Ha hello előző próbálja sikertelen volt, hello következő kísérlet után hello van **retryInterval** időszak.</span><span class="sxs-lookup"><span data-stu-id="ad78f-342">If hello previous try failed, hello next try is after hello **retryInterval** period.</span></span> <br/><br/><span data-ttu-id="ad78f-343">Ha 1:00 PM most, az első lépések hello első próbálkozás.</span><span class="sxs-lookup"><span data-stu-id="ad78f-343">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="ad78f-344">Hello időtartama toocomplete hello első eredetiség ellenőrzésének 1 perc és hello művelet végrehajtása sikertelen volt, hello legközelebbi újrapróbálkozás akkor 1:00 + 1 perc (időtartam) + (újrapróbálkozási időköz) 1 perc = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="ad78f-344">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1min (duration) + 1min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="ad78f-345">Az elmúlt hello szeletek nincs késleltetés.</span><span class="sxs-lookup"><span data-stu-id="ad78f-345">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="ad78f-346">hello újrapróbálkozási azonnal történik.</span><span class="sxs-lookup"><span data-stu-id="ad78f-346">hello retry happens immediately.</span></span> |<span data-ttu-id="ad78f-347">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-347">No</span></span> |<span data-ttu-id="ad78f-348">00:01:00 (1 perc)</span><span class="sxs-lookup"><span data-stu-id="ad78f-348">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="ad78f-349">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="ad78f-349">retryTimeout</span></span> |<span data-ttu-id="ad78f-350">hello időkorlátjának újrapróbálkozási kísérletei.</span><span class="sxs-lookup"><span data-stu-id="ad78f-350">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="ad78f-351">Ha ez a tulajdonság értéke too10 perc hello érvényesítési 10 percen belül kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="ad78f-351">If this property is set too10 minutes, hello validation should be completed within 10 minutes.</span></span> <span data-ttu-id="ad78f-352">Ha hosszabb ideig tart, mint 10 percig tooperform hello érvényesítési, hello időtúllépést próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="ad78f-352">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="ad78f-353">Ha bármilyen kísérlet a hello érvényesítési túllépi az időkorlátot, hello szelet jelölésű **időtúllépésbe került**.</span><span class="sxs-lookup"><span data-stu-id="ad78f-353">If all attempts for hello validation time out, hello slice is marked as **TimedOut**.</span></span> |<span data-ttu-id="ad78f-354">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-354">No</span></span> |<span data-ttu-id="ad78f-355">00:10:00 (10 perc)</span><span class="sxs-lookup"><span data-stu-id="ad78f-355">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="ad78f-356">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="ad78f-356">maximumRetry</span></span> |<span data-ttu-id="ad78f-357">hello számú alkalommal fordult elő az toocheck a hello hello külső adatok rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="ad78f-357">hello number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="ad78f-358">hello megengedett maximális érték: 10.</span><span class="sxs-lookup"><span data-stu-id="ad78f-358">hello maximum allowed value is 10.</span></span> |<span data-ttu-id="ad78f-359">Nem</span><span class="sxs-lookup"><span data-stu-id="ad78f-359">No</span></span> |<span data-ttu-id="ad78f-360">3</span><span class="sxs-lookup"><span data-stu-id="ad78f-360">3</span></span> |


## <a name="create-datasets"></a><span data-ttu-id="ad78f-361">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad78f-361">Create datasets</span></span>
<span data-ttu-id="ad78f-362">Az ezen eszközök vagy az SDK-k adatkészletek hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="ad78f-362">You can create datasets by using one of these tools or SDKs:</span></span> 

- <span data-ttu-id="ad78f-363">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="ad78f-363">Copy Wizard</span></span> 
- <span data-ttu-id="ad78f-364">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad78f-364">Azure portal</span></span>
- <span data-ttu-id="ad78f-365">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad78f-365">Visual Studio</span></span>
- <span data-ttu-id="ad78f-366">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad78f-366">PowerShell</span></span>
- <span data-ttu-id="ad78f-367">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="ad78f-367">Azure Resource Manager template</span></span>
- <span data-ttu-id="ad78f-368">REST API</span><span class="sxs-lookup"><span data-stu-id="ad78f-368">REST API</span></span>
- <span data-ttu-id="ad78f-369">.NET API</span><span class="sxs-lookup"><span data-stu-id="ad78f-369">.NET API</span></span>

<span data-ttu-id="ad78f-370">Tekintse meg a lépésenkénti útmutatók adatcsatornákat és adathalmazokat egyikével a következő eszközök, illetve az SDK-k létrehozása a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-370">See hello following tutorials for step-by-step instructions for creating pipelines and datasets by using one of these tools or SDKs:</span></span>
 
- [<span data-ttu-id="ad78f-371">Adatátalakítási tevékenységgel rendelkező folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad78f-371">Build a pipeline with a data transformation activity</span></span>](data-factory-build-your-first-pipeline.md)
- [<span data-ttu-id="ad78f-372">Adatok mozgása tevékenységgel folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad78f-372">Build a pipeline with a data movement activity</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

<span data-ttu-id="ad78f-373">Miután egy folyamat létrehozása és telepítése, felügyelheti és figyelheti a folyamatok használatával hello Azure portál paneleken, vagy hello figyelés és a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ad78f-373">After a pipeline is created and deployed, you can manage and monitor your pipelines by using hello Azure portal blades, or hello Monitoring and Management app.</span></span> <span data-ttu-id="ad78f-374">Tekintse meg a következő témakörök részletes utasításokat hello:</span><span class="sxs-lookup"><span data-stu-id="ad78f-374">See hello following topics for step-by-step instructions:</span></span> 

- [<span data-ttu-id="ad78f-375">Figyelheti és folyamatok kezelése az Azure portál paneleken használatával</span><span class="sxs-lookup"><span data-stu-id="ad78f-375">Monitor and manage pipelines by using Azure portal blades</span></span>](data-factory-monitor-manage-pipelines.md)
- [<span data-ttu-id="ad78f-376">Figyelheti és kezelheti a folyamatok hello figyelés és felügyelet alkalmazással</span><span class="sxs-lookup"><span data-stu-id="ad78f-376">Monitor and manage pipelines by using hello Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a><span data-ttu-id="ad78f-377">Hatókört használó adatkészletek</span><span class="sxs-lookup"><span data-stu-id="ad78f-377">Scoped datasets</span></span>
<span data-ttu-id="ad78f-378">Létrehozhat hello segítségével hatókörön belüli tooa csővezeték adatkészletek **adatkészletek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ad78f-378">You can create datasets that are scoped tooa pipeline by using hello **datasets** property.</span></span> <span data-ttu-id="ad78f-379">Ezek az adatkészletek csak akkor használható, ez az adatcsatorna tevékenységét, nem pedig más folyamatok tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="ad78f-379">These datasets can only be used by activities within this pipeline, not by activities in other pipelines.</span></span> <span data-ttu-id="ad78f-380">a következő példa hello folyamat két adatkészletet (InputDataset-rdc és OutputDataset-rdc) toobe hello csővezeték történik az határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ad78f-380">hello following example defines a pipeline with two datasets (InputDataset-rdc and OutputDataset-rdc) toobe used within hello pipeline.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="ad78f-381">Egyszeri adatcsatornák csak a hatókörön belüli adatkészletek támogatottak (ahol **pipelineMode** értéke túl**OneTime**).</span><span class="sxs-lookup"><span data-stu-id="ad78f-381">Scoped datasets are supported only with one-time pipelines (where **pipelineMode** is set too**OneTime**).</span></span> <span data-ttu-id="ad78f-382">Lásd: [Onetime csővezeték](data-factory-create-pipelines.md#onetime-pipeline) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="ad78f-382">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="ad78f-383">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad78f-383">Next steps</span></span>
- <span data-ttu-id="ad78f-384">Folyamatok kapcsolatos további információkért lásd: [hozzon létre adatcsatornák](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-384">For more information about pipelines, see [Create pipelines](data-factory-create-pipelines.md).</span></span> 
- <span data-ttu-id="ad78f-385">Hogyan adatcsatornák ütemezett és végrehajtott kapcsolatos további információkért lásd: [ütemezés és a végrehajtása az Azure Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ad78f-385">For more information about how pipelines are scheduled and executed, see [Scheduling and execution in Azure Data Factory](data-factory-scheduling-and-execution.md).</span></span> 
