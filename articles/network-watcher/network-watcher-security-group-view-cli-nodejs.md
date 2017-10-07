---
title: "aaaAnalyze hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - Azure CLI 1.0 |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure CLI 1.0 tooanalyze a virtuális gépek biztonsági a biztonsági csoport megtekintése."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a>A virtuális gép biztonsági a biztonsági csoport nézet segítségével az Azure CLI 1.0 elemzése

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok, amelyek a virtuális gép alkalmazott tooa adja vissza. Ez a funkció hasznos tooaudit és diagnosztizálhatja a hálózati biztonsági csoportok és a virtuális gépek tooensure forgalma konfigurált szabályok folyamatban van megfelelően engedélyez vagy tilt. Ebben a cikkben megmutatjuk, hogyan tooretrieve hello konfigurálva és hatékony biztonsági szabályok tooa virtuális gépet az Azure parancssori felület használatával

Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux. Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv lekéri a konfigurált hello és a hatékony biztonsági szabályok egy adott virtuális géphez.

## <a name="get-a-vm"></a>A virtuális gép beolvasása

A virtuális gép szükség toorun hello `vm list` parancsmag. hello következő parancs kilistázza hello virtuális machinese erőforráscsoportban.

```azurecli
azure vm list -g resourceGroupName
```

Miután eldöntötte, hello virtuális gép, használhatja a hello `vm show` parancsmag tooget az erőforrás-azonosítót:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Biztonsági csoport nézet beolvasása

hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye. Hozzáadását hello "--json" jelző formázza hello eredmények json-ban.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a>Hello eredmények megtekintése

hello következő példa egy rövidített választ hello eredményt adott vissza. hello eredményeket jelenít meg minden hello hatékony és alkalmazott biztonsági szabályokat csoportok bontásban hello virtuális gépen **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Következő lépések

Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) toolearn hogyan hálózati biztonsági csoportok tooautomate érvényesítése.

További tudnivalók hello biztonsági szabályokat alkalmazott tooyour hálózati erőforrások ellátogatva [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)
