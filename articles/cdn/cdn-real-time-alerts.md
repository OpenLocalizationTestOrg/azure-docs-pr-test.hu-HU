---
title: "Az Azure CDN valós idejű riasztások |} Microsoft Docs"
description: "A Microsoft Azure CDN valós idejű riasztásokat. Valós idejű riasztások adja meg a CDN-profil végpontja teljesítményének kapcsolatos értesítéseket."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6e66eb076ac7220823a848b5047f147d4101cd55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a><span data-ttu-id="8a403-104">A Microsoft Azure CDN valós idejű riasztások</span><span class="sxs-lookup"><span data-stu-id="8a403-104">Real-time alerts in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="8a403-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8a403-105">Overview</span></span>
<span data-ttu-id="8a403-106">Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="8a403-106">This document explains real-time alerts in Microsoft Azure CDN.</span></span> <span data-ttu-id="8a403-107">Ez a funkció a CDN-profil végpontja teljesítményének valós idejű értesítések biztosít.</span><span class="sxs-lookup"><span data-stu-id="8a403-107">This functionality provides real-time notifications about the performance of the endpoints in your CDN profile.</span></span>  <span data-ttu-id="8a403-108">E-mailek vagy a HTTP-riasztások alapján állíthat be:</span><span class="sxs-lookup"><span data-stu-id="8a403-108">You can set up email or HTTP alerts based on:</span></span>

* <span data-ttu-id="8a403-109">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="8a403-109">Bandwidth</span></span>
* <span data-ttu-id="8a403-110">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="8a403-110">Status Codes</span></span>
* <span data-ttu-id="8a403-111">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="8a403-111">Cache Statuses</span></span>
* <span data-ttu-id="8a403-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8a403-112">Connections</span></span>

## <a name="creating-a-real-time-alert"></a><span data-ttu-id="8a403-113">A valós idejű riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a403-113">Creating a real-time alert</span></span>
1. <span data-ttu-id="8a403-114">Az a [Azure Portal](https://portal.azure.com), keresse meg a CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="8a403-114">In the [Azure Portal](https://portal.azure.com), browse to your CDN profile.</span></span>
   
    ![CDN-profil panelje](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. <span data-ttu-id="8a403-116">A CDN-profil panelje, kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a403-116">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    <span data-ttu-id="8a403-118">Megnyitja a CDN-felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="8a403-118">The CDN management portal opens.</span></span>
3. <span data-ttu-id="8a403-119">Vigye a **Analytics** lapra, és vigye a **valós idejű statisztikák** menü.</span><span class="sxs-lookup"><span data-stu-id="8a403-119">Hover over the **Analytics** tab, then hover over the **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="8a403-120">Kattintson a **valós idejű riasztások**.</span><span class="sxs-lookup"><span data-stu-id="8a403-120">Click on **Real-Time Alerts**.</span></span>
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    <span data-ttu-id="8a403-122">A meglévő riasztás konfigurációk (ha van ilyen) listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8a403-122">The list of existing alert configurations (if any) is displayed.</span></span>
4. <span data-ttu-id="8a403-123">Kattintson a **riasztási hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a403-123">Click the **Add Alert** button.</span></span>
   
    ![Riasztási gomb felvétele](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    <span data-ttu-id="8a403-125">Új riasztás létrehozásához űrlap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8a403-125">A form for creating a new alert is displayed.</span></span>
   
    ![Új riasztás űrlap](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. <span data-ttu-id="8a403-127">Ha azt szeretné, hogy ez a riasztás aktív elemre **mentése**, ellenőrizze a **riasztás engedélyezve** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8a403-127">If you want this alert to be active when you click **Save**, check the **Alert Enabled** checkbox.</span></span>
6. <span data-ttu-id="8a403-128">Adjon meg egy leíró nevet a riasztással a **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="8a403-128">Enter a descriptive name for your alert in the **Name** field.</span></span>
7. <span data-ttu-id="8a403-129">Az a **médiatípus** legördülő menüből válassza **HTTP nagy objektum**.</span><span class="sxs-lookup"><span data-stu-id="8a403-129">In the **Media Type** dropdown, select **HTTP Large Object**.</span></span>
   
    ![A kijelölt HTTP nagy objektum adathordozó-típus](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="8a403-131">Ki kell választania **HTTP nagy objektum** , a **médiatípus**.</span><span class="sxs-lookup"><span data-stu-id="8a403-131">You must select **HTTP Large Object** as the **Media Type**.</span></span>  <span data-ttu-id="8a403-132">Az egyéb lehetőségek nem használják **Azure CDN Verizon**.</span><span class="sxs-lookup"><span data-stu-id="8a403-132">The other choices are not used by **Azure CDN from Verizon**.</span></span>  <span data-ttu-id="8a403-133">Válassza ki a hibát **HTTP nagy objektum** , akkor soha nem aktiválódott a riasztást.</span><span class="sxs-lookup"><span data-stu-id="8a403-133">Failure to select **HTTP Large Object** will cause your alert to never be triggered.</span></span>
   > 
   > 
8. <span data-ttu-id="8a403-134">Hozzon létre egy **kifejezés** kiválasztásával figyelése egy **metrika**, **operátor**, és **érték indítás**.</span><span class="sxs-lookup"><span data-stu-id="8a403-134">Create an **Expression** to monitor by selecting a **Metric**, **Operator**, and **Trigger value**.</span></span>
   
   * <span data-ttu-id="8a403-135">A **metrika**, válassza ki a figyelt kívánt feltétel típusát.</span><span class="sxs-lookup"><span data-stu-id="8a403-135">For **Metric**, select the type of condition you want monitored.</span></span>  <span data-ttu-id="8a403-136">**Sávszélesség MB/s** a memóriakészletben felhasznált sávszélesség-használat megabit / másodperc.</span><span class="sxs-lookup"><span data-stu-id="8a403-136">**Bandwidth Mbps** is the amount of bandwidth usage in megabits per second.</span></span>  <span data-ttu-id="8a403-137">**Kapcsolatok száma összesen** peremhálózati kiszolgálókról egyidejű HTTP-kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="8a403-137">**Total Connections** is the number of concurrent HTTP connections to our edge servers.</span></span>  <span data-ttu-id="8a403-138">A különböző gyorsítótár állapotok és állapotkódok-definíciók, lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx) és [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx)</span><span class="sxs-lookup"><span data-stu-id="8a403-138">For definitions of the various cache statuses and status codes, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) and [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx)</span></span>
   * <span data-ttu-id="8a403-139">**Operátor** a matematikai operátor, amely megteremti a kapcsolatot a metrika és az eseményindítási között.</span><span class="sxs-lookup"><span data-stu-id="8a403-139">**Operator** is the mathematical operator that establishes the relationship between the metric and the trigger value.</span></span>
   * <span data-ttu-id="8a403-140">**Indítás, érték** a küszöbérték, amelyeknek teljesülniük kell ahhoz, egy értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="8a403-140">**Trigger Value** is the threshold value that must be met before a notification will be sent out.</span></span>
     
     <span data-ttu-id="8a403-141">Az az alábbi példában, a létrehozott kifejezés azt jelzi, hogy szeretnék-e értesítést, ha a 404-es állapotkód száma nagyobb, mint 25.</span><span class="sxs-lookup"><span data-stu-id="8a403-141">In the below example, the expression I have created indicates that I would like to be notified when the number of 404 status codes is greater than 25.</span></span>
     
     ![Valós idejű riasztási mintakifejezés](./media/cdn-real-time-alerts/cdn-expression.png)
9. <span data-ttu-id="8a403-143">A **időköz**, adja meg, hogy milyen gyakran szeretné a kiértékelendő kifejezés.</span><span class="sxs-lookup"><span data-stu-id="8a403-143">For **Interval**, enter how frequently you would like the expression evaluated.</span></span>
10. <span data-ttu-id="8a403-144">Az a **értesítés** legördülő menüből válassza, ha szeretne értesítést kapni a kifejezés igaz.</span><span class="sxs-lookup"><span data-stu-id="8a403-144">In the **Notify on** dropdown, select when you would like to be notified when the expression is true.</span></span>
    
    * <span data-ttu-id="8a403-145">**A feltétel Start** azt jelzi, hogy értesítést küld, amikor először észlelt a megadott feltételnek.</span><span class="sxs-lookup"><span data-stu-id="8a403-145">**Condition Start** indicates that a notification will be sent when the specified condition is first detected.</span></span>
    * <span data-ttu-id="8a403-146">**A feltétel End** azt jelzi, hogy értesítést küld, amikor a megadott feltétel már nem észlelhető.</span><span class="sxs-lookup"><span data-stu-id="8a403-146">**Condition End** indicates that a notification will be sent when the specified condition is no longer detected.</span></span> <span data-ttu-id="8a403-147">Ezt az értesítést csak akkor is kiváltódik, miután a hálózatfigyelési rendszer azt észlelte, hogy a megadott állapot fordult elő.</span><span class="sxs-lookup"><span data-stu-id="8a403-147">This notification can only be triggered after our network monitoring system detected that the specified condition occurred.</span></span>
    * <span data-ttu-id="8a403-148">**Folyamatos** azt jelzi, hogy egy értesítést küld minden alkalommal, hogy a hálózatfigyelési rendszer észleli az adott feltételnek.</span><span class="sxs-lookup"><span data-stu-id="8a403-148">**Continuous** indicates that a notification will be sent each time that the network monitoring system detects the specified condition.</span></span> <span data-ttu-id="8a403-149">Ne feledje, hogy a hálózatfigyelési rendszer a megadott feltétel időszakonként csak győződik egyszer.</span><span class="sxs-lookup"><span data-stu-id="8a403-149">Keep in mind that the network monitoring system will only check once per interval for the specified condition.</span></span>
    * <span data-ttu-id="8a403-150">**Az állapot kezdő és záró** azt jelzi, hogy értesítést küld először, hogy a megadott feltétel észlel, és ismét Ha a feltétel már nem észlelhető.</span><span class="sxs-lookup"><span data-stu-id="8a403-150">**Condition Start and End** indicates that a notification will be sent the first time that the specified condition is detected and once again when the condition is no longer detected.</span></span>
11. <span data-ttu-id="8a403-151">Ha azt szeretné, hogy e-mailben értesítést kapjon, ellenőrizze a **e-mailben értesítse** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8a403-151">If you want to receive notifications by email, check the **Notify by Email** checkbox.</span></span>  
    
    ![E-mailek űrlappal értesítése](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    <span data-ttu-id="8a403-153">Az a **való** mezőbe írja be az e-mail cím, ha azt szeretné, hogy értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="8a403-153">In the **To** field, enter the email address you where you want notifications sent.</span></span> <span data-ttu-id="8a403-154">A **tulajdonos** és **törzs**, előfordulhat, hogy hagyja meg az alapértelmezett vagy testreszabhatja az üzenet használatával a **elérhető kulcsszavak** lista dinamikusan szúrható be riasztási adatokat, az üzenet elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="8a403-154">For **Subject** and **Body**, you may leave the default, or you may customize the message using the **Available keywords** list to dynamically insert alert data when the message is sent.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="8a403-155">Az e-mail értesítés kattintva tesztelheti a **ellenőrző értesítés** gomb, de csak a riasztás konfigurálásában mentését követően.</span><span class="sxs-lookup"><span data-stu-id="8a403-155">You can test the email notification by clicking the **Test Notification** button, but only after the alert configuration has been saved.</span></span>
    > 
    > 
12. <span data-ttu-id="8a403-156">Ha azt szeretné, hogy a webkiszolgáló program értesítések, ellenőrizze a **HTTP Post által értesítendő** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8a403-156">If you want notifications to be posted to a web server, check the **Notify by HTTP Post** checkbox.</span></span>
    
    ![Közli a HTTP Post képernyő](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    <span data-ttu-id="8a403-158">Az a **URL-cím** mezőbe írja be az URL-cím, ahol azt szeretné, hogy a HTTP-üzenet közzé.</span><span class="sxs-lookup"><span data-stu-id="8a403-158">In the **Url** field, enter the URL you where you want the HTTP message posted.</span></span> <span data-ttu-id="8a403-159">Az a **fejlécek** szövegmező, adja meg a HTTP-fejlécek küldését a kérelemben.</span><span class="sxs-lookup"><span data-stu-id="8a403-159">In the **Headers** textbox, enter the HTTP headers to be sent in the request.</span></span>  <span data-ttu-id="8a403-160">A **törzs** testreszabhatja az üzenet használatával a **elérhető kulcsszavak** lista dinamikusan szúrható be riasztási adatokat, az üzenet elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="8a403-160">For **Body** you may customize the message using the **Available keywords** list to dynamically insert alert data when the message is sent.</span></span>  <span data-ttu-id="8a403-161">**Fejlécek** és **törzs** alapértelmezett egy XML-adattartalmat hasonló az alábbi példában.</span><span class="sxs-lookup"><span data-stu-id="8a403-161">**Headers** and **Body** default to an XML payload similar to the below example.</span></span>
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > <span data-ttu-id="8a403-162">A HTTP Post értesítési kattintva tesztelheti a **ellenőrző értesítés** gomb, de csak a riasztás konfigurálásában mentését követően.</span><span class="sxs-lookup"><span data-stu-id="8a403-162">You can test the HTTP Post notification by clicking the **Test Notification** button, but only after the alert configuration has been saved.</span></span>
    > 
    > 
13. <span data-ttu-id="8a403-163">Kattintson a **mentése** gombra kattintva mentse a riasztás konfigurálásában.</span><span class="sxs-lookup"><span data-stu-id="8a403-163">Click the **Save** button to save your alert configuration.</span></span>  <span data-ttu-id="8a403-164">Ha bejelölte **riasztás engedélyezve** 5. lépésben, a riasztás mostantól aktív.</span><span class="sxs-lookup"><span data-stu-id="8a403-164">If you checked **Alert Enabled** in step 5, your alert is now active.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a403-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a403-165">Next Steps</span></span>
* <span data-ttu-id="8a403-166">Elemezze [Azure CDN valós idejű statisztikák](cdn-real-time-stats.md)</span><span class="sxs-lookup"><span data-stu-id="8a403-166">Analyze [Real-time stats in Azure CDN](cdn-real-time-stats.md)</span></span>
* <span data-ttu-id="8a403-167">Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="8a403-167">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="8a403-168">Elemezze [használati minták](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="8a403-168">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

