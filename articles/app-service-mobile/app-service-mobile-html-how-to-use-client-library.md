---
title: "aaaHow tooUse hello JavaScript SDK az Azure Mobile Apps-alkalmazáshoz"
description: "Hogyan tooUse v Azure Mobile Apps-alkalmazáshoz"
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
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a><span data-ttu-id="82056-103">Hogyan tooUse hello Azure Mobile Apps a JavaScript ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="82056-103">How tooUse hello JavaScript client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="82056-104">Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [JavaScript SDK az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="82056-104">This guide teaches you tooperform common scenarios using hello latest [JavaScript SDK for Azure Mobile Apps].</span></span> <span data-ttu-id="82056-105">Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate háttérkiszolgálón és hozzon létre egy táblát.</span><span class="sxs-lookup"><span data-stu-id="82056-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend and create a table.</span></span> <span data-ttu-id="82056-106">Az útmutató azt összpontosítanak hello mobil-háttéralkalmazások használatával HTML/JavaScript webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="82056-106">In this guide, we focus on using hello mobile backend in HTML/JavaScript Web applications.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="82056-107">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="82056-107">Supported platforms</span></span>
<span data-ttu-id="82056-108">Azt böngésző támogatási toohello aktuális korlátjának növelését, és az utolsó hello verziói ismertebb böngésző: Google Chrome, a Microsoft Edge, a Microsoft Internet Explorer vagy a Mozilla Firefox.</span><span class="sxs-lookup"><span data-stu-id="82056-108">We limit browser support toohello current and last versions of hello major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.</span></span>  <span data-ttu-id="82056-109">Várhatóan hello SDK toofunction minden viszonylag modern böngészőben.</span><span class="sxs-lookup"><span data-stu-id="82056-109">We expect hello SDK toofunction with any relatively modern browser.</span></span>

<span data-ttu-id="82056-110">hello csomag terjesztése egy univerzális JavaScript-modult, így támogatja a globális változók globals, AMD, és az CommonJS formátumú.</span><span class="sxs-lookup"><span data-stu-id="82056-110">hello package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.</span></span>

## <span data-ttu-id="82056-111"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="82056-111"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="82056-112">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="82056-112">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="82056-113">Ez az útmutató feltételezi, hogy hello táblához hello hello táblák ezen oktatóprogram az azonos sémából.</span><span class="sxs-lookup"><span data-stu-id="82056-113">This guide assumes that hello table has hello same schema as hello tables in those tutorials.</span></span>

<span data-ttu-id="82056-114">Hello Azure Mobile Apps JavaScript SDK telepítése megteheti a hello `npm` parancs:</span><span class="sxs-lookup"><span data-stu-id="82056-114">Installing hello Azure Mobile Apps JavaScript SDK can be done via hello `npm` command:</span></span>

```
npm install azure-mobile-apps-client --save
```

<span data-ttu-id="82056-115">hello könyvtár egy ES2015 modul, például Browserify és Webpack és egy AMD tárként CommonJS környezetekben is használható.</span><span class="sxs-lookup"><span data-stu-id="82056-115">hello library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.</span></span>  <span data-ttu-id="82056-116">Példa:</span><span class="sxs-lookup"><span data-stu-id="82056-116">For example:</span></span>

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

<span data-ttu-id="82056-117">Hello SDK egy előre elkészített verziója is használható, ha úgy, hogy letölti a CDN-ről:</span><span class="sxs-lookup"><span data-stu-id="82056-117">You can also use a pre-built version of hello SDK by downloading directly from our CDN:</span></span>

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="82056-118"><a name="auth"></a>Hogyan: hitelesíti a felhasználókat</span><span class="sxs-lookup"><span data-stu-id="82056-118"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="82056-119">Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter.</span><span class="sxs-lookup"><span data-stu-id="82056-119">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="82056-120">Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="82056-120">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="82056-121">Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="82056-121">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="82056-122">További információkért lásd: hello [Bevezetés a hitelesítés használatába] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="82056-122">For more information, see hello [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="82056-123">Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.</span><span class="sxs-lookup"><span data-stu-id="82056-123">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="82056-124">hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="82056-124">hello server flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="82056-125">hello ügyfél folyamata lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói SDK-k támaszkodnak az.</span><span class="sxs-lookup"><span data-stu-id="82056-125">hello client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="82056-126"><a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="82056-126"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="82056-127">A JavaScript-alkalmazások számos különböző használja egy visszacsatolási funkció toohandle OAuth UI zajlik.</span><span class="sxs-lookup"><span data-stu-id="82056-127">Several types of JavaScript applications use a loopback capability toohandle OAuth UI flows.</span></span>  <span data-ttu-id="82056-128">Ezek a képességek közé tartoznak:</span><span class="sxs-lookup"><span data-stu-id="82056-128">These capabilities include:</span></span>

* <span data-ttu-id="82056-129">A szolgáltatás helyben fut</span><span class="sxs-lookup"><span data-stu-id="82056-129">Running your service locally</span></span>
* <span data-ttu-id="82056-130">Élő Újrabetöltés hello ionos keretrendszer használatával</span><span class="sxs-lookup"><span data-stu-id="82056-130">Using Live Reload with hello Ionic Framework</span></span>
* <span data-ttu-id="82056-131">A hitelesítési szolgáltatás tooApp átirányítása.</span><span class="sxs-lookup"><span data-stu-id="82056-131">Redirecting tooApp Service for authentication.</span></span>

<span data-ttu-id="82056-132">Helyileg futó problémákat okozhat, mert alapértelmezés szerint csak az App Service tooallow a hozzáférést a Mobile Apps-háttéralkalmazását konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="82056-132">Running locally can cause problems because, by default, App Service authentication is only configured tooallow access from your Mobile App backend.</span></span> <span data-ttu-id="82056-133">Hello server a helyi futtatás során a következő lépéseket toochange hello App Service beállítások tooenable hitelesítési hello használata:</span><span class="sxs-lookup"><span data-stu-id="82056-133">Use hello following steps toochange hello App Service settings tooenable authentication when running hello server locally:</span></span>

1. <span data-ttu-id="82056-134">Jelentkezzen be toohello [Azure-portálon]</span><span class="sxs-lookup"><span data-stu-id="82056-134">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="82056-135">Lépjen a Mobile Apps-háttéralkalmazás tooyour.</span><span class="sxs-lookup"><span data-stu-id="82056-135">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="82056-136">Válassza ki **erőforrás-kezelő** a hello **FEJLESZTŐESZKÖZÖK** menü.</span><span class="sxs-lookup"><span data-stu-id="82056-136">Select **Resource explorer** in hello **DEVELOPMENT TOOLS** menu.</span></span>
4. <span data-ttu-id="82056-137">Kattintson a **Ugrás** tooopen hello erőforrás-kezelő a Mobile Apps-háttéralkalmazás egy új lapot vagy ablakban.</span><span class="sxs-lookup"><span data-stu-id="82056-137">Click **Go** tooopen hello resource explorer for your Mobile App backend in a new tab or window.</span></span>
5. <span data-ttu-id="82056-138">Bontsa ki a hello **config** > **authsettings** csomópont az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="82056-138">Expand hello **config** > **authsettings** node for your app.</span></span>
6. <span data-ttu-id="82056-139">Kattintson a hello **szerkesztése** tooenable hello erőforrás Szerkesztés gomb.</span><span class="sxs-lookup"><span data-stu-id="82056-139">Click hello **Edit** button tooenable editing of hello resource.</span></span>
7. <span data-ttu-id="82056-140">Hello található **allowedExternalRedirectUrls** elemmel rendelkezik, ami null értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82056-140">Find hello **allowedExternalRedirectUrls** element, which should be null.</span></span> <span data-ttu-id="82056-141">Adja hozzá az URL-címek tömbjét:</span><span class="sxs-lookup"><span data-stu-id="82056-141">Add your URLs in an array:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="82056-142">Hello URL-címek hello tömb cserélje le a szolgáltatás, amely ebben a példában hello URL-címei `http://localhost:3000` hello helyi Node.js sample szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82056-142">Replace hello URLs in hello array with hello URLs of your service, which in this example is `http://localhost:3000` for hello local Node.js sample service.</span></span> <span data-ttu-id="82056-143">Is `http://localhost:4400` hello Fodrozás szolgáltatás vagy egy másik URL-cím, attól függően, hogy az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="82056-143">You could also use `http://localhost:4400` for hello Ripple service or some other URL, depending on how your app is configured.</span></span>
8. <span data-ttu-id="82056-144">Hello hello oldal tetején, kattintson a **olvasási/írási**, majd kattintson a **PUT** toosave a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="82056-144">At hello top of hello page, click **Read/Write**, then click **PUT** toosave your updates.</span></span>

<span data-ttu-id="82056-145">Szükség tooadd hello azonos visszacsatolási URL-címek toohello CORS engedélyezési lista beállításait:</span><span class="sxs-lookup"><span data-stu-id="82056-145">You also need tooadd hello same loopback URLs toohello CORS whitelist settings:</span></span>

1. <span data-ttu-id="82056-146">Keresse meg a visszafelé toohello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="82056-146">Navigate back toohello [Azure portal].</span></span>
2. <span data-ttu-id="82056-147">Lépjen a Mobile Apps-háttéralkalmazás tooyour.</span><span class="sxs-lookup"><span data-stu-id="82056-147">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="82056-148">Kattintson a **CORS** a hello **API** menü.</span><span class="sxs-lookup"><span data-stu-id="82056-148">Click **CORS** in hello **API** menu.</span></span>
4. <span data-ttu-id="82056-149">Adja meg minden egyes URL-címe üres hello **engedélyezett eredetet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="82056-149">Enter each URL in hello empty **Allowed Origins** text box.</span></span>  <span data-ttu-id="82056-150">Egy új szövegmező jön létre.</span><span class="sxs-lookup"><span data-stu-id="82056-150">A new text box is created.</span></span>
5. <span data-ttu-id="82056-151">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="82056-151">Click **SAVE**</span></span>

<span data-ttu-id="82056-152">Után hello háttér frissíti, az alkalmazás fogja tudni toouse hello új visszacsatolási URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="82056-152">After hello backend updates, you will be able toouse hello new loopback URLs in your app.</span></span>

<!-- URLs. -->
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Azure-portálon]: https://portal.azure.com/
[JavaScript SDK az Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
