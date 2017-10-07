---
title: "egy virtuális hálózatot egy prémium szintű Azure Redis Cache aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és kezelheti a Premium szint Azure Redis Cache példány virtuális hálózati támogatása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: sdanie
ms.openlocfilehash: fab715f4d9365ee4c2f8b89d2e2e58768c25b671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Hogyan tooconfigure a virtuális hálózati egy prémium szintű Azure Redis Cache támogatása
Azure Redis Cache rendelkezik másik gyorsítótármappa ajánlatokat, amelyek hello választott gyorsítótár mérete és a funkciót, beleértve a prémium réteg szolgáltatások, például a fürtszolgáltatás, az adatmegőrzésre és a virtuális hálózat támogatásának rugalmasságot biztosítanak. A virtuális hálózat egy magánhálózaton hello felhőben. Egy Vnetet az Azure Redis Cache példány van beállítva, amikor nincs nyilvánosan megcímezhető, és csak a virtuális gépek és az alkalmazások hello virtuális hálózaton belül érhető el. Ez a cikk bemutatja, hogyan támogatják a virtuális hálózati tooconfigure a premium Azure Redis Cache példány.

> [!NOTE]
> Azure Redis Cache támogatja mindkét klasszikus és Resource Manager Vnetek.
> 
> 

Más prémium gyorsítótár funkciókról további információért lásd: [bemutatása toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Miért virtuális hálózatot?
[Az Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) telepítés biztosítja a fokozott biztonságot és az Azure Redis Cache, valamint a alhálózatok, hozzáférés-vezérlési házirendeket, elkülönítési és egyéb szolgáltatások toofurther korlátozhatja a hozzáférést.

## <a name="virtual-network-support"></a>Virtuális hálózati támogatása
Virtual Network (VNet) támogatása konfigurált hello **új Redis Cache** panel gyorsítótár létrehozása során. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Miután kiválasztotta a prémium tarifacsomag, konfigurálhatja a virtuális hálózat, amely hello kiválasztásával Redis virtuális integráció azonos előfizetés és a gyorsítótár. új virtuális hálózatot, toouse először hozza létre azt hello utasításait követve [hello Azure-portál virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [hozzon létre egy virtuális hálózat (klasszikus) hello Azure-portál használatával](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) , és visszatér a toohello **Új Redis Cache** panel toocreate és a prémium szintű gyorsítótár konfigurálása.

Kattintson az új gyorsítótár, a virtuális hálózat tooconfigure hello **virtuális hálózati** a hello **új Redis Cache** panelen, és jelölje be hello virtuális hálózat szükséges hello legördülő listából.

![Virtuális hálózat][redis-cache-vnet]

Válassza ki a kívánt alhálózatot a hello hello **alhálózati** legördülő listában, és adja meg a szükséges hello **statikus IP-cím**. Ha használja a klasszikus virtuális hálózat hello **statikus IP-cím** mező kitöltése nem kötelező, és ha nincs megadva, egy kijelölt hello alhálózatból választja.

> [!IMPORTANT]
> Az Azure Redis Cache tooa erőforrás-kezelő virtuális hálózat telepítésekor hello gyorsítótár, amely tartalmazza az Azure Redis Cache példány kívül más erőforrások dedikált alhálózat kell lennie. Ha toodeploy tett kísérlet az Azure Redis Cache tooa más erőforrások, az hello telepítését tartalmazó erőforrás-kezelő virtuális hálózat tooa alhálózati sikertelen lesz.
> 
> 

![Virtuális hálózat][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure fenntartja az egyes IP-címek minden alhálózaton belül, és ezeknél a címeknél nem használható. hello hello alhálózatok első és utolsó IP-címek számára fenntartott protokoll megfelelési, valamint három további címek az Azure-szolgáltatásokhoz használt. További információkért lásd: [vannak-e bármilyen korlátozás belül ezek alhálózatok IP-címeket használnak?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Továbbá a hello Azure virtuális hálózat infrastruktúra által használt toohello IP-címek, a minden egyes Redis példány hello alhálózati használja a két IP-címek / shard és egy további IP-cím hello terheléselosztóhoz. Egy nem fürtözött gyorsítótár toohave egy shard minősül.
> 
> 

Hello gyorsítótár létrehozása után megtekintheti kattintva hello VNet konfigurációja hello **virtuális hálózati** a hello **erőforrás menü**.

![Virtuális hálózat][redis-cache-vnet-info]

tooconnect tooyour Azure Redis cache példány egy Vnetet használatakor hello kapcsolati karakterláncban adja meg a gyorsítótár hello állomásnevét, ahogy az alábbi példa hello:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis Cache VNet – gyakori kérdések
a következő lista hello hello Azure Redis Cache méretezésével kapcsolatos kérdések válaszok toocommonly tartalmazza.

* [Mik azok a gyakori hiba a konfiguráció problémákat az Azure Redis Cache és Vnetekhez?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Hogyan ellenőrizhetem, hogy működik-e a gyorsítótár a VNETEN belül?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Használható alapszintű vagy standard gyorsítótárával Vnetekhez?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Miért nem a Redis gyorsítótár létrehozása sikertelen lesz az egyes alhálózatok, de nem mások?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Mik azok a hello alhálózati kapcsolatos követelmények?](#what-are-the-subnet-address-space-requirements)
* [Működnek-e minden gyorsítótár-funkciók egy VNETET a gyorsítótárhoz esetén?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Mik azok a gyakori hiba a konfiguráció problémákat az Azure Redis Cache és Vnetekhez?
Azure Redis Cache a Vneten belül helyezkedik el, amikor a rendszer a következő táblák hello hello portokat használja. 

>[!IMPORTANT]
>Ha a következő táblák hello hello portjainak blokkolja, előfordulhat, hogy hello gyorsítótár nem működik megfelelően. Rendelkezik egy vagy több ezeket a portokat blokkolva hello leggyakoribb helytelen konfiguráció probléma használata esetén egy Vnetet az Azure Redis Cache.
> 
> 

- [Kimenő portokra vonatkozó követelmények](#outbound-port-requirements)
- [Bejövő portokra vonatkozó követelmények](#inbound-port-requirements)

### <a name="outbound-port-requirements"></a>Kimenő portokra vonatkozó követelmények

Nincsenek hét kimenő port.

- Ha szükséges, minden kimenő kapcsolatok toohello internet létrehozható egy ügyfélen keresztül helyszíni naplózási eszköz.
- Három hello portok útvonal-forgalom tooAzure végpontok karbantartási Azure Storage és az Azure DNS-ben.
- porttartományok fennmaradó hello és a belső Redis alhálózati kommunikációhoz. Nincs alhálózat NSG-szabályok a belső Redis alhálózati kommunikációhoz szükségesek.

| Port(ok) | Irány | Átviteli protokoll | Cél | Helyi IP | Távoli IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Kimenő |TCP |Redis függőségek az Azure Storage/nyilvános kulcsokra épülő infrastruktúra (Internet) | (Redis alhálózati) |* |
| 53 |Kimenő |TCP/UDP |A DNS-(Internet/VNet) függőségek redis | (Redis alhálózati) |* |
| 8443 |Kimenő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) | (Redis alhálózati) |
| 10221-10231 |Kimenő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) | (Redis alhálózati) |
| 20226 |Kimenő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati) |
| 13000-13999 |Kimenő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati) |
| 15000-15999 |Kimenő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati) |


### <a name="inbound-port-requirements"></a>Bejövő portokra vonatkozó követelmények

Nincsenek nyolc bejövő port tartományon. A bejövő kérések ezen tartományok használata vagy bejövő más hello üzemeltetett szolgáltatások ugyanazt a virtuális Hálózatot, vagy a belső toohello Redis alhálózati kommunikáció.

| Port(ok) | Irány | Átviteli protokoll | Cél | Helyi IP | Távoli IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Bejövő |TCP |Ügyfél-kommunikáció tooRedis, Azure terheléselosztás | (Redis alhálózati) |Virtuális hálózat, az Azure terheléselosztó |
| 8443 |Bejövő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati) |
| 8500 |Bejövő |TCP/UDP |Az Azure terheléselosztás | (Redis alhálózati) |Azure Load Balancer |
| 10221-10231 |Bejövő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati), Azure Load Balancer |
| 13000-13999 |Bejövő |TCP |Ügyfél-kommunikáció tooRedis fürtök, Azure terheléselosztás | (Redis alhálózati) |Virtuális hálózat, az Azure terheléselosztó |
| 15000-15999 |Bejövő |TCP |Ügyfél-kommunikáció tooRedis fürtök, Azure terheléselosztás | (Redis alhálózati) |Virtuális hálózat, az Azure terheléselosztó |
| 16001 |Bejövő |TCP/UDP |Az Azure terheléselosztás | (Redis alhálózati) |Azure Load Balancer |
| 20226 |Bejövő |TCP |Belső Redis-kommunikáció | (Redis alhálózati) |(Redis alhálózati) |

### <a name="additional-vnet-network-connectivity-requirements"></a>További virtuális hálózat hálózati kapcsolat követelmények

Nincsenek hálózati kapcsolat az Azure Redis Cache, előfordulhat, hogy kezdetben jutott virtuális hálózatban. Azure Redis Cache szükséges összes hello a következő elemek toofunction megfelelően, ha a virtuális hálózaton belül.

* Kimenő hálózati kapcsolat tooAzure tárolási végpontok világszerte. Ez magában foglalja a végpontok hello található hello Azure Redis Cache példány, valamint tárolási végpontok található, és ugyanabban a régióban **más** Azure-régiók. Azure Storage-végpontok oldja meg a következő DNS-tartományok hello: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*, és *file.core.windows.net*. 
* Kimenő hálózati kapcsolatra túl*ocsp.msocsp.com*, *mscrl.microsoft.com*, és *crl.microsoft.com*. Ez az összekapcsolhatóság szükséges toosupport SSL funkció.
* hello virtuális hálózat DNS-konfiguráció hello megoldásának hello végpontok képesnek kell lennie, és a tartományok említett hello korábbi pontok. A DNS-követelmények érheti el, egy érvényes DNS-infrastruktúra hello virtuális hálózat megmarad és konfigurált biztosításával.
* Kimenő hálózati kapcsolat toohello Azure figyelési végpontok, amelyek alapján a következő DNS-tartományok hello feloldani a következő: shoebox2-black.shoebox2.metrics.nsatc.net, Észak-prod2.prod2.metrics.nsatc.net, azglobal-black.azglobal.metrics.nsatc.net, shoebox2-red.shoebox2.metrics.nsatc.net, kelet-prod2.prod2.metrics.nsatc.net, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Hogyan ellenőrizhetem, hogy működik-e a gyorsítótár a VNETEN belül?

>[!IMPORTANT]
>Ha egy virtuális hálózat alkotóelem tooan Azure Redis Cache példány, a gyorsítótár-ügyfeleknek kell lenniük a hello ugyanazt a virtuális Hálózatot, beleértve az alkalmazások tesztelése vagy diagnosztikai pingelés eszközök.
>
>

Hello előző szakaszban leírtak szerint hello portokra vonatkozó követelmények konfigurálása után ellenőrizheti, hogy működik-e a gyorsítótár hello lépések elvégzésével.

- [Újraindítás](cache-administration.md#reboot) összes hello gyorsítótár csomópontok. Ha az összes hello szükséges gyorsítótár-függőségeit nem érhető el (leírtak [portokra vonatkozó követelmények bejövő](cache-how-to-premium-vnet.md#inbound-port-requirements) és [kimenő portokra vonatkozó követelmények](cache-how-to-premium-vnet.md#outbound-port-requirements)), hello gyorsítótár sikeresen nem fogja tudni toorestart.
- Hello gyorsítótár-csomópontok (a hello gyorsítótár állapotát hello Azure-portál által jelentett) újraindítása, ha a következő tesztek hello végezheti el:
  - Pingelje hello gyorsítótár végpontjához (használja a portot 6380), a gép, amely hello belül ugyanazt a virtuális Hálózatot, hello gyorsítótár használata [tcping](https://www.elifulkerson.com/projects/tcping.php). Példa:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Ha hello `tcping` eszköz jelzi, hogy hello port meg nyitva, illetve hello gyorsítótár kapcsolat hello virtuális hálózaton lévő ügyfelek számára elérhető.

  - Egy másik módja tootest egy teszt gyorsítótárügyfél toocreate (amely lehet egy egyszerű konzolalkalmazást StackExchange.Redis használatával), amely toohello gyorsítótár csatlakozik, és hozzáadja, és néhány elemet lekéri hello gyorsítótárból. Ügyfél mintaalkalmazás hello alakzatot, amely a virtuális gép telepítése hello hello gyorsítótár, ugyanazt a virtuális Hálózatot, és futtassa azt tooverify kapcsolat toohello gyorsítótár.


### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Használható alapszintű vagy standard gyorsítótárával Vnetekhez?
Vnetek csak prémium szintű gyorsítótárak használható.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Miért nem a Redis gyorsítótár létrehozása sikertelen lesz az egyes alhálózatok, de nem mások?
Ha telepít egy Azure Redis Cache tooa erőforrás-kezelő virtuális hálózatot, hello gyorsítótár nincs más erőforrástípus tartalmazó dedikált alhálózat kell lennie. Ha toodeploy tett kísérlet az Azure Redis Cache tooa más erőforrások, az hello telepítését tartalmazó erőforrás-kezelő virtuális hálózat alhálózati sikertelen lesz. Egy új Redis gyorsítótár létrehozása előtt törölnie kell az hello meglévő erőforrásokat hello alhálózaton belül.

Telepíthet több típust az erőforrások tooa klasszikus virtuális hálózatot, amíg meg vannak-e elegendő IP-címek érhető el.

### <a name="what-are-hello-subnet-address-space-requirements"></a>Mik azok a hello alhálózati kapcsolatos követelmények?
Azure fenntartja az egyes IP-címek minden alhálózaton belül, és ezeknél a címeknél nem használható. hello hello alhálózatok első és utolsó IP-címek számára fenntartott protokoll megfelelési, valamint három további címek az Azure-szolgáltatásokhoz használt. További információkért lásd: [vannak-e bármilyen korlátozás belül ezek alhálózatok IP-címeket használnak?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Továbbá a hello Azure virtuális hálózat infrastruktúra által használt toohello IP-címek, a minden egyes Redis példány hello alhálózati használja a két IP-címek / shard és egy további IP-cím hello terheléselosztóhoz. Egy nem fürtözött gyorsítótár toohave egy shard minősül.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Működnek-e minden gyorsítótár-funkciók egy VNETET a gyorsítótárhoz esetén?
Ha a gyorsítótár egy virtuális hálózat része, csak a virtuális hálózat hello ügyfelek hello gyorsítótár férhet hozzá. Ennek eredményeképpen hello következő gyorsítótár-kezelési funkciók nem működnek most.

* Konzol redis - konzol Redis fut a helyi böngészőben, amely hello virtuális hálózaton kívül, mert tooyour gyorsítótár nem tud kapcsolódni.

## <a name="use-expressroute-with-azure-redis-cache"></a>Azure Redis gyorsítótár ExpressRoute használata
Az ügyfelek kapcsolódhatnak egy [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) áramkör tootheir virtuális hálózati infrastruktúra, így kiterjesztése a helyszíni hálózati tooAzure. 

Alapértelmezés szerint egy újonnan létrehozott ExpressRoute-kapcsolatcsoport nem végzi el a kényszerített bújtatás (az alapértelmezett útvonal hirdetmény 0.0.0.0/0) egy virtuális hálózaton. Ennek eredményeképpen kimenő internetkapcsolat közvetlenül a virtuális hálózat hello engedélyezett, és ügyfélalkalmazások képes tooconnect tooother Azure végpontok, beleértve az Azure Redis Cache.

Azonban ez a gyakori ügyfél-konfigurációs a kényszerített bújtatás toouse (az alapértelmezett útvonal hirdetése) amely arra kényszeríti a kimenő Internet forgalom tooinstead folyamata a helyszíni. A forgalom áramlását hello kimenő forgalom esetén megsérti Azure Redis Cache kapcsolatot, majd helyszíni blokkolva, például úgy, hogy hello Azure Redis Cache példány nem tud toocommunicate és annak függőségeit.

egy (vagy több) toodefine felhasználó által definiált útvonalak (udr-EK), amely tartalmazza az Azure Redis Cache hello hello alhálózaton hello megoldás. Egy UDR hello alapértelmezett útvonal helyett szembeni szerződéses kötelezettségeket vonatkozó alhálózati útvonalakat határozza meg.

Ha lehetséges a következő konfigurációs toouse hello ajánlott:

* hello ExpressRoute konfigurációs hirdeti 0.0.0.0/0, és alapértelmezés szerint kényszerített bújtatja minden kimenő forgalom helyszíni.
* hello alkalmazott UDR toohello alhálózat hello Azure Redis Cache tartalmazó határoz meg egy TCP/IP-forgalom toohello működő útvonalat a 0.0.0.0/0 nyilvános internethez. például hello beállítása szerint a következő ugrás típusa too'Internet ".

hello kombinált hatását, hogy ezeket a lépéseket az, hogy a hello alhálózat-szintű UDR elsőbbséget élvez az ExpressRoute kényszerített bújtatás, biztosítva ezzel kimenő Internet-hozzáférést a hello Azure Redis Cache hello.

ExpressRoute segítségével helyszíni alkalmazásból összekötő tooan Azure Redis Cache példány nincs egy tipikus használati eset tooperformance okok miatt (a legjobb teljesítmény érdekében az Azure Redis Cache ügyfelek kell hello hello Azure Redis Cache és ugyanabban a régióban).

>[!IMPORTANT] 
>egy UDR definiált útvonalak hello **kell** elég konkrétan fogalmaz ahhoz tootake sorrend lehet bármely hello ExpressRoute-konfiguráció által hirdetett útvonalakat keresztül. hello alábbi példa hello széleskörű 0.0.0.0/0 címtartományt használja, és így potenciálisan véletlenül felülbírálhatja útvonal-hirdetéseinek pontosabb címtartományai használatával.

>[!WARNING]  
>Azure Redis Cache ExpressRoute-konfigurációk használata nem támogatott, amelyek **helytelenül hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útvonalak határokon hirdetési**. ExpressRoute konfigurációkat, amelyek rendelkeznek a nyilvános társviszony konfigurálva, a Microsoft Azure IP-címtartományok számos útvonal-hirdetéseinek kapni a Microsofttól. Ha a címtartomány helytelenül határokon meghirdetett hello magánhálózati társviszony-létesítési elérési úton, hello eredménye, hogy minden kimenő hálózati csomagok hello Azure Redis Cache példány alhálózatból-e a kényszerített bújtatott helytelenül tooa az ügyfél helyszíni hálózat infrastruktúra. A hálózati folyamata Azure Redis Cache megszakad. hello megoldás toothis probléma a toostop cross-hirdetési útvonalak hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útról.


Háttér-információkat a felhasználó által definiált útvonalak érhető el ezen [áttekintése](../virtual-network/virtual-networks-udr-overview.md).

ExpressRoute kapcsolatos további információkért lásd: [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan több premium toouse gyorsítótár szolgáltatásokat.

* [Bevezetés toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

