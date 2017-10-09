---
title: "a Git és az Azure-ban a Visual Studio Team Services aaaContinuous kézbesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure a Visual Studio Team Services team projects toouse Git tooautomatically és toohello webalkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások üzembe."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="ddcdc-103">Folyamatos kézbesítési tooAzure Visual Studio Team Services és a Git használatával</span><span class="sxs-lookup"><span data-stu-id="ddcdc-103">Continuous delivery tooAzure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="ddcdc-104">Használja a Visual Studio Team Services team projects toohost Git-tárház a forráskódot, és automatikusan build és tooAzure webes alkalmazások telepítését vagy felhőalapú szolgáltatások, amikor egy véglegesítési toohello tárház leküldéses.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-104">You can use Visual Studio Team Services team projects toohost a Git repository for your source code, and automatically build and deploy tooAzure web apps or cloud services whenever you push a commit toohello repository.</span></span>

<span data-ttu-id="ddcdc-105">Visual Studio 2013 és Azure SDK telepítése hello lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-105">You'll need Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="ddcdc-106">Ha még nem rendelkezik a Visual Studio 2013, töltse le hello kiválasztásával **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Telepítse az Azure SDK hello [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-106">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="ddcdc-107">Ez az oktatóanyag kell egy Visual Studio Team Services-fiók toocomplete: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-107">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="ddcdc-108">egy felhőalapú szolgáltatás tooautomatically mentése tooset build és tooAzure telepíteni a Visual Studio Team Services, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-108">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="ddcdc-109">1: a Git-tárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="ddcdc-109">1: Create a Git repository</span></span>
1. <span data-ttu-id="ddcdc-110">Ha még nem rendelkezik a Visual Studio Team Services-fiók, akkor kaphat egy [Itt](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-110">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="ddcdc-111">A csapatprojekt létrehozásakor a forrásrendszerben vezérlő, válassza a Git.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-111">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="ddcdc-112">Hajtsa végre a hello utasításokat tooconnect Visual Studio tooyour csapatprojektben.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-112">Follow hello instructions tooconnect Visual Studio tooyour team project.</span></span>
2. <span data-ttu-id="ddcdc-113">A **Team Explorer**, válassza ki a hello **klónozza a tárházat** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-113">In **Team Explorer**, choose hello **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="ddcdc-114">Adja meg a helyi példány hello hello helyét, és válassza a hello **Klónozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-114">Specify hello location of hello local copy and then choose hello **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a><span data-ttu-id="ddcdc-115">2: projekt létrehozása és véglegesítheti toohello tárház</span><span class="sxs-lookup"><span data-stu-id="ddcdc-115">2: Create a project and commit it toohello repository</span></span>
1. <span data-ttu-id="ddcdc-116">A **Team Explorer**, a hello **megoldások** területen válassza ki a hello **új** toocreate egy új projektet hello helyi tárházban található hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-116">In **Team Explorer**, in hello **Solutions** section, choose hello **New** link toocreate a new project in hello local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="ddcdc-117">Webes alkalmazást telepít, vagy a bemutatóban szereplő lépések egy felhőalapú szolgáltatás (Azure-alkalmazás) a következő hello által.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-117">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span> <span data-ttu-id="ddcdc-118">Hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-118">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="ddcdc-119">Győződjön meg arról, hogy hello projekt célok hello .NET-keretrendszer 4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-119">Make sure that hello project targets hello .NET Framework 4 or later.</span></span> <span data-ttu-id="ddcdc-120">Egy felhőszolgáltatás-projekt létrehozásakor, adjon hozzá egy ASP.NET MVC webes és feldolgozói szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-120">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="ddcdc-121">Ha azt szeretné, hogy a webes alkalmazás toocreate, válassza ki azt a hello **ASP.NET Web Application** a project, és válassza a **MVC**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-121">If you want toocreate a web app, choose hello **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="ddcdc-122">Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="ddcdc-123">Nyissa meg a helyi menü hello hello megoldás, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-123">Open hello shortcut menu for hello solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="ddcdc-124">Ha hello első alkalommal a Visual Studio Team Services már használta a Git, szüksége lesz tooprovide néhány információt tooidentify saját kezűleg a Gitben.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-124">If this is hello first time you've used Git in Visual Studio Team Services, you'll need tooprovide some information tooidentify yourself in Git.</span></span> <span data-ttu-id="ddcdc-125">A hello **függőben lévő módosítások** területén **Team Explorer**, adja meg a felhasználónevet és e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-125">In hello **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="ddcdc-126">Írjon megjegyzést hello véglegesítési, és válassza a hello **véglegesítési** gombra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-126">Enter a comment for hello commit and then choose hello **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="ddcdc-127">Megjegyzés: hello beállítások tooinclude, vagy adott módosítások kizárása a verziókezelőbe mentésekor.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-127">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="ddcdc-128">Ha hello változik meg akarja ki vannak zárva, válassza a **tartalmazza az összes**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-128">If hello changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="ddcdc-129">Megismerte a most már véglegesített hello hello tárház helyi példányának módosításait.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-129">You've now committed hello changes in your local copy of hello repository.</span></span> <span data-ttu-id="ddcdc-130">Ezután szinkronizálhatja a módosításokat, hello kiszolgálóval hello kiválasztásával **szinkronizálási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-130">Next, sync those changes with hello server by choosing hello **Sync** link.</span></span>

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="ddcdc-131">3: hello projekt tooAzure csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ddcdc-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="ddcdc-132">Most, hogy az egyes forráskód azt a Visual Studio Team Services Git-tárház, tooconnect készen áll a git-tárház tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-132">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready tooconnect your git repository tooAzure.</span></span>  <span data-ttu-id="ddcdc-133">A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat hello + balra hello le, majd válassza a ikonra kiválasztásával **Felhőszolgáltatás** vagy **webalkalmazás**, majd **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello + icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="ddcdc-134">A felhőszolgáltatások, válassza ki a hello **Visual Studio Team Services való közzététel beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-134">For cloud services, choose hello **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="ddcdc-135">A web Apps, válassza ki a hello **verziókövetésből üzembe helyezés beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-135">For web apps, choose hello **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="ddcdc-136">Hello varázslóban, írja be a Visual Studio Team Services-fiók nevét hello hello szövegmezőben, és válassza a hello **engedélyezik most** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-136">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and choose hello **Authorize Now** link.</span></span> <span data-ttu-id="ddcdc-137">Előfordulhat, hogy a toosign kéri.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-137">You might be asked toosign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="ddcdc-138">A hello **kapcsolatkérelem** előugró párbeszédpanelen válasszon **elfogadás** tooauthorize a csapat projektre a Visual Studio Team Services, Azure tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-138">In hello **Connection Request** pop-up dialog, choose **Accept** tooauthorize Azure tooconfigure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="ddcdc-139">Után engedélyezési sikeres, megjelenik egy legördülő listát, amely tartalmazza a Visual Studio Team Services csapatprojektek.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-139">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="ddcdc-140">Válassza ki a csapatprojekt hello előző lépésekben létrehozott hello nevét, és válassza a hello varázsló pipa gombra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-140">Select hello name of team project that you created in hello previous steps, and choose hello wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="ddcdc-141">hello legközelebb leküldéses egy véglegesítési tooyour tárházat, a Visual Studio Team Services elkészíti és központi telepítése a projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-141">hello next time you push a commit tooyour repository, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="ddcdc-142">4: egy újjáépítését indítja, és telepítse újra a projekthez</span><span class="sxs-lookup"><span data-stu-id="ddcdc-142">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="ddcdc-143">A Visual Studióban nyissa meg a fájlt, és módosítsa úgy.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-143">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="ddcdc-144">Például a hello fájl módosítása `_Layout.cshtml` hello nézetek alapján\\egy MVC webes szerepkör megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-144">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="ddcdc-145">Hello előláb szövegét hello hely szerkesztése, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-145">Edit hello footer text for hello site and save hello file.</span></span>
   
    ![][18]
3. <span data-ttu-id="ddcdc-146">A **Megoldáskezelőben**, hello helyi menü megnyitásához, az hello megoldás csomópont, projektcsomópontra vagy hello fájl módosult, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-146">In **Solution Explorer**, open hello shortcut menu for hello solution node, project node, or hello file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="ddcdc-147">Írja be a megjegyzést, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-147">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="ddcdc-148">Válassza ki a hello **szinkronizálási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-148">Choose hello **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="ddcdc-149">Válassza ki a hello **leküldéses** toopush hivatkozásra a Visual Studio Team Services véglegesítési toohello tárházba.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-149">Choose hello **Push** link toopush your commit toohello repository in Visual Studio Team Services.</span></span> <span data-ttu-id="ddcdc-150">(Használhatja hello **szinkronizálási** toocopy a véglegesítések toohello tárház gombra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-150">(You can also use hello **Sync** button toocopy your commits toohello repository.</span></span> <span data-ttu-id="ddcdc-151">hello különbség az, hogy **szinkronizálási** is ponttá hello legutóbbi módosítások adattárból hello.)</span><span class="sxs-lookup"><span data-stu-id="ddcdc-151">hello difference is that **Sync** also pulls hello latest changes from hello repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="ddcdc-152">Válassza ki a hello **otthoni** gomb tooreturn toohello **Team Explorer** kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-152">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="ddcdc-153">Válasszon **buildek** tooview hello buildek folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-153">Choose **Builds** tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="ddcdc-154">**Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="ddcdc-155">tooview részletes napló szerint hello létrehozása történik, kattintson duplán a hello összeállítása folyamatban hello nevére.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-155">tooview a detailed log as hello build progresses, double-click hello name of hello build in progress.</span></span>
10. <span data-ttu-id="ddcdc-156">Amíg hello build folyamatban, tekintse meg hello build-definíciót, amely során használt hello varázsló toolink tooAzure jött létre.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-156">While hello build is in-progress, take a look at hello build definition that was created when you used hello wizard toolink tooAzure.</span></span>  <span data-ttu-id="ddcdc-157">Hello build definíciójának hello helyi menü megnyitásához, és válassza a **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-157">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="ddcdc-158">A hello **eseményindító** lapon látni fogja, hogy hello build definíciója van megadva toobuild a minden bejelentkezés alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-158">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in, by default.</span></span> <span data-ttu-id="ddcdc-159">(Egy felhőalapú szolgáltatás, a Visual Studio Team Services alapszik, és automatikusan átmeneti környezet hello főághoz toohello telepíti.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-159">(For a cloud service, Visual Studio Team Services builds and deploys hello master branch toohello staging environment automatically.</span></span> <span data-ttu-id="ddcdc-160">Továbbra is meg kell toodo egy manuális lépés toodeploy toohello élő webhelyet.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-160">You still have toodo a manual step toodeploy toohello live site.</span></span> <span data-ttu-id="ddcdc-161">A webalkalmazás nem található a környezet előkészítési telepíti hello főághoz közvetlen toohello élő hely.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-161">For a web app that doesn't have staging environment, it deploys hello master branch directly toohello live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="ddcdc-162">A hello **folyamat** lapon látható hello környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-162">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="ddcdc-163">Hello tulajdonságok értékeket megadni, ha azt szeretné, hogy a különböző hello alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-163">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="ddcdc-164">hello Azure közzétételi tulajdonságainak vannak hello **telepítési** szakaszt, és előfordulhat, hogy is kell tooset MSBuild paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-164">hello properties for Azure publishing are in hello **Deployment** section, and you might also need tooset MSBuild parameters.</span></span> <span data-ttu-id="ddcdc-165">Például egy felhőszolgáltatás-projekt toospecify eltérő "Felhő" szolgáltatáskonfiguráció található hello MSbuild paraméterek beállítása túl`/p:TargetProfile=[YourProfile]` ahol *[YourProfile]* hasonló nevű szolgáltatás konfigurációs fájlok ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-165">For example, in a cloud service project, toospecify a service configuration other than "Cloud", set hello MSbuild parameters too`/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="ddcdc-166">hello alábbi táblázat tartalmazza hello rendelkezésre álló tulajdonságok hello **telepítési** szakasz:</span><span class="sxs-lookup"><span data-stu-id="ddcdc-166">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="ddcdc-167">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ddcdc-167">Property</span></span> | <span data-ttu-id="ddcdc-168">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ddcdc-168">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="ddcdc-169">Nem megbízható tanúsítványok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ddcdc-169">Allow Untrusted Certificates</span></span> |<span data-ttu-id="ddcdc-170">Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-170">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="ddcdc-171">Frissítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ddcdc-171">Allow Upgrade</span></span> |<span data-ttu-id="ddcdc-172">Lehetővé teszi, hogy hello telepítési tooupdate létre egy új, hanem egy meglévő telepítését.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-172">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="ddcdc-173">Megőrzi a hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-173">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="ddcdc-174">Ne törölje</span><span class="sxs-lookup"><span data-stu-id="ddcdc-174">Do Not Delete</span></span> |<span data-ttu-id="ddcdc-175">Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-175">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="ddcdc-176">Elérési út tooDeployment beállítások</span><span class="sxs-lookup"><span data-stu-id="ddcdc-176">Path tooDeployment Settings</span></span> |<span data-ttu-id="ddcdc-177">hello elérési tooyour .pubxml fájl egy webalkalmazás, hello tárház relatív toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-177">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="ddcdc-178">Cloud services csomag figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-178">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="ddcdc-179">SharePoint-környezet</span><span class="sxs-lookup"><span data-stu-id="ddcdc-179">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="ddcdc-180">hello hello szolgáltatás neve azonos.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-180">hello same as hello service name.</span></span> |
    | <span data-ttu-id="ddcdc-181">Az Azure-telepítés környezet</span><span class="sxs-lookup"><span data-stu-id="ddcdc-181">Azure Deployment Environment</span></span> |<span data-ttu-id="ddcdc-182">hello webszolgáltatás alkalmazás vagy felhő neve.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-182">hello web app or cloud service name.</span></span> |
14. <span data-ttu-id="ddcdc-183">Időpontig a build sikeresen meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-183">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="ddcdc-184">Ha duplán kattint a hello build nevét, a Visual Studio megjeleníti egy **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-184">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="ddcdc-185">A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), hello tartozó központi telepítést megtekintheti a hello **központi telepítések** átmeneti környezet hello kiválasztásakor fülre.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-185">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="ddcdc-186">Keresse meg a tooyour webhelyének URL-címe.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-186">Browse tooyour site's URL.</span></span> <span data-ttu-id="ddcdc-187">A webes alkalmazás imént válassza hello **Tallózás** hello portál gombjára.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-187">For a web app, just choose  hello **Browse** button in hello portal.</span></span> <span data-ttu-id="ddcdc-188">Egy felhőalapú szolgáltatás, válassza ki az hello URL-címet a hello **gyors áttekintése** hello szakasza **irányítópult** oldal, amely hello átmeneti környezet látható.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-188">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment.</span></span>
    
    <span data-ttu-id="ddcdc-189">A folyamatos integrációt szolgáltatásokhoz központi telepítések közzétett toohello átmeneti környezet alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-189">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="ddcdc-190">Módosíthatja a hello beállítása **másik felhőalapú szolgáltatási környezet** tulajdonság túl**éles**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-190">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="ddcdc-191">Itt található, ahol hello webhely URL-címe megtalálható-e hello felhőalapú szolgáltatás irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-191">Here's where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="ddcdc-192">Egy új böngészőlapon nyílik tooreveal futó webhelyét.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-192">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="ddcdc-193">Ha egyéb módosítások tooyour projekt, több alkot, eseményindító, és Ön felhalmozódnak több központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-193">If you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="ddcdc-194">hello legújabb egy aktív jelölésű.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-194">hello latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="ddcdc-195">5: telepítse újra egy korábbi verzióját</span><span class="sxs-lookup"><span data-stu-id="ddcdc-195">5: Redeploy an earlier build</span></span>
<span data-ttu-id="ddcdc-196">Ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-196">This step is optional.</span></span> <span data-ttu-id="ddcdc-197">A klasszikus Azure portálon hello, egy korábbi üzemelő és kiválasztható **újratelepíteni** toorewind a hely tooan korábban be.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-197">In hello Azure classic portal, choose an earlier deployment and choose **Redeploy** toorewind your site tooan earlier check-in.</span></span> <span data-ttu-id="ddcdc-198">Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-198">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="ddcdc-199">6: hello éles környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="ddcdc-199">6: Change hello Production deployment</span></span>
<span data-ttu-id="ddcdc-200">Ha készen áll, előléptetheti kiválasztásával hello átmeneti toohello üzemi környezet **felcserélése** a hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-200">When you are ready, you can promote hello Staging environment toohello Production environment by choosing **Swap** in hello Azure classic portal.</span></span> <span data-ttu-id="ddcdc-201">újonnan telepített hello átmeneti környezet előléptetett tooProduction, és hello előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-201">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="ddcdc-202">hello aktív központi telepítéssel hello üzemi és átmeneti környezetek eltérőek lehetnek, de hello üzembe helyezési előzményeket legutóbbi buildek van hello azonos környezetben függetlenül.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-202">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="ddcdc-203">7: egy munkaágat a telepítése.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-203">7: Deploy from a working branch.</span></span>
<span data-ttu-id="ddcdc-204">Git használata esetén, általában egy munkaágat módosítsa, majd hello főághoz integrálása, amikor a fejlesztési eléri a állapotban.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-204">When you use Git, you usually make changes in a working branch and integrate into hello master branch when your development reaches a finished state.</span></span> <span data-ttu-id="ddcdc-205">Fázisban hello fejlesztési projekt fogja toobuild szeretné és hello működő fiókirodai tooAzure telepítése.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-205">During hello development phase of a project, you'll want toobuild and deploy hello working branch tooAzure.</span></span>

1. <span data-ttu-id="ddcdc-206">A **Team Explorer**, válassza ki a hello **Home** gombra, majd kattintson a hello **ágak** gombra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-206">In **Team Explorer**, choose hello **Home** button and then choose hello **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="ddcdc-207">Válassza ki a hello **új fiókirodai** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-207">Choose hello **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="ddcdc-208">Hello ág, például a "folyamatban"állapotra vált, hello nevét adja meg, és válassza a **ág létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-208">Enter hello name of hello branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="ddcdc-209">Ezzel létrehoz egy új helyi ágat.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-209">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="ddcdc-210">Hello fiókirodai közzététele.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-210">Publish hello branch.</span></span> <span data-ttu-id="ddcdc-211">Válasszon hello fiókirodai nevet a **közzé nem tett ágak**, és válassza a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-211">Choose hello branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="ddcdc-212">Alapértelmezés szerint csak akkor változik meg toohello főághoz eseményindító folyamatos build.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-212">By default, only changes toohello master branch trigger a continuous build.</span></span> <span data-ttu-id="ddcdc-213">fel egy munkaágat folyamatos buildet tooset válasszon hello **buildek** lap **Team Explorer**, és válassza **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-213">tooset up continuous build for a working branch, choose hello **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="ddcdc-214">Nyissa meg hello **adatforrás-beállítások** fülre. A **folyamatos integrációt és build ágak figyelt**, válassza a **kattintson ide egy új sor tooadd**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-214">Open hello **Source Settings** tab. Under **Monitored branches for continuous integration and build**, choose **Click here tooadd a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="ddcdc-215">Adja meg a létrehozott, például a refs fájlrendszer/fejek/működő hello ág.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-215">Specify hello branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="ddcdc-216">Olyan módosítást hello kódban, nyissa meg hello helyi menüje hello módosított fájlt, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-216">Make a change in hello code, open hello shortcut menu for hello changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="ddcdc-217">Válassza ki a hello **szinkronizálatlan véglegesítések** hivatkozásra, és válassza ki a hello **szinkronizálási** gombra való kattintást vagy hello **leküldéses** hivatkozás toocopy hello módosítja a Visual Studio hello munkaágat toohello példányát Team Services.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-217">Choose hello **Unsynced Commits** link, and choose  hello **Sync** button or hello **Push** link toocopy hello changes toohello copy of hello working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="ddcdc-218">Keresse meg a toohello **buildek** megtekintése és hello munkaágat hello build kiváltó csak kapott található.</span><span class="sxs-lookup"><span data-stu-id="ddcdc-218">Navigate toohello **Builds** view and find hello build that just got triggered for hello working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ddcdc-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ddcdc-219">Next steps</span></span>
<span data-ttu-id="ddcdc-220">toolearn további tippek a Visual Studio Team Services, a Git használatával, lásd: [fejlesztése és megoszthatja a Visual Studio használatával Git kód](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) és a Git-tárház, amely nem Visual Studio Team Services toopublish által kezelt információk tooAzure, lásd: [folyamatos üzembe helyezés tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-220">toolearn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services toopublish tooAzure, see [Continuous Deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="ddcdc-221">A Visual Studio Team Services további információkért lásd: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="ddcdc-221">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
