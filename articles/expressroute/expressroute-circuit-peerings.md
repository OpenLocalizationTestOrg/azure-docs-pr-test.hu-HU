---
title: "aaaAzure ExpressRoute Kapcsolatcsoportok és útválasztási tartományok |} Microsoft Docs"
description: "Ezen a lapon ExpressRoute-Kapcsolatcsoportok és útválasztási tartományok hello áttekintést nyújt."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 1d43cbf668accdd7aa4efb053ea1e9027d10e4a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute-Kapcsolatcsoportok és útválasztási tartományok
 Kell rendeznie egy *ExpressRoute-kapcsolatcsoportot* tooconnect a helyszíni infrastruktúra tooMicrosoft kapcsolat-szolgáltató használatával. az alábbi ábra hello reprezentációja logikai a WAN és a Microsoft közötti kapcsolatot.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>ExpressRoute-Kapcsolatcsoportok
Egy *ExpressRoute-kapcsolatcsoportot* jelenti. a helyszíni infrastruktúra és a kapcsolat-szolgáltató használatával a Microsoft felhőszolgáltatásai között. Több ExpressRoute-Kapcsolatcsoportok sorba rendezheti. A kapcsolatok lehet hello azonos vagy különböző régiókban, és különböző kapcsolat szolgáltatók keresztül csatlakoztatott tooyour helyi lehet. 

ExpressRoute-Kapcsolatcsoportok tooany fizikai entitások nem képezi le. Expressroute-kapcsolatcsoporthoz egyedileg azonosít egy szabványos GUID neve (s-kulcs) szolgáltatás kulcsként. hello szolgáltatás kulcsa hello egyetlen adat, Microsoft hello kapcsolat szolgáltatójánál és Ön között. hello s-kulcs nincs titkos kulcs biztonsági okokból. Van egy ExpressRoute-kapcsolatcsoportot és hello közötti társítás 1:1 s-kulcs.

Egy ExpressRoute-kapcsolatcsoportot toothree független esetében legfeljebb tartalmazhat: az Azure nyilvános, Azure személyes és a Microsoft. Két független BGP minden társviszony-létesítés munkamenetek minden azok redundantly magas rendelkezésre állásúként konfigurálva. Van egy 1: n (1 < = N < = 3) ExpressRoute-kapcsolatcsoportot közötti megfeleltetés és útválasztási tartományok. Egy ExpressRoute-kapcsolatcsoportot rendelkezhet közül legalább egy, a két vagy ExpressRoute-kapcsolatcsoportot / engedélyezve az összes három esetében.

A kapcsolatok rögzített sávszélességű (50 MB/s, 100 MB/s, 200 MB/s, 500 MB/s, 1 GB/s, 10 GB/s), és a csatlakoztatott tooa kapcsolat szolgáltatóját, és társviszony-létesítési helye. hello sávszélesség választja által megosztott minden hello társviszony hello kör. 

### <a name="quotas-limits-and-limitations"></a>Kvóták korlátai és korlátozásai
Alapértelmezett kvótái és korlátai érvényes minden ExpressRoute-kapcsolatcsoportot. Tekintse meg a toohello [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md) kvóták naprakész információkat lapját.

## <a name="expressroute-routing-domains"></a>ExpressRoute útválasztási tartományok
Egy ExpressRoute-kapcsolatcsoportot több útválasztási tartomány társítva van: az Azure nyilvános, Azure személyes és a Microsoft. Minden hello útválasztási tartomány két útválasztók konfigurált azonos (aktív-aktív vagy terheléselosztási megosztása konfigurációs) a magas rendelkezésre állás érdekében. Azure-szolgáltatások vannak üzletiként besorolva *Azure nyilvános* és *Azure saját* toorepresent hello IP-címzési séma.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="private-peering"></a>Magánhálózati társviszony-létesítés
Azure számítási szolgáltatások, azaz a virtuális gépek (IaaS) és magánhálózati társviszony-létesítési tartomány hello keresztül csatlakozhatnak felhőszolgáltatásokat (PaaS), amely a virtuális hálózaton belül vannak telepítve. hello magánhálózati társviszony-létesítési tartomány az a Microsoft Azure tekinthető toobe az alaphálózat megbízható kiterjesztését. A központi hálózat és az Azure virtuális hálózatokról (Vnetekről) közötti kétirányú kapcsolatot állíthat be. A társviszony lehetővé teszi a csatlakoztathatók toovirtual gépek és felhőszolgáltatások közvetlenül a saját privát IP-címek.  

Egynél több virtuális hálózati toohello magánhálózati társviszony-létesítési tartományhoz csatlakoztathatja. Felülvizsgálati hello [gyakori kérdéseket tartalmazó oldal](expressroute-faqs.md) korlátai és korlátozásai információt. Hello ellátogathat [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md) lap korlátozások naprakész információkat.  Tekintse meg a toohello [útválasztás](expressroute-routing.md) lap részletes információkat a útválasztási konfigurációjáról.

### <a name="public-peering"></a>Nyilvános társviszony-létesítés
A nyilvános IP-szolgáltatások, például az Azure tárolás, az SQL-adatbázisok és a webhelyek érhető el. Nyilvános IP-címek, beleértve a virtuális IP-címek a felhőalapú szolgáltatások hello nyilvános társviszony-létesítési útválasztási tartomány segítségével üzemeltetett tooservices közvetlenül kapcsolódhatnak. Hello nyilvános társviszony-létesítési tartomány tooyour DMZ csatlakozhat, és csatlakozzon az tooall Azure a nyilvános IP-címeket a WAN tooconnect keresztül nélkül szolgáltatások hello internet. 

Kapcsolat mindig kezdeményezi a WAN tooMicrosoft Azure szolgáltatások. A Microsoft Azure szolgáltatás nem lesz képes tooinitiate kapcsolatok azokat a hálózaton keresztül az útválasztási tartomány. Nyilvános társviszony engedélyezése után fog tudni tooconnect tooall Azure szolgáltatások. Jelenleg nem teszik lehetővé, amelynek azt hirdetési útvonalakat tooselectively kivételezési szolgáltatások. Azt a hello társviszony keresztül tooyou hirdetési előtagok hello listáját megtekintheti [Microsoft Azure Datacenter IP-címtartományok](http://www.microsoft.com/download/details.aspx?id=41653) lap. heti hello lap tartalmát.

Egyéni útvonal szűrőket belül a hálózati tooconsume csak hello útvonalak kell határozhat meg. Tekintse meg a toohello [útválasztás](expressroute-routing.md) lap részletes információkat a útválasztási konfigurációjáról. 

Lásd: hello [gyakori kérdéseket tartalmazó oldal](expressroute-faqs.md) hello nyilvános társviszony-létesítési útválasztási tartomány támogatott szolgáltatások olvashat. 

### <a name="microsoft-peering"></a>Microsoft társviszony-létesítés
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Kapcsolat tooall más Microsoft online szolgáltatásokhoz (például Office 365-szolgáltatásokhoz) keresztül hello Microsoft társviszony-létesítés lesz. Engedélyezzük a WAN és a Microsoft felhőszolgáltatások keresztül hello Microsoft társviszony-létesítési útválasztási tartomány közötti kétirányú kapcsolatot. Csak a nyilvános IP-címek, amelynek a tulajdonosa, vagy a kapcsolat szolgáltatójánál keresztül tooMicrosoft felhőszolgáltatások kell csatlakoztatni, és meg kell felelnie a tooall hello definiált szabályok. Lásd: hello [ExpressRoute Előfeltételek](expressroute-prerequisites.md) olvashat.

Lásd: hello [gyakori kérdéseket tartalmazó oldal](expressroute-faqs.md) további információk a támogatott szolgáltatások, a költségek és a konfigurációs adatait. Lásd: hello [helyek ExpressRoute](expressroute-locations.md) kapcsolatos adatokat kapcsolat szolgáltatók ajánlat a Microsoft társviszony-létesítés támogatása hello lista lap.

## <a name="routing-domain-comparison"></a>Útválasztási tartomány összehasonlítása
hello az alábbi táblázat összehasonlítja a hello három útválasztási tartományok.

|  | **Magánhálózati társviszony-létesítés** | **Nyilvános társviszony-létesítés** | **A Microsoft társviszony-létesítés** |
| --- | --- | --- | --- |
| **Max. támogatott társviszony-létesítés # előtagok** |Alapértelmezés szerint 10 000-re az ExpressRoute Premium 4000 |200 |200 |
| **Az IP-címtartományok támogatott** |Bármilyen érvényes IPv4-cím a WAN hálózaton belül. |Nyilvános IPv4-címet, vagy a kapcsolat szolgáltatóját. |Nyilvános IPv4-címet, vagy a kapcsolat szolgáltatóját. |
| **Követelmények száma szerint** |Privát és nyilvános SZÁMAIT. Kell saját hello nyilvános SZÁMOT, ha úgy dönt, hogy egy toouse. |Privát és nyilvános SZÁMAIT. Azonban igazolnia kell tulajdonjogát, a nyilvános IP-címeket. |Privát és nyilvános SZÁMAIT. Azonban igazolnia kell tulajdonjogát, a nyilvános IP-címeket. |
| **Útválasztó illesztő IP-címek** |RFC1918 és nyilvános IP-címek |Nyilvános IP-címek az útválasztási nyilvántartó regisztrált tooyou. |Nyilvános IP-címek az útválasztási nyilvántartó regisztrált tooyou. |
| **Az MD5 kivonatoló támogatása** |Igen |Igen |Igen |

Egy vagy több útválasztási tartományok hello tooenable kiválaszthatja az ExpressRoute-kapcsolatcsoportot részeként. Választhat toohave összes hello útválasztási tartomány elhelyezése hello azonos VPN, ha azt szeretné, hogy toocombine egyetlen útválasztási tartományról őket. Akkor helyezheti is őket másik útválasztási tartományok, hasonló toohello diagramja. hello konfigurációs van a magánhálózati társviszony-létesítés csatlakozik közvetlenül toohello alaphálózat, illetve hello nyilvános és a Microsoft társviszony-létesítési hivatkozások csatlakoztatott tooyour DMZ ajánlott.

Ha úgy dönt, toohave összes három társviszony-létesítési munkameneteket, a BGP-munkamenetek (az egyes társviszony-létesítési egy pár) három párok kell rendelkeznie. hello BGP munkamenet Értékpárokat adjon meg egy magas rendelkezésre állású hivatkozást. Ha réteg 2 kapcsolat szolgáltatók keresztül kapcsolódik, konfigurálása és kezelése a útválasztási felelős fogja. További hello megtekintésével [munkafolyamatok](expressroute-workflows.md) ExpressRoute beállításához.

## <a name="next-steps"></a>Következő lépések
* Találjon egy szolgáltatót. Lásd: [ExpressRoute szolgáltatás szolgáltatók és a helyek](expressroute-locations.md).
* Ellenőrizze, hogy minden előfeltétel teljesül-e. Lásd: [ExpressRoute-előfeltételek](expressroute-prerequisites.md).
* Az ExpressRoute-kapcsolat konfigurálása.
  * [ExpressRoute-kapcsolatcsoportok létrehozása és kezelése](expressroute-howto-circuit-portal-resource-manager.md)
  * [Útválasztás (társviszony-létesítés) konfigurálása ExpressRoute-kapcsolatcsoportok számára](expressroute-howto-routing-portal-resource-manager.md)

