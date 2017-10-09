---
title: "aaaAutomate Azure hálózati figyelő biztonsági csoport megtekintése naplózás NSG |} Microsoft Docs"
description: "Ezen a lapon útmutatás tooconfigure naplózását a hálózati biztonsági csoport"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Automatizálható a NSG naplózás Azure hálózati figyelő biztonsági csoport megtekintése

Az ügyfelek gyakran kihívással hello kérdéssel hello biztonsági állapotát az infrastruktúra ellenőrzésére. Ez a kérdés ugyanolyan helyzetet teremt, a virtuális gépek Azure-ban. Fontos fontos toohave hasonló biztonsági profil alapján hello hálózati biztonsági csoport (NSG) szabálya. Biztonsági csoport megtekintése hello használ, most olvashatók és szabálya tooa belül egy NSG VM hello listája. Egy arany NSG biztonsági profil meghatározásához és biztonsági csoport megtekintése heti ütemben történik kezdeményezzen és hasonlítsa össze a hello kimeneti arany toohello profil és -jelentés létrehozása. Így azonosíthatja a könnyű összes hello virtuális gépek, amelyek nem felelnek meg a biztonsági profil előírt toohello.

Ha nem ismeri a hálózati biztonsági csoportokkal, látogasson el a [hálózati biztonsági – áttekintés](../virtual-network/virtual-networks-nsg.md)

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben egy ismert helyes alapterv toohello biztonsági csoport összehasonlítja a virtuális gép eredményének megtekintéséhez.

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv hello biztonsági csoport megtekintése a virtuális gép lekérdezi.

Ebben a forgatókönyvben a tartalma:

- Egy ismert helyes szabálykészletben beolvasása
- Rest API-hoz egy virtuális gép beolvasása
- A virtuális gép biztonsági csoport nézet beolvasása
- Értékelje ki a válasz

## <a name="retrieve-rule-set"></a>Szabály beolvasása

Ez a példa első lépéseként hello együtt egy meglévő toowork. hello alábbi példa néhány hello használata meglévő hálózati biztonsági csoport kinyert json `Get-AzureRmNetworkSecurityGroup` parancsmag, amely ehhez a példához hello alapjául szolgál.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-toopowershell-objects"></a>Alakítsa át a szabály beállított tooPowerShell objektumok

Ebben a lépésben egy json-fájl, amely a hálózati biztonsági csoport hello ehhez a példához várt toobe hello szabályokat a korábban létrehozott olvassák azt.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Hálózati figyelőt beolvasása

hello tovább tooretrieve hello hálózati figyelőt példány. Hello `$networkWatcher` változó átadása toohello `AzureRmNetworkWatcherSecurityGroupView` parancsmag.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>A virtuális gép beolvasása

A virtuális gép szükség toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` parancsmag ellen. a következő példa hello lekérdezi a VM-objektumhoz.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Biztonsági csoport nézet beolvasása

hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye. Az eredménye, hogy a korábban bemutatott összehasonlított toohello "eredeti" json.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a>Hello eredmények elemzése

hello válasz hálózati illesztők szerint vannak csoportosítva. különböző típusú hello visszaadott szabályok érvényben, és az alapértelmezett biztonsági szabályokat. hello eredmény további oszlanak meg az alkalmazásának módját, egy alhálózatot vagy egy virtuális hálózati adaptert.

hello következő PowerShell-parancsfájl összehasonlítja hello eredményeit hello biztonsági csoport megtekintése tooan meglévő kimenete egy NSG. hello alábbi példa: hogyan hello eredmények is össze kell hasonlítani egy egyszerű példa `Compare-Object` parancsmag.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

a következő példa hello hello eredménye. Láthatja, hogy két szereplő először szabálykészleten hello hello szabályok nem volt jelen hello szemben.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Következő lépések

Ha a beállítások módosítása, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek a szóban forgó tootrack.













