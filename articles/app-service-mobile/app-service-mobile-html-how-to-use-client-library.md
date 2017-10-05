---
title: "A JavaScript SDK használata az Azure Mobile Apps szolgáltatásban"
description: "Az Azure Mobile Apps v használata"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 0c4b4de560d70592f5bbdee28b56a7686b5689f4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a><span data-ttu-id="5cee1-103">A JavaScript ügyféloldali kódtár használata az Azure Mobile Apps-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="5cee1-103">How to Use the JavaScript client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="5cee1-104">Ez az útmutató útmutatást ad teszi a végrehajtását szolgáltatást a legújabb használó általános forgatókönyvhöz [JavaScript SDK az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="5cee1-104">This guide teaches you to perform common scenarios using the latest [JavaScript SDK for Azure Mobile Apps].</span></span> <span data-ttu-id="5cee1-105">Ha most ismerkedik az Azure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] -háttéralkalmazás létrehozása, és hozzon létre egy táblát.</span><span class="sxs-lookup"><span data-stu-id="5cee1-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend and create a table.</span></span> <span data-ttu-id="5cee1-106">Ebben az útmutatóban azt összpontosítani a mobil-háttéralkalmazások használatával HTML/JavaScript webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5cee1-106">In this guide, we focus on using the mobile backend in HTML/JavaScript Web applications.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5cee1-107">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="5cee1-107">Supported platforms</span></span>
<span data-ttu-id="5cee1-108">Jelenleg korlátozza a ismertebb böngésző aktuális és az utolsó verzióiban támogatott böngésző: Google Chrome, a Microsoft Edge, a Microsoft Internet Explorer vagy a Mozilla Firefox.</span><span class="sxs-lookup"><span data-stu-id="5cee1-108">We limit browser support to the current and last versions of the major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.</span></span>  <span data-ttu-id="5cee1-109">Várhatóan viszonylag modern böngészők függvényt SDK.</span><span class="sxs-lookup"><span data-stu-id="5cee1-109">We expect the SDK to function with any relatively modern browser.</span></span>

<span data-ttu-id="5cee1-110">A csomag terjesztésekor modulként univerzális JavaScript, így támogatja a globális változók globals, AMD, és az CommonJS formátumú.</span><span class="sxs-lookup"><span data-stu-id="5cee1-110">The package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.</span></span>

## <span data-ttu-id="5cee1-111"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5cee1-111"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="5cee1-112">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5cee1-112">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="5cee1-113">Ez az útmutató feltételezi, hogy rendelkezik-e a tábla a táblák ugyanazon séma ezen oktatóprogram a.</span><span class="sxs-lookup"><span data-stu-id="5cee1-113">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span>

<span data-ttu-id="5cee1-114">Az Azure Mobile Apps JavaScript SDK telepítése megteheti a `npm` parancs:</span><span class="sxs-lookup"><span data-stu-id="5cee1-114">Installing the Azure Mobile Apps JavaScript SDK can be done via the `npm` command:</span></span>

```
npm install azure-mobile-apps-client --save
```

<span data-ttu-id="5cee1-115">A könyvtár egy ES2015 modul, például Browserify és Webpack és egy AMD tárként CommonJS környezetekben is használható.</span><span class="sxs-lookup"><span data-stu-id="5cee1-115">The library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.</span></span>  <span data-ttu-id="5cee1-116">Példa:</span><span class="sxs-lookup"><span data-stu-id="5cee1-116">For example:</span></span>

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

<span data-ttu-id="5cee1-117">Az SDK előre elkészített verziójának úgy, hogy letölti a CDN-ről is használhatja:</span><span class="sxs-lookup"><span data-stu-id="5cee1-117">You can also use a pre-built version of the SDK by downloading directly from our CDN:</span></span>

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="5cee1-118"><a name="auth"></a>Hogyan: hitelesíti a felhasználókat</span><span class="sxs-lookup"><span data-stu-id="5cee1-118"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="5cee1-119">Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter.</span><span class="sxs-lookup"><span data-stu-id="5cee1-119">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="5cee1-120">A engedélyeket korlátozhatja a hozzáférést a megadott művelet csak a hitelesített felhasználók táblákon.</span><span class="sxs-lookup"><span data-stu-id="5cee1-120">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="5cee1-121">Hitelesített felhasználók identitásának használhatja, ha az engedélyezési szabályok megvalósítását a kiszolgáló parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="5cee1-121">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="5cee1-122">További információkért lásd: a [Bevezetés a hitelesítés használatába] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="5cee1-122">For more information, see the [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="5cee1-123">Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.</span><span class="sxs-lookup"><span data-stu-id="5cee1-123">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="5cee1-124">A kiszolgáló folyamata nyújt a legegyszerűbb felhasználói hitelesítés, a szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="5cee1-124">The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="5cee1-125">Az ügyféltanúsítvány-folyamat lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói SDK-k támaszkodnak az.</span><span class="sxs-lookup"><span data-stu-id="5cee1-125">The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="5cee1-126"><a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="5cee1-126"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="5cee1-127">A JavaScript-alkalmazások számos különböző egy visszacsatolási funkció segítségével OAuth UI-forgalom kezelésére.</span><span class="sxs-lookup"><span data-stu-id="5cee1-127">Several types of JavaScript applications use a loopback capability to handle OAuth UI flows.</span></span>  <span data-ttu-id="5cee1-128">Ezek a képességek közé tartoznak:</span><span class="sxs-lookup"><span data-stu-id="5cee1-128">These capabilities include:</span></span>

* <span data-ttu-id="5cee1-129">A szolgáltatás helyben fut</span><span class="sxs-lookup"><span data-stu-id="5cee1-129">Running your service locally</span></span>
* <span data-ttu-id="5cee1-130">Élő Újrabetöltés használata a ionos keretrendszer</span><span class="sxs-lookup"><span data-stu-id="5cee1-130">Using Live Reload with the Ionic Framework</span></span>
* <span data-ttu-id="5cee1-131">App Service hitelesítés irányít át.</span><span class="sxs-lookup"><span data-stu-id="5cee1-131">Redirecting to App Service for authentication.</span></span>

<span data-ttu-id="5cee1-132">Helyileg futó problémákat okozhat, mert alapértelmezés szerint az App Service hitelesítés csak engedélyezésére van konfigurálva a Mobile Apps-háttéralkalmazás való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="5cee1-132">Running locally can cause problems because, by default, App Service authentication is only configured to allow access from your Mobile App backend.</span></span> <span data-ttu-id="5cee1-133">Az alábbi lépések segítségével módosíthatja az App Service-beállítások a kiszolgáló a helyi futtatás során a hitelesítés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="5cee1-133">Use the following steps to change the App Service settings to enable authentication when running the server locally:</span></span>

1. <span data-ttu-id="5cee1-134">Jelentkezzen be az [Azure Portalra]</span><span class="sxs-lookup"><span data-stu-id="5cee1-134">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="5cee1-135">Nyissa meg a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="5cee1-135">Navigate to your Mobile App backend.</span></span>
3. <span data-ttu-id="5cee1-136">Válassza ki **erőforrás-kezelő** a a **FEJLESZTŐESZKÖZÖK** menü.</span><span class="sxs-lookup"><span data-stu-id="5cee1-136">Select **Resource explorer** in the **DEVELOPMENT TOOLS** menu.</span></span>
4. <span data-ttu-id="5cee1-137">Kattintson a **Ugrás** az erőforrás-kezelő, a mobilalkalmazás háttérrendszerének egy új lapot vagy ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5cee1-137">Click **Go** to open the resource explorer for your Mobile App backend in a new tab or window.</span></span>
5. <span data-ttu-id="5cee1-138">Bontsa ki a **config** > **authsettings** csomópont az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="5cee1-138">Expand the **config** > **authsettings** node for your app.</span></span>
6. <span data-ttu-id="5cee1-139">Kattintson a **szerkesztése** ahhoz, hogy az erőforrás Szerkesztés gomb.</span><span class="sxs-lookup"><span data-stu-id="5cee1-139">Click the **Edit** button to enable editing of the resource.</span></span>
7. <span data-ttu-id="5cee1-140">Keresés a **allowedExternalRedirectUrls** elemmel rendelkezik, ami null értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5cee1-140">Find the **allowedExternalRedirectUrls** element, which should be null.</span></span> <span data-ttu-id="5cee1-141">Adja hozzá az URL-címek tömbjét:</span><span class="sxs-lookup"><span data-stu-id="5cee1-141">Add your URLs in an array:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="5cee1-142">Az URL-címeket, a tömb cserélje le a szolgáltatást, amely ebben a példában az URL-címek `http://localhost:3000` a Node.js-mintát helyi szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5cee1-142">Replace the URLs in the array with the URLs of your service, which in this example is `http://localhost:3000` for the local Node.js sample service.</span></span> <span data-ttu-id="5cee1-143">Is `http://localhost:4400` a Fodrozás szolgáltatás vagy egy másik URL-cím, attól függően, hogy az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="5cee1-143">You could also use `http://localhost:4400` for the Ripple service or some other URL, depending on how your app is configured.</span></span>
8. <span data-ttu-id="5cee1-144">Kattintson a lap tetején **olvasási/írási**, majd kattintson a **PUT** a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5cee1-144">At the top of the page, click **Read/Write**, then click **PUT** to save your updates.</span></span>

<span data-ttu-id="5cee1-145">Is szüksége lesz, ugyanazon visszacsatolási URL-címek hozzáadása a CORS-engedélyezési beállítások:</span><span class="sxs-lookup"><span data-stu-id="5cee1-145">You also need to add the same loopback URLs to the CORS whitelist settings:</span></span>

1. <span data-ttu-id="5cee1-146">Lépjen vissza a [Azure Portalra].</span><span class="sxs-lookup"><span data-stu-id="5cee1-146">Navigate back to the [Azure portal].</span></span>
2. <span data-ttu-id="5cee1-147">Nyissa meg a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="5cee1-147">Navigate to your Mobile App backend.</span></span>
3. <span data-ttu-id="5cee1-148">Kattintson a **CORS** a a **API** menü.</span><span class="sxs-lookup"><span data-stu-id="5cee1-148">Click **CORS** in the **API** menu.</span></span>
4. <span data-ttu-id="5cee1-149">Adja meg az egyes URL-címet az üres **engedélyezett eredetet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="5cee1-149">Enter each URL in the empty **Allowed Origins** text box.</span></span>  <span data-ttu-id="5cee1-150">Egy új szövegmező jön létre.</span><span class="sxs-lookup"><span data-stu-id="5cee1-150">A new text box is created.</span></span>
5. <span data-ttu-id="5cee1-151">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="5cee1-151">Click **SAVE**</span></span>

<span data-ttu-id="5cee1-152">Miután a háttér-frissítéseket, lesz, az új visszacsatolási URL-címek használatához az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5cee1-152">After the backend updates, you will be able to use the new loopback URLs in your app.</span></span>

<!-- URLs. -->
<span data-ttu-id="5cee1-153">[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="5cee1-153">[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="5cee1-154">[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="5cee1-154">[Get started with authentication]: app-service-mobile-cordova-get-started-users.md</span></span>
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

<span data-ttu-id="5cee1-155">[Azure Portalra]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="5cee1-155">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="5cee1-156">[JavaScript SDK az Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client</span><span class="sxs-lookup"><span data-stu-id="5cee1-156">[JavaScript SDK for Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client</span></span>
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
