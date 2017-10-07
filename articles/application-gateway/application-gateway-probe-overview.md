---
title: "figyelési áttekintés az Azure Application Gateway aaaHealth |} Microsoft Docs"
description: "Figyelési képességek az Azure alkalmazás átjáró hello megismerése"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 5091d80394a354ff849ce7ccee8cc9d2fd0456db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Átjáró állapotfigyelő figyelési – áttekintés

Alapértelmezés szerint az Azure Application Gateway figyeli a hello állapotát a háttér-készletben erőforrásait, és automatikusan eltávolítja az összes erőforrás hello készlet megfelelő állapotúnak számít. Alkalmazásátjáró toomonitor hello sérült példányok folytatódik, és hozzáadja őket biztonsági toohello kifogástalan háttér-készlet, amennyiben azok elérhetővé válnak, és válaszoljon toohealth-vizsgálat. Alkalmazásátjáró küldi hello állapotfigyelő mintavételt a hello ugyanazt a portot definiált hello háttér HTTP-beállítások. Ez a konfiguráció biztosítja, hogy hello mintavételi azonos port, hogy az ügyfelek dolgozna tooconnect toohello háttér hello teszteli.

![alkalmazás átjáró mintavételi – példa][1]

Továbbá toousing alapértelmezett mintavételi állapotfigyelés, is testreszabhatja hello állapotfigyelő mintavételi toosuit az alkalmazás követelményeinek. Ebben a cikkben alapértelmezett és egyéni állapotteljesítmény tartoznak.

> [!NOTE]
> Ha Alkalmazásátjáró alhálózat egy NSG-t, porttartományok 65503-65534 meg kell nyitni bejövő forgalom hello Alkalmazásátjáró alhálózaton. Ezeket a portokat hello háttér állapotfigyelő API toowork szükségesek.

## <a name="default-health-probe"></a>Alapértelmezett állapotmintáihoz

Alkalmazásátjáró automatikusan meghatározza, hogy egy alapértelmezett állapotmintáihoz nem állít be egyéni mintavételi beállításra. hello viselkedésének figyelése elvégzése egy HTTP-kérelem toohello IP-címek hello háttér-készlet beállítása során. Az alapértelmezett mintavételt hello backendhttpsettings osztályhoz vannak konfigurálva, a HTTPS-hez, ha hello mintavételi használja HTTPS hello háttérkiszolgálókon jól tootest állapotát.

Például: konfigurálhatja az alkalmazás átjáró toouse háttérkiszolgálók A, B és C tooreceive HTTP hálózati forgalom 80-as porton. hello három kiszolgáló hello alapértelmezett állapotfigyelés tesztek 30 másodpercenként kifogástalan HTTP-választ. Rendelkezik a megfelelő HTTP-válasz egy [állapotkód](https://msdn.microsoft.com/library/aa287675.aspx) 200 és 399 között.

Hello alapértelmezett mintavétel-ellenőrzés sikertelen, A kiszolgáló, ha hello Alkalmazásátjáró automatikusan eltávolítja a háttér-készlet, és a hálózati forgalom leállítja toothis server továbbítására. hello alapértelmezett mintavétel egy 30 másodpercenként továbbra is fennáll toocheck kiszolgáló. A kiszolgáló válaszol sikeresen tooone kérelem érkező egy alapértelmezett állapotmintáihoz, amikor felveszik vissza megfelelő toohello háttér-készlet, valamint a forgalom megkezdődésekor toohello server újra.

### <a name="default-health-probe-settings"></a>Alapértelmezett állapotfigyelő mintavételi beállításai

| Mintavételi tulajdonság | Érték | Leírás |
| --- | --- | --- |
| A mintavételi URL-címe |http://127.0.0.1:\<port\>/ |URL-címe |
| időköz |30 |Mintavételi időköz másodpercben |
| Időtúllépés |30 |Mintavételi időkorlátja másodpercben |
| Sérült küszöbérték |3 |Mintavételi újrapróbálkozások maximális számát. hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték. |

> [!NOTE]
> hello port van hello azonos port hello háttér HTTP-beállításként.

hello alapértelmezett mintavétel ellenőrzi, hogy csak az http://127.0.0.1:\<port\> toodetermine állapotát. Ha tooconfigure hello állapotfigyelő mintavételi toogo tooa egyéni URL-címhez kell, vagy bármely egyéb beállítások módosítása, a következő lépéseket hello egyéni mintavételt kell használnia:

## <a name="custom-health-probe"></a>Egyéni állapotmintáihoz

Egyéni mintavételt toohave hello állapotfigyelés részletesebb szabályozhatják engedélyezése. Egyéni mintavételt használata esetén konfigurálhatja hello mintavétel gyakoriságát, hello URL-cím és az elérési út tootest, hogy hány sikertelen válaszok tooaccept hello háttér-készlet példány sérültként való megjelölése előtt.

### <a name="custom-health-probe-settings"></a>Egyéni állapotfigyelő mintavételi beállítások

hello következő tábla biztosít jelentésdefiníciókat hello tulajdonságának értéke egy egyéni állapotmintáihoz.

| Mintavételi tulajdonság | Leírás |
| --- | --- |
| Név |Hello mintavétel neve. Ez a név foglalt toorefer toohello mintavételi a háttér-HTTP-beállítások. |
| Protokoll |Toosend hello mintavételi használt protokoll. hello mintavételi hello háttér HTTP-beállítások között megadott hello protokollt használja |
| Gazdagép |Gazdagép neve toosend hello mintavétel. Alkalmazandó csak akkor, ha több hely van beállítva az alkalmazás-átjárón, ellenkező esetben használja a "127.0.0.1". Ez az érték eltér a virtuális gép állomásnevét. |
| Elérési út |Hello mintavételi relatív elérési út. hello érvényes elérési út elindítja a "/". |
| időköz |Mintavételi időköz másodpercben. Ez az érték két egymást követő mintavételek menüpontban hello időközét. |
| Időtúllépés |Mintavételi időtúllépés másodpercben. Ha az időkorláton belül nem érkezik meg egy érvényes válasz, hello mintavételi hibás jelölést.  |
| Sérült küszöbérték |Mintavételi újrapróbálkozások maximális számát. hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték. |

> [!IMPORTANT]
> Ha Alkalmazásátjáró egy adott hely van beállítva, alapértelmezett hello állomás által nevét kell megadni a "127.0.0.1", kivéve, ha az egyéni tesztműveleti egyéb konfigurálni.
> Hivatkozás egy egyéni mintavételt továbbítja a rendszer túl\<protokoll\>://\<állomás\>:\<port\>\<elérési\>. hello használt port lesz hello ugyanazt a portot, a hello háttér HTTP-beállításait.

## <a name="next-steps"></a>Következő lépések
Alkalmazásátjáró állapotfigyelés megismerését követően konfigurálhatja a [egyéni állapotmintáihoz](application-gateway-create-probe-portal.md) hello Azure-portálon az vagy egy [egyéni állapotmintáihoz](application-gateway-create-probe-ps.md) PowerShell és a hello Azure Resource Manager használatával üzembe helyezési modellben.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
