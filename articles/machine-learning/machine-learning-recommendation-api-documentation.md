---
title: "Számítógép tanulási javaslatok API-dokumentáció |} Microsoft Docs"
description: "Egy a Microsoft Azure piactéren elérhető javaslatok motor Azure Machine Learning javaslatok API dokumentációjában."
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
redirect_document_id: TRUE
ms.openlocfilehash: 1fba64d78d779344e2895b0d54419186b7584865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a><span data-ttu-id="180f5-103">Azure Machine Learning Recommendations – API-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="180f5-103">Azure Machine Learning Recommendations API Documentation</span></span>
> [!NOTE]
> <span data-ttu-id="180f5-104">El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="180f5-104">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="180f5-105">A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="180f5-105">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="180f5-106">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="180f5-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="180f5-107">További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="180f5-107">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="180f5-108">Ez a dokumentum a Microsoft Azure Machine Learning javaslatok API-k ábrázol.</span><span class="sxs-lookup"><span data-stu-id="180f5-108">This document depicts Microsoft Azure Machine Learning Recommendations APIs.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="180f5-109">1. Általános – áttekintés</span><span class="sxs-lookup"><span data-stu-id="180f5-109">1. General overview</span></span>
<span data-ttu-id="180f5-110">Ez a dokumentum egy API-hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="180f5-110">This document is an API reference.</span></span> <span data-ttu-id="180f5-111">Az "Azure Machine Learning javaslat – gyors üzembe" dokumentum kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="180f5-111">You should start with the “Azure Machine Learning Recommendation - Quick Start” document.</span></span>

<span data-ttu-id="180f5-112">Az Azure Machine Learning javaslatok API a következő logikai csoportokra osztható:</span><span class="sxs-lookup"><span data-stu-id="180f5-112">The Azure Machine Learning Recommendations API can be divided into the following logical groups:</span></span>

* <span data-ttu-id="180f5-113"><ins>Korlátozások</ins> -javaslatok API-korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="180f5-113"><ins>Limitations</ins> - Recommendations API limitations.</span></span>
* <span data-ttu-id="180f5-114"><ins>Általános információk</ins> -információkat a hitelesítés, szolgáltatás URI és versioning.</span><span class="sxs-lookup"><span data-stu-id="180f5-114"><ins>General Information</ins> - Information on authentication, service URI and versioning.</span></span>
* <span data-ttu-id="180f5-115"><ins>Basic modell</ins> -API-k, amelyek lehetővé teszik a modellt az alapvető műveleteket (pl. létrehozása, frissítése és modell törlése).</span><span class="sxs-lookup"><span data-stu-id="180f5-115"><ins>Model Basic</ins> - APIs that enable you to do the basic operations on model (e.g. create, update and delete a model).</span></span>
* <span data-ttu-id="180f5-116"><ins>A modell speciális</ins> -API-k, amelyek lehetővé teszik a modellen adatok insights speciális.</span><span class="sxs-lookup"><span data-stu-id="180f5-116"><ins>Model Advanced</ins> - APIs that enable you to get advanced data insights on the model.</span></span>
* <span data-ttu-id="180f5-117"><ins>A modell az üzleti szabályok</ins> -API-k, amelyek lehetővé teszik, hogy a modell javaslat eredmények üzleti szabályok kezelése.</span><span class="sxs-lookup"><span data-stu-id="180f5-117"><ins>Model Business Rules</ins> - APIs that enable you to manage business rules on the model recommendation results.</span></span>
* <span data-ttu-id="180f5-118"><ins>Katalógus</ins> -API-k, amelyek lehetővé teszik a modell katalógus alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="180f5-118"><ins>Catalog</ins> - APIs that enable you to do basic operations on a model catalog.</span></span> <span data-ttu-id="180f5-119">A katalógus metaadat-információkat a használati adatok elemek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-119">A catalog contains metadata information on the items of the usage data.</span></span>
* <span data-ttu-id="180f5-120"><ins>A szolgáltatás</ins> -API-k, amelyek lehetővé teszik elemen insights feltölti a katalógus és hogyan lehet ezt az információt jobb javaslatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="180f5-120"><ins>Feature</ins> - APIs that enable to get insights on item into the catalog and how to use this information to build better recommendations.</span></span>
* <span data-ttu-id="180f5-121"><ins>Használati adatok</ins> -API-k, amelyek lehetővé teszik a modell használati adatok alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="180f5-121"><ins>Usage Data</ins> - APIs that enable you to do basic operations on the model usage data.</span></span> <span data-ttu-id="180f5-122">Használati adatok alapvető formájában pár &#60; userId &#62; tartalmazó sorok áll, &#60; itemId &#62;.</span><span class="sxs-lookup"><span data-stu-id="180f5-122">Usage data in the basic form consists of rows that include pairs of &#60;userId&#62;,&#60;itemId&#62;.</span></span>
* <span data-ttu-id="180f5-123"><ins>Build</ins> -API-k, amelyek lehetővé teszik, hogy a modell build indíthat, és a build kapcsolatos alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="180f5-123"><ins>Build</ins> - APIs that enable you to trigger a model build and do basic operations that are related to this build.</span></span> <span data-ttu-id="180f5-124">Ha elvégezte a értékes használati adatok modell build indíthat el.</span><span class="sxs-lookup"><span data-stu-id="180f5-124">You can trigger a model build once you have valuable usage data.</span></span>
* <span data-ttu-id="180f5-125"><ins>A javaslat</ins> -API-k, amelyek lehetővé teszik, hogy javaslatokat felhasználását, a build-modell leteltével.</span><span class="sxs-lookup"><span data-stu-id="180f5-125"><ins>Recommendation</ins> - APIs that enable you to consume recommendations once the build of a model ends.</span></span>
* <span data-ttu-id="180f5-126"><ins>Felhasználói adatok</ins> -API-k, amelyek lehetővé teszik az adatok beolvasása a felhasználó használati adatai.</span><span class="sxs-lookup"><span data-stu-id="180f5-126"><ins>User Data</ins> - APIs that enable you to fetch information on the user usage data.</span></span>
* <span data-ttu-id="180f5-127"><ins>Értesítések</ins> -API-k, amelyek lehetővé teszik a problémák értesítést szeretne kapni a API műveleteivel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="180f5-127"><ins>Notifications</ins> - APIs that enable you to receive notifications on problems related to your API operations.</span></span> <span data-ttu-id="180f5-128">(Például a használati adatok adatgyűjtést és az események feldolgozása sikertelen a legtöbb jelentik.</span><span class="sxs-lookup"><span data-stu-id="180f5-128">(For example, you are reporting usage data via Data Acquisition and most of the events processing are failing.</span></span> <span data-ttu-id="180f5-129">Egy hiba értesítést generál.)</span><span class="sxs-lookup"><span data-stu-id="180f5-129">An error notification will be raised.)</span></span>

## <a name="2-limitations"></a><span data-ttu-id="180f5-130">2. Korlátozások</span><span class="sxs-lookup"><span data-stu-id="180f5-130">2. Limitations</span></span>
* <span data-ttu-id="180f5-131">Előfizetésenként modellek maximális száma érték a 10.</span><span class="sxs-lookup"><span data-stu-id="180f5-131">The maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="180f5-132">Egyes modellek buildek legfeljebb 20.</span><span class="sxs-lookup"><span data-stu-id="180f5-132">The maximum number of builds per model is 20.</span></span>
* <span data-ttu-id="180f5-133">A katalógus rendelkező elemek maximális száma: 100 000.</span><span class="sxs-lookup"><span data-stu-id="180f5-133">The maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="180f5-134">Mindig használati pontok maximális számát ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="180f5-134">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="180f5-135">A legrégebbi esetén törlendő újakat feltöltött vagy fogja jelenteni.</span><span class="sxs-lookup"><span data-stu-id="180f5-135">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="180f5-136">A FELADÁS egy vagy több (pl. katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="180f5-136">The maximum size of data that can be sent in POST (e.g. import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="180f5-137">A maximális elemek száma, amelyek is meg kell adnia a javaslatok beolvasásakor 150.</span><span class="sxs-lookup"><span data-stu-id="180f5-137">The maximum number of items that can be asked for when getting recommendations is 150.</span></span>

## <a name="3-apis---general-information"></a><span data-ttu-id="180f5-138">3. API-k – általános információk</span><span class="sxs-lookup"><span data-stu-id="180f5-138">3. APIs - general information</span></span>
### <a name="31-authentication"></a><span data-ttu-id="180f5-139">3.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-139">3.1.</span></span> <span data-ttu-id="180f5-140">Authentication</span><span class="sxs-lookup"><span data-stu-id="180f5-140">Authentication</span></span>
<span data-ttu-id="180f5-141">Kérjük, kövesse a Microsoft Azure piactérről irányelveket hitelesítési kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="180f5-141">Please follow the Microsoft Azure Marketplace guidelines regarding authentication.</span></span> <span data-ttu-id="180f5-142">A piactér a Basic vagy az OAuth hitelesítési módszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="180f5-142">The marketplace supports either the Basic or OAuth authentication method.</span></span>

### <a name="32-service-uri"></a><span data-ttu-id="180f5-143">3.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-143">3.2.</span></span> <span data-ttu-id="180f5-144">Szolgáltatás URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-144">Service URI</span></span>
<span data-ttu-id="180f5-145">A szolgáltatás legfelső szintű URI-azonosítóját az Azure Machine Learning javaslatok API-k [itt.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span><span class="sxs-lookup"><span data-stu-id="180f5-145">The service root URI for the Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span></span>

<span data-ttu-id="180f5-146">A teljes szolgáltatás URI kifejezett elemek az OData specifikáció használatával.</span><span class="sxs-lookup"><span data-stu-id="180f5-146">The full service URI is expressed using elements of the OData specification.</span></span>  

### <a name="33-api-version"></a><span data-ttu-id="180f5-147">3.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-147">3.3.</span></span> <span data-ttu-id="180f5-148">API-verzió</span><span class="sxs-lookup"><span data-stu-id="180f5-148">API version</span></span>
<span data-ttu-id="180f5-149">Minden API-hívás végén, lesz egy lekérdezési paraméter apiVersion, amely 1.0 értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="180f5-149">Each API call will have, at the end, a query parameter called apiVersion that should be set to 1.0.</span></span>

### <a name="34-ids-are-case-sensitive"></a><span data-ttu-id="180f5-150">3.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-150">3.4.</span></span> <span data-ttu-id="180f5-151">Azonosítók különbözőnek számítanak a kis</span><span class="sxs-lookup"><span data-stu-id="180f5-151">IDs are case sensitive</span></span>
<span data-ttu-id="180f5-152">Azonosítók, az API-k által visszaadott különbözőnek számítanak a kis és kell használni, így amikor későbbi API-hívásokban paraméterként.</span><span class="sxs-lookup"><span data-stu-id="180f5-152">IDs, returned by any of the APIs, are case sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="180f5-153">Például modellazonosítóját és a katalógus azonosítók különbözőnek számítanak a kis.</span><span class="sxs-lookup"><span data-stu-id="180f5-153">For instance, model IDs and catalog IDs are case sensitive.</span></span>

## <a name="4-recommendations-quality-and-cold-items"></a><span data-ttu-id="180f5-154">4. Javaslatok minőségének és Cold elemek</span><span class="sxs-lookup"><span data-stu-id="180f5-154">4. Recommendations Quality and Cold Items</span></span>
### <a name="41-recommendation-quality"></a><span data-ttu-id="180f5-155">4.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-155">4.1.</span></span> <span data-ttu-id="180f5-156">A javaslat minősége</span><span class="sxs-lookup"><span data-stu-id="180f5-156">Recommendation quality</span></span>
<span data-ttu-id="180f5-157">A javaslat modellek létrehozásához általában van elegendő, a rendszer tudjon ajánlani.</span><span class="sxs-lookup"><span data-stu-id="180f5-157">Creating a recommendation model is usually enough to allow the system to provide recommendations.</span></span> <span data-ttu-id="180f5-158">Ettől függetlenül javaslat minőségi függ a használatát, feldolgozása és a katalógus körét.</span><span class="sxs-lookup"><span data-stu-id="180f5-158">Nevertheless, recommendation quality varies based on the usage processed and the coverage of the catalog.</span></span> <span data-ttu-id="180f5-159">Például ha sok cold elem (elem jelentős használata nélkül), a rendszer rendelkezik-e egy ilyen elem ajánlást megadása, vagy egy ilyen elem használata javasolt egy nehézségek.</span><span class="sxs-lookup"><span data-stu-id="180f5-159">For example if you have a lot of cold items (items without significant usage), the system will have difficulties providing a recommendation for such an item or using such an item as a recommended one.</span></span> <span data-ttu-id="180f5-160">Ahhoz, hogy a cold elem probléma megoldásához, a rendszer engedélyezi a metaadatok javítása érdekében a javaslatok elemek használatát.</span><span class="sxs-lookup"><span data-stu-id="180f5-160">In order to overcome the cold item problem, the system allows the use of metadata of the items to enhance the recommendations.</span></span> <span data-ttu-id="180f5-161">A metaadatok szolgáltatások nevezzük.</span><span class="sxs-lookup"><span data-stu-id="180f5-161">This metadata is referred to as features.</span></span> <span data-ttu-id="180f5-162">Tipikus szolgáltatásokat egy könyv szerző vagy egy movie szereplő.</span><span class="sxs-lookup"><span data-stu-id="180f5-162">Typical features are a book's author or a movie's actor.</span></span> <span data-ttu-id="180f5-163">A kulcs/érték karakterláncok formájában katalógus keresztül szolgáltatást kínál.</span><span class="sxs-lookup"><span data-stu-id="180f5-163">Features are provided via the catalog in the form of key/value strings.</span></span> <span data-ttu-id="180f5-164">A teljes fájl formátuma, a katalógus, tekintse meg a [katalógus szakasz importálása](#81-import-catalog-data).</span><span class="sxs-lookup"><span data-stu-id="180f5-164">For the full format of the catalog file, please refer to the [import catalog section](#81-import-catalog-data).</span></span> 

### <a name="42-rank-build"></a><span data-ttu-id="180f5-165">4.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-165">4.2.</span></span> <span data-ttu-id="180f5-166">Rangsorolt build</span><span class="sxs-lookup"><span data-stu-id="180f5-166">Rank build</span></span>
<span data-ttu-id="180f5-167">Szolgáltatások javíthatja a javaslat modell, de ehhez lekérdezhetik a fontos funkciók használatát igényli.</span><span class="sxs-lookup"><span data-stu-id="180f5-167">Features can enhance the recommendation model, but to do so requires the use of meaningful features.</span></span> <span data-ttu-id="180f5-168">Erre a célra egy új build bevezetett - build a sorrend első helyén.</span><span class="sxs-lookup"><span data-stu-id="180f5-168">For this purpose a new build was introduced - a rank build.</span></span> <span data-ttu-id="180f5-169">A build fog rangsorolja szolgáltatások használhatóságát.</span><span class="sxs-lookup"><span data-stu-id="180f5-169">This build will rank the usefulness of features.</span></span> <span data-ttu-id="180f5-170">Egy olyan jelentéssel bíró szolgáltatása, 2, illetve a hierarchiában felfelé a sorrendet megadó pontszám szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="180f5-170">A meaningful feature is a feature with a rank score of 2 and up.</span></span>
<span data-ttu-id="180f5-171">Után az ismertetése, a szolgáltatások ezek jelentéssel bíró, jelentéssel bíró szolgáltatások listája (vagy allista) javaslat build kiváltani.</span><span class="sxs-lookup"><span data-stu-id="180f5-171">After understanding which of the features are meaningful, trigger a recommendation build with the list (or sublist) of meaningful features.</span></span> <span data-ttu-id="180f5-172">E funkció használatát a fejlesztésen esett át a meleg elemek és a cold elemeket is.</span><span class="sxs-lookup"><span data-stu-id="180f5-172">It is possible to use these feature for the enhancement of both warm items and cold items.</span></span> <span data-ttu-id="180f5-173">Meleg elemek használatához a `UseFeatureInModel` build paramétert kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="180f5-173">In order to use them for warm items, the `UseFeatureInModel` build parameter should be set up.</span></span> <span data-ttu-id="180f5-174">Ahhoz, hogy szolgáltatások cold elemek, a `AllowColdItemPlacement` engedélyezni kell a build paramétert.</span><span class="sxs-lookup"><span data-stu-id="180f5-174">In order to use features for cold items, the `AllowColdItemPlacement` build parameter should be enabled.</span></span>
<span data-ttu-id="180f5-175">Megjegyzés: Nincs lehetőség engedélyezése `AllowColdItemPlacement` engedélyezése nélkül `UseFeatureInModel`.</span><span class="sxs-lookup"><span data-stu-id="180f5-175">Note: It is not possible to enable `AllowColdItemPlacement` without enabling `UseFeatureInModel`.</span></span>

### <a name="43-recommendation-reasoning"></a><span data-ttu-id="180f5-176">4.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-176">4.3.</span></span> <span data-ttu-id="180f5-177">A javaslat indoklást</span><span class="sxs-lookup"><span data-stu-id="180f5-177">Recommendation reasoning</span></span>
<span data-ttu-id="180f5-178">A javaslat indoklást funkciók használata szerepet játszó másik tényező.</span><span class="sxs-lookup"><span data-stu-id="180f5-178">Recommendation reasoning is another aspect of feature usage.</span></span> <span data-ttu-id="180f5-179">Ezenkívül az Azure Machine Learning javaslatok motor szolgáltatások segítségével javaslat magyarázattal (más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-179">Indeed, the Azure Machine Learning Recommendations engine can use features to provide recommendation explanations (a.k.a.</span></span> <span data-ttu-id="180f5-180">mintafelismerési), a javaslat fogyasztó vezető további abban, hogy a javasolt elemben.</span><span class="sxs-lookup"><span data-stu-id="180f5-180">reasoning), leading to more confidence in the recommended item from the recommendation consumer.</span></span>
<span data-ttu-id="180f5-181">Ahhoz, hogy indoklást, a `AllowFeatureCorrelation` és `ReasoningFeatureList` paraméterek beállítása előtt kérése ajánlás build kell lennie.</span><span class="sxs-lookup"><span data-stu-id="180f5-181">To enable reasoning, the `AllowFeatureCorrelation` and `ReasoningFeatureList` parameters should be setup prior to requesting a recommendation build.</span></span>

## <a name="5-model-basic"></a><span data-ttu-id="180f5-182">5. Basic modell</span><span class="sxs-lookup"><span data-stu-id="180f5-182">5. Model Basic</span></span>
### <a name="51-create-model"></a><span data-ttu-id="180f5-183">5.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-183">5.1.</span></span> <span data-ttu-id="180f5-184">Modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="180f5-184">Create Model</span></span>
<span data-ttu-id="180f5-185">Kérést hoz létre "létrehozása a modell".</span><span class="sxs-lookup"><span data-stu-id="180f5-185">Creates a “create model” request.</span></span>

| <span data-ttu-id="180f5-186">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-186">HTTP Method</span></span> | <span data-ttu-id="180f5-187">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-187">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-188">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-188">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-189">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-189">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-190">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-190">Parameter Name</span></span> | <span data-ttu-id="180f5-191">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-191">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-192">modelName</span><span class="sxs-lookup"><span data-stu-id="180f5-192">modelName</span></span> |<span data-ttu-id="180f5-193">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="180f5-193">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="180f5-194">Maximális hossz: 20</span><span class="sxs-lookup"><span data-stu-id="180f5-194">Max length: 20</span></span> |
| <span data-ttu-id="180f5-195">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-195">apiVersion</span></span> |<span data-ttu-id="180f5-196">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-196">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-197">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-197">Request Body</span></span> |<span data-ttu-id="180f5-198">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-198">NONE</span></span> |

<span data-ttu-id="180f5-199">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-199">**Response**:</span></span>

<span data-ttu-id="180f5-200">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-200">HTTP Status code: 200</span></span>

* <span data-ttu-id="180f5-201">`feed/entry/content/properties/id`-Tartalmazza a modell.</span><span class="sxs-lookup"><span data-stu-id="180f5-201">`feed/entry/content/properties/id` - Contains the model ID.</span></span>
  <span data-ttu-id="180f5-202">**Megjegyzés:**: Modellazonosító a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="180f5-202">**Note**: model ID is case sensitive.</span></span>

<span data-ttu-id="180f5-203">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-203">OData XML</span></span>

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

### <a name="52-get-model"></a><span data-ttu-id="180f5-204">5.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-204">5.2.</span></span> <span data-ttu-id="180f5-205">Modell lekérése</span><span class="sxs-lookup"><span data-stu-id="180f5-205">Get Model</span></span>
<span data-ttu-id="180f5-206">A "get-modell" kérést hoz.</span><span class="sxs-lookup"><span data-stu-id="180f5-206">Creates a “get model” request.</span></span>

| <span data-ttu-id="180f5-207">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-207">HTTP Method</span></span> | <span data-ttu-id="180f5-208">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-208">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-209">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-209">GET</span></span> |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-210">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-210">Example:</span></span><br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-211">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-211">Parameter Name</span></span> | <span data-ttu-id="180f5-212">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-212">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-213">id</span><span class="sxs-lookup"><span data-stu-id="180f5-213">id</span></span> |<span data-ttu-id="180f5-214">A modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-214">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="180f5-215">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-215">apiVersion</span></span> |<span data-ttu-id="180f5-216">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-216">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-217">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-217">Request Body</span></span> |<span data-ttu-id="180f5-218">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-218">NONE</span></span> |

<span data-ttu-id="180f5-219">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-219">**Response**:</span></span>

<span data-ttu-id="180f5-220">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-220">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-221">A modell adatai található a következő elemeket:</span><span class="sxs-lookup"><span data-stu-id="180f5-221">The model data can be found under the following elements:</span></span>

* <span data-ttu-id="180f5-222">`feed/entry/content/properties/Id`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-222">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="180f5-223">`feed/entry/content/properties/Name`-Modell neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-223">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="180f5-224">`feed/entry/content/properties/Date`-Modell létrehozásának dátumát.</span><span class="sxs-lookup"><span data-stu-id="180f5-224">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="180f5-225">`feed/entry/content/properties/Status`-Modell állapota.</span><span class="sxs-lookup"><span data-stu-id="180f5-225">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="180f5-226">A következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="180f5-226">One of the following:</span></span>
  * <span data-ttu-id="180f5-227">Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.</span><span class="sxs-lookup"><span data-stu-id="180f5-227">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="180f5-228">ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-228">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="180f5-229">`feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha a modell sikeresen lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="180f5-229">`feed/entry/content/properties/HasActiveBuild` - Indicates if the model was built successfully.</span></span>
* <span data-ttu-id="180f5-230">`feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-230">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="180f5-231">`feed/entry/content/properties/Mpr`-Modell átlagos PERCENTILIS rangsorolási (MPR - ModelInsight további információt talál).</span><span class="sxs-lookup"><span data-stu-id="180f5-231">`feed/entry/content/properties/Mpr` - Model mean percentile ranking (MPR - see ModelInsight for more information).</span></span>
* <span data-ttu-id="180f5-232">`feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="180f5-232">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>

<span data-ttu-id="180f5-233">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-233">OData XML</span></span>

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

### <a name="53----get-all-models"></a><span data-ttu-id="180f5-234">5.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-234">5.3.</span></span>    <span data-ttu-id="180f5-235">Összes modell lekérése</span><span class="sxs-lookup"><span data-stu-id="180f5-235">Get All Models</span></span>
<span data-ttu-id="180f5-236">Lekéri az összes modellt az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="180f5-236">Retrieves all models of the current user.</span></span>

| <span data-ttu-id="180f5-237">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-237">HTTP Method</span></span> | <span data-ttu-id="180f5-238">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-238">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-239">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-239">GET</span></span> |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br><span data-ttu-id="180f5-240">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-240">Example:</span></span><br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-241">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-241">Parameter Name</span></span> | <span data-ttu-id="180f5-242">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-242">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-243">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-243">apiVersion</span></span> |<span data-ttu-id="180f5-244">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-244">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-245">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-245">Request Body</span></span> |<span data-ttu-id="180f5-246">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-246">NONE</span></span> |

<span data-ttu-id="180f5-247">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-247">**Response**:</span></span>

<span data-ttu-id="180f5-248">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-248">HTTP Status code: 200</span></span>

* <span data-ttu-id="180f5-249">`feed/entry/content/properties/Id`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-249">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="180f5-250">`feed/entry/content/properties/Name`-Modell neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-250">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="180f5-251">`feed/entry/content/properties/Date`-Modell létrehozásának dátumát.</span><span class="sxs-lookup"><span data-stu-id="180f5-251">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="180f5-252">`feed/entry/content/properties/Status`-Modell állapota.</span><span class="sxs-lookup"><span data-stu-id="180f5-252">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="180f5-253">A következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="180f5-253">One of the following:</span></span>
  * <span data-ttu-id="180f5-254">Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.</span><span class="sxs-lookup"><span data-stu-id="180f5-254">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="180f5-255">ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-255">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="180f5-256">`feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha a modell sikeresen lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="180f5-256">`feed/entry/content/properties/HasActiveBuild` - Indicates if the model was built successfully.</span></span>
* <span data-ttu-id="180f5-257">`feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-257">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="180f5-258">`feed/entry/content/properties/Mpr`-Modell MPR (lásd a ModelInsight olvashat).</span><span class="sxs-lookup"><span data-stu-id="180f5-258">`feed/entry/content/properties/Mpr` - Model MPR (see ModelInsight for more information).</span></span>
* <span data-ttu-id="180f5-259">`feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="180f5-259">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>
* <span data-ttu-id="180f5-260">`feed/entry/content/properties/UsageFileNames`-Modell fájlok vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-260">`feed/entry/content/properties/UsageFileNames` - List of model usage files separated by comma.</span></span>
* <span data-ttu-id="180f5-261">`feed/entry/content/properties/CatalogId`-Modell katalógus azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-261">`feed/entry/content/properties/CatalogId` - Model catalog ID.</span></span>
* <span data-ttu-id="180f5-262">`feed/entry/content/properties/Description`-Modell leírása.</span><span class="sxs-lookup"><span data-stu-id="180f5-262">`feed/entry/content/properties/Description` - Model description.</span></span>
* <span data-ttu-id="180f5-263">`feed/entry/content/properties/CatalogFileName`-Fájl modell katalógusnév.</span><span class="sxs-lookup"><span data-stu-id="180f5-263">`feed/entry/content/properties/CatalogFileName` - Model catalog file name.</span></span>

<span data-ttu-id="180f5-264">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-264">OData XML</span></span>

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

### <a name="54----update-model"></a><span data-ttu-id="180f5-265">5.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-265">5.4.</span></span>    <span data-ttu-id="180f5-266">Frissítési modell</span><span class="sxs-lookup"><span data-stu-id="180f5-266">Update Model</span></span>
<span data-ttu-id="180f5-267">Frissítheti a modell leírása vagy az aktív build azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="180f5-267">You can update the model description or the active build ID.</span></span><br><span data-ttu-id="180f5-268">
<ins>Aktív build azonosító</ins> -minden build minden modell van állítva egy összeállítási.</span><span class="sxs-lookup"><span data-stu-id="180f5-268">
<ins>Active build ID</ins> - Every build for every model has a build ID.</span></span> <span data-ttu-id="180f5-269">Az aktív build azonosító: az első sikeres build minden új modell.</span><span class="sxs-lookup"><span data-stu-id="180f5-269">The active build ID is the first successful build of every new model.</span></span> <span data-ttu-id="180f5-270">Amennyiben van egy aktív build ID azonosítója és teheti meg ugyanannak a modellnek további alkot, explicit módon állítsa be alapértelmezett build Azonosítóként, ha azt szeretné, hogy szüksége.</span><span class="sxs-lookup"><span data-stu-id="180f5-270">Once you have an active build ID and you do additional builds for the same model, you need to explicitly set it as the default build ID if you want to.</span></span> <span data-ttu-id="180f5-271">Felhasznált ajánlásokat, ha nem adja meg a build Azonosítóját, amelynek szeretné használni, amikor az alapértelmezett automatikusan használható.</span><span class="sxs-lookup"><span data-stu-id="180f5-271">When you consume recommendations, if you do not specify the build ID that you want to use, the default one will be used automatically.</span></span><br>
<span data-ttu-id="180f5-272">Ez az eljárás lehetővé teszi - után egy javaslat modell éles - új modell összeállításához és teszteléséhez őket, mielőtt termelési támogatja azokat.</span><span class="sxs-lookup"><span data-stu-id="180f5-272">This mechanism enables you - once you have a recommendation model in production - to build new models and test them before you promote them to production.</span></span>

| <span data-ttu-id="180f5-273">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-273">HTTP Method</span></span> | <span data-ttu-id="180f5-274">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-275">A PUT</span><span class="sxs-lookup"><span data-stu-id="180f5-275">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-276">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-276">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-277">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-277">Parameter Name</span></span> | <span data-ttu-id="180f5-278">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-278">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-279">id</span><span class="sxs-lookup"><span data-stu-id="180f5-279">id</span></span> |<span data-ttu-id="180f5-280">A modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-280">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="180f5-281">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-281">apiVersion</span></span> |<span data-ttu-id="180f5-282">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-282">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-283">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-283">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br><span data-ttu-id="180f5-284">Ne feledje, hogy az XML-címkék leírását és ActiveBuildId nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="180f5-284">Note that the XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="180f5-285">Ha nem szeretné állítani a leírás vagy ActiveBuildId, távolítsa el a teljes címke.</span><span class="sxs-lookup"><span data-stu-id="180f5-285">If you do not want to set Description or ActiveBuildId, remove the entire tag.</span></span> |

<span data-ttu-id="180f5-286">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-286">**Response**:</span></span>

<span data-ttu-id="180f5-287">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-287">HTTP Status code: 200</span></span>

### <a name="55----delete-model"></a><span data-ttu-id="180f5-288">5.5.</span><span class="sxs-lookup"><span data-stu-id="180f5-288">5.5.</span></span>    <span data-ttu-id="180f5-289">Modell törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-289">Delete Model</span></span>
<span data-ttu-id="180f5-290">Törli a meglévő modell által azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-290">Deletes an existing model by ID.</span></span>

| <span data-ttu-id="180f5-291">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-291">HTTP Method</span></span> | <span data-ttu-id="180f5-292">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-292">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-293">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-293">DELETE</span></span> |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-294">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-294">Example:</span></span><br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-295">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-295">Parameter Name</span></span> | <span data-ttu-id="180f5-296">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-296">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-297">id</span><span class="sxs-lookup"><span data-stu-id="180f5-297">id</span></span> |<span data-ttu-id="180f5-298">A modell (kis-és nagybetűket) egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-298">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="180f5-299">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-299">apiVersion</span></span> |<span data-ttu-id="180f5-300">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-300">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-301">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-301">Request Body</span></span> |<span data-ttu-id="180f5-302">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-302">NONE</span></span> |

<span data-ttu-id="180f5-303">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-303">**Response**:</span></span>

<span data-ttu-id="180f5-304">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-304">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-305">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-305">OData XML</span></span>

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

## <a name="6-model-advanced"></a><span data-ttu-id="180f5-306">6. Speciális modell</span><span class="sxs-lookup"><span data-stu-id="180f5-306">6. Model Advanced</span></span>
### <a name="61----model-data-insight"></a><span data-ttu-id="180f5-307">6.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-307">6.1.</span></span>    <span data-ttu-id="180f5-308">Modell adatok felmérése</span><span class="sxs-lookup"><span data-stu-id="180f5-308">Model Data Insight</span></span>
<span data-ttu-id="180f5-309">A használati adatok alapján ez a modell a következővel történt a statisztikai adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-309">Returns statistical data on the usage data that this model was built with.</span></span>

<span data-ttu-id="180f5-310">Csak a javaslat build esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="180f5-310">Available only for Recommendation build.</span></span>

| <span data-ttu-id="180f5-311">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-311">HTTP Method</span></span> | <span data-ttu-id="180f5-312">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-312">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-313">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-313">GET</span></span> |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-314">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-314">Example:</span></span><br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-315">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-315">Parameter Name</span></span> | <span data-ttu-id="180f5-316">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-316">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-317">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-317">modelId</span></span> |<span data-ttu-id="180f5-318">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-318">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-319">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-319">apiVersion</span></span> |<span data-ttu-id="180f5-320">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-320">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-321">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-321">Request Body</span></span> |<span data-ttu-id="180f5-322">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-322">NONE</span></span> |

<span data-ttu-id="180f5-323">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-323">**Response**:</span></span>

<span data-ttu-id="180f5-324">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-324">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-325">Az adatok tulajdonságainak gyűjteményét adja vissza a rendszer.</span><span class="sxs-lookup"><span data-stu-id="180f5-325">The data is returned as a collection of properties.</span></span>

* <span data-ttu-id="180f5-326">`feed/entry/id/content/properties/key`-Érvényes tulajdonságnév.</span><span class="sxs-lookup"><span data-stu-id="180f5-326">`feed/entry/id/content/properties/key` - Holds the property name.</span></span>
* <span data-ttu-id="180f5-327">`feed/entry/id/content/properties/value`-A tulajdonság értékét fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="180f5-327">`feed/entry/id/content/properties/value` - Holds the property value.</span></span>

<span data-ttu-id="180f5-328">Az alábbi táblázat az egyes kulcsok értéket mutatja be.</span><span class="sxs-lookup"><span data-stu-id="180f5-328">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="180f5-329">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-329">Key</span></span> | <span data-ttu-id="180f5-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-330">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-331">AvgItemLength</span><span class="sxs-lookup"><span data-stu-id="180f5-331">AvgItemLength</span></span> |<span data-ttu-id="180f5-332">Elemenként egyedi felhasználók átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-332">Average number of distinct users per item.</span></span> |
| <span data-ttu-id="180f5-333">AvgUserLength</span><span class="sxs-lookup"><span data-stu-id="180f5-333">AvgUserLength</span></span> |<span data-ttu-id="180f5-334">Felhasználónként eltérő elemek átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-334">Average number of distinct items per user.</span></span> |
| <span data-ttu-id="180f5-335">DensificationNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="180f5-335">DensificationNumberOfItems</span></span> |<span data-ttu-id="180f5-336">Cikkek követően nem kell modellezni törlési elemek száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-336">Number of items after pruning items that cannot be modelled.</span></span> |
| <span data-ttu-id="180f5-337">DensificationNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="180f5-337">DensificationNumberOfUsers</span></span> |<span data-ttu-id="180f5-338">Használati pontok után törlési felhasználók és nem kell modellezni elemek száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-338">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="180f5-339">DensificationNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="180f5-339">DensificationNumberOfRecords</span></span> |<span data-ttu-id="180f5-340">Használati pontok után törlési felhasználók és nem kell modellezni elemek száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-340">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="180f5-341">MaxItemLength</span><span class="sxs-lookup"><span data-stu-id="180f5-341">MaxItemLength</span></span> |<span data-ttu-id="180f5-342">A legnépszerűbb elem egyedi felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-342">Number of distinct users for the most popular item.</span></span> |
| <span data-ttu-id="180f5-343">MaxUserLength</span><span class="sxs-lookup"><span data-stu-id="180f5-343">MaxUserLength</span></span> |<span data-ttu-id="180f5-344">Egy felhasználó különálló elemek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-344">Maximal number of distinct items for a user.</span></span> |
| <span data-ttu-id="180f5-345">MinItemLength</span><span class="sxs-lookup"><span data-stu-id="180f5-345">MinItemLength</span></span> |<span data-ttu-id="180f5-346">Egy elem egyedi felhasználók maximális száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-346">Maximal number of distinct users for an item.</span></span> |
| <span data-ttu-id="180f5-347">MinUserLength</span><span class="sxs-lookup"><span data-stu-id="180f5-347">MinUserLength</span></span> |<span data-ttu-id="180f5-348">Egy felhasználó különálló elemek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-348">Minimal number of distinct items for a user.</span></span> |
| <span data-ttu-id="180f5-349">RawNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="180f5-349">RawNumberOfItems</span></span> |<span data-ttu-id="180f5-350">A fájlok elemek száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-350">Number of items in the usage files.</span></span> |
| <span data-ttu-id="180f5-351">RawNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="180f5-351">RawNumberOfUsers</span></span> |<span data-ttu-id="180f5-352">Mielőtt bármely karbantartási eltávolítási használati pontok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-352">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="180f5-353">RawNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="180f5-353">RawNumberOfRecords</span></span> |<span data-ttu-id="180f5-354">Mielőtt bármely karbantartási eltávolítási használati pontok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-354">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="180f5-355">SamplingNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="180f5-355">SamplingNumberOfItems</span></span> |<span data-ttu-id="180f5-356">N/A</span><span class="sxs-lookup"><span data-stu-id="180f5-356">N/A</span></span> |
| <span data-ttu-id="180f5-357">SamplingNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="180f5-357">SamplingNumberOfRecords</span></span> |<span data-ttu-id="180f5-358">N/A</span><span class="sxs-lookup"><span data-stu-id="180f5-358">N/A</span></span> |
| <span data-ttu-id="180f5-359">SamplingNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="180f5-359">SamplingNumberOfUsers</span></span> |<span data-ttu-id="180f5-360">N/A</span><span class="sxs-lookup"><span data-stu-id="180f5-360">N/A</span></span> |

<span data-ttu-id="180f5-361">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-361">OData XML</span></span>

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

### <a name="62----model-insight"></a><span data-ttu-id="180f5-362">6.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-362">6.2.</span></span>    <span data-ttu-id="180f5-363">Modell felmérése</span><span class="sxs-lookup"><span data-stu-id="180f5-363">Model Insight</span></span>
<span data-ttu-id="180f5-364">A modell insight értéket ad vissza a aktív összeállítása a vagy (Ha a kapott) adott build.</span><span class="sxs-lookup"><span data-stu-id="180f5-364">Returns model insight on the active build or (if given) on a specific build.</span></span>

<span data-ttu-id="180f5-365">Csak a javaslat build esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="180f5-365">Available only for Recommendation build.</span></span>

| <span data-ttu-id="180f5-366">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-366">HTTP Method</span></span> | <span data-ttu-id="180f5-367">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-367">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-368">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-368">GET</span></span> |<span data-ttu-id="180f5-369">Aktív build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="180f5-369">With active build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-370">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-370">Example:</span></span><br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-371">Adott build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="180f5-371">With specific build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-372">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-372">Parameter Name</span></span> | <span data-ttu-id="180f5-373">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-373">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-374">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-374">modelId</span></span> |<span data-ttu-id="180f5-375">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-375">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-376">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-376">buildId</span></span> |<span data-ttu-id="180f5-377">Nem kötelező – sikeres build azonosító szám.</span><span class="sxs-lookup"><span data-stu-id="180f5-377">Optional - number that identifies a successful build.</span></span> |
| <span data-ttu-id="180f5-378">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-378">apiVersion</span></span> |<span data-ttu-id="180f5-379">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-379">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-380">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-380">Request Body</span></span> |<span data-ttu-id="180f5-381">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-381">NONE</span></span> |

<span data-ttu-id="180f5-382">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-382">**Response**:</span></span>

<span data-ttu-id="180f5-383">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-383">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-384">Az adatok tulajdonságainak gyűjteményét adja vissza a rendszer.</span><span class="sxs-lookup"><span data-stu-id="180f5-384">The data is returned as a collection of properties.</span></span>

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

<span data-ttu-id="180f5-385">Az alábbi táblázat az egyes kulcsok értéket mutatja be.</span><span class="sxs-lookup"><span data-stu-id="180f5-385">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="180f5-386">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-386">Key</span></span> | <span data-ttu-id="180f5-387">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-387">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-388">CatalogCoverage</span><span class="sxs-lookup"><span data-stu-id="180f5-388">CatalogCoverage</span></span> |<span data-ttu-id="180f5-389">A katalógus részét modellezhető használati mintákat.</span><span class="sxs-lookup"><span data-stu-id="180f5-389">What part of the catalog can be modelled with usage patterns.</span></span> <span data-ttu-id="180f5-390">A további elemeket kell tartalom-alapú szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="180f5-390">The rest of the items will need content-based features.</span></span> |
| <span data-ttu-id="180f5-391">MPR-hez</span><span class="sxs-lookup"><span data-stu-id="180f5-391">Mpr</span></span> |<span data-ttu-id="180f5-392">Átlagos PERCENTILIS rangsorolási a modellnek.</span><span class="sxs-lookup"><span data-stu-id="180f5-392">Mean percentile ranking of the model.</span></span> <span data-ttu-id="180f5-393">Jobb alsó.</span><span class="sxs-lookup"><span data-stu-id="180f5-393">Lower is better.</span></span> |
| <span data-ttu-id="180f5-394">NumberOfDimensions</span><span class="sxs-lookup"><span data-stu-id="180f5-394">NumberOfDimensions</span></span> |<span data-ttu-id="180f5-395">A mátrix factorization algoritmus által használt dimenziók száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-395">Number of dimensions used by the matrix factorization algorithm.</span></span> |

<span data-ttu-id="180f5-396">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-396">OData XML</span></span>

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

### <a name="63----get-model-sample"></a><span data-ttu-id="180f5-397">6.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-397">6.3.</span></span>    <span data-ttu-id="180f5-398">Modell minta beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-398">Get Model Sample</span></span>
<span data-ttu-id="180f5-399">Lekérdezi a javaslat modell minta.</span><span class="sxs-lookup"><span data-stu-id="180f5-399">Gets a sample of the recommendation model.</span></span>

| <span data-ttu-id="180f5-400">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-400">HTTP Method</span></span> | <span data-ttu-id="180f5-401">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-401">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-402">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-402">GET</span></span> |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-403">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-403">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-404">Adott build azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="180f5-404">With specific build ID:</span></span><br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-405">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-405">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-406">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-406">Parameter Name</span></span> | <span data-ttu-id="180f5-407">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-407">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-408">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-408">modelId</span></span> |<span data-ttu-id="180f5-409">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-409">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-410">apiVersion</span></span> |<span data-ttu-id="180f5-411">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-411">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-412">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-412">Request Body</span></span> |<span data-ttu-id="180f5-413">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-413">NONE</span></span> |

<span data-ttu-id="180f5-414">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-414">**Response**:</span></span>

<span data-ttu-id="180f5-415">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-415">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-416">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-416">OData XML</span></span>

<span data-ttu-id="180f5-417">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="180f5-417">Response is returned in raw text format:</span></span>

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
50471eec-9aeb-4900-84d7-21567ab18546, If the Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of the Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on the Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in the Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, The Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On the Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, The Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories to Tell in the Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, The Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic the Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in the Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, The Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, The X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell the President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In the Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, The Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of the Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, The Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in the Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (The Official Guide to the X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, The Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, The Truth Is Out There (The Official Guide to the X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, The Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of the Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where the Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, The God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (The Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in the Time of Cholera (Penguin Great Books of the 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, The Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories To Tell In The Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from the Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a><span data-ttu-id="180f5-418">7. Modell az üzleti szabályok</span><span class="sxs-lookup"><span data-stu-id="180f5-418">7. Model Business Rules</span></span>
<span data-ttu-id="180f5-419">A támogatott szabályok típusai a következők:</span><span class="sxs-lookup"><span data-stu-id="180f5-419">These are the types of rules supported:</span></span>

* <span data-ttu-id="180f5-420"><strong>BlockList</strong> -BlockList lehetővé teszi, hogy adja meg a javaslat eredményéből nem kívánt elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="180f5-420"><strong>BlockList</strong> - BlockList enables you to provide a list of items that you do not want to return in the recommendation results.</span></span> 
* <span data-ttu-id="180f5-421"><strong>FeatureBlockList</strong> -funkció BlockList lehetővé teszi a blokkolása szolgáltatása értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="180f5-421"><strong>FeatureBlockList</strong> - Feature BlockList enables you to block items based on the values of its features.</span></span>

<span data-ttu-id="180f5-422">*Ne küldjön 1000-nél több elemek egy egyetlen blocklist szabályban, vagy a hívás előfordulhat, hogy az időkorlát. Ha 1000-nél több elemet zárolnia kell, hogy több blocklist hívások.*</span><span class="sxs-lookup"><span data-stu-id="180f5-422">*Do not send more than 1000 items in a single blocklist rule or your call may timeout. If you need to block more than 1000 items, you can make several blocklist calls.*</span></span>

* <span data-ttu-id="180f5-423"><strong>Upsale</strong> -Upsale lehetővé teszi a javaslat eredmények visszaadandó elemek kényszerítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-423"><strong>Upsale</strong> - Upsale enables you to enforce items to return in the recommendation results.</span></span>
* <span data-ttu-id="180f5-424"><strong>Engedélyezési lista</strong> -fehér lista lehetővé teszi csak elemek listájából javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="180f5-424"><strong>WhiteList</strong> - White List enables you to only suggest recommendations from a list of items.</span></span>
* <span data-ttu-id="180f5-425"><strong>FeatureWhiteList</strong> -funkció fehér lista lehetővé teszi, hogy csak az elemek, amelyekhez az adott termékfejlesztő értékek ajánlani.</span><span class="sxs-lookup"><span data-stu-id="180f5-425"><strong>FeatureWhiteList</strong> - Feature White List enables you to only recommend items that have specific feature values.</span></span>
* <span data-ttu-id="180f5-426"><strong>PerSeedBlockList</strong> -/ Seed tiltólista lehetővé teszi elemenként javaslat eredményeként nem adható vissza elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="180f5-426"><strong>PerSeedBlockList</strong> - Per Seed Block List enables you to provide per item a list of items that cannot be returned as recommendation results.</span></span>

### <a name="71----get-model-rules"></a><span data-ttu-id="180f5-427">7.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-427">7.1.</span></span>    <span data-ttu-id="180f5-428">Modell szabályok lekérése</span><span class="sxs-lookup"><span data-stu-id="180f5-428">Get Model Rules</span></span>
| <span data-ttu-id="180f5-429">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-429">HTTP Method</span></span> | <span data-ttu-id="180f5-430">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-430">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-431">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-431">GET</span></span> |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="180f5-432">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-432">Example:</span></span><br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-433">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-433">Parameter Name</span></span> | <span data-ttu-id="180f5-434">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-434">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-435">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-435">modelId</span></span> |<span data-ttu-id="180f5-436">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-436">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-437">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-437">apiVersion</span></span> |<span data-ttu-id="180f5-438">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-438">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-439">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-439">Request Body</span></span> |<span data-ttu-id="180f5-440">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-440">NONE</span></span> |

<span data-ttu-id="180f5-441">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-441">**Response**:</span></span>

<span data-ttu-id="180f5-442">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-442">HTTP Status code: 200</span></span>

* <span data-ttu-id="180f5-443">`feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-443">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="180f5-444">`feed/entry/content/properties/Type`– A szabály típusa.</span><span class="sxs-lookup"><span data-stu-id="180f5-444">`feed/entry/content/properties/Type` - Type of the rule.</span></span>
* <span data-ttu-id="180f5-445">`feed/entry/content/properties/Parameter`-Szabály paraméter.</span><span class="sxs-lookup"><span data-stu-id="180f5-445">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="180f5-446">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-446">OData XML</span></span>

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

### <a name="72----add-rule"></a><span data-ttu-id="180f5-447">7.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-447">7.2.</span></span>    <span data-ttu-id="180f5-448">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="180f5-448">Add Rule</span></span>
| <span data-ttu-id="180f5-449">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-449">HTTP Method</span></span> | <span data-ttu-id="180f5-450">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-450">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-451">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-451">POST</span></span> |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| <span data-ttu-id="180f5-452">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="180f5-452">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="180f5-453">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-453">Parameter Name</span></span> | <span data-ttu-id="180f5-454">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-454">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-455">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-455">apiVersion</span></span> |<span data-ttu-id="180f5-456">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-456">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-457">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-457">Request Body</span></span> | |

<span data-ttu-id="180f5-458"><ins>Amikor biztosít azonosítónak olyan üzleti szabályok esetén, ügyeljen arra, hogy használja a cikk (azonos azonosítóval, amelyet a katalógus fájlban használt) külső azonosítóját</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-458"><ins>Whenever providing Item Ids for business rules, make sure to use the External Id of the item (the same Id that you used in the catalog file)</ins></span></span><br><span data-ttu-id="180f5-459">
<ins>BlockList szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-459">
<ins>To add a BlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="180f5-460"><ins>
<ins>FeatureBlockList szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-460"><ins>
<ins>To add a FeatureBlockList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><span data-ttu-id="180f5-461"><ins>Upsale szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-461"><ins> To add an Upsale rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br><span data-ttu-id="180f5-462">
<ins>Engedélyezési szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-462">
<ins>To add a WhiteList rule:</ins></span></span><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="180f5-463"><ins>
<ins>FeatureWhiteList szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-463"><ins>
<ins>To add a FeatureWhiteList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><span data-ttu-id="180f5-464"><ins>PerSeedBlockList szabály hozzáadása:</ins></span><span class="sxs-lookup"><span data-stu-id="180f5-464"><ins> To add a PerSeedBlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

<span data-ttu-id="180f5-465">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-465">**Response**:</span></span>

<span data-ttu-id="180f5-466">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-466">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-467">Az API-t adja vissza az újonnan létrehozott szabály a hozzá tartozó részletek.</span><span class="sxs-lookup"><span data-stu-id="180f5-467">The API returns the newly created rule with its details.</span></span> <span data-ttu-id="180f5-468">A szabályok tulajdonság lekérhetők a következő elérési utak:</span><span class="sxs-lookup"><span data-stu-id="180f5-468">The rules property can be retrieved from the following paths:</span></span>

* <span data-ttu-id="180f5-469">`feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-469">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="180f5-470">`feed/entry/content/properties/Type`-A szabály típusa: BlockList vagy Upsale.</span><span class="sxs-lookup"><span data-stu-id="180f5-470">`feed/entry/content/properties/Type` - Type of the rule: BlockList or Upsale.</span></span>
* <span data-ttu-id="180f5-471">`feed/entry/content/properties/Parameter`-Szabály paraméter.</span><span class="sxs-lookup"><span data-stu-id="180f5-471">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="180f5-472">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-472">OData XML</span></span>

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

### <a name="73----delete-rule"></a><span data-ttu-id="180f5-473">7.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-473">7.3.</span></span>    <span data-ttu-id="180f5-474">A szabály törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-474">Delete Rule</span></span>
| <span data-ttu-id="180f5-475">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-475">HTTP Method</span></span> | <span data-ttu-id="180f5-476">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-476">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-477">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-477">DELETE</span></span> |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-478">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-478">Example:</span></span><br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-479">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-479">Parameter Name</span></span> | <span data-ttu-id="180f5-480">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-480">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-481">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-481">modelId</span></span> |<span data-ttu-id="180f5-482">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-482">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-483">filterId</span><span class="sxs-lookup"><span data-stu-id="180f5-483">filterId</span></span> |<span data-ttu-id="180f5-484">A szűrő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-484">Unique identifier of the filter</span></span> |
| <span data-ttu-id="180f5-485">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-485">apiVersion</span></span> |<span data-ttu-id="180f5-486">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-486">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-487">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-487">Request Body</span></span> |<span data-ttu-id="180f5-488">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-488">NONE</span></span> |

<span data-ttu-id="180f5-489">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-489">**Response**:</span></span>

<span data-ttu-id="180f5-490">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-490">HTTP Status code: 200</span></span>

### <a name="74----delete-all-rules"></a><span data-ttu-id="180f5-491">7.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-491">7.4.</span></span>    <span data-ttu-id="180f5-492">A szabályok törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-492">Delete All Rules</span></span>
| <span data-ttu-id="180f5-493">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-493">HTTP Method</span></span> | <span data-ttu-id="180f5-494">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-495">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-495">DELETE</span></span> |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-496">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-496">Example:</span></span><br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-497">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-497">Parameter Name</span></span> | <span data-ttu-id="180f5-498">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-499">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-499">modelId</span></span> |<span data-ttu-id="180f5-500">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-500">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-501">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-501">apiVersion</span></span> |<span data-ttu-id="180f5-502">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-502">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-503">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-503">Request Body</span></span> |<span data-ttu-id="180f5-504">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-504">NONE</span></span> |

<span data-ttu-id="180f5-505">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-505">**Response**:</span></span>

<span data-ttu-id="180f5-506">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-506">HTTP Status code: 200</span></span>

## <a name="8-catalog"></a><span data-ttu-id="180f5-507">8. Alkalmazáskatalógus</span><span class="sxs-lookup"><span data-stu-id="180f5-507">8. Catalog</span></span>
### <a name="81----import-catalog-data"></a><span data-ttu-id="180f5-508">8.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-508">8.1.</span></span>    <span data-ttu-id="180f5-509">Eseménykatalógus-adatok importálása</span><span class="sxs-lookup"><span data-stu-id="180f5-509">Import Catalog Data</span></span>
<span data-ttu-id="180f5-510">Ha ugyanannak a modellnek több hívások több katalógus fájlt feltölteni, azt csak az új katalóguselemek szúrja be.</span><span class="sxs-lookup"><span data-stu-id="180f5-510">If you upload several catalog files to the same model with several calls, we will insert only the new catalog items.</span></span> <span data-ttu-id="180f5-511">A meglévő elemek marad az eredeti értékekkel.</span><span class="sxs-lookup"><span data-stu-id="180f5-511">Existing items will remain with the original values.</span></span> <span data-ttu-id="180f5-512">Eseménykatalógus-adatok a metódus nem frissíthető.</span><span class="sxs-lookup"><span data-stu-id="180f5-512">You cannot update catalog data by using this method.</span></span>

<span data-ttu-id="180f5-513">A katalógus adatait a következő formátumot kell követnie:</span><span class="sxs-lookup"><span data-stu-id="180f5-513">The catalog data should follow the following format:</span></span>

* <span data-ttu-id="180f5-514">Szolgáltatások – nélkül`<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span><span class="sxs-lookup"><span data-stu-id="180f5-514">Without features - `<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span></span>
* <span data-ttu-id="180f5-515">-Szolgáltatásokkal`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span><span class="sxs-lookup"><span data-stu-id="180f5-515">With features - `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span></span>

<span data-ttu-id="180f5-516">Megjegyzés: A fájl maximális mérete 200MB.</span><span class="sxs-lookup"><span data-stu-id="180f5-516">Note: The maximum file size is 200MB.</span></span>

<span data-ttu-id="180f5-517">** Részletek formázása **</span><span class="sxs-lookup"><span data-stu-id="180f5-517">** Format details **</span></span>

| <span data-ttu-id="180f5-518">Név</span><span class="sxs-lookup"><span data-stu-id="180f5-518">Name</span></span> | <span data-ttu-id="180f5-519">Kötelező</span><span class="sxs-lookup"><span data-stu-id="180f5-519">Mandatory</span></span> | <span data-ttu-id="180f5-520">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-520">Type</span></span> | <span data-ttu-id="180f5-521">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-521">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="180f5-522">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-522">Item Id</span></span> |<span data-ttu-id="180f5-523">Igen</span><span class="sxs-lookup"><span data-stu-id="180f5-523">Yes</span></span> |<span data-ttu-id="180f5-524">[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="180f5-524">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="180f5-525">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-525">Max length: 50</span></span> |<span data-ttu-id="180f5-526">Egy elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-526">Unique identifier of an item.</span></span> |
| <span data-ttu-id="180f5-527">Elem neve</span><span class="sxs-lookup"><span data-stu-id="180f5-527">Item Name</span></span> |<span data-ttu-id="180f5-528">Igen</span><span class="sxs-lookup"><span data-stu-id="180f5-528">Yes</span></span> |<span data-ttu-id="180f5-529">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="180f5-529">Any alphanumeric characters</span></span><br> <span data-ttu-id="180f5-530">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="180f5-530">Max length: 255</span></span> |<span data-ttu-id="180f5-531">Elem neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-531">Item name.</span></span> |
| <span data-ttu-id="180f5-532">Elem kategória</span><span class="sxs-lookup"><span data-stu-id="180f5-532">Item Category</span></span> |<span data-ttu-id="180f5-533">Igen</span><span class="sxs-lookup"><span data-stu-id="180f5-533">Yes</span></span> |<span data-ttu-id="180f5-534">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="180f5-534">Any alphanumeric characters</span></span> <br> <span data-ttu-id="180f5-535">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="180f5-535">Max length: 255</span></span> |<span data-ttu-id="180f5-536">Kategória, amelyhez ez a cikk tartozik (pl. főzés könyvek, tragédiát...); üres is lehet.</span><span class="sxs-lookup"><span data-stu-id="180f5-536">Category to which this item belongs (e.g. Cooking Books, Drama…); can be empty.</span></span> |
| <span data-ttu-id="180f5-537">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-537">Description</span></span> |<span data-ttu-id="180f5-538">Nem, kivéve, ha a funkciók jelent-e (de üres lehet)</span><span class="sxs-lookup"><span data-stu-id="180f5-538">No, unless features are present (but can be empty)</span></span> |<span data-ttu-id="180f5-539">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="180f5-539">Any alphanumeric characters</span></span> <br> <span data-ttu-id="180f5-540">Maximális hossz: 4000</span><span class="sxs-lookup"><span data-stu-id="180f5-540">Max length: 4000</span></span> |<span data-ttu-id="180f5-541">Ez az elem leírása.</span><span class="sxs-lookup"><span data-stu-id="180f5-541">Description of this item.</span></span> |
| <span data-ttu-id="180f5-542">Szolgáltatások listája</span><span class="sxs-lookup"><span data-stu-id="180f5-542">Features list</span></span> |<span data-ttu-id="180f5-543">Nem</span><span class="sxs-lookup"><span data-stu-id="180f5-543">No</span></span> |<span data-ttu-id="180f5-544">Bármely alfanumerikus karakterek</span><span class="sxs-lookup"><span data-stu-id="180f5-544">Any alphanumeric characters</span></span> <br> <span data-ttu-id="180f5-545">Maximális hossz: 4000; Szolgáltatások: 20 maximális száma</span><span class="sxs-lookup"><span data-stu-id="180f5-545">Max length: 4000; Max number of features:20</span></span> |<span data-ttu-id="180f5-546">Szolgáltatás neve vesszővel elválasztott listája modell ajánlás; javítására használható szolgáltatás érték = Lásd: [témakörök speciális](#2-advanced-topics) szakasz.</span><span class="sxs-lookup"><span data-stu-id="180f5-546">Comma-separated list of feature name=feature value that can be used to enhance model recommendation; see [Advanced topics](#2-advanced-topics) section.</span></span> |

| <span data-ttu-id="180f5-547">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-547">HTTP Method</span></span> | <span data-ttu-id="180f5-548">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-548">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-549">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-549">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-550">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-550">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| <span data-ttu-id="180f5-551">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="180f5-551">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="180f5-552">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-552">Parameter Name</span></span> | <span data-ttu-id="180f5-553">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-553">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-554">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-554">modelId</span></span> |<span data-ttu-id="180f5-555">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-555">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-556">fájlnév</span><span class="sxs-lookup"><span data-stu-id="180f5-556">filename</span></span> |<span data-ttu-id="180f5-557">A katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-557">Textual identifier of the catalog.</span></span><br><span data-ttu-id="180f5-558">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="180f5-558">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="180f5-559">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-559">Max length: 50</span></span> |
| <span data-ttu-id="180f5-560">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-560">apiVersion</span></span> |<span data-ttu-id="180f5-561">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-561">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-562">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-562">Request Body</span></span> |<span data-ttu-id="180f5-563">(A szolgáltatások) például:</span><span class="sxs-lookup"><span data-stu-id="180f5-563">Example (with features):</span></span><br/><span data-ttu-id="180f5-564">2406e770-769c-4189-89de-1c9283f93a96 Clara Callan, megjelenik a könyv leírás szerzői = Richard Wright, közzétevő = Harper Flamingo Kanada, év = 2001</span><span class="sxs-lookup"><span data-stu-id="180f5-564">2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book,the book  description,author=Richard Wright,publisher=Harper Flamingo Canada,year=2001</span></span><br><span data-ttu-id="180f5-565">21bf8088-b6c0-4509-870c-e1c7ac78304a, elfelejti a helyiségben: A példa (Byzantium könyv), a könyv,, szerzői = Nick Bantock, közzétevő = Harpercollins, év = 1997</span><span class="sxs-lookup"><span data-stu-id="180f5-565">21bf8088-b6c0-4509-870c-e1c7ac78304a,The Forgetting Room: A Fiction (Byzantium Book),Book,,author=Nick Bantock,publisher=Harpercollins,year=1997</span></span><br><span data-ttu-id="180f5-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23 Spadework, könyv,, szerzői = komócsin Findley, közzétevő = HarperFlamingo Kanada év = 2001</span><span class="sxs-lookup"><span data-stu-id="180f5-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book,,author=Timothy Findley, publisher=HarperFlamingo Canada, year=2001</span></span><br><span data-ttu-id="180f5-567">552a1940-21e4-4399-82bb-594b46d7ed54 korlátozás a vadállatok, megjelenik a könyv leírás szerzői = Magnus Mills, közzétevő = Arcade közzétételi év = 1998</span><span class="sxs-lookup"><span data-stu-id="180f5-567">552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book,the book description,author=Magnus Mills, publisher=Arcade Publishing, year=1998</span></span></pre> |

<span data-ttu-id="180f5-568">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-568">**Response**:</span></span>

<span data-ttu-id="180f5-569">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-569">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-570">Az API-t adja vissza az importálás jelentést.</span><span class="sxs-lookup"><span data-stu-id="180f5-570">The API returns a report of the import.</span></span>

* <span data-ttu-id="180f5-571">`feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-571">`feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="180f5-572">`feed\entry\content\properties\ErrorCount`-Hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-572">`feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>

<span data-ttu-id="180f5-573">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-573">OData XML</span></span>

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

### <a name="82----get-catalog"></a><span data-ttu-id="180f5-574">8.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-574">8.2.</span></span>    <span data-ttu-id="180f5-575">Katalógus beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-575">Get Catalog</span></span>
<span data-ttu-id="180f5-576">Lekéri az összes katalóguselemeket.</span><span class="sxs-lookup"><span data-stu-id="180f5-576">Retrieves all catalog items.</span></span>
<span data-ttu-id="180f5-577">A katalógus lesz egyszerre egy lap lekérése.</span><span class="sxs-lookup"><span data-stu-id="180f5-577">The catalog will be retrieved one page at a time.</span></span> <span data-ttu-id="180f5-578">Ha azt szeretné, egy adott indexű beolvasásának, használhatja a $skip odata paraméter.</span><span class="sxs-lookup"><span data-stu-id="180f5-578">If you want to get items at a particular index, you can use the $skip odata parameter.</span></span> <span data-ttu-id="180f5-579">Például ha le szeretné kérdezni 100 pozíciótól kezdődően elemet, adja hozzá a $skip paraméter = 100 kérésre.</span><span class="sxs-lookup"><span data-stu-id="180f5-579">For instance if you want to get items starting at position 100, add the parameter $skip=100 to the request.</span></span>

| <span data-ttu-id="180f5-580">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-580">HTTP Method</span></span> | <span data-ttu-id="180f5-581">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-581">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-582">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-582">GET</span></span> |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-583">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-583">Example:</span></span><br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-584">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-584">Parameter Name</span></span> | <span data-ttu-id="180f5-585">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-585">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-586">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-586">modelId</span></span> |<span data-ttu-id="180f5-587">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-587">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-588">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-588">apiVersion</span></span> |<span data-ttu-id="180f5-589">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-589">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-590">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-590">Request Body</span></span> |<span data-ttu-id="180f5-591">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-591">NONE</span></span> |

<span data-ttu-id="180f5-592">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-592">**Response**:</span></span>

<span data-ttu-id="180f5-593">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-593">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-594">A válasz tartalmazza az elemet egy tételt.</span><span class="sxs-lookup"><span data-stu-id="180f5-594">The response includes one entry per catalog item.</span></span> <span data-ttu-id="180f5-595">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-595">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-596">`feed/entry/content/properties/ExternalId`-Katalógus külső azonosítója, az ügyfél által a megadottal.</span><span class="sxs-lookup"><span data-stu-id="180f5-596">`feed/entry/content/properties/ExternalId` - Catalog item external ID, the one provided by the customer.</span></span>
* <span data-ttu-id="180f5-597">`feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, azt, amelyik az Azure Machine Learning javaslatok hozott létre.</span><span class="sxs-lookup"><span data-stu-id="180f5-597">`feed/entry/content/properties/InternalId` - Catalog item internal ID, the one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="180f5-598">`feed/entry/content/properties/Name`-Katalógus elem neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-598">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="180f5-599">`feed/entry/content/properties/Category`-Katalógus cikk kategóriát.</span><span class="sxs-lookup"><span data-stu-id="180f5-599">`feed/entry/content/properties/Category` - Catalog item category.</span></span>
* <span data-ttu-id="180f5-600">`feed/entry/content/properties/Description`-Katalógus leírása.</span><span class="sxs-lookup"><span data-stu-id="180f5-600">`feed/entry/content/properties/Description` - Catalog item description.</span></span>
* <span data-ttu-id="180f5-601">`feed/entry/content/properties/Metadata`-Katalógus elem metaadatait.</span><span class="sxs-lookup"><span data-stu-id="180f5-601">`feed/entry/content/properties/Metadata` - Catalog item metadata.</span></span>

<span data-ttu-id="180f5-602">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-602">OData XML</span></span>

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
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a><span data-ttu-id="180f5-603">8.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-603">8.3.</span></span>    <span data-ttu-id="180f5-604">Beolvasni a Katalóguselemeket a Token által</span><span class="sxs-lookup"><span data-stu-id="180f5-604">Get Catalog Items by Token</span></span>
| <span data-ttu-id="180f5-605">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-605">HTTP Method</span></span> | <span data-ttu-id="180f5-606">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-606">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-607">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-607">GET</span></span> |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-608">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-608">Example:</span></span><br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-609">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-609">Parameter Name</span></span> | <span data-ttu-id="180f5-610">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-610">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-611">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-611">modelId</span></span> |<span data-ttu-id="180f5-612">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-612">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-613">Token</span><span class="sxs-lookup"><span data-stu-id="180f5-613">token</span></span> |<span data-ttu-id="180f5-614">A Katalóguselem nevű token.</span><span class="sxs-lookup"><span data-stu-id="180f5-614">Token of the catalog item’s name.</span></span> <span data-ttu-id="180f5-615">Legalább 3 karaktert kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="180f5-615">Should contain at least 3 characters.</span></span> |
| <span data-ttu-id="180f5-616">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-616">apiVersion</span></span> |<span data-ttu-id="180f5-617">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-617">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-618">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-618">Request Body</span></span> |<span data-ttu-id="180f5-619">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-619">NONE</span></span> |

<span data-ttu-id="180f5-620">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-620">**Response**:</span></span>

<span data-ttu-id="180f5-621">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-621">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-622">A válasz tartalmazza az elemet egy tételt.</span><span class="sxs-lookup"><span data-stu-id="180f5-622">The response includes one entry per catalog item.</span></span> <span data-ttu-id="180f5-623">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-623">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-624">`feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, azt, amelyik az Azure Machine Learning javaslatok hozott létre.</span><span class="sxs-lookup"><span data-stu-id="180f5-624">`feed/entry/content/properties/InternalId` - Catalog item internal ID, the one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="180f5-625">`feed/entry/content/properties/Name`-Katalógus elem neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-625">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="180f5-626">`feed/entry/content/properties/Rating`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="180f5-626">`feed/entry/content/properties/Rating` -  (for future use)</span></span>
* <span data-ttu-id="180f5-627">`feed/entry/content/properties/Reasoning`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="180f5-627">`feed/entry/content/properties/Reasoning` -  (for future use)</span></span>
* <span data-ttu-id="180f5-628">`feed/entry/content/properties/Metadata`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="180f5-628">`feed/entry/content/properties/Metadata` -  (for future use)</span></span>
* <span data-ttu-id="180f5-629">`feed/entry/content/properties/FormattedRating`-(későbbi használatra)</span><span class="sxs-lookup"><span data-stu-id="180f5-629">`feed/entry/content/properties/FormattedRating` - (for future use)</span></span>

<span data-ttu-id="180f5-630">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-630">OData XML</span></span>

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

## <a name="9-usage-data"></a><span data-ttu-id="180f5-631">9. Használati adatok</span><span class="sxs-lookup"><span data-stu-id="180f5-631">9. Usage data</span></span>
### <a name="91----import-usage-data"></a><span data-ttu-id="180f5-632">9.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-632">9.1.</span></span>    <span data-ttu-id="180f5-633">Használati adatok importálása</span><span class="sxs-lookup"><span data-stu-id="180f5-633">Import Usage Data</span></span>
#### <a name="911-uploading-file"></a><span data-ttu-id="180f5-634">9.1.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-634">9.1.1.</span></span> <span data-ttu-id="180f5-635">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="180f5-635">Uploading File</span></span>
<span data-ttu-id="180f5-636">Ez a szakasz bemutatja, hogyan használati adatok feltöltése egy fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="180f5-636">This section shows how to upload usage data by using a file.</span></span> <span data-ttu-id="180f5-637">A használati adatokat tartalmazó többször API hívása.</span><span class="sxs-lookup"><span data-stu-id="180f5-637">You can call this API several times with usage data.</span></span> <span data-ttu-id="180f5-638">Minden használati adatokat a rendszer minden hívások menti.</span><span class="sxs-lookup"><span data-stu-id="180f5-638">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="180f5-639">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-639">HTTP Method</span></span> | <span data-ttu-id="180f5-640">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-640">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-641">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-641">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-642">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-642">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-643">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-643">Parameter Name</span></span> | <span data-ttu-id="180f5-644">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-644">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-645">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-645">modelId</span></span> |<span data-ttu-id="180f5-646">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-646">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-647">fájlnév</span><span class="sxs-lookup"><span data-stu-id="180f5-647">filename</span></span> |<span data-ttu-id="180f5-648">A katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-648">Textual identifier of the catalog.</span></span><br><span data-ttu-id="180f5-649">Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.</span><span class="sxs-lookup"><span data-stu-id="180f5-649">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="180f5-650">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-650">Max length: 50</span></span> |
| <span data-ttu-id="180f5-651">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-651">apiVersion</span></span> |<span data-ttu-id="180f5-652">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-652">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-653">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-653">Request Body</span></span> |<span data-ttu-id="180f5-654">Használati adatok.</span><span class="sxs-lookup"><span data-stu-id="180f5-654">Usage data.</span></span> <span data-ttu-id="180f5-655">Formátum:</span><span class="sxs-lookup"><span data-stu-id="180f5-655">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="180f5-656">Név</span><span class="sxs-lookup"><span data-stu-id="180f5-656">Name</span></span></th><th><span data-ttu-id="180f5-657">Kötelező</span><span class="sxs-lookup"><span data-stu-id="180f5-657">Mandatory</span></span></th><th><span data-ttu-id="180f5-658">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-658">Type</span></span></th><th><span data-ttu-id="180f5-659">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-659">Description</span></span></th></tr><tr><td><span data-ttu-id="180f5-660">Felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="180f5-660">User Id</span></span></td><td><span data-ttu-id="180f5-661">Igen</span><span class="sxs-lookup"><span data-stu-id="180f5-661">Yes</span></span></td><td><span data-ttu-id="180f5-662">[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="180f5-662">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="180f5-663">Maximális hossz: 255</span><span class="sxs-lookup"><span data-stu-id="180f5-663">Max length: 255</span></span> </td><td><span data-ttu-id="180f5-664">A felhasználó egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-664">Unique identifier of a user.</span></span></td></tr><tr><td><span data-ttu-id="180f5-665">Cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-665">Item Id</span></span></td><td><span data-ttu-id="180f5-666">Igen</span><span class="sxs-lookup"><span data-stu-id="180f5-666">Yes</span></span></td><td><span data-ttu-id="180f5-667">[A-z], [a-z], [0-9], [&#95;] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="180f5-667">[A-z], [a-z], [0-9], [&#95;] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="180f5-668">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-668">Max length: 50</span></span></td><td><span data-ttu-id="180f5-669">Egy elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-669">Unique identifier of an item.</span></span></td></tr><tr><td><span data-ttu-id="180f5-670">Time</span><span class="sxs-lookup"><span data-stu-id="180f5-670">Time</span></span></td><td><span data-ttu-id="180f5-671">Nem</span><span class="sxs-lookup"><span data-stu-id="180f5-671">No</span></span></td><td><span data-ttu-id="180f5-672">Dátum formátumban: éééé/hh/nnTóó: pp: (pl. 2013 06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="180f5-672">Date in format: YYYY/MM/DDTHH:MM:SS (e.g. 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="180f5-673">Az adatok időpontja.</span><span class="sxs-lookup"><span data-stu-id="180f5-673">Time of data.</span></span></td></tr><tr><td><span data-ttu-id="180f5-674">Esemény</span><span class="sxs-lookup"><span data-stu-id="180f5-674">Event</span></span></td><td><span data-ttu-id="180f5-675">Nincs; Ha a megadott dátum is kell állítani</span><span class="sxs-lookup"><span data-stu-id="180f5-675">No; if supplied then must also put date</span></span></td><td><span data-ttu-id="180f5-676">A következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="180f5-676">One of the following:</span></span><br><span data-ttu-id="180f5-677">• Kattintson</span><span class="sxs-lookup"><span data-stu-id="180f5-677">• Click</span></span><br><span data-ttu-id="180f5-678">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="180f5-678">• RecommendationClick</span></span><br><span data-ttu-id="180f5-679">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="180f5-679">•    AddShopCart</span></span><br><span data-ttu-id="180f5-680">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="180f5-680">• RemoveShopCart</span></span><br><span data-ttu-id="180f5-681">• Beszerzési</span><span class="sxs-lookup"><span data-stu-id="180f5-681">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="180f5-682">Maximális fájlméret: 200MB</span><span class="sxs-lookup"><span data-stu-id="180f5-682">Maximum file size: 200MB</span></span><br><br><span data-ttu-id="180f5-683">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-683">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="180f5-684">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-684">**Response**:</span></span>

<span data-ttu-id="180f5-685">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-685">HTTP Status code: 200</span></span>

* <span data-ttu-id="180f5-686">`Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-686">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="180f5-687">`Feed\entry\content\properties\ErrorCount`-Hiba miatt nem beillesztett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-687">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>
* <span data-ttu-id="180f5-688">`Feed\entry\content\properties\FileId`-Fájl azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-688">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="180f5-689">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-689">OData XML</span></span>

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


#### <a name="912-using-data-acquisition"></a><span data-ttu-id="180f5-690">9.1.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-690">9.1.2.</span></span> <span data-ttu-id="180f5-691">Adatgyűjtést használatával</span><span class="sxs-lookup"><span data-stu-id="180f5-691">Using Data Acquisition</span></span>
<span data-ttu-id="180f5-692">Ez a szakasz bemutatja, hogyan küldhet események valós idejű Azure Machine Learning ajánlásokat, általában a webhelyről.</span><span class="sxs-lookup"><span data-stu-id="180f5-692">This section shows how to send events in real time to Azure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="180f5-693">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-693">HTTP Method</span></span> | <span data-ttu-id="180f5-694">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-694">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-695">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-695">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| <span data-ttu-id="180f5-696">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="180f5-696">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="180f5-697">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-697">Parameter Name</span></span> | <span data-ttu-id="180f5-698">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-698">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-699">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-699">apiVersion</span></span> |<span data-ttu-id="180f5-700">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-700">1.0</span></span> |
| <span data-ttu-id="180f5-701">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="180f5-701">Request body</span></span> |<span data-ttu-id="180f5-702">Esemény esetében bevitt adat minden esemény szeretne küldeni.</span><span class="sxs-lookup"><span data-stu-id="180f5-702">Event data entry for each event you want to send.</span></span> <span data-ttu-id="180f5-703">Küldje el a ugyanazon felhasználó vagy a böngésző-munkamenet ugyanazt az Azonosítót a munkamenet-azonosító mezőben.</span><span class="sxs-lookup"><span data-stu-id="180f5-703">You should send for the same user or browser session the same ID in the SessionId field.</span></span> <span data-ttu-id="180f5-704">(Lásd az alábbi esemény törzsében mintáját.)</span><span class="sxs-lookup"><span data-stu-id="180f5-704">(See sample of event body below.)</span></span> |

* <span data-ttu-id="180f5-705">Példa a "Kattintson" esemény:</span><span class="sxs-lookup"><span data-stu-id="180f5-705">Example for event 'Click':</span></span>
  
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
* <span data-ttu-id="180f5-706">Példa a "RecommendationClick" esemény:</span><span class="sxs-lookup"><span data-stu-id="180f5-706">Example for event 'RecommendationClick':</span></span>
  
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
* <span data-ttu-id="180f5-707">Példa a "AddShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="180f5-707">Example for event 'AddShopCart':</span></span>
  
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
* <span data-ttu-id="180f5-708">Példa a "RemoveShopCart" esemény:</span><span class="sxs-lookup"><span data-stu-id="180f5-708">Example for event 'RemoveShopCart':</span></span>
  
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
* <span data-ttu-id="180f5-709">Példa a "Vásárlás" esemény:</span><span class="sxs-lookup"><span data-stu-id="180f5-709">Example for event 'Purchase':</span></span>
  
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
* <span data-ttu-id="180f5-710">Példa 2 események "kattintson a" és "AddShopCart" küldésekor:</span><span class="sxs-lookup"><span data-stu-id="180f5-710">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
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

<span data-ttu-id="180f5-711">**Válasz**: HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-711">**Response**: HTTP Status code: 200</span></span>

### <a name="92----list-model-usage-files"></a><span data-ttu-id="180f5-712">9.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-712">9.2.</span></span>    <span data-ttu-id="180f5-713">Modell használati fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="180f5-713">List Model Usage Files</span></span>
<span data-ttu-id="180f5-714">Lekéri az összes modell fájlok metaadatait.</span><span class="sxs-lookup"><span data-stu-id="180f5-714">Retrieves metadata of all model usage files.</span></span>
<span data-ttu-id="180f5-715">A fájlok lesznek használati egy oldalt az attribútumböngészőben egyszerre beolvasott.</span><span class="sxs-lookup"><span data-stu-id="180f5-715">The usage files will be retrieved one page at a time.</span></span> <span data-ttu-id="180f5-716">Minden lap containes 100 elemeket.</span><span class="sxs-lookup"><span data-stu-id="180f5-716">Each page containes 100 items.</span></span> <span data-ttu-id="180f5-717">Ha azt szeretné, egy adott indexű beolvasásának, használhatja a $skip odata paraméter.</span><span class="sxs-lookup"><span data-stu-id="180f5-717">If you want to get items at a particular index, you can use the $skip odata parameter.</span></span> <span data-ttu-id="180f5-718">Például ha le szeretné kérdezni 100 pozíciótól kezdődően elemet, adja hozzá a $skip paraméter = 100 kérésre.</span><span class="sxs-lookup"><span data-stu-id="180f5-718">For instance if you want to get items starting at position 100, add the parameter $skip=100 to the request.</span></span>

| <span data-ttu-id="180f5-719">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-719">HTTP Method</span></span> | <span data-ttu-id="180f5-720">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-720">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-721">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-721">GET</span></span> |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-722">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-722">Example:</span></span><br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-723">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-723">Parameter Name</span></span> | <span data-ttu-id="180f5-724">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-724">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-725">forModelId</span><span class="sxs-lookup"><span data-stu-id="180f5-725">forModelId</span></span> |<span data-ttu-id="180f5-726">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-726">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-727">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-727">apiVersion</span></span> |<span data-ttu-id="180f5-728">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-728">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-729">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-729">Request Body</span></span> |<span data-ttu-id="180f5-730">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-730">NONE</span></span> |

<span data-ttu-id="180f5-731">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-731">**Response**:</span></span>

<span data-ttu-id="180f5-732">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-732">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-733">A válasz használati fájlonként egy bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="180f5-733">The response includes one entry per usage file.</span></span> <span data-ttu-id="180f5-734">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-734">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-735">`feed\entry\content\properties\Id`-Használati azonosítót.</span><span class="sxs-lookup"><span data-stu-id="180f5-735">`feed\entry\content\properties\Id` - Usage file ID.</span></span>
* <span data-ttu-id="180f5-736">`feed\entry\content\properties\Length`-MB használat a fájl hosszát.</span><span class="sxs-lookup"><span data-stu-id="180f5-736">`feed\entry\content\properties\Length` - Usage file length in MB.</span></span>
* <span data-ttu-id="180f5-737">`feed\entry\content\properties\DateModified`-A használati fájl létrehozásának dátuma.</span><span class="sxs-lookup"><span data-stu-id="180f5-737">`feed\entry\content\properties\DateModified` - Date when the usage file was created.</span></span>
* <span data-ttu-id="180f5-738">`feed\entry\content\properties\UseInModel`-E a használati fájllal a modellben.</span><span class="sxs-lookup"><span data-stu-id="180f5-738">`feed\entry\content\properties\UseInModel` - Whether the usage file is used in the model.</span></span>

<span data-ttu-id="180f5-739">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-739">OData XML</span></span>

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

### <a name="93----get-usage-statistics"></a><span data-ttu-id="180f5-740">9.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-740">9.3.</span></span>    <span data-ttu-id="180f5-741">Használati statisztikák beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-741">Get Usage Statistics</span></span>
<span data-ttu-id="180f5-742">Lekéri a használati statisztikáit.</span><span class="sxs-lookup"><span data-stu-id="180f5-742">Gets usage statistics.</span></span>

| <span data-ttu-id="180f5-743">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-743">HTTP Method</span></span> | <span data-ttu-id="180f5-744">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-744">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-745">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-745">GET</span></span> |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-746">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-746">Example:</span></span><br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-747">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-747">Parameter Name</span></span> | <span data-ttu-id="180f5-748">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-748">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-749">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-749">modelId</span></span> |<span data-ttu-id="180f5-750">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-750">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-751">A StartDate</span><span class="sxs-lookup"><span data-stu-id="180f5-751">startDate</span></span> |<span data-ttu-id="180f5-752">Kezdő dátum.</span><span class="sxs-lookup"><span data-stu-id="180f5-752">Start date.</span></span> <span data-ttu-id="180f5-753">Formátum: éééé/hh/nnTóó: pp:</span><span class="sxs-lookup"><span data-stu-id="180f5-753">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="180f5-754">endDate</span><span class="sxs-lookup"><span data-stu-id="180f5-754">endDate</span></span> |<span data-ttu-id="180f5-755">Záró dátum.</span><span class="sxs-lookup"><span data-stu-id="180f5-755">End date.</span></span> <span data-ttu-id="180f5-756">Formátum: éééé/hh/nnTóó: pp:</span><span class="sxs-lookup"><span data-stu-id="180f5-756">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="180f5-757">eventTypes</span><span class="sxs-lookup"><span data-stu-id="180f5-757">eventTypes</span></span> |<span data-ttu-id="180f5-758">Vesszővel elválasztott karakterlánc eseménytípusok vagy null az összes eseményének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="180f5-758">Comma-separated string of event types or null to get all events</span></span> |
| <span data-ttu-id="180f5-759">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-759">apiVersion</span></span> |<span data-ttu-id="180f5-760">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-760">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-761">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-761">Request Body</span></span> |<span data-ttu-id="180f5-762">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-762">NONE</span></span> |

<span data-ttu-id="180f5-763">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-763">**Response**:</span></span>

<span data-ttu-id="180f5-764">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-764">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-765">Kulcs/érték elemek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="180f5-765">A collection of key/value elements.</span></span> <span data-ttu-id="180f5-766">Mindegyik tartalmazza egy adott esemény típusa óránként csoportosítva események.</span><span class="sxs-lookup"><span data-stu-id="180f5-766">Each one contains the sum of events for a specific event type grouped by hour.</span></span>

* <span data-ttu-id="180f5-767">`feed\entry[i]\content\properties\Key`– Az idő (óra szerint csoportosítva) és az eseménytípus tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-767">`feed\entry[i]\content\properties\Key` - Contains the time (grouped by hour) and the event type.</span></span>
* <span data-ttu-id="180f5-768">`feed\entry[i]\content\properties\Value`-Esemény teljes száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-768">`feed\entry[i]\content\properties\Value` - Total event count.</span></span>

<span data-ttu-id="180f5-769">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-769">OData XML</span></span>

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

### <a name="94----get-usage-file-sample"></a><span data-ttu-id="180f5-770">9.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-770">9.4.</span></span>    <span data-ttu-id="180f5-771">Használati fájl minta beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-771">Get Usage File Sample</span></span>
<span data-ttu-id="180f5-772">Lekéri az első 2KB használati fájl tartalma.</span><span class="sxs-lookup"><span data-stu-id="180f5-772">Retrieves the first 2KB of usage file content.</span></span>

| <span data-ttu-id="180f5-773">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-773">HTTP Method</span></span> | <span data-ttu-id="180f5-774">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-774">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-775">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-775">GET</span></span> |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-776">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-776">Example:</span></span><br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-777">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-777">Parameter Name</span></span> | <span data-ttu-id="180f5-778">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-778">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-779">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-779">modelId</span></span> |<span data-ttu-id="180f5-780">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-780">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-781">fileId</span><span class="sxs-lookup"><span data-stu-id="180f5-781">fileId</span></span> |<span data-ttu-id="180f5-782">A modellfájl használati egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-782">Unique identifier of the model usage file</span></span> |
| <span data-ttu-id="180f5-783">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-783">apiVersion</span></span> |<span data-ttu-id="180f5-784">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-784">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-785">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-785">Request Body</span></span> |<span data-ttu-id="180f5-786">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-786">NONE</span></span> |

<span data-ttu-id="180f5-787">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-787">**Response**:</span></span>

<span data-ttu-id="180f5-788">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-788">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-789">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="180f5-789">Response is returned in raw text format:</span></span>

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


### <a name="95----get-model-usage-file"></a><span data-ttu-id="180f5-790">9.5.</span><span class="sxs-lookup"><span data-stu-id="180f5-790">9.5.</span></span>    <span data-ttu-id="180f5-791">Használati modellfájl beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-791">Get Model Usage File</span></span>
<span data-ttu-id="180f5-792">Lekéri a használati fájl teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="180f5-792">Retrieves the full content of the usage file.</span></span>

| <span data-ttu-id="180f5-793">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-793">HTTP Method</span></span> | <span data-ttu-id="180f5-794">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-794">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-795">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-795">GET</span></span> |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-796">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-796">Example:</span></span><br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-797">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-797">Parameter Name</span></span> | <span data-ttu-id="180f5-798">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-798">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-799">Mid</span><span class="sxs-lookup"><span data-stu-id="180f5-799">mid</span></span> |<span data-ttu-id="180f5-800">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-800">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-801">FID</span><span class="sxs-lookup"><span data-stu-id="180f5-801">fid</span></span> |<span data-ttu-id="180f5-802">A modellfájl használati egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-802">Unique identifier of the model usage file</span></span> |
| <span data-ttu-id="180f5-803">Letöltése</span><span class="sxs-lookup"><span data-stu-id="180f5-803">download</span></span> |<span data-ttu-id="180f5-804">1</span><span class="sxs-lookup"><span data-stu-id="180f5-804">1</span></span> |
| <span data-ttu-id="180f5-805">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-805">apiVersion</span></span> |<span data-ttu-id="180f5-806">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-806">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-807">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-807">Request Body</span></span> |<span data-ttu-id="180f5-808">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-808">NONE</span></span> |

<span data-ttu-id="180f5-809">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-809">**Response**:</span></span>

<span data-ttu-id="180f5-810">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-810">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-811">Nyers szöveges formátumú választ küld vissza:</span><span class="sxs-lookup"><span data-stu-id="180f5-811">Response is returned in raw text format:</span></span>

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

### <a name="96----delete-usage-file"></a><span data-ttu-id="180f5-812">9.6.</span><span class="sxs-lookup"><span data-stu-id="180f5-812">9.6.</span></span>    <span data-ttu-id="180f5-813">Használati fájl törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-813">Delete Usage File</span></span>
<span data-ttu-id="180f5-814">A megadott modell használati fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="180f5-814">Deletes the specified model usage file.</span></span>

| <span data-ttu-id="180f5-815">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-815">HTTP Method</span></span> | <span data-ttu-id="180f5-816">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-816">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-817">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-817">DELETE</span></span> |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-818">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-818">Example:</span></span><br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-819">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-819">Parameter Name</span></span> | <span data-ttu-id="180f5-820">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-820">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-821">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-821">modelId</span></span> |<span data-ttu-id="180f5-822">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-822">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-823">fileId</span><span class="sxs-lookup"><span data-stu-id="180f5-823">fileId</span></span> |<span data-ttu-id="180f5-824">A fájl törlendő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-824">Unique identifier of the file to be deleted</span></span> |
| <span data-ttu-id="180f5-825">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-825">apiVersion</span></span> |<span data-ttu-id="180f5-826">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-826">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-827">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-827">Request Body</span></span> |<span data-ttu-id="180f5-828">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-828">NONE</span></span> |

<span data-ttu-id="180f5-829">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-829">**Response**:</span></span>

<span data-ttu-id="180f5-830">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-830">HTTP Status code: 200</span></span>

### <a name="97----delete-all-usage-files"></a><span data-ttu-id="180f5-831">9.7.</span><span class="sxs-lookup"><span data-stu-id="180f5-831">9.7.</span></span>    <span data-ttu-id="180f5-832">Minden fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-832">Delete All Usage Files</span></span>
<span data-ttu-id="180f5-833">Törli az összes modell használati fájlt.</span><span class="sxs-lookup"><span data-stu-id="180f5-833">Deletes all model usage files.</span></span>

| <span data-ttu-id="180f5-834">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-834">HTTP Method</span></span> | <span data-ttu-id="180f5-835">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-835">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-836">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-836">DELETE</span></span> |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-837">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-837">Example:</span></span><br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-838">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-838">Parameter Name</span></span> | <span data-ttu-id="180f5-839">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-839">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-840">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-840">modelId</span></span> |<span data-ttu-id="180f5-841">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-841">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-842">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-842">apiVersion</span></span> |<span data-ttu-id="180f5-843">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-843">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-844">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-844">Request Body</span></span> |<span data-ttu-id="180f5-845">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-845">NONE</span></span> |

<span data-ttu-id="180f5-846">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-846">**Response**:</span></span>

<span data-ttu-id="180f5-847">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-847">HTTP Status code: 200</span></span>

## <a name="10-features"></a><span data-ttu-id="180f5-848">10. Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="180f5-848">10. Features</span></span>
<span data-ttu-id="180f5-849">Ez a szakasz bemutatja, hogyan adatbeolvasás funkció, például az importált szolgáltatások és azok értékeit, azok dimenziószáma és a dimenziószáma lett lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="180f5-849">This section shows how to retrieve feature information, such as the imported features and their values, their rank, and when this rank was allocated.</span></span> <span data-ttu-id="180f5-850">Szolgáltatások részeként az eseménykatalógus-adatok importálása, és majd a rangsor tartozik amikor rank build hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="180f5-850">Features are imported as part of the catalog data, and then their rank is associated when a rank build is done.</span></span>
<span data-ttu-id="180f5-851">A szolgáltatás dimenziószáma használati adatok és típusú elemeket a minta alapján módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="180f5-851">Feature rank can change according to the pattern of usage data and type of items.</span></span> <span data-ttu-id="180f5-852">De konzisztens használati elemek, a dimenziószáma csak kis ingadozását kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="180f5-852">But for consistent usage/items, the rank should have only small fluctuations.</span></span>
<span data-ttu-id="180f5-853">A szolgáltatások rangja egy nem negatív szám.</span><span class="sxs-lookup"><span data-stu-id="180f5-853">The rank of features is a non-negative number.</span></span> <span data-ttu-id="180f5-854">A szám 0 azt jelenti, hogy a szolgáltatás nem lett-e rangsorolva (történik, ha ez az API a sorrendet megadó első build befejezése előtt indít).</span><span class="sxs-lookup"><span data-stu-id="180f5-854">The  number 0 means that the feature was not ranked (happens if you invoke this API prior to the completion of the first rank build).</span></span> <span data-ttu-id="180f5-855">A dátum, amelyen a dimenziószáma attribútummal volt a pontszám frissesség nevezik.</span><span class="sxs-lookup"><span data-stu-id="180f5-855">The date at which the rank was attributed is called the score freshness.</span></span>

### <a name="101-get-features-info-for-last-rank-build"></a><span data-ttu-id="180f5-856">10.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-856">10.1.</span></span> <span data-ttu-id="180f5-857">Található szolgáltatások adat (a sorrendet megadó utolsó Buildverziót)</span><span class="sxs-lookup"><span data-stu-id="180f5-857">Get Features Info (For Last Rank Build)</span></span>
<span data-ttu-id="180f5-858">A szolgáltatás információt, beleértve a prioritást, a legutóbbi sikeres rank buildjéhez kéri le.</span><span class="sxs-lookup"><span data-stu-id="180f5-858">Retrieves the feature information, including ranking, for the last successful rank build.</span></span>

| <span data-ttu-id="180f5-859">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-859">HTTP Method</span></span> | <span data-ttu-id="180f5-860">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-860">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-861">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-861">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-862">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-862">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-863">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-863">Parameter Name</span></span> | <span data-ttu-id="180f5-864">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-864">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-865">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-865">modelId</span></span> |<span data-ttu-id="180f5-866">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-866">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-867">samplingSize</span><span class="sxs-lookup"><span data-stu-id="180f5-867">samplingSize</span></span> |<span data-ttu-id="180f5-868">Az egyes szolgáltatásokhoz, a katalógusban szereplő adatok alapján is tartalmazzanak száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-868">Number of values to include for each feature according to the data present in the catalog.</span></span> <br/><span data-ttu-id="180f5-869">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="180f5-869">Possible values are:</span></span><br> <span data-ttu-id="180f5-870">-1 - összes mintát.</span><span class="sxs-lookup"><span data-stu-id="180f5-870">-1 - All samples.</span></span> <br><span data-ttu-id="180f5-871">0 - nem mintavételi.</span><span class="sxs-lookup"><span data-stu-id="180f5-871">0 - No sampling.</span></span> <br><span data-ttu-id="180f5-872">N - N minták minden szolgáltatás nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-872">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="180f5-873">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-873">apiVersion</span></span> |<span data-ttu-id="180f5-874">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-874">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-875">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-875">Request Body</span></span> |<span data-ttu-id="180f5-876">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-876">NONE</span></span> |

<span data-ttu-id="180f5-877">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-877">**Response**:</span></span>

<span data-ttu-id="180f5-878">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-878">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-879">A válasz szolgáltatás info bejegyzések listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-879">The response contains a list of feature info entries.</span></span> <span data-ttu-id="180f5-880">Mindegyik bejegyzés tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="180f5-880">Each entry contains:</span></span>

* <span data-ttu-id="180f5-881">`feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-881">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="180f5-882">`feed/entry/content/m:properties/d:RankUpdateDate`-A dátum, amelyen a dimenziószáma lett lefoglalva Ez a szolgáltatás, más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-882">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which the rank was allocated to this feature, a.k.a.</span></span> <span data-ttu-id="180f5-883">pontszám frissesség szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="180f5-883">score freshness feature.</span></span> <span data-ttu-id="180f5-884">Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.</span><span class="sxs-lookup"><span data-stu-id="180f5-884">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="180f5-885">`feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float).</span><span class="sxs-lookup"><span data-stu-id="180f5-885">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="180f5-886">A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.</span><span class="sxs-lookup"><span data-stu-id="180f5-886">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="180f5-887">`feed/entry/content/m:properties/d:SampleValues`-A kért mintavételi mérete legfeljebb értékek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-887">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up to the sampling size requested.</span></span>

<span data-ttu-id="180f5-888">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-888">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
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

### <a name="102-get-features-info-for-specific-rank-build"></a><span data-ttu-id="180f5-889">10.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-889">10.2.</span></span> <span data-ttu-id="180f5-890">Található szolgáltatások adat (a megadott sorszám Build)</span><span class="sxs-lookup"><span data-stu-id="180f5-890">Get Features Info (For Specific Rank Build)</span></span>
<span data-ttu-id="180f5-891">Például egy adott buildjénél a sorrendet megadó rangsorolási szolgáltatás adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="180f5-891">Retrieves the feature information, including the ranking for a specific rank build.</span></span>

| <span data-ttu-id="180f5-892">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-892">HTTP Method</span></span> | <span data-ttu-id="180f5-893">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-893">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-894">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-894">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-895">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-895">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-896">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-896">Parameter Name</span></span> | <span data-ttu-id="180f5-897">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-897">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-898">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-898">modelId</span></span> |<span data-ttu-id="180f5-899">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-899">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-900">samplingSize</span><span class="sxs-lookup"><span data-stu-id="180f5-900">samplingSize</span></span> |<span data-ttu-id="180f5-901">Az egyes szolgáltatásokhoz, a katalógusban szereplő adatok alapján is tartalmazzanak száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-901">Number of values to include for each feature according to the data present in the catalog.</span></span><br/> <span data-ttu-id="180f5-902">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="180f5-902">Possible values are:</span></span><br> <span data-ttu-id="180f5-903">-1 - összes mintát.</span><span class="sxs-lookup"><span data-stu-id="180f5-903">-1 - All samples.</span></span> <br><span data-ttu-id="180f5-904">0 - nem mintavételi.</span><span class="sxs-lookup"><span data-stu-id="180f5-904">0 - No sampling.</span></span> <br><span data-ttu-id="180f5-905">N - N minták minden szolgáltatás nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-905">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="180f5-906">rankBuildId</span><span class="sxs-lookup"><span data-stu-id="180f5-906">rankBuildId</span></span> |<span data-ttu-id="180f5-907">A sorrendet megadó build vagy -1 -nek a sorrendet megadó utolsó buildverziót egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-907">Unique identifier of the rank build or -1 for the last rank build</span></span> |
| <span data-ttu-id="180f5-908">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-908">apiVersion</span></span> |<span data-ttu-id="180f5-909">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-909">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-910">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-910">Request Body</span></span> |<span data-ttu-id="180f5-911">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-911">NONE</span></span> |

<span data-ttu-id="180f5-912">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-912">**Response**:</span></span>

<span data-ttu-id="180f5-913">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-913">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-914">A válasz szolgáltatás info bejegyzések listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-914">The response contains a list of feature info entries.</span></span> <span data-ttu-id="180f5-915">Mindegyik bejegyzés tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="180f5-915">Each entry contains:</span></span>

* <span data-ttu-id="180f5-916">`feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-916">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="180f5-917">`feed/entry/content/m:properties/d:RankUpdateDate`-A dátum, amelyen a dimenziószáma lett lefoglalva Ez a szolgáltatás, más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-917">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which the rank was allocated to this feature, a.k.a.</span></span> <span data-ttu-id="180f5-918">pontszám frissesség szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="180f5-918">score freshness feature.</span></span> <span data-ttu-id="180f5-919">Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.</span><span class="sxs-lookup"><span data-stu-id="180f5-919">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="180f5-920">`feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float).</span><span class="sxs-lookup"><span data-stu-id="180f5-920">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="180f5-921">A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.</span><span class="sxs-lookup"><span data-stu-id="180f5-921">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="180f5-922">`feed/entry/content/m:properties/d:SampleValues`-A kért mintavételi mérete legfeljebb értékek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-922">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up to the sampling size requested.</span></span>

<span data-ttu-id="180f5-923">OData</span><span class="sxs-lookup"><span data-stu-id="180f5-923">OData</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
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


## <a name="11-build"></a><span data-ttu-id="180f5-924">11. Felépítés</span><span class="sxs-lookup"><span data-stu-id="180f5-924">11. Build</span></span>
  <span data-ttu-id="180f5-925">Ez a szakasz ismerteti a különböző API-k buildek kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="180f5-925">This section explains the different APIs related to builds.</span></span> <span data-ttu-id="180f5-926">3 különböző típusú buildek: javaslat build, a sorrendet megadó build és egy (gyakran vásárolt együtt) FBT build.</span><span class="sxs-lookup"><span data-stu-id="180f5-926">There are 3 types of builds: a recommendation build, a rank build and an FBT (frequently bought together) build.</span></span>

<span data-ttu-id="180f5-927">A javaslat build célja egy előrejelzéseket használt javaslat modell generálásához.</span><span class="sxs-lookup"><span data-stu-id="180f5-927">The recommendation build's purpose is to generate a recommendation model used for predictions.</span></span> <span data-ttu-id="180f5-928">(Az ilyen típusú build) előrejelzéseket térjen két verziója:</span><span class="sxs-lookup"><span data-stu-id="180f5-928">Predictions (for this type of build) come in two flavors:</span></span>

* <span data-ttu-id="180f5-929">I2I – más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-929">I2I - a.k.a.</span></span> <span data-ttu-id="180f5-930">A cikk ajánlottak - elem vagy egy elemlistát, ez a beállítás lesz előrejelzése, amelyek magas érdeklődésére elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="180f5-930">Item to Item recommendations - given an item or a list of items this option will predict a list of items that are likely to be of high interest.</span></span>
* <span data-ttu-id="180f5-931">U2I – más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-931">U2I - a.k.a.</span></span> <span data-ttu-id="180f5-932">Felhasználói elem javaslatok – a megadott felhasználói azonosító (és nem kötelezően elemek listáját) ezt a beállítást fogja előre jelezni, amelyek a megadott felhasználó (és a további elemeket választhat) magas érdeklődésére elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="180f5-932">User to Item recommendations - given a user id (and optionally a list of items) this option will predict a list of items that are likely to be of high interest for the given user (and its additional choice of items).</span></span> <span data-ttu-id="180f5-933">A U2I javaslatok a felhasználó a modell készült idejével érdeklő elemeket előzményeinek alapulnak.</span><span class="sxs-lookup"><span data-stu-id="180f5-933">The U2I recommendations are based on the history of items that were of interest for the user up to the time the model was built.</span></span>

<span data-ttu-id="180f5-934">A sorrendet megadó build, amely lehetővé teszi további információk a szolgáltatások használhatóságát műszaki build.</span><span class="sxs-lookup"><span data-stu-id="180f5-934">A rank build is a technical build that allows you to learn about the usefulness of your features.</span></span> <span data-ttu-id="180f5-935">Általában a legjobb eredmények eléréséhez a szolgáltatásokat érintő javaslat modell, meg kell tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="180f5-935">Usually, in order to get the best result for a recommendation model involving features, you should take the following steps:</span></span>

* <span data-ttu-id="180f5-936">Aktiválhatja a sorrendet megadó build, (kivéve, ha a szolgáltatások a pontszám stabil), és várjon, amíg a szolgáltatás pontszámot kap.</span><span class="sxs-lookup"><span data-stu-id="180f5-936">Trigger a rank build (unless the score of your features is stable) and wait till you get the feature score.</span></span>
* <span data-ttu-id="180f5-937">A szolgáltatások rangja meghívásával beolvasása a [szolgáltatások információ](#101-get-features-info-for-last-rank-build) API.</span><span class="sxs-lookup"><span data-stu-id="180f5-937">Retrieve the rank of your features by calling the [Get Features Info](#101-get-features-info-for-last-rank-build) API.</span></span>
* <span data-ttu-id="180f5-938">A javaslat build konfigurálása a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="180f5-938">Configure a recommendation build with the following parameters:</span></span>
  * <span data-ttu-id="180f5-939">`useFeatureInModel`-Igaz értékre kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="180f5-939">`useFeatureInModel` - Set to True.</span></span>
  * <span data-ttu-id="180f5-940">`ModelingFeatureList`– Állítsa a 2.0-s vagy több (az egyes holtversenyekben az előző lépésben lekért) megfelelően pontszámot funkcióinak vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-940">`ModelingFeatureList` - Set to a comma-separated list of features with a score of 2.0 or more (according to the ranks you retrieved in the previous step).</span></span>
  * <span data-ttu-id="180f5-941">`AllowColdItemPlacement`-Igaz értékre kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="180f5-941">`AllowColdItemPlacement` - Set to True.</span></span>
  * <span data-ttu-id="180f5-942">Opcionálisan megadhat `EnableFeatureCorrelation` TRUE és `ReasoningFeatureList` szolgáltatások listájához magyarázatot (általában ugyanaz a szolgáltatások listájában modellezés vagy allista) használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="180f5-942">Optionally you can set `EnableFeatureCorrelation` to True and `ReasoningFeatureList` to the list of features you want to use for explanations (usually the same list of features used in modelling or a sublist).</span></span>
* <span data-ttu-id="180f5-943">Indítás, a javaslat összeállítása a konfigurált adatokkal.</span><span class="sxs-lookup"><span data-stu-id="180f5-943">Trigger the recommendation build with the configured parameters.</span></span>

<span data-ttu-id="180f5-944">Megjegyzés: Ha nem adja meg a paramétereket (pl. meghívása a javaslat build paraméterek nélkül) vagy nem explicit módon letiltja az funkciók használatát (pl. `UseFeatureInModel` értéke hamis), a rendszer a szolgáltatás kapcsolatos paramétereket a magyarázat beállításához a fenti értékek abban az esetben, ha a sorrend első helyén build létezik-e.</span><span class="sxs-lookup"><span data-stu-id="180f5-944">Note: If you do not configure any parameters (e.g. invoke the recommendation build without parameters) or you do not explicitly disable the usage of features (e.g. `UseFeatureInModel` set to False), the system will set up the feature-related parameters to the explained values above in case a rank build exists.</span></span>

<span data-ttu-id="180f5-945">Nincs a sorrendet megadó build és egyidejűleg ugyanannak a modellnek az ajánlás build futó korlátozva.</span><span class="sxs-lookup"><span data-stu-id="180f5-945">There is no restriction on running a rank build and a recommendation build concurrently for the same model.</span></span> <span data-ttu-id="180f5-946">Ettől függetlenül ugyanannak a modellnek párhuzamosan azonos típusú két változata nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="180f5-946">Nevertheless, you cannot run two builds of the same type on the same model in parallel.</span></span>

<span data-ttu-id="180f5-947">Egy (gyakran vásárolt együtt) FBT build még egy másik javaslatok algoritmus nevezik néha "konzervatív" ajánló, ami ideális, amelyek nincsenek homogén jellegű katalógusok (homogén: könyvek, filmek, néhány étele módon; nem homogén: számítógép és eszközök, a tartományok közötti, rendkívül sokféle).</span><span class="sxs-lookup"><span data-stu-id="180f5-947">An FBT (Frequently bought together) build is yet another recommendations algorithm called sometimes "conservative" recommender, which is good for catalogs that are not homogeneous in nature (homogeneous: books, movies, some food, fashion; non-homogeneous: computer and devices, cross-domain, highly diverse).</span></span>

<span data-ttu-id="180f5-948">Megjegyzés: Ha a feltöltött fájlok tartalmazzák a nem kötelező mező "eseménytípus" majd FBT a modellezési csak "a Vásárlás" események lesz.</span><span class="sxs-lookup"><span data-stu-id="180f5-948">Note: if the usage files that you uploaded contain the optional field "event type" then for FBT modelling only "Purchase" events will be used.</span></span> <span data-ttu-id="180f5-949">Ha nincs eseménytípus biztosítja az összes esemény beszerzési kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="180f5-949">If no event type is provided all events will be considered as purchase.</span></span>

#### <a name="111-build-parameters"></a><span data-ttu-id="180f5-950">11.1 build paraméterek</span><span class="sxs-lookup"><span data-stu-id="180f5-950">11.1 Build parameters</span></span>
<span data-ttu-id="180f5-951">Minden build típusú paraméterek (kitaláltak alább) segítségével konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="180f5-951">Each build type can be configured via a set of parameters (depicted below).</span></span> <span data-ttu-id="180f5-952">Ha nem adja meg a paraméterek, a rendszer automatikusan attribútumok értéke a paraméterek build indít el a jelen információ alapján.</span><span class="sxs-lookup"><span data-stu-id="180f5-952">If you don't configure the parameters, the system will automatically attribute values to the parameters according to the information present at the time you trigger a build.</span></span>

##### <a name="1111-usage-condenser"></a><span data-ttu-id="180f5-953">11.1.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-953">11.1.1.</span></span> <span data-ttu-id="180f5-954">Használati hűtő</span><span class="sxs-lookup"><span data-stu-id="180f5-954">Usage condenser</span></span>
<span data-ttu-id="180f5-955">Felhasználók, illetve néhány használati pont cikkeket további zaj mint információkat is tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="180f5-955">Users or items with few usage points might contain more noise than information.</span></span> <span data-ttu-id="180f5-956">A rendszer megkísérli előrejelzése használati pontok modellben használandó felhasználói/elemenként minimális száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-956">The system attempts to predict the minimal number of usage points per user/item to be used in a model.</span></span> <span data-ttu-id="180f5-957">Ez a szám lesz elemek ItemCutoffLowerBound és ItemCutoffUpperBound paramétereinek megadott tartományt, és a UserCutOffLowerBound és UserCutoffUpperBound paraméterek, a felhasználók által megadott tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="180f5-957">This number will be within the range defined by the ItemCutoffLowerBound and ItemCutoffUpperBound parameters for items, and the range defined by the UserCutOffLowerBound and UserCutoffUpperBound parameters for users.</span></span> <span data-ttu-id="180f5-958">Az elemek vagy felhasználók hűtő hatással nulla megfelelő határain közül legalább egy beállítást minimálisra csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="180f5-958">The condenser effect on items or users can be minimized by setting at least one of the corresponding bounds to zero.</span></span>

##### <a name="1112-rank-build-parameters"></a><span data-ttu-id="180f5-959">11.1.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-959">11.1.2.</span></span> <span data-ttu-id="180f5-960">Dimenziószáma build paraméterek</span><span class="sxs-lookup"><span data-stu-id="180f5-960">Rank build parameters</span></span>
<span data-ttu-id="180f5-961">Az alábbi táblázat mutatja be, hogy a sorrendet megadó build build paramétereinek.</span><span class="sxs-lookup"><span data-stu-id="180f5-961">The table below depicts the build parameters for a rank build.</span></span>

| <span data-ttu-id="180f5-962">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-962">Key</span></span> | <span data-ttu-id="180f5-963">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-963">Description</span></span> | <span data-ttu-id="180f5-964">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-964">Type</span></span> | <span data-ttu-id="180f5-965">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="180f5-965">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="180f5-966">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="180f5-966">NumberOfModelIterations</span></span> |<span data-ttu-id="180f5-967">Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="180f5-967">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="180f5-968">Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="180f5-968">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="180f5-969">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-969">Integer</span></span> |<span data-ttu-id="180f5-970">10-50</span><span class="sxs-lookup"><span data-stu-id="180f5-970">10-50</span></span> |
| <span data-ttu-id="180f5-971">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="180f5-971">NumberOfModelDimensions</span></span> |<span data-ttu-id="180f5-972">A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="180f5-972">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="180f5-973">Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra.</span><span class="sxs-lookup"><span data-stu-id="180f5-973">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="180f5-974">Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között.</span><span class="sxs-lookup"><span data-stu-id="180f5-974">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="180f5-975">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-975">Integer</span></span> |<span data-ttu-id="180f5-976">10-40</span><span class="sxs-lookup"><span data-stu-id="180f5-976">10-40</span></span> |
| <span data-ttu-id="180f5-977">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-977">ItemCutOffLowerBound</span></span> |<span data-ttu-id="180f5-978">Meghatározza a hűtő elem alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-978">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="180f5-979">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-979">See usage condenser above.</span></span> |<span data-ttu-id="180f5-980">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-980">Integer</span></span> |<span data-ttu-id="180f5-981">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-981">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-982">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-982">ItemCutOffUpperBound</span></span> |<span data-ttu-id="180f5-983">Meghatározza az elem a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-983">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="180f5-984">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-984">See usage condenser above.</span></span> |<span data-ttu-id="180f5-985">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-985">Integer</span></span> |<span data-ttu-id="180f5-986">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-986">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-987">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-987">UserCutOffLowerBound</span></span> |<span data-ttu-id="180f5-988">Meghatározza a hűtő felhasználói alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-988">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="180f5-989">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-989">See usage condenser above.</span></span> |<span data-ttu-id="180f5-990">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-990">Integer</span></span> |<span data-ttu-id="180f5-991">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-991">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-992">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-992">UserCutOffUpperBound</span></span> |<span data-ttu-id="180f5-993">Határozza meg a felhasználó a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-993">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="180f5-994">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-994">See usage condenser above.</span></span> |<span data-ttu-id="180f5-995">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-995">Integer</span></span> |<span data-ttu-id="180f5-996">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-996">2 or more (0 disable condenser)</span></span> |

##### <a name="1113-recommendation-build-parameters"></a><span data-ttu-id="180f5-997">11.1.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-997">11.1.3.</span></span> <span data-ttu-id="180f5-998">A javaslat build paraméterek</span><span class="sxs-lookup"><span data-stu-id="180f5-998">Recommendation build parameters</span></span>
<span data-ttu-id="180f5-999">Az alábbi táblázat a javaslat build build paramétereinek ábrázol.</span><span class="sxs-lookup"><span data-stu-id="180f5-999">The table below depicts the build parameters for recommendation build.</span></span>

| <span data-ttu-id="180f5-1000">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-1000">Key</span></span> | <span data-ttu-id="180f5-1001">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-1001">Description</span></span> | <span data-ttu-id="180f5-1002">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-1002">Type</span></span> | <span data-ttu-id="180f5-1003">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="180f5-1003">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="180f5-1004">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="180f5-1004">NumberOfModelIterations</span></span> |<span data-ttu-id="180f5-1005">Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="180f5-1005">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="180f5-1006">Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="180f5-1006">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="180f5-1007">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1007">Integer</span></span> |<span data-ttu-id="180f5-1008">10-50</span><span class="sxs-lookup"><span data-stu-id="180f5-1008">10-50</span></span> |
| <span data-ttu-id="180f5-1009">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="180f5-1009">NumberOfModelDimensions</span></span> |<span data-ttu-id="180f5-1010">A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="180f5-1010">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="180f5-1011">Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra.</span><span class="sxs-lookup"><span data-stu-id="180f5-1011">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="180f5-1012">Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között.</span><span class="sxs-lookup"><span data-stu-id="180f5-1012">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="180f5-1013">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1013">Integer</span></span> |<span data-ttu-id="180f5-1014">10-40</span><span class="sxs-lookup"><span data-stu-id="180f5-1014">10-40</span></span> |
| <span data-ttu-id="180f5-1015">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1015">ItemCutOffLowerBound</span></span> |<span data-ttu-id="180f5-1016">Meghatározza a hűtő elem alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1016">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="180f5-1017">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1017">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1018">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1018">Integer</span></span> |<span data-ttu-id="180f5-1019">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1019">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1020">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1020">ItemCutOffUpperBound</span></span> |<span data-ttu-id="180f5-1021">Meghatározza az elem a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1021">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="180f5-1022">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1022">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1023">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1023">Integer</span></span> |<span data-ttu-id="180f5-1024">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1024">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1025">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1025">UserCutOffLowerBound</span></span> |<span data-ttu-id="180f5-1026">Meghatározza a hűtő felhasználói alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1026">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="180f5-1027">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1027">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1028">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1028">Integer</span></span> |<span data-ttu-id="180f5-1029">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1029">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1030">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1030">UserCutOffUpperBound</span></span> |<span data-ttu-id="180f5-1031">Határozza meg a felhasználó a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1031">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="180f5-1032">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1032">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1033">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1033">Integer</span></span> |<span data-ttu-id="180f5-1034">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1034">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1035">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-1035">Description</span></span> |<span data-ttu-id="180f5-1036">Build leírása.</span><span class="sxs-lookup"><span data-stu-id="180f5-1036">Build description.</span></span> |<span data-ttu-id="180f5-1037">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1037">String</span></span> |<span data-ttu-id="180f5-1038">A szöveg, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1038">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="180f5-1039">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="180f5-1039">EnableModelingInsights</span></span> |<span data-ttu-id="180f5-1040">A javaslat modell metrikák számítási teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="180f5-1040">Allows you to compute metrics on the recommendation model.</span></span> |<span data-ttu-id="180f5-1041">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1041">Boolean</span></span> |<span data-ttu-id="180f5-1042">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1042">True/False</span></span> |
| <span data-ttu-id="180f5-1043">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="180f5-1043">UseFeaturesInModel</span></span> |<span data-ttu-id="180f5-1044">Azt jelzi, ha szolgáltatása használható a javaslat modell növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="180f5-1044">Indicates if features can be used in order to enhance the recommendation model.</span></span> |<span data-ttu-id="180f5-1045">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1045">Boolean</span></span> |<span data-ttu-id="180f5-1046">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1046">True/False</span></span> |
| <span data-ttu-id="180f5-1047">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="180f5-1047">ModelingFeatureList</span></span> |<span data-ttu-id="180f5-1048">A javaslat növelése érdekében a javaslat építés használandó neveinek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1048">Comma-separated list of feature names to be used in the recommendation build, in order to enhance the recommendation.</span></span> |<span data-ttu-id="180f5-1049">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1049">String</span></span> |<span data-ttu-id="180f5-1050">Funkcióneveket, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1050">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="180f5-1051">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="180f5-1051">AllowColdItemPlacement</span></span> |<span data-ttu-id="180f5-1052">Azt jelzi, ha a javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül.</span><span class="sxs-lookup"><span data-stu-id="180f5-1052">Indicates if the recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="180f5-1053">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1053">Boolean</span></span> |<span data-ttu-id="180f5-1054">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1054">True/False</span></span> |
| <span data-ttu-id="180f5-1055">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="180f5-1055">EnableFeatureCorrelation</span></span> |<span data-ttu-id="180f5-1056">Azt jelzi, ha szolgáltatások indoklást is használható.</span><span class="sxs-lookup"><span data-stu-id="180f5-1056">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="180f5-1057">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1057">Boolean</span></span> |<span data-ttu-id="180f5-1058">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1058">True/False</span></span> |
| <span data-ttu-id="180f5-1059">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="180f5-1059">ReasoningFeatureList</span></span> |<span data-ttu-id="180f5-1060">Vesszővel tagolt listája mintafelismerési mondat (pl. javaslat magyarázatokat) használt.</span><span class="sxs-lookup"><span data-stu-id="180f5-1060">Comma-separated list of feature names to be used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="180f5-1061">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1061">String</span></span> |<span data-ttu-id="180f5-1062">Funkcióneveket, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1062">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="180f5-1063">EnableU2I</span><span class="sxs-lookup"><span data-stu-id="180f5-1063">EnableU2I</span></span> |<span data-ttu-id="180f5-1064">A személyre szabott javaslat más néven engedélyezése</span><span class="sxs-lookup"><span data-stu-id="180f5-1064">Allow the personalized recommendation a.k.a.</span></span> <span data-ttu-id="180f5-1065">U2I (elem javaslatok felhasználó).</span><span class="sxs-lookup"><span data-stu-id="180f5-1065">U2I (user to item recommendations).</span></span> |<span data-ttu-id="180f5-1066">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1066">Boolean</span></span> |<span data-ttu-id="180f5-1067">Igaz/hamis (alapértelmezett értéke igaz)</span><span class="sxs-lookup"><span data-stu-id="180f5-1067">True/False (default true)</span></span> |

##### <a name="1114-fbt-build-parameters"></a><span data-ttu-id="180f5-1068">11.1.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-1068">11.1.4.</span></span> <span data-ttu-id="180f5-1069">FBT build paraméterek</span><span class="sxs-lookup"><span data-stu-id="180f5-1069">FBT build parameters</span></span>
<span data-ttu-id="180f5-1070">Az alábbi táblázat a javaslat build build paramétereinek ábrázol.</span><span class="sxs-lookup"><span data-stu-id="180f5-1070">The table below depicts the build parameters for recommendation build.</span></span>

| <span data-ttu-id="180f5-1071">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-1071">Key</span></span> | <span data-ttu-id="180f5-1072">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-1072">Description</span></span> | <span data-ttu-id="180f5-1073">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-1073">Type</span></span> | <span data-ttu-id="180f5-1074">Érvénytelen érték (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="180f5-1074">Valid Value (Default)</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="180f5-1075">FbtSupportThreshold</span><span class="sxs-lookup"><span data-stu-id="180f5-1075">FbtSupportThreshold</span></span> |<span data-ttu-id="180f5-1076">Hogyan konzervatív a modell van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1076">How conservative the model is.</span></span> <span data-ttu-id="180f5-1077">Figyelembe kell venni modellezési elemek közös előfordulásainak száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-1077">Number of co-occurrences of items to be considered for modeling.</span></span> |<span data-ttu-id="180f5-1078">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1078">Integer</span></span> |<span data-ttu-id="180f5-1079">3-50 (6)</span><span class="sxs-lookup"><span data-stu-id="180f5-1079">3-50 (6)</span></span> |
| <span data-ttu-id="180f5-1080">FbtMaxItemSetSize</span><span class="sxs-lookup"><span data-stu-id="180f5-1080">FbtMaxItemSetSize</span></span> |<span data-ttu-id="180f5-1081">Bounds gyakori csoportban lévő elemek száma.</span><span class="sxs-lookup"><span data-stu-id="180f5-1081">Bounds the number of items in a frequent set.</span></span> |<span data-ttu-id="180f5-1082">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1082">Integer</span></span> |<span data-ttu-id="180f5-1083">2-3 (2)</span><span class="sxs-lookup"><span data-stu-id="180f5-1083">2-3 (2)</span></span> |
| <span data-ttu-id="180f5-1084">FbtMinimalScore</span><span class="sxs-lookup"><span data-stu-id="180f5-1084">FbtMinimalScore</span></span> |<span data-ttu-id="180f5-1085">Egy gyakori készlet kell rendelkeznie kell ahhoz, hogy a keresés eredményeit minimális pontszámot.</span><span class="sxs-lookup"><span data-stu-id="180f5-1085">Minimal score that a frequent set should have in order to be included in the returned results.</span></span> <span data-ttu-id="180f5-1086">Minél nagyobb a megfelelőbb.</span><span class="sxs-lookup"><span data-stu-id="180f5-1086">The higher the better.</span></span> |<span data-ttu-id="180f5-1087">Dupla</span><span class="sxs-lookup"><span data-stu-id="180f5-1087">Double</span></span> |<span data-ttu-id="180f5-1088">0 és újabb (0)</span><span class="sxs-lookup"><span data-stu-id="180f5-1088">0 and above (0)</span></span> |
| <span data-ttu-id="180f5-1089">FbtSimilarityFunction</span><span class="sxs-lookup"><span data-stu-id="180f5-1089">FbtSimilarityFunction</span></span> |<span data-ttu-id="180f5-1090">Meghatározza a hasonlóság működnek, mint a build használják.</span><span class="sxs-lookup"><span data-stu-id="180f5-1090">Defines the similarity function to be used by the build.</span></span> <span data-ttu-id="180f5-1091">Növekedési előtérbe serendipity, közös előfordulási előtérbe kiszámíthatóságot és Jaccard a két töltött kompromisszumot.</span><span class="sxs-lookup"><span data-stu-id="180f5-1091">Lift favors serendipity, Co-occurrence favors predictability, and Jaccard is a nice compromise between the two.</span></span> |<span data-ttu-id="180f5-1092">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1092">String</span></span> |<span data-ttu-id="180f5-1093">cooccurrence, növekedési, jaccard (növekedési)</span><span class="sxs-lookup"><span data-stu-id="180f5-1093">cooccurrence, lift, jaccard (lift)</span></span> |

### <a name="112-trigger-a-recommendation-build"></a><span data-ttu-id="180f5-1094">11.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-1094">11.2.</span></span> <span data-ttu-id="180f5-1095">A javaslat Build eseményindító</span><span class="sxs-lookup"><span data-stu-id="180f5-1095">Trigger a Recommendation Build</span></span>
  <span data-ttu-id="180f5-1096">Alapértelmezés szerint ez az API javaslat modell build vált.</span><span class="sxs-lookup"><span data-stu-id="180f5-1096">By default this API will trigger a recommendation model build.</span></span> <span data-ttu-id="180f5-1097">Elindítani a sorrendet megadó build (ahhoz, hogy szolgáltatások pontozása), a build API variant build típusú paraméterrel kell használni.</span><span class="sxs-lookup"><span data-stu-id="180f5-1097">To trigger a rank build (in order to score  features), the build API variant with build type parameter should be used.</span></span>

| <span data-ttu-id="180f5-1098">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1098">HTTP Method</span></span> | <span data-ttu-id="180f5-1099">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1099">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1100">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-1100">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1101">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1101">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| <span data-ttu-id="180f5-1102">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="180f5-1102">HEADER</span></span> |<span data-ttu-id="180f5-1103">`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése)</span><span class="sxs-lookup"><span data-stu-id="180f5-1103">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="180f5-1104">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1104">Parameter Name</span></span> | <span data-ttu-id="180f5-1105">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1105">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1106">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1106">modelId</span></span> |<span data-ttu-id="180f5-1107">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1107">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1108">userDescription</span><span class="sxs-lookup"><span data-stu-id="180f5-1108">userDescription</span></span> |<span data-ttu-id="180f5-1109">A katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1109">Textual identifier of the catalog.</span></span> <span data-ttu-id="180f5-1110">Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette.</span><span class="sxs-lookup"><span data-stu-id="180f5-1110">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="180f5-1111">Tekintse meg a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="180f5-1111">See example above.</span></span><br><span data-ttu-id="180f5-1112">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-1112">Max length: 50</span></span> |
| <span data-ttu-id="180f5-1113">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1113">apiVersion</span></span> |<span data-ttu-id="180f5-1114">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1114">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-1115">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-1115">Request Body</span></span> |<span data-ttu-id="180f5-1116">Ha üres majd a build fogja végrehajtani az alapértelmezett build paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="180f5-1116">If left empty then the build will be executed with the default build parameters.</span></span><br><br><span data-ttu-id="180f5-1117">Ha be szeretné állítani a build paraméterek, küldje el a paraméterek XML törzsébe, ahogy az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="180f5-1117">If you want to set the build parameters, send the parameters as XML into the body as in the following sample.</span></span> <span data-ttu-id="180f5-1118">(Lásd a "Paraméterek létrehozása" című szakasz annak magyarázatát, a paraméterek.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="180f5-1118">(See the "Build parameters" section for an explanation of the parameters.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span></span> |

<span data-ttu-id="180f5-1119">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1119">**Response**:</span></span>

<span data-ttu-id="180f5-1120">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1120">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1121">Ez az egy aszinkron API-t.</span><span class="sxs-lookup"><span data-stu-id="180f5-1121">This is an asynchronous API.</span></span> <span data-ttu-id="180f5-1122">Válaszként elérhetővé válik egy build azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1122">You will get a build ID as a response.</span></span> <span data-ttu-id="180f5-1123">Ha a build véget ért, kell az "Első buildek állapota az egy modell" API, és keresse meg a választ a build Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1123">To know when the build has ended, you should call the “Get Builds Status of a Model” API and locate this build ID in the response.</span></span> <span data-ttu-id="180f5-1124">Fontos tudni, hogy build is perces óra adatok méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="180f5-1124">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="180f5-1125">Nem lehet felhasználni a build: javaslatok karakterlánccal végződik-e.</span><span class="sxs-lookup"><span data-stu-id="180f5-1125">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="180f5-1126">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="180f5-1126">Valid build status:</span></span>

* <span data-ttu-id="180f5-1127">Hozzon létre - létrehozási kérelem lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="180f5-1127">Create - Build request was created.</span></span>
* <span data-ttu-id="180f5-1128">Aszinkron - Build kérést küld, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="180f5-1128">Queued - Build request was sent and it is queued.</span></span>
* <span data-ttu-id="180f5-1129">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1129">Building - Build is in progress.</span></span>
* <span data-ttu-id="180f5-1130">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="180f5-1130">Success - Build ended successfully.</span></span>
* <span data-ttu-id="180f5-1131">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="180f5-1131">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="180f5-1132">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="180f5-1132">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="180f5-1133">Kapcsolódó - a build megszakítási kérelmet küldött.</span><span class="sxs-lookup"><span data-stu-id="180f5-1133">Cancelling - A cancel request for the build was sent.</span></span>

<span data-ttu-id="180f5-1134">Vegye figyelembe, hogy megtalálható-e a build azonosítója a következő elérési úton:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="180f5-1134">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="180f5-1135">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1135">OData XML</span></span>

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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a><span data-ttu-id="180f5-1136">11.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-1136">11.3.</span></span> <span data-ttu-id="180f5-1137">Eseményindító Build (javaslat, dimenziószáma vagy FBT)</span><span class="sxs-lookup"><span data-stu-id="180f5-1137">Trigger Build (Recommendation, Rank or FBT)</span></span>
| <span data-ttu-id="180f5-1138">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1138">HTTP Method</span></span> | <span data-ttu-id="180f5-1139">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1139">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1140">POST</span><span class="sxs-lookup"><span data-stu-id="180f5-1140">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1141">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1141">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| <span data-ttu-id="180f5-1142">FEJLÉC</span><span class="sxs-lookup"><span data-stu-id="180f5-1142">HEADER</span></span> |<span data-ttu-id="180f5-1143">`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése)</span><span class="sxs-lookup"><span data-stu-id="180f5-1143">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="180f5-1144">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1144">Parameter Name</span></span> | <span data-ttu-id="180f5-1145">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1145">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1146">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1146">modelId</span></span> |<span data-ttu-id="180f5-1147">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1147">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1148">userDescription</span><span class="sxs-lookup"><span data-stu-id="180f5-1148">userDescription</span></span> |<span data-ttu-id="180f5-1149">A katalógus szöveges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1149">Textual identifier of the catalog.</span></span> <span data-ttu-id="180f5-1150">Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette.</span><span class="sxs-lookup"><span data-stu-id="180f5-1150">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="180f5-1151">Tekintse meg a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="180f5-1151">See example above.</span></span><br><span data-ttu-id="180f5-1152">Maximális hossz: 50</span><span class="sxs-lookup"><span data-stu-id="180f5-1152">Max length: 50</span></span> |
| <span data-ttu-id="180f5-1153">buildType</span><span class="sxs-lookup"><span data-stu-id="180f5-1153">buildType</span></span> |<span data-ttu-id="180f5-1154">A meghívni kívánt build típusú:</span><span class="sxs-lookup"><span data-stu-id="180f5-1154">Type of the build to invoke:</span></span> <br/> <span data-ttu-id="180f5-1155">-Javaslat build "ajánlott"</span><span class="sxs-lookup"><span data-stu-id="180f5-1155">- 'Recommendation' for recommendation build</span></span> <br> <span data-ttu-id="180f5-1156">-"Prioritás" rank buildjéhez</span><span class="sxs-lookup"><span data-stu-id="180f5-1156">- 'Ranking' for rank build</span></span> <br/> <span data-ttu-id="180f5-1157">-FBT build "Fbt"</span><span class="sxs-lookup"><span data-stu-id="180f5-1157">- 'Fbt' for FBT build</span></span> |
| <span data-ttu-id="180f5-1158">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1158">apiVersion</span></span> |<span data-ttu-id="180f5-1159">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1159">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-1160">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-1160">Request Body</span></span> |<span data-ttu-id="180f5-1161">Ha üres majd a build fogja végrehajtani az alapértelmezett build paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="180f5-1161">If left empty then the build will be executed with the default build parameters.</span></span><br><br><span data-ttu-id="180f5-1162">Ha be szeretné állítani a build paraméterek, küldje el XML formátumban a szervezet például az alábbi mintában azokat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1162">If you want to set build parameters, send them as XML into the body like in the following sample.</span></span> <span data-ttu-id="180f5-1163">(Lásd a "Paraméterek Build" Magyarázat és a paraméterek teljes listája.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="180f5-1163">(See the "Build parameters" section for an explanation and full list of the parameters.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span></span> |

<span data-ttu-id="180f5-1164">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1164">**Response**:</span></span>

<span data-ttu-id="180f5-1165">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1165">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1166">Ez az egy aszinkron API-t.</span><span class="sxs-lookup"><span data-stu-id="180f5-1166">This is an asynchronous API.</span></span> <span data-ttu-id="180f5-1167">Válaszként elérhetővé válik egy build azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1167">You will get a build ID as a response.</span></span> <span data-ttu-id="180f5-1168">Ha a build véget ért, kell az "Első buildek állapota az egy modell" API, és keresse meg a választ a build Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1168">To know when the build has ended, you should call the “Get Builds Status of a Model” API and locate this build ID in the response.</span></span> <span data-ttu-id="180f5-1169">Fontos tudni, hogy build is perces óra adatok méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="180f5-1169">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="180f5-1170">Nem lehet felhasználni a build: javaslatok karakterlánccal végződik-e.</span><span class="sxs-lookup"><span data-stu-id="180f5-1170">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="180f5-1171">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="180f5-1171">Valid build status:</span></span>

* <span data-ttu-id="180f5-1172">Hozzon létre - modell lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="180f5-1172">Create - Model was created.</span></span>
* <span data-ttu-id="180f5-1173">Aszinkron - modell build lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="180f5-1173">Queued - Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="180f5-1174">Épület - modell éppen készül.</span><span class="sxs-lookup"><span data-stu-id="180f5-1174">Building - Model is being built.</span></span>
* <span data-ttu-id="180f5-1175">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="180f5-1175">Success - Build ended successfully.</span></span>
* <span data-ttu-id="180f5-1176">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="180f5-1176">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="180f5-1177">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="180f5-1177">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="180f5-1178">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="180f5-1178">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="180f5-1179">Vegye figyelembe, hogy megtalálható-e a build azonosítója a következő elérési úton:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="180f5-1179">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="180f5-1180">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1180">OData XML</span></span>

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




### <a name="114-get-builds-status-of-a-model"></a><span data-ttu-id="180f5-1181">11.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-1181">11.4.</span></span> <span data-ttu-id="180f5-1182">A modell buildek állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-1182">Get Builds Status of a Model</span></span>
<span data-ttu-id="180f5-1183">Lekéri a buildek és azok állapotát a megadott modell.</span><span class="sxs-lookup"><span data-stu-id="180f5-1183">Retrieves builds and their status for a specified model.</span></span>

| <span data-ttu-id="180f5-1184">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1184">HTTP Method</span></span> | <span data-ttu-id="180f5-1185">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1185">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1186">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1186">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1187">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1187">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1188">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1188">Parameter Name</span></span> | <span data-ttu-id="180f5-1189">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1189">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1190">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1190">modelId</span></span> |<span data-ttu-id="180f5-1191">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1191">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1192">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="180f5-1192">onlyLastBuild</span></span> |<span data-ttu-id="180f5-1193">Azt jelzi, hogy a modell létrehozási előzmények vagy csak a legutóbbi build állapotának visszaadása</span><span class="sxs-lookup"><span data-stu-id="180f5-1193">Indicates whether to return all the build history of the model or only the status of the most recent build</span></span> |
| <span data-ttu-id="180f5-1194">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1194">apiVersion</span></span> |<span data-ttu-id="180f5-1195">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1195">1.0</span></span> |

<span data-ttu-id="180f5-1196">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1196">**Response**:</span></span>

<span data-ttu-id="180f5-1197">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1197">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1198">A válasz tartalmazza a build egy tételt.</span><span class="sxs-lookup"><span data-stu-id="180f5-1198">The response includes one entry per build.</span></span> <span data-ttu-id="180f5-1199">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1199">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1200">`feed/entry/content/properties/UserName`-Felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1200">`feed/entry/content/properties/UserName` - Name of the user.</span></span>
* <span data-ttu-id="180f5-1201">`feed/entry/content/properties/ModelName`-A modell neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-1201">`feed/entry/content/properties/ModelName` - Name of the model.</span></span>
* <span data-ttu-id="180f5-1202">`feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1202">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="180f5-1203">`feed/entry/content/properties/IsDeployed`-E a buildverziót központilag telepítik (más néven</span><span class="sxs-lookup"><span data-stu-id="180f5-1203">`feed/entry/content/properties/IsDeployed` - Whether the build is deployed (a.k.a.</span></span> <span data-ttu-id="180f5-1204">aktív build).</span><span class="sxs-lookup"><span data-stu-id="180f5-1204">active build).</span></span>
* <span data-ttu-id="180f5-1205">`feed/entry/content/properties/BuildId`-Build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1205">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="180f5-1206">`feed/entry/content/properties/BuildType`-A build típusú.</span><span class="sxs-lookup"><span data-stu-id="180f5-1206">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="180f5-1207">`feed/entry/content/properties/Status`-Létrehozási állapot.</span><span class="sxs-lookup"><span data-stu-id="180f5-1207">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="180f5-1208">A következők egyike lehet: Hiba történt, épület, várakozik, Cancelling, megszakítva, sikeres.</span><span class="sxs-lookup"><span data-stu-id="180f5-1208">Can be one of the following: Error, Building, Queued, Cancelling, Cancelled, Success.</span></span>
* <span data-ttu-id="180f5-1209">`feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak bizonyos állapotaihoz vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="180f5-1209">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="180f5-1210">`feed/entry/content/properties/Progress`-Build (%) folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1210">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="180f5-1211">`feed/entry/content/properties/StartTime`-Build kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1211">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="180f5-1212">`feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1212">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="180f5-1213">`feed/entry/content/properties/ExecutionTime`-Build időtartama.</span><span class="sxs-lookup"><span data-stu-id="180f5-1213">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="180f5-1214">`feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális állapotának részleteit.</span><span class="sxs-lookup"><span data-stu-id="180f5-1214">`feed/entry/content/properties/ProgressStep` - Details about the current stage of a build in progress.</span></span>

<span data-ttu-id="180f5-1215">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="180f5-1215">Valid build status:</span></span>

* <span data-ttu-id="180f5-1216">Létre - létrehozási kérelem tétel jött létre.</span><span class="sxs-lookup"><span data-stu-id="180f5-1216">Created - Build request entry was created.</span></span>
* <span data-ttu-id="180f5-1217">Aszinkron - kérelmet lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="180f5-1217">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="180f5-1218">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1218">Building - Build is in process.</span></span>
* <span data-ttu-id="180f5-1219">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="180f5-1219">Success - Build ended successfully.</span></span>
* <span data-ttu-id="180f5-1220">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="180f5-1220">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="180f5-1221">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="180f5-1221">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="180f5-1222">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="180f5-1222">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="180f5-1223">Build típusú engedélyezett érvényes értékek:</span><span class="sxs-lookup"><span data-stu-id="180f5-1223">Valid values for build type:</span></span>

* <span data-ttu-id="180f5-1224">Sorszám - dimenziószáma felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1224">Rank - Rank build.</span></span>
* <span data-ttu-id="180f5-1225">A javaslat - javaslat build.</span><span class="sxs-lookup"><span data-stu-id="180f5-1225">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="180f5-1226">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1226">OData XML</span></span>

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


### <a name="115-get-builds-status"></a><span data-ttu-id="180f5-1227">11.5.</span><span class="sxs-lookup"><span data-stu-id="180f5-1227">11.5.</span></span> <span data-ttu-id="180f5-1228">Buildek állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-1228">Get Builds Status</span></span>
<span data-ttu-id="180f5-1229">Lekéri az összes modellt a felhasználói állapotok felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1229">Retrieves build statuses of all models of a user.</span></span>

| <span data-ttu-id="180f5-1230">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1230">HTTP Method</span></span> | <span data-ttu-id="180f5-1231">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1231">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1232">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1232">GET</span></span> |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1233">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1233">Example:</span></span><br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1234">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1234">Parameter Name</span></span> | <span data-ttu-id="180f5-1235">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1235">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1236">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="180f5-1236">onlyLastBuild</span></span> |<span data-ttu-id="180f5-1237">Azt jelzi, hogy a modell létrehozási előzményei, vagy csak a legutóbbi build állapotát.</span><span class="sxs-lookup"><span data-stu-id="180f5-1237">Indicates whether to return all the build history of the model or only the status of the most recent build.</span></span> |
| <span data-ttu-id="180f5-1238">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1238">apiVersion</span></span> |<span data-ttu-id="180f5-1239">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1239">1.0</span></span> |

<span data-ttu-id="180f5-1240">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1240">**Response**:</span></span>

<span data-ttu-id="180f5-1241">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1241">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1242">A válasz tartalmazza a build egy tételt.</span><span class="sxs-lookup"><span data-stu-id="180f5-1242">The response includes one entry per build.</span></span> <span data-ttu-id="180f5-1243">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1243">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1244">`feed/entry/content/properties/UserName`-Felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1244">`feed/entry/content/properties/UserName` - Name of the user.</span></span>
* <span data-ttu-id="180f5-1245">`feed/entry/content/properties/ModelName`-A modell neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-1245">`feed/entry/content/properties/ModelName` - Name of the model.</span></span>
* <span data-ttu-id="180f5-1246">`feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1246">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="180f5-1247">`feed/entry/content/properties/IsDeployed`-E a buildverziót központilag telepítik.</span><span class="sxs-lookup"><span data-stu-id="180f5-1247">`feed/entry/content/properties/IsDeployed` - Whether the build is deployed.</span></span>
* <span data-ttu-id="180f5-1248">`feed/entry/content/properties/BuildId`-Build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1248">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="180f5-1249">`feed/entry/content/properties/BuildType`-A build típusú.</span><span class="sxs-lookup"><span data-stu-id="180f5-1249">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="180f5-1250">`feed/entry/content/properties/Status`-Létrehozási állapot.</span><span class="sxs-lookup"><span data-stu-id="180f5-1250">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="180f5-1251">A következők egyike lehet: Hiba történt, épület, várakozik, megszakítva, Cancelling, sikeres.</span><span class="sxs-lookup"><span data-stu-id="180f5-1251">Can be one of the following: Error, Building, Queued, Cancelled, Cancelling, Success.</span></span>
* <span data-ttu-id="180f5-1252">`feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak bizonyos állapotaihoz vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="180f5-1252">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="180f5-1253">`feed/entry/content/properties/Progress`-Build (%) folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1253">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="180f5-1254">`feed/entry/content/properties/StartTime`-Build kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1254">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="180f5-1255">`feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1255">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="180f5-1256">`feed/entry/content/properties/ExecutionTime`-Build időtartama.</span><span class="sxs-lookup"><span data-stu-id="180f5-1256">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="180f5-1257">`feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális állapotának részleteit.</span><span class="sxs-lookup"><span data-stu-id="180f5-1257">`feed/entry/content/properties/ProgressStep` - Details about the current stage of a build in progress.</span></span>

<span data-ttu-id="180f5-1258">Érvényes létrehozásának állapota:</span><span class="sxs-lookup"><span data-stu-id="180f5-1258">Valid build status:</span></span>

* <span data-ttu-id="180f5-1259">Létre - létrehozási kérelem tétel jött létre.</span><span class="sxs-lookup"><span data-stu-id="180f5-1259">Created - Build request entry was created.</span></span>
* <span data-ttu-id="180f5-1260">Aszinkron - kérelmet lett elindítva, és azt a várólistára.</span><span class="sxs-lookup"><span data-stu-id="180f5-1260">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="180f5-1261">Épület - Build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1261">Building - Build is in process.</span></span>
* <span data-ttu-id="180f5-1262">Sikeres – a létrehozási művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="180f5-1262">Success - Build ended successfully.</span></span>
* <span data-ttu-id="180f5-1263">Hiba – Build hibával ért véget.</span><span class="sxs-lookup"><span data-stu-id="180f5-1263">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="180f5-1264">Megszakítva - Build megszakították.</span><span class="sxs-lookup"><span data-stu-id="180f5-1264">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="180f5-1265">Kapcsolódó - Build megszakítás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="180f5-1265">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="180f5-1266">Build típusú engedélyezett érvényes értékek:</span><span class="sxs-lookup"><span data-stu-id="180f5-1266">Valid values for build type:</span></span>

* <span data-ttu-id="180f5-1267">Sorszám - dimenziószáma felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1267">Rank - Rank build.</span></span>
* <span data-ttu-id="180f5-1268">A javaslat - javaslat build.</span><span class="sxs-lookup"><span data-stu-id="180f5-1268">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="180f5-1269">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1269">OData XML</span></span>

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


### <a name="116-delete-build"></a><span data-ttu-id="180f5-1270">11.6.</span><span class="sxs-lookup"><span data-stu-id="180f5-1270">11.6.</span></span> <span data-ttu-id="180f5-1271">Build törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-1271">Delete Build</span></span>
<span data-ttu-id="180f5-1272">Build törli.</span><span class="sxs-lookup"><span data-stu-id="180f5-1272">Deletes a build.</span></span>

<span data-ttu-id="180f5-1273">MEGJEGYZÉS:</span><span class="sxs-lookup"><span data-stu-id="180f5-1273">NOTE:</span></span> <br><span data-ttu-id="180f5-1274">Egy aktív build nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="180f5-1274">You cannot delete an active build.</span></span> <span data-ttu-id="180f5-1275">A modell frissíteni kell a különböző aktív buildre törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="180f5-1275">The model should be updated to a different active build before you delete it.</span></span><br><span data-ttu-id="180f5-1276">Egy folyamatban lévő build nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="180f5-1276">You cannot delete an in-progress build.</span></span> <span data-ttu-id="180f5-1277">Törölnie kell a build először meghívásával <strong>Szerkesztés megszakítása</strong>.</span><span class="sxs-lookup"><span data-stu-id="180f5-1277">You should cancel the build first by calling <strong>Cancel Build</strong>.</span></span>

| <span data-ttu-id="180f5-1278">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1278">HTTP Method</span></span> | <span data-ttu-id="180f5-1279">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1279">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1280">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-1280">DELETE</span></span> |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1281">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1281">Example:</span></span><br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1282">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1282">Parameter Name</span></span> | <span data-ttu-id="180f5-1283">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1283">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1284">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1284">buildId</span></span> |<span data-ttu-id="180f5-1285">A build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1285">Unique identifier of the build.</span></span> |
| <span data-ttu-id="180f5-1286">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1286">apiVersion</span></span> |<span data-ttu-id="180f5-1287">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1287">1.0</span></span> |

<span data-ttu-id="180f5-1288">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1288">**Response:**</span></span>

<span data-ttu-id="180f5-1289">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1289">HTTP Status code: 200</span></span>

### <a name="117-cancel-build"></a><span data-ttu-id="180f5-1290">11.7.</span><span class="sxs-lookup"><span data-stu-id="180f5-1290">11.7.</span></span> <span data-ttu-id="180f5-1291">Szakítsa meg a Build</span><span class="sxs-lookup"><span data-stu-id="180f5-1291">Cancel Build</span></span>
<span data-ttu-id="180f5-1292">Állapot fejlesztése során build visszavonása.</span><span class="sxs-lookup"><span data-stu-id="180f5-1292">Cancels a build that is in building status.</span></span>

| <span data-ttu-id="180f5-1293">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1293">HTTP Method</span></span> | <span data-ttu-id="180f5-1294">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1294">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1295">A PUT</span><span class="sxs-lookup"><span data-stu-id="180f5-1295">PUT</span></span> |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1296">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1296">Example:</span></span><br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1297">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1297">Parameter Name</span></span> | <span data-ttu-id="180f5-1298">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1298">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1299">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1299">buildId</span></span> |<span data-ttu-id="180f5-1300">A build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1300">Unique identifier of the build.</span></span> |
| <span data-ttu-id="180f5-1301">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1301">apiVersion</span></span> |<span data-ttu-id="180f5-1302">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1302">1.0</span></span> |

<span data-ttu-id="180f5-1303">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1303">**Response:**</span></span>

<span data-ttu-id="180f5-1304">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1304">HTTP Status code: 200</span></span>

### <a name="118-get-build-parameters"></a><span data-ttu-id="180f5-1305">11.8.</span><span class="sxs-lookup"><span data-stu-id="180f5-1305">11.8.</span></span> <span data-ttu-id="180f5-1306">Build paraméterek lekérése</span><span class="sxs-lookup"><span data-stu-id="180f5-1306">Get Build Parameters</span></span>
<span data-ttu-id="180f5-1307">Lekéri a paraméterek felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1307">Retrieves build parameters.</span></span>

| <span data-ttu-id="180f5-1308">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1308">HTTP Method</span></span> | <span data-ttu-id="180f5-1309">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1309">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1310">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1310">GET</span></span> |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1311">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1311">Example:</span></span><br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1312">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1312">Parameter Name</span></span> | <span data-ttu-id="180f5-1313">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1313">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1314">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1314">buildId</span></span> |<span data-ttu-id="180f5-1315">A build egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1315">Unique identifier of the build.</span></span> |
| <span data-ttu-id="180f5-1316">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1316">apiVersion</span></span> |<span data-ttu-id="180f5-1317">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1317">1.0</span></span> |

<span data-ttu-id="180f5-1318">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1318">**Response:**</span></span>

<span data-ttu-id="180f5-1319">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1319">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1320">Ez az API a kulcs/érték elemgyűjtemény adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1320">This API returns a collection of key/value elements.</span></span> <span data-ttu-id="180f5-1321">Minden elem egy paraméter és az érték jelöli:</span><span class="sxs-lookup"><span data-stu-id="180f5-1321">Each element represents a parameter and its value:</span></span>

* <span data-ttu-id="180f5-1322">`feed/entry/content/properties/Key`-Build paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="180f5-1322">`feed/entry/content/properties/Key` - Build parameter name.</span></span>
* <span data-ttu-id="180f5-1323">`feed/entry/content/properties/Value`-Build paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1323">`feed/entry/content/properties/Value` - Build parameter value.</span></span>

<span data-ttu-id="180f5-1324">Az alábbi táblázat az egyes kulcsok értéket mutatja be.</span><span class="sxs-lookup"><span data-stu-id="180f5-1324">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="180f5-1325">Kulcs</span><span class="sxs-lookup"><span data-stu-id="180f5-1325">Key</span></span> | <span data-ttu-id="180f5-1326">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-1326">Description</span></span> | <span data-ttu-id="180f5-1327">Típus</span><span class="sxs-lookup"><span data-stu-id="180f5-1327">Type</span></span> | <span data-ttu-id="180f5-1328">Érvényes értéket</span><span class="sxs-lookup"><span data-stu-id="180f5-1328">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="180f5-1329">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="180f5-1329">NumberOfModelIterations</span></span> |<span data-ttu-id="180f5-1330">Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="180f5-1330">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="180f5-1331">Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="180f5-1331">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="180f5-1332">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1332">Integer</span></span> |<span data-ttu-id="180f5-1333">10-50</span><span class="sxs-lookup"><span data-stu-id="180f5-1333">10-50</span></span> |
| <span data-ttu-id="180f5-1334">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="180f5-1334">NumberOfModelDimensions</span></span> |<span data-ttu-id="180f5-1335">A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="180f5-1335">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="180f5-1336">Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra.</span><span class="sxs-lookup"><span data-stu-id="180f5-1336">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="180f5-1337">Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között.</span><span class="sxs-lookup"><span data-stu-id="180f5-1337">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="180f5-1338">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1338">Integer</span></span> |<span data-ttu-id="180f5-1339">10-40</span><span class="sxs-lookup"><span data-stu-id="180f5-1339">10-40</span></span> |
| <span data-ttu-id="180f5-1340">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1340">ItemCutOffLowerBound</span></span> |<span data-ttu-id="180f5-1341">Meghatározza a hűtő elem alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1341">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="180f5-1342">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1342">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1343">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1343">Integer</span></span> |<span data-ttu-id="180f5-1344">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1344">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1345">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1345">ItemCutOffUpperBound</span></span> |<span data-ttu-id="180f5-1346">Meghatározza az elem a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1346">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="180f5-1347">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1347">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1348">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1348">Integer</span></span> |<span data-ttu-id="180f5-1349">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1349">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1350">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1350">UserCutOffLowerBound</span></span> |<span data-ttu-id="180f5-1351">Meghatározza a hűtő felhasználói alsó korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1351">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="180f5-1352">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1352">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1353">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1353">Integer</span></span> |<span data-ttu-id="180f5-1354">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1354">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1355">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="180f5-1355">UserCutOffUpperBound</span></span> |<span data-ttu-id="180f5-1356">Határozza meg a felhasználó a hűtő felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1356">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="180f5-1357">Tekintse meg a fenti használati hűtő.</span><span class="sxs-lookup"><span data-stu-id="180f5-1357">See usage condenser above.</span></span> |<span data-ttu-id="180f5-1358">Egész szám</span><span class="sxs-lookup"><span data-stu-id="180f5-1358">Integer</span></span> |<span data-ttu-id="180f5-1359">2 vagy több (a 0 hűtő letiltása)</span><span class="sxs-lookup"><span data-stu-id="180f5-1359">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="180f5-1360">Leírás</span><span class="sxs-lookup"><span data-stu-id="180f5-1360">Description</span></span> |<span data-ttu-id="180f5-1361">Build leírása.</span><span class="sxs-lookup"><span data-stu-id="180f5-1361">Build description.</span></span> |<span data-ttu-id="180f5-1362">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1362">String</span></span> |<span data-ttu-id="180f5-1363">A szöveg, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1363">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="180f5-1364">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="180f5-1364">EnableModelingInsights</span></span> |<span data-ttu-id="180f5-1365">A javaslat modell metrikák számítási teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="180f5-1365">Allows you to compute metrics on the recommendation model.</span></span> |<span data-ttu-id="180f5-1366">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1366">Boolean</span></span> |<span data-ttu-id="180f5-1367">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1367">True/False</span></span> |
| <span data-ttu-id="180f5-1368">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="180f5-1368">UseFeaturesInModel</span></span> |<span data-ttu-id="180f5-1369">Azt jelzi, ha szolgáltatása használható a javaslat modell növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="180f5-1369">Indicates if features can be used in order to enhance the recommendation model.</span></span> |<span data-ttu-id="180f5-1370">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1370">Boolean</span></span> |<span data-ttu-id="180f5-1371">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1371">True/False</span></span> |
| <span data-ttu-id="180f5-1372">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="180f5-1372">ModelingFeatureList</span></span> |<span data-ttu-id="180f5-1373">A javaslat növelése érdekében a javaslat építés használandó neveinek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1373">Comma-separated list of feature names to be used in the recommendation build, in order to enhance the recommendation.</span></span> |<span data-ttu-id="180f5-1374">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1374">String</span></span> |<span data-ttu-id="180f5-1375">Funkcióneveket, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1375">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="180f5-1376">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="180f5-1376">AllowColdItemPlacement</span></span> |<span data-ttu-id="180f5-1377">Azt jelzi, ha a javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül.</span><span class="sxs-lookup"><span data-stu-id="180f5-1377">Indicates if the recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="180f5-1378">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1378">Boolean</span></span> |<span data-ttu-id="180f5-1379">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1379">True/False</span></span> |
| <span data-ttu-id="180f5-1380">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="180f5-1380">EnableFeatureCorrelation</span></span> |<span data-ttu-id="180f5-1381">Azt jelzi, ha szolgáltatások indoklást is használható.</span><span class="sxs-lookup"><span data-stu-id="180f5-1381">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="180f5-1382">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="180f5-1382">Boolean</span></span> |<span data-ttu-id="180f5-1383">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1383">True/False</span></span> |
| <span data-ttu-id="180f5-1384">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="180f5-1384">ReasoningFeatureList</span></span> |<span data-ttu-id="180f5-1385">Vesszővel tagolt listája mintafelismerési mondat (pl. javaslat magyarázatokat) használt.</span><span class="sxs-lookup"><span data-stu-id="180f5-1385">Comma-separated list of feature names to be used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="180f5-1386">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="180f5-1386">String</span></span> |<span data-ttu-id="180f5-1387">Funkcióneveket, legfeljebb 512 karakter</span><span class="sxs-lookup"><span data-stu-id="180f5-1387">Feature names, up to 512 chars</span></span> |

<span data-ttu-id="180f5-1388">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1388">OData XML</span></span>

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

## <a name="12-recommendation"></a><span data-ttu-id="180f5-1389">12. Ajánlás</span><span class="sxs-lookup"><span data-stu-id="180f5-1389">12. Recommendation</span></span>
### <a name="121-get-item-recommendations-for-active-build"></a><span data-ttu-id="180f5-1390">12.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-1390">12.1.</span></span> <span data-ttu-id="180f5-1391">Elem javaslatok beolvasása (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="180f5-1391">Get Item Recommendations (for active build)</span></span>
<span data-ttu-id="180f5-1392">A aktív build típusú javaslatok beszerzése "Ajánlás" vagy "Fbt" mag (bemeneti) elemek listája alapján.</span><span class="sxs-lookup"><span data-stu-id="180f5-1392">Get recommendations of the active build of type "Recommendation" or "Fbt" based on a list of seeds (input) items.</span></span>

| <span data-ttu-id="180f5-1393">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1393">HTTP Method</span></span> | <span data-ttu-id="180f5-1394">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1394">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1395">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1395">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1396">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1396">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1397">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1397">Parameter Name</span></span> | <span data-ttu-id="180f5-1398">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1398">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1399">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1399">modelId</span></span> |<span data-ttu-id="180f5-1400">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1400">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1401">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="180f5-1401">itemIds</span></span> |<span data-ttu-id="180f5-1402">Az elemek javasolt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1402">Comma-separated list of the items to recommend for.</span></span> <br><span data-ttu-id="180f5-1403">Ha az aktív build típusú FBT majd küldhet csak egy elemet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1403">If the active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="180f5-1404">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1404">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1405">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1405">numberOfResults</span></span> |<span data-ttu-id="180f5-1406">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1406">Number of required results</span></span> <br> <span data-ttu-id="180f5-1407">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="180f5-1407">Max: 150</span></span> |
| <span data-ttu-id="180f5-1408">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1408">includeMetatadata</span></span> |<span data-ttu-id="180f5-1409">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1409">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1410">apiVersion</span></span> |<span data-ttu-id="180f5-1411">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1411">1.0</span></span> |

<span data-ttu-id="180f5-1412">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1412">**Response:**</span></span>

<span data-ttu-id="180f5-1413">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1413">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1414">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1414">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1415">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1415">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1416">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1416">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1417">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1417">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1418">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1418">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1419">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1419">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1420">Az alábbi példa egy válasz 10 ajánlott elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="180f5-1420">The example response below includes 10 recommended items.</span></span>

<span data-ttu-id="180f5-1421">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1421">OData XML</span></span>

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

### <a name="122-get-item-recommendations-of-a-specific-build"></a><span data-ttu-id="180f5-1422">12.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-1422">12.2.</span></span> <span data-ttu-id="180f5-1423">(Az adott build) elem javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="180f5-1423">Get Item Recommendations (of a specific build)</span></span>
<span data-ttu-id="180f5-1424">Egy adott build "Ajánlás" vagy "Fbt" típusú javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="180f5-1424">Get recommendations of a specific build of type "Recommendation" or "Fbt".</span></span>

| <span data-ttu-id="180f5-1425">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1425">HTTP Method</span></span> | <span data-ttu-id="180f5-1426">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1426">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1427">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1427">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1428">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1428">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1429">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1429">Parameter Name</span></span> | <span data-ttu-id="180f5-1430">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1430">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1431">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1431">modelId</span></span> |<span data-ttu-id="180f5-1432">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1432">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1433">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="180f5-1433">itemIds</span></span> |<span data-ttu-id="180f5-1434">Az elemek javasolt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1434">Comma-separated list of the items to recommend for.</span></span> <br><span data-ttu-id="180f5-1435">Ha az aktív build típusú FBT majd küldhet csak egy elemet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1435">If the active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="180f5-1436">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1436">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1437">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1437">numberOfResults</span></span> |<span data-ttu-id="180f5-1438">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1438">Number of required results</span></span> <br> <span data-ttu-id="180f5-1439">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="180f5-1439">Max: 150</span></span> |
| <span data-ttu-id="180f5-1440">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1440">includeMetatadata</span></span> |<span data-ttu-id="180f5-1441">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1441">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1442">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1442">buildId</span></span> |<span data-ttu-id="180f5-1443">a build azonosítót használja a javaslat kérelem</span><span class="sxs-lookup"><span data-stu-id="180f5-1443">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="180f5-1444">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1444">apiVersion</span></span> |<span data-ttu-id="180f5-1445">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1445">1.0</span></span> |

<span data-ttu-id="180f5-1446">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1446">**Response:**</span></span>

<span data-ttu-id="180f5-1447">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1447">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1448">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1448">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1449">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1449">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1450">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1450">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1451">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1451">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1452">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1452">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1453">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1453">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1454">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="180f5-1454">See a response example in 12.1</span></span>

### <a name="123-get-fbt-recommendations-for-active-build"></a><span data-ttu-id="180f5-1455">12.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-1455">12.3.</span></span> <span data-ttu-id="180f5-1456">(Az aktív build) FBT javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="180f5-1456">Get FBT Recommendations (for active build)</span></span>
<span data-ttu-id="180f5-1457">Tekintse át a javasolt a aktív build típusú, "Fbt" alapján a kezdőérték (bemeneti) elemet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1457">Get recommendations of the active build of type "Fbt" based on a seed (input) item.</span></span>

| <span data-ttu-id="180f5-1458">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1458">HTTP Method</span></span> | <span data-ttu-id="180f5-1459">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1459">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1460">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1460">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1461">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1461">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1462">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1462">Parameter Name</span></span> | <span data-ttu-id="180f5-1463">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1463">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1464">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1464">modelId</span></span> |<span data-ttu-id="180f5-1465">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1465">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1466">itemId</span><span class="sxs-lookup"><span data-stu-id="180f5-1466">itemId</span></span> |<span data-ttu-id="180f5-1467">Ajánlott elemet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1467">Item to recommend for.</span></span> <br><span data-ttu-id="180f5-1468">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1468">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1469">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1469">numberOfResults</span></span> |<span data-ttu-id="180f5-1470">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1470">Number of required results</span></span> <br><span data-ttu-id="180f5-1471">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="180f5-1471">Max: 150</span></span> |
| <span data-ttu-id="180f5-1472">minimalScore</span><span class="sxs-lookup"><span data-stu-id="180f5-1472">minimalScore</span></span> |<span data-ttu-id="180f5-1473">Minimális pontszám, amelyben egy gyakori készlet kell ahhoz, hogy a keresés eredményeit</span><span class="sxs-lookup"><span data-stu-id="180f5-1473">Minimal score that a frequent set should have in order to be included in the returned results</span></span> |
| <span data-ttu-id="180f5-1474">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1474">includeMetatadata</span></span> |<span data-ttu-id="180f5-1475">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1475">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1476">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1476">apiVersion</span></span> |<span data-ttu-id="180f5-1477">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1477">1.0</span></span> |

<span data-ttu-id="180f5-1478">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1478">**Response:**</span></span>

<span data-ttu-id="180f5-1479">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1479">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1480">A válasz egy tételt ajánlott elem beállítása (olyan elemek, amelyek általában a kezdőérték/bemeneti elemszintű együtt vannak vásárolt készlete) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1480">The response includes one entry per recommended item set (a set of items which are usually bought together with the seed/input item).</span></span> <span data-ttu-id="180f5-1481">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1481">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1482">`Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1482">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1483">`Feed\entry\content\properties\Name1`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1483">`Feed\entry\content\properties\Name1` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1484">`Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).</span><span class="sxs-lookup"><span data-stu-id="180f5-1484">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="180f5-1485">`Feed\entry\content\properties\Name2`-A 2. (nem kötelező) elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1485">`Feed\entry\content\properties\Name2` - Name of the 2nd item (optional).</span></span>
* <span data-ttu-id="180f5-1486">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1486">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1487">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1487">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1488">Az alábbi példa egy válasz 3 ajánlott elem beállítása magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="180f5-1488">The example response below includes 3 recommended item sets.</span></span>

<span data-ttu-id="180f5-1489">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1489">OData XML</span></span>

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

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a><span data-ttu-id="180f5-1490">12.4.</span><span class="sxs-lookup"><span data-stu-id="180f5-1490">12.4.</span></span> <span data-ttu-id="180f5-1491">(Az adott build) FBT javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="180f5-1491">Get FBT Recommendations (of a specific build)</span></span>
<span data-ttu-id="180f5-1492">Egy adott build "Fbt" típusú javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="180f5-1492">Get recommendations of a specific build of type "Fbt".</span></span>

| <span data-ttu-id="180f5-1493">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1493">HTTP Method</span></span> | <span data-ttu-id="180f5-1494">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1495">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1495">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1496">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1496">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1497">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1497">Parameter Name</span></span> | <span data-ttu-id="180f5-1498">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1499">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1499">modelId</span></span> |<span data-ttu-id="180f5-1500">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1500">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1501">itemId</span><span class="sxs-lookup"><span data-stu-id="180f5-1501">itemId</span></span> |<span data-ttu-id="180f5-1502">Ajánlott elemet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1502">Item to recommend for.</span></span> <br><span data-ttu-id="180f5-1503">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1503">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1504">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1504">numberOfResults</span></span> |<span data-ttu-id="180f5-1505">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1505">Number of required results</span></span> <br><span data-ttu-id="180f5-1506">Maximális: 150</span><span class="sxs-lookup"><span data-stu-id="180f5-1506">Max: 150</span></span> |
| <span data-ttu-id="180f5-1507">minimalScore</span><span class="sxs-lookup"><span data-stu-id="180f5-1507">minimalScore</span></span> |<span data-ttu-id="180f5-1508">Minimális pontszám, amelyben egy gyakori készlet kell ahhoz, hogy a keresés eredményeit</span><span class="sxs-lookup"><span data-stu-id="180f5-1508">Minimal score that a frequent set should have in order to be included in the returned results</span></span> |
| <span data-ttu-id="180f5-1509">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1509">includeMetatadata</span></span> |<span data-ttu-id="180f5-1510">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1510">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1511">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1511">buildId</span></span> |<span data-ttu-id="180f5-1512">a build azonosítót használja a javaslat kérelem</span><span class="sxs-lookup"><span data-stu-id="180f5-1512">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="180f5-1513">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1513">apiVersion</span></span> |<span data-ttu-id="180f5-1514">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1514">1.0</span></span> |

<span data-ttu-id="180f5-1515">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1515">**Response:**</span></span>

<span data-ttu-id="180f5-1516">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1516">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1517">A válasz egy tételt ajánlott elem beállítása (olyan elemek, amelyek általában a kezdőérték/bemeneti elemszintű együtt vannak vásárolt készlete) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1517">The response includes one entry per recommended item set (a set of items which are usually bought together with the seed/input item).</span></span> <span data-ttu-id="180f5-1518">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1518">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1519">`Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1519">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1520">`Feed\entry\content\properties\Name1`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1520">`Feed\entry\content\properties\Name1` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1521">`Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).</span><span class="sxs-lookup"><span data-stu-id="180f5-1521">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="180f5-1522">`Feed\entry\content\properties\Name2`-A 2. (nem kötelező) elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1522">`Feed\entry\content\properties\Name2` - Name of the 2nd item (optional).</span></span>
* <span data-ttu-id="180f5-1523">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1523">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1524">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1524">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1525">A válasz példa a 12.3</span><span class="sxs-lookup"><span data-stu-id="180f5-1525">See a response example in 12.3</span></span>

### <a name="125-get-user-recommendations-for-active-build"></a><span data-ttu-id="180f5-1526">12.5.</span><span class="sxs-lookup"><span data-stu-id="180f5-1526">12.5.</span></span> <span data-ttu-id="180f5-1527">Felhasználói javaslatok beolvasása (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="180f5-1527">Get User Recommendations (for active build)</span></span>
<span data-ttu-id="180f5-1528">A build "Ajánlás" jelölésű aktív build típusú felhasználói javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="180f5-1528">Get user recommendations of a build of type "Recommendation" marked as active build.</span></span>

<span data-ttu-id="180f5-1529">Az API-t a felhasználó a használati előzményei alapján előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1529">The API will return a list of predicted item according to the usage history of the user.</span></span>

<span data-ttu-id="180f5-1530">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="180f5-1530">Notes:</span></span> 

1. <span data-ttu-id="180f5-1531">Nincs felhasználói javaslat FBT buildjéhez van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1531">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="180f5-1532">Ha az aktív build ezt a módszert fog FBT hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1532">If the active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="180f5-1533">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1533">HTTP Method</span></span> | <span data-ttu-id="180f5-1534">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1534">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1535">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1535">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1536">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1536">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1537">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1537">Parameter Name</span></span> | <span data-ttu-id="180f5-1538">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1538">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1539">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1539">modelId</span></span> |<span data-ttu-id="180f5-1540">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1540">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1541">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="180f5-1541">userId</span></span> |<span data-ttu-id="180f5-1542">A felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1542">Unique identifier of the user</span></span> |
| <span data-ttu-id="180f5-1543">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1543">numberOfResults</span></span> |<span data-ttu-id="180f5-1544">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1544">Number of required results</span></span> |
| <span data-ttu-id="180f5-1545">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1545">includeMetatadata</span></span> |<span data-ttu-id="180f5-1546">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1546">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1547">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1547">apiVersion</span></span> |<span data-ttu-id="180f5-1548">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1548">1.0</span></span> |

<span data-ttu-id="180f5-1549">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1549">**Response:**</span></span>

<span data-ttu-id="180f5-1550">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1550">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1551">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1551">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1552">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1552">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1553">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1553">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1554">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1554">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1555">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1555">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1556">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1556">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1557">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="180f5-1557">See a response example in 12.1</span></span>

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a><span data-ttu-id="180f5-1558">12.6.</span><span class="sxs-lookup"><span data-stu-id="180f5-1558">12.6.</span></span> <span data-ttu-id="180f5-1559">Felhasználói javaslatok lekérdezni elemek listáját (az aktív build)</span><span class="sxs-lookup"><span data-stu-id="180f5-1559">Get User Recommendations with item list (for active build)</span></span>
<span data-ttu-id="180f5-1560">A build "Ajánlás" jelölésű elemek bejegyzés további listája az aktív build típusú felhasználói javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="180f5-1560">Get user recommendations of a build of type "Recommendation" marked as active build with an additional list of items</span></span>

<span data-ttu-id="180f5-1561">Az API-t a felhasználó a használati előzményei alapján előre jelzett elem és a további megadott elemek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1561">The API will return a list of predicted item according to the usage history of the user and the additional provided items.</span></span>

<span data-ttu-id="180f5-1562">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="180f5-1562">Notes:</span></span> 

1. <span data-ttu-id="180f5-1563">Nincs felhasználói javaslat FBT buildjéhez van.</span><span class="sxs-lookup"><span data-stu-id="180f5-1563">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="180f5-1564">Ha az aktív build ezt a módszert fog FBT hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1564">If the active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="180f5-1565">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1565">HTTP Method</span></span> | <span data-ttu-id="180f5-1566">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1566">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1567">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1567">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1568">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1568">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1569">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1569">Parameter Name</span></span> | <span data-ttu-id="180f5-1570">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1570">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1571">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1571">modelId</span></span> |<span data-ttu-id="180f5-1572">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1572">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1573">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="180f5-1573">userId</span></span> |<span data-ttu-id="180f5-1574">A felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1574">Unique identifier of the user</span></span> |
| <span data-ttu-id="180f5-1575">itemsIds</span><span class="sxs-lookup"><span data-stu-id="180f5-1575">itemsIds</span></span> |<span data-ttu-id="180f5-1576">Az elemek javasolt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1576">Comma-separated list of the items to recommend for.</span></span> <span data-ttu-id="180f5-1577">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1577">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1578">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1578">numberOfResults</span></span> |<span data-ttu-id="180f5-1579">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1579">Number of required results</span></span> |
| <span data-ttu-id="180f5-1580">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1580">includeMetatadata</span></span> |<span data-ttu-id="180f5-1581">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1581">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1582">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1582">apiVersion</span></span> |<span data-ttu-id="180f5-1583">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1583">1.0</span></span> |

<span data-ttu-id="180f5-1584">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1584">**Response:**</span></span>

<span data-ttu-id="180f5-1585">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1585">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1586">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1586">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1587">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1587">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1588">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1588">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1589">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1589">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1590">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1590">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1591">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1591">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1592">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="180f5-1592">See a response example in 12.1</span></span>

### <a name="127-get-user-recommendations--of-a-specific-build"></a><span data-ttu-id="180f5-1593">12.7.</span><span class="sxs-lookup"><span data-stu-id="180f5-1593">12.7.</span></span> <span data-ttu-id="180f5-1594">(Az adott build) felhasználói javaslatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="180f5-1594">Get User Recommendations  (of a specific build)</span></span>
<span data-ttu-id="180f5-1595">Egy adott build "Ajánlás" típusú felhasználói javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="180f5-1595">Get user recommendations of a specific build of type "Recommendation".</span></span>

<span data-ttu-id="180f5-1596">Az API-t (az adott build szerepel) felhasználó a használati előzményei alapján előre jelzett elem listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1596">The API will return a list of predicted item according to the usage history of the user (used in the specific build).</span></span>

<span data-ttu-id="180f5-1597">Megjegyzés: Nincs FBT buildjéhez nincs felhasználói javaslat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1597">Note: There is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="180f5-1598">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1598">HTTP Method</span></span> | <span data-ttu-id="180f5-1599">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1599">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1600">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1600">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1601">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1601">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1602">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1602">Parameter Name</span></span> | <span data-ttu-id="180f5-1603">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1603">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1604">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1604">modelId</span></span> |<span data-ttu-id="180f5-1605">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1605">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1606">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="180f5-1606">userId</span></span> |<span data-ttu-id="180f5-1607">A felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1607">Unique identifier of the user</span></span> |
| <span data-ttu-id="180f5-1608">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1608">numberOfResults</span></span> |<span data-ttu-id="180f5-1609">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1609">Number of required results</span></span> |
| <span data-ttu-id="180f5-1610">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1610">includeMetatadata</span></span> |<span data-ttu-id="180f5-1611">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1611">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1612">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1612">buildId</span></span> |<span data-ttu-id="180f5-1613">a build azonosítót használja a javaslat kérelem</span><span class="sxs-lookup"><span data-stu-id="180f5-1613">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="180f5-1614">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1614">apiVersion</span></span> |<span data-ttu-id="180f5-1615">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1615">1.0</span></span> |

<span data-ttu-id="180f5-1616">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1616">**Response:**</span></span>

<span data-ttu-id="180f5-1617">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1617">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1618">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1618">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1619">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1619">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1620">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1620">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1621">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1621">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1622">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1622">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1623">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1623">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1624">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="180f5-1624">See a response example in 12.1</span></span>

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a><span data-ttu-id="180f5-1625">12.8.</span><span class="sxs-lookup"><span data-stu-id="180f5-1625">12.8.</span></span> <span data-ttu-id="180f5-1626">Felhasználói javaslatok beszerzése elem listáját (adott build)</span><span class="sxs-lookup"><span data-stu-id="180f5-1626">Get User Recommendations with item list (of a specific build)</span></span>
<span data-ttu-id="180f5-1627">Egy adott build "Ajánlás" típusú felhasználói javaslatok és további elemek listájának kapják meg.</span><span class="sxs-lookup"><span data-stu-id="180f5-1627">Get user recommendations of a specific build of type "Recommendation" and the list of additional items.</span></span>

<span data-ttu-id="180f5-1628">Az API-t visszaállítja az előre jelzett elem a használati előzményei alapján a felhasználó listáját, valamint a további elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1628">The API will return a list of predicted item according to the usage history of the user and the additional list of items.</span></span>

<span data-ttu-id="180f5-1629">Megjegyzés: A Tthere nincs felhasználói javaslat FBT buildjéhez.</span><span class="sxs-lookup"><span data-stu-id="180f5-1629">Note: Tthere is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="180f5-1630">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1630">HTTP Method</span></span> | <span data-ttu-id="180f5-1631">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1631">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1632">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1632">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1633">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1633">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1634">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1634">Parameter Name</span></span> | <span data-ttu-id="180f5-1635">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1635">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1636">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1636">modelId</span></span> |<span data-ttu-id="180f5-1637">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1637">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1638">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="180f5-1638">userId</span></span> |<span data-ttu-id="180f5-1639">A felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1639">Unique identifier of the user</span></span> |
| <span data-ttu-id="180f5-1640">hogy az elemazonosítók</span><span class="sxs-lookup"><span data-stu-id="180f5-1640">itemIds</span></span> |<span data-ttu-id="180f5-1641">Az elemek javasolt vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="180f5-1641">Comma-separated list of the items to recommend for.</span></span> <span data-ttu-id="180f5-1642">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="180f5-1642">Max length: 1024</span></span> |
| <span data-ttu-id="180f5-1643">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="180f5-1643">numberOfResults</span></span> |<span data-ttu-id="180f5-1644">Szükséges eredmények száma</span><span class="sxs-lookup"><span data-stu-id="180f5-1644">Number of required results</span></span> |
| <span data-ttu-id="180f5-1645">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="180f5-1645">includeMetatadata</span></span> |<span data-ttu-id="180f5-1646">Jövőbeli használatra, mindig hamis</span><span class="sxs-lookup"><span data-stu-id="180f5-1646">Future use, always false</span></span> |
| <span data-ttu-id="180f5-1647">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1647">buildId</span></span> |<span data-ttu-id="180f5-1648">a build azonosítót használja a javaslat kérelem</span><span class="sxs-lookup"><span data-stu-id="180f5-1648">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="180f5-1649">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1649">apiVersion</span></span> |<span data-ttu-id="180f5-1650">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1650">1.0</span></span> |

<span data-ttu-id="180f5-1651">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1651">**Response:**</span></span>

<span data-ttu-id="180f5-1652">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1652">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1653">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1653">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1654">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1654">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1655">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1655">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1656">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1656">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1657">`Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="180f5-1657">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="180f5-1658">`Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).</span><span class="sxs-lookup"><span data-stu-id="180f5-1658">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="180f5-1659">A válasz példa a 12.1</span><span class="sxs-lookup"><span data-stu-id="180f5-1659">See a response example in 12.1</span></span>

## <a name="13-user-usage-history"></a><span data-ttu-id="180f5-1660">13. A felhasználó használati előzményei</span><span class="sxs-lookup"><span data-stu-id="180f5-1660">13. User Usage History</span></span>
<span data-ttu-id="180f5-1661">Miután egy javaslat modell lett létrehozva a rendszer engedélyezi a felhasználók előzményei (egy adott felhasználóhoz tartozó elemeket) beolvasása a build használatos.</span><span class="sxs-lookup"><span data-stu-id="180f5-1661">Once a recommendation model was built the system will allow to retrieve the user history (items associated to a specific user) used for the build.</span></span>
<span data-ttu-id="180f5-1662">Ez az API engedélyezése a felhasználók előzményei beolvasása</span><span class="sxs-lookup"><span data-stu-id="180f5-1662">This API allow to retrieve the user history</span></span>

<span data-ttu-id="180f5-1663">Megjegyzés: a felhasználók előzményei érhető el jelenleg csak a javaslat buildek.</span><span class="sxs-lookup"><span data-stu-id="180f5-1663">Note: the user history is currently available only for recommendation builds.</span></span>

### <a name="131-retrieve-user-history"></a><span data-ttu-id="180f5-1664">13.1 beolvasása felhasználók előzményei</span><span class="sxs-lookup"><span data-stu-id="180f5-1664">13.1 Retrieve user history</span></span>
<span data-ttu-id="180f5-1665">A megadott felhasználói azonosító az aktív build vagy a megadott build használt elem listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="180f5-1665">Retrieve the list of item used in the active build or in the specified build for the given user id.</span></span>

| <span data-ttu-id="180f5-1666">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1666">HTTP Method</span></span> | <span data-ttu-id="180f5-1667">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1667">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1668">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1668">GET</span></span> |<span data-ttu-id="180f5-1669">A felhasználók előzményei beolvasása az active build.</span><span class="sxs-lookup"><span data-stu-id="180f5-1669">Get the user history for the active build.</span></span><br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/><span data-ttu-id="180f5-1670">A felhasználó előzményeinek lekérése a megadott build`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span><span class="sxs-lookup"><span data-stu-id="180f5-1670">Get the user history for the given build `<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span></span><br/><br/><span data-ttu-id="180f5-1671">Példa:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span><span class="sxs-lookup"><span data-stu-id="180f5-1671">Example:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span></span> |

| <span data-ttu-id="180f5-1672">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1672">Parameter Name</span></span> | <span data-ttu-id="180f5-1673">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1673">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1674">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1674">modelId</span></span> |<span data-ttu-id="180f5-1675">a modell egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1675">the unique identifier of the model.</span></span> |
| <span data-ttu-id="180f5-1676">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="180f5-1676">userId</span></span> |<span data-ttu-id="180f5-1677">a felhasználó egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="180f5-1677">the unique identifier of the user.</span></span> |
| <span data-ttu-id="180f5-1678">buildId</span><span class="sxs-lookup"><span data-stu-id="180f5-1678">buildId</span></span> |<span data-ttu-id="180f5-1679">nem kötelező paraméter, annak jelzésére, hogy mely build a a felhasználók előzményei kell fetch engedélyezése</span><span class="sxs-lookup"><span data-stu-id="180f5-1679">optional parameter, allow to indicate from which build the user history should be fetch</span></span> |
| <span data-ttu-id="180f5-1680">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1680">apiVersion</span></span> |<span data-ttu-id="180f5-1681">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1681">1.0</span></span> |

<span data-ttu-id="180f5-1682">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1682">**Response:**</span></span>

<span data-ttu-id="180f5-1683">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1683">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1684">A válasz egy tételt ajánlott cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="180f5-1684">The response includes one entry per recommended item.</span></span> <span data-ttu-id="180f5-1685">Mindegyik bejegyzés rendelkezik a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="180f5-1685">Each entry has the following data:</span></span>

* <span data-ttu-id="180f5-1686">`Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1686">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="180f5-1687">`Feed\entry\content\properties\Name`-Az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="180f5-1687">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="180f5-1688">`Feed\entry\content\properties\Rating`– NEM ÁLL RENDELKEZÉSRE.</span><span class="sxs-lookup"><span data-stu-id="180f5-1688">`Feed\entry\content\properties\Rating` - N/A.</span></span>
* <span data-ttu-id="180f5-1689">`Feed\entry\content\properties\Reasoning`– NEM ÁLL RENDELKEZÉSRE.</span><span class="sxs-lookup"><span data-stu-id="180f5-1689">`Feed\entry\content\properties\Reasoning` - N/A.</span></span>

<span data-ttu-id="180f5-1690">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1690">OData XML</span></span>

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

## <a name="14-notifications"></a><span data-ttu-id="180f5-1691">14. Értesítések</span><span class="sxs-lookup"><span data-stu-id="180f5-1691">14. Notifications</span></span>
<span data-ttu-id="180f5-1692">Az Azure Machine Learning javaslatok létrehozza értesítések, amikor állandó hiba fordulhat elő, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="180f5-1692">Azure Machine Learning Recommendations creates notifications when persistent errors happen in the system.</span></span> <span data-ttu-id="180f5-1693">Az értesítések 3 típusa van:</span><span class="sxs-lookup"><span data-stu-id="180f5-1693">There are 3 types of notifications:</span></span>

1. <span data-ttu-id="180f5-1694">Build hiba – ezt az értesítést akkor váltódik ki, minden összeállítási hiba.</span><span class="sxs-lookup"><span data-stu-id="180f5-1694">Build failure - This notification is triggered for every build failure.</span></span>
2. <span data-ttu-id="180f5-1695">Hiba – az értesítés feldolgozása adatgyűjtést lesz kiváltva, ha több mint 100 hiba van a használati események száma modell feldolgozása az elmúlt 5 percben.</span><span class="sxs-lookup"><span data-stu-id="180f5-1695">Data acquisition processing failure - This notification is triggered when we have more than 100 errors in the last 5 minutes in the processing of usage events per model.</span></span>
3. <span data-ttu-id="180f5-1696">Javaslat fogyasztás hiba – ezt az értesítést lesz kiváltva, ha van 100-nál több hibák az elmúlt 5 percben az ajánlás kérelmek / modell feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="180f5-1696">Recommendation consumption failure - This notification is triggered when we have more than 100 errors in the last 5 minutes in the processing of recommendation requests per model.</span></span>

### <a name="141-get-notifications"></a><span data-ttu-id="180f5-1697">14.1.</span><span class="sxs-lookup"><span data-stu-id="180f5-1697">14.1.</span></span> <span data-ttu-id="180f5-1698">Értesítéseket</span><span class="sxs-lookup"><span data-stu-id="180f5-1698">Get Notifications</span></span>
<span data-ttu-id="180f5-1699">Lekéri az összes vagy egy egyetlen modell minden értesítést.</span><span class="sxs-lookup"><span data-stu-id="180f5-1699">Retrieves all the notifications for all models or for a single model.</span></span>

| <span data-ttu-id="180f5-1700">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1700">HTTP Method</span></span> | <span data-ttu-id="180f5-1701">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1701">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1702">GET</span><span class="sxs-lookup"><span data-stu-id="180f5-1702">GET</span></span> |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1703">Első összes minden értesítés:</span><span class="sxs-lookup"><span data-stu-id="180f5-1703">Getting all notifications for all models:</span></span><br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1704">Példa egy adott modell értesítések kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="180f5-1704">Example for getting notifications for a specific model:</span></span><br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| <span data-ttu-id="180f5-1705">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1705">Parameter Name</span></span> | <span data-ttu-id="180f5-1706">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1706">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1707">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1707">modelId</span></span> |<span data-ttu-id="180f5-1708">Nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="180f5-1708">Optional parameter.</span></span> <span data-ttu-id="180f5-1709">Ha nincs megadva, az összes modell összes értesítéseket fog kapni.</span><span class="sxs-lookup"><span data-stu-id="180f5-1709">When omitted, you will get all notifications for all models.</span></span> <br><span data-ttu-id="180f5-1710">Érvénytelen érték: a modell egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="180f5-1710">Valid value: unique identifier of the model.</span></span> |
| <span data-ttu-id="180f5-1711">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1711">apiVersion</span></span> |<span data-ttu-id="180f5-1712">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1712">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-1713">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-1713">Request Body</span></span> |<span data-ttu-id="180f5-1714">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-1714">NONE</span></span> |

<span data-ttu-id="180f5-1715">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="180f5-1715">**Response:**</span></span>

<span data-ttu-id="180f5-1716">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1716">HTTP Status code: 200</span></span>

<span data-ttu-id="180f5-1717">Az OData-XML</span><span class="sxs-lookup"><span data-stu-id="180f5-1717">OData XML</span></span>

    The response includes one entry per notification. Each entry has the following data:
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

### <a name="142-delete-model-notifications"></a><span data-ttu-id="180f5-1718">14.2.</span><span class="sxs-lookup"><span data-stu-id="180f5-1718">14.2.</span></span> <span data-ttu-id="180f5-1719">Modell értesítések törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-1719">Delete Model Notifications</span></span>
<span data-ttu-id="180f5-1720">Törli az összes olvasási értesítések levő modell esetén.</span><span class="sxs-lookup"><span data-stu-id="180f5-1720">Deletes all read notifications for a model.</span></span>

| <span data-ttu-id="180f5-1721">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1721">HTTP Method</span></span> | <span data-ttu-id="180f5-1722">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1722">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1723">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-1723">DELETE</span></span> |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="180f5-1724">Példa:</span><span class="sxs-lookup"><span data-stu-id="180f5-1724">Example:</span></span><br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1725">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1725">Parameter Name</span></span> | <span data-ttu-id="180f5-1726">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1726">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1727">modelId</span><span class="sxs-lookup"><span data-stu-id="180f5-1727">modelId</span></span> |<span data-ttu-id="180f5-1728">A modell egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="180f5-1728">Unique identifier of the model</span></span> |
| <span data-ttu-id="180f5-1729">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1729">apiVersion</span></span> |<span data-ttu-id="180f5-1730">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1730">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-1731">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-1731">Request Body</span></span> |<span data-ttu-id="180f5-1732">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-1732">NONE</span></span> |

<span data-ttu-id="180f5-1733">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1733">**Response**:</span></span>

<span data-ttu-id="180f5-1734">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1734">HTTP Status code: 200</span></span>

### <a name="143-delete-user-notifications"></a><span data-ttu-id="180f5-1735">14.3.</span><span class="sxs-lookup"><span data-stu-id="180f5-1735">14.3.</span></span> <span data-ttu-id="180f5-1736">Felhasználó értesítései törlése</span><span class="sxs-lookup"><span data-stu-id="180f5-1736">Delete User Notifications</span></span>
<span data-ttu-id="180f5-1737">Törli az összes minden értesítést.</span><span class="sxs-lookup"><span data-stu-id="180f5-1737">Deletes all notifications for all models.</span></span>

| <span data-ttu-id="180f5-1738">HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="180f5-1738">HTTP Method</span></span> | <span data-ttu-id="180f5-1739">URI</span><span class="sxs-lookup"><span data-stu-id="180f5-1739">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1740">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="180f5-1740">DELETE</span></span> |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| <span data-ttu-id="180f5-1741">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="180f5-1741">Parameter Name</span></span> | <span data-ttu-id="180f5-1742">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="180f5-1742">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="180f5-1743">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180f5-1743">apiVersion</span></span> |<span data-ttu-id="180f5-1744">1.0</span><span class="sxs-lookup"><span data-stu-id="180f5-1744">1.0</span></span> |
|  | |
| <span data-ttu-id="180f5-1745">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="180f5-1745">Request Body</span></span> |<span data-ttu-id="180f5-1746">EGYIK SEM</span><span class="sxs-lookup"><span data-stu-id="180f5-1746">NONE</span></span> |

<span data-ttu-id="180f5-1747">**Válasz**:</span><span class="sxs-lookup"><span data-stu-id="180f5-1747">**Response**:</span></span>

<span data-ttu-id="180f5-1748">HTTP-állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="180f5-1748">HTTP Status code: 200</span></span>

## <a name="15-legal"></a><span data-ttu-id="180f5-1749">15. Jogi tudnivalók</span><span class="sxs-lookup"><span data-stu-id="180f5-1749">15. Legal</span></span>
<span data-ttu-id="180f5-1750">Ez a dokumentum biztosított ",-van".</span><span class="sxs-lookup"><span data-stu-id="180f5-1750">This document is provided “as-is”.</span></span> <span data-ttu-id="180f5-1751">Információk és nézetek ebben a dokumentumban, beleértve az URL-cím és az egyéb webhelyhivatkozásokat, értesítés nélkül változhatnak.</span><span class="sxs-lookup"><span data-stu-id="180f5-1751">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span><br><br>
<span data-ttu-id="180f5-1752">A felhasznált példák némelyike csak illusztrációs célokat szolgálnak, és kitalált esetet szemléltet.</span><span class="sxs-lookup"><span data-stu-id="180f5-1752">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="180f5-1753">Nincs valós association vagy a kapcsolat célja, vagy eseményekkel.</span><span class="sxs-lookup"><span data-stu-id="180f5-1753">No real association or connection is intended or should be inferred.</span></span><br><br>
<span data-ttu-id="180f5-1754">Jelen dokumentum nem biztosít semmilyen jogot semmilyen Microsoft-termékben található szellemi tulajdonhoz.</span><span class="sxs-lookup"><span data-stu-id="180f5-1754">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="180f5-1755">Előfordulhat, hogy másolja és használja a dokumentum belső használatra, tájékoztatási céllal.</span><span class="sxs-lookup"><span data-stu-id="180f5-1755">You may copy and use this document for your internal, reference purposes.</span></span><br><br>
<span data-ttu-id="180f5-1756">© 2015 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="180f5-1756">© 2015 Microsoft.</span></span> <span data-ttu-id="180f5-1757">Minden jog fenntartva.</span><span class="sxs-lookup"><span data-stu-id="180f5-1757">All rights reserved.</span></span>

