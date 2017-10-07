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
# <a name="create-a-php-web-app-in-azure"></a>PHP-webapp létrehozása az Azure-ban

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.  A gyors üzembe helyezési oktatóanyag bemutatja, hogyan toodeploy egy PHP-alkalmazás tooAzure webalkalmazások. Létrehozhat hello web app használatával hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a felhő rendszerhéjat, és Git toodeploy minta PHP kód toohello webalkalmazás használatát.

![Az Azure-ban futó mintaalkalmazás]](media/app-service-web-get-started-php/hello-world-in-browser.png)

A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt. Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezés:

* [A Git telepítése](https://git-scm.com/)
* [A PHP telepítése](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a>Helyileg hello minta letöltése

Egy terminálablakot futtassa a következő parancsok hello. A hello minta alkalmazás tooyour helyi gép klónozását, és keresse meg a toohello directory hello minta kódot tartalmazó.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Hello alkalmazás helyi futtatása

Hello alkalmazás helyi futtatásához nyisson meg egy terminálablakot, és hello segítségével `php` parancs toolaunch hello beépített PHP webkiszolgáló.

```bash
php -S localhost:8080
```

Nyisson meg egy webböngészőt, és keresse meg a mintaalkalmazás toohello: 8080.

Megjelenik a hello **Hello World!** hello lapján megjelenő hello mintaalkalmazás üzenetét.

![A helyileg futó mintaalkalmazás](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

A Terminálszolgáltatások ablakában, nyomja le az **Ctrl + C** tooexit hello webkiszolgáló.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Üres webalkalmazás oldal](media/app-service-web-get-started-php/app-service-web-service-created.png)

Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.

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

## <a name="browse-toohello-app-locally"></a>Keresse meg a helyi toohello alkalmazás

Keresse meg a webböngésző segítségével toohello telepített alkalmazás.

```bash
http://<app_name>.azurewebsites.net
```

hello PHP mintakód fut. Ha az Azure App Service web app alkalmazásban.

![Az Azure-ban futó mintaalkalmazás](media/app-service-web-get-started-php/hello-world-in-browser.png)

**Gratulálunk!** Az első PHP-alkalmazás tooApp szolgáltatás telepítése után.

## <a name="update-locally-and-redeploy-hello-code"></a>Frissítés helyben, és telepítse újra a hello kódot

Egy helyi szövegszerkesztőben nyissa meg hello `index.php` hello PHP alkalmazáson belül, és győződjön szöveg egy kis módosítása toohello hello karakterláncon belüli következő túl`echo`:

```php
echo "Hello Azure!";
```

A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.

```bash
git commit -am "updated output"
git push azure master
```

Központi telepítés befejezése után kapcsoló hello megnyitott hátsó toohello böngészőablakban **Tallózás toohello app** lépést, és a frissítési hello lap.

![Az Azure-ban futó frissített mintaalkalmazás](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Az új Azure-webapp kezelése

Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Megtekintheti a webalkalmazás Áttekintés oldalát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.

![Az App Service panel az Azure Portalon](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [PHP és MySQL](app-service-web-tutorial-php-mysql.md)
