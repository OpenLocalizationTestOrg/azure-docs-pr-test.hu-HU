---
title: "aaaMachine tanulási javaslatok API-dokumentáció |} Microsoft Docs"
description: "Egy hello Microsoft Azure piactéren elérhető javaslatok motor Azure Machine Learning javaslatok API dokumentációjában."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d1cec228bf23870c05c8ab8df2779b0c3c65b06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a><span data-ttu-id="21ca5-103">Azure Machine Learning Recommendations – API-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="21ca5-103">Azure Machine Learning Recommendations API Documentation</span></span>
> [!NOTE]
> <span data-ttu-id="21ca5-104">El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="21ca5-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="21ca5-105">hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="21ca5-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="21ca5-106">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="21ca5-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="21ca5-107">További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="21ca5-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="21ca5-108">Ez a dokumentum a Microsoft Azure Machine Learning javaslatok API-k ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-108">This document depicts Microsoft Azure Machine Learning Recommendations APIs.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="21ca5-109">1. Általános – áttekintés</span><span class="sxs-lookup"><span data-stu-id="21ca5-109">1. General overview</span></span>
<span data-ttu-id="21ca5-110">Ez a dokumentum egy API-hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="21ca5-110">This document is an API reference.</span></span> <span data-ttu-id="21ca5-111">Hello "Azure Machine Learning javaslat – gyors üzembe" dokumentum kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="21ca5-111">You should start with hello “Azure Machine Learning Recommendation - Quick Start” document.</span></span>

<span data-ttu-id="21ca5-112">hello Azure Machine Learning javaslatok API a következő logikai csoportok hello osztható:</span><span class="sxs-lookup"><span data-stu-id="21ca5-112">hello Azure Machine Learning Recommendations API can be divided into hello following logical groups:</span></span>

* <span data-ttu-id="21ca5-113"><ins>Korlátozások</ins> -javaslatok API-korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="21ca5-113"><ins>Limitations</ins> - Recommendations API limitations.</span></span>
* <span data-ttu-id="21ca5-114"><ins>Általános információk</ins> -információkat a hitelesítés, szolgáltatás URI és versioning.</span><span class="sxs-lookup"><span data-stu-id="21ca5-114"><ins>General Information</ins> - Information on authentication, service URI and versioning.</span></span>
* <span data-ttu-id="21ca5-115"><ins>Basic modell</ins> -API-k, amelyek lehetővé teszik toodo hello alapvető műveleteket modell (pl. létrehozása, frissítése és modell törlése).</span><span class="sxs-lookup"><span data-stu-id="21ca5-115"><ins>Model Basic</ins> - APIs that enable you toodo hello basic operations on model (e.g. create, update and delete a model).</span></span>
* <span data-ttu-id="21ca5-116"><ins>A modell speciális</ins> -API-k, amelyek segítségével speciális tooget adatok insights hello modellen.</span><span class="sxs-lookup"><span data-stu-id="21ca5-116"><ins>Model Advanced</ins> - APIs that enable you tooget advanced data insights on hello model.</span></span>
* <span data-ttu-id="21ca5-117"><ins>A modell az üzleti szabályok</ins> -API-k, amelyek lehetővé teszik toomanage üzleti szabályok hello modell javaslat eredmények.</span><span class="sxs-lookup"><span data-stu-id="21ca5-117"><ins>Model Business Rules</ins> - APIs that enable you toomanage business rules on hello model recommendation results.</span></span>
* <span data-ttu-id="21ca5-118"><ins>Katalógus</ins> -API-k, amelyek lehetővé teszik a modell katalógus toodo alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-118"><ins>Catalog</ins> - APIs that enable you toodo basic operations on a model catalog.</span></span> <span data-ttu-id="21ca5-119">A katalógus hello elemek hello használati adatok metaadatok információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-119">A catalog contains metadata information on hello items of hello usage data.</span></span>
* <span data-ttu-id="21ca5-120"><ins>A szolgáltatás</ins> -API-k, amelyek lehetővé teszik a tooget insights elemen hello katalógusba, és hogyan toouse ezen információk toobuild jobb javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-120"><ins>Feature</ins> - APIs that enable tooget insights on item into hello catalog and how toouse this information toobuild better recommendations.</span></span>
* <span data-ttu-id="21ca5-121"><ins>Használati adatok</ins> -API-k, amelyek lehetővé teszik toodo hello modell használati adatok alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-121"><ins>Usage Data</ins> - APIs that enable you toodo basic operations on hello model usage data.</span></span> <span data-ttu-id="21ca5-122">Használati adatok alapszintű hello képernyőn pár &#60; userId &#62; tartalmazó sorok áll, &#60; itemId &#62;.</span><span class="sxs-lookup"><span data-stu-id="21ca5-122">Usage data in hello basic form consists of rows that include pairs of &#60;userId&#62;,&#60;itemId&#62;.</span></span>
* <span data-ttu-id="21ca5-123"><ins>Build</ins> – API-k, amelyek lehetővé teszik a modell tootrigger hozza létre, és alapvető műveleteket, amelyek kapcsolódó toothis build tegye.</span><span class="sxs-lookup"><span data-stu-id="21ca5-123"><ins>Build</ins> - APIs that enable you tootrigger a model build and do basic operations that are related toothis build.</span></span> <span data-ttu-id="21ca5-124">Ha elvégezte a értékes használati adatok modell build indíthat el.</span><span class="sxs-lookup"><span data-stu-id="21ca5-124">You can trigger a model build once you have valuable usage data.</span></span>
* <span data-ttu-id="21ca5-125"><ins>A javaslat</ins> -API-k, amelyek lehetővé teszik tooconsume javaslatok hello build-modell leteltével.</span><span class="sxs-lookup"><span data-stu-id="21ca5-125"><ins>Recommendation</ins> - APIs that enable you tooconsume recommendations once hello build of a model ends.</span></span>
* <span data-ttu-id="21ca5-126"><ins>Felhasználói adatok</ins> -API-k, amelyek lehetővé teszik hello felhasználói használati adatok toofetch információkat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-126"><ins>User Data</ins> - APIs that enable you toofetch information on hello user usage data.</span></span>
* <span data-ttu-id="21ca5-127"><ins>Értesítések</ins> -API-k, amelyek lehetővé teszik a problémák tooreceive értesítések kapcsolatos tooyour API-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-127"><ins>Notifications</ins> - APIs that enable you tooreceive notifications on problems related tooyour API operations.</span></span> <span data-ttu-id="21ca5-128">(Például a használati adatok adatgyűjtést és a legtöbb hello események feldolgozása sikertelen jelentik.</span><span class="sxs-lookup"><span data-stu-id="21ca5-128">(For example, you are reporting usage data via Data Acquisition and most of hello events processing are failing.</span></span> <span data-ttu-id="21ca5-129">Egy hiba értesítést generál.)</span><span class="sxs-lookup"><span data-stu-id="21ca5-129">An error notification will be raised.)</span></span>

## <a name="2-limitations"></a><span data-ttu-id="21ca5-130">2. Korlátozások</span><span class="sxs-lookup"><span data-stu-id="21ca5-130">2. Limitations</span></span>
* <span data-ttu-id="21ca5-131">előfizetésenként modellek hello maximális száma érték a 10.</span><span class="sxs-lookup"><span data-stu-id="21ca5-131">hello maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="21ca5-132">egyes modellek buildek hello legfeljebb 20.</span><span class="sxs-lookup"><span data-stu-id="21ca5-132">hello maximum number of builds per model is 20.</span></span>
* <span data-ttu-id="21ca5-133">a katalógus rendelkező elemek maximális számát hello 100 000.</span><span class="sxs-lookup"><span data-stu-id="21ca5-133">hello maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="21ca5-134">mindig használati pontok maximális számát hello ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="21ca5-134">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="21ca5-135">Ha újakat feltöltött vagy fogja jelenteni a legrégebbi hello törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="21ca5-135">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="21ca5-136">hello maximális a FELADÁS egy vagy több (pl. katalógus adatokat importálhat, használati adatok importálása) elküldött adatok mérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="21ca5-136">hello maximum size of data that can be sent in POST (e.g. import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="21ca5-137">hello maximális elemek száma, amelyek is meg kell adnia a javaslatok beolvasásakor 150.</span><span class="sxs-lookup"><span data-stu-id="21ca5-137">hello maximum number of items that can be asked for when getting recommendations is 150.</span></span>

## <a name="3-apis---general-information"></a><span data-ttu-id="21ca5-138">3. API-k – általános információk</span><span class="sxs-lookup"><span data-stu-id="21ca5-138">3. APIs - general information</span></span>
### <a name="31-authentication"></a><span data-ttu-id="21ca5-139">3.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-139">3.1.</span></span> <span data-ttu-id="21ca5-140">Authentication</span><span class="sxs-lookup"><span data-stu-id="21ca5-140">Authentication</span></span>
<span data-ttu-id="21ca5-141">Kövesse az kapcsolatos hitelesítési hello Microsoft Azure piactérről útmutatások.</span><span class="sxs-lookup"><span data-stu-id="21ca5-141">Please follow hello Microsoft Azure Marketplace guidelines regarding authentication.</span></span> <span data-ttu-id="21ca5-142">hello piactér vagy hello Basic vagy az OAuth hitelesítési módszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-142">hello marketplace supports either hello Basic or OAuth authentication method.</span></span>

### <a name="32-service-uri"></a><span data-ttu-id="21ca5-143">3.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-143">3.2.</span></span> <span data-ttu-id="21ca5-144">Szolgáltatás URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-144">Service URI</span></span>
<span data-ttu-id="21ca5-145">hello szolgáltatás gyökér URI hello Azure Machine Learning javaslatok API-k [itt.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span><span class="sxs-lookup"><span data-stu-id="21ca5-145">hello service root URI for hello Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span></span>

<span data-ttu-id="21ca5-146">hello teljes szolgáltatás URI kifejezett elemek hello OData specifikáció használatával.</span><span class="sxs-lookup"><span data-stu-id="21ca5-146">hello full service URI is expressed using elements of hello OData specification.</span></span>  

### <a name="33-api-version"></a><span data-ttu-id="21ca5-147">3.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-147">3.3.</span></span> <span data-ttu-id="21ca5-148">API-verzió</span><span class="sxs-lookup"><span data-stu-id="21ca5-148">API version</span></span>
<span data-ttu-id="21ca5-149">Minden API-hívás fog működni, hello végén apiVersion too1.0 be kell állítania egy lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="21ca5-149">Each API call will have, at hello end, a query parameter called apiVersion that should be set too1.0.</span></span>

### <a name="34-ids-are-case-sensitive"></a><span data-ttu-id="21ca5-150">3.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-150">3.4.</span></span> <span data-ttu-id="21ca5-151">Azonosítók különbözőnek számítanak a kis</span><span class="sxs-lookup"><span data-stu-id="21ca5-151">IDs are case sensitive</span></span>
<span data-ttu-id="21ca5-152">Azonosítók, bármelyik hello API-k, visszaadott különbözőnek számítanak a kis és kell használni, így amikor későbbi API-hívásokban paraméterként.</span><span class="sxs-lookup"><span data-stu-id="21ca5-152">IDs, returned by any of hello APIs, are case sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="21ca5-153">Például modellazonosítóját és a katalógus azonosítók különbözőnek számítanak a kis.</span><span class="sxs-lookup"><span data-stu-id="21ca5-153">For instance, model IDs and catalog IDs are case sensitive.</span></span>

## <a name="4-recommendations-quality-and-cold-items"></a><span data-ttu-id="21ca5-154">4. Javaslatok minőségének és Cold elemek</span><span class="sxs-lookup"><span data-stu-id="21ca5-154">4. Recommendations Quality and Cold Items</span></span>
### <a name="41-recommendation-quality"></a><span data-ttu-id="21ca5-155">4.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-155">4.1.</span></span> <span data-ttu-id="21ca5-156">A javaslat minősége</span><span class="sxs-lookup"><span data-stu-id="21ca5-156">Recommendation quality</span></span>
<span data-ttu-id="21ca5-157">A javaslat modellek létrehozása az általában elegendő tooallow hello rendszer tooprovide javaslatok.</span><span class="sxs-lookup"><span data-stu-id="21ca5-157">Creating a recommendation model is usually enough tooallow hello system tooprovide recommendations.</span></span> <span data-ttu-id="21ca5-158">Ettől függetlenül javaslat minőségének feldolgozott hello használat alapján változik, és hello hello katalógus körét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-158">Nevertheless, recommendation quality varies based on hello usage processed and hello coverage of hello catalog.</span></span> <span data-ttu-id="21ca5-159">Például ha sok cold elem (elem jelentős használata nélkül), hello rendszer van-e egy ilyen elem ajánlást megadása, vagy egy ilyen elem használata javasolt egy nehézségek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-159">For example if you have a lot of cold items (items without significant usage), hello system will have difficulties providing a recommendation for such an item or using such an item as a recommended one.</span></span> <span data-ttu-id="21ca5-160">A sorrend tooovercome hello cold elem probléma hello rendszer lehetővé teszi a metaadatok hello elemek tooenhance hello ajánlások hello használatát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-160">In order tooovercome hello cold item problem, hello system allows hello use of metadata of hello items tooenhance hello recommendations.</span></span> <span data-ttu-id="21ca5-161">A metaadatok hivatkozott tooas szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="21ca5-161">This metadata is referred tooas features.</span></span> <span data-ttu-id="21ca5-162">Tipikus szolgáltatásokat egy könyv szerző vagy egy movie szereplő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-162">Typical features are a book's author or a movie's actor.</span></span> <span data-ttu-id="21ca5-163">Kulcs/érték karakterláncok hello formában hello katalógus keresztül szolgáltatást kínál.</span><span class="sxs-lookup"><span data-stu-id="21ca5-163">Features are provided via hello catalog in hello form of key/value strings.</span></span> <span data-ttu-id="21ca5-164">A hello teljes formázás hello katalógusfájl, tekintse meg az toohello [katalógus szakasz importálása](#81-import-catalog-data).</span><span class="sxs-lookup"><span data-stu-id="21ca5-164">For hello full format of hello catalog file, please refer toohello [import catalog section](#81-import-catalog-data).</span></span> 

### <a name="42-rank-build"></a><span data-ttu-id="21ca5-165">4.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-165">4.2.</span></span> <span data-ttu-id="21ca5-166">Rangsorolt build</span><span class="sxs-lookup"><span data-stu-id="21ca5-166">Rank build</span></span>
<span data-ttu-id="21ca5-167">Szolgáltatások fokozott hello javaslat modell, de toodo megkövetelik jelentéssel bíró funkciók hello használatát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-167">Features can enhance hello recommendation model, but toodo so requires hello use of meaningful features.</span></span> <span data-ttu-id="21ca5-168">Erre a célra egy új build bevezetett - build a sorrend első helyén.</span><span class="sxs-lookup"><span data-stu-id="21ca5-168">For this purpose a new build was introduced - a rank build.</span></span> <span data-ttu-id="21ca5-169">A build fog rangsorolja hello hasznosságát funkcióját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-169">This build will rank hello usefulness of features.</span></span> <span data-ttu-id="21ca5-170">Egy olyan jelentéssel bíró szolgáltatása, 2, illetve a hierarchiában felfelé a sorrendet megadó pontszám szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="21ca5-170">A meaningful feature is a feature with a rank score of 2 and up.</span></span>
<span data-ttu-id="21ca5-171">Után ismertetése hello szolgáltatást ezek jelentéssel bíró, hello lista (vagy allista) jelentéssel bíró funkciók javaslat build kiváltani.</span><span class="sxs-lookup"><span data-stu-id="21ca5-171">After understanding which of hello features are meaningful, trigger a recommendation build with hello list (or sublist) of meaningful features.</span></span> <span data-ttu-id="21ca5-172">Ezek hello fejlesztésen esett át a meleg elemek és a cold elemek szolgáltatásainak lehetséges toouse.</span><span class="sxs-lookup"><span data-stu-id="21ca5-172">It is possible toouse these feature for hello enhancement of both warm items and cold items.</span></span> <span data-ttu-id="21ca5-173">A sorrend toouse őket meleg elemek hello `UseFeatureInModel` build paramétert kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="21ca5-173">In order toouse them for warm items, hello `UseFeatureInModel` build parameter should be set up.</span></span> <span data-ttu-id="21ca5-174">Rendelés toouse funkciói cold elemekhez, hello `AllowColdItemPlacement` build paramétert engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-174">In order toouse features for cold items, hello `AllowColdItemPlacement` build parameter should be enabled.</span></span>
<span data-ttu-id="21ca5-175">Megjegyzés: Nincs lehetséges tooenable `AllowColdItemPlacement` engedélyezése nélkül `UseFeatureInModel`.</span><span class="sxs-lookup"><span data-stu-id="21ca5-175">Note: It is not possible tooenable `AllowColdItemPlacement` without enabling `UseFeatureInModel`.</span></span>

### <a name="43-recommendation-reasoning"></a><span data-ttu-id="21ca5-176">4.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-176">4.3.</span></span> <span data-ttu-id="21ca5-177">A javaslat indoklást</span><span class="sxs-lookup"><span data-stu-id="21ca5-177">Recommendation reasoning</span></span>
<span data-ttu-id="21ca5-178">A javaslat indoklást funkciók használata szerepet játszó másik tényező.</span><span class="sxs-lookup"><span data-stu-id="21ca5-178">Recommendation reasoning is another aspect of feature usage.</span></span> <span data-ttu-id="21ca5-179">Valóban hello Azure Machine Learning javaslatok motor használható szolgáltatások tooprovide javaslat magyarázatokat (más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-179">Indeed, hello Azure Machine Learning Recommendations engine can use features tooprovide recommendation explanations (a.k.a.</span></span> <span data-ttu-id="21ca5-180">mintafelismerési), ami toomore abban, hogy a hello, javasolt a elem a hello javaslat fogyasztói.</span><span class="sxs-lookup"><span data-stu-id="21ca5-180">reasoning), leading toomore confidence in hello recommended item from hello recommendation consumer.</span></span>
<span data-ttu-id="21ca5-181">tooenable mintafelismerési, hello `AllowFeatureCorrelation` és `ReasoningFeatureList` paraméterek telepítő előzetes toorequesting javaslat build kell lennie.</span><span class="sxs-lookup"><span data-stu-id="21ca5-181">tooenable reasoning, hello `AllowFeatureCorrelation` and `ReasoningFeatureList` parameters should be setup prior toorequesting a recommendation build.</span></span>

## <a name="5-model-basic"></a><span data-ttu-id="21ca5-182">5. Basic modell</span><span class="sxs-lookup"><span data-stu-id="21ca5-182">5. Model Basic</span></span>
### <a name="51-create-model"></a><span data-ttu-id="21ca5-183">5.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-183">5.1.</span></span> <span data-ttu-id="21ca5-184">Modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="21ca5-184">Create Model</span></span>
<span data-ttu-id="21ca5-185">Kérést hoz létre "létrehozása a modell".</span><span class="sxs-lookup"><span data-stu-id="21ca5-185">Creates a “create model” request.</span></span>

| <span data-ttu-id="21ca5-186">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-186">HTTP Method</span></span> | <span data-ttu-id="21ca5-187">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-187">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-188">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-188">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-189">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-189">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-190">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-190">Parameter Name</span></span> | <span data-ttu-id="21ca5-191">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-191">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-192">modelName</span><span class="sxs-lookup"><span data-stu-id="21ca5-192">modelName</span></span> |<span data-ttu-id="21ca5-193">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-193">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="21ca5-194">Maximális hossz: 20</span><span class="sxs-lookup"><span data-stu-id="21ca5-194">Max length: 20</span></span> |
| <span data-ttu-id="21ca5-195">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-195">apiVersion</span></span> |<span data-ttu-id="21ca5-196">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-196">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-197">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-197">Request Body</span></span> |<span data-ttu-id="21ca5-198">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-198">NONE</span></span> |

<span data-ttu-id="21ca5-199">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-199">**Response**:</span></span>

<span data-ttu-id="21ca5-200">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-200">HTTP Status code: 200</span></span>

* <span data-ttu-id="21ca5-201">`feed/entry/content/properties/id`-Hello modell eltérő azonosítót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-201">`feed/entry/content/properties/id` - Contains hello model ID.</span></span>
  <span data-ttu-id="21ca5-202">**Megjegyzés:**: Modellazonosító a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-202">**Note**: model ID is case sensitive.</span></span>

<span data-ttu-id="21ca5-203">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-203">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="52-get-model"></a><span data-ttu-id="21ca5-204">5.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-204">5.2.</span></span> <span data-ttu-id="21ca5-205">Modell lekérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-205">Get Model</span></span>
<span data-ttu-id="21ca5-206">A "get-modell" kérést hoz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-206">Creates a “get model” request.</span></span>

| <span data-ttu-id="21ca5-207">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-207">HTTP Method</span></span> | <span data-ttu-id="21ca5-208">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-208">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-209">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-209">GET</span></span> |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-210">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-210">Example:</span></span><br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-211">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-211">Parameter Name</span></span> | <span data-ttu-id="21ca5-212">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-212">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-213">id</span><span class="sxs-lookup"><span data-stu-id="21ca5-213">id</span></span> |<span data-ttu-id="21ca5-214">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-214">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="21ca5-215">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-215">apiVersion</span></span> |<span data-ttu-id="21ca5-216">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-216">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-217">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-217">Request Body</span></span> |<span data-ttu-id="21ca5-218">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-218">NONE</span></span> |

<span data-ttu-id="21ca5-219">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-219">**Response**:</span></span>

<span data-ttu-id="21ca5-220">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-220">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-221">hello a modell adatai a következő elemek hello alatt található:</span><span class="sxs-lookup"><span data-stu-id="21ca5-221">hello model data can be found under hello following elements:</span></span>

* <span data-ttu-id="21ca5-222">`feed/entry/content/properties/Id`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-222">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="21ca5-223">`feed/entry/content/properties/Name`-Modell neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-223">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="21ca5-224">`feed/entry/content/properties/Date`-Modell létrehozásának dátumát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-224">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="21ca5-225">`feed/entry/content/properties/Status`-Modell állapota.</span><span class="sxs-lookup"><span data-stu-id="21ca5-225">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="21ca5-226">Hello a következők egyike:</span><span class="sxs-lookup"><span data-stu-id="21ca5-226">One of hello following:</span></span>
  * <span data-ttu-id="21ca5-227">Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.</span><span class="sxs-lookup"><span data-stu-id="21ca5-227">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="21ca5-228">ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-228">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="21ca5-229">`feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha hello modell sikeresen lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-229">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="21ca5-230">`feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-230">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="21ca5-231">`feed/entry/content/properties/Mpr`-Modell átlagos PERCENTILIS rangsorolási (MPR - ModelInsight további információt talál).</span><span class="sxs-lookup"><span data-stu-id="21ca5-231">`feed/entry/content/properties/Mpr` - Model mean percentile ranking (MPR - see ModelInsight for more information).</span></span>
* <span data-ttu-id="21ca5-232">`feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-232">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>

<span data-ttu-id="21ca5-233">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-233">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a><span data-ttu-id="21ca5-234">5.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-234">5.3.</span></span>    <span data-ttu-id="21ca5-235">Összes modell lekérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-235">Get All Models</span></span>
<span data-ttu-id="21ca5-236">Lekéri az összes modellt a hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="21ca5-236">Retrieves all models of hello current user.</span></span>

| <span data-ttu-id="21ca5-237">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-237">HTTP Method</span></span> | <span data-ttu-id="21ca5-238">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-238">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-239">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-239">GET</span></span> |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-240">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-240">Example:</span></span><br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-241">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-241">Parameter Name</span></span> | <span data-ttu-id="21ca5-242">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-242">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-243">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-243">apiVersion</span></span> |<span data-ttu-id="21ca5-244">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-244">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-245">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-245">Request Body</span></span> |<span data-ttu-id="21ca5-246">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-246">NONE</span></span> |

<span data-ttu-id="21ca5-247">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-247">**Response**:</span></span>

<span data-ttu-id="21ca5-248">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-248">HTTP Status code: 200</span></span>

* <span data-ttu-id="21ca5-249">`feed/entry/content/properties/Id`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-249">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="21ca5-250">`feed/entry/content/properties/Name`-Modell neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-250">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="21ca5-251">`feed/entry/content/properties/Date`-Modell létrehozásának dátumát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-251">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="21ca5-252">`feed/entry/content/properties/Status`-Modell állapota.</span><span class="sxs-lookup"><span data-stu-id="21ca5-252">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="21ca5-253">Hello a következők egyike:</span><span class="sxs-lookup"><span data-stu-id="21ca5-253">One of hello following:</span></span>
  * <span data-ttu-id="21ca5-254">Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.</span><span class="sxs-lookup"><span data-stu-id="21ca5-254">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="21ca5-255">ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-255">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="21ca5-256">`feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha hello modell sikeresen lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-256">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="21ca5-257">`feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-257">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="21ca5-258">`feed/entry/content/properties/Mpr`-Modell MPR (lásd a ModelInsight olvashat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-258">`feed/entry/content/properties/Mpr` - Model MPR (see ModelInsight for more information).</span></span>
* <span data-ttu-id="21ca5-259">`feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-259">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>
* <span data-ttu-id="21ca5-260">`feed/entry/content/properties/UsageFileNames`-Modell fájlok vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-260">`feed/entry/content/properties/UsageFileNames` - List of model usage files separated by comma.</span></span>
* <span data-ttu-id="21ca5-261">`feed/entry/content/properties/CatalogId`-Modell katalógus azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-261">`feed/entry/content/properties/CatalogId` - Model catalog ID.</span></span>
* <span data-ttu-id="21ca5-262">`feed/entry/content/properties/Description`-Modell leírása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-262">`feed/entry/content/properties/Description` - Model description.</span></span>
* <span data-ttu-id="21ca5-263">`feed/entry/content/properties/CatalogFileName`-Fájl modell katalógusnév.</span><span class="sxs-lookup"><span data-stu-id="21ca5-263">`feed/entry/content/properties/CatalogFileName` - Model catalog file name.</span></span>

<span data-ttu-id="21ca5-264">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-264">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a><span data-ttu-id="21ca5-265">5.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-265">5.4.</span></span>    <span data-ttu-id="21ca5-266">Frissítési modell</span><span class="sxs-lookup"><span data-stu-id="21ca5-266">Update Model</span></span>
<span data-ttu-id="21ca5-267">Frissítés hello modell leírása, vagy hello aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-267">You can update hello model description or hello active build ID.</span></span><br><span data-ttu-id="21ca5-268">
<ins>Aktív build azonosító</ins> -minden build minden modell van állítva egy összeállítási.</span><span class="sxs-lookup"><span data-stu-id="21ca5-268">
<ins>Active build ID</ins> - Every build for every model has a build ID.</span></span> <span data-ttu-id="21ca5-269">hello aktív build azonosító: hello első sikeres build minden új modell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-269">hello active build ID is hello first successful build of every new model.</span></span> <span data-ttu-id="21ca5-270">Amennyiben van egy aktív build ID azonosítója és további változata hello teszi ugyanannak a modellnek van szüksége tooexplicitly állítsa be úgy, mint hello alapértelmezett build azonosító Ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="21ca5-270">Once you have an active build ID and you do additional builds for hello same model, you need tooexplicitly set it as hello default build ID if you want to.</span></span> <span data-ttu-id="21ca5-271">Ha felhasznált ajánlásokat, ha nem adja meg a hello build azonosítója, amelyet toouse, hello alapértelmezett egy automatikusan fog történni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-271">When you consume recommendations, if you do not specify hello build ID that you want toouse, hello default one will be used automatically.</span></span><br>
<span data-ttu-id="21ca5-272">Ez az eljárás lehetővé teszi - Miután javaslat modell éles - toobuild új modellek és tesztelését ahhoz, azok előlépteti tooproduction.</span><span class="sxs-lookup"><span data-stu-id="21ca5-272">This mechanism enables you - once you have a recommendation model in production - toobuild new models and test them before you promote them tooproduction.</span></span>

| <span data-ttu-id="21ca5-273">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-273">HTTP Method</span></span> | <span data-ttu-id="21ca5-274">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-275">A PUT</span><span class="sxs-lookup"><span data-stu-id="21ca5-275">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-276">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-276">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-277">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-277">Parameter Name</span></span> | <span data-ttu-id="21ca5-278">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-278">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-279">id</span><span class="sxs-lookup"><span data-stu-id="21ca5-279">id</span></span> |<span data-ttu-id="21ca5-280">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-280">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="21ca5-281">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-281">apiVersion</span></span> |<span data-ttu-id="21ca5-282">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-282">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-283">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-283">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br><span data-ttu-id="21ca5-284">Ne feledje, hogy hello XML-címkék leírás ActiveBuildId opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="21ca5-284">Note that hello XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="21ca5-285">Ha nem szeretné, hogy tooset leírás vagy ActiveBuildId, hello teljes címke eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-285">If you do not want tooset Description or ActiveBuildId, remove hello entire tag.</span></span> |

<span data-ttu-id="21ca5-286">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-286">**Response**:</span></span>

<span data-ttu-id="21ca5-287">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-287">HTTP Status code: 200</span></span>

### <a name="55----delete-model"></a><span data-ttu-id="21ca5-288">5.5.</span><span class="sxs-lookup"><span data-stu-id="21ca5-288">5.5.</span></span>    <span data-ttu-id="21ca5-289">Modell törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-289">Delete Model</span></span>
<span data-ttu-id="21ca5-290">Törli a meglévő modell által azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-290">Deletes an existing model by ID.</span></span>

| <span data-ttu-id="21ca5-291">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-291">HTTP Method</span></span> | <span data-ttu-id="21ca5-292">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-292">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-293">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-293">DELETE</span></span> |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-294">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-294">Example:</span></span><br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-295">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-295">Parameter Name</span></span> | <span data-ttu-id="21ca5-296">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-296">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-297">id</span><span class="sxs-lookup"><span data-stu-id="21ca5-297">id</span></span> |<span data-ttu-id="21ca5-298">Hello modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-298">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="21ca5-299">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-299">apiVersion</span></span> |<span data-ttu-id="21ca5-300">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-300">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-301">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-301">Request Body</span></span> |<span data-ttu-id="21ca5-302">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-302">NONE</span></span> |

<span data-ttu-id="21ca5-303">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-303">**Response**:</span></span>

<span data-ttu-id="21ca5-304">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-304">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-305">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-305">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a><span data-ttu-id="21ca5-306">6. Speciális modell</span><span class="sxs-lookup"><span data-stu-id="21ca5-306">6. Model Advanced</span></span>
### <a name="61----model-data-insight"></a><span data-ttu-id="21ca5-307">6.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-307">6.1.</span></span>    <span data-ttu-id="21ca5-308">Modell adatok felmérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-308">Model Data Insight</span></span>
<span data-ttu-id="21ca5-309">Ez a modell a következővel történt hello-használati adatokat statisztikai adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-309">Returns statistical data on hello usage data that this model was built with.</span></span>

<span data-ttu-id="21ca5-310">Csak a javaslat build esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="21ca5-310">Available only for Recommendation build.</span></span>

| <span data-ttu-id="21ca5-311">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-311">HTTP Method</span></span> | <span data-ttu-id="21ca5-312">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-312">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-313">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-313">GET</span></span> |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-314">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-314">Example:</span></span><br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-315">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-315">Parameter Name</span></span> | <span data-ttu-id="21ca5-316">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-316">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-317">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-317">modelId</span></span> |<span data-ttu-id="21ca5-318">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-318">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-319">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-319">apiVersion</span></span> |<span data-ttu-id="21ca5-320">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-320">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-321">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-321">Request Body</span></span> |<span data-ttu-id="21ca5-322">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-322">NONE</span></span> |

<span data-ttu-id="21ca5-323">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-323">**Response**:</span></span>

<span data-ttu-id="21ca5-324">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-324">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-325">hello adatok tulajdonságainak gyűjteményét adja vissza a rendszer.</span><span class="sxs-lookup"><span data-stu-id="21ca5-325">hello data is returned as a collection of properties.</span></span>

* <span data-ttu-id="21ca5-326">`feed/entry/id/content/properties/key`-Érvényes hello tulajdonságnév.</span><span class="sxs-lookup"><span data-stu-id="21ca5-326">`feed/entry/id/content/properties/key` - Holds hello property name.</span></span>
* <span data-ttu-id="21ca5-327">`feed/entry/id/content/properties/value`-Hello tulajdonság értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-327">`feed/entry/id/content/properties/value` - Holds hello property value.</span></span>

<span data-ttu-id="21ca5-328">az alábbi táblázat hello hello szám, amely minden kulcs ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-328">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="21ca5-329">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-329">Key</span></span> | <span data-ttu-id="21ca5-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-330">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-331">AvgItemLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-331">AvgItemLength</span></span> |<span data-ttu-id="21ca5-332">Elemenként egyedi felhasználók átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-332">Average number of distinct users per item.</span></span> |
| <span data-ttu-id="21ca5-333">AvgUserLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-333">AvgUserLength</span></span> |<span data-ttu-id="21ca5-334">Felhasználónként eltérő elemek átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-334">Average number of distinct items per user.</span></span> |
| <span data-ttu-id="21ca5-335">DensificationNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="21ca5-335">DensificationNumberOfItems</span></span> |<span data-ttu-id="21ca5-336">Cikkek követően nem kell modellezni törlési elemek száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-336">Number of items after pruning items that cannot be modelled.</span></span> |
| <span data-ttu-id="21ca5-337">DensificationNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="21ca5-337">DensificationNumberOfUsers</span></span> |<span data-ttu-id="21ca5-338">Használati pontok után törlési felhasználók és nem kell modellezni elemek száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-338">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="21ca5-339">DensificationNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="21ca5-339">DensificationNumberOfRecords</span></span> |<span data-ttu-id="21ca5-340">Használati pontok után törlési felhasználók és nem kell modellezni elemek száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-340">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="21ca5-341">MaxItemLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-341">MaxItemLength</span></span> |<span data-ttu-id="21ca5-342">Hello legnépszerűbb elem egyedi felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-342">Number of distinct users for hello most popular item.</span></span> |
| <span data-ttu-id="21ca5-343">MaxUserLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-343">MaxUserLength</span></span> |<span data-ttu-id="21ca5-344">Egy felhasználó különálló elemek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-344">Maximal number of distinct items for a user.</span></span> |
| <span data-ttu-id="21ca5-345">MinItemLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-345">MinItemLength</span></span> |<span data-ttu-id="21ca5-346">Egy elem egyedi felhasználók maximális száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-346">Maximal number of distinct users for an item.</span></span> |
| <span data-ttu-id="21ca5-347">MinUserLength</span><span class="sxs-lookup"><span data-stu-id="21ca5-347">MinUserLength</span></span> |<span data-ttu-id="21ca5-348">Egy felhasználó különálló elemek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-348">Minimal number of distinct items for a user.</span></span> |
| <span data-ttu-id="21ca5-349">RawNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="21ca5-349">RawNumberOfItems</span></span> |<span data-ttu-id="21ca5-350">Hello fájlok elemek száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-350">Number of items in hello usage files.</span></span> |
| <span data-ttu-id="21ca5-351">RawNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="21ca5-351">RawNumberOfUsers</span></span> |<span data-ttu-id="21ca5-352">Mielőtt bármely karbantartási eltávolítási használati pontok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-352">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="21ca5-353">RawNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="21ca5-353">RawNumberOfRecords</span></span> |<span data-ttu-id="21ca5-354">Mielőtt bármely karbantartási eltávolítási használati pontok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-354">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="21ca5-355">SamplingNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="21ca5-355">SamplingNumberOfItems</span></span> |<span data-ttu-id="21ca5-356">N/A</span><span class="sxs-lookup"><span data-stu-id="21ca5-356">N/A</span></span> |
| <span data-ttu-id="21ca5-357">SamplingNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="21ca5-357">SamplingNumberOfRecords</span></span> |<span data-ttu-id="21ca5-358">N/A</span><span class="sxs-lookup"><span data-stu-id="21ca5-358">N/A</span></span> |
| <span data-ttu-id="21ca5-359">SamplingNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="21ca5-359">SamplingNumberOfUsers</span></span> |<span data-ttu-id="21ca5-360">N/A</span><span class="sxs-lookup"><span data-stu-id="21ca5-360">N/A</span></span> |

<span data-ttu-id="21ca5-361">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-361">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a><span data-ttu-id="21ca5-362">6.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-362">6.2.</span></span>    <span data-ttu-id="21ca5-363">Modell felmérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-363">Model Insight</span></span>
<span data-ttu-id="21ca5-364">Értéket ad vissza a insight hello aktív build a modell vagy (Ha a kapott) adott build.</span><span class="sxs-lookup"><span data-stu-id="21ca5-364">Returns model insight on hello active build or (if given) on a specific build.</span></span>

<span data-ttu-id="21ca5-365">Csak a javaslat build esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="21ca5-365">Available only for Recommendation build.</span></span>

| <span data-ttu-id="21ca5-366">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-366">HTTP Method</span></span> | <span data-ttu-id="21ca5-367">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-367">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-368">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-368">GET</span></span> |<span data-ttu-id="21ca5-369">Aktív build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="21ca5-369">With active build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-370">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-370">Example:</span></span><br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-371">Adott build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="21ca5-371">With specific build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-372">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-372">Parameter Name</span></span> | <span data-ttu-id="21ca5-373">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-373">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-374">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-374">modelId</span></span> |<span data-ttu-id="21ca5-375">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-375">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-376">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-376">buildId</span></span> |<span data-ttu-id="21ca5-377">Nem kötelező – sikeres build azonosító szám.</span><span class="sxs-lookup"><span data-stu-id="21ca5-377">Optional - number that identifies a successful build.</span></span> |
| <span data-ttu-id="21ca5-378">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-378">apiVersion</span></span> |<span data-ttu-id="21ca5-379">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-379">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-380">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-380">Request Body</span></span> |<span data-ttu-id="21ca5-381">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-381">NONE</span></span> |

<span data-ttu-id="21ca5-382">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-382">**Response**:</span></span>

<span data-ttu-id="21ca5-383">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-383">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-384">hello adatok tulajdonságainak gyűjteményét adja vissza a rendszer.</span><span class="sxs-lookup"><span data-stu-id="21ca5-384">hello data is returned as a collection of properties.</span></span>

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

<span data-ttu-id="21ca5-385">az alábbi táblázat hello hello szám, amely minden kulcs ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-385">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="21ca5-386">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-386">Key</span></span> | <span data-ttu-id="21ca5-387">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-387">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-388">CatalogCoverage</span><span class="sxs-lookup"><span data-stu-id="21ca5-388">CatalogCoverage</span></span> |<span data-ttu-id="21ca5-389">Hello katalógus részét modellezhető használati mintákat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-389">What part of hello catalog can be modelled with usage patterns.</span></span> <span data-ttu-id="21ca5-390">hello elemek hello többi tartalom-alapú szolgáltatásokat kell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-390">hello rest of hello items will need content-based features.</span></span> |
| <span data-ttu-id="21ca5-391">MPR-hez</span><span class="sxs-lookup"><span data-stu-id="21ca5-391">Mpr</span></span> |<span data-ttu-id="21ca5-392">Átlagos PERCENTILIS rangsorolási hello modell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-392">Mean percentile ranking of hello model.</span></span> <span data-ttu-id="21ca5-393">Jobb alsó.</span><span class="sxs-lookup"><span data-stu-id="21ca5-393">Lower is better.</span></span> |
| <span data-ttu-id="21ca5-394">NumberOfDimensions</span><span class="sxs-lookup"><span data-stu-id="21ca5-394">NumberOfDimensions</span></span> |<span data-ttu-id="21ca5-395">Hello mátrix factorization algoritmus által használt dimenziók száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-395">Number of dimensions used by hello matrix factorization algorithm.</span></span> |

<span data-ttu-id="21ca5-396">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-396">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a><span data-ttu-id="21ca5-397">6.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-397">6.3.</span></span>    <span data-ttu-id="21ca5-398">Modell minta beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-398">Get Model Sample</span></span>
<span data-ttu-id="21ca5-399">Egy minta hello javaslat modell lekérése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-399">Gets a sample of hello recommendation model.</span></span>

| <span data-ttu-id="21ca5-400">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-400">HTTP Method</span></span> | <span data-ttu-id="21ca5-401">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-401">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-402">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-402">GET</span></span> |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-403">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-403">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-404">Adott build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="21ca5-404">With specific build ID:</span></span><br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-405">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-405">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-406">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-406">Parameter Name</span></span> | <span data-ttu-id="21ca5-407">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-407">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-408">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-408">modelId</span></span> |<span data-ttu-id="21ca5-409">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-409">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-410">apiVersion</span></span> |<span data-ttu-id="21ca5-411">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-411">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-412">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-412">Request Body</span></span> |<span data-ttu-id="21ca5-413">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-413">NONE</span></span> |

<span data-ttu-id="21ca5-414">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-414">**Response**:</span></span>

<span data-ttu-id="21ca5-415">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-415">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-416">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-416">OData XML</span></span>

<span data-ttu-id="21ca5-417">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="21ca5-417">Response is returned in raw text format:</span></span>

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If hello Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of hello Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on hello Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in hello Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, hello Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On hello Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, hello Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories tooTell in hello Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, hello Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic hello Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in hello Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, hello Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, hello X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell hello President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In hello Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, hello Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of hello Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, hello Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in hello Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (hello Official Guide toohello X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, hello Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, hello Truth Is Out There (hello Official Guide toohello X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, hello Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of hello Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where hello Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, hello God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (hello Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in hello Time of Cholera (Penguin Great Books of hello 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, hello Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories tooTell In hello Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from hello Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a><span data-ttu-id="21ca5-418">7. Modell az üzleti szabályok</span><span class="sxs-lookup"><span data-stu-id="21ca5-418">7. Model Business Rules</span></span>
<span data-ttu-id="21ca5-419">Hello típusú támogatott szabályok a következők:</span><span class="sxs-lookup"><span data-stu-id="21ca5-419">These are hello types of rules supported:</span></span>

* <span data-ttu-id="21ca5-420"><strong>BlockList</strong> -BlockList lehetővé teszi a tooprovide nem szeretné, hogy hello javaslat eredmények tooreturn elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-420"><strong>BlockList</strong> - BlockList enables you tooprovide a list of items that you do not want tooreturn in hello recommendation results.</span></span> 
* <span data-ttu-id="21ca5-421"><strong>FeatureBlockList</strong> -funkció BlockList lehetővé teszi, hogy Ön tooblock elemek szolgáltatása hello értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="21ca5-421"><strong>FeatureBlockList</strong> - Feature BlockList enables you tooblock items based on hello values of its features.</span></span>

<span data-ttu-id="21ca5-422">*Ne küldjön 1000-nél több elemek egy egyetlen blocklist szabályban, vagy a hívás előfordulhat, hogy az időkorlát. Ha tooblock 1000-nél több elemet kell, hogy több blocklist hívások.*</span><span class="sxs-lookup"><span data-stu-id="21ca5-422">*Do not send more than 1000 items in a single blocklist rule or your call may timeout. If you need tooblock more than 1000 items, you can make several blocklist calls.*</span></span>

* <span data-ttu-id="21ca5-423"><strong>Upsale</strong> -Upsale lehetővé teszi a tooenforce elemek tooreturn hello javaslat eredmények között.</span><span class="sxs-lookup"><span data-stu-id="21ca5-423"><strong>Upsale</strong> - Upsale enables you tooenforce items tooreturn in hello recommendation results.</span></span>
* <span data-ttu-id="21ca5-424"><strong>Engedélyezési lista</strong> -fehér lista lehetővé teszi, hogy Ön tooonly javaslat elemek listájából javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-424"><strong>WhiteList</strong> - White List enables you tooonly suggest recommendations from a list of items.</span></span>
* <span data-ttu-id="21ca5-425"><strong>FeatureWhiteList</strong> -funkció fehér lista lehetővé teszi tooonly ajánlott elemek, amelyek adott termékfejlesztő-értékek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-425"><strong>FeatureWhiteList</strong> - Feature White List enables you tooonly recommend items that have specific feature values.</span></span>
* <span data-ttu-id="21ca5-426"><strong>PerSeedBlockList</strong> - / Seed tiltólista lehetővé teszi, hogy tooprovide egy konfigurációelem-javaslat eredményeként nem adható vissza elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-426"><strong>PerSeedBlockList</strong> - Per Seed Block List enables you tooprovide per item a list of items that cannot be returned as recommendation results.</span></span>

### <a name="71----get-model-rules"></a><span data-ttu-id="21ca5-427">7.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-427">7.1.</span></span>    <span data-ttu-id="21ca5-428">Modell szabályok lekérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-428">Get Model Rules</span></span>
| <span data-ttu-id="21ca5-429">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-429">HTTP Method</span></span> | <span data-ttu-id="21ca5-430">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-430">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-431">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-431">GET</span></span> |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="21ca5-432">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-432">Example:</span></span><br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-433">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-433">Parameter Name</span></span> | <span data-ttu-id="21ca5-434">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-434">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-435">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-435">modelId</span></span> |<span data-ttu-id="21ca5-436">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-436">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-437">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-437">apiVersion</span></span> |<span data-ttu-id="21ca5-438">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-438">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-439">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-439">Request Body</span></span> |<span data-ttu-id="21ca5-440">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-440">NONE</span></span> |

<span data-ttu-id="21ca5-441">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-441">**Response**:</span></span>

<span data-ttu-id="21ca5-442">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-442">HTTP Status code: 200</span></span>

* <span data-ttu-id="21ca5-443">`feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-443">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="21ca5-444">`feed/entry/content/properties/Type`-Hello szabály típusa.</span><span class="sxs-lookup"><span data-stu-id="21ca5-444">`feed/entry/content/properties/Type` - Type of hello rule.</span></span>
* <span data-ttu-id="21ca5-445">`feed/entry/content/properties/Parameter`-Szabály paraméter.</span><span class="sxs-lookup"><span data-stu-id="21ca5-445">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="21ca5-446">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-446">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a><span data-ttu-id="21ca5-447">7.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-447">7.2.</span></span>    <span data-ttu-id="21ca5-448">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21ca5-448">Add Rule</span></span>
| <span data-ttu-id="21ca5-449">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-449">HTTP Method</span></span> | <span data-ttu-id="21ca5-450">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-450">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-451">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-451">POST</span></span> |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| <span data-ttu-id="21ca5-452">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="21ca5-452">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="21ca5-453">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-453">Parameter Name</span></span> | <span data-ttu-id="21ca5-454">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-454">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-455">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-455">apiVersion</span></span> |<span data-ttu-id="21ca5-456">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-456">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-457">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-457">Request Body</span></span> | |

<span data-ttu-id="21ca5-458"><ins>Amikor biztosít azonosítónak olyan üzleti szabályok esetén, győződjön meg arról, hogy toouse hello hello elem külső azonosítója (hello ugyanazzal az azonosítóval hello katalógusfájl használt)</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-458"><ins>Whenever providing Item Ids for business rules, make sure toouse hello External Id of hello item (hello same Id that you used in hello catalog file)</ins></span></span><br><span data-ttu-id="21ca5-459">
<ins>tooadd BlockList szabály:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-459">
<ins>tooadd a BlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="21ca5-460"><ins>
<ins>tooadd FeatureBlockList szabály:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-460"><ins>
<ins>tooadd a FeatureBlockList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><span data-ttu-id="21ca5-461"><ins>tooadd egy Upsale szabályt:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-461"><ins> tooadd an Upsale rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br><span data-ttu-id="21ca5-462">
<ins>tooadd egy engedélyezési szabályt:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-462">
<ins>tooadd a WhiteList rule:</ins></span></span><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="21ca5-463"><ins>
<ins>tooadd FeatureWhiteList szabály:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-463"><ins>
<ins>tooadd a FeatureWhiteList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><span data-ttu-id="21ca5-464"><ins>tooadd PerSeedBlockList szabály:</ins></span><span class="sxs-lookup"><span data-stu-id="21ca5-464"><ins> tooadd a PerSeedBlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

<span data-ttu-id="21ca5-465">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-465">**Response**:</span></span>

<span data-ttu-id="21ca5-466">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-466">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-467">hello API újonnan létrehozott szabály a részletek hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-467">hello API returns hello newly created rule with its details.</span></span> <span data-ttu-id="21ca5-468">hello szabályok tulajdonság olvasható be a következő elérési utak hello:</span><span class="sxs-lookup"><span data-stu-id="21ca5-468">hello rules property can be retrieved from hello following paths:</span></span>

* <span data-ttu-id="21ca5-469">`feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-469">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="21ca5-470">`feed/entry/content/properties/Type`-Hello szabály típusa: BlockList vagy Upsale.</span><span class="sxs-lookup"><span data-stu-id="21ca5-470">`feed/entry/content/properties/Type` - Type of hello rule: BlockList or Upsale.</span></span>
* <span data-ttu-id="21ca5-471">`feed/entry/content/properties/Parameter`-Szabály paraméter.</span><span class="sxs-lookup"><span data-stu-id="21ca5-471">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="21ca5-472">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-472">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a><span data-ttu-id="21ca5-473">7.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-473">7.3.</span></span>    <span data-ttu-id="21ca5-474">A szabály törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-474">Delete Rule</span></span>
| <span data-ttu-id="21ca5-475">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-475">HTTP Method</span></span> | <span data-ttu-id="21ca5-476">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-476">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-477">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-477">DELETE</span></span> |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-478">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-478">Example:</span></span><br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-479">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-479">Parameter Name</span></span> | <span data-ttu-id="21ca5-480">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-480">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-481">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-481">modelId</span></span> |<span data-ttu-id="21ca5-482">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-482">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-483">filterId</span><span class="sxs-lookup"><span data-stu-id="21ca5-483">filterId</span></span> |<span data-ttu-id="21ca5-484">Hello szűrő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-484">Unique identifier of hello filter</span></span> |
| <span data-ttu-id="21ca5-485">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-485">apiVersion</span></span> |<span data-ttu-id="21ca5-486">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-486">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-487">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-487">Request Body</span></span> |<span data-ttu-id="21ca5-488">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-488">NONE</span></span> |

<span data-ttu-id="21ca5-489">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-489">**Response**:</span></span>

<span data-ttu-id="21ca5-490">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-490">HTTP Status code: 200</span></span>

### <a name="74----delete-all-rules"></a><span data-ttu-id="21ca5-491">7.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-491">7.4.</span></span>    <span data-ttu-id="21ca5-492">A szabályok törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-492">Delete All Rules</span></span>
| <span data-ttu-id="21ca5-493">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-493">HTTP Method</span></span> | <span data-ttu-id="21ca5-494">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-495">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-495">DELETE</span></span> |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-496">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-496">Example:</span></span><br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-497">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-497">Parameter Name</span></span> | <span data-ttu-id="21ca5-498">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-499">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-499">modelId</span></span> |<span data-ttu-id="21ca5-500">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-501">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-501">apiVersion</span></span> |<span data-ttu-id="21ca5-502">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-502">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-503">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-503">Request Body</span></span> |<span data-ttu-id="21ca5-504">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-504">NONE</span></span> |

<span data-ttu-id="21ca5-505">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-505">**Response**:</span></span>

<span data-ttu-id="21ca5-506">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-506">HTTP Status code: 200</span></span>

## <a name="8-catalog"></a><span data-ttu-id="21ca5-507">8. Alkalmazáskatalógus</span><span class="sxs-lookup"><span data-stu-id="21ca5-507">8. Catalog</span></span>
### <a name="81----import-catalog-data"></a><span data-ttu-id="21ca5-508">8.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-508">8.1.</span></span>    <span data-ttu-id="21ca5-509">Eseménykatalógus-adatok importálása</span><span class="sxs-lookup"><span data-stu-id="21ca5-509">Import Catalog Data</span></span>
<span data-ttu-id="21ca5-510">Ha több katalógus fájlok toohello ugyanaz a modell több hívások tölt fel, azt csak hello új katalóguselemek szúrja be.</span><span class="sxs-lookup"><span data-stu-id="21ca5-510">If you upload several catalog files toohello same model with several calls, we will insert only hello new catalog items.</span></span> <span data-ttu-id="21ca5-511">A meglévő elemek hello eredeti értékekkel marad.</span><span class="sxs-lookup"><span data-stu-id="21ca5-511">Existing items will remain with hello original values.</span></span> <span data-ttu-id="21ca5-512">Eseménykatalógus-adatok a metódus nem frissíthető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-512">You cannot update catalog data by using this method.</span></span>

<span data-ttu-id="21ca5-513">hello eseménykatalógus-adatok hello a következő formátumot kell követnie:</span><span class="sxs-lookup"><span data-stu-id="21ca5-513">hello catalog data should follow hello following format:</span></span>

* <span data-ttu-id="21ca5-514">Szolgáltatások – nélkül`<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span><span class="sxs-lookup"><span data-stu-id="21ca5-514">Without features - `<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span></span>
* <span data-ttu-id="21ca5-515">-Szolgáltatásokkal`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span><span class="sxs-lookup"><span data-stu-id="21ca5-515">With features - `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span></span>

<span data-ttu-id="21ca5-516">Megjegyzés: hello maximális mérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="21ca5-516">Note: hello maximum file size is 200MB.</span></span>

<span data-ttu-id="21ca5-517">** Részletek formázása **</span><span class="sxs-lookup"><span data-stu-id="21ca5-517">** Format details **</span></span>

| <span data-ttu-id="21ca5-518">Név</span><span class="sxs-lookup"><span data-stu-id="21ca5-518">Name</span></span> | <span data-ttu-id="21ca5-519">Kötelező</span><span class="sxs-lookup"><span data-stu-id="21ca5-519">Mandatory</span></span> | <span data-ttu-id="21ca5-520">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-520">Type</span></span> | <span data-ttu-id="21ca5-521">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-521">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="21ca5-522">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-522">Item Id</span></span> |<span data-ttu-id="21ca5-523">Igen</span><span class="sxs-lookup"><span data-stu-id="21ca5-523">Yes</span></span> |<span data-ttu-id="21ca5-524">[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="21ca5-524">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="21ca5-525">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-525">Max length: 50</span></span> |<span data-ttu-id="21ca5-526">Egy elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-526">Unique identifier of an item.</span></span> |
| <span data-ttu-id="21ca5-527">Elem neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-527">Item Name</span></span> |<span data-ttu-id="21ca5-528">Igen</span><span class="sxs-lookup"><span data-stu-id="21ca5-528">Yes</span></span> |<span data-ttu-id="21ca5-529">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-529">Any alphanumeric characters</span></span><br> <span data-ttu-id="21ca5-530">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="21ca5-530">Max length: 255</span></span> |<span data-ttu-id="21ca5-531">Elem neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-531">Item name.</span></span> |
| <span data-ttu-id="21ca5-532">Elem kategória</span><span class="sxs-lookup"><span data-stu-id="21ca5-532">Item Category</span></span> |<span data-ttu-id="21ca5-533">Igen</span><span class="sxs-lookup"><span data-stu-id="21ca5-533">Yes</span></span> |<span data-ttu-id="21ca5-534">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-534">Any alphanumeric characters</span></span> <br> <span data-ttu-id="21ca5-535">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="21ca5-535">Max length: 255</span></span> |<span data-ttu-id="21ca5-536">Kategória toowhich Ez a cikk tartozik (pl. főzés könyvek, tragédiát...); üres is lehet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-536">Category toowhich this item belongs (e.g. Cooking Books, Drama…); can be empty.</span></span> |
| <span data-ttu-id="21ca5-537">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-537">Description</span></span> |<span data-ttu-id="21ca5-538">Nem, kivéve, ha a funkciók jelent-e (de üres lehet)</span><span class="sxs-lookup"><span data-stu-id="21ca5-538">No, unless features are present (but can be empty)</span></span> |<span data-ttu-id="21ca5-539">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-539">Any alphanumeric characters</span></span> <br> <span data-ttu-id="21ca5-540">Maximális hossz: 4000</span><span class="sxs-lookup"><span data-stu-id="21ca5-540">Max length: 4000</span></span> |<span data-ttu-id="21ca5-541">Ez az elem leírása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-541">Description of this item.</span></span> |
| <span data-ttu-id="21ca5-542">Szolgáltatások listája</span><span class="sxs-lookup"><span data-stu-id="21ca5-542">Features list</span></span> |<span data-ttu-id="21ca5-543">Nem</span><span class="sxs-lookup"><span data-stu-id="21ca5-543">No</span></span> |<span data-ttu-id="21ca5-544">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-544">Any alphanumeric characters</span></span> <br> <span data-ttu-id="21ca5-545">Maximális hossz: 4000; Szolgáltatások: 20 maximális száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-545">Max length: 4000; Max number of features:20</span></span> |<span data-ttu-id="21ca5-546">Szolgáltatás neve vesszővel elválasztott listája = szolgáltatás érték, amely használt tooenhance modell javaslat; Lásd: [témakörök speciális](#2-advanced-topics) szakasz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-546">Comma-separated list of feature name=feature value that can be used tooenhance model recommendation; see [Advanced topics](#2-advanced-topics) section.</span></span> |

| <span data-ttu-id="21ca5-547">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-547">HTTP Method</span></span> | <span data-ttu-id="21ca5-548">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-548">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-549">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-549">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-550">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-550">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| <span data-ttu-id="21ca5-551">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="21ca5-551">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="21ca5-552">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-552">Parameter Name</span></span> | <span data-ttu-id="21ca5-553">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-553">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-554">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-554">modelId</span></span> |<span data-ttu-id="21ca5-555">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-555">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-556">fájlnév</span><span class="sxs-lookup"><span data-stu-id="21ca5-556">filename</span></span> |<span data-ttu-id="21ca5-557">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-557">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="21ca5-558">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-558">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="21ca5-559">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-559">Max length: 50</span></span> |
| <span data-ttu-id="21ca5-560">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-560">apiVersion</span></span> |<span data-ttu-id="21ca5-561">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-561">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-562">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-562">Request Body</span></span> |<span data-ttu-id="21ca5-563">(A szolgáltatások) például:</span><span class="sxs-lookup"><span data-stu-id="21ca5-563">Example (with features):</span></span><br/><span data-ttu-id="21ca5-564">2406e770-769c-4189-89de-1c9283f93a96 Clara Callan, megjelenik a hello könyv leírása, szerzői = Richard Wright, közzétevő = Harper Flamingo Kanada, év = 2001</span><span class="sxs-lookup"><span data-stu-id="21ca5-564">2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book,hello book  description,author=Richard Wright,publisher=Harper Flamingo Canada,year=2001</span></span><br><span data-ttu-id="21ca5-565">21bf8088-b6c0-4509-870c-e1c7ac78304a hello Forgetting helyiségben: A példa (Byzantium könyv), a könyv,, szerzői = Nick Bantock, közzétevő = Harpercollins, év = 1997</span><span class="sxs-lookup"><span data-stu-id="21ca5-565">21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book,,author=Nick Bantock,publisher=Harpercollins,year=1997</span></span><br><span data-ttu-id="21ca5-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23 Spadework, könyv,, szerzői = komócsin Findley, közzétevő = HarperFlamingo Kanada év = 2001</span><span class="sxs-lookup"><span data-stu-id="21ca5-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book,,author=Timothy Findley, publisher=HarperFlamingo Canada, year=2001</span></span><br><span data-ttu-id="21ca5-567">552a1940-21e4-4399-82bb-594b46d7ed54 korlátozás a vadállatok, megjelenik a hello könyv leírása, szerzői = Magnus Mills, közzétevő = Arcade közzétételi év = 1998</span><span class="sxs-lookup"><span data-stu-id="21ca5-567">552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book,hello book description,author=Magnus Mills, publisher=Arcade Publishing, year=1998</span></span></pre> |

<span data-ttu-id="21ca5-568">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-568">**Response**:</span></span>

<span data-ttu-id="21ca5-569">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-569">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-570">hello API hello importálási jelentést adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-570">hello API returns a report of hello import.</span></span>

* <span data-ttu-id="21ca5-571">`feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-571">`feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="21ca5-572">`feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-572">`feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>

<span data-ttu-id="21ca5-573">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-573">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a><span data-ttu-id="21ca5-574">8.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-574">8.2.</span></span>    <span data-ttu-id="21ca5-575">Katalógus beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-575">Get Catalog</span></span>
<span data-ttu-id="21ca5-576">Lekéri az összes katalóguselemeket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-576">Retrieves all catalog items.</span></span>
<span data-ttu-id="21ca5-577">hello katalógus lesz egyszerre egy lap lekérése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-577">hello catalog will be retrieved one page at a time.</span></span> <span data-ttu-id="21ca5-578">Ha azt szeretné, hogy egy adott indexű tooget elemek, hello $skip odata paraméter használható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-578">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="21ca5-579">Például ha azt szeretné, hogy a 100 pozíciótól kezdődően tooget elemek, adja hozzá a hello paramétert $skip = 100 toohello kérelmet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-579">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="21ca5-580">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-580">HTTP Method</span></span> | <span data-ttu-id="21ca5-581">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-581">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-582">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-582">GET</span></span> |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-583">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-583">Example:</span></span><br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-584">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-584">Parameter Name</span></span> | <span data-ttu-id="21ca5-585">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-585">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-586">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-586">modelId</span></span> |<span data-ttu-id="21ca5-587">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-587">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-588">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-588">apiVersion</span></span> |<span data-ttu-id="21ca5-589">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-589">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-590">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-590">Request Body</span></span> |<span data-ttu-id="21ca5-591">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-591">NONE</span></span> |

<span data-ttu-id="21ca5-592">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-592">**Response**:</span></span>

<span data-ttu-id="21ca5-593">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-593">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-594">hello válasz tartalmazza az elemet egy tételt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-594">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="21ca5-595">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-595">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-596">`feed/entry/content/properties/ExternalId`-Katalógus külső Cikkazonosító, egy olyan hello ügyfél hello.</span><span class="sxs-lookup"><span data-stu-id="21ca5-596">`feed/entry/content/properties/ExternalId` - Catalog item external ID, hello one provided by hello customer.</span></span>
* <span data-ttu-id="21ca5-597">`feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, egy, az Azure Machine Learning javaslatok generált hello.</span><span class="sxs-lookup"><span data-stu-id="21ca5-597">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="21ca5-598">`feed/entry/content/properties/Name`-Katalógus elem neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-598">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="21ca5-599">`feed/entry/content/properties/Category`-Katalógus cikk kategóriát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-599">`feed/entry/content/properties/Category` - Catalog item category.</span></span>
* <span data-ttu-id="21ca5-600">`feed/entry/content/properties/Description`-Katalógus leírása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-600">`feed/entry/content/properties/Description` - Catalog item description.</span></span>
* <span data-ttu-id="21ca5-601">`feed/entry/content/properties/Metadata`-Katalógus elem metaadatait.</span><span class="sxs-lookup"><span data-stu-id="21ca5-601">`feed/entry/content/properties/Metadata` - Catalog item metadata.</span></span>

<span data-ttu-id="21ca5-602">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-602">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">hello Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a><span data-ttu-id="21ca5-603">8.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-603">8.3.</span></span>    <span data-ttu-id="21ca5-604">Beolvasni a Katalóguselemeket a Token által</span><span class="sxs-lookup"><span data-stu-id="21ca5-604">Get Catalog Items by Token</span></span>
| <span data-ttu-id="21ca5-605">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-605">HTTP Method</span></span> | <span data-ttu-id="21ca5-606">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-606">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-607">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-607">GET</span></span> |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-608">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-608">Example:</span></span><br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-609">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-609">Parameter Name</span></span> | <span data-ttu-id="21ca5-610">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-610">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-611">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-611">modelId</span></span> |<span data-ttu-id="21ca5-612">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-612">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-613">Token</span><span class="sxs-lookup"><span data-stu-id="21ca5-613">token</span></span> |<span data-ttu-id="21ca5-614">Token hello katalógus elem neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-614">Token of hello catalog item’s name.</span></span> <span data-ttu-id="21ca5-615">Legalább 3 karaktert kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="21ca5-615">Should contain at least 3 characters.</span></span> |
| <span data-ttu-id="21ca5-616">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-616">apiVersion</span></span> |<span data-ttu-id="21ca5-617">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-617">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-618">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-618">Request Body</span></span> |<span data-ttu-id="21ca5-619">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-619">NONE</span></span> |

<span data-ttu-id="21ca5-620">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-620">**Response**:</span></span>

<span data-ttu-id="21ca5-621">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-621">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-622">hello válasz tartalmazza az elemet egy tételt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-622">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="21ca5-623">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-623">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-624">`feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, egy, az Azure Machine Learning javaslatok generált hello.</span><span class="sxs-lookup"><span data-stu-id="21ca5-624">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="21ca5-625">`feed/entry/content/properties/Name`-Katalógus elem neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-625">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="21ca5-626">`feed/entry/content/properties/Rating`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="21ca5-626">`feed/entry/content/properties/Rating` -  (for future use)</span></span>
* <span data-ttu-id="21ca5-627">`feed/entry/content/properties/Reasoning`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="21ca5-627">`feed/entry/content/properties/Reasoning` -  (for future use)</span></span>
* <span data-ttu-id="21ca5-628">`feed/entry/content/properties/Metadata`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="21ca5-628">`feed/entry/content/properties/Metadata` -  (for future use)</span></span>
* <span data-ttu-id="21ca5-629">`feed/entry/content/properties/FormattedRating`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="21ca5-629">`feed/entry/content/properties/FormattedRating` - (for future use)</span></span>

<span data-ttu-id="21ca5-630">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-630">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a><span data-ttu-id="21ca5-631">9. Használati adatok</span><span class="sxs-lookup"><span data-stu-id="21ca5-631">9. Usage data</span></span>
### <a name="91----import-usage-data"></a><span data-ttu-id="21ca5-632">9.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-632">9.1.</span></span>    <span data-ttu-id="21ca5-633">Használati adatok importálása</span><span class="sxs-lookup"><span data-stu-id="21ca5-633">Import Usage Data</span></span>
#### <a name="911-uploading-file"></a><span data-ttu-id="21ca5-634">9.1.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-634">9.1.1.</span></span> <span data-ttu-id="21ca5-635">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="21ca5-635">Uploading File</span></span>
<span data-ttu-id="21ca5-636">Ez a szakasz bemutatja, hogyan tooupload használati adatok egy fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="21ca5-636">This section shows how tooupload usage data by using a file.</span></span> <span data-ttu-id="21ca5-637">A használati adatokat tartalmazó többször API hívása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-637">You can call this API several times with usage data.</span></span> <span data-ttu-id="21ca5-638">Minden használati adatokat a rendszer minden hívások menti.</span><span class="sxs-lookup"><span data-stu-id="21ca5-638">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="21ca5-639">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-639">HTTP Method</span></span> | <span data-ttu-id="21ca5-640">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-640">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-641">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-641">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-642">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-642">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-643">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-643">Parameter Name</span></span> | <span data-ttu-id="21ca5-644">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-644">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-645">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-645">modelId</span></span> |<span data-ttu-id="21ca5-646">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-646">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-647">fájlnév</span><span class="sxs-lookup"><span data-stu-id="21ca5-647">filename</span></span> |<span data-ttu-id="21ca5-648">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-648">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="21ca5-649">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-649">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="21ca5-650">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-650">Max length: 50</span></span> |
| <span data-ttu-id="21ca5-651">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-651">apiVersion</span></span> |<span data-ttu-id="21ca5-652">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-652">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-653">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-653">Request Body</span></span> |<span data-ttu-id="21ca5-654">Használati adatok.</span><span class="sxs-lookup"><span data-stu-id="21ca5-654">Usage data.</span></span> <span data-ttu-id="21ca5-655">Formátum:</span><span class="sxs-lookup"><span data-stu-id="21ca5-655">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="21ca5-656">Név</span><span class="sxs-lookup"><span data-stu-id="21ca5-656">Name</span></span></th><th><span data-ttu-id="21ca5-657">Kötelező</span><span class="sxs-lookup"><span data-stu-id="21ca5-657">Mandatory</span></span></th><th><span data-ttu-id="21ca5-658">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-658">Type</span></span></th><th><span data-ttu-id="21ca5-659">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-659">Description</span></span></th></tr><tr><td><span data-ttu-id="21ca5-660">Felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="21ca5-660">User Id</span></span></td><td><span data-ttu-id="21ca5-661">Igen</span><span class="sxs-lookup"><span data-stu-id="21ca5-661">Yes</span></span></td><td><span data-ttu-id="21ca5-662">[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="21ca5-662">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="21ca5-663">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="21ca5-663">Max length: 255</span></span> </td><td><span data-ttu-id="21ca5-664">A felhasználó egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-664">Unique identifier of a user.</span></span></td></tr><tr><td><span data-ttu-id="21ca5-665">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-665">Item Id</span></span></td><td><span data-ttu-id="21ca5-666">Igen</span><span class="sxs-lookup"><span data-stu-id="21ca5-666">Yes</span></span></td><td><span data-ttu-id="21ca5-667">[A-z], [a-z], [0-9], [&#95;] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="21ca5-667">[A-z], [a-z], [0-9], [&#95;] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="21ca5-668">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-668">Max length: 50</span></span></td><td><span data-ttu-id="21ca5-669">Egy elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-669">Unique identifier of an item.</span></span></td></tr><tr><td><span data-ttu-id="21ca5-670">Time</span><span class="sxs-lookup"><span data-stu-id="21ca5-670">Time</span></span></td><td><span data-ttu-id="21ca5-671">Nem</span><span class="sxs-lookup"><span data-stu-id="21ca5-671">No</span></span></td><td><span data-ttu-id="21ca5-672">Dátum formátumban: éééé/hh/nnTóó: pp: (pl. 2013 06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="21ca5-672">Date in format: YYYY/MM/DDTHH:MM:SS (e.g. 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="21ca5-673">Az adatok időpontja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-673">Time of data.</span></span></td></tr><tr><td><span data-ttu-id="21ca5-674">Esemény</span><span class="sxs-lookup"><span data-stu-id="21ca5-674">Event</span></span></td><td><span data-ttu-id="21ca5-675">Nincs; Ha a megadott dátum is kell állítani</span><span class="sxs-lookup"><span data-stu-id="21ca5-675">No; if supplied then must also put date</span></span></td><td><span data-ttu-id="21ca5-676">Hello a következők egyike:</span><span class="sxs-lookup"><span data-stu-id="21ca5-676">One of hello following:</span></span><br><span data-ttu-id="21ca5-677">• Kattintson</span><span class="sxs-lookup"><span data-stu-id="21ca5-677">• Click</span></span><br><span data-ttu-id="21ca5-678">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="21ca5-678">• RecommendationClick</span></span><br><span data-ttu-id="21ca5-679">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="21ca5-679">•    AddShopCart</span></span><br><span data-ttu-id="21ca5-680">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="21ca5-680">• RemoveShopCart</span></span><br><span data-ttu-id="21ca5-681">• Beszerzési</span><span class="sxs-lookup"><span data-stu-id="21ca5-681">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="21ca5-682">Maximális fájlméret: 200MB</span><span class="sxs-lookup"><span data-stu-id="21ca5-682">Maximum file size: 200MB</span></span><br><br><span data-ttu-id="21ca5-683">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-683">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="21ca5-684">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-684">**Response**:</span></span>

<span data-ttu-id="21ca5-685">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-685">HTTP Status code: 200</span></span>

* <span data-ttu-id="21ca5-686">`Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-686">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="21ca5-687">`Feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-687">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>
* <span data-ttu-id="21ca5-688">`Feed\entry\content\properties\FileId`-Fájl azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-688">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="21ca5-689">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-689">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a><span data-ttu-id="21ca5-690">9.1.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-690">9.1.2.</span></span> <span data-ttu-id="21ca5-691">Adatgyűjtést használatával</span><span class="sxs-lookup"><span data-stu-id="21ca5-691">Using Data Acquisition</span></span>
<span data-ttu-id="21ca5-692">Ez a szakasz bemutatja, hogyan toosend események valós időben tooAzure Machine Learning ajánlásokat, általában a webhelyről.</span><span class="sxs-lookup"><span data-stu-id="21ca5-692">This section shows how toosend events in real time tooAzure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="21ca5-693">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-693">HTTP Method</span></span> | <span data-ttu-id="21ca5-694">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-694">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-695">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-695">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| <span data-ttu-id="21ca5-696">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="21ca5-696">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="21ca5-697">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-697">Parameter Name</span></span> | <span data-ttu-id="21ca5-698">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-698">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-699">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-699">apiVersion</span></span> |<span data-ttu-id="21ca5-700">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-700">1.0</span></span> |
| <span data-ttu-id="21ca5-701">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="21ca5-701">Request body</span></span> |<span data-ttu-id="21ca5-702">Esemény adatbevitel minden esemény toosend keresi.</span><span class="sxs-lookup"><span data-stu-id="21ca5-702">Event data entry for each event you want toosend.</span></span> <span data-ttu-id="21ca5-703">Küldje el a hello ugyanazon felhasználó vagy a böngésző-munkamenet hello hello munkamenet-azonosító mezőben ugyanazzal az Azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="21ca5-703">You should send for hello same user or browser session hello same ID in hello SessionId field.</span></span> <span data-ttu-id="21ca5-704">(Lásd az alábbi esemény törzsében mintáját.)</span><span class="sxs-lookup"><span data-stu-id="21ca5-704">(See sample of event body below.)</span></span> |

* <span data-ttu-id="21ca5-705">Példa a "Kattintson" esemény:</span><span class="sxs-lookup"><span data-stu-id="21ca5-705">Example for event 'Click':</span></span>
  
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
* <span data-ttu-id="21ca5-706">Példa a "RecommendationClick" esemény:</span><span class="sxs-lookup"><span data-stu-id="21ca5-706">Example for event 'RecommendationClick':</span></span>
  
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
* <span data-ttu-id="21ca5-707">Példa a "AddShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="21ca5-707">Example for event 'AddShopCart':</span></span>
  
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
* <span data-ttu-id="21ca5-708">Példa a "RemoveShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="21ca5-708">Example for event 'RemoveShopCart':</span></span>
  
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
* <span data-ttu-id="21ca5-709">Példa a "Vásárlás" esemény:</span><span class="sxs-lookup"><span data-stu-id="21ca5-709">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
            <EventData>
                <Name>Purchase</Name>
                <PurchaseItems>
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
                </PurchaseItems>
            </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="21ca5-710">Példa 2 események "kattintson a" és "AddShopCart" küldésekor:</span><span class="sxs-lookup"><span data-stu-id="21ca5-710">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
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

<span data-ttu-id="21ca5-711">**Válasz**: HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-711">**Response**: HTTP Status code: 200</span></span>

### <a name="92----list-model-usage-files"></a><span data-ttu-id="21ca5-712">9.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-712">9.2.</span></span>    <span data-ttu-id="21ca5-713">Modell használati fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="21ca5-713">List Model Usage Files</span></span>
<span data-ttu-id="21ca5-714">Lekéri az összes modell fájlok metaadatait.</span><span class="sxs-lookup"><span data-stu-id="21ca5-714">Retrieves metadata of all model usage files.</span></span>
<span data-ttu-id="21ca5-715">hello fájlok lesznek használati egy oldalt az attribútumböngészőben egyszerre beolvasott.</span><span class="sxs-lookup"><span data-stu-id="21ca5-715">hello usage files will be retrieved one page at a time.</span></span> <span data-ttu-id="21ca5-716">Minden lap containes 100 elemeket.</span><span class="sxs-lookup"><span data-stu-id="21ca5-716">Each page containes 100 items.</span></span> <span data-ttu-id="21ca5-717">Ha azt szeretné, hogy egy adott indexű tooget elemek, hello $skip odata paraméter használható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-717">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="21ca5-718">Például ha azt szeretné, hogy a 100 pozíciótól kezdődően tooget elemek, adja hozzá a hello paramétert $skip = 100 toohello kérelmet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-718">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="21ca5-719">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-719">HTTP Method</span></span> | <span data-ttu-id="21ca5-720">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-720">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-721">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-721">GET</span></span> |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-722">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-722">Example:</span></span><br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-723">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-723">Parameter Name</span></span> | <span data-ttu-id="21ca5-724">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-724">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-725">forModelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-725">forModelId</span></span> |<span data-ttu-id="21ca5-726">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-726">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-727">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-727">apiVersion</span></span> |<span data-ttu-id="21ca5-728">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-728">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-729">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-729">Request Body</span></span> |<span data-ttu-id="21ca5-730">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-730">NONE</span></span> |

<span data-ttu-id="21ca5-731">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-731">**Response**:</span></span>

<span data-ttu-id="21ca5-732">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-732">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-733">hello válasz használati fájlonként egy bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-733">hello response includes one entry per usage file.</span></span> <span data-ttu-id="21ca5-734">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-734">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-735">`feed\entry\content\properties\Id`-Használati azonosítót.</span><span class="sxs-lookup"><span data-stu-id="21ca5-735">`feed\entry\content\properties\Id` - Usage file ID.</span></span>
* <span data-ttu-id="21ca5-736">`feed\entry\content\properties\Length`-MB használat a fájl hosszát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-736">`feed\entry\content\properties\Length` - Usage file length in MB.</span></span>
* <span data-ttu-id="21ca5-737">`feed\entry\content\properties\DateModified`-Hello használati fájl létrehozásának dátuma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-737">`feed\entry\content\properties\DateModified` - Date when hello usage file was created.</span></span>
* <span data-ttu-id="21ca5-738">`feed\entry\content\properties\UseInModel`-E hello használati fájllal hello modellben.</span><span class="sxs-lookup"><span data-stu-id="21ca5-738">`feed\entry\content\properties\UseInModel` - Whether hello usage file is used in hello model.</span></span>

<span data-ttu-id="21ca5-739">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-739">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a><span data-ttu-id="21ca5-740">9.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-740">9.3.</span></span>    <span data-ttu-id="21ca5-741">Használati statisztikák beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-741">Get Usage Statistics</span></span>
<span data-ttu-id="21ca5-742">Lekéri a használati statisztikáit.</span><span class="sxs-lookup"><span data-stu-id="21ca5-742">Gets usage statistics.</span></span>

| <span data-ttu-id="21ca5-743">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-743">HTTP Method</span></span> | <span data-ttu-id="21ca5-744">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-744">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-745">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-745">GET</span></span> |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-746">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-746">Example:</span></span><br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-747">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-747">Parameter Name</span></span> | <span data-ttu-id="21ca5-748">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-748">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-749">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-749">modelId</span></span> |<span data-ttu-id="21ca5-750">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-750">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-751">A StartDate</span><span class="sxs-lookup"><span data-stu-id="21ca5-751">startDate</span></span> |<span data-ttu-id="21ca5-752">Kezdő dátum.</span><span class="sxs-lookup"><span data-stu-id="21ca5-752">Start date.</span></span> <span data-ttu-id="21ca5-753">Formátum: éééé/hh/nnTóó: pp:</span><span class="sxs-lookup"><span data-stu-id="21ca5-753">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="21ca5-754">endDate</span><span class="sxs-lookup"><span data-stu-id="21ca5-754">endDate</span></span> |<span data-ttu-id="21ca5-755">Záró dátum.</span><span class="sxs-lookup"><span data-stu-id="21ca5-755">End date.</span></span> <span data-ttu-id="21ca5-756">Formátum: éééé/hh/nnTóó: pp:</span><span class="sxs-lookup"><span data-stu-id="21ca5-756">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="21ca5-757">eventTypes</span><span class="sxs-lookup"><span data-stu-id="21ca5-757">eventTypes</span></span> |<span data-ttu-id="21ca5-758">Vesszővel elválasztott karakterlánc esemény meg kell adnia, vagy null értékű tooget összes esemény</span><span class="sxs-lookup"><span data-stu-id="21ca5-758">Comma-separated string of event types or null tooget all events</span></span> |
| <span data-ttu-id="21ca5-759">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-759">apiVersion</span></span> |<span data-ttu-id="21ca5-760">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-760">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-761">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-761">Request Body</span></span> |<span data-ttu-id="21ca5-762">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-762">NONE</span></span> |

<span data-ttu-id="21ca5-763">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-763">**Response**:</span></span>

<span data-ttu-id="21ca5-764">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-764">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-765">Kulcs/érték elemek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="21ca5-765">A collection of key/value elements.</span></span> <span data-ttu-id="21ca5-766">Mindegyik tartalmazza egy adott esemény típusa óránként csoportosítva eseményeket hello összege.</span><span class="sxs-lookup"><span data-stu-id="21ca5-766">Each one contains hello sum of events for a specific event type grouped by hour.</span></span>

* <span data-ttu-id="21ca5-767">`feed\entry[i]\content\properties\Key`-Tartalmaz hello ideje (óra szerint csoportosítva) és hello eseménytípus.</span><span class="sxs-lookup"><span data-stu-id="21ca5-767">`feed\entry[i]\content\properties\Key` - Contains hello time (grouped by hour) and hello event type.</span></span>
* <span data-ttu-id="21ca5-768">`feed\entry[i]\content\properties\Value`-Esemény teljes száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-768">`feed\entry[i]\content\properties\Value` - Total event count.</span></span>

<span data-ttu-id="21ca5-769">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-769">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a><span data-ttu-id="21ca5-770">9.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-770">9.4.</span></span>    <span data-ttu-id="21ca5-771">Használati fájl minta beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-771">Get Usage File Sample</span></span>
<span data-ttu-id="21ca5-772">Lekéri hello használati fájl tartalma első 2 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-772">Retrieves hello first 2KB of usage file content.</span></span>

| <span data-ttu-id="21ca5-773">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-773">HTTP Method</span></span> | <span data-ttu-id="21ca5-774">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-774">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-775">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-775">GET</span></span> |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-776">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-776">Example:</span></span><br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-777">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-777">Parameter Name</span></span> | <span data-ttu-id="21ca5-778">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-778">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-779">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-779">modelId</span></span> |<span data-ttu-id="21ca5-780">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-780">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-781">fileId</span><span class="sxs-lookup"><span data-stu-id="21ca5-781">fileId</span></span> |<span data-ttu-id="21ca5-782">Hello használati modellfájl egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-782">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="21ca5-783">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-783">apiVersion</span></span> |<span data-ttu-id="21ca5-784">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-784">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-785">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-785">Request Body</span></span> |<span data-ttu-id="21ca5-786">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-786">NONE</span></span> |

<span data-ttu-id="21ca5-787">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-787">**Response**:</span></span>

<span data-ttu-id="21ca5-788">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-788">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-789">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="21ca5-789">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a><span data-ttu-id="21ca5-790">9.5.</span><span class="sxs-lookup"><span data-stu-id="21ca5-790">9.5.</span></span>    <span data-ttu-id="21ca5-791">Használati modellfájl beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-791">Get Model Usage File</span></span>
<span data-ttu-id="21ca5-792">Lekéri a hello hello használati fájl teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-792">Retrieves hello full content of hello usage file.</span></span>

| <span data-ttu-id="21ca5-793">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-793">HTTP Method</span></span> | <span data-ttu-id="21ca5-794">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-794">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-795">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-795">GET</span></span> |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-796">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-796">Example:</span></span><br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-797">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-797">Parameter Name</span></span> | <span data-ttu-id="21ca5-798">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-798">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-799">Mid</span><span class="sxs-lookup"><span data-stu-id="21ca5-799">mid</span></span> |<span data-ttu-id="21ca5-800">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-800">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-801">FID</span><span class="sxs-lookup"><span data-stu-id="21ca5-801">fid</span></span> |<span data-ttu-id="21ca5-802">Hello használati modellfájl egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-802">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="21ca5-803">Letöltése</span><span class="sxs-lookup"><span data-stu-id="21ca5-803">download</span></span> |<span data-ttu-id="21ca5-804">1</span><span class="sxs-lookup"><span data-stu-id="21ca5-804">1</span></span> |
| <span data-ttu-id="21ca5-805">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-805">apiVersion</span></span> |<span data-ttu-id="21ca5-806">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-806">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-807">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-807">Request Body</span></span> |<span data-ttu-id="21ca5-808">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-808">NONE</span></span> |

<span data-ttu-id="21ca5-809">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-809">**Response**:</span></span>

<span data-ttu-id="21ca5-810">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-810">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-811">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="21ca5-811">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a><span data-ttu-id="21ca5-812">9.6.</span><span class="sxs-lookup"><span data-stu-id="21ca5-812">9.6.</span></span>    <span data-ttu-id="21ca5-813">Használati fájl törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-813">Delete Usage File</span></span>
<span data-ttu-id="21ca5-814">Hello megadott modell használati fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-814">Deletes hello specified model usage file.</span></span>

| <span data-ttu-id="21ca5-815">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-815">HTTP Method</span></span> | <span data-ttu-id="21ca5-816">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-816">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-817">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-817">DELETE</span></span> |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-818">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-818">Example:</span></span><br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-819">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-819">Parameter Name</span></span> | <span data-ttu-id="21ca5-820">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-820">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-821">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-821">modelId</span></span> |<span data-ttu-id="21ca5-822">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-822">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-823">fileId</span><span class="sxs-lookup"><span data-stu-id="21ca5-823">fileId</span></span> |<span data-ttu-id="21ca5-824">Hello fájl toobe törölt egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-824">Unique identifier of hello file toobe deleted</span></span> |
| <span data-ttu-id="21ca5-825">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-825">apiVersion</span></span> |<span data-ttu-id="21ca5-826">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-826">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-827">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-827">Request Body</span></span> |<span data-ttu-id="21ca5-828">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-828">NONE</span></span> |

<span data-ttu-id="21ca5-829">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-829">**Response**:</span></span>

<span data-ttu-id="21ca5-830">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-830">HTTP Status code: 200</span></span>

### <a name="97----delete-all-usage-files"></a><span data-ttu-id="21ca5-831">9.7.</span><span class="sxs-lookup"><span data-stu-id="21ca5-831">9.7.</span></span>    <span data-ttu-id="21ca5-832">Minden fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-832">Delete All Usage Files</span></span>
<span data-ttu-id="21ca5-833">Törli az összes modell használati fájlt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-833">Deletes all model usage files.</span></span>

| <span data-ttu-id="21ca5-834">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-834">HTTP Method</span></span> | <span data-ttu-id="21ca5-835">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-835">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-836">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-836">DELETE</span></span> |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-837">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-837">Example:</span></span><br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-838">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-838">Parameter Name</span></span> | <span data-ttu-id="21ca5-839">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-839">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-840">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-840">modelId</span></span> |<span data-ttu-id="21ca5-841">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-841">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-842">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-842">apiVersion</span></span> |<span data-ttu-id="21ca5-843">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-843">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-844">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-844">Request Body</span></span> |<span data-ttu-id="21ca5-845">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-845">NONE</span></span> |

<span data-ttu-id="21ca5-846">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-846">**Response**:</span></span>

<span data-ttu-id="21ca5-847">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-847">HTTP Status code: 200</span></span>

## <a name="10-features"></a><span data-ttu-id="21ca5-848">10. Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="21ca5-848">10. Features</span></span>
<span data-ttu-id="21ca5-849">Ez a szakasz bemutatja, hogyan tooretrieve funkció információkat, például importált hello szolgáltatások és azok értékeit, az osztályozás, és amikor a dimenziószáma lett lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-849">This section shows how tooretrieve feature information, such as hello imported features and their values, their rank, and when this rank was allocated.</span></span> <span data-ttu-id="21ca5-850">Szolgáltatások importált hello eseménykatalógus-adatok részeként, és majd a rangsor tartozik amikor rank build hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="21ca5-850">Features are imported as part of hello catalog data, and then their rank is associated when a rank build is done.</span></span>
<span data-ttu-id="21ca5-851">A szolgáltatás dimenziószáma függően toohello mintát használati adatok és típusú elemeket módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-851">Feature rank can change according toohello pattern of usage data and type of items.</span></span> <span data-ttu-id="21ca5-852">De konzisztens használati elemek, hello dimenziószáma csak kis ingadozását kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="21ca5-852">But for consistent usage/items, hello rank should have only small fluctuations.</span></span>
<span data-ttu-id="21ca5-853">szolgáltatások hello rangsorát egy nem negatív szám.</span><span class="sxs-lookup"><span data-stu-id="21ca5-853">hello rank of features is a non-negative number.</span></span> <span data-ttu-id="21ca5-854">hello száma 0 azt jelenti, hogy adott hello szolgáltatás nem volt rangsorolva (történik, ha a hello első rank build API előzetes toohello megvalósításának indításakor).</span><span class="sxs-lookup"><span data-stu-id="21ca5-854">hello  number 0 means that hello feature was not ranked (happens if you invoke this API prior toohello completion of hello first rank build).</span></span> <span data-ttu-id="21ca5-855">hello dátum, amelyen hello dimenziószáma attribútummal lett hello pontszám frissesség nevezik.</span><span class="sxs-lookup"><span data-stu-id="21ca5-855">hello date at which hello rank was attributed is called hello score freshness.</span></span>

### <a name="101-get-features-info-for-last-rank-build"></a><span data-ttu-id="21ca5-856">10.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-856">10.1.</span></span> <span data-ttu-id="21ca5-857">Található szolgáltatások adat (a sorrendet megadó utolsó Buildverziót)</span><span class="sxs-lookup"><span data-stu-id="21ca5-857">Get Features Info (For Last Rank Build)</span></span>
<span data-ttu-id="21ca5-858">Hello szolgáltatás információt, beleértve a rangsorolási hello utolsó sikeres rank buildjéhez kéri le.</span><span class="sxs-lookup"><span data-stu-id="21ca5-858">Retrieves hello feature information, including ranking, for hello last successful rank build.</span></span>

| <span data-ttu-id="21ca5-859">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-859">HTTP Method</span></span> | <span data-ttu-id="21ca5-860">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-860">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-861">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-861">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-862">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-862">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-863">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-863">Parameter Name</span></span> | <span data-ttu-id="21ca5-864">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-864">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-865">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-865">modelId</span></span> |<span data-ttu-id="21ca5-866">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-866">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-867">samplingSize</span><span class="sxs-lookup"><span data-stu-id="21ca5-867">samplingSize</span></span> |<span data-ttu-id="21ca5-868">Az egyes szolgáltatásokhoz szerint toohello adatok hello-katalógusban szereplő értékek tooinclude száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-868">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span> <br/><span data-ttu-id="21ca5-869">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="21ca5-869">Possible values are:</span></span><br> <span data-ttu-id="21ca5-870">-1 - összes mintát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-870">-1 - All samples.</span></span> <br><span data-ttu-id="21ca5-871">0 - nem mintavételi.</span><span class="sxs-lookup"><span data-stu-id="21ca5-871">0 - No sampling.</span></span> <br><span data-ttu-id="21ca5-872">N - N minták minden szolgáltatás nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-872">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="21ca5-873">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-873">apiVersion</span></span> |<span data-ttu-id="21ca5-874">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-874">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-875">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-875">Request Body</span></span> |<span data-ttu-id="21ca5-876">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-876">NONE</span></span> |

<span data-ttu-id="21ca5-877">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-877">**Response**:</span></span>

<span data-ttu-id="21ca5-878">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-878">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-879">hello válasz szolgáltatás info bejegyzések listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-879">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="21ca5-880">Mindegyik bejegyzés tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="21ca5-880">Each entry contains:</span></span>

* <span data-ttu-id="21ca5-881">`feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-881">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="21ca5-882">`feed/entry/content/m:properties/d:RankUpdateDate`-A dátum mely hello dimenziószáma volt lefoglalt toothis szolgáltatás, más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-882">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="21ca5-883">pontszám frissesség szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21ca5-883">score freshness feature.</span></span> <span data-ttu-id="21ca5-884">Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.</span><span class="sxs-lookup"><span data-stu-id="21ca5-884">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="21ca5-885">`feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float).</span><span class="sxs-lookup"><span data-stu-id="21ca5-885">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="21ca5-886">A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-886">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="21ca5-887">`feed/entry/content/m:properties/d:SampleValues`-A kért toohello mintavételi méretét értékek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-887">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="21ca5-888">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-888">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get hello features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a><span data-ttu-id="21ca5-889">10.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-889">10.2.</span></span> <span data-ttu-id="21ca5-890">Található szolgáltatások adat (a megadott sorszám Build)</span><span class="sxs-lookup"><span data-stu-id="21ca5-890">Get Features Info (For Specific Rank Build)</span></span>
<span data-ttu-id="21ca5-891">Hello szolgáltatás adatai, beleértve az adott rank build besorolása hello kéri le.</span><span class="sxs-lookup"><span data-stu-id="21ca5-891">Retrieves hello feature information, including hello ranking for a specific rank build.</span></span>

| <span data-ttu-id="21ca5-892">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-892">HTTP Method</span></span> | <span data-ttu-id="21ca5-893">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-893">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-894">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-894">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-895">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-895">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-896">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-896">Parameter Name</span></span> | <span data-ttu-id="21ca5-897">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-897">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-898">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-898">modelId</span></span> |<span data-ttu-id="21ca5-899">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-899">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-900">samplingSize</span><span class="sxs-lookup"><span data-stu-id="21ca5-900">samplingSize</span></span> |<span data-ttu-id="21ca5-901">Az egyes szolgáltatásokhoz szerint toohello adatok hello-katalógusban szereplő értékek tooinclude száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-901">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span><br/> <span data-ttu-id="21ca5-902">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="21ca5-902">Possible values are:</span></span><br> <span data-ttu-id="21ca5-903">-1 - összes mintát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-903">-1 - All samples.</span></span> <br><span data-ttu-id="21ca5-904">0 - nem mintavételi.</span><span class="sxs-lookup"><span data-stu-id="21ca5-904">0 - No sampling.</span></span> <br><span data-ttu-id="21ca5-905">N - N minták minden szolgáltatás nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-905">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="21ca5-906">rankBuildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-906">rankBuildId</span></span> |<span data-ttu-id="21ca5-907">Hello rank build vagy -1 -nek hello utolsó rank build egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-907">Unique identifier of hello rank build or -1 for hello last rank build</span></span> |
| <span data-ttu-id="21ca5-908">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-908">apiVersion</span></span> |<span data-ttu-id="21ca5-909">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-909">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-910">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-910">Request Body</span></span> |<span data-ttu-id="21ca5-911">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-911">NONE</span></span> |

<span data-ttu-id="21ca5-912">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-912">**Response**:</span></span>

<span data-ttu-id="21ca5-913">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-913">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-914">hello válasz szolgáltatás info bejegyzések listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-914">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="21ca5-915">Mindegyik bejegyzés tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="21ca5-915">Each entry contains:</span></span>

* <span data-ttu-id="21ca5-916">`feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-916">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="21ca5-917">`feed/entry/content/m:properties/d:RankUpdateDate`-A dátum mely hello dimenziószáma volt lefoglalt toothis szolgáltatás, más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-917">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="21ca5-918">pontszám frissesség szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21ca5-918">score freshness feature.</span></span> <span data-ttu-id="21ca5-919">Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.</span><span class="sxs-lookup"><span data-stu-id="21ca5-919">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="21ca5-920">`feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float).</span><span class="sxs-lookup"><span data-stu-id="21ca5-920">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="21ca5-921">A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-921">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="21ca5-922">`feed/entry/content/m:properties/d:SampleValues`-A kért toohello mintavételi méretét értékek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-922">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="21ca5-923">OData</span><span class="sxs-lookup"><span data-stu-id="21ca5-923">OData</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get hello features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a><span data-ttu-id="21ca5-924">11. Felépítés</span><span class="sxs-lookup"><span data-stu-id="21ca5-924">11. Build</span></span>
  <span data-ttu-id="21ca5-925">Ez a szakasz ismerteti a hello különböző API-k kapcsolódó toobuilds.</span><span class="sxs-lookup"><span data-stu-id="21ca5-925">This section explains hello different APIs related toobuilds.</span></span> <span data-ttu-id="21ca5-926">3 különböző típusú buildek: javaslat build, a sorrendet megadó build és egy (gyakran vásárolt együtt) FBT build.</span><span class="sxs-lookup"><span data-stu-id="21ca5-926">There are 3 types of builds: a recommendation build, a rank build and an FBT (frequently bought together) build.</span></span>

<span data-ttu-id="21ca5-927">hello javaslat build célja egy javaslat modell használt előrejelzéseket toogenerate.</span><span class="sxs-lookup"><span data-stu-id="21ca5-927">hello recommendation build's purpose is toogenerate a recommendation model used for predictions.</span></span> <span data-ttu-id="21ca5-928">(Az ilyen típusú build) előrejelzéseket térjen két verziója:</span><span class="sxs-lookup"><span data-stu-id="21ca5-928">Predictions (for this type of build) come in two flavors:</span></span>

* <span data-ttu-id="21ca5-929">I2I – más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-929">I2I - a.k.a.</span></span> <span data-ttu-id="21ca5-930">Konfigurációelem-tooItem ajánlásokkal – a megadott elem vagy egy lista elemek ezt a beállítást fogja becsülhető, valószínűleg toobe magas érdeklő elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-930">Item tooItem recommendations - given an item or a list of items this option will predict a list of items that are likely toobe of high interest.</span></span>
* <span data-ttu-id="21ca5-931">U2I – más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-931">U2I - a.k.a.</span></span> <span data-ttu-id="21ca5-932">Felhasználói tooItem ajánlásokkal – a megadott felhasználói azonosító (és nem kötelezően elemek listáját) Ez a beállítás lesz előrejelzése, amelyek az adott felhasználó (és a további elemeket választhat) hello magas érdeklő valószínűleg toobe elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-932">User tooItem recommendations - given a user id (and optionally a list of items) this option will predict a list of items that are likely toobe of high interest for hello given user (and its additional choice of items).</span></span> <span data-ttu-id="21ca5-933">hello U2I az ajánlások hello felhasználó hello modell készült toohello elérhetőséggel érdeklő elemeket hello előzményeit.</span><span class="sxs-lookup"><span data-stu-id="21ca5-933">hello U2I recommendations are based on hello history of items that were of interest for hello user up toohello time hello model was built.</span></span>

<span data-ttu-id="21ca5-934">A sorrendet megadó build, amely lehetővé teszi a szolgáltatások hello hasznosságát kapcsolatos toolearn műszaki build.</span><span class="sxs-lookup"><span data-stu-id="21ca5-934">A rank build is a technical build that allows you toolearn about hello usefulness of your features.</span></span> <span data-ttu-id="21ca5-935">Általában sorrendben tooget hello legjobb eredmény egy javaslat modell szolgáltatások használata esetén, hajtson hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21ca5-935">Usually, in order tooget hello best result for a recommendation model involving features, you should take hello following steps:</span></span>

* <span data-ttu-id="21ca5-936">Aktiválhatja a sorrendet megadó build, (kivéve, ha a szolgáltatások hello pontszám stabil), és várjon, amíg hello szolgáltatás pontszámot kap.</span><span class="sxs-lookup"><span data-stu-id="21ca5-936">Trigger a rank build (unless hello score of your features is stable) and wait till you get hello feature score.</span></span>
* <span data-ttu-id="21ca5-937">A szolgáltatások hello rangsorát lekérni hívó hello [szolgáltatások információ](#101-get-features-info-for-last-rank-build) API.</span><span class="sxs-lookup"><span data-stu-id="21ca5-937">Retrieve hello rank of your features by calling hello [Get Features Info](#101-get-features-info-for-last-rank-build) API.</span></span>
* <span data-ttu-id="21ca5-938">A javaslat build a következő paraméterek hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="21ca5-938">Configure a recommendation build with hello following parameters:</span></span>
  * <span data-ttu-id="21ca5-939">`useFeatureInModel`-Set tooTrue.</span><span class="sxs-lookup"><span data-stu-id="21ca5-939">`useFeatureInModel` - Set tooTrue.</span></span>
  * <span data-ttu-id="21ca5-940">`ModelingFeatureList`-(Szerint toohello egyes holtversenyekben hello előző lépésben lekért) set tooa vesszővel tagolt listája a 2.0-s vagy több jelző pontszámot funkcióinak használatát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-940">`ModelingFeatureList` - Set tooa comma-separated list of features with a score of 2.0 or more (according toohello ranks you retrieved in hello previous step).</span></span>
  * <span data-ttu-id="21ca5-941">`AllowColdItemPlacement`-Set tooTrue.</span><span class="sxs-lookup"><span data-stu-id="21ca5-941">`AllowColdItemPlacement` - Set tooTrue.</span></span>
  * <span data-ttu-id="21ca5-942">Opcionálisan megadhat `EnableFeatureCorrelation` tooTrue és `ReasoningFeatureList` toohello listája toouse magyarázatot (általában szolgáltatások azonos listája szerepel modellezés vagy allista hello) kívánt szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-942">Optionally you can set `EnableFeatureCorrelation` tooTrue and `ReasoningFeatureList` toohello list of features you want toouse for explanations (usually hello same list of features used in modelling or a sublist).</span></span>
* <span data-ttu-id="21ca5-943">Indítás, hello javaslat build hello konfigurált paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="21ca5-943">Trigger hello recommendation build with hello configured parameters.</span></span>

<span data-ttu-id="21ca5-944">Megjegyzés: Ha nem adja meg a paramétereket (pl. meghívása hello javaslat build paraméterek nélkül) vagy nem explicit módon letiltja szolgáltatások hello használatát (pl. `UseFeatureInModel` tooFalse beállítása), hello rendszer hello szolgáltatás kapcsolatos paraméterek beállításához toohello viszonylag fenti alapértékeket, abban az esetben, ha a sorrendet megadó build létezik-e.</span><span class="sxs-lookup"><span data-stu-id="21ca5-944">Note: If you do not configure any parameters (e.g. invoke hello recommendation build without parameters) or you do not explicitly disable hello usage of features (e.g. `UseFeatureInModel` set tooFalse), hello system will set up hello feature-related parameters toohello explained values above in case a rank build exists.</span></span>

<span data-ttu-id="21ca5-945">A sorrendet megadó build futó nincs korlátja, és az ajánlás építése egyidejűleg hello azonos modell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-945">There is no restriction on running a rank build and a recommendation build concurrently for hello same model.</span></span> <span data-ttu-id="21ca5-946">Ettől függetlenül az azonos írja be ugyanazt a modell párhuzamosan hello hello két változata nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-946">Nevertheless, you cannot run two builds of hello same type on hello same model in parallel.</span></span>

<span data-ttu-id="21ca5-947">Egy (gyakran vásárolt együtt) FBT build még egy másik javaslatok algoritmus nevezik néha "konzervatív" ajánló, ami ideális, amelyek nincsenek homogén jellegű katalógusok (homogén: könyvek, filmek, néhány étele módon; nem homogén: számítógép és eszközök, a tartományok közötti, rendkívül sokféle).</span><span class="sxs-lookup"><span data-stu-id="21ca5-947">An FBT (Frequently bought together) build is yet another recommendations algorithm called sometimes "conservative" recommender, which is good for catalogs that are not homogeneous in nature (homogeneous: books, movies, some food, fashion; non-homogeneous: computer and devices, cross-domain, highly diverse).</span></span>

<span data-ttu-id="21ca5-948">Megjegyzés: Ha hello feltöltött fájlok tartalmaz hello választható mező "eseménytípus" majd FBT a modellezési csak "a Vásárlás" események használható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-948">Note: if hello usage files that you uploaded contain hello optional field "event type" then for FBT modelling only "Purchase" events will be used.</span></span> <span data-ttu-id="21ca5-949">Ha nincs eseménytípus biztosítja az összes esemény beszerzési kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-949">If no event type is provided all events will be considered as purchase.</span></span>

#### <a name="111-build-parameters"></a><span data-ttu-id="21ca5-950">11.1 build paraméterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-950">11.1 Build parameters</span></span>
<span data-ttu-id="21ca5-951">Minden build típusú paraméterek (kitaláltak alább) segítségével konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="21ca5-951">Each build type can be configured via a set of parameters (depicted below).</span></span> <span data-ttu-id="21ca5-952">Ha nem állít hello paraméterek, a hello rendszer automatikusan attribútum értékek toohello paraméterek toohello információk hello jelen indít el a build szerint.</span><span class="sxs-lookup"><span data-stu-id="21ca5-952">If you don't configure hello parameters, hello system will automatically attribute values toohello parameters according toohello information present at hello time you trigger a build.</span></span>

##### <a name="1111-usage-condenser"></a><span data-ttu-id="21ca5-953">11.1.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-953">11.1.1.</span></span> <span data-ttu-id="21ca5-954">Használati hűtő</span><span class="sxs-lookup"><span data-stu-id="21ca5-954">Usage condenser</span></span>
<span data-ttu-id="21ca5-955">Felhasználók, illetve néhány használati pont cikkeket további zaj mint információkat is tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="21ca5-955">Users or items with few usage points might contain more noise than information.</span></span> <span data-ttu-id="21ca5-956">hello rendszer toopredict hello minimális használati pontok száma a modellben használt felhasználói/elem toobe próbál.</span><span class="sxs-lookup"><span data-stu-id="21ca5-956">hello system attempts toopredict hello minimal number of usage points per user/item toobe used in a model.</span></span> <span data-ttu-id="21ca5-957">Ez a szám hello ItemCutoffLowerBound és elemeket, és hello UserCutOffLowerBound és UserCutoffUpperBound paramétereket a felhasználó által definiált hello tartomány ItemCutoffUpperBound paramétereinek hello tartományon belüli lesz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-957">This number will be within hello range defined by hello ItemCutoffLowerBound and ItemCutoffUpperBound parameters for items, and hello range defined by hello UserCutOffLowerBound and UserCutoffUpperBound parameters for users.</span></span> <span data-ttu-id="21ca5-958">hello hűtő hatással elemek vagy felhasználók hello megfelelő határainak toozero közül legalább egy beállítást minimálisra csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-958">hello condenser effect on items or users can be minimized by setting at least one of hello corresponding bounds toozero.</span></span>

##### <a name="1112-rank-build-parameters"></a><span data-ttu-id="21ca5-959">11.1.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-959">11.1.2.</span></span> <span data-ttu-id="21ca5-960">Dimenziószáma build paraméterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-960">Rank build parameters</span></span>
<span data-ttu-id="21ca5-961">az alábbi táblázat hello hello build paramétereket a sorrendet megadó build ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-961">hello table below depicts hello build parameters for a rank build.</span></span>

| <span data-ttu-id="21ca5-962">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-962">Key</span></span> | <span data-ttu-id="21ca5-963">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-963">Description</span></span> | <span data-ttu-id="21ca5-964">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-964">Type</span></span> | <span data-ttu-id="21ca5-965">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="21ca5-965">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="21ca5-966">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="21ca5-966">NumberOfModelIterations</span></span> |<span data-ttu-id="21ca5-967">hello modell hajtja végre az ismétlések száma hello hello jelzi a teljes számítási és hello modell pontosságának.</span><span class="sxs-lookup"><span data-stu-id="21ca5-967">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="21ca5-968">hello magasabb hello számát, nagyobb pontosságot hello elérhetővé válik, de hello számítási idő hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="21ca5-968">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="21ca5-969">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-969">Integer</span></span> |<span data-ttu-id="21ca5-970">10-50</span><span class="sxs-lookup"><span data-stu-id="21ca5-970">10-50</span></span> |
| <span data-ttu-id="21ca5-971">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="21ca5-971">NumberOfModelDimensions</span></span> |<span data-ttu-id="21ca5-972">a dimenziók száma hello vonatkozik toohello száma "szolgáltatások" hello modell megpróbál toofind belül az adatokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-972">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="21ca5-973">Hello dimenziószámának növelése lehetővé teszi a nagyobb pontosabb beállításra hello eredmények kisebb csoportokba.</span><span class="sxs-lookup"><span data-stu-id="21ca5-973">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="21ca5-974">Azonban túl sok dimenzióval megakadályozza hello modell elemek közötti összefüggések keresése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-974">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="21ca5-975">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-975">Integer</span></span> |<span data-ttu-id="21ca5-976">10-40</span><span class="sxs-lookup"><span data-stu-id="21ca5-976">10-40</span></span> |
| <span data-ttu-id="21ca5-977">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-977">ItemCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-978">Hello elem alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-978">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-979">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-979">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-980">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-980">Integer</span></span> |<span data-ttu-id="21ca5-981">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-981">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-982">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-982">ItemCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-983">Elem felső korlátja hello hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-983">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-984">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-984">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-985">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-985">Integer</span></span> |<span data-ttu-id="21ca5-986">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-986">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-987">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-987">UserCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-988">Hello felhasználói alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-988">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-989">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-989">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-990">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-990">Integer</span></span> |<span data-ttu-id="21ca5-991">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-991">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-992">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-992">UserCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-993">Meghatározza a hello hűtő hello felhasználói felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-993">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-994">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-994">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-995">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-995">Integer</span></span> |<span data-ttu-id="21ca5-996">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-996">2 or more (0 disable condenser)</span></span> |

##### <a name="1113-recommendation-build-parameters"></a><span data-ttu-id="21ca5-997">11.1.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-997">11.1.3.</span></span> <span data-ttu-id="21ca5-998">A javaslat build paraméterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-998">Recommendation build parameters</span></span>
<span data-ttu-id="21ca5-999">az alábbi táblázat hello hello build paramétereinek javaslat build ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-999">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="21ca5-1000">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-1000">Key</span></span> | <span data-ttu-id="21ca5-1001">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1001">Description</span></span> | <span data-ttu-id="21ca5-1002">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1002">Type</span></span> | <span data-ttu-id="21ca5-1003">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="21ca5-1003">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="21ca5-1004">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="21ca5-1004">NumberOfModelIterations</span></span> |<span data-ttu-id="21ca5-1005">hello modell hajtja végre az ismétlések száma hello hello jelzi a teljes számítási és hello modell pontosságának.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1005">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="21ca5-1006">hello magasabb hello számát, nagyobb pontosságot hello elérhetővé válik, de hello számítási idő hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1006">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="21ca5-1007">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1007">Integer</span></span> |<span data-ttu-id="21ca5-1008">10-50</span><span class="sxs-lookup"><span data-stu-id="21ca5-1008">10-50</span></span> |
| <span data-ttu-id="21ca5-1009">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="21ca5-1009">NumberOfModelDimensions</span></span> |<span data-ttu-id="21ca5-1010">a dimenziók száma hello vonatkozik toohello száma "szolgáltatások" hello modell megpróbál toofind belül az adatokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1010">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="21ca5-1011">Hello dimenziószámának növelése lehetővé teszi a nagyobb pontosabb beállításra hello eredmények kisebb csoportokba.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1011">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="21ca5-1012">Azonban túl sok dimenzióval megakadályozza hello modell elemek közötti összefüggések keresése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1012">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="21ca5-1013">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1013">Integer</span></span> |<span data-ttu-id="21ca5-1014">10-40</span><span class="sxs-lookup"><span data-stu-id="21ca5-1014">10-40</span></span> |
| <span data-ttu-id="21ca5-1015">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1015">ItemCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-1016">Hello elem alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1016">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1017">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1017">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1018">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1018">Integer</span></span> |<span data-ttu-id="21ca5-1019">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1019">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1020">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1020">ItemCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-1021">Elem felső korlátja hello hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1021">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1022">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1022">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1023">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1023">Integer</span></span> |<span data-ttu-id="21ca5-1024">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1024">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1025">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1025">UserCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-1026">Hello felhasználói alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1026">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1027">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1027">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1028">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1028">Integer</span></span> |<span data-ttu-id="21ca5-1029">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1029">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1030">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1030">UserCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-1031">Meghatározza a hello hűtő hello felhasználói felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1031">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1032">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1032">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1033">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1033">Integer</span></span> |<span data-ttu-id="21ca5-1034">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1034">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1035">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1035">Description</span></span> |<span data-ttu-id="21ca5-1036">Build leírása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1036">Build description.</span></span> |<span data-ttu-id="21ca5-1037">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1037">String</span></span> |<span data-ttu-id="21ca5-1038">A szöveg, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="21ca5-1038">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="21ca5-1039">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="21ca5-1039">EnableModelingInsights</span></span> |<span data-ttu-id="21ca5-1040">Lehetővé teszi toocompute metrikák hello javaslat modellen.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1040">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="21ca5-1041">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1041">Boolean</span></span> |<span data-ttu-id="21ca5-1042">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1042">True/False</span></span> |
| <span data-ttu-id="21ca5-1043">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="21ca5-1043">UseFeaturesInModel</span></span> |<span data-ttu-id="21ca5-1044">Azt jelzi, ha használható-e a szolgáltatások rendelés tooenhance hello javaslat modellben.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1044">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="21ca5-1045">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1045">Boolean</span></span> |<span data-ttu-id="21ca5-1046">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1046">True/False</span></span> |
| <span data-ttu-id="21ca5-1047">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="21ca5-1047">ModelingFeatureList</span></span> |<span data-ttu-id="21ca5-1048">Szolgáltatás neve toobe hello ajánlás buildverziót, sorrendben tooenhance hello javaslat használt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1048">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="21ca5-1049">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1049">String</span></span> |<span data-ttu-id="21ca5-1050">Funkcióneveket too512 karakter mentése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1050">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="21ca5-1051">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="21ca5-1051">AllowColdItemPlacement</span></span> |<span data-ttu-id="21ca5-1052">Azt jelzi, ha hello javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1052">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="21ca5-1053">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1053">Boolean</span></span> |<span data-ttu-id="21ca5-1054">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1054">True/False</span></span> |
| <span data-ttu-id="21ca5-1055">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="21ca5-1055">EnableFeatureCorrelation</span></span> |<span data-ttu-id="21ca5-1056">Azt jelzi, ha szolgáltatások indoklást is használható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1056">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="21ca5-1057">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1057">Boolean</span></span> |<span data-ttu-id="21ca5-1058">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1058">True/False</span></span> |
| <span data-ttu-id="21ca5-1059">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="21ca5-1059">ReasoningFeatureList</span></span> |<span data-ttu-id="21ca5-1060">Szolgáltatás neve toobe mintafelismerési mondat (pl. javaslat magyarázatokat) használt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1060">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="21ca5-1061">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1061">String</span></span> |<span data-ttu-id="21ca5-1062">Funkcióneveket too512 karakter mentése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1062">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="21ca5-1063">EnableU2I</span><span class="sxs-lookup"><span data-stu-id="21ca5-1063">EnableU2I</span></span> |<span data-ttu-id="21ca5-1064">Személyre szabott hello javaslat más néven engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1064">Allow hello personalized recommendation a.k.a.</span></span> <span data-ttu-id="21ca5-1065">U2I (felhasználói tooitem javaslatok).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1065">U2I (user tooitem recommendations).</span></span> |<span data-ttu-id="21ca5-1066">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1066">Boolean</span></span> |<span data-ttu-id="21ca5-1067">Igaz/hamis (alapértelmezett értéke igaz)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1067">True/False (default true)</span></span> |

##### <a name="1114-fbt-build-parameters"></a><span data-ttu-id="21ca5-1068">11.1.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1068">11.1.4.</span></span> <span data-ttu-id="21ca5-1069">FBT build paraméterek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1069">FBT build parameters</span></span>
<span data-ttu-id="21ca5-1070">az alábbi táblázat hello hello build paramétereinek javaslat build ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1070">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="21ca5-1071">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-1071">Key</span></span> | <span data-ttu-id="21ca5-1072">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1072">Description</span></span> | <span data-ttu-id="21ca5-1073">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1073">Type</span></span> | <span data-ttu-id="21ca5-1074">Érvénytelen érték (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1074">Valid Value (Default)</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="21ca5-1075">FbtSupportThreshold</span><span class="sxs-lookup"><span data-stu-id="21ca5-1075">FbtSupportThreshold</span></span> |<span data-ttu-id="21ca5-1076">Hogyan konzervatív hello modell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1076">How conservative hello model is.</span></span> <span data-ttu-id="21ca5-1077">Elemek toobe modellezési infrastruktúrához közös előfordulásainak száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1077">Number of co-occurrences of items toobe considered for modeling.</span></span> |<span data-ttu-id="21ca5-1078">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1078">Integer</span></span> |<span data-ttu-id="21ca5-1079">3-50 (6)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1079">3-50 (6)</span></span> |
| <span data-ttu-id="21ca5-1080">FbtMaxItemSetSize</span><span class="sxs-lookup"><span data-stu-id="21ca5-1080">FbtMaxItemSetSize</span></span> |<span data-ttu-id="21ca5-1081">Gyakori készletben határainak hello száma.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1081">Bounds hello number of items in a frequent set.</span></span> |<span data-ttu-id="21ca5-1082">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1082">Integer</span></span> |<span data-ttu-id="21ca5-1083">2-3 (2)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1083">2-3 (2)</span></span> |
| <span data-ttu-id="21ca5-1084">FbtMinimalScore</span><span class="sxs-lookup"><span data-stu-id="21ca5-1084">FbtMinimalScore</span></span> |<span data-ttu-id="21ca5-1085">Egy gyakori készlet kell vissza kellett volna a hello szereplő sorrendben toobe minimális pontszámot annak az eredménye.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1085">Minimal score that a frequent set should have in order toobe included in hello returned results.</span></span> <span data-ttu-id="21ca5-1086">hello magasabb hello jobb.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1086">hello higher hello better.</span></span> |<span data-ttu-id="21ca5-1087">Dupla</span><span class="sxs-lookup"><span data-stu-id="21ca5-1087">Double</span></span> |<span data-ttu-id="21ca5-1088">0 és újabb (0)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1088">0 and above (0)</span></span> |
| <span data-ttu-id="21ca5-1089">FbtSimilarityFunction</span><span class="sxs-lookup"><span data-stu-id="21ca5-1089">FbtSimilarityFunction</span></span> |<span data-ttu-id="21ca5-1090">Meghatározza a hello hasonlóság függvény toobe hello build használják.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1090">Defines hello similarity function toobe used by hello build.</span></span> <span data-ttu-id="21ca5-1091">Növekedési előtérbe serendipity, közös előfordulási előtérbe kiszámíthatóságot és Jaccard hello két töltött kompromisszumot.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1091">Lift favors serendipity, Co-occurrence favors predictability, and Jaccard is a nice compromise between hello two.</span></span> |<span data-ttu-id="21ca5-1092">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1092">String</span></span> |<span data-ttu-id="21ca5-1093">cooccurrence, növekedési, jaccard (növekedési)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1093">cooccurrence, lift, jaccard (lift)</span></span> |

### <a name="112-trigger-a-recommendation-build"></a><span data-ttu-id="21ca5-1094">11.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1094">11.2.</span></span> <span data-ttu-id="21ca5-1095">A javaslat Build eseményindító</span><span class="sxs-lookup"><span data-stu-id="21ca5-1095">Trigger a Recommendation Build</span></span>
  <span data-ttu-id="21ca5-1096">Alapértelmezés szerint ez az API javaslat modell build vált.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1096">By default this API will trigger a recommendation model build.</span></span> <span data-ttu-id="21ca5-1097">a sorrend első helyén tootrigger build (a rendelés tooscore szolgáltatások), hello build API variant build típusú paraméterrel kell használni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1097">tootrigger a rank build (in order tooscore  features), hello build API variant with build type parameter should be used.</span></span>

| <span data-ttu-id="21ca5-1098">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1098">HTTP Method</span></span> | <span data-ttu-id="21ca5-1099">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1099">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1100">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-1100">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1101">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1101">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| <span data-ttu-id="21ca5-1102">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="21ca5-1102">HEADER</span></span> |<span data-ttu-id="21ca5-1103">`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1103">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="21ca5-1104">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1104">Parameter Name</span></span> | <span data-ttu-id="21ca5-1105">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1105">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1106">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1106">modelId</span></span> |<span data-ttu-id="21ca5-1107">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1107">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1108">userDescription</span><span class="sxs-lookup"><span data-stu-id="21ca5-1108">userDescription</span></span> |<span data-ttu-id="21ca5-1109">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1109">Textual identifier of hello catalog.</span></span> <span data-ttu-id="21ca5-1110">Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1110">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="21ca5-1111">Tekintse meg a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1111">See example above.</span></span><br><span data-ttu-id="21ca5-1112">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-1112">Max length: 50</span></span> |
| <span data-ttu-id="21ca5-1113">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1113">apiVersion</span></span> |<span data-ttu-id="21ca5-1114">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1114">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-1115">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-1115">Request Body</span></span> |<span data-ttu-id="21ca5-1116">Ha üres majd hello build fogja végrehajtani hello alapértelmezett build paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1116">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="21ca5-1117">Ha azt szeretné, hogy tooset hello build paraméterek, hello paraméterek küldése XML formátumban történő hello törzs hasonlóan a következő minta hello.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1117">If you want tooset hello build parameters, send hello parameters as XML into hello body as in hello following sample.</span></span> <span data-ttu-id="21ca5-1118">(Lásd "Build paraméterek" hello hello paraméterek előzetesben.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1118">(See hello "Build parameters" section for an explanation of hello parameters.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span></span> |

<span data-ttu-id="21ca5-1119">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1119">**Response**:</span></span>

<span data-ttu-id="21ca5-1120">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1120">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1121">Ez az egy aszinkron API-t.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1121">This is an asynchronous API.</span></span> <span data-ttu-id="21ca5-1122">Válaszként elérhetővé válik egy build azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1122">You will get a build ID as a response.</span></span> <span data-ttu-id="21ca5-1123">Amikor véget ért a hello build tooknow, hello "Beolvasása buildek állapota, a modell" API hívása, és keresse meg a build azonosító hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1123">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="21ca5-1124">Ne feledje, hogy a build telhet-e a perc toohours hello hello adatok méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1124">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="21ca5-1125">Javaslatok keretein belül hello build vége nem lehet felhasználni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1125">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="21ca5-1126">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1126">Valid build status:</span></span>

* <span data-ttu-id="21ca5-1127">Hozzon létre - létrehozási kérelem lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1127">Create - Build request was created.</span></span>
* <span data-ttu-id="21ca5-1128">Aszinkron - Build kérést küld, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1128">Queued - Build request was sent and it is queued.</span></span>
* <span data-ttu-id="21ca5-1129">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1129">Building - Build is in progress.</span></span>
* <span data-ttu-id="21ca5-1130">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1130">Success - Build ended successfully.</span></span>
* <span data-ttu-id="21ca5-1131">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1131">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="21ca5-1132">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1132">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="21ca5-1133">Kapcsolódó - hello build megszakítási kérelmet küldött.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1133">Cancelling - A cancel request for hello build was sent.</span></span>

<span data-ttu-id="21ca5-1134">Vegye figyelembe, hogy hello build azonosító hello a következő elérési út alatt található:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1134">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="21ca5-1135">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1135">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a><span data-ttu-id="21ca5-1136">11.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1136">11.3.</span></span> <span data-ttu-id="21ca5-1137">Eseményindító Build (javaslat, dimenziószáma vagy FBT)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1137">Trigger Build (Recommendation, Rank or FBT)</span></span>
| <span data-ttu-id="21ca5-1138">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1138">HTTP Method</span></span> | <span data-ttu-id="21ca5-1139">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1139">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1140">POST</span><span class="sxs-lookup"><span data-stu-id="21ca5-1140">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1141">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1141">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| <span data-ttu-id="21ca5-1142">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="21ca5-1142">HEADER</span></span> |<span data-ttu-id="21ca5-1143">`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1143">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="21ca5-1144">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1144">Parameter Name</span></span> | <span data-ttu-id="21ca5-1145">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1145">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1146">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1146">modelId</span></span> |<span data-ttu-id="21ca5-1147">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1147">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1148">userDescription</span><span class="sxs-lookup"><span data-stu-id="21ca5-1148">userDescription</span></span> |<span data-ttu-id="21ca5-1149">Hello katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1149">Textual identifier of hello catalog.</span></span> <span data-ttu-id="21ca5-1150">Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1150">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="21ca5-1151">Tekintse meg a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1151">See example above.</span></span><br><span data-ttu-id="21ca5-1152">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="21ca5-1152">Max length: 50</span></span> |
| <span data-ttu-id="21ca5-1153">buildType</span><span class="sxs-lookup"><span data-stu-id="21ca5-1153">buildType</span></span> |<span data-ttu-id="21ca5-1154">Hello build tooinvoke típusa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1154">Type of hello build tooinvoke:</span></span> <br/> <span data-ttu-id="21ca5-1155">-Javaslat build "ajánlott"</span><span class="sxs-lookup"><span data-stu-id="21ca5-1155">- 'Recommendation' for recommendation build</span></span> <br> <span data-ttu-id="21ca5-1156">-"Prioritás" rank buildjéhez</span><span class="sxs-lookup"><span data-stu-id="21ca5-1156">- 'Ranking' for rank build</span></span> <br/> <span data-ttu-id="21ca5-1157">-FBT build "Fbt"</span><span class="sxs-lookup"><span data-stu-id="21ca5-1157">- 'Fbt' for FBT build</span></span> |
| <span data-ttu-id="21ca5-1158">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1158">apiVersion</span></span> |<span data-ttu-id="21ca5-1159">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1159">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-1160">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-1160">Request Body</span></span> |<span data-ttu-id="21ca5-1161">Ha üres majd hello build fogja végrehajtani hello alapértelmezett build paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1161">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="21ca5-1162">Ha azt szeretné, hogy tooset build paraméterek, küldje el XML formátumban történő hello szervezet például a következő minta hello.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1162">If you want tooset build parameters, send them as XML into hello body like in hello following sample.</span></span> <span data-ttu-id="21ca5-1163">(Lásd "Build paraméterek" hello magyarázat és hello paraméterek teljes listája.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1163">(See hello "Build parameters" section for an explanation and full list of hello parameters.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span></span> |

<span data-ttu-id="21ca5-1164">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1164">**Response**:</span></span>

<span data-ttu-id="21ca5-1165">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1165">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1166">Ez az egy aszinkron API-t.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1166">This is an asynchronous API.</span></span> <span data-ttu-id="21ca5-1167">Válaszként elérhetővé válik egy build azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1167">You will get a build ID as a response.</span></span> <span data-ttu-id="21ca5-1168">Amikor véget ért a hello build tooknow, hello "Beolvasása buildek állapota, a modell" API hívása, és keresse meg a build azonosító hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1168">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="21ca5-1169">Ne feledje, hogy a build telhet-e a perc toohours hello hello adatok méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1169">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="21ca5-1170">Javaslatok keretein belül hello build vége nem lehet felhasználni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1170">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="21ca5-1171">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1171">Valid build status:</span></span>

* <span data-ttu-id="21ca5-1172">Hozzon létre - modell lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1172">Create - Model was created.</span></span>
* <span data-ttu-id="21ca5-1173">Aszinkron - modell build lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1173">Queued - Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="21ca5-1174">Épület - modell éppen készül.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1174">Building - Model is being built.</span></span>
* <span data-ttu-id="21ca5-1175">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1175">Success - Build ended successfully.</span></span>
* <span data-ttu-id="21ca5-1176">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1176">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="21ca5-1177">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1177">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="21ca5-1178">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1178">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="21ca5-1179">Vegye figyelembe, hogy hello build azonosító hello a következő elérési út alatt található:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1179">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="21ca5-1180">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1180">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




### <a name="114-get-builds-status-of-a-model"></a><span data-ttu-id="21ca5-1181">11.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1181">11.4.</span></span> <span data-ttu-id="21ca5-1182">A modell buildek állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1182">Get Builds Status of a Model</span></span>
<span data-ttu-id="21ca5-1183">Lekéri a buildek és azok állapotát a megadott modell.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1183">Retrieves builds and their status for a specified model.</span></span>

| <span data-ttu-id="21ca5-1184">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1184">HTTP Method</span></span> | <span data-ttu-id="21ca5-1185">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1185">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1186">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1186">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1187">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1187">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1188">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1188">Parameter Name</span></span> | <span data-ttu-id="21ca5-1189">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1189">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1190">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1190">modelId</span></span> |<span data-ttu-id="21ca5-1191">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1191">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1192">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="21ca5-1192">onlyLastBuild</span></span> |<span data-ttu-id="21ca5-1193">Azt jelzi, hogy összes hello tooreturn build hello modell előzmények vagy hello legutóbbi build csak hello állapota</span><span class="sxs-lookup"><span data-stu-id="21ca5-1193">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build</span></span> |
| <span data-ttu-id="21ca5-1194">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1194">apiVersion</span></span> |<span data-ttu-id="21ca5-1195">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1195">1.0</span></span> |

<span data-ttu-id="21ca5-1196">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1196">**Response**:</span></span>

<span data-ttu-id="21ca5-1197">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1197">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1198">hello válasz tartalmazza a build egy tételt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1198">hello response includes one entry per build.</span></span> <span data-ttu-id="21ca5-1199">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1199">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1200">`feed/entry/content/properties/UserName`-Hello felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1200">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="21ca5-1201">`feed/entry/content/properties/ModelName`-Hello modell neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1201">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="21ca5-1202">`feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1202">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="21ca5-1203">`feed/entry/content/properties/IsDeployed`-E hello buildverziót központilag telepítik (más néven</span><span class="sxs-lookup"><span data-stu-id="21ca5-1203">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed (a.k.a.</span></span> <span data-ttu-id="21ca5-1204">aktív build).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1204">active build).</span></span>
* <span data-ttu-id="21ca5-1205">`feed/entry/content/properties/BuildId`-Build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1205">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="21ca5-1206">`feed/entry/content/properties/BuildType`-Hello build típusú.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1206">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="21ca5-1207">`feed/entry/content/properties/Status`-Létrehozási állapot.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1207">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="21ca5-1208">Hello a következők egyike lehet: Hiba történt, épület, várakozik, Cancelling, megszakítva, sikeres.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1208">Can be one of hello following: Error, Building, Queued, Cancelling, Cancelled, Success.</span></span>
* <span data-ttu-id="21ca5-1209">`feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak toospecific állapotok vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1209">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="21ca5-1210">`feed/entry/content/properties/Progress`-Build (%) folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1210">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="21ca5-1211">`feed/entry/content/properties/StartTime`-Build kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1211">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="21ca5-1212">`feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1212">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="21ca5-1213">`feed/entry/content/properties/ExecutionTime`-Build időtartama.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1213">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="21ca5-1214">`feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális szakasza hello adatait.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1214">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="21ca5-1215">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1215">Valid build status:</span></span>

* <span data-ttu-id="21ca5-1216">Létre - létrehozási kérelem tétel jött létre.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1216">Created - Build request entry was created.</span></span>
* <span data-ttu-id="21ca5-1217">Aszinkron - kérelmet lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1217">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="21ca5-1218">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1218">Building - Build is in process.</span></span>
* <span data-ttu-id="21ca5-1219">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1219">Success - Build ended successfully.</span></span>
* <span data-ttu-id="21ca5-1220">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1220">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="21ca5-1221">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1221">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="21ca5-1222">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1222">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="21ca5-1223">Build típusú engedélyezett érvényes értékek:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1223">Valid values for build type:</span></span>

* <span data-ttu-id="21ca5-1224">Sorszám - dimenziószáma felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1224">Rank - Rank build.</span></span>
* <span data-ttu-id="21ca5-1225">A javaslat - javaslat build.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1225">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="21ca5-1226">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1226">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="115-get-builds-status"></a><span data-ttu-id="21ca5-1227">11.5.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1227">11.5.</span></span> <span data-ttu-id="21ca5-1228">Buildek állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1228">Get Builds Status</span></span>
<span data-ttu-id="21ca5-1229">Lekéri az összes modellt a felhasználói állapotok felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1229">Retrieves build statuses of all models of a user.</span></span>

| <span data-ttu-id="21ca5-1230">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1230">HTTP Method</span></span> | <span data-ttu-id="21ca5-1231">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1231">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1232">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1232">GET</span></span> |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1233">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1233">Example:</span></span><br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1234">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1234">Parameter Name</span></span> | <span data-ttu-id="21ca5-1235">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1235">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1236">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="21ca5-1236">onlyLastBuild</span></span> |<span data-ttu-id="21ca5-1237">Azt jelzi, hogy összes hello tooreturn build hello modell előzmények vagy hello legutóbbi build csak hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1237">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build.</span></span> |
| <span data-ttu-id="21ca5-1238">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1238">apiVersion</span></span> |<span data-ttu-id="21ca5-1239">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1239">1.0</span></span> |

<span data-ttu-id="21ca5-1240">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1240">**Response**:</span></span>

<span data-ttu-id="21ca5-1241">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1241">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1242">hello válasz tartalmazza a build egy tételt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1242">hello response includes one entry per build.</span></span> <span data-ttu-id="21ca5-1243">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1243">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1244">`feed/entry/content/properties/UserName`-Hello felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1244">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="21ca5-1245">`feed/entry/content/properties/ModelName`-Hello modell neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1245">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="21ca5-1246">`feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1246">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="21ca5-1247">`feed/entry/content/properties/IsDeployed`-E hello buildverziót központilag telepítik.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1247">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed.</span></span>
* <span data-ttu-id="21ca5-1248">`feed/entry/content/properties/BuildId`-Build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1248">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="21ca5-1249">`feed/entry/content/properties/BuildType`-Hello build típusú.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1249">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="21ca5-1250">`feed/entry/content/properties/Status`-Létrehozási állapot.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1250">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="21ca5-1251">Hello a következők egyike lehet: Hiba történt, épület, várakozik, megszakítva, Cancelling, sikeres.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1251">Can be one of hello following: Error, Building, Queued, Cancelled, Cancelling, Success.</span></span>
* <span data-ttu-id="21ca5-1252">`feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak toospecific állapotok vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1252">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="21ca5-1253">`feed/entry/content/properties/Progress`-Build (%) folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1253">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="21ca5-1254">`feed/entry/content/properties/StartTime`-Build kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1254">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="21ca5-1255">`feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1255">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="21ca5-1256">`feed/entry/content/properties/ExecutionTime`-Build időtartama.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1256">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="21ca5-1257">`feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális szakasza hello adatait.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1257">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="21ca5-1258">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1258">Valid build status:</span></span>

* <span data-ttu-id="21ca5-1259">Létre - létrehozási kérelem tétel jött létre.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1259">Created - Build request entry was created.</span></span>
* <span data-ttu-id="21ca5-1260">Aszinkron - kérelmet lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1260">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="21ca5-1261">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1261">Building - Build is in process.</span></span>
* <span data-ttu-id="21ca5-1262">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1262">Success - Build ended successfully.</span></span>
* <span data-ttu-id="21ca5-1263">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1263">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="21ca5-1264">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1264">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="21ca5-1265">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1265">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="21ca5-1266">Build típusú engedélyezett érvényes értékek:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1266">Valid values for build type:</span></span>

* <span data-ttu-id="21ca5-1267">Sorszám - dimenziószáma felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1267">Rank - Rank build.</span></span>
* <span data-ttu-id="21ca5-1268">A javaslat - javaslat build.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1268">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="21ca5-1269">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1269">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="116-delete-build"></a><span data-ttu-id="21ca5-1270">11.6.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1270">11.6.</span></span> <span data-ttu-id="21ca5-1271">Build törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1271">Delete Build</span></span>
<span data-ttu-id="21ca5-1272">Build törli.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1272">Deletes a build.</span></span>

<span data-ttu-id="21ca5-1273">MEGJEGYZÉS:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1273">NOTE:</span></span> <br><span data-ttu-id="21ca5-1274">Egy aktív build nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1274">You cannot delete an active build.</span></span> <span data-ttu-id="21ca5-1275">hello modell lehet frissíteni a tooa különböző aktív build törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1275">hello model should be updated tooa different active build before you delete it.</span></span><br><span data-ttu-id="21ca5-1276">Egy folyamatban lévő build nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1276">You cannot delete an in-progress build.</span></span> <span data-ttu-id="21ca5-1277">Törölnie kell hello build először meghívásával <strong>Szerkesztés megszakítása</strong>.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1277">You should cancel hello build first by calling <strong>Cancel Build</strong>.</span></span>

| <span data-ttu-id="21ca5-1278">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1278">HTTP Method</span></span> | <span data-ttu-id="21ca5-1279">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1279">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1280">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-1280">DELETE</span></span> |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1281">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1281">Example:</span></span><br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1282">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1282">Parameter Name</span></span> | <span data-ttu-id="21ca5-1283">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1283">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1284">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1284">buildId</span></span> |<span data-ttu-id="21ca5-1285">Hello build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1285">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="21ca5-1286">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1286">apiVersion</span></span> |<span data-ttu-id="21ca5-1287">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1287">1.0</span></span> |

<span data-ttu-id="21ca5-1288">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1288">**Response:**</span></span>

<span data-ttu-id="21ca5-1289">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1289">HTTP Status code: 200</span></span>

### <a name="117-cancel-build"></a><span data-ttu-id="21ca5-1290">11.7.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1290">11.7.</span></span> <span data-ttu-id="21ca5-1291">Szakítsa meg a Build</span><span class="sxs-lookup"><span data-stu-id="21ca5-1291">Cancel Build</span></span>
<span data-ttu-id="21ca5-1292">Állapot fejlesztése során build visszavonása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1292">Cancels a build that is in building status.</span></span>

| <span data-ttu-id="21ca5-1293">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1293">HTTP Method</span></span> | <span data-ttu-id="21ca5-1294">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1294">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1295">A PUT</span><span class="sxs-lookup"><span data-stu-id="21ca5-1295">PUT</span></span> |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1296">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1296">Example:</span></span><br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1297">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1297">Parameter Name</span></span> | <span data-ttu-id="21ca5-1298">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1298">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1299">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1299">buildId</span></span> |<span data-ttu-id="21ca5-1300">Hello build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1300">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="21ca5-1301">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1301">apiVersion</span></span> |<span data-ttu-id="21ca5-1302">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1302">1.0</span></span> |

<span data-ttu-id="21ca5-1303">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1303">**Response:**</span></span>

<span data-ttu-id="21ca5-1304">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1304">HTTP Status code: 200</span></span>

### <a name="118-get-build-parameters"></a><span data-ttu-id="21ca5-1305">11.8.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1305">11.8.</span></span> <span data-ttu-id="21ca5-1306">Build paraméterek lekérése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1306">Get Build Parameters</span></span>
<span data-ttu-id="21ca5-1307">Lekéri a paraméterek felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1307">Retrieves build parameters.</span></span>

| <span data-ttu-id="21ca5-1308">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1308">HTTP Method</span></span> | <span data-ttu-id="21ca5-1309">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1309">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1310">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1310">GET</span></span> |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1311">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1311">Example:</span></span><br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1312">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1312">Parameter Name</span></span> | <span data-ttu-id="21ca5-1313">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1313">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1314">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1314">buildId</span></span> |<span data-ttu-id="21ca5-1315">Hello build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1315">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="21ca5-1316">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1316">apiVersion</span></span> |<span data-ttu-id="21ca5-1317">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1317">1.0</span></span> |

<span data-ttu-id="21ca5-1318">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1318">**Response:**</span></span>

<span data-ttu-id="21ca5-1319">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1319">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1320">Ez az API a kulcs/érték elemgyűjtemény adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1320">This API returns a collection of key/value elements.</span></span> <span data-ttu-id="21ca5-1321">Minden elem egy paraméter és az érték jelöli:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1321">Each element represents a parameter and its value:</span></span>

* <span data-ttu-id="21ca5-1322">`feed/entry/content/properties/Key`-Build paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1322">`feed/entry/content/properties/Key` - Build parameter name.</span></span>
* <span data-ttu-id="21ca5-1323">`feed/entry/content/properties/Value`-Build paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1323">`feed/entry/content/properties/Value` - Build parameter value.</span></span>

<span data-ttu-id="21ca5-1324">az alábbi táblázat hello hello szám, amely minden kulcs ábrázol.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1324">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="21ca5-1325">Kulcs</span><span class="sxs-lookup"><span data-stu-id="21ca5-1325">Key</span></span> | <span data-ttu-id="21ca5-1326">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1326">Description</span></span> | <span data-ttu-id="21ca5-1327">Típus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1327">Type</span></span> | <span data-ttu-id="21ca5-1328">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="21ca5-1328">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="21ca5-1329">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="21ca5-1329">NumberOfModelIterations</span></span> |<span data-ttu-id="21ca5-1330">hello modell hajtja végre az ismétlések száma hello hello jelzi a teljes számítási és hello modell pontosságának.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1330">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="21ca5-1331">hello magasabb hello számát, nagyobb pontosságot hello elérhetővé válik, de hello számítási idő hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1331">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="21ca5-1332">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1332">Integer</span></span> |<span data-ttu-id="21ca5-1333">10-50</span><span class="sxs-lookup"><span data-stu-id="21ca5-1333">10-50</span></span> |
| <span data-ttu-id="21ca5-1334">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="21ca5-1334">NumberOfModelDimensions</span></span> |<span data-ttu-id="21ca5-1335">a dimenziók száma hello vonatkozik toohello száma "szolgáltatások" hello modell megpróbál toofind belül az adatokat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1335">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="21ca5-1336">Hello dimenziószámának növelése lehetővé teszi a nagyobb pontosabb beállításra hello eredmények kisebb csoportokba.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1336">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="21ca5-1337">Azonban túl sok dimenzióval megakadályozza hello modell elemek közötti összefüggések keresése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1337">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="21ca5-1338">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1338">Integer</span></span> |<span data-ttu-id="21ca5-1339">10-40</span><span class="sxs-lookup"><span data-stu-id="21ca5-1339">10-40</span></span> |
| <span data-ttu-id="21ca5-1340">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1340">ItemCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-1341">Hello elem alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1341">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1342">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1342">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1343">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1343">Integer</span></span> |<span data-ttu-id="21ca5-1344">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1344">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1345">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1345">ItemCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-1346">Elem felső korlátja hello hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1346">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1347">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1347">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1348">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1348">Integer</span></span> |<span data-ttu-id="21ca5-1349">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1349">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1350">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1350">UserCutOffLowerBound</span></span> |<span data-ttu-id="21ca5-1351">Hello felhasználói alsó határa hello hűtő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1351">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1352">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1352">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1353">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1353">Integer</span></span> |<span data-ttu-id="21ca5-1354">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1354">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1355">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="21ca5-1355">UserCutOffUpperBound</span></span> |<span data-ttu-id="21ca5-1356">Meghatározza a hello hűtő hello felhasználói felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1356">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="21ca5-1357">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1357">See usage condenser above.</span></span> |<span data-ttu-id="21ca5-1358">Egész szám</span><span class="sxs-lookup"><span data-stu-id="21ca5-1358">Integer</span></span> |<span data-ttu-id="21ca5-1359">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1359">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="21ca5-1360">Leírás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1360">Description</span></span> |<span data-ttu-id="21ca5-1361">Build leírása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1361">Build description.</span></span> |<span data-ttu-id="21ca5-1362">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1362">String</span></span> |<span data-ttu-id="21ca5-1363">A szöveg, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="21ca5-1363">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="21ca5-1364">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="21ca5-1364">EnableModelingInsights</span></span> |<span data-ttu-id="21ca5-1365">Lehetővé teszi toocompute metrikák hello javaslat modellen.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1365">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="21ca5-1366">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1366">Boolean</span></span> |<span data-ttu-id="21ca5-1367">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1367">True/False</span></span> |
| <span data-ttu-id="21ca5-1368">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="21ca5-1368">UseFeaturesInModel</span></span> |<span data-ttu-id="21ca5-1369">Azt jelzi, ha használható-e a szolgáltatások rendelés tooenhance hello javaslat modellben.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1369">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="21ca5-1370">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1370">Boolean</span></span> |<span data-ttu-id="21ca5-1371">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1371">True/False</span></span> |
| <span data-ttu-id="21ca5-1372">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="21ca5-1372">ModelingFeatureList</span></span> |<span data-ttu-id="21ca5-1373">Szolgáltatás neve toobe hello ajánlás buildverziót, sorrendben tooenhance hello javaslat használt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1373">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="21ca5-1374">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1374">String</span></span> |<span data-ttu-id="21ca5-1375">Funkcióneveket too512 karakter mentése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1375">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="21ca5-1376">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="21ca5-1376">AllowColdItemPlacement</span></span> |<span data-ttu-id="21ca5-1377">Azt jelzi, ha hello javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1377">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="21ca5-1378">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1378">Boolean</span></span> |<span data-ttu-id="21ca5-1379">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1379">True/False</span></span> |
| <span data-ttu-id="21ca5-1380">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="21ca5-1380">EnableFeatureCorrelation</span></span> |<span data-ttu-id="21ca5-1381">Azt jelzi, ha szolgáltatások indoklást is használható.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1381">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="21ca5-1382">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="21ca5-1382">Boolean</span></span> |<span data-ttu-id="21ca5-1383">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1383">True/False</span></span> |
| <span data-ttu-id="21ca5-1384">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="21ca5-1384">ReasoningFeatureList</span></span> |<span data-ttu-id="21ca5-1385">Szolgáltatás neve toobe mintafelismerési mondat (pl. javaslat magyarázatokat) használt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1385">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="21ca5-1386">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21ca5-1386">String</span></span> |<span data-ttu-id="21ca5-1387">Funkcióneveket too512 karakter mentése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1387">Feature names, up too512 chars</span></span> |

<span data-ttu-id="21ca5-1388">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1388">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a><span data-ttu-id="21ca5-1389">12. Ajánlás</span><span class="sxs-lookup"><span data-stu-id="21ca5-1389">12. Recommendation</span></span>
### <a name="121-get-item-recommendations-for-active-build"></a><span data-ttu-id="21ca5-1390">12.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1390">12.1.</span></span> <span data-ttu-id="21ca5-1391">Elem javaslatok beolvasása (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1391">Get Item Recommendations (for active build)</span></span>
<span data-ttu-id="21ca5-1392">Hello aktív build típusú javaslatok beszerzése "Ajánlás" vagy "Fbt" mag (bemeneti) elemek listája alapján.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1392">Get recommendations of hello active build of type "Recommendation" or "Fbt" based on a list of seeds (input) items.</span></span>

| <span data-ttu-id="21ca5-1393">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1393">HTTP Method</span></span> | <span data-ttu-id="21ca5-1394">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1394">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1395">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1395">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1396">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1396">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1397">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1397">Parameter Name</span></span> | <span data-ttu-id="21ca5-1398">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1398">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1399">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1399">modelId</span></span> |<span data-ttu-id="21ca5-1400">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1400">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1401">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="21ca5-1401">itemIds</span></span> |<span data-ttu-id="21ca5-1402">Hello elemek toorecommend a vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1402">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="21ca5-1403">Ha hello aktív build küldhet csak egy elemet, majd írja be FBT.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1403">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="21ca5-1404">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1404">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1405">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1405">numberOfResults</span></span> |<span data-ttu-id="21ca5-1406">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1406">Number of required results</span></span> <br> <span data-ttu-id="21ca5-1407">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="21ca5-1407">Max: 150</span></span> |
| <span data-ttu-id="21ca5-1408">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1408">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1409">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1409">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1410">apiVersion</span></span> |<span data-ttu-id="21ca5-1411">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1411">1.0</span></span> |

<span data-ttu-id="21ca5-1412">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1412">**Response:**</span></span>

<span data-ttu-id="21ca5-1413">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1413">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1414">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1414">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1415">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1415">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1416">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1416">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1417">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1417">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1418">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1418">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1419">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1419">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1420">az alábbi hello példa egy válasz 10 ajánlott elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1420">hello example response below includes 10 recommended items.</span></span>

<span data-ttu-id="21ca5-1421">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1421">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

### <a name="122-get-item-recommendations-of-a-specific-build"></a><span data-ttu-id="21ca5-1422">12.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1422">12.2.</span></span> <span data-ttu-id="21ca5-1423">(Az adott build) elem javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1423">Get Item Recommendations (of a specific build)</span></span>
<span data-ttu-id="21ca5-1424">Egy adott build "Ajánlás" vagy "Fbt" típusú javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1424">Get recommendations of a specific build of type "Recommendation" or "Fbt".</span></span>

| <span data-ttu-id="21ca5-1425">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1425">HTTP Method</span></span> | <span data-ttu-id="21ca5-1426">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1426">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1427">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1427">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1428">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1428">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1429">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1429">Parameter Name</span></span> | <span data-ttu-id="21ca5-1430">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1430">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1431">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1431">modelId</span></span> |<span data-ttu-id="21ca5-1432">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1432">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1433">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="21ca5-1433">itemIds</span></span> |<span data-ttu-id="21ca5-1434">Hello elemek toorecommend a vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1434">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="21ca5-1435">Ha hello aktív build küldhet csak egy elemet, majd írja be FBT.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1435">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="21ca5-1436">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1436">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1437">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1437">numberOfResults</span></span> |<span data-ttu-id="21ca5-1438">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1438">Number of required results</span></span> <br> <span data-ttu-id="21ca5-1439">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="21ca5-1439">Max: 150</span></span> |
| <span data-ttu-id="21ca5-1440">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1440">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1441">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1441">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1442">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1442">buildId</span></span> |<span data-ttu-id="21ca5-1443">hello azonosító toouse a javaslat kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1443">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="21ca5-1444">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1444">apiVersion</span></span> |<span data-ttu-id="21ca5-1445">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1445">1.0</span></span> |

<span data-ttu-id="21ca5-1446">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1446">**Response:**</span></span>

<span data-ttu-id="21ca5-1447">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1447">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1448">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1448">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1449">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1449">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1450">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1450">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1451">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1451">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1452">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1452">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1453">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1453">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1454">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="21ca5-1454">See a response example in 12.1</span></span>

### <a name="123-get-fbt-recommendations-for-active-build"></a><span data-ttu-id="21ca5-1455">12.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1455">12.3.</span></span> <span data-ttu-id="21ca5-1456">(Az aktív build) FBT javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1456">Get FBT Recommendations (for active build)</span></span>
<span data-ttu-id="21ca5-1457">Tekintse át a javasolt hello aktív build típusú, "Fbt" alapján a kezdőérték (bemeneti) elemet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1457">Get recommendations of hello active build of type "Fbt" based on a seed (input) item.</span></span>

| <span data-ttu-id="21ca5-1458">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1458">HTTP Method</span></span> | <span data-ttu-id="21ca5-1459">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1459">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1460">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1460">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1461">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1461">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1462">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1462">Parameter Name</span></span> | <span data-ttu-id="21ca5-1463">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1463">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1464">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1464">modelId</span></span> |<span data-ttu-id="21ca5-1465">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1465">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1466">itemId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1466">itemId</span></span> |<span data-ttu-id="21ca5-1467">A cikk toorecommend.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1467">Item toorecommend for.</span></span> <br><span data-ttu-id="21ca5-1468">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1468">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1469">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1469">numberOfResults</span></span> |<span data-ttu-id="21ca5-1470">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1470">Number of required results</span></span> <br><span data-ttu-id="21ca5-1471">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="21ca5-1471">Max: 150</span></span> |
| <span data-ttu-id="21ca5-1472">minimalScore</span><span class="sxs-lookup"><span data-stu-id="21ca5-1472">minimalScore</span></span> |<span data-ttu-id="21ca5-1473">Egy gyakori készlet kell vissza kellett volna a hello szereplő sorrendben toobe minimális pontszámot annak az eredménye</span><span class="sxs-lookup"><span data-stu-id="21ca5-1473">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="21ca5-1474">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1474">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1475">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1475">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1476">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1476">apiVersion</span></span> |<span data-ttu-id="21ca5-1477">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1477">1.0</span></span> |

<span data-ttu-id="21ca5-1478">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1478">**Response:**</span></span>

<span data-ttu-id="21ca5-1479">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1479">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1480">hello válasz egy tételt ajánlott elem beállítása (általában hello kezdőérték/bemeneti elemszintű együtt vannak vásárolt elemek készlete) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1480">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="21ca5-1481">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1481">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1482">`Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1482">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1483">`Feed\entry\content\properties\Name1`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1483">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1484">`Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1484">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="21ca5-1485">`Feed\entry\content\properties\Name2`– 2. (nem kötelező) hello-elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1485">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="21ca5-1486">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1486">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1487">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1487">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1488">az alábbi hello példa egy válasz 3 ajánlott elem beállítása magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1488">hello example response below includes 3 recommended item sets.</span></span>

<span data-ttu-id="21ca5-1489">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1489">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a><span data-ttu-id="21ca5-1490">12.4.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1490">12.4.</span></span> <span data-ttu-id="21ca5-1491">(Az adott build) FBT javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1491">Get FBT Recommendations (of a specific build)</span></span>
<span data-ttu-id="21ca5-1492">Egy adott build "Fbt" típusú javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1492">Get recommendations of a specific build of type "Fbt".</span></span>

| <span data-ttu-id="21ca5-1493">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1493">HTTP Method</span></span> | <span data-ttu-id="21ca5-1494">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1495">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1495">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1496">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1496">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1497">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1497">Parameter Name</span></span> | <span data-ttu-id="21ca5-1498">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1499">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1499">modelId</span></span> |<span data-ttu-id="21ca5-1500">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1501">itemId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1501">itemId</span></span> |<span data-ttu-id="21ca5-1502">A cikk toorecommend.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1502">Item toorecommend for.</span></span> <br><span data-ttu-id="21ca5-1503">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1503">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1504">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1504">numberOfResults</span></span> |<span data-ttu-id="21ca5-1505">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1505">Number of required results</span></span> <br><span data-ttu-id="21ca5-1506">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="21ca5-1506">Max: 150</span></span> |
| <span data-ttu-id="21ca5-1507">minimalScore</span><span class="sxs-lookup"><span data-stu-id="21ca5-1507">minimalScore</span></span> |<span data-ttu-id="21ca5-1508">Egy gyakori készlet kell vissza kellett volna a hello szereplő sorrendben toobe minimális pontszámot annak az eredménye</span><span class="sxs-lookup"><span data-stu-id="21ca5-1508">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="21ca5-1509">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1509">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1510">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1510">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1511">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1511">buildId</span></span> |<span data-ttu-id="21ca5-1512">hello azonosító toouse a javaslat kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1512">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="21ca5-1513">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1513">apiVersion</span></span> |<span data-ttu-id="21ca5-1514">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1514">1.0</span></span> |

<span data-ttu-id="21ca5-1515">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1515">**Response:**</span></span>

<span data-ttu-id="21ca5-1516">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1516">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1517">hello válasz egy tételt ajánlott elem beállítása (általában hello kezdőérték/bemeneti elemszintű együtt vannak vásárolt elemek készlete) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1517">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="21ca5-1518">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1518">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1519">`Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1519">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1520">`Feed\entry\content\properties\Name1`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1520">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1521">`Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1521">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="21ca5-1522">`Feed\entry\content\properties\Name2`– 2. (nem kötelező) hello-elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1522">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="21ca5-1523">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1523">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1524">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1524">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1525">A válasz példa a 12.3</span><span class="sxs-lookup"><span data-stu-id="21ca5-1525">See a response example in 12.3</span></span>

### <a name="125-get-user-recommendations-for-active-build"></a><span data-ttu-id="21ca5-1526">12.5.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1526">12.5.</span></span> <span data-ttu-id="21ca5-1527">Felhasználói javaslatok beolvasása (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1527">Get User Recommendations (for active build)</span></span>
<span data-ttu-id="21ca5-1528">A build "Ajánlás" jelölésű aktív build típusú felhasználói javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1528">Get user recommendations of a build of type "Recommendation" marked as active build.</span></span>

<span data-ttu-id="21ca5-1529">hello API hello felhasználó toohello használati előzményei alapján előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1529">hello API will return a list of predicted item according toohello usage history of hello user.</span></span>

<span data-ttu-id="21ca5-1530">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1530">Notes:</span></span> 

1. <span data-ttu-id="21ca5-1531">Nincs felhasználói javaslat FBT buildjéhez van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1531">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="21ca5-1532">Ha hello active van FBT ezt a módszert fog hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1532">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="21ca5-1533">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1533">HTTP Method</span></span> | <span data-ttu-id="21ca5-1534">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1534">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1535">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1535">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1536">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1536">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1537">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1537">Parameter Name</span></span> | <span data-ttu-id="21ca5-1538">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1538">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1539">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1539">modelId</span></span> |<span data-ttu-id="21ca5-1540">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1540">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1541">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="21ca5-1541">userId</span></span> |<span data-ttu-id="21ca5-1542">Hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1542">Unique identifier of hello user</span></span> |
| <span data-ttu-id="21ca5-1543">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1543">numberOfResults</span></span> |<span data-ttu-id="21ca5-1544">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1544">Number of required results</span></span> |
| <span data-ttu-id="21ca5-1545">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1545">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1546">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1546">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1547">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1547">apiVersion</span></span> |<span data-ttu-id="21ca5-1548">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1548">1.0</span></span> |

<span data-ttu-id="21ca5-1549">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1549">**Response:**</span></span>

<span data-ttu-id="21ca5-1550">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1550">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1551">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1551">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1552">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1552">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1553">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1553">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1554">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1554">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1555">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1555">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1556">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1556">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1557">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="21ca5-1557">See a response example in 12.1</span></span>

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a><span data-ttu-id="21ca5-1558">12.6.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1558">12.6.</span></span> <span data-ttu-id="21ca5-1559">Felhasználói javaslatok lekérdezni elemek listáját (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1559">Get User Recommendations with item list (for active build)</span></span>
<span data-ttu-id="21ca5-1560">A build "Ajánlás" jelölésű elemek bejegyzés további listája az aktív build típusú felhasználói javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1560">Get user recommendations of a build of type "Recommendation" marked as active build with an additional list of items</span></span>

<span data-ttu-id="21ca5-1561">hello API hello felhasználói és hello további megadott elemek toohello használati előzményei alapján előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1561">hello API will return a list of predicted item according toohello usage history of hello user and hello additional provided items.</span></span>

<span data-ttu-id="21ca5-1562">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1562">Notes:</span></span> 

1. <span data-ttu-id="21ca5-1563">Nincs felhasználói javaslat FBT buildjéhez van.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1563">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="21ca5-1564">Ha hello active van FBT ezt a módszert fog hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1564">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="21ca5-1565">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1565">HTTP Method</span></span> | <span data-ttu-id="21ca5-1566">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1566">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1567">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1567">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1568">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1568">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1569">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1569">Parameter Name</span></span> | <span data-ttu-id="21ca5-1570">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1570">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1571">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1571">modelId</span></span> |<span data-ttu-id="21ca5-1572">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1572">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1573">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="21ca5-1573">userId</span></span> |<span data-ttu-id="21ca5-1574">Hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1574">Unique identifier of hello user</span></span> |
| <span data-ttu-id="21ca5-1575">itemsIds</span><span class="sxs-lookup"><span data-stu-id="21ca5-1575">itemsIds</span></span> |<span data-ttu-id="21ca5-1576">Hello elemek toorecommend a vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1576">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="21ca5-1577">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1577">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1578">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1578">numberOfResults</span></span> |<span data-ttu-id="21ca5-1579">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1579">Number of required results</span></span> |
| <span data-ttu-id="21ca5-1580">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1580">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1581">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1581">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1582">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1582">apiVersion</span></span> |<span data-ttu-id="21ca5-1583">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1583">1.0</span></span> |

<span data-ttu-id="21ca5-1584">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1584">**Response:**</span></span>

<span data-ttu-id="21ca5-1585">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1585">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1586">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1586">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1587">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1587">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1588">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1588">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1589">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1589">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1590">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1590">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1591">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1591">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1592">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="21ca5-1592">See a response example in 12.1</span></span>

### <a name="127-get-user-recommendations--of-a-specific-build"></a><span data-ttu-id="21ca5-1593">12.7.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1593">12.7.</span></span> <span data-ttu-id="21ca5-1594">(Az adott build) felhasználói javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1594">Get User Recommendations  (of a specific build)</span></span>
<span data-ttu-id="21ca5-1595">Egy adott build "Ajánlás" típusú felhasználói javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1595">Get user recommendations of a specific build of type "Recommendation".</span></span>

<span data-ttu-id="21ca5-1596">hello API (hello adott build szerepel) hello felhasználó toohello használati előzményei alapján előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1596">hello API will return a list of predicted item according toohello usage history of hello user (used in hello specific build).</span></span>

<span data-ttu-id="21ca5-1597">Megjegyzés: Nincs FBT buildjéhez nincs felhasználói javaslat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1597">Note: There is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="21ca5-1598">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1598">HTTP Method</span></span> | <span data-ttu-id="21ca5-1599">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1599">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1600">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1600">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1601">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1601">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1602">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1602">Parameter Name</span></span> | <span data-ttu-id="21ca5-1603">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1603">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1604">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1604">modelId</span></span> |<span data-ttu-id="21ca5-1605">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1605">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1606">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="21ca5-1606">userId</span></span> |<span data-ttu-id="21ca5-1607">Hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1607">Unique identifier of hello user</span></span> |
| <span data-ttu-id="21ca5-1608">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1608">numberOfResults</span></span> |<span data-ttu-id="21ca5-1609">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1609">Number of required results</span></span> |
| <span data-ttu-id="21ca5-1610">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1610">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1611">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1611">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1612">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1612">buildId</span></span> |<span data-ttu-id="21ca5-1613">hello azonosító toouse a javaslat kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1613">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="21ca5-1614">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1614">apiVersion</span></span> |<span data-ttu-id="21ca5-1615">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1615">1.0</span></span> |

<span data-ttu-id="21ca5-1616">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1616">**Response:**</span></span>

<span data-ttu-id="21ca5-1617">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1617">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1618">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1618">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1619">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1619">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1620">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1620">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1621">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1621">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1622">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1622">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1623">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1623">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1624">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="21ca5-1624">See a response example in 12.1</span></span>

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a><span data-ttu-id="21ca5-1625">12.8.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1625">12.8.</span></span> <span data-ttu-id="21ca5-1626">Felhasználói javaslatok beszerzése elem listáját (adott build)</span><span class="sxs-lookup"><span data-stu-id="21ca5-1626">Get User Recommendations with item list (of a specific build)</span></span>
<span data-ttu-id="21ca5-1627">Egy adott build "Ajánlás" típusú felhasználói javaslatok és további elemek listáját hello kapják meg.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1627">Get user recommendations of a specific build of type "Recommendation" and hello list of additional items.</span></span>

<span data-ttu-id="21ca5-1628">hello API toohello használati előzmények hello felhasználó és hello további elemek listájának megfelelően előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1628">hello API will return a list of predicted item according toohello usage history of hello user and hello additional list of items.</span></span>

<span data-ttu-id="21ca5-1629">Megjegyzés: A Tthere nincs felhasználói javaslat FBT buildjéhez.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1629">Note: Tthere is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="21ca5-1630">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1630">HTTP Method</span></span> | <span data-ttu-id="21ca5-1631">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1631">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1632">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1632">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1633">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1633">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1634">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1634">Parameter Name</span></span> | <span data-ttu-id="21ca5-1635">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1635">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1636">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1636">modelId</span></span> |<span data-ttu-id="21ca5-1637">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1637">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1638">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="21ca5-1638">userId</span></span> |<span data-ttu-id="21ca5-1639">Hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1639">Unique identifier of hello user</span></span> |
| <span data-ttu-id="21ca5-1640">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="21ca5-1640">itemIds</span></span> |<span data-ttu-id="21ca5-1641">Hello elemek toorecommend a vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1641">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="21ca5-1642">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="21ca5-1642">Max length: 1024</span></span> |
| <span data-ttu-id="21ca5-1643">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="21ca5-1643">numberOfResults</span></span> |<span data-ttu-id="21ca5-1644">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="21ca5-1644">Number of required results</span></span> |
| <span data-ttu-id="21ca5-1645">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="21ca5-1645">includeMetatadata</span></span> |<span data-ttu-id="21ca5-1646">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="21ca5-1646">Future use, always false</span></span> |
| <span data-ttu-id="21ca5-1647">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1647">buildId</span></span> |<span data-ttu-id="21ca5-1648">hello azonosító toouse a javaslat kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="21ca5-1648">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="21ca5-1649">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1649">apiVersion</span></span> |<span data-ttu-id="21ca5-1650">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1650">1.0</span></span> |

<span data-ttu-id="21ca5-1651">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1651">**Response:**</span></span>

<span data-ttu-id="21ca5-1652">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1652">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1653">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1653">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1654">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1654">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1655">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1655">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1656">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1656">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1657">`Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1657">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="21ca5-1658">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1658">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="21ca5-1659">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="21ca5-1659">See a response example in 12.1</span></span>

## <a name="13-user-usage-history"></a><span data-ttu-id="21ca5-1660">13. A felhasználó használati előzményei</span><span class="sxs-lookup"><span data-stu-id="21ca5-1660">13. User Usage History</span></span>
<span data-ttu-id="21ca5-1661">Miután egy javaslat modell készült hello rendszer lehetővé teszi hello build használt tooretrieve hello felhasználók előzményei (cikkeket társított tooa adott felhasználó).</span><span class="sxs-lookup"><span data-stu-id="21ca5-1661">Once a recommendation model was built hello system will allow tooretrieve hello user history (items associated tooa specific user) used for hello build.</span></span>
<span data-ttu-id="21ca5-1662">Ez az API engedélyezése tooretrieve hello felhasználók előzményei</span><span class="sxs-lookup"><span data-stu-id="21ca5-1662">This API allow tooretrieve hello user history</span></span>

<span data-ttu-id="21ca5-1663">Megjegyzés: hello felhasználók előzményei érhető el jelenleg csak a javaslat buildek.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1663">Note: hello user history is currently available only for recommendation builds.</span></span>

### <a name="131-retrieve-user-history"></a><span data-ttu-id="21ca5-1664">13.1 beolvasása felhasználók előzményei</span><span class="sxs-lookup"><span data-stu-id="21ca5-1664">13.1 Retrieve user history</span></span>
<span data-ttu-id="21ca5-1665">Aktív hello elemének beolvasása hello listája összeállítását, vagy a megadott hello hello megadott felhasználói azonosító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1665">Retrieve hello list of item used in hello active build or in hello specified build for hello given user id.</span></span>

| <span data-ttu-id="21ca5-1666">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1666">HTTP Method</span></span> | <span data-ttu-id="21ca5-1667">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1667">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1668">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1668">GET</span></span> |<span data-ttu-id="21ca5-1669">Hello aktív build hello felhasználók előzményei beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1669">Get hello user history for hello active build.</span></span><br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/><span data-ttu-id="21ca5-1670">Build megadott hello hello felhasználók előzményei lekérése`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1670">Get hello user history for hello given build `<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span></span><br/><br/><span data-ttu-id="21ca5-1671">Példa:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span><span class="sxs-lookup"><span data-stu-id="21ca5-1671">Example:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span></span> |

| <span data-ttu-id="21ca5-1672">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1672">Parameter Name</span></span> | <span data-ttu-id="21ca5-1673">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1673">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1674">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1674">modelId</span></span> |<span data-ttu-id="21ca5-1675">hello hello modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1675">hello unique identifier of hello model.</span></span> |
| <span data-ttu-id="21ca5-1676">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="21ca5-1676">userId</span></span> |<span data-ttu-id="21ca5-1677">hello hello felhasználó egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1677">hello unique identifier of hello user.</span></span> |
| <span data-ttu-id="21ca5-1678">buildId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1678">buildId</span></span> |<span data-ttu-id="21ca5-1679">nem kötelező paraméter, mely build a hello felhasználók előzményei kell fetch tooindicate engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1679">optional parameter, allow tooindicate from which build hello user history should be fetch</span></span> |
| <span data-ttu-id="21ca5-1680">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1680">apiVersion</span></span> |<span data-ttu-id="21ca5-1681">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1681">1.0</span></span> |

<span data-ttu-id="21ca5-1682">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1682">**Response:**</span></span>

<span data-ttu-id="21ca5-1683">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1683">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1684">hello válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1684">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="21ca5-1685">Mindegyik bejegyzés rendelkezik hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1685">Each entry has hello following data:</span></span>

* <span data-ttu-id="21ca5-1686">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1686">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="21ca5-1687">`Feed\entry\content\properties\Name`-Hello elem nevét.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1687">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="21ca5-1688">`Feed\entry\content\properties\Rating`– NEM ÁLL RENDELKEZÉSRE.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1688">`Feed\entry\content\properties\Rating` - N/A.</span></span>
* <span data-ttu-id="21ca5-1689">`Feed\entry\content\properties\Reasoning`– NEM ÁLL RENDELKEZÉSRE.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1689">`Feed\entry\content\properties\Reasoning` - N/A.</span></span>

<span data-ttu-id="21ca5-1690">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1690">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a><span data-ttu-id="21ca5-1691">14. Értesítések</span><span class="sxs-lookup"><span data-stu-id="21ca5-1691">14. Notifications</span></span>
<span data-ttu-id="21ca5-1692">Az Azure Machine Learning javaslatok létrehozza értesítések, amikor állandó hiba fordul elő hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1692">Azure Machine Learning Recommendations creates notifications when persistent errors happen in hello system.</span></span> <span data-ttu-id="21ca5-1693">Az értesítések 3 típusa van:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1693">There are 3 types of notifications:</span></span>

1. <span data-ttu-id="21ca5-1694">Build hiba – ezt az értesítést akkor váltódik ki, minden összeállítási hiba.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1694">Build failure - This notification is triggered for every build failure.</span></span>
2. <span data-ttu-id="21ca5-1695">Hiba – az értesítés feldolgozása adatgyűjtést lesz kiváltva, ha tudunk 100-nál több hibák hello az utolsó 5 perc, a használati események száma modell hello feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1695">Data acquisition processing failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of usage events per model.</span></span>
3. <span data-ttu-id="21ca5-1696">Javaslat fogyasztás hiba – ezt az értesítést lesz kiváltva, ha tudunk 100-nál több hibák hello az ajánlás kérelmek / modell hello feldolgozása az utolsó 5 perc.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1696">Recommendation consumption failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of recommendation requests per model.</span></span>

### <a name="141-get-notifications"></a><span data-ttu-id="21ca5-1697">14.1.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1697">14.1.</span></span> <span data-ttu-id="21ca5-1698">Értesítéseket</span><span class="sxs-lookup"><span data-stu-id="21ca5-1698">Get Notifications</span></span>
<span data-ttu-id="21ca5-1699">Lekéri az összes vagy egy egyetlen modell minden hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1699">Retrieves all hello notifications for all models or for a single model.</span></span>

| <span data-ttu-id="21ca5-1700">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1700">HTTP Method</span></span> | <span data-ttu-id="21ca5-1701">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1701">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1702">GET</span><span class="sxs-lookup"><span data-stu-id="21ca5-1702">GET</span></span> |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1703">Első összes minden értesítés:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1703">Getting all notifications for all models:</span></span><br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1704">Példa egy adott modell értesítések kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1704">Example for getting notifications for a specific model:</span></span><br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| <span data-ttu-id="21ca5-1705">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1705">Parameter Name</span></span> | <span data-ttu-id="21ca5-1706">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1706">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1707">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1707">modelId</span></span> |<span data-ttu-id="21ca5-1708">Nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1708">Optional parameter.</span></span> <span data-ttu-id="21ca5-1709">Ha nincs megadva, az összes modell összes értesítéseket fog kapni.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1709">When omitted, you will get all notifications for all models.</span></span> <br><span data-ttu-id="21ca5-1710">Érvénytelen érték: hello modell egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1710">Valid value: unique identifier of hello model.</span></span> |
| <span data-ttu-id="21ca5-1711">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1711">apiVersion</span></span> |<span data-ttu-id="21ca5-1712">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1712">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-1713">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-1713">Request Body</span></span> |<span data-ttu-id="21ca5-1714">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-1714">NONE</span></span> |

<span data-ttu-id="21ca5-1715">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="21ca5-1715">**Response:**</span></span>

<span data-ttu-id="21ca5-1716">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1716">HTTP Status code: 200</span></span>

<span data-ttu-id="21ca5-1717">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="21ca5-1717">OData XML</span></span>

    hello response includes one entry per notification. Each entry has hello following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a><span data-ttu-id="21ca5-1718">14.2.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1718">14.2.</span></span> <span data-ttu-id="21ca5-1719">Modell értesítések törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1719">Delete Model Notifications</span></span>
<span data-ttu-id="21ca5-1720">Törli az összes olvasási értesítések levő modell esetén.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1720">Deletes all read notifications for a model.</span></span>

| <span data-ttu-id="21ca5-1721">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1721">HTTP Method</span></span> | <span data-ttu-id="21ca5-1722">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1722">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1723">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-1723">DELETE</span></span> |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="21ca5-1724">Példa:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1724">Example:</span></span><br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1725">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1725">Parameter Name</span></span> | <span data-ttu-id="21ca5-1726">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1726">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1727">modelId</span><span class="sxs-lookup"><span data-stu-id="21ca5-1727">modelId</span></span> |<span data-ttu-id="21ca5-1728">Hello modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="21ca5-1728">Unique identifier of hello model</span></span> |
| <span data-ttu-id="21ca5-1729">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1729">apiVersion</span></span> |<span data-ttu-id="21ca5-1730">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1730">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-1731">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-1731">Request Body</span></span> |<span data-ttu-id="21ca5-1732">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-1732">NONE</span></span> |

<span data-ttu-id="21ca5-1733">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1733">**Response**:</span></span>

<span data-ttu-id="21ca5-1734">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1734">HTTP Status code: 200</span></span>

### <a name="143-delete-user-notifications"></a><span data-ttu-id="21ca5-1735">14.3.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1735">14.3.</span></span> <span data-ttu-id="21ca5-1736">Felhasználó értesítései törlése</span><span class="sxs-lookup"><span data-stu-id="21ca5-1736">Delete User Notifications</span></span>
<span data-ttu-id="21ca5-1737">Törli az összes minden értesítést.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1737">Deletes all notifications for all models.</span></span>

| <span data-ttu-id="21ca5-1738">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="21ca5-1738">HTTP Method</span></span> | <span data-ttu-id="21ca5-1739">URI</span><span class="sxs-lookup"><span data-stu-id="21ca5-1739">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1740">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="21ca5-1740">DELETE</span></span> |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| <span data-ttu-id="21ca5-1741">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="21ca5-1741">Parameter Name</span></span> | <span data-ttu-id="21ca5-1742">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="21ca5-1742">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="21ca5-1743">apiVersion</span><span class="sxs-lookup"><span data-stu-id="21ca5-1743">apiVersion</span></span> |<span data-ttu-id="21ca5-1744">1.0</span><span class="sxs-lookup"><span data-stu-id="21ca5-1744">1.0</span></span> |
|  | |
| <span data-ttu-id="21ca5-1745">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="21ca5-1745">Request Body</span></span> |<span data-ttu-id="21ca5-1746">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="21ca5-1746">NONE</span></span> |

<span data-ttu-id="21ca5-1747">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="21ca5-1747">**Response**:</span></span>

<span data-ttu-id="21ca5-1748">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="21ca5-1748">HTTP Status code: 200</span></span>

## <a name="15-legal"></a><span data-ttu-id="21ca5-1749">15. Jogi tudnivalók</span><span class="sxs-lookup"><span data-stu-id="21ca5-1749">15. Legal</span></span>
<span data-ttu-id="21ca5-1750">Ez a dokumentum biztosított ",-van".</span><span class="sxs-lookup"><span data-stu-id="21ca5-1750">This document is provided “as-is”.</span></span> <span data-ttu-id="21ca5-1751">Információk és nézetek ebben a dokumentumban, beleértve az URL-cím és az egyéb webhelyhivatkozásokat, értesítés nélkül változhatnak.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1751">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span><br><br>
<span data-ttu-id="21ca5-1752">A felhasznált példák némelyike csak illusztrációs célokat szolgálnak, és kitalált esetet szemléltet.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1752">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="21ca5-1753">Nincs valós association vagy a kapcsolat célja, vagy eseményekkel.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1753">No real association or connection is intended or should be inferred.</span></span><br><br>
<span data-ttu-id="21ca5-1754">Ez a dokumentum nem biztosít semmilyen jogot tooany semmilyen Microsoft-termékben található szellemi tulajdonhoz.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1754">This document does not provide you with any legal rights tooany intellectual property in any Microsoft product.</span></span> <span data-ttu-id="21ca5-1755">Előfordulhat, hogy másolja és használja a dokumentum belső használatra, tájékoztatási céllal.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1755">You may copy and use this document for your internal, reference purposes.</span></span><br><br>
<span data-ttu-id="21ca5-1756">© 2015 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1756">© 2015 Microsoft.</span></span> <span data-ttu-id="21ca5-1757">Minden jog fenntartva.</span><span class="sxs-lookup"><span data-stu-id="21ca5-1757">All rights reserved.</span></span>

