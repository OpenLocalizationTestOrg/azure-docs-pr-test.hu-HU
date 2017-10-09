---
title: "aaaSet be és használja a Machine Learning javaslatok API hello |} Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="0785c-103">A Machine Learning Recommendations API beállítása és használata – GYIK</span><span class="sxs-lookup"><span data-stu-id="0785c-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="0785c-104">**Mi az az ajánlások?**</span><span class="sxs-lookup"><span data-stu-id="0785c-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="0785c-105">El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="0785c-105">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="0785c-106">hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="0785c-106">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="0785c-107">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="0785c-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="0785c-108">További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="0785c-108">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="0785c-109">A szervezetek és vállalatok számára a javaslatok toocross-értékesít és felfelé-értékesít termékek és szolgáltatások tootheir ügyfelek használó javaslatok az Azure Machine Learning egy önkiszolgáló javaslatok motor biztosít.</span><span class="sxs-lookup"><span data-stu-id="0785c-109">For organizations and businesses that rely on recommendations toocross-sell and up-sell products and services tootheir customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="0785c-110">A core algoritmus mátrix factorization használó együttműködési szűrés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="0785c-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="0785c-111">JAVASLATOK férhetnek hozzá alkalmazásfejlesztők REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="0785c-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="0785c-112">**Mi a teendő a javaslatok?**</span><span class="sxs-lookup"><span data-stu-id="0785c-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="0785c-113">JAVASLATOK vesz igénybe, adjon meg egy elem vagy egy elemet, és a vonatkozó javaslatok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0785c-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="0785c-114">Például: az ügyfél egy online közvetítő kattint egy termék.</span><span class="sxs-lookup"><span data-stu-id="0785c-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="0785c-115">hello online közvetítő elküldi a terméket, bemeneti tooRECOMMENDATIONS, ismét lekérdezi a termékek listáját, és úgy dönt, amelyhez ezeket a termékeket toohello ügyfél megjelenik.</span><span class="sxs-lookup"><span data-stu-id="0785c-115">hello online retailer sends that product as input tooRECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown toohello customer.</span></span> <span data-ttu-id="0785c-116">Előfordulhat, hogy toouse javaslatok toooptimize szeretné, hogy az online áruházból vagy tooinform még a belső értékesítési részleg vagy hívás center.</span><span class="sxs-lookup"><span data-stu-id="0785c-116">You may want toouse RECOMMENDATIONS toooptimize your online store or even tooinform your inside sales department or call center.</span></span>

<span data-ttu-id="0785c-117">**Vannak-e használati korlátozások?**</span><span class="sxs-lookup"><span data-stu-id="0785c-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="0785c-118">Javaslatok a következő használati korlátozások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="0785c-118">Recommendations has hello following usage limitations:</span></span>

* <span data-ttu-id="0785c-119">Előfizetésenként modellek maximális száma: 10</span><span class="sxs-lookup"><span data-stu-id="0785c-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="0785c-120">A katalógus rendelkező elemek maximális számát: 100 000</span><span class="sxs-lookup"><span data-stu-id="0785c-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="0785c-121">mindig használati pontok maximális számát hello ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="0785c-121">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="0785c-122">Ha újakat feltöltött vagy fogja jelenteni a legrégebbi hello törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="0785c-122">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="0785c-123">E-mailben (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200 MB</span><span class="sxs-lookup"><span data-stu-id="0785c-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="0785c-124">Tranzakció nincs aktív javaslatok modell build (TP-k) másodpercenkénti száma nem ~ 2 TP-k.</span><span class="sxs-lookup"><span data-stu-id="0785c-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="0785c-125">Aktív javaslatok modell build mentése too20 TP-k tárolására képes.</span><span class="sxs-lookup"><span data-stu-id="0785c-125">A Recommendations model build that is active can hold up too20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="0785c-126">A vásárlás és számlázási</span><span class="sxs-lookup"><span data-stu-id="0785c-126">Purchase and Billing</span></span>
<span data-ttu-id="0785c-127">**Javaslatok mennyi nem költség hello indítási időszakban?**</span><span class="sxs-lookup"><span data-stu-id="0785c-127">**How much does Recommendations cost during hello launch period?**</span></span>

<span data-ttu-id="0785c-128">Javaslatok egy olyan előfizetés-alapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0785c-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="0785c-129">Díjszabási havonta tranzakciók mennyisége alapján történik.</span><span class="sxs-lookup"><span data-stu-id="0785c-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="0785c-130">Ellenőrizheti a hello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) a Microsoft Azure piactéren díjszabási információkat.</span><span class="sxs-lookup"><span data-stu-id="0785c-130">You can check hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="0785c-131">**-E javaslatok rendelkező társított költségekre nyomon követése és tárolja a felhasználói tevékenység a számomra?**</span><span class="sxs-lookup"><span data-stu-id="0785c-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="0785c-132">Nem pillanatban hello.</span><span class="sxs-lookup"><span data-stu-id="0785c-132">Not at hello moment.</span></span>

<span data-ttu-id="0785c-133">**Javaslatok rendelkezik egy ingyenes próbaverzióra?**</span><span class="sxs-lookup"><span data-stu-id="0785c-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="0785c-134">Nincs szabad nyomokat, amely korlátozott too10, havonta 000 tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="0785c-134">There is a free trail which is restricted too10,000 transactions per month.</span></span>

<span data-ttu-id="0785c-135">**Ha I alapján számlázzuk ajánlások?**</span><span class="sxs-lookup"><span data-stu-id="0785c-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="0785c-136">A szolgáltatás fizetős egyetlen előfizetéshez nincs havi díjért.</span><span class="sxs-lookup"><span data-stu-id="0785c-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="0785c-137">Ha megvásárolja a fizetős verziót választja, akkor azonnal felszámított hello először havi használja.</span><span class="sxs-lookup"><span data-stu-id="0785c-137">When you purchase a paid subscription, you are immediately charged for hello first month's use.</span></span> <span data-ttu-id="0785c-138">Hello összeg hello ajánlat hello előfizetés lapján (és az adót) a társított van szó.</span><span class="sxs-lookup"><span data-stu-id="0785c-138">You are charged hello amount that is associated with hello offer on hello subscription page (plus applicable taxes).</span></span> <span data-ttu-id="0785c-139">Ez a havi díj havonta készült hello azonos naptári dátum, ahogy az eredeti vásárlás, amíg hello előfizetés megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="0785c-139">This monthly charge is made each month on hello same calendar date as your original purchase until you cancel hello subscription.</span></span> 

<span data-ttu-id="0785c-140">**Hogyan lehet frissíteni a tooa magasabb réteg szolgáltatás?**</span><span class="sxs-lookup"><span data-stu-id="0785c-140">**How do I upgrade tooa higher tier service?**</span></span>

<span data-ttu-id="0785c-141">Vásároljon, vagy frissítse az előfizetést hello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) oldalon, a Microsoft Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="0785c-141">You can buy or update your subscription from hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="0785c-142">Amikor frissít egy előfizetést:</span><span class="sxs-lookup"><span data-stu-id="0785c-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="0785c-143">A régi előfizetés hátralévő tranzakciók nem kerülnek tooyour új előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0785c-143">Transactions that are remaining on your old subscription are not added tooyour new subscription.</span></span> 
* <span data-ttu-id="0785c-144">Kell fizetnie teljes ár hello új előfizetés, annak ellenére, hogy a régi előfizetésben nem használt tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="0785c-144">You pay full price for hello new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="0785c-145">Folyamat tooupgrade előfizetést:</span><span class="sxs-lookup"><span data-stu-id="0785c-145">Process tooupgrade a subscription:</span></span>

* <span data-ttu-id="0785c-146">Nevigate toohello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="0785c-146">Nevigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="0785c-147">Jelentkezzen be a piactér toohello, ha még nem jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="0785c-147">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="0785c-148">A jobb oldali hello összes hello elérhető csomagok vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="0785c-148">In hello right pane, all hello available plans are listed.</span></span> <span data-ttu-id="0785c-149">Kattintson a tooupgrade kívánt hello terv hello választógombra.</span><span class="sxs-lookup"><span data-stu-id="0785c-149">Click hello radio button for hello plan you want tooupgrade to.</span></span>
* <span data-ttu-id="0785c-150">Ha azt szeretné, hogy tooupgrade, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="0785c-150">If you want tooupgrade, click **OK**.</span></span> <span data-ttu-id="0785c-151">Ha nem szeretné, hogy tooupgrade, kattintson a **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="0785c-151">If you do not want tooupgrade, click **Cancel**.</span></span>

<span data-ttu-id="0785c-152">**Fontos** gondosan olvasási hello párbeszédpanel, mert számlázási és -felhasználási implications van frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="0785c-152">**Important** Carefully read hello dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="0785c-153">**Amikor véget ér az előfizetés tooRecommendations?**</span><span class="sxs-lookup"><span data-stu-id="0785c-153">**When will my subscription tooRecommendations end?**</span></span>

<span data-ttu-id="0785c-154">Az előfizetés véget ér, amikor vethető el.</span><span class="sxs-lookup"><span data-stu-id="0785c-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="0785c-155">Ha azt szeretné, hogy toocancel előfizetése, tekintse meg az utasításoknak hello.</span><span class="sxs-lookup"><span data-stu-id="0785c-155">If you would like toocancel your subscriptions, see hello following instructions.</span></span>

<span data-ttu-id="0785c-156">**Hogyan javaslatok előfizetés megszüntetése?**</span><span class="sxs-lookup"><span data-stu-id="0785c-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="0785c-157">az előfizetés, a következő használatát hello lépések toocancel.</span><span class="sxs-lookup"><span data-stu-id="0785c-157">toocancel your subscription, use hello following steps.</span></span> <span data-ttu-id="0785c-158">Ha a jelenlegi előfizetés a fizetős verziót választja, az előfizetés továbbra is érvényben aktuális számlázási időszak hello hello végéig.</span><span class="sxs-lookup"><span data-stu-id="0785c-158">If your current subscription is a paid subscription, your subscription continues in effect until hello end of hello current billing period.</span></span> <span data-ttu-id="0785c-159">Ha hatékony cancellation toobe azonnal kell hello, írjon nekünk az [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="0785c-159">If you need hello cancellation toobe effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="0785c-160">**Megjegyzés:** visszatérítés nem kap, ha megszakítja a számlázási időszak vagy a nem használt tranzakciók hello vége előtt számlázási időszakban.</span><span class="sxs-lookup"><span data-stu-id="0785c-160">**Note** No refund is given if you cancel before hello end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="0785c-161">Keresse meg a toohello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="0785c-161">Navigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="0785c-162">Jelentkezzen be a piactér toohello, ha még nem jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="0785c-162">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="0785c-163">Kattintson a **Mégse** toohello sarkában hello adatkészlet nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="0785c-163">Click **Cancel** toohello right of hello dataset name and status.</span></span> <span data-ttu-id="0785c-164">Ehhez az előfizetéshez is használhatja, amíg el nem hello végét hello aktuális számlázási időszak vagy tranzakció vonatkozó felső korlát (amelyik következik be előbb).</span><span class="sxs-lookup"><span data-stu-id="0785c-164">You can use this subscription until hello end of hello current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="0785c-165">Ha azt szeretné, hogy az előfizetés azonnal, vásárolhat egy új előfizetés fájlt a jegy toocancel [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="0785c-165">If you would like toocancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="0785c-166">Ismerkedés a javaslatok</span><span class="sxs-lookup"><span data-stu-id="0785c-166">Getting started with Recommendations</span></span>
<span data-ttu-id="0785c-167">**Az ajánlások a számomra?**</span><span class="sxs-lookup"><span data-stu-id="0785c-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="0785c-168">A Machine Learning javaslatok a szervezetek és vállalatok számára a javaslatok toocross-értékesít és a fel-értékesít termékek vagy szolgáltatások tootheir ügyfél használó van.</span><span class="sxs-lookup"><span data-stu-id="0785c-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations toocross-sell and up-sell products or services tootheir customers.</span></span> <span data-ttu-id="0785c-169">Ha egy ügyfél-webhely, az értékesítési szakemberek, a belső értékesítés, vagy egy ügyfélszolgálatával, és, ha csak néhány dozen termékek vagy szolgáltatások katalógus, a lényeg előfordulhat, hogy előnyt az ajánlások.</span><span class="sxs-lookup"><span data-stu-id="0785c-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="0785c-170">Javaslatok ki tervezett toobe viszonylag egyszerű.</span><span class="sxs-lookup"><span data-stu-id="0785c-170">Experimenting with Recommendations is designed toobe fairly simple.</span></span> <span data-ttu-id="0785c-171">hello jelenlegi API-alapú verziójával alapszintű programozási szakértelmet igényel.</span><span class="sxs-lookup"><span data-stu-id="0785c-171">hello current API-based version requires basic programming skills.</span></span> <span data-ttu-id="0785c-172">Ha segítségre van szüksége, forduljon a webhely fejlesztett hello szállító.</span><span class="sxs-lookup"><span data-stu-id="0785c-172">If you need assistance, contact hello vendor who developed your website.</span></span> <span data-ttu-id="0785c-173">Ha egy belső IT-részlegtől vagy egy belső fejlesztő, képes tooget javaslatok toowork meg kell.</span><span class="sxs-lookup"><span data-stu-id="0785c-173">If you have an internal IT department or an in-house developer, they should be able tooget Recommendations toowork for you.</span></span> 

<span data-ttu-id="0785c-174">**Mik azok a javaslatok beállítása hello előfeltételei**</span><span class="sxs-lookup"><span data-stu-id="0785c-174">**What are hello prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="0785c-175">Javaslatok megköveteli, hogy Ön naplózása a felhasználó lehetőségek tooyour katalógus vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="0785c-175">Recommendations requires that you have a log of user choices as it relates tooyour catalog.</span></span> <span data-ttu-id="0785c-176">Ha nincs ilyen naplót, és egy ügyfél elérhető webhelyek rendelkezik, a javaslatok gyűjthet felhasználói tevékenység meg.</span><span class="sxs-lookup"><span data-stu-id="0785c-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="0785c-177">Javaslatok is szükséges a termékek vagy szolgáltatások egy katalógust.</span><span class="sxs-lookup"><span data-stu-id="0785c-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="0785c-178">Ha hello katalógus nem rendelkezik, javaslatok használhatja hello tényleges felhasználói használati adatok és a katalógus átalakítást.</span><span class="sxs-lookup"><span data-stu-id="0785c-178">If you don't have hello catalog, Recommendations can use hello actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="0785c-179">Egy implicit katalógus nem fog tartalmazni, amelyek nem jelentették a felhasználói tranzakció részeként elemeket.</span><span class="sxs-lookup"><span data-stu-id="0785c-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="0785c-180">**Hogyan állíthatom be javaslatok hello az első alkalommal?**</span><span class="sxs-lookup"><span data-stu-id="0785c-180">**How do I set up Recommendations for hello first time?**</span></span>

<span data-ttu-id="0785c-181">Után [előfizető](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, használja a hello hello API dokumentációjában [Azure Machine Learning ajánlásokkal – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md) tooset hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0785c-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, you should use hello API documentation in hello [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) tooset up hello service.</span></span>

<span data-ttu-id="0785c-182">**Hol található API-JÁNAK dokumentációja?**</span><span class="sxs-lookup"><span data-stu-id="0785c-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="0785c-183">API-JÁNAK dokumentációja hello van [Azure Machine Learning javaslatok – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="0785c-183">hello API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="0785c-184">**Beállítások mire tooupload katalógus és használati adatok tooRecommendations van?**</span><span class="sxs-lookup"><span data-stu-id="0785c-184">**What options do I have tooupload catalog and usage data tooRecommendations?**</span></span>

<span data-ttu-id="0785c-185">A katalógus és használati adatok feltöltése a két lehetőség közül választhat: hello adatok exportálása a CRM-rendszerből vagy más naplókat, és töltse fel az tooRecommendations, vagy hozzáadhat címkék tooyour webhelyet, ahol a felhasználói tevékenységek fogja követni.</span><span class="sxs-lookup"><span data-stu-id="0785c-185">You have two options for uploading your catalog and usage data: You can export hello data from your CRM system or other logs and upload it tooRecommendations, or you can add tags tooyour website that will track user activities.</span></span> <span data-ttu-id="0785c-186">Ha ez utóbbi hello a módszert használja, hello adattárolás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0785c-186">If you use hello latter method, hello data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="0785c-187">Karbantartási és támogatás</span><span class="sxs-lookup"><span data-stu-id="0785c-187">Maintenance and support</span></span>
<span data-ttu-id="0785c-188">**Hogyan nagy a adatkészlet lehet?**</span><span class="sxs-lookup"><span data-stu-id="0785c-188">**How large can my data set be?**</span></span>

<span data-ttu-id="0785c-189">Egyes adatokat is tartalmazhatnak too100, 000 a szolgáltatáskatalógusban található elemek és használati adatok too2048 MB.</span><span class="sxs-lookup"><span data-stu-id="0785c-189">Each data set can contain up too100,000 catalog items and up too2048 MB of usage data.</span></span>
<span data-ttu-id="0785c-190">Emellett egy előfizetés mentése too10 adatkészletek (modellek) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="0785c-190">In addition, a subscription can contain up too10 data sets (models).</span></span>

<span data-ttu-id="0785c-191">**Hol találhatok ajánlások a technikai támogatási szolgálathoz?**</span><span class="sxs-lookup"><span data-stu-id="0785c-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="0785c-192">Technikai támogatás érhető el hello [Microsoft Azure támogatási](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) hely.</span><span class="sxs-lookup"><span data-stu-id="0785c-192">Technical support is available on hello [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="0785c-193">**Hol találnak a használati feltételek hello?**</span><span class="sxs-lookup"><span data-stu-id="0785c-193">**Where can I find hello terms of use?**</span></span>

<span data-ttu-id="0785c-194">[Microsoft Azure Machine Learning szolgáltatás javaslatok API feltételeit](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="0785c-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

