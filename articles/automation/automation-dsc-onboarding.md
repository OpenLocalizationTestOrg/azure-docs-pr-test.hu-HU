---
title: "Azure Automation DSC általi kezelésre aaaOnboarding gépek |} Microsoft Docs"
description: "Hogyan toosetup gépek-kezelés az Azure Automation DSC"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
ms.assetid: da13e1f5-2a1c-443b-8e3b-9f0d6f9e4810
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 12/13/2016
ms.author: eslesar
ms.openlocfilehash: ef15801fec2ffea4ba62dcba2fbe9af09268e424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a><span data-ttu-id="e2b6e-103">Azure Automation DSC általi kezelésre bevezetési gépek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-103">Onboarding machines for management by Azure Automation DSC</span></span>

## <a name="why-manage-machines-with-azure-automation-dsc"></a><span data-ttu-id="e2b6e-104">Miért kezelése az Azure Automation DSC Szolgáltatásban gépek?</span><span class="sxs-lookup"><span data-stu-id="e2b6e-104">Why manage machines with Azure Automation DSC?</span></span>

<span data-ttu-id="e2b6e-105">Például [PowerShell célállapot-konfiguráció](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation célállapot-konfiguráció egy olyan egyszerű, mégis hatékony, konfigurációs felügyeleti szolgáltatás a DSC-csomópontok (fizikai és virtuális gépek) számára a felhőalapú vagy helyszíni adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-105">Like [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is a simple, yet powerful, configuration management service for DSC nodes (physical and virtual machines) in any cloud or on-premises datacenter.</span></span> <span data-ttu-id="e2b6e-106">Lehetővé teszi méretezhetőség több ezer gép között gyors és egyszerű központi, biztonságos helyről.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-106">It enables scalability across thousands of machines quickly and easily from a central, secure location.</span></span> <span data-ttu-id="e2b6e-107">Könnyen előkészítésére gépek is előfordulhatnak, rendelje hozzá őket deklaratív konfigurációk és nézet jelentéseken minden számítógép megadott megfelelőségi szükséges toohello állapota.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-107">You can easily onboard machines, assign them declarative configurations, and view reports showing each machine's compliance toohello desired state you specified.</span></span> <span data-ttu-id="e2b6e-108">hello Azure Automation DSC alkalmazáskezelési réteg tooDSC milyen hello Azure Automation felügyeleti réteg van tooPowerShell parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-108">hello Azure Automation DSC management layer is tooDSC what hello Azure Automation management layer is tooPowerShell scripting.</span></span> <span data-ttu-id="e2b6e-109">Más szóval hello Azure Automation segít ugyanúgy felügyelhetők PowerShell-parancsfájlok, emellett segítséget nyújt a DSC-konfigurációk kezelése.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-109">In other words, in hello same way that Azure Automation helps you manage PowerShell scripts, it also helps you manage DSC configurations.</span></span> <span data-ttu-id="e2b6e-110">További információ az Azure Automation DSC használatának előnyei hello toolearn lásd [Azure Automation DSC – áttekintés](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e2b6e-110">toolearn more about hello benefits of using Azure Automation DSC, see [Azure Automation DSC overview](automation-dsc-overview.md).</span></span>

<span data-ttu-id="e2b6e-111">Az Azure Automation DSC használt toomanage gépek számos lehet:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-111">Azure Automation DSC can be used toomanage a variety of machines:</span></span>

* <span data-ttu-id="e2b6e-112">Az Azure virtuális gépek (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="e2b6e-112">Azure virtual machines (classic)</span></span>
* <span data-ttu-id="e2b6e-113">Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-113">Azure virtual machines</span></span>
* <span data-ttu-id="e2b6e-114">Amazon Web Services (AWS) virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-114">Amazon Web Services (AWS) virtual machines</span></span>
* <span data-ttu-id="e2b6e-115">A Windows fizikai vagy virtuális gépek a helyszíni vagy felhőben működő Azure/AWS eltérő</span><span class="sxs-lookup"><span data-stu-id="e2b6e-115">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>
* <span data-ttu-id="e2b6e-116">Fizikai vagy virtuális Linux számítógépek, a helyszíni, az Azure-ban, vagy eltérő Azure felhőben</span><span class="sxs-lookup"><span data-stu-id="e2b6e-116">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="e2b6e-117">Emellett ha még nem kész toomanage gépkonfiguráció hello felhőből, Azure Automation DSC is használható csak a jelentés-végpontként.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-117">In addition, if you are not ready toomanage machine configuration from hello cloud, Azure Automation DSC can also be used as a report-only endpoint.</span></span> <span data-ttu-id="e2b6e-118">Ez lehetővé teszi tooset (leküldéses) szükségeskonfiguráció helyszíni DSC keresztül, és gazdag jelentéskészítési részleteinek megtekintése hello csomópont való megfelelés szükséges állapotát az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-118">This allows you tooset (push) desired configuration through DSC on-premises and view rich reporting details on node compliance with hello desired state in Azure Automation.</span></span>

<span data-ttu-id="e2b6e-119">a következő szakaszok hello helyzeteit vázolják fel, hogyan zajlik bevezetésében minden gép tooAzure Automation DSC típusú.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-119">hello following sections outline how you can onboard each type of machine tooAzure Automation DSC.</span></span>

## <a name="azure-virtual-machines-classic"></a><span data-ttu-id="e2b6e-120">Az Azure virtuális gépek (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="e2b6e-120">Azure virtual machines (classic)</span></span>

<span data-ttu-id="e2b6e-121">Az Azure Automation DSC Szolgáltatásban könnyen bevezetni az Azure virtuális gépek (klasszikus) konfigurációs Management hello Azure-portálon, vagy a PowerShell használatával is.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-121">With Azure Automation DSC, you can easily onboard Azure virtual machines (classic) for configuration management using either hello Azure portal, or PowerShell.</span></span> <span data-ttu-id="e2b6e-122">Hello technikai részletek alatt, és a virtuális gép hello tooremote rendelkező rendszergazda nélkül a hello Azure VM célállapot-konfiguráció bővítmény hello VM regisztrálja az Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-122">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="e2b6e-123">Mivel hello Azure VM célállapot-konfiguráció bővítmény előrehaladásával lépéseket tootrack aszinkron módon futtatja, és a hibakeresést, igénybe hello [ **hibaelhárítási Azure virtuális gép bevezetési** ](#troubleshooting-azure-virtual-machine-onboarding)az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-123">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="e2b6e-124">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e2b6e-124">Azure portal</span></span>

<span data-ttu-id="e2b6e-125">A hello [Azure-portálon](http://portal.azure.com/), kattintson a **Tallózás** -> **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-125">In hello [Azure portal](http://portal.azure.com/), click **Browse** -> **Virtual machines (classic)**.</span></span> <span data-ttu-id="e2b6e-126">Válassza ki a Windows virtuális gép kívánt tooonboard hello.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-126">Select hello Windows VM you want tooonboard.</span></span> <span data-ttu-id="e2b6e-127">Hello virtuális gép irányítópult paneljén kattintson **összes beállítás** -> **bővítmények** -> **Hozzáadás**  ->   **Azure Automation DSC** -> **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-127">On hello virtual machine's dashboard blade, click **All settings** -> **Extensions** -> **Add** -> **Azure Automation DSC** -> **Create**.</span></span> <span data-ttu-id="e2b6e-128">Adja meg a hello [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) a használati eset, az Automation-fiók regisztrációs kulcsot és regisztrációs URL-címet, és nem kötelező a csomópont konfigurációs tooassign toohello virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-128">Enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, your Automation account's registration key and registration URL, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

<span data-ttu-id="e2b6e-129">toofind hello regisztrációs URL-cím és a kulcs hello automatizálási fiókot tooonboard hello gép, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-129">toofind hello registration URL and key for hello Automation account tooonboard hello machine to, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="e2b6e-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2b6e-130">PowerShell</span></span>

```powershell
# log in tooboth Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in hello name of a Node Configuration in Azure Automation DSC, for this VM tooconform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use hello DSC extension tooonboard hello VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

## <a name="azure-virtual-machines"></a><span data-ttu-id="e2b6e-131">Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-131">Azure virtual machines</span></span>

<span data-ttu-id="e2b6e-132">Az Azure Automation DSC lehetővé teszi a konfigurációkezelésre, könnyen bevezetni az Azure virtuális gépek hello Azure-portál, Azure Resource Manager-sablonok, vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-132">Azure Automation DSC lets you easily onboard Azure virtual machines for configuration management, using either hello Azure portal, Azure Resource Manager templates, or PowerShell.</span></span> <span data-ttu-id="e2b6e-133">Hello technikai részletek alatt, és a virtuális gép hello tooremote rendelkező rendszergazda nélkül a hello Azure VM célállapot-konfiguráció bővítmény hello VM regisztrálja az Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-133">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="e2b6e-134">Mivel hello Azure VM célállapot-konfiguráció bővítmény előrehaladásával lépéseket tootrack aszinkron módon futtatja, és a hibakeresést, igénybe hello [ **hibaelhárítási Azure virtuális gép bevezetési** ](#troubleshooting-azure-virtual-machine-onboarding)az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-134">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="e2b6e-135">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e2b6e-135">Azure portal</span></span>

<span data-ttu-id="e2b6e-136">A hello [Azure-portálon](https://portal.azure.com/), keresse meg a kívánt virtuális gépek tooonboard toohello Azure Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-136">In hello [Azure portal](https://portal.azure.com/), navigate toohello Azure Automation account where you want tooonboard virtual machines.</span></span> <span data-ttu-id="e2b6e-137">Az automatizálási fiók irányítópultján hello kattintson **DSC-csomópontok** -> **adja hozzá az Azure virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-137">On hello Automation account dashboard, click **DSC Nodes** -> **Add Azure VM**.</span></span>

<span data-ttu-id="e2b6e-138">A **válassza ki a virtuális gépek tooonboard**, válasszon ki egy vagy több Azure virtuális gépek tooonboard.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-138">Under **Select virtual machines tooonboard**, select one or more Azure virtual machines tooonboard.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

<span data-ttu-id="e2b6e-139">A **regisztrációs adatok konfigurálása**, adja meg a hello [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) a használati eset, és szükség esetén a csomópont konfigurációs tooassign toohello virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-139">Under **Configure registration data**, enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="e2b6e-140">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="e2b6e-140">Azure Resource Manager templates</span></span>

<span data-ttu-id="e2b6e-141">Azure virtuális gépeken is telepíthető és előkészítve tooAzure Automation DSC Azure Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-141">Azure virtual machines can be deployed and onboarded tooAzure Automation DSC via Azure Resource Manager templates.</span></span> <span data-ttu-id="e2b6e-142">Lásd: [konfigurálja a virtuális gépről a DSC-bővítményt és Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) egy példa sablon, hogy egy meglévő virtuális gép tooAzure Automation DSC onboards.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-142">See [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) for an example template that onboards an existing VM tooAzure Automation DSC.</span></span> <span data-ttu-id="e2b6e-143">toofind hello regisztrációs kulcs és a regisztrációs URL-cím szükséges bemeneti sablonban szereplő, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-143">toofind hello registration key and registration URL taken as input in this template, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="e2b6e-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2b6e-144">PowerShell</span></span>

<span data-ttu-id="e2b6e-145">Hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) parancsmag használt tooonboard virtuális gépek hello Azure-portálon keresztül is lehet.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-145">hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet can be used tooonboard virtual machines in hello Azure portal via PowerShell.</span></span>

## <a name="amazon-web-services-aws-virtual-machines"></a><span data-ttu-id="e2b6e-146">Amazon Web Services (AWS) virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-146">Amazon Web Services (AWS) virtual machines</span></span>

<span data-ttu-id="e2b6e-147">Azure Automation DSC hello AWS DSC eszközkészlet használatával által konfigurációkezelés könnyen előkészítésére Amazon Web Services virtuális gépek is.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-147">You can easily onboard Amazon Web Services virtual machines for configuration management by Azure Automation DSC using hello AWS DSC Toolkit.</span></span> <span data-ttu-id="e2b6e-148">Hello eszközkészlet kapcsolatos részletesebb [Itt](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span><span class="sxs-lookup"><span data-stu-id="e2b6e-148">You can learn more about hello toolkit [here](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span></span>

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a><span data-ttu-id="e2b6e-149">A Windows fizikai vagy virtuális gépek a helyszíni vagy felhőben működő Azure/AWS eltérő</span><span class="sxs-lookup"><span data-stu-id="e2b6e-149">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>

<span data-ttu-id="e2b6e-150">A helyi Windows-alapú gépek és a Windows-alapú gépek az-Azure felhők (például az Amazon Web Services) is lehet előkészítve tooAzure Automation DSC, mindaddig, amíg kimenő hozzáférést toohello rendelkeznek internet-néhány egyszerű lépésben:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-150">On-premises Windows machines and Windows machines in non-Azure clouds (such as Amazon Web Services) can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="e2b6e-151">Győződjön meg arról, hogy hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítése hello gépeken tooonboard tooAzure Automation DSC szeretné.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-151">Make sure hello latest version of [WMF 5](http://aka.ms/wmf5latest) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="e2b6e-152">Hello szakasz útmutatásait követve [ **generálása DSC metaconfigurations** ](#generating-dsc-metaconfigurations) toogenerate alatt hello tartalmazó mappa DSC metaconfigurations szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-152">Follow hello directions in section [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
3. <span data-ttu-id="e2b6e-153">Hello PowerShell DSC metakonfigurációját toohello gépek tooonboard kívánt távolról alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-153">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard.</span></span> <span data-ttu-id="e2b6e-154">**Ez a parancs fut hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepített**:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-154">**hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed**:</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. <span data-ttu-id="e2b6e-155">Hello PowerShell DSC metaconfigurations távolról nem alkalmazható, ha hello metaconfigurations mappába másolása, minden gép tooonboard 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-155">If you cannot apply hello PowerShell DSC metaconfigurations remotely, copy hello metaconfigurations folder from step 2 onto each machine tooonboard.</span></span> <span data-ttu-id="e2b6e-156">Majd meghívják a **Set-DscLocalConfigurationManager** helyileg az egyes gépek tooonboard.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-156">Then call **Set-DscLocalConfigurationManager** locally on each machine tooonboard.</span></span>
5. <span data-ttu-id="e2b6e-157">Hello Azure-portál vagy parancsmagok, ellenőrizze, hogy a regisztrált hello gépek tooonboard mostantól megjelennek a DSC-csomópontként az Azure Automation-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-157">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a><span data-ttu-id="e2b6e-158">Fizikai vagy virtuális Linux számítógépek, a helyszíni, az Azure-ban, vagy eltérő Azure felhőben</span><span class="sxs-lookup"><span data-stu-id="e2b6e-158">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="e2b6e-159">Helyszíni Linux számítógépek, az Azure Linux-gépek és a Linux-gépek-Azure felhőben is lehet előkészítve tooAzure Automation DSC, mindaddig, amíg azok rendelkeznek-e kimenő hozzáférést toohello internet-néhány egyszerű lépésben:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-159">On-premises Linux machines, Linux machines in Azure, and Linux machines in non-Azure clouds can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="e2b6e-160">Győződjön meg arról, hogy hello legújabb verziójának [PowerShell célállapot-konfiguráció Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) telepítve van hello gépeken tooonboard tooAzure Automation DSC szeretné.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-160">Make sure hello latest version of [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="e2b6e-161">Ha hello [PowerShell DSC helyi Configuration Manager alapértelmezett](https://msdn.microsoft.com/powershell/dsc/metaconfig4) felel meg a használati eset, és azt szeretné, hogy a számítógépek tooonboard, például, hogy azok **mindkét** lekérés a és tooAzure Automation DSC jelentést:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-161">If hello [PowerShell DSC Local Configuration Manager defaults](https://msdn.microsoft.com/powershell/dsc/metaconfig4) match your use case, and you want tooonboard machines such that they **both** pull from and report tooAzure Automation DSC:</span></span>

   + <span data-ttu-id="e2b6e-162">Minden egyes Linux számítógép tooonboard tooAzure Automation DSC Register.py tooonboard hello alapértelmezés szerint használt érték a PowerShell DSC helyi Configuration Manager használatával kell használni:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-162">On each Linux machine tooonboard tooAzure Automation DSC, use Register.py tooonboard using hello PowerShell DSC Local Configuration Manager defaults:</span></span>

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + <span data-ttu-id="e2b6e-163">toofind hello regisztrációs kulcs és a regisztrációs URL-címe az Automation-fiók, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-163">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>

     <span data-ttu-id="e2b6e-164">Ha hello PowerShell DSC helyi Configuration Manager alapértelmezett **tegye** **nem** felel meg a használati eset, vagy a kívánt tooonboard gépek úgy, hogy csak tooAzure Automation DSC jelentést, hanem kéri le a konfiguráció vagy a PowerShell-modulok, akkor kövesse a lépéseket 3-6.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-164">If hello PowerShell DSC Local Configuration Manager defaults **do** **not** match your use case, or you want tooonboard machines such that they only report tooAzure Automation DSC, but do not pull configuration or PowerShell modules from it,  follow steps 3 - 6.</span></span> <span data-ttu-id="e2b6e-165">Egyéb esetben folytassa a műveletet közvetlenül toostep 6.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-165">Otherwise, proceed directly toostep 6.</span></span>

3. <span data-ttu-id="e2b6e-166">Hello hello útmutatásait követve [ **generálása DSC metaconfigurations** ](#generating-dsc-metaconfigurations) szakasz alatt toogenerate szükséges hello DSC metaconfigurations tartalmazó mappához.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-166">Follow hello directions in hello [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) section below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
4. <span data-ttu-id="e2b6e-167">Hello PowerShell DSC metakonfigurációját toohello gépek tooonboard kívánt távolról vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-167">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard:</span></span>

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

<span data-ttu-id="e2b6e-168">Ez a parancs fut hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítve.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-168">hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>

1. <span data-ttu-id="e2b6e-169">Ha nem alkalmazhat hello PowerShell DSC metaconfigurations távolról, minden egyes Linux-gép tooonboard átmásolja a hello metakonfigurációját megfelelő toothat gép hello mappába települ hello Linux gépre 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-169">If you cannot apply hello PowerShell DSC metaconfigurations remotely, for each Linux machine tooonboard, copy hello metaconfiguration corresponding toothat machine from hello folder in step 5 onto hello Linux machine.</span></span> <span data-ttu-id="e2b6e-170">Majd meghívják a `SetDscLocalConfigurationManager.py` helyileg minden egyes Linux gépen szeretné tooonboard tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-170">Then call `SetDscLocalConfigurationManager.py` locally on each Linux machine you want tooonboard tooAzure Automation DSC:</span></span>

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. <span data-ttu-id="e2b6e-171">Hello Azure-portál vagy parancsmagok, ellenőrizze, hogy a regisztrált hello gépek tooonboard mostantól megjelennek a DSC-csomópontként az Azure Automation-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-171">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="generating-dsc-metaconfigurations"></a><span data-ttu-id="e2b6e-172">A DSC metaconfigurations létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b6e-172">Generating DSC metaconfigurations</span></span>

<span data-ttu-id="e2b6e-173">toogenerically bevezetésében bármely gépre tooAzure Automation DSC, egy [DSC metakonfigurációját](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) lehet, hogy a létre, amikor egy alkalmazott, hello DSC ügynök közli a hello gép toopull származó és/vagy tooAzure Automation DSC jelentést.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-173">toogenerically onboard any machine tooAzure Automation DSC, a [DSC metaconfiguration](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) can be generated that, when applied, tells hello DSC agent on hello machine toopull from and/or report tooAzure Automation DSC.</span></span> <span data-ttu-id="e2b6e-174">Az Azure Automation DSC DSC metaconfigurations a PowerShell DSC-konfiguráció, vagy hello Azure Automation PowerShell-parancsmagok használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-174">DSC metaconfigurations for Azure Automation DSC can be generated using either a PowerShell DSC configuration, or hello Azure Automation PowerShell cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="e2b6e-175">A DSC metaconfigurations hello szükséges titkok tooonboard egy gép tooan management Automation-fiók tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-175">DSC metaconfigurations contain hello secrets needed tooonboard a machine tooan Automation account for management.</span></span> <span data-ttu-id="e2b6e-176">Győződjön meg arról, hogy tooproperly védelme bármely DSC metaconfigurations hoz létre, vagy után törölje őket.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-176">Make sure tooproperly protect any DSC metaconfigurations you create, or delete them after use.</span></span>

### <a name="using-a-dsc-configuration"></a><span data-ttu-id="e2b6e-177">A DSC-konfiguráció használata</span><span class="sxs-lookup"><span data-stu-id="e2b6e-177">Using a DSC Configuration</span></span>

1. <span data-ttu-id="e2b6e-178">Nyissa meg rendszergazdaként a PowerShell ISE hello a helyi környezet valamelyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-178">Open hello PowerShell ISE as an administrator in a machine in your local environment.</span></span> <span data-ttu-id="e2b6e-179">hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítve.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-179">hello machine must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>
2. <span data-ttu-id="e2b6e-180">A következő parancsfájl helyi hello másolja.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-180">Copy hello following script locally.</span></span> <span data-ttu-id="e2b6e-181">A parancsfájl tartalmazza a PowerShell DSC-konfiguráció metaconfigurations, és a parancs tookick hello metakonfigurációját létrehozása ki lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-181">This script contains a PowerShell DSC configuration for creating metaconfigurations, and a command tookick off hello metaconfiguration creation.</span></span>

    ```powershell
    # hello DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create hello metaconfigurations
    # TODO: edit hello below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM tooonboard>', '<some other VM tooonboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set too$True toohave machines only report tooAA DSC but not pull from it
    }

    # Use PowerShell splatting toopass parameters toohello DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. <span data-ttu-id="e2b6e-182">Töltse ki hello regisztrációs kulcsot és az URL-címet az Automation-fiók, valamint a hello gépek tooonboard hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-182">Fill in hello registration key and URL for your Automation account, as well as hello names of hello machines tooonboard.</span></span> <span data-ttu-id="e2b6e-183">Más paraméterek opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-183">All other parameters are optional.</span></span> <span data-ttu-id="e2b6e-184">toofind hello regisztrációs kulcs és a regisztrációs URL-címe az Automation-fiók, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-184">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>
4. <span data-ttu-id="e2b6e-185">Ha szeretné, hogy hello gépek tooreport DSC állapot információk tooAzure Automation DSC, de nem lekérni a konfigurációját vagy a PowerShell-modulok, állítsa be a hello **ReportOnly** paraméter tootrue.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-185">If you want hello machines tooreport DSC status information tooAzure Automation DSC, but not pull configuration or PowerShell modules, set hello **ReportOnly** parameter tootrue.</span></span>
5. <span data-ttu-id="e2b6e-186">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-186">Run hello script.</span></span> <span data-ttu-id="e2b6e-187">Most már rendelkeznie kell egy nevű mappát **DscMetaConfigs** a munkakönyvtárba tartalmazó hello PowerShell DSC metaconfigurations a hello gépek tooonboard (rendszergazdaként):</span><span class="sxs-lookup"><span data-stu-id="e2b6e-187">You should now have a folder called **DscMetaConfigs** in your working directory, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a><span data-ttu-id="e2b6e-188">Hello Azure Automation parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="e2b6e-188">Using hello Azure Automation cmdlets</span></span>

<span data-ttu-id="e2b6e-189">Ha hello PowerShell DSC helyi Configuration Manager alapértelmezett felel meg a használati eset, és gépeihez tooonboard úgy, hogy azok mind a lekéréses, és jelentést tooAzure Automation DSC, hello Azure Automation parancsmagokkal hello DSC generálása egy egyszerűsített lehetőséget nyújt a a metaconfigurations szükséges:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-189">If hello PowerShell DSC Local Configuration Manager defaults match your use case, and you want tooonboard machines such that they both pull from and report tooAzure Automation DSC, hello Azure Automation cmdlets provide a simplified method of generating hello DSC metaconfigurations needed:</span></span>

1. <span data-ttu-id="e2b6e-190">Hello PowerShell-konzolt vagy a PowerShell ISE rendszergazdaként nyissa meg a helyi környezet valamelyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-190">Open hello PowerShell console or PowerShell ISE as an administrator in a machine in your local environment.</span></span>
2. <span data-ttu-id="e2b6e-191">Csatlakozás tooAzure erőforrás-kezelő használatával **Add-AzureRmAccount**</span><span class="sxs-lookup"><span data-stu-id="e2b6e-191">Connect tooAzure Resource Manager using **Add-AzureRmAccount**</span></span>
3. <span data-ttu-id="e2b6e-192">Hello PowerShell DSC metaconfigurations tölthető azt szeretné, hogy a hello Automation tooonboard hello gépek toowhich tooonboard csomópontok kívánt fiókot:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-192">Download hello PowerShell DSC metaconfigurations for hello machines you want tooonboard from hello Automation account toowhich you want tooonboard nodes:</span></span>

    ```powershell
    # Define hello parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # hello name of hello ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # hello name of hello Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # hello names of hello computers that hello meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting toopass parameters toohello Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. <span data-ttu-id="e2b6e-193">Most már rendelkeznie kell egy nevű mappát ***DscMetaConfigs***, tartalmazó hello PowerShell DSC metaconfigurations a hello gépek tooonboard (rendszergazdaként):</span><span class="sxs-lookup"><span data-stu-id="e2b6e-193">You should now have a folder called ***DscMetaConfigs***, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a><span data-ttu-id="e2b6e-194">Biztonságos regisztrációs</span><span class="sxs-lookup"><span data-stu-id="e2b6e-194">Secure registration</span></span>

<span data-ttu-id="e2b6e-195">Gépek biztonságosan előkészítésére tooan Azure Automation-fiók hello WMF 5 DSC regisztrációs protokoll, amely lehetővé teszi egy DSC csomópont tooauthenticate tooa PowerShell DSC V2 lekéréses vagy a Reporting server (az Azure Automation DSC) keresztül is.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-195">Machines can securely onboard tooan Azure Automation account through hello WMF 5 DSC registration protocol, which allows a DSC node tooauthenticate tooa PowerShell DSC V2 Pull or Reporting server (including Azure Automation DSC).</span></span> <span data-ttu-id="e2b6e-196">hello csomópont toohello kiszolgáló regisztrálása a **regisztrációs URL-cím**, hitelesítő használatával egy **regisztrációs kulcs**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-196">hello node registers toohello server at a **Registration URL**, authenticating using a **Registration key**.</span></span> <span data-ttu-id="e2b6e-197">Alatt a regisztrációt hello DSC-csomópont és DSC lekérési/jelentéskészítési kiszolgáló egyezteti a csomópont toouse hitelesítési toohello server utáni regisztrálási egyedi tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-197">During registration, hello DSC node and DSC Pull/Reporting server negotiate a unique certificate for this node toouse for authentication toohello server post-registration.</span></span> <span data-ttu-id="e2b6e-198">Ez a folyamat megakadályozza, hogy a előkészítve csomópontok megszemélyesít egy másikra, például ha egy csomópont biztonsága sérül, és rosszindulatúan viselkedik.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-198">This process prevents onboarded nodes from impersonating one another, such as if a node is compromised and behaving maliciously.</span></span> <span data-ttu-id="e2b6e-199">A regisztrációt követően hello regisztrációs kulcs nem használatos a hitelesítés újra, és hello csomópont törlődik.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-199">After registration, hello Registration key is not used for authentication again, and is deleted from hello node.</span></span>

<span data-ttu-id="e2b6e-200">A hello hello DSC regisztrációs protokoll szükséges hello adatokat kaphat **kulcsok kezelése** panel hello Azure betekintő portálon.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-200">You can get hello information required for hello DSC registration protocol from hello **Manage Keys** blade in hello Azure preview portal.</span></span> <span data-ttu-id="e2b6e-201">Nyissa meg ezen a panelen hello hello kulcs ikonra kattintva **Essentials** hello Automation-fiók panelen.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-201">Open this blade by clicking hello key icon on hello **Essentials** panel for hello Automation account.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* <span data-ttu-id="e2b6e-202">Regisztrációs URL-címe: hello URL-cím mező hello kulcsok kezelése panelen.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-202">Registration URL is hello URL field in hello Manage Keys blade.</span></span>
* <span data-ttu-id="e2b6e-203">Regisztrációs kulcs hello kulcsok kezelése panel hello elsődleges elérési kulcsot vagy másodlagos elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-203">Registration key is hello Primary Access Key or Secondary Access Key in hello Manage Keys blade.</span></span> <span data-ttu-id="e2b6e-204">Vagy a kulcs használható.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-204">Either key can be used.</span></span>

<span data-ttu-id="e2b6e-205">A nagyobb biztonság érdekében bármikor helyreállíthatja a hello elsődleges és másodlagos kulcsok az Automation-fiók (a hello **kulcsok kezelése** panel) tooprevent jövőbeli csomópont regisztrációk korábbi kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-205">For added security, hello primary and secondary access keys of an Automation account can be regenerated at any time (on hello **Manage Keys** blade) tooprevent future node registrations using previous keys.</span></span>

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a><span data-ttu-id="e2b6e-206">Hibaelhárítás az Azure virtuális gép bevezetése</span><span class="sxs-lookup"><span data-stu-id="e2b6e-206">Troubleshooting Azure virtual machine onboarding</span></span>

<span data-ttu-id="e2b6e-207">Az Azure Automation DSC könnyen előkészítésére Azure Windows virtuális gépek a konfigurációkezelésre lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-207">Azure Automation DSC lets you easily onboard Azure Windows VMs for configuration management.</span></span> <span data-ttu-id="e2b6e-208">A hello technikai hello Azure VM célállapot-konfiguráció bővítmény azonban a virtuális gép az Azure Automation DSC Szolgáltatásban használt tooregister hello.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-208">Under hello hood, hello Azure VM Desired State Configuration extension is used tooregister hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="e2b6e-209">Mivel hello Azure VM célállapot-konfiguráció bővítmény aszinkron módon futtatja, figyelemmel kíséri, a telepítés előrehaladását, és a végrehajtása hibaelhárítási fontos lehet.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-209">Since hello Azure VM Desired State Configuration extension runs asynchronously, tracking its progress and troubleshooting its execution may be important.</span></span>

> [!NOTE]
> <span data-ttu-id="e2b6e-210">A bevezetési egy Azure Windows virtuális gép tooAzure Automation DSC hello Azure VM célállapot-konfiguráció bővítmény használó eltarthat tooan órában hello csomópont tooshow mentése az Azure Automationben regisztrált bármely metódusát.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-210">Any method of onboarding an Azure Windows VM tooAzure Automation DSC that uses hello Azure VM Desired State Configuration extension could take up tooan hour for hello node tooshow up as registered in Azure Automation.</span></span> <span data-ttu-id="e2b6e-211">Ez egy, a virtuális gép hello által hello Azure VM DSC-bővítményt, amely szükséges tooonboard hello VM tooAzure Automation DSC toohello Windows Management Framework 5.0 telepítése miatt.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-211">This is due toohello installation of Windows Management Framework 5.0 on hello VM by hello Azure VM DSC extension, which is required tooonboard hello VM tooAzure Automation DSC.</span></span>

<span data-ttu-id="e2b6e-212">hello Azure VM célállapot-konfiguráció bővítménye hello Azure-portálon tootroubleshoot vagy nézet hello állapotának toohello keresse meg a virtuális gép előkészítve alatt, majd kattintson -> **összes beállítás** -> **bővítmények**   ->  **DSC**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-212">tootroubleshoot or view hello status of hello Azure VM Desired State Configuration extension, in hello Azure portal navigate toohello VM being onboarded, then click -> **All settings** -> **Extensions** -> **DSC**.</span></span> <span data-ttu-id="e2b6e-213">További részletekért kattintson **az állapot**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-213">For more details, you can click **View detailed status**.</span></span>

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a><span data-ttu-id="e2b6e-214">Tanúsítványok és ismételt</span><span class="sxs-lookup"><span data-stu-id="e2b6e-214">Certificate expiration and reregistration</span></span>

<span data-ttu-id="e2b6e-215">A gépek DSC-csomópontként az Azure Automation DSC a regisztrálás után számos okból miért esetleg tooreregister hello az adott csomópont jövőbeli:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-215">After registering a machine as a DSC node in Azure Automation DSC, there are a number of reasons why you may need tooreregister that node in hello future:</span></span>

* <span data-ttu-id="e2b6e-216">A regisztrálás után minden csomóponton automatikusan egyezteti egyedi tanúsítványt a hitelesítéshez, amely egy év után lejár.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-216">After registering, each node automatically negotiates a unique certificate for authentication that expires after one year.</span></span> <span data-ttu-id="e2b6e-217">Jelenleg hello PowerShell DSC regisztrációs protokoll nem automatikusan megújítani a tanúsítványokat, amikor azok hamarosan lejáró, ezért szüksége lesz egy év idő múlva tooreregister hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-217">Currently, hello PowerShell DSC registration protocol cannot automatically renew certificates when they are nearing expiration, so you need tooreregister hello nodes after a year's time.</span></span> <span data-ttu-id="e2b6e-218">Előtt újraregisztrálása, ellenőrizze, hogy mindegyik csomópontján fut a Windows Management Framework 5.0 RTM-re.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-218">Before reregistering, ensure that each node is running Windows Management Framework 5.0 RTM.</span></span> <span data-ttu-id="e2b6e-219">Ha a csomópont-hitelesítési tanúsítvány lejár, és hello csomópontot a rendszer nem újra regisztrálja, hello csomópont lesz az Azure Automation szolgáltatásban nem toocommunicate, és megjelöli "Unresponsive."</span><span class="sxs-lookup"><span data-stu-id="e2b6e-219">If a node's authentication certificate expires, and hello node is not reregistered, hello node will be unable toocommunicate with Azure Automation and will be marked 'Unresponsive.'</span></span> <span data-ttu-id="e2b6e-220">Ismételt 90 naponta végrehajtott vagy kisebb hello tanúsítvány lejárati ideje, vagy bármikor hello tanúsítvány lejárati ideje után egy új tanúsítványt generált és a használt eredményez.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-220">Reregistration performed 90 days or less from hello certificate expiration time, or at any point after hello certificate expiration time, will result in a new certificate being generated and used.</span></span>
* <span data-ttu-id="e2b6e-221">toochange bármely [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) , amely beállított hello csomópont, például a ConfigurationMode kezdeti regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-221">toochange any [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) that were set during initial registration of hello node, such as ConfigurationMode.</span></span> <span data-ttu-id="e2b6e-222">Jelenleg a DSC-ügynök értékeiről csak módosítható újraregisztrálni keresztül.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-222">Currently, these DSC agent values can only be changed through reregistration.</span></span> <span data-ttu-id="e2b6e-223">hello egyetlen kivétel ez alól a csomópont-konfiguráció hozzárendelése toohello csomópont hello – ez módosítható az Azure Automation DSC közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-223">hello one exception is hello Node Configuration assigned toohello node -- this can be changed in Azure Automation DSC directly.</span></span>

<span data-ttu-id="e2b6e-224">A jelen dokumentumban ismertetett regisztrált hello csomópont kezdetben hello bevezetési módszereket ugyanúgy hello ismételt hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-224">Reregistration can be performed in hello same way you registered hello node initially, using any of hello onboarding methods described in this document.</span></span> <span data-ttu-id="e2b6e-225">Egy Azure Automation DSC csomópontot toounregister előtt újraregisztrálása azt nem kell.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-225">You do not need toounregister a node from Azure Automation DSC before reregistering it.</span></span>

## <a name="related-articles"></a><span data-ttu-id="e2b6e-226">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="e2b6e-226">Related Articles</span></span>

* [<span data-ttu-id="e2b6e-227">Azure Automation DSC – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e2b6e-227">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="e2b6e-228">Azure Automation DSC-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="e2b6e-228">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="e2b6e-229">Azure Automation DSC – díjszabás</span><span class="sxs-lookup"><span data-stu-id="e2b6e-229">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)
