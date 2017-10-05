---
title: "Adatok áthelyezése a Data Factory használatával Cassandra |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával a helyszíni Cassandra adatbázisból."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: f2b225bdbdf2880d26a6ab5f992301bf0a804b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="553a9-103">Adatok áthelyezése az Azure Data Factory használatával a helyszíni Cassandra adatbázisból</span><span class="sxs-lookup"><span data-stu-id="553a9-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="553a9-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory áthelyezni az adatokat a helyszíni Cassandra adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="553a9-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Cassandra database.</span></span> <span data-ttu-id="553a9-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="553a9-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="553a9-106">Egy helyszíni Cassandra adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="553a9-106">You can copy data from an on-premises Cassandra data store to any supported sink data store.</span></span> <span data-ttu-id="553a9-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="553a9-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="553a9-108">Adat-előállító jelenleg mozgási adatok kizárólag egy Cassandra adattárból egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz Cassandra adattárat.</span><span class="sxs-lookup"><span data-stu-id="553a9-108">Data factory currently supports only moving data from a Cassandra data store to other data stores, but not for moving data from other data stores to a Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="553a9-109">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="553a9-109">Supported versions</span></span>
<span data-ttu-id="553a9-110">A Cassandra összekötő Cassandra a következő verzióit támogatja: 2.X.</span><span class="sxs-lookup"><span data-stu-id="553a9-110">The Cassandra connector supports the following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="553a9-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="553a9-111">Prerequisites</span></span>
<span data-ttu-id="553a9-112">A helyszíni Cassandra adatbázishoz csatlakozni az Azure Data Factory szolgáltatás telepítenie kell az adatkezelési átjáró ugyanazon a számítógépen, amelyen az adatbázis vagy egy külön számítógépen elkerülésére használják a források az adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="553a9-112">For the Azure Data Factory service to be able to connect to your on-premises Cassandra database, you must install a Data Management Gateway on the same machine that hosts the database or on a separate machine to avoid competing for resources with the database.</span></span> <span data-ttu-id="553a9-113">Az adatkezelési átjáró összetevő, amely a helyszíni adatforrások felhőszolgáltatások felügyelt és biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="553a9-113">Data Management Gateway is a component that connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="553a9-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="553a9-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="553a9-115">Lásd: [tárolt adatok mozgatása felhőbe helyszíni](data-factory-move-data-between-onprem-and-cloud.md) cikk lépésenkénti adatok folyamat az átjáró beállítása áthelyezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="553a9-115">See [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

<span data-ttu-id="553a9-116">Az átjáró adatbázishoz való kapcsolódáshoz a Cassandra akkor is, ha az adatbázis egy a felhőben, például egy Azure IaaS virtuális gépen kell használnia.</span><span class="sxs-lookup"><span data-stu-id="553a9-116">You must use the gateway to connect to a Cassandra database even if the database is hosted in the cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="553a9-117">Y is van az átjáró az azonos virtuális gépen, amelyen az adatbázist, vagy egy külön virtuális gépre mindaddig az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="553a9-117">Y You can have the gateway on the same VM that hosts the database or on a separate VM as long as the gateway can connect to the database.</span></span>  

<span data-ttu-id="553a9-118">Az átjáró telepítésekor automatikusan telepíti a Microsoft Cassandra ODBC-illesztőprogram Cassandra adatbázishoz való kapcsolódáshoz használt.</span><span class="sxs-lookup"><span data-stu-id="553a9-118">When you install the gateway, it automatically installs a Microsoft Cassandra ODBC driver used to connect to Cassandra database.</span></span> <span data-ttu-id="553a9-119">Emiatt nem kell manuálisan telepítenie minden olyan illesztőprogram az átjáró számítógépe a Cassandra adatbázisból származó adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="553a9-119">Therefore, you don't need to manually install any driver on the gateway machine when copying data from the Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="553a9-120">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="553a9-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="553a9-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="553a9-121">Getting started</span></span>
<span data-ttu-id="553a9-122">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="553a9-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="553a9-123">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="553a9-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="553a9-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="553a9-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="553a9-125">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="553a9-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="553a9-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="553a9-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="553a9-127">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="553a9-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="553a9-128">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="553a9-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="553a9-129">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="553a9-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="553a9-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="553a9-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="553a9-131">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="553a9-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="553a9-132">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="553a9-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="553a9-133">Adatok másolása egy helyszíni Cassandra adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Cassandra az Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="553a9-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra to Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="553a9-134">A következő szakaszok részletesen bemutatják való Cassandra adattároló adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="553a9-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="553a9-135">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="553a9-135">Linked service properties</span></span>
<span data-ttu-id="553a9-136">A következő táblázat a JSON-elemek szerepelnek Cassandra kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="553a9-136">The following table provides description for JSON elements specific to Cassandra linked service.</span></span>

| <span data-ttu-id="553a9-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="553a9-137">Property</span></span> | <span data-ttu-id="553a9-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="553a9-138">Description</span></span> | <span data-ttu-id="553a9-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="553a9-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="553a9-140">type</span><span class="sxs-lookup"><span data-stu-id="553a9-140">type</span></span> |<span data-ttu-id="553a9-141">A type tulajdonságot kell beállítani: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="553a9-141">The type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="553a9-142">Igen</span><span class="sxs-lookup"><span data-stu-id="553a9-142">Yes</span></span> |
| <span data-ttu-id="553a9-143">állomás</span><span class="sxs-lookup"><span data-stu-id="553a9-143">host</span></span> |<span data-ttu-id="553a9-144">Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.</span><span class="sxs-lookup"><span data-stu-id="553a9-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="553a9-145">IP-címek vagy állomásnevek kiszolgálókhoz való kapcsolódáshoz összes egyidejűleg vesszővel tagolt listáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="553a9-145">Specify a comma-separated list of IP addresses or host names to connect to all servers concurrently.</span></span> |<span data-ttu-id="553a9-146">Igen</span><span class="sxs-lookup"><span data-stu-id="553a9-146">Yes</span></span> |
| <span data-ttu-id="553a9-147">port</span><span class="sxs-lookup"><span data-stu-id="553a9-147">port</span></span> |<span data-ttu-id="553a9-148">A TCP-portot, amelyen a Cassandra kiszolgáló ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="553a9-148">The TCP port that the Cassandra server uses to listen for client connections.</span></span> |<span data-ttu-id="553a9-149">Nem, alapértelmezett érték: 9042</span><span class="sxs-lookup"><span data-stu-id="553a9-149">No, default value: 9042</span></span> |
| <span data-ttu-id="553a9-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="553a9-150">authenticationType</span></span> |<span data-ttu-id="553a9-151">Basic vagy Anonymous</span><span class="sxs-lookup"><span data-stu-id="553a9-151">Basic, or Anonymous</span></span> |<span data-ttu-id="553a9-152">Igen</span><span class="sxs-lookup"><span data-stu-id="553a9-152">Yes</span></span> |
| <span data-ttu-id="553a9-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="553a9-153">username</span></span> |<span data-ttu-id="553a9-154">Adja meg a felhasználói fiók felhasználónevét.</span><span class="sxs-lookup"><span data-stu-id="553a9-154">Specify user name for the user account.</span></span> |<span data-ttu-id="553a9-155">Igen, ha authenticationType beállítása alapszintű.</span><span class="sxs-lookup"><span data-stu-id="553a9-155">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="553a9-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="553a9-156">password</span></span> |<span data-ttu-id="553a9-157">Adja meg a felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="553a9-157">Specify password for the user account.</span></span> |<span data-ttu-id="553a9-158">Igen, ha authenticationType beállítása alapszintű.</span><span class="sxs-lookup"><span data-stu-id="553a9-158">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="553a9-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="553a9-159">gatewayName</span></span> |<span data-ttu-id="553a9-160">A helyszíni Cassandra adatbázishoz való csatlakozáshoz használt átjáró neve.</span><span class="sxs-lookup"><span data-stu-id="553a9-160">The name of the gateway that is used to connect to the on-premises Cassandra database.</span></span> |<span data-ttu-id="553a9-161">Igen</span><span class="sxs-lookup"><span data-stu-id="553a9-161">Yes</span></span> |
| <span data-ttu-id="553a9-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="553a9-162">encryptedCredential</span></span> |<span data-ttu-id="553a9-163">Az átjáró által titkosított hitelesítő.</span><span class="sxs-lookup"><span data-stu-id="553a9-163">Credential encrypted by the gateway.</span></span> |<span data-ttu-id="553a9-164">Nem</span><span class="sxs-lookup"><span data-stu-id="553a9-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="553a9-165">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="553a9-165">Dataset properties</span></span>
<span data-ttu-id="553a9-166">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="553a9-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="553a9-167">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="553a9-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="553a9-168">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="553a9-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="553a9-169">A typeProperties szakasz típusú adatkészlet **CassandraTable** tulajdonságai a következők</span><span class="sxs-lookup"><span data-stu-id="553a9-169">The typeProperties section for dataset of type **CassandraTable** has the following properties</span></span>

| <span data-ttu-id="553a9-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="553a9-170">Property</span></span> | <span data-ttu-id="553a9-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="553a9-171">Description</span></span> | <span data-ttu-id="553a9-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="553a9-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="553a9-173">kulcstérértesítések használatával</span><span class="sxs-lookup"><span data-stu-id="553a9-173">keyspace</span></span> |<span data-ttu-id="553a9-174">Kulcstérértesítések használatával vagy séma Cassandra adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="553a9-174">Name of the keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="553a9-175">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="553a9-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="553a9-176">tableName</span><span class="sxs-lookup"><span data-stu-id="553a9-176">tableName</span></span> |<span data-ttu-id="553a9-177">A tábla Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="553a9-177">Name of the table in Cassandra database.</span></span> |<span data-ttu-id="553a9-178">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="553a9-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="553a9-179">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="553a9-179">Copy activity properties</span></span>
<span data-ttu-id="553a9-180">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="553a9-180">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="553a9-181">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="553a9-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="553a9-182">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="553a9-182">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="553a9-183">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="553a9-183">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="553a9-184">Ha a forrás típusa van **CassandraSource**, a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="553a9-184">When source is of type **CassandraSource**, the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="553a9-185">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="553a9-185">Property</span></span> | <span data-ttu-id="553a9-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="553a9-186">Description</span></span> | <span data-ttu-id="553a9-187">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="553a9-187">Allowed values</span></span> | <span data-ttu-id="553a9-188">Szükséges</span><span class="sxs-lookup"><span data-stu-id="553a9-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="553a9-189">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="553a9-189">query</span></span> |<span data-ttu-id="553a9-190">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="553a9-190">Use the custom query to read data.</span></span> |<span data-ttu-id="553a9-191">SQL-92 vagy CQL lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="553a9-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="553a9-192">Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="553a9-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="553a9-193">SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** a lekérdezni kívánt táblázat képviseli.</span><span class="sxs-lookup"><span data-stu-id="553a9-193">When using SQL query, specify **keyspace name.table name** to represent the table you want to query.</span></span> |<span data-ttu-id="553a9-194">Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával).</span><span class="sxs-lookup"><span data-stu-id="553a9-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="553a9-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="553a9-195">consistencyLevel</span></span> |<span data-ttu-id="553a9-196">A konzisztencia szint határozza meg, hány replikák adatok visszatér az ügyfélalkalmazás egy olvasási kérést kell válaszolnia.</span><span class="sxs-lookup"><span data-stu-id="553a9-196">The consistency level specifies how many replicas must respond to a read request before returning data to the client application.</span></span> <span data-ttu-id="553a9-197">Cassandra ellenőrzi a megadott számú replikákat az adatok a olvasási kérelem teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="553a9-197">Cassandra checks the specified number of replicas for data to satisfy the read request.</span></span> |<span data-ttu-id="553a9-198">EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="553a9-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="553a9-199">Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="553a9-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="553a9-200">Nem.</span><span class="sxs-lookup"><span data-stu-id="553a9-200">No.</span></span> <span data-ttu-id="553a9-201">Alapértelmezett érték: egyet.</span><span class="sxs-lookup"><span data-stu-id="553a9-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a><span data-ttu-id="553a9-202">JSON-példa: adatok másolása az Cassandra az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="553a9-202">JSON example: Copy data from Cassandra to Azure Blob</span></span>
<span data-ttu-id="553a9-203">Ebben a példában a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="553a9-203">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="553a9-204">Azt illusztrálja, hogyan helyszíni Cassandra adatbázisból származó adatok másolása az Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="553a9-204">It shows how to copy data from an on-premises Cassandra database to an Azure Blob Storage.</span></span> <span data-ttu-id="553a9-205">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="553a9-205">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="553a9-206">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="553a9-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="553a9-207">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="553a9-207">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="553a9-208">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="553a9-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="553a9-209">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="553a9-209">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="553a9-210">A társított szolgáltatás típusa [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="553a9-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="553a9-211">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="553a9-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="553a9-212">Bemeneti [dataset](data-factory-create-datasets.md) típusú [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="553a9-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="553a9-213">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="553a9-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="553a9-214">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [CassandraSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="553a9-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="553a9-215">**Cassandra társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="553a9-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="553a9-216">Ez a példa a **Cassandra** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="553a9-216">This example uses the **Cassandra** linked service.</span></span> <span data-ttu-id="553a9-217">Lásd: [Cassandra társított szolgáltatás](#linked-service-properties) szakasz szolgáltatásnak által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="553a9-217">See [Cassandra linked service](#linked-service-properties) section for the properties supported by this linked service.</span></span>  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="553a9-218">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="553a9-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="553a9-219">**Cassandra bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="553a9-219">**Cassandra input dataset:**</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
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

<span data-ttu-id="553a9-220">Beállítás **külső** való **igaz** tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="553a9-220">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="553a9-221">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="553a9-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="553a9-222">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="553a9-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="553a9-223">**Másolási tevékenység Cassandra és Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="553a9-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="553a9-224">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="553a9-224">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="553a9-225">Az adatcsatorna JSON-definícióból a **forrás** típusúra **CassandraSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="553a9-225">In the pipeline JSON definition, the **source** type is set to **CassandraSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="553a9-226">Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) a RelationalSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="553a9-226">See [RelationalSource type properties](#copy-activity-properties) for the list of properties supported by the RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]    
    }
}
```

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="553a9-227">Típus identitástagok esetén Cassandra</span><span class="sxs-lookup"><span data-stu-id="553a9-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="553a9-228">Cassandra típusa</span><span class="sxs-lookup"><span data-stu-id="553a9-228">Cassandra Type</span></span> | <span data-ttu-id="553a9-229">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="553a9-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="553a9-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="553a9-230">ASCII</span></span> |<span data-ttu-id="553a9-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="553a9-231">String</span></span> |
| <span data-ttu-id="553a9-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="553a9-232">BIGINT</span></span> |<span data-ttu-id="553a9-233">Int64</span><span class="sxs-lookup"><span data-stu-id="553a9-233">Int64</span></span> |
| <span data-ttu-id="553a9-234">A BLOB</span><span class="sxs-lookup"><span data-stu-id="553a9-234">BLOB</span></span> |<span data-ttu-id="553a9-235">Byte]</span><span class="sxs-lookup"><span data-stu-id="553a9-235">Byte[]</span></span> |
| <span data-ttu-id="553a9-236">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="553a9-236">BOOLEAN</span></span> |<span data-ttu-id="553a9-237">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="553a9-237">Boolean</span></span> |
| <span data-ttu-id="553a9-238">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="553a9-238">DECIMAL</span></span> |<span data-ttu-id="553a9-239">Decimális</span><span class="sxs-lookup"><span data-stu-id="553a9-239">Decimal</span></span> |
| <span data-ttu-id="553a9-240">DUPLA</span><span class="sxs-lookup"><span data-stu-id="553a9-240">DOUBLE</span></span> |<span data-ttu-id="553a9-241">Dupla</span><span class="sxs-lookup"><span data-stu-id="553a9-241">Double</span></span> |
| <span data-ttu-id="553a9-242">LEBEGŐPONTOS</span><span class="sxs-lookup"><span data-stu-id="553a9-242">FLOAT</span></span> |<span data-ttu-id="553a9-243">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="553a9-243">Single</span></span> |
| <span data-ttu-id="553a9-244">INET</span><span class="sxs-lookup"><span data-stu-id="553a9-244">INET</span></span> |<span data-ttu-id="553a9-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="553a9-245">String</span></span> |
| <span data-ttu-id="553a9-246">INT</span><span class="sxs-lookup"><span data-stu-id="553a9-246">INT</span></span> |<span data-ttu-id="553a9-247">Int32</span><span class="sxs-lookup"><span data-stu-id="553a9-247">Int32</span></span> |
| <span data-ttu-id="553a9-248">SZÖVEG</span><span class="sxs-lookup"><span data-stu-id="553a9-248">TEXT</span></span> |<span data-ttu-id="553a9-249">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="553a9-249">String</span></span> |
| <span data-ttu-id="553a9-250">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="553a9-250">TIMESTAMP</span></span> |<span data-ttu-id="553a9-251">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="553a9-251">DateTime</span></span> |
| <span data-ttu-id="553a9-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="553a9-252">TIMEUUID</span></span> |<span data-ttu-id="553a9-253">GUID</span><span class="sxs-lookup"><span data-stu-id="553a9-253">Guid</span></span> |
| <span data-ttu-id="553a9-254">UUID</span><span class="sxs-lookup"><span data-stu-id="553a9-254">UUID</span></span> |<span data-ttu-id="553a9-255">GUID</span><span class="sxs-lookup"><span data-stu-id="553a9-255">Guid</span></span> |
| <span data-ttu-id="553a9-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="553a9-256">VARCHAR</span></span> |<span data-ttu-id="553a9-257">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="553a9-257">String</span></span> |
| <span data-ttu-id="553a9-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="553a9-258">VARINT</span></span> |<span data-ttu-id="553a9-259">Decimális</span><span class="sxs-lookup"><span data-stu-id="553a9-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="553a9-260">A gyűjtemény típusa (térkép, set, lista stb.), meg [virtuális tábla használatával Cassandra gyűjteménytípusok együttműködve](#work-with-collections-using-virtual-table) szakasz.</span><span class="sxs-lookup"><span data-stu-id="553a9-260">For collection types (map, set, list, etc.), refer to [Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="553a9-261">Felhasználó által definiált típusok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="553a9-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="553a9-262">A bináris oszlop és karakterlánc-oszlopnak hosszúságú hossza nem lehet nagyobb, mint 4000.</span><span class="sxs-lookup"><span data-stu-id="553a9-262">The length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="553a9-263">Virtuális tábla használatával gyűjtemények használata</span><span class="sxs-lookup"><span data-stu-id="553a9-263">Work with collections using virtual table</span></span>
<span data-ttu-id="553a9-264">Az Azure Data Factory beépített ODBC-illesztőprogram használatával csatlakozhat, és másolja az adatokat a Cassandra adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="553a9-264">Azure Data Factory uses a built-in ODBC driver to connect to and copy data from your Cassandra database.</span></span> <span data-ttu-id="553a9-265">A gyűjtemény típusú térkép, a készlet és a lista az illesztőprogram renormalizes az adatok megfelelő virtuális táblákba.</span><span class="sxs-lookup"><span data-stu-id="553a9-265">For collection types including map, set and list, the driver renormalizes the data into corresponding virtual tables.</span></span> <span data-ttu-id="553a9-266">Pontosabban Ha a tábla gyűjtemény oszlopot tartalmaz, az illesztőprogram állít elő, a következő virtuális táblák:</span><span class="sxs-lookup"><span data-stu-id="553a9-266">Specifically, if a table contains any collection columns, the driver generates the following virtual tables:</span></span>

* <span data-ttu-id="553a9-267">A **alaptábla**, amely tartalmazza a tényleges táblából, a gyűjtemény oszlopok ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="553a9-267">A **base table**, which contains the same data as the real table except for the collection columns.</span></span> <span data-ttu-id="553a9-268">Az alaptábla ugyanazt a nevet használja, amely valós táblázatként.</span><span class="sxs-lookup"><span data-stu-id="553a9-268">The base table uses the same name as the real table that it represents.</span></span>
* <span data-ttu-id="553a9-269">A **virtuális tábla** gyűjtemény oszlopok, amely a beágyazott adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="553a9-269">A **virtual table** for each collection column, which expands the nested data.</span></span> <span data-ttu-id="553a9-270">A virtuális táblákat, amelyek megfelelnek a gyűjtemények az alábbi néven valós tábla, egy elválasztó elnevezése "*vt*" és az oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="553a9-270">The virtual tables that represent collections are named using the name of the real table, a separator “*vt*” and the name of the column.</span></span>

<span data-ttu-id="553a9-271">Virtuális táblák tekintse meg az adatok a valós táblázatban, az illesztőprogram, a nem normalizált adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="553a9-271">Virtual tables refer to the data in the real table, enabling the driver to access the denormalized data.</span></span> <span data-ttu-id="553a9-272">Lásd például című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="553a9-272">See Example section for details.</span></span> <span data-ttu-id="553a9-273">A tartalom Cassandra gyűjtemények kérdez le, és a virtuális tábla érheti el.</span><span class="sxs-lookup"><span data-stu-id="553a9-273">You can access the content of Cassandra collections by querying and joining the virtual tables.</span></span>

<span data-ttu-id="553a9-274">Használhatja a [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuitív táblázatok listájának megtekintése a virtuális táblázatot is Cassandra adatbázisban és található adatok megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="553a9-274">You can use the [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) to intuitively view the list of tables in Cassandra database including the virtual tables, and preview the data inside.</span></span> <span data-ttu-id="553a9-275">Is másolása varázsló az olyan lekérdezést, és az érvényesítés lehetőségre az eredményt.</span><span class="sxs-lookup"><span data-stu-id="553a9-275">You can also construct a query in the Copy Wizard and validate to see the result.</span></span>

### <a name="example"></a><span data-ttu-id="553a9-276">Példa</span><span class="sxs-lookup"><span data-stu-id="553a9-276">Example</span></span>
<span data-ttu-id="553a9-277">A következő "ExampleTable" például egy egész elsődleges kulcsként megadott oszlop "pk_int", érték nevű szöveges oszlop, egy listaoszlop, térkép oszlop és egy oszlopkészlet ("StringSet" nevű) nevű tartalmazó Cassandra adatbázistábla.</span><span class="sxs-lookup"><span data-stu-id="553a9-277">For example, the following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="553a9-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="553a9-278">pk_int</span></span> | <span data-ttu-id="553a9-279">Érték</span><span class="sxs-lookup"><span data-stu-id="553a9-279">Value</span></span> | <span data-ttu-id="553a9-280">Lista</span><span class="sxs-lookup"><span data-stu-id="553a9-280">List</span></span> | <span data-ttu-id="553a9-281">térkép</span><span class="sxs-lookup"><span data-stu-id="553a9-281">Map</span></span> | <span data-ttu-id="553a9-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="553a9-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="553a9-283">1</span><span class="sxs-lookup"><span data-stu-id="553a9-283">1</span></span> |<span data-ttu-id="553a9-284">"a minta érték 1"</span><span class="sxs-lookup"><span data-stu-id="553a9-284">"sample value 1"</span></span> |<span data-ttu-id="553a9-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="553a9-285">["1", "2", "3"]</span></span> |<span data-ttu-id="553a9-286">{"S1": "a", "S2": "b"}</span><span class="sxs-lookup"><span data-stu-id="553a9-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="553a9-287">{"A", "B", "C"}</span><span class="sxs-lookup"><span data-stu-id="553a9-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="553a9-288">3</span><span class="sxs-lookup"><span data-stu-id="553a9-288">3</span></span> |<span data-ttu-id="553a9-289">"a minta 3. érték"</span><span class="sxs-lookup"><span data-stu-id="553a9-289">"sample value 3"</span></span> |<span data-ttu-id="553a9-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="553a9-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="553a9-291">{"S1": "t"}</span><span class="sxs-lookup"><span data-stu-id="553a9-291">{"S1": "t"}</span></span> |<span data-ttu-id="553a9-292">{"A", "E"}</span><span class="sxs-lookup"><span data-stu-id="553a9-292">{"A", "E"}</span></span> |

<span data-ttu-id="553a9-293">Az illesztőprogram az egyetlen tábla képviselő virtuális táblákat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="553a9-293">The driver would generate multiple virtual tables to represent this single table.</span></span> <span data-ttu-id="553a9-294">A külső kulcs oszlopokra virtuális táblázatokban a valódi tábla elsődleges kulcs oszlopai hivatkozik, és melyik felel meg a virtuális tábla sorainak valós tábla sorainak jelzi.</span><span class="sxs-lookup"><span data-stu-id="553a9-294">The foreign key columns in the virtual tables reference the primary key columns in the real table, and indicate which real table row the virtual table row corresponds to.</span></span>

<span data-ttu-id="553a9-295">Az első virtuális táblát kell az alaptábla "ExampleTable" nevű az alábbi táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="553a9-295">The first virtual table is the base table named “ExampleTable” is shown in the following table.</span></span> <span data-ttu-id="553a9-296">A következő alaptáblában az eredeti adatbázis táblából a gyűjteményeket, amelyek ebből a táblázatból nincs megadva, és más virtuális táblák kibontva ugyanazokat az adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="553a9-296">The base table contains the same data as the original database table except for the collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="553a9-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="553a9-297">pk_int</span></span> | <span data-ttu-id="553a9-298">Érték</span><span class="sxs-lookup"><span data-stu-id="553a9-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="553a9-299">1</span><span class="sxs-lookup"><span data-stu-id="553a9-299">1</span></span> |<span data-ttu-id="553a9-300">"a minta érték 1"</span><span class="sxs-lookup"><span data-stu-id="553a9-300">"sample value 1"</span></span> |
| <span data-ttu-id="553a9-301">3</span><span class="sxs-lookup"><span data-stu-id="553a9-301">3</span></span> |<span data-ttu-id="553a9-302">"a minta 3. érték"</span><span class="sxs-lookup"><span data-stu-id="553a9-302">"sample value 3"</span></span> |

<span data-ttu-id="553a9-303">Az alábbi táblázatok bemutatják az adatokat a listában, térkép és StringSet oszlopokból renormalize virtuális táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="553a9-303">The following tables show the virtual tables that renormalize the data from the List, Map, and StringSet columns.</span></span> <span data-ttu-id="553a9-304">Az oszlopok kiderül, hogy a "_index" vagy "_kulcsvédelmi" adja meg az adatokat az eredeti lista vagy a térkép pozícióját.</span><span class="sxs-lookup"><span data-stu-id="553a9-304">The columns with names that end with “_index” or “_key” indicate the position of the data within the original list or map.</span></span> <span data-ttu-id="553a9-305">"_Value" végződő nevű oszlopot tartalmazhat a kibontott adatok a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="553a9-305">The columns with names that end with “_value” contain the expanded data from the collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="553a9-306">"ExampleTable_vt_List". tábla:</span><span class="sxs-lookup"><span data-stu-id="553a9-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="553a9-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="553a9-307">pk_int</span></span> | <span data-ttu-id="553a9-308">List_index</span><span class="sxs-lookup"><span data-stu-id="553a9-308">List_index</span></span> | <span data-ttu-id="553a9-309">List_value</span><span class="sxs-lookup"><span data-stu-id="553a9-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="553a9-310">1</span><span class="sxs-lookup"><span data-stu-id="553a9-310">1</span></span> |<span data-ttu-id="553a9-311">0</span><span class="sxs-lookup"><span data-stu-id="553a9-311">0</span></span> |<span data-ttu-id="553a9-312">1</span><span class="sxs-lookup"><span data-stu-id="553a9-312">1</span></span> |
| <span data-ttu-id="553a9-313">1</span><span class="sxs-lookup"><span data-stu-id="553a9-313">1</span></span> |<span data-ttu-id="553a9-314">1</span><span class="sxs-lookup"><span data-stu-id="553a9-314">1</span></span> |<span data-ttu-id="553a9-315">2</span><span class="sxs-lookup"><span data-stu-id="553a9-315">2</span></span> |
| <span data-ttu-id="553a9-316">1</span><span class="sxs-lookup"><span data-stu-id="553a9-316">1</span></span> |<span data-ttu-id="553a9-317">2</span><span class="sxs-lookup"><span data-stu-id="553a9-317">2</span></span> |<span data-ttu-id="553a9-318">3</span><span class="sxs-lookup"><span data-stu-id="553a9-318">3</span></span> |
| <span data-ttu-id="553a9-319">3</span><span class="sxs-lookup"><span data-stu-id="553a9-319">3</span></span> |<span data-ttu-id="553a9-320">0</span><span class="sxs-lookup"><span data-stu-id="553a9-320">0</span></span> |<span data-ttu-id="553a9-321">100</span><span class="sxs-lookup"><span data-stu-id="553a9-321">100</span></span> |
| <span data-ttu-id="553a9-322">3</span><span class="sxs-lookup"><span data-stu-id="553a9-322">3</span></span> |<span data-ttu-id="553a9-323">1</span><span class="sxs-lookup"><span data-stu-id="553a9-323">1</span></span> |<span data-ttu-id="553a9-324">101</span><span class="sxs-lookup"><span data-stu-id="553a9-324">101</span></span> |
| <span data-ttu-id="553a9-325">3</span><span class="sxs-lookup"><span data-stu-id="553a9-325">3</span></span> |<span data-ttu-id="553a9-326">2</span><span class="sxs-lookup"><span data-stu-id="553a9-326">2</span></span> |<span data-ttu-id="553a9-327">102</span><span class="sxs-lookup"><span data-stu-id="553a9-327">102</span></span> |
| <span data-ttu-id="553a9-328">3</span><span class="sxs-lookup"><span data-stu-id="553a9-328">3</span></span> |<span data-ttu-id="553a9-329">3</span><span class="sxs-lookup"><span data-stu-id="553a9-329">3</span></span> |<span data-ttu-id="553a9-330">103</span><span class="sxs-lookup"><span data-stu-id="553a9-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="553a9-331">"ExampleTable_vt_Map". tábla:</span><span class="sxs-lookup"><span data-stu-id="553a9-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="553a9-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="553a9-332">pk_int</span></span> | <span data-ttu-id="553a9-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="553a9-333">Map_key</span></span> | <span data-ttu-id="553a9-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="553a9-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="553a9-335">1</span><span class="sxs-lookup"><span data-stu-id="553a9-335">1</span></span> |<span data-ttu-id="553a9-336">S1</span><span class="sxs-lookup"><span data-stu-id="553a9-336">S1</span></span> |<span data-ttu-id="553a9-337">A</span><span class="sxs-lookup"><span data-stu-id="553a9-337">A</span></span> |
| <span data-ttu-id="553a9-338">1</span><span class="sxs-lookup"><span data-stu-id="553a9-338">1</span></span> |<span data-ttu-id="553a9-339">S2</span><span class="sxs-lookup"><span data-stu-id="553a9-339">S2</span></span> |<span data-ttu-id="553a9-340">B</span><span class="sxs-lookup"><span data-stu-id="553a9-340">b</span></span> |
| <span data-ttu-id="553a9-341">3</span><span class="sxs-lookup"><span data-stu-id="553a9-341">3</span></span> |<span data-ttu-id="553a9-342">S1</span><span class="sxs-lookup"><span data-stu-id="553a9-342">S1</span></span> |<span data-ttu-id="553a9-343">T</span><span class="sxs-lookup"><span data-stu-id="553a9-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="553a9-344">"ExampleTable_vt_StringSet". tábla:</span><span class="sxs-lookup"><span data-stu-id="553a9-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="553a9-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="553a9-345">pk_int</span></span> | <span data-ttu-id="553a9-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="553a9-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="553a9-347">1</span><span class="sxs-lookup"><span data-stu-id="553a9-347">1</span></span> |<span data-ttu-id="553a9-348">A</span><span class="sxs-lookup"><span data-stu-id="553a9-348">A</span></span> |
| <span data-ttu-id="553a9-349">1</span><span class="sxs-lookup"><span data-stu-id="553a9-349">1</span></span> |<span data-ttu-id="553a9-350">B</span><span class="sxs-lookup"><span data-stu-id="553a9-350">B</span></span> |
| <span data-ttu-id="553a9-351">1</span><span class="sxs-lookup"><span data-stu-id="553a9-351">1</span></span> |<span data-ttu-id="553a9-352">C#</span><span class="sxs-lookup"><span data-stu-id="553a9-352">C</span></span> |
| <span data-ttu-id="553a9-353">3</span><span class="sxs-lookup"><span data-stu-id="553a9-353">3</span></span> |<span data-ttu-id="553a9-354">A</span><span class="sxs-lookup"><span data-stu-id="553a9-354">A</span></span> |
| <span data-ttu-id="553a9-355">3</span><span class="sxs-lookup"><span data-stu-id="553a9-355">3</span></span> |<span data-ttu-id="553a9-356">E</span><span class="sxs-lookup"><span data-stu-id="553a9-356">E</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="553a9-357">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="553a9-357">Map source to sink columns</span></span>
<span data-ttu-id="553a9-358">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="553a9-358">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="553a9-359">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="553a9-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="553a9-360">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="553a9-360">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="553a9-361">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="553a9-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="553a9-362">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="553a9-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="553a9-363">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="553a9-363">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="553a9-364">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="553a9-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="553a9-365">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="553a9-365">Performance and Tuning</span></span>
<span data-ttu-id="553a9-366">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="553a9-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
