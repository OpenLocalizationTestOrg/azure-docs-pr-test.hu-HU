---
title: "Másolja az adatokat, és az Azure Data Lake Store |} Microsoft Docs"
description: "Ismerje meg az adatok másolása és Data Lake Store az Azure Data Factory használatával"
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
ms.openlocfilehash: 11629fbc83f0554e2097eb4322701654c0bc2028
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="copy-data-to-and-from-data-lake-store-by-using-data-factory"></a><span data-ttu-id="7b93e-103">Adatok másolása, illetve onnan Data Lake Store Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="7b93e-103">Copy data to and from Data Lake Store by using Data Factory</span></span>
<span data-ttu-id="7b93e-104">Ez a cikk ismerteti, hogyan másolási tevékenység az Azure Data Factory és az Azure Data Lake Store áthelyezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7b93e-104">This article explains how to use Copy Activity in Azure Data Factory to move data to and from Azure Data Lake Store.</span></span> <span data-ttu-id="7b93e-105">Az épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során adatátvitel áttekintését.</span><span class="sxs-lookup"><span data-stu-id="7b93e-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, an overview of data movement with Copy Activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="7b93e-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="7b93e-106">Supported scenarios</span></span>
<span data-ttu-id="7b93e-107">Adatokat másolhat **az Azure Data Lake Store** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="7b93e-107">You can copy data **from Azure Data Lake Store** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="7b93e-108">Adatok másolása a következő adatokat tárolja **Azure Data Lake store**:</span><span class="sxs-lookup"><span data-stu-id="7b93e-108">You can copy data from the following data stores **to Azure Data Lake Store**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="7b93e-109">Data Lake Store-fiók létrehozása a másolási tevékenység során a folyamat létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="7b93e-109">Create a Data Lake Store account before creating a pipeline with Copy Activity.</span></span> <span data-ttu-id="7b93e-110">További információkért lásd: [Ismerkedés az Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7b93e-110">For more information, see [Get started with Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="7b93e-111">Támogatott hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="7b93e-111">Supported authentication types</span></span>
<span data-ttu-id="7b93e-112">A Data Lake Store-összekötő e hitelesítési típusokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="7b93e-112">The Data Lake Store connector supports these authentication types:</span></span>
* <span data-ttu-id="7b93e-113">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="7b93e-113">Service principal authentication</span></span>
* <span data-ttu-id="7b93e-114">Felhasználói hitelesítő adatok (OAuth) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7b93e-114">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="7b93e-115">Azt javasoljuk, hogy a szolgáltatás egyszerű hitelesítést, különösen egy ütemezett adatok másolását a használja.</span><span class="sxs-lookup"><span data-stu-id="7b93e-115">We recommend that you use service principal authentication, especially for a scheduled data copy.</span></span> <span data-ttu-id="7b93e-116">Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="7b93e-116">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="7b93e-117">További konfigurációs információkért lásd: a [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-117">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>

## <a name="get-started"></a><span data-ttu-id="7b93e-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7b93e-118">Get started</span></span>
<span data-ttu-id="7b93e-119">A másolási tevékenység, amely helyezi át az adatokat az Azure Data Lake Store vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="7b93e-119">You can create a pipeline with a copy activity that moves data to/from an Azure Data Lake Store by using different tools/APIs.</span></span>

<span data-ttu-id="7b93e-120">Adatok másolása folyamatokat létrehozni legegyszerűbb módja a használja a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-120">The easiest way to create a pipeline to copy data is to use the **Copy Wizard**.</span></span> <span data-ttu-id="7b93e-121">Folyamat létrehozása a varázsló segítségével oktatóanyag, lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="7b93e-121">For a tutorial on creating a pipeline by using the Copy Wizard, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="7b93e-122">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7b93e-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="7b93e-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="7b93e-124">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="7b93e-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="7b93e-125">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-125">Create a **data factory**.</span></span> <span data-ttu-id="7b93e-126">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="7b93e-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="7b93e-127">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="7b93e-127">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="7b93e-128">Például ha a másolt adatok az az Azure blob storage egy Azure Data Lake Store, akkor két társított szolgáltatások létrehozásához az Azure storage-fiókok és az Azure Data Lake store összekapcsolása a data factory.</span><span class="sxs-lookup"><span data-stu-id="7b93e-128">For example, if you are copying data from an Azure blob storage to an Azure Data Lake Store, you create two linked services to link your Azure storage account and Azure Data Lake store to your data factory.</span></span> <span data-ttu-id="7b93e-129">Azure Data Lake Store jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-129">For linked service properties that are specific to Azure Data Lake Store, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="7b93e-130">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="7b93e-130">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="7b93e-131">A példában az előző lépésben említett hozzon létre egy adatkészlet adja meg a blob-tároló és a bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="7b93e-131">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="7b93e-132">És hoz létre egy másik DataSet adatkészletben, hogy a mappához, majd a fájl elérési útját adja meg a Data Lake-adattár, amely tárolja az adatokat a blob-tároló átmásolva.</span><span class="sxs-lookup"><span data-stu-id="7b93e-132">And, you create another dataset to specify the folder and file path in the Data Lake store that holds the data copied from the blob storage.</span></span> <span data-ttu-id="7b93e-133">Azure Data Lake Store adott adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-133">For dataset properties that are specific to Azure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="7b93e-134">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="7b93e-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="7b93e-135">A korábban említett példában BlobSource forrás-és AzureDataLakeStoreSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="7b93e-135">In the example mentioned earlier, you use BlobSource as a source and AzureDataLakeStoreSink as a sink for the copy activity.</span></span> <span data-ttu-id="7b93e-136">Ehhez hasonlóan az Azure Data Lake Store az Azure Blob Storage másolása, használható AzureDataLakeStoreSource és BlobSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7b93e-136">Similarly, if you are copying from Azure Data Lake Store to Azure Blob Storage, you use AzureDataLakeStoreSource  and BlobSink in the copy activity.</span></span> <span data-ttu-id="7b93e-137">Azure Data Lake Store adott tevékenység Tulajdonságok másolása, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-137">For copy activity properties that are specific to Azure Data Lake Store, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="7b93e-138">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7b93e-138">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>  

<span data-ttu-id="7b93e-139">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="7b93e-139">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="7b93e-140">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="7b93e-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="7b93e-141">A mintában használt adatok másolása az Azure Data Lake Store az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-data-lake-store) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="7b93e-141">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure Data Lake Store, see [JSON examples](#json-examples-for-copying-data-to-and-from-data-lake-store) section of this article.</span></span>

<span data-ttu-id="7b93e-142">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory entitások adott Data Lake Store JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="7b93e-142">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Data Lake Store.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="7b93e-143">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="7b93e-143">Linked service properties</span></span>
<span data-ttu-id="7b93e-144">A társított szolgáltatás adattárat egy adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-144">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="7b93e-145">Típusú társított szolgáltatás létrehozása **AzureDataLakeStore** a Data Lake Store-adatok összekapcsolása a data factory.</span><span class="sxs-lookup"><span data-stu-id="7b93e-145">You create a linked service of type **AzureDataLakeStore** to link your Data Lake Store data to your data factory.</span></span> <span data-ttu-id="7b93e-146">Az alábbi táblázat a Data Lake Store kapcsolódó szolgáltatások vonatkozó JSON-elemeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7b93e-146">The following table describes JSON elements specific to Data Lake Store linked services.</span></span> <span data-ttu-id="7b93e-147">Egyszerű szolgáltatásnév és a felhasználói hitelesítő adatok hitelesítése közül választhat.</span><span class="sxs-lookup"><span data-stu-id="7b93e-147">You can choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="7b93e-148">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-148">Property</span></span> | <span data-ttu-id="7b93e-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-149">Description</span></span> | <span data-ttu-id="7b93e-150">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-150">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7b93e-151">**típusa**</span><span class="sxs-lookup"><span data-stu-id="7b93e-151">**type**</span></span> | <span data-ttu-id="7b93e-152">A type tulajdonságot meg kell **AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-152">The type property must be set to **AzureDataLakeStore**.</span></span> | <span data-ttu-id="7b93e-153">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-153">Yes</span></span> |
| <span data-ttu-id="7b93e-154">**dataLakeStoreUri**</span><span class="sxs-lookup"><span data-stu-id="7b93e-154">**dataLakeStoreUri**</span></span> | <span data-ttu-id="7b93e-155">Az Azure Data Lake Store-fiókja adatait.</span><span class="sxs-lookup"><span data-stu-id="7b93e-155">Information about the Azure Data Lake Store account.</span></span> <span data-ttu-id="7b93e-156">Ez az információ időt vesz igénybe, a következő formátumok egyikének: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="7b93e-156">This information takes one of the following formats: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="7b93e-157">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-157">Yes</span></span> |
| <span data-ttu-id="7b93e-158">**előfizetés-azonosító**</span><span class="sxs-lookup"><span data-stu-id="7b93e-158">**subscriptionId**</span></span> | <span data-ttu-id="7b93e-159">Azure-előfizetése Azonosítóját, amelyhez a Data Lake Store-fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-159">Azure subscription ID to which the Data Lake Store account belongs.</span></span> | <span data-ttu-id="7b93e-160">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-160">Required for sink</span></span> |
| <span data-ttu-id="7b93e-161">**erőforráscsoport-név**</span><span class="sxs-lookup"><span data-stu-id="7b93e-161">**resourceGroupName**</span></span> | <span data-ttu-id="7b93e-162">Azure erőforráscsoport-név, amely a Data Lake Store-fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-162">Azure resource group name to which the Data Lake Store account belongs.</span></span> | <span data-ttu-id="7b93e-163">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-163">Required for sink</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="7b93e-164">Szolgáltatás egyszerű hitelesítés (ajánlott)</span><span class="sxs-lookup"><span data-stu-id="7b93e-164">Service principal authentication (recommended)</span></span>
<span data-ttu-id="7b93e-165">Szolgáltatás egyszerű hitelesítést használ, egy alkalmazás entitás regisztrálni kell az Azure Active Directory (Azure AD), és hozzáférést, a Data Lake Store-bA.</span><span class="sxs-lookup"><span data-stu-id="7b93e-165">To use service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it the access to Data Lake Store.</span></span> <span data-ttu-id="7b93e-166">Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="7b93e-166">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="7b93e-167">Jegyezze fel a következő érték, melynek segítségével határozza meg a társított szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="7b93e-167">Make note of the following values, which you use to define the linked service:</span></span>
* <span data-ttu-id="7b93e-168">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="7b93e-168">Application ID</span></span>
* <span data-ttu-id="7b93e-169">Alkalmazás kulcs</span><span class="sxs-lookup"><span data-stu-id="7b93e-169">Application key</span></span> 
* <span data-ttu-id="7b93e-170">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="7b93e-170">Tenant ID</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b93e-171">Ha másolása varázsló segítségével adatokat folyamatok hozhatnak létre, győződjön meg arról, hogy megadta-e az egyszerű szolgáltatás legalább egy **olvasó** szerepkör a hozzáférés-vezérlés (identitások és hozzáférések felügyeletéhez) a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="7b93e-171">If you are using the Copy Wizard to author data pipelines, make sure that you grant the service principal at least a **Reader** role in access control (identity and access management) for the Data Lake Store account.</span></span> <span data-ttu-id="7b93e-172">Is, adja meg a szolgáltatás egyszerű legalább **olvasási + Execute** engedéllyel a Data Lake Store gyökér ("/") és gyermekét.</span><span class="sxs-lookup"><span data-stu-id="7b93e-172">Also, grant the service principal at least **Read + Execute** permission to your Data Lake Store root ("/") and its children.</span></span> <span data-ttu-id="7b93e-173">Ellenkező esetben előfordulhat, hogy megjelenik az üzenet "a megadott hitelesítő adatok érvénytelenek."</span><span class="sxs-lookup"><span data-stu-id="7b93e-173">Otherwise you might see the message "The credentials provided are invalid."</span></span><br/><br/>
<span data-ttu-id="7b93e-174">Létrehozása vagy frissítése egy egyszerű szolgáltatást az Azure ad-ben, után is igénybe vehet néhány percet, amíg a módosítások életbe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="7b93e-174">After you create or update a service principal in Azure AD, it can take a few minutes for the changes to take effect.</span></span> <span data-ttu-id="7b93e-175">Ellenőrizze az egyszerű szolgáltatásnév és a Data Lake Store access control lista (ACL) konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-175">Check the service principal and Data Lake Store access control list (ACL) configurations.</span></span> <span data-ttu-id="7b93e-176">Ha még mindig tekintse meg az üzenet "a megadott hitelesítő adatok érvénytelenek," Várjon egy kicsit, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="7b93e-176">If you still see the message "The credentials provided are invalid," wait a while and try again.</span></span>

<span data-ttu-id="7b93e-177">Szolgáltatás egyszerű hitelesítés használatára a következő tulajdonságok megadásával:</span><span class="sxs-lookup"><span data-stu-id="7b93e-177">Use service principal authentication by specifying the following properties:</span></span>

| <span data-ttu-id="7b93e-178">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-178">Property</span></span> | <span data-ttu-id="7b93e-179">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-179">Description</span></span> | <span data-ttu-id="7b93e-180">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-180">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7b93e-181">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="7b93e-181">**servicePrincipalId**</span></span> | <span data-ttu-id="7b93e-182">Adja meg az alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="7b93e-182">Specify the application's client ID.</span></span> | <span data-ttu-id="7b93e-183">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-183">Yes</span></span> |
| <span data-ttu-id="7b93e-184">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="7b93e-184">**servicePrincipalKey**</span></span> | <span data-ttu-id="7b93e-185">Adja meg az alkalmazás kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7b93e-185">Specify the application's key.</span></span> | <span data-ttu-id="7b93e-186">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-186">Yes</span></span> |
| <span data-ttu-id="7b93e-187">**Bérlői**</span><span class="sxs-lookup"><span data-stu-id="7b93e-187">**tenant**</span></span> | <span data-ttu-id="7b93e-188">Adja meg a bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="7b93e-188">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="7b93e-189">Azt az Azure-portál jobb felső sarkában az egér rámutató által kérheti le.</span><span class="sxs-lookup"><span data-stu-id="7b93e-189">You can retrieve it by hovering the mouse in the upper-right corner of the Azure portal.</span></span> | <span data-ttu-id="7b93e-190">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-190">Yes</span></span> |

<span data-ttu-id="7b93e-191">**Példa: Szolgáltatás egyszerű hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="7b93e-191">**Example: Service principal authentication**</span></span>
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

### <a name="user-credential-authentication"></a><span data-ttu-id="7b93e-192">Felhasználói hitelesítő adatok hitelesítése</span><span class="sxs-lookup"><span data-stu-id="7b93e-192">User credential authentication</span></span>
<span data-ttu-id="7b93e-193">Felhasználói hitelesítő adatok hitelesítése segítségével azt is megteheti, másolja a következő tulajdonságok megadásával vagy Data Lake Store-bA:</span><span class="sxs-lookup"><span data-stu-id="7b93e-193">Alternatively, you can use user credential authentication to copy from or to Data Lake Store by specifying the following properties:</span></span>

| <span data-ttu-id="7b93e-194">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-194">Property</span></span> | <span data-ttu-id="7b93e-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-195">Description</span></span> | <span data-ttu-id="7b93e-196">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-196">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7b93e-197">**engedélyezési**</span><span class="sxs-lookup"><span data-stu-id="7b93e-197">**authorization**</span></span> | <span data-ttu-id="7b93e-198">Kattintson a **engedélyezés** a Data Factory Editor gombra, és adja meg a hitelesítő adatok, amelyek az automatikusan létrehozott engedélyezési URL-címet rendel hozzá ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-198">Click the **Authorize** button in the Data Factory Editor and enter your credential that assigns the autogenerated authorization URL to this property.</span></span> | <span data-ttu-id="7b93e-199">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-199">Yes</span></span> |
| <span data-ttu-id="7b93e-200">**munkamenet-azonosító**</span><span class="sxs-lookup"><span data-stu-id="7b93e-200">**sessionId**</span></span> | <span data-ttu-id="7b93e-201">OAuth munkamenet-azonosító az OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="7b93e-201">OAuth session ID from the OAuth authorization session.</span></span> <span data-ttu-id="7b93e-202">Minden munkamenet-azonosító egyedi, és csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="7b93e-202">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="7b93e-203">Ez a beállítás automatikusan jön létre, a Data Factory Editor használatakor.</span><span class="sxs-lookup"><span data-stu-id="7b93e-203">This setting is automatically generated when you use the Data Factory Editor.</span></span> | <span data-ttu-id="7b93e-204">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-204">Yes</span></span> |

<span data-ttu-id="7b93e-205">**Példa: Felhasználók hitelesítő adatok hitelesítése**</span><span class="sxs-lookup"><span data-stu-id="7b93e-205">**Example: User credential authentication**</span></span>
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

#### <a name="token-expiration"></a><span data-ttu-id="7b93e-206">Jogkivonat lejáratáról</span><span class="sxs-lookup"><span data-stu-id="7b93e-206">Token expiration</span></span>
<span data-ttu-id="7b93e-207">Az engedélyezési kód használatával generáló a **engedélyezés** gombra egy bizonyos idő elteltével lejár.</span><span class="sxs-lookup"><span data-stu-id="7b93e-207">The authorization code that you generate by using the **Authorize** button expires after a certain amount of time.</span></span> <span data-ttu-id="7b93e-208">A következő üzenet azt jelenti, hogy a hitelesítési jogkivonat lejárt:</span><span class="sxs-lookup"><span data-stu-id="7b93e-208">The following message means that the authentication token has expired:</span></span>

<span data-ttu-id="7b93e-209">Hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7b93e-209">Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="7b93e-210">AADSTS70008: A megadott hozzáférés biztosítása lejárt vagy visszavonták.</span><span class="sxs-lookup"><span data-stu-id="7b93e-210">AADSTS70008: The provided access grant is expired or revoked.</span></span> <span data-ttu-id="7b93e-211">Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21-09-31Z.</span><span class="sxs-lookup"><span data-stu-id="7b93e-211">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.</span></span>

<span data-ttu-id="7b93e-212">Az alábbi táblázat a különböző típusú felhasználói fiókok lejárati időpontja:</span><span class="sxs-lookup"><span data-stu-id="7b93e-212">The following table shows the expiration times of different types of user accounts:</span></span>


| <span data-ttu-id="7b93e-213">Felhasználó típusa</span><span class="sxs-lookup"><span data-stu-id="7b93e-213">User type</span></span> | <span data-ttu-id="7b93e-214">Után lejár</span><span class="sxs-lookup"><span data-stu-id="7b93e-214">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="7b93e-215">Felhasználói fiókok *nem* kezeli az Azure Active Directoryban (például @hotmail.com vagy @live.com)</span><span class="sxs-lookup"><span data-stu-id="7b93e-215">User accounts *not* managed by Azure Active Directory (for example, @hotmail.com or @live.com)</span></span> |<span data-ttu-id="7b93e-216">12 óra</span><span class="sxs-lookup"><span data-stu-id="7b93e-216">12 hours</span></span> |
| <span data-ttu-id="7b93e-217">Felhasználói fiókok Azure Active Directory által felügyelt</span><span class="sxs-lookup"><span data-stu-id="7b93e-217">Users accounts managed by Azure Active Directory</span></span> |<span data-ttu-id="7b93e-218">14 napja után utolsó újrapróbálása futtatása</span><span class="sxs-lookup"><span data-stu-id="7b93e-218">14 days after the last slice run</span></span> <br/><br/><span data-ttu-id="7b93e-219">90 nap, ha egy olyan OAuth-alapú társított szolgáltatás szelet 14 naponta legalább egyszer fut</span><span class="sxs-lookup"><span data-stu-id="7b93e-219">90 days, if a slice based on an OAuth-based linked service runs at least once every 14 days</span></span> |

<span data-ttu-id="7b93e-220">Ha a jogkivonat lejárati idő előtt módosítsa jelszavát, a jogkivonat azonnal lejár.</span><span class="sxs-lookup"><span data-stu-id="7b93e-220">If you change your password before the token expiration time, the token expires immediately.</span></span> <span data-ttu-id="7b93e-221">Az ebben a szakaszban korábban említett üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7b93e-221">You will see the message mentioned earlier in this section.</span></span>

<span data-ttu-id="7b93e-222">Segítségével tudja ismét engedélyezheti a fiók a **engedélyezés** gombra kattint, amikor a jogkivonat lejár, a társított szolgáltatás újratelepíteni.</span><span class="sxs-lookup"><span data-stu-id="7b93e-222">You can reauthorize the account by using the **Authorize** button when the token expires to redeploy the linked service.</span></span> <span data-ttu-id="7b93e-223">Értékek is létrehozhat a **sessionId** és **engedélyezési** tulajdonságok programozott módon az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="7b93e-223">You can also generate values for the **sessionId** and **authorization** properties programmatically by using the following code:</span></span>


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
<span data-ttu-id="7b93e-224">A Data Factory osztályok, a kódban használt kapcsolatos részletekért lásd: a [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [ AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="7b93e-224">For details about the Data Factory classes used in the code, see the [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics.</span></span> <span data-ttu-id="7b93e-225">Adjon hozzá egy hivatkozást verzióra `2.9.10826.1824` a `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` a a `WindowsFormsWebAuthenticationDialog` a kódban használt osztály.</span><span class="sxs-lookup"><span data-stu-id="7b93e-225">Add a reference to version `2.9.10826.1824` of `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` for the `WindowsFormsWebAuthenticationDialog` class used in the code.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="7b93e-226">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7b93e-226">Dataset properties</span></span>
<span data-ttu-id="7b93e-227">Szeretné megadni a bemeneti adatok egy Data Lake Store a dataset, állítsa be a **típus** a DataSet tulajdonság **AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-227">To specify a dataset to represent input data in a Data Lake Store, you set the **type** property of the dataset to **AzureDataLakeStore**.</span></span> <span data-ttu-id="7b93e-228">Állítsa be a **linkedServiceName** a Data Lake Store nevét a DataSet tulajdonság társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7b93e-228">Set the **linkedServiceName** property of the dataset to the name of the Data Lake Store linked service.</span></span> <span data-ttu-id="7b93e-229">JSON-szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-229">For a full list of JSON sections and properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7b93e-230">A JSON-ban, a DataSet adatkészlet szakaszok például **struktúra**, **rendelkezésre állási**, és **házirend**, minden adatkészlet esetében hasonló (Azure SQL-adatbázis, az Azure blob és az Azure tábla a Példa).</span><span class="sxs-lookup"><span data-stu-id="7b93e-230">Sections of a dataset in JSON, such as **structure**, **availability**, and **policy**, are similar for all dataset types (Azure SQL database, Azure blob, and Azure table, for example).</span></span> <span data-ttu-id="7b93e-231">A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú, és megjelenik többek között a hely és az adattár adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="7b93e-231">The **typeProperties** section is different for each type of dataset and provides information such as location and format of the data in the data store.</span></span> 

<span data-ttu-id="7b93e-232">A **typeProperties** szakasz egy adatkészlet típusú **AzureDataLakeStore** tartalmazza a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="7b93e-232">The **typeProperties** section for a dataset of type **AzureDataLakeStore** contains the following properties:</span></span>

| <span data-ttu-id="7b93e-233">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-233">Property</span></span> | <span data-ttu-id="7b93e-234">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-234">Description</span></span> | <span data-ttu-id="7b93e-235">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-235">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7b93e-236">**folderPath**</span><span class="sxs-lookup"><span data-stu-id="7b93e-236">**folderPath**</span></span> |<span data-ttu-id="7b93e-237">A tároló és a Data Lake Store mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="7b93e-237">Path to the container and folder in Data Lake Store.</span></span> |<span data-ttu-id="7b93e-238">Igen</span><span class="sxs-lookup"><span data-stu-id="7b93e-238">Yes</span></span> |
| <span data-ttu-id="7b93e-239">**Fájlnév**</span><span class="sxs-lookup"><span data-stu-id="7b93e-239">**fileName**</span></span> |<span data-ttu-id="7b93e-240">A fájl az Azure Data Lake Store nevét.</span><span class="sxs-lookup"><span data-stu-id="7b93e-240">Name of the file in Azure Data Lake Store.</span></span> <span data-ttu-id="7b93e-241">A **Fájlnév** tulajdonság nem kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="7b93e-241">The **fileName** property is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="7b93e-242">Ha megad **Fájlnév**, a tevékenység (például a Másolás) működik-e az adott fájlt.</span><span class="sxs-lookup"><span data-stu-id="7b93e-242">If you specify **fileName**, the activity (including Copy) works on the specific file.</span></span><br/><br/><span data-ttu-id="7b93e-243">Ha **Fájlnév** nincs megadva, másolása tartalmazza az összes fájl **folderPath** bemeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="7b93e-243">When **fileName** is not specified, Copy includes all files in **folderPath** in the input dataset.</span></span><br/><br/><span data-ttu-id="7b93e-244">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó, a létrehozott fájl neve nem formátumú adatok. _GUID_.txt ".</span><span class="sxs-lookup"><span data-stu-id="7b93e-244">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file is in the format Data._Guid_.txt\`.</span></span> <span data-ttu-id="7b93e-245">Például: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span><span class="sxs-lookup"><span data-stu-id="7b93e-245">For example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span></span> |<span data-ttu-id="7b93e-246">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-246">No</span></span> |
| <span data-ttu-id="7b93e-247">**partitionedBy**</span><span class="sxs-lookup"><span data-stu-id="7b93e-247">**partitionedBy**</span></span> |<span data-ttu-id="7b93e-248">A **partitionedBy** tulajdonság nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="7b93e-248">The **partitionedBy** property is optional.</span></span> <span data-ttu-id="7b93e-249">Használhatja a dinamikus elérési út és fájlnév idősorozat adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="7b93e-249">You can use it to specify a dynamic path and file name for time-series data.</span></span> <span data-ttu-id="7b93e-250">Például **folderPath** adatok órára is lehet paraméterezett.</span><span class="sxs-lookup"><span data-stu-id="7b93e-250">For example, **folderPath** can be parameterized for every hour of data.</span></span> <span data-ttu-id="7b93e-251">Részletek és példák: [partitionedBy tulajdonság](#using-partitionedby-property).</span><span class="sxs-lookup"><span data-stu-id="7b93e-251">For details and examples, see [The partitionedBy property](#using-partitionedby-property).</span></span> |<span data-ttu-id="7b93e-252">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-252">No</span></span> |
| <span data-ttu-id="7b93e-253">**formátumban**</span><span class="sxs-lookup"><span data-stu-id="7b93e-253">**format**</span></span> | <span data-ttu-id="7b93e-254">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, és  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-254">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, and **ParquetFormat**.</span></span> <span data-ttu-id="7b93e-255">Állítsa be a **típus** tulajdonság alapján **formátum** számára a következő értékek egyike.</span><span class="sxs-lookup"><span data-stu-id="7b93e-255">Set the **type** property under **format** to one of these values.</span></span> <span data-ttu-id="7b93e-256">További információkért lásd: a [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszában a [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-256">For more information, see the [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections in the [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span> <br><br> <span data-ttu-id="7b93e-257">Ha a másolandó fájlok ",-van" közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a `format` mindkét bemeneti és kimeneti adatkészlet-definíciókban szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-257">If you want to copy files "as-is" between file-based stores (binary copy), skip the `format` section in both input and output dataset definitions.</span></span> |<span data-ttu-id="7b93e-258">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-258">No</span></span> |
| <span data-ttu-id="7b93e-259">**tömörítés**</span><span class="sxs-lookup"><span data-stu-id="7b93e-259">**compression**</span></span> | <span data-ttu-id="7b93e-260">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="7b93e-260">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="7b93e-261">Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-261">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="7b93e-262">Támogatott szintek a következők **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="7b93e-262">Supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="7b93e-263">További információkért lásd: [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="7b93e-263">For more information, see [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="7b93e-264">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-264">No</span></span> |

### <a name="the-partitionedby-property"></a><span data-ttu-id="7b93e-265">A partitionedBy tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-265">The partitionedBy property</span></span>
<span data-ttu-id="7b93e-266">Megadhatja, hogy a dinamikus **folderPath** és **Fájlnév** tulajdonságok idősorozat adatokhoz a **partitionedBy** tulajdonság, adat-előállító funkciók és rendszerváltozók.</span><span class="sxs-lookup"><span data-stu-id="7b93e-266">You can specify dynamic **folderPath** and **fileName** properties for time-series data with the **partitionedBy** property, Data Factory functions, and system variables.</span></span> <span data-ttu-id="7b93e-267">További információkért lásd: a [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-267">For details, see the [Azure Data Factory - functions and system variables](data-factory-functions-variables.md) article.</span></span>


<span data-ttu-id="7b93e-268">A következő példában `{Slice}` cseréli a Data Factory rendszer változó értékének `SliceStart` megadott formátumban (`yyyyMMddHH`).</span><span class="sxs-lookup"><span data-stu-id="7b93e-268">In the following example, `{Slice}` is replaced with the value of the Data Factory system variable `SliceStart` in the format specified (`yyyyMMddHH`).</span></span> <span data-ttu-id="7b93e-269">A név `SliceStart` hivatkozik a szelet kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="7b93e-269">The name `SliceStart` refers to the start time of the slice.</span></span> <span data-ttu-id="7b93e-270">A `folderPath` tulajdonság nem az egyes szeletek, mint az azonos `wikidatagateway/wikisampledataout/2014100103` vagy `wikidatagateway/wikisampledataout/2014100104`.</span><span class="sxs-lookup"><span data-stu-id="7b93e-270">The `folderPath` property is different for each slice, as in `wikidatagateway/wikisampledataout/2014100103` or `wikidatagateway/wikisampledataout/2014100104`.</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="7b93e-271">Az alábbi példa, év, hónap, nap, és az idő a `SliceStart` ki kell olvasni a által használt külön változók a `folderPath` és `fileName` tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="7b93e-271">In the following example, the year, month, day, and time of `SliceStart` are extracted into separate variables that are used by the `folderPath` and `fileName` properties:</span></span>
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
<span data-ttu-id="7b93e-272">Az idősorozat adatkészleteket, az ütemezés és a szeletek további részletekért lásd: a [Azure Data Factory adathalmazok](data-factory-create-datasets.md) és [adat-előállító ütemezés és a végrehajtási](data-factory-scheduling-and-execution.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="7b93e-272">For more details on time-series datasets, scheduling, and slices, see the [Datasets in Azure Data Factory](data-factory-create-datasets.md) and [Data Factory scheduling and execution](data-factory-scheduling-and-execution.md) articles.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="7b93e-273">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7b93e-273">Copy activity properties</span></span>
<span data-ttu-id="7b93e-274">Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [folyamatok létrehozása](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-274">For a full list of sections and properties available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7b93e-275">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7b93e-275">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="7b93e-276">A tulajdonságok érhetők el a **typeProperties** tevékenység szakasz tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="7b93e-276">The properties available in the **typeProperties** section of an activity vary with each activity type.</span></span> <span data-ttu-id="7b93e-277">A másolási tevékenységhez változik a források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="7b93e-277">For a copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="7b93e-278">**AzureDataLakeStoreSource** támogatja a következő tulajdonságot a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="7b93e-278">**AzureDataLakeStoreSource** supports the following property in the **typeProperties** section:</span></span>

| <span data-ttu-id="7b93e-279">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-279">Property</span></span> | <span data-ttu-id="7b93e-280">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-280">Description</span></span> | <span data-ttu-id="7b93e-281">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="7b93e-281">Allowed values</span></span> | <span data-ttu-id="7b93e-282">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-282">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7b93e-283">**rekurzív**</span><span class="sxs-lookup"><span data-stu-id="7b93e-283">**recursive**</span></span> |<span data-ttu-id="7b93e-284">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappákat, illetve csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="7b93e-284">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="7b93e-285">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="7b93e-285">True (default value), False</span></span> |<span data-ttu-id="7b93e-286">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-286">No</span></span> |


<span data-ttu-id="7b93e-287">**AzureDataLakeStoreSink** támogatja a következő tulajdonságok a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="7b93e-287">**AzureDataLakeStoreSink** supports the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="7b93e-288">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b93e-288">Property</span></span> | <span data-ttu-id="7b93e-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b93e-289">Description</span></span> | <span data-ttu-id="7b93e-290">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="7b93e-290">Allowed values</span></span> | <span data-ttu-id="7b93e-291">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7b93e-291">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7b93e-292">**copyBehavior**</span><span class="sxs-lookup"><span data-stu-id="7b93e-292">**copyBehavior**</span></span> |<span data-ttu-id="7b93e-293">Meghatározza a másolási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="7b93e-293">Specifies the copy behavior.</span></span> |<span data-ttu-id="7b93e-294"><b>PreserveHierarchy</b>: őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="7b93e-294"><b>PreserveHierarchy</b>: Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="7b93e-295">A következő forrásfájl forrásmappához relatív elérési a relatív elérési út a cél-fájlját és a célmappa megegyezik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-295">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="7b93e-296"><b>FlattenHierarchy</b>: összes fájl a forrásmappából az első szintjét a célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="7b93e-296"><b>FlattenHierarchy</b>: All files from the source folder are created in the first level of the target folder.</span></span> <span data-ttu-id="7b93e-297">A cél fájlok jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="7b93e-297">The target files are created with autogenerated names.</span></span><br/><br/><span data-ttu-id="7b93e-298"><b>Mergefiles típusú</b>: egy fájl összes fájlt a forrásmappából egyesíti.</span><span class="sxs-lookup"><span data-stu-id="7b93e-298"><b>MergeFiles</b>: Merges all files from the source folder to one file.</span></span> <span data-ttu-id="7b93e-299">Ha a fájl vagy a blob neve meg van adva, az egyesített fájlnév a megadott név.</span><span class="sxs-lookup"><span data-stu-id="7b93e-299">If the file or blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="7b93e-300">Ellenkező esetben a fájlnév pedig automatikusan elnevezi.</span><span class="sxs-lookup"><span data-stu-id="7b93e-300">Otherwise, the file name is autogenerated.</span></span> |<span data-ttu-id="7b93e-301">Nem</span><span class="sxs-lookup"><span data-stu-id="7b93e-301">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="7b93e-302">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="7b93e-302">recursive and copyBehavior examples</span></span>
<span data-ttu-id="7b93e-303">Ez a szakasz ismerteti az eredményül kapott viselkedéstől rekurzív és copyBehavior kombinációk a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="7b93e-303">This section describes the resulting behavior of the Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="7b93e-304">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="7b93e-304">recursive</span></span> | <span data-ttu-id="7b93e-305">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="7b93e-305">copyBehavior</span></span> | <span data-ttu-id="7b93e-306">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="7b93e-306">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7b93e-307">Igaz</span><span class="sxs-lookup"><span data-stu-id="7b93e-307">true</span></span> |<span data-ttu-id="7b93e-308">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="7b93e-308">preserveHierarchy</span></span> |<span data-ttu-id="7b93e-309">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-309">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-310">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-310">Folder1</span></span><br/><span data-ttu-id="7b93e-311">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-317">a célmappában mappa1 forrásaként azonos struktúrájú jön létre.</span><span class="sxs-lookup"><span data-stu-id="7b93e-317">the target folder Folder1 is created with the same structure as the source</span></span><br/><br/><span data-ttu-id="7b93e-318">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-318">Folder1</span></span><br/><span data-ttu-id="7b93e-319">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="7b93e-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="7b93e-325">Igaz</span><span class="sxs-lookup"><span data-stu-id="7b93e-325">true</span></span> |<span data-ttu-id="7b93e-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="7b93e-326">flattenHierarchy</span></span> |<span data-ttu-id="7b93e-327">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-327">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-328">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-328">Folder1</span></span><br/><span data-ttu-id="7b93e-329">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-335">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="7b93e-335">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-336">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-336">Folder1</span></span><br/><span data-ttu-id="7b93e-337">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="7b93e-338">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="7b93e-339">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-339">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="7b93e-340">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-340">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="7b93e-341">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-341">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="7b93e-342">Igaz</span><span class="sxs-lookup"><span data-stu-id="7b93e-342">true</span></span> |<span data-ttu-id="7b93e-343">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="7b93e-343">mergeFiles</span></span> |<span data-ttu-id="7b93e-344">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-344">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-345">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-345">Folder1</span></span><br/><span data-ttu-id="7b93e-346">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-352">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="7b93e-352">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-353">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-353">Folder1</span></span><br/><span data-ttu-id="7b93e-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek</span><span class="sxs-lookup"><span data-stu-id="7b93e-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="7b93e-355">hamis</span><span class="sxs-lookup"><span data-stu-id="7b93e-355">false</span></span> |<span data-ttu-id="7b93e-356">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="7b93e-356">preserveHierarchy</span></span> |<span data-ttu-id="7b93e-357">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-357">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="7b93e-358">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-358">Folder1</span></span><br/><span data-ttu-id="7b93e-359">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-365">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="7b93e-365">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="7b93e-366">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-366">Folder1</span></span><br/><span data-ttu-id="7b93e-367">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="7b93e-369">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-369">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="7b93e-370">hamis</span><span class="sxs-lookup"><span data-stu-id="7b93e-370">false</span></span> |<span data-ttu-id="7b93e-371">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="7b93e-371">flattenHierarchy</span></span> |<span data-ttu-id="7b93e-372">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-372">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="7b93e-373">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-373">Folder1</span></span><br/><span data-ttu-id="7b93e-374">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-380">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="7b93e-380">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="7b93e-381">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-381">Folder1</span></span><br/><span data-ttu-id="7b93e-382">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-382">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="7b93e-383">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-383">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="7b93e-384">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-384">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="7b93e-385">hamis</span><span class="sxs-lookup"><span data-stu-id="7b93e-385">false</span></span> |<span data-ttu-id="7b93e-386">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="7b93e-386">mergeFiles</span></span> |<span data-ttu-id="7b93e-387">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="7b93e-387">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="7b93e-388">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-388">Folder1</span></span><br/><span data-ttu-id="7b93e-389">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="7b93e-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="7b93e-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="7b93e-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="7b93e-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="7b93e-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="7b93e-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="7b93e-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="7b93e-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="7b93e-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="7b93e-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="7b93e-395">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="7b93e-395">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="7b93e-396">Mappa1</span><span class="sxs-lookup"><span data-stu-id="7b93e-396">Folder1</span></span><br/><span data-ttu-id="7b93e-397">&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma.</span><span class="sxs-lookup"><span data-stu-id="7b93e-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="7b93e-398">automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="7b93e-398">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="7b93e-399">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-399">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="7b93e-400">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="7b93e-400">Supported file and compression formats</span></span>
<span data-ttu-id="7b93e-401">További információkért lásd: a [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-401">For details, see the [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span>

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a><span data-ttu-id="7b93e-402">Adatok másolása, és a Data Lake Store JSON példák</span><span class="sxs-lookup"><span data-stu-id="7b93e-402">JSON examples for copying data to and from Data Lake Store</span></span>
<span data-ttu-id="7b93e-403">Az alábbi példák megadják minta JSON-definíciók.</span><span class="sxs-lookup"><span data-stu-id="7b93e-403">The following examples provide sample JSON definitions.</span></span> <span data-ttu-id="7b93e-404">Ezek a minta a definíciók segítségével hozzon létre egy folyamatot a [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7b93e-404">You can use these sample definitions to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7b93e-405">A példák bemutatják, hogyan irányuló és onnan Data Lake Store és az Azure Blob storage-adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="7b93e-405">The examples show how to copy data to and from Data Lake Store and Azure Blob storage.</span></span> <span data-ttu-id="7b93e-406">Azonban az adatok átmásolhatók _közvetlenül_ bármelyik bármelyik a támogatott adatforrások fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="7b93e-406">However, data can be copied _directly_ from any of the sources to any of the supported sinks.</span></span> <span data-ttu-id="7b93e-407">További információkért lásd: "támogatott adattárolókhoz és formátumok" szakasz a a [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-407">For more information, see the section "Supported data stores and formats" in the [Move data by using Copy Activity](data-factory-data-movement-activities.md) article.</span></span>  

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a><span data-ttu-id="7b93e-408">Példa: Adatok másolása az Azure Blob Storage az Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7b93e-408">Example: Copy data from Azure Blob Storage to Azure Data Lake Store</span></span>
<span data-ttu-id="7b93e-409">Ez a szakasz a példakód mutatja:</span><span class="sxs-lookup"><span data-stu-id="7b93e-409">The example code in this section shows:</span></span>

* <span data-ttu-id="7b93e-410">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-410">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="7b93e-411">A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-411">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="7b93e-412">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-412">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="7b93e-413">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-413">An output [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="7b93e-414">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [AzureDataLakeStoreSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-414">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureDataLakeStoreSink](#copy-activity-properties).</span></span>

<span data-ttu-id="7b93e-415">A példák azt szemléltetik, hogyan idősorozat adatokat az Azure Blob Storage van Data Lake Store másolt óránként.</span><span class="sxs-lookup"><span data-stu-id="7b93e-415">The examples show how time-series data from Azure Blob Storage is copied to Data Lake Store every hour.</span></span> 

<span data-ttu-id="7b93e-416">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="7b93e-416">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="7b93e-417">**Azure Data Lake Store társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="7b93e-417">**Azure Data Lake Store linked service**</span></span>

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
> <span data-ttu-id="7b93e-418">További konfigurációs információkért lásd: a [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-418">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="7b93e-419">**Azure blobbemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="7b93e-419">**Azure blob input dataset**</span></span>

<span data-ttu-id="7b93e-420">A következő példában adatok van felvett egy új blobból minden órában (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="7b93e-420">In the following example, data is picked up from a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="7b93e-421">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="7b93e-421">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="7b93e-422">A mappa elérési útját használja, év, hónap és nap részéhez kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="7b93e-422">The folder path uses the year, month, and day portion of the start time.</span></span> <span data-ttu-id="7b93e-423">A fájl nevét a kezdő időpontja óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="7b93e-423">The file name uses the hour portion of the start time.</span></span> <span data-ttu-id="7b93e-424">A `"external": true` beállítás arról értesíti az, hogy a tábla az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="7b93e-424">The `"external": true` setting informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="7b93e-425">**Azure Data Lake Store kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="7b93e-425">**Azure Data Lake Store output dataset**</span></span>

<span data-ttu-id="7b93e-426">A következő példa a Data Lake Store adatainak másolása.</span><span class="sxs-lookup"><span data-stu-id="7b93e-426">The following example copies data to Data Lake Store.</span></span> <span data-ttu-id="7b93e-427">Új adatok másolásakor Data Lake Store óránként.</span><span class="sxs-lookup"><span data-stu-id="7b93e-427">New data is copied to Data Lake Store every hour.</span></span>

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


<span data-ttu-id="7b93e-428">**A folyamat egy blob-forrás és a Data Lake Store fogadó másolási tevékenység**</span><span class="sxs-lookup"><span data-stu-id="7b93e-428">**Copy activity in a pipeline with a blob source and a Data Lake Store sink**</span></span>

<span data-ttu-id="7b93e-429">A következő példában a feldolgozási sor tartalmazza a másolási tevékenység során használja a bemeneti és kimeneti adatkészletek van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7b93e-429">In the following example, the pipeline contains a copy activity that is configured to use the input and output datasets.</span></span> <span data-ttu-id="7b93e-430">A másolási tevékenység óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="7b93e-430">The copy activity is scheduled to run every hour.</span></span> <span data-ttu-id="7b93e-431">Az adatcsatorna JSON-definícióból a `source` típusúra `BlobSource`, és a `sink` típusúra `AzureDataLakeStoreSink`.</span><span class="sxs-lookup"><span data-stu-id="7b93e-431">In the pipeline JSON definition, the `source` type is set to `BlobSource`, and the `sink` type is set to `AzureDataLakeStoreSink`.</span></span>

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

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a><span data-ttu-id="7b93e-432">Példa: Adatok másolása az Azure Data Lake Store az Azure-blobba</span><span class="sxs-lookup"><span data-stu-id="7b93e-432">Example: Copy data from Azure Data Lake Store to an Azure blob</span></span>
<span data-ttu-id="7b93e-433">Ez a szakasz a példakód mutatja:</span><span class="sxs-lookup"><span data-stu-id="7b93e-433">The example code in this section shows:</span></span>

* <span data-ttu-id="7b93e-434">A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-434">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="7b93e-435">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-435">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="7b93e-436">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-436">An input [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="7b93e-437">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-437">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="7b93e-438">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [AzureDataLakeStoreSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7b93e-438">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [AzureDataLakeStoreSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7b93e-439">A kód másolja-idősoros adatok Data Lake Store-ból az Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="7b93e-439">The code copies time-series data from Data Lake Store to an Azure blob every hour.</span></span> 

<span data-ttu-id="7b93e-440">**Azure Data Lake Store társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="7b93e-440">**Azure Data Lake Store linked service**</span></span>

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
> <span data-ttu-id="7b93e-441">További konfigurációs információkért lásd: a [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b93e-441">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="7b93e-442">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="7b93e-442">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="7b93e-443">**Azure Data Lake bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="7b93e-443">**Azure Data Lake input dataset**</span></span>

<span data-ttu-id="7b93e-444">Ebben a példában, beállítása `"external"` való `true` tájékoztatja a Data Factory szolgáltatásnak, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7b93e-444">In this example, setting `"external"` to `true` informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="7b93e-445">**Azure blobkimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="7b93e-445">**Azure blob output dataset**</span></span>

<span data-ttu-id="7b93e-446">A következő példában adatot ír egy új blob minden órában (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="7b93e-446">In the following example, data is written to a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="7b93e-447">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="7b93e-447">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="7b93e-448">A mappa elérési útját használja, év, hónap, nap, és a kezdési idő órában része.</span><span class="sxs-lookup"><span data-stu-id="7b93e-448">The folder path uses the year, month, day, and hours portion of the start time.</span></span>

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

<span data-ttu-id="7b93e-449">**A másolási tevékenység során az Azure Data Lake Store és egy blob fogadó egy folyamaton belül**</span><span class="sxs-lookup"><span data-stu-id="7b93e-449">**A copy activity in a pipeline with an Azure Data Lake Store source and a blob sink**</span></span>

<span data-ttu-id="7b93e-450">A következő példában a feldolgozási sor tartalmazza a másolási tevékenység során használja a bemeneti és kimeneti adatkészletek van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7b93e-450">In the following example, the pipeline contains a copy activity that is configured to use the input and output datasets.</span></span> <span data-ttu-id="7b93e-451">A másolási tevékenység óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="7b93e-451">The copy activity is scheduled to run every hour.</span></span> <span data-ttu-id="7b93e-452">Az adatcsatorna JSON-definícióból a `source` típusúra `AzureDataLakeStoreSource`, és a `sink` típusúra `BlobSink`.</span><span class="sxs-lookup"><span data-stu-id="7b93e-452">In the pipeline JSON definition, the `source` type is set to `AzureDataLakeStoreSource`, and the `sink` type is set to `BlobSink`.</span></span>

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

<span data-ttu-id="7b93e-453">A másolási tevékenység definíciójának oszlop szerepel a fogadó dataset a forrás adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="7b93e-453">In the copy activity definition, you can also map columns from the source dataset to columns in the sink dataset.</span></span> <span data-ttu-id="7b93e-454">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7b93e-454">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="7b93e-455">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="7b93e-455">Performance and tuning</span></span>
<span data-ttu-id="7b93e-456">A másolási tevékenység teljesítmény- és hogyan optimalizálható az azt befolyásoló tényezők kapcsolatos további tudnivalókért lásd: a [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7b93e-456">To learn about the factors that affect Copy Activity performance and how to optimize it, see the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) article.</span></span>
