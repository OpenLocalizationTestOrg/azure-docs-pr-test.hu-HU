---
title: "a Python webalkalmazás az Azure-ban aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 42178d490d8aa8eaf93710667aad598794c62c8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="88905-103">Python-webapp létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="88905-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="88905-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="88905-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="88905-105">A gyors üzembe helyezési útmutató végigvezeti toodevelop és a Python alkalmazást tooAzure webalkalmazások telepítését.</span><span class="sxs-lookup"><span data-stu-id="88905-105">This quickstart walks through how toodevelop and deploy a Python app tooAzure Web Apps.</span></span> <span data-ttu-id="88905-106">Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta Python kódját toohello webes alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="88905-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Python code toohello web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="88905-108">A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.</span><span class="sxs-lookup"><span data-stu-id="88905-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="88905-109">Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="88905-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="88905-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="88905-110">Prerequisites</span></span>

<span data-ttu-id="88905-111">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="88905-111">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="88905-112">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="88905-112">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="88905-113">Telepítse a Pythont</span><span class="sxs-lookup"><span data-stu-id="88905-113">Install Python</span></span>](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="88905-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="88905-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="88905-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="88905-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="88905-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="88905-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="88905-117">Hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="88905-117">Download hello sample</span></span>

<span data-ttu-id="88905-118">Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="88905-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="88905-119">Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="88905-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="88905-120">Módosítsa a hello mintakódot tartalmazó toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="88905-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="88905-121">Hello alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="88905-121">Run hello app locally</span></span>

<span data-ttu-id="88905-122">Szükséges hello csomagok használatával `pip`.</span><span class="sxs-lookup"><span data-stu-id="88905-122">Install hello required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="88905-123">Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `Python` parancs toolaunch hello beépített Python webalkalmazás-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="88905-123">Run hello application locally by opening a terminal window and using hello `Python` command toolaunch hello built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="88905-124">Nyisson meg egy webböngészőt, és keresse meg a http://localhost:5000 toohello minta alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="88905-124">Open a web browser, and navigate toohello sample app at http://localhost:5000.</span></span>

<span data-ttu-id="88905-125">Megtekintheti a hello **Hello World** üzenetet kapott hello mintaalkalmazás hello oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="88905-125">You can see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="88905-127">A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="88905-127">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="88905-129">Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="88905-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-toouse-python"></a><span data-ttu-id="88905-130">Toouse Python konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88905-130">Configure toouse Python</span></span>

<span data-ttu-id="88905-131">Használjon hello [az webapp konfiguráció](/cli/azure/webapp/config#set) parancs tooconfigure hello webes alkalmazás toouse Python verziója `3.4`.</span><span class="sxs-lookup"><span data-stu-id="88905-131">Use hello [az webapp config set](/cli/azure/webapp/config#set) command tooconfigure hello web app toouse Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="88905-132">Egy hello platform által biztosított alapértelmezett tároló hello Python verziója ezzel a módszerrel beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="88905-132">Setting hello Python version this way uses a default container provided by hello platform.</span></span> <span data-ttu-id="88905-133">toouse saját tárolót, lásd: hello hello parancssori felület referenciája [az webapp tároló konfiguráció](/cli/azure/webapp/config/container#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="88905-133">toouse your own container, see hello CLI reference for hello [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up too4 threads.
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
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="88905-134">Keresse meg a toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="88905-134">Browse toohello app</span></span>

<span data-ttu-id="88905-135">Keresse meg a webböngésző segítségével toohello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="88905-135">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="88905-136">Python mintakód hello fut. Ha az Azure App Service web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="88905-136">hello Python sample code is running in an Azure App Service web app.</span></span>

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="88905-138">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="88905-138">**Congratulations!**</span></span> <span data-ttu-id="88905-139">Az első Python-alkalmazás tooApp szolgáltatás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="88905-139">You've deployed your first Python app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="88905-140">Frissítse, és telepítse újra a hello kódot</span><span class="sxs-lookup"><span data-stu-id="88905-140">Update and redeploy hello code</span></span>

<span data-ttu-id="88905-141">Egy helyi szövegszerkesztőben nyissa meg hello `main.py` hello Python alkalmazásban, és egy kis változást toohello szöveg következő toohello győződjön `return` utasítást:</span><span class="sxs-lookup"><span data-stu-id="88905-141">Using a local text editor, open hello `main.py` file in hello Python app, and make a small change toohello text next toohello `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="88905-142">A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="88905-142">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="88905-143">Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban [Tallózás toohello app](#browse-to-the-app) lépést, és a frissítési hello lap.</span><span class="sxs-lookup"><span data-stu-id="88905-143">Once deployment has completed, switch back toohello browser window that opened in hello [Browse toohello app](#browse-to-the-app) step, and refresh hello page.</span></span>

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="88905-145">Az új Azure-webapp kezelése</span><span class="sxs-lookup"><span data-stu-id="88905-145">Manage your new Azure web app</span></span>

<span data-ttu-id="88905-146">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="88905-146">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="88905-147">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="88905-147">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="88905-149">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="88905-149">You see your web app's Overview page.</span></span> <span data-ttu-id="88905-150">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="88905-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="88905-152">hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="88905-152">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="88905-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88905-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="88905-154">Python és PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="88905-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
