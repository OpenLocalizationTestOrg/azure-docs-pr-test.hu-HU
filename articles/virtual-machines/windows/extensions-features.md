---
title: "aaaVirtual gép bővítményeket és szolgáltatásokat a Windows Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 61ccfd696b38e9be1026d836d5796c2346fd650f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a><span data-ttu-id="93014-103">Virtuálisgép-bővítmények és a Windows szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="93014-103">Virtual machine extensions and features for Windows</span></span>

<span data-ttu-id="93014-104">Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="93014-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="93014-105">Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény is használt toocomplete kell ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="93014-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="93014-106">Az Azure Virtuálisgép-bővítmények hello Azure CLI PowerShell, Azure Resource Manager sablonok segítségével futtathatja, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="93014-106">Azure VM extensions can be run by using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="93014-107">Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.</span><span class="sxs-lookup"><span data-stu-id="93014-107">Extensions can be bundled with a new virtual machine deployment or run against any existing system.</span></span>

<span data-ttu-id="93014-108">Ez a dokumentum áttekintést virtuálisgép-bővítmények, hogyan toodetect, kezeléséhez, és távolítsa el a virtuálisgép-bővítmények a virtuálisgép-bővítmények és útmutatás használatának előfeltételei.</span><span class="sxs-lookup"><span data-stu-id="93014-108">This document provides an overview of virtual machine extensions, prerequisites for using virtual machine extensions, and guidance on how toodetect, manage, and remove virtual machine extensions.</span></span> <span data-ttu-id="93014-109">A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="93014-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="93014-110">Minden egyes dokumentum adott toohello egyedi bővítmény részletei bővítmény-specifikus megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="93014-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="93014-111">Használati esetek és minták</span><span class="sxs-lookup"><span data-stu-id="93014-111">Use cases and samples</span></span>

<span data-ttu-id="93014-112">Nincsenek elérhető számos különböző Azure Virtuálisgép-bővítmények, az adott használati eset.</span><span class="sxs-lookup"><span data-stu-id="93014-112">There are many different Azure VM extensions available, each with a specific use case.</span></span> <span data-ttu-id="93014-113">Néhány példa használati esetek a következők:</span><span class="sxs-lookup"><span data-stu-id="93014-113">Some example use cases are:</span></span>

- <span data-ttu-id="93014-114">Alkalmazza a PowerShell kívánt állapot konfigurációk tooa virtuális gép Windows hello DSC-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="93014-114">Apply PowerShell Desired State configurations tooa virtual machine by using hello DSC extension for Windows.</span></span> <span data-ttu-id="93014-115">További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](extensions-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="93014-115">For more information, see [Azure Desired State configuration extension](extensions-dsc-overview.md).</span></span>
- <span data-ttu-id="93014-116">Konfigurálja a virtuális gép figyelése hello Microsoft Monitoring Agent Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="93014-116">Configure virtual machine monitoring by using hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="93014-117">További információkért lásd: [csatlakozás Azure virtuális gépek tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="93014-117">For more information, see [Connect Azure virtual machines tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="93014-118">Az Azure infrastruktúra hello Datadog bővítmény a figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="93014-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="93014-119">További információkért lásd: hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="93014-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="93014-120">Konfiguráljon egy Azure virtuális gép Chef.</span><span class="sxs-lookup"><span data-stu-id="93014-120">Configure an Azure virtual machine by using Chef.</span></span> <span data-ttu-id="93014-121">További információkért lásd: [automatizálása Azure virtuális gépek telepítése a Chef](chef-automation.md).</span><span class="sxs-lookup"><span data-stu-id="93014-121">For more information, see [Automating Azure virtual machine deployment with Chef](chef-automation.md).</span></span>

<span data-ttu-id="93014-122">Továbbá tooprocess-specifikus bővítmények, egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="93014-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="93014-123">Egyéni parancsprogramok futtatására szolgáló bővítmény Windows hello lehetővé teszi, hogy a PowerShell parancsfájl toobe futtasson egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="93014-123">hello Custom Script extension for Windows allows any PowerShell script toobe run on a virtual machine.</span></span> <span data-ttu-id="93014-124">Ez akkor hasznos, amikor az identitásfelügyelet Azure-környezetekhez, hogy milyen natív Azure tooling biztosíthat túl beállításokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="93014-124">This is useful when you're designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="93014-125">További információkért lásd: [Windows virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="93014-125">For more information, see [Windows VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="93014-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="93014-126">Prerequisites</span></span>

<span data-ttu-id="93014-127">Minden egyes virtuálisgép-bővítmény is rendelkeznek a saját előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="93014-127">Each virtual machine extension may have its own set of prerequisites.</span></span> <span data-ttu-id="93014-128">Például hello Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="93014-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="93014-129">Egyes bővítmények követelményeinek részletes leírást talál hello bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="93014-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="93014-130">Azure virtuálisgép-ügynök</span><span class="sxs-lookup"><span data-stu-id="93014-130">Azure VM agent</span></span>
<span data-ttu-id="93014-131">hello Azure Virtuálisgép-ügynök kezelése az Azure virtuális gép és a hello Azure fabric controller közötti interakció.</span><span class="sxs-lookup"><span data-stu-id="93014-131">hello Azure VM agent manages interaction between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="93014-132">hello Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős.</span><span class="sxs-lookup"><span data-stu-id="93014-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="93014-133">hello Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép és támogatott operációs rendszerekre telepíthető.</span><span class="sxs-lookup"><span data-stu-id="93014-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed on supported operating systems.</span></span>

<span data-ttu-id="93014-134">Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="93014-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](agent-user-guide.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="93014-135">Virtuálisgép-bővítmények felderítése</span><span class="sxs-lookup"><span data-stu-id="93014-135">Discover VM extensions</span></span>
<span data-ttu-id="93014-136">Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="93014-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="93014-137">toosee teljes listáját, futtassa a következő parancs hello Azure Resource Manager PowerShell modullal hello.</span><span class="sxs-lookup"><span data-stu-id="93014-137">toosee a complete list, run hello following command with hello Azure Resource Manager PowerShell module.</span></span> <span data-ttu-id="93014-138">Győződjön meg arról, hogy toospecify hello szükséges hely jelenik meg a parancsot.</span><span class="sxs-lookup"><span data-stu-id="93014-138">Make sure toospecify hello desired location when you're running this command.</span></span>

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a><span data-ttu-id="93014-139">Futtassa a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="93014-139">Run VM extensions</span></span>

<span data-ttu-id="93014-140">Azure virtuálisgép-bővítmények futtathatja a meglévő virtuális gépeken, amelyek akkor hasznos, ha a szükséges konfigurációs módosítások toomake, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="93014-140">Azure virtual machine extensions can be run on existing virtual machines, which is useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="93014-141">Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="93014-141">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="93014-142">Bővítmények a Resource Manager-sablonok segítségével engedélyezheti az Azure virtuális gépek toobe üzembe helyezését és konfigurálását hello telepítés utáni beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="93014-142">By using extensions with Resource Manager templates, you can enable Azure virtual machines toobe deployed and configured without hello need for post-deployment intervention.</span></span>

<span data-ttu-id="93014-143">a következő módszerek hello használt toorun egy bővítmény egy meglévő virtuális gépek ellen lehet.</span><span class="sxs-lookup"><span data-stu-id="93014-143">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="powershell"></a><span data-ttu-id="93014-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="93014-144">PowerShell</span></span>

<span data-ttu-id="93014-145">Több PowerShell-parancsok futtatása az egyes bővítmények léteznek.</span><span class="sxs-lookup"><span data-stu-id="93014-145">Several PowerShell commands exist for running individual extensions.</span></span> <span data-ttu-id="93014-146">toosee a listáját, és futtassa a következő PowerShell-parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="93014-146">toosee a list, run hello following PowerShell commands.</span></span>

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

<span data-ttu-id="93014-147">Ez a kimeneti hasonló toohello következőket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="93014-147">This provides output similar toohello following:</span></span>

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

<span data-ttu-id="93014-148">hello alábbi példa hello Custom Script bővítmény toodownload parancsfájlt használ a egy GitHub-tárházban települ hello cél virtuális gépre, majd futtassa hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="93014-148">hello following example uses hello Custom Script extension toodownload a script from a GitHub repository onto hello target virtual machine and then run hello script.</span></span> <span data-ttu-id="93014-149">Az egyéni parancsprogramok futtatására szolgáló bővítmény hello további információkért lásd: [egyéni parancsfájl-bővítmény áttekintése](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="93014-149">For more information on hello Custom Script extension, see [Custom Script extension overview](extensions-customscript.md).</span></span>

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

<span data-ttu-id="93014-150">Ebben a példában a hello VM hozzáférési bővítmény használt tooreset hello rendszergazdai jelszót egy Windows virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="93014-150">In this example, hello VM Access extension is used tooreset hello administrative password of a Windows virtual machine.</span></span> <span data-ttu-id="93014-151">A virtuális gép hozzáférési bővítmény hello további információkért lásd: [alaphelyzetbe állítja a távoli asztal szolgáltatás egy Windows virtuális gépre](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="93014-151">For more information on hello VM Access extension, see [Reset Remote Desktop service in a Windows VM](reset-rdp.md).</span></span>

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

<span data-ttu-id="93014-152">Hello `Set-AzureRmVMExtension` parancs lehet használt toostart Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="93014-152">hello `Set-AzureRmVMExtension` command can be used toostart any VM extension.</span></span> <span data-ttu-id="93014-153">További információkért lásd: hello [Set-AzureRmVMExtension hivatkozás](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span><span class="sxs-lookup"><span data-stu-id="93014-153">For more information, see hello [Set-AzureRmVMExtension reference](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span></span>


### <a name="azure-portal"></a><span data-ttu-id="93014-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="93014-154">Azure portal</span></span>

<span data-ttu-id="93014-155">A Virtuálisgép-bővítmény alkalmazott tooan meglévő virtuális gép hello Azure-portálon keresztül lehet.</span><span class="sxs-lookup"><span data-stu-id="93014-155">A VM extension can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="93014-156">toodo úgy, válassza ki a virtuális gép hello toouse szeretne, válassza a **bővítmények**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="93014-156">toodo so, select hello virtual machine you want toouse, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="93014-157">Ez a rendelkezésre álló bővítmények listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="93014-157">This provides a list of available extensions.</span></span> <span data-ttu-id="93014-158">Válassza ki a hello egy szeretné, hajtsa végre a hello hello varázsló lépéseit.</span><span class="sxs-lookup"><span data-stu-id="93014-158">Select hello one you want and follow hello steps in hello wizard.</span></span>

<span data-ttu-id="93014-159">hello következő kép bemutatja hello hello hello Azure-portál a Microsoft Antimalware-bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="93014-159">hello following image shows hello installation of hello Microsoft Antimalware extension from hello Azure portal.</span></span>

![Kártevőirtó-kiterjesztés telepítése](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="93014-161">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="93014-161">Azure Resource Manager templates</span></span>

<span data-ttu-id="93014-162">Virtuálisgép-bővítmények hozzáadott tooan Azure Resource Manager-sablon és hello sablon hello telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="93014-162">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="93014-163">Központi telepítése és egy sablon létrehozása esetén hasznos teljesen konfigurált Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="93014-163">Deploying extensions with a template is useful for creating fully configured Azure deployments.</span></span> <span data-ttu-id="93014-164">Például hello következő JSON származik a Resource Manager-sablon, amely telepíti az elosztott terhelésű virtuális gépek és az Azure SQL-adatbázis, és ezután telepíti a .NET Core alkalmazások minden egyes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="93014-164">For example, hello following JSON is taken from a Resource Manager template that deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="93014-165">Virtuálisgép-bővítmény hello hello Szoftvertelepítés gondoskodik.</span><span class="sxs-lookup"><span data-stu-id="93014-165">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="93014-166">További információkért lásd: hello [teljes Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="93014-166">For more information, see hello [full Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

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

<span data-ttu-id="93014-167">További információkért lásd: [szerzői Azure Resource Manager-sablon is van Windows Virtuálisgép-bővítmények](template-description.md#extensions).</span><span class="sxs-lookup"><span data-stu-id="93014-167">For more information, see [Authoring Azure Resource Manager templates with Windows VM extensions](template-description.md#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="93014-168">Virtuálisgép-bővítmény adatvédelem</span><span class="sxs-lookup"><span data-stu-id="93014-168">Secure VM extension data</span></span>

<span data-ttu-id="93014-169">Ha egy Virtuálisgép-bővítmény, szükséges tooinclude bizalmas információk, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok lehet.</span><span class="sxs-lookup"><span data-stu-id="93014-169">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="93014-170">Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti hello cél virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="93014-170">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="93014-171">Minden bővítmény rendelkezik egy adott védett konfigurációs sémában, amely a bővítmény-specifikus dokumentációjának nyilatkozatát.</span><span class="sxs-lookup"><span data-stu-id="93014-171">Each extension has a specific protected configuration schema that will be detailed in extension-specific documentation.</span></span>

<span data-ttu-id="93014-172">a következő példa hello Windows hello egyéni parancsprogramok futtatására szolgáló bővítmény példányának jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="93014-172">hello following example shows an instance of hello Custom Script extension for Windows.</span></span> <span data-ttu-id="93014-173">Figyelje meg, hogy hello parancs tooexecute hitelesítő adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="93014-173">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="93014-174">Ebben a példában hello parancs tooexecute nem lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="93014-174">In this example, hello command tooexecute will not be encrypted.</span></span>


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

<span data-ttu-id="93014-175">Biztonságos hello végrehajtási karakterlánc hello mozgatásával **parancs tooexecute** tulajdonság toohello **védett** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="93014-175">Secure hello execution string by moving hello **command tooexecute** property toohello **protected** configuration.</span></span>

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

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="93014-176">Virtuálisgép-bővítmények hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="93014-176">Troubleshoot VM extensions</span></span>

<span data-ttu-id="93014-177">Előfordulhat, hogy az egyes Virtuálisgép-bővítmény adott hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="93014-177">Each VM extension may have specific troubleshooting steps.</span></span> <span data-ttu-id="93014-178">Például ha egyéni parancsprogramok futtatására szolgáló bővítmény hello használata esetén parancsfájl végrehajtásának részletes adatai található helyileg hello bővítmény futtatása hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="93014-178">For instance, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="93014-179">Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="93014-179">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="93014-180">a következő hibaelhárítási lépéseket hello tooall virtuálisgép-bővítmények alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="93014-180">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="93014-181">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="93014-181">View extension status</span></span>

<span data-ttu-id="93014-182">Egy virtuálisgép-bővítmény futtatása a virtuális gépek ellen, után használja a PowerShell parancs tooreturn bővítmény állapota a következő hello.</span><span class="sxs-lookup"><span data-stu-id="93014-182">After a virtual machine extension has been run against a virtual machine, use hello following PowerShell command tooreturn extension status.</span></span> <span data-ttu-id="93014-183">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="93014-183">Replace example parameter names with your own values.</span></span> <span data-ttu-id="93014-184">Hello `Name` paramétert fogad hello neve toohello bővítmény a végrehajtás során.</span><span class="sxs-lookup"><span data-stu-id="93014-184">hello `Name` parameter takes hello name given toohello extension at execution time.</span></span>

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="93014-185">hello kimeneti következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="93014-185">hello output looks like hello following:</span></span>

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

<span data-ttu-id="93014-186">Bővítmény végrehajtási állapotát hello Azure-portálon is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="93014-186">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="93014-187">egy bővítmény, jelölje be hello virtuális gép, tooview hello állapotának kiválasztása **bővítmények**, és válassza ki a kívánt bővítmény hello.</span><span class="sxs-lookup"><span data-stu-id="93014-187">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-vm-extensions"></a><span data-ttu-id="93014-188">Futtassa újra a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="93014-188">Rerun VM extensions</span></span>

<span data-ttu-id="93014-189">Előfordulhat, hogy esetekben, ahol a virtuálisgép-bővítmény kell toobe futtassa újra.</span><span class="sxs-lookup"><span data-stu-id="93014-189">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="93014-190">Ehhez hello bővítmény eltávolítása, és ezután futtassa újból a hello bővítmény egy végrehajtási módszer az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="93014-190">You can do this by removing hello extension and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="93014-191">egy bővítmény tooremove futtassa a következő parancs hello Azure PowerShell modul hello.</span><span class="sxs-lookup"><span data-stu-id="93014-191">tooremove an extension, run hello following command with hello Azure PowerShell module.</span></span> <span data-ttu-id="93014-192">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="93014-192">Replace example parameter names with your own values.</span></span>

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="93014-193">Egy bővítmény hello Azure-portál használatával is eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="93014-193">An extension can also be removed using hello Azure portal.</span></span> <span data-ttu-id="93014-194">toodo így:</span><span class="sxs-lookup"><span data-stu-id="93014-194">toodo so:</span></span>

1. <span data-ttu-id="93014-195">Válasszon ki egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="93014-195">Select a virtual machine.</span></span>
2. <span data-ttu-id="93014-196">Válassza ki **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="93014-196">Select **Extensions**.</span></span>
3. <span data-ttu-id="93014-197">Válassza ki a kívánt hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="93014-197">Choose hello desired extension.</span></span>
4. <span data-ttu-id="93014-198">Válassza ki **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="93014-198">Select **Uninstall**.</span></span>

## <a name="common-vm-extensions-reference"></a><span data-ttu-id="93014-199">Közös Virtuálisgép-bővítmények hivatkozása</span><span class="sxs-lookup"><span data-stu-id="93014-199">Common VM extensions reference</span></span>
| <span data-ttu-id="93014-200">Bővítmény neve</span><span class="sxs-lookup"><span data-stu-id="93014-200">Extension name</span></span> | <span data-ttu-id="93014-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="93014-201">Description</span></span> | <span data-ttu-id="93014-202">További információ</span><span class="sxs-lookup"><span data-stu-id="93014-202">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="93014-203">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-203">Custom Script Extension for Windows</span></span> |<span data-ttu-id="93014-204">Parancsfájlok futtatásához egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="93014-204">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="93014-205">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-205">Custom Script Extension for Windows</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="93014-206">A Windows DSC-bővítményt</span><span class="sxs-lookup"><span data-stu-id="93014-206">DSC Extension for Windows</span></span> |<span data-ttu-id="93014-207">PowerShell DSC (célállapot-konfiguráció) bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-207">PowerShell DSC (Desired State Configuration) Extension</span></span> |[<span data-ttu-id="93014-208">A Windows DSC-bővítményt</span><span class="sxs-lookup"><span data-stu-id="93014-208">DSC Extension for Windows</span></span>](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="93014-209">Azure Diagnostics bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-209">Azure Diagnostics Extension</span></span> |<span data-ttu-id="93014-210">Az Azure Diagnostics kezelése</span><span class="sxs-lookup"><span data-stu-id="93014-210">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="93014-211">Azure Diagnostics bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-211">Azure Diagnostics Extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="93014-212">Azure virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-212">Azure VM Access Extension</span></span> |<span data-ttu-id="93014-213">Felhasználók és a hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="93014-213">Manage users and credentials</span></span> |[<span data-ttu-id="93014-214">Linux virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="93014-214">VM Access Extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
