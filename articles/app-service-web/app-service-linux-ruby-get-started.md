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
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a><span data-ttu-id="740f8-104">A Linux webalkalmazásokkal Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="740f8-104">Create a Ruby App with Web Apps on Linux</span></span> 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="740f8-105">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="740f8-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="740f8-106">A gyors üzembe helyezés bemutatja, hogyan toocreate egy alapszintű Ruby sínek alkalmazásra, majd telepítenie kell azt tooAzure webalkalmazásként Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="740f8-106">This quickstart shows you how toocreate a basic Ruby on Rails application you then deploy it tooAzure as a Web App on Linux.</span></span>

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a><span data-ttu-id="740f8-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="740f8-108">Prerequisites</span></span>

* <span data-ttu-id="740f8-109">[Ruby 2.4.1 vagy magasabb](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span><span class="sxs-lookup"><span data-stu-id="740f8-109">[Ruby 2.4.1 or higher](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span></span>
* <span data-ttu-id="740f8-110">[Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="740f8-110">[Git](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="740f8-111">Egy [aktív Azure-előfizetéssel](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="740f8-111">An [active Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="740f8-112">Hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="740f8-112">Download hello sample</span></span>

<span data-ttu-id="740f8-113">Egy terminálablakot futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello:</span><span class="sxs-lookup"><span data-stu-id="740f8-113">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a><span data-ttu-id="740f8-114">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="740f8-114">Run hello application locally</span></span>

<span data-ttu-id="740f8-115">Ahhoz, hogy hello alkalmazás toowork hello sínek server fut.</span><span class="sxs-lookup"><span data-stu-id="740f8-115">Run hello rails server in order for hello application toowork.</span></span> <span data-ttu-id="740f8-116">Toohello módosítása *hello életben* könyvtárra, és hello `rails server` indítása hello kiszolgáló parancsot.</span><span class="sxs-lookup"><span data-stu-id="740f8-116">Change toohello *hello-world* directory, and hello `rails server` command starts hello server.</span></span>

```bash
cd hello-world\bin
rails server
```
    
<span data-ttu-id="740f8-117">A webböngészőben nyissa meg a túl`http://localhost:3000` tootest hello alkalmazás helyileg.</span><span class="sxs-lookup"><span data-stu-id="740f8-117">Using your web browser, navigate too`http://localhost:3000` tootest hello app locally.</span></span>  

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a><span data-ttu-id="740f8-119">Alkalmazás toodisplay üdvözlő üzenet módosítása</span><span class="sxs-lookup"><span data-stu-id="740f8-119">Modify app toodisplay welcome message</span></span>

<span data-ttu-id="740f8-120">Módosítsa a hello alkalmazás, így üdvözlő üzenet megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="740f8-120">Modify hello application so it displays a welcome message.</span></span> <span data-ttu-id="740f8-121">Változás hello alkalmazás vezérlő így HTML toohello böngészőként üdvözlőüzenetére adja vissza.</span><span class="sxs-lookup"><span data-stu-id="740f8-121">Change hello application's controller so it returns hello message as HTML toohello browser.</span></span> 

<span data-ttu-id="740f8-122">Nyissa meg *~/workspace/hello-world/app/controllers/application_controller.rb* szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="740f8-122">Open *~/workspace/hello-world/app/controllers/application_controller.rb* for editing.</span></span> <span data-ttu-id="740f8-123">Módosítsa a hello `ApplicationController` osztály toolook például a következő példakód hello:</span><span class="sxs-lookup"><span data-stu-id="740f8-123">Modify hello `ApplicationController` class toolook like hello following code sample:</span></span>

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

<span data-ttu-id="740f8-124">Az alkalmazás konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="740f8-124">Your app is now configured.</span></span> <span data-ttu-id="740f8-125">A webböngészőben nyissa meg a túl`http://localhost:3000` tooconfirm hello legfelső szintű kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="740f8-125">Using your web browser, navigate too`http://localhost:3000` tooconfirm hello root landing page.</span></span>

![Hello World konfigurálva](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a><span data-ttu-id="740f8-127">Ruby-webalkalmazás létrehozása az Azure-on</span><span class="sxs-lookup"><span data-stu-id="740f8-127">Create a Ruby web app on Azure</span></span>

<span data-ttu-id="740f8-128">Használjon hello [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) parancs toocreate a webalkalmazás az app service-csomag.</span><span class="sxs-lookup"><span data-stu-id="740f8-128">Use hello [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) command toocreate an app service plan for your web app.</span></span> 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

<span data-ttu-id="740f8-129">Ezt követően adja ki a hello [az webalkalmazás létrehozása](https://docs.microsoft.com/cli/azure/webapp) parancs toocreate hello webes alkalmazás, amely az újonnan létrehozott hello service-csomagot használ.</span><span class="sxs-lookup"><span data-stu-id="740f8-129">Next, issue hello [az webapp create](https://docs.microsoft.com/cli/azure/webapp) command toocreate hello web app that uses hello newly created service plan.</span></span> <span data-ttu-id="740f8-130">Figyelje meg, hogy hello futásidejű értéke túl`ruby|2.3`.</span><span class="sxs-lookup"><span data-stu-id="740f8-130">Notice that hello runtime is set too`ruby|2.3`.</span></span> <span data-ttu-id="740f8-131">Ne feledje tooreplace `<app name>` egy egyedi alkalmazásnévvel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="740f8-131">Don't forget tooreplace `<app name>` with a unique app name.</span></span>

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

<span data-ttu-id="740f8-132">Hello webalkalmazás létrehozása után egy **áttekintése** lap elérhető tooview.</span><span class="sxs-lookup"><span data-stu-id="740f8-132">Once hello web app is created, an **Overview** page is available tooview.</span></span> <span data-ttu-id="740f8-133">Keresse meg a tooit.</span><span class="sxs-lookup"><span data-stu-id="740f8-133">Navigate tooit.</span></span> <span data-ttu-id="740f8-134">a következő kezdőkép lap hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="740f8-134">hello following splash page is displayed:</span></span>

![Kezdőkép lap](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a><span data-ttu-id="740f8-136">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="740f8-136">Deploy your application</span></span>

<span data-ttu-id="740f8-137">Git toodeploy hello Ruby alkalmazás tooAzure használja.</span><span class="sxs-lookup"><span data-stu-id="740f8-137">Use Git toodeploy hello Ruby application tooAzure.</span></span> <span data-ttu-id="740f8-138">hello webalkalmazás már konfigurált egy Git-telepítés.</span><span class="sxs-lookup"><span data-stu-id="740f8-138">hello web app already has a Git deployment configured.</span></span> <span data-ttu-id="740f8-139">Hello telepítés URL-CÍMÉT kiállításával le egy [az webalkalmazás központi telepítési](https://docs.microsoft.com/cli/azure/webapp/deployment) parancsot.</span><span class="sxs-lookup"><span data-stu-id="740f8-139">You can retrieve hello deployment URL by issuing an [az webapp deployment](https://docs.microsoft.com/cli/azure/webapp/deployment) command.</span></span>  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="740f8-140">Figyelje meg, hogy a Git URL-cím hello rendelkezik a webes alkalmazás neve alapján a következő hello:</span><span class="sxs-lookup"><span data-stu-id="740f8-140">Notice that hello Git URL has hello following form based on your web app name:</span></span>

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

<span data-ttu-id="740f8-141">Futtassa a következő parancsok toodeploy hello helyi alkalmazás tooyour Azure-webhelyen hello:</span><span class="sxs-lookup"><span data-stu-id="740f8-141">Run hello following commands toodeploy hello local application tooyour Azure website:</span></span>

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="740f8-142">Győződjön meg arról, hogy a hello távoli üzembe helyezési műveleteket sikeres jelentést.</span><span class="sxs-lookup"><span data-stu-id="740f8-142">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="740f8-143">hello parancsok által előállított kimeneti hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="740f8-143">hello commands produce output similar toohello following text:</span></span>

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

<span data-ttu-id="740f8-144">Hello központi telepítés befejezése után indítsa újra a webes alkalmazás hello telepítési tootake hatás hello segítségével [az webalkalmazás újraindítása](https://docs.microsoft.com/cli/azure/webapp#restart) parancsban, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="740f8-144">Once hello deployment has completed, restart your web app for hello deployment tootake effect by using hello [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#restart) command, as shown here:</span></span>

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="740f8-145">Keresse meg a tooyour hely, és hello eredményeket.</span><span class="sxs-lookup"><span data-stu-id="740f8-145">Navigate tooyour site and verify hello results.</span></span>

```bash
http://<your web app name>.azurewebsites.net
```
![frissített webalkalmazás](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> <span data-ttu-id="740f8-147">Hello alkalmazás újraindul, miközben kísérlet toobrowse hello webhely HTTP-állapotkódot eredményez `Error 503 Server unavailable`.</span><span class="sxs-lookup"><span data-stu-id="740f8-147">While hello app is restarting, attempting toobrowse hello site results in an HTTP status code `Error 503 Server unavailable`.</span></span> <span data-ttu-id="740f8-148">Néhány percig is tarthat toofully újraindítás.</span><span class="sxs-lookup"><span data-stu-id="740f8-148">It may take a few minutes toofully restart.</span></span>
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a><span data-ttu-id="740f8-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="740f8-149">Next steps</span></span>

[<span data-ttu-id="740f8-150">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="740f8-150">Azure App Service Web App on Linux FAQ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)