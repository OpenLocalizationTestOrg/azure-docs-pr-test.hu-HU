---
title: "Azure Data Factory használatával Teradata aaaMove adatait |} Microsoft Docs"
description: "A Data Factory szolgáltatásnak, amely lehetővé teszi az adatok áthelyezése Teradata-adatbázishoz hello Teradata összekötő megismerése"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 98eb76d8-5f3d-4667-b76e-e59ed3eea3ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: 79153476157666463b499edaa7585adaf8ad3bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a><span data-ttu-id="fec0f-103">Adatok áthelyezése az Azure Data Factory használatával teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="fec0f-103">Move data from Teradata using Azure Data Factory</span></span>
<span data-ttu-id="fec0f-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni Teradata-adatbázishoz a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fec0f-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Teradata database.</span></span> <span data-ttu-id="fec0f-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="fec0f-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="fec0f-106">Egy helyszíni Teradata adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="fec0f-106">You can copy data from an on-premises Teradata data store tooany supported sink data store.</span></span> <span data-ttu-id="fec0f-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="fec0f-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="fec0f-108">Adat-előállító jelenleg csak egy Teradata-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooa Teradata adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="fec0f-108">Data factory currently supports only moving data from a Teradata data store tooother data stores, but not for moving data from other data stores tooa Teradata data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fec0f-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fec0f-109">Prerequisites</span></span>
<span data-ttu-id="fec0f-110">Adat-előállító csatlakozó tooon helyszíni Teradata adatforrások az adatkezelési átjáró hello keresztül támogatja.</span><span class="sxs-lookup"><span data-stu-id="fec0f-110">Data factory supports connecting tooon-premises Teradata sources via hello Data Management Gateway.</span></span> <span data-ttu-id="fec0f-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="fec0f-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="fec0f-112">Átjáróra szükség, akkor is, ha hello Teradata van tárolva az Azure infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fec0f-112">Gateway is required even if hello Teradata is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="fec0f-113">Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="fec0f-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="fec0f-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="fec0f-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="fec0f-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="fec0f-115">Supported versions and installation</span></span>
<span data-ttu-id="fec0f-116">Az adatkezelési átjáró tooconnect toohello Teradata-adatbázishoz, a tooinstall hello kell [.NET-adatszolgáltató a teradata rendszerhez](http://go.microsoft.com/fwlink/?LinkId=278886) verzió 14 vagy fent a hello ugyanaz az adatkezelési átjáró hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="fec0f-116">For Data Management Gateway tooconnect toohello Teradata Database, you need tooinstall hello [.NET Data Provider for Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="fec0f-117">Teradata 12-es és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="fec0f-117">Teradata version 12 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="fec0f-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="fec0f-118">Getting started</span></span>
<span data-ttu-id="fec0f-119">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="fec0f-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="fec0f-120">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="fec0f-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="fec0f-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="fec0f-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="fec0f-122">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="fec0f-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="fec0f-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="fec0f-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="fec0f-124">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="fec0f-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="fec0f-125">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="fec0f-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="fec0f-126">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="fec0f-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="fec0f-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="fec0f-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="fec0f-128">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="fec0f-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="fec0f-129">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="fec0f-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="fec0f-130">Az adat-előállító entitások, amelyek egy a helyszíni Teradata adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="fec0f-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Teradata data store, see [JSON example: Copy data from Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="fec0f-131">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa Teradata adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="fec0f-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Teradata data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="fec0f-132">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="fec0f-132">Linked service properties</span></span>
<span data-ttu-id="fec0f-133">a következő táblázat hello biztosít JSON elemek adott tooTeradata kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="fec0f-133">hello following table provides description for JSON elements specific tooTeradata linked service.</span></span>

| <span data-ttu-id="fec0f-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fec0f-134">Property</span></span> | <span data-ttu-id="fec0f-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="fec0f-135">Description</span></span> | <span data-ttu-id="fec0f-136">Szükséges</span><span class="sxs-lookup"><span data-stu-id="fec0f-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fec0f-137">type</span><span class="sxs-lookup"><span data-stu-id="fec0f-137">type</span></span> |<span data-ttu-id="fec0f-138">hello type tulajdonságot kell beállítani: **OnPremisesTeradata**</span><span class="sxs-lookup"><span data-stu-id="fec0f-138">hello type property must be set to: **OnPremisesTeradata**</span></span> |<span data-ttu-id="fec0f-139">Igen</span><span class="sxs-lookup"><span data-stu-id="fec0f-139">Yes</span></span> |
| <span data-ttu-id="fec0f-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="fec0f-140">server</span></span> |<span data-ttu-id="fec0f-141">Hello Teradata-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="fec0f-141">Name of hello Teradata server.</span></span> |<span data-ttu-id="fec0f-142">Igen</span><span class="sxs-lookup"><span data-stu-id="fec0f-142">Yes</span></span> |
| <span data-ttu-id="fec0f-143">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="fec0f-143">authenticationType</span></span> |<span data-ttu-id="fec0f-144">Tooconnect toohello Teradata-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="fec0f-144">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="fec0f-145">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="fec0f-145">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="fec0f-146">Igen</span><span class="sxs-lookup"><span data-stu-id="fec0f-146">Yes</span></span> |
| <span data-ttu-id="fec0f-147">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="fec0f-147">username</span></span> |<span data-ttu-id="fec0f-148">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="fec0f-148">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="fec0f-149">Nem</span><span class="sxs-lookup"><span data-stu-id="fec0f-149">No</span></span> |
| <span data-ttu-id="fec0f-150">jelszó</span><span class="sxs-lookup"><span data-stu-id="fec0f-150">password</span></span> |<span data-ttu-id="fec0f-151">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="fec0f-151">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="fec0f-152">Nem</span><span class="sxs-lookup"><span data-stu-id="fec0f-152">No</span></span> |
| <span data-ttu-id="fec0f-153">gatewayName</span><span class="sxs-lookup"><span data-stu-id="fec0f-153">gatewayName</span></span> |<span data-ttu-id="fec0f-154">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni Teradata-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="fec0f-154">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="fec0f-155">Igen</span><span class="sxs-lookup"><span data-stu-id="fec0f-155">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="fec0f-156">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="fec0f-156">Dataset properties</span></span>
<span data-ttu-id="fec0f-157">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="fec0f-157">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="fec0f-158">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="fec0f-158">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="fec0f-159">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fec0f-159">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="fec0f-160">Jelenleg nem támogatott hello Teradata adatkészlet tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="fec0f-160">Currently, there are no type properties supported for hello Teradata dataset.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="fec0f-161">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="fec0f-161">Copy activity properties</span></span>
<span data-ttu-id="fec0f-162">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="fec0f-162">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="fec0f-163">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="fec0f-163">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="fec0f-164">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="fec0f-164">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="fec0f-165">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="fec0f-165">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="fec0f-166">Ha hello forrás típusa nem **RelationalSource** (amely tartalmazza a teradata rendszerhez), a következő tulajdonságok hello érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="fec0f-166">When hello source is of type **RelationalSource** (which includes Teradata), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="fec0f-167">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fec0f-167">Property</span></span> | <span data-ttu-id="fec0f-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="fec0f-168">Description</span></span> | <span data-ttu-id="fec0f-169">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="fec0f-169">Allowed values</span></span> | <span data-ttu-id="fec0f-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="fec0f-170">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fec0f-171">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="fec0f-171">query</span></span> |<span data-ttu-id="fec0f-172">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="fec0f-172">Use hello custom query tooread data.</span></span> |<span data-ttu-id="fec0f-173">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="fec0f-173">SQL query string.</span></span> <span data-ttu-id="fec0f-174">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="fec0f-174">For example: select * from MyTable.</span></span> |<span data-ttu-id="fec0f-175">Igen</span><span class="sxs-lookup"><span data-stu-id="fec0f-175">Yes</span></span> |

### <a name="json-example-copy-data-from-teradata-tooazure-blob"></a><span data-ttu-id="fec0f-176">JSON-példa: adatok másolása az Teradata tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="fec0f-176">JSON example: Copy data from Teradata tooAzure Blob</span></span>
<span data-ttu-id="fec0f-177">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fec0f-177">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="fec0f-178">Azok hogyan Teradata tooAzure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="fec0f-178">They show how toocopy data from Teradata tooAzure Blob Storage.</span></span> <span data-ttu-id="fec0f-179">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="fec0f-179">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="fec0f-180">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="fec0f-180">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="fec0f-181">A társított szolgáltatás típusa [OnPremisesTeradata](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="fec0f-181">A linked service of type [OnPremisesTeradata](#linked-service-properties).</span></span>
2. <span data-ttu-id="fec0f-182">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="fec0f-182">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="fec0f-183">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="fec0f-183">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="fec0f-184">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="fec0f-184">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="fec0f-185">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="fec0f-185">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="fec0f-186">hello minta másol adatokat egy lekérdezés eredményét Teradata adatbázis tooa BLOB minden órában.</span><span class="sxs-lookup"><span data-stu-id="fec0f-186">hello sample copies data from a query result in Teradata database tooa blob every hour.</span></span> <span data-ttu-id="fec0f-187">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="fec0f-187">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="fec0f-188">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="fec0f-188">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="fec0f-189">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="fec0f-189">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="fec0f-190">**Teradata társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="fec0f-190">**Teradata linked service:**</span></span>

```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="fec0f-191">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="fec0f-191">**Azure Blob storage linked service:**</span></span>

```json
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

<span data-ttu-id="fec0f-192">**Teradata bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="fec0f-192">**Teradata input dataset:**</span></span>

<span data-ttu-id="fec0f-193">hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Teradata és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fec0f-193">hello sample assumes you have created a table “MyTable” in Teradata and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="fec0f-194">"External" beállítása: igaz arról értesíti az hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="fec0f-194">Setting “external”: true informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "TeradataDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {
        },
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

<span data-ttu-id="fec0f-195">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="fec0f-195">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="fec0f-196">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="fec0f-196">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="fec0f-197">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="fec0f-197">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="fec0f-198">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="fec0f-198">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobTeradataDataSet",
    "properties": {
        "published": false,
        "location": {
            "type": "AzureBlobLocation",
            "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "linkedServiceName": "AzureStorageLinkedService"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="fec0f-199">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="fec0f-199">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="fec0f-200">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="fec0f-200">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="fec0f-201">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="fec0f-201">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="fec0f-202">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="fec0f-202">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "TeradataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobTeradataDataSet"
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
                "name": "TeradataToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z",
        "isPaused": false
    }
}
```
## <a name="type-mapping-for-teradata"></a><span data-ttu-id="fec0f-203">Típusleképezés a teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="fec0f-203">Type mapping for Teradata</span></span>
<span data-ttu-id="fec0f-204">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="fec0f-204">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="fec0f-205">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="fec0f-205">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="fec0f-206">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="fec0f-206">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="fec0f-207">Adatok tooTeradata áthelyezésekor hello következő megfeleltetéseket használtak Teradata too.NET típusának.</span><span class="sxs-lookup"><span data-stu-id="fec0f-207">When moving data tooTeradata, hello following mappings are used from Teradata type too.NET type.</span></span>

| <span data-ttu-id="fec0f-208">Teradata-adatbázishoz típusa</span><span class="sxs-lookup"><span data-stu-id="fec0f-208">Teradata Database type</span></span> | <span data-ttu-id="fec0f-209">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="fec0f-209">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="fec0f-210">Karakter</span><span class="sxs-lookup"><span data-stu-id="fec0f-210">Char</span></span> |<span data-ttu-id="fec0f-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-211">String</span></span> |
| <span data-ttu-id="fec0f-212">CLOB</span><span class="sxs-lookup"><span data-stu-id="fec0f-212">Clob</span></span> |<span data-ttu-id="fec0f-213">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-213">String</span></span> |
| <span data-ttu-id="fec0f-214">Kép</span><span class="sxs-lookup"><span data-stu-id="fec0f-214">Graphic</span></span> |<span data-ttu-id="fec0f-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-215">String</span></span> |
| <span data-ttu-id="fec0f-216">VarChar</span><span class="sxs-lookup"><span data-stu-id="fec0f-216">VarChar</span></span> |<span data-ttu-id="fec0f-217">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-217">String</span></span> |
| <span data-ttu-id="fec0f-218">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="fec0f-218">VarGraphic</span></span> |<span data-ttu-id="fec0f-219">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-219">String</span></span> |
| <span data-ttu-id="fec0f-220">Blob</span><span class="sxs-lookup"><span data-stu-id="fec0f-220">Blob</span></span> |<span data-ttu-id="fec0f-221">Byte]</span><span class="sxs-lookup"><span data-stu-id="fec0f-221">Byte[]</span></span> |
| <span data-ttu-id="fec0f-222">Bájt</span><span class="sxs-lookup"><span data-stu-id="fec0f-222">Byte</span></span> |<span data-ttu-id="fec0f-223">Byte]</span><span class="sxs-lookup"><span data-stu-id="fec0f-223">Byte[]</span></span> |
| <span data-ttu-id="fec0f-224">VarByte</span><span class="sxs-lookup"><span data-stu-id="fec0f-224">VarByte</span></span> |<span data-ttu-id="fec0f-225">Byte]</span><span class="sxs-lookup"><span data-stu-id="fec0f-225">Byte[]</span></span> |
| <span data-ttu-id="fec0f-226">BigInt</span><span class="sxs-lookup"><span data-stu-id="fec0f-226">BigInt</span></span> |<span data-ttu-id="fec0f-227">Int64</span><span class="sxs-lookup"><span data-stu-id="fec0f-227">Int64</span></span> |
| <span data-ttu-id="fec0f-228">ByteInt</span><span class="sxs-lookup"><span data-stu-id="fec0f-228">ByteInt</span></span> |<span data-ttu-id="fec0f-229">Int16</span><span class="sxs-lookup"><span data-stu-id="fec0f-229">Int16</span></span> |
| <span data-ttu-id="fec0f-230">Decimális</span><span class="sxs-lookup"><span data-stu-id="fec0f-230">Decimal</span></span> |<span data-ttu-id="fec0f-231">Decimális</span><span class="sxs-lookup"><span data-stu-id="fec0f-231">Decimal</span></span> |
| <span data-ttu-id="fec0f-232">Dupla</span><span class="sxs-lookup"><span data-stu-id="fec0f-232">Double</span></span> |<span data-ttu-id="fec0f-233">Dupla</span><span class="sxs-lookup"><span data-stu-id="fec0f-233">Double</span></span> |
| <span data-ttu-id="fec0f-234">Egész szám</span><span class="sxs-lookup"><span data-stu-id="fec0f-234">Integer</span></span> |<span data-ttu-id="fec0f-235">Int32</span><span class="sxs-lookup"><span data-stu-id="fec0f-235">Int32</span></span> |
| <span data-ttu-id="fec0f-236">Szám</span><span class="sxs-lookup"><span data-stu-id="fec0f-236">Number</span></span> |<span data-ttu-id="fec0f-237">Dupla</span><span class="sxs-lookup"><span data-stu-id="fec0f-237">Double</span></span> |
| <span data-ttu-id="fec0f-238">SmallInt</span><span class="sxs-lookup"><span data-stu-id="fec0f-238">SmallInt</span></span> |<span data-ttu-id="fec0f-239">Int16</span><span class="sxs-lookup"><span data-stu-id="fec0f-239">Int16</span></span> |
| <span data-ttu-id="fec0f-240">Dátum</span><span class="sxs-lookup"><span data-stu-id="fec0f-240">Date</span></span> |<span data-ttu-id="fec0f-241">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="fec0f-241">DateTime</span></span> |
| <span data-ttu-id="fec0f-242">Time</span><span class="sxs-lookup"><span data-stu-id="fec0f-242">Time</span></span> |<span data-ttu-id="fec0f-243">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-243">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-244">Időzóna idő</span><span class="sxs-lookup"><span data-stu-id="fec0f-244">Time With Time Zone</span></span> |<span data-ttu-id="fec0f-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-245">String</span></span> |
| <span data-ttu-id="fec0f-246">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="fec0f-246">Timestamp</span></span> |<span data-ttu-id="fec0f-247">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="fec0f-247">DateTime</span></span> |
| <span data-ttu-id="fec0f-248">Az időzóna időbélyeg</span><span class="sxs-lookup"><span data-stu-id="fec0f-248">Timestamp With Time Zone</span></span> |<span data-ttu-id="fec0f-249">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="fec0f-249">DateTimeOffset</span></span> |
| <span data-ttu-id="fec0f-250">Időköz nap</span><span class="sxs-lookup"><span data-stu-id="fec0f-250">Interval Day</span></span> |<span data-ttu-id="fec0f-251">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-251">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-252">Időköz nap tooHour</span><span class="sxs-lookup"><span data-stu-id="fec0f-252">Interval Day tooHour</span></span> |<span data-ttu-id="fec0f-253">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-253">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-254">Időköz nap tooMinute</span><span class="sxs-lookup"><span data-stu-id="fec0f-254">Interval Day tooMinute</span></span> |<span data-ttu-id="fec0f-255">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-255">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-256">Időköz nap tooSecond</span><span class="sxs-lookup"><span data-stu-id="fec0f-256">Interval Day tooSecond</span></span> |<span data-ttu-id="fec0f-257">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-257">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-258">Időköz óra</span><span class="sxs-lookup"><span data-stu-id="fec0f-258">Interval Hour</span></span> |<span data-ttu-id="fec0f-259">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-259">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-260">Időköz óra tooMinute</span><span class="sxs-lookup"><span data-stu-id="fec0f-260">Interval Hour tooMinute</span></span> |<span data-ttu-id="fec0f-261">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-261">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-262">Időköz óra tooSecond</span><span class="sxs-lookup"><span data-stu-id="fec0f-262">Interval Hour tooSecond</span></span> |<span data-ttu-id="fec0f-263">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-263">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-264">Időköz percben</span><span class="sxs-lookup"><span data-stu-id="fec0f-264">Interval Minute</span></span> |<span data-ttu-id="fec0f-265">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-265">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-266">Időköz percben tooSecond</span><span class="sxs-lookup"><span data-stu-id="fec0f-266">Interval Minute tooSecond</span></span> |<span data-ttu-id="fec0f-267">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-267">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-268">Időköz második</span><span class="sxs-lookup"><span data-stu-id="fec0f-268">Interval Second</span></span> |<span data-ttu-id="fec0f-269">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fec0f-269">TimeSpan</span></span> |
| <span data-ttu-id="fec0f-270">Időköz év</span><span class="sxs-lookup"><span data-stu-id="fec0f-270">Interval Year</span></span> |<span data-ttu-id="fec0f-271">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-271">String</span></span> |
| <span data-ttu-id="fec0f-272">Időköz év tooMonth</span><span class="sxs-lookup"><span data-stu-id="fec0f-272">Interval Year tooMonth</span></span> |<span data-ttu-id="fec0f-273">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-273">String</span></span> |
| <span data-ttu-id="fec0f-274">Időköz hónap</span><span class="sxs-lookup"><span data-stu-id="fec0f-274">Interval Month</span></span> |<span data-ttu-id="fec0f-275">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-275">String</span></span> |
| <span data-ttu-id="fec0f-276">Period(date)</span><span class="sxs-lookup"><span data-stu-id="fec0f-276">Period(Date)</span></span> |<span data-ttu-id="fec0f-277">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-277">String</span></span> |
| <span data-ttu-id="fec0f-278">Period(Time)</span><span class="sxs-lookup"><span data-stu-id="fec0f-278">Period(Time)</span></span> |<span data-ttu-id="fec0f-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-279">String</span></span> |
| <span data-ttu-id="fec0f-280">Időtartam (idő időzóna)</span><span class="sxs-lookup"><span data-stu-id="fec0f-280">Period(Time With Time Zone)</span></span> |<span data-ttu-id="fec0f-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-281">String</span></span> |
| <span data-ttu-id="fec0f-282">Period(Timestamp)</span><span class="sxs-lookup"><span data-stu-id="fec0f-282">Period(Timestamp)</span></span> |<span data-ttu-id="fec0f-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-283">String</span></span> |
| <span data-ttu-id="fec0f-284">Időtartam (időbélyegzője az időzóna)</span><span class="sxs-lookup"><span data-stu-id="fec0f-284">Period(Timestamp With Time Zone)</span></span> |<span data-ttu-id="fec0f-285">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-285">String</span></span> |
| <span data-ttu-id="fec0f-286">XML</span><span class="sxs-lookup"><span data-stu-id="fec0f-286">Xml</span></span> |<span data-ttu-id="fec0f-287">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fec0f-287">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="fec0f-288">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="fec0f-288">Map source toosink columns</span></span>
<span data-ttu-id="fec0f-289">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="fec0f-289">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="fec0f-290">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="fec0f-290">Repeatable read from relational sources</span></span>
<span data-ttu-id="fec0f-291">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="fec0f-291">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="fec0f-292">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="fec0f-292">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="fec0f-293">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="fec0f-293">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="fec0f-294">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="fec0f-294">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="fec0f-295">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="fec0f-295">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="fec0f-296">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="fec0f-296">Performance and Tuning</span></span>
<span data-ttu-id="fec0f-297">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="fec0f-297">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
