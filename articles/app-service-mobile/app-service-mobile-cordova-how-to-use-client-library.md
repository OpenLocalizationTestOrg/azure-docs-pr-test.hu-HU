---
title: "Apache Cordova beépülő modul használata az Azure Mobile Apps szolgáltatásban"
description: "Apache Cordova beépülő modul használata az Azure Mobile Apps szolgáltatásban"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: ebf0e911eeada0e529f908dd3e3430c94edae763
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a><span data-ttu-id="3b085-103">Az Azure Mobile Apps Apache Cordova ügyféloldali kódtár használata</span><span class="sxs-lookup"><span data-stu-id="3b085-103">How to use Apache Cordova client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="3b085-104">Ez az útmutató útmutatást ad teszi a végrehajtását szolgáltatást a legújabb használó általános forgatókönyvhöz [Apache Cordova beépülő modul az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="3b085-104">This guide teaches you to perform common scenarios using the latest [Apache Cordova Plugin for Azure Mobile Apps].</span></span> <span data-ttu-id="3b085-105">Ha most ismerkedik az Azure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] -háttéralkalmazás létrehozása, hozzon létre egy táblát, és egy előre elkészített Apache Cordova-projekt letöltése.</span><span class="sxs-lookup"><span data-stu-id="3b085-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend, create a table, and download a pre-built Apache Cordova project.</span></span> <span data-ttu-id="3b085-106">Az útmutató azt összpontosítanak az ügyféloldali Apache Cordova beépülő modul is.</span><span class="sxs-lookup"><span data-stu-id="3b085-106">In this guide, we focus on the client-side Apache Cordova Plugin.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3b085-107">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="3b085-107">Supported platforms</span></span>
<span data-ttu-id="3b085-108">Ez az SDK támogatja az Apache Cordova v6.0.0 és későbbi iOS, Android és Windows eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="3b085-108">This SDK supports Apache Cordova v6.0.0 and later on iOS, Android, and Windows devices.</span></span>  <span data-ttu-id="3b085-109">A platform támogatása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="3b085-109">The platform support is as follows:</span></span>

* <span data-ttu-id="3b085-110">Android API 19-24 (KitKat nugát keresztül).</span><span class="sxs-lookup"><span data-stu-id="3b085-110">Android API 19-24 (KitKat through Nougat).</span></span>
* <span data-ttu-id="3b085-111">iOS 8.0-s és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="3b085-111">iOS versions 8.0 and later.</span></span>
* <span data-ttu-id="3b085-112">Windows Phone 8.1-es.</span><span class="sxs-lookup"><span data-stu-id="3b085-112">Windows Phone 8.1.</span></span>
* <span data-ttu-id="3b085-113">Az univerzális Windows Platform.</span><span class="sxs-lookup"><span data-stu-id="3b085-113">Universal Windows Platform.</span></span>

## <span data-ttu-id="3b085-114"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3b085-114"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="3b085-115">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3b085-115">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="3b085-116">Ez az útmutató feltételezi, hogy rendelkezik-e a tábla a táblák ugyanazon séma ezen oktatóprogram a.</span><span class="sxs-lookup"><span data-stu-id="3b085-116">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span> <span data-ttu-id="3b085-117">Ez az útmutató feltételezi, hogy hozzáadta az Apache Cordova beépülő modul a kódot.</span><span class="sxs-lookup"><span data-stu-id="3b085-117">This guide also assumes that you have added the Apache Cordova Plugin to your code.</span></span>  <span data-ttu-id="3b085-118">Ha nem tette, előfordulhat, hogy adja hozzá az Apache Cordova beépülő modul a projekthez a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="3b085-118">If you have not done so, you may add the Apache Cordova plugin to your project on the command line:</span></span>

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="3b085-119">További létrehozásával kapcsolatos információkat [az első Apache Cordova-alkalmazás], azok dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="3b085-119">For more information on creating [your first Apache Cordova app], see their documentation.</span></span>

## <span data-ttu-id="3b085-120"><a name="ionic"></a>Ionos v2 alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="3b085-120"><a name="ionic"></a>Setting up an Ionic v2 app</span></span>

<span data-ttu-id="3b085-121">Megfelelően konfigurálni ionos v2 projektben, hozzon létre egy alapszintű alkalmazást, majd adja hozzá a Cordova beépülő modul:</span><span class="sxs-lookup"><span data-stu-id="3b085-121">To properly configure an Ionic v2 project, first create a basic app and add the Cordova plugin:</span></span>

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="3b085-122">Adja hozzá a következő sorokat `app.component.ts` az ügyfél objektum létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="3b085-122">Add the following lines to `app.component.ts` to create the client object:</span></span>

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

<span data-ttu-id="3b085-123">Most már létrehozhatja és a projekt futtatását a böngészőben:</span><span class="sxs-lookup"><span data-stu-id="3b085-123">You can now build and run the project in the browser:</span></span>

```
ionic platform add browser
ionic run browser
```

<span data-ttu-id="3b085-124">Az Azure Mobile Apps Cordova beépülő modul mindkét ionos v1 és v2 alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="3b085-124">The Azure Mobile Apps Cordova plugin supports both Ionic v1 and v2 apps.</span></span>  <span data-ttu-id="3b085-125">Csak az ionos v2 alkalmazásokhoz szükséges további deklaráció található a `WindowsAzure` objektum.</span><span class="sxs-lookup"><span data-stu-id="3b085-125">Only the Ionic v2 apps require the additional declaration for the `WindowsAzure` object.</span></span>

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="3b085-126"><a name="auth"></a>Hogyan: hitelesíti a felhasználókat</span><span class="sxs-lookup"><span data-stu-id="3b085-126"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="3b085-127">Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter.</span><span class="sxs-lookup"><span data-stu-id="3b085-127">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="3b085-128">A engedélyeket korlátozhatja a hozzáférést a megadott művelet csak a hitelesített felhasználók táblákon.</span><span class="sxs-lookup"><span data-stu-id="3b085-128">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="3b085-129">Hitelesített felhasználók identitásának használhatja, ha az engedélyezési szabályok megvalósítását a kiszolgáló parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="3b085-129">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="3b085-130">További információkért lásd: a [Bevezetés a hitelesítés használatába] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3b085-130">For more information, see the [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="3b085-131">Ha hitelesítési Apache Cordova-alkalmazást használ, a következő Cordova beépülő modulok elérhetőnek kell lennie:</span><span class="sxs-lookup"><span data-stu-id="3b085-131">When using authentication in an Apache Cordova app, the following Cordova plugins must be available:</span></span>

* <span data-ttu-id="3b085-132">[cordova-beépülőmodul-eszköz]</span><span class="sxs-lookup"><span data-stu-id="3b085-132">[cordova-plugin-device]</span></span>
* <span data-ttu-id="3b085-133">[cordova-beépülőmodul-inappbrowser]</span><span class="sxs-lookup"><span data-stu-id="3b085-133">[cordova-plugin-inappbrowser]</span></span>

<span data-ttu-id="3b085-134">Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.</span><span class="sxs-lookup"><span data-stu-id="3b085-134">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="3b085-135">A kiszolgáló folyamata nyújt a legegyszerűbb felhasználói hitelesítés, a szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="3b085-135">The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="3b085-136">Az ügyféltanúsítvány-folyamat lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói eszközspecifikus SDK-k támaszkodnak az.</span><span class="sxs-lookup"><span data-stu-id="3b085-136">The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific device-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="3b085-137"><a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="3b085-137"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="3b085-138">Az Apache Cordova-alkalmazások számos különböző egy visszacsatolási funkció segítségével OAuth UI-forgalom kezelésére.</span><span class="sxs-lookup"><span data-stu-id="3b085-138">Several types of Apache Cordova applications use a loopback capability to handle OAuth UI flows.</span></span>  <span data-ttu-id="3b085-139">OAuth UI adatfolyamok – localhost problémákat okozhat, mert a hitelesítési szolgáltatás csak tudja, hol használják a szolgáltatás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="3b085-139">OAuth UI flows on localhost cause problems since the authentication service only knows how to utilize your service by default.</span></span>  <span data-ttu-id="3b085-140">Hibás OAuth UI flow például a következők:</span><span class="sxs-lookup"><span data-stu-id="3b085-140">Examples of problematic OAuth UI flows include:</span></span>

* <span data-ttu-id="3b085-141">A Fodrozás emulátor.</span><span class="sxs-lookup"><span data-stu-id="3b085-141">The Ripple emulator.</span></span>
* <span data-ttu-id="3b085-142">Élő rendelkező ionos be újra.</span><span class="sxs-lookup"><span data-stu-id="3b085-142">Live Reload with Ionic.</span></span>
* <span data-ttu-id="3b085-143">A mobil-háttéralkalmazást helyileg futó</span><span class="sxs-lookup"><span data-stu-id="3b085-143">Running the mobile backend locally</span></span>
* <span data-ttu-id="3b085-144">A mobil-háttéralkalmazást fut egy másik Azure App Service, mint a egy olyan hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="3b085-144">Running the mobile backend in a different Azure App Service than the one providing authentication.</span></span>

<span data-ttu-id="3b085-145">Kövesse ezeket az utasításokat a helyi beállításokat hozzáadni a konfigurációhoz:</span><span class="sxs-lookup"><span data-stu-id="3b085-145">Follow these instructions to add your local settings to the configuration:</span></span>

1. <span data-ttu-id="3b085-146">Jelentkezzen be az [Azure Portalra]</span><span class="sxs-lookup"><span data-stu-id="3b085-146">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="3b085-147">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="3b085-147">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="3b085-148">Kattintson a **eszközök**</span><span class="sxs-lookup"><span data-stu-id="3b085-148">Click **Tools**</span></span>
4. <span data-ttu-id="3b085-149">Kattintson a **erőforrás-kezelő** OBSERVE menüben kattintson a **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="3b085-149">Click **Resource explorer** in the OBSERVE menu, then click **Go**.</span></span>  <span data-ttu-id="3b085-150">Megnyílik egy új ablakot vagy lapot.</span><span class="sxs-lookup"><span data-stu-id="3b085-150">A new window or tab opens.</span></span>
5. <span data-ttu-id="3b085-151">Bontsa ki a **config**, **authsettings** a helyhez, a bal oldali navigációs csomópontok.</span><span class="sxs-lookup"><span data-stu-id="3b085-151">Expand the **config**, **authsettings** nodes for your site in the left-hand navigation.</span></span>
6. <span data-ttu-id="3b085-152">Kattintson a **szerkesztése**</span><span class="sxs-lookup"><span data-stu-id="3b085-152">Click **Edit**</span></span>
7. <span data-ttu-id="3b085-153">Keresse meg a "allowedExternalRedirectUrls" elemet.</span><span class="sxs-lookup"><span data-stu-id="3b085-153">Look for the "allowedExternalRedirectUrls" element.</span></span>  <span data-ttu-id="3b085-154">NULL értékű vagy egy tömböt is beállítható.</span><span class="sxs-lookup"><span data-stu-id="3b085-154">It may be set to null or an array of values.</span></span>  <span data-ttu-id="3b085-155">Módosítsa az értéket a következő értéket:</span><span class="sxs-lookup"><span data-stu-id="3b085-155">Change the value to the following value:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="3b085-156">Cserélje le az URL-címeket a szolgáltatás URL-címei.</span><span class="sxs-lookup"><span data-stu-id="3b085-156">Replace the URLs with the URLs of your service.</span></span>  <span data-ttu-id="3b085-157">Például a "http://localhost: 3000" (a Node.js sample szolgáltatás), vagy "http://localhost:4400" (a Fodrozás szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="3b085-157">Examples include "http://localhost:3000" (for the Node.js sample service), or "http://localhost:4400" (for the Ripple service).</span></span>  <span data-ttu-id="3b085-158">Azonban ezen URL-címei, példák - a helyzet, beleértve a szolgáltatások a példákban eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="3b085-158">However, these URLs are examples - your situation, including for the services mentioned in the examples, may be different.</span></span>
8. <span data-ttu-id="3b085-159">Kattintson a **olvasási/írási** gombra a képernyő jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="3b085-159">Click the **Read/Write** button in the top-right corner of the screen.</span></span>
9. <span data-ttu-id="3b085-160">Kattintson a zöld **PUT** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b085-160">Click the green **PUT** button.</span></span>

<span data-ttu-id="3b085-161">A beállítások mentése ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="3b085-161">The settings are saved at this point.</span></span>  <span data-ttu-id="3b085-162">Ne zárja be a böngészőablakot amíg tart a beállítások mentése.</span><span class="sxs-lookup"><span data-stu-id="3b085-162">Do not close the browser window until the settings have finished saving.</span></span>
<span data-ttu-id="3b085-163">Ezek visszacsatolási URL-címeket is hozzá az App Service CORS-beállítások:</span><span class="sxs-lookup"><span data-stu-id="3b085-163">Also add these loopback URLs to the CORS settings for your App Service:</span></span>

1. <span data-ttu-id="3b085-164">Jelentkezzen be az [Azure Portalra]</span><span class="sxs-lookup"><span data-stu-id="3b085-164">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="3b085-165">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="3b085-165">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="3b085-166">A beállítások panelről automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="3b085-166">The Settings blade opens automatically.</span></span>  <span data-ttu-id="3b085-167">Ha nem, kattintson a gombra **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="3b085-167">If it doesn't, click **All Settings**.</span></span>
4. <span data-ttu-id="3b085-168">Kattintson a **CORS** az API menüjében.</span><span class="sxs-lookup"><span data-stu-id="3b085-168">Click **CORS** under the API menu.</span></span>
5. <span data-ttu-id="3b085-169">A mezőbe a felvenni kívánt URL-cím megadva, és nyomja le az Enter billentyűt.</span><span class="sxs-lookup"><span data-stu-id="3b085-169">Enter the URL that you wish to add in the box provided and press Enter.</span></span>
6. <span data-ttu-id="3b085-170">Adja meg a további URL-címek, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="3b085-170">Enter additional URLs as needed.</span></span>
7. <span data-ttu-id="3b085-171">Kattintson a **mentése** menti a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3b085-171">Click **Save** to save the settings.</span></span>

<span data-ttu-id="3b085-172">Körülbelül 10 – 15 másodperc az új beállítások érvénybe léptetéséhez vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3b085-172">It takes approximately 10-15 seconds for the new settings to take effect.</span></span>

## <span data-ttu-id="3b085-173"><a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3b085-173"><a name="register-for-push"></a>How to: Register for push notifications</span></span>
<span data-ttu-id="3b085-174">Telepítse a [phonegap-beépülőmodul-leküldéses] leküldéses értesítések kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3b085-174">Install the [phonegap-plugin-push] to handle push notifications.</span></span>  <span data-ttu-id="3b085-175">A beépülő modul könnyen segítségével adhatók a `cordova plugin add` parancsot a parancssorban, vagy a Git beépülő modul telepítő Visual Studio segítségével.</span><span class="sxs-lookup"><span data-stu-id="3b085-175">This plugin can be easily added using the `cordova plugin add` command on the command line, or via the Git plugin installer within Visual Studio.</span></span>  <span data-ttu-id="3b085-176">A következő kódot az Apache Cordova-alkalmazás regisztrálja az eszközt a leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="3b085-176">The following code in your Apache Cordova app registers your device for push notifications:</span></span>

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

<span data-ttu-id="3b085-177">A Notification Hubs SDK használatával leküldéses értesítéseket küldeni a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="3b085-177">Use the Notification Hubs SDK to send push notifications from the server.</span></span>  <span data-ttu-id="3b085-178">Soha ne küldjön leküldéses értesítéseket közvetlenül az ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="3b085-178">Never send push notifications directly from clients.</span></span> <span data-ttu-id="3b085-179">Ezzel a szolgáltatásmegtagadási támadások elleni Notification hubs használatával vagy a pns-sel való is használható.</span><span class="sxs-lookup"><span data-stu-id="3b085-179">Doing so could be used to trigger a denial of service attack against Notification Hubs or the PNS.</span></span>  <span data-ttu-id="3b085-180">A pns-sel sikerült bA a forgalom miatt az ilyen támadásokat.</span><span class="sxs-lookup"><span data-stu-id="3b085-180">The PNS could ban your traffic as a result of such attacks.</span></span>

## <a name="more-information"></a><span data-ttu-id="3b085-181">További információ</span><span class="sxs-lookup"><span data-stu-id="3b085-181">More information</span></span>

<span data-ttu-id="3b085-182">A részletes API részletei a [API-JÁNAK dokumentációja](http://azure.github.io/azure-mobile-apps-js-client/).</span><span class="sxs-lookup"><span data-stu-id="3b085-182">You can find detailed API details in our [API documentation](http://azure.github.io/azure-mobile-apps-js-client/).</span></span>

<!-- URLs. -->
<span data-ttu-id="3b085-183">[Azure Portalra]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="3b085-183">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="3b085-184">[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="3b085-184">[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="3b085-185">[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="3b085-185">[Get started with authentication]: app-service-mobile-cordova-get-started-users.md</span></span>
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

<span data-ttu-id="3b085-186">[Apache Cordova beépülő modul az Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span><span class="sxs-lookup"><span data-stu-id="3b085-186">[Apache Cordova Plugin for Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span></span>
<span data-ttu-id="3b085-187">[az első Apache Cordova-alkalmazás]: http://cordova.apache.org/#getstarted</span><span class="sxs-lookup"><span data-stu-id="3b085-187">[your first Apache Cordova app]: http://cordova.apache.org/#getstarted</span></span>
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
<span data-ttu-id="3b085-188">[phonegap-beépülőmodul-leküldéses]: https://www.npmjs.com/package/phonegap-plugin-push</span><span class="sxs-lookup"><span data-stu-id="3b085-188">[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push</span></span>
<span data-ttu-id="3b085-189">[cordova-beépülőmodul-eszköz]: https://www.npmjs.com/package/cordova-plugin-device</span><span class="sxs-lookup"><span data-stu-id="3b085-189">[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device</span></span>
<span data-ttu-id="3b085-190">[cordova-beépülőmodul-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span><span class="sxs-lookup"><span data-stu-id="3b085-190">[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span></span>
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
