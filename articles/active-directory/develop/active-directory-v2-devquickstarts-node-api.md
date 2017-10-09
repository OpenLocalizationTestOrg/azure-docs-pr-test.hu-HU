---
title: "az Azure Active Directory v2.0 webes API-k aaaSecure Node.js használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild egy Node.js webes API, a személyes Microsoft-fiók pedig a munkahelyi vagy iskolai fiókok jogkivonatokat fogad el."
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
ms.openlocfilehash: 219e324cca11e107186b7e5f995589b9260af8a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="807d9-103">Biztonságos webes API-k számára a Node.js segítségével</span><span class="sxs-lookup"><span data-stu-id="807d9-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="807d9-104">Nem minden Azure Active Directory forgatókönyvek és funkciók együttműködve hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="807d9-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="807d9-105">toodetermine kell használnia a v2.0-végponttól hello vagy hello 1.0-s verziójú végpontot, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="807d9-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="807d9-106">Azure Active Directory (Azure AD) v2.0-végponttól hello használata esetén használhatja [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok tooprotect a webes API-t.</span><span class="sxs-lookup"><span data-stu-id="807d9-106">When you use hello Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens tooprotect your web API.</span></span> <span data-ttu-id="807d9-107">OAuth 2.0 hozzáférési jogkivonatok, felhasználók, akik egy személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok biztonságosan elérhet a webes API-t.</span><span class="sxs-lookup"><span data-stu-id="807d9-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="807d9-108">A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="807d9-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="807d9-109">Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="807d9-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="807d9-110">Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="807d9-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="807d9-111">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="807d9-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="807d9-112">Ebben a cikkben megmutatjuk, hogyan tooinstall hello modul, és adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="807d9-112">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="807d9-113">Letöltés</span><span class="sxs-lookup"><span data-stu-id="807d9-113">Download</span></span>
<span data-ttu-id="807d9-114">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="807d9-114">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="807d9-115">toofollow hello oktatóanyagban is [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="807d9-115">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="807d9-116">Ez az oktatóanyag végén hello befejeződött hello alkalmazás is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="807d9-116">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="807d9-117">1: az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="807d9-117">1: Register an app</span></span>
<span data-ttu-id="807d9-118">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) tooregister egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="807d9-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="807d9-119">Ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="807d9-119">Make sure you:</span></span>

* <span data-ttu-id="807d9-120">Másolás hello **alkalmazásazonosító** tooyour app rendelve.</span><span class="sxs-lookup"><span data-stu-id="807d9-120">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="807d9-121">Szüksége lesz az ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="807d9-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="807d9-122">Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="807d9-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="807d9-123">Másolás hello **átirányítási URI-** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="807d9-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="807d9-124">Hello alapértelmezett URI azonosító értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="807d9-124">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="807d9-125">2: telepítse a Node.js-</span><span class="sxs-lookup"><span data-stu-id="807d9-125">2: Install Node.js</span></span>
<span data-ttu-id="807d9-126">Ebben az oktatóanyagban toouse hello példában kell [telepítse a Node.js-](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="807d9-126">toouse hello sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="807d9-127">3: a MongoDB telepítése</span><span class="sxs-lookup"><span data-stu-id="807d9-127">3: Install MongoDB</span></span>
<span data-ttu-id="807d9-128">toosuccessfully Ez a minta használatához be kell [a MongoDB telepítése](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="807d9-128">toosuccessfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="807d9-129">Ez a példa MongoDB toomake a REST API-t használja az állandó különböző kiszolgálópéldányok közötti.</span><span class="sxs-lookup"><span data-stu-id="807d9-129">In this sample, you use MongoDB toomake your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="807d9-130">Ez a cikk azt feltételezzük, hogy mongodb alapértelmezett telepítését és kiszolgálóvégpontjait hello használata: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="807d9-130">In this article, we assume that you use hello default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="807d9-131">4: telepítés hello restify-modulok a webes API</span><span class="sxs-lookup"><span data-stu-id="807d9-131">4: Install hello restify modules in your web API</span></span>
<span data-ttu-id="807d9-132">Használjuk Resitfy toobuild REST API-n.</span><span class="sxs-lookup"><span data-stu-id="807d9-132">We use Resitfy toobuild our REST API.</span></span> <span data-ttu-id="807d9-133">A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van.</span><span class="sxs-lookup"><span data-stu-id="807d9-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="807d9-134">A restify tartozik egy hatékony funkciókat, amelyeket felhasználhat toobuild REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="807d9-134">Restify has a robust set of features that you can use toobuild REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="807d9-135">A restify telepítése</span><span class="sxs-lookup"><span data-stu-id="807d9-135">Install restify</span></span>
1.  <span data-ttu-id="807d9-136">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-136">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="807d9-137">Ha hello **azuread** könyvtár nem létezik, hozza létre:</span><span class="sxs-lookup"><span data-stu-id="807d9-137">If hello **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="807d9-138">A restify telepítése:</span><span class="sxs-lookup"><span data-stu-id="807d9-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="807d9-139">a parancs kimenetének hello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="807d9-139">hello output of this command should look like this:</span></span>

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

#### <a name="did-you-get-an-error"></a><span data-ttu-id="807d9-140">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="807d9-140">Did you get an error?</span></span>
<span data-ttu-id="807d9-141">Egyes operációs rendszerek, hello használatakor `npm` parancsban, láthatja, hogy ez az üzenet: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="807d9-141">On some operating systems, when you use hello `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="807d9-142">hello hiba követi, hogy rendszergazdaként futó hello fiók megpróbál kérelmet.</span><span class="sxs-lookup"><span data-stu-id="807d9-142">hello error is followed by a request that you try running hello account as an administrator.</span></span> <span data-ttu-id="807d9-143">Ha ez történik, a hello parancs `sudo` toorun `npm` magasabb jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="807d9-143">If this occurs, use hello command `sudo` toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-toodtrace"></a><span data-ttu-id="807d9-144">Vonatkozó hibaüzenetet kapott tooDTrace?</span><span class="sxs-lookup"><span data-stu-id="807d9-144">Did you get an error related tooDTrace?</span></span>
<span data-ttu-id="807d9-145">Amikor telepíti a restify, láthatja, hogy ez az üzenet:</span><span class="sxs-lookup"><span data-stu-id="807d9-145">When you install restify, you might see this message:</span></span>

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

<span data-ttu-id="807d9-146">A restify egy rendkívül erős eszköz tootrace REST meghívja a DTrace segítségével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="807d9-146">Restify has a powerful mechanism tootrace REST calls by using DTrace.</span></span> <span data-ttu-id="807d9-147">DTrace azonban számos operációs rendszeren nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="807d9-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="807d9-148">Ez a hibaüzenet nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="807d9-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="807d9-149">5: telepítés Passport.js a webes API</span><span class="sxs-lookup"><span data-stu-id="807d9-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="807d9-150">Hello parancssorban módosítsa hello könyvtárat túl**azuread**.</span><span class="sxs-lookup"><span data-stu-id="807d9-150">At hello command prompt, change hello directory too**azuread**.</span></span>

2.  <span data-ttu-id="807d9-151">Passport.js telepítése:</span><span class="sxs-lookup"><span data-stu-id="807d9-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="807d9-152">hello parancs kimenetében hello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="807d9-152">hello output of hello command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="807d9-153">6: a passport-azure-ad tooyour webes API hozzáadása</span><span class="sxs-lookup"><span data-stu-id="807d9-153">6: Add passport-azure-ad tooyour web API</span></span>
<span data-ttu-id="807d9-154">Ezután adja hozzá a hello OAuth stratégiát, a passport-azuread használatával.</span><span class="sxs-lookup"><span data-stu-id="807d9-154">Next, add hello OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="807d9-155">`passport-azuread`a stratégiacsomag szolgál, amely az Azure AD connect a Passport van.</span><span class="sxs-lookup"><span data-stu-id="807d9-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="807d9-156">Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.</span><span class="sxs-lookup"><span data-stu-id="807d9-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="807d9-157">Bár az OAuth 2.0-s, amelyben bármely ismert jogkivonat típus kibocsáthatja keretet biztosít, néhány gyakran használják.</span><span class="sxs-lookup"><span data-stu-id="807d9-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="807d9-158">Tulajdonosi jogkivonatok olyan gyakran használt tooprotect végpontok.</span><span class="sxs-lookup"><span data-stu-id="807d9-158">Bearer tokens are commonly used tooprotect endpoints.</span></span> <span data-ttu-id="807d9-159">Tulajdonosi jogkivonatok legszélesebb körben kiadott hello lexikális elem szerepel az OAuth 2.0 típusú.</span><span class="sxs-lookup"><span data-stu-id="807d9-159">Bearer tokens are hello most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="807d9-160">Sok OAuth 2.0 típusú hitelesítés megvalósításához feltételezik, hogy hello csak ilyen típusú kiállított jogkivonat tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="807d9-160">Many OAuth 2.0 implementations assume that bearer tokens are hello only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="807d9-161">A parancssorban módosítsa hello könyvtárat túl**azuread**.</span><span class="sxs-lookup"><span data-stu-id="807d9-161">At a command prompt, change hello directory too**azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-162">Telepítse a hello Passport.js `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="807d9-162">Install hello Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="807d9-163">hello parancs kimenetében hello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="807d9-163">hello output of hello command should look like this:</span></span>

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

## <a name="7-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="807d9-164">7: vegye fel a MongoDB modulok tooyour webes API</span><span class="sxs-lookup"><span data-stu-id="807d9-164">7: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="807d9-165">Ez a példa azt a mongodb az adattárban.</span><span class="sxs-lookup"><span data-stu-id="807d9-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="807d9-166">Telepítse a mongoose bővítményt, széles körben használt beépülő modult, toomanage modellek és sémák:</span><span class="sxs-lookup"><span data-stu-id="807d9-166">Install Mongoose, a widely used plug-in, toomanage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="807d9-167">Hello adatbázis illesztőprogram telepítése a mongodb, más néven MongoDB:</span><span class="sxs-lookup"><span data-stu-id="807d9-167">Install hello database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="807d9-168">8: további modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="807d9-168">8: Install additional modules</span></span>
<span data-ttu-id="807d9-169">Telepítse a szükséges modulokat fennmaradó hello.</span><span class="sxs-lookup"><span data-stu-id="807d9-169">Install hello remaining required modules.</span></span>

1.  <span data-ttu-id="807d9-170">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-170">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-171">Adja meg a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="807d9-171">Enter hello following commands.</span></span> <span data-ttu-id="807d9-172">hello parancsok a node_modules könyvtárban modulok a következő hello telepítése:</span><span class="sxs-lookup"><span data-stu-id="807d9-172">hello commands install hello following modules in your node_modules directory:</span></span>

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

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="807d9-173">9: a függőségek Server.js fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="807d9-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="807d9-174">A Server.js fájl tárolja a hello többsége hello funkcióit a webes API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="807d9-174">A Server.js file holds hello majority of hello functionality for your web API server.</span></span> <span data-ttu-id="807d9-175">Adja hozzá a kódfájl toothis része.</span><span class="sxs-lookup"><span data-stu-id="807d9-175">Add most of your code toothis file.</span></span> <span data-ttu-id="807d9-176">Élesben való használat akkor is refactor hello funkciókat a kisebb fájlok, például különálló útvonalak és vezérlők.</span><span class="sxs-lookup"><span data-stu-id="807d9-176">For production purposes, you can refactor hello functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="807d9-177">Ebben a cikkben a Server.js erre a célra használjuk.</span><span class="sxs-lookup"><span data-stu-id="807d9-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="807d9-178">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-178">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-179">A szerkesztővel az Ön által választott, hozzon létre egy Server.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="807d9-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="807d9-180">Adja hozzá a következő információk toohello fájl hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-180">Add hello following information toohello file:</span></span>

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

3.  <span data-ttu-id="807d9-181">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="807d9-181">Save hello file.</span></span> <span data-ttu-id="807d9-182">Ekkor visszakerül tooit hamarosan.</span><span class="sxs-lookup"><span data-stu-id="807d9-182">You will return tooit shortly.</span></span>

## <a name="10-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="807d9-183">10: hozzon létre egy konfigurációs fájl toostore az Azure AD-beállítások</span><span class="sxs-lookup"><span data-stu-id="807d9-183">10: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="807d9-184">Ez a kódfájl hello konfigurációs paraméterek az Azure AD portálon tooPassport.js a adja át.</span><span class="sxs-lookup"><span data-stu-id="807d9-184">This code file passes hello configuration parameters from your Azure AD portal tooPassport.js.</span></span> <span data-ttu-id="807d9-185">Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello elején hello cikk felvételekor.</span><span class="sxs-lookup"><span data-stu-id="807d9-185">You created these configuration values when you added hello web API toohello portal at hello beginning of hello article.</span></span> <span data-ttu-id="807d9-186">Hello kód másolását követően azt ismertetjük, milyen tooput hello értékeket a következő paraméterek közül.</span><span class="sxs-lookup"><span data-stu-id="807d9-186">After you copy hello code, we'll explain what tooput in hello values of these parameters.</span></span>

1.  <span data-ttu-id="807d9-187">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-187">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-188">Hozzon létre egy Config.js fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="807d9-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="807d9-189">Adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-189">Add hello following information:</span></span>

    ```Javascript
    // Don't commit this file tooyour public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need toochange this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="807d9-190">Kötelező értékek</span><span class="sxs-lookup"><span data-stu-id="807d9-190">Required values</span></span>

*   <span data-ttu-id="807d9-191">**IdentityMetadata**: Ez akkor, ha `passport-azure-ad` hello identitásszolgáltató (IDP) és hello kulcsok toovalidate hello JSON Web Tokens (JWTs) a konfigurációs adatok keresi.</span><span class="sxs-lookup"><span data-stu-id="807d9-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for hello identity provider (IDP) and hello keys toovalidate hello JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="807d9-192">Ha az Azure AD használ, valószínűleg nem toochange ezt szeretné.</span><span class="sxs-lookup"><span data-stu-id="807d9-192">If you are using Azure AD, you probably don't want toochange this.</span></span>

*   <span data-ttu-id="807d9-193">**a célközönség**: az átirányítási URI-t hello portálról.</span><span class="sxs-lookup"><span data-stu-id="807d9-193">**audience**: Your redirect URI from hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="807d9-194">Rotálja a kulcsokat rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="807d9-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="807d9-195">Győződjön meg arról, hogy mindig lekéréses hello "openid_keys" URL-címről, és adott hello alkalmazáshoz hozzáférnek hello Internet.</span><span class="sxs-lookup"><span data-stu-id="807d9-195">Be sure that you always pull from hello "openid_keys" URL, and that hello app can access hello Internet.</span></span>
> 
> 

## <a name="11-add-hello-configuration-tooyour-serverjs-file"></a><span data-ttu-id="807d9-196">11: hello konfigurációs tooyour Server.js fájl felvétele</span><span class="sxs-lookup"><span data-stu-id="807d9-196">11: Add hello configuration tooyour Server.js file</span></span>
<span data-ttu-id="807d9-197">Az alkalmazás kell tooread hello értékek újonnan létrehozott hello konfigurációs fájlból.</span><span class="sxs-lookup"><span data-stu-id="807d9-197">Your application needs tooread hello values from hello config file you just created.</span></span> <span data-ttu-id="807d9-198">Adja hozzá a hello .config kiterjesztésű fájlt kötelező erőforrásként az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="807d9-198">Add hello .config file as a required resource in your application.</span></span> <span data-ttu-id="807d9-199">Állítsa be a Config.js hello globális változók toothose.</span><span class="sxs-lookup"><span data-stu-id="807d9-199">Set hello global variables toothose that are in Config.js.</span></span>

1.  <span data-ttu-id="807d9-200">Hello parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-200">At hello command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-201">Nyissa meg a Server.js valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="807d9-201">In an editor, open Server.js.</span></span> <span data-ttu-id="807d9-202">Adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-202">Add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="807d9-203">Adja hozzá egy új szakasz tooServer.js:</span><span class="sxs-lookup"><span data-stu-id="807d9-203">Add a new section tooServer.js:</span></span>

    ```Javascript
    // Pass these options in toohello ODICBearerStrategy.
    var options = {
    // hello URL of hello metadata document for your app. Put hello keys for token validation from hello URL found in hello jwks_uri tag in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array toohold signed-in users and hello current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="807d9-204">12: hello MongoDB modell és séma-információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="807d9-204">12: Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="807d9-205">Ezek a fájlok a REST API-szolgáltatás a következő csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="807d9-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="807d9-206">Ebben a cikkben használjuk MongoDB toostore a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="807d9-206">In this article, we use MongoDB toostore our tasks.</span></span> <span data-ttu-id="807d9-207">Ezt a tárgyaljuk *4. lépés*.</span><span class="sxs-lookup"><span data-stu-id="807d9-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="807d9-208">Hello Config.js fájl 11 lépésben létrehozott, az adatbázis neve *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="807d9-208">In hello Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="807d9-209">Amely volt a mongoose_auth_local kapcsolati URL-cím végén hello helyezni.</span><span class="sxs-lookup"><span data-stu-id="807d9-209">That was what you put at hello end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="807d9-210">Ez az adatbázis előre a MongoDB-ben nem kell toocreate.</span><span class="sxs-lookup"><span data-stu-id="807d9-210">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="807d9-211">hello adatbázis létrehozása a hello először futtassa a kiszolgálóalkalmazás (feltéve, hogy hello adatbázis már nem létezik).</span><span class="sxs-lookup"><span data-stu-id="807d9-211">hello database is created on hello first run of your server application (assuming hello database does not already exist).</span></span>

<span data-ttu-id="807d9-212">Megadta, hogy rendelkezik hello server melyik MongoDB adatbázis toouse.</span><span class="sxs-lookup"><span data-stu-id="807d9-212">You've told hello server what MongoDB database toouse.</span></span> <span data-ttu-id="807d9-213">A következő lépésben toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="807d9-213">Next, you need toowrite some additional code toocreate hello model and schema for your server's tasks.</span></span>

### <a name="hello-model"></a><span data-ttu-id="807d9-214">hello modell</span><span class="sxs-lookup"><span data-stu-id="807d9-214">hello model</span></span>
<span data-ttu-id="807d9-215">hello sémamodell nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="807d9-215">hello schema model is very basic.</span></span> <span data-ttu-id="807d9-216">Ha szeretné bővítheti.</span><span class="sxs-lookup"><span data-stu-id="807d9-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="807d9-217">hello séma modellje ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="807d9-217">hello schema model has these values:</span></span>

*   <span data-ttu-id="807d9-218">**NÉV**.</span><span class="sxs-lookup"><span data-stu-id="807d9-218">**NAME**.</span></span> <span data-ttu-id="807d9-219">hello személy hozzárendelt toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="807d9-219">hello person assigned toohello task.</span></span> <span data-ttu-id="807d9-220">Ez egy **karakterlánc** érték.</span><span class="sxs-lookup"><span data-stu-id="807d9-220">This is a **string** value.</span></span>
*   <span data-ttu-id="807d9-221">**A FELADAT**.</span><span class="sxs-lookup"><span data-stu-id="807d9-221">**TASK**.</span></span> <span data-ttu-id="807d9-222">hello hello feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="807d9-222">hello name of hello task.</span></span> <span data-ttu-id="807d9-223">Ez egy **karakterlánc** érték.</span><span class="sxs-lookup"><span data-stu-id="807d9-223">This is a **string** value.</span></span>
*   <span data-ttu-id="807d9-224">**DÁTUM**.</span><span class="sxs-lookup"><span data-stu-id="807d9-224">**DATE**.</span></span> <span data-ttu-id="807d9-225">hello dátum hello tevékenység esedékessé válik.</span><span class="sxs-lookup"><span data-stu-id="807d9-225">hello date that hello task is due.</span></span> <span data-ttu-id="807d9-226">Ez egy **datetime** érték.</span><span class="sxs-lookup"><span data-stu-id="807d9-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="807d9-227">**BEFEJEZŐDÖTT**.</span><span class="sxs-lookup"><span data-stu-id="807d9-227">**COMPLETED**.</span></span> <span data-ttu-id="807d9-228">E hello feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="807d9-228">Whether hello task is completed.</span></span> <span data-ttu-id="807d9-229">Ez egy **logikai** érték.</span><span class="sxs-lookup"><span data-stu-id="807d9-229">This is a **Boolean** value.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="807d9-230">Hozzon létre hello séma hello kódban</span><span class="sxs-lookup"><span data-stu-id="807d9-230">Create hello schema in hello code</span></span>
1.  <span data-ttu-id="807d9-231">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-231">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-232">A szerkesztő nyissa meg a Server.js.</span><span class="sxs-lookup"><span data-stu-id="807d9-232">In your editor, open Server.js.</span></span> <span data-ttu-id="807d9-233">Hello konfigurációs bejegyzés alatt adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-233">Below hello configuration entry, add hello following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="807d9-234">Ez a kód toohello MongoDB-kiszolgálóhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="807d9-234">This code connects toohello MongoDB server.</span></span> <span data-ttu-id="807d9-235">Emellett egy séma-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="807d9-235">It also returns a schema object.</span></span>

#### <a name="using-hello-schema-create-your-model-in-hello-code"></a><span data-ttu-id="807d9-236">A modell hello séma használatával hozhat létre hello kódban</span><span class="sxs-lookup"><span data-stu-id="807d9-236">Using hello schema, create your model in hello code</span></span>
<span data-ttu-id="807d9-237">Kód megelőző hello, alá adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-237">Below hello preceding code, add hello following code:</span></span>

```Javascript
// Create a basic schema toostore your tasks and users.
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

<span data-ttu-id="807d9-238">Hello kódból állapítható meg, mert először hoz létre a sémát.</span><span class="sxs-lookup"><span data-stu-id="807d9-238">As you can tell from hello code, first you create your schema.</span></span> <span data-ttu-id="807d9-239">A következő modell-objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="807d9-239">Next, you create a model object.</span></span> <span data-ttu-id="807d9-240">Hello modell objektum toostore az adatok teljes hello code definiálásakor használja a **útvonalak**.</span><span class="sxs-lookup"><span data-stu-id="807d9-240">You use hello model object toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="807d9-241">13: a feladat REST API-kiszolgálóhoz a mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="807d9-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="807d9-242">Most, hogy egy adatbázis-modell toowork rendelkező, adja hozzá a hello útvonalakat a REST API-kiszolgálóhoz használni.</span><span class="sxs-lookup"><span data-stu-id="807d9-242">Now that you have a database model toowork with, add hello routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="807d9-243">Az útvonalakra vonatkozó restify</span><span class="sxs-lookup"><span data-stu-id="807d9-243">About routes in restify</span></span>
<span data-ttu-id="807d9-244">Az útvonalak restify pontosan hello használatakor tehetik ugyanúgy hello Express verem munka.</span><span class="sxs-lookup"><span data-stu-id="807d9-244">Routes in restify work exactly hello same way they do when you use hello Express stack.</span></span> <span data-ttu-id="807d9-245">Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével.</span><span class="sxs-lookup"><span data-stu-id="807d9-245">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="807d9-246">Az útvonalakat általában külön fájlban definiálni.</span><span class="sxs-lookup"><span data-stu-id="807d9-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="807d9-247">Ebben az oktatóanyagban a Server.js az útvonalak elhelyezni azt.</span><span class="sxs-lookup"><span data-stu-id="807d9-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="807d9-248">Éles környezetben való használathoz javasoljuk a saját fájlba útvonalak figyelembe.</span><span class="sxs-lookup"><span data-stu-id="807d9-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="807d9-249">A restify-útvonal egy tipikus mintája a következő:</span><span class="sxs-lookup"><span data-stu-id="807d9-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep hello server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="807d9-250">Ez a hello mintát hello legalapvetőbb szinten.</span><span class="sxs-lookup"><span data-stu-id="807d9-250">This is hello pattern at hello most basic level.</span></span> <span data-ttu-id="807d9-251">A restify (és az Express) sokkal mélyebb funkciók, például a hello képességét toodefine alkalmazástípusok és összetett útválasztás különböző végpontok között adja meg.</span><span class="sxs-lookup"><span data-stu-id="807d9-251">Restify (and Express) provide much deeper functionality, like hello ability toodefine application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="807d9-252">Alapértelmezett útvonalak tooyour kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="807d9-252">Add default routes tooyour server</span></span>
<span data-ttu-id="807d9-253">Adja hozzá a hello alapszintű CRUD-útvonalakat: **létrehozása**, **beolvasása**, **frissítése**, és **törlése**.</span><span class="sxs-lookup"><span data-stu-id="807d9-253">Add hello basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="807d9-254">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-254">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="807d9-255">Nyissa meg a Server.js valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="807d9-255">In an editor, open Server.js.</span></span> <span data-ttu-id="807d9-256">Az alábbiakban hello korábban létrehozott adatbázis-bejegyzések, adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-256">Below hello database entries you made earlier, add hello following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow you toouse MongoDB Server as your response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it tooMongoDB.
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
    /// Returns hello list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow us toouse MongoDB Server as our response tooany origin.
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
    log.warn(err, "There are no tasks in hello database. Add one!");
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

### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="807d9-257">Hibakezelés hello útvonalak beállítása</span><span class="sxs-lookup"><span data-stu-id="807d9-257">Add error handling for hello routes</span></span>
<span data-ttu-id="807d9-258">Hibakezelést, képes kommunikálni a hátsó toohello ügyfél hello tapasztalt problémáról.</span><span class="sxs-lookup"><span data-stu-id="807d9-258">Add some error handling so you can communicate back toohello client about hello problem you encountered.</span></span>

<span data-ttu-id="807d9-259">Adja hozzá a következő alábbi hello kód, amely korábban már írt kódot hello:</span><span class="sxs-lookup"><span data-stu-id="807d9-259">Add hello following code below hello code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back toohello client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="807d9-260">14: a kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="807d9-260">14: Create your server</span></span>
<span data-ttu-id="807d9-261">utolsó lépésként toodo hello tooadd a server-példány van.</span><span class="sxs-lookup"><span data-stu-id="807d9-261">hello last thing toodo is tooadd your server instance.</span></span> <span data-ttu-id="807d9-262">hello server-példány kezeli a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="807d9-262">hello server instance manages your calls.</span></span>

<span data-ttu-id="807d9-263">A restify (és az Express) részletes testreszabási, melyekkel egy REST API-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="807d9-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="807d9-264">Ebben az oktatóanyagban hello legalapvetőbb telepítő használjuk.</span><span class="sxs-lookup"><span data-stu-id="807d9-264">In this tutorial, we use hello most basic setup.</span></span>

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
// Allow 5 requests/second by IP address, and burst too10.
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
## <a name="15-add-hello-routes-without-authentication-for-now"></a><span data-ttu-id="807d9-265">15: Adjon hozzá hello útvonalakat (hitelesítés nélkül, a lépést)</span><span class="sxs-lookup"><span data-stu-id="807d9-265">15: Add hello routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD tooadd hello real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in hello pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need toomaintain session state. You can experiment with removing API protection.
/* toodo this, remove hello passport.authenticate() method:
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
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-run-hello-server"></a><span data-ttu-id="807d9-266">16: hello server futtatása</span><span class="sxs-lookup"><span data-stu-id="807d9-266">16: Run hello server</span></span>
<span data-ttu-id="807d9-267">Egy jó ötlet tootest a kiszolgáló hitelesítési hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="807d9-267">It's a good idea tootest your server before you add authentication.</span></span>

<span data-ttu-id="807d9-268">hello legegyszerűbb módja tootest a kiszolgáló van, a parancssorba curl használatával.</span><span class="sxs-lookup"><span data-stu-id="807d9-268">hello easiest way tootest your server is by using curl at a command prompt.</span></span> <span data-ttu-id="807d9-269">toodo, ez egy egyszerű segédprogramra használható tooparse kimeneti adatok JSON-ként van szüksége.</span><span class="sxs-lookup"><span data-stu-id="807d9-269">toodo this, you need a simple utility that you can use tooparse output as JSON.</span></span> 

1.  <span data-ttu-id="807d9-270">Telepítse a következő példák hello hello JSON-segédprogramot, amely használjuk:</span><span class="sxs-lookup"><span data-stu-id="807d9-270">Install hello JSON tool that we use in hello following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="807d9-271">Ezzel globálisan telepíti a hello JSON-segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="807d9-271">This installs hello JSON tool globally.</span></span>

2.  <span data-ttu-id="807d9-272">Ellenőrizze, hogy fut-e a MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="807d9-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="807d9-273">Hello könyvtárváltás túl**azuread**, majd futtassa a curl:</span><span class="sxs-lookup"><span data-stu-id="807d9-273">Change hello directory too**azuread**, and then run curl:</span></span>

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

4.  <span data-ttu-id="807d9-274">egy feladat tooadd:</span><span class="sxs-lookup"><span data-stu-id="807d9-274">tooadd a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="807d9-275">hello választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="807d9-275">hello response should be:</span></span>

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

5.  <span data-ttu-id="807d9-276">Lista feladatok Brandon:</span><span class="sxs-lookup"><span data-stu-id="807d9-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="807d9-277">Ha ezek a parancsok futnak, nem jelenik meg hibaüzenet, készen áll a tooadd OAuth toohello REST API-kiszolgálóhoz is.</span><span class="sxs-lookup"><span data-stu-id="807d9-277">If all these commands run without errors, you are ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="807d9-278">*Most már rendelkezik a MongoDB-vel futó REST API-kiszolgáló!*</span><span class="sxs-lookup"><span data-stu-id="807d9-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="807d9-279">17: hitelesítési tooyour REST API-kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="807d9-279">17: Add authentication tooyour REST API server</span></span>
<span data-ttu-id="807d9-280">Most, hogy a futó REST API-t, állítsa be toouse az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="807d9-280">Now that you have a running REST API, set it up toouse it with Azure AD.</span></span>

<span data-ttu-id="807d9-281">A parancssorban módosítsa hello könyvtárat túl**azuread**:</span><span class="sxs-lookup"><span data-stu-id="807d9-281">At a command prompt, change hello directory too**azuread**:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="807d9-282">Hello részét képező képező oidcbearerstrategy használata a passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="807d9-282">Use hello oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="807d9-283">Az eddigi létrehozott egy tipikus REST TODO kiszolgálót hitelesítés nélkül.</span><span class="sxs-lookup"><span data-stu-id="807d9-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="807d9-284">Ezután adja hozzá a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="807d9-284">Now, add authentication.</span></span>

<span data-ttu-id="807d9-285">Először adja meg, hogy toouse Passport.</span><span class="sxs-lookup"><span data-stu-id="807d9-285">First,  indicate that you want toouse Passport.</span></span> <span data-ttu-id="807d9-286">Ez a jogosultság helyezze a korábbi kiszolgáló konfigurálása után:</span><span class="sxs-lookup"><span data-stu-id="807d9-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="807d9-287">API-k írásakor egy jó ötlet tooalways hivatkozás hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="807d9-287">When you write APIs, it's a good idea tooalways link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="807d9-288">Ha ez a kiszolgáló tárolja a TODO elemeket tárolja őket (más néven token.sub keresztül) hello token hello felhasználói előfizetés-azonosító alapján.</span><span class="sxs-lookup"><span data-stu-id="807d9-288">When this server stores TODO items, it stores them based on hello user subscription ID in hello token (called through token.sub).</span></span> <span data-ttu-id="807d9-289">Hello token.sub be hello "owner" mezőjében.</span><span class="sxs-lookup"><span data-stu-id="807d9-289">You put hello token.sub in hello “owner” field.</span></span> <span data-ttu-id="807d9-290">Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e hello felhasználói TODOs.</span><span class="sxs-lookup"><span data-stu-id="807d9-290">This ensures that only that user can access hello user's TODOs.</span></span> <span data-ttu-id="807d9-291">A megadott hello TODOs senki más nem érheti el.</span><span class="sxs-lookup"><span data-stu-id="807d9-291">No one else can access hello TODOs that were entered.</span></span> <span data-ttu-id="807d9-292">Nem jelenik meg sehol van hello API a "tulajdonos".</span><span class="sxs-lookup"><span data-stu-id="807d9-292">There is no exposure in hello API for “owner.”</span></span> <span data-ttu-id="807d9-293">A külső felhasználó kérhet más felhasználók TODOs, még akkor is, ha hitelesített.</span><span class="sxs-lookup"><span data-stu-id="807d9-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="807d9-294">Következő lépésként az hello nyitott ID Connect tulajdonosi stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="807d9-294">Next, use hello Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="807d9-295">Be az alábbiakat mi beillesztett korábban:</span><span class="sxs-lookup"><span data-stu-id="807d9-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling hello OIDCBearerStrategy and managing users.
/*
/* Because of hello Passport pattern, you need toomanage users and info tokens
/* with a FindorCreate() method. hello method must be provided by hello implementor.
/* In hello following code, you autoregister any user and implement a FindById().
/* It's a good idea toodo something more advanced.
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
log.info('verifying hello user');
log.info(token, 'was hello token retrieved');
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

<span data-ttu-id="807d9-296">A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="807d9-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="807d9-297">Stratégiafejlesztő toohello mintát.</span><span class="sxs-lookup"><span data-stu-id="807d9-297">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="807d9-298">Hello stratégia átadni egy `function()` tokent használ, és `done` paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="807d9-298">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="807d9-299">hello stratégia ad vissza, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="807d9-299">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="807d9-300">Hello felhasználói és stash hello token tárolására, így nem kell tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="807d9-300">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="807d9-301">hello előző kód készít minden olyan felhasználó, amely képes hitelesíteni tooyour kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="807d9-301">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="807d9-302">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="807d9-302">This is known as auto-registration.</span></span> <span data-ttu-id="807d9-303">Az üzemi kiszolgálón általában csak akkor érdemes toolet bárki, aki nélkül őket nyissa meg az Ön által regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="807d9-303">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="807d9-304">Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásokba.</span><span class="sxs-lookup"><span data-stu-id="807d9-304">This is usually hello pattern you see in consumer apps.</span></span> <span data-ttu-id="807d9-305">hello alkalmazást is lehetővé teheti a Facebook tooregister, de majd kéri tooenter további információt.</span><span class="sxs-lookup"><span data-stu-id="807d9-305">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="807d9-306">Ha ez az oktatóanyag nem parancssori program alkalmaz, objektumból hello token visszaadott lehetett kibontani a hello e-mail.</span><span class="sxs-lookup"><span data-stu-id="807d9-306">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="807d9-307">Ezt követően előfordulhat, hogy kérje meg hello felhasználói tooenter további információt.</span><span class="sxs-lookup"><span data-stu-id="807d9-307">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="807d9-308">Mivel ez egy tesztkiszolgálón, hozzáadhat hello felhasználó közvetlenül a toohello memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="807d9-308">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="807d9-309">A végpontok védelme</span><span class="sxs-lookup"><span data-stu-id="807d9-309">Protect endpoints</span></span>
<span data-ttu-id="807d9-310">A végpontok védelme hello megadásával **passport.authenticate()** hívás hello protokoll, amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="807d9-310">Protect endpoints by specifying hello **passport.authenticate()** call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="807d9-311">Az útvonal egy speciális beállítás a kiszolgáló kódjában található módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="807d9-311">You can edit your route in your server code for a more advanced option:</span></span>

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

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="807d9-312">18: futtassa újra a kiszolgálóalkalmazás</span><span class="sxs-lookup"><span data-stu-id="807d9-312">18: Run your server application again</span></span>
<span data-ttu-id="807d9-313">Használata curl újra toosee Ha OAuth 2.0 védelmi e a végpontokon.</span><span class="sxs-lookup"><span data-stu-id="807d9-313">Use curl again toosee if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="807d9-314">Ehhez minden az ügyfél-SDK-kat a végponton futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="807d9-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="807d9-315">visszaadott hello fejlécek kell tájékoztatnia, ha a hitelesítés megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="807d9-315">hello headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="807d9-316">Győződjön meg arról, hogy fut-e a MongoDB isntance:</span><span class="sxs-lookup"><span data-stu-id="807d9-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="807d9-317">Módosítsa a toohello **azuread** könyvtárra, és használata curl:</span><span class="sxs-lookup"><span data-stu-id="807d9-317">Change toohello **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="807d9-318">Próbálkozzon meg egy egyszerű POST művelettel:</span><span class="sxs-lookup"><span data-stu-id="807d9-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="807d9-319">A 401-es válasz azt jelzi, hogy a hello Passport réteg próbál tooredirect toohello engedélyezik a végponthoz, vagyis pontosan mit szeretne.</span><span class="sxs-lookup"><span data-stu-id="807d9-319">A 401 response indicates that hello Passport layer is trying tooredirect toohello authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="807d9-320">*Most már rendelkezik egy REST API-szolgáltatás által használt OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="807d9-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="807d9-321">Ön már megtettünk mindent, ami az OAuth 2.0-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="807d9-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="807d9-322">Az adott szüksége lesz egy további oktatóanyag tooreview.</span><span class="sxs-lookup"><span data-stu-id="807d9-322">For that, you will need tooreview an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="807d9-323">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="807d9-323">Next steps</span></span>
<span data-ttu-id="807d9-324">Befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="807d9-324">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="807d9-325">Akkor is klónozhatja azt a Githubról:</span><span class="sxs-lookup"><span data-stu-id="807d9-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="807d9-326">Most áthelyezheti a témakörök speciális toomore.</span><span class="sxs-lookup"><span data-stu-id="807d9-326">Now, you can move on toomore advanced topics.</span></span> <span data-ttu-id="807d9-327">Érdemes lehet tootry [hello v2.0-végponttól használata Node.js-webalkalmazás biztonságos](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="807d9-327">You might want tootry [Secure a Node.js web app using hello v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="807d9-328">Az alábbiakban néhány további források:</span><span class="sxs-lookup"><span data-stu-id="807d9-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="807d9-329">Az Azure AD v2.0 fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="807d9-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="807d9-330">A verem túlcsordulás "azure-active-directory" címke</span><span class="sxs-lookup"><span data-stu-id="807d9-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="807d9-331">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="807d9-331">Get security updates for our products</span></span>
<span data-ttu-id="807d9-332">Javasoljuk toosign biztonsági incidensek értesítendő toobe fel.</span><span class="sxs-lookup"><span data-stu-id="807d9-332">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="807d9-333">A hello [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) előfizetés tooSecurity tanácsadók riasztások lapján.</span><span class="sxs-lookup"><span data-stu-id="807d9-333">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

