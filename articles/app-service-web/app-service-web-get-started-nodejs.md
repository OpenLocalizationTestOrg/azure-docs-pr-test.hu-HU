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
# <a name="create-a-nodejs-web-app-in-azure"></a>Node.js-webalkalmazás létrehozása az Azure-ban

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.  A gyors üzembe helyezés bemutatja, hogyan toodeploy egy Node.js-alkalmazás tooAzure webalkalmazások. Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta Node.js kód toohello webes alkalmazás használatát.

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt. Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezés:

* [A Git telepítése](https://git-scm.com/)
* [Telepítse a Node.js-t és az NPM-et](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hello minta letöltése

Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.

Módosítsa a hello mintakódot tartalmazó toohello könyvtár.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Hello alkalmazás helyi futtatása

Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `npm start` parancsfájl toolaunch hello Node.js HTTP-kiszolgáló a beépített.

```bash
npm start
```

Nyisson meg egy webböngészőt, és keresse meg a http://localhost:1337 toohello minta alkalmazást.

Megjelenik a hello **Hello World** üzenetet kapott hello mintaalkalmazás hello oldal jelenik meg.

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.

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

## <a name="browse-toohello-app"></a>Keresse meg a toohello alkalmazás

Keresse meg a webböngésző segítségével toohello telepített alkalmazás.

```bash
http://<app_name>.azurewebsites.net
```

Node.js mintakód hello fut. Ha az Azure App Service web app alkalmazásban.

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

**Gratulálunk!** Az első Node.js-alkalmazás tooApp szolgáltatás telepítése után.

## <a name="update-and-redeploy-hello-code"></a>Frissítse, és telepítse újra a hello kódot

Egy szövegszerkesztőben nyissa meg hello `index.js` hello Node.js-alkalmazás, és győződjön meg szöveg egy kis módosítása toohello hello hívásában túl`response.end`:

```nodejs
response.end("Hello Azure!");
```

A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban **Tallózás toohello app** . lépés:, majd nyomja le a frissítést.

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

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
> [Node.js és MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)
