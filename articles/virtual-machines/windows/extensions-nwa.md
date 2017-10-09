---
title: "a Windows hálózati figyelő ügynök virtuálisgép-bővítmény aaaAzure |} Microsoft Docs"
description: "Hello hálózati figyelő ügynök a Windows virtuális gépet egy virtuálisgép-bővítmény telepítése."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Windows hálózati figyelő ügynök virtuálisgép-bővítmény

## <a name="overview"></a>Áttekintés

[Az Azure hálózati figyelőt](https://review.docs.microsoft.com/en-us/azure/network-watcher/) hálózati teljesítmény figyelési, diagnosztikai és elemzési szolgáltatás, amely lehetővé teszi az Azure-hálózatok figyelését. Hálózati figyelő ügynök virtuálisgép-bővítmény hello feltétele néhány hello hálózati figyelőt szolgáltatást az Azure virtuális gépeken. Ez magában foglalja, igény szerint és egyéb speciális funkciók a hálózati forgalom rögzítése.

Ez a dokumentum részletek hello támogatott platformokat, a központi telepítési beállítások hello hálózati figyelő ügynök virtuálisgép-bővítmény Windows.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer

Hálózati figyelő ügynök bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását. Vegye figyelembe, hogy a Nano Server hello jelenleg nem támogatott.

### <a name="internet-connectivity"></a>Internetkapcsolat

Egyes hálózati figyelő ügynök funkcionalitást hello használatához hello cél virtuális gép csatlakoztatott toohello Internet kell lennie. Hello képességét tooestablish kimenő kapcsolatok nélkül néhány hello hálózati figyelő ügynök szolgáltatást előfordulhat, hogy hibás működését, vagy már nem érhető el. További részletekért lásd: hello [hálózati figyelőt dokumentáció](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>A séma kiterjesztése

hello következő JSON látható hello hálózati figyelő ügynök bővítmény hello sémáját. hello bővítmény sem szükséges és nem támogatja a megadott felhasználó által megadott beállításokat jelenleg és az alapértelmezett beállításon támaszkodik.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>A tulajdonság értékek

| Név | Érték / – példa |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Közzétevő | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello hálózati figyelő ügynöke bővítmény egy Azure Resource Manager sablon telepítése során használható.

## <a name="powershell-deployment"></a>PowerShell telepítése

Hello `Set-AzureRmVMExtension` parancs lehet használt toodeploy hello hálózati figyelő ügynök virtuális gép bővítmény tooan meglévő virtuális gépet.

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshooting"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával. egy adott virtuális Gépet, a következő parancs használatával futtatási hello kiterjesztéseinek toosee hello telepítési állapota hello Azure PowerShell modul.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, toohello megfigyelő felhasználói útmutató dokumentációjában talál, vagy lépjen kapcsolatba az hello Azure hello szakértői [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
