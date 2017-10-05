---
title: "Az App Service szolgáltatások alkalmazás Microsoft Account hitelesítés konfigurálása"
description: "Megtudhatja, hogyan konfigurálja az App Service szolgáltatások alkalmazás Microsoft Account hitelesítést."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 67386b03ae4cc683fe00e11e8dad19d1442eff09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a><span data-ttu-id="754f9-103">Az App Service alkalmazás használatához Microsoft Account bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="754f9-103">How to configure your App Service application to use Microsoft Account login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="754f9-104">Ez a témakör bemutatja, hogyan konfigurálhatja az Azure App Service-nek Microsoft Account használja, mint egy hitelesítésszolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="754f9-104">This topic shows you how to configure Azure App Service to use Microsoft Account as an authentication provider.</span></span> 

## <span data-ttu-id="754f9-105"><a name="register-microsoft-account"></a>Regisztrálja az alkalmazást a Microsoft-fiók</span><span class="sxs-lookup"><span data-stu-id="754f9-105"><a name="register-microsoft-account"> </a>Register your app with Microsoft Account</span></span>
1. <span data-ttu-id="754f9-106">Jelentkezzen be a [Azure-portálon], és keresse meg az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="754f9-106">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="754f9-107">Másolás a **URL-cím**, amelyet később az alkalmazás konfigurálása a Microsoft Account használhat.</span><span class="sxs-lookup"><span data-stu-id="754f9-107">Copy your **URL**, which later you use to configure your app with Microsoft Account.</span></span>
2. <span data-ttu-id="754f9-108">Keresse meg a [saját alkalmazások] a Microsoft Account Developer Center weblapjára, és jelentkezzen be Microsoft-fiókjával, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="754f9-108">Navigate to the [My Applications] page in the Microsoft Account Developer Center, and log on with your Microsoft account, if required.</span></span>
3. <span data-ttu-id="754f9-109">Kattintson a **hozzáadhat egy alkalmazást**, majd írja be az alkalmazás nevét, majd kattintson **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="754f9-109">Click **Add an app**, then type an application name, and click **Create application**.</span></span>
4. <span data-ttu-id="754f9-110">Jegyezze fel a **Alkalmazásazonosító**, mert később szüksége lesz az.</span><span class="sxs-lookup"><span data-stu-id="754f9-110">Make a note of the **Application ID**, as you will need it later.</span></span> 
5. <span data-ttu-id="754f9-111">Kattintson a "Platformok," **hozzáadása Platform** válassza ki a "Web".</span><span class="sxs-lookup"><span data-stu-id="754f9-111">Under "Platforms," click **Add Platform** and select "Web".</span></span>
6. <span data-ttu-id="754f9-112">A "Átirányítási URI-k" adja meg a végpont az alkalmazáshoz, majd kattintson az **mentése**.</span><span class="sxs-lookup"><span data-stu-id="754f9-112">Under "Redirect URIs" supply the endpoint for your application, then click **Save**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="754f9-113">Az átirányítási URI megadása az alkalmazás fűzhető hozzá az elérési út az URL-cím */.auth/login/microsoftaccount/callback*.</span><span class="sxs-lookup"><span data-stu-id="754f9-113">Your redirect URI is the URL of your application appended with the path, */.auth/login/microsoftaccount/callback*.</span></span> <span data-ttu-id="754f9-114">Például: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span><span class="sxs-lookup"><span data-stu-id="754f9-114">For example, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span></span>   
   > <span data-ttu-id="754f9-115">Győződjön meg arról, hogy a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="754f9-115">Make sure that you are using the HTTPS scheme.</span></span>
   
7. <span data-ttu-id="754f9-116">Kattintson a "Titkos kulcsok alkalmazás," **új jelszó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="754f9-116">Under "Application Secrets," click **Generate New Password**.</span></span> <span data-ttu-id="754f9-117">Jegyezze fel az érték, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="754f9-117">Make note of the value that appears.</span></span> <span data-ttu-id="754f9-118">Ha elhagyja a lapot, akkor nem jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="754f9-118">Once you leave the page, it will not be displayed again.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="754f9-119">A jelszó nem egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="754f9-119">The password is an important security credential.</span></span> <span data-ttu-id="754f9-120">Ne ossza meg senkivel a jelszót, vagy eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="754f9-120">Do not share the password with anyone or distribute it within a client application.</span></span>

## <span data-ttu-id="754f9-121"><a name="secrets"></a>Az App Service alkalmazás azon Microsoft-fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="754f9-121"><a name="secrets"> </a>Add Microsoft Account information to your App Service application</span></span>
1. <span data-ttu-id="754f9-122">Vissza a [Azure-portálon], keresse meg az alkalmazás, kattintson a **beállítások** > **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="754f9-122">Back in the [Azure portal], navigate to your application, click **Settings** > **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="754f9-123">Ha a hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja **a**.</span><span class="sxs-lookup"><span data-stu-id="754f9-123">If the Authentication / Authorization feature is not enabled, switch it **On**.</span></span>
3. <span data-ttu-id="754f9-124">Kattintson a **Microsoft-fiók**.</span><span class="sxs-lookup"><span data-stu-id="754f9-124">Click **Microsoft Account**.</span></span> <span data-ttu-id="754f9-125">Illessze be az alkalmazás Azonosítóját és jelszavát értékek, amelyek korábban beszerzett, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök.</span><span class="sxs-lookup"><span data-stu-id="754f9-125">Paste in the Application ID and Password values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="754f9-126">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="754f9-126">Then click **OK**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="754f9-127">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem engedélyezett hozzáférés korlátozása a tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="754f9-127">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="754f9-128">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="754f9-128">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="754f9-129">(Választható) Csak a Microsoft-fiók által hitelesített felhasználók a helyhez való hozzáférésének korlátozásához, állítsa be **hitelesítetlen kérés esetén elvégzendő művelet** való **Microsoft Account**.</span><span class="sxs-lookup"><span data-stu-id="754f9-129">(Optional) To restrict access to your site to only users authenticated by Microsoft account, set **Action to take when request is not authenticated** to **Microsoft Account**.</span></span> <span data-ttu-id="754f9-130">Ehhez szükséges összes kérelmet hitelesíteni, és minden nem hitelesített kérelmek Microsoft-fiók hitelesítési van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="754f9-130">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Microsoft account for authentication.</span></span>
5. <span data-ttu-id="754f9-131">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="754f9-131">Click **Save**.</span></span>

<span data-ttu-id="754f9-132">Most már készen áll a Microsoft Account használja a hitelesítéshez, az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="754f9-132">You are now ready to use Microsoft Account for authentication in your app.</span></span>

## <span data-ttu-id="754f9-133"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="754f9-133"><a name="related-content"> </a>Related content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[saját alkalmazások]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portálon]: https://portal.azure.com/
