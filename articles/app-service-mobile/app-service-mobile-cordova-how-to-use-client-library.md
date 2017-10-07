---
title: "Apache Cordova beépülő modul az Azure Mobile Apps aaaHow tooUse"
description: "Hogyan tooUse Apache Cordova beépülő modul Azure Mobile Apps-alkalmazáshoz"
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
ms.openlocfilehash: d3e0639e6478c409132af25304a2fb0f28401e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-apache-cordova-client-library-for-azure-mobile-apps"></a><span data-ttu-id="83595-103">Hogyan toouse Apache Cordova ügyféloldali kódtár Azure Mobile Apps-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="83595-103">How toouse Apache Cordova client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="83595-104">Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [Apache Cordova beépülő modul az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="83595-104">This guide teaches you tooperform common scenarios using hello latest [Apache Cordova Plugin for Azure Mobile Apps].</span></span> <span data-ttu-id="83595-105">Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate egy háttér, hozzon létre egy táblát, és egy előre elkészített Apache Cordova-projekt letöltése.</span><span class="sxs-lookup"><span data-stu-id="83595-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend, create a table, and download a pre-built Apache Cordova project.</span></span> <span data-ttu-id="83595-106">Az útmutató azt összpontosítani hello ügyféloldali Apache Cordova beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="83595-106">In this guide, we focus on hello client-side Apache Cordova Plugin.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="83595-107">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="83595-107">Supported platforms</span></span>
<span data-ttu-id="83595-108">Ez az SDK támogatja az Apache Cordova v6.0.0 és későbbi iOS, Android és Windows eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="83595-108">This SDK supports Apache Cordova v6.0.0 and later on iOS, Android, and Windows devices.</span></span>  <span data-ttu-id="83595-109">hello eszközplatform-támogatás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="83595-109">hello platform support is as follows:</span></span>

* <span data-ttu-id="83595-110">Android API 19-24 (KitKat nugát keresztül).</span><span class="sxs-lookup"><span data-stu-id="83595-110">Android API 19-24 (KitKat through Nougat).</span></span>
* <span data-ttu-id="83595-111">iOS 8.0-s és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="83595-111">iOS versions 8.0 and later.</span></span>
* <span data-ttu-id="83595-112">Windows Phone 8.1-es.</span><span class="sxs-lookup"><span data-stu-id="83595-112">Windows Phone 8.1.</span></span>
* <span data-ttu-id="83595-113">Az univerzális Windows Platform.</span><span class="sxs-lookup"><span data-stu-id="83595-113">Universal Windows Platform.</span></span>

## <span data-ttu-id="83595-114"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83595-114"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="83595-115">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="83595-115">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="83595-116">Ez az útmutató feltételezi, hogy hello táblához hello hello táblák ezen oktatóprogram az azonos sémából.</span><span class="sxs-lookup"><span data-stu-id="83595-116">This guide assumes that hello table has hello same schema as hello tables in those tutorials.</span></span> <span data-ttu-id="83595-117">Ez az útmutató feltételezi hozzáadásának hello Apache Cordova beépülő modul tooyour kódot.</span><span class="sxs-lookup"><span data-stu-id="83595-117">This guide also assumes that you have added hello Apache Cordova Plugin tooyour code.</span></span>  <span data-ttu-id="83595-118">Ha nem tette, bővítheti hello Apache Cordova beépülő modul tooyour projekt hello parancssorban:</span><span class="sxs-lookup"><span data-stu-id="83595-118">If you have not done so, you may add hello Apache Cordova plugin tooyour project on hello command line:</span></span>

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="83595-119">További létrehozásával kapcsolatos információkat [az első Apache Cordova-alkalmazás], azok dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="83595-119">For more information on creating [your first Apache Cordova app], see their documentation.</span></span>

## <span data-ttu-id="83595-120"><a name="ionic"></a>Ionos v2 alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="83595-120"><a name="ionic"></a>Setting up an Ionic v2 app</span></span>

<span data-ttu-id="83595-121">tooproperly ionos v2-projekt konfigurálása, hozzon létre egy alapszintű alkalmazást, majd hello Cordova beépülő modul hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="83595-121">tooproperly configure an Ionic v2 project, first create a basic app and add hello Cordova plugin:</span></span>

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="83595-122">Adja hozzá az alábbi túl hello`app.component.ts` toocreate hello objektumot:</span><span class="sxs-lookup"><span data-stu-id="83595-122">Add hello following lines too`app.component.ts` toocreate hello client object:</span></span>

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

<span data-ttu-id="83595-123">Most már létrehozhatja és futtatását hello projekt hello böngészőben:</span><span class="sxs-lookup"><span data-stu-id="83595-123">You can now build and run hello project in hello browser:</span></span>

```
ionic platform add browser
ionic run browser
```

<span data-ttu-id="83595-124">hello Azure Mobile Apps Cordova beépülő modul mindkét ionos v1 és v2 alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="83595-124">hello Azure Mobile Apps Cordova plugin supports both Ionic v1 and v2 apps.</span></span>  <span data-ttu-id="83595-125">Csak hello ionos v2 alkalmazások a kiegészítő nyilatkozat kérése hello `WindowsAzure` objektum.</span><span class="sxs-lookup"><span data-stu-id="83595-125">Only hello Ionic v2 apps require the additional declaration for hello `WindowsAzure` object.</span></span>

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="83595-126"><a name="auth"></a>Hogyan: hitelesíti a felhasználókat</span><span class="sxs-lookup"><span data-stu-id="83595-126"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="83595-127">Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter.</span><span class="sxs-lookup"><span data-stu-id="83595-127">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="83595-128">Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="83595-128">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="83595-129">Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="83595-129">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="83595-130">További információkért lásd: hello [Bevezetés a hitelesítés használatába] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="83595-130">For more information, see hello [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="83595-131">Ha hitelesítési Apache Cordova-alkalmazást használ, a következő Cordova beépülő modulok hello elérhetőnek kell lennie:</span><span class="sxs-lookup"><span data-stu-id="83595-131">When using authentication in an Apache Cordova app, hello following Cordova plugins must be available:</span></span>

* <span data-ttu-id="83595-132">[cordova-beépülőmodul-eszköz]</span><span class="sxs-lookup"><span data-stu-id="83595-132">[cordova-plugin-device]</span></span>
* <span data-ttu-id="83595-133">[cordova-beépülőmodul-inappbrowser]</span><span class="sxs-lookup"><span data-stu-id="83595-133">[cordova-plugin-inappbrowser]</span></span>

<span data-ttu-id="83595-134">Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.</span><span class="sxs-lookup"><span data-stu-id="83595-134">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="83595-135">hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="83595-135">hello server flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="83595-136">hello ügyfél folyamata lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói eszközspecifikus SDK-k támaszkodnak az.</span><span class="sxs-lookup"><span data-stu-id="83595-136">hello client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific device-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="83595-137"><a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="83595-137"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="83595-138">Az Apache Cordova-alkalmazások számos különböző használja egy visszacsatolási funkció toohandle OAuth UI zajlik.</span><span class="sxs-lookup"><span data-stu-id="83595-138">Several types of Apache Cordova applications use a loopback capability toohandle OAuth UI flows.</span></span>  <span data-ttu-id="83595-139">A localhost OAuth UI-adatfolyamok miatt a problémák, mivel az hello hitelesítési szolgáltatás csak hogyan tooutilize a szolgáltatás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="83595-139">OAuth UI flows on localhost cause problems since hello authentication service only knows how tooutilize your service by default.</span></span>  <span data-ttu-id="83595-140">Hibás OAuth UI flow például a következők:</span><span class="sxs-lookup"><span data-stu-id="83595-140">Examples of problematic OAuth UI flows include:</span></span>

* <span data-ttu-id="83595-141">hello Fodrozás emulátor.</span><span class="sxs-lookup"><span data-stu-id="83595-141">hello Ripple emulator.</span></span>
* <span data-ttu-id="83595-142">Élő rendelkező ionos be újra.</span><span class="sxs-lookup"><span data-stu-id="83595-142">Live Reload with Ionic.</span></span>
* <span data-ttu-id="83595-143">Hello mobil-háttéralkalmazást helyileg futó</span><span class="sxs-lookup"><span data-stu-id="83595-143">Running hello mobile backend locally</span></span>
* <span data-ttu-id="83595-144">Egy másik Azure App Service, mint egy olyan hitelesítési hello hello mobil-háttéralkalmazást fut.</span><span class="sxs-lookup"><span data-stu-id="83595-144">Running hello mobile backend in a different Azure App Service than hello one providing authentication.</span></span>

<span data-ttu-id="83595-145">Kövesse ezeket az utasításokat tooadd a helyi toohello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="83595-145">Follow these instructions tooadd your local settings toohello configuration:</span></span>

1. <span data-ttu-id="83595-146">Jelentkezzen be toohello [Azure-portálon]</span><span class="sxs-lookup"><span data-stu-id="83595-146">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="83595-147">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.</span><span class="sxs-lookup"><span data-stu-id="83595-147">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="83595-148">Kattintson a **eszközök**</span><span class="sxs-lookup"><span data-stu-id="83595-148">Click **Tools**</span></span>
4. <span data-ttu-id="83595-149">Kattintson a **erőforrás-kezelő** hello OBSERVE menüben, majd kattintson a **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="83595-149">Click **Resource explorer** in hello OBSERVE menu, then click **Go**.</span></span>  <span data-ttu-id="83595-150">Megnyílik egy új ablakot vagy lapot.</span><span class="sxs-lookup"><span data-stu-id="83595-150">A new window or tab opens.</span></span>
5. <span data-ttu-id="83595-151">Bontsa ki a hello **config**, **authsettings** a hely hello bal oldali navigációs csomópontok.</span><span class="sxs-lookup"><span data-stu-id="83595-151">Expand hello **config**, **authsettings** nodes for your site in hello left-hand navigation.</span></span>
6. <span data-ttu-id="83595-152">Kattintson a **szerkesztése**</span><span class="sxs-lookup"><span data-stu-id="83595-152">Click **Edit**</span></span>
7. <span data-ttu-id="83595-153">Keresse meg hello "allowedExternalRedirectUrls" elemet.</span><span class="sxs-lookup"><span data-stu-id="83595-153">Look for hello "allowedExternalRedirectUrls" element.</span></span>  <span data-ttu-id="83595-154">Toonull vagy egy tömböt is beállítható.</span><span class="sxs-lookup"><span data-stu-id="83595-154">It may be set toonull or an array of values.</span></span>  <span data-ttu-id="83595-155">A következő érték hello érték toohello módosítása:</span><span class="sxs-lookup"><span data-stu-id="83595-155">Change hello value toohello following value:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="83595-156">Cserélje le a szolgáltatás URL-címei hello hello URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="83595-156">Replace hello URLs with hello URLs of your service.</span></span>  <span data-ttu-id="83595-157">Például "http://localhost: 3000" (a hello Node.js sample szolgáltatás), vagy "http://localhost:4400" (a hello Fodrozás szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="83595-157">Examples include "http://localhost:3000" (for hello Node.js sample service), or "http://localhost:4400" (for hello Ripple service).</span></span>  <span data-ttu-id="83595-158">Azonban ezen URL-címei, példák - hello szolgáltatások említett hello példák, beleértve a helyzetnek eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="83595-158">However, these URLs are examples - your situation, including for hello services mentioned in hello examples, may be different.</span></span>
8. <span data-ttu-id="83595-159">Kattintson a hello **olvasási/írási** hello képernyő jobb felső sarkában hello gombra.</span><span class="sxs-lookup"><span data-stu-id="83595-159">Click hello **Read/Write** button in hello top-right corner of hello screen.</span></span>
9. <span data-ttu-id="83595-160">Kattintson a zöld hello **PUT** gombra.</span><span class="sxs-lookup"><span data-stu-id="83595-160">Click hello green **PUT** button.</span></span>

<span data-ttu-id="83595-161">hello-beállítások mentése ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="83595-161">hello settings are saved at this point.</span></span>  <span data-ttu-id="83595-162">Ne zárja be az hello böngészőablakban amíg tart hello-beállítások mentése.</span><span class="sxs-lookup"><span data-stu-id="83595-162">Do not close hello browser window until hello settings have finished saving.</span></span>
<span data-ttu-id="83595-163">Ezek visszacsatolási URL-címek toohello CORS-beállítások az App Service is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="83595-163">Also add these loopback URLs toohello CORS settings for your App Service:</span></span>

1. <span data-ttu-id="83595-164">Jelentkezzen be toohello [Azure-portálon]</span><span class="sxs-lookup"><span data-stu-id="83595-164">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="83595-165">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.</span><span class="sxs-lookup"><span data-stu-id="83595-165">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="83595-166">hello-beállítások panel automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="83595-166">hello Settings blade opens automatically.</span></span>  <span data-ttu-id="83595-167">Ha nem, kattintson a gombra **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="83595-167">If it doesn't, click **All Settings**.</span></span>
4. <span data-ttu-id="83595-168">Kattintson a **CORS** hello API menüjében.</span><span class="sxs-lookup"><span data-stu-id="83595-168">Click **CORS** under hello API menu.</span></span>
5. <span data-ttu-id="83595-169">Meg kívánja hello mezőben megadott tooadd hello URL-címet, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="83595-169">Enter hello URL that you wish tooadd in hello box provided and press Enter.</span></span>
6. <span data-ttu-id="83595-170">Adja meg a további URL-címek, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="83595-170">Enter additional URLs as needed.</span></span>
7. <span data-ttu-id="83595-171">Kattintson a **mentése** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="83595-171">Click **Save** toosave hello settings.</span></span>

<span data-ttu-id="83595-172">Hello új beállítások tootake hatás körülbelül 10 – 15 másodpercre vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="83595-172">It takes approximately 10-15 seconds for hello new settings tootake effect.</span></span>

## <span data-ttu-id="83595-173"><a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="83595-173"><a name="register-for-push"></a>How to: Register for push notifications</span></span>
<span data-ttu-id="83595-174">Telepítse a hello [phonegap-beépülőmodul-leküldéses] toohandle leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="83595-174">Install hello [phonegap-plugin-push] toohandle push notifications.</span></span>  <span data-ttu-id="83595-175">A beépülő modul könnyen segítségével adhatók a `cordova plugin add` parancsot a parancssorban hello, vagy hello Git beépülő modul telepítő Visual Studio segítségével.</span><span class="sxs-lookup"><span data-stu-id="83595-175">This plugin can be easily added using the `cordova plugin add` command on hello command line, or via hello Git plugin installer within Visual Studio.</span></span>  <span data-ttu-id="83595-176">A következő kódot az Apache Cordova-alkalmazás regisztrálja az eszközt a leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="83595-176">The following code in your Apache Cordova app registers your device for push notifications:</span></span>

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
    // For cross-platform, you can use hello device plugin toodetermine hello device
    // Best is toouse device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by hello PNS - check hello format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

<span data-ttu-id="83595-177">Hello Notification Hubs SDK toosend leküldéses értesítések hello kiszolgálóról használja.</span><span class="sxs-lookup"><span data-stu-id="83595-177">Use hello Notification Hubs SDK toosend push notifications from hello server.</span></span>  <span data-ttu-id="83595-178">Soha ne küldjön leküldéses értesítéseket közvetlenül az ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="83595-178">Never send push notifications directly from clients.</span></span> <span data-ttu-id="83595-179">Ez így használt tootrigger egy szolgáltatásmegtagadási támadások elleni Notification Hubs vagy hello PNS.</span><span class="sxs-lookup"><span data-stu-id="83595-179">Doing so could be used tootrigger a denial of service attack against Notification Hubs or hello PNS.</span></span>  <span data-ttu-id="83595-180">hello PNS sikerült bA a forgalom miatt az ilyen támadásokat.</span><span class="sxs-lookup"><span data-stu-id="83595-180">hello PNS could ban your traffic as a result of such attacks.</span></span>

## <a name="more-information"></a><span data-ttu-id="83595-181">További információ</span><span class="sxs-lookup"><span data-stu-id="83595-181">More information</span></span>

<span data-ttu-id="83595-182">A részletes API részletei a [API-JÁNAK dokumentációja](http://azure.github.io/azure-mobile-apps-js-client/).</span><span class="sxs-lookup"><span data-stu-id="83595-182">You can find detailed API details in our [API documentation](http://azure.github.io/azure-mobile-apps-js-client/).</span></span>

<!-- URLs. -->
[Azure-portálon]: https://portal.azure.com
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova beépülő modul az Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[az első Apache Cordova-alkalmazás]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-beépülőmodul-leküldéses]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-beépülőmodul-eszköz]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-beépülőmodul-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
