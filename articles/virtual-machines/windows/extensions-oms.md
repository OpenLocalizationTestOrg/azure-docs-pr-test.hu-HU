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
# <a name="oms-virtual-machine-extension-for-windows"></a>A Windows MOBILE virtuálisgép-bővítmény

Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök. Virtuálisgép-bővítmény OMS-ügynököt a Windows hello közzétett és a Microsoft támogatja. hello bővítmény hello OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület. Ez a dokumentum részletek hello támogatott platformokat, a konfigurációk és a központi telepítési beállítások hello OMS virtuálisgép-bővítmény Windows.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer
hello bővítmény OMS-ügynököt a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.

### <a name="internet-connectivity"></a>Internetkapcsolat
bővítmény OMS-ügynököt a Windows hello kell lennie, hogy hello a cél virtuális gép csatlakoztatott toohello internet. 

## <a name="extension-schema"></a>A séma kiterjesztése

hello következő JSON látható hello OMS-ügynököt bővítmény hello sémáját. hello bővítmény hello munkaterület azonosítója és a munkaterületen a hello cél OMS-munkaterület kulcs szükséges, ezek találhatók az hello OMS-portálon. Hello munkaterületkulcsot bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni. Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen. Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.

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
### <a name="property-values"></a>A tulajdonság értékek

| Név | Érték / – példa |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Közzétevő | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| workspaceId (például) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (például) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |

## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello bővítmény OMS-ügynököt az Azure Resource Manager sablon telepítése során használható. Hello található, amely tartalmazza az OMS-ügynök Virtuálisgép-bővítmény hello mintasablon [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

a virtuálisgép-bővítmény JSON hello ágyazott hello virtuálisgép-erőforrás, vagy hello gyökér- vagy legfelső szintű erőforrás-kezelő JSON-sablon elhelyezve. hello JSON hello elhelyezését hello értéket hello erőforrás neve és típusa befolyásolja. További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md). 

hello alábbi példa azt feltételezi, hogy hello OMS bővítmény hello virtuálisgép-erőforrás van beágyazva. Ha hello bővítmény erőforrás beágyazási, hello JSON hello kerül `"resources": []` objektum hello virtuális gép.


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

Hello bővítmény JSON hello gyökerében hello sablon elhelyezésekor hello erőforrás neve tartalmaz egy hivatkozást toohello szülő virtuális gép, és hello típus hello beágyazott konfigurációs tükrözi. 

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

## <a name="powershell-deployment"></a>PowerShell telepítése

Hello `Set-AzureRmVMExtension` parancs lehet használt toodeploy hello OMS-ügynököt a virtuális gép bővítmény tooan meglévő virtuális gépet. Hello parancs futtatása előtt hello nyilvános és titkos konfigurációk kell toobe PowerShell kivonattáblát tárolja. 

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

## <a name="troubleshoot-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshoot"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával. egy adott virtuális Gépet, a következő parancs használatával futtatási hello kiterjesztéseinek toosee hello telepítési állapota hello Azure PowerShell modul.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
