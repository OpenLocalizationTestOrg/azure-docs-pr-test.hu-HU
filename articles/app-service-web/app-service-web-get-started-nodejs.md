---
title: "Node.js-webalkalmazás létrehozása az Azure-ban | Microsoft Docs"
description: "Percek alatt üzembe helyezheti első Hello World Node.js-alkalmazását az App Service Web Apps szolgáltatásban."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ce845da09a7c088b8a2ba29b818a46a3b41aa4e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="058e9-103">Node.js-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="058e9-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="058e9-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="058e9-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="058e9-105">Ez a gyorsútmutató a Node.js-alkalmazások Azure Web Apps szolgáltatásban történő üzembe helyezésén vezeti végig.</span><span class="sxs-lookup"><span data-stu-id="058e9-105">This quickstart shows how to deploy a Node.js app to Azure Web Apps.</span></span> <span data-ttu-id="058e9-106">Az [Azure CLI-vel](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) létrehozhatja a webalkalmazást, a Git szoftver használatával pedig üzembe helyezheti a Node.js-mintakódot a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="058e9-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Node.js code to the web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="058e9-108">Az alábbi lépéseket Mac, Windows vagy Linux rendszert futtató gépen is követheti.</span><span class="sxs-lookup"><span data-stu-id="058e9-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="058e9-109">Az előfeltételek telepítése után a lépések végrehajtása nagyjából öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="058e9-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="058e9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="058e9-110">Prerequisites</span></span>

<span data-ttu-id="058e9-111">A gyorsútmutató elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="058e9-111">To complete this quickstart:</span></span>

* [<span data-ttu-id="058e9-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="058e9-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="058e9-113">Telepítse a Node.js-t és az NPM-et</span><span class="sxs-lookup"><span data-stu-id="058e9-113">Install Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="058e9-114">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="058e9-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="058e9-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="058e9-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="058e9-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="058e9-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="058e9-117">A minta letöltése</span><span class="sxs-lookup"><span data-stu-id="058e9-117">Download the sample</span></span>

<span data-ttu-id="058e9-118">Egy terminálablakban futtassa a következő parancsot a mintaalkalmazás-tárház helyi számítógépre történő klónozásához.</span><span class="sxs-lookup"><span data-stu-id="058e9-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="058e9-119">Ezt a terminálablakot használhatja az összes parancs gyorsútmutatóban történő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="058e9-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="058e9-120">Váltson arra a könyvtárra, amelyben a mintakód megtalálható.</span><span class="sxs-lookup"><span data-stu-id="058e9-120">Change to the directory that contains the sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="058e9-121">Az alkalmazás futtatása helyben</span><span class="sxs-lookup"><span data-stu-id="058e9-121">Run the app locally</span></span>

<span data-ttu-id="058e9-122">Az alkalmazás a terminálablak megnyitásával és a `npm start` szkript használatával helyben futtatható a beépített Node.js HTTP-kiszolgáló indításához.</span><span class="sxs-lookup"><span data-stu-id="058e9-122">Run the application locally by opening a terminal window and using the `npm start` script to launch the built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="058e9-123">Nyisson meg egy webböngészőt, majd keresse meg a mintaalkalmazást a http://localhost:1337 címen.</span><span class="sxs-lookup"><span data-stu-id="058e9-123">Open a web browser, and navigate to the sample app at http://localhost:1337.</span></span>

<span data-ttu-id="058e9-124">Az oldalon látható mintaalkalmazáson ekkor a **Hello World** üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="058e9-124">You see the **Hello World** message from the sample app displayed in the page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="058e9-126">A terminálablakban nyomja le a **Ctrl+C** billentyűkombinációt a webkiszolgálóból történő kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="058e9-126">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="058e9-128">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="058e9-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="058e9-129">Az alkalmazás megkeresése tallózással</span><span class="sxs-lookup"><span data-stu-id="058e9-129">Browse to the app</span></span>

<span data-ttu-id="058e9-130">Tallózással keresse meg az üzembe helyezett alkalmazást a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="058e9-130">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="058e9-131">A Node.js mintakód az Azure App Service webalkalmazásban fut.</span><span class="sxs-lookup"><span data-stu-id="058e9-131">The Node.js sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="058e9-133">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="058e9-133">**Congratulations!**</span></span> <span data-ttu-id="058e9-134">Elvégezte az első Node.js-app üzembe helyezését az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="058e9-134">You've deployed your first Node.js app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="058e9-135">A kód frissítése és ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="058e9-135">Update and redeploy the code</span></span>

<span data-ttu-id="058e9-136">Egy szövegszerkesztő használatával nyissa meg a Node.js-alkalmazáson belüli `index.js` fájlt, majd módosítsa annak szövegét a `response.end` hívásán belül:</span><span class="sxs-lookup"><span data-stu-id="058e9-136">Using a text editor, open the `index.js` file in the Node.js app, and make a small change to the text in the call to `response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="058e9-137">Mentse a módosításokat a Gitben, majd továbbítsa a kód módosításait az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="058e9-137">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="058e9-138">Az üzembe helyezés befejezését követően váltson vissza **Az alkalmazás megkeresése tallózással** lépésben megnyitott böngészőablakra, és frissítse azt.</span><span class="sxs-lookup"><span data-stu-id="058e9-138">Once deployment has completed, switch back to the browser window that opened in the **Browse to the app** step, and hit refresh.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="058e9-140">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="058e9-140">Manage your new Azure web app</span></span>

<span data-ttu-id="058e9-141">A létrehozott webalkalmazás felügyeletéhez ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>.</span><span class="sxs-lookup"><span data-stu-id="058e9-141">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="058e9-142">A bal oldali menüben kattintson az **App Services** lehetőségre, majd az Azure-webalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="058e9-142">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="058e9-144">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="058e9-144">You see your web app's Overview page.</span></span> <span data-ttu-id="058e9-145">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="058e9-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="058e9-147">A bal oldali menü az alkalmazás konfigurálásához biztosít különböző oldalakat.</span><span class="sxs-lookup"><span data-stu-id="058e9-147">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="058e9-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="058e9-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="058e9-149">Node.js és MongoDB</span><span class="sxs-lookup"><span data-stu-id="058e9-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
