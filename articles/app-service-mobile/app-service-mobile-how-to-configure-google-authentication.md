---
title: "Google hitelesítési alkalmazásszolgáltatások alkalmazás konfigurálása"
description: "Tudnivalók az App Service szolgáltatások alkalmazás Google-hitelesítés konfigurálása."
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
ms.openlocfilehash: d6c1707f67d986487e5a45e76ffc9a02ddf16eb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a><span data-ttu-id="41dd5-103">Az App Service alkalmazás használhatja a Google-bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41dd5-103">How to configure your App Service application to use Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="41dd5-104">Ez a témakör bemutatja, hogyan konfigurálhatja az Azure App Service egy hitelesítésszolgáltatót Google használandó.</span><span class="sxs-lookup"><span data-stu-id="41dd5-104">This topic shows you how to configure Azure App Service to use Google as an authentication provider.</span></span>

<span data-ttu-id="41dd5-105">Ebben a témakörben az eljárás végrehajtásához egy hitelesített e-mail-címmel rendelkező Google fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="41dd5-105">To complete the procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="41dd5-106">Új Google-fiók létrehozásához látogassa meg az [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="41dd5-106">To create a new Google account, go to [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="41dd5-107"><a name="register"></a>Az alkalmazás regisztrálása a Google</span><span class="sxs-lookup"><span data-stu-id="41dd5-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="41dd5-108">Jelentkezzen be a [Azure-portálon], és keresse meg az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="41dd5-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="41dd5-109">Másolás a **URL-cím**, amely később a Google alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="41dd5-109">Copy your **URL**, which you use later to configure your Google app.</span></span>
2. <span data-ttu-id="41dd5-110">Keresse meg a [Google API-k](http://go.microsoft.com/fwlink/p/?LinkId=268303) webhelyét, jelentkezzen be Google fiók hitelesítő adatait, kattintson **projekt létrehozása**, adjon meg egy **projekt neve**, majd kattintson a  **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-110">Navigate to the [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="41dd5-111">A **közösségi API-k** kattintson **Google + API** , majd **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="41dd5-112">A bal oldali navigációs **hitelesítő adatok** > **OAuth-hozzájárulás képernyő**, majd jelölje be a **E-mail cím**, adjon meg egy **termékneve**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-112">In the left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="41dd5-113">Az a **hitelesítő adatok** lapra, majd **hitelesítő adatok létrehozása** > **OAuth-Ügyfélazonosító**, majd jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-113">In the **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="41dd5-114">Illessze be az App Service **URL-cím** másolt korábban be **jogosult JavaScript-források**, majd illessze be az átirányítási az URI **jogosult átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-114">Paste the App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="41dd5-115">Az átirányítási URI megadása az alkalmazás fűzhető hozzá az elérési út az URL-cím */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="41dd5-115">The redirect URI is the URL of your application appended with the path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="41dd5-116">Például: `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="41dd5-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="41dd5-117">Győződjön meg arról, hogy a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="41dd5-117">Make sure that you are using the HTTPS scheme.</span></span> <span data-ttu-id="41dd5-118">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="41dd5-118">Then click **Create**.</span></span>
7. <span data-ttu-id="41dd5-119">A következő képernyőn jegyezze fel az ügyfél-azonosító és a titkos ügyfélkódot értékeit.</span><span class="sxs-lookup"><span data-stu-id="41dd5-119">On the next screen, make a note of the values of the client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="41dd5-120">A titkos ügyfélkulcs egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="41dd5-120">The client secret is an important security credential.</span></span> <span data-ttu-id="41dd5-121">Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="41dd5-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="41dd5-122"><a name="secrets"></a>Hozzáadása Google információkat az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="41dd5-122"><a name="secrets"> </a>Add Google information to your application</span></span>
1. <span data-ttu-id="41dd5-123">Vissza a [Azure-portálon], keresse meg az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="41dd5-123">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="41dd5-124">Kattintson a **beállítások**, majd **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="41dd5-125">Ha a hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a kapcsoló **a**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-125">If the Authentication / Authorization feature is not enabled, turn the switch to **On**.</span></span>
3. <span data-ttu-id="41dd5-126">Kattintson a **Google**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-126">Click **Google**.</span></span> <span data-ttu-id="41dd5-127">Illessze be az alkalmazás Azonosítóját és az alkalmazás titkos kulcs értékek, amelyek korábban beszerzett, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök.</span><span class="sxs-lookup"><span data-stu-id="41dd5-127">Paste in the App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="41dd5-128">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="41dd5-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="41dd5-129">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem engedélyezett hozzáférés korlátozása a tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="41dd5-129">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="41dd5-130">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="41dd5-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="41dd5-131">(Választható) Csak a Google által hitelesített felhasználók a helyhez való hozzáférésének korlátozásához, állítsa be **hitelesítetlen kérés esetén elvégzendő művelet** való **Google**.</span><span class="sxs-lookup"><span data-stu-id="41dd5-131">(Optional) To restrict access to your site to only users authenticated by Google, set **Action to take when request is not authenticated** to **Google**.</span></span> <span data-ttu-id="41dd5-132">Ehhez szükséges összes kérelmet hitelesíteni, és minden nem hitelesített kérelmek Google hitelesítésre van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="41dd5-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Google for authentication.</span></span>
5. <span data-ttu-id="41dd5-133">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="41dd5-133">Click **Save**.</span></span>

<span data-ttu-id="41dd5-134">Most már készen áll a Google az alkalmazáson belüli hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="41dd5-134">You are now ready to use Google for authentication in your app.</span></span>

## <span data-ttu-id="41dd5-135"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="41dd5-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portálon]: https://portal.azure.com/

