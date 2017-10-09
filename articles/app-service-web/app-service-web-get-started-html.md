---
title: "a statikus HTML aaaCreate webalkalmazás az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun webalkalmazásokat az Azure App Service egy statikus HTML üzembe helyezésével mintaalkalmazás."
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
ms.openlocfilehash: efd8c8189a3aa1ac35602b688eeb31bff6f5a373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a><span data-ttu-id="51766-103">Statikus HTML-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="51766-103">Create a static HTML web app in Azure</span></span>

<span data-ttu-id="51766-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="51766-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="51766-105">A gyors üzembe helyezés bemutatja, hogyan toodeploy egy egyszerű HTML + CSS hely tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="51766-105">This quickstart shows how toodeploy a basic HTML+CSS site tooAzure Web Apps.</span></span> <span data-ttu-id="51766-106">Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta HTML tartalom toohello webes alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="51766-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample HTML content toohello web app.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="51766-108">A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.</span><span class="sxs-lookup"><span data-stu-id="51766-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="51766-109">Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="51766-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51766-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="51766-110">Prerequisites</span></span>

<span data-ttu-id="51766-111">toocomplete a gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="51766-111">toocomplete this quickstart:</span></span>

- <span data-ttu-id="51766-112">[A Git telepítése](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span><span class="sxs-lookup"><span data-stu-id="51766-112">[Install Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="51766-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="51766-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="51766-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="51766-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="51766-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="51766-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="51766-116">Hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="51766-116">Download hello sample</span></span>

<span data-ttu-id="51766-117">Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="51766-117">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

<span data-ttu-id="51766-118">Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="51766-118">You use this terminal window toorun all hello commands in this quickstart.</span></span>

## <a name="view-hello-html"></a><span data-ttu-id="51766-119">Hello HTML megtekintése</span><span class="sxs-lookup"><span data-stu-id="51766-119">View hello HTML</span></span>

<span data-ttu-id="51766-120">Keresse meg a HTML hello mintát tartalmazó toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="51766-120">Navigate toohello directory that contains hello sample HTML.</span></span> <span data-ttu-id="51766-121">Nyissa meg hello *index.html* fájl a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="51766-121">Open hello *index.html* file in your browser.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-html/app-service-web-service-created.png)

<span data-ttu-id="51766-124">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="51766-124">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up too4 threads.
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
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="51766-125">Keresse meg a toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="51766-125">Browse toohello app</span></span>

<span data-ttu-id="51766-126">Nyissa meg böngészőben, toohello Azure webes alkalmazás URL-címe:</span><span class="sxs-lookup"><span data-stu-id="51766-126">In a browser, go toohello Azure web app URL:</span></span>

```
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="51766-127">az Azure App Service webalkalmazás futtatásához használt hello lap.</span><span class="sxs-lookup"><span data-stu-id="51766-127">hello page is running as an Azure App Service web app.</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="51766-129">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="51766-129">**Congratulations!**</span></span> <span data-ttu-id="51766-130">Az első HTML-alkalmazás tooApp szolgáltatás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="51766-130">You've deployed your first HTML app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-app"></a><span data-ttu-id="51766-131">Frissítse és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="51766-131">Update and redeploy hello app</span></span>

<span data-ttu-id="51766-132">Nyissa meg hello *index.html* fájlt egy szövegszerkesztőben, és tegye a módosítás toohello jelölés során.</span><span class="sxs-lookup"><span data-stu-id="51766-132">Open hello *index.html* file in a text editor, and make a change toohello markup.</span></span> <span data-ttu-id="51766-133">Hello H1 címsor módosítsa az "Azure App Service – minta statikus HTML-webhely" toojust például "az Azure App Service".</span><span class="sxs-lookup"><span data-stu-id="51766-133">For example, change hello H1 heading from "Azure App Service - Sample Static HTML Site" toojust "Azure App Service\`.</span></span>

<span data-ttu-id="51766-134">A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="51766-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated HTML"
git push azure master
```

<span data-ttu-id="51766-135">Központi telepítés befejezése után frissítse a böngésző toosee hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="51766-135">Once deployment has completed, refresh your browser toosee hello changes.</span></span>

![Frissített mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="51766-137">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="51766-137">Manage your new Azure web app</span></span>

<span data-ttu-id="51766-138">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51766-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="51766-139">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="51766-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-html/portal1.png)

<span data-ttu-id="51766-141">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="51766-141">You see your web app's Overview page.</span></span> <span data-ttu-id="51766-142">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="51766-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-html/portal2.png)

<span data-ttu-id="51766-144">hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="51766-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="51766-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51766-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="51766-146">Egyéni tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="51766-146">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
