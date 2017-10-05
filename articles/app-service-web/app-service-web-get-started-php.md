---
title: "PHP-webalkalmazás létrehozása az Azure-ban | Microsoft Docs"
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
ms.openlocfilehash: 3a78e0b485046ad6228bf4819d3908042c298d1a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-php-web-app-in-azure"></a><span data-ttu-id="d0f59-103">PHP-webapp létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d0f59-103">Create a PHP web app in Azure</span></span>

<span data-ttu-id="d0f59-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d0f59-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="d0f59-105">Ez a gyorsútmutató a PHP-alkalmazás Azure Web Apps szolgáltatásban történő üzembe helyezésén vezeti végig.</span><span class="sxs-lookup"><span data-stu-id="d0f59-105">This quickstart tutorial shows how to deploy a PHP app to Azure Web Apps.</span></span> <span data-ttu-id="d0f59-106">A Cloud Shellben az [Azure CLI-vel](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) létrehozza a webalkalmazást, a Gittel pedig üzembe helyezi a PHP-mintakódot a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d0f59-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell, and you use Git to deploy sample PHP code to the web app.</span></span>

<span data-ttu-id="d0f59-107">![Az Azure-ban futó mintaalkalmazás]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span><span class="sxs-lookup"><span data-stu-id="d0f59-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span></span>

<span data-ttu-id="d0f59-108">Az alábbi lépéseket Mac, Windows vagy Linux rendszert futtató gépen is követheti.</span><span class="sxs-lookup"><span data-stu-id="d0f59-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="d0f59-109">Az előfeltételek telepítése után a lépések végrehajtása nagyjából öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d0f59-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0f59-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d0f59-110">Prerequisites</span></span>

<span data-ttu-id="d0f59-111">A gyorsútmutató elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="d0f59-111">To complete this quickstart:</span></span>

* [<span data-ttu-id="d0f59-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="d0f59-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="d0f59-113">A PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="d0f59-113">Install PHP</span></span>](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample-locally"></a><span data-ttu-id="d0f59-114">Minta helyi letöltése</span><span class="sxs-lookup"><span data-stu-id="d0f59-114">Download the sample locally</span></span>

<span data-ttu-id="d0f59-115">Futtassa a következő parancsokat egy terminálablakban.</span><span class="sxs-lookup"><span data-stu-id="d0f59-115">In a terminal window, run the following commands.</span></span> <span data-ttu-id="d0f59-116">Ezzel klónozza a mintaalkalmazást a helyi gépre, és a mintakódot tartalmazó könyvtárba lép.</span><span class="sxs-lookup"><span data-stu-id="d0f59-116">This will clone the sample application to your local machine, and navigate to the directory containing the sample code.</span></span>

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="d0f59-117">Az alkalmazás futtatása helyben</span><span class="sxs-lookup"><span data-stu-id="d0f59-117">Run the app locally</span></span>

<span data-ttu-id="d0f59-118">Az alkalmazás a terminálablak megnyitásával és a `php` parancs használatával helyben futtatható a beépített PHP-webkiszolgáló indításához.</span><span class="sxs-lookup"><span data-stu-id="d0f59-118">Run the application locally by opening a terminal window and using the `php` command to launch the built-in PHP web server.</span></span>

```bash
php -S localhost:8080
```

<span data-ttu-id="d0f59-119">Nyisson meg egy webböngészőt, majd keresse meg a mintaalkalmazást a http://localhost:8080. címen.</span><span class="sxs-lookup"><span data-stu-id="d0f59-119">Open a web browser, and navigate to the sample app at http://localhost:8080.</span></span>

<span data-ttu-id="d0f59-120">Az oldalon látható mintaalkalmazáson ekkor a **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="d0f59-120">You see the **Hello World!**</span></span> <span data-ttu-id="d0f59-121">üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d0f59-121">message from the sample app displayed in the page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

<span data-ttu-id="d0f59-123">A terminálablakban nyomja le a **Ctrl+C** billentyűkombinációt a webkiszolgálóból történő kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="d0f59-123">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="d0f59-125">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d0f59-125">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app-locally"></a><span data-ttu-id="d0f59-126">Alkalmazás megkeresése tallózással, helyileg</span><span class="sxs-lookup"><span data-stu-id="d0f59-126">Browse to the app locally</span></span>

<span data-ttu-id="d0f59-127">Tallózással keresse meg az üzembe helyezett alkalmazást a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="d0f59-127">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="d0f59-128">A PHP mintakód az Azure App Service webalkalmazásban fut.</span><span class="sxs-lookup"><span data-stu-id="d0f59-128">The PHP sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-php/hello-world-in-browser.png)

<span data-ttu-id="d0f59-130">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="d0f59-130">**Congratulations!**</span></span> <span data-ttu-id="d0f59-131">Elvégezte az első PHP-webapp üzembe helyezését az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="d0f59-131">You've deployed your first PHP app to App Service.</span></span>

## <a name="update-locally-and-redeploy-the-code"></a><span data-ttu-id="d0f59-132">A kód frissítése helyileg és ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d0f59-132">Update locally and redeploy the code</span></span>

<span data-ttu-id="d0f59-133">Egy helyi szövegszerkesztő használatával nyissa meg a `index.php` fájlt a PHP-alkalmazáson belül, majd módosítsa kissé annak szövegét a `echo` melletti karakterláncon belül:</span><span class="sxs-lookup"><span data-stu-id="d0f59-133">Using a local text editor, open the `index.php` file within the PHP app, and make a small change to the text within the string next to `echo`:</span></span>

```php
echo "Hello Azure!";
```

<span data-ttu-id="d0f59-134">Mentse a módosításokat a Gitben, majd továbbítsa a kód módosításait az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="d0f59-134">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="d0f59-135">Az üzembe helyezés befejezését követően váltson vissza **Az alkalmazás megkeresése tallózással** lépésben megnyitott böngészőablakra, és frissítse az oldalt.</span><span class="sxs-lookup"><span data-stu-id="d0f59-135">Once deployment has completed, switch back to the browser window that opened in the **Browse to the app** step, and refresh the page.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="d0f59-137">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="d0f59-137">Manage your new Azure web app</span></span>

<span data-ttu-id="d0f59-138">A létrehozott webalkalmazás felügyeletéhez ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>.</span><span class="sxs-lookup"><span data-stu-id="d0f59-138">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="d0f59-139">A bal oldali menüben kattintson az **App Services** lehetőségre, majd az Azure-webalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="d0f59-139">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

<span data-ttu-id="d0f59-141">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="d0f59-141">You see your web app's Overview page.</span></span> <span data-ttu-id="d0f59-142">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="d0f59-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span>

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

<span data-ttu-id="d0f59-144">A bal oldali menü az alkalmazás konfigurálásához biztosít különböző oldalakat.</span><span class="sxs-lookup"><span data-stu-id="d0f59-144">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="d0f59-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0f59-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d0f59-146">PHP és MySQL</span><span class="sxs-lookup"><span data-stu-id="d0f59-146">PHP with MySQL</span></span>](app-service-web-tutorial-php-mysql.md)
