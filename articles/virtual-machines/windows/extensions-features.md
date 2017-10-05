---
title: "Virtuálisgép-bővítmények és a szolgáltatások a Windows Azure-ban |} Microsoft Docs"
description: "Ismerje meg, milyen bővítmények érhetők el, mi akkor adja meg, vagy javítania szerint csoportosítva, az Azure virtuális gépeken."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1ce0eebd2585c9457d7f922898d7f2fa3e7ffad7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a><span data-ttu-id="6ba7a-103">Virtuálisgép-bővítmények és a Windows szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6ba7a-103">Virtual machine extensions and features for Windows</span></span>

<span data-ttu-id="6ba7a-104">Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="6ba7a-105">Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény segítségével ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used to complete these tasks.</span></span> <span data-ttu-id="6ba7a-106">Az Azure Virtuálisgép-bővítmények futtathatja az Azure parancssori felület, PowerShell, az Azure Resource Manager-sablonok és az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-106">Azure VM extensions can be run by using the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span> <span data-ttu-id="6ba7a-107">Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-107">Extensions can be bundled with a new virtual machine deployment or run against any existing system.</span></span>

<span data-ttu-id="6ba7a-108">Ez a dokumentum áttekintést virtuálisgép-bővítmények, virtuálisgép-bővítmények és útmutatás észleléséhez, kezeléséhez, és távolítsa el a virtuálisgép-bővítmények használatának előfeltételei.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-108">This document provides an overview of virtual machine extensions, prerequisites for using virtual machine extensions, and guidance on how to detect, manage, and remove virtual machine extensions.</span></span> <span data-ttu-id="6ba7a-109">A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="6ba7a-110">Bővítmény felhasználóspecifikus adatainak található minden a dokumentumban a egyéni bővítmény egyedi.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-110">Extension-specific details can be found in each document specific to the individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="6ba7a-111">Használati esetek és minták</span><span class="sxs-lookup"><span data-stu-id="6ba7a-111">Use cases and samples</span></span>

<span data-ttu-id="6ba7a-112">Nincsenek elérhető számos különböző Azure Virtuálisgép-bővítmények, az adott használati eset.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-112">There are many different Azure VM extensions available, each with a specific use case.</span></span> <span data-ttu-id="6ba7a-113">Néhány példa használati esetek a következők:</span><span class="sxs-lookup"><span data-stu-id="6ba7a-113">Some example use cases are:</span></span>

- <span data-ttu-id="6ba7a-114">A virtuális gépek a DSC-bővítményt a Windows PowerShell kívánt állapot konfiguráció alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-114">Apply PowerShell Desired State configurations to a virtual machine by using the DSC extension for Windows.</span></span> <span data-ttu-id="6ba7a-115">További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](extensions-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-115">For more information, see [Azure Desired State configuration extension](extensions-dsc-overview.md).</span></span>
- <span data-ttu-id="6ba7a-116">Konfigurálja a virtuális gép figyelése a Microsoft Monitoring Agent Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-116">Configure virtual machine monitoring by using the Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="6ba7a-117">További információkért lásd: [Log Analyticshez való csatlakozás Azure virtuális gépek](../../log-analytics/log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-117">For more information, see [Connect Azure virtual machines to Log Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="6ba7a-118">Az Azure infrastruktúra Datadog kiterjesztésű figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-118">Configure monitoring of your Azure infrastructure with the Datadog extension.</span></span> <span data-ttu-id="6ba7a-119">További információkért lásd: a [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-119">For more information, see the [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="6ba7a-120">Konfiguráljon egy Azure virtuális gép Chef.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-120">Configure an Azure virtual machine by using Chef.</span></span> <span data-ttu-id="6ba7a-121">További információkért lásd: [automatizálása Azure virtuális gépek telepítése a Chef](chef-automation.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-121">For more information, see [Automating Azure virtual machine deployment with Chef](chef-automation.md).</span></span>

<span data-ttu-id="6ba7a-122">Folyamatokra vonatkozó bővítmények mellett egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és a Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-122">In addition to process-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="6ba7a-123">Az egyéni parancsprogramok futtatására szolgáló bővítmény Windows lehetővé teszi, hogy minden virtuális gépen futtatandó PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-123">The Custom Script extension for Windows allows any PowerShell script to be run on a virtual machine.</span></span> <span data-ttu-id="6ba7a-124">Ez akkor hasznos, amikor az identitásfelügyelet Azure-környezetekhez, hogy milyen natív Azure tooling biztosíthat túl beállításokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-124">This is useful when you're designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="6ba7a-125">További információkért lásd: [Windows virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-125">For more information, see [Windows VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6ba7a-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6ba7a-126">Prerequisites</span></span>

<span data-ttu-id="6ba7a-127">Minden egyes virtuálisgép-bővítmény is rendelkeznek a saját előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-127">Each virtual machine extension may have its own set of prerequisites.</span></span> <span data-ttu-id="6ba7a-128">Például a Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-128">For instance, the Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="6ba7a-129">Egyes bővítmények követelményeinek részletes leírást talál a bővítmény-specifikus dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-129">Requirements of individual extensions are detailed in the extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="6ba7a-130">Azure virtuálisgép-ügynök</span><span class="sxs-lookup"><span data-stu-id="6ba7a-130">Azure VM agent</span></span>
<span data-ttu-id="6ba7a-131">Az Azure Virtuálisgép-ügynök kezeli az Azure virtuális gép és az Azure fabric controller közötti interakció.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-131">The Azure VM agent manages interaction between an Azure virtual machine and the Azure fabric controller.</span></span> <span data-ttu-id="6ba7a-132">A Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-132">The VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="6ba7a-133">Az Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép, és támogatott operációs rendszerekre telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-133">The Azure VM agent is preinstalled on Azure Marketplace images and can be installed on supported operating systems.</span></span>

<span data-ttu-id="6ba7a-134">Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](agent-user-guide.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="6ba7a-135">Virtuálisgép-bővítmények felderítése</span><span class="sxs-lookup"><span data-stu-id="6ba7a-135">Discover VM extensions</span></span>
<span data-ttu-id="6ba7a-136">Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="6ba7a-137">Teljes listájának megtekintéséhez futtassa a következő parancsot az Azure Resource Manager PowerShell-modul.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-137">To see a complete list, run the following command with the Azure Resource Manager PowerShell module.</span></span> <span data-ttu-id="6ba7a-138">Ügyeljen arra, hogy adja meg a kívánt helyre, ha futtatja a parancsot.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-138">Make sure to specify the desired location when you're running this command.</span></span>

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a><span data-ttu-id="6ba7a-139">Futtassa a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="6ba7a-139">Run VM extensions</span></span>

<span data-ttu-id="6ba7a-140">Azure virtuálisgép-bővítmények futtathatja a meglévő virtuális gépeken, amelyek akkor hasznos, ha a konfigurációs módosításokat, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen kell.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-140">Azure virtual machine extensions can be run on existing virtual machines, which is useful when you need to make configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="6ba7a-141">Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-141">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="6ba7a-142">Bővítmények a Resource Manager-sablonok segítségével engedélyezheti az Azure virtuális gépek telepítése és a telepítés utáni beavatkozás nélkül konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-142">By using extensions with Resource Manager templates, you can enable Azure virtual machines to be deployed and configured without the need for post-deployment intervention.</span></span>

<span data-ttu-id="6ba7a-143">Az alábbi eljárások segítségével bővítmény futtathat egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-143">The following methods can be used to run an extension against an existing virtual machine.</span></span>

### <a name="powershell"></a><span data-ttu-id="6ba7a-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ba7a-144">PowerShell</span></span>

<span data-ttu-id="6ba7a-145">Több PowerShell-parancsok futtatása az egyes bővítmények léteznek.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-145">Several PowerShell commands exist for running individual extensions.</span></span> <span data-ttu-id="6ba7a-146">Listájának megtekintéséhez futtassa a következő PowerShell-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-146">To see a list, run the following PowerShell commands.</span></span>

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

<span data-ttu-id="6ba7a-147">Ez biztosítja, hogy a kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="6ba7a-147">This provides output similar to the following:</span></span>

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

<span data-ttu-id="6ba7a-148">A következő példa az egyéni parancsprogramok futtatására szolgáló bővítmény parancsfájl letölthető a GitHub-tárházban települ a cél virtuális gépre, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-148">The following example uses the Custom Script extension to download a script from a GitHub repository onto the target virtual machine and then run the script.</span></span> <span data-ttu-id="6ba7a-149">Az egyéni parancsprogramok futtatására szolgáló bővítmény további információkért lásd: [egyéni parancsfájl-bővítmény áttekintése](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-149">For more information on the Custom Script extension, see [Custom Script extension overview](extensions-customscript.md).</span></span>

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

<span data-ttu-id="6ba7a-150">Ebben a példában a virtuális gép hozzáférési bővítmény segítségével Windows rendszerű virtuális gép rendszergazdai jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-150">In this example, the VM Access extension is used to reset the administrative password of a Windows virtual machine.</span></span> <span data-ttu-id="6ba7a-151">A virtuális gép hozzáférési bővítmény további információkért lásd: [alaphelyzetbe állítja a távoli asztal szolgáltatás egy Windows virtuális gépre](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-151">For more information on the VM Access extension, see [Reset Remote Desktop service in a Windows VM](reset-rdp.md).</span></span>

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

<span data-ttu-id="6ba7a-152">A `Set-AzureRmVMExtension` parancs segítségével indítsa el a Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-152">The `Set-AzureRmVMExtension` command can be used to start any VM extension.</span></span> <span data-ttu-id="6ba7a-153">További információkért lásd: a [Set-AzureRmVMExtension hivatkozás](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-153">For more information, see the [Set-AzureRmVMExtension reference](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span></span>


### <a name="azure-portal"></a><span data-ttu-id="6ba7a-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6ba7a-154">Azure portal</span></span>

<span data-ttu-id="6ba7a-155">A Virtuálisgép-bővítmény egy meglévő virtuális gépet az Azure portálon keresztül is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-155">A VM extension can be applied to an existing virtual machine through the Azure portal.</span></span> <span data-ttu-id="6ba7a-156">Ehhez az szükséges, válassza ki a virtuális gép szeretne használni, válasszon **bővítmények**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-156">To do so, select the virtual machine you want to use, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="6ba7a-157">Ez a rendelkezésre álló bővítmények listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-157">This provides a list of available extensions.</span></span> <span data-ttu-id="6ba7a-158">Jelölje ki a kívánt, és kövesse a varázsló lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-158">Select the one you want and follow the steps in the wizard.</span></span>

<span data-ttu-id="6ba7a-159">A következő kép bemutatja az Azure-portálról a Microsoft Antimalware-bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-159">The following image shows the installation of the Microsoft Antimalware extension from the Azure portal.</span></span>

![Kártevőirtó-kiterjesztés telepítése](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="6ba7a-161">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="6ba7a-161">Azure Resource Manager templates</span></span>

<span data-ttu-id="6ba7a-162">Virtuálisgép-bővítmények felvehető az Azure Resource Manager-sablon és a sablon a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-162">VM extensions can be added to an Azure Resource Manager template and executed with the deployment of the template.</span></span> <span data-ttu-id="6ba7a-163">Központi telepítése és egy sablon létrehozása esetén hasznos teljesen konfigurált Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-163">Deploying extensions with a template is useful for creating fully configured Azure deployments.</span></span> <span data-ttu-id="6ba7a-164">Például a következő JSON származik a Resource Manager-sablon, amely telepíti az elosztott terhelésű virtuális gépek és Azure SQL-adatbázis, és ezután telepíti a .NET Core alkalmazások minden egyes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-164">For example, the following JSON is taken from a Resource Manager template that deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="6ba7a-165">A Virtuálisgép-bővítmény gondoskodik a szoftver telepítését.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-165">The VM extension takes care of the software installation.</span></span>

<span data-ttu-id="6ba7a-166">További információkért lásd: a [teljes Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-166">For more information, see the [full Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="6ba7a-167">További információkért lásd: [szerzői Azure Resource Manager-sablon is van Windows Virtuálisgép-bővítmények](template-description.md#extensions).</span><span class="sxs-lookup"><span data-stu-id="6ba7a-167">For more information, see [Authoring Azure Resource Manager templates with Windows VM extensions](template-description.md#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="6ba7a-168">Virtuálisgép-bővítmény adatvédelem</span><span class="sxs-lookup"><span data-stu-id="6ba7a-168">Secure VM extension data</span></span>

<span data-ttu-id="6ba7a-169">Jelenik meg a Virtuálisgép-bővítmény, akkor lehet szükség, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok bizalmas adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-169">When you're running a VM extension, it may be necessary to include sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="6ba7a-170">Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti a cél virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-170">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside the target virtual machine.</span></span> <span data-ttu-id="6ba7a-171">Minden bővítmény rendelkezik egy adott védett konfigurációs sémában, amely a bővítmény-specifikus dokumentációjának nyilatkozatát.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-171">Each extension has a specific protected configuration schema that will be detailed in extension-specific documentation.</span></span>

<span data-ttu-id="6ba7a-172">A következő példa bemutatja az egyéni parancsprogramok futtatására szolgáló bővítmény példányának Windows.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-172">The following example shows an instance of the Custom Script extension for Windows.</span></span> <span data-ttu-id="6ba7a-173">Figyelje meg, hogy a parancs végrehajtásának tartalmazza-e a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-173">Notice that the command to execute includes a set of credentials.</span></span> <span data-ttu-id="6ba7a-174">Ebben a példában a parancs végrehajtása nem lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-174">In this example, the command to execute will not be encrypted.</span></span>


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="6ba7a-175">A végrehajtási karakterlánc biztonságos át a **végrehajtandó parancs** tulajdonságot a **védett** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-175">Secure the execution string by moving the **command to execute** property to the **protected** configuration.</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="6ba7a-176">Virtuálisgép-bővítmények hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6ba7a-176">Troubleshoot VM extensions</span></span>

<span data-ttu-id="6ba7a-177">Előfordulhat, hogy az egyes Virtuálisgép-bővítmény adott hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-177">Each VM extension may have specific troubleshooting steps.</span></span> <span data-ttu-id="6ba7a-178">Például az egyéni parancsprogramok futtatására szolgáló bővítmény használatakor parancsfájl végrehajtásának részletes adatai található helyileg a virtuális gépen, amelyen a bővítmény futott.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-178">For instance, when you're using the Custom Script extension, script execution details can be found locally on the virtual machine on which the extension was run.</span></span> <span data-ttu-id="6ba7a-179">Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-179">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="6ba7a-180">A következő hibaelhárítási lépéseket az összes virtuálisgép-bővítmény vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-180">The following troubleshooting steps apply to all virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="6ba7a-181">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="6ba7a-181">View extension status</span></span>

<span data-ttu-id="6ba7a-182">A virtuálisgép-bővítmény futtatása a virtuális gépek ellen, után a következő PowerShell-parancs segítségével térjen vissza a bővítmény állapotát.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-182">After a virtual machine extension has been run against a virtual machine, use the following PowerShell command to return extension status.</span></span> <span data-ttu-id="6ba7a-183">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-183">Replace example parameter names with your own values.</span></span> <span data-ttu-id="6ba7a-184">A `Name` paraméter szükséges a bővítmény a végrehajtás során a megadott név.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-184">The `Name` parameter takes the name given to the extension at execution time.</span></span>

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="6ba7a-185">A kimenet a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="6ba7a-185">The output looks like the following:</span></span>

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

<span data-ttu-id="6ba7a-186">Bővítmény végrehajtási állapotát az Azure portálon is található.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-186">Extension execution status can also be found in the Azure portal.</span></span> <span data-ttu-id="6ba7a-187">Egy bővítmény állapotának megtekintéséhez válassza ki a virtuális gépet, kattintson a **bővítmények**, és jelölje ki a kívánt bővítményt.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-187">To view the status of an extension, select the virtual machine, choose **Extensions**, and select the desired extension.</span></span>

### <a name="rerun-vm-extensions"></a><span data-ttu-id="6ba7a-188">Futtassa újra a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="6ba7a-188">Rerun VM extensions</span></span>

<span data-ttu-id="6ba7a-189">Előfordulhatnak olyan esetekben, ahol egy virtuálisgép-bővítmény futtatható kell.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-189">There may be cases in which a virtual machine extension needs to be rerun.</span></span> <span data-ttu-id="6ba7a-190">Ehhez a bővítmény eltávolítása, és majd futtassa újból a kiterjesztés egy végrehajtási módszer az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-190">You can do this by removing the extension and then rerunning the extension with an execution method of your choice.</span></span> <span data-ttu-id="6ba7a-191">Egy bővítmény eltávolításához futtassa a következő parancsot az Azure PowerShell modullal.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-191">To remove an extension, run the following command with the Azure PowerShell module.</span></span> <span data-ttu-id="6ba7a-192">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-192">Replace example parameter names with your own values.</span></span>

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="6ba7a-193">Egy bővítmény is távolítható el az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-193">An extension can also be removed using the Azure portal.</span></span> <span data-ttu-id="6ba7a-194">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6ba7a-194">To do so:</span></span>

1. <span data-ttu-id="6ba7a-195">Válasszon ki egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-195">Select a virtual machine.</span></span>
2. <span data-ttu-id="6ba7a-196">Válassza ki **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-196">Select **Extensions**.</span></span>
3. <span data-ttu-id="6ba7a-197">Válassza ki a kívánt bővítményt.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-197">Choose the desired extension.</span></span>
4. <span data-ttu-id="6ba7a-198">Válassza ki **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="6ba7a-198">Select **Uninstall**.</span></span>

## <a name="common-vm-extensions-reference"></a><span data-ttu-id="6ba7a-199">Közös Virtuálisgép-bővítmények hivatkozása</span><span class="sxs-lookup"><span data-stu-id="6ba7a-199">Common VM extensions reference</span></span>
| <span data-ttu-id="6ba7a-200">Bővítmény neve</span><span class="sxs-lookup"><span data-stu-id="6ba7a-200">Extension name</span></span> | <span data-ttu-id="6ba7a-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ba7a-201">Description</span></span> | <span data-ttu-id="6ba7a-202">További információ</span><span class="sxs-lookup"><span data-stu-id="6ba7a-202">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ba7a-203">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-203">Custom Script Extension for Windows</span></span> |<span data-ttu-id="6ba7a-204">Parancsfájlok futtatásához egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="6ba7a-204">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="6ba7a-205">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-205">Custom Script Extension for Windows</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="6ba7a-206">A Windows DSC-bővítményt</span><span class="sxs-lookup"><span data-stu-id="6ba7a-206">DSC Extension for Windows</span></span> |<span data-ttu-id="6ba7a-207">PowerShell DSC (célállapot-konfiguráció) bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-207">PowerShell DSC (Desired State Configuration) Extension</span></span> |[<span data-ttu-id="6ba7a-208">A Windows DSC-bővítményt</span><span class="sxs-lookup"><span data-stu-id="6ba7a-208">DSC Extension for Windows</span></span>](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="6ba7a-209">Azure Diagnostics bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-209">Azure Diagnostics Extension</span></span> |<span data-ttu-id="6ba7a-210">Az Azure Diagnostics kezelése</span><span class="sxs-lookup"><span data-stu-id="6ba7a-210">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="6ba7a-211">Azure Diagnostics bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-211">Azure Diagnostics Extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="6ba7a-212">Azure virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-212">Azure VM Access Extension</span></span> |<span data-ttu-id="6ba7a-213">Felhasználók és a hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="6ba7a-213">Manage users and credentials</span></span> |[<span data-ttu-id="6ba7a-214">Linux virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6ba7a-214">VM Access Extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
