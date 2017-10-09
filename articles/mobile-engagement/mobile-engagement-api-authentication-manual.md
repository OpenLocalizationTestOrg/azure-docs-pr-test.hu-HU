---
title: "a Mobile Engagement-REST API-k - manuális telepítési módra aaaAuthenticate"
description: "Ismerteti, hogyan toomanually beállítani a Mobile Engagement REST API-k hitelesítés"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a><span data-ttu-id="fa94a-103">Azokkal a Mobile Engagement-REST API-k - manuális beállítása</span><span class="sxs-lookup"><span data-stu-id="fa94a-103">Authenticate with Mobile Engagement REST APIs - manual setup</span></span>
<span data-ttu-id="fa94a-104">Ez a egy függelék dokumentáció túl[hitelesítés a Mobile Engagement REST API-k](mobile-engagement-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="fa94a-104">This is an appendix documentation too[Authenticate with Mobile Engagement REST APIs](mobile-engagement-api-authentication.md).</span></span> <span data-ttu-id="fa94a-105">Győződjön meg arról, hogy olvassa el, az első tooget hello környezetben.</span><span class="sxs-lookup"><span data-stu-id="fa94a-105">Make sure you read it first tooget hello context.</span></span> <span data-ttu-id="fa94a-106">Ez ez a cikk ismerteti egy másik módja toodo hello egyszeri telepítőprogramja a hitelesítés beállítása hello Mobile Engagement REST API-k használatával hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="fa94a-106">This describes an alternate way toodo hello One-time setup for setting up your authentication for hello Mobile Engagement REST APIs using hello Azure Portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="fa94a-107">az alábbi utasítások hello ez alapuló [útmutató az Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) és testre szabható a Mobile Engagement API-k hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="fa94a-107">hello instructions below are based on this [Active Directory guide](../azure-resource-manager/resource-group-create-service-principal-portal.md) and customized for what is required for authentication for Mobile Engagement APIs.</span></span> <span data-ttu-id="fa94a-108">Ezért tekintse meg tooit, ha azt szeretné, hogy toounderstand hello alábbi lépéseket részletesen.</span><span class="sxs-lookup"><span data-stu-id="fa94a-108">So refer tooit if you want toounderstand hello steps below in detail.</span></span> 
> 
> 

1. <span data-ttu-id="fa94a-109">Bejelentkezési tooyour Azure-fiók keresztül hello [klasszikus portál](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="fa94a-109">Login tooyour Azure Account through hello [classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="fa94a-110">Válassza ki **Active Directory** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="fa94a-110">Select **Active Directory** from hello left pane.</span></span>
   
     ![az Active Directory kiválasztása][1]
3. <span data-ttu-id="fa94a-112">Válassza ki a hello **Active Directory alapértelmezett** az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fa94a-112">Choose hello **Default Active Directory** in your Azure portal.</span></span> 
   
     ![Válassza ki a könyvtár][2]
   
   > [!IMPORTANT]
   > <span data-ttu-id="fa94a-114">Ez a módszer csak akkor, ha az Active Directory-fiókja hello alapértelmezett dolgozik, és nem fog működni, akkor használatos, ha ez egy Active Directory fiókjában létrehozott működik.</span><span class="sxs-lookup"><span data-stu-id="fa94a-114">This approach works only when you are working in hello default Active Directory of your account and will not work if you are doing this in an Active Directory that you have created in your account.</span></span> 
   > 
   > 
4. <span data-ttu-id="fa94a-115">tooview hello alkalmazások a könyvtárban kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-115">tooview hello applications in your directory, click on **Applications**.</span></span>
   
     ![alkalmazások megtekintése][3]
5. <span data-ttu-id="fa94a-117">Kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-117">Click on **ADD**.</span></span> 
   
     ![Alkalmazás hozzáadása][4]
6. <span data-ttu-id="fa94a-119">Kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="fa94a-119">Click on **Add an application my organization is developing**</span></span>
   
     ![Új alkalmazás][5]
7. <span data-ttu-id="fa94a-121">Adja meg a hello alkalmazás és az alkalmazás válassza hello típusú nevét **WEB APPLICATION AND/OR WEB API** és hello Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="fa94a-121">Fill in name of hello application and select hello type of application as **WEB APPLICATION AND/OR WEB API** and click hello next button.</span></span>
   
     ![alkalmazás neve][6]
8. <span data-ttu-id="fa94a-123">Megadhatja, hogy bármilyen típusú URL-címéből **SIGN-ON URL** és **APP ID URI**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-123">You can provide any dummy URLs for **SIGN-ON URL** and **APP ID URI**.</span></span> <span data-ttu-id="fa94a-124">A mi esetünkben nem használhatók, és maguk hello URL-címeket a rendszer nem érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="fa94a-124">They are not used for our scenario and hello URLs themselves are not validated.</span></span>  
   
     ![Alkalmazás tulajdonságai][7]
9. <span data-ttu-id="fa94a-126">Ezen hello végén kell hasonló hello korábban megadott hello nevű alkalmazás aad-ben.</span><span class="sxs-lookup"><span data-stu-id="fa94a-126">At hello end of this, you will have an AAD app with hello name you provided previously like hello following.</span></span> <span data-ttu-id="fa94a-127">Ez a **AD\_APP\_neve** és jegyezze fel a azt.</span><span class="sxs-lookup"><span data-stu-id="fa94a-127">This is your **AD\_APP\_NAME** and make a note of it.</span></span>  
   
     ![Alkalmazás neve][8]
10. <span data-ttu-id="fa94a-129">Kattintson a hello alkalmazás nevére, majd kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-129">Click on hello app name and click on **Configure**.</span></span>
    
      ![alkalmazás konfigurálása][9]
11. <span data-ttu-id="fa94a-131">Jegyezze fel a használandó ügyfél-azonosító hello **ügyfél\_azonosító** az API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="fa94a-131">Make a note of hello CLIENT ID that will be used as **CLIENT\_ID** for your API calls.</span></span> 
    
     ![alkalmazás konfigurálása][10]
12. <span data-ttu-id="fa94a-133">Görgessen lefelé toohello **kulcsok** szakaszt, és adja hozzá a 2 év (lejárati) időtartama lehetőleg kulcsot, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-133">Scroll down toohello **Keys** section and add a key with preferably 2 years (expiry) duration and click **Save**.</span></span> 
    
     ![alkalmazás konfigurálása][11]
13. <span data-ttu-id="fa94a-135">Azonnal másolási hello érték látható hello kulcs csak akkor jelenik meg most és nem tárolja, nem fog megjelenni egyre újra.</span><span class="sxs-lookup"><span data-stu-id="fa94a-135">Immediately copy hello value which is shown for hello key as it is only shown now and is not stored so will not be displayed ever again.</span></span> <span data-ttu-id="fa94a-136">Ha elveszített majd akkor toogenerate egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fa94a-136">If you lose it then you will have toogenerate a new key.</span></span> <span data-ttu-id="fa94a-137">Ez lesz a hello **CLIENT_SECRET** az API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="fa94a-137">This will be hello **CLIENT_SECRET** for your API calls.</span></span> 
    
     ![alkalmazás konfigurálása][12]
    
    > [!IMPORTANT]
    > <span data-ttu-id="fa94a-139">Ezt a kulcsot, hogy a megadott igen győződjön meg arról, hogy toorenew, ha hello idő ellenkező esetben az API-hitelesítés nem fog többé működni hello végén lévő hello időtartama lejár.</span><span class="sxs-lookup"><span data-stu-id="fa94a-139">This key will expire at hello end of hello duration that you specified so make sure toorenew it when hello time comes otherwise your API authentication will not work anymore.</span></span> <span data-ttu-id="fa94a-140">Töröl, és hozza létre újból ezt a kulcsot, ha úgy gondolja, hogy veszélyben van.</span><span class="sxs-lookup"><span data-stu-id="fa94a-140">You can also delete and recreate this key if you think that it has been compromised.</span></span>
    > 
    > 
14. <span data-ttu-id="fa94a-141">Kattintson a **VÉGPONTOK megtekintése** gombra kattint, most amely hello megnyílik **App végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fa94a-141">Click on **VIEW ENDPOINTS** button now which will open up hello **App Endpoints** dialog box.</span></span> 
    
    ![][13]
15. <span data-ttu-id="fa94a-142">Hello App végpontok párbeszédpanel, másolja a hello **OAUTH 2.0 TOKEN-VÉGPONT**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-142">From hello App Endpoints dialog box, copy hello **OAUTH 2.0 TOKEN ENDPOINT**.</span></span> 
    
    ![][14]
16. <span data-ttu-id="fa94a-143">Ez a végpont kerül, ahol hello GUID hello URL-címben az űrlap következő hello a **TENANT_ID** , jegyezze fel a azt:</span><span class="sxs-lookup"><span data-stu-id="fa94a-143">This endpoint will be in hello following form where hello GUID in hello URL is your **TENANT_ID** so make a note of it:</span></span> 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. <span data-ttu-id="fa94a-144">Most azt folytatódik tooconfigure hello engedélyeinek ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa94a-144">Now we will proceed tooconfigure hello permissions on this app.</span></span> <span data-ttu-id="fa94a-145">Ez tooopen hello fel lesz [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa94a-145">For this you will have tooopen up hello [Azure portal](https://portal.azure.com).</span></span> 
18. <span data-ttu-id="fa94a-146">Kattintson a **erőforráscsoportok** és hello található **a Mobile Engagement** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="fa94a-146">Click on **Resource Groups** and find hello **Mobile Engagement** resource group.</span></span>  
    
    ![][15]
19. <span data-ttu-id="fa94a-147">Hello kattintson **a Mobile Engagement** erőforrás csoportban, és keresse meg a toohello **beállítások** itt panelen.</span><span class="sxs-lookup"><span data-stu-id="fa94a-147">Click hello **Mobile Engagement** resource group and navigate toohello **Settings** blade here.</span></span> 
    
    ![][16]
20. <span data-ttu-id="fa94a-148">Kattintson a **felhasználók** a hello beállítások panelen, majd kattintson a **Hozzáadás** tooadd a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="fa94a-148">Click on **Users** in hello Settings blade and then click on **Add** tooadd a user.</span></span> 
    
    ![][17]
21. <span data-ttu-id="fa94a-149">Kattintson a **szerepkör kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="fa94a-149">Click on **Select a role**</span></span>
    
    ![][18]
22. <span data-ttu-id="fa94a-150">Kattintson a **tulajdonosa**</span><span class="sxs-lookup"><span data-stu-id="fa94a-150">Click on **Owner**</span></span>
    
    ![][19]
23. <span data-ttu-id="fa94a-151">Keresse meg az alkalmazás nevére hello **AD\_APP\_neve** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fa94a-151">Search for hello name of your application **AD\_APP\_NAME** in hello Search box.</span></span> <span data-ttu-id="fa94a-152">Nem látják ezt itt alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="fa94a-152">You will not see this by default here.</span></span> <span data-ttu-id="fa94a-153">Miután megtalálta, válassza ki azt, majd kattintson a **válasszon** hello hello panel alsó részén.</span><span class="sxs-lookup"><span data-stu-id="fa94a-153">Once you find it, select it and click on **Select** at hello bottom of hello blade.</span></span> 
    
    ![][20]
24. <span data-ttu-id="fa94a-154">A hello **hozzáférés hozzáadása** panelen megjelenik **1 felhasználó, a 0 csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fa94a-154">On hello **Add Access** blade, it will show up as **1 user, 0 groups**.</span></span> <span data-ttu-id="fa94a-155">Kattintson a **OK** a panel tooconfirm hello változás.</span><span class="sxs-lookup"><span data-stu-id="fa94a-155">Click **OK** on this blade tooconfirm hello change.</span></span> 
    
    ![][21]

<span data-ttu-id="fa94a-156">Ezzel befejezte a szükséges hello AAD-konfigurációt, és minden set toocall hello API-k.</span><span class="sxs-lookup"><span data-stu-id="fa94a-156">You have now completed hello required AAD configuration and you are all set toocall hello APIs.</span></span> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



