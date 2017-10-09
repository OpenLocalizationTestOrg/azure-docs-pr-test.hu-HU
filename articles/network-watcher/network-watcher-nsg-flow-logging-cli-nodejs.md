---
title: "aaaManage hálózati biztonsági csoport Flow naplózza az Azure hálózati figyelőt - Azure CLI 1.0 |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan hálózati biztonsági csoport Flow toomanage bejelentkezik-e az Azure CLI 1.0 Azure hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2535eea665a99cffe7569a8d976333435f946a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a>Hálózati biztonsági csoport Flow naplók konfigurálása az Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra. A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.

Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux. Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.

## <a name="register-insights-provider"></a>Elemzések szolgáltató regisztrálása

Ahhoz, hogy a folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie. Ha nem biztos benne, hogy hello **Microsoft.Insights** szolgáltató regisztrált, futtatási hello következő parancsfájlt.

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a>Hálózati biztonsági csoport Flow naplók engedélyezése

hello parancs tooenable folyamata naplók hello a következő példa látható:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a>Tiltsa le a hálózati biztonsági csoport folyamata naplók

A következő példa toodisable folyamata naplók használata hello:

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a>A folyamat napló letöltése

a folyamat napló hello tárolási helye a létrehozásakor van definiálva. Egy eszköz tooaccess ezen folyamat mentett naplók tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/

Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Hello struktúra hello napló információt [hálózati biztonsági csoport Flow napló áttekintése](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
