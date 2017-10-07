---
title: "az Android a Mobile Apps aaaAdd hitelesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello az Android-alkalmazás Identitásszolgáltatók, beleértve a Google, a Facebook, a Twitter és a Microsoft számos tooauthenticate felhasználói Azure App Service Mobile Apps szolgáltatásának."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a><span data-ttu-id="e69e4-103">Hitelesítési tooyour Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e69e4-103">Add authentication tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="e69e4-104">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e69e4-104">Summary</span></span>
<span data-ttu-id="e69e4-105">Ebben az oktatóanyagban hozzáadása hitelesítési toohello todolist gyorsútmutató-projekt az Android a támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="e69e4-105">In this tutorial, you add authentication toohello todolist quickstart project on Android by using a supported identity provider.</span></span> <span data-ttu-id="e69e4-106">Ez az oktatóanyag hello alapuló [Ismerkedés a Mobile Apps] oktatóanyag, amely először el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="e69e4-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="e69e4-107"><a name="register"></a>Hitelesítés az alkalmazás regisztrálása és konfigurálása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e69e4-107"><a name="register"></a>Register your app for authentication and configure Azure App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="e69e4-108"><a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e69e4-108"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="e69e4-109">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="e69e4-109">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="e69e4-110">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="e69e4-110">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="e69e4-111">Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="e69e4-111">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="e69e4-112">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e69e4-112">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="e69e4-113">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e69e4-113">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="e69e4-114">tooenable hello átirányítási hello kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="e69e4-114">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="e69e4-115">Hello [Azure-portálon] válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="e69e4-115">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="e69e4-116">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="e69e4-116">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="e69e4-117">A hello **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="e69e4-117">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="e69e4-118">Hello _appname_ hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e69e4-118">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="e69e4-119">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="e69e4-119">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="e69e4-120">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e69e4-120">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="e69e4-121">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e69e4-121">Click **OK**.</span></span>

5. <span data-ttu-id="e69e4-122">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e69e4-122">Click **Save**.</span></span>

## <span data-ttu-id="e69e4-123"><a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="e69e4-123"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* <span data-ttu-id="e69e4-124">Az Android Studióban nyissa meg elvégezte hello projekt hello oktatóanyag [Ismerkedés a Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="e69e4-124">In Android Studio, open hello project you completed with hello tutorial [Get started with Mobile Apps].</span></span> <span data-ttu-id="e69e4-125">A hello **futtatása** menüben kattintson **-alkalmazás futtatása**, és győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e69e4-125">From hello **Run** menu, click **Run app**, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span>

     <span data-ttu-id="e69e4-126">Ez a kivétel az oka, hogy hello app kísérletek tooaccess hello vissza, nem hitelesített felhasználónak végén, de hello *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="e69e4-126">This exception happens because hello app attempts tooaccess hello back end as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="e69e4-127">Következő lépésként frissítse a hello app tooauthenticate felhasználók erőforrások kér hello vissza a Mobile Apps befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="e69e4-127">Next, you update hello app tooauthenticate users before requesting resources from hello Mobile Apps back end.</span></span> 

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="e69e4-128">Hitelesítési toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e69e4-128">Add authentication toohello app</span></span>
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <span data-ttu-id="e69e4-129"><a name="cache-tokens"></a>A hitelesítési tokenek hello ügyfélen gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="e69e4-129"><a name="cache-tokens"></a>Cache authentication tokens on hello client</span></span>
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="e69e4-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e69e4-130">Next steps</span></span>
<span data-ttu-id="e69e4-131">Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:</span><span class="sxs-lookup"><span data-stu-id="e69e4-131">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="e69e4-132">[Leküldéses értesítések tooyour Android-alkalmazás hozzáadása](app-service-mobile-android-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="e69e4-132">[Add push notifications tooyour Android app](app-service-mobile-android-get-started-push.md).</span></span>
  <span data-ttu-id="e69e4-133">Ismerje meg, hogyan a Mobile Apps biztonsági tooconfigure end toouse Azure notification hubs toosend leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="e69e4-133">Learn how tooconfigure your Mobile Apps back end toouse Azure notification hubs toosend push notifications.</span></span>
* <span data-ttu-id="e69e4-134">[Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="e69e4-134">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="e69e4-135">Ismerje meg, hogyan tooadd offline tooyour alkalmazás használatával támogatják a Mobile Apps háttérből.</span><span class="sxs-lookup"><span data-stu-id="e69e4-135">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="e69e4-136">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="e69e4-136">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Ismerkedés a Mobile Apps]: app-service-mobile-android-get-started.md
