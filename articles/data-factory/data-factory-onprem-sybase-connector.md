---
title: "Adatok áthelyezése az Azure Data Factory használatával Sybase |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával Sybase-adatbázisból."
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
ms.openlocfilehash: 617e604b220b8bc1c452e67da83f733448e16c0b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="a1c4e-103">Adatok áthelyezése az Azure Data Factory használatával Sybase</span><span class="sxs-lookup"><span data-stu-id="a1c4e-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="a1c4e-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Sybase database.</span></span> <span data-ttu-id="a1c4e-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="a1c4e-106">Egy helyszíni Sybase adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-106">You can copy data from an on-premises Sybase data store to any supported sink data store.</span></span> <span data-ttu-id="a1c4e-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a1c4e-108">Adat-előállító jelenleg mozgási adatok kizárólag egy Sybase adattárból egyéb adattárakhoz, de nem az egyéb adattárakhoz adatok áthelyezése a Sybase-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-108">Data factory currently supports only moving data from a Sybase data store to other data stores, but not for moving data from other data stores to a Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a1c4e-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a1c4e-109">Prerequisites</span></span>
<span data-ttu-id="a1c4e-110">Data Factory szolgáltatásnak a helyszíni Sybase adatforrások az adatkezelési átjáró használatával történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-110">Data Factory service supports connecting to on-premises Sybase sources using the Data Management Gateway.</span></span> <span data-ttu-id="a1c4e-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="a1c4e-112">Átjáróra szükség, akkor is, ha a Sybase-adatbázishoz egy Azure IaaS virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-112">Gateway is required even if the Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="a1c4e-113">Telepítheti az átjáró adattárként ugyanazon infrastruktúra-szolgáltatási virtuális gép vagy egy másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c4e-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="a1c4e-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="a1c4e-115">Supported versions and installation</span></span>
<span data-ttu-id="a1c4e-116">Az adatkezelési átjáró a Sybase-adatbázishoz való kapcsolódáshoz, telepítenie kell a [Sybase iAnywhere.Data.SQLAnywhere adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=324846) 16 vagy a fenti az adatkezelési átjáró ugyanazon a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-116">For Data Management Gateway to connect to the Sybase Database, you need to install the [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="a1c4e-117">Sybase verzió 16 és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a1c4e-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a1c4e-118">Getting started</span></span>
<span data-ttu-id="a1c4e-119">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="a1c4e-120">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="a1c4e-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="a1c4e-122">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a1c4e-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a1c4e-124">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="a1c4e-125">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="a1c4e-126">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="a1c4e-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a1c4e-128">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="a1c4e-129">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="a1c4e-130">Adatok másolása egy helyszíni Sybase adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Sybase az Azure Blob](#json-example-copy-data-from-sybase-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase to Azure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a1c4e-131">A következő szakaszok részletesen bemutatják a Sybase-tárolóban való adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a1c4e-132">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="a1c4e-132">Linked service properties</span></span>
<span data-ttu-id="a1c4e-133">A következő táblázat a JSON-elemek szerepelnek Sybase kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-133">The following table provides description for JSON elements specific to Sybase linked service.</span></span>

| <span data-ttu-id="a1c4e-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a1c4e-134">Property</span></span> | <span data-ttu-id="a1c4e-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="a1c4e-135">Description</span></span> | <span data-ttu-id="a1c4e-136">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a1c4e-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1c4e-137">type</span><span class="sxs-lookup"><span data-stu-id="a1c4e-137">type</span></span> |<span data-ttu-id="a1c4e-138">A type tulajdonságot kell beállítani: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-138">The type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="a1c4e-139">Igen</span><span class="sxs-lookup"><span data-stu-id="a1c4e-139">Yes</span></span> |
| <span data-ttu-id="a1c4e-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a1c4e-140">server</span></span> |<span data-ttu-id="a1c4e-141">A Sybase-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-141">Name of the Sybase server.</span></span> |<span data-ttu-id="a1c4e-142">Igen</span><span class="sxs-lookup"><span data-stu-id="a1c4e-142">Yes</span></span> |
| <span data-ttu-id="a1c4e-143">adatbázis</span><span class="sxs-lookup"><span data-stu-id="a1c4e-143">database</span></span> |<span data-ttu-id="a1c4e-144">Neve a Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-144">Name of the Sybase database.</span></span> |<span data-ttu-id="a1c4e-145">Igen</span><span class="sxs-lookup"><span data-stu-id="a1c4e-145">Yes</span></span> |
| <span data-ttu-id="a1c4e-146">Séma</span><span class="sxs-lookup"><span data-stu-id="a1c4e-146">schema</span></span> |<span data-ttu-id="a1c4e-147">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-147">Name of the schema in the database.</span></span> |<span data-ttu-id="a1c4e-148">Nem</span><span class="sxs-lookup"><span data-stu-id="a1c4e-148">No</span></span> |
| <span data-ttu-id="a1c4e-149">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a1c4e-149">authenticationType</span></span> |<span data-ttu-id="a1c4e-150">A Sybase-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-150">Type of authentication used to connect to the Sybase database.</span></span> <span data-ttu-id="a1c4e-151">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="a1c4e-152">Igen</span><span class="sxs-lookup"><span data-stu-id="a1c4e-152">Yes</span></span> |
| <span data-ttu-id="a1c4e-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a1c4e-153">username</span></span> |<span data-ttu-id="a1c4e-154">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="a1c4e-155">Nem</span><span class="sxs-lookup"><span data-stu-id="a1c4e-155">No</span></span> |
| <span data-ttu-id="a1c4e-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="a1c4e-156">password</span></span> |<span data-ttu-id="a1c4e-157">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-157">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="a1c4e-158">Nem</span><span class="sxs-lookup"><span data-stu-id="a1c4e-158">No</span></span> |
| <span data-ttu-id="a1c4e-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a1c4e-159">gatewayName</span></span> |<span data-ttu-id="a1c4e-160">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni Sybase-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-160">Name of the gateway that the Data Factory service should use to connect to the on-premises Sybase database.</span></span> |<span data-ttu-id="a1c4e-161">Igen</span><span class="sxs-lookup"><span data-stu-id="a1c4e-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="a1c4e-162">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a1c4e-162">Dataset properties</span></span>
<span data-ttu-id="a1c4e-163">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-163">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a1c4e-164">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a1c4e-165">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-165">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="a1c4e-166">A **typeProperties** típusú adatkészlet szakasz **RelationalTable** (amely tartalmazza a Sybase dataset) tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-166">The **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has the following properties:</span></span>

| <span data-ttu-id="a1c4e-167">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a1c4e-167">Property</span></span> | <span data-ttu-id="a1c4e-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="a1c4e-168">Description</span></span> | <span data-ttu-id="a1c4e-169">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a1c4e-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1c4e-170">tableName</span><span class="sxs-lookup"><span data-stu-id="a1c4e-170">tableName</span></span> |<span data-ttu-id="a1c4e-171">A tábla a Sybase-adatbázishoz példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-171">Name of the table in the Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="a1c4e-172">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="a1c4e-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="a1c4e-173">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a1c4e-173">Copy activity properties</span></span>
<span data-ttu-id="a1c4e-174">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a1c4e-175">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="a1c4e-176">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-176">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="a1c4e-177">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-177">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="a1c4e-178">Ha a forrás típusa nem **RelationalSource** (amely tartalmazza a Sybase), a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-178">When the source is of type **RelationalSource** (which includes Sybase), the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="a1c4e-179">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a1c4e-179">Property</span></span> | <span data-ttu-id="a1c4e-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="a1c4e-180">Description</span></span> | <span data-ttu-id="a1c4e-181">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="a1c4e-181">Allowed values</span></span> | <span data-ttu-id="a1c4e-182">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a1c4e-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a1c4e-183">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="a1c4e-183">query</span></span> |<span data-ttu-id="a1c4e-184">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-184">Use the custom query to read data.</span></span> |<span data-ttu-id="a1c4e-185">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-185">SQL query string.</span></span> <span data-ttu-id="a1c4e-186">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="a1c4e-187">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="a1c4e-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-to-azure-blob"></a><span data-ttu-id="a1c4e-188">JSON-példa: adatok másolása az Sybase az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="a1c4e-188">JSON example: Copy data from Sybase to Azure Blob</span></span>
<span data-ttu-id="a1c4e-189">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-189">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a1c4e-190">Adatok másolása Sybase-adatbázisból az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-190">They show how to copy data from Sybase database to Azure Blob Storage.</span></span> <span data-ttu-id="a1c4e-191">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-191">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="a1c4e-192">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-192">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="a1c4e-193">A társított szolgáltatás típusa [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="a1c4e-194">A következő típusú liked szolgáltatást [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a1c4e-195">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="a1c4e-196">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a1c4e-197">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-197">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a1c4e-198">A minta másol adatokat egy lekérdezés eredményét a Sybase-adatbázishoz egy blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-198">The sample copies data from a query result in Sybase database to a blob every hour.</span></span> <span data-ttu-id="a1c4e-199">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-199">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="a1c4e-200">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-200">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="a1c4e-201">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-201">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="a1c4e-202">**Sybase társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-202">**Sybase linked service:**</span></span>

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

<span data-ttu-id="a1c4e-203">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-203">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="a1c4e-204">**Sybase bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="a1c4e-205">A példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Sybase és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-205">The sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="a1c4e-206">"External" beállítása: igaz, hogy ehhez az adatkészlethez az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák arról tájékoztatja a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-206">Setting “external”: true informs the Data Factory service that this dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="a1c4e-207">Figyelje meg, hogy a **típus** társított szolgáltatás beállítása: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-207">Notice that the **type** of the linked service is set to: **RelationalTable**.</span></span>

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

<span data-ttu-id="a1c4e-208">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="a1c4e-209">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-209">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a1c4e-210">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-210">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="a1c4e-211">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-211">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="a1c4e-212">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="a1c4e-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="a1c4e-213">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-213">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="a1c4e-214">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-214">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="a1c4e-215">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság a DBA kiválasztja azokat az adatokat. Rendelések tábla az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-215">The SQL query specified for the **query** property selects the data from the DBA.Orders table in the database.</span></span>

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

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="a1c4e-216">Típusleképezés a Sybase rendszerhez</span><span class="sxs-lookup"><span data-stu-id="a1c4e-216">Type mapping for Sybase</span></span>
<span data-ttu-id="a1c4e-217">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során hajtja végre a módszert használja a következő 2. lépés típusok gyűjtése eseményforrás-típusnak automatikus típuskonverziók:</span><span class="sxs-lookup"><span data-stu-id="a1c4e-217">As mentioned in the [Data Movement Activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="a1c4e-218">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="a1c4e-218">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="a1c4e-219">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="a1c4e-219">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="a1c4e-220">Sybase T-SQL és a T-SQL-típusokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="a1c4e-221">Leképezési tábla sql-típus a .NET-típusa, lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-221">For a mapping table from sql types to .NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="a1c4e-222">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="a1c4e-222">Map source to sink columns</span></span>
<span data-ttu-id="a1c4e-223">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-223">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a1c4e-224">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="a1c4e-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="a1c4e-225">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-225">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="a1c4e-226">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a1c4e-227">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a1c4e-228">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-228">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a1c4e-229">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="a1c4e-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a1c4e-230">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="a1c4e-230">Performance and Tuning</span></span>
<span data-ttu-id="a1c4e-231">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="a1c4e-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
