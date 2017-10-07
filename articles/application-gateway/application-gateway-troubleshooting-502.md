---
title: "aaaTroubleshoot Azure Application Gateway hibás átjáró (502-es) hibák |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot alkalmazás átjáró 502-es hiba"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Az Alkalmazásátjáró hibás átjáró hibák elhárítása

Ismerje meg, hogyan tootroubleshoot hibás átjáró (502) hibák fogadja az Alkalmazásátjáró használatakor.

## <a name="overview"></a>Áttekintés

Miután olyan átjárót, hello a hibákat, a felhasználók szembesülhetnek egyik "kiszolgálóhiba: 502 - webkiszolgáló érvénytelen választ kapott miközben átjáróként vagy proxyként működött". Ez a hiba akkor fordulhat elő miatt toohello következő fő oka:

* Az Azure Application Gateway [háttér-címkészlet nincs konfigurálva üres](#empty-backendaddresspool).
* Hello virtuális gépek vagy a példányok egyike [megfelelő Virtuálisgép-méretezési csoportban](#unhealthy-instances-in-backendaddresspool).
* Háttér-alapú virtuális gépek és Virtuálisgép-méretezési csoport példányai vannak [nem válaszol toohello alapértelmezett állapotmintáihoz](#problems-with-default-health-probe.md).
* Érvénytelen vagy nem megfelelő [egyéni állapotteljesítmény konfigurációjának](#problems-with-custom-health-probe.md).
* [Időtúllépés vagy csatlakozási problémák kérést](#request-time-out) a felhasználói kérelmek.

## <a name="empty-backendaddresspool"></a>Üres BackendAddressPool

### <a name="cause"></a>Ok

Ha hello Alkalmazásátjáró virtuális gép vagy Virtuálisgép-méretezési csoportban konfigurált hello háttér címkészletet, minden felhasználói kérelem nem irányíthatja, és a hibás átjáró hibát jelez.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy hello háttér-címkészlet nem üres. Ezt megteheti, vagy PowerShell, a parancssori felületen vagy a portálon.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

parancsmag megelőző hello hello kimenete nem üres háttér címkészletet kell tartalmaznia. Az alábbiakban látható egy példa ahol két rendelkezik visszaadott amely háttérkiszolgáló virtuális gépek teljes Tartománynevét vagy IP-címmel vannak konfigurálva. üzembe helyezési állapotát hello BackendAddressPool hello kell lennie "sikeres".

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>Nem megfelelő állapotú példány BackendAddressPool

### <a name="cause"></a>Ok

Ha BackendAddressPool példányainak hello sérült állapotban, majd az Alkalmazásátjáró nem kellene bármely háttér-tooroute felhasználói kérés. Hello eset is ennek oka lehet, ha a háttér-példányok kifogástalan állapotú, de nem rendelkezik a szükséges hello központilag telepített alkalmazás.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy hello példányok kifogástalan és hello alkalmazás megfelelően van konfigurálva. Jelölőnégyzet hello háttér-példányok esetén képes toorespond tooa ping egy másik virtuális gépről a hello ugyanazt a virtuális hálózatot. Ha be van állítva egy nyilvános végpontot, gondoskodjon arról, hogy a böngésző kérelem toohello webalkalmazás használható.

## <a name="problems-with-default-health-probe"></a>Alapértelmezett állapotmintáihoz kapcsolatos problémák

### <a name="cause"></a>Ok

502 hibákat is lehet, hogy az alapértelmezett állapotmintáihoz hello gyakori mutatók nem tud tooreach háttér virtuális gépek van. Ha egy alkalmazás átjárópéldány ki van építve, automatikusan beállítja az alapértelmezett állapot-mintavételi tooeach BackendAddressPool hello BackendHttpSetting annak a tulajdonságai. Nincs felhasználói bevitel szükséges tooset Ez a Hálózatfigyelő. Terheléselosztási szabály konfigurálásakor társítás van tenni egy BackendHttpSetting és BackendAddressPool között. Egy alapértelmezett mintavételt ezek társítások és Alkalmazásátjáró kezdeményezi a rendszeres ellenőrzés kapcsolat tooeach példánya a hello BackendAddressPool hello BackendHttpSetting elemben megadott hello porton van konfigurálva. Következő táblázatban hello alapértelmezett állapotmintáihoz társított hello értékek.

| Mintavételi tulajdonság | Érték | Leírás |
| --- | --- | --- |
| A mintavételi URL-címe |http://127.0.0.1/ |URL-címe |
| időköz |30 |Mintavételi időköz másodpercben |
| Időtúllépés |30 |Mintavételi időkorlátja másodpercben |
| Sérült küszöbérték |3 |Mintavételi újrapróbálkozások maximális számát. hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték. |

### <a name="solution"></a>Megoldás

* Győződjön meg arról, hogy egy alapértelmezett webhelyet van konfigurálva, és a 127.0.0.1 figyel.
* Ha BackendHttpSetting határozza meg a 80-as portra, hello alapértelmezett webhely adott porton konfigurált toolisten kell lennie.
* hello hívás toohttp://127.0.0.1:port kell visszaadnia egy 200-as HTTP-eredménykódja. Ez a visszaadandó hello 30 másodpercet időkorláton belül.
* Győződjön meg arról, hogy a konfigurált port nyitva-e, hogy nincsenek-e a tűzfalszabályok vagy Azure hálózati biztonsági csoportok, amelyek konfigurált hello port bejövő vagy kimenő forgalmát blokkolja.
* Az Azure klasszikus virtuális gépek vagy a felhőalapú szolgáltatás FQDN vagy a nyilvános IP-cím használata esetén győződjön meg arról, hogy hello megfelelő [végpont](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) már meg van nyitva.
* Ha hello VM keresztül Azure Resource Manager konfigurálva van és hello Alkalmazásátjáró telepítési helyét, VNet kívül [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) be kell állítani hello tooallow hozzáférés port szükséges.

## <a name="problems-with-custom-health-probe"></a>Egyéni állapotmintáihoz kapcsolatos problémák

### <a name="cause"></a>Ok

Egyéni állapotteljesítmény további rugalmasságával toohello alapértelmezett viselkedése probing engedélyezése. Egyéni mintavételt használatakor felhasználók beállíthatják hello mintavétel gyakoriságát, hello URL-címet, és elérési útja tootest és hány sikertelen válaszok tooaccept hello háttér-készlet példány sérültként való megjelölése előtt. a következő további tulajdonságok hello kerülnek.

| Mintavételi tulajdonság | Leírás |
| --- | --- |
| Név |Hello mintavétel neve. Ez a név foglalt toorefer toohello mintavételi a háttér-HTTP-beállítások. |
| Protokoll |Toosend hello mintavételi használt protokoll. hello mintavételi hello háttér HTTP-beállítások között megadott hello protokollt használja |
| Gazdagép |Gazdagép neve toosend hello mintavétel. Akkor alkalmazható, csak akkor, ha több hely úgy van konfigurálva, az alkalmazás-átjárón. Ez eltér a virtuális gép állomásnevét. |
| Elérési út |Hello mintavételi relatív elérési út. hello érvényes elérési út elindítja a "/". hello mintavételi túl küldött\<protokoll\>://\<állomás\>:\<port\>\<elérési útja\> |
| időköz |Mintavételi időköz másodpercben. Ez a két egymást követő mintavételek menüpontban hello időközét. |
| Időtúllépés |Mintavételi időtúllépés másodpercben. Ha az időkorláton belül nem érkezik meg egy érvényes válasz, hello mintavételi hibás jelölést. |
| Sérült küszöbérték |Mintavételi újrapróbálkozások maximális számát. hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték. |

### <a name="solution"></a>Megoldás

Ellenőrizze, hogy egyéni állapot-mintavételi megfelelően van konfigurálva, tábla megelőző hello hello. Továbbá előző toohello hibaelhárítási lépések, bizonyosodjon hello következő:

* Győződjön meg arról, hogy hello mintavételi helyesen van megadva a visszamenőleges hello [útmutató](application-gateway-create-probe-ps.md).
* Ha Alkalmazásátjáró egy adott hely van beállítva, alapértelmezett hello állomás által nevét kell megadni a "127.0.0.1", kivéve, ha az egyéni tesztműveleti egyéb konfigurálni.
* Győződjön meg arról, hogy a hívás toohttp: / /\<állomás\>:\<port\>\<elérési\> 200-as HTTP eredmény kódot ad vissza.
* Ellenőrizze, hogy időköz, időtúllépési és UnhealtyThreshold belül hello elfogadható tartományban.
* Egy HTTPS-kapcsolaton keresztül mintavételi, győződjön meg arról, hogy hello háttérkiszolgáló nem igényel SNI hello háttérkiszolgálón magát a fallback tanúsítvány konfigurálásával. 
* Gondoskodjon arról, hogy időköz, időtúllépési és UnhealtyThreshold belül hello elfogadható tartományban.

## <a name="request-time-out"></a>Kérelem időtúllépése

### <a name="cause"></a>Ok

Amikor egy felhasználói kérelem érkezik, az Application Gateway hello konfigurált szabályok toohello kérelem vonatkozik, és továbbítja azt tooa háttér-készlet példány. A konfigurálható időköz hello háttér-példányból választ vár. Alapértelmezés szerint ez az időtartam alatt van **30 másodperces**. Ha a Alkalmazásátjáró nem kapott választ háttéralkalmazás ezen időszak, felhasználói kérelem 502 hiba jelenik meg.

### <a name="solution"></a>Megoldás

Alkalmazásátjáró lehetővé teszi a felhasználók tooconfigure keresztül BackendHttpSetting, lehet, majd toodifferent készletek alkalmazza ezt a beállítást. Különböző háttér-készletek lehet különböző BackendHttpSetting és konfigurált ezért különböző kérelem túllépi az időkorlátot.

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a>Következő lépések

Ha hello előző lépések nem hello probléma megoldásához nyissa meg a [támogatja a jegy](https://azure.microsoft.com/support/options/).

