---
title: "A Git és az Azure-ban a Visual Studio Team Services folyamatos kézbesítési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja a Visual Studio Team Services csapatprojektek Git automatikusan létrehozásához, és a webes alkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások telepítéséhez használandó."
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
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="15dd3-103">Folyamatos készregyártás az Azure-ban a Visual Studio Team Services-zel és a Gittel</span><span class="sxs-lookup"><span data-stu-id="15dd3-103">Continuous delivery to Azure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="15dd3-104">Visual Studio Team Services csapatprojektek segítségével egy Git-tárház gazdagépet a forráskódot, és automatikusan build és Azure web Apps alkalmazások központi telepítése vagy felhőalapú szolgáltatások, amikor egy véglegesítési leküldése a tárházban.</span><span class="sxs-lookup"><span data-stu-id="15dd3-104">You can use Visual Studio Team Services team projects to host a Git repository for your source code, and automatically build and deploy to Azure web apps or cloud services whenever you push a commit to the repository.</span></span>

<span data-ttu-id="15dd3-105">Visual Studio 2013 és az Azure SDK telepítve lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="15dd3-105">You'll need Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="15dd3-106">Ha még nem rendelkezik a Visual Studio 2013, töltse le kiválasztásával a **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="15dd3-106">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="15dd3-107">Az Azure SDK telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="15dd3-107">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="15dd3-108">Az oktatóanyag elvégzéséhez egy Visual Studio Team Services-fiók szükséges: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="15dd3-108">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="15dd3-109">Egy felhőalapú szolgáltatás automatikusan építsenek, és telepítse az Azure, a Visual Studio Team Services telepítéséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="15dd3-109">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="15dd3-110">1: a Git-tárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="15dd3-110">1: Create a Git repository</span></span>
1. <span data-ttu-id="15dd3-111">Ha még nem rendelkezik a Visual Studio Team Services-fiók, akkor kaphat egy [Itt](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="15dd3-111">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="15dd3-112">A csapatprojekt létrehozásakor a forrásrendszerben vezérlő, válassza a Git.</span><span class="sxs-lookup"><span data-stu-id="15dd3-112">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="15dd3-113">Kövesse az utasításokat a csapatprojekt Visual Studio csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="15dd3-113">Follow the instructions to connect Visual Studio to your team project.</span></span>
2. <span data-ttu-id="15dd3-114">A **Team Explorer**, válassza ki a **klónozza a tárházat** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-114">In **Team Explorer**, choose the **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="15dd3-115">Adja meg a helyi másolat helyét, és válassza a **Klónozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-115">Specify the location of the local copy and then choose the **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a><span data-ttu-id="15dd3-116">2: projekt létrehozása és véglegesítheti a tárházban</span><span class="sxs-lookup"><span data-stu-id="15dd3-116">2: Create a project and commit it to the repository</span></span>
1. <span data-ttu-id="15dd3-117">A **Team Explorer**, a a **megoldások** területen válassza ki a **új** hozzon létre egy új projektet a helyi tárházban mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="15dd3-117">In **Team Explorer**, in the **Solutions** section, choose the **New** link to create a new project in the local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="15dd3-118">A bemutatóban szereplő lépéseket követve telepítheti a webes alkalmazás vagy a felhőszolgáltatás (Azure-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="15dd3-118">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span> <span data-ttu-id="15dd3-119">Hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt.</span><span class="sxs-lookup"><span data-stu-id="15dd3-119">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="15dd3-120">Győződjön meg arról, hogy a projekt célozza a .NET-keretrendszer 4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="15dd3-120">Make sure that the project targets the .NET Framework 4 or later.</span></span> <span data-ttu-id="15dd3-121">Egy felhőszolgáltatás-projekt létrehozásakor, adjon hozzá egy ASP.NET MVC webes és feldolgozói szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="15dd3-121">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="15dd3-122">Ha szeretne létrehozni egy webalkalmazást, válassza ki azt a **ASP.NET Web Application** a project, és válassza a **MVC**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-122">If you want to create a web app, choose the **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="15dd3-123">Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="15dd3-123">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="15dd3-124">Nyissa meg a helyi menüben a megoldás, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-124">Open the shortcut menu for the solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="15dd3-125">Ha az első alkalommal a Visual Studio Team Services már használta a Git, szüksége arra, hogy néhány adatra, hogy azonosítsa magát a Gitben.</span><span class="sxs-lookup"><span data-stu-id="15dd3-125">If this is the first time you've used Git in Visual Studio Team Services, you'll need to provide some information to identify yourself in Git.</span></span> <span data-ttu-id="15dd3-126">Az a **függőben lévő módosítások** területén **Team Explorer**, adja meg a felhasználónevet és e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="15dd3-126">In the **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="15dd3-127">Írjon megjegyzést a véglegesítés, és válassza ki a **véglegesítési** gombra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-127">Enter a comment for the commit and then choose the **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="15dd3-128">Vegye figyelembe a beállításokat, vagy adott módosítások kizárja a verziókezelőbe mentésekor.</span><span class="sxs-lookup"><span data-stu-id="15dd3-128">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="15dd3-129">Ha a szükséges módosításokat ki vannak zárva, válassza a **tartalmazza az összes**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-129">If the changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="15dd3-130">Most már véglegesített a módosításokat a tárházba helyi példánya.</span><span class="sxs-lookup"><span data-stu-id="15dd3-130">You've now committed the changes in your local copy of the repository.</span></span> <span data-ttu-id="15dd3-131">Ezután szinkronizálhatja a kiszolgáló a módosításokat kiválasztásával a **szinkronizálási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-131">Next, sync those changes with the server by choosing the **Sync** link.</span></span>

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="15dd3-132">3: a projekt csatlakozzon az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="15dd3-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="15dd3-133">Most, hogy az egyes forráskód azt a Visual Studio Team Services Git-tárház, készen áll a git-tárház csatlakozzon az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="15dd3-133">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready to connect your git repository to Azure.</span></span>  <span data-ttu-id="15dd3-134">Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat, válassza ki a + ikonra a lap alján maradt, és válassza **Felhőszolgáltatás** vagy **Web App** és majd **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the + icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="15dd3-135">A felhőszolgáltatások, válassza ki a **Visual Studio Team Services való közzététel beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-135">For cloud services, choose the **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="15dd3-136">A web Apps, válassza ki a **verziókövetésből üzembe helyezés beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-136">For web apps, choose the **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="15dd3-137">A varázslóban a szövegmezőben írja be a Visual Studio Team Services-fiók nevét, és válassza ki a **engedélyezik most** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-137">In the wizard, type the name of your Visual Studio Team Services account in the textbox and choose the **Authorize Now** link.</span></span> <span data-ttu-id="15dd3-138">Jelentkezzen be a kérheti.</span><span class="sxs-lookup"><span data-stu-id="15dd3-138">You might be asked to sign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="15dd3-139">Az a **kapcsolatkérelem** előugró párbeszédpanelen válasszon **elfogadás** a csapatprojekt konfigurálása a Visual Studio Team Services Azure engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="15dd3-139">In the **Connection Request** pop-up dialog, choose **Accept** to authorize Azure to configure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="15dd3-140">Után engedélyezési sikeres, megjelenik egy legördülő listát, amely tartalmazza a Visual Studio Team Services csapatprojektek.</span><span class="sxs-lookup"><span data-stu-id="15dd3-140">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="15dd3-141">Válassza ki az előző lépésben létrehozott csapatprojekt nevét, és a varázsló a pipa gombra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-141">Select the name of team project that you created in the previous steps, and choose the wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="15dd3-142">A véglegesítési leküldése a tárház legközelebb a Visual Studio Team Services elkészíti és a projekt telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="15dd3-142">The next time you push a commit to your repository, Visual Studio Team Services will build and deploy your project to Azure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="15dd3-143">4: egy újjáépítését indítja, és telepítse újra a projekthez</span><span class="sxs-lookup"><span data-stu-id="15dd3-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="15dd3-144">A Visual Studióban nyissa meg a fájlt, és módosítsa úgy.</span><span class="sxs-lookup"><span data-stu-id="15dd3-144">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="15dd3-145">Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\egy MVC webes szerepkör megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="15dd3-145">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="15dd3-146">Szerkessze az előláb szövegét, a helyhez, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="15dd3-146">Edit the footer text for the site and save the file.</span></span>
   
    ![][18]
3. <span data-ttu-id="15dd3-147">A **Megoldáskezelőben**, nyissa meg a helyi menüben a megoldás csomópont, projektcsomópontra vagy megváltozott, és válassza a fájl **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-147">In **Solution Explorer**, open the shortcut menu for the solution node, project node, or the file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="15dd3-148">Írja be a megjegyzést, és válassza a **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-148">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="15dd3-149">Válassza ki a **szinkronizálási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-149">Choose the **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="15dd3-150">Válassza ki a **leküldéses** a véglegesítési leküldése a Visual Studio Team Services-tárház mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="15dd3-150">Choose the **Push** link to push your commit to the repository in Visual Studio Team Services.</span></span> <span data-ttu-id="15dd3-151">(Is használhatja a **szinkronizálási** gombra kattintva a véglegesítések másolja a tárházba.</span><span class="sxs-lookup"><span data-stu-id="15dd3-151">(You can also use the **Sync** button to copy your commits to the repository.</span></span> <span data-ttu-id="15dd3-152">A különbség az, hogy **szinkronizálási** is kéri le. a legutóbbi változtatásokat a tárházból.)</span><span class="sxs-lookup"><span data-stu-id="15dd3-152">The difference is that **Sync** also pulls the latest changes from the repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="15dd3-153">Válassza ki a **otthoni** gombra kattintva visszatérhet a **Team Explorer** kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="15dd3-153">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="15dd3-154">Válasszon **buildek** megtekintéséhez a buildek folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="15dd3-154">Choose **Builds** to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="15dd3-155">**Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="15dd3-155">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="15dd3-156">A build előrehaladtával a részletes napló megtekintéséhez kattintson duplán a a build folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="15dd3-156">To view a detailed log as the build progresses, double-click the name of the build in progress.</span></span>
10. <span data-ttu-id="15dd3-157">Bár a összeállítása folyamatban, tekintse meg a build-definíciót, amely jött létre, amikor a varázsló segítségével csatolja az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="15dd3-157">While the build is in-progress, take a look at the build definition that was created when you used the wizard to link to Azure.</span></span>  <span data-ttu-id="15dd3-158">Nyissa meg a helyi menüben a build definíciójához, és válassza a **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-158">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="15dd3-159">Az a **eseményindító** lapon látni fogja, hogy a build definition létrehozásához, minden egyes bejelentkezéskor értéke alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="15dd3-159">In the **Trigger** tab, you will see that the build definition is set to build on every check-in, by default.</span></span> <span data-ttu-id="15dd3-160">(Egy felhőalapú szolgáltatás, a Visual Studio Team Services alapszik, és automatikusan telepíti a főágba az átmeneti környezet.</span><span class="sxs-lookup"><span data-stu-id="15dd3-160">(For a cloud service, Visual Studio Team Services builds and deploys the master branch to the staging environment automatically.</span></span> <span data-ttu-id="15dd3-161">Továbbra is fennáll az élő webhelyet szeretne telepíteni egy manuális lépés elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="15dd3-161">You still have to do a manual step to deploy to the live site.</span></span> <span data-ttu-id="15dd3-162">A webalkalmazás nem található a környezet előkészítési telepíti a főágba közvetlenül az élő webhelyet.</span><span class="sxs-lookup"><span data-stu-id="15dd3-162">For a web app that doesn't have staging environment, it deploys the master branch directly to the live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="15dd3-163">Az a **folyamat** lapon megtekintheti a telepítési környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="15dd3-163">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="15dd3-164">Adja meg a tulajdonságok értékeit, ha azt szeretné, hogy a különböző értékeket, mint az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="15dd3-164">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="15dd3-165">Az Azure közzétételi tulajdonságok szerepelnek a **telepítési** szakaszt, és előfordulhat, hogy is be kell MSBuild paraméterek.</span><span class="sxs-lookup"><span data-stu-id="15dd3-165">The properties for Azure publishing are in the **Deployment** section, and you might also need to set MSBuild parameters.</span></span> <span data-ttu-id="15dd3-166">Például a felhő service-projektet, adja meg a "Felhő" eltérő szolgáltatáskonfiguráció állítsa az MSbuild paramétereit `/p:TargetProfile=[YourProfile]` ahol *[YourProfile]* hasonló nevű szolgáltatás konfigurációs fájlok ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="15dd3-166">For example, in a cloud service project, to specify a service configuration other than "Cloud", set the MSbuild parameters to `/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="15dd3-167">Az alábbi táblázat a rendelkezésre álló tulajdonságok a **telepítési** szakasz:</span><span class="sxs-lookup"><span data-stu-id="15dd3-167">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="15dd3-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="15dd3-168">Property</span></span> | <span data-ttu-id="15dd3-169">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="15dd3-169">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="15dd3-170">Nem megbízható tanúsítványok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15dd3-170">Allow Untrusted Certificates</span></span> |<span data-ttu-id="15dd3-171">Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni.</span><span class="sxs-lookup"><span data-stu-id="15dd3-171">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="15dd3-172">Frissítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15dd3-172">Allow Upgrade</span></span> |<span data-ttu-id="15dd3-173">Lehetővé teszi, hogy a központi telepítés helyett újat hoz létre egy meglévő üzemelő példány frissítése.</span><span class="sxs-lookup"><span data-stu-id="15dd3-173">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="15dd3-174">Megőrzi az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="15dd3-174">Preserves the IP address.</span></span> |
    | <span data-ttu-id="15dd3-175">Ne törölje</span><span class="sxs-lookup"><span data-stu-id="15dd3-175">Do Not Delete</span></span> |<span data-ttu-id="15dd3-176">Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett).</span><span class="sxs-lookup"><span data-stu-id="15dd3-176">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="15dd3-177">A telepítési beállítások elérési útja</span><span class="sxs-lookup"><span data-stu-id="15dd3-177">Path to Deployment Settings</span></span> |<span data-ttu-id="15dd3-178">A fájl elérési útját a .pubxml webes alkalmazások esetén a tárház gyökérkönyvtárában viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="15dd3-178">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="15dd3-179">Cloud services csomag figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="15dd3-179">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="15dd3-180">SharePoint-környezet</span><span class="sxs-lookup"><span data-stu-id="15dd3-180">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="15dd3-181">Ugyanaz, mint a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="15dd3-181">The same as the service name.</span></span> |
    | <span data-ttu-id="15dd3-182">Az Azure-telepítés környezet</span><span class="sxs-lookup"><span data-stu-id="15dd3-182">Azure Deployment Environment</span></span> |<span data-ttu-id="15dd3-183">A webes alkalmazás vagy a felhőbeli szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="15dd3-183">The web app or cloud service name.</span></span> |
14. <span data-ttu-id="15dd3-184">Időpontig a build sikeresen meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="15dd3-184">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="15dd3-185">Ha duplán kattint a build nevét, a Visual Studio megjeleníti a **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.</span><span class="sxs-lookup"><span data-stu-id="15dd3-185">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="15dd3-186">Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), a tekintheti meg a társított központi telepítéshez a **központi telepítések** az átmeneti kiválasztásakor fülre.</span><span class="sxs-lookup"><span data-stu-id="15dd3-186">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="15dd3-187">Tallózással keresse meg a webhely URL-címe.</span><span class="sxs-lookup"><span data-stu-id="15dd3-187">Browse to your site's URL.</span></span> <span data-ttu-id="15dd3-188">A webes alkalmazás imént válassza a **Tallózás** a portál gombjára.</span><span class="sxs-lookup"><span data-stu-id="15dd3-188">For a web app, just choose  the **Browse** button in the portal.</span></span> <span data-ttu-id="15dd3-189">Egy felhőalapú szolgáltatás, válassza az URL-címet a **gyors áttekintése** szakasza a **irányítópult** oldal, amely az átmeneti környezet látható.</span><span class="sxs-lookup"><span data-stu-id="15dd3-189">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment.</span></span>
    
    <span data-ttu-id="15dd3-190">A folyamatos integrációt szolgáltatásokhoz központi telepítések alapértelmezés szerint az átmeneti környezetben kerülnek közzétételre.</span><span class="sxs-lookup"><span data-stu-id="15dd3-190">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="15dd3-191">Ez módosítható úgy, hogy a **másik felhőalapú szolgáltatási környezet** tulajdonságot **éles**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-191">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="15dd3-192">Ez a webhely URL-címe is helyezi a felhőalapú szolgáltatás irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="15dd3-192">Here's where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="15dd3-193">Hogy láthatóvá váljon a futó helyet egy új böngészőlapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="15dd3-193">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="15dd3-194">Ha más módosítja a projekthez, több alkot, eseményindító, és akkor felhalmozódnak több központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="15dd3-194">If you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="15dd3-195">A legújabb egy aktív van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="15dd3-195">The latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="15dd3-196">5: telepítse újra egy korábbi verzióját</span><span class="sxs-lookup"><span data-stu-id="15dd3-196">5: Redeploy an earlier build</span></span>
<span data-ttu-id="15dd3-197">Ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="15dd3-197">This step is optional.</span></span> <span data-ttu-id="15dd3-198">A klasszikus Azure portálon, válasszon egy korábbi üzemelő, és válassza a **újratelepíteni** való visszatekerés a webhely egy korábbi be.</span><span class="sxs-lookup"><span data-stu-id="15dd3-198">In the Azure classic portal, choose an earlier deployment and choose **Redeploy** to rewind your site to an earlier check-in.</span></span> <span data-ttu-id="15dd3-199">Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.</span><span class="sxs-lookup"><span data-stu-id="15dd3-199">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="15dd3-200">6: módosítsa az éles környezet</span><span class="sxs-lookup"><span data-stu-id="15dd3-200">6: Change the Production deployment</span></span>
<span data-ttu-id="15dd3-201">Ha készen áll, előléptetheti kiválasztásával az éles környezetbe átmeneti környezet **felcserélése** a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="15dd3-201">When you are ready, you can promote the Staging environment to the Production environment by choosing **Swap** in the Azure classic portal.</span></span> <span data-ttu-id="15dd3-202">Az újonnan telepített átmeneti környezet éles van előléptetve, és az előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet.</span><span class="sxs-lookup"><span data-stu-id="15dd3-202">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="15dd3-203">Az aktív központi telepítéssel üzemi és átmeneti környezetek eltérőek lehetnek, de az üzembe helyezési előzményeket legutóbbi buildek környezet függetlenül ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="15dd3-203">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="15dd3-204">7: egy munkaágat a telepítése.</span><span class="sxs-lookup"><span data-stu-id="15dd3-204">7: Deploy from a working branch.</span></span>
<span data-ttu-id="15dd3-205">Git használata esetén általában egy munkaágat a módosítások végrehajtása és a főágba integrálása, ha a fejlesztési elér egy állapotban.</span><span class="sxs-lookup"><span data-stu-id="15dd3-205">When you use Git, you usually make changes in a working branch and integrate into the master branch when your development reaches a finished state.</span></span> <span data-ttu-id="15dd3-206">A fázisban a fejlesztési projekt érdemes létrehozásához és telepítéséhez a munkaágat az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="15dd3-206">During the development phase of a project, you'll want to build and deploy the working branch to Azure.</span></span>

1. <span data-ttu-id="15dd3-207">A **Team Explorer**, válassza ki a **Home** gombra, majd válassza ki a **ágak** gombra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-207">In **Team Explorer**, choose the **Home** button and then choose the **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="15dd3-208">Válassza ki a **új fiókirodai** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="15dd3-208">Choose the **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="15dd3-209">Írja be a fiókiroda, például a "folyamatban"állapotra vált, a nevét, és válassza a **ág létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-209">Enter the name of the branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="15dd3-210">Ezzel létrehoz egy új helyi ágat.</span><span class="sxs-lookup"><span data-stu-id="15dd3-210">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="15dd3-211">A fiókirodai közzététele.</span><span class="sxs-lookup"><span data-stu-id="15dd3-211">Publish the branch.</span></span> <span data-ttu-id="15dd3-212">Válassza ki a fiókiroda nevét a **közzé nem tett ágak**, és válassza a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-212">Choose the branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="15dd3-213">Alapértelmezés szerint csak a főágba változtatások folyamatos build indítható el.</span><span class="sxs-lookup"><span data-stu-id="15dd3-213">By default, only changes to the master branch trigger a continuous build.</span></span> <span data-ttu-id="15dd3-214">Egy munkaágat folyamatos buildet beállításához válassza ki a **buildek** lap **Team Explorer**, válassza **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-214">To set up continuous build for a working branch, choose the **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="15dd3-215">Nyissa meg a **adatforrás-beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="15dd3-215">Open the **Source Settings** tab.</span></span> <span data-ttu-id="15dd3-216">A **folyamatos integrációt és build ágak figyelt**, válassza a **kattintson ide egy új sort ad hozzá a**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-216">Under **Monitored branches for continuous integration and build**, choose **Click here to add a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="15dd3-217">Adja meg a létrehozott, például a refs fájlrendszer/fejek/működő ágat.</span><span class="sxs-lookup"><span data-stu-id="15dd3-217">Specify the branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="15dd3-218">Módosítja a kódban, nyissa meg a helyi menüben a módosított fájlt, és válassza **véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="15dd3-218">Make a change in the code, open the shortcut menu for the changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="15dd3-219">Válassza ki a **szinkronizálatlan véglegesítések** hivatkozásra, és válassza ki a **szinkronizálási** gombra vagy a **leküldéses** másolja a módosításokat a munkaágat a Visual Studio Team Services példányát mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="15dd3-219">Choose the **Unsynced Commits** link, and choose  the **Sync** button or the **Push** link to copy the changes to the copy of the working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="15dd3-220">Keresse meg a **buildek** megtekintése és a munkaágat a build kiváltó csak kapott található.</span><span class="sxs-lookup"><span data-stu-id="15dd3-220">Navigate to the **Builds** view and find the build that just got triggered for the working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15dd3-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15dd3-221">Next steps</span></span>
<span data-ttu-id="15dd3-222">A Git használata a Visual Studio Team Services további tippek további tudnivalókért lásd: [fejlesztése és megoszthatja a Visual Studio használatával Git kód](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) és a Git-tárház, amely nem Visual Studio Team Services közzétételére által kezelt információk Az Azure, lásd: [folyamatos üzembe helyezés az Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="15dd3-222">To learn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services to publish to Azure, see [Continuous Deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="15dd3-223">A Visual Studio Team Services további információkért lásd: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="15dd3-223">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
