---
title: "Ismerkedés az Azure Automation DSC szolgáltatással |} Microsoft Docs"
description: "MAGYARÁZAT és példákat a leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 8a10d961ad7c107c68b57c64ee6c88544ff8832b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="6f00e-103">Ismerkedés az Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="6f00e-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="6f00e-104">Ez a témakör ismerteti a leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC), például a létrehozása, importálása, és konfigurációk, kezeléséhez, a bevezetési gépek fordítása és jelentések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="6f00e-104">This topic explains how to do the most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines to manage, and viewing reports.</span></span> <span data-ttu-id="6f00e-105">Milyen Azure Automation DSC áttekintést van, a következő témakörben: [Azure Automation DSC – áttekintés](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6f00e-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="6f00e-106">A DSC-dokumentáció, lásd: [Windows PowerShell kívánt állapot beállítása – áttekintés](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="6f00e-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="6f00e-107">Ez a témakör részletesen ismerteti, Azure Automation DSC használata.</span><span class="sxs-lookup"><span data-stu-id="6f00e-107">This topic provides a step-by-step guide to using Azure Automation DSC.</span></span> <span data-ttu-id="6f00e-108">Ha azt szeretné, hogy egy minta-környezet, amely már be van állítva a jelen témakörben ismertetett lépések nélkül, [a következő ARM-sablon](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="6f00e-108">If you want a sample environment that is already set up without following the steps described in this topic, you can use [the following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="6f00e-109">Ez a sablon állít be egy befejezett Azure Automation DSC-környezetben, például egy Azure Automation DSC által felügyelt Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="6f00e-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f00e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6f00e-110">Prerequisites</span></span>
<span data-ttu-id="6f00e-111">Ebben a témakörben szereplő példák elvégzéséhez a következőkre szükség:</span><span class="sxs-lookup"><span data-stu-id="6f00e-111">To complete the examples in this topic, the following are required:</span></span>

* <span data-ttu-id="6f00e-112">Egy Azure Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="6f00e-112">An Azure Automation account.</span></span> <span data-ttu-id="6f00e-113">Egy Azure Automation futtató fiók létrehozása, lásd: [Azure futtató fiók](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="6f00e-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="6f00e-114">Az Azure Resource Manager virtuális gép (klasszikus) Windows Server 2008 R2 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6f00e-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="6f00e-115">Virtuális gép létrehozása, lásd: [az első Windows rendszerű virtuális gép létrehozása az Azure portálon](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="6f00e-115">For instructions on creating a VM, see [Create your first Windows virtual machine in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="6f00e-116">A DSC-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f00e-116">Creating a DSC configuration</span></span>
<span data-ttu-id="6f00e-117">Létre fogunk hozni egy egyszerű [DSC-konfiguráció](https://msdn.microsoft.com/powershell/dsc/configurations) megléte vagy hiánya, ezzel biztosítható a **webkiszolgáló** Windows szolgáltatás (IIS), attól függően, hogy hogyan lehet kijelölni csomópontok.</span><span class="sxs-lookup"><span data-stu-id="6f00e-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either the presence or absence of the **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="6f00e-118">Indítsa el a Windows PowerShell ISE (és bármilyen szövegszerkesztővel).</span><span class="sxs-lookup"><span data-stu-id="6f00e-118">Start the Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="6f00e-119">Írja be a következőket:</span><span class="sxs-lookup"><span data-stu-id="6f00e-119">Type the following text:</span></span>
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. <span data-ttu-id="6f00e-120">Mentse a fájlt `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="6f00e-120">Save the file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="6f00e-121">Ez a konfiguráció minden csomópont blokkban meghívja a egy erőforrást a [WindowsFeature erőforrás](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), megléte vagy hiánya, ezzel biztosítható a **webkiszolgáló** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6f00e-121">This configuration calls one resource in each node block, the [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either the presence or absence of the **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="6f00e-122">Azure Automation konfiguráció importálása</span><span class="sxs-lookup"><span data-stu-id="6f00e-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="6f00e-123">Ezután azt fogja importálnia kell a konfiguráció az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="6f00e-123">Next, we'll import the configuration into the Automation account.</span></span>

1. <span data-ttu-id="6f00e-124">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-125">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-125">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-126">Az a **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-126">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="6f00e-127">A a **a DSC-konfigurációk** panelen kattintson a **hozzáadása egy konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-127">On the **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="6f00e-128">Az a **konfiguráció importálása** panelen keresse meg a `TestConfig.ps1` fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6f00e-128">On the **Import Configuration** blade, browse to the `TestConfig.ps1` file on your computer.</span></span>
   
    ![Képernyőfelvétel a ** importálási konfigurációs ** panel](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="6f00e-130">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6f00e-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="6f00e-131">A beállítások megjelenítése az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="6f00e-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="6f00e-132">Miután importálta a konfiguráció, megtekintheti az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6f00e-132">After you have imported a configuration, you can view it in the Azure portal.</span></span>

1. <span data-ttu-id="6f00e-133">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-133">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-134">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-134">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-135">Az a **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="6f00e-135">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="6f00e-136">Az a **a DSC-konfigurációk** panelen kattintson a **TestConfig** (azt a nevet, a konfiguráció az előző eljárásban importált).</span><span class="sxs-lookup"><span data-stu-id="6f00e-136">On the **DSC Configurations** blade, click **TestConfig** (this is the name of the configuration you imported in the previous procedure).</span></span>
5. <span data-ttu-id="6f00e-137">Az a **TestConfig konfigurációs** panelen kattintson a **konfigurációs forrás megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-137">On the **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![A TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="6f00e-139">A **TestConfig konfigurációs forrás** panel jelenik meg, amelyen a PowerShell-kódot, a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6f00e-139">A **TestConfig Configuration source** blade opens, displaying the PowerShell code for the configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="6f00e-140">Az Azure Automationben konfiguráció fordítása</span><span class="sxs-lookup"><span data-stu-id="6f00e-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="6f00e-141">Alkalmazása előtt ki a kívánt állapot csomóponthoz, a DSC-konfiguráció meghatározása az állapotban kell egy vagy több csomópont-konfigurációt (MOF dokumentum) lefordítva, és a Automation DSC-lekéréses kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6f00e-141">Before you can apply a desired state to a node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on the Automation DSC Pull Server.</span></span> <span data-ttu-id="6f00e-142">Azure Automation DSC-konfigurációja összeállításának részletes ismertetését lásd: [Azure Automation DSC-konfigurációja fordítása](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="6f00e-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="6f00e-143">Konfiguráció fordítása kapcsolatos további információkért lásd: [a DSC-konfigurációk](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="6f00e-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="6f00e-144">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-144">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-145">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-145">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-146">Az a **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="6f00e-146">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="6f00e-147">Az a **a DSC-konfigurációk** panelen kattintson a **TestConfig** (a korábban importált konfiguráció neve).</span><span class="sxs-lookup"><span data-stu-id="6f00e-147">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="6f00e-148">Az a **TestConfig konfigurációs** panelen kattintson a **fordítási**, és kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-148">On the **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="6f00e-149">Egy fordítási feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="6f00e-149">This starts a compilation job.</span></span>
   
    ![Fordítási gomb kiemelés TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="6f00e-151">Ha egy Azure Automation konfigurációs lefordításához azt automatikusan telepíti a bármely létrehozott csomópont-konfiguráció MOF-ot a lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="6f00e-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs to the pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="6f00e-152">Egy fordítási feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="6f00e-152">Viewing a compilation job</span></span>
<span data-ttu-id="6f00e-153">Egy fordítási elindítása után megtekintheti a a **fordítási feladatok** csempére a **konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="6f00e-153">After you start a compilation, you can view it in the **Compilation jobs** tile in the **Configuration** blade.</span></span> <span data-ttu-id="6f00e-154">A **fordítási feladatok** csempe megjeleníti a jelenleg futó, befejeződött, és sikertelen feladatok.</span><span class="sxs-lookup"><span data-stu-id="6f00e-154">The **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="6f00e-155">Amikor megnyit egy fordítási feladat panelen, azt mutatja, beleértve az esetleges hibákat vagy figyelmeztetéseket észlelt feladatot információt, a bemeneti paramétereket használni a konfigurációs és a fordítás naplók.</span><span class="sxs-lookup"><span data-stu-id="6f00e-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in the configuration, and compilation logs.</span></span>

1. <span data-ttu-id="6f00e-156">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-156">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-157">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-157">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-158">Az a **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-158">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="6f00e-159">Az a **a DSC-konfigurációk** panelen kattintson a **TestConfig** (a korábban importált konfiguráció neve).</span><span class="sxs-lookup"><span data-stu-id="6f00e-159">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="6f00e-160">Az a **fordítási feladatok** csempéjén a **TestConfig konfigurációs** panelen kattintson a felsorolt feladatok bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-160">On the **Compilation jobs** tile of the **TestConfig Configuration** blade, click on any of the jobs listed.</span></span> <span data-ttu-id="6f00e-161">A **fordítási feladat** panelen nyitja meg, a fordítási feladat el lett indítva dátumával.</span><span class="sxs-lookup"><span data-stu-id="6f00e-161">A **Compilation Job** blade opens, labeled with the date that the compilation job was started.</span></span>
   
    ![A fordítási feladat paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="6f00e-163">Kattintson az összes csempe az a **fordítási feladat** panelen tekintse meg a további részleteket a feladat.</span><span class="sxs-lookup"><span data-stu-id="6f00e-163">Click on any tile in the **Compilation Job** blade to see further details about the job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="6f00e-164">Csomópont-konfigurációt megtekintése</span><span class="sxs-lookup"><span data-stu-id="6f00e-164">Viewing node configurations</span></span>
<span data-ttu-id="6f00e-165">Egy fordítási feladat sikeres létrehozása után egy vagy több új csomópont-konfigurációkat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6f00e-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="6f00e-166">A csomópont-konfiguráció a lekérési kiszolgálójával, és készen áll a lekért és egy vagy több csomópont által alkalmazott telepített MOF dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6f00e-166">A node configuration is a MOF document that is deployed to the pull server and ready to be pulled and applied by one or more nodes.</span></span> <span data-ttu-id="6f00e-167">Az Automation-fiókban lévő meg tudja tekinteni a csomópont-konfigurációt a **DSC-Csomópontkonfigurációk** panelen.</span><span class="sxs-lookup"><span data-stu-id="6f00e-167">You can view the node configurations in your Automation account in the **DSC Node Configurations** blade.</span></span> <span data-ttu-id="6f00e-168">A csomópont-konfiguráció van az űrlap egy nevű *ConfigurationName*. *Csomópontnév*.</span><span class="sxs-lookup"><span data-stu-id="6f00e-168">A node configuration has a name with the form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="6f00e-169">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-169">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-170">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-170">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-171">Az a **Automation-fiók** panelen kattintson a **DSC-Csomópontkonfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-171">On the **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![A DSC-Csomópontkonfigurációk paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="6f00e-173">Egy Azure virtuális gép Management az Azure Automation DSC Szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="6f00e-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="6f00e-174">Azure Automation DSC segítségével kezelheti az Azure virtuális gépeken (klasszikus és Resource Manager), a helyszíni virtuális gépek, Linux rendszerű gépek, AWS virtuális gépek és a helyszíni fizikai gépeket.</span><span class="sxs-lookup"><span data-stu-id="6f00e-174">You can use Azure Automation DSC to manage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="6f00e-175">Ebben a témakörben azt fedezi történő előkészítésére csak Azure Resource Manager virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6f00e-175">In this topic, we cover how to onboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="6f00e-176">Gépek, más típusú bevezetési kapcsolatos információk: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="6f00e-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="6f00e-177">A bevezetni az Azure Resource Manager virtuális gép Azure Automation DSC általi kezelésre</span><span class="sxs-lookup"><span data-stu-id="6f00e-177">To onboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="6f00e-178">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-179">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-179">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-180">Az a **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-180">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="6f00e-181">Az a **DSC-csomópontok** panelen kattintson a **adja hozzá az Azure virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-181">In the **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Az Azure VM hozzáadása gomb kiemelés DSC-csomópontok paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="6f00e-183">Az a **adja hozzá az Azure virtuális gépek** panelen kattintson a **válassza ki a bevezetni kívánt virtuális gépeket**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-183">In the **Add Azure VMs** blade, click **Select virtual machines to onboard**.</span></span>
6. <span data-ttu-id="6f00e-184">Az a **válassza ki a virtuális gépek** panelen, válassza ki a virtuális gép kívánt bevezetésében, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-184">In the **Select VMs** blade, select the VM you want to onboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="6f00e-185">Az értéknek kell lennie az Azure Resource Manager virtuális gép Windows Server 2008 R2 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6f00e-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="6f00e-186">Az a **adja hozzá az Azure virtuális gépek** panelen kattintson a **regisztrációs adatok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-186">In the **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="6f00e-187">Az a **regisztrációs** panelen adja meg a virtuális gépre, az alkalmazni kívánt csomópont-konfiguráció nevét a **csomópont-konfiguráció neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6f00e-187">In the **Registration** blade, enter the name of the node configuration you want to apply to the VM in the **Node Configuration Name** box.</span></span> <span data-ttu-id="6f00e-188">Ez pontosan egyeznie kell a csomópont-konfiguráció az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-188">This must exactly match the name of a node configuration in the Automation account.</span></span> <span data-ttu-id="6f00e-189">Ezen a ponton biztosító nevét nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="6f00e-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="6f00e-190">A csomópont bevezetése után a hozzárendelt csomópont-konfiguráció módosítása</span><span class="sxs-lookup"><span data-stu-id="6f00e-190">You can change the assigned node configuration after onboarding the node.</span></span>
   <span data-ttu-id="6f00e-191">Ellenőrizze **szükség esetén újraindítás csomópont**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![A regisztrációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="6f00e-193">A megadott csomópont-konfiguráció által meghatározott időközönként alkalmazzuk a virtuális gép a **konfigurációs mód gyakoriságának**, és a virtuális gép frissítések a csomópont-konfiguráció által meghatározott időközönként ellenőrzi a  **Frissítési gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-193">The node configuration you specified will be applied to the VM at intervals specified by the **Configuration Mode Frequency**, and the VM will check for updates to the node configuration at intervals specified by the **Refresh Frequency**.</span></span> <span data-ttu-id="6f00e-194">Ezek az értékek módjáról további információkért lásd: [konfigurálása a helyi Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="6f00e-194">For more information about how these values are used, see [Configuring the Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="6f00e-195">Az a **adja hozzá az Azure virtuális gépek** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-195">In the **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="6f00e-196">Azure indul el a virtuális gép folyamatán.</span><span class="sxs-lookup"><span data-stu-id="6f00e-196">Azure will start the process of onboarding the VM.</span></span> <span data-ttu-id="6f00e-197">Ha befejeződött, a virtuális Gépet fog megjelenni a **DSC-csomópontok** az Automation-fiók panelén.</span><span class="sxs-lookup"><span data-stu-id="6f00e-197">When it is complete, the VM will show up in the **DSC Nodes** blade in the Automation account.</span></span>

## <a name="viewing-the-list-of-dsc-nodes"></a><span data-ttu-id="6f00e-198">A DSC-csomópontok listájának megtekintése</span><span class="sxs-lookup"><span data-stu-id="6f00e-198">Viewing the list of DSC nodes</span></span>
<span data-ttu-id="6f00e-199">Lett előkészítve felügyeletre a az Automation-fiókban lévő összes gép listáját megtekintheti a **DSC-csomópontok** panelen.</span><span class="sxs-lookup"><span data-stu-id="6f00e-199">You can view the list of all machines that have been onboarded for management in your Automation account in the **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="6f00e-200">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-200">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-201">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-201">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-202">Az a **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-202">On the **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="6f00e-203">A DSC-csomópontok számára jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="6f00e-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="6f00e-204">Minden alkalommal, amikor az Azure Automation DSC a felügyelt csomóponton konzisztencia-ellenőrzést végez a csomópont visszaküldi egy állapotjelentést a lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="6f00e-204">Each time Azure Automation DSC performs a consistency check on a managed node, the node sends a status report back to the pull server.</span></span> <span data-ttu-id="6f00e-205">Az adott csomópont a panelen megtekintheti ezeket a jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="6f00e-205">You can view these reports on the blade for that node.</span></span>

1. <span data-ttu-id="6f00e-206">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-206">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-207">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-207">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-208">Az a **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-208">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="6f00e-209">Az a **jelentések** csempére, kattintson a jelentések listájában.</span><span class="sxs-lookup"><span data-stu-id="6f00e-209">On the **Reports** tile, click on any of the reports in the list.</span></span>
   
    ![A jelentés paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="6f00e-211">Egy egyedi jelentés paneljén megtekintheti a következő állapotadatokat a megfelelő konzisztencia-ellenőrzés céljából:</span><span class="sxs-lookup"><span data-stu-id="6f00e-211">On the blade for an individual report, you can see the following status information for the corresponding consistency check:</span></span>

* <span data-ttu-id="6f00e-212">A jelentés állapotának – hogy a csomópont "Megfelelő", a konfiguráció "Sikertelen" vagy a csomópont "Nem megfelelő" (ha van a csomópont **applyandmonitor** mód és a gép nincs a kívánt állapot).</span><span class="sxs-lookup"><span data-stu-id="6f00e-212">The report status — whether the node is "Compliant", the configuration "Failed", or the node is "Not Compliant" (when the node is in **applyandmonitor** mode and the machine is not in the desired state).</span></span>
* <span data-ttu-id="6f00e-213">A konzisztencia-ellenőrzés kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-213">The start time for the consistency check.</span></span>
* <span data-ttu-id="6f00e-214">A teljes futási a konzisztencia-ellenőrzés céljából.</span><span class="sxs-lookup"><span data-stu-id="6f00e-214">The total runtime for the consistency check.</span></span>
* <span data-ttu-id="6f00e-215">A konzisztencia-ellenőrzés típusa.</span><span class="sxs-lookup"><span data-stu-id="6f00e-215">The type of consistency check.</span></span>
* <span data-ttu-id="6f00e-216">Hibáit, beleértve a hibakódot és a hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="6f00e-216">Any errors, including the error code and error message.</span></span> 
* <span data-ttu-id="6f00e-217">A konfigurációban, és minden egyes (hogy a csomópont állapotban van a kívánt erőforrás) erőforrás állapotának használt DSC erőforrásokat – meg az egyes erőforrások további részletes információk az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="6f00e-217">Any DSC resources used in the configuration, and the state of each resource (whether the node is in the desired state for that resource) — you can click on each resource to get more detailed information for that resource.</span></span>
* <span data-ttu-id="6f00e-218">A név, IP-cím és a csomópont konfigurációs módja.</span><span class="sxs-lookup"><span data-stu-id="6f00e-218">The name, IP address, and configuration mode of the node.</span></span>

<span data-ttu-id="6f00e-219">Is **nyers jelentés megtekintéséhez** a csomópont küld a kiszolgálónak a tényleges adatokat.</span><span class="sxs-lookup"><span data-stu-id="6f00e-219">You can also click **View raw report** to see the actual data that the node sends to the server.</span></span> <span data-ttu-id="6f00e-220">Adatok használatával kapcsolatos további információkért lásd: [a DSC-jelentés kiszolgálóval](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="6f00e-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="6f00e-221">Miután egy csomópont előkészítve, mielőtt az első jelentés érhető el némi időbe telhet.</span><span class="sxs-lookup"><span data-stu-id="6f00e-221">It can take some time after a node is onboarded before the first report is available.</span></span> <span data-ttu-id="6f00e-222">Várjon, amíg az első jelentés akár 30 percet követően bevezetésében csomópont módosítania.</span><span class="sxs-lookup"><span data-stu-id="6f00e-222">You might need to wait up to 30 minutes for the first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-to-a-different-node-configuration"></a><span data-ttu-id="6f00e-223">Egy csomópont egy másik csomópont-konfiguráció újbóli hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6f00e-223">Reassigning a node to a different node configuration</span></span>
<span data-ttu-id="6f00e-224">Egy csomópont kezdetben rendelve egy másik csomópont-konfiguráció rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="6f00e-224">You can assign a node to use a different node configuration than the one you initially assigned.</span></span>

1. <span data-ttu-id="6f00e-225">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-225">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-226">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-226">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-227">Az a **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-227">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="6f00e-228">Az a **DSC-csomópontok** panelen kattintson a szeretne rendelni a csomópont neve.</span><span class="sxs-lookup"><span data-stu-id="6f00e-228">On the **DSC Nodes** blade, click on the name of the node you want to reassign.</span></span>
5. <span data-ttu-id="6f00e-229">Az adott csomópont paneljén kattintson **hozzárendelése csomópont**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-229">On the blade for that node, click **Assign node**.</span></span>
   
    ![A csomópont hozzárendelése gomb kiemelés csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="6f00e-231">Az a **csomópont-konfiguráció hozzárendelése** panelen válassza ki a kívánt rendelje hozzá a csomópontot, és kattintson a csomópont-konfiguráció **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-231">On the **Assign Node Configuration** blade, select the node configuration to which you want to assign the node, and then click **OK**.</span></span>
   
    ![A csomópont-konfiguráció hozzárendelése paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="6f00e-233">Egy csomópont</span><span class="sxs-lookup"><span data-stu-id="6f00e-233">Unregistering a node</span></span>
<span data-ttu-id="6f00e-234">Ha már nem szeretné, hogy egy csomópont Azure Automation DSC Szolgáltatásban történő kezeléshez, regisztrációjának megszüntetésére.</span><span class="sxs-lookup"><span data-stu-id="6f00e-234">If you no longer want a node to be managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="6f00e-235">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f00e-235">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6f00e-236">A központ menüben kattintson a **összes erőforrás** és majd az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6f00e-236">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="6f00e-237">Az a **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-237">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="6f00e-238">Az a **DSC-csomópontok** panelen kattintson a kívánt regisztrációjának törlése a csomópont neve.</span><span class="sxs-lookup"><span data-stu-id="6f00e-238">On the **DSC Nodes** blade, click on the name of the node you want to unregister.</span></span>
5. <span data-ttu-id="6f00e-239">Az adott csomópont paneljén kattintson **Unregister**.</span><span class="sxs-lookup"><span data-stu-id="6f00e-239">On the blade for that node, click **Unregister**.</span></span>
   
    ![A Unregister gomb kiemelés csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="6f00e-241">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="6f00e-241">Related Articles</span></span>
* [<span data-ttu-id="6f00e-242">Azure Automation DSC – áttekintés</span><span class="sxs-lookup"><span data-stu-id="6f00e-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="6f00e-243">Azure Automation DSC általi kezelésre bevezetési gépek</span><span class="sxs-lookup"><span data-stu-id="6f00e-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="6f00e-244">A Windows PowerShell célállapot-konfiguráló áttekintése</span><span class="sxs-lookup"><span data-stu-id="6f00e-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="6f00e-245">Azure Automation DSC-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="6f00e-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="6f00e-246">Azure Automation DSC – díjszabás</span><span class="sxs-lookup"><span data-stu-id="6f00e-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

