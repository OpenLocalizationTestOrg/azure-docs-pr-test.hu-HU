---
title: "a Web Apps Linux Ruby-alkalmazás aaaCreate |} Microsoft Docs"
description: "Ismerje meg, toocreate Ruby Azure web wpp Linux-alkalmazások."
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
ms.openlocfilehash: 99ce3b5ee16703a147787387bb02973defce8190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a>A Linux webalkalmazásokkal Ruby-alkalmazás létrehozása 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. A gyors üzembe helyezés bemutatja, hogyan toocreate egy alapszintű Ruby sínek alkalmazásra, majd telepítenie kell azt tooAzure webalkalmazásként Linux rendszeren.

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a>Előfeltételek

* [Ruby 2.4.1 vagy magasabb](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads).
* Egy [aktív Azure-előfizetéssel](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Hello minta letöltése

Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása

Ahhoz, hogy hello alkalmazás toowork hello sínek server fut. Toohello módosítása *hello életben* könyvtárra, és hello `rails server` indítása hello kiszolgáló parancsot.

```bash
cd hello-world\bin
rails server
```
    
A webböngészőben nyissa meg a túl`http://localhost:3000` tootest hello alkalmazás helyileg.  

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a>Alkalmazás toodisplay üdvözlő üzenet módosítása

Módosítsa a hello alkalmazás, így üdvözlő üzenet megjeleníti. Változás hello alkalmazás vezérlő így HTML toohello böngészőként üdvözlőüzenetére adja vissza. 

Nyissa meg *~/workspace/hello-world/app/controllers/application_controller.rb* szerkesztésre. Módosítsa a hello `ApplicationController` osztály toolook például a következő példakód hello:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Az alkalmazás konfigurálva van. A webböngészőben nyissa meg a túl`http://localhost:3000` tooconfirm hello legfelső szintű kezdőlapja.

![Hello World konfigurálva](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Ruby-webalkalmazás létrehozása az Azure-on

Használjon hello [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) parancs toocreate a webalkalmazás az app service-csomag. 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Ezt követően adja ki a hello [az webalkalmazás létrehozása](https://docs.microsoft.com/cli/azure/webapp) parancs toocreate hello webes alkalmazás, amely az újonnan létrehozott hello service-csomagot használ. Figyelje meg, hogy hello futásidejű értéke túl`ruby|2.3`. Ne feledje tooreplace `<app name>` egy egyedi alkalmazásnévvel rendelkező.

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

Hello webalkalmazás létrehozása után egy **áttekintése** lap elérhető tooview. Keresse meg a tooit. a következő kezdőkép lap hello jelenik meg:

![Kezdőkép lap](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Az alkalmazás központi telepítése

Git toodeploy hello Ruby alkalmazás tooAzure használja. hello webalkalmazás már konfigurált egy Git-telepítés. Hello telepítés URL-CÍMÉT kiállításával le egy [az webalkalmazás központi telepítési](https://docs.microsoft.com/cli/azure/webapp/deployment) parancsot.  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

Figyelje meg, hogy a Git URL-cím hello rendelkezik a webes alkalmazás neve alapján a következő hello:

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

Futtassa a következő parancsok toodeploy hello helyi alkalmazás tooyour Azure-webhelyen hello:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Győződjön meg arról, hogy a hello távoli üzembe helyezési műveleteket sikeres jelentést. hello parancsok által előállított kimeneti hasonló toohello a következő szöveget:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Hello központi telepítés befejezése után indítsa újra a webes alkalmazás hello telepítési tootake hatás hello segítségével [az webalkalmazás újraindítása](https://docs.microsoft.com/cli/azure/webapp#restart) parancsban, ahogy az itt látható:

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

Keresse meg a tooyour hely, és hello eredményeket.

```bash
http://<your web app name>.azurewebsites.net
```
![frissített webalkalmazás](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> Hello alkalmazás újraindul, miközben kísérlet toobrowse hello webhely HTTP-állapotkódot eredményez `Error 503 Server unavailable`. Néhány percig is tarthat toofully újraindítás.
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Következő lépések

[Az Azure App Service webalkalmazásba Linux – gyakori kérdések](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)