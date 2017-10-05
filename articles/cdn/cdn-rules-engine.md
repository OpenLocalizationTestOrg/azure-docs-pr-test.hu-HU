---
title: "Az Azure CDN szabályok motor használata HTTP működés felülbírálásához |} Microsoft Docs"
description: "A szabályok motor teszi lehetővé testre szabhatja a HTTP-kérések kezelésének módja Azure CDN által például blokkolja-e a tartalom bizonyos típusú kézbesítését, gyorsítótárazási házirend határozza meg és módosíthatja a HTTP-fejlécek."
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
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a><span data-ttu-id="e7ab4-103">Az Azure CDN szabálymotor HTTP viselkedésének felülbírálása</span><span class="sxs-lookup"><span data-stu-id="e7ab4-103">Override HTTP behavior using the Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="e7ab4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e7ab4-104">Overview</span></span>
<span data-ttu-id="e7ab4-105">A szabályok motor lehetővé teszi, hogy segítségével testre szabhatja a HTTP-kérések kezelésének módja, például bizonyos típusú tartalom kézbesítésével blokkolja, a gyorsítótárazási házirend meghatározása és a HTTP-fejlécek módosítása.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-105">The rules engine allows you to customize how HTTP requests are handled, such as blocking the delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="e7ab4-106">Ez az oktatóanyag mutatni szabályt hoz létre a gyorsítótár-viselkedést CDN eszközök változik.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-106">This tutorial will demonstrate creating a rule that will change the caching behavior of CDN assets.</span></span>  <span data-ttu-id="e7ab4-107">Nincs is videotartalom érhető el a "[lásd még:](#see-also)" szakasz.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-107">There's also video content available in the "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="e7ab4-108">Részletes szintaxisa referenciáért lásd: [szabályok motor hivatkozás](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="e7ab4-108">For a reference to the syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="e7ab4-109">Oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="e7ab4-109">Tutorial</span></span>
1. <span data-ttu-id="e7ab4-110">A CDN-profil panelje, kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-110">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="e7ab4-112">Megnyitja a CDN-felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-112">The CDN management portal opens.</span></span>
2. <span data-ttu-id="e7ab4-113">Kattintson a **HTTP nagy** fülre, majd **szabálymotor**.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-113">Click on the **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="e7ab4-114">Új szabály beállításait jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-114">Options for a new rule are displayed.</span></span>
   
    ![Új CDN-szabály beállítások](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="e7ab4-116">A több szabály listán sorrendje befolyásolja kezelésének módját.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-116">The order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="e7ab4-117">A következő szabály felülbírálhatják az előző szabályok által meghatározott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-117">A subsequent rule may override the actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="e7ab4-118">Írjon be egy nevet a **neve / leírása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-118">Enter a name in the **Name / Description** textbox.</span></span>
4. <span data-ttu-id="e7ab4-119">Azonosítja a kéréseket a szabály vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-119">Identify the type of requests the rule will apply to.</span></span>  <span data-ttu-id="e7ab4-120">Alapértelmezés szerint a **mindig** egyezés feltétel meg van jelölve.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-120">By default, the **Always** match condition is selected.</span></span>  <span data-ttu-id="e7ab4-121">Fogjuk **mindig** ehhez az oktatóanyaghoz, így hagyja kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![CDN-egyeztetés feltétel](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="e7ab4-123">Nincsenek számos különböző típusú egyezik meg a legördülő listában elérhető feltételek.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-123">There are many types of match conditions available in the dropdown.</span></span>  <span data-ttu-id="e7ab4-124">A bal oldalra, az egyeztetés feltétel kék információs ikonra kattint alapján meghatározható a jelenleg kijelölt feltételt részletesen.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-124">Clicking on the blue informational icon to the left of the match condition will explain the currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="e7ab4-125">Részletes feltételes kifejezések teljes listáját lásd: [szabályok motor feltételes kifejezések](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="e7ab4-125">For the full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="e7ab4-126">Egyezés feltételek részletesen teljes listáját lásd: [szabályok motor megfelelő feltételek](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="e7ab4-126">For the full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="e7ab4-127">Kattintson a  **+**  gombra **szolgáltatások** hozzáadni egy új funkciót.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-127">Click the **+** button next to **Features** to add a new feature.</span></span>  <span data-ttu-id="e7ab4-128">A bal oldali legördülő menüben válassza ki **kényszerített belső maximális életkora**.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-128">In the dropdown on the left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="e7ab4-129">A szövegmezőben, amely akkor jelenik meg, írja be a **300**.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-129">In the textbox that appears, enter **300**.</span></span>  <span data-ttu-id="e7ab4-130">Hagyja a további alapértelmezett értékei.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-130">Leave the remaining default values.</span></span>
   
   ![CDN-szolgáltatás](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="e7ab4-132">Mint feltételek egyeznek, a bal oldalán a új szolgáltatás a kék információs ikonra kattintva jelenik meg ez a szolgáltatás adatait.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-132">As with match conditions, clicking the blue informational icon to the left of the new feature will display details about this feature.</span></span>  <span data-ttu-id="e7ab4-133">A következőket **kényszerített belső maximális életkora**, azt mérvadóak az eszköz **Cache-Control** és **Expires** fejlécek szabályozására, ha a CDN élcsomópont frissíti az eszköz a forrás.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-133">In the case of **Force Internal Max-Age**, we are overriding the asset's **Cache-Control** and **Expires** headers to control when the CDN edge node will refresh the asset from the origin.</span></span>  <span data-ttu-id="e7ab4-134">300 másodperc példában azt jelenti, hogy a CDN élcsomópont gyorsítótárazhatják az eszköz 5 perccel eredetéül az eszköz frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-134">Our example of 300 seconds means the CDN edge node will cache the asset for 5 minutes before refreshing the asset from its origin.</span></span>
   > 
   > <span data-ttu-id="e7ab4-135">A részletes szolgáltatások teljes listájáért lásd: [szabályok motor a szolgáltatás részletei](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="e7ab4-135">For the full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="e7ab4-136">Kattintson a **Hozzáadás** gombra kattintva mentse az új szabályt.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-136">Click the **Add** button to save the new rule.</span></span>  <span data-ttu-id="e7ab4-137">Az új szabály most jóváhagyására vár.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-137">The new rule is now awaiting approval.</span></span> <span data-ttu-id="e7ab4-138">Miután engedélyezte, az állapot változik **függőben lévő XML** való **aktív XML**.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-138">Once it has been approved, the status will change from **Pending XML** to **Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="e7ab4-139">Szabályok módosítások keresztül a CDN propagálása 90 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e7ab4-139">Rules changes may take up to 90 minutes to propagate through the CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="e7ab4-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e7ab4-140">See also</span></span>
* [<span data-ttu-id="e7ab4-141">Az Azure CDN áttekintése</span><span class="sxs-lookup"><span data-stu-id="e7ab4-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="e7ab4-142">Szabályok motor referencia</span><span class="sxs-lookup"><span data-stu-id="e7ab4-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="e7ab4-143">Szabályok motor egyezés feltételek</span><span class="sxs-lookup"><span data-stu-id="e7ab4-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="e7ab4-144">Szabályok motor feltételes kifejezések</span><span class="sxs-lookup"><span data-stu-id="e7ab4-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="e7ab4-145">Szabályok adatbázismotor-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e7ab4-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="e7ab4-146">A szabályok használata alapértelmezett HTTP működés felülbírálata</span><span class="sxs-lookup"><span data-stu-id="e7ab4-146">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="e7ab4-147">[Az Azure péntekenként: Az Azure CDN új Premium szolgáltatással](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (videó)</span><span class="sxs-lookup"><span data-stu-id="e7ab4-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>