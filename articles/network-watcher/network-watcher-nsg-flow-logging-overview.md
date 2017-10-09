---
title: "a hálózati biztonsági csoportok az Azure hálózati figyelőt aaaIntroduction tooflow naplózási |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse NSG folyamata naplózza az Azure hálózati figyelőt szolgáltatása"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: da85e946147b14717144cb47d1c742057c6dfa24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooflow-logging-for-network-security-groups"></a>Hálózati biztonsági csoportok tooflow naplózását bemutatása

Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra. A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.

![Attribútumfolyam naplók – áttekintés][1]

Attribútumfolyam naplózza a cél hálózati biztonsági csoportok, amíg azok nem jelennek meg hello ugyanaz, mint a hello további naplófájlokat. Csak egy tárfiók és a következő hello naplózási elérési út belül folyamata naplófájljainak tárolása a hello a következő példában látható módon:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

hello ugyanaz, mint a többi naplófájlt adatmegőrzési alkalmazni a naplókat tooflow. Naplók esetében 1 nap too365 nap beállítható adatmegőrzési rendelkezik. Ha nincs megadva adatmegőrzési, hello naplók végtelen karbantartása.

## <a name="log-file"></a>Naplófájl

Attribútumfolyam naplók több tulajdonságokkal rendelkezik. hello alábbi lista a következő hello tulajdonságok belül hello NSG folyamata napló visszaadott lista tartalmazza:

* **idő** – hello esemény naplózásának ideje
* **Rendszerazonosító** -hálózati biztonsági csoport erőforrás azonosítóját.
* **kategória** -hello kategória hello esemény, a rendszer mindig kell NetworkSecurityGroupFlowEvent
* **ResourceId** -erőforrás-azonosítót az hello NSG hello
* **operationName** -mindig NetworkSecurityGroupFlowEvents
* **Tulajdonságok** -hello folyamat tulajdonságainak gyűjteménye
    * **Verzió** -hello egymást követő esemény séma verziószáma
    * **adatfolyamok** -adatfolyamok gyűjteménye. Ez a tulajdonság különböző szabályok több bejegyzést tartalmaz
        * **a szabály** -mely hello adatfolyamok megjelennek szabály
            * **adatfolyamok** -adatfolyamok gyűjteménye
                * **Mac** -hello hello NIC hello VM, ahol hello folyamata gyűjtése történt a MAC-címe
                * **flowTuples** -hello folyamata rekordot vesszővel tagolt formátumú több tulajdonságait tartalmazó karakterlánc
                    * **Időbélyeg** -Ez az érték esetén időbélyegzőjét hello hello folyamat történt UNIX EPOCH formátumban
                    * **Forrás IP-címe** – hello forrás IP-címe
                    * **Cél IP** -hello cél IP-címe
                    * **Forrásport** – hello forrásport
                    * **Célport** -hello célport
                    * **Protokoll** -protokoll hello áramlás hello. Érvényes értékek a következők **T** TCP-hez és **U** UDP-hez
                    * **Forgalom bonyolódjon** -hello hello adatforgalmat irányát. Érvényes értékek a következők **I** bejövő és **O** a kimenő.
                    * **Forgalom** – akár forgalom lett engedélyez vagy tilt. Érvényes értékek a következők **A** engedélyezett, és **D** a megtagadva.


hello az alábbiakban látható egy példa egy folyamat napló. Az alábbi hello előző szakaszban leírt hello keresésitulajdonság-lista több rekordot látható. 

> [!NOTE]
> Hello flowTuples tulajdonság értékei egy vesszővel elválasztott listában.
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogy miként naplózza az tooenable folyamat ellátogatva [naplózás engedélyezésével Flow](network-watcher-nsg-flow-logging-portal.md).

Látogasson el az NSG-naplózás megismerése [elemzések a hálózati biztonsági csoportokkal (NSG-k) jelentkezzen](../virtual-network/virtual-network-nsg-manage-log.md).

Megtudhatja, ha a forgalom engedélyezett vagy megtagadott a virtuális gép ellátogatva [ellenőrizze forgalmat az IP-adatfolyam ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

