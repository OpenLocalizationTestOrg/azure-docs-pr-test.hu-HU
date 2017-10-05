---
title: "Az adatszolgáltatás-ajánlat a piactér tesztelése |} Microsoft Docs"
description: "Megtudhatja, hogyan tesztelheti az adatszolgáltatás-ajánlat a Azure piactérről."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="7f78c-103">Az adatszolgáltatás ajánlatot átmeneti tesztelése</span><span class="sxs-lookup"><span data-stu-id="7f78c-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7f78c-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="7f78c-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="7f78c-105">Ha egy SaaS-üzleti AppSource a közzétenni kívánt alkalmazás további információt talál [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="7f78c-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="7f78c-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatás szeretne közzétenni az Azure piactérről, további információt talál [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="7f78c-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="7f78c-107">Az első két lépések végrehajtását követően [a Microsoft Developer-fiók létrehozása](marketplace-publishing-accounts-creation-registration.md) és [a szolgáltatás-ajánlatot létre közzétételi portálon](marketplace-publishing-data-service-creation.md) , hogy készen áll az ajánlatot az Azure-ban elérhetővé tétele Piactér.</span><span class="sxs-lookup"><span data-stu-id="7f78c-107">After completing the first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in the Azure Marketplace.</span></span> <span data-ttu-id="7f78c-108">Ez a témakör végigvezeti az első, köztes, a "Tesztelés" nevű lépés</span><span class="sxs-lookup"><span data-stu-id="7f78c-108">This topic will walk you through the first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="7f78c-109">Átmeneti azt jelenti, hogy a "védőfal", ahol tesztelése és funkciókat ellenőrzése előtt az üzemi környezetben privát ajánlatát telepítése.</span><span class="sxs-lookup"><span data-stu-id="7f78c-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it to production.</span></span> <span data-ttu-id="7f78c-110">Az ajánlat volna egy felhasználói, akik már telepítették az átmeneti fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="7f78c-110">The offer will appear in staging just as it would to a customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-to-staging"></a><span data-ttu-id="7f78c-111">1. lépés</span><span class="sxs-lookup"><span data-stu-id="7f78c-111">Step 1.</span></span> <span data-ttu-id="7f78c-112">A lefokozásra szolgáló átmeneti küldését</span><span class="sxs-lookup"><span data-stu-id="7f78c-112">Pushing your offer to staging</span></span>
<span data-ttu-id="7f78c-113">Kérdez le az átmeneti tesznek lehetővé teszi, hogy az ajánlat tesztelni, mielőtt jövőbeli előfizetők számára elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="7f78c-113">Pushing your offer to staging allows you to test the offer before it becomes available to future subscribers.</span></span>  <span data-ttu-id="7f78c-114">Láthatja, hogyan ajánlatát fog jelenik meg, és azok az adatok előfizetés működik.</span><span class="sxs-lookup"><span data-stu-id="7f78c-114">You can see how your offer will appear and function for those subscribing to your data.</span></span>  

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="7f78c-116">Bejelentkezés a a [Portal közzététele](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="7f78c-116">Login into the [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="7f78c-117">Válassza ki **adatszolgáltatások** a bal oldali navigációs ablakban</span><span class="sxs-lookup"><span data-stu-id="7f78c-117">Select **Data Services** in the left navigation window</span></span>
3. <span data-ttu-id="7f78c-118">Válassza ki az átmeneti leküldése kívánt ajánlatot.</span><span class="sxs-lookup"><span data-stu-id="7f78c-118">Select your offer you want to push to staging.</span></span> <span data-ttu-id="7f78c-119">A fenti képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f78c-119">You will see the above screen.</span></span>
4. <span data-ttu-id="7f78c-120">Kattintson a **leküldéses átmeneti** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f78c-120">Click **Push To Staging** button.</span></span>  
5. <span data-ttu-id="7f78c-121">Az ajánlat, amelyek végrehajtása előtt kérdez le, hogy átmeneti problémák vannak, ha megjelenik egy lista jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f78c-121">If there are issues with the offer that needed to be completed prior to pushing to staging, you will see a list displayed.</span></span>  <span data-ttu-id="7f78c-122">Javítsa ki ezeket az elemeket a lista összes elemére kattintva.</span><span class="sxs-lookup"><span data-stu-id="7f78c-122">Correct these items by clicking on each item in the list.</span></span> <span data-ttu-id="7f78c-123">Amikor végzett, az összes korrekció kattintson **átmeneti leküldése** újra gombra.</span><span class="sxs-lookup"><span data-stu-id="7f78c-123">When all corrections made, click **Push to Staging** button again.</span></span>

<span data-ttu-id="7f78c-124">Ha nincsenek a ajánlat problémák látni fogja az alábbi előugró ablak.</span><span class="sxs-lookup"><span data-stu-id="7f78c-124">If there are no issues with your offer you will see the popup window below.</span></span>  

<span data-ttu-id="7f78c-125">Ha még nem tervezési/nem jóvá az Azure portálon ajánlatot illesztésének (jelenleg korlátozott kapacitás), majd bezárja az előugró ablak.</span><span class="sxs-lookup"><span data-stu-id="7f78c-125">If you’re not planning/not approved to surface your offer in Azure Portal (currently has limited capacity), then just close the pop-up window.</span></span>

<span data-ttu-id="7f78c-126">A szolgáltatás teszteléséhez az Azure portálon (mellett a DataMarket portál), szüksége lesz egy Azure-előfizetési Azonosítót tesztelni.</span><span class="sxs-lookup"><span data-stu-id="7f78c-126">To test your Data Service in Azure Portal (in addition to the DataMarket portal), you will need an Azure Subscription ID to test with.</span></span>  <span data-ttu-id="7f78c-127">Az előfizetés-azonosító azonosítja a fiókot, amelyet a rendszer engedélyezi-e az ajánlatot tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7f78c-127">This Subscription ID will identify the account that will be allowed to test your offer.</span></span>  

<span data-ttu-id="7f78c-128">Kivágás, és illessze be az előfizetés-Azonosítóval, és kattintson a pipa jelre a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="7f78c-128">Cut and paste your Subscription ID and click the checkmark to continue.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="7f78c-130">Ezek az Azure-előfizetések azonosítói csak tesztelési és átmeneti előkészítő a szükséges a [Azure felügyeleti portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7f78c-130">These Azure subscriptions IDs are only required for testing and staging in the [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="7f78c-131">Ezek nem szükségesek tesztelése az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="7f78c-131">They are not required to test in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="7f78c-132">A következő képernyőn megjelenő mutat be, hogy a közzététel van folyamatban a "folyamatban" ikonra megjelenítésével jelzi ezt az alábbi sárga kiemelve.</span><span class="sxs-lookup"><span data-stu-id="7f78c-132">The next screen that appears shows that publishing is taking place by displaying the “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="7f78c-133">Kérdez le, hogy átmeneti közötti 10 – 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7f78c-133">Pushing to staging takes between 10 to 15 minutes.</span></span>  <span data-ttu-id="7f78c-134">Ha hosszabb ideig tart, először frissítse a böngésző (nyomja le az F5 IE-ben).</span><span class="sxs-lookup"><span data-stu-id="7f78c-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="7f78c-135">Bizonyos ritkán előforduló esetekben, ahol az ajánlatot továbbra is küld egy óra múlva átmeneti, kattintson a lépjen kapcsolatba velünk csatolása ossza meg velünk, hogy probléma van.</span><span class="sxs-lookup"><span data-stu-id="7f78c-135">In the rare cases where your offer is still pushing to staging after an hour, click the contact us link to let us know that there is an issue.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="7f78c-137">A "folyamatban" ikonra a leküldéses átmeneti befejeződésekor stop áthelyezése és az állapot lesz frissíti "Előkészített".</span><span class="sxs-lookup"><span data-stu-id="7f78c-137">When the Push to Staging completes the “In progress” icon will stop moving and the status will be updated to “Staged”.</span></span>  <span data-ttu-id="7f78c-138">Most már készen áll az ajánlatot teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7f78c-138">You are now ready to test your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="7f78c-139">2. lépés</span><span class="sxs-lookup"><span data-stu-id="7f78c-139">Step 2.</span></span> <span data-ttu-id="7f78c-140">Az előkészített ajánlatot DataMarket tesztelése</span><span class="sxs-lookup"><span data-stu-id="7f78c-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="7f78c-141">A szöveg a következő hivatkozásra kattintva **"tekintse meg a szolgáltatás következő kínálnak..."**</span><span class="sxs-lookup"><span data-stu-id="7f78c-141">Click the link following the text **“See Your service offer at…”**</span></span> <span data-ttu-id="7f78c-142">megjelenítéséhez a képernyőn, hogy az előfizető megjelenik, amikor az ajánlat éles kerül, és DataMarket fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="7f78c-142">to display the screen that the subscriber will see when your offer goes to production and will appear in DataMarket.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="7f78c-144">Tesztelje, vagy ellenőrizze a 12 elemeket megjelölve, fent győződjön meg arról, minden emblémák, árak/tranzakciók, szöveg, képek, dokumentáció, és hivatkozások helyességét, és működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7f78c-144">Test or verify each of the 12 items marked above to ensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="7f78c-145">Ez az egy időben teszt értékeket az ajánlat létrehozásakor megadott helyett tényleges értékeket biztosításához.</span><span class="sxs-lookup"><span data-stu-id="7f78c-145">This is a good time to ensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="7f78c-146">Az ajánlat embléma</span><span class="sxs-lookup"><span data-stu-id="7f78c-146">Offer logo</span></span>
2. <span data-ttu-id="7f78c-147">Az ajánlat neve</span><span class="sxs-lookup"><span data-stu-id="7f78c-147">Offer name</span></span>
3. <span data-ttu-id="7f78c-148">Közzétevő neve/hivatkozásra a vállalati webhely</span><span class="sxs-lookup"><span data-stu-id="7f78c-148">Publisher name/link to your company's website</span></span>
4. <span data-ttu-id="7f78c-149">Az ajánlat kategóriák keresése</span><span class="sxs-lookup"><span data-stu-id="7f78c-149">Search categories for your offer</span></span>
5. <span data-ttu-id="7f78c-150">Az ajánlat támogatási hivatkozás előfizetők segít</span><span class="sxs-lookup"><span data-stu-id="7f78c-150">Your offer's support link to assist subscribers</span></span>
6. <span data-ttu-id="7f78c-151">Környezetfüggő leírást az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="7f78c-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="7f78c-152">Az ajánlat terv végzett ügyfélellenőrzések számlázási részletek</span><span class="sxs-lookup"><span data-stu-id="7f78c-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="7f78c-153">Végrehajtási kód csatolása</span><span class="sxs-lookup"><span data-stu-id="7f78c-153">Link to implementation code</span></span>
9. <span data-ttu-id="7f78c-154">Minta képek, mely ajánlat adatok használata</span><span class="sxs-lookup"><span data-stu-id="7f78c-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="7f78c-155">Az ajánlat belül minden egyes szolgáltatás bemeneti/kimeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="7f78c-155">Input/Output metadata for each service within the offer</span></span>
11. <span data-ttu-id="7f78c-156">Ajánlat használati feltételek</span><span class="sxs-lookup"><span data-stu-id="7f78c-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="7f78c-157">Az ajánlat adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="7f78c-157">Preview of the offer's data</span></span>

<span data-ttu-id="7f78c-158">Végül ellenőrizze a szolgáltatás "MEGISMERKEDHET a DATASET" hivatkozásra kattintva a Datamarket keresztül fog működni.</span><span class="sxs-lookup"><span data-stu-id="7f78c-158">Finally, check the service will work through the Datamarket by clicking the link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="7f78c-159">Egy új ablakban nyílik meg az eszköz a közvetlen telepítésnek "Szolgáltatás Explorer", egy lekérdezés eredményét megtekintheti a szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="7f78c-159">A new window will open in the tool we call “Service Explorer” so you can preview the results of a query against your service.</span></span>  <span data-ttu-id="7f78c-160">Ebben az ablakban adja meg a szükséges paramétereket, és tekintse meg a szolgáltatás egy lekérdezés által megjelenített eredményekre.</span><span class="sxs-lookup"><span data-stu-id="7f78c-160">In this window, you can enter the parameters needed and see the results displayed from a query against your service.</span></span>   <span data-ttu-id="7f78c-161">Megjelenik a, az URL-címet a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="7f78c-161">Also, displayed is the URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="7f78c-162">Győződjön meg arról, hogy tekintse át a szolgáltatás a lap tetején megjelenik a szöveges leírása.</span><span class="sxs-lookup"><span data-stu-id="7f78c-162">Be sure to review the textual description of the service displayed at the top.</span></span>  <span data-ttu-id="7f78c-163">És ha az ajánlatot több szolgáltatás hívja, kattintson a lap alján váltson át a következő szolgáltatás tekintse át és tesztelje a lappal.</span><span class="sxs-lookup"><span data-stu-id="7f78c-163">And if your offer consists of more than one service call, click the tabs at the bottom to switch to the next service to review and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="7f78c-164">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="7f78c-164">Next step</span></span>
<span data-ttu-id="7f78c-165">Ha problémát tapasztal, és segítségre van szüksége megoldása kérdéseivel forduljon [Azure közzétevő támogatási csoportjához](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="7f78c-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="7f78c-166">Ha-e meg, és készen állnak a közzététele az ajánlatot, olvassa el a [vonatkozó kérés jóváhagyása leküldéses üzemi](marketplace-publishing-push-to-production.md) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="7f78c-166">If you are satisfied and ready to publish your offer please read the [Request Approval to Push To Production](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="7f78c-167">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7f78c-167">See Also</span></span>
* [<span data-ttu-id="7f78c-168">Első lépések: Hogyan ajánlat közzététele az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="7f78c-168">Getting Started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

