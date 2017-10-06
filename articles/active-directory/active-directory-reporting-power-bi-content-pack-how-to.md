---
title: aaaHow toouse hello Azure Active Directory Power BI tartalomcsomag |} Microsoft Docs
description: Ismerje meg, hogyan toouse hello Azure Active Directory Power BI tartalomcsomag
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
ms.openlocfilehash: d07d678aedbe3089c4ea5f981f72311bdb389a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-active-directory-power-bi-content-pack"></a><span data-ttu-id="b560f-103">Hogyan toouse hello Azure Active Directory Power BI tartalomcsomag</span><span class="sxs-lookup"><span data-stu-id="b560f-103">How toouse hello Azure Active Directory Power BI Content Pack</span></span>

<span data-ttu-id="b560f-104">Rendszergazdaként elengedhetetlen, hogy tudja, hogyan használják felhasználói az Azure Active Directory funkcióit. Ez lehetővé teszi az informatikai infrastruktúra és a kommunikáció tooincrease használati és tooget hello legtöbb kívül AAD szolgáltatások tooplan.</span><span class="sxs-lookup"><span data-stu-id="b560f-104">Understanding how your users adopt and use Azure Active Directory features is critical for you as an IT admin. It allows you tooplan your IT infrastructure and communication tooincrease usage and tooget hello most out of AAD features.</span></span> <span data-ttu-id="b560f-105">A Power BI-tartalmakat csomag az Azure Active Directory által biztosított lehetőséget toofurther hello, elemezheti az adatokat toounderstand, hogyan használhatja a adatok toogather mélyebb betekintést az Azure Active Directoryval a hello jelenít meg különböző képességeket, erősen támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="b560f-105">Power BI Content Pack for Azure Active Directory gives you hello ability toofurther analyze your data toounderstand how you can use this data toogather richer insights into what’s going on with their Azure Active Directory for hello various capabilities you heavily rely on.</span></span>  <span data-ttu-id="b560f-106">A Power BI-bA az Azure Active Directory API integrációja hello egyszerűen letöltheti hello előre elkészített tartalomcsomagok és dcu tooall hello tevékenységek az Azure Active Directoryban gazdag képi megjelenítés élményt nyújt a Power BI használatával.</span><span class="sxs-lookup"><span data-stu-id="b560f-106">With hello integration of Azure Active Directory APIs into Power BI, you can easily download hello pre-built content packs and gain insights tooall hello activities within your Azure Active Directory using rich visualization experience Power BI offers.</span></span> <span data-ttu-id="b560f-107">Létrehozhatja saját irányítópultját, és könnyedén megoszthatja szervezete bármelyik tagjával.</span><span class="sxs-lookup"><span data-stu-id="b560f-107">You can create your own dashboard and share it easily with anyone in your organization.</span></span> 

<span data-ttu-id="b560f-108">Ez a témakör részletes lépéseit hogyan tooinstall és -felhasználási hello tartalom csomagolja a környezetben.</span><span class="sxs-lookup"><span data-stu-id="b560f-108">This topic provides you with step-by-step instructions on how tooinstall and use hello content pack in your environment.</span></span>

## <a name="installation"></a><span data-ttu-id="b560f-109">Telepítés</span><span class="sxs-lookup"><span data-stu-id="b560f-109">Installation</span></span>  

<span data-ttu-id="b560f-110">**tooinstall hello Power BI-tartalomcsomag:**</span><span class="sxs-lookup"><span data-stu-id="b560f-110">**tooinstall hello Power BI Content Pack:**</span></span>

1. <span data-ttu-id="b560f-111">Jelentkezzen be [Power BI](https://app.powerbi.com/groups/me/getdata/services) a Power BI-fiókjába (Ez az hello ugyanazt a fiókot az Office 365 vagy Azure AD-fiókot).</span><span class="sxs-lookup"><span data-stu-id="b560f-111">Log into [Power BI](https://app.powerbi.com/groups/me/getdata/services) with your Power BI Account (this is hello same account as your O365 or Azure AD Account).</span></span>

2. <span data-ttu-id="b560f-112">Alján hello hello bal oldali navigációs panelen, jelölje ki a **adatok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="b560f-112">At hello bottom of hello left navigation pane, select **Get Data**.</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. <span data-ttu-id="b560f-114">A hello **szolgáltatások** kattintson **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="b560f-114">In hello **Services** box, click **Get**.</span></span>
   
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  <span data-ttu-id="b560f-116">Keresse meg az **Azure Active Directoryt**.</span><span class="sxs-lookup"><span data-stu-id="b560f-116">Search for **Azure Active Directory**.</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  <span data-ttu-id="b560f-118">Amikor a rendszer arra kéri, írja be Azure AD-bérlőazonosítóját, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b560f-118">When prompted, type your Azure AD Tenant ID, and then click **Next**.</span></span>

    > [!TIP] 
    > <span data-ttu-id="b560f-119">Egy gyorsan tooget hello Bérlőazonosító az Office 365 / Azure AD-bérlő toologin toohello Azure AD-portálhoz, részletekbe menően tárhatják toohello directory, és másolja hello azonosító URL-cím a következő hello: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ ActiveDirectoryExtension vagykönyvtár/<tenantid>/directoryQuickStart</span><span class="sxs-lookup"><span data-stu-id="b560f-119">A quick way tooget hello Tenant Id for your Office 365 / Azure AD tenant is toologin toohello Azure AD Portal, drill down toohello directory and copy hello ID from hello following URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  <span data-ttu-id="b560f-121">Kattintson a **Bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b560f-121">Click **Sign-in**.</span></span> 
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  <span data-ttu-id="b560f-123">Írja be felhasználónevét és jelszavát, majd kattintson a **Bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b560f-123">Enter your username and password, and then click **Sign in**.</span></span>
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  <span data-ttu-id="b560f-125">A következő párbeszédpanelen: hello app hozzájárulási, kattintson a **elfogadás**.</span><span class="sxs-lookup"><span data-stu-id="b560f-125">On hello app consent dialog, click **Accept**.</span></span>
 
9.  <span data-ttu-id="b560f-126">Miután létrejött az Azure Active Directory-tevékenységnaplók irányítópultja, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="b560f-126">When your Azure Active Directory Activity logs dashboard has been created, click it.</span></span>
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a><span data-ttu-id="b560f-128">Mire használhatom ezt a tartalomcsomagot?</span><span class="sxs-lookup"><span data-stu-id="b560f-128">What can I do with this content pack?</span></span>

<span data-ttu-id="b560f-129">Ahhoz, hogy belevágjon mit tehet a tartalomcsomaggal, az alábbiakban rövid áttekintés hello különböző jelentéseket hello tartalmat a csomag.</span><span class="sxs-lookup"><span data-stu-id="b560f-129">Before we jump into what you can do with this content pack, here’s quick preview of hello various reports in hello content pack.</span></span> <span data-ttu-id="b560f-130">A jelentés adatainak Visszalépés toohello **utolsó 30 nap**.</span><span class="sxs-lookup"><span data-stu-id="b560f-130">Report data goes back toohello **last 30 days**.</span></span>

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a><span data-ttu-id="b560f-131">Az Azure Active Directory-naplók tartalomcsomagjának jelen verziójában található jelentések</span><span class="sxs-lookup"><span data-stu-id="b560f-131">Reports included in this version of Azure Active Directory logs Content Pack</span></span>

<span data-ttu-id="b560f-132">**Alkalmazás használatának és a Trend jelentés**: hello alkalmazásokba használt nyerhet a szervezet és a munkaterületek használt legtöbb hello és mikor.</span><span class="sxs-lookup"><span data-stu-id="b560f-132">**App Usage and Trend report**:  Get insights into hello apps used in your organization and which ones are being used hello most and when.</span></span> <span data-ttu-id="b560f-133">Ez a jelentés toogather betekintést Ön nemrég megkezdődött a szervezetben az alkalmazások használatának használhatja, vagy megtudhatja, mely alkalmazások állnak népszerű.</span><span class="sxs-lookup"><span data-stu-id="b560f-133">You can use this report toogather insights into how an app you recently rolled out in your organization is being used or find out which apps are popular.</span></span> <span data-ttu-id="b560f-134">Ezzel az eljárással használati is javítható, ha megjelenik, ha hello alkalmazás nem használja.</span><span class="sxs-lookup"><span data-stu-id="b560f-134">By doing this, you can improve usage if you see if hello app is not being used.</span></span>

<span data-ttu-id="b560f-135">**Hely és a felhasználói bejelentkezéseket**: az összes hello bejelentkezések alapján történik az Azure Identity és hello identitásának hello felhasználók által biztosított betekintést nyerhet.</span><span class="sxs-lookup"><span data-stu-id="b560f-135">**Sign-ins by location and users**: Get insights into all hello sign-ins performed using Azure Identity and gives insights into hello identity of hello users.</span></span> <span data-ttu-id="b560f-136">Ennek segítségével részletesebb betekintést nyerhet az egyes bejelentkezésekbe, és választ kaphat többek között a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="b560f-136">With this, you can dig deeper into individual sign-ins and answer questions like:</span></span>

- <span data-ttu-id="b560f-137">Honnan jelentkezett be ez a felhasználó?</span><span class="sxs-lookup"><span data-stu-id="b560f-137">From where did this user sign-ins?</span></span>
- <span data-ttu-id="b560f-138">Melyik felhasználó van hello legtöbb bejelentkezéseket és ahol tegye azokat jelentkezzen be a?</span><span class="sxs-lookup"><span data-stu-id="b560f-138">Which user has hello most sign-ins and where do they sign-in from?</span></span> 
- <span data-ttu-id="b560f-139">Hello bejelentkezés sikeres volt?</span><span class="sxs-lookup"><span data-stu-id="b560f-139">Was hello sign-in successful?</span></span>  
 
<span data-ttu-id="b560f-140">Megtekintheti a részleteket is egy konkrét dátumra vagy helyre kattintva.</span><span class="sxs-lookup"><span data-stu-id="b560f-140">You can drill into details by clicking on a specific date or location.</span></span>

<span data-ttu-id="b560f-141">**Egyedi felhasználók alkalmazásonként**: Egy adott alkalmazás összes egyedi felhasználója.</span><span class="sxs-lookup"><span data-stu-id="b560f-141">**Unique users per app**:  Get a view of all unique users using a given app.</span></span> <span data-ttu-id="b560f-142">Ebbe csak azok a felhasználók számítanak bele, akik „*sikeresen*” bejelentkeztek az adott alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="b560f-142">This includes only users who have “*successfully*” signed into an application.</span></span>

<span data-ttu-id="b560f-143">**Eszköz bejelentkezések**: egy megtekintheti a hello operációsrendszer-típust, és böngészők hello felhasználók többek között részletes tájékoztatást a szervezethez tartozó felhasználók használnak:</span><span class="sxs-lookup"><span data-stu-id="b560f-143">**Device sign-ins**: Get a view of hello type of OS and browsers are being used by users in your organization with detailed information on hello users including:</span></span>

- <span data-ttu-id="b560f-144">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b560f-144">User Name</span></span>
- <span data-ttu-id="b560f-145">IP-cím</span><span class="sxs-lookup"><span data-stu-id="b560f-145">IP Address</span></span>
- <span data-ttu-id="b560f-146">Hely</span><span class="sxs-lookup"><span data-stu-id="b560f-146">Location</span></span> 
- <span data-ttu-id="b560f-147">Bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="b560f-147">Sign-in status</span></span> 

<span data-ttu-id="b560f-148">Ezzel a jelentéssel megismerheti hello különböző eszközprofilok a szervezetén belül, és határozza meg az eszközökre vonatkozó házirendeket használttól alapján</span><span class="sxs-lookup"><span data-stu-id="b560f-148">With this report, you can understand hello various device profiles used within your organization and determine device policies based on what’s used</span></span>

<span data-ttu-id="b560f-149">**SSPR tölcsér**: Megismerheti a szervezete jelszó-visszaállítási folyamatait.</span><span class="sxs-lookup"><span data-stu-id="b560f-149">**SSPR Funnel**: Get an understanding on how password resets are being done in your organization.</span></span> <span data-ttu-id="b560f-150">Egy betekintés bejutni hány jelszó alaphelyzetbe állítását megkísérelte hello SSPR eszköz segítségével, és ezeknek sikerrel járt-e.</span><span class="sxs-lookup"><span data-stu-id="b560f-150">Get a peek into how many password resets were attempted through hello SSPR tool and how many of them were successful.</span></span> <span data-ttu-id="b560f-151">Feltárva hello jelszó alaphelyzetbe állítása sikertelen hello SSPR tölcsér használatával, és megismerheti a miért bizonyos hibák történtek-e.</span><span class="sxs-lookup"><span data-stu-id="b560f-151">Dig deeper into hello Password resets failure using hello SSPR funnel and understand why certain failures occurred.</span></span> <span data-ttu-id="b560f-152">Ez a jelentés hello SSPR eszköz használatáról a szervezeten belül, hogy jobb döntéseket hello bemutatják tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b560f-152">This report gives a deeper understanding of how hello SSPR tool is used within your organization so you can make hello right decisions.</span></span>

## <a name="customizing-azure-ad-activity-content-pack"></a><span data-ttu-id="b560f-153">Az Azure AD-tevékenység tartalomcsomag személyre szabása</span><span class="sxs-lookup"><span data-stu-id="b560f-153">Customizing Azure AD Activity content pack</span></span>

<span data-ttu-id="b560f-154">**Módosíthatja a képi megjelenítés**: kattintva módosíthatja a képi megjelenítés jelentés **jelentés szerkesztése** válassza ki a kívánt hello képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="b560f-154">**Change Visualization**:  You can change a report visualization by clicking **Edit Report** and select hello visualization you want.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

<span data-ttu-id="b560f-157">**További mezők szerepelhetnek,**: mező toohello jelentés hozzáadása, vagy távolítsa el a hello visual toowhich tooadd/eltávolítása hello mező kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="b560f-157">**Include additional fields**:  You can add a field toohello report or remove it by selecting hello visual toowhich you want tooadd/remove hello field.</span></span> <span data-ttu-id="b560f-158">Hello az alábbi példában a "bejelentkezés" mező toohello tábla nézet hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="b560f-158">In hello example below, I am adding “sign-in status” field toohello table view.</span></span> 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

<span data-ttu-id="b560f-160">**PIN-kód képi megjelenítések tooyour irányítópult**: testre szabhatja az irányítópultot és a saját képi megjelenítések toohello jelentéssel és toohello irányítópulton rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="b560f-160">**Pin visualizations tooyour dashboard**:  You can customize your dashboard and include your own visualizations toohello report and pin it toohello dashboard.</span></span> <span data-ttu-id="b560f-161">A hello az alábbi példában I "bejelentkezési állapot" nevű új szűrő hozzáadott és hello jelentésben szereplő.</span><span class="sxs-lookup"><span data-stu-id="b560f-161">In hello example below, I added a new filter called “Sign-in Status” and included it in hello report.</span></span> <span data-ttu-id="b560f-162">I is hello képi megjelenítés változása sávdiagram tooa vonaldiagram, és az új visual toohello irányítópult rögzíthető.</span><span class="sxs-lookup"><span data-stu-id="b560f-162">I also changed hello visualization from bar chart tooa line chart and can pin this new visual toohello dashboard.</span></span>

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


<span data-ttu-id="b560f-165">**Az irányítópult megosztása**: Miután létrehozta a kívánt hello tartalmat, megoszthatja hello irányítópult hello felhasználókat a szervezetben.</span><span class="sxs-lookup"><span data-stu-id="b560f-165">**Sharing your dashboard**: Once you have created hello content you want, you can share hello dashboard with hello users in your organization.</span></span> <span data-ttu-id="b560f-166">Ne feledje, hogy miután hello jelentés megosztásához láthatják hello jelentésben kijelölt hello mezőt.</span><span class="sxs-lookup"><span data-stu-id="b560f-166">Please remember that once you share hello report, they can see hello fields you have selected in hello report.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a><span data-ttu-id="b560f-168">A Power BI-jelentés napi frissítésének ütemezése</span><span class="sxs-lookup"><span data-stu-id="b560f-168">Scheduling a daily refresh of your Power BI report</span></span>

<span data-ttu-id="b560f-169">túl nyissa meg a Power BI-jelentés napi frissítését tooschedule**adatkészletek > Beállítások > ütemezés frissítése** és állítsa be az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="b560f-169">tooschedule a daily refresh of your Power BI report, go too**Datasets > Settings > Schedule Refresh** and set it as shown below.</span></span>
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-toonewer-version-of-content-pack"></a><span data-ttu-id="b560f-171">Tartalomcsomag toonewer verziójának frissítése</span><span class="sxs-lookup"><span data-stu-id="b560f-171">Updating toonewer version of content pack</span></span>

<span data-ttu-id="b560f-172">Ha azt szeretné, hogy tooupdate a tartalom pack tooget egy újabb verzióra:</span><span class="sxs-lookup"><span data-stu-id="b560f-172">If you want tooupdate your content pack tooget a newer version:</span></span>

- <span data-ttu-id="b560f-173">Töltse le a hello új csomagot, és állítsa be a cikkben szereplő utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="b560f-173">Download hello new content pack and set it up as per instructions listed in this article.</span></span>

- <span data-ttu-id="b560f-174">Miután állított be, lépjen túl**adatforrás > Beállítások > adatforrás hitelesítő adatait** írja be újra a hitelesítő adatok alább látható módon</span><span class="sxs-lookup"><span data-stu-id="b560f-174">Once you have set it up, go too**Data Source > Settings > Data source credentials** and re-enter your credentials as shown below</span></span>

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

<span data-ttu-id="b560f-176">Amint hello hello tartalomcsomag új verziója nem működik, eltávolíthatja hello régi verziója, hello alapul szolgáló jelentések és adatkészletek adott tartalomcsomag társított törlésével szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="b560f-176">As soon as hello new version of hello content pack is working, you can remove hello old version if needed by deleting hello underlying reports and datasets associated with that content pack.</span></span>

## <a name="still-having-issues"></a><span data-ttu-id="b560f-177">Továbbra is problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="b560f-177">Still having issues?</span></span> 

<span data-ttu-id="b560f-178">Tekintse meg a [hibaelhárítási útmutatót](active-directory-reporting-troubleshoot-content-pack.md).</span><span class="sxs-lookup"><span data-stu-id="b560f-178">Check out our [troubleshooting guide](active-directory-reporting-troubleshoot-content-pack.md).</span></span> <span data-ttu-id="b560f-179">A Power BI-jal kapcsolatos általános útmutatásért tekintse meg ezeket a [súgótémaköröket](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span><span class="sxs-lookup"><span data-stu-id="b560f-179">For general help with Power BI, check out these [help articles](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span></span>
 

## <a name="next-steps"></a><span data-ttu-id="b560f-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b560f-180">Next steps</span></span>

<span data-ttu-id="b560f-181">Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b560f-181">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
