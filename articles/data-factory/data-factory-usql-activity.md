---
title: "U-SQL parancsfájl - Azure aaaTransform adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess vagy átalakítási adatok Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával számítási szolgáltatás."
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
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="cb288-103">Adatok átalakítása Azure Data Lake Analytics U-SQL-parancsfájlok futtatásával</span><span class="sxs-lookup"><span data-stu-id="cb288-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="cb288-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="cb288-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="cb288-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="cb288-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="cb288-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="cb288-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="cb288-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="cb288-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="cb288-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="cb288-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="cb288-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="cb288-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="cb288-114">Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="cb288-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="cb288-115">Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cb288-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="cb288-116">Ez a cikk ismerteti a hello **Data Lake Analytics U-SQL tevékenység** , amelyen fut a **U-SQL** a parancsfájl egy **Azure Data Lake Analytics** számítási kapcsolódó szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cb288-116">This article describes hello **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="cb288-117">Azure Data Lake Analytics-fiók létrehozása előtt hoz létre egy folyamatot egy Data Lake Analytics U-SQL-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cb288-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="cb288-118">Azure Data Lake Analytics kapcsolatos toolearn lásd: [Ismerkedés az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cb288-118">toolearn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="cb288-119">Felülvizsgálati hello [az adatcsatorna első oktatóanyaga Szerkesztés](data-factory-build-your-first-pipeline.md) részletes lépéseket toocreate egy adat-előállítót, a kapcsolódó szolgáltatások, az adatkészletek és a folyamat.</span><span class="sxs-lookup"><span data-stu-id="cb288-119">Review hello [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps toocreate a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="cb288-120">JSON kódtöredékek használja a Data Factory Editor vagy a Visual Studio vagy az Azure PowerShell toocreate adat-előállító szervezeteknél.</span><span class="sxs-lookup"><span data-stu-id="cb288-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell toocreate Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="cb288-121">Támogatott hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="cb288-121">Supported authentication types</span></span>
<span data-ttu-id="cb288-122">U-SQL-tevékenység alábbi Data Lake Analytics elleni hitelesítési típust támogatja:</span><span class="sxs-lookup"><span data-stu-id="cb288-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="cb288-123">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="cb288-123">Service principal authentication</span></span>
* <span data-ttu-id="cb288-124">Felhasználói hitelesítő adatok (OAuth) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cb288-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="cb288-125">Azt javasoljuk, hogy használja-e szolgáltatás egyszerű hitelesítést, különösen a egy ütemezett U-SQL-végrehajtást.</span><span class="sxs-lookup"><span data-stu-id="cb288-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="cb288-126">Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="cb288-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="cb288-127">További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#azure-data-lake-analytics-linked-service) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cb288-127">For configuration details, see hello [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="cb288-128">Az Azure Data Lake Analytics társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cb288-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="cb288-129">Létrehozhat egy **Azure Data Lake Analytics** szolgáltatás toolink egy Azure Data Lake Analytics számítási szolgáltatás tooan az Azure data factory kapcsolt.</span><span class="sxs-lookup"><span data-stu-id="cb288-129">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory.</span></span> <span data-ttu-id="cb288-130">Data Lake Analytics U-SQL-tevékenység hello folyamat hello toothis kapcsolódó szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cb288-130">hello Data Lake Analytics U-SQL activity in hello pipeline refers toothis linked service.</span></span> 

<span data-ttu-id="cb288-131">hello következő táblázat ismerteti hello hello JSON-definícióból használt általános tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cb288-131">hello following table provides descriptions for hello generic properties used in hello JSON definition.</span></span> <span data-ttu-id="cb288-132">További választhat egyszerű szolgáltatásnév és felhasználói hitelesítő adatok hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="cb288-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="cb288-133">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb288-133">Property</span></span> | <span data-ttu-id="cb288-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb288-134">Description</span></span> | <span data-ttu-id="cb288-135">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb288-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cb288-136">**típusa**</span><span class="sxs-lookup"><span data-stu-id="cb288-136">**type**</span></span> |<span data-ttu-id="cb288-137">hello tulajdonságra kell megadni: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="cb288-137">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="cb288-138">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-138">Yes</span></span> |
| <span data-ttu-id="cb288-139">**Fióknév**</span><span class="sxs-lookup"><span data-stu-id="cb288-139">**accountName**</span></span> |<span data-ttu-id="cb288-140">Az Azure Data Lake Analytics-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="cb288-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="cb288-141">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-141">Yes</span></span> |
| <span data-ttu-id="cb288-142">**datalakeanalyticsuri paraméter**</span><span class="sxs-lookup"><span data-stu-id="cb288-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="cb288-143">Az Azure Data Lake Analytics URI.</span><span class="sxs-lookup"><span data-stu-id="cb288-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="cb288-144">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-144">No</span></span> |
| <span data-ttu-id="cb288-145">**előfizetés-azonosító**</span><span class="sxs-lookup"><span data-stu-id="cb288-145">**subscriptionId**</span></span> |<span data-ttu-id="cb288-146">Az Azure előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="cb288-146">Azure subscription id</span></span> |<span data-ttu-id="cb288-147">Nem (Ha nincs megadva, az adat-előállító használt hello előfizetés).</span><span class="sxs-lookup"><span data-stu-id="cb288-147">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="cb288-148">**erőforráscsoport-név**</span><span class="sxs-lookup"><span data-stu-id="cb288-148">**resourceGroupName**</span></span> |<span data-ttu-id="cb288-149">Azure erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="cb288-149">Azure resource group name</span></span> |<span data-ttu-id="cb288-150">Nem (Ha nincs megadva, az adat-előállító használt hello erőforráscsoport).</span><span class="sxs-lookup"><span data-stu-id="cb288-150">No (If not specified, resource group of hello data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="cb288-151">Szolgáltatás egyszerű hitelesítés (ajánlott)</span><span class="sxs-lookup"><span data-stu-id="cb288-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="cb288-152">toouse szolgáltatás egyszerű hitelesítési regisztrálása az Azure Active Directory (Azure AD) és támogatás azt hello alkalmazás entitás tooData Lake áruház eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cb288-152">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="cb288-153">Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="cb288-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="cb288-154">Jegyezze fel az értéket, amely a következő hello toodefine hello társított szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="cb288-154">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="cb288-155">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="cb288-155">Application ID</span></span>
* <span data-ttu-id="cb288-156">Alkalmazás kulcs</span><span class="sxs-lookup"><span data-stu-id="cb288-156">Application key</span></span> 
* <span data-ttu-id="cb288-157">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="cb288-157">Tenant ID</span></span>

<span data-ttu-id="cb288-158">Szolgáltatás egyszerű hitelesítés használata a következő tulajdonságok hello megadásával:</span><span class="sxs-lookup"><span data-stu-id="cb288-158">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="cb288-159">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb288-159">Property</span></span> | <span data-ttu-id="cb288-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb288-160">Description</span></span> | <span data-ttu-id="cb288-161">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb288-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cb288-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="cb288-162">**servicePrincipalId**</span></span> | <span data-ttu-id="cb288-163">Adja meg a hello alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="cb288-163">Specify hello application's client ID.</span></span> | <span data-ttu-id="cb288-164">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-164">Yes</span></span> |
| <span data-ttu-id="cb288-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="cb288-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="cb288-166">Adja meg a hello kulcsát.</span><span class="sxs-lookup"><span data-stu-id="cb288-166">Specify hello application's key.</span></span> | <span data-ttu-id="cb288-167">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-167">Yes</span></span> |
| <span data-ttu-id="cb288-168">**Bérlői**</span><span class="sxs-lookup"><span data-stu-id="cb288-168">**tenant**</span></span> | <span data-ttu-id="cb288-169">Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="cb288-169">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="cb288-170">Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="cb288-170">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="cb288-171">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-171">Yes</span></span> |

<span data-ttu-id="cb288-172">**Példa: Szolgáltatás egyszerű hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="cb288-172">**Example: Service principal authentication**</span></span>
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

### <a name="user-credential-authentication"></a><span data-ttu-id="cb288-173">Felhasználói hitelesítő adatok hitelesítése</span><span class="sxs-lookup"><span data-stu-id="cb288-173">User credential authentication</span></span>
<span data-ttu-id="cb288-174">Alternatív megoldásként használható felhasználói hitelesítő adatok hitelesítése Data Lake Analytics megadásával következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="cb288-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying hello following properties:</span></span>

| <span data-ttu-id="cb288-175">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb288-175">Property</span></span> | <span data-ttu-id="cb288-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb288-176">Description</span></span> | <span data-ttu-id="cb288-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb288-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cb288-178">**engedélyezési**</span><span class="sxs-lookup"><span data-stu-id="cb288-178">**authorization**</span></span> | <span data-ttu-id="cb288-179">Kattintson a hello **engedélyezés** hello Data Factory Editor gombra, és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cb288-179">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="cb288-180">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-180">Yes</span></span> |
| <span data-ttu-id="cb288-181">**munkamenet-azonosító**</span><span class="sxs-lookup"><span data-stu-id="cb288-181">**sessionId**</span></span> | <span data-ttu-id="cb288-182">OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="cb288-182">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="cb288-183">Minden munkamenet-azonosító egyedi, és csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="cb288-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="cb288-184">Ez a beállítás automatikusan létrejön a Data Factory Editor hello használatakor.</span><span class="sxs-lookup"><span data-stu-id="cb288-184">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="cb288-185">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-185">Yes</span></span> |

<span data-ttu-id="cb288-186">**Példa: Felhasználók hitelesítő adatok hitelesítése**</span><span class="sxs-lookup"><span data-stu-id="cb288-186">**Example: User credential authentication**</span></span>
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

#### <a name="token-expiration"></a><span data-ttu-id="cb288-187">Jogkivonat lejáratáról</span><span class="sxs-lookup"><span data-stu-id="cb288-187">Token expiration</span></span>
<span data-ttu-id="cb288-188">Ön hello segítségével létrehozott engedélyezési kód hello **engedélyezés** gomb némi várakozás után lejár.</span><span class="sxs-lookup"><span data-stu-id="cb288-188">hello authorization code you generated by using hello **Authorize** button expires after sometime.</span></span> <span data-ttu-id="cb288-189">Tekintse meg a következő táblázat a különböző típusú felhasználói fiókok hello lejárati idejének hello.</span><span class="sxs-lookup"><span data-stu-id="cb288-189">See hello following table for hello expiration times for different types of user accounts.</span></span> <span data-ttu-id="cb288-190">Hello a következő hibaüzenetet fog látni amikor hello hitelesítési **jogkivonat lejár**: hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="cb288-190">You may see hello following error message when hello authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="cb288-191">AADSTS70008: hello megadott hozzáférés biztosítása lejárt vagy visszavonták.</span><span class="sxs-lookup"><span data-stu-id="cb288-191">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="cb288-192">Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="cb288-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="cb288-193">Felhasználó típusa</span><span class="sxs-lookup"><span data-stu-id="cb288-193">User type</span></span> | <span data-ttu-id="cb288-194">Után lejár</span><span class="sxs-lookup"><span data-stu-id="cb288-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="cb288-195">Felhasználói fiókok Azure Active Directory által nem kezelt (@hotmail.com, @live.comstb.)</span><span class="sxs-lookup"><span data-stu-id="cb288-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="cb288-196">12 óra</span><span class="sxs-lookup"><span data-stu-id="cb288-196">12 hours</span></span> |
| <span data-ttu-id="cb288-197">Felhasználói fiókok felügyelete által Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="cb288-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="cb288-198">Futtatás után utolsó szelet hello 14 nap.</span><span class="sxs-lookup"><span data-stu-id="cb288-198">14 days after hello last slice run.</span></span> <br/><br/><span data-ttu-id="cb288-199">90 nap, ha a szelet OAuth-alapú társított szolgáltatás fut legalább 14 naponta.</span><span class="sxs-lookup"><span data-stu-id="cb288-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="cb288-200">tooavoid/hárítsa el a hiba, ismét engedélyezheti a hello segítségével **engedélyezés** gombbal hello **jogkivonat lejár** és telepítse újra a kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cb288-200">tooavoid/resolve this error, reauthorize using hello **Authorize** button when hello **token expires** and redeploy hello linked service.</span></span> <span data-ttu-id="cb288-201">Értékek is létrehozhat **sessionId** és **engedélyezési** programozott módon, az alábbiak szerint kód tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="cb288-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

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

<span data-ttu-id="cb288-202">Lásd: [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) részletes kapcsolatos témakörök kapcsolatos hello adat-előállító osztályok hello kódban használt.</span><span class="sxs-lookup"><span data-stu-id="cb288-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about hello Data Factory classes used in hello code.</span></span> <span data-ttu-id="cb288-203">Adjon hozzá egy hivatkozást,: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll hello WindowsFormsWebAuthenticationDialog osztály számára.</span><span class="sxs-lookup"><span data-stu-id="cb288-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for hello WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="cb288-204">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cb288-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="cb288-205">a következő JSON részlet hello egy Data Lake Analytics U-SQL-tevékenység az adatcsatorna határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb288-205">hello following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="cb288-206">hello tevékenység egy hivatkozást a korábban létrehozott társított Azure Data Lake Analytics szolgáltatás toohello van.</span><span class="sxs-lookup"><span data-stu-id="cb288-206">hello activity definition has a reference toohello Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
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

<span data-ttu-id="cb288-207">hello következő táblázatban neveit és leírásait, amelyek az adott toothis tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cb288-207">hello following table describes names and descriptions of properties that are specific toothis activity.</span></span> 

| <span data-ttu-id="cb288-208">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb288-208">Property</span></span> | <span data-ttu-id="cb288-209">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb288-209">Description</span></span> | <span data-ttu-id="cb288-210">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb288-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cb288-211">type</span><span class="sxs-lookup"><span data-stu-id="cb288-211">type</span></span> |<span data-ttu-id="cb288-212">hello type tulajdonság túl be kell állítani**DataLakeAnalyticsU-SQL**.</span><span class="sxs-lookup"><span data-stu-id="cb288-212">hello type property must be set too**DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="cb288-213">Igen</span><span class="sxs-lookup"><span data-stu-id="cb288-213">Yes</span></span> |
| <span data-ttu-id="cb288-214">scriptPath</span><span class="sxs-lookup"><span data-stu-id="cb288-214">scriptPath</span></span> |<span data-ttu-id="cb288-215">Elérési út toofolder hello U-SQL parancsfájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="cb288-215">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="cb288-216">Hello fájl neve nem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cb288-216">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="cb288-217">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="cb288-217">No (if you use script)</span></span> |
| <span data-ttu-id="cb288-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="cb288-218">scriptLinkedService</span></span> |<span data-ttu-id="cb288-219">Kapcsolódó szolgáltatás, amely a hello parancsfájl toohello adat-előállító tartalmazó hello tároló</span><span class="sxs-lookup"><span data-stu-id="cb288-219">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="cb288-220">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="cb288-220">No (if you use script)</span></span> |
| <span data-ttu-id="cb288-221">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cb288-221">script</span></span> |<span data-ttu-id="cb288-222">Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="cb288-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="cb288-223">Például: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="cb288-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="cb288-224">Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ)</span><span class="sxs-lookup"><span data-stu-id="cb288-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="cb288-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="cb288-225">degreeOfParallelism</span></span> |<span data-ttu-id="cb288-226">csomópontok maximális száma hello egyidejűleg használt toorun hello feladat.</span><span class="sxs-lookup"><span data-stu-id="cb288-226">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="cb288-227">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-227">No</span></span> |
| <span data-ttu-id="cb288-228">Prioritás</span><span class="sxs-lookup"><span data-stu-id="cb288-228">priority</span></span> |<span data-ttu-id="cb288-229">Meghatározza, hogy szereplő várólistáján szereplő feladatok közül kell lennie a kijelölt toorun először.</span><span class="sxs-lookup"><span data-stu-id="cb288-229">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="cb288-230">hello hello kevesebb, hello hello elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="cb288-230">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="cb288-231">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-231">No</span></span> |
| <span data-ttu-id="cb288-232">paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb288-232">parameters</span></span> |<span data-ttu-id="cb288-233">Hello U-SQL parancsfájl paramétereinek</span><span class="sxs-lookup"><span data-stu-id="cb288-233">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="cb288-234">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-234">No</span></span> |
| <span data-ttu-id="cb288-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="cb288-235">runtimeVersion</span></span> | <span data-ttu-id="cb288-236">U-SQL hello motor toouse futásidejű verzióját</span><span class="sxs-lookup"><span data-stu-id="cb288-236">Runtime version of hello U-SQL engine toouse</span></span> | <span data-ttu-id="cb288-237">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-237">No</span></span> | 
| <span data-ttu-id="cb288-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="cb288-238">compilationMode</span></span> | <p><span data-ttu-id="cb288-239">Fordítási mód az U-SQL.</span><span class="sxs-lookup"><span data-stu-id="cb288-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="cb288-240">A következő értékek egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="cb288-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="cb288-241">**Szemantikus:** csak a szemantikai ellenőrzések és a szükséges megerősítések végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="cb288-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="cb288-242">**Teljes:** hajtsa végre a hello teljes fordítási, beleértve a szintaxis-ellenőrzés, optimalizálás, kódgenerálás, stb.</span><span class="sxs-lookup"><span data-stu-id="cb288-242">**Full:** Perform hello full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="cb288-243">**SingleBox:** hello teljes fordítási a TargetType beállítás tooSingleBox végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="cb288-243">**SingleBox:** Perform hello full compilation, with TargetType setting tooSingleBox.</span></span></li></ul><p><span data-ttu-id="cb288-244">Ez a tulajdonság értékét nem adja meg, ha a hello server hello optimális lefordítására határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb288-244">If you don't specify a value for this property, hello server determines hello optimal compilation mode.</span></span> </p>| <span data-ttu-id="cb288-245">Nem</span><span class="sxs-lookup"><span data-stu-id="cb288-245">No</span></span> | 

<span data-ttu-id="cb288-246">Lásd: [SearchLogProcessing.txt parancsfájl Definition](#sample-u-sql-script) hello parancsfájl definíciójához.</span><span class="sxs-lookup"><span data-stu-id="cb288-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for hello script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="cb288-247">Minta bemeneti és kimeneti adatkészletek</span><span class="sxs-lookup"><span data-stu-id="cb288-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="cb288-248">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cb288-248">Input dataset</span></span>
<span data-ttu-id="cb288-249">Ebben a példában hello bemeneti adatokat az Azure Data Lake Store (searchlog.tsv fájl fájl hello datalake-/ bemeneti mappában) található.</span><span class="sxs-lookup"><span data-stu-id="cb288-249">In this example, hello input data resides in an Azure Data Lake Store (SearchLog.tsv file in hello datalake/input folder).</span></span> 

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

### <a name="output-dataset"></a><span data-ttu-id="cb288-250">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cb288-250">Output dataset</span></span>
<span data-ttu-id="cb288-251">Ebben a példában hello U-SQL-parancsfájl által előállított hello kimeneti adatok tárolódnak az Azure Data Lake Store (datalake-/ kimeneti mappa).</span><span class="sxs-lookup"><span data-stu-id="cb288-251">In this example, hello output data produced by hello U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

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

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="cb288-252">A minta Data Lake Store társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cb288-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="cb288-253">Itt található hello definition hello minta Azure Data Lake Store társított szolgáltatás hello bemeneti/kimeneti adatkészletek használják.</span><span class="sxs-lookup"><span data-stu-id="cb288-253">Here is hello definition of hello sample Azure Data Lake Store linked service used by hello input/output datasets.</span></span> 

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

<span data-ttu-id="cb288-254">Lásd: [tooand adatok áthelyezése az Azure Data Lake Store](data-factory-azure-datalake-connector.md) cikk a JSON-tulajdonságok leírását.</span><span class="sxs-lookup"><span data-stu-id="cb288-254">See [Move data tooand from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="cb288-255">Minta U-SQL parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cb288-255">Sample U-SQL Script</span></span>

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
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="cb288-256">az értékek hello ** @in ** és ** @out ** hello U-SQL parancsfájl átadott paraméterek dinamikusan által ADF hello "parameters" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="cb288-256">hello values for **@in** and **@out** parameters in hello U-SQL script are passed dynamically by ADF using hello ‘parameters’ section.</span></span> <span data-ttu-id="cb288-257">Hello csővezeték-definícióban hello "parameters" című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="cb288-257">See hello ‘parameters’ section in hello pipeline definition.</span></span>

<span data-ttu-id="cb288-258">Adhat meg egyéb tulajdonságait, például degreeOfParallelism és prioritását, valamint a feldolgozási sor hello Azure Data Lake Analytics szolgáltatásban futó feladatok hello definíciója.</span><span class="sxs-lookup"><span data-stu-id="cb288-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for hello jobs that run on hello Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="cb288-259">Dinamikus paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb288-259">Dynamic parameters</span></span>
<span data-ttu-id="cb288-260">Hello minta csővezeték-definícióban és paraméterek vannak hozzárendelve kódolt értékekkel.</span><span class="sxs-lookup"><span data-stu-id="cb288-260">In hello sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="cb288-261">Ehelyett is lehetséges toouse dinamikus paraméterek.</span><span class="sxs-lookup"><span data-stu-id="cb288-261">It is possible toouse dynamic parameters instead.</span></span> <span data-ttu-id="cb288-262">Példa:</span><span class="sxs-lookup"><span data-stu-id="cb288-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="cb288-263">Ebben az esetben a bemeneti fájlok továbbra is átveszik hello /datalake/input mappából, és a kimeneti fájlok jönnek létre hello /datalake/output mappában.</span><span class="sxs-lookup"><span data-stu-id="cb288-263">In this case, input files are still picked up from hello /datalake/input folder and output files are generated in hello /datalake/output folder.</span></span> <span data-ttu-id="cb288-264">hello fájlnevek dinamikusak hello szelet kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="cb288-264">hello file names are dynamic based on hello slice start time.</span></span>  

