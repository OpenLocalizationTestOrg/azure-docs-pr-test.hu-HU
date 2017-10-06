---
title: "aaaDeploy az alkalmazás tooAzure App Service FTP/S használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy az alkalmazás tooAzure App Service segítségével FTP- vagy FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a><span data-ttu-id="db57a-103">Az alkalmazás tooAzure használatával az FTP/S App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="db57a-103">Deploy your app tooAzure App Service using FTP/S</span></span>

<span data-ttu-id="db57a-104">Ez a cikk bemutatja, hogyan toouse FTP vagy FTPS toodeploy a webalkalmazást, mobil-háttéralkalmazás vagy API-alkalmazás túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="db57a-104">This article shows you how toouse FTP or FTPS toodeploy your web app, mobile app backend, or API app too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="db57a-105">hello FTP/S végpont az alkalmazás már aktív.</span><span class="sxs-lookup"><span data-stu-id="db57a-105">hello FTP/S endpoint for your app is already active.</span></span> <span data-ttu-id="db57a-106">Szükséges tooenable FTP/mp telepítése nem igényel konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="db57a-106">No configuration is necessary tooenable FTP/S deployment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db57a-107">Folyamatosan jegyében lépéseket tooimprove Microsoft Azure Platform biztonsági.</span><span class="sxs-lookup"><span data-stu-id="db57a-107">We are continuously taking steps tooimprove Microsoft Azure Platform security.</span></span> <span data-ttu-id="db57a-108">A folyamatban lévő elérhető részeként webalkalmazások frissítés tervezett Németország központi és Németország szerepel.</span><span class="sxs-lookup"><span data-stu-id="db57a-108">As part of this ongoing effort an upgrade of Web Applications is planned for Germany Central and Germany Northeast regions.</span></span> <span data-ttu-id="db57a-109">Ennek során Web Apps letiltása telepítések hello egyszerű szöveges FTP protokoll használatát fog lehet.</span><span class="sxs-lookup"><span data-stu-id="db57a-109">During this Web Apps will be disabling hello use of plain text FTP protocol for deployments.</span></span> <span data-ttu-id="db57a-110">A javaslat tooour ügyfeleink tooswitch tooFTPS telepítésekhez.</span><span class="sxs-lookup"><span data-stu-id="db57a-110">Our recommendation tooour customers is tooswitch tooFTPS for deployments.</span></span> <span data-ttu-id="db57a-111">Ez a frissítés, amely a 9/5 tervezett során nem minden megszűnésének tooyour szolgáltatás várhatóan.</span><span class="sxs-lookup"><span data-stu-id="db57a-111">We do not expect any disruption tooyour service during this upgrade which is planned for 9/5.</span></span> <span data-ttu-id="db57a-112">Nagyra értékeljük ebből a törekvésből támogatja.</span><span class="sxs-lookup"><span data-stu-id="db57a-112">We appreciate you support in this effort.</span></span>

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a><span data-ttu-id="db57a-113">1. lépés: Az üzembe helyezési hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="db57a-113">Step 1: Set deployment credentials</span></span>

<span data-ttu-id="db57a-114">tooaccess hello FTP-kiszolgáló beállítása az alkalmazáshoz, először üzembe helyezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="db57a-114">tooaccess hello FTP server for your app, you first need deployment credentials.</span></span> 

<span data-ttu-id="db57a-115">tooset vagy visszaállítása az üzembe helyezési hitelesítő adatokat, lásd: [Azure App Service üzembe helyezési hitelesítő adatok](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="db57a-115">tooset or reset your deployment credentials, see [Azure App Service Deployment Credentials](app-service-deployment-credentials.md).</span></span> <span data-ttu-id="db57a-116">Ez az oktatóanyag bemutatja, hello felhasználói szintű hitelesítő adatokat használjanak.</span><span class="sxs-lookup"><span data-stu-id="db57a-116">This tutorial demonstrates hello use of user-level credentials.</span></span>

## <a name="step-2-get-ftp-connection-information"></a><span data-ttu-id="db57a-117">2. lépés: FTP-kiszolgáló kapcsolati adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="db57a-117">Step 2: Get FTP connection information</span></span>

1. <span data-ttu-id="db57a-118">A hello [Azure-portálon](https://portal.azure.com), nyissa meg az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="db57a-118">In hello [Azure portal](https://portal.azure.com), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="db57a-119">Válassza ki **áttekintése** hello bal oldali menüben, majd jegyezze fel hello értékeinek **FTP vagy üzembe helyező felhasználó**, **FTP-állomás neve**, és **állomásnév FTPS**.</span><span class="sxs-lookup"><span data-stu-id="db57a-119">Select **Overview** in hello left menu, then note hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span> 

    ![FTP-kapcsolat információi](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > <span data-ttu-id="db57a-121">Hello **FTP vagy üzembe helyező felhasználó** felhasználói érték által megjelenített hello hello alkalmazás neve például rendelés tooprovide megfelelő környezetben hello FTP-kiszolgáló Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="db57a-121">hello **FTP/Deployment User** user value as displayed by hello Azure Portal including hello app name in order tooprovide proper context for hello FTP server.</span></span>
    > <span data-ttu-id="db57a-122">Található hello ugyanazokat az információkat, amikor kiválaszt **tulajdonságok** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="db57a-122">You can find hello same information when you select **Properties** in hello left menu.</span></span> 
    >
    > <span data-ttu-id="db57a-123">Emellett hello telepítési jelszó soha nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="db57a-123">Also, hello deployment password is never shown.</span></span> <span data-ttu-id="db57a-124">Ha a központi telepítés jelszavát, lépjen vissza túl[1. lépés](#step1) és a központi telepítés jelszó.</span><span class="sxs-lookup"><span data-stu-id="db57a-124">If you forget your deployment password, go back too[step 1](#step1) and reset your deployment password.</span></span>
    >
    >

## <a name="step-3-deploy-files-tooazure"></a><span data-ttu-id="db57a-125">3. lépés: A fájlok tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="db57a-125">Step 3: Deploy files tooAzure</span></span>

1. <span data-ttu-id="db57a-126">Az FTP-ügyfél ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)stb), tooconnect tooyour app összegyűjtött információkat felhasználva hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="db57a-126">From your FTP client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), etc), use hello connection information you gathered tooconnect tooyour app.</span></span>
3. <span data-ttu-id="db57a-127">Másolja a fájlokat és a megfelelő könyvtár struktúra toohello [ **/hely/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) az Azure-ban (vagy hello **/hely/wwwroot/App_Data/feladatok/** a könyvtár Webjobs-feladatok).</span><span class="sxs-lookup"><span data-stu-id="db57a-127">Copy your files and their respective directory structure toohello [**/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (or hello **/site/wwwroot/App_Data/Jobs/** directory for WebJobs).</span></span>
4. <span data-ttu-id="db57a-128">Tallózás tooyour alkalmazás URL-cím tooverify hello alkalmazás megfelelően fut.</span><span class="sxs-lookup"><span data-stu-id="db57a-128">Browse tooyour app's URL tooverify hello app is running properly.</span></span> 

> [!NOTE] 
> <span data-ttu-id="db57a-129">Eltérően [Git-alapú telepítések](app-service-deploy-local-git.md), FTP-telepítés nem támogatja a központi telepítés automatizálása a következő hello:</span><span class="sxs-lookup"><span data-stu-id="db57a-129">Unlike [Git-based deployments](app-service-deploy-local-git.md), FTP deployment doesn't support hello following deployment automations:</span></span> 
>
> - <span data-ttu-id="db57a-130">a függőségi visszaállítási (például a NuGet, NPM, PIP és szerkesztő automatizálása)</span><span class="sxs-lookup"><span data-stu-id="db57a-130">dependency restore (such as NuGet, NPM, PIP, and Composer automations)</span></span>
> - <span data-ttu-id="db57a-131">a .NET bináris fájl fordítás</span><span class="sxs-lookup"><span data-stu-id="db57a-131">compilation of .NET binaries</span></span>
> - <span data-ttu-id="db57a-132">a Web.config fájl. generációs (itt egy [Node.js példa](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span><span class="sxs-lookup"><span data-stu-id="db57a-132">generation of web.config (here is a [Node.js example](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span></span>
> 
> <span data-ttu-id="db57a-133">Ön kell visszaállítani, felépítéséhez, és hozza létre ezeket manuálisan a helyi számítógépen a szükséges fájlokat és együtt az alkalmazás központi telepítésére.</span><span class="sxs-lookup"><span data-stu-id="db57a-133">You must restore, build, and generate these necessary files manually on your local machine and deploy them together with your app.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="db57a-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db57a-134">Next steps</span></span>

<span data-ttu-id="db57a-135">Összetettebb központi telepítési forgatókönyve esetén próbálja [központi telepítése a Git tooAzure](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="db57a-135">For more advanced deployment scenarios, try [deploying tooAzure with Git](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="db57a-136">Git-alapú üzembe helyezési tooAzure lehetővé teszi a verziókövetés, a csomag visszaállítás, az MSBuild és több.</span><span class="sxs-lookup"><span data-stu-id="db57a-136">Git-based deployment tooAzure enables version control, package restore, MSBuild, and more.</span></span>

## <a name="more-resources"></a><span data-ttu-id="db57a-137">További források</span><span class="sxs-lookup"><span data-stu-id="db57a-137">More Resources</span></span>

* <span data-ttu-id="db57a-138">[PHP-MySQL-webalkalmazás létrehozása és központi telepítése FTP használatával](web-sites-php-mysql-deploy-use-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="db57a-138">[Create a PHP-MySQL web app and deploy using FTP](web-sites-php-mysql-deploy-use-ftp.md).</span></span>
* [<span data-ttu-id="db57a-139">Az Azure App Service üzembe helyezési hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="db57a-139">Azure App Service Deployment Credentials</span></span>](app-service-deploy-ftp.md)
