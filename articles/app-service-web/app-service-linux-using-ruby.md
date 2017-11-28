---
title: az Azure App Service Web Apps Linux Ruby aaaUsing |} Microsoft Docs
description: "Az Azure App Service webalkalmazásba Linux Ruby használatát."
keywords: "az Azure app service, webalkalmazás, gyakran ismételt kérdések, linux, oss, ruby"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 45692cb3bf1da9ff65b9466055029bfaef8b7d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a><span data-ttu-id="f6be2-104">Ruby Linux Web App használatával</span><span class="sxs-lookup"><span data-stu-id="f6be2-104">Using Ruby in Web App on Linux</span></span> #

<span data-ttu-id="f6be2-105">Hello legújabb frissítés tooour háttérrendszerrel hogy Ruby v.2.3 támogatása vezette be.</span><span class="sxs-lookup"><span data-stu-id="f6be2-105">With hello latest update tooour backend, we introduced support for Ruby v.2.3.</span></span> <span data-ttu-id="f6be2-106">Beállítás hello konfigurációja a Linux-webalkalmazás hello alkalmazáscsoportokat bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="f6be2-106">By setting hello configuration of your Linux web app, you can change hello application stack.</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="f6be2-107">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="f6be2-107">Using hello Azure portal</span></span> ##

<span data-ttu-id="f6be2-108">Új menüjéből hello hello [Azure-portálon](https://portal.azure.com), választhat toocreate Linux webalkalmazás hello Web + mobil option hello kép a következő ábrán:</span><span class="sxs-lookup"><span data-stu-id="f6be2-108">From hello new menu in hello [Azure portal](https://portal.azure.com), you can choose toocreate a Web App on Linux from hello Web + Mobile option as shown in hello following image:</span></span>

![Egy webalkalmazás létrehozása a hello Azure-portálon][1]

<span data-ttu-id="f6be2-110">A következő hello **létrehozás panel** megnyílik, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="f6be2-110">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![hello létrehozás panel][2]

1. <span data-ttu-id="f6be2-112">Adjon meg egy nevet a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f6be2-112">Give your web app a name.</span></span>
2. <span data-ttu-id="f6be2-113">Válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="f6be2-113">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="f6be2-114">(Lásd a hello elérhető régiók [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="f6be2-114">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="f6be2-115">Válassza ki a meglévő Azure App Service-csomagot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="f6be2-115">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="f6be2-116">(Lásd az App Service csomag a hello [korlátozások szakasz](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="f6be2-116">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="f6be2-117">Válasszon hello Ruby hello beépített futásidejű verem.</span><span class="sxs-lookup"><span data-stu-id="f6be2-117">Choose hello Ruby from hello Built-in Runtime stacks.</span></span>

<span data-ttu-id="f6be2-118">A Ruby webalkalmazás lekérdezi a létrehozása után tooit Git vagy FTP segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="f6be2-118">After your Ruby web app gets created, you can deploy tooit using Git or FTP.</span></span>

<span data-ttu-id="f6be2-119">toolearn további információt a Ruby alkalmazások létrehozása ellenőrizze hello [get lépésekről szóló útmutatót](app-service-linux-ruby-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="f6be2-119">toolearn more about creating a Ruby app, check hello [get started guide](app-service-linux-ruby-get-started.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6be2-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6be2-120">Next steps</span></span>
* [<span data-ttu-id="f6be2-121">Mi az a webes alkalmazás Linux?</span><span class="sxs-lookup"><span data-stu-id="f6be2-121">What is Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="f6be2-122">Helyi Git-telepítésének tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="f6be2-122">Local Git Deployment tooAzure App Service</span></span>](app-service-deploy-local-git.md)
* [<span data-ttu-id="f6be2-123">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="f6be2-123">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="f6be2-124">Ruby-alkalmazás létrehozása az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="f6be2-124">Create a Ruby App with Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png