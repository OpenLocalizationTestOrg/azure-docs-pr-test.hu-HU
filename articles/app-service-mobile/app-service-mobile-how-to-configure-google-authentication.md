---
title: "aaaHow tooconfigure Google hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Google hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a><span data-ttu-id="42227-103">Hogyan tooconfigure App Service alkalmazás toouse Google bejelentkezési adatait</span><span class="sxs-lookup"><span data-stu-id="42227-103">How tooconfigure your App Service application toouse Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="42227-104">Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Google hitelesítési-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="42227-104">This topic shows you how tooconfigure Azure App Service toouse Google as an authentication provider.</span></span>

<span data-ttu-id="42227-105">toocomplete hello a jelen témakör eljárásában, rendelkeznie kell egy hitelesített e-mail-címmel rendelkező Google-fiók.</span><span class="sxs-lookup"><span data-stu-id="42227-105">toocomplete hello procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="42227-106">új Google-fiók toocreate lépjen túl[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="42227-106">toocreate a new Google account, go too[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="42227-107"><a name="register"></a>Az alkalmazás regisztrálása a Google</span><span class="sxs-lookup"><span data-stu-id="42227-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="42227-108">Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="42227-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="42227-109">Másolás a **URL-cím**, amely újabb tooconfigure a Google alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="42227-109">Copy your **URL**, which you use later tooconfigure your Google app.</span></span>
2. <span data-ttu-id="42227-110">Keresse meg a toohello [Google API-k](http://go.microsoft.com/fwlink/p/?LinkId=268303) webhelyét, jelentkezzen be Google fiók hitelesítő adatait, kattintson a **projekt létrehozása**, adjon meg egy **projekt neve**, kattintson a  **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="42227-110">Navigate toohello [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="42227-111">A **közösségi API-k** kattintson **Google + API** , majd **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="42227-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="42227-112">A bal oldali navigációs, hello **hitelesítő adatok** > **OAuth-hozzájárulás képernyő**, majd válassza a **E-mail cím**, adjon meg egy **termékneve**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="42227-112">In hello left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="42227-113">A hello **hitelesítő adatok** lapra, majd **hitelesítő adatok létrehozása** > **OAuth-Ügyfélazonosító**, majd jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42227-113">In hello **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="42227-114">Beillesztés hello App Service **URL-cím** másolt korábban be **jogosult JavaScript-források**, majd illessze be az átirányítási az URI **jogosult átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="42227-114">Paste hello App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="42227-115">az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési hello */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="42227-115">hello redirect URI is hello URL of your application appended with hello path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="42227-116">Például: `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="42227-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="42227-117">Győződjön meg arról, hogy hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="42227-117">Make sure that you are using hello HTTPS scheme.</span></span> <span data-ttu-id="42227-118">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="42227-118">Then click **Create**.</span></span>
7. <span data-ttu-id="42227-119">A következő képernyőn hello jegyezze fel a hello értékek hello ügyfél azonosító és az ügyfél titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="42227-119">On hello next screen, make a note of hello values of hello client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="42227-120">hello ügyfélkulcs egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="42227-120">hello client secret is an important security credential.</span></span> <span data-ttu-id="42227-121">Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="42227-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="42227-122"><a name="secrets"></a>Hozzáadása Google információk tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="42227-122"><a name="secrets"> </a>Add Google information tooyour application</span></span>
1. <span data-ttu-id="42227-123">Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="42227-123">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="42227-124">Kattintson a **beállítások**, majd **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="42227-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="42227-125">Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a hello kapcsoló túl**a**.</span><span class="sxs-lookup"><span data-stu-id="42227-125">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="42227-126">Kattintson a **Google**.</span><span class="sxs-lookup"><span data-stu-id="42227-126">Click **Google**.</span></span> <span data-ttu-id="42227-127">Illessze be a hello Alkalmazásazonosító és alkalmazás titkos kulcs értékét, amely korábban megszerezte, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök.</span><span class="sxs-lookup"><span data-stu-id="42227-127">Paste in hello App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="42227-128">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42227-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="42227-129">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="42227-129">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="42227-130">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="42227-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="42227-131">(Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók a Google, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Google**.</span><span class="sxs-lookup"><span data-stu-id="42227-131">(Optional) toorestrict access tooyour site tooonly users authenticated by Google, set **Action tootake when request is not authenticated** too**Google**.</span></span> <span data-ttu-id="42227-132">Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooGoogle hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="42227-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooGoogle for authentication.</span></span>
5. <span data-ttu-id="42227-133">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42227-133">Click **Save**.</span></span>

<span data-ttu-id="42227-134">Most már áll készen toouse Google-hitelesítéshez az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="42227-134">You are now ready toouse Google for authentication in your app.</span></span>

## <span data-ttu-id="42227-135"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="42227-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portálon]: https://portal.azure.com/

