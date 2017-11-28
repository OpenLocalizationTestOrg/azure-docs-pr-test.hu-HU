---
title: "Linux rendszerű aaaCreate egy Azure webalkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="482f4-104">Hozzon létre egy Azure Linux rendszeren futó webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="482f4-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a><span data-ttu-id="482f4-105">Használja az Azure portál toocreate hello a webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="482f4-105">Use hello Azure portal toocreate your web app</span></span>
<span data-ttu-id="482f4-106">Megkezdheti a webalkalmazás létrehozása Linux hello [Azure-portálon](https://portal.azure.com) látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="482f4-106">You can start creating your web app on Linux from hello [Azure portal](https://portal.azure.com) as shown in hello following image:</span></span>

![Egy webalkalmazás létrehozása a hello Azure-portálon][1]

<span data-ttu-id="482f4-108">A következő hello **létrehozás panel** megnyílik, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="482f4-108">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![hello létrehozás panel][2]

1. <span data-ttu-id="482f4-110">Adjon meg egy nevet a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="482f4-110">Give your web app a name.</span></span>
2. <span data-ttu-id="482f4-111">Válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="482f4-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="482f4-112">(Lásd a hello elérhető régiók [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="482f4-112">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="482f4-113">Válassza ki a meglévő Azure App Service-csomagot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="482f4-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="482f4-114">(Lásd az App Service csomag a hello [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="482f4-114">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="482f4-115">Válassza ki a hello alkalmazás, hogy szeretné-e toouse verem.</span><span class="sxs-lookup"><span data-stu-id="482f4-115">Choose hello application stack that you intend toouse.</span></span> <span data-ttu-id="482f4-116">Választhat a különböző verzióiban Node.js, PHP, a .net Core és a Ruby.</span><span class="sxs-lookup"><span data-stu-id="482f4-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="482f4-117">Hello alkalmazás létrehozását követően módosíthatja hello alkalmazáscsoportokat hello Alkalmazásbeállítások látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="482f4-117">Once you have created hello app, you can change hello application stack from hello application settings as shown in hello following image:</span></span>

![Alkalmazásbeállítások][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="482f4-119">A webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="482f4-119">Deploy your web app</span></span>
<span data-ttu-id="482f4-120">Kiválasztása **központi telepítési beállítások** hello felügyeleti portál által biztosított az Ön hello beállítás toouse helyi Git- vagy GitHub tárház toodeploy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="482f4-120">Choosing **deployment options** from hello management portal gives you hello option toouse local Git or GitHub repository toodeploy your application.</span></span> <span data-ttu-id="482f4-121">hello részeinek hello utasításokat a nem Linux webalkalmazás hasonló toothose.</span><span class="sxs-lookup"><span data-stu-id="482f4-121">hello rest of hello instructions are similar toothose for a non-Linux web app.</span></span> <span data-ttu-id="482f4-122">Hajtsa végre az hello utasításait [helyi Git-telepítésének](app-service-deploy-local-git.md) vagy [folyamatos üzembe helyezés](app-service-continuous-deployment.md) toodeploy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="482f4-122">You can follow hello instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) toodeploy your app.</span></span>

<span data-ttu-id="482f4-123">FTP-tooupload az alkalmazás tooyour hely is használja.</span><span class="sxs-lookup"><span data-stu-id="482f4-123">You can also use FTP tooupload your application tooyour site.</span></span> <span data-ttu-id="482f4-124">Letölthető hello FTP végpont a webalkalmazás hello diagnosztikai naplók szakasz látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="482f4-124">You can get hello FTP endpoint for your web app from hello diagnostics logs section as shown in hello following image:</span></span>

![Diagnosztikai naplók][4]

## <a name="next-steps"></a><span data-ttu-id="482f4-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="482f4-126">Next steps</span></span>
* [<span data-ttu-id="482f4-127">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="482f4-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="482f4-128">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="482f4-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="482f4-129">Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="482f4-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="482f4-130">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="482f4-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
