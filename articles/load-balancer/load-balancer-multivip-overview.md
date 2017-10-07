---
title: "az Azure terheléselosztó virtuális IP-címek aaaMultiple |} Microsoft Docs"
description: "Több virtuális IP-címek az Azure Load Balancer áttekintése"
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: 
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: chkuhtz
ms.openlocfilehash: e186e1248e7d187e7efd0668dc3244465ce3e156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a>Több virtuális IP-cím az Azure Load Balancerhez

Az Azure Load Balancer lehetővé teszi a tooload egyenleg szolgáltatások több portot vagy több IP-címet. Használhatja a nyilvános és a belső terheléselosztó definíciók tooload egyenleg adatfolyamok virtuális gépek csoportja között.

Ez a cikk ismerteti a lehetősége, fontos fogalmakat és korlátozások hello alapjait. Ha csak egy IP-címet kíván tooexpose szolgáltatások egyszerűsített utasításokat talál [nyilvános](load-balancer-get-started-internet-portal.md) vagy [belső](load-balancer-get-started-ilb-arm-portal.md) terheléselosztó konfigurációjában betölteni. Több virtuális IP-címek hozzáadása akkor növekményes tooa egyetlen virtuális IP-konfigurációval. Ebben a cikkben hello fogalmak használva bővítheti egy egyszerűsített konfigurációs bármikor.

Ha az Azure Load Balancer, egy előtér- és a háttér-konfiguráció csatlakoztatott szabályok. hello hello szabály által hivatkozott állapotmintáihoz új használt toodetermine adatfolyamok tooa csomópont küldi hello háttérkészlet. hello előtér által a virtuális IP-cím (VIP), amely egy 3-rekord az IP-cím (nyilvános vagy belső), egy átviteli protokoll (UDP vagy TCP) és a portszám van definiálva. A DIP-t, a egy olyan Azure virtuális hálózati adapter IP-cím van csatolva tooa hello háttérkészletben lévő virtuális gép.

hello a következő táblázatban néhány példa előtér konfigurációkat tartalmazza:

| VIP | IP-cím | Protokoll | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

a táblázat hello négy különböző frontends. Frontends #1, #2 és a #3 egyetlen virtuális IP-címhez több szabályt a rendszer. hello azonos IP-címet használja, de hello port vagy a protokoll nem azonos az összes előtérbeli. Frontends #1 és #4 például több virtuális IP-címek, ahol hello azonos elülső rétegbeli protokoll és port újra felhasználja a rendszer több virtuális IP-címek között.

Az Azure terheléselosztó hello terheléselosztási szabályok definiálása rugalmasságot biztosít. Egy szabály kijelenti, hogyan egy címet és egy hello elülső rétegbeli portja csatlakoztatott toohello rendeltetési cím és port hello háttérkiszolgálón. Hello típusú hello szabályt függ-e szabályok között háttérportokat fel újra. Minden szabály megvan konkrét követelmények, amelyek hatással lehetnek a gazdagép-konfiguráció és a mintavételi megtervezése. A szabályok két típusa van:

1. alapértelmezett szabály hello nem háttérportot újrafelhasználása
2. Ha háttérportokat újrafelhasznált hello fix IP-szabály

Az Azure terheléselosztó lehetővé teszi mindkét-szabályok típusai toomix hello azonos terheléselosztó-konfigurációt. hello terheléselosztót is használhatók egyidejűleg egy adott virtuális Gépet, vagy tetszőleges kombinációját, mindaddig, amíg Ön elfogadják hello megkötések hello szabály. Úgy dönt, a szabály típusát attól függ, hogy az alkalmazás- és hello összetettsége támogatása, hogy a konfigurálás hello követelményeinek. Akkor ki kell értékelnie, mely szabálytípusok ajánlott a forgatókönyvéhez.

További forgatókönyvekben hello alapértelmezett viselkedés kezdve megismerkedhet azt.

## <a name="rule-type-1-no-backend-port-reuse"></a>Szabály típusa #1: nincs háttér port újbóli

![Több virtuális ábra](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Ebben a forgatókönyvben hello előtér virtuális IP-címmel vannak konfigurálva az alábbiak szerint:

| VIP | IP-cím | Protokoll | port |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

hello DIP az hello hello hely bejövő adatfolyam. Hello háttérkészlet, a virtuális gépek mutatja meg egy egyedi portot, a DIP-ről a kívánt hello szolgáltatás. Ez a szolgáltatás nem tartozik hello előtér keresztül egy szabályát leíró definíció beolvasása.

Azt adja meg a két szabályt:

| Szabály | Előtérbeli leképezése | toobackend készlet |
| --- | --- | --- |
| 1 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

teljes leképezését hello Azure Load Balancer jelenleg az alábbiak szerint:

| Szabály | VIP IP-cím | Protokoll | port | Cél | port |
| --- | --- | --- | --- | --- | --- |
| ![A szabály](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP IP-cím |80 |
| ![A szabály](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP IP-cím |81 |

Minden egyes szabály a folyamat a cél IP-cím és a célport egyedi kombinációját kell eredményez. Különböző hello célport hello folyamat, amelyet több szabály által biztosított adatfolyamok toohello azonos DIP eltérő portokon.

Állapotteljesítmény mindig irányított toohello DIP-t a virtuális gépek találhatók. Győződjön meg arról, hogy a mintavételi tükrözi hello hello VM állapotának.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Szabály típusa #2: háttér port újbóli fix IP-Címek használatával

Az Azure Load Balancer hello rugalmasságot biztosít tooreuse hello elülső rétegbeli portot több virtuális IP-címek függetlenül használt hello szabálytípus között. Emellett egyes alkalmazás-forgatókönyvek inkább, vagy hello azonos toobe több alkalmazáspéldányt a hello háttérkészletben lévő egyetlen virtuális gép által használt port szükséges. Például a port újbóli magában foglalhatja a fürtözése a magas rendelkezésre állású, hálózati, a virtuális készülékek, és adjon meg több TLS végpont ismételt titkosítás nélkül.

Ha több szabályok között tooreuse hello háttérportot, engedélyeznie kell fix IP-Címek hello szabályát leíró definíció beolvasása.

Lebegőpontos IP egy része, mint a közvetlen kiszolgálói vissza (DSR) néven ismert. DSR két részből áll: a folyamat-topológia és egy IP-cím a leképezési séma. A platform szintjén Azure Load Balancer mindig működik, függetlenül attól, hogy a fix IP-Címek engedélyezve van-e vagy sem DSR folyamata topológiában. Ez azt jelenti, hogy hello kimenő része a folyamat mindig megfelelően egy átírt tooflow közvetlenül hátsó toohello forrása.

Hello alapértelmezett szabály típusa, az Azure közzététele egy hagyományos terheléselosztási IP cím leképezési séma könnyű használatra. Lehetővé teszi fix IP-Címek hello IP-cím hozzárendelés séma tooallow további rugalmasságot biztosít, amint azt alább módosítja.

hello a következő ábra szemlélteti ezt a konfigurációt:

![Több virtuális ábra](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Ebben a forgatókönyvben minden virtuális gép hello háttérkészlet három hálózati adapterrel rendelkezik:

* DIP: virtuális hálózati Adapterhez társított hello VM csomag (IP-konfiguráció Azure hálózati erőforrás)
* Vip1-en: egy visszacsatolási belül a vendég operációs rendszer, amely vip1-en az IP-címmel van konfigurálva
* VIP2: egy visszacsatolási belül a vendég operációs rendszer, amely a VIP2 IP-címmel van konfigurálva

> [!IMPORTANT]
> hello konfigurációs hello logikai felületek hello vendég operációs rendszer belül történik. Ez a konfiguráció nem végre vagy Azure által kezelt. E beállítások nélkül hello szabályok nem működnek. Állapotfigyelő mintavételi definíciók hello DIP-t a virtuális gép hello használata helyett logikai VIP hello. Ezért a szolgáltatást biztosítania kell mintavételi válaszok hello szolgáltatás felajánlott hello állapotát tükrözi hello logikai VIP DIP porton.

Tegyük fel hello azonos előtérbeli konfigurációja mint hello előző forgatókönyv esetén:

| VIP | IP-cím | Protokoll | port |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Azt adja meg a két szabályt:

| Szabály | Előtérbeli leképezése | toobackend készlet |
| --- | --- | --- |
| 1 |![A szabály](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) (A VM1 és vm2 virtuális gépnek) VIP1:80 |
| 2 |![A szabály](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![háttér](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) (A VM1 és vm2 virtuális gépnek) VIP2:80 |

a következő táblázat hello hello teljes leképezés hello terheléselosztó mutatja:

| Szabály | VIP IP-cím | Protokoll | port | Cél | port |
| --- | --- | --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |ugyanaz, mint VIP (65.52.0.1) |ugyanaz, mint VIP (80-as) |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |ugyanaz, mint VIP (65.52.0.2) |ugyanaz, mint VIP (80-as) |

hello cél hello bejövő folyamata az hello VIP-cím szerepel a virtuális gép hello hello visszacsatolási. Minden egyes szabály a folyamat a cél IP-cím és a célport egyedi kombinációját kell eredményez. Különböző hello cél IP-cím alapján hello folyamata, port újbóli kiszolgálón lehetőség hello azonos virtuális gép. A szolgáltatás elérhetővé toohello terheléselosztó szerint toohello VIP tartozó IP-cím és port hello megfelelő visszacsatolási kötés.

Figyelje meg, hogy ez a példa nem változtatja meg a célport hello. Annak ellenére, hogy ez egy webfarmos fix IP-Címek, Azure Load Balancer is támogatja a szabály toorewrite hello háttér célport és toomake azt hello előtér célport eltérő.

hello fix IP-Címek szabálytípus hello foundation több terhelés terheléselosztó konfigurációs minták. Egy példa, hogy jelenleg rendelkezésre áll: hello [több figyelők az SQL AlwaysOn](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfigurációs. Adott idő alatt azt fogja a dokumentum több forgatókönyvekben.

## <a name="limitations"></a>Korlátozások

* Több virtuális IP-konfiguráció csak az infrastruktúra-szolgáltatási virtuális gépek támogatottak.
* Hello fix IP-szabállyal az alkalmazást kell használnia hello DIP kimenő forgalom. Ha az alkalmazás hello visszacsatolási hello vendég operációs rendszer beállított toohello virtuális IP-címével van kötve, majd SNAT nem érhető el toorewrite hello kimenő folyam és hello folyamat sikertelen lesz.
* Nyilvános IP-címek számlázási hatással. További információkért lásd: [IP-cím díjszabása](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Érvényes előfizetési korlátozásait. További információkért lásd: [szolgáltatási korlátait](../azure-subscription-service-limits.md#networking-limits) részleteiről.
