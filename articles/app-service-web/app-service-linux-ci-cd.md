---
title: "központi telepítése Linux Azure Web Apps aaaContinuous |} Microsoft Docs"
description: "Hogyan toosetup folyamatos üzembe helyezés Linux Azure Web App alkalmazásban."
keywords: az Azure app service, linux, oss, acr
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f94d837e27605da58428f507ab2b0eb3af3297e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a><span data-ttu-id="6c021-104">Folyamatos üzembe helyezés az Azure Web Apps Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="6c021-104">Continuous deployment with Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="6c021-105">Ebben az oktatóanyagban konfigurálása a folyamatos üzembe egy egyéni tároló lemezkép felügyelt [Azure tároló beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/) tárházak vagy [Docker Hub](https://hub.docker.com).</span><span class="sxs-lookup"><span data-stu-id="6c021-105">In this tutorial, you configure continuous deployment for a custom container image from Managed [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) repositories or [Docker Hub](https://hub.docker.com).</span></span>

## <a name="step-1---sign-in-tooazure"></a><span data-ttu-id="6c021-106">1. lépés – tooAzure bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6c021-106">Step 1 - Sign in tooAzure</span></span>

<span data-ttu-id="6c021-107">Az Azure portálon, a http://portal.azure.com toohello bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6c021-107">Sign in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---enable-container-continuous-deployment-feature"></a><span data-ttu-id="6c021-108">2. lépés - engedélyezési tároló folyamatos üzembe helyezés szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6c021-108">Step 2 - Enable container continuous deployment feature</span></span>

<span data-ttu-id="6c021-109">Engedélyezheti a hello folyamatos üzembe helyezés funkció használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és hello a következő parancs végrehajtása</span><span class="sxs-lookup"><span data-stu-id="6c021-109">You can enable hello continuous deployment feature using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

<span data-ttu-id="6c021-110">A hello  **[Azure-portálon](https://portal.azure.com/)**, hello kattintson **App Service** hello lap hello bal oldali lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6c021-110">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="6c021-111">Kattintson a az alkalmazás, amelyet tooconfigure Docker Hub folyamatos üzembe hello nevére.</span><span class="sxs-lookup"><span data-stu-id="6c021-111">Click on hello name of your app that you want tooconfigure Docker Hub continuous deployment for.</span></span>

<span data-ttu-id="6c021-112">A hello **Alkalmazásbeállítások**, hozzáadhat egy alkalmazást nevű beállítása `DOCKER_ENABLE_CI` hello értékű `true`.</span><span class="sxs-lookup"><span data-stu-id="6c021-112">In hello **App settings**, add an app setting called `DOCKER_ENABLE_CI` with hello value `true`.</span></span>

![Helyezze be a Alkalmazásbeállítás képe](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a><span data-ttu-id="6c021-114">3. lépés - a Webhook URL-CÍMÉT előkészítése</span><span class="sxs-lookup"><span data-stu-id="6c021-114">Step 3 - Prepare Webhook URL</span></span>

<span data-ttu-id="6c021-115">Ezt úgy szerezheti be hello Webhook URL-cím használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és hello a következő parancs végrehajtása</span><span class="sxs-lookup"><span data-stu-id="6c021-115">You can obtain hello Webhook URL using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

<span data-ttu-id="6c021-116">Hello Webhook URL-CÍMÉT, a következő végpont toohave hello kell: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span><span class="sxs-lookup"><span data-stu-id="6c021-116">For hello Webhook URL, you need toohave hello following endpoint: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span></span>

<span data-ttu-id="6c021-117">Ezt úgy szerezheti be a `publishingusername` és `publishingpwd` hello webalkalmazás letöltésével közzététele a profil hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6c021-117">You can obtain your `publishingusername` and `publishingpwd` by downloading hello web app publish profile using hello Azure portal.</span></span>

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a><span data-ttu-id="6c021-119">4. lépés – a webes hook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6c021-119">Step 4 - Add a web hook</span></span>

### <a name="azure-container-registry"></a><span data-ttu-id="6c021-120">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="6c021-120">Azure Container Registry</span></span>

<span data-ttu-id="6c021-121">A beállításjegyzék portálpanelén kattintson **Webhookok**, hozzon létre egy új webhook kattintva **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6c021-121">In your registry portal blade, click **Webhooks**, create a new webhook by clicking **Add**.</span></span> <span data-ttu-id="6c021-122">A hello **webhook létrehozása** panelen adjon a webhook nevét.</span><span class="sxs-lookup"><span data-stu-id="6c021-122">In hello **Create webhook** blade, give your webhook a name.</span></span> <span data-ttu-id="6c021-123">Hello Webhook URI, meg kell tooprovide hello URL-címet szerzett **3. lépés**</span><span class="sxs-lookup"><span data-stu-id="6c021-123">For hello Webhook URI, you need tooprovide hello URL obtained from **Step 3**</span></span>

<span data-ttu-id="6c021-124">Győződjön meg arról, hogy hello hatókör határozza meg, a tároló képet tartalmazó tárház hello.</span><span class="sxs-lookup"><span data-stu-id="6c021-124">Make sure that you define hello scope as hello repo that contains your container image.</span></span>

![Helyezze be a webhook képe](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

<span data-ttu-id="6c021-126">Hello lemezkép frissítésekor a lekérdezi hello webalkalmazás frissített automatikusan hello új lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="6c021-126">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

### <a name="docker-hub"></a><span data-ttu-id="6c021-127">Docker központ</span><span class="sxs-lookup"><span data-stu-id="6c021-127">Docker Hub</span></span>

<span data-ttu-id="6c021-128">A Docker Hub oldalon kattintson **Webhookok**, majd **A WEBHOOK létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6c021-128">In your Docker Hub page, click **Webhooks**, then **CREATE A WEBHOOK**.</span></span>

![Helyezze be a webhook 1 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

<span data-ttu-id="6c021-130">A Webhook URL-CÍMÉT. hello, tooprovide hello URL-címet szerzett kell **3. lépés**</span><span class="sxs-lookup"><span data-stu-id="6c021-130">For hello Webhook URL, you need tooprovide hello URL obtained from **Step 3**</span></span>

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

<span data-ttu-id="6c021-132">Hello lemezkép frissítésekor a lekérdezi hello webalkalmazás frissített automatikusan hello új lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="6c021-132">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c021-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c021-133">Next steps</span></span>
* [<span data-ttu-id="6c021-134">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="6c021-134">What is Azure Web App on Linux?</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="6c021-135">Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="6c021-135">Azure Container Registry</span></span>](https://azure.microsoft.com/en-us/services/container-registry/)
* [<span data-ttu-id="6c021-136">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="6c021-136">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="6c021-137">.NET Core Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="6c021-137">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="6c021-138">Ruby Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="6c021-138">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="6c021-139">Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="6c021-139">How toouse a custom Docker image for Azure Web App on Linux</span></span>](./app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="6c021-140">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="6c021-140">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md) 
* [<span data-ttu-id="6c021-141">Webalkalmazás az Azure CLI 2.0 verziót használja Linux kezelése</span><span class="sxs-lookup"><span data-stu-id="6c021-141">Manage Web App on Linux using Azure CLI 2.0</span></span>](./app-service-linux-cli.md)



