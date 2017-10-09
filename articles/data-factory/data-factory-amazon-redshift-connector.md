---
title: "használatával a Data Factory Amazon Redshift aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával Amazon Redshift toomove adatok."
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
ms.openlocfilehash: 2a097320734ebdd57282d250f7fdba35741777f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="f7c5d-103">Helyezze át az adatokat az Amazon Redshift Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f7c5d-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="f7c5d-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait Amazon Redshift a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from Amazon Redshift.</span></span> <span data-ttu-id="f7c5d-105">hello cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-105">hello article builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="f7c5d-106">Amazon Redshift támogatott tooany fogadó adattár adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-106">You can copy data from Amazon Redshift tooany supported sink data store.</span></span> <span data-ttu-id="f7c5d-107">Adattároló mosdók hello másolási tevékenység által támogatott listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="f7c5d-108">Adat-előállító jelenleg Amazon Redshift tooother adattárolókhoz, de nem az adatok áthelyezését más adatokat tároló tooAmazon Redshift áthelyezése adatokat.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-108">Data factory currently supports moving data from Amazon Redshift tooother data stores, but not for moving data from other data stores tooAmazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7c5d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f7c5d-109">Prerequisites</span></span>
* <span data-ttu-id="f7c5d-110">Ha adatok tooan helyszíni adattár helyez át, telepítse [az adatkezelési átjáró](data-factory-data-management-gateway.md) a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-110">If you are moving data tooan on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="f7c5d-111">Ezt követően Grant adatkezelési átjáró (IP-cím használata hello gép) hello hozzáférés tooAmazon Redshift fürt.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-111">Then, Grant Data Management Gateway (use IP address of hello machine) hello access tooAmazon Redshift cluster.</span></span> <span data-ttu-id="f7c5d-112">Lásd: [engedélyezés hozzáférés toohello fürt](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-112">See [Authorize access toohello cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="f7c5d-113">Ha tooan az Azure data adattár, lásd: [Azure Data Center IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) hello számítási IP-cím és hello Azure-adatközpont által használt SQL-tartományok.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-113">If you are moving data tooan Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for hello Compute IP address and SQL ranges used by hello Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f7c5d-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f7c5d-114">Getting started</span></span>
<span data-ttu-id="f7c5d-115">A másolási tevékenység, amely helyezi át az adatokat Amazon Redshift forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="f7c5d-116">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-116">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="f7c5d-117">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="f7c5d-118">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-118">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f7c5d-119">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f7c5d-120">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-120">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="f7c5d-121">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-121">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="f7c5d-122">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-122">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="f7c5d-123">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f7c5d-124">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-124">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="f7c5d-125">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="f7c5d-126">Az adat-előállító entitások, amelyek az Amazon Redshift adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-126">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="f7c5d-127">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAmazon Redshift részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-127">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="f7c5d-128">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="f7c5d-128">Linked service properties</span></span>
<span data-ttu-id="f7c5d-129">a következő táblázat hello biztosít JSON-elemek adott tooAmazon Redshift kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-129">hello following table provides description for JSON elements specific tooAmazon Redshift linked service.</span></span>

| <span data-ttu-id="f7c5d-130">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7c5d-130">Property</span></span> | <span data-ttu-id="f7c5d-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7c5d-131">Description</span></span> | <span data-ttu-id="f7c5d-132">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7c5d-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f7c5d-133">type</span><span class="sxs-lookup"><span data-stu-id="f7c5d-133">type</span></span> |<span data-ttu-id="f7c5d-134">hello type tulajdonságot kell beállítani: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-134">hello type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="f7c5d-135">Igen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-135">Yes</span></span> |
| <span data-ttu-id="f7c5d-136">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f7c5d-136">server</span></span> |<span data-ttu-id="f7c5d-137">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-137">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="f7c5d-138">Igen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-138">Yes</span></span> |
| <span data-ttu-id="f7c5d-139">port</span><span class="sxs-lookup"><span data-stu-id="f7c5d-139">port</span></span> |<span data-ttu-id="f7c5d-140">Amazon Redshift server hello hello TCP port száma hello toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-140">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="f7c5d-141">Nem, alapértelmezett érték: 5439</span><span class="sxs-lookup"><span data-stu-id="f7c5d-141">No, default value: 5439</span></span> |
| <span data-ttu-id="f7c5d-142">adatbázis</span><span class="sxs-lookup"><span data-stu-id="f7c5d-142">database</span></span> |<span data-ttu-id="f7c5d-143">Hello Amazon Redshift adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-143">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="f7c5d-144">Igen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-144">Yes</span></span> |
| <span data-ttu-id="f7c5d-145">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="f7c5d-145">username</span></span> |<span data-ttu-id="f7c5d-146">Hozzáférés toohello adatbázis-felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-146">Name of user who has access toohello database.</span></span> |<span data-ttu-id="f7c5d-147">Igen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-147">Yes</span></span> |
| <span data-ttu-id="f7c5d-148">jelszó</span><span class="sxs-lookup"><span data-stu-id="f7c5d-148">password</span></span> |<span data-ttu-id="f7c5d-149">Hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-149">Password for hello user account.</span></span> |<span data-ttu-id="f7c5d-150">Igen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="f7c5d-151">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f7c5d-151">Dataset properties</span></span>
<span data-ttu-id="f7c5d-152">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-152">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f7c5d-153">Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="f7c5d-154">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-154">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="f7c5d-155">Az adattár hello hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-155">It provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="f7c5d-156">hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza az Amazon Redshift dataset) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-156">hello typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has hello following properties</span></span>

| <span data-ttu-id="f7c5d-157">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7c5d-157">Property</span></span> | <span data-ttu-id="f7c5d-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7c5d-158">Description</span></span> | <span data-ttu-id="f7c5d-159">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7c5d-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f7c5d-160">tableName</span><span class="sxs-lookup"><span data-stu-id="f7c5d-160">tableName</span></span> |<span data-ttu-id="f7c5d-161">Hello tábla hello Amazon Redshift adatbázis, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-161">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="f7c5d-162">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f7c5d-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="f7c5d-163">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f7c5d-163">Copy activity properties</span></span>
<span data-ttu-id="f7c5d-164">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-164">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f7c5d-165">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="f7c5d-166">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-166">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="f7c5d-167">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-167">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="f7c5d-168">Ha másolási tevékenység forrása típusú **RelationalSource** (amely tartalmazza az Amazon Redshift), typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="f7c5d-169">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7c5d-169">Property</span></span> | <span data-ttu-id="f7c5d-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7c5d-170">Description</span></span> | <span data-ttu-id="f7c5d-171">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="f7c5d-171">Allowed values</span></span> | <span data-ttu-id="f7c5d-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7c5d-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f7c5d-173">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="f7c5d-173">query</span></span> |<span data-ttu-id="f7c5d-174">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-174">Use hello custom query tooread data.</span></span> |<span data-ttu-id="f7c5d-175">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-175">SQL query string.</span></span> <span data-ttu-id="f7c5d-176">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="f7c5d-177">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f7c5d-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-tooazure-blob"></a><span data-ttu-id="f7c5d-178">JSON-példa: adatok másolása az Amazon Redshift tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="f7c5d-178">JSON example: Copy data from Amazon Redshift tooAzure Blob</span></span>
<span data-ttu-id="f7c5d-179">Ez a példa bemutatja, hogyan toocopy adatait az Amazon Redshift adatbázis tooan Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-179">This sample shows how toocopy data from an Amazon Redshift database tooan Azure Blob Storage.</span></span> <span data-ttu-id="f7c5d-180">Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-180">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="f7c5d-181">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-181">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="f7c5d-182">A társított szolgáltatás típusa [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="f7c5d-183">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="f7c5d-184">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="f7c5d-185">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="f7c5d-186">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="f7c5d-187">hello minta másol adatokat egy lekérdezés eredményét Amazon Redshift tooa BLOB minden órában.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-187">hello sample copies data from a query result in Amazon Redshift tooa blob every hour.</span></span> <span data-ttu-id="f7c5d-188">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-188">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="f7c5d-189">**Amazon Redshift társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="f7c5d-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< hello IP address or host name of hello Amazon Redshift server >",
            "port": <hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.>,
            "database": "<hello database name of hello Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="f7c5d-190">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="f7c5d-190">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="f7c5d-191">**Amazon Redshift bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f7c5d-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="f7c5d-192">Beállítás `"external": true` hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-192">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="f7c5d-193">Ez a tulajdonság tootrue be egy bemeneti adatkészlet nem hello feldolgozási soros tevékenység által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-193">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

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

<span data-ttu-id="f7c5d-194">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f7c5d-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="f7c5d-195">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-195">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="f7c5d-196">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-196">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="f7c5d-197">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-197">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="f7c5d-198">**A folyamat Azure Redshift (RelationalSource) és a fogadó Blob másolási tevékenység:**</span><span class="sxs-lookup"><span data-stu-id="f7c5d-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="f7c5d-199">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-199">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="f7c5d-200">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-200">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="f7c5d-201">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-201">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="f7c5d-202">Az Amazon Redshift leképezésének</span><span class="sxs-lookup"><span data-stu-id="f7c5d-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="f7c5d-203">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-203">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="f7c5d-204">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="f7c5d-204">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="f7c5d-205">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="f7c5d-205">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="f7c5d-206">Adatok tooAmazon Redshift áthelyezésekor hello leképezéseket a következő Amazon Redshift típusok too.NET típusok használtak.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-206">When moving data tooAmazon Redshift, hello following mappings are used from Amazon Redshift types too.NET types.</span></span>

| <span data-ttu-id="f7c5d-207">Amazon Redshift típusa</span><span class="sxs-lookup"><span data-stu-id="f7c5d-207">Amazon Redshift Type</span></span> | <span data-ttu-id="f7c5d-208">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="f7c5d-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="f7c5d-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="f7c5d-209">SMALLINT</span></span> |<span data-ttu-id="f7c5d-210">Int16</span><span class="sxs-lookup"><span data-stu-id="f7c5d-210">Int16</span></span> |
| <span data-ttu-id="f7c5d-211">EGÉSZ SZÁM</span><span class="sxs-lookup"><span data-stu-id="f7c5d-211">INTEGER</span></span> |<span data-ttu-id="f7c5d-212">Int32</span><span class="sxs-lookup"><span data-stu-id="f7c5d-212">Int32</span></span> |
| <span data-ttu-id="f7c5d-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="f7c5d-213">BIGINT</span></span> |<span data-ttu-id="f7c5d-214">Int64</span><span class="sxs-lookup"><span data-stu-id="f7c5d-214">Int64</span></span> |
| <span data-ttu-id="f7c5d-215">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="f7c5d-215">DECIMAL</span></span> |<span data-ttu-id="f7c5d-216">Decimális</span><span class="sxs-lookup"><span data-stu-id="f7c5d-216">Decimal</span></span> |
| <span data-ttu-id="f7c5d-217">VALÓS</span><span class="sxs-lookup"><span data-stu-id="f7c5d-217">REAL</span></span> |<span data-ttu-id="f7c5d-218">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="f7c5d-218">Single</span></span> |
| <span data-ttu-id="f7c5d-219">A KÉTSZERES PONTOSSÁG</span><span class="sxs-lookup"><span data-stu-id="f7c5d-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="f7c5d-220">Dupla</span><span class="sxs-lookup"><span data-stu-id="f7c5d-220">Double</span></span> |
| <span data-ttu-id="f7c5d-221">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="f7c5d-221">BOOLEAN</span></span> |<span data-ttu-id="f7c5d-222">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7c5d-222">String</span></span> |
| <span data-ttu-id="f7c5d-223">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="f7c5d-223">CHAR</span></span> |<span data-ttu-id="f7c5d-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7c5d-224">String</span></span> |
| <span data-ttu-id="f7c5d-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="f7c5d-225">VARCHAR</span></span> |<span data-ttu-id="f7c5d-226">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7c5d-226">String</span></span> |
| <span data-ttu-id="f7c5d-227">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="f7c5d-227">DATE</span></span> |<span data-ttu-id="f7c5d-228">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f7c5d-228">DateTime</span></span> |
| <span data-ttu-id="f7c5d-229">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="f7c5d-229">TIMESTAMP</span></span> |<span data-ttu-id="f7c5d-230">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f7c5d-230">DateTime</span></span> |
| <span data-ttu-id="f7c5d-231">SZÖVEG</span><span class="sxs-lookup"><span data-stu-id="f7c5d-231">TEXT</span></span> |<span data-ttu-id="f7c5d-232">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7c5d-232">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="f7c5d-233">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="f7c5d-233">Map source toosink columns</span></span>
<span data-ttu-id="f7c5d-234">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f7c5d-234">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="f7c5d-235">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="f7c5d-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="f7c5d-236">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-236">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="f7c5d-237">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="f7c5d-238">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="f7c5d-239">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-239">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="f7c5d-240">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="f7c5d-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f7c5d-241">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="f7c5d-241">Performance and Tuning</span></span>
<span data-ttu-id="f7c5d-242">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7c5d-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7c5d-243">Next Steps</span></span>
<span data-ttu-id="f7c5d-244">Tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="f7c5d-244">See hello following articles:</span></span>

* <span data-ttu-id="f7c5d-245">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="f7c5d-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
