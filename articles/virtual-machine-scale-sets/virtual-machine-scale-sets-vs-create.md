---
title: "Telepítheti a virtuálisgép-méretezési készlet Visual Studio használatával |} Microsoft Docs"
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
ms.openlocfilehash: 78a4b0c8d305f57f495402cecb92d18425ff6bff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a><span data-ttu-id="069b0-103">A virtuálisgép-méretezési készlet létrehozása a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="069b0-103">How to create a Virtual Machine Scale Set with Visual Studio</span></span>
<span data-ttu-id="069b0-104">Ez a cikk bemutatja, hogyan telepítheti egy Azure virtuális gépek méretezési csoportján Visual Studio erőforrás csoport központi telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="069b0-104">This article shows you how to deploy an Azure Virtual Machine Scale Set using a Visual Studio Resource Group Deployment.</span></span>

<span data-ttu-id="069b0-105">[Az Azure virtuálisgép-méretezési csoportok](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) olyan Azure számítási erőforrás telepítéséhez és a hasonló virtuális gépek automatikus méretezése a gyűjteményeinek kezelését és a terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="069b0-105">[Azure Virtual Machine Scale Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) is an Azure Compute resource to deploy and manage a collection of similar virtual machines with auto-scale and load balancing.</span></span> <span data-ttu-id="069b0-106">Kioszthatja és központi telepítése használatával virtuálisgép-méretezési csoportok [Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="069b0-106">You can provision and deploy Virtual Machine Scale Sets using [Azure Resource Manager Templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="069b0-107">Az Azure Resource Manager-sablonok az Azure parancssori felület, PowerShell, REST telepíthető és is közvetlenül a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="069b0-107">Azure Resource Manager Templates can be deployed using Azure CLI, PowerShell, REST and also directly from Visual Studio.</span></span> <span data-ttu-id="069b0-108">A Visual Studio számos példa sablonok, amelyek az Azure erőforrás-csoport központi telepítésének projektek részeként telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="069b0-108">Visual Studio provides a set of example templates, which can be deployed as part of an Azure Resource Group Deployment project.</span></span>

<span data-ttu-id="069b0-109">Azure-erőforráscsoport központi telepítéseket, amelyek egy csoportot, és tegye közzé a kapcsolódó Azure-erőforrások olyan készletét egyetlen központi telepítési művelettel.</span><span class="sxs-lookup"><span data-stu-id="069b0-109">Azure Resource Group deployments are a way to group and publish a set of related Azure resources in a single deployment operation.</span></span> <span data-ttu-id="069b0-110">További róluk itt: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="069b0-110">You can learn more about them here: [Creating and deploying Azure resource groups through Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="069b0-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="069b0-111">Pre-requisites</span></span>
<span data-ttu-id="069b0-112">A Visual Studio virtuálisgép-méretezési csoportok üzembe helyezésének első lépései, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="069b0-112">To get started deploying Virtual Machine Scale Sets in Visual Studio, you need the following:</span></span>

* <span data-ttu-id="069b0-113">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="069b0-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="069b0-114">Az Azure SDK 2.7, 2.8 vagy 2.9</span><span class="sxs-lookup"><span data-stu-id="069b0-114">Azure SDK 2.7, 2.8 or 2.9</span></span>

>[!NOTE]
><span data-ttu-id="069b0-115">Ezek az utasítások azt feltételezik, hogy a Visual Studio használata [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span><span class="sxs-lookup"><span data-stu-id="069b0-115">These instructions assume you are using Visual Studio with [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span></span>

## <a name="creating-a-project"></a><span data-ttu-id="069b0-116">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="069b0-116">Creating a Project</span></span>
1. <span data-ttu-id="069b0-117">Hozzon létre egy új projektet a Visual Studio kiválasztásával **fájl |} Új |} Projekt**.</span><span class="sxs-lookup"><span data-stu-id="069b0-117">Create a new project in Visual Studio by choosing **File | New | Project**.</span></span>
   
    ![Új fájl][file_new]

2. <span data-ttu-id="069b0-119">A **Visual C# |} Felhő**, válassza a **Azure Resource Manager** projekt telepítéséhez az Azure Resource Manager-sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="069b0-119">Under **Visual C# | Cloud**, choose **Azure Resource Manager** to create a project for deploying an Azure Resource Manager Template.</span></span>
   
    ![Projekt létrehozása][create_project]

3. <span data-ttu-id="069b0-121">A sablonok listáján válassza ki a Linux vagy a Windows virtuális gép méretezési beállítása sablon.</span><span class="sxs-lookup"><span data-stu-id="069b0-121">From the list of Templates, select either the Linux or Windows Virtual Machine Scale Set Template.</span></span>
   
   ![Sablon kiválasztása][select_Template]

4. <span data-ttu-id="069b0-123">A projekt létrehozása után megjelenik PowerShell telepítési parancsfájlok, az Azure Resource Manager-sablon vagy egy paraméterfájl a virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="069b0-123">Once your project is created you see PowerShell deployment scripts, an Azure Resource Manager Template, and a parameter file for the Virtual Machine Scale Set.</span></span>
   
    ![Megoldáskezelő][solution_explorer]

## <a name="customize-your-project"></a><span data-ttu-id="069b0-125">A projekt testreszabása</span><span class="sxs-lookup"><span data-stu-id="069b0-125">Customize your project</span></span>
<span data-ttu-id="069b0-126">Most szerkesztheti a sablont tartozó, az alkalmazás igényeinek, például a Virtuálisgép-bővítmény tulajdonságai hozzáadása vagy szerkesztése terheléselosztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="069b0-126">Now you can edit the Template to customize it for your application's needs, such as adding VM extension properties or editing load balancing rules.</span></span> <span data-ttu-id="069b0-127">Alapértelmezés szerint a központi telepítése a AzureDiagnostics bővítményt, amely megkönnyíti az automatikus skálázási szabályok hozzáadása a virtuális gép méretezési beállítása sablonok vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="069b0-127">By default the Virtual Machine Scale Set Templates are configured to deploy the AzureDiagnostics extension, which makes it easy to add autoscale rules.</span></span> <span data-ttu-id="069b0-128">Emellett központilag telepíti a terheléselosztó a nyilvános IP-cím, a bejövő NAT-szabályok konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="069b0-128">It also deploys a load balancer with a public IP address, configured with inbound NAT rules.</span></span> 

<span data-ttu-id="069b0-129">A load balancer amellyel SSH (Linux) vagy RDP (Windows) a Virtuálisgép-példányok csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="069b0-129">The load balancer lets you connect to the VM instances with SSH (Linux) or RDP (Windows).</span></span> <span data-ttu-id="069b0-130">Az előtér-porttartomány 50000 kezdődik.</span><span class="sxs-lookup"><span data-stu-id="069b0-130">The front-end port range starts at 50000.</span></span> <span data-ttu-id="069b0-131">Ez azt jelenti, hogy ha Linux SSH port 50000, hogy meg legyenek átirányítva a méretezési csoportban lévő első VM 22-es portot.</span><span class="sxs-lookup"><span data-stu-id="069b0-131">For linux this means that if you SSH to port 50000, you are routed to port 22 of the first VM in the Scale Set.</span></span> <span data-ttu-id="069b0-132">Kapcsolódás port 50001 irányítása 22-es portot a második virtuális gép és így tovább.</span><span class="sxs-lookup"><span data-stu-id="069b0-132">Connecting to port 50001 is routed to port 22 of the second VM and so on.</span></span>

 <span data-ttu-id="069b0-133">Egy jó módszer a Visual Studio a sablonok szerkesztése, hogy a JSON-vázlat használja a paraméterek, a változók és az erőforrások rendszerezéséhez.</span><span class="sxs-lookup"><span data-stu-id="069b0-133">A good way to edit your Templates with Visual Studio is to use the JSON Outline to organize the parameters, variables, and resources.</span></span> <span data-ttu-id="069b0-134">Megismerhesse a séma, a Visual Studio felhívhatják a sablonban hibák telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="069b0-134">With an understanding of the schema Visual Studio can point out errors in your Template before you deploy it.</span></span>

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a><span data-ttu-id="069b0-136">A projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="069b0-136">Deploy the project</span></span>
1. <span data-ttu-id="069b0-137">A virtuálisgép-méretezési csoport erőforrás létrehozása az Azure Resource Manager-sablon telepítése.</span><span class="sxs-lookup"><span data-stu-id="069b0-137">Deploy the Azure Resource Manager Template to create the Virtual Machine Scale Set resource.</span></span> <span data-ttu-id="069b0-138">Kattintson a jobb gombbal a projektcsomópontra, és válassza a **telepítés |} Új központi telepítési**.</span><span class="sxs-lookup"><span data-stu-id="069b0-138">Right-click on the project node and choose **Deploy | New Deployment**.</span></span>
   
    ![Sablon üzembe helyezése][5deploy_Template]
    
2. <span data-ttu-id="069b0-140">A "Központi telepítése az erőforráscsoport" párbeszédpanelen jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="069b0-140">Select your subscription in the “Deploy to Resource Group” dialog.</span></span>
   
    ![Sablon üzembe helyezése][6deploy_Template]

3. <span data-ttu-id="069b0-142">Itt létrehozhat egy Azure-erőforráscsoportot, hogy a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="069b0-142">From here, you can create an Azure Resource Group to deploy your Template to.</span></span>
   
    ![Új erőforráscsoport][new_resource]

4. <span data-ttu-id="069b0-144">Ezután kattintson **paraméterek szerkesztése** a sablon átadott paraméterek.</span><span class="sxs-lookup"><span data-stu-id="069b0-144">Next, click **Edit Parameters** to enter parameters that are passed to your Template.</span></span> <span data-ttu-id="069b0-145">Az operációs rendszer, amely azonban szükséges a központi telepítés létrehozásához adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="069b0-145">Provide the username and password for the OS, which is required to create the deployment.</span></span> <span data-ttu-id="069b0-146">Ha nincs PowerShell eszközök telepített Visual Studio, javasoljuk, hogy ellenőrizze **menthetik a jelszavakat** elkerülése érdekében a rejtett PowerShell parancssorba, vagy használjon [keyvault támogatási](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span><span class="sxs-lookup"><span data-stu-id="069b0-146">If you don't have PowerShell Tools for Visual Studio installed, it is recommended to check **Save passwords** to avoid a hidden PowerShell command-line prompt, or use [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span></span>
   
    ![Paraméterek szerkesztése][edit_parameters]

5. <span data-ttu-id="069b0-148">Most kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="069b0-148">Now click **Deploy**.</span></span> <span data-ttu-id="069b0-149">A **kimeneti** ablak a központi telepítés végrehajtási állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="069b0-149">The **Output** window shows the deployment progress.</span></span> <span data-ttu-id="069b0-150">Vegye figyelembe, hogy a művelet végrehajtása történik a **telepítés-AzureResourceGroup.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="069b0-150">Note that the action is executing the **Deploy-AzureResourceGroup.ps1** script.</span></span>
   
   ![Kimeneti ablak][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a><span data-ttu-id="069b0-152">A virtuálisgép-méretezési csoport felfedezése</span><span class="sxs-lookup"><span data-stu-id="069b0-152">Exploring your Virtual Machine Scale Set</span></span>
<span data-ttu-id="069b0-153">Miután a telepítés befejeződik, megtekintheti az új virtuálisgép-méretezési csoportban a Visual Studio **Cloud Explorer** (frissítse a listát).</span><span class="sxs-lookup"><span data-stu-id="069b0-153">Once the deployment completes, you can view the new Virtual Machine Scale Set in the Visual Studio **Cloud Explorer** (refresh the list).</span></span> <span data-ttu-id="069b0-154">Cloud Explorer lehetővé teszi a Visual Studio Azure-erőforrások kezeléséhez alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="069b0-154">Cloud Explorer lets you manage Azure resources in Visual Studio while developing applications.</span></span> <span data-ttu-id="069b0-155">Megtekintheti a virtuális gép méretezési csoportban lévő a [Azure-portálon](https://portal.azure.com) és [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="069b0-155">You can also view your Virtual Machine Scale Set in the [Azure portal](https://portal.azure.com) and [Azure Resource Explorer](https://resources.azure.com/).</span></span>

![Cloud Explorer][cloud_explorer]

 <span data-ttu-id="069b0-157">A portál biztosítja a legjobb módszer az Azure infrastruktúra egy webböngészővel rendelkező vizuálisan kezelését, míg jogosultságot ad az ablak azokat a "példányait tartalmazó nézetet" és a PowerShell-parancsokat is megjelenítő szolgál az Azure erőforrás-kezelő egyszerűen megismerheti és hibakeresése az Azure-erőforrások, az erőforrások nézi.</span><span class="sxs-lookup"><span data-stu-id="069b0-157">The portal provides the best way to visually manage your Azure infrastructure with a web browser, while Azure Resource Explorer provides an easy way to explore and debug Azure resources, giving a window into the "instance view" and also showing PowerShell commands for the resources you are looking at.</span></span>

## <a name="next-steps"></a><span data-ttu-id="069b0-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="069b0-158">Next steps</span></span>
<span data-ttu-id="069b0-159">Miután sikeresen telepítése a Visual Studio alkalmazással virtuálisgép-méretezési csoportok, a projekt alkalmazás igényeinek még jobban testre szabható.</span><span class="sxs-lookup"><span data-stu-id="069b0-159">Once you’ve successfully deployed Virtual Machine Scale Sets through Visual Studio, you can further customize your project to suit your application requirements.</span></span> <span data-ttu-id="069b0-160">Például állítsa be automatikus méretezése hozzáadásával egy **Insights** erőforrás, infrastruktúra ad hozzá a sablont, (például önálló virtuális gépek), vagy az egyéni parancsprogramok futtatására szolgáló bővítmény használatával alkalmazások központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="069b0-160">For example, configure auto-scale by adding an **Insights** resource, adding infrastructure to your Template (like standalone VMs), or deploying applications using the custom script extension.</span></span> <span data-ttu-id="069b0-161">Jó példa sablonok találhatók a [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates) GitHub-adattár ("vmss" keresése).</span><span class="sxs-lookup"><span data-stu-id="069b0-161">Good example templates can be found in the [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) GitHub repository (search for "vmss").</span></span>

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
