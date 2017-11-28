---
title: "Adatok leküldése Search-index használatával a Data Factory |} Microsoft Docs"
description: "További tudnivalók az adatok Azure Data Factory használatával küldje le az Azure Search-Index."
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
ms.openlocfilehash: 5c617c7a2f2eb4da2164ce5f802354a64dfd1fa1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a><span data-ttu-id="009b6-103">Elküldik az adatokat az Azure Search-index Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="009b6-103">Push data to an Azure Search index by using Azure Data Factory</span></span>
<span data-ttu-id="009b6-104">A cikkből megtudhatja, hogyan használható a másolási tevékenység során elküldik az adatokat egy támogatott forráshierarchiából adattárból az Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="009b6-104">This article describes how to use the Copy Activity to push data from a supported source data store to Azure Search index.</span></span> <span data-ttu-id="009b6-105">A forrás oszlop a felsorolt támogatott forrás adattárolókhoz a [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="009b6-105">Supported source data stores are listed in the Source column of the [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="009b6-106">Ez a cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.</span><span class="sxs-lookup"><span data-stu-id="009b6-106">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="009b6-107">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="009b6-107">Enabling connectivity</span></span>
<span data-ttu-id="009b6-108">Ahhoz, hogy a Data Factory service csatlakozás helyszíni adattárolóihoz, telepítse az adatkezelési átjáró a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="009b6-108">To allow Data Factory service connect to an on-premises data store, you install Data Management Gateway in your on-premises environment.</span></span> <span data-ttu-id="009b6-109">Átjáró telepíthető ugyanazon a számítógépen a forrásadatok állomások tároló vagy egy külön számítógépen erőforrás esetén az adattár versengő elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="009b6-109">You can install gateway on the same machine that hosts the source data store or on a separate machine to avoid competing for resources with the data store.</span></span>

<span data-ttu-id="009b6-110">Az adatkezelési átjáró csatlakozik a helyszíni adatforrások felhőszolgáltatások biztonságának és kezelésének módja.</span><span class="sxs-lookup"><span data-stu-id="009b6-110">Data Management Gateway connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="009b6-111">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="009b6-111">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="009b6-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="009b6-112">Getting started</span></span>
<span data-ttu-id="009b6-113">A másolási tevékenység során, az leküldi adatok egy forrás adattárból Azure Search-index különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="009b6-113">You can create a pipeline with a copy activity that pushes data from a source data store to Azure Search index by using different tools/APIs.</span></span>

<span data-ttu-id="009b6-114">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="009b6-114">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="009b6-115">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="009b6-115">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="009b6-116">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="009b6-116">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="009b6-117">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="009b6-117">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="009b6-118">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="009b6-118">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="009b6-119">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="009b6-119">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="009b6-120">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="009b6-120">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="009b6-121">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="009b6-121">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="009b6-122">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="009b6-122">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="009b6-123">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="009b6-123">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="009b6-124">Adatok másolása az Azure Search-index használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az helyszíni SQL Server az Azure Search-index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="009b6-124">For a sample with JSON definitions for Data Factory entities that are used to copy data to Azure Search index, see [JSON example: Copy data from on-premises SQL Server to Azure Search index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) section of this article.</span></span> 

<span data-ttu-id="009b6-125">A következő szakaszok részletesen bemutatják, amely segítségével az Azure Search-Index megadása a Data Factory tartozó entitások JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="009b6-125">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure Search Index:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="009b6-126">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="009b6-126">Linked service properties</span></span>

<span data-ttu-id="009b6-127">A következő táblázat ismerteti, amelyek a kapcsolódó Azure Search szolgáltatásra vonatkozó JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="009b6-127">The following table provides descriptions for JSON elements that are specific to the Azure Search linked service.</span></span>

| <span data-ttu-id="009b6-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="009b6-128">Property</span></span> | <span data-ttu-id="009b6-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="009b6-129">Description</span></span> | <span data-ttu-id="009b6-130">Szükséges</span><span class="sxs-lookup"><span data-stu-id="009b6-130">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="009b6-131">type</span><span class="sxs-lookup"><span data-stu-id="009b6-131">type</span></span> | <span data-ttu-id="009b6-132">A type tulajdonságot kell beállítani: **AzureSearch**.</span><span class="sxs-lookup"><span data-stu-id="009b6-132">The type property must be set to: **AzureSearch**.</span></span> | <span data-ttu-id="009b6-133">Igen</span><span class="sxs-lookup"><span data-stu-id="009b6-133">Yes</span></span> |
| <span data-ttu-id="009b6-134">URL-címe</span><span class="sxs-lookup"><span data-stu-id="009b6-134">url</span></span> | <span data-ttu-id="009b6-135">Az Azure Search szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="009b6-135">URL for the Azure Search service.</span></span> | <span data-ttu-id="009b6-136">Igen</span><span class="sxs-lookup"><span data-stu-id="009b6-136">Yes</span></span> |
| <span data-ttu-id="009b6-137">kulcs</span><span class="sxs-lookup"><span data-stu-id="009b6-137">key</span></span> | <span data-ttu-id="009b6-138">Az Azure Search szolgáltatás adminisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="009b6-138">Admin key for the Azure Search service.</span></span> | <span data-ttu-id="009b6-139">Igen</span><span class="sxs-lookup"><span data-stu-id="009b6-139">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="009b6-140">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="009b6-140">Dataset properties</span></span>

<span data-ttu-id="009b6-141">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="009b6-141">For a full list of sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="009b6-142">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="009b6-142">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span> <span data-ttu-id="009b6-143">A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="009b6-143">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="009b6-144">A typeProperties szakasz egy adatkészlet típusú **AzureSearchIndex** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="009b6-144">The typeProperties section for a dataset of the type **AzureSearchIndex** has the following properties:</span></span>

| <span data-ttu-id="009b6-145">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="009b6-145">Property</span></span> | <span data-ttu-id="009b6-146">Leírás</span><span class="sxs-lookup"><span data-stu-id="009b6-146">Description</span></span> | <span data-ttu-id="009b6-147">Szükséges</span><span class="sxs-lookup"><span data-stu-id="009b6-147">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="009b6-148">type</span><span class="sxs-lookup"><span data-stu-id="009b6-148">type</span></span> | <span data-ttu-id="009b6-149">A type tulajdonságot meg kell **AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="009b6-149">The type property must be set to **AzureSearchIndex**.</span></span>| <span data-ttu-id="009b6-150">Igen</span><span class="sxs-lookup"><span data-stu-id="009b6-150">Yes</span></span> |
| <span data-ttu-id="009b6-151">indexName</span><span class="sxs-lookup"><span data-stu-id="009b6-151">indexName</span></span> | <span data-ttu-id="009b6-152">Az Azure Search-index neve.</span><span class="sxs-lookup"><span data-stu-id="009b6-152">Name of the Azure Search index.</span></span> <span data-ttu-id="009b6-153">Adat-előállító nem hoz létre az indexet.</span><span class="sxs-lookup"><span data-stu-id="009b6-153">Data Factory does not create the index.</span></span> <span data-ttu-id="009b6-154">Az index az Azure Search léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="009b6-154">The index must exist in Azure Search.</span></span> | <span data-ttu-id="009b6-155">Igen</span><span class="sxs-lookup"><span data-stu-id="009b6-155">Yes</span></span> |


## <a name="copy-activity-properties"></a><span data-ttu-id="009b6-156">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="009b6-156">Copy activity properties</span></span>
<span data-ttu-id="009b6-157">Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: a [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="009b6-157">For a full list of sections and properties that are available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="009b6-158">Például a nevét, leírását, bemeneti és kimeneti tábláinak és különböző házirendek tulajdonságok minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="009b6-158">Properties such as name, description, input and output tables, and various policies are available for all types of activities.</span></span> <span data-ttu-id="009b6-159">Mivel a typeProperties szakaszban rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="009b6-159">Whereas, properties available in the typeProperties section vary with each activity type.</span></span> <span data-ttu-id="009b6-160">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="009b6-160">For Copy Activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="009b6-161">A másolási tevékenység, ha a fogadó típusa nem **AzureSearchIndexSink**, a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="009b6-161">For Copy Activity, when the sink is of the type **AzureSearchIndexSink**, the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="009b6-162">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="009b6-162">Property</span></span> | <span data-ttu-id="009b6-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="009b6-163">Description</span></span> | <span data-ttu-id="009b6-164">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="009b6-164">Allowed values</span></span> | <span data-ttu-id="009b6-165">Szükséges</span><span class="sxs-lookup"><span data-stu-id="009b6-165">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="009b6-166">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="009b6-166">WriteBehavior</span></span> | <span data-ttu-id="009b6-167">Megadja, hogy egyesíteni vagy cserélje le, ha az index már létezik egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="009b6-167">Specifies whether to merge or replace when a document already exists in the index.</span></span> <span data-ttu-id="009b6-168">Tekintse meg a [WriteBehavior tulajdonság](#writebehavior-property).</span><span class="sxs-lookup"><span data-stu-id="009b6-168">See the [WriteBehavior property](#writebehavior-property).</span></span>| <span data-ttu-id="009b6-169">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="009b6-169">Merge (default)</span></span><br/><span data-ttu-id="009b6-170">Feltöltés</span><span class="sxs-lookup"><span data-stu-id="009b6-170">Upload</span></span>| <span data-ttu-id="009b6-171">Nem</span><span class="sxs-lookup"><span data-stu-id="009b6-171">No</span></span> |
| <span data-ttu-id="009b6-172">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="009b6-172">WriteBatchSize</span></span> | <span data-ttu-id="009b6-173">Amikor a puffer mérete eléri writeBatchSize feltölti az adatok be az Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="009b6-173">Uploads data into the Azure Search index when the buffer size reaches writeBatchSize.</span></span> <span data-ttu-id="009b6-174">Tekintse meg a [WriteBatchSize tulajdonság](#writebatchsize-property) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="009b6-174">See the [WriteBatchSize property](#writebatchsize-property) for details.</span></span> | <span data-ttu-id="009b6-175">1-1 000.</span><span class="sxs-lookup"><span data-stu-id="009b6-175">1 to 1,000.</span></span> <span data-ttu-id="009b6-176">Alapértelmezett érték 1000.</span><span class="sxs-lookup"><span data-stu-id="009b6-176">Default value is 1000.</span></span> | <span data-ttu-id="009b6-177">Nem</span><span class="sxs-lookup"><span data-stu-id="009b6-177">No</span></span> |

### <a name="writebehavior-property"></a><span data-ttu-id="009b6-178">WriteBehavior tulajdonság</span><span class="sxs-lookup"><span data-stu-id="009b6-178">WriteBehavior property</span></span>
<span data-ttu-id="009b6-179">AzureSearchSink upserts adatok írásakor.</span><span class="sxs-lookup"><span data-stu-id="009b6-179">AzureSearchSink upserts when writing data.</span></span> <span data-ttu-id="009b6-180">Ez azt jelenti történő írásakor egy dokumentumot, ha a dokumentum kulcs már létezik az Azure Search-index, az Azure Search frissíti a meglévő dokumentumról, hanem egy ütközés Kivétel kiváltása.</span><span class="sxs-lookup"><span data-stu-id="009b6-180">In other words, when writing a document, if the document key already exists in the Azure Search index, Azure Search updates the existing document rather than throwing a conflict exception.</span></span>

<span data-ttu-id="009b6-181">A AzureSearchSink az alábbi két upsert viselkedésmódok biztosít a (AzureSearch SDK használatával):</span><span class="sxs-lookup"><span data-stu-id="009b6-181">The AzureSearchSink provides the following two upsert behaviors (by using AzureSearch SDK):</span></span>

- <span data-ttu-id="009b6-182">**Egyesítési**: kombinálhatja az összes oszlop az új dokumentum a meglévővel.</span><span class="sxs-lookup"><span data-stu-id="009b6-182">**Merge**: combine all the columns in the new document with the existing one.</span></span> <span data-ttu-id="009b6-183">Az oszlop, null értéket ad meg az új dokumentum a meglévő ütemezés értékét megőrződik.</span><span class="sxs-lookup"><span data-stu-id="009b6-183">For columns with null value in the new document, the value in the existing one is preserved.</span></span>
- <span data-ttu-id="009b6-184">**Töltse fel**: az új dokumentum lecseréli a meglévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="009b6-184">**Upload**: The new document replaces the existing one.</span></span> <span data-ttu-id="009b6-185">Nincs megadva az új dokumentum oszlopokhoz a beállítás értéke be NULL értékre van-e egy nem null értéket a meglévő dokumentum vagy sem.</span><span class="sxs-lookup"><span data-stu-id="009b6-185">For columns not specified in the new document, the value is set to null whether there is a non-null value in the existing document or not.</span></span>

<span data-ttu-id="009b6-186">Az alapértelmezett viselkedés **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="009b6-186">The default behavior is **Merge**.</span></span>

### <a name="writebatchsize-property"></a><span data-ttu-id="009b6-187">WriteBatchSize tulajdonság</span><span class="sxs-lookup"><span data-stu-id="009b6-187">WriteBatchSize Property</span></span>
<span data-ttu-id="009b6-188">Az Azure Search szolgáltatás egy kötegelt dokumentumok írása támogatja.</span><span class="sxs-lookup"><span data-stu-id="009b6-188">Azure Search service supports writing documents as a batch.</span></span> <span data-ttu-id="009b6-189">A kötegelt 1-1 000 műveletek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="009b6-189">A batch can contain 1 to 1,000 Actions.</span></span> <span data-ttu-id="009b6-190">Egy műveletet a feltöltési/egyesítési művelet egy dokumentum kezeli.</span><span class="sxs-lookup"><span data-stu-id="009b6-190">An action handles one document to perform the upload/merge operation.</span></span>

### <a name="data-type-support"></a><span data-ttu-id="009b6-191">Adattípus-támogatás</span><span class="sxs-lookup"><span data-stu-id="009b6-191">Data type support</span></span>
<span data-ttu-id="009b6-192">Az alábbi táblázat felsorolja, hogy az Azure Search adattípus támogatott-e, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="009b6-192">The following table specifies whether an Azure Search data type is supported or not.</span></span>

| <span data-ttu-id="009b6-193">Az Azure Search-adattípus</span><span class="sxs-lookup"><span data-stu-id="009b6-193">Azure Search data type</span></span> | <span data-ttu-id="009b6-194">Az Azure Search fogadó támogatott</span><span class="sxs-lookup"><span data-stu-id="009b6-194">Supported in Azure Search Sink</span></span> |
| ---------------------- | ------------------------------ |
| <span data-ttu-id="009b6-195">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="009b6-195">String</span></span> | <span data-ttu-id="009b6-196">I</span><span class="sxs-lookup"><span data-stu-id="009b6-196">Y</span></span> |
| <span data-ttu-id="009b6-197">Int32</span><span class="sxs-lookup"><span data-stu-id="009b6-197">Int32</span></span> | <span data-ttu-id="009b6-198">I</span><span class="sxs-lookup"><span data-stu-id="009b6-198">Y</span></span> |
| <span data-ttu-id="009b6-199">Int64</span><span class="sxs-lookup"><span data-stu-id="009b6-199">Int64</span></span> | <span data-ttu-id="009b6-200">I</span><span class="sxs-lookup"><span data-stu-id="009b6-200">Y</span></span> |
| <span data-ttu-id="009b6-201">Dupla</span><span class="sxs-lookup"><span data-stu-id="009b6-201">Double</span></span> | <span data-ttu-id="009b6-202">I</span><span class="sxs-lookup"><span data-stu-id="009b6-202">Y</span></span> |
| <span data-ttu-id="009b6-203">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="009b6-203">Boolean</span></span> | <span data-ttu-id="009b6-204">I</span><span class="sxs-lookup"><span data-stu-id="009b6-204">Y</span></span> |
| <span data-ttu-id="009b6-205">DataTimeOffset</span><span class="sxs-lookup"><span data-stu-id="009b6-205">DataTimeOffset</span></span> | <span data-ttu-id="009b6-206">I</span><span class="sxs-lookup"><span data-stu-id="009b6-206">Y</span></span> |
| <span data-ttu-id="009b6-207">Karakterlánc-tömbben</span><span class="sxs-lookup"><span data-stu-id="009b6-207">String Array</span></span> | <span data-ttu-id="009b6-208">N</span><span class="sxs-lookup"><span data-stu-id="009b6-208">N</span></span> |
| <span data-ttu-id="009b6-209">GeographyPoint</span><span class="sxs-lookup"><span data-stu-id="009b6-209">GeographyPoint</span></span> | <span data-ttu-id="009b6-210">N</span><span class="sxs-lookup"><span data-stu-id="009b6-210">N</span></span> |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a><span data-ttu-id="009b6-211">JSON-példa: adatok másolása az helyszíni SQL Server az Azure Search-index</span><span class="sxs-lookup"><span data-stu-id="009b6-211">JSON example: Copy data from on-premises SQL Server to Azure Search index</span></span>

<span data-ttu-id="009b6-212">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="009b6-212">The following sample shows:</span></span>

1.  <span data-ttu-id="009b6-213">A társított szolgáltatás típusa [AzureSearch](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="009b6-213">A linked service of type [AzureSearch](#linked-service-properties).</span></span>
2.  <span data-ttu-id="009b6-214">A társított szolgáltatás típusa [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="009b6-214">A linked service of type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span></span>
3.  <span data-ttu-id="009b6-215">Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="009b6-215">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
4.  <span data-ttu-id="009b6-216">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSearchIndex](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="009b6-216">An output [dataset](data-factory-create-datasets.md) of type [AzureSearchIndex](#dataset-properties).</span></span>
4.  <span data-ttu-id="009b6-217">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) és [AzureSearchIndexSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="009b6-217">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) and [AzureSearchIndexSink](#copy-activity-properties).</span></span>

<span data-ttu-id="009b6-218">A minta másol idősorozat adatokat a helyszíni SQL Server adatbázis az Azure Search-index óránként.</span><span class="sxs-lookup"><span data-stu-id="009b6-218">The sample copies time-series data from an on-premises SQL Server database to an Azure Search index hourly.</span></span> <span data-ttu-id="009b6-219">Ez a minta használt JSON-tulajdonságok a mintákat a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="009b6-219">The JSON properties used in this sample are described in sections following the samples.</span></span>

<span data-ttu-id="009b6-220">Első lépésként a telepítő az adatkezelési átjáró a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="009b6-220">As a first step, setup the data management gateway on your on-premises machine.</span></span> <span data-ttu-id="009b6-221">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="009b6-221">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="009b6-222">**Az Azure Search kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="009b6-222">**Azure Search linked service:**</span></span>

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

<span data-ttu-id="009b6-223">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="009b6-223">**SQL Server linked service**</span></span>

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

<span data-ttu-id="009b6-224">**SQL Server bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="009b6-224">**SQL Server input dataset**</span></span>

<span data-ttu-id="009b6-225">A minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="009b6-225">The sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="009b6-226">Lekérheti az egyetlen adatkészlet ugyanazon adatbázis több tábla keresztül, de egyetlen tábla a dataset tableName typeProperty kell használni.</span><span class="sxs-lookup"><span data-stu-id="009b6-226">You can query over multiple tables within the same database using a single dataset, but a single table must be used for the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="009b6-227">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="009b6-227">Setting “external”: ”true” informs Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="009b6-228">**Az Azure Search kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="009b6-228">**Azure Search output dataset:**</span></span>

<span data-ttu-id="009b6-229">A minta másolja az adatokat az Azure Search-index nevű **termékek**.</span><span class="sxs-lookup"><span data-stu-id="009b6-229">The sample copies data to an Azure Search index named **products**.</span></span> <span data-ttu-id="009b6-230">Adat-előállító nem hoz létre az indexet.</span><span class="sxs-lookup"><span data-stu-id="009b6-230">Data Factory does not create the index.</span></span> <span data-ttu-id="009b6-231">A minta teszteléséhez index létrehozása ezen a néven.</span><span class="sxs-lookup"><span data-stu-id="009b6-231">To test the sample, create an index with this name.</span></span> <span data-ttu-id="009b6-232">Hozzon létre az Azure Search-index a azonos számú oszlopot, mint a bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="009b6-232">Create the Azure Search index with the same number of columns as in the input dataset.</span></span> <span data-ttu-id="009b6-233">Új bejegyzések kerülnek az Azure Search-index óránként.</span><span class="sxs-lookup"><span data-stu-id="009b6-233">New entries are added to the Azure Search index every hour.</span></span>

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

<span data-ttu-id="009b6-234">**Másolási tevékenység során a folyamat az SQL-forrás és fogadó Azure Search-Index:**</span><span class="sxs-lookup"><span data-stu-id="009b6-234">**Copy activity in a pipeline with SQL source and Azure Search Index sink:**</span></span>

<span data-ttu-id="009b6-235">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="009b6-235">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="009b6-236">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlSource** és **fogadó** típusúra **AzureSearchIndexSink**.</span><span class="sxs-lookup"><span data-stu-id="009b6-236">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **AzureSearchIndexSink**.</span></span> <span data-ttu-id="009b6-237">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="009b6-237">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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

<span data-ttu-id="009b6-238">Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="009b6-238">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="009b6-239">Az alábbi JSON kódrészletben láthatja a másolási tevékenység során a szükséges módosítást `typeProperties` példaként.</span><span class="sxs-lookup"><span data-stu-id="009b6-239">The following JSON snippet shows the change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="009b6-240">Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.</span><span class="sxs-lookup"><span data-stu-id="009b6-240">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

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


## <a name="copy-from-a-cloud-source"></a><span data-ttu-id="009b6-241">A felhő forrás másolása</span><span class="sxs-lookup"><span data-stu-id="009b6-241">Copy from a cloud source</span></span>
<span data-ttu-id="009b6-242">Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="009b6-242">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="009b6-243">Az alábbi JSON kódrészletben láthatja a másolási tevékenység során a szükséges módosítást `typeProperties` példaként.</span><span class="sxs-lookup"><span data-stu-id="009b6-243">The following JSON snippet shows the change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="009b6-244">Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.</span><span class="sxs-lookup"><span data-stu-id="009b6-244">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

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

<span data-ttu-id="009b6-245">A másolási tevékenység definíciójának fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="009b6-245">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="009b6-246">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="009b6-246">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="009b6-247">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="009b6-247">Performance and tuning</span></span>  
<span data-ttu-id="009b6-248">Tekintse meg a [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) adatátvitelt jelölik a (másolási tevékenység) és a különböző módokon azt optimalizálása hatása teljesítmény kulcsfontosságú szerepet játszik tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="009b6-248">See the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="009b6-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="009b6-249">Next steps</span></span>
<span data-ttu-id="009b6-250">Lásd az alábbi cikkeket:</span><span class="sxs-lookup"><span data-stu-id="009b6-250">See the following articles:</span></span>

* <span data-ttu-id="009b6-251">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="009b6-251">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
