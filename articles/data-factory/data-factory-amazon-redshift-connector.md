---
title: "Adatok áthelyezése az Amazon Redshift Data Factory használatával |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával Amazon Redshift áthelyezni az adatokat."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: bccb941363952bb2251629240a88148a6527d62e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="21ed3-103">Helyezze át az adatokat az Amazon Redshift Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="21ed3-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="21ed3-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="21ed3-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from Amazon Redshift.</span></span> <span data-ttu-id="21ed3-105">A cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="21ed3-105">The article builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="21ed3-106">Amazon Redshift adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="21ed3-106">You can copy data from Amazon Redshift to any supported sink data store.</span></span> <span data-ttu-id="21ed3-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="21ed3-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="21ed3-108">Adat-előállító jelenleg a mozgóátlag adatait Amazon Redshift egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz Amazon Redshift támogatja.</span><span class="sxs-lookup"><span data-stu-id="21ed3-108">Data factory currently supports moving data from Amazon Redshift to other data stores, but not for moving data from other data stores to Amazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21ed3-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21ed3-109">Prerequisites</span></span>
* <span data-ttu-id="21ed3-110">Ha a helyszíni adattárolóihoz adatokat helyez át, telepítse [az adatkezelési átjáró](data-factory-data-management-gateway.md) a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="21ed3-110">If you are moving data to an on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="21ed3-111">Ezt követően hozzáférést adatkezelési átjáró (használata IP-cím a gép) az Amazon Redshift fürt.</span><span class="sxs-lookup"><span data-stu-id="21ed3-111">Then, Grant Data Management Gateway (use IP address of the machine) the access to Amazon Redshift cluster.</span></span> <span data-ttu-id="21ed3-112">Lásd: [engedélyezi a hozzáférést a fürthöz](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="21ed3-112">See [Authorize access to the cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="21ed3-113">Ha egy Azure data Store helyez át adatokat, lásd: [Azure Data Center IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) számítási IP-cím és az Azure-adatok által használt SQL-címtartományok szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="21ed3-113">If you are moving data to an Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for the Compute IP address and SQL ranges used by the Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="21ed3-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="21ed3-114">Getting started</span></span>
<span data-ttu-id="21ed3-115">A másolási tevékenység, amely helyezi át az adatokat Amazon Redshift forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="21ed3-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="21ed3-116">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="21ed3-116">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="21ed3-117">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="21ed3-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="21ed3-118">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="21ed3-118">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="21ed3-119">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="21ed3-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="21ed3-120">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="21ed3-120">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="21ed3-121">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="21ed3-121">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="21ed3-122">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="21ed3-122">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="21ed3-123">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="21ed3-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="21ed3-124">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="21ed3-124">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="21ed3-125">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="21ed3-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="21ed3-126">Adatok másolása az Amazon Redshift adattár használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Amazon Redshift az Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="21ed3-126">For a sample with JSON definitions for Data Factory entities that are used to copy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift to Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="21ed3-127">A következő szakaszok részletesen bemutatják, amely segítségével az Amazon Redshift megadása a Data Factory tartozó entitások JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="21ed3-127">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Amazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="21ed3-128">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="21ed3-128">Linked service properties</span></span>
<span data-ttu-id="21ed3-129">A következő táblázat a JSON-elemek szerepelnek Amazon Redshift kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="21ed3-129">The following table provides description for JSON elements specific to Amazon Redshift linked service.</span></span>

| <span data-ttu-id="21ed3-130">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="21ed3-130">Property</span></span> | <span data-ttu-id="21ed3-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ed3-131">Description</span></span> | <span data-ttu-id="21ed3-132">Szükséges</span><span class="sxs-lookup"><span data-stu-id="21ed3-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="21ed3-133">type</span><span class="sxs-lookup"><span data-stu-id="21ed3-133">type</span></span> |<span data-ttu-id="21ed3-134">A type tulajdonságot kell beállítani: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="21ed3-134">The type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="21ed3-135">Igen</span><span class="sxs-lookup"><span data-stu-id="21ed3-135">Yes</span></span> |
| <span data-ttu-id="21ed3-136">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="21ed3-136">server</span></span> |<span data-ttu-id="21ed3-137">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét az Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="21ed3-137">IP address or host name of the Amazon Redshift server.</span></span> |<span data-ttu-id="21ed3-138">Igen</span><span class="sxs-lookup"><span data-stu-id="21ed3-138">Yes</span></span> |
| <span data-ttu-id="21ed3-139">port</span><span class="sxs-lookup"><span data-stu-id="21ed3-139">port</span></span> |<span data-ttu-id="21ed3-140">A TCP-portot, amelyen az Amazon Redshift kiszolgáló ügyfélkapcsolatokat száma.</span><span class="sxs-lookup"><span data-stu-id="21ed3-140">The number of the TCP port that the Amazon Redshift server uses to listen for client connections.</span></span> |<span data-ttu-id="21ed3-141">Nem, alapértelmezett érték: 5439</span><span class="sxs-lookup"><span data-stu-id="21ed3-141">No, default value: 5439</span></span> |
| <span data-ttu-id="21ed3-142">adatbázis</span><span class="sxs-lookup"><span data-stu-id="21ed3-142">database</span></span> |<span data-ttu-id="21ed3-143">Az Amazon Redshift adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="21ed3-143">Name of the Amazon Redshift database.</span></span> |<span data-ttu-id="21ed3-144">Igen</span><span class="sxs-lookup"><span data-stu-id="21ed3-144">Yes</span></span> |
| <span data-ttu-id="21ed3-145">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="21ed3-145">username</span></span> |<span data-ttu-id="21ed3-146">Felhasználó, aki hozzáfér az adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="21ed3-146">Name of user who has access to the database.</span></span> |<span data-ttu-id="21ed3-147">Igen</span><span class="sxs-lookup"><span data-stu-id="21ed3-147">Yes</span></span> |
| <span data-ttu-id="21ed3-148">jelszó</span><span class="sxs-lookup"><span data-stu-id="21ed3-148">password</span></span> |<span data-ttu-id="21ed3-149">A felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="21ed3-149">Password for the user account.</span></span> |<span data-ttu-id="21ed3-150">Igen</span><span class="sxs-lookup"><span data-stu-id="21ed3-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="21ed3-151">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="21ed3-151">Dataset properties</span></span>
<span data-ttu-id="21ed3-152">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="21ed3-152">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="21ed3-153">Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="21ed3-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="21ed3-154">A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="21ed3-154">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="21ed3-155">Tartalmazza az adatokat az adattár a helyére vonatkozó adatokat.</span><span class="sxs-lookup"><span data-stu-id="21ed3-155">It provides information about the location of the data in the data store.</span></span> <span data-ttu-id="21ed3-156">A typeProperties szakasz típusú adatkészlet **RelationalTable** (amely tartalmazza az Amazon Redshift dataset) a következő tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="21ed3-156">The typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has the following properties</span></span>

| <span data-ttu-id="21ed3-157">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="21ed3-157">Property</span></span> | <span data-ttu-id="21ed3-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ed3-158">Description</span></span> | <span data-ttu-id="21ed3-159">Szükséges</span><span class="sxs-lookup"><span data-stu-id="21ed3-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="21ed3-160">tableName</span><span class="sxs-lookup"><span data-stu-id="21ed3-160">tableName</span></span> |<span data-ttu-id="21ed3-161">A tábla az Amazon Redshift adatbázisban, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="21ed3-161">Name of the table in the Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="21ed3-162">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="21ed3-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="21ed3-163">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="21ed3-163">Copy activity properties</span></span>
<span data-ttu-id="21ed3-164">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="21ed3-164">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="21ed3-165">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="21ed3-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="21ed3-166">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="21ed3-166">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="21ed3-167">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="21ed3-167">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="21ed3-168">Ha másolási tevékenység forrása típusú **RelationalSource** (amely tartalmazza az Amazon Redshift), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="21ed3-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="21ed3-169">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="21ed3-169">Property</span></span> | <span data-ttu-id="21ed3-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ed3-170">Description</span></span> | <span data-ttu-id="21ed3-171">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="21ed3-171">Allowed values</span></span> | <span data-ttu-id="21ed3-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="21ed3-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="21ed3-173">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="21ed3-173">query</span></span> |<span data-ttu-id="21ed3-174">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="21ed3-174">Use the custom query to read data.</span></span> |<span data-ttu-id="21ed3-175">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="21ed3-175">SQL query string.</span></span> <span data-ttu-id="21ed3-176">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="21ed3-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="21ed3-177">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="21ed3-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob"></a><span data-ttu-id="21ed3-178">JSON-példa: adatok másolása az Amazon Redshift az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="21ed3-178">JSON example: Copy data from Amazon Redshift to Azure Blob</span></span>
<span data-ttu-id="21ed3-179">Ez a példa bemutatja, hogyan Amazon Redshift adatbázisból származó adatok másolása az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="21ed3-179">This sample shows how to copy data from an Amazon Redshift database to an Azure Blob Storage.</span></span> <span data-ttu-id="21ed3-180">Azonban az adatok átmásolhatók **közvetlenül** bármely, a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="21ed3-180">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="21ed3-181">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="21ed3-181">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="21ed3-182">A társított szolgáltatás típusa [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="21ed3-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="21ed3-183">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="21ed3-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="21ed3-184">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="21ed3-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="21ed3-185">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="21ed3-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="21ed3-186">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="21ed3-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="21ed3-187">A minta másol adatokat az Amazon Redshift egy lekérdezés eredményét blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="21ed3-187">The sample copies data from a query result in Amazon Redshift to a blob every hour.</span></span> <span data-ttu-id="21ed3-188">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="21ed3-188">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="21ed3-189">**Amazon Redshift társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="21ed3-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="21ed3-190">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="21ed3-190">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="21ed3-191">**Amazon Redshift bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="21ed3-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="21ed3-192">Beállítás `"external": true` tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="21ed3-192">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="21ed3-193">Ez a tulajdonság igaz értékre be egy bemeneti adatkészlet nem a feldolgozási tevékenység által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="21ed3-193">Set this property to true on an input dataset that is not produced by an activity in the pipeline.</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="21ed3-194">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="21ed3-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="21ed3-195">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="21ed3-195">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="21ed3-196">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="21ed3-196">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="21ed3-197">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="21ed3-197">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="21ed3-198">**A folyamat Azure Redshift (RelationalSource) és a fogadó Blob másolási tevékenység:**</span><span class="sxs-lookup"><span data-stu-id="21ed3-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="21ed3-199">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="21ed3-199">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="21ed3-200">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="21ed3-200">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="21ed3-201">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="21ed3-201">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="21ed3-202">Az Amazon Redshift leképezésének</span><span class="sxs-lookup"><span data-stu-id="21ed3-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="21ed3-203">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="21ed3-203">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="21ed3-204">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="21ed3-204">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="21ed3-205">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="21ed3-205">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="21ed3-206">Ha adatok áthelyezése Amazon Redshift, a következő megfeleltetéseket használ Amazon Redshift való .NET típusú.</span><span class="sxs-lookup"><span data-stu-id="21ed3-206">When moving data to Amazon Redshift, the following mappings are used from Amazon Redshift types to .NET types.</span></span>

| <span data-ttu-id="21ed3-207">Amazon Redshift típusa</span><span class="sxs-lookup"><span data-stu-id="21ed3-207">Amazon Redshift Type</span></span> | <span data-ttu-id="21ed3-208">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="21ed3-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="21ed3-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="21ed3-209">SMALLINT</span></span> |<span data-ttu-id="21ed3-210">Int16</span><span class="sxs-lookup"><span data-stu-id="21ed3-210">Int16</span></span> |
| <span data-ttu-id="21ed3-211">EGÉSZ SZÁM</span><span class="sxs-lookup"><span data-stu-id="21ed3-211">INTEGER</span></span> |<span data-ttu-id="21ed3-212">Int32</span><span class="sxs-lookup"><span data-stu-id="21ed3-212">Int32</span></span> |
| <span data-ttu-id="21ed3-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="21ed3-213">BIGINT</span></span> |<span data-ttu-id="21ed3-214">Int64</span><span class="sxs-lookup"><span data-stu-id="21ed3-214">Int64</span></span> |
| <span data-ttu-id="21ed3-215">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="21ed3-215">DECIMAL</span></span> |<span data-ttu-id="21ed3-216">Decimális</span><span class="sxs-lookup"><span data-stu-id="21ed3-216">Decimal</span></span> |
| <span data-ttu-id="21ed3-217">VALÓS</span><span class="sxs-lookup"><span data-stu-id="21ed3-217">REAL</span></span> |<span data-ttu-id="21ed3-218">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="21ed3-218">Single</span></span> |
| <span data-ttu-id="21ed3-219">A KÉTSZERES PONTOSSÁG</span><span class="sxs-lookup"><span data-stu-id="21ed3-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="21ed3-220">Dupla</span><span class="sxs-lookup"><span data-stu-id="21ed3-220">Double</span></span> |
| <span data-ttu-id="21ed3-221">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="21ed3-221">BOOLEAN</span></span> |<span data-ttu-id="21ed3-222">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ed3-222">String</span></span> |
| <span data-ttu-id="21ed3-223">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="21ed3-223">CHAR</span></span> |<span data-ttu-id="21ed3-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ed3-224">String</span></span> |
| <span data-ttu-id="21ed3-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="21ed3-225">VARCHAR</span></span> |<span data-ttu-id="21ed3-226">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ed3-226">String</span></span> |
| <span data-ttu-id="21ed3-227">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="21ed3-227">DATE</span></span> |<span data-ttu-id="21ed3-228">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="21ed3-228">DateTime</span></span> |
| <span data-ttu-id="21ed3-229">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="21ed3-229">TIMESTAMP</span></span> |<span data-ttu-id="21ed3-230">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="21ed3-230">DateTime</span></span> |
| <span data-ttu-id="21ed3-231">SZÖVEG</span><span class="sxs-lookup"><span data-stu-id="21ed3-231">TEXT</span></span> |<span data-ttu-id="21ed3-232">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ed3-232">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="21ed3-233">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="21ed3-233">Map source to sink columns</span></span>
<span data-ttu-id="21ed3-234">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="21ed3-234">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="21ed3-235">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="21ed3-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="21ed3-236">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="21ed3-236">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="21ed3-237">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="21ed3-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="21ed3-238">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="21ed3-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="21ed3-239">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="21ed3-239">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="21ed3-240">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="21ed3-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="21ed3-241">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="21ed3-241">Performance and Tuning</span></span>
<span data-ttu-id="21ed3-242">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="21ed3-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21ed3-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21ed3-243">Next Steps</span></span>
<span data-ttu-id="21ed3-244">Lásd az alábbi cikkeket:</span><span class="sxs-lookup"><span data-stu-id="21ed3-244">See the following articles:</span></span>

* <span data-ttu-id="21ed3-245">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="21ed3-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
