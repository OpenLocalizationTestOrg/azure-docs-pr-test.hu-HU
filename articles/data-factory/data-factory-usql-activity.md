---
title: "U-SQL parancsfájl - Azure használatával adatok átalakítása |} Microsoft Docs"
description: "Ismerje meg, hogyan kell feldolgozni vagy átalakítási adatok számítási szolgáltatás Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 49a809af92ed1bc6664fbdd3bf1aabf36afb8180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="b7a07-103">Adatok átalakítása Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával</span><span class="sxs-lookup"><span data-stu-id="b7a07-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b7a07-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b7a07-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b7a07-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="b7a07-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b7a07-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="b7a07-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b7a07-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b7a07-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b7a07-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b7a07-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b7a07-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b7a07-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="b7a07-114">Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="b7a07-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="b7a07-115">Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b7a07-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="b7a07-116">Ez a cikk ismerteti a **Data Lake Analytics U-SQL tevékenység** , amelyen fut a **U-SQL** a parancsfájl egy **Azure Data Lake Analytics** számítási kapcsolódó szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b7a07-116">This article describes the **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="b7a07-117">Azure Data Lake Analytics-fiók létrehozása előtt hoz létre egy folyamatot egy Data Lake Analytics U-SQL-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b7a07-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="b7a07-118">Azure Data Lake Analytics kapcsolatos további tudnivalókért lásd: [Ismerkedés az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b7a07-118">To learn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="b7a07-119">Tekintse át a [az adatcsatorna első oktatóanyaga Szerkesztés](data-factory-build-your-first-pipeline.md) egy adat-előállító létrehozásának részletes lépései a kapcsolódó szolgáltatások, az adatkészletek és a folyamat.</span><span class="sxs-lookup"><span data-stu-id="b7a07-119">Review the [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps to create a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="b7a07-120">A Data Factory Editor vagy a Visual Studio vagy az Azure PowerShell JSON kódtöredékek segítségével adat-előállító entitásokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="b7a07-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell to create Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="b7a07-121">Támogatott hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="b7a07-121">Supported authentication types</span></span>
<span data-ttu-id="b7a07-122">U-SQL-tevékenység alábbi Data Lake Analytics elleni hitelesítési típust támogatja:</span><span class="sxs-lookup"><span data-stu-id="b7a07-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="b7a07-123">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="b7a07-123">Service principal authentication</span></span>
* <span data-ttu-id="b7a07-124">Felhasználói hitelesítő adatok (OAuth) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="b7a07-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="b7a07-125">Azt javasoljuk, hogy használja-e szolgáltatás egyszerű hitelesítést, különösen a egy ütemezett U-SQL-végrehajtást.</span><span class="sxs-lookup"><span data-stu-id="b7a07-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="b7a07-126">Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="b7a07-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="b7a07-127">További konfigurációs információkért lásd: a [szolgáltatástulajdonságok kapcsolódó](#azure-data-lake-analytics-linked-service) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b7a07-127">For configuration details, see the [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="b7a07-128">Az Azure Data Lake Analytics társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b7a07-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="b7a07-129">Létrehozhat egy **Azure Data Lake Analytics** társított szolgáltatás az Azure Data Lake Analytics csatolásához számítási egy az Azure data factory szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b7a07-129">You create an **Azure Data Lake Analytics** linked service to link an Azure Data Lake Analytics compute service to an Azure data factory.</span></span> <span data-ttu-id="b7a07-130">A Data Lake Analytics U-SQL-tevékenység a feldolgozási szolgáltatásnak hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b7a07-130">The Data Lake Analytics U-SQL activity in the pipeline refers to this linked service.</span></span> 

<span data-ttu-id="b7a07-131">A következő táblázat ismerteti a JSON-definícióból használt általános tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b7a07-131">The following table provides descriptions for the generic properties used in the JSON definition.</span></span> <span data-ttu-id="b7a07-132">További választhat egyszerű szolgáltatásnév és felhasználói hitelesítő adatok hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="b7a07-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="b7a07-133">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b7a07-133">Property</span></span> | <span data-ttu-id="b7a07-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7a07-134">Description</span></span> | <span data-ttu-id="b7a07-135">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b7a07-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b7a07-136">**típusa**</span><span class="sxs-lookup"><span data-stu-id="b7a07-136">**type**</span></span> |<span data-ttu-id="b7a07-137">A type tulajdonságot kell megadni: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="b7a07-137">The type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="b7a07-138">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-138">Yes</span></span> |
| <span data-ttu-id="b7a07-139">**Fióknév**</span><span class="sxs-lookup"><span data-stu-id="b7a07-139">**accountName**</span></span> |<span data-ttu-id="b7a07-140">Az Azure Data Lake Analytics-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="b7a07-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="b7a07-141">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-141">Yes</span></span> |
| <span data-ttu-id="b7a07-142">**datalakeanalyticsuri paraméter**</span><span class="sxs-lookup"><span data-stu-id="b7a07-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="b7a07-143">Az Azure Data Lake Analytics URI.</span><span class="sxs-lookup"><span data-stu-id="b7a07-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="b7a07-144">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-144">No</span></span> |
| <span data-ttu-id="b7a07-145">**előfizetés-azonosító**</span><span class="sxs-lookup"><span data-stu-id="b7a07-145">**subscriptionId**</span></span> |<span data-ttu-id="b7a07-146">Az Azure előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="b7a07-146">Azure subscription id</span></span> |<span data-ttu-id="b7a07-147">Nem (Ha nincs megadva, a data factory-előfizetése szerepel).</span><span class="sxs-lookup"><span data-stu-id="b7a07-147">No (If not specified, subscription of the data factory is used).</span></span> |
| <span data-ttu-id="b7a07-148">**erőforráscsoport-név**</span><span class="sxs-lookup"><span data-stu-id="b7a07-148">**resourceGroupName**</span></span> |<span data-ttu-id="b7a07-149">Azure erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="b7a07-149">Azure resource group name</span></span> |<span data-ttu-id="b7a07-150">Nem (Ha nincs megadva, az adat-előállító erőforráscsoport szerepel).</span><span class="sxs-lookup"><span data-stu-id="b7a07-150">No (If not specified, resource group of the data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="b7a07-151">Szolgáltatás egyszerű hitelesítés (ajánlott)</span><span class="sxs-lookup"><span data-stu-id="b7a07-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="b7a07-152">Szolgáltatás egyszerű hitelesítést használ, egy alkalmazás entitás regisztrálni kell az Azure Active Directory (Azure AD), és hozzáférést, a Data Lake Store-bA.</span><span class="sxs-lookup"><span data-stu-id="b7a07-152">To use service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it the access to Data Lake Store.</span></span> <span data-ttu-id="b7a07-153">Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="b7a07-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="b7a07-154">Jegyezze fel a következő érték, melynek segítségével határozza meg a társított szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="b7a07-154">Make note of the following values, which you use to define the linked service:</span></span>
* <span data-ttu-id="b7a07-155">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="b7a07-155">Application ID</span></span>
* <span data-ttu-id="b7a07-156">Alkalmazás kulcs</span><span class="sxs-lookup"><span data-stu-id="b7a07-156">Application key</span></span> 
* <span data-ttu-id="b7a07-157">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="b7a07-157">Tenant ID</span></span>

<span data-ttu-id="b7a07-158">Szolgáltatás egyszerű hitelesítés használatára a következő tulajdonságok megadásával:</span><span class="sxs-lookup"><span data-stu-id="b7a07-158">Use service principal authentication by specifying the following properties:</span></span>

| <span data-ttu-id="b7a07-159">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b7a07-159">Property</span></span> | <span data-ttu-id="b7a07-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7a07-160">Description</span></span> | <span data-ttu-id="b7a07-161">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b7a07-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b7a07-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="b7a07-162">**servicePrincipalId**</span></span> | <span data-ttu-id="b7a07-163">Adja meg az alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b7a07-163">Specify the application's client ID.</span></span> | <span data-ttu-id="b7a07-164">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-164">Yes</span></span> |
| <span data-ttu-id="b7a07-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="b7a07-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="b7a07-166">Adja meg az alkalmazás kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b7a07-166">Specify the application's key.</span></span> | <span data-ttu-id="b7a07-167">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-167">Yes</span></span> |
| <span data-ttu-id="b7a07-168">**Bérlői**</span><span class="sxs-lookup"><span data-stu-id="b7a07-168">**tenant**</span></span> | <span data-ttu-id="b7a07-169">Adja meg a bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="b7a07-169">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="b7a07-170">Azt az Azure-portál jobb felső sarkában az egér rámutató által kérheti le.</span><span class="sxs-lookup"><span data-stu-id="b7a07-170">You can retrieve it by hovering the mouse in the upper-right corner of the Azure portal.</span></span> | <span data-ttu-id="b7a07-171">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-171">Yes</span></span> |

<span data-ttu-id="b7a07-172">**Példa: Szolgáltatás egyszerű hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="b7a07-172">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="b7a07-173">Felhasználói hitelesítő adatok hitelesítése</span><span class="sxs-lookup"><span data-stu-id="b7a07-173">User credential authentication</span></span>
<span data-ttu-id="b7a07-174">Alternatív megoldásként használható felhasználói hitelesítő Data Lake Analytics megadását követően az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b7a07-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying the following properties:</span></span>

| <span data-ttu-id="b7a07-175">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b7a07-175">Property</span></span> | <span data-ttu-id="b7a07-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7a07-176">Description</span></span> | <span data-ttu-id="b7a07-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b7a07-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b7a07-178">**engedélyezési**</span><span class="sxs-lookup"><span data-stu-id="b7a07-178">**authorization**</span></span> | <span data-ttu-id="b7a07-179">Kattintson a **engedélyezés** a Data Factory Editor gombra, és adja meg a hitelesítő adatok, amelyek az automatikusan létrehozott engedélyezési URL-címet rendel hozzá ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="b7a07-179">Click the **Authorize** button in the Data Factory Editor and enter your credential that assigns the autogenerated authorization URL to this property.</span></span> | <span data-ttu-id="b7a07-180">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-180">Yes</span></span> |
| <span data-ttu-id="b7a07-181">**munkamenet-azonosító**</span><span class="sxs-lookup"><span data-stu-id="b7a07-181">**sessionId**</span></span> | <span data-ttu-id="b7a07-182">OAuth munkamenet-azonosító az OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="b7a07-182">OAuth session ID from the OAuth authorization session.</span></span> <span data-ttu-id="b7a07-183">Minden munkamenet-azonosító egyedi, és csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="b7a07-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="b7a07-184">Ez a beállítás automatikusan jön létre, a Data Factory Editor használatakor.</span><span class="sxs-lookup"><span data-stu-id="b7a07-184">This setting is automatically generated when you use the Data Factory Editor.</span></span> | <span data-ttu-id="b7a07-185">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-185">Yes</span></span> |

<span data-ttu-id="b7a07-186">**Példa: Felhasználók hitelesítő adatok hitelesítése**</span><span class="sxs-lookup"><span data-stu-id="b7a07-186">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="b7a07-187">Jogkivonat lejáratáról</span><span class="sxs-lookup"><span data-stu-id="b7a07-187">Token expiration</span></span>
<span data-ttu-id="b7a07-188">Az engedélyezési kód használatával előállított a **engedélyezés** gomb némi várakozás után lejár.</span><span class="sxs-lookup"><span data-stu-id="b7a07-188">The authorization code you generated by using the **Authorize** button expires after sometime.</span></span> <span data-ttu-id="b7a07-189">Lásd az alábbi táblázatban a különböző típusú felhasználói fiókokat a lejárati idejét.</span><span class="sxs-lookup"><span data-stu-id="b7a07-189">See the following table for the expiration times for different types of user accounts.</span></span> <span data-ttu-id="b7a07-190">Jelenhetnek meg a következő hiba jelenik meg, ha a hitelesítési **-token érvényessége lejár**: hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b7a07-190">You may see the following error message when the authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="b7a07-191">AADSTS70008: A megadott hozzáférés biztosítása lejárt vagy visszavonták.</span><span class="sxs-lookup"><span data-stu-id="b7a07-191">AADSTS70008: The provided access grant is expired or revoked.</span></span> <span data-ttu-id="b7a07-192">Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="b7a07-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="b7a07-193">Felhasználó típusa</span><span class="sxs-lookup"><span data-stu-id="b7a07-193">User type</span></span> | <span data-ttu-id="b7a07-194">Után lejár</span><span class="sxs-lookup"><span data-stu-id="b7a07-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="b7a07-195">Felhasználói fiókok Azure Active Directory által nem kezelt (@hotmail.com, @live.comstb.)</span><span class="sxs-lookup"><span data-stu-id="b7a07-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="b7a07-196">12 óra</span><span class="sxs-lookup"><span data-stu-id="b7a07-196">12 hours</span></span> |
| <span data-ttu-id="b7a07-197">Felhasználói fiókok felügyelete által Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="b7a07-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="b7a07-198">Futtassa a 14 nap után utolsó újrapróbálása.</span><span class="sxs-lookup"><span data-stu-id="b7a07-198">14 days after the last slice run.</span></span> <br/><br/><span data-ttu-id="b7a07-199">90 nap, ha a szelet OAuth-alapú társított szolgáltatás fut legalább 14 naponta.</span><span class="sxs-lookup"><span data-stu-id="b7a07-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="b7a07-200">Ez a hiba elkerüléséhez/hárítsa, ismét engedélyezheti a használatával a **engedélyezés** gombra kattint, ha a **jogkivonat lejár** és telepítse újra a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b7a07-200">To avoid/resolve this error, reauthorize using the **Authorize** button when the **token expires** and redeploy the linked service.</span></span> <span data-ttu-id="b7a07-201">Értékek is létrehozhat **sessionId** és **engedélyezési** programozott módon, az alábbiak szerint kód tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="b7a07-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

<span data-ttu-id="b7a07-202">Lásd: [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témakörök a Data Factory osztályok, a kódban használt vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="b7a07-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about the Data Factory classes used in the code.</span></span> <span data-ttu-id="b7a07-203">Adjon hozzá egy hivatkozást,: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog osztálynál.</span><span class="sxs-lookup"><span data-stu-id="b7a07-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for the WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="b7a07-204">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b7a07-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="b7a07-205">A következő JSON-részlet egy folyamatot, egy Data Lake Analytics U-SQL tevékenység határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b7a07-205">The following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="b7a07-206">A tevékenység definíciójának van a korábban létrehozott Azure Data Lake Analytics társított szolgáltatás hivatkozása.</span><span class="sxs-lookup"><span data-stu-id="b7a07-206">The activity definition has a reference to the Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="b7a07-207">A következő táblázat ismerteti a neveket és leírásokat erre a tevékenységre jellemző tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b7a07-207">The following table describes names and descriptions of properties that are specific to this activity.</span></span> 

| <span data-ttu-id="b7a07-208">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b7a07-208">Property</span></span> | <span data-ttu-id="b7a07-209">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7a07-209">Description</span></span> | <span data-ttu-id="b7a07-210">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b7a07-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b7a07-211">type</span><span class="sxs-lookup"><span data-stu-id="b7a07-211">type</span></span> |<span data-ttu-id="b7a07-212">A type tulajdonságot meg kell **DataLakeAnalyticsU-SQL**.</span><span class="sxs-lookup"><span data-stu-id="b7a07-212">The type property must be set to **DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="b7a07-213">Igen</span><span class="sxs-lookup"><span data-stu-id="b7a07-213">Yes</span></span> |
| <span data-ttu-id="b7a07-214">scriptPath</span><span class="sxs-lookup"><span data-stu-id="b7a07-214">scriptPath</span></span> |<span data-ttu-id="b7a07-215">A U-SQL parancsfájlt tartalmazó mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="b7a07-215">Path to folder that contains the U-SQL script.</span></span> <span data-ttu-id="b7a07-216">A fájl neve nem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b7a07-216">Name of the file is case-sensitive.</span></span> |<span data-ttu-id="b7a07-217">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="b7a07-217">No (if you use script)</span></span> |
| <span data-ttu-id="b7a07-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="b7a07-218">scriptLinkedService</span></span> |<span data-ttu-id="b7a07-219">Kapcsolódó szolgáltatás, amely a tárolóban, amely tartalmazza az adat-előállító parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b7a07-219">Linked service that links the storage that contains the script to the data factory</span></span> |<span data-ttu-id="b7a07-220">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="b7a07-220">No (if you use script)</span></span> |
| <span data-ttu-id="b7a07-221">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b7a07-221">script</span></span> |<span data-ttu-id="b7a07-222">Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="b7a07-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="b7a07-223">Például: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="b7a07-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="b7a07-224">Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ)</span><span class="sxs-lookup"><span data-stu-id="b7a07-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="b7a07-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="b7a07-225">degreeOfParallelism</span></span> |<span data-ttu-id="b7a07-226">A feladat futtatásához egyidejűleg használt csomópontok maximális száma.</span><span class="sxs-lookup"><span data-stu-id="b7a07-226">The maximum number of nodes simultaneously used to run the job.</span></span> |<span data-ttu-id="b7a07-227">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-227">No</span></span> |
| <span data-ttu-id="b7a07-228">Prioritás</span><span class="sxs-lookup"><span data-stu-id="b7a07-228">priority</span></span> |<span data-ttu-id="b7a07-229">Azt határozza meg, melyet futtatni kíván szereplő várólistáján szereplő feladatok közül melyeket.</span><span class="sxs-lookup"><span data-stu-id="b7a07-229">Determines which jobs out of all that are queued should be selected to run first.</span></span> <span data-ttu-id="b7a07-230">Az alacsonyabb a szám, annál magasabb a prioritás.</span><span class="sxs-lookup"><span data-stu-id="b7a07-230">The lower the number, the higher the priority.</span></span> |<span data-ttu-id="b7a07-231">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-231">No</span></span> |
| <span data-ttu-id="b7a07-232">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="b7a07-232">parameters</span></span> |<span data-ttu-id="b7a07-233">A U-SQL parancsfájl paraméterek</span><span class="sxs-lookup"><span data-stu-id="b7a07-233">Parameters for the U-SQL script</span></span> |<span data-ttu-id="b7a07-234">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-234">No</span></span> |
| <span data-ttu-id="b7a07-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="b7a07-235">runtimeVersion</span></span> | <span data-ttu-id="b7a07-236">A U-SQL motort használja futásidejű verzióját</span><span class="sxs-lookup"><span data-stu-id="b7a07-236">Runtime version of the U-SQL engine to use</span></span> | <span data-ttu-id="b7a07-237">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-237">No</span></span> | 
| <span data-ttu-id="b7a07-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="b7a07-238">compilationMode</span></span> | <p><span data-ttu-id="b7a07-239">Fordítási mód az U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b7a07-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="b7a07-240">A következő értékek egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="b7a07-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="b7a07-241">**Szemantikus:** csak a szemantikai ellenőrzések és a szükséges megerősítések végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b7a07-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="b7a07-242">**Teljes:** hajtsa végre a teljes fordítási, beleértve a szintaxis-ellenőrzés, optimalizálás, kódgenerálás, stb.</span><span class="sxs-lookup"><span data-stu-id="b7a07-242">**Full:** Perform the full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="b7a07-243">**SingleBox:** hajtsa végre a teljes fordítási SingleBox való a TargetType beállítással.</span><span class="sxs-lookup"><span data-stu-id="b7a07-243">**SingleBox:** Perform the full compilation, with TargetType setting to SingleBox.</span></span></li></ul><p><span data-ttu-id="b7a07-244">Ez a tulajdonság értékét nem adja meg, ha a kiszolgáló meghatározza a optimális fordítás módja.</span><span class="sxs-lookup"><span data-stu-id="b7a07-244">If you don't specify a value for this property, the server determines the optimal compilation mode.</span></span> </p>| <span data-ttu-id="b7a07-245">Nem</span><span class="sxs-lookup"><span data-stu-id="b7a07-245">No</span></span> | 

<span data-ttu-id="b7a07-246">Lásd: [SearchLogProcessing.txt parancsfájl Definition](#sample-u-sql-script) a parancsfájl definíciójának.</span><span class="sxs-lookup"><span data-stu-id="b7a07-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for the script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="b7a07-247">Minta bemeneti és kimeneti adatkészletek</span><span class="sxs-lookup"><span data-stu-id="b7a07-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="b7a07-248">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b7a07-248">Input dataset</span></span>
<span data-ttu-id="b7a07-249">Ebben a példában a bemeneti adatok az Azure Data Lake Store (searchlog.tsv fájl fájl a datalake-/ bemeneti mappában) található.</span><span class="sxs-lookup"><span data-stu-id="b7a07-249">In this example, the input data resides in an Azure Data Lake Store (SearchLog.tsv file in the datalake/input folder).</span></span> 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a><span data-ttu-id="b7a07-250">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b7a07-250">Output dataset</span></span>
<span data-ttu-id="b7a07-251">Ebben a példában a U-SQL parancsfájl által előállított kimeneti adatokat az Azure Data Lake Store (datalake-/ kimeneti mappa) tárolódik.</span><span class="sxs-lookup"><span data-stu-id="b7a07-251">In this example, the output data produced by the U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="b7a07-252">A minta Data Lake Store társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b7a07-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="b7a07-253">Ez a minta Azure Data Lake Store társított szolgáltatás által a bemeneti/kimeneti adatkészletek használt definíciója.</span><span class="sxs-lookup"><span data-stu-id="b7a07-253">Here is the definition of the sample Azure Data Lake Store linked service used by the input/output datasets.</span></span> 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

<span data-ttu-id="b7a07-254">Lásd: [helyezze át az adatokat, és az Azure Data Lake Store](data-factory-azure-datalake-connector.md) cikk a JSON-tulajdonságok leírását.</span><span class="sxs-lookup"><span data-stu-id="b7a07-254">See [Move data to and from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="b7a07-255">Minta U-SQL parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b7a07-255">Sample U-SQL Script</span></span>

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="b7a07-256">Az értékek  **@in**  és  **@out**  a U-SQL parancsfájl átadott paraméterek dinamikusan ADF által a "parameters" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="b7a07-256">The values for **@in** and **@out** parameters in the U-SQL script are passed dynamically by ADF using the ‘parameters’ section.</span></span> <span data-ttu-id="b7a07-257">A "parameters" című rész a csővezeték-definícióban.</span><span class="sxs-lookup"><span data-stu-id="b7a07-257">See the ‘parameters’ section in the pipeline definition.</span></span>

<span data-ttu-id="b7a07-258">Megadhat más tulajdonságait, például degreeOfParallelism és prioritását, valamint a csővezeték-definícióban az az Azure Data Lake Analytics szolgáltatásban futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="b7a07-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for the jobs that run on the Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="b7a07-259">Dinamikus paraméterek</span><span class="sxs-lookup"><span data-stu-id="b7a07-259">Dynamic parameters</span></span>
<span data-ttu-id="b7a07-260">A minta-feldolgozási folyamat definícióban bejövő és kimenő adatforgalma paraméterek vannak hozzárendelve kódolt értékekkel.</span><span class="sxs-lookup"><span data-stu-id="b7a07-260">In the sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="b7a07-261">Akkor használja helyette a dinamikus paraméterek lehet.</span><span class="sxs-lookup"><span data-stu-id="b7a07-261">It is possible to use dynamic parameters instead.</span></span> <span data-ttu-id="b7a07-262">Példa:</span><span class="sxs-lookup"><span data-stu-id="b7a07-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="b7a07-263">Ebben az esetben a bemeneti fájlok továbbra is átveszik a /datalake/input mappából, és kimeneti fájlok jönnek létre, a /datalake/output mappában.</span><span class="sxs-lookup"><span data-stu-id="b7a07-263">In this case, input files are still picked up from the /datalake/input folder and output files are generated in the /datalake/output folder.</span></span> <span data-ttu-id="b7a07-264">Fájlnevek nem dinamikus szelet kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="b7a07-264">The file names are dynamic based on the slice start time.</span></span>  

