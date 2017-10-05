---
title: "Azokkal a Mobile Engagement-REST API-k - manuális beállítása"
description: "Ismerteti, hogyan lehet manuálisan a Mobile Engagement REST API-k hitelesítés beállítása"
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
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a><span data-ttu-id="08105-103">Azokkal a Mobile Engagement-REST API-k - manuális beállítása</span><span class="sxs-lookup"><span data-stu-id="08105-103">Authenticate with Mobile Engagement REST APIs - manual setup</span></span>
<span data-ttu-id="08105-104">Ez egy függelék dokumentációt, amelyik az [hitelesítés a Mobile Engagement REST API-k](mobile-engagement-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="08105-104">This is an appendix documentation to [Authenticate with Mobile Engagement REST APIs](mobile-engagement-api-authentication.md).</span></span> <span data-ttu-id="08105-105">Győződjön meg arról, hogy elolvasta, a környezet használatának.</span><span class="sxs-lookup"><span data-stu-id="08105-105">Make sure you read it first to get the context.</span></span> <span data-ttu-id="08105-106">Ez azt ismerteti, hogy egy másik módja a hitelesítés a Mobile Engagement REST API-kat az Azure portál használata az egyszeri telepítőprogramja.</span><span class="sxs-lookup"><span data-stu-id="08105-106">This describes an alternate way to do the One-time setup for setting up your authentication for the Mobile Engagement REST APIs using the Azure Portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="08105-107">Az alábbi utasításokat a alapuló [útmutató az Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) és testre szabható a Mobile Engagement API-k hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="08105-107">The instructions below are based on this [Active Directory guide](../azure-resource-manager/resource-group-create-service-principal-portal.md) and customized for what is required for authentication for Mobile Engagement APIs.</span></span> <span data-ttu-id="08105-108">Ezért tekintse meg, ha szeretné tudni, hogy az alábbi lépéseket részletesen.</span><span class="sxs-lookup"><span data-stu-id="08105-108">So refer to it if you want to understand the steps below in detail.</span></span> 
> 
> 

1. <span data-ttu-id="08105-109">Jelentkezzen be az Azure-fiók keresztül a [klasszikus portál](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="08105-109">Login to your Azure Account through the [classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="08105-110">Válassza ki **Active Directory** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="08105-110">Select **Active Directory** from the left pane.</span></span>
   
     ![az Active Directory kiválasztása][1]
3. <span data-ttu-id="08105-112">Válassza ki a **Active Directory alapértelmezett** az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="08105-112">Choose the **Default Active Directory** in your Azure portal.</span></span> 
   
     ![Válassza ki a könyvtár][2]
   
   > [!IMPORTANT]
   > <span data-ttu-id="08105-114">Ez a módszer csak akkor, ha az Active Directory-fiókja alapértelmezett dolgozik, és nem fog működni, akkor használatos, ha ez egy Active Directory fiókjában létrehozott működik.</span><span class="sxs-lookup"><span data-stu-id="08105-114">This approach works only when you are working in the default Active Directory of your account and will not work if you are doing this in an Active Directory that you have created in your account.</span></span> 
   > 
   > 
4. <span data-ttu-id="08105-115">A könyvtárban az alkalmazások megtekintéséhez kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="08105-115">To view the applications in your directory, click on **Applications**.</span></span>
   
     ![alkalmazások megtekintése][3]
5. <span data-ttu-id="08105-117">Kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="08105-117">Click on **ADD**.</span></span> 
   
     ![Alkalmazás hozzáadása][4]
6. <span data-ttu-id="08105-119">Kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="08105-119">Click on **Add an application my organization is developing**</span></span>
   
     ![Új alkalmazás][5]
7. <span data-ttu-id="08105-121">Adja meg az alkalmazás nevét, és adja meg, amelyeket **WEB APPLICATION AND/OR WEB API** , majd kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="08105-121">Fill in name of the application and select the type of application as **WEB APPLICATION AND/OR WEB API** and click the next button.</span></span>
   
     ![alkalmazás neve][6]
8. <span data-ttu-id="08105-123">Megadhatja, hogy bármilyen típusú URL-címéből **SIGN-ON URL** és **APP ID URI**.</span><span class="sxs-lookup"><span data-stu-id="08105-123">You can provide any dummy URLs for **SIGN-ON URL** and **APP ID URI**.</span></span> <span data-ttu-id="08105-124">A mi esetünkben nem használhatók, és maguk URL-címeket a rendszer nem érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="08105-124">They are not used for our scenario and the URLs themselves are not validated.</span></span>  
   
     ![Alkalmazás tulajdonságai][7]
9. <span data-ttu-id="08105-126">Ez végén kell egy AAD-alkalmazás a következő korábban megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="08105-126">At the end of this, you will have an AAD app with the name you provided previously like the following.</span></span> <span data-ttu-id="08105-127">Ez a **AD\_APP\_neve** és jegyezze fel a azt.</span><span class="sxs-lookup"><span data-stu-id="08105-127">This is your **AD\_APP\_NAME** and make a note of it.</span></span>  
   
     ![Alkalmazás neve][8]
10. <span data-ttu-id="08105-129">Kattintson az alkalmazás nevére, majd kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="08105-129">Click on the app name and click on **Configure**.</span></span>
    
      ![alkalmazás konfigurálása][9]
11. <span data-ttu-id="08105-131">Jegyezze fel a használandó ügyfél-azonosító **ügyfél\_azonosító** az API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="08105-131">Make a note of the CLIENT ID that will be used as **CLIENT\_ID** for your API calls.</span></span> 
    
     ![alkalmazás konfigurálása][10]
12. <span data-ttu-id="08105-133">Görgessen le a **kulcsok** szakaszt, és adja hozzá a 2 év (lejárati) időtartama lehetőleg kulcsot, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="08105-133">Scroll down to the **Keys** section and add a key with preferably 2 years (expiry) duration and click **Save**.</span></span> 
    
     ![alkalmazás konfigurálása][11]
13. <span data-ttu-id="08105-135">Azonnal kimásolhatja az értéket, a kulcs csak akkor jelenik meg most és nem tárolja, nem fog megjelenni egyre újra látható.</span><span class="sxs-lookup"><span data-stu-id="08105-135">Immediately copy the value which is shown for the key as it is only shown now and is not stored so will not be displayed ever again.</span></span> <span data-ttu-id="08105-136">Ha az elveszített majd kell hozzon létre egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="08105-136">If you lose it then you will have to generate a new key.</span></span> <span data-ttu-id="08105-137">Ez lesz a **CLIENT_SECRET** az API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="08105-137">This will be the **CLIENT_SECRET** for your API calls.</span></span> 
    
     ![alkalmazás konfigurálása][12]
    
    > [!IMPORTANT]
    > <span data-ttu-id="08105-139">Ezt a kulcsot a, ügyeljen rá, hogy újítsa meg, ha a idő ellenkező esetben az API-hitelesítés nem fog többé működni megadott időtartamot végén lejár.</span><span class="sxs-lookup"><span data-stu-id="08105-139">This key will expire at the end of the duration that you specified so make sure to renew it when the time comes otherwise your API authentication will not work anymore.</span></span> <span data-ttu-id="08105-140">Töröl, és hozza létre újból ezt a kulcsot, ha úgy gondolja, hogy veszélyben van.</span><span class="sxs-lookup"><span data-stu-id="08105-140">You can also delete and recreate this key if you think that it has been compromised.</span></span>
    > 
    > 
14. <span data-ttu-id="08105-141">Kattintson a **VÉGPONTOK megtekintése** gombra kattint, most amely kattintva megnyílik a **App végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="08105-141">Click on **VIEW ENDPOINTS** button now which will open up the **App Endpoints** dialog box.</span></span> 
    
    ![][13]
15. <span data-ttu-id="08105-142">Az alkalmazás végpontok párbeszédpanel, másolja a **OAUTH 2.0 TOKEN-VÉGPONT**.</span><span class="sxs-lookup"><span data-stu-id="08105-142">From the App Endpoints dialog box, copy the **OAUTH 2.0 TOKEN ENDPOINT**.</span></span> 
    
    ![][14]
16. <span data-ttu-id="08105-143">A végpont a következő képernyőn a GUID az URL-cím lesz a **TENANT_ID** , jegyezze fel a azt:</span><span class="sxs-lookup"><span data-stu-id="08105-143">This endpoint will be in the following form where the GUID in the URL is your **TENANT_ID** so make a note of it:</span></span> 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. <span data-ttu-id="08105-144">Most azt folytatódik az engedélyek beállításához meg ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="08105-144">Now we will proceed to configure the permissions on this app.</span></span> <span data-ttu-id="08105-145">Ehhez meg kell nyitnia a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08105-145">For this you will have to open up the [Azure portal](https://portal.azure.com).</span></span> 
18. <span data-ttu-id="08105-146">Kattintson a **erőforráscsoportok** keresse meg a **a Mobile Engagement** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="08105-146">Click on **Resource Groups** and find the **Mobile Engagement** resource group.</span></span>  
    
    ![][15]
19. <span data-ttu-id="08105-147">Kattintson a **a Mobile Engagement** erőforrás csoportban, és keresse meg a **beállítások** itt panelen.</span><span class="sxs-lookup"><span data-stu-id="08105-147">Click the **Mobile Engagement** resource group and navigate to the **Settings** blade here.</span></span> 
    
    ![][16]
20. <span data-ttu-id="08105-148">Kattintson a **felhasználók** a beállítások panelen, és kattintson a **Hozzáadás** hozzáadni egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="08105-148">Click on **Users** in the Settings blade and then click on **Add** to add a user.</span></span> 
    
    ![][17]
21. <span data-ttu-id="08105-149">Kattintson a **szerepkör kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="08105-149">Click on **Select a role**</span></span>
    
    ![][18]
22. <span data-ttu-id="08105-150">Kattintson a **tulajdonosa**</span><span class="sxs-lookup"><span data-stu-id="08105-150">Click on **Owner**</span></span>
    
    ![][19]
23. <span data-ttu-id="08105-151">Keresse meg az alkalmazás neve **AD\_APP\_neve** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="08105-151">Search for the name of your application **AD\_APP\_NAME** in the Search box.</span></span> <span data-ttu-id="08105-152">Nem látják ezt itt alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="08105-152">You will not see this by default here.</span></span> <span data-ttu-id="08105-153">Miután megtalálta, válassza ki azt, majd kattintson a **válasszon** a panel alján.</span><span class="sxs-lookup"><span data-stu-id="08105-153">Once you find it, select it and click on **Select** at the bottom of the blade.</span></span> 
    
    ![][20]
24. <span data-ttu-id="08105-154">Az a **hozzáférés hozzáadása** panelen megjelenik **1 felhasználó, a 0 csoportok**.</span><span class="sxs-lookup"><span data-stu-id="08105-154">On the **Add Access** blade, it will show up as **1 user, 0 groups**.</span></span> <span data-ttu-id="08105-155">Kattintson a **OK** a panel a módosítás megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="08105-155">Click **OK** on this blade to confirm the change.</span></span> 
    
    ![][21]

<span data-ttu-id="08105-156">Ezennel befejezte a szükséges AAD-konfigurációt, és a rendszer összes megadta az API-k meghívásához.</span><span class="sxs-lookup"><span data-stu-id="08105-156">You have now completed the required AAD configuration and you are all set to call the APIs.</span></span> 

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



