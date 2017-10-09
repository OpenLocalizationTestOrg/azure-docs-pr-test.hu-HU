---
title: "Azure Data Factory használatával MySQL aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat a MySQL-adatbázis használati ideje Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.openlocfilehash: 3ffe969e42ce1a54b265c4739df43fdc594ea891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="f3034-103">Helyezze át az adatokat a MySQL Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f3034-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="f3034-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat a helyszíni MySQL-adatbázis a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f3034-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MySQL database.</span></span> <span data-ttu-id="f3034-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="f3034-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="f3034-106">Egy helyszíni MySQL adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="f3034-106">You can copy data from an on-premises MySQL data store tooany supported sink data store.</span></span> <span data-ttu-id="f3034-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="f3034-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="f3034-108">Adat-előállító jelenleg csak egy MySQL adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooan MySQL adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="f3034-108">Data factory currently supports only moving data from a MySQL data store tooother data stores, but not for moving data from other data stores tooan MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f3034-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f3034-109">Prerequisites</span></span>
<span data-ttu-id="f3034-110">Data Factory szolgáltatásnak csatlakozó tooon helyszíni MySQL adatforrások az adatkezelési átjáró hello segítségével támogatja.</span><span class="sxs-lookup"><span data-stu-id="f3034-110">Data Factory service supports connecting tooon-premises MySQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="f3034-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f3034-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="f3034-112">Átjáróra szükség, akkor is, ha hello MySQL adatbázis egy Azure IaaS virtuális gépre (VM).</span><span class="sxs-lookup"><span data-stu-id="f3034-112">Gateway is required even if hello MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="f3034-113">Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="f3034-113">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="f3034-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="f3034-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="f3034-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="f3034-115">Supported versions and installation</span></span>
<span data-ttu-id="f3034-116">Az adatkezelési átjáró tooconnect toohello MySQL-adatbázis, a tooinstall hello kell [MySQL összekötő/Net számára a Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (6.6.5 verzió vagy újabb) a hello ugyanaz az adatkezelési átjáró hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="f3034-116">For Data Management Gateway tooconnect toohello MySQL Database, you need tooinstall hello [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="f3034-117">5.1-es verzió MySQL és az újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f3034-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="f3034-118">Ha kattintson a hiba "Hitelesítés sikertelen, mert hello távoli fél bezárta hello transport stream.", akkor fontolja meg a tooupgrade hello MySQL összekötő/Net toohigher verziója.</span><span class="sxs-lookup"><span data-stu-id="f3034-118">If you hit error on "Authentication failed because hello remote party has closed hello transport stream.", consider tooupgrade hello MySQL Connector/Net toohigher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f3034-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f3034-119">Getting started</span></span>
<span data-ttu-id="f3034-120">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f3034-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="f3034-121">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="f3034-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="f3034-122">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f3034-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="f3034-123">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="f3034-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f3034-124">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="f3034-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f3034-125">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="f3034-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="f3034-126">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f3034-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="f3034-127">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="f3034-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="f3034-128">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="f3034-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f3034-129">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="f3034-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="f3034-130">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="f3034-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="f3034-131">Az adat-előállító entitások, amelyek egy helyszíni MySQL adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f3034-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="f3034-132">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa MySQL adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="f3034-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="f3034-133">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="f3034-133">Linked service properties</span></span>
<span data-ttu-id="f3034-134">a következő táblázat hello biztosít JSON elemek adott tooMySQL kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="f3034-134">hello following table provides description for JSON elements specific tooMySQL linked service.</span></span>

| <span data-ttu-id="f3034-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f3034-135">Property</span></span> | <span data-ttu-id="f3034-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="f3034-136">Description</span></span> | <span data-ttu-id="f3034-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f3034-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3034-138">type</span><span class="sxs-lookup"><span data-stu-id="f3034-138">type</span></span> |<span data-ttu-id="f3034-139">hello type tulajdonságot kell beállítani: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="f3034-139">hello type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="f3034-140">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-140">Yes</span></span> |
| <span data-ttu-id="f3034-141">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f3034-141">server</span></span> |<span data-ttu-id="f3034-142">Hello MySQL kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="f3034-142">Name of hello MySQL server.</span></span> |<span data-ttu-id="f3034-143">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-143">Yes</span></span> |
| <span data-ttu-id="f3034-144">adatbázis</span><span class="sxs-lookup"><span data-stu-id="f3034-144">database</span></span> |<span data-ttu-id="f3034-145">Hello MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="f3034-145">Name of hello MySQL database.</span></span> |<span data-ttu-id="f3034-146">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-146">Yes</span></span> |
| <span data-ttu-id="f3034-147">Séma</span><span class="sxs-lookup"><span data-stu-id="f3034-147">schema</span></span> |<span data-ttu-id="f3034-148">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="f3034-148">Name of hello schema in hello database.</span></span> |<span data-ttu-id="f3034-149">Nem</span><span class="sxs-lookup"><span data-stu-id="f3034-149">No</span></span> |
| <span data-ttu-id="f3034-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="f3034-150">authenticationType</span></span> |<span data-ttu-id="f3034-151">Tooconnect toohello MySQL-adatbázis használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="f3034-151">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="f3034-152">Lehetséges értékek a következők: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="f3034-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="f3034-153">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-153">Yes</span></span> |
| <span data-ttu-id="f3034-154">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="f3034-154">username</span></span> |<span data-ttu-id="f3034-155">Adja meg a felhasználó nevét tooconnect toohello MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f3034-155">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="f3034-156">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-156">Yes</span></span> |
| <span data-ttu-id="f3034-157">jelszó</span><span class="sxs-lookup"><span data-stu-id="f3034-157">password</span></span> |<span data-ttu-id="f3034-158">Adja meg a megadott hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f3034-158">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="f3034-159">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-159">Yes</span></span> |
| <span data-ttu-id="f3034-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="f3034-160">gatewayName</span></span> |<span data-ttu-id="f3034-161">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét kell tooconnect toohello helyszíni MySQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="f3034-161">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="f3034-162">Igen</span><span class="sxs-lookup"><span data-stu-id="f3034-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="f3034-163">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f3034-163">Dataset properties</span></span>
<span data-ttu-id="f3034-164">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f3034-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f3034-165">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="f3034-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="f3034-166">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f3034-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="f3034-167">hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a MySQL dataset) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f3034-167">hello typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has hello following properties</span></span>

| <span data-ttu-id="f3034-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f3034-168">Property</span></span> | <span data-ttu-id="f3034-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="f3034-169">Description</span></span> | <span data-ttu-id="f3034-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f3034-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3034-171">tableName</span><span class="sxs-lookup"><span data-stu-id="f3034-171">tableName</span></span> |<span data-ttu-id="f3034-172">Hello tábla hello MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f3034-172">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="f3034-173">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f3034-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="f3034-174">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f3034-174">Copy activity properties</span></span>
<span data-ttu-id="f3034-175">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f3034-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f3034-176">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f3034-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="f3034-177">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="f3034-177">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="f3034-178">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="f3034-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="f3034-179">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a MySQL), a következő tulajdonságok hello typeProperties szakaszában érhetők:</span><span class="sxs-lookup"><span data-stu-id="f3034-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="f3034-180">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f3034-180">Property</span></span> | <span data-ttu-id="f3034-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="f3034-181">Description</span></span> | <span data-ttu-id="f3034-182">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="f3034-182">Allowed values</span></span> | <span data-ttu-id="f3034-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f3034-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f3034-184">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="f3034-184">query</span></span> |<span data-ttu-id="f3034-185">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="f3034-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="f3034-186">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f3034-186">SQL query string.</span></span> <span data-ttu-id="f3034-187">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="f3034-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="f3034-188">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f3034-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-tooazure-blob"></a><span data-ttu-id="f3034-189">JSON-példa: adatok másolása az MySQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="f3034-189">JSON example: Copy data from MySQL tooAzure Blob</span></span>
<span data-ttu-id="f3034-190">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f3034-190">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f3034-191">Azt illusztrálja, hogyan toocopy adatait egy helyszíni MySQL-adatbázis használati ideje tooan Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f3034-191">It shows how toocopy data from an on-premises MySQL database tooan Azure Blob Storage.</span></span> <span data-ttu-id="f3034-192">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="f3034-192">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3034-193">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="f3034-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="f3034-194">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="f3034-194">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="f3034-195">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="f3034-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="f3034-196">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f3034-196">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="f3034-197">A társított szolgáltatás típusa [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3034-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="f3034-198">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3034-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="f3034-199">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f3034-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="f3034-200">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f3034-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="f3034-201">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f3034-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="f3034-202">hello minta másol adatokat a MySQL adatbázis tooa blob egy lekérdezés eredményét óránként.</span><span class="sxs-lookup"><span data-stu-id="f3034-202">hello sample copies data from a query result in MySQL database tooa blob hourly.</span></span> <span data-ttu-id="f3034-203">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f3034-203">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="f3034-204">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="f3034-204">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="f3034-205">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f3034-205">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="f3034-206">**MySQL társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="f3034-206">**MySQL linked service:**</span></span>

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

<span data-ttu-id="f3034-207">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="f3034-207">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="f3034-208">**MySQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f3034-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="f3034-209">hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" MySQL és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f3034-209">hello sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="f3034-210">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="f3034-210">Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

<span data-ttu-id="f3034-211">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f3034-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="f3034-212">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="f3034-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="f3034-213">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="f3034-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="f3034-214">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="f3034-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="f3034-215">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="f3034-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="f3034-216">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="f3034-216">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="f3034-217">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f3034-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="f3034-218">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="f3034-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```JSON
    {
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="f3034-219">A MySQL leképezésének</span><span class="sxs-lookup"><span data-stu-id="f3034-219">Type mapping for MySQL</span></span>
<span data-ttu-id="f3034-220">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="f3034-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="f3034-221">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="f3034-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="f3034-222">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="f3034-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="f3034-223">Adatok tooMySQL áthelyezésekor hello következő megfeleltetéseket használtak MySQL típusok too.NET típusok.</span><span class="sxs-lookup"><span data-stu-id="f3034-223">When moving data tooMySQL, hello following mappings are used from MySQL types too.NET types.</span></span>

| <span data-ttu-id="f3034-224">A MySQL-adatbázis típusa</span><span class="sxs-lookup"><span data-stu-id="f3034-224">MySQL Database type</span></span> | <span data-ttu-id="f3034-225">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="f3034-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="f3034-226">aláíratlan bigint</span><span class="sxs-lookup"><span data-stu-id="f3034-226">bigint unsigned</span></span> |<span data-ttu-id="f3034-227">Decimális</span><span class="sxs-lookup"><span data-stu-id="f3034-227">Decimal</span></span> |
| <span data-ttu-id="f3034-228">bigint</span><span class="sxs-lookup"><span data-stu-id="f3034-228">bigint</span></span> |<span data-ttu-id="f3034-229">Int64</span><span class="sxs-lookup"><span data-stu-id="f3034-229">Int64</span></span> |
| <span data-ttu-id="f3034-230">bit</span><span class="sxs-lookup"><span data-stu-id="f3034-230">bit</span></span> |<span data-ttu-id="f3034-231">Decimális</span><span class="sxs-lookup"><span data-stu-id="f3034-231">Decimal</span></span> |
| <span data-ttu-id="f3034-232">A BLOB</span><span class="sxs-lookup"><span data-stu-id="f3034-232">blob</span></span> |<span data-ttu-id="f3034-233">Byte]</span><span class="sxs-lookup"><span data-stu-id="f3034-233">Byte[]</span></span> |
| <span data-ttu-id="f3034-234">logikai érték</span><span class="sxs-lookup"><span data-stu-id="f3034-234">bool</span></span> |<span data-ttu-id="f3034-235">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="f3034-235">Boolean</span></span> |
| <span data-ttu-id="f3034-236">Karakter</span><span class="sxs-lookup"><span data-stu-id="f3034-236">char</span></span> |<span data-ttu-id="f3034-237">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-237">String</span></span> |
| <span data-ttu-id="f3034-238">Dátum</span><span class="sxs-lookup"><span data-stu-id="f3034-238">date</span></span> |<span data-ttu-id="f3034-239">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f3034-239">Datetime</span></span> |
| <span data-ttu-id="f3034-240">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f3034-240">datetime</span></span> |<span data-ttu-id="f3034-241">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f3034-241">Datetime</span></span> |
| <span data-ttu-id="f3034-242">Decimális</span><span class="sxs-lookup"><span data-stu-id="f3034-242">decimal</span></span> |<span data-ttu-id="f3034-243">Decimális</span><span class="sxs-lookup"><span data-stu-id="f3034-243">Decimal</span></span> |
| <span data-ttu-id="f3034-244">a kétszeres pontosság</span><span class="sxs-lookup"><span data-stu-id="f3034-244">double precision</span></span> |<span data-ttu-id="f3034-245">Dupla</span><span class="sxs-lookup"><span data-stu-id="f3034-245">Double</span></span> |
| <span data-ttu-id="f3034-246">Dupla</span><span class="sxs-lookup"><span data-stu-id="f3034-246">double</span></span> |<span data-ttu-id="f3034-247">Dupla</span><span class="sxs-lookup"><span data-stu-id="f3034-247">Double</span></span> |
| <span data-ttu-id="f3034-248">Enum</span><span class="sxs-lookup"><span data-stu-id="f3034-248">enum</span></span> |<span data-ttu-id="f3034-249">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-249">String</span></span> |
| <span data-ttu-id="f3034-250">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="f3034-250">float</span></span> |<span data-ttu-id="f3034-251">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="f3034-251">Single</span></span> |
| <span data-ttu-id="f3034-252">aláíratlan int</span><span class="sxs-lookup"><span data-stu-id="f3034-252">int unsigned</span></span> |<span data-ttu-id="f3034-253">Int64</span><span class="sxs-lookup"><span data-stu-id="f3034-253">Int64</span></span> |
| <span data-ttu-id="f3034-254">int</span><span class="sxs-lookup"><span data-stu-id="f3034-254">int</span></span> |<span data-ttu-id="f3034-255">Int32</span><span class="sxs-lookup"><span data-stu-id="f3034-255">Int32</span></span> |
| <span data-ttu-id="f3034-256">aláíratlan egész szám</span><span class="sxs-lookup"><span data-stu-id="f3034-256">integer unsigned</span></span> |<span data-ttu-id="f3034-257">Int64</span><span class="sxs-lookup"><span data-stu-id="f3034-257">Int64</span></span> |
| <span data-ttu-id="f3034-258">egész szám</span><span class="sxs-lookup"><span data-stu-id="f3034-258">integer</span></span> |<span data-ttu-id="f3034-259">Int32</span><span class="sxs-lookup"><span data-stu-id="f3034-259">Int32</span></span> |
| <span data-ttu-id="f3034-260">hosszú varbinary</span><span class="sxs-lookup"><span data-stu-id="f3034-260">long varbinary</span></span> |<span data-ttu-id="f3034-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="f3034-261">Byte[]</span></span> |
| <span data-ttu-id="f3034-262">hosszú varchar</span><span class="sxs-lookup"><span data-stu-id="f3034-262">long varchar</span></span> |<span data-ttu-id="f3034-263">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-263">String</span></span> |
| <span data-ttu-id="f3034-264">longblob</span><span class="sxs-lookup"><span data-stu-id="f3034-264">longblob</span></span> |<span data-ttu-id="f3034-265">Byte]</span><span class="sxs-lookup"><span data-stu-id="f3034-265">Byte[]</span></span> |
| <span data-ttu-id="f3034-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="f3034-266">longtext</span></span> |<span data-ttu-id="f3034-267">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-267">String</span></span> |
| <span data-ttu-id="f3034-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="f3034-268">mediumblob</span></span> |<span data-ttu-id="f3034-269">Byte]</span><span class="sxs-lookup"><span data-stu-id="f3034-269">Byte[]</span></span> |
| <span data-ttu-id="f3034-270">aláíratlan mediumint</span><span class="sxs-lookup"><span data-stu-id="f3034-270">mediumint unsigned</span></span> |<span data-ttu-id="f3034-271">Int64</span><span class="sxs-lookup"><span data-stu-id="f3034-271">Int64</span></span> |
| <span data-ttu-id="f3034-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="f3034-272">mediumint</span></span> |<span data-ttu-id="f3034-273">Int32</span><span class="sxs-lookup"><span data-stu-id="f3034-273">Int32</span></span> |
| <span data-ttu-id="f3034-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="f3034-274">mediumtext</span></span> |<span data-ttu-id="f3034-275">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-275">String</span></span> |
| <span data-ttu-id="f3034-276">Numerikus</span><span class="sxs-lookup"><span data-stu-id="f3034-276">numeric</span></span> |<span data-ttu-id="f3034-277">Decimális</span><span class="sxs-lookup"><span data-stu-id="f3034-277">Decimal</span></span> |
| <span data-ttu-id="f3034-278">valós</span><span class="sxs-lookup"><span data-stu-id="f3034-278">real</span></span> |<span data-ttu-id="f3034-279">Dupla</span><span class="sxs-lookup"><span data-stu-id="f3034-279">Double</span></span> |
| <span data-ttu-id="f3034-280">Állítsa be</span><span class="sxs-lookup"><span data-stu-id="f3034-280">set</span></span> |<span data-ttu-id="f3034-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-281">String</span></span> |
| <span data-ttu-id="f3034-282">aláíratlan smallint</span><span class="sxs-lookup"><span data-stu-id="f3034-282">smallint unsigned</span></span> |<span data-ttu-id="f3034-283">Int32</span><span class="sxs-lookup"><span data-stu-id="f3034-283">Int32</span></span> |
| <span data-ttu-id="f3034-284">smallint</span><span class="sxs-lookup"><span data-stu-id="f3034-284">smallint</span></span> |<span data-ttu-id="f3034-285">Int16</span><span class="sxs-lookup"><span data-stu-id="f3034-285">Int16</span></span> |
| <span data-ttu-id="f3034-286">Szöveg</span><span class="sxs-lookup"><span data-stu-id="f3034-286">text</span></span> |<span data-ttu-id="f3034-287">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-287">String</span></span> |
| <span data-ttu-id="f3034-288">time</span><span class="sxs-lookup"><span data-stu-id="f3034-288">time</span></span> |<span data-ttu-id="f3034-289">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="f3034-289">TimeSpan</span></span> |
| <span data-ttu-id="f3034-290">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="f3034-290">timestamp</span></span> |<span data-ttu-id="f3034-291">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f3034-291">Datetime</span></span> |
| <span data-ttu-id="f3034-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="f3034-292">tinyblob</span></span> |<span data-ttu-id="f3034-293">Byte]</span><span class="sxs-lookup"><span data-stu-id="f3034-293">Byte[]</span></span> |
| <span data-ttu-id="f3034-294">aláíratlan tinyint</span><span class="sxs-lookup"><span data-stu-id="f3034-294">tinyint unsigned</span></span> |<span data-ttu-id="f3034-295">Int16</span><span class="sxs-lookup"><span data-stu-id="f3034-295">Int16</span></span> |
| <span data-ttu-id="f3034-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="f3034-296">tinyint</span></span> |<span data-ttu-id="f3034-297">Int16</span><span class="sxs-lookup"><span data-stu-id="f3034-297">Int16</span></span> |
| <span data-ttu-id="f3034-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="f3034-298">tinytext</span></span> |<span data-ttu-id="f3034-299">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-299">String</span></span> |
| <span data-ttu-id="f3034-300">varchar</span><span class="sxs-lookup"><span data-stu-id="f3034-300">varchar</span></span> |<span data-ttu-id="f3034-301">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f3034-301">String</span></span> |
| <span data-ttu-id="f3034-302">Év</span><span class="sxs-lookup"><span data-stu-id="f3034-302">year</span></span> |<span data-ttu-id="f3034-303">int</span><span class="sxs-lookup"><span data-stu-id="f3034-303">Int</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="f3034-304">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="f3034-304">Map source toosink columns</span></span>
<span data-ttu-id="f3034-305">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f3034-305">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="f3034-306">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="f3034-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="f3034-307">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="f3034-307">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="f3034-308">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="f3034-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="f3034-309">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="f3034-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="f3034-310">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="f3034-310">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="f3034-311">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="f3034-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f3034-312">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="f3034-312">Performance and Tuning</span></span>
<span data-ttu-id="f3034-313">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="f3034-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
