---
title: "aaaQoS ExpressRoute követelményei |} Microsoft Docs"
description: "Ez az oldal ExpressRoute-kapcsolatcsoportok QoS-konfigurálásának és -kezelésének részletes követelményeit ismerteti."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a>Az ExpressRoute QoS-követelményei
A Skype Vállalati verzió különböző számítási feladatokat tartalmaz, amelyek különböző QoS-kezelést igényelnek. Ha tooconsume szolgáltatások ExpressRoute keresztül, meg kell felelnie toohello követelmények az alábbiakban.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> A QoS követelményeit toohello Microsoft társviszony-létesítés csak. a hálózati forgalom az Azure nyilvános társviszony és Azure magánhálózati társviszony-létesítés kapott hello DSCP értékkel alaphelyzetbe állítása too0 lesz. 
> 
> 

hello következő táblázat felsorolja a DSCP-jelölések Skype vállalati használja. Tekintse meg a túl[QoS kezelése a Skype vállalati verzió](https://technet.microsoft.com/library/gg405409.aspx) további információt.

| **Forgalomosztály** | **Kezelés (DSCP-jelölés)** | **Skype Vállalati verzió számítási feladata** |
| --- | --- | --- |
| **Hang** |EF (46) |Skype- / Lync-hang |
| **Interaktív** |AF41 (34) |Videó, VBSS |
| AF21 (18) |Alkalmazásmegosztás | |
| **Alapértelmezett** |AF11 (10) |Fájlátvitel |
| CS0 (0) |Bármi más | |

* Hello munkaterhelések besorolásához kell, és jelölje meg hello jobb DSCP értékkel. Követve hello [Itt](https://technet.microsoft.com/library/gg405409.aspx) hogyan tooset DSCP megjelölés a hálózaton.
* Több QoS várakozási sort kell konfigurálnia és támogatnia a hálózaton belül. Hangalámondás egy önálló osztály legyen, és RFC 3246 megadott hello EF-kezelés. 
* Dönthet úgy is hello queuing mechanizmus, torlódás szabályzat és sávszélesség-lefoglalást egyes forgalmi osztálynak. De hello DSCP-jelölés a Skype vállalati munkaterhelések esetén meg kell őrizni. Ha használ, fent nem említett DSCP-jelölés például AF31 (26), meg kell írniuk a DSCP érték too0 hello csomagok tooMicrosoft elküldése előtt. A Microsoft csak akkor csomagok DSCP érték hello táblázat felett megjelenő hello jelölésű küldi el. 

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a toohello követelményei [útválasztás](expressroute-routing.md) és [NAT](expressroute-nat.md).
* Tekintse meg a következő hello hivatkozásait tooconfigure az ExpressRoute-kapcsolatot.
  
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-classic.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-classic.md)
  * [Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md)

