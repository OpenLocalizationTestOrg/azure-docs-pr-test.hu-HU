---
title: a Data Factory aaaMove Salesforce adatokat |} Microsoft Docs
description: "Megtudhatja, hogyan toomove Azure Data Factory használatával Salesforce adatait."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="f7df1-103">Adatok áthelyezése Salesforce Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f7df1-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="f7df1-104">Ez a cikk ismerteti, hogyan használhatja az az Azure data factory toocopy adatok Salesforce tooany adattárból hello fogadó oszlopában az hello felsorolt másolási tevékenység [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="f7df1-104">This article outlines how you can use Copy Activity in an Azure data factory toocopy data from Salesforce tooany data store that is listed under hello Sink column in hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="f7df1-105">Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.</span><span class="sxs-lookup"><span data-stu-id="f7df1-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="f7df1-106">Az Azure Data Factory jelenleg csak az adatok áthelyezése Salesforce túl[fogadó adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), azonban nem támogatja egyéb adatok tárolók tooSalesforce áthelyezése adatait.</span><span class="sxs-lookup"><span data-stu-id="f7df1-106">Azure Data Factory currently supports only moving data from Salesforce too[supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores tooSalesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="f7df1-107">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="f7df1-107">Supported versions</span></span>
<span data-ttu-id="f7df1-108">Ez az összekötő támogatja a következő Salesforce kiadása hello: Developer Edition, Professional Edition, Enterprise Edition vagy korlátlan kiadását.</span><span class="sxs-lookup"><span data-stu-id="f7df1-108">This connector supports hello following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="f7df1-109">És a Salesforce éles, védőfal és az egyéni tartomány másolását támogatja.</span><span class="sxs-lookup"><span data-stu-id="f7df1-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7df1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f7df1-110">Prerequisites</span></span>
* <span data-ttu-id="f7df1-111">API-engedély engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f7df1-111">API permission must be enabled.</span></span> <span data-ttu-id="f7df1-112">Lásd: [hogyan engedélyezhető az engedélycsoport által a Salesforce-ban API-hozzáférés?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="f7df1-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="f7df1-113">Salesforce tooon helyszíni adattárolókhoz toocopy adatait, rendelkeznie kell legalább adatok felügyeleti átjáró 2.0-s verziójával a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="f7df1-113">toocopy data from Salesforce tooon-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="f7df1-114">Salesforce kérelmekre vonatkozó korlátok</span><span class="sxs-lookup"><span data-stu-id="f7df1-114">Salesforce request limits</span></span>
<span data-ttu-id="f7df1-115">Salesforce rendelkezik mind az API-kérelmek teljes száma, és a egyidejű API-kérés.</span><span class="sxs-lookup"><span data-stu-id="f7df1-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="f7df1-116">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="f7df1-116">Note hello following points:</span></span>

- <span data-ttu-id="f7df1-117">Ha hello egyidejű kérelmek száma meghaladja hello, sávszélesség-szabályozás következik be, és látni fogja a véletlen hibákat.</span><span class="sxs-lookup"><span data-stu-id="f7df1-117">If hello number of concurrent requests exceeds hello limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="f7df1-118">Ha hello kérelmek teljes száma túllépi hello, hello Salesforce-fiókban le lesz tiltva 24 órán át.</span><span class="sxs-lookup"><span data-stu-id="f7df1-118">If hello total number of requests exceeds hello limit, hello Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="f7df1-119">Hello "REQUEST_LIMIT_EXCEEDED" hiba mindkét forgatókönyvet is akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="f7df1-119">You might also receive hello “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="f7df1-120">Hello hello "API kérelmekre vonatkozó korlátok" szakaszában talál [Salesforce fejlesztői korlátok](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="f7df1-120">See hello "API Request Limits" section in hello [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f7df1-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f7df1-121">Getting started</span></span>
<span data-ttu-id="f7df1-122">A másolási tevékenység, mely az adatok Salesforce különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f7df1-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="f7df1-123">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="f7df1-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="f7df1-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f7df1-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="f7df1-125">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="f7df1-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f7df1-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="f7df1-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f7df1-127">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="f7df1-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="f7df1-128">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f7df1-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="f7df1-129">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="f7df1-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="f7df1-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="f7df1-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f7df1-131">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="f7df1-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="f7df1-132">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="f7df1-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="f7df1-133">Az adat-előállító entitások, amelyek a Salesforce adatait használt toocopy JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f7df1-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from Salesforce, see [JSON example: Copy data from Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="f7df1-134">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooSalesforce részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="f7df1-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSalesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="f7df1-135">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="f7df1-135">Linked service properties</span></span>
<span data-ttu-id="f7df1-136">hello a következő táblázat ismerteti, amelyek adott toohello Salesforce társított szolgáltatás JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="f7df1-136">hello following table provides descriptions for JSON elements that are specific toohello Salesforce linked service.</span></span>

| <span data-ttu-id="f7df1-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7df1-137">Property</span></span> | <span data-ttu-id="f7df1-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7df1-138">Description</span></span> | <span data-ttu-id="f7df1-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7df1-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f7df1-140">type</span><span class="sxs-lookup"><span data-stu-id="f7df1-140">type</span></span> |<span data-ttu-id="f7df1-141">hello type tulajdonságot kell beállítani: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="f7df1-141">hello type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="f7df1-142">Igen</span><span class="sxs-lookup"><span data-stu-id="f7df1-142">Yes</span></span> |
| <span data-ttu-id="f7df1-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="f7df1-143">environmentUrl</span></span> | <span data-ttu-id="f7df1-144">Adja meg a hello URL-címet a Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="f7df1-144">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="f7df1-145">-Alapértelmezett érték a "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="f7df1-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="f7df1-146">-toocopy adatok védőfalak, adja meg a "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="f7df1-146">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="f7df1-147">-toocopy adatait egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="f7df1-147">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="f7df1-148">Nem</span><span class="sxs-lookup"><span data-stu-id="f7df1-148">No</span></span> |
| <span data-ttu-id="f7df1-149">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="f7df1-149">username</span></span> |<span data-ttu-id="f7df1-150">Adjon meg egy felhasználónevet hello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="f7df1-150">Specify a user name for hello user account.</span></span> |<span data-ttu-id="f7df1-151">Igen</span><span class="sxs-lookup"><span data-stu-id="f7df1-151">Yes</span></span> |
| <span data-ttu-id="f7df1-152">jelszó</span><span class="sxs-lookup"><span data-stu-id="f7df1-152">password</span></span> |<span data-ttu-id="f7df1-153">Adjon meg egy hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f7df1-153">Specify a password for hello user account.</span></span> |<span data-ttu-id="f7df1-154">Igen</span><span class="sxs-lookup"><span data-stu-id="f7df1-154">Yes</span></span> |
| <span data-ttu-id="f7df1-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="f7df1-155">securityToken</span></span> |<span data-ttu-id="f7df1-156">Adjon meg egy biztonsági jogkivonatot hello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="f7df1-156">Specify a security token for hello user account.</span></span> <span data-ttu-id="f7df1-157">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást tooreset/get egy biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="f7df1-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="f7df1-158">biztonsági jogkivonatok kapcsolatos toolearn általában, lásd: [biztonsági és hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="f7df1-158">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="f7df1-159">Igen</span><span class="sxs-lookup"><span data-stu-id="f7df1-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="f7df1-160">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f7df1-160">Dataset properties</span></span>
<span data-ttu-id="f7df1-161">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f7df1-161">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f7df1-162">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla és így tovább).</span><span class="sxs-lookup"><span data-stu-id="f7df1-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="f7df1-163">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f7df1-163">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="f7df1-164">a DataSet hello típusú szakasz hello typeProperties **RelationalTable** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="f7df1-164">hello typeProperties section for a dataset of hello type **RelationalTable** has hello following properties:</span></span>

| <span data-ttu-id="f7df1-165">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7df1-165">Property</span></span> | <span data-ttu-id="f7df1-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7df1-166">Description</span></span> | <span data-ttu-id="f7df1-167">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7df1-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f7df1-168">tableName</span><span class="sxs-lookup"><span data-stu-id="f7df1-168">tableName</span></span> |<span data-ttu-id="f7df1-169">A Salesforce-ban hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="f7df1-169">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="f7df1-170">Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f7df1-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f7df1-171">hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="f7df1-171">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="f7df1-173">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f7df1-173">Copy activity properties</span></span>
<span data-ttu-id="f7df1-174">Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f7df1-174">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f7df1-175">Tulajdonságok nevét, leírását, bemeneti és kimeneti táblák, valamint különböző házirendeket is tevékenységi érhető el.</span><span class="sxs-lookup"><span data-stu-id="f7df1-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="f7df1-176">hello tulajdonságok által biztosított hello typeProperties szakasz hello tevékenységekre, hello ugyanakkor, tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="f7df1-176">hello properties that are available in hello typeProperties section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="f7df1-177">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="f7df1-177">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="f7df1-178">A másolási tevékenység, ha hello adatforrás hello típusú **RelationalSource** (amely tartalmazza a Salesforce), typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="f7df1-178">In copy activity, when hello source is of hello type **RelationalSource** (which includes Salesforce), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="f7df1-179">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7df1-179">Property</span></span> | <span data-ttu-id="f7df1-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7df1-180">Description</span></span> | <span data-ttu-id="f7df1-181">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="f7df1-181">Allowed values</span></span> | <span data-ttu-id="f7df1-182">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f7df1-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f7df1-183">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="f7df1-183">query</span></span> |<span data-ttu-id="f7df1-184">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="f7df1-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="f7df1-185">Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="f7df1-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="f7df1-186">Például: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="f7df1-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="f7df1-187">Nem (ha hello **tableName** a hello **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="f7df1-187">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f7df1-188">hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="f7df1-188">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="f7df1-190">Lekérdezés tippek</span><span class="sxs-lookup"><span data-stu-id="f7df1-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="f7df1-191">Adatgyűjtés használata a where záradék található dátum és idő oszlop</span><span class="sxs-lookup"><span data-stu-id="f7df1-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="f7df1-192">Ha meg hello SOQL vagy SQL-lekérdezés, fizetési figyelmet toohello különbség a dátum és idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="f7df1-192">When specify hello SOQL or SQL query, pay attention toohello DateTime format difference.</span></span> <span data-ttu-id="f7df1-193">Példa:</span><span class="sxs-lookup"><span data-stu-id="f7df1-193">For example:</span></span>

* <span data-ttu-id="f7df1-194">**SOQL minta**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="f7df1-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="f7df1-195">**SQL-minta**:</span><span class="sxs-lookup"><span data-stu-id="f7df1-195">**SQL sample**:</span></span>
    * <span data-ttu-id="f7df1-196">**Másolása varázsló toospecify hello lekérdezéssel:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="f7df1-196">**Using copy wizard toospecify hello query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="f7df1-197">**Toospecify hello lekérdezés szerkesztése JSON használatával (escape-karakter megfelelően):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="f7df1-197">**Using JSON editing toospecify hello query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="f7df1-198">Adatok beolvasása a Salesforce-jelentés</span><span class="sxs-lookup"><span data-stu-id="f7df1-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="f7df1-199">Beolvasható adat Salesforce-jelentéseket lekérdezést megadásával `{call "<report name>"}`, például.</span><span class="sxs-lookup"><span data-stu-id="f7df1-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="f7df1-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="f7df1-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="f7df1-201">A Salesforce Lomtárból törölt rekordok visszanyerése</span><span class="sxs-lookup"><span data-stu-id="f7df1-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="f7df1-202">tooquery hello véglegesen törölve rögzíti a Salesforce Lomtárból, megadhat **"IsDeleted = 1"** a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="f7df1-202">tooquery hello soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="f7df1-203">Például:</span><span class="sxs-lookup"><span data-stu-id="f7df1-203">For example,</span></span>

* <span data-ttu-id="f7df1-204">csak hello törölt rekordok tooquery, adja meg "Válasszon * a MyTable__c **ahol IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="f7df1-204">tooquery only hello deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="f7df1-205">összes hello hello meglévő és hello törölni, beleértve rekordok tooquery adja meg "Válasszon * a MyTable__c **ahol IsDeleted = 0 vagy IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="f7df1-205">tooquery all hello records including hello existing and hello deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a><span data-ttu-id="f7df1-206">JSON-példa: adatok másolása az Salesforce tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="f7df1-206">JSON example: Copy data from Salesforce tooAzure Blob</span></span>
<span data-ttu-id="f7df1-207">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f7df1-207">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f7df1-208">Azok hogyan Salesforce tooAzure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="f7df1-208">They show how toocopy data from Salesforce tooAzure Blob Storage.</span></span> <span data-ttu-id="f7df1-209">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="f7df1-209">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="f7df1-210">Az alábbiakban hello adat-előállító összetevők, konfigurálnia kell toocreate tooimplement hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="f7df1-210">Here are hello Data Factory artifacts that you'll need toocreate tooimplement hello scenario.</span></span> <span data-ttu-id="f7df1-211">hello lista hello szakaszok ezeket a lépéseket részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="f7df1-211">hello sections that follow hello list provide details about these steps.</span></span>

* <span data-ttu-id="f7df1-212">Hello típusú társított szolgáltatás [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="f7df1-212">A linked service of hello type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="f7df1-213">Hello típusú társított szolgáltatás [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="f7df1-213">A linked service of hello type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="f7df1-214">Bemeneti [dataset](data-factory-create-datasets.md) hello típusú [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="f7df1-214">An input [dataset](data-factory-create-datasets.md) of hello type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="f7df1-215">Egy kimeneti [dataset](data-factory-create-datasets.md) hello típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="f7df1-215">An output [dataset](data-factory-create-datasets.md) of hello type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="f7df1-216">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="f7df1-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="f7df1-217">**A Salesforce társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f7df1-217">**Salesforce linked service**</span></span>

<span data-ttu-id="f7df1-218">Ez a példa hello **Salesforce** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f7df1-218">This example uses hello **Salesforce** linked service.</span></span> <span data-ttu-id="f7df1-219">Lásd: hello [Salesforce társított szolgáltatás](#linked-service-properties) szolgáltatásnak által támogatott hello tulajdonságok szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f7df1-219">See hello [Salesforce linked service](#linked-service-properties) section for hello properties that are supported by this linked service.</span></span>  <span data-ttu-id="f7df1-220">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) kapcsolatban, hogyan tooreset/get hello biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="f7df1-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get hello security token.</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
<span data-ttu-id="f7df1-221">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f7df1-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="f7df1-222">**Bemeneti Salesforce-adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="f7df1-222">**Salesforce input dataset**</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
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

<span data-ttu-id="f7df1-223">Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="f7df1-223">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7df1-224">hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="f7df1-224">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="f7df1-226">**Azure blobkimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="f7df1-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="f7df1-227">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="f7df1-227">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="f7df1-228">**A másolási tevékenység folyamat**</span><span class="sxs-lookup"><span data-stu-id="f7df1-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="f7df1-229">hello csővezeték tartalmaz másolási tevékenység során beállított toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="f7df1-229">hello pipeline contains Copy Activity, which is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="f7df1-230">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource**, és hello **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f7df1-230">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource**, and hello **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="f7df1-231">Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) hello hello RelationalSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="f7df1-231">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties that are supported by hello RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
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
> [!IMPORTANT]
> <span data-ttu-id="f7df1-232">hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="f7df1-232">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="f7df1-234">A Salesforce leképezésének</span><span class="sxs-lookup"><span data-stu-id="f7df1-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="f7df1-235">Salesforce-típus</span><span class="sxs-lookup"><span data-stu-id="f7df1-235">Salesforce type</span></span> | <span data-ttu-id="f7df1-236">. A NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="f7df1-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="f7df1-237">Automatikus szám</span><span class="sxs-lookup"><span data-stu-id="f7df1-237">Auto Number</span></span> |<span data-ttu-id="f7df1-238">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-238">String</span></span> |
| <span data-ttu-id="f7df1-239">Jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="f7df1-239">Checkbox</span></span> |<span data-ttu-id="f7df1-240">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="f7df1-240">Boolean</span></span> |
| <span data-ttu-id="f7df1-241">Currency (Pénznem)</span><span class="sxs-lookup"><span data-stu-id="f7df1-241">Currency</span></span> |<span data-ttu-id="f7df1-242">Dupla</span><span class="sxs-lookup"><span data-stu-id="f7df1-242">Double</span></span> |
| <span data-ttu-id="f7df1-243">Dátum</span><span class="sxs-lookup"><span data-stu-id="f7df1-243">Date</span></span> |<span data-ttu-id="f7df1-244">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f7df1-244">DateTime</span></span> |
| <span data-ttu-id="f7df1-245">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f7df1-245">Date/Time</span></span> |<span data-ttu-id="f7df1-246">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="f7df1-246">DateTime</span></span> |
| <span data-ttu-id="f7df1-247">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="f7df1-247">Email</span></span> |<span data-ttu-id="f7df1-248">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-248">String</span></span> |
| <span data-ttu-id="f7df1-249">Azonosító</span><span class="sxs-lookup"><span data-stu-id="f7df1-249">Id</span></span> |<span data-ttu-id="f7df1-250">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-250">String</span></span> |
| <span data-ttu-id="f7df1-251">Keresési kapcsolat</span><span class="sxs-lookup"><span data-stu-id="f7df1-251">Lookup Relationship</span></span> |<span data-ttu-id="f7df1-252">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-252">String</span></span> |
| <span data-ttu-id="f7df1-253">Többszörös kiválasztási lista</span><span class="sxs-lookup"><span data-stu-id="f7df1-253">Multi-Select Picklist</span></span> |<span data-ttu-id="f7df1-254">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-254">String</span></span> |
| <span data-ttu-id="f7df1-255">Szám</span><span class="sxs-lookup"><span data-stu-id="f7df1-255">Number</span></span> |<span data-ttu-id="f7df1-256">Dupla</span><span class="sxs-lookup"><span data-stu-id="f7df1-256">Double</span></span> |
| <span data-ttu-id="f7df1-257">Százalék</span><span class="sxs-lookup"><span data-stu-id="f7df1-257">Percent</span></span> |<span data-ttu-id="f7df1-258">Dupla</span><span class="sxs-lookup"><span data-stu-id="f7df1-258">Double</span></span> |
| <span data-ttu-id="f7df1-259">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="f7df1-259">Phone</span></span> |<span data-ttu-id="f7df1-260">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-260">String</span></span> |
| <span data-ttu-id="f7df1-261">Választási lista</span><span class="sxs-lookup"><span data-stu-id="f7df1-261">Picklist</span></span> |<span data-ttu-id="f7df1-262">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-262">String</span></span> |
| <span data-ttu-id="f7df1-263">Szöveg</span><span class="sxs-lookup"><span data-stu-id="f7df1-263">Text</span></span> |<span data-ttu-id="f7df1-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-264">String</span></span> |
| <span data-ttu-id="f7df1-265">Szövegmező</span><span class="sxs-lookup"><span data-stu-id="f7df1-265">Text Area</span></span> |<span data-ttu-id="f7df1-266">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-266">String</span></span> |
| <span data-ttu-id="f7df1-267">Szövegmező (nagy)</span><span class="sxs-lookup"><span data-stu-id="f7df1-267">Text Area (Long)</span></span> |<span data-ttu-id="f7df1-268">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-268">String</span></span> |
| <span data-ttu-id="f7df1-269">Szövegmező (gazdag)</span><span class="sxs-lookup"><span data-stu-id="f7df1-269">Text Area (Rich)</span></span> |<span data-ttu-id="f7df1-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-270">String</span></span> |
| <span data-ttu-id="f7df1-271">Szöveg (titkosítva)</span><span class="sxs-lookup"><span data-stu-id="f7df1-271">Text (Encrypted)</span></span> |<span data-ttu-id="f7df1-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-272">String</span></span> |
| <span data-ttu-id="f7df1-273">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="f7df1-273">URL</span></span> |<span data-ttu-id="f7df1-274">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f7df1-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="f7df1-275">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f7df1-275">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="f7df1-276">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="f7df1-276">Performance and tuning</span></span>
<span data-ttu-id="f7df1-277">Lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="f7df1-277">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
