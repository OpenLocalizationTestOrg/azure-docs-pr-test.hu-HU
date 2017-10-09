---
title: "Következő Ugrás az Azure hálózati figyelő következő ugrásaként - REST aaaFind |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan milyen hello a következő ugrás típusa, és IP-cím használatával a következő ugrás használatával hello Azure REST API-n található"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ, amely a következőkre hálózati figyelőt Azure REST API használatával

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Az Azure REST API-n](network-watcher-check-next-hop-rest.md)

Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet. Ez a funkció akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.

## <a name="before-you-begin"></a>Előkészületek

ARMclient használt toocall hello REST API használatával PowerShell. ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt. toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).

Ebben a forgatókönyvben a tartalma:

* Hello a következő ugrás a virtuális gép beolvasása.

## <a name="log-in-with-armclient"></a>Jelentkezzen be ARMClient

Jelentkezzen be a Azure hitelesítő adataival tooarmclient.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>A virtuális gép beolvasása

A következő parancsfájl tooreturn hello egy virtuális gép futtatásához. Ezek az információk szükségesek a következő ugrás futtatásához.

a következő kód hello értékeket a következő változók hello szüksége van:

- **a subscriptionId** -előfizetési azonosító toouse hello.
- **resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Hello következő kimeneti, hello virtuális gép hello azonosítója szerepel a következő példa hello:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Következő ugrás beolvasása

Hello authorization fejlécet létrehozása után lekérhető hello a következő ugrás a virtuális gépről. hello következő értékek cserélni hello kód példa toowork.

> [!Important]
> Hálózati figyelő REST API-hívások hello hello kérelem URI hello hálózati figyelőt nem hello olyan erőforrásokat tartalmaz, a hello diagnosztikai műveleteket hajt végre hello erőforráscsoport erőforráscsoport-név.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.

## <a name="results"></a>Results (Eredmények)

hello következő kódrészlettel példája hello kimeneti kapott. hello eredmények tartalmazzák a következő értékek hello:

* **nextHopType** -ezt az értéket hello a következő értékek egyike: Internet, VirtualAppliance, pedig, VnetLocal, HyperNetGateway vagy None.
* **nextHopIpAddress** -hello hello következő ugrás IP-címét.
* **routeTableId** – hello értéke, vagy egy URI-jának hello útvonal társított hello útvonaltábla, vagy ha a felhasználó által definiált nem útvonal meghatározott hello érték *Rendszerútvonal* adja vissza.

Az alábbiakban hello hello eredmények json formátumban.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Következő lépések

Kimenő hello a következő ugrás a virtuális gépek képesek toofind követően megtekintheti látogasson el a hálózati erőforrások biztonsági hello [biztonsági nézettel áttekintése](network-watcher-security-group-view-overview.md)














