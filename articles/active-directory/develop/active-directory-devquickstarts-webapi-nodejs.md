---
title: "Ismerkedés az Azure AD Node.js |} Microsoft Docs"
description: "Hogyan hozhat létre a többi Node.js webes API-k, amely az Azure AD használatára a hitelesítéshez."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="af1b2-103">A Node.js webes API-knak az első lépései</span><span class="sxs-lookup"><span data-stu-id="af1b2-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="af1b2-104">A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="af1b2-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="af1b2-105">Rugalmas és, moduláris Passport diszkréten eldobja bármely Express-alapú vagy Restify webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="af1b2-105">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="af1b2-106">Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több.</span><span class="sxs-lookup"><span data-stu-id="af1b2-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="af1b2-107">Kidolgoztunk is stratégiát a Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="af1b2-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="af1b2-108">Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="af1b2-108">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="af1b2-109">Ehhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="af1b2-109">To do this, you need to:</span></span>

1. <span data-ttu-id="af1b2-110">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="af1b2-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="af1b2-111">Az alkalmazás beállítása a Passport által használandó `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="af1b2-111">Set up your app to use Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="af1b2-112">A teendőlista webes API hívásához ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="af1b2-112">Configure a client application to call the To Do List web API.</span></span>

<span data-ttu-id="af1b2-113">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="af1b2-113">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="af1b2-114">A cikk nem tér ki, megvalósítható bejelentkezési, regisztrációs vagy Azure AD B2C profilok kezelése.</span><span class="sxs-lookup"><span data-stu-id="af1b2-114">This article doesn't cover how to implement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="af1b2-115">A cikk foglalkozik hívó Web API-kat a felhasználó már hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="af1b2-115">It focuses on calling web APIs after the user is already authenticated.</span></span>  <span data-ttu-id="af1b2-116">Azt javasoljuk, hogy a kiindulási pont [integrálása az Azure Active Directory-dokumentum](active-directory-how-to-integrate.md) az Azure Active Directory alapjainak megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="af1b2-116">We recommend that you start with [How to integrate with Azure Active Directory document](active-directory-how-to-integrate.md) to learn about the basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="af1b2-117">Azt is, amely a futó példa github MIT licenccel a forráskód, így nyugodtan klón (vagy még jobban elágazás), és visszajelzést és lekéréses kérelmek.</span><span class="sxs-lookup"><span data-stu-id="af1b2-117">We've released all the source code for this running example in GitHub under an MIT license, so feel free to clone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="af1b2-118">A Node.js modulok kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="af1b2-118">About Node.js modules</span></span>
<span data-ttu-id="af1b2-119">Ebben a bemutatóban a Node.js modulok használjuk.</span><span class="sxs-lookup"><span data-stu-id="af1b2-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="af1b2-120">A modulokra betölthető JavaScript-csomagok, amelyek bizonyos funkciók biztosítanak az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="af1b2-121">Általában telepítése modult az NPM telepítési könyvtárában a Node.js egy NPM parancssori eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="af1b2-121">You usually install modules by using the Node.js an NPM command-line tool in the NPM installation directory.</span></span> <span data-ttu-id="af1b2-122">Azonban néhány modulok, például a HTTP-modulja az alapvető Node.js csomagban található.</span><span class="sxs-lookup"><span data-stu-id="af1b2-122">However, some modules, such as the HTTP module, are included in the core Node.js package.</span></span>

<span data-ttu-id="af1b2-123">Telepített modulok lesznek mentve a **node_modules** könyvtárhoz, a Node.js telepítőkönyvtár gyökerében.</span><span class="sxs-lookup"><span data-stu-id="af1b2-123">Installed modules are saved in the **node_modules** directory at the root of your Node.js installation directory.</span></span> <span data-ttu-id="af1b2-124">Minden modulja a **node_modules** directory kezeli a saját **node_modules** , amelyektől függ modulokat tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-124">Each module in the **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="af1b2-125">Emellett minden egyes szükséges a modulnak a **node_modules** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="af1b2-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="af1b2-126">A rekurzív könyvtárstruktúrát a függőségi láncból jelöli.</span><span class="sxs-lookup"><span data-stu-id="af1b2-126">This recursive directory structure represents the dependency chain.</span></span>

<span data-ttu-id="af1b2-127">Ez a függőségi lánc struktúra egy nagyobb alkalmazás erőforrásigényét eredményez.</span><span class="sxs-lookup"><span data-stu-id="af1b2-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="af1b2-128">De azt is biztosítja, hogy az összes függősége teljesítésének és szolgál, hogy a modulok szolgál a fejlesztői verzióját is éles.</span><span class="sxs-lookup"><span data-stu-id="af1b2-128">But it also guarantees that all dependencies are met and that the version of the modules that's used in development is also used in production.</span></span> <span data-ttu-id="af1b2-129">Ez sokkal kiszámíthatóbbá teszi a termelési alkalmazása működését, és megakadályozza, hogy a versioning problémák, amelyek hatással lehetnek a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="af1b2-129">This makes the production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="af1b2-130">1. lépés: Az Azure AD-bérlő regisztrálása</span><span class="sxs-lookup"><span data-stu-id="af1b2-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="af1b2-131">Ez a minta használatához szüksége van az Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="af1b2-131">To use this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="af1b2-132">Ha még nem meg arról, hogy milyen a bérlő vagy az beszerzése, lásd: [az Azure AD-bérlő beszerzése](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="af1b2-132">If you're not sure what a tenant is or how to get one, see [How to get an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="af1b2-133">2. lépés: Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="af1b2-133">Step 2: Create an application</span></span>
<span data-ttu-id="af1b2-134">Ezután hoz létre egy alkalmazást a címtárában, amely biztosítja, hogy az alkalmazás biztonságos kommunikációhoz szükséges információkat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af1b2-134">Next you create an app in your directory that gives Azure AD information that it needs to securely communicate with your app.</span></span>  <span data-ttu-id="af1b2-135">Az ügyfélalkalmazást és a webes API-t egyetlen jelölik **Alkalmazásazonosító** ebben az esetben, mert alkalmazássá logikai.</span><span class="sxs-lookup"><span data-stu-id="af1b2-135">Both the client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="af1b2-136">Az alkalmazást a következő [utasítások](active-directory-how-applications-are-added.md) alapján hozza létre.</span><span class="sxs-lookup"><span data-stu-id="af1b2-136">To create an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="af1b2-137">A sor üzleti alkalmazás készítésekor [hasznosak lehetnek a további utasítások](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="af1b2-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="af1b2-138">Alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="af1b2-138">To create an application:</span></span>

1. <span data-ttu-id="af1b2-139">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af1b2-139">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="af1b2-140">A felső menüben válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="af1b2-140">On the top menu, select your account.</span></span> <span data-ttu-id="af1b2-141">Ekkor a a **Directory** menüben válassza ki, hol szeretné az alkalmazás regisztrálása az Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="af1b2-141">Then, under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="af1b2-142">A bal oldali menüben válasszon ki **több szolgáltatások**, majd válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-142">In the menu on the left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="af1b2-143">Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="af1b2-144">Kövesse a megjelenő utasításokat hozzon létre egy **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-144">Follow the prompts to create a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="af1b2-145">A **neve** , az alkalmazás írja le az alkalmazást a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="af1b2-145">The **name** of the application describes your application to end users.</span></span>

      * <span data-ttu-id="af1b2-146">A **bejelentkezési URL-cím** az alkalmazás alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="af1b2-146">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="af1b2-147">Az alapértelmezett URL-címe mintakódot `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="af1b2-147">The default URL of the sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="af1b2-148">Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="af1b2-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="af1b2-149">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="af1b2-149">You need this value in the next sections, so copy it from the application page.</span></span>

7. <span data-ttu-id="af1b2-150">Az a **beállítások** -> **tulajdonságok** az alkalmazás lapján frissítse a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="af1b2-150">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="af1b2-151">A **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="af1b2-151">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="af1b2-152">Az egyezmény használandó `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="af1b2-152">The convention is to use `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="af1b2-153">Hozzon létre egy **kulcs** az alkalmazás a **beállítások** lapon, majd másolja ki valahová.</span><span class="sxs-lookup"><span data-stu-id="af1b2-153">Create a **Key** for your application from the **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="af1b2-154">Szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="af1b2-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="af1b2-155">3. lépés: A Node.js letöltése a platformra</span><span class="sxs-lookup"><span data-stu-id="af1b2-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="af1b2-156">A minta használatához rendelkeznie kell a Node.js telepített és működő verziójával.</span><span class="sxs-lookup"><span data-stu-id="af1b2-156">To successfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="af1b2-157">Telepítse a Node.js-t [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="af1b2-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="af1b2-158">4. lépés: Telepítse MongoDB a platformon</span><span class="sxs-lookup"><span data-stu-id="af1b2-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="af1b2-159">Ez a minta használatához a MongoDB telepített és működő kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="af1b2-159">To successfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="af1b2-160">A mongodb a REST API különböző kiszolgálópéldányok közötti állandó ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="af1b2-160">You use MongoDB to make the REST API persistent across server instances.</span></span>

<span data-ttu-id="af1b2-161">Telepítse a mongodb-t a [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="af1b2-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="af1b2-162">Ez a forgatókönyv azt feltételezi, hogy kell használni az alapértelmezett telepítését és kiszolgálóvégpontjait a MongoDB, amely írásának időpontjában mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="af1b2-162">This walkthrough assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="af1b2-163">5. lépés: A Restify-modulok telepítése a webes API</span><span class="sxs-lookup"><span data-stu-id="af1b2-163">Step 5: Install the Restify modules in your web API</span></span>
<span data-ttu-id="af1b2-164">Restify hozhat létre REST API-n használjuk.</span><span class="sxs-lookup"><span data-stu-id="af1b2-164">We are using Restify to build our REST API.</span></span> <span data-ttu-id="af1b2-165">A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van.</span><span class="sxs-lookup"><span data-stu-id="af1b2-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="af1b2-166">Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="af1b2-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="af1b2-167">A Restify telepítése</span><span class="sxs-lookup"><span data-stu-id="af1b2-167">Install Restify</span></span>
1. <span data-ttu-id="af1b2-168">A parancssorban lépjen a **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="af1b2-168">From the command line, change directories to the **azuread** directory.</span></span> <span data-ttu-id="af1b2-169">Ha a **azuread** könyvtár nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="af1b2-169">If the **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="af1b2-170">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="af1b2-170">Type the following command:</span></span>

    `npm install restify`

    <span data-ttu-id="af1b2-171">Ez a parancs elvégzi a Restify telepítését.</span><span class="sxs-lookup"><span data-stu-id="af1b2-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="af1b2-172">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="af1b2-172">Did you get an error?</span></span>
<span data-ttu-id="af1b2-173">Egyes operációs rendszerek NPM használata esetén előfordulhat, hogy hibaüzenet, amely szerint **hiba: EPERM, chmod "/ usr/helyi/bin /..."**</span><span class="sxs-lookup"><span data-stu-id="af1b2-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="af1b2-174">és rendszergazdaként futtatja a figyelembe próbálja javaslatot.</span><span class="sxs-lookup"><span data-stu-id="af1b2-174">and a suggestion that you try running the account as an administrator.</span></span> <span data-ttu-id="af1b2-175">Ha ez történik, a sudo utasítás használatával NPM futtassa magasabb jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="af1b2-175">If this occurs, use the sudo command to run NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="af1b2-176">Vonatkozó DTRACE hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="af1b2-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="af1b2-177">Ilyen hiba láthatja a Restify telepítése során:</span><span class="sxs-lookup"><span data-stu-id="af1b2-177">You might see an error like this when installing Restify:</span></span>

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
<span data-ttu-id="af1b2-178">A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="af1b2-179">Számos operációs rendszer azonban nem rendelkezik DTrace.</span><span class="sxs-lookup"><span data-stu-id="af1b2-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="af1b2-180">Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="af1b2-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="af1b2-181">Ez a parancs kimenete a következő kimeneti hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af1b2-181">The output of this command should look similar to the following output:</span></span>

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="af1b2-182">6. lépés: Telepítse Passport.js a webes API</span><span class="sxs-lookup"><span data-stu-id="af1b2-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="af1b2-183">A [Passport](http://passportjs.org/) a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="af1b2-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="af1b2-184">Rugalmas és, moduláris Passport diszkréten eldobja bármely Express-alapú vagy Restify webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="af1b2-184">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="af1b2-185">Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több.</span><span class="sxs-lookup"><span data-stu-id="af1b2-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="af1b2-186">Az Azure Active Directory kidolgoztunk is stratégiát.</span><span class="sxs-lookup"><span data-stu-id="af1b2-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="af1b2-187">Azt telepítenie kell a modult, és adja hozzá az Azure Active Directory stratégiai bővítményt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-187">We install this module and then add the Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="af1b2-188">A parancssorban lépjen a **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="af1b2-188">From the command line, change directories to the **azuread** directory.</span></span>

2. <span data-ttu-id="af1b2-189">Passport.js telepítéséhez adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="af1b2-189">To install passport.js, enter the following command :</span></span>

    `npm install passport`

    <span data-ttu-id="af1b2-190">A parancs a következőhöz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af1b2-190">The output of the command should look similar to the following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="af1b2-191">7. lépés: A Passport-Azure-AD hozzá a web API</span><span class="sxs-lookup"><span data-stu-id="af1b2-191">Step 7: Add Passport-Azure-AD to your web API</span></span>
<span data-ttu-id="af1b2-192">Ezután azt hozzá az OAuth stratégiát `passport-azure-ad`, a stratégiacsomag szolgál az Azure Active Directory Passport-hez.</span><span class="sxs-lookup"><span data-stu-id="af1b2-192">Next we add the OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory to Passport.</span></span> <span data-ttu-id="af1b2-193">Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.</span><span class="sxs-lookup"><span data-stu-id="af1b2-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="af1b2-194">Bár az OAuth2 keretrendszerében, amelyben bármely ismert jogkivonat típus kibocsáthatja, csak bizonyos tokentípusokat gyakran használják.</span><span class="sxs-lookup"><span data-stu-id="af1b2-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="af1b2-195">Tulajdonosi jogkivonatok a leggyakrabban használt jogkivonatokat a végpontok védelmére.</span><span class="sxs-lookup"><span data-stu-id="af1b2-195">Bearer tokens are the most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="af1b2-196">A leggyakrabban kibocsátott típusú lexikális elem szerepel az OAuth2.</span><span class="sxs-lookup"><span data-stu-id="af1b2-196">They are the most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="af1b2-197">Számos implementáció eleve feltételezik, hogy csak az ilyen típusú kiadott jogkivonatokat, tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="af1b2-197">Many implementations assume that bearer tokens are the only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="af1b2-198">A parancssorban lépjen a **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="af1b2-198">From the command line, change directories to the **azuread** directory.</span></span>

<span data-ttu-id="af1b2-199">Telepítéséhez írja be a következő parancsot a Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="af1b2-199">Type the following command to install the Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="af1b2-200">A parancs a következő kimeneti hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af1b2-200">The output of the command should look similar to the following output:</span></span>


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="af1b2-201">8. lépés: A MongoDB-modulok hozzáadása a webes API-hoz</span><span class="sxs-lookup"><span data-stu-id="af1b2-201">Step 8: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="af1b2-202">Az adattároló azt a mongodb.</span><span class="sxs-lookup"><span data-stu-id="af1b2-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="af1b2-203">Ezért azt telepítenie kell a széles körben használt beépülő modul hívott Mongoose modellek és sémák kezelésére.</span><span class="sxs-lookup"><span data-stu-id="af1b2-203">For that reason, we need to install the widely used plug-in called Mongoose to manage models and schemas.</span></span> <span data-ttu-id="af1b2-204">Azt is telepítenie kell az adatbázis-illesztőprogramját mongodb-protokolltámogatással (amelyet a MongoDB is neveznek).</span><span class="sxs-lookup"><span data-stu-id="af1b2-204">We also need to install the database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="af1b2-205">9. lépés: További modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="af1b2-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="af1b2-206">Ezután a további szükséges modulokat telepítésére.</span><span class="sxs-lookup"><span data-stu-id="af1b2-206">Next we install the remaining required modules.</span></span>

1. <span data-ttu-id="af1b2-207">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-207">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-208">Adja meg a következő parancsok futtatásával telepítse ezeket a modulokat a **node_modules** könyvtár:</span><span class="sxs-lookup"><span data-stu-id="af1b2-208">Enter the following commands to install these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="af1b2-209">10. lépés: Hozzon létre egy server.js a függőségek</span><span class="sxs-lookup"><span data-stu-id="af1b2-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="af1b2-210">A server.js fájl legtöbb funkciója biztosítja a web API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-210">The server.js file provides most of the functionality for our web API server.</span></span> <span data-ttu-id="af1b2-211">Jelenleg felvenni található kód jelentős részét ehhez a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-211">We add most of our code to this file.</span></span> <span data-ttu-id="af1b2-212">Élesben való használat azt javasoljuk, hogy refactor-e a kisebb fájlok, például különálló útvonalak és vezérlők funkciókat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-212">For production purposes, we recommend that you refactor the functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="af1b2-213">Ebben a bemutatóban a server.js használjuk funkcióhoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="af1b2-214">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-214">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-215">Hozzon létre egy `server.js` a kedvenc szerkesztő fájlt, és adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="af1b2-215">Create a `server.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. <span data-ttu-id="af1b2-216">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-216">Save the file.</span></span> <span data-ttu-id="af1b2-217">Még visszatérünk rá hamarosan.</span><span class="sxs-lookup"><span data-stu-id="af1b2-217">We'll return to it shortly.</span></span>

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="af1b2-218">11. lépés: Az Azure AD-beállítások tárolására konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="af1b2-218">Step 11: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="af1b2-219">Ez a kódfájl adja át a konfigurációs paraméterek az Azure Active Directory portálon Passport.js.</span><span class="sxs-lookup"><span data-stu-id="af1b2-219">This code file passes the configuration parameters from your Azure Active Directory portal to Passport.js.</span></span> <span data-ttu-id="af1b2-220">A webes API-t a portálhoz, a forgatókönyv első része az hozzáadásakor létrehozta ezeket a konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="af1b2-220">You created these configuration values when you added the web API to the portal in the first part of the walkthrough.</span></span> <span data-ttu-id="af1b2-221">A paraméterek kitöltésének módját a kód másolását követően ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="af1b2-221">We explain what to put in the values of these parameters after you copy the code.</span></span>

1. <span data-ttu-id="af1b2-222">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-222">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-223">Hozzon létre egy `config.js` a kedvenc szerkesztő fájlt, és adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="af1b2-223">Create a `config.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. <span data-ttu-id="af1b2-224">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-224">Save the file.</span></span>

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a><span data-ttu-id="af1b2-225">12. lépés: A konfigurációs értékek hozzáadása a server.js fájlhoz</span><span class="sxs-lookup"><span data-stu-id="af1b2-225">Step 12: Add configuration values to your server.js file</span></span>
<span data-ttu-id="af1b2-226">Igazolnia kell olvasni ezeket az értékeket az alkalmazás között létrehozott .config kiterjesztésű fájl.</span><span class="sxs-lookup"><span data-stu-id="af1b2-226">We need to read these values from the .config file that you created across our application.</span></span> <span data-ttu-id="af1b2-227">Ehhez az szükséges, jelenleg felvenni a .config fájl kötelező erőforrásként az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="af1b2-227">To do this, we add the .config file as a required resource in our application.</span></span> <span data-ttu-id="af1b2-228">Majd hivatott meg kell egyeznie a változók a config.js dokumentumban a globális változókat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-228">Then we set the global variables to match the variables in the config.js document.</span></span>

1. <span data-ttu-id="af1b2-229">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-229">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-230">Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="af1b2-230">Open your `server.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="af1b2-231">Adja meg az új szakasz `server.js` az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="af1b2-231">Then add a new section to `server.js` with the following code:</span></span>

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. <span data-ttu-id="af1b2-232">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-232">Save the file.</span></span>

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="af1b2-233">13. lépés: A MongoDB-modellre és -sémára információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="af1b2-233">Step 13: Add The MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="af1b2-234">Ez a felkészítés érintetlen most fizető, mivel ezek a fájlok egyesítése azt REST API-szolgáltatás elindításához.</span><span class="sxs-lookup"><span data-stu-id="af1b2-234">Now all this preparation is going to start paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="af1b2-235">Jelen kalauz használatához a MongoDB a feladatok tárolására, lásd a 4. lépés használjuk.</span><span class="sxs-lookup"><span data-stu-id="af1b2-235">For this walkthrough, we use MongoDB to store our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="af1b2-236">Az a `config.js` fájlt, hogy a 11. lépésben létrehozott, az adatbázis hívtuk `tasklist`, mert, hogy mi azt put végén a **mogoose_auth_local** kapcsolat URL-címet.</span><span class="sxs-lookup"><span data-stu-id="af1b2-236">In the `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at the end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="af1b2-237">Ezt az adatbázist nem szükséges előre létrehozni a MongoDB-ben.</span><span class="sxs-lookup"><span data-stu-id="af1b2-237">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="af1b2-238">Ehelyett MongoDB hoz létre ez az USA első alkalommal történő futtatásakor a kiszolgálói alkalmazás (feltéve, hogy az adatbázis nem létezik).</span><span class="sxs-lookup"><span data-stu-id="af1b2-238">Instead, MongoDB creates this for us on the first run of our server application (assuming that the database doesn't already exist).</span></span>

<span data-ttu-id="af1b2-239">Most, hogy megadta, hogy rendelkezik a kiszolgáló melyik MongoDB-adatbázist kell használni, azt kell írnia a tartozó modell és séma a kiszolgálói feladatok létrehozásához további kódot.</span><span class="sxs-lookup"><span data-stu-id="af1b2-239">Now that we've told the server which MongoDB database we'd like to use, we need to write some additional code to create the model and schema for our server's tasks.</span></span>

### <a name="discussion-of-the-model"></a><span data-ttu-id="af1b2-240">Az ismertető a modell</span><span class="sxs-lookup"><span data-stu-id="af1b2-240">Discussion of the model</span></span>
<span data-ttu-id="af1b2-241">A sémamodell felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="af1b2-241">Our schema model is simple.</span></span> <span data-ttu-id="af1b2-242">Szükség szerint bontsa ki.</span><span class="sxs-lookup"><span data-stu-id="af1b2-242">You expand it as required.</span></span>

<span data-ttu-id="af1b2-243">: A név a feladathoz rendelt személy.</span><span class="sxs-lookup"><span data-stu-id="af1b2-243">NAME: The name of the person who is assigned to the task.</span></span> <span data-ttu-id="af1b2-244">A **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-244">A **String**.</span></span>

<span data-ttu-id="af1b2-245">FELADAT: A feladat magát.</span><span class="sxs-lookup"><span data-stu-id="af1b2-245">TASK: The task itself.</span></span> <span data-ttu-id="af1b2-246">A **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-246">A **String**.</span></span>

<span data-ttu-id="af1b2-247">DÁTUM: Az a dátum, a feladat miatt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-247">DATE: The date that the task is due.</span></span> <span data-ttu-id="af1b2-248">A **DATETIME**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-248">A **DATETIME**.</span></span>

<span data-ttu-id="af1b2-249">BEFEJEZŐDÖTT: Ha a feladat befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="af1b2-249">COMPLETED: If the task has been completed or not.</span></span> <span data-ttu-id="af1b2-250">A **LOGIKAI**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-250">A **BOOLEAN**.</span></span>

### <a name="creating-the-schema-in-the-code"></a><span data-ttu-id="af1b2-251">A kódban a séma létrehozása</span><span class="sxs-lookup"><span data-stu-id="af1b2-251">Creating the schema in the code</span></span>
1. <span data-ttu-id="af1b2-252">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-252">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-253">Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja meg a következő adatokat a konfigurációs bejegyzés alatt:</span><span class="sxs-lookup"><span data-stu-id="af1b2-253">Open your `server.js` file in your favorite editor, and then add the following information below the configuration entry:</span></span>

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
<span data-ttu-id="af1b2-254">Megadható, hogy a kód, mivel azt először hozza létre a séma.</span><span class="sxs-lookup"><span data-stu-id="af1b2-254">As you can tell from the code, we create our schema first.</span></span> <span data-ttu-id="af1b2-255">Ezután a modellobjektumot, amelyet az adatok a kód egészében tárolja, amikor meghatároztuk használatával hoz létre, azt a **útvonalak**.</span><span class="sxs-lookup"><span data-stu-id="af1b2-255">Then we create a model object that we use to store our data throughout the code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="af1b2-256">14. lépés: A feladat REST API-kiszolgálóhoz az mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af1b2-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="af1b2-257">Most, hogy egy adatbázismodell együttműködni, adjuk hozzá a REST API-kiszolgálóhoz használni fogjuk útvonalak.</span><span class="sxs-lookup"><span data-stu-id="af1b2-257">Now that we have a database model to work with, let's add the routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="af1b2-258">Az útvonalak működése a Restify programban</span><span class="sxs-lookup"><span data-stu-id="af1b2-258">About routes in Restify</span></span>
<span data-ttu-id="af1b2-259">Útvonalak az Express verem az azonos módon működnek működése a Restify programban.</span><span class="sxs-lookup"><span data-stu-id="af1b2-259">Routes work in Restify the same way they do in the Express stack.</span></span> <span data-ttu-id="af1b2-260">Az útvonalakat azon URI használatával kell meghatározni, amelyet elképzelései szerint az ügyfélalkalmazások meg fognak hívni.</span><span class="sxs-lookup"><span data-stu-id="af1b2-260">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="af1b2-261">Az útvonalakat általában külön fájlban definiálni.</span><span class="sxs-lookup"><span data-stu-id="af1b2-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="af1b2-262">A célokra azt helyezze el az útvonalakat a server.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-262">For our purposes, we put our routes in the server.js file.</span></span> <span data-ttu-id="af1b2-263">Azt javasoljuk, hogy a saját üzemi használatra fájlba figyelembe ezeket az útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="af1b2-264">A Restify-útvonal egy tipikus mintája a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="af1b2-264">A typical pattern for a Restify route is as follows:</span></span>

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


<span data-ttu-id="af1b2-265">Ez a minta legalapvetőbb megjelenése.</span><span class="sxs-lookup"><span data-stu-id="af1b2-265">This is the pattern at its most basic level.</span></span> <span data-ttu-id="af1b2-266">A restify (és az Express) sokkal összetettebb funkciók, például velük az alkalmazástípusokat és a különböző végpontok között összetett útválasztási adja meg.</span><span class="sxs-lookup"><span data-stu-id="af1b2-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="af1b2-267">A célból hogy megakadályozzák ezeket az útvonalakat egyszerű.</span><span class="sxs-lookup"><span data-stu-id="af1b2-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-to-our-server"></a><span data-ttu-id="af1b2-268">A kiszolgáló alapértelmezett útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af1b2-268">Add default routes to our server</span></span>
<span data-ttu-id="af1b2-269">A Microsoft most hozzáadása az alapszintű CRUD-útvonalakat létrehozása, beolvasása, frissítése és törlése.</span><span class="sxs-lookup"><span data-stu-id="af1b2-269">We now add the basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="af1b2-270">A parancssorban lépjen a **azuread** mappát, ha már nem létezik:</span><span class="sxs-lookup"><span data-stu-id="af1b2-270">From the command line, change directories to the **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="af1b2-271">Nyissa meg a `server.js` a kedvenc szerkesztőben fájlt, és adja meg a következő információkat végzett korábbi adatbázis-bejegyzések alá:</span><span class="sxs-lookup"><span data-stu-id="af1b2-271">Open the `server.js` file in your favorite editor, and then add the following information below the previous database entries that you made:</span></span>

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="af1b2-272">Az API-Jainkkal a hibakezelés</span><span class="sxs-lookup"><span data-stu-id="af1b2-272">Add error handling in our APIs</span></span>
```

///--- Errors for communicating something interesting back to the client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a><span data-ttu-id="af1b2-273">15. lépés: A kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="af1b2-273">Step 15: Create your server</span></span>
<span data-ttu-id="af1b2-274">Az adatbázis definiáltuk, és az útvonalak legyenek érvényben.</span><span class="sxs-lookup"><span data-stu-id="af1b2-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="af1b2-275">Az utolsó lépés, adja hozzá a hívásokat kezelő kiszolgálópéldányt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-275">The last thing to do is add the server instance that manages our calls.</span></span>

<span data-ttu-id="af1b2-276">A Restify (és az Express) teheti a nagy mennyiségű testreszabási egy REST API-kiszolgálóhoz, de ismét fogjuk használni a legalapvetőbb beállítása a célokra.</span><span class="sxs-lookup"><span data-stu-id="af1b2-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going to use the most basic setup for our purposes.</span></span>

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a><span data-ttu-id="af1b2-277">16. lépés: Az útvonalak hozzáadása a kiszolgálóhoz (hitelesítés nélkül a lépést)</span><span class="sxs-lookup"><span data-stu-id="af1b2-277">Step 16: Add the routes to the server (without authentication for now)</span></span>
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a><span data-ttu-id="af1b2-278">17. lépés: A kiszolgáló futtatása (előtt OAuth-támogatás hozzáadása)</span><span class="sxs-lookup"><span data-stu-id="af1b2-278">Step 17: Run the server (before adding OAuth support)</span></span>
<span data-ttu-id="af1b2-279">Ahhoz, hogy fel hitelesítési tesztelje a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="af1b2-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="af1b2-280">A legegyszerűbben úgy lehet ellenőrizni a kiszolgáló van, a parancssorban curl használatával.</span><span class="sxs-lookup"><span data-stu-id="af1b2-280">The easiest way to test your server is by using curl in a command line.</span></span> <span data-ttu-id="af1b2-281">Nézzük meg, igazolnia kell egy segédprogram, amely lehetővé teszi, hogy a kimeneti adatok JSON-ként értelmezni.</span><span class="sxs-lookup"><span data-stu-id="af1b2-281">Before we do that, we need a utility that allows us to parse output as JSON.</span></span>

1. <span data-ttu-id="af1b2-282">Telepítse a következő JSON-segédprogramot, (az alábbi példák ezzel az eszközzel):</span><span class="sxs-lookup"><span data-stu-id="af1b2-282">Install the following JSON tool (all the following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="af1b2-283">Ezzel globálisan telepíti a JSON-segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="af1b2-283">This installs the JSON tool globally.</span></span> <span data-ttu-id="af1b2-284">Most, hogy azt már valósítható meg, amely, most lejátszása a kiszolgálóval:</span><span class="sxs-lookup"><span data-stu-id="af1b2-284">Now that we’ve accomplished that, let’s play with the server:</span></span>

2. <span data-ttu-id="af1b2-285">Először is győződjön meg arról, hogy fut-e a mongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="af1b2-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="af1b2-286">Ezután nyissa meg azt a könyvtárat, és indítsa el a sütővasak:</span><span class="sxs-lookup"><span data-stu-id="af1b2-286">Then, change to the directory and start curling:</span></span>

    <span data-ttu-id="af1b2-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="af1b2-287">`$ cd azuread` `$ node server.js`</span></span>

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. <span data-ttu-id="af1b2-288">Ezt követően hozzá lehessen adni a feladat ily módon:</span><span class="sxs-lookup"><span data-stu-id="af1b2-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="af1b2-289">A következő választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="af1b2-289">The response should be:</span></span>

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    <span data-ttu-id="af1b2-290">És megjelenő feladatok Brandon az ilyen módon:</span><span class="sxs-lookup"><span data-stu-id="af1b2-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="af1b2-291">Ez akkor működik, ha még hozzáadhatja az OAuth a REST API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-291">If all this works, we're ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="af1b2-292">MongoDB-vel futó REST API-kiszolgáló rendelkezik!</span><span class="sxs-lookup"><span data-stu-id="af1b2-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-to-our-rest-api-server"></a><span data-ttu-id="af1b2-293">18. lépés: Hitelesítés hozzáadása a REST API-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="af1b2-293">Step 18: Add authentication to our REST API server</span></span>
<span data-ttu-id="af1b2-294">Most, hogy a futó REST API-t, először az Azure ad-val hasznos információkkal.</span><span class="sxs-lookup"><span data-stu-id="af1b2-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="af1b2-295">A parancssorban lépjen a **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-295">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="af1b2-296">A passport-azure-ad részét képező OIDCBearerStrategy használata</span><span class="sxs-lookup"><span data-stu-id="af1b2-296">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="af1b2-297">Mi építettünk, amennyiben egy tipikus REST TODO kiszolgálót hitelesítés nélkül.</span><span class="sxs-lookup"><span data-stu-id="af1b2-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="af1b2-298">Ez azért, ha először üzembe, amelyek.</span><span class="sxs-lookup"><span data-stu-id="af1b2-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="af1b2-299">Először azt kell jelezheti, hogy kívánja-e használni a Passport.</span><span class="sxs-lookup"><span data-stu-id="af1b2-299">First, we need to indicate that we want to use Passport.</span></span> <span data-ttu-id="af1b2-300">Ez a jogosultság helyezze a másik kiszolgáló konfigurációja után:</span><span class="sxs-lookup"><span data-stu-id="af1b2-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="af1b2-301">API-k írásakor azt javasoljuk, hogy Ön mindig kapcsolja az adatokat egy egyedi, amely a felhasználói nem tudnak jogosulatlanul hozzáférni a jogkivonatból.</span><span class="sxs-lookup"><span data-stu-id="af1b2-301">When you write APIs, we recommend that you always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="af1b2-302">Ha a kiszolgáló tárolja a TODO elemeket tárolja őket (a token.OID), a jogkivonatot, amely a "tulajdonos" mező be azt a felhasználó objektum azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="af1b2-302">When this server stores TODO items, it stores them based on the object ID of the user in the token (called through token.oid), which we put in the “owner” field.</span></span> <span data-ttu-id="af1b2-303">Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e a TODOs.</span><span class="sxs-lookup"><span data-stu-id="af1b2-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="af1b2-304">Nincs nem jelenik meg sehol a "tulajdonos" API-ban, a külső felhasználó kérhet mások TODOs, még akkor is, ha hitelesített.</span><span class="sxs-lookup"><span data-stu-id="af1b2-304">There is no exposure in the API of “owner,” so an external user can request the TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="af1b2-305">Következő most használja az rendelkező részét képező tulajdonosi stratégiát `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="af1b2-305">Next let’s use the bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="af1b2-306">Most tekintse meg a kódot, és a többi azt ismertetjük, hamarosan.</span><span class="sxs-lookup"><span data-stu-id="af1b2-306">Look at the code for now and we'll explain the rest shortly.</span></span> <span data-ttu-id="af1b2-307">Be az alábbiakat a fentiekben beillesztett:</span><span class="sxs-lookup"><span data-stu-id="af1b2-307">Put this after what you pasted above:</span></span>

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

<span data-ttu-id="af1b2-308">A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="af1b2-309">Megnézzük a stratégiát, láthatja azt adja át, amely rendelkezik a jogkivonatot, és kész paraméterek függvényt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-309">Looking at the strategy, you see we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="af1b2-310">A stratégia ismét elérhető lesz a számunkra után a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="af1b2-310">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="af1b2-311">Miután Igen, azt tárolni a felhasználót, és menteni a jogkivonatot, így azt nem kell megismételni.</span><span class="sxs-lookup"><span data-stu-id="af1b2-311">After it does, we store the user and stash the token so we won’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af1b2-312">Az előző kód készít minden olyan felhasználó, hogy a kiszolgáló hitelesítésére történik.</span><span class="sxs-lookup"><span data-stu-id="af1b2-312">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="af1b2-313">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="af1b2-313">This is known as auto-registration.</span></span> <span data-ttu-id="af1b2-314">Az üzemi kiszolgálók, azt javasoljuk, ne engedélyezze, hogy bárki, aki nélkül őket keresse meg, amelyeket a regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="af1b2-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="af1b2-315">Ez általában az a megoldást látjuk a fogyasztói alkalmazásoknál, melyek lehetővé teszik regisztrálni a Facebook, de majd kérje meg, hogy további adatokat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-315">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="af1b2-316">Ha a cserét nem parancssori program, kinyerhettük volna az e-mailt jogkivonat-objektumból, amely adott vissza, és felkérhettük volna a felhasználót, hogy további adatokat.</span><span class="sxs-lookup"><span data-stu-id="af1b2-316">If this wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="af1b2-317">Mivel a kiszolgáló csak ellenőrzési célokat szolgál, most egyszerűen hozzáadjuk őket a memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="af1b2-317">Because this is a test server, we simply add them to the in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="af1b2-318">Egyes végpontok védelme</span><span class="sxs-lookup"><span data-stu-id="af1b2-318">Protect some endpoints</span></span>
<span data-ttu-id="af1b2-319">Védelmet biztosít a végpontoknak megadásával a `passport.authenticate()` hívja meg a használni kívánt protokollt.</span><span class="sxs-lookup"><span data-stu-id="af1b2-319">You protect endpoints by specifying the `passport.authenticate()` call with the protocol that you want to use.</span></span>

<span data-ttu-id="af1b2-320">Szeretné, hogy a kiszolgálóoldali kódban valami ennél is érdekesebb megoldást, a most Szerkesztés az útvonal.</span><span class="sxs-lookup"><span data-stu-id="af1b2-320">To make our server code do something more interesting, let’s edit the route.</span></span>

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="af1b2-321">19. lépés: Futtassa újra a kiszolgálóalkalmazás, és győződjön meg arról, hogy elutasítja</span><span class="sxs-lookup"><span data-stu-id="af1b2-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="af1b2-322">Most használja `curl` ismételt használatával ellenőrizheti, ha már tudunk OAuth2-védelem a végpontok ellen.</span><span class="sxs-lookup"><span data-stu-id="af1b2-322">Let's use `curl` again to see if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="af1b2-323">Ez a vizsgálat előtt futtató az ügyfél-SDK-kat a végponton végezzük.</span><span class="sxs-lookup"><span data-stu-id="af1b2-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="af1b2-324">A visszakapott fejlécek alapján kell lennie ahhoz, hogy adja meg, ha az oktatóanyagban módosítjuk jó úton jár le.</span><span class="sxs-lookup"><span data-stu-id="af1b2-324">The headers that are returned should be enough to tell us if we're going down the right path.</span></span>

1. <span data-ttu-id="af1b2-325">Először is győződjön meg arról, hogy fut-e a mongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="af1b2-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="af1b2-326">Ezután nyissa meg azt a könyvtárat, és indítsa el a sütővasak.</span><span class="sxs-lookup"><span data-stu-id="af1b2-326">Then, change to the directory and start curling.</span></span>

      <span data-ttu-id="af1b2-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="af1b2-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="af1b2-328">Próbálja meg egy egyszerű POST MŰVELETTEL.</span><span class="sxs-lookup"><span data-stu-id="af1b2-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="af1b2-329">A 401-es értéke a keresett Itt választ.</span><span class="sxs-lookup"><span data-stu-id="af1b2-329">A 401 is the response you are looking for here.</span></span> <span data-ttu-id="af1b2-330">Ez a válasz azt jelzi, hogy a Passport réteg megpróbálja átirányítani a engedélyezett végpontot, amely pontosan mit szeretne.</span><span class="sxs-lookup"><span data-stu-id="af1b2-330">This response indicates that the Passport layer is trying to redirect to the authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af1b2-331">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af1b2-331">Next steps</span></span>
<span data-ttu-id="af1b2-332">Ön már megtettünk mindent, ami az OAuth2-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="af1b2-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="af1b2-333">Szüksége lesz egy további forgatókönyv keresztül halad.</span><span class="sxs-lookup"><span data-stu-id="af1b2-333">You will need to go through an additional walkthrough.</span></span>

<span data-ttu-id="af1b2-334">Most már tudja, hogyan egy REST API Restify és OAuth2 segítségével megvalósítható.</span><span class="sxs-lookup"><span data-stu-id="af1b2-334">You've now learned how to implement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="af1b2-335">Lehetősége van több mint elég tartani a szolgáltatás fejlesztés, és hogyan hozhat létre ebben a példában a kódot is.</span><span class="sxs-lookup"><span data-stu-id="af1b2-335">You also have more than enough code to keep developing your service and learning how to build on this example.</span></span>

<span data-ttu-id="af1b2-336">Ha érdekli a következő lépéseket a a ADAL használatában, az alábbiakban néhány azt javasoljuk, hogy tovább dolgozik a támogatott ADAL ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="af1b2-336">If you are interested in the next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="af1b2-337">Klónozza a fejlesztői gépen le, és konfigurálja a bemutatóban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="af1b2-337">Clone down to your developer machine and configure as described in the walkthrough.</span></span>

[<span data-ttu-id="af1b2-338">Az IOS-es ADAL</span><span class="sxs-lookup"><span data-stu-id="af1b2-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="af1b2-339">ADAL androidhoz</span><span class="sxs-lookup"><span data-stu-id="af1b2-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
