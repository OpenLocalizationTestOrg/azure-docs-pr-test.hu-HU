---
title: "aaaMonitor műveleteket, eseményeket és NSG-ket számlálói |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable számlálók, eseményeket és NSG-ket műveleti naplózás"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: f16f1a0ad693028ee7aba21574b5c8ddfcd27096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Naplóelemzés hálózati biztonsági csoportokhoz

Az NSG-ket a diagnosztikai naplófájl kategóriák következő hello engedélyezéséhez:

* **Esemény:** mely NSG szabályok lettek alkalmazott tooVMs és a MAC-címe alapján példány szerepkörök bejegyzést tartalmaz. Ezek a szabályok hello állapota 60 másodpercenként gyűjti.
* **A szabály számláló:** Contains bejegyzéseket hányszor minden NSG-szabályok alkalmazott toodeny vagy -kezelési forgalom engedélyezése.

> [!NOTE]
> Diagnosztikai naplók esetén csak az NSG-k hello Azure Resource Manager üzembe helyezési modellben telepített érhetők el. Diagnosztikai naplózás hello klasszikus üzembe helyezési modellben telepített NSG-k nem engedélyezhető. Kra hello két modell, hivatkozhat hello [ismertetése Azure üzembe helyezési modellel](../resource-manager-deployment-model.md) cikk.

(Korábbi nevén naplózási és a műveleti naplókat) naplózása alapértelmezés szerint engedélyezve van a az NSG-k vagy az Azure-telepítés modell használatával létrehozott. milyen műveletek befejeződtek az NSG-ket hello műveletnaplóban, keresse meg a következő típusú erőforrások hello tartalmazó toodetermine: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Olvasási hello [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) cikk toolearn tevékenységi naplóit többet. 

## <a name="enable-diagnostic-logging"></a>Diagnosztikai naplózás engedélyezése

Diagnosztikai naplózás engedélyezni kell a *minden* NSG kívánt toocollect adatait. Hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a cikk azt ismerteti, ahol a diagnosztikai naplók küldhető. Ha nem rendelkezik egy meglévő NSG, teljes hello hello szükséges lépések [hálózati biztonsági csoport létrehozása](virtual-networks-create-nsg-arm-pportal.md) egy cikk toocreate. Diagnosztikai naplózás hello a következő módszerek bármelyikével NSG engedélyezéséhez:

### <a name="azure-portal"></a>Azure Portal

toouse hello portál tooenable naplózás, bejelentkezési toohello [portal](https://portal.azure.com). Kattintson a **további szolgáltatások**, majd írja be a *hálózati biztonsági csoportok*. Válassza ki a kívánt naplózása tooenable NSG hello. Hello utasítások nem számítási erőforrások hello [hello portálon diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk. Válassza ki **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, vagy mindkét kategóriák a naplók.

### <a name="powershell"></a>PowerShell

naplózás, toouse PowerShell tooenable hello hello utasításokat követve [PowerShell diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk. Értékelje ki a következő információ, mielőtt belép a parancs hello cikkből hello:

- Azt is meghatározhatja, hello érték toouse a hello `-ResourceId` hello következő [szöveg] megfelelő, majd a hello parancs bevitele lecserélésével paraméter `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. hello parancs kimenetében hello azonosító hasonlít túl*következő [előfizetés Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG neve]*.
- Ha csak a napló kategória toocollect adatait szeretné hozzáadni `-Categories [category]` hello parancs hello cikkben, ahol a kategóriát vagy a toohello végére *NetworkSecurityGroupEvent* vagy *NetworkSecurityGroupRuleCounter*. Ha nem adja meg hello `-Categories` paraméter, adatgyűjtés engedélyezve van, az kategóriák naplófájl.

### <a name="azure-command-line-interface-cli"></a>Azure parancssori felület (CLI)

toouse hello CLI tooenable naplózási, hello hello utasításait követve [parancssori felületen keresztül diagnosztikai naplók engedélyezése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) cikk. Értékelje ki a következő információ, mielőtt belép a parancs hello cikkből hello:

- Azt is meghatározhatja, hello érték toouse a hello `-ResourceId` hello következő [szöveg] megfelelő, majd a hello parancs bevitele lecserélésével paraméter `azure network nsg show [resource-group-name] [nsg-name]`. hello parancs kimenetében hello azonosító hasonlít túl*következő [előfizetés Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG neve]*.
- Ha csak a napló kategória toocollect adatait szeretné hozzáadni `-Categories [category]` hello parancs hello cikkben, ahol a kategóriát vagy a toohello végére *NetworkSecurityGroupEvent* vagy *NetworkSecurityGroupRuleCounter*. Ha nem adja meg hello `-Categories` paraméter, adatgyűjtés engedélyezve van, az kategóriák naplófájl.

## <a name="logged-data"></a>A naplózott adatok

JSON-formátumú adatot ír a mindkét naplókhoz. hello adatokat az egyes napló írása, a következő szakaszok hello szerepel:

### <a name="event-log"></a>Eseménynapló
A napló tartalmaz információkat, mely NSG vonatkozó szabályokat alkalmazott tooVMs, és a felhőalapú szolgáltatás szerepkörpéldányt beállítani, a MAC-címe alapján. a következő példa adatok hello minden esemény kerül:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>A szabály számláló napló

Ez a napló minden alkalmazott szabály tooresources információkat tartalmaz. hello alábbi példa a rendszer adatokat naplózza minden alkalommal, amikor egy szabály alkalmazásakor:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Megtekintése és elemzése a naplókat

toolearn hogyan tooview tevékenységet naplózni, olvassa el a hello [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk. toolearn hogyan tooview diagnosztikai naplózni, olvassa el a hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk. Ha diagnosztikai adatokat tooLog Analytics küldi el, használhatja a hello [Azure hálózati biztonsági csoport analytics](../log-analytics/log-analytics-azure-networking-analytics.md) továbbfejlesztett insights (előzetes verzió) felügyeleti megoldás. 
