---
title: "Azure Data Factory használatával Sybase aaaMove adatait |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával Sybase-adatbázisból."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: b379ee10-0ff5-4974-8c87-c95f82f1c5c6
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ad003ec502028d56db9570fe08af329eb5b71817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="e6c89-103">Adatok áthelyezése az Azure Data Factory használatával Sybase</span><span class="sxs-lookup"><span data-stu-id="e6c89-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="e6c89-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni Sybase-adatbázishoz a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e6c89-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Sybase database.</span></span> <span data-ttu-id="e6c89-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="e6c89-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="e6c89-106">Egy helyszíni Sybase adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="e6c89-106">You can copy data from an on-premises Sybase data store tooany supported sink data store.</span></span> <span data-ttu-id="e6c89-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="e6c89-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="e6c89-108">Adat-előállító jelenleg csak egy Sybase-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooa Sybase adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="e6c89-108">Data factory currently supports only moving data from a Sybase data store tooother data stores, but not for moving data from other data stores tooa Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e6c89-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6c89-109">Prerequisites</span></span>
<span data-ttu-id="e6c89-110">Data Factory szolgáltatásnak csatlakozó tooon helyszíni Sybase adatforrások az adatkezelési átjáró hello segítségével támogatja.</span><span class="sxs-lookup"><span data-stu-id="e6c89-110">Data Factory service supports connecting tooon-premises Sybase sources using hello Data Management Gateway.</span></span> <span data-ttu-id="e6c89-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="e6c89-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="e6c89-112">Átjáróra szükség, akkor is, ha egy Azure IaaS virtuális gépen található hello Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e6c89-112">Gateway is required even if hello Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="e6c89-113">Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="e6c89-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="e6c89-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="e6c89-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="e6c89-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="e6c89-115">Supported versions and installation</span></span>
<span data-ttu-id="e6c89-116">Az adatkezelési átjáró tooconnect toohello Sybase-adatbázishoz, a tooinstall hello kell [Sybase iAnywhere.Data.SQLAnywhere rendszerhez készült adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=324846) 16 vagy fent a hello ugyanaz az adatkezelési átjáró hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="e6c89-116">For Data Management Gateway tooconnect toohello Sybase Database, you need tooinstall hello [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="e6c89-117">Sybase verzió 16 és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="e6c89-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e6c89-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e6c89-118">Getting started</span></span>
<span data-ttu-id="e6c89-119">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="e6c89-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="e6c89-120">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="e6c89-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="e6c89-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="e6c89-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="e6c89-122">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="e6c89-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e6c89-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="e6c89-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e6c89-124">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="e6c89-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="e6c89-125">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="e6c89-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="e6c89-126">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="e6c89-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="e6c89-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="e6c89-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="e6c89-128">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="e6c89-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="e6c89-129">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="e6c89-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="e6c89-130">Az adat-előállító entitások, amelyek egy helyszíni Sybase adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="e6c89-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="e6c89-131">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa Sybase adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="e6c89-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="e6c89-132">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="e6c89-132">Linked service properties</span></span>
<span data-ttu-id="e6c89-133">a következő táblázat hello biztosít JSON elemek adott tooSybase kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="e6c89-133">hello following table provides description for JSON elements specific tooSybase linked service.</span></span>

| <span data-ttu-id="e6c89-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e6c89-134">Property</span></span> | <span data-ttu-id="e6c89-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="e6c89-135">Description</span></span> | <span data-ttu-id="e6c89-136">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e6c89-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e6c89-137">type</span><span class="sxs-lookup"><span data-stu-id="e6c89-137">type</span></span> |<span data-ttu-id="e6c89-138">hello type tulajdonságot kell beállítani: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="e6c89-138">hello type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="e6c89-139">Igen</span><span class="sxs-lookup"><span data-stu-id="e6c89-139">Yes</span></span> |
| <span data-ttu-id="e6c89-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="e6c89-140">server</span></span> |<span data-ttu-id="e6c89-141">Hello Sybase-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="e6c89-141">Name of hello Sybase server.</span></span> |<span data-ttu-id="e6c89-142">Igen</span><span class="sxs-lookup"><span data-stu-id="e6c89-142">Yes</span></span> |
| <span data-ttu-id="e6c89-143">adatbázis</span><span class="sxs-lookup"><span data-stu-id="e6c89-143">database</span></span> |<span data-ttu-id="e6c89-144">Hello Sybase-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="e6c89-144">Name of hello Sybase database.</span></span> |<span data-ttu-id="e6c89-145">Igen</span><span class="sxs-lookup"><span data-stu-id="e6c89-145">Yes</span></span> |
| <span data-ttu-id="e6c89-146">Séma</span><span class="sxs-lookup"><span data-stu-id="e6c89-146">schema</span></span> |<span data-ttu-id="e6c89-147">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="e6c89-147">Name of hello schema in hello database.</span></span> |<span data-ttu-id="e6c89-148">Nem</span><span class="sxs-lookup"><span data-stu-id="e6c89-148">No</span></span> |
| <span data-ttu-id="e6c89-149">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="e6c89-149">authenticationType</span></span> |<span data-ttu-id="e6c89-150">Tooconnect toohello Sybase-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e6c89-150">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="e6c89-151">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="e6c89-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="e6c89-152">Igen</span><span class="sxs-lookup"><span data-stu-id="e6c89-152">Yes</span></span> |
| <span data-ttu-id="e6c89-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="e6c89-153">username</span></span> |<span data-ttu-id="e6c89-154">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="e6c89-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="e6c89-155">Nem</span><span class="sxs-lookup"><span data-stu-id="e6c89-155">No</span></span> |
| <span data-ttu-id="e6c89-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="e6c89-156">password</span></span> |<span data-ttu-id="e6c89-157">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="e6c89-157">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="e6c89-158">Nem</span><span class="sxs-lookup"><span data-stu-id="e6c89-158">No</span></span> |
| <span data-ttu-id="e6c89-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="e6c89-159">gatewayName</span></span> |<span data-ttu-id="e6c89-160">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e6c89-160">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="e6c89-161">Igen</span><span class="sxs-lookup"><span data-stu-id="e6c89-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="e6c89-162">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e6c89-162">Dataset properties</span></span>
<span data-ttu-id="e6c89-163">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e6c89-163">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e6c89-164">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="e6c89-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e6c89-165">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e6c89-165">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="e6c89-166">Hello **typeProperties** típusú adatkészlet szakasz **RelationalTable** következő tulajdonságai hello (amely tartalmazza a Sybase dataset) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e6c89-166">hello **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has hello following properties:</span></span>

| <span data-ttu-id="e6c89-167">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e6c89-167">Property</span></span> | <span data-ttu-id="e6c89-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="e6c89-168">Description</span></span> | <span data-ttu-id="e6c89-169">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e6c89-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e6c89-170">tableName</span><span class="sxs-lookup"><span data-stu-id="e6c89-170">tableName</span></span> |<span data-ttu-id="e6c89-171">Hello tábla hello Sybase-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e6c89-171">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="e6c89-172">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="e6c89-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e6c89-173">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e6c89-173">Copy activity properties</span></span>
<span data-ttu-id="e6c89-174">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e6c89-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e6c89-175">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="e6c89-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="e6c89-176">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="e6c89-176">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="e6c89-177">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="e6c89-177">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="e6c89-178">Ha hello forrás típusa nem **RelationalSource** (amely tartalmazza a Sybase), a következő tulajdonságok hello érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="e6c89-178">When hello source is of type **RelationalSource** (which includes Sybase), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="e6c89-179">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e6c89-179">Property</span></span> | <span data-ttu-id="e6c89-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="e6c89-180">Description</span></span> | <span data-ttu-id="e6c89-181">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="e6c89-181">Allowed values</span></span> | <span data-ttu-id="e6c89-182">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e6c89-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e6c89-183">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e6c89-183">query</span></span> |<span data-ttu-id="e6c89-184">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="e6c89-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="e6c89-185">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e6c89-185">SQL query string.</span></span> <span data-ttu-id="e6c89-186">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="e6c89-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="e6c89-187">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="e6c89-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-tooazure-blob"></a><span data-ttu-id="e6c89-188">JSON-példa: adatok másolása az Sybase tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="e6c89-188">JSON example: Copy data from Sybase tooAzure Blob</span></span>
<span data-ttu-id="e6c89-189">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e6c89-189">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="e6c89-190">Azok megjelenítése, hogyan Sybase toocopy adatait adatbázis tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e6c89-190">They show how toocopy data from Sybase database tooAzure Blob Storage.</span></span> <span data-ttu-id="e6c89-191">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="e6c89-191">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="e6c89-192">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e6c89-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="e6c89-193">A társított szolgáltatás típusa [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e6c89-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="e6c89-194">A következő típusú liked szolgáltatást [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e6c89-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e6c89-195">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e6c89-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="e6c89-196">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e6c89-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e6c89-197">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e6c89-197">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="e6c89-198">hello minta másol adatokat egy lekérdezés eredményét Sybase adatbázis tooa BLOB minden órában.</span><span class="sxs-lookup"><span data-stu-id="e6c89-198">hello sample copies data from a query result in Sybase database tooa blob every hour.</span></span> <span data-ttu-id="e6c89-199">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="e6c89-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="e6c89-200">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="e6c89-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="e6c89-201">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e6c89-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="e6c89-202">**Sybase társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="e6c89-202">**Sybase linked service:**</span></span>

```JSON
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
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

<span data-ttu-id="e6c89-203">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="e6c89-203">**Azure Blob storage linked service:**</span></span>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

<span data-ttu-id="e6c89-204">**Sybase bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e6c89-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="e6c89-205">hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Sybase és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e6c89-205">hello sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="e6c89-206">"External" beállítása: igaz arról értesíti az, hogy ez az adatkészlet külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="e6c89-206">Setting “external”: true informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="e6c89-207">Figyelje meg, hogy hello **típus** hello szolgáltatásnak van állítva: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="e6c89-207">Notice that hello **type** of hello linked service is set to: **RelationalTable**.</span></span>

```JSON
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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

<span data-ttu-id="e6c89-208">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e6c89-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="e6c89-209">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e6c89-209">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e6c89-210">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="e6c89-210">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="e6c89-211">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="e6c89-211">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobSybaseDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="e6c89-212">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="e6c89-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="e6c89-213">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="e6c89-213">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="e6c89-214">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e6c89-214">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="e6c89-215">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság hello DBA hello adatok választja ki. Hello adatbázis rendelések táblájában.</span><span class="sxs-lookup"><span data-stu-id="e6c89-215">hello SQL query specified for hello **query** property selects hello data from hello DBA.Orders table in hello database.</span></span>

```JSON
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from DBA.Orders"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "SybaseDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobSybaseDataSet"
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
                "name": "SybaseToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="e6c89-216">Típusleképezés a Sybase rendszerhez</span><span class="sxs-lookup"><span data-stu-id="e6c89-216">Type mapping for Sybase</span></span>
<span data-ttu-id="e6c89-217">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="e6c89-217">As mentioned in hello [Data Movement Activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="e6c89-218">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="e6c89-218">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="e6c89-219">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="e6c89-219">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="e6c89-220">Sybase T-SQL és a T-SQL-típusokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="e6c89-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="e6c89-221">Az sql-típusok too.NET típusból hozzárendelési táblázatot [Azure SQL-összekötő](data-factory-azure-sql-connector.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e6c89-221">For a mapping table from sql types too.NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="e6c89-222">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="e6c89-222">Map source toosink columns</span></span>
<span data-ttu-id="e6c89-223">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e6c89-223">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e6c89-224">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="e6c89-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="e6c89-225">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="e6c89-225">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="e6c89-226">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="e6c89-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e6c89-227">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="e6c89-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e6c89-228">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="e6c89-228">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e6c89-229">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="e6c89-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e6c89-230">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="e6c89-230">Performance and Tuning</span></span>
<span data-ttu-id="e6c89-231">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="e6c89-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
