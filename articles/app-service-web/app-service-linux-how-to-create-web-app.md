---
title: "Hozzon létre egy Azure Linux rendszeren futó webalkalmazás |} Microsoft Docs"
description: "Webes alkalmazás létrehozási munkafolyamat Linux Azure webalkalmazás számára."
keywords: "az Azure app service, webalkalmazás, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="2e165-104">Hozzon létre egy Azure Linux rendszeren futó webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2e165-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a><span data-ttu-id="2e165-105">A webalkalmazás létrehozása az Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="2e165-105">Use the Azure portal to create your web app</span></span>
<span data-ttu-id="2e165-106">Megkezdheti a webalkalmazás létrehozása a Linux rendszeren a [Azure-portálon](https://portal.azure.com) a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2e165-106">You can start creating your web app on Linux from the [Azure portal](https://portal.azure.com) as shown in the following image:</span></span>

![Egy webalkalmazás létrehozása az Azure portálon][1]

<span data-ttu-id="2e165-108">Ezt követően a **létrehozás panel** nyitja meg a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2e165-108">Next, the **Create blade** opens as shown in the following image:</span></span>

![A létrehozás panel][2]

1. <span data-ttu-id="2e165-110">Adjon meg egy nevet a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2e165-110">Give your web app a name.</span></span>
2. <span data-ttu-id="2e165-111">Válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2e165-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="2e165-112">(Lásd az elérhető régiók a [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="2e165-112">(See available regions in the [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="2e165-113">Válassza ki a meglévő Azure App Service-csomagot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2e165-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="2e165-114">(Lásd az App Service-csomag jegyzetének a [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="2e165-114">(See App Service plan notes in the [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="2e165-115">Válassza ki a használni kívánt alkalmazáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="2e165-115">Choose the application stack that you intend to use.</span></span> <span data-ttu-id="2e165-116">Választhat a különböző verzióiban Node.js, PHP, a .net Core és a Ruby.</span><span class="sxs-lookup"><span data-stu-id="2e165-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="2e165-117">Miután létrehozta az alkalmazást, módosíthatja az alkalmazáscsoportokat az alkalmazásbeállítások az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2e165-117">Once you have created the app, you can change the application stack from the application settings as shown in the following image:</span></span>

![Alkalmazásbeállítások][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="2e165-119">A webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="2e165-119">Deploy your web app</span></span>
<span data-ttu-id="2e165-120">Kiválasztása **központi telepítési beállítások** a felügyeleti portál felajánlja a lehetőséget az alkalmazás központi telepítéséhez a helyi Git- vagy GitHub-tárház használandó.</span><span class="sxs-lookup"><span data-stu-id="2e165-120">Choosing **deployment options** from the management portal gives you the option to use local Git or GitHub repository to deploy your application.</span></span> <span data-ttu-id="2e165-121">A további utasításokat hasonlóak a nem Linux webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2e165-121">The rest of the instructions are similar to those for a non-Linux web app.</span></span> <span data-ttu-id="2e165-122">Az utasítások a [helyi Git-telepítésének](app-service-deploy-local-git.md) vagy [folyamatos üzembe helyezés](app-service-continuous-deployment.md) segítségével telepítse az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2e165-122">You can follow the instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) to deploy your app.</span></span>

<span data-ttu-id="2e165-123">FTP segítségével töltse fel az alkalmazást a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="2e165-123">You can also use FTP to upload your application to your site.</span></span> <span data-ttu-id="2e165-124">Letölthető az FTP-végpont a webalkalmazás a diagnosztikai naplók szakaszban a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2e165-124">You can get the FTP endpoint for your web app from the diagnostics logs section as shown in the following image:</span></span>

![Diagnosztikai naplók][4]

## <a name="next-steps"></a><span data-ttu-id="2e165-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e165-126">Next steps</span></span>
* [<span data-ttu-id="2e165-127">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="2e165-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="2e165-128">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="2e165-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="2e165-129">Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2e165-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="2e165-130">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2e165-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
