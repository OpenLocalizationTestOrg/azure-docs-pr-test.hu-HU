---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
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
ms.openlocfilehash: 0220d357ae0ec89e7d4f6399605850e57c646f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-service-publishing-guide-for-hello-azure-marketplace"></a><span data-ttu-id="9a240-103">Adatok szolgáltatás közzétételi útmutatója hello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="9a240-103">Data Service Publishing Guide for hello Azure Marketplace</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9a240-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="9a240-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="9a240-105">Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="9a240-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="9a240-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="9a240-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="9a240-107">1. lépésben hello befejezése után [fiók létrehozása és regisztrálása](marketplace-publishing-accounts-creation-registration.md), azt végigvezeti meg hello [általános jellegű](marketplace-publishing-pre-requisites.md) és [műszaki követelményeiben](marketplace-publishing-data-service-creation-prerequisites.md) adatok szolgáltatás ajánlat Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="9a240-107">After completing hello step 1, [Account Creation and Registration](marketplace-publishing-accounts-creation-registration.md), we guided you through hello [General Non-Technical](marketplace-publishing-pre-requisites.md) and [Technical Requirements](marketplace-publishing-data-service-creation-prerequisites.md) of a Data Service offer on Azure Marketplace.</span></span> <span data-ttu-id="9a240-108">Most végigvezetjük meg olyan adatszolgáltatás ajánlat létrehozásakor hello hello lépései [közzétételi Portáljára] [ link-pubportal] a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="9a240-108">Now we will walk you through hello steps for creating a Data Service offer on hello [Publishing Portal][link-pubportal] for hello Azure Marketplace.</span></span>

## <a name="1----login-toohello-publishing-portal"></a><span data-ttu-id="9a240-109">1.    Bejelentkezési toohello közzétételi Portáljára.</span><span class="sxs-lookup"><span data-stu-id="9a240-109">1.    Login toohello Publishing Portal.</span></span>
<span data-ttu-id="9a240-110">Nyissa meg túl[https://publish.windowsazure.com](https://publish.windowsazure.com.)</span><span class="sxs-lookup"><span data-stu-id="9a240-110">Go too[https://publish.windowsazure.com](https://publish.windowsazure.com.)</span></span>

<span data-ttu-id="9a240-111">**Az első alkalommal bejelentkezési tooPublishing Portal használja ugyanazt a fiókot, amellyel a vállalati értékesítő profil sikeresen regisztrálva lett fejlesztői központ hello.**</span><span class="sxs-lookup"><span data-stu-id="9a240-111">**For first time login tooPublishing Portal, use hello same account with which your company’s Seller Profile was registered in Developer Center.**</span></span>  <span data-ttu-id="9a240-112">(Később is hozzáadhat a vállalat bármely alkalmazott hello közzétételi Portáljára lévő co-rendszergazdaként).</span><span class="sxs-lookup"><span data-stu-id="9a240-112">(Later you can add any employee of your company as a co-admin in hello Publishing Portal).</span></span>

<span data-ttu-id="9a240-113">Kattintson a hello **közzététele egy Data Services** Ha ez az első bejelentkezés hello hello közzétételi portált csempéje.</span><span class="sxs-lookup"><span data-stu-id="9a240-113">Click on hello **Publish a Data Services** tile if this is hello first login into hello publishing portal.</span></span>

## <a name="2----choose-data-services-in-hello-navigation-menu-on-hello-left-side"></a><span data-ttu-id="9a240-114">2.    Válasszon **adatszolgáltatások** hello hello bal oldali navigációs menüjében található.</span><span class="sxs-lookup"><span data-stu-id="9a240-114">2.    Choose **Data Services** in hello navigation menu on hello left side.</span></span>
  ![rajz](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a><span data-ttu-id="9a240-116">3.    Egy új szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a240-116">3.    Create a New Data Service</span></span>
<span data-ttu-id="9a240-117">Töltse ki az új adatok szolgáltatás kínálnak a hello cím, és kattintson a "+" a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="9a240-117">Fill in hello title for your new Data Service Offer and click on “+” on hello right.</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-hello-sub-menu-under-hello-newly-created-data-service-in-hello-navigation-menu"></a><span data-ttu-id="9a240-119">4.    Felülvizsgálati hello almenü hello az újonnan létrehozott adatszolgáltatás hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="9a240-119">4.    Review hello sub-menu under hello newly created Data Service in hello navigation menu.</span></span>
<span data-ttu-id="9a240-120">Kattintson a hello **forgatókönyv** lapra, és tekintse át az összes szükséges lépéseket szükséges toopublish megfelelően hello szolgáltatás a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="9a240-120">Click on hello **Walkthrough** tab and review all necessary steps needed toopublish properly hello Data Service on hello Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="9a240-121">Mindig hello hivatkozások hello "Forgatókönyv" lapon kattintson vagy hello adatszolgáltatás ajánlat almenü bal oldalán hello a lapok használatának.</span><span class="sxs-lookup"><span data-stu-id="9a240-121">You always can click on hello links in hello “Walkthrough” page or use tabs on hello Data Service offer’s sub-menu on hello left side.</span></span>
> 
> 

## <a name="5----create-a-new-plan"></a><span data-ttu-id="9a240-122">5.    Hozzon létre egy új tervet.</span><span class="sxs-lookup"><span data-stu-id="9a240-122">5.    Create a new Plan.</span></span>
### <a name="offers-plans-transactions"></a><span data-ttu-id="9a240-123">Ajánlatokat, tervek, tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="9a240-123">Offers, Plans, transactions.</span></span>
<span data-ttu-id="9a240-124">Minden egyes ajánlat rendelkezhet több terveket, de rendelkeznie kell legalább egy (1) terv.</span><span class="sxs-lookup"><span data-stu-id="9a240-124">Each Offer can have multiple Plans, but must have at least one (1) Plan.</span></span> <span data-ttu-id="9a240-125">Amikor a végfelhasználók előfizetés tooyour ajánlat előfizet egy hello ajánlat terv.</span><span class="sxs-lookup"><span data-stu-id="9a240-125">When end-users subscribe tooyour offer they subscribe for one of hello offer’s Plan.</span></span> <span data-ttu-id="9a240-126">Minden csomag határozza meg, hogyan történik a végfelhasználók képes toouse lehet a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9a240-126">Each plan defines how end-users will be able toouse your service.</span></span>

<span data-ttu-id="9a240-127">Jelenleg Azure piactéren csak a havi előfizetési tranzakció alapú modell támogatása adatszolgáltatások, azaz végfelhasználók hello meghatározott csomag azokat az előfizetett tooand toohello ára szerint havi díj fizet lesz képes tooconsume minden hónap száma a tranzakció hello terv által definiált.</span><span class="sxs-lookup"><span data-stu-id="9a240-127">Currently Azure Marketplace support only Monthly Subscription Transaction Based model for Data Services, i.e. end-users will pay monthly fee according toohello price of hello specific plan they subscribed tooand will be able tooconsume each month number of transaction defined by hello plan.</span></span>

<span data-ttu-id="9a240-128">Minden tranzakció általában definiálva, a szolgáltatás ad vissza rekordok száma a szolgáltatás toohello küldött hello lekérdezés alapján.</span><span class="sxs-lookup"><span data-stu-id="9a240-128">Each Transaction usually defined as number of records your Data Service will return based on hello query sent toohello Service.</span></span> <span data-ttu-id="9a240-129">hello alapértelmezett érték 100.</span><span class="sxs-lookup"><span data-stu-id="9a240-129">hello default is 100.</span></span> <span data-ttu-id="9a240-130">A tranzakciók számának visszaadott tooeach megpróbálkozik a bejegyzések száma osztva 100 és toohello legközelebbi egész számra kerekítve.</span><span class="sxs-lookup"><span data-stu-id="9a240-130">Number of transactions returned tooeach query will be number of records divided by 100 and rounded up toohello closest integer.</span></span>

<span data-ttu-id="9a240-131">Azure piactér szolgáltatás réteg felelősségi toomonitor (mérő) tranzakciók száma minden egyes lekérdezés által felhasznált.</span><span class="sxs-lookup"><span data-stu-id="9a240-131">It’s Azure Marketplace Service layer responsibility toomonitor (meter) number of transactions consumed by each query.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9a240-132">Végfelhasználók számára, amely elérte hello tranzakció hello hónap során le lesz tiltva, a havi előfizetési ciklus végéig toouse hello szolgáltatás folytatása.</span><span class="sxs-lookup"><span data-stu-id="9a240-132">End-Users which reached hello transaction limit during hello month will be blocked from continuing toouse hello service until end of their monthly subscription cycle.</span></span>
> 
> <span data-ttu-id="9a240-133">hello terv vagy hello csomagok valamelyikének is (de nem kell) korlátlan számú tranzakciók közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="9a240-133">hello plan or one of hello plans can (but not must) include unlimited number of transactions.</span></span>
> 
> 

### <a name="create-a-plan"></a><span data-ttu-id="9a240-134">Hozzon létre egy csomagot.</span><span class="sxs-lookup"><span data-stu-id="9a240-134">Create a plan.</span></span>
1. <span data-ttu-id="9a240-135">Kattintson a **"+"** következő toohello "hozzáadása egy új tervet".</span><span class="sxs-lookup"><span data-stu-id="9a240-135">Click on **“+”** next toohello “Add a new plan”.</span></span>
2. <span data-ttu-id="9a240-136">Hello lehetőségek közül választhat: **korlátlan** vagy **korlátozott** a terv használata.</span><span class="sxs-lookup"><span data-stu-id="9a240-136">Choose one of hello options: **Unlimited** or **Limited** usage for this plan.</span></span>  <span data-ttu-id="9a240-137">Ha korlátozott adja meg a tranzakció hello terv hello száma a hónap tooconsume lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="9a240-137">If Limited then provide hello number of transaction hello plan will allow tooconsume in a month.</span></span>
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    <span data-ttu-id="9a240-139">Portál közzétételi is javaslatot tesz "Terv azonosítója", amely lesz használt toocommunicate toohello végfelhasználók hello hello terv a felhasználói felület hello neve és a hello piactéren szolgáltatás tooidentify hello terv által is használt.</span><span class="sxs-lookup"><span data-stu-id="9a240-139">Publishing Portal will also suggest “Plan Identifier”, which will be used toocommunicate toohello end-users hello name of hello plan in hello UI and also used by hello Market Place Service tooidentify hello Plan.</span></span> <span data-ttu-id="9a240-140">Ha azt szeretné, hello "Terv azonosítója" módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="9a240-140">You can change hello “Plan Identifier” if you want.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9a240-141">hello "Terv azonosítója" minden ajánlat hello hatókörén belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9a240-141">hello “Plan Identifier” must be unique within hello scope of each offer.</span></span> <span data-ttu-id="9a240-142">Az alkalmazott sok egyéb azonosítókhoz hello közzétételi Portal megtervezése után hello első közzétételi tooproduction, és nem lesz képes toochange azonosítója lesz zárolva. Ez az azonosító.</span><span class="sxs-lookup"><span data-stu-id="9a240-142">As many other Identifiers used in hello Publishing Portal Plan identifier will be locked after hello first publishing tooproduction and you will not be able toochange this identifier.</span></span>
   > 
   > 
3. <span data-ttu-id="9a240-143">Kattintson a tooaccept választását.</span><span class="sxs-lookup"><span data-stu-id="9a240-143">Click tooaccept your choice.</span></span>
4. <span data-ttu-id="9a240-144">Majd kérni fogja néhány további kérdést az újonnan létrehozott terv kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="9a240-144">Then you will be asked few additional questions regarding your newly created Plan.</span></span>
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.2.png)

| <span data-ttu-id="9a240-146">Kérdés</span><span class="sxs-lookup"><span data-stu-id="9a240-146">Question</span></span> | <span data-ttu-id="9a240-147">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="9a240-147">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="9a240-148">**Ezt a tervet az ingyenes és rendelkezésre álló világszerte?**</span><span class="sxs-lookup"><span data-stu-id="9a240-148">**This Plan is free and available world-wide?**</span></span> |<span data-ttu-id="9a240-149">Egy teljesen ingyenes-az-kell fizetni tervet is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="9a240-149">You can create a completely free-of-charge plan.</span></span> <span data-ttu-id="9a240-150">Ha az hello csak tervezése – Ez az ajánlat azt jelenti, hogy közzétételekor "Szabad kínálnak" hello piactér a.</span><span class="sxs-lookup"><span data-stu-id="9a240-150">If it’s hello only plan for this offer – it means that you are publishing “Free Offer” in hello Marketplace.</span></span> <span data-ttu-id="9a240-151">Ha csak az egyik (kevés) terv, hello biztosít egy beállítás toooffer végfelhasználók toolearn további információk a szolgáltatást, amely viszonylag kis számú havonta tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="9a240-151">If it’s only for one (of few) Plan, hello it gives you an option toooffer end-users toolearn more about your service with a relatively small number of transactions per month.</span></span>  <span data-ttu-id="9a240-152">Ha hello válasz "Yes", akkor nincs további kérdéseket kell adnia.</span><span class="sxs-lookup"><span data-stu-id="9a240-152">If hello answer is "Yes," then no further questions will be asked.</span></span> |

> [!NOTE]
> <span data-ttu-id="9a240-153">A végfelhasználók mindig frissítheti a fizetős csomagokban toohello.</span><span class="sxs-lookup"><span data-stu-id="9a240-153">End users can always upgrade toohello paid plans.</span></span>
> 
> 

| <span data-ttu-id="9a240-154">Kérdés</span><span class="sxs-lookup"><span data-stu-id="9a240-154">Question</span></span> | <span data-ttu-id="9a240-155">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="9a240-155">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="9a240-156">**Ingyenes próbaverzió érhető el?**</span><span class="sxs-lookup"><span data-stu-id="9a240-156">**Is free trial available?**</span></span> |<span data-ttu-id="9a240-157">"Nem próbaverzió" minden választhat, vagy adjon egy beállítás toouse a terv "Hónapig".</span><span class="sxs-lookup"><span data-stu-id="9a240-157">You can choose between “No Trial” at all or give an option toouse your Plan for “One Month”.</span></span> <span data-ttu-id="9a240-158">Közzétevők például toouse Ez a beállítás tooprovide végfelhasználók hello lehetőségét toounderstand hello előnyei hello szabad hónapig kínálnak.</span><span class="sxs-lookup"><span data-stu-id="9a240-158">Publishers like toouse this option tooprovide end-users hello possibility toounderstand hello benefits of hello offer for free for one month.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="9a240-159">Végfelhasználók csak akkor tudja toopurchase egy ingyenes próbaverziót, ha az általuk létrehozott fizetési eszközt pl. hitelkártya, nagyvállalati szerződésben.</span><span class="sxs-lookup"><span data-stu-id="9a240-159">End-users will only be able toopurchase a free trial if they have established payment instrument e.g. credit card, enterprise agreement.</span></span>
> 
> <span data-ttu-id="9a240-160">Az ingyenes próbaverzió hello egy hónap után Azure piactér indul el díjszabási ügyfelek hello ár hello dátum hello előfizetés, kivéve, ha hello ügyfél által kezdeményezett hello előfizetés törlése.</span><span class="sxs-lookup"><span data-stu-id="9a240-160">After one month of hello free trial, Azure Marketplace will start charging customers hello price as of hello date of hello subscription, unless hello customer initiated hello subscription cancellation.</span></span> <span data-ttu-id="9a240-161">Nincsenek különleges értesítési toohello végfelhasználók biztosítja.</span><span class="sxs-lookup"><span data-stu-id="9a240-161">No special notification will be provided toohello end-users.</span></span>
> 
> 

| <span data-ttu-id="9a240-162">Kérdés</span><span class="sxs-lookup"><span data-stu-id="9a240-162">Question</span></span> | <span data-ttu-id="9a240-163">Többszörösére.</span><span class="sxs-lookup"><span data-stu-id="9a240-163">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="9a240-164">**Ez a séma szükséges egy promóciós kódot toopurchase?**</span><span class="sxs-lookup"><span data-stu-id="9a240-164">**This plan requires a promotion code toopurchase?**</span></span> |<span data-ttu-id="9a240-165">Közzétevők egy beállítás toolimit hozzáférés tootheir Service-csomagok révén egy különleges kódot, "A Promocode" toospecific ügyfelek nevű rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9a240-165">Publishers have an option toolimit access tootheir Service Plans by providing a special code, called “A Promocode” toospecific customers.</span></span> <span data-ttu-id="9a240-166">Csak azok a végfelhasználók a Promocode amelyekről tudja toosubscribe toohello terv lesz.</span><span class="sxs-lookup"><span data-stu-id="9a240-166">Only end-users which will have this Promocode will be able toosubscribe toohello Plan.</span></span> <span data-ttu-id="9a240-167">Ha úgy dönt, hogy a "nem", akkor beleegyezik abba, hogy rendelkezésre áll-e mindenki hello régióban, ahol hello kínálnak (lásd: [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) további részletekért) képes toosubscribe toothis terv lesz.</span><span class="sxs-lookup"><span data-stu-id="9a240-167">If you choose “No”, then you agree that everyone from hello region where hello offer is available (See [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) for more details) will be able toosubscribe toothis plan.</span></span> <span data-ttu-id="9a240-168">Nincsenek további kérdések meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="9a240-168">No further questions will be asked.</span></span> |
| <span data-ttu-id="9a240-169">**A terv a bárki, aki nem rendelkezik érvényes promóciós kódot is elrejthetik?**</span><span class="sxs-lookup"><span data-stu-id="9a240-169">**Also hide this plan from anyone who doesn’t have a valid promotion code?**</span></span> |<span data-ttu-id="9a240-170">Ha hello válasz toohello előző kérdéssel "Yes" hello Publisher rendelkezik egy beállítás toocompletely, távolítsa el a terv jelennek meg hello hello piactér felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="9a240-170">If hello answer toohello previous question is “Yes” hello Publisher has an option toocompletely remove this plan from appearing in hello UI of hello Marketplace.</span></span> <span data-ttu-id="9a240-171">Azt jelenti, ügyfelek nem fogják látni a terv hello ajánlat részletei lapon.</span><span class="sxs-lookup"><span data-stu-id="9a240-171">It means, customers will not see this plan in hello Offer’s details page.</span></span> <span data-ttu-id="9a240-172">Végfelhasználók számára, amely megkapja a promocode toopurchase, lesz képes toosubscribe tooit a promocode használatával.</span><span class="sxs-lookup"><span data-stu-id="9a240-172">End-users which will receive a promocode toopurchase it, will be able toosubscribe tooit using this promocode.</span></span> |

## <a name="6----create-your-marketplace-marketing-content"></a><span data-ttu-id="9a240-173">6.    A tartalom marketing piactér létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a240-173">6.    Create your Marketplace marketing content</span></span>
<span data-ttu-id="9a240-174">Hogyan tooprovide információ szükséges a **Marketing, árazás, támogatást és kategóriák** lapok látogasson el a [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) Ez az hello közzétett közös tooall összetevők Az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="9a240-174">For How tooprovide information required in **Marketing, Pricing, Support and Categories** tabs please visit [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) which is common tooall artifacts published in hello Azure Marketplace.</span></span>  

## <a name="7----connect-your-offer-tooyour-service-sql-azure-based-or-web-service-based"></a><span data-ttu-id="9a240-175">7.    Csatlakoztassa az ajánlat tooyour (SQL Azure-alapú vagy webszolgáltatás-alapú) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9a240-175">7.    Connect your offer tooyour Service (SQL Azure based or Web Service based).</span></span>
<span data-ttu-id="9a240-176">Kattintson a hello **adatszolgáltatások** almenü.</span><span class="sxs-lookup"><span data-stu-id="9a240-176">Click on hello **Data Services** sub-menu.</span></span>

<span data-ttu-id="9a240-177">A hello lap felső részén hello meg kell adnia tooprovide hello ajánlat **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="9a240-177">On hello upper half of hello page you’ll be asked tooprovide hello offer’s **Namespace**.</span></span>  

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.png)

<span data-ttu-id="9a240-179">alább kérdés hello meghatározzák, hogyan hello Publisher állapotra vált, az újonnan létrehozott tooexpose ajánlat tooAzure piactér.</span><span class="sxs-lookup"><span data-stu-id="9a240-179">hello below question will define how hello Publisher is going tooexpose newly created offer tooAzure Marketplace.</span></span> <span data-ttu-id="9a240-180">(További információ: hello [Data Services műszaki előfeltétel útmutató](marketplace-publishing-data-service-creation-prerequisites.md)).</span><span class="sxs-lookup"><span data-stu-id="9a240-180">(For more details see hello [Data Services Technical Prerequisite Guide](marketplace-publishing-data-service-creation-prerequisites.md)).</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.2.png)

<span data-ttu-id="9a240-182">**Adatbázis alapú közzétételi hello szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="9a240-182">**Publishing hello Database based service**</span></span>

<span data-ttu-id="9a240-183">Kattintson a **adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="9a240-183">Click on **Database**.</span></span> <span data-ttu-id="9a240-184">a következő lap hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9a240-184">hello following page will appear:</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.3.png)

<span data-ttu-id="9a240-186">toocreate hello Dataset CSDL társítás hello SQL Azure Database alapján:</span><span class="sxs-lookup"><span data-stu-id="9a240-186">toocreate a CSDL mapping for hello Dataset based on hello SQL Azure DB:</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.4.png)

<span data-ttu-id="9a240-188">Majd minden táblához</span><span class="sxs-lookup"><span data-stu-id="9a240-188">And then for each table</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.6.png)

<span data-ttu-id="9a240-191">Ha a webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9a240-191">If Web Service</span></span>

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> <span data-ttu-id="9a240-193">Olvasási [leképezése egy meglévő webes szolgáltatás tooOData keresztül CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) részletes útmutatásért és példákért CSDL webes szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9a240-193">Read [Mapping an existing web service tooOData through CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) for detailed instructions and examples for creating a CSDL Web Service.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="9a240-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a240-194">Next Steps</span></span>
<span data-ttu-id="9a240-195">Most, hogy létrehozta az adatszolgáltatás ajánlatot, győződjön meg arról, hogy elvégezte-e hello hello utasításait [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) áthelyezése előtt előre túl[a szolgáltatás átmenetitesztelése](marketplace-publishing-data-service-test-in-staging.md).</span><span class="sxs-lookup"><span data-stu-id="9a240-195">Now that you've created your Data Service offer, please ensure that you complete hello instructions in hello [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) before you move forward too[Testing your Data Service in Staging](marketplace-publishing-data-service-test-in-staging.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="9a240-196">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9a240-196">See Also</span></span>
* [<span data-ttu-id="9a240-197">Első lépések: Hogyan toopublish egy ajánlat toohello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="9a240-197">Getting Started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* <span data-ttu-id="9a240-198">A cikkből megtudhatja, hogyan készítheti ismertetése a hello általános OData-megfeleltetési folyamat és a cél, [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definíciókat, és a utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9a240-198">If you are interested in understanding hello overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitions, structures, and instructions.</span></span>
* <span data-ttu-id="9a240-199">Ha érdekli tanulási és ismertetése hello adott csomópontok és a paraméterek, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="9a240-199">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="9a240-200">Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee példakód és kód szintaxis és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="9a240-200">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee sample code and understand code syntax and context.</span></span>

[link-pubportal]:https://publish.windowsazure.com
