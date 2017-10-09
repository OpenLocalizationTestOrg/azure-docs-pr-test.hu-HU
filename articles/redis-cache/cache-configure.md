---
title: Azure Redis Cache aaaHow tooconfigure |} Microsoft Docs
description: "Az Azure Redis Cache hello alapértelmezett Redis konfigurációjának megértéséhez, valamint megtudhatja, hogyan tooconfigure az Azure Redis gyorsítótár-példányokon"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: sdanie
ms.openlocfilehash: 46bffb74cdf40e0e0a99c3a83dbe06d6fe1ea65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-azure-redis-cache"></a>Hogyan tooconfigure Azure Redis Cache-gyorsítótár
Ez a témakör ismerteti, hogyan tooreview és frissítés hello konfigurációját az Azure Redis Cache példányt, és magában foglalja az hello alapértelmezett Redis kiszolgáló beállítása az Azure Redis Cache példány.

> [!NOTE]
> Konfigurálásához és használatához prémium gyorsítótár-funkciók további információkért lásd: [hogyan tooconfigure adatmegőrzési](cache-how-to-premium-persistence.md), [hogyan fürtszolgáltatás tooconfigure](cache-how-to-premium-clustering.md), és [hogyan támogatják a virtuális hálózati tooconfigure ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Redis gyorsítótár beállításainak konfigurálása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Az Azure Redis Cache-gyorsítótár beállításai vannak tekinthetők meg és hello konfigurált **Redis Cache** hello segítségével panel **erőforrás menü**.

![Redis gyorsítótár beállításai](./media/cache-configure/redis-cache-settings.png)

Tekintheti meg és konfigurálja a következő beállításokat a hello hello **erőforrás menü**.

* [Áttekintés](#overview)
* [Tevékenységnapló](#activity-log)
* [Hozzáférés-vezérlés (IAM)](#access-control-iam)
* [Címkék](#tags)
* [Problémák diagnosztizálása és megoldása](#diagnose-and-solve-problems)
* [Beállítások](#settings)
    * [Tárelérési kulcsok](#access-keys)
    * [Speciális beállítások](#advanced-settings)
    * [Redis gyorsítótár Advisor](#redis-cache-advisor)
    * [Méretezés](#scale)
    * [Redis foglalásiegység-méret](#cluster-size)
    * [Redis-adatmegőrzés](#redis-data-persistence)
    * [Frissítések ütemezése](#schedule-updates)
    * [Georeplikáció](#geo-replication)
    * [Virtuális hálózat](#virtual-network)
    * [Tűzfal](#firewall)
    * [Tulajdonságok](#properties)
    * [Zárolások feloldása](#locks)
    * [Automatizálási parancsfájl](#automation-script)
* [Felügyeleti](#administration)
    * [Adatok importálása](#importexport)
    * [Adatok exportálása](#importexport)
    * [Újraindítás](#reboot)
* [Figyelés](#monitoring)
    * [Redis metrikák](#redis-metrics)
    * [A riasztási szabályok](#alert-rules)
    * [Diagnosztika](#diagnostics)
* [Támogatási és hibaelhárítási beállítások](#support-amp-troubleshooting-settings)
    * [Erőforrás állapota](#resource-health)
    * [Új támogatási kérelem](#new-support-request)


## <a name="overview"></a>Áttekintés

**Áttekintés** biztosít alapvető információkat, például nevét, a gyorsítótár, a portok, a tarifacsomag, és kijelölt gyorsítótár metrikákat.

### <a name="activity-log"></a>Tevékenységnapló

Kattintson a **tevékenységnapló** tooview műveleteket végezni a gyorsítótárhoz. Is használhatja szűrési tooexpand a nézet tooinclude más erőforrásokat. A vizsgálati naplók munkáról bővebben lásd: [naplózási műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md). Azure Redis Cache események figyelésével kapcsolatos további információkért lásd: [műveletek és a riasztások](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Hozzáférés-vezérlés (IAM)

Hello **hozzáférés-vezérlés (IAM)** szakasz támogatja a szerepköralapú hozzáférés-vezérlést (RBAC) hello Azure portál toohelp szervezetek megfelel az access management igényeik egyszerűen és pontosan. További információkért lásd: [hello Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Címkék

Hello **címkék** szakasz segítséget nyújt az erőforrások rendszerezése. További információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Problémák diagnosztizálása és megoldása

Kattintson a **Diagnosztizálás és problémák megoldására** toobe a gyakori problémák és stratégiák előírt megoldása.



## <a name="settings"></a>Beállítások
Hello **beállítások** szakasz lehetővé teszi a tooaccess, és a gyorsítótár beállításait a következő hello konfigurálása.

* [Tárelérési kulcsok](#access-keys)
* [Speciális beállítások](#advanced-settings)
* [Redis gyorsítótár Advisor](#redis-cache-advisor)
* [Méretezés](#scale)
* [Redis foglalásiegység-méret](#cluster-size)
* [Redis-adatmegőrzés](#redis-data-persistence)
* [Frissítések ütemezése](#schedule-updates)
* [Georeplikáció](#geo-replication)
* [Virtuális hálózat](#virtual-network)
* [Tűzfal](#firewall)
* [Tulajdonságok](#properties)
* [Zárolások feloldása](#locks)
* [Automatizálási parancsfájl](#automation-script)



### <a name="access-keys"></a>Elérési kulcs
Kattintson a **hívóbetűk** tooview vagy újragenerálása hello elérési kulcsainak a gyorsítótárhoz. Ezek a kulcsok tooyour gyorsítótár kapcsolódó hello ügyfelek által használt.

![Redis gyorsítótár elérési kulcsainak](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Speciális beállítások
hello alábbi beállításainak konfigurálása a hello **speciális beállítások** panelen.

* [Hozzáférési portok](#access-ports)
* [Memória házirendek](#memory-policies)
* [Kulcstérértesítések használatával értesítések (Speciális beállítások)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Hozzáférési portok
A nem SSL hozzáférés alapértelmezés szerint le van tiltva az új gyorsítótárakhoz. tooenable hello nem SSL port, kattintson a **nem** a **csak SSL keresztül hozzáférést** a hello **speciális beállítások** panel megnyitásához, és kattintson **mentése**.

![A redis Cache hozzáférési portok](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Memória házirendek
Hello **Maxmemory házirend**, **maxmemory fenntartott**, és **maxfragmentationmemory fenntartott** hello beállítások **speciális beállítások**panel hello memória házirendek hello gyorsítótár konfigurálása.

![Redis gyorsítótár Maxmemory házirend](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory házirend** hello kiürítés irányelvet hello gyorsítótár, és lehetővé teszi a következő kiürítési házirendek hello toochoose:

* `volatile-lru`-Ez az alapértelmezett hello.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

További információ `maxmemory` házirendek, lásd: [kiürítés házirendek](http://redis.io/topics/lru-cache#eviction-policies).

Hello **maxmemory fenntartott** beállítással hello memóriamennyiség (MB), amely nem gyorsítótár műveletek, például a feladatátvétel során replikációs számára van fenntartva. Ha az érték lehetővé teszi toohave Redis server egységesebb amikor változik a terhelés. Ez az érték nagyobb munkaterhelésekhez, amelyek írni a gyakori kell beállítani. Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.

Hello **maxfragmentationmemory fenntartott** beállítással hello memóriamennyiség, amely a memória töredezettségét fenntartott tooaccommodate MB-ban. Ha az érték lehetővé teszi toohave Redis server egységesebb amikor hello gyorsítótár megtelt, vagy a Bezárás toofull és hello töredezettsége arány is nagy. Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.

Egy új foglalás memóriaméretnél kiválasztásakor egy dolog tooconsider (**maxmemory fenntartott** vagy **maxfragmentationmemory fenntartott**) milyen hatással van a módosítás az egy gyorsítótár, amely már fut. nagy mennyiségű adatot. Például ha 49 GB adatot 53 GB gyorsítótár rendelkezik, majd hello foglalási érték too8 GB, ezzel eldobja hello maximális memória hello rendszer too45 GB le. Ha az aktuális `used_memory` vagy a `used_memory_rss` értékek magasabbak hello új 45 GB-os korlátját, majd hello rendszer tooevict adatokat fog rendelkezni, amíg `used_memory` és `used_memory_rss` 45 GB alatt van. A kiürítési növelheti a kiszolgáló terhelés és a memória töredezettsége. További információt a gyorsítótár mérőszámokat például `used_memory` és `used_memory_rss`, lásd: [elérhető és a jelentéskészítés intervallumok](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> Hello **maxmemory fenntartott** és **maxfragmentationmemory fenntartott** beállítások csak érhetők el a Standard és Premium gyorsítótárazza.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Kulcstérértesítések használatával értesítések (Speciális beállítások)
Kulcstérértesítések használatával értesítések konfigurálása a hello redis **speciális beállítások** panelen. Kulcstérértesítések használatával értesítések engedélyezése az ügyfelek tooreceive értesítések bizonyos események megtörténtekor.

![Redis gyorsítótár speciális beállításai](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Kulcstérértesítések használatával értesítéseket és hello **értesítés kulcstérértesítések használatával-események** beállítás csak akkor érhetők Standard és Premium gyorsítótárak esetében.
> 
> 

További információkért lásd: [Redis kulcstérértesítések használatával értesítések](http://redis.io/topics/notifications). Mintakód, lásd: hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) hello fájlban [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis gyorsítótár Advisor
Hello **Redis gyorsítótár Advisor** csempe megjeleníti a gyorsítótár javaslatok. A normál működés során nincsenek ajánlatok jelennek meg. 

![Javaslatok](./media/cache-configure/redis-cache-no-recommendations.png)

Azokat a feltételeket a gyorsítótár például magas memóriahasználat, a hálózati sávszélesség vagy a kiszolgáló terhelését hello műveletek során fordul elő, ha egy riasztás jelenik meg hello **Redis Cache** panelen.

![Javaslatok](./media/cache-configure/redis-cache-recommendations-alert.png)

További információ a hello **javaslatok** panelen.

![Javaslatok](./media/cache-configure/redis-cache-recommendations.png)

A metrikák hello a figyelheti [diagramok figyelési](cache-how-to-monitor.md#monitoring-charts) és [használati diagramok](cache-how-to-monitor.md#usage-charts) hello szakasza **Redis Cache** panelen.

Minden tarifacsomag különböző korlátai ügyfélkapcsolatokat, a memória és a sávszélesség rendelkezik. Ha a gyorsítótár maximális kapacitás metrikákat megközelíti egy huzamosabb ideig keresztül, az ajánlás jön létre. Hello metrikák és korlátai hello vizsgálja felül további információt **javaslatok** eszköz, tekintse meg a következő táblázat hello:

| Redis gyorsítótár metrika | További információ |
| --- | --- |
| A sávszélesség-használat |[Gyorsítótár teljesítmény - rendelkezésre álló sávszélesség](cache-faq.md#cache-performance) |
| Csatlakozott ügyfelek |[Alapértelmezett Redis-kiszolgáló konfiguráció - maxclients](#maxclients) |
| A kiszolgálóterhelés |[Használati diagramok - Redis-kiszolgáló terhelését](cache-how-to-monitor.md#usage-charts) |
| Memóriahasználat |[Gyorsítótár teljesítmény - mérete](cache-faq.md#cache-performance) |

tooupgrade a gyorsítótár kattintson **frissítés most** toochange hello IP-címek és [méretezési](#scale) a gyorsítótárhoz. Tarifacsomag kiválasztásáról további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Méretezés
Kattintson a **méretezési** tooview, vagy módosítsa hello IP-címek a gyorsítótárhoz. A méretezés további információkért lásd: [hogyan tooScale Azure Redis Cache-gyorsítótár](cache-how-to-scale.md).

![Redis gyorsítótár tarifacsomag](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis foglalásiegység-méret
Kattintson a **(előzetes verzió) Redis fürtméret** toochange hello fürtméret futó Premium gyorsítótár fürtözési engedélyezve van.

> [!NOTE]
> Vegye figyelembe, hogy közben hello Azure Redis Cache prémium szint lett tooGeneral rendelkezésre állás érdekében hello fürtméret Redis funkció, amely jelenleg előzetes verzió.
> 
> 

![Redis foglalásiegység-méret](./media/cache-configure/redis-cache-redis-cluster-size.png)

toochange hello fürtméret hello csúszkával, vagy írjon be egy számot 1 és 10 közötti hello **Shard száma** szöveges értéket, majd kattintson **OK** toosave.

> [!IMPORTANT]
> Redis fürtszolgáltatás csak érhető el prémium gyorsítótárak esetében. További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Redis-adatmegőrzés
Kattintson a **Redis-adatmegőrzés** tooenable, letiltása, vagy konfigurálja a prémium szintű gyorsítótár adatainak megőrzését. Azure Redis Cache Redis-adatmegőrzés használatával biztosít [Rekordadatbázis adatmegőrzési](cache-how-to-premium-persistence.md#configure-rdb-persistence) vagy [AOF adatmegőrzési](cache-how-to-premium-persistence.md#configure-aof-persistence).

További információkért lásd: [hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Prémium szintű gyorsítótárak redis-adatmegőrzés csak érhető el. 
> 
> 

### <a name="schedule-updates"></a>Frissítések ütemezése
Hello **frissítések ütemezése** panel lehetővé teszi a toodesignate Redis server frissítései, a gyorsítótár egy karbantartási időszakot. 

> [!IMPORTANT]
> hello karbantartási időszak vonatkozzon, csak tooRedis kiszolgáló, és nem tooany Azure frissíti, vagy toohello hello hello gyorsítótár üzemeltető virtuális gépek operációs rendszerének frissítése.
> 
> 

![Frissítések ütemezése](./media/cache-configure/redis-schedule-updates.png)

toospecify karbantartási időszak, ellenőrizze a szükségeskonfiguráció-hello nap és hello karbantartási időszak kezdő időpontja minden nap adja meg, majd kattintson **OK**. Vegye figyelembe, hogy hello karbantartási ablak időpontja UTC szerint. 

> [!IMPORTANT]
> Hello **frissítések ütemezése** funkciót csak érhető el prémium réteghez gyorsítótárainak. További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - frissítések ütemezése](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Georeplikáció

Hello **georeplikáció** panel lehetővé teszi a csatolás két Premium szint Azure Redis Cache példányt. Egy gyorsítótár hello elsődleges csatolt gyorsítótárat és más, mint hello másodlagos csatolt gyorsítótár hello van kijelölve. hello másodlagos csatolt gyorsítótár csak olvashatóvá válik, és adatok írásbeli toohello elsődleges gyorsítótár toohello másodlagos csatolt gyorsítótár replikálva. Ez a funkció használt tooreplicate a gyorsítótár Azure-régiók közötti lehet.

> [!IMPORTANT]
> **A georeplikáció** lehetőség csak a prémium szintű réteghez gyorsítótárainak. További információt és útmutatást lásd: [hogyan tooconfigure georeplikáció az Azure Redis Cache](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Virtual Network
Hello **virtuális hálózati** szakasz lehetővé teszi a gyorsítótár tooconfigure hello virtuális hálózati beállításait. A prémium szintű gyorsítótár virtuális hálózaton létrehozásával kapcsolatos információkat támogatja, és a beállítások frissítése, lásd: [hogyan tooconfigure a virtuális hálózati támogatása a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Virtuális hálózati beállítások csak érhetők el a prémium szintű gyorsítótárak konfigurált VNET támogató gyorsítótár létrehozása során. 
> 
> 

### <a name="firewall"></a>Tűzfal

Kattintson a **tűzfal** tooview és tűzfalszabályok beállítása a prémium szintű Azure Redis Cache.

![Tűzfal](./media/cache-configure/redis-firewall-rules.png)

A tűzfalszabályok kezdő és záró IP-címtartománnyal rendelkező adhatja meg. Tűzfalszabályok konfigurálásakor csak hello érkező ügyfélkapcsolatokat megadott, az IP-címtartományok toohello gyorsítótár kapcsolódhatnak. Egy tűzfalszabály mentésekor a rendszer nincs rövid késleltetés előtt hello szabály érvényben. Ez a késés van általában kevesebb mint egy perc.

> [!IMPORTANT]
> Azure Redis Cache rendszerek figyelése a kapcsolatok mindig engedélyezettek, még akkor is, ha a tűzfal-szabályok úgy vannak konfigurálva.
> 
> Tűzfalszabályok esetén csak prémium szintű réteghez gyorsítótárainak érhetők el.
> 
> 

### <a name="properties"></a>Tulajdonságok
Kattintson a **tulajdonságok** tooview információt a gyorsítótár, beleértve a gyorsítótár végpontjához hello és portok.

![Redis gyorsítótár tulajdonságai](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Zárolások
Hello **zárolja** szakasz lehetővé teszi a toolock előfizetés, erőforrás vagy az erőforrás tooprevent véletlen törlése vagy a kritikus erőforrásokat módosítása a munkahely más felhasználóinak. További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Automatizálási parancsfájl

Kattintson a **automatizálási parancsfájl** toobuild és a telepített erőforrások a későbbi telepítési sablon exportálása. A sablonok használatának kapcsolatos további információkért lásd: [telepítése Azure Resource Manager-sablonok erőforrások](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Felügyeleti beállítások
hello szolgáltatásbeállításai hello **felügyeleti** szakasz lehetővé teszi a következő felügyeleti feladatok a gyorsítótárhoz tooperform hello. 

![Adminisztráció](./media/cache-configure/redis-cache-administration.png)

* [Adatok importálása](#importexport)
* [Adatok exportálása](#importexport)
* [Újraindítás](#reboot)


### <a name="importexport"></a>Import/Export
Importálási/exportálási során Azure Redis Cache adatok felügyeleti, amely lehetővé teszi által importálás és exportálás a Redis Cache-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótár tooa oldalakra vonatkozó blob egy Azure Storage-fiók hello gyorsítótár tooimport és exportálási adatokat. Importálási/exportálási lehetővé teszi a különböző Azure Redis Cache példányai között toomigrate vagy hello gyorsítótár adatokkal használat előtt.

Importálás használt toobring Redis kompatibilis Rekordadatbázis fájlokat minden futtató bármely felhő és a környezet, beleértve a futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások Redis Redis-kiszolgáló lehet. Adatok importálása egy egyszerűen toocreate egy előre megadott adatokkal gyorsítótár. Hello importálási folyamat során az Azure Redis Cache hello Rekordadatbázis fájlok az Azure storage betölti a memóriába, és, majd a kívánt hello kulcsok hello gyorsítótárba.

Exportálás lehetővé teszi tooexport hello adatok Azure Redis Cache tooRedis kompatibilis Rekordadatbázis fájlokat tárolja. Ez a szolgáltatás toomove adatokat egy Azure Redis Cache példány tooanother vagy tooanother Redis-kiszolgáló is használhatja. VM állomások hello Azure Redis Cache server-példány, és hello fájl kijelölve tárfiók feltöltött toohello hello hello exportálás során ideiglenes fájl jön létre. Hello az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel hello ideiglenes fájl törlődik.

> [!IMPORTANT]
> Prémium szintű réteghez gyorsítótárainak importálási/exportálási csak érhető el. További információt és útmutatást lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Újraindítás
Hello **újraindítás** panel lehetővé teszi a gyorsítótár tooreboot hello csomópontok. Az újraindítás funkció lehetővé teszi, hogy Ön tootest a rugalmasságot az alkalmazás egy gyorsítótár-csomópont hibája esetén.

![Újraindítás](./media/cache-configure/redis-cache-reboot.png)

Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, mely szilánkok hello gyorsítótár tooreboot a választhatja meg.

![Újraindítás](./media/cache-configure/redis-cache-reboot-cluster.png)

tooreboot a gyorsítótár egy vagy több csomópontján válasszon szükséges hello csomópontot, majd kattintson a **újraindítás**. Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, jelölje ki a hello shard(s) tooreboot, és kattintson **újraindítás**. Néhány perc elteltével hello kijelölt csomópont újraindítás, és néhány percen belül újra online állapotba kerülnek.

> [!IMPORTANT]
> Minden tarifacsomagok Újraindítás most érhető el. További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - újraindítás](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Figyelés

Hello **figyelés** szakasz lehetővé teszi a tooconfigure diagnosztikai és a Redis Cache figyelését. Azure Redis Cache-figyelés és diagnosztika további információkért lásd: [hogyan toomonitor Azure Redis Cache-gyorsítótár](cache-how-to-monitor.md).

![Diagnosztika](./media/cache-configure/redis-cache-diagnostics.png)

* [Redis metrikák](#redis-metrics)
* [A riasztási szabályok](#alert-rules)
* [Diagnosztika](#diagnostics)

### <a name="redis-metrics"></a>Redis metrikák
Kattintson a **metrikák Redis** túl[metrikák megtekintése](cache-how-to-monitor.md#view-cache-metrics) a gyorsítótárhoz.

### <a name="alert-rules"></a>A riasztási szabályok

Kattintson a **riasztási szabályok** tooconfigure riasztások a Redis Cache metrikák alapján. További információkért lásd: [riasztások](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Diagnosztika

Gyorsítótár-metrikát a Azure figyelő alapesetben [30 napig tárolja a](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) , majd törölni. toopersist a 30 napnál hosszabb gyorsítótár metrikáját kattintson **diagnosztika** túl[hello tárfiók konfigurálása](cache-how-to-monitor.md#export-cache-metrics) használt toostore gyorsítótár diagnosztika.

>[!NOTE]
>A hozzáadása tooarchiving a gyorsítótár metrikák toostorage is [tooan eseményközpont adatfolyam formájában, vagy küldhet nekik tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

## <a name="support--troubleshooting-settings"></a>Támogatási és hibaelhárítási beállítások
hello szolgáltatásbeállításai hello **támogatási + hibaelhárítási** szakasz a gyorsítótár kapcsolatos problémák megoldása lehetőségeket biztosít.

![Támogatási + hibaelhárítása](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Erőforrás állapota](#resource-health)
* [Új támogatási kérelem](#new-support-request)

### <a name="resource-health"></a>Erőforrás állapota
**Erőforrás állapota** az erőforrás figyeli, és jelzi, hogy ha a várt módon fut.. Hello Azure Resource health service kapcsolatos további információkért lásd: [Azure-erőforrás állapotának áttekintése](../resource-health/resource-health-overview.md).

> [!NOTE]
> Erőforrás állapota jelenleg nem tudja tooreport hello egészségügyi üzemeltetett virtuális hálózatban Azure Redis Cache példány. További információkért lásd: [minden gyorsítótár-funkciók esetén egy virtuális hálózat a gyorsítótárhoz működik?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Új támogatási kérelem
Kattintson a **új támogatja a kérelem** tooopen egy támogatási kérést a gyorsítótárhoz.





## <a name="default-redis-server-configuration"></a>Alapértelmezett Redis-kiszolgálókonfiguráció
Új Azure Redis Cache példány alapértelmezett Redis konfigurációs értékeket a következő hello vannak konfigurálva.

> [!NOTE]
> Ebben a szakaszban hello-beállítások használatával nem lehet megváltoztatni hello `StackExchange.Redis.IServer.ConfigSet` metódust. Ha ezt a módszert nevezik ebben a szakaszban hello parancsok egyikét, egy kivétel hasonló toohello következő fordul elő:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Bármely értékek, amelyek konfigurálhatók, például **maximális memória-házirenddel**, hello Azure-portálon vagy a parancssori felügyeleti eszközök, például az Azure parancssori felület vagy a PowerShell segítségével konfigurálható.
> 
> 

| Beállítás | Alapértelmezett érték | Leírás |
| --- | --- | --- |
| `databases` |16 |hello alapértelmezett számú adatbázis 16, de be lehet állítani egy másik száma alapján hello IP-címek. <sup>1</sup> hello alapértelmezett adatbázis DB 0, kiválaszthatja, hogy egy másik kapcsolati alapját használatával `connection.GetDatabase(dbid)` ahol `dbid` közötti szám `0` és `databases - 1`. |
| `maxclients` |IP-címek hello függ<sup>2</sup> |Hello engedélyezett azonos kapcsolódó ügyfelek maximális száma hello idő. Hello korlát elérését követően a Redis bezárása hello új kapcsolatok egy "elért ügyfelek maximális száma" hibát ad vissza. |
| `maxmemory-policy` |`volatile-lru` |Maxmemory házirend hello beállítás hogyan választja ki a Redis milyen tooremove amikor `maxmemory` (hello hello gyorsítótár létrehozásakor kiválasztott ajánlat hello gyorsítótár) méretet. Azure Redis Cache hello alapértelmezett a beállítás `volatile-lru`, amely hello kulcsok távolítja el a jelszólejárat LRU algoritmus használatával. Ez a beállítás hello Azure-portálon konfigurálható. További információkért lásd: [memória házirendek](#memory-policies). |
| `maxmemory-samples` |3 |toosave memória, a LRU és a minimális TTL algoritmusok is közelítő algoritmusok pontos algoritmusok helyett. Alapértelmezés szerint Redis ellenőrzések három kulcsok és kivételezések hello egy kisebb mostanában használt. |
| `lua-time-limit` |5,000 |Maximális végrehajtási idő ezredmásodpercben Lua parancsfájlra. Hello maximális végrehajtási idő elérésekor a Redis naplózza, hogy egy parancsfájl még végrehajtása után hello maximális engedélyezett idő, és hiba tooreply tooqueries kezdődik. |
| `lua-event-limit` |500 |Parancsfájl esemény sor maximális mérete. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 032mb 8mb 60 0 |hello ügyfél kimeneti puffer korlátok lehetnek ügyfelek, amelyek nem adatok beolvasása hello server elég gyors (gyakori oka az, hogy Pub/Sub ügyfelet nem lehet gyors hello publisher születik őket felhasználni üzenetek) valamilyen okból tooforce leválasztása használt. További információkért lásd: [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>hello korlát `databases` eltér az egyes Azure Redis Cache IP-címek és a gyorsítótár létrehozásakor állítható be. Ha nincs `databases` beállítás van megadva gyorsítótár létrehozása során hello alapértelmezett érték 16.

* Basic és Standard gyorsítótárak
  * C0 csomag (250 MB) gyorsítótár - too16 adatbázisok
  * C1 (1 GB-os) gyorsítótár - too16 adatbázisok
  * C2 csomag (2,5 GB) gyorsítótár - too16 adatbázisok
  * C3 csomag (6 GB) gyorsítótár - too16 adatbázisok
  * C4 (13 GB) gyorsítótár - too32 adatbázisok
  * C5 (26 GB) gyorsítótár - too48 adatbázisok
  * C6 (53 GB) gyorsítótár - too64 adatbázisok
* Prémium szintű gyorsítótárak
  * (6 GB - 60 GB) – P1 too16 adatbázisok
  * (13 GB - 130 GB) – P2 too32 adatbázisok
  * (26 GB - 260 GB) – P3 too48 adatbázisok
  * P4 (53 GB - 530 GB) – too64 adatbázisok
  * Engedélyezve - Redis-fürt minden prémium gyorsítótárak Redis fürt csak 0-adatbázis úgy hello támogatja `databases` korlátozza a prémium szintű gyorsítótár engedélyezve van a Redis-fürt hatékonyan érték 1 és hello [válasszon](http://redis.io/commands/select) parancs nem engedélyezett. További információkért lásd: [van toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Adatbázisokkal kapcsolatos további információkért lásd: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> Hello `databases` beállítás csak a gyorsítótár létrehozása során konfigurált, és csak a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek lehet. Példa konfigurálása `databases` gyorsítótár létrehozása PowerShell használatával, lásd: [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` eltér az egyes Azure Redis Cache tarifacsomagra vált.

* Basic és Standard gyorsítótárak
  * C0 csomag (250 MB) gyorsítótár - too256 kapcsolatok
  * C1 (1 GB-os) gyorsítótár - too1 mentése, 000 kapcsolatok
  * C2 csomag (2,5 GB) gyorsítótár - too2 mentése, 000 kapcsolatok
  * C3 csomag (6 GB) gyorsítótár - too5 mentése, 000 kapcsolatok
  * C4 (13 GB) gyorsítótár - too10 mentése, 000 kapcsolatok
  * C5 (26 GB) gyorsítótár - too15 mentése, 000 kapcsolatok
  * C6 (53 GB) gyorsítótár - too20 mentése, 000 kapcsolatok
* Prémium szintű gyorsítótárak
  * (6 GB - 60 GB) – P1 too7, 500 kapcsolatok mentése
  * (13 GB - 130 GB) – P2 too15, 000 kapcsolatok mentése
  * (26 GB - 260 GB) – P3 too30, 000 kapcsolatok mentése
  * P4 (53 GB - 530 GB) – too40, 000 kapcsolatok mentése

> [!NOTE]
> Miközben lehetővé teszi, hogy minden egyes gyorsítótár méretének *legfeljebb* bizonyos számú kapcsolatok, minden kapcsolat tooRedis rendelkezik terhet társítva. Erre példa ilyen terheléssel lehet Processzor- és memóriahasználatról miatt a TLS/SSL-titkosítást. a megadott gyorsítótárméret hello maximális kapcsolathoz megadott korlátot azt feltételezi, hogy egy némileg betöltött gyorsítótár. Ha a kapcsolat terhelés betöltése *plus* Ügyfélműveletek a terhelés meghaladja a kapacitását hello rendszerhez, hello gyorsítótár kapacitás problémákat tapasztalhat, még akkor is, ha nem lépik túl a gyorsítótár jelenlegi mérete hello hello kapcsolathoz megadott korlátot.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>A parancsok nem támogatott az Azure Redis Cache redis
> [!IMPORTANT]
> Microsoft által felügyelt konfigurálása és kezelése az Azure Redis Cache példányt, mert hello következő parancsok le vannak tiltva. Ha tooinvoke próbálja őket, túl a hasonló hibaüzenetet kap`"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * CONFIG
> * HIBAKERESÉSI
> * ÁTTELEPÍTÉSE
> * MENTÉSE
> * LEÁLLÍTÁS
> * SLAVEOF
> * FÜRT - fürt írási parancsokat le vannak tiltva, de csak olvasható fürt parancsot.
> 
> 

Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis-konzol
Biztonságosan kiadhatja parancsok tooyour Azure Redis Cache példány hello segítségével **Redis konzol**, a hello Azure-portál a gyorsítótár minden csomagban elérhető.

> [!IMPORTANT]
> - hello Redis-konzol nem tud együttműködni [VNET](cache-how-to-premium-vnet.md). Ha a gyorsítótár egy virtuális hálózat része, csak a virtuális hálózat hello ügyfelek hello gyorsítótár férhet hozzá. Konzol Redis fut a helyi böngészőben, amely hello virtuális hálózaton kívül, mert tooyour gyorsítótár nem tud kapcsolódni.
> - Nem minden Redis-parancsok az Azure Redis Cache támogatottak. Le vannak tiltva, az Azure Redis Cache Redis-parancsok listáját, tekintse meg az előző hello [parancsok nem támogatott az Azure Redis Cache Redis](#redis-commands-not-supported-in-azure-redis-cache) szakasz. Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).
> 
> 

tooaccess hello Redis konzol, kattintson a **konzol** a hello **Redis Cache** panelen.

![Redis-konzol](./media/cache-configure/redis-console-menu.png)

tooissue parancsok a gyorsítótárpéldány ellen, egyszerűen típus hello a keresett parancs hello konzolba.

![Redis-konzol](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a>Hello segítségével a prémium konzolon Redis gyorsítótár fürtözött

Ha a gyorsítótár hello Redis-konzol használata a prémium fürtözött, parancsok tooa egyetlen shard hello gyorsítótár adhat ki. tooissue parancs tooa adott shard, először kapcsolódik toohello kívánt shard hello shard objektumválasztó kattintva.

![Redis-konzol](./media/cache-configure/redis-console-premium-cluster.png)

Ha egy kulcsot, amely egy másik shard van tárolva, mint a csatlakoztatott shard hello tooaccess kísérli meg, egy hiba üzenet hasonló toohello a következő üzenet jelenik meg.

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

Hello előző példában shard 1: hello kijelölt shard, de `myKey` található a szilánkcímtárban 0, hello jelöli `(shard 0)` hello hibaüzenet része. Ebben a példában tooaccess `myKey`, jelölje be shard 0 használatával hello shard objektumválasztó, majd a probléma hello szükséges parancsot.


## <a name="move-your-cache-tooa-new-subscription"></a>Helyezze át a gyorsítótár tooa új előfizetés
A gyorsítótár tooa új előfizetés kattintva áthelyezheti **áthelyezése**.

![Helyezze át a Redis gyorsítótár](./media/cache-configure/redis-cache-move.png)

Az erőforrások áthelyezése egy erőforrás csoport tooanother, és egy előfizetés tooanother információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Következő lépések
* További információ a Redis-parancsok használatával: [hogyan futtathatom Redis parancsok?](cache-faq.md#how-can-i-run-redis-commands)

