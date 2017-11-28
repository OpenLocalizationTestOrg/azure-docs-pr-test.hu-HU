---
title: "a Mobile Apps Apache Cordova-aaaAdd hitelesítés |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Mobile Apps az Azure App Service tooauthenticate felhasználókat identitás-szolgáltatóktól, beleértve a Google, a Facebook, a Twitter és a Microsoft számos az Apache Cordova-alkalmazás."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a><span data-ttu-id="910d7-103">Hitelesítési tooyour Apache Cordova-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="910d7-103">Add authentication tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="910d7-104">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="910d7-104">Summary</span></span>
<span data-ttu-id="910d7-105">Ebben az oktatóanyagban a hitelesítési toohello todolist gyorsútmutató-projekt az Apache Cordova segítségével egy támogatott identitásszolgáltató hozzá.</span><span class="sxs-lookup"><span data-stu-id="910d7-105">In this tutorial, you add authentication toohello todolist quickstart project on Apache Cordova using a supported identity provider.</span></span> <span data-ttu-id="910d7-106">Ez az oktatóanyag hello alapuló [Ismerkedés a Mobile Apps] oktatóanyag, amely először el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="910d7-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="910d7-107"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="910d7-107"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[<span data-ttu-id="910d7-108">Tekintse meg hasonló lépéseket ismertető videót</span><span class="sxs-lookup"><span data-stu-id="910d7-108">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <span data-ttu-id="910d7-109"><a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="910d7-109"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="910d7-110">Most ellenőrizheti, hogy a névtelen hozzáférés tooyour háttér le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="910d7-110">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="910d7-111">A Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="910d7-111">In Visual Studio:</span></span>

* <span data-ttu-id="910d7-112">Nyissa meg hello projekt hello oktatóanyag befejezésekor létrehozott [Ismerkedés a Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="910d7-112">Open hello project that you created when you completed hello tutorial [Get started with Mobile Apps].</span></span>
* <span data-ttu-id="910d7-113">Futtassa az alkalmazást a hello **Google Android Emulator**.</span><span class="sxs-lookup"><span data-stu-id="910d7-113">Run your application in hello **Google Android Emulator**.</span></span>
* <span data-ttu-id="910d7-114">Ellenőrizze, hogy egy váratlan csatlakozási hiba hello alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="910d7-114">Verify that an Unexpected Connection Failure is shown after hello app starts.</span></span>

<span data-ttu-id="910d7-115">Következő lépésként frissítse hello app tooauthenticate felhasználók erőforrások kér hello Mobile Apps-háttéralkalmazás előtt.</span><span class="sxs-lookup"><span data-stu-id="910d7-115">Next, update hello app tooauthenticate users before requesting resources from hello Mobile App backend.</span></span>

## <span data-ttu-id="910d7-116"><a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="910d7-116"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="910d7-117">Nyissa meg a projektet a **Visual Studio**, majd nyissa meg hello `www/index.html` fájlt szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="910d7-117">Open your project in **Visual Studio**, then open hello `www/index.html` file for editing.</span></span>
2. <span data-ttu-id="910d7-118">Keresse meg a hello `Content-Security-Policy` metaadatként hello központi szakaszban.</span><span class="sxs-lookup"><span data-stu-id="910d7-118">Locate hello `Content-Security-Policy` meta tag in hello head section.</span></span>  <span data-ttu-id="910d7-119">Adja hozzá a hello OAuth állomás toohello engedélyezett adatforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="910d7-119">Add hello OAuth host toohello list of allowed sources.</span></span>

   | <span data-ttu-id="910d7-120">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="910d7-120">Provider</span></span> | <span data-ttu-id="910d7-121">SDK-szolgáltató neve</span><span class="sxs-lookup"><span data-stu-id="910d7-121">SDK Provider Name</span></span> | <span data-ttu-id="910d7-122">OAuth-állomás</span><span class="sxs-lookup"><span data-stu-id="910d7-122">OAuth Host</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="910d7-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="910d7-123">Azure Active Directory</span></span> | <span data-ttu-id="910d7-124">aad-ben</span><span class="sxs-lookup"><span data-stu-id="910d7-124">aad</span></span> | <span data-ttu-id="910d7-125">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="910d7-125">https://login.microsoftonline.com</span></span> |
   | <span data-ttu-id="910d7-126">Facebook</span><span class="sxs-lookup"><span data-stu-id="910d7-126">Facebook</span></span> | <span data-ttu-id="910d7-127">Facebook-on</span><span class="sxs-lookup"><span data-stu-id="910d7-127">facebook</span></span> | <span data-ttu-id="910d7-128">https://www.Facebook.com</span><span class="sxs-lookup"><span data-stu-id="910d7-128">https://www.facebook.com</span></span> |
   | <span data-ttu-id="910d7-129">Google</span><span class="sxs-lookup"><span data-stu-id="910d7-129">Google</span></span> | <span data-ttu-id="910d7-130">Google</span><span class="sxs-lookup"><span data-stu-id="910d7-130">google</span></span> | <span data-ttu-id="910d7-131">https://Accounts.google.com</span><span class="sxs-lookup"><span data-stu-id="910d7-131">https://accounts.google.com</span></span> |
   | <span data-ttu-id="910d7-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="910d7-132">Microsoft</span></span> | <span data-ttu-id="910d7-133">MicrosoftAccount</span><span class="sxs-lookup"><span data-stu-id="910d7-133">microsoftaccount</span></span> | <span data-ttu-id="910d7-134">https://Login.live.com</span><span class="sxs-lookup"><span data-stu-id="910d7-134">https://login.live.com</span></span> |
   | <span data-ttu-id="910d7-135">Twitter</span><span class="sxs-lookup"><span data-stu-id="910d7-135">Twitter</span></span> | <span data-ttu-id="910d7-136">Twitter</span><span class="sxs-lookup"><span data-stu-id="910d7-136">twitter</span></span> | <span data-ttu-id="910d7-137">https://API.Twitter.com</span><span class="sxs-lookup"><span data-stu-id="910d7-137">https://api.twitter.com</span></span> |

    <span data-ttu-id="910d7-138">Tartalom-biztonsági-Policy (az Azure Active Directory megvalósítva) például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="910d7-138">An example Content-Security-Policy (implemented for Azure Active Directory) is as follows:</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    <span data-ttu-id="910d7-139">Cserélje le `https://login.microsoftonline.com` a tábla megelőző hello hello OAuth állomással.</span><span class="sxs-lookup"><span data-stu-id="910d7-139">Replace `https://login.microsoftonline.com` with hello OAuth host from hello preceding table.</span></span>  <span data-ttu-id="910d7-140">Hello tartalom-security-házirend metaadatként kapcsolatos további információkért lásd: hello [tartalom-Security-házirend dokumentációját].</span><span class="sxs-lookup"><span data-stu-id="910d7-140">For more information about hello content-security-policy meta tag, see hello [Content-Security-Policy documentation].</span></span>

    <span data-ttu-id="910d7-141">Néhány hitelesítésszolgáltatókat tartalom-Security-házirend módosítja a megfelelő mobileszközök használatakor nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="910d7-141">Some authentication providers do not require Content-Security-Policy changes when used on appropriate mobile devices.</span></span>  <span data-ttu-id="910d7-142">Például nem tartalom-Security-házirend módosításai szükségesek, ha a Google-hitelesítést használó Android-eszközön.</span><span class="sxs-lookup"><span data-stu-id="910d7-142">For example, no Content-Security-Policy changes are required when using Google authentication on an Android device.</span></span>

3. <span data-ttu-id="910d7-143">Nyissa meg hello `www/js/index.js` szerkesztésre fájlt, keresse meg a hello `onDeviceReady()` metódust, és hello ügyfél létrehozása alatt kódot adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="910d7-143">Open hello `www/js/index.js` file for editing, locate hello `onDeviceReady()` method, and under hello client  creation code add hello following code:</span></span>

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    <span data-ttu-id="910d7-144">Ez a kód hello meglévő kódot, amely hello táblahivatkozás hoz létre, és frissíti a felhasználói felület hello váltja fel.</span><span class="sxs-lookup"><span data-stu-id="910d7-144">This code replaces hello existing code that creates hello table reference and refreshes hello UI.</span></span>

    <span data-ttu-id="910d7-145">hello login() metódus elindítja a hitelesítési hello szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="910d7-145">hello login() method starts authentication with hello provider.</span></span> <span data-ttu-id="910d7-146">hello login() metódus JavaScript ígéret visszaadó aszinkron függvényre.</span><span class="sxs-lookup"><span data-stu-id="910d7-146">hello login() method is an async function that returns a JavaScript Promise.</span></span>  <span data-ttu-id="910d7-147">hello inicializálási hello részeinek hello ígéret válasz belül kerül, hogy nem végre, amíg hello login() metódus befejeződik.</span><span class="sxs-lookup"><span data-stu-id="910d7-147">hello rest of hello initialization is placed inside hello promise response so that it is not executed until hello login() method completes.</span></span>

4. <span data-ttu-id="910d7-148">Hello kódot, amely az előzőekben adott hozzá, cserélje le `SDK_Provider_Name` hello nevet, a bejelentkezés-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="910d7-148">In hello code that you just added, replace `SDK_Provider_Name` with hello name of your login provider.</span></span> <span data-ttu-id="910d7-149">Például az Azure Active Directory használata `client.login('aad')`.</span><span class="sxs-lookup"><span data-stu-id="910d7-149">For example, for Azure Active Directory, use `client.login('aad')`.</span></span>
5. <span data-ttu-id="910d7-150">Futtatja a projektet.</span><span class="sxs-lookup"><span data-stu-id="910d7-150">Run your project.</span></span>  <span data-ttu-id="910d7-151">Hello projekt inicializálása befejeződött, amikor az alkalmazás a kiválasztott hitelesítési szolgáltató hello hello OAuth bejelentkezési lapját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="910d7-151">When hello project has finished initializing, your application shows hello OAuth login page for hello chosen authentication provider.</span></span>

## <span data-ttu-id="910d7-152"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="910d7-152"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="910d7-153">További [vonatkozó hitelesítési] Azure App Service szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="910d7-153">Learn more [About Authentication] with Azure App Service.</span></span>
* <span data-ttu-id="910d7-154">Folytassa a hello oktatóanyag hozzáadásával [leküldéses értesítések] tooyour Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="910d7-154">Continue hello tutorial by adding [Push Notifications] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="910d7-155">Ismerje meg, hogyan toouse hello SDK-k.</span><span class="sxs-lookup"><span data-stu-id="910d7-155">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="910d7-156">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="910d7-156">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="910d7-157">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="910d7-157">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="910d7-158">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="910d7-158">[Node.js Server SDK]</span></span>

<!-- URLs. -->
[Ismerkedés a Mobile Apps]: app-service-mobile-cordova-get-started.md
[tartalom-Security-házirend dokumentációját]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[leküldéses értesítések]: app-service-mobile-cordova-get-started-push.md
[vonatkozó hitelesítési]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
