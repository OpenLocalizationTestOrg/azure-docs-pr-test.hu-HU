---
title: "aaaAnalyze hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure CLI 2.0 tooanalyze a virtuális gépek biztonsági a biztonsági csoport megtekintése."
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
ms.openlocfilehash: 31a4cd628f54d7548f495251fd275f099e79a060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a>A biztonsági csoport megtekintése az Azure CLI 2.0 verziót használja a virtuális gép biztonsági elemzése

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok, amelyek a virtuális gép alkalmazott tooa adja vissza. Ez a funkció hasznos tooaudit és diagnosztizálhatja a hálózati biztonsági csoportok és a virtuális gépek tooensure forgalma konfigurált szabályok folyamatban van megfelelően engedélyez vagy tilt. Ebben a cikkben megmutatjuk, hogyan tooretrieve hello konfigurálva és hatékony biztonsági szabályok tooa virtuális gépet az Azure parancssori felület használatával


Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv lekéri a konfigurált hello és a hatékony biztonsági szabályok egy adott virtuális géphez.

## <a name="get-a-vm"></a>A virtuális gép beolvasása

A virtuális gép szükség toorun hello `vm list` parancsmag. hello következő parancs kilistázza hello virtuális gépek erőforráscsoportban.

```azurecli
az vm list -resource-group resourceGroupName
```

Miután eldöntötte, hello virtuális gép, használhatja a hello `vm show` parancsmag tooget az erőforrás-azonosítót:

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Biztonsági csoport nézet beolvasása

hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye.

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-hello-results"></a>Hello eredmények megtekintése

hello következő példa egy rövidített választ hello eredményt adott vissza. hello eredményeket jelenít meg minden hello hatékony és alkalmazott biztonsági szabályokat csoportok bontásban hello virtuális gépen **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a>Következő lépések

Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) toolearn hogyan hálózati biztonsági csoportok tooautomate érvényesítése.

További tudnivalók hello biztonsági szabályokat alkalmazott tooyour hálózati erőforrások ellátogatva [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)
