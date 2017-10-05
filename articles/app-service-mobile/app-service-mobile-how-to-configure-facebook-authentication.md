---
title: "Az App Service szolgáltatások alkalmazás Facebook-hitelesítés konfigurálása"
description: "Tudnivalók az App Service szolgáltatások alkalmazásbeli Facebook hitelesítés konfigurálása."
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
ms.openlocfilehash: c1b4c91d384c56c4f55bf8d31ced250f51c0d837
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a><span data-ttu-id="9e7a3-103">App Service-alkalmazás konfigurálása Facebook-bejelentkezés használatához</span><span class="sxs-lookup"><span data-stu-id="9e7a3-103">How to configure your App Service application to use Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="9e7a3-104">Ez a témakör bemutatja, hogyan konfigurálhatja az Azure App Service egy hitelesítésszolgáltatót Facebook használandó.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-104">This topic shows you how to configure Azure App Service to use Facebook as an authentication provider.</span></span>

<span data-ttu-id="9e7a3-105">Ebben a témakörben az eljárás végrehajtásához egy hitelesített e-mail címet és a mobiltelefonszám Facebook fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-105">To complete the procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="9e7a3-106">Hozzon létre egy új Facebook-fiókkal, Ugrás [Facebook.com weboldalt].</span><span class="sxs-lookup"><span data-stu-id="9e7a3-106">To create a new Facebook account, go to [facebook.com].</span></span>

## <span data-ttu-id="9e7a3-107"><a name="register"></a>Regisztrálhatja alkalmazását Facebook-on</span><span class="sxs-lookup"><span data-stu-id="9e7a3-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="9e7a3-108">Jelentkezzen be a [Azure-portálon], és keresse meg az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="9e7a3-109">Másolás a **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-109">Copy your **URL**.</span></span> <span data-ttu-id="9e7a3-110">A Facebook-alkalmazást konfigurálásához használandó.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-110">You will use this to configure your Facebook app.</span></span>
2. <span data-ttu-id="9e7a3-111">Egy másik böngészőablakban, navigáljon a [Facebook fejlesztők] webhelyet, és jelentkezzen be a Facebook-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-111">In another browser window, navigate to the [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="9e7a3-112">(Választható) Ha már nem regisztrált, kattintson a **alkalmazások** > **fejlesztőként nyilvántartásba**, majd fogadja el a szabályzatot, és kövesse a regisztráció lépéseit.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept the policy and follow the registration steps.</span></span>
4. <span data-ttu-id="9e7a3-113">Kattintson a **alkalmazásaimat** > **fel egy új alkalmazást** > **webhely** > **kihagyhatja, és létrehozhat Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="9e7a3-114">A **megjelenített név**, adjon meg egy egyedi nevet az alkalmazás típusa a **kapcsolattartó E-mail**, válasszon egy **kategória** beállítása az alkalmazáshoz, majd kattintson a **Alkalmazásazonosító létrehozása** és a biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete the security check.</span></span> <span data-ttu-id="9e7a3-115">Ezzel megnyitná a fejlesztői irányítópult az új Facebook-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-115">This takes you to the developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="9e7a3-116">Kattintson a "Bejelentkezés Facebook-on," **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="9e7a3-117">Adja hozzá az alkalmazást **átirányítási URI-** való **érvényes OAuth-alapú átirányítási URI-azonosítók**, majd kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-117">Add your application's **Redirect URI** to **Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="9e7a3-118">Az átirányítási URI megadása az alkalmazás fűzhető hozzá az elérési út az URL-cím */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-118">Your redirect URI is the URL of your application appended with the path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="9e7a3-119">Például: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="9e7a3-120">Győződjön meg arról, hogy a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-120">Make sure that you are using the HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="9e7a3-121">Kattintson a bal oldali navigációs **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-121">In the left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="9e7a3-122">Az a **alkalmazás titkos kulcs** , majd kattintson **megjelenítése**, adja meg a jelszót, ha a kért, majd jegyezze fel a közül **Alkalmazásazonosító** és **alkalmazás titkos kulcs**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-122">On the **App Secret** field, click **Show**, provide your password if requested, then make a note of the values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="9e7a3-123">Ezek később konfigurálására használt az alkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-123">You use these later to configure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="9e7a3-124">Az alkalmazás titkos kulcs egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-124">The app secret is an important security credential.</span></span> <span data-ttu-id="9e7a3-125">Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="9e7a3-126">A Facebook-fiók, amely az alkalmazás regisztrálása lett megadva a rendszergazda az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-126">The Facebook account which was used to register the application is an administrator of the app.</span></span> <span data-ttu-id="9e7a3-127">Ezen a ponton csak a rendszergazdák jelentkezhetnek be az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="9e7a3-128">Más Facebook fiókok hitelesítéséhez kattintson **App felülvizsgálati** , és engedélyezze **< saját-alkalmazás-neve > tegyék közzé** a Facebook-hitelesítéssel általános nyilvános hozzáférés engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-128">To authenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** to enable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="9e7a3-129"><a name="secrets"></a>Hozzáadása Facebook információkat az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="9e7a3-129"><a name="secrets"> </a>Add Facebook information to your application</span></span>
1. <span data-ttu-id="9e7a3-130">Vissza a [Azure-portálon], keresse meg az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-130">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="9e7a3-131">Kattintson a **beállítások** > **hitelesítési / engedélyezési**, és győződjön meg arról, hogy **App Service hitelesítés** van **a**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="9e7a3-132">Kattintson a **Facebook**, illessze be az alkalmazás Azonosítóját és az alkalmazás titkos kulcs értékek, amelyek korábban beszerzett, nem kötelezően bármely hatókörök, az alkalmazás által igényelt engedélyezése, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-132">Click **Facebook**, paste in the App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="9e7a3-133">Alapértelmezés szerint az App Service hitelesítést nyújt, de nem engedélyezett hozzáférés korlátozása a tartalom és API-k.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-133">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="9e7a3-134">Kell engedélyeznie a felhasználók az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="9e7a3-135">(Választható) Csak azok a felhasználók hitelesítése Facebook-on a helyhez való hozzáférésének korlátozásához, állítsa be **hitelesítetlen kérés esetén elvégzendő művelet** való **Facebook**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-135">(Optional) To restrict access to your site to only users authenticated by Facebook, set **Action to take when request is not authenticated** to **Facebook**.</span></span> <span data-ttu-id="9e7a3-136">Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek Facebook-hitelesítéshez van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Facebook for authentication.</span></span>
4. <span data-ttu-id="9e7a3-137">Konfigurálás hitelesítési végzett, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="9e7a3-138">Most már készen áll a Facebook az alkalmazáson belüli hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9e7a3-138">You are now ready to use Facebook for authentication in your app.</span></span>

## <span data-ttu-id="9e7a3-139"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="9e7a3-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
<span data-ttu-id="9e7a3-140">[Facebook fejlesztők]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span><span class="sxs-lookup"><span data-stu-id="9e7a3-140">[Facebook Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span></span>
<span data-ttu-id="9e7a3-141">[Facebook.com weboldalt]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span><span class="sxs-lookup"><span data-stu-id="9e7a3-141">[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span></span>
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
<span data-ttu-id="9e7a3-142">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="9e7a3-142">[Azure portal]: https://portal.azure.com/</span></span>
