---
title: "Egyéni parancsfájl kiterjesztése a Windows aaaAzure |} Microsoft Docs"
description: "Windows virtuális gép konfigurációs feladatok automatizálásához hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: 97e065242e9fed116ee20b074f4e302a0cd10585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="39897-103">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="39897-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="39897-104">Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="39897-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="39897-105">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="39897-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="39897-106">Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt.</span><span class="sxs-lookup"><span data-stu-id="39897-106">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="39897-107">Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="39897-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="39897-108">Ez a dokumentum részletesen, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello a Azure PowerShell-modult, Azure Resource Manager-sablonok és a Windows rendszer hibaelhárítási részleteket.</span><span class="sxs-lookup"><span data-stu-id="39897-108">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39897-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="39897-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="39897-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="39897-110">Operating System</span></span>

<span data-ttu-id="39897-111">Egyéni parancsprogramok futtatására szolgáló bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="39897-111">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="39897-112">Parancsfájl helyét</span><span class="sxs-lookup"><span data-stu-id="39897-112">Script Location</span></span>

<span data-ttu-id="39897-113">hello parancsfájl kell toobe Azure Blob Storage tárolóban, vagy egy érvényes URL-cím elérhető bármely más helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="39897-113">hello script needs toobe stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="39897-114">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="39897-114">Internet Connectivity</span></span>

<span data-ttu-id="39897-115">hello egyéni parancsfájl kiterjesztése Windows rendszerhez szükséges, hogy hello a cél virtuális gép csatlakoztatott toohello internet.</span><span class="sxs-lookup"><span data-stu-id="39897-115">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="39897-116">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="39897-116">Extension schema</span></span>

<span data-ttu-id="39897-117">hello következő JSON jeleníti meg az egyéni parancsprogramok futtatására szolgáló bővítmény hello hello séma.</span><span class="sxs-lookup"><span data-stu-id="39897-117">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="39897-118">egy parancsfájl helyét (Azure Storage vagy más helyre érvényes URL-CÍMMEL rendelkező), valamint egy parancs tooexecute hello bővítmény kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="39897-118">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="39897-119">Ha használja az Azure Storage hello parancsfájl forrásaként, az Azure storage fiók név és fiókkulcs kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="39897-119">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="39897-120">Legyen bizalmas adatokat a rendszer ezeket az elemeket, és hello bővítmények védett beállítás konfigurációjában megadott.</span><span class="sxs-lookup"><span data-stu-id="39897-120">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="39897-121">Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="39897-121">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

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
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="39897-122">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="39897-122">Property values</span></span>

| <span data-ttu-id="39897-123">Név</span><span class="sxs-lookup"><span data-stu-id="39897-123">Name</span></span> | <span data-ttu-id="39897-124">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="39897-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="39897-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="39897-125">apiVersion</span></span> | <span data-ttu-id="39897-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="39897-126">2015-06-15</span></span> |
| <span data-ttu-id="39897-127">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="39897-127">publisher</span></span> | <span data-ttu-id="39897-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="39897-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="39897-129">type</span><span class="sxs-lookup"><span data-stu-id="39897-129">type</span></span> | <span data-ttu-id="39897-130">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="39897-130">extensions</span></span> |
| <span data-ttu-id="39897-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="39897-131">typeHandlerVersion</span></span> | <span data-ttu-id="39897-132">1.9</span><span class="sxs-lookup"><span data-stu-id="39897-132">1.9</span></span> |
| <span data-ttu-id="39897-133">fileUris (például)</span><span class="sxs-lookup"><span data-stu-id="39897-133">fileUris (e.g)</span></span> | <span data-ttu-id="39897-134">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="39897-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="39897-135">commandToExecute (például)</span><span class="sxs-lookup"><span data-stu-id="39897-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="39897-136">PowerShell - ExecutionPolicy Unrestricted - fájl konfigurálása zene-app.ps1</span><span class="sxs-lookup"><span data-stu-id="39897-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="39897-137">storageAccountName (például)</span><span class="sxs-lookup"><span data-stu-id="39897-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="39897-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="39897-138">examplestorageacct</span></span> |
| <span data-ttu-id="39897-139">storageAccountKey (például)</span><span class="sxs-lookup"><span data-stu-id="39897-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="39897-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="39897-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="39897-141">**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="39897-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="39897-142">Hello-neveket használja, mint fent tooavoid telepítési problémák esetére.</span><span class="sxs-lookup"><span data-stu-id="39897-142">Use hello names as seen above tooavoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="39897-143">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="39897-143">Template deployment</span></span>

<span data-ttu-id="39897-144">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="39897-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="39897-145">hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény egy Azure Resource Manager sablon telepítése során használható.</span><span class="sxs-lookup"><span data-stu-id="39897-145">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="39897-146">Amely tartalmazza az egyéni parancsprogramok futtatására szolgáló bővítmény itt található hello mintasablon [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="39897-146">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="39897-147">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="39897-147">PowerShell deployment</span></span>

<span data-ttu-id="39897-148">Hello `Set-AzureRmVMCustomScriptExtension` parancs lehet használt tooadd hello egyéni parancsfájl kiterjesztése tooan meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="39897-148">hello `Set-AzureRmVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="39897-149">További információkért lásd: [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="39897-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="39897-150">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="39897-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="39897-151">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="39897-151">Troubleshoot</span></span>

<span data-ttu-id="39897-152">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával.</span><span class="sxs-lookup"><span data-stu-id="39897-152">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="39897-153">toosee hello telepítési állapota egy adott virtuális Gépet, futtassa a következő parancs hello-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="39897-153">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="39897-154">A kimeneti bővítmény végrehajtási van naplózott toofiles hello alatt található a következő könyvtár hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="39897-154">Extension execution output is logged toofiles found under hello following directory on hello target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="39897-155">a megadott hello fájlokat tölti be hello directory következő hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="39897-155">hello specified files are downloaded into hello following directory on hello target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="39897-156">Ha `<n>` decimális egész szám, amely hello bővítmény végrehajtások közötti módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="39897-156">where `<n>` is a decimal integer which may change between executions of hello extension.</span></span>  <span data-ttu-id="39897-157">Hello `1.*` értéke megegyezik a tényleges, aktuális hello `typeHandlerVersion` hello bővítmény értékét.</span><span class="sxs-lookup"><span data-stu-id="39897-157">hello `1.*` value matches hello actual, current `typeHandlerVersion` value of hello extension.</span></span>  <span data-ttu-id="39897-158">Hello tényleges directory lehet például `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="39897-158">For example, hello actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="39897-159">Hello végrehajtásakor `commandToExecute` parancs hello bővítmény fogja beállítani a könyvtár (például `...\Downloads\2`), az aktuális munkakönyvtárban hello.</span><span class="sxs-lookup"><span data-stu-id="39897-159">When executing hello `commandToExecute` command, hello extension will have set this directory (e.g., `...\Downloads\2`) as hello current working directory.</span></span> <span data-ttu-id="39897-160">Ez lehetővé teszi, hogy relatív elérési utak toolocate hello fájlok hello használata letöltött hello `fileURIs` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="39897-160">This enables hello use of relative paths toolocate hello files downloaded via hello `fileURIs` property.</span></span> <span data-ttu-id="39897-161">Lásd az alábbi táblázatban hello példák.</span><span class="sxs-lookup"><span data-stu-id="39897-161">See hello table below for examples.</span></span>

<span data-ttu-id="39897-162">Hello abszolút letöltési mappa elérési útját idővel változhatnak, mert-e a fájl relatív parancsfájl elérési utak, a hello jobb tooopt `commandToExecute` karakterlánc, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="39897-162">Since hello absolute download path may vary over time, it is better tooopt for relative script/file paths in hello `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="39897-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="39897-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="39897-164">Útvonal-információinak után hello első URI-szegmens őrzi meg a fájlok letöltött hello `fileUris` tulajdonságlistát.</span><span class="sxs-lookup"><span data-stu-id="39897-164">Path information after hello first URI segment is retained for files downloaded via hello `fileUris` property list.</span></span>  <span data-ttu-id="39897-165">Ahogy az az alábbi táblázat hello, letöltött fájlok letöltési alkönyvtárak tooreflect hello szerkezete hello rendszer társítsa `fileUris` értékeket.</span><span class="sxs-lookup"><span data-stu-id="39897-165">As shown in hello table below, downloaded files are mapped into download subdirectories tooreflect hello structure of hello `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="39897-166">A letöltött fájlok példák</span><span class="sxs-lookup"><span data-stu-id="39897-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="39897-167">URI-azonosító található fileUris</span><span class="sxs-lookup"><span data-stu-id="39897-167">URI in fileUris</span></span> | <span data-ttu-id="39897-168">A letöltött helyzetéhez viszonyítva</span><span class="sxs-lookup"><span data-stu-id="39897-168">Relative downloaded location</span></span> | <span data-ttu-id="39897-169">Abszolút helye letöltött *</span><span class="sxs-lookup"><span data-stu-id="39897-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="39897-170">\*Mint a fenti hello könyvtár abszolút elérési utak változik hello VM, de nem belül egyetlen végrehajtásának hello CustomScript bővítménnyel hello élettartamuk során.</span><span class="sxs-lookup"><span data-stu-id="39897-170">\* As above, hello absolute directory paths will change over hello lifetime of hello VM, but not within a single execution of hello CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="39897-171">Támogatás</span><span class="sxs-lookup"><span data-stu-id="39897-171">Support</span></span>

<span data-ttu-id="39897-172">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello [MSDN Azure és a Stack Overflow fórumok] hello (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="39897-172">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="39897-173">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="39897-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="39897-174">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="39897-174">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="39897-175">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="39897-175">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
