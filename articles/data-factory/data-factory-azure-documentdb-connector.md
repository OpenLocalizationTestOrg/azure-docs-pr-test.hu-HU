---
title: az Azure Cosmos DB aaaMove adatok |} Microsoft Docs
description: "Megtudhatja, hogyan helyezi át az adatokat az Azure Data Factory használatához Azure Cosmos DB gyűjtemény"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="c9c47-103">Adatok tooand áthelyezése az Azure Data Factory használatához Azure Cosmos-Adatbázisból</span><span class="sxs-lookup"><span data-stu-id="c9c47-103">Move data tooand from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="c9c47-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure Cosmos DB (a DocumentDB API) és a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c9c47-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="c9c47-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="c9c47-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="c9c47-106">Bármely támogatott forráshierarchiából adatokat tooAzure Cosmos-adatbázis tárolásához, vagy az Azure Cosmos DB támogatott tooany fogadó adatok tárolására adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="c9c47-106">You can copy data from any supported source data store tooAzure Cosmos DB or from Azure Cosmos DB tooany supported sink data store.</span></span> <span data-ttu-id="c9c47-107">Adatforrások vagy mosdók hello másolási tevékenység által támogatott adattárolókhoz listáját lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="c9c47-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c9c47-108">Azure Cosmos DB összekötő csak a DocumentDB API támogatja.</span><span class="sxs-lookup"><span data-stu-id="c9c47-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="c9c47-109">az adatok toocopy-van/JSON-fájlokat vagy egy másik Cosmos DB gyűjteményhez, lásd: [Import/Export JSON-dokumentumok](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="c9c47-109">toocopy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="c9c47-110">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c9c47-110">Getting started</span></span>
<span data-ttu-id="c9c47-111">A másolási tevékenység, mely az adatok Azure Cosmos DB és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c9c47-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="c9c47-112">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="c9c47-112">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="c9c47-113">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c9c47-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="c9c47-114">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="c9c47-114">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c9c47-115">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="c9c47-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="c9c47-116">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="c9c47-116">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="c9c47-117">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-117">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="c9c47-118">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="c9c47-118">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="c9c47-119">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="c9c47-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="c9c47-120">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="c9c47-120">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="c9c47-121">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="c9c47-122">Minták használt toocopy adatok Cosmos DB az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c9c47-122">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="c9c47-123">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooCosmos DB részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="c9c47-123">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooCosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="c9c47-124">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c9c47-124">Linked service properties</span></span>
<span data-ttu-id="c9c47-125">a következő táblázat hello biztosít JSON-elemek adott tooAzure Cosmos DB kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="c9c47-125">hello following table provides description for JSON elements specific tooAzure Cosmos DB linked service.</span></span>

| <span data-ttu-id="c9c47-126">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="c9c47-126">**Property**</span></span> | <span data-ttu-id="c9c47-127">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="c9c47-127">**Description**</span></span> | <span data-ttu-id="c9c47-128">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="c9c47-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c9c47-129">type</span><span class="sxs-lookup"><span data-stu-id="c9c47-129">type</span></span> |<span data-ttu-id="c9c47-130">hello type tulajdonságot kell beállítani: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="c9c47-130">hello type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="c9c47-131">Igen</span><span class="sxs-lookup"><span data-stu-id="c9c47-131">Yes</span></span> |
| <span data-ttu-id="c9c47-132">connectionString</span><span class="sxs-lookup"><span data-stu-id="c9c47-132">connectionString</span></span> |<span data-ttu-id="c9c47-133">Adja meg a szükséges adatok tooconnect tooAzure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-133">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="c9c47-134">Igen</span><span class="sxs-lookup"><span data-stu-id="c9c47-134">Yes</span></span> |

<span data-ttu-id="c9c47-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="c9c47-135">Example:</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="c9c47-136">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c9c47-136">Dataset properties</span></span>
<span data-ttu-id="c9c47-137">A szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listájáért tekintse meg az toohello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c9c47-137">For a full list of sections & properties available for defining datasets please refer toohello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c9c47-138">Például a struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="c9c47-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="c9c47-139">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c9c47-139">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="c9c47-140">hello typeProperties szakasz hello adatkészlet típusú **DocumentDbCollection** hello alábbi tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c9c47-140">hello typeProperties section for hello dataset of type **DocumentDbCollection** has hello following properties.</span></span>

| <span data-ttu-id="c9c47-141">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="c9c47-141">**Property**</span></span> | <span data-ttu-id="c9c47-142">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="c9c47-142">**Description**</span></span> | <span data-ttu-id="c9c47-143">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="c9c47-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c9c47-144">CollectionName</span><span class="sxs-lookup"><span data-stu-id="c9c47-144">collectionName</span></span> |<span data-ttu-id="c9c47-145">Hello Cosmos DB dokumentumgyűjteményt neve.</span><span class="sxs-lookup"><span data-stu-id="c9c47-145">Name of hello Cosmos DB document collection.</span></span> |<span data-ttu-id="c9c47-146">Igen</span><span class="sxs-lookup"><span data-stu-id="c9c47-146">Yes</span></span> |

<span data-ttu-id="c9c47-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="c9c47-147">Example:</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a><span data-ttu-id="c9c47-148">Adat-előállító sémája</span><span class="sxs-lookup"><span data-stu-id="c9c47-148">Schema by Data Factory</span></span>
<span data-ttu-id="c9c47-149">Sémamentesadat-tárolókhoz, például az Azure Cosmos DB a Data Factory szolgáltatásnak hello kikövetkezteti hello séma a következő módokon hello egyikében:</span><span class="sxs-lookup"><span data-stu-id="c9c47-149">For schema-free data stores such as Azure Cosmos DB, hello Data Factory service infers hello schema in one of hello following ways:</span></span>  

1. <span data-ttu-id="c9c47-150">Ha megadja az adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság eleget tegyen hello séma szerint ez a struktúra.</span><span class="sxs-lookup"><span data-stu-id="c9c47-150">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="c9c47-151">Ebben az esetben ha egy sort tartalmaz egy olyan oszlop értékét, null értékű nyújtanak az.</span><span class="sxs-lookup"><span data-stu-id="c9c47-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="c9c47-152">Ha nem ad meg adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság hello séma kikövetkezteti hello adatok első sorának hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c9c47-152">If you do not specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="c9c47-153">Ebben az esetben ha hello első sor nem tartalmaz teljes séma hello, néhány oszlop nem érhető el a másolási művelet hello eredményét.</span><span class="sxs-lookup"><span data-stu-id="c9c47-153">In this case, if hello first row does not contain hello full schema, some columns will be missing in hello result of copy operation.</span></span>

<span data-ttu-id="c9c47-154">Sémamentes adatforrások hello célszerű ezért hello segítségével adatok szerkezete toospecify hello **struktúra** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c9c47-154">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="c9c47-155">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c9c47-155">Copy activity properties</span></span>
<span data-ttu-id="c9c47-156">A szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listájáért tekintse meg az toohello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c9c47-156">For a full list of sections & properties available for defining activities please refer toohello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c9c47-157">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c9c47-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="c9c47-158">hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="c9c47-158">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="c9c47-159">Tulajdonságok hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor eltérők lehetnek a tevékenységek minden típusának és források és mosdók hello típusától függően változik, a másolási tevékenység esetén.</span><span class="sxs-lookup"><span data-stu-id="c9c47-159">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type and in case of Copy activity they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="c9c47-160">Ha forrás típusa másolási tevékenység esetén **DocumentDbCollectionSource** hello a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="c9c47-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="c9c47-161">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="c9c47-161">**Property**</span></span> | <span data-ttu-id="c9c47-162">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="c9c47-162">**Description**</span></span> | <span data-ttu-id="c9c47-163">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="c9c47-163">**Allowed values**</span></span> | <span data-ttu-id="c9c47-164">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="c9c47-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c9c47-165">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="c9c47-165">query</span></span> |<span data-ttu-id="c9c47-166">Adja meg a hello tooread adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="c9c47-166">Specify hello query tooread data.</span></span> |<span data-ttu-id="c9c47-167">Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott.</span><span class="sxs-lookup"><span data-stu-id="c9c47-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="c9c47-168">Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="c9c47-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="c9c47-169">Nem</span><span class="sxs-lookup"><span data-stu-id="c9c47-169">No</span></span> <br/><br/><span data-ttu-id="c9c47-170">Ha nincs megadva, hello végrehajtott SQL-utasítást:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="c9c47-170">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="c9c47-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="c9c47-171">nestingSeparator</span></span> |<span data-ttu-id="c9c47-172">Speciális karakter tooindicate, hogy a dokumentum hello van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="c9c47-172">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="c9c47-173">Bármely karakter.</span><span class="sxs-lookup"><span data-stu-id="c9c47-173">Any character.</span></span> <br/><br/><span data-ttu-id="c9c47-174">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c9c47-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="c9c47-175">Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül nestingSeparator, amely "."</span><span class="sxs-lookup"><span data-stu-id="c9c47-175">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="c9c47-176">a fenti példák hello.</span><span class="sxs-lookup"><span data-stu-id="c9c47-176">in hello above examples.</span></span> <span data-ttu-id="c9c47-177">Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása.</span><span class="sxs-lookup"><span data-stu-id="c9c47-177">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="c9c47-178">Nem</span><span class="sxs-lookup"><span data-stu-id="c9c47-178">No</span></span> |

<span data-ttu-id="c9c47-179">**DocumentDbCollectionSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="c9c47-179">**DocumentDbCollectionSink** supports hello following properties:</span></span>

| <span data-ttu-id="c9c47-180">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="c9c47-180">**Property**</span></span> | <span data-ttu-id="c9c47-181">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="c9c47-181">**Description**</span></span> | <span data-ttu-id="c9c47-182">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="c9c47-182">**Allowed values**</span></span> | <span data-ttu-id="c9c47-183">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="c9c47-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c9c47-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="c9c47-184">nestingSeparator</span></span> |<span data-ttu-id="c9c47-185">A különleges karakterek hello forrás oszlop neve tooindicate, amely a beágyazott dokumentumok van szükség.</span><span class="sxs-lookup"><span data-stu-id="c9c47-185">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="c9c47-186">Például fent: `Name.First` hello kimeneti táblát hoz létre hello JSON struktúrában hello Cosmos DB dokumentumban a következő:</span><span class="sxs-lookup"><span data-stu-id="c9c47-186">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="c9c47-187">"Name": {</span><span class="sxs-lookup"><span data-stu-id="c9c47-187">"Name": {</span></span><br/>    <span data-ttu-id="c9c47-188">"Első": "John"</span><span class="sxs-lookup"><span data-stu-id="c9c47-188">"First": "John"</span></span><br/><span data-ttu-id="c9c47-189">},</span><span class="sxs-lookup"><span data-stu-id="c9c47-189">},</span></span> |<span data-ttu-id="c9c47-190">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="c9c47-190">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="c9c47-191">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="c9c47-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="c9c47-192">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="c9c47-192">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="c9c47-193">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="c9c47-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="c9c47-194">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="c9c47-194">writeBatchSize</span></span> |<span data-ttu-id="c9c47-195">A lekérdezések tooAzure Cosmos DB toocreate dokumentumok párhuzamos száma.</span><span class="sxs-lookup"><span data-stu-id="c9c47-195">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="c9c47-196">Hello teljesítmény úgy finomhangolhatja, és a Cosmos DB adatok másolásakor e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="c9c47-196">You can fine-tune hello performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="c9c47-197">A jobb teljesítmény számíthat, mivel a rendszer további kérelmeket párhuzamos tooCosmos DB elküldi a writeBatchSize növelésével.</span><span class="sxs-lookup"><span data-stu-id="c9c47-197">You can expect a better performance when you increase writeBatchSize because more parallel requests tooCosmos DB are sent.</span></span> <span data-ttu-id="c9c47-198">Azonban szüksége lesz, amelyek sávszélesség-szabályozás tooavoid is throw hello hibaüzenet: "Ez nagy lekérő".</span><span class="sxs-lookup"><span data-stu-id="c9c47-198">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="c9c47-199">Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek, használhat egy jobb gyűjtemény (pl. S3) toohave hello legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc).</span><span class="sxs-lookup"><span data-stu-id="c9c47-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="c9c47-200">Egész szám</span><span class="sxs-lookup"><span data-stu-id="c9c47-200">Integer</span></span> |<span data-ttu-id="c9c47-201">Nem (alapértelmezett: 5)</span><span class="sxs-lookup"><span data-stu-id="c9c47-201">No (default: 5)</span></span> |
| <span data-ttu-id="c9c47-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="c9c47-202">writeBatchTimeout</span></span> |<span data-ttu-id="c9c47-203">Várnia kell az hello művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="c9c47-203">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="c9c47-204">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c9c47-204">timespan</span></span><br/><br/> <span data-ttu-id="c9c47-205">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="c9c47-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="c9c47-206">Nem</span><span class="sxs-lookup"><span data-stu-id="c9c47-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="c9c47-207">Importálási/exportálási JSON-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="c9c47-207">Import/Export JSON documents</span></span>
<span data-ttu-id="c9c47-208">A Cosmos DB összekötő használatával egyszerűen</span><span class="sxs-lookup"><span data-stu-id="c9c47-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="c9c47-209">Importálja a JSON-dokumentumok különböző forrásokból Cosmos DB Azure Blob, beleértve az Azure Data Lake, a helyszíni fájlrendszer vagy egyéb fájlalapú tárolók Azure Data Factory által támogatott.</span><span class="sxs-lookup"><span data-stu-id="c9c47-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="c9c47-210">A Cosmos DB collecton JSON-dokumentumok exportálása különböző fájlalapú tárolók.</span><span class="sxs-lookup"><span data-stu-id="c9c47-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="c9c47-211">Adatok áttelepítése között két Cosmos DB gyűjteményeket-van.</span><span class="sxs-lookup"><span data-stu-id="c9c47-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="c9c47-212">tooachieve ilyen séma-független másolja,</span><span class="sxs-lookup"><span data-stu-id="c9c47-212">tooachieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="c9c47-213">Másolása varázsló segítségével, hogy hello **"exportálni-tooJSON fájlok vagy Cosmos DB gyűjtemény"** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c9c47-213">When using copy wizard, check hello **"Export as-is tooJSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="c9c47-214">Ha JSON szerkesztésére használatával nem adnak meg hello "structure" szakasz Cosmos DB adatkészlet(ek), sem "nestingSeparator" tulajdonságának Cosmos DB forrás/fogadó a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c9c47-214">When using JSON editing, do not specify hello "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="c9c47-215">a tooimport / tooJSON fájlok, hello fájl tároló adatkészlet adja meg formázási típusa "JsonFormat", "filePattern" konfigurációs és kihagyása hello rest formátum beállításainak exportálásához lásd: [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format) részletei szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-215">tooimport from/export tooJSON files, in hello file store dataset specify format type as "JsonFormat", config "filePattern" and skip hello rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="c9c47-216">JSON-példák</span><span class="sxs-lookup"><span data-stu-id="c9c47-216">JSON examples</span></span>
<span data-ttu-id="c9c47-217">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c9c47-217">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c9c47-218">Azok hogyan toocopy adatok tooand Azure Cosmos DB és az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-218">They show how toocopy data tooand from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="c9c47-219">Azonban az adatok átmásolhatók **közvetlenül** bármelyik hello források tooany közölt hello nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="c9c47-219">However, data can be copied **directly** from any of hello sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a><span data-ttu-id="c9c47-220">Példa: Adatok másolása az Azure Cosmos DB tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="c9c47-220">Example: Copy data from Azure Cosmos DB tooAzure Blob</span></span>
<span data-ttu-id="c9c47-221">az alábbi hello minta mutatja:</span><span class="sxs-lookup"><span data-stu-id="c9c47-221">hello sample below shows:</span></span>

1. <span data-ttu-id="c9c47-222">A társított szolgáltatás típusa [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="c9c47-223">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="c9c47-224">Bemeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="c9c47-225">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="c9c47-226">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [DocumentDbCollectionSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="c9c47-227">hello minta Azure Cosmos DB tooAzure Blob másolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c9c47-227">hello sample copies data in Azure Cosmos DB tooAzure Blob.</span></span> <span data-ttu-id="c9c47-228">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c9c47-228">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c9c47-229">**A társított szolgáltatásnak Azure Cosmos DB:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-229">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="c9c47-230">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-230">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="c9c47-231">**Az Azure Document DB rendszerbe bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="c9c47-232">hello példa feltételezi, hogy rendelkezik-e egy nevű gyűjtemény **személy** Azure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-232">hello sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="c9c47-233">"External" beállítása: "true", és megadása externalData házirenddel kapcsolatos információk az Azure Data Factory hello szolgáltatás hello tábla külső toohello adat-előállítót, adat-előállítóban hello tevékenység nem eredményezett.</span><span class="sxs-lookup"><span data-stu-id="c9c47-233">Setting “external”: ”true” and specifying externalData policy information hello Azure Data Factory service that hello table is external toohello data factory and not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="c9c47-234">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="c9c47-235">Adata másolt tooa új blob tükröző hello adott dátum és idő órában lépésköz hello BLOB hello elérési úttal rendelkező óránként.</span><span class="sxs-lookup"><span data-stu-id="c9c47-235">Data is copied tooa new blob every hour with hello path for hello blob reflecting hello specific datetime with hour granularity.</span></span>

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="c9c47-236">A minta JSON-dokumentum a hello személy gyűjtemény egy Cosmos DB adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="c9c47-236">Sample JSON document in hello Person collection in a Cosmos DB database:</span></span>

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
<span data-ttu-id="c9c47-237">Cosmos DB támogatja a JSON-dokumentumok hierarchikus over szintaxis például egy SQL használatával dokumentumok lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="c9c47-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="c9c47-238">Példa:</span><span class="sxs-lookup"><span data-stu-id="c9c47-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="c9c47-239">hello következő csővezeték-másolatok adatait hello személy hello Azure Cosmos DB adatbázis tooan Azure blob-gyűjteményt érinti.</span><span class="sxs-lookup"><span data-stu-id="c9c47-239">hello following pipeline copies data from hello Person collection in hello Azure Cosmos DB database tooan Azure blob.</span></span> <span data-ttu-id="c9c47-240">Hello másolási tevékenység hello részeként bemeneti és kimeneti adatkészletek van megadva.</span><span class="sxs-lookup"><span data-stu-id="c9c47-240">As part of hello copy activity hello input and output datasets have been specified.</span></span>  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a><span data-ttu-id="c9c47-241">Példa: Adatok másolása az Azure Blob tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c9c47-241">Example: Copy data from Azure Blob tooAzure Cosmos DB</span></span> 
<span data-ttu-id="c9c47-242">az alábbi hello minta mutatja:</span><span class="sxs-lookup"><span data-stu-id="c9c47-242">hello sample below shows:</span></span>

1. <span data-ttu-id="c9c47-243">A társított szolgáltatás típusa [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="c9c47-244">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="c9c47-245">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="c9c47-246">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="c9c47-247">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="c9c47-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="c9c47-248">hello minta Azure blob tooAzure Cosmos DB másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="c9c47-248">hello sample copies data from Azure blob tooAzure Cosmos DB.</span></span> <span data-ttu-id="c9c47-249">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c9c47-249">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c9c47-250">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-250">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="c9c47-251">**A társított szolgáltatásnak Azure Cosmos DB:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-251">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="c9c47-252">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-252">**Azure Blob input dataset:**</span></span>

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="c9c47-253">**Az Azure Cosmos DB kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c9c47-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="c9c47-254">hello minta "Személy" nevű tooa adatgyűjtés másolja.</span><span class="sxs-lookup"><span data-stu-id="c9c47-254">hello sample copies data tooa collection named “Person”.</span></span>

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="c9c47-255">hello következő Azure Blob toohello hello Cosmos DB személy gyűjtemény másolatok adatait a következő feldolgozási sorban.</span><span class="sxs-lookup"><span data-stu-id="c9c47-255">hello following pipeline copies data from Azure Blob toohello Person collection in hello Cosmos DB.</span></span> <span data-ttu-id="c9c47-256">Hello másolási tevékenység hello részeként bemeneti és kimeneti adatkészletek van megadva.</span><span class="sxs-lookup"><span data-stu-id="c9c47-256">As part of hello copy activity hello input and output datasets have been specified.</span></span>

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
<span data-ttu-id="c9c47-257">Ha hello minta blob bemeneti van, mint</span><span class="sxs-lookup"><span data-stu-id="c9c47-257">If hello sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="c9c47-258">Hello kimeneti JSON-t Cosmos DB lesz majd megfelelően:</span><span class="sxs-lookup"><span data-stu-id="c9c47-258">Then hello output JSON in Cosmos DB will be as:</span></span>

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
<span data-ttu-id="c9c47-259">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c9c47-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="c9c47-260">Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül **nestingSeparator**, amely "."</span><span class="sxs-lookup"><span data-stu-id="c9c47-260">Azure Data Factory enables user toodenote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="c9c47-261">Ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="c9c47-261">in this example.</span></span> <span data-ttu-id="c9c47-262">Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása.</span><span class="sxs-lookup"><span data-stu-id="c9c47-262">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="c9c47-263">Függelék:</span><span class="sxs-lookup"><span data-stu-id="c9c47-263">Appendix</span></span>
1. <span data-ttu-id="c9c47-264">**Kérdés:** hello másolási tevékenység támogatási frissítés a meglévő rekordok?</span><span class="sxs-lookup"><span data-stu-id="c9c47-264">**Question:** Does hello Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="c9c47-265">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="c9c47-265">**Answer:** No.</span></span>
2. <span data-ttu-id="c9c47-266">**Kérdés:** hogyan működik már egy másolás tooAzure Cosmos DB foglalkozzon ismétlését másolt rekordok?</span><span class="sxs-lookup"><span data-stu-id="c9c47-266">**Question:** How does a retry of a copy tooAzure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="c9c47-267">**Válasz:** ha rögzíti egy "ID" mezőt és hello másolási művelet megkísérli tooinsert hello rekord azonos azonosítója, hello másolási művelet hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="c9c47-267">**Answer:** If records have an "ID" field and hello copy operation tries tooinsert a record with hello same ID, hello copy operation throws an error.</span></span>  
3. <span data-ttu-id="c9c47-268">**Kérdés:** nem támogatja a Data Factory [tartományt vagy a kivonat-alapú adatparticionálás](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="c9c47-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="c9c47-269">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="c9c47-269">**Answer:** No.</span></span>
4. <span data-ttu-id="c9c47-270">**Kérdés:** állítható be egy táblához több Azure Cosmos DB gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="c9c47-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="c9c47-271">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="c9c47-271">**Answer:** No.</span></span> <span data-ttu-id="c9c47-272">Jelenleg csak egy gyűjteményhez adható meg.</span><span class="sxs-lookup"><span data-stu-id="c9c47-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="c9c47-273">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="c9c47-273">Performance and Tuning</span></span>
<span data-ttu-id="c9c47-274">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="c9c47-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
