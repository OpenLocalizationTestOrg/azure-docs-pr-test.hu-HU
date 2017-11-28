---
title: "Adatok áthelyezése az Azure Cosmos DB |} Microsoft Docs"
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
ms.openlocfilehash: 7a11c6ade0325b08ad520448bbf82d64a0a555f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="7005d-103">Adatok áthelyezése, és az Azure Cosmos Adatbázisba az Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="7005d-103">Move data to and from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="7005d-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory áthelyezni az adatokat és a Azure Cosmos DB (a DocumentDB API).</span><span class="sxs-lookup"><span data-stu-id="7005d-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="7005d-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="7005d-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="7005d-106">Bármely támogatott forrás adattárolóból Azure Cosmos DB vagy az Azure Cosmos Adatbázisból bármely támogatott fogadó adattárolóhoz adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="7005d-106">You can copy data from any supported source data store to Azure Cosmos DB or from Azure Cosmos DB to any supported sink data store.</span></span> <span data-ttu-id="7005d-107">Adatforrások vagy mosdók a másolási tevékenység által támogatott adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="7005d-107">For a list of data stores supported as sources or sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7005d-108">Azure Cosmos DB összekötő csak a DocumentDB API támogatja.</span><span class="sxs-lookup"><span data-stu-id="7005d-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="7005d-109">Az adatok másolása-van/JSON-fájlokat vagy egy másik Cosmos DB gyűjteményhez, lásd: [Import/Export JSON-dokumentumok](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="7005d-109">To copy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="7005d-110">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7005d-110">Getting started</span></span>
<span data-ttu-id="7005d-111">A másolási tevékenység, mely az adatok Azure Cosmos DB és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="7005d-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="7005d-112">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="7005d-112">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="7005d-113">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="7005d-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="7005d-114">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7005d-114">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7005d-115">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="7005d-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="7005d-116">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="7005d-116">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="7005d-117">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="7005d-117">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="7005d-118">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="7005d-118">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="7005d-119">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="7005d-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="7005d-120">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="7005d-120">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="7005d-121">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="7005d-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="7005d-122">Adatok másolása az Cosmos DB használandó adat-előállító entitások JSON-definíciók minták, lásd: [JSON példák](#json-examples) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="7005d-122">For samples with JSON definitions for Data Factory entities that are used to copy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="7005d-123">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások Cosmos DB JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="7005d-123">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Cosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="7005d-124">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="7005d-124">Linked service properties</span></span>
<span data-ttu-id="7005d-125">A következő táblázat a JSON-elemek szerepelnek Azure Cosmos DB kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="7005d-125">The following table provides description for JSON elements specific to Azure Cosmos DB linked service.</span></span>

| <span data-ttu-id="7005d-126">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="7005d-126">**Property**</span></span> | <span data-ttu-id="7005d-127">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="7005d-127">**Description**</span></span> | <span data-ttu-id="7005d-128">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="7005d-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7005d-129">type</span><span class="sxs-lookup"><span data-stu-id="7005d-129">type</span></span> |<span data-ttu-id="7005d-130">A type tulajdonságot kell beállítani: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="7005d-130">The type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="7005d-131">Igen</span><span class="sxs-lookup"><span data-stu-id="7005d-131">Yes</span></span> |
| <span data-ttu-id="7005d-132">connectionString</span><span class="sxs-lookup"><span data-stu-id="7005d-132">connectionString</span></span> |<span data-ttu-id="7005d-133">Adja meg Azure Cosmos DB adatbázishoz való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="7005d-133">Specify information needed to connect to Azure Cosmos DB database.</span></span> |<span data-ttu-id="7005d-134">Igen</span><span class="sxs-lookup"><span data-stu-id="7005d-134">Yes</span></span> |

<span data-ttu-id="7005d-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="7005d-135">Example:</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="7005d-136">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7005d-136">Dataset properties</span></span>
<span data-ttu-id="7005d-137">A szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listájáért tekintse meg a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7005d-137">For a full list of sections & properties available for defining datasets please refer to the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7005d-138">Például a struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="7005d-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="7005d-139">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="7005d-139">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="7005d-140">A typeProperties szakasz az adatkészlet típusú **DocumentDbCollection** a következő tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7005d-140">The typeProperties section for the dataset of type **DocumentDbCollection** has the following properties.</span></span>

| <span data-ttu-id="7005d-141">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="7005d-141">**Property**</span></span> | <span data-ttu-id="7005d-142">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="7005d-142">**Description**</span></span> | <span data-ttu-id="7005d-143">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="7005d-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7005d-144">CollectionName</span><span class="sxs-lookup"><span data-stu-id="7005d-144">collectionName</span></span> |<span data-ttu-id="7005d-145">A Cosmos DB dokumentum gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="7005d-145">Name of the Cosmos DB document collection.</span></span> |<span data-ttu-id="7005d-146">Igen</span><span class="sxs-lookup"><span data-stu-id="7005d-146">Yes</span></span> |

<span data-ttu-id="7005d-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="7005d-147">Example:</span></span>

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
### <a name="schema-by-data-factory"></a><span data-ttu-id="7005d-148">Adat-előállító sémája</span><span class="sxs-lookup"><span data-stu-id="7005d-148">Schema by Data Factory</span></span>
<span data-ttu-id="7005d-149">Például az Azure Cosmos DB tárolóinak sémamentes adatokra a Data Factory szolgáltatásnak kikövetkezteti a séma a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="7005d-149">For schema-free data stores such as Azure Cosmos DB, the Data Factory service infers the schema in one of the following ways:</span></span>  

1. <span data-ttu-id="7005d-150">Ha az adatok szerkezete használatával adja meg a **struktúra** tulajdonsághoz a DataSet adatkészlet-definícióban a Data Factory szolgáltatásnak eleget tegyen a séma szerint ez a struktúra.</span><span class="sxs-lookup"><span data-stu-id="7005d-150">If you specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service honors this structure as the schema.</span></span> <span data-ttu-id="7005d-151">Ebben az esetben ha egy sort tartalmaz egy olyan oszlop értékét, null értékű nyújtanak az.</span><span class="sxs-lookup"><span data-stu-id="7005d-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="7005d-152">Ha nincs megadva az adatok szerkezete használatával a **struktúra** tulajdonság az adatkészlet-definícióban, a Data Factory szolgáltatásnak kikövetkezteti a séma az adatok első sora használatával.</span><span class="sxs-lookup"><span data-stu-id="7005d-152">If you do not specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service infers the schema by using the first row in the data.</span></span> <span data-ttu-id="7005d-153">Ebben az esetben ha az első sort tartalmazza a teljes séma, néhány oszlop nem érhető el a másolási művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="7005d-153">In this case, if the first row does not contain the full schema, some columns will be missing in the result of copy operation.</span></span>

<span data-ttu-id="7005d-154">Ezért sémamentes adatforrások, az ajánlott eljárás, hogy adja meg az adatok szerkezete a **struktúra** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="7005d-154">Therefore, for schema-free data sources, the best practice is to specify the structure of data using the **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="7005d-155">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7005d-155">Copy activity properties</span></span>
<span data-ttu-id="7005d-156">A szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listájáért tekintse meg a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7005d-156">For a full list of sections & properties available for defining activities please refer to the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7005d-157">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7005d-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="7005d-158">A másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="7005d-158">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="7005d-159">Tulajdonságok érhetők el a tevékenység typeProperties szakaszában viszont eltérőek a tevékenységek minden típusának, és a források és mosdók típusától függően változik másolási tevékenység esetén.</span><span class="sxs-lookup"><span data-stu-id="7005d-159">Properties available in the typeProperties section of the activity on the other hand vary with each activity type and in case of Copy activity they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="7005d-160">Ha forrás típusa másolási tevékenység esetén **DocumentDbCollectionSource** a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="7005d-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="7005d-161">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="7005d-161">**Property**</span></span> | <span data-ttu-id="7005d-162">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="7005d-162">**Description**</span></span> | <span data-ttu-id="7005d-163">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="7005d-163">**Allowed values**</span></span> | <span data-ttu-id="7005d-164">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="7005d-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7005d-165">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="7005d-165">query</span></span> |<span data-ttu-id="7005d-166">Adja meg a lekérdezés adatainak olvasására.</span><span class="sxs-lookup"><span data-stu-id="7005d-166">Specify the query to read data.</span></span> |<span data-ttu-id="7005d-167">Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott.</span><span class="sxs-lookup"><span data-stu-id="7005d-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="7005d-168">Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="7005d-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="7005d-169">Nem</span><span class="sxs-lookup"><span data-stu-id="7005d-169">No</span></span> <br/><br/><span data-ttu-id="7005d-170">Ha nincs megadva, az SQL-utasítást, amely végrehajtja a rendszer:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="7005d-170">If not specified, the SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="7005d-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="7005d-171">nestingSeparator</span></span> |<span data-ttu-id="7005d-172">Jelzi, hogy a dokumentum van beágyazva speciális karakter</span><span class="sxs-lookup"><span data-stu-id="7005d-172">Special character to indicate that the document is nested</span></span> |<span data-ttu-id="7005d-173">Bármely karakter.</span><span class="sxs-lookup"><span data-stu-id="7005d-173">Any character.</span></span> <br/><br/><span data-ttu-id="7005d-174">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7005d-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="7005d-175">Az Azure Data Factory lehetővé teszi, hogy a felhasználó nestingSeparator, amely használatával a hierarchia jelöléséhez "."</span><span class="sxs-lookup"><span data-stu-id="7005d-175">Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="7005d-176">a fenti példákban.</span><span class="sxs-lookup"><span data-stu-id="7005d-176">in the above examples.</span></span> <span data-ttu-id="7005d-177">A elválasztóval, a másolási tevékenység hoz létre a "Name" objektum három gyermekek elemekkel először középső és az utolsó, "Name.First", "Name.Middle" és "Name.Last" tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="7005d-177">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span> |<span data-ttu-id="7005d-178">Nem</span><span class="sxs-lookup"><span data-stu-id="7005d-178">No</span></span> |

<span data-ttu-id="7005d-179">**DocumentDbCollectionSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="7005d-179">**DocumentDbCollectionSink** supports the following properties:</span></span>

| <span data-ttu-id="7005d-180">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="7005d-180">**Property**</span></span> | <span data-ttu-id="7005d-181">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="7005d-181">**Description**</span></span> | <span data-ttu-id="7005d-182">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="7005d-182">**Allowed values**</span></span> | <span data-ttu-id="7005d-183">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="7005d-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7005d-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="7005d-184">nestingSeparator</span></span> |<span data-ttu-id="7005d-185">A forrás oszlop nevét jelzi, hogy a beágyazott dokumentum egy különleges karakterek van szükség.</span><span class="sxs-lookup"><span data-stu-id="7005d-185">A special character in the source column name to indicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="7005d-186">Például fent: `Name.First` a kimeneti táblát hoz létre a következő JSON struktúrában a Cosmos DB dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="7005d-186">For example above: `Name.First` in the output table produces the following JSON structure in the Cosmos DB document:</span></span><br/><br/><span data-ttu-id="7005d-187">"Name": {</span><span class="sxs-lookup"><span data-stu-id="7005d-187">"Name": {</span></span><br/>    <span data-ttu-id="7005d-188">"Első": "John"</span><span class="sxs-lookup"><span data-stu-id="7005d-188">"First": "John"</span></span><br/><span data-ttu-id="7005d-189">},</span><span class="sxs-lookup"><span data-stu-id="7005d-189">},</span></span> |<span data-ttu-id="7005d-190">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="7005d-190">Character that is used to separate nesting levels.</span></span><br/><br/><span data-ttu-id="7005d-191">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="7005d-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="7005d-192">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="7005d-192">Character that is used to separate nesting levels.</span></span> <br/><br/><span data-ttu-id="7005d-193">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="7005d-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="7005d-194">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="7005d-194">writeBatchSize</span></span> |<span data-ttu-id="7005d-195">Dokumentumok létrehozásához Azure Cosmos DB szolgáltatás párhuzamos kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="7005d-195">Number of parallel requests to Azure Cosmos DB service to create documents.</span></span><br/><br/><span data-ttu-id="7005d-196">A teljesítmény úgy finomhangolhatja, és a Cosmos DB adatok másolásakor e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="7005d-196">You can fine-tune the performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="7005d-197">A jobb teljesítmény számíthat, mivel több párhuzamos kérelem Cosmos DB writeBatchSize növelésével.</span><span class="sxs-lookup"><span data-stu-id="7005d-197">You can expect a better performance when you increase writeBatchSize because more parallel requests to Cosmos DB are sent.</span></span> <span data-ttu-id="7005d-198">Azonban kell elkerülheti a sávszélesség-szabályozás, amely képes throw a hibaüzenet a következő: "Ez nagy lekérő".</span><span class="sxs-lookup"><span data-stu-id="7005d-198">However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="7005d-199">Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek segítségével jobban gyűjtemény (pl. S3) rendelkezik a legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc).</span><span class="sxs-lookup"><span data-stu-id="7005d-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) to have the most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="7005d-200">Egész szám</span><span class="sxs-lookup"><span data-stu-id="7005d-200">Integer</span></span> |<span data-ttu-id="7005d-201">Nem (alapértelmezett: 5)</span><span class="sxs-lookup"><span data-stu-id="7005d-201">No (default: 5)</span></span> |
| <span data-ttu-id="7005d-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="7005d-202">writeBatchTimeout</span></span> |<span data-ttu-id="7005d-203">Várakozási idő a művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="7005d-203">Wait time for the operation to complete before it times out.</span></span> |<span data-ttu-id="7005d-204">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="7005d-204">timespan</span></span><br/><br/> <span data-ttu-id="7005d-205">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="7005d-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="7005d-206">Nem</span><span class="sxs-lookup"><span data-stu-id="7005d-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="7005d-207">Importálási/exportálási JSON-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="7005d-207">Import/Export JSON documents</span></span>
<span data-ttu-id="7005d-208">A Cosmos DB összekötő használatával egyszerűen</span><span class="sxs-lookup"><span data-stu-id="7005d-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="7005d-209">Importálja a JSON-dokumentumok különböző forrásokból Cosmos DB Azure Blob, beleértve az Azure Data Lake, a helyszíni fájlrendszer vagy egyéb fájlalapú tárolók Azure Data Factory által támogatott.</span><span class="sxs-lookup"><span data-stu-id="7005d-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="7005d-210">A Cosmos DB collecton JSON-dokumentumok exportálása különböző fájlalapú tárolók.</span><span class="sxs-lookup"><span data-stu-id="7005d-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="7005d-211">Adatok áttelepítése között két Cosmos DB gyűjteményeket-van.</span><span class="sxs-lookup"><span data-stu-id="7005d-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="7005d-212">A séma-független másolat eléréséhez</span><span class="sxs-lookup"><span data-stu-id="7005d-212">To achieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="7005d-213">Másolása varázsló segítségével, hogy a **"exportálni-JSON-fájlokat vagy Cosmos DB gyűjtemény"** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7005d-213">When using copy wizard, check the **"Export as-is to JSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="7005d-214">Ha JSON szerkesztésére használatával nem adnak meg a "structure" szakasz Cosmos DB adatkészlet(ek), sem "nestingSeparator" tulajdonságának Cosmos DB forrás/fogadó a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7005d-214">When using JSON editing, do not specify the "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="7005d-215">Importálhat / JSON fájlokba exportálását, a fájl tároló adatkészlet adja meg formázási típusa "JsonFormat", "filePattern" konfigurációs és hagyja ki a többi formázási beállítások. További információ: [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format) részletei szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7005d-215">To import from/export to JSON files, in the file store dataset specify format type as "JsonFormat", config "filePattern" and skip the rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="7005d-216">JSON-példák</span><span class="sxs-lookup"><span data-stu-id="7005d-216">JSON examples</span></span>
<span data-ttu-id="7005d-217">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7005d-217">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7005d-218">Adatok másolása az Azure Cosmos DB és az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="7005d-218">They show how to copy data to and from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="7005d-219">Azonban az adatok átmásolhatók **közvetlenül** bármelyik bármely, a megadott nyelő források [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="7005d-219">However, data can be copied **directly** from any of the sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a><span data-ttu-id="7005d-220">Példa: Adatok másolása az Azure Cosmos DB az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="7005d-220">Example: Copy data from Azure Cosmos DB to Azure Blob</span></span>
<span data-ttu-id="7005d-221">Az alábbi példa látható:</span><span class="sxs-lookup"><span data-stu-id="7005d-221">The sample below shows:</span></span>

1. <span data-ttu-id="7005d-222">A társított szolgáltatás típusa [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="7005d-223">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7005d-224">Bemeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="7005d-225">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7005d-226">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [DocumentDbCollectionSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7005d-227">A minta Azure Cosmos DB adatainak Azure Blob másolása.</span><span class="sxs-lookup"><span data-stu-id="7005d-227">The sample copies data in Azure Cosmos DB to Azure Blob.</span></span> <span data-ttu-id="7005d-228">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="7005d-228">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="7005d-229">**A társított szolgáltatásnak Azure Cosmos DB:**</span><span class="sxs-lookup"><span data-stu-id="7005d-229">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="7005d-230">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="7005d-230">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="7005d-231">**Az Azure Document DB rendszerbe bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="7005d-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="7005d-232">A példa feltételezi, hogy rendelkezik-e egy nevű gyűjtemény **személy** Azure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="7005d-232">The sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="7005d-233">"External" beállítása: "true", és externalData házirenddel kapcsolatos információk az Azure Data Factory szolgáltatásnak, hogy a tábla külső data factoryval való, és nem egy adat-előállító tevékenység által előállított megadásával.</span><span class="sxs-lookup"><span data-stu-id="7005d-233">Setting “external”: ”true” and specifying externalData policy information the Azure Data Factory service that the table is external to the data factory and not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="7005d-234">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="7005d-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="7005d-235">Adatokat egy új blob minden órában az elérési úttal rendelkező a BLOB a megadott dátum és idő órában lépésköz tükröző másolja.</span><span class="sxs-lookup"><span data-stu-id="7005d-235">Data is copied to a new blob every hour with the path for the blob reflecting the specific datetime with hour granularity.</span></span>

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
<span data-ttu-id="7005d-236">A minta JSON-dokumentum a személy gyűjtemény egy Cosmos DB adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="7005d-236">Sample JSON document in the Person collection in a Cosmos DB database:</span></span>

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
<span data-ttu-id="7005d-237">Cosmos DB támogatja a JSON-dokumentumok hierarchikus over szintaxis például egy SQL használatával dokumentumok lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="7005d-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="7005d-238">Példa:</span><span class="sxs-lookup"><span data-stu-id="7005d-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="7005d-239">A következő adatcsatorna másol adatokat az Azure Cosmos DB adatbázisban a személy gyűjtemény egy Azure-blobot.</span><span class="sxs-lookup"><span data-stu-id="7005d-239">The following pipeline copies data from the Person collection in the Azure Cosmos DB database to an Azure blob.</span></span> <span data-ttu-id="7005d-240">Része a másolási tevékenység során a bemeneti és kimeneti adatkészletek van megadva.</span><span class="sxs-lookup"><span data-stu-id="7005d-240">As part of the copy activity the input and output datasets have been specified.</span></span>  

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
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a><span data-ttu-id="7005d-241">Példa: Adatok másolása az Azure Blob az Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7005d-241">Example: Copy data from Azure Blob to Azure Cosmos DB</span></span> 
<span data-ttu-id="7005d-242">Az alábbi példa látható:</span><span class="sxs-lookup"><span data-stu-id="7005d-242">The sample below shows:</span></span>

1. <span data-ttu-id="7005d-243">A társított szolgáltatás típusa [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="7005d-244">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7005d-245">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="7005d-246">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="7005d-247">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="7005d-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="7005d-248">A minta másolja az adatokat az Azure blob az Azure Cosmos-Adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="7005d-248">The sample copies data from Azure blob to Azure Cosmos DB.</span></span> <span data-ttu-id="7005d-249">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="7005d-249">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="7005d-250">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="7005d-250">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="7005d-251">**A társított szolgáltatásnak Azure Cosmos DB:**</span><span class="sxs-lookup"><span data-stu-id="7005d-251">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="7005d-252">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="7005d-252">**Azure Blob input dataset:**</span></span>

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
<span data-ttu-id="7005d-253">**Az Azure Cosmos DB kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="7005d-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="7005d-254">A minta egy "Személy" nevű gyűjtemény másolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7005d-254">The sample copies data to a collection named “Person”.</span></span>

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
<span data-ttu-id="7005d-255">A következő adatcsatorna másol adatokat Azure Blob a Cosmos DB a személy gyűjteménynek.</span><span class="sxs-lookup"><span data-stu-id="7005d-255">The following pipeline copies data from Azure Blob to the Person collection in the Cosmos DB.</span></span> <span data-ttu-id="7005d-256">Része a másolási tevékenység során a bemeneti és kimeneti adatkészletek van megadva.</span><span class="sxs-lookup"><span data-stu-id="7005d-256">As part of the copy activity the input and output datasets have been specified.</span></span>

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
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
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
<span data-ttu-id="7005d-257">Ha a minta blob bemeneti van, mint</span><span class="sxs-lookup"><span data-stu-id="7005d-257">If the sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="7005d-258">A kimeneti JSON-t Cosmos DB lesz majd megfelelően:</span><span class="sxs-lookup"><span data-stu-id="7005d-258">Then the output JSON in Cosmos DB will be as:</span></span>

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
<span data-ttu-id="7005d-259">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7005d-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="7005d-260">Az Azure Data Factory lehetővé teszi, hogy a felhasználó keresztül hierarchia jelöléséhez **nestingSeparator**, amely "."</span><span class="sxs-lookup"><span data-stu-id="7005d-260">Azure Data Factory enables user to denote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="7005d-261">Ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="7005d-261">in this example.</span></span> <span data-ttu-id="7005d-262">A elválasztóval, a másolási tevékenység hoz létre a "Name" objektum három gyermekek elemekkel először középső és az utolsó, "Name.First", "Name.Middle" és "Name.Last" tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="7005d-262">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="7005d-263">Függelék:</span><span class="sxs-lookup"><span data-stu-id="7005d-263">Appendix</span></span>
1. <span data-ttu-id="7005d-264">**Kérdés:** a másolási tevékenység támogatási frissítés a meglévő rekordok Does?</span><span class="sxs-lookup"><span data-stu-id="7005d-264">**Question:** Does the Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="7005d-265">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="7005d-265">**Answer:** No.</span></span>
2. <span data-ttu-id="7005d-266">**Kérdés:** hogyan működik az Azure Cosmos DB kezelésére másolatának újrapróbálkozást már másolt rekordok?</span><span class="sxs-lookup"><span data-stu-id="7005d-266">**Question:** How does a retry of a copy to Azure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="7005d-267">**Válasz:** ha rögzíti egy "ID" mezőt rendelkezik, és a másolási művelet megkísérli beszúrására ugyanezzel az Azonosítóval rendelkező, a másolási művelet hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="7005d-267">**Answer:** If records have an "ID" field and the copy operation tries to insert a record with the same ID, the copy operation throws an error.</span></span>  
3. <span data-ttu-id="7005d-268">**Kérdés:** nem támogatja a Data Factory [tartományt vagy a kivonat-alapú adatparticionálás](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="7005d-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="7005d-269">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="7005d-269">**Answer:** No.</span></span>
4. <span data-ttu-id="7005d-270">**Kérdés:** állítható be egy táblához több Azure Cosmos DB gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="7005d-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="7005d-271">**Válasz:** nem.</span><span class="sxs-lookup"><span data-stu-id="7005d-271">**Answer:** No.</span></span> <span data-ttu-id="7005d-272">Jelenleg csak egy gyűjteményhez adható meg.</span><span class="sxs-lookup"><span data-stu-id="7005d-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="7005d-273">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="7005d-273">Performance and Tuning</span></span>
<span data-ttu-id="7005d-274">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="7005d-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
