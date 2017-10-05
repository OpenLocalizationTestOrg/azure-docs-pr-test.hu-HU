---
title: "Python-webalkalmazás létrehozása az Azure-ban | Microsoft Docs"
description: "Percek alatt üzembe helyezheti első Hello World Python-alkalmazását az Azure App Service Web Apps szolgáltatásban."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 119f9770097c010cc360e0e204d06a307a268814
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="85990-103">Python-webapp létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="85990-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="85990-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="85990-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="85990-105">Ez a gyorsútmutató a Python-alkalmazások Azure Web Apps szolgáltatásban történő fejlesztésén és üzembe helyezésén vezeti végig.</span><span class="sxs-lookup"><span data-stu-id="85990-105">This quickstart walks through how to develop and deploy a Python app to Azure Web Apps.</span></span> <span data-ttu-id="85990-106">Az [Azure CLI-vel](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) létrehozhatja a webalkalmazást, a Gittel pedig üzembe helyezheti a Python-mintakódot a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="85990-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Python code to the web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="85990-108">Az alábbi lépéseket Mac, Windows vagy Linux rendszert futtató gépen is követheti.</span><span class="sxs-lookup"><span data-stu-id="85990-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="85990-109">Az előfeltételek telepítése után a lépések végrehajtása nagyjából öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="85990-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="85990-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="85990-110">Prerequisites</span></span>

<span data-ttu-id="85990-111">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="85990-111">To complete this tutorial:</span></span>

1. [<span data-ttu-id="85990-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="85990-112">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="85990-113">Telepítse a Pythont</span><span class="sxs-lookup"><span data-stu-id="85990-113">Install Python</span></span>](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="85990-114">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="85990-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="85990-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="85990-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="85990-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="85990-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="85990-117">A minta letöltése</span><span class="sxs-lookup"><span data-stu-id="85990-117">Download the sample</span></span>

<span data-ttu-id="85990-118">Egy terminálablakban futtassa a következő parancsot a mintaalkalmazás-tárház helyi számítógépre történő klónozásához.</span><span class="sxs-lookup"><span data-stu-id="85990-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="85990-119">Ezt a terminálablakot használhatja az összes parancs gyorsútmutatóban történő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="85990-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="85990-120">Váltson arra a könyvtárra, amelyben a mintakód megtalálható.</span><span class="sxs-lookup"><span data-stu-id="85990-120">Change to the directory that contains the sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="85990-121">Az alkalmazás futtatása helyben</span><span class="sxs-lookup"><span data-stu-id="85990-121">Run the app locally</span></span>

<span data-ttu-id="85990-122">Telepítse a szükséges csomagokat a(z) `pip` használatával.</span><span class="sxs-lookup"><span data-stu-id="85990-122">Install the required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="85990-123">Az alkalmazás a terminálablak megnyitásával és a `Python` parancs használatával helyben futtatható a beépített Python-webkiszolgáló indításához.</span><span class="sxs-lookup"><span data-stu-id="85990-123">Run the application locally by opening a terminal window and using the `Python` command to launch the built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="85990-124">Nyisson meg egy webböngészőt, majd keresse meg a mintaalkalmazást a http://localhost:5000 címen.</span><span class="sxs-lookup"><span data-stu-id="85990-124">Open a web browser, and navigate to the sample app at http://localhost:5000.</span></span>

<span data-ttu-id="85990-125">Az oldalon látható mintaalkalmazáson ekkor a **Hello World** üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="85990-125">You can see the **Hello World** message from the sample app displayed in the page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="85990-127">A terminálablakban nyomja le a **Ctrl+C** billentyűkombinációt a webkiszolgálóból történő kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="85990-127">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="85990-129">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="85990-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-to-use-python"></a><span data-ttu-id="85990-130">A Python használatának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85990-130">Configure to use Python</span></span>

<span data-ttu-id="85990-131">A Python `3.4` verziójának használatához futtassa az [az webapp config set](/cli/azure/webapp/config#set) parancsot a webalkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="85990-131">Use the [az webapp config set](/cli/azure/webapp/config#set) command to configure the web app to use Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="85990-132">A Python-verzió ezen konfigurációja a platform által biztosított alapértelmezett tárolót használja.</span><span class="sxs-lookup"><span data-stu-id="85990-132">Setting the Python version this way uses a default container provided by the platform.</span></span> <span data-ttu-id="85990-133">Ha saját tárolót szeretne használni, tekintse meg az [az webapp config container set](/cli/azure/webapp/config/container#set) parancs CLI-referenciáját.</span><span class="sxs-lookup"><span data-stu-id="85990-133">To use your own container, see the CLI reference for the [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="85990-134">Az alkalmazás megkeresése tallózással</span><span class="sxs-lookup"><span data-stu-id="85990-134">Browse to the app</span></span>

<span data-ttu-id="85990-135">Tallózással keresse meg az üzembe helyezett alkalmazást a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="85990-135">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="85990-136">A Python-mintakód az Azure App Service-webalkalmazásban fut.</span><span class="sxs-lookup"><span data-stu-id="85990-136">The Python sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="85990-138">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="85990-138">**Congratulations!**</span></span> <span data-ttu-id="85990-139">Elvégezte az első Python-webapp üzembe helyezését az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="85990-139">You've deployed your first Python app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="85990-140">A kód frissítése és ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="85990-140">Update and redeploy the code</span></span>

<span data-ttu-id="85990-141">Egy helyi szövegszerkesztővel nyissa meg a `main.py` fájlt a Python-alkalmazásban, majd módosítsa kissé annak szövegét a `return` utasítás mellett:</span><span class="sxs-lookup"><span data-stu-id="85990-141">Using a local text editor, open the `main.py` file in the Python app, and make a small change to the text next to the `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="85990-142">Mentse a módosításokat a Gitben, majd továbbítsa a kód módosításait az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="85990-142">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="85990-143">Az üzembe helyezés befejezését követően váltson vissza [Az alkalmazás megkeresése tallózással](#browse-to-the-app) lépésben megnyitott böngészőablakra, és frissítse az oldalt.</span><span class="sxs-lookup"><span data-stu-id="85990-143">Once deployment has completed, switch back to the browser window that opened in the [Browse to the app](#browse-to-the-app) step, and refresh the page.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="85990-145">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="85990-145">Manage your new Azure web app</span></span>

<span data-ttu-id="85990-146">A létrehozott webalkalmazás felügyeletéhez ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>.</span><span class="sxs-lookup"><span data-stu-id="85990-146">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="85990-147">A bal oldali menüben kattintson az **App Services** lehetőségre, majd az Azure-webalkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="85990-147">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="85990-149">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="85990-149">You see your web app's Overview page.</span></span> <span data-ttu-id="85990-150">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="85990-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="85990-152">A bal oldali menü az alkalmazás konfigurálásához biztosít különböző oldalakat.</span><span class="sxs-lookup"><span data-stu-id="85990-152">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="85990-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85990-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="85990-154">Python és PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="85990-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
