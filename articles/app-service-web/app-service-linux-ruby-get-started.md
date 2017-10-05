---
title: "A Linux webalkalmazásokkal Ruby-alkalmazás létrehozása |} Microsoft Docs"
description: "Ismerje meg, az Azure web wpp Linux Ruby-alkalmazásai létrehozására."
keywords: az Azure app service, a linux, a oss, a ruby
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: wesmc;rachelap
ms.openlocfilehash: 17f3f1a2122c508501134a0c43ab6abce412fb44
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a>A Linux webalkalmazásokkal Ruby-alkalmazás létrehozása 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. A gyors üzembe helyezés bemutatja, hogyan hozzon létre egy alapszintű Ruby sínek alkalmazásra, majd telepítheti az Azure Linux webalkalmazásként.

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a>Előfeltételek

* [Ruby 2.4.1 vagy magasabb](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads).
* Egy [aktív Azure-előfizetéssel](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>A minta letöltése

Egy terminálablakot futtassa a következő parancsot a helyi számítógépen, a minta app tárház klónozása:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-the-application-locally"></a>Az alkalmazás helyi futtatása

Futtassa a sínek kiszolgáló ahhoz, hogy az alkalmazás működéséhez. Módosítsa a *hello életben* könyvtár, és a `rails server` indítja el a kiszolgálón.

```bash
cd hello-world\bin
rails server
```
    
Nyissa meg webböngészővel, `http://localhost:3000` az alkalmazás helyi teszteléséhez.    

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Módosítsa a alkalmazást az üdvözlő üzenet megjelenítése

Módosítsa az alkalmazás, így üdvözlő üzenet megjeleníti. Módosítsa az alkalmazás a tartományvezérlő, a rendszer visszaadja az üzenet HTML formátumban a böngészőben. 

Nyissa meg *~/workspace/hello-world/app/controllers/application_controller.rb* szerkesztésre. Módosítsa a `ApplicationController` kikeresni a következő osztály, például a következő példakód:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Az alkalmazás konfigurálva van. Nyissa meg webböngészővel, `http://localhost:3000` a legfelső szintű kezdőlapja megerősítéséhez.

![Hello World konfigurálva](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Ruby-webalkalmazás létrehozása az Azure-on

Használja a [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) parancsot a webalkalmazás az app service-csomag létrehozásához. 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Ezt követően adja ki a [az webalkalmazás létrehozása](https://docs.microsoft.com/cli/azure/webapp) parancsot a webes alkalmazás, amely használja az újonnan létrehozott service-csomag létrehozásához. Figyelje meg, hogy a futtatókörnyezet értéke `ruby|2.3`. Ne felejtse el lecserélni `<app name>` egy egyedi alkalmazásnévvel rendelkező.

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

A webalkalmazás létrehozása után egy **áttekintése** a lap megtekintéséhez érhető el. Keresse meg azt. A következő kezdőkép lap jelenik meg:

![Kezdőkép lap](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Az alkalmazás központi telepítése

A Git segítségével az Azure Ruby alkalmazást telepíti. A webalkalmazás már konfigurált egy Git-telepítés. Visszaállíthatja a telepítés URL-CÍMÉT is kiállításával egy [az webalkalmazás központi telepítési](https://docs.microsoft.com/cli/azure/webapp/deployment) parancsot.  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

Figyelje meg, hogy a Git URL-cím formátuma a következő a webes alkalmazás neve alapján:

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

Az Azure webhelyének helyi alkalmazást telepíti a következő parancsok futtatásával:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Győződjön meg arról, hogy a távoli üzembe helyezési műveleteinek sikeres jelentést. A parancsok a kimenet az alábbihoz hasonló termékek:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

A telepítés befejezése után indítsa újra a webes alkalmazást a központi telepítés használatával érvénybe a [az webalkalmazás újraindítása](https://docs.microsoft.com/cli/azure/webapp#restart) parancsban, ahogy az itt látható:

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

Keresse meg a helyet, és ellenőrizze az eredményt.

```bash
http://<your web app name>.azurewebsites.net
```
![frissített webalkalmazás](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> Az alkalmazás újraindul, miközben megpróbálta keresse meg a hely HTTP-állapotkódot eredményez `Error 503 Server unavailable`. Teljes újraindítására néhány percig is eltarthat.
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Következő lépések

[Az Azure App Service webalkalmazásba Linux – gyakori kérdések](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)