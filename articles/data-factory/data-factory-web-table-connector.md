---
title: "Adatok áthelyezése az Azure Data Factory használatával webes táblából |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése egy táblából egy Azure Data Factory használatával weblapon."
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
ms.openlocfilehash: 9e006bc7289fa0239f1650ac6ad43dd159e3c7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="80a33-103">Adatok áthelyezése az Azure Data Factory használatával webes tábla forrásból</span><span class="sxs-lookup"><span data-stu-id="80a33-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="80a33-104">Ez a cikk ismerteti a másolási tevékenység használata az Azure Data Factory tárolt adatok mozgatása egy tábla egy weblapon támogatott fogadó adattárat.</span><span class="sxs-lookup"><span data-stu-id="80a33-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from a table in a Web page to a supported sink data store.</span></span> <span data-ttu-id="80a33-105">Ez a cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja az adatmozgás általános áttekintést a másolási tevékenység és a támogatott adatforrások/mosdók adattárolókhoz listáját.</span><span class="sxs-lookup"><span data-stu-id="80a33-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="80a33-106">Adat-előállító jelenleg csak áthelyezése egy webes tábla adatai más adattárolókhoz, de egy webes tábla célra nem adatok áthelyezését más adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="80a33-106">Data factory currently supports only moving data from a Web table to other data stores, but not moving data from other data stores to a Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80a33-107">A webalkalmazás-összekötőjének jelenleg csak kibontása tábla tartalma HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="80a33-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="80a33-108">A HTTP/s végpont adatok lekéréséhez használja [HTTP összekötő](data-factory-http-connector.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="80a33-108">To retrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="80a33-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="80a33-109">Getting started</span></span>
<span data-ttu-id="80a33-110">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="80a33-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="80a33-111">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="80a33-111">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="80a33-112">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="80a33-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="80a33-113">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="80a33-113">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="80a33-114">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="80a33-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="80a33-115">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="80a33-115">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="80a33-116">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="80a33-116">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="80a33-117">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="80a33-117">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="80a33-118">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="80a33-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="80a33-119">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="80a33-119">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="80a33-120">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="80a33-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="80a33-121">Adatok másolása egy webes tábla használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az webes tábla az Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="80a33-121">For a sample with JSON definitions for Data Factory entities that are used to copy data from a web table, see [JSON example: Copy data from Web table to Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="80a33-122">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások webes táblához JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="80a33-122">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="80a33-123">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="80a33-123">Linked service properties</span></span>
<span data-ttu-id="80a33-124">A következő táblázat a JSON-elemek szerepelnek jellemző csatolt webszolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="80a33-124">The following table provides description for JSON elements specific to Web linked service.</span></span>

| <span data-ttu-id="80a33-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="80a33-125">Property</span></span> | <span data-ttu-id="80a33-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="80a33-126">Description</span></span> | <span data-ttu-id="80a33-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="80a33-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80a33-128">type</span><span class="sxs-lookup"><span data-stu-id="80a33-128">type</span></span> |<span data-ttu-id="80a33-129">A type tulajdonságot kell beállítani: **webes**</span><span class="sxs-lookup"><span data-stu-id="80a33-129">The type property must be set to: **Web**</span></span> |<span data-ttu-id="80a33-130">Igen</span><span class="sxs-lookup"><span data-stu-id="80a33-130">Yes</span></span> |
| <span data-ttu-id="80a33-131">URL-cím</span><span class="sxs-lookup"><span data-stu-id="80a33-131">Url</span></span> |<span data-ttu-id="80a33-132">A webes forrás URL-címe</span><span class="sxs-lookup"><span data-stu-id="80a33-132">URL to the Web source</span></span> |<span data-ttu-id="80a33-133">Igen</span><span class="sxs-lookup"><span data-stu-id="80a33-133">Yes</span></span> |
| <span data-ttu-id="80a33-134">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="80a33-134">authenticationType</span></span> |<span data-ttu-id="80a33-135">Névtelen.</span><span class="sxs-lookup"><span data-stu-id="80a33-135">Anonymous.</span></span> |<span data-ttu-id="80a33-136">Igen</span><span class="sxs-lookup"><span data-stu-id="80a33-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="80a33-137">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="80a33-137">Using Anonymous authentication</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="80a33-138">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="80a33-138">Dataset properties</span></span>
<span data-ttu-id="80a33-139">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="80a33-139">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="80a33-140">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="80a33-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="80a33-141">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="80a33-141">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="80a33-142">A typeProperties szakasz típusú adatkészlet **Webtábla** tulajdonságai a következők</span><span class="sxs-lookup"><span data-stu-id="80a33-142">The typeProperties section for dataset of type **WebTable** has the following properties</span></span>

| <span data-ttu-id="80a33-143">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="80a33-143">Property</span></span> | <span data-ttu-id="80a33-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="80a33-144">Description</span></span> | <span data-ttu-id="80a33-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="80a33-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="80a33-146">type</span><span class="sxs-lookup"><span data-stu-id="80a33-146">type</span></span> |<span data-ttu-id="80a33-147">A dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="80a33-147">type of the dataset.</span></span> <span data-ttu-id="80a33-148">meg kell **Webtábla**</span><span class="sxs-lookup"><span data-stu-id="80a33-148">must be set to **WebTable**</span></span> |<span data-ttu-id="80a33-149">Igen</span><span class="sxs-lookup"><span data-stu-id="80a33-149">Yes</span></span> |
| <span data-ttu-id="80a33-150">Elérési út</span><span class="sxs-lookup"><span data-stu-id="80a33-150">path</span></span> |<span data-ttu-id="80a33-151">Az erőforrás, amely tartalmazza a tábla relatív URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="80a33-151">A relative URL to the resource that contains the table.</span></span> |<span data-ttu-id="80a33-152">Nem.</span><span class="sxs-lookup"><span data-stu-id="80a33-152">No.</span></span> <span data-ttu-id="80a33-153">Ha nincs megadva, csak a megadott URL-cím a társított szolgáltatás definíciójának használja.</span><span class="sxs-lookup"><span data-stu-id="80a33-153">When path is not specified, only the URL specified in the linked service definition is used.</span></span> |
| <span data-ttu-id="80a33-154">Index</span><span class="sxs-lookup"><span data-stu-id="80a33-154">index</span></span> |<span data-ttu-id="80a33-155">Annak az erőforrás a táblának az indexe.</span><span class="sxs-lookup"><span data-stu-id="80a33-155">The index of the table in the resource.</span></span> <span data-ttu-id="80a33-156">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit egy tábla indexének első HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="80a33-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span> |<span data-ttu-id="80a33-157">Igen</span><span class="sxs-lookup"><span data-stu-id="80a33-157">Yes</span></span> |

<span data-ttu-id="80a33-158">**Példa**</span><span class="sxs-lookup"><span data-stu-id="80a33-158">**Example:**</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="80a33-159">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="80a33-159">Copy activity properties</span></span>
<span data-ttu-id="80a33-160">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="80a33-160">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="80a33-161">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="80a33-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="80a33-162">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="80a33-162">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="80a33-163">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="80a33-163">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="80a33-164">Ha a forrás, a másolási tevékenység jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="80a33-164">Currently, when the source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a><span data-ttu-id="80a33-165">JSON-példa: adatok másolása az webes tábla az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="80a33-165">JSON example: Copy data from Web table to Azure Blob</span></span>
<span data-ttu-id="80a33-166">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="80a33-166">The following sample shows:</span></span>

1. <span data-ttu-id="80a33-167">A társított szolgáltatás típusa [webes](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="80a33-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="80a33-168">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="80a33-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="80a33-169">Bemeneti [dataset](data-factory-create-datasets.md) típusú [Webtábla](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="80a33-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="80a33-170">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="80a33-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="80a33-171">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [WebSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="80a33-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="80a33-172">A minta másol adatokat egy webes tábla egy Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="80a33-172">The sample copies data from a Web table to an Azure blob every hour.</span></span> <span data-ttu-id="80a33-173">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="80a33-173">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="80a33-174">A következő példa bemutatja, hogyan adatok másolása az Azure blob egy webes táblából.</span><span class="sxs-lookup"><span data-stu-id="80a33-174">The following sample shows how to copy data from a Web table to an Azure blob.</span></span> <span data-ttu-id="80a33-175">Azonban adatok átmásolhatók közvetlenül a megadott mosdók bármelyikét a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="80a33-175">However, data can be copied directly to any of the sinks stated in the [Data Movement Activities](data-factory-data-movement-activities.md) article by using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="80a33-176">**Webes társított szolgáltatás** a példában a kapcsolódó webszolgáltatás névtelen hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="80a33-176">**Web linked service** This example uses the Web linked service with anonymous authentication.</span></span> <span data-ttu-id="80a33-177">Lásd: [webes társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="80a33-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="80a33-178">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="80a33-178">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="80a33-179">**Webtábla bemeneti adatkészlet** beállítás **külső** való **igaz** tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="80a33-179">**WebTable input dataset** Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="80a33-180">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit egy tábla indexének első HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="80a33-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span>  
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


<span data-ttu-id="80a33-181">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="80a33-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="80a33-182">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="80a33-182">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

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



<span data-ttu-id="80a33-183">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="80a33-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="80a33-184">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="80a33-184">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="80a33-185">Az adatcsatorna JSON-definícióból a **forrás** típusúra **WebSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="80a33-185">In the pipeline JSON definition, the **source** type is set to **WebSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="80a33-186">Lásd: [WebSource típustulajdonságokat](#copy-activity-type-properties) a WebSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="80a33-186">See [WebSource type properties](#copy-activity-type-properties) for the list of properties supported by the WebSource.</span></span>

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
        "description": "Copy from a Web table to an Azure blob",
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

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="80a33-187">Egy tábla indexének lekérése egy HTML-weblap</span><span class="sxs-lookup"><span data-stu-id="80a33-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="80a33-188">Indítsa el **Excel 2016** majd átváltása a **adatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="80a33-188">Launch **Excel 2016** and switch to the **Data** tab.</span></span>  
2. <span data-ttu-id="80a33-189">Kattintson a **új lekérdezés** eszköztárán mutasson **egyéb forrásokból származó** kattintson **a webes**.</span><span class="sxs-lookup"><span data-stu-id="80a33-189">Click **New Query** on the toolbar, point to **From Other Sources** and click **From Web**.</span></span>

    ![A Power Query menü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="80a33-191">A a **a webes** párbeszédpanelen adja meg a **URL-cím** használható a társított szolgáltatás JSON (például: https://en.wikipedia.org/wiki/) elérési út lehet megadni az adatkészlet együtt (például: AFI % 27s_100_Years 100_Movies), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="80a33-191">In the **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for the dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Webes párbeszédpanelről](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="80a33-193">Ebben a példában használt URL-cím: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="80a33-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="80a33-194">Ha látja **hozzáférés webes tartalom** párbeszédpanelen jelölje ki a jobb **URL-cím**, **hitelesítési**, kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="80a33-194">If you see **Access Web content** dialog box, select the right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Webes tartalom párbeszédpanel](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="80a33-196">Kattintson egy **tábla** tekintse meg a tábla tartalmát, és kattintson a fanézetben elem **szerkesztése** panel alján.</span><span class="sxs-lookup"><span data-stu-id="80a33-196">Click a **table** item in the tree view to see content from the table and then click **Edit** button at the bottom.</span></span>  

   ![Navigator párbeszédpanel](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="80a33-198">Az a **Lekérdezésszerkesztő** ablak, kattintson a **speciális szerkesztő** gomb az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="80a33-198">In the **Query Editor** window, click **Advanced Editor** button on the toolbar.</span></span>

    ![Speciális szerkesztő gomb](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="80a33-200">A speciális szerkesztése párbeszédpanelen az mellett látható "Forrás" érték az index.</span><span class="sxs-lookup"><span data-stu-id="80a33-200">In the Advanced Editor dialog box, the number next to "Source" is the index.</span></span>

    ![Speciális szerkesztő - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="80a33-202">Ha az Excel 2013 használ, [Microsoft Power Query az Excel programhoz](https://www.microsoft.com/download/details.aspx?id=39379) lekérni az index.</span><span class="sxs-lookup"><span data-stu-id="80a33-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) to get the index.</span></span> <span data-ttu-id="80a33-203">Lásd: [weblapon kapcsolódás](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="80a33-203">See [Connect to a web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="80a33-204">A lépések hasonlóak használata [Microsoft Power BI Desktop az](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="80a33-204">The steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="80a33-205">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="80a33-205">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="80a33-206">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="80a33-206">Performance and Tuning</span></span>
<span data-ttu-id="80a33-207">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="80a33-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
