---
title: "a Data Factory használatával Cassandra aaaMove adatok |} Microsoft Docs"
description: "További információk a hogyan toomove adatait egy helyszíni Cassandra adatbázis-Azure Data Factory használatával."
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
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="8b897-103">Adatok áthelyezése az Azure Data Factory használatával a helyszíni Cassandra adatbázisból</span><span class="sxs-lookup"><span data-stu-id="8b897-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="8b897-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove az adatbázisból a helyszíni Cassandra a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8b897-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Cassandra database.</span></span> <span data-ttu-id="8b897-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="8b897-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="8b897-106">Egy helyszíni Cassandra adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="8b897-106">You can copy data from an on-premises Cassandra data store tooany supported sink data store.</span></span> <span data-ttu-id="8b897-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="8b897-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="8b897-108">Adat-előállító jelenleg csak egy Cassandra adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooa Cassandra adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="8b897-108">Data factory currently supports only moving data from a Cassandra data store tooother data stores, but not for moving data from other data stores tooa Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="8b897-109">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="8b897-109">Supported versions</span></span>
<span data-ttu-id="8b897-110">hello Cassandra összekötő támogatja a következő verziói Cassandra hello: 2.X.</span><span class="sxs-lookup"><span data-stu-id="8b897-110">hello Cassandra connector supports hello following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b897-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8b897-111">Prerequisites</span></span>
<span data-ttu-id="8b897-112">Hello Azure Data Factory szolgáltatás toobe képes tooconnect tooyour helyszíni Cassandra adatbázisához, adatkezelési átjárót kell telepítenie a hello azonos számítógépre, hogy az állomások hello adatbázis vagy egy másik számítógépre tooavoid versengő hello erőforrás az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8b897-112">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises Cassandra database, you must install a Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="8b897-113">Az adatkezelési átjáró összetevő, amely összeköti a helyszíni adatok források toocloud szolgáltatások biztonságának és kezelésének módja.</span><span class="sxs-lookup"><span data-stu-id="8b897-113">Data Management Gateway is a component that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="8b897-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="8b897-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="8b897-115">Lásd: [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk hello átjáró egy adatok adatcsatorna toomove adatok beállításának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="8b897-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="8b897-116">Hello átjáró tooconnect tooa Cassandra adatbázis kell használnia, akkor is, ha hello adatbázis egy hello felhőben, például egy Azure IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="8b897-116">You must use hello gateway tooconnect tooa Cassandra database even if hello database is hosted in hello cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="8b897-117">Hello átjáró lehet Y hello ugyanazon VM állomások hello adatbázis, vagy egy külön virtuális gépre mindaddig hello átjáró képes kapcsolódni toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8b897-117">Y You can have hello gateway on hello same VM that hosts hello database or on a separate VM as long as hello gateway can connect toohello database.</span></span>  

<span data-ttu-id="8b897-118">Hello átjáró telepítésekor automatikusan telepíti a Microsoft Cassandra ODBC használt tooconnect tooCassandra-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="8b897-118">When you install hello gateway, it automatically installs a Microsoft Cassandra ODBC driver used tooconnect tooCassandra database.</span></span> <span data-ttu-id="8b897-119">Emiatt nem kell toomanually hello-átjáró számítógépén bármely illesztőprogram telepítése a hello Cassandra adatbázisból származó adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="8b897-119">Therefore, you don't need toomanually install any driver on hello gateway machine when copying data from hello Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="8b897-120">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="8b897-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8b897-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8b897-121">Getting started</span></span>
<span data-ttu-id="8b897-122">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="8b897-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="8b897-123">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="8b897-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="8b897-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="8b897-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="8b897-125">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="8b897-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="8b897-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="8b897-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="8b897-127">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="8b897-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="8b897-128">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="8b897-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="8b897-129">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="8b897-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="8b897-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="8b897-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="8b897-131">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="8b897-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="8b897-132">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="8b897-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="8b897-133">Az adat-előállító entitások, amelyek egy helyszíni Cassandra adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="8b897-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="8b897-134">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooa Cassandra adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="8b897-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="8b897-135">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="8b897-135">Linked service properties</span></span>
<span data-ttu-id="8b897-136">a következő táblázat hello biztosít JSON elemek adott tooCassandra kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="8b897-136">hello following table provides description for JSON elements specific tooCassandra linked service.</span></span>

| <span data-ttu-id="8b897-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b897-137">Property</span></span> | <span data-ttu-id="8b897-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b897-138">Description</span></span> | <span data-ttu-id="8b897-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b897-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b897-140">type</span><span class="sxs-lookup"><span data-stu-id="8b897-140">type</span></span> |<span data-ttu-id="8b897-141">hello type tulajdonságot kell beállítani: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="8b897-141">hello type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="8b897-142">Igen</span><span class="sxs-lookup"><span data-stu-id="8b897-142">Yes</span></span> |
| <span data-ttu-id="8b897-143">állomás</span><span class="sxs-lookup"><span data-stu-id="8b897-143">host</span></span> |<span data-ttu-id="8b897-144">Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.</span><span class="sxs-lookup"><span data-stu-id="8b897-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="8b897-145">IP-címek vagy nevek tooconnect tooall kiszolgálók vesszővel tagolt listáját adja meg a egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="8b897-145">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="8b897-146">Igen</span><span class="sxs-lookup"><span data-stu-id="8b897-146">Yes</span></span> |
| <span data-ttu-id="8b897-147">port</span><span class="sxs-lookup"><span data-stu-id="8b897-147">port</span></span> |<span data-ttu-id="8b897-148">hello hello Cassandra server TCP-port toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="8b897-148">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="8b897-149">Nem, alapértelmezett érték: 9042</span><span class="sxs-lookup"><span data-stu-id="8b897-149">No, default value: 9042</span></span> |
| <span data-ttu-id="8b897-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="8b897-150">authenticationType</span></span> |<span data-ttu-id="8b897-151">Basic vagy Anonymous</span><span class="sxs-lookup"><span data-stu-id="8b897-151">Basic, or Anonymous</span></span> |<span data-ttu-id="8b897-152">Igen</span><span class="sxs-lookup"><span data-stu-id="8b897-152">Yes</span></span> |
| <span data-ttu-id="8b897-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="8b897-153">username</span></span> |<span data-ttu-id="8b897-154">Adja meg a hello felhasználói fiókhoz tartozó felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="8b897-154">Specify user name for hello user account.</span></span> |<span data-ttu-id="8b897-155">Igen, ha authenticationType tooBasic van beállítva.</span><span class="sxs-lookup"><span data-stu-id="8b897-155">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="8b897-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="8b897-156">password</span></span> |<span data-ttu-id="8b897-157">Adja meg a hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="8b897-157">Specify password for hello user account.</span></span> |<span data-ttu-id="8b897-158">Igen, ha authenticationType tooBasic van beállítva.</span><span class="sxs-lookup"><span data-stu-id="8b897-158">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="8b897-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="8b897-159">gatewayName</span></span> |<span data-ttu-id="8b897-160">hello hello átjáró, amely használt tooconnect toohello helyszíni Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="8b897-160">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="8b897-161">Igen</span><span class="sxs-lookup"><span data-stu-id="8b897-161">Yes</span></span> |
| <span data-ttu-id="8b897-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="8b897-162">encryptedCredential</span></span> |<span data-ttu-id="8b897-163">A hitelesítő adatok hello átjáró titkosítja.</span><span class="sxs-lookup"><span data-stu-id="8b897-163">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="8b897-164">Nem</span><span class="sxs-lookup"><span data-stu-id="8b897-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="8b897-165">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8b897-165">Dataset properties</span></span>
<span data-ttu-id="8b897-166">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b897-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="8b897-167">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="8b897-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="8b897-168">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8b897-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="8b897-169">hello typeProperties szakasz típusú adatkészlet **CassandraTable** rendelkezik hello következő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8b897-169">hello typeProperties section for dataset of type **CassandraTable** has hello following properties</span></span>

| <span data-ttu-id="8b897-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b897-170">Property</span></span> | <span data-ttu-id="8b897-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b897-171">Description</span></span> | <span data-ttu-id="8b897-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b897-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b897-173">kulcstérértesítések használatával</span><span class="sxs-lookup"><span data-stu-id="8b897-173">keyspace</span></span> |<span data-ttu-id="8b897-174">Hello kulcstérértesítések használatával vagy a séma Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="8b897-174">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="8b897-175">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="8b897-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="8b897-176">tableName</span><span class="sxs-lookup"><span data-stu-id="8b897-176">tableName</span></span> |<span data-ttu-id="8b897-177">Hello tábla Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="8b897-177">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="8b897-178">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="8b897-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="8b897-179">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8b897-179">Copy activity properties</span></span>
<span data-ttu-id="8b897-180">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b897-180">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="8b897-181">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8b897-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="8b897-182">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="8b897-182">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="8b897-183">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="8b897-183">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="8b897-184">Ha a forrás típusa van **CassandraSource**, typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="8b897-184">When source is of type **CassandraSource**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="8b897-185">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b897-185">Property</span></span> | <span data-ttu-id="8b897-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b897-186">Description</span></span> | <span data-ttu-id="8b897-187">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="8b897-187">Allowed values</span></span> | <span data-ttu-id="8b897-188">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b897-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8b897-189">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="8b897-189">query</span></span> |<span data-ttu-id="8b897-190">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="8b897-190">Use hello custom query tooread data.</span></span> |<span data-ttu-id="8b897-191">SQL-92 vagy CQL lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="8b897-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="8b897-192">Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="8b897-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="8b897-193">SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** tooquery kívánt toorepresent hello tábla.</span><span class="sxs-lookup"><span data-stu-id="8b897-193">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="8b897-194">Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával).</span><span class="sxs-lookup"><span data-stu-id="8b897-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="8b897-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="8b897-195">consistencyLevel</span></span> |<span data-ttu-id="8b897-196">hello konzisztencia szint határozza meg, hány replikák tooa olvasási kérelem kell válaszolnia kell a visszatérésre adatok toohello ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8b897-196">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="8b897-197">Cassandra ellenőrzések hello replikák megadott számú, az adatok toosatisfy hello olvasni a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="8b897-197">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="8b897-198">EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="8b897-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="8b897-199">Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="8b897-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="8b897-200">Nem.</span><span class="sxs-lookup"><span data-stu-id="8b897-200">No.</span></span> <span data-ttu-id="8b897-201">Alapértelmezett érték: egyet.</span><span class="sxs-lookup"><span data-stu-id="8b897-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a><span data-ttu-id="8b897-202">JSON-példa: adatok másolása az Cassandra tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="8b897-202">JSON example: Copy data from Cassandra tooAzure Blob</span></span>
<span data-ttu-id="8b897-203">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8b897-203">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="8b897-204">Azt illusztrálja, hogyan toocopy adatait egy helyszíni Cassandra adatbázis tooan Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8b897-204">It shows how toocopy data from an on-premises Cassandra database tooan Azure Blob Storage.</span></span> <span data-ttu-id="8b897-205">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="8b897-205">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8b897-206">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="8b897-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="8b897-207">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="8b897-207">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="8b897-208">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="8b897-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="8b897-209">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8b897-209">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="8b897-210">A társított szolgáltatás típusa [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b897-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="8b897-211">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b897-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="8b897-212">Bemeneti [dataset](data-factory-create-datasets.md) típusú [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b897-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="8b897-213">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b897-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="8b897-214">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [CassandraSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="8b897-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="8b897-215">**Cassandra társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="8b897-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="8b897-216">Ez a példa hello **Cassandra** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b897-216">This example uses hello **Cassandra** linked service.</span></span> <span data-ttu-id="8b897-217">Lásd: [Cassandra társított szolgáltatás](#linked-service-properties) szolgáltatásnak által támogatott hello tulajdonságok szakaszát.</span><span class="sxs-lookup"><span data-stu-id="8b897-217">See [Cassandra linked service](#linked-service-properties) section for hello properties supported by this linked service.</span></span>  

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

<span data-ttu-id="8b897-218">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="8b897-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="8b897-219">**Cassandra bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="8b897-219">**Cassandra input dataset:**</span></span>

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

<span data-ttu-id="8b897-220">Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="8b897-220">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="8b897-221">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="8b897-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="8b897-222">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="8b897-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="8b897-223">**Másolási tevékenység Cassandra és Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="8b897-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="8b897-224">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="8b897-224">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="8b897-225">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**CassandraSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8b897-225">In hello pipeline JSON definition, hello **source** type is set too**CassandraSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="8b897-226">Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) hello RelationalSource által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="8b897-226">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties supported by hello RelationalSource.</span></span>

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
            "description": "Copy from Cassandra tooan Azure blob",
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

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="8b897-227">Típus identitástagok esetén Cassandra</span><span class="sxs-lookup"><span data-stu-id="8b897-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="8b897-228">Cassandra típusa</span><span class="sxs-lookup"><span data-stu-id="8b897-228">Cassandra Type</span></span> | <span data-ttu-id="8b897-229">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="8b897-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="8b897-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="8b897-230">ASCII</span></span> |<span data-ttu-id="8b897-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b897-231">String</span></span> |
| <span data-ttu-id="8b897-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="8b897-232">BIGINT</span></span> |<span data-ttu-id="8b897-233">Int64</span><span class="sxs-lookup"><span data-stu-id="8b897-233">Int64</span></span> |
| <span data-ttu-id="8b897-234">A BLOB</span><span class="sxs-lookup"><span data-stu-id="8b897-234">BLOB</span></span> |<span data-ttu-id="8b897-235">Byte]</span><span class="sxs-lookup"><span data-stu-id="8b897-235">Byte[]</span></span> |
| <span data-ttu-id="8b897-236">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="8b897-236">BOOLEAN</span></span> |<span data-ttu-id="8b897-237">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="8b897-237">Boolean</span></span> |
| <span data-ttu-id="8b897-238">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="8b897-238">DECIMAL</span></span> |<span data-ttu-id="8b897-239">Decimális</span><span class="sxs-lookup"><span data-stu-id="8b897-239">Decimal</span></span> |
| <span data-ttu-id="8b897-240">DUPLA</span><span class="sxs-lookup"><span data-stu-id="8b897-240">DOUBLE</span></span> |<span data-ttu-id="8b897-241">Dupla</span><span class="sxs-lookup"><span data-stu-id="8b897-241">Double</span></span> |
| <span data-ttu-id="8b897-242">LEBEGŐPONTOS</span><span class="sxs-lookup"><span data-stu-id="8b897-242">FLOAT</span></span> |<span data-ttu-id="8b897-243">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="8b897-243">Single</span></span> |
| <span data-ttu-id="8b897-244">INET</span><span class="sxs-lookup"><span data-stu-id="8b897-244">INET</span></span> |<span data-ttu-id="8b897-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b897-245">String</span></span> |
| <span data-ttu-id="8b897-246">INT</span><span class="sxs-lookup"><span data-stu-id="8b897-246">INT</span></span> |<span data-ttu-id="8b897-247">Int32</span><span class="sxs-lookup"><span data-stu-id="8b897-247">Int32</span></span> |
| <span data-ttu-id="8b897-248">SZÖVEG</span><span class="sxs-lookup"><span data-stu-id="8b897-248">TEXT</span></span> |<span data-ttu-id="8b897-249">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b897-249">String</span></span> |
| <span data-ttu-id="8b897-250">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="8b897-250">TIMESTAMP</span></span> |<span data-ttu-id="8b897-251">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b897-251">DateTime</span></span> |
| <span data-ttu-id="8b897-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="8b897-252">TIMEUUID</span></span> |<span data-ttu-id="8b897-253">GUID</span><span class="sxs-lookup"><span data-stu-id="8b897-253">Guid</span></span> |
| <span data-ttu-id="8b897-254">UUID</span><span class="sxs-lookup"><span data-stu-id="8b897-254">UUID</span></span> |<span data-ttu-id="8b897-255">GUID</span><span class="sxs-lookup"><span data-stu-id="8b897-255">Guid</span></span> |
| <span data-ttu-id="8b897-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="8b897-256">VARCHAR</span></span> |<span data-ttu-id="8b897-257">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b897-257">String</span></span> |
| <span data-ttu-id="8b897-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="8b897-258">VARINT</span></span> |<span data-ttu-id="8b897-259">Decimális</span><span class="sxs-lookup"><span data-stu-id="8b897-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="8b897-260">A gyűjtemény típusa (térkép, set, lista stb.), tekintse meg a túl[virtuális tábla használatával Cassandra gyűjteménytípusok együttműködve](#work-with-collections-using-virtual-table) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b897-260">For collection types (map, set, list, etc.), refer too[Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="8b897-261">Felhasználó által definiált típusok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8b897-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="8b897-262">Bináris oszlop és karakterlánc-oszlopnak hosszúságú hello hossza nem lehet nagyobb, mint 4000.</span><span class="sxs-lookup"><span data-stu-id="8b897-262">hello length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="8b897-263">Virtuális tábla használatával gyűjtemények használata</span><span class="sxs-lookup"><span data-stu-id="8b897-263">Work with collections using virtual table</span></span>
<span data-ttu-id="8b897-264">Az Azure Data Factory használ egy beépített ODBC illesztőprogram tooconnect tooand másolása az Cassandra adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="8b897-264">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your Cassandra database.</span></span> <span data-ttu-id="8b897-265">A térkép, a készlet és a lista gyűjtemény típusú hello illesztőprogram hello adatok renormalizes megfelelő virtuális táblákba.</span><span class="sxs-lookup"><span data-stu-id="8b897-265">For collection types including map, set and list, hello driver renormalizes hello data into corresponding virtual tables.</span></span> <span data-ttu-id="8b897-266">Pontosabban Ha egy tábla gyűjtemény oszlopot tartalmaz, a hello illesztőprogram a következő virtuális táblák hello állít elő:</span><span class="sxs-lookup"><span data-stu-id="8b897-266">Specifically, if a table contains any collection columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="8b897-267">A **alaptábla**, amely tartalmazza hello hello valós táblából hello gyűjtemény oszlopok ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b897-267">A **base table**, which contains hello same data as hello real table except for hello collection columns.</span></span> <span data-ttu-id="8b897-268">hello alaptábla hello ugyanazt a nevet használja, mint hello valós táblázatot, amely azt jelöli.</span><span class="sxs-lookup"><span data-stu-id="8b897-268">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="8b897-269">A **virtuális tábla** gyűjtemény oszlopok, amely bővíti a beágyazott hello adatok.</span><span class="sxs-lookup"><span data-stu-id="8b897-269">A **virtual table** for each collection column, which expands hello nested data.</span></span> <span data-ttu-id="8b897-270">hello virtuális táblákat, amelyek megfelelnek a gyűjtemények használatával hello hello valós tábla neve, az elválasztó elnevezése "*vt*" és a hello hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="8b897-270">hello virtual tables that represent collections are named using hello name of hello real table, a separator “*vt*” and hello name of hello column.</span></span>

<span data-ttu-id="8b897-271">Virtuális táblák tekintse meg a hello valós tábla toohello adatokat, lehetővé teszi hello illesztőprogram tooaccess hello nem normalizált adatok.</span><span class="sxs-lookup"><span data-stu-id="8b897-271">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="8b897-272">Lásd például című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8b897-272">See Example section for details.</span></span> <span data-ttu-id="8b897-273">Cassandra gyűjtemények hello tartalmának lekérdezése és hello virtuális tábla érheti el.</span><span class="sxs-lookup"><span data-stu-id="8b897-273">You can access hello content of Cassandra collections by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="8b897-274">Használhatja a hello [másolása varázsló](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively nézet hello hello virtuális táblázatot is Cassandra adatbázis táblák listáját és hello található adatok megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="8b897-274">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in Cassandra database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="8b897-275">Is hello másolása varázsló az olyan lekérdezést, és toosee hello eredmény ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="8b897-275">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="8b897-276">Példa</span><span class="sxs-lookup"><span data-stu-id="8b897-276">Example</span></span>
<span data-ttu-id="8b897-277">Például hello következő "ExampleTable", amely tartalmazza az egész elsődleges kulcsként megadott oszlop "pk_int", érték nevű szöveges oszlop, egy listaoszlop, térkép oszlop és egy oszlopkészlet ("StringSet" nevű) nevű Cassandra adatbázistábla.</span><span class="sxs-lookup"><span data-stu-id="8b897-277">For example, hello following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="8b897-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="8b897-278">pk_int</span></span> | <span data-ttu-id="8b897-279">Érték</span><span class="sxs-lookup"><span data-stu-id="8b897-279">Value</span></span> | <span data-ttu-id="8b897-280">Lista</span><span class="sxs-lookup"><span data-stu-id="8b897-280">List</span></span> | <span data-ttu-id="8b897-281">térkép</span><span class="sxs-lookup"><span data-stu-id="8b897-281">Map</span></span> | <span data-ttu-id="8b897-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="8b897-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8b897-283">1</span><span class="sxs-lookup"><span data-stu-id="8b897-283">1</span></span> |<span data-ttu-id="8b897-284">"a minta érték 1"</span><span class="sxs-lookup"><span data-stu-id="8b897-284">"sample value 1"</span></span> |<span data-ttu-id="8b897-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="8b897-285">["1", "2", "3"]</span></span> |<span data-ttu-id="8b897-286">{"S1": "a", "S2": "b"}</span><span class="sxs-lookup"><span data-stu-id="8b897-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="8b897-287">{"A", "B", "C"}</span><span class="sxs-lookup"><span data-stu-id="8b897-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="8b897-288">3</span><span class="sxs-lookup"><span data-stu-id="8b897-288">3</span></span> |<span data-ttu-id="8b897-289">"a minta 3. érték"</span><span class="sxs-lookup"><span data-stu-id="8b897-289">"sample value 3"</span></span> |<span data-ttu-id="8b897-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="8b897-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="8b897-291">{"S1": "t"}</span><span class="sxs-lookup"><span data-stu-id="8b897-291">{"S1": "t"}</span></span> |<span data-ttu-id="8b897-292">{"A", "E"}</span><span class="sxs-lookup"><span data-stu-id="8b897-292">{"A", "E"}</span></span> |

<span data-ttu-id="8b897-293">hello illesztőprogram több virtuális táblák toorepresent hoz létre a egyetlen tábla.</span><span class="sxs-lookup"><span data-stu-id="8b897-293">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="8b897-294">hello külső kulcs oszlopokra hello virtuális táblák elsődleges kulcsát alkotó hello hello valós tábla hivatkozik, és jelezze mely valós tábla sor hello virtuális tábla sorainak felel meg.</span><span class="sxs-lookup"><span data-stu-id="8b897-294">hello foreign key columns in hello virtual tables reference hello primary key columns in hello real table, and indicate which real table row hello virtual table row corresponds to.</span></span>

<span data-ttu-id="8b897-295">első virtuális tábla hello hello alaptábla "ExampleTable" nevű hello a következő táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="8b897-295">hello first virtual table is hello base table named “ExampleTable” is shown in hello following table.</span></span> <span data-ttu-id="8b897-296">hello alaptábla tartalmaz hello hello eredeti adatbázis táblából hello gyűjteményeket, amelyek ebből a táblázatból nincs megadva, és más virtuális táblák kibontva ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b897-296">hello base table contains hello same data as hello original database table except for hello collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="8b897-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="8b897-297">pk_int</span></span> | <span data-ttu-id="8b897-298">Érték</span><span class="sxs-lookup"><span data-stu-id="8b897-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="8b897-299">1</span><span class="sxs-lookup"><span data-stu-id="8b897-299">1</span></span> |<span data-ttu-id="8b897-300">"a minta érték 1"</span><span class="sxs-lookup"><span data-stu-id="8b897-300">"sample value 1"</span></span> |
| <span data-ttu-id="8b897-301">3</span><span class="sxs-lookup"><span data-stu-id="8b897-301">3</span></span> |<span data-ttu-id="8b897-302">"a minta 3. érték"</span><span class="sxs-lookup"><span data-stu-id="8b897-302">"sample value 3"</span></span> |

<span data-ttu-id="8b897-303">hello alábbi táblázatokban hello virtuális táblákhoz renormalize hello lista, térkép és StringSet oszlopok hello adatait.</span><span class="sxs-lookup"><span data-stu-id="8b897-303">hello following tables show hello virtual tables that renormalize hello data from hello List, Map, and StringSet columns.</span></span> <span data-ttu-id="8b897-304">hello oszlopok kiderül, hogy a "_index" vagy "_kulcsvédelmi" adja meg hello adatainak hello eredeti lista vagy a térkép hello pozícióját.</span><span class="sxs-lookup"><span data-stu-id="8b897-304">hello columns with names that end with “_index” or “_key” indicate hello position of hello data within hello original list or map.</span></span> <span data-ttu-id="8b897-305">hello oszlopok kiderül, hogy a "_value" végződhet hello gyűjtemény kibontva hello adatait tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="8b897-305">hello columns with names that end with “_value” contain hello expanded data from hello collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="8b897-306">"ExampleTable_vt_List". tábla:</span><span class="sxs-lookup"><span data-stu-id="8b897-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="8b897-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="8b897-307">pk_int</span></span> | <span data-ttu-id="8b897-308">List_index</span><span class="sxs-lookup"><span data-stu-id="8b897-308">List_index</span></span> | <span data-ttu-id="8b897-309">List_value</span><span class="sxs-lookup"><span data-stu-id="8b897-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b897-310">1</span><span class="sxs-lookup"><span data-stu-id="8b897-310">1</span></span> |<span data-ttu-id="8b897-311">0</span><span class="sxs-lookup"><span data-stu-id="8b897-311">0</span></span> |<span data-ttu-id="8b897-312">1</span><span class="sxs-lookup"><span data-stu-id="8b897-312">1</span></span> |
| <span data-ttu-id="8b897-313">1</span><span class="sxs-lookup"><span data-stu-id="8b897-313">1</span></span> |<span data-ttu-id="8b897-314">1</span><span class="sxs-lookup"><span data-stu-id="8b897-314">1</span></span> |<span data-ttu-id="8b897-315">2</span><span class="sxs-lookup"><span data-stu-id="8b897-315">2</span></span> |
| <span data-ttu-id="8b897-316">1</span><span class="sxs-lookup"><span data-stu-id="8b897-316">1</span></span> |<span data-ttu-id="8b897-317">2</span><span class="sxs-lookup"><span data-stu-id="8b897-317">2</span></span> |<span data-ttu-id="8b897-318">3</span><span class="sxs-lookup"><span data-stu-id="8b897-318">3</span></span> |
| <span data-ttu-id="8b897-319">3</span><span class="sxs-lookup"><span data-stu-id="8b897-319">3</span></span> |<span data-ttu-id="8b897-320">0</span><span class="sxs-lookup"><span data-stu-id="8b897-320">0</span></span> |<span data-ttu-id="8b897-321">100</span><span class="sxs-lookup"><span data-stu-id="8b897-321">100</span></span> |
| <span data-ttu-id="8b897-322">3</span><span class="sxs-lookup"><span data-stu-id="8b897-322">3</span></span> |<span data-ttu-id="8b897-323">1</span><span class="sxs-lookup"><span data-stu-id="8b897-323">1</span></span> |<span data-ttu-id="8b897-324">101</span><span class="sxs-lookup"><span data-stu-id="8b897-324">101</span></span> |
| <span data-ttu-id="8b897-325">3</span><span class="sxs-lookup"><span data-stu-id="8b897-325">3</span></span> |<span data-ttu-id="8b897-326">2</span><span class="sxs-lookup"><span data-stu-id="8b897-326">2</span></span> |<span data-ttu-id="8b897-327">102</span><span class="sxs-lookup"><span data-stu-id="8b897-327">102</span></span> |
| <span data-ttu-id="8b897-328">3</span><span class="sxs-lookup"><span data-stu-id="8b897-328">3</span></span> |<span data-ttu-id="8b897-329">3</span><span class="sxs-lookup"><span data-stu-id="8b897-329">3</span></span> |<span data-ttu-id="8b897-330">103</span><span class="sxs-lookup"><span data-stu-id="8b897-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="8b897-331">"ExampleTable_vt_Map". tábla:</span><span class="sxs-lookup"><span data-stu-id="8b897-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="8b897-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="8b897-332">pk_int</span></span> | <span data-ttu-id="8b897-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="8b897-333">Map_key</span></span> | <span data-ttu-id="8b897-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="8b897-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b897-335">1</span><span class="sxs-lookup"><span data-stu-id="8b897-335">1</span></span> |<span data-ttu-id="8b897-336">S1</span><span class="sxs-lookup"><span data-stu-id="8b897-336">S1</span></span> |<span data-ttu-id="8b897-337">A</span><span class="sxs-lookup"><span data-stu-id="8b897-337">A</span></span> |
| <span data-ttu-id="8b897-338">1</span><span class="sxs-lookup"><span data-stu-id="8b897-338">1</span></span> |<span data-ttu-id="8b897-339">S2</span><span class="sxs-lookup"><span data-stu-id="8b897-339">S2</span></span> |<span data-ttu-id="8b897-340">B</span><span class="sxs-lookup"><span data-stu-id="8b897-340">b</span></span> |
| <span data-ttu-id="8b897-341">3</span><span class="sxs-lookup"><span data-stu-id="8b897-341">3</span></span> |<span data-ttu-id="8b897-342">S1</span><span class="sxs-lookup"><span data-stu-id="8b897-342">S1</span></span> |<span data-ttu-id="8b897-343">T</span><span class="sxs-lookup"><span data-stu-id="8b897-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="8b897-344">"ExampleTable_vt_StringSet". tábla:</span><span class="sxs-lookup"><span data-stu-id="8b897-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="8b897-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="8b897-345">pk_int</span></span> | <span data-ttu-id="8b897-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="8b897-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="8b897-347">1</span><span class="sxs-lookup"><span data-stu-id="8b897-347">1</span></span> |<span data-ttu-id="8b897-348">A</span><span class="sxs-lookup"><span data-stu-id="8b897-348">A</span></span> |
| <span data-ttu-id="8b897-349">1</span><span class="sxs-lookup"><span data-stu-id="8b897-349">1</span></span> |<span data-ttu-id="8b897-350">B</span><span class="sxs-lookup"><span data-stu-id="8b897-350">B</span></span> |
| <span data-ttu-id="8b897-351">1</span><span class="sxs-lookup"><span data-stu-id="8b897-351">1</span></span> |<span data-ttu-id="8b897-352">C#</span><span class="sxs-lookup"><span data-stu-id="8b897-352">C</span></span> |
| <span data-ttu-id="8b897-353">3</span><span class="sxs-lookup"><span data-stu-id="8b897-353">3</span></span> |<span data-ttu-id="8b897-354">A</span><span class="sxs-lookup"><span data-stu-id="8b897-354">A</span></span> |
| <span data-ttu-id="8b897-355">3</span><span class="sxs-lookup"><span data-stu-id="8b897-355">3</span></span> |<span data-ttu-id="8b897-356">E</span><span class="sxs-lookup"><span data-stu-id="8b897-356">E</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="8b897-357">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="8b897-357">Map source toosink columns</span></span>
<span data-ttu-id="8b897-358">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="8b897-358">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="8b897-359">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="8b897-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="8b897-360">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="8b897-360">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="8b897-361">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="8b897-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="8b897-362">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="8b897-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="8b897-363">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="8b897-363">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="8b897-364">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="8b897-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="8b897-365">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="8b897-365">Performance and Tuning</span></span>
<span data-ttu-id="8b897-366">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="8b897-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
