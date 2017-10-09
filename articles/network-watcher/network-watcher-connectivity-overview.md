---
title: "az Azure hálózati figyelőt aaaIntroduction tooconnectivity ellenőrzése |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt kapcsolat funkció áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 52fc4547f167cea2992a046859dc0550d136e80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooconnectivity-check-in-azure-network-watcher"></a>Bevezetés az tooconnectivity ellenőrzéshez Azure hálózati figyelőt

hello kapcsolatot a hálózati figyelőt funkciójával hello funkció toocheck közvetlen TCP-kapcsolatot a virtuális gép tooa virtuális gép (VM), teljesen minősített tartománynevét (FQDN), URI, vagy IPv4-címet. Hálózati forgatókönyvek a következők összetett, végrehajtásuk hálózati biztonsági csoportok, tűzfalak, felhasználó által definiált útvonalak és az Azure által biztosított erőforrásokhoz. Összetett konfigurációk ellenőrizze a hibaelhárítási problémák kihívást. Hálózati figyelőt segít csökkenteni hello idő toofind és problémák észlelése. hello eredményének is betekintést egy hálózati probléma miatt tooa platform vagy felhasználó konfigurációs hiba lépett fel van-e. Kapcsolat ellenőrizhetők a [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), és [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`. A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Válasz

a következő táblázat hello vissza, ha a kapcsolat-ellenőrzés futása hello tulajdonságok láthatók.

|Tulajdonság  |Leírás  |
|---------|---------|
|ConnectionStatus     | hello állapota hello kapcsolat ellenőrzése. Lehetséges eredményei **elérhető** és **Unreachable**.        |
|AvgLatencyInMs     | Átlagos késleltetése ezredmásodpercben hello kapcsolat ellenőrzése során. (Csak jelenik meg, ha a jelölőnégyzet állapota érhető el)        |
|MinLatencyInMs     | Minimális késésre során hello kapcsolat ellenőrzése ezredmásodpercben. (Csak jelenik meg, ha a jelölőnégyzet állapota érhető el)        |
|MaxLatencyInMs     | Maximális késleltetés során hello kapcsolat ellenőrzése ezredmásodpercben. (Csak jelenik meg, ha a jelölőnégyzet állapota érhető el)        |
|ProbesSent     | Hello ellenőrzése során küldött mintavételt száma. Maximális érték 100.        |
|ProbesFailed     | Hello ellenőrzése sikertelen mintavételek menüpontban száma. Maximális érték 100.        |
|Az ugrásszám     | Ugrás a forrás toodestination Ugrás elérési úttal.        |
|[] Az útválasztók. Típusa     | Az erőforrás típusát. A lehetséges értékek: **forrás**, **VirtualAppliance**, **VnetLocal**, és **Internet**.        |
|[] Az útválasztók. Azonosítója | Hello Ugrás egyedi azonosítója.|
|[] Az útválasztók. Cím | Hello ugrás IP-címe.|
|[] Az útválasztók. ResourceId | Ha hello Ugrás az Azure erőforrás hello Ugrás ResourceID. Ha egy internetes erőforrást, van-e a ResourceID **Internet**. |
|[] Az útválasztók. NextHopIds | hello hello következő ugrás végrehajtott egyedi azonosítója.|
|[] Az útválasztók. Problémák | Hello ellenőrzése, hogy Ugrás során felmerült problémákat gyűjteménye. Ha nincs probléma, hello értéke üres.|
|[] Az útválasztók. [] Ad ki. Forrás | A hello aktuális hop, ahol probléma lépett fel. Lehetséges értékek:<br/> **Bejövő** -probléma van a hello előző Ugrás toohello aktuális Ugrás hello hivatkozásra<br/>**Kimenő** -probléma van a hello hivatkozásra a hello aktuális Ugrás toohello következő ugrás<br/>**Helyi** -hello aktuális ugrásnál van probléma.|
|[] Az útválasztók. [] Ad ki. Súlyosság: | hello problémát észlelt hello súlyossága. A lehetséges értékek: **hiba** és **figyelmeztetés**. |
|[] Az útválasztók. [] Ad ki. Típusa |a probléma található hello típusa. Lehetséges értékek: <br/>**PROCESSZOR**<br/>**Memória**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|[] Az útválasztók. [] Ad ki. A környezetben |Hello probléma található kapcsolatos részleteket.|
|[] Az útválasztók. [] Ad ki. [] .key környezetben |Hello kulcs-érték pár kulcsa vissza.|
|[] Az útválasztók. [] Ad ki. [] .value környezetben |Hello kulcs-érték pár értékét adja vissza.|

hello az alábbiakban látható egy példa található egy Ugrás a problémát.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Hiba típusa

hello kapcsolat ellenőrzése tartalék típusok hello kapcsolatról adja vissza. hello következő táblázat felsorolja típusú hello aktuális hibát adott vissza.

|Típus  |Leírás  |
|---------|---------|
|CPU     | Magas fokú Processzorhasználatot tapasztalható.       |
|Memory (Memória)     | Magas memóriahasználat.       |
|GuestFirewall     | Forgalom tooa a virtuális gép tűzfalbeállítások miatt le van tiltva.        |
|DNSResolution     | DNS-feloldás hello célcím nem sikerült.        |
|NetworkSecurityRule    | Forgalmát blokkolja egy NSG-szabály (szabály visszaadott)        |
|UserDefinedRoute|Forgalom megfelelő tooa felhasználó által definiált vagy rendszerútvonal megszakad. |

### <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooverify kapcsolat tooa erőforrás ellátogatva: [ellenőrizze a kapcsolatot az Azure hálózati figyelőt](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

