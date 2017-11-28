---
title: "Gyors üzembe helyezési: az Azure Machine Learning javaslatok-API (ver. 1.) |} Microsoft Docs"
description: "Az Azure gépi tanulási ajánlásokkal – gyors üzembe helyezési útmutató"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d8f98e85f723a1104cb169b26d797934140979f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-start-guide-for-hello-machine-learning-recommendations-api-version-1"></a><span data-ttu-id="8e904-104">Rövid útmutató az hello Machine Learning javaslatok API (verzió: 1)</span><span class="sxs-lookup"><span data-stu-id="8e904-104">Quick start guide for hello Machine Learning Recommendations API (version 1)</span></span>

> [!NOTE]
> <span data-ttu-id="8e904-105">Hello webalkalmazásokba [javaslatok API kognitív szolgáltatás](https://www.microsoft.com/cognitive-services/recommendations-api) helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="8e904-105">You should start using hello [Recommendations API Cognitive Service](https://www.microsoft.com/cognitive-services/recommendations-api) instead of this version.</span></span> <span data-ttu-id="8e904-106">hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="8e904-106">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="8e904-107">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="8e904-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
>
> <span data-ttu-id="8e904-108">További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate).</span><span class="sxs-lookup"><span data-stu-id="8e904-108">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate).</span></span>
> 
> 

<span data-ttu-id="8e904-109">Ez a dokumentum ismerteti, hogyan tooonboard a szolgáltatás vagy alkalmazás toouse Microsoft Azure Machine Learning javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="8e904-109">This document describes how tooonboard your service or application toouse Microsoft Azure Machine Learning Recommendations.</span></span> <span data-ttu-id="8e904-110">További részletekért található hello javaslatok API a hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span><span class="sxs-lookup"><span data-stu-id="8e904-110">You can find more details on hello Recommendations API in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a><span data-ttu-id="8e904-111">Általános – áttekintés</span><span class="sxs-lookup"><span data-stu-id="8e904-111">General overview</span></span>
<span data-ttu-id="8e904-112">toouse Azure Machine Learning ajánlásokat, kell tootake hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8e904-112">toouse Azure Machine Learning Recommendations, you need tootake hello following steps:</span></span>

* <span data-ttu-id="8e904-113">Hozzon létre egy modell - modell egy olyan tároló, a használati adatok, a katalógus adatait és a hello javaslat modell.</span><span class="sxs-lookup"><span data-stu-id="8e904-113">Create a model - A model is a container of your usage data, catalog data, and hello recommendation model.</span></span>
* <span data-ttu-id="8e904-114">Katalógus adatokat importálhat - katalógus hello cikkek metaadatai információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8e904-114">Import catalog data - A catalog contains metadata information on hello items.</span></span> 
* <span data-ttu-id="8e904-115">Használati adatok importálása - használati adatok is feltölthetők a két módszer egyikével (vagy mindkettőt):</span><span class="sxs-lookup"><span data-stu-id="8e904-115">Import usage data - Usage data can be uploaded in one of two ways (or both):</span></span>
  * <span data-ttu-id="8e904-116">Hello használati adatokat tartalmazó fájl feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="8e904-116">By uploading a file that contains hello usage data.</span></span>
  * <span data-ttu-id="8e904-117">Küldött adatok megszerzésére események.</span><span class="sxs-lookup"><span data-stu-id="8e904-117">By sending data acquisition events.</span></span>
    <span data-ttu-id="8e904-118">Általában a rendelés toobe képes toocreate egy kezdeti javaslat modell (a rendszerindítási) használati-fájl feltöltése, és akkor használható, amíg hello rendszer elegendő adatot gyűjt hello adatok megszerzése formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="8e904-118">Usually you upload a usage file in order toobe able toocreate an initial recommendation model (bootstrap) and use it until hello system gathers enough data by using hello data acquisition format.</span></span>
* <span data-ttu-id="8e904-119">A javaslat modell létrehozása – Ez egy aszinkron művelet, mely hello javaslat rendszer összes hello használati adatokat fogad, és létrehoz egy javaslat modell.</span><span class="sxs-lookup"><span data-stu-id="8e904-119">Build a recommendation model - This is an asynchronous operation in which hello recommendation system takes all hello usage data and creates a recommendation model.</span></span> <span data-ttu-id="8e904-120">Ez a művelet több percet is igénybe vehet, vagy hello függően több órába hello adatok és hello méretének összeállíthatja konfigurációs paramétereket.</span><span class="sxs-lookup"><span data-stu-id="8e904-120">This operation can take several minutes or several hours depending on hello size of hello data and hello build configuration parameters.</span></span> <span data-ttu-id="8e904-121">Hello build váltanak, amikor elérhetővé válik egy build.</span><span class="sxs-lookup"><span data-stu-id="8e904-121">When triggering hello build, you will get a build ID.</span></span> <span data-ttu-id="8e904-122">Felhasználhatja azt toocheck hello felépítési folyamat véget ért tooconsume javaslatok megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="8e904-122">Use it toocheck when hello build process has ended before starting tooconsume recommendations.</span></span>
* <span data-ttu-id="8e904-123">Használja a javaslatok - Get ajánlások egy adott elem vagy elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="8e904-123">Consume recommendations - Get recommendations for a specific item or list of items.</span></span>

<span data-ttu-id="8e904-124">A fenti hello lépéseket hello Azure Machine Learning javaslatok API keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="8e904-124">All hello steps above are done through hello Azure Machine Learning Recommendations API.</span></span>  <span data-ttu-id="8e904-125">Letöltheti a mintaalkalmazás, amely megvalósítja az egyes lépéseket a hello [gyűjtemény is.](http://1drv.ms/1xeO2F3)</span><span class="sxs-lookup"><span data-stu-id="8e904-125">You can download a sample application that implements each of these steps from hello [gallery as well.](http://1drv.ms/1xeO2F3)</span></span>

## <a name="limitations"></a><span data-ttu-id="8e904-126">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8e904-126">Limitations</span></span>
* <span data-ttu-id="8e904-127">előfizetésenként modellek hello maximális száma érték a 10.</span><span class="sxs-lookup"><span data-stu-id="8e904-127">hello maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="8e904-128">a katalógus rendelkező elemek maximális számát hello 100 000.</span><span class="sxs-lookup"><span data-stu-id="8e904-128">hello maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="8e904-129">mindig használati pontok maximális számát hello ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="8e904-129">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="8e904-130">Ha újakat feltöltött vagy fogja jelenteni a legrégebbi hello törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="8e904-130">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="8e904-131">hello maximális a FELADÁS egy vagy több (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok mérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="8e904-131">hello maximum size of data that can be sent in POST (for example, import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="8e904-132">hello másodpercenkénti javaslat modell build nem aktív tranzakciók száma van ~ 2TPS.</span><span class="sxs-lookup"><span data-stu-id="8e904-132">hello number of transactions per second for a recommendation model build that is not active is ~2TPS.</span></span> <span data-ttu-id="8e904-133">Aktív javaslat modell build mentése too20TPS tárolására képes.</span><span class="sxs-lookup"><span data-stu-id="8e904-133">A recommendation model build that is active can hold up too20TPS.</span></span>

## <a name="integration"></a><span data-ttu-id="8e904-134">Integráció</span><span class="sxs-lookup"><span data-stu-id="8e904-134">Integration</span></span>
### <a name="authentication"></a><span data-ttu-id="8e904-135">Authentication</span><span class="sxs-lookup"><span data-stu-id="8e904-135">Authentication</span></span>
<span data-ttu-id="8e904-136">A Microsoft Azure piactérről vagy hello Basic vagy az OAuth hitelesítési módszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="8e904-136">Microsoft Azure Marketplace supports either hello Basic or OAuth authentication method.</span></span> <span data-ttu-id="8e904-137">Könnyedén megtalálhatja a hello kulcsait hello piactér toohello kulcsokat navigálva [fiókbeállításokban](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="8e904-137">You can easily find hello account keys by navigating toohello keys in hello marketplace under [your account settings](https://datamarket.azure.com/account/keys).</span></span> 

#### <a name="basic-authentication"></a><span data-ttu-id="8e904-138">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8e904-138">Basic Authentication</span></span>
<span data-ttu-id="8e904-139">Hello engedélyezési fejléc hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="8e904-139">Add hello Authorization header:</span></span>

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

<span data-ttu-id="8e904-140">Átalakítás tooBase64 (C#)</span><span class="sxs-lookup"><span data-stu-id="8e904-140">Convert tooBase64 (C#)</span></span>

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

<span data-ttu-id="8e904-141">Átalakítás tooBase64 (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="8e904-141">Convert tooBase64 (JavaScript)</span></span>

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a><span data-ttu-id="8e904-142">Szolgáltatás URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-142">Service URI</span></span>
<span data-ttu-id="8e904-143">hello szolgáltatás gyökér URI hello Azure Machine Learning javaslatok API-k [itt.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span><span class="sxs-lookup"><span data-stu-id="8e904-143">hello service root URI for hello Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span></span>

<span data-ttu-id="8e904-144">hello teljes szolgáltatás URI kifejezett elemek hello OData specifikáció használatával.</span><span class="sxs-lookup"><span data-stu-id="8e904-144">hello full service URI is expressed using elements of hello OData specification.</span></span>

### <a name="api-version"></a><span data-ttu-id="8e904-145">API-verzió</span><span class="sxs-lookup"><span data-stu-id="8e904-145">API version</span></span>
<span data-ttu-id="8e904-146">Minden API-hívás fog működni, hello végén apiVersion be kell állítania egy lekérdezési paraméter túl "1.0".</span><span class="sxs-lookup"><span data-stu-id="8e904-146">Each API call will have, at hello end, a query parameter called apiVersion that should be set too"1.0".</span></span>

### <a name="ids-are-case-sensitive"></a><span data-ttu-id="8e904-147">Azonosítók-és nagybetűk</span><span class="sxs-lookup"><span data-stu-id="8e904-147">IDs are case-sensitive</span></span>
<span data-ttu-id="8e904-148">Azonosítók, bármelyik hello API-k, visszaadott kis-és nagybetűket, és kell használni, így amikor későbbi API-hívásokban paraméterként.</span><span class="sxs-lookup"><span data-stu-id="8e904-148">IDs, returned by any of hello APIs, are case-sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="8e904-149">Például modellazonosítóját és a katalógus azonosítók nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="8e904-149">For instance, model IDs and catalog IDs are case-sensitive.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="8e904-150">Modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e904-150">Create a model</span></span>
<span data-ttu-id="8e904-151">Egy "létrehozása a modell" kérést hoz létre:</span><span class="sxs-lookup"><span data-stu-id="8e904-151">Creating a "create model" request:</span></span>

| <span data-ttu-id="8e904-152">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-152">HTTP Method</span></span> | <span data-ttu-id="8e904-153">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-153">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-154">POST</span><span class="sxs-lookup"><span data-stu-id="8e904-154">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-155">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-155">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-156">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-156">Parameter Name</span></span> | <span data-ttu-id="8e904-157">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-157">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-158">modelName</span><span class="sxs-lookup"><span data-stu-id="8e904-158">modelName</span></span> |<span data-ttu-id="8e904-159">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="8e904-159">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="8e904-160">Maximális hossz: 20</span><span class="sxs-lookup"><span data-stu-id="8e904-160">Max length: 20</span></span> |
| <span data-ttu-id="8e904-161">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-161">apiVersion</span></span> |<span data-ttu-id="8e904-162">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-162">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-163">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="8e904-163">Request Body</span></span> |<span data-ttu-id="8e904-164">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="8e904-164">NONE</span></span> |

<span data-ttu-id="8e904-165">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-165">**Response**:</span></span>

<span data-ttu-id="8e904-166">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-166">HTTP Status code: 200</span></span>

* <span data-ttu-id="8e904-167">`feed/entry/content/properties/id`-Hello modell eltérő azonosítót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8e904-167">`feed/entry/content/properties/id` - Contains hello model ID.</span></span>
  <span data-ttu-id="8e904-168">Ne feledje, hogy hello Modellazonosító kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="8e904-168">Note that hello model ID is case-sensitive.</span></span>

<span data-ttu-id="8e904-169">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-169">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a><span data-ttu-id="8e904-170">Eseménykatalógus-adatok importálása</span><span class="sxs-lookup"><span data-stu-id="8e904-170">Import catalog data</span></span>
<span data-ttu-id="8e904-171">Ha több katalógus fájlok toohello ugyanaz a modell több hívások tölt fel, azt csak hello új katalóguselemek szúrja be.</span><span class="sxs-lookup"><span data-stu-id="8e904-171">If you upload several catalog files toohello same model with several calls, we will insert only hello new catalog items.</span></span> <span data-ttu-id="8e904-172">A meglévő elemek hello eredeti értékekkel marad.</span><span class="sxs-lookup"><span data-stu-id="8e904-172">Existing items will remain with hello original values.</span></span>

| <span data-ttu-id="8e904-173">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-173">HTTP Method</span></span> | <span data-ttu-id="8e904-174">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-174">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-175">POST</span><span class="sxs-lookup"><span data-stu-id="8e904-175">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-176">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-176">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-177">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-177">Parameter Name</span></span> | <span data-ttu-id="8e904-178">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-178">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-179">modelId</span><span class="sxs-lookup"><span data-stu-id="8e904-179">modelId</span></span> |<span data-ttu-id="8e904-180">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-180">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-181">fájlnév</span><span class="sxs-lookup"><span data-stu-id="8e904-181">filename</span></span> |<span data-ttu-id="8e904-182">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-182">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="8e904-183">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="8e904-183">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="8e904-184">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="8e904-184">Max length: 50</span></span> |
| <span data-ttu-id="8e904-185">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-185">apiVersion</span></span> |<span data-ttu-id="8e904-186">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-186">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-187">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="8e904-187">Request Body</span></span> |<span data-ttu-id="8e904-188">Katalógus-adatokat.</span><span class="sxs-lookup"><span data-stu-id="8e904-188">Catalog data.</span></span> <span data-ttu-id="8e904-189">Formátum:</span><span class="sxs-lookup"><span data-stu-id="8e904-189">Format:</span></span><br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th><span data-ttu-id="8e904-190">Név</span><span class="sxs-lookup"><span data-stu-id="8e904-190">Name</span></span></th><th><span data-ttu-id="8e904-191">Kötelező</span><span class="sxs-lookup"><span data-stu-id="8e904-191">Mandatory</span></span></th><th><span data-ttu-id="8e904-192">Típus</span><span class="sxs-lookup"><span data-stu-id="8e904-192">Type</span></span></th><th><span data-ttu-id="8e904-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e904-193">Description</span></span></th></tr><tr><td><span data-ttu-id="8e904-194">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-194">Item Id</span></span></td><td><span data-ttu-id="8e904-195">Igen</span><span class="sxs-lookup"><span data-stu-id="8e904-195">Yes</span></span></td><td><span data-ttu-id="8e904-196">Alfanumerikus, a maximális hossz 50</span><span class="sxs-lookup"><span data-stu-id="8e904-196">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="8e904-197">Egy elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-197">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="8e904-198">Elem neve</span><span class="sxs-lookup"><span data-stu-id="8e904-198">Item Name</span></span></td><td><span data-ttu-id="8e904-199">Igen</span><span class="sxs-lookup"><span data-stu-id="8e904-199">Yes</span></span></td><td><span data-ttu-id="8e904-200">Alfanumerikus, a maximális hossz 255</span><span class="sxs-lookup"><span data-stu-id="8e904-200">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="8e904-201">Elem neve</span><span class="sxs-lookup"><span data-stu-id="8e904-201">Item name</span></span></td></tr><tr><td><span data-ttu-id="8e904-202">Elem kategória</span><span class="sxs-lookup"><span data-stu-id="8e904-202">Item Category</span></span></td><td><span data-ttu-id="8e904-203">Igen</span><span class="sxs-lookup"><span data-stu-id="8e904-203">Yes</span></span></td><td><span data-ttu-id="8e904-204">Alfanumerikus, a maximális hossz 255</span><span class="sxs-lookup"><span data-stu-id="8e904-204">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="8e904-205">Kategória toowhich ez elem tartozik (például főzés könyvek tragédiát...)</span><span class="sxs-lookup"><span data-stu-id="8e904-205">Category toowhich this item belongs (for example, Cooking Books, Drama…)</span></span></td></tr><tr><td><span data-ttu-id="8e904-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e904-206">Description</span></span></td><td><span data-ttu-id="8e904-207">Nem</span><span class="sxs-lookup"><span data-stu-id="8e904-207">No</span></span></td><td><span data-ttu-id="8e904-208">Alfanumerikus, maximális hosszúság 4000</span><span class="sxs-lookup"><span data-stu-id="8e904-208">Alphanumeric, max length 4000</span></span></td><td><span data-ttu-id="8e904-209">Ez az elem leírása</span><span class="sxs-lookup"><span data-stu-id="8e904-209">Description of this item</span></span></td></tr></table><br><span data-ttu-id="8e904-210">Maximális fájlmérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="8e904-210">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="8e904-211">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-211">Example:</span></span><br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

<span data-ttu-id="8e904-212">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-212">**Response**:</span></span>

<span data-ttu-id="8e904-213">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-213">HTTP Status code: 200</span></span>

* <span data-ttu-id="8e904-214">`Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="8e904-214">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="8e904-215">`Feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="8e904-215">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>

<span data-ttu-id="8e904-216">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-216">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a><span data-ttu-id="8e904-217">Használati adatok importálása</span><span class="sxs-lookup"><span data-stu-id="8e904-217">Import usage data</span></span>
#### <a name="uploading-a-file"></a><span data-ttu-id="8e904-218">A fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="8e904-218">Uploading a file</span></span>
<span data-ttu-id="8e904-219">Ez a szakasz bemutatja, hogyan tooupload használati adatok egy fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="8e904-219">This section shows how tooupload usage data by using a file.</span></span> <span data-ttu-id="8e904-220">A használati adatokat tartalmazó többször API hívása.</span><span class="sxs-lookup"><span data-stu-id="8e904-220">You can call this API several times with usage data.</span></span> <span data-ttu-id="8e904-221">Minden használati adatokat a rendszer minden hívások menti.</span><span class="sxs-lookup"><span data-stu-id="8e904-221">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="8e904-222">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-222">HTTP Method</span></span> | <span data-ttu-id="8e904-223">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-223">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-224">POST</span><span class="sxs-lookup"><span data-stu-id="8e904-224">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-225">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-225">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-226">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-226">Parameter Name</span></span> | <span data-ttu-id="8e904-227">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-227">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-228">modelId</span><span class="sxs-lookup"><span data-stu-id="8e904-228">modelId</span></span> |<span data-ttu-id="8e904-229">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-229">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-230">fájlnév</span><span class="sxs-lookup"><span data-stu-id="8e904-230">filename</span></span> |<span data-ttu-id="8e904-231">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-231">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="8e904-232">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="8e904-232">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="8e904-233">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="8e904-233">Max length: 50</span></span> |
| <span data-ttu-id="8e904-234">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-234">apiVersion</span></span> |<span data-ttu-id="8e904-235">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-235">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-236">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="8e904-236">Request Body</span></span> |<span data-ttu-id="8e904-237">Használati adatok.</span><span class="sxs-lookup"><span data-stu-id="8e904-237">Usage data.</span></span> <span data-ttu-id="8e904-238">Formátum:</span><span class="sxs-lookup"><span data-stu-id="8e904-238">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="8e904-239">Név</span><span class="sxs-lookup"><span data-stu-id="8e904-239">Name</span></span></th><th><span data-ttu-id="8e904-240">Kötelező</span><span class="sxs-lookup"><span data-stu-id="8e904-240">Mandatory</span></span></th><th><span data-ttu-id="8e904-241">Típus</span><span class="sxs-lookup"><span data-stu-id="8e904-241">Type</span></span></th><th><span data-ttu-id="8e904-242">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e904-242">Description</span></span></th></tr><tr><td><span data-ttu-id="8e904-243">Felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="8e904-243">User Id</span></span></td><td><span data-ttu-id="8e904-244">Igen</span><span class="sxs-lookup"><span data-stu-id="8e904-244">Yes</span></span></td><td><span data-ttu-id="8e904-245">Alfanumerikus</span><span class="sxs-lookup"><span data-stu-id="8e904-245">Alphanumeric</span></span></td><td><span data-ttu-id="8e904-246">A felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-246">Unique identifier of a user</span></span></td></tr><tr><td><span data-ttu-id="8e904-247">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-247">Item Id</span></span></td><td><span data-ttu-id="8e904-248">Igen</span><span class="sxs-lookup"><span data-stu-id="8e904-248">Yes</span></span></td><td><span data-ttu-id="8e904-249">Alfanumerikus, a maximális hossz 50</span><span class="sxs-lookup"><span data-stu-id="8e904-249">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="8e904-250">Egy elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-250">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="8e904-251">Time</span><span class="sxs-lookup"><span data-stu-id="8e904-251">Time</span></span></td><td><span data-ttu-id="8e904-252">Nem</span><span class="sxs-lookup"><span data-stu-id="8e904-252">No</span></span></td><td><span data-ttu-id="8e904-253">Dátum formátumban: éééé/hh/nnTóó: pp: (06 például 2013/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="8e904-253">Date in format: YYYY/MM/DDTHH:MM:SS (for example, 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="8e904-254">Az adatok ideje</span><span class="sxs-lookup"><span data-stu-id="8e904-254">Time of data</span></span></td></tr><tr><td><span data-ttu-id="8e904-255">Esemény</span><span class="sxs-lookup"><span data-stu-id="8e904-255">Event</span></span></td><td><span data-ttu-id="8e904-256">Nem, ha a megadott, akkor is kell helyezni a dátum</span><span class="sxs-lookup"><span data-stu-id="8e904-256">No, if supplied then must also put date</span></span></td><td><span data-ttu-id="8e904-257">Hello a következők egyike:</span><span class="sxs-lookup"><span data-stu-id="8e904-257">One of hello following:</span></span><br><span data-ttu-id="8e904-258">• Kattintson</span><span class="sxs-lookup"><span data-stu-id="8e904-258">• Click</span></span><br><span data-ttu-id="8e904-259">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="8e904-259">• RecommendationClick</span></span><br><span data-ttu-id="8e904-260">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="8e904-260">•    AddShopCart</span></span><br><span data-ttu-id="8e904-261">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="8e904-261">• RemoveShopCart</span></span><br><span data-ttu-id="8e904-262">• Beszerzési</span><span class="sxs-lookup"><span data-stu-id="8e904-262">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="8e904-263">Maximális fájlmérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="8e904-263">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="8e904-264">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-264">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="8e904-265">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-265">**Response**:</span></span>

<span data-ttu-id="8e904-266">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-266">HTTP Status code: 200</span></span>

* <span data-ttu-id="8e904-267">`Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="8e904-267">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="8e904-268">`Feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="8e904-268">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>
* <span data-ttu-id="8e904-269">`Feed\entry\content\properties\FileId`-Fájl azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-269">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="8e904-270">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-270">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a><span data-ttu-id="8e904-271">Adatgyűjtést használatával</span><span class="sxs-lookup"><span data-stu-id="8e904-271">Using data acquisition</span></span>
<span data-ttu-id="8e904-272">Ez a szakasz bemutatja, hogyan toosend események valós időben tooAzure Machine Learning ajánlásokat, általában a webhelyről.</span><span class="sxs-lookup"><span data-stu-id="8e904-272">This section shows how toosend events in real time tooAzure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="8e904-273">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-273">HTTP Method</span></span> | <span data-ttu-id="8e904-274">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-275">POST</span><span class="sxs-lookup"><span data-stu-id="8e904-275">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-276">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-276">Parameter Name</span></span> | <span data-ttu-id="8e904-277">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-277">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-278">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-278">apiVersion</span></span> |<span data-ttu-id="8e904-279">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-279">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-280">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="8e904-280">Request body</span></span> |<span data-ttu-id="8e904-281">Esemény adatbevitel minden esemény toosend keresi.</span><span class="sxs-lookup"><span data-stu-id="8e904-281">Event data entry for each event you want toosend.</span></span> <span data-ttu-id="8e904-282">Küldje el a hello ugyanazon felhasználó vagy a böngésző-munkamenet hello hello munkamenet-azonosító mezőben ugyanazzal az Azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="8e904-282">You should send for hello same user or browser session hello same ID in hello SessionId field.</span></span> <span data-ttu-id="8e904-283">(Lásd az alábbi esemény törzsében mintáját.)</span><span class="sxs-lookup"><span data-stu-id="8e904-283">(See sample of event body below.)</span></span> |

* <span data-ttu-id="8e904-284">Példa a "Kattintson" esemény:</span><span class="sxs-lookup"><span data-stu-id="8e904-284">Example for event 'Click':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="8e904-285">Példa a "RecommendationClick" esemény:</span><span class="sxs-lookup"><span data-stu-id="8e904-285">Example for event 'RecommendationClick':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="8e904-286">Példa a "AddShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="8e904-286">Example for event 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="8e904-287">Példa a "RemoveShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="8e904-287">Example for event 'RemoveShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="8e904-288">Példa a "Vásárlás" esemény:</span><span class="sxs-lookup"><span data-stu-id="8e904-288">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="8e904-289">Példa 2 események "kattintson a" és "AddShopCart" küldésekor:</span><span class="sxs-lookup"><span data-stu-id="8e904-289">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

<span data-ttu-id="8e904-290">**Válasz**: HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-290">**Response**: HTTP Status code: 200</span></span>

### <a name="build-a-recommendation-model"></a><span data-ttu-id="8e904-291">A javaslat modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e904-291">Build a recommendation model</span></span>
| <span data-ttu-id="8e904-292">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-292">HTTP Method</span></span> | <span data-ttu-id="8e904-293">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-293">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-294">POST</span><span class="sxs-lookup"><span data-stu-id="8e904-294">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-295">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-295">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-296">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-296">Parameter Name</span></span> | <span data-ttu-id="8e904-297">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-297">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-298">modelId</span><span class="sxs-lookup"><span data-stu-id="8e904-298">modelId</span></span> |<span data-ttu-id="8e904-299">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-299">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-300">userDescription</span><span class="sxs-lookup"><span data-stu-id="8e904-300">userDescription</span></span> |<span data-ttu-id="8e904-301">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-301">Textual identifier of hello catalog.</span></span> <span data-ttu-id="8e904-302">Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette.</span><span class="sxs-lookup"><span data-stu-id="8e904-302">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="8e904-303">Tekintse meg a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="8e904-303">See example above.</span></span><br><span data-ttu-id="8e904-304">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="8e904-304">Max length: 50</span></span> |
| <span data-ttu-id="8e904-305">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-305">apiVersion</span></span> |<span data-ttu-id="8e904-306">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-306">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-307">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="8e904-307">Request Body</span></span> |<span data-ttu-id="8e904-308">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="8e904-308">NONE</span></span> |

<span data-ttu-id="8e904-309">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-309">**Response**:</span></span>

<span data-ttu-id="8e904-310">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-310">HTTP Status code: 200</span></span>

<span data-ttu-id="8e904-311">Ez az egy aszinkron API-t.</span><span class="sxs-lookup"><span data-stu-id="8e904-311">This is an asynchronous API.</span></span> <span data-ttu-id="8e904-312">Válaszként elérhetővé válik egy build azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-312">You will get a build ID as a response.</span></span> <span data-ttu-id="8e904-313">Amikor véget ért a hello build tooknow, hello "Beolvasása buildek állapota, a modell" API hívása, és keresse meg a build azonosító hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="8e904-313">tooknow when hello build has ended, you should call hello "Get Builds Status of a Model" API and locate this build ID in hello response.</span></span> <span data-ttu-id="8e904-314">Ne feledje, hogy a build telhet-e a perc toohours hello hello adatok méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="8e904-314">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="8e904-315">Javaslatok keretein belül hello build vége nem lehet felhasználni.</span><span class="sxs-lookup"><span data-stu-id="8e904-315">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="8e904-316">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="8e904-316">Valid build status:</span></span>

* <span data-ttu-id="8e904-317">Hozzon létre – modell lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="8e904-317">Create – Model was created.</span></span>
* <span data-ttu-id="8e904-318">Aszinkron – modell build lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="8e904-318">Queued – Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="8e904-319">Épület – modell éppen készül.</span><span class="sxs-lookup"><span data-stu-id="8e904-319">Building – Model is being built.</span></span>
* <span data-ttu-id="8e904-320">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8e904-320">Success – Build ended successfully.</span></span>
* <span data-ttu-id="8e904-321">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="8e904-321">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="8e904-322">Visszavonva – Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="8e904-322">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="8e904-323">Visszavonása – Build megszakítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="8e904-323">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="8e904-324">Vegye figyelembe, hogy hello build azonosító hello a következő elérési út alatt található:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="8e904-324">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="8e904-325">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-325">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a><span data-ttu-id="8e904-326">A modell létrehozási állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="8e904-326">Get build status of a model</span></span>
| <span data-ttu-id="8e904-327">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-327">HTTP Method</span></span> | <span data-ttu-id="8e904-328">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-328">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-329">GET</span><span class="sxs-lookup"><span data-stu-id="8e904-329">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-330">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-330">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-331">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-331">Parameter Name</span></span> | <span data-ttu-id="8e904-332">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-332">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-333">modelId</span><span class="sxs-lookup"><span data-stu-id="8e904-333">modelId</span></span> |<span data-ttu-id="8e904-334">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-334">Unique identifier of hello model  (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-335">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="8e904-335">onlyLastBuild</span></span> |<span data-ttu-id="8e904-336">Azt jelzi, hogy összes hello tooreturn build hello modell előzmények vagy hello legutóbbi build csak hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="8e904-336">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build.</span></span> |
| <span data-ttu-id="8e904-337">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-337">apiVersion</span></span> |<span data-ttu-id="8e904-338">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-338">1.0</span></span> |

<span data-ttu-id="8e904-339">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-339">**Response**:</span></span>

<span data-ttu-id="8e904-340">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-340">HTTP Status code: 200</span></span>

<span data-ttu-id="8e904-341">hello válasz tartalmazza a build egy tételt.</span><span class="sxs-lookup"><span data-stu-id="8e904-341">hello response includes one entry per build.</span></span> <span data-ttu-id="8e904-342">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="8e904-342">Each entry has hello following data:</span></span>

* <span data-ttu-id="8e904-343">`feed/entry/content/properties/UserName`– Hello felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="8e904-343">`feed/entry/content/properties/UserName` – Name of hello user.</span></span>
* <span data-ttu-id="8e904-344">`feed/entry/content/properties/ModelName`– Hello modell neve.</span><span class="sxs-lookup"><span data-stu-id="8e904-344">`feed/entry/content/properties/ModelName` – Name of hello model.</span></span>
* <span data-ttu-id="8e904-345">`feed/entry/content/properties/ModelId`– Minta egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e904-345">`feed/entry/content/properties/ModelId` – Model unique identifier.</span></span>
* <span data-ttu-id="8e904-346">`feed/entry/content/properties/IsDeployed`– E hello buildverziót központilag telepítik (más néven</span><span class="sxs-lookup"><span data-stu-id="8e904-346">`feed/entry/content/properties/IsDeployed` – Whether hello build is deployed (a.k.a.</span></span> <span data-ttu-id="8e904-347">aktív build).</span><span class="sxs-lookup"><span data-stu-id="8e904-347">active build).</span></span>
* <span data-ttu-id="8e904-348">`feed/entry/content/properties/BuildId`– Az egyedi azonosító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8e904-348">`feed/entry/content/properties/BuildId` – Build unique identifier.</span></span>
* <span data-ttu-id="8e904-349">`feed/entry/content/properties/BuildType`-Hello build típusú.</span><span class="sxs-lookup"><span data-stu-id="8e904-349">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="8e904-350">`feed/entry/content/properties/Status`– Létrehozási állapot.</span><span class="sxs-lookup"><span data-stu-id="8e904-350">`feed/entry/content/properties/Status` – Build status.</span></span> <span data-ttu-id="8e904-351">Hello a következők egyike lehet: Hiba történt, épület, várakozik, Canceling, visszavonva, sikeres</span><span class="sxs-lookup"><span data-stu-id="8e904-351">Can be one of hello following: Error, Building, Queued, Canceling, Canceled, Success</span></span>
* <span data-ttu-id="8e904-352">`feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak toospecific állapotok vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="8e904-352">`feed/entry/content/properties/StatusMessage` – Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="8e904-353">`feed/entry/content/properties/Progress`– Összeállítása folyamatban (%).</span><span class="sxs-lookup"><span data-stu-id="8e904-353">`feed/entry/content/properties/Progress` – Build progress (%).</span></span>
* <span data-ttu-id="8e904-354">`feed/entry/content/properties/StartTime`– A kezdő időpont felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8e904-354">`feed/entry/content/properties/StartTime` – Build start time.</span></span>
* <span data-ttu-id="8e904-355">`feed/entry/content/properties/EndTime`– Build befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="8e904-355">`feed/entry/content/properties/EndTime` – Build end time.</span></span>
* <span data-ttu-id="8e904-356">`feed/entry/content/properties/ExecutionTime`– Build időtartama.</span><span class="sxs-lookup"><span data-stu-id="8e904-356">`feed/entry/content/properties/ExecutionTime` – Build duration.</span></span>
* <span data-ttu-id="8e904-357">`feed/entry/content/properties/ProgressStep`– Hello jelenlegi fázisa, amely a folyamatban lévő build adatait.</span><span class="sxs-lookup"><span data-stu-id="8e904-357">`feed/entry/content/properties/ProgressStep` – Details about hello current stage that a build in progress is in.</span></span>

<span data-ttu-id="8e904-358">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="8e904-358">Valid build status:</span></span>

* <span data-ttu-id="8e904-359">Létrehozott – Build kérelem tétel jött létre.</span><span class="sxs-lookup"><span data-stu-id="8e904-359">Created – Build request entry was created.</span></span>
* <span data-ttu-id="8e904-360">Aszinkron – kérelmet lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="8e904-360">Queued – Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="8e904-361">Épület – összeállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="8e904-361">Building – Build is in process.</span></span>
* <span data-ttu-id="8e904-362">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8e904-362">Success – Build ended successfully.</span></span>
* <span data-ttu-id="8e904-363">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="8e904-363">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="8e904-364">Visszavonva – Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="8e904-364">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="8e904-365">Visszavonása – Build megszakítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="8e904-365">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="8e904-366">Build típusú engedélyezett érvényes értékek:</span><span class="sxs-lookup"><span data-stu-id="8e904-366">Valid values for build type:</span></span>

* <span data-ttu-id="8e904-367">Sorszám - dimenziószáma felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8e904-367">Rank - Rank build.</span></span> <span data-ttu-id="8e904-368">(Dimenziószáma összeállítása a részleteket, olvassa el "Machine Learning javaslat API dokumentációjában" dokumentum toohello.)</span><span class="sxs-lookup"><span data-stu-id="8e904-368">(For rank build details, please refer toohello "Machine Learning Recommendation API documentation" document.)</span></span>
* <span data-ttu-id="8e904-369">A javaslat - javaslat build.</span><span class="sxs-lookup"><span data-stu-id="8e904-369">Recommendation - Recommendation build.</span></span>
* <span data-ttu-id="8e904-370">FBT - együtt build gyakran vásárolt.</span><span class="sxs-lookup"><span data-stu-id="8e904-370">Fbt - Frequently bought together build.</span></span>

<span data-ttu-id="8e904-371">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-371">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a><span data-ttu-id="8e904-372">Javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="8e904-372">Get recommendations</span></span>
| <span data-ttu-id="8e904-373">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-373">HTTP Method</span></span> | <span data-ttu-id="8e904-374">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-374">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-375">GET</span><span class="sxs-lookup"><span data-stu-id="8e904-375">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-376">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-376">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-377">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-377">Parameter Name</span></span> | <span data-ttu-id="8e904-378">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-378">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-379">modelId</span><span class="sxs-lookup"><span data-stu-id="8e904-379">modelId</span></span> |<span data-ttu-id="8e904-380">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-380">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-381">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="8e904-381">itemIds</span></span> |<span data-ttu-id="8e904-382">Hello elemek toorecommend a vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="8e904-382">Comma-separated list of hello items toorecommend for.</span></span><br><span data-ttu-id="8e904-383">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="8e904-383">Max length: 1024</span></span> |
| <span data-ttu-id="8e904-384">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="8e904-384">numberOfResults</span></span> |<span data-ttu-id="8e904-385">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="8e904-385">Number of required results</span></span> |
| <span data-ttu-id="8e904-386">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="8e904-386">includeMetatadata</span></span> |<span data-ttu-id="8e904-387">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="8e904-387">Future use, always false</span></span> |
| <span data-ttu-id="8e904-388">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-388">apiVersion</span></span> |<span data-ttu-id="8e904-389">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-389">1.0</span></span> |

<span data-ttu-id="8e904-390">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="8e904-390">**Response:**</span></span>

<span data-ttu-id="8e904-391">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-391">HTTP Status code: 200</span></span>

<span data-ttu-id="8e904-392">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e904-392">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="8e904-393">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="8e904-393">Each entry has hello following data:</span></span>

* <span data-ttu-id="8e904-394">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="8e904-394">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="8e904-395">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="8e904-395">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="8e904-396">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="8e904-396">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="8e904-397">`Feed\entry\content\properties\Reasoning`-Javaslat indoklást (például a javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="8e904-397">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (for example, recommendation explanations).</span></span>

<span data-ttu-id="8e904-398">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-398">OData XML</span></span>

<span data-ttu-id="8e904-399">az alábbi hello példa egy válasz 10 ajánlott elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8e904-399">hello example response below includes 10 recommended items:</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a><span data-ttu-id="8e904-400">Frissítési modell</span><span class="sxs-lookup"><span data-stu-id="8e904-400">Update model</span></span>
<span data-ttu-id="8e904-401">Frissítés hello modell leírása, vagy hello aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="8e904-401">You can update hello model description or hello active build ID.</span></span>
<span data-ttu-id="8e904-402">*Aktív build azonosító* -minden build minden modell van állítva egy összeállítási.</span><span class="sxs-lookup"><span data-stu-id="8e904-402">*Active build ID* - Every build for every model has a build ID.</span></span> <span data-ttu-id="8e904-403">hello aktív build azonosító: hello első sikeres build minden új modell.</span><span class="sxs-lookup"><span data-stu-id="8e904-403">hello active build ID is hello first successful build of every new model.</span></span> <span data-ttu-id="8e904-404">Amennyiben van egy aktív build ID azonosítója és további változata hello teszi ugyanannak a modellnek van szüksége tooexplicitly állítsa be úgy, mint hello alapértelmezett build azonosító Ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="8e904-404">Once you have an active build ID and you do additional builds for hello same model, you need tooexplicitly set it as hello default build ID if you want to.</span></span> <span data-ttu-id="8e904-405">Ha felhasznált ajánlásokat, ha nem adja meg a hello build azonosítója, amelyet toouse, hello alapértelmezett egy automatikusan fog történni.</span><span class="sxs-lookup"><span data-stu-id="8e904-405">When you consume recommendations, if you do not specify hello build ID that you want toouse, hello default one will be used automatically.</span></span>

<span data-ttu-id="8e904-406">Ez az eljárás lehetővé teszi - Miután javaslat modell éles - toobuild új modellek és tesztelését ahhoz, azok előlépteti tooproduction.</span><span class="sxs-lookup"><span data-stu-id="8e904-406">This mechanism enables you - once you have a recommendation model in production - toobuild new models and test them before you promote them tooproduction.</span></span>

| <span data-ttu-id="8e904-407">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="8e904-407">HTTP Method</span></span> | <span data-ttu-id="8e904-408">URI</span><span class="sxs-lookup"><span data-stu-id="8e904-408">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-409">A PUT</span><span class="sxs-lookup"><span data-stu-id="8e904-409">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="8e904-410">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e904-410">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="8e904-411">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="8e904-411">Parameter Name</span></span> | <span data-ttu-id="8e904-412">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="8e904-412">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e904-413">id</span><span class="sxs-lookup"><span data-stu-id="8e904-413">id</span></span> |<span data-ttu-id="8e904-414">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8e904-414">Unique identifier of hello model (case-sensitive)</span></span> |
| <span data-ttu-id="8e904-415">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e904-415">apiVersion</span></span> |<span data-ttu-id="8e904-416">1.0</span><span class="sxs-lookup"><span data-stu-id="8e904-416">1.0</span></span> |
|  | |
| <span data-ttu-id="8e904-417">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="8e904-417">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br><span data-ttu-id="8e904-418">Ne feledje, hogy hello XML-címkék leírás ActiveBuildId opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="8e904-418">Note that hello XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="8e904-419">Ha nem szeretné, hogy tooset leírás vagy ActiveBuildId, hello teljes címke eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="8e904-419">If you do not want tooset Description or ActiveBuildId, remove hello entire tag.</span></span> |

<span data-ttu-id="8e904-420">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="8e904-420">**Response**:</span></span>

<span data-ttu-id="8e904-421">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="8e904-421">HTTP Status code: 200</span></span>

<span data-ttu-id="8e904-422">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="8e904-422">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a><span data-ttu-id="8e904-423">Jogi tudnivalók</span><span class="sxs-lookup"><span data-stu-id="8e904-423">Legal</span></span>
<span data-ttu-id="8e904-424">Ez a dokumentum biztosított ",-van".</span><span class="sxs-lookup"><span data-stu-id="8e904-424">This document is provided "as-is".</span></span> <span data-ttu-id="8e904-425">Információk és nézetek ebben a dokumentumban, beleértve az URL-CÍMEK és más internetes webhelyet, értesítés nélkül változhatnak.</span><span class="sxs-lookup"><span data-stu-id="8e904-425">Information and views expressed in this document, including URL and other Internet website references, may change without notice.</span></span> <span data-ttu-id="8e904-426">A felhasznált példák némelyike csak illusztrációs célokat szolgálnak, és kitalált esetet szemléltet.</span><span class="sxs-lookup"><span data-stu-id="8e904-426">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="8e904-427">Nincs valós association vagy a kapcsolat célja, vagy eseményekkel.</span><span class="sxs-lookup"><span data-stu-id="8e904-427">No real association or connection is intended or should be inferred.</span></span> <span data-ttu-id="8e904-428">Ez a dokumentum nem biztosít semmilyen jogot tooany semmilyen Microsoft-termékben található szellemi tulajdonhoz.</span><span class="sxs-lookup"><span data-stu-id="8e904-428">This document does not provide you with any legal rights tooany intellectual property in any Microsoft product.</span></span> <span data-ttu-id="8e904-429">Előfordulhat, hogy másolja és használja a dokumentum belső használatra, tájékoztatási céllal.</span><span class="sxs-lookup"><span data-stu-id="8e904-429">You may copy and use this document for your internal, reference purposes.</span></span> <span data-ttu-id="8e904-430">© 2014 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8e904-430">© 2014 Microsoft.</span></span> <span data-ttu-id="8e904-431">Minden jog fenntartva.</span><span class="sxs-lookup"><span data-stu-id="8e904-431">All rights reserved.</span></span> 

