---
title: "aaaHow tooconfigure Twitter hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Twitter hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a><span data-ttu-id="00d16-103">Hogyan tooconfigure App Service alkalmazás toouse Twitter bejelentkezési adatait</span><span class="sxs-lookup"><span data-stu-id="00d16-103">How tooconfigure your App Service application toouse Twitter login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="00d16-104">Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Twitter hitelesítési-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="00d16-104">This topic shows you how tooconfigure Azure App Service toouse Twitter as an authentication provider.</span></span>

<span data-ttu-id="00d16-105">toocomplete hello a jelen témakör eljárásában, rendelkeznie kell egy Twitter-fiók, amely több hitelesített e-mail címét és telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="00d16-105">toocomplete hello procedure in this topic, you must have a Twitter account that has a verified email address and phone number.</span></span> <span data-ttu-id="00d16-106">túl nyissa meg egy új Twitter-fiók toocreate<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span><span class="sxs-lookup"><span data-stu-id="00d16-106">toocreate a new Twitter account, go too<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span></span>

## <span data-ttu-id="00d16-107"><a name="register"></a>Az alkalmazás regisztrálása a Twitteren</span><span class="sxs-lookup"><span data-stu-id="00d16-107"><a name="register"> </a>Register your application with Twitter</span></span>
1. <span data-ttu-id="00d16-108">Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00d16-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="00d16-109">Másolás a **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="00d16-109">Copy your **URL**.</span></span> <span data-ttu-id="00d16-110">A tooconfigure az Twitter-alkalmazás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="00d16-110">You will use this tooconfigure your Twitter app.</span></span>
2. <span data-ttu-id="00d16-111">Keresse meg a toohello [Twitter fejlesztők] webhelyét, jelentkezzen be Twitter-fiók hitelesítő adataival, majd kattintson a **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="00d16-111">Navigate toohello [Twitter Developers] website, sign in with your Twitter account credentials, and click **Create New App**.</span></span>
3. <span data-ttu-id="00d16-112">A típushoz hello **neve** és egy **leírása** az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00d16-112">Type in hello **Name** and a **Description** for your new app.</span></span> <span data-ttu-id="00d16-113">Illessze be az alkalmazás **URL-cím** a hello **webhely** érték.</span><span class="sxs-lookup"><span data-stu-id="00d16-113">Paste in your application's **URL** for hello **Website** value.</span></span> <span data-ttu-id="00d16-114">Ezt követően a hello **visszahívási URL-cím**, illessze be a hello **visszahívási URL-cím** korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="00d16-114">Then, for hello **Callback URL**, paste hello **Callback URL** you copied earlier.</span></span> <span data-ttu-id="00d16-115">Ez az a mobilalkalmazás átjáró hello elérési hozzáíródik */.auth/login/twitter/callback*.</span><span class="sxs-lookup"><span data-stu-id="00d16-115">This is your Mobile App gateway appended with hello path, */.auth/login/twitter/callback*.</span></span> <span data-ttu-id="00d16-116">Például: `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="00d16-116">For example, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span></span> <span data-ttu-id="00d16-117">Győződjön meg arról, hogy hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="00d16-117">Make sure that you are using hello HTTPS scheme.</span></span>
4. <span data-ttu-id="00d16-118">Hello alsó hello lapon olvassa el, és fogadja el hello feltételeit.</span><span class="sxs-lookup"><span data-stu-id="00d16-118">At hello bottom hello page, read and accept hello terms.</span></span> <span data-ttu-id="00d16-119">Kattintson a **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="00d16-119">Then click **Create your Twitter application**.</span></span> <span data-ttu-id="00d16-120">A regisztereket hello app hello alkalmazás részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="00d16-120">This registers hello app displays hello application details.</span></span>
5. <span data-ttu-id="00d16-121">Hello kattintson **beállítások** lapon jelölje **engedélyezése az alkalmazás használt toobe toosign twitterrel**, majd kattintson a **frissítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="00d16-121">Click hello **Settings** tab, check **Allow this application toobe used toosign in with Twitter**, then click **Update Settings**.</span></span>
6. <span data-ttu-id="00d16-122">Jelölje be hello **kulcsok és a hozzáférési jogkivonatok** fülre. Jegyezze fel a hello értékének **(API-kulcs) kulcsa** és **felhasználói titok (API titkos)**.</span><span class="sxs-lookup"><span data-stu-id="00d16-122">Select hello **Keys and Access Tokens** tab. Make a note of hello values of **Consumer Key (API Key)** and **Consumer secret (API Secret)**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="00d16-123">hello fogyasztói titkos kulcs egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="00d16-123">hello consumer secret is an important security credential.</span></span> <span data-ttu-id="00d16-124">Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="00d16-124">Do not share this secret with anyone or distribute it with your app.</span></span>
   > 
   > 

## <span data-ttu-id="00d16-125"><a name="secrets"></a>Információk tooyour alkalmazás Twitter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00d16-125"><a name="secrets"> </a>Add Twitter information tooyour application</span></span>
1. <span data-ttu-id="00d16-126">Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00d16-126">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="00d16-127">Kattintson a **beállítások**, majd **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="00d16-127">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="00d16-128">Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a hello kapcsoló túl**a**.</span><span class="sxs-lookup"><span data-stu-id="00d16-128">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="00d16-129">Kattintson a **Twitter**.</span><span class="sxs-lookup"><span data-stu-id="00d16-129">Click **Twitter**.</span></span> <span data-ttu-id="00d16-130">Illessze be a hello alkalmazás Azonosítóját és az alkalmazás titkos kulcs értékek, amelyek korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="00d16-130">Paste in hello App ID and App Secret values which you obtained previously.</span></span> <span data-ttu-id="00d16-131">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="00d16-131">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="00d16-132">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="00d16-132">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="00d16-133">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="00d16-133">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="00d16-134">(Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Twitter, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Twitter**.</span><span class="sxs-lookup"><span data-stu-id="00d16-134">(Optional) toorestrict access tooyour site tooonly users authenticated by Twitter, set **Action tootake when request is not authenticated** too**Twitter**.</span></span> <span data-ttu-id="00d16-135">Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooTwitter hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="00d16-135">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooTwitter for authentication.</span></span>
5. <span data-ttu-id="00d16-136">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="00d16-136">Click **Save**.</span></span>

<span data-ttu-id="00d16-137">Most már áll készen toouse Twitter-hitelesítéshez az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="00d16-137">You are now ready toouse Twitter for authentication in your app.</span></span>

## <span data-ttu-id="00d16-138"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="00d16-138"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter fejlesztők]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portálon]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
