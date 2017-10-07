---
title: "aaaHow toowork hello Node.js háttérrendszer kiszolgálóval a Mobile Apps SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toowork az Azure App Service Mobile Apps Node.js háttérkiszolgáló SDK hello."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="dc0c5-103">Hogyan toouse hello Azure Mobile Apps Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="dc0c5-103">How toouse hello Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="dc0c5-104">Ez a cikk ismerteti a részletes útmutatást és példákat megjelenítő hogyan toowork az Azure App Service Mobile Apps a Node.js-háttéralkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-104">This article provides detailed information and examples showing how toowork with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="dc0c5-105"><a name="Introduction"></a>Bevezetés</span><span class="sxs-lookup"><span data-stu-id="dc0c5-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="dc0c5-106">Az Azure App Service Mobile Apps hello funkció mobile optimalizált tooadd adatelérési Web API tooa webalkalmazás biztosít.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-106">Azure App Service Mobile Apps provides hello capability tooadd a mobile-optimized data access Web API tooa web application.</span></span>  <span data-ttu-id="dc0c5-107">az ASP.NET és a Node.js webes alkalmazásokhoz, Azure App Service Mobile Apps SDK hello valósul meg.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-107">hello Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="dc0c5-108">hello SDK tartalmazza a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-108">hello SDK provides hello following operations:</span></span>

* <span data-ttu-id="dc0c5-109">Az adatelérési tábla műveletek (olvasás, Insert, Update, Delete)</span><span class="sxs-lookup"><span data-stu-id="dc0c5-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="dc0c5-110">Egyéni API-műveletek</span><span class="sxs-lookup"><span data-stu-id="dc0c5-110">Custom API operations</span></span>

<span data-ttu-id="dc0c5-111">Mindkét műveletek Azure App Service, beleértve a közösségi Identitásszolgáltatók például a Facebookhoz, Twitter, Google és a Microsoft, valamint Azure Active Directory vállalati identitás által engedélyezett összes Identitásszolgáltatók biztosít a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="dc0c5-112">Minták minden egyes felhasználási eseténél a hello található [minták directory a Githubon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-112">You can find samples for each use case in hello [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="dc0c5-113">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="dc0c5-113">Supported Platforms</span></span>
<span data-ttu-id="dc0c5-114">hello Azure Mobile Apps csomópont SDK aktuális LTS kiadási csomópont, és később hello támogatja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-114">hello Azure Mobile Apps Node SDK supports hello current LTS release of Node and later.</span></span>  <span data-ttu-id="dc0c5-115">Től írást, hello legújabb LTS verzió a csomópont v4.5.0.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-115">As of writing, hello latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="dc0c5-116">A csomópont más verzióiban előfordulhat, hogy működik, de nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="dc0c5-117">Azure Mobile Apps csomópont SDK hello támogatja a két adatbázis illesztőprogram - hello csomópont-mssql illesztőprogram támogatja az SQL Azure és a helyi SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-117">hello Azure Mobile Apps Node SDK supports two database drivers - hello node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="dc0c5-118">hello sqlite3 illesztőprogram csak egyetlen példányán SQLite adatbázisokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-118">hello sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="dc0c5-119"><a name="howto-cmdline-basicapp"></a>Hogyan: hozzon létre egy alapszintű Node.js-háttéralkalmazáshoz hello parancssor használatával</span><span class="sxs-lookup"><span data-stu-id="dc0c5-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using hello Command Line</span></span>
<span data-ttu-id="dc0c5-120">Minden Azure App Service Mobile App Node.js-háttéralkalmazáshoz ExpressJS alkalmazásként indul.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="dc0c5-121">ExpressJS hello legnépszerűbb web service keretrendszer Node.js érhető el.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-121">ExpressJS is hello most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="dc0c5-122">Létrehozhat egy alapszintű [Express] alkalmazás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="dc0c5-123">Egy parancs vagy a PowerShell-ablakot hozzon létre egy könyvtárat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="dc0c5-124">Futtassa az npm init tooinitialize hello csomag struktúra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-124">Run npm init tooinitialize hello package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="dc0c5-125">hello npm init parancs kérdések tooinitialize hello projekt készlete kéri.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-125">hello npm init command asks a set of questions tooinitialize hello project.</span></span>  <span data-ttu-id="dc0c5-126">Lásd: hello egy példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-126">See hello example output:</span></span>

    ![hello npm init kimeneti][0]
3. <span data-ttu-id="dc0c5-128">Telepítse a hello expressz és az azure-mobilalkalmazások szalagtárak adattárból hello npm.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-128">Install hello express and azure-mobile-apps libraries from hello npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="dc0c5-129">Az app.js fájl tooimplement hello alapvető mobil kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-129">Create an app.js file tooimplement hello basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="dc0c5-130">Ez az alkalmazás egy végpontot hoz létre egy mobile optimalizált WebAPI (`/tables/TodoItem`), amely biztosítja, nem hitelesített hozzáférést tooan az alapul szolgáló SQL-adattár egy dinamikus sémáját használja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access tooan underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="dc0c5-131">Alkalmas az ügyfél szalagtár gyors üzembe helyezések a következő:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="dc0c5-132">[Android ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-133">[Apache Cordova-ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-134">[iOS ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-135">[A Windows Store ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-136">[Xamarin.iOS ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-137">[Xamarin.Android ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="dc0c5-138">[Xamarin.Forms-ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="dc0c5-139">Ezen alapszintű hello alkalmazás hello kód található [basicapp mintát a Githubon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-139">You can find hello code for this basic application in hello [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="dc0c5-140"><a name="howto-vs2015-basicapp"></a>Útmutató: a Visual Studio 2015 csomópont-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="dc0c5-141">Visual Studio 2015-öt egy bővítmény toodevelop Node.js-alkalmazások hello IDE belül van szükség.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-141">Visual Studio 2015 requires an extension toodevelop Node.js applications within hello IDE.</span></span>  <span data-ttu-id="dc0c5-142">toostart, telepítés hello [Node.js eszközök 1.1 a Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-142">toostart, install hello [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="dc0c5-143">Hello Node.js Tools for Visual Studio telepítése után hozzon létre egy Express 4.x alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-143">Once hello Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="dc0c5-144">Nyissa meg hello **új projekt** párbeszédpanelen (a **fájl** > **új** > **projekt …** ).</span><span class="sxs-lookup"><span data-stu-id="dc0c5-144">Open hello **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="dc0c5-145">Bontsa ki a **sablonok** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="dc0c5-146">Jelölje be hello **alapszintű Azure Node.js Express 4 Application**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-146">Select hello **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="dc0c5-147">Töltse ki hello projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-147">Fill in hello project name.</span></span>  <span data-ttu-id="dc0c5-148">Kattintson az *OK* gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-148">Click *OK*.</span></span>

    ![Visual Studio 2015-öt új projekt][1]
5. <span data-ttu-id="dc0c5-150">Kattintson a jobb gombbal hello **npm** csomópont, és válassza **telepítése új npm csomagok...** .</span><span class="sxs-lookup"><span data-stu-id="dc0c5-150">Right-click hello **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="dc0c5-151">Szükség lehet toorefresh hello npm katalógus az első Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-151">You may need toorefresh hello npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="dc0c5-152">Kattintson a **frissítése** szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="dc0c5-153">Adja meg *azure-mobilalkalmazások* hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-153">Enter *azure-mobile-apps* in hello search box.</span></span>  <span data-ttu-id="dc0c5-154">Kattintson a hello **azure-mobileszköz-alkalmazások 2.0.0** csomagot, majd kattintson az **csomagtelepítés**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-154">Click hello **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Új npm-csomagok][2]
8. <span data-ttu-id="dc0c5-156">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-156">Click **Close**.</span></span>
9. <span data-ttu-id="dc0c5-157">Nyissa meg hello *app.js* tooadd támogatás az Azure Mobile Apps SDK hello.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-157">Open hello *app.js* file tooadd support for hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="dc0c5-158">Sor 6 at hello aljához hello könyvtár utasítás szükséges, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-158">At line 6 at hello bottom of hello library require statements, add hello following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="dc0c5-159">Egyéb app.use utasítások körülbelül sor 27 hello után adja hozzá a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-159">At approximately line 27 after hello other app.use statements, add hello following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="dc0c5-160">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-160">Save hello file.</span></span>
10. <span data-ttu-id="dc0c5-161">Futtassa helyileg hello alkalmazást (hello API kiszolgált a http://localhost: 3000), vagy tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-161">Either run hello application locally (hello API is served on http://localhost:3000) or publish tooAzure.</span></span>

### <span data-ttu-id="dc0c5-162"><a name="create-node-backend-portal"></a>Útmutató: Azure-portálon hello használata Node.js-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using hello Azure portal</span></span>
<span data-ttu-id="dc0c5-163">Létrehozhat egy mobilalkalmazás-háttérrendszer jobb hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-163">You can create a Mobile App backend right in hello [Azure portal].</span></span> <span data-ttu-id="dc0c5-164">Következő lépések, vagy hozzon létre egy ügyfél és kiszolgáló együtt a következő hello hello vagy követésével [mobilalkalmazás létrehozása](app-service-mobile-ios-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-164">You can either follow hello following steps or create a client and server together by following hello [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="dc0c5-165">hello oktatóanyag tartalmazza ezeket az utasításokat egy egyszerűsített verziója és a projektek a koncepció igazolása.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-165">hello tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="dc0c5-166">Vissza a hello *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **Node.js** , a **háttéralkalmazás-nyelv**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-166">Back in hello *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="dc0c5-167">Hello jelölőnégyzetet, a "**tudomásul veszem, hogy ezzel a művelettel felülírja az összes hely tartalmának.**", majd kattintson a **TodoItem tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-167">Check hello box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="dc0c5-168"><a name="download-quickstart"></a>Hogyan: letöltési hello Node.js háttérrendszer gyors üzembe helyezés kód projekt Git használatával</span><span class="sxs-lookup"><span data-stu-id="dc0c5-168"><a name="download-quickstart"></a>How to: Download hello Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="dc0c5-169">Ha Node.js Mobile Apps-háttéralkalmazás létrehozása hello portál használatával **gyors üzembe helyezési** panelen, egy Node.js-projektet hoz létre, és központilag telepített tooyour hely.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-169">When you create a Node.js Mobile App backend by using hello portal **Quick start** blade, a Node.js project is created for you and deployed tooyour site.</span></span> <span data-ttu-id="dc0c5-170">Adja hozzá a táblák és API-kat, és Node.js-háttéralkalmazáshoz hello hello portálon tartozó kódfájlok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-170">You can add tables and APIs and edit code files for hello Node.js backend in hello portal.</span></span> <span data-ttu-id="dc0c5-171">Különböző központi telepítési eszközök toodownload hello háttéralkalmazás-projekt, hogy hozzáadása vagy módosítása a táblák és API-k, majd közzé hello projekt is használható.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-171">You can also use various deployment tools toodownload hello backend project so that you can add or modify tables and APIs, then republish hello project.</span></span> <span data-ttu-id="dc0c5-172">További információkért lásd: a [Azure App Service telepítési útmutató].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="dc0c5-173">hello következő eljárás használja a Git tárház toodownload hello gyors üzembe helyezési projekt kódját.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-173">hello following procedure uses a Git repository toodownload hello quickstart project code.</span></span>

1. <span data-ttu-id="dc0c5-174">Ha még nem tette, telepítse a Git esetében.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="dc0c5-175">hello lépéseket szükséges tooinstall Git eltérő operációs rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-175">hello steps required tooinstall Git vary between operating systems.</span></span> <span data-ttu-id="dc0c5-176">Lásd: [telepítése Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) az operációsrendszer-specifikus disztribúcióiról, valamint a telepítési útmutatót.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="dc0c5-177">Hello kövesse [engedélyezése hello App Service alkalmazás tárház](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git-tárház háttér webhelyét, és jegyezze fel hello telepítési felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-177">Follow hello steps in [Enable hello App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git repository for your backend site, making a note of hello deployment username and password.</span></span>
3. <span data-ttu-id="dc0c5-178">A mobilalkalmazás háttérrendszerének hello panelen, jegyezze fel a hello **Git-klón URL-cím** beállítást.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-178">In hello blade for your Mobile App backend, make a note of hello **Git clone URL** setting.</span></span>
4. <span data-ttu-id="dc0c5-179">Hello végrehajtása `git clone` parancs használatával hello Git klón URL-címet írja be a jelszót, ha szükséges, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-179">Execute hello `git clone` command using hello Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="dc0c5-180">Tallózás toolocal könyvtár, amely a fenti példa hello /todolist, és figyelje meg, hogy a projektfájlok le vannak töltve.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-180">Browse toolocal directory, which in hello preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="dc0c5-181">Keresse meg a hello `todoitem.json` hello fájlban `/tables` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-181">Locate hello `todoitem.json` file in hello `/tables` directory.</span></span>  <span data-ttu-id="dc0c5-182">Ez a fájl engedélyeit a tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="dc0c5-183">Is megkeresi a hello `todoitem.js` hello fájlt azonos könyvtárban, amely meghatározza, hogy a CRUD művelet parancsfájlok hello tábla.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-183">Also find hello `todoitem.js` file in hello same directory, which defines that CRUD operation scripts for hello table.</span></span>
6. <span data-ttu-id="dc0c5-184">Módosításokat végzett tooproject fájlokat, miután hajtható végre a következő parancsok tooadd, véglegesítése, majd töltse fel a változások toohello hely hello:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-184">After you have made changes tooproject files, execute hello following commands tooadd, commit, then upload the changes toohello site:</span></span>

        $ git commit -m "updated hello table script"
        $ git push origin master

    <span data-ttu-id="dc0c5-185">Amikor új fájlok toohello projekt, először tooexecute hello `git add .` parancsot.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-185">When you add new files toohello project, you first need tooexecute hello `git add .` command.</span></span>

<span data-ttu-id="dc0c5-186">hello webhely minden alkalommal, amikor véglegesíti az új készlet fejlesztőre toohello webhely ismételt közzététele.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-186">hello site is republished every time a new set of commits is pushed toohello site.</span></span>

### <span data-ttu-id="dc0c5-187"><a name="howto-publish-to-azure"></a>Útmutató: a Node.js háttérrendszer tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="dc0c5-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend tooAzure</span></span>
<span data-ttu-id="dc0c5-188">Microsoft Azure közzététel az Azure App Service Mobile Apps Node.js-háttéralkalmazását az Azure szolgáltatás hello számos mechanizmust biztosít.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to hello Azure service.</span></span>  <span data-ttu-id="dc0c5-189">Ezek közé tartozik a központi telepítési eszközöket a Visual Studio integrált, parancssori eszközök és verziókövetési alapuló folyamatos üzembe helyezés beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="dc0c5-190">A témakörrel kapcsolatos további információkért lásd: a [Azure App Service telepítési útmutató].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="dc0c5-191">Az Azure App Service a Node.js-alkalmazás üzembe helyezése előtt tekintse át a konkrét útmutatásért rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="dc0c5-192">Hogyan túl[hello csomópont verzió megadása]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-192">How too[specify hello Node Version]</span></span>
* <span data-ttu-id="dc0c5-193">Hogyan túl[csomópont modulok használata]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-193">How too[use Node modules]</span></span>

### <span data-ttu-id="dc0c5-194"><a name="howto-enable-homepage"></a>Útmutató: az alkalmazás kezdőlapját engedélyezése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="dc0c5-195">Számos alkalmazás webes és mobilalkalmazások és hello ExpressJS keretrendszer lehetővé teszi a két toocombine értékkorlátozást.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-195">Many applications are a combination of web and mobile apps and hello ExpressJS framework allows you toocombine the two facets.</span></span>  <span data-ttu-id="dc0c5-196">Egyes esetekben azonban Kezdésként tooonly alkalmazzon egy mobil felületet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-196">Sometimes, however, you may wish tooonly implement a mobile interface.</span></span>  <span data-ttu-id="dc0c5-197">Egy követően lap tooensure hello alkalmazásszolgáltatás megfelelően működik, és hasznos tooprovide.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-197">It is useful tooprovide a landing page tooensure hello app service is up and running.</span></span>  <span data-ttu-id="dc0c5-198">Adja meg a saját kezdőlapján, vagy egy ideiglenes kezdőlap engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="dc0c5-199">egy ideiglenes kezdőlapján tooenable hello tooinstantiate Azure Mobile Apps a következő használja:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-199">tooenable a temporary home page, use hello following tooinstantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="dc0c5-200">Ha csak ez a beállítás érhető el helyi fejlesztésekor, ez a beállítás tooyour adhat hozzá `azureMobile.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-200">If you only want this option available when developing locally, you can add this setting tooyour `azureMobile.js` file.</span></span>

## <span data-ttu-id="dc0c5-201"><a name="TableOperations"></a>Tábla műveletek</span><span class="sxs-lookup"><span data-stu-id="dc0c5-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="dc0c5-202">hello azure-mobilalkalmazások Node.js Server SDK mechanizmusok tooexpose adatokat biztosít az Azure SQL Database, a WebAPI tárolja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-202">hello azure-mobile-apps Node.js Server SDK provides mechanisms tooexpose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="dc0c5-203">Öt műveleti rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-203">Five operations are provided.</span></span>

| <span data-ttu-id="dc0c5-204">Művelet</span><span class="sxs-lookup"><span data-stu-id="dc0c5-204">Operation</span></span> | <span data-ttu-id="dc0c5-205">Leírás</span><span class="sxs-lookup"><span data-stu-id="dc0c5-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dc0c5-206">GET /tables/*táblanév*</span><span class="sxs-lookup"><span data-stu-id="dc0c5-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="dc0c5-207">Az összes rekord hello tábla beolvasása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-207">Get all records in hello table</span></span> |
| <span data-ttu-id="dc0c5-208">GET /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="dc0c5-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="dc0c5-209">Egy megadott rekord hello tábla beolvasása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-209">Get a specific record in hello table</span></span> |
| <span data-ttu-id="dc0c5-210">POST /tables/*táblanév*</span><span class="sxs-lookup"><span data-stu-id="dc0c5-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="dc0c5-211">Hozzon létre egy bejegyzést a hello tábla</span><span class="sxs-lookup"><span data-stu-id="dc0c5-211">Create a record in hello table</span></span> |
| <span data-ttu-id="dc0c5-212">JAVÍTÁS /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="dc0c5-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="dc0c5-213">Módosítani egy rekordot hello tábla</span><span class="sxs-lookup"><span data-stu-id="dc0c5-213">Update a record in hello table</span></span> |
| <span data-ttu-id="dc0c5-214">TÖRLÉS /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="dc0c5-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="dc0c5-215">Hello tábla rekord törlése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-215">Delete a record in hello table</span></span> |

<span data-ttu-id="dc0c5-216">Támogatja a WebAPI [OData] és kiterjesztik az hello tábla séma toosupport [offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-216">This WebAPI supports [OData] and extends hello table schema toosupport [offline data sync].</span></span>

### <span data-ttu-id="dc0c5-217"><a name="howto-dynamicschema"></a>Hogyan: Adja meg a táblák egy dinamikus séma használata</span><span class="sxs-lookup"><span data-stu-id="dc0c5-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="dc0c5-218">Egy tábla használt, meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="dc0c5-219">Táblák meghatározása (ahol hello fejlesztői meghatározása hello oszlopok hello sémáján belül) statikus séma vagy dinamikusan (ahol hello SDK szabályozza a bejövő kérések alapján hello séma).</span><span class="sxs-lookup"><span data-stu-id="dc0c5-219">Tables can be defined with a static schema (where hello developer defines hello columns within hello schema) or dynamically (where hello SDK controls hello schema based on incoming requests).</span></span> <span data-ttu-id="dc0c5-220">Ezenkívül hello fejlesztői irányítani tudja adott aspektusait hello WebAPI Javascript-kód toohello definition hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-220">In addition, hello developer can control specific aspects of hello WebAPI by adding Javascript code toohello definition.</span></span>

<span data-ttu-id="dc0c5-221">Ajánlott eljárásként akkor kell mindegyik tábla hello táblák könyvtárban Javascript-fájlt ad meg, majd a tables.import() metódus tooimport hello táblázatot használnak.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-221">As a best practice, you should define each table in a Javascript file in hello tables directory, then use the tables.import() method tooimport hello tables.</span></span>  <span data-ttu-id="dc0c5-222">Hello basic alkalmazáson belüli kiterjesztése esetén hello app.js fájl ki kell igazítani:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-222">Extending hello basic-app, hello app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="dc0c5-223">Adja meg a hello tábla. vagy tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-223">Define hello table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

<span data-ttu-id="dc0c5-224">Táblák alapértelmezés szerint dinamikus séma használja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="dc0c5-225">dinamikus séma ki tooturn globálisan, állítsa be a hello Alkalmazásbeállítás **MS_DynamicSchema** toofalse hello Azure-portálon belül.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-225">tooturn off dynamic schema globally, set hello App Setting **MS_DynamicSchema** toofalse within hello Azure portal.</span></span>

<span data-ttu-id="dc0c5-226">Teljes példa található hello [todo mintát a Githubon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-226">You can find a complete example in hello [todo sample on GitHub].</span></span>

### <span data-ttu-id="dc0c5-227"><a name="howto-staticschema"></a>Hogyan: Adja meg a táblák egy statikus séma használata</span><span class="sxs-lookup"><span data-stu-id="dc0c5-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="dc0c5-228">Explicit módon meghatározhatja hello oszlopok tooexpose hello WebAPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-228">You can explicitly define hello columns tooexpose via hello WebAPI.</span></span>  <span data-ttu-id="dc0c5-229">hello azure-mobilalkalmazások Node.js SDK automatikusan hozzáadja a kapcsolat nélküli szinkronizálás toohello lista, amely megadja a szükséges további oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-229">hello azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync toohello list that you provide.</span></span>  <span data-ttu-id="dc0c5-230">Például a gyors üzembe helyezés ügyfélalkalmazások szükséges két oszlopokkal rendelkező táblát: text (szöveg) (logikai) hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="dc0c5-231">hello tábla hello tábla definícióját JavaScript-fájlt (hello táblák könyvtárában található) az alábbiak szerint lehet meghatározni:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-231">hello table can be defined in hello table definition JavaScript file (located in hello tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="dc0c5-232">Ha statikusan határozza meg a táblák, majd kell is meghívja a hello tables.initialize() metódus toocreate hello adatbázisséma indításakor.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-232">If you define tables statically, then you must also call hello tables.initialize() method toocreate hello database schema on startup.</span></span>  <span data-ttu-id="dc0c5-233">hello tables.initialize() metódus visszaadja a [ígéret] , hogy hello webszolgáltatás nem teljesíti a kérelmeket hello adatbázis inicializálása előtt.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-233">hello tables.initialize() method returns a [Promise] so that hello web service does not serve requests before hello database being initialized.</span></span>

### <span data-ttu-id="dc0c5-234"><a name="howto-sqlexpress-setup"></a>Hogyan: használja az SQL Express fejlesztési adattárként a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="dc0c5-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="dc0c5-235">hello Azure Mobile Apps hello AzureMobile alkalmazások csomópont SDK három lehetőséget kínál a kiszolgáló hello kezdő verzióról: SDK hello kezdő verzióról kiszolgáló három lehetőséget biztosít:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-235">hello Azure Mobile Apps hello AzureMobile Apps Node SDK provides three options for serving data out of hello box: SDK provides three options for serving data out of hello box:</span></span>

* <span data-ttu-id="dc0c5-236">Használjon hello **memória** illesztőprogramtárába egy nem állandó tooprovide – példa</span><span class="sxs-lookup"><span data-stu-id="dc0c5-236">Use hello **memory** driver tooprovide a non-persistent example store</span></span>
* <span data-ttu-id="dc0c5-237">Használjon hello **mssql** illesztőprogram tooprovide fejlesztési SQL Express adattárat</span><span class="sxs-lookup"><span data-stu-id="dc0c5-237">Use hello **mssql** driver tooprovide a SQL Express data store for development</span></span>
* <span data-ttu-id="dc0c5-238">Használjon hello **mssql** illesztőprogram tooprovide egy Azure SQL Database adattároló termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="dc0c5-238">Use hello **mssql** driver tooprovide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="dc0c5-239">hello Azure Mobile Apps Node.js SDK-t használ a hello [mssql Node.js csomag] tooestablish és az SQL Express kapcsolat tooboth és SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-239">hello Azure Mobile Apps Node.js SDK uses hello [mssql Node.js package] tooestablish and use a connection tooboth SQL Express and SQL Database.</span></span>  <span data-ttu-id="dc0c5-240">A csomag igényli-e, hogy az SQL Express-példány TCP-kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="dc0c5-241">hello memória illesztőprogram nem biztosít tesztelési létesítményekben teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-241">hello memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="dc0c5-242">Ha a tootest a háttéralkalmazást helyileg, azt egy SQL Express adattárból hello használatát javasoljuk, és hello mssql illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-242">If you wish tootest your backend locally, we recommend hello use of a SQL Express data store and hello mssql driver.</span></span>
>
>

1. <span data-ttu-id="dc0c5-243">Töltse le és telepítse [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="dc0c5-244">Ügyeljen arra, SQL Server 2014 Express hello eszközök kiadásával telepítse.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-244">Ensure you install hello SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="dc0c5-245">Ha 64 bites támogatás kifejezetten szüksége hello 32 bites verziója kevesebb memóriát igényel, futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-245">Unless you explicitly require 64-bit support, hello 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="dc0c5-246">Futtassa az SQL Server 2014 Configuration Manager hello.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-246">Run hello SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="dc0c5-247">Bontsa ki a hello **SQL Server hálózati konfigurációja** hello bal oldali fában menü csomópontja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-247">Expand hello **SQL Server Network Configuration** node in hello left-hand tree menu.</span></span>
   2. <span data-ttu-id="dc0c5-248">Kattintson a **SQLEXPRESS protokollok**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="dc0c5-249">Kattintson a jobb gombbal **TCP/IP** válassza **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="dc0c5-250">Kattintson a **OK** hello előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-250">Click **OK** in hello pop-up dialog.</span></span>
   4. <span data-ttu-id="dc0c5-251">Kattintson a jobb gombbal **TCP/IP** válassza **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="dc0c5-252">Kattintson a hello **IP-címek** fülre.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-252">Click hello **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="dc0c5-253">Hello található **IPAll** csomópont.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-253">Find hello **IPAll** node.</span></span>  <span data-ttu-id="dc0c5-254">A hello **TCP-Port** mezőbe írja be **1433**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-254">In hello **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="dc0c5-255">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-255">Click **OK**.</span></span>  <span data-ttu-id="dc0c5-256">Kattintson a **OK** hello előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-256">Click **OK** in hello pop-up dialog.</span></span>
   8. <span data-ttu-id="dc0c5-257">Kattintson a **SQL Server Services** hello bal oldali fában menüben.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-257">Click **SQL Server Services** in hello left-hand tree menu.</span></span>
   9. <span data-ttu-id="dc0c5-258">Kattintson a jobb gombbal **SQL Server (SQLEXPRESS)** válassza **újraindítása**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="dc0c5-259">Zárja be az SQL Server 2014 Configuration Manager hello.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-259">Close hello SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="dc0c5-260">Futtassa az SQL Server 2014 Management Studio hello, és csatlakozzon a helyi SQL Express példány tooyour</span><span class="sxs-lookup"><span data-stu-id="dc0c5-260">Run hello SQL Server 2014 Management Studio and connect tooyour local SQL Express instance</span></span>

   1. <span data-ttu-id="dc0c5-261">Kattintson a jobb gombbal az Object Explorer hello a példányát, és válassza ki **tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-261">Right-click your instance in hello Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="dc0c5-262">Jelölje be hello **biztonsági** lap.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-262">Select hello **Security** page.</span></span>
   3. <span data-ttu-id="dc0c5-263">Győződjön meg arról hello **SQL Server és a Windows-hitelesítés üzemmód** van kijelölve</span><span class="sxs-lookup"><span data-stu-id="dc0c5-263">Ensure hello **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="dc0c5-264">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="dc0c5-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="dc0c5-265">Bontsa ki a **biztonsági** > **bejelentkezések** az Object Explorer hello</span><span class="sxs-lookup"><span data-stu-id="dc0c5-265">Expand **Security** > **Logins** in hello Object Explorer</span></span>
   6. <span data-ttu-id="dc0c5-266">Kattintson a jobb gombbal **bejelentkezések** válassza **új bejelentkezés...**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="dc0c5-267">Adja meg a bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-267">Enter a Login name.</span></span>  <span data-ttu-id="dc0c5-268">Kattintson az **SQL Server-hitelesítés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="dc0c5-269">Adjon meg egy jelszót, majd adja meg a hello ugyanezt a jelszót **jelszó megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-269">Enter a Password, then enter hello same password in **Confirm password**.</span></span>  <span data-ttu-id="dc0c5-270">hello jelszónak meg kell felelnie a Windows bonyolultsági követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-270">hello password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="dc0c5-271">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="dc0c5-271">Click **OK**</span></span>

          ![Add a new user tooSQL Express][5]
   9. <span data-ttu-id="dc0c5-272">Kattintson a jobb gombbal az új bejelentkezési adatait, és válassza ki **tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="dc0c5-273">Jelölje be hello **kiszolgálói szerepkörök** lap</span><span class="sxs-lookup"><span data-stu-id="dc0c5-273">Select hello **Server Roles** page</span></span>
   11. <span data-ttu-id="dc0c5-274">Ellenőrizze hello mezőben következő toohello **dbcreator** kiszolgálói szerepkör</span><span class="sxs-lookup"><span data-stu-id="dc0c5-274">Check hello box next toohello **dbcreator** server role</span></span>
   12. <span data-ttu-id="dc0c5-275">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="dc0c5-275">Click **OK**</span></span>
   13. <span data-ttu-id="dc0c5-276">Zárja be az SQL Server 2015 Management Studio hello</span><span class="sxs-lookup"><span data-stu-id="dc0c5-276">Close hello SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="dc0c5-277">Győződjön meg arról, hogy rekord hello felhasználónév és jelszó választotta.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-277">Ensure you record hello username and password you selected.</span></span>  <span data-ttu-id="dc0c5-278">A megadott adatbázis követelményeitől függően esetleg tooassign további kiszolgálói szerepkörök vagy engedélyek.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-278">You may need tooassign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="dc0c5-279">Node.js-alkalmazás hello beolvassa hello **SQLCONNSTR_MS_TableConnectionString** környezeti változó hello kapcsolati karakterláncot kell megadnia ehhez az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-279">hello Node.js application reads hello **SQLCONNSTR_MS_TableConnectionString** environment variable for hello connection string for this database.</span></span>  <span data-ttu-id="dc0c5-280">A változó beállított a környezetben.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="dc0c5-281">Például használhatja PowerShell tooset e környezeti változó:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-281">For example, you can use PowerShell tooset this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="dc0c5-282">TCP/IP-kapcsolaton keresztül érik el hello adatbázist, és adjon meg egy felhasználónevet és jelszót hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-282">Access hello database through a TCP/IP connection and provide a username and password for hello connection.</span></span>

### <span data-ttu-id="dc0c5-283"><a name="howto-config-localdev"></a>Útmutató: a helyi fejlesztési projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="dc0c5-284">Az Azure Mobile Apps beolvassa a JavaScript-fájl neve *azureMobile.js* hello helyi FileSystem.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from hello local filesystem.</span></span>  <span data-ttu-id="dc0c5-285">Nem használja a fájl tooconfigure hello Azure Mobile Apps SDK üzemi - Alkalmazásbeállítások használható hello [Azure-portálon] helyette.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-285">Do not use this file tooconfigure hello Azure Mobile Apps SDK in production - use App Settings within hello [Azure portal] instead.</span></span>  <span data-ttu-id="dc0c5-286">Hello *azureMobile.js* fájlba exportálja a konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-286">hello *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="dc0c5-287">hello leggyakrabban használt beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-287">hello most common settings are:</span></span>

* <span data-ttu-id="dc0c5-288">Adatbázis-beállítások</span><span class="sxs-lookup"><span data-stu-id="dc0c5-288">Database Settings</span></span>
* <span data-ttu-id="dc0c5-289">Diagnosztikai naplózás beállításait</span><span class="sxs-lookup"><span data-stu-id="dc0c5-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="dc0c5-290">Alternatív CORS-beállítások</span><span class="sxs-lookup"><span data-stu-id="dc0c5-290">Alternate CORS Settings</span></span>

<span data-ttu-id="dc0c5-291">Példa *azureMobile.js* fájl végrehajtási hello megelőző adatbázis beállítások követi:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-291">An example *azureMobile.js* file implementing hello preceding database settings follows:</span></span>

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

<span data-ttu-id="dc0c5-292">Javasoljuk, hogy adja hozzá *azureMobile.js* tooyour *.gitignore* fájlt (vagy más verziókövetési figyelmen kívül hagyja a fájlt) hello felhőben tárolt tooprevent jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-292">We recommend that you add *azureMobile.js* tooyour *.gitignore* file (or other source code control ignore file) tooprevent passwords from being stored in hello cloud.</span></span>  <span data-ttu-id="dc0c5-293">Mindig adja meg az üzemi beállításait Alkalmazásbeállítások belül hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-293">Always configure production settings in App Settings within hello [Azure portal].</span></span>

### <span data-ttu-id="dc0c5-294"><a name="howto-appsettings"></a>Útmutató: A mobilalkalmazás-beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="dc0c5-295">A legtöbb beállításai a hello *azureMobile.js* fájl rendelkezik egyenértékű Alkalmazásbeállítás hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-295">Most settings in hello *azureMobile.js* file have an equivalent App Setting in hello [Azure portal].</span></span>  <span data-ttu-id="dc0c5-296">A következő lista tooconfigure hello az alkalmazás használata beállítást:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-296">Use hello following list tooconfigure your app in App Settings:</span></span>

| <span data-ttu-id="dc0c5-297">Alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-297">App Setting</span></span> | <span data-ttu-id="dc0c5-298">*azureMobile.js* beállítás</span><span class="sxs-lookup"><span data-stu-id="dc0c5-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="dc0c5-299">Leírás</span><span class="sxs-lookup"><span data-stu-id="dc0c5-299">Description</span></span> | <span data-ttu-id="dc0c5-300">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="dc0c5-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="dc0c5-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="dc0c5-302">név</span><span class="sxs-lookup"><span data-stu-id="dc0c5-302">name</span></span> |<span data-ttu-id="dc0c5-303">hello hello alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="dc0c5-303">hello name of hello app</span></span> |<span data-ttu-id="dc0c5-304">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="dc0c5-304">string</span></span> |
| <span data-ttu-id="dc0c5-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="dc0c5-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="dc0c5-306">logging.level</span></span> |<span data-ttu-id="dc0c5-307">Az üzenetek toolog minimális naplózási szint</span><span class="sxs-lookup"><span data-stu-id="dc0c5-307">Minimum log level of messages toolog</span></span> |<span data-ttu-id="dc0c5-308">Hiba, figyelmeztetés, információ, részletes, hibakeresési silly</span><span class="sxs-lookup"><span data-stu-id="dc0c5-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="dc0c5-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-309">**MS_DebugMode**</span></span> |<span data-ttu-id="dc0c5-310">Hibakeresési</span><span class="sxs-lookup"><span data-stu-id="dc0c5-310">debug</span></span> |<span data-ttu-id="dc0c5-311">Engedélyezés vagy letiltás hibakeresési mód</span><span class="sxs-lookup"><span data-stu-id="dc0c5-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="dc0c5-312">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="dc0c5-312">true, false</span></span> |
| <span data-ttu-id="dc0c5-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-313">**MS_TableSchema**</span></span> |<span data-ttu-id="dc0c5-314">Data.Schema</span><span class="sxs-lookup"><span data-stu-id="dc0c5-314">data.schema</span></span> |<span data-ttu-id="dc0c5-315">SQL-táblák alapértelmezett séma neve</span><span class="sxs-lookup"><span data-stu-id="dc0c5-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="dc0c5-316">karakterlánc (alapértelmezett: dbo)</span><span class="sxs-lookup"><span data-stu-id="dc0c5-316">string (default: dbo)</span></span> |
| <span data-ttu-id="dc0c5-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="dc0c5-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="dc0c5-318">data.dynamicSchema</span></span> |<span data-ttu-id="dc0c5-319">Engedélyezés vagy letiltás hibakeresési mód</span><span class="sxs-lookup"><span data-stu-id="dc0c5-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="dc0c5-320">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="dc0c5-320">true, false</span></span> |
| <span data-ttu-id="dc0c5-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="dc0c5-322">verzió (set tooundefined)</span><span class="sxs-lookup"><span data-stu-id="dc0c5-322">version (set tooundefined)</span></span> |<span data-ttu-id="dc0c5-323">Letiltja az X-ZUMO-kiszolgálóverzió hello fejléc</span><span class="sxs-lookup"><span data-stu-id="dc0c5-323">Disables hello X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="dc0c5-324">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="dc0c5-324">true, false</span></span> |
| <span data-ttu-id="dc0c5-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="dc0c5-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="dc0c5-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="dc0c5-326">skipversioncheck</span></span> |<span data-ttu-id="dc0c5-327">Letiltja a hello ügyfél API-verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-327">Disables hello client API version check</span></span> |<span data-ttu-id="dc0c5-328">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="dc0c5-328">true, false</span></span> |

<span data-ttu-id="dc0c5-329">egy Alkalmazásbeállítás tooset:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-329">tooset an App Setting:</span></span>

1. <span data-ttu-id="dc0c5-330">Jelentkezzen be toohello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-330">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="dc0c5-331">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-331">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="dc0c5-332">Alapértelmezés szerint hello-beállítások panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-332">hello Settings blade opens by default.</span></span> <span data-ttu-id="dc0c5-333">Ha nem, kattintson a gombra **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="dc0c5-334">Kattintson a **Alkalmazásbeállítások** hello általános menüben.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-334">Click **Application settings** in hello GENERAL menu.</span></span>
5. <span data-ttu-id="dc0c5-335">Görgessen toohello App Settings szakasz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-335">Scroll toohello App Settings section.</span></span>
6. <span data-ttu-id="dc0c5-336">Ha az alkalmazás, beállítás már létezik, kattintson a hello app tooedit hello beállításérték hello értékét.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-336">If your app setting already exists, click hello value of hello app setting tooedit hello value.</span></span>
7. <span data-ttu-id="dc0c5-337">Ha az alkalmazás nem létezik, adja meg a hello Alkalmazásbeállítás hello kulcs és hello érték hello érték mezőbe.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-337">If your app setting does not exist, enter hello App Setting in hello Key box and hello value in hello Value box.</span></span>
8. <span data-ttu-id="dc0c5-338">Ha befejeződött, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="dc0c5-339">A legtöbb alkalmazás beállításainak módosítása a szolgáltatás újraindítását igényli.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="dc0c5-340"><a name="howto-use-sqlazure"></a>Útmutató: az üzemi adattároló SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="dc0c5-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="dc0c5-341">Az Azure SQL Database adattárként használata azonos Azure App Service-alkalmazás összes típusa.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="dc0c5-342">Ha még nem tette meg, kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-342">If you have not done so already, follow these steps toocreate a Mobile App backend.</span></span>

1. <span data-ttu-id="dc0c5-343">Jelentkezzen be toohello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-343">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="dc0c5-344">A hello bal felső részén hello ablak, kattintson a hello **+ új** gomb > **Web + mobil** > **mobilalkalmazás**, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-344">In hello top left of hello window, click hello **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="dc0c5-345">A hello **erőforráscsoport** mezőbe írja be a hello ugyanazt a nevet az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-345">In hello **Resource Group** box, enter hello same name as your app.</span></span>
4. <span data-ttu-id="dc0c5-346">hello alapértelmezett App Service-csomag van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-346">hello Default App Service plan is selected.</span></span>  <span data-ttu-id="dc0c5-347">Ha toochange App Service-csomag, azt is megteheti hello App Service-csomag kattintva > **+ új**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-347">If you wish toochange your App Service plan, you can do so by clicking hello App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="dc0c5-348">Adjon meg egy nevet az új App Service-csomag hello, és válasszon egy megfelelő helyet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-348">Provide a name of hello new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="dc0c5-349">Kattintson a hello tarifacsomag, és válasszon egy megfelelő tarifacsomagot hello szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-349">Click hello Pricing tier and select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="dc0c5-350">Válassza ki **összes** több, mint az beállítások árképzési tooview **szabad** és **megosztott**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-350">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="dc0c5-351">Miután kiválasztotta a tarifacsomagot, kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-351">Once you have selected the pricing tier, click hello **Select** button.</span></span>  <span data-ttu-id="dc0c5-352">Vissza a hello **App Service-csomag** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-352">Back in hello **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="dc0c5-353">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-353">Click **Create**.</span></span> <span data-ttu-id="dc0c5-354">A Mobile Apps-háttéralkalmazás kiépítés néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="dc0c5-355">Hello Mobile Apps-háttéralkalmazás kiépítését, követően hello portál megnyitása hello **beállítások** hello Mobile Apps-háttéralkalmazás paneljét.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-355">Once hello Mobile App backend is provisioned, hello portal opens hello **Settings** blade for hello Mobile App backend.</span></span>

<span data-ttu-id="dc0c5-356">Hello Mobile Apps-háttéralkalmazás létrehozása után dönthet úgy, tooeither csatlakozzon egy létező SQL adatbázis tooyour mobil-háttéralkalmazást, vagy hozzon létre egy új SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-356">Once hello Mobile App backend is created, you can choose tooeither connect an existing SQL database tooyour Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="dc0c5-357">Ebben a szakaszban a Microsoft SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="dc0c5-358">Ha már rendelkezik adatbázissal hello hello mobil-háttéralkalmazás és ugyanazon a helyen, választhatja **meglévő adatbázis használata** , és válassza ki, hogy az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-358">If you already have a database in hello same location as hello mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="dc0c5-359">egy adatbázist egy másik helyen lévő hello használata nem ajánlott magasabb késések miatt.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-359">hello use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="dc0c5-360">Hello új mobil-háttéralkalmazást, kattintson **beállítások** > **mobilalkalmazás** > **adatok** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-360">In hello new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="dc0c5-361">A hello **adatkapcsolat hozzáadása** panelen kattintson a **SQL adatbázis - kötelező beállítások konfigurálása** > **hozzon létre egy új adatbázist**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-361">In hello **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="dc0c5-362">Adja meg az új adatbázis hello hello nevét a hello **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-362">Enter hello name of hello new database in hello **Name** field.</span></span>
3. <span data-ttu-id="dc0c5-363">Kattintson a **Server**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-363">Click **Server**.</span></span>  <span data-ttu-id="dc0c5-364">A hello **új kiszolgáló** panelen adjon meg egy egyedi kiszolgálónevet a hello **kiszolgálónév** mezőben, majd adjon meg egy megfelelő **kiszolgáló-rendszergazdai bejelentkezés** és **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-364">In hello **New server** blade, enter a unique server name in hello **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="dc0c5-365">Győződjön meg arról **engedélyezése az azure szolgáltatások tooaccess server** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-365">Ensure **Allow azure services tooaccess server** is checked.</span></span>  <span data-ttu-id="dc0c5-366">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-366">Click **OK**.</span></span>

    ![Egy Azure SQL-adatbázis létrehozása][6]
4. <span data-ttu-id="dc0c5-368">A hello **új adatbázis** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-368">On hello **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="dc0c5-369">Vissza a hello **adatkapcsolat hozzáadása** panelen válassza **kapcsolati karakterlánc**, adja meg hello bejelentkezési és hello adatbázis létrehozásakor megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-369">Back on hello **Add data connection** blade, select **Connection string**, enter hello login and password that you provided when creating hello database.</span></span>  <span data-ttu-id="dc0c5-370">Ha a meglévő adatbázis használata hello bejelentkezési hitelesítő adatok megadása, hogy az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-370">If you use an existing database, provide hello login credentials for that database.</span></span>  <span data-ttu-id="dc0c5-371">Ha a megadott, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="dc0c5-372">Vissza a hello **adatkapcsolat hozzáadása** újra, kattintson a panel **OK** toocreate hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-372">Back on hello **Add data connection** blade again, click **OK** toocreate hello database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="dc0c5-373">Hello adatbázisának létrehozása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-373">Creation of hello database can take a few minutes.</span></span>  <span data-ttu-id="dc0c5-374">Használjon hello **értesítések** terület toomonitor hello hello központi telepítés végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-374">Use hello **Notifications** area toomonitor hello progress of hello deployment.</span></span>  <span data-ttu-id="dc0c5-375">Nem folytatódni mindaddig, amíg hello adatbázis sikeresen telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-375">Do not progress until hello database has been deployed successfully.</span></span>  <span data-ttu-id="dc0c5-376">Amennyiben sikeresen telepített, hello SQL Database-példányt a mobil-háttéralkalmazást alkalmazás beállításaiban szereplő kapcsolati karakterláncot jön létre.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-376">Once successfully deployed, a Connection String is created for hello SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="dc0c5-377">Láthatja, hogy ez a hello Alkalmazásbeállítás **beállítások** > **Alkalmazásbeállítások** > **kapcsolati karakterláncok**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-377">You can see this app setting in hello **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="dc0c5-378"><a name="howto-tables-auth"></a>Útmutató: hitelesítés megkövetelése a hozzáférési tootables</span><span class="sxs-lookup"><span data-stu-id="dc0c5-378"><a name="howto-tables-auth"></a>How to: Require authentication for access tootables</span></span>
<span data-ttu-id="dc0c5-379">Ha a toouse App Service hitelesítés hello táblák végponttal, konfigurálnia kell App Service hitelesítés hello [Azure-portálon] első.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-379">If you wish toouse App Service Authentication with hello tables endpoint, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="dc0c5-380">A hitelesítés beállítása az Azure App Service szolgáltatásban kapcsolatos további részletekért tekintse át a konfigurációs útmutató hello hello identitásszolgáltató az toouse szeretné:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-380">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="dc0c5-381">[Hogyan tooconfigure Azure Active Directory-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-381">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="dc0c5-382">[Hogyan tooconfigure Facebook-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-382">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="dc0c5-383">[Hogyan tooconfigure Google-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-383">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="dc0c5-384">[Hogyan tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-384">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="dc0c5-385">[Hogyan tooconfigure Twitter hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-385">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="dc0c5-386">Minden tábla tulajdonsága hozzáférés használt toocontrol access toohello tábla használható.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-386">Each table has an access property that can be used toocontrol access toohello table.</span></span>  <span data-ttu-id="dc0c5-387">a következő minta hello hitelesítés szükséges a statikusan megadott táblát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-387">hello following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="dc0c5-388">hello hozzáférés tulajdonságot is igénybe vehet, értékek egyike</span><span class="sxs-lookup"><span data-stu-id="dc0c5-388">hello access property can take one of three values</span></span>

* <span data-ttu-id="dc0c5-389">*Névtelen* azt jelzi, hogy hello ügyfélalkalmazás tooread adatok hitelesítés nélkül</span><span class="sxs-lookup"><span data-stu-id="dc0c5-389">*anonymous* indicates that hello client application is allowed tooread data without authentication</span></span>
* <span data-ttu-id="dc0c5-390">*hitelesített* azt jelzi, hogy hello ügyfélalkalmazás kell küldenie egy érvényes hitelesítési jogkivonat hello kérelemhez</span><span class="sxs-lookup"><span data-stu-id="dc0c5-390">*authenticated* indicates that hello client application must send a valid authentication token with hello request</span></span>
* <span data-ttu-id="dc0c5-391">*letiltott* azt jelzi, hogy ez a táblázat jelenleg le van tiltva</span><span class="sxs-lookup"><span data-stu-id="dc0c5-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="dc0c5-392">Ha a hello access tulajdonság nincs meghatározva, nem hitelesített elérését.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-392">If hello access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="dc0c5-393"><a name="howto-tables-getidentity"></a>Útmutató: hitelesítés jogcímek a táblák használata</span><span class="sxs-lookup"><span data-stu-id="dc0c5-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="dc0c5-394">Beállíthat különböző hitelesítési beállításakor igényelt jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="dc0c5-395">Ezeket a jogcímeket nem általában keresztül érhetők hello `context.user` objektum.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-395">These claims are not normally available through hello `context.user` object.</span></span>  <span data-ttu-id="dc0c5-396">Azonban azokat lekérheti hello segítségével `context.user.getIdentity()` metódust.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-396">However, they can be retrieved using hello `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="dc0c5-397">Hello `getIdentity()` metódus, amely tooan objektum ígéret ad vissza.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-397">hello `getIdentity()` method returns a Promise that resolves tooan object.</span></span>  <span data-ttu-id="dc0c5-398">hello objektum kulccsal van ellátva, a hitelesítési módszerrel (facebook, google, twitter, microsoftaccount vagy aad-ben).</span><span class="sxs-lookup"><span data-stu-id="dc0c5-398">hello object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="dc0c5-399">Például beállította a Microsoft Account hitelesítésének és kérelem hello e-mail cím jogcímet, ha adhat hozzá hello e-mail cím toohello rekord, a következő táblázat a tartományvezérlő hello:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-399">For example, if you set up Microsoft Account authentication and request hello email addresses claim, you can add hello email address toohello record with hello following table controller:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="dc0c5-400">toosee milyen jogcímek elérhető, hogy egy webes böngésző tooview hello használata `/.auth/me` végpont a webhely.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-400">toosee what claims are available, use a web browser tooview hello `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="dc0c5-401"><a name="howto-tables-disabled"></a>Hogyan: toospecific tábla műveletek letiltása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-401"><a name="howto-tables-disabled"></a>How to: Disable access toospecific table operations</span></span>
<span data-ttu-id="dc0c5-402">Továbbá tooappearing hello táblán, a hello hozzáférés tulajdonság használt toocontrol műveletenként is lehet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-402">In addition tooappearing on hello table, hello access property can be used toocontrol individual operations.</span></span>  <span data-ttu-id="dc0c5-403">Nincsenek négy műveletet:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-403">There are four operations:</span></span>

* <span data-ttu-id="dc0c5-404">*olvassa el* hello hello tábla RESTful BEOLVASNI művelet</span><span class="sxs-lookup"><span data-stu-id="dc0c5-404">*read* is hello RESTful GET operation on hello table</span></span>
* <span data-ttu-id="dc0c5-405">*Helyezze be* hello RESTful POST művelet hello tábla</span><span class="sxs-lookup"><span data-stu-id="dc0c5-405">*insert* is hello RESTful POST operation on hello table</span></span>
* <span data-ttu-id="dc0c5-406">*frissítés* hello tábla hello RESTful javítás művelet</span><span class="sxs-lookup"><span data-stu-id="dc0c5-406">*update* is hello RESTful PATCH operation on hello table</span></span>
* <span data-ttu-id="dc0c5-407">*Törlés* hello RESTful DELETE művelet hello táblán van</span><span class="sxs-lookup"><span data-stu-id="dc0c5-407">*delete* is hello RESTful DELETE operation on hello table</span></span>

<span data-ttu-id="dc0c5-408">Például előfordulhat, hogy kívánja tooprovide egy csak olvasható nem hitelesített táblázatot:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-408">For example, you may wish tooprovide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="dc0c5-409"><a name="howto-tables-query"></a>Útmutató: a tábla műveletekkel használható hello lekérdezés beállítása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-409"><a name="howto-tables-query"></a>How to: Adjust hello query that is used with table operations</span></span>
<span data-ttu-id="dc0c5-410">Kötelező megadni a tábla műveletek tooprovide hello adatok korlátozott nézete.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-410">A common requirement for table operations is tooprovide a restricted view of hello data.</span></span>  <span data-ttu-id="dc0c5-411">Megadhat például hello címkéje tábla hitelesített felhasználói Azonosítóját, hogy csak olvasható vagy a saját rekordok frissítése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-411">For example, you may provide a table that is tagged with hello authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="dc0c5-412">a következő tábla definíciójában hello ezt a szolgáltatást biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-412">hello following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="dc0c5-413">Műveletek általában a lekérdezés végrehajtása rendelkezik egy lekérdezés tulajdonságot, amelynek módosíthatja, a where záradékban.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="dc0c5-414">hello lekérdezés tulajdonság egy [QueryJS] objektum, amely egy OData-lekérdezés toosomething, amely az adatok háttérbeli hello használt tooconvert tud feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-414">hello query property is a [QueryJS] object that is used tooconvert an OData query toosomething that hello data backend can process.</span></span>  <span data-ttu-id="dc0c5-415">Egyszerű egyenlőség esetekben (például egy megelőző hello) térképre használhatók.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-415">For simple equality cases (like hello preceding one), a map can be used.</span></span> <span data-ttu-id="dc0c5-416">SQL záradékokat is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="dc0c5-417"><a name="howto-tables-softdelete"></a>Útmutató: egy helyreállítható törlésre konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="dc0c5-418">Helyreállítható törlésre ténylegesen nem törli a rekordok.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="dc0c5-419">Ehelyett azt jelöli meg azokat úgy, hogy törölte hello oszlop tootrue hello adatbázis törlése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-419">Instead it marks them as deleted within hello database by setting hello deleted column tootrue.</span></span>  <span data-ttu-id="dc0c5-420">hello Azure Mobile Apps SDK automatikusan eltávolítja helyreállíthatóan törölt rekordok eredmények, kivéve, ha hello Mobile ügyfél SDK-t használ IncludeDeleted().</span><span class="sxs-lookup"><span data-stu-id="dc0c5-420">hello Azure Mobile Apps SDK automatically removes soft-deleted records from results unless hello Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="dc0c5-421">az ideiglenes tábla törléséhez tooconfigure hello beállítása `softDelete` tulajdonság hello tábla szolgáltatásdefiníciós fájlban:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-421">tooconfigure a table for soft delete, set hello `softDelete` property in hello table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="dc0c5-422">Létre kell hoznia egy olyan mechanizmus kiürítése rekordok - vagy a ügyfélalkalmazás keresztül webjobs-feladat, Azure-függvény, vagy egy egyéni API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="dc0c5-423"><a name="howto-tables-seeding"></a>Útmutató: az adatbázis adatokkal magtípusú leképezést</span><span class="sxs-lookup"><span data-stu-id="dc0c5-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="dc0c5-424">Amikor egy új alkalmazást hoz létre, Kezdésként tooseed adatokat tartalmazó táblát.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-424">When creating a new application, you may wish tooseed a table with data.</span></span>  <span data-ttu-id="dc0c5-425">Ezt megteheti belül hello tábla definícióját JavaScript-fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-425">This can be done within hello table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="dc0c5-426">Az adatok összehangolása csak történik hello Azure Mobile Apps SDK hello tábla létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-426">Seeding of data is only done when hello table is created by hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="dc0c5-427">Hello tábla már létezik hello adatbázison belül, ha a program hello táblába beszúrta nincsenek adatok.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-427">If hello table already exists within hello database, no data is injected into hello table.</span></span>  <span data-ttu-id="dc0c5-428">Ha dinamikus séma be van kapcsolva, akkor a séma táblanévhez rendezés hello adatokból.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-428">If dynamic schema is turned on, then the schema is inferred from hello seeded data.</span></span>

<span data-ttu-id="dc0c5-429">Azt javasoljuk, hogy explicit módon hívjon hello `tables.initialize()` metódus toocreate hello tábla hello szolgáltatás elindul.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-429">We recommend that you explicitly call hello `tables.initialize()` method toocreate hello table when hello service starts running.</span></span>

### <span data-ttu-id="dc0c5-430"><a name="Swagger"></a>Útmutató: a Swagger-támogatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="dc0c5-431">Az Azure App Service Mobile Apps mellékelt beépített [Swagger] támogatja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="dc0c5-432">tooenable Swagger-támogatást, először telepítenie hello swagger-ui a függőség beállításához:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-432">tooenable Swagger support, first install hello swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="dc0c5-433">A telepítést követően engedélyezheti a Swagger támogatását hello Azure Mobile Apps konstruktorban:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-433">Once installed, you can enable Swagger support in hello Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="dc0c5-434">Ön valószínűleg csak kívánt tooenable Swagger támogatási fejlesztési kiadásában.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-434">You probably only want tooenable Swagger support in development editions.</span></span>  <span data-ttu-id="dc0c5-435">Ehhez felügyelniük a `NODE_ENV` Alkalmazásbeállítás:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="dc0c5-436">hello swagger-végpont itt található: http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-436">hello swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="dc0c5-437">Swagger felhasználói felület hello hello keresztül érheti el `/swagger/ui` végpont.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-437">You can access hello Swagger UI via hello `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="dc0c5-438">Ha a teljes alkalmazás közötti toorequire hitelesítés, a Swagger hiba hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-438">if you choose toorequire authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="dc0c5-439">A legjobb eredmények elérése érdekében válassza ki a hitelesítés nélküli tooallow kérelmeket keresztül hello Azure App Service Authentication / engedélyezési beállításait, majd vezérlőt hitelesítéssel hello `table.access` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-439">For best results, choose tooallow unauthenticated requests through in hello Azure App Service Authentication / Authorization settings, then control authentication using hello `table.access` property.</span></span>

<span data-ttu-id="dc0c5-440">Azt is megteheti hello Swagger beállítás tooyour `azureMobile.js` fájlt, ha azt szeretné csak Swagger támogatási helyileg fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-440">You can also add hello Swagger option tooyour `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="dc0c5-441"><a name="push">Leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="dc0c5-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="dc0c5-442">Mobile Apps integrálódik az Azure Notification Hubs tooenable, toosend célzott leküldéses értesítések toomillions eszközök minden fő platformon.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-442">Mobile Apps integrates with Azure Notification Hubs tooenable you toosend targeted push notifications toomillions of devices across all major platforms.</span></span> <span data-ttu-id="dc0c5-443">A Notification Hubs használatával küldhet leküldéses értesítések tooiOS, Android és Windows rendszerű eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-443">By using Notification Hubs, you can send push notifications tooiOS, Android and Windows devices.</span></span> <span data-ttu-id="dc0c5-444">További információk az akkor lehet elvégezni a segítségével toolearn Notification hubs használatával, lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dc0c5-444">toolearn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="dc0c5-445"></a><a name="send-push"></a>Hogyan: leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="dc0c5-446">a következő kód hello látható toouse hello leküldéses objektum toosend közvetítés a leküldéses értesítési tooregistered iOS-eszközök:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-446">hello following code shows how toouse hello push object toosend a broadcast push notification tooregistered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="dc0c5-447">Hozzon létre egy sablon leküldéses regisztrációs hello ügyfélről, ehelyett küldhet egy sablon leküldéses üzenet toodevices az összes támogatott platformon.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-447">By creating a template push registration from hello client, you can instead send a template push message toodevices on all supported platforms.</span></span> <span data-ttu-id="dc0c5-448">a következő kód bemutatja hogyan hello toosend sablon értesítést:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-448">hello following code shows how toosend a template notification:</span></span>

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <span data-ttu-id="dc0c5-449"><a name="push-user"></a>Hogyan: küldés leküldéses értesítések tooan hitelesített felhasználó címkék használatával</span><span class="sxs-lookup"><span data-stu-id="dc0c5-449"><a name="push-user"></a>How to: Send push notifications tooan authenticated user using tags</span></span>
<span data-ttu-id="dc0c5-450">Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan kerül toohello regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-450">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="dc0c5-451">Ezt a címkét használatával küldhet leküldéses értesítések tooall eszközök egy adott felhasználó regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-451">By using this tag, you can send push notifications tooall devices registered by a specific user.</span></span> <span data-ttu-id="dc0c5-452">hello alábbira lekérdezi hello hello kérelmező felhasználó biztonsági azonosítója és a hozzájuk a sablon leküldéses értesítés tooevery eszköz regisztrálása, hogy a felhasználó:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-452">hello following code gets hello SID of user making hello request and sends a template push notification tooevery device registration for that user:</span></span>

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="dc0c5-453">Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="dc0c5-454"><a name="CustomAPI"></a>Egyéni API-k</span><span class="sxs-lookup"><span data-stu-id="dc0c5-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="dc0c5-455"><a name="howto-customapi-basic"></a>Hogyan: Adja meg egy egyéni API</span><span class="sxs-lookup"><span data-stu-id="dc0c5-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="dc0c5-456">Ezenkívül toohello adatokat érhetnek el hello /tables végpont API, Azure Mobile Apps is biztosít egyéni API.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-456">In addition toohello data access API via hello /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="dc0c5-457">Egyéni API-k egy hasonló módon toohello definíciói vannak definiálva, és elérheti hello ugyanazokat a lehetőségeket, például a hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-457">Custom APIs are defined in a similar way toohello table definitions and can access all hello same facilities, including authentication.</span></span>

<span data-ttu-id="dc0c5-458">Ha a toouse App Service hitelesítés egy egyéni API-t, konfigurálnia kell App Service hitelesítés hello [Azure-portálon] első.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-458">If you wish toouse App Service Authentication with a Custom API, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="dc0c5-459">A hitelesítés beállítása az Azure App Service szolgáltatásban kapcsolatos további részletekért tekintse át a konfigurációs útmutató hello hello identitásszolgáltató az toouse szeretné:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-459">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="dc0c5-460">[Hogyan tooconfigure Azure Active Directory-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-460">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="dc0c5-461">[Hogyan tooconfigure Facebook-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-461">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="dc0c5-462">[Hogyan tooconfigure Google-hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-462">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="dc0c5-463">[Hogyan tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-463">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="dc0c5-464">[Hogyan tooconfigure Twitter hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-464">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="dc0c5-465">Egyéni API-k meghatározott nagy hello azonos módon hello táblák API.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-465">Custom APIs are defined in much hello same way as hello Tables API.</span></span>

1. <span data-ttu-id="dc0c5-466">Hozzon létre egy **api** könyvtár</span><span class="sxs-lookup"><span data-stu-id="dc0c5-466">Create an **api** directory</span></span>
2. <span data-ttu-id="dc0c5-467">Hozzon létre egy API definition JavaScript fájlt hello **api** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-467">Create an API definition JavaScript file in hello **api** directory.</span></span>
3. <span data-ttu-id="dc0c5-468">Használjon hello importálás módszer tooimport hello **api** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-468">Use hello import method tooimport hello **api** directory.</span></span>

<span data-ttu-id="dc0c5-469">Itt található hello prototípus api definition korábbi használtuk hello basic alkalmazáson belüli minta alapján.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-469">Here is hello prototype api definition based on hello basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="dc0c5-470">Vegyünk egy példát hello server dátumot hello visszaadó API *Date.now()* metódust.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-470">Let's take an example API that returns hello server date using hello *Date.now()* method.</span></span>  <span data-ttu-id="dc0c5-471">Hello api/date.js fájl itt található:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-471">Here is hello api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="dc0c5-472">Minden paraméter hello szabványos RESTful műveletek egyike - GET, POST, javítását vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-472">Each parameter is one of hello standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="dc0c5-473">hello metódus szabványos [ExpressJS köztes] függvény által küldött szükséges hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-473">hello method is a standard [ExpressJS Middleware] function that sends hello required output.</span></span>

### <span data-ttu-id="dc0c5-474"><a name="howto-customapi-auth"></a>Útmutató: hitelesítés szükséges hozzáférési tooa egyéni API-hoz.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access tooa custom API</span></span>
<span data-ttu-id="dc0c5-475">Az Azure Mobile Apps SDK megvalósítja a hitelesítési hello a megszokott módon hello táblák végpont és egyéni API-k.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-475">Azure Mobile Apps SDK implements authentication in hello same way for both hello tables endpoint and custom APIs.</span></span>  <span data-ttu-id="dc0c5-476">Hitelesítési toohello API hello előző szakaszban létrehozott felvételéhez adja hozzá egy **hozzáférés** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-476">To add authentication toohello API developed in hello previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="dc0c5-477">A konkrét műveletek is hitelesítési adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="dc0c5-478">hello ugyanezt a tokent hello táblák végpont használt kell használni a hitelesítést igénylő, egyéni API-k.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-478">hello same token that is used for hello tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="dc0c5-479"><a name="howto-customapi-auth"></a>Hogyan: nagy fájlfeltöltéseket kezelése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="dc0c5-480">Az Azure Mobile Apps SDK-t használ a hello [törzs-elemző köztes](https://github.com/expressjs/body-parser) tooaccept és dekódolási törzs tartalma a jelentkezést.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-480">Azure Mobile Apps SDK uses hello [body-parser middleware](https://github.com/expressjs/body-parser) tooaccept and decode body content in your submission.</span></span>  <span data-ttu-id="dc0c5-481">Törzs-elemző tooaccept nagyobb fájlfeltöltéseket előre konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-481">You can pre-configure body-parser tooaccept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="dc0c5-482">hello fájl base-64 kódolású továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-482">hello file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="dc0c5-483">Ez növeli a hello tényleges feltöltés hello méretét (és figyelembe kell venni mérete ezért hello).</span><span class="sxs-lookup"><span data-stu-id="dc0c5-483">This increases hello size of hello actual upload (and hence hello size you must account for).</span></span>

### <span data-ttu-id="dc0c5-484"><a name="howto-customapi-sql"></a>Útmutató: egyéni SQL-utasítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="dc0c5-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="dc0c5-485">hello Azure Mobile Apps SDK lehetővé teszi, hogy a hozzáférés toohello teljes környezetben keresztül hello request objektumon, így tooexecute paraméteres SQL utasítás toohello meghatározott adatszolgáltató könnyen:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-485">hello Azure Mobile Apps SDK allows access toohello entire Context through hello request object, allowing you tooexecute parameterized SQL statements toohello defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="dc0c5-486"><a name="Debugging"></a>Hibakeresés, könnyen táblák és egyszerű API-k</span><span class="sxs-lookup"><span data-stu-id="dc0c5-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="dc0c5-487"><a name="howto-diagnostic-logs"></a>Hogyan: hibakeresési, diagnosztizálása és elhárítása az Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="dc0c5-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="dc0c5-488">hello Azure App Service számos Hibakeresés és hibaelhárítási módszerekről a Node.js-alkalmazások biztosít.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-488">hello Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="dc0c5-489">Tekintse meg a következő lépések során a Node.js mobil-háttéralkalmazás cikkek tooget toohello:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-489">Refer toohello following articles tooget started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="dc0c5-490">[Az Azure App Service figyelése]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="dc0c5-491">[Az Azure App Service diagnosztikai naplózás engedélyezése]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="dc0c5-492">[A Visual Studio egy Azure App Service hibaelhárítása]</span><span class="sxs-lookup"><span data-stu-id="dc0c5-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="dc0c5-493">NODE.js-alkalmazások hozzáférhetnek tooa diagnosztikai naplófájl eszközök széles skáláját.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-493">Node.js applications have access tooa wide range of diagnostic log tools.</span></span>  <span data-ttu-id="dc0c5-494">Belsőleg, az Azure Mobile Apps Node.js SDK-t használ hello [Winston] diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-494">Internally, hello Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="dc0c5-495">Automatikusan naplózását hibakeresési mód engedélyezése vagy hello beállítása **MS_DebugMode** hello az alkalmazás-beállítás tootrue [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-495">Logging is automatically enabled by enabling debug mode or by setting hello **MS_DebugMode** app setting tootrue in hello [Azure portal].</span></span> <span data-ttu-id="dc0c5-496">Létrehozott naplók jelennek meg a hello diagnosztikai naplók hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="dc0c5-496">Generated logs appear in hello Diagnostic Logs on hello [Azure portal].</span></span>

### <span data-ttu-id="dc0c5-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Útmutató: egyszerű táblák munkához hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dc0c5-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in hello Azure portal</span></span>
<span data-ttu-id="dc0c5-498">Hello portálon könnyen táblák lehetővé teszik, hogy létrehozása és használata táblákat közvetlenül hello portálon.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-498">Easy Tables in hello portal let you create and work with tables right in hello portal.</span></span> <span data-ttu-id="dc0c5-499">Tábla műveletekbe hello App Service-szerkesztő akár szerkesztheti is.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-499">You can even edit table operations using hello App Service Editor.</span></span>

<span data-ttu-id="dc0c5-500">Amikor rákattint **könnyen táblák** a háttér-webhely beállításai hozzáadása, módosítása és törlése egy tábla.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="dc0c5-501">Hello tábla is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-501">You can also see data in hello table.</span></span>

![A könnyen táblázatok használata](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="dc0c5-503">hello következő parancsok is elérhetők hello parancssávon táblázat:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-503">hello following commands are available on hello command bar for a table:</span></span>

* <span data-ttu-id="dc0c5-504">**Engedélyek módosítása** - olvasási engedély hello módosítás, Beszúrás, frissítése és törlési műveletek hello táblán.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-504">**Change permissions** - modify hello permission for read, insert, update and delete operations on hello table.</span></span>
  <span data-ttu-id="dc0c5-505">Beállítások a következők tooallow névtelen hozzáférés, toorequire hitelesítési vagy toodisable minden hozzáférési toohello műveletet.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-505">Options are tooallow anonymous access, toorequire authentication, or toodisable all access toohello operation.</span></span>
* <span data-ttu-id="dc0c5-506">**Parancsfájl szerkesztése** -hello parancsfájl hello tábla az App Service-szerkesztő hello van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-506">**Edit script** - hello script file for hello table is opened in hello App Service Editor.</span></span>
* <span data-ttu-id="dc0c5-507">**Séma kezelése** - hozzáadása vagy oszlopok törlése vagy hello tábla index módosítása.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-507">**Manage schema** - add or delete columns or change hello table index.</span></span>
* <span data-ttu-id="dc0c5-508">**Törölje a jelet tábla** -csonkolja a meglévő tábla kell az összes adat sorok törléséhez, de hello séma hagyja változatlanul.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-508">**Clear table** - truncates an existing table be deleting all data rows but leaving hello schema unchanged.</span></span>
* <span data-ttu-id="dc0c5-509">**Sorok törlése** -egyedi sornyi adatot törli.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="dc0c5-510">**A folyamatos átviteli naplók megtekintése** -összeköttetést toohello adatfolyam-naplózási szolgáltatás a helyhez.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-510">**View streaming logs** - connects you toohello streaming log service for your site.</span></span>

### <span data-ttu-id="dc0c5-511"><a name="work-easy-apis"></a>Útmutató: egyszerű API-k munkához hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dc0c5-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in hello Azure portal</span></span>
<span data-ttu-id="dc0c5-512">Hello portal egyszerű API-k lehetővé teszik, hogy létrehozása és használata egyéni API-k közvetlenül hello portálon.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-512">Easy APIs in hello portal let you create and work with custom APIs right in hello portal.</span></span> <span data-ttu-id="dc0c5-513">API-parancsfájlok hello App Service-szerkesztő használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-513">You can edit API scripts using hello App Service Editor.</span></span>

<span data-ttu-id="dc0c5-514">Amikor rákattint **egyszerű API-k** a háttér-webhely beállításai hozzáadása, módosítása és egy egyéni API-végpont törlése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Egyszerű API-khoz](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="dc0c5-516">Hello portálon egy adott HTTP-művelet hello hozzáférési engedélyeinek módosítása, hello API parancsfájlt a App Service-szerkesztőben szerkesztheti vagy hello folyamatos átviteli naplók megtekintése.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-516">In hello portal, you can change hello access permissions for a given HTTP action, edit hello API script file in the App Service Editor, or view hello streaming logs.</span></span>

### <span data-ttu-id="dc0c5-517"><a name="online-editor"></a>Hogyan: hello App Service-szerkesztő kód szerkesztése</span><span class="sxs-lookup"><span data-stu-id="dc0c5-517"><a name="online-editor"></a>How to: Edit code in hello App Service Editor</span></span>
<span data-ttu-id="dc0c5-518">hello Azure-portál lehetővé teszi a Node.js háttérrendszer parancsfájlok hello App Service-szerkesztő szerkesztése hello projekt tooyour helyi számítógép letöltése nélkül.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-518">hello Azure portal lets you edit your Node.js backend script files in hello App Service Editor without having to download hello project tooyour local computer.</span></span> <span data-ttu-id="dc0c5-519">a parancsfájlok tooedit hello online szerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="dc0c5-519">tooedit script files in hello online editor:</span></span>

1. <span data-ttu-id="dc0c5-520">A mobilalkalmazás-háttérrendszer paneljén kattintson **összes beállítás** > vagy **könnyen táblák** vagy **egyszerű API-k**, egy táblázatot vagy API-t, majd **parancsfájl szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="dc0c5-521">hello parancsfájlt az App Service-szerkesztő hello van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-521">hello script file is opened in hello App Service Editor.</span></span>

    ![App Service-szerkesztő](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="dc0c5-523">Végezze el a módosításokat toohello kódfájl hello online szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-523">Make your changes toohello code file in hello online editor.</span></span> <span data-ttu-id="dc0c5-524">Változások automatikusan mentése közben.</span><span class="sxs-lookup"><span data-stu-id="dc0c5-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android ügyfél gyors üzembe helyezés]: app-service-mobile-android-get-started.md
[Apache Cordova-ügyfél gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[iOS ügyfél gyors üzembe helyezés]: app-service-mobile-ios-get-started.md
[Xamarin.iOS ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms-ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-forms-get-started.md
[A Windows Store ügyfél gyors üzembe helyezés]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[Hogyan tooconfigure Azure Active Directory-hitelesítés]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hogyan tooconfigure Facebook-hitelesítés]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hogyan tooconfigure Google-hitelesítés]: app-service-mobile-how-to-configure-google-authentication.md
[Hogyan tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hogyan tooconfigure Twitter hitelesítés]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service telepítési útmutató]: ../app-service-web/web-sites-deploy.md
[Az Azure App Service figyelése]: ../app-service-web/web-sites-monitor.md
[Az Azure App Service diagnosztikai naplózás engedélyezése]: ../app-service-web/web-sites-enable-diagnostic-log.md
[A Visual Studio egy Azure App Service hibaelhárítása]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[hello csomópont verzió megadása]: ../nodejs-specify-node-version-azure-apps.md
[csomópont modulok használata]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portálon]: https://portal.azure.com/
[OData]: http://www.odata.org
[ígéret]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[minták directory a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js eszközök 1.1 a Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js csomag]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS köztes]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
