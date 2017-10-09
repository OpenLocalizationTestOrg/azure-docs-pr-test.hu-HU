---
title: "az adatszolgáltatás hello Piactéri ajánlat aaaTesting |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest a adatszolgáltatás ajánlat hello Azure piactéren."
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
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="86239-103">Az adatszolgáltatás ajánlatot átmeneti tesztelése</span><span class="sxs-lookup"><span data-stu-id="86239-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="86239-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="86239-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="86239-105">Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="86239-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="86239-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="86239-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="86239-107">Hello két lépést az befejezése után [a Microsoft Developer-fiók létrehozása](marketplace-publishing-accounts-creation-registration.md) és [a szolgáltatás-ajánlatot létre közzétételi portálon](marketplace-publishing-data-service-creation.md) , hogy készen áll az ajánlatot hello elérhetővé tétele Az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="86239-107">After completing hello first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in hello Azure Marketplace.</span></span> <span data-ttu-id="86239-108">Ez a témakör fog végigvezetik hello először, köztes, hívott "tesztelés" lépés</span><span class="sxs-lookup"><span data-stu-id="86239-108">This topic will walk you through hello first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="86239-109">Átmeneti azt jelenti, hogy az ajánlatot "védőfal", ahol tesztelése és funkciókat ellenőrzése előtt az tooproduction privát telepítése.</span><span class="sxs-lookup"><span data-stu-id="86239-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it tooproduction.</span></span> <span data-ttu-id="86239-110">hello ajánlat tooa felhasználói, akik már telepítették volna átmeneti fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="86239-110">hello offer will appear in staging just as it would tooa customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-toostaging"></a><span data-ttu-id="86239-111">1. lépés</span><span class="sxs-lookup"><span data-stu-id="86239-111">Step 1.</span></span> <span data-ttu-id="86239-112">Az ajánlat toostaging küldését</span><span class="sxs-lookup"><span data-stu-id="86239-112">Pushing your offer toostaging</span></span>
<span data-ttu-id="86239-113">Az ajánlat toostaging küldését teszi lehetővé tootest hello ajánlat ahhoz, hogy elérhető toofuture előfizetők.</span><span class="sxs-lookup"><span data-stu-id="86239-113">Pushing your offer toostaging allows you tootest hello offer before it becomes available toofuture subscribers.</span></span>  <span data-ttu-id="86239-114">Láthatja, hogyan ajánlatát fog jelenik meg, és azok tooyour adatok előfizetés működik.</span><span class="sxs-lookup"><span data-stu-id="86239-114">You can see how your offer will appear and function for those subscribing tooyour data.</span></span>  

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="86239-116">Bejelentkezés a hello [közzétételi Portáljára](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="86239-116">Login into hello [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="86239-117">Válassza ki **adatszolgáltatások** hello bal oldali navigációs ablakban</span><span class="sxs-lookup"><span data-stu-id="86239-117">Select **Data Services** in hello left navigation window</span></span>
3. <span data-ttu-id="86239-118">Válassza ki a kívánt toopush toostaging ajánlatot.</span><span class="sxs-lookup"><span data-stu-id="86239-118">Select your offer you want toopush toostaging.</span></span> <span data-ttu-id="86239-119">Hello képernyője felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86239-119">You will see hello above screen.</span></span>
4. <span data-ttu-id="86239-120">Kattintson a **tooStaging leküldéses** gombra.</span><span class="sxs-lookup"><span data-stu-id="86239-120">Click **Push tooStaging** button.</span></span>  
5. <span data-ttu-id="86239-121">Ha problémák vannak az hello ajánlat, amely szükséges toobe előzetes toopushing toostaging befejeződött, megjelenik egy lista jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86239-121">If there are issues with hello offer that needed toobe completed prior toopushing toostaging, you will see a list displayed.</span></span>  <span data-ttu-id="86239-122">Javítsa ki ezeket az elemeket hello lista összes elemére kattintva.</span><span class="sxs-lookup"><span data-stu-id="86239-122">Correct these items by clicking on each item in hello list.</span></span> <span data-ttu-id="86239-123">Amikor végzett, az összes korrekció kattintson **tooStaging leküldéses** újra gombra.</span><span class="sxs-lookup"><span data-stu-id="86239-123">When all corrections made, click **Push tooStaging** button again.</span></span>

<span data-ttu-id="86239-124">Ha nincsenek a ajánlat problémák hello előugró ablakban az alábbi jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86239-124">If there are no issues with your offer you will see hello popup window below.</span></span>  

<span data-ttu-id="86239-125">Ha még nem jóváhagyott toosurface az Azure portálon ajánlatot tervezési/nem (jelenleg korlátozott kapacitás), majd az imént Bezárás hello előugró ablak.</span><span class="sxs-lookup"><span data-stu-id="86239-125">If you’re not planning/not approved toosurface your offer in Azure Portal (currently has limited capacity), then just close hello pop-up window.</span></span>

<span data-ttu-id="86239-126">tootest az adatok szolgáltatás az Azure portálon (a hozzáadása toohello DataMarket portál), szüksége lesz egy Azure-előfizetési Azonosítót tootest rendelkező.</span><span class="sxs-lookup"><span data-stu-id="86239-126">tootest your Data Service in Azure Portal (in addition toohello DataMarket portal), you will need an Azure Subscription ID tootest with.</span></span>  <span data-ttu-id="86239-127">Az előfizetés-azonosító hello fiók, amely azonosítja tootest ajánlatát engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="86239-127">This Subscription ID will identify hello account that will be allowed tootest your offer.</span></span>  

<span data-ttu-id="86239-128">Kivágás, és illessze be az előfizetés-Azonosítóval, és kattintson a pipa toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="86239-128">Cut and paste your Subscription ID and click hello checkmark toocontinue.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="86239-130">Ezek az Azure-előfizetések azonosítói csak tesztelési és átmeneti előkészítő hello a szükséges [Azure felügyeleti portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="86239-130">These Azure subscriptions IDs are only required for testing and staging in hello [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="86239-131">Nincsenek szükséges tootest Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="86239-131">They are not required tootest in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="86239-132">hello tovább a képernyőn megjelenő látható, hogy a közzététel lefolyása "folyamatban" hello megjelenítésével ikonja kiemelve az alábbi sárga.</span><span class="sxs-lookup"><span data-stu-id="86239-132">hello next screen that appears shows that publishing is taking place by displaying hello “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="86239-133">Toostaging küldését veszi too15 10 perc között.</span><span class="sxs-lookup"><span data-stu-id="86239-133">Pushing toostaging takes between 10 too15 minutes.</span></span>  <span data-ttu-id="86239-134">Ha hosszabb ideig tart, először frissítse a böngésző (nyomja le az F5 IE-ben).</span><span class="sxs-lookup"><span data-stu-id="86239-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="86239-135">Bizonyos ritkán előforduló esetekben, ahol az ajánlat még küldését toostaging egy óra elteltével hello kattintson hello lépjen kapcsolatba velünk toolet arról, hogy probléma van a hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="86239-135">In hello rare cases where your offer is still pushing toostaging after an hour, click hello contact us link toolet us know that there is an issue.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="86239-137">Hello leküldéses tooStaging befejezéséről hello "folyamatban" ikonra a továbbiakban nem áthelyezése és túl "előkészített" hello állapota frissül.</span><span class="sxs-lookup"><span data-stu-id="86239-137">When hello Push tooStaging completes hello “In progress” icon will stop moving and hello status will be updated too“Staged”.</span></span>  <span data-ttu-id="86239-138">Ön most már készen áll a tootest ajánlatát vannak.</span><span class="sxs-lookup"><span data-stu-id="86239-138">You are now ready tootest your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="86239-139">2. lépés</span><span class="sxs-lookup"><span data-stu-id="86239-139">Step 2.</span></span> <span data-ttu-id="86239-140">Az előkészített ajánlatot DataMarket tesztelése</span><span class="sxs-lookup"><span data-stu-id="86239-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="86239-141">A következő hello szöveg hello hivatkozásra **"tekintse meg a szolgáltatás következő kínálnak..."**</span><span class="sxs-lookup"><span data-stu-id="86239-141">Click hello link following hello text **“See Your service offer at…”**</span></span> <span data-ttu-id="86239-142">amely előfizető hello toodisplay üdvözlő képernyőt fog látni, amikor az ajánlat kerül tooproduction, és DataMarket fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="86239-142">toodisplay hello screen that hello subscriber will see when your offer goes tooproduction and will appear in DataMarket.</span></span>

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="86239-144">Tesztelje, vagy ellenőrizze hello 12 elemeket jelölte meg fent tooensure összes emblémák, árak/tranzakciók, szöveg, képek, dokumentáció, és hivatkozások helyességét, és működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="86239-144">Test or verify each of hello 12 items marked above tooensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="86239-145">Ez az egy időben tooensure teszt értékeket az ajánlat létrehozásakor megadott helyett tényleges értékeket.</span><span class="sxs-lookup"><span data-stu-id="86239-145">This is a good time tooensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="86239-146">Az ajánlat embléma</span><span class="sxs-lookup"><span data-stu-id="86239-146">Offer logo</span></span>
2. <span data-ttu-id="86239-147">Az ajánlat neve</span><span class="sxs-lookup"><span data-stu-id="86239-147">Offer name</span></span>
3. <span data-ttu-id="86239-148">Közzétevő neve/link tooyour vállalati webhely</span><span class="sxs-lookup"><span data-stu-id="86239-148">Publisher name/link tooyour company's website</span></span>
4. <span data-ttu-id="86239-149">Az ajánlat kategóriák keresése</span><span class="sxs-lookup"><span data-stu-id="86239-149">Search categories for your offer</span></span>
5. <span data-ttu-id="86239-150">Az ajánlat támogatási hivatkozás tooassist előfizetők</span><span class="sxs-lookup"><span data-stu-id="86239-150">Your offer's support link tooassist subscribers</span></span>
6. <span data-ttu-id="86239-151">Környezetfüggő leírást az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="86239-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="86239-152">Az ajánlat terv végzett ügyfélellenőrzések számlázási részletek</span><span class="sxs-lookup"><span data-stu-id="86239-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="86239-153">Hivatkozás tooimplementation kódot</span><span class="sxs-lookup"><span data-stu-id="86239-153">Link tooimplementation code</span></span>
9. <span data-ttu-id="86239-154">Minta képek, mely ajánlat adatok használata</span><span class="sxs-lookup"><span data-stu-id="86239-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="86239-155">Bemeneti/kimeneti metaadatok hello ajánlat belül minden szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="86239-155">Input/Output metadata for each service within hello offer</span></span>
11. <span data-ttu-id="86239-156">Ajánlat használati feltételek</span><span class="sxs-lookup"><span data-stu-id="86239-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="86239-157">Hello ajánlat adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="86239-157">Preview of hello offer's data</span></span>

<span data-ttu-id="86239-158">Végül ellenőrizze hello szolgáltatás "MEGISMERKEDHET a DATASET" hello hivatkozásra kattintva hello Datamarket keresztül fog működni.</span><span class="sxs-lookup"><span data-stu-id="86239-158">Finally, check hello service will work through hello Datamarket by clicking hello link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="86239-159">Egy új ablakban nyílik meg hello eszközben közvetlen telepítésnek "Szolgáltatás Explorer", a lekérdezés eredményeinek hello megtekintheti a szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="86239-159">A new window will open in hello tool we call “Service Explorer” so you can preview hello results of a query against your service.</span></span>  <span data-ttu-id="86239-160">Ebben az ablakban hello paraméterek szükséges, és tekintse meg a szolgáltatás egy lekérdezés által megjelenített hello eredményeket is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="86239-160">In this window, you can enter hello parameters needed and see hello results displayed from a query against your service.</span></span>   <span data-ttu-id="86239-161">Megjelenik a, hello URL-címet a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="86239-161">Also, displayed is hello URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="86239-162">Lehet, hogy tooreview hello szöveges leírása hello szolgáltatás hello tetején jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86239-162">Be sure tooreview hello textual description of hello service displayed at hello top.</span></span>  <span data-ttu-id="86239-163">És ha az ajánlatot több szolgáltatás hívja, hello alsó tooswitch toohello tovább szolgáltatás tooreview hello fülek kattintson, és teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="86239-163">And if your offer consists of more than one service call, click hello tabs at hello bottom tooswitch toohello next service tooreview and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="86239-164">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="86239-164">Next step</span></span>
<span data-ttu-id="86239-165">Ha problémát tapasztal, és segítségre van szüksége megoldása kérdéseivel forduljon [Azure közzétevő támogatási csoportjához](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="86239-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="86239-166">Ha elégedett, és készen áll a toopublish ajánlatát olvassa el az hello [vonatkozó kérés jóváhagyása tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="86239-166">If you are satisfied and ready toopublish your offer please read hello [Request Approval tooPush tooProduction](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="86239-167">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="86239-167">See Also</span></span>
* [<span data-ttu-id="86239-168">Első lépések: Hogyan toopublish egy ajánlat toohello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="86239-168">Getting Started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

