---
title: "Statikus HTML-webalkalmazás létrehozása az Azure-ban | Microsoft Docs"
description: "Egy statikus HTML-webalkalmazás üzembe helyezésével megtudhatja, hogy miként futtathat webalkalmazásokat az Azure App Service-ben."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/26/2017
ms.author: riande
ms.custom: mvc
ms.openlocfilehash: 42af5b08b8d2ff0c75fd73dcfa61c861647fd2c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a><span data-ttu-id="585dc-103">Statikus HTML-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="585dc-103">Create a static HTML web app in Azure</span></span>

<span data-ttu-id="585dc-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="585dc-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="585dc-105">Ez a gyorsútmutató egy alapszintű HTML+CSS hely Azure Web Apps szolgáltatásban történő üzembe helyezésén vezeti végig.</span><span class="sxs-lookup"><span data-stu-id="585dc-105">This quickstart shows how to deploy a basic HTML+CSS site to Azure Web Apps.</span></span> <span data-ttu-id="585dc-106">Az [Azure CLI-vel](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) létrehozhatja a webalkalmazást, a Git szoftver használatával pedig üzembe helyezheti a HTML mintatartalmat a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="585dc-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample HTML content to the web app.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="585dc-108">Az alábbi lépéseket Mac, Windows vagy Linux rendszert futtató gépen is követheti.</span><span class="sxs-lookup"><span data-stu-id="585dc-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="585dc-109">Az előfeltételek telepítése után a lépések végrehajtása nagyjából öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="585dc-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="585dc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="585dc-110">Prerequisites</span></span>

<span data-ttu-id="585dc-111">A gyorsútmutató elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="585dc-111">To complete this quickstart:</span></span>

- <span data-ttu-id="585dc-112">[A Git telepítése](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span><span class="sxs-lookup"><span data-stu-id="585dc-112">[Install Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="585dc-113">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="585dc-113">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="585dc-114">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="585dc-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="585dc-115">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="585dc-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="585dc-116">A minta letöltése</span><span class="sxs-lookup"><span data-stu-id="585dc-116">Download the sample</span></span>

<span data-ttu-id="585dc-117">Egy terminálablakban futtassa a következő parancsot a mintaalkalmazás-tárház helyi számítógépre történő klónozásához.</span><span class="sxs-lookup"><span data-stu-id="585dc-117">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

<span data-ttu-id="585dc-118">Ezt a terminálablakot használhatja az összes parancs gyorsútmutatóban történő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="585dc-118">You use this terminal window to run all the commands in this quickstart.</span></span>

## <a name="view-the-html"></a><span data-ttu-id="585dc-119">HTML megjelenítése</span><span class="sxs-lookup"><span data-stu-id="585dc-119">View the HTML</span></span>

<span data-ttu-id="585dc-120">Váltson a minta HTML-t tartalmazó könyvtárra.</span><span class="sxs-lookup"><span data-stu-id="585dc-120">Navigate to the directory that contains the sample HTML.</span></span> <span data-ttu-id="585dc-121">Nyissa meg a *index.html* fájlt a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="585dc-121">Open the *index.html* file in your browser.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-html/app-service-web-service-created.png)

<span data-ttu-id="585dc-124">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="585dc-124">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="585dc-125">Az alkalmazás megkeresése tallózással</span><span class="sxs-lookup"><span data-stu-id="585dc-125">Browse to the app</span></span>

<span data-ttu-id="585dc-126">Nyissa meg böngészőben az Azure webalkalmazás URL-címét:</span><span class="sxs-lookup"><span data-stu-id="585dc-126">In a browser, go to the Azure web app URL:</span></span>

```
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="585dc-127">Az oldal Azure App Service webalkalmazásként fut.</span><span class="sxs-lookup"><span data-stu-id="585dc-127">The page is running as an Azure App Service web app.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="585dc-129">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="585dc-129">**Congratulations!**</span></span> <span data-ttu-id="585dc-130">Elvégezte az első HTML-webapp üzembe helyezését az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="585dc-130">You've deployed your first HTML app to App Service.</span></span>

## <a name="update-and-redeploy-the-app"></a><span data-ttu-id="585dc-131">Az alkalmazás frissítése és ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="585dc-131">Update and redeploy the app</span></span>

<span data-ttu-id="585dc-132">Egy szövegszerkesztőben nyissa meg az *index.html* fájlt, és hajtson végre módosítást a jelölőnyelvi kódon.</span><span class="sxs-lookup"><span data-stu-id="585dc-132">Open the *index.html* file in a text editor, and make a change to the markup.</span></span> <span data-ttu-id="585dc-133">Például, módosítsa az „Azure App Service – statikus HTML-mintahely” nevű H1 fejlécet egyszerűen „Azure App Service”-re.</span><span class="sxs-lookup"><span data-stu-id="585dc-133">For example, change the H1 heading from "Azure App Service - Sample Static HTML Site" to just "Azure App Service\`.</span></span>

<span data-ttu-id="585dc-134">Mentse a módosításokat a Gitben, majd továbbítsa a kód módosításait az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="585dc-134">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated HTML"
git push azure master
```

<span data-ttu-id="585dc-135">A telepítés befejezése után frissítse a lapot a böngészőben, hogy lássa a változásokat.</span><span class="sxs-lookup"><span data-stu-id="585dc-135">Once deployment has completed, refresh your browser to see the changes.</span></span>

![Frissített mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="585dc-137">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="585dc-137">Manage your new Azure web app</span></span>

<span data-ttu-id="585dc-138">A létrehozott webalkalmazás felügyeletéhez ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>.</span><span class="sxs-lookup"><span data-stu-id="585dc-138">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="585dc-139">A bal oldali menüben kattintson az **App Services** lehetőségre, majd az Azure-webalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="585dc-139">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-get-started-html/portal1.png)

<span data-ttu-id="585dc-141">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="585dc-141">You see your web app's Overview page.</span></span> <span data-ttu-id="585dc-142">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="585dc-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-html/portal2.png)

<span data-ttu-id="585dc-144">A bal oldali menü az alkalmazás konfigurálásához biztosít különböző oldalakat.</span><span class="sxs-lookup"><span data-stu-id="585dc-144">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="585dc-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="585dc-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="585dc-146">Egyéni tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="585dc-146">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
