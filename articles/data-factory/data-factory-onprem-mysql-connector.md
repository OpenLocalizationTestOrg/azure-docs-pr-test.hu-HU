---
title: "Adatok áthelyezése az Azure Data Factory használatával MySQL |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával MySQL-adatbázisból."
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
ms.openlocfilehash: 05159bfd98977d0b57b43fbc02e4579439f7ce4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="e0276-103">Helyezze át az adatokat a MySQL Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="e0276-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="e0276-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e0276-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises MySQL database.</span></span> <span data-ttu-id="e0276-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="e0276-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="e0276-106">Egy helyszíni MySQL adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="e0276-106">You can copy data from an on-premises MySQL data store to any supported sink data store.</span></span> <span data-ttu-id="e0276-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="e0276-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="e0276-108">Adat-előállító jelenleg a mozgóátlag adatok kizárólag egy MySQL adattárból egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz MySQL adattárolóihoz támogatja.</span><span class="sxs-lookup"><span data-stu-id="e0276-108">Data factory currently supports only moving data from a MySQL data store to other data stores, but not for moving data from other data stores to an MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e0276-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e0276-109">Prerequisites</span></span>
<span data-ttu-id="e0276-110">Data Factory szolgáltatásnak a helyszíni MySQL adatforrások az adatkezelési átjáró használatával történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="e0276-110">Data Factory service supports connecting to on-premises MySQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="e0276-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="e0276-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="e0276-112">Átjáróra szükség, akkor is, ha a MySQL-adatbázis egy Azure IaaS virtuális gépre (VM).</span><span class="sxs-lookup"><span data-stu-id="e0276-112">Gateway is required even if the MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="e0276-113">Az átjárót telepítheti a adattárként azonos virtuális Gépen vagy a másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e0276-113">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="e0276-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="e0276-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="e0276-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="e0276-115">Supported versions and installation</span></span>
<span data-ttu-id="e0276-116">Az adatkezelési átjáró a MySQL-adatbázishoz való kapcsolódáshoz, telepítenie kell a [MySQL összekötő/Net számára a Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (6.6.5 verzió vagy újabb) az adatkezelési átjáró ugyanazon a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="e0276-116">For Data Management Gateway to connect to the MySQL Database, you need to install the [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="e0276-117">5.1-es verzió MySQL és az újabb verzió esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="e0276-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="e0276-118">Kattintson a "Hitelesítés sikertelen, mert a távoli fél bezárta az átviteli adatfolyamot." hibaüzenet, ha fontolja meg a MySQL-összekötő/Net frissítése újabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="e0276-118">If you hit error on "Authentication failed because the remote party has closed the transport stream.", consider to upgrade the MySQL Connector/Net to higher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e0276-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e0276-119">Getting started</span></span>
<span data-ttu-id="e0276-120">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="e0276-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="e0276-121">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="e0276-121">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="e0276-122">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="e0276-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="e0276-123">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="e0276-123">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e0276-124">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="e0276-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e0276-125">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="e0276-125">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="e0276-126">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="e0276-126">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="e0276-127">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="e0276-127">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="e0276-128">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="e0276-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="e0276-129">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="e0276-129">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="e0276-130">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="e0276-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="e0276-131">Adatok másolása egy helyszíni MySQL adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MySQL az Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="e0276-131">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL to Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="e0276-132">A következő szakaszok részletesen bemutatják a MySQL-tárolóban való adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="e0276-132">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="e0276-133">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="e0276-133">Linked service properties</span></span>
<span data-ttu-id="e0276-134">A következő táblázat a JSON-elemek szerepelnek MySQL kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="e0276-134">The following table provides description for JSON elements specific to MySQL linked service.</span></span>

| <span data-ttu-id="e0276-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e0276-135">Property</span></span> | <span data-ttu-id="e0276-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="e0276-136">Description</span></span> | <span data-ttu-id="e0276-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e0276-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0276-138">type</span><span class="sxs-lookup"><span data-stu-id="e0276-138">type</span></span> |<span data-ttu-id="e0276-139">A type tulajdonságot kell beállítani: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="e0276-139">The type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="e0276-140">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-140">Yes</span></span> |
| <span data-ttu-id="e0276-141">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="e0276-141">server</span></span> |<span data-ttu-id="e0276-142">A MySQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="e0276-142">Name of the MySQL server.</span></span> |<span data-ttu-id="e0276-143">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-143">Yes</span></span> |
| <span data-ttu-id="e0276-144">adatbázis</span><span class="sxs-lookup"><span data-stu-id="e0276-144">database</span></span> |<span data-ttu-id="e0276-145">A MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="e0276-145">Name of the MySQL database.</span></span> |<span data-ttu-id="e0276-146">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-146">Yes</span></span> |
| <span data-ttu-id="e0276-147">Séma</span><span class="sxs-lookup"><span data-stu-id="e0276-147">schema</span></span> |<span data-ttu-id="e0276-148">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="e0276-148">Name of the schema in the database.</span></span> |<span data-ttu-id="e0276-149">Nem</span><span class="sxs-lookup"><span data-stu-id="e0276-149">No</span></span> |
| <span data-ttu-id="e0276-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="e0276-150">authenticationType</span></span> |<span data-ttu-id="e0276-151">A MySQL-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e0276-151">Type of authentication used to connect to the MySQL database.</span></span> <span data-ttu-id="e0276-152">Lehetséges értékek a következők: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="e0276-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="e0276-153">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-153">Yes</span></span> |
| <span data-ttu-id="e0276-154">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="e0276-154">username</span></span> |<span data-ttu-id="e0276-155">Adja meg a felhasználónevet a MySQL-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="e0276-155">Specify user name to connect to the MySQL database.</span></span> |<span data-ttu-id="e0276-156">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-156">Yes</span></span> |
| <span data-ttu-id="e0276-157">jelszó</span><span class="sxs-lookup"><span data-stu-id="e0276-157">password</span></span> |<span data-ttu-id="e0276-158">Adja meg a megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e0276-158">Specify password for the user account you specified.</span></span> |<span data-ttu-id="e0276-159">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-159">Yes</span></span> |
| <span data-ttu-id="e0276-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="e0276-160">gatewayName</span></span> |<span data-ttu-id="e0276-161">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia való kapcsolódáshoz a helyszíni MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="e0276-161">Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database.</span></span> |<span data-ttu-id="e0276-162">Igen</span><span class="sxs-lookup"><span data-stu-id="e0276-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="e0276-163">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e0276-163">Dataset properties</span></span>
<span data-ttu-id="e0276-164">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e0276-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e0276-165">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="e0276-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e0276-166">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="e0276-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="e0276-167">A typeProperties szakasz típusú adatkészlet **RelationalTable** (amely tartalmazza a MySQL dataset) a következő tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e0276-167">The typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has the following properties</span></span>

| <span data-ttu-id="e0276-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e0276-168">Property</span></span> | <span data-ttu-id="e0276-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="e0276-169">Description</span></span> | <span data-ttu-id="e0276-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e0276-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0276-171">tableName</span><span class="sxs-lookup"><span data-stu-id="e0276-171">tableName</span></span> |<span data-ttu-id="e0276-172">A tábla a MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e0276-172">Name of the table in the MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="e0276-173">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="e0276-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e0276-174">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e0276-174">Copy activity properties</span></span>
<span data-ttu-id="e0276-175">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e0276-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e0276-176">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e0276-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="e0276-177">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="e0276-177">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="e0276-178">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="e0276-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="e0276-179">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a MySQL), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="e0276-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="e0276-180">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e0276-180">Property</span></span> | <span data-ttu-id="e0276-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="e0276-181">Description</span></span> | <span data-ttu-id="e0276-182">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="e0276-182">Allowed values</span></span> | <span data-ttu-id="e0276-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e0276-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e0276-184">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e0276-184">query</span></span> |<span data-ttu-id="e0276-185">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="e0276-185">Use the custom query to read data.</span></span> |<span data-ttu-id="e0276-186">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e0276-186">SQL query string.</span></span> <span data-ttu-id="e0276-187">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="e0276-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="e0276-188">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="e0276-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a><span data-ttu-id="e0276-189">JSON-példa: adatok másolása az MySQL az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="e0276-189">JSON example: Copy data from MySQL to Azure Blob</span></span>
<span data-ttu-id="e0276-190">Ebben a példában a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e0276-190">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="e0276-191">Azt illusztrálja, hogyan adatok másolása egy helyszíni MySQL-adatbázis egy Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e0276-191">It shows how to copy data from an on-premises MySQL database to an Azure Blob Storage.</span></span> <span data-ttu-id="e0276-192">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="e0276-192">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e0276-193">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="e0276-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="e0276-194">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="e0276-194">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="e0276-195">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="e0276-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="e0276-196">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e0276-196">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="e0276-197">A társított szolgáltatás típusa [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e0276-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="e0276-198">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e0276-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e0276-199">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e0276-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="e0276-200">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e0276-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e0276-201">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e0276-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="e0276-202">A minta másol adatokat a MySQL-adatbázis egy lekérdezés eredményét blob óránként.</span><span class="sxs-lookup"><span data-stu-id="e0276-202">The sample copies data from a query result in MySQL database to a blob hourly.</span></span> <span data-ttu-id="e0276-203">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e0276-203">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="e0276-204">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="e0276-204">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="e0276-205">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e0276-205">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="e0276-206">**MySQL társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="e0276-206">**MySQL linked service:**</span></span>

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

<span data-ttu-id="e0276-207">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="e0276-207">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="e0276-208">**MySQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e0276-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="e0276-209">A példa azt feltételezi, hogy létrehozott egy tábla "MyTable" MySQL és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e0276-209">The sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="e0276-210">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e0276-210">Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="e0276-211">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e0276-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="e0276-212">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e0276-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e0276-213">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="e0276-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="e0276-214">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="e0276-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="e0276-215">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="e0276-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="e0276-216">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="e0276-216">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="e0276-217">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e0276-217">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="e0276-218">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="e0276-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="e0276-219">A MySQL leképezésének</span><span class="sxs-lookup"><span data-stu-id="e0276-219">Type mapping for MySQL</span></span>
<span data-ttu-id="e0276-220">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="e0276-220">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="e0276-221">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="e0276-221">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="e0276-222">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="e0276-222">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="e0276-223">Ha az adatok áthelyezése a MySQL, .NET típusú a következő megfeleltetéseket használtak MySQL típusok.</span><span class="sxs-lookup"><span data-stu-id="e0276-223">When moving data to MySQL, the following mappings are used from MySQL types to .NET types.</span></span>

| <span data-ttu-id="e0276-224">A MySQL-adatbázis típusa</span><span class="sxs-lookup"><span data-stu-id="e0276-224">MySQL Database type</span></span> | <span data-ttu-id="e0276-225">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="e0276-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="e0276-226">aláíratlan bigint</span><span class="sxs-lookup"><span data-stu-id="e0276-226">bigint unsigned</span></span> |<span data-ttu-id="e0276-227">Decimális</span><span class="sxs-lookup"><span data-stu-id="e0276-227">Decimal</span></span> |
| <span data-ttu-id="e0276-228">bigint</span><span class="sxs-lookup"><span data-stu-id="e0276-228">bigint</span></span> |<span data-ttu-id="e0276-229">Int64</span><span class="sxs-lookup"><span data-stu-id="e0276-229">Int64</span></span> |
| <span data-ttu-id="e0276-230">bit</span><span class="sxs-lookup"><span data-stu-id="e0276-230">bit</span></span> |<span data-ttu-id="e0276-231">Decimális</span><span class="sxs-lookup"><span data-stu-id="e0276-231">Decimal</span></span> |
| <span data-ttu-id="e0276-232">A BLOB</span><span class="sxs-lookup"><span data-stu-id="e0276-232">blob</span></span> |<span data-ttu-id="e0276-233">Byte]</span><span class="sxs-lookup"><span data-stu-id="e0276-233">Byte[]</span></span> |
| <span data-ttu-id="e0276-234">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e0276-234">bool</span></span> |<span data-ttu-id="e0276-235">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="e0276-235">Boolean</span></span> |
| <span data-ttu-id="e0276-236">Karakter</span><span class="sxs-lookup"><span data-stu-id="e0276-236">char</span></span> |<span data-ttu-id="e0276-237">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-237">String</span></span> |
| <span data-ttu-id="e0276-238">Dátum</span><span class="sxs-lookup"><span data-stu-id="e0276-238">date</span></span> |<span data-ttu-id="e0276-239">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="e0276-239">Datetime</span></span> |
| <span data-ttu-id="e0276-240">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="e0276-240">datetime</span></span> |<span data-ttu-id="e0276-241">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="e0276-241">Datetime</span></span> |
| <span data-ttu-id="e0276-242">Decimális</span><span class="sxs-lookup"><span data-stu-id="e0276-242">decimal</span></span> |<span data-ttu-id="e0276-243">Decimális</span><span class="sxs-lookup"><span data-stu-id="e0276-243">Decimal</span></span> |
| <span data-ttu-id="e0276-244">a kétszeres pontosság</span><span class="sxs-lookup"><span data-stu-id="e0276-244">double precision</span></span> |<span data-ttu-id="e0276-245">Dupla</span><span class="sxs-lookup"><span data-stu-id="e0276-245">Double</span></span> |
| <span data-ttu-id="e0276-246">Dupla</span><span class="sxs-lookup"><span data-stu-id="e0276-246">double</span></span> |<span data-ttu-id="e0276-247">Dupla</span><span class="sxs-lookup"><span data-stu-id="e0276-247">Double</span></span> |
| <span data-ttu-id="e0276-248">Enum</span><span class="sxs-lookup"><span data-stu-id="e0276-248">enum</span></span> |<span data-ttu-id="e0276-249">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-249">String</span></span> |
| <span data-ttu-id="e0276-250">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="e0276-250">float</span></span> |<span data-ttu-id="e0276-251">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="e0276-251">Single</span></span> |
| <span data-ttu-id="e0276-252">aláíratlan int</span><span class="sxs-lookup"><span data-stu-id="e0276-252">int unsigned</span></span> |<span data-ttu-id="e0276-253">Int64</span><span class="sxs-lookup"><span data-stu-id="e0276-253">Int64</span></span> |
| <span data-ttu-id="e0276-254">int</span><span class="sxs-lookup"><span data-stu-id="e0276-254">int</span></span> |<span data-ttu-id="e0276-255">Int32</span><span class="sxs-lookup"><span data-stu-id="e0276-255">Int32</span></span> |
| <span data-ttu-id="e0276-256">aláíratlan egész szám</span><span class="sxs-lookup"><span data-stu-id="e0276-256">integer unsigned</span></span> |<span data-ttu-id="e0276-257">Int64</span><span class="sxs-lookup"><span data-stu-id="e0276-257">Int64</span></span> |
| <span data-ttu-id="e0276-258">egész szám</span><span class="sxs-lookup"><span data-stu-id="e0276-258">integer</span></span> |<span data-ttu-id="e0276-259">Int32</span><span class="sxs-lookup"><span data-stu-id="e0276-259">Int32</span></span> |
| <span data-ttu-id="e0276-260">hosszú varbinary</span><span class="sxs-lookup"><span data-stu-id="e0276-260">long varbinary</span></span> |<span data-ttu-id="e0276-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="e0276-261">Byte[]</span></span> |
| <span data-ttu-id="e0276-262">hosszú varchar</span><span class="sxs-lookup"><span data-stu-id="e0276-262">long varchar</span></span> |<span data-ttu-id="e0276-263">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-263">String</span></span> |
| <span data-ttu-id="e0276-264">longblob</span><span class="sxs-lookup"><span data-stu-id="e0276-264">longblob</span></span> |<span data-ttu-id="e0276-265">Byte]</span><span class="sxs-lookup"><span data-stu-id="e0276-265">Byte[]</span></span> |
| <span data-ttu-id="e0276-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="e0276-266">longtext</span></span> |<span data-ttu-id="e0276-267">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-267">String</span></span> |
| <span data-ttu-id="e0276-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="e0276-268">mediumblob</span></span> |<span data-ttu-id="e0276-269">Byte]</span><span class="sxs-lookup"><span data-stu-id="e0276-269">Byte[]</span></span> |
| <span data-ttu-id="e0276-270">aláíratlan mediumint</span><span class="sxs-lookup"><span data-stu-id="e0276-270">mediumint unsigned</span></span> |<span data-ttu-id="e0276-271">Int64</span><span class="sxs-lookup"><span data-stu-id="e0276-271">Int64</span></span> |
| <span data-ttu-id="e0276-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="e0276-272">mediumint</span></span> |<span data-ttu-id="e0276-273">Int32</span><span class="sxs-lookup"><span data-stu-id="e0276-273">Int32</span></span> |
| <span data-ttu-id="e0276-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="e0276-274">mediumtext</span></span> |<span data-ttu-id="e0276-275">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-275">String</span></span> |
| <span data-ttu-id="e0276-276">Numerikus</span><span class="sxs-lookup"><span data-stu-id="e0276-276">numeric</span></span> |<span data-ttu-id="e0276-277">Decimális</span><span class="sxs-lookup"><span data-stu-id="e0276-277">Decimal</span></span> |
| <span data-ttu-id="e0276-278">valós</span><span class="sxs-lookup"><span data-stu-id="e0276-278">real</span></span> |<span data-ttu-id="e0276-279">Dupla</span><span class="sxs-lookup"><span data-stu-id="e0276-279">Double</span></span> |
| <span data-ttu-id="e0276-280">Állítsa be</span><span class="sxs-lookup"><span data-stu-id="e0276-280">set</span></span> |<span data-ttu-id="e0276-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-281">String</span></span> |
| <span data-ttu-id="e0276-282">aláíratlan smallint</span><span class="sxs-lookup"><span data-stu-id="e0276-282">smallint unsigned</span></span> |<span data-ttu-id="e0276-283">Int32</span><span class="sxs-lookup"><span data-stu-id="e0276-283">Int32</span></span> |
| <span data-ttu-id="e0276-284">smallint</span><span class="sxs-lookup"><span data-stu-id="e0276-284">smallint</span></span> |<span data-ttu-id="e0276-285">Int16</span><span class="sxs-lookup"><span data-stu-id="e0276-285">Int16</span></span> |
| <span data-ttu-id="e0276-286">Szöveg</span><span class="sxs-lookup"><span data-stu-id="e0276-286">text</span></span> |<span data-ttu-id="e0276-287">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-287">String</span></span> |
| <span data-ttu-id="e0276-288">time</span><span class="sxs-lookup"><span data-stu-id="e0276-288">time</span></span> |<span data-ttu-id="e0276-289">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e0276-289">TimeSpan</span></span> |
| <span data-ttu-id="e0276-290">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="e0276-290">timestamp</span></span> |<span data-ttu-id="e0276-291">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="e0276-291">Datetime</span></span> |
| <span data-ttu-id="e0276-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="e0276-292">tinyblob</span></span> |<span data-ttu-id="e0276-293">Byte]</span><span class="sxs-lookup"><span data-stu-id="e0276-293">Byte[]</span></span> |
| <span data-ttu-id="e0276-294">aláíratlan tinyint</span><span class="sxs-lookup"><span data-stu-id="e0276-294">tinyint unsigned</span></span> |<span data-ttu-id="e0276-295">Int16</span><span class="sxs-lookup"><span data-stu-id="e0276-295">Int16</span></span> |
| <span data-ttu-id="e0276-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="e0276-296">tinyint</span></span> |<span data-ttu-id="e0276-297">Int16</span><span class="sxs-lookup"><span data-stu-id="e0276-297">Int16</span></span> |
| <span data-ttu-id="e0276-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="e0276-298">tinytext</span></span> |<span data-ttu-id="e0276-299">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-299">String</span></span> |
| <span data-ttu-id="e0276-300">varchar</span><span class="sxs-lookup"><span data-stu-id="e0276-300">varchar</span></span> |<span data-ttu-id="e0276-301">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e0276-301">String</span></span> |
| <span data-ttu-id="e0276-302">Év</span><span class="sxs-lookup"><span data-stu-id="e0276-302">year</span></span> |<span data-ttu-id="e0276-303">int</span><span class="sxs-lookup"><span data-stu-id="e0276-303">Int</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="e0276-304">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="e0276-304">Map source to sink columns</span></span>
<span data-ttu-id="e0276-305">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e0276-305">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e0276-306">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="e0276-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="e0276-307">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e0276-307">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="e0276-308">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="e0276-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e0276-309">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="e0276-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e0276-310">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="e0276-310">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e0276-311">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="e0276-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e0276-312">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="e0276-312">Performance and Tuning</span></span>
<span data-ttu-id="e0276-313">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="e0276-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
