---
title: "Adatok áthelyezése az Azure Data Factory használatával Teradata |} Microsoft Docs"
description: "A Data Factory szolgáltatásnak, amely lehetővé teszi az adatok áthelyezése Teradata-adatbázishoz a Teradata-összekötő megismerése"
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
ms.openlocfilehash: 01edb32cd9e20d4199feac5b98a73aa06b74fec2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a><span data-ttu-id="baf1c-103">Adatok áthelyezése az Azure Data Factory használatával teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="baf1c-103">Move data from Teradata using Azure Data Factory</span></span>
<span data-ttu-id="baf1c-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni Teradata-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="baf1c-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Teradata database.</span></span> <span data-ttu-id="baf1c-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="baf1c-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="baf1c-106">Egy helyszíni Teradata adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="baf1c-106">You can copy data from an on-premises Teradata data store to any supported sink data store.</span></span> <span data-ttu-id="baf1c-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="baf1c-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="baf1c-108">Adat-előállító jelenleg csak áthelyezése adatait a Teradata-tárolóban egyéb adattárakhoz, de nem az egyéb adattárakhoz adatok áthelyezése a Teradata-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="baf1c-108">Data factory currently supports only moving data from a Teradata data store to other data stores, but not for moving data from other data stores to a Teradata data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="baf1c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="baf1c-109">Prerequisites</span></span>
<span data-ttu-id="baf1c-110">Adat-előállítót a helyszíni Teradata adatforrások az adatkezelési átjáró keresztül történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="baf1c-110">Data factory supports connecting to on-premises Teradata sources via the Data Management Gateway.</span></span> <span data-ttu-id="baf1c-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="baf1c-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="baf1c-112">Átjáróra szükség, akkor is, ha a Teradata egy Azure IaaS virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="baf1c-112">Gateway is required even if the Teradata is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="baf1c-113">Telepítheti az átjáró adattárként ugyanazon infrastruktúra-szolgáltatási virtuális gép vagy egy másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="baf1c-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="baf1c-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="baf1c-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="baf1c-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="baf1c-115">Supported versions and installation</span></span>
<span data-ttu-id="baf1c-116">Az adatkezelési átjáró a Teradata-adatbázishoz való kapcsolódáshoz, telepítenie kell a [.NET-adatszolgáltató a teradata rendszerhez](http://go.microsoft.com/fwlink/?LinkId=278886) 14 verzió vagy újabb az adatkezelési átjáró ugyanazon a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="baf1c-116">For Data Management Gateway to connect to the Teradata Database, you need to install the [.NET Data Provider for Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="baf1c-117">Teradata 12-es és újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="baf1c-117">Teradata version 12 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="baf1c-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="baf1c-118">Getting started</span></span>
<span data-ttu-id="baf1c-119">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="baf1c-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="baf1c-120">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="baf1c-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="baf1c-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="baf1c-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="baf1c-122">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="baf1c-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="baf1c-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="baf1c-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="baf1c-124">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="baf1c-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="baf1c-125">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="baf1c-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="baf1c-126">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="baf1c-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="baf1c-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="baf1c-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="baf1c-128">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="baf1c-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="baf1c-129">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="baf1c-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="baf1c-130">Adatok másolása egy helyszíni Teradata adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Teradata az Azure Blob](#json-example-copy-data-from-teradata-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="baf1c-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Teradata data store, see [JSON example: Copy data from Teradata to Azure Blob](#json-example-copy-data-from-teradata-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="baf1c-131">A következő szakaszok részletesen bemutatják a Teradata-tárolóban való adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="baf1c-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Teradata data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="baf1c-132">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="baf1c-132">Linked service properties</span></span>
<span data-ttu-id="baf1c-133">A következő táblázat a JSON-elemek szerepelnek a teradata rendszerhez kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="baf1c-133">The following table provides description for JSON elements specific to Teradata linked service.</span></span>

| <span data-ttu-id="baf1c-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="baf1c-134">Property</span></span> | <span data-ttu-id="baf1c-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="baf1c-135">Description</span></span> | <span data-ttu-id="baf1c-136">Szükséges</span><span class="sxs-lookup"><span data-stu-id="baf1c-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="baf1c-137">type</span><span class="sxs-lookup"><span data-stu-id="baf1c-137">type</span></span> |<span data-ttu-id="baf1c-138">A type tulajdonságot kell beállítani: **OnPremisesTeradata**</span><span class="sxs-lookup"><span data-stu-id="baf1c-138">The type property must be set to: **OnPremisesTeradata**</span></span> |<span data-ttu-id="baf1c-139">Igen</span><span class="sxs-lookup"><span data-stu-id="baf1c-139">Yes</span></span> |
| <span data-ttu-id="baf1c-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="baf1c-140">server</span></span> |<span data-ttu-id="baf1c-141">A Teradata-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="baf1c-141">Name of the Teradata server.</span></span> |<span data-ttu-id="baf1c-142">Igen</span><span class="sxs-lookup"><span data-stu-id="baf1c-142">Yes</span></span> |
| <span data-ttu-id="baf1c-143">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="baf1c-143">authenticationType</span></span> |<span data-ttu-id="baf1c-144">A Teradata-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="baf1c-144">Type of authentication used to connect to the Teradata database.</span></span> <span data-ttu-id="baf1c-145">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="baf1c-145">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="baf1c-146">Igen</span><span class="sxs-lookup"><span data-stu-id="baf1c-146">Yes</span></span> |
| <span data-ttu-id="baf1c-147">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="baf1c-147">username</span></span> |<span data-ttu-id="baf1c-148">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="baf1c-148">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="baf1c-149">Nem</span><span class="sxs-lookup"><span data-stu-id="baf1c-149">No</span></span> |
| <span data-ttu-id="baf1c-150">jelszó</span><span class="sxs-lookup"><span data-stu-id="baf1c-150">password</span></span> |<span data-ttu-id="baf1c-151">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="baf1c-151">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="baf1c-152">Nem</span><span class="sxs-lookup"><span data-stu-id="baf1c-152">No</span></span> |
| <span data-ttu-id="baf1c-153">gatewayName</span><span class="sxs-lookup"><span data-stu-id="baf1c-153">gatewayName</span></span> |<span data-ttu-id="baf1c-154">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni Teradata-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="baf1c-154">Name of the gateway that the Data Factory service should use to connect to the on-premises Teradata database.</span></span> |<span data-ttu-id="baf1c-155">Igen</span><span class="sxs-lookup"><span data-stu-id="baf1c-155">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="baf1c-156">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="baf1c-156">Dataset properties</span></span>
<span data-ttu-id="baf1c-157">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="baf1c-157">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="baf1c-158">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="baf1c-158">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="baf1c-159">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="baf1c-159">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="baf1c-160">Jelenleg nem támogatott a Teradata-adatkészlet tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="baf1c-160">Currently, there are no type properties supported for the Teradata dataset.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="baf1c-161">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="baf1c-161">Copy activity properties</span></span>
<span data-ttu-id="baf1c-162">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="baf1c-162">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="baf1c-163">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="baf1c-163">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="baf1c-164">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="baf1c-164">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="baf1c-165">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="baf1c-165">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="baf1c-166">Ha a forrás típusa nem **RelationalSource** (amely tartalmazza a teradata rendszerhez), a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="baf1c-166">When the source is of type **RelationalSource** (which includes Teradata), the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="baf1c-167">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="baf1c-167">Property</span></span> | <span data-ttu-id="baf1c-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="baf1c-168">Description</span></span> | <span data-ttu-id="baf1c-169">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="baf1c-169">Allowed values</span></span> | <span data-ttu-id="baf1c-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="baf1c-170">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="baf1c-171">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="baf1c-171">query</span></span> |<span data-ttu-id="baf1c-172">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="baf1c-172">Use the custom query to read data.</span></span> |<span data-ttu-id="baf1c-173">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="baf1c-173">SQL query string.</span></span> <span data-ttu-id="baf1c-174">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="baf1c-174">For example: select * from MyTable.</span></span> |<span data-ttu-id="baf1c-175">Igen</span><span class="sxs-lookup"><span data-stu-id="baf1c-175">Yes</span></span> |

### <a name="json-example-copy-data-from-teradata-to-azure-blob"></a><span data-ttu-id="baf1c-176">JSON-példa: adatok másolása az Teradata az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="baf1c-176">JSON example: Copy data from Teradata to Azure Blob</span></span>
<span data-ttu-id="baf1c-177">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="baf1c-177">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="baf1c-178">A Teradata adatok másolása az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="baf1c-178">They show how to copy data from Teradata to Azure Blob Storage.</span></span> <span data-ttu-id="baf1c-179">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="baf1c-179">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="baf1c-180">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="baf1c-180">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="baf1c-181">A társított szolgáltatás típusa [OnPremisesTeradata](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="baf1c-181">A linked service of type [OnPremisesTeradata](#linked-service-properties).</span></span>
2. <span data-ttu-id="baf1c-182">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="baf1c-182">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="baf1c-183">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="baf1c-183">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="baf1c-184">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="baf1c-184">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="baf1c-185">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="baf1c-185">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="baf1c-186">A minta másol adatokat egy lekérdezés eredményét a Teradata-adatbázishoz egy blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="baf1c-186">The sample copies data from a query result in Teradata database to a blob every hour.</span></span> <span data-ttu-id="baf1c-187">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="baf1c-187">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="baf1c-188">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="baf1c-188">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="baf1c-189">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="baf1c-189">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="baf1c-190">**Teradata társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="baf1c-190">**Teradata linked service:**</span></span>

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

<span data-ttu-id="baf1c-191">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="baf1c-191">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="baf1c-192">**Teradata bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="baf1c-192">**Teradata input dataset:**</span></span>

<span data-ttu-id="baf1c-193">A példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Teradata és egy "időbélyeg" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="baf1c-193">The sample assumes you have created a table “MyTable” in Teradata and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="baf1c-194">"External" beállítása: igaz, hogy a tábla az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák arról tájékoztatja a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="baf1c-194">Setting “external”: true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="baf1c-195">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="baf1c-195">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="baf1c-196">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="baf1c-196">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="baf1c-197">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="baf1c-197">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="baf1c-198">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="baf1c-198">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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
<span data-ttu-id="baf1c-199">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="baf1c-199">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="baf1c-200">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="baf1c-200">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="baf1c-201">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="baf1c-201">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="baf1c-202">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="baf1c-202">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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
## <a name="type-mapping-for-teradata"></a><span data-ttu-id="baf1c-203">Típusleképezés a teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="baf1c-203">Type mapping for Teradata</span></span>
<span data-ttu-id="baf1c-204">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során hajtja végre a módszert használja a következő 2. lépés típusok gyűjtése eseményforrás-típusnak automatikus típuskonverziók:</span><span class="sxs-lookup"><span data-stu-id="baf1c-204">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="baf1c-205">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="baf1c-205">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="baf1c-206">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="baf1c-206">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="baf1c-207">Adatok áthelyezése a teradata rendszerhez, amikor a következő leképezéseit segítségével Teradata típusból .NET-típusa.</span><span class="sxs-lookup"><span data-stu-id="baf1c-207">When moving data to Teradata, the following mappings are used from Teradata type to .NET type.</span></span>

| <span data-ttu-id="baf1c-208">Teradata-adatbázishoz típusa</span><span class="sxs-lookup"><span data-stu-id="baf1c-208">Teradata Database type</span></span> | <span data-ttu-id="baf1c-209">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="baf1c-209">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="baf1c-210">Karakter</span><span class="sxs-lookup"><span data-stu-id="baf1c-210">Char</span></span> |<span data-ttu-id="baf1c-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-211">String</span></span> |
| <span data-ttu-id="baf1c-212">CLOB</span><span class="sxs-lookup"><span data-stu-id="baf1c-212">Clob</span></span> |<span data-ttu-id="baf1c-213">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-213">String</span></span> |
| <span data-ttu-id="baf1c-214">Kép</span><span class="sxs-lookup"><span data-stu-id="baf1c-214">Graphic</span></span> |<span data-ttu-id="baf1c-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-215">String</span></span> |
| <span data-ttu-id="baf1c-216">VarChar</span><span class="sxs-lookup"><span data-stu-id="baf1c-216">VarChar</span></span> |<span data-ttu-id="baf1c-217">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-217">String</span></span> |
| <span data-ttu-id="baf1c-218">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="baf1c-218">VarGraphic</span></span> |<span data-ttu-id="baf1c-219">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-219">String</span></span> |
| <span data-ttu-id="baf1c-220">Blob</span><span class="sxs-lookup"><span data-stu-id="baf1c-220">Blob</span></span> |<span data-ttu-id="baf1c-221">Byte]</span><span class="sxs-lookup"><span data-stu-id="baf1c-221">Byte[]</span></span> |
| <span data-ttu-id="baf1c-222">Bájt</span><span class="sxs-lookup"><span data-stu-id="baf1c-222">Byte</span></span> |<span data-ttu-id="baf1c-223">Byte]</span><span class="sxs-lookup"><span data-stu-id="baf1c-223">Byte[]</span></span> |
| <span data-ttu-id="baf1c-224">VarByte</span><span class="sxs-lookup"><span data-stu-id="baf1c-224">VarByte</span></span> |<span data-ttu-id="baf1c-225">Byte]</span><span class="sxs-lookup"><span data-stu-id="baf1c-225">Byte[]</span></span> |
| <span data-ttu-id="baf1c-226">BigInt</span><span class="sxs-lookup"><span data-stu-id="baf1c-226">BigInt</span></span> |<span data-ttu-id="baf1c-227">Int64</span><span class="sxs-lookup"><span data-stu-id="baf1c-227">Int64</span></span> |
| <span data-ttu-id="baf1c-228">ByteInt</span><span class="sxs-lookup"><span data-stu-id="baf1c-228">ByteInt</span></span> |<span data-ttu-id="baf1c-229">Int16</span><span class="sxs-lookup"><span data-stu-id="baf1c-229">Int16</span></span> |
| <span data-ttu-id="baf1c-230">Decimális</span><span class="sxs-lookup"><span data-stu-id="baf1c-230">Decimal</span></span> |<span data-ttu-id="baf1c-231">Decimális</span><span class="sxs-lookup"><span data-stu-id="baf1c-231">Decimal</span></span> |
| <span data-ttu-id="baf1c-232">Dupla</span><span class="sxs-lookup"><span data-stu-id="baf1c-232">Double</span></span> |<span data-ttu-id="baf1c-233">Dupla</span><span class="sxs-lookup"><span data-stu-id="baf1c-233">Double</span></span> |
| <span data-ttu-id="baf1c-234">Egész szám</span><span class="sxs-lookup"><span data-stu-id="baf1c-234">Integer</span></span> |<span data-ttu-id="baf1c-235">Int32</span><span class="sxs-lookup"><span data-stu-id="baf1c-235">Int32</span></span> |
| <span data-ttu-id="baf1c-236">Szám</span><span class="sxs-lookup"><span data-stu-id="baf1c-236">Number</span></span> |<span data-ttu-id="baf1c-237">Dupla</span><span class="sxs-lookup"><span data-stu-id="baf1c-237">Double</span></span> |
| <span data-ttu-id="baf1c-238">SmallInt</span><span class="sxs-lookup"><span data-stu-id="baf1c-238">SmallInt</span></span> |<span data-ttu-id="baf1c-239">Int16</span><span class="sxs-lookup"><span data-stu-id="baf1c-239">Int16</span></span> |
| <span data-ttu-id="baf1c-240">Dátum</span><span class="sxs-lookup"><span data-stu-id="baf1c-240">Date</span></span> |<span data-ttu-id="baf1c-241">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="baf1c-241">DateTime</span></span> |
| <span data-ttu-id="baf1c-242">Time</span><span class="sxs-lookup"><span data-stu-id="baf1c-242">Time</span></span> |<span data-ttu-id="baf1c-243">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-243">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-244">Időzóna idő</span><span class="sxs-lookup"><span data-stu-id="baf1c-244">Time With Time Zone</span></span> |<span data-ttu-id="baf1c-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-245">String</span></span> |
| <span data-ttu-id="baf1c-246">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="baf1c-246">Timestamp</span></span> |<span data-ttu-id="baf1c-247">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="baf1c-247">DateTime</span></span> |
| <span data-ttu-id="baf1c-248">Az időzóna időbélyeg</span><span class="sxs-lookup"><span data-stu-id="baf1c-248">Timestamp With Time Zone</span></span> |<span data-ttu-id="baf1c-249">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="baf1c-249">DateTimeOffset</span></span> |
| <span data-ttu-id="baf1c-250">Időköz nap</span><span class="sxs-lookup"><span data-stu-id="baf1c-250">Interval Day</span></span> |<span data-ttu-id="baf1c-251">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-251">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-252">Időköz nap, óra</span><span class="sxs-lookup"><span data-stu-id="baf1c-252">Interval Day To Hour</span></span> |<span data-ttu-id="baf1c-253">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-253">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-254">Naponta időköz percben</span><span class="sxs-lookup"><span data-stu-id="baf1c-254">Interval Day To Minute</span></span> |<span data-ttu-id="baf1c-255">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-255">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-256">Második intervallum naponta</span><span class="sxs-lookup"><span data-stu-id="baf1c-256">Interval Day To Second</span></span> |<span data-ttu-id="baf1c-257">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-257">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-258">Időköz óra</span><span class="sxs-lookup"><span data-stu-id="baf1c-258">Interval Hour</span></span> |<span data-ttu-id="baf1c-259">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-259">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-260">Időköz óra, perc alatt</span><span class="sxs-lookup"><span data-stu-id="baf1c-260">Interval Hour To Minute</span></span> |<span data-ttu-id="baf1c-261">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-261">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-262">Második intervallum óra</span><span class="sxs-lookup"><span data-stu-id="baf1c-262">Interval Hour To Second</span></span> |<span data-ttu-id="baf1c-263">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-263">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-264">Időköz percben</span><span class="sxs-lookup"><span data-stu-id="baf1c-264">Interval Minute</span></span> |<span data-ttu-id="baf1c-265">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-265">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-266">Másik időköz percben</span><span class="sxs-lookup"><span data-stu-id="baf1c-266">Interval Minute To Second</span></span> |<span data-ttu-id="baf1c-267">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-267">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-268">Időköz második</span><span class="sxs-lookup"><span data-stu-id="baf1c-268">Interval Second</span></span> |<span data-ttu-id="baf1c-269">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="baf1c-269">TimeSpan</span></span> |
| <span data-ttu-id="baf1c-270">Időköz év</span><span class="sxs-lookup"><span data-stu-id="baf1c-270">Interval Year</span></span> |<span data-ttu-id="baf1c-271">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-271">String</span></span> |
| <span data-ttu-id="baf1c-272">Időköz hónap év</span><span class="sxs-lookup"><span data-stu-id="baf1c-272">Interval Year To Month</span></span> |<span data-ttu-id="baf1c-273">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-273">String</span></span> |
| <span data-ttu-id="baf1c-274">Időköz hónap</span><span class="sxs-lookup"><span data-stu-id="baf1c-274">Interval Month</span></span> |<span data-ttu-id="baf1c-275">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-275">String</span></span> |
| <span data-ttu-id="baf1c-276">Period(date)</span><span class="sxs-lookup"><span data-stu-id="baf1c-276">Period(Date)</span></span> |<span data-ttu-id="baf1c-277">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-277">String</span></span> |
| <span data-ttu-id="baf1c-278">Period(Time)</span><span class="sxs-lookup"><span data-stu-id="baf1c-278">Period(Time)</span></span> |<span data-ttu-id="baf1c-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-279">String</span></span> |
| <span data-ttu-id="baf1c-280">Időtartam (idő időzóna)</span><span class="sxs-lookup"><span data-stu-id="baf1c-280">Period(Time With Time Zone)</span></span> |<span data-ttu-id="baf1c-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-281">String</span></span> |
| <span data-ttu-id="baf1c-282">Period(Timestamp)</span><span class="sxs-lookup"><span data-stu-id="baf1c-282">Period(Timestamp)</span></span> |<span data-ttu-id="baf1c-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-283">String</span></span> |
| <span data-ttu-id="baf1c-284">Időtartam (időbélyegzője az időzóna)</span><span class="sxs-lookup"><span data-stu-id="baf1c-284">Period(Timestamp With Time Zone)</span></span> |<span data-ttu-id="baf1c-285">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-285">String</span></span> |
| <span data-ttu-id="baf1c-286">XML</span><span class="sxs-lookup"><span data-stu-id="baf1c-286">Xml</span></span> |<span data-ttu-id="baf1c-287">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="baf1c-287">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="baf1c-288">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="baf1c-288">Map source to sink columns</span></span>
<span data-ttu-id="baf1c-289">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="baf1c-289">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="baf1c-290">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="baf1c-290">Repeatable read from relational sources</span></span>
<span data-ttu-id="baf1c-291">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="baf1c-291">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="baf1c-292">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="baf1c-292">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="baf1c-293">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="baf1c-293">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="baf1c-294">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="baf1c-294">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="baf1c-295">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="baf1c-295">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="baf1c-296">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="baf1c-296">Performance and Tuning</span></span>
<span data-ttu-id="baf1c-297">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="baf1c-297">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
