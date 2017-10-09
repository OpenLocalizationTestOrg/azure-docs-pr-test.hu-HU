---
title: az Azure Data Lake Store aaaCopy adatok tooand |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatok tooand Data Lake Store az Azure Data Factory használatával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a><span data-ttu-id="bec2d-103">Adatok tooand másolása Data Lake Store a Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="bec2d-103">Copy data tooand from Data Lake Store by using Data Factory</span></span>
<span data-ttu-id="bec2d-104">Ez a cikk azt ismerteti, hogyan toouse másolási tevékenység az Azure Data Factory toomove adatok tooand az Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="bec2d-104">This article explains how toouse Copy Activity in Azure Data Factory toomove data tooand from Azure Data Lake Store.</span></span> <span data-ttu-id="bec2d-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során adatátvitel áttekintését.</span><span class="sxs-lookup"><span data-stu-id="bec2d-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, an overview of data movement with Copy Activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="bec2d-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="bec2d-106">Supported scenarios</span></span>
<span data-ttu-id="bec2d-107">Adatokat másolhat **az Azure Data Lake Store** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="bec2d-107">You can copy data **from Azure Data Lake Store** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="bec2d-108">Adatok másolása a következő adatokat tárolja hello **tooAzure Data Lake Store**:</span><span class="sxs-lookup"><span data-stu-id="bec2d-108">You can copy data from hello following data stores **tooAzure Data Lake Store**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="bec2d-109">Data Lake Store-fiók létrehozása a másolási tevékenység során a folyamat létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="bec2d-109">Create a Data Lake Store account before creating a pipeline with Copy Activity.</span></span> <span data-ttu-id="bec2d-110">További információkért lásd: [Ismerkedés az Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bec2d-110">For more information, see [Get started with Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="bec2d-111">Támogatott hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="bec2d-111">Supported authentication types</span></span>
<span data-ttu-id="bec2d-112">hello Data Lake Store összekötő ezeket hitelesítési típusokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="bec2d-112">hello Data Lake Store connector supports these authentication types:</span></span>
* <span data-ttu-id="bec2d-113">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="bec2d-113">Service principal authentication</span></span>
* <span data-ttu-id="bec2d-114">Felhasználói hitelesítő adatok (OAuth) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="bec2d-114">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="bec2d-115">Azt javasoljuk, hogy a szolgáltatás egyszerű hitelesítést, különösen egy ütemezett adatok másolását a használja.</span><span class="sxs-lookup"><span data-stu-id="bec2d-115">We recommend that you use service principal authentication, especially for a scheduled data copy.</span></span> <span data-ttu-id="bec2d-116">Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="bec2d-116">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="bec2d-117">További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-117">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>

## <a name="get-started"></a><span data-ttu-id="bec2d-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="bec2d-118">Get started</span></span>
<span data-ttu-id="bec2d-119">A másolási tevékenység, amely helyezi át az adatokat az Azure Data Lake Store vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="bec2d-119">You can create a pipeline with a copy activity that moves data to/from an Azure Data Lake Store by using different tools/APIs.</span></span>

<span data-ttu-id="bec2d-120">hello legegyszerűbb módja toocreate egy folyamat toocopy adatok toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-120">hello easiest way toocreate a pipeline toocopy data is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="bec2d-121">Az adatcsatorna létrehozásával hello másolása varázsló használatával oktatóanyagok esetén lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="bec2d-121">For a tutorial on creating a pipeline by using hello Copy Wizard, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="bec2d-122">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="bec2d-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="bec2d-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="bec2d-124">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="bec2d-125">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-125">Create a **data factory**.</span></span> <span data-ttu-id="bec2d-126">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="bec2d-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="bec2d-127">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="bec2d-128">Adatok másolása az Azure blob storage tooan Azure Data Lake Store az, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure Data Lake store tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-128">For example, if you are copying data from an Azure blob storage tooan Azure Data Lake Store, you create two linked services toolink your Azure storage account and Azure Data Lake store tooyour data factory.</span></span> <span data-ttu-id="bec2d-129">Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure Data Lake Store, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-129">For linked service properties that are specific tooAzure Data Lake Store, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="bec2d-130">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="bec2d-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="bec2d-131">Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="bec2d-131">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="bec2d-132">És egy másik dataset toospecify hello mappa és a fájlelérési út hello Data Lake Store-ban hello blob-tároló átmásolva hello adatokat tartalmazó hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="bec2d-132">And, you create another dataset toospecify hello folder and file path in hello Data Lake store that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="bec2d-133">Adatkészlet tulajdonságai, amelyek adott tooAzure Data Lake Store, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-133">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="bec2d-134">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="bec2d-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="bec2d-135">A korábban említett hello példában BlobSource forrás-és AzureDataLakeStoreSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="bec2d-135">In hello example mentioned earlier, you use BlobSource as a source and AzureDataLakeStoreSink as a sink for hello copy activity.</span></span> <span data-ttu-id="bec2d-136">Ehhez hasonlóan az Azure Data Lake Store tooAzure Blob Storage másolása, használható AzureDataLakeStoreSource és BlobSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="bec2d-136">Similarly, if you are copying from Azure Data Lake Store tooAzure Blob Storage, you use AzureDataLakeStoreSource  and BlobSink in hello copy activity.</span></span> <span data-ttu-id="bec2d-137">A másolási tevékenység tulajdonságait, amelyek adott tooAzure Data Lake Store, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-137">For copy activity properties that are specific tooAzure Data Lake Store, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="bec2d-138">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="bec2d-139">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="bec2d-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="bec2d-140">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="bec2d-141">JSON-definíciók, amelyek használt toocopy adatokat az Azure Data Lake Store az adat-előállító entitások minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-data-lake-store) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="bec2d-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Data Lake Store, see [JSON examples](#json-examples-for-copying-data-to-and-from-data-lake-store) section of this article.</span></span>

<span data-ttu-id="bec2d-142">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooData Lake Store részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="bec2d-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooData Lake Store.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="bec2d-143">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="bec2d-143">Linked service properties</span></span>
<span data-ttu-id="bec2d-144">A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-144">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="bec2d-145">Típusú társított szolgáltatás létrehozása **AzureDataLakeStore** toolink a Data Lake Store adatok tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-145">You create a linked service of type **AzureDataLakeStore** toolink your Data Lake Store data tooyour data factory.</span></span> <span data-ttu-id="bec2d-146">a következő táblázat hello JSON elemek adott tooData Lake Store kapcsolódó szolgáltatások ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bec2d-146">hello following table describes JSON elements specific tooData Lake Store linked services.</span></span> <span data-ttu-id="bec2d-147">Egyszerű szolgáltatásnév és a felhasználói hitelesítő adatok hitelesítése közül választhat.</span><span class="sxs-lookup"><span data-stu-id="bec2d-147">You can choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="bec2d-148">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-148">Property</span></span> | <span data-ttu-id="bec2d-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-149">Description</span></span> | <span data-ttu-id="bec2d-150">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-150">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bec2d-151">**típusa**</span><span class="sxs-lookup"><span data-stu-id="bec2d-151">**type**</span></span> | <span data-ttu-id="bec2d-152">hello type tulajdonság túl be kell állítani**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-152">hello type property must be set too**AzureDataLakeStore**.</span></span> | <span data-ttu-id="bec2d-153">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-153">Yes</span></span> |
| <span data-ttu-id="bec2d-154">**dataLakeStoreUri**</span><span class="sxs-lookup"><span data-stu-id="bec2d-154">**dataLakeStoreUri**</span></span> | <span data-ttu-id="bec2d-155">Hello Azure Data Lake Store-fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="bec2d-155">Information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="bec2d-156">Ezt az információt fogad hello a következő formátumok egyikét: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="bec2d-156">This information takes one of hello following formats: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="bec2d-157">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-157">Yes</span></span> |
| <span data-ttu-id="bec2d-158">**előfizetés-azonosító**</span><span class="sxs-lookup"><span data-stu-id="bec2d-158">**subscriptionId**</span></span> | <span data-ttu-id="bec2d-159">Azure-előfizetés azonosítója toowhich hello Data Lake Store-fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-159">Azure subscription ID toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="bec2d-160">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-160">Required for sink</span></span> |
| <span data-ttu-id="bec2d-161">**erőforráscsoport-név**</span><span class="sxs-lookup"><span data-stu-id="bec2d-161">**resourceGroupName**</span></span> | <span data-ttu-id="bec2d-162">Azure-erőforrás csoport neve toowhich hello Data Lake Store-fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-162">Azure resource group name toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="bec2d-163">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-163">Required for sink</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="bec2d-164">Szolgáltatás egyszerű hitelesítés (ajánlott)</span><span class="sxs-lookup"><span data-stu-id="bec2d-164">Service principal authentication (recommended)</span></span>
<span data-ttu-id="bec2d-165">toouse szolgáltatás egyszerű hitelesítési regisztrálása az Azure Active Directory (Azure AD) és támogatás azt hello alkalmazás entitás tooData Lake áruház eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="bec2d-165">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="bec2d-166">Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="bec2d-166">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="bec2d-167">Jegyezze fel az értéket, amely a következő hello toodefine hello társított szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="bec2d-167">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="bec2d-168">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="bec2d-168">Application ID</span></span>
* <span data-ttu-id="bec2d-169">Alkalmazás kulcs</span><span class="sxs-lookup"><span data-stu-id="bec2d-169">Application key</span></span> 
* <span data-ttu-id="bec2d-170">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="bec2d-170">Tenant ID</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bec2d-171">Ha hello másolása varázsló tooauthor adatok folyamatok használ, győződjön meg arról, hogy megadta-e egyszerű hello szolgáltatást legalább egy **olvasó** hello Data Lake Store-fiók hozzáférés-vezérlés (identitások és hozzáférések felügyeletéhez) szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-171">If you are using hello Copy Wizard tooauthor data pipelines, make sure that you grant hello service principal at least a **Reader** role in access control (identity and access management) for hello Data Lake Store account.</span></span> <span data-ttu-id="bec2d-172">Is, legalább az egyszerű hello szolgáltatást biztosítania **olvasási + Execute** engedély tooyour Data Lake Store gyökér ("/") és gyermekét.</span><span class="sxs-lookup"><span data-stu-id="bec2d-172">Also, grant hello service principal at least **Read + Execute** permission tooyour Data Lake Store root ("/") and its children.</span></span> <span data-ttu-id="bec2d-173">Ellenkező esetben láthatja üdvözlőüzenetére "hello megadott hitelesítő adatok érvénytelenek."</span><span class="sxs-lookup"><span data-stu-id="bec2d-173">Otherwise you might see hello message "hello credentials provided are invalid."</span></span><br/><br/>
<span data-ttu-id="bec2d-174">Miután létrehozása vagy frissítése egy egyszerű szolgáltatást az Azure ad-ben, hello módosítások tootake hatás néhány percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="bec2d-174">After you create or update a service principal in Azure AD, it can take a few minutes for hello changes tootake effect.</span></span> <span data-ttu-id="bec2d-175">Ellenőrizze a hello egyszerű szolgáltatásnév és a Data Lake Store access control lista (ACL) konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="bec2d-175">Check hello service principal and Data Lake Store access control list (ACL) configurations.</span></span> <span data-ttu-id="bec2d-176">Ha továbbra is megjelenik az "a megadott hitelesítő adatok érvénytelenek hello" hello üzenet, várjon egy kicsit, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="bec2d-176">If you still see hello message "hello credentials provided are invalid," wait a while and try again.</span></span>

<span data-ttu-id="bec2d-177">Szolgáltatás egyszerű hitelesítés használata a következő tulajdonságok hello megadásával:</span><span class="sxs-lookup"><span data-stu-id="bec2d-177">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="bec2d-178">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-178">Property</span></span> | <span data-ttu-id="bec2d-179">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-179">Description</span></span> | <span data-ttu-id="bec2d-180">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-180">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bec2d-181">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="bec2d-181">**servicePrincipalId**</span></span> | <span data-ttu-id="bec2d-182">Adja meg a hello alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="bec2d-182">Specify hello application's client ID.</span></span> | <span data-ttu-id="bec2d-183">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-183">Yes</span></span> |
| <span data-ttu-id="bec2d-184">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="bec2d-184">**servicePrincipalKey**</span></span> | <span data-ttu-id="bec2d-185">Adja meg a hello kulcsát.</span><span class="sxs-lookup"><span data-stu-id="bec2d-185">Specify hello application's key.</span></span> | <span data-ttu-id="bec2d-186">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-186">Yes</span></span> |
| <span data-ttu-id="bec2d-187">**Bérlői**</span><span class="sxs-lookup"><span data-stu-id="bec2d-187">**tenant**</span></span> | <span data-ttu-id="bec2d-188">Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="bec2d-188">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="bec2d-189">Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="bec2d-189">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="bec2d-190">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-190">Yes</span></span> |

<span data-ttu-id="bec2d-191">**Példa: Szolgáltatás egyszerű hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="bec2d-191">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="bec2d-192">Felhasználói hitelesítő adatok hitelesítése</span><span class="sxs-lookup"><span data-stu-id="bec2d-192">User credential authentication</span></span>
<span data-ttu-id="bec2d-193">Másik megoldásként használhatja felhasználói hitelesítő adatok hitelesítése toocopy a vagy tooData Lake Store megadásával következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-193">Alternatively, you can use user credential authentication toocopy from or tooData Lake Store by specifying hello following properties:</span></span>

| <span data-ttu-id="bec2d-194">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-194">Property</span></span> | <span data-ttu-id="bec2d-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-195">Description</span></span> | <span data-ttu-id="bec2d-196">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-196">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bec2d-197">**engedélyezési**</span><span class="sxs-lookup"><span data-stu-id="bec2d-197">**authorization**</span></span> | <span data-ttu-id="bec2d-198">Kattintson a hello **engedélyezés** hello Data Factory Editor gombra, és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="bec2d-198">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="bec2d-199">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-199">Yes</span></span> |
| <span data-ttu-id="bec2d-200">**munkamenet-azonosító**</span><span class="sxs-lookup"><span data-stu-id="bec2d-200">**sessionId**</span></span> | <span data-ttu-id="bec2d-201">OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="bec2d-201">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="bec2d-202">Minden munkamenet-azonosító egyedi, és csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="bec2d-202">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="bec2d-203">Ez a beállítás automatikusan létrejön a Data Factory Editor hello használatakor.</span><span class="sxs-lookup"><span data-stu-id="bec2d-203">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="bec2d-204">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-204">Yes</span></span> |

<span data-ttu-id="bec2d-205">**Példa: Felhasználók hitelesítő adatok hitelesítése**</span><span class="sxs-lookup"><span data-stu-id="bec2d-205">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="bec2d-206">Jogkivonat lejáratáról</span><span class="sxs-lookup"><span data-stu-id="bec2d-206">Token expiration</span></span>
<span data-ttu-id="bec2d-207">engedélyezési kód hello segítségével generáló hello **engedélyezés** gombra egy bizonyos idő elteltével lejár.</span><span class="sxs-lookup"><span data-stu-id="bec2d-207">hello authorization code that you generate by using hello **Authorize** button expires after a certain amount of time.</span></span> <span data-ttu-id="bec2d-208">a következő üzenet hello azt jelenti, hogy adott hello hitelesítésére szolgáló jogkivonat érvényessége lejárt:</span><span class="sxs-lookup"><span data-stu-id="bec2d-208">hello following message means that hello authentication token has expired:</span></span>

<span data-ttu-id="bec2d-209">Hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="bec2d-209">Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="bec2d-210">AADSTS70008: hello megadott hozzáférés biztosítása lejárt vagy visszavonták.</span><span class="sxs-lookup"><span data-stu-id="bec2d-210">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="bec2d-211">Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21-09-31Z.</span><span class="sxs-lookup"><span data-stu-id="bec2d-211">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.</span></span>

<span data-ttu-id="bec2d-212">hello következő táblázatban a különböző típusú felhasználói fiókok hello lejárati ideje:</span><span class="sxs-lookup"><span data-stu-id="bec2d-212">hello following table shows hello expiration times of different types of user accounts:</span></span>


| <span data-ttu-id="bec2d-213">Felhasználó típusa</span><span class="sxs-lookup"><span data-stu-id="bec2d-213">User type</span></span> | <span data-ttu-id="bec2d-214">Után lejár</span><span class="sxs-lookup"><span data-stu-id="bec2d-214">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="bec2d-215">Felhasználói fiókok *nem* kezeli az Azure Active Directoryban (például @hotmail.com vagy @live.com)</span><span class="sxs-lookup"><span data-stu-id="bec2d-215">User accounts *not* managed by Azure Active Directory (for example, @hotmail.com or @live.com)</span></span> |<span data-ttu-id="bec2d-216">12 óra</span><span class="sxs-lookup"><span data-stu-id="bec2d-216">12 hours</span></span> |
| <span data-ttu-id="bec2d-217">Felhasználói fiókok Azure Active Directory által felügyelt</span><span class="sxs-lookup"><span data-stu-id="bec2d-217">Users accounts managed by Azure Active Directory</span></span> |<span data-ttu-id="bec2d-218">14 napja után utolsó szelet hello futtatása</span><span class="sxs-lookup"><span data-stu-id="bec2d-218">14 days after hello last slice run</span></span> <br/><br/><span data-ttu-id="bec2d-219">90 nap, ha egy olyan OAuth-alapú társított szolgáltatás szelet 14 naponta legalább egyszer fut</span><span class="sxs-lookup"><span data-stu-id="bec2d-219">90 days, if a slice based on an OAuth-based linked service runs at least once every 14 days</span></span> |

<span data-ttu-id="bec2d-220">Ha a jelszó módosítása előtt hello jogkivonat lejárati ideje, hello jogkivonat azonnal lejár.</span><span class="sxs-lookup"><span data-stu-id="bec2d-220">If you change your password before hello token expiration time, hello token expires immediately.</span></span> <span data-ttu-id="bec2d-221">Ebben a szakaszban korábban említett hello üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bec2d-221">You will see hello message mentioned earlier in this section.</span></span>

<span data-ttu-id="bec2d-222">Hello segítségével tudja ismét engedélyezheti hello fiók **engedélyezés** gomb, amikor hello jogkivonat lejár tooredeploy hello társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bec2d-222">You can reauthorize hello account by using hello **Authorize** button when hello token expires tooredeploy hello linked service.</span></span> <span data-ttu-id="bec2d-223">Hello értékeket is létrehozhat **sessionId** és **engedélyezési** programozott módon használatával tulajdonságok hello következő kódot:</span><span class="sxs-lookup"><span data-stu-id="bec2d-223">You can also generate values for hello **sessionId** and **authorization** properties programmatically by using hello following code:</span></span>


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
<span data-ttu-id="bec2d-224">Hello kódban használt hello adat-előállító osztályok, lásd: hello [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [ AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="bec2d-224">For details about hello Data Factory classes used in hello code, see hello [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics.</span></span> <span data-ttu-id="bec2d-225">Adja hozzá egy hivatkozást tooversion `2.9.10826.1824` a `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` a hello `WindowsFormsWebAuthenticationDialog` hello kódban használt osztály.</span><span class="sxs-lookup"><span data-stu-id="bec2d-225">Add a reference tooversion `2.9.10826.1824` of `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` for hello `WindowsFormsWebAuthenticationDialog` class used in hello code.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="bec2d-226">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="bec2d-226">Dataset properties</span></span>
<span data-ttu-id="bec2d-227">hello beállítása toospecify egy adatkészlet toorepresent bemeneti adatok egy Data Lake Store-ban, **típus** hello dataset tulajdonsága túl**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-227">toospecify a dataset toorepresent input data in a Data Lake Store, you set hello **type** property of hello dataset too**AzureDataLakeStore**.</span></span> <span data-ttu-id="bec2d-228">Set hello **linkedServiceName** hello dataset toohello nevének hello Data Lake Store tulajdonság társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bec2d-228">Set hello **linkedServiceName** property of hello dataset toohello name of hello Data Lake Store linked service.</span></span> <span data-ttu-id="bec2d-229">JSON-szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-229">For a full list of JSON sections and properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="bec2d-230">A JSON-ban, a DataSet adatkészlet szakaszok például **struktúra**, **rendelkezésre állási**, és **házirend**, minden adatkészlet esetében hasonló (Azure SQL-adatbázis, az Azure blob és az Azure tábla a Példa).</span><span class="sxs-lookup"><span data-stu-id="bec2d-230">Sections of a dataset in JSON, such as **structure**, **availability**, and **policy**, are similar for all dataset types (Azure SQL database, Azure blob, and Azure table, for example).</span></span> <span data-ttu-id="bec2d-231">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú, és megjelenik többek között a helyét és hello adattár hello adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="bec2d-231">hello **typeProperties** section is different for each type of dataset and provides information such as location and format of hello data in hello data store.</span></span> 

<span data-ttu-id="bec2d-232">Hello **typeProperties** szakasz egy adatkészlet típusú **AzureDataLakeStore** következő tulajdonságai hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="bec2d-232">hello **typeProperties** section for a dataset of type **AzureDataLakeStore** contains hello following properties:</span></span>

| <span data-ttu-id="bec2d-233">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-233">Property</span></span> | <span data-ttu-id="bec2d-234">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-234">Description</span></span> | <span data-ttu-id="bec2d-235">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-235">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bec2d-236">**folderPath**</span><span class="sxs-lookup"><span data-stu-id="bec2d-236">**folderPath**</span></span> |<span data-ttu-id="bec2d-237">Elérési út toohello tároló és mappa Data Lake Store-ban.</span><span class="sxs-lookup"><span data-stu-id="bec2d-237">Path toohello container and folder in Data Lake Store.</span></span> |<span data-ttu-id="bec2d-238">Igen</span><span class="sxs-lookup"><span data-stu-id="bec2d-238">Yes</span></span> |
| <span data-ttu-id="bec2d-239">**Fájlnév**</span><span class="sxs-lookup"><span data-stu-id="bec2d-239">**fileName**</span></span> |<span data-ttu-id="bec2d-240">Az Azure Data Lake Store hello fájl neve.</span><span class="sxs-lookup"><span data-stu-id="bec2d-240">Name of hello file in Azure Data Lake Store.</span></span> <span data-ttu-id="bec2d-241">Hello **Fájlnév** tulajdonság nem kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="bec2d-241">hello **fileName** property is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="bec2d-242">Ha megad **Fájlnév**, hello tevékenység (például a Másolás) működik-e az adott fájl hello.</span><span class="sxs-lookup"><span data-stu-id="bec2d-242">If you specify **fileName**, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="bec2d-243">Ha **Fájlnév** nincs megadva, másolása tartalmazza az összes fájl **folderPath** hello bemeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="bec2d-243">When **fileName** is not specified, Copy includes all files in **folderPath** in hello input dataset.</span></span><br/><br/><span data-ttu-id="bec2d-244">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello hello létrehozott fájl neve nem hello formátumú adatok. _GUID_.txt ".</span><span class="sxs-lookup"><span data-stu-id="bec2d-244">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello format Data._Guid_.txt\`.</span></span> <span data-ttu-id="bec2d-245">Például: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span><span class="sxs-lookup"><span data-stu-id="bec2d-245">For example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span></span> |<span data-ttu-id="bec2d-246">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-246">No</span></span> |
| <span data-ttu-id="bec2d-247">**partitionedBy**</span><span class="sxs-lookup"><span data-stu-id="bec2d-247">**partitionedBy**</span></span> |<span data-ttu-id="bec2d-248">Hello **partitionedBy** tulajdonság nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="bec2d-248">hello **partitionedBy** property is optional.</span></span> <span data-ttu-id="bec2d-249">Toospecify egy dinamikus elérési útját és fájlnevét-idősoros adatok használhatja.</span><span class="sxs-lookup"><span data-stu-id="bec2d-249">You can use it toospecify a dynamic path and file name for time-series data.</span></span> <span data-ttu-id="bec2d-250">Például **folderPath** adatok órára is lehet paraméterezett.</span><span class="sxs-lookup"><span data-stu-id="bec2d-250">For example, **folderPath** can be parameterized for every hour of data.</span></span> <span data-ttu-id="bec2d-251">Részletek és példák: [partitionedBy tulajdonság hello](#using-partitionedby-property).</span><span class="sxs-lookup"><span data-stu-id="bec2d-251">For details and examples, see [hello partitionedBy property](#using-partitionedby-property).</span></span> |<span data-ttu-id="bec2d-252">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-252">No</span></span> |
| <span data-ttu-id="bec2d-253">**formátumban**</span><span class="sxs-lookup"><span data-stu-id="bec2d-253">**format**</span></span> | <span data-ttu-id="bec2d-254">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, és  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-254">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, and **ParquetFormat**.</span></span> <span data-ttu-id="bec2d-255">Set hello **típus** tulajdonság alapján **formátum** tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bec2d-255">Set hello **type** property under **format** tooone of these values.</span></span> <span data-ttu-id="bec2d-256">További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) hello részeiben [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-256">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections in hello [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span> <br><br> <span data-ttu-id="bec2d-257">Ha azt szeretné, hogy toocopy fájlok ",-van" közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello `format` mindkét bemeneti és kimeneti adatkészlet-definíciókban szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-257">If you want toocopy files "as-is" between file-based stores (binary copy), skip hello `format` section in both input and output dataset definitions.</span></span> |<span data-ttu-id="bec2d-258">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-258">No</span></span> |
| <span data-ttu-id="bec2d-259">**tömörítés**</span><span class="sxs-lookup"><span data-stu-id="bec2d-259">**compression**</span></span> | <span data-ttu-id="bec2d-260">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="bec2d-260">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="bec2d-261">Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-261">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="bec2d-262">Támogatott szintek a következők **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="bec2d-262">Supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="bec2d-263">További információkért lásd: [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="bec2d-263">For more information, see [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="bec2d-264">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-264">No</span></span> |

### <a name="hello-partitionedby-property"></a><span data-ttu-id="bec2d-265">hello partitionedBy tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-265">hello partitionedBy property</span></span>
<span data-ttu-id="bec2d-266">Megadhatja, hogy a dinamikus **folderPath** és **Fájlnév** hello idősorozat adatokhoz tulajdonságok **partitionedBy** tulajdonság, a Data Factory funkciók és a rendszer változók.</span><span class="sxs-lookup"><span data-stu-id="bec2d-266">You can specify dynamic **folderPath** and **fileName** properties for time-series data with hello **partitionedBy** property, Data Factory functions, and system variables.</span></span> <span data-ttu-id="bec2d-267">További információkért lásd: hello [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-267">For details, see hello [Azure Data Factory - functions and system variables](data-factory-functions-variables.md) article.</span></span>


<span data-ttu-id="bec2d-268">A példában a következő hello `{Slice}` hello érték hello adat-előállító rendszer változó helyére `SliceStart` megadott hello formátumban (`yyyyMMddHH`).</span><span class="sxs-lookup"><span data-stu-id="bec2d-268">In hello following example, `{Slice}` is replaced with hello value of hello Data Factory system variable `SliceStart` in hello format specified (`yyyyMMddHH`).</span></span> <span data-ttu-id="bec2d-269">hello neve `SliceStart` toohello kezdő időpontjának hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-269">hello name `SliceStart` refers toohello start time of hello slice.</span></span> <span data-ttu-id="bec2d-270">Hello `folderPath` tulajdonság nem az egyes szeletek, mint az azonos `wikidatagateway/wikisampledataout/2014100103` vagy `wikidatagateway/wikisampledataout/2014100104`.</span><span class="sxs-lookup"><span data-stu-id="bec2d-270">hello `folderPath` property is different for each slice, as in `wikidatagateway/wikisampledataout/2014100103` or `wikidatagateway/wikisampledataout/2014100104`.</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="bec2d-271">A következő példa, hello év, hónap, nap és idején hello a `SliceStart` ki kell olvasni a külön változók hello által használt `folderPath` és `fileName` tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="bec2d-271">In hello following example, hello year, month, day, and time of `SliceStart` are extracted into separate variables that are used by hello `folderPath` and `fileName` properties:</span></span>
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="bec2d-272">Idősorozat adatkészletek olvashat, ütemezés és a szeletek, lásd: hello [Azure Data Factory adathalmazok](data-factory-create-datasets.md) és [adat-előállító ütemezés és a végrehajtási](data-factory-scheduling-and-execution.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="bec2d-272">For more details on time-series datasets, scheduling, and slices, see hello [Datasets in Azure Data Factory](data-factory-create-datasets.md) and [Data Factory scheduling and execution](data-factory-scheduling-and-execution.md) articles.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="bec2d-273">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="bec2d-273">Copy activity properties</span></span>
<span data-ttu-id="bec2d-274">Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-274">For a full list of sections and properties available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="bec2d-275">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="bec2d-275">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="bec2d-276">tulajdonságok érhetők el hello hello **typeProperties** tevékenység szakasz tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="bec2d-276">hello properties available in hello **typeProperties** section of an activity vary with each activity type.</span></span> <span data-ttu-id="bec2d-277">A másolási tevékenységhez változik források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="bec2d-277">For a copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="bec2d-278">**AzureDataLakeStoreSource** támogatja a következő tulajdonság a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="bec2d-278">**AzureDataLakeStoreSource** supports hello following property in hello **typeProperties** section:</span></span>

| <span data-ttu-id="bec2d-279">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-279">Property</span></span> | <span data-ttu-id="bec2d-280">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-280">Description</span></span> | <span data-ttu-id="bec2d-281">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="bec2d-281">Allowed values</span></span> | <span data-ttu-id="bec2d-282">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-282">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bec2d-283">**rekurzív**</span><span class="sxs-lookup"><span data-stu-id="bec2d-283">**recursive**</span></span> |<span data-ttu-id="bec2d-284">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="bec2d-284">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="bec2d-285">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="bec2d-285">True (default value), False</span></span> |<span data-ttu-id="bec2d-286">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-286">No</span></span> |


<span data-ttu-id="bec2d-287">**AzureDataLakeStoreSink** támogatja a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="bec2d-287">**AzureDataLakeStoreSink** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="bec2d-288">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bec2d-288">Property</span></span> | <span data-ttu-id="bec2d-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="bec2d-289">Description</span></span> | <span data-ttu-id="bec2d-290">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="bec2d-290">Allowed values</span></span> | <span data-ttu-id="bec2d-291">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bec2d-291">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bec2d-292">**copyBehavior**</span><span class="sxs-lookup"><span data-stu-id="bec2d-292">**copyBehavior**</span></span> |<span data-ttu-id="bec2d-293">Meghatározza a hello másolási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="bec2d-293">Specifies hello copy behavior.</span></span> |<span data-ttu-id="bec2d-294"><b>PreserveHierarchy</b>: hello fájl hierarchia hello célmappában megőrzi.</span><span class="sxs-lookup"><span data-stu-id="bec2d-294"><b>PreserveHierarchy</b>: Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="bec2d-295">hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.</span><span class="sxs-lookup"><span data-stu-id="bec2d-295">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="bec2d-296"><b>FlattenHierarchy</b>: hello forrásmappából minden fájl első szintű hello hello célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="bec2d-296"><b>FlattenHierarchy</b>: All files from hello source folder are created in hello first level of hello target folder.</span></span> <span data-ttu-id="bec2d-297">hello fájljaira jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="bec2d-297">hello target files are created with autogenerated names.</span></span><br/><br/><span data-ttu-id="bec2d-298"><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti.</span><span class="sxs-lookup"><span data-stu-id="bec2d-298"><b>MergeFiles</b>: Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="bec2d-299">Ha hello fájl vagy a blob neve meg van adva, a hello egyesített fájl neve az hello megadott név.</span><span class="sxs-lookup"><span data-stu-id="bec2d-299">If hello file or blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="bec2d-300">Ellenkező esetben hello fájlnév rendszer automatikusan elnevezi.</span><span class="sxs-lookup"><span data-stu-id="bec2d-300">Otherwise, hello file name is autogenerated.</span></span> |<span data-ttu-id="bec2d-301">Nem</span><span class="sxs-lookup"><span data-stu-id="bec2d-301">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="bec2d-302">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="bec2d-302">recursive and copyBehavior examples</span></span>
<span data-ttu-id="bec2d-303">Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk rekurzív és copybehavior tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="bec2d-303">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="bec2d-304">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="bec2d-304">recursive</span></span> | <span data-ttu-id="bec2d-305">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="bec2d-305">copyBehavior</span></span> | <span data-ttu-id="bec2d-306">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="bec2d-306">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bec2d-307">Igaz</span><span class="sxs-lookup"><span data-stu-id="bec2d-307">true</span></span> |<span data-ttu-id="bec2d-308">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="bec2d-308">preserveHierarchy</span></span> |<span data-ttu-id="bec2d-309">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-309">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-310">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-310">Folder1</span></span><br/><span data-ttu-id="bec2d-311">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-317">hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre</span><span class="sxs-lookup"><span data-stu-id="bec2d-317">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="bec2d-318">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-318">Folder1</span></span><br/><span data-ttu-id="bec2d-319">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="bec2d-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="bec2d-325">Igaz</span><span class="sxs-lookup"><span data-stu-id="bec2d-325">true</span></span> |<span data-ttu-id="bec2d-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="bec2d-326">flattenHierarchy</span></span> |<span data-ttu-id="bec2d-327">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-327">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-328">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-328">Folder1</span></span><br/><span data-ttu-id="bec2d-329">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-335">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="bec2d-335">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-336">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-336">Folder1</span></span><br/><span data-ttu-id="bec2d-337">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="bec2d-338">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="bec2d-339">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-339">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="bec2d-340">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-340">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="bec2d-341">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-341">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="bec2d-342">Igaz</span><span class="sxs-lookup"><span data-stu-id="bec2d-342">true</span></span> |<span data-ttu-id="bec2d-343">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="bec2d-343">mergeFiles</span></span> |<span data-ttu-id="bec2d-344">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-344">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-345">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-345">Folder1</span></span><br/><span data-ttu-id="bec2d-346">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-352">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="bec2d-352">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-353">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-353">Folder1</span></span><br/><span data-ttu-id="bec2d-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek</span><span class="sxs-lookup"><span data-stu-id="bec2d-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="bec2d-355">hamis</span><span class="sxs-lookup"><span data-stu-id="bec2d-355">false</span></span> |<span data-ttu-id="bec2d-356">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="bec2d-356">preserveHierarchy</span></span> |<span data-ttu-id="bec2d-357">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-357">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="bec2d-358">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-358">Folder1</span></span><br/><span data-ttu-id="bec2d-359">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-365">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="bec2d-365">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="bec2d-366">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-366">Folder1</span></span><br/><span data-ttu-id="bec2d-367">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="bec2d-369">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-369">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="bec2d-370">hamis</span><span class="sxs-lookup"><span data-stu-id="bec2d-370">false</span></span> |<span data-ttu-id="bec2d-371">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="bec2d-371">flattenHierarchy</span></span> |<span data-ttu-id="bec2d-372">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-372">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="bec2d-373">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-373">Folder1</span></span><br/><span data-ttu-id="bec2d-374">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-380">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="bec2d-380">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="bec2d-381">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-381">Folder1</span></span><br/><span data-ttu-id="bec2d-382">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-382">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="bec2d-383">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-383">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="bec2d-384">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-384">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="bec2d-385">hamis</span><span class="sxs-lookup"><span data-stu-id="bec2d-385">false</span></span> |<span data-ttu-id="bec2d-386">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="bec2d-386">mergeFiles</span></span> |<span data-ttu-id="bec2d-387">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bec2d-387">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="bec2d-388">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-388">Folder1</span></span><br/><span data-ttu-id="bec2d-389">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="bec2d-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="bec2d-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="bec2d-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="bec2d-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="bec2d-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="bec2d-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="bec2d-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="bec2d-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="bec2d-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="bec2d-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="bec2d-395">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="bec2d-395">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="bec2d-396">Mappa1</span><span class="sxs-lookup"><span data-stu-id="bec2d-396">Folder1</span></span><br/><span data-ttu-id="bec2d-397">&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma.</span><span class="sxs-lookup"><span data-stu-id="bec2d-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="bec2d-398">automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="bec2d-398">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="bec2d-399">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="bec2d-399">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="bec2d-400">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="bec2d-400">Supported file and compression formats</span></span>
<span data-ttu-id="bec2d-401">További információkért lásd: hello [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-401">For details, see hello [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a><span data-ttu-id="bec2d-402">JSON például a Data Lake Store-ból adatokat tooand másolása</span><span class="sxs-lookup"><span data-stu-id="bec2d-402">JSON examples for copying data tooand from Data Lake Store</span></span>
<span data-ttu-id="bec2d-403">a következő példák hello adja meg a minta JSON-definíciók.</span><span class="sxs-lookup"><span data-stu-id="bec2d-403">hello following examples provide sample JSON definitions.</span></span> <span data-ttu-id="bec2d-404">Hello mezővel használhat ezen minta definíciók toocreate adatcsatorna [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="bec2d-404">You can use these sample definitions toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="bec2d-405">Példák megjelenítése hogyan hello toocopy adatok tooand Data Lake Store és az Azure Blob-tárolóból.</span><span class="sxs-lookup"><span data-stu-id="bec2d-405">hello examples show how toocopy data tooand from Data Lake Store and Azure Blob storage.</span></span> <span data-ttu-id="bec2d-406">Azonban az adatok átmásolhatók _közvetlenül_ bármelyik hello források tooany hello támogatott nyelő.</span><span class="sxs-lookup"><span data-stu-id="bec2d-406">However, data can be copied _directly_ from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="bec2d-407">További információkért lásd: hello szakasz "támogatott adattárolókhoz és formátumok" hello [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-407">For more information, see hello section "Supported data stores and formats" in hello [Move data by using Copy Activity](data-factory-data-movement-activities.md) article.</span></span>  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a><span data-ttu-id="bec2d-408">Példa: Adatok másolása az Azure Blob Storage tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="bec2d-408">Example: Copy data from Azure Blob Storage tooAzure Data Lake Store</span></span>
<span data-ttu-id="bec2d-409">Ebben a szakaszban hello példakód mutatja:</span><span class="sxs-lookup"><span data-stu-id="bec2d-409">hello example code in this section shows:</span></span>

* <span data-ttu-id="bec2d-410">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-410">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="bec2d-411">A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-411">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="bec2d-412">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-412">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="bec2d-413">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-413">An output [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="bec2d-414">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [AzureDataLakeStoreSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-414">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureDataLakeStoreSink](#copy-activity-properties).</span></span>

<span data-ttu-id="bec2d-415">hello példák azt szemléltetik, hogyan idősorozat adatokat az Azure Blob Storage van másolt tooData Lake Store óránként.</span><span class="sxs-lookup"><span data-stu-id="bec2d-415">hello examples show how time-series data from Azure Blob Storage is copied tooData Lake Store every hour.</span></span> 

<span data-ttu-id="bec2d-416">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="bec2d-416">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="bec2d-417">**Azure Data Lake Store társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="bec2d-417">**Azure Data Lake Store linked service**</span></span>

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="bec2d-418">További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-418">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="bec2d-419">**Azure blobbemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="bec2d-419">**Azure blob input dataset**</span></span>

<span data-ttu-id="bec2d-420">A következő példa hello, adatok van felvett egy új blobból minden órában (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="bec2d-420">In hello following example, data is picked up from a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="bec2d-421">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="bec2d-421">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="bec2d-422">hello mappa elérési útját használja hello év, hónap és nap részéhez hello kezdési ideje.</span><span class="sxs-lookup"><span data-stu-id="bec2d-422">hello folder path uses hello year, month, and day portion of hello start time.</span></span> <span data-ttu-id="bec2d-423">hello fájlnév hello része kezdési időpont hello óra használja.</span><span class="sxs-lookup"><span data-stu-id="bec2d-423">hello file name uses hello hour portion of hello start time.</span></span> <span data-ttu-id="bec2d-424">Hello `"external": true` beállítás arról értesíti az hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="bec2d-424">hello `"external": true` setting informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

<span data-ttu-id="bec2d-425">**Azure Data Lake Store kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="bec2d-425">**Azure Data Lake Store output dataset**</span></span>

<span data-ttu-id="bec2d-426">a következő példa másolatok tooData Lake adattárban hello.</span><span class="sxs-lookup"><span data-stu-id="bec2d-426">hello following example copies data tooData Lake Store.</span></span> <span data-ttu-id="bec2d-427">Új adatok másolásakor tooData Lake Store óránként.</span><span class="sxs-lookup"><span data-stu-id="bec2d-427">New data is copied tooData Lake Store every hour.</span></span>

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


<span data-ttu-id="bec2d-428">**A folyamat egy blob-forrás és a Data Lake Store fogadó másolási tevékenység**</span><span class="sxs-lookup"><span data-stu-id="bec2d-428">**Copy activity in a pipeline with a blob source and a Data Lake Store sink**</span></span>

<span data-ttu-id="bec2d-429">A következő példa hello, hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse hello bemeneti és kimeneti adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="bec2d-429">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="bec2d-430">hello másolási tevékenység az ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="bec2d-430">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="bec2d-431">Hello adatcsatorna JSON-definícióból, hello `source` típusuk értéke túl`BlobSource`, és hello `sink` típusuk értéke túl`AzureDataLakeStoreSink`.</span><span class="sxs-lookup"><span data-stu-id="bec2d-431">In hello pipeline JSON definition, hello `source` type is set too`BlobSource`, and hello `sink` type is set too`AzureDataLakeStoreSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
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

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a><span data-ttu-id="bec2d-432">Példa: Adatok másolása az Azure Data Lake Store tooan Azure blob</span><span class="sxs-lookup"><span data-stu-id="bec2d-432">Example: Copy data from Azure Data Lake Store tooan Azure blob</span></span>
<span data-ttu-id="bec2d-433">Ebben a szakaszban hello példakód mutatja:</span><span class="sxs-lookup"><span data-stu-id="bec2d-433">hello example code in this section shows:</span></span>

* <span data-ttu-id="bec2d-434">A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-434">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="bec2d-435">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-435">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="bec2d-436">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-436">An input [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="bec2d-437">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-437">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="bec2d-438">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [AzureDataLakeStoreSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="bec2d-438">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [AzureDataLakeStoreSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="bec2d-439">hello kód átmásolja-idősoros adatok Data Lake Store tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="bec2d-439">hello code copies time-series data from Data Lake Store tooan Azure blob every hour.</span></span> 

<span data-ttu-id="bec2d-440">**Azure Data Lake Store társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="bec2d-440">**Azure Data Lake Store linked service**</span></span>

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="bec2d-441">További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec2d-441">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="bec2d-442">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="bec2d-442">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="bec2d-443">**Azure Data Lake bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="bec2d-443">**Azure Data Lake input dataset**</span></span>

<span data-ttu-id="bec2d-444">Ebben a példában, beállítása `"external"` túl`true` hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák arról tájékoztatja a hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="bec2d-444">In this example, setting `"external"` too`true` informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
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
        "external": true,
        "availability": {
            "frequency": "Hour",
              "interval": 1
        },
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
<span data-ttu-id="bec2d-445">**Azure blobkimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="bec2d-445">**Azure blob output dataset**</span></span>

<span data-ttu-id="bec2d-446">A következő példa hello, adatot ír tooa új blob minden órában (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="bec2d-446">In hello following example, data is written tooa new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="bec2d-447">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="bec2d-447">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="bec2d-448">hello mappa elérési útja hello év, hónap, nap és hello kezdő időpontja óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="bec2d-448">hello folder path uses hello year, month, day, and hours portion of hello start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="bec2d-449">**A másolási tevékenység során az Azure Data Lake Store és egy blob fogadó egy folyamaton belül**</span><span class="sxs-lookup"><span data-stu-id="bec2d-449">**A copy activity in a pipeline with an Azure Data Lake Store source and a blob sink**</span></span>

<span data-ttu-id="bec2d-450">A következő példa hello, hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse hello bemeneti és kimeneti adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="bec2d-450">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="bec2d-451">hello másolási tevékenység az ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="bec2d-451">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="bec2d-452">Hello adatcsatorna JSON-definícióból, hello `source` típusuk értéke túl`AzureDataLakeStoreSource`, és hello `sink` típusuk értéke túl`BlobSink`.</span><span class="sxs-lookup"><span data-stu-id="bec2d-452">In hello pipeline JSON definition, hello `source` type is set too`AzureDataLakeStoreSource`, and hello `sink` type is set too`BlobSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

<span data-ttu-id="bec2d-453">Hello másolási tevékenység definíciójának hello forrás adatkészlet toocolumns hello fogadó adatkészlet oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="bec2d-453">In hello copy activity definition, you can also map columns from hello source dataset toocolumns in hello sink dataset.</span></span> <span data-ttu-id="bec2d-454">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="bec2d-454">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="bec2d-455">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="bec2d-455">Performance and tuning</span></span>
<span data-ttu-id="bec2d-456">Másolási tevékenység teljesítményét befolyásoló tényezők hello kapcsolatos toolearn és hogyan toooptimize, lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bec2d-456">toolearn about hello factors that affect Copy Activity performance and how toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) article.</span></span>
