---
title: "Hitelesítés hozzáadása az Android a Mobile Apps |} Microsoft Docs"
description: "Útmutató az Azure App Service Mobile Apps szolgáltatásának segítségével hitelesíti a felhasználókat identitás-szolgáltatóktól, beleértve a Google, a Facebook, a Twitter és a Microsoft számos az Android-alkalmazás."
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
ms.openlocfilehash: 81331142aa6110d4e29e6fb30a90ce6e3a853439
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-android-app"></a><span data-ttu-id="fdbac-103">Hitelesítés hozzáadása az Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fdbac-103">Add authentication to your Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="fdbac-104">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="fdbac-104">Summary</span></span>
<span data-ttu-id="fdbac-105">Ebben az oktatóanyagban hozzáadhat hitelesítési a todolist gyorsútmutató-projekt az Android támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="fdbac-105">In this tutorial, you add authentication to the todolist quickstart project on Android by using a supported identity provider.</span></span> <span data-ttu-id="fdbac-106">Ez az oktatóanyag alapul a [Ismerkedés a Mobile Apps] oktatóanyag, amely először el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="fdbac-106">This tutorial is based on the [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="fdbac-107"><a name="register"></a>Hitelesítés az alkalmazás regisztrálása és konfigurálása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fdbac-107"><a name="register"></a>Register your app for authentication and configure Azure App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="fdbac-108"><a name="redirecturl"></a>Az alkalmazás hozzáadása az engedélyezett külső átirányítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="fdbac-108"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="fdbac-109">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="fdbac-109">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="fdbac-110">Ez lehetővé teszi a hitelesítési rendszer visszairányítja az alkalmazás a hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="fdbac-110">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="fdbac-111">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="fdbac-111">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="fdbac-112">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fdbac-112">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="fdbac-113">A mobilalkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fdbac-113">It should be unique to your mobile application.</span></span> <span data-ttu-id="fdbac-114">A kiszolgáló oldalán engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="fdbac-114">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="fdbac-115">Válassza ki az App Service az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="fdbac-115">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="fdbac-116">Kattintson a **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="fdbac-116">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="fdbac-117">Az a **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="fdbac-117">In the **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="fdbac-118">A _appname_ ezt a karakterláncot a rendszer az URL-séma, a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdbac-118">The _appname_ in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="fdbac-119">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="fdbac-119">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="fdbac-120">Meg kell győződnie, jegyezze fel a karakterláncot, amely úgy dönt, mivel úgy, hogy az URL-séma több helyen a mobilalkalmazás kódot kell.</span><span class="sxs-lookup"><span data-stu-id="fdbac-120">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="fdbac-121">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fdbac-121">Click **OK**.</span></span>

5. <span data-ttu-id="fdbac-122">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdbac-122">Click **Save**.</span></span>

## <span data-ttu-id="fdbac-123"><a name="permissions"></a>A hitelesített felhasználókhoz jogosultságok korlátozása</span><span class="sxs-lookup"><span data-stu-id="fdbac-123"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* <span data-ttu-id="fdbac-124">Az Android Studióban nyissa meg a projektet, elvégezte az oktatóanyag [Ismerkedés a Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="fdbac-124">In Android Studio, open the project you completed with the tutorial [Get started with Mobile Apps].</span></span> <span data-ttu-id="fdbac-125">Az a **futtassa** menüben kattintson a **-alkalmazás futtatása**, és győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="fdbac-125">From the **Run** menu, click **Run app**, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span>

     <span data-ttu-id="fdbac-126">Ezt a kivételt az oka, hogy az alkalmazás megpróbál hozzáférni a háttérben, nem hitelesített felhasználóként, de a *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="fdbac-126">This exception happens because the app attempts to access the back end as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="fdbac-127">Ezt követően a felhasználók hitelesítésére, mielőtt kérelmezi erőforrások a Mobile Apps háttérből alkalmazást frissíti.</span><span class="sxs-lookup"><span data-stu-id="fdbac-127">Next, you update the app to authenticate users before requesting resources from the Mobile Apps back end.</span></span> 

## <a name="add-authentication-to-the-app"></a><span data-ttu-id="fdbac-128">Hitelesítés hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="fdbac-128">Add authentication to the app</span></span>
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <span data-ttu-id="fdbac-129"><a name="cache-tokens"></a>Gyorsítótár a hitelesítési tokenek az ügyfélen</span><span class="sxs-lookup"><span data-stu-id="fdbac-129"><a name="cache-tokens"></a>Cache authentication tokens on the client</span></span>
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="fdbac-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdbac-130">Next steps</span></span>
<span data-ttu-id="fdbac-131">Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, fontolja meg valamelyik az alábbi oktatóanyagok folytatása:</span><span class="sxs-lookup"><span data-stu-id="fdbac-131">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* <span data-ttu-id="fdbac-132">[Leküldéses értesítések hozzáadása az Android-alkalmazás](app-service-mobile-android-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="fdbac-132">[Add push notifications to your Android app](app-service-mobile-android-get-started-push.md).</span></span>
  <span data-ttu-id="fdbac-133">Megtudhatja, hogyan konfigurálhatja a Mobile Apps háttér Azure notification hubs használata leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdbac-133">Learn how to configure your Mobile Apps back end to use Azure notification hubs to send push notifications.</span></span>
* <span data-ttu-id="fdbac-134">[Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="fdbac-134">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="fdbac-135">Megtudhatja, hogyan adhat offline támogatást az alkalmazást a Mobile Apps háttérből segítségével.</span><span class="sxs-lookup"><span data-stu-id="fdbac-135">Learn how to add offline support to your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="fdbac-136">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="fdbac-136">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
<span data-ttu-id="fdbac-137">[Ismerkedés a Mobile Apps]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="fdbac-137">[Get started with Mobile Apps]: app-service-mobile-android-get-started.md</span></span>
