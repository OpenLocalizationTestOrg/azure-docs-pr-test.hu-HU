---
title: "a Windows virtuális gép parancsprogramok futtatására szolgáló bővítmény aaaCustom |} Microsoft Docs"
description: "Azure virtuális gép konfigurációs feladatok automatizálása a Windows virtuális gép távoli hello egyéni parancsfájl kiterjesztése toorun PowerShell-parancsfájlok használatával"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: cf7bb895dd211f07fd010dc03b68cd77df1127b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a><span data-ttu-id="180ae-103">Egyéni parancsfájl kiterjesztése a Windows hello klasszikus telepítési modell segítségével</span><span class="sxs-lookup"><span data-stu-id="180ae-103">Custom Script Extension for Windows using hello classic deployment model</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="180ae-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="180ae-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="180ae-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="180ae-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="180ae-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="180ae-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="180ae-107">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="180ae-107">Learn how too[perform these steps using hello Resource Manager model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="180ae-108">Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="180ae-108">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="180ae-109">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="180ae-109">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="180ae-110">Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt.</span><span class="sxs-lookup"><span data-stu-id="180ae-110">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="180ae-111">Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="180ae-111">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="180ae-112">Ez a dokumentum részletesen, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello a Azure PowerShell-modult, Azure Resource Manager-sablonok és a Windows rendszer hibaelhárítási részleteket.</span><span class="sxs-lookup"><span data-stu-id="180ae-112">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="180ae-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="180ae-113">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="180ae-114">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="180ae-114">Operating System</span></span>

<span data-ttu-id="180ae-115">Egyéni parancsprogramok futtatására szolgáló bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="180ae-115">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="180ae-116">Parancsfájl helyét</span><span class="sxs-lookup"><span data-stu-id="180ae-116">Script Location</span></span>

<span data-ttu-id="180ae-117">hello parancsfájl az Azure storage vagy egy érvényes URL-cím elérhető bármely más helyen tárolt toobe kell.</span><span class="sxs-lookup"><span data-stu-id="180ae-117">hello script needs toobe stored in Azure storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="180ae-118">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="180ae-118">Internet Connectivity</span></span>

<span data-ttu-id="180ae-119">hello egyéni parancsfájl kiterjesztése Windows rendszerhez szükséges, hogy hello a cél virtuális gép csatlakoztatott toohello internet.</span><span class="sxs-lookup"><span data-stu-id="180ae-119">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="180ae-120">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="180ae-120">Extension schema</span></span>

<span data-ttu-id="180ae-121">hello következő JSON jeleníti meg az egyéni parancsprogramok futtatására szolgáló bővítmény hello hello séma.</span><span class="sxs-lookup"><span data-stu-id="180ae-121">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="180ae-122">egy parancsfájl helyét (Azure Storage vagy más helyre érvényes URL-CÍMMEL rendelkező), valamint egy parancs tooexecute hello bővítmény kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="180ae-122">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="180ae-123">Ha használja az Azure Storage hello parancsfájl forrásaként, az Azure storage fiók név és fiókkulcs kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="180ae-123">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="180ae-124">Legyen bizalmas adatokat a rendszer ezeket az elemeket, és hello bővítmények védett beállítás konfigurációjában megadott.</span><span class="sxs-lookup"><span data-stu-id="180ae-124">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="180ae-125">Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="180ae-125">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="180ae-126">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="180ae-126">Property values</span></span>

| <span data-ttu-id="180ae-127">Név</span><span class="sxs-lookup"><span data-stu-id="180ae-127">Name</span></span> | <span data-ttu-id="180ae-128">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="180ae-128">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="180ae-129">apiVersion</span><span class="sxs-lookup"><span data-stu-id="180ae-129">apiVersion</span></span> | <span data-ttu-id="180ae-130">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="180ae-130">2015-06-15</span></span> |
| <span data-ttu-id="180ae-131">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="180ae-131">publisher</span></span> | <span data-ttu-id="180ae-132">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="180ae-132">Microsoft.Compute</span></span> |
| <span data-ttu-id="180ae-133">Bővítmény</span><span class="sxs-lookup"><span data-stu-id="180ae-133">extension</span></span> | <span data-ttu-id="180ae-134">CustomScriptExtension</span><span class="sxs-lookup"><span data-stu-id="180ae-134">CustomScriptExtension</span></span> |
| <span data-ttu-id="180ae-135">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="180ae-135">typeHandlerVersion</span></span> | <span data-ttu-id="180ae-136">1.8</span><span class="sxs-lookup"><span data-stu-id="180ae-136">1.8</span></span> |
| <span data-ttu-id="180ae-137">fileUris (például)</span><span class="sxs-lookup"><span data-stu-id="180ae-137">fileUris (e.g)</span></span> | <span data-ttu-id="180ae-138">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="180ae-138">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="180ae-139">commandToExecute (például)</span><span class="sxs-lookup"><span data-stu-id="180ae-139">commandToExecute (e.g)</span></span> | <span data-ttu-id="180ae-140">PowerShell - ExecutionPolicy Unrestricted - fájl konfigurálása zene-app.ps1</span><span class="sxs-lookup"><span data-stu-id="180ae-140">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="180ae-141">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="180ae-141">Template deployment</span></span>

<span data-ttu-id="180ae-142">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="180ae-142">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="180ae-143">hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény egy Azure Resource Manager sablon telepítése során használható.</span><span class="sxs-lookup"><span data-stu-id="180ae-143">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="180ae-144">Amely tartalmazza az egyéni parancsprogramok futtatására szolgáló bővítmény itt található hello mintasablon [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="180ae-144">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="180ae-145">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="180ae-145">PowerShell deployment</span></span>

<span data-ttu-id="180ae-146">Hello `Set-AzureVMCustomScriptExtension` parancs lehet használt tooadd hello egyéni parancsfájl kiterjesztése tooan meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="180ae-146">hello `Set-AzureVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="180ae-147">További információkért lásd: [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="180ae-147">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="180ae-148">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="180ae-148">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="180ae-149">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="180ae-149">Troubleshoot</span></span>

<span data-ttu-id="180ae-150">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával.</span><span class="sxs-lookup"><span data-stu-id="180ae-150">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="180ae-151">toosee hello telepítési állapota egy adott virtuális Gépet, futtassa a következő parancs hello-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="180ae-151">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="180ae-152">Bővítmény végrehajtási kimeneti velünk naplózott toofiles hello directory következő hello cél virtuális gépen található.</span><span class="sxs-lookup"><span data-stu-id="180ae-152">Extension execution output us logged toofiles found in hello following directory on hello target virtual machine.</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="180ae-153">hello parancsfájl önmagában directory hello cél virtuális gépen a következő hello be lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="180ae-153">hello script itself is downloaded into hello following directory on hello target virtual machine.</span></span>

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a><span data-ttu-id="180ae-154">Támogatás</span><span class="sxs-lookup"><span data-stu-id="180ae-154">Support</span></span>

<span data-ttu-id="180ae-155">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="180ae-155">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="180ae-156">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="180ae-156">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="180ae-157">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="180ae-157">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="180ae-158">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="180ae-158">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
