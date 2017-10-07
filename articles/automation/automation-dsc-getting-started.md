---
title: "Azure Automation DSC használatába aaaGetting |} Microsoft Docs"
description: "MAGYARÁZAT és példákat a hello leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC)"
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
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="fff6e-103">Ismerkedés az Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="fff6e-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="fff6e-104">Ez a témakör azt ismerteti, hogyan toodo hello leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC), például a létrehozása, importálása, és konfigurációk, gépek túl kezelése, bevezetési fordítása és jelentések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="fff6e-104">This topic explains how toodo hello most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines too manage, and viewing reports.</span></span> <span data-ttu-id="fff6e-105">Milyen Azure Automation DSC áttekintést van, a következő témakörben: [Azure Automation DSC – áttekintés](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fff6e-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="fff6e-106">A DSC-dokumentáció, lásd: [Windows PowerShell kívánt állapot beállítása – áttekintés](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="fff6e-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="fff6e-107">Ez a témakör egy részletes útmutató toousing Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="fff6e-107">This topic provides a step-by-step guide toousing Azure Automation DSC.</span></span> <span data-ttu-id="fff6e-108">Ha azt szeretné, hogy egy minta-környezet, amely már be van állítva a jelen témakörben ismertetett hello lépések nélkül, [ARM-sablon következő hello](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="fff6e-108">If you want a sample environment that is already set up without following hello steps described in this topic, you can use [hello following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="fff6e-109">Ez a sablon állít be egy befejezett Azure Automation DSC-környezetben, például egy Azure Automation DSC által felügyelt Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="fff6e-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fff6e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fff6e-110">Prerequisites</span></span>
<span data-ttu-id="fff6e-111">Ebben a témakörben toocomplete hello példákért hello következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="fff6e-111">toocomplete hello examples in this topic, hello following are required:</span></span>

* <span data-ttu-id="fff6e-112">Egy Azure Automation-fiókra.</span><span class="sxs-lookup"><span data-stu-id="fff6e-112">An Azure Automation account.</span></span> <span data-ttu-id="fff6e-113">Azure Automation futtató fiók létrehozásával kapcsolatos információkért tekintse meg az [Azure-beli futtató fiókkal](automation-sec-configure-azure-runas-account.md) kapcsolatos részt.</span><span class="sxs-lookup"><span data-stu-id="fff6e-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="fff6e-114">Az Azure Resource Manager virtuális gép (klasszikus) Windows Server 2008 R2 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fff6e-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="fff6e-115">Virtuális gép létrehozása, lásd: [hello Azure-portálon az első Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="fff6e-115">For instructions on creating a VM, see [Create your first Windows virtual machine in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="fff6e-116">A DSC-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="fff6e-116">Creating a DSC configuration</span></span>
<span data-ttu-id="fff6e-117">Létre fogunk hozni egy egyszerű [DSC-konfiguráció](https://msdn.microsoft.com/powershell/dsc/configurations) hello jelenléte vagy hiánya hello, ezzel biztosítható **webkiszolgáló** Windows szolgáltatás (IIS), attól függően, hogy hogyan lehet kijelölni csomópontok.</span><span class="sxs-lookup"><span data-stu-id="fff6e-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either hello presence or absence of hello **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="fff6e-118">Indítsa el a Windows PowerShell ISE hello (vagy bármilyen szövegszerkesztővel).</span><span class="sxs-lookup"><span data-stu-id="fff6e-118">Start hello Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="fff6e-119">Írja be a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="fff6e-119">Type hello following text:</span></span>
   
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
3. <span data-ttu-id="fff6e-120">Hello fájl mentése másként `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="fff6e-120">Save hello file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="fff6e-121">Ez a konfiguráció meghívja egy erőforrást minden egyes csomópont blokkban hello [WindowsFeature erőforrás](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), hello jelenléte vagy hiánya hello, ezzel biztosítható **webkiszolgáló** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fff6e-121">This configuration calls one resource in each node block, hello [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either hello presence or absence of hello **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="fff6e-122">Azure Automation konfiguráció importálása</span><span class="sxs-lookup"><span data-stu-id="fff6e-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="fff6e-123">A következő hello konfigurációs, azt fogja importálni hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="fff6e-123">Next, we'll import hello configuration into hello Automation account.</span></span>

1. <span data-ttu-id="fff6e-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-125">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-125">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-126">A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-126">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="fff6e-127">A hello **a DSC-konfigurációk** panelen kattintson a **hozzáadása egy konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-127">On hello **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="fff6e-128">A hello **konfiguráció importálása** panelen, a Tallózás toohello `TestConfig.ps1` fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fff6e-128">On hello **Import Configuration** blade, browse toohello `TestConfig.ps1` file on your computer.</span></span>
   
    ![Képernyőfelvétel a hello ** importálási konfigurációs ** panel](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="fff6e-130">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fff6e-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="fff6e-131">A beállítások megjelenítése az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="fff6e-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="fff6e-132">Miután importálta a konfiguráció, megtekintheti a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fff6e-132">After you have imported a configuration, you can view it in hello Azure portal.</span></span>

1. <span data-ttu-id="fff6e-133">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-133">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-134">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-134">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-135">A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="fff6e-135">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="fff6e-136">A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (Ez az importált hello konfigurációs hello neve hello előző eljárásban).</span><span class="sxs-lookup"><span data-stu-id="fff6e-136">On hello **DSC Configurations** blade, click **TestConfig** (this is hello name of hello configuration you imported in hello previous procedure).</span></span>
5. <span data-ttu-id="fff6e-137">A hello **TestConfig konfigurációs** panelen kattintson a **konfigurációs forrás megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-137">On hello **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![Hello TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="fff6e-139">A **TestConfig konfigurációs forrás** panel nyílik hello PowerShell-kódjába hello konfigurációs megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="fff6e-139">A **TestConfig Configuration source** blade opens, displaying hello PowerShell code for hello configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="fff6e-140">Az Azure Automationben konfiguráció fordítása</span><span class="sxs-lookup"><span data-stu-id="fff6e-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="fff6e-141">A kívánt állapot tooa csomópont alkalmazása előtt a DSC-konfiguráció meghatározása az állapotban kell egy vagy több csomópont-konfigurációt (MOF dokumentum) lefordítva, és hello Automation DSC lekéréses kiszolgáló helyezve.</span><span class="sxs-lookup"><span data-stu-id="fff6e-141">Before you can apply a desired state tooa node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on hello Automation DSC Pull Server.</span></span> <span data-ttu-id="fff6e-142">Azure Automation DSC-konfigurációja összeállításának részletes ismertetését lásd: [Azure Automation DSC-konfigurációja fordítása](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="fff6e-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="fff6e-143">Konfiguráció fordítása kapcsolatos további információkért lásd: [a DSC-konfigurációk](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="fff6e-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="fff6e-144">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-144">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-145">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-145">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-146">A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="fff6e-146">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="fff6e-147">A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (korábban importált konfigurációs hello hello neve).</span><span class="sxs-lookup"><span data-stu-id="fff6e-147">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="fff6e-148">A hello **TestConfig konfigurációs** panelen kattintson a **fordítási**, és kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-148">On hello **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="fff6e-149">Egy fordítási feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="fff6e-149">This starts a compilation job.</span></span>
   
    ![Fordítási gomb kiemelés hello TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="fff6e-151">Ha egy Azure Automation konfigurációs lefordításához bármely létrehozott csomópont konfigurációs MOF-ot toohello lekérési kiszolgálójával automatikusan telepíti.</span><span class="sxs-lookup"><span data-stu-id="fff6e-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs toohello pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="fff6e-152">Egy fordítási feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="fff6e-152">Viewing a compilation job</span></span>
<span data-ttu-id="fff6e-153">A fordítás megkezdése után megtekintheti az hello **fordítási feladatok** hello csempére **konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="fff6e-153">After you start a compilation, you can view it in hello **Compilation jobs** tile in hello **Configuration** blade.</span></span> <span data-ttu-id="fff6e-154">Hello **fordítási feladatok** csempe megjeleníti a jelenleg futó, befejeződött, és sikertelen feladatok.</span><span class="sxs-lookup"><span data-stu-id="fff6e-154">hello **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="fff6e-155">Amikor megnyit egy fordítási feladat panelen, azt mutatja, beleértve az esetleges hibákat vagy figyelmeztetéseket észlelt feladatot információt, hello konfigurációja és fordítási használt bemeneti paraméterek naplókat.</span><span class="sxs-lookup"><span data-stu-id="fff6e-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in hello configuration, and compilation logs.</span></span>

1. <span data-ttu-id="fff6e-156">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-157">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-157">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-158">A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-158">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="fff6e-159">A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (korábban importált konfigurációs hello hello neve).</span><span class="sxs-lookup"><span data-stu-id="fff6e-159">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="fff6e-160">A hello **fordítási feladatok** hello csempéjéről **TestConfig konfigurációs** panelen kattintson a felsorolt hello feladatok bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-160">On hello **Compilation jobs** tile of hello **TestConfig Configuration** blade, click on any of hello jobs listed.</span></span> <span data-ttu-id="fff6e-161">A **fordítási feladat** panel nyílik fordítási feladat hello hello dátummal feliratú lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="fff6e-161">A **Compilation Job** blade opens, labeled with hello date that hello compilation job was started.</span></span>
   
    ![Hello fordítási feladat paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="fff6e-163">Kattintson az összes csempe hello az **fordítási feladat** panel toosee további hello feladat vonatkozó részletek.</span><span class="sxs-lookup"><span data-stu-id="fff6e-163">Click on any tile in hello **Compilation Job** blade toosee further details about hello job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="fff6e-164">Csomópont-konfigurációt megtekintése</span><span class="sxs-lookup"><span data-stu-id="fff6e-164">Viewing node configurations</span></span>
<span data-ttu-id="fff6e-165">Egy fordítási feladat sikeres létrehozása után egy vagy több új csomópont-konfigurációkat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fff6e-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="fff6e-166">A csomópont-konfiguráció, amely telepített toohello lekérési kiszolgálójával, és készen áll a toobe lekért és egy vagy több csomópont által alkalmazott MOF dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fff6e-166">A node configuration is a MOF document that is deployed toohello pull server and ready toobe pulled and applied by one or more nodes.</span></span> <span data-ttu-id="fff6e-167">Az Automation-fiókban lévő hello meg tudja tekinteni hello csomópont-konfigurációt **DSC-Csomópontkonfigurációk** panelen.</span><span class="sxs-lookup"><span data-stu-id="fff6e-167">You can view hello node configurations in your Automation account in hello **DSC Node Configurations** blade.</span></span> <span data-ttu-id="fff6e-168">A csomópont-konfiguráció van egy hello űrlap nevű *ConfigurationName*. *Csomópontnév*.</span><span class="sxs-lookup"><span data-stu-id="fff6e-168">A node configuration has a name with hello form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="fff6e-169">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-170">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-170">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-171">A hello **Automation-fiók** panelen kattintson a **DSC-Csomópontkonfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-171">On hello **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![Hello DSC-Csomópontkonfigurációk paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="fff6e-173">Egy Azure virtuális gép Management az Azure Automation DSC Szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="fff6e-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="fff6e-174">Azure Automation DSC toomanage Azure virtuális gépeken (klasszikus és Resource Manager), a helyszíni virtuális gépek, Linux rendszerű gépek, AWS virtuális gépek és a helyszíni fizikai gépeket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fff6e-174">You can use Azure Automation DSC toomanage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="fff6e-175">Ebben a témakörben azt fedezi hogyan tooonboard csak a Azure Resource Manager virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="fff6e-175">In this topic, we cover how tooonboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="fff6e-176">Gépek, más típusú bevezetési kapcsolatos információk: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="fff6e-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="fff6e-177">az Azure Resource Manager virtuális gép Azure Automation DSC általi kezelésre tooonboard</span><span class="sxs-lookup"><span data-stu-id="fff6e-177">tooonboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="fff6e-178">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-179">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-179">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-180">A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-180">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="fff6e-181">A hello **DSC-csomópontok** panelen kattintson a **adja hozzá az Azure virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-181">In hello **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Hello Azure VM hozzáadása gomb kiemelés hello DSC-csomópontok paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="fff6e-183">A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **válassza ki a virtuális gépek tooonboard**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-183">In hello **Add Azure VMs** blade, click **Select virtual machines tooonboard**.</span></span>
6. <span data-ttu-id="fff6e-184">A hello **válassza ki a virtuális gépek** panelen, jelölje be hello tooonboard szeretne, és kattintson a virtuális gép **OK**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-184">In hello **Select VMs** blade, select hello VM you want tooonboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="fff6e-185">Az értéknek kell lennie az Azure Resource Manager virtuális gép Windows Server 2008 R2 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fff6e-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="fff6e-186">A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **konfigurálása a regisztrációs adatok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-186">In hello **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="fff6e-187">A hello **regisztrációs** panelen hello beírása hello csomópont-konfiguráció tooapply toohello VM hello a kívánt **csomópont-konfiguráció neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fff6e-187">In hello **Registration** blade, enter hello name of hello node configuration you want tooapply toohello VM in hello **Node Configuration Name** box.</span></span> <span data-ttu-id="fff6e-188">A csomópont-konfiguráció a hello Automation-fiók neve hello egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="fff6e-188">This must exactly match hello name of a node configuration in hello Automation account.</span></span> <span data-ttu-id="fff6e-189">Ezen a ponton biztosító nevét nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="fff6e-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="fff6e-190">Csomópont-konfiguráció hozzárendelése hello bevezetési hello csomópont után módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="fff6e-190">You can change hello assigned node configuration after onboarding hello node.</span></span>
   <span data-ttu-id="fff6e-191">Ellenőrizze **szükség esetén újraindítás csomópont**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![Hello regisztrációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="fff6e-193">a megadott csomópont-konfiguráció hello lesz alkalmazott toohello VM hello által meghatározott időközönként **konfigurációs mód gyakoriságának**, és a frissítések toohello csomópont-konfiguráció hello általmeghatározottidőközönkéntellenőrzihelloVM **Frissítési gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-193">hello node configuration you specified will be applied toohello VM at intervals specified by hello **Configuration Mode Frequency**, and hello VM will check for updates toohello node configuration at intervals specified by hello **Refresh Frequency**.</span></span> <span data-ttu-id="fff6e-194">Ezek az értékek módjáról további információkért lásd: [konfigurálása hello helyi Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="fff6e-194">For more information about how these values are used, see [Configuring hello Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="fff6e-195">A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-195">In hello **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="fff6e-196">Azure VM bevezetési hello hello folyamata elindul.</span><span class="sxs-lookup"><span data-stu-id="fff6e-196">Azure will start hello process of onboarding hello VM.</span></span> <span data-ttu-id="fff6e-197">Amikor elkészült, hello VM fog megjelenni hello **DSC-csomópontok** hello Automation-fiók panelén.</span><span class="sxs-lookup"><span data-stu-id="fff6e-197">When it is complete, hello VM will show up in hello **DSC Nodes** blade in hello Automation account.</span></span>

## <a name="viewing-hello-list-of-dsc-nodes"></a><span data-ttu-id="fff6e-198">A DSC-csomópontok hello listájának megtekintése</span><span class="sxs-lookup"><span data-stu-id="fff6e-198">Viewing hello list of DSC nodes</span></span>
<span data-ttu-id="fff6e-199">Megtekintheti az összes gép lett előkészítve az Automation-fiókban lévő hello Management hello listája **DSC-csomópontok** panelen.</span><span class="sxs-lookup"><span data-stu-id="fff6e-199">You can view hello list of all machines that have been onboarded for management in your Automation account in hello **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="fff6e-200">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-200">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-201">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-201">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-202">A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-202">On hello **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="fff6e-203">A DSC-csomópontok számára jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="fff6e-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="fff6e-204">Minden alkalommal, amikor az Azure Automation DSC a felügyelt csomóponton konzisztencia-ellenőrzést végez hello csomópont állapota hátsó toohello lekéréses jelentéskiszolgáló küld.</span><span class="sxs-lookup"><span data-stu-id="fff6e-204">Each time Azure Automation DSC performs a consistency check on a managed node, hello node sends a status report back toohello pull server.</span></span> <span data-ttu-id="fff6e-205">Az adott csomópont hello panelen megtekintheti ezekre a jelentésekre.</span><span class="sxs-lookup"><span data-stu-id="fff6e-205">You can view these reports on hello blade for that node.</span></span>

1. <span data-ttu-id="fff6e-206">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-206">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-207">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-207">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-208">A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-208">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="fff6e-209">A hello **jelentések** csempére, az egyes hello jelentések hello listában.</span><span class="sxs-lookup"><span data-stu-id="fff6e-209">On hello **Reports** tile, click on any of hello reports in hello list.</span></span>
   
    ![Hello jelentés paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="fff6e-211">Egy egyedi jelentés hello paneljén láthatja, ellenőrizze a következő állapotadatokat hello megfelelő konzisztencia hello:</span><span class="sxs-lookup"><span data-stu-id="fff6e-211">On hello blade for an individual report, you can see hello following status information for hello corresponding consistency check:</span></span>

* <span data-ttu-id="fff6e-212">az állapot jelentése hello – e hello csomópont "Megfelelő", "Sikertelen" hello konfigurációs vagy hello csomópont "Nem megfelelő" (ha hello csomópontjának nem **applyandmonitor** hello és a gép nem szükséges hello állapotban van.).</span><span class="sxs-lookup"><span data-stu-id="fff6e-212">hello report status — whether hello node is "Compliant", hello configuration "Failed", or hello node is "Not Compliant" (when hello node is in **applyandmonitor** mode and hello machine is not in hello desired state).</span></span>
* <span data-ttu-id="fff6e-213">hello kezdési idejét hello konzisztencia-ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="fff6e-213">hello start time for hello consistency check.</span></span>
* <span data-ttu-id="fff6e-214">hello teljes futási hello konzisztencia-ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="fff6e-214">hello total runtime for hello consistency check.</span></span>
* <span data-ttu-id="fff6e-215">hello típusú konzisztencia-ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="fff6e-215">hello type of consistency check.</span></span>
* <span data-ttu-id="fff6e-216">Hibáit, beleértve a hiba kódja és hiba üdvözlőüzenetére.</span><span class="sxs-lookup"><span data-stu-id="fff6e-216">Any errors, including hello error code and error message.</span></span> 
* <span data-ttu-id="fff6e-217">Bármely hello konfigurációja és hello állapotát (hogy hello csomópont hello szükséges állapotban van az adott erőforrás) az egyes erőforrások használt DSC-erőforrások – kattinthat a minden egyes erőforrás tooget részletes információkat az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="fff6e-217">Any DSC resources used in hello configuration, and hello state of each resource (whether hello node is in hello desired state for that resource) — you can click on each resource tooget more detailed information for that resource.</span></span>
* <span data-ttu-id="fff6e-218">hello nevét, IP-cím és hello csomópont konfigurációs módja.</span><span class="sxs-lookup"><span data-stu-id="fff6e-218">hello name, IP address, and configuration mode of hello node.</span></span>

<span data-ttu-id="fff6e-219">Is **nyers jelentés megtekintéséhez** toosee hello tényleges adatokat csomópont hello toohello server küld.</span><span class="sxs-lookup"><span data-stu-id="fff6e-219">You can also click **View raw report** toosee hello actual data that hello node sends toohello server.</span></span> <span data-ttu-id="fff6e-220">Adatok használatával kapcsolatos további információkért lásd: [a DSC-jelentés kiszolgálóval](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="fff6e-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="fff6e-221">Miután csomópont előkészítve, mielőtt hello első jelentés érhető el némi időbe telhet.</span><span class="sxs-lookup"><span data-stu-id="fff6e-221">It can take some time after a node is onboarded before hello first report is available.</span></span> <span data-ttu-id="fff6e-222">Szükség lehet másolatot too30 perc toowait hello első jelentés követően bevezetésében egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="fff6e-222">You might need toowait up too30 minutes for hello first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-tooa-different-node-configuration"></a><span data-ttu-id="fff6e-223">Egy csomópont tooa másik csomópont-konfiguráció újbóli hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fff6e-223">Reassigning a node tooa different node configuration</span></span>
<span data-ttu-id="fff6e-224">Hozzárendelhet egy csomópont toouse egy másik csomópont-konfiguráció mint hello kezdetben hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="fff6e-224">You can assign a node toouse a different node configuration than hello one you initially assigned.</span></span>

1. <span data-ttu-id="fff6e-225">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-225">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-226">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-226">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-227">A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-227">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="fff6e-228">A hello **DSC-csomópontok** panelen kattintson a hello neve hello csomópont tooreassign szeretné.</span><span class="sxs-lookup"><span data-stu-id="fff6e-228">On hello **DSC Nodes** blade, click on hello name of hello node you want tooreassign.</span></span>
5. <span data-ttu-id="fff6e-229">Az adott csomópont hello paneljén kattintson **hozzárendelése csomópont**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-229">On hello blade for that node, click **Assign node**.</span></span>
   
    ![Hello hozzárendelése csomópont gomb kiemelés hello csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="fff6e-231">A hello **csomópont-konfiguráció hozzárendelése** panelen, jelölje be hello csomópont konfigurációs toowhich szeretné tooassign hello csomópont, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-231">On hello **Assign Node Configuration** blade, select hello node configuration toowhich you want tooassign hello node, and then click **OK**.</span></span>
   
    ![Hello csomópont-konfiguráció hozzárendelése paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="fff6e-233">Egy csomópont</span><span class="sxs-lookup"><span data-stu-id="fff6e-233">Unregistering a node</span></span>
<span data-ttu-id="fff6e-234">Ha már nincs szüksége egy Azure Automation DSC által kezelt csomópont toobe, regisztrációjának megszüntetésére.</span><span class="sxs-lookup"><span data-stu-id="fff6e-234">If you no longer want a node toobe managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="fff6e-235">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fff6e-235">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fff6e-236">Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fff6e-236">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="fff6e-237">A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-237">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="fff6e-238">A hello **DSC-csomópontok** panelen kattintson a hello neve hello csomópont toounregister szeretné.</span><span class="sxs-lookup"><span data-stu-id="fff6e-238">On hello **DSC Nodes** blade, click on hello name of hello node you want toounregister.</span></span>
5. <span data-ttu-id="fff6e-239">Az adott csomópont hello paneljén kattintson **Unregister**.</span><span class="sxs-lookup"><span data-stu-id="fff6e-239">On hello blade for that node, click **Unregister**.</span></span>
   
    ![Hello Unregister gomb kiemelés hello csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="fff6e-241">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="fff6e-241">Related Articles</span></span>
* [<span data-ttu-id="fff6e-242">Azure Automation DSC – áttekintés</span><span class="sxs-lookup"><span data-stu-id="fff6e-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="fff6e-243">Azure Automation DSC általi kezelésre bevezetési gépek</span><span class="sxs-lookup"><span data-stu-id="fff6e-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="fff6e-244">A Windows PowerShell célállapot-konfiguráló áttekintése</span><span class="sxs-lookup"><span data-stu-id="fff6e-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="fff6e-245">Azure Automation DSC-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="fff6e-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="fff6e-246">Azure Automation DSC – díjszabás</span><span class="sxs-lookup"><span data-stu-id="fff6e-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

