---
title: "a PHP aaaCreate webalkalmazás az Azure-ban |} Microsoft Docs"
description: "Percek alatt üzembe helyezheti első Hello World PHP-jét az App Service Web Apps szolgáltatásban."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/21/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8e1022889ca162f8f15ce7435cc9393cc6efef06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-web-app-in-azure"></a><span data-ttu-id="5fb19-103">PHP-webapp létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="5fb19-103">Create a PHP web app in Azure</span></span>

<span data-ttu-id="5fb19-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5fb19-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="5fb19-105">A gyors üzembe helyezési oktatóanyag bemutatja, hogyan toodeploy egy PHP-alkalmazás tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5fb19-105">This quickstart tutorial shows how toodeploy a PHP app tooAzure Web Apps.</span></span> <span data-ttu-id="5fb19-106">Létrehozhat hello web app használatával hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a felhő rendszerhéjat, és Git toodeploy minta PHP kód toohello webalkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="5fb19-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell, and you use Git toodeploy sample PHP code toohello web app.</span></span>

<span data-ttu-id="5fb19-107">![Az Azure-ban futó mintaalkalmazás]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span><span class="sxs-lookup"><span data-stu-id="5fb19-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span></span>

<span data-ttu-id="5fb19-108">A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.</span><span class="sxs-lookup"><span data-stu-id="5fb19-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="5fb19-109">Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5fb19-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fb19-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5fb19-110">Prerequisites</span></span>

<span data-ttu-id="5fb19-111">toocomplete a gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="5fb19-111">toocomplete this quickstart:</span></span>

* [<span data-ttu-id="5fb19-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="5fb19-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="5fb19-113">A PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="5fb19-113">Install PHP</span></span>](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a><span data-ttu-id="5fb19-114">Helyileg hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="5fb19-114">Download hello sample locally</span></span>

<span data-ttu-id="5fb19-115">Egy terminálablakot futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="5fb19-115">In a terminal window, run hello following commands.</span></span> <span data-ttu-id="5fb19-116">A hello minta alkalmazás tooyour helyi gép klónozását, és keresse meg a toohello directory hello minta kódot tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="5fb19-116">This will clone hello sample application tooyour local machine, and navigate toohello directory containing hello sample code.</span></span>

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="5fb19-117">Hello alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="5fb19-117">Run hello app locally</span></span>

<span data-ttu-id="5fb19-118">Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `php` parancs toolaunch hello beépített PHP webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5fb19-118">Run hello application locally by opening a terminal window and using hello `php` command toolaunch hello built-in PHP web server.</span></span>

```bash
php -S localhost:8080
```

<span data-ttu-id="5fb19-119">Nyisson meg egy webböngészőt, és keresse meg a mintaalkalmazás toohello: 8080.</span><span class="sxs-lookup"><span data-stu-id="5fb19-119">Open a web browser, and navigate toohello sample app at http://localhost:8080.</span></span>

<span data-ttu-id="5fb19-120">Megjelenik a hello **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="5fb19-120">You see hello **Hello World!**</span></span> <span data-ttu-id="5fb19-121">hello lapján megjelenő hello mintaalkalmazás üzenetét.</span><span class="sxs-lookup"><span data-stu-id="5fb19-121">message from hello sample app displayed in hello page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

<span data-ttu-id="5fb19-123">A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5fb19-123">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="5fb19-125">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5fb19-125">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up too4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-toohello-app-locally"></a><span data-ttu-id="5fb19-126">Keresse meg a helyi toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5fb19-126">Browse toohello app locally</span></span>

<span data-ttu-id="5fb19-127">Keresse meg a webböngésző segítségével toohello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5fb19-127">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="5fb19-128">hello PHP mintakód fut. Ha az Azure App Service web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5fb19-128">hello PHP sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-php/hello-world-in-browser.png)

<span data-ttu-id="5fb19-130">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="5fb19-130">**Congratulations!**</span></span> <span data-ttu-id="5fb19-131">Az első PHP-alkalmazás tooApp szolgáltatás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="5fb19-131">You've deployed your first PHP app tooApp Service.</span></span>

## <a name="update-locally-and-redeploy-hello-code"></a><span data-ttu-id="5fb19-132">Frissítés helyben, és telepítse újra a hello kódot</span><span class="sxs-lookup"><span data-stu-id="5fb19-132">Update locally and redeploy hello code</span></span>

<span data-ttu-id="5fb19-133">Egy helyi szövegszerkesztőben nyissa meg hello `index.php` hello PHP alkalmazáson belül, és győződjön szöveg egy kis módosítása toohello hello karakterláncon belüli következő túl`echo`:</span><span class="sxs-lookup"><span data-stu-id="5fb19-133">Using a local text editor, open hello `index.php` file within hello PHP app, and make a small change toohello text within hello string next too`echo`:</span></span>

```php
echo "Hello Azure!";
```

<span data-ttu-id="5fb19-134">A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5fb19-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="5fb19-135">Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban **Tallózás toohello app** lépést, és a frissítési hello lap.</span><span class="sxs-lookup"><span data-stu-id="5fb19-135">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and refresh hello page.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="5fb19-137">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="5fb19-137">Manage your new Azure web app</span></span>

<span data-ttu-id="5fb19-138">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5fb19-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="5fb19-139">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5fb19-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

<span data-ttu-id="5fb19-141">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="5fb19-141">You see your web app's Overview page.</span></span> <span data-ttu-id="5fb19-142">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="5fb19-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span>

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

<span data-ttu-id="5fb19-144">hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="5fb19-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="5fb19-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5fb19-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5fb19-146">PHP és MySQL</span><span class="sxs-lookup"><span data-stu-id="5fb19-146">PHP with MySQL</span></span>](app-service-web-tutorial-php-mysql.md)
