---
title: az Azure-ban Team Services CI/CD adatcsatorna aaaCreate |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate a Visual Studio Team Services a következő feldolgozási sorban folyamatos integrációt és kézbesítését, hogy egy webes alkalmazás tooIIS egy Windows virtuális gépre telepíti"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a><span data-ttu-id="0031e-103">Visual Studio Team Services és az IIS egy folyamatos integrációt folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-103">Create a continuous integration pipeline with Visual Studio Team Services and IIS</span></span>
<span data-ttu-id="0031e-104">tooautomate hello build, tesztelése és alkalmazásfejlesztés szakaszainak központi telepítés, a folyamatos integrációt és a központi telepítés (CI/CD) folyamat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0031e-104">tooautomate hello build, test, and deployment phases of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="0031e-105">Ebben az oktatóanyagban létrehoz egy Visual Studio Team Services és a Windows rendszerű virtuális gép (VM) használata az IIS-t futtató Azure CI/CD folyamat.</span><span class="sxs-lookup"><span data-stu-id="0031e-105">In this tutorial, you create a CI/CD pipeline using Visual Studio Team Services and a Windows virtual machine (VM) in Azure that runs IIS.</span></span> <span data-ttu-id="0031e-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="0031e-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0031e-107">ASP.NET webes alkalmazás tooa Team Services projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="0031e-107">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="0031e-108">A kód véglegesítések által elindított build-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-108">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="0031e-109">IIS telepítése és konfigurálása az Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="0031e-109">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="0031e-110">A Team Services hello IIS példány tooa üzembe helyezési csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0031e-110">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="0031e-111">Kiadás definition toopublish webhely tooIIS csomagok központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0031e-111">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="0031e-112">Hello CI/CD folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="0031e-112">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="0031e-113">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="0031e-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="0031e-114">Futtatás `Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="0031e-114">Run `Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="0031e-115">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="0031e-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="create-project-in-team-services"></a><span data-ttu-id="0031e-116">A Team Services-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-116">Create project in Team Services</span></span>
<span data-ttu-id="0031e-117">A Visual Studio Team Services lehetővé teszi, hogy könnyen együttműködés és fejlesztési egy helyszíni-felügyeleti megoldás fenntartása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0031e-117">Visual Studio Team Services allows for easy collaboration and development without maintaining an on-premises code management solution.</span></span> <span data-ttu-id="0031e-118">Team Services biztosít a felhő kód tesztelése, elkészítéséhez és az application insights.</span><span class="sxs-lookup"><span data-stu-id="0031e-118">Team Services provides cloud code testing, build, and application insights.</span></span> <span data-ttu-id="0031e-119">Választhat egy verzió vezérlő tárház és IDE a kód fejlesztési legjobban illő.</span><span class="sxs-lookup"><span data-stu-id="0031e-119">You can choose a version control repo and IDE that best fits your code development.</span></span> <span data-ttu-id="0031e-120">Ebben az oktatóanyagban egy ingyenes fiókot toocreate egy egyszerű ASP.NET webalkalmazást és CI/CD kimenetátirányítási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0031e-120">For this tutorial, you can use a free account toocreate a basic ASP.NET web app and CI/CD pipeline.</span></span> <span data-ttu-id="0031e-121">Ha még nem rendelkezik egy Team Services-fiók [hozzon létre egyet](http://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="0031e-121">If you do not already have a Team Services account, [create one](http://go.microsoft.com/fwlink/?LinkId=307137).</span></span>

<span data-ttu-id="0031e-122">toomanage hello kód véglegesítési folyamat build-definíciókat, és kiadási definíciók, az alábbiak szerint hozzon létre egy projektet Team Services:</span><span class="sxs-lookup"><span data-stu-id="0031e-122">toomanage hello code commit process, build definitions, and release definitions, create a project in Team Services as follows:</span></span>

1. <span data-ttu-id="0031e-123">A Team Services irányítópult megnyitása a böngészőben, és válassza a **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="0031e-123">Open your Team Services dashboard in a web browser and choose **New project**.</span></span>
2. <span data-ttu-id="0031e-124">Adja meg *myWebApp* a hello **projektnevet**.</span><span class="sxs-lookup"><span data-stu-id="0031e-124">Enter *myWebApp* for hello **Project name**.</span></span> <span data-ttu-id="0031e-125">Hagyja a további alapértelmezett értékek toouse *Git* verziókezelést és *Agile* munkaelemet bekérő folyamat.</span><span class="sxs-lookup"><span data-stu-id="0031e-125">Leave all other default values toouse *Git* version control and *Agile* work item process.</span></span>
3. <span data-ttu-id="0031e-126">Hello választógomb túl**megosztása** *csapattagok*, majd jelölje be **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0031e-126">Choose hello option too**Share with** *Team Members*, then select **Create**.</span></span>
5. <span data-ttu-id="0031e-127">A projekt létrehozása után válassza ki az hello beállítás túl**információs vagy gitignore inicializálása**, majd **inicializálása**.</span><span class="sxs-lookup"><span data-stu-id="0031e-127">Once your project has been created, choose hello option too**Initialize with a README or gitignore**, then **Initialize**.</span></span>
6. <span data-ttu-id="0031e-128">Válassza ki az új projekt belül **irányítópultok** hello tetején, majd válassza ki **Megnyitás Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="0031e-128">Inside your new project, choose **Dashboards** across hello top, then select **Open in Visual Studio**.</span></span>


## <a name="create-aspnet-web-application"></a><span data-ttu-id="0031e-129">ASP.NET-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-129">Create ASP.NET web application</span></span>
<span data-ttu-id="0031e-130">Hello előző lépésben létrehozott csapat szolgáltatásokban projekt.</span><span class="sxs-lookup"><span data-stu-id="0031e-130">In hello previous step, you created a project in Team Services.</span></span> <span data-ttu-id="0031e-131">utolsó lépésként hello Visual Studio az új projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="0031e-131">hello final step opens your new project in Visual Studio.</span></span> <span data-ttu-id="0031e-132">Kezelheti a kód véglegesíti a hello **Team Explorer** ablak.</span><span class="sxs-lookup"><span data-stu-id="0031e-132">You manage your code commits in hello **Team Explorer** window.</span></span> <span data-ttu-id="0031e-133">Hozzon létre az új projekt helyi példányát, majd egy ASP.NET-webalkalmazás létrehozása sablonból az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0031e-133">Create a local copy of your new project, then create an ASP.NET web application from a template as follows:</span></span>

1. <span data-ttu-id="0031e-134">Válassza ki **Klónozás** toocreate egy helyi git-tárház a Team Services projekt.</span><span class="sxs-lookup"><span data-stu-id="0031e-134">Select **Clone** toocreate a local git repo of your Team Services project.</span></span>
    
    ![Team Services projektből tárház klónozása](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. <span data-ttu-id="0031e-136">A **megoldások**, jelölje be **új**.</span><span class="sxs-lookup"><span data-stu-id="0031e-136">Under **Solutions**, select **New**.</span></span>

    ![Webalkalmazási megoldás létrehozása](media/tutorial-vsts-iis-cicd/new_solution.png)

3. <span data-ttu-id="0031e-138">Válassza ki **webes** sablonokat, és válassza a hello **ASP.NET Web Application** sablont.</span><span class="sxs-lookup"><span data-stu-id="0031e-138">Select **Web** templates, and then choose hello **ASP.NET Web Application** template.</span></span>
    1. <span data-ttu-id="0031e-139">Írjon be egy nevet az alkalmazásnak, például *myWebApp*, és törölje a jelet hello be a **könyvtár létrehozása a megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="0031e-139">Enter a name for your application, such as *myWebApp*, and uncheck hello box for **Create directory for solution**.</span></span>
    2. <span data-ttu-id="0031e-140">Ha hello lehetőség nem érhető el, törölje a jelet hello mezőben túl**Application Insights hozzáadása tooproject**.</span><span class="sxs-lookup"><span data-stu-id="0031e-140">If hello option is available, uncheck hello box too**Add Application Insights tooproject**.</span></span> <span data-ttu-id="0031e-141">Az Application Insights igényel, tooauthorize a webalkalmazás az Azure Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="0031e-141">Application Insights requires you tooauthorize your web application with Azure Application Insights.</span></span> <span data-ttu-id="0031e-142">tookeep ebben az oktatóanyagban egyszerű, hagyja ki ezt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="0031e-142">tookeep it simple in this tutorial, skip this process.</span></span>
    3. <span data-ttu-id="0031e-143">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0031e-143">Select **OK**.</span></span>
4. <span data-ttu-id="0031e-144">Válasszon **MVC** hello sablon listából.</span><span class="sxs-lookup"><span data-stu-id="0031e-144">Choose **MVC** from hello template list.</span></span>
    1. <span data-ttu-id="0031e-145">Válassza ki **hitelesítés módosítása**, válassza a **nem hitelesítési**, majd jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="0031e-145">Select **Change Authentication**, choose **No Authentication**, then select **OK**.</span></span>
    2. <span data-ttu-id="0031e-146">Válassza ki **OK** toocreate a megoldás.</span><span class="sxs-lookup"><span data-stu-id="0031e-146">Select **OK** toocreate your solution.</span></span>
5. <span data-ttu-id="0031e-147">A hello **Team Explorer** ablakban válasszon **módosítások**.</span><span class="sxs-lookup"><span data-stu-id="0031e-147">In hello **Team Explorer** window, choose **Changes**.</span></span>

    ![Véglegesítse a változtatásokat tooTeam szolgáltatások git-tárház](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. <span data-ttu-id="0031e-149">Hello véglegesítési szövegmezőben, adjon meg egy üzenetet, mint *kezdeti véglegesítési*.</span><span class="sxs-lookup"><span data-stu-id="0031e-149">In hello commit text box, enter a message such as *Initial commit*.</span></span> <span data-ttu-id="0031e-150">Válasszon **véglegesíti az összes és a szinkronizálási** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="0031e-150">Choose **Commit All and Sync** from hello drop-down menu.</span></span>


## <a name="create-build-definition"></a><span data-ttu-id="0031e-151">Build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-151">Create build definition</span></span>
<span data-ttu-id="0031e-152">A Team Services a build definition toooutline, hogyan kell kialakítani, az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="0031e-152">In Team Services, you use a build definition toooutline how your application should be built.</span></span> <span data-ttu-id="0031e-153">Ebben az oktatóanyagban létrehozhatunk egy alapszintű definition vesz igénybe a forráskódban, hello megoldást hoz létre, akkor hoz létre web csomag használhatjuk toorun hello webalkalmazást az IIS-kiszolgáló üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="0031e-153">In this tutorial, we create a basic definition that takes our source code, builds hello solution, then creates web deploy package we can use toorun hello web app on an IIS server.</span></span>

1. <span data-ttu-id="0031e-154">A Team Services projekthez, válassza a **Build & kiadási** hello tetején, majd válassza ki **buildek**.</span><span class="sxs-lookup"><span data-stu-id="0031e-154">In your Team Services project, choose **Build & Release** across hello top, then select **Builds**.</span></span>
3. <span data-ttu-id="0031e-155">Válassza ki **+ új definition**.</span><span class="sxs-lookup"><span data-stu-id="0031e-155">Select **+ New definition**.</span></span>
4. <span data-ttu-id="0031e-156">Válassza ki a hello **ASP.NET (előzetes verzió)** sablont, és válassza **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="0031e-156">Choose hello **ASP.NET (PREVIEW)** template and select **Apply**.</span></span>
5. <span data-ttu-id="0031e-157">Minden hello alapértelmezett feladat értékek hagyja.</span><span class="sxs-lookup"><span data-stu-id="0031e-157">Leave all hello default task values.</span></span> <span data-ttu-id="0031e-158">A **adatforrások beolvasása**, győződjön meg arról, hogy hello *myWebApp* tárház és *fő* fiókirodai van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0031e-158">Under **Get sources**, ensure that hello *myWebApp* repository and *master* branch are selected.</span></span>

    ![Build definíció Team Services-projekt létrehozása](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. <span data-ttu-id="0031e-160">A hello **eseményindítók** lapon, a hello csúszkát **engedélyezése ehhez az eseményindítóhoz** túl*engedélyezve*.</span><span class="sxs-lookup"><span data-stu-id="0031e-160">On hello **Triggers** tab, move hello slider for **Enable this trigger** too*Enabled*.</span></span>
7. <span data-ttu-id="0031e-161">Mentés hello build definíció- és várólista új buildverziót kiválasztásával **mentés és a feldolgozási sor** , majd **mentés és a feldolgozási sor** újra.</span><span class="sxs-lookup"><span data-stu-id="0031e-161">Save hello build definition and queue a new build by selecting **Save & queue** , then **Save & queue** again.</span></span> <span data-ttu-id="0031e-162">Meghagyhatja hello alapértelmezett beállításokat, és válassza ki **várólista**.</span><span class="sxs-lookup"><span data-stu-id="0031e-162">Leave hello defaults and select **Queue**.</span></span>

<span data-ttu-id="0031e-163">A Watch hello build üzemeltetett ügynökön van ütemezve, majd megkezdi toobuild.</span><span class="sxs-lookup"><span data-stu-id="0031e-163">Watch as hello build is scheduled on a hosted agent, then begins toobuild.</span></span> <span data-ttu-id="0031e-164">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="0031e-164">hello output is similar toohello following example:</span></span>

![A Team Services projekt sikeres build](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a><span data-ttu-id="0031e-166">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-166">Create virtual machine</span></span>
<span data-ttu-id="0031e-167">a platform toorun tooprovide az ASP.NET webalkalmazás szükséges IIS-t futtató Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="0031e-167">tooprovide a platform toorun your ASP.NET web app, you need a Windows virtual machine that runs IIS.</span></span> <span data-ttu-id="0031e-168">Team Services egy ügynök toointeract kód véglegesíti és buildek által kiváltott hello IIS-példány használja.</span><span class="sxs-lookup"><span data-stu-id="0031e-168">Team Services uses an agent toointeract with hello IIS instance as you commit code and builds are triggered.</span></span>

<span data-ttu-id="0031e-169">Windows Server 2016 VM létre [a parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0031e-169">Create a Windows Server 2016 VM using [this script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span></span> <span data-ttu-id="0031e-170">Hello parancsfájl toorun néhány percet vesz igénybe, és hozzon létre virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="0031e-170">It takes a few minutes for hello script toorun and create hello VM.</span></span> <span data-ttu-id="0031e-171">Hello virtuális gép létrehozása után, nyissa meg a 80-as port webes forgalom a [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0031e-171">Once hello VM has been created, open port 80 for web traffic with [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) as follows:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

<span data-ttu-id="0031e-172">tooconnect tooyour VM, hello nyilvános IP-cím az beszerzése [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0031e-172">tooconnect tooyour VM, obtain hello public IP address with [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) as follows:</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

<span data-ttu-id="0031e-173">A távoli asztali munkamenetgazda tooyour virtuális gép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="0031e-173">Create a remote desktop session tooyour VM:</span></span>

```cmd
mstsc /v:<publicIpAddress>
```

<span data-ttu-id="0031e-174">Hello VM, nyisson meg egy **rendszergazda PowerShell** parancssort.</span><span class="sxs-lookup"><span data-stu-id="0031e-174">On hello VM, open an **Administrator PowerShell** command prompt.</span></span> <span data-ttu-id="0031e-175">Az IIS és a kötelező .NET-szolgáltatások telepítése a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="0031e-175">Install IIS and required .NET features as follows:</span></span>

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a><span data-ttu-id="0031e-176">Üzembe helyezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-176">Create deployment group</span></span>
<span data-ttu-id="0031e-177">kimenő hello webes toopush csomag toohello IIS-kiszolgálón, a rendszer Team Services adjon meg egy központi telepítési csoportot.</span><span class="sxs-lookup"><span data-stu-id="0031e-177">toopush out hello web deploy package toohello IIS server, you define a deployment group in Team Services.</span></span> <span data-ttu-id="0031e-178">Ennek a csoportnak azok a kiszolgálók olyan új buildek hello célját, mint a szolgáltatások és -buildek befejezése kód tooTeam véglegesítést toospecify lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="0031e-178">This group allows you toospecify which servers are hello target of new builds as you commit code tooTeam Services and builds are completed.</span></span>

1. <span data-ttu-id="0031e-179">Team Services, válassza a **Build & kiadási** , és válassza **telepítési csoportban**.</span><span class="sxs-lookup"><span data-stu-id="0031e-179">In Team Services, choose **Build & Release** and then select **Deployment groups**.</span></span>
2. <span data-ttu-id="0031e-180">Válasszon **központi telepítés hozzáadása csoporthoz**.</span><span class="sxs-lookup"><span data-stu-id="0031e-180">Choose **Add Deployment group**.</span></span>
3. <span data-ttu-id="0031e-181">Írjon be hello csoport nevét, például *myIIS*, majd jelölje be **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0031e-181">Enter a name for hello group, such as *myIIS*, then select **Create**.</span></span>
4. <span data-ttu-id="0031e-182">A hello **gépek regisztrálása** területen győződjön meg arról *Windows* van kiválasztva, majd hello jelölőnégyzetet túl**személyes hozzáférési jogkivonat hello parancsfájlban a hitelesítéshez használandó**.</span><span class="sxs-lookup"><span data-stu-id="0031e-182">In hello **Register machines** section, ensure *Windows* is selected, then check hello box too**Use a personal access token in hello script for authentication**.</span></span>
5. <span data-ttu-id="0031e-183">Válassza ki **másolja a parancsfájl tooclipboard**.</span><span class="sxs-lookup"><span data-stu-id="0031e-183">Select **Copy script tooclipboard**.</span></span>


### <a name="add-iis-vm-toohello-deployment-group"></a><span data-ttu-id="0031e-184">Az IIS virtuális gép toohello üzembe helyezési csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0031e-184">Add IIS VM toohello deployment group</span></span>
<span data-ttu-id="0031e-185">A hello telepítési csoportot létrehozni minden egyes IIS példány toohello csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="0031e-185">With hello deployment group created, add each IIS instance toohello group.</span></span> <span data-ttu-id="0031e-186">Team Services hoz létre olyan parancsfájlt, amely tölti le, és konfigurálja az ügynök központi telepítése a virtuális Gépet, amely megkapja az új webes hello csomagok, és alkalmazza azt tooIIS.</span><span class="sxs-lookup"><span data-stu-id="0031e-186">Team Services generates a script that downloads and configures an agent on hello VM that receives new web deploy packages then applies it tooIIS.</span></span>

1. <span data-ttu-id="0031e-187">Vissza a hello **rendszergazda PowerShell** munkamenet a virtuális gépen, illessze be, és átmásolja a Team Services hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="0031e-187">Back in hello **Administrator PowerShell** session on your VM, paste and run hello script copied from Team Services.</span></span>
2. <span data-ttu-id="0031e-188">Ha felszólító tooconfigure címkék hello Agent dönt *Y* , és írja be *webes*.</span><span class="sxs-lookup"><span data-stu-id="0031e-188">When prompted tooconfigure tags for hello agent, choose *Y* and enter *web*.</span></span>
3. <span data-ttu-id="0031e-189">Amikor a hello felhasználói fiók megadását kéri, nyomja meg az *vissza* tooaccept hello alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="0031e-189">When prompted for hello user account, press *Return* tooaccept hello defaults.</span></span>
4. <span data-ttu-id="0031e-190">Várjon, amíg hello parancsfájl toofinish üzenetet *szolgáltatás vstsagent.account.computername sikeresen elindult*.</span><span class="sxs-lookup"><span data-stu-id="0031e-190">Wait for hello script toofinish with a message *Service vstsagent.account.computername started successfully*.</span></span>
5. <span data-ttu-id="0031e-191">A hello **telepítési csoportban** hello oldalán **Build & kiadási** menü, nyissa meg hello *myIIS* üzembe helyezési csoport.</span><span class="sxs-lookup"><span data-stu-id="0031e-191">In hello **Deployment groups** page of hello **Build & Release** menu, open hello *myIIS* deployment group.</span></span> <span data-ttu-id="0031e-192">A hello **gépek** lapra, győződjön meg arról, hogy a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="0031e-192">On hello **Machines** tab, verify that your VM is listed.</span></span>

    ![Virtuális gép sikeresen hozzáadta a tooTeam szolgáltatások üzembe helyezési csoport](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a><span data-ttu-id="0031e-194">Kiadás definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-194">Create release definition</span></span>
<span data-ttu-id="0031e-195">toopublish a buildek hoz létre egy kiadási definition Team Services.</span><span class="sxs-lookup"><span data-stu-id="0031e-195">toopublish your builds, you create a release definition in Team Services.</span></span> <span data-ttu-id="0031e-196">Ez a definíció sikeres build az alkalmazás automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="0031e-196">This definition is triggered automatically by a successful build of your application.</span></span> <span data-ttu-id="0031e-197">Válassza ki a hello telepítési csoport toopush a web deploy csomag számára, és hello megfelelő IIS-beállítások megadása.</span><span class="sxs-lookup"><span data-stu-id="0031e-197">You choose hello deployment group toopush your web deploy package to, and define hello appropriate IIS settings.</span></span>

1. <span data-ttu-id="0031e-198">Válasszon **Build & kiadási**, majd jelölje be **buildek**.</span><span class="sxs-lookup"><span data-stu-id="0031e-198">Choose **Build & Release**, then select **Builds**.</span></span> <span data-ttu-id="0031e-199">Válassza ki az előző lépésben létrehozott hello build definition.</span><span class="sxs-lookup"><span data-stu-id="0031e-199">Choose hello build definition created in a previous step.</span></span>
2. <span data-ttu-id="0031e-200">A **nemrég befejeződött**hello legutóbbi build válassza, majd válassza ki **kiadás**.</span><span class="sxs-lookup"><span data-stu-id="0031e-200">Under **Recently completed**, choose hello most recent build, then select **Release**.</span></span>
3. <span data-ttu-id="0031e-201">Válasszon **Igen** toocreate kiadás definícióját.</span><span class="sxs-lookup"><span data-stu-id="0031e-201">Choose **Yes** toocreate a release definition.</span></span>
4. <span data-ttu-id="0031e-202">Válassza ki a hello **üres** sablont, majd válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="0031e-202">Choose hello **Empty** template, then select **Next**.</span></span>
5. <span data-ttu-id="0031e-203">Ellenőrizze, hogy hello projekt és a forrás-build definíciója a rendszer feltölti a projektet.</span><span class="sxs-lookup"><span data-stu-id="0031e-203">Verify hello project and source build definition are populated with your project.</span></span>
6. <span data-ttu-id="0031e-204">Jelölje be hello **folyamatos üzembe helyezés** jelölőnégyzetet, majd válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0031e-204">Select hello **Continuous deployment** check box, then select **Create**.</span></span>
7. <span data-ttu-id="0031e-205">Válasszon hello legördülő lista melletti túl**+ Hozzáadás feladatok** válassza *adja hozzá a központi telepítési csoport szakaszok*.</span><span class="sxs-lookup"><span data-stu-id="0031e-205">Select hello drop-down box next too**+ Add tasks** and choose *Add a deployment group phase*.</span></span>
    
    ![A Team Services toorelease feladatdefiníció hozzáadása](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. <span data-ttu-id="0031e-207">Válasszon **Hozzáadás** következő túl**IIS Web App Deploy(Preview)**, majd jelölje be **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="0031e-207">Choose **Add** next too**IIS Web App Deploy(Preview)**, then select **Close**.</span></span>
9. <span data-ttu-id="0031e-208">Jelölje be hello **futhat az üzembe helyezési csoport** Szülőtevékenység.</span><span class="sxs-lookup"><span data-stu-id="0031e-208">Select hello **Run on deployment group** parent task.</span></span>
    1. <span data-ttu-id="0031e-209">A **üzembe helyezési csoport**, jelölje be hello telepítési csoportjának, létrehozott korábbi, mint például *myIIS*.</span><span class="sxs-lookup"><span data-stu-id="0031e-209">For **Deployment Group**, select hello deployment group you created earlier, such as *myIIS*.</span></span>
    2. <span data-ttu-id="0031e-210">A hello **számítógép-címkék** mezőben válassza **Hozzáadás** , és válassza a hello *webes* címke.</span><span class="sxs-lookup"><span data-stu-id="0031e-210">In hello **Machine tags** box, select **Add** and choose hello *web* tag.</span></span>
    
    ![Kiadás definition telepítési csoport feladat az IIS-hez](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. <span data-ttu-id="0031e-212">SELECT hello **központi telepítése: az IIS webes alkalmazás központi telepítése** feladat tooconfigure az IIS-példány beállításokat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0031e-212">Select hello **Deploy: IIS Web App Deploy** task tooconfigure your IIS instance settings as follows:</span></span>
    1. <span data-ttu-id="0031e-213">A **webhelynevet**, adja meg *alapértelmezett webhely*.</span><span class="sxs-lookup"><span data-stu-id="0031e-213">For **Website Name**, enter *Default Web Site*.</span></span>
    2. <span data-ttu-id="0031e-214">Minden hello hagyja a további alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="0031e-214">Leave all hello other default settings.</span></span>
12. <span data-ttu-id="0031e-215">Válasszon **mentése**, majd jelölje be **OK** kétszer.</span><span class="sxs-lookup"><span data-stu-id="0031e-215">Choose **Save**, then select **OK** twice.</span></span>


## <a name="create-a-release-and-publish"></a><span data-ttu-id="0031e-216">Egy kiadási létrehozása és közzététele</span><span class="sxs-lookup"><span data-stu-id="0031e-216">Create a release and publish</span></span>
<span data-ttu-id="0031e-217">Most tolható ki a webhely egy új kiadási csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0031e-217">You can now push your web deploy package as a new release.</span></span> <span data-ttu-id="0031e-218">Ez a lépés kommunikál hello ügynök hello üzembe helyezési csoport része, a leküldéses értesítések hello webes összes példányt csomag telepítése, majd konfigurálja az IIS toorun frissített hello webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0031e-218">This step communicates with hello agent on each instance that is part of hello deployment group, pushes hello web deploy package, then configures IIS toorun hello updated web application.</span></span>

1. <span data-ttu-id="0031e-219">A kiadási definíciót, válassza ki **+ kiadási**, majd válassza ki **létrehozása kiadási**.</span><span class="sxs-lookup"><span data-stu-id="0031e-219">In your release definition, select **+ Release**, then choose **Create Release**.</span></span>
2. <span data-ttu-id="0031e-220">Győződjön meg arról, hogy hello legújabb buildjével hello legördülő listában kiválasztott, valamint **automatikus központi telepítési: kiadás létrehozása után**.</span><span class="sxs-lookup"><span data-stu-id="0031e-220">Verify that hello latest build is selected in hello drop-down list, along with **Automated deployment: After release creation**.</span></span> <span data-ttu-id="0031e-221">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0031e-221">Select **Create**.</span></span>
3. <span data-ttu-id="0031e-222">Egy kis szalagcím akkor jelenik meg a kiadási definíció hello tetején, mint *kiadás kibocsátási-1 létrejött*.</span><span class="sxs-lookup"><span data-stu-id="0031e-222">A small banner appears across hello top of your release definition, such as *Release 'Release-1' has been created*.</span></span> <span data-ttu-id="0031e-223">Jelölje be hello kiadás hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="0031e-223">Select hello release link.</span></span>
4. <span data-ttu-id="0031e-224">Nyissa meg hello **naplók** lapon toowatch hello kiadás folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="0031e-224">Open hello **Logs** tab toowatch hello release progress.</span></span>
    
    ![Sikeres Team Services kiadás és webes telepítési csomag leküldéses](media/tutorial-vsts-iis-cicd/successful_release.png)

5. <span data-ttu-id="0031e-226">Hello kiadás befejezése után nyisson meg egy webböngészőt, és adja meg a virtuális gép hello nyilvános KVI címét.</span><span class="sxs-lookup"><span data-stu-id="0031e-226">After hello release is complete, open a web browser and enter hello public IIP address of your VM.</span></span> <span data-ttu-id="0031e-227">Az ASP.NET-webalkalmazás fut.</span><span class="sxs-lookup"><span data-stu-id="0031e-227">Your ASP.NET web application is running.</span></span>

    ![ASP.NET webalkalmazás az IIS virtuális gépen](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a><span data-ttu-id="0031e-229">Hello teljes CI/CD folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="0031e-229">Test hello whole CI/CD pipeline</span></span>
<span data-ttu-id="0031e-230">A webes alkalmazás IIS-kiszolgálón fut, és próbálja meg hello teljes CI/CD folyamat.</span><span class="sxs-lookup"><span data-stu-id="0031e-230">With your web application running on IIS, now try hello whole CI/CD pipeline.</span></span> <span data-ttu-id="0031e-231">Után módosítja a Visual Studio és akkor váltódik ki, a kódot, a build véglegesítési, majd elindítja a frissített Web kiadási csomag tooIIS központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="0031e-231">After you make a change in Visual Studio and commit your code, a build is triggered which then triggers a release of your updated web deploy package tooIIS:</span></span>

1. <span data-ttu-id="0031e-232">A Visual Studióban nyissa meg a hello **Megoldáskezelőben** ablak.</span><span class="sxs-lookup"><span data-stu-id="0031e-232">In Visual Studio, open hello **Solution Explorer** window.</span></span>
2. <span data-ttu-id="0031e-233">Keresse meg a tooand nyílt *myWebApp |} Nézetek |} Kezdőlap |} Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0031e-233">Navigate tooand open *myWebApp | Views | Home | Index.cshtml*</span></span>
3. <span data-ttu-id="0031e-234">Sor 6 tooread szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="0031e-234">Edit line 6 tooread:</span></span>

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. <span data-ttu-id="0031e-235">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0031e-235">Save hello file.</span></span>
5. <span data-ttu-id="0031e-236">Nyissa meg hello **Team Explorer** ablakban, a select hello *myWebApp* projektre, majd kattintson a **módosítások**.</span><span class="sxs-lookup"><span data-stu-id="0031e-236">Open hello **Team Explorer** window, select hello *myWebApp* project, then choose **Changes**.</span></span>
6. <span data-ttu-id="0031e-237">Adja meg például a véglegesítési üzenetet, *tesztelés CI/CD csővezeték*, majd válassza ki **véglegesítési összes és a szinkronizálási** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="0031e-237">Enter a commit message, such as *Testing CI/CD pipeline*, then choose **Commit All and Sync** from hello drop-down menu.</span></span>
7. <span data-ttu-id="0031e-238">Team Services munkaterület egy új build hello kód véglegesítési a elindul.</span><span class="sxs-lookup"><span data-stu-id="0031e-238">In Team Services workspace, a new build is triggered from hello code commit.</span></span> 
    - <span data-ttu-id="0031e-239">Válasszon **Build & kiadási**, majd jelölje be **buildek**.</span><span class="sxs-lookup"><span data-stu-id="0031e-239">Choose **Build & Release**, then select **Builds**.</span></span> 
    - <span data-ttu-id="0031e-240">Válasszon a build-definíciót, majd válassza ki a hello **várakozik & futó** build toowatch, hello létrehozása történik.</span><span class="sxs-lookup"><span data-stu-id="0031e-240">Choose your build definition, then select hello **Queued & running** build toowatch as hello build progresses.</span></span>
9. <span data-ttu-id="0031e-241">Ha sikeres hello build, egy új kiadási elindul.</span><span class="sxs-lookup"><span data-stu-id="0031e-241">Once hello build is successful, a new release is triggered.</span></span>
    - <span data-ttu-id="0031e-242">Válasszon **Build & kiadási**, majd **kiadásokban** toosee hello web deploy csomag tooyour IIS VM leküldött.</span><span class="sxs-lookup"><span data-stu-id="0031e-242">Choose **Build & Release**, then **Releases** toosee hello web deploy package pushed tooyour IIS VM.</span></span> 
    - <span data-ttu-id="0031e-243">Jelölje be hello **frissítése** ikon tooupdate hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="0031e-243">Select hello **Refresh** icon tooupdate hello status.</span></span> <span data-ttu-id="0031e-244">Ha hello *környezetek* az oszlopban látható, egy zöld pipa, hello kiadás sikeresen alkalmazva van a tooIIS.</span><span class="sxs-lookup"><span data-stu-id="0031e-244">When hello *Environments* column shows a green check mark, hello release has successfully deployed tooIIS.</span></span>
11. <span data-ttu-id="0031e-245">toosee a módosításokat alkalmazza, frissítse az IIS-webhely a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0031e-245">toosee your changes applied, refresh your IIS website in a browser.</span></span>

    ![CI/CD láncból az IIS virtuális gépen futó ASP.NET-webalkalmazás](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a><span data-ttu-id="0031e-247">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0031e-247">Next steps</span></span>

<span data-ttu-id="0031e-248">Ebben az oktatóanyagban egy ASP.NET-webalkalmazás létrehozott Team Services, és konfigurált build és kiadás definíciók toodeploy új webes telepítési csomagokat tooIIS minden kód érvényesítéskor.</span><span class="sxs-lookup"><span data-stu-id="0031e-248">In this tutorial, you created an ASP.NET web application in Team Services and configured build and release definitions toodeploy new web deploy packages tooIIS on each code commit.</span></span> <span data-ttu-id="0031e-249">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="0031e-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0031e-250">ASP.NET webes alkalmazás tooa Team Services projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="0031e-250">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="0031e-251">A kód véglegesítések által elindított build-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0031e-251">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="0031e-252">IIS telepítése és konfigurálása az Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="0031e-252">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="0031e-253">A Team Services hello IIS példány tooa üzembe helyezési csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0031e-253">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="0031e-254">Kiadás definition toopublish webhely tooIIS csomagok központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0031e-254">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="0031e-255">Hello CI/CD folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="0031e-255">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="0031e-256">Hogyan előzetes toohello következő útmutató toolearn toosecure tartalmazó webkiszolgáló SSL-tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="0031e-256">Advance toohello next tutorial toolearn how toosecure a web server with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0031e-257">Biztonságos webkiszolgáló SSL</span><span class="sxs-lookup"><span data-stu-id="0031e-257">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)