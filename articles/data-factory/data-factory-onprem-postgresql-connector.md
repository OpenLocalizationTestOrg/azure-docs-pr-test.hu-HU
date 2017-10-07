---
title: "aaaMove adatok a PostgreSQL Azure Data Factory használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával PostgreSQL-adatbázisból."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="1fdc4-103">Adatok áthelyezése az Azure Data Factory használatával PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1fdc4-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="1fdc4-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni PostgreSQL-adatbázishoz a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="1fdc4-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="1fdc4-106">Egy helyszíni PostgreSQL adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-106">You can copy data from an on-premises PostgreSQL data store tooany supported sink data store.</span></span> <span data-ttu-id="1fdc4-107">Adattároló mosdók hello másolási tevékenység által támogatott listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="1fdc4-108">Adat-előállító jelenleg adatbázis támogatja az adatok áthelyezése egy PostgreSQL-tooother adattárolókhoz, de nem az adatok áthelyezését más adatok tooan PostgreSQL-adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-108">Data factory currently supports moving data from a PostgreSQL database tooother data stores, but not for moving data from other data stores tooan PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1fdc4-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1fdc4-109">prerequisites</span></span>

<span data-ttu-id="1fdc4-110">Data Factory szolgáltatásnak csatlakozó tooon helyszíni PostgreSQL adatforrások az adatkezelési átjáró hello segítségével támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-110">Data Factory service supports connecting tooon-premises PostgreSQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="1fdc4-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="1fdc4-112">Átjáróra szükség, akkor is, ha egy Azure IaaS virtuális gép hello PostgreSQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-112">Gateway is required even if hello PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="1fdc4-113">Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-113">You can install gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="1fdc4-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="1fdc4-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="1fdc4-115">Supported versions and installation</span></span>
<span data-ttu-id="1fdc4-116">Az adatkezelési átjáró tooconnect toohello PostgreSQL-adatbázisból, telepítse a hello [PostgreSQL-Ngpsql adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 vagy fent a hello azonos az adatkezelési átjáró hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-116">For Data Management Gateway tooconnect toohello PostgreSQL Database, install hello [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="1fdc4-117">7.4 verzió PostgreSQL és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="1fdc4-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="1fdc4-118">Getting started</span></span>
<span data-ttu-id="1fdc4-119">A másolási tevékenység, mely az adatok egy helyszíni PostgreSQL adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="1fdc4-120">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="1fdc4-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="1fdc4-122">A következő eszközök toocreate adatcsatorna hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-122">You can also use hello following tools toocreate a pipeline:</span></span> 
    - <span data-ttu-id="1fdc4-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1fdc4-123">Azure portal</span></span>
    - <span data-ttu-id="1fdc4-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1fdc4-124">Visual Studio</span></span>
    - <span data-ttu-id="1fdc4-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1fdc4-125">Azure PowerShell</span></span>
    - <span data-ttu-id="1fdc4-126">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="1fdc4-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="1fdc4-127">.NET API</span><span class="sxs-lookup"><span data-stu-id="1fdc4-127">.NET API</span></span>
    - <span data-ttu-id="1fdc4-128">REST API</span><span class="sxs-lookup"><span data-stu-id="1fdc4-128">REST API</span></span>

     <span data-ttu-id="1fdc4-129">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="1fdc4-130">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="1fdc4-131">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="1fdc4-132">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="1fdc4-133">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="1fdc4-134">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="1fdc4-135">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="1fdc4-136">Az adat-előállító entitások, amelyek egy a helyszíni PostgreSQL adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="1fdc4-137">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa PostgreSQL adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="1fdc4-138">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="1fdc4-138">Linked service properties</span></span>
<span data-ttu-id="1fdc4-139">a következő táblázat hello biztosít JSON elemek adott tooPostgreSQL kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-139">hello following table provides description for JSON elements specific tooPostgreSQL linked service.</span></span>

| <span data-ttu-id="1fdc4-140">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1fdc4-140">Property</span></span> | <span data-ttu-id="1fdc4-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fdc4-141">Description</span></span> | <span data-ttu-id="1fdc4-142">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fdc4-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1fdc4-143">type</span><span class="sxs-lookup"><span data-stu-id="1fdc4-143">type</span></span> |<span data-ttu-id="1fdc4-144">hello type tulajdonságot kell beállítani: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-144">hello type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="1fdc4-145">Igen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-145">Yes</span></span> |
| <span data-ttu-id="1fdc4-146">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1fdc4-146">server</span></span> |<span data-ttu-id="1fdc4-147">Hello PostgreSQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-147">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="1fdc4-148">Igen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-148">Yes</span></span> |
| <span data-ttu-id="1fdc4-149">adatbázis</span><span class="sxs-lookup"><span data-stu-id="1fdc4-149">database</span></span> |<span data-ttu-id="1fdc4-150">Hello PostgreSQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-150">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="1fdc4-151">Igen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-151">Yes</span></span> |
| <span data-ttu-id="1fdc4-152">Séma</span><span class="sxs-lookup"><span data-stu-id="1fdc4-152">schema</span></span> |<span data-ttu-id="1fdc4-153">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-153">Name of hello schema in hello database.</span></span> <span data-ttu-id="1fdc4-154">hello sémanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-154">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="1fdc4-155">Nem</span><span class="sxs-lookup"><span data-stu-id="1fdc4-155">No</span></span> |
| <span data-ttu-id="1fdc4-156">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="1fdc4-156">authenticationType</span></span> |<span data-ttu-id="1fdc4-157">Tooconnect toohello PostgreSQL-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-157">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="1fdc4-158">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="1fdc4-159">Igen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-159">Yes</span></span> |
| <span data-ttu-id="1fdc4-160">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="1fdc4-160">username</span></span> |<span data-ttu-id="1fdc4-161">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="1fdc4-162">Nem</span><span class="sxs-lookup"><span data-stu-id="1fdc4-162">No</span></span> |
| <span data-ttu-id="1fdc4-163">jelszó</span><span class="sxs-lookup"><span data-stu-id="1fdc4-163">password</span></span> |<span data-ttu-id="1fdc4-164">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="1fdc4-165">Nem</span><span class="sxs-lookup"><span data-stu-id="1fdc4-165">No</span></span> |
| <span data-ttu-id="1fdc4-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="1fdc4-166">gatewayName</span></span> |<span data-ttu-id="1fdc4-167">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-167">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="1fdc4-168">Igen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="1fdc4-169">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1fdc4-169">Dataset properties</span></span>
<span data-ttu-id="1fdc4-170">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="1fdc4-171">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="1fdc4-172">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-172">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="1fdc4-173">hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a PostgreSQL dataset) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-173">hello typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has hello following properties:</span></span>

| <span data-ttu-id="1fdc4-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1fdc4-174">Property</span></span> | <span data-ttu-id="1fdc4-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fdc4-175">Description</span></span> | <span data-ttu-id="1fdc4-176">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fdc4-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1fdc4-177">tableName</span><span class="sxs-lookup"><span data-stu-id="1fdc4-177">tableName</span></span> |<span data-ttu-id="1fdc4-178">Hello tábla hello PostgreSQL-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-178">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="1fdc4-179">hello táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-179">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="1fdc4-180">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="1fdc4-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="1fdc4-181">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1fdc4-181">Copy activity properties</span></span>
<span data-ttu-id="1fdc4-182">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-182">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="1fdc4-183">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="1fdc4-184">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-184">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="1fdc4-185">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-185">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="1fdc4-186">Ha a forrás típusa van **RelationalSource** (amely tartalmazza a PostgreSQL), typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-186">When source is of type **RelationalSource** (which includes PostgreSQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="1fdc4-187">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1fdc4-187">Property</span></span> | <span data-ttu-id="1fdc4-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fdc4-188">Description</span></span> | <span data-ttu-id="1fdc4-189">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="1fdc4-189">Allowed values</span></span> | <span data-ttu-id="1fdc4-190">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fdc4-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1fdc4-191">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="1fdc4-191">query</span></span> |<span data-ttu-id="1fdc4-192">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-192">Use hello custom query tooread data.</span></span> |<span data-ttu-id="1fdc4-193">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-193">SQL query string.</span></span> <span data-ttu-id="1fdc4-194">Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="1fdc4-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="1fdc4-195">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="1fdc4-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="1fdc4-196">Séma-és tábla-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="1fdc4-197">Tegye őket `""` (idézőjelek) hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-197">Enclose them in `""` (double quotes) in hello query.</span></span>  

<span data-ttu-id="1fdc4-198">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a><span data-ttu-id="1fdc4-199">JSON-példa: adatok másolása az PostgreSQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="1fdc4-199">JSON example: Copy data from PostgreSQL tooAzure Blob</span></span>
<span data-ttu-id="1fdc4-200">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-200">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="1fdc4-201">Azok megjelenítése, hogyan PostgreSQL toocopy adatait adatbázis tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-201">They show how toocopy data from PostgreSQL database tooAzure Blob Storage.</span></span> <span data-ttu-id="1fdc4-202">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-202">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="1fdc4-203">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="1fdc4-204">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-204">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="1fdc4-205">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="1fdc4-206">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-206">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="1fdc4-207">A társított szolgáltatás típusa [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="1fdc4-208">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="1fdc4-209">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="1fdc4-210">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="1fdc4-211">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-211">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="1fdc4-212">hello minta másol adatokat egy lekérdezés eredményét PostgreSQL adatbázis tooa BLOB minden órában.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-212">hello sample copies data from a query result in PostgreSQL database tooa blob every hour.</span></span> <span data-ttu-id="1fdc4-213">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-213">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="1fdc4-214">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-214">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="1fdc4-215">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-215">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="1fdc4-216">**PostgreSQL társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-216">**PostgreSQL linked service:**</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="1fdc4-217">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-217">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
<span data-ttu-id="1fdc4-218">**PostgreSQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="1fdc4-219">hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" PostgreSQL és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-219">hello sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="1fdc4-220">Beállítás `"external": true` hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-220">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="1fdc4-221">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="1fdc4-222">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="1fdc4-223">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-223">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="1fdc4-224">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="1fdc4-225">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="1fdc4-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="1fdc4-226">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="1fdc4-227">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="1fdc4-228">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság kiválasztja hello adatokat hello public.usstates táblából hello PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-228">hello SQL query specified for hello **query** property selects hello data from hello public.usstates table in hello PostgreSQL database.</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="1fdc4-229">Típus identitástagok esetén PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1fdc4-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="1fdc4-230">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="1fdc4-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="1fdc4-231">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="1fdc4-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="1fdc4-232">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="1fdc4-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="1fdc4-233">Adatok tooPostgreSQL áthelyezésekor hello következő megfeleltetéseket használtak PostgreSQL too.NET típusának.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-233">When moving data tooPostgreSQL, hello following mappings are used from PostgreSQL type too.NET type.</span></span>

| <span data-ttu-id="1fdc4-234">PostgreSQL-adatbázisból típusa</span><span class="sxs-lookup"><span data-stu-id="1fdc4-234">PostgreSQL Database type</span></span> | <span data-ttu-id="1fdc4-235">PostgresSQL aliasok</span><span class="sxs-lookup"><span data-stu-id="1fdc4-235">PostgresSQL aliases</span></span> | <span data-ttu-id="1fdc4-236">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="1fdc4-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1fdc4-237">abstime</span><span class="sxs-lookup"><span data-stu-id="1fdc4-237">abstime</span></span> | |<span data-ttu-id="1fdc4-238">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="1fdc4-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="1fdc4-239">bigint</span><span class="sxs-lookup"><span data-stu-id="1fdc4-239">bigint</span></span> |<span data-ttu-id="1fdc4-240">int8</span><span class="sxs-lookup"><span data-stu-id="1fdc4-240">int8</span></span> |<span data-ttu-id="1fdc4-241">Int64</span><span class="sxs-lookup"><span data-stu-id="1fdc4-241">Int64</span></span> |
| <span data-ttu-id="1fdc4-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="1fdc4-242">bigserial</span></span> |<span data-ttu-id="1fdc4-243">serial8</span><span class="sxs-lookup"><span data-stu-id="1fdc4-243">serial8</span></span> |<span data-ttu-id="1fdc4-244">Int64</span><span class="sxs-lookup"><span data-stu-id="1fdc4-244">Int64</span></span> |
| <span data-ttu-id="1fdc4-245">bit [(n)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-245">bit [ (n) ]</span></span> | |<span data-ttu-id="1fdc4-246">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="1fdc4-247">bit különböző [(n)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="1fdc4-248">varbit</span><span class="sxs-lookup"><span data-stu-id="1fdc4-248">varbit</span></span> |<span data-ttu-id="1fdc4-249">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-249">Byte[], String</span></span> |
| <span data-ttu-id="1fdc4-250">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="1fdc4-250">boolean</span></span> |<span data-ttu-id="1fdc4-251">logikai érték</span><span class="sxs-lookup"><span data-stu-id="1fdc4-251">bool</span></span> |<span data-ttu-id="1fdc4-252">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="1fdc4-252">Boolean</span></span> |
| <span data-ttu-id="1fdc4-253">Mezőbe</span><span class="sxs-lookup"><span data-stu-id="1fdc4-253">box</span></span> | |<span data-ttu-id="1fdc4-254">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-255">bytea</span><span class="sxs-lookup"><span data-stu-id="1fdc4-255">bytea</span></span> | |<span data-ttu-id="1fdc4-256">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-257">[(n)] karakter</span><span class="sxs-lookup"><span data-stu-id="1fdc4-257">character [ (n) ]</span></span> |<span data-ttu-id="1fdc4-258">a char [(n)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-258">char [ (n) ]</span></span> |<span data-ttu-id="1fdc4-259">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-259">String</span></span> |
| <span data-ttu-id="1fdc4-260">[(n)] eltérő karaktert</span><span class="sxs-lookup"><span data-stu-id="1fdc4-260">character varying [ (n) ]</span></span> |<span data-ttu-id="1fdc4-261">varchar [(n)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-261">varchar [ (n) ]</span></span> |<span data-ttu-id="1fdc4-262">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-262">String</span></span> |
| <span data-ttu-id="1fdc4-263">CID</span><span class="sxs-lookup"><span data-stu-id="1fdc4-263">cid</span></span> | |<span data-ttu-id="1fdc4-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-264">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="1fdc4-265">cidr</span></span> | |<span data-ttu-id="1fdc4-266">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-266">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-267">kör</span><span class="sxs-lookup"><span data-stu-id="1fdc4-267">circle</span></span> | |<span data-ttu-id="1fdc4-268">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-269">Dátum</span><span class="sxs-lookup"><span data-stu-id="1fdc4-269">date</span></span> | |<span data-ttu-id="1fdc4-270">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="1fdc4-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="1fdc4-271">daterange</span></span> | |<span data-ttu-id="1fdc4-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-272">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-273">a kétszeres pontosság</span><span class="sxs-lookup"><span data-stu-id="1fdc4-273">double precision</span></span> |<span data-ttu-id="1fdc4-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="1fdc4-274">float8</span></span> |<span data-ttu-id="1fdc4-275">Dupla</span><span class="sxs-lookup"><span data-stu-id="1fdc4-275">Double</span></span> |
| <span data-ttu-id="1fdc4-276">INet</span><span class="sxs-lookup"><span data-stu-id="1fdc4-276">inet</span></span> | |<span data-ttu-id="1fdc4-277">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-278">intarry</span><span class="sxs-lookup"><span data-stu-id="1fdc4-278">intarry</span></span> | |<span data-ttu-id="1fdc4-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-279">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-280">int4range</span><span class="sxs-lookup"><span data-stu-id="1fdc4-280">int4range</span></span> | |<span data-ttu-id="1fdc4-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-281">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-282">int8range</span><span class="sxs-lookup"><span data-stu-id="1fdc4-282">int8range</span></span> | |<span data-ttu-id="1fdc4-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-283">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-284">egész szám</span><span class="sxs-lookup"><span data-stu-id="1fdc4-284">integer</span></span> |<span data-ttu-id="1fdc4-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="1fdc4-285">int, int4</span></span> |<span data-ttu-id="1fdc4-286">Int32</span><span class="sxs-lookup"><span data-stu-id="1fdc4-286">Int32</span></span> |
| <span data-ttu-id="1fdc4-287">időköz [mezők] [(p)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="1fdc4-288">Időtartomány</span><span class="sxs-lookup"><span data-stu-id="1fdc4-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-289">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="1fdc4-289">json</span></span> | |<span data-ttu-id="1fdc4-290">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-290">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="1fdc4-291">jsonb</span></span> | |<span data-ttu-id="1fdc4-292">Byte]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-293">sor</span><span class="sxs-lookup"><span data-stu-id="1fdc4-293">line</span></span> | |<span data-ttu-id="1fdc4-294">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-295">lseg</span><span class="sxs-lookup"><span data-stu-id="1fdc4-295">lseg</span></span> | |<span data-ttu-id="1fdc4-296">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="1fdc4-297">macaddr</span></span> | |<span data-ttu-id="1fdc4-298">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-299">pénz</span><span class="sxs-lookup"><span data-stu-id="1fdc4-299">money</span></span> | |<span data-ttu-id="1fdc4-300">Decimális</span><span class="sxs-lookup"><span data-stu-id="1fdc4-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-301">numerikus [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="1fdc4-302">Decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="1fdc4-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="1fdc4-303">Decimális</span><span class="sxs-lookup"><span data-stu-id="1fdc4-303">Decimal</span></span> |
| <span data-ttu-id="1fdc4-304">numrange</span><span class="sxs-lookup"><span data-stu-id="1fdc4-304">numrange</span></span> | |<span data-ttu-id="1fdc4-305">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-305">String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-306">OID</span><span class="sxs-lookup"><span data-stu-id="1fdc4-306">oid</span></span> | |<span data-ttu-id="1fdc4-307">Int32</span><span class="sxs-lookup"><span data-stu-id="1fdc4-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-308">Elérési út</span><span class="sxs-lookup"><span data-stu-id="1fdc4-308">path</span></span> | |<span data-ttu-id="1fdc4-309">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="1fdc4-310">pg_lsn</span></span> | |<span data-ttu-id="1fdc4-311">Int64</span><span class="sxs-lookup"><span data-stu-id="1fdc4-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-312">pont</span><span class="sxs-lookup"><span data-stu-id="1fdc4-312">point</span></span> | |<span data-ttu-id="1fdc4-313">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-314">Sokszög</span><span class="sxs-lookup"><span data-stu-id="1fdc4-314">polygon</span></span> | |<span data-ttu-id="1fdc4-315">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="1fdc4-316">valós</span><span class="sxs-lookup"><span data-stu-id="1fdc4-316">real</span></span> |<span data-ttu-id="1fdc4-317">float4</span><span class="sxs-lookup"><span data-stu-id="1fdc4-317">float4</span></span> |<span data-ttu-id="1fdc4-318">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="1fdc4-318">Single</span></span> |
| <span data-ttu-id="1fdc4-319">smallint</span><span class="sxs-lookup"><span data-stu-id="1fdc4-319">smallint</span></span> |<span data-ttu-id="1fdc4-320">int2</span><span class="sxs-lookup"><span data-stu-id="1fdc4-320">int2</span></span> |<span data-ttu-id="1fdc4-321">Int16</span><span class="sxs-lookup"><span data-stu-id="1fdc4-321">Int16</span></span> |
| <span data-ttu-id="1fdc4-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="1fdc4-322">smallserial</span></span> |<span data-ttu-id="1fdc4-323">serial2</span><span class="sxs-lookup"><span data-stu-id="1fdc4-323">serial2</span></span> |<span data-ttu-id="1fdc4-324">Int16</span><span class="sxs-lookup"><span data-stu-id="1fdc4-324">Int16</span></span> |
| <span data-ttu-id="1fdc4-325">Soros</span><span class="sxs-lookup"><span data-stu-id="1fdc4-325">serial</span></span> |<span data-ttu-id="1fdc4-326">serial4</span><span class="sxs-lookup"><span data-stu-id="1fdc4-326">serial4</span></span> |<span data-ttu-id="1fdc4-327">Int32</span><span class="sxs-lookup"><span data-stu-id="1fdc4-327">Int32</span></span> |
| <span data-ttu-id="1fdc4-328">Szöveg</span><span class="sxs-lookup"><span data-stu-id="1fdc4-328">text</span></span> | |<span data-ttu-id="1fdc4-329">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fdc4-329">String</span></span> |&nbsp;

## <a name="map-source-toosink-columns"></a><span data-ttu-id="1fdc4-330">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="1fdc4-330">Map source toosink columns</span></span>
<span data-ttu-id="1fdc4-331">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-331">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="1fdc4-332">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="1fdc4-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="1fdc4-333">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-333">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="1fdc4-334">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="1fdc4-335">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="1fdc4-336">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-336">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="1fdc4-337">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="1fdc4-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="1fdc4-338">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="1fdc4-338">Performance and Tuning</span></span>
<span data-ttu-id="1fdc4-339">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="1fdc4-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
