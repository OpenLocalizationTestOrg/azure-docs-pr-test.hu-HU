---
title: "Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével | Microsoft Docs"
description: "Hogyan toobuild egy Node.js webes API-t, amely a B2C-bérlő származó jogkivonatokat fogad el"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.openlocfilehash: 47f5bae025a9ba2f486e36acef36aa37cfb43543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="a3b0d-103">Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével</span><span class="sxs-lookup"><span data-stu-id="a3b0d-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="a3b0d-104">Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="a3b0d-105">Ezek a jogkivonatok engedélyezik az Azure AD B2C tooauthenticate toohello API-t használó ügyfél alkalmazásait.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-105">These tokens allow your client apps that use Azure AD B2C tooauthenticate toohello API.</span></span> <span data-ttu-id="a3b0d-106">Ez a cikk bemutatja, hogyan toocreate egy "Feladatlista" API-t, amely lehetővé teszi a felhasználók tooadd és lista feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-106">This article shows you how toocreate a "to-do list" API that allows users tooadd and list tasks.</span></span> <span data-ttu-id="a3b0d-107">hello webes API-t az Azure AD B2C használatával lett biztonságossá téve, és csak lehetővé teszi a hitelesített felhasználók toomanage a tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="a3b0d-108">Ez a minta csatlakoztatott toobe tooby használatával íródott a [iOS B2C mintaalkalmazással](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-108">This sample was written toobe connected tooby using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="a3b0d-109">Aktuális útmutató hello először, és hajtsa végre mintaalkalmazással.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-109">Do hello current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="a3b0d-110">A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="a3b0d-111">A rugalmasan működő, moduláris Passport gyakorlatilag bármely Express- vagy Restify-alapú webalkalmazásba diszkréten telepíthető.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="a3b0d-112">A program számos különböző lehetőséget kínál a felhasználók hitelesítésére: felhasználónév/jelszó, Facebook- vagy Twitter-fiók és így tovább.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="a3b0d-113">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure Active Directory (Azure AD) esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="a3b0d-114">Telepítenie kell a modult, és adja meg az Azure AD hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-114">You install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="a3b0d-115">toodo ezt a mintát kell:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-115">toodo this sample, you need to:</span></span>

1. <span data-ttu-id="a3b0d-116">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="a3b0d-117">Állítsa be az alkalmazás toouse Passport által `azure-ad-passport` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-117">Set up your application toouse Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="a3b0d-118">Ügyfél alkalmazás toocall hello "Feladatlista" webes API-k konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-118">Configure a client application toocall hello "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="a3b0d-119">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="a3b0d-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="a3b0d-120">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="a3b0d-121">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="a3b0d-122">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="a3b0d-123">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-123">Create an application</span></span>
<span data-ttu-id="a3b0d-124">A következő lépésben egy alkalmazást a B2C-címtárban, amely ellátja az Azure AD néhány információt, hogy kell-e toosecurely kommunikációhoz toocreate az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-124">Next, you need toocreate an app in your B2C directory that gives Azure AD some information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="a3b0d-125">Ebben az esetben hello ügyfélalkalmazást és a webes API jelöli egyetlen **Alkalmazásazonosító**, mert alkalmazássá logikai.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-125">In this case, both hello client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="a3b0d-126">hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="a3b0d-127">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-127">Be sure to:</span></span>

* <span data-ttu-id="a3b0d-128">Tartalmaznak egy **webalkalmazást vagy webes api** hello alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="a3b0d-128">Include a **web app/web api** in hello application</span></span>
* <span data-ttu-id="a3b0d-129">A **Reply URL** (Válasz URL-cím) legyen a következő: `http://localhost/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="a3b0d-130">A fenti hello alapértelmezett URL-.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-130">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="a3b0d-131">Hozzon létre egy **alkalmazástitkot** az alkalmazáshoz, majd másolja.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="a3b0d-132">Erre az adatra később még szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-132">You need this data later.</span></span> <span data-ttu-id="a3b0d-133">Vegye figyelembe, hogy ezt az értéket kell toobe [escape-karaktersorozatot XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használatba vétel előtt.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="a3b0d-134">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="a3b0d-135">Erre az adatra később még szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="a3b0d-136">Házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-136">Create your policies</span></span>
<span data-ttu-id="a3b0d-137">Az Azure AD B2C-ben minden felhasználói élményt [házirendek](active-directory-b2c-reference-policies.md) határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="a3b0d-138">Ez az alkalmazás két, identitással kapcsolatos műveletet tartalmaz: regisztráció és bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="a3b0d-139">Szüksége van a toocreate egy házirendet az egyes típusok leírtak szerint a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-139">You need toocreate one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="a3b0d-140">A három szabályzat létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="a3b0d-141">Válassza ki a hello **megjelenített név** és az egyéb regisztrációs attribútumokat a regisztrációs szabályzatban.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="a3b0d-142">Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="a3b0d-143">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="a3b0d-144">Másolja le hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-144">Copy down hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="a3b0d-145">Hello előtag nem rendelkezhet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="a3b0d-146">A szabályzatok nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="a3b0d-147">A három szabályzat létrehozását követően készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-147">After you have created your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="a3b0d-148">toolearn arról, hogyan házirendek az Azure AD B2C működnek, hello kezdődnie [.NET webalkalmazások használatába bevezető oktatóanyagot](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-148">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="a3b0d-149">Hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="a3b0d-149">Download hello code</span></span>
<span data-ttu-id="a3b0d-150">Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-150">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="a3b0d-151">Ugrás toobuild hello mintát, ha Ön is [töltse le a projektvázát tartalmazó .zip-fájlt](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-151">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="a3b0d-152">Hello vázat klónozhatja is:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-152">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="a3b0d-153">befejeződött hello alkalmazás is van [elérhető .zip-fájlként](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) vagy hello `complete` hello ága adott adattár.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-153">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="a3b0d-154">A Node.js letöltése a platformra</span><span class="sxs-lookup"><span data-stu-id="a3b0d-154">Download Node.js for your platform</span></span>
<span data-ttu-id="a3b0d-155">toosuccessfully ezt a mintát használja, a Node.js telepített és működő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-155">toosuccessfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="a3b0d-156">Telepítse a Node.js-t a [nodejs.org](http://nodejs.org) oldalról.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="a3b0d-157">A MongoDB telepítése a platformra</span><span class="sxs-lookup"><span data-stu-id="a3b0d-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="a3b0d-158">toosuccessfully Ez a minta használatához a MongoDB telepített és működő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-158">toosuccessfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="a3b0d-159">Használjuk MongoDB toomake állandó a REST API különböző kiszolgálópéldányok közötti.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-159">We use MongoDB toomake your REST API persistent across server instances.</span></span>

<span data-ttu-id="a3b0d-160">Telepítse a MongoDB-t a [mongodb.org](http://www.mongodb.org) oldalról.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="a3b0d-161">Ez az útmutató feltételezi, hogy MongoDB, amely írásának hello időpontban hello alapértelmezett telepítését és kiszolgálóvégpontjait használja `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-161">This walk-through assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="a3b0d-162">Hello Restify-modulok telepítése a webes API</span><span class="sxs-lookup"><span data-stu-id="a3b0d-162">Install hello Restify modules in your web API</span></span>
<span data-ttu-id="a3b0d-163">Használjuk Restify toobuild a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-163">We use Restify toobuild your REST API.</span></span> <span data-ttu-id="a3b0d-164">A Restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer, amely az Express alapján készült.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="a3b0d-165">Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="a3b0d-166">A Restify telepítése</span><span class="sxs-lookup"><span data-stu-id="a3b0d-166">Install Restify</span></span>
<span data-ttu-id="a3b0d-167">Hello parancssorból módosítsa a könyvtárat túl`azuread`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-167">From hello command line, change your directory too`azuread`.</span></span> <span data-ttu-id="a3b0d-168">Ha hello `azuread` könyvtár nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-168">If hello `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="a3b0d-169">`cd azuread` vagy `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="a3b0d-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="a3b0d-170">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-170">Enter hello following command:</span></span>

`npm install restify`

<span data-ttu-id="a3b0d-171">Ez a parancs elvégzi a Restify telepítését.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="a3b0d-172">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="a3b0d-172">Did you get an error?</span></span>
<span data-ttu-id="a3b0d-173">Az egyes operációs rendszerek használatakor `npm`, hello hibaüzenet `Error: EPERM, chmod '/usr/local/bin/..'` és rendszergazdaként futtatni a hello fiók kérelmet.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-173">In some operating systems, when you use `npm`, you may receive hello error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run hello account as an administrator.</span></span> <span data-ttu-id="a3b0d-174">Ha a probléma akkor fordul elő, használja a hello `sudo` parancs toorun `npm` magasabb jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-174">If this problem occurs, use hello `sudo` command toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="a3b0d-175">DTrace hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="a3b0d-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="a3b0d-176">Előfordulhat, hogy a Restify telepítése során az alábbihoz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="a3b0d-177">A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="a3b0d-178">Számos operációs rendszerben azonban nem található meg a DTrace.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="a3b0d-179">Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="a3b0d-180">hello parancs kimenetében hello hasonló toothis szöveget kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-180">hello output of hello command should appear similar toothis text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="a3b0d-181">A Passport telepítése a webes API-ba</span><span class="sxs-lookup"><span data-stu-id="a3b0d-181">Install Passport in your web API</span></span>
<span data-ttu-id="a3b0d-182">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-182">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="a3b0d-183">A következő parancs hello használata a Passport telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-183">Install Passport using hello following command:</span></span>

`npm install passport`

<span data-ttu-id="a3b0d-184">hello parancs kimenetében hello hasonló toothis szöveget kell lennie:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-184">hello output of hello command should be similar toothis text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a><span data-ttu-id="a3b0d-185">Adja hozzá a passport-azuread tooyour webes API</span><span class="sxs-lookup"><span data-stu-id="a3b0d-185">Add passport-azuread tooyour web API</span></span>
<span data-ttu-id="a3b0d-186">Ezután adja hozzá az OAuth stratégiát hello segítségével `passport-azuread`, a stratégiacsomag szolgál, amely az Azure AD connect a Passport.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-186">Next, add hello OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="a3b0d-187">Használja ezt a stratégiát használható a tulajdonosi jogkivonatokhoz hello REST API minta.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-187">Use this strategy for bearer tokens in hello REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="a3b0d-188">Az OAuth2 keretrendszerében ugyan bármely ismert jogkivonat kibocsátható, csupán néhány típust használnak szélesebb körben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="a3b0d-189">hello végpontok védelmére szolgáló jogkivonatok tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-189">hello tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="a3b0d-190">Az ilyen típusú jogkivonatok hello legszélesebb körben kiadott OAuth2-ben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-190">These types of tokens are hello most widely issued in OAuth2.</span></span> <span data-ttu-id="a3b0d-191">Számos implementáció eleve feltételezik, hogy hello csak ilyen típusú kiállított jogkivonat tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-191">Many implementations assume that bearer tokens are hello only type of token issued.</span></span>
>
>

<span data-ttu-id="a3b0d-192">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-192">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="a3b0d-193">Telepítse a hello Passport `passport-azure-ad` moduljának hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-193">Install hello Passport `passport-azure-ad` module using hello following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="a3b0d-194">hello parancs kimenetében hello hasonló toothis szöveget kell lennie:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-194">hello output of hello command should be similar toothis text:</span></span>

``
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
``

## <a name="add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="a3b0d-195">Adja hozzá a MongoDB modulok tooyour webes API</span><span class="sxs-lookup"><span data-stu-id="a3b0d-195">Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="a3b0d-196">Ebben a példában a MongoDB lesz az adattároló.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="a3b0d-197">Ezért telepítenie kell a népszerű Mongoose bővítményt, amely modellek és sémák kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="a3b0d-198">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="a3b0d-198">Install additional modules</span></span>
<span data-ttu-id="a3b0d-199">Következő lépésként telepítse a szükséges modulokat fennmaradó hello.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-199">Next, install hello remaining required modules.</span></span>

<span data-ttu-id="a3b0d-200">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-200">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-201">Hello modulok telepítése a `node_modules` könyvtár:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-201">Install hello modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="a3b0d-202">A függőségeknek megfelelő server.js fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="a3b0d-203">Hello `server.js` fájl hello többsége hello funkciókat biztosít a webes API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-203">hello `server.js` file provides hello majority of hello functionality for your Web API server.</span></span>

<span data-ttu-id="a3b0d-204">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-204">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-205">Hozza létre a `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="a3b0d-206">Adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-206">Add hello following information:</span></span>

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

<span data-ttu-id="a3b0d-207">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-207">Save hello file.</span></span> <span data-ttu-id="a3b0d-208">Később visszatérhet tooit.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-208">You return tooit later.</span></span>

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="a3b0d-209">A config.js fájl toostore létrehozása az Azure AD-beállítások</span><span class="sxs-lookup"><span data-stu-id="a3b0d-209">Create a config.js file toostore your Azure AD settings</span></span>
<span data-ttu-id="a3b0d-210">Ez a kódfájl adja át a hello konfigurációs paraméterek, az az Azure AD portálon toohello `Passport.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-210">This code file passes hello configuration parameters from your Azure AD Portal toohello `Passport.js` file.</span></span> <span data-ttu-id="a3b0d-211">Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello segédlet első része hello való hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-211">You created these configuration values when you added hello web API toohello portal in hello first part of hello walk-through.</span></span> <span data-ttu-id="a3b0d-212">Milyen tooput hello értékeket a következő paraméterek közül azt ismertetik, hello kód másolását követően.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-212">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

<span data-ttu-id="a3b0d-213">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-213">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-214">Hozza létre a `config.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="a3b0d-215">Adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-215">Add hello following information:</span></span>

```Javascript
// Don't commit this file tooyour public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in hello portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // hello Client ID of hello application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add hello B2C tenant name in hello <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is hello policy you'll want toovalidate against in B2C. Usually this is your Sign-in policy (as users sign in toothis API)
passReqToCallback: false // This is a node.js construct that lets you pass hello req all hello way back tooany upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a><span data-ttu-id="a3b0d-216">Kötelező értékek</span><span class="sxs-lookup"><span data-stu-id="a3b0d-216">Required values</span></span>
<span data-ttu-id="a3b0d-217">`clientID`: a webes API-alkalmazás Ügyfélazonosítója hello.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-217">`clientID`: hello client ID of your Web API application.</span></span>

<span data-ttu-id="a3b0d-218">`IdentityMetadata`: Ez akkor, ha `passport-azure-ad` keresi a hello identitásszolgáltató konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for hello identity provider.</span></span> <span data-ttu-id="a3b0d-219">Hello kulcsok toovalidate hello JSON webes jogkivonatainak is keres.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-219">It also looks for hello keys toovalidate hello JSON web tokens.</span></span>

<span data-ttu-id="a3b0d-220">`audience`: hello egységes erőforrás-azonosítója (URI), amely azonosítja a hívó alkalmazás hello portálról.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-220">`audience`: hello uniform resource identifier (URI) from hello portal that identifies your calling application.</span></span>

<span data-ttu-id="a3b0d-221">`tenantName`: A bérlő neve (például **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="a3b0d-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="a3b0d-222">`policyName`: hello tooyour server érkező toovalidate hello jogkivonatok beállítani kívánt szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-222">`policyName`: hello policy that you want toovalidate hello tokens coming in tooyour server.</span></span> <span data-ttu-id="a3b0d-223">Ezzel a házirend-érdemes lehet ugyanabban a házirendben a hello ügyfélalkalmazás a használt bejelentkezési hello.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-223">This policy should be hello same policy that you use on hello client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="a3b0d-224">Most használja hello azonos házirendek közötti ügyfél és a kiszolgáló beállítása.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-224">For now, use hello same policies across both client and server setup.</span></span> <span data-ttu-id="a3b0d-225">Ha már oktatóanyagok keretében létrehozta ezeket a szabályzatokat, ezért újra toodo nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-225">If you have already completed a walk-through and created these policies, you don't need toodo so again.</span></span> <span data-ttu-id="a3b0d-226">Hello segédlet fejeződött be, mert nem szükséges új szabályzatokat tooset az ügyfelekre hello helyen.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-226">Because you completed hello walk-through, you shouldn't need tooset up new policies for client walk-throughs on hello site.</span></span>
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a><span data-ttu-id="a3b0d-227">Konfigurációs tooyour server.js fájl felvétele</span><span class="sxs-lookup"><span data-stu-id="a3b0d-227">Add configuration tooyour server.js file</span></span>
<span data-ttu-id="a3b0d-228">hello tooread hello értékeinek `config.js` létrehozott fájlt, adja hozzá a hello `.config` fájlt kötelező erőforrásként az alkalmazásban, és utána állítsa be a globális változók toothose hello hello `config.js` dokumentum.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-228">tooread hello values from hello `config.js` file you created, add hello `.config` file as a required resource in your application, and then set hello global variables toothose in hello `config.js` document.</span></span>

<span data-ttu-id="a3b0d-229">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-229">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-230">Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-230">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="a3b0d-231">Adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-231">Add hello following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="a3b0d-232">Adja hozzá egy új szakaszt túl`server.js` , amely tartalmazza a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-232">Add a new section too`server.js` that includes hello following code:</span></span>

```Javascript
// We pass these options in toohello ODICBearerStrategy.

var options = {
    // hello URL of hello metadata document for your app. We put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

<span data-ttu-id="a3b0d-233">A következő adjunk néhány helyőrzőket a hívó alkalmazásból érkező hello felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-233">Next, let's add some placeholders for hello users we receive from our calling applications.</span></span>

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="a3b0d-234">Most pedig hozzon létre naplózót.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="a3b0d-235">Hello MongoDB modell és séma-információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="a3b0d-235">Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="a3b0d-236">hello korábbi előkészítő fizet nevén, ezek a fájlok együtt a REST API-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-236">hello earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="a3b0d-237">Ez az útmutató használja MongoDB toostore a feladatokat, a fentiekben taglaltak.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-237">For this walk-through, use MongoDB toostore your tasks, as discussed earlier.</span></span>

<span data-ttu-id="a3b0d-238">A hello `config.js` az adatbázis nevű fájl, **tasklist**.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-238">In hello `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="a3b0d-239">Ugyanez a neve egyben hello hello végén put `mongoose_auth_local` kapcsolat URL-címet.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-239">That name was also what you put at hello end of hello `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="a3b0d-240">Ez az adatbázis előre a MongoDB-ben nem kell toocreate.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-240">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="a3b0d-241">Az adatbázist hoz létre hello meg hello első futtatáskor a kiszolgálói alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-241">It creates hello database for you on hello first run of your server application.</span></span>

<span data-ttu-id="a3b0d-242">Miután Ön hello server melyik MongoDB adatbázis toouse, szükség van toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-242">After you tell hello server which MongoDB database toouse, you need toowrite some additional code toocreate hello model and schema for your server tasks.</span></span>

### <a name="expand-hello-model"></a><span data-ttu-id="a3b0d-243">Bontsa ki a hello modell</span><span class="sxs-lookup"><span data-stu-id="a3b0d-243">Expand hello model</span></span>
<span data-ttu-id="a3b0d-244">Ez a sémamodell egyszerű.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-244">This schema model is simple.</span></span> <span data-ttu-id="a3b0d-245">Ez azt is jelenti, hogy szükség esetén könnyedén kibővíthető.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-245">You can expand it as required.</span></span>

<span data-ttu-id="a3b0d-246">`owner`: Toohello feladat ki hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-246">`owner`: Who is assigned toohello task.</span></span> <span data-ttu-id="a3b0d-247">Ez egy **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-247">This object is a **string**.</span></span>  

<span data-ttu-id="a3b0d-248">`Text`: maga hello feladat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-248">`Text`: hello task itself.</span></span> <span data-ttu-id="a3b0d-249">Ez egy **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-249">This object is a **string**.</span></span>

<span data-ttu-id="a3b0d-250">`date`: hello dátum hello tevékenység esedékessé válik.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-250">`date`: hello date that hello task is due.</span></span> <span data-ttu-id="a3b0d-251">Ez egy **datetime** (dátum és idő) típusú objektum.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-251">This object is a **datetime**.</span></span>

<span data-ttu-id="a3b0d-252">`completed`: Ha hello feladat már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-252">`completed`: If hello task is complete.</span></span> <span data-ttu-id="a3b0d-253">Ez egy **logikai** típusú objektum.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-253">This object is a **Boolean**.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="a3b0d-254">Hozzon létre hello séma hello kódban</span><span class="sxs-lookup"><span data-stu-id="a3b0d-254">Create hello schema in hello code</span></span>
<span data-ttu-id="a3b0d-255">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-255">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-256">Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-256">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="a3b0d-257">Adja hozzá a következő információ hello konfigurációs bejegyzés alatt hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-257">Add hello following information below hello configuration entry:</span></span>

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect tooMongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema toostore our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use hello schema tooregister a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
<span data-ttu-id="a3b0d-258">Először létre kell hoznia hello séma, és majd a modellobjektumot, amelyet használ az adatok teljes hello code definiálásakor toostore hozza létre a **útvonalak**.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-258">You first create hello schema, and then you create a model object that you use toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="a3b0d-259">A REST API-feladatkiszolgálóra mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="a3b0d-260">Most, hogy egy adatbázis-modell toowork rendelkező, adja hozzá a hello útvonalakat használ a REST API-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-260">Now that you have a database model toowork with, add hello routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="a3b0d-261">Az útvonalak működése a Restify programban</span><span class="sxs-lookup"><span data-stu-id="a3b0d-261">About routes in Restify</span></span>
<span data-ttu-id="a3b0d-262">Útvonalak használhatók a Restifyban hello azonos úgy, hogy működnek-hello Express verem használata.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-262">Routes work in Restify in hello same way that they work when they use hello Express stack.</span></span> <span data-ttu-id="a3b0d-263">Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-263">You define routes by using hello URI that you expect hello client applications toocall.</span></span>

<span data-ttu-id="a3b0d-264">Íme egy jellegzetes Restify-útvonal:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-264">A typical pattern for a Restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep hello server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

<span data-ttu-id="a3b0d-265">A Restify és az Express ennél jóval összetettebb funkciók biztosítására is képes: például definiálhatja velük az alkalmazástípusokat, valamint bonyolult útvonalvezetést valósíthat meg a különböző végpontok között.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="a3b0d-266">Ez az oktatóanyag hello alkalmazásában azt megőrizni ezeket az útvonalakat egyszerű.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-266">For hello purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="a3b0d-267">Alapértelmezett útvonalak tooyour kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-267">Add default routes tooyour server</span></span>
<span data-ttu-id="a3b0d-268">Mostantól felvehet hello alapvető CRUD-útvonalakat: **létrehozása** és **lista** a REST API-n.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-268">You now add hello basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="a3b0d-269">Más útvonalak hello található `complete` hello minta ága.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-269">Other routes can be found in hello `complete` branch of hello sample.</span></span>

<span data-ttu-id="a3b0d-270">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-270">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="a3b0d-271">Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-271">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="a3b0d-272">Hello adatbáziselemek alatt végrehajtott újabb adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-272">Below hello database entries you made above add hello following information:</span></span>

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it tooMongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

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
            log.warn(err, "There is no tasks in hello database. Add one!");
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


#### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="a3b0d-273">Hibakezelés hello útvonalak beállítása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-273">Add error handling for hello routes</span></span>
<span data-ttu-id="a3b0d-274">Adjon hozzá néhány hibakezelés, így képes kommunikálni a hátsó toohello ügyfél úgy, hogy az képes megérteni felmerülő problémákat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-274">Add some error handling so that you can communicate any problems you encounter back toohello client in a way that it can understand.</span></span>

<span data-ttu-id="a3b0d-275">Adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-275">Add hello following code:</span></span>

```Javascript
///--- Errors for communicating something interesting back toohello client
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


## <a name="create-your-server"></a><span data-ttu-id="a3b0d-276">A kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-276">Create your server</span></span>
<span data-ttu-id="a3b0d-277">Készen áll az adatbázis, és az útvonalak is a helyükön vannak.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="a3b0d-278">utolsó lépésként hello meg toodo tooadd hello server-példány a hívásokat kezelő.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-278">hello last thing for you toodo is tooadd hello server instance that manages your calls.</span></span>

<span data-ttu-id="a3b0d-279">A restify és Express adja meg a részletes testreszabási egy REST API-kiszolgálóhoz, de itt használjuk hello most az alapszintű beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-279">Restify and Express provide deep customization for a REST API server, but we use hello most basic setup here.</span></span>

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst too10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping tooREST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-hello-routes-toohello-server-without-authentication"></a><span data-ttu-id="a3b0d-280">Adja hozzá a hello útvonalak toohello kiszolgálóhoz (hitelesítés nélkül)</span><span class="sxs-lookup"><span data-stu-id="a3b0d-280">Add hello routes toohello server (without authentication)</span></span>
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser too%s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="a3b0d-281">Hitelesítési tooyour REST API-kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a3b0d-281">Add authentication tooyour REST API server</span></span>
<span data-ttu-id="a3b0d-282">Most, hogy rendelkezésére áll a futó REST API-kiszolgáló, felhasználhatja azt az Azure AD-ben is.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="a3b0d-283">Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-283">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="a3b0d-284">Hello részét passport-azure-ad részét képező OIDCBearerStrategy használata</span><span class="sxs-lookup"><span data-stu-id="a3b0d-284">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="a3b0d-285">Amikor ír API-k, mindig csatolja hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-285">When you write APIs, you should always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="a3b0d-286">Hello kiszolgáló tárolja a ToDo elemeket hajtja hello alapján **oid** hello felhasználó hello jogkivonatot (a token.OID), amely a "hello"owner"mezőjében.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-286">When hello server stores ToDo items, it does so based on hello **oid** of hello user in hello token (called through token.oid), which goes in hello “owner” field.</span></span> <span data-ttu-id="a3b0d-287">Ez az érték biztosítja, hogy csak az adott felhasználó férhet hozzá a saját ToDo elemeihez.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="a3b0d-288">Nincs nem jelenik meg sehol hello API "tulajdonos", a külső felhasználó kérhet mások ToDo elemeit, még akkor is, ha hitelesített.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-288">There is no exposure in hello API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="a3b0d-289">Következő lépésként az hello tulajdonosi stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-289">Next, use hello bearer strategy that comes with `passport-azure-ad`.</span></span>

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying hello user');
        log.info(token, 'was hello token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

<span data-ttu-id="a3b0d-290">A Passport a stratégiákhoz hello azonos mintát használ.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-290">Passport uses hello same pattern for all its strategies.</span></span> <span data-ttu-id="a3b0d-291">Át kell neki adni egy `function()` elemet, amelynek paramétereihez tartozik a `token` és a `done`.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="a3b0d-292">hello stratégia ismét elérhető lesz tooyou után fordult elő az összes teendőit.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-292">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="a3b0d-293">Kell majd hello felhasználói tároljuk, és mentse a hello jogkivonat, így nem kell tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-293">You should then store hello user and save hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3b0d-294">hello a fenti kódrészlettel áteső felhasználók bejuthatnak tooauthenticate tooyour kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-294">hello code above takes any user who happens tooauthenticate tooyour server.</span></span> <span data-ttu-id="a3b0d-295">Ezt a folyamatot automatikus regisztrációnak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-295">This process is known as autoregistration.</span></span> <span data-ttu-id="a3b0d-296">Az üzemi kiszolgálók nem lehetővé teszik a felhasználók hozzáférést hello API-k nélkül nyissa meg a regisztrációs folyamat őket.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-296">In production servers, don't let in any users access hello API without first having them go through a registration process.</span></span> <span data-ttu-id="a3b0d-297">Ez a folyamat az általában hello megoldást látjuk a fogyasztói alkalmazásoknál, amelyek lehetővé teszik tooregister Facebook-fiókkal, majd megkérdezi, hogy toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-297">This process is usually hello pattern you see in consumer apps that allow you tooregister by using Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="a3b0d-298">Ha a program nem parancssori program, kinyerhettük volna hello e-mailt a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna a felhasználók toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-298">If this program wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked users toofill out additional information.</span></span> <span data-ttu-id="a3b0d-299">Mivel ez egy minta, azt hozzá tooan memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-299">Because this is a sample, we add them tooan in-memory database.</span></span>
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a><span data-ttu-id="a3b0d-300">A server application tooverify futtassa, hogy elutasítja-e Önt</span><span class="sxs-lookup"><span data-stu-id="a3b0d-300">Run your server application tooverify that it rejects you</span></span>
<span data-ttu-id="a3b0d-301">Használhat `curl` toosee, ha már rendelkezik az OAuth2-védelem e a végpontokon.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-301">You can use `curl` toosee if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="a3b0d-302">hello visszaadott fejlécek legyen elég tootell, hogy a hello helyes elérési útjával.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-302">hello headers returned should be enough tootell you that you are on hello right path.</span></span>

<span data-ttu-id="a3b0d-303">Ellenőrizze, hogy fut-e a MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="a3b0d-304">Toohello könyvtár és futtatási hello kiszolgáló módosítása:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-304">Change toohello directory and run hello server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="a3b0d-305">Egy új terminálablakban futtassa a `curl` parancsot</span><span class="sxs-lookup"><span data-stu-id="a3b0d-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="a3b0d-306">Próbálkozzon meg egy egyszerű POST művelettel:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="a3b0d-307">A 401-es hiba: hello jelenjen meg.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-307">A 401 error is hello response you want.</span></span> <span data-ttu-id="a3b0d-308">Azt jelzi, hogy hello Passport réteg próbál tooredirect toohello végpont hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-308">It indicates that hello Passport layer is trying tooredirect toohello authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="a3b0d-309">Az OAuth2-t használó REST API-szolgáltatás ezzel elkészült</span><span class="sxs-lookup"><span data-stu-id="a3b0d-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="a3b0d-310">A Restify és az OAuth használatával elkészítette a REST API-t!</span><span class="sxs-lookup"><span data-stu-id="a3b0d-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="a3b0d-311">Már megfelelő kódrészletek, hogy toodevelop a szolgáltatás folytatáshoz és létrehozása az ebben a példában-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-311">You now have sufficient code so that you can continue toodevelop your service and build on this example.</span></span> <span data-ttu-id="a3b0d-312">Megtettünk mindent, ami a kiszolgálón OAuth2-kompatibilis ügyfél nélkül lehetséges volt.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="a3b0d-313">A következő lépéshez használja a továbbiakhoz újabb oktatóanyagokat, például a [iOS B2C használatával tooa webes API kapcsolódnak](active-directory-b2c-devquickstarts-ios.md) forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="a3b0d-313">For that next step use an additional walk-through like our [Connect tooa web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3b0d-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3b0d-314">Next steps</span></span>
<span data-ttu-id="a3b0d-315">Speciális toomore témakörök, most már továbbléphet, mint:</span><span class="sxs-lookup"><span data-stu-id="a3b0d-315">You can now move toomore advanced topics, such as:</span></span>

[<span data-ttu-id="a3b0d-316">Csatlakozás tooa webes API iOS B2C használatával</span><span class="sxs-lookup"><span data-stu-id="a3b0d-316">Connect tooa web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
