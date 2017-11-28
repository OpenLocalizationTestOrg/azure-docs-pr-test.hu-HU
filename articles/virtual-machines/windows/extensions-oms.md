---
title: "A Windows MOBILE Azure virtuálisgép-bővítmény |} Microsoft Docs"
description: "Az OMS-ügynököt a Windows virtuális gépet egy virtuálisgép-bővítmény telepítése."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: nepeters
ms.openlocfilehash: d933f488fdda0c1d37892be65f2712cf0eb5694e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="25585-103">A Windows MOBILE virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="25585-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="25585-104">Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök.</span><span class="sxs-lookup"><span data-stu-id="25585-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="25585-105">Az OMS-ügynököt a Windows virtuálisgép-bővítmény közzétett és a Microsoft támogatja.</span><span class="sxs-lookup"><span data-stu-id="25585-105">The OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="25585-106">A bővítmény, az OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="25585-106">The extension installs the OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="25585-107">Ez a dokumentum részletesen a támogatott platformok, a konfigurációk és a Windows MOBILE virtuálisgép-bővítmény telepítési beállítások.</span><span class="sxs-lookup"><span data-stu-id="25585-107">This document details the supported platforms, configurations, and deployment options for the OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25585-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25585-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="25585-109">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="25585-109">Operating system</span></span>
<span data-ttu-id="25585-110">Az OMS-ügynököt bővítmény a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="25585-110">The OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="25585-111">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="25585-111">Internet connectivity</span></span>
<span data-ttu-id="25585-112">Az OMS-ügynököt bővítmény Windows megköveteli, hogy a cél virtuális gép csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="25585-112">The OMS Agent extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="25585-113">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="25585-113">Extension schema</span></span>

<span data-ttu-id="25585-114">A következő JSON jeleníti meg az OMS-ügynököt bővítmény sémáját.</span><span class="sxs-lookup"><span data-stu-id="25585-114">The following JSON shows the schema for the OMS Agent extension.</span></span> <span data-ttu-id="25585-115">A bővítmény szükséges a munkaterület azonosítója és a cél OMS-munkaterület kulcsát, ezek találhatók az OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="25585-115">The extension requires the workspace Id and workspace key from the target OMS workspace, these can be found in the OMS portal.</span></span> <span data-ttu-id="25585-116">A munkaterület-kulcs bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="25585-116">Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="25585-117">Az Azure Virtuálisgép-bővítmény védett beállítás adatokat titkosít, és csak visszafejti a cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="25585-117">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span> <span data-ttu-id="25585-118">Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="25585-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a><span data-ttu-id="25585-119">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="25585-119">Property values</span></span>

| <span data-ttu-id="25585-120">Név</span><span class="sxs-lookup"><span data-stu-id="25585-120">Name</span></span> | <span data-ttu-id="25585-121">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="25585-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="25585-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="25585-122">apiVersion</span></span> | <span data-ttu-id="25585-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="25585-123">2015-06-15</span></span> |
| <span data-ttu-id="25585-124">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="25585-124">publisher</span></span> | <span data-ttu-id="25585-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="25585-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="25585-126">type</span><span class="sxs-lookup"><span data-stu-id="25585-126">type</span></span> | <span data-ttu-id="25585-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="25585-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="25585-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="25585-128">typeHandlerVersion</span></span> | <span data-ttu-id="25585-129">1.0</span><span class="sxs-lookup"><span data-stu-id="25585-129">1.0</span></span> |
| <span data-ttu-id="25585-130">workspaceId (például)</span><span class="sxs-lookup"><span data-stu-id="25585-130">workspaceId (e.g)</span></span> | <span data-ttu-id="25585-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="25585-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="25585-132">workspaceKey (például)</span><span class="sxs-lookup"><span data-stu-id="25585-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="25585-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="25585-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="25585-134">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="25585-134">Template deployment</span></span>

<span data-ttu-id="25585-135">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="25585-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="25585-136">Az előző szakaszban ismertetett JSON-séma segítségével az Azure Resource Manager-sablonok az OMS-ügynököt bővítmény futtatása az Azure Resource Manager sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="25585-136">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="25585-137">Az OMS-ügynök Virtuálisgép-bővítmény tartalmazó minta sablon megtalálható a [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="25585-137">A sample template that includes the OMS Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="25585-138">A virtuálisgép-bővítmény JSON ágyazott a virtuálisgép-erőforrást, vagy elhelyezve, a gyökér vagy a legfelső szintű erőforrás-kezelő JSON-sablon.</span><span class="sxs-lookup"><span data-stu-id="25585-138">The JSON for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="25585-139">A JSON elhelyezésének befolyásolja az erőforrás neve és típusa értékét.</span><span class="sxs-lookup"><span data-stu-id="25585-139">The placement of the JSON affects the value of the resource name and type.</span></span> <span data-ttu-id="25585-140">További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="25585-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="25585-141">Az alábbi példa azt feltételezi, hogy a virtuálisgép-erőforrást az OMS-bővítmény van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="25585-141">The following example assumes the OMS extension is nested inside the virtual machine resource.</span></span> <span data-ttu-id="25585-142">A bővítmény erőforrás beágyazási, amikor bekerül a JSON a `"resources": []` objektum a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="25585-142">When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.</span></span>


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

<span data-ttu-id="25585-143">A bővítmény JSON elhelyezésekor a sablon gyökerében, az erőforrás nevét a szülő virtuális gép egy hivatkozást tartalmaz, és a típus beágyazott konfigurációját tükrözi.</span><span class="sxs-lookup"><span data-stu-id="25585-143">When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.</span></span> 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a><span data-ttu-id="25585-144">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="25585-144">PowerShell deployment</span></span>

<span data-ttu-id="25585-145">A `Set-AzureRmVMExtension` parancs segítségével az OMS-ügynököt virtuálisgép-bővítmény telepítése egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="25585-145">The `Set-AzureRmVMExtension` command can be used to deploy the OMS Agent virtual machine extension to an existing virtual machine.</span></span> <span data-ttu-id="25585-146">A parancs futtatása előtt a nyilvános és titkos konfigurációk kell egy kivonattáblát a PowerShell tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="25585-146">Before running the command, the public and private configurations need to be stored in a PowerShell hash table.</span></span> 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="25585-147">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="25585-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="25585-148">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="25585-148">Troubleshoot</span></span>

<span data-ttu-id="25585-149">Bővítmény központi telepítések állapotára vonatkozó lehet adatokat beolvasni az Azure-portálon, és az Azure PowerShell modul segítségével.</span><span class="sxs-lookup"><span data-stu-id="25585-149">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="25585-150">A megadott virtuális gépek bővítmények központi telepítési állapotának megtekintéséhez a következő parancsot az Azure PowerShell modullal.</span><span class="sxs-lookup"><span data-stu-id="25585-150">To see the deployment state of extensions for a given VM, run the following command using the Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="25585-151">A következő könyvtárban található fájlok kerül a bővítmény végrehajtás kimenetének:</span><span class="sxs-lookup"><span data-stu-id="25585-151">Extension execution output is logged to files found in the following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="25585-152">Támogatás</span><span class="sxs-lookup"><span data-stu-id="25585-152">Support</span></span>

<span data-ttu-id="25585-153">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők a a [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="25585-153">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="25585-154">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="25585-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="25585-155">Lépjen a [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="25585-155">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="25585-156">Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="25585-156">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
