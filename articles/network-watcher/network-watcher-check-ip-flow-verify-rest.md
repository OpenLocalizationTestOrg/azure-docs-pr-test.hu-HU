---
title: "Ellenőrizze a aaaVerify forgalom Azure hálózati figyelő IP-adatfolyam - REST |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Ellenőrizze, hogy a forgalom engedélyezett vagy megtagadott IP-folyamat az Azure hálózati figyelőt összetevője ellenőrzése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Az Azure REST API-n](network-watcher-check-ip-flow-verify-rest.md)


IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt. hello érvényesítési futtathatja a bejövő vagy kimenő forgalmat. Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának. IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása. Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.

## <a name="before-you-begin"></a>Előkészületek

ARMclient használt toocall hello REST API használatával PowerShell. ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

Ez a forgatókönyv IP folyamata ellenőrizze tooverify használ, ha egy virtuális gép működik tooanother gép 443-as porton keresztül. Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza. További információ az IP-adatfolyam győződjön meg arról, toolearn látogasson el [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)

Ebben a forgatókönyvben azt:

* A virtuális gép beolvasása
* Hívás IP folyamat ellenőrzése
* Eredmények ellenőrzése

## <a name="log-in-with-armclient"></a>Jelentkezzen be ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>A virtuális gép beolvasása

A következő parancsfájl tooreturn hello egy virtuális gép futtatásához. a következő kódot kell hello változók értékeit hello:

* **a subscriptionId** -előfizetési azonosító toouse hello.
* **resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

hello szükséges adatokat a rendszer hello azonosítóját a hello típusa `Microsoft.Compute/virtualMachines`. hello eredményeket kell lennie a következő példakód hasonló toohello:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Hívás IP folyamat ellenőrzése

hello alábbi példa létrehoz egy kérelem tooverify hello forgalmat egy adott virtuális géphez. hello választ ad vissza, ha hello forgalom engedélyezve van, vagy ha hello forgalom megtagadva. Ha a forgalom is adja vissza, milyen szabály blokkok hello forgalom megtagadva.

> [!NOTE]
> IP-adatfolyam ellenőrizze, hogy megköveteli, hogy a virtuális gép erőforrásához hello le van foglalva.

hello parancsfájlhoz szükséges hello erőforrás-azonosítót a virtuális gépek és a hálózati kártya hello virtuális gépen. Ezek az értékek kimeneti megelőző hello által biztosított.

> [!Important]
> Az összes hálózati figyelő REST hívások hello hello kérelem URI hello egy hello hálózati figyelőt példányát, nem hello olyan erőforrásokat tartalmaz, a hello diagnosztikai műveleteket hajt végre, az erőforráscsoport neve.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-hello-results"></a>Hello eredmények ismertetése

hello válasz vissza azt ismerteti, hogy hello forgalom engedélyezett vagy megtagadott. hello választ egyet a következő példák hello néz:

**Engedélyezett**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Megtagadva**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Következő lépések

Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn további hálózati biztonsági csoportok.












