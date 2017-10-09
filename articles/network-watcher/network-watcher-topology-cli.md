---
title: "aaaView Azure hálózati figyelőt topológia - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure CLI tooquery a hálózati topológia."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: afa7e7dd844ecb2ab4c616ba99fa0a433f1e4ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a>Az Azure parancssori felület hálózati figyelőt topológia megtekintése

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

hello topológia szolgáltatása hálózati figyelőt hello hálózati erőforrások egy előfizetésben vizuális ábrázolását biztosítja. Hello portal Ez a képi megjelenítés áll rendelkezésre tooyou automatikusan. hello információk mögött hello topológia e nézetében hello portálon PowerShell kérhető.
E képesség révén hello topológiainfomációja rugalmasabb hello adatok más eszközök toobuild kimenő hello képi megjelenítés által feldolgozottként.

Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux. Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.

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

Ebben az esetben használhatja az hello `network watcher topology` parancsmag tooretrieve hello topológiainfomációja. Van még egy cikk hogyan túl[beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.

## <a name="retrieve-topology"></a>Topológia beolvasása

Hello `network watcher topology` parancsmag beolvassa a megadott erőforráscsoport hello topológia. Adja hozzá a hello argumentum "--json" tooview hello oput json formátumban

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>Results (Eredmények)

hello eredményének rendelkezik tulajdonsággal "Források" név, hello json adott válasz törzsének hello tartalmazó `network watcher topology` parancsmag.  hello válasz hello erőforrások hello hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) tartalmaz.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>Következő lépések

További tudnivalók hello biztonsági szabályokat alkalmazott tooyour hálózati erőforrások ellátogatva [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)
