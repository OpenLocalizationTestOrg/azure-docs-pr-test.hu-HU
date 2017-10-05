---
title: "A Visual Studio Team Services, Azure-ban folyamatos kézbesítési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja a Visual Studio Team Services csapatprojektek automatikusan létrehozásához, és a webes alkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások telepítéséhez."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a><span data-ttu-id="25bcf-103">Folyamatos készregyártás az Azure-ban a Visual Studio Team Services-zel</span><span class="sxs-lookup"><span data-stu-id="25bcf-103">Continuous delivery to Azure using Visual Studio Team Services</span></span>
<span data-ttu-id="25bcf-104">A Visual Studio Team Services csapatprojektek automatikusan létrehozása és telepítése az Azure web apps, vagy a felhőalapú szolgáltatások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25bcf-104">You can configure your Visual Studio Team Services team projects to automatically build and deploy to Azure web apps or cloud services.</span></span>  <span data-ttu-id="25bcf-105">(Folyamatos build beállítása és telepítése a rendszer használatával kapcsolatos egy *helyszíni* a Team Foundation Server, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="25bcf-105">(For information on how to set up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="25bcf-106">Ez az oktatóanyag feltételezi, hogy rendelkezik a Visual Studio 2013 és az Azure SDK telepítése.</span><span class="sxs-lookup"><span data-stu-id="25bcf-106">This tutorial assumes you have Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="25bcf-107">Ha még nem rendelkezik a Visual Studio 2013, töltse le kiválasztásával a **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="25bcf-107">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="25bcf-108">Az Azure SDK telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="25bcf-108">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="25bcf-109">Az oktatóanyag elvégzéséhez egy Visual Studio Team Services-fiók szükséges: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="25bcf-109">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="25bcf-110">Egy felhőalapú szolgáltatás automatikusan építsenek, és telepítse az Azure, a Visual Studio Team Services telepítéséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="25bcf-110">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="25bcf-111">1: team-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="25bcf-111">1: Create a team project</span></span>
<span data-ttu-id="25bcf-112">Kövesse az utasításokat [Itt](http://go.microsoft.com/fwlink/?LinkId=512980) a csapatprojekt létrehozásához, és csatolja a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25bcf-112">Follow the instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) to create your team project and link it to Visual Studio.</span></span> <span data-ttu-id="25bcf-113">Ez a forgatókönyv azt feltételezi, hogy a forrás vezérlő megoldásként használja a Team Foundation verzió vezérlő (TFVC).</span><span class="sxs-lookup"><span data-stu-id="25bcf-113">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="25bcf-114">Ha szeretné a Git verziókövető, lásd: [Ez a forgatókönyv Git verziójának](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="25bcf-114">If you want to use Git for version control, see [the Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-to-source-control"></a><span data-ttu-id="25bcf-115">2: a verziókövetési projektet</span><span class="sxs-lookup"><span data-stu-id="25bcf-115">2: Check in a project to source control</span></span>
1. <span data-ttu-id="25bcf-116">A Visual Studióban nyissa meg a megoldást, amelyet központilag telepíteni szeretne, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="25bcf-116">In Visual Studio, open the solution you want to deploy, or create a new one.</span></span>
   <span data-ttu-id="25bcf-117">A bemutatóban szereplő lépéseket követve telepítheti a webes alkalmazás vagy a felhőszolgáltatás (Azure-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="25bcf-117">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span>
   <span data-ttu-id="25bcf-118">Ha szeretne létrehozni egy új megoldást, hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt.</span><span class="sxs-lookup"><span data-stu-id="25bcf-118">If you want to create a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="25bcf-119">Győződjön meg arról, hogy a projekt irányul-e a .NET-keretrendszer 4 vagy 4.5 és a felhőszolgáltatás-projekt létrehozásakor, egy ASP.NET MVC webes és feldolgozói szerepkörök hozzáadása, és válassza a webes szerepkör az internetes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="25bcf-119">Make sure that the project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for the web role.</span></span> <span data-ttu-id="25bcf-120">Amikor a rendszer kéri, válassza ki a **az internetes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-120">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="25bcf-121">Ha szeretne létrehozni egy webalkalmazást, válassza ki az ASP.NET-webalkalmazás projekt sablont, és válassza a MVC.</span><span class="sxs-lookup"><span data-stu-id="25bcf-121">If you want to create a web app, choose the ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="25bcf-122">Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="25bcf-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="25bcf-123">Visual Studio Team Services csak a jelenleg támogatott CI központi telepítések, a Visual Studio-webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="25bcf-123">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="25bcf-124">Webhely-projektek kívül esnek a hatókörön.</span><span class="sxs-lookup"><span data-stu-id="25bcf-124">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="25bcf-125">Nyissa meg a helyi menüben a megoldás, és válassza a **megoldás hozzáadása a verziókövetési**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-125">Open the context menu for the solution, and choose **Add Solution to Source Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="25bcf-126">Fogadja el vagy módosítsa az alapértelmezett beállításokat, és válassza ki a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-126">Accept or change the defaults and choose the **OK** button.</span></span> <span data-ttu-id="25bcf-127">Amennyiben az a folyamat befejeződik, forrás vezérlő ikonok jelennek meg **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-127">Once the process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="25bcf-128">Nyissa meg a helyi menüben a megoldás, és válassza a **felvétel**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-128">Open the shortcut menu for the solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="25bcf-129">Az a **függőben lévő módosítások** területén **Team Explorer**, írja be a jelölőnégyzetet a Megjegyzés, és válassza ki a **felvétel** gombra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-129">In the **Pending Changes** area of **Team Explorer**, type a comment for the check-in and choose the **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="25bcf-130">Vegye figyelembe a beállításokat, vagy adott módosítások kizárja a verziókezelőbe mentésekor.</span><span class="sxs-lookup"><span data-stu-id="25bcf-130">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="25bcf-131">Ha szükséges, nem tartoznak a módosítások, válassza ki azt a **tartalmazza az összes** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-131">If desired changes are excluded, choose the **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="25bcf-132">3: a projekt csatlakozzon az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="25bcf-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="25bcf-133">Most, hogy a Visual STUDIO Team Services csapatprojekt néhány adatforrás kóddal azt, készen áll a csapatprojekt csatlakozzon az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="25bcf-133">Now that you have a VS Team Services team project with some source code in it, you are ready to connect your team project to Azure.</span></span>  <span data-ttu-id="25bcf-134">A a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat, válassza ki a  **+**  ikonra a lap alján maradt, és válassza **Felhőszolgáltatás** vagy **webalkalmazás** , majd **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the **+** icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="25bcf-135">Válassza ki a **Visual Studio Team Services való közzététel beállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-135">Choose the **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="25bcf-136">A varázslót, írja be a szövegmezőbe, majd kattintson a Visual Studio Team Services fiókja nevét a **engedélyezik most** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-136">In the wizard, type the name of your Visual Studio Team Services account in the textbox and click the **Authorize Now** link.</span></span> <span data-ttu-id="25bcf-137">Jelentkezzen be a kérheti.</span><span class="sxs-lookup"><span data-stu-id="25bcf-137">You might be asked to sign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="25bcf-138">Az a **kapcsolatkérelem** előugró párbeszédpanelen válassza ki a **elfogadás** gombra a Visual STUDIO Team Services a csapatprojekt konfigurálása Azure engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="25bcf-138">In the **Connection Request** pop-up dialog, choose the **Accept** button to authorize Azure to configure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="25bcf-139">Ha engedélyezési sikeres, megjelenik egy legördülő lista a Visual Studio Team Services csapatprojektek listáját tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="25bcf-139">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="25bcf-140">Válassza ki az előző lépésben létrehozott csapatprojekt nevét, és kattintson a pipa gombra a varázsló.</span><span class="sxs-lookup"><span data-stu-id="25bcf-140">Choose  the name of team project that you created in the previous steps, and then choose the wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="25bcf-141">Után a projekt ki van kapcsolva, néhány utasítások a Visual Studio Team Services csapatprojekt megváltozik ellenőrzése a következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="25bcf-141">After your project is linked, you will see some instructions for checking in changes to your Visual Studio Team Services team project.</span></span>  <span data-ttu-id="25bcf-142">A következő bejelentkezéskor, a Visual Studio Team Services elkészíti és a projekt telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="25bcf-142">On your next check-in, Visual Studio Team Services will build and deploy your project to Azure.</span></span>  <span data-ttu-id="25bcf-143">Ez most ikonjára kattintva próbálkozzon a **ellenőrizze a Visual Studio** hivatkozásra, majd a **indítsa el a Visual Studio** hivatkozásra (vagy ezzel egyenértékű **Visual Studio** gomb alsó részén a Letöltés gombra).</span><span class="sxs-lookup"><span data-stu-id="25bcf-143">Try this now by clicking the **Check In from Visual Studio** link, and then the **Launch Visual Studio** link (or the equivalent **Visual Studio** button at the bottom of the portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="25bcf-144">4: egy újjáépítését indítja, és telepítse újra a projekthez</span><span class="sxs-lookup"><span data-stu-id="25bcf-144">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="25bcf-145">A Visual Studio **Team Explorer**, válassza ki a **forrás vezérlő Explorer** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-145">In Visual Studio's **Team Explorer**, choose the **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="25bcf-146">Keresse meg a megoldásfájlt, majd nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="25bcf-146">Navigate to your solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="25bcf-147">A **Megoldáskezelőben**, nyissa meg a fájlt, és módosítsa azt.</span><span class="sxs-lookup"><span data-stu-id="25bcf-147">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="25bcf-148">Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\egy MVC webes szerepkör megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="25bcf-148">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="25bcf-149">Az embléma a hely szerkesztése, és válassza a **Ctrl + S** fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="25bcf-149">Edit the logo for the site and choose **Ctrl+S** to save the file.</span></span>
   
    ![][18]
5. <span data-ttu-id="25bcf-150">A **Team Explorer**, válassza ki a **függőben lévő módosítások** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-150">In **Team Explorer**, choose the **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="25bcf-151">Írjon egy megjegyzést, és válassza ki a **felvétel** gombra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-151">Enter a comment and then choose the **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="25bcf-152">Válassza ki a **otthoni** gombra kattintva visszatérhet a **Team Explorer** kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="25bcf-152">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="25bcf-153">Válassza ki a **buildek** hivatkozásra a buildek megtekintéséhez folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="25bcf-153">Choose the **Builds** link to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="25bcf-154">**Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="25bcf-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="25bcf-155">Kattintson duplán a összeállítása folyamatban van a részletes napló megtekintéséhez a build előrehaladtával nevére.</span><span class="sxs-lookup"><span data-stu-id="25bcf-155">Double-click the name of the build in progress to view a detailed log as the build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="25bcf-156">Bár a összeállítása folyamatban, tekintse meg a build-definíciót, amely jött létre, amikor a varázsló segítségével az Azure-bA a TFS kapcsolódó..</span><span class="sxs-lookup"><span data-stu-id="25bcf-156">While the build is in-progress, take a look at the build definition that was created when you linked TFS to Azure by using the wizard.</span></span>  <span data-ttu-id="25bcf-157">Nyissa meg a helyi menüben a build definíciójához, és válassza a **Build definíciójának szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-157">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="25bcf-158">Az a **eseményindító** lapon látni fogja, hogy a build definition létrehozásához, minden egyes bejelentkezéskor értéke alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="25bcf-158">In the **Trigger** tab, you will see that the build definition is set to build on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="25bcf-159">Az a **folyamat** lapon megtekintheti a telepítési környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="25bcf-159">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span> <span data-ttu-id="25bcf-160">Ha a webalkalmazásokkal dolgozik, látható tulajdonságainak eltérő itt látható lesz.</span><span class="sxs-lookup"><span data-stu-id="25bcf-160">If you are working with web apps, the properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="25bcf-161">Adja meg a tulajdonságok értékeit, ha azt szeretné, hogy a különböző értékeket, mint az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="25bcf-161">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="25bcf-162">Az Azure közzétételi tulajdonságok szerepelnek a **telepítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="25bcf-162">The properties for Azure publishing are in the **Deployment** section.</span></span>
    
     <span data-ttu-id="25bcf-163">Az alábbi táblázat a rendelkezésre álló tulajdonságok a **telepítési** szakasz:</span><span class="sxs-lookup"><span data-stu-id="25bcf-163">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="25bcf-164">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="25bcf-164">Property</span></span> | <span data-ttu-id="25bcf-165">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="25bcf-165">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="25bcf-166">Nem megbízható tanúsítványok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="25bcf-166">Allow Untrusted Certificates</span></span> |<span data-ttu-id="25bcf-167">Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni.</span><span class="sxs-lookup"><span data-stu-id="25bcf-167">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="25bcf-168">Frissítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="25bcf-168">Allow Upgrade</span></span> |<span data-ttu-id="25bcf-169">Lehetővé teszi, hogy a központi telepítés helyett újat hoz létre egy meglévő üzemelő példány frissítése.</span><span class="sxs-lookup"><span data-stu-id="25bcf-169">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="25bcf-170">Megőrzi az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="25bcf-170">Preserves the IP address.</span></span> |
    | <span data-ttu-id="25bcf-171">Ne törölje</span><span class="sxs-lookup"><span data-stu-id="25bcf-171">Do Not Delete</span></span> |<span data-ttu-id="25bcf-172">Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett).</span><span class="sxs-lookup"><span data-stu-id="25bcf-172">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="25bcf-173">A telepítési beállítások elérési útja</span><span class="sxs-lookup"><span data-stu-id="25bcf-173">Path to Deployment Settings</span></span> |<span data-ttu-id="25bcf-174">A fájl elérési útját a .pubxml webes alkalmazások esetén a tárház gyökérkönyvtárában viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="25bcf-174">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="25bcf-175">Cloud services csomag figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="25bcf-175">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="25bcf-176">SharePoint-környezet</span><span class="sxs-lookup"><span data-stu-id="25bcf-176">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="25bcf-177">Ugyanaz, mint a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="25bcf-177">The same as the service name.</span></span> |
    | <span data-ttu-id="25bcf-178">Az Azure-telepítés környezet</span><span class="sxs-lookup"><span data-stu-id="25bcf-178">Azure Deployment Environment</span></span> |<span data-ttu-id="25bcf-179">A webes alkalmazás vagy a felhőbeli szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="25bcf-179">The web app or cloud service name.</span></span> |
12. <span data-ttu-id="25bcf-180">Ha több szolgáltatáskonfiguráció (.cscfg-fájlok) használ, megadhatja a kívánt szolgáltatás konfigurációját az a **felépítéséhez, speciális, MSBuild-argumentumok** beállítást.</span><span class="sxs-lookup"><span data-stu-id="25bcf-180">If you are using multiple service configurations (.cscfg files), you can specify the desired service configuration in the **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="25bcf-181">Például ServiceConfiguration.Test.cscfg használatához állítsa MSBuild-argumentumok telepítés `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="25bcf-181">For example, to use ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="25bcf-182">Időpontig a build sikeresen meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="25bcf-182">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="25bcf-183">Ha duplán kattint a build nevét, a Visual Studio megjeleníti a **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.</span><span class="sxs-lookup"><span data-stu-id="25bcf-183">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="25bcf-184">Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), a tekintheti meg a társított központi telepítéshez a **központi telepítések** az átmeneti kiválasztásakor fülre.</span><span class="sxs-lookup"><span data-stu-id="25bcf-184">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="25bcf-185">Tallózással keresse meg a webhely URL-címe.</span><span class="sxs-lookup"><span data-stu-id="25bcf-185">Browse to your site's URL.</span></span> <span data-ttu-id="25bcf-186">A webalkalmazás, kattintson a **Tallózás** gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="25bcf-186">For a web app, just click the **Browse** button on the command bar.</span></span> <span data-ttu-id="25bcf-187">Egy felhőalapú szolgáltatás, válassza az URL-címet a **gyors áttekintése** szakasza a **irányítópult** oldal, amely bemutatja az átmeneti környezetben egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="25bcf-187">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment for a cloud service.</span></span> <span data-ttu-id="25bcf-188">A folyamatos integrációt szolgáltatásokhoz központi telepítések alapértelmezés szerint az átmeneti környezetben kerülnek közzétételre.</span><span class="sxs-lookup"><span data-stu-id="25bcf-188">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="25bcf-189">Ez módosítható úgy, hogy a **másik felhőalapú szolgáltatási környezet** tulajdonságot **éles**.</span><span class="sxs-lookup"><span data-stu-id="25bcf-189">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="25bcf-190">Ez képernyőkép jeleníti meg, ha a webhely URL-címe megtalálható-e a felhőalapú szolgáltatás irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="25bcf-190">This screenshot shows where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="25bcf-191">Hogy láthatóvá váljon a futó helyet egy új böngészőlapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="25bcf-191">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="25bcf-192">A felhőszolgáltatások, ha más módosításokat szeretne a projekt, akkor eseményindító több épít fel és meg felhalmozódnak több központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="25bcf-192">For cloud services, if you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="25bcf-193">A legújabb egy aktív jelölésű.</span><span class="sxs-lookup"><span data-stu-id="25bcf-193">The latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="25bcf-194">5: telepítse újra egy korábbi verzióját</span><span class="sxs-lookup"><span data-stu-id="25bcf-194">5: Redeploy an earlier build</span></span>
<span data-ttu-id="25bcf-195">Ez a lépés nem kötelező, és a felhőszolgáltatások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="25bcf-195">This step applies to cloud services and is optional.</span></span> <span data-ttu-id="25bcf-196">A klasszikus Azure portálon, válasszon egy korábbi üzemelő, és válassza a **újratelepíteni** a webhely egy korábbi be visszatekerés gombra.</span><span class="sxs-lookup"><span data-stu-id="25bcf-196">In the Azure classic portal, choose an earlier deployment and then choose the **Redeploy** button to rewind your site to an earlier check-in.</span></span>  <span data-ttu-id="25bcf-197">Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.</span><span class="sxs-lookup"><span data-stu-id="25bcf-197">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="25bcf-198">6: módosítsa az éles környezet</span><span class="sxs-lookup"><span data-stu-id="25bcf-198">6: Change the Production deployment</span></span>
<span data-ttu-id="25bcf-199">Ez a lépés csak a felhőszolgáltatások, nem a webalkalmazások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="25bcf-199">This step applies only to cloud services, not web apps.</span></span> <span data-ttu-id="25bcf-200">Ha készen áll, az éles környezetbe átmeneti környezet kiválasztásával előléptetheti az **felcserélése** gombra a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="25bcf-200">When you are ready, you can promote the Staging environment to the production environment by choosing the **Swap** button in the Azure classic portal.</span></span> <span data-ttu-id="25bcf-201">Az újonnan telepített átmeneti környezet éles van előléptetve, és az előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet.</span><span class="sxs-lookup"><span data-stu-id="25bcf-201">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="25bcf-202">Az aktív központi telepítéssel üzemi és átmeneti környezetek eltérőek lehetnek, de az üzembe helyezési előzményeket legutóbbi buildek környezet függetlenül ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="25bcf-202">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="25bcf-203">7: egység tesztek futtatása</span><span class="sxs-lookup"><span data-stu-id="25bcf-203">7: Run unit tests</span></span>
<span data-ttu-id="25bcf-204">Ez a lépés csak a webes alkalmazásokat, nem a felhőalapú szolgáltatások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="25bcf-204">This step applies only to web apps, not cloud services.</span></span> <span data-ttu-id="25bcf-205">Minőségi kaput elhelyezése a központi telepítés, egység tesztek futtatása, és ha azt elmulasztják, leállíthatja a központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="25bcf-205">To put a quality gate on your deployment, you can run unit tests and if they fail, you can stop the deployment.</span></span>

1. <span data-ttu-id="25bcf-206">A Visual Studio egy egység teszt projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="25bcf-206">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="25bcf-207">A vizsgálni kívánt projekt projekt hivatkozásokat adni.</span><span class="sxs-lookup"><span data-stu-id="25bcf-207">Add project references to the project you want to test.</span></span>
   
   ![][40]
3. <span data-ttu-id="25bcf-208">Adja hozzá az egyes egység tesztek.</span><span class="sxs-lookup"><span data-stu-id="25bcf-208">Add some unit tests.</span></span> <span data-ttu-id="25bcf-209">Először próbálja meg egy előzetes vizsgálat, amely mindig adjon át.</span><span class="sxs-lookup"><span data-stu-id="25bcf-209">To get started, try a dummy test that will always pass.</span></span>
   
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
4. <span data-ttu-id="25bcf-210">Szerkessze a build definíciót, válassza ki a **folyamat** lapot, és bontsa ki a **teszt** csomópont.</span><span class="sxs-lookup"><span data-stu-id="25bcf-210">Edit the build definition, choose the **Process** tab, and expand the **Test** node.</span></span>
5. <span data-ttu-id="25bcf-211">Állítsa be a **tesztje sikertelen sikertelen épülő** igaz értékre.</span><span class="sxs-lookup"><span data-stu-id="25bcf-211">Set the **Fail build on test failure** to True.</span></span> <span data-ttu-id="25bcf-212">Ez azt jelenti, hogy a telepítés nem fordulhat elő, ha a teszt sikeresen lezajlott.</span><span class="sxs-lookup"><span data-stu-id="25bcf-212">This means that the deployment won't occur unless the tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="25bcf-213">Várólista új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="25bcf-213">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="25bcf-214">A build folyik, amíg a folyamat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="25bcf-214">While the build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="25bcf-215">A build történik, ellenőrizze a teszteredmények.</span><span class="sxs-lookup"><span data-stu-id="25bcf-215">When the build is done, check the test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="25bcf-216">Próbálja meg létrehozni egy tesztet, amely sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="25bcf-216">Try creating a test that will fail.</span></span> <span data-ttu-id="25bcf-217">Új teszt hozzáadása az első címtárra másolásával, nevezze át és arról, hogy NotImplementedException ugyanakkor nem várt kódsort megjegyzéssé.</span><span class="sxs-lookup"><span data-stu-id="25bcf-217">Add a new test by copying the first one, rename it, and comment out the line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="25bcf-218">Ellenőrizze, hogy a módosítás várólistára új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="25bcf-218">Check in the change to queue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="25bcf-219">A hibával kapcsolatos részletek megtekintéséhez a teszteredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="25bcf-219">View the test results to see details about the failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="25bcf-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25bcf-220">Next steps</span></span>
<span data-ttu-id="25bcf-221">A Visual Studio Team Services tesztelés egység kapcsolatban bővebben lásd: [egység tesztek futtatása a építés](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="25bcf-221">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="25bcf-222">Ha a Git használata esetén lásd: [megosztani a Git kód](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és [folyamatos üzembe helyezés az Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="25bcf-222">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="25bcf-223">További információ a Visual Studio Team Services: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="25bcf-223">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
