---
title: "egy Sails.js webalkalmazás az App Service alkalmazás tooAzure aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy egy Node.js-alkalmazás Azure App Service szolgáltatásban. Az oktatóanyag bemutatja, hogyan toodeploy egy Sails.js webalkalmazás."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a><span data-ttu-id="73887-104">Sails.js webalkalmazás app tooAzure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="73887-104">Deploy a Sails.js web app tooAzure App Service</span></span>
<span data-ttu-id="73887-105">Az oktatóanyag bemutatja, hogyan toodeploy egy Sails.js app tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="73887-105">This tutorial shows you how toodeploy a Sails.js app tooAzure App Service.</span></span> <span data-ttu-id="73887-106">Hello folyamatban, néhány általános információkat, hogyan lehet glean tooconfigure a Node.js-alkalmazás toorun az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="73887-106">In hello process, you can glean some general knowledge on how tooconfigure your Node.js app toorun in App Service.</span></span>

<span data-ttu-id="73887-107">Itt megtudhatja, hasznos képességek, például:</span><span class="sxs-lookup"><span data-stu-id="73887-107">Here, you will learn useful skills like:</span></span>

* <span data-ttu-id="73887-108">Egy App Service-ben futtassa Sails.js alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="73887-108">Configure a Sails.js app run in App Service.</span></span>
* <span data-ttu-id="73887-109">Telepítsen egy alkalmazást tooApp szolgáltatás hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="73887-109">Deploy an app tooApp Service from hello command line.</span></span>
* <span data-ttu-id="73887-110">Olvassa el a stderr-en és a stdout naplók tootroubleshoot telepítési problémáit.</span><span class="sxs-lookup"><span data-stu-id="73887-110">Read stderr and stdout logs tootroubleshoot any deployment issues.</span></span>
* <span data-ttu-id="73887-111">Környezeti változók verziókezelő kívül tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="73887-111">Store environment variables outside of source control.</span></span>
* <span data-ttu-id="73887-112">Hozzáférés az Azure környezeti változókat az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="73887-112">Access Azure environment variables from your app.</span></span>
* <span data-ttu-id="73887-113">Csatlakozás tooa adatbázis (MongoDB).</span><span class="sxs-lookup"><span data-stu-id="73887-113">Connect tooa database (MongoDB).</span></span>

<span data-ttu-id="73887-114">Sails.js ismeretét kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="73887-114">You should have working knowledge of Sails.js.</span></span> <span data-ttu-id="73887-115">Ebben az oktatóanyagban nincs meg problémákkal kapcsolatos toorunning Sail.js általában tervezett toohelp.</span><span class="sxs-lookup"><span data-stu-id="73887-115">This tutorial is not intended toohelp you with issues related toorunning Sail.js in general.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="73887-116">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="73887-116">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="73887-117">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="73887-117">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="73887-118">[Az Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="73887-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="73887-119">[Az Azure CLI 2.0](app-service-web-nodejs-sails.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="73887-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73887-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73887-120">Prerequisites</span></span>
* [<span data-ttu-id="73887-121">Node.js</span><span class="sxs-lookup"><span data-stu-id="73887-121">Node.js</span></span>](https://nodejs.org/)
* [<span data-ttu-id="73887-122">Sails.js</span><span class="sxs-lookup"><span data-stu-id="73887-122">Sails.js</span></span>](http://sailsjs.org/get-started)
* [<span data-ttu-id="73887-123">Git</span><span class="sxs-lookup"><span data-stu-id="73887-123">Git</span></span>](http://www.git-scm.com/downloads)
* [<span data-ttu-id="73887-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="73887-124">Azure CLI 2.0</span></span>](/cli/azure/install-az-cli2)
* <span data-ttu-id="73887-125">Egy Microsoft Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="73887-125">A Microsoft Azure account.</span></span> <span data-ttu-id="73887-126">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="73887-126">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="73887-127">Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges.</span><span class="sxs-lookup"><span data-stu-id="73887-127">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="73887-128">Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.</span><span class="sxs-lookup"><span data-stu-id="73887-128">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a><span data-ttu-id="73887-129">1. lépés: Hozzon létre és konfigurálhat egy Sails.js alkalmazást helyileg</span><span class="sxs-lookup"><span data-stu-id="73887-129">Step 1: Create and configure a Sails.js app locally</span></span>
<span data-ttu-id="73887-130">Először gyorsan létrehozhat egy alapértelmezett Sails.js alkalmazást a fejlesztési környezetben az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="73887-130">First, quickly create a default Sails.js app in your development environment by following these steps:</span></span>

1. <span data-ttu-id="73887-131">Nyissa meg hello parancssori terminált az Ön által választott és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="73887-131">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span>
2. <span data-ttu-id="73887-132">Hozzon létre egy Sails.js alkalmazást, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="73887-132">Create a Sails.js app and run it:</span></span>

        sails new <app_name>
        cd <app_name>
        sails lift

    <span data-ttu-id="73887-133">Ellenőrizze, hogy toohello alapértelmezett kezdőlapja a http://localhost:1377 léphet.</span><span class="sxs-lookup"><span data-stu-id="73887-133">Make sure you can navigate toohello default home page at http://localhost:1377.</span></span>

1. <span data-ttu-id="73887-134">Következő lépésként engedélyezze az Azure-naplózás.</span><span class="sxs-lookup"><span data-stu-id="73887-134">Next, enable logging for Azure.</span></span> <span data-ttu-id="73887-135">A gyökérkönyvtár, hozzon létre egy nevű fájlt `iisnode.yml` , és adja hozzá az alábbi két hello:</span><span class="sxs-lookup"><span data-stu-id="73887-135">In your root directory, create a file called `iisnode.yml` and add hello following two lines:</span></span>

        loggingEnabled: true
        logDirectory: iisnode

    <span data-ttu-id="73887-136">Naplózás engedélyezve van a hello [iisnode](https://github.com/tjanczuk/iisnode) , hogy az Azure App Service használja toorun Node.js alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="73887-136">Logging is now enabled for hello [iisnode](https://github.com/tjanczuk/iisnode) server that Azure App Service uses toorun Node.js apps.</span></span> 
    <span data-ttu-id="73887-137">Ennek működéséről további információkért lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="73887-137">For more information on how this works, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

2. <span data-ttu-id="73887-138">Ezután konfigurálja a hello Sails.js app toouse Azure környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="73887-138">Next, configure hello Sails.js app toouse Azure environment variables.</span></span> <span data-ttu-id="73887-139">Nyissa meg a config/env/production.js tooconfigure az éles környezetben, és állítsa be `port` és `hookTimeout`:</span><span class="sxs-lookup"><span data-stu-id="73887-139">Open config/env/production.js tooconfigure your production environment, and set `port` and `hookTimeout`:</span></span>

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    <span data-ttu-id="73887-140">Ezeket a konfigurációs beállításokat a dokumentációjában találja a [Sails.js dokumentáció](http://sailsjs.org/documentation/reference/configuration/sails-config).</span><span class="sxs-lookup"><span data-stu-id="73887-140">You can find documentation for these configuration settings in the  [Sails.js Documentation](http://sailsjs.org/documentation/reference/configuration/sails-config).</span></span>

4. <span data-ttu-id="73887-141">Ezt követően kódba foglalni hello Node.js verziót, amelybe toouse.</span><span class="sxs-lookup"><span data-stu-id="73887-141">Next, hardcode hello Node.js version you want toouse.</span></span> <span data-ttu-id="73887-142">A Package.JSON kódjában, adja hozzá a hello következő `engines` tulajdonság tooset hello Node.js verzió tooone, amely azt szeretnénk, ha.</span><span class="sxs-lookup"><span data-stu-id="73887-142">In package.json, add hello following `engines` property tooset hello Node.js version tooone that we want.</span></span>

        "engines": {
            "node": "6.9.1"
        },

5. <span data-ttu-id="73887-143">Végezetül a Git-tárház inicializálása, és véglegesítse a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="73887-143">Finally, initialize a Git repository and commit your files.</span></span> <span data-ttu-id="73887-144">Hello alkalmazás gyökérkönyvtárában (ahol a package.json azt), a következő Git-parancsok futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="73887-144">In hello application root (where package.json is), run hello following Git commands:</span></span>

        git init
        git add .
        git commit -m "<your commit message>"

<span data-ttu-id="73887-145">A kód készen toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="73887-145">Your code is ready toobe deployed.</span></span> 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a><span data-ttu-id="73887-146">2. lépés: Az Azure-alkalmazás létrehozása és központi telepítése Sails.js</span><span class="sxs-lookup"><span data-stu-id="73887-146">Step 2: Create an Azure app and deploy Sails.js</span></span>

<span data-ttu-id="73887-147">A következő hello App Service-erőforrás létrehozása az Azure-ban, és a Sails.js app tooit telepítése.</span><span class="sxs-lookup"><span data-stu-id="73887-147">Next, create hello App Service resource in Azure and deploy your Sails.js app tooit.</span></span>

1. <span data-ttu-id="73887-148">bejelentkezés tooAzure, például így:</span><span class="sxs-lookup"><span data-stu-id="73887-148">log in tooAzure like so:</span></span>

        az login

    <span data-ttu-id="73887-149">Hajtsa végre a hello Rákérdezés toocontinue hello bejelentkezési egy böngészőben az Azure-előfizetéssel rendelkező Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="73887-149">Follow hello prompt toocontinue hello login in a browser with a Microsoft account that has your Azure subscription.</span></span>

3. <span data-ttu-id="73887-150">App Service hello központi felhasználói be.</span><span class="sxs-lookup"><span data-stu-id="73887-150">Set hello deployment user for App Service.</span></span> <span data-ttu-id="73887-151">A kód üzembe helyezését később fogja elvégezni ezen hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="73887-151">You will deploy code using these credentials later.</span></span>
   
        az appservice web deployment user set --user-name <username> --password <password>

3. <span data-ttu-id="73887-152">Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) néven.</span><span class="sxs-lookup"><span data-stu-id="73887-152">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with a name.</span></span> <span data-ttu-id="73887-153">A jelen Node.js oktatóanyag esetében nem valóban szükséges tooknow mi.</span><span class="sxs-lookup"><span data-stu-id="73887-153">For this Node.js tutorial, you don't really need tooknow what it is.</span></span>

        az group create --location "<location>" --name my-sailsjs-app-group

    <span data-ttu-id="73887-154">toosee milyen lehetséges értékei, akkor is használhat `<location>`, használja a hello `az appservice list-locations` CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="73887-154">toosee what possible values you can use for `<location>`, use hello `az appservice list-locations` CLI command.</span></span>

3. <span data-ttu-id="73887-155">Hozzon létre egy "ingyenes" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) néven.</span><span class="sxs-lookup"><span data-stu-id="73887-155">Create a "FREE" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) with a name.</span></span> <span data-ttu-id="73887-156">A jelen Node.js oktatóanyag esetében csak tudja, hogy azt nem kell felszámítani a web Apps a terv.</span><span class="sxs-lookup"><span data-stu-id="73887-156">For this Node.js tutorial, just know that you won't be charged for web apps in this plan.</span></span>

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. <span data-ttu-id="73887-157">Hozzon létre egy új, egyéni névvel rendelkező webappot az `<app_name>` paraméterben.</span><span class="sxs-lookup"><span data-stu-id="73887-157">Create a new web app with a unique name in `<app_name>`.</span></span>

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a><span data-ttu-id="73887-158">3. lépés: Konfigurálja, és az Sails.js alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="73887-158">Step 3: Configure and deploy your Sails.js app</span></span>

1. <span data-ttu-id="73887-159">Az új webes alkalmazás a helyi Git-telepítés a következő parancs hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="73887-159">Configure local Git deployment for your new web app with hello following command:</span></span>

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="73887-160">A JSON-kimenetét ilyen, ami azt jelenti, hogy hello távoli Git-tárház beállítása jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="73887-160">You will get a JSON output like this, which means that hello remote Git repository is configured:</span></span>

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. <span data-ttu-id="73887-161">Hello URL-cím hozzáadása a hello JSON a helyi tárház távoli Git mappaként (nevű `azure` az egyszerűség érdekében).</span><span class="sxs-lookup"><span data-stu-id="73887-161">Add hello URL in hello JSON as a Git remote for your local repository (called `azure` for simplicity).</span></span>

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. <span data-ttu-id="73887-162">A minta kód toohello telepítése `azure` Git távoli.</span><span class="sxs-lookup"><span data-stu-id="73887-162">Deploy your sample code toohello `azure` Git remote.</span></span> <span data-ttu-id="73887-163">Amikor a rendszer kéri, használja a korábban konfigurált hello üzembe helyezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="73887-163">When prompted, use hello deployment credentials you configured earlier.</span></span>

        git push azure master

7. <span data-ttu-id="73887-164">Végezetül csak indítsa el az élő Azure alkalmazást hello böngészőben:</span><span class="sxs-lookup"><span data-stu-id="73887-164">Finally, just launch your live Azure app in hello browser:</span></span>

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="73887-165">Most látnia kell hello azonos Sails.js kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="73887-165">You should now see hello same Sails.js home page.</span></span>

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a><span data-ttu-id="73887-166">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="73887-166">Troubleshoot your deployment</span></span>
<span data-ttu-id="73887-167">A Sails.js alkalmazás az App Service valamilyen okból nem sikerül, ha található hello stderr naplók toohelp hibaelhárítás érdekében.</span><span class="sxs-lookup"><span data-stu-id="73887-167">If your Sails.js application fails for some reason in App Service, find hello stderr logs toohelp troubleshoot it.</span></span>
<span data-ttu-id="73887-168">További információkért lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="73887-168">For more information, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>
<span data-ttu-id="73887-169">Ha hello app sikeresen elindult, hello stdout napló meg kell jelennie, ismerős üdvözlőüzenetére:</span><span class="sxs-lookup"><span data-stu-id="73887-169">If hello app has started successfully, hello stdout log should show you hello familiar message:</span></span>

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

<span data-ttu-id="73887-170">Megadhatja a lépésköz legyen hello stdout-naplók hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) fájlt.</span><span class="sxs-lookup"><span data-stu-id="73887-170">You can control granularity of hello stdout logs in hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) file.</span></span>

## <a name="connect-tooa-database-in-azure"></a><span data-ttu-id="73887-171">Csatlakozás az Azure-ban tooa adatbázis</span><span class="sxs-lookup"><span data-stu-id="73887-171">Connect tooa database in Azure</span></span>
<span data-ttu-id="73887-172">tooconnect tooa adatbázis az Azure az Ön által választott hello-adatbázis létrehozása az Azure, például az Azure SQL Database, a MySQL, a MongoDB, a (Redis) Azure Cache, stb., és használja a megfelelő hello [datastore adapter](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="73887-172">tooconnect tooa database in Azure, you create hello database of your choice in Azure, such as Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, etc., and use hello corresponding [datastore adapter](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span></span> <span data-ttu-id="73887-173">hello lépésekből ebben a szakaszban megtudhatja, hogyan tooconnect tooMongoDB használatával egy [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) adatbázis, amely támogatja a MongoDB-ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="73887-173">hello steps in this section show you how tooconnect tooMongoDB by using an [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) database, which can support MongoDB client connections.</span></span>

1. <span data-ttu-id="73887-174">[Hozzon létre egy Cosmos-DB-fiók MongoDB-protokolltámogatással rendelkező](../documentdb/documentdb-create-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="73887-174">[Create a Cosmos DB account with MongoDB protocol support](../documentdb/documentdb-create-mongodb-account.md).</span></span>
2. <span data-ttu-id="73887-175">[Hozzon létre egy Cosmos DB gyűjtemény és az adatbázis](../documentdb/documentdb-create-collection.md).</span><span class="sxs-lookup"><span data-stu-id="73887-175">[Create a Cosmos DB collection and database](../documentdb/documentdb-create-collection.md).</span></span> <span data-ttu-id="73887-176">hello gyűjtemény hello nevét nem számít, de ha Sails.js kell hello hello adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="73887-176">hello name of hello collection doesn't matter, but you need hello name of hello database when you connect from Sails.js.</span></span>
3. <span data-ttu-id="73887-177">[A Cosmos-adatbázis adatbázis-kapcsolódási információt hello található](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="73887-177">[Find hello connection information for your Cosmos DB database](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span></span>
2. <span data-ttu-id="73887-178">A parancssori terminálról hello MongoDB-adapter telepítése:</span><span class="sxs-lookup"><span data-stu-id="73887-178">From your command-line terminal, install hello MongoDB adapter:</span></span>

        npm install sails-mongo --save

3. <span data-ttu-id="73887-179">Nyissa meg a config/connections.js, és adja hozzá a következő objektum toohello kapcsolatlista hello:</span><span class="sxs-lookup"><span data-stu-id="73887-179">Open config/connections.js and add hello following connection object toohello list:</span></span>

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > <span data-ttu-id="73887-180">Hello `ssl: true` beállítás fontos, mert [Cosmos DB írja elő](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="73887-180">hello `ssl: true` option is important because [Cosmos DB requires it](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 
    >
    >

4. <span data-ttu-id="73887-181">Környezeti változók (`process.env.*`), tooset van szüksége az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="73887-181">For each environment variable (`process.env.*`), you need tooset it in App Service.</span></span> <span data-ttu-id="73887-182">toodo a, futtatási hello parancsok követően a terminálon.</span><span class="sxs-lookup"><span data-stu-id="73887-182">toodo this, run hello following commands from your terminal.</span></span> <span data-ttu-id="73887-183">A Cosmos DB hello kapcsolati adatokat használjon.</span><span class="sxs-lookup"><span data-stu-id="73887-183">Use hello connection information for your Cosmos DB.</span></span>

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="73887-184">A beállítások üzembe az Azure alkalmazás beállításaiban tartja a bizalmas adatokat a verziókövetési rendszerrel (Git).</span><span class="sxs-lookup"><span data-stu-id="73887-184">Putting your settings in Azure app settings keeps sensitive data out of your source control (Git).</span></span> <span data-ttu-id="73887-185">Ezután konfigurálja a fejlesztési környezet toouse hello kapcsolat ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="73887-185">Next, you will configure your development environment toouse hello same connection information.</span></span>
5. <span data-ttu-id="73887-186">Nyissa meg a config/local.js, és adja hozzá a következő kapcsolatok objektum hello:</span><span class="sxs-lookup"><span data-stu-id="73887-186">Open config/local.js and add hello following connections object:</span></span>

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    <span data-ttu-id="73887-187">Ez a konfiguráció felülbírálja a config/connections.js fájl hello helyi környezet hello beállításai.</span><span class="sxs-lookup"><span data-stu-id="73887-187">This configuration overrides hello settings in your config/connections.js file for hello local environment.</span></span> <span data-ttu-id="73887-188">Így nem kerül a Git hello alapértelmezett .gitignore a projekt ki van zárva a be ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="73887-188">This file is excluded by hello default .gitignore in your project, so it will not be stored in Git.</span></span> <span data-ttu-id="73887-189">Most képes tooconnect tooyour Cosmos DB (MongoDB) adatbázis, mind az Azure web app és a helyi fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="73887-189">Now, you are able tooconnect tooyour Cosmos DB (MongoDB) database both from your Azure web app and from your local development environment.</span></span>
6. <span data-ttu-id="73887-190">Nyissa meg a config/env/production.js tooconfigure az éles környezetben, és adja hozzá a következő hello `models` objektum:</span><span class="sxs-lookup"><span data-stu-id="73887-190">Open config/env/production.js tooconfigure your production environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. <span data-ttu-id="73887-191">Nyissa meg a fejlesztési környezetet az config/env/development.js tooconfigure, és adja hozzá a következő hello `models` objektum:</span><span class="sxs-lookup"><span data-stu-id="73887-191">Open config/env/development.js tooconfigure your development environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    <span data-ttu-id="73887-192">`migrate: 'alter'`lehetővé teszi használja adatbázis áttelepítési funkciók toocreate és adatbázis-gyűjteményeket, illetve olyan táblázatok könnyen.</span><span class="sxs-lookup"><span data-stu-id="73887-192">`migrate: 'alter'` lets you use database migration features toocreate and update database collections or tables easily.</span></span> <span data-ttu-id="73887-193">Azonban `migrate: 'safe'` használatos az Azure (éles) környezetben, mert Sails.js nem teszi lehetővé a toouse `migrate: 'alter'` éles környezetben (lásd: [Sails.js dokumentáció](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span><span class="sxs-lookup"><span data-stu-id="73887-193">However, `migrate: 'safe'` is used for your Azure (production) environment because Sails.js does not allow you toouse `migrate: 'alter'` in a production environment (see [Sails.js Documentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span></span>
8. <span data-ttu-id="73887-194">A Terminálszolgáltatások, hello [készítése](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) egy Sails.js [tervezetének API](http://sailsjs.org/documentation/concepts/blueprints) például általában, majd futtassa `sails lift` Sails.js adatbázis áttelepítési hello adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="73887-194">From hello terminal, [generate](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) a Sails.js [blueprint API](http://sailsjs.org/documentation/concepts/blueprints) like you normally would, then run `sails lift` to create hello database with Sails.js database migration.</span></span> <span data-ttu-id="73887-195">Példa:</span><span class="sxs-lookup"><span data-stu-id="73887-195">For example:</span></span>

         sails generate api mywidget
         sails lift

    <span data-ttu-id="73887-196">Hello `mywidget` Ez a parancs által létrehozott modell üres, de használhatjuk tooshow, hogy rendelkezik-e az adatbázis-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="73887-196">hello `mywidget` model generated by this command is empty, but we can use it tooshow that we have database connectivity.</span></span>
    <span data-ttu-id="73887-197">Amikor futtatja `sails lift`, hello hiányzik gyűjteményt hoz létre, és hello táblák modellek az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="73887-197">When you run `sails lift`, it creates hello missing collections and tables for hello models your app uses.</span></span>
9. <span data-ttu-id="73887-198">Hozzáférés a most létrehozott hello böngészőben hello tervezetének API.</span><span class="sxs-lookup"><span data-stu-id="73887-198">Access hello blueprint API you just created in hello browser.</span></span> <span data-ttu-id="73887-199">Példa:</span><span class="sxs-lookup"><span data-stu-id="73887-199">For example:</span></span>

        http://localhost:1337/mywidget/create

    <span data-ttu-id="73887-200">hello API létrehozott hello bejegyzés hátsó tooyou hello böngészőablakban, ami azt jelenti, hogy a gyűjtemény sikeresen létrejött-e vissza.</span><span class="sxs-lookup"><span data-stu-id="73887-200">hello API should return hello created entry back tooyou in hello browser window, which means that your collection is created  successfully.</span></span>

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. <span data-ttu-id="73887-201">Most a módosítások tooAzure leküldéses, és keresse meg a tooyour app toomake meg arról, hogy továbbra is működik.</span><span class="sxs-lookup"><span data-stu-id="73887-201">Now, push your changes tooAzure, and browse tooyour app toomake sure it still works.</span></span>

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. <span data-ttu-id="73887-202">Hozzáférés hello tervezetének API Azure webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="73887-202">Access hello blueprint API of your Azure web app.</span></span> <span data-ttu-id="73887-203">Példa:</span><span class="sxs-lookup"><span data-stu-id="73887-203">For example:</span></span>

         http://<appname>.azurewebsites.net/mywidget/create

     <span data-ttu-id="73887-204">Ha hello API adja vissza egy másik új bejegyzést, majd az Azure-webalkalmazásban van van szó tooyour Cosmos DB (MongoDB) adatbázis.</span><span class="sxs-lookup"><span data-stu-id="73887-204">If hello API returns another new entry, then your Azure web app is talking tooyour Cosmos DB (MongoDB) database.</span></span>

## <a name="more-resources"></a><span data-ttu-id="73887-205">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="73887-205">More resources</span></span>
* [<span data-ttu-id="73887-206">Ismerkedés a Node.js webalkalmazásokkal az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="73887-206">Get started with Node.js web apps in Azure App Service</span></span>](app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="73887-207">A Node.js modulok használata az Azure alkalmazásokkal</span><span class="sxs-lookup"><span data-stu-id="73887-207">Using Node.js Modules with Azure applications</span></span>](../nodejs-use-node-modules-azure-apps.md)
