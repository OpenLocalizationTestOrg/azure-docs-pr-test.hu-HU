---
title: "a Windows Azure virtuális gép bővítmény aaaOMS |} Microsoft Docs"
description: "A Windows virtuális gépet egy virtuálisgép-bővítmény használatával hello OMS-ügynököt telepíteni."
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
ms.openlocfilehash: 3000f66c0acdec1d1fad2125b8c6b72a92b1ec92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="886b7-103">A Windows MOBILE virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="886b7-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="886b7-104">Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök.</span><span class="sxs-lookup"><span data-stu-id="886b7-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="886b7-105">Virtuálisgép-bővítmény OMS-ügynököt a Windows hello közzétett és a Microsoft támogatja.</span><span class="sxs-lookup"><span data-stu-id="886b7-105">hello OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="886b7-106">hello bővítmény hello OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="886b7-106">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="886b7-107">Ez a dokumentum részletek hello támogatott platformokat, a konfigurációk és a központi telepítési beállítások hello OMS virtuálisgép-bővítmény Windows.</span><span class="sxs-lookup"><span data-stu-id="886b7-107">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="886b7-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="886b7-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="886b7-109">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="886b7-109">Operating system</span></span>
<span data-ttu-id="886b7-110">hello bővítmény OMS-ügynököt a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="886b7-110">hello OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="886b7-111">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="886b7-111">Internet connectivity</span></span>
<span data-ttu-id="886b7-112">bővítmény OMS-ügynököt a Windows hello kell lennie, hogy hello a cél virtuális gép csatlakoztatott toohello internet.</span><span class="sxs-lookup"><span data-stu-id="886b7-112">hello OMS Agent extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="886b7-113">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="886b7-113">Extension schema</span></span>

<span data-ttu-id="886b7-114">hello következő JSON látható hello OMS-ügynököt bővítmény hello sémáját.</span><span class="sxs-lookup"><span data-stu-id="886b7-114">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="886b7-115">hello bővítmény hello munkaterület azonosítója és a munkaterületen a hello cél OMS-munkaterület kulcs szükséges, ezek találhatók az hello OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="886b7-115">hello extension requires hello workspace Id and workspace key from hello target OMS workspace, these can be found in hello OMS portal.</span></span> <span data-ttu-id="886b7-116">Hello munkaterületkulcsot bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="886b7-116">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="886b7-117">Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="886b7-117">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="886b7-118">Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="886b7-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

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
### <a name="property-values"></a><span data-ttu-id="886b7-119">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="886b7-119">Property values</span></span>

| <span data-ttu-id="886b7-120">Név</span><span class="sxs-lookup"><span data-stu-id="886b7-120">Name</span></span> | <span data-ttu-id="886b7-121">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="886b7-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="886b7-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="886b7-122">apiVersion</span></span> | <span data-ttu-id="886b7-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="886b7-123">2015-06-15</span></span> |
| <span data-ttu-id="886b7-124">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="886b7-124">publisher</span></span> | <span data-ttu-id="886b7-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="886b7-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="886b7-126">type</span><span class="sxs-lookup"><span data-stu-id="886b7-126">type</span></span> | <span data-ttu-id="886b7-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="886b7-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="886b7-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="886b7-128">typeHandlerVersion</span></span> | <span data-ttu-id="886b7-129">1.0</span><span class="sxs-lookup"><span data-stu-id="886b7-129">1.0</span></span> |
| <span data-ttu-id="886b7-130">workspaceId (például)</span><span class="sxs-lookup"><span data-stu-id="886b7-130">workspaceId (e.g)</span></span> | <span data-ttu-id="886b7-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="886b7-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="886b7-132">workspaceKey (például)</span><span class="sxs-lookup"><span data-stu-id="886b7-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="886b7-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="886b7-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="886b7-134">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="886b7-134">Template deployment</span></span>

<span data-ttu-id="886b7-135">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="886b7-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="886b7-136">hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello bővítmény OMS-ügynököt az Azure Resource Manager sablon telepítése során használható.</span><span class="sxs-lookup"><span data-stu-id="886b7-136">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="886b7-137">Hello található, amely tartalmazza az OMS-ügynök Virtuálisgép-bővítmény hello mintasablon [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="886b7-137">A sample template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="886b7-138">a virtuálisgép-bővítmény JSON hello ágyazott hello virtuálisgép-erőforrás, vagy hello gyökér- vagy legfelső szintű erőforrás-kezelő JSON-sablon elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="886b7-138">hello JSON for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="886b7-139">hello JSON hello elhelyezését hello értéket hello erőforrás neve és típusa befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="886b7-139">hello placement of hello JSON affects hello value of hello resource name and type.</span></span> <span data-ttu-id="886b7-140">További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="886b7-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="886b7-141">hello alábbi példa azt feltételezi, hogy hello OMS bővítmény hello virtuálisgép-erőforrás van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="886b7-141">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="886b7-142">Ha hello bővítmény erőforrás beágyazási, hello JSON hello kerül `"resources": []` objektum hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="886b7-142">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>


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

<span data-ttu-id="886b7-143">Hello bővítmény JSON hello gyökerében hello sablon elhelyezésekor hello erőforrás neve tartalmaz egy hivatkozást toohello szülő virtuális gép, és hello típus hello beágyazott konfigurációs tükrözi.</span><span class="sxs-lookup"><span data-stu-id="886b7-143">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span> 

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

## <a name="powershell-deployment"></a><span data-ttu-id="886b7-144">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="886b7-144">PowerShell deployment</span></span>

<span data-ttu-id="886b7-145">Hello `Set-AzureRmVMExtension` parancs lehet használt toodeploy hello OMS-ügynököt a virtuális gép bővítmény tooan meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="886b7-145">hello `Set-AzureRmVMExtension` command can be used toodeploy hello OMS Agent virtual machine extension tooan existing virtual machine.</span></span> <span data-ttu-id="886b7-146">Hello parancs futtatása előtt hello nyilvános és titkos konfigurációk kell toobe PowerShell kivonattáblát tárolja.</span><span class="sxs-lookup"><span data-stu-id="886b7-146">Before running hello command, hello public and private configurations need toobe stored in a PowerShell hash table.</span></span> 

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

## <a name="troubleshoot-and-support"></a><span data-ttu-id="886b7-147">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="886b7-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="886b7-148">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="886b7-148">Troubleshoot</span></span>

<span data-ttu-id="886b7-149">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával.</span><span class="sxs-lookup"><span data-stu-id="886b7-149">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="886b7-150">egy adott virtuális Gépet, a következő parancs használatával futtatási hello kiterjesztéseinek toosee hello telepítési állapota hello Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="886b7-150">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="886b7-151">Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:</span><span class="sxs-lookup"><span data-stu-id="886b7-151">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="886b7-152">Támogatás</span><span class="sxs-lookup"><span data-stu-id="886b7-152">Support</span></span>

<span data-ttu-id="886b7-153">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="886b7-153">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="886b7-154">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="886b7-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="886b7-155">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="886b7-155">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="886b7-156">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="886b7-156">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
