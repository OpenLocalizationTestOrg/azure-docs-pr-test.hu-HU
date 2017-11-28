---
title: "az Azure Cosmos DB aaaDatabase áttelepítési eszköz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello nyissa meg a forrás Azure Cosmos DB adatok áttelepítési eszközök tooimport adatok tooAzure Cosmos DB különböző forrásokból, beleértve a MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV és JSON fájlokat. Fürt megosztott kötetei szolgáltatás tooJSON átalakítás."
keywords: "fürt megosztott kötetei szolgáltatás toojson, adatbázis-áttelepítési eszközök, alakítsa át a fürt megosztott kötetei szolgáltatás toojson"
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 997648a31602d854db75bb6ce4e2ecff36fc1069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a><span data-ttu-id="1557f-105">Hogyan tooimport adatok Azure Cosmos DB hello DocumentDB API?</span><span class="sxs-lookup"><span data-stu-id="1557f-105">How tooimport data into Azure Cosmos DB for hello DocumentDB API?</span></span>

<span data-ttu-id="1557f-106">Ez az oktatóanyag biztosít használatával hello Azure Cosmos DB: DocumentDB API adatáttelepítés eszközt, amely a különböző forrásokból, beleértve a JSON-fájlokat importálhatja az adatok CSV-fájlok, SQL, a MongoDB, az Azure Table storage, Amazon DynamoDB és Azure Cosmos DB DocumentDB API-gyűjtemények a gyűjteményekbe használható az Azure Cosmos DB és hello DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="1557f-106">This tutorial provides instructions on using hello Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and hello DocumentDB API.</span></span> <span data-ttu-id="1557f-107">hello adatáttelepítési eszközét is használható, a DocumentDB API hello egypartíciós gyűjtemény tooa több partíció gyűjteményét áttelepítésekor.</span><span class="sxs-lookup"><span data-stu-id="1557f-107">hello Data Migration tool can also be used when migrating from a single partition collection tooa multi-partition collection for hello DocumentDB API.</span></span>

<span data-ttu-id="1557f-108">hello adatáttelepítési eszköz csak akkor működik, amikor Azure Cosmos DB importáló adatimportáláshoz hello DocumentDB API használata.</span><span class="sxs-lookup"><span data-stu-id="1557f-108">hello Data Migration tool only works when importing data into Azure Cosmos DB for use with hello DocumentDB API.</span></span> <span data-ttu-id="1557f-109">A hello tábla API és a Graph API adatok importálása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1557f-109">Importing data for use with hello Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="1557f-110">tooimport adatok hello MongoDB API-val való használatra, lásd: [Azure Cosmos DB: hogyan toomigrate adatainak hello MongoDB API?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-110">tooimport data for use with hello MongoDB API, see [Azure Cosmos DB: How toomigrate data for hello MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="1557f-111">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-111">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1557f-112">Hello adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="1557f-112">Installing hello Data Migration tool</span></span>
> * <span data-ttu-id="1557f-113">Különböző forrásokból származó adatok importálása</span><span class="sxs-lookup"><span data-stu-id="1557f-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="1557f-114">Azure Cosmos DB tooJSON exportálása</span><span class="sxs-lookup"><span data-stu-id="1557f-114">Exporting from Azure Cosmos DB tooJSON</span></span>

## <span data-ttu-id="1557f-115"><a id="Prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1557f-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="1557f-116">Hello jelen cikkben lévő utasítások követése, előtt győződjön meg arról, hogy hello következőkkel:</span><span class="sxs-lookup"><span data-stu-id="1557f-116">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="1557f-117">[A Microsoft .NET-keretrendszer 4.51](https://www.microsoft.com/download/developer-tools.aspx) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="1557f-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="1557f-118"><a id="Overviewl"></a>Hello adatáttelepítési eszköz áttekintése</span><span class="sxs-lookup"><span data-stu-id="1557f-118"><a id="Overviewl"></a>Overview of hello Data Migration tool</span></span>
<span data-ttu-id="1557f-119">hello adatáttelepítési eszközét egy nyílt forráskódú megoldás, amellyel különféle adatok tooAzure Cosmos DB móddal, többek között számos különböző:</span><span class="sxs-lookup"><span data-stu-id="1557f-119">hello Data Migration tool is an open source solution that imports data tooAzure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="1557f-120">JSON-fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-120">JSON files</span></span>
* <span data-ttu-id="1557f-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1557f-121">MongoDB</span></span>
* <span data-ttu-id="1557f-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1557f-122">SQL Server</span></span>
* <span data-ttu-id="1557f-123">CSV-fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-123">CSV files</span></span>
* <span data-ttu-id="1557f-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="1557f-124">Azure Table storage</span></span>
* <span data-ttu-id="1557f-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="1557f-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="1557f-126">HBase</span><span class="sxs-lookup"><span data-stu-id="1557f-126">HBase</span></span>
* <span data-ttu-id="1557f-127">Az Azure Cosmos DB gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="1557f-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="1557f-128">Amíg hello import eszközt tartalmaz egy grafikus felhasználói felületen (dtui.exe), azt is is vezeti hello parancssorból (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="1557f-128">While hello import tool includes a graphical user interface (dtui.exe), it can also be driven from hello command line (dt.exe).</span></span> <span data-ttu-id="1557f-129">Valójában van egy beállítás toooutput tartozó hello parancs hello felhasználói felületén keresztül importálás beállítása után.</span><span class="sxs-lookup"><span data-stu-id="1557f-129">In fact, there is an option toooutput hello associated command after setting up an import through hello UI.</span></span> <span data-ttu-id="1557f-130">Táblázatos forrásadatok (pl. az SQL Server vagy CSV-fájlokban) is kell alakítani, úgy, hogy az importálás során hozható létre a is a hierarchikus kapcsolat (aldokumentumok).</span><span class="sxs-lookup"><span data-stu-id="1557f-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="1557f-131">További információk az adatforrás beállításai minta parancssor tooimport minden forrás toolearn olvasása megőrzéséhez a cél lehetőségekkel, valamint a Megtekintés eredmények importálása.</span><span class="sxs-lookup"><span data-stu-id="1557f-131">Keep reading toolearn more about source options, sample command lines tooimport from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="1557f-132"><a id="Install"></a>Hello adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="1557f-132"><a id="Install"></a>Install hello Data Migration tool</span></span>
<span data-ttu-id="1557f-133">hello áttelepítési eszköz a forráskód nem elérhető a Githubon található [ebben a tárházban](https://github.com/azure/azure-documentdb-datamigrationtool) és egy lefordított elérhető a [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="1557f-133">hello migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="1557f-134">Akkor lehet, hogy vagy hello megoldás összeállításához vagy egyszerűen letöltéséhez és kibontásához hello összeállított tooa könyvtár az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="1557f-134">You may either compile hello solution or simply download and extract hello compiled version tooa directory of your choice.</span></span> <span data-ttu-id="1557f-135">Ezután futtassa vagy:</span><span class="sxs-lookup"><span data-stu-id="1557f-135">Then run either:</span></span>

* <span data-ttu-id="1557f-136">**Dtui.exe**: hello eszköz grafikus felület verziója</span><span class="sxs-lookup"><span data-stu-id="1557f-136">**Dtui.exe**: Graphical interface version of hello tool</span></span>
* <span data-ttu-id="1557f-137">**DT.exe**: hello eszköz parancssori verziója</span><span class="sxs-lookup"><span data-stu-id="1557f-137">**Dt.exe**: Command-line version of hello tool</span></span>

## <a name="import-data"></a><span data-ttu-id="1557f-138">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="1557f-138">Import data</span></span>

<span data-ttu-id="1557f-139">Hello eszköz telepítését követően idő tooimport az adatok.</span><span class="sxs-lookup"><span data-stu-id="1557f-139">Once you've installed hello tool, it's time tooimport your data.</span></span> <span data-ttu-id="1557f-140">Milyen adatok szeretné, hogy tooimport?</span><span class="sxs-lookup"><span data-stu-id="1557f-140">What kind of data do you want tooimport?</span></span>

* [<span data-ttu-id="1557f-141">JSON-fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="1557f-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1557f-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="1557f-143">MongoDB exportfájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="1557f-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1557f-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="1557f-145">CSV-fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="1557f-146">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="1557f-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="1557f-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="1557f-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="1557f-148">A BLOB</span><span class="sxs-lookup"><span data-stu-id="1557f-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="1557f-149">Az Azure Cosmos DB gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="1557f-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="1557f-150">HBase</span><span class="sxs-lookup"><span data-stu-id="1557f-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="1557f-151">Az Azure Cosmos DB tömeges importálással</span><span class="sxs-lookup"><span data-stu-id="1557f-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="1557f-152">Az Azure Cosmos DB szekvenciális rekord importálása</span><span class="sxs-lookup"><span data-stu-id="1557f-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="1557f-153"><a id="JSON"></a>tooimport JSON-fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-153"><a id="JSON"></a>tooimport JSON files</span></span>
<span data-ttu-id="1557f-154">hello JSON fájl forrás importáló beállítás lehetővé teszi tooimport egy vagy több egyetlen dokumentum JSON-fájlokat vagy JSON-fájlokat, hogy minden egyes tartalmazza JSON-dokumentumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="1557f-154">hello JSON file source importer option allows you tooimport one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="1557f-155">JSON-fájlok tooimport tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="1557f-155">When adding folders that contain JSON files tooimport, you have hello option of recursively searching for files in subfolders.</span></span>

![Képernyőfelvétel a JSON fájl forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/jsonsource.png)

<span data-ttu-id="1557f-157">Az alábbiakban néhány parancssor minták tooimport JSON-fájlok:</span><span class="sxs-lookup"><span data-stu-id="1557f-157">Here are some command line samples tooimport JSON files:</span></span>

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition hello data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-158"><a id="MongoDB"></a>a MongoDB tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-158"><a id="MongoDB"></a>tooimport from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1557f-159">Ha tooan Azure Cosmos DB fiók mongodb-Protokolltámogatással rendelkező importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-159">If you are importing tooan Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="1557f-160">hello MongoDB forrás importáló beállítás lehetővé teszi az egyes MongoDB gyűjteményből tooimport és opcionálisan szűrése lekérdezéssel dokumentumok és/vagy módosításához hello dokumentumstruktúrával leképezés használatával.</span><span class="sxs-lookup"><span data-stu-id="1557f-160">hello MongoDB source importer option allows you tooimport from an individual MongoDB collection and optionally filter documents using a query and/or modify hello document structure by using a projection.</span></span>  

![Képernyőfelvétel a MongoDB forrására vonatkozó beállítások](./media/import-data/mongodbsource.png)

<span data-ttu-id="1557f-162">hello kapcsolati karakterlánc hello szabványos MongoDB formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="1557f-162">hello connection string is in hello standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="1557f-163">Használjon hello ellenőrizze parancs tooensure, amelyek a MongoDB-példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-163">Use hello Verify command tooensure that hello MongoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-164">Adja meg, amelyből adatok importálódnak hello gyűjtemény hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1557f-164">Enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="1557f-165">Szükség lehet, hogy adja meg, vagy adjon meg egy fájlt egy lekérdezés (pl. {pop: {$gt: 5000}}) és/vagy a leképezése (például {loc:0}) tooboth szűrő és alakzat hello adatok toobe importálva.</span><span class="sxs-lookup"><span data-stu-id="1557f-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) tooboth filter and shape hello data toobe imported.</span></span>

<span data-ttu-id="1557f-166">Az alábbiakban néhány parancssor minták tooimport a MongoDB:</span><span class="sxs-lookup"><span data-stu-id="1557f-166">Here are some command line samples tooimport from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="1557f-167"><a id="MongoDBExport"></a>tooimport MongoDB exportfájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-167"><a id="MongoDBExport"></a>tooimport MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1557f-168">Ha tooan Azure Cosmos DB fiók mongodb-protokolltámogatással rendelkező importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-168">If you are importing tooan Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="1557f-169">hello MongoDB exportálási JSON fájl forrás importáló beállítás lehetővé teszi egy tooimport, vagy további JSON-fájlok előállított hello mongoexport segédprogram.</span><span class="sxs-lookup"><span data-stu-id="1557f-169">hello MongoDB export JSON file source importer option allows you tooimport one or more JSON files produced from hello mongoexport utility.</span></span>  

![Képernyőfelvétel a MongoDB exportálási forrására vonatkozó beállítások](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="1557f-171">MongoDB exportálási JSON-fájlok importálása tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="1557f-171">When adding folders that contain MongoDB export JSON files for import, you have hello option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="1557f-172">Ez a parancssor minta tooimport a MongoDB-exportálás JSON-fájlok:</span><span class="sxs-lookup"><span data-stu-id="1557f-172">Here is a command line sample tooimport from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-173"><a id="SQL"></a>az SQL Server tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-173"><a id="SQL"></a>tooimport from SQL Server</span></span>
<span data-ttu-id="1557f-174">hello SQL importáló forrásbeállítás lehetővé teszi tooimport egy egyéni SQL Server-adatbázisból, és opcionálisan szűrése hello rekordok toobe importált lekérdezés segítségével.</span><span class="sxs-lookup"><span data-stu-id="1557f-174">hello SQL source importer option allows you tooimport from an individual SQL Server database and optionally filter hello records toobe imported using a query.</span></span> <span data-ttu-id="1557f-175">Emellett módosíthatja hello dokumentumstruktúrával (további sablonokat, amelyek később) beágyazási elválasztó megadásával.</span><span class="sxs-lookup"><span data-stu-id="1557f-175">In addition, you can modify hello document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Képernyőkép az SQL - adatbázis áttelepítési eszközök forrása](./media/import-data/sqlexportsource.png)

<span data-ttu-id="1557f-177">hello hello kapcsolati karakterlánc formátuma hello szabványos SQL kapcsolati karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="1557f-177">hello format of hello connection string is hello standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="1557f-178">Használjon hello ellenőrizze parancs tooensure, amely hello hello kapcsolati karakterlánc mezőben megadott SQL Server-példány segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-178">Use hello Verify command tooensure that hello SQL Server instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-179">elválasztó tulajdonság beágyazási hello importálása során használt toocreate hierarchikus kapcsolat (alárendelt dokumentumok).</span><span class="sxs-lookup"><span data-stu-id="1557f-179">hello nesting separator property is used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="1557f-180">Vegye figyelembe a következő SQL-lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-180">Consider hello following SQL query:</span></span>

<span data-ttu-id="1557f-181">*Válassza ki a TÍPUSKONVERZIÓ (BusinessEntityID AS varchar) azonosítója, neve, mint [Address.AddressType] AddressType, AddressLine1 [Address.AddressLine1], [Address.Location.City] városát, StateProvinceName [Address.Location.StateProvinceName], irányítószám szerint [Address.PostalCode], CountryRegionName [Address.CountryRegionName], a Sales.vStoreWithAddresses ahol AddressType = "Main Office"*</span><span class="sxs-lookup"><span data-stu-id="1557f-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="1557f-182">A következő (részleges) eredmények hello visszaadó:</span><span class="sxs-lookup"><span data-stu-id="1557f-182">Which returns hello following (partial) results:</span></span>

![Képernyőkép az SQL-lekérdezés eredményei](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="1557f-184">Vegye figyelembe például Address.AddressType és Address.Location.StateProvinceName hello aliasok.</span><span class="sxs-lookup"><span data-stu-id="1557f-184">Note hello aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="1557f-185">A beágyazási elválasztó megadásával '.', hello import eszközt cím hoz létre, és Address.Location aldokumentumok során hello importálása.</span><span class="sxs-lookup"><span data-stu-id="1557f-185">By specifying a nesting separator of ‘.’, hello import tool creates Address and Address.Location subdocuments during hello import.</span></span> <span data-ttu-id="1557f-186">Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:</span><span class="sxs-lookup"><span data-stu-id="1557f-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="1557f-187">*{"id": "956", "Name": "Egyeztetését értékesítés és a szolgáltatás", "Cím": {"AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor utca", "Hely": {"Város": "Ottawai", "StateProvinceName": "Ontario"}, "Irányítószám": "K4B 1S2", "CountryRegionName": "Kanada"}}*</span><span class="sxs-lookup"><span data-stu-id="1557f-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="1557f-188">Az alábbiakban néhány parancssor minták tooimport az SQL-kiszolgálóról:</span><span class="sxs-lookup"><span data-stu-id="1557f-188">Here are some command line samples tooimport from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-189"><a id="CSV"></a>tooimport CSV-fájlok és a konvertálás CSV tooJSON</span><span class="sxs-lookup"><span data-stu-id="1557f-189"><a id="CSV"></a>tooimport CSV files and convert CSV tooJSON</span></span>
<span data-ttu-id="1557f-190">hello CSV fájl forrásbeállítás importáló lehetővé teszi, hogy Ön tooimport egy vagy több CSV-fájlt.</span><span class="sxs-lookup"><span data-stu-id="1557f-190">hello CSV file source importer option enables you tooimport one or more CSV files.</span></span> <span data-ttu-id="1557f-191">Az Importálás CSV-fájlokat tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="1557f-191">When adding folders that contain CSV files for import, you have hello option of recursively searching for files in subfolders.</span></span>

![Képernyőfelvétel a CSV-forrására vonatkozó beállítások - CSV tooJSON](media/import-data/csvsource.png)

<span data-ttu-id="1557f-193">Hasonló toohello SQL-forrás, beágyazási elválasztó tulajdonság hello lehet használt toocreate hierarchikus kapcsolat (alárendelt dokumentumok) importálása során.</span><span class="sxs-lookup"><span data-stu-id="1557f-193">Similar toohello SQL source, hello nesting separator property may be used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="1557f-194">Vegye figyelembe a következő CSV-fejléc és az adatsorok hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-194">Consider hello following CSV header row and data rows:</span></span>

![Képernyőfelvétel a fürt megosztott kötetei szolgáltatás minta rekordok - CSV tooJSON](./media/import-data/csvsample.png)

<span data-ttu-id="1557f-196">Vegye figyelembe például DomainInfo.Domain_Name és RedirectInfo.Redirecting hello aliasok.</span><span class="sxs-lookup"><span data-stu-id="1557f-196">Note hello aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="1557f-197">A beágyazási elválasztó megadásával '.', hello import eszközt DomainInfo hoz létre, és közben hello RedirectInfo aldokumentumok importálása.</span><span class="sxs-lookup"><span data-stu-id="1557f-197">By specifying a nesting separator of ‘.’, hello import tool will create DomainInfo and RedirectInfo subdocuments during hello import.</span></span> <span data-ttu-id="1557f-198">Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:</span><span class="sxs-lookup"><span data-stu-id="1557f-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="1557f-199">*{"DomainInfo": {"Tartománynév": "ACUS.GOV", "Domain_Name_Address": "http://www. ACUS.GOV"},"Szövetségi ügynökség":"Az Amerikai Egyesült Államokban hello felügyeleti konferencia","RedirectInfo": {"Átirányítása":"0","Redirect_Destination":" "},"id":"9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*</span><span class="sxs-lookup"><span data-stu-id="1557f-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of hello United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="1557f-200">hello import eszközt megpróbál tooinfer információja nem jegyzett értékek CSV-fájlok (idézőjelek közé zárt értékek mindig tekintendők karakterláncok).</span><span class="sxs-lookup"><span data-stu-id="1557f-200">hello import tool will attempt tooinfer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="1557f-201">Típusok sorrend hello azonosítja: szám, dátum és idő, logikai érték.</span><span class="sxs-lookup"><span data-stu-id="1557f-201">Types are identified in hello following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="1557f-202">Két más dolgokat toonote kapcsolatos CSV import van:</span><span class="sxs-lookup"><span data-stu-id="1557f-202">There are two other things toonote about CSV import:</span></span>

1. <span data-ttu-id="1557f-203">Alapértelmezés szerint nem jegyzett értékek mindig elrejti a program lapok és szóközt tartalmaz, idézőjelek közé zárt értékek őrződnek meg amíg-van.</span><span class="sxs-lookup"><span data-stu-id="1557f-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="1557f-204">Ez a viselkedés felülbírálható a hello vágást idézőjelek között értékek jelölőnégyzet vagy hello /s.TrimQuoted parancssori kapcsolóval együtt.</span><span class="sxs-lookup"><span data-stu-id="1557f-204">This behavior can be overridden with hello Trim quoted values checkbox or hello /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="1557f-205">Alapértelmezés szerint egy nem jegyzett null értéke null értékű.</span><span class="sxs-lookup"><span data-stu-id="1557f-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="1557f-206">Ez a viselkedés felülbírálható (azaz tekinti egy nem jegyzett null "null" karakterlánc) rendelkező hello Treat utasítás nem null értékű karakterlánc jelölőnégyzet vagy hello /s.NoUnquotedNulls parancssori kapcsolóval, jegyzett.</span><span class="sxs-lookup"><span data-stu-id="1557f-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with hello Treat unquoted NULL as string checkbox or hello /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="1557f-207">A CSV-importálási parancssori minta a következő:</span><span class="sxs-lookup"><span data-stu-id="1557f-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-208"><a id="AzureTableSource"></a>az Azure Table storage tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-208"><a id="AzureTableSource"></a>tooimport from Azure Table storage</span></span>
<span data-ttu-id="1557f-209">hello Azure Table storage forrásbeállítás importáló lehetővé teszi az egyes Azure Table storage táblából tooimport, és opcionálisan a hello tábla entitások toobe importált szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="1557f-209">hello Azure Table storage source importer option allows you tooimport from an individual Azure Table storage table and optionally filter hello table entities toobe imported.</span></span> <span data-ttu-id="1557f-210">Vegye figyelembe, hogy nem használható hello adatáttelepítési eszköz tooimport Azure Table storage adatok az Azure Cosmos DB hello tábla API való használatra.</span><span class="sxs-lookup"><span data-stu-id="1557f-210">Note that you cannot use hello Data Migration tool tooimport Azure Table storage data into Azure Cosmos DB for use with hello Table API.</span></span> <span data-ttu-id="1557f-211">Most támogatja az csak importálását tooAzure Cosmos DB hello DocumentDB API való használatra.</span><span class="sxs-lookup"><span data-stu-id="1557f-211">Only importing tooAzure Cosmos DB for use with hello DocumentDB API is supported at this time.</span></span>

![Képernyőfelvétel az Azure Table storage forrására vonatkozó beállítások](./media/import-data/azuretablesource.png)

<span data-ttu-id="1557f-213">hello hello Azure Table storage kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-213">hello format of hello Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="1557f-214">Használjon hello ellenőrizze parancs tooensure, amely az Azure Table storage példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-214">Use hello Verify command tooensure that hello Azure Table storage instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-215">Adja meg hello Azure hello nevét, amelyből adatok importálódnak tábla.</span><span class="sxs-lookup"><span data-stu-id="1557f-215">Enter hello name of hello Azure table from which data will be imported.</span></span> <span data-ttu-id="1557f-216">Opcionálisan megadhat egy [szűrő](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="1557f-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="1557f-217">hello Azure Table storage forrásbeállítás importáló alábbi további beállítások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="1557f-217">hello Azure Table storage source importer option has hello following additional options:</span></span>

1. <span data-ttu-id="1557f-218">Belső mezők szerepelhetnek</span><span class="sxs-lookup"><span data-stu-id="1557f-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="1557f-219">Az összes - mezők szerepelhetnek, az összes belső (PartitionKey, RowKey, vagy időbélyeg)</span><span class="sxs-lookup"><span data-stu-id="1557f-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="1557f-220">Nincs – összes belső mező kizárása</span><span class="sxs-lookup"><span data-stu-id="1557f-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="1557f-221">RowKey - csak a hello RowKey mező is</span><span class="sxs-lookup"><span data-stu-id="1557f-221">RowKey - Only include hello RowKey field</span></span>
2. <span data-ttu-id="1557f-222">Oszlopok kiválasztása</span><span class="sxs-lookup"><span data-stu-id="1557f-222">Select Columns</span></span>
   1. <span data-ttu-id="1557f-223">Az Azure Table storage szűrők nem támogatják a leképezések.</span><span class="sxs-lookup"><span data-stu-id="1557f-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="1557f-224">Ha azt szeretné, hogy tooonly importálása adott Azure Table entitás tulajdonságai, vegye fel őket a toohello Select Columns listája.</span><span class="sxs-lookup"><span data-stu-id="1557f-224">If you want tooonly import specific Azure Table entity properties, add them toohello Select Columns list.</span></span> <span data-ttu-id="1557f-225">Minden más entitás tulajdonságait a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="1557f-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="1557f-226">Az Azure Table storage egy parancssor minta tooimport itt található:</span><span class="sxs-lookup"><span data-stu-id="1557f-226">Here is a command line sample tooimport from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-227"><a id="DynamoDBSource"></a>az Amazon DynamoDB tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-227"><a id="DynamoDBSource"></a>tooimport from Amazon DynamoDB</span></span>
<span data-ttu-id="1557f-228">hello Amazon DynamoDB importáló forrásbeállítás lehetővé teszi az egyes Amazon DynamoDB táblából tooimport, és opcionálisan a hello entitások toobe importált szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="1557f-228">hello Amazon DynamoDB source importer option allows you tooimport from an individual Amazon DynamoDB table and optionally filter hello entities toobe imported.</span></span> <span data-ttu-id="1557f-229">Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.</span><span class="sxs-lookup"><span data-stu-id="1557f-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource1.png)

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="1557f-232">hello hello Amazon DynamoDB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-232">hello format of hello Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="1557f-233">Használjon hello ellenőrizze parancs tooensure, Amazon DynamoDB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-233">Use hello Verify command tooensure that hello Amazon DynamoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-234">Íme egy parancssor minta tooimport az Amazon DynamoDB:</span><span class="sxs-lookup"><span data-stu-id="1557f-234">Here is a command line sample tooimport from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="1557f-235"><a id="BlobImport"></a>az Azure Blob storage tooimport fájlok</span><span class="sxs-lookup"><span data-stu-id="1557f-235"><a id="BlobImport"></a>tooimport files from Azure Blob storage</span></span>
<span data-ttu-id="1557f-236">hello JSON-fájl, a MongoDB exportfájl és a fürt megosztott kötetei szolgáltatás fájl forrás importáló beállítások lehetővé teszik tooimport az Azure Blob storage egy vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="1557f-236">hello JSON file, MongoDB export file, and CSV file source importer options allow you tooimport one or more files from Azure Blob storage.</span></span> <span data-ttu-id="1557f-237">Adja meg a Blob-tároló URL-cím és a Fiókkulcsot, adjon meg egy reguláris kifejezést tooselect hello (oka) t tooimport.</span><span class="sxs-lookup"><span data-stu-id="1557f-237">After specifying a Blob container URL and Account Key, simply provide a regular expression tooselect hello file(s) tooimport.</span></span>

![Képernyőfelvétel a Blob fájl forrására vonatkozó beállítások](./media/import-data/blobsource.png)

<span data-ttu-id="1557f-239">Parancssori minta tooimport JSON-fájlokat az Azure Blob storage a következő:</span><span class="sxs-lookup"><span data-stu-id="1557f-239">Here is command line sample tooimport JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="1557f-240"><a id="DocumentDBSource"></a>egy Azure Cosmos DB DocumentDB API gyűjteményből tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-240"><a id="DocumentDBSource"></a>tooimport from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="1557f-241">hello Azure Cosmos DB adatforrás-importáló beállítást lehetővé teszi egy vagy több Azure Cosmos DB gyűjtemények tooimport adatait, és opcionálisan a dokumentumok lekérdezéssel szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="1557f-241">hello Azure Cosmos DB source importer option allows you tooimport data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Képernyőfelvétel az Azure Cosmos DB adatforrás-beállítások](./media/import-data/documentdbsource.png)

<span data-ttu-id="1557f-243">hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-243">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="1557f-244">hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-244">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="1557f-245">Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-245">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-246">egyetlen Azure Cosmos DB gyűjteményből tooimport adja meg, amelyből adatok importálódnak hello gyűjtemény hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1557f-246">tooimport from a single Azure Cosmos DB collection, enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="1557f-247">a több Azure Cosmos DB ügyfélgyűjteményekből tooimport adja meg a reguláris kifejezés toomatch egy vagy több gyűjtemény neve (pl. collection01 |} collection02 |} collection03).</span><span class="sxs-lookup"><span data-stu-id="1557f-247">tooimport from multiple Azure Cosmos DB collections, provide a regular expression toomatch one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="1557f-248">Ön nem kötelezően adja meg, vagy adjon meg egy fájlt, egy lekérdezés tooboth szűrő és hello adatok toobe importált alakul.</span><span class="sxs-lookup"><span data-stu-id="1557f-248">You may optionally specify, or provide a file for, a query tooboth filter and shape hello data toobe imported.</span></span>

> [!NOTE]
> <span data-ttu-id="1557f-249">Mivel a hello gyűjtemény mezőben reguláris kifejezések fogad el, ha importál egy egyetlen gyűjteményből, amelynek a neve reguláris kifejezés karaktereket tartalmaz, majd ezeket a karaktereket kell megjelölni ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="1557f-249">Since hello collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="1557f-250">hello Azure Cosmos DB adatforrás-importáló beállítást rendelkezik hello következő speciális beállítások:</span><span class="sxs-lookup"><span data-stu-id="1557f-250">hello Azure Cosmos DB source importer option has hello following advanced options:</span></span>

1. <span data-ttu-id="1557f-251">Belső mezők szerepelhetnek: Határozza meg, függetlenül attól, tooinclude Azure Cosmos DB rendszer dokumentumtulajdonságok hello az Exportálás (pl. _rid, _ts).</span><span class="sxs-lookup"><span data-stu-id="1557f-251">Include Internal Fields: Specifies whether or not tooinclude Azure Cosmos DB document system properties in hello export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="1557f-252">Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-252">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="1557f-253">Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="1557f-253">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="1557f-254">Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-254">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="1557f-255">hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró.</span><span class="sxs-lookup"><span data-stu-id="1557f-255">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="1557f-256">hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="1557f-256">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB adatforrás speciális beállítások](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="1557f-258">hello eszköz alapértelmezett tooconnection mód DirectTcp importálni.</span><span class="sxs-lookup"><span data-stu-id="1557f-258">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="1557f-259">Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1557f-259">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="1557f-260">Az alábbiakban néhány parancssor minták tooimport az Azure Cosmos Adatbázisból:</span><span class="sxs-lookup"><span data-stu-id="1557f-260">Here are some command line samples tooimport from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="1557f-261">hello Azure Cosmos DB adatok Import eszközt is támogat hello származó adatok importálása [Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-261">hello Azure Cosmos DB Data Import Tool also supports import of data from hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="1557f-262">Amikor adatokat importál egy helyi emulátor, állítsa be az hello végpont túl`https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="1557f-262">When importing data from a local emulator, set hello endpoint too`https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="1557f-263"><a id="HBaseSource"></a>a HBase tooimport</span><span class="sxs-lookup"><span data-stu-id="1557f-263"><a id="HBaseSource"></a>tooimport from HBase</span></span>
<span data-ttu-id="1557f-264">hello HBase importáló forrásbeállítás lehetővé teszi egy HBase tábla tooimport adatait, és opcionálisan szűrje hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="1557f-264">hello HBase source importer option allows you tooimport data from an HBase table and optionally filter hello data.</span></span> <span data-ttu-id="1557f-265">Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.</span><span class="sxs-lookup"><span data-stu-id="1557f-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource1.png)

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource2.png)

<span data-ttu-id="1557f-268">hello hello HBase Stargate kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-268">hello format of hello HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="1557f-269">Használjon hello ellenőrizze parancs tooensure, amely a HBase példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-269">Use hello Verify command tooensure that hello HBase instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-270">A HBase egy parancssor minta tooimport itt található:</span><span class="sxs-lookup"><span data-stu-id="1557f-270">Here is a command line sample tooimport from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="1557f-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB API (tömeges importálással)</span><span class="sxs-lookup"><span data-stu-id="1557f-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="1557f-272">hello Azure Cosmos DB tömeges importáló lehetővé teszi a tooimport bármelyik hello elérhető forrására vonatkozó beállítások, egy Azure Cosmos DB tárolt eljárás használatával hatékonyságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="1557f-272">hello Azure Cosmos DB Bulk importer allows you tooimport from any of hello available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="1557f-273">hello eszköz támogatja a tooone egy particionált Azure Cosmos DB importgyűjteményhez, valamint a szilánkos importálása, amelynek során az adatok több egy particionált Azure Cosmos DB gyűjtemények között particionált van.</span><span class="sxs-lookup"><span data-stu-id="1557f-273">hello tool supports import tooone single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="1557f-274">Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="1557f-275">hello eszköz létrehozása, hajtható végre, és törölje a hello cél következő gyűjtemény(ek) készleteit szinkronizálja hello tárolt eljárás.</span><span class="sxs-lookup"><span data-stu-id="1557f-275">hello tool will create, execute, and then delete hello stored procedure from hello target collection(s).</span></span>  

![Képernyőfelvétel az Azure Cosmos DB tömeges beállítások](./media/import-data/documentdbbulk.png)

<span data-ttu-id="1557f-277">hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-277">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="1557f-278">hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-278">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="1557f-279">Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-279">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-280">tooimport tooa egyetlen gyűjtemény, adjon meg hello hello gyűjtemény toowhich adatok Importálja, majd kattintson hello Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="1557f-280">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="1557f-281">tooimport toomultiple gyűjtemények, adjon meg külön-külön minden gyűjtemény nevét, vagy használja a következő szintaxist toospecify hello több gyűjteményt: *collection_prefix*[kezdő index - end index].</span><span class="sxs-lookup"><span data-stu-id="1557f-281">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="1557f-282">Hello fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe hello a következőket:</span><span class="sxs-lookup"><span data-stu-id="1557f-282">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="1557f-283">Csak az egész tartomány neve minták támogatottak.</span><span class="sxs-lookup"><span data-stu-id="1557f-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="1557f-284">Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket hello: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="1557f-284">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="1557f-285">Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.</span><span class="sxs-lookup"><span data-stu-id="1557f-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="1557f-286">Egynél több helyettesítési megadható.</span><span class="sxs-lookup"><span data-stu-id="1557f-286">More than one substitution can be provided.</span></span> <span data-ttu-id="1557f-287">Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).</span><span class="sxs-lookup"><span data-stu-id="1557f-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="1557f-288">Miután hello gyűjtemény neve van megadva, válassza ki a kívánt átviteli hello hello következő gyűjtemény(ek) készleteit szinkronizálja (400 RUs too10, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="1557f-288">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too10,000 RUs).</span></span> <span data-ttu-id="1557f-289">Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="1557f-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="1557f-290">Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1557f-291">hello teljesítmény átviteli beállítás csak akkor érvényes toocollection létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1557f-291">hello performance throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="1557f-292">Hello megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="1557f-292">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="1557f-293">Toomultiple gyűjtemények importálásakor hello import eszközt a kivonat-alapú horizontális támogatja.</span><span class="sxs-lookup"><span data-stu-id="1557f-293">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="1557f-294">Ebben az esetben adja meg, partíciós kulcs hello toouse kívánja hello dokumentumtulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen hello cél gyűjtemények között).</span><span class="sxs-lookup"><span data-stu-id="1557f-294">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="1557f-295">Opcionálisan megadhat melyik hello importálási forrás mezője hello importálás (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd hello importálási eszköz létrehoz egy GUID hello azonosító tulajdonság értékeként) alatt kell használható hello Azure Cosmos DB dokumentum id tulajdonságának.</span><span class="sxs-lookup"><span data-stu-id="1557f-295">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="1557f-296">Nincsenek speciális beállítások számos elérhető importálása során.</span><span class="sxs-lookup"><span data-stu-id="1557f-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="1557f-297">Először amíg hello eszköz tartalmazza a alapértelmezett tömeges importálásához tárolt eljárás (BulkInsert.js), illetve előfordulhat, hogy toospecify saját importálási tárolt eljárás:</span><span class="sxs-lookup"><span data-stu-id="1557f-297">First, while hello tool includes a default bulk import stored procedure (BulkInsert.js), you may choose toospecify your own import stored procedure:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB tömeges beszúrási sproc beállítást](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="1557f-299">Emellett dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1557f-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="1557f-301">Karakterlánc: Egy karakterláncértéket áll fenn</span><span class="sxs-lookup"><span data-stu-id="1557f-301">String: Persist as a string value</span></span>
* <span data-ttu-id="1557f-302">Epoch: Egy érték szám Epoch áll fenn</span><span class="sxs-lookup"><span data-stu-id="1557f-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="1557f-303">Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit.</span><span class="sxs-lookup"><span data-stu-id="1557f-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="1557f-304">Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="1557f-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="1557f-305">hello Azure Cosmos DB tömeges importáló rendelkezik hello következő további speciális beállítások:</span><span class="sxs-lookup"><span data-stu-id="1557f-305">hello Azure Cosmos DB Bulk importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="1557f-306">Köteg mérete: hello eszköz alapértelmezett tooa köteg mérete 50.</span><span class="sxs-lookup"><span data-stu-id="1557f-306">Batch Size: hello tool defaults tooa batch size of 50.</span></span>  <span data-ttu-id="1557f-307">Ha hello dokumentumok toobe importált nagy, fontolja meg hello a köteg méretének csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="1557f-307">If hello documents toobe imported are large, consider lowering hello batch size.</span></span> <span data-ttu-id="1557f-308">Ezzel szemben ha importált hello dokumentumok toobe kicsi, fontolja meg hello a köteg méretének emelés.</span><span class="sxs-lookup"><span data-stu-id="1557f-308">Conversely, if hello documents toobe imported are small, consider raising hello batch size.</span></span>
2. <span data-ttu-id="1557f-309">Max. parancsfájl mérete (bájt): hello eszköz alapértelmezés szerint használt érték tooa maximális parancsfájl 512 KB-os méret</span><span class="sxs-lookup"><span data-stu-id="1557f-309">Max Script Size (bytes): hello tool defaults tooa max script size of 512KB</span></span>
3. <span data-ttu-id="1557f-310">Tiltsa le automatikus azonosító létrehozása: Ha minden importált dokumentum toobe azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="1557f-310">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="1557f-311">Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="1557f-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="1557f-312">Frissítés meglévő dokumentumok: hello eszköz alapértelmezett toonot meglévő dokumentumok cseréje ütközését.</span><span class="sxs-lookup"><span data-stu-id="1557f-312">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="1557f-313">Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="1557f-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="1557f-314">A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="1557f-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="1557f-315">Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-315">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="1557f-316">Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="1557f-316">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="1557f-317">Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-317">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="1557f-318">hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró.</span><span class="sxs-lookup"><span data-stu-id="1557f-318">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="1557f-319">hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="1557f-319">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB tömeges importálással speciális beállítások](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="1557f-321">hello eszköz alapértelmezett tooconnection mód DirectTcp importálni.</span><span class="sxs-lookup"><span data-stu-id="1557f-321">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="1557f-322">Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1557f-322">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="1557f-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB API (egymást követő rekord importálása)</span><span class="sxs-lookup"><span data-stu-id="1557f-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="1557f-324">hello Azure Cosmos DB szekvenciális rekord importáló lehetővé teszi a hello az elérhető forráskiszolgálók beállításokat rekord által rekord alapon tooimport.</span><span class="sxs-lookup"><span data-stu-id="1557f-324">hello Azure Cosmos DB sequential record importer allows you tooimport from any of hello available source options on a record by record basis.</span></span> <span data-ttu-id="1557f-325">Akkor célszerű használni ezt a beállítást, ha importál tooan meglévő gyűjteményt, amely elérte a tárolt eljárások vonatkozó kvótáját.</span><span class="sxs-lookup"><span data-stu-id="1557f-325">You might choose this option if you’re importing tooan existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="1557f-326">hello eszköz támogatja importálási tooa egyetlen Azure Cosmos DB gyűjtemény (egypartíciós és több partíció is), valamint a szilánkos importálása, amelynek során az adatok több egypartíciós és/vagy több partíció Azure Cosmos DB gyűjtemények között particionált van.</span><span class="sxs-lookup"><span data-stu-id="1557f-326">hello tool supports import tooa single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="1557f-327">Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálási beállítások](./media/import-data/documentdbsequential.png)

<span data-ttu-id="1557f-329">hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="1557f-329">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="1557f-330">hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1557f-330">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="1557f-331">Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1557f-331">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="1557f-332">tooimport tooa egyetlen gyűjtemény, adjon meg hello hello gyűjtemény toowhich adatok Importálja, majd kattintson hello Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="1557f-332">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="1557f-333">tooimport toomultiple gyűjtemények, adjon meg külön-külön minden gyűjtemény nevét, vagy használja a következő szintaxist toospecify hello több gyűjteményt: *collection_prefix*[kezdő index - end index].</span><span class="sxs-lookup"><span data-stu-id="1557f-333">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="1557f-334">Hello fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe hello a következőket:</span><span class="sxs-lookup"><span data-stu-id="1557f-334">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="1557f-335">Csak az egész tartomány neve minták támogatottak.</span><span class="sxs-lookup"><span data-stu-id="1557f-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="1557f-336">Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket hello: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="1557f-336">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="1557f-337">Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.</span><span class="sxs-lookup"><span data-stu-id="1557f-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="1557f-338">Egynél több helyettesítési megadható.</span><span class="sxs-lookup"><span data-stu-id="1557f-338">More than one substitution can be provided.</span></span> <span data-ttu-id="1557f-339">Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).</span><span class="sxs-lookup"><span data-stu-id="1557f-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="1557f-340">Miután hello gyűjtemény neve van megadva, válassza ki a kívánt átviteli hello hello következő gyűjtemény(ek) készleteit szinkronizálja (400 RUs too250, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="1557f-340">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too250,000 RUs).</span></span> <span data-ttu-id="1557f-341">Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="1557f-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="1557f-342">Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="1557f-343">Bármely importálása átviteli toocollections > 10 000 RUs partíciós kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="1557f-343">Any import toocollections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="1557f-344">Ha úgy dönt, toohave több mint 250 000 RUs, szüksége lesz a kérést hello portál toohave fiókja nagyobb toofile.</span><span class="sxs-lookup"><span data-stu-id="1557f-344">If you choose toohave more than 250,000 RUs, you will need toofile a request in hello portal toohave your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="1557f-345">hello átviteli beállítás csak akkor érvényes toocollection létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1557f-345">hello throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="1557f-346">Hello megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="1557f-346">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="1557f-347">Toomultiple gyűjtemények importálásakor hello import eszközt a kivonat-alapú horizontális támogatja.</span><span class="sxs-lookup"><span data-stu-id="1557f-347">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="1557f-348">Ebben az esetben adja meg, partíciós kulcs hello toouse kívánja hello dokumentumtulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen hello cél gyűjtemények között).</span><span class="sxs-lookup"><span data-stu-id="1557f-348">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="1557f-349">Opcionálisan megadhat melyik hello importálási forrás mezője hello importálás (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd hello importálási eszköz létrehoz egy GUID hello azonosító tulajdonság értékeként) alatt kell használható hello Azure Cosmos DB dokumentum id tulajdonságának.</span><span class="sxs-lookup"><span data-stu-id="1557f-349">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="1557f-350">Nincsenek speciális beállítások számos elérhető importálása során.</span><span class="sxs-lookup"><span data-stu-id="1557f-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="1557f-351">Első lépésként dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1557f-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="1557f-353">Karakterlánc: Egy karakterláncértéket áll fenn</span><span class="sxs-lookup"><span data-stu-id="1557f-353">String: Persist as a string value</span></span>
* <span data-ttu-id="1557f-354">Epoch: Egy érték szám Epoch áll fenn</span><span class="sxs-lookup"><span data-stu-id="1557f-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="1557f-355">Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit.</span><span class="sxs-lookup"><span data-stu-id="1557f-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="1557f-356">Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="1557f-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="1557f-357">hello Azure Cosmos DB - szekvenciális rekord importáló rendelkezik hello következő további speciális beállítások:</span><span class="sxs-lookup"><span data-stu-id="1557f-357">hello Azure Cosmos DB - Sequential record importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="1557f-358">Párhuzamos kérelem: hello eszköz alapértelmezés szerint használt érték too2 párhuzamos kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="1557f-358">Number of Parallel Requests: hello tool defaults too2 parallel requests.</span></span> <span data-ttu-id="1557f-359">Ha hello dokumentumok toobe importált kicsi, fontolja meg hello száma párhuzamos kérelem kiváltása.</span><span class="sxs-lookup"><span data-stu-id="1557f-359">If hello documents toobe imported are small, consider raising hello number of parallel requests.</span></span> <span data-ttu-id="1557f-360">Vegye figyelembe, hogy ez a szám túl sok következik be, ha hello importálási problémákat tapasztalhat a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="1557f-360">Note that if this number is raised too much, hello import may experience throttling.</span></span>
2. <span data-ttu-id="1557f-361">Tiltsa le automatikus azonosító létrehozása: Ha minden importált dokumentum toobe azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="1557f-361">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="1557f-362">Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="1557f-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="1557f-363">Frissítés meglévő dokumentumok: hello eszköz alapértelmezett toonot meglévő dokumentumok cseréje ütközését.</span><span class="sxs-lookup"><span data-stu-id="1557f-363">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="1557f-364">Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="1557f-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="1557f-365">A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="1557f-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="1557f-366">Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-366">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="1557f-367">Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="1557f-367">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="1557f-368">Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1557f-368">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="1557f-369">hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró.</span><span class="sxs-lookup"><span data-stu-id="1557f-369">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="1557f-370">hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="1557f-370">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálása speciális beállítások](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="1557f-372">hello eszköz alapértelmezett tooconnection mód DirectTcp importálni.</span><span class="sxs-lookup"><span data-stu-id="1557f-372">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="1557f-373">Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1557f-373">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="1557f-374"><a id="IndexingPolicy"></a>Adjon meg egy indexelési házirendet, amikor Azure Cosmos DB gyűjtemények létrehozása</span><span class="sxs-lookup"><span data-stu-id="1557f-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="1557f-375">Ha engedélyezi a hello áttelepítési eszköz toocreate gyűjtemények importálása során, az indexelési házirendet hello hello gyűjtemények is megadhat.</span><span class="sxs-lookup"><span data-stu-id="1557f-375">When you allow hello migration tool toocreate collections during import, you can specify hello indexing policy of hello collections.</span></span> <span data-ttu-id="1557f-376">Speciális beállítások szakasz hello Azure Cosmos DB tömeges importálással és Azure Cosmos DB szekvenciális rekord beállítások hello lépjen a toohello indexelő házirend szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1557f-376">In hello advanced options section of hello Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate toohello Indexing Policy section.</span></span>

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="1557f-378">Hello speciális indexelő házirend-beállítást használja, akkor is indexelési házirend fájlt válasszon ki, manuálisan adja meg az indexelési házirendet, vagy kattintva válassza ki az alapértelmezett sablonok közül (jobb gombbal a házirend szövegmező indexelő hello).</span><span class="sxs-lookup"><span data-stu-id="1557f-378">Using hello Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in hello indexing policy textbox).</span></span>

<span data-ttu-id="1557f-379">hello sablonok hello eszközt biztosít a következők:</span><span class="sxs-lookup"><span data-stu-id="1557f-379">hello policy templates hello tool provides are:</span></span>

* <span data-ttu-id="1557f-380">Alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="1557f-380">Default.</span></span> <span data-ttu-id="1557f-381">Ez a házirend akkor ajánlott, ha Ön karakterláncok egyenlőséglekérdezéséhez végrehajtása és a számok ORDER BY, tartomány és egyenlőség lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="1557f-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="1557f-382">Ez a házirend rendelkezik egy alacsonyabb indextárolási terheléssel jár mint tartomány.</span><span class="sxs-lookup"><span data-stu-id="1557f-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="1557f-383">Tartomány.</span><span class="sxs-lookup"><span data-stu-id="1557f-383">Range.</span></span> <span data-ttu-id="1557f-384">Ezzel a házirend-érdemes a számok és karakterláncok ORDER BY, tartomány-és egyenlőséglekérdezéséhez használ.</span><span class="sxs-lookup"><span data-stu-id="1557f-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="1557f-385">Ez a házirend egy magasabb indextárolási terheléssel jár, mint az alapértelmezett vagy az ujjlenyomat-rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1557f-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="1557f-387">Ha nem adja meg az indexelési házirendet, majd hello alapértelmezett házirend lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="1557f-387">If you do not specify an indexing policy, then hello default policy will be applied.</span></span> <span data-ttu-id="1557f-388">Az indexelő házirendekkel kapcsolatos további információkért lásd: [Azure Cosmos DB házirendek indexelő](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1557f-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-toojson-file"></a><span data-ttu-id="1557f-389">TooJSON fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="1557f-389">Export tooJSON file</span></span>
<span data-ttu-id="1557f-390">hello Azure Cosmos DB JSON-exportáló tooexport lehetővé teszi bármely hello az elérhető forráskiszolgálók beállítások tooa JSON-fájl, amely tartalmazza a JSON-dokumentumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="1557f-390">hello Azure Cosmos DB JSON exporter allows you tooexport any of hello available source options tooa JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="1557f-391">hello eszköz hello exportálási meg fogja kezelni, vagy választhat tooview hello eredményül kapott áttelepítési parancsot, és saját kezűleg hello parancs futtatása.</span><span class="sxs-lookup"><span data-stu-id="1557f-391">hello tool will handle hello export for you, or you can choose tooview hello resulting migration command and run hello command yourself.</span></span> <span data-ttu-id="1557f-392">hello letöltött JSON-fájlt helyben vagy az Azure Blob storage tárolhatók.</span><span class="sxs-lookup"><span data-stu-id="1557f-392">hello resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Az Azure Cosmos DB JSON képernyőkép helyi fájl exportálási beállítás](./media/import-data/jsontarget.png)

![Képernyőkép Azure Cosmos DB JSON Azure Blob storage exportálási beállítás](./media/import-data/jsontarget2.png)

<span data-ttu-id="1557f-395">Úgy is dönthet, JSON, ami növeli a hello eredményül kapott dokumentum hello mérete közben végzett hello tartalom olvasható további emberi eredő tooprettify hello nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="1557f-395">You may optionally choose tooprettify hello resulting JSON, which will increase hello size of hello resulting document while making hello contents more human readable.</span></span>

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places toosee in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places toosee in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a><span data-ttu-id="1557f-396">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="1557f-396">Advanced configuration</span></span>
<span data-ttu-id="1557f-397">Hello speciális konfigurálására szolgáló képernyőn adja meg a hello napló fájl toowhich írt hibák milyen hello elhelyezkedését.</span><span class="sxs-lookup"><span data-stu-id="1557f-397">In hello Advanced configuration screen, specify hello location of hello log file toowhich you would like any errors written.</span></span> <span data-ttu-id="1557f-398">a következő szabályok hello toothis lap vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="1557f-398">hello following rules apply toothis page:</span></span>

1. <span data-ttu-id="1557f-399">Ha egy fájl neve nem áll rendelkezésre, majd a program az összes hiba visszatér hello eredmények lapon.</span><span class="sxs-lookup"><span data-stu-id="1557f-399">If a file name is not provided, then all errors will be returned on hello Results page.</span></span>
2. <span data-ttu-id="1557f-400">Ha a fájl nevét a címtár nélkül áll rendelkezésre, majd hello fájl létrejön (vagy felül) hello aktuális környezet könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="1557f-400">If a file name is provided without a directory, then hello file will be created (or overwritten) in hello current environment directory.</span></span>
3. <span data-ttu-id="1557f-401">Ha egy meglévő fájlt, majd hello fájl felül lesznek írva, nincs append beállítás.</span><span class="sxs-lookup"><span data-stu-id="1557f-401">If you select an existing file, then hello file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="1557f-402">Ezután válassza e toolog minden, a kritikus, vagy hibaüzeneteket.</span><span class="sxs-lookup"><span data-stu-id="1557f-402">Then, choose whether toolog all, critical, or no error messages.</span></span> <span data-ttu-id="1557f-403">Végül döntse el, hogy milyen gyakran a képernyő-átviteli üzenetet hello frissíti a rendszer a telepítés előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="1557f-403">Finally, decide how frequently hello on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="1557f-404">Erősítse meg a beállításokat importálhat és parancs sor megtekintése</span><span class="sxs-lookup"><span data-stu-id="1557f-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="1557f-405">Adja meg az adatforrásra vonatkozó információ, a cél adatokat és a speciális konfigurációs, áttekintheti hello áttelepítési összefoglaló, és szükség esetén megtekintése és másolása hello eredő áttelepítési parancs (Másolás hello parancs hasznos tooautomate importálási műveletek):</span><span class="sxs-lookup"><span data-stu-id="1557f-405">After specifying source information, target information, and advanced configuration, review hello migration summary and, optionally, view/copy hello resulting migration command (copying hello command is useful tooautomate import operations):</span></span>
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summary.png)
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summarycommand.png)
2. <span data-ttu-id="1557f-408">Ha elégedett a forrás- és beállításokat, kattintson **importálási**.</span><span class="sxs-lookup"><span data-stu-id="1557f-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="1557f-409">hello eltelt idő, átvitt száma és (Ha nem adott meg a fájl nevét a hello speciális konfigurációs) hibaadatokat frissíti hello-import van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="1557f-409">hello elapsed time, transferred count, and failure information (if you didn't provide a file name in hello Advanced configuration) will update as hello import is in process.</span></span> <span data-ttu-id="1557f-410">Művelet befejeződése után exportálhatja hello eredmények (pl. bármely importálás hibákkal toodeal).</span><span class="sxs-lookup"><span data-stu-id="1557f-410">Once complete, you can export hello results (e.g. toodeal with any import failures).</span></span>
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/viewresults.png)
3. <span data-ttu-id="1557f-412">Egy új importálhat indulhat tartani a meglévő beállítások hello (pl. kapcsolati karakterlánc adatokat, a forrás és cél kiválasztása, stb.), vagy alaphelyzetbe állítása az összes értéket.</span><span class="sxs-lookup"><span data-stu-id="1557f-412">You may also start a new import, either keeping hello existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="1557f-414">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1557f-414">Next steps</span></span>

<span data-ttu-id="1557f-415">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="1557f-415">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1557f-416">Hello adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="1557f-416">Installed hello Data Migration tool</span></span>
> * <span data-ttu-id="1557f-417">Különböző adatforrásokból importált adatok</span><span class="sxs-lookup"><span data-stu-id="1557f-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="1557f-418">Exportált Azure Cosmos DB tooJSON</span><span class="sxs-lookup"><span data-stu-id="1557f-418">Exported from Azure Cosmos DB tooJSON</span></span>

<span data-ttu-id="1557f-419">Ezután folytassa a következő oktatóanyag toohello és megtudhatja, hogyan tooquery adatok Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="1557f-419">You can now proceed toohello next tutorial and learn how tooquery data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="1557f-420">Hogyan tooquery adatokat?</span><span class="sxs-lookup"><span data-stu-id="1557f-420">How tooquery data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
