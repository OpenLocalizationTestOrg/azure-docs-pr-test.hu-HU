---
title: "Adatok áthelyezése a Data Factory használatával MongoDB |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával MongoDB-adatbázisból."
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
ms.openlocfilehash: ac4ff55c765a5b874b81714c3d0063a5b4765a05
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a><span data-ttu-id="da7af-103">Helyezze át az adatokat a MongoDB Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="da7af-103">Move data From MongoDB using Azure Data Factory</span></span>
<span data-ttu-id="da7af-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="da7af-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises MongoDB database.</span></span> <span data-ttu-id="da7af-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="da7af-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="da7af-106">Egy helyszíni MongoDB adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="da7af-106">You can copy data from an on-premises MongoDB data store to any supported sink data store.</span></span> <span data-ttu-id="da7af-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="da7af-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="da7af-108">Adat-előállító jelenleg csak áthelyezése adatait a MongoDB-tárolóban egyéb adattárakhoz, de nem az adatok áthelyezése az egyéb adattárakhoz egy MongoDB adattárral.</span><span class="sxs-lookup"><span data-stu-id="da7af-108">Data factory currently supports only moving data from a MongoDB data store to other data stores, but not for moving data from other data stores to an MongoDB datastore.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="da7af-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="da7af-109">Prerequisites</span></span>
<span data-ttu-id="da7af-110">Az Azure Data Factory szolgáltatás kell kapcsolódnia kell a helyszíni MongoDB-adatbázist a következő összetevőket kell telepíteni:</span><span class="sxs-lookup"><span data-stu-id="da7af-110">For the Azure Data Factory service to be able to connect to your on-premises MongoDB database, you must install the following components:</span></span>

- <span data-ttu-id="da7af-111">MongoDB-verziók a következők: 2.4, 2.6, 3.0-s és 3.2-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="da7af-111">Supported MongoDB versions are:  2.4, 2.6, 3.0, and 3.2.</span></span>
- <span data-ttu-id="da7af-112">Az adatkezelési átjáró ugyanazon a számítógépen, amelyen az adatbázis vagy egy külön számítógépen elkerülésére használják a források az adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="da7af-112">Data Management Gateway on the same machine that hosts the database or on a separate machine to avoid competing for resources with the database.</span></span> <span data-ttu-id="da7af-113">Az adatkezelési átjáró olyan szoftver, a helyszíni adatforrások csatlakozik a felhőalapú szolgáltatások biztonságának és kezelésének módja.</span><span class="sxs-lookup"><span data-stu-id="da7af-113">Data Management Gateway is a software that connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="da7af-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="da7af-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="da7af-115">Lásd: [tárolt adatok mozgatása felhőbe helyszíni](data-factory-move-data-between-onprem-and-cloud.md) cikk lépésenkénti adatok folyamat az átjáró beállítása áthelyezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="da7af-115">See [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

    <span data-ttu-id="da7af-116">Az átjáró telepítésekor automatikusan telepíti a MongoDB való kapcsolódáshoz használt Microsoft MongoDB ODBC-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="da7af-116">When you install the gateway, it automatically installs a Microsoft MongoDB ODBC driver used to connect to MongoDB.</span></span>

    > [!NOTE]
    > <span data-ttu-id="da7af-117">Az átjáró használatához a MongoDB kapcsolódni, akkor is, ha az Azure IaaS virtuális gépeken fut. kell.</span><span class="sxs-lookup"><span data-stu-id="da7af-117">You need to use the gateway to connect to MongoDB even if it is hosted in Azure IaaS VMs.</span></span> <span data-ttu-id="da7af-118">Ha próbál csatlakozni a felhőben üzemeltetett MongoDB példánya, az infrastruktúra-szolgáltatási virtuális gép is az átjárópéldány is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="da7af-118">If you are trying to connect to an instance of MongoDB hosted in cloud, you can also install the gateway instance in the IaaS VM.</span></span>

## <a name="getting-started"></a><span data-ttu-id="da7af-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="da7af-119">Getting started</span></span>
<span data-ttu-id="da7af-120">A másolási tevékenység, mely az adatok egy helyszíni MongoDB adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="da7af-120">You can create a pipeline with a copy activity that moves data from an on-premises MongoDB data store by using different tools/APIs.</span></span>

<span data-ttu-id="da7af-121">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="da7af-121">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="da7af-122">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="da7af-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="da7af-123">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="da7af-123">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="da7af-124">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="da7af-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="da7af-125">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="da7af-125">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="da7af-126">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="da7af-126">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="da7af-127">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="da7af-127">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="da7af-128">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="da7af-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="da7af-129">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="da7af-129">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="da7af-130">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="da7af-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="da7af-131">Adatok másolása egy helyszíni MongoDB adattároló használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az MongoDB az Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="da7af-131">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises MongoDB data store, see [JSON example: Copy data from MongoDB to Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="da7af-132">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások MongoDB forrás JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="da7af-132">The following sections provide details about JSON properties that are used to define Data Factory entities specific to MongoDB source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="da7af-133">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="da7af-133">Linked service properties</span></span>
<span data-ttu-id="da7af-134">A következő táblázat tartalmazza a JSON-elemek szerepelnek jellemző leírást **OnPremisesMongoDB** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="da7af-134">The following table provides description for JSON elements specific to **OnPremisesMongoDB** linked service.</span></span>

| <span data-ttu-id="da7af-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="da7af-135">Property</span></span> | <span data-ttu-id="da7af-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="da7af-136">Description</span></span> | <span data-ttu-id="da7af-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="da7af-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="da7af-138">type</span><span class="sxs-lookup"><span data-stu-id="da7af-138">type</span></span> |<span data-ttu-id="da7af-139">A type tulajdonságot kell beállítani: **OnPremisesMongoDb**</span><span class="sxs-lookup"><span data-stu-id="da7af-139">The type property must be set to: **OnPremisesMongoDb**</span></span> |<span data-ttu-id="da7af-140">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-140">Yes</span></span> |
| <span data-ttu-id="da7af-141">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="da7af-141">server</span></span> |<span data-ttu-id="da7af-142">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét a mongodb-Protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="da7af-142">IP address or host name of the MongoDB server.</span></span> |<span data-ttu-id="da7af-143">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-143">Yes</span></span> |
| <span data-ttu-id="da7af-144">port</span><span class="sxs-lookup"><span data-stu-id="da7af-144">port</span></span> |<span data-ttu-id="da7af-145">A MongoDB-kiszolgálóhoz a kapcsolatok figyelésére használt TCP portot.</span><span class="sxs-lookup"><span data-stu-id="da7af-145">TCP port that the MongoDB server uses to listen for client connections.</span></span> |<span data-ttu-id="da7af-146">Nem kötelező, alapértelmezett érték: 27017</span><span class="sxs-lookup"><span data-stu-id="da7af-146">Optional, default value: 27017</span></span> |
| <span data-ttu-id="da7af-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="da7af-147">authenticationType</span></span> |<span data-ttu-id="da7af-148">Alapszintű, vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="da7af-148">Basic, or Anonymous.</span></span> |<span data-ttu-id="da7af-149">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-149">Yes</span></span> |
| <span data-ttu-id="da7af-150">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="da7af-150">username</span></span> |<span data-ttu-id="da7af-151">Felhasználói fiók MongoDB eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="da7af-151">User account to access MongoDB.</span></span> |<span data-ttu-id="da7af-152">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="da7af-152">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="da7af-153">jelszó</span><span class="sxs-lookup"><span data-stu-id="da7af-153">password</span></span> |<span data-ttu-id="da7af-154">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="da7af-154">Password for the user.</span></span> |<span data-ttu-id="da7af-155">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="da7af-155">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="da7af-156">authSource</span><span class="sxs-lookup"><span data-stu-id="da7af-156">authSource</span></span> |<span data-ttu-id="da7af-157">A MongoDB-adatbázist, amely a hitelesítő adatok kereséséhez használni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="da7af-157">Name of the MongoDB database that you want to use to check your credentials for authentication.</span></span> |<span data-ttu-id="da7af-158">Választható (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="da7af-158">Optional (if basic authentication is used).</span></span> <span data-ttu-id="da7af-159">alapértelmezett: a rendszergazdai fiókot és a databaseName tulajdonsággal megadott adatbázis használ.</span><span class="sxs-lookup"><span data-stu-id="da7af-159">default: uses the admin account and the database specified using databaseName property.</span></span> |
| <span data-ttu-id="da7af-160">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="da7af-160">databaseName</span></span> |<span data-ttu-id="da7af-161">A MongoDB-adatbázist, amely az elérni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="da7af-161">Name of the MongoDB database that you want to access.</span></span> |<span data-ttu-id="da7af-162">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-162">Yes</span></span> |
| <span data-ttu-id="da7af-163">gatewayName</span><span class="sxs-lookup"><span data-stu-id="da7af-163">gatewayName</span></span> |<span data-ttu-id="da7af-164">Az átjáró, aki hozzáfér az adattár neve.</span><span class="sxs-lookup"><span data-stu-id="da7af-164">Name of the gateway that accesses the data store.</span></span> |<span data-ttu-id="da7af-165">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-165">Yes</span></span> |
| <span data-ttu-id="da7af-166">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="da7af-166">encryptedCredential</span></span> |<span data-ttu-id="da7af-167">A hitelesítőadat-átjáró által titkosított.</span><span class="sxs-lookup"><span data-stu-id="da7af-167">Credential encrypted by gateway.</span></span> |<span data-ttu-id="da7af-168">Optional</span><span class="sxs-lookup"><span data-stu-id="da7af-168">Optional</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="da7af-169">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="da7af-169">Dataset properties</span></span>
<span data-ttu-id="da7af-170">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="da7af-170">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="da7af-171">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="da7af-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="da7af-172">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="da7af-172">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="da7af-173">A typeProperties szakasz típusú adatkészlet **MongoDbCollection** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="da7af-173">The typeProperties section for dataset of type **MongoDbCollection** has the following properties:</span></span>

| <span data-ttu-id="da7af-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="da7af-174">Property</span></span> | <span data-ttu-id="da7af-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="da7af-175">Description</span></span> | <span data-ttu-id="da7af-176">Szükséges</span><span class="sxs-lookup"><span data-stu-id="da7af-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="da7af-177">CollectionName</span><span class="sxs-lookup"><span data-stu-id="da7af-177">collectionName</span></span> |<span data-ttu-id="da7af-178">A MongoDB-adatbázist a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="da7af-178">Name of the collection in MongoDB database.</span></span> |<span data-ttu-id="da7af-179">Igen</span><span class="sxs-lookup"><span data-stu-id="da7af-179">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="da7af-180">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="da7af-180">Copy activity properties</span></span>
<span data-ttu-id="da7af-181">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="da7af-181">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="da7af-182">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="da7af-182">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="da7af-183">Tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység viszont eltérőek a tevékenységek minden típusának.</span><span class="sxs-lookup"><span data-stu-id="da7af-183">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="da7af-184">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="da7af-184">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="da7af-185">Ha a forrás típusa nem **MongoDbSource** typeProperties szakaszában érhetők a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="da7af-185">When the source is of type **MongoDbSource** the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="da7af-186">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="da7af-186">Property</span></span> | <span data-ttu-id="da7af-187">Leírás</span><span class="sxs-lookup"><span data-stu-id="da7af-187">Description</span></span> | <span data-ttu-id="da7af-188">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="da7af-188">Allowed values</span></span> | <span data-ttu-id="da7af-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="da7af-189">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="da7af-190">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="da7af-190">query</span></span> |<span data-ttu-id="da7af-191">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="da7af-191">Use the custom query to read data.</span></span> |<span data-ttu-id="da7af-192">SQL-92 lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="da7af-192">SQL-92 query string.</span></span> <span data-ttu-id="da7af-193">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="da7af-193">For example: select * from MyTable.</span></span> |<span data-ttu-id="da7af-194">Nem (Ha **collectionName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="da7af-194">No (if **collectionName** of **dataset** is specified)</span></span> |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a><span data-ttu-id="da7af-195">JSON-példa: adatok másolása az MongoDB az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="da7af-195">JSON example: Copy data from MongoDB to Azure Blob</span></span>
<span data-ttu-id="da7af-196">Ebben a példában a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="da7af-196">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="da7af-197">Azt illusztrálja, hogyan adatok másolása egy helyszíni MongoDB egy Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="da7af-197">It shows how to copy data from an on-premises MongoDB to an Azure Blob Storage.</span></span> <span data-ttu-id="da7af-198">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="da7af-198">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="da7af-199">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="da7af-199">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="da7af-200">A társított szolgáltatás típusa [OnPremisesMongoDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="da7af-200">A linked service of type [OnPremisesMongoDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="da7af-201">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="da7af-201">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="da7af-202">Bemeneti [dataset](data-factory-create-datasets.md) típusú [MongoDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="da7af-202">An input [dataset](data-factory-create-datasets.md) of type [MongoDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="da7af-203">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="da7af-203">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="da7af-204">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [MongoDbSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="da7af-204">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [MongoDbSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="da7af-205">A minta másol adatokat egy lekérdezés eredményét a MongoDB adatbázis blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="da7af-205">The sample copies data from a query result in MongoDB database to a blob every hour.</span></span> <span data-ttu-id="da7af-206">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="da7af-206">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="da7af-207">Első lépésként, a telepítő az adatkezelési átjáró szerint utasításait a [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="da7af-207">As a first step, setup the data management gateway as per the instructions in the [Data Management Gateway](data-factory-data-management-gateway.md) article.</span></span>

<span data-ttu-id="da7af-208">**MongoDB társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="da7af-208">**MongoDB linked service:**</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",  
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

<span data-ttu-id="da7af-209">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="da7af-209">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="da7af-210">**MongoDB bemeneti adatkészlet:** "external" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="da7af-210">**MongoDB input dataset:** Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="da7af-211">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="da7af-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="da7af-212">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="da7af-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="da7af-213">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="da7af-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="da7af-214">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="da7af-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="da7af-215">**Másolási tevékenység a MongoDB-forrás és a Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="da7af-215">**Copy activity in a pipeline with MongoDB source and Blob sink:**</span></span>

<span data-ttu-id="da7af-216">A feldolgozási sor tartalmazza a másolási tevékenység során használja a fenti bemeneti és kimeneti adatkészletek konfigurált és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="da7af-216">The pipeline contains a Copy Activity that is configured to use the above input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="da7af-217">Az adatcsatorna JSON-definícióból a **forrás** típusúra **MongoDbSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="da7af-217">In the pipeline JSON definition, the **source** type is set to **MongoDbSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="da7af-218">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="da7af-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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


## <a name="schema-by-data-factory"></a><span data-ttu-id="da7af-219">Adat-előállító sémája</span><span class="sxs-lookup"><span data-stu-id="da7af-219">Schema by Data Factory</span></span>
<span data-ttu-id="da7af-220">Az Azure Data Factory szolgáltatásnak a MongoDB-gyűjteményből séma kikövetkezteti a legújabb 100 dokumentumok használatával a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="da7af-220">Azure Data Factory service infers schema from a MongoDB collection by using the latest 100 documents in the collection.</span></span> <span data-ttu-id="da7af-221">A 100 dokumentumok teljes séma nem tartalmaznak, ha azokat az oszlopokat is figyelmen kívül hagyja a másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="da7af-221">If these 100 documents do not contain full schema, some columns may be ignored during the copy operation.</span></span>

## <a name="type-mapping-for-mongodb"></a><span data-ttu-id="da7af-222">Mongodb-protokolltámogatással leképezésének</span><span class="sxs-lookup"><span data-stu-id="da7af-222">Type mapping for MongoDB</span></span>
<span data-ttu-id="da7af-223">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység eseményforrás-típusnak gyűjtése típusa a következő 2. lépés – a módszert használja az automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="da7af-223">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="da7af-224">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="da7af-224">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="da7af-225">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="da7af-225">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="da7af-226">Ha az adatok áthelyezése a MongoDB .NET típusú a következő megfeleltetéseket használtak MongoDB típusok.</span><span class="sxs-lookup"><span data-stu-id="da7af-226">When moving data to MongoDB the following mappings are used from MongoDB types to .NET types.</span></span>

| <span data-ttu-id="da7af-227">MongoDB-típus</span><span class="sxs-lookup"><span data-stu-id="da7af-227">MongoDB type</span></span> | <span data-ttu-id="da7af-228">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="da7af-228">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="da7af-229">Bináris</span><span class="sxs-lookup"><span data-stu-id="da7af-229">Binary</span></span> |<span data-ttu-id="da7af-230">Byte]</span><span class="sxs-lookup"><span data-stu-id="da7af-230">Byte[]</span></span> |
| <span data-ttu-id="da7af-231">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="da7af-231">Boolean</span></span> |<span data-ttu-id="da7af-232">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="da7af-232">Boolean</span></span> |
| <span data-ttu-id="da7af-233">Dátum</span><span class="sxs-lookup"><span data-stu-id="da7af-233">Date</span></span> |<span data-ttu-id="da7af-234">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="da7af-234">DateTime</span></span> |
| <span data-ttu-id="da7af-235">NumberDouble</span><span class="sxs-lookup"><span data-stu-id="da7af-235">NumberDouble</span></span> |<span data-ttu-id="da7af-236">Dupla</span><span class="sxs-lookup"><span data-stu-id="da7af-236">Double</span></span> |
| <span data-ttu-id="da7af-237">NumberInt</span><span class="sxs-lookup"><span data-stu-id="da7af-237">NumberInt</span></span> |<span data-ttu-id="da7af-238">Int32</span><span class="sxs-lookup"><span data-stu-id="da7af-238">Int32</span></span> |
| <span data-ttu-id="da7af-239">NumberLong</span><span class="sxs-lookup"><span data-stu-id="da7af-239">NumberLong</span></span> |<span data-ttu-id="da7af-240">Int64</span><span class="sxs-lookup"><span data-stu-id="da7af-240">Int64</span></span> |
| <span data-ttu-id="da7af-241">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="da7af-241">ObjectID</span></span> |<span data-ttu-id="da7af-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="da7af-242">String</span></span> |
| <span data-ttu-id="da7af-243">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="da7af-243">String</span></span> |<span data-ttu-id="da7af-244">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="da7af-244">String</span></span> |
| <span data-ttu-id="da7af-245">UUID</span><span class="sxs-lookup"><span data-stu-id="da7af-245">UUID</span></span> |<span data-ttu-id="da7af-246">GUID</span><span class="sxs-lookup"><span data-stu-id="da7af-246">Guid</span></span> |
| <span data-ttu-id="da7af-247">Objektum</span><span class="sxs-lookup"><span data-stu-id="da7af-247">Object</span></span> |<span data-ttu-id="da7af-248">Renormalized történő egybesimítására "_" beágyazott elválasztójelként oszlopok</span><span class="sxs-lookup"><span data-stu-id="da7af-248">Renormalized into flatten columns with “_” as nested separator</span></span> |

> [!NOTE]
> <span data-ttu-id="da7af-249">Virtuális táblák használata tömbök-támogatással kapcsolatos további tudnivalókért tekintse meg [támogatja az összetett típusok virtuális táblák használata](#support-for-complex-types-using-virtual-tables) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="da7af-249">To learn about support for arrays using virtual tables, refer to [Support for complex types using virtual tables](#support-for-complex-types-using-virtual-tables) section below.</span></span>

<span data-ttu-id="da7af-250">Jelenleg a következő MongoDB-adattípusok nem támogatottak: DBPointer, JavaScript, Max percenkénti kulcs, reguláris kifejezés, szimbólum, Timestamp, meghatározatlan</span><span class="sxs-lookup"><span data-stu-id="da7af-250">Currently, the following MongoDB data types are not supported: DBPointer, JavaScript, Max/Min key, Regular Expression, Symbol, Timestamp, Undefined</span></span>

## <a name="support-for-complex-types-using-virtual-tables"></a><span data-ttu-id="da7af-251">Virtuális táblák használata összetett típusok támogatása</span><span class="sxs-lookup"><span data-stu-id="da7af-251">Support for complex types using virtual tables</span></span>
<span data-ttu-id="da7af-252">Az Azure Data Factory beépített ODBC-illesztőprogram használatával csatlakozhat, és másolja az adatokat a MongoDB-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="da7af-252">Azure Data Factory uses a built-in ODBC driver to connect to and copy data from your MongoDB database.</span></span> <span data-ttu-id="da7af-253">A dokumentumok között különböző típusú tömb, vagy objektumok például összetett típusok az illesztőprogram újra normalizálja adatok megfelelő virtuális táblákba.</span><span class="sxs-lookup"><span data-stu-id="da7af-253">For complex types such as arrays or objects with different types across the documents, the driver re-normalizes data into corresponding virtual tables.</span></span> <span data-ttu-id="da7af-254">Pontosabban Ha a tábla tartalmaz ilyen oszlopok, az illesztőprogram állít elő, a következő virtuális táblák:</span><span class="sxs-lookup"><span data-stu-id="da7af-254">Specifically, if a table contains such columns, the driver generates the following virtual tables:</span></span>

* <span data-ttu-id="da7af-255">A **alaptábla**, amely tartalmazza a tényleges táblából, az összetett típusú oszlopok ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="da7af-255">A **base table**, which contains the same data as the real table except for the complex type columns.</span></span> <span data-ttu-id="da7af-256">Az alaptábla ugyanazt a nevet használja, amely valós táblázatként.</span><span class="sxs-lookup"><span data-stu-id="da7af-256">The base table uses the same name as the real table that it represents.</span></span>
* <span data-ttu-id="da7af-257">A **virtuális tábla** minden összetett típusú oszlophoz, amely kiterjeszti a beágyazott adatok.</span><span class="sxs-lookup"><span data-stu-id="da7af-257">A **virtual table** for each complex type column, which expands the nested data.</span></span> <span data-ttu-id="da7af-258">A virtuális táblák neve a valódi tábla, "_" elválasztó és a nevét, a tömb vagy objektum nevével.</span><span class="sxs-lookup"><span data-stu-id="da7af-258">The virtual tables are named using the name of the real table, a separator “_” and the name of the array or object.</span></span>

<span data-ttu-id="da7af-259">Virtuális táblák tekintse meg az adatok a valós táblázatban, az illesztőprogram, a nem normalizált adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="da7af-259">Virtual tables refer to the data in the real table, enabling the driver to access the denormalized data.</span></span> <span data-ttu-id="da7af-260">Részletekért lásd: Példa szakaszban olvashatók.</span><span class="sxs-lookup"><span data-stu-id="da7af-260">See Example section below details.</span></span> <span data-ttu-id="da7af-261">A MongoDB-tömbök tartalmának lekérdezése, és a virtuális tábla érheti el.</span><span class="sxs-lookup"><span data-stu-id="da7af-261">You can access the content of MongoDB arrays by querying and joining the virtual tables.</span></span>

<span data-ttu-id="da7af-262">Használhatja a [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuitív táblázatok listájának megtekintése a MongoDB-adatbázist, beleértve a virtuális táblák és található adatok megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="da7af-262">You can use the [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) to intuitively view the list of tables in MongoDB database including the virtual tables, and preview the data inside.</span></span> <span data-ttu-id="da7af-263">Is másolása varázsló az olyan lekérdezést, és az érvényesítés lehetőségre az eredményt.</span><span class="sxs-lookup"><span data-stu-id="da7af-263">You can also construct a query in the Copy Wizard and validate to see the result.</span></span>

### <a name="example"></a><span data-ttu-id="da7af-264">Példa</span><span class="sxs-lookup"><span data-stu-id="da7af-264">Example</span></span>
<span data-ttu-id="da7af-265">"ExampleTable" alatt például MongoDB tábla egy objektumokból álló tömb egy oszlopot az egyes cellák – számlákat és a skaláris típusok – minősítések tömbje egy oszlop.</span><span class="sxs-lookup"><span data-stu-id="da7af-265">For example, “ExampleTable” below is a MongoDB table that has one column with an array of Objects in each cell – Invoices, and one column with an array of Scalar types – Ratings.</span></span>

| <span data-ttu-id="da7af-266">_id</span><span class="sxs-lookup"><span data-stu-id="da7af-266">_id</span></span> | <span data-ttu-id="da7af-267">Ügyfél neve</span><span class="sxs-lookup"><span data-stu-id="da7af-267">Customer Name</span></span> | <span data-ttu-id="da7af-268">Számlák</span><span class="sxs-lookup"><span data-stu-id="da7af-268">Invoices</span></span> | <span data-ttu-id="da7af-269">Szolgáltatási szint</span><span class="sxs-lookup"><span data-stu-id="da7af-269">Service Level</span></span> | <span data-ttu-id="da7af-270">Minősítések</span><span class="sxs-lookup"><span data-stu-id="da7af-270">Ratings</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="da7af-271">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-271">1111</span></span> |<span data-ttu-id="da7af-272">ABC</span><span class="sxs-lookup"><span data-stu-id="da7af-272">ABC</span></span> |<span data-ttu-id="da7af-273">[{invoice_id: "123", cikk: "toaster", az ár: "456" kedvezményes: "0,2"}, {invoice_id: "124" elem: "helyezzük", az ár: "1235" kedvezményes: "0,2"}]</span><span class="sxs-lookup"><span data-stu-id="da7af-273">[{invoice_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice_id:”124”, item:”oven”, price: ”1235”, discount: ”0.2”}]</span></span> |<span data-ttu-id="da7af-274">Ezüst</span><span class="sxs-lookup"><span data-stu-id="da7af-274">Silver</span></span> |<span data-ttu-id="da7af-275">[5,6]</span><span class="sxs-lookup"><span data-stu-id="da7af-275">[5,6]</span></span> |
| <span data-ttu-id="da7af-276">2222</span><span class="sxs-lookup"><span data-stu-id="da7af-276">2222</span></span> |<span data-ttu-id="da7af-277">XYZ</span><span class="sxs-lookup"><span data-stu-id="da7af-277">XYZ</span></span> |<span data-ttu-id="da7af-278">[{invoice_id: "135", cikk: "kombinált hűtőszekrények", az ár: "12543" kedvezménnyel: "0,0"}]</span><span class="sxs-lookup"><span data-stu-id="da7af-278">[{invoice_id:”135”, item:”fridge”, price: ”12543”, discount: ”0.0”}]</span></span> |<span data-ttu-id="da7af-279">Arany</span><span class="sxs-lookup"><span data-stu-id="da7af-279">Gold</span></span> |<span data-ttu-id="da7af-280">[1,2]</span><span class="sxs-lookup"><span data-stu-id="da7af-280">[1,2]</span></span> |

<span data-ttu-id="da7af-281">Az illesztőprogram az egyetlen tábla képviselő virtuális táblákat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="da7af-281">The driver would generate multiple virtual tables to represent this single table.</span></span> <span data-ttu-id="da7af-282">Az első virtuális táblát kell az alaptábla nevű "ExampleTable" alább látható.</span><span class="sxs-lookup"><span data-stu-id="da7af-282">The first virtual table is the base table named “ExampleTable”, shown below.</span></span> <span data-ttu-id="da7af-283">Az alaptábla az eredeti tábla összes adatot tartalmaz, de az adatokat a tömbök kimaradt, és a virtuális táblázatokban ki van bontva.</span><span class="sxs-lookup"><span data-stu-id="da7af-283">The base table contains all the data of the original table, but the data from the arrays has been omitted and is expanded in the virtual tables.</span></span>

| <span data-ttu-id="da7af-284">_id</span><span class="sxs-lookup"><span data-stu-id="da7af-284">_id</span></span> | <span data-ttu-id="da7af-285">Ügyfél neve</span><span class="sxs-lookup"><span data-stu-id="da7af-285">Customer Name</span></span> | <span data-ttu-id="da7af-286">Szolgáltatási szint</span><span class="sxs-lookup"><span data-stu-id="da7af-286">Service Level</span></span> |
| --- | --- | --- |
| <span data-ttu-id="da7af-287">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-287">1111</span></span> |<span data-ttu-id="da7af-288">ABC</span><span class="sxs-lookup"><span data-stu-id="da7af-288">ABC</span></span> |<span data-ttu-id="da7af-289">Ezüst</span><span class="sxs-lookup"><span data-stu-id="da7af-289">Silver</span></span> |
| <span data-ttu-id="da7af-290">2222</span><span class="sxs-lookup"><span data-stu-id="da7af-290">2222</span></span> |<span data-ttu-id="da7af-291">XYZ</span><span class="sxs-lookup"><span data-stu-id="da7af-291">XYZ</span></span> |<span data-ttu-id="da7af-292">Arany</span><span class="sxs-lookup"><span data-stu-id="da7af-292">Gold</span></span> |

<span data-ttu-id="da7af-293">Az alábbi táblázatok bemutatják a virtuális táblákat, amelyek megfelelnek a példában az eredeti tömbök.</span><span class="sxs-lookup"><span data-stu-id="da7af-293">The following tables show the virtual tables that represent the original arrays in the example.</span></span> <span data-ttu-id="da7af-294">Ezek a táblázatok tartalmazzák a következő:</span><span class="sxs-lookup"><span data-stu-id="da7af-294">These tables contain the following:</span></span>

* <span data-ttu-id="da7af-295">Vissza az eredeti elsődleges kulcsként megadott oszlop (keresztül a _id. oszlop) eredeti tömb sorának megfelelő mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="da7af-295">A reference back to the original primary key column corresponding to the row of the original array (via the _id column)</span></span>
* <span data-ttu-id="da7af-296">Utalhat, hogy az adatok az eredeti tömbön belüli pozíciója</span><span class="sxs-lookup"><span data-stu-id="da7af-296">An indication of the position of the data within the original array</span></span>
* <span data-ttu-id="da7af-297">A kibontott adatok belül a tömb egyes elemei</span><span class="sxs-lookup"><span data-stu-id="da7af-297">The expanded data for each element within the array</span></span>

<span data-ttu-id="da7af-298">"ExampleTable_Invoices". tábla:</span><span class="sxs-lookup"><span data-stu-id="da7af-298">Table “ExampleTable_Invoices”:</span></span>

| <span data-ttu-id="da7af-299">_id</span><span class="sxs-lookup"><span data-stu-id="da7af-299">_id</span></span> | <span data-ttu-id="da7af-300">ExampleTable_Invoices_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="da7af-300">ExampleTable_Invoices_dim1_idx</span></span> | <span data-ttu-id="da7af-301">invoice_id</span><span class="sxs-lookup"><span data-stu-id="da7af-301">invoice_id</span></span> | <span data-ttu-id="da7af-302">Elem</span><span class="sxs-lookup"><span data-stu-id="da7af-302">item</span></span> | <span data-ttu-id="da7af-303">price</span><span class="sxs-lookup"><span data-stu-id="da7af-303">price</span></span> | <span data-ttu-id="da7af-304">Kedvezmény</span><span class="sxs-lookup"><span data-stu-id="da7af-304">Discount</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="da7af-305">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-305">1111</span></span> |<span data-ttu-id="da7af-306">0</span><span class="sxs-lookup"><span data-stu-id="da7af-306">0</span></span> |<span data-ttu-id="da7af-307">123</span><span class="sxs-lookup"><span data-stu-id="da7af-307">123</span></span> |<span data-ttu-id="da7af-308">a toaster</span><span class="sxs-lookup"><span data-stu-id="da7af-308">toaster</span></span> |<span data-ttu-id="da7af-309">456</span><span class="sxs-lookup"><span data-stu-id="da7af-309">456</span></span> |<span data-ttu-id="da7af-310">0.2</span><span class="sxs-lookup"><span data-stu-id="da7af-310">0.2</span></span> |
| <span data-ttu-id="da7af-311">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-311">1111</span></span> |<span data-ttu-id="da7af-312">1</span><span class="sxs-lookup"><span data-stu-id="da7af-312">1</span></span> |<span data-ttu-id="da7af-313">124</span><span class="sxs-lookup"><span data-stu-id="da7af-313">124</span></span> |<span data-ttu-id="da7af-314">Helyezzük</span><span class="sxs-lookup"><span data-stu-id="da7af-314">oven</span></span> |<span data-ttu-id="da7af-315">1235</span><span class="sxs-lookup"><span data-stu-id="da7af-315">1235</span></span> |<span data-ttu-id="da7af-316">0.2</span><span class="sxs-lookup"><span data-stu-id="da7af-316">0.2</span></span> |
| <span data-ttu-id="da7af-317">2222</span><span class="sxs-lookup"><span data-stu-id="da7af-317">2222</span></span> |<span data-ttu-id="da7af-318">0</span><span class="sxs-lookup"><span data-stu-id="da7af-318">0</span></span> |<span data-ttu-id="da7af-319">135</span><span class="sxs-lookup"><span data-stu-id="da7af-319">135</span></span> |<span data-ttu-id="da7af-320">kombinált hűtőszekrények</span><span class="sxs-lookup"><span data-stu-id="da7af-320">fridge</span></span> |<span data-ttu-id="da7af-321">12543</span><span class="sxs-lookup"><span data-stu-id="da7af-321">12543</span></span> |<span data-ttu-id="da7af-322">0.0</span><span class="sxs-lookup"><span data-stu-id="da7af-322">0.0</span></span> |

<span data-ttu-id="da7af-323">"ExampleTable_Ratings". tábla:</span><span class="sxs-lookup"><span data-stu-id="da7af-323">Table “ExampleTable_Ratings”:</span></span>

| <span data-ttu-id="da7af-324">_id</span><span class="sxs-lookup"><span data-stu-id="da7af-324">_id</span></span> | <span data-ttu-id="da7af-325">ExampleTable_Ratings_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="da7af-325">ExampleTable_Ratings_dim1_idx</span></span> | <span data-ttu-id="da7af-326">ExampleTable_Ratings</span><span class="sxs-lookup"><span data-stu-id="da7af-326">ExampleTable_Ratings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="da7af-327">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-327">1111</span></span> |<span data-ttu-id="da7af-328">0</span><span class="sxs-lookup"><span data-stu-id="da7af-328">0</span></span> |<span data-ttu-id="da7af-329">5</span><span class="sxs-lookup"><span data-stu-id="da7af-329">5</span></span> |
| <span data-ttu-id="da7af-330">1111</span><span class="sxs-lookup"><span data-stu-id="da7af-330">1111</span></span> |<span data-ttu-id="da7af-331">1</span><span class="sxs-lookup"><span data-stu-id="da7af-331">1</span></span> |<span data-ttu-id="da7af-332">6</span><span class="sxs-lookup"><span data-stu-id="da7af-332">6</span></span> |
| <span data-ttu-id="da7af-333">2222</span><span class="sxs-lookup"><span data-stu-id="da7af-333">2222</span></span> |<span data-ttu-id="da7af-334">0</span><span class="sxs-lookup"><span data-stu-id="da7af-334">0</span></span> |<span data-ttu-id="da7af-335">1</span><span class="sxs-lookup"><span data-stu-id="da7af-335">1</span></span> |
| <span data-ttu-id="da7af-336">2222</span><span class="sxs-lookup"><span data-stu-id="da7af-336">2222</span></span> |<span data-ttu-id="da7af-337">1</span><span class="sxs-lookup"><span data-stu-id="da7af-337">1</span></span> |<span data-ttu-id="da7af-338">2</span><span class="sxs-lookup"><span data-stu-id="da7af-338">2</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="da7af-339">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="da7af-339">Map source to sink columns</span></span>
<span data-ttu-id="da7af-340">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="da7af-340">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="da7af-341">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="da7af-341">Repeatable read from relational sources</span></span>
<span data-ttu-id="da7af-342">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="da7af-342">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="da7af-343">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="da7af-343">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="da7af-344">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="da7af-344">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="da7af-345">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="da7af-345">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="da7af-346">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="da7af-346">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="da7af-347">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="da7af-347">Performance and Tuning</span></span>
<span data-ttu-id="da7af-348">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="da7af-348">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da7af-349">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da7af-349">Next Steps</span></span>
<span data-ttu-id="da7af-350">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk részletes utasításokat az adatok folyamat létrehozása, amely mozgatja az adatokat kereshet ki egy a helyszíni adatok Azure adattárolóihoz.</span><span class="sxs-lookup"><span data-stu-id="da7af-350">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions for creating a data pipeline that moves data from an on-premises data store to an Azure data store.</span></span>
