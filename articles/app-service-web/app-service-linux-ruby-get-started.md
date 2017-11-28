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
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a><span data-ttu-id="596ae-104">A Linux webalkalmazásokkal Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="596ae-104">Create a Ruby App with Web Apps on Linux</span></span> 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="596ae-105">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="596ae-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="596ae-106">A gyors üzembe helyezés bemutatja, hogyan hozzon létre egy alapszintű Ruby sínek alkalmazásra, majd telepítheti az Azure Linux webalkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="596ae-106">This quickstart shows you how to create a basic Ruby on Rails application you then deploy it to Azure as a Web App on Linux.</span></span>

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a><span data-ttu-id="596ae-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="596ae-108">Prerequisites</span></span>

* <span data-ttu-id="596ae-109">[Ruby 2.4.1 vagy magasabb](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span><span class="sxs-lookup"><span data-stu-id="596ae-109">[Ruby 2.4.1 or higher](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span></span>
* <span data-ttu-id="596ae-110">[Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="596ae-110">[Git](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="596ae-111">Egy [aktív Azure-előfizetéssel](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="596ae-111">An [active Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="596ae-112">A minta letöltése</span><span class="sxs-lookup"><span data-stu-id="596ae-112">Download the sample</span></span>

<span data-ttu-id="596ae-113">Egy terminálablakot futtassa a következő parancsot a helyi számítógépen, a minta app tárház klónozása:</span><span class="sxs-lookup"><span data-stu-id="596ae-113">In a terminal window, run the following command to clone the sample app repository to your local machine:</span></span>

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-the-application-locally"></a><span data-ttu-id="596ae-114">Az alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="596ae-114">Run the application locally</span></span>

<span data-ttu-id="596ae-115">Futtassa a sínek kiszolgáló ahhoz, hogy az alkalmazás működéséhez.</span><span class="sxs-lookup"><span data-stu-id="596ae-115">Run the rails server in order for the application to work.</span></span> <span data-ttu-id="596ae-116">Módosítsa a *hello életben* könyvtár, és a `rails server` indítja el a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="596ae-116">Change to the *hello-world* directory, and the `rails server` command starts the server.</span></span>

```bash
cd hello-world\bin
rails server
```
    
<span data-ttu-id="596ae-117">Nyissa meg webböngészővel, `http://localhost:3000` az alkalmazás helyi teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="596ae-117">Using your web browser, navigate to `http://localhost:3000` to test the app locally.</span></span>    

![A globális hello](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a><span data-ttu-id="596ae-119">Módosítsa a alkalmazást az üdvözlő üzenet megjelenítése</span><span class="sxs-lookup"><span data-stu-id="596ae-119">Modify app to display welcome message</span></span>

<span data-ttu-id="596ae-120">Módosítsa az alkalmazás, így üdvözlő üzenet megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="596ae-120">Modify the application so it displays a welcome message.</span></span> <span data-ttu-id="596ae-121">Módosítsa az alkalmazás a tartományvezérlő, a rendszer visszaadja az üzenet HTML formátumban a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="596ae-121">Change the application's controller so it returns the message as HTML to the browser.</span></span> 

<span data-ttu-id="596ae-122">Nyissa meg *~/workspace/hello-world/app/controllers/application_controller.rb* szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="596ae-122">Open *~/workspace/hello-world/app/controllers/application_controller.rb* for editing.</span></span> <span data-ttu-id="596ae-123">Módosítsa a `ApplicationController` kikeresni a következő osztály, például a következő példakód:</span><span class="sxs-lookup"><span data-stu-id="596ae-123">Modify the `ApplicationController` class to look like the following code sample:</span></span>

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

<span data-ttu-id="596ae-124">Az alkalmazás konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="596ae-124">Your app is now configured.</span></span> <span data-ttu-id="596ae-125">Nyissa meg webböngészővel, `http://localhost:3000` a legfelső szintű kezdőlapja megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="596ae-125">Using your web browser, navigate to `http://localhost:3000` to confirm the root landing page.</span></span>

![Hello World konfigurálva](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a><span data-ttu-id="596ae-127">Ruby-webalkalmazás létrehozása az Azure-on</span><span class="sxs-lookup"><span data-stu-id="596ae-127">Create a Ruby web app on Azure</span></span>

<span data-ttu-id="596ae-128">Használja a [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) parancsot a webalkalmazás az app service-csomag létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="596ae-128">Use the [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) command to create an app service plan for your web app.</span></span> 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

<span data-ttu-id="596ae-129">Ezt követően adja ki a [az webalkalmazás létrehozása](https://docs.microsoft.com/cli/azure/webapp) parancsot a webes alkalmazás, amely használja az újonnan létrehozott service-csomag létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="596ae-129">Next, issue the [az webapp create](https://docs.microsoft.com/cli/azure/webapp) command to create the web app that uses the newly created service plan.</span></span> <span data-ttu-id="596ae-130">Figyelje meg, hogy a futtatókörnyezet értéke `ruby|2.3`.</span><span class="sxs-lookup"><span data-stu-id="596ae-130">Notice that the runtime is set to `ruby|2.3`.</span></span> <span data-ttu-id="596ae-131">Ne felejtse el lecserélni `<app name>` egy egyedi alkalmazásnévvel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="596ae-131">Don't forget to replace `<app name>` with a unique app name.</span></span>

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

<span data-ttu-id="596ae-132">A webalkalmazás létrehozása után egy **áttekintése** a lap megtekintéséhez érhető el.</span><span class="sxs-lookup"><span data-stu-id="596ae-132">Once the web app is created, an **Overview** page is available to view.</span></span> <span data-ttu-id="596ae-133">Keresse meg azt.</span><span class="sxs-lookup"><span data-stu-id="596ae-133">Navigate to it.</span></span> <span data-ttu-id="596ae-134">A következő kezdőkép lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="596ae-134">The following splash page is displayed:</span></span>

![Kezdőkép lap](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a><span data-ttu-id="596ae-136">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="596ae-136">Deploy your application</span></span>

<span data-ttu-id="596ae-137">A Git segítségével az Azure Ruby alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="596ae-137">Use Git to deploy the Ruby application to Azure.</span></span> <span data-ttu-id="596ae-138">A webalkalmazás már konfigurált egy Git-telepítés.</span><span class="sxs-lookup"><span data-stu-id="596ae-138">The web app already has a Git deployment configured.</span></span> <span data-ttu-id="596ae-139">Visszaállíthatja a telepítés URL-CÍMÉT is kiállításával egy [az webalkalmazás központi telepítési](https://docs.microsoft.com/cli/azure/webapp/deployment) parancsot.</span><span class="sxs-lookup"><span data-stu-id="596ae-139">You can retrieve the deployment URL by issuing an [az webapp deployment](https://docs.microsoft.com/cli/azure/webapp/deployment) command.</span></span>  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="596ae-140">Figyelje meg, hogy a Git URL-cím formátuma a következő a webes alkalmazás neve alapján:</span><span class="sxs-lookup"><span data-stu-id="596ae-140">Notice that the Git URL has the following form based on your web app name:</span></span>

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

<span data-ttu-id="596ae-141">Az Azure webhelyének helyi alkalmazást telepíti a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="596ae-141">Run the following commands to deploy the local application to your Azure website:</span></span>

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="596ae-142">Győződjön meg arról, hogy a távoli üzembe helyezési műveleteinek sikeres jelentést.</span><span class="sxs-lookup"><span data-stu-id="596ae-142">Confirm that the remote deployment operations report success.</span></span> <span data-ttu-id="596ae-143">A parancsok a kimenet az alábbihoz hasonló termékek:</span><span class="sxs-lookup"><span data-stu-id="596ae-143">The commands produce output similar to the following text:</span></span>

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

<span data-ttu-id="596ae-144">A telepítés befejezése után indítsa újra a webes alkalmazást a központi telepítés használatával érvénybe a [az webalkalmazás újraindítása](https://docs.microsoft.com/cli/azure/webapp#restart) parancsban, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="596ae-144">Once the deployment has completed, restart your web app for the deployment to take effect by using the [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#restart) command, as shown here:</span></span>

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="596ae-145">Keresse meg a helyet, és ellenőrizze az eredményt.</span><span class="sxs-lookup"><span data-stu-id="596ae-145">Navigate to your site and verify the results.</span></span>

```bash
http://<your web app name>.azurewebsites.net
```
![frissített webalkalmazás](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> <span data-ttu-id="596ae-147">Az alkalmazás újraindul, miközben megpróbálta keresse meg a hely HTTP-állapotkódot eredményez `Error 503 Server unavailable`.</span><span class="sxs-lookup"><span data-stu-id="596ae-147">While the app is restarting, attempting to browse the site results in an HTTP status code `Error 503 Server unavailable`.</span></span> <span data-ttu-id="596ae-148">Teljes újraindítására néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="596ae-148">It may take a few minutes to fully restart.</span></span>
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a><span data-ttu-id="596ae-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="596ae-149">Next steps</span></span>

[<span data-ttu-id="596ae-150">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="596ae-150">Azure App Service Web App on Linux FAQ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)