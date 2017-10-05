---
title: "Adatok áthelyezése Salesforce Data Factory használatával |} Microsoft Docs"
description: "További tudnivalók az adatok mozgatása Salesforce Azure Data Factory használatával."
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
ms.openlocfilehash: 9390b992bce2dede750c3fc55b7783a6b0db678f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="99202-103">Adatok áthelyezése Salesforce Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="99202-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="99202-104">Ez a cikk ismerteti, hogyan használhatja másolási tevékenység az Azure data factoryban Salesforce bármely adattárolóhoz, amely szerepel a fogadó oszlopában az adatok másolása az [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="99202-104">This article outlines how you can use Copy Activity in an Azure data factory to copy data from Salesforce to any data store that is listed under the Sink column in the [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="99202-105">Ez a cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.</span><span class="sxs-lookup"><span data-stu-id="99202-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="99202-106">Az Azure Data Factory jelenleg csak áthelyezése adatait a Salesforce [fogadó adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), de nem támogatják az adatok áthelyezését más adatok a Salesforce tárolja.</span><span class="sxs-lookup"><span data-stu-id="99202-106">Azure Data Factory currently supports only moving data from Salesforce to [supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores to Salesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="99202-107">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="99202-107">Supported versions</span></span>
<span data-ttu-id="99202-108">Ez az összekötő a következő Salesforce-kiadásokat támogatja: Developer Edition, Professional Edition, Enterprise Edition vagy korlátlan kiadását.</span><span class="sxs-lookup"><span data-stu-id="99202-108">This connector supports the following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="99202-109">És a Salesforce éles, védőfal és az egyéni tartomány másolását támogatja.</span><span class="sxs-lookup"><span data-stu-id="99202-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99202-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="99202-110">Prerequisites</span></span>
* <span data-ttu-id="99202-111">API-engedély engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="99202-111">API permission must be enabled.</span></span> <span data-ttu-id="99202-112">Lásd: [hogyan engedélyezhető az engedélycsoport által a Salesforce-ban API-hozzáférés?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="99202-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="99202-113">Adatok másolása Salesforce a helyszíni adattárolókhoz, rendelkeznie kell legalább adatok felügyeleti átjáró 2.0-s verziójával a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="99202-113">To copy data from Salesforce to on-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="99202-114">Salesforce kérelmekre vonatkozó korlátok</span><span class="sxs-lookup"><span data-stu-id="99202-114">Salesforce request limits</span></span>
<span data-ttu-id="99202-115">Salesforce rendelkezik mind az API-kérelmek teljes száma, és a egyidejű API-kérés.</span><span class="sxs-lookup"><span data-stu-id="99202-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="99202-116">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="99202-116">Note the following points:</span></span>

- <span data-ttu-id="99202-117">Az egyidejű kérelmek száma meghaladja a korlátot, ha sávszélesség-szabályozás következik be, és látni fogja a véletlen hibákat.</span><span class="sxs-lookup"><span data-stu-id="99202-117">If the number of concurrent requests exceeds the limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="99202-118">Ha a kérelmek teljes száma túllépi a korlátot, a Salesforce-fiókban le lesz tiltva 24 órán át.</span><span class="sxs-lookup"><span data-stu-id="99202-118">If the total number of requests exceeds the limit, the Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="99202-119">A "REQUEST_LIMIT_EXCEEDED" hiba mindkét forgatókönyvet is akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="99202-119">You might also receive the “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="99202-120">A "API kérelmekre vonatkozó korlátok" című a [Salesforce fejlesztői korlátok](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="99202-120">See the "API Request Limits" section in the [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="99202-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="99202-121">Getting started</span></span>
<span data-ttu-id="99202-122">A másolási tevékenység, mely az adatok Salesforce különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="99202-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="99202-123">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="99202-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="99202-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="99202-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="99202-125">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="99202-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="99202-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="99202-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="99202-127">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="99202-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="99202-128">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="99202-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="99202-129">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="99202-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="99202-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="99202-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="99202-131">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="99202-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="99202-132">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="99202-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="99202-133">Adatok másolása Salesforce használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Salesforce az Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="99202-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from Salesforce, see [JSON example: Copy data from Salesforce to Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="99202-134">A következő szakaszok részletesen bemutatják való Salesforce adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="99202-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Salesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="99202-135">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="99202-135">Linked service properties</span></span>
<span data-ttu-id="99202-136">A következő táblázat ismerteti, amelyek a Salesforce kapcsolódó szolgáltatásra vonatkozó JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="99202-136">The following table provides descriptions for JSON elements that are specific to the Salesforce linked service.</span></span>

| <span data-ttu-id="99202-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="99202-137">Property</span></span> | <span data-ttu-id="99202-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="99202-138">Description</span></span> | <span data-ttu-id="99202-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="99202-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99202-140">type</span><span class="sxs-lookup"><span data-stu-id="99202-140">type</span></span> |<span data-ttu-id="99202-141">A type tulajdonságot kell beállítani: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="99202-141">The type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="99202-142">Igen</span><span class="sxs-lookup"><span data-stu-id="99202-142">Yes</span></span> |
| <span data-ttu-id="99202-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="99202-143">environmentUrl</span></span> | <span data-ttu-id="99202-144">Adja meg az URL-címet a Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="99202-144">Specify the URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="99202-145">-Alapértelmezett érték a "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="99202-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="99202-146">-Adatok másolása az védőfal, adja meg a "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="99202-146">- To copy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="99202-147">-Adatok másolása az egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="99202-147">- To copy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="99202-148">Nem</span><span class="sxs-lookup"><span data-stu-id="99202-148">No</span></span> |
| <span data-ttu-id="99202-149">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="99202-149">username</span></span> |<span data-ttu-id="99202-150">Adja meg a felhasználói fiók felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="99202-150">Specify a user name for the user account.</span></span> |<span data-ttu-id="99202-151">Igen</span><span class="sxs-lookup"><span data-stu-id="99202-151">Yes</span></span> |
| <span data-ttu-id="99202-152">jelszó</span><span class="sxs-lookup"><span data-stu-id="99202-152">password</span></span> |<span data-ttu-id="99202-153">Adja meg a felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="99202-153">Specify a password for the user account.</span></span> |<span data-ttu-id="99202-154">Igen</span><span class="sxs-lookup"><span data-stu-id="99202-154">Yes</span></span> |
| <span data-ttu-id="99202-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="99202-155">securityToken</span></span> |<span data-ttu-id="99202-156">Adja meg a felhasználói fiók biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="99202-156">Specify a security token for the user account.</span></span> <span data-ttu-id="99202-157">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást, ha alaphelyzetbe állítása/get egy biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="99202-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get a security token.</span></span> <span data-ttu-id="99202-158">Általános biztonsági jogkivonatokat kapcsolatos további tudnivalókért lásd: [biztonsági és az API-t](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="99202-158">To learn about security tokens in general, see [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="99202-159">Igen</span><span class="sxs-lookup"><span data-stu-id="99202-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="99202-160">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="99202-160">Dataset properties</span></span>
<span data-ttu-id="99202-161">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="99202-161">For a full list of sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="99202-162">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla és így tovább).</span><span class="sxs-lookup"><span data-stu-id="99202-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="99202-163">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="99202-163">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="99202-164">A typeProperties szakasz egy adatkészlet típusú **RelationalTable** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="99202-164">The typeProperties section for a dataset of the type **RelationalTable** has the following properties:</span></span>

| <span data-ttu-id="99202-165">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="99202-165">Property</span></span> | <span data-ttu-id="99202-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="99202-166">Description</span></span> | <span data-ttu-id="99202-167">Szükséges</span><span class="sxs-lookup"><span data-stu-id="99202-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99202-168">tableName</span><span class="sxs-lookup"><span data-stu-id="99202-168">tableName</span></span> |<span data-ttu-id="99202-169">A Salesforce-ban a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="99202-169">Name of the table in Salesforce.</span></span> |<span data-ttu-id="99202-170">Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="99202-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="99202-171">Az API neve "__c" részét bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="99202-171">The "__c" part of the API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="99202-173">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="99202-173">Copy activity properties</span></span>
<span data-ttu-id="99202-174">Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: a [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="99202-174">For a full list of sections and properties that are available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="99202-175">Tulajdonságok nevét, leírását, bemeneti és kimeneti táblák, valamint különböző házirendeket is tevékenységi érhető el.</span><span class="sxs-lookup"><span data-stu-id="99202-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="99202-176">Által biztosított a typeProperties szakaszban a tevékenység tulajdonságait, másrészt tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="99202-176">The properties that are available in the typeProperties section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="99202-177">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="99202-177">For Copy Activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="99202-178">A másolási tevékenység, ha az adatforrás típusú **RelationalSource** (amely tartalmazza a Salesforce), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="99202-178">In copy activity, when the source is of the type **RelationalSource** (which includes Salesforce), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="99202-179">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="99202-179">Property</span></span> | <span data-ttu-id="99202-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="99202-180">Description</span></span> | <span data-ttu-id="99202-181">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="99202-181">Allowed values</span></span> | <span data-ttu-id="99202-182">Szükséges</span><span class="sxs-lookup"><span data-stu-id="99202-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="99202-183">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="99202-183">query</span></span> |<span data-ttu-id="99202-184">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="99202-184">Use the custom query to read data.</span></span> |<span data-ttu-id="99202-185">Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="99202-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="99202-186">Például: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="99202-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="99202-187">Nem (Ha a **tableName** , a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="99202-187">No (if the **tableName** of the **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="99202-188">Az API neve "__c" részét bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="99202-188">The "__c" part of the API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="99202-190">Lekérdezés tippek</span><span class="sxs-lookup"><span data-stu-id="99202-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="99202-191">Adatgyűjtés használata a where záradék található dátum és idő oszlop</span><span class="sxs-lookup"><span data-stu-id="99202-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="99202-192">Ha SOQL vagy SQL-lekérdezés megadása, vegye figyelembe a dátum és idő formátumú különbség.</span><span class="sxs-lookup"><span data-stu-id="99202-192">When specify the SOQL or SQL query, pay attention to the DateTime format difference.</span></span> <span data-ttu-id="99202-193">Példa:</span><span class="sxs-lookup"><span data-stu-id="99202-193">For example:</span></span>

* <span data-ttu-id="99202-194">**SOQL minta**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="99202-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="99202-195">**SQL-minta**:</span><span class="sxs-lookup"><span data-stu-id="99202-195">**SQL sample**:</span></span>
    * <span data-ttu-id="99202-196">**Adja meg a lekérdezés másolása varázslóval:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="99202-196">**Using copy wizard to specify the query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="99202-197">**Használatával adhatja meg a lekérdezés szerkesztéséhez JSON (escape-karakter megfelelően):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="99202-197">**Using JSON editing to specify the query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="99202-198">Adatok beolvasása a Salesforce-jelentés</span><span class="sxs-lookup"><span data-stu-id="99202-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="99202-199">Beolvasható adat Salesforce-jelentéseket lekérdezést megadásával `{call "<report name>"}`, például.</span><span class="sxs-lookup"><span data-stu-id="99202-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="99202-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="99202-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="99202-201">A Salesforce Lomtárból törölt rekordok visszanyerése</span><span class="sxs-lookup"><span data-stu-id="99202-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="99202-202">A Salesforce Lomtárból letölthető a törölt rekordok lekérdezéséhez is megadhat **"IsDeleted = 1"** a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="99202-202">To query the soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="99202-203">Például:</span><span class="sxs-lookup"><span data-stu-id="99202-203">For example,</span></span>

* <span data-ttu-id="99202-204">A törölt rekordok lekérdezése, adja meg a "jelölje ki * a MyTable__c **ahol IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="99202-204">To query only the deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="99202-205">A többek között a meglévő és a törölt rekordok lekérdezése az összes, adja meg a "jelölje ki * MyTable__c a **ahol IsDeleted = 0 vagy IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="99202-205">To query all the records including the existing and the deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a><span data-ttu-id="99202-206">JSON-példa: adatok másolása az Salesforce az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="99202-206">JSON example: Copy data from Salesforce to Azure Blob</span></span>
<span data-ttu-id="99202-207">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít a [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="99202-207">The following example provides sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="99202-208">A Salesforce adatok másolása az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="99202-208">They show how to copy data from Salesforce to Azure Blob Storage.</span></span> <span data-ttu-id="99202-209">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="99202-209">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="99202-210">Az alábbiakban az adat-előállító összetevők, amelyeket kezelni kell létrehozni a forgatókönyv végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="99202-210">Here are the Data Factory artifacts that you'll need to create to implement the scenario.</span></span> <span data-ttu-id="99202-211">A lista kövesse a következő szakaszok ezeket a lépéseket részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="99202-211">The sections that follow the list provide details about these steps.</span></span>

* <span data-ttu-id="99202-212">A típusú társított szolgáltatás [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="99202-212">A linked service of the type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="99202-213">A típusú társított szolgáltatás [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="99202-213">A linked service of the type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="99202-214">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="99202-214">An input [dataset](data-factory-create-datasets.md) of the type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="99202-215">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="99202-215">An output [dataset](data-factory-create-datasets.md) of the type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="99202-216">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="99202-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="99202-217">**A Salesforce társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="99202-217">**Salesforce linked service**</span></span>

<span data-ttu-id="99202-218">Ez a példa a **Salesforce** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="99202-218">This example uses the **Salesforce** linked service.</span></span> <span data-ttu-id="99202-219">Tekintse meg a [Salesforce társított szolgáltatás](#linked-service-properties) szolgáltatásnak által támogatott tulajdonságok szakaszát.</span><span class="sxs-lookup"><span data-stu-id="99202-219">See the [Salesforce linked service](#linked-service-properties) section for the properties that are supported by this linked service.</span></span>  <span data-ttu-id="99202-220">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást a biztonsági jogkivonat alaphelyzetbe állítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="99202-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get the security token.</span></span>

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
<span data-ttu-id="99202-221">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="99202-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="99202-222">**Bemeneti Salesforce-adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="99202-222">**Salesforce input dataset**</span></span>

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

<span data-ttu-id="99202-223">Beállítás **külső** való **igaz** tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="99202-223">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99202-224">Az API neve "__c" részét bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="99202-224">The "__c" part of the API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="99202-226">**Azure blobkimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="99202-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="99202-227">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="99202-227">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="99202-228">**A másolási tevékenység folyamat**</span><span class="sxs-lookup"><span data-stu-id="99202-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="99202-229">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="99202-229">The pipeline contains Copy Activity, which is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="99202-230">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource**, és a **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="99202-230">In the pipeline JSON definition, the **source** type is set to **RelationalSource**, and the **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="99202-231">Lásd: [RelationalSource típustulajdonságokat](#copy-activity-properties) a RelationalSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="99202-231">See [RelationalSource type properties](#copy-activity-properties) for the list of properties that are supported by the RelationalSource.</span></span>

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
            "description": "Copy from Salesforce to an Azure blob",
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
> <span data-ttu-id="99202-232">Az API neve "__c" részét bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="99202-232">The "__c" part of the API Name is needed for any custom object.</span></span>

![Data Factory - kapcsolat Salesforce - API neve](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="99202-234">A Salesforce leképezésének</span><span class="sxs-lookup"><span data-stu-id="99202-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="99202-235">Salesforce-típus</span><span class="sxs-lookup"><span data-stu-id="99202-235">Salesforce type</span></span> | <span data-ttu-id="99202-236">. A NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="99202-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="99202-237">Automatikus szám</span><span class="sxs-lookup"><span data-stu-id="99202-237">Auto Number</span></span> |<span data-ttu-id="99202-238">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-238">String</span></span> |
| <span data-ttu-id="99202-239">Jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="99202-239">Checkbox</span></span> |<span data-ttu-id="99202-240">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="99202-240">Boolean</span></span> |
| <span data-ttu-id="99202-241">Currency (Pénznem)</span><span class="sxs-lookup"><span data-stu-id="99202-241">Currency</span></span> |<span data-ttu-id="99202-242">Dupla</span><span class="sxs-lookup"><span data-stu-id="99202-242">Double</span></span> |
| <span data-ttu-id="99202-243">Dátum</span><span class="sxs-lookup"><span data-stu-id="99202-243">Date</span></span> |<span data-ttu-id="99202-244">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="99202-244">DateTime</span></span> |
| <span data-ttu-id="99202-245">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="99202-245">Date/Time</span></span> |<span data-ttu-id="99202-246">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="99202-246">DateTime</span></span> |
| <span data-ttu-id="99202-247">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="99202-247">Email</span></span> |<span data-ttu-id="99202-248">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-248">String</span></span> |
| <span data-ttu-id="99202-249">Azonosító</span><span class="sxs-lookup"><span data-stu-id="99202-249">Id</span></span> |<span data-ttu-id="99202-250">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-250">String</span></span> |
| <span data-ttu-id="99202-251">Keresési kapcsolat</span><span class="sxs-lookup"><span data-stu-id="99202-251">Lookup Relationship</span></span> |<span data-ttu-id="99202-252">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-252">String</span></span> |
| <span data-ttu-id="99202-253">Többszörös kiválasztási lista</span><span class="sxs-lookup"><span data-stu-id="99202-253">Multi-Select Picklist</span></span> |<span data-ttu-id="99202-254">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-254">String</span></span> |
| <span data-ttu-id="99202-255">Szám</span><span class="sxs-lookup"><span data-stu-id="99202-255">Number</span></span> |<span data-ttu-id="99202-256">Dupla</span><span class="sxs-lookup"><span data-stu-id="99202-256">Double</span></span> |
| <span data-ttu-id="99202-257">Százalék</span><span class="sxs-lookup"><span data-stu-id="99202-257">Percent</span></span> |<span data-ttu-id="99202-258">Dupla</span><span class="sxs-lookup"><span data-stu-id="99202-258">Double</span></span> |
| <span data-ttu-id="99202-259">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="99202-259">Phone</span></span> |<span data-ttu-id="99202-260">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-260">String</span></span> |
| <span data-ttu-id="99202-261">Választási lista</span><span class="sxs-lookup"><span data-stu-id="99202-261">Picklist</span></span> |<span data-ttu-id="99202-262">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-262">String</span></span> |
| <span data-ttu-id="99202-263">Szöveg</span><span class="sxs-lookup"><span data-stu-id="99202-263">Text</span></span> |<span data-ttu-id="99202-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-264">String</span></span> |
| <span data-ttu-id="99202-265">Szövegmező</span><span class="sxs-lookup"><span data-stu-id="99202-265">Text Area</span></span> |<span data-ttu-id="99202-266">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-266">String</span></span> |
| <span data-ttu-id="99202-267">Szövegmező (nagy)</span><span class="sxs-lookup"><span data-stu-id="99202-267">Text Area (Long)</span></span> |<span data-ttu-id="99202-268">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-268">String</span></span> |
| <span data-ttu-id="99202-269">Szövegmező (gazdag)</span><span class="sxs-lookup"><span data-stu-id="99202-269">Text Area (Rich)</span></span> |<span data-ttu-id="99202-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-270">String</span></span> |
| <span data-ttu-id="99202-271">Szöveg (titkosítva)</span><span class="sxs-lookup"><span data-stu-id="99202-271">Text (Encrypted)</span></span> |<span data-ttu-id="99202-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-272">String</span></span> |
| <span data-ttu-id="99202-273">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="99202-273">URL</span></span> |<span data-ttu-id="99202-274">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99202-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="99202-275">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="99202-275">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="99202-276">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="99202-276">Performance and tuning</span></span>
<span data-ttu-id="99202-277">Tekintse meg a [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="99202-277">See the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
