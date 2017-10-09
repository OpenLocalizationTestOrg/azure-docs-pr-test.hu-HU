---
title: az Azure Table aaaMove adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toomove adatokat az Azure Table Storage Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a><span data-ttu-id="c96d5-103">Adatok tooand áthelyezése az Azure Data Factory használatához Azure táblából</span><span class="sxs-lookup"><span data-stu-id="c96d5-103">Move data tooand from Azure Table using Azure Data Factory</span></span>
<span data-ttu-id="c96d5-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure Table Storage és a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c96d5-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Table Storage.</span></span> <span data-ttu-id="c96d5-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="c96d5-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="c96d5-106">Bármely támogatott forráshierarchiából adatokat tooAzure Table Storage tárolja, és az Azure Table Storage támogatott tooany fogadó adatok tárolásához adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="c96d5-106">You can copy data from any supported source data store tooAzure Table Storage or from Azure Table Storage tooany supported sink data store.</span></span> <span data-ttu-id="c96d5-107">Adatforrások vagy mosdók hello másolási tevékenység által támogatott adattárolókhoz listáját lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="c96d5-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="c96d5-108">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c96d5-108">Getting started</span></span>
<span data-ttu-id="c96d5-109">A másolási tevékenység, amely helyezi át az adatokat az Azure Table Storage és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c96d5-109">You can create a pipeline with a copy activity that moves data to/from an Azure Table Storage by using different tools/APIs.</span></span>

<span data-ttu-id="c96d5-110">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-110">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="c96d5-111">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c96d5-111">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="c96d5-112">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-112">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c96d5-113">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="c96d5-113">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="c96d5-114">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="c96d5-114">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="c96d5-115">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c96d5-115">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="c96d5-116">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="c96d5-116">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="c96d5-117">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="c96d5-117">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="c96d5-118">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="c96d5-118">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="c96d5-119">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="c96d5-119">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="c96d5-120">JSON-definíciók, amelyek használt toocopy adatokat az Azure Table Storage az adat-előállító entitások minták, lásd: [JSON példák](#json-examples) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c96d5-120">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Table Storage, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="c96d5-121">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Table Storage részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="c96d5-121">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Table Storage:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="c96d5-122">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c96d5-122">Linked service properties</span></span>
<span data-ttu-id="c96d5-123">Két különböző összekapcsolt szolgáltatások toolink egy Azure blob storage tooan az Azure data factory használatával.</span><span class="sxs-lookup"><span data-stu-id="c96d5-123">There are two types of linked services you can use toolink an Azure blob storage tooan Azure data factory.</span></span> <span data-ttu-id="c96d5-124">Ezek: **AzureStorage** társított szolgáltatás és **AzureStorageSas** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c96d5-124">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="c96d5-125">hello Azure tárolás társított szolgáltatásának biztosít hello data Factory összetevőnek a globális hozzáférési toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c96d5-125">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="c96d5-126">Mivel hello Azure Storage SAS (közös hozzáférésű Jogosultságkód) kapcsolódó szolgáltatás biztosítja azt az Azure Storage korlátozott/időhöz kötött hozzáférés toohello hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c96d5-126">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="c96d5-127">Nincsenek más különbségek a következő két összekapcsolt szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="c96d5-127">There are no other differences between these two linked services.</span></span> <span data-ttu-id="c96d5-128">Válassza ki az igényeinek megfelelő kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c96d5-128">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="c96d5-129">hello következő szakaszokban további részleteket a következő két összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c96d5-129">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="c96d5-130">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c96d5-130">Dataset properties</span></span>
<span data-ttu-id="c96d5-131">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c96d5-131">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c96d5-132">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="c96d5-132">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="c96d5-133">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c96d5-133">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="c96d5-134">Hello **typeProperties** típusú hello adatkészlet szakasz **AzureTable** hello alábbi tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c96d5-134">hello **typeProperties** section for hello dataset of type **AzureTable** has hello following properties.</span></span>

| <span data-ttu-id="c96d5-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c96d5-135">Property</span></span> | <span data-ttu-id="c96d5-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="c96d5-136">Description</span></span> | <span data-ttu-id="c96d5-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c96d5-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c96d5-138">tableName</span><span class="sxs-lookup"><span data-stu-id="c96d5-138">tableName</span></span> |<span data-ttu-id="c96d5-139">Hello hello Azure tábla adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c96d5-139">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="c96d5-140">Igen.</span><span class="sxs-lookup"><span data-stu-id="c96d5-140">Yes.</span></span> <span data-ttu-id="c96d5-141">Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="c96d5-141">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="c96d5-142">Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="c96d5-142">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

### <a name="schema-by-data-factory"></a><span data-ttu-id="c96d5-143">Adat-előállító sémája</span><span class="sxs-lookup"><span data-stu-id="c96d5-143">Schema by Data Factory</span></span>
<span data-ttu-id="c96d5-144">Sémamentesadat-tárolókhoz, például az Azure tábla a Data Factory szolgáltatásnak hello kikövetkezteti hello séma a következő módokon hello egyikében:</span><span class="sxs-lookup"><span data-stu-id="c96d5-144">For schema-free data stores such as Azure Table, hello Data Factory service infers hello schema in one of hello following ways:</span></span>

1. <span data-ttu-id="c96d5-145">Ha megadja az adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság eleget tegyen hello séma szerint ez a struktúra.</span><span class="sxs-lookup"><span data-stu-id="c96d5-145">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="c96d5-146">Ebben az esetben ha egy sort tartalmaz egy olyan oszlop értékét, null értékű biztosított azt.</span><span class="sxs-lookup"><span data-stu-id="c96d5-146">In this case, if a row does not contain a value for a column, a null value is provided for it.</span></span>
2. <span data-ttu-id="c96d5-147">Ha nem ad meg adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban, adat-előállító tulajdonság hello séma kikövetkezteti hello adatok első sorának hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c96d5-147">If you don't specify hello structure of data by using hello **structure** property in hello dataset definition, Data Factory infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="c96d5-148">Ebben az esetben ha hello első sor nem tartalmaz teljes séma hello, azokat az oszlopokat vannak nem talált a másolási művelet hello eredményét.</span><span class="sxs-lookup"><span data-stu-id="c96d5-148">In this case, if hello first row does not contain hello full schema, some columns are missed in hello result of copy operation.</span></span>

<span data-ttu-id="c96d5-149">Sémamentes adatforrások hello célszerű ezért hello segítségével adatok szerkezete toospecify hello **struktúra** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c96d5-149">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="c96d5-150">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c96d5-150">Copy activity properties</span></span>
<span data-ttu-id="c96d5-151">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c96d5-151">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c96d5-152">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c96d5-152">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span>

<span data-ttu-id="c96d5-153">Tulajdonságok tevékenységek minden típusának hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor függenek.</span><span class="sxs-lookup"><span data-stu-id="c96d5-153">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="c96d5-154">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="c96d5-154">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="c96d5-155">**AzureTableSource** következő tulajdonságai typeProperties szakaszban hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="c96d5-155">**AzureTableSource** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="c96d5-156">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c96d5-156">Property</span></span> | <span data-ttu-id="c96d5-157">Leírás</span><span class="sxs-lookup"><span data-stu-id="c96d5-157">Description</span></span> | <span data-ttu-id="c96d5-158">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="c96d5-158">Allowed values</span></span> | <span data-ttu-id="c96d5-159">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c96d5-159">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c96d5-160">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="c96d5-160">azureTableSourceQuery</span></span> |<span data-ttu-id="c96d5-161">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="c96d5-161">Use hello custom query tooread data.</span></span> |<span data-ttu-id="c96d5-162">Azure-tábla lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c96d5-162">Azure table query string.</span></span> <span data-ttu-id="c96d5-163">Példák a következő szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="c96d5-163">See examples in hello next section.</span></span> |<span data-ttu-id="c96d5-164">Nem.</span><span class="sxs-lookup"><span data-stu-id="c96d5-164">No.</span></span> <span data-ttu-id="c96d5-165">Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="c96d5-165">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="c96d5-166">Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="c96d5-166">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="c96d5-167">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="c96d5-167">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="c96d5-168">Azt jelzi, hogy swallow hello kivétel tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="c96d5-168">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="c96d5-169">IGAZ</span><span class="sxs-lookup"><span data-stu-id="c96d5-169">TRUE</span></span><br/><span data-ttu-id="c96d5-170">HAMIS</span><span class="sxs-lookup"><span data-stu-id="c96d5-170">FALSE</span></span> |<span data-ttu-id="c96d5-171">Nem</span><span class="sxs-lookup"><span data-stu-id="c96d5-171">No</span></span> |

### <a name="azuretablesourcequery-examples"></a><span data-ttu-id="c96d5-172">azureTableSourceQuery példák</span><span class="sxs-lookup"><span data-stu-id="c96d5-172">azureTableSourceQuery examples</span></span>
<span data-ttu-id="c96d5-173">Ha Azure táblaoszlop karakterlánc típusú:</span><span class="sxs-lookup"><span data-stu-id="c96d5-173">If Azure Table column is of string type:</span></span>

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

<span data-ttu-id="c96d5-174">Ha Azure táblaoszlop dátum/idő típusú:</span><span class="sxs-lookup"><span data-stu-id="c96d5-174">If Azure Table column is of datetime type:</span></span>

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

<span data-ttu-id="c96d5-175">**AzureTableSink** következő tulajdonságai typeProperties szakaszban hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="c96d5-175">**AzureTableSink** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="c96d5-176">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c96d5-176">Property</span></span> | <span data-ttu-id="c96d5-177">Leírás</span><span class="sxs-lookup"><span data-stu-id="c96d5-177">Description</span></span> | <span data-ttu-id="c96d5-178">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="c96d5-178">Allowed values</span></span> | <span data-ttu-id="c96d5-179">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c96d5-179">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c96d5-180">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="c96d5-180">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="c96d5-181">Alapértelmezett partíció kulcsérték hello a fogadó által használható.</span><span class="sxs-lookup"><span data-stu-id="c96d5-181">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="c96d5-182">Egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="c96d5-182">A string value.</span></span> |<span data-ttu-id="c96d5-183">Nem</span><span class="sxs-lookup"><span data-stu-id="c96d5-183">No</span></span> |
| <span data-ttu-id="c96d5-184">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="c96d5-184">azureTablePartitionKeyName</span></span> |<span data-ttu-id="c96d5-185">Adja meg, amelynek értékeket fogja használni, mint partíciókulcsok hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="c96d5-185">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="c96d5-186">Ha nincs megadva, AzureTableDefaultPartitionKeyValue hello partíciókulcs lesz.</span><span class="sxs-lookup"><span data-stu-id="c96d5-186">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="c96d5-187">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="c96d5-187">A column name.</span></span> |<span data-ttu-id="c96d5-188">Nem</span><span class="sxs-lookup"><span data-stu-id="c96d5-188">No</span></span> |
| <span data-ttu-id="c96d5-189">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="c96d5-189">azureTableRowKeyName</span></span> |<span data-ttu-id="c96d5-190">Adja meg, amelynek oszlop értékeit kulcsként sor hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="c96d5-190">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="c96d5-191">Ha nincs megadva, minden egyes sorára használjon a GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="c96d5-191">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="c96d5-192">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="c96d5-192">A column name.</span></span> |<span data-ttu-id="c96d5-193">Nem</span><span class="sxs-lookup"><span data-stu-id="c96d5-193">No</span></span> |
| <span data-ttu-id="c96d5-194">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="c96d5-194">azureTableInsertType</span></span> |<span data-ttu-id="c96d5-195">hello mód tooinsert adatait az Azure táblájába.</span><span class="sxs-lookup"><span data-stu-id="c96d5-195">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="c96d5-196">Ez a tulajdonság szabja meg, hogy meglévő hello kimeneti táblát, amely megfelelő a partíció-és sorkulcsok sora cseréje vagy egyesített értékükre.</span><span class="sxs-lookup"><span data-stu-id="c96d5-196">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="c96d5-197">toolearn arról, hogyan működnek ezek a beállítások (lemezegyesítési és -csere), lásd: [Insert vagy az egyesítéses entitás](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Insert vagy az entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="c96d5-197">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="c96d5-198">Ez a beállítás érvényes szinten hello sor nem hello táblaszintű, és sem a lehetőség törli a nem létező hello bemeneti hello kimeneti tábla sorainak.</span><span class="sxs-lookup"><span data-stu-id="c96d5-198">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="c96d5-199">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="c96d5-199">merge (default)</span></span><br/><span data-ttu-id="c96d5-200">cserélje le</span><span class="sxs-lookup"><span data-stu-id="c96d5-200">replace</span></span> |<span data-ttu-id="c96d5-201">Nem</span><span class="sxs-lookup"><span data-stu-id="c96d5-201">No</span></span> |
| <span data-ttu-id="c96d5-202">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="c96d5-202">writeBatchSize</span></span> |<span data-ttu-id="c96d5-203">Amikor hello writeBatchSize vagy writeBatchTimeout találati adatok beillesztése hello Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="c96d5-203">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="c96d5-204">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="c96d5-204">Integer (number of rows)</span></span> |<span data-ttu-id="c96d5-205">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="c96d5-205">No (default: 10000)</span></span> |
| <span data-ttu-id="c96d5-206">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="c96d5-206">writeBatchTimeout</span></span> |<span data-ttu-id="c96d5-207">Adatbeszúrást hello Azure-tábla amikor hello writeBatchSize vagy writeBatchTimeout találati</span><span class="sxs-lookup"><span data-stu-id="c96d5-207">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="c96d5-208">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c96d5-208">timespan</span></span><br/><br/><span data-ttu-id="c96d5-209">Példa: "00: 20:00" (20 perc)</span><span class="sxs-lookup"><span data-stu-id="c96d5-209">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="c96d5-210">Nem (alapértelmezett toostorage ügyfél alapértelmezett időtúllépési érték 90 másodperc)</span><span class="sxs-lookup"><span data-stu-id="c96d5-210">No (Default toostorage client default timeout value 90 sec)</span></span> |

### <a name="azuretablepartitionkeyname"></a><span data-ttu-id="c96d5-211">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="c96d5-211">azureTablePartitionKeyName</span></span>
<span data-ttu-id="c96d5-212">Képezze le a forrás oszlop tooa céloszlop hello fordító JSON tulajdonság használatával, mint hello azureTablePartitionKeyName hello céloszlop használatba vétele előtt.</span><span class="sxs-lookup"><span data-stu-id="c96d5-212">Map a source column tooa destination column using hello translator JSON property before you can use hello destination column as hello azureTablePartitionKeyName.</span></span>

<span data-ttu-id="c96d5-213">A következő példa hello, a forrásoszlop DivisionID a csatlakoztatott toohello céloszlop: DivisionID.</span><span class="sxs-lookup"><span data-stu-id="c96d5-213">In hello following example, source column DivisionID is mapped toohello destination column: DivisionID.</span></span>  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
<span data-ttu-id="c96d5-214">hello DivisionID hello partíciós kulcs van megadva.</span><span class="sxs-lookup"><span data-stu-id="c96d5-214">hello DivisionID is specified as hello partition key.</span></span>

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a><span data-ttu-id="c96d5-215">JSON-példák</span><span class="sxs-lookup"><span data-stu-id="c96d5-215">JSON examples</span></span>
<span data-ttu-id="c96d5-216">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c96d5-216">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c96d5-217">Azok hogyan toocopy adatok tooand Azure Table Storage és az Azure Blob-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c96d5-217">They show how toocopy data tooand from Azure Table Storage and Azure Blob Database.</span></span> <span data-ttu-id="c96d5-218">Azonban az adatok átmásolhatók **közvetlenül** bármelyik hello források tooany hello támogatott nyelő.</span><span class="sxs-lookup"><span data-stu-id="c96d5-218">However, data can be copied **directly** from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="c96d5-219">További információkért lásd: hello szakasz "támogatott adattárolókhoz és formátumok" a [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c96d5-219">For more information, see hello section "Supported data stores and formats" in [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a><span data-ttu-id="c96d5-220">Példa: Adatok másolása az Azure Table tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="c96d5-220">Example: Copy data from Azure Table tooAzure Blob</span></span>
<span data-ttu-id="c96d5-221">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="c96d5-221">hello following sample shows:</span></span>

1. <span data-ttu-id="c96d5-222">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (blob & tábla használatos).</span><span class="sxs-lookup"><span data-stu-id="c96d5-222">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob).</span></span>
2. <span data-ttu-id="c96d5-223">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-223">An input [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
3. <span data-ttu-id="c96d5-224">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-224">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="c96d5-225">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [AzureTableSource](#activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-225">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [AzureTableSource](#activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="c96d5-226">hello minta másolja az adatokat az Azure Table tooa blob toohello alapértelmezett partícióhoz tartozó minden órában.</span><span class="sxs-lookup"><span data-stu-id="c96d5-226">hello sample copies data belonging toohello default partition in an Azure Table tooa blob every hour.</span></span> <span data-ttu-id="c96d5-227">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c96d5-227">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c96d5-228">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-228">**Azure storage linked service:**</span></span>

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
<span data-ttu-id="c96d5-229">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-229">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="c96d5-230">Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg.</span><span class="sxs-lookup"><span data-stu-id="c96d5-230">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="c96d5-231">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="c96d5-231">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="c96d5-232">**Az Azure tábla bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-232">**Azure Table input dataset:**</span></span>

<span data-ttu-id="c96d5-233">hello példa feltételezi, hogy létrehozott egy "MyTable" tábla Azure tábla.</span><span class="sxs-lookup"><span data-stu-id="c96d5-233">hello sample assumes you have created a table “MyTable” in Azure Table.</span></span>

<span data-ttu-id="c96d5-234">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="c96d5-234">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="c96d5-235">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-235">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="c96d5-236">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="c96d5-236">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="c96d5-237">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="c96d5-237">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c96d5-238">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="c96d5-238">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="c96d5-239">**Másolási tevékenység során a folyamat AzureTableSource és BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-239">**Copy activity in a pipeline with AzureTableSource and BlobSink:**</span></span>

<span data-ttu-id="c96d5-240">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="c96d5-240">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="c96d5-241">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**AzureTableSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-241">In hello pipeline JSON definition, hello **source** type is set too**AzureTableSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="c96d5-242">hello SQL-lekérdezésben megadott **AzureTableSourceQuery** tulajdonság kiválaszt hello adatok hello alapértelmezett partíció minden órában toocopy.</span><span class="sxs-lookup"><span data-stu-id="c96d5-242">hello SQL query specified with **AzureTableSourceQuery** property selects hello data from hello default partition every hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a><span data-ttu-id="c96d5-243">Példa: Adatok másolása az Azure Blob tooAzure tábla</span><span class="sxs-lookup"><span data-stu-id="c96d5-243">Example: Copy data from Azure Blob tooAzure Table</span></span>
<span data-ttu-id="c96d5-244">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="c96d5-244">hello following sample shows:</span></span>

1. <span data-ttu-id="c96d5-245">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (blob & tábla használatos)</span><span class="sxs-lookup"><span data-stu-id="c96d5-245">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob)</span></span>
2. <span data-ttu-id="c96d5-246">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-246">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="c96d5-247">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-247">An output [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
4. <span data-ttu-id="c96d5-248">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [AzureTableSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c96d5-248">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureTableSink](#copy-activity-properties).</span></span>

<span data-ttu-id="c96d5-249">hello minta másol idősorozat adatokat az Azure blob tooan Azure-tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="c96d5-249">hello sample copies time-series data from an Azure blob tooan Azure table hourly.</span></span> <span data-ttu-id="c96d5-250">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c96d5-250">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c96d5-251">**A társított szolgáltatásnak Azure storage (az Azure tábla & Blob):**</span><span class="sxs-lookup"><span data-stu-id="c96d5-251">**Azure storage (for both Azure Table & Blob) linked service:**</span></span>

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

<span data-ttu-id="c96d5-252">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-252">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="c96d5-253">Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg.</span><span class="sxs-lookup"><span data-stu-id="c96d5-253">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="c96d5-254">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="c96d5-254">See [Linked Services](#linked-service-properties) section for details.</span></span>

<span data-ttu-id="c96d5-255">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-255">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="c96d5-256">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="c96d5-256">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="c96d5-257">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="c96d5-257">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c96d5-258">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="c96d5-258">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="c96d5-259">"external": "true" beállítás arról értesíti az adott hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="c96d5-259">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="c96d5-260">**Azure-tábla kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-260">**Azure Table output dataset:**</span></span>

<span data-ttu-id="c96d5-261">hello minta Azure Table "MyTable" nevű tooa adattábla másolja.</span><span class="sxs-lookup"><span data-stu-id="c96d5-261">hello sample copies data tooa table named “MyTable” in Azure Table.</span></span> <span data-ttu-id="c96d5-262">Hozzon létre egy Azure tábla azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="c96d5-262">Create an Azure table with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="c96d5-263">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="c96d5-263">New rows are added toohello table every hour.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="c96d5-264">**Másolási tevékenység során a folyamat BlobSource és AzureTableSink:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-264">**Copy activity in a pipeline with BlobSource and AzureTableSink:**</span></span>

<span data-ttu-id="c96d5-265">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="c96d5-265">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="c96d5-266">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**AzureTableSink**.</span><span class="sxs-lookup"><span data-stu-id="c96d5-266">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**AzureTableSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a><span data-ttu-id="c96d5-267">Az Azure-tábla leképezésének</span><span class="sxs-lookup"><span data-stu-id="c96d5-267">Type Mapping for Azure Table</span></span>
<span data-ttu-id="c96d5-268">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello.</span><span class="sxs-lookup"><span data-stu-id="c96d5-268">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="c96d5-269">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="c96d5-269">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="c96d5-270">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="c96d5-270">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="c96d5-271">Áthelyezésekor adatok túl & Azure táblából hello következő [Azure Table szolgáltatás által meghatározott hozzárendelések](https://msdn.microsoft.com/library/azure/dd179338.aspx) használják az Azure tábla OData típusok too.NET típusánál, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="c96d5-271">When moving data too& from Azure Table, hello following [mappings defined by Azure Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx) are used from Azure Table OData types too.NET type and vice versa.</span></span>

| <span data-ttu-id="c96d5-272">Az OData-adattípus</span><span class="sxs-lookup"><span data-stu-id="c96d5-272">OData Data Type</span></span> | <span data-ttu-id="c96d5-273">.NET-típusa</span><span class="sxs-lookup"><span data-stu-id="c96d5-273">.NET Type</span></span> | <span data-ttu-id="c96d5-274">Részletek</span><span class="sxs-lookup"><span data-stu-id="c96d5-274">Details</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c96d5-275">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="c96d5-275">Edm.Binary</span></span> |<span data-ttu-id="c96d5-276">Byte]</span><span class="sxs-lookup"><span data-stu-id="c96d5-276">byte[]</span></span> |<span data-ttu-id="c96d5-277">Bájttömb too64 KB fel.</span><span class="sxs-lookup"><span data-stu-id="c96d5-277">An array of bytes up too64 KB.</span></span> |
| <span data-ttu-id="c96d5-278">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="c96d5-278">Edm.Boolean</span></span> |<span data-ttu-id="c96d5-279">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c96d5-279">bool</span></span> |<span data-ttu-id="c96d5-280">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="c96d5-280">A Boolean value.</span></span> |
| <span data-ttu-id="c96d5-281">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="c96d5-281">Edm.DateTime</span></span> |<span data-ttu-id="c96d5-282">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="c96d5-282">DateTime</span></span> |<span data-ttu-id="c96d5-283">Egy 64 bites érték kifejezett, egyezményes világidő (UTC).</span><span class="sxs-lookup"><span data-stu-id="c96d5-283">A 64-bit value expressed as Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="c96d5-284">hello támogatott dátum és idő tartomány kezdődik 12:00 éjféltől. január 1, i.. 1601.</span><span class="sxs-lookup"><span data-stu-id="c96d5-284">hello supported DateTime range begins from 12:00 midnight, January 1, 1601 A.D.</span></span> <span data-ttu-id="c96d5-285">(SZ) (UTC).</span><span class="sxs-lookup"><span data-stu-id="c96d5-285">(C.E.), UTC.</span></span> <span data-ttu-id="c96d5-286">hello tartomány vége December 31 9999.</span><span class="sxs-lookup"><span data-stu-id="c96d5-286">hello range ends at December 31, 9999.</span></span> |
| <span data-ttu-id="c96d5-287">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="c96d5-287">Edm.Double</span></span> |<span data-ttu-id="c96d5-288">Dupla</span><span class="sxs-lookup"><span data-stu-id="c96d5-288">double</span></span> |<span data-ttu-id="c96d5-289">Egy 64 bites lebegőpontos értéket.</span><span class="sxs-lookup"><span data-stu-id="c96d5-289">A 64-bit floating point value.</span></span> |
| <span data-ttu-id="c96d5-290">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="c96d5-290">Edm.Guid</span></span> |<span data-ttu-id="c96d5-291">GUID</span><span class="sxs-lookup"><span data-stu-id="c96d5-291">Guid</span></span> |<span data-ttu-id="c96d5-292">A 128 bites globálisan egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c96d5-292">A 128-bit globally unique identifier.</span></span> |
| <span data-ttu-id="c96d5-293">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="c96d5-293">Edm.Int32</span></span> |<span data-ttu-id="c96d5-294">Int32</span><span class="sxs-lookup"><span data-stu-id="c96d5-294">Int32</span></span> |<span data-ttu-id="c96d5-295">Egy 32 bites egész számot.</span><span class="sxs-lookup"><span data-stu-id="c96d5-295">A 32-bit integer.</span></span> |
| <span data-ttu-id="c96d5-296">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="c96d5-296">Edm.Int64</span></span> |<span data-ttu-id="c96d5-297">Int64</span><span class="sxs-lookup"><span data-stu-id="c96d5-297">Int64</span></span> |<span data-ttu-id="c96d5-298">Egy 64 bites egész számot.</span><span class="sxs-lookup"><span data-stu-id="c96d5-298">A 64-bit integer.</span></span> |
| <span data-ttu-id="c96d5-299">Edm.String</span><span class="sxs-lookup"><span data-stu-id="c96d5-299">Edm.String</span></span> |<span data-ttu-id="c96d5-300">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c96d5-300">String</span></span> |<span data-ttu-id="c96d5-301">Az UTF-16 kódolású érték.</span><span class="sxs-lookup"><span data-stu-id="c96d5-301">A UTF-16-encoded value.</span></span> <span data-ttu-id="c96d5-302">Karakterlánc-értékek mentése too64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="c96d5-302">String values may be up too64 KB.</span></span> |

### <a name="type-conversion-sample"></a><span data-ttu-id="c96d5-303">Átalakítás minta</span><span class="sxs-lookup"><span data-stu-id="c96d5-303">Type Conversion Sample</span></span>
<span data-ttu-id="c96d5-304">a következő minta hello szolgál az adatok másolása az Azure Blob tooAzure tábla a típuskonverziók.</span><span class="sxs-lookup"><span data-stu-id="c96d5-304">hello following sample is for copying data from an Azure Blob tooAzure Table with type conversions.</span></span>

<span data-ttu-id="c96d5-305">Tegyük fel, hogy hello Blob-adathalmazra CSV formátumban van, és három oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c96d5-305">Suppose hello Blob dataset is in CSV format and contains three columns.</span></span> <span data-ttu-id="c96d5-306">Őket egyik hello hét napjára rövidített francia nevekkel egyéni dátum és idő formátumú dátum és idő oszlop.</span><span class="sxs-lookup"><span data-stu-id="c96d5-306">One of them is a datetime column with a custom datetime format using abbreviated French names for day of hello week.</span></span>

<span data-ttu-id="c96d5-307">Adja meg az alábbiak szerint együtt típusdefiníciók hello oszlopokhoz hello Blob-forrás adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c96d5-307">Define hello Blob Source dataset as follows along with type definitions for hello columns.</span></span>

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
<span data-ttu-id="c96d5-308">Megadott hello hozzárendelése az Azure tábla OData too.NET típusának, határozzák meg hello tábla Azure tábla a következő séma hello.</span><span class="sxs-lookup"><span data-stu-id="c96d5-308">Given hello type mapping from Azure Table OData type too.NET type, you would define hello table in Azure Table with hello following schema.</span></span>

<span data-ttu-id="c96d5-309">**Az Azure tábla sémája:**</span><span class="sxs-lookup"><span data-stu-id="c96d5-309">**Azure Table schema:**</span></span>

| <span data-ttu-id="c96d5-310">Oszlop neve</span><span class="sxs-lookup"><span data-stu-id="c96d5-310">Column name</span></span> | <span data-ttu-id="c96d5-311">Típus</span><span class="sxs-lookup"><span data-stu-id="c96d5-311">Type</span></span> |
| --- | --- |
| <span data-ttu-id="c96d5-312">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="c96d5-312">userid</span></span> |<span data-ttu-id="c96d5-313">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="c96d5-313">Edm.Int64</span></span> |
| <span data-ttu-id="c96d5-314">név</span><span class="sxs-lookup"><span data-stu-id="c96d5-314">name</span></span> |<span data-ttu-id="c96d5-315">Edm.String</span><span class="sxs-lookup"><span data-stu-id="c96d5-315">Edm.String</span></span> |
| <span data-ttu-id="c96d5-316">lastlogindate</span><span class="sxs-lookup"><span data-stu-id="c96d5-316">lastlogindate</span></span> |<span data-ttu-id="c96d5-317">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="c96d5-317">Edm.DateTime</span></span> |

<span data-ttu-id="c96d5-318">A következő határozza meg az alábbiak szerint hello Azure Table-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c96d5-318">Next, define hello Azure Table dataset as follows.</span></span> <span data-ttu-id="c96d5-319">Mivel hello típusra vonatkozó adat már meg van adva az alapul szolgáló adattár hello nem kell toospecify "structure" szakasz hello típusú adatokkal.</span><span class="sxs-lookup"><span data-stu-id="c96d5-319">You do not need toospecify “structure” section with hello type information since hello type information is already specified in hello underlying data store.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="c96d5-320">Ebben az esetben Data Factory automatikusan írja be a átalakítások, beleértve az hello egyéni dátum és idő formátumban hello "fr-fr" kulturális környezet használatával, amikor adatokat Blob tooAzure tábla hello Datetime mező.</span><span class="sxs-lookup"><span data-stu-id="c96d5-320">In this case, Data Factory automatically does type conversions including hello Datetime field with hello custom datetime format using hello "fr-fr" culture when moving data from Blob tooAzure Table.</span></span>

> [!NOTE]
> <span data-ttu-id="c96d5-321">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c96d5-321">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="c96d5-322">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="c96d5-322">Performance and Tuning</span></span>
<span data-ttu-id="c96d5-323">toolearn kulccsal kapcsolatos tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény, lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="c96d5-323">toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
