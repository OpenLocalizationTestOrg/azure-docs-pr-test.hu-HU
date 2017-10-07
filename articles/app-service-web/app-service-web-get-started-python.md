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
# <a name="create-a-python-web-app-in-azure"></a>Python-webapp létrehozása az Azure-ban

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.  A gyors üzembe helyezési útmutató végigvezeti toodevelop és a Python alkalmazást tooAzure webalkalmazások telepítését. Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta Python kódját toohello webes alkalmazás használatát.

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt. Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.
## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

1. [A Git telepítése](https://git-scm.com/)
1. [Telepítse a Pythont](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hello minta letöltése

Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.

Módosítsa a hello mintakódot tartalmazó toohello könyvtár.

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Hello alkalmazás helyi futtatása

Szükséges hello csomagok használatával `pip`.

```bash
pip install -r requirements.txt
```

Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `Python` parancs toolaunch hello beépített Python webalkalmazás-kiszolgáló.

```bash
python main.py
```

Nyisson meg egy webböngészőt, és keresse meg a http://localhost:5000 toohello minta alkalmazást.

Megtekintheti a hello **Hello World** üzenetet kapott hello mintaalkalmazás hello oldal jelenik meg.

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-python/app-service-web-service-created.png)

Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.

## <a name="configure-toouse-python"></a>Toouse Python konfigurálása

Használjon hello [az webapp konfiguráció](/cli/azure/webapp/config#set) parancs tooconfigure hello webes alkalmazás toouse Python verziója `3.4`.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Egy hello platform által biztosított alapértelmezett tároló hello Python verziója ezzel a módszerrel beállítást használja. toouse saját tárolót, lásd: hello hello parancssori felület referenciája [az webapp tároló konfiguráció](/cli/azure/webapp/config/container#set) parancsot.

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

## <a name="browse-toohello-app"></a>Keresse meg a toohello alkalmazás

Keresse meg a webböngésző segítségével toohello telepített alkalmazás.

```bash
http://<app_name>.azurewebsites.net
```

Python mintakód hello fut. Ha az Azure App Service web app alkalmazásban.

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Gratulálunk!** Az első Python-alkalmazás tooApp szolgáltatás telepítése után.

## <a name="update-and-redeploy-hello-code"></a>Frissítse, és telepítse újra a hello kódot

Egy helyi szövegszerkesztőben nyissa meg hello `main.py` hello Python alkalmazásban, és egy kis változást toohello szöveg következő toohello győződjön `return` utasítást:

```python
return 'Hello, Azure!'
```

A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban [Tallózás toohello app](#browse-to-the-app) lépést, és a frissítési hello lap.

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Az új Azure-webapp kezelése

Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Megtekintheti a webalkalmazás Áttekintés oldalát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés. 

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Python és PostgreSQL](app-service-web-tutorial-docker-python-postgresql-app.md)
