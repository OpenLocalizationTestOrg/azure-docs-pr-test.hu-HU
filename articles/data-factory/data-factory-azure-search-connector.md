---
title: "aaaPush adatok tooSearch index Data Factory használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toopush adatok tooAzure Search-Index Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a><span data-ttu-id="17027-103">Adatok tooan Azure Search-index leküldéses Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="17027-103">Push data tooan Azure Search index by using Azure Data Factory</span></span>
<span data-ttu-id="17027-104">Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység toopush támogatott forrásadatok adattároló tooAzure Search-index.</span><span class="sxs-lookup"><span data-stu-id="17027-104">This article describes how toouse hello Copy Activity toopush data from a supported source data store tooAzure Search index.</span></span> <span data-ttu-id="17027-105">Támogatott forráshierarchiából adattárolókhoz hello forrásoszlopa hello szereplő [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="17027-105">Supported source data stores are listed in hello Source column of hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="17027-106">Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.</span><span class="sxs-lookup"><span data-stu-id="17027-106">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="17027-107">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="17027-107">Enabling connectivity</span></span>
<span data-ttu-id="17027-108">tooallow Data Factory szolgáltatásnak csatlakozás helyszíni adattár tooan, az adatkezelési átjáró telepítése a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="17027-108">tooallow Data Factory service connect tooan on-premises data store, you install Data Management Gateway in your on-premises environment.</span></span> <span data-ttu-id="17027-109">Átjáró telepíthető hello ugyanaz a gép, amelyen hello forrásadatok tárolja, vagy egy másik számítógépre tooavoid hello adatokkal erőforrások használják a tárolja.</span><span class="sxs-lookup"><span data-stu-id="17027-109">You can install gateway on hello same machine that hosts hello source data store or on a separate machine tooavoid competing for resources with hello data store.</span></span>

<span data-ttu-id="17027-110">Az adatkezelési átjáró csatlakozik a helyszíni adatok források toocloud szolgáltatások biztonságos és felügyelt módon.</span><span class="sxs-lookup"><span data-stu-id="17027-110">Data Management Gateway connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="17027-111">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="17027-111">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="17027-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="17027-112">Getting started</span></span>
<span data-ttu-id="17027-113">A másolási tevékenység, amely a leküldéses értesítések adatok egy forrás adatokat tároló tooAzure Search-index a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="17027-113">You can create a pipeline with a copy activity that pushes data from a source data store tooAzure Search index by using different tools/APIs.</span></span>

<span data-ttu-id="17027-114">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="17027-114">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="17027-115">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="17027-115">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="17027-116">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="17027-116">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="17027-117">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="17027-117">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="17027-118">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="17027-118">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="17027-119">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="17027-119">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="17027-120">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="17027-120">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="17027-121">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="17027-121">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="17027-122">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="17027-122">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="17027-123">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="17027-123">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="17027-124">Az adat-előállító entitások, amelyek használt toocopy adatok tooAzure Search-index JSON-definíciók minta, lásd: [JSON-példa: adatok másolása a helyszíni SQL Server tooAzure Search-index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="17027-124">For a sample with JSON definitions for Data Factory entities that are used toocopy data tooAzure Search index, see [JSON example: Copy data from on-premises SQL Server tooAzure Search index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) section of this article.</span></span> 

<span data-ttu-id="17027-125">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Search-Index részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="17027-125">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Search Index:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="17027-126">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="17027-126">Linked service properties</span></span>

<span data-ttu-id="17027-127">hello a következő táblázat ismerteti, amelyek adott toohello csatolt Azure Search szolgáltatás JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="17027-127">hello following table provides descriptions for JSON elements that are specific toohello Azure Search linked service.</span></span>

| <span data-ttu-id="17027-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17027-128">Property</span></span> | <span data-ttu-id="17027-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="17027-129">Description</span></span> | <span data-ttu-id="17027-130">Szükséges</span><span class="sxs-lookup"><span data-stu-id="17027-130">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="17027-131">type</span><span class="sxs-lookup"><span data-stu-id="17027-131">type</span></span> | <span data-ttu-id="17027-132">hello type tulajdonságot kell beállítani: **AzureSearch**.</span><span class="sxs-lookup"><span data-stu-id="17027-132">hello type property must be set to: **AzureSearch**.</span></span> | <span data-ttu-id="17027-133">Igen</span><span class="sxs-lookup"><span data-stu-id="17027-133">Yes</span></span> |
| <span data-ttu-id="17027-134">URL-címe</span><span class="sxs-lookup"><span data-stu-id="17027-134">url</span></span> | <span data-ttu-id="17027-135">Hello Azure Search szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="17027-135">URL for hello Azure Search service.</span></span> | <span data-ttu-id="17027-136">Igen</span><span class="sxs-lookup"><span data-stu-id="17027-136">Yes</span></span> |
| <span data-ttu-id="17027-137">kulcs</span><span class="sxs-lookup"><span data-stu-id="17027-137">key</span></span> | <span data-ttu-id="17027-138">Adminisztrátori kulcsot a hello Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="17027-138">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="17027-139">Igen</span><span class="sxs-lookup"><span data-stu-id="17027-139">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="17027-140">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="17027-140">Dataset properties</span></span>

<span data-ttu-id="17027-141">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="17027-141">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="17027-142">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="17027-142">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span> <span data-ttu-id="17027-143">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="17027-143">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="17027-144">a DataSet hello típusú szakasz hello typeProperties **AzureSearchIndex** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="17027-144">hello typeProperties section for a dataset of hello type **AzureSearchIndex** has hello following properties:</span></span>

| <span data-ttu-id="17027-145">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17027-145">Property</span></span> | <span data-ttu-id="17027-146">Leírás</span><span class="sxs-lookup"><span data-stu-id="17027-146">Description</span></span> | <span data-ttu-id="17027-147">Szükséges</span><span class="sxs-lookup"><span data-stu-id="17027-147">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="17027-148">type</span><span class="sxs-lookup"><span data-stu-id="17027-148">type</span></span> | <span data-ttu-id="17027-149">hello type tulajdonság túl be kell állítani**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="17027-149">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="17027-150">Igen</span><span class="sxs-lookup"><span data-stu-id="17027-150">Yes</span></span> |
| <span data-ttu-id="17027-151">indexName</span><span class="sxs-lookup"><span data-stu-id="17027-151">indexName</span></span> | <span data-ttu-id="17027-152">Hello Azure Search-index neve.</span><span class="sxs-lookup"><span data-stu-id="17027-152">Name of hello Azure Search index.</span></span> <span data-ttu-id="17027-153">Adat-előállító nem hoz létre hello index.</span><span class="sxs-lookup"><span data-stu-id="17027-153">Data Factory does not create hello index.</span></span> <span data-ttu-id="17027-154">az Azure Search léteznie kell a hello index.</span><span class="sxs-lookup"><span data-stu-id="17027-154">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="17027-155">Igen</span><span class="sxs-lookup"><span data-stu-id="17027-155">Yes</span></span> |


## <a name="copy-activity-properties"></a><span data-ttu-id="17027-156">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="17027-156">Copy activity properties</span></span>
<span data-ttu-id="17027-157">Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="17027-157">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="17027-158">Például a nevét, leírását, bemeneti és kimeneti tábláinak és különböző házirendek tulajdonságok minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="17027-158">Properties such as name, description, input and output tables, and various policies are available for all types of activities.</span></span> <span data-ttu-id="17027-159">Mivel a hello typeProperties szakaszban rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="17027-159">Whereas, properties available in hello typeProperties section vary with each activity type.</span></span> <span data-ttu-id="17027-160">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="17027-160">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="17027-161">A másolási tevékenység, ha hello fogadó hello típusú **AzureSearchIndexSink**, typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="17027-161">For Copy Activity, when hello sink is of hello type **AzureSearchIndexSink**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="17027-162">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17027-162">Property</span></span> | <span data-ttu-id="17027-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="17027-163">Description</span></span> | <span data-ttu-id="17027-164">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="17027-164">Allowed values</span></span> | <span data-ttu-id="17027-165">Szükséges</span><span class="sxs-lookup"><span data-stu-id="17027-165">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="17027-166">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="17027-166">WriteBehavior</span></span> | <span data-ttu-id="17027-167">Meghatározza, hogy toomerge vagy cserélje le a dokumentum már létezik hello index.</span><span class="sxs-lookup"><span data-stu-id="17027-167">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> <span data-ttu-id="17027-168">Lásd: hello [WriteBehavior tulajdonság](#writebehavior-property).</span><span class="sxs-lookup"><span data-stu-id="17027-168">See hello [WriteBehavior property](#writebehavior-property).</span></span>| <span data-ttu-id="17027-169">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="17027-169">Merge (default)</span></span><br/><span data-ttu-id="17027-170">Feltöltés</span><span class="sxs-lookup"><span data-stu-id="17027-170">Upload</span></span>| <span data-ttu-id="17027-171">Nem</span><span class="sxs-lookup"><span data-stu-id="17027-171">No</span></span> |
| <span data-ttu-id="17027-172">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="17027-172">WriteBatchSize</span></span> | <span data-ttu-id="17027-173">Fájlfeltöltések hello Azure Search-index az adatokat, amikor hello puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="17027-173">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> <span data-ttu-id="17027-174">Lásd: hello [WriteBatchSize tulajdonság](#writebatchsize-property) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="17027-174">See hello [WriteBatchSize property](#writebatchsize-property) for details.</span></span> | <span data-ttu-id="17027-175">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="17027-175">1 too1,000.</span></span> <span data-ttu-id="17027-176">Alapértelmezett érték 1000.</span><span class="sxs-lookup"><span data-stu-id="17027-176">Default value is 1000.</span></span> | <span data-ttu-id="17027-177">Nem</span><span class="sxs-lookup"><span data-stu-id="17027-177">No</span></span> |

### <a name="writebehavior-property"></a><span data-ttu-id="17027-178">WriteBehavior tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17027-178">WriteBehavior property</span></span>
<span data-ttu-id="17027-179">AzureSearchSink upserts adatok írásakor.</span><span class="sxs-lookup"><span data-stu-id="17027-179">AzureSearchSink upserts when writing data.</span></span> <span data-ttu-id="17027-180">Más szóval történő írásakor egy dokumentumot, ha hello dokumentum kulcs már létezik a hello Azure Search-index, az Azure Search frissíti hello meglévő dokumentumról, hanem egy ütközés Kivétel kiváltása.</span><span class="sxs-lookup"><span data-stu-id="17027-180">In other words, when writing a document, if hello document key already exists in hello Azure Search index, Azure Search updates hello existing document rather than throwing a conflict exception.</span></span>

<span data-ttu-id="17027-181">hello AzureSearchSink két upsert viselkedések (AzureSearch SDK használatával) a következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="17027-181">hello AzureSearchSink provides hello following two upsert behaviors (by using AzureSearch SDK):</span></span>

- <span data-ttu-id="17027-182">**Egyesítési**: hello új dokumentum hello oszlopok egyesíthető egy meglévő hello.</span><span class="sxs-lookup"><span data-stu-id="17027-182">**Merge**: combine all hello columns in hello new document with hello existing one.</span></span> <span data-ttu-id="17027-183">Az új dokumentum hello null értékű oszlopokhoz hello értéket egy meglévő hello megőrződik.</span><span class="sxs-lookup"><span data-stu-id="17027-183">For columns with null value in hello new document, hello value in hello existing one is preserved.</span></span>
- <span data-ttu-id="17027-184">**Töltse fel**: hello új dokumentum cserél hello meglévőt.</span><span class="sxs-lookup"><span data-stu-id="17027-184">**Upload**: hello new document replaces hello existing one.</span></span> <span data-ttu-id="17027-185">Nincs megadva a hello új dokumentum oszlopok hello értéke toonull van-e egy nem null értéket hello meglévő dokumentum vagy sem.</span><span class="sxs-lookup"><span data-stu-id="17027-185">For columns not specified in hello new document, hello value is set toonull whether there is a non-null value in hello existing document or not.</span></span>

<span data-ttu-id="17027-186">hello alapértelmezett viselkedése **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="17027-186">hello default behavior is **Merge**.</span></span>

### <a name="writebatchsize-property"></a><span data-ttu-id="17027-187">WriteBatchSize tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17027-187">WriteBatchSize Property</span></span>
<span data-ttu-id="17027-188">Az Azure Search szolgáltatás egy kötegelt dokumentumok írása támogatja.</span><span class="sxs-lookup"><span data-stu-id="17027-188">Azure Search service supports writing documents as a batch.</span></span> <span data-ttu-id="17027-189">A kötegelt 1 too1, 000 műveletek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="17027-189">A batch can contain 1 too1,000 Actions.</span></span> <span data-ttu-id="17027-190">Egy művelet egy dokumentum tooperform hello feltöltés/egyesítési művelet kezeli.</span><span class="sxs-lookup"><span data-stu-id="17027-190">An action handles one document tooperform hello upload/merge operation.</span></span>

### <a name="data-type-support"></a><span data-ttu-id="17027-191">Adattípus-támogatás</span><span class="sxs-lookup"><span data-stu-id="17027-191">Data type support</span></span>
<span data-ttu-id="17027-192">hello alábbi táblázat megadja, hogy egy Azure Search adattípus támogatott-e, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="17027-192">hello following table specifies whether an Azure Search data type is supported or not.</span></span>

| <span data-ttu-id="17027-193">Az Azure Search-adattípus</span><span class="sxs-lookup"><span data-stu-id="17027-193">Azure Search data type</span></span> | <span data-ttu-id="17027-194">Az Azure Search fogadó támogatott</span><span class="sxs-lookup"><span data-stu-id="17027-194">Supported in Azure Search Sink</span></span> |
| ---------------------- | ------------------------------ |
| <span data-ttu-id="17027-195">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="17027-195">String</span></span> | <span data-ttu-id="17027-196">I</span><span class="sxs-lookup"><span data-stu-id="17027-196">Y</span></span> |
| <span data-ttu-id="17027-197">Int32</span><span class="sxs-lookup"><span data-stu-id="17027-197">Int32</span></span> | <span data-ttu-id="17027-198">I</span><span class="sxs-lookup"><span data-stu-id="17027-198">Y</span></span> |
| <span data-ttu-id="17027-199">Int64</span><span class="sxs-lookup"><span data-stu-id="17027-199">Int64</span></span> | <span data-ttu-id="17027-200">I</span><span class="sxs-lookup"><span data-stu-id="17027-200">Y</span></span> |
| <span data-ttu-id="17027-201">Dupla</span><span class="sxs-lookup"><span data-stu-id="17027-201">Double</span></span> | <span data-ttu-id="17027-202">I</span><span class="sxs-lookup"><span data-stu-id="17027-202">Y</span></span> |
| <span data-ttu-id="17027-203">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="17027-203">Boolean</span></span> | <span data-ttu-id="17027-204">I</span><span class="sxs-lookup"><span data-stu-id="17027-204">Y</span></span> |
| <span data-ttu-id="17027-205">DataTimeOffset</span><span class="sxs-lookup"><span data-stu-id="17027-205">DataTimeOffset</span></span> | <span data-ttu-id="17027-206">I</span><span class="sxs-lookup"><span data-stu-id="17027-206">Y</span></span> |
| <span data-ttu-id="17027-207">Karakterlánc-tömbben</span><span class="sxs-lookup"><span data-stu-id="17027-207">String Array</span></span> | <span data-ttu-id="17027-208">N</span><span class="sxs-lookup"><span data-stu-id="17027-208">N</span></span> |
| <span data-ttu-id="17027-209">GeographyPoint</span><span class="sxs-lookup"><span data-stu-id="17027-209">GeographyPoint</span></span> | <span data-ttu-id="17027-210">N</span><span class="sxs-lookup"><span data-stu-id="17027-210">N</span></span> |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a><span data-ttu-id="17027-211">JSON-példa: adatok másolása a helyszíni SQL Server tooAzure Search-index</span><span class="sxs-lookup"><span data-stu-id="17027-211">JSON example: Copy data from on-premises SQL Server tooAzure Search index</span></span>

<span data-ttu-id="17027-212">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="17027-212">hello following sample shows:</span></span>

1.  <span data-ttu-id="17027-213">A társított szolgáltatás típusa [AzureSearch](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="17027-213">A linked service of type [AzureSearch](#linked-service-properties).</span></span>
2.  <span data-ttu-id="17027-214">A társított szolgáltatás típusa [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="17027-214">A linked service of type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span></span>
3.  <span data-ttu-id="17027-215">Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="17027-215">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
4.  <span data-ttu-id="17027-216">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSearchIndex](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="17027-216">An output [dataset](data-factory-create-datasets.md) of type [AzureSearchIndex](#dataset-properties).</span></span>
4.  <span data-ttu-id="17027-217">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) és [AzureSearchIndexSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="17027-217">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) and [AzureSearchIndexSink](#copy-activity-properties).</span></span>

<span data-ttu-id="17027-218">hello minta másol idősorozat adatokat a helyszíni SQL Server adatbázis tooan Azure Search-index óránként.</span><span class="sxs-lookup"><span data-stu-id="17027-218">hello sample copies time-series data from an on-premises SQL Server database tooan Azure Search index hourly.</span></span> <span data-ttu-id="17027-219">Ez a minta használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="17027-219">hello JSON properties used in this sample are described in sections following hello samples.</span></span>

<span data-ttu-id="17027-220">Első lépésként a telepítő hello az adatkezelési átjáró a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="17027-220">As a first step, setup hello data management gateway on your on-premises machine.</span></span> <span data-ttu-id="17027-221">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="17027-221">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="17027-222">**Az Azure Search kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="17027-222">**Azure Search linked service:**</span></span>

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="17027-223">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="17027-223">**SQL Server linked service**</span></span>

```JSON
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

<span data-ttu-id="17027-224">**SQL Server bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="17027-224">**SQL Server input dataset**</span></span>

<span data-ttu-id="17027-225">hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="17027-225">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="17027-226">Több tábla belül azonos adatbázist egyetlen dataset, de egy táblát kell használni az hello dataset tableName typeProperty hello keresztül kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="17027-226">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="17027-227">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="17027-227">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "SqlServerDataset",
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

<span data-ttu-id="17027-228">**Az Azure Search kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="17027-228">**Azure Search output dataset:**</span></span>

<span data-ttu-id="17027-229">hello minta másolatok adatok tooan Azure Search-index nevű **termékek**.</span><span class="sxs-lookup"><span data-stu-id="17027-229">hello sample copies data tooan Azure Search index named **products**.</span></span> <span data-ttu-id="17027-230">Adat-előállító nem hoz létre hello index.</span><span class="sxs-lookup"><span data-stu-id="17027-230">Data Factory does not create hello index.</span></span> <span data-ttu-id="17027-231">tootest hello mintát, index létrehozása ezen a néven.</span><span class="sxs-lookup"><span data-stu-id="17027-231">tootest hello sample, create an index with this name.</span></span> <span data-ttu-id="17027-232">Hozzon létre hello Azure Search-index hello azonos számú oszlopot hasonlóan hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="17027-232">Create hello Azure Search index with hello same number of columns as in hello input dataset.</span></span> <span data-ttu-id="17027-233">Új bejegyzések kerülnek toohello Azure Search-index óránként.</span><span class="sxs-lookup"><span data-stu-id="17027-233">New entries are added toohello Azure Search index every hour.</span></span>

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

<span data-ttu-id="17027-234">**Másolási tevékenység során a folyamat az SQL-forrás és fogadó Azure Search-Index:**</span><span class="sxs-lookup"><span data-stu-id="17027-234">**Copy activity in a pipeline with SQL source and Azure Search Index sink:**</span></span>

<span data-ttu-id="17027-235">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="17027-235">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="17027-236">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**AzureSearchIndexSink**.</span><span class="sxs-lookup"><span data-stu-id="17027-236">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**AzureSearchIndexSink**.</span></span> <span data-ttu-id="17027-237">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="17027-237">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

<span data-ttu-id="17027-238">Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="17027-238">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="17027-239">hello JSON alábbi kódrészletben láthatja a másolási tevékenység során szükséges hello módosítása `typeProperties` példaként.</span><span class="sxs-lookup"><span data-stu-id="17027-239">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="17027-240">Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.</span><span class="sxs-lookup"><span data-stu-id="17027-240">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a><span data-ttu-id="17027-241">A felhő forrás másolása</span><span class="sxs-lookup"><span data-stu-id="17027-241">Copy from a cloud source</span></span>
<span data-ttu-id="17027-242">Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="17027-242">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="17027-243">hello JSON alábbi kódrészletben láthatja a másolási tevékenység során szükséges hello módosítása `typeProperties` példaként.</span><span class="sxs-lookup"><span data-stu-id="17027-243">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="17027-244">Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.</span><span class="sxs-lookup"><span data-stu-id="17027-244">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

<span data-ttu-id="17027-245">Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="17027-245">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="17027-246">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="17027-246">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="17027-247">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="17027-247">Performance and tuning</span></span>  
<span data-ttu-id="17027-248">Lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn hatás teljesítmény adatátvitelt jelölik a (másolási tevékenység), és különböző módokon toooptimize tényezők azt.</span><span class="sxs-lookup"><span data-stu-id="17027-248">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17027-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17027-249">Next steps</span></span>
<span data-ttu-id="17027-250">Tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="17027-250">See hello following articles:</span></span>

* <span data-ttu-id="17027-251">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="17027-251">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
