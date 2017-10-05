---
title: "A Mobile Apps használata a Node.js háttérkiszolgáló SDK |} Microsoft Docs"
description: "Megtudhatja, hogyan használható a Node.js háttérkiszolgáló SDK az Azure App Service Mobile Apps a."
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
ms.openlocfilehash: 1d3aa7a0089279a8eafeb0ded951a5238e189eaa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="c0456-103">Az Azure Mobile Apps Node.js SDK használatával</span><span class="sxs-lookup"><span data-stu-id="c0456-103">How to use the Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="c0456-104">Ez a cikk ismerteti a részletes útmutatást és példákat bemutatja, hogyan használható az Azure App Service Mobile Apps Node.js-háttéralkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c0456-104">This article provides detailed information and examples showing how to work with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="c0456-105"><a name="Introduction"></a>Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c0456-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="c0456-106">Az Azure App Service Mobile Apps lehetővé teszi a mobil optimalizált adatelérési Web API hozzáadása egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c0456-106">Azure App Service Mobile Apps provides the capability to add a mobile-optimized data access Web API to a web application.</span></span>  <span data-ttu-id="c0456-107">Az ASP.NET és a Node.js webes alkalmazásokhoz az Azure App Service Mobile Apps SDK valósul meg.</span><span class="sxs-lookup"><span data-stu-id="c0456-107">The Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="c0456-108">Az SDK tartalmazza a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="c0456-108">The SDK provides the following operations:</span></span>

* <span data-ttu-id="c0456-109">Az adatelérési tábla műveletek (olvasás, Insert, Update, Delete)</span><span class="sxs-lookup"><span data-stu-id="c0456-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="c0456-110">Egyéni API-műveletek</span><span class="sxs-lookup"><span data-stu-id="c0456-110">Custom API operations</span></span>

<span data-ttu-id="c0456-111">Mindkét műveletek Azure App Service, beleértve a közösségi Identitásszolgáltatók például a Facebookhoz, Twitter, Google és a Microsoft, valamint Azure Active Directory vállalati identitás által engedélyezett összes Identitásszolgáltatók biztosít a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="c0456-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="c0456-112">Minták találhat meg minden esetben használja a [minták directory a Githubon].</span><span class="sxs-lookup"><span data-stu-id="c0456-112">You can find samples for each use case in the [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c0456-113">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="c0456-113">Supported Platforms</span></span>
<span data-ttu-id="c0456-114">Az Azure Mobile Apps csomópont SDK támogatja a jelenlegi LTS verzió, csomópont és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="c0456-114">The Azure Mobile Apps Node SDK supports the current LTS release of Node and later.</span></span>  <span data-ttu-id="c0456-115">Frissítésétől írást, a legújabb LTS verzió a csomópont v4.5.0.</span><span class="sxs-lookup"><span data-stu-id="c0456-115">As of writing, the latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="c0456-116">A csomópont más verzióiban előfordulhat, hogy működik, de nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="c0456-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="c0456-117">Az Azure Mobile Apps csomópont SDK két adatbázis-illesztőprogram támogatja, mert a csomópont-mssql illesztőprogram támogatja az SQL Azure és a helyi SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="c0456-117">The Azure Mobile Apps Node SDK supports two database drivers - the node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="c0456-118">A sqlite3 illesztőprogram csak egyetlen példányán SQLite adatbázisokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="c0456-118">The sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="c0456-119"><a name="howto-cmdline-basicapp"></a>Útmutató: a parancssor használatával alapszintű Node.js-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0456-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using the Command Line</span></span>
<span data-ttu-id="c0456-120">Minden Azure App Service Mobile App Node.js-háttéralkalmazáshoz ExpressJS alkalmazásként indul.</span><span class="sxs-lookup"><span data-stu-id="c0456-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="c0456-121">ExpressJS a legnépszerűbb web service keretrendszer Node.js érhető el.</span><span class="sxs-lookup"><span data-stu-id="c0456-121">ExpressJS is the most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="c0456-122">Létrehozhat egy alapszintű [Express] alkalmazás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c0456-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="c0456-123">Egy parancs vagy a PowerShell-ablakot hozzon létre egy könyvtárat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="c0456-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="c0456-124">A csomag struktúra inicializálása npm init futtassa.</span><span class="sxs-lookup"><span data-stu-id="c0456-124">Run npm init to initialize the package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="c0456-125">Az npm init parancs kéri a projekt inicializálása kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="c0456-125">The npm init command asks a set of questions to initialize the project.</span></span>  <span data-ttu-id="c0456-126">Tekintse meg a példa az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="c0456-126">See the example output:</span></span>

    ![A npm init kimenet][0]
3. <span data-ttu-id="c0456-128">Telepítse a sürgős és az azure-mobilalkalmazások könyvtárak összetevőtárházat npm.</span><span class="sxs-lookup"><span data-stu-id="c0456-128">Install the express and azure-mobile-apps libraries from the npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="c0456-129">A kiszolgáló alapszintű mobil végrehajtásához app.js fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c0456-129">Create an app.js file to implement the basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="c0456-130">Ez az alkalmazás egy végpontot hoz létre egy mobile optimalizált WebAPI (`/tables/TodoItem`), amely egy dinamikus sémával alapul szolgáló SQL adattároló nem hitelesített hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0456-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access to an underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="c0456-131">Alkalmas az ügyfél szalagtár gyors üzembe helyezések a következő:</span><span class="sxs-lookup"><span data-stu-id="c0456-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="c0456-132">[Android ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="c0456-133">[Apache Cordova-ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="c0456-134">[iOS ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="c0456-135">[A Windows Store ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="c0456-136">[Xamarin.iOS ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="c0456-137">[Xamarin.Android ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="c0456-138">[Xamarin.Forms-ügyfél gyors üzembe helyezés]</span><span class="sxs-lookup"><span data-stu-id="c0456-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="c0456-139">Ezen alapszintű alkalmazás találja a kódot a [basicapp mintát a Githubon].</span><span class="sxs-lookup"><span data-stu-id="c0456-139">You can find the code for this basic application in the [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="c0456-140"><a name="howto-vs2015-basicapp"></a>Útmutató: a Visual Studio 2015 csomópont-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0456-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="c0456-141">Visual Studio 2015-öt bővítménye belül az IDE Node.js-alkalmazások fejlesztéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c0456-141">Visual Studio 2015 requires an extension to develop Node.js applications within the IDE.</span></span>  <span data-ttu-id="c0456-142">Indítsa el, telepítse a [Node.js eszközök 1.1 a Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="c0456-142">To start, install the [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="c0456-143">A Node.js Tools for Visual Studio telepítése után hozzon létre egy Express 4.x alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="c0456-143">Once the Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="c0456-144">Nyissa meg a **új projekt** párbeszédpanelen (a **fájl** > **új** > **projekt …** ).</span><span class="sxs-lookup"><span data-stu-id="c0456-144">Open the **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="c0456-145">Bontsa ki a **sablonok** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="c0456-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="c0456-146">Válassza ki a **alapszintű Azure Node.js Express 4 alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c0456-146">Select the **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="c0456-147">Töltse ki a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="c0456-147">Fill in the project name.</span></span>  <span data-ttu-id="c0456-148">Kattintson az *OK* gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-148">Click *OK*.</span></span>

    ![Visual Studio 2015-öt új projekt][1]
5. <span data-ttu-id="c0456-150">Kattintson a jobb gombbal a **npm** csomópont, és válassza **telepítése új npm csomagok...** .</span><span class="sxs-lookup"><span data-stu-id="c0456-150">Right-click the **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="c0456-151">Szükség lehet az npm-katalógus az első Node.js-alkalmazás létrehozása a frissítése.</span><span class="sxs-lookup"><span data-stu-id="c0456-151">You may need to refresh the npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="c0456-152">Kattintson a **frissítése** szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="c0456-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="c0456-153">Adja meg *azure-mobilalkalmazások* be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="c0456-153">Enter *azure-mobile-apps* in the search box.</span></span>  <span data-ttu-id="c0456-154">Kattintson a **azure-mobileszköz-alkalmazások 2.0.0** csomagot, majd kattintson az **csomagtelepítés**.</span><span class="sxs-lookup"><span data-stu-id="c0456-154">Click the **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Új npm-csomagok][2]
8. <span data-ttu-id="c0456-156">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-156">Click **Close**.</span></span>
9. <span data-ttu-id="c0456-157">Nyissa meg a *app.js* fájl az Azure Mobile Apps SDK támogatásához.</span><span class="sxs-lookup"><span data-stu-id="c0456-157">Open the *app.js* file to add support for the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="c0456-158">Sor 6 at alsó részén a könyvtárban található utasítások szükséges, adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="c0456-158">At line 6 at the bottom of the library require statements, add the following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="c0456-159">Körülbelül sor 27 után az egyéb app.use utasítások adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="c0456-159">At approximately line 27 after the other app.use statements, add the following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="c0456-160">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="c0456-160">Save the file.</span></span>
10. <span data-ttu-id="c0456-161">Futtassa az alkalmazást helyileg (az API-t kiszolgált a http://localhost: 3000), vagy közzététele az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="c0456-161">Either run the application locally (the API is served on http://localhost:3000) or publish to Azure.</span></span>

### <span data-ttu-id="c0456-162"><a name="create-node-backend-portal"></a>Útmutató: az Azure portál használata Node.js-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0456-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using the Azure portal</span></span>
<span data-ttu-id="c0456-163">A Mobile Apps háttér közvetlenül hozhat létre a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-163">You can create a Mobile App backend right in the [Azure portal].</span></span> <span data-ttu-id="c0456-164">Kövesse az alábbi lépéseket, vagy hozzon létre egy ügyfél és kiszolgáló együtt a [mobilalkalmazás létrehozása](app-service-mobile-ios-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c0456-164">You can either follow the following steps or create a client and server together by following the [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="c0456-165">Az oktatóanyag tartalmazza ezeket az utasításokat egy egyszerűsített verziója és a projektek a koncepció igazolása.</span><span class="sxs-lookup"><span data-stu-id="c0456-165">The tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="c0456-166">Vissza a *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **Node.js** , a **háttéralkalmazás-nyelv**.</span><span class="sxs-lookup"><span data-stu-id="c0456-166">Back in the *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="c0456-167">Jelölje be a "**tudomásul veszem, hogy ezzel a művelettel felülírja az összes hely tartalmának.**", majd kattintson a **TodoItem tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c0456-167">Check the box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="c0456-168"><a name="download-quickstart"></a>Hogyan: Töltse le a Node.js háttérrendszer gyors üzembe helyezés kód projekt Git használatával</span><span class="sxs-lookup"><span data-stu-id="c0456-168"><a name="download-quickstart"></a>How to: Download the Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="c0456-169">Ha Node.js Mobile Apps-háttéralkalmazás létrehozása a portál használatával **gyors üzembe helyezési** panelen, egy Node.js-projektet, létrehozása és telepítése a helyen.</span><span class="sxs-lookup"><span data-stu-id="c0456-169">When you create a Node.js Mobile App backend by using the portal **Quick start** blade, a Node.js project is created for you and deployed to your site.</span></span> <span data-ttu-id="c0456-170">Adja hozzá a táblák és API-kat, és a Node.js-háttéralkalmazáshoz a portálon tartozó kódfájlok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="c0456-170">You can add tables and APIs and edit code files for the Node.js backend in the portal.</span></span> <span data-ttu-id="c0456-171">Különböző központi telepítési eszközök segítségével is letölti a háttéralkalmazás-projekt hozzáadása vagy módosítása a táblák és API-k, majd a projekt közzé.</span><span class="sxs-lookup"><span data-stu-id="c0456-171">You can also use various deployment tools to download the backend project so that you can add or modify tables and APIs, then republish the project.</span></span> <span data-ttu-id="c0456-172">További információkért lásd: a [Azure App Service telepítési útmutató].</span><span class="sxs-lookup"><span data-stu-id="c0456-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="c0456-173">az alábbi eljárás egy Git-tárház használatával töltse le a gyors üzembe helyezési projekt kódját.</span><span class="sxs-lookup"><span data-stu-id="c0456-173">the following procedure uses a Git repository to download the quickstart project code.</span></span>

1. <span data-ttu-id="c0456-174">Ha még nem tette, telepítse a Git esetében.</span><span class="sxs-lookup"><span data-stu-id="c0456-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="c0456-175">A Git telepítéséhez szükséges lépések eltérőek, operációs rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="c0456-175">The steps required to install Git vary between operating systems.</span></span> <span data-ttu-id="c0456-176">Lásd: [telepítése Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) az operációsrendszer-specifikus disztribúcióiról, valamint a telepítési útmutatót.</span><span class="sxs-lookup"><span data-stu-id="c0456-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="c0456-177">Kövesse a [engedélyezése az App Service alkalmazás tárház](../app-service-web/app-service-deploy-local-git.md#Step3) ahhoz, hogy a háttér-helyhez, és jegyezze fel a központi telepítés felhasználónevet és jelszót a Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="c0456-177">Follow the steps in [Enable the App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) to enable the Git repository for your backend site, making a note of the deployment username and password.</span></span>
3. <span data-ttu-id="c0456-178">A mobilalkalmazás háttérrendszerének panelen, jegyezze fel a **Git-klón URL-cím** beállítást.</span><span class="sxs-lookup"><span data-stu-id="c0456-178">In the blade for your Mobile App backend, make a note of the **Git clone URL** setting.</span></span>
4. <span data-ttu-id="c0456-179">Hajtsa végre a `git clone` parancsot a Git használatával klón URL-CÍMÉT, írja be a jelszót, ha szükséges, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="c0456-179">Execute the `git clone` command using the Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="c0456-180">Keresse meg a helyi könyvtárba, amely az előző példában /todolist, és figyelje meg, hogy project fájlok le vannak töltve.</span><span class="sxs-lookup"><span data-stu-id="c0456-180">Browse to local directory, which in the preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="c0456-181">Keresse meg a `todoitem.json` fájlt a `/tables` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c0456-181">Locate the `todoitem.json` file in the `/tables` directory.</span></span>  <span data-ttu-id="c0456-182">Ez a fájl engedélyeit a tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c0456-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="c0456-183">Is megtalálhatja a `todoitem.js` fájl ugyanabban a könyvtárban, amely meghatározza, hogy CRUD művelet a következő táblázatban a parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="c0456-183">Also find the `todoitem.js` file in the same directory, which defines that CRUD operation scripts for the table.</span></span>
6. <span data-ttu-id="c0456-184">Project fájlok módosult, miután hajtható végre a következő parancsok futtatásával adja hozzá, véglegesítése, majd töltse fel a módosításokat a helyhez:</span><span class="sxs-lookup"><span data-stu-id="c0456-184">After you have made changes to project files, execute the following commands to add, commit, then upload the changes to the site:</span></span>

        $ git commit -m "updated the table script"
        $ git push origin master

    <span data-ttu-id="c0456-185">Amikor új fájlok hozzáadása a projekthez, először kell végrehajtani a `git add .` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c0456-185">When you add new files to the project, you first need to execute the `git add .` command.</span></span>

<span data-ttu-id="c0456-186">A hely minden alkalommal, amikor egy új készletét véglegesíti a rendszer előkészítésre továbbít a webhely ismételt közzététele.</span><span class="sxs-lookup"><span data-stu-id="c0456-186">The site is republished every time a new set of commits is pushed to the site.</span></span>

### <span data-ttu-id="c0456-187"><a name="howto-publish-to-azure"></a>Útmutató: a Node.js-háttéralkalmazáshoz közzététele az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="c0456-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend to Azure</span></span>
<span data-ttu-id="c0456-188">A Microsoft Azure az Azure szolgáltatásban az Azure App Service Mobile Apps Node.js háttérrendszer közzétételéhez számos mechanizmust biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0456-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to the Azure service.</span></span>  <span data-ttu-id="c0456-189">Ezek közé tartozik a központi telepítési eszközöket a Visual Studio integrált, parancssori eszközök és verziókövetési alapuló folyamatos üzembe helyezés beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="c0456-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="c0456-190">A témakörrel kapcsolatos további információkért lásd: a [Azure App Service telepítési útmutató].</span><span class="sxs-lookup"><span data-stu-id="c0456-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="c0456-191">Az Azure App Service a Node.js-alkalmazás üzembe helyezése előtt tekintse át a konkrét útmutatásért rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c0456-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="c0456-192">Hogyan [a csomópont verzió megadása]</span><span class="sxs-lookup"><span data-stu-id="c0456-192">How to [specify the Node Version]</span></span>
* <span data-ttu-id="c0456-193">Hogyan [csomópont modulok használata]</span><span class="sxs-lookup"><span data-stu-id="c0456-193">How to [use Node modules]</span></span>

### <span data-ttu-id="c0456-194"><a name="howto-enable-homepage"></a>Útmutató: az alkalmazás kezdőlapját engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c0456-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="c0456-195">Számos alkalmazás webes és mobilalkalmazások és ExpressJS keretében teszi lehetővé a két értékkorlátozás kombinálhatók.</span><span class="sxs-lookup"><span data-stu-id="c0456-195">Many applications are a combination of web and mobile apps and the ExpressJS framework allows you to combine the two facets.</span></span>  <span data-ttu-id="c0456-196">Egyes esetekben azonban Kezdésként csak valósítja meg a mobil illesztőfelületet.</span><span class="sxs-lookup"><span data-stu-id="c0456-196">Sometimes, however, you may wish to only implement a mobile interface.</span></span>  <span data-ttu-id="c0456-197">Ez akkor hasznos, annak érdekében, az app service egy kezdőlapja megfelelően működik, és így.</span><span class="sxs-lookup"><span data-stu-id="c0456-197">It is useful to provide a landing page to ensure the app service is up and running.</span></span>  <span data-ttu-id="c0456-198">Adja meg a saját kezdőlapján, vagy egy ideiglenes kezdőlap engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c0456-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="c0456-199">Ahhoz, hogy egy ideiglenes kezdőlapját, használja a következő példányának létrehozása az Azure Mobile Apps:</span><span class="sxs-lookup"><span data-stu-id="c0456-199">To enable a temporary home page, use the following to instantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="c0456-200">Ha csak ez a beállítás érhető el helyi fejlesztésekor, ezzel a beállítással adhat hozzá a `azureMobile.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="c0456-200">If you only want this option available when developing locally, you can add this setting to your `azureMobile.js` file.</span></span>

## <span data-ttu-id="c0456-201"><a name="TableOperations"></a>Tábla műveletek</span><span class="sxs-lookup"><span data-stu-id="c0456-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="c0456-202">Az azure-mobilalkalmazások Node.js Server SDK teszi közzé az Azure SQL Database, a WebAPI tárolt adattáblák mechanizmust biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0456-202">The azure-mobile-apps Node.js Server SDK provides mechanisms to expose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="c0456-203">Öt műveleti rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="c0456-203">Five operations are provided.</span></span>

| <span data-ttu-id="c0456-204">Művelet</span><span class="sxs-lookup"><span data-stu-id="c0456-204">Operation</span></span> | <span data-ttu-id="c0456-205">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0456-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c0456-206">GET /tables/*táblanév*</span><span class="sxs-lookup"><span data-stu-id="c0456-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="c0456-207">A tábla összes rekord beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0456-207">Get all records in the table</span></span> |
| <span data-ttu-id="c0456-208">GET /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="c0456-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="c0456-209">Egy adott rekord a táblázatban beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0456-209">Get a specific record in the table</span></span> |
| <span data-ttu-id="c0456-210">POST /tables/*táblanév*</span><span class="sxs-lookup"><span data-stu-id="c0456-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="c0456-211">A tábla rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0456-211">Create a record in the table</span></span> |
| <span data-ttu-id="c0456-212">JAVÍTÁS /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="c0456-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="c0456-213">Módosítani egy rekordot a tábla</span><span class="sxs-lookup"><span data-stu-id="c0456-213">Update a record in the table</span></span> |
| <span data-ttu-id="c0456-214">TÖRLÉS /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="c0456-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="c0456-215">A tábla rekord törlése</span><span class="sxs-lookup"><span data-stu-id="c0456-215">Delete a record in the table</span></span> |

<span data-ttu-id="c0456-216">Támogatja a WebAPI [OData] és bővíti a következő tábla sémáját támogatásához [offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="c0456-216">This WebAPI supports [OData] and extends the table schema to support [offline data sync].</span></span>

### <span data-ttu-id="c0456-217"><a name="howto-dynamicschema"></a>Hogyan: Adja meg a táblák egy dinamikus séma használata</span><span class="sxs-lookup"><span data-stu-id="c0456-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="c0456-218">Egy tábla használt, meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="c0456-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="c0456-219">Táblák (ahol a fejlesztői definiálja a séma oszlopaira) statikus séma vagy dinamikusan adható meg (ha az SDK szabályozza a séma, a bejövő kérések alapján).</span><span class="sxs-lookup"><span data-stu-id="c0456-219">Tables can be defined with a static schema (where the developer defines the columns within the schema) or dynamically (where the SDK controls the schema based on incoming requests).</span></span> <span data-ttu-id="c0456-220">Emellett a fejlesztői szabályozhatja a WebAPI bizonyos aspektusainak Javascript-kód hozzáadása a definíciót.</span><span class="sxs-lookup"><span data-stu-id="c0456-220">In addition, the developer can control specific aspects of the WebAPI by adding Javascript code to the definition.</span></span>

<span data-ttu-id="c0456-221">Ajánlott eljárásként akkor kell mindegyik tábla a táblák könyvtárban Javascript-fájlt ad meg, majd a tables.import() metódus használatával importálhatja a táblákat.</span><span class="sxs-lookup"><span data-stu-id="c0456-221">As a best practice, you should define each table in a Javascript file in the tables directory, then use the tables.import() method to import the tables.</span></span>  <span data-ttu-id="c0456-222">Az alkalmazáson belüli basic kiterjesztése esetén az app.js fájl ki kell igazítani:</span><span class="sxs-lookup"><span data-stu-id="c0456-222">Extending the basic-app, the app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="c0456-223">Adja meg a táblázatban szereplő. / tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="c0456-223">Define the table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

<span data-ttu-id="c0456-224">Táblák alapértelmezés szerint dinamikus séma használja.</span><span class="sxs-lookup"><span data-stu-id="c0456-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="c0456-225">Dinamikus séma globálisan kikapcsolásához állítsa be az Alkalmazásbeállítás **MS_DynamicSchema** false értékre az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c0456-225">To turn off dynamic schema globally, set the App Setting **MS_DynamicSchema** to false within the Azure portal.</span></span>

<span data-ttu-id="c0456-226">A teljes példáját megtalálhatja a [todo mintát a Githubon].</span><span class="sxs-lookup"><span data-stu-id="c0456-226">You can find a complete example in the [todo sample on GitHub].</span></span>

### <span data-ttu-id="c0456-227"><a name="howto-staticschema"></a>Hogyan: Adja meg a táblák egy statikus séma használata</span><span class="sxs-lookup"><span data-stu-id="c0456-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="c0456-228">Explicit módon határozhatja meg a WebAPI keresztül teszi közzé az oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="c0456-228">You can explicitly define the columns to expose via the WebAPI.</span></span>  <span data-ttu-id="c0456-229">Az azure-mobilalkalmazások Node.js SDK automatikusan hozzáadja a listához, Ön által biztosított offline adatszinkronizálás szükséges további oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="c0456-229">The azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync to the list that you provide.</span></span>  <span data-ttu-id="c0456-230">Például a gyors üzembe helyezés ügyfélalkalmazások szükséges két oszlopokkal rendelkező táblát: text (szöveg) (logikai) hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="c0456-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="c0456-231">A tábla a tábla definícióját JavaScript-fájlt (a táblák könyvtárban található) az alábbiak szerint lehet meghatározni:</span><span class="sxs-lookup"><span data-stu-id="c0456-231">The table can be defined in the table definition JavaScript file (located in the tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="c0456-232">Ha statikusan határozza meg a táblák, majd kell is a tables.initialize() metódust hívja az adatbázisséma indításkor létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c0456-232">If you define tables statically, then you must also call the tables.initialize() method to create the database schema on startup.</span></span>  <span data-ttu-id="c0456-233">A tables.initialize() metódus visszaadja a [ígéret] , hogy a webszolgáltatás nem teljesíti a kérelmeket az adatbázis inicializálása előtt.</span><span class="sxs-lookup"><span data-stu-id="c0456-233">The tables.initialize() method returns a [Promise] so that the web service does not serve requests before the database being initialized.</span></span>

### <span data-ttu-id="c0456-234"><a name="howto-sqlexpress-setup"></a>Hogyan: használja az SQL Express fejlesztési adattárként a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="c0456-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="c0456-235">Az Azure Mobile Apps a AzureMobile alkalmazások csomópont SDK kiszolgáló kívül a mezőbe a három lehetőséget kínál: SDK kiszolgáló kívül a mezőbe a három lehetőséget kínál:</span><span class="sxs-lookup"><span data-stu-id="c0456-235">The Azure Mobile Apps The AzureMobile Apps Node SDK provides three options for serving data out of the box: SDK provides three options for serving data out of the box:</span></span>

* <span data-ttu-id="c0456-236">Használja a **memória** nem állandó példa áruházbeli-illesztőprogrammal</span><span class="sxs-lookup"><span data-stu-id="c0456-236">Use the **memory** driver to provide a non-persistent example store</span></span>
* <span data-ttu-id="c0456-237">Használja a **mssql** adja meg az SQL Express adattárat fejlesztési-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="c0456-237">Use the **mssql** driver to provide a SQL Express data store for development</span></span>
* <span data-ttu-id="c0456-238">Használja a **mssql** adjon meg egy Azure SQL Database adattároló üzemi-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="c0456-238">Use the **mssql** driver to provide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="c0456-239">Az Azure Mobile Apps Node.js SDK-t használ a [mssql Node.js csomag] létrehozásához és az SQL Express és SQL-adatbázis kapcsolat használatára.</span><span class="sxs-lookup"><span data-stu-id="c0456-239">The Azure Mobile Apps Node.js SDK uses the [mssql Node.js package] to establish and use a connection to both SQL Express and SQL Database.</span></span>  <span data-ttu-id="c0456-240">A csomag igényli-e, hogy az SQL Express-példány TCP-kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c0456-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="c0456-241">A memória illesztőprogram nem biztosít tesztelési létesítményekben teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="c0456-241">The memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="c0456-242">Ha a háttéralkalmazást helyileg tesztelni kívánt, SQL Express adattárat és az mssql illesztőprogram használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="c0456-242">If you wish to test your backend locally, we recommend the use of a SQL Express data store and the mssql driver.</span></span>
>
>

1. <span data-ttu-id="c0456-243">Töltse le és telepítse [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="c0456-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="c0456-244">Győződjön meg arról, telepíti az SQL Server 2014 Express eszközök kiadásával.</span><span class="sxs-lookup"><span data-stu-id="c0456-244">Ensure you install the SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="c0456-245">Ha 64 bites támogatás kifejezetten szüksége a 32 bites kevesebb memóriát igényel, futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="c0456-245">Unless you explicitly require 64-bit support, the 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="c0456-246">Futtassa az SQL Server 2014 Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="c0456-246">Run the SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="c0456-247">Bontsa ki a **SQL Server hálózati konfigurációja** csomópontot a bal oldali fában menüben.</span><span class="sxs-lookup"><span data-stu-id="c0456-247">Expand the **SQL Server Network Configuration** node in the left-hand tree menu.</span></span>
   2. <span data-ttu-id="c0456-248">Kattintson a **SQLEXPRESS protokollok**.</span><span class="sxs-lookup"><span data-stu-id="c0456-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="c0456-249">Kattintson a jobb gombbal **TCP/IP** válassza **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="c0456-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="c0456-250">Kattintson a **OK** az előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="c0456-250">Click **OK** in the pop-up dialog.</span></span>
   4. <span data-ttu-id="c0456-251">Kattintson a jobb gombbal **TCP/IP** válassza **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="c0456-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="c0456-252">Kattintson a **IP-címek** fülre.</span><span class="sxs-lookup"><span data-stu-id="c0456-252">Click the **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="c0456-253">Keresés a **IPAll** csomópont.</span><span class="sxs-lookup"><span data-stu-id="c0456-253">Find the **IPAll** node.</span></span>  <span data-ttu-id="c0456-254">Az a **TCP-Port** mezőbe írja be **1433**.</span><span class="sxs-lookup"><span data-stu-id="c0456-254">In the **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="c0456-255">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-255">Click **OK**.</span></span>  <span data-ttu-id="c0456-256">Kattintson a **OK** az előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="c0456-256">Click **OK** in the pop-up dialog.</span></span>
   8. <span data-ttu-id="c0456-257">Kattintson a **SQL Server Services** a bal oldali fában menüben.</span><span class="sxs-lookup"><span data-stu-id="c0456-257">Click **SQL Server Services** in the left-hand tree menu.</span></span>
   9. <span data-ttu-id="c0456-258">Kattintson a jobb gombbal **SQL Server (SQLEXPRESS)** válassza **újraindítása**</span><span class="sxs-lookup"><span data-stu-id="c0456-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="c0456-259">Zárja be az SQL Server 2014 Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="c0456-259">Close the SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="c0456-260">Futtassa az SQL Server 2014 Management Studio, és csatlakozzon a helyi SQL Express-példány</span><span class="sxs-lookup"><span data-stu-id="c0456-260">Run the SQL Server 2014 Management Studio and connect to your local SQL Express instance</span></span>

   1. <span data-ttu-id="c0456-261">Kattintson a jobb gombbal az Object Explorer-példány, és válassza ki **tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="c0456-261">Right-click your instance in the Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="c0456-262">Válassza ki a **biztonsági** lap.</span><span class="sxs-lookup"><span data-stu-id="c0456-262">Select the **Security** page.</span></span>
   3. <span data-ttu-id="c0456-263">Győződjön meg arról a **SQL Server és a Windows-hitelesítés üzemmód** van kijelölve</span><span class="sxs-lookup"><span data-stu-id="c0456-263">Ensure the **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="c0456-264">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="c0456-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="c0456-265">Bontsa ki a **biztonsági** > **bejelentkezések** az Object Explorer</span><span class="sxs-lookup"><span data-stu-id="c0456-265">Expand **Security** > **Logins** in the Object Explorer</span></span>
   6. <span data-ttu-id="c0456-266">Kattintson a jobb gombbal **bejelentkezések** válassza **új bejelentkezés...**</span><span class="sxs-lookup"><span data-stu-id="c0456-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="c0456-267">Adja meg a bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="c0456-267">Enter a Login name.</span></span>  <span data-ttu-id="c0456-268">Kattintson az **SQL Server-hitelesítés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="c0456-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="c0456-269">Adjon meg egy jelszót, majd írja be ugyanezt a jelszót **jelszó megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="c0456-269">Enter a Password, then enter the same password in **Confirm password**.</span></span>  <span data-ttu-id="c0456-270">A jelszónak meg kell felelnie a Windows bonyolultsági követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="c0456-270">The password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="c0456-271">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="c0456-271">Click **OK**</span></span>

          ![Add a new user to SQL Express][5]
   9. <span data-ttu-id="c0456-272">Kattintson a jobb gombbal az új bejelentkezési adatait, és válassza ki **tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="c0456-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="c0456-273">Válassza ki a **kiszolgálói szerepkörök** lap</span><span class="sxs-lookup"><span data-stu-id="c0456-273">Select the **Server Roles** page</span></span>
   11. <span data-ttu-id="c0456-274">Mellett jelölje be a jelölőnégyzetet a **dbcreator** kiszolgálói szerepkör</span><span class="sxs-lookup"><span data-stu-id="c0456-274">Check the box next to the **dbcreator** server role</span></span>
   12. <span data-ttu-id="c0456-275">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="c0456-275">Click **OK**</span></span>
   13. <span data-ttu-id="c0456-276">Zárja be az SQL Server 2015 Management Studio</span><span class="sxs-lookup"><span data-stu-id="c0456-276">Close the SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="c0456-277">Győződjön meg arról, a felhasználónév és a megadott jelszó rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c0456-277">Ensure you record the username and password you selected.</span></span>  <span data-ttu-id="c0456-278">Szükség lehet további kiszolgálói szerepkörök vagy engedélyek attól függően, hogy az adott adatbázis követelményeit.</span><span class="sxs-lookup"><span data-stu-id="c0456-278">You may need to assign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="c0456-279">A Node.js-alkalmazás olvasása a **SQLCONNSTR_MS_TableConnectionString** környezeti változó a kapcsolati karakterlánc ehhez az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="c0456-279">The Node.js application reads the **SQLCONNSTR_MS_TableConnectionString** environment variable for the connection string for this database.</span></span>  <span data-ttu-id="c0456-280">A változó beállított a környezetben.</span><span class="sxs-lookup"><span data-stu-id="c0456-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="c0456-281">Például a PowerShell segítségével a környezeti változót:</span><span class="sxs-lookup"><span data-stu-id="c0456-281">For example, you can use PowerShell to set this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="c0456-282">TCP/IP-kapcsolaton keresztül érik el az adatbázist, és adjon meg egy felhasználónevet és jelszót a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c0456-282">Access the database through a TCP/IP connection and provide a username and password for the connection.</span></span>

### <span data-ttu-id="c0456-283"><a name="howto-config-localdev"></a>Útmutató: a helyi fejlesztési projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0456-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="c0456-284">Az Azure Mobile Apps beolvassa a JavaScript-fájl neve *azureMobile.js* helyi objektumot.</span><span class="sxs-lookup"><span data-stu-id="c0456-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from the local filesystem.</span></span>  <span data-ttu-id="c0456-285">Ne használja ezt a fájlt az Azure Mobile Apps SDK konfigurálásához éles környezetben, – használja az alkalmazásbeállítások belül a [Azure-portálon] helyette.</span><span class="sxs-lookup"><span data-stu-id="c0456-285">Do not use this file to configure the Azure Mobile Apps SDK in production - use App Settings within the [Azure portal] instead.</span></span>  <span data-ttu-id="c0456-286">A *azureMobile.js* fájlba exportálja a konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="c0456-286">The *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="c0456-287">A leggyakrabban használt beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="c0456-287">The most common settings are:</span></span>

* <span data-ttu-id="c0456-288">Adatbázis-beállítások</span><span class="sxs-lookup"><span data-stu-id="c0456-288">Database Settings</span></span>
* <span data-ttu-id="c0456-289">Diagnosztikai naplózás beállításait</span><span class="sxs-lookup"><span data-stu-id="c0456-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="c0456-290">Alternatív CORS-beállítások</span><span class="sxs-lookup"><span data-stu-id="c0456-290">Alternate CORS Settings</span></span>

<span data-ttu-id="c0456-291">Példa *azureMobile.js* az előző adatbázis-beállítások megvalósításához fájl a következő:</span><span class="sxs-lookup"><span data-stu-id="c0456-291">An example *azureMobile.js* file implementing the preceding database settings follows:</span></span>

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

<span data-ttu-id="c0456-292">Javasoljuk, hogy adja hozzá *azureMobile.js* számára a *.gitignore* fájlt (vagy más verziókövetési figyelmen kívül hagyja a fájlt) a felhőben tárolt jelszavak megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c0456-292">We recommend that you add *azureMobile.js* to your *.gitignore* file (or other source code control ignore file) to prevent passwords from being stored in the cloud.</span></span>  <span data-ttu-id="c0456-293">Mindig adja meg az üzemi beállításait az alkalmazás beállításairól a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-293">Always configure production settings in App Settings within the [Azure portal].</span></span>

### <span data-ttu-id="c0456-294"><a name="howto-appsettings"></a>Útmutató: A mobilalkalmazás-beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0456-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="c0456-295">A legtöbb beállítását a *azureMobile.js* fájl rendelkezik egyenértékű Alkalmazásbeállítás a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-295">Most settings in the *azureMobile.js* file have an equivalent App Setting in the [Azure portal].</span></span>  <span data-ttu-id="c0456-296">Az alábbi lista segítségével állítsa be alkalmazását az alkalmazás beállításai:</span><span class="sxs-lookup"><span data-stu-id="c0456-296">Use the following list to configure your app in App Settings:</span></span>

| <span data-ttu-id="c0456-297">Alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="c0456-297">App Setting</span></span> | <span data-ttu-id="c0456-298">*azureMobile.js* beállítás</span><span class="sxs-lookup"><span data-stu-id="c0456-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="c0456-299">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0456-299">Description</span></span> | <span data-ttu-id="c0456-300">Érvényes értékek</span><span class="sxs-lookup"><span data-stu-id="c0456-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="c0456-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="c0456-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="c0456-302">név</span><span class="sxs-lookup"><span data-stu-id="c0456-302">name</span></span> |<span data-ttu-id="c0456-303">Az alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="c0456-303">The name of the app</span></span> |<span data-ttu-id="c0456-304">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c0456-304">string</span></span> |
| <span data-ttu-id="c0456-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="c0456-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="c0456-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="c0456-306">logging.level</span></span> |<span data-ttu-id="c0456-307">Naplózandó üzenetek minimális naplózási szint</span><span class="sxs-lookup"><span data-stu-id="c0456-307">Minimum log level of messages to log</span></span> |<span data-ttu-id="c0456-308">Hiba, figyelmeztetés, információ, részletes, hibakeresési silly</span><span class="sxs-lookup"><span data-stu-id="c0456-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="c0456-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="c0456-309">**MS_DebugMode**</span></span> |<span data-ttu-id="c0456-310">Hibakeresési</span><span class="sxs-lookup"><span data-stu-id="c0456-310">debug</span></span> |<span data-ttu-id="c0456-311">Engedélyezés vagy letiltás hibakeresési mód</span><span class="sxs-lookup"><span data-stu-id="c0456-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="c0456-312">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="c0456-312">true, false</span></span> |
| <span data-ttu-id="c0456-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="c0456-313">**MS_TableSchema**</span></span> |<span data-ttu-id="c0456-314">Data.Schema</span><span class="sxs-lookup"><span data-stu-id="c0456-314">data.schema</span></span> |<span data-ttu-id="c0456-315">SQL-táblák alapértelmezett séma neve</span><span class="sxs-lookup"><span data-stu-id="c0456-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="c0456-316">karakterlánc (alapértelmezett: dbo)</span><span class="sxs-lookup"><span data-stu-id="c0456-316">string (default: dbo)</span></span> |
| <span data-ttu-id="c0456-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="c0456-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="c0456-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="c0456-318">data.dynamicSchema</span></span> |<span data-ttu-id="c0456-319">Engedélyezés vagy letiltás hibakeresési mód</span><span class="sxs-lookup"><span data-stu-id="c0456-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="c0456-320">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="c0456-320">true, false</span></span> |
| <span data-ttu-id="c0456-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="c0456-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="c0456-322">verzió (a nem definiált beállítása)</span><span class="sxs-lookup"><span data-stu-id="c0456-322">version (set to undefined)</span></span> |<span data-ttu-id="c0456-323">Az X-ZUMO-kiszolgáló-Version fejlécnek letiltása</span><span class="sxs-lookup"><span data-stu-id="c0456-323">Disables the X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="c0456-324">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="c0456-324">true, false</span></span> |
| <span data-ttu-id="c0456-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="c0456-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="c0456-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="c0456-326">skipversioncheck</span></span> |<span data-ttu-id="c0456-327">Letiltja az ügyfél API-verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c0456-327">Disables the client API version check</span></span> |<span data-ttu-id="c0456-328">IGAZ, hamis</span><span class="sxs-lookup"><span data-stu-id="c0456-328">true, false</span></span> |

<span data-ttu-id="c0456-329">Egy Alkalmazásbeállítás beállítása:</span><span class="sxs-lookup"><span data-stu-id="c0456-329">To set an App Setting:</span></span>

1. <span data-ttu-id="c0456-330">Jelentkezzen be az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-330">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="c0456-331">Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="c0456-331">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="c0456-332">Alapértelmezés szerint a beállítások panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="c0456-332">The Settings blade opens by default.</span></span> <span data-ttu-id="c0456-333">Ha nem, kattintson a gombra **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c0456-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="c0456-334">Kattintson a **Alkalmazásbeállítások** az általános menüjében.</span><span class="sxs-lookup"><span data-stu-id="c0456-334">Click **Application settings** in the GENERAL menu.</span></span>
5. <span data-ttu-id="c0456-335">Görgessen az alkalmazásbeállítások szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="c0456-335">Scroll to the App Settings section.</span></span>
6. <span data-ttu-id="c0456-336">Ha az alkalmazás, beállítás már létezik, kattintson az Alkalmazásbeállítás értéke pedig az az érték szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="c0456-336">If your app setting already exists, click the value of the app setting to edit the value.</span></span>
7. <span data-ttu-id="c0456-337">Ha az alkalmazás nem létezik, adja meg az Alkalmazásbeállítás a kulcs és az érték mezőben megadott érték.</span><span class="sxs-lookup"><span data-stu-id="c0456-337">If your app setting does not exist, enter the App Setting in the Key box and the value in the Value box.</span></span>
8. <span data-ttu-id="c0456-338">Ha befejeződött, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c0456-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="c0456-339">A legtöbb alkalmazás beállításainak módosítása a szolgáltatás újraindítását igényli.</span><span class="sxs-lookup"><span data-stu-id="c0456-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="c0456-340"><a name="howto-use-sqlazure"></a>Útmutató: az üzemi adattároló SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="c0456-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="c0456-341">Az Azure SQL Database adattárként használata azonos Azure App Service-alkalmazás összes típusa.</span><span class="sxs-lookup"><span data-stu-id="c0456-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="c0456-342">Ha még nem tette meg, kövesse az alábbi lépéseket a mobilalkalmazás háttérrendszerének létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c0456-342">If you have not done so already, follow these steps to create a Mobile App backend.</span></span>

1. <span data-ttu-id="c0456-343">Jelentkezzen be az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-343">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="c0456-344">A felső bal oldali ablak, kattintson a **+ új** gomb > **Web + mobil** > **mobilalkalmazás**, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="c0456-344">In the top left of the window, click the **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="c0456-345">Az a **erőforráscsoport** mezőbe írja be a néven az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c0456-345">In the **Resource Group** box, enter the same name as your app.</span></span>
4. <span data-ttu-id="c0456-346">Az alapértelmezett App Service-csomag van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c0456-346">The Default App Service plan is selected.</span></span>  <span data-ttu-id="c0456-347">Ha úgy szeretné módosítani az App Service-csomag, akkor megteheti az App Service-csomag kattintva > **+ új**.</span><span class="sxs-lookup"><span data-stu-id="c0456-347">If you wish to change your App Service plan, you can do so by clicking the App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="c0456-348">Adja meg az új App Service-csomag nevét, és válasszon egy megfelelő helyet.</span><span class="sxs-lookup"><span data-stu-id="c0456-348">Provide a name of the new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="c0456-349">Kattintson a tarifacsomag, és válasszon egy megfelelő tarifacsomagot a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c0456-349">Click the Pricing tier and select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="c0456-350">Válassza ki **összes** több, mint az beállítások árképzési nézetre **szabad** és **megosztott**.</span><span class="sxs-lookup"><span data-stu-id="c0456-350">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="c0456-351">Miután kiválasztotta a tarifacsomagot, kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-351">Once you have selected the pricing tier, click the **Select** button.</span></span>  <span data-ttu-id="c0456-352">Vissza a **App Service-csomag** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0456-352">Back in the **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="c0456-353">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-353">Click **Create**.</span></span> <span data-ttu-id="c0456-354">A Mobile Apps-háttéralkalmazás kiépítés néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="c0456-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="c0456-355">Miután a Mobile Apps-háttéralkalmazás ki van építve, a portál megnyitja a **beállítások** panel a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="c0456-355">Once the Mobile App backend is provisioned, the portal opens the **Settings** blade for the Mobile App backend.</span></span>

<span data-ttu-id="c0456-356">A Mobile Apps-háttéralkalmazás létrehozása után ha szeretné, vagy egy meglévő SQL-adatbázis csatlakoztatása a Mobile Apps-háttéralkalmazás, vagy hozzon létre egy új SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c0456-356">Once the Mobile App backend is created, you can choose to either connect an existing SQL database to your Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="c0456-357">Ebben a szakaszban a Microsoft SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c0456-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="c0456-358">Ha már rendelkezik adatbázissal ugyanazon a helyen, a mobil-háttéralkalmazást, választhatja **meglévő adatbázis használata** , és válassza ki, hogy az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c0456-358">If you already have a database in the same location as the mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="c0456-359">Egy másik helyen található adatbázis használata nem ajánlott miatt magasabb késések fordulnak elő.</span><span class="sxs-lookup"><span data-stu-id="c0456-359">The use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="c0456-360">Kattintson az új Mobile Apps-háttéralkalmazás **beállítások** > **mobilalkalmazás** > **adatok** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c0456-360">In the new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="c0456-361">Az a **adatkapcsolat hozzáadása** panelen kattintson a **SQL adatbázis - kötelező beállítások konfigurálása** > **hozzon létre egy új adatbázist**.</span><span class="sxs-lookup"><span data-stu-id="c0456-361">In the **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="c0456-362">Adja meg az új adatbázis nevét a **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="c0456-362">Enter the name of the new database in the **Name** field.</span></span>
3. <span data-ttu-id="c0456-363">Kattintson a **Server**.</span><span class="sxs-lookup"><span data-stu-id="c0456-363">Click **Server**.</span></span>  <span data-ttu-id="c0456-364">Az a **új kiszolgáló** panelen adjon meg egy egyedi kiszolgálónevet a a **kiszolgálónév** mezőben, majd adjon meg egy megfelelő **kiszolgáló-rendszergazdai bejelentkezés** és **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c0456-364">In the **New server** blade, enter a unique server name in the **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="c0456-365">Győződjön meg arról **azure-szolgáltatások kiszolgálói hozzáférésének engedélyezése** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="c0456-365">Ensure **Allow azure services to access server** is checked.</span></span>  <span data-ttu-id="c0456-366">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0456-366">Click **OK**.</span></span>

    ![Egy Azure SQL-adatbázis létrehozása][6]
4. <span data-ttu-id="c0456-368">Az a **új adatbázis** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0456-368">On the **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="c0456-369">Vissza a **adatkapcsolat hozzáadása** panelen válassza **kapcsolati karakterlánc**, adja meg a bejelentkezési és az adatbázis létrehozásakor megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="c0456-369">Back on the **Add data connection** blade, select **Connection string**, enter the login and password that you provided when creating the database.</span></span>  <span data-ttu-id="c0456-370">Ha egy meglévő adatbázist használ, a bejelentkezési hitelesítő adatok megadása, hogy az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c0456-370">If you use an existing database, provide the login credentials for that database.</span></span>  <span data-ttu-id="c0456-371">Ha a megadott, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0456-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="c0456-372">Vissza a **adatkapcsolat hozzáadása** újra, kattintson a panel **OK** létrehozni az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="c0456-372">Back on the **Add data connection** blade again, click **OK** to create the database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="c0456-373">Az adatbázis létrehozása néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="c0456-373">Creation of the database can take a few minutes.</span></span>  <span data-ttu-id="c0456-374">Használja a **értesítések** kell figyelni a telepítés előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="c0456-374">Use the **Notifications** area to monitor the progress of the deployment.</span></span>  <span data-ttu-id="c0456-375">Nem folytatódni mindaddig, amíg az adatbázis sikeresen telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="c0456-375">Do not progress until the database has been deployed successfully.</span></span>  <span data-ttu-id="c0456-376">Miután sikeresen telepítve központilag, egy kapcsolati karakterláncot a mobil-háttéralkalmazást Alkalmazásbeállítások az SQL-adatbázispéldány jön létre.</span><span class="sxs-lookup"><span data-stu-id="c0456-376">Once successfully deployed, a Connection String is created for the SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="c0456-377">A app beállítás látható a **beállítások** > **Alkalmazásbeállítások** > **kapcsolati karakterláncok**.</span><span class="sxs-lookup"><span data-stu-id="c0456-377">You can see this app setting in the **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="c0456-378"><a name="howto-tables-auth"></a>Útmutató: hitelesítés megkövetelése a hozzáféréshez táblákhoz</span><span class="sxs-lookup"><span data-stu-id="c0456-378"><a name="howto-tables-auth"></a>How to: Require authentication for access to tables</span></span>
<span data-ttu-id="c0456-379">Ha szeretne az App Service-hitelesítés használatára a táblák végpont, konfigurálnia kell a App Service hitelesítés a [Azure-portálon] első.</span><span class="sxs-lookup"><span data-stu-id="c0456-379">If you wish to use App Service Authentication with the tables endpoint, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="c0456-380">Az Azure App Service hitelesítés konfigurálásával kapcsolatos további részletekért tekintse át a konfigurációs útmutató szeretne használni az identitásszolgáltató számára:</span><span class="sxs-lookup"><span data-stu-id="c0456-380">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="c0456-381">[Azure Active Directory-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-381">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="c0456-382">[Facebook-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-382">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="c0456-383">[Google-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-383">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="c0456-384">[A Microsoft Authentication konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-384">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="c0456-385">[Twitter-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-385">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="c0456-386">Minden tábla tulajdonsága hozzáférés táblájához való hozzáférés vezérlésére használható.</span><span class="sxs-lookup"><span data-stu-id="c0456-386">Each table has an access property that can be used to control access to the table.</span></span>  <span data-ttu-id="c0456-387">Az alábbi minta-hitelesítés használatához a statikusan megadott táblát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c0456-387">The following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="c0456-388">A hozzáférés tulajdonságot is igénybe vehet, értékek egyike</span><span class="sxs-lookup"><span data-stu-id="c0456-388">The access property can take one of three values</span></span>

* <span data-ttu-id="c0456-389">*Névtelen* azt jelzi, hogy az ügyfélalkalmazás hitelesítés nélkül-adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="c0456-389">*anonymous* indicates that the client application is allowed to read data without authentication</span></span>
* <span data-ttu-id="c0456-390">*hitelesített* azt jelzi, hogy az ügyfélalkalmazás egy érvényes hitelesítési jogkivonatot a kérelmet kell küldenie</span><span class="sxs-lookup"><span data-stu-id="c0456-390">*authenticated* indicates that the client application must send a valid authentication token with the request</span></span>
* <span data-ttu-id="c0456-391">*letiltott* azt jelzi, hogy ez a táblázat jelenleg le van tiltva</span><span class="sxs-lookup"><span data-stu-id="c0456-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="c0456-392">Ha a hozzáférési tulajdonság nincs definiálva, nem hitelesített elérését.</span><span class="sxs-lookup"><span data-stu-id="c0456-392">If the access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="c0456-393"><a name="howto-tables-getidentity"></a>Útmutató: hitelesítés jogcímek a táblák használata</span><span class="sxs-lookup"><span data-stu-id="c0456-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="c0456-394">Beállíthat különböző hitelesítési beállításakor igényelt jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="c0456-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="c0456-395">Ezeket a jogcímeket amelyek általában telepítéseinél nem használhatók a `context.user` objektum.</span><span class="sxs-lookup"><span data-stu-id="c0456-395">These claims are not normally available through the `context.user` object.</span></span>  <span data-ttu-id="c0456-396">Azonban azokat lekérheti használatával a `context.user.getIdentity()` metódust.</span><span class="sxs-lookup"><span data-stu-id="c0456-396">However, they can be retrieved using the `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="c0456-397">A `getIdentity()` metódus, amely egy objektum ígéret ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c0456-397">The `getIdentity()` method returns a Promise that resolves to an object.</span></span>  <span data-ttu-id="c0456-398">Az objektum van kulccsal (facebook, google, twitter, microsoftaccount vagy aad-ben) hitelesítési módszerrel.</span><span class="sxs-lookup"><span data-stu-id="c0456-398">The object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="c0456-399">Például ha korábban beállított Microsoft Account hitelesítése és a kérés e-mail cím jogcím, adhat hozzá az e-mail cím a rekord, a következő táblázat a tartományvezérlő:</span><span class="sxs-lookup"><span data-stu-id="c0456-399">For example, if you set up Microsoft Account authentication and request the email addresses claim, you can add the email address to the record with the following table controller:</span></span>

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
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="c0456-400">Milyen jogcímeket érhetők el, használja a webböngésző megtekintéséhez a `/.auth/me` végpont a webhely.</span><span class="sxs-lookup"><span data-stu-id="c0456-400">To see what claims are available, use a web browser to view the `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="c0456-401"><a name="howto-tables-disabled"></a>Útmutató: bizonyos táblájához műveletek való hozzáférés letiltása</span><span class="sxs-lookup"><span data-stu-id="c0456-401"><a name="howto-tables-disabled"></a>How to: Disable access to specific table operations</span></span>
<span data-ttu-id="c0456-402">Mellett a táblázatban szereplő, az access tulajdonság segítségével szabályozhatja az egyes műveletek.</span><span class="sxs-lookup"><span data-stu-id="c0456-402">In addition to appearing on the table, the access property can be used to control individual operations.</span></span>  <span data-ttu-id="c0456-403">Nincsenek négy műveletet:</span><span class="sxs-lookup"><span data-stu-id="c0456-403">There are four operations:</span></span>

* <span data-ttu-id="c0456-404">*olvassa el* a RESTful BEOLVASNI művelet a táblán</span><span class="sxs-lookup"><span data-stu-id="c0456-404">*read* is the RESTful GET operation on the table</span></span>
* <span data-ttu-id="c0456-405">*Helyezze be* a RESTful POST művelet a táblázat</span><span class="sxs-lookup"><span data-stu-id="c0456-405">*insert* is the RESTful POST operation on the table</span></span>
* <span data-ttu-id="c0456-406">*frissítés* a RESTful javítás művelet a táblán</span><span class="sxs-lookup"><span data-stu-id="c0456-406">*update* is the RESTful PATCH operation on the table</span></span>
* <span data-ttu-id="c0456-407">*Törlés* a RESTful DELETE művelet a táblán van</span><span class="sxs-lookup"><span data-stu-id="c0456-407">*delete* is the RESTful DELETE operation on the table</span></span>

<span data-ttu-id="c0456-408">Például Kezdésként érdemes lehet egy olyan írásvédett nem hitelesített táblázat:</span><span class="sxs-lookup"><span data-stu-id="c0456-408">For example, you may wish to provide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="c0456-409"><a name="howto-tables-query"></a>Hogyan: módosítsa a táblázat műveletek használt lekérdezés</span><span class="sxs-lookup"><span data-stu-id="c0456-409"><a name="howto-tables-query"></a>How to: Adjust the query that is used with table operations</span></span>
<span data-ttu-id="c0456-410">Kötelező megadni a tábla műveletek arra, hogy az adatok korlátozott nézete.</span><span class="sxs-lookup"><span data-stu-id="c0456-410">A common requirement for table operations is to provide a restricted view of the data.</span></span>  <span data-ttu-id="c0456-411">Például előfordulhat, hogy adjon meg egy táblát, amely a hitelesített felhasználói azonosítóval van megjelölve, hogy csak olvasható vagy a saját rekordok frissítése.</span><span class="sxs-lookup"><span data-stu-id="c0456-411">For example, you may provide a table that is tagged with the authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="c0456-412">A következő tábla definícióját tartalmazza ezt a funkciót:</span><span class="sxs-lookup"><span data-stu-id="c0456-412">The following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="c0456-413">Műveletek általában a lekérdezés végrehajtása rendelkezik egy lekérdezés tulajdonságot, amelynek módosíthatja, a where záradékban.</span><span class="sxs-lookup"><span data-stu-id="c0456-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="c0456-414">A lekérdezés tulajdonság egy [QueryJS] egy OData-lekérdezés konvertálása valamilyen, amely képes az adatok háttérbeli használt objektum.</span><span class="sxs-lookup"><span data-stu-id="c0456-414">The query property is a [QueryJS] object that is used to convert an OData query to something that the data backend can process.</span></span>  <span data-ttu-id="c0456-415">Egyszerű egyenlőség esetekben (például az előző) térképre használhatók.</span><span class="sxs-lookup"><span data-stu-id="c0456-415">For simple equality cases (like the preceding one), a map can be used.</span></span> <span data-ttu-id="c0456-416">SQL záradékokat is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="c0456-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="c0456-417"><a name="howto-tables-softdelete"></a>Útmutató: egy helyreállítható törlésre konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0456-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="c0456-418">Helyreállítható törlésre ténylegesen nem törli a rekordok.</span><span class="sxs-lookup"><span data-stu-id="c0456-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="c0456-419">Ehelyett azt jelöli meg azokat a törölt oszlop true értékre állításával az adatbázis törlése.</span><span class="sxs-lookup"><span data-stu-id="c0456-419">Instead it marks them as deleted within the database by setting the deleted column to true.</span></span>  <span data-ttu-id="c0456-420">Az Azure Mobile Apps SDK automatikusan eltávolítja helyreállíthatóan törölt rekordok eredmények, kivéve, ha a mobileszköz ügyfél SDK IncludeDeleted() használja.</span><span class="sxs-lookup"><span data-stu-id="c0456-420">The Azure Mobile Apps SDK automatically removes soft-deleted records from results unless the Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="c0456-421">Egy helyreállítható törlésre tábla konfigurálásához állítsa a `softDelete` tulajdonság a tábla szolgáltatásdefiníciós fájlban:</span><span class="sxs-lookup"><span data-stu-id="c0456-421">To configure a table for soft delete, set the `softDelete` property in the table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="c0456-422">Létre kell hoznia egy olyan mechanizmus kiürítése rekordok - vagy a ügyfélalkalmazás keresztül webjobs-feladat, Azure-függvény, vagy egy egyéni API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="c0456-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="c0456-423"><a name="howto-tables-seeding"></a>Útmutató: az adatbázis adatokkal magtípusú leképezést</span><span class="sxs-lookup"><span data-stu-id="c0456-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="c0456-424">Amikor egy új alkalmazást hoz létre, érdemes rendezi az adatokat tartalmazó táblát.</span><span class="sxs-lookup"><span data-stu-id="c0456-424">When creating a new application, you may wish to seed a table with data.</span></span>  <span data-ttu-id="c0456-425">Ezt megteheti a tábla definícióját JavaScript-fájlon belüli az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c0456-425">This can be done within the table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
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

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="c0456-426">Az adatok összehangolása csak végre, ha az Azure Mobile Apps SDK hozta létre a tábla.</span><span class="sxs-lookup"><span data-stu-id="c0456-426">Seeding of data is only done when the table is created by the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="c0456-427">Ha a tábla már létezik az adatbázisban, a program a táblába beszúrta nincsenek adatok.</span><span class="sxs-lookup"><span data-stu-id="c0456-427">If the table already exists within the database, no data is injected into the table.</span></span>  <span data-ttu-id="c0456-428">Ha dinamikus séma be van kapcsolva, majd a séma van következtetni a kiemelt adatokat.</span><span class="sxs-lookup"><span data-stu-id="c0456-428">If dynamic schema is turned on, then the schema is inferred from the seeded data.</span></span>

<span data-ttu-id="c0456-429">Azt javasoljuk, hogy explicit módon meghívja a `tables.initialize()` metódus fut a szolgáltatás indulásakor a tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c0456-429">We recommend that you explicitly call the `tables.initialize()` method to create the table when the service starts running.</span></span>

### <span data-ttu-id="c0456-430"><a name="Swagger"></a>Útmutató: a Swagger-támogatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c0456-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="c0456-431">Az Azure App Service Mobile Apps mellékelt beépített [Swagger] támogatja.</span><span class="sxs-lookup"><span data-stu-id="c0456-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="c0456-432">A Swagger-támogatásának engedélyezése, először telepítenie kell a swagger felhasználói felület a függőség beállításához:</span><span class="sxs-lookup"><span data-stu-id="c0456-432">To enable Swagger support, first install the swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="c0456-433">A telepítést követően engedélyezheti a Swagger-támogatás az Azure Mobile Apps-konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c0456-433">Once installed, you can enable Swagger support in the Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="c0456-434">Ön valószínűleg csak szeretné fejlesztési kiadásai Swagger-támogatás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c0456-434">You probably only want to enable Swagger support in development editions.</span></span>  <span data-ttu-id="c0456-435">Ehhez felügyelniük a `NODE_ENV` Alkalmazásbeállítás:</span><span class="sxs-lookup"><span data-stu-id="c0456-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="c0456-436">A swagger-végpont nem található: http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="c0456-436">The swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="c0456-437">A Swagger felhasználói felületén keresztül érheti el a `/swagger/ui` végpont.</span><span class="sxs-lookup"><span data-stu-id="c0456-437">You can access the Swagger UI via the `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="c0456-438">Ha a teljes alkalmazás közötti hitelesítés szükséges, a Swagger hibát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c0456-438">if you choose to require authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="c0456-439">A legjobb eredmények elérése érdekében válassza ki, hogy lehetővé tegyék a nem hitelesített kérelmek használatával az Azure App Service Authentication / engedélyezési beállításait, majd vezérlőt használatával a `table.access` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c0456-439">For best results, choose to allow unauthenticated requests through in the Azure App Service Authentication / Authorization settings, then control authentication using the `table.access` property.</span></span>

<span data-ttu-id="c0456-440">A Swagger beállítással is hozzáadhat a `azureMobile.js` fájlt, ha azt szeretné csak Swagger támogatási helyileg fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="c0456-440">You can also add the Swagger option to your `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="c0456-441"><a name="push">Leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="c0456-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="c0456-442">Mobile Apps integrálja az Azure Notification Hubs ahhoz, hogy célzott leküldéses értesítések küldésére eszközök millióira minden fő platformon.</span><span class="sxs-lookup"><span data-stu-id="c0456-442">Mobile Apps integrates with Azure Notification Hubs to enable you to send targeted push notifications to millions of devices across all major platforms.</span></span> <span data-ttu-id="c0456-443">A Notification Hubs használatával leküldéses értesítéseket küldhet az iOS, Android és Windows rendszerű eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="c0456-443">By using Notification Hubs, you can send push notifications to iOS, Android and Windows devices.</span></span> <span data-ttu-id="c0456-444">A Notification hubs használatával elvégezhető az összes kapcsolatos további információkért lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0456-444">To learn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="c0456-445"></a><a name="send-push"></a>Hogyan: leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="c0456-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="c0456-446">A következő kód bemutatja, hogyan küldhet leküldéses objektum leküldéses értesítések a regisztrált iOS-eszközök:</span><span class="sxs-lookup"><span data-stu-id="c0456-446">The following code shows how to use the push object to send a broadcast push notification to registered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="c0456-447">Az ügyfél egy sablon leküldéses regisztrációs létrehozásával is inkább egy sablon leküldéses üzenetet küld eszközök az összes támogatott platformon.</span><span class="sxs-lookup"><span data-stu-id="c0456-447">By creating a template push registration from the client, you can instead send a template push message to devices on all supported platforms.</span></span> <span data-ttu-id="c0456-448">A következő kód bemutatja, hogyan sablon értesítést küldhet:</span><span class="sxs-lookup"><span data-stu-id="c0456-448">The following code shows how to send a template notification:</span></span>

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <span data-ttu-id="c0456-449"><a name="push-user"></a>Útmutató: egy hitelesített felhasználó számára címkék használatával leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="c0456-449"><a name="push-user"></a>How to: Send push notifications to an authenticated user using tags</span></span>
<span data-ttu-id="c0456-450">Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan hozzáadódik a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="c0456-450">When an authenticated user registers for push notifications, a user ID tag is automatically added to the registration.</span></span> <span data-ttu-id="c0456-451">Ezt a címkét használatával küldhet leküldéses értesítéseket egy adott felhasználó által regisztrált összes eszközt.</span><span class="sxs-lookup"><span data-stu-id="c0456-451">By using this tag, you can send push notifications to all devices registered by a specific user.</span></span> <span data-ttu-id="c0456-452">Az alábbi kód lekérdezi a kérelmező felhasználó biztonsági azonosítója és minden eszköz regisztrálása az adott felhasználó sablon leküldéses értesítést küld:</span><span class="sxs-lookup"><span data-stu-id="c0456-452">The following code gets the SID of user making the request and sends a template push notification to every device registration for that user:</span></span>

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="c0456-453">Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="c0456-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="c0456-454"><a name="CustomAPI"></a>Egyéni API-k</span><span class="sxs-lookup"><span data-stu-id="c0456-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="c0456-455"><a name="howto-customapi-basic"></a>Hogyan: Adja meg egy egyéni API</span><span class="sxs-lookup"><span data-stu-id="c0456-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="c0456-456">Az adatok hozzáférésének API a /tables végpont, mellett Azure Mobile Apps is biztosít egyéni API.</span><span class="sxs-lookup"><span data-stu-id="c0456-456">In addition to the data access API via the /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="c0456-457">Egyéni API-k a definíciói hasonló módon határozza meg, és hozzáférhet ugyanazokat létesítményekben, például a hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="c0456-457">Custom APIs are defined in a similar way to the table definitions and can access all the same facilities, including authentication.</span></span>

<span data-ttu-id="c0456-458">Ha szeretné használni az App Service hitelesítés egy egyéni API-t, konfigurálnia kell a App Service hitelesítés a [Azure-portálon] első.</span><span class="sxs-lookup"><span data-stu-id="c0456-458">If you wish to use App Service Authentication with a Custom API, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="c0456-459">Az Azure App Service hitelesítés konfigurálásával kapcsolatos további részletekért tekintse át a konfigurációs útmutató szeretne használni az identitásszolgáltató számára:</span><span class="sxs-lookup"><span data-stu-id="c0456-459">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="c0456-460">[Azure Active Directory-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-460">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="c0456-461">[Facebook-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-461">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="c0456-462">[Google-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-462">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="c0456-463">[A Microsoft Authentication konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-463">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="c0456-464">[Twitter-hitelesítés konfigurálása]</span><span class="sxs-lookup"><span data-stu-id="c0456-464">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="c0456-465">Egyéni API-k sokkal ugyanúgy, mint a táblák API vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="c0456-465">Custom APIs are defined in much the same way as the Tables API.</span></span>

1. <span data-ttu-id="c0456-466">Hozzon létre egy **api** könyvtár</span><span class="sxs-lookup"><span data-stu-id="c0456-466">Create an **api** directory</span></span>
2. <span data-ttu-id="c0456-467">Az API definition JavaScript fájl létrehozása a **api** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c0456-467">Create an API definition JavaScript file in the **api** directory.</span></span>
3. <span data-ttu-id="c0456-468">Az importálás módszer használatával importálja a **api** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c0456-468">Use the import method to import the **api** directory.</span></span>

<span data-ttu-id="c0456-469">Ez a korábbi használtuk az alkalmazáson belüli basic minta alapján prototípus api-definíció.</span><span class="sxs-lookup"><span data-stu-id="c0456-469">Here is the prototype api definition based on the basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="c0456-470">Vegyünk egy példát a kiszolgálóra dátum történő visszaadó API a *Date.now()* metódust.</span><span class="sxs-lookup"><span data-stu-id="c0456-470">Let's take an example API that returns the server date using the *Date.now()* method.</span></span>  <span data-ttu-id="c0456-471">A api/date.js fájl itt található:</span><span class="sxs-lookup"><span data-stu-id="c0456-471">Here is the api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="c0456-472">Egyes paraméterek egyike a szabványos RESTful HTTP-parancsokat - GET, POST, javítását vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="c0456-472">Each parameter is one of the standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="c0456-473">A metódus egy szabványos [ExpressJS köztes] függvény, amelyet a kimeneti küld.</span><span class="sxs-lookup"><span data-stu-id="c0456-473">The method is a standard [ExpressJS Middleware] function that sends the required output.</span></span>

### <span data-ttu-id="c0456-474"><a name="howto-customapi-auth"></a>Útmutató: hitelesítés egy egyéni API eléréséhez szükséges</span><span class="sxs-lookup"><span data-stu-id="c0456-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access to a custom API</span></span>
<span data-ttu-id="c0456-475">Az Azure Mobile Apps SDK hitelesítés a táblák végpont és az egyéni API-k ugyanúgy valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="c0456-475">Azure Mobile Apps SDK implements authentication in the same way for both the tables endpoint and custom APIs.</span></span>  <span data-ttu-id="c0456-476">Hitelesítés hozzáadása az előző szakaszban létrehozott API-hoz, vegye fel egy **hozzáférés** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="c0456-476">To add authentication to the API developed in the previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="c0456-477">A konkrét műveletek is hitelesítési adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="c0456-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="c0456-478">Ugyanezt a tokent a táblák végpont használt hitelesítést igénylő, egyéni API-kat kell használni.</span><span class="sxs-lookup"><span data-stu-id="c0456-478">The same token that is used for the tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="c0456-479"><a name="howto-customapi-auth"></a>Hogyan: nagy fájlfeltöltéseket kezelése</span><span class="sxs-lookup"><span data-stu-id="c0456-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="c0456-480">Az Azure Mobile Apps SDK-t használ a [törzs-elemző köztes](https://github.com/expressjs/body-parser) elfogadásához és dekódolási törzs tartalma a jelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c0456-480">Azure Mobile Apps SDK uses the [body-parser middleware](https://github.com/expressjs/body-parser) to accept and decode body content in your submission.</span></span>  <span data-ttu-id="c0456-481">Törzs-elemző nagyobb fájlfeltöltések fogadásához előre konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="c0456-481">You can pre-configure body-parser to accept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="c0456-482">A fájl nem base-64 kódolású továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="c0456-482">The file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="c0456-483">Ez növeli a tényleges feltöltés méretét (és így mérete figyelembe kell venni).</span><span class="sxs-lookup"><span data-stu-id="c0456-483">This increases the size of the actual upload (and hence the size you must account for).</span></span>

### <span data-ttu-id="c0456-484"><a name="howto-customapi-sql"></a>Útmutató: egyéni SQL-utasítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c0456-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="c0456-485">Az Azure Mobile Apps SDK lehetővé teszi a hozzáférést a teljes környezet a request objektumon keresztül lehetővé téve a paraméteres SQL utasítást, hogy az adott szolgáltató egyszerűen hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="c0456-485">The Azure Mobile Apps SDK allows access to the entire Context through the request object, allowing you to execute parameterized SQL statements to the defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="c0456-486"><a name="Debugging"></a>Hibakeresés, könnyen táblák és egyszerű API-k</span><span class="sxs-lookup"><span data-stu-id="c0456-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="c0456-487"><a name="howto-diagnostic-logs"></a>Hogyan: hibakeresési, diagnosztizálása és elhárítása az Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="c0456-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="c0456-488">Az Azure App Service számos Hibakeresés és hibaelhárítási módszerekről a Node.js-alkalmazások biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0456-488">The Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="c0456-489">Ismerkedés a Node.js mobil-háttéralkalmazás elhárításához a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="c0456-489">Refer to the following articles to get started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="c0456-490">[Az Azure App Service figyelése]</span><span class="sxs-lookup"><span data-stu-id="c0456-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="c0456-491">[Az Azure App Service diagnosztikai naplózás engedélyezése]</span><span class="sxs-lookup"><span data-stu-id="c0456-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="c0456-492">[A Visual Studio egy Azure App Service hibaelhárítása]</span><span class="sxs-lookup"><span data-stu-id="c0456-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="c0456-493">NODE.js-alkalmazások hozzáférhetnek a diagnosztikai naplófájl eszközök széles skáláját.</span><span class="sxs-lookup"><span data-stu-id="c0456-493">Node.js applications have access to a wide range of diagnostic log tools.</span></span>  <span data-ttu-id="c0456-494">Belsőleg, az Azure Mobile Apps Node.js SDK használ [Winston] diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="c0456-494">Internally, the Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="c0456-495">Automatikusan naplózását hibakeresési mód engedélyezésével, illetve a **MS_DebugMode** Alkalmazásbeállítás igaz értékű a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-495">Logging is automatically enabled by enabling debug mode or by setting the **MS_DebugMode** app setting to true in the [Azure portal].</span></span> <span data-ttu-id="c0456-496">A diagnosztikai naplók létrehozott naplók jelennek meg a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="c0456-496">Generated logs appear in the Diagnostic Logs on the [Azure portal].</span></span>

### <span data-ttu-id="c0456-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Útmutató: egyszerű táblák használata az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c0456-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in the Azure portal</span></span>
<span data-ttu-id="c0456-498">Egyszerű táblák a portálon lehetővé teszik, hogy létrehozása és használata táblákat közvetlenül a portálon.</span><span class="sxs-lookup"><span data-stu-id="c0456-498">Easy Tables in the portal let you create and work with tables right in the portal.</span></span> <span data-ttu-id="c0456-499">Az App Service-szerkesztővel tábla műveletek akár szerkesztheti is.</span><span class="sxs-lookup"><span data-stu-id="c0456-499">You can even edit table operations using the App Service Editor.</span></span>

<span data-ttu-id="c0456-500">Amikor rákattint **könnyen táblák** a háttér-webhely beállításai hozzáadása, módosítása és törlése egy tábla.</span><span class="sxs-lookup"><span data-stu-id="c0456-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="c0456-501">Megtekintheti a tábla adatai is.</span><span class="sxs-lookup"><span data-stu-id="c0456-501">You can also see data in the table.</span></span>

![A könnyen táblázatok használata](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="c0456-503">Az alábbi parancsokat a táblázat parancssávon érhetők el:</span><span class="sxs-lookup"><span data-stu-id="c0456-503">The following commands are available on the command bar for a table:</span></span>

* <span data-ttu-id="c0456-504">**Engedélyek módosítása** - módosítási olvasási engedéllyel, beszúrása, frissítése és törlési műveletek a táblában.</span><span class="sxs-lookup"><span data-stu-id="c0456-504">**Change permissions** - modify the permission for read, insert, update and delete operations on the table.</span></span>
  <span data-ttu-id="c0456-505">Is, hogy engedélyezze a névtelen hozzáférést, a hitelesítés szükséges, vagy tiltsa le a műveletet az elérésére.</span><span class="sxs-lookup"><span data-stu-id="c0456-505">Options are to allow anonymous access, to require authentication, or to disable all access to the operation.</span></span>
* <span data-ttu-id="c0456-506">**Parancsfájl szerkesztése** -a parancsfájl a következő táblázatban a App Service-szerkesztőben van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="c0456-506">**Edit script** - the script file for the table is opened in the App Service Editor.</span></span>
* <span data-ttu-id="c0456-507">**Séma kezelése** - hozzáadása vagy oszlopok törlése vagy a tábla index módosítása.</span><span class="sxs-lookup"><span data-stu-id="c0456-507">**Manage schema** - add or delete columns or change the table index.</span></span>
* <span data-ttu-id="c0456-508">**Törölje a jelet tábla** -csonkolja a meglévő tábla kell az összes adat sorok törléséhez, de a séma hagyja változatlanul.</span><span class="sxs-lookup"><span data-stu-id="c0456-508">**Clear table** - truncates an existing table be deleting all data rows but leaving the schema unchanged.</span></span>
* <span data-ttu-id="c0456-509">**Sorok törlése** -egyedi sornyi adatot törli.</span><span class="sxs-lookup"><span data-stu-id="c0456-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="c0456-510">**A folyamatos átviteli naplók megtekintése** -csatlakoztatja a helyhez a folyamatos átviteli naplózási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0456-510">**View streaming logs** - connects you to the streaming log service for your site.</span></span>

### <span data-ttu-id="c0456-511"><a name="work-easy-apis"></a>Útmutató: egyszerű API-k használata az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c0456-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in the Azure portal</span></span>
<span data-ttu-id="c0456-512">A portál egyszerű API-k lehetővé teszik, hogy létrehozása és használata egyéni API-k közvetlenül a portálon.</span><span class="sxs-lookup"><span data-stu-id="c0456-512">Easy APIs in the portal let you create and work with custom APIs right in the portal.</span></span> <span data-ttu-id="c0456-513">Az App Service-szerkesztővel API parancsfájlok szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="c0456-513">You can edit API scripts using the App Service Editor.</span></span>

<span data-ttu-id="c0456-514">Amikor rákattint **egyszerű API-k** a háttér-webhely beállításai hozzáadása, módosítása és egy egyéni API-végpont törlése.</span><span class="sxs-lookup"><span data-stu-id="c0456-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Egyszerű API-khoz](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="c0456-516">A portál egy adott HTTP-művelet hozzáférési engedélyeinek módosítása, az API-parancsfájlt a App Service-szerkesztőben szerkesztheti vagy a folyamatos átviteli naplók megtekintése.</span><span class="sxs-lookup"><span data-stu-id="c0456-516">In the portal, you can change the access permissions for a given HTTP action, edit the API script file in the App Service Editor, or view the streaming logs.</span></span>

### <span data-ttu-id="c0456-517"><a name="online-editor"></a>Útmutató: a App Service-szerkesztőben kód szerkesztése</span><span class="sxs-lookup"><span data-stu-id="c0456-517"><a name="online-editor"></a>How to: Edit code in the App Service Editor</span></span>
<span data-ttu-id="c0456-518">Az Azure portál segítségével anélkül, hogy a projekt letöltése a helyi számítógépen a Node.js háttérrendszer parancsprogramok a App Service-szerkesztőben szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="c0456-518">The Azure portal lets you edit your Node.js backend script files in the App Service Editor without having to download the project to your local computer.</span></span> <span data-ttu-id="c0456-519">Az online szerkesztő parancsfájlok szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="c0456-519">To edit script files in the online editor:</span></span>

1. <span data-ttu-id="c0456-520">A mobilalkalmazás-háttérrendszer paneljén kattintson **összes beállítás** > vagy **könnyen táblák** vagy **egyszerű API-k**, egy táblázatot vagy API-t, majd **parancsfájl szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="c0456-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="c0456-521">A parancsfájl a App Service-szerkesztőben van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="c0456-521">The script file is opened in the App Service Editor.</span></span>

    ![App Service-szerkesztő](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="c0456-523">A módosításokat a kódfájl online-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="c0456-523">Make your changes to the code file in the online editor.</span></span> <span data-ttu-id="c0456-524">Változások automatikusan mentése közben.</span><span class="sxs-lookup"><span data-stu-id="c0456-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
<span data-ttu-id="c0456-525">[Android ügyfél gyors üzembe helyezés]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-525">[Android Client QuickStart]: app-service-mobile-android-get-started.md</span></span>
<span data-ttu-id="c0456-526">[Apache Cordova-ügyfél gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-526">[Apache Cordova Client QuickStart]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="c0456-527">[iOS ügyfél gyors üzembe helyezés]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-527">[iOS Client QuickStart]: app-service-mobile-ios-get-started.md</span></span>
<span data-ttu-id="c0456-528">[Xamarin.iOS ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-528">[Xamarin.iOS Client QuickStart]: app-service-mobile-xamarin-ios-get-started.md</span></span>
<span data-ttu-id="c0456-529">[Xamarin.Android ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-529">[Xamarin.Android Client QuickStart]: app-service-mobile-xamarin-android-get-started.md</span></span>
<span data-ttu-id="c0456-530">[Xamarin.Forms-ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-forms-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-530">[Xamarin.Forms Client QuickStart]: app-service-mobile-xamarin-forms-get-started.md</span></span>
<span data-ttu-id="c0456-531">[A Windows Store ügyfél gyors üzembe helyezés]: app-service-mobile-windows-store-dotnet-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c0456-531">[Windows Store Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md</span></span>
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
<span data-ttu-id="c0456-532">[offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="c0456-532">[offline data sync]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="c0456-533">[Azure Active Directory-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="c0456-533">[How to configure Azure Active Directory Authentication]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>
<span data-ttu-id="c0456-534">[Facebook-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-facebook-authentication.md</span><span class="sxs-lookup"><span data-stu-id="c0456-534">[How to configure Facebook Authentication]: app-service-mobile-how-to-configure-facebook-authentication.md</span></span>
<span data-ttu-id="c0456-535">[Google-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-google-authentication.md</span><span class="sxs-lookup"><span data-stu-id="c0456-535">[How to configure Google Authentication]: app-service-mobile-how-to-configure-google-authentication.md</span></span>
<span data-ttu-id="c0456-536">[A Microsoft Authentication konfigurálása]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="c0456-536">[How to configure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="c0456-537">[Twitter-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-twitter-authentication.md</span><span class="sxs-lookup"><span data-stu-id="c0456-537">[How to configure Twitter Authentication]: app-service-mobile-how-to-configure-twitter-authentication.md</span></span>
<span data-ttu-id="c0456-538">[Azure App Service telepítési útmutató]: ../app-service-web/web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="c0456-538">[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md</span></span>
<span data-ttu-id="c0456-539">[Az Azure App Service figyelése]: ../app-service-web/web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="c0456-539">[Monitoring an Azure App Service]: ../app-service-web/web-sites-monitor.md</span></span>
<span data-ttu-id="c0456-540">[Az Azure App Service diagnosztikai naplózás engedélyezése]: ../app-service-web/web-sites-enable-diagnostic-log.md</span><span class="sxs-lookup"><span data-stu-id="c0456-540">[Enable Diagnostic Logging in Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md</span></span>
<span data-ttu-id="c0456-541">[A Visual Studio egy Azure App Service hibaelhárítása]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span><span class="sxs-lookup"><span data-stu-id="c0456-541">[Troubleshoot an Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span></span>
<span data-ttu-id="c0456-542">[a csomópont verzió megadása]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="c0456-542">[specify the Node Version]: ../nodejs-specify-node-version-azure-apps.md</span></span>
<span data-ttu-id="c0456-543">[csomópont modulok használata]: ../nodejs-use-node-modules-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="c0456-543">[use Node modules]: ../nodejs-use-node-modules-azure-apps.md</span></span>
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
<span data-ttu-id="c0456-544">[Express]: http://expressjs.com/</span><span class="sxs-lookup"><span data-stu-id="c0456-544">[Express]: http://expressjs.com/</span></span>
<span data-ttu-id="c0456-545">[Swagger]: http://swagger.io/</span><span class="sxs-lookup"><span data-stu-id="c0456-545">[Swagger]: http://swagger.io/</span></span>

<span data-ttu-id="c0456-546">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="c0456-546">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="c0456-547">[OData]: http://www.odata.org</span><span class="sxs-lookup"><span data-stu-id="c0456-547">[OData]: http://www.odata.org</span></span>
<span data-ttu-id="c0456-548">[ígéret]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span><span class="sxs-lookup"><span data-stu-id="c0456-548">[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span></span>
<span data-ttu-id="c0456-549">[basicapp mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span><span class="sxs-lookup"><span data-stu-id="c0456-549">[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span></span>
<span data-ttu-id="c0456-550">[todo mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span><span class="sxs-lookup"><span data-stu-id="c0456-550">[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span></span>
<span data-ttu-id="c0456-551">[minták directory a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="c0456-551">[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span></span>
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
<span data-ttu-id="c0456-552">[QueryJS]: https://github.com/Azure/queryjs</span><span class="sxs-lookup"><span data-stu-id="c0456-552">[QueryJS]: https://github.com/Azure/queryjs</span></span>
<span data-ttu-id="c0456-553">[Node.js eszközök 1.1 a Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span><span class="sxs-lookup"><span data-stu-id="c0456-553">[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span></span>
<span data-ttu-id="c0456-554">[mssql Node.js csomag]: https://www.npmjs.com/package/mssql</span><span class="sxs-lookup"><span data-stu-id="c0456-554">[mssql Node.js package]: https://www.npmjs.com/package/mssql</span></span>
<span data-ttu-id="c0456-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span><span class="sxs-lookup"><span data-stu-id="c0456-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span></span>
<span data-ttu-id="c0456-556">[ExpressJS köztes]: http://expressjs.com/guide/using-middleware.html</span><span class="sxs-lookup"><span data-stu-id="c0456-556">[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html</span></span>
<span data-ttu-id="c0456-557">[Winston]: https://github.com/winstonjs/winston</span><span class="sxs-lookup"><span data-stu-id="c0456-557">[Winston]: https://github.com/winstonjs/winston</span></span>
