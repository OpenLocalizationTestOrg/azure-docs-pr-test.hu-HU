---
title: "A Windows Azure egyéni parancsprogramok futtatására szolgáló bővítmény |} Microsoft Docs"
description: "Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával Windows virtuális gép konfigurációs feladatok automatizálásához"
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
ms.openlocfilehash: a6f417ea6575b81258998ae3b31c10e9df59b603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="f04e6-103">A Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="f04e6-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="f04e6-104">Az egyéni parancsprogramok futtatására szolgáló bővítmény és hajtanak végre a parancsfájl az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f04e6-104">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="f04e6-105">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="f04e6-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="f04e6-106">Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott futásidejű bővítmény: az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f04e6-106">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span> <span data-ttu-id="f04e6-107">Az egyéni parancsprogramok futtatására szolgáló bővítmény integrálódik az Azure Resource Manager-sablonok, és is futtathat az Azure parancssori felület, PowerShell, Azure-portálon vagy az Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="f04e6-107">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="f04e6-108">Ez a dokumentum részletesen az egyéni parancsprogramok futtatására szolgáló bővítmény használatával az Azure PowerShell modul, Azure Resource Manager-sablonok és a Windows rendszer hibaelhárítási részletek használata.</span><span class="sxs-lookup"><span data-stu-id="f04e6-108">This document details how to use the Custom Script Extension using the Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f04e6-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f04e6-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="f04e6-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="f04e6-110">Operating System</span></span>

<span data-ttu-id="f04e6-111">Az egyéni parancsprogramok futtatására szolgáló bővítmény a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="f04e6-111">The Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="f04e6-112">Parancsfájl helyét</span><span class="sxs-lookup"><span data-stu-id="f04e6-112">Script Location</span></span>

<span data-ttu-id="f04e6-113">A parancsfájl kell Azure Blob Storage tárolóban, vagy egy érvényes URL-cím elérhető bármely más helyen kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="f04e6-113">The script needs to be stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="f04e6-114">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="f04e6-114">Internet Connectivity</span></span>

<span data-ttu-id="f04e6-115">Az egyéni parancsfájl kiterjesztése a Windows megköveteli, hogy a cél virtuális gép csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="f04e6-115">The Custom Script Extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="f04e6-116">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="f04e6-116">Extension schema</span></span>

<span data-ttu-id="f04e6-117">A következő JSON a séma az egyéni parancsprogramok futtatására szolgáló bővítmény jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f04e6-117">The following JSON shows the schema for the Custom Script Extension.</span></span> <span data-ttu-id="f04e6-118">A bővítmény (Azure Storage vagy más helyre érvényes URL-CÍMMEL rendelkező) a parancsfájl helyét, valamint végrehajtandó parancsot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f04e6-118">The extension requires a script location (Azure Storage or other location with valid URL), and a command to execute.</span></span> <span data-ttu-id="f04e6-119">Ha a parancsprogram forráskódjának Azure Storage használ, az Azure storage fiók név és fiókkulcs kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="f04e6-119">If using Azure Storage as the script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="f04e6-120">Legyen bizalmas adatokat a rendszer ezeket az elemeket, és a bővítmények védett beállítás konfigurációjában megadott.</span><span class="sxs-lookup"><span data-stu-id="f04e6-120">These items should be treated as sensitive data and specified in the extensions protected setting configuration.</span></span> <span data-ttu-id="f04e6-121">Az Azure Virtuálisgép-bővítmény védett beállítás adatokat titkosít, és csak visszafejti a cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f04e6-121">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="f04e6-122">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="f04e6-122">Property values</span></span>

| <span data-ttu-id="f04e6-123">Név</span><span class="sxs-lookup"><span data-stu-id="f04e6-123">Name</span></span> | <span data-ttu-id="f04e6-124">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="f04e6-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="f04e6-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="f04e6-125">apiVersion</span></span> | <span data-ttu-id="f04e6-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="f04e6-126">2015-06-15</span></span> |
| <span data-ttu-id="f04e6-127">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="f04e6-127">publisher</span></span> | <span data-ttu-id="f04e6-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="f04e6-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="f04e6-129">type</span><span class="sxs-lookup"><span data-stu-id="f04e6-129">type</span></span> | <span data-ttu-id="f04e6-130">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="f04e6-130">extensions</span></span> |
| <span data-ttu-id="f04e6-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="f04e6-131">typeHandlerVersion</span></span> | <span data-ttu-id="f04e6-132">1.9</span><span class="sxs-lookup"><span data-stu-id="f04e6-132">1.9</span></span> |
| <span data-ttu-id="f04e6-133">fileUris (például)</span><span class="sxs-lookup"><span data-stu-id="f04e6-133">fileUris (e.g)</span></span> | <span data-ttu-id="f04e6-134">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="f04e6-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="f04e6-135">commandToExecute (például)</span><span class="sxs-lookup"><span data-stu-id="f04e6-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="f04e6-136">PowerShell - ExecutionPolicy Unrestricted - fájl konfigurálása zene-app.ps1</span><span class="sxs-lookup"><span data-stu-id="f04e6-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="f04e6-137">storageAccountName (például)</span><span class="sxs-lookup"><span data-stu-id="f04e6-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="f04e6-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="f04e6-138">examplestorageacct</span></span> |
| <span data-ttu-id="f04e6-139">storageAccountKey (például)</span><span class="sxs-lookup"><span data-stu-id="f04e6-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="f04e6-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="f04e6-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="f04e6-141">**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="f04e6-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="f04e6-142">-Neveket használja, a fent látható telepítési problémák elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f04e6-142">Use the names as seen above to avoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="f04e6-143">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="f04e6-143">Template deployment</span></span>

<span data-ttu-id="f04e6-144">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="f04e6-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="f04e6-145">Az előző szakaszban ismertetett JSON-séma segítségével az Azure Resource Manager-sablonok az egyéni parancsprogramok futtatására szolgáló bővítmény futtatása az Azure Resource Manager sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="f04e6-145">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="f04e6-146">Itt található, amely tartalmazza az egyéni parancsprogramok futtatására szolgáló bővítmény mintasablon [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="f04e6-146">A sample template that includes the Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="f04e6-147">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="f04e6-147">PowerShell deployment</span></span>

<span data-ttu-id="f04e6-148">A `Set-AzureRmVMCustomScriptExtension` parancs használható az egyéni parancsprogramok futtatására szolgáló bővítmény hozzáadása egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f04e6-148">The `Set-AzureRmVMCustomScriptExtension` command can be used to add the Custom Script extension to an existing virtual machine.</span></span> <span data-ttu-id="f04e6-149">További információkért lásd: [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="f04e6-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="f04e6-150">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="f04e6-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="f04e6-151">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f04e6-151">Troubleshoot</span></span>

<span data-ttu-id="f04e6-152">Bővítmény központi telepítések állapotára vonatkozó lehet adatokat beolvasni az Azure-portálon, és az Azure PowerShell modul segítségével.</span><span class="sxs-lookup"><span data-stu-id="f04e6-152">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="f04e6-153">A következő parancsot egy adott virtuális gép bővítmények központi telepítési állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="f04e6-153">To see the deployment state of extensions for a given VM, run the following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="f04e6-154">A cél virtuális gépen a következő könyvtárban található fájlok bővítmény végrehajtás kimenetének van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="f04e6-154">Extension execution output is logged to files found under the following directory on the target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="f04e6-155">A megadott fájlok letöltése a cél virtuális gépen a következő könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="f04e6-155">The specified files are downloaded into the following directory on the target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="f04e6-156">Ha `<n>` decimális egész szám, amely módosíthatja a bővítmény végrehajtások közötti.</span><span class="sxs-lookup"><span data-stu-id="f04e6-156">where `<n>` is a decimal integer which may change between executions of the extension.</span></span>  <span data-ttu-id="f04e6-157">A `1.*` értéke megegyezik a tényleges, jelenlegi `typeHandlerVersion` érték a kiterjesztést.</span><span class="sxs-lookup"><span data-stu-id="f04e6-157">The `1.*` value matches the actual, current `typeHandlerVersion` value of the extension.</span></span>  <span data-ttu-id="f04e6-158">A tényleges directory lehet például `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="f04e6-158">For example, the actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="f04e6-159">Végrehajtásakor a `commandToExecute` parancs, a bővítmény fogja beállítani a könyvtár (pl. `...\Downloads\2`), az aktuális munkakönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f04e6-159">When executing the `commandToExecute` command, the extension will have set this directory (e.g., `...\Downloads\2`) as the current working directory.</span></span> <span data-ttu-id="f04e6-160">Ez lehetővé teszi, hogy keresse meg a letöltött fájlokat relatív elérési utak használatát a `fileURIs` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f04e6-160">This enables the use of relative paths to locate the files downloaded via the `fileURIs` property.</span></span> <span data-ttu-id="f04e6-161">Lásd az alábbi táblázatban a példákat.</span><span class="sxs-lookup"><span data-stu-id="f04e6-161">See the table below for examples.</span></span>

<span data-ttu-id="f04e6-162">Mivel a abszolút letöltési mappa elérési útját idővel változhatnak, ajánlott, hogy a fájl relatív parancsfájl elérési utak a a `commandToExecute` karakterlánc, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="f04e6-162">Since the absolute download path may vary over time, it is better to opt for relative script/file paths in the `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="f04e6-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="f04e6-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="f04e6-164">Útvonal-információinak után az első URI-szegmens őrzi meg a letöltött fájlok a `fileUris` tulajdonságlistát.</span><span class="sxs-lookup"><span data-stu-id="f04e6-164">Path information after the first URI segment is retained for files downloaded via the `fileUris` property list.</span></span>  <span data-ttu-id="f04e6-165">Ahogy az alábbi táblázatban is látható, letöltött fájlok letöltési alkönyvtárak szerkezete megfelelően be van leképezve a `fileUris` értékeket.</span><span class="sxs-lookup"><span data-stu-id="f04e6-165">As shown in the table below, downloaded files are mapped into download subdirectories to reflect the structure of the `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="f04e6-166">A letöltött fájlok példák</span><span class="sxs-lookup"><span data-stu-id="f04e6-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="f04e6-167">URI-azonosító található fileUris</span><span class="sxs-lookup"><span data-stu-id="f04e6-167">URI in fileUris</span></span> | <span data-ttu-id="f04e6-168">A letöltött helyzetéhez viszonyítva</span><span class="sxs-lookup"><span data-stu-id="f04e6-168">Relative downloaded location</span></span> | <span data-ttu-id="f04e6-169">Abszolút helye letöltött *</span><span class="sxs-lookup"><span data-stu-id="f04e6-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="f04e6-170">\*Mint a fenti abszolút elérési útjainak fogja módosítani a virtuális gép, de nem esik a CustomScript bővítménnyel egyetlen végrehajtásának életciklusa alatt.</span><span class="sxs-lookup"><span data-stu-id="f04e6-170">\* As above, the absolute directory paths will change over the lifetime of the VM, but not within a single execution of the CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="f04e6-171">Támogatás</span><span class="sxs-lookup"><span data-stu-id="f04e6-171">Support</span></span>

<span data-ttu-id="f04e6-172">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon a [MSDN Azure és a Stack Overflow fórumokban] Azure szakértők (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="f04e6-172">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="f04e6-173">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="f04e6-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="f04e6-174">Lépjen a [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="f04e6-174">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="f04e6-175">Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="f04e6-175">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
