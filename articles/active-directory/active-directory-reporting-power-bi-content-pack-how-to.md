---
title: "Az Azure Active Directory Power BI-tartalomcsomag használata | Microsoft Docs"
description: "Útmutató az Azure Active Directory Power BI-tartalomcsomag használatához | Microsoft Docs"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5c642bb814a279756e8391f12fdc86b6ec0b4a8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a><span data-ttu-id="a7001-103">Az Azure Active Directory Power BI-tartalomcsomag használata</span><span class="sxs-lookup"><span data-stu-id="a7001-103">How to use the Azure Active Directory Power BI Content Pack</span></span>

<span data-ttu-id="a7001-104">Rendszergazdaként elengedhetetlen, hogy tudja, hogyan használják felhasználói az Azure Active Directory funkcióit.</span><span class="sxs-lookup"><span data-stu-id="a7001-104">Understanding how your users adopt and use Azure Active Directory features is critical for you as an IT admin.</span></span> <span data-ttu-id="a7001-105">Ez lehetővé teszi, hogy úgy tervezze meg az informatikai infrastruktúráját és kommunikációját, hogy növelni tudja annak használatát, és a lehető legtöbbet hozza ki az AAD funkcióiból.</span><span class="sxs-lookup"><span data-stu-id="a7001-105">It allows you to plan your IT infrastructure and communication to increase usage and to get the most out of AAD features.</span></span> <span data-ttu-id="a7001-106">Az Azure Active Directory lehetővé teszi adatai részletesebb elemzését, így átláthatja, hogyan használhatja ezeket az adatokat részletesebb betekintések szerzéséhez abba, mi történik az Azure Active Directoryjukban az Ön számára legfontosabb képességek tekintetében.</span><span class="sxs-lookup"><span data-stu-id="a7001-106">Power BI Content Pack for Azure Active Directory gives you the ability to further analyze your data to understand how you can use this data to gather richer insights into what’s going on with their Azure Active Directory for the various capabilities you heavily rely on.</span></span>  <span data-ttu-id="a7001-107">Az Azure Active Directory API-jainak a Power BI-ba integrálásával könnyedén letöltheti az előre összeállított tartalomcsomagokat, valamint betekintést nyerhet az Azure Active Directoryn belüli tevékenységekbe a Power BI által biztosított részletes megjelenítések használatával.</span><span class="sxs-lookup"><span data-stu-id="a7001-107">With the integration of Azure Active Directory APIs into Power BI, you can easily download the pre-built content packs and gain insights to all the activities within your Azure Active Directory using rich visualization experience Power BI offers.</span></span> <span data-ttu-id="a7001-108">Létrehozhatja saját irányítópultját, és könnyedén megoszthatja szervezete bármelyik tagjával.</span><span class="sxs-lookup"><span data-stu-id="a7001-108">You can create your own dashboard and share it easily with anyone in your organization.</span></span> 

<span data-ttu-id="a7001-109">Ez a témakör lépésről lépésre bemutatja, hogyan telepítheti és használhatja környezetében a tartalomcsomagot.</span><span class="sxs-lookup"><span data-stu-id="a7001-109">This topic provides you with step-by-step instructions on how to install and use the content pack in your environment.</span></span>

## <a name="installation"></a><span data-ttu-id="a7001-110">Telepítés</span><span class="sxs-lookup"><span data-stu-id="a7001-110">Installation</span></span>  

<span data-ttu-id="a7001-111">**A Power BI-tartalomcsomag telepítése:**</span><span class="sxs-lookup"><span data-stu-id="a7001-111">**To install the Power BI Content Pack:**</span></span>

1. <span data-ttu-id="a7001-112">Jelentkezzen be a [Power BI-ba](https://app.powerbi.com/groups/me/getdata/services) Power BI-fiókjával (ez megegyezik az O365- vagy Azure AD-fiókjával).</span><span class="sxs-lookup"><span data-stu-id="a7001-112">Log into [Power BI](https://app.powerbi.com/groups/me/getdata/services) with your Power BI Account (this is the same account as your O365 or Azure AD Account).</span></span>

2. <span data-ttu-id="a7001-113">A bal oldali navigációs panel alján kattintson az **Adatok lekérése** elemre.</span><span class="sxs-lookup"><span data-stu-id="a7001-113">At the bottom of the left navigation pane, select **Get Data**.</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. <span data-ttu-id="a7001-115">A **Szolgáltatások** panelen kattintson a **Lekérés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7001-115">In the **Services** box, click **Get**.</span></span>
   
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  <span data-ttu-id="a7001-117">Keresse meg az **Azure Active Directoryt**.</span><span class="sxs-lookup"><span data-stu-id="a7001-117">Search for **Azure Active Directory**.</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  <span data-ttu-id="a7001-119">Amikor a rendszer arra kéri, írja be Azure AD-bérlőazonosítóját, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7001-119">When prompted, type your Azure AD Tenant ID, and then click **Next**.</span></span>

    > [!TIP] 
    > <span data-ttu-id="a7001-120">Gyorsan megszerezheti az Office 365-/Azure AD-bérlőazonosítóját, ha bejelentkezik az Azure AD portálra, megkeresi a könyvtárat, és másolja az azonosítót a következő URL-címről: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span><span class="sxs-lookup"><span data-stu-id="a7001-120">A quick way to get the Tenant Id for your Office 365 / Azure AD tenant is to login to the Azure AD Portal, drill down to the directory and copy the ID from the following URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  <span data-ttu-id="a7001-122">Kattintson a **Bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7001-122">Click **Sign-in**.</span></span> 
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  <span data-ttu-id="a7001-124">Írja be felhasználónevét és jelszavát, majd kattintson a **Bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7001-124">Enter your username and password, and then click **Sign in**.</span></span>
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  <span data-ttu-id="a7001-126">Az alkalmazással kapcsolatos beleegyezési párbeszédpanelen kattintson az **Elfogadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="a7001-126">On the app consent dialog, click **Accept**.</span></span>
 
9.  <span data-ttu-id="a7001-127">Miután létrejött az Azure Active Directory-tevékenységnaplók irányítópultja, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="a7001-127">When your Azure Active Directory Activity logs dashboard has been created, click it.</span></span>
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a><span data-ttu-id="a7001-129">Mire használhatom ezt a tartalomcsomagot?</span><span class="sxs-lookup"><span data-stu-id="a7001-129">What can I do with this content pack?</span></span>

<span data-ttu-id="a7001-130">Mielőtt mélyebben beleásnánk magunkat, mi mindenre használhatja ezt a tartalomcsomagot, tekintse meg ezt a rövid áttekintést a tartalomcsomagban található különböző jelentésekről.</span><span class="sxs-lookup"><span data-stu-id="a7001-130">Before we jump into what you can do with this content pack, here’s quick preview of the various reports in the content pack.</span></span> <span data-ttu-id="a7001-131">**Az elmúlt 30 nap** jelentésadatai érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a7001-131">Report data goes back to the **last 30 days**.</span></span>

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a><span data-ttu-id="a7001-132">Az Azure Active Directory-naplók tartalomcsomagjának jelen verziójában található jelentések</span><span class="sxs-lookup"><span data-stu-id="a7001-132">Reports included in this version of Azure Active Directory logs Content Pack</span></span>

<span data-ttu-id="a7001-133">**Alkalmazáshasználat és -trendek jelentés**: Többet tudhat meg a szervezetében használt alkalmazásokról: melyiket használják a legtöbbet és mikor.</span><span class="sxs-lookup"><span data-stu-id="a7001-133">**App Usage and Trend report**:  Get insights into the apps used in your organization and which ones are being used the most and when.</span></span> <span data-ttu-id="a7001-134">Ebből a jelentésből megtudhatja, hogy a szervezetében a közelmúltban kiadott alkalmazásokat hogyan használják, vagy hogy mely alkalmazások népszerűek.</span><span class="sxs-lookup"><span data-stu-id="a7001-134">You can use this report to gather insights into how an app you recently rolled out in your organization is being used or find out which apps are popular.</span></span> <span data-ttu-id="a7001-135">Így javíthatja a kihasználtságot, ha azt tapasztalja, hogy az alkalmazást nem használják.</span><span class="sxs-lookup"><span data-stu-id="a7001-135">By doing this, you can improve usage if you see if the app is not being used.</span></span>

<span data-ttu-id="a7001-136">**Bejelentkezések hely és felhasználók szerint**: Megtekintheti az összes Azure-identitással végzett bejelentkezést, és láthatja a felhasználók identitását.</span><span class="sxs-lookup"><span data-stu-id="a7001-136">**Sign-ins by location and users**: Get insights into all the sign-ins performed using Azure Identity and gives insights into the identity of the users.</span></span> <span data-ttu-id="a7001-137">Ennek segítségével részletesebb betekintést nyerhet az egyes bejelentkezésekbe, és választ kaphat többek között a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="a7001-137">With this, you can dig deeper into individual sign-ins and answer questions like:</span></span>

- <span data-ttu-id="a7001-138">Honnan jelentkezett be ez a felhasználó?</span><span class="sxs-lookup"><span data-stu-id="a7001-138">From where did this user sign-ins?</span></span>
- <span data-ttu-id="a7001-139">Melyik felhasználó jelentkezett be a legtöbbször, és honnan jelentkezett be?</span><span class="sxs-lookup"><span data-stu-id="a7001-139">Which user has the most sign-ins and where do they sign-in from?</span></span> 
- <span data-ttu-id="a7001-140">Sikeres volt a bejelentkezés?</span><span class="sxs-lookup"><span data-stu-id="a7001-140">Was the sign-in successful?</span></span>  
 
<span data-ttu-id="a7001-141">Megtekintheti a részleteket is egy konkrét dátumra vagy helyre kattintva.</span><span class="sxs-lookup"><span data-stu-id="a7001-141">You can drill into details by clicking on a specific date or location.</span></span>

<span data-ttu-id="a7001-142">**Egyedi felhasználók alkalmazásonként**: Egy adott alkalmazás összes egyedi felhasználója.</span><span class="sxs-lookup"><span data-stu-id="a7001-142">**Unique users per app**:  Get a view of all unique users using a given app.</span></span> <span data-ttu-id="a7001-143">Ebbe csak azok a felhasználók számítanak bele, akik „*sikeresen*” bejelentkeztek az adott alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="a7001-143">This includes only users who have “*successfully*” signed into an application.</span></span>

<span data-ttu-id="a7001-144">**Eszközbejelentkezések**: Tekintse meg a szervezete felhasználói által használt operációs rendszereket és böngészőket a felhasználókra vonatkozó részletes információkkal együtt, például a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="a7001-144">**Device sign-ins**: Get a view of the type of OS and browsers are being used by users in your organization with detailed information on the users including:</span></span>

- <span data-ttu-id="a7001-145">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a7001-145">User Name</span></span>
- <span data-ttu-id="a7001-146">IP-cím</span><span class="sxs-lookup"><span data-stu-id="a7001-146">IP Address</span></span>
- <span data-ttu-id="a7001-147">Hely</span><span class="sxs-lookup"><span data-stu-id="a7001-147">Location</span></span> 
- <span data-ttu-id="a7001-148">Bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="a7001-148">Sign-in status</span></span> 

<span data-ttu-id="a7001-149">Ebből a jelentésből megismerheti a szervezetében használt különböző eszközprofilokat, és ez alapján dönthet az eszközházirendekről.</span><span class="sxs-lookup"><span data-stu-id="a7001-149">With this report, you can understand the various device profiles used within your organization and determine device policies based on what’s used</span></span>

<span data-ttu-id="a7001-150">**SSPR tölcsér**: Megismerheti a szervezete jelszó-visszaállítási folyamatait.</span><span class="sxs-lookup"><span data-stu-id="a7001-150">**SSPR Funnel**: Get an understanding on how password resets are being done in your organization.</span></span> <span data-ttu-id="a7001-151">Betekintést nyerhet abba, hány új jelszót igényeltek az SSPR-eszközből, és hány volt ezek közül sikeres.</span><span class="sxs-lookup"><span data-stu-id="a7001-151">Get a peek into how many password resets were attempted through the SSPR tool and how many of them were successful.</span></span> <span data-ttu-id="a7001-152">Többet tudhat meg az SSPR tölcsérrel végzett sikertelen jelszó-visszaállításokról, és megvizsgálhatja, mi lehetett a sikertelenség oka.</span><span class="sxs-lookup"><span data-stu-id="a7001-152">Dig deeper into the Password resets failure using the SSPR funnel and understand why certain failures occurred.</span></span> <span data-ttu-id="a7001-153">Ez jelentés részletesebb betekintést biztosít az SSPR eszköz szervezeten belüli használatába, és ezáltal segít a helyes döntések meghozatalában.</span><span class="sxs-lookup"><span data-stu-id="a7001-153">This report gives a deeper understanding of how the SSPR tool is used within your organization so you can make the right decisions.</span></span>

## <a name="customizing-azure-ad-activity-content-pack"></a><span data-ttu-id="a7001-154">Az Azure AD-tevékenység tartalomcsomag személyre szabása</span><span class="sxs-lookup"><span data-stu-id="a7001-154">Customizing Azure AD Activity content pack</span></span>

<span data-ttu-id="a7001-155">**Megjelenítés módosítása**: Megváltoztathatja egy jelentés megjelenítését a **Jelentés szerkesztése** elemre kattintással és a kívánt megjelenítés kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="a7001-155">**Change Visualization**:  You can change a report visualization by clicking **Edit Report** and select the visualization you want.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

<span data-ttu-id="a7001-158">**További mezők belefoglalása**: Hozzáadhat mezőket a jelentéshez, vagy eltávolíthat mezőket azon vizualizáció kiválasztásával, amelyhez a hozzáadási/eltávolítási műveletet végre kívánja hajtani.</span><span class="sxs-lookup"><span data-stu-id="a7001-158">**Include additional fields**:  You can add a field to the report or remove it by selecting the visual to which you want to add/remove the field.</span></span> <span data-ttu-id="a7001-159">A következő példában a „Sign-in status” (Bejelentkezési állapot) mezőt adom hozzá a táblanézethez.</span><span class="sxs-lookup"><span data-stu-id="a7001-159">In the example below, I am adding “sign-in status” field to the table view.</span></span> 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

<span data-ttu-id="a7001-161">**Megjelenítések rögzítése az irányítópulton**: Személyre szabhatja irányítópultját, és felveheti saját megjelenítéseit a jelentésbe, amelyet azután az irányítópulton rögzíthet.</span><span class="sxs-lookup"><span data-stu-id="a7001-161">**Pin visualizations to your dashboard**:  You can customize your dashboard and include your own visualizations to the report and pin it to the dashboard.</span></span> <span data-ttu-id="a7001-162">A következő példában egy új, „Sign-in Status” (Bejelentkezési állapot) nevű szűrőt adtam hozzá és vettem fel a jelentésbe.</span><span class="sxs-lookup"><span data-stu-id="a7001-162">In the example below, I added a new filter called “Sign-in Status” and included it in the report.</span></span> <span data-ttu-id="a7001-163">Ezenkívül megváltoztattam a megjelenítést oszlopdiagramról vonaldiagramra. Ez az új vizualizáció rögzíthető az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="a7001-163">I also changed the visualization from bar chart to a line chart and can pin this new visual to the dashboard.</span></span>

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


<span data-ttu-id="a7001-166">**Irányítópult megosztása**: Miután létrehozta a kívánt tartalmat, megoszthatja irányítópultját szervezete felhasználóival.</span><span class="sxs-lookup"><span data-stu-id="a7001-166">**Sharing your dashboard**: Once you have created the content you want, you can share the dashboard with the users in your organization.</span></span> <span data-ttu-id="a7001-167">Vegye figyelembe, hogy ha megoszt egy jelentést, azokat a mezőket láthatják, amelyeket a jelentésben kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="a7001-167">Please remember that once you share the report, they can see the fields you have selected in the report.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a><span data-ttu-id="a7001-169">A Power BI-jelentés napi frissítésének ütemezése</span><span class="sxs-lookup"><span data-stu-id="a7001-169">Scheduling a daily refresh of your Power BI report</span></span>

<span data-ttu-id="a7001-170">A Power BI-jelentés napi frissítésének ütemezéséhez lépjen az **Adatkészletek > Beállítások > Frissítés ütemezése** részbe, és állítsa be az alábbiaknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a7001-170">To schedule a daily refresh of your Power BI report, go to **Datasets > Settings > Schedule Refresh** and set it as shown below.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a><span data-ttu-id="a7001-172">Frissítés a tartalomcsomag újabb verziójára</span><span class="sxs-lookup"><span data-stu-id="a7001-172">Updating to newer version of content pack</span></span>

<span data-ttu-id="a7001-173">A tartalomcsomag frissítése új verzió beszerzéséhez:</span><span class="sxs-lookup"><span data-stu-id="a7001-173">If you want to update your content pack to get a newer version:</span></span>

- <span data-ttu-id="a7001-174">Töltse le az új tartalomcsomagot, és állítsa be az itt szereplő utasításoknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a7001-174">Download the new content pack and set it up as per instructions listed in this article.</span></span>

- <span data-ttu-id="a7001-175">Ha elkészült a beállítással, lépjen az **Adatforrás > Beállítások > Adatforráshoz tartozó hitelesítő adatok** részbe, és adja meg újra a hitelesítő adatait az alábbiaknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a7001-175">Once you have set it up, go to **Data Source > Settings > Data source credentials** and re-enter your credentials as shown below</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

<span data-ttu-id="a7001-177">Amint a tartalomcsomag új verziója működőképes, a korábbi tartalomcsomag alapjául szolgáló jelentések és a tartalomcsomaghoz tartozó adatkészletek törlésével eltávolíthatja a régi verziót, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7001-177">As soon as the new version of the content pack is working, you can remove the old version if needed by deleting the underlying reports and datasets associated with that content pack.</span></span>

## <a name="still-having-issues"></a><span data-ttu-id="a7001-178">Továbbra is problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="a7001-178">Still having issues?</span></span> 

<span data-ttu-id="a7001-179">Tekintse meg a [hibaelhárítási útmutatót](active-directory-reporting-troubleshoot-content-pack.md).</span><span class="sxs-lookup"><span data-stu-id="a7001-179">Check out our [troubleshooting guide](active-directory-reporting-troubleshoot-content-pack.md).</span></span> <span data-ttu-id="a7001-180">A Power BI-jal kapcsolatos általános útmutatásért tekintse meg ezeket a [súgótémaköröket](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span><span class="sxs-lookup"><span data-stu-id="a7001-180">For general help with Power BI, check out these [help articles](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span></span>
 

## <a name="next-steps"></a><span data-ttu-id="a7001-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7001-181">Next steps</span></span>

<span data-ttu-id="a7001-182">A jelentéskészítés áttekintéséért lásd: [Jelentéskészítés az Azure Active Directoryban](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a7001-182">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
