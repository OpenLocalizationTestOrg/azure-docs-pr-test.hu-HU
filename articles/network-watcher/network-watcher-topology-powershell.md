---
title: "aaaView Azure hálózati figyelőt topológia - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse PowerShell tooquery a hálózati topológia."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2bc0ecf5baa81a68be53f55c74f362a7bc97116f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a>A PowerShell-lel hálózati figyelőt topológia megtekintése

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

Ebben az esetben használhatja az hello `Get-AzureRmNetworkWatcherTopology` parancsmag tooretrieve hello topológiainfomációja. Van még egy cikk hogyan túl[beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.

## <a name="retrieve-network-watcher"></a>Hálózati figyelőt beolvasása

hello első lépéseként tooretrieve hello hálózati figyelőt példány. Hello `$networkWatcher` változó átadása toohello `Get-AzureRmNetworkWatcherTopology` parancsmag.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a>Topológia beolvasása

Hello `Get-AzureRmNetworkWatcherTopology` parancsmag beolvassa a megadott erőforráscsoport hello topológia.

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a>Results (Eredmények)

hello eredményének rendelkezik tulajdonsággal "Források" név, hello json adott válasz törzsének hello tartalmazó `Get-AzureRmNetworkWatcherTopology` parancsmag.  hello válasz hello erőforrások hello hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) tartalmaz.

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)


