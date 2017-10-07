---
title: "Virtuálisgép-méretezési készlet Visual Studio használatával aaaDeploy |} Microsoft Docs"
description: "Központi telepítése a Visual Studio és a Resource Manager-sablon használatával virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a><span data-ttu-id="c6ac7-103">Hogyan toocreate egy virtuálisgép-méretezési készlet a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6ac7-103">How toocreate a Virtual Machine Scale Set with Visual Studio</span></span>
<span data-ttu-id="c6ac7-104">Ez a cikk bemutatja, hogyan toodeploy Azure virtuálisgép-méretezési beállítása a Visual Studio erőforrás csoport központi telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-104">This article shows you how toodeploy an Azure Virtual Machine Scale Set using a Visual Studio Resource Group Deployment.</span></span>

<span data-ttu-id="c6ac7-105">[Az Azure virtuálisgép-méretezési csoportok](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) van egy Azure számítási erőforrás toodeploy és hasonló virtuális gépek automatikus méretezése a gyűjteményeinek kezelését, és a terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-105">[Azure Virtual Machine Scale Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) is an Azure Compute resource toodeploy and manage a collection of similar virtual machines with auto-scale and load balancing.</span></span> <span data-ttu-id="c6ac7-106">Kioszthatja és központi telepítése használatával virtuálisgép-méretezési csoportok [Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-106">You can provision and deploy Virtual Machine Scale Sets using [Azure Resource Manager Templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="c6ac7-107">Az Azure Resource Manager-sablonok az Azure parancssori felület, PowerShell, REST telepíthető és is közvetlenül a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-107">Azure Resource Manager Templates can be deployed using Azure CLI, PowerShell, REST and also directly from Visual Studio.</span></span> <span data-ttu-id="c6ac7-108">A Visual Studio számos példa sablonok, amelyek az Azure erőforrás-csoport központi telepítésének projektek részeként telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-108">Visual Studio provides a set of example templates, which can be deployed as part of an Azure Resource Group Deployment project.</span></span>

<span data-ttu-id="c6ac7-109">Azure-erőforráscsoport központi telepítések egy módon toogroup, és tegye közzé a kapcsolódó Azure-erőforrások olyan készletét egyetlen központi telepítési művelettel.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-109">Azure Resource Group deployments are a way toogroup and publish a set of related Azure resources in a single deployment operation.</span></span> <span data-ttu-id="c6ac7-110">További róluk itt: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-110">You can learn more about them here: [Creating and deploying Azure resource groups through Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c6ac7-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c6ac7-111">Pre-requisites</span></span>
<span data-ttu-id="c6ac7-112">a Visual Studio virtuálisgép-méretezési csoportok üzembe helyezésének lépései tooget, következőkre lesz szüksége hello:</span><span class="sxs-lookup"><span data-stu-id="c6ac7-112">tooget started deploying Virtual Machine Scale Sets in Visual Studio, you need hello following:</span></span>

* <span data-ttu-id="c6ac7-113">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="c6ac7-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="c6ac7-114">Az Azure SDK 2.7, 2.8 vagy 2.9</span><span class="sxs-lookup"><span data-stu-id="c6ac7-114">Azure SDK 2.7, 2.8 or 2.9</span></span>

>[!NOTE]
><span data-ttu-id="c6ac7-115">Ezek az utasítások azt feltételezik, hogy a Visual Studio használata [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-115">These instructions assume you are using Visual Studio with [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span></span>

## <a name="creating-a-project"></a><span data-ttu-id="c6ac7-116">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6ac7-116">Creating a Project</span></span>
1. <span data-ttu-id="c6ac7-117">Hozzon létre egy új projektet a Visual Studio kiválasztásával **fájl |} Új |} Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-117">Create a new project in Visual Studio by choosing **File | New | Project**.</span></span>
   
    ![Új fájl][file_new]

2. <span data-ttu-id="c6ac7-119">A **Visual C# |} Felhő**, válassza a **Azure Resource Manager** toocreate projekt telepítése egy Azure Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-119">Under **Visual C# | Cloud**, choose **Azure Resource Manager** toocreate a project for deploying an Azure Resource Manager Template.</span></span>
   
    ![Projekt létrehozása][create_project]

3. <span data-ttu-id="c6ac7-121">Válassza ki a sablonok hello listáról hello Linux vagy a Windows virtuális gép méretezési beállítása sablon.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-121">From hello list of Templates, select either hello Linux or Windows Virtual Machine Scale Set Template.</span></span>
   
   ![Sablon kiválasztása][select_Template]

4. <span data-ttu-id="c6ac7-123">A projekt létrehozása után megjelenik PowerShell üzembe helyezési parancsfájlok, az Azure Resource Manager-sablon és a virtuálisgép-méretezési csoport hello paraméter fájl.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-123">Once your project is created you see PowerShell deployment scripts, an Azure Resource Manager Template, and a parameter file for hello Virtual Machine Scale Set.</span></span>
   
    ![Megoldáskezelő][solution_explorer]

## <a name="customize-your-project"></a><span data-ttu-id="c6ac7-125">A projekt testreszabása</span><span class="sxs-lookup"><span data-stu-id="c6ac7-125">Customize your project</span></span>
<span data-ttu-id="c6ac7-126">Most hello sablon toocustomize módosíthatja azt az alkalmazás igényeinek, például a Virtuálisgép-bővítmény tulajdonságai hozzáadása vagy szerkesztése terheléselosztási szabályok betöltése.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-126">Now you can edit hello Template toocustomize it for your application's needs, such as adding VM extension properties or editing load balancing rules.</span></span> <span data-ttu-id="c6ac7-127">Alapértelmezés szerint a hello virtuális gép méretezési beállítása sablonok a konfigurált toodeploy hello AzureDiagnostics bővítményt, így könnyen tooadd automatikus skálázási szabályok.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-127">By default hello Virtual Machine Scale Set Templates are configured toodeploy hello AzureDiagnostics extension, which makes it easy tooadd autoscale rules.</span></span> <span data-ttu-id="c6ac7-128">Emellett központilag telepíti a terheléselosztó a nyilvános IP-cím, a bejövő NAT-szabályok konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-128">It also deploys a load balancer with a public IP address, configured with inbound NAT rules.</span></span> 

<span data-ttu-id="c6ac7-129">hello terheléselosztó lehetővé teszi a csatlakozást a Virtuálisgép-példányok toohello SSH (Linux) vagy RDP (Windows).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-129">hello load balancer lets you connect toohello VM instances with SSH (Linux) or RDP (Windows).</span></span> <span data-ttu-id="c6ac7-130">hello előtéri porttartománya 50000 kezdődik.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-130">hello front-end port range starts at 50000.</span></span> <span data-ttu-id="c6ac7-131">Linux Ez azt jelenti, hogy ha akkor SSH tooport 50000, irányított tooport 22 az első virtuális gép a virtuális gépek méretezési csoportjának hello hello.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-131">For linux this means that if you SSH tooport 50000, you are routed tooport 22 of hello first VM in hello Scale Set.</span></span> <span data-ttu-id="c6ac7-132">Csatlakozás tooport 50001 irányított tooport 22 hello virtuális gép második és így tovább.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-132">Connecting tooport 50001 is routed tooport 22 of hello second VM and so on.</span></span>

 <span data-ttu-id="c6ac7-133">Egy jó módszer tooedit a sablonokat, a Visual Studio toouse hello JSON-vázlat tooorganize hello paraméterek, változók és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-133">A good way tooedit your Templates with Visual Studio is toouse hello JSON Outline tooorganize hello parameters, variables, and resources.</span></span> <span data-ttu-id="c6ac7-134">Séma Visual Studio egy hello ismeretének mutathat hibát jelez a sablon telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-134">With an understanding of hello schema Visual Studio can point out errors in your Template before you deploy it.</span></span>

![JSON Explorer][json_explorer]

## <a name="deploy-hello-project"></a><span data-ttu-id="c6ac7-136">Hello projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="c6ac7-136">Deploy hello project</span></span>
1. <span data-ttu-id="c6ac7-137">Hello Azure Resource Manager-sablon toocreate hello virtuálisgép-méretezési csoport erőforrás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-137">Deploy hello Azure Resource Manager Template toocreate hello Virtual Machine Scale Set resource.</span></span> <span data-ttu-id="c6ac7-138">Kattintson jobb gombbal a projektcsomópontra hello, és válassza a **telepítés |} Új központi telepítési**.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-138">Right-click on hello project node and choose **Deploy | New Deployment**.</span></span>
   
    ![Sablon üzembe helyezése][5deploy_Template]
    
2. <span data-ttu-id="c6ac7-140">Hello "Központi telepítés tooResource csoport" párbeszédpanelen jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-140">Select your subscription in hello “Deploy tooResource Group” dialog.</span></span>
   
    ![Sablon üzembe helyezése][6deploy_Template]

3. <span data-ttu-id="c6ac7-142">Itt egy Azure-erőforráscsoport toodeploy a sablont hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-142">From here, you can create an Azure Resource Group toodeploy your Template to.</span></span>
   
    ![Új erőforráscsoport][new_resource]

4. <span data-ttu-id="c6ac7-144">Ezután kattintson **paraméterek szerkesztése** tooenter tooyour sablon átadott paraméterek.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-144">Next, click **Edit Parameters** tooenter parameters that are passed tooyour Template.</span></span> <span data-ttu-id="c6ac7-145">Adja meg az operációs rendszer, amely szükséges toocreate hello telepítési hello hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-145">Provide hello username and password for hello OS, which is required toocreate hello deployment.</span></span> <span data-ttu-id="c6ac7-146">Ha a PowerShell-eszközök nem rendelkezik a telepített Visual Studio, akkor ajánlott toocheck **menthetik a jelszavakat** tooavoid egy rejtett PowerShell parancssori kérni, vagy használjon [keyvault támogatási](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-146">If you don't have PowerShell Tools for Visual Studio installed, it is recommended toocheck **Save passwords** tooavoid a hidden PowerShell command-line prompt, or use [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span></span>
   
    ![Paraméterek szerkesztése][edit_parameters]

5. <span data-ttu-id="c6ac7-148">Most kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-148">Now click **Deploy**.</span></span> <span data-ttu-id="c6ac7-149">Hello **kimeneti** ablakban látható hello központi telepítésének állapotáról.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-149">hello **Output** window shows hello deployment progress.</span></span> <span data-ttu-id="c6ac7-150">Vegye figyelembe, hogy hello művelet végrehajtja az hello **telepítés-AzureResourceGroup.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-150">Note that hello action is executing hello **Deploy-AzureResourceGroup.ps1** script.</span></span>
   
   ![Kimeneti ablak][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a><span data-ttu-id="c6ac7-152">A virtuálisgép-méretezési csoport felfedezése</span><span class="sxs-lookup"><span data-stu-id="c6ac7-152">Exploring your Virtual Machine Scale Set</span></span>
<span data-ttu-id="c6ac7-153">Hello telepítése után megtekintheti hello új virtuálisgép-méretezési a Visual Studio hello **Cloud Explorer** (frissítési hello lista).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-153">Once hello deployment completes, you can view hello new Virtual Machine Scale Set in hello Visual Studio **Cloud Explorer** (refresh hello list).</span></span> <span data-ttu-id="c6ac7-154">Cloud Explorer lehetővé teszi a Visual Studio Azure-erőforrások kezeléséhez alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-154">Cloud Explorer lets you manage Azure resources in Visual Studio while developing applications.</span></span> <span data-ttu-id="c6ac7-155">Megtekintheti a virtuálisgép-méretezési csoportban lévő hello [Azure-portálon](https://portal.azure.com) és [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-155">You can also view your Virtual Machine Scale Set in hello [Azure portal](https://portal.azure.com) and [Azure Resource Explorer](https://resources.azure.com/).</span></span>

![Cloud Explorer][cloud_explorer]

 <span data-ttu-id="c6ac7-157">hello portal hello toovisually kezelése az Azure-infrastruktúra egy webböngészővel rendelkező, míg az Azure erőforrás-kezelő kínál egy egyszerűen tooexplore legjobb megoldást kínál és hibakeresése az Azure-erőforrások, egyúttal jogosultságot ad az ablak "példány megtekintése" hello és is megjeleníti a PowerShell az éppen megtekintett hello erőforrások parancsok.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-157">hello portal provides hello best way toovisually manage your Azure infrastructure with a web browser, while Azure Resource Explorer provides an easy way tooexplore and debug Azure resources, giving a window into hello "instance view" and also showing PowerShell commands for hello resources you are looking at.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6ac7-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6ac7-158">Next steps</span></span>
<span data-ttu-id="c6ac7-159">Miután sikeresen telepítése a Visual Studio alkalmazással virtuálisgép-méretezési csoportok, még jobban testre szabható a projekt toosuit alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-159">Once you’ve successfully deployed Virtual Machine Scale Sets through Visual Studio, you can further customize your project toosuit your application requirements.</span></span> <span data-ttu-id="c6ac7-160">Például állítsa be automatikus méretezése hozzáadásával egy **Insights** erőforrás, infrastruktúra tooyour sablon (például önálló virtuális gépek) hozzáadásakor, vagy egyéni parancsprogramok futtatására szolgáló bővítmény hello használó alkalmazások telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="c6ac7-160">For example, configure auto-scale by adding an **Insights** resource, adding infrastructure tooyour Template (like standalone VMs), or deploying applications using hello custom script extension.</span></span> <span data-ttu-id="c6ac7-161">Jó példa sablonok találhatók hello [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates) GitHub-adattár ("vmss" keresése).</span><span class="sxs-lookup"><span data-stu-id="c6ac7-161">Good example templates can be found in hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) GitHub repository (search for "vmss").</span></span>

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
