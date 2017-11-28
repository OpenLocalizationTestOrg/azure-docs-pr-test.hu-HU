---
title: "Egy Azure Active Directory v2.0 webes API biztonságossá tétele a Node.js használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan build egy Node.js webes API-t, amely fogadja a jogkivonatokat, a személyes Microsoft-fiók pedig a munkahelyi vagy iskolai fiókok."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 94e945a52b9df7c495de1611baa08083357670c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="b6456-103">Biztonságos webes API-k számára a Node.js segítségével</span><span class="sxs-lookup"><span data-stu-id="b6456-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="b6456-104">Nem minden Azure Active Directory forgatókönyvek és funkciók használata a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="b6456-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="b6456-105">Annak megállapításához, hogy használja a v2.0-végpontra vagy az 1.0-s verziójú végpont, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="b6456-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="b6456-106">Az Azure Active Directory (Azure AD) v2.0-végponttól használata esetén használhatja [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok védelme érdekében a webes API-t.</span><span class="sxs-lookup"><span data-stu-id="b6456-106">When you use the Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens to protect your web API.</span></span> <span data-ttu-id="b6456-107">OAuth 2.0 hozzáférési jogkivonatok, felhasználók, akik egy személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok biztonságosan elérhet a webes API-t.</span><span class="sxs-lookup"><span data-stu-id="b6456-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="b6456-108">A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="b6456-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="b6456-109">Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6456-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="b6456-110">Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="b6456-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="b6456-111">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="b6456-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="b6456-112">Ebben a cikkben azt mutatja be a modul telepítése, és adja hozzá a az Azure AD `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="b6456-112">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="b6456-113">Letöltés</span><span class="sxs-lookup"><span data-stu-id="b6456-113">Download</span></span>
<span data-ttu-id="b6456-114">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="b6456-114">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="b6456-115">Az oktatóanyag ismerteti, hogy is [töltse le az alkalmazás vázát .zip-fájlként](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="b6456-115">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="b6456-116">Ez az oktatóanyag végén az elkészült alkalmazás is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="b6456-116">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="b6456-117">1: az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b6456-117">1: Register an app</span></span>
<span data-ttu-id="b6456-118">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) regisztrálnia az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b6456-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="b6456-119">Ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="b6456-119">Make sure you:</span></span>

* <span data-ttu-id="b6456-120">Másolás a **alkalmazásazonosító** az alkalmazáshoz hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="b6456-120">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="b6456-121">Szüksége lesz az ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="b6456-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="b6456-122">Adja hozzá a **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b6456-122">Add the **Mobile** platform for your app.</span></span>
* <span data-ttu-id="b6456-123">Másolás a **átirányítási URI-** a portálról.</span><span class="sxs-lookup"><span data-stu-id="b6456-123">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="b6456-124">Az alapértelmezett URI értéket kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="b6456-124">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="b6456-125">2: telepítse a Node.js-</span><span class="sxs-lookup"><span data-stu-id="b6456-125">2: Install Node.js</span></span>
<span data-ttu-id="b6456-126">Ebben az oktatóanyagban a minta használatához le kell [telepítse a Node.js-](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="b6456-126">To use the sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="b6456-127">3: a MongoDB telepítése</span><span class="sxs-lookup"><span data-stu-id="b6456-127">3: Install MongoDB</span></span>
<span data-ttu-id="b6456-128">Ez a minta használatához le kell [a MongoDB telepítése](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="b6456-128">To successfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="b6456-129">Ez a példa azt mongodb-re a REST API állandó különböző kiszolgálópéldányok közötti.</span><span class="sxs-lookup"><span data-stu-id="b6456-129">In this sample, you use MongoDB to make your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="b6456-130">Ez a cikk feltételezzük, hogy az alapértelmezett telepítését és kiszolgálóvégpontjait használja MongoDB: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="b6456-130">In this article, we assume that you use the default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="b6456-131">4: a restify-modulok telepítése a webes API</span><span class="sxs-lookup"><span data-stu-id="b6456-131">4: Install the restify modules in your web API</span></span>
<span data-ttu-id="b6456-132">Resitfy hozhat létre REST API-n használjuk.</span><span class="sxs-lookup"><span data-stu-id="b6456-132">We use Resitfy to build our REST API.</span></span> <span data-ttu-id="b6456-133">A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van.</span><span class="sxs-lookup"><span data-stu-id="b6456-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="b6456-134">A restify tartozik egy hatékony funkciókat, amelyek hozhat létre REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b6456-134">Restify has a robust set of features that you can use to build REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="b6456-135">A restify telepítése</span><span class="sxs-lookup"><span data-stu-id="b6456-135">Install restify</span></span>
1.  <span data-ttu-id="b6456-136">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-136">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="b6456-137">Ha a **azuread** könyvtár nem létezik, hozza létre:</span><span class="sxs-lookup"><span data-stu-id="b6456-137">If the **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="b6456-138">A restify telepítése:</span><span class="sxs-lookup"><span data-stu-id="b6456-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="b6456-139">Ez a parancs kimenetében kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b6456-139">The output of this command should look like this:</span></span>

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a><span data-ttu-id="b6456-140">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="b6456-140">Did you get an error?</span></span>
<span data-ttu-id="b6456-141">Egyes operációs rendszerek használatakor a `npm` parancsban, láthatja, hogy ez az üzenet: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="b6456-141">On some operating systems, when you use the `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="b6456-142">A hiba, akkor próbálja meg a rendszergazdaként futtatja a figyelembe kérelmet követi.</span><span class="sxs-lookup"><span data-stu-id="b6456-142">The error is followed by a request that you try running the account as an administrator.</span></span> <span data-ttu-id="b6456-143">Ha ez történik, a paranccsal `sudo` futtatásához `npm` magasabb jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="b6456-143">If this occurs, use the command `sudo` to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-to-dtrace"></a><span data-ttu-id="b6456-144">Vonatkozó DTrace hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="b6456-144">Did you get an error related to DTrace?</span></span>
<span data-ttu-id="b6456-145">Amikor telepíti a restify, láthatja, hogy ez az üzenet:</span><span class="sxs-lookup"><span data-stu-id="b6456-145">When you install restify, you might see this message:</span></span>

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

<span data-ttu-id="b6456-146">A restify egy rendkívül erős eszköz többi meghívja a DTrace segítségével nyomkövetési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b6456-146">Restify has a powerful mechanism to trace REST calls by using DTrace.</span></span> <span data-ttu-id="b6456-147">DTrace azonban számos operációs rendszeren nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b6456-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="b6456-148">Ez a hibaüzenet nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="b6456-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="b6456-149">5: telepítés Passport.js a webes API</span><span class="sxs-lookup"><span data-stu-id="b6456-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="b6456-150">A parancssorban lépjen be **azuread**.</span><span class="sxs-lookup"><span data-stu-id="b6456-150">At the command prompt, change the directory to **azuread**.</span></span>

2.  <span data-ttu-id="b6456-151">Passport.js telepítése:</span><span class="sxs-lookup"><span data-stu-id="b6456-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="b6456-152">A parancs kimenetében kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b6456-152">The output of the command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="b6456-153">6: a passport-azure-ad hozzá a web API</span><span class="sxs-lookup"><span data-stu-id="b6456-153">6: Add passport-azure-ad to your web API</span></span>
<span data-ttu-id="b6456-154">Ezután adja hozzá az OAuth stratégiát, a passport-azuread használatával.</span><span class="sxs-lookup"><span data-stu-id="b6456-154">Next, add the OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="b6456-155">`passport-azuread`a stratégiacsomag szolgál, amely az Azure AD connect a Passport van.</span><span class="sxs-lookup"><span data-stu-id="b6456-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="b6456-156">Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.</span><span class="sxs-lookup"><span data-stu-id="b6456-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="b6456-157">Bár az OAuth 2.0-s, amelyben bármely ismert jogkivonat típus kibocsáthatja keretet biztosít, néhány gyakran használják.</span><span class="sxs-lookup"><span data-stu-id="b6456-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="b6456-158">Tulajdonosi jogkivonatok gyakran használják a végpontok védelme.</span><span class="sxs-lookup"><span data-stu-id="b6456-158">Bearer tokens are commonly used to protect endpoints.</span></span> <span data-ttu-id="b6456-159">Tulajdonosi jogkivonatok olyan OAuth 2.0 token leggyakrabban kibocsátott típusú.</span><span class="sxs-lookup"><span data-stu-id="b6456-159">Bearer tokens are the most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="b6456-160">Sok OAuth 2.0 típusú hitelesítés megvalósításához feltételezik, hogy tulajdonosi jogkivonatokat bocsátottak ki csak ilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="b6456-160">Many OAuth 2.0 implementations assume that bearer tokens are the only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="b6456-161">A parancssorban lépjen be **azuread**.</span><span class="sxs-lookup"><span data-stu-id="b6456-161">At a command prompt, change the directory to **azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-162">Telepítse a Passport.js `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="b6456-162">Install the Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="b6456-163">A parancs kimenetében kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b6456-163">The output of the command should look like this:</span></span>

    ```
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
    ```

## <a name="7-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="b6456-164">7: MongoDB-modulok hozzáadása a webes API</span><span class="sxs-lookup"><span data-stu-id="b6456-164">7: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="b6456-165">Ez a példa azt a mongodb az adattárban.</span><span class="sxs-lookup"><span data-stu-id="b6456-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="b6456-166">Telepítse a mongoose bővítményt, széles körben használt beépülő modult, modellek és sémák kezelésére:</span><span class="sxs-lookup"><span data-stu-id="b6456-166">Install Mongoose, a widely used plug-in, to manage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="b6456-167">Az adatbázis illesztőprogram telepítése a mongodb, más néven MongoDB:</span><span class="sxs-lookup"><span data-stu-id="b6456-167">Install the database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="b6456-168">8: további modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="b6456-168">8: Install additional modules</span></span>
<span data-ttu-id="b6456-169">Telepítse a további szükséges modulokat.</span><span class="sxs-lookup"><span data-stu-id="b6456-169">Install the remaining required modules.</span></span>

1.  <span data-ttu-id="b6456-170">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-170">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-171">Adja meg a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b6456-171">Enter the following commands.</span></span> <span data-ttu-id="b6456-172">A parancsok a következő modulok telepítése a node_modules könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="b6456-172">The commands install the following modules in your node_modules directory:</span></span>

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="b6456-173">9: a függőségek Server.js fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6456-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="b6456-174">A Server.js fájl tartalmazza a a funkciók legnagyobb részét a webes API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="b6456-174">A Server.js file holds the majority of the functionality for your web API server.</span></span> <span data-ttu-id="b6456-175">A kód a legtöbb hozzáadása ehhez a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="b6456-175">Add most of your code to this file.</span></span> <span data-ttu-id="b6456-176">Élesben való használat akkor is refactor a funkciókat a kisebb fájlok, például különálló útvonalak és vezérlők.</span><span class="sxs-lookup"><span data-stu-id="b6456-176">For production purposes, you can refactor the functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="b6456-177">Ebben a cikkben a Server.js erre a célra használjuk.</span><span class="sxs-lookup"><span data-stu-id="b6456-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="b6456-178">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-178">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-179">A szerkesztővel az Ön által választott, hozzon létre egy Server.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="b6456-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="b6456-180">Adja hozzá a fájlhoz a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b6456-180">Add the following information to the file:</span></span>

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  <span data-ttu-id="b6456-181">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="b6456-181">Save the file.</span></span> <span data-ttu-id="b6456-182">Ekkor visszakerül az hamarosan.</span><span class="sxs-lookup"><span data-stu-id="b6456-182">You will return to it shortly.</span></span>

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="b6456-183">10: az Azure AD-beállítások tárolására konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6456-183">10: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="b6456-184">Ez a kódfájl adja át a konfigurációs paraméterek az Azure AD portálon Passport.js.</span><span class="sxs-lookup"><span data-stu-id="b6456-184">This code file passes the configuration parameters from your Azure AD portal to Passport.js.</span></span> <span data-ttu-id="b6456-185">A webes API-t a cikk elején a portálra való felvételekor létrehozta ezeket a konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="b6456-185">You created these configuration values when you added the web API to the portal at the beginning of the article.</span></span> <span data-ttu-id="b6456-186">A kód másolását követően azt ismertetjük, ezek a paraméterek kitöltésének elhelyezhető.</span><span class="sxs-lookup"><span data-stu-id="b6456-186">After you copy the code, we'll explain what to put in the values of these parameters.</span></span>

1.  <span data-ttu-id="b6456-187">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-187">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-188">Hozzon létre egy Config.js fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="b6456-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="b6456-189">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b6456-189">Add the following information:</span></span>

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="b6456-190">Kötelező értékek</span><span class="sxs-lookup"><span data-stu-id="b6456-190">Required values</span></span>

*   <span data-ttu-id="b6456-191">**IdentityMetadata**: Ez akkor, ha `passport-azure-ad` keresi a konfigurációs adatokat az identitásszolgáltató (IDP) és a kulcsok a JSON Web Tokens (JWTs) érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b6456-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for the identity provider (IDP) and the keys to validate the JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="b6456-192">Ha az Azure AD használ, nem érdemes lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="b6456-192">If you are using Azure AD, you probably don't want to change this.</span></span>

*   <span data-ttu-id="b6456-193">**a célközönség**: az átirányítási URI-t a portálról.</span><span class="sxs-lookup"><span data-stu-id="b6456-193">**audience**: Your redirect URI from the portal.</span></span>

> [!NOTE]
> <span data-ttu-id="b6456-194">Rotálja a kulcsokat rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="b6456-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="b6456-195">Győződjön meg arról, hogy Ön mindig húzza a "openid_keys" URL-címről, és, hogy az alkalmazás képes-e érni az internetet.</span><span class="sxs-lookup"><span data-stu-id="b6456-195">Be sure that you always pull from the "openid_keys" URL, and that the app can access the Internet.</span></span>
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a><span data-ttu-id="b6456-196">11: a konfiguráció hozzáadása a Server.js fájlhoz</span><span class="sxs-lookup"><span data-stu-id="b6456-196">11: Add the configuration to your Server.js file</span></span>
<span data-ttu-id="b6456-197">Az alkalmazás kell kiolvasni az értékeket a most létrehozott konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="b6456-197">Your application needs to read the values from the config file you just created.</span></span> <span data-ttu-id="b6456-198">Adja hozzá a .config kiterjesztésű fájlt kötelező erőforrásként az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b6456-198">Add the .config file as a required resource in your application.</span></span> <span data-ttu-id="b6456-199">A globális változók értékét, amit a Config.js.</span><span class="sxs-lookup"><span data-stu-id="b6456-199">Set the global variables to those that are in Config.js.</span></span>

1.  <span data-ttu-id="b6456-200">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-200">At the command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-201">Nyissa meg a Server.js valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="b6456-201">In an editor, open Server.js.</span></span> <span data-ttu-id="b6456-202">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b6456-202">Add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="b6456-203">Új szakasz hozzáadása a Server.js:</span><span class="sxs-lookup"><span data-stu-id="b6456-203">Add a new section to Server.js:</span></span>

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="b6456-204">12: a MongoDB-modell és séma-információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="b6456-204">12: Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="b6456-205">Ezek a fájlok a REST API-szolgáltatás a következő csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="b6456-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="b6456-206">Ebben a cikkben a MongoDB a feladatok tárolására használjuk.</span><span class="sxs-lookup"><span data-stu-id="b6456-206">In this article, we use MongoDB to store our tasks.</span></span> <span data-ttu-id="b6456-207">Ezt a tárgyaljuk *4. lépés*.</span><span class="sxs-lookup"><span data-stu-id="b6456-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="b6456-208">A Config.js fájl 11 lépésben létrehozott, az adatbázis neve *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="b6456-208">In the Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="b6456-209">Amely volt helyezze a mongoose_auth_local kapcsolati URL-cím végén.</span><span class="sxs-lookup"><span data-stu-id="b6456-209">That was what you put at the end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="b6456-210">Ezt az adatbázist nem szükséges előre létrehozni a MongoDB-ben.</span><span class="sxs-lookup"><span data-stu-id="b6456-210">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="b6456-211">Az adatbázis létrehozása (feltéve, hogy az adatbázis már nem létezik) a kiszolgálóalkalmazás első alkalommal történő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="b6456-211">The database is created on the first run of your server application (assuming the database does not already exist).</span></span>

<span data-ttu-id="b6456-212">Megadta, hogy rendelkezik a kiszolgáló milyen MongoDB-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="b6456-212">You've told the server what MongoDB database to use.</span></span> <span data-ttu-id="b6456-213">Ezután meg kell írnia a modell és a kiszolgáló feladatok séma létrehozásához további kódot.</span><span class="sxs-lookup"><span data-stu-id="b6456-213">Next, you need to write some additional code to create the model and schema for your server's tasks.</span></span>

### <a name="the-model"></a><span data-ttu-id="b6456-214">A modell</span><span class="sxs-lookup"><span data-stu-id="b6456-214">The model</span></span>
<span data-ttu-id="b6456-215">A séma modell nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="b6456-215">The schema model is very basic.</span></span> <span data-ttu-id="b6456-216">Ha szeretné bővítheti.</span><span class="sxs-lookup"><span data-stu-id="b6456-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="b6456-217">A séma modellje ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="b6456-217">The schema model has these values:</span></span>

*   <span data-ttu-id="b6456-218">**NÉV**.</span><span class="sxs-lookup"><span data-stu-id="b6456-218">**NAME**.</span></span> <span data-ttu-id="b6456-219">A feladathoz rendelt személy.</span><span class="sxs-lookup"><span data-stu-id="b6456-219">The person assigned to the task.</span></span> <span data-ttu-id="b6456-220">Ez egy **karakterlánc** érték.</span><span class="sxs-lookup"><span data-stu-id="b6456-220">This is a **string** value.</span></span>
*   <span data-ttu-id="b6456-221">**A FELADAT**.</span><span class="sxs-lookup"><span data-stu-id="b6456-221">**TASK**.</span></span> <span data-ttu-id="b6456-222">A feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="b6456-222">The name of the task.</span></span> <span data-ttu-id="b6456-223">Ez egy **karakterlánc** érték.</span><span class="sxs-lookup"><span data-stu-id="b6456-223">This is a **string** value.</span></span>
*   <span data-ttu-id="b6456-224">**DÁTUM**.</span><span class="sxs-lookup"><span data-stu-id="b6456-224">**DATE**.</span></span> <span data-ttu-id="b6456-225">A dátum, a feladat miatt.</span><span class="sxs-lookup"><span data-stu-id="b6456-225">The date that the task is due.</span></span> <span data-ttu-id="b6456-226">Ez egy **datetime** érték.</span><span class="sxs-lookup"><span data-stu-id="b6456-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="b6456-227">**BEFEJEZŐDÖTT**.</span><span class="sxs-lookup"><span data-stu-id="b6456-227">**COMPLETED**.</span></span> <span data-ttu-id="b6456-228">Hogy a feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b6456-228">Whether the task is completed.</span></span> <span data-ttu-id="b6456-229">Ez egy **logikai** érték.</span><span class="sxs-lookup"><span data-stu-id="b6456-229">This is a **Boolean** value.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="b6456-230">A séma kódjának megírása</span><span class="sxs-lookup"><span data-stu-id="b6456-230">Create the schema in the code</span></span>
1.  <span data-ttu-id="b6456-231">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-231">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-232">A szerkesztő nyissa meg a Server.js.</span><span class="sxs-lookup"><span data-stu-id="b6456-232">In your editor, open Server.js.</span></span> <span data-ttu-id="b6456-233">A konfigurációs bejegyzés alatt adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b6456-233">Below the configuration entry, add the following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="b6456-234">Ez a kód a MongoDB-kiszolgálóhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b6456-234">This code connects to the MongoDB server.</span></span> <span data-ttu-id="b6456-235">Emellett egy séma-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b6456-235">It also returns a schema object.</span></span>

#### <a name="using-the-schema-create-your-model-in-the-code"></a><span data-ttu-id="b6456-236">A modell a kódban a séma használatával hozhat létre</span><span class="sxs-lookup"><span data-stu-id="b6456-236">Using the schema, create your model in the code</span></span>
<span data-ttu-id="b6456-237">Az előzőekben látható kód alatt adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="b6456-237">Below the preceding code, add the following code:</span></span>

```Javascript
// Create a basic schema to store your tasks and users.
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

<span data-ttu-id="b6456-238">Mivel megadható, hogy a kód, először hoz létre a sémát.</span><span class="sxs-lookup"><span data-stu-id="b6456-238">As you can tell from the code, first you create your schema.</span></span> <span data-ttu-id="b6456-239">A következő modell-objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b6456-239">Next, you create a model object.</span></span> <span data-ttu-id="b6456-240">A modellobjektumot használatával tárolja az adatait, a kód egészében, ha a **útvonalak**.</span><span class="sxs-lookup"><span data-stu-id="b6456-240">You use the model object to store your data throughout the code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="b6456-241">13: a feladat REST API-kiszolgálóhoz a mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b6456-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="b6456-242">Most, hogy egy adatbázismodell együttműködni, adja hozzá az útvonalakat a REST API-kiszolgálóhoz használni.</span><span class="sxs-lookup"><span data-stu-id="b6456-242">Now that you have a database model to work with, add the routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="b6456-243">Az útvonalakra vonatkozó restify</span><span class="sxs-lookup"><span data-stu-id="b6456-243">About routes in restify</span></span>
<span data-ttu-id="b6456-244">Az útvonalak restify munkahelyi ugyanúgy így tesznek, amikor az Express verem használata.</span><span class="sxs-lookup"><span data-stu-id="b6456-244">Routes in restify work exactly the same way they do when you use the Express stack.</span></span> <span data-ttu-id="b6456-245">Az útvonalakat azon URI használatával kell meghatározni, amelyet elképzelései szerint az ügyfélalkalmazások meg fognak hívni.</span><span class="sxs-lookup"><span data-stu-id="b6456-245">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="b6456-246">Az útvonalakat általában külön fájlban definiálni.</span><span class="sxs-lookup"><span data-stu-id="b6456-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="b6456-247">Ebben az oktatóanyagban a Server.js az útvonalak elhelyezni azt.</span><span class="sxs-lookup"><span data-stu-id="b6456-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="b6456-248">Éles környezetben való használathoz javasoljuk a saját fájlba útvonalak figyelembe.</span><span class="sxs-lookup"><span data-stu-id="b6456-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="b6456-249">A restify-útvonal egy tipikus mintája a következő:</span><span class="sxs-lookup"><span data-stu-id="b6456-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="b6456-250">Ez a minta a legalapvetőbb szinten.</span><span class="sxs-lookup"><span data-stu-id="b6456-250">This is the pattern at the most basic level.</span></span> <span data-ttu-id="b6456-251">A restify (és az Express) sokkal összetettebb funkciók, például a alkalmazástípusok és a különböző végpontok között összetett útválasztási adja meg.</span><span class="sxs-lookup"><span data-stu-id="b6456-251">Restify (and Express) provide much deeper functionality, like the ability to define application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="b6456-252">Alapértelmezett útvonalak beállítása a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="b6456-252">Add default routes to your server</span></span>
<span data-ttu-id="b6456-253">Adja hozzá az alapszintű CRUD-útvonalakat: **létrehozása**, **beolvasása**, **frissítése**, és **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b6456-253">Add the basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="b6456-254">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-254">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="b6456-255">Nyissa meg a Server.js valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="b6456-255">In an editor, open Server.js.</span></span> <span data-ttu-id="b6456-256">A korábban létrehozott adatbázis-bejegyzések az alábbiakban adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b6456-256">Below the database entries you made earlier, add the following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
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
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    log.info("listTasks was called for: ", owner);
    Task.find({
    owner: owner
    }).limit(20).sort('date').exec(function(err, data) {
    if (err)
    return next(err);
    if (data.length > 0) {
    log.info(data);
    }
    if (!data.length) {
    log.warn(err, "There are no tasks in the database. Add one!");
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

### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="b6456-257">Hibakezelés beállítása az útvonalakhoz</span><span class="sxs-lookup"><span data-stu-id="b6456-257">Add error handling for the routes</span></span>
<span data-ttu-id="b6456-258">Hibakezelést, az ügyfélnek a tapasztalt problémáról kommunikálhat.</span><span class="sxs-lookup"><span data-stu-id="b6456-258">Add some error handling so you can communicate back to the client about the problem you encountered.</span></span>

<span data-ttu-id="b6456-259">Adja hozzá a következő kódot már megírt kód alá:</span><span class="sxs-lookup"><span data-stu-id="b6456-259">Add the following code below the code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back to the client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="b6456-260">14: a kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6456-260">14: Create your server</span></span>
<span data-ttu-id="b6456-261">Az utolsó lépés, hogy a kiszolgáló példányát adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="b6456-261">The last thing to do is to add your server instance.</span></span> <span data-ttu-id="b6456-262">A server-példány kezeli a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="b6456-262">The server instance manages your calls.</span></span>

<span data-ttu-id="b6456-263">A restify (és az Express) részletes testreszabási, melyekkel egy REST API-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b6456-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="b6456-264">Ebben az oktatóanyagban a legalapvetőbb telepítő használjuk.</span><span class="sxs-lookup"><span data-stu-id="b6456-264">In this tutorial, we use the most basic setup.</span></span>

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst to 10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-the-routes-without-authentication-for-now"></a><span data-ttu-id="b6456-265">15: (hitelesítés nélkül, most) az útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b6456-265">15: Add the routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
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
// Register a default '/' handler
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
## <a name="16-run-the-server"></a><span data-ttu-id="b6456-266">16: a kiszolgáló futtatása</span><span class="sxs-lookup"><span data-stu-id="b6456-266">16: Run the server</span></span>
<span data-ttu-id="b6456-267">Célszerű ellenőrizni a kiszolgáló hitelesítési hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="b6456-267">It's a good idea to test your server before you add authentication.</span></span>

<span data-ttu-id="b6456-268">A legegyszerűbben úgy lehet ellenőrizni a kiszolgáló van a curl használatával parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="b6456-268">The easiest way to test your server is by using curl at a command prompt.</span></span> <span data-ttu-id="b6456-269">Ehhez szüksége egy egyszerű segédprogramra, melyekkel kimeneti adatok JSON-ként értelmezni.</span><span class="sxs-lookup"><span data-stu-id="b6456-269">To do this, you need a simple utility that you can use to parse output as JSON.</span></span> 

1.  <span data-ttu-id="b6456-270">Telepítse a JSON-segédprogramot, amely azt használja az alábbi példák:</span><span class="sxs-lookup"><span data-stu-id="b6456-270">Install the JSON tool that we use in the following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="b6456-271">Ezzel globálisan telepíti a JSON-segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="b6456-271">This installs the JSON tool globally.</span></span>

2.  <span data-ttu-id="b6456-272">Ellenőrizze, hogy fut-e a MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="b6456-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="b6456-273">Lépjen be **azuread**, majd futtassa a curl:</span><span class="sxs-lookup"><span data-stu-id="b6456-273">Change the directory to **azuread**, and then run curl:</span></span>

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  <span data-ttu-id="b6456-274">Egy feladat hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b6456-274">To add a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="b6456-275">A következő választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="b6456-275">The response should be:</span></span>

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

5.  <span data-ttu-id="b6456-276">Lista feladatok Brandon:</span><span class="sxs-lookup"><span data-stu-id="b6456-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="b6456-277">Ha ezek a parancsok futtatása nem jelenik meg hibaüzenet, készen áll OAuth hozzáadása a REST API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="b6456-277">If all these commands run without errors, you are ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="b6456-278">*Most már rendelkezik a MongoDB-vel futó REST API-kiszolgáló!*</span><span class="sxs-lookup"><span data-stu-id="b6456-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-to-your-rest-api-server"></a><span data-ttu-id="b6456-279">17: hitelesítés hozzáadása a REST API-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="b6456-279">17: Add authentication to your REST API server</span></span>
<span data-ttu-id="b6456-280">Most, hogy a futó REST API-t, beállítása az Azure AD-val használandó.</span><span class="sxs-lookup"><span data-stu-id="b6456-280">Now that you have a running REST API, set it up to use it with Azure AD.</span></span>

<span data-ttu-id="b6456-281">A parancssorban lépjen be **azuread**:</span><span class="sxs-lookup"><span data-stu-id="b6456-281">At a command prompt, change the directory to **azuread**:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="b6456-282">A passport-azure-ad részét képező képező oidcbearerstrategy használata</span><span class="sxs-lookup"><span data-stu-id="b6456-282">Use the oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="b6456-283">Az eddigi létrehozott egy tipikus REST TODO kiszolgálót hitelesítés nélkül.</span><span class="sxs-lookup"><span data-stu-id="b6456-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="b6456-284">Ezután adja hozzá a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="b6456-284">Now, add authentication.</span></span>

<span data-ttu-id="b6456-285">Először is jelzi, hogy szeretné-e használni a Passport.</span><span class="sxs-lookup"><span data-stu-id="b6456-285">First,  indicate that you want to use Passport.</span></span> <span data-ttu-id="b6456-286">Ez a jogosultság helyezze a korábbi kiszolgáló konfigurálása után:</span><span class="sxs-lookup"><span data-stu-id="b6456-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="b6456-287">API-k írásakor mindig kapcsolja az adatokat egy egyedi, amely a felhasználói nem tudnak jogosulatlanul hozzáférni a jogkivonatból jó ötlet is.</span><span class="sxs-lookup"><span data-stu-id="b6456-287">When you write APIs, it's a good idea to always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="b6456-288">Ha a kiszolgáló tárolja a TODO elemeket tárolja őket a felhasználó a token (más néven token.sub keresztül) szereplő előfizetés-azonosító alapján.</span><span class="sxs-lookup"><span data-stu-id="b6456-288">When this server stores TODO items, it stores them based on the user subscription ID in the token (called through token.sub).</span></span> <span data-ttu-id="b6456-289">A token.sub be a "tulajdonos" mező.</span><span class="sxs-lookup"><span data-stu-id="b6456-289">You put the token.sub in the “owner” field.</span></span> <span data-ttu-id="b6456-290">Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e a felhasználó TODOs.</span><span class="sxs-lookup"><span data-stu-id="b6456-290">This ensures that only that user can access the user's TODOs.</span></span> <span data-ttu-id="b6456-291">A megadott TODOs senki más nem érheti el.</span><span class="sxs-lookup"><span data-stu-id="b6456-291">No one else can access the TODOs that were entered.</span></span> <span data-ttu-id="b6456-292">Nem jelenik meg sehol van az API-t, a "tulajdonos".</span><span class="sxs-lookup"><span data-stu-id="b6456-292">There is no exposure in the API for “owner.”</span></span> <span data-ttu-id="b6456-293">A külső felhasználó kérhet más felhasználók TODOs, még akkor is, ha hitelesített.</span><span class="sxs-lookup"><span data-stu-id="b6456-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="b6456-294">Ezután használhatja a nyitott ID Connect tulajdonosi stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="b6456-294">Next, use the Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="b6456-295">Be az alábbiakat mi beillesztett korábban:</span><span class="sxs-lookup"><span data-stu-id="b6456-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

<span data-ttu-id="b6456-296">A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="b6456-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="b6456-297">Stratégiafejlesztő mintát.</span><span class="sxs-lookup"><span data-stu-id="b6456-297">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="b6456-298">A stratégia átadni egy `function()` tokent használ, és `done` paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="b6456-298">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="b6456-299">A stratégia ad vissza, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="b6456-299">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="b6456-300">Tárolni a felhasználót és menteni a jogkivonatot, így Önnek nem kell megismételni.</span><span class="sxs-lookup"><span data-stu-id="b6456-300">Store the user and stash the token so you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6456-301">Az előzőekben látható kód minden olyan felhasználó, amely képes hitelesíteni a kiszolgáló vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b6456-301">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="b6456-302">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="b6456-302">This is known as auto-registration.</span></span> <span data-ttu-id="b6456-303">Az üzemi kiszolgálón így engedélyezni kívánja bárki, aki nélkül őket nyissa meg a regisztrációs folyamat az Ön által.</span><span class="sxs-lookup"><span data-stu-id="b6456-303">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="b6456-304">Ez általában az a megoldást látjuk a fogyasztói alkalmazásoknál.</span><span class="sxs-lookup"><span data-stu-id="b6456-304">This is usually the pattern you see in consumer apps.</span></span> <span data-ttu-id="b6456-305">Az alkalmazás is lehetővé teheti, regisztrálni a Facebook, de azt kéri, hogy további adatokat.</span><span class="sxs-lookup"><span data-stu-id="b6456-305">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="b6456-306">Ha ez az oktatóanyag nem parancssori program alkalmaz, jogkivonat-objektumból visszaadott lehetett kibontani a az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="b6456-306">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="b6456-307">Ezután meg lehet, hogy kérnie a felhasználót adhat meg további információt.</span><span class="sxs-lookup"><span data-stu-id="b6456-307">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="b6456-308">Mivel ez egy tesztkiszolgálón, hozzáadja a felhasználó közvetlenül a memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b6456-308">Because this is a test server, you add the user directly to the in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="b6456-309">A végpontok védelme</span><span class="sxs-lookup"><span data-stu-id="b6456-309">Protect endpoints</span></span>
<span data-ttu-id="b6456-310">A végpontok védelme megadásával a **passport.authenticate()** hívja meg a használni kívánt protokollt.</span><span class="sxs-lookup"><span data-stu-id="b6456-310">Protect endpoints by specifying the **passport.authenticate()** call with the protocol that you want to use.</span></span>

<span data-ttu-id="b6456-311">Az útvonal egy speciális beállítás a kiszolgáló kódjában található módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="b6456-311">You can edit your route in your server code for a more advanced option:</span></span>

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="b6456-312">18: futtassa újra a kiszolgálóalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b6456-312">18: Run your server application again</span></span>
<span data-ttu-id="b6456-313">Curl újra használja OAuth 2.0 védelmi van-e a végpontokon.</span><span class="sxs-lookup"><span data-stu-id="b6456-313">Use curl again to see if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="b6456-314">Ehhez minden az ügyfél-SDK-kat a végponton futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="b6456-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="b6456-315">A visszaadott fejlécek kell mondja el, ha a hitelesítés megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="b6456-315">The headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="b6456-316">Győződjön meg arról, hogy fut-e a MongoDB isntance:</span><span class="sxs-lookup"><span data-stu-id="b6456-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="b6456-317">Módosítsa a **azuread** könyvtárra, és használata curl:</span><span class="sxs-lookup"><span data-stu-id="b6456-317">Change to the **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="b6456-318">Próbálkozzon meg egy egyszerű POST művelettel:</span><span class="sxs-lookup"><span data-stu-id="b6456-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="b6456-319">A 401-es válasz azt jelzi, hogy a Passport réteg megpróbálja átirányítani a hitelesítési végpontra, vagyis pontosan mit szeretne.</span><span class="sxs-lookup"><span data-stu-id="b6456-319">A 401 response indicates that the Passport layer is trying to redirect to the authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="b6456-320">*Most már rendelkezik egy REST API-szolgáltatás által használt OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="b6456-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="b6456-321">Ön már megtettünk mindent, ami az OAuth 2.0-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b6456-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="b6456-322">Ehhez szüksége lesz egy további oktatóanyag áttekintése.</span><span class="sxs-lookup"><span data-stu-id="b6456-322">For that, you will need to review an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6456-323">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6456-323">Next steps</span></span>
<span data-ttu-id="b6456-324">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="b6456-324">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="b6456-325">Akkor is klónozhatja azt a Githubról:</span><span class="sxs-lookup"><span data-stu-id="b6456-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="b6456-326">Most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="b6456-326">Now, you can move on to more advanced topics.</span></span> <span data-ttu-id="b6456-327">Akkor célszerű [a v2.0-végpontra használata Node.js-webalkalmazás biztonságos](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="b6456-327">You might want to try [Secure a Node.js web app using the v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="b6456-328">Az alábbiakban néhány további források:</span><span class="sxs-lookup"><span data-stu-id="b6456-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="b6456-329">Az Azure AD v2.0 fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="b6456-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="b6456-330">A verem túlcsordulás "azure-active-directory" címke</span><span class="sxs-lookup"><span data-stu-id="b6456-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="b6456-331">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="b6456-331">Get security updates for our products</span></span>
<span data-ttu-id="b6456-332">Javasoljuk, hogy regisztráljon biztonsági incidensek fordulhat elő, ha értesítést szeretne kapni.</span><span class="sxs-lookup"><span data-stu-id="b6456-332">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="b6456-333">Az a [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) lapon, a biztonsági tanácsadók riasztást fizessen elő.</span><span class="sxs-lookup"><span data-stu-id="b6456-333">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

