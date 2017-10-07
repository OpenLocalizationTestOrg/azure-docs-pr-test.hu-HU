---
title: "központi telepítés tooAzure App Service aaaContinuous |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable folyamatos üzembe helyezés tooAzure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a><span data-ttu-id="e5a5a-103">Folyamatos telepítés tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="e5a5a-103">Continuous Deployment tooAzure App Service</span></span>
<span data-ttu-id="e5a5a-104">Az oktatóanyag bemutatja, hogyan tooconfigure folyamatos üzembe helyezés munkafolyamat az a [Azure App Service] alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-104">This tutorial shows you how tooconfigure a continuous deployment workflow for your [Azure App Service] app.</span></span> <span data-ttu-id="e5a5a-105">App Service integrációja bitbucket szolgáltatásokhoz, a github webhelyen, és [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) lehetővé teszi, hogy egy folyamatos telepítési munkafolyamat, ahol a Azure kéri le a legújabb frissítéseket hello a projektből közzétett tooone ezen szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-105">App Service integration with BitBucket, GitHub, and [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) enables a continuous deployment workflow where Azure pulls in hello most recent updates from your project published tooone of these services.</span></span> <span data-ttu-id="e5a5a-106">A folyamatos üzembe helyezés jó megoldás lehet olyan projektek esetén, amelyeknél többszöri és gyakori közreműködői változtatást kell integrálni.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-106">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span>

<span data-ttu-id="e5a5a-107">toofind ki hogyan tooconfigure manuálisan egy felhőalapú-tárházat nem szerepel a folyamatos üzembe helyezés hello Azure Portal (például [GitLab](https://gitlab.com/)), lásd: [manuális lépéseket követve folyamatos üzembe helyezés beállítása](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span><span class="sxs-lookup"><span data-stu-id="e5a5a-107">toofind out how tooconfigure continuous deployment manually from a cloud repository not listed by hello Azure Portal (such as [GitLab](https://gitlab.com/)), see [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span></span>

## <span data-ttu-id="e5a5a-108"><a name="overview"></a>Folyamatos üzembe helyezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e5a5a-108"><a name="overview"></a>Enable continuous deployment</span></span>
<span data-ttu-id="e5a5a-109">tooenable folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="e5a5a-109">tooenable continuous deployment,</span></span>

1. <span data-ttu-id="e5a5a-110">Tegye közzé az alkalmazást tartalom toohello tárház, amely jelzi a folyamatos üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-110">Publish your app content toohello repository that will be used for continuous deployment.</span></span>  
    <span data-ttu-id="e5a5a-111">A közzétételi a projekt toothese szolgáltatások további információkért lásd: [hozzon létre egy tárház (GitHub)], [hozzon létre egy tárház (Bitbucketből)], és [Ismerkedés a VSTS].</span><span class="sxs-lookup"><span data-stu-id="e5a5a-111">For more information on publishing your project toothese services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS].</span></span>
2. <span data-ttu-id="e5a5a-112">Az alkalmazás menü panelen a hello [Azure-portálon], kattintson a **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek**.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-112">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="e5a5a-113">Kattintson a **forrás választása**, majd jelölje be hello telepítési forrása.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-113">Click **Choose Source**, then select hello deployment source.</span></span>  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > <span data-ttu-id="e5a5a-114">a VSTS fiókot, az App Service üzembe helyezéshez tooconfigure lásd a [oktatóanyag](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span><span class="sxs-lookup"><span data-stu-id="e5a5a-114">tooconfigure a VSTS account for App Service deployment please see this [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span></span>
   > 
   > 
3. <span data-ttu-id="e5a5a-115">Teljes hello engedélyezési munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-115">Complete hello authorization workflow.</span></span>
4. <span data-ttu-id="e5a5a-116">A hello **központi telepítés forrásának** panelen válassza ki a hello projekt és toodeploy a fiókirodai.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-116">In hello **Deployment source** blade, choose hello project and branch toodeploy from.</span></span> <span data-ttu-id="e5a5a-117">Ha végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-117">When you're done, click **OK**.</span></span>
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > <span data-ttu-id="e5a5a-118">Ha engedélyezve van a folyamatos üzembe helyezés a GitHubbal vagy a BitBuckettel, mind a nyilvános, mind a magánjellegű projektek megjelennek.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-118">When enabling continuous deployment with GitHub or BitBucket, both public and private projects will be displayed.</span></span>
   > 
   > 
   
    <span data-ttu-id="e5a5a-119">App Service hello kijelölt tárházzal való társítást hoz létre, lekéri a megadott fiókirodai hello hello fájlokat, és a tárház beállítása az App Service alkalmazáshoz készült másolat fenn.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-119">App Service creates an association with hello selected repository, pulls in hello files from hello specified branch, and maintains a clone of your repository for your App Service app.</span></span> <span data-ttu-id="e5a5a-120">VSTS folyamatos üzembe helyezés az Azure-portálon hello konfigurálásakor hello integrációs használ hello App Service [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki), mely már automatizálja az buildelés és üzembe helyezés feladatok minden `git push`.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-120">When you configure VSTS continuous deployment from hello Azure portal, hello integration uses hello App Service [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki), which already automates build and deployment tasks with every `git push`.</span></span> <span data-ttu-id="e5a5a-121">Nem kell tooseparately VSTS a folyamatos üzembe helyezést beállítani.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-121">You do not need tooseparately set up continuous deployment in VSTS.</span></span> <span data-ttu-id="e5a5a-122">Ez a folyamat befejezése után hello **központi telepítési beállítások** panelen megjelenik egy aktív központi telepítéssel, amely jelzi a telepítés sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-122">After this process completes, hello **Deployment options** app blade will show an active deployment that indicates deployment has succeeded.</span></span>
5. <span data-ttu-id="e5a5a-123">tooverify hello alkalmazás telepítése sikeres volt, kattintson a hello **URL-cím** hello app panel az Azure-portálon hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-123">tooverify hello app is successfully deployed, click hello **URL** at hello top of hello app's blade in hello Azure portal.</span></span>
6. <span data-ttu-id="e5a5a-124">amely a folyamatos üzembe helyezés folyamatban van az Ön által választott hello tárházból tooverify leküldéses módosítása toohello tára.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-124">tooverify that continuous deployment is occurring from hello repository of your choice, push a change toohello repository.</span></span> <span data-ttu-id="e5a5a-125">Az alkalmazás hamarosan hello leküldéses toohello tárház befejeződése után frissítenie kell tooreflect hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-125">Your app should update tooreflect hello changes shortly after hello push toohello repository completes.</span></span> <span data-ttu-id="e5a5a-126">Ellenőrizheti, hogy rendelkezik-e lekért hello frissítés a hello **központi telepítési beállítások** az App.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-126">You can verify that it has pulled in hello update in hello **Deployment options** blade of your app.</span></span>

## <span data-ttu-id="e5a5a-127"><a name="VSsolution"></a>Egy Visual Studio-megoldás folyamatos üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="e5a5a-127"><a name="VSsolution"></a>Continuous deployment of a Visual Studio solution</span></span>
<span data-ttu-id="e5a5a-128">A Visual Studio megoldás tooAzure App Service kérdez le csak egyszerű módon kérdez le egy egyszerű index.html fájl.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-128">Pushing a Visual Studio solution tooAzure App Service is just as easy as pushing a simple index.html file.</span></span> <span data-ttu-id="e5a5a-129">App Service-telepítési folyamat hello egyszerűsítheti az összes hello adatait, többek között a NuGet-függőségek visszaállítása és felépítése hello bináris alkalmazásfájlokat.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-129">hello App Service deployment process streamlines all hello details, including restoring NuGet dependencies and building hello application binaries.</span></span> <span data-ttu-id="e5a5a-130">Kövesse a hello forrás vezérlő bevált gyakorlatokat kód csak a Git-tárház fenntartásának, és lehetővé teszik az alkalmazás szolgáltatástelepítés hello rest gondoskodunk.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-130">You can follow hello source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of hello rest.</span></span>

<span data-ttu-id="e5a5a-131">hello lépések a Visual Studio megoldás tooApp szolgáltatás tárházat hello ugyanaz, mint hello [előző szakaszban](#overview), feltéve, hogy konfigurálja a megoldás és a tárház az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e5a5a-131">hello steps for pushing your Visual Studio solution tooApp Service are hello same as in hello [previous section](#overview), provided that you configure your solution and repository as follows:</span></span>

* <span data-ttu-id="e5a5a-132">Hello Visual Studio forrás vezérlő beállítás toogenerate használja egy `.gitignore` fájlt például az alábbi képen hello, vagy manuálisan adja hozzá a `.gitignore` fájlt az adattár gyökérkönyvtárában, a tartalom hasonló toothis [.gitignore minta](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="e5a5a-132">Use hello Visual Studio source control option toogenerate a `.gitignore` file such as hello image below or manually add a `.gitignore` file in your repository root with content similar toothis [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* <span data-ttu-id="e5a5a-133">Vegyen fel hello teljes megoldás directory fa tooyour tárházat, az adattár gyökérkönyvtárában hello hello .sln-fájlt.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-133">Add hello entire solution's directory tree tooyour repository, with hello .sln file in hello repository root.</span></span>

<span data-ttu-id="e5a5a-134">Miután leírtak a tárház beállítása, és az alkalmazás konfigurálva az Azure-ban egy hello online Git adattárak folyamatos közzétételt, az ASP.NET-alkalmazás helyileg a Visual Studio fejlesztése, és folyamatosan egyszerűen, az a kód telepítésére a módosítások tooyour online Git-tárház kérdez le.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-134">Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of hello online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes tooyour online Git repository.</span></span>

## <span data-ttu-id="e5a5a-135"><a name="disableCD"></a>Folyamatos üzembe helyezés letiltása</span><span class="sxs-lookup"><span data-stu-id="e5a5a-135"><a name="disableCD"></a>Disable continuous deployment</span></span>
<span data-ttu-id="e5a5a-136">toodisable folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="e5a5a-136">toodisable continuous deployment,</span></span>

1. <span data-ttu-id="e5a5a-137">Az alkalmazás menü panelen a hello [Azure-portálon], kattintson a **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek**.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-137">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="e5a5a-138">Kattintson a **Disconnect** a hello **központi telepítési beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-138">Then click **Disconnect** in hello **Deployment options** blade.</span></span>
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. <span data-ttu-id="e5a5a-139">Fogadás után **Igen** toohello megerősítő üzenetet, térjen vissza a tooyour alkalmazás paneljének szabadon kattintson **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek** Ha azt szeretné, hogy fel más forrásból származó közzététel tooset.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-139">After answering **Yes** toohello confirmation message, you can return tooyour app's blade and click **APP DEPLOYMENT > Deployment options** if you would like tooset up publishing from another source.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5a5a-140">További források</span><span class="sxs-lookup"><span data-stu-id="e5a5a-140">Additional Resources</span></span>
* [<span data-ttu-id="e5a5a-141">Hogyan tooinvestigate közös érintő problémákat folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="e5a5a-141">How tooinvestigate common issues with continuous deployment</span></span>](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* <span data-ttu-id="e5a5a-142">[Hogyan toouse az Azure PowerShell]</span><span class="sxs-lookup"><span data-stu-id="e5a5a-142">[How toouse PowerShell for Azure]</span></span>
* <span data-ttu-id="e5a5a-143">[Hogyan toouse hello for Mac és Linux Azure parancssori eszközei]</span><span class="sxs-lookup"><span data-stu-id="e5a5a-143">[How toouse hello Azure Command-Line Tools for Mac and Linux]</span></span>
* <span data-ttu-id="e5a5a-144">[A Git dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="e5a5a-144">[Git documentation]</span></span>
* [<span data-ttu-id="e5a5a-145">A Kudu projekt</span><span class="sxs-lookup"><span data-stu-id="e5a5a-145">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="e5a5a-146">Használja az Azure tooautomatically egy CI/CD adatcsatorna toodeploy az ASP.NET 4 alkalmazás készítése</span><span class="sxs-lookup"><span data-stu-id="e5a5a-146">Use Azure tooautomatically generate a CI/CD pipeline toodeploy an ASP.NET 4 app</span></span>](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> <span data-ttu-id="e5a5a-147">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-147">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e5a5a-148">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="e5a5a-148">No credit cards required; no commitments.</span></span>
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[Azure-portálon]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Hogyan toouse az Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Hogyan toouse hello for Mac és Linux Azure parancssori eszközei]:../cli-install-nodejs.md
[A Git dokumentációja]: http://git-scm.com/documentation

[hozzon létre egy tárház (GitHub)]: https://help.github.com/articles/create-a-repo
[hozzon létre egy tárház (Bitbucketből)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Ismerkedés a VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
