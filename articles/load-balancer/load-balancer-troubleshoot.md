---
title: Azure Load Balancer aaaTroubleshoot |} Microsoft Docs
description: "Azure Load Balancer szolgáltatással kapcsolatos ismert problémák elhárítása"
services: load-balancer
documentationcenter: na
author: RamanDhillon
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2017
ms.author: kumud
ms.openlocfilehash: 56b73fcbf0bbf18cedfd113a280cfafa2a3dc9f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure Load Balancer hibaelhárítása

Ezen a lapon elhárításával kapcsolatban biztosít információkat Azure Load Balancer gyakori kérdésekre. Ha hello terheléselosztó kapcsolat nem érhető el, hello leggyakoribb tüneteket a következők: 
- Hello terheléselosztó mögötti virtuális gépek nem válaszolnak toohealth mintavétel 
- Hello terheléselosztó mögötti virtuális gépek nem válaszolnak toohello adatforgalmat hello konfigurált porton

## <a name="symptom-vms-behind-hello-load-balancer-are-not-responding-toohealth-probes"></a>Jelenség: Hello terheléselosztó mögötti virtuális gépek nem válaszolnak toohealth mintavétel
Hello háttér kiszolgálók tooparticipate hello load balancer készletben akkor meg kell felelnie hello mintavételi ellenőrzése. Állapotteljesítmény kapcsolatos további információkért lásd: [ismertetése Load Balancer vizsgálat](load-balancer-custom-probe-overview.md). 

nem válaszol hello terheléselosztó háttérkészletének virtuális gépek toohello vizsgálat a következő okok miatt hello esedékes tooany: 
- A terheléselosztó háttérkészletének virtuális gép állapota nem megfelelő 
- Virtuális gép nem figyeli a hello mintavételi portot a terheléselosztó háttérkészletének betöltése 
- Tűzfal- vagy hálózati biztonsági csoport blokkolja a terheléselosztó háttérkészletéből virtuális gépek hello hello port 
- Más hibás konfigurációi által a terheléselosztó

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>1. ok: A terheléselosztó háttérkészletéből virtuális gép állapota nem megfelelő 

**Érvényesítési és megoldás szerint**

tooresolve probléma részt vevő virtuális gépek toohello be, és ellenőrizze, hogy hello virtuális gép állapota kifogástalan, és képes válaszolni túl**PsPing** vagy **TCPing** hello készletben egy másik virtuális gépről. Ha hello virtuális gép állapota nem megfelelő, vagy nem toorespond toohello mintavételi, akkor hello probléma kijavításához és VM biztonsági tooa kifogástalan állapotban, mielőtt részt vehetne terheléselosztási hello beolvasása.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-hello-probe-port"></a>2. ok: Terheléselosztó háttérkészletének virtuális gép nem hello mintavételi portot figyel
Ha hello VM működik megfelelően, de toohello mintavételi nem válaszol, majd egy lehetséges oka lehet, hogy hello mintavételi port nincs nyitva a részt vevő virtuális gép hello, vagy hello virtuális gép nem adott portot figyel.

**Érvényesítési és megoldás szerint**

1. Toohello háttér Virtuálisgép jelentkezni. 
2. Nyisson meg egy parancssort, és futtassa a következő parancs toovalidate hello mintavételi portot figyel egy alkalmazás hello:   
            netstat - a
3. Ha hello port állapota nem szerepel az **FIGYELŐ**, hello megfelelő port konfigurálása. 
4. Azt is megteheti, válasszon egy másik portra, amely blokkolandóként **FIGYELŐ**, és ennek megfelelően betölteni a terheléselosztó-konfiguráció frissítése.              

###<a name="cause-3-firewall-or-a-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vms"></a>3. ok: Tűzfal vagy egy hálózati biztonsági csoport blokkolja hello hello terheléselosztó háttérkészletének virtuális gépek portja  
VM blokkolja a hello mintavételi portot vagy egy vagy több hálózati konfigurált biztonsági csoportok hello alhálózaton lévő vagy a virtuális gép, hello hello hello tűzfala nem engedélyezi az hello mintavételi tooreach hello portot, hello virtuális gép esetén nem toorespond toohello állapotmintáihoz.          

**Érvényesítési és megoldás szerint**

* Ha hello tűzfal engedélyezve van, ellenőrizze, ha van konfigurálva tooallow hello mintavételi portot. Ha nem, hello tűzfal tooallow forgalom konfigurálása hello mintavételi portot, majd tesztelje újból. 
* A hálózati biztonsági csoportok hello listáját ellenőrizze, hogy hello mintavételi portot a bejövő vagy kimenő forgalom hello zavaró tényező rendelkezik-e. 
* Továbbá ellenőrizze, hogy egy **megtagadása az összes** hálózati biztonsági csoportok hello hello virtuális gép hálózati Adapterének-szabályok, vagy az alhálózatot, amely a magasabb prioritású, mint hello alapértelmezett szabály, amely lehetővé teszi a forgalom & LB mintavételt hello (hálózati biztonsági csoportok engedélyeznie kell a Load Balancer IP-címe 168.63.129.16). 
* Ha ezek a szabályok bármelyikét blokkolják hello mintavételi forgalmát, távolítsa el, és konfigurálja újra a hello szabályok tooallow hello mintavételi forgalmat.  
* Ha a virtuális gép hello most már megkezdődött toohello állapot-mintavételi csomagjai válaszol tesztelése. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4. ok: Más hibás konfigurációi által a terheléselosztó
Ha az összes hello előző okok úgy tűnik, toobe érvényesítve, és helyesen feloldani és hello háttér virtuális gép továbbra is nem válaszol toohello állapotmintáihoz, majd manuálisan ellenőrizze a hálózati kapcsolatot, és néhány nyomkövetések toounderstand hello kapcsolat gyűjtése.

**Érvényesítési és megoldás szerint**

* Használjon **Psping** hello egyikét a többi virtuális gépen belül hello VNet tootest hello mintavételi portot válasz (Példa:.\psping.exe -t 10.0.0.4:3389), és jegyezze fel eredmények. 
* Használjon **TCPing** hello egyikét a többi virtuális gépen belül hello VNet tootest hello mintavételi portot válasz (Példa:.\tcping.exe 10.0.0.4 3389-es), és jegyezze fel eredmények. 
* Ha nem érkezik válasz ping a vizsgálatok, majd
    - Futtassa a egyidejű Netsh nyomkövetési hello cél háttérkészlet virtuális gép és egy másik teszteléshez használt virtuális gép hello az azonos virtuális hálózaton. Most futtassa a PsPing tesztet egy kis ideig, bizonyos hálózati nyomkövetési és hello tesztet állítsa le. 
    - Hello hálózati rögzítési elemzéséhez, és van-e mind a bejövő és kimenő csomagok kapcsolódó toohello ping lekérdezés. 
        - Ha nincs bejövő csomagok figyelhetők a hello háttérkészlet VM, nincs potenciálisan a hálózati biztonsági csoportok vagy UDR nem megfelelő konfiguráció blokkoló hello forgalmat. 
        - Ha nincs kimenő csomagok figyelhetők a hello háttérkészlet VM, hello VM kell toobe be van jelölve, a nem kapcsolódó probléma merül fel (a eample, az alkalmazás blokkoló hello mintavételi portot). 
    - Győződjön meg arról, ha hello mintavételi csomagok folyamatban van a kényszerített tooanother cél (valószínűleg keresztül UDR beállítások), hello terheléselosztó elérése előtt. Ennek hatására hello forgalom toonever reach hello háttér virtuális gép. 
* Módosítsa a hello mintavételi típusa (például HTTP tooTCP), majd hello megfelelő portot a hálózati biztonsági csoportok hozzáférés-vezérlési listák és a tűzfalon toovalidate konfigurálása, ha hello probléma mintavételi válasz hello konfigurációjával. Állapot-mintavételi konfigurációjával kapcsolatos további információkért lásd: [végpont terheléselosztás állapotkonfiguráció mintavételi](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-tootraffic-on-hello-configured-data-port"></a>Jelenség: Terheléselosztó mögötti virtuális gépek nem válaszolnak tootraffic konfigurált hello adatok porton

Ha VM háttérkészlet van megadva, a megfelelő és toohello állapotfigyelő mintavételek menüpontban, válaszol, de még nem vesz részt terheléselosztás hello, vagy nem válaszol toohello az adatforgalmat, miatt lehet a következő okok miatt hello tooany: 
* Virtuális gép nem figyeli a hello adatok port terheléselosztó háttérkészletének betöltése 
* Hálózati biztonsági csoport blokkolja a terheléselosztó háttérkészletéből VM hello hello port  
* Hello használata a terheléselosztó hello azonos virtuális gép és a hálózati adapter 
* A terheléselosztó háttérkészletéből VM részt vevő hello hello Internet Terheléselosztói VIP elérése 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-hello-data-port"></a>1. ok: Terheléselosztó háttérkészletének virtuális gép nem hello adatok portot figyel 
Ha egy virtuális gép nem felel meg a toohello az adatforgalmat, akkor valószínűleg vagy hello cél port nincs nyitva a részt vevő virtuális gép hello, vagy hello virtuális gép nem adott portot figyel. 

**Érvényesítési és megoldás szerint**

1. Toohello háttér Virtuálisgép jelentkezni. 
2. Nyisson meg egy parancssort, és futtassa a következő parancs toovalidate hello adatok portot figyel egy alkalmazás hello:  
            netstat - a 
3. Ha hello port nem szerepel a állapotú "Figyelés" hello megfelelő figyelő port konfigurálása 
4. Ha hello port van megjelölve, figyelés, akkor ellenőrizze a célalkalmazás hello lehetséges probléma merül fel az adott porton. 

### <a name="cause-2-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vm"></a>2. ok: Hálózati biztonsági csoport blokkolja a terheléselosztó háttérkészletéből VM hello hello port  

Ha egy vagy több hálózati hello alhálózaton lévő vagy a virtuális gép hello konfigurált biztonsági csoportok, blokkolja a hello forrás IP-cím és port, akkor hello virtuális gép nem toorespond.

* Hello hello háttérkiszolgálón VM konfigurált hálózati biztonsági csoportok listában. További információkért lásd:
    -  [Hello portál használatával a hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md)
    -  [Hálózati biztonsági csoportok kezelése a PowerShell-lel](../virtual-network/virtual-network-manage-nsg-arm-ps.md)
* A hálózati biztonsági csoportok hello listáját ellenőrizze, hogy:
    - hello adatok port bejövő vagy kimenő forgalom hello zavaró tényező rendelkezik. 
    - egy **megtagadása az összes** hálózati biztonsági csoport szabály hello hello VM vagy hello alhálózatot, amely egy magasabb prioritással bír, hogy hello alapértelmezett szabály, amely lehetővé teszi, hogy a terheléselosztó mintavételt és a forgalom hálózati Adapterének (hálózati biztonsági csoportok engedélyeznie kell a Load Balancer IP-címe 168.63.129.16, amely mintavételi portot) 
* Ha bármelyik hello szabályok blokkolják hello forgalmát, távolítsa el, és konfigurálja újra ezeket szabályok tooallow hello az adatforgalmat.  
* Ha hello VM most már megkezdődött toorespond toohello állapotfigyelő mintavételt tesztelése.

### <a name="cause-3-accessing-hello-load-balancer-from-hello-same-vm-and-network-interface"></a>3. ok: Hozzáférés hello terheléselosztó hello a virtuális gép és a hálózati adaptert 

Ha a virtuális gép egy terheléselosztó hello háttér alkalmazást próbál tooaccess egy másik alkalmazást hello azonos háttér virtuális gép hello keresztül azonos hálózati csatoló, a nem támogatott lehetőséget, és sikertelen lesz. 

**Megoldási** feloldhatja a problémát az Adminisztráció hello a következő módszerek egyikét:
* Alkalmazásonként külön háttérkészlet virtuális gépeket konfigurálhatja. 
* Hello alkalmazás beállítása az kettős NIC virtuális gépeken, minden egyes alkalmazás használta a saját hálózati illesztő IP-címet. 

### <a name="cause-4-accessing-hello-internal-load-balancer-vip-from-hello-participating-load-balancer-backend-pool-vm"></a>4. ok: Belső Terheléselosztói VIP hello eléréséhez hello részt vesz a terheléselosztó háttérkészletéből VM

Ha egy ILB VIP egy Vneten belül van konfigurálva, és megpróbál hello résztvevő háttér virtuális gépek egyik tooaccess hello belső Terheléselosztói VIP, amely hibát eredményez. Ez egy nem támogatott eset.
**Megoldási** kiértékelése Alkalmazásátjáró vagy más proxyk (például nginx vagy haproxy esetén), amely kind forgatókönyv toosupport. Alkalmazásátjáró kapcsolatos további információkért lásd: [Alkalmazásátjáró – áttekintés](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>További hálózati rögzítést
Ha úgy dönt, hogy tooopen támogatási esetet, gyűjteni a következő információ gyorsabb rendezése hello. Válassza ki a virtuális gép egyetlen háttér tooperform hello tesztek a következő:
- Az egyik hello háttér virtuális gépeken belül hello VNet tootest hello mintavételi portot válasz Psping eszköz használata (Példa: a psping 10.0.0.4:3389), és jegyezze fel eredmények. 
- Egy hello háttér virtuális gépeken belül hello VNet tootest hello mintavételi portot válasz TCPing használja (Példa: a psping 10.0.0.4:3389), és jegyezze fel eredmények.
- Ha nem érkezik válasz ping a vizsgálatok, futtassa a egyidejű Netsh trace parancsának hello háttér virtuális gép, és hello VNet teszteléshez használt virtuális gép, amíg PsPing futtatni, majd állítsa le a hello Netsh trace. 
  
## <a name="next-steps"></a>Következő lépések

Ha hello előző lépések nem hello probléma megoldásához nyissa meg a [támogatja a jegy](https://azure.microsoft.com/support/options/).

