---
title: "Első lépések AD Node.js aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Node.js REST webes API, amely integrálható az Azure AD használatára a hitelesítéshez."
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
ms.openlocfilehash: 512ae6de9acfde8b58c0447ab4a6b573fb6407c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="e2167-103">A Node.js webes API-knak az első lépései</span><span class="sxs-lookup"><span data-stu-id="e2167-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="e2167-104">A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="e2167-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="e2167-105">Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy Restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2167-105">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="e2167-106">Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több.</span><span class="sxs-lookup"><span data-stu-id="e2167-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="e2167-107">Kidolgoztunk is stratégiát a Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e2167-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e2167-108">Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="e2167-108">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="e2167-109">toodo, kell:</span><span class="sxs-lookup"><span data-stu-id="e2167-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="e2167-110">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e2167-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="e2167-111">Állítsa be az alkalmazás toouse Passport által `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="e2167-111">Set up your app toouse Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="e2167-112">Ügyfél alkalmazás toocall hello tooDo lista webes API-k konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e2167-112">Configure a client application toocall hello tooDo List web API.</span></span>

<span data-ttu-id="e2167-113">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="e2167-113">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="e2167-114">Ez a cikk nem fedi le, hogyan tooimplement bejelentkezés, -előfizetés, vagy az Azure AD B2C felügyeleti profilt.</span><span class="sxs-lookup"><span data-stu-id="e2167-114">This article doesn't cover how tooimplement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="e2167-115">A cikk foglalkozik a hívó webes API-k hello felhasználó már hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="e2167-115">It focuses on calling web APIs after hello user is already authenticated.</span></span>  <span data-ttu-id="e2167-116">Azt javasoljuk, hogy a kiindulási pont [hogyan Azure Active Directory dokumentummal toointegrate](active-directory-how-to-integrate.md) toolearn hello alapokról az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e2167-116">We recommend that you start with [How toointegrate with Azure Active Directory document](active-directory-how-to-integrate.md) toolearn about hello basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="e2167-117">A futó példa github MIT licenccel, az összes hello forráskódja már megjelent úgy érzi, hogy szabad tooclone (vagy még jobban elágazás), és visszajelzést, és lekéréses kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="e2167-117">We've released all hello source code for this running example in GitHub under an MIT license, so feel free tooclone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="e2167-118">A Node.js modulok kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="e2167-118">About Node.js modules</span></span>
<span data-ttu-id="e2167-119">Ebben a bemutatóban a Node.js modulok használjuk.</span><span class="sxs-lookup"><span data-stu-id="e2167-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="e2167-120">A modulokra betölthető JavaScript-csomagok, amelyek bizonyos funkciók biztosítanak az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="e2167-121">Általában modulok használatával telepítse az NPM parancssori eszköz Node.js hello hello NPM telepítési könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="e2167-121">You usually install modules by using hello Node.js an NPM command-line tool in hello NPM installation directory.</span></span> <span data-ttu-id="e2167-122">Azonban néhány modulok, például HTTP-modulja hello hello core Node.js csomagban található.</span><span class="sxs-lookup"><span data-stu-id="e2167-122">However, some modules, such as hello HTTP module, are included in hello core Node.js package.</span></span>

<span data-ttu-id="e2167-123">Telepített modulok menti a hello **node_modules** könyvtárhoz, a Node.js telepítési könyvtár hello gyökerében.</span><span class="sxs-lookup"><span data-stu-id="e2167-123">Installed modules are saved in hello **node_modules** directory at hello root of your Node.js installation directory.</span></span> <span data-ttu-id="e2167-124">Minden hello modulja **node_modules** directory kezeli a saját **node_modules** , amelyektől függ modulokat tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="e2167-124">Each module in hello **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="e2167-125">Emellett minden egyes szükséges a modulnak a **node_modules** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e2167-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="e2167-126">A rekurzív könyvtárstruktúrát hello függőségi lánc jelöli.</span><span class="sxs-lookup"><span data-stu-id="e2167-126">This recursive directory structure represents hello dependency chain.</span></span>

<span data-ttu-id="e2167-127">Ez a függőségi lánc struktúra egy nagyobb alkalmazás erőforrásigényét eredményez.</span><span class="sxs-lookup"><span data-stu-id="e2167-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="e2167-128">De azt is garantálja, hogy minden függőség teljesülnek, és adott hello verziójának hello modulok fejlesztési használt termelési is használja.</span><span class="sxs-lookup"><span data-stu-id="e2167-128">But it also guarantees that all dependencies are met and that hello version of hello modules that's used in development is also used in production.</span></span> <span data-ttu-id="e2167-129">Ez sokkal kiszámíthatóbbá teszi hello éles alkalmazása működését, és megakadályozza, hogy a versioning problémák, amelyek hatással lehetnek a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e2167-129">This makes hello production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="e2167-130">1. lépés: Az Azure AD-bérlő regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e2167-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="e2167-131">toouse ez mintát, az Azure Active Directory-bérlő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e2167-131">toouse this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="e2167-132">Ha nem tudja, milyen a bérlő vagy hogyan tooget, lásd [hogyan tooget az Azure AD bérlői](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e2167-132">If you're not sure what a tenant is or how tooget one, see [How tooget an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="e2167-133">2. lépés: Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2167-133">Step 2: Create an application</span></span>
<span data-ttu-id="e2167-134">Ezután alkalmazást hoz létre a címtárban, hogy az Azure AD által biztosított információkat, hogy kell-e toosecurely kommunikálni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2167-134">Next you create an app in your directory that gives Azure AD information that it needs toosecurely communicate with your app.</span></span>  <span data-ttu-id="e2167-135">Hello ügyfélalkalmazást és a webes API jelölik egyetlen **Alkalmazásazonosító** ebben az esetben, mert alkalmazássá logikai.</span><span class="sxs-lookup"><span data-stu-id="e2167-135">Both hello client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="e2167-136">hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-how-applications-are-added.md).</span><span class="sxs-lookup"><span data-stu-id="e2167-136">toocreate an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="e2167-137">A sor üzleti alkalmazás készítésekor [hasznosak lehetnek a további utasítások](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="e2167-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="e2167-138">egy alkalmazás toocreate:</span><span class="sxs-lookup"><span data-stu-id="e2167-138">toocreate an application:</span></span>

1. <span data-ttu-id="e2167-139">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2167-139">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="e2167-140">A hello felső menüben válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="e2167-140">On hello top menu, select your account.</span></span> <span data-ttu-id="e2167-141">Ekkor a hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2167-141">Then, under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="e2167-142">Hello hello bal oldali menüben válasszon ki **több szolgáltatások**, majd válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e2167-142">In hello menu on hello left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="e2167-143">Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e2167-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="e2167-144">Kövesse az utasításokat toocreate hello egy **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="e2167-144">Follow hello prompts toocreate a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="e2167-145">Hello **neve** hello az alkalmazás az alkalmazás tooend felhasználók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e2167-145">hello **name** of hello application describes your application tooend users.</span></span>

      * <span data-ttu-id="e2167-146">Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2167-146">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="e2167-147">az alapértelmezett URL-címe hello mintakód hello `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e2167-147">hello default URL of hello sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="e2167-148">Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e2167-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="e2167-149">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="e2167-149">You need this value in hello next sections, so copy it from hello application page.</span></span>

7. <span data-ttu-id="e2167-150">A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése.</span><span class="sxs-lookup"><span data-stu-id="e2167-150">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="e2167-151">Hello **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e2167-151">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="e2167-152">hello konvenció: toouse `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="e2167-152">hello convention is toouse `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="e2167-153">Hozzon létre egy **kulcs** hello az alkalmazás **beállítások** lapon, majd másolja ki valahová.</span><span class="sxs-lookup"><span data-stu-id="e2167-153">Create a **Key** for your application from hello **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="e2167-154">Szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="e2167-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="e2167-155">3. lépés: A Node.js letöltése a platformra</span><span class="sxs-lookup"><span data-stu-id="e2167-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="e2167-156">toosuccessfully Ez a minta használatához rendelkeznie kell a Node.js telepített és működő.</span><span class="sxs-lookup"><span data-stu-id="e2167-156">toosuccessfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="e2167-157">Telepítse a Node.js-t [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="e2167-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="e2167-158">4. lépés: Telepítse MongoDB a platformon</span><span class="sxs-lookup"><span data-stu-id="e2167-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="e2167-159">toosuccessfully Ez a minta használatához rendelkeznie kell a MongoDB telepített és működő.</span><span class="sxs-lookup"><span data-stu-id="e2167-159">toosuccessfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="e2167-160">MongoDB toomake hello állandó REST API különböző kiszolgálópéldányok közötti használhatja.</span><span class="sxs-lookup"><span data-stu-id="e2167-160">You use MongoDB toomake hello REST API persistent across server instances.</span></span>

<span data-ttu-id="e2167-161">Telepítse a mongodb-t a [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="e2167-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="e2167-162">Ez a forgatókönyv azt feltételezi, hogy a használt hello alapértelmezett telepítését és kiszolgálóvégpontjait mongodb, amelyek írásának időpontjában hello mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="e2167-162">This walkthrough assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="e2167-163">5. lépés: Hello Restify-modulok telepítése a webes API</span><span class="sxs-lookup"><span data-stu-id="e2167-163">Step 5: Install hello Restify modules in your web API</span></span>
<span data-ttu-id="e2167-164">Használunk Restify toobuild REST API-n.</span><span class="sxs-lookup"><span data-stu-id="e2167-164">We are using Restify toobuild our REST API.</span></span> <span data-ttu-id="e2167-165">A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van.</span><span class="sxs-lookup"><span data-stu-id="e2167-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="e2167-166">Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e2167-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="e2167-167">A Restify telepítése</span><span class="sxs-lookup"><span data-stu-id="e2167-167">Install Restify</span></span>
1. <span data-ttu-id="e2167-168">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e2167-168">From hello command line, change directories toohello **azuread** directory.</span></span> <span data-ttu-id="e2167-169">Ha hello **azuread** könyvtár nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="e2167-169">If hello **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="e2167-170">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-170">Type hello following command:</span></span>

    `npm install restify`

    <span data-ttu-id="e2167-171">Ez a parancs elvégzi a Restify telepítését.</span><span class="sxs-lookup"><span data-stu-id="e2167-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="e2167-172">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="e2167-172">Did you get an error?</span></span>
<span data-ttu-id="e2167-173">Egyes operációs rendszerek NPM használata esetén előfordulhat, hogy hibaüzenet, amely szerint **hiba: EPERM, chmod "/ usr/helyi/bin /..."**</span><span class="sxs-lookup"><span data-stu-id="e2167-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="e2167-174">és, hogy rendszergazdaként futó hello fiók megpróbál javaslatot.</span><span class="sxs-lookup"><span data-stu-id="e2167-174">and a suggestion that you try running hello account as an administrator.</span></span> <span data-ttu-id="e2167-175">Ha ez történik, a hello sudo parancs toorun NPM magasabb jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="e2167-175">If this occurs, use hello sudo command toorun NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="e2167-176">Vonatkozó DTRACE hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="e2167-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="e2167-177">Ilyen hiba láthatja a Restify telepítése során:</span><span class="sxs-lookup"><span data-stu-id="e2167-177">You might see an error like this when installing Restify:</span></span>

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
<span data-ttu-id="e2167-178">A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat.</span><span class="sxs-lookup"><span data-stu-id="e2167-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="e2167-179">Számos operációs rendszer azonban nem rendelkezik DTrace.</span><span class="sxs-lookup"><span data-stu-id="e2167-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="e2167-180">Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="e2167-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="e2167-181">a parancs kimenetének hello alábbihoz hasonló toohello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="e2167-181">hello output of this command should look similar toohello following output:</span></span>

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


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="e2167-182">6. lépés: Telepítse Passport.js a webes API</span><span class="sxs-lookup"><span data-stu-id="e2167-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="e2167-183">A [Passport](http://passportjs.org/) a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="e2167-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="e2167-184">Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy Restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2167-184">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="e2167-185">Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több.</span><span class="sxs-lookup"><span data-stu-id="e2167-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="e2167-186">Az Azure Active Directory kidolgoztunk is stratégiát.</span><span class="sxs-lookup"><span data-stu-id="e2167-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="e2167-187">Azt telepítenie kell a modult, és adja hozzá a hello Azure Active Directory stratégiai bővítményt.</span><span class="sxs-lookup"><span data-stu-id="e2167-187">We install this module and then add hello Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="e2167-188">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e2167-188">From hello command line, change directories toohello **azuread** directory.</span></span>

2. <span data-ttu-id="e2167-189">tooinstall passport.js, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-189">tooinstall passport.js, enter hello following command :</span></span>

    `npm install passport`

    <span data-ttu-id="e2167-190">hello parancs kimenetében hello alábbihoz hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="e2167-190">hello output of hello command should look similar toohello following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="e2167-191">7. lépés:, Adja hozzá a Passport-Azure-AD tooyour webes API</span><span class="sxs-lookup"><span data-stu-id="e2167-191">Step 7: Add Passport-Azure-AD tooyour web API</span></span>
<span data-ttu-id="e2167-192">Ezután azt hozzá hello OAuth stratégiát `passport-azure-ad`, a stratégiacsomag szolgál, amelyek kapcsolódnak az Azure Active Directory tooPassport.</span><span class="sxs-lookup"><span data-stu-id="e2167-192">Next we add hello OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory tooPassport.</span></span> <span data-ttu-id="e2167-193">Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.</span><span class="sxs-lookup"><span data-stu-id="e2167-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="e2167-194">Bár az OAuth2 keretrendszerében, amelyben bármely ismert jogkivonat típus kibocsáthatja, csak bizonyos tokentípusokat gyakran használják.</span><span class="sxs-lookup"><span data-stu-id="e2167-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="e2167-195">Tulajdonosi jogkivonatok olyan végpontok védelmére szolgáló leggyakrabban használt hello jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="e2167-195">Bearer tokens are hello most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="e2167-196">Az OAuth2 token legszélesebb körben kiadott hello típusú.</span><span class="sxs-lookup"><span data-stu-id="e2167-196">They are hello most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="e2167-197">Számos implementáció eleve feltételezik, hogy hello csak ilyen típusú kiállított jogkivonatokat tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="e2167-197">Many implementations assume that bearer tokens are hello only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="e2167-198">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e2167-198">From hello command line, change directories toohello **azuread** directory.</span></span>

<span data-ttu-id="e2167-199">Típus hello következő parancsot a tooinstall hello Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="e2167-199">Type hello following command tooinstall hello Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="e2167-200">hello hello parancs kimenete a következő kimeneti hasonló toohello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="e2167-200">hello output of hello command should look similar toohello following output:</span></span>


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



## <a name="step-8-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="e2167-201">8. lépés: A MongoDB modulok tooyour webes API hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2167-201">Step 8: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="e2167-202">Az adattároló azt a mongodb.</span><span class="sxs-lookup"><span data-stu-id="e2167-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="e2167-203">Éppen ezért ellenőriznünk kell tooinstall hello széles körben használt beépülő modul hívott Mongoose toomanage modellek és sémák.</span><span class="sxs-lookup"><span data-stu-id="e2167-203">For that reason, we need tooinstall hello widely used plug-in called Mongoose toomanage models and schemas.</span></span> <span data-ttu-id="e2167-204">Mongodb-protokolltámogatással (amelyet a MongoDB is neveznek) kell tooinstall hello adatbázis-illesztőprogramot is.</span><span class="sxs-lookup"><span data-stu-id="e2167-204">We also need tooinstall hello database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="e2167-205">9. lépés: További modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="e2167-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="e2167-206">Ezután a fennmaradó szükséges modulokat hello telepítjük.</span><span class="sxs-lookup"><span data-stu-id="e2167-206">Next we install hello remaining required modules.</span></span>

1. <span data-ttu-id="e2167-207">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-207">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-208">Adja meg a következő parancsok tooinstall hello ezeket a modulokat a **node_modules** könyvtár:</span><span class="sxs-lookup"><span data-stu-id="e2167-208">Enter hello following commands tooinstall these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="e2167-209">10. lépés: Hozzon létre egy server.js a függőségek</span><span class="sxs-lookup"><span data-stu-id="e2167-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="e2167-210">hello server.js fájl hello funkcióinak többségét biztosít a webes API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-210">hello server.js file provides most of hello functionality for our web API server.</span></span> <span data-ttu-id="e2167-211">Azt adja hozzá a kódfájl toothis része.</span><span class="sxs-lookup"><span data-stu-id="e2167-211">We add most of our code toothis file.</span></span> <span data-ttu-id="e2167-212">Élesben való használat esetén javasoljuk, hogy Ön azonosítóterületen kisebb fájlok, például különálló útvonalak és vezérlők hello funkciókat.</span><span class="sxs-lookup"><span data-stu-id="e2167-212">For production purposes, we recommend that you refactor hello functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="e2167-213">Ebben a bemutatóban a server.js használjuk funkcióhoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="e2167-214">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-214">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-215">Hozzon létre egy `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-215">Create a `server.js` file in your favorite editor, and then add hello following information:</span></span>

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

3. <span data-ttu-id="e2167-216">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2167-216">Save hello file.</span></span> <span data-ttu-id="e2167-217">Tooit hamarosan még visszatérünk.</span><span class="sxs-lookup"><span data-stu-id="e2167-217">We'll return tooit shortly.</span></span>

## <a name="step-11-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="e2167-218">11. lépés: Hozzon létre egy konfigurációs fájl toostore az Azure AD-beállítások</span><span class="sxs-lookup"><span data-stu-id="e2167-218">Step 11: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="e2167-219">Ez a kódfájl hello konfigurációs paraméterek az Azure Active Directory portálon tooPassport.js a adja át.</span><span class="sxs-lookup"><span data-stu-id="e2167-219">This code file passes hello configuration parameters from your Azure Active Directory portal tooPassport.js.</span></span> <span data-ttu-id="e2167-220">Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello forgatókönyv első része hello való hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="e2167-220">You created these configuration values when you added hello web API toohello portal in hello first part of hello walkthrough.</span></span> <span data-ttu-id="e2167-221">Milyen tooput hello értékeket a következő paraméterek közül azt ismertetik, hello kód másolását követően.</span><span class="sxs-lookup"><span data-stu-id="e2167-221">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

1. <span data-ttu-id="e2167-222">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-222">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-223">Hozzon létre egy `config.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-223">Create a `config.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in tooyour server unless you use hello common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in tooyour server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes toostderr in Unix.

         };
    ```
3. <span data-ttu-id="e2167-224">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2167-224">Save hello file.</span></span>

## <a name="step-12-add-configuration-values-tooyour-serverjs-file"></a><span data-ttu-id="e2167-225">12. lépés: A konfigurációs értékek tooyour server.js fájl felvétele</span><span class="sxs-lookup"><span data-stu-id="e2167-225">Step 12: Add configuration values tooyour server.js file</span></span>
<span data-ttu-id="e2167-226">Igazolnia kell tooread ezeket az értékeket a hello .config fájlból létrehozott alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="e2167-226">We need tooread these values from hello .config file that you created across our application.</span></span> <span data-ttu-id="e2167-227">toodo, jelenleg felvenni hello .config kiterjesztésű fájlt kötelező erőforrásként az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2167-227">toodo this, we add hello .config file as a required resource in our application.</span></span> <span data-ttu-id="e2167-228">Majd hello globális változók toomatch hello változók hivatott hello config.js dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e2167-228">Then we set hello global variables toomatch hello variables in hello config.js document.</span></span>

1. <span data-ttu-id="e2167-229">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-229">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-230">Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-230">Open your `server.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="e2167-231">Majd adja hozzá az új szakasz túl`server.js` a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="e2167-231">Then add a new section too`server.js` with hello following code:</span></span>

    ```Javascript
    var options = {
        // hello URL of hello metadata document for your app. We will put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array toohold logged in users and hello current logged in user (owner).
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

      // If hello logging level is specified, switch tooit.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. <span data-ttu-id="e2167-232">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2167-232">Save hello file.</span></span>

## <a name="step-13-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="e2167-233">13. lépés: Hello MongoDB modell és séma információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="e2167-233">Step 13: Add hello MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="e2167-234">Ez a felkészítés most toostart fizető, mivel ezek a fájlok egy REST API-szolgáltatásba kombinációja lesz.</span><span class="sxs-lookup"><span data-stu-id="e2167-234">Now all this preparation is going toostart paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="e2167-235">Ennél a bemutatónál használjuk MongoDB toostore a feladatok, lásd a 4. lépés.</span><span class="sxs-lookup"><span data-stu-id="e2167-235">For this walkthrough, we use MongoDB toostore our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="e2167-236">A hello `config.js` fájlt, hogy a 11. lépésben létrehozott, az adatbázis hívtuk `tasklist`, mert, hogy mi azt put hello végén a **mogoose_auth_local** kapcsolat URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e2167-236">In hello `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at hello end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="e2167-237">Ez az adatbázis előre a MongoDB-ben nem kell toocreate.</span><span class="sxs-lookup"><span data-stu-id="e2167-237">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="e2167-238">Ehelyett MongoDB hoz létre ez az USA hello először futtassa a kiszolgálói alkalmazás (feltéve, hogy hello az adatbázis nem létezik).</span><span class="sxs-lookup"><span data-stu-id="e2167-238">Instead, MongoDB creates this for us on hello first run of our server application (assuming that hello database doesn't already exist).</span></span>

<span data-ttu-id="e2167-239">Most, hogy megadta, hogy rendelkezik hello server melyik MongoDB-adatbázist toouse tapasztalatairól, igazolnia kell toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-239">Now that we've told hello server which MongoDB database we'd like toouse, we need toowrite some additional code toocreate hello model and schema for our server's tasks.</span></span>

### <a name="discussion-of-hello-model"></a><span data-ttu-id="e2167-240">Az ismertető hello modell</span><span class="sxs-lookup"><span data-stu-id="e2167-240">Discussion of hello model</span></span>
<span data-ttu-id="e2167-241">A sémamodell felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="e2167-241">Our schema model is simple.</span></span> <span data-ttu-id="e2167-242">Szükség szerint bontsa ki.</span><span class="sxs-lookup"><span data-stu-id="e2167-242">You expand it as required.</span></span>

<span data-ttu-id="e2167-243">: Hello név hello személy, aki hozzá van rendelve toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="e2167-243">NAME: hello name of hello person who is assigned toohello task.</span></span> <span data-ttu-id="e2167-244">A **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="e2167-244">A **String**.</span></span>

<span data-ttu-id="e2167-245">FELADAT: hello feladat magát.</span><span class="sxs-lookup"><span data-stu-id="e2167-245">TASK: hello task itself.</span></span> <span data-ttu-id="e2167-246">A **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="e2167-246">A **String**.</span></span>

<span data-ttu-id="e2167-247">: Hello dátum hello tevékenység egy lejárt.</span><span class="sxs-lookup"><span data-stu-id="e2167-247">DATE: hello date that hello task is due.</span></span> <span data-ttu-id="e2167-248">A **DATETIME**.</span><span class="sxs-lookup"><span data-stu-id="e2167-248">A **DATETIME**.</span></span>

<span data-ttu-id="e2167-249">BEFEJEZŐDÖTT: Ha hello feladat befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="e2167-249">COMPLETED: If hello task has been completed or not.</span></span> <span data-ttu-id="e2167-250">A **LOGIKAI**.</span><span class="sxs-lookup"><span data-stu-id="e2167-250">A **BOOLEAN**.</span></span>

### <a name="creating-hello-schema-in-hello-code"></a><span data-ttu-id="e2167-251">Hello kódban hello séma létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2167-251">Creating hello schema in hello code</span></span>
1. <span data-ttu-id="e2167-252">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-252">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-253">Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello konfigurációs bejegyzés alatt hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-253">Open your `server.js` file in your favorite editor, and then add hello following information below hello configuration entry:</span></span>

    ```Javascript
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema toostore our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use hello schema tooregister a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
<span data-ttu-id="e2167-254">Hello kódból állapítható meg, mivel azt először hozza létre a séma.</span><span class="sxs-lookup"><span data-stu-id="e2167-254">As you can tell from hello code, we create our schema first.</span></span> <span data-ttu-id="e2167-255">Majd a modellobjektumot, amelyet az adatok teljes hello code, amikor meghatároztuk toostore használjuk létrehozhatunk a **útvonalak**.</span><span class="sxs-lookup"><span data-stu-id="e2167-255">Then we create a model object that we use toostore our data throughout hello code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="e2167-256">14. lépés: A feladat REST API-kiszolgálóhoz az mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2167-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="e2167-257">Most, hogy egy adatbázis-modell toowork rendelkező, adjuk hozzá hello útvonalak folyamatos használata dolgozunk a REST API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-257">Now that we have a database model toowork with, let's add hello routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="e2167-258">Az útvonalak működése a Restify programban</span><span class="sxs-lookup"><span data-stu-id="e2167-258">About routes in Restify</span></span>
<span data-ttu-id="e2167-259">Útvonalak használhatók Restify hello azonos módon azokat a hello Express verem.</span><span class="sxs-lookup"><span data-stu-id="e2167-259">Routes work in Restify hello same way they do in hello Express stack.</span></span> <span data-ttu-id="e2167-260">Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével.</span><span class="sxs-lookup"><span data-stu-id="e2167-260">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="e2167-261">Az útvonalakat általában külön fájlban definiálni.</span><span class="sxs-lookup"><span data-stu-id="e2167-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="e2167-262">A célokra azt helyezze el az útvonalakat hello server.js fájl.</span><span class="sxs-lookup"><span data-stu-id="e2167-262">For our purposes, we put our routes in hello server.js file.</span></span> <span data-ttu-id="e2167-263">Azt javasoljuk, hogy a saját üzemi használatra fájlba figyelembe ezeket az útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="e2167-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="e2167-264">A Restify-útvonal egy tipikus mintája a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="e2167-264">A typical pattern for a Restify route is as follows:</span></span>

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep hello server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


<span data-ttu-id="e2167-265">Ez az a legalapvetőbb szinten hello mintát.</span><span class="sxs-lookup"><span data-stu-id="e2167-265">This is hello pattern at its most basic level.</span></span> <span data-ttu-id="e2167-266">A restify (és az Express) sokkal összetettebb funkciók, például velük az alkalmazástípusokat és a különböző végpontok között összetett útválasztási adja meg.</span><span class="sxs-lookup"><span data-stu-id="e2167-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="e2167-267">A célból hogy megakadályozzák ezeket az útvonalakat egyszerű.</span><span class="sxs-lookup"><span data-stu-id="e2167-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-tooour-server"></a><span data-ttu-id="e2167-268">Alapértelmezett útvonalak tooour kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2167-268">Add default routes tooour server</span></span>
<span data-ttu-id="e2167-269">A Microsoft most hozzáadása hello alapvető CRUD-útvonalakat: létrehozása, beolvasása, frissítése és törlése.</span><span class="sxs-lookup"><span data-stu-id="e2167-269">We now add hello basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="e2167-270">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik:</span><span class="sxs-lookup"><span data-stu-id="e2167-270">From hello command line, change directories toohello **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="e2167-271">Nyissa meg hello `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ alább hello előző adatbázis bejegyzések hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-271">Open hello `server.js` file in your favorite editor, and then add hello following information below hello previous database entries that you made:</span></span>

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it tooMongodb.
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
            req.log.warn(err, 'createTask: unable toosave');
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
                'removeTask: unable toodelete %s',
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
            req.log.warn(err, 'get: unable tooread %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

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
            log.warn(err, "There is no tasks in hello database. Did you initialize hello database as stated in hello README?");
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

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="e2167-272">Az API-Jainkkal a hibakezelés</span><span class="sxs-lookup"><span data-stu-id="e2167-272">Add error handling in our APIs</span></span>
```

///--- Errors for communicating something interesting back toohello client.

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


## <a name="step-15-create-your-server"></a><span data-ttu-id="e2167-273">15. lépés: A kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2167-273">Step 15: Create your server</span></span>
<span data-ttu-id="e2167-274">Az adatbázis definiáltuk, és az útvonalak legyenek érvényben.</span><span class="sxs-lookup"><span data-stu-id="e2167-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="e2167-275">hello utolsó lépésként toodo van adja hozzá a hívásokat kezelő hello server-példányt.</span><span class="sxs-lookup"><span data-stu-id="e2167-275">hello last thing toodo is add hello server instance that manages our calls.</span></span>

<span data-ttu-id="e2167-276">A Restify (és az Express) teheti a nagy mennyiségű testreszabási egy REST API-kiszolgálóhoz, de újra fogjuk toouse hello most az alapszintű beállításokat a célokra.</span><span class="sxs-lookup"><span data-stu-id="e2167-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going toouse hello most basic setup for our purposes.</span></span>

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

// Allow five requests per second by IP, and burst too10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping tooREST.
```

## <a name="step-16-add-hello-routes-toohello-server-without-authentication-for-now"></a><span data-ttu-id="e2167-277">16. lépés: Hello útvonalak toohello kiszolgáló (hitelesítés nélkül most) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2167-277">Step 16: Add hello routes toohello server (without authentication for now)</span></span>
```Javascript
/// Now hello real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In hello pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need toomaintain session state. You can experiment with removing API protection
/* by removing hello passport.authenticate() method as follows:
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
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-hello-server-before-adding-oauth-support"></a><span data-ttu-id="e2167-278">17. lépés: Futtatása hello server (OAuth-támogatás hozzáadása)</span><span class="sxs-lookup"><span data-stu-id="e2167-278">Step 17: Run hello server (before adding OAuth support)</span></span>
<span data-ttu-id="e2167-279">Ahhoz, hogy fel hitelesítési tesztelje a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="e2167-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="e2167-280">hello legegyszerűbb módja tootest a kiszolgáló van, a parancssorban curl használatával.</span><span class="sxs-lookup"><span data-stu-id="e2167-280">hello easiest way tootest your server is by using curl in a command line.</span></span> <span data-ttu-id="e2167-281">Nézzük meg, igazolnia kell egy segédprogram, amely lehetővé teszi tooparse kimeneti adatok JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="e2167-281">Before we do that, we need a utility that allows us tooparse output as JSON.</span></span>

1. <span data-ttu-id="e2167-282">Telepítse a következő (a következő példák az összes hello ezzel az eszközzel) JSON-segédprogramot hello:</span><span class="sxs-lookup"><span data-stu-id="e2167-282">Install hello following JSON tool (all hello following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="e2167-283">Ezzel globálisan telepíti a hello JSON-segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="e2167-283">This installs hello JSON tool globally.</span></span> <span data-ttu-id="e2167-284">Most, hogy azt már valósítható meg, amely, most lejátszása hello kiszolgálóval:</span><span class="sxs-lookup"><span data-stu-id="e2167-284">Now that we’ve accomplished that, let’s play with hello server:</span></span>

2. <span data-ttu-id="e2167-285">Először is győződjön meg arról, hogy fut-e a mongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="e2167-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="e2167-286">Ezután módosítsa toohello könyvtárat, és sütővasak start:</span><span class="sxs-lookup"><span data-stu-id="e2167-286">Then, change toohello directory and start curling:</span></span>

    <span data-ttu-id="e2167-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="e2167-287">`$ cd azuread` `$ node server.js`</span></span>

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

4. <span data-ttu-id="e2167-288">Ezt követően hozzá lehessen adni a feladat ily módon:</span><span class="sxs-lookup"><span data-stu-id="e2167-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="e2167-289">hello választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="e2167-289">hello response should be:</span></span>

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
    <span data-ttu-id="e2167-290">És megjelenő feladatok Brandon az ilyen módon:</span><span class="sxs-lookup"><span data-stu-id="e2167-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="e2167-291">Ha minden minden megfelelően működik, a rendszer készen áll a tooadd OAuth toohello REST API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-291">If all this works, we're ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="e2167-292">MongoDB-vel futó REST API-kiszolgáló rendelkezik!</span><span class="sxs-lookup"><span data-stu-id="e2167-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-tooour-rest-api-server"></a><span data-ttu-id="e2167-293">18. lépés: A hitelesítés tooour REST API-kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2167-293">Step 18: Add authentication tooour REST API server</span></span>
<span data-ttu-id="e2167-294">Most, hogy a futó REST API-t, először az Azure ad-val hasznos információkkal.</span><span class="sxs-lookup"><span data-stu-id="e2167-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="e2167-295">Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e2167-295">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="e2167-296">Hello részét passport-azure-ad részét képező OIDCBearerStrategy használata</span><span class="sxs-lookup"><span data-stu-id="e2167-296">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="e2167-297">Mi építettünk, amennyiben egy tipikus REST TODO kiszolgálót hitelesítés nélkül.</span><span class="sxs-lookup"><span data-stu-id="e2167-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="e2167-298">Ez azért, ha először üzembe, amelyek.</span><span class="sxs-lookup"><span data-stu-id="e2167-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="e2167-299">Először igazolnia kell, hogy szeretnénk toouse Passport tooindicate.</span><span class="sxs-lookup"><span data-stu-id="e2167-299">First, we need tooindicate that we want toouse Passport.</span></span> <span data-ttu-id="e2167-300">Ez a jogosultság helyezze a másik kiszolgáló konfigurációja után:</span><span class="sxs-lookup"><span data-stu-id="e2167-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="e2167-301">Amikor ír API-k, javasoljuk, hogy mindig kapcsolja hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="e2167-301">When you write APIs, we recommend that you always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="e2167-302">Ha a kiszolgáló tárolja a TODO elemeket tárolja őket hello Objektumazonosító hello felhasználó hello jogkivonatot (a token.OID), amelyeket a Microsoft hello "tulajdonos" mező alapján.</span><span class="sxs-lookup"><span data-stu-id="e2167-302">When this server stores TODO items, it stores them based on hello object ID of hello user in hello token (called through token.oid), which we put in hello “owner” field.</span></span> <span data-ttu-id="e2167-303">Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e a TODOs.</span><span class="sxs-lookup"><span data-stu-id="e2167-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="e2167-304">Nincs nem jelenik meg sehol hello API "tulajdonos", a külső felhasználó kérhet hello TODOs mások, még akkor is, ha hitelesített.</span><span class="sxs-lookup"><span data-stu-id="e2167-304">There is no exposure in hello API of “owner,” so an external user can request hello TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="e2167-305">Következő most használja az hello tulajdonosi stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="e2167-305">Next let’s use hello bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="e2167-306">Tekintse meg most hello kódot, és hamarosan elmagyarázzuk hello rest.</span><span class="sxs-lookup"><span data-stu-id="e2167-306">Look at hello code for now and we'll explain hello rest shortly.</span></span> <span data-ttu-id="e2167-307">Be az alábbiakat a fentiekben beillesztett:</span><span class="sxs-lookup"><span data-stu-id="e2167-307">Put this after what you pasted above:</span></span>

```Javascript
    /**
    /*
    /* Calling hello OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides hello need toomanage users and info tokens
    /* with a FindorCreate() method that must be provided by hello implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want toodo something smarter.
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
            log.info('verifying hello user');
            log.info(token, 'was hello token retreived');
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

<span data-ttu-id="e2167-308">A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="e2167-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="e2167-309">Hello stratégia megnézi, láthatja azt adja át azt egy függvényt, amely rendelkezik egy jogkivonat és egy kész hello paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="e2167-309">Looking at hello strategy, you see we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="e2167-310">hello stratégia előre vissza toous, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="e2167-310">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="e2167-311">Igen, miután tároljuk hello felhasználói és stash hello jogkivonat, nem szükséges tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="e2167-311">After it does, we store hello user and stash hello token so we won’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2167-312">hello előző kód minden olyan felhasználó, akkor fordul elő, tooauthenticate tooour kiszolgáló vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e2167-312">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="e2167-313">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="e2167-313">This is known as auto-registration.</span></span> <span data-ttu-id="e2167-314">Az üzemi kiszolgálók, azt javasoljuk, ne engedélyezze, hogy bárki, aki nélkül őket keresse meg, amelyeket a regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="e2167-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="e2167-315">Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásokba, lehetővé teszi a Facebook tooregister, de majd megkérdezi, hogy toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="e2167-315">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="e2167-316">Ha a cserét nem parancssori program, kinyerhettük volna hello e-mailt a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna hello felhasználói toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="e2167-316">If this wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="e2167-317">Mivel ez egy tesztkiszolgálón, egyszerűen hozzáadjuk a őket toohello memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e2167-317">Because this is a test server, we simply add them toohello in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="e2167-318">Egyes végpontok védelme</span><span class="sxs-lookup"><span data-stu-id="e2167-318">Protect some endpoints</span></span>
<span data-ttu-id="e2167-319">Védelmet biztosít a végpontoknak hello megadásával `passport.authenticate()` hívás hello protokoll, amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="e2167-319">You protect endpoints by specifying hello `passport.authenticate()` call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="e2167-320">a kiszolgálóoldali kódban tegye valami további érdekes toomake most Szerkesztés hello útvonal.</span><span class="sxs-lookup"><span data-stu-id="e2167-320">toomake our server code do something more interesting, let’s edit hello route.</span></span>

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

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="e2167-321">19. lépés: Futtassa újra a kiszolgálóalkalmazás, és győződjön meg arról, hogy elutasítja</span><span class="sxs-lookup"><span data-stu-id="e2167-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="e2167-322">Most használja `curl` újra toosee szemben a végpontok most megfelelő OAuth2-védelem.</span><span class="sxs-lookup"><span data-stu-id="e2167-322">Let's use `curl` again toosee if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="e2167-323">Ez a vizsgálat előtt futtató az ügyfél-SDK-kat a végponton végezzük.</span><span class="sxs-lookup"><span data-stu-id="e2167-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="e2167-324">hello visszakapott fejlécek alapján legyen elég tootell, ha az oktatóanyagban módosítjuk hello jó úton jár le.</span><span class="sxs-lookup"><span data-stu-id="e2167-324">hello headers that are returned should be enough tootell us if we're going down hello right path.</span></span>

1. <span data-ttu-id="e2167-325">Először is győződjön meg arról, hogy fut-e a mongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="e2167-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="e2167-326">Ezután módosítsa toohello könyvtárat, és sütővasak start.</span><span class="sxs-lookup"><span data-stu-id="e2167-326">Then, change toohello directory and start curling.</span></span>

      <span data-ttu-id="e2167-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="e2167-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="e2167-328">Próbálja meg egy egyszerű POST MŰVELETTEL.</span><span class="sxs-lookup"><span data-stu-id="e2167-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="e2167-329">A 401-es hello választ keres itt.</span><span class="sxs-lookup"><span data-stu-id="e2167-329">A 401 is hello response you are looking for here.</span></span> <span data-ttu-id="e2167-330">Ez a válasz azt jelzi, hogy a hello Passport réteg próbál tooredirect toohello engedélyezett végpontot, amely pontosan mit szeretne.</span><span class="sxs-lookup"><span data-stu-id="e2167-330">This response indicates that hello Passport layer is trying tooredirect toohello authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2167-331">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2167-331">Next steps</span></span>
<span data-ttu-id="e2167-332">Ön már megtettünk mindent, ami az OAuth2-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e2167-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="e2167-333">Szüksége lesz egy további forgatókönyv keresztül toogo.</span><span class="sxs-lookup"><span data-stu-id="e2167-333">You will need toogo through an additional walkthrough.</span></span>

<span data-ttu-id="e2167-334">Most már tudja, hogyan tooimplement segítségével egy REST API Restify és OAuth2.</span><span class="sxs-lookup"><span data-stu-id="e2167-334">You've now learned how tooimplement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="e2167-335">Azt is, hogy a szolgáltatás fejlesztés és megismerését több mint elég kód tookeep hogyan ebben a példában a toobuild.</span><span class="sxs-lookup"><span data-stu-id="e2167-335">You also have more than enough code tookeep developing your service and learning how toobuild on this example.</span></span>

<span data-ttu-id="e2167-336">Ha érdekli hello lépések a a ADAL használatában, az alábbiakban néhány azt javasoljuk, hogy tovább dolgozik a támogatott ADAL ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="e2167-336">If you are interested in hello next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="e2167-337">Lefelé tooyour fejlesztői gép klónozását, és hello forgatókönyv megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="e2167-337">Clone down tooyour developer machine and configure as described in hello walkthrough.</span></span>

[<span data-ttu-id="e2167-338">Az IOS-es ADAL</span><span class="sxs-lookup"><span data-stu-id="e2167-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="e2167-339">ADAL androidhoz</span><span class="sxs-lookup"><span data-stu-id="e2167-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
