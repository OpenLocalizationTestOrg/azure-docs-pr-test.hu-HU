---
title: "aaaAzure CDN valós idejű riasztások |} Microsoft Docs"
description: "A Microsoft Azure CDN valós idejű riasztásokat. Valós idejű riasztások adja meg a CDN-profil hello végpontok hello teljesítményével kapcsolatos értesítéseket."
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
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a><span data-ttu-id="3ad1e-104">A Microsoft Azure CDN valós idejű riasztások</span><span class="sxs-lookup"><span data-stu-id="3ad1e-104">Real-time alerts in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="3ad1e-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3ad1e-105">Overview</span></span>
<span data-ttu-id="3ad1e-106">Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-106">This document explains real-time alerts in Microsoft Azure CDN.</span></span> <span data-ttu-id="3ad1e-107">Ezt a funkciót biztosít a valós idejű értesítések hello teljesítmény hello végpontok a CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-107">This functionality provides real-time notifications about hello performance of hello endpoints in your CDN profile.</span></span>  <span data-ttu-id="3ad1e-108">E-mailek vagy a HTTP-riasztások alapján állíthat be:</span><span class="sxs-lookup"><span data-stu-id="3ad1e-108">You can set up email or HTTP alerts based on:</span></span>

* <span data-ttu-id="3ad1e-109">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="3ad1e-109">Bandwidth</span></span>
* <span data-ttu-id="3ad1e-110">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="3ad1e-110">Status Codes</span></span>
* <span data-ttu-id="3ad1e-111">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="3ad1e-111">Cache Statuses</span></span>
* <span data-ttu-id="3ad1e-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="3ad1e-112">Connections</span></span>

## <a name="creating-a-real-time-alert"></a><span data-ttu-id="3ad1e-113">A valós idejű riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ad1e-113">Creating a real-time alert</span></span>
1. <span data-ttu-id="3ad1e-114">A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![CDN-profil panelje](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. <span data-ttu-id="3ad1e-116">A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    <span data-ttu-id="3ad1e-118">Megnyílik a hello CDN felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="3ad1e-119">Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **valós idejű statisztikák** menü.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="3ad1e-120">Kattintson a **valós idejű riasztások**.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-120">Click on **Real-Time Alerts**.</span></span>
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    <span data-ttu-id="3ad1e-122">megjelenik a meglévő riasztás konfigurációk (ha van ilyen) hello listája.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-122">hello list of existing alert configurations (if any) is displayed.</span></span>
4. <span data-ttu-id="3ad1e-123">Kattintson a hello **riasztási hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-123">Click hello **Add Alert** button.</span></span>
   
    ![Riasztási gomb felvétele](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    <span data-ttu-id="3ad1e-125">Új riasztás létrehozásához űrlap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-125">A form for creating a new alert is displayed.</span></span>
   
    ![Új riasztás űrlap](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. <span data-ttu-id="3ad1e-127">Ha azt szeretné, a riasztás toobe aktív elemre **mentése**, ellenőrizze a hello **riasztás engedélyezve** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-127">If you want this alert toobe active when you click **Save**, check hello **Alert Enabled** checkbox.</span></span>
6. <span data-ttu-id="3ad1e-128">Adjon meg egy leíró nevet a riasztáshoz hello **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-128">Enter a descriptive name for your alert in hello **Name** field.</span></span>
7. <span data-ttu-id="3ad1e-129">A hello **médiatípus** legördülő menüből válassza **HTTP nagy objektum**.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-129">In hello **Media Type** dropdown, select **HTTP Large Object**.</span></span>
   
    ![A kijelölt HTTP nagy objektum adathordozó-típus](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="3ad1e-131">Ki kell választania **HTTP nagy objektum** , hello **médiatípus**.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-131">You must select **HTTP Large Object** as hello **Media Type**.</span></span>  <span data-ttu-id="3ad1e-132">hello egyéb lehetőségek nem használja a **Azure CDN Verizon**.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-132">hello other choices are not used by **Azure CDN from Verizon**.</span></span>  <span data-ttu-id="3ad1e-133">Hiba tooselect **HTTP nagy objektum** , akkor a riasztás toonever elindul.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-133">Failure tooselect **HTTP Large Object** will cause your alert toonever be triggered.</span></span>
   > 
   > 
8. <span data-ttu-id="3ad1e-134">Hozzon létre egy **kifejezés** toomonitor kiválasztásával egy **metrika**, **operátor**, és **érték indítás**.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-134">Create an **Expression** toomonitor by selecting a **Metric**, **Operator**, and **Trigger value**.</span></span>
   
   * <span data-ttu-id="3ad1e-135">A **metrika**, válassza ki a kívánt figyelt feltétel hello típusa.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-135">For **Metric**, select hello type of condition you want monitored.</span></span>  <span data-ttu-id="3ad1e-136">**Sávszélesség MB/s** hello mennyiségű sávszélesség-használat megabit / másodperc.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-136">**Bandwidth Mbps** is hello amount of bandwidth usage in megabits per second.</span></span>  <span data-ttu-id="3ad1e-137">**Kapcsolatok száma összesen** hello száma párhuzamos HTTP-kapcsolatok tooour peremhálózati kiszolgálóinak van.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-137">**Total Connections** is hello number of concurrent HTTP connections tooour edge servers.</span></span>  <span data-ttu-id="3ad1e-138">Hello definíciójának különböző gyorsítótár állapotok és állapotkódok, lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx) és [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx)</span><span class="sxs-lookup"><span data-stu-id="3ad1e-138">For definitions of hello various cache statuses and status codes, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) and [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx)</span></span>
   * <span data-ttu-id="3ad1e-139">**Operátor** hello matematikai operátor hello hello metrika és hello eseményindító érték közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-139">**Operator** is hello mathematical operator that establishes hello relationship between hello metric and hello trigger value.</span></span>
   * <span data-ttu-id="3ad1e-140">**Indítás, érték** van, amelyeknek teljesülniük kell ahhoz, egy értesítés küldése hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-140">**Trigger Value** is hello threshold value that must be met before a notification will be sent out.</span></span>
     
     <span data-ttu-id="3ad1e-141">Az alábbi példában hello létrehoztam hello kifejezés azt jelzi, hogy értesítést kap, ha 404-es állapotkód hello száma nagyobb, mint 25 toobe szeretném.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-141">In hello below example, hello expression I have created indicates that I would like toobe notified when hello number of 404 status codes is greater than 25.</span></span>
     
     ![Valós idejű riasztási mintakifejezés](./media/cdn-real-time-alerts/cdn-expression.png)
9. <span data-ttu-id="3ad1e-143">A **időköz**, adja meg, hogy milyen gyakran szeretné hello kiértékelendő kifejezés.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-143">For **Interval**, enter how frequently you would like hello expression evaluated.</span></span>
10. <span data-ttu-id="3ad1e-144">A hello **értesítés** legördülő menüből válassza, ha szeretné toobe értesíti, ha a hello kifejezés értéke true.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-144">In hello **Notify on** dropdown, select when you would like toobe notified when hello expression is true.</span></span>
    
    * <span data-ttu-id="3ad1e-145">**A feltétel Start** azt jelzi, hogy értesítést küld, amikor hello megadott feltétel először észlel.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-145">**Condition Start** indicates that a notification will be sent when hello specified condition is first detected.</span></span>
    * <span data-ttu-id="3ad1e-146">**A feltétel End** azt jelzi, hogy értesítést küld, amikor hello megadott feltétel már nem észlelhető.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-146">**Condition End** indicates that a notification will be sent when hello specified condition is no longer detected.</span></span> <span data-ttu-id="3ad1e-147">Csak akkor is kiváltódik ezt az értesítést, miután a hálózatfigyelési rendszer azt észlelte, hogy a megadott hello állapot fordult elő.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-147">This notification can only be triggered after our network monitoring system detected that hello specified condition occurred.</span></span>
    * <span data-ttu-id="3ad1e-148">**Folyamatos** azt jelzi, hogy értesítést küld minden alkalommal, amikor hálózati figyelési rendszer hello észleli hello megadott feltétel.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-148">**Continuous** indicates that a notification will be sent each time that hello network monitoring system detects hello specified condition.</span></span> <span data-ttu-id="3ad1e-149">Ne feledje, hogy hello hálózatfigyelési rendszer lesz csak ellenőrzés egyszer időszakonként hello a megadott feltétel.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-149">Keep in mind that hello network monitoring system will only check once per interval for hello specified condition.</span></span>
    * <span data-ttu-id="3ad1e-150">**Az állapot kezdő és záró** azt jelzi, hogy egy értesítést küld-e az első alkalom, hogy a megadott feltétel hello hello észlel, és ismét amikor hello feltétel már nem észlelhető.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-150">**Condition Start and End** indicates that a notification will be sent hello first time that hello specified condition is detected and once again when hello condition is no longer detected.</span></span>
11. <span data-ttu-id="3ad1e-151">Ha azt szeretné, tooreceive értesítéseket e-mailben, ellenőrizze a hello **e-mailben értesítse** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-151">If you want tooreceive notifications by email, check hello **Notify by Email** checkbox.</span></span>  
    
    ![E-mailek űrlappal értesítése](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    <span data-ttu-id="3ad1e-153">A hello **való** mezőbe írja be a hello e-mail címet, ha azt szeretné, hogy értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-153">In hello **To** field, enter hello email address you where you want notifications sent.</span></span> <span data-ttu-id="3ad1e-154">A **tulajdonos** és **törzs**, előfordulhat, hogy hagyja hello alapértelmezett vagy üdvözlőüzenetére hello segítségével szabhatja testre **elérhető kulcsszavak** lista toodynamically riasztási adatok beszúrása során hello üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-154">For **Subject** and **Body**, you may leave hello default, or you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="3ad1e-155">Hello e-mailben értesítést hello kattintva tesztelheti **ellenőrző értesítés** gomb, de csak hello riasztás konfigurációjának mentése után.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-155">You can test hello email notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
12. <span data-ttu-id="3ad1e-156">Ha azt szeretné, hogy a közzétett tooa webkiszolgáló értesítések toobe, ellenőrizze a hello **HTTP Post által értesítendő** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-156">If you want notifications toobe posted tooa web server, check hello **Notify by HTTP Post** checkbox.</span></span>
    
    ![Közli a HTTP Post képernyő](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    <span data-ttu-id="3ad1e-158">A hello **URL-cím** mezőbe írja be, ha azt szeretné, hogy HTTP üdvözlőüzenetére közzétett hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-158">In hello **Url** field, enter hello URL you where you want hello HTTP message posted.</span></span> <span data-ttu-id="3ad1e-159">A hello **fejlécek** szövegmezőhöz hello HTTP-fejlécek toobe hello kérelemben küldött adja meg.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-159">In hello **Headers** textbox, enter hello HTTP headers toobe sent in hello request.</span></span>  <span data-ttu-id="3ad1e-160">A **törzs** szabhatja testre a hello segítségével üdvözlőüzenetére **elérhető kulcsszavak** lista toodynamically beszúrása riasztási adatokat hello üzenet elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-160">For **Body** you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>  <span data-ttu-id="3ad1e-161">**Fejlécek** és **törzs** alapértelmezett tooan XML hasznos hasonló toohello alábbi példában.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-161">**Headers** and **Body** default tooan XML payload similar toohello below example.</span></span>
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > <span data-ttu-id="3ad1e-162">Hello HTTP Post értesítési hello kattintva tesztelheti **ellenőrző értesítés** gomb, de csak hello riasztás konfigurációjának mentése után.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-162">You can test hello HTTP Post notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
13. <span data-ttu-id="3ad1e-163">Kattintson a hello **mentése** toosave gombra a riasztás konfigurálásában.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-163">Click hello **Save** button toosave your alert configuration.</span></span>  <span data-ttu-id="3ad1e-164">Ha bejelölte **riasztás engedélyezve** 5. lépésben, a riasztás mostantól aktív.</span><span class="sxs-lookup"><span data-stu-id="3ad1e-164">If you checked **Alert Enabled** in step 5, your alert is now active.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ad1e-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ad1e-165">Next Steps</span></span>
* <span data-ttu-id="3ad1e-166">Elemezze [Azure CDN valós idejű statisztikák](cdn-real-time-stats.md)</span><span class="sxs-lookup"><span data-stu-id="3ad1e-166">Analyze [Real-time stats in Azure CDN](cdn-real-time-stats.md)</span></span>
* <span data-ttu-id="3ad1e-167">Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="3ad1e-167">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="3ad1e-168">Elemezze [használati minták](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="3ad1e-168">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

