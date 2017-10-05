---
title: "Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével | Microsoft Docs"
description: "Node.js webes API létrehozása, amely fogadja a B2C-bérlők által küldött jogkivonatokat"
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
ms.openlocfilehash: 6480be75c314ede1b786e959a79c0385dd2edea8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="0b56e-103">Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével</span><span class="sxs-lookup"><span data-stu-id="0b56e-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="0b56e-104">Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="0b56e-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="0b56e-105">Ezek a jogkivonatok engedélyezik az Azure AD B2C-t alkalmazó ügyfélalkalmazások számára az API hitelesítését.</span><span class="sxs-lookup"><span data-stu-id="0b56e-105">These tokens allow your client apps that use Azure AD B2C to authenticate to the API.</span></span> <span data-ttu-id="0b56e-106">Ebből a cikkből megtudhatja, hogyan hozhat létre egy olyan „feladatlista” API-t, amelyben a felhasználók megjeleníthetik a listát és új feladatokat adhatnak hozzá.</span><span class="sxs-lookup"><span data-stu-id="0b56e-106">This article shows you how to create a "to-do list" API that allows users to add and list tasks.</span></span> <span data-ttu-id="0b56e-107">A webes API-t az Azure AD B2C látja el védelemmel, így csak a hitelesített felhasználók kezelhetik a feladatlistájukat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="0b56e-108">Ezt a mintát úgy írtuk meg, hogy az [iOS B2C mintaalkalmazással](active-directory-b2c-devquickstarts-ios.md) lehessen hozzá csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-108">This sample was written to be connected to by using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="0b56e-109">Először haladjon végig ezen az ismertetőn, és aztán térjen rá az említett mintára.</span><span class="sxs-lookup"><span data-stu-id="0b56e-109">Do the current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="0b56e-110">A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="0b56e-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="0b56e-111">A rugalmasan működő, moduláris Passport gyakorlatilag bármely Express- vagy Restify-alapú webalkalmazásba diszkréten telepíthető.</span><span class="sxs-lookup"><span data-stu-id="0b56e-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="0b56e-112">A program számos különböző lehetőséget kínál a felhasználók hitelesítésére: felhasználónév/jelszó, Facebook- vagy Twitter-fiók és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0b56e-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="0b56e-113">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure Active Directory (Azure AD) esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="0b56e-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="0b56e-114">Először telepítenie kell a modult, majd hozzá kell adni az Azure AD `passport-azure-ad` bővítményt.</span><span class="sxs-lookup"><span data-stu-id="0b56e-114">You install this module and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="0b56e-115">Ennek a mintának az elvégzéséhez először az alábbiakat kell elvégeznie:</span><span class="sxs-lookup"><span data-stu-id="0b56e-115">To do this sample, you need to:</span></span>

1. <span data-ttu-id="0b56e-116">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="0b56e-117">Az alkalmazás beállítása a Passport `azure-ad-passport` bővítményének használatára.</span><span class="sxs-lookup"><span data-stu-id="0b56e-117">Set up your application to use Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="0b56e-118">Ügyfélalkalmazás konfigurálása a „feladatlista” webes API meghívására.</span><span class="sxs-lookup"><span data-stu-id="0b56e-118">Configure a client application to call the "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="0b56e-119">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="0b56e-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="0b56e-120">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="0b56e-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="0b56e-121">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket.</span><span class="sxs-lookup"><span data-stu-id="0b56e-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="0b56e-122">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="0b56e-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="0b56e-123">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b56e-123">Create an application</span></span>
<span data-ttu-id="0b56e-124">Következő lépésként létre kell hoznia egy alkalmazást a B2C-címtárban, amely ellátja az Azure AD-t a biztonságos kommunikációhoz szükséges információkkal.</span><span class="sxs-lookup"><span data-stu-id="0b56e-124">Next, you need to create an app in your B2C directory that gives Azure AD some information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="0b56e-125">Ebben az esetben az ügyfélalkalmazáshoz és a webes API-hoz egyetlen **alkalmazásazonosító** tartozik, mivel a két elem egyetlen logikai alkalmazás lesz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-125">In this case, both the client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="0b56e-126">Az alkalmazást a következő [utasítások](active-directory-b2c-app-registration.md) alapján hozza létre.</span><span class="sxs-lookup"><span data-stu-id="0b56e-126">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="0b56e-127">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="0b56e-127">Be sure to:</span></span>

* <span data-ttu-id="0b56e-128">Az alkalmazás tartalmazzon egy **webalkalmazást vagy webes API-t**</span><span class="sxs-lookup"><span data-stu-id="0b56e-128">Include a **web app/web api** in the application</span></span>
* <span data-ttu-id="0b56e-129">A **Reply URL** (Válasz URL-cím) legyen a következő: `http://localhost/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="0b56e-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="0b56e-130">Ez a kódminta alapértelmezett URL-címe.</span><span class="sxs-lookup"><span data-stu-id="0b56e-130">It is the default URL for this code sample.</span></span>
* <span data-ttu-id="0b56e-131">Hozzon létre egy **alkalmazástitkot** az alkalmazáshoz, majd másolja.</span><span class="sxs-lookup"><span data-stu-id="0b56e-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="0b56e-132">Erre az adatra később még szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-132">You need this data later.</span></span> <span data-ttu-id="0b56e-133">Ne feledje, hogy az értékben használat előtt [az XML-nek megfelelő feloldójelekkel](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) kell megjelölni a vezérlőkaraktereket.</span><span class="sxs-lookup"><span data-stu-id="0b56e-133">Note that this value needs to be [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="0b56e-134">Másolja az alkalmazáshoz rendelt **alkalmazásazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="0b56e-134">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="0b56e-135">Erre az adatra később még szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="0b56e-136">Házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b56e-136">Create your policies</span></span>
<span data-ttu-id="0b56e-137">Az Azure AD B2C-ben minden felhasználói élményt [házirendek](active-directory-b2c-reference-policies.md) határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="0b56e-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="0b56e-138">Ez az alkalmazás két, identitással kapcsolatos műveletet tartalmaz: regisztráció és bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="0b56e-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="0b56e-139">Mindkettőhöz létre kell hoznia egy szabályzatot a [szabályzatok áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy) leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="0b56e-139">You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="0b56e-140">A három szabályzat létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="0b56e-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="0b56e-141">A regisztrációs szabályzatban adja meg a **Megjelenített név** értékét, illetve az egyéb regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-141">Choose the **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="0b56e-142">Az összes szabályzatban válassza ki a **Megjelenített név** és az **Objektumazonosító** alkalmazási jogcímet.</span><span class="sxs-lookup"><span data-stu-id="0b56e-142">Choose the **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="0b56e-143">Ezenfelül más jogcímeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="0b56e-144">Az egyes szabályzatok létrehozását követően másolja a szabályzat **nevét**.</span><span class="sxs-lookup"><span data-stu-id="0b56e-144">Copy down the **Name** of each policy after you create it.</span></span> <span data-ttu-id="0b56e-145">A névnek a következő előtaggal kell rendelkeznie: `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="0b56e-145">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="0b56e-146">A szabályzatok nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="0b56e-147">A három szabályzat létrehozását követően készen áll az alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="0b56e-147">After you have created your three policies, you're ready to build your app.</span></span>

<span data-ttu-id="0b56e-148">Ha szeretné megismerni a szabályzatoknak az Azure AD B2C alatti működését, végezze el a [.NET-es webalkalmazások használatába bevezető oktatóanyagot](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0b56e-148">To learn about how policies work in Azure AD B2C, start with the [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-the-code"></a><span data-ttu-id="0b56e-149">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="0b56e-149">Download the code</span></span>
<span data-ttu-id="0b56e-150">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="0b56e-150">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="0b56e-151">A minta menet közbeni létrehozásához [letöltheti a projektvázát tartalmazó .zip-fájlt](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="0b56e-151">To build the sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="0b56e-152">A vázprojektet klónozhatja is:</span><span class="sxs-lookup"><span data-stu-id="0b56e-152">You can also clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="0b56e-153">A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) vagy `complete` az adott adattár ágában is elérhető.</span><span class="sxs-lookup"><span data-stu-id="0b56e-153">The completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on the `complete` branch of the same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="0b56e-154">A Node.js letöltése a platformra</span><span class="sxs-lookup"><span data-stu-id="0b56e-154">Download Node.js for your platform</span></span>
<span data-ttu-id="0b56e-155">A minta használatához rendelkeznie kell a Node.js telepített és működő verziójával.</span><span class="sxs-lookup"><span data-stu-id="0b56e-155">To successfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="0b56e-156">Telepítse a Node.js-t a [nodejs.org](http://nodejs.org) oldalról.</span><span class="sxs-lookup"><span data-stu-id="0b56e-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="0b56e-157">A MongoDB telepítése a platformra</span><span class="sxs-lookup"><span data-stu-id="0b56e-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="0b56e-158">A minta használatához rendelkeznie kell a MongoDB telepített és működő verziójával.</span><span class="sxs-lookup"><span data-stu-id="0b56e-158">To successfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="0b56e-159">A MongoDB-re a REST API különböző kiszolgálópéldányok közötti perzisztenciájának kialakításához van szükség.</span><span class="sxs-lookup"><span data-stu-id="0b56e-159">We use MongoDB to make your REST API persistent across server instances.</span></span>

<span data-ttu-id="0b56e-160">Telepítse a MongoDB-t a [mongodb.org](http://www.mongodb.org) oldalról.</span><span class="sxs-lookup"><span data-stu-id="0b56e-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="0b56e-161">Ebben az útmutatóban feltételezzük, hogy Ön a MongoDB alapértelmezett telepítését és kiszolgálóvégpontjait használja, amely a cikk írásának időpontjában a következő: `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="0b56e-161">This walk-through assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="0b56e-162">A Restify-modulok telepítése a webes API-ra.</span><span class="sxs-lookup"><span data-stu-id="0b56e-162">Install the Restify modules in your web API</span></span>
<span data-ttu-id="0b56e-163">A REST API létrehozásához a Restify programot használjuk.</span><span class="sxs-lookup"><span data-stu-id="0b56e-163">We use Restify to build your REST API.</span></span> <span data-ttu-id="0b56e-164">A Restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer, amely az Express alapján készült.</span><span class="sxs-lookup"><span data-stu-id="0b56e-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="0b56e-165">Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0b56e-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="0b56e-166">A Restify telepítése</span><span class="sxs-lookup"><span data-stu-id="0b56e-166">Install Restify</span></span>
<span data-ttu-id="0b56e-167">A parancssorban módosítsa a következőre a könyvtárat: `azuread`.</span><span class="sxs-lookup"><span data-stu-id="0b56e-167">From the command line, change your directory to `azuread`.</span></span> <span data-ttu-id="0b56e-168">Ha az `azuread` könyvtár nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="0b56e-168">If the `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="0b56e-169">`cd azuread` vagy `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="0b56e-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="0b56e-170">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0b56e-170">Enter the following command:</span></span>

`npm install restify`

<span data-ttu-id="0b56e-171">Ez a parancs elvégzi a Restify telepítését.</span><span class="sxs-lookup"><span data-stu-id="0b56e-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="0b56e-172">Hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="0b56e-172">Did you get an error?</span></span>
<span data-ttu-id="0b56e-173">Egyes operációs rendszereken az `npm` parancs használata esetén az `Error: EPERM, chmod '/usr/local/bin/..'` szövegű hiba jelenik meg, és a rendszer felszólítja, hogy futtassa rendszergazdaként a fiókot.</span><span class="sxs-lookup"><span data-stu-id="0b56e-173">In some operating systems, when you use `npm`, you may receive the error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run the account as an administrator.</span></span> <span data-ttu-id="0b56e-174">Ha ez a probléma jelentkezik, a `sudo` parancs használatával futtassa magasabb jogosultsági szinten az `npm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="0b56e-174">If this problem occurs, use the `sudo` command to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="0b56e-175">DTrace hibaüzenetet kapott?</span><span class="sxs-lookup"><span data-stu-id="0b56e-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="0b56e-176">Előfordulhat, hogy a Restify telepítése során az alábbihoz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0b56e-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="0b56e-177">A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="0b56e-178">Számos operációs rendszerben azonban nem található meg a DTrace.</span><span class="sxs-lookup"><span data-stu-id="0b56e-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="0b56e-179">Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="0b56e-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="0b56e-180">A parancs kimenetének a következőképp kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0b56e-180">The output of the command should appear similar to this text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="0b56e-181">A Passport telepítése a webes API-ba</span><span class="sxs-lookup"><span data-stu-id="0b56e-181">Install Passport in your web API</span></span>
<span data-ttu-id="0b56e-182">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van).</span><span class="sxs-lookup"><span data-stu-id="0b56e-182">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="0b56e-183">A Passport telepítéséhez használja az alábbi parancsot :</span><span class="sxs-lookup"><span data-stu-id="0b56e-183">Install Passport using the following command:</span></span>

`npm install passport`

<span data-ttu-id="0b56e-184">A parancs kimenetének a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0b56e-184">The output of the command should be similar to this text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a><span data-ttu-id="0b56e-185">A passport-azuread hozzáadása a webes API-hoz</span><span class="sxs-lookup"><span data-stu-id="0b56e-185">Add passport-azuread to your web API</span></span>
<span data-ttu-id="0b56e-186">Következő lépésként a `passport-azuread` használatával adja hozzá az OAuth stratégiát. Ez a stratégiacsomag szolgál az Azure AD és a Passport összekapcsolására.</span><span class="sxs-lookup"><span data-stu-id="0b56e-186">Next, add the OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="0b56e-187">A REST API-mintában ez a stratégia használható a tulajdonosi jogkivonatokhoz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-187">Use this strategy for bearer tokens in the REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="0b56e-188">Az OAuth2 keretrendszerében ugyan bármely ismert jogkivonat kibocsátható, csupán néhány típust használnak szélesebb körben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="0b56e-189">A végpontok védelmére szolgáló jogkivonatokat tulajdonosi jogkivonatoknak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="0b56e-189">The tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="0b56e-190">Az OAuth2-ben ezek a leggyakrabban kibocsátott jogkivonattípusok.</span><span class="sxs-lookup"><span data-stu-id="0b56e-190">These types of tokens are the most widely issued in OAuth2.</span></span> <span data-ttu-id="0b56e-191">Számos implementáció eleve úgy veszi, hogy csak tulajdonosi jogkivonatokat bocsátottak ki.</span><span class="sxs-lookup"><span data-stu-id="0b56e-191">Many implementations assume that bearer tokens are the only type of token issued.</span></span>
>
>

<span data-ttu-id="0b56e-192">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van).</span><span class="sxs-lookup"><span data-stu-id="0b56e-192">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="0b56e-193">Telepítse a `passport-azure-ad` Passport-modult az alábbi paranccsal:</span><span class="sxs-lookup"><span data-stu-id="0b56e-193">Install the Passport `passport-azure-ad` module using the following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="0b56e-194">A parancs kimenetének a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0b56e-194">The output of the command should be similar to this text:</span></span>

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

## <a name="add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="0b56e-195">MongoDB-modulok hozzáadása a webes API-hoz</span><span class="sxs-lookup"><span data-stu-id="0b56e-195">Add MongoDB modules to your web API</span></span>
<span data-ttu-id="0b56e-196">Ebben a példában a MongoDB lesz az adattároló.</span><span class="sxs-lookup"><span data-stu-id="0b56e-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="0b56e-197">Ezért telepítenie kell a népszerű Mongoose bővítményt, amely modellek és sémák kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="0b56e-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="0b56e-198">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="0b56e-198">Install additional modules</span></span>
<span data-ttu-id="0b56e-199">Következő lépésként telepítse a további szükséges modulokat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-199">Next, install the remaining required modules.</span></span>

<span data-ttu-id="0b56e-200">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-200">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-201">Telepítse a modulokat a `node_modules` könyvtárba:</span><span class="sxs-lookup"><span data-stu-id="0b56e-201">Install the modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="0b56e-202">A függőségeknek megfelelő server.js fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b56e-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="0b56e-203">A `server.js` fájl biztosítja a funkciók legnagyobb részét a webes API kiszolgálója számára.</span><span class="sxs-lookup"><span data-stu-id="0b56e-203">The `server.js` file provides the majority of the functionality for your Web API server.</span></span>

<span data-ttu-id="0b56e-204">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-204">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-205">Hozza létre a `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="0b56e-206">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="0b56e-206">Add the following information:</span></span>

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

<span data-ttu-id="0b56e-207">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="0b56e-207">Save the file.</span></span> <span data-ttu-id="0b56e-208">Később még visszatérünk rá.</span><span class="sxs-lookup"><span data-stu-id="0b56e-208">You return to it later.</span></span>

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="0b56e-209">Az Azure AD-beállításokat tároló config.js fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b56e-209">Create a config.js file to store your Azure AD settings</span></span>
<span data-ttu-id="0b56e-210">Ez a kódfájl adja át a konfigurációs paramétereket az Azure AD-portálból a `Passport.js` fájlnak.</span><span class="sxs-lookup"><span data-stu-id="0b56e-210">This code file passes the configuration parameters from your Azure AD Portal to the `Passport.js` file.</span></span> <span data-ttu-id="0b56e-211">Ezeket a konfigurációs értékeket akkor hozta létre, amikor az útmutató korábbi részében hozzáadta a webes API-t a portálhoz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-211">You created these configuration values when you added the web API to the portal in the first part of the walk-through.</span></span> <span data-ttu-id="0b56e-212">A paraméterek kitöltésének módját a kód másolását követően ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="0b56e-212">We explain what to put in the values of these parameters after you copy the code.</span></span>

<span data-ttu-id="0b56e-213">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-213">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-214">Hozza létre a `config.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="0b56e-215">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="0b56e-215">Add the following information:</span></span>

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a><span data-ttu-id="0b56e-216">Kötelező értékek</span><span class="sxs-lookup"><span data-stu-id="0b56e-216">Required values</span></span>
<span data-ttu-id="0b56e-217">`clientID`: A webes API-alkalmazás ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0b56e-217">`clientID`: The client ID of your Web API application.</span></span>

<span data-ttu-id="0b56e-218">`IdentityMetadata`: a `passport-azure-ad` itt fogja keresni az identitásszolgáltatóra vonatkozó konfigurációs adatokat,</span><span class="sxs-lookup"><span data-stu-id="0b56e-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for the identity provider.</span></span> <span data-ttu-id="0b56e-219">továbbá a JSON webes jogkivonatainak érvényesítéséhez szükséges kulcsokat is.</span><span class="sxs-lookup"><span data-stu-id="0b56e-219">It also looks for the keys to validate the JSON web tokens.</span></span>

<span data-ttu-id="0b56e-220">`audience`: A hívást indító alkalmazást azonosító portál URI-ja.</span><span class="sxs-lookup"><span data-stu-id="0b56e-220">`audience`: The uniform resource identifier (URI) from the portal that identifies your calling application.</span></span>

<span data-ttu-id="0b56e-221">`tenantName`: A bérlő neve (például **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="0b56e-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="0b56e-222">`policyName`: A kiszolgálóra érkező jogkivonatok érvényesítésére használni kívánt szabályzat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-222">`policyName`: The policy that you want to validate the tokens coming in to your server.</span></span> <span data-ttu-id="0b56e-223">Ez ugyanaz a szabályzat legyen, mint amelyet az ügyfélalkalmazásban használ a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="0b56e-223">This policy should be the same policy that you use on the client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="0b56e-224">Egyelőre az ügyfél és a kiszolgáló beállítása során használja ugyanazokat a házirendeket.</span><span class="sxs-lookup"><span data-stu-id="0b56e-224">For now, use the same policies across both client and server setup.</span></span> <span data-ttu-id="0b56e-225">Ha már más oktatóanyagok keretében létrehozta ezeket a szabályzatokat, nem szükséges ismét létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="0b56e-225">If you have already completed a walk-through and created these policies, you don't need to do so again.</span></span> <span data-ttu-id="0b56e-226">Mivel korábban már elvégezte az előírt lépéseket, nem szükséges új szabályzatokat létrehozni a webhelyen az ügyfelekre vonatkozó útmutatás részeként.</span><span class="sxs-lookup"><span data-stu-id="0b56e-226">Because you completed the walk-through, you shouldn't need to set up new policies for client walk-throughs on the site.</span></span>
>
>

## <a name="add-configuration-to-your-serverjs-file"></a><span data-ttu-id="0b56e-227">Konfiguráció hozzáadása a server.js fájlhoz</span><span class="sxs-lookup"><span data-stu-id="0b56e-227">Add configuration to your server.js file</span></span>
<span data-ttu-id="0b56e-228">A létrehozott `config.js` fájl értékeinek kiolvasásához kötelező erőforrásként adja hozzá a `.config` fájlt az alkalmazáshoz, majd állítsa a globális változókat a `config.js` dokumentumban megadott értékekre.</span><span class="sxs-lookup"><span data-stu-id="0b56e-228">To read the values from the `config.js` file you created, add the `.config` file as a required resource in your application, and then set the global variables to those in the `config.js` document.</span></span>

<span data-ttu-id="0b56e-229">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-229">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-230">Nyissa meg a `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-230">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="0b56e-231">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="0b56e-231">Add the following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="0b56e-232">Adjon egy új szakaszt a `server.js` fájlhoz. A szakasz a következő kódot tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0b56e-232">Add a new section to `server.js` that includes the following code:</span></span>

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

<span data-ttu-id="0b56e-233">A következő lépésben hozzon létre helyőrzőket a hívó alkalmazásból érkező felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="0b56e-233">Next, let's add some placeholders for the users we receive from our calling applications.</span></span>

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="0b56e-234">Most pedig hozzon létre naplózót.</span><span class="sxs-lookup"><span data-stu-id="0b56e-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="0b56e-235">A MongoDB-modellre és -sémára vonatkozó információk hozzáadása a Mongoose segítségével</span><span class="sxs-lookup"><span data-stu-id="0b56e-235">Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="0b56e-236">Itt érik be a korábban elvégzett munka gyümölcse: az elkészített három fájlt most összehozzuk egy REST API-szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="0b56e-236">The earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="0b56e-237">Az itt olvasható lépéseknél (ahogy arra már korábban utaltunk) a MongoDB-t használja a feladatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="0b56e-237">For this walk-through, use MongoDB to store your tasks, as discussed earlier.</span></span>

<span data-ttu-id="0b56e-238">A `config.js` fájlban az adatbázis a **tasklist** nevet kapta.</span><span class="sxs-lookup"><span data-stu-id="0b56e-238">In the `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="0b56e-239">Ez az név került a `mongoose_auth_local` csatlakozási URL-cím végére is.</span><span class="sxs-lookup"><span data-stu-id="0b56e-239">That name was also what you put at the end of the `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="0b56e-240">Ezt az adatbázist nem szükséges előre létrehozni a MongoDB-ben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-240">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="0b56e-241">Az adatbázist az kiszolgálóalkalmazás az első futtatásakor létrehozza.</span><span class="sxs-lookup"><span data-stu-id="0b56e-241">It creates the database for you on the first run of your server application.</span></span>

<span data-ttu-id="0b56e-242">Miután közli a kiszolgálóval, hogy melyik MongoDB-adatbázist használja, a kiszolgálói feladatokhoz tartozó modell és séma létrehozásához további kódot kell írnia.</span><span class="sxs-lookup"><span data-stu-id="0b56e-242">After you tell the server which MongoDB database to use, you need to write some additional code to create the model and schema for your server tasks.</span></span>

### <a name="expand-the-model"></a><span data-ttu-id="0b56e-243">A modell kibővítése</span><span class="sxs-lookup"><span data-stu-id="0b56e-243">Expand the model</span></span>
<span data-ttu-id="0b56e-244">Ez a sémamodell egyszerű.</span><span class="sxs-lookup"><span data-stu-id="0b56e-244">This schema model is simple.</span></span> <span data-ttu-id="0b56e-245">Ez azt is jelenti, hogy szükség esetén könnyedén kibővíthető.</span><span class="sxs-lookup"><span data-stu-id="0b56e-245">You can expand it as required.</span></span>

<span data-ttu-id="0b56e-246">`owner`: A feladathoz rendelt személy.</span><span class="sxs-lookup"><span data-stu-id="0b56e-246">`owner`: Who is assigned to the task.</span></span> <span data-ttu-id="0b56e-247">Ez egy **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="0b56e-247">This object is a **string**.</span></span>  

<span data-ttu-id="0b56e-248">`Text`: Maga a feladat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-248">`Text`: The task itself.</span></span> <span data-ttu-id="0b56e-249">Ez egy **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="0b56e-249">This object is a **string**.</span></span>

<span data-ttu-id="0b56e-250">`date`: A feladat határideje.</span><span class="sxs-lookup"><span data-stu-id="0b56e-250">`date`: The date that the task is due.</span></span> <span data-ttu-id="0b56e-251">Ez egy **datetime** (dátum és idő) típusú objektum.</span><span class="sxs-lookup"><span data-stu-id="0b56e-251">This object is a **datetime**.</span></span>

<span data-ttu-id="0b56e-252">`completed`: Azt jelzi, hogy a feladat elkészült-e.</span><span class="sxs-lookup"><span data-stu-id="0b56e-252">`completed`: If the task is complete.</span></span> <span data-ttu-id="0b56e-253">Ez egy **logikai** típusú objektum.</span><span class="sxs-lookup"><span data-stu-id="0b56e-253">This object is a **Boolean**.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="0b56e-254">A séma kódjának megírása</span><span class="sxs-lookup"><span data-stu-id="0b56e-254">Create the schema in the code</span></span>
<span data-ttu-id="0b56e-255">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-255">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-256">Nyissa meg a `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-256">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="0b56e-257">Helyezze el a következő adatokat a konfigurációs bejegyzés alatt:</span><span class="sxs-lookup"><span data-stu-id="0b56e-257">Add the following information below the configuration entry:</span></span>

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
<span data-ttu-id="0b56e-258">Először létre kell hozni a sémát, majd a modellobjektumot, amelyet arra fog használni, hogy a kód egészében tárolja adatait az **útvonalak** meghatározása során.</span><span class="sxs-lookup"><span data-stu-id="0b56e-258">You first create the schema, and then you create a model object that you use to store your data throughout the code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="0b56e-259">A REST API-feladatkiszolgálóra mutató útvonalak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0b56e-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="0b56e-260">Most, hogy létrehozta az adatbázismodellt, adja hozzá a REST API-kiszolgálóhoz használandó útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="0b56e-260">Now that you have a database model to work with, add the routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="0b56e-261">Az útvonalak működése a Restify programban</span><span class="sxs-lookup"><span data-stu-id="0b56e-261">About routes in Restify</span></span>
<span data-ttu-id="0b56e-262">Az útvonalak a Restifyban ugyanúgy működnek, mint az Express verem használata esetén.</span><span class="sxs-lookup"><span data-stu-id="0b56e-262">Routes work in Restify in the same way that they work when they use the Express stack.</span></span> <span data-ttu-id="0b56e-263">Az útvonalakat azon URI használatával kell meghatározni, amelyet elképzelései szerint az ügyfélalkalmazások meg fognak hívni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-263">You define routes by using the URI that you expect the client applications to call.</span></span>

<span data-ttu-id="0b56e-264">Íme egy jellegzetes Restify-útvonal:</span><span class="sxs-lookup"><span data-stu-id="0b56e-264">A typical pattern for a Restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

<span data-ttu-id="0b56e-265">A Restify és az Express ennél jóval összetettebb funkciók biztosítására is képes: például definiálhatja velük az alkalmazástípusokat, valamint bonyolult útvonalvezetést valósíthat meg a különböző végpontok között.</span><span class="sxs-lookup"><span data-stu-id="0b56e-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="0b56e-266">Ebben az oktatóanyagban egyszerűbb útvonalakat használunk.</span><span class="sxs-lookup"><span data-stu-id="0b56e-266">For the purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="0b56e-267">Alapértelmezett útvonalak beállítása a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="0b56e-267">Add default routes to your server</span></span>
<span data-ttu-id="0b56e-268">Most a REST API-hoz hozzáadjuk az alapszintű CRUD-útvonalakat: **létrehozás** és **listázás**.</span><span class="sxs-lookup"><span data-stu-id="0b56e-268">You now add the basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="0b56e-269">A példa `complete` részében további útvonalak is szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="0b56e-269">Other routes can be found in the `complete` branch of the sample.</span></span>

<span data-ttu-id="0b56e-270">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-270">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="0b56e-271">Nyissa meg a `server.js` fájlt valamelyik szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0b56e-271">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="0b56e-272">Írja be az alábbi információkat a fentiekben létrehozott adatbázis-bejegyzések alá:</span><span class="sxs-lookup"><span data-stu-id="0b56e-272">Below the database entries you made above add the following information:</span></span>

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
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
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

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
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="0b56e-273">Hibakezelés beállítása az útvonalakhoz</span><span class="sxs-lookup"><span data-stu-id="0b56e-273">Add error handling for the routes</span></span>
<span data-ttu-id="0b56e-274">Állítson be hibakezelést is, hogy az észlelt problémákat olyan formában továbbíthassa az ügyfélhez, amelyet az képes megérteni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-274">Add some error handling so that you can communicate any problems you encounter back to the client in a way that it can understand.</span></span>

<span data-ttu-id="0b56e-275">Adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0b56e-275">Add the following code:</span></span>

```Javascript
///--- Errors for communicating something interesting back to the client
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


## <a name="create-your-server"></a><span data-ttu-id="0b56e-276">A kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b56e-276">Create your server</span></span>
<span data-ttu-id="0b56e-277">Készen áll az adatbázis, és az útvonalak is a helyükön vannak.</span><span class="sxs-lookup"><span data-stu-id="0b56e-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="0b56e-278">Utolsó lépésként hozzá kell adni a hívásokat kezelő kiszolgálópéldányt.</span><span class="sxs-lookup"><span data-stu-id="0b56e-278">The last thing for you to do is to add the server instance that manages your calls.</span></span>

<span data-ttu-id="0b56e-279">A Restify és az Express mélyreható konfigurációs funkciókat biztosítanak a REST API-kiszolgálóhoz, de itt most az alapszintű beállításokat fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-279">Restify and Express provide deep customization for a REST API server, but we use the most basic setup here.</span></span>

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

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a><span data-ttu-id="0b56e-280">Útvonalak hozzáadása a kiszolgálóhoz (hitelesítés nélkül)</span><span class="sxs-lookup"><span data-stu-id="0b56e-280">Add the routes to the server (without authentication)</span></span>
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
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-to-your-rest-api-server"></a><span data-ttu-id="0b56e-281">Hitelesítés hozzáadása a REST API-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="0b56e-281">Add authentication to your REST API server</span></span>
<span data-ttu-id="0b56e-282">Most, hogy rendelkezésére áll a futó REST API-kiszolgáló, felhasználhatja azt az Azure AD-ben is.</span><span class="sxs-lookup"><span data-stu-id="0b56e-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="0b56e-283">A parancssorban módosítsa a következőre a könyvtárat: `azuread` (ha jelenleg nem ebben a könyvtárban van):</span><span class="sxs-lookup"><span data-stu-id="0b56e-283">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="0b56e-284">A passport-azure-ad részét képező OIDCBearerStrategy használata</span><span class="sxs-lookup"><span data-stu-id="0b56e-284">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="0b56e-285">Az API-k írásakor mindig kapcsolja az adatokat a jogkivonat valamely különleges részéhez, amelyhez a felhasználók nem tudnak jogosulatlanul hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-285">When you write APIs, you should always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="0b56e-286">A kiszolgáló a jogkivonat „owner” mezőjében szereplő felhasználónak (a token.oid segítségével meghívott) **objektumazonosítója** alapján tárolja a ToDo elemeket.</span><span class="sxs-lookup"><span data-stu-id="0b56e-286">When the server stores ToDo items, it does so based on the **oid** of the user in the token (called through token.oid), which goes in the “owner” field.</span></span> <span data-ttu-id="0b56e-287">Ez az érték biztosítja, hogy csak az adott felhasználó férhet hozzá a saját ToDo elemeihez.</span><span class="sxs-lookup"><span data-stu-id="0b56e-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="0b56e-288">Az „owner” API-ja nem jelenik meg sehol, így még a hitelesített külső felhasználók sem kérelmezhetik mások ToDo elemeit.</span><span class="sxs-lookup"><span data-stu-id="0b56e-288">There is no exposure in the API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="0b56e-289">Ezt követően használja a `passport-azure-ad` részét képező tulajdonosi stratégiát.</span><span class="sxs-lookup"><span data-stu-id="0b56e-289">Next, use the bearer strategy that comes with `passport-azure-ad`.</span></span>

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
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
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

<span data-ttu-id="0b56e-290">A Passport az összes stratégia esetében hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-290">Passport uses the same pattern for all its strategies.</span></span> <span data-ttu-id="0b56e-291">Át kell neki adni egy `function()` elemet, amelynek paramétereihez tartozik a `token` és a `done`.</span><span class="sxs-lookup"><span data-stu-id="0b56e-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="0b56e-292">Ha a stratégia elvégezte teendőit, visszatér Önhöz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-292">The strategy comes back to you after it has done all of its work.</span></span> <span data-ttu-id="0b56e-293">Ekkor érdemes tárolni a felhasználót és menteni a jogkivonatot, hogy a műveletet ne kelljen megismételni.</span><span class="sxs-lookup"><span data-stu-id="0b56e-293">You should then store the user and save the token so that you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b56e-294">A fenti kódrészlettel a hitelesítésen áteső felhasználók bejuthatnak a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="0b56e-294">The code above takes any user who happens to authenticate to your server.</span></span> <span data-ttu-id="0b56e-295">Ezt a folyamatot automatikus regisztrációnak nevezzük.</span><span class="sxs-lookup"><span data-stu-id="0b56e-295">This process is known as autoregistration.</span></span> <span data-ttu-id="0b56e-296">Az élesben működő kiszolgálók esetében csak akkor engedélyezze az API-khoz való felhasználói hozzáférést, ha már elvégezték a regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="0b56e-296">In production servers, don't let in any users access the API without first having them go through a registration process.</span></span> <span data-ttu-id="0b56e-297">Általában ezt a megoldást látjuk az olyan fogyasztói alkalmazásoknál, amelyek engedélyezik a Facebook segítségével történő regisztrációt, de aztán további adatok megadását kérik.</span><span class="sxs-lookup"><span data-stu-id="0b56e-297">This process is usually the pattern you see in consumer apps that allow you to register by using Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="0b56e-298">Ha ez nem parancssori program lenne, a visszakapott jogkivonat-objektumból kinyerhettük volna az e-mail-címet, és felkérhettük volna a felhasználót a kiegészítő adatok megadására.</span><span class="sxs-lookup"><span data-stu-id="0b56e-298">If this program wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked users to fill out additional information.</span></span> <span data-ttu-id="0b56e-299">Mivel ez csupán egy példa, most egyszerűen hozzáadjuk őket a memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="0b56e-299">Because this is a sample, we add them to an in-memory database.</span></span>
>
>

## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a><span data-ttu-id="0b56e-300">A kiszolgálóalkalmazás futtatása annak ellenőrzése érdekében, hogy elutasítja-e Önt</span><span class="sxs-lookup"><span data-stu-id="0b56e-300">Run your server application to verify that it rejects you</span></span>
<span data-ttu-id="0b56e-301">A `curl` használatával ellenőrizheti, hogy az OAuth2-védelem működik-e a végpontokon.</span><span class="sxs-lookup"><span data-stu-id="0b56e-301">You can use `curl` to see if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="0b56e-302">A visszakapott fejlécek alapján már megállapíthatja, hogy jó úton jár-e.</span><span class="sxs-lookup"><span data-stu-id="0b56e-302">The headers returned should be enough to tell you that you are on the right path.</span></span>

<span data-ttu-id="0b56e-303">Ellenőrizze, hogy fut-e a MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="0b56e-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="0b56e-304">Váltson át a könyvtárra és futtassa a kiszolgálót:</span><span class="sxs-lookup"><span data-stu-id="0b56e-304">Change to the directory and run the server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="0b56e-305">Egy új terminálablakban futtassa a `curl` parancsot</span><span class="sxs-lookup"><span data-stu-id="0b56e-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="0b56e-306">Próbálkozzon meg egy egyszerű POST művelettel:</span><span class="sxs-lookup"><span data-stu-id="0b56e-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="0b56e-307">A cél, hogy 401-es hiba jelenjen meg.</span><span class="sxs-lookup"><span data-stu-id="0b56e-307">A 401 error is the response you want.</span></span> <span data-ttu-id="0b56e-308">Ez azt jelzi, hogy a Passport réteg megpróbálja átirányítani a hitelesítési végpontra.</span><span class="sxs-lookup"><span data-stu-id="0b56e-308">It indicates that the Passport layer is trying to redirect to the authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="0b56e-309">Az OAuth2-t használó REST API-szolgáltatás ezzel elkészült</span><span class="sxs-lookup"><span data-stu-id="0b56e-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="0b56e-310">A Restify és az OAuth használatával elkészítette a REST API-t!</span><span class="sxs-lookup"><span data-stu-id="0b56e-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="0b56e-311">Most már rendelkezésre állnak a megfelelő kódrészletek, amelyek segítségével tovább fejlesztheti szolgáltatását.</span><span class="sxs-lookup"><span data-stu-id="0b56e-311">You now have sufficient code so that you can continue to develop your service and build on this example.</span></span> <span data-ttu-id="0b56e-312">Megtettünk mindent, ami a kiszolgálón OAuth2-kompatibilis ügyfél nélkül lehetséges volt.</span><span class="sxs-lookup"><span data-stu-id="0b56e-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="0b56e-313">A következő lépéshez használjon újabb útmutatókat, például a [Csatlakozás webes API-hoz az iOS rendszer és a B2C segítségével](active-directory-b2c-devquickstarts-ios.md) című témakört.</span><span class="sxs-lookup"><span data-stu-id="0b56e-313">For that next step use an additional walk-through like our [Connect to a web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b56e-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b56e-314">Next steps</span></span>
<span data-ttu-id="0b56e-315">Most már továbbléphet az összetettebb témákra, például:</span><span class="sxs-lookup"><span data-stu-id="0b56e-315">You can now move to more advanced topics, such as:</span></span>

[<span data-ttu-id="0b56e-316">Csatlakozás webes API-hoz az iOS rendszer és a B2C segítségével</span><span class="sxs-lookup"><span data-stu-id="0b56e-316">Connect to a web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
