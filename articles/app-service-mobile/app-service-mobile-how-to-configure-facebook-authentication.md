---
title: "aaaHow tooconfigure Facebook hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Facebook hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a><span data-ttu-id="d8a36-103">Hogyan tooconfigure App Service alkalmazás toouse Facebook bejelentkezési adatait</span><span class="sxs-lookup"><span data-stu-id="d8a36-103">How tooconfigure your App Service application toouse Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="d8a36-104">Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Facebook hitelesítési-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="d8a36-104">This topic shows you how tooconfigure Azure App Service toouse Facebook as an authentication provider.</span></span>

<span data-ttu-id="d8a36-105">toocomplete hello a jelen témakör eljárásában, fiókkal kell rendelkeznie a Facebook, amelyen egy ellenőrzött és érvényes e-mail címet és egy mobiltelefon számával.</span><span class="sxs-lookup"><span data-stu-id="d8a36-105">toocomplete hello procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="d8a36-106">toocreate új Facebook-fiókkal, Ugrás túl[Facebook.com weboldalt].</span><span class="sxs-lookup"><span data-stu-id="d8a36-106">toocreate a new Facebook account, go too[facebook.com].</span></span>

## <span data-ttu-id="d8a36-107"><a name="register"></a>Regisztrálhatja alkalmazását Facebook-on</span><span class="sxs-lookup"><span data-stu-id="d8a36-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="d8a36-108">Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d8a36-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="d8a36-109">Másolás a **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-109">Copy your **URL**.</span></span> <span data-ttu-id="d8a36-110">A Facebook-alkalmazást a tooconfigure fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d8a36-110">You will use this tooconfigure your Facebook app.</span></span>
2. <span data-ttu-id="d8a36-111">Egy másik böngészőablakban, keresse meg a toohello [Facebook fejlesztők] webhelyet, és jelentkezzen be a Facebook-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d8a36-111">In another browser window, navigate toohello [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="d8a36-112">(Választható) Ha már nem regisztrált, kattintson a **alkalmazások** > **fejlesztőként nyilvántartásba**, majd fogadja el hello szabályzatot, és hello regisztrációs lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="d8a36-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept hello policy and follow hello registration steps.</span></span>
4. <span data-ttu-id="d8a36-113">Kattintson a **alkalmazásaimat** > **fel egy új alkalmazást** > **webhely** > **kihagyhatja, és létrehozhat Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="d8a36-114">A **megjelenített név**, adjon meg egy egyedi nevet az alkalmazás típusa a **kapcsolattartó E-mail**, válasszon egy **kategória** beállítása az alkalmazáshoz, majd kattintson a **Alkalmazásazonosító létrehozása**és hello biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="d8a36-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete hello security check.</span></span> <span data-ttu-id="d8a36-115">Ezzel megnyitná toohello fejlesztői irányítópult az új Facebook-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d8a36-115">This takes you toohello developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="d8a36-116">Kattintson a "Bejelentkezés Facebook-on," **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="d8a36-117">Adja hozzá az alkalmazást **átirányítási URI-** túl**érvényes OAuth-alapú átirányítási URI-azonosítók**, majd kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-117">Add your application's **Redirect URI** too**Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="d8a36-118">Az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="d8a36-118">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="d8a36-119">Például: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="d8a36-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="d8a36-120">Győződjön meg arról, hogy hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="d8a36-120">Make sure that you are using hello HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="d8a36-121">Kattintson a bal oldali navigációs hello, **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-121">In hello left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="d8a36-122">A hello **alkalmazás titkos kulcs** , majd kattintson **megjelenítése**, adja meg a jelszót, ha a kért, majd jegyezze fel a hello értékének **Alkalmazásazonosító** és **alkalmazás titkos kulcs**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-122">On hello **App Secret** field, click **Show**, provide your password if requested, then make a note of hello values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="d8a36-123">Használja a későbbi tooconfigure az alkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d8a36-123">You use these later tooconfigure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="d8a36-124">hello alkalmazás titkos kulcs egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="d8a36-124">hello app secret is an important security credential.</span></span> <span data-ttu-id="d8a36-125">Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="d8a36-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="d8a36-126">hello Facebook-fiókhoz használt tooregister hello alkalmazás lépett hello alkalmazás rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="d8a36-126">hello Facebook account which was used tooregister hello application is an administrator of hello app.</span></span> <span data-ttu-id="d8a36-127">Ezen a ponton csak a rendszergazdák jelentkezhetnek be az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d8a36-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="d8a36-128">tooauthenticate más Facebook-fiókba, kattintson a **App felülvizsgálati** , és engedélyezze **< saját-alkalmazás-neve > tegyék közzé** tooenable általános hozzáférésű Facebook-hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="d8a36-128">tooauthenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** tooenable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="d8a36-129"><a name="secrets"></a>Hozzáadása Facebook-információk tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d8a36-129"><a name="secrets"> </a>Add Facebook information tooyour application</span></span>
1. <span data-ttu-id="d8a36-130">Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d8a36-130">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="d8a36-131">Kattintson a **beállítások** > **hitelesítési / engedélyezési**, és győződjön meg arról, hogy **App Service hitelesítés** van **a**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="d8a36-132">Kattintson a **Facebook**, illessze be a hello Alkalmazásazonosító és alkalmazás titkos kulcs értékét, amely korábban beszerzett, nem kötelezően bármely hatókörök, az alkalmazás által igényelt engedélyezése, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-132">Click **Facebook**, paste in hello App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="d8a36-133">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="d8a36-133">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="d8a36-134">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="d8a36-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="d8a36-135">(Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Facebook, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Facebook**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-135">(Optional) toorestrict access tooyour site tooonly users authenticated by Facebook, set **Action tootake when request is not authenticated** too**Facebook**.</span></span> <span data-ttu-id="d8a36-136">Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooFacebook hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d8a36-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooFacebook for authentication.</span></span>
4. <span data-ttu-id="d8a36-137">Konfigurálás hitelesítési végzett, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d8a36-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="d8a36-138">Most már áll készen toouse Facebook-hitelesítéshez az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d8a36-138">You are now ready toouse Facebook for authentication in your app.</span></span>

## <span data-ttu-id="d8a36-139"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="d8a36-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook fejlesztők]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com weboldalt]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portálon]: https://portal.azure.com/
