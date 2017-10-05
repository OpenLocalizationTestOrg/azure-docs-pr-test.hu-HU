---
title: "Egy szolgáltatás számára a piactér létrehozását bemutató útmutatónak |} Microsoft Docs"
description: "Részletes utasításokra van szüksége, hogy hogyan hozhatja létre, hitelesíthet és az adatok szolgáltatás telepítése az Azure piactéren vásárolhat."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a><span data-ttu-id="91775-103">Útmutató az Azure piactér a közzétételi szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="91775-103">Data Service Publishing Guide for the Azure Marketplace</span></span>
> [!IMPORTANT]
> <span data-ttu-id="91775-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="91775-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="91775-105">Ha egy SaaS-üzleti AppSource a közzétenni kívánt alkalmazás további információt talál [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="91775-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="91775-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatás szeretne közzétenni az Azure piactérről, további információt talál [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="91775-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="91775-107">Az 1. lépésben befejezése után [fiók létrehozása és nyilvántartási](marketplace-publishing-accounts-creation-registration.md), interaktív azt le a a [általános jellegű](marketplace-publishing-pre-requisites.md) és [műszaki követelményeiben](marketplace-publishing-data-service-creation-prerequisites.md) egy adatszolgáltatás ajánlat Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="91775-107">After completing the step 1, [Account Creation and Registration](marketplace-publishing-accounts-creation-registration.md), we guided you through the [General Non-Technical](marketplace-publishing-pre-requisites.md) and [Technical Requirements](marketplace-publishing-data-service-creation-prerequisites.md) of a Data Service offer on Azure Marketplace.</span></span> <span data-ttu-id="91775-108">Most végigvezetjük meg a olyan adatszolgáltatás ajánlat létrehozásának a lépései a [közzétételi Portáljára] [ link-pubportal] az Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="91775-108">Now we will walk you through the steps for creating a Data Service offer on the [Publishing Portal][link-pubportal] for the Azure Marketplace.</span></span>

## <a name="1----login-to-the-publishing-portal"></a><span data-ttu-id="91775-109">1.    Jelentkezzen be a közzétételi portálon.</span><span class="sxs-lookup"><span data-stu-id="91775-109">1.    Login to the Publishing Portal.</span></span>
<span data-ttu-id="91775-110">Ugrás a [https://publish.windowsazure.com](https://publish.windowsazure.com.)</span><span class="sxs-lookup"><span data-stu-id="91775-110">Go to [https://publish.windowsazure.com](https://publish.windowsazure.com.)</span></span>

<span data-ttu-id="91775-111">**Közzétételi Portáljára bejelentkezni első alkalommal használja ugyanazt a fiókot, amellyel a vállalati értékesítő profil sikeresen regisztrálva lett fejlesztői központban.**</span><span class="sxs-lookup"><span data-stu-id="91775-111">**For first time login to Publishing Portal, use the same account with which your company’s Seller Profile was registered in Developer Center.**</span></span>  <span data-ttu-id="91775-112">(Később is hozzáadhat a vállalat bármely alkalmazott a közzétételi portálon co-rendszergazdaként).</span><span class="sxs-lookup"><span data-stu-id="91775-112">(Later you can add any employee of your company as a co-admin in the Publishing Portal).</span></span>

<span data-ttu-id="91775-113">Kattintson a **közzététele egy Data Services** Ha ez az első bejelentkezés a közzétételi portált csempéje.</span><span class="sxs-lookup"><span data-stu-id="91775-113">Click on the **Publish a Data Services** tile if this is the first login into the publishing portal.</span></span>

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a><span data-ttu-id="91775-114">2.    Válasszon **adatszolgáltatások** a bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="91775-114">2.    Choose **Data Services** in the navigation menu on the left side.</span></span>
  ![rajz](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a><span data-ttu-id="91775-116">3.    Egy új szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="91775-116">3.    Create a New Data Service</span></span>
<span data-ttu-id="91775-117">Töltse ki a cím az új adatok szolgáltatás kínálnak, és kattintson a "+" jobb.</span><span class="sxs-lookup"><span data-stu-id="91775-117">Fill in the title for your new Data Service Offer and click on “+” on the right.</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a><span data-ttu-id="91775-119">4.    Tekintse át az almenü az újonnan létrehozott szolgáltatás a navigációs menü alatti.</span><span class="sxs-lookup"><span data-stu-id="91775-119">4.    Review the sub-menu under the newly created Data Service in the navigation menu.</span></span>
<span data-ttu-id="91775-120">Kattintson a **forgatókönyv** lapra, és tekintse át az összes szükséges lépéseken, amelyekkel megfelelően közzététele az Azure piactéren adatszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="91775-120">Click on the **Walkthrough** tab and review all necessary steps needed to publish properly the Data Service on the Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="91775-121">Mindig hivatkozásokra a "Forgatókönyv" lapon vagy az adatszolgáltatás ajánlat almenü bal oldalán a lapok használatának.</span><span class="sxs-lookup"><span data-stu-id="91775-121">You always can click on the links in the “Walkthrough” page or use tabs on the Data Service offer’s sub-menu on the left side.</span></span>
> 
> 

## <a name="5----create-a-new-plan"></a><span data-ttu-id="91775-122">5.    Hozzon létre egy új tervet.</span><span class="sxs-lookup"><span data-stu-id="91775-122">5.    Create a new Plan.</span></span>
### <a name="offers-plans-transactions"></a><span data-ttu-id="91775-123">Ajánlatokat, tervek, tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="91775-123">Offers, Plans, transactions.</span></span>
<span data-ttu-id="91775-124">Minden egyes ajánlat rendelkezhet több terveket, de rendelkeznie kell legalább egy (1) terv.</span><span class="sxs-lookup"><span data-stu-id="91775-124">Each Offer can have multiple Plans, but must have at least one (1) Plan.</span></span> <span data-ttu-id="91775-125">Amikor a végfelhasználók előfizetni ajánlatát előfizet az ajánlat terv egyike.</span><span class="sxs-lookup"><span data-stu-id="91775-125">When end-users subscribe to your offer they subscribe for one of the offer’s Plan.</span></span> <span data-ttu-id="91775-126">Minden csomag határozza meg, hogyan fogja tudni a szolgáltatás használata a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="91775-126">Each plan defines how end-users will be able to use your service.</span></span>

<span data-ttu-id="91775-127">Azure Piactér jelenleg csak a havi előfizetési tranzakció alapú modellt támogatják adatszolgáltatások, azaz a végfelhasználók havi díj alapján a meghatározott csomag azokat az előfizetett ára fizet, és képes a tranzakció határozzák meg a terv minden hónap száma lesz.</span><span class="sxs-lookup"><span data-stu-id="91775-127">Currently Azure Marketplace support only Monthly Subscription Transaction Based model for Data Services, i.e. end-users will pay monthly fee according to the price of the specific plan they subscribed to and will be able to consume each month number of transaction defined by the plan.</span></span>

<span data-ttu-id="91775-128">Minden tranzakció általában definiálva az adatszolgáltatás ad vissza rekordok száma, a szolgáltatásnak küldött lekérdezés alapján.</span><span class="sxs-lookup"><span data-stu-id="91775-128">Each Transaction usually defined as number of records your Data Service will return based on the query sent to the Service.</span></span> <span data-ttu-id="91775-129">Az alapértelmezett érték 100.</span><span class="sxs-lookup"><span data-stu-id="91775-129">The default is 100.</span></span> <span data-ttu-id="91775-130">Minden egyes lekérdezés vissza tranzakciók száma szám lesz a rekordok 100 és a legközelebbi egész számra kerekítve.</span><span class="sxs-lookup"><span data-stu-id="91775-130">Number of transactions returned to each query will be number of records divided by 100 and rounded up to the closest integer.</span></span>

<span data-ttu-id="91775-131">Feladata Azure piactér szolgáltatási réteg (mérő) száma minden egyes lekérdezés által felhasznált tranzakciók figyelésére.</span><span class="sxs-lookup"><span data-stu-id="91775-131">It’s Azure Marketplace Service layer responsibility to monitor (meter) number of transactions consumed by each query.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91775-132">Végfelhasználók számára, amely tranzakció elérte a hónap során le lesz tiltva, továbbra is használja a szolgáltatás a havi előfizetési ciklus végéig.</span><span class="sxs-lookup"><span data-stu-id="91775-132">End-Users which reached the transaction limit during the month will be blocked from continuing to use the service until end of their monthly subscription cycle.</span></span>
> 
> <span data-ttu-id="91775-133">A szolgáltatáscsomag vagy a tervek egyikét is (de nem kell) tranzakciók korlátlan számú tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="91775-133">The plan or one of the plans can (but not must) include unlimited number of transactions.</span></span>
> 
> 

### <a name="create-a-plan"></a><span data-ttu-id="91775-134">Hozzon létre egy csomagot.</span><span class="sxs-lookup"><span data-stu-id="91775-134">Create a plan.</span></span>
1. <span data-ttu-id="91775-135">Kattintson a **"+"** mellett a "hozzáadása egy új tervet".</span><span class="sxs-lookup"><span data-stu-id="91775-135">Click on **“+”** next to the “Add a new plan”.</span></span>
2. <span data-ttu-id="91775-136">A lehetőségek közül választhat: **korlátlan** vagy **korlátozott** a terv használata.</span><span class="sxs-lookup"><span data-stu-id="91775-136">Choose one of the options: **Unlimited** or **Limited** usage for this plan.</span></span>  <span data-ttu-id="91775-137">Ha korlátozott majd biztosítanak tranzakciók száma a terv lehetővé teszi egy hónap felhasználását.</span><span class="sxs-lookup"><span data-stu-id="91775-137">If Limited then provide the number of transaction the plan will allow to consume in a month.</span></span>
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    <span data-ttu-id="91775-139">Portál közzétételi is javasolhatnak "Terv azonosítója", amely a felhasználói felületen a terv neve a végfelhasználók számára kommunikációhoz használt, és a terv azonosításához a piactéren szolgáltatás által is használt.</span><span class="sxs-lookup"><span data-stu-id="91775-139">Publishing Portal will also suggest “Plan Identifier”, which will be used to communicate to the end-users the name of the plan in the UI and also used by the Market Place Service to identify the Plan.</span></span> <span data-ttu-id="91775-140">Ha azt szeretné, a "terv azonosítója" módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="91775-140">You can change the “Plan Identifier” if you want.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="91775-141">A "terv azonosítója" minden ajánlat hatókörén belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="91775-141">The “Plan Identifier” must be unique within the scope of each offer.</span></span> <span data-ttu-id="91775-142">Mivel sok egyéb-azonosítók a közzétételi Portal megtervezése azonosító szerepel az első közzététel az éles, és nem lesz képes ennek az azonosítónak módosítása után lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="91775-142">As many other Identifiers used in the Publishing Portal Plan identifier will be locked after the first publishing to production and you will not be able to change this identifier.</span></span>
   > 
   > 
3. <span data-ttu-id="91775-143">A kiválasztott elfogadásához kattintson.</span><span class="sxs-lookup"><span data-stu-id="91775-143">Click to accept your choice.</span></span>
4. <span data-ttu-id="91775-144">Majd kérni fogja néhány további kérdést az újonnan létrehozott terv kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="91775-144">Then you will be asked few additional questions regarding your newly created Plan.</span></span>
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.2.png)

| <span data-ttu-id="91775-146">Kérdés</span><span class="sxs-lookup"><span data-stu-id="91775-146">Question</span></span> | <span data-ttu-id="91775-147">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="91775-147">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="91775-148">**Ezt a tervet az ingyenes és rendelkezésre álló világszerte?**</span><span class="sxs-lookup"><span data-stu-id="91775-148">**This Plan is free and available world-wide?**</span></span> |<span data-ttu-id="91775-149">Egy teljesen ingyenes-az-kell fizetni tervet is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="91775-149">You can create a completely free-of-charge plan.</span></span> <span data-ttu-id="91775-150">Ha ez az ajánlat – kizárólag tervezése az azt jelenti, hogy közzétételekor "Szabad kínálnak" a piactéren.</span><span class="sxs-lookup"><span data-stu-id="91775-150">If it’s the only plan for this offer – it means that you are publishing “Free Offer” in the Marketplace.</span></span> <span data-ttu-id="91775-151">Ha csak az egyik (kevés) terv, az informatikai biztosít lehetőséget nyújtanak további információt a szolgáltatás egy viszonylag kis mennyiségű tranzakciók havonkénti végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="91775-151">If it’s only for one (of few) Plan, the it gives you an option to offer end-users to learn more about your service with a relatively small number of transactions per month.</span></span>  <span data-ttu-id="91775-152">Ha a válasz "Yes", akkor nincs további kérdéseket kell adnia.</span><span class="sxs-lookup"><span data-stu-id="91775-152">If the answer is "Yes," then no further questions will be asked.</span></span> |

> [!NOTE]
> <span data-ttu-id="91775-153">A végfelhasználók a fizetős csomagokban mindig verzióra frissítené.</span><span class="sxs-lookup"><span data-stu-id="91775-153">End users can always upgrade to the paid plans.</span></span>
> 
> 

| <span data-ttu-id="91775-154">Kérdés</span><span class="sxs-lookup"><span data-stu-id="91775-154">Question</span></span> | <span data-ttu-id="91775-155">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="91775-155">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="91775-156">**Ingyenes próbaverzió érhető el?**</span><span class="sxs-lookup"><span data-stu-id="91775-156">**Is free trial available?**</span></span> |<span data-ttu-id="91775-157">"Nem próbaverzió" minden választhat, vagy adjon meg egy lehetőség, hogy a terv "Hónapig" használata.</span><span class="sxs-lookup"><span data-stu-id="91775-157">You can choose between “No Trial” at all or give an option to use your Plan for “One Month”.</span></span> <span data-ttu-id="91775-158">Közzétevők, például a beállítás használatához az ingyenes egy hónapos tartozó ajánlat előnyeinek megismerése lehetőséget nyújt a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="91775-158">Publishers like to use this option to provide end-users the possibility to understand the benefits of the offer for free for one month.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="91775-159">Végfelhasználók csak akkor tudja egy ingyenes próbaverzióra vásárolni, ha az általuk létrehozott fizetési eszközt pl. hitelkártya, nagyvállalati szerződésben.</span><span class="sxs-lookup"><span data-stu-id="91775-159">End-users will only be able to purchase a free trial if they have established payment instrument e.g. credit card, enterprise agreement.</span></span>
> 
> <span data-ttu-id="91775-160">Az ingyenes próbaverzióval az egy hónap után Azure piactér indul el díjszabási ügyfelek az ár, az előfizetés napjától kivéve, ha az ügyfél által kezdeményezett előfizetés megszakítását.</span><span class="sxs-lookup"><span data-stu-id="91775-160">After one month of the free trial, Azure Marketplace will start charging customers the price as of the date of the subscription, unless the customer initiated the subscription cancellation.</span></span> <span data-ttu-id="91775-161">Nincsenek különleges értesítés a végfelhasználók számára biztosított.</span><span class="sxs-lookup"><span data-stu-id="91775-161">No special notification will be provided to the end-users.</span></span>
> 
> 

| <span data-ttu-id="91775-162">Kérdés</span><span class="sxs-lookup"><span data-stu-id="91775-162">Question</span></span> | <span data-ttu-id="91775-163">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="91775-163">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="91775-164">**A terv szükséges beszerzési egy promóciós kódot?**</span><span class="sxs-lookup"><span data-stu-id="91775-164">**This plan requires a promotion code to purchase?**</span></span> |<span data-ttu-id="91775-165">Közzétevők van korlátozhatják a Service-csomagok révén egy különleges kódot, "A Promocode" nevű adott ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="91775-165">Publishers have an option to limit access to their Service Plans by providing a special code, called “A Promocode” to specific customers.</span></span> <span data-ttu-id="91775-166">Csak a Promocode amelyekről végfelhasználók fog tudni a terv előfizetni.</span><span class="sxs-lookup"><span data-stu-id="91775-166">Only end-users which will have this Promocode will be able to subscribe to the Plan.</span></span> <span data-ttu-id="91775-167">Ha úgy dönt, hogy a "nem", akkor beleegyezik abba, hogy mindenki a régióban, amennyiben az ajánlat nem érhető el (lásd: [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) további részletekért) fogja tudni a terv előfizetni.</span><span class="sxs-lookup"><span data-stu-id="91775-167">If you choose “No”, then you agree that everyone from the region where the offer is available (See [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) for more details) will be able to subscribe to this plan.</span></span> <span data-ttu-id="91775-168">Nincsenek további kérdések meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="91775-168">No further questions will be asked.</span></span> |
| <span data-ttu-id="91775-169">**A terv a bárki, aki nem rendelkezik érvényes promóciós kódot is elrejthetik?**</span><span class="sxs-lookup"><span data-stu-id="91775-169">**Also hide this plan from anyone who doesn’t have a valid promotion code?**</span></span> |<span data-ttu-id="91775-170">Ha az előző kérdésre adott válasz "Yes" a közzétevő rendelkezik egy lehetőség, hogy teljesen távolítsa el a terv jelennek meg a piactér felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="91775-170">If the answer to the previous question is “Yes” the Publisher has an option to completely remove this plan from appearing in the UI of the Marketplace.</span></span> <span data-ttu-id="91775-171">Azt jelenti, ügyfelek nem fogják látni az ajánlat részleteit megjelenítő oldalon a tervet.</span><span class="sxs-lookup"><span data-stu-id="91775-171">It means, customers will not see this plan in the Offer’s details page.</span></span> <span data-ttu-id="91775-172">Végfelhasználók számára, amely megkapja a promocode, azok megvásárlását fog tudni arra a promocode használatával.</span><span class="sxs-lookup"><span data-stu-id="91775-172">End-users which will receive a promocode to purchase it, will be able to subscribe to it using this promocode.</span></span> |

## <a name="6----create-your-marketplace-marketing-content"></a><span data-ttu-id="91775-173">6.    A tartalom marketing piactér létrehozása</span><span class="sxs-lookup"><span data-stu-id="91775-173">6.    Create your Marketplace marketing content</span></span>
<span data-ttu-id="91775-174">Adja meg a szükséges információkat arról, hogyan **Marketing, árazás, támogatást és kategóriák** lapok látogasson el a [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) vagyis minden összetevőihez közzé az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="91775-174">For How to provide information required in **Marketing, Pricing, Support and Categories** tabs please visit [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) which is common to all artifacts published in the Azure Marketplace.</span></span>  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a><span data-ttu-id="91775-175">7.    Az ajánlat kapcsolódik a szolgáltatáshoz (SQL Azure-alapú vagy webszolgáltatás-alapú).</span><span class="sxs-lookup"><span data-stu-id="91775-175">7.    Connect your offer to your Service (SQL Azure based or Web Service based).</span></span>
<span data-ttu-id="91775-176">Kattintson a **adatszolgáltatások** almenü.</span><span class="sxs-lookup"><span data-stu-id="91775-176">Click on the **Data Services** sub-menu.</span></span>

<span data-ttu-id="91775-177">A lap felső részén a meg kell adnia annak az ajánlat **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="91775-177">On the upper half of the page you’ll be asked to provide the offer’s **Namespace**.</span></span>  

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.png)

<span data-ttu-id="91775-179">Az alábbi kérdés fog elláthat hogyan a közzétevő teszi közzé az újonnan létrehozott ajánlat az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="91775-179">The below question will define how the Publisher is going to expose newly created offer to Azure Marketplace.</span></span> <span data-ttu-id="91775-180">(További információ: a [Data Services műszaki előfeltétel útmutató](marketplace-publishing-data-service-creation-prerequisites.md)).</span><span class="sxs-lookup"><span data-stu-id="91775-180">(For more details see the [Data Services Technical Prerequisite Guide](marketplace-publishing-data-service-creation-prerequisites.md)).</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.2.png)

<span data-ttu-id="91775-182">**Közzététel az adatbázis-alapú szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="91775-182">**Publishing the Database based service**</span></span>

<span data-ttu-id="91775-183">Kattintson a **adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="91775-183">Click on **Database**.</span></span> <span data-ttu-id="91775-184">A következő lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="91775-184">The following page will appear:</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.3.png)

<span data-ttu-id="91775-186">Egy CSDL-hozzárendelés létrehozása az SQL Azure Database alapuló adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="91775-186">To create a CSDL mapping for the Dataset based on the SQL Azure DB:</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.4.png)

<span data-ttu-id="91775-188">Majd minden táblához</span><span class="sxs-lookup"><span data-stu-id="91775-188">And then for each table</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.6.png)

<span data-ttu-id="91775-191">Ha a webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="91775-191">If Web Service</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> <span data-ttu-id="91775-193">Olvasási [leképezése egy meglévő webes CSDL keresztül OData szolgáltatás](marketplace-publishing-data-service-creation-odata-mapping.md) részletes útmutatásért és példákért CSDL webes szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="91775-193">Read [Mapping an existing web service to OData through CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) for detailed instructions and examples for creating a CSDL Web Service.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="91775-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91775-194">Next Steps</span></span>
<span data-ttu-id="91775-195">Most, hogy létrehozta az adatszolgáltatás ajánlatot, győződjön meg arról, hogy elvégezte-e a utasításait a [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) előtt előre [tesztelése a szolgáltatás átmeneti](marketplace-publishing-data-service-test-in-staging.md).</span><span class="sxs-lookup"><span data-stu-id="91775-195">Now that you've created your Data Service offer, please ensure that you complete the instructions in the [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) before you move forward to [Testing your Data Service in Staging](marketplace-publishing-data-service-test-in-staging.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="91775-196">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="91775-196">See Also</span></span>
* [<span data-ttu-id="91775-197">Első lépések: Hogyan ajánlat közzététele az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="91775-197">Getting Started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* <span data-ttu-id="91775-198">Ha a teljes OData-megfeleltetési folyamat és a rendeltetés megválaszolásával, olvassa el ebben a cikkben [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) áttekintheti a definíciókat, és a utasításokat.</span><span class="sxs-lookup"><span data-stu-id="91775-198">If you are interested in understanding the overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) to review definitions, structures, and instructions.</span></span>
* <span data-ttu-id="91775-199">Ha érdekli tanulási és az adott csomópont, és a paraméterek ismertetése, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="91775-199">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="91775-200">Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) mintakód és kód szintaxis és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="91775-200">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) to see sample code and understand code syntax and context.</span></span>

[link-pubportal]:https://publish.windowsazure.com
