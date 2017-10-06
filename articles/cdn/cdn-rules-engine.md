---
title: "hello Azure CDN szabálymotor aaaOverride HTTP viselkedésének |} Microsoft Docs"
description: "hello szabálymotor lehetővé teszi a HTTP-kérések kezelésének módja Azure CDN által például blokkolja a tartalom bizonyos típusú hello kézbesítési toocustomize, a gyorsítótárazási házirend határozza meg és módosíthatja a HTTP-fejlécek."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a><span data-ttu-id="c32fe-103">Bírálja felül a HTTP viselkedés hello Azure CDN szabályok motor használata</span><span class="sxs-lookup"><span data-stu-id="c32fe-103">Override HTTP behavior using hello Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="c32fe-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c32fe-104">Overview</span></span>
<span data-ttu-id="c32fe-105">hello szabálymotor lehetővé teszi a HTTP-kérések kezelésének módja, például tartalom bizonyos típusú hello kézbesítési blokkolja, a gyorsítótárazási házirend meghatározása és a HTTP-fejlécek módosítása toocustomize.</span><span class="sxs-lookup"><span data-stu-id="c32fe-105">hello rules engine allows you toocustomize how HTTP requests are handled, such as blocking hello delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="c32fe-106">Ez az oktatóanyag mutatni szabályt hoz létre a CDN eszközök gyorsítótárazásának hello változik.</span><span class="sxs-lookup"><span data-stu-id="c32fe-106">This tutorial will demonstrate creating a rule that will change hello caching behavior of CDN assets.</span></span>  <span data-ttu-id="c32fe-107">Nincs is videotartalom hello érhető el "[lásd még:](#see-also)" szakasz.</span><span class="sxs-lookup"><span data-stu-id="c32fe-107">There's also video content available in hello "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="c32fe-108">Egy hivatkozási toohello szintaxis részletesen, lásd: [szabályok motor hivatkozás](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="c32fe-108">For a reference toohello syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="c32fe-109">Oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="c32fe-109">Tutorial</span></span>
1. <span data-ttu-id="c32fe-110">A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c32fe-110">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="c32fe-112">Megnyílik a hello CDN felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="c32fe-112">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="c32fe-113">Kattintson a hello **HTTP nagy** fülre, majd **szabálymotor**.</span><span class="sxs-lookup"><span data-stu-id="c32fe-113">Click on hello **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="c32fe-114">Új szabály beállításait jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c32fe-114">Options for a new rule are displayed.</span></span>
   
    ![Új CDN-szabály beállítások](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="c32fe-116">a több szabály listán hello rendelés van hatással, kezelésének módját.</span><span class="sxs-lookup"><span data-stu-id="c32fe-116">hello order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="c32fe-117">Egy későbbi szabály felülbírálhatják az előző szabályok által megadott hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="c32fe-117">A subsequent rule may override hello actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="c32fe-118">Adjon meg egy nevet a hello **neve / leírása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="c32fe-118">Enter a name in hello **Name / Description** textbox.</span></span>
4. <span data-ttu-id="c32fe-119">Azonosítsa a hello típusú kérelmek hello szabály vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c32fe-119">Identify hello type of requests hello rule will apply to.</span></span>  <span data-ttu-id="c32fe-120">Alapértelmezés szerint hello **mindig** egyezés feltétel meg van jelölve.</span><span class="sxs-lookup"><span data-stu-id="c32fe-120">By default, hello **Always** match condition is selected.</span></span>  <span data-ttu-id="c32fe-121">Fogjuk **mindig** ehhez az oktatóanyaghoz, így hagyja kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="c32fe-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![CDN-egyeztetés feltétel](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="c32fe-123">Nincsenek megfelelő számos különböző feltételek hello legördülő érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c32fe-123">There are many types of match conditions available in hello dropdown.</span></span>  <span data-ttu-id="c32fe-124">Kék hello információs ikon toohello kattintva balra hello egyezés feltétel alapján meghatározható, jelenleg kijelölt hello feltétel részletesen.</span><span class="sxs-lookup"><span data-stu-id="c32fe-124">Clicking on hello blue informational icon toohello left of hello match condition will explain hello currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="c32fe-125">Hello teljes listája a feltételes kifejezések részletesen [szabályok motor feltételes kifejezések](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="c32fe-125">For hello full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="c32fe-126">Hello teljes listája megtalálható egyezés feltételek részletesen, [szabályok motor megfelelő feltételek](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="c32fe-126">For hello full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="c32fe-127">Kattintson a hello  **+**  gomb melletti túl**szolgáltatások** tooadd új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="c32fe-127">Click hello **+** button next too**Features** tooadd a new feature.</span></span>  <span data-ttu-id="c32fe-128">Hello legördülő hello bal oldali menüben válassza ki **kényszerített belső maximális életkora**.</span><span class="sxs-lookup"><span data-stu-id="c32fe-128">In hello dropdown on hello left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="c32fe-129">Hello szövegmezőben, amely akkor jelenik meg, írja be a **300**.</span><span class="sxs-lookup"><span data-stu-id="c32fe-129">In hello textbox that appears, enter **300**.</span></span>  <span data-ttu-id="c32fe-130">Alapértelmezett értékek fennmaradó hello hagyja.</span><span class="sxs-lookup"><span data-stu-id="c32fe-130">Leave hello remaining default values.</span></span>
   
   ![CDN-szolgáltatás](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="c32fe-132">Mivel feltételek egyeznek, a kék hello információs ikon toohello kattintva balra hello az új szolgáltatás jelenik meg részleteket ezzel a funkcióval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c32fe-132">As with match conditions, clicking hello blue informational icon toohello left of hello new feature will display details about this feature.</span></span>  <span data-ttu-id="c32fe-133">Hello esetében **kényszerített belső maximális életkora**, azt mérvadóak hello eszköz **Cache-Control** és **Expires** fejlécek toocontrol hello CDN élcsomópont hello fog frissítése után eszköz hello a forrásból.</span><span class="sxs-lookup"><span data-stu-id="c32fe-133">In hello case of **Force Internal Max-Age**, we are overriding hello asset's **Cache-Control** and **Expires** headers toocontrol when hello CDN edge node will refresh hello asset from hello origin.</span></span>  <span data-ttu-id="c32fe-134">300 másodperc példában azt jelenti, hogy hello CDN élcsomópont gyorsítótárazhatják az hello eszköz 5 perccel az eredetéül hello eszköz frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="c32fe-134">Our example of 300 seconds means hello CDN edge node will cache hello asset for 5 minutes before refreshing hello asset from its origin.</span></span>
   > 
   > <span data-ttu-id="c32fe-135">A szolgáltatások részletes hello teljes listáját, lásd: [szabályok motor a szolgáltatás részletei](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="c32fe-135">For hello full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="c32fe-136">Kattintson a hello **Hozzáadás** gomb toosave hello új szabályt.</span><span class="sxs-lookup"><span data-stu-id="c32fe-136">Click hello **Add** button toosave hello new rule.</span></span>  <span data-ttu-id="c32fe-137">Új szabály hello most jóváhagyására vár.</span><span class="sxs-lookup"><span data-stu-id="c32fe-137">hello new rule is now awaiting approval.</span></span> <span data-ttu-id="c32fe-138">Amennyiben jóváhagyták, hello állapota változik **függőben lévő XML** túl**aktív XML**.</span><span class="sxs-lookup"><span data-stu-id="c32fe-138">Once it has been approved, hello status will change from **Pending XML** too**Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="c32fe-139">Szabályok módosítások too90 perc toopropagate hello CDN keresztül is tarthat.</span><span class="sxs-lookup"><span data-stu-id="c32fe-139">Rules changes may take up too90 minutes toopropagate through hello CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="c32fe-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c32fe-140">See also</span></span>
* [<span data-ttu-id="c32fe-141">Az Azure CDN áttekintése</span><span class="sxs-lookup"><span data-stu-id="c32fe-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="c32fe-142">Szabályok motor referencia</span><span class="sxs-lookup"><span data-stu-id="c32fe-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="c32fe-143">Szabályok motor egyezés feltételek</span><span class="sxs-lookup"><span data-stu-id="c32fe-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="c32fe-144">Szabályok motor feltételes kifejezések</span><span class="sxs-lookup"><span data-stu-id="c32fe-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="c32fe-145">Szabályok adatbázismotor-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="c32fe-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="c32fe-146">Alapértelmezett HTTP működés használata hello szabályok felülbírálása</span><span class="sxs-lookup"><span data-stu-id="c32fe-146">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="c32fe-147">[Az Azure péntekenként: Az Azure CDN új Premium szolgáltatással](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (videó)</span><span class="sxs-lookup"><span data-stu-id="c32fe-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>