---
title: "a Visual Studio Team Services, Azure-ban aaaContinuous kézbesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure a Visual Studio Team Services team projects tooautomatically és toohello webalkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások üzembe."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a><span data-ttu-id="a4f2e-103">Folyamatos kézbesítési tooAzure Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="a4f2e-103">Continuous delivery tooAzure using Visual Studio Team Services</span></span>
<span data-ttu-id="a4f2e-104">A Visual Studio Team Services team projects tooautomatically build konfigurálása és tooAzure webes alkalmazásokat, vagy a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-104">You can configure your Visual Studio Team Services team projects tooautomatically build and deploy tooAzure web apps or cloud services.</span></span>  <span data-ttu-id="a4f2e-105">(Tooset akár folyamatos build és a rendszer szabályzatsablonokat információt egy *helyszíni* a Team Foundation Server, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="a4f2e-105">(For information on how tooset up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="a4f2e-106">Ez az oktatóanyag feltételezi, hogy rendelkezik a Visual Studio 2013 és Azure SDK telepítése hello.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-106">This tutorial assumes you have Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="a4f2e-107">Ha még nem rendelkezik a Visual Studio 2013, töltse le hello kiválasztásával **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Telepítse az Azure SDK hello [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-107">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="a4f2e-108">Ez az oktatóanyag kell egy Visual Studio Team Services-fiók toocomplete: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-108">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="a4f2e-109">egy felhőalapú szolgáltatás tooautomatically mentése tooset build és tooAzure telepíteni a Visual Studio Team Services, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-109">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="a4f2e-110">1: team-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a4f2e-110">1: Create a team project</span></span>
<span data-ttu-id="a4f2e-111">Útmutatás alapján hello [Itt](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate a csapat projektre, és hivatkozás tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-111">Follow hello instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate your team project and link it tooVisual Studio.</span></span> <span data-ttu-id="a4f2e-112">Ez a forgatókönyv azt feltételezi, hogy a forrás vezérlő megoldásként használja a Team Foundation verzió vezérlő (TFVC).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-112">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="a4f2e-113">Ha szeretne toouse Git verziókezelést, lásd: [hello Git verziója, ez a forgatókönyv](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-113">If you want toouse Git for version control, see [hello Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-toosource-control"></a><span data-ttu-id="a4f2e-114">2: Ellenőrizze, hogy a projekt toosource vezérlő szerepel</span><span class="sxs-lookup"><span data-stu-id="a4f2e-114">2: Check in a project toosource control</span></span>
1. <span data-ttu-id="a4f2e-115">A Visual Studióban nyissa meg a hello megoldást szeretné, hogy toodeploy, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-115">In Visual Studio, open hello solution you want toodeploy, or create a new one.</span></span>
   <span data-ttu-id="a4f2e-116">Webes alkalmazást telepít, vagy a bemutatóban szereplő lépések egy felhőalapú szolgáltatás (Azure-alkalmazás) a következő hello által.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-116">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span>
   <span data-ttu-id="a4f2e-117">Ha azt szeretné, hogy egy új megoldás toocreate, hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-117">If you want toocreate a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="a4f2e-118">Győződjön meg arról, hogy hello projekt célok .NET-keretrendszer 4 vagy 4.5, és egy felhőszolgáltatás-projekt létrehozásakor egy ASP.NET MVC webes és feldolgozói szerepkörök hozzáadása, és válassza ki az internetes alkalmazás hello webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-118">Make sure that hello project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for hello web role.</span></span> <span data-ttu-id="a4f2e-119">Amikor a rendszer kéri, válassza ki a **az internetes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-119">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="a4f2e-120">Ha azt szeretné, hogy a webes alkalmazás toocreate, válassza ki a hello ASP.NET Web Application project sablont, és válassza a MVC.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-120">If you want toocreate a web app, choose hello ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="a4f2e-121">Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-121">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a4f2e-122">Visual Studio Team Services csak a jelenleg támogatott CI központi telepítések, a Visual Studio-webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-122">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="a4f2e-123">Webhely-projektek kívül esnek a hatókörön.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-123">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="a4f2e-124">Nyissa meg a helyi menü hello hello megoldás, és válassza a **megoldás hozzáadása tooSource vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-124">Open hello context menu for hello solution, and choose **Add Solution tooSource Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="a4f2e-125">Fogadja el, vagy módosítsa a hello alapértelmezett beállításokat, és válassza ki a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-125">Accept or change hello defaults and choose hello **OK** button.</span></span> <span data-ttu-id="a4f2e-126">Miután hello folyamat befejeződött, forrás vezérlő ikonok jelennek meg **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-126">Once hello process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="a4f2e-127">Nyissa meg a helyi menü hello hello megoldás, és válassza a **felvétel**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-127">Open hello shortcut menu for hello solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="a4f2e-128">A hello **függőben lévő módosítások** területén **Team Explorer**, írja be a hello be megjegyzéseit, és válassza ki a hello **felvétel** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-128">In hello **Pending Changes** area of **Team Explorer**, type a comment for hello check-in and choose hello **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="a4f2e-129">Megjegyzés: hello beállítások tooinclude, vagy adott módosítások kizárása a verziókezelőbe mentésekor.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-129">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="a4f2e-130">Ha szükséges, nem tartoznak a módosítások, válassza ki azt a hello **tartalmazza az összes** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-130">If desired changes are excluded, choose hello **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="a4f2e-131">3: hello projekt tooAzure csatlakozás</span><span class="sxs-lookup"><span data-stu-id="a4f2e-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="a4f2e-132">Most, hogy a Visual STUDIO Team Services csapatprojekt néhány adatforrás kóddal azt, tooconnect készen áll a csapat tooAzure projektre.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-132">Now that you have a VS Team Services team project with some source code in it, you are ready tooconnect your team project tooAzure.</span></span>  <span data-ttu-id="a4f2e-133">A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat hello kiválasztásával  **+**  hello bal alsó és választja ikonra **Felhőszolgáltatás** vagy **webalkalmazás** , majd **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello **+** icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="a4f2e-134">Válassza ki a hello **Visual Studio Team Services való közzététel beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-134">Choose hello **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="a4f2e-135">Hello varázslóban, írja be a Visual Studio Team Services-fiók nevét hello hello szövegmezőben, majd kattintson a hello **engedélyezik most** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-135">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and click hello **Authorize Now** link.</span></span> <span data-ttu-id="a4f2e-136">Előfordulhat, hogy a toosign kéri.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-136">You might be asked toosign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="a4f2e-137">A hello **kapcsolatkérelem** előugró párbeszédpanelen válassza ki a hello **elfogadás** gomb tooauthorize a csapat projektre a Visual STUDIO Team Services, Azure tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-137">In hello **Connection Request** pop-up dialog, choose hello **Accept** button tooauthorize Azure tooconfigure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="a4f2e-138">Ha engedélyezési sikeres, megjelenik egy legördülő lista a Visual Studio Team Services csapatprojektek listáját tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-138">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="a4f2e-139">Válassza ki a csapatprojekt hello előző lépésekben létrehozott hello nevét, és kattintson a pipa gombra hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-139">Choose  hello name of team project that you created in hello previous steps, and then choose hello wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="a4f2e-140">Után a projekt ki van kapcsolva, látni fogja, hogy egyes módosítások tooyour Visual Studio Team Services csapatprojektben ellenőrzése a következő útmutatást ki.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-140">After your project is linked, you will see some instructions for checking in changes tooyour Visual Studio Team Services team project.</span></span>  <span data-ttu-id="a4f2e-141">A következő bejelentkezéskor, a Visual Studio Team Services elkészíti és központi telepítése a projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-141">On your next check-in, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>  <span data-ttu-id="a4f2e-142">Ez most hello kattintva próbálkozzon **ellenőrizze a Visual Studio** hivatkozásra, és ezután hello **indítsa el a Visual Studio** hivatkozásra (vagy ezzel egyenértékű hello **Visual Studio** hello aljához gomb hello Letöltés gombra).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-142">Try this now by clicking hello **Check In from Visual Studio** link, and then hello **Launch Visual Studio** link (or hello equivalent **Visual Studio** button at hello bottom of hello portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="a4f2e-143">4: egy újjáépítését indítja, és telepítse újra a projekthez</span><span class="sxs-lookup"><span data-stu-id="a4f2e-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="a4f2e-144">A Visual Studio **Team Explorer**, válassza ki a hello **forrás vezérlő Explorer** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-144">In Visual Studio's **Team Explorer**, choose hello **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="a4f2e-145">Keresse meg a megoldásfájlt tooyour, majd nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-145">Navigate tooyour solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="a4f2e-146">A **Megoldáskezelőben**, nyissa meg a fájlt, és módosítsa azt.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-146">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="a4f2e-147">Például a hello fájl módosítása `_Layout.cshtml` hello nézetek alapján\\egy MVC webes szerepkör megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-147">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="a4f2e-148">Hello embléma hello hely szerkesztése, és válassza a **Ctrl + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-148">Edit hello logo for hello site and choose **Ctrl+S** toosave hello file.</span></span>
   
    ![][18]
5. <span data-ttu-id="a4f2e-149">A **Team Explorer**, válassza ki a hello **függőben lévő módosítások** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-149">In **Team Explorer**, choose hello **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="a4f2e-150">Írjon egy megjegyzést, és válassza a hello **felvétel** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-150">Enter a comment and then choose hello **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="a4f2e-151">Válassza ki a hello **otthoni** gomb tooreturn toohello **Team Explorer** kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-151">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="a4f2e-152">Válassza ki a hello **buildek** hivatkozás tooview hello buildek folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-152">Choose hello **Builds** link tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="a4f2e-153">**Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-153">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="a4f2e-154">Kattintson duplán a folyamatban lévő tooview részletes napló hello build hello nevére hello build előrehaladtával.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-154">Double-click hello name of hello build in progress tooview a detailed log as hello build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="a4f2e-155">Amíg hello build folyamatban, tekintse meg hello build-definíciót, amely jött létre, amikor a TFS tooAzure csatolta hello varázsló segítségével.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-155">While hello build is in-progress, take a look at hello build definition that was created when you linked TFS tooAzure by using hello wizard.</span></span>  <span data-ttu-id="a4f2e-156">Hello build definíciójának hello helyi menü megnyitásához, és válassza a **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-156">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="a4f2e-157">A hello **eseményindító** lapon látni fogja, hogy hello build definíciója van megadva toobuild a minden bejelentkezés alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-157">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="a4f2e-158">A hello **folyamat** lapon látható hello környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-158">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span> <span data-ttu-id="a4f2e-159">Ha a webalkalmazásokkal dolgozik, hello tulajdonságok látja eltérő itt látható lesz.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-159">If you are working with web apps, hello properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="a4f2e-160">Hello tulajdonságok értékeket megadni, ha azt szeretné, hogy a különböző hello alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-160">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="a4f2e-161">hello Azure közzétételi tulajdonságainak vannak hello **telepítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-161">hello properties for Azure publishing are in hello **Deployment** section.</span></span>
    
     <span data-ttu-id="a4f2e-162">hello alábbi táblázat tartalmazza hello rendelkezésre álló tulajdonságok hello **telepítési** szakasz:</span><span class="sxs-lookup"><span data-stu-id="a4f2e-162">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="a4f2e-163">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a4f2e-163">Property</span></span> | <span data-ttu-id="a4f2e-164">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="a4f2e-164">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="a4f2e-165">Nem megbízható tanúsítványok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a4f2e-165">Allow Untrusted Certificates</span></span> |<span data-ttu-id="a4f2e-166">Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-166">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="a4f2e-167">Frissítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a4f2e-167">Allow Upgrade</span></span> |<span data-ttu-id="a4f2e-168">Lehetővé teszi, hogy hello telepítési tooupdate létre egy új, hanem egy meglévő telepítését.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-168">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="a4f2e-169">Megőrzi a hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-169">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="a4f2e-170">Ne törölje</span><span class="sxs-lookup"><span data-stu-id="a4f2e-170">Do Not Delete</span></span> |<span data-ttu-id="a4f2e-171">Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-171">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="a4f2e-172">Elérési út tooDeployment beállítások</span><span class="sxs-lookup"><span data-stu-id="a4f2e-172">Path tooDeployment Settings</span></span> |<span data-ttu-id="a4f2e-173">hello elérési tooyour .pubxml fájl egy webalkalmazás, hello tárház relatív toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-173">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="a4f2e-174">Cloud services csomag figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-174">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="a4f2e-175">SharePoint-környezet</span><span class="sxs-lookup"><span data-stu-id="a4f2e-175">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="a4f2e-176">hello hello szolgáltatás neve azonos.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-176">hello same as hello service name.</span></span> |
    | <span data-ttu-id="a4f2e-177">Az Azure-telepítés környezet</span><span class="sxs-lookup"><span data-stu-id="a4f2e-177">Azure Deployment Environment</span></span> |<span data-ttu-id="a4f2e-178">hello webszolgáltatás alkalmazás vagy felhő neve.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-178">hello web app or cloud service name.</span></span> |
12. <span data-ttu-id="a4f2e-179">Ha több szolgáltatáskonfiguráció (.cscfg-fájlok) használ, megadhat hello kívánt szolgáltatáskonfiguráció hello **felépítéséhez, speciális, MSBuild-argumentumok** beállítást.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-179">If you are using multiple service configurations (.cscfg files), you can specify hello desired service configuration in hello **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="a4f2e-180">Például toouse ServiceConfiguration.Test.cscfg, beállításhoz MSBuild-argumentumok sor `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-180">For example, toouse ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="a4f2e-181">Időpontig a build sikeresen meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-181">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="a4f2e-182">Ha duplán kattint a hello build nevét, a Visual Studio megjeleníti egy **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-182">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="a4f2e-183">A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), hello tartozó központi telepítést megtekintheti a hello **központi telepítések** átmeneti környezet hello kiválasztásakor fülre.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-183">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="a4f2e-184">Keresse meg a tooyour webhelyének URL-címe.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-184">Browse tooyour site's URL.</span></span> <span data-ttu-id="a4f2e-185">A webalkalmazás kattintson hello **Tallózás** hello parancssáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-185">For a web app, just click hello **Browse** button on hello command bar.</span></span> <span data-ttu-id="a4f2e-186">Egy felhőalapú szolgáltatás, válassza ki az hello URL-címet a hello **gyors áttekintése** hello szakasza **irányítópult** oldal, amely hello átmeneti környezet egy felhőalapú szolgáltatás számára látható.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-186">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment for a cloud service.</span></span> <span data-ttu-id="a4f2e-187">A folyamatos integrációt szolgáltatásokhoz központi telepítések közzétett toohello átmeneti környezet alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-187">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="a4f2e-188">Módosíthatja a hello beállítása **másik felhőalapú szolgáltatási környezet** tulajdonság túl**éles**.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-188">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="a4f2e-189">Ezen a képernyőfelvételen látható látható, ahol hello webhely URL-címe hello felhőalapú szolgáltatás irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-189">This screenshot shows where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="a4f2e-190">Egy új böngészőlapon nyílik tooreveal futó webhelyét.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-190">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="a4f2e-191">A felhőszolgáltatások Ha egyéb módosítások tooyour projekt, több alkot, eseményindító, és Ön felhalmozódnak több központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-191">For cloud services, if you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="a4f2e-192">hello legújabb egy aktív jelölésű.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-192">hello latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="a4f2e-193">5: telepítse újra egy korábbi verzióját</span><span class="sxs-lookup"><span data-stu-id="a4f2e-193">5: Redeploy an earlier build</span></span>
<span data-ttu-id="a4f2e-194">Ez a lépés toocloud szolgáltatások vonatkozik, és nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-194">This step applies toocloud services and is optional.</span></span> <span data-ttu-id="a4f2e-195">A klasszikus Azure portálon hello, válasszon egy korábbi üzemelő, és válassza a hello **újratelepíteni** toorewind gombra a hely tooan korábban be.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-195">In hello Azure classic portal, choose an earlier deployment and then choose hello **Redeploy** button toorewind your site tooan earlier check-in.</span></span>  <span data-ttu-id="a4f2e-196">Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-196">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="a4f2e-197">6: hello éles környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="a4f2e-197">6: Change hello Production deployment</span></span>
<span data-ttu-id="a4f2e-198">Ez a lépés csak toocloud szolgáltatásokat, nem a webalkalmazások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-198">This step applies only toocloud services, not web apps.</span></span> <span data-ttu-id="a4f2e-199">Ha készen áll, előléptetheti hello kiválasztásával hello átmeneti toohello üzemi környezet **felcserélése** hello a klasszikus Azure portálon gombjára.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-199">When you are ready, you can promote hello Staging environment toohello production environment by choosing hello **Swap** button in hello Azure classic portal.</span></span> <span data-ttu-id="a4f2e-200">újonnan telepített hello átmeneti környezet előléptetett tooProduction, és hello előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-200">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="a4f2e-201">hello aktív központi telepítéssel hello üzemi és átmeneti környezetek eltérőek lehetnek, de hello üzembe helyezési előzményeket legutóbbi buildek van hello azonos környezetben függetlenül.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-201">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="a4f2e-202">7: egység tesztek futtatása</span><span class="sxs-lookup"><span data-stu-id="a4f2e-202">7: Run unit tests</span></span>
<span data-ttu-id="a4f2e-203">Ez a lépés csak tooweb alkalmazásokat, nem a felhőszolgáltatások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-203">This step applies only tooweb apps, not cloud services.</span></span> <span data-ttu-id="a4f2e-204">Ha azt elmulasztják, leállíthatja, hello telepítési és tooput a telepítés minőségi kaput, is futtathatók egység tesztek.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-204">tooput a quality gate on your deployment, you can run unit tests and if they fail, you can stop hello deployment.</span></span>

1. <span data-ttu-id="a4f2e-205">A Visual Studio egy egység teszt projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-205">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="a4f2e-206">Adja hozzá a projekt hivatkozásainak toohello projekt tootest szeretné.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-206">Add project references toohello project you want tootest.</span></span>
   
   ![][40]
3. <span data-ttu-id="a4f2e-207">Adja hozzá az egyes egység tesztek.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-207">Add some unit tests.</span></span> <span data-ttu-id="a4f2e-208">tooget elindult, próbálkozzon egy előzetes vizsgálat mindig adja meg, hogy.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-208">tooget started, try a dummy test that will always pass.</span></span>
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. <span data-ttu-id="a4f2e-209">Hello build definíciójának szerkesztése, válassza a hello **folyamat** lapot, és bontsa ki a hello **teszt** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-209">Edit hello build definition, choose hello **Process** tab, and expand hello **Test** node.</span></span>
5. <span data-ttu-id="a4f2e-210">Set hello **tesztje sikertelen sikertelen épülő** tooTrue.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-210">Set hello **Fail build on test failure** tooTrue.</span></span> <span data-ttu-id="a4f2e-211">Ez azt jelenti, hogy hello telepítési nem fordulhat elő, ha a vizsgálatok sikeresek hello.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-211">This means that hello deployment won't occur unless hello tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="a4f2e-212">Várólista új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-212">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="a4f2e-213">Hello build folyik, amíg a folyamat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-213">While hello build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="a4f2e-214">Hello build történik, hogy hello teszteredmények.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-214">When hello build is done, check hello test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="a4f2e-215">Próbálja meg létrehozni egy tesztet, amely sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-215">Try creating a test that will fail.</span></span> <span data-ttu-id="a4f2e-216">Első hozzáadása egy új teszt hello másolásával, nevezze át és megjegyzéssé hello kódsort arról, hogy NotImplementedException várt kivétel.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-216">Add a new test by copying hello first one, rename it, and comment out hello line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="a4f2e-217">Az ellenőrzéshez hello tooqueue új buildverziót módosítása.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-217">Check in hello change tooqueue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="a4f2e-218">Hello vizsgálati eredmények toosee részleteinek megtekintése hello hiba.</span><span class="sxs-lookup"><span data-stu-id="a4f2e-218">View hello test results toosee details about hello failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="a4f2e-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4f2e-219">Next steps</span></span>
<span data-ttu-id="a4f2e-220">A Visual Studio Team Services tesztelés egység kapcsolatban bővebben lásd: [egység tesztek futtatása a építés](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-220">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="a4f2e-221">Ha a Git használata esetén lásd: [megosztani a Git kód](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és [folyamatos üzembe helyezés tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-221">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="a4f2e-222">További információ a Visual Studio Team Services: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="a4f2e-222">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
