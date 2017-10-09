---
title: "Azure Data Factory használatával webes tábla aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan egy tábla egy webes toomove adatait lapon Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e52216305583ebbe71ed896522f361bb22f01278
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="77ebb-103">Adatok áthelyezése az Azure Data Factory használatával webes tábla forrásból</span><span class="sxs-lookup"><span data-stu-id="77ebb-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="77ebb-104">Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok egy táblából egy weblap tooa támogatott fogadó adattár.</span><span class="sxs-lookup"><span data-stu-id="77ebb-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from a table in a Web page tooa supported sink data store.</span></span> <span data-ttu-id="77ebb-105">Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="77ebb-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="77ebb-106">Adat-előállító jelenleg csak egy webes tábla tooother adatok áthelyezése adatait tárolja, de nem az adatok áthelyezése más adatok tooa webes tábla cél tárolja.</span><span class="sxs-lookup"><span data-stu-id="77ebb-106">Data factory currently supports only moving data from a Web table tooother data stores, but not moving data from other data stores tooa Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77ebb-107">A webalkalmazás-összekötőjének jelenleg csak kibontása tábla tartalma HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="77ebb-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="77ebb-108">a HTTP/s végpont tooretrieve adatait használja [HTTP összekötő](data-factory-http-connector.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="77ebb-108">tooretrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="77ebb-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="77ebb-109">Getting started</span></span>
<span data-ttu-id="77ebb-110">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="77ebb-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="77ebb-111">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-111">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="77ebb-112">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="77ebb-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="77ebb-113">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-113">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="77ebb-114">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="77ebb-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="77ebb-115">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="77ebb-115">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="77ebb-116">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="77ebb-116">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="77ebb-117">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="77ebb-117">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="77ebb-118">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="77ebb-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="77ebb-119">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="77ebb-119">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="77ebb-120">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="77ebb-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="77ebb-121">Az adat-előállító entitások, amelyek egy webes tábla használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az webes tábla tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="77ebb-121">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a web table, see [JSON example: Copy data from Web table tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="77ebb-122">a következő szakaszok hello használt toodefine adat-előállító entitások adott tooa webes tábla JSON-tulajdonághoz részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="77ebb-122">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="77ebb-123">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="77ebb-123">Linked service properties</span></span>
<span data-ttu-id="77ebb-124">a következő táblázat hello biztosít JSON elemek adott tooWeb kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="77ebb-124">hello following table provides description for JSON elements specific tooWeb linked service.</span></span>

| <span data-ttu-id="77ebb-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77ebb-125">Property</span></span> | <span data-ttu-id="77ebb-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="77ebb-126">Description</span></span> | <span data-ttu-id="77ebb-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77ebb-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77ebb-128">type</span><span class="sxs-lookup"><span data-stu-id="77ebb-128">type</span></span> |<span data-ttu-id="77ebb-129">hello type tulajdonságot kell beállítani: **webes**</span><span class="sxs-lookup"><span data-stu-id="77ebb-129">hello type property must be set to: **Web**</span></span> |<span data-ttu-id="77ebb-130">Igen</span><span class="sxs-lookup"><span data-stu-id="77ebb-130">Yes</span></span> |
| <span data-ttu-id="77ebb-131">URL-cím</span><span class="sxs-lookup"><span data-stu-id="77ebb-131">Url</span></span> |<span data-ttu-id="77ebb-132">URL-cím toohello webes forrás</span><span class="sxs-lookup"><span data-stu-id="77ebb-132">URL toohello Web source</span></span> |<span data-ttu-id="77ebb-133">Igen</span><span class="sxs-lookup"><span data-stu-id="77ebb-133">Yes</span></span> |
| <span data-ttu-id="77ebb-134">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="77ebb-134">authenticationType</span></span> |<span data-ttu-id="77ebb-135">Névtelen.</span><span class="sxs-lookup"><span data-stu-id="77ebb-135">Anonymous.</span></span> |<span data-ttu-id="77ebb-136">Igen</span><span class="sxs-lookup"><span data-stu-id="77ebb-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="77ebb-137">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="77ebb-137">Using Anonymous authentication</span></span>

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="77ebb-138">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77ebb-138">Dataset properties</span></span>
<span data-ttu-id="77ebb-139">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="77ebb-139">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="77ebb-140">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="77ebb-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="77ebb-141">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="77ebb-141">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="77ebb-142">hello typeProperties szakasz típusú adatkészlet **Webtábla** rendelkezik hello következő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77ebb-142">hello typeProperties section for dataset of type **WebTable** has hello following properties</span></span>

| <span data-ttu-id="77ebb-143">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77ebb-143">Property</span></span> | <span data-ttu-id="77ebb-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="77ebb-144">Description</span></span> | <span data-ttu-id="77ebb-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77ebb-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="77ebb-146">type</span><span class="sxs-lookup"><span data-stu-id="77ebb-146">type</span></span> |<span data-ttu-id="77ebb-147">Hello dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="77ebb-147">type of hello dataset.</span></span> <span data-ttu-id="77ebb-148">be kell állítani túl**Webtábla**</span><span class="sxs-lookup"><span data-stu-id="77ebb-148">must be set too**WebTable**</span></span> |<span data-ttu-id="77ebb-149">Igen</span><span class="sxs-lookup"><span data-stu-id="77ebb-149">Yes</span></span> |
| <span data-ttu-id="77ebb-150">Elérési út</span><span class="sxs-lookup"><span data-stu-id="77ebb-150">path</span></span> |<span data-ttu-id="77ebb-151">Relatív URL-cím toohello erőforrás hello táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="77ebb-151">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="77ebb-152">Nem.</span><span class="sxs-lookup"><span data-stu-id="77ebb-152">No.</span></span> <span data-ttu-id="77ebb-153">Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál.</span><span class="sxs-lookup"><span data-stu-id="77ebb-153">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="77ebb-154">Index</span><span class="sxs-lookup"><span data-stu-id="77ebb-154">index</span></span> |<span data-ttu-id="77ebb-155">hello tábla hello erőforrás hello indexe.</span><span class="sxs-lookup"><span data-stu-id="77ebb-155">hello index of hello table in hello resource.</span></span> <span data-ttu-id="77ebb-156">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="77ebb-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="77ebb-157">Igen</span><span class="sxs-lookup"><span data-stu-id="77ebb-157">Yes</span></span> |

<span data-ttu-id="77ebb-158">**Példa**</span><span class="sxs-lookup"><span data-stu-id="77ebb-158">**Example:**</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="77ebb-159">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77ebb-159">Copy activity properties</span></span>
<span data-ttu-id="77ebb-160">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="77ebb-160">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="77ebb-161">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="77ebb-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="77ebb-162">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="77ebb-162">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="77ebb-163">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="77ebb-163">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="77ebb-164">Ha a másolási tevékenység hello forrás jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="77ebb-164">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-tooazure-blob"></a><span data-ttu-id="77ebb-165">JSON-példa: adatok másolása az webes tábla tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="77ebb-165">JSON example: Copy data from Web table tooAzure Blob</span></span>
<span data-ttu-id="77ebb-166">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="77ebb-166">hello following sample shows:</span></span>

1. <span data-ttu-id="77ebb-167">A társított szolgáltatás típusa [webes](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="77ebb-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="77ebb-168">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="77ebb-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="77ebb-169">Bemeneti [dataset](data-factory-create-datasets.md) típusú [Webtábla](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="77ebb-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="77ebb-170">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="77ebb-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="77ebb-171">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [WebSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="77ebb-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="77ebb-172">hello minta másol adatokat egy webes tábla tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="77ebb-172">hello sample copies data from a Web table tooan Azure blob every hour.</span></span> <span data-ttu-id="77ebb-173">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="77ebb-173">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="77ebb-174">a következő minta hello jeleníti meg, hogyan toocopy adatait egy webes tábla tooan Azure blob-e.</span><span class="sxs-lookup"><span data-stu-id="77ebb-174">hello following sample shows how toocopy data from a Web table tooan Azure blob.</span></span> <span data-ttu-id="77ebb-175">Azonban adatok átmásolhatók, közvetlenül a hello tooany fogadók esetében a megadott hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="77ebb-175">However, data can be copied directly tooany of hello sinks stated in hello [Data Movement Activities](data-factory-data-movement-activities.md) article by using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="77ebb-176">**Webes társított szolgáltatás** ebben a példában használt webes hello társított szolgáltatás névtelen hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="77ebb-176">**Web linked service** This example uses hello Web linked service with anonymous authentication.</span></span> <span data-ttu-id="77ebb-177">Lásd: [webes társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="77ebb-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="77ebb-178">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="77ebb-178">**Azure Storage linked service**</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="77ebb-179">**Webtábla bemeneti adatkészlet** beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállító, és nem hozzák hello tevékenységet adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="77ebb-179">**WebTable input dataset** Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="77ebb-180">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="77ebb-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span>  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


<span data-ttu-id="77ebb-181">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="77ebb-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="77ebb-182">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="77ebb-182">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



<span data-ttu-id="77ebb-183">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="77ebb-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="77ebb-184">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="77ebb-184">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="77ebb-185">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**WebSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-185">In hello pipeline JSON definition, hello **source** type is set too**WebSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="77ebb-186">Lásd: [WebSource típustulajdonságokat](#copy-activity-type-properties) hello WebSource által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="77ebb-186">See [WebSource type properties](#copy-activity-type-properties) for hello list of properties supported by hello WebSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="77ebb-187">Egy tábla indexének lekérése egy HTML-weblap</span><span class="sxs-lookup"><span data-stu-id="77ebb-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="77ebb-188">Indítsa el **Excel 2016** és toohello **adatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="77ebb-188">Launch **Excel 2016** and switch toohello **Data** tab.</span></span>  
2. <span data-ttu-id="77ebb-189">Kattintson a **új lekérdezés** hello eszköztár pont túl**egyéb forrásokból származó** kattintson **a webes**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-189">Click **New Query** on hello toolbar, point too**From Other Sources** and click **From Web**.</span></span>

    ![A Power Query menü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="77ebb-191">A hello **a webes** párbeszédpanelen adja meg a **URL-cím** használható a társított szolgáltatás JSON (például: https://en.wikipedia.org/wiki/) elérési út lehet megadni hello adatkészlet együtt (például: AFI % 27s_100_Years... 100_Movies), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-191">In hello **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for hello dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Webes párbeszédpanelről](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="77ebb-193">Ebben a példában használt URL-cím: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="77ebb-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="77ebb-194">Ha látja **hozzáférés webes tartalom** párbeszédpanel megnyitásához, jelölje be hello jobb **URL-cím**, **hitelesítési**, és kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="77ebb-194">If you see **Access Web content** dialog box, select hello right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Webes tartalom párbeszédpanel](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="77ebb-196">Kattintson egy **tábla** hello fa nézet toosee tartalom hello táblából elemét, majd **szerkesztése** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="77ebb-196">Click a **table** item in hello tree view toosee content from hello table and then click **Edit** button at hello bottom.</span></span>  

   ![Navigator párbeszédpanel](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="77ebb-198">A hello **Lekérdezésszerkesztő** ablak, kattintson a **speciális szerkesztő** hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="77ebb-198">In hello **Query Editor** window, click **Advanced Editor** button on hello toolbar.</span></span>

    ![Speciális szerkesztő gomb](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="77ebb-200">A hello speciális szerkesztő párbeszédpanel hello szám következő túl "Forrás" hello index.</span><span class="sxs-lookup"><span data-stu-id="77ebb-200">In hello Advanced Editor dialog box, hello number next too"Source" is hello index.</span></span>

    ![Speciális szerkesztő - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="77ebb-202">Ha az Excel 2013 használ, [Microsoft Power Query az Excel programhoz](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span><span class="sxs-lookup"><span data-stu-id="77ebb-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span></span> <span data-ttu-id="77ebb-203">Lásd: [Connect tooa weblap](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="77ebb-203">See [Connect tooa web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="77ebb-204">hello lépések hasonlóak használata [Microsoft Power BI Desktop az](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="77ebb-204">hello steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="77ebb-205">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="77ebb-205">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="77ebb-206">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="77ebb-206">Performance and Tuning</span></span>
<span data-ttu-id="77ebb-207">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="77ebb-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
