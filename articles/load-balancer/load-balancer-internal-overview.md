---
title: "Belső terheléselosztó áttekintése |} Microsoft Docs"
description: "Belső terheléselosztó és a szolgáltatások áttekintése. A terheléselosztó belső végpont konfigurálása az Azure és a lehetséges forgatókönyvek működése"
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
ms.openlocfilehash: d324aaf8ec2c8766d5cf11452158d14c19cba4d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="internal-load-balancer-overview"></a>Belső terheléselosztó áttekintése

A belső terheléselosztó (ILB) eltérően az internetre irányuló a terheléselosztóhoz, arra utasítja a forgalom csak az erőforráshoz, a felhőalapú szolgáltatás, vagy az Azure-infrastruktúra eléréséhez a VPN-kapcsolattal. Az infrastruktúra korlátozza a hozzáférést a terheléselosztó virtuális IP-címek (VIP) egy felhőalapú szolgáltatás, vagy egy virtuális hálózatot, hogy azok soha nem közvetlenül számára megjelenik egy Internet-végpontot. Ez lehetővé teszi a belső üzletági üzletági (LOB) alkalmazások futtatása az Azure-ban, és elérhető a felhő vagy a helyszíni erőforrások.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Miért szükség lehet egy belső terheléselosztó

Az Azure belső betöltése terheléselosztás (ILB) belül egy felhőalapú szolgáltatás, vagy egy virtuális hálózat regionális hatókörbe telepített virtuális gépek közötti terheléselosztást biztosít. A használat és a virtuális hálózatokat regionális hatókörbe és konfigurálásával kapcsolatos további információkért lásd: [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) az Azure blogján. Az affinitáscsoporthoz konfigurált meglévő virtuális hálózatok nem használhatják az ILB-t.

ILB lehetővé teszi, hogy a következő típusú terheléselosztási:

* Egy felhőalapú szolgáltatás, a virtuális gépek számára a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás belüli belül (lásd az 1. ábra).
* A virtuális hálózaton belül a virtuális hálózat a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás, a virtuális található virtuális gépekről származó (lásd a 2. ábra) hálózati.
* A létesítmények közötti virtuális hálózat a helyszíni számítógépek számára a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás, a virtuális belüli (lásd a 3. ábra) hálózati.
* Az Internet felé néző, a többrétegű alkalmazások, amelyben a háttér-rétegek nincsenek internetre irányuló, de szükséges hálózati terheléselosztást a forgalmat az Internet felé néző réteg alapján.
* A LOB-alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett terheléselosztást. Elosztott terhelésű többek között a helyszíni kiszolgálók a számítógépet, amelynek a forgalmát terhelés készletében.

## <a name="internet-facing-multi-tier-applications"></a>Többrétegű alkalmazások internetes

A webes réteg Internet felé néző végpontok van az internetes ügyfeleket, és egy elosztott terhelésű készlet része. A load balancer továbbítja a webkiszolgálók TCP port 443-as (HTTPS) webalkalmazás-ügyfelekről érkező bejövő forgalmat.

Az adatbázis-kiszolgálók a webkiszolgálók használó tárolási ILB végpont mögött találhatók. Az adatbázis szolgáltatás az elosztott terhelésű végpont, mely forgalom az adatbázis-kiszolgálóin ILB készletében betöltése.

A következő kép bemutatja, az internetre irányuló a többrétegű alkalmazást belül az azonos felhőalapú szolgáltatás.

![Belső terheléselosztási egyetlen felhőszolgáltatás](./media/load-balancer-internal-overview/IC736321.png)

1. ábra – internetre irányuló többrétegű alkalmazást

Egy másik lehetséges Többrétegű alkalmazások használata a ILB központi telepítési fel a Példánynak a szolgáltatás egy másik felhőalapú szolgáltatást.

Felhőszolgáltatások a azonos virtuális hálózaton keresztül hozzáférhet a Példánynak a végponthoz. A következő kép bemutatja az előtér-webkiszolgáló egy másik felhőalapú szolgáltatást, az adatbázis-háttér és használata a ILB végpontot a virtuális hálózaton belül vannak.

![Felhőszolgáltatások közötti belső terheléselosztás](./media/load-balancer-internal-overview/IC744147.png)

2. ábra – másik felhőalapú szolgáltatást az előtér-kiszolgálók

## <a name="intranet-line-of-business-applications"></a>Intranetes üzletági alkalmazásokat

A helyszíni hálózaton-ügyfelektől érkező forgalom beolvasása terhelésű közötti VPN-kapcsolat az Azure-hálózat használatával LOB-kiszolgálók készlete.

Az ügyfélszámítógép hozzáférhet a IP-cím pont közötti VPN használatával Azure VPN szolgáltatásból. A LOB-alkalmazások a ILB végpont mögött található engedélyezze a használatát.

![Belső terheléselosztás pont közötti VPN használatával](./media/load-balancer-internal-overview/IC744148.png)

3. ábra - végpont LB mögött található, a LOB-alkalmazások

Az üzleti egy másik forgatókönyve, hogy a webhelyek közötti VPN a virtuális hálózathoz, ahol a ILB végpont van konfigurálva. Ez lehetővé teszi a helyszíni hálózati forgalom átirányítását a ILB végpont.

![Belső terheléselosztás webhelyek közötti VPN használatával](./media/load-balancer-internal-overview/IC744150.png)

4. ábra – a helyi hálózati forgalom irányítja át a ILB végpont

## <a name="limitations"></a>Korlátozások

Belső terheléselosztó nem támogatja a SNAT. Ez a dokumentum összefüggésében SNAT port színleg forrás hálózati címfordítás hivatkozik.  Ez vonatkozik helyzetek, amikor egy virtuális Gépet a load balancer készlet kell-e a megfelelő belső terheléselosztó tartozó előtérbeli IP-cím elérésére. Ez a forgatókönyv nem támogatott belső terheléselosztóhoz. Kapcsolódási hibák történik, ha a folyamat a virtuális gépre, a folyamat szolgáltatásoktól elosztott terhelésű. Ilyen helyzetekben a proxy stílus terheléselosztó kell használnia.

## <a name="next-steps"></a>Következő lépések

[Azure Load Balancer Azure Resource Manager támogatása](load-balancer-arm.md)

[Ismerkedés az Internet felé néző terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[Ismerkedés a belső terheléselosztók konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
