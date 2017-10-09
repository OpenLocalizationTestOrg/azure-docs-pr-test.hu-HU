---
title: "aaaInternal terheléselosztó áttekintése |} Microsoft Docs"
description: "Belső terheléselosztó és a szolgáltatások áttekintése. A terheléselosztó működéséről az Azure és a lehetséges forgatókönyvek tooconfigure belső végpont"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a>Belső terheléselosztó áttekintése

Hello Internet felé néző terheléselosztó eltérően hello belső terheléselosztón (ILB) irányítja a forgalmat csak tooresources hello felhőalapú szolgáltatás, vagy a VPN tooaccess hello Azure-infrastruktúra használatával belül. hello infrastruktúra korlátozza a hozzáférési toohello terhelésű virtuális IP-címek (VIP) egy felhőalapú szolgáltatás, vagy egy virtuális hálózatot, hogy soha nem fogja közvetlenül elérhetővé tooan Internet végpont. Ez lehetővé teszi, hogy a belső üzletági üzletági (LOB) alkalmazások toorun az Azure-ban, és elérhető az hello felhőben, vagy a helyszíni erőforrások.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Miért szükség lehet egy belső terheléselosztó

Az Azure belső betöltése terheléselosztás (ILB) belül egy felhőalapú szolgáltatás, vagy egy virtuális hálózat regionális hatókörbe telepített virtuális gépek közötti terheléselosztást biztosít. Hello használata és a virtuális hálózatokat regionális hatókörbe és konfigurálásával kapcsolatos további információkért lásd: [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) a hello Azure blog. Az affinitáscsoporthoz konfigurált meglévő virtuális hálózatok nem használhatják az ILB-t.

ILB lehetővé teszi, hogy a következő típusú terheléselosztás hello:

* Belül egy felhőalapú szolgáltatás, a virtuális gépek hello belüli virtuális gépek tooa készletből ugyanaz a felhőalapú szolgáltatás (lásd az 1. ábra).
* A virtuális hálózaton belül hello virtuális hálózati tooa hello belüli virtuális gépek halmaza virtuális gépekről származó ugyanaz a felhőalapú szolgáltatás, hello virtuális hálózat (lásd a 2. ábra).
* A létesítmények közötti virtuális hálózat, a helyszíni számítógépek tooa hello belüli virtuális gépek halmaza azonos a felhőalapú szolgáltatás a hello virtuális hálózat (lásd a 3. ábra).
* Internet felé néző, a többrétegű alkalmazások hello háttér-rétegek nincsenek internetre de igénylő terheléselosztás forgalom hello internetre réteg alapján.
* A LOB-alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett terheléselosztást. A számítógépet, amelynek a forgalmát terhelés hello készletét is beleértve a helyszíni kiszolgálók átgondolni.

## <a name="internet-facing-multi-tier-applications"></a>Többrétegű alkalmazások internetes

hello webes réteg Internet felé néző végpontok van az internetes ügyfeleket, és egy elosztott terhelésű készlet része. hello terheléselosztó osztja el a TCP port 443 (HTTPS) toohello webkiszolgálók webes ügyfelekről érkező bejövő forgalmat.

adatbázis-kiszolgálók hello hello webkiszolgálók használó tárolási ILB végpont mögött találhatók. Az adatbázis szolgáltatás az elosztott terhelésű végpont, mely akkor hello ILB set hello adatbázis kiszolgálója között elosztott terhelésű.

a következő kép bemutatja hello hello internetre irányuló többrétegű alkalmazást hello belül ugyanaz a felhőalapú szolgáltatás.

![Belső terheléselosztási egyetlen felhőszolgáltatás](./media/load-balancer-internal-overview/IC736321.png)

1. ábra – internetre irányuló többrétegű alkalmazást

Egy másik lehetséges Többrétegű alkalmazások használata telepítésekor hello ILB tooa másik felhőalapú szolgáltatást, mint a hello hello ILB egy fogyasztó hello szolgáltatást.

Felhőalapú szolgáltatások segítségével hello azonos virtuális hálózatban kell elérni toohello ILB végpont. a következő kép azt mutatja be, előtér-webkiszolgálók szerepelnek a hello adatbázis háttér-másik felhőalapú szolgáltatást, és segítségével hello hello ILB végpont hello belül azonos virtuális hálózatban.

![Felhőszolgáltatások közötti belső terheléselosztás](./media/load-balancer-internal-overview/IC744147.png)

2. ábra – másik felhőalapú szolgáltatást az előtér-kiszolgálók

## <a name="intranet-line-of-business-applications"></a>Intranetes üzletági alkalmazásokat

Hello a helyi hálózaton-ügyfelektől érkező forgalom beolvasása terhelésű hello beállítása a VPN-kapcsolat tooAzure hálózat használatával LOB-kiszolgálók között.

hello ügyfélszámítógép lesz hozzáférés tooan IP-cím pont toosite VPN-kapcsolattal Azure VPN szolgáltatásból. Lehetővé teszi a hello használata hello LOB-alkalmazás mögött hello ILB végpont.

![Pont toosite VPN-kapcsolattal belső terheléselosztás](./media/load-balancer-internal-overview/IC744148.png)

3. ábra - végpont hello LB mögött található, a LOB-alkalmazások

A hello LOB egy másik helyzet lehet toohave hely toosite VPN toohello virtuális hálózat ahol hello ILB végpont van konfigurálva. Ez lehetővé teszi a helyszíni hálózati forgalom irányított toobe toohello ILB végpontot.

![Hely toosite VPN használatával belső terheléselosztás](./media/load-balancer-internal-overview/IC744150.png)

4. ábra – a helyi hálózati forgalom irányított toohello ILB végpont

## <a name="limitations"></a>Korlátozások

Belső terheléselosztó nem támogatja a SNAT. Ez a dokumentum a hello környezetében SNAT tooport színleg forrás hálózati címfordítás hivatkozik.  Amikor egy virtuális Gépet a load balancer készlet kell tooreach hello megfelelő belső terheléselosztó tartozó előtérbeli IP-cím tooscenarios vonatkozik. Ez a forgatókönyv nem támogatott belső terheléselosztóhoz. Kapcsolódási hibák hello folyamata az elosztott terhelésű toohello hello folyamata szolgáltatásoktól VM történjen. Ilyen helyzetekben a proxy stílus terheléselosztó kell használnia.

## <a name="next-steps"></a>Következő lépések

[Azure Load Balancer Azure Resource Manager támogatása](load-balancer-arm.md)

[Ismerkedés az Internet felé néző terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[Ismerkedés a belső terheléselosztók konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
