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
# <a name="create-a-static-html-web-app-in-azure"></a>Statikus HTML-webalkalmazás létrehozása az Azure-ban

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.  A gyors üzembe helyezés bemutatja, hogyan toodeploy egy egyszerű HTML + CSS hely tooAzure webalkalmazások. Hello segítségével hello-webalkalmazás létrehozása [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git toodeploy minta HTML tartalom toohello webes alkalmazás használatát.

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt. Hello előfeltételek telepítése után tart, körülbelül öt perc toocomplete hello lépéseket.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezés:

- [A Git telepítése](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Hello minta letöltése

Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

Használható a terminálablakot toorun minden hello parancsot a gyors üzembe helyezés.

## <a name="view-hello-html"></a>Hello HTML megtekintése

Keresse meg a HTML hello mintát tartalmazó toohello könyvtár. Nyissa meg hello *index.html* fájl a böngészőben.

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Üres webalkalmazás oldal](media/app-service-web-get-started-html/app-service-web-service-created.png)

Ezzel létrehozott egy üres, új webalkalmazást az Azure-ban.

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

## <a name="browse-toohello-app"></a>Keresse meg a toohello alkalmazás

Nyissa meg böngészőben, toohello Azure webes alkalmazás URL-címe:

```
http://<app_name>.azurewebsites.net
```

az Azure App Service webalkalmazás futtatásához használt hello lap.

![Mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Gratulálunk!** Az első HTML-alkalmazás tooApp szolgáltatás telepítése után.

## <a name="update-and-redeploy-hello-app"></a>Frissítse és telepítse újra hello alkalmazást

Nyissa meg hello *index.html* fájlt egy szövegszerkesztőben, és tegye a módosítás toohello jelölés során. Hello H1 címsor módosítsa az "Azure App Service – minta statikus HTML-webhely" toojust például "az Azure App Service".

A Git a változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.

```bash
git commit -am "updated HTML"
git push azure master
```

Központi telepítés befejezése után frissítse a böngésző toosee hello módosításokat.

![Frissített mintaalkalmazás kezdőlapja](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Az új Azure-webapp kezelése

Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-html/portal1.png)

Megtekintheti a webalkalmazás Áttekintés oldalát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés. 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-html/portal2.png)

hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Egyéni tartomány leképezése](app-service-web-tutorial-custom-domain.md)
