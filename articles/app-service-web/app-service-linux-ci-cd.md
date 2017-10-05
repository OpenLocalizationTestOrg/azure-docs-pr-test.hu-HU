---
title: "Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux |} Microsoft Docs"
description: "A telepítő a folyamatos üzembe helyezés az Azure Web Apps Linux módjáról."
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
ms.openlocfilehash: f8f7d51003f8a55b7f51e8cc2cea838e8e5a6196
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a><span data-ttu-id="7af42-104">Folyamatos üzembe helyezés az Azure Web Apps Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="7af42-104">Continuous deployment with Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="7af42-105">Ebben az oktatóanyagban konfigurálása a folyamatos üzembe egy egyéni tároló lemezkép felügyelt [Azure tároló beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/) tárházak vagy [Docker Hub](https://hub.docker.com).</span><span class="sxs-lookup"><span data-stu-id="7af42-105">In this tutorial, you configure continuous deployment for a custom container image from Managed [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) repositories or [Docker Hub](https://hub.docker.com).</span></span>

## <a name="step-1---sign-in-to-azure"></a><span data-ttu-id="7af42-106">1. lépés – bejelentkezés az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="7af42-106">Step 1 - Sign in to Azure</span></span>

<span data-ttu-id="7af42-107">Jelentkezzen be az Azure portálon, a http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="7af42-107">Sign in to the Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---enable-container-continuous-deployment-feature"></a><span data-ttu-id="7af42-108">2. lépés - engedélyezési tároló folyamatos üzembe helyezés szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7af42-108">Step 2 - Enable container continuous deployment feature</span></span>

<span data-ttu-id="7af42-109">Engedélyezheti a folyamatos üzembe helyezés funkció használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és a következő parancs végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7af42-109">You can enable the continuous deployment feature using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing the following command</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

<span data-ttu-id="7af42-110">Az a  **[Azure-portálon](https://portal.azure.com/)**, kattintson a **App Service** lehetőséget a bal oldali a lap.</span><span class="sxs-lookup"><span data-stu-id="7af42-110">In the **[Azure portal](https://portal.azure.com/)**, click the **App Service** option on the left of the page.</span></span>

<span data-ttu-id="7af42-111">Kattintson a nevére, amely a folyamatos üzembe Docker Hub konfigurálni szeretné az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7af42-111">Click on the name of your app that you want to configure Docker Hub continuous deployment for.</span></span>

<span data-ttu-id="7af42-112">Az a **Alkalmazásbeállítások**, hozzáadhat egy alkalmazást nevű beállítása `DOCKER_ENABLE_CI` értékű `true`.</span><span class="sxs-lookup"><span data-stu-id="7af42-112">In the **App settings**, add an app setting called `DOCKER_ENABLE_CI` with the value `true`.</span></span>

![Helyezze be a Alkalmazásbeállítás képe](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a><span data-ttu-id="7af42-114">3. lépés - a Webhook URL-CÍMÉT előkészítése</span><span class="sxs-lookup"><span data-stu-id="7af42-114">Step 3 - Prepare Webhook URL</span></span>

<span data-ttu-id="7af42-115">Ezt úgy szerezheti be a Webhook URL-cím használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és a következő parancs végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7af42-115">You can obtain the Webhook URL using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing the following command</span></span>

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

<span data-ttu-id="7af42-116">A Webhook URL-címhez kell rendelkeznie a következő végpontot: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span><span class="sxs-lookup"><span data-stu-id="7af42-116">For the Webhook URL, you need to have the following endpoint: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span></span>

<span data-ttu-id="7af42-117">Ezt úgy szerezheti be a `publishingusername` és `publishingpwd` úgy, hogy letölti a webes alkalmazás közzététele a profil az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="7af42-117">You can obtain your `publishingusername` and `publishingpwd` by downloading the web app publish profile using the Azure portal.</span></span>

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a><span data-ttu-id="7af42-119">4. lépés – a webes hook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7af42-119">Step 4 - Add a web hook</span></span>

### <a name="azure-container-registry"></a><span data-ttu-id="7af42-120">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="7af42-120">Azure Container Registry</span></span>

<span data-ttu-id="7af42-121">A beállításjegyzék portálpanelén kattintson **Webhookok**, hozzon létre egy új webhook kattintva **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7af42-121">In your registry portal blade, click **Webhooks**, create a new webhook by clicking **Add**.</span></span> <span data-ttu-id="7af42-122">Az a **webhook létrehozása** panelen adjon a webhook nevét.</span><span class="sxs-lookup"><span data-stu-id="7af42-122">In the **Create webhook** blade, give your webhook a name.</span></span> <span data-ttu-id="7af42-123">A Webhook URI-hoz, meg kell adnia az URL-címet szerzett **3. lépés**</span><span class="sxs-lookup"><span data-stu-id="7af42-123">For the Webhook URI, you need to provide the URL obtained from **Step 3**</span></span>

<span data-ttu-id="7af42-124">Győződjön meg arról, mint a tárház, amely tartalmazza a tároló lemezkép hatókörének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="7af42-124">Make sure that you define the scope as the repo that contains your container image.</span></span>

![Helyezze be a webhook képe](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

<span data-ttu-id="7af42-126">A lemezkép frissítésekor a lekérdezi a web app frissül automatikusan a új lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="7af42-126">When the image gets updated, the web app get updated automatically with the new image.</span></span>

### <a name="docker-hub"></a><span data-ttu-id="7af42-127">Docker központ</span><span class="sxs-lookup"><span data-stu-id="7af42-127">Docker Hub</span></span>

<span data-ttu-id="7af42-128">A Docker Hub oldalon kattintson **Webhookok**, majd **A WEBHOOK létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7af42-128">In your Docker Hub page, click **Webhooks**, then **CREATE A WEBHOOK**.</span></span>

![Helyezze be a webhook 1 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

<span data-ttu-id="7af42-130">A Webhook URL-címhez, meg kell adnia az URL-címet szerzett **3. lépés**</span><span class="sxs-lookup"><span data-stu-id="7af42-130">For the Webhook URL, you need to provide the URL obtained from **Step 3**</span></span>

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

<span data-ttu-id="7af42-132">A lemezkép frissítésekor a lekérdezi a web app frissül automatikusan a új lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="7af42-132">When the image gets updated, the web app get updated automatically with the new image.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7af42-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7af42-133">Next steps</span></span>
* [<span data-ttu-id="7af42-134">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="7af42-134">What is Azure Web App on Linux?</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="7af42-135">Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="7af42-135">Azure Container Registry</span></span>](https://azure.microsoft.com/en-us/services/container-registry/)
* [<span data-ttu-id="7af42-136">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="7af42-136">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="7af42-137">.NET Core Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="7af42-137">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="7af42-138">Ruby Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="7af42-138">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="7af42-139">Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="7af42-139">How to use a custom Docker image for Azure Web App on Linux</span></span>](./app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="7af42-140">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="7af42-140">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md) 
* [<span data-ttu-id="7af42-141">Webalkalmazás az Azure CLI 2.0 verziót használja Linux kezelése</span><span class="sxs-lookup"><span data-stu-id="7af42-141">Manage Web App on Linux using Azure CLI 2.0</span></span>](./app-service-linux-cli.md)



