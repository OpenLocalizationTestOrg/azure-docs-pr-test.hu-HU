---
title: "aaaView Azure hálózati figyelőt topológia - REST API |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse REST API tooquery a hálózati topológia."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a>REST API-t a hálózati figyelőt topológia megtekintése

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

hello topológia szolgáltatása hálózati figyelőt hello hálózati erőforrások egy előfizetésben vizuális ábrázolását biztosítja. Hello portal Ez a képi megjelenítés áll rendelkezésre tooyou automatikusan. hello információk mögött hello topológia e nézetében hello portálon PowerShell kérhető.
E képesség révén hello topológiainfomációja rugalmasabb hello adatok más eszközök toobuild kimenő hello képi megjelenítés által feldolgozottként.

hello összekapcsolása a két van modellezve.

- **Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz
- **Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek

hello következő lista rendszer hello topológia REST API lekérdezésekor visszaadott tulajdonságait.

* **név** – hello hello erőforrás neve
* **azonosító** -hello hello erőforrás URI azonosítóját.
* **hely** -hello helyre, ahol hello erőforrás létezik-e.
* **társítások** -társítások toohello listája hivatkozott objektum.
    * **név** -hello hello neve hivatkozott erőforrás.
    * **resourceId** -hello resourceId hello hello társítás hivatkozott hello erőforrás URI azonosítóját.
    * **associationType** -Ez az érték hivatkozik hello gyermekobjektum és hello szülő hello kapcsolatát. Érvényes értékek a következők **Contains** vagy **társított**.

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben hello topológia adatainak beolvasása. ARMclient használt toocall hello REST API használatával PowerShell. ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.

## <a name="log-in-with-armclient"></a>Jelentkezzen be ARMClient

Jelentkezzen be a Azure hitelesítő adataival tooarmclient.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Topológia beolvasása

a következő példa hello hello topológia kér hello REST API-t.  hello példája paraméteres tooallow létrehozása, például a rugalmasságot biztosít.  Cserélje le az összes érték \< \> körülvevő őket.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

válasz a következő hello, a rövidített válasz eredményül, ha például egy erőforráscsoport-topológiát beolvasása:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)

