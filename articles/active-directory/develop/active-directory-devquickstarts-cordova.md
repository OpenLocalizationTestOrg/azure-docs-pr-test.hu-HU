---
title: "Első lépések AD Cordova aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild Cordova-alkalmazás, amely integrálható az Azure ad-val bejelentkezési és az Azure AD-védett API OAuth használatával."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a><span data-ttu-id="4792e-103">Azure AD integrálása az Apache Cordova-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4792e-103">Integrate Azure AD with an Apache Cordova app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="4792e-104">Apache Cordova toodevelop HTML5/JavaScript alkalmazások futtatható, mint a teljes körű natív alkalmazások mobileszközökön is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4792e-104">You can use Apache Cordova toodevelop HTML5/JavaScript applications that can run on mobile devices as full-fledged native applications.</span></span> <span data-ttu-id="4792e-105">Az Azure Active Directoryval (Azure AD) a vállalati szintű hitelesítési képességek tooyour Cordova alkalmazások is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="4792e-105">With Azure Active Directory (Azure AD), you can add enterprise-grade authentication capabilities tooyour Cordova applications.</span></span>

<span data-ttu-id="4792e-106">A Cordova beépülő modul becsomagolja az Azure AD natív SDK-k iOS, Android, Windows áruház és Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="4792e-106">A Cordova plug-in wraps Azure AD native SDKs on iOS, Android, Windows Store, and Windows Phone.</span></span> <span data-ttu-id="4792e-107">Hogy a beépülő modul, növelheti az alkalmazás toosupport jelentkezzen be a felhasználók a Windows Server Active Directory-fiókok használatával hozzáférést tooOffice 365 és Azure API-k, és még védelme hívások tooyour saját egyéni webes API-t.</span><span class="sxs-lookup"><span data-stu-id="4792e-107">By using that plug-in, you can enhance your application toosupport sign-in with your users' Windows Server Active Directory accounts, gain access tooOffice 365 and Azure APIs, and even help protect calls tooyour own custom web API.</span></span>

<span data-ttu-id="4792e-108">Ebben az oktatóanyagban fogjuk használni hello Apache Cordova beépülő modul az Active Directory Authentication Library (ADAL) tooimprove egy egyszerű alkalmazást a következő funkciók hello hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="4792e-108">In this tutorial, we'll use hello Apache Cordova plug-in for Active Directory Authentication Library (ADAL) tooimprove a simple app by adding hello following features:</span></span>

* <span data-ttu-id="4792e-109">A pár sornyi kód hitelesíteni a felhasználót, és jogkivonat beszerzése.</span><span class="sxs-lookup"><span data-stu-id="4792e-109">With just a few lines of code, authenticate a user and obtain a token.</span></span>
* <span data-ttu-id="4792e-110">Használja a token tooinvoke hello Graph API tooquery könyvtárhoz, és hello eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4792e-110">Use that token tooinvoke hello Graph API tooquery that directory and display hello results.</span></span>  
* <span data-ttu-id="4792e-111">Hello ADAL jogkivonat gyorsítótára toominimize hitelesítés használata a hello felhasználó kéri.</span><span class="sxs-lookup"><span data-stu-id="4792e-111">Use hello ADAL token cache toominimize authentication prompts for hello user.</span></span>

<span data-ttu-id="4792e-112">toomake ezen fejlesztések kell:</span><span class="sxs-lookup"><span data-stu-id="4792e-112">toomake those improvements, you need to:</span></span>

1. <span data-ttu-id="4792e-113">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4792e-113">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="4792e-114">Adja meg a kódot tooyour app toorequest elemeket.</span><span class="sxs-lookup"><span data-stu-id="4792e-114">Add code tooyour app toorequest tokens.</span></span>
3. <span data-ttu-id="4792e-115">Adja hozzá a kódot toouse hello token lekérdezése hello Graph API-val, és eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4792e-115">Add code toouse hello token for querying hello Graph API and display results.</span></span>
4. <span data-ttu-id="4792e-116">Hello Cordova telepítési projekt létrehozása minden hello platformokkal meg szeretné, hogy tootarget hello ADAL Cordova beépülő modul hozzáadása és hello megoldás tesztelése az emulátorok.</span><span class="sxs-lookup"><span data-stu-id="4792e-116">Create hello Cordova deployment project with all hello platforms you want tootarget, add hello Cordova ADAL plug-in, and test hello solution in emulators.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4792e-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4792e-117">Prerequisites</span></span>
<span data-ttu-id="4792e-118">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="4792e-118">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="4792e-119">Az Azure AD-bérlő app development jogosultságokkal rendelkező fiók esetében.</span><span class="sxs-lookup"><span data-stu-id="4792e-119">An Azure AD tenant where you have an account with app development rights.</span></span>
* <span data-ttu-id="4792e-120">A fejlesztési környezet, amely toouse Apache Cordova van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4792e-120">A development environment that's configured toouse Apache Cordova.</span></span>  

<span data-ttu-id="4792e-121">Ha mindkét már beállítása, hogy továbblépjen közvetlenül toostep 1.</span><span class="sxs-lookup"><span data-stu-id="4792e-121">If you have both already set up, proceed directly toostep 1.</span></span>

<span data-ttu-id="4792e-122">Ha az Azure AD-bérlő nem rendelkezik, akkor hello [útmutatást egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="4792e-122">If you don't have an Azure AD tenant, use hello [instructions on how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="4792e-123">Ha Apache Cordova állítsa be a számítógépre nincs, telepítse a hello következő:</span><span class="sxs-lookup"><span data-stu-id="4792e-123">If you don't have Apache Cordova set up on your machine, install hello following:</span></span>

* [<span data-ttu-id="4792e-124">Git</span><span class="sxs-lookup"><span data-stu-id="4792e-124">Git</span></span>](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [<span data-ttu-id="4792e-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="4792e-125">Node.js</span></span>](https://nodejs.org/download/)
* <span data-ttu-id="4792e-126">[Cordova CLI](https://cordova.apache.org/) (Csomagkezelő NPM segítségével könnyedén telepíthető: `npm install -g cordova`)</span><span class="sxs-lookup"><span data-stu-id="4792e-126">[Cordova CLI](https://cordova.apache.org/) (can be easily installed via NPM package manager: `npm install -g cordova`)</span></span>

<span data-ttu-id="4792e-127">hello telepítések megelőző hello PC és a Mac hello kell működnie.</span><span class="sxs-lookup"><span data-stu-id="4792e-127">hello preceding installations should work both on hello PC and on hello Mac.</span></span>

<span data-ttu-id="4792e-128">Minden érintett rendszerek különböző előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="4792e-128">Each target platform has different prerequisites:</span></span>

* <span data-ttu-id="4792e-129">toobuild, és futtassa az alkalmazást a Windows/Táblagép vagy a Windows Phone:</span><span class="sxs-lookup"><span data-stu-id="4792e-129">toobuild and run an app for Windows Tablet/PC or Windows Phone:</span></span>
  * <span data-ttu-id="4792e-130">Telepítés [Visual Studio 2013 Update 2 vagy újabb verziójú Windows](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express vagy egy másik verziója) vagy [Visual Studio 2015-öt](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span><span class="sxs-lookup"><span data-stu-id="4792e-130">Install [Visual Studio 2013 for Windows with Update 2 or later](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express or another version) or [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).</span></span>

* <span data-ttu-id="4792e-131">toobuild, és futtassa az alkalmazást IOS rendszerre:</span><span class="sxs-lookup"><span data-stu-id="4792e-131">toobuild and run an app for iOS:</span></span>

  * <span data-ttu-id="4792e-132">Xcode telepítése 6.x vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4792e-132">Install Xcode 6.x or later.</span></span> <span data-ttu-id="4792e-133">Töltse le a hello [Apple Developer hely](http://developer.apple.com/downloads) vagy hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span><span class="sxs-lookup"><span data-stu-id="4792e-133">Download it from hello [Apple Developer site](http://developer.apple.com/downloads) or hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).</span></span>
  * <span data-ttu-id="4792e-134">Telepítés [ios-sim](https://www.npmjs.org/package/ios-sim).</span><span class="sxs-lookup"><span data-stu-id="4792e-134">Install [ios-sim](https://www.npmjs.org/package/ios-sim).</span></span> <span data-ttu-id="4792e-135">Használhatja toostart iOS-alkalmazások az iOS-szimulátor hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="4792e-135">You can use it toostart iOS apps in iOS Simulator from hello command line.</span></span> <span data-ttu-id="4792e-136">(A Terminálszolgáltatások hello keresztül könnyedén telepíthető: `npm install -g ios-sim`.)</span><span class="sxs-lookup"><span data-stu-id="4792e-136">(You can easily install it via hello terminal: `npm install -g ios-sim`.)</span></span>
* <span data-ttu-id="4792e-137">toobuild, és futtassa az alkalmazást az Android:</span><span class="sxs-lookup"><span data-stu-id="4792e-137">toobuild and run an app for Android:</span></span>

  * <span data-ttu-id="4792e-138">Telepítés [Java fejlesztői készlet (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4792e-138">Install [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or later.</span></span> <span data-ttu-id="4792e-139">Győződjön meg arról, hogy `JAVA_HOME` (környezeti változó) megfelelően van beállítva, toohello JDK telepítési útvonalat (például, C:\Program Files\Java\jdk1.7.0_75) szerint.</span><span class="sxs-lookup"><span data-stu-id="4792e-139">Make sure `JAVA_HOME` (environment variable) is correctly set according toohello JDK installation path (for example, C:\Program Files\Java\jdk1.7.0_75).</span></span>
  * <span data-ttu-id="4792e-140">Telepítés [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) , és adja hozzá a hello `<android-sdk-location>\tools` helyét (például C:\tools\Android\android-sdk\tools) tooyour `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="4792e-140">Install [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) and add hello `<android-sdk-location>\tools` location (for example, C:\tools\Android\android-sdk\tools) tooyour `PATH` environment variable.</span></span>
  * <span data-ttu-id="4792e-141">Nyissa meg az Android SDK Manager (például a Terminálszolgáltatások hello keresztül: `android`) és telepítése:</span><span class="sxs-lookup"><span data-stu-id="4792e-141">Open Android SDK Manager (for example, via hello terminal: `android`) and install:</span></span>
    * <span data-ttu-id="4792e-142">*Android 5.0.1-es (API 21)* platform SDK</span><span class="sxs-lookup"><span data-stu-id="4792e-142">*Android 5.0.1 (API 21)* platform SDK</span></span>
    * <span data-ttu-id="4792e-143">*Android SDK Build Tools* 19.1.0 verzió vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="4792e-143">*Android SDK Build Tools* version 19.1.0 or later</span></span>
    * <span data-ttu-id="4792e-144">*Android-támogatás tárház* (kiegészítő funkciók)</span><span class="sxs-lookup"><span data-stu-id="4792e-144">*Android Support Repository* (Extras)</span></span>

  <span data-ttu-id="4792e-145">Android SDK hello bármely alapértelmezett emulátor példány nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="4792e-145">hello Android SDK doesn't provide any default emulator instance.</span></span> <span data-ttu-id="4792e-146">Futtatásával `android avd` hello terminál és jelölje be a **létrehozása**, ha azt szeretné, hogy toorun hello Android-alkalmazást az emulátor.</span><span class="sxs-lookup"><span data-stu-id="4792e-146">Create one by running `android avd` from hello terminal and then selecting **Create**, if you want toorun hello Android app on an emulator.</span></span> <span data-ttu-id="4792e-147">Azt javasoljuk, hogy egy API-szintet 19 vagy magasabb.</span><span class="sxs-lookup"><span data-stu-id="4792e-147">We recommend an API level of 19 or higher.</span></span> <span data-ttu-id="4792e-148">Hello Android emulator és létrehozásának beállításokkal kapcsolatos további információkért lásd: [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) hello Android helyen.</span><span class="sxs-lookup"><span data-stu-id="4792e-148">For more information about hello Android emulator and creation options, see [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) on hello Android site.</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="4792e-149">1. lépés: Egy alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="4792e-149">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="4792e-150">Ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="4792e-150">This step is optional.</span></span> <span data-ttu-id="4792e-151">Ez az oktatóanyag biztosít használható toosee előtti ponton aktívvá vált értékek hello művelet: a minta bármely saját bérlőt kiépítési nélkül.</span><span class="sxs-lookup"><span data-stu-id="4792e-151">This tutorial provides pre-provisioned values that you can use toosee hello sample in action without doing any provisioning in your own tenant.</span></span> <span data-ttu-id="4792e-152">Azonban azt javasoljuk, hogy hajtsa végre ezt a lépést, és ismerkedjen meg hello folyamat, mert lesz szükség a saját alkalmazások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4792e-152">However, we recommend that you do perform this step and become familiar with hello process, because it will be required when you create your own applications.</span></span>

<span data-ttu-id="4792e-153">Az Azure AD kibocsát jogkivonatok tooonly alkalmazások ismert.</span><span class="sxs-lookup"><span data-stu-id="4792e-153">Azure AD issues tokens tooonly known applications.</span></span> <span data-ttu-id="4792e-154">Az Azure AD a alkalmazás használata előtt kell toocreate bejegyzése ki azt az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="4792e-154">Before you can use Azure AD from your app, you need toocreate an entry for it in your tenant.</span></span> <span data-ttu-id="4792e-155">az Ön bérelt szolgáltatásának új alkalmazás tooregister:</span><span class="sxs-lookup"><span data-stu-id="4792e-155">tooregister a new application in your tenant:</span></span>

1. <span data-ttu-id="4792e-156">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4792e-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4792e-157">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="4792e-157">On hello top bar, click your account.</span></span> <span data-ttu-id="4792e-158">A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4792e-158">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="4792e-159">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4792e-159">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="4792e-160">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4792e-160">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="4792e-161">Hello utasításokat követve, és hozzon létre egy **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4792e-161">Follow hello prompts and create a **Native Client Application**.</span></span> <span data-ttu-id="4792e-162">(Bár a Cordova-alkalmazásokkal HTML-alapú, most létrehozzuk natív ügyfélalkalmazás itt.</span><span class="sxs-lookup"><span data-stu-id="4792e-162">(Although Cordova apps are HTML based, we're creating a native client application here.</span></span> <span data-ttu-id="4792e-163">Hello **natív ügyfélalkalmazás** beállítást, vagy hello alkalmazás nem fog működni.)</span><span class="sxs-lookup"><span data-stu-id="4792e-163">hello **Native Client Application** option must be selected, or hello application won't work.)</span></span>
  * <span data-ttu-id="4792e-164">**Név** az alkalmazás toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4792e-164">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="4792e-165">**Átirányítási URI** hello tooreturn jogkivonatok tooyour alkalmazás által használt URI.</span><span class="sxs-lookup"><span data-stu-id="4792e-165">**Redirect URI** is hello URI that's used tooreturn tokens tooyour app.</span></span> <span data-ttu-id="4792e-166">Adja meg **http://MyDirectorySearcherApp**.</span><span class="sxs-lookup"><span data-stu-id="4792e-166">Enter **http://MyDirectorySearcherApp**.</span></span>

<span data-ttu-id="4792e-167">Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="4792e-167">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="4792e-168">Ez az érték a következő szakaszok hello lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="4792e-168">You’ll need this value in hello next sections.</span></span> <span data-ttu-id="4792e-169">Az újonnan létrehozott alkalmazás hello hello alkalmazás lapján találja.</span><span class="sxs-lookup"><span data-stu-id="4792e-169">You can find it on hello application tab of hello newly created app.</span></span>

<span data-ttu-id="4792e-170">toorun `DirSearchClient Sample`, adja meg az újonnan létrehozott hello app engedély tooquery hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="4792e-170">toorun `DirSearchClient Sample`, grant hello newly created app permission tooquery hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="4792e-171">A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4792e-171">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>  
2. <span data-ttu-id="4792e-172">Hello Azure Active Directory-alkalmazást, válassza a **Microsoft Graph** , hello API, és adja hozzá a hello **hozzáférés hello a címtárhoz hello bejelentkezett felhasználó** engedélyt a **meghatalmazott Engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="4792e-172">For hello Azure Active Directory application, select **Microsoft Graph** as hello API and add hello **Access hello directory as hello signed-in user** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="4792e-173">Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="4792e-173">This enables your application tooquery hello Graph API for users.</span></span>

## <a name="step-2-clone-hello-sample-app-repository"></a><span data-ttu-id="4792e-174">2. lépés: Hello sample app tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="4792e-174">Step 2: Clone hello sample app repository</span></span>
<span data-ttu-id="4792e-175">A rendszerhéj vagy a parancssorból írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4792e-175">From your shell or command line, type hello following command:</span></span>

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a><span data-ttu-id="4792e-176">3. lépés: Hello Cordova-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4792e-176">Step 3: Create hello Cordova app</span></span>
<span data-ttu-id="4792e-177">Toocreate Cordova-alkalmazás több módon is.</span><span class="sxs-lookup"><span data-stu-id="4792e-177">There are multiple ways toocreate Cordova applications.</span></span> <span data-ttu-id="4792e-178">Ebben az oktatóanyagban hello Cordova parancssori felület (CLI) fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="4792e-178">In this tutorial, we'll use hello Cordova command-line interface (CLI).</span></span>

1. <span data-ttu-id="4792e-179">A rendszerhéj vagy a parancssorból írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4792e-179">From your shell or command line, type hello following command:</span></span>

        cordova create DirSearchClient

   <span data-ttu-id="4792e-180">Ez a parancs hello mappaszerkezet és állványok hello Cordova-projekt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4792e-180">That command creates hello folder structure and scaffolding for hello Cordova project.</span></span>

2. <span data-ttu-id="4792e-181">Toohello új DirSearchClient mappa áthelyezése:</span><span class="sxs-lookup"><span data-stu-id="4792e-181">Move toohello new DirSearchClient folder:</span></span>

        cd .\DirSearchClient

3. <span data-ttu-id="4792e-182">Hello tartalom hello alapszintű projekt hello www almappájában másolja a Fájlkezelőben vagy a következő parancsot a rendszerhéj hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="4792e-182">Copy hello content of hello starter project in hello www subfolder by using a file manager or hello following command in your shell:</span></span>

  * <span data-ttu-id="4792e-183">Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span><span class="sxs-lookup"><span data-stu-id="4792e-183">Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`</span></span>
  * <span data-ttu-id="4792e-184">Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span><span class="sxs-lookup"><span data-stu-id="4792e-184">Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`</span></span>

4. <span data-ttu-id="4792e-185">Adja hozzá a beépülő modul hello engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="4792e-185">Add hello whitelist plug-in.</span></span> <span data-ttu-id="4792e-186">Erre akkor szükség, hello Graph API meghívására.</span><span class="sxs-lookup"><span data-stu-id="4792e-186">This is necessary for invoking hello Graph API.</span></span>

        cordova plugin add cordova-plugin-whitelist

5. <span data-ttu-id="4792e-187">Adja hozzá a megjeleníteni kívánt toosupport platformfüggetlen hello.</span><span class="sxs-lookup"><span data-stu-id="4792e-187">Add all hello platforms that you want toosupport.</span></span> <span data-ttu-id="4792e-188">egy minta toohave, a következő parancsok hello legalább egy tooexecute kell.</span><span class="sxs-lookup"><span data-stu-id="4792e-188">toohave a working sample, you need tooexecute at least one of hello following commands.</span></span> <span data-ttu-id="4792e-189">Vegye figyelembe, hogy nem kell tudni tooemulate iOS, Windows vagy Mac a Windows rendszer</span><span class="sxs-lookup"><span data-stu-id="4792e-189">Note that you won't be able tooemulate iOS on Windows or emulate Windows on a Mac.</span></span>

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. <span data-ttu-id="4792e-190">Adja hozzá a Cordova beépülő modul tooyour projekt ADAL hello:</span><span class="sxs-lookup"><span data-stu-id="4792e-190">Add hello ADAL for Cordova plug-in tooyour project:</span></span>

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a><span data-ttu-id="4792e-191">4. lépés: Kód tooauthenticate felhasználók hozzáadása, és az Azure AD tokenek beszerzése</span><span class="sxs-lookup"><span data-stu-id="4792e-191">Step 4: Add code tooauthenticate users and obtain tokens from Azure AD</span></span>
<span data-ttu-id="4792e-192">Ebben az oktatóanyagban kidolgozása hello alkalmazás nyújt egy egyszerű directory keresési funkciót.</span><span class="sxs-lookup"><span data-stu-id="4792e-192">hello application that you're developing in this tutorial will provide a simple directory search feature.</span></span> <span data-ttu-id="4792e-193">hello felhasználói ezután írja be a hello alias bármely felhasználó hello könyvtárban, és néhány alapvető attribútum megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4792e-193">hello user can then type hello alias of any user in hello directory and visualize some basic attributes.</span></span> <span data-ttu-id="4792e-194">hello alapszintű projekt hello alapszintű felhasználói felület (a www/index.html) hello alkalmazás hello definícióját tartalmazza, és hello állványok, amely alapszintű alkalmazást eseményt kábeleket ciklusok, a felhasználói felület kötések, és ennek eredményeként a megjelenítési a logikai (www/js/index.js).</span><span class="sxs-lookup"><span data-stu-id="4792e-194">hello starter project contains hello definition of hello basic user interface of hello app (in www/index.html) and hello scaffolding that wires up basic app event cycles, user interface bindings, and results display logic (in www/js/index.js).</span></span> <span data-ttu-id="4792e-195">hello csak akkor maradt feladata tooadd hello logika, amely megvalósítja az identitás-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="4792e-195">hello only task left for you is tooadd hello logic that implements identity tasks.</span></span>

<span data-ttu-id="4792e-196">hello toodo a kódban kell elsőként bevezetni hello protokoll értékek alapján azonosítja az alkalmazást használó Azure AD és erőforrásokhoz, hogy a célként meghatározott hello.</span><span class="sxs-lookup"><span data-stu-id="4792e-196">hello first thing you need toodo in your code is introduce hello protocol values that Azure AD uses for identifying your app and hello resources that you target.</span></span> <span data-ttu-id="4792e-197">Ezeket az értékeket meg lesz használt tooconstruct hello jogkivonat-kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="4792e-197">Those values will be used tooconstruct hello token requests later on.</span></span> <span data-ttu-id="4792e-198">Szúrja be a következő kódrészletet hello index.js fájl hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="4792e-198">Insert hello following snippet at hello top of hello index.js file:</span></span>

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

<span data-ttu-id="4792e-199">Hello `redirectUri` és `clientId` értékek illeszkednie kell az alkalmazást az Azure AD-leíró hello értékek.</span><span class="sxs-lookup"><span data-stu-id="4792e-199">hello `redirectUri` and `clientId` values should match hello values that describe your app in Azure AD.</span></span> <span data-ttu-id="4792e-200">Hello szerintiek található **konfigurálása** hello Azure-portálon lapon ez az oktatóanyag a korábban az 1. lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="4792e-200">You can find those from hello **Configure** tab in hello Azure portal, as described in step 1 earlier in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="4792e-201">Ha nem regisztrálja egy új alkalmazást a saját bérlőt választotta, egyszerűen beillesztheti, előre konfigurált hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="4792e-201">If you opted for not registering a new app in your own tenant, you can simply paste hello preconfigured values as is.</span></span> <span data-ttu-id="4792e-202">Majd látható hello minta fut, bár mindig hozzon létre saját bejegyzést az alkalmazások, amelyek termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4792e-202">You can then see hello sample running, though you should always create your own entry for your apps that are meant for production.</span></span>

<span data-ttu-id="4792e-203">Ezután adja hozzá a hello jogkivonatkérelem kódot.</span><span class="sxs-lookup"><span data-stu-id="4792e-203">Next, add hello token request code.</span></span> <span data-ttu-id="4792e-204">Helyezze be a következő kódrészletben közötti hello hello `search` és `renderData` definíciók:</span><span class="sxs-lookup"><span data-stu-id="4792e-204">Insert hello following snippet between hello `search` and `renderData` definitions:</span></span>

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
<span data-ttu-id="4792e-205">Vizsgáljuk meg, hogy a függvény által bontásához a két fő részből áll.</span><span class="sxs-lookup"><span data-stu-id="4792e-205">Let's examine that function by breaking it down in its two main parts.</span></span>
<span data-ttu-id="4792e-206">Ez a minta nem tervezett toowork semmilyen bérlővel, mert az megakadályozását toobeing kötött tooa adott egy.</span><span class="sxs-lookup"><span data-stu-id="4792e-206">This sample is designed toowork with any tenant, as opposed toobeing tied tooa particular one.</span></span> <span data-ttu-id="4792e-207">Hello használ "/ közös" végpont, amely lehetővé teszi, hogy hello felhasználói tooenter bármely fiók hitelesítési időpontban, és hello kérelem toohello bérlői irányítja, ahol tartozik.</span><span class="sxs-lookup"><span data-stu-id="4792e-207">It uses hello "/common" endpoint, which allows hello user tooenter any account at authentication time and directs hello request toohello tenant where it belongs.</span></span>

<span data-ttu-id="4792e-208">Ezen hello metódus első része hello ADAL gyorsítótár toosee megvizsgálja, ha egy jogkivonatot már van tárolva.</span><span class="sxs-lookup"><span data-stu-id="4792e-208">This first part of hello method inspects hello ADAL cache toosee if a token is already stored.</span></span> <span data-ttu-id="4792e-209">Ha igen, hello metódusnak hello bérlők hello token származási helyét az adal-t újrainicializálását.</span><span class="sxs-lookup"><span data-stu-id="4792e-209">If so, hello method uses hello tenants where hello token came from for reinitializing ADAL.</span></span> <span data-ttu-id="4792e-210">Ez az további utasításokat, szükséges tooavoid oka hello használja a "/ közös" mindig eredményezi hello felhasználói tooenter egy új fiókot kérni.</span><span class="sxs-lookup"><span data-stu-id="4792e-210">This is necessary tooavoid extra prompts, because hello use of "/common" always results in asking hello user tooenter a new account.</span></span>

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
<span data-ttu-id="4792e-211">hello metódus második része hello hello megfelelő jogkivonatkérelem hajt végre.</span><span class="sxs-lookup"><span data-stu-id="4792e-211">hello second part of hello method performs hello proper token request.</span></span> <span data-ttu-id="4792e-212">Hello `acquireTokenSilentAsync` metódus kér ADAL tooreturn jogkivonat megadott hello erőforrás bármely UX megjelenítése nélkül</span><span class="sxs-lookup"><span data-stu-id="4792e-212">hello `acquireTokenSilentAsync` method asks ADAL tooreturn a token for hello specified resource without showing any UX.</span></span> <span data-ttu-id="4792e-213">Amely akkor fordulhat elő, ha hello gyorsítótár már tartalmaz egy tárolt, megfelelő hozzáférési jogkivonatot, vagy ha egy frissítési jogkivonat tooget új tokenre használt minden kérdés megjelenítése nélkül.</span><span class="sxs-lookup"><span data-stu-id="4792e-213">That can happen if hello cache already has a suitable access token stored, or if a refresh token can be used tooget a new access token without showing any prompt.</span></span> <span data-ttu-id="4792e-214">Amely sikertelen lesz, ha azt térhet vissza `acquireTokenAsync`– amely láthatóan fogja kérni a hello felhasználói tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="4792e-214">If that attempt fails, we fall back on `acquireTokenAsync`--which will visibly prompt hello user tooauthenticate.</span></span>

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
<span data-ttu-id="4792e-215">Most, hogy hello jogkivonat, azt végül hello Graph API meghívása és szeretnénk hello-keresési lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="4792e-215">Now that we have hello token, we can finally invoke hello Graph API and perform hello search query that we want.</span></span> <span data-ttu-id="4792e-216">Helyezze be a következő kódrészletet alatt hello hello `authenticate` definíciója:</span><span class="sxs-lookup"><span data-stu-id="4792e-216">Insert hello following snippet below hello `authenticate` definition:</span></span>

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
<span data-ttu-id="4792e-217">hello kiindulópont-fájlokat egy egyszerű UX megadott beviteli mezőben írja be a felhasználói alias.</span><span class="sxs-lookup"><span data-stu-id="4792e-217">hello starting-point files supplied a simple UX for entering a user's alias in a text box.</span></span> <span data-ttu-id="4792e-218">Ezt a módszert használja, hogy érték tooconstruct egy lekérdezést, összevonásához hello hozzáférési jogkivonatot, elküldi a tooMicrosoft Graph és hello eredményeket elemezni.</span><span class="sxs-lookup"><span data-stu-id="4792e-218">This method uses that value tooconstruct a query, combine it with hello access token, send it tooMicrosoft Graph, and parse hello results.</span></span> <span data-ttu-id="4792e-219">Hello `renderData` metódus már szerepel a hello kiindulópont-fájl, gondoskodik hello eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4792e-219">hello `renderData` method, already present in hello starting-point file, takes care of visualizing hello results.</span></span>

## <a name="step-5-run-hello-app"></a><span data-ttu-id="4792e-220">5. lépés: Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4792e-220">Step 5: Run hello app</span></span>
<span data-ttu-id="4792e-221">Az alkalmazás készen áll a finally toorun.</span><span class="sxs-lookup"><span data-stu-id="4792e-221">Your app is finally ready toorun.</span></span> <span data-ttu-id="4792e-222">Az operációs rendszer egyszerű: hello alkalmazás indításakor hello alias toolook akarja hello felhasználó adja meg, majd hello gombra.</span><span class="sxs-lookup"><span data-stu-id="4792e-222">Operating it is simple: when hello app starts, enter hello alias of hello user you want toolook up, and then click hello button.</span></span> <span data-ttu-id="4792e-223">Megkéri, hogy hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="4792e-223">You're prompted for authentication.</span></span> <span data-ttu-id="4792e-224">Sikeres hitelesítés és a keresés sikeres a keresés hello felhasználó hello attribútumok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4792e-224">Upon successful authentication and successful search, hello attributes of hello searched user are displayed.</span></span>

<span data-ttu-id="4792e-225">Utólagosan végrehajtják a hello keresési bármilyen kérdés megjelenítése nélkül, köszönhetően hello toohello jelenléte korábban beszerzett jogkivonat a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="4792e-225">Subsequent runs will perform hello search without showing any prompt, thanks toohello presence of hello previously acquired token in cache.</span></span>

<span data-ttu-id="4792e-226">Platform hello konkrét hello alkalmazást futtató lépései eltérők lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4792e-226">hello concrete steps for running hello app vary by platform.</span></span>

### <a name="windows-10"></a><span data-ttu-id="4792e-227">Windows 10</span><span class="sxs-lookup"><span data-stu-id="4792e-227">Windows 10</span></span>
   <span data-ttu-id="4792e-228">/ Táblagép:`cordova run windows --archs=x64 -- --appx=uap`</span><span class="sxs-lookup"><span data-stu-id="4792e-228">Tablet/PC: `cordova run windows --archs=x64 -- --appx=uap`</span></span>

   <span data-ttu-id="4792e-229">A Mobile (a Windows 10 Mobile-eszköz csatlakoztatásakor tooa PC szükséges):`cordova run windows --archs=arm -- --appx=uap --phone`</span><span class="sxs-lookup"><span data-stu-id="4792e-229">Mobile (requires a Windows 10 Mobile device connected tooa PC): `cordova run windows --archs=arm -- --appx=uap --phone`</span></span>

   > [!NOTE]
   > <span data-ttu-id="4792e-230">Során hello először futtatja kérheti a toosign fejlesztői licencet.</span><span class="sxs-lookup"><span data-stu-id="4792e-230">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="4792e-231">További információkért lásd: [fejlesztői licenc](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="4792e-231">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-81-tabletpc"></a><span data-ttu-id="4792e-232">Windows 8.1 / Táblagép</span><span class="sxs-lookup"><span data-stu-id="4792e-232">Windows 8.1 Tablet/PC</span></span>
   `cordova run windows`

   > [!NOTE]
   > <span data-ttu-id="4792e-233">Során hello először futtatja kérheti a toosign fejlesztői licencet.</span><span class="sxs-lookup"><span data-stu-id="4792e-233">During hello first run, you might be asked toosign in for a developer license.</span></span> <span data-ttu-id="4792e-234">További információkért lásd: [fejlesztői licenc](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span><span class="sxs-lookup"><span data-stu-id="4792e-234">For more information, see [Developer license](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).</span></span>

### <a name="windows-phone-81"></a><span data-ttu-id="4792e-235">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="4792e-235">Windows Phone 8.1</span></span>
   <span data-ttu-id="4792e-236">egy csatlakoztatott eszközön toorun:`cordova run windows --device -- --phone`</span><span class="sxs-lookup"><span data-stu-id="4792e-236">toorun on a connected device: `cordova run windows --device -- --phone`</span></span>

   <span data-ttu-id="4792e-237">hello alapértelmezett emulátor toorun:`cordova emulate windows -- --phone`</span><span class="sxs-lookup"><span data-stu-id="4792e-237">toorun on hello default emulator: `cordova emulate windows -- --phone`</span></span>

   <span data-ttu-id="4792e-238">Használjon `cordova run windows --list -- --phone` toosee összes elérhető cél és `cordova run windows --target=<target_name> -- --phone` toorun hello alkalmazás egy adott eszközt vagy emulátort (például `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span><span class="sxs-lookup"><span data-stu-id="4792e-238">Use `cordova run windows --list -- --phone` toosee all available targets and `cordova run windows --target=<target_name> -- --phone` toorun hello application on a specific device or emulator (for example, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).</span></span>

### <a name="android"></a><span data-ttu-id="4792e-239">Android</span><span class="sxs-lookup"><span data-stu-id="4792e-239">Android</span></span>
   <span data-ttu-id="4792e-240">egy csatlakoztatott eszközön toorun:`cordova run android --device`</span><span class="sxs-lookup"><span data-stu-id="4792e-240">toorun on a connected device: `cordova run android --device`</span></span>

   <span data-ttu-id="4792e-241">hello alapértelmezett emulátor toorun:`cordova emulate android`</span><span class="sxs-lookup"><span data-stu-id="4792e-241">toorun on hello default emulator: `cordova emulate android`</span></span>

   <span data-ttu-id="4792e-242">Ellenőrizze, hogy létrehozott egy emulátor példány AVD Manager használatával hello "Előfeltételek" szakaszban korábban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="4792e-242">Make sure you've created an emulator instance by using AVD Manager, as described earlier in hello "Prerequisites" section.</span></span>

   <span data-ttu-id="4792e-243">Használjon `cordova run android --list` toosee összes elérhető cél és `cordova run android --target=<target_name>` toorun hello alkalmazás egy adott eszközt vagy emulátort (például `cordova run android --target="Nexus4_emulator"`).</span><span class="sxs-lookup"><span data-stu-id="4792e-243">Use `cordova run android --list` toosee all available targets and `cordova run android --target=<target_name>` toorun hello application on a specific device or emulator (for example, `cordova run android --target="Nexus4_emulator"`).</span></span>

### <a name="ios"></a><span data-ttu-id="4792e-244">iOS</span><span class="sxs-lookup"><span data-stu-id="4792e-244">iOS</span></span>
   <span data-ttu-id="4792e-245">egy csatlakoztatott eszközön toorun:`cordova run ios --device`</span><span class="sxs-lookup"><span data-stu-id="4792e-245">toorun on a connected device: `cordova run ios --device`</span></span>

   <span data-ttu-id="4792e-246">hello alapértelmezett emulátor toorun:`cordova emulate ios`</span><span class="sxs-lookup"><span data-stu-id="4792e-246">toorun on hello default emulator: `cordova emulate ios`</span></span>

   > [!NOTE]
   > <span data-ttu-id="4792e-247">Győződjön meg arról, hogy hello `ios-sim` telepített csomag toorun hello emulátorának.</span><span class="sxs-lookup"><span data-stu-id="4792e-247">Make sure you have hello `ios-sim` package installed toorun on hello emulator.</span></span> <span data-ttu-id="4792e-248">További információkért tekintse meg a "Hello"Előfeltételek"szakaszában.</span><span class="sxs-lookup"><span data-stu-id="4792e-248">For more information, see hello "Prerequisites" section.</span></span>

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a><span data-ttu-id="4792e-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4792e-249">Next steps</span></span>
<span data-ttu-id="4792e-250">Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span><span class="sxs-lookup"><span data-stu-id="4792e-250">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).</span></span>

<span data-ttu-id="4792e-251">Most áthelyezés speciális toomore (és további érdekes) forgatókönyvek is.</span><span class="sxs-lookup"><span data-stu-id="4792e-251">You can now move on toomore advanced (and more interesting) scenarios.</span></span> <span data-ttu-id="4792e-252">Érdemes lehet tootry: [egy Node.js webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4792e-252">You might want tootry: [Secure a Node.js Web API with Azure AD](active-directory-devquickstarts-webapi-nodejs.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
