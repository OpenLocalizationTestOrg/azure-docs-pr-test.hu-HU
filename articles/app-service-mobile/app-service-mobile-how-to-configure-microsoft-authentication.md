---
title: "aaaHow tooconfigure Microsoft Account hitelesítése alkalmazás App Service szolgáltatások"
description: "Megtudhatja, hogyan tooconfigure Microsoft Account hitelesítése az App Service szolgáltatások alkalmazáshoz."
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
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a><span data-ttu-id="07132-103">Hogyan tooconfigure App Service alkalmazás toouse Microsoft Account bejelentkezési adatait</span><span class="sxs-lookup"><span data-stu-id="07132-103">How tooconfigure your App Service application toouse Microsoft Account login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="07132-104">Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Microsoft Account egy hitelesítésszolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="07132-104">This topic shows you how tooconfigure Azure App Service toouse Microsoft Account as an authentication provider.</span></span> 

## <span data-ttu-id="07132-105"><a name="register-microsoft-account"></a>Regisztrálja az alkalmazást a Microsoft-fiók</span><span class="sxs-lookup"><span data-stu-id="07132-105"><a name="register-microsoft-account"> </a>Register your app with Microsoft Account</span></span>
1. <span data-ttu-id="07132-106">Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="07132-106">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="07132-107">Másolás a **URL-cím**, mely később tooconfigure az alkalmazás használatát a Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="07132-107">Copy your **URL**, which later you use tooconfigure your app with Microsoft Account.</span></span>
2. <span data-ttu-id="07132-108">Keresse meg a toohello [saját alkalmazások] hello Microsoft Account Developer Center weblapjára, és jelentkezzen be Microsoft-fiókjával, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="07132-108">Navigate toohello [My Applications] page in hello Microsoft Account Developer Center, and log on with your Microsoft account, if required.</span></span>
3. <span data-ttu-id="07132-109">Kattintson a **hozzáadhat egy alkalmazást**, majd írja be az alkalmazás nevét, majd kattintson **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="07132-109">Click **Add an app**, then type an application name, and click **Create application**.</span></span>
4. <span data-ttu-id="07132-110">Jegyezze fel a hello **Alkalmazásazonosító**, mert később szüksége lesz az.</span><span class="sxs-lookup"><span data-stu-id="07132-110">Make a note of hello **Application ID**, as you will need it later.</span></span> 
5. <span data-ttu-id="07132-111">Kattintson a "Platformok," **hozzáadása Platform** válassza ki a "Web".</span><span class="sxs-lookup"><span data-stu-id="07132-111">Under "Platforms," click **Add Platform** and select "Web".</span></span>
6. <span data-ttu-id="07132-112">A "Átirányítási URI-k" ellátási hello végpont az alkalmazáshoz, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="07132-112">Under "Redirect URIs" supply hello endpoint for your application, then click **Save**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="07132-113">Az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési */.auth/login/microsoftaccount/callback*.</span><span class="sxs-lookup"><span data-stu-id="07132-113">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/microsoftaccount/callback*.</span></span> <span data-ttu-id="07132-114">Például: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span><span class="sxs-lookup"><span data-stu-id="07132-114">For example, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span></span>   
   > <span data-ttu-id="07132-115">Győződjön meg arról, hogy hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="07132-115">Make sure that you are using hello HTTPS scheme.</span></span>
   
7. <span data-ttu-id="07132-116">Kattintson a "Titkos kulcsok alkalmazás," **új jelszó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="07132-116">Under "Application Secrets," click **Generate New Password**.</span></span> <span data-ttu-id="07132-117">Jegyezze fel a hello érték, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="07132-117">Make note of hello value that appears.</span></span> <span data-ttu-id="07132-118">Hello lapon adja meg, ha az nem jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="07132-118">Once you leave hello page, it will not be displayed again.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="07132-119">hello jelszava egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="07132-119">hello password is an important security credential.</span></span> <span data-ttu-id="07132-120">Ne ossza meg senkivel hello jelszó vagy eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="07132-120">Do not share hello password with anyone or distribute it within a client application.</span></span>

## <span data-ttu-id="07132-121"><a name="secrets"></a>Microsoft-fiók hozzáadása információk tooyour App Service-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="07132-121"><a name="secrets"> </a>Add Microsoft Account information tooyour App Service application</span></span>
1. <span data-ttu-id="07132-122">Vissza a hello [Azure-portálon]tooyour alkalmazás lépjen, kattintson **beállítások** > **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="07132-122">Back in hello [Azure portal], navigate tooyour application, click **Settings** > **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="07132-123">Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja **a**.</span><span class="sxs-lookup"><span data-stu-id="07132-123">If hello Authentication / Authorization feature is not enabled, switch it **On**.</span></span>
3. <span data-ttu-id="07132-124">Kattintson a **Microsoft-fiók**.</span><span class="sxs-lookup"><span data-stu-id="07132-124">Click **Microsoft Account**.</span></span> <span data-ttu-id="07132-125">Illessze be a hello alkalmazás Azonosítóját és jelszavát értékek, amelyek korábban beszerzett, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök.</span><span class="sxs-lookup"><span data-stu-id="07132-125">Paste in hello Application ID and Password values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="07132-126">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="07132-126">Then click **OK**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="07132-127">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="07132-127">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="07132-128">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="07132-128">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="07132-129">(Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Microsoft-fiók beállítása **hitelesítetlen kérés esetén a művelet tootake** túl**Microsoft Account**.</span><span class="sxs-lookup"><span data-stu-id="07132-129">(Optional) toorestrict access tooyour site tooonly users authenticated by Microsoft account, set **Action tootake when request is not authenticated** too**Microsoft Account**.</span></span> <span data-ttu-id="07132-130">Ehhez szükséges, hogy az összes kérelem hitelesítését, és a rendszer az összes nem hitelesített kérelmek hitelesítés tooMicrosoft fiókját.</span><span class="sxs-lookup"><span data-stu-id="07132-130">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooMicrosoft account for authentication.</span></span>
5. <span data-ttu-id="07132-131">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="07132-131">Click **Save**.</span></span>

<span data-ttu-id="07132-132">Most már áll készen toouse Microsoft Account hitelesítéshez az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="07132-132">You are now ready toouse Microsoft Account for authentication in your app.</span></span>

## <span data-ttu-id="07132-133"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="07132-133"><a name="related-content"> </a>Related content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[saját alkalmazások]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portálon]: https://portal.azure.com/
