---
title: "Node.js-webalkalmazás az Azure-ban aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 163edf83b2353755fc9fa2d75aed489038cf7c81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="7207d-103">Node.js-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="7207d-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="7207d-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7207d-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="7207d-105">A gyors üzembe helyezés bemutatja, hogyan toodeploy egy Node.js-alkalmazás tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="7207d-105">This quickstart shows how toodeploy a Node.js app tooAzure Web Apps.</span></span> <span data-ttu-id="7207d-106">Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta Node.js kód toohello webes alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="7207d-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Node.js code toohello web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="7207d-108">A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.</span><span class="sxs-lookup"><span data-stu-id="7207d-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="7207d-109">Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7207d-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="7207d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7207d-110">Prerequisites</span></span>

<span data-ttu-id="7207d-111">toocomplete a gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="7207d-111">toocomplete this quickstart:</span></span>

* [<span data-ttu-id="7207d-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="7207d-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="7207d-113">Telepítse a Node.js-t és az NPM-et</span><span class="sxs-lookup"><span data-stu-id="7207d-113">Install Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7207d-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7207d-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7207d-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="7207d-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="7207d-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7207d-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="7207d-117">Hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="7207d-117">Download hello sample</span></span>

<span data-ttu-id="7207d-118">Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="7207d-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="7207d-119">Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="7207d-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="7207d-120">Módosítsa a hello mintakódot tartalmazó toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7207d-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="7207d-121">Hello alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="7207d-121">Run hello app locally</span></span>

<span data-ttu-id="7207d-122">Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `npm start` parancsfájl toolaunch hello Node.js HTTP-kiszolgáló a beépített.</span><span class="sxs-lookup"><span data-stu-id="7207d-122">Run hello application locally by opening a terminal window and using hello `npm start` script toolaunch hello built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="7207d-123">Nyisson meg egy webböngészőt, és keresse meg a http://localhost:1337 toohello minta alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7207d-123">Open a web browser, and navigate toohello sample app at http://localhost:1337.</span></span>

<span data-ttu-id="7207d-124">Megjelenik a hello **Hello World** üzenetet kapott hello mintaalkalmazás hello oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7207d-124">You see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="7207d-126">A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="7207d-126">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="7207d-128">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7207d-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up too4 threads.
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
remote: Node.js versions available on hello platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file toochoose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="7207d-129">Keresse meg a toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7207d-129">Browse toohello app</span></span>

<span data-ttu-id="7207d-130">Keresse meg a webböngésző segítségével toohello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7207d-130">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="7207d-131">Node.js mintakód hello fut. Ha az Azure App Service web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7207d-131">hello Node.js sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="7207d-133">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="7207d-133">**Congratulations!**</span></span> <span data-ttu-id="7207d-134">Az első Node.js-alkalmazás tooApp szolgáltatás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="7207d-134">You've deployed your first Node.js app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="7207d-135">Frissítse, és telepítse újra a hello kódot</span><span class="sxs-lookup"><span data-stu-id="7207d-135">Update and redeploy hello code</span></span>

<span data-ttu-id="7207d-136">Egy szövegszerkesztőben nyissa meg hello `index.js` hello Node.js-alkalmazás, és győződjön meg szöveg egy kis módosítása toohello hello hívásában túl`response.end`:</span><span class="sxs-lookup"><span data-stu-id="7207d-136">Using a text editor, open hello `index.js` file in hello Node.js app, and make a small change toohello text in hello call too`response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="7207d-137">A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7207d-137">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="7207d-138">Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban **Tallózás toohello app** . lépés:, majd nyomja le a frissítést.</span><span class="sxs-lookup"><span data-stu-id="7207d-138">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and hit refresh.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="7207d-140">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="7207d-140">Manage your new Azure web app</span></span>

<span data-ttu-id="7207d-141">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7207d-141">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="7207d-142">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="7207d-142">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="7207d-144">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="7207d-144">You see your web app's Overview page.</span></span> <span data-ttu-id="7207d-145">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="7207d-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="7207d-147">hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="7207d-147">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="7207d-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7207d-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7207d-149">Node.js és MongoDB</span><span class="sxs-lookup"><span data-stu-id="7207d-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
