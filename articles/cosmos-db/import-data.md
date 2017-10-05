---
title: "Adatbázis-áttelepítési eszköz Azure Cosmos DB |} Microsoft Docs"
description: "Útmutató: adatok importálása az Azure Cosmos Adatbázishoz különböző forrásokból, beleértve a MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV és JSON fájlokat a nyílt forráskódú Azure Cosmos DB adatok áttelepítési eszközök segítségével. A fürt megosztott kötetei szolgáltatás JSON alakításához."
keywords: "fürt megosztott kötetei szolgáltatás JSON, adatbázis-áttelepítési eszközök, csv konvertálása json"
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
ms.openlocfilehash: 23a4a82dbdb611f4da90562af936fca28da9b24d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-data-into-azure-cosmos-db-for-the-documentdb-api"></a><span data-ttu-id="a569b-105">Adatok importálása az Azure Cosmos DB a DocumentDB API hogyan?</span><span class="sxs-lookup"><span data-stu-id="a569b-105">How to import data into Azure Cosmos DB for the DocumentDB API?</span></span>

<span data-ttu-id="a569b-106">Ez az oktatóanyag az Azure Cosmos DB használatával kapcsolatos utasításokat tartalmazza: a DocumentDB API adatáttelepítés eszközt, amely adatokat importálhat különböző móddal, többek között a JSON-fájlokat, CSV-fájlok, SQL, MongoDB, az Azure Table storage, Amazon DynamoDB és Azure Cosmos DB DocumentDB API gyűjtemények gyűjteményekbe Azure Cosmos adatbázis és a DocumentDB API-val való használatra.</span><span class="sxs-lookup"><span data-stu-id="a569b-106">This tutorial provides instructions on using the Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and the DocumentDB API.</span></span> <span data-ttu-id="a569b-107">Az adatok áttelepítési eszköz is használható, áttelepítésekor az egypartíciós gyűjtemény több partíció gyűjteménybe a DocumentDB az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="a569b-107">The Data Migration tool can also be used when migrating from a single partition collection to a multi-partition collection for the DocumentDB API.</span></span>

<span data-ttu-id="a569b-108">Az adatok áttelepítési eszköz csak akkor működik, amikor Azure Cosmos DB importáló adatokat a DocumentDB API-t használja.</span><span class="sxs-lookup"><span data-stu-id="a569b-108">The Data Migration tool only works when importing data into Azure Cosmos DB for use with the DocumentDB API.</span></span> <span data-ttu-id="a569b-109">A tábla API és a Graph API adatok importálása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a569b-109">Importing data for use with the Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="a569b-110">Adatimportálás a MongoDB API-val való használatra, lásd: [Azure Cosmos DB: adatok áttelepítése a MongoDB API?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-110">To import data for use with the MongoDB API, see [Azure Cosmos DB: How to migrate data for the MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="a569b-111">Ez az oktatóanyag ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="a569b-111">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a569b-112">Az adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="a569b-112">Installing the Data Migration tool</span></span>
> * <span data-ttu-id="a569b-113">Különböző forrásokból származó adatok importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="a569b-114">A JSON Azure Cosmos DB exportálása</span><span class="sxs-lookup"><span data-stu-id="a569b-114">Exporting from Azure Cosmos DB to JSON</span></span>

## <span data-ttu-id="a569b-115"><a id="Prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a569b-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="a569b-116">Mielőtt Ez a cikk utasításait követve ellenőrizze, hogy a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="a569b-116">Before following the instructions in this article, ensure that you have the following installed:</span></span>

* <span data-ttu-id="a569b-117">[A Microsoft .NET-keretrendszer 4.51](https://www.microsoft.com/download/developer-tools.aspx) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a569b-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="a569b-118"><a id="Overviewl"></a>Az adatok áttelepítési eszköz áttekintése</span><span class="sxs-lookup"><span data-stu-id="a569b-118"><a id="Overviewl"></a>Overview of the Data Migration tool</span></span>
<span data-ttu-id="a569b-119">Az adatok áttelepítési eszköz egy nyílt forráskódú megoldás, amellyel különféle adatok Azure Cosmos DB móddal, többek között számos különböző:</span><span class="sxs-lookup"><span data-stu-id="a569b-119">The Data Migration tool is an open source solution that imports data to Azure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="a569b-120">JSON-fájlok</span><span class="sxs-lookup"><span data-stu-id="a569b-120">JSON files</span></span>
* <span data-ttu-id="a569b-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a569b-121">MongoDB</span></span>
* <span data-ttu-id="a569b-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a569b-122">SQL Server</span></span>
* <span data-ttu-id="a569b-123">CSV-fájlok</span><span class="sxs-lookup"><span data-stu-id="a569b-123">CSV files</span></span>
* <span data-ttu-id="a569b-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="a569b-124">Azure Table storage</span></span>
* <span data-ttu-id="a569b-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="a569b-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="a569b-126">HBase</span><span class="sxs-lookup"><span data-stu-id="a569b-126">HBase</span></span>
* <span data-ttu-id="a569b-127">Az Azure Cosmos DB gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="a569b-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="a569b-128">Amíg az importálási eszköz tartalmazza a grafikus felhasználói felületen (dtui.exe), azt is is vezeti a parancssorból (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="a569b-128">While the import tool includes a graphical user interface (dtui.exe), it can also be driven from the command line (dt.exe).</span></span> <span data-ttu-id="a569b-129">Valójában nincs lehetősége van arra beállítása a felhasználói felületen az importálás után a hozzárendelt parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="a569b-129">In fact, there is an option to output the associated command after setting up an import through the UI.</span></span> <span data-ttu-id="a569b-130">Táblázatos forrásadatok (pl. az SQL Server vagy CSV-fájlokban) is kell alakítani, úgy, hogy az importálás során hozható létre a is a hierarchikus kapcsolat (aldokumentumok).</span><span class="sxs-lookup"><span data-stu-id="a569b-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="a569b-131">Adatforrás-beállításokkal kapcsolatos további, a minta parancssor minden forrás, a cél lehetőségekkel, valamint a Megtekintés importálási eredmények importálása olvasási megtartása.</span><span class="sxs-lookup"><span data-stu-id="a569b-131">Keep reading to learn more about source options, sample command lines to import from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="a569b-132"><a id="Install"></a>Az adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="a569b-132"><a id="Install"></a>Install the Data Migration tool</span></span>
<span data-ttu-id="a569b-133">Az áttelepítési eszköz a forráskód nem elérhető a Githubon található [ebben a tárházban](https://github.com/azure/azure-documentdb-datamigrationtool) és egy lefordított elérhető a [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="a569b-133">The migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="a569b-134">Akkor lehet, hogy vagy a megoldás összeállításához vagy egyszerűen letöltéséhez és kibontásához a lefordított az Ön által választott könyvtárra.</span><span class="sxs-lookup"><span data-stu-id="a569b-134">You may either compile the solution or simply download and extract the compiled version to a directory of your choice.</span></span> <span data-ttu-id="a569b-135">Ezután futtassa vagy:</span><span class="sxs-lookup"><span data-stu-id="a569b-135">Then run either:</span></span>

* <span data-ttu-id="a569b-136">**Dtui.exe**: az eszköz grafikus felület verziója</span><span class="sxs-lookup"><span data-stu-id="a569b-136">**Dtui.exe**: Graphical interface version of the tool</span></span>
* <span data-ttu-id="a569b-137">**DT.exe**: az eszköz parancssori verziója</span><span class="sxs-lookup"><span data-stu-id="a569b-137">**Dt.exe**: Command-line version of the tool</span></span>

## <a name="import-data"></a><span data-ttu-id="a569b-138">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-138">Import data</span></span>

<span data-ttu-id="a569b-139">Az eszköz telepítését követően a rendszer az idő, importálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a569b-139">Once you've installed the tool, it's time to import your data.</span></span> <span data-ttu-id="a569b-140">Milyen típusú adatokat kívánja importálni?</span><span class="sxs-lookup"><span data-stu-id="a569b-140">What kind of data do you want to import?</span></span>

* [<span data-ttu-id="a569b-141">JSON-fájlok</span><span class="sxs-lookup"><span data-stu-id="a569b-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="a569b-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a569b-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="a569b-143">MongoDB exportfájlok</span><span class="sxs-lookup"><span data-stu-id="a569b-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="a569b-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a569b-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="a569b-145">CSV-fájlok</span><span class="sxs-lookup"><span data-stu-id="a569b-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="a569b-146">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="a569b-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="a569b-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="a569b-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="a569b-148">A BLOB</span><span class="sxs-lookup"><span data-stu-id="a569b-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="a569b-149">Az Azure Cosmos DB gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="a569b-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="a569b-150">HBase</span><span class="sxs-lookup"><span data-stu-id="a569b-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="a569b-151">Az Azure Cosmos DB tömeges importálással</span><span class="sxs-lookup"><span data-stu-id="a569b-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="a569b-152">Az Azure Cosmos DB szekvenciális rekord importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="a569b-153"><a id="JSON"></a>JSON-fájlok importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-153"><a id="JSON"></a>To import JSON files</span></span>
<span data-ttu-id="a569b-154">A JSON fájl forrás importáló beállítással importálása egy vagy több egyetlen dokumentum JSON-fájlokat vagy JSON-fájlokat, hogy minden egyes tartalmazza JSON-dokumentumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="a569b-154">The JSON file source importer option allows you to import one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="a569b-155">Importálása JSON-fájlokat tartalmazó mappák hozzáadásakor lehetősége van a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="a569b-155">When adding folders that contain JSON files to import, you have the option of recursively searching for files in subfolders.</span></span>

![Képernyőfelvétel a JSON fájl forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/jsonsource.png)

<span data-ttu-id="a569b-157">Az alábbiakban néhány parancssor minták JSON-fájlok importálása:</span><span class="sxs-lookup"><span data-stu-id="a569b-157">Here are some command line samples to import JSON files:</span></span>

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-158"><a id="MongoDB"></a>MongoDB importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-158"><a id="MongoDB"></a>To import from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a569b-159">Ha egy, a mongodb-Protokolltámogatással rendelkező Azure Cosmos DB fiókot importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-159">If you are importing to an Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="a569b-160">A MongoDB forrás importáló beállítás lehetővé teszi az egyes MongoDB-gyűjteményt importálása és opcionálisan szűrése lekérdezéssel dokumentumok és/vagy módosítani a dokumentumot szerkezetét leképezés használatával.</span><span class="sxs-lookup"><span data-stu-id="a569b-160">The MongoDB source importer option allows you to import from an individual MongoDB collection and optionally filter documents using a query and/or modify the document structure by using a projection.</span></span>  

![Képernyőfelvétel a MongoDB forrására vonatkozó beállítások](./media/import-data/mongodbsource.png)

<span data-ttu-id="a569b-162">A kapcsolati karakterláncot a szabványos MongoDB formátumban van:</span><span class="sxs-lookup"><span data-stu-id="a569b-162">The connection string is in the standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="a569b-163">Az ellenőrzés parancs segítségével győződjön meg arról, hogy elérhető-e a MongoDB-példány a kapcsolati karakterlánc mezőben megadott.</span><span class="sxs-lookup"><span data-stu-id="a569b-163">Use the Verify command to ensure that the MongoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-164">Adja meg, amelyből adatok Importálja a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="a569b-164">Enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="a569b-165">Szükség lehet, hogy adja meg, vagy adjon meg egy fájlt egy lekérdezés (pl. {pop: {$gt: 5000}}) és/vagy a leképezése (például {loc:0}) szűrő és az adatokat, importálandók alakul.</span><span class="sxs-lookup"><span data-stu-id="a569b-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) to both filter and shape the data to be imported.</span></span>

<span data-ttu-id="a569b-166">Az alábbiakban néhány parancssor minták MongoDB importálása:</span><span class="sxs-lookup"><span data-stu-id="a569b-166">Here are some command line samples to import from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="a569b-167"><a id="MongoDBExport"></a>MongoDB exportálási fájlok importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-167"><a id="MongoDBExport"></a>To import MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a569b-168">Ha egy, a mongodb-protokolltámogatással rendelkező Azure Cosmos DB fiókot importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-168">If you are importing to an Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="a569b-169">A MongoDB exportálási JSON fájl forrás importáló beállítás lehetővé teszi egy vagy több JSON-fájlokat a mongoexport segédprogram előállított importálását.</span><span class="sxs-lookup"><span data-stu-id="a569b-169">The MongoDB export JSON file source importer option allows you to import one or more JSON files produced from the mongoexport utility.</span></span>  

![Képernyőfelvétel a MongoDB exportálási forrására vonatkozó beállítások](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="a569b-171">MongoDB exportálási JSON-fájlok importálása tartalmazó mappák hozzáadásakor lehetősége van a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="a569b-171">When adding folders that contain MongoDB export JSON files for import, you have the option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="a569b-172">Egy parancssorban minta MongoDB exportálási JSON-fájlok importálása a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-172">Here is a command line sample to import from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-173"><a id="SQL"></a>SQL Server importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-173"><a id="SQL"></a>To import from SQL Server</span></span>
<span data-ttu-id="a569b-174">Az SQL-adatforrás-importáló beállítást lehetővé teszi egy egyéni SQL Server-adatbázis importálása, és igény szerint szűrheti a rekordokat, importálandók lekérdezés segítségével.</span><span class="sxs-lookup"><span data-stu-id="a569b-174">The SQL source importer option allows you to import from an individual SQL Server database and optionally filter the records to be imported using a query.</span></span> <span data-ttu-id="a569b-175">Emellett módosíthatja a dokumentumstruktúrával (további sablonokat, amelyek később) beágyazási elválasztó megadásával.</span><span class="sxs-lookup"><span data-stu-id="a569b-175">In addition, you can modify the document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Képernyőkép az SQL - adatbázis áttelepítési eszközök forrása](./media/import-data/sqlexportsource.png)

<span data-ttu-id="a569b-177">A kapcsolati karakterlánc formátuma a szokásos SQL kapcsolati karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="a569b-177">The format of the connection string is the standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="a569b-178">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott SQL Server-példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-178">Use the Verify command to ensure that the SQL Server instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-179">A beágyazási elválasztó tulajdonság importálása során (alárendelt dokumentumok) hierarchikus kapcsolat létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="a569b-179">The nesting separator property is used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="a569b-180">Vegye figyelembe a következő SQL-lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="a569b-180">Consider the following SQL query:</span></span>

<span data-ttu-id="a569b-181">*Válassza ki a TÍPUSKONVERZIÓ (BusinessEntityID AS varchar) azonosítója, neve, mint [Address.AddressType] AddressType, AddressLine1 [Address.AddressLine1], [Address.Location.City] városát, StateProvinceName [Address.Location.StateProvinceName], irányítószám szerint [Address.PostalCode], CountryRegionName [Address.CountryRegionName], a Sales.vStoreWithAddresses ahol AddressType = "Main Office"*</span><span class="sxs-lookup"><span data-stu-id="a569b-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="a569b-182">Amely a következő (részleges) eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="a569b-182">Which returns the following (partial) results:</span></span>

![Képernyőkép az SQL-lekérdezés eredményei](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="a569b-184">Vegye figyelembe például Address.AddressType és Address.Location.StateProvinceName aliasok.</span><span class="sxs-lookup"><span data-stu-id="a569b-184">Note the aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="a569b-185">A beágyazási elválasztó megadásával ".", az importálási eszköz cím és Address.Location aldokumentumok létrehozza az importálás során.</span><span class="sxs-lookup"><span data-stu-id="a569b-185">By specifying a nesting separator of ‘.’, the import tool creates Address and Address.Location subdocuments during the import.</span></span> <span data-ttu-id="a569b-186">Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:</span><span class="sxs-lookup"><span data-stu-id="a569b-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="a569b-187">*{"id": "956", "Name": "Egyeztetését értékesítés és a szolgáltatás", "Cím": {"AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor utca", "Hely": {"Város": "Ottawai", "StateProvinceName": "Ontario"}, "Irányítószám": "K4B 1S2", "CountryRegionName": "Kanada"}}*</span><span class="sxs-lookup"><span data-stu-id="a569b-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="a569b-188">Az alábbiakban néhány importálása az SQL Server parancssori minták:</span><span class="sxs-lookup"><span data-stu-id="a569b-188">Here are some command line samples to import from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-189"><a id="CSV"></a>Importálása CSV-fájlok és a fürt megosztott kötetei szolgáltatás átalakítása JSON</span><span class="sxs-lookup"><span data-stu-id="a569b-189"><a id="CSV"></a>To import CSV files and convert CSV to JSON</span></span>
<span data-ttu-id="a569b-190">A fürt megosztott kötetei szolgáltatás fájl forrás importáló beállítás lehetővé teszi egy vagy több CSV-fájl importálása.</span><span class="sxs-lookup"><span data-stu-id="a569b-190">The CSV file source importer option enables you to import one or more CSV files.</span></span> <span data-ttu-id="a569b-191">Az Importálás CSV-fájlokat tartalmazó mappák hozzáadásakor lehetősége van a rekurzív módon almappákban lévő fájlok keresése.</span><span class="sxs-lookup"><span data-stu-id="a569b-191">When adding folders that contain CSV files for import, you have the option of recursively searching for files in subfolders.</span></span>

![Képernyőfelvétel a CSV-forrására vonatkozó beállítások - JSON CSV](media/import-data/csvsource.png)

<span data-ttu-id="a569b-193">Az SQL-forrás hasonló, a beágyazási elválasztó tulajdonság lehetséges, hogy használható létrehozásához hierarchikus kapcsolat (alárendelt dokumentumok) importálása során.</span><span class="sxs-lookup"><span data-stu-id="a569b-193">Similar to the SQL source, the nesting separator property may be used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="a569b-194">Vegye figyelembe a következő CSV-fejléc sor és az adatok sorok:</span><span class="sxs-lookup"><span data-stu-id="a569b-194">Consider the following CSV header row and data rows:</span></span>

![Képernyőfelvétel a fürt megosztott kötetei szolgáltatás minta rekordok - JSON CSV](./media/import-data/csvsample.png)

<span data-ttu-id="a569b-196">Vegye figyelembe például DomainInfo.Domain_Name és RedirectInfo.Redirecting aliasok.</span><span class="sxs-lookup"><span data-stu-id="a569b-196">Note the aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="a569b-197">A beágyazási elválasztó megadásával ".", az importálási eszköz DomainInfo és RedirectInfo aldokumentumok hoz létre az importálás során.</span><span class="sxs-lookup"><span data-stu-id="a569b-197">By specifying a nesting separator of ‘.’, the import tool will create DomainInfo and RedirectInfo subdocuments during the import.</span></span> <span data-ttu-id="a569b-198">Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:</span><span class="sxs-lookup"><span data-stu-id="a569b-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="a569b-199">*{"DomainInfo": {"Tartománynév": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV"}, "Szövetségi ügynökség": "felügyeleti konferencia az Amerikai Egyesült államokbeli", "RedirectInfo": {"Átirányítása": "0", "Redirect_Destination": ""}, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*</span><span class="sxs-lookup"><span data-stu-id="a569b-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="a569b-200">Az import eszközt, amelyből megállapítható a CSV-fájlok nem jegyzett értékekre típusinformációt (idézőjelek közé zárt értékek mindig tekintendők karakterláncok) kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-200">The import tool will attempt to infer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="a569b-201">Típusok azonosítja a következő sorrendben: szám, dátum és idő, logikai érték.</span><span class="sxs-lookup"><span data-stu-id="a569b-201">Types are identified in the following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="a569b-202">Két dolgot más kapcsolatos CSV import figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="a569b-202">There are two other things to note about CSV import:</span></span>

1. <span data-ttu-id="a569b-203">Alapértelmezés szerint nem jegyzett értékek mindig elrejti a program lapok és szóközt tartalmaz, idézőjelek közé zárt értékek őrződnek meg amíg-van.</span><span class="sxs-lookup"><span data-stu-id="a569b-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="a569b-204">Ez a viselkedés felülbírálható a vágás idézőjelek közé zárt értékek jelölőnégyzet vagy /s.TrimQuoted parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="a569b-204">This behavior can be overridden with the Trim quoted values checkbox or the /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="a569b-205">Alapértelmezés szerint egy nem jegyzett null értéke null értékű.</span><span class="sxs-lookup"><span data-stu-id="a569b-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="a569b-206">Ez a viselkedés felülbírálható (azaz tekinti egy nem jegyzett null "null" karakterlánc) és a Treat utasítás nem jegyzett NULL karakterlánc jelölőnégyzet vagy /s.NoUnquotedNulls parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="a569b-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with the Treat unquoted NULL as string checkbox or the /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="a569b-207">A CSV-importálási parancssori minta a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-208"><a id="AzureTableSource"></a>Az Azure Table storage importálható</span><span class="sxs-lookup"><span data-stu-id="a569b-208"><a id="AzureTableSource"></a>To import from Azure Table storage</span></span>
<span data-ttu-id="a569b-209">Az Azure Table storage importáló forrásbeállítás lehetővé teszi az egyes Azure Table storage tábla importálása, és igény szerint szűrheti a táblaentitásokat, importálandók.</span><span class="sxs-lookup"><span data-stu-id="a569b-209">The Azure Table storage source importer option allows you to import from an individual Azure Table storage table and optionally filter the table entities to be imported.</span></span> <span data-ttu-id="a569b-210">Vegye figyelembe, hogy az adatáttelepítési eszköz nem használható az Azure Table storage adatok importálása az Azure Cosmos DB a tábla API-val való használatra.</span><span class="sxs-lookup"><span data-stu-id="a569b-210">Note that you cannot use the Data Migration tool to import Azure Table storage data into Azure Cosmos DB for use with the Table API.</span></span> <span data-ttu-id="a569b-211">Most támogatja a DocumentDB API-val való használatra Azure Cosmos DB csak importálását.</span><span class="sxs-lookup"><span data-stu-id="a569b-211">Only importing to Azure Cosmos DB for use with the DocumentDB API is supported at this time.</span></span>

![Képernyőfelvétel az Azure Table storage forrására vonatkozó beállítások](./media/import-data/azuretablesource.png)

<span data-ttu-id="a569b-213">Az Azure Table storage kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-213">The format of the Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="a569b-214">Az ellenőrzés parancs segítségével győződjön meg arról, hogy az Azure Table storage-példány a kapcsolati karakterlánc mezőben megadott elérhető.</span><span class="sxs-lookup"><span data-stu-id="a569b-214">Use the Verify command to ensure that the Azure Table storage instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-215">Adja meg a nevét, amelyről adatokat importálja az Azure tábla.</span><span class="sxs-lookup"><span data-stu-id="a569b-215">Enter the name of the Azure table from which data will be imported.</span></span> <span data-ttu-id="a569b-216">Opcionálisan megadhat egy [szűrő](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="a569b-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="a569b-217">Az Azure Table storage importáló beállítást az alábbi lehetőségekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a569b-217">The Azure Table storage source importer option has the following additional options:</span></span>

1. <span data-ttu-id="a569b-218">Belső mezők szerepelhetnek</span><span class="sxs-lookup"><span data-stu-id="a569b-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="a569b-219">Az összes - mezők szerepelhetnek, az összes belső (PartitionKey, RowKey, vagy időbélyeg)</span><span class="sxs-lookup"><span data-stu-id="a569b-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="a569b-220">Nincs – összes belső mező kizárása</span><span class="sxs-lookup"><span data-stu-id="a569b-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="a569b-221">RowKey - csak közé tartozik a RowKey mező</span><span class="sxs-lookup"><span data-stu-id="a569b-221">RowKey - Only include the RowKey field</span></span>
2. <span data-ttu-id="a569b-222">Oszlopok kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a569b-222">Select Columns</span></span>
   1. <span data-ttu-id="a569b-223">Az Azure Table storage szűrők nem támogatják a leképezések.</span><span class="sxs-lookup"><span data-stu-id="a569b-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="a569b-224">Ha csak az adott Azure Table Entitástulajdonságok importálni kívánt, vegye fel a Select Columns listára.</span><span class="sxs-lookup"><span data-stu-id="a569b-224">If you want to only import specific Azure Table entity properties, add them to the Select Columns list.</span></span> <span data-ttu-id="a569b-225">Minden más entitás tulajdonságait a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="a569b-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="a569b-226">Az Azure Table storage importálható parancssori minta a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-226">Here is a command line sample to import from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-227"><a id="DynamoDBSource"></a>Amazon DynamoDB importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-227"><a id="DynamoDBSource"></a>To import from Amazon DynamoDB</span></span>
<span data-ttu-id="a569b-228">Az Amazon DynamoDB forrás importáló beállítás lehetővé teszi az egyes Amazon DynamoDB tábla importálása, és opcionálisan szűrheti az entitásokat, importálandók.</span><span class="sxs-lookup"><span data-stu-id="a569b-228">The Amazon DynamoDB source importer option allows you to import from an individual Amazon DynamoDB table and optionally filter the entities to be imported.</span></span> <span data-ttu-id="a569b-229">Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.</span><span class="sxs-lookup"><span data-stu-id="a569b-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource1.png)

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="a569b-232">Az Amazon DynamoDB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-232">The format of the Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="a569b-233">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott Amazon DynamoDB példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-233">Use the Verify command to ensure that the Amazon DynamoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-234">Íme egy parancssor minta Amazon DynamoDB importálása:</span><span class="sxs-lookup"><span data-stu-id="a569b-234">Here is a command line sample to import from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="a569b-235"><a id="BlobImport"></a>Az Azure Blob storage fájlok importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-235"><a id="BlobImport"></a>To import files from Azure Blob storage</span></span>
<span data-ttu-id="a569b-236">A JSON-fájl, a MongoDB exportfájl és a fürt megosztott kötetei szolgáltatás fájl forrás importáló beállítások lehetővé teszik egy vagy több fájlt importálja az Azure Blob-tárolóból.</span><span class="sxs-lookup"><span data-stu-id="a569b-236">The JSON file, MongoDB export file, and CSV file source importer options allow you to import one or more files from Azure Blob storage.</span></span> <span data-ttu-id="a569b-237">Adja meg a Blob-tároló URL-cím és a Fiókkulcsot, adjon meg egy reguláris kifejezést az importálandó fájl kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a569b-237">After specifying a Blob container URL and Account Key, simply provide a regular expression to select the file(s) to import.</span></span>

![Képernyőfelvétel a Blob fájl forrására vonatkozó beállítások](./media/import-data/blobsource.png)

<span data-ttu-id="a569b-239">JSON-fájlok importálása az Azure Blob storage parancssori minta a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-239">Here is command line sample to import JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="a569b-240"><a id="DocumentDBSource"></a>Egy Azure Cosmos DB DocumentDB API gyűjtemény importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-240"><a id="DocumentDBSource"></a>To import from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="a569b-241">Az Azure Cosmos DB adatforrás-importáló beállítást adatokat importálhat egy vagy több Azure Cosmos DB gyűjteményt, és opcionálisan szűréséhez a dokumentumok lekérdezés segítségével teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="a569b-241">The Azure Cosmos DB source importer option allows you to import data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Képernyőfelvétel az Azure Cosmos DB adatforrás-beállítások](./media/import-data/documentdbsource.png)

<span data-ttu-id="a569b-243">Az Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-243">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="a569b-244">A Azure Cosmos DB fiók kapcsolati karakterláncot az Azure-portálon (kulcsok) panelén lekérhetők, a [Azure Cosmos DB fiók kezelése](manage-account.md), azonban annak az adatbázisnak a nevét meg kell hozzáfűzi a kapcsolati karakterlánc a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a569b-244">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="a569b-245">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott Azure Cosmos DB-példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-245">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-246">Egyetlen Azure Cosmos DB gyűjtemény importálása, adja meg, amelyből adatok Importálja a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="a569b-246">To import from a single Azure Cosmos DB collection, enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="a569b-247">Több Azure Cosmos DB gyűjtemények importálása, adja meg egy vagy több gyűjtemény neve kereséséhez reguláris kifejezés (pl. collection01 |} collection02 |} collection03).</span><span class="sxs-lookup"><span data-stu-id="a569b-247">To import from multiple Azure Cosmos DB collections, provide a regular expression to match one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="a569b-248">Meg lehet, hogy igény szerint adja meg, vagy adjon meg egy fájlt, szűrő és az adatokat, importálandók shape lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a569b-248">You may optionally specify, or provide a file for, a query to both filter and shape the data to be imported.</span></span>

> [!NOTE]
> <span data-ttu-id="a569b-249">Mivel a gyűjtemény mezőben reguláris kifejezések fogad el, ha importál egy egyetlen gyűjteményből, amelynek a neve reguláris kifejezés karaktereket tartalmaz, majd ezeket a karaktereket kell megjelölni ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a569b-249">Since the collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="a569b-250">Az Azure Cosmos DB adatforrás-importáló beállítást tartalmaz a következő speciális beállítások:</span><span class="sxs-lookup"><span data-stu-id="a569b-250">The Azure Cosmos DB source importer option has the following advanced options:</span></span>

1. <span data-ttu-id="a569b-251">Belső mezők szerepelhetnek: Megadja, hogy Azure Cosmos DB rendszer dokumentumtulajdonságok szerepeljenek az Exportálás (pl. _rid, _ts).</span><span class="sxs-lookup"><span data-stu-id="a569b-251">Include Internal Fields: Specifies whether or not to include Azure Cosmos DB document system properties in the export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="a569b-252">Hiba újbóli próbálkozások számát: a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén próbálkozások számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-252">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="a569b-253">Újrapróbálkozási időköz: Azt határozza meg mennyi ideig várjon, amíg a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén újrapróbálkozása között.</span><span class="sxs-lookup"><span data-stu-id="a569b-253">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="a569b-254">Csatlakozási mód: A csatlakozási mód használata Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-254">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="a569b-255">A lehetséges értékek DirectTcp, DirectHttps és az átjáró.</span><span class="sxs-lookup"><span data-stu-id="a569b-255">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="a569b-256">A közvetlen kapcsolat módok a következők: gyorsabb, amíg az átjáró mód rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="a569b-256">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB adatforrás speciális beállítások](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="a569b-258">Az import eszközt az alapértelmezett csatlakozási mód DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="a569b-258">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="a569b-259">Ha tűzfal problémák, kapcsolat módba vált-átjárón csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a569b-259">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="a569b-260">Az alábbiakban néhány parancssor minták Azure Cosmos DB importálása:</span><span class="sxs-lookup"><span data-stu-id="a569b-260">Here are some command line samples to import from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="a569b-261">Az Azure Cosmos DB importálása eszközzel is támogatja az adatok importálása a [Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-261">The Azure Cosmos DB Data Import Tool also supports import of data from the [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="a569b-262">Adatok importálása egy helyi emulátor, állítja a végpont `https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="a569b-262">When importing data from a local emulator, set the endpoint to `https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="a569b-263"><a id="HBaseSource"></a>A HBase importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-263"><a id="HBaseSource"></a>To import from HBase</span></span>
<span data-ttu-id="a569b-264">A HBase forrás importáló beállítás lehetővé teszi adatokat importálhat egy HBase tábla, és opcionálisan szűrje az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a569b-264">The HBase source importer option allows you to import data from an HBase table and optionally filter the data.</span></span> <span data-ttu-id="a569b-265">Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.</span><span class="sxs-lookup"><span data-stu-id="a569b-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource1.png)

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource2.png)

<span data-ttu-id="a569b-268">A HBase Stargate kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-268">The format of the HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="a569b-269">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott HBase-példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-269">Use the Verify command to ensure that the HBase instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-270">Íme egy parancssor minta HBase importálása:</span><span class="sxs-lookup"><span data-stu-id="a569b-270">Here is a command line sample to import from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="a569b-271"><a id="DocumentDBBulkTarget"></a>A DocumentDB-API (tömeges importálásához) importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-271"><a id="DocumentDBBulkTarget"></a>To import to the DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="a569b-272">Az Azure Cosmos DB tömeges importáló lehetővé teszi az elérhető forráskiszolgálók beállításokat, egy Azure Cosmos DB tárolt eljárás használatával a hatékonyság importálása.</span><span class="sxs-lookup"><span data-stu-id="a569b-272">The Azure Cosmos DB Bulk importer allows you to import from any of the available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="a569b-273">Az eszköz támogatja az importálás egy egyetlen particionált Azure Cosmos DB gyűjteménybe, valamint a szilánkos importálása, amelynek során az adatok több egy particionált Azure Cosmos DB gyűjtemények között particionált van.</span><span class="sxs-lookup"><span data-stu-id="a569b-273">The tool supports import to one single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="a569b-274">Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="a569b-275">Az eszköz létrehozása, hajtható végre, és törölje a cél a következő gyűjtemény(ek) készleteit szinkronizálja a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="a569b-275">The tool will create, execute, and then delete the stored procedure from the target collection(s).</span></span>  

![Képernyőfelvétel az Azure Cosmos DB tömeges beállítások](./media/import-data/documentdbbulk.png)

<span data-ttu-id="a569b-277">Az Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-277">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="a569b-278">A Azure Cosmos DB fiók kapcsolati karakterláncot az Azure-portálon (kulcsok) panelén lekérhetők, a [Azure Cosmos DB fiók kezelése](manage-account.md), azonban annak az adatbázisnak a nevét meg kell hozzáfűzi a kapcsolati karakterlánc a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a569b-278">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="a569b-279">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott Azure Cosmos DB-példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-279">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-280">Egyetlen gyűjtemény importálásához adja meg, amelyhez adatok Importálja, és kattintson a Hozzáadás gombra a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="a569b-280">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="a569b-281">Több gyűjteményre importálásához külön-külön adja meg az egyes kollekció nevét vagy az alábbi szintaxissal adhatja meg több gyűjteményt: *collection_prefix*[kezdő index - end index].</span><span class="sxs-lookup"><span data-stu-id="a569b-281">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="a569b-282">A fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-282">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="a569b-283">Csak az egész tartomány neve minták támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a569b-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="a569b-284">Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="a569b-284">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="a569b-285">Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.</span><span class="sxs-lookup"><span data-stu-id="a569b-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="a569b-286">Egynél több helyettesítési megadható.</span><span class="sxs-lookup"><span data-stu-id="a569b-286">More than one substitution can be provided.</span></span> <span data-ttu-id="a569b-287">Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).</span><span class="sxs-lookup"><span data-stu-id="a569b-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="a569b-288">A gyűjtemény neve megadása után válassza ki a kívánt átviteli sebességgel (10 000 RUs a 400 RUs) a következő gyűjtemény(ek) készleteit szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="a569b-288">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 10,000 RUs).</span></span> <span data-ttu-id="a569b-289">Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="a569b-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="a569b-290">Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a569b-291">A teljesítmény beállítását csak a webhelycsoport létrehozása vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a569b-291">The performance throughput setting only applies to collection creation.</span></span> <span data-ttu-id="a569b-292">A megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="a569b-292">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="a569b-293">Több gyűjteményre importálásakor a importálási eszköz által támogatott kivonatoló alapú horizontális.</span><span class="sxs-lookup"><span data-stu-id="a569b-293">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="a569b-294">Ebben az esetben adja meg a partíciókulcsnak használni kívánt tulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen a célként megadott gyűjtemények között).</span><span class="sxs-lookup"><span data-stu-id="a569b-294">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="a569b-295">Opcionálisan megadhat melyik mezőt a importálási forrásból (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd az importálási eszköz létrehoz egy GUID azonosító tulajdonság értékeként) importálásakor kell használható az Azure Cosmos adatbázis-azonosító tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="a569b-295">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="a569b-296">Nincsenek speciális beállítások számos elérhető importálása során.</span><span class="sxs-lookup"><span data-stu-id="a569b-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="a569b-297">Először amíg az eszköz tartalmaz alapértelmezett tömeges importálásához tárolt eljárás (BulkInsert.js), választhatja a saját importálási tárolt eljárás megadása:</span><span class="sxs-lookup"><span data-stu-id="a569b-297">First, while the tool includes a default bulk import stored procedure (BulkInsert.js), you may choose to specify your own import stored procedure:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB tömeges beszúrási sproc beállítást](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="a569b-299">Emellett dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a569b-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="a569b-301">Karakterlánc: Egy karakterláncértéket áll fenn</span><span class="sxs-lookup"><span data-stu-id="a569b-301">String: Persist as a string value</span></span>
* <span data-ttu-id="a569b-302">Epoch: Egy érték szám Epoch áll fenn</span><span class="sxs-lookup"><span data-stu-id="a569b-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="a569b-303">Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit.</span><span class="sxs-lookup"><span data-stu-id="a569b-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="a569b-304">Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="a569b-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="a569b-305">Az Azure Cosmos DB tömeges importáló a következő további speciális beállítások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a569b-305">The Azure Cosmos DB Bulk importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="a569b-306">Köteg mérete: Az eszköz az alapértelmezett érték a Köteg mérete 50.</span><span class="sxs-lookup"><span data-stu-id="a569b-306">Batch Size: The tool defaults to a batch size of 50.</span></span>  <span data-ttu-id="a569b-307">Ha a dokumentumokat, importálandók nagy, fontolja meg a köteg méretének csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="a569b-307">If the documents to be imported are large, consider lowering the batch size.</span></span> <span data-ttu-id="a569b-308">Ezzel szemben ha a dokumentumokat, importálandók kicsi, fontolja meg a köteg méretének emelés.</span><span class="sxs-lookup"><span data-stu-id="a569b-308">Conversely, if the documents to be imported are small, consider raising the batch size.</span></span>
2. <span data-ttu-id="a569b-309">Max. parancsfájl mérete (bájt): az eszköz az alapértelmezett 512KB parancsfájl maximális mérete</span><span class="sxs-lookup"><span data-stu-id="a569b-309">Max Script Size (bytes): The tool defaults to a max script size of 512KB</span></span>
3. <span data-ttu-id="a569b-310">Tiltsa le automatikus azonosító létrehozása: Ha minden dokumentumot, importálandók azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="a569b-310">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="a569b-311">Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="a569b-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="a569b-312">Frissítés meglévő dokumentumok: Az eszköz az alapértelmezett érték nem azonosító ütközik a meglévő dokumentumok cseréje.</span><span class="sxs-lookup"><span data-stu-id="a569b-312">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="a569b-313">Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="a569b-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="a569b-314">A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="a569b-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="a569b-315">Hiba újbóli próbálkozások számát: a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén próbálkozások számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-315">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="a569b-316">Újrapróbálkozási időköz: Azt határozza meg mennyi ideig várjon, amíg a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén újrapróbálkozása között.</span><span class="sxs-lookup"><span data-stu-id="a569b-316">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="a569b-317">Csatlakozási mód: A csatlakozási mód használata Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-317">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="a569b-318">A lehetséges értékek DirectTcp, DirectHttps és az átjáró.</span><span class="sxs-lookup"><span data-stu-id="a569b-318">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="a569b-319">A közvetlen kapcsolat módok a következők: gyorsabb, amíg az átjáró mód rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="a569b-319">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB tömeges importálással speciális beállítások](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="a569b-321">Az import eszközt az alapértelmezett csatlakozási mód DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="a569b-321">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="a569b-322">Ha tűzfal problémák, kapcsolat módba vált-átjárón csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a569b-322">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="a569b-323"><a id="DocumentDBSeqTarget"></a>A DocumentDB-API (egymást követő rekord importálása) importálása</span><span class="sxs-lookup"><span data-stu-id="a569b-323"><a id="DocumentDBSeqTarget"></a>To import to the DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="a569b-324">Az Azure Cosmos DB szekvenciális rekord importáló lehetővé teszi, hogy a rekord által rekord alapon az elérhető forráskiszolgálók beállításokat importálhat.</span><span class="sxs-lookup"><span data-stu-id="a569b-324">The Azure Cosmos DB sequential record importer allows you to import from any of the available source options on a record by record basis.</span></span> <span data-ttu-id="a569b-325">Akkor célszerű használni ezt a beállítást, ha importál egy meglevő gyűjteményhez éri el a tárolt eljárásokra vonatkozó kvótáját.</span><span class="sxs-lookup"><span data-stu-id="a569b-325">You might choose this option if you’re importing to an existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="a569b-326">Az eszköz támogatja az importálás (egypartíciós és több partíció) egyetlen Azure Cosmos DB gyűjteménybe, valamint a szilánkos importálása, amelynek során az adatok több egypartíciós és/vagy több partíció Azure Cosmos DB gyűjtemények között particionált van.</span><span class="sxs-lookup"><span data-stu-id="a569b-326">The tool supports import to a single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="a569b-327">Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálási beállítások](./media/import-data/documentdbsequential.png)

<span data-ttu-id="a569b-329">Az Azure Cosmos DB kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="a569b-329">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="a569b-330">A Azure Cosmos DB fiók kapcsolati karakterláncot az Azure-portálon (kulcsok) panelén lekérhetők, a [Azure Cosmos DB fiók kezelése](manage-account.md), azonban annak az adatbázisnak a nevét meg kell hozzáfűzi a kapcsolati karakterlánc a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a569b-330">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="a569b-331">A ellenőrizze parancs segítségével győződjön meg arról, hogy a kapcsolati karakterlánc mezőben megadott Azure Cosmos DB-példány is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="a569b-331">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="a569b-332">Egyetlen gyűjtemény importálásához adja meg, amelyhez adatok Importálja, és kattintson a Hozzáadás gombra a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="a569b-332">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="a569b-333">Több gyűjteményre importálásához külön-külön adja meg az egyes kollekció nevét vagy az alábbi szintaxissal adhatja meg több gyűjteményt: *collection_prefix*[kezdő index - end index].</span><span class="sxs-lookup"><span data-stu-id="a569b-333">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="a569b-334">A fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe a következő:</span><span class="sxs-lookup"><span data-stu-id="a569b-334">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="a569b-335">Csak az egész tartomány neve minták támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a569b-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="a569b-336">Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="a569b-336">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="a569b-337">Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.</span><span class="sxs-lookup"><span data-stu-id="a569b-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="a569b-338">Egynél több helyettesítési megadható.</span><span class="sxs-lookup"><span data-stu-id="a569b-338">More than one substitution can be provided.</span></span> <span data-ttu-id="a569b-339">Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).</span><span class="sxs-lookup"><span data-stu-id="a569b-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="a569b-340">A gyűjtemény neve megadása után válassza ki a kívánt átviteli sebességgel (a 250 000 RUs 400 RUs) a következő gyűjtemény(ek) készleteit szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="a569b-340">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 250,000 RUs).</span></span> <span data-ttu-id="a569b-341">Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="a569b-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="a569b-342">Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="a569b-343">Bármely importálás átviteli gyűjtemények > 10 000 RUs partíciós kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="a569b-343">Any import to collections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="a569b-344">Ha több mint 250 000 RUs választ, akkor küldje el a kérést a portálon növelni a fiókját.</span><span class="sxs-lookup"><span data-stu-id="a569b-344">If you choose to have more than 250,000 RUs, you will need to file a request in the portal to have your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="a569b-345">Az átviteli sebesség beállítás csak a webhelycsoport létrehozása vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a569b-345">The throughput setting only applies to collection creation.</span></span> <span data-ttu-id="a569b-346">A megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="a569b-346">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="a569b-347">Több gyűjteményre importálásakor a importálási eszköz által támogatott kivonatoló alapú horizontális.</span><span class="sxs-lookup"><span data-stu-id="a569b-347">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="a569b-348">Ebben az esetben adja meg a partíciókulcsnak használni kívánt tulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen a célként megadott gyűjtemények között).</span><span class="sxs-lookup"><span data-stu-id="a569b-348">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="a569b-349">Opcionálisan megadhat melyik mezőt a importálási forrásból (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd az importálási eszköz létrehoz egy GUID azonosító tulajdonság értékeként) importálásakor kell használható az Azure Cosmos adatbázis-azonosító tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="a569b-349">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="a569b-350">Nincsenek speciális beállítások számos elérhető importálása során.</span><span class="sxs-lookup"><span data-stu-id="a569b-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="a569b-351">Első lépésként dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a569b-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="a569b-353">Karakterlánc: Egy karakterláncértéket áll fenn</span><span class="sxs-lookup"><span data-stu-id="a569b-353">String: Persist as a string value</span></span>
* <span data-ttu-id="a569b-354">Epoch: Egy érték szám Epoch áll fenn</span><span class="sxs-lookup"><span data-stu-id="a569b-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="a569b-355">Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit.</span><span class="sxs-lookup"><span data-stu-id="a569b-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="a569b-356">Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="a569b-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="a569b-357">Az Azure Cosmos DB - szekvenciális rekord importáló a következő további speciális beállításokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="a569b-357">The Azure Cosmos DB - Sequential record importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="a569b-358">Párhuzamos kérelem: az eszköz az alapértelmezett 2 párhuzamos kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="a569b-358">Number of Parallel Requests: The tool defaults to 2 parallel requests.</span></span> <span data-ttu-id="a569b-359">Ha a dokumentumokat, importálandók kicsi, fontolja meg előléptetése párhuzamos kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="a569b-359">If the documents to be imported are small, consider raising the number of parallel requests.</span></span> <span data-ttu-id="a569b-360">Vegye figyelembe, hogy ez a szám túl sok következik be, ha az importálás problémákat tapasztalhat a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="a569b-360">Note that if this number is raised too much, the import may experience throttling.</span></span>
2. <span data-ttu-id="a569b-361">Tiltsa le automatikus azonosító létrehozása: Ha minden dokumentumot, importálandók azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="a569b-361">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="a569b-362">Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="a569b-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="a569b-363">Frissítés meglévő dokumentumok: Az eszköz az alapértelmezett érték nem azonosító ütközik a meglévő dokumentumok cseréje.</span><span class="sxs-lookup"><span data-stu-id="a569b-363">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="a569b-364">Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="a569b-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="a569b-365">A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="a569b-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="a569b-366">Hiba újbóli próbálkozások számát: a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén próbálkozások számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-366">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="a569b-367">Újrapróbálkozási időköz: Azt határozza meg mennyi ideig várjon, amíg a kapcsolat az Azure Cosmos Adatbázishoz (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén újrapróbálkozása között.</span><span class="sxs-lookup"><span data-stu-id="a569b-367">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="a569b-368">Csatlakozási mód: A csatlakozási mód használata Azure Cosmos DB határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a569b-368">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="a569b-369">A lehetséges értékek DirectTcp, DirectHttps és az átjáró.</span><span class="sxs-lookup"><span data-stu-id="a569b-369">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="a569b-370">A közvetlen kapcsolat módok a következők: gyorsabb, amíg az átjáró mód rövid több tűzfal, mert csak 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="a569b-370">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálása speciális beállítások](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="a569b-372">Az import eszközt az alapértelmezett csatlakozási mód DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="a569b-372">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="a569b-373">Ha tűzfal problémák, kapcsolat módba vált-átjárón csak 443-as portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a569b-373">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="a569b-374"><a id="IndexingPolicy"></a>Adjon meg egy indexelési házirendet, amikor Azure Cosmos DB gyűjtemények létrehozása</span><span class="sxs-lookup"><span data-stu-id="a569b-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="a569b-375">Ha engedélyezi az áttelepítési eszköz importálása során gyűjtemények létrehozásához, a gyűjtemények az indexelési házirendet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="a569b-375">When you allow the migration tool to create collections during import, you can specify the indexing policy of the collections.</span></span> <span data-ttu-id="a569b-376">A Speciális beállítások szakaszban Azure Cosmos DB tömeges importálással és Azure Cosmos DB szekvenciális rekord beállítások keresse meg az indexelési házirendet szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a569b-376">In the advanced options section of the Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate to the Indexing Policy section.</span></span>

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="a569b-378">A speciális beállítás indexelési házirendet használja, akkor is indexelési házirend fájlt válasszon ki, manuálisan adja meg az indexelési házirendet vagy kattintva válassza ki az alapértelmezett sablonok közül (jobb gombbal az indexelési házirendet szövegmezőjének).</span><span class="sxs-lookup"><span data-stu-id="a569b-378">Using the Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in the indexing policy textbox).</span></span>

<span data-ttu-id="a569b-379">A sablonok az eszközt biztosít a következők:</span><span class="sxs-lookup"><span data-stu-id="a569b-379">The policy templates the tool provides are:</span></span>

* <span data-ttu-id="a569b-380">Alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a569b-380">Default.</span></span> <span data-ttu-id="a569b-381">Ez a házirend akkor ajánlott, ha Ön karakterláncok egyenlőséglekérdezéséhez végrehajtása és a számok ORDER BY, tartomány és egyenlőség lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="a569b-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="a569b-382">Ez a házirend rendelkezik egy alacsonyabb indextárolási terheléssel jár mint tartomány.</span><span class="sxs-lookup"><span data-stu-id="a569b-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="a569b-383">Tartomány.</span><span class="sxs-lookup"><span data-stu-id="a569b-383">Range.</span></span> <span data-ttu-id="a569b-384">Ezzel a házirend-érdemes a számok és karakterláncok ORDER BY, tartomány-és egyenlőséglekérdezéséhez használ.</span><span class="sxs-lookup"><span data-stu-id="a569b-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="a569b-385">Ez a házirend egy magasabb indextárolási terheléssel jár, mint az alapértelmezett vagy az ujjlenyomat-rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a569b-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="a569b-387">Ha nem adja meg az indexelési házirendet, majd az alapértelmezett házirend lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="a569b-387">If you do not specify an indexing policy, then the default policy will be applied.</span></span> <span data-ttu-id="a569b-388">Az indexelő házirendekkel kapcsolatos további információkért lásd: [Azure Cosmos DB házirendek indexelő](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="a569b-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-to-json-file"></a><span data-ttu-id="a569b-389">JSON-fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="a569b-389">Export to JSON file</span></span>
<span data-ttu-id="a569b-390">Az Azure Cosmos DB JSON-exportáló lehetővé teszi a JSON-dokumentumok bájttömb tartalmazó JSON-fájlba exportálja az elérhető forráskiszolgálók beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a569b-390">The Azure Cosmos DB JSON exporter allows you to export any of the available source options to a JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="a569b-391">Az eszköz az Exportálás meg fogja kezelni, vagy ha szeretné megtekinteni az eredményül kapott áttelepítési parancsot, és futtassa a parancsot.</span><span class="sxs-lookup"><span data-stu-id="a569b-391">The tool will handle the export for you, or you can choose to view the resulting migration command and run the command yourself.</span></span> <span data-ttu-id="a569b-392">Az eredményül kapott JSON-fájl helyben vagy az Azure Blob storage tárolhatók.</span><span class="sxs-lookup"><span data-stu-id="a569b-392">The resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Az Azure Cosmos DB JSON képernyőkép helyi fájl exportálási beállítás](./media/import-data/jsontarget.png)

![Képernyőkép Azure Cosmos DB JSON Azure Blob storage exportálási beállítás](./media/import-data/jsontarget2.png)

<span data-ttu-id="a569b-395">Opcionálisan választhatja az eredményül kapott JSON, ami növeli az eredményül kapott dokumentum méretét a tartalom több tétele közben prettify emberi olvasható.</span><span class="sxs-lookup"><span data-stu-id="a569b-395">You may optionally choose to prettify the resulting JSON, which will increase the size of the resulting document while making the contents more human readable.</span></span>

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

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
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a><span data-ttu-id="a569b-396">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="a569b-396">Advanced configuration</span></span>
<span data-ttu-id="a569b-397">A speciális konfigurálására szolgáló képernyőn adja meg a naplófájlt, amelyet szeretne írt hibák helyét.</span><span class="sxs-lookup"><span data-stu-id="a569b-397">In the Advanced configuration screen, specify the location of the log file to which you would like any errors written.</span></span> <span data-ttu-id="a569b-398">Ezen a lapon az alábbi szabályok vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="a569b-398">The following rules apply to this page:</span></span>

1. <span data-ttu-id="a569b-399">Ha egy fájl neve nem áll rendelkezésre, majd a program az összes hiba visszatér az eredmények lapon.</span><span class="sxs-lookup"><span data-stu-id="a569b-399">If a file name is not provided, then all errors will be returned on the Results page.</span></span>
2. <span data-ttu-id="a569b-400">Ha a fájl nevét a címtár nélkül áll rendelkezésre, majd a fájl létrejön (vagy felül) az aktuális környezet könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="a569b-400">If a file name is provided without a directory, then the file will be created (or overwritten) in the current environment directory.</span></span>
3. <span data-ttu-id="a569b-401">Ha egy meglévő fájlt, majd a fájl felül lesznek írva, nincs append beállítás.</span><span class="sxs-lookup"><span data-stu-id="a569b-401">If you select an existing file, then the file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="a569b-402">Ezután válassza naplózását, kritikus, vagy a hibaüzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a569b-402">Then, choose whether to log all, critical, or no error messages.</span></span> <span data-ttu-id="a569b-403">Végül döntse el, hogy milyen gyakran az a képernyő átviteli üzenet frissíti a rendszer a telepítés előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="a569b-403">Finally, decide how frequently the on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="a569b-404">Erősítse meg a beállításokat importálhat és parancs sor megtekintése</span><span class="sxs-lookup"><span data-stu-id="a569b-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="a569b-405">Adja meg az adatforrásra vonatkozó információ, a cél adatokat és a speciális konfigurációs, áttekintheti összefoglaló az áttelepítés, és szükség esetén megtekintése és másolása az eredményül kapott áttelepítési parancsot (a parancs másolás akkor hasznos, importálási műveletek automatizálásához):</span><span class="sxs-lookup"><span data-stu-id="a569b-405">After specifying source information, target information, and advanced configuration, review the migration summary and, optionally, view/copy the resulting migration command (copying the command is useful to automate import operations):</span></span>
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summary.png)
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summarycommand.png)
2. <span data-ttu-id="a569b-408">Ha elégedett a forrás- és beállításokat, kattintson **importálási**.</span><span class="sxs-lookup"><span data-stu-id="a569b-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="a569b-409">Az eltelt idő, átvitt száma és hibaadatokat (Ha nem adott meg egy fájlnevet a Speciális konfiguráció) frissíti az importálás folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a569b-409">The elapsed time, transferred count, and failure information (if you didn't provide a file name in the Advanced configuration) will update as the import is in process.</span></span> <span data-ttu-id="a569b-410">Művelet befejeződése után exportálhatja az eredményeket (pl. importálási hibák kezelése érdekében).</span><span class="sxs-lookup"><span data-stu-id="a569b-410">Once complete, you can export the results (e.g. to deal with any import failures).</span></span>
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/viewresults.png)
3. <span data-ttu-id="a569b-412">Egy új importálhat indulhat tartani a meglévő beállítások (például kapcsolati karakterlánc adatokat, a forrás és cél kiválasztása, stb.), vagy alaphelyzetbe állítása az összes értéket.</span><span class="sxs-lookup"><span data-stu-id="a569b-412">You may also start a new import, either keeping the existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="a569b-414">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a569b-414">Next steps</span></span>

<span data-ttu-id="a569b-415">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="a569b-415">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a569b-416">Az adatáttelepítési eszköz telepítése</span><span class="sxs-lookup"><span data-stu-id="a569b-416">Installed the Data Migration tool</span></span>
> * <span data-ttu-id="a569b-417">Különböző adatforrásokból importált adatok</span><span class="sxs-lookup"><span data-stu-id="a569b-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="a569b-418">Exportált Azure Cosmos DB JSON</span><span class="sxs-lookup"><span data-stu-id="a569b-418">Exported from Azure Cosmos DB to JSON</span></span>

<span data-ttu-id="a569b-419">Most már továbbléphet a következő oktatóanyagot, és megtudhatja, hogyan kérdezhet le adatokat Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="a569b-419">You can now proceed to the next tutorial and learn how to query data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="a569b-420">Hogyan kérdezhet le adatokat?</span><span class="sxs-lookup"><span data-stu-id="a569b-420">How to query data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
