---
title: "Hálózati figyelő ügynök virtuálisgép-bővítmény Linux aaaAzure |} Microsoft Docs"
description: "Hello hálózati figyelő ügynök a Linux virtuális gépet egy virtuálisgép-bővítmény telepítése."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Hálózati figyelő ügynök virtuálisgép-bővítmény Linux

## <a name="overview"></a>Áttekintés

[Az Azure hálózati figyelőt](https://review.docs.microsoft.com/en-us/azure/network-watcher/) hálózati teljesítmény figyelési, diagnosztikai és elemzési szolgáltatás, amely lehetővé teszi az Azure-hálózatok figyelését. Hálózati figyelő ügynök virtuálisgép-bővítmény hello feltétele néhány hello hálózati figyelőt szolgáltatást az Azure virtuális gépeken. Ez magában foglalja, igény szerint és egyéb speciális funkciók a hálózati forgalom rögzítése.

Ez a dokumentum részletek hello támogatott platformokat, a központi telepítési beállítások hello hálózati figyelő ügynök virtuálisgép-bővítmény Linux.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer

Hálózati figyelő ügynök bővítmény hello is futtathatók a a Linux terjesztéseket:

| Disztribúció | Verzió |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS és 12.04 LTS |
| Debian | 7. és 8 |
| RedHat | 6.x, 7.x és |
| Oracle Linux | 7 x |
| SUSE | 11 és 12 |
| OpenSuse | 7.0 |
| CentOS | 7.0 |

Vegye figyelembe, hogy CoreOS jelenleg nem támogatott.

### <a name="internet-connectivity"></a>Internetkapcsolat

Egyes hálózati figyelő ügynök funkcionalitást hello használatához hello cél virtuális gép csatlakoztatott toohello Internet kell lennie. Hello képességét tooestablish kimenő kapcsolatok nélkül néhány hello hálózati figyelő ügynök szolgáltatást előfordulhat, hogy hibás működését, vagy már nem érhető el. További részletekért lásd: hello [hálózati figyelőt dokumentáció](https://review.docs.microsoft.com/en-us/azure/network-watcher/).

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
        "type": "NetworkWatcherAgentLinux",
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
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello hálózati figyelő ügynöke bővítmény egy Azure Resource Manager sablon telepítése során használható.

## <a name="azure-cli-deployment"></a>Az Azure CLI-telepítés

hello Azure CLI használt toodeploy hello hálózati figyelő ügynök VM bővítmény tooan meglévő virtuális gép is lehet.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshooting"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni a hello Azure-portálon, és hello Azure parancssori felület használatával. egy adott virtuális Gépet, futtassa a következő parancs használatával hello kiterjesztéseinek toosee hello telepítési állapota hello Azure parancssori felület.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, tekintse meg a toohello hálózati figyelőt dokumentációját, vagy forduljon hello Azure hello szakértői [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
