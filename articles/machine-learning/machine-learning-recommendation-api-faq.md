---
title: "Beállítása és használata a Machine Learning javaslatok API |} Microsoft Docs"
description: "Az Azure Machine Learning GYIK a beépített Microsoft javaslatok API"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="bfe8f-103">A Machine Learning Recommendations API beállítása és használata – GYIK</span><span class="sxs-lookup"><span data-stu-id="bfe8f-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="bfe8f-104">**Mi az az ajánlások?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="bfe8f-105">El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-105">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="bfe8f-106">A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-106">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="bfe8f-107">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="bfe8f-108">További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="bfe8f-108">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="bfe8f-109">A szervezetek és vállalatok számára, a kereszt-értékesít javaslatok és felfelé-értékesít termékek és az ügyfeleknek szolgáltatásokat használó javaslatok az Azure Machine Learning egy önkiszolgáló javaslatok motor biztosít.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-109">For organizations and businesses that rely on recommendations to cross-sell and up-sell products and services to their customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="bfe8f-110">A core algoritmus mátrix factorization használó együttműködési szűrés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="bfe8f-111">JAVASLATOK férhetnek hozzá alkalmazásfejlesztők REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="bfe8f-112">**Mi a teendő a javaslatok?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="bfe8f-113">JAVASLATOK vesz igénybe, adjon meg egy elem vagy egy elemet, és a vonatkozó javaslatok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="bfe8f-114">Például: az ügyfél egy online közvetítő kattint egy termék.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="bfe8f-115">Az online közvetítő küld ennek a terméknek javaslatok bemenetként, ismét lekérdezi a termékek listáját, és úgy dönt, amelyhez ezeket a termékeket nem jelenik meg az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-115">The online retailer sends that product as input to RECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown to the customer.</span></span> <span data-ttu-id="bfe8f-116">Előfordulhat, hogy használandó javaslatok optimalizálja az online áruházból vagy akár a vállalaton belüli tájékoztatja értékesítési részleg vagy hívás center.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-116">You may want to use RECOMMENDATIONS to optimize your online store or even to inform your inside sales department or call center.</span></span>

<span data-ttu-id="bfe8f-117">**Vannak-e használati korlátozások?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="bfe8f-118">Javaslatok rendelkezik a következő használati korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="bfe8f-118">Recommendations has the following usage limitations:</span></span>

* <span data-ttu-id="bfe8f-119">Előfizetésenként modellek maximális száma: 10</span><span class="sxs-lookup"><span data-stu-id="bfe8f-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="bfe8f-120">A katalógus rendelkező elemek maximális számát: 100 000</span><span class="sxs-lookup"><span data-stu-id="bfe8f-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="bfe8f-121">Mindig használati pontok maximális számát ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-121">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="bfe8f-122">A legrégebbi esetén törlendő újakat feltöltött vagy fogja jelenteni.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-122">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="bfe8f-123">E-mailben (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200 MB</span><span class="sxs-lookup"><span data-stu-id="bfe8f-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="bfe8f-124">Tranzakció nincs aktív javaslatok modell build (TP-k) másodpercenkénti száma nem ~ 2 TP-k.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="bfe8f-125">Aktív javaslatok modell build legfeljebb 20 TP-k tárolására képes.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-125">A Recommendations model build that is active can hold up to 20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="bfe8f-126">A vásárlás és számlázási</span><span class="sxs-lookup"><span data-stu-id="bfe8f-126">Purchase and Billing</span></span>
<span data-ttu-id="bfe8f-127">**Mennyibe javaslatok költség indítási időszakban?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-127">**How much does Recommendations cost during the launch period?**</span></span>

<span data-ttu-id="bfe8f-128">Javaslatok egy olyan előfizetés-alapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="bfe8f-129">Díjszabási havonta tranzakciók mennyisége alapján történik.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="bfe8f-130">Ellenőrizheti a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) a Microsoft Azure piactéren díjszabási információkat.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-130">You can check the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="bfe8f-131">**-E javaslatok rendelkező társított költségekre nyomon követése és tárolja a felhasználói tevékenység a számomra?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="bfe8f-132">Nem abban a pillanatban.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-132">Not at the moment.</span></span>

<span data-ttu-id="bfe8f-133">**Javaslatok rendelkezik egy ingyenes próbaverzióra?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="bfe8f-134">Nincs szabad nyomokat, amely 10 000 tranzakciók havonta korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-134">There is a free trail which is restricted to 10,000 transactions per month.</span></span>

<span data-ttu-id="bfe8f-135">**Ha I alapján számlázzuk ajánlások?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="bfe8f-136">A szolgáltatás fizetős egyetlen előfizetéshez nincs havi díjért.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="bfe8f-137">Ha megvásárolja a fizetős verziót választja, azonnal számolnak hónap első használatra.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-137">When you purchase a paid subscription, you are immediately charged for the first month's use.</span></span> <span data-ttu-id="bfe8f-138">Az ajánlat az előfizetés lapján (és az adót) a társított összeg van szó.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-138">You are charged the amount that is associated with the offer on the subscription page (plus applicable taxes).</span></span> <span data-ttu-id="bfe8f-139">Ez a havi díj minden hónapban, mint az eredeti vásárlás naptár napon történik, mindaddig, amíg az előfizetés megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-139">This monthly charge is made each month on the same calendar date as your original purchase until you cancel the subscription.</span></span> 

<span data-ttu-id="bfe8f-140">**Hogyan lehet frissíteni egy magasabb réteg szolgáltatásra?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-140">**How do I upgrade to a higher tier service?**</span></span>

<span data-ttu-id="bfe8f-141">Vásároljon, vagy frissítse az előfizetést a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) oldalon, a Microsoft Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-141">You can buy or update your subscription from the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="bfe8f-142">Amikor frissít egy előfizetést:</span><span class="sxs-lookup"><span data-stu-id="bfe8f-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="bfe8f-143">A régi előfizetés hátralévő tranzakciók nem adódnak hozzá az új előfizetés.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-143">Transactions that are remaining on your old subscription are not added to your new subscription.</span></span> 
* <span data-ttu-id="bfe8f-144">Teljes ár az új előfizetés fizetnie, annak ellenére, hogy a régi előfizetésben nem használt tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-144">You pay full price for the new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="bfe8f-145">A folyamat egy előfizetés frissítése:</span><span class="sxs-lookup"><span data-stu-id="bfe8f-145">Process to upgrade a subscription:</span></span>

* <span data-ttu-id="bfe8f-146">A Nevigate a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-146">Nevigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="bfe8f-147">Ha még nem jelentkezett be, jelentkezzen be a piactéren.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-147">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="bfe8f-148">A jobb oldali ablaktáblában az elérhető csomagok vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-148">In the right pane, all the available plans are listed.</span></span> <span data-ttu-id="bfe8f-149">Kattintson a gombra, hogy frissíteni szeretné a terv.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-149">Click the radio button for the plan you want to upgrade to.</span></span>
* <span data-ttu-id="bfe8f-150">Ha frissíteni szeretné, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-150">If you want to upgrade, click **OK**.</span></span> <span data-ttu-id="bfe8f-151">Ha nem szeretné frissíteni, kattintson a **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-151">If you do not want to upgrade, click **Cancel**.</span></span>

<span data-ttu-id="bfe8f-152">**Fontos** gondosan olvassa el a párbeszédpanelen, mert vannak a számlázási és -felhasználási megvalósítását frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-152">**Important** Carefully read the dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="bfe8f-153">**Amikor véget ér az előfizetésem ajánlásokhoz?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-153">**When will my subscription to Recommendations end?**</span></span>

<span data-ttu-id="bfe8f-154">Az előfizetés véget ér, amikor vethető el.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="bfe8f-155">Ha szeretné, hogy az előfizetések megszakítja, tekintse meg az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-155">If you would like to cancel your subscriptions, see the following instructions.</span></span>

<span data-ttu-id="bfe8f-156">**Hogyan javaslatok előfizetés megszüntetése?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="bfe8f-157">Az előfizetés megszüntetéséhez használja az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-157">To cancel your subscription, use the following steps.</span></span> <span data-ttu-id="bfe8f-158">Ha a jelenlegi előfizetés a fizetős verziót választja, az előfizetés továbbra is érvényes az aktuális elszámolási időszak végéig.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-158">If your current subscription is a paid subscription, your subscription continues in effect until the end of the current billing period.</span></span> <span data-ttu-id="bfe8f-159">Ha a visszavonás azonnal érvénybe a van szüksége, lépjen velünk kapcsolatba [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-159">If you need the cancellation to be effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="bfe8f-160">**Megjegyzés:** visszatérítés nem kap, ha megszakítja a számlázási időszak vagy a nem használt tranzakciók vége előtt számlázási időszakban.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-160">**Note** No refund is given if you cancel before the end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="bfe8f-161">Keresse meg a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-161">Navigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="bfe8f-162">Ha még nem jelentkezett be, jelentkezzen be a piactéren.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-162">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="bfe8f-163">Kattintson a **Mégse** jobb oldalán a DataSet adatkészlet nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-163">Click **Cancel** to the right of the dataset name and status.</span></span> <span data-ttu-id="bfe8f-164">Ez az előfizetés az aktuális elszámolási időszak végéig is használhatja, vagy a tranzakció eléri a korlátot (amelyik következik be előbb).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-164">You can use this subscription until the end of the current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="bfe8f-165">Ha azt szeretné, azonnal, egy új előfizetést vásárolhat előfizetést, a fájl a jegy [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-165">If you would like to cancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="bfe8f-166">Ismerkedés a javaslatok</span><span class="sxs-lookup"><span data-stu-id="bfe8f-166">Getting started with Recommendations</span></span>
<span data-ttu-id="bfe8f-167">**Az ajánlások a számomra?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="bfe8f-168">A Machine Learning javaslatok a szervezetek és vállalatok számára, a kereszt-értékesít ajánlásokat, a fel-értékesít termékek és a szolgáltatások az ügyfeleknek használó van.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations to cross-sell and up-sell products or services to their customers.</span></span> <span data-ttu-id="bfe8f-169">Ha egy ügyfél-webhely, az értékesítési szakemberek, a belső értékesítés, vagy egy ügyfélszolgálatával, és, ha csak néhány dozen termékek vagy szolgáltatások katalógus, a lényeg előfordulhat, hogy előnyt az ajánlások.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="bfe8f-170">A javaslatok kísérletezés célja, hogy meglehetősen egyszerűek legyenek.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-170">Experimenting with Recommendations is designed to be fairly simple.</span></span> <span data-ttu-id="bfe8f-171">A jelenlegi API-alapú verziójával alapszintű programozási szakértelmet igényel.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-171">The current API-based version requires basic programming skills.</span></span> <span data-ttu-id="bfe8f-172">Ha segítségre van szüksége, forduljon a webhely fejlesztett szállító.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-172">If you need assistance, contact the vendor who developed your website.</span></span> <span data-ttu-id="bfe8f-173">Ha egy belső IT-részlegtől vagy egy belső fejlesztő, össze kell tudni lehetősége javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-173">If you have an internal IT department or an in-house developer, they should be able to get Recommendations to work for you.</span></span> 

<span data-ttu-id="bfe8f-174">**Mik azok a javaslatok beállításával kapcsolatos előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-174">**What are the prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="bfe8f-175">Javaslatok megköveteli, hogy Ön naplózása a felhasználó lehetőségek a katalógus vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-175">Recommendations requires that you have a log of user choices as it relates to your catalog.</span></span> <span data-ttu-id="bfe8f-176">Ha nincs ilyen naplót, és egy ügyfél elérhető webhelyek rendelkezik, a javaslatok gyűjthet felhasználói tevékenység meg.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="bfe8f-177">Javaslatok is szükséges a termékek vagy szolgáltatások egy katalógust.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="bfe8f-178">Ha nincs a katalógusban, javaslatok használhatja a tényleges felhasználói használati adatok és átalakítást egy katalógust.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-178">If you don't have the catalog, Recommendations can use the actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="bfe8f-179">Egy implicit katalógus nem fog tartalmazni, amelyek nem jelentették a felhasználói tranzakció részeként elemeket.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="bfe8f-180">**Hogyan állíthatom be javaslatok először?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-180">**How do I set up Recommendations for the first time?**</span></span>

<span data-ttu-id="bfe8f-181">Után [előfizető](https://datamarket.azure.com/dataset/amla/recommendations) ajánlásokat, az API dokumentációjának a használjon a [Azure Machine Learning ajánlásokkal – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md) állíthatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) to Recommendations, you should use the API documentation in the [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) to set up the service.</span></span>

<span data-ttu-id="bfe8f-182">**Hol található API-JÁNAK dokumentációja?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="bfe8f-183">Az API-dokumentáció [Azure Machine Learning javaslatok – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-183">The API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="bfe8f-184">**Milyen lehetőségek állnak rendelkezésre a katalógus és használati adatok feltöltése a javaslatok?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-184">**What options do I have to upload catalog and usage data to Recommendations?**</span></span>

<span data-ttu-id="bfe8f-185">A katalógus és használati adatok feltöltése a két lehetőség közül választhat: exportálja az adatokat a CRM-rendszerből vagy más naplókat, és töltse fel a javaslatok, vagy címkék hozzáadása a webhely, amelyet a felhasználói tevékenységek fogja követni.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-185">You have two options for uploading your catalog and usage data: You can export the data from your CRM system or other logs and upload it to Recommendations, or you can add tags to your website that will track user activities.</span></span> <span data-ttu-id="bfe8f-186">Ha az utóbbi módszert használja, az adattárolás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-186">If you use the latter method, the data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="bfe8f-187">Karbantartási és támogatás</span><span class="sxs-lookup"><span data-stu-id="bfe8f-187">Maintenance and support</span></span>
<span data-ttu-id="bfe8f-188">**Hogyan nagy a adatkészlet lehet?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-188">**How large can my data set be?**</span></span>

<span data-ttu-id="bfe8f-189">Minden egyes, legfeljebb 100 000 katalóguselemek tartalmaz, és akár 2048 MB-ra a használati adatok.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-189">Each data set can contain up to 100,000 catalog items and up to 2048 MB of usage data.</span></span>
<span data-ttu-id="bfe8f-190">Emellett egy előfizetés legfeljebb 10 adatkészletek (modellek) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-190">In addition, a subscription can contain up to 10 data sets (models).</span></span>

<span data-ttu-id="bfe8f-191">**Hol találhatok ajánlások a technikai támogatási szolgálathoz?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="bfe8f-192">Technikai támogatás érhető el a [Microsoft Azure támogatási](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) hely.</span><span class="sxs-lookup"><span data-stu-id="bfe8f-192">Technical support is available on the [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="bfe8f-193">**Hol találnak a használati feltételeket?**</span><span class="sxs-lookup"><span data-stu-id="bfe8f-193">**Where can I find the terms of use?**</span></span>

<span data-ttu-id="bfe8f-194">[Microsoft Azure Machine Learning szolgáltatás javaslatok API feltételeit](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="bfe8f-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

