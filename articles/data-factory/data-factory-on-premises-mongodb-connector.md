---
title: "használatával a Data Factory MongoDB aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatait MongoDB adatbázis-Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a><span data-ttu-id="a278c-103">Helyezze át az adatokat a MongoDB Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="a278c-103">Move data From MongoDB using Azure Data Factory</span></span>
<span data-ttu-id="a278c-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove az adatbázisból a helyszíni MongoDB a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="a278c-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MongoDB database.</span></span> <span data-ttu-id="a278c-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="a278c-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="a278c-106">Egy helyszíni MongoDB adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="a278c-106">You can copy data from an on-premises MongoDB data store tooany supported sink data store.</span></span> <span data-ttu-id="a278c-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="a278c-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a278c-108">Adat-előállító jelenleg csak a MongoDB-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatokat tároló tooan MongoDB adattárolót.</span><span class="sxs-lookup"><span data-stu-id="a278c-108">Data factory currently supports only moving data from a MongoDB data store tooother data stores, but not for moving data from other data stores tooan MongoDB datastore.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a278c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a278c-109">Prerequisites</span></span>
<span data-ttu-id="a278c-110">Hello Azure Data Factory szolgáltatás toobe képes tooconnect tooyour helyszíni MongoDB adatbázis telepítenie kell a következő összetevők hello:</span><span class="sxs-lookup"><span data-stu-id="a278c-110">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises MongoDB database, you must install hello following components:</span></span>

- <span data-ttu-id="a278c-111">MongoDB-verziók a következők: 2.4, 2.6, 3.0-s és 3.2-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="a278c-111">Supported MongoDB versions are:  2.4, 2.6, 3.0, and 3.2.</span></span>
- <span data-ttu-id="a278c-112">Az adatkezelési átjáró hello ugyanaz a gép állomások hello adatbázis vagy egy másik számítógépre tooavoid hello adatbázis erőforrás használják.</span><span class="sxs-lookup"><span data-stu-id="a278c-112">Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="a278c-113">Az adatkezelési átjáró egy szoftver, amely összeköti a helyszíni adatok források toocloud szolgáltatások biztonságának és kezelésének módja.</span><span class="sxs-lookup"><span data-stu-id="a278c-113">Data Management Gateway is a software that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="a278c-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="a278c-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="a278c-115">Lásd: [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk hello átjáró egy adatok adatcsatorna toomove adatok beállításának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="a278c-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

    <span data-ttu-id="a278c-116">Hello átjáró telepítésekor automatikusan telepíti a Microsoft MongoDB ODBC-illesztőprogram használt tooconnect tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="a278c-116">When you install hello gateway, it automatically installs a Microsoft MongoDB ODBC driver used tooconnect tooMongoDB.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a278c-117">Akkor is, ha az Azure IaaS virtuális gépeken fut. toouse hello átjáró tooconnect tooMongoDB kell.</span><span class="sxs-lookup"><span data-stu-id="a278c-117">You need toouse hello gateway tooconnect tooMongoDB even if it is hosted in Azure IaaS VMs.</span></span> <span data-ttu-id="a278c-118">Ha a felhőben üzemeltetett MongoDB tooconnect tooan példányának próbált, hello infrastruktúra-szolgáltatási virtuális gép is hello átjárópéldány is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="a278c-118">If you are trying tooconnect tooan instance of MongoDB hosted in cloud, you can also install hello gateway instance in hello IaaS VM.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a278c-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a278c-119">Getting started</span></span>
<span data-ttu-id="a278c-120">A másolási tevékenység, mely az adatok egy helyszíni MongoDB adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="a278c-120">You can create a pipeline with a copy activity that moves data from an on-premises MongoDB data store by using different tools/APIs.</span></span>

<span data-ttu-id="a278c-121">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="a278c-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a278c-122">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a278c-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="a278c-123">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="a278c-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a278c-124">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="a278c-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a278c-125">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="a278c-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="a278c-126">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="a278c-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="a278c-127">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="a278c-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="a278c-128">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="a278c-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a278c-129">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="a278c-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="a278c-130">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="a278c-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="a278c-131">Az adat-előállító entitások, amelyek egy helyszíni MongoDB adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="a278c-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MongoDB data store, see [JSON example: Copy data from MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a278c-132">a következő szakaszok hello használt toodefine adat-előállító entitások adott tooMongoDB forrás JSON-tulajdonághoz részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="a278c-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooMongoDB source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a278c-133">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="a278c-133">Linked service properties</span></span>
<span data-ttu-id="a278c-134">hello alábbi táblázatban megadott JSON-elemek leírása túl**OnPremisesMongoDB** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a278c-134">hello following table provides description for JSON elements specific too**OnPremisesMongoDB** linked service.</span></span>

| <span data-ttu-id="a278c-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a278c-135">Property</span></span> | <span data-ttu-id="a278c-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="a278c-136">Description</span></span> | <span data-ttu-id="a278c-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a278c-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a278c-138">type</span><span class="sxs-lookup"><span data-stu-id="a278c-138">type</span></span> |<span data-ttu-id="a278c-139">hello type tulajdonságot kell beállítani: **OnPremisesMongoDb**</span><span class="sxs-lookup"><span data-stu-id="a278c-139">hello type property must be set to: **OnPremisesMongoDb**</span></span> |<span data-ttu-id="a278c-140">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-140">Yes</span></span> |
| <span data-ttu-id="a278c-141">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a278c-141">server</span></span> |<span data-ttu-id="a278c-142">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a278c-142">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="a278c-143">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-143">Yes</span></span> |
| <span data-ttu-id="a278c-144">port</span><span class="sxs-lookup"><span data-stu-id="a278c-144">port</span></span> |<span data-ttu-id="a278c-145">TCP-portot, amely a MongoDB-kiszolgálóhoz hello toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="a278c-145">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="a278c-146">Nem kötelező, alapértelmezett érték: 27017</span><span class="sxs-lookup"><span data-stu-id="a278c-146">Optional, default value: 27017</span></span> |
| <span data-ttu-id="a278c-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a278c-147">authenticationType</span></span> |<span data-ttu-id="a278c-148">Alapszintű, vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="a278c-148">Basic, or Anonymous.</span></span> |<span data-ttu-id="a278c-149">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-149">Yes</span></span> |
| <span data-ttu-id="a278c-150">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a278c-150">username</span></span> |<span data-ttu-id="a278c-151">Felhasználói fiók MongoDB tooaccess.</span><span class="sxs-lookup"><span data-stu-id="a278c-151">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="a278c-152">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="a278c-152">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="a278c-153">jelszó</span><span class="sxs-lookup"><span data-stu-id="a278c-153">password</span></span> |<span data-ttu-id="a278c-154">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a278c-154">Password for hello user.</span></span> |<span data-ttu-id="a278c-155">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="a278c-155">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="a278c-156">authSource</span><span class="sxs-lookup"><span data-stu-id="a278c-156">authSource</span></span> |<span data-ttu-id="a278c-157">Hello MongoDB-adatbázist, amelyet toouse toocheck a hitelesítő adatok hitelesítéshez neve.</span><span class="sxs-lookup"><span data-stu-id="a278c-157">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="a278c-158">Választható (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="a278c-158">Optional (if basic authentication is used).</span></span> <span data-ttu-id="a278c-159">alapértelmezett: hello rendszergazdai fiókot és a databaseName tulajdonsággal megadott hello adatbázis használ.</span><span class="sxs-lookup"><span data-stu-id="a278c-159">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="a278c-160">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="a278c-160">databaseName</span></span> |<span data-ttu-id="a278c-161">Hello MongoDB-adatbázist, amelyet az tooaccess neve.</span><span class="sxs-lookup"><span data-stu-id="a278c-161">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="a278c-162">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-162">Yes</span></span> |
| <span data-ttu-id="a278c-163">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a278c-163">gatewayName</span></span> |<span data-ttu-id="a278c-164">Hello adattár hozzáférő hello átjáró neve.</span><span class="sxs-lookup"><span data-stu-id="a278c-164">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="a278c-165">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-165">Yes</span></span> |
| <span data-ttu-id="a278c-166">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="a278c-166">encryptedCredential</span></span> |<span data-ttu-id="a278c-167">A hitelesítőadat-átjáró által titkosított.</span><span class="sxs-lookup"><span data-stu-id="a278c-167">Credential encrypted by gateway.</span></span> |<span data-ttu-id="a278c-168">Optional</span><span class="sxs-lookup"><span data-stu-id="a278c-168">Optional</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="a278c-169">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a278c-169">Dataset properties</span></span>
<span data-ttu-id="a278c-170">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a278c-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a278c-171">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="a278c-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a278c-172">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a278c-172">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="a278c-173">hello typeProperties szakasz típusú adatkészlet **MongoDbCollection** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="a278c-173">hello typeProperties section for dataset of type **MongoDbCollection** has hello following properties:</span></span>

| <span data-ttu-id="a278c-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a278c-174">Property</span></span> | <span data-ttu-id="a278c-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="a278c-175">Description</span></span> | <span data-ttu-id="a278c-176">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a278c-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a278c-177">CollectionName</span><span class="sxs-lookup"><span data-stu-id="a278c-177">collectionName</span></span> |<span data-ttu-id="a278c-178">A MongoDB adatbázis hello gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="a278c-178">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="a278c-179">Igen</span><span class="sxs-lookup"><span data-stu-id="a278c-179">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="a278c-180">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a278c-180">Copy activity properties</span></span>
<span data-ttu-id="a278c-181">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a278c-181">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a278c-182">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a278c-182">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="a278c-183">Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="a278c-183">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="a278c-184">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="a278c-184">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="a278c-185">Ha hello forrás típusa nem **MongoDbSource** typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="a278c-185">When hello source is of type **MongoDbSource** hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="a278c-186">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a278c-186">Property</span></span> | <span data-ttu-id="a278c-187">Leírás</span><span class="sxs-lookup"><span data-stu-id="a278c-187">Description</span></span> | <span data-ttu-id="a278c-188">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="a278c-188">Allowed values</span></span> | <span data-ttu-id="a278c-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a278c-189">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a278c-190">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="a278c-190">query</span></span> |<span data-ttu-id="a278c-191">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="a278c-191">Use hello custom query tooread data.</span></span> |<span data-ttu-id="a278c-192">SQL-92 lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a278c-192">SQL-92 query string.</span></span> <span data-ttu-id="a278c-193">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="a278c-193">For example: select * from MyTable.</span></span> |<span data-ttu-id="a278c-194">Nem (Ha **collectionName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="a278c-194">No (if **collectionName** of **dataset** is specified)</span></span> |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a><span data-ttu-id="a278c-195">JSON-példa: adatok másolása az MongoDB tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="a278c-195">JSON example: Copy data from MongoDB tooAzure Blob</span></span>
<span data-ttu-id="a278c-196">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a278c-196">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a278c-197">Azt illusztrálja, hogyan egy helyszíni MongoDB tooan Azure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="a278c-197">It shows how toocopy data from an on-premises MongoDB tooan Azure Blob Storage.</span></span> <span data-ttu-id="a278c-198">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="a278c-198">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="a278c-199">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a278c-199">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="a278c-200">A társított szolgáltatás típusa [OnPremisesMongoDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a278c-200">A linked service of type [OnPremisesMongoDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="a278c-201">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a278c-201">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a278c-202">Bemeneti [dataset](data-factory-create-datasets.md) típusú [MongoDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a278c-202">An input [dataset](data-factory-create-datasets.md) of type [MongoDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="a278c-203">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a278c-203">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a278c-204">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [MongoDbSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a278c-204">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [MongoDbSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a278c-205">hello minta másol adatokat egy lekérdezés eredményét a MongoDB adatbázis tooa blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="a278c-205">hello sample copies data from a query result in MongoDB database tooa blob every hour.</span></span> <span data-ttu-id="a278c-206">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="a278c-206">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="a278c-207">Első lépésként, a telepítő hello az adatkezelési átjáró szerint hello hello utasításait [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a278c-207">As a first step, setup hello data management gateway as per hello instructions in hello [Data Management Gateway](data-factory-data-management-gateway.md) article.</span></span>

<span data-ttu-id="a278c-208">**MongoDB társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="a278c-208">**MongoDB linked service:**</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

<span data-ttu-id="a278c-209">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="a278c-209">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="a278c-210">**MongoDB bemeneti adatkészlet:** "external" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="a278c-210">**MongoDB input dataset:** Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="a278c-211">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="a278c-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="a278c-212">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="a278c-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a278c-213">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="a278c-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="a278c-214">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="a278c-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="a278c-215">**Másolási tevékenység a MongoDB-forrás és a Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="a278c-215">**Copy activity in a pipeline with MongoDB source and Blob sink:**</span></span>

<span data-ttu-id="a278c-216">hello folyamat, amely a bemeneti és kimeneti adatkészletek fent konfigurált toouse hello és ütemezett toorun óránként másolatot tevékenységet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a278c-216">hello pipeline contains a Copy Activity that is configured toouse hello above input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="a278c-217">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**MongoDbSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a278c-217">In hello pipeline JSON definition, hello **source** type is set too**MongoDbSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="a278c-218">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="a278c-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a><span data-ttu-id="a278c-219">Adat-előállító sémája</span><span class="sxs-lookup"><span data-stu-id="a278c-219">Schema by Data Factory</span></span>
<span data-ttu-id="a278c-220">Az Azure Data Factory szolgáltatásnak kikövetkezteti a MongoDB-gyűjteményből séma hello legújabb 100 dokumentumok hello gyűjtemény használatával.</span><span class="sxs-lookup"><span data-stu-id="a278c-220">Azure Data Factory service infers schema from a MongoDB collection by using hello latest 100 documents in hello collection.</span></span> <span data-ttu-id="a278c-221">A 100 dokumentumok teljes séma nem tartalmaznak, ha azokat az oszlopokat hello másolási művelet során előfordulhat, hogy figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="a278c-221">If these 100 documents do not contain full schema, some columns may be ignored during hello copy operation.</span></span>

## <a name="type-mapping-for-mongodb"></a><span data-ttu-id="a278c-222">Mongodb-protokolltámogatással leképezésének</span><span class="sxs-lookup"><span data-stu-id="a278c-222">Type mapping for MongoDB</span></span>
<span data-ttu-id="a278c-223">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="a278c-223">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="a278c-224">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="a278c-224">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="a278c-225">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="a278c-225">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="a278c-226">Amikor helyezi át a következő adatok tooMongoDB hello hozzárendelések MongoDB típusok too.NET típusok használtak.</span><span class="sxs-lookup"><span data-stu-id="a278c-226">When moving data tooMongoDB hello following mappings are used from MongoDB types too.NET types.</span></span>

| <span data-ttu-id="a278c-227">MongoDB-típus</span><span class="sxs-lookup"><span data-stu-id="a278c-227">MongoDB type</span></span> | <span data-ttu-id="a278c-228">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="a278c-228">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="a278c-229">Bináris</span><span class="sxs-lookup"><span data-stu-id="a278c-229">Binary</span></span> |<span data-ttu-id="a278c-230">Byte]</span><span class="sxs-lookup"><span data-stu-id="a278c-230">Byte[]</span></span> |
| <span data-ttu-id="a278c-231">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="a278c-231">Boolean</span></span> |<span data-ttu-id="a278c-232">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="a278c-232">Boolean</span></span> |
| <span data-ttu-id="a278c-233">Dátum</span><span class="sxs-lookup"><span data-stu-id="a278c-233">Date</span></span> |<span data-ttu-id="a278c-234">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="a278c-234">DateTime</span></span> |
| <span data-ttu-id="a278c-235">NumberDouble</span><span class="sxs-lookup"><span data-stu-id="a278c-235">NumberDouble</span></span> |<span data-ttu-id="a278c-236">Dupla</span><span class="sxs-lookup"><span data-stu-id="a278c-236">Double</span></span> |
| <span data-ttu-id="a278c-237">NumberInt</span><span class="sxs-lookup"><span data-stu-id="a278c-237">NumberInt</span></span> |<span data-ttu-id="a278c-238">Int32</span><span class="sxs-lookup"><span data-stu-id="a278c-238">Int32</span></span> |
| <span data-ttu-id="a278c-239">NumberLong</span><span class="sxs-lookup"><span data-stu-id="a278c-239">NumberLong</span></span> |<span data-ttu-id="a278c-240">Int64</span><span class="sxs-lookup"><span data-stu-id="a278c-240">Int64</span></span> |
| <span data-ttu-id="a278c-241">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="a278c-241">ObjectID</span></span> |<span data-ttu-id="a278c-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a278c-242">String</span></span> |
| <span data-ttu-id="a278c-243">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a278c-243">String</span></span> |<span data-ttu-id="a278c-244">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a278c-244">String</span></span> |
| <span data-ttu-id="a278c-245">UUID</span><span class="sxs-lookup"><span data-stu-id="a278c-245">UUID</span></span> |<span data-ttu-id="a278c-246">GUID</span><span class="sxs-lookup"><span data-stu-id="a278c-246">Guid</span></span> |
| <span data-ttu-id="a278c-247">Objektum</span><span class="sxs-lookup"><span data-stu-id="a278c-247">Object</span></span> |<span data-ttu-id="a278c-248">Renormalized történő egybesimítására "_" beágyazott elválasztójelként oszlopok</span><span class="sxs-lookup"><span data-stu-id="a278c-248">Renormalized into flatten columns with “_” as nested separator</span></span> |

> [!NOTE]
> <span data-ttu-id="a278c-249">virtuális táblák, tömbök-támogatással kapcsolatos toolearn tekintse meg a túl[támogatja az összetett típusok virtuális táblák használata](#support-for-complex-types-using-virtual-tables) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="a278c-249">toolearn about support for arrays using virtual tables, refer too[Support for complex types using virtual tables](#support-for-complex-types-using-virtual-tables) section below.</span></span>

<span data-ttu-id="a278c-250">Jelenleg nem támogatottak a következő MongoDB adattípusok hello: DBPointer, JavaScript, Max percenkénti kulcs, reguláris kifejezés, szimbólum, Timestamp, meghatározatlan</span><span class="sxs-lookup"><span data-stu-id="a278c-250">Currently, hello following MongoDB data types are not supported: DBPointer, JavaScript, Max/Min key, Regular Expression, Symbol, Timestamp, Undefined</span></span>

## <a name="support-for-complex-types-using-virtual-tables"></a><span data-ttu-id="a278c-251">Virtuális táblák használata összetett típusok támogatása</span><span class="sxs-lookup"><span data-stu-id="a278c-251">Support for complex types using virtual tables</span></span>
<span data-ttu-id="a278c-252">Az Azure Data Factory egy beépített ODBC illesztőprogram tooconnect tooand példány adatait használja a MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a278c-252">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your MongoDB database.</span></span> <span data-ttu-id="a278c-253">Hello dokumentumok között különböző típusú tömb, vagy objektumok például összetett típusok hello illesztőprogram újra normalizálja adatok megfelelő virtuális táblákba.</span><span class="sxs-lookup"><span data-stu-id="a278c-253">For complex types such as arrays or objects with different types across hello documents, hello driver re-normalizes data into corresponding virtual tables.</span></span> <span data-ttu-id="a278c-254">Pontosabban Ha a tábla tartalmaz ilyen oszlopok, hello illesztőprogram állít elő, a következő virtuális táblák hello:</span><span class="sxs-lookup"><span data-stu-id="a278c-254">Specifically, if a table contains such columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="a278c-255">A **alaptábla**, amely tartalmazza hello hello valós táblából hello összetett típusú oszlopok ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a278c-255">A **base table**, which contains hello same data as hello real table except for hello complex type columns.</span></span> <span data-ttu-id="a278c-256">hello alaptábla hello ugyanazt a nevet használja, mint hello valós táblázatot, amely azt jelöli.</span><span class="sxs-lookup"><span data-stu-id="a278c-256">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="a278c-257">A **virtuális tábla** minden összetett típusú oszlophoz, amely kiterjeszti hello beágyazott adatok.</span><span class="sxs-lookup"><span data-stu-id="a278c-257">A **virtual table** for each complex type column, which expands hello nested data.</span></span> <span data-ttu-id="a278c-258">hello virtuális táblák megnevezett hello valós tábla hello neve, "_" elválasztó és hello tömb vagy objektum hello neve.</span><span class="sxs-lookup"><span data-stu-id="a278c-258">hello virtual tables are named using hello name of hello real table, a separator “_” and hello name of hello array or object.</span></span>

<span data-ttu-id="a278c-259">Virtuális táblák tekintse meg a hello valós tábla toohello adatokat, lehetővé teszi hello illesztőprogram tooaccess hello nem normalizált adatok.</span><span class="sxs-lookup"><span data-stu-id="a278c-259">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="a278c-260">Részletekért lásd: Példa szakaszban olvashatók.</span><span class="sxs-lookup"><span data-stu-id="a278c-260">See Example section below details.</span></span> <span data-ttu-id="a278c-261">MongoDB-tömbök hello tartalmának lekérdezése és hello virtuális tábla érheti el.</span><span class="sxs-lookup"><span data-stu-id="a278c-261">You can access hello content of MongoDB arrays by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="a278c-262">Használhatja a hello [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively nézet hello hello virtuális táblázatot is MongoDB-adatbázist a táblák listáját és hello található adatok megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="a278c-262">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in MongoDB database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="a278c-263">Is hello másolása varázsló az olyan lekérdezést, és toosee hello eredmény ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a278c-263">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="a278c-264">Példa</span><span class="sxs-lookup"><span data-stu-id="a278c-264">Example</span></span>
<span data-ttu-id="a278c-265">"ExampleTable" alatt például MongoDB tábla egy objektumokból álló tömb egy oszlopot az egyes cellák – számlákat és a skaláris típusok – minősítések tömbje egy oszlop.</span><span class="sxs-lookup"><span data-stu-id="a278c-265">For example, “ExampleTable” below is a MongoDB table that has one column with an array of Objects in each cell – Invoices, and one column with an array of Scalar types – Ratings.</span></span>

| <span data-ttu-id="a278c-266">_id</span><span class="sxs-lookup"><span data-stu-id="a278c-266">_id</span></span> | <span data-ttu-id="a278c-267">Ügyfél neve</span><span class="sxs-lookup"><span data-stu-id="a278c-267">Customer Name</span></span> | <span data-ttu-id="a278c-268">Számlák</span><span class="sxs-lookup"><span data-stu-id="a278c-268">Invoices</span></span> | <span data-ttu-id="a278c-269">Szolgáltatási szint</span><span class="sxs-lookup"><span data-stu-id="a278c-269">Service Level</span></span> | <span data-ttu-id="a278c-270">Minősítések</span><span class="sxs-lookup"><span data-stu-id="a278c-270">Ratings</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="a278c-271">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-271">1111</span></span> |<span data-ttu-id="a278c-272">ABC</span><span class="sxs-lookup"><span data-stu-id="a278c-272">ABC</span></span> |<span data-ttu-id="a278c-273">[{invoice_id: "123", cikk: "toaster", az ár: "456" kedvezményes: "0,2"}, {invoice_id: "124" elem: "helyezzük", az ár: "1235" kedvezményes: "0,2"}]</span><span class="sxs-lookup"><span data-stu-id="a278c-273">[{invoice_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice_id:”124”, item:”oven”, price: ”1235”, discount: ”0.2”}]</span></span> |<span data-ttu-id="a278c-274">Ezüst</span><span class="sxs-lookup"><span data-stu-id="a278c-274">Silver</span></span> |<span data-ttu-id="a278c-275">[5,6]</span><span class="sxs-lookup"><span data-stu-id="a278c-275">[5,6]</span></span> |
| <span data-ttu-id="a278c-276">2222</span><span class="sxs-lookup"><span data-stu-id="a278c-276">2222</span></span> |<span data-ttu-id="a278c-277">XYZ</span><span class="sxs-lookup"><span data-stu-id="a278c-277">XYZ</span></span> |<span data-ttu-id="a278c-278">[{invoice_id: "135", cikk: "kombinált hűtőszekrények", az ár: "12543" kedvezménnyel: "0,0"}]</span><span class="sxs-lookup"><span data-stu-id="a278c-278">[{invoice_id:”135”, item:”fridge”, price: ”12543”, discount: ”0.0”}]</span></span> |<span data-ttu-id="a278c-279">Arany</span><span class="sxs-lookup"><span data-stu-id="a278c-279">Gold</span></span> |<span data-ttu-id="a278c-280">[1,2]</span><span class="sxs-lookup"><span data-stu-id="a278c-280">[1,2]</span></span> |

<span data-ttu-id="a278c-281">hello illesztőprogram több virtuális táblák toorepresent hoz létre a egyetlen tábla.</span><span class="sxs-lookup"><span data-stu-id="a278c-281">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="a278c-282">első virtuális tábla hello hello alaptábla "ExampleTable" alább látható nevű.</span><span class="sxs-lookup"><span data-stu-id="a278c-282">hello first virtual table is hello base table named “ExampleTable”, shown below.</span></span> <span data-ttu-id="a278c-283">hello alaptábla hello eredeti tábla összes hello adatokat tartalmaz, de hello tömbök hello adatait kimaradt, és ki van bontva hello virtuális táblában.</span><span class="sxs-lookup"><span data-stu-id="a278c-283">hello base table contains all hello data of hello original table, but hello data from hello arrays has been omitted and is expanded in hello virtual tables.</span></span>

| <span data-ttu-id="a278c-284">_id</span><span class="sxs-lookup"><span data-stu-id="a278c-284">_id</span></span> | <span data-ttu-id="a278c-285">Ügyfél neve</span><span class="sxs-lookup"><span data-stu-id="a278c-285">Customer Name</span></span> | <span data-ttu-id="a278c-286">Szolgáltatási szint</span><span class="sxs-lookup"><span data-stu-id="a278c-286">Service Level</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a278c-287">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-287">1111</span></span> |<span data-ttu-id="a278c-288">ABC</span><span class="sxs-lookup"><span data-stu-id="a278c-288">ABC</span></span> |<span data-ttu-id="a278c-289">Ezüst</span><span class="sxs-lookup"><span data-stu-id="a278c-289">Silver</span></span> |
| <span data-ttu-id="a278c-290">2222</span><span class="sxs-lookup"><span data-stu-id="a278c-290">2222</span></span> |<span data-ttu-id="a278c-291">XYZ</span><span class="sxs-lookup"><span data-stu-id="a278c-291">XYZ</span></span> |<span data-ttu-id="a278c-292">Arany</span><span class="sxs-lookup"><span data-stu-id="a278c-292">Gold</span></span> |

<span data-ttu-id="a278c-293">hello táblázatokban hello virtuális táblák megjelenítése, amelyek megfelelnek az eredeti tömbök hello hello példában.</span><span class="sxs-lookup"><span data-stu-id="a278c-293">hello following tables show hello virtual tables that represent hello original arrays in hello example.</span></span> <span data-ttu-id="a278c-294">Ezek a táblázatok hello alábbiakat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a278c-294">These tables contain hello following:</span></span>

* <span data-ttu-id="a278c-295">Hivatkozás biztonsági toohello eredeti elsődleges kulcsként megadott oszlop megfelelő toohello hello eredeti tömb sora (keresztül hello _id oszlop)</span><span class="sxs-lookup"><span data-stu-id="a278c-295">A reference back toohello original primary key column corresponding toohello row of hello original array (via hello _id column)</span></span>
* <span data-ttu-id="a278c-296">Utalhat, hogy hello adatok hello eredeti tömbön belüli hello pozíciója</span><span class="sxs-lookup"><span data-stu-id="a278c-296">An indication of hello position of hello data within hello original array</span></span>
* <span data-ttu-id="a278c-297">hello kibontva az egyes elemekhez hello tömbön belüli adatok</span><span class="sxs-lookup"><span data-stu-id="a278c-297">hello expanded data for each element within hello array</span></span>

<span data-ttu-id="a278c-298">"ExampleTable_Invoices". tábla:</span><span class="sxs-lookup"><span data-stu-id="a278c-298">Table “ExampleTable_Invoices”:</span></span>

| <span data-ttu-id="a278c-299">_id</span><span class="sxs-lookup"><span data-stu-id="a278c-299">_id</span></span> | <span data-ttu-id="a278c-300">ExampleTable_Invoices_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="a278c-300">ExampleTable_Invoices_dim1_idx</span></span> | <span data-ttu-id="a278c-301">invoice_id</span><span class="sxs-lookup"><span data-stu-id="a278c-301">invoice_id</span></span> | <span data-ttu-id="a278c-302">Elem</span><span class="sxs-lookup"><span data-stu-id="a278c-302">item</span></span> | <span data-ttu-id="a278c-303">price</span><span class="sxs-lookup"><span data-stu-id="a278c-303">price</span></span> | <span data-ttu-id="a278c-304">Kedvezmény</span><span class="sxs-lookup"><span data-stu-id="a278c-304">Discount</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a278c-305">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-305">1111</span></span> |<span data-ttu-id="a278c-306">0</span><span class="sxs-lookup"><span data-stu-id="a278c-306">0</span></span> |<span data-ttu-id="a278c-307">123</span><span class="sxs-lookup"><span data-stu-id="a278c-307">123</span></span> |<span data-ttu-id="a278c-308">a toaster</span><span class="sxs-lookup"><span data-stu-id="a278c-308">toaster</span></span> |<span data-ttu-id="a278c-309">456</span><span class="sxs-lookup"><span data-stu-id="a278c-309">456</span></span> |<span data-ttu-id="a278c-310">0.2</span><span class="sxs-lookup"><span data-stu-id="a278c-310">0.2</span></span> |
| <span data-ttu-id="a278c-311">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-311">1111</span></span> |<span data-ttu-id="a278c-312">1</span><span class="sxs-lookup"><span data-stu-id="a278c-312">1</span></span> |<span data-ttu-id="a278c-313">124</span><span class="sxs-lookup"><span data-stu-id="a278c-313">124</span></span> |<span data-ttu-id="a278c-314">Helyezzük</span><span class="sxs-lookup"><span data-stu-id="a278c-314">oven</span></span> |<span data-ttu-id="a278c-315">1235</span><span class="sxs-lookup"><span data-stu-id="a278c-315">1235</span></span> |<span data-ttu-id="a278c-316">0.2</span><span class="sxs-lookup"><span data-stu-id="a278c-316">0.2</span></span> |
| <span data-ttu-id="a278c-317">2222</span><span class="sxs-lookup"><span data-stu-id="a278c-317">2222</span></span> |<span data-ttu-id="a278c-318">0</span><span class="sxs-lookup"><span data-stu-id="a278c-318">0</span></span> |<span data-ttu-id="a278c-319">135</span><span class="sxs-lookup"><span data-stu-id="a278c-319">135</span></span> |<span data-ttu-id="a278c-320">kombinált hűtőszekrények</span><span class="sxs-lookup"><span data-stu-id="a278c-320">fridge</span></span> |<span data-ttu-id="a278c-321">12543</span><span class="sxs-lookup"><span data-stu-id="a278c-321">12543</span></span> |<span data-ttu-id="a278c-322">0.0</span><span class="sxs-lookup"><span data-stu-id="a278c-322">0.0</span></span> |

<span data-ttu-id="a278c-323">"ExampleTable_Ratings". tábla:</span><span class="sxs-lookup"><span data-stu-id="a278c-323">Table “ExampleTable_Ratings”:</span></span>

| <span data-ttu-id="a278c-324">_id</span><span class="sxs-lookup"><span data-stu-id="a278c-324">_id</span></span> | <span data-ttu-id="a278c-325">ExampleTable_Ratings_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="a278c-325">ExampleTable_Ratings_dim1_idx</span></span> | <span data-ttu-id="a278c-326">ExampleTable_Ratings</span><span class="sxs-lookup"><span data-stu-id="a278c-326">ExampleTable_Ratings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a278c-327">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-327">1111</span></span> |<span data-ttu-id="a278c-328">0</span><span class="sxs-lookup"><span data-stu-id="a278c-328">0</span></span> |<span data-ttu-id="a278c-329">5</span><span class="sxs-lookup"><span data-stu-id="a278c-329">5</span></span> |
| <span data-ttu-id="a278c-330">1111</span><span class="sxs-lookup"><span data-stu-id="a278c-330">1111</span></span> |<span data-ttu-id="a278c-331">1</span><span class="sxs-lookup"><span data-stu-id="a278c-331">1</span></span> |<span data-ttu-id="a278c-332">6</span><span class="sxs-lookup"><span data-stu-id="a278c-332">6</span></span> |
| <span data-ttu-id="a278c-333">2222</span><span class="sxs-lookup"><span data-stu-id="a278c-333">2222</span></span> |<span data-ttu-id="a278c-334">0</span><span class="sxs-lookup"><span data-stu-id="a278c-334">0</span></span> |<span data-ttu-id="a278c-335">1</span><span class="sxs-lookup"><span data-stu-id="a278c-335">1</span></span> |
| <span data-ttu-id="a278c-336">2222</span><span class="sxs-lookup"><span data-stu-id="a278c-336">2222</span></span> |<span data-ttu-id="a278c-337">1</span><span class="sxs-lookup"><span data-stu-id="a278c-337">1</span></span> |<span data-ttu-id="a278c-338">2</span><span class="sxs-lookup"><span data-stu-id="a278c-338">2</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="a278c-339">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="a278c-339">Map source toosink columns</span></span>
<span data-ttu-id="a278c-340">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a278c-340">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a278c-341">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="a278c-341">Repeatable read from relational sources</span></span>
<span data-ttu-id="a278c-342">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="a278c-342">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="a278c-343">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="a278c-343">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a278c-344">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="a278c-344">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a278c-345">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="a278c-345">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a278c-346">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="a278c-346">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a278c-347">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="a278c-347">Performance and Tuning</span></span>
<span data-ttu-id="a278c-348">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="a278c-348">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a278c-349">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a278c-349">Next Steps</span></span>
<span data-ttu-id="a278c-350">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk részletes utasításokat az adatok folyamat létrehozása, amely áthelyezi adatait egy a helyszíni adatok tooan az Azure data tárolni.</span><span class="sxs-lookup"><span data-stu-id="a278c-350">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions for creating a data pipeline that moves data from an on-premises data store tooan Azure data store.</span></span>
