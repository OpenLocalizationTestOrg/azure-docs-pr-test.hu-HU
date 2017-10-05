---
title: "Helyezze át az adatokat a PostgreSQL Azure Data Factory használatával |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával PostgreSQL-adatbázisból."
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
ms.openlocfilehash: fd26f0d03f8b0b352a6544a81ad952d2e2a1b7a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="b3547-103">Adatok áthelyezése az Azure Data Factory használatával PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b3547-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="b3547-104">Ez a cikk ismerteti, hogyan a másolási tevékenység az Azure Data Factoryben az adatok mozgatása egy helyszíni PostgreSQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b3547-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="b3547-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="b3547-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="b3547-106">Egy helyszíni PostgreSQL adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b3547-106">You can copy data from an on-premises PostgreSQL data store to any supported sink data store.</span></span> <span data-ttu-id="b3547-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="b3547-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="b3547-108">Adat-előállító jelenleg áthelyezése adatokat egy PostgreSQL-adatbázisból az egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz PostgreSQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b3547-108">Data factory currently supports moving data from a PostgreSQL database to other data stores, but not for moving data from other data stores to an PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b3547-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b3547-109">prerequisites</span></span>

<span data-ttu-id="b3547-110">Data Factory szolgáltatásnak a helyszíni PostgreSQL adatforrások az adatkezelési átjáró használatával történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="b3547-110">Data Factory service supports connecting to on-premises PostgreSQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="b3547-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="b3547-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="b3547-112">Átjáróra szükség, akkor is, ha egy Azure IaaS virtuális gép a PostgreSQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="b3547-112">Gateway is required even if the PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="b3547-113">Átjárót telepítheti adattárként ugyanazon infrastruktúra-szolgáltatási virtuális gép vagy egy másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b3547-113">You can install gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="b3547-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="b3547-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="b3547-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="b3547-115">Supported versions and installation</span></span>
<span data-ttu-id="b3547-116">Az adatkezelési átjáró a PostgreSQL-adatbázishoz való kapcsolódáshoz, telepítse a [PostgreSQL-Ngpsql adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 vagy a fenti az adatkezelési átjáró ugyanazon a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="b3547-116">For Data Management Gateway to connect to the PostgreSQL Database, install the [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="b3547-117">7.4 verzió PostgreSQL és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="b3547-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b3547-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b3547-118">Getting started</span></span>
<span data-ttu-id="b3547-119">A másolási tevékenység, mely az adatok egy helyszíni PostgreSQL adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="b3547-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="b3547-120">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="b3547-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="b3547-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b3547-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="b3547-122">A következő eszközök segítségével hozzon létre egy folyamatot:</span><span class="sxs-lookup"><span data-stu-id="b3547-122">You can also use the following tools to create a pipeline:</span></span> 
    - <span data-ttu-id="b3547-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b3547-123">Azure portal</span></span>
    - <span data-ttu-id="b3547-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3547-124">Visual Studio</span></span>
    - <span data-ttu-id="b3547-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3547-125">Azure PowerShell</span></span>
    - <span data-ttu-id="b3547-126">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="b3547-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="b3547-127">.NET API</span><span class="sxs-lookup"><span data-stu-id="b3547-127">.NET API</span></span>
    - <span data-ttu-id="b3547-128">REST API</span><span class="sxs-lookup"><span data-stu-id="b3547-128">REST API</span></span>

     <span data-ttu-id="b3547-129">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="b3547-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b3547-130">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="b3547-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="b3547-131">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="b3547-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="b3547-132">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="b3547-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="b3547-133">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="b3547-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b3547-134">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="b3547-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="b3547-135">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="b3547-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="b3547-136">Adatok másolása egy helyszíni PostgreSQL adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az PostgreSQL az Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="b3547-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL to Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="b3547-137">A következő szakaszok részletesen bemutatják a PostgreSQL-tárolóban való adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b3547-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b3547-138">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="b3547-138">Linked service properties</span></span>
<span data-ttu-id="b3547-139">A következő táblázat a JSON-elemek szerepelnek PostgreSQL kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="b3547-139">The following table provides description for JSON elements specific to PostgreSQL linked service.</span></span>

| <span data-ttu-id="b3547-140">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b3547-140">Property</span></span> | <span data-ttu-id="b3547-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="b3547-141">Description</span></span> | <span data-ttu-id="b3547-142">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b3547-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b3547-143">type</span><span class="sxs-lookup"><span data-stu-id="b3547-143">type</span></span> |<span data-ttu-id="b3547-144">A type tulajdonságot kell beállítani: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="b3547-144">The type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="b3547-145">Igen</span><span class="sxs-lookup"><span data-stu-id="b3547-145">Yes</span></span> |
| <span data-ttu-id="b3547-146">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b3547-146">server</span></span> |<span data-ttu-id="b3547-147">A PostgreSQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b3547-147">Name of the PostgreSQL server.</span></span> |<span data-ttu-id="b3547-148">Igen</span><span class="sxs-lookup"><span data-stu-id="b3547-148">Yes</span></span> |
| <span data-ttu-id="b3547-149">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b3547-149">database</span></span> |<span data-ttu-id="b3547-150">A PostgreSQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b3547-150">Name of the PostgreSQL database.</span></span> |<span data-ttu-id="b3547-151">Igen</span><span class="sxs-lookup"><span data-stu-id="b3547-151">Yes</span></span> |
| <span data-ttu-id="b3547-152">Séma</span><span class="sxs-lookup"><span data-stu-id="b3547-152">schema</span></span> |<span data-ttu-id="b3547-153">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="b3547-153">Name of the schema in the database.</span></span> <span data-ttu-id="b3547-154">A séma neve a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b3547-154">The schema name is case-sensitive.</span></span> |<span data-ttu-id="b3547-155">Nem</span><span class="sxs-lookup"><span data-stu-id="b3547-155">No</span></span> |
| <span data-ttu-id="b3547-156">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b3547-156">authenticationType</span></span> |<span data-ttu-id="b3547-157">A PostgreSQL-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b3547-157">Type of authentication used to connect to the PostgreSQL database.</span></span> <span data-ttu-id="b3547-158">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b3547-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b3547-159">Igen</span><span class="sxs-lookup"><span data-stu-id="b3547-159">Yes</span></span> |
| <span data-ttu-id="b3547-160">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b3547-160">username</span></span> |<span data-ttu-id="b3547-161">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="b3547-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b3547-162">Nem</span><span class="sxs-lookup"><span data-stu-id="b3547-162">No</span></span> |
| <span data-ttu-id="b3547-163">jelszó</span><span class="sxs-lookup"><span data-stu-id="b3547-163">password</span></span> |<span data-ttu-id="b3547-164">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b3547-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="b3547-165">Nem</span><span class="sxs-lookup"><span data-stu-id="b3547-165">No</span></span> |
| <span data-ttu-id="b3547-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b3547-166">gatewayName</span></span> |<span data-ttu-id="b3547-167">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni PostgreSQL-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="b3547-167">Name of the gateway that the Data Factory service should use to connect to the on-premises PostgreSQL database.</span></span> |<span data-ttu-id="b3547-168">Igen</span><span class="sxs-lookup"><span data-stu-id="b3547-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="b3547-169">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b3547-169">Dataset properties</span></span>
<span data-ttu-id="b3547-170">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b3547-170">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b3547-171">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="b3547-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="b3547-172">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="b3547-172">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="b3547-173">A typeProperties szakasz típusú adatkészlet **RelationalTable** (amely tartalmazza a PostgreSQL dataset) tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="b3547-173">The typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has the following properties:</span></span>

| <span data-ttu-id="b3547-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b3547-174">Property</span></span> | <span data-ttu-id="b3547-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="b3547-175">Description</span></span> | <span data-ttu-id="b3547-176">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b3547-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b3547-177">tableName</span><span class="sxs-lookup"><span data-stu-id="b3547-177">tableName</span></span> |<span data-ttu-id="b3547-178">A tábla PostgreSQL-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b3547-178">Name of the table in the PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="b3547-179">A táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b3547-179">The tableName is case-sensitive.</span></span> |<span data-ttu-id="b3547-180">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b3547-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="b3547-181">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b3547-181">Copy activity properties</span></span>
<span data-ttu-id="b3547-182">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b3547-182">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b3547-183">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b3547-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="b3547-184">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="b3547-184">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="b3547-185">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="b3547-185">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="b3547-186">Ha a forrás típusa van **RelationalSource** (amely tartalmazza a PostgreSQL), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="b3547-186">When source is of type **RelationalSource** (which includes PostgreSQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="b3547-187">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b3547-187">Property</span></span> | <span data-ttu-id="b3547-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="b3547-188">Description</span></span> | <span data-ttu-id="b3547-189">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b3547-189">Allowed values</span></span> | <span data-ttu-id="b3547-190">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b3547-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b3547-191">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b3547-191">query</span></span> |<span data-ttu-id="b3547-192">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="b3547-192">Use the custom query to read data.</span></span> |<span data-ttu-id="b3547-193">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b3547-193">SQL query string.</span></span> <span data-ttu-id="b3547-194">Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="b3547-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="b3547-195">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b3547-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="b3547-196">Séma-és tábla-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="b3547-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="b3547-197">Tegye őket `""` (idézőjelek) a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="b3547-197">Enclose them in `""` (double quotes) in the query.</span></span>  

<span data-ttu-id="b3547-198">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b3547-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a><span data-ttu-id="b3547-199">JSON-példa: adatok másolása az PostgreSQL az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="b3547-199">JSON example: Copy data from PostgreSQL to Azure Blob</span></span>
<span data-ttu-id="b3547-200">Ebben a példában a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b3547-200">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b3547-201">Adatok másolása PostgreSQL-adatbázisból az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="b3547-201">They show how to copy data from PostgreSQL database to Azure Blob Storage.</span></span> <span data-ttu-id="b3547-202">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="b3547-202">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="b3547-203">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="b3547-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="b3547-204">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="b3547-204">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="b3547-205">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="b3547-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="b3547-206">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b3547-206">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="b3547-207">A társított szolgáltatás típusa [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b3547-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="b3547-208">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b3547-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b3547-209">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b3547-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="b3547-210">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b3547-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b3547-211">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b3547-211">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b3547-212">A minta másol adatokat egy lekérdezés eredményét PostgreSQL-adatbázisban egy blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="b3547-212">The sample copies data from a query result in PostgreSQL database to a blob every hour.</span></span> <span data-ttu-id="b3547-213">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b3547-213">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b3547-214">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="b3547-214">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="b3547-215">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b3547-215">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="b3547-216">**PostgreSQL társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="b3547-216">**PostgreSQL linked service:**</span></span>

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
<span data-ttu-id="b3547-217">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="b3547-217">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="b3547-218">**PostgreSQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="b3547-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="b3547-219">A példa azt feltételezi, hogy létrehozott egy tábla "MyTable" PostgreSQL és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b3547-219">The sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="b3547-220">Beállítás `"external": true` tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="b3547-220">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="b3547-221">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="b3547-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="b3547-222">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="b3547-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b3547-223">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="b3547-223">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="b3547-224">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="b3547-224">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="b3547-225">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="b3547-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="b3547-226">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="b3547-226">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="b3547-227">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b3547-227">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="b3547-228">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztja az adatokat a public.usstates táblából a PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="b3547-228">The SQL query specified for the **query** property selects the data from the public.usstates table in the PostgreSQL database.</span></span>

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
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="b3547-229">Típus identitástagok esetén PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b3547-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="b3547-230">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az eseményforrás-típusnak gyűjtése módszert használja a következő 2. lépés típusok automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="b3547-230">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="b3547-231">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="b3547-231">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b3547-232">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="b3547-232">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b3547-233">Amikor adatokat PostgreSQL helyezi át, a következő megfeleltetéseket segítségével PostgreSQL típusból .NET-típusa.</span><span class="sxs-lookup"><span data-stu-id="b3547-233">When moving data to PostgreSQL, the following mappings are used from PostgreSQL type to .NET type.</span></span>

| <span data-ttu-id="b3547-234">PostgreSQL-adatbázisból típusa</span><span class="sxs-lookup"><span data-stu-id="b3547-234">PostgreSQL Database type</span></span> | <span data-ttu-id="b3547-235">PostgresSQL aliasok</span><span class="sxs-lookup"><span data-stu-id="b3547-235">PostgresSQL aliases</span></span> | <span data-ttu-id="b3547-236">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="b3547-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b3547-237">abstime</span><span class="sxs-lookup"><span data-stu-id="b3547-237">abstime</span></span> | |<span data-ttu-id="b3547-238">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b3547-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="b3547-239">bigint</span><span class="sxs-lookup"><span data-stu-id="b3547-239">bigint</span></span> |<span data-ttu-id="b3547-240">int8</span><span class="sxs-lookup"><span data-stu-id="b3547-240">int8</span></span> |<span data-ttu-id="b3547-241">Int64</span><span class="sxs-lookup"><span data-stu-id="b3547-241">Int64</span></span> |
| <span data-ttu-id="b3547-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="b3547-242">bigserial</span></span> |<span data-ttu-id="b3547-243">serial8</span><span class="sxs-lookup"><span data-stu-id="b3547-243">serial8</span></span> |<span data-ttu-id="b3547-244">Int64</span><span class="sxs-lookup"><span data-stu-id="b3547-244">Int64</span></span> |
| <span data-ttu-id="b3547-245">bit [(n)]</span><span class="sxs-lookup"><span data-stu-id="b3547-245">bit [ (n) ]</span></span> | |<span data-ttu-id="b3547-246">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="b3547-247">bit különböző [(n)]</span><span class="sxs-lookup"><span data-stu-id="b3547-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="b3547-248">varbit</span><span class="sxs-lookup"><span data-stu-id="b3547-248">varbit</span></span> |<span data-ttu-id="b3547-249">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-249">Byte[], String</span></span> |
| <span data-ttu-id="b3547-250">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="b3547-250">boolean</span></span> |<span data-ttu-id="b3547-251">logikai érték</span><span class="sxs-lookup"><span data-stu-id="b3547-251">bool</span></span> |<span data-ttu-id="b3547-252">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="b3547-252">Boolean</span></span> |
| <span data-ttu-id="b3547-253">Mezőbe</span><span class="sxs-lookup"><span data-stu-id="b3547-253">box</span></span> | |<span data-ttu-id="b3547-254">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-255">bytea</span><span class="sxs-lookup"><span data-stu-id="b3547-255">bytea</span></span> | |<span data-ttu-id="b3547-256">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-257">[(n)] karakter</span><span class="sxs-lookup"><span data-stu-id="b3547-257">character [ (n) ]</span></span> |<span data-ttu-id="b3547-258">a char [(n)]</span><span class="sxs-lookup"><span data-stu-id="b3547-258">char [ (n) ]</span></span> |<span data-ttu-id="b3547-259">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-259">String</span></span> |
| <span data-ttu-id="b3547-260">[(n)] eltérő karaktert</span><span class="sxs-lookup"><span data-stu-id="b3547-260">character varying [ (n) ]</span></span> |<span data-ttu-id="b3547-261">varchar [(n)]</span><span class="sxs-lookup"><span data-stu-id="b3547-261">varchar [ (n) ]</span></span> |<span data-ttu-id="b3547-262">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-262">String</span></span> |
| <span data-ttu-id="b3547-263">CID</span><span class="sxs-lookup"><span data-stu-id="b3547-263">cid</span></span> | |<span data-ttu-id="b3547-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-264">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="b3547-265">cidr</span></span> | |<span data-ttu-id="b3547-266">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-266">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-267">kör</span><span class="sxs-lookup"><span data-stu-id="b3547-267">circle</span></span> | |<span data-ttu-id="b3547-268">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-269">Dátum</span><span class="sxs-lookup"><span data-stu-id="b3547-269">date</span></span> | |<span data-ttu-id="b3547-270">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b3547-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="b3547-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="b3547-271">daterange</span></span> | |<span data-ttu-id="b3547-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-272">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-273">a kétszeres pontosság</span><span class="sxs-lookup"><span data-stu-id="b3547-273">double precision</span></span> |<span data-ttu-id="b3547-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="b3547-274">float8</span></span> |<span data-ttu-id="b3547-275">Dupla</span><span class="sxs-lookup"><span data-stu-id="b3547-275">Double</span></span> |
| <span data-ttu-id="b3547-276">INet</span><span class="sxs-lookup"><span data-stu-id="b3547-276">inet</span></span> | |<span data-ttu-id="b3547-277">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-278">intarry</span><span class="sxs-lookup"><span data-stu-id="b3547-278">intarry</span></span> | |<span data-ttu-id="b3547-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-279">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-280">int4range</span><span class="sxs-lookup"><span data-stu-id="b3547-280">int4range</span></span> | |<span data-ttu-id="b3547-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-281">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-282">int8range</span><span class="sxs-lookup"><span data-stu-id="b3547-282">int8range</span></span> | |<span data-ttu-id="b3547-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-283">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-284">egész szám</span><span class="sxs-lookup"><span data-stu-id="b3547-284">integer</span></span> |<span data-ttu-id="b3547-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="b3547-285">int, int4</span></span> |<span data-ttu-id="b3547-286">Int32</span><span class="sxs-lookup"><span data-stu-id="b3547-286">Int32</span></span> |
| <span data-ttu-id="b3547-287">időköz [mezők] [(p)]</span><span class="sxs-lookup"><span data-stu-id="b3547-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="b3547-288">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b3547-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="b3547-289">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="b3547-289">json</span></span> | |<span data-ttu-id="b3547-290">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-290">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="b3547-291">jsonb</span></span> | |<span data-ttu-id="b3547-292">Byte]</span><span class="sxs-lookup"><span data-stu-id="b3547-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="b3547-293">sor</span><span class="sxs-lookup"><span data-stu-id="b3547-293">line</span></span> | |<span data-ttu-id="b3547-294">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-295">lseg</span><span class="sxs-lookup"><span data-stu-id="b3547-295">lseg</span></span> | |<span data-ttu-id="b3547-296">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="b3547-297">macaddr</span></span> | |<span data-ttu-id="b3547-298">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-299">pénz</span><span class="sxs-lookup"><span data-stu-id="b3547-299">money</span></span> | |<span data-ttu-id="b3547-300">Decimális</span><span class="sxs-lookup"><span data-stu-id="b3547-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="b3547-301">numerikus [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="b3547-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="b3547-302">Decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="b3547-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="b3547-303">Decimális</span><span class="sxs-lookup"><span data-stu-id="b3547-303">Decimal</span></span> |
| <span data-ttu-id="b3547-304">numrange</span><span class="sxs-lookup"><span data-stu-id="b3547-304">numrange</span></span> | |<span data-ttu-id="b3547-305">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-305">String</span></span> |&nbsp;
| <span data-ttu-id="b3547-306">OID</span><span class="sxs-lookup"><span data-stu-id="b3547-306">oid</span></span> | |<span data-ttu-id="b3547-307">Int32</span><span class="sxs-lookup"><span data-stu-id="b3547-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="b3547-308">Elérési út</span><span class="sxs-lookup"><span data-stu-id="b3547-308">path</span></span> | |<span data-ttu-id="b3547-309">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="b3547-310">pg_lsn</span></span> | |<span data-ttu-id="b3547-311">Int64</span><span class="sxs-lookup"><span data-stu-id="b3547-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="b3547-312">pont</span><span class="sxs-lookup"><span data-stu-id="b3547-312">point</span></span> | |<span data-ttu-id="b3547-313">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-314">Sokszög</span><span class="sxs-lookup"><span data-stu-id="b3547-314">polygon</span></span> | |<span data-ttu-id="b3547-315">Byte [], karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b3547-316">valós</span><span class="sxs-lookup"><span data-stu-id="b3547-316">real</span></span> |<span data-ttu-id="b3547-317">float4</span><span class="sxs-lookup"><span data-stu-id="b3547-317">float4</span></span> |<span data-ttu-id="b3547-318">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="b3547-318">Single</span></span> |
| <span data-ttu-id="b3547-319">smallint</span><span class="sxs-lookup"><span data-stu-id="b3547-319">smallint</span></span> |<span data-ttu-id="b3547-320">int2</span><span class="sxs-lookup"><span data-stu-id="b3547-320">int2</span></span> |<span data-ttu-id="b3547-321">Int16</span><span class="sxs-lookup"><span data-stu-id="b3547-321">Int16</span></span> |
| <span data-ttu-id="b3547-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="b3547-322">smallserial</span></span> |<span data-ttu-id="b3547-323">serial2</span><span class="sxs-lookup"><span data-stu-id="b3547-323">serial2</span></span> |<span data-ttu-id="b3547-324">Int16</span><span class="sxs-lookup"><span data-stu-id="b3547-324">Int16</span></span> |
| <span data-ttu-id="b3547-325">Soros</span><span class="sxs-lookup"><span data-stu-id="b3547-325">serial</span></span> |<span data-ttu-id="b3547-326">serial4</span><span class="sxs-lookup"><span data-stu-id="b3547-326">serial4</span></span> |<span data-ttu-id="b3547-327">Int32</span><span class="sxs-lookup"><span data-stu-id="b3547-327">Int32</span></span> |
| <span data-ttu-id="b3547-328">Szöveg</span><span class="sxs-lookup"><span data-stu-id="b3547-328">text</span></span> | |<span data-ttu-id="b3547-329">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b3547-329">String</span></span> |&nbsp;

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="b3547-330">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="b3547-330">Map source to sink columns</span></span>
<span data-ttu-id="b3547-331">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b3547-331">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="b3547-332">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="b3547-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="b3547-333">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b3547-333">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="b3547-334">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="b3547-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="b3547-335">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="b3547-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="b3547-336">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="b3547-336">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="b3547-337">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="b3547-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b3547-338">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="b3547-338">Performance and Tuning</span></span>
<span data-ttu-id="b3547-339">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="b3547-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
