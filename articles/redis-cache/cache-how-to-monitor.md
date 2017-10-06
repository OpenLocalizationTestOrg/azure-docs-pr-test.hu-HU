---
title: Azure Redis Cache aaaHow toomonitor |} Microsoft Docs
description: "Ismerje meg, hogyan toomonitor az Azure Redis Cache példány hello állapotának és teljesítményének"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: c474d485dfcbb109d5bb634a980f6db080598e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-azure-redis-cache"></a>Hogyan toomonitor Azure Redis Cache-gyorsítótár
Használja az Azure Redis Cache [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) tooprovide a gyorsítótár-példányok számos lehetőség közül választhat. Akkor megtekintheti metrikákat, PIN-kód metrikák diagramok toohello Kezdőpulton, hello dátum és idő tartomány diagramok figyelési testreszabása, hozzáadása és metrikák eltávolítása hello diagramok, és állítson be riasztásokat, bizonyos feltételek teljesülése esetén. Ezek az eszközök akkor toomonitor hello állapotát az Azure Redis Cache példány engedélyezése, és a gyorsítótárazási alkalmazásokat kezeléséhez nyújt segítséget.

Az Azure Redis Cache példány használatával a rendszer gyűjti hello Redis metrikák [információ](http://redis.io/commands/info) parancs körülbelül kétszer / perc és automatikusan tárolt 30 napig (lásd: [gyorsítótár metrikák exportálása](#export-cache-metrics) tooconfigure egy az eltérő megőrzési házirend), hogy hello metrikák diagramok jelenik meg, és a riasztási szabályok szerint értékeli ki. Hello különböző INFO értékeket minden egyes gyorsítótár metrika kapcsolatos további információkért lásd: [elérhető és a jelentéskészítés intervallumok](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

tooview gyorsítótár metrika, [Tallózás](cache-configure.md#configure-redis-cache-settings) tooyour gyorsítótárpéldány a hello [Azure-portálon](https://portal.azure.com).  Azure Redis Cache biztosít néhány beépített diagramok hello **áttekintése** panel és hello **metrikák Redis** panelen. Minden egyes diagram hozzáadásával vagy eltávolításával metrikák és hello jelentési időköz módosítása testreszabhatók.

![Redis metrikák](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Előre konfigurált metrikák diagramok megtekintése

Hello **áttekintése** panelen a következő előre konfigurált figyelési diagramok hello rendelkezik.

* [Figyelési diagramok](#monitoring-charts)
* [Használati diagramok](#usage-charts)

### <a name="monitoring-charts"></a>Figyelési diagramok
Hello **figyelés** hello szakasz **áttekintése** panel **illetve sikertelen**, **lekérdezi és beállítja a**, **kapcsolatok**, és **teljes parancsok** diagramokat.

![Figyelési diagramok](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Használati diagramok
Hello **használati** hello szakasz **áttekintése** panel **kiszolgálóterhelés Redis**, **memóriahasználat**, **hálózati sávszélesség **, és **CPU-használat** diagramokat, valamint a is hello **tarifacsomag** hello gyorsítótár-példányhoz.

![Használati diagramok](./media/cache-how-to-monitor/redis-cache-usage-part.png)

Hello **tarifacsomag** megjeleníti hello gyorsítótár árképzési réteg, és túl használható[méretezési](cache-how-to-scale.md) hello gyorsítótár tooa másik tarifacsomagra vált.

## <a name="view-metrics-with-azure-monitor"></a>Az Azure-figyelő nézet metrikák
tooview Redis metrikákat, és hozzon létre egyéni diagramok Azure figyelővel kattintson **metrikák** a hello **erőforrás menü**, és szabja testre a diagram kívánt hello mérőszámainkat, a jelentési időszakban, a diagram típusát, és További.

![Redis metrikák](./media/cache-how-to-monitor/redis-cache-monitor.png)

Azure-figyelővel metrikák használatával kapcsolatos további információkért lásd: [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Gyorsítótár metrikák exportálása
Gyorsítótár-metrikát a Azure figyelő alapesetben [30 napig tárolja a](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) , majd törölni. toopersist a gyorsítótár metrikákat 30 napnál hosszabb ideig is [tárfiók kijelölése](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) , és adja meg a **megőrzési (nap)** -házirend a gyorsítótár metrikákat. 

a tárfiók a gyorsítótár metrikáihoz tooconfigure:

1. Kattintson a **diagnosztika** a hello **erőforrás menü** a hello **Redis Cache** panelen.
2. Kattintson a **a**.
3. Ellenőrizze **tooa tárfiók archiválására**.
4. Mely toostore hello gyorsítótár metrikák hello storage-fiók kiválasztása
5. Hello ellenőrizze **1 perc** jelölőnégyzetet, és adja meg egy **megőrzés (nap)** házirend. Ha nem szeretné, hogy bármely adatmegőrzési tooapply, és nem tartja megőrizhetők az adatok, **megőrzés (nap)** túl**0**.
6. Kattintson a **Save** (Mentés) gombra.

![Redis diagnosztika](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>A hozzáadása tooarchiving a gyorsítótár metrikák toostorage is [tooan eseményközpont adatfolyam formájában, vagy küldhet nekik tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

tooaccess a metrikákat, megtekintheti azokat a hello cikkben korábban leírt Azure-portálon, és is elérheti őket hello segítségével [Azure figyelő metrikák REST API](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

> [!NOTE]
> Storage-fiókok módosítása esetén a korábban megadott hello tárfiók hello adatok marad letölthető, de azt hello Azure-portál nem jelenik meg.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Elérhető és a jelentéskészítés időközök
Gyorsítótár metrikák használatával több jelentéskészítési időközök, beleértve a jelentett **óránként túlra**, **Ma**, **elmúlt hét**, és **egyéni**. Hello **metrika** panel minden metrikákat a diagramon az hello átlagos, minimális és maximális értékek egyes mérőszám hello diagram jeleníti meg, és néhány metrikákat jeleníti meg a jelentési időköz hello összesen. 

Mindegyik metrikát két verzióját tartalmazza. Még egy metrika méri a teljesítményt hello teljes gyorsítótárat, valamint a gyorsítótárak használó [Fürtszolgáltatás](cache-how-to-premium-clustering.md), egy második verziója, amely tartalmazza a hello metrika `(Shard 0-9)` hello neve intézkedések teljesítményét a gyorsítótár egy egyetlen szilánkcímtárban. Például ha a gyorsítótár tartalmaz 4 szilánkok `Cache Hits` hello teljes összeg hello teljes gyorsítótár, a találatok és `Cache Hits (Shard 3)` csak hello találatok, hogy a shard hello gyorsítótár a rendszer.

> [!NOTE]
> Akkor is, ha hello gyorsítótár üresjáratban a csatlakoztatott aktív ügyfél alkalmazások nélkül, néhány gyorsítótár tevékenységgel, például egy csatlakoztatott ügyfelek, a memóriafelhasználásról és a végrehajtott műveletek jelenhet meg. Ez a tevékenység nem jelent problémát egy Azure Redis Cache példány hello művelet során.
> 
> 

| Metrika | Leírás |
| --- | --- |
| Találatot eredményező gyorsítótárbeli kereséseinek |sikeres kulcs keresések hello megadott jelentéskészítési időszakban hello száma. Ez túl leképezi`keyspace_hits` a hello Redis [INFO](http://redis.io/commands/info) parancsot. |
| Gyorsítótár-tévesztései |nem sikerült kulcs keresések hello megadott jelentéskészítési időszakban hello száma. Ez túl leképezi`keyspace_misses` a hello Redis INFO parancs. Gyorsítótárbeli nem feltétlenül jelenti azt hello gyorsítótárával probléma van. Például gyorsítótár-tartalékoljon programozási mintát hello használata esetén az alkalmazás jelenjenek meg első elemének hello gyorsítótár. Ha hello elem nem létezik (gyorsítótár-tévesztései), a hello elemet hello adatbázisból beolvasott, és toohello gyorsítótár hozzáadni a következő alkalommal. Gyorsítótárbeli a gyorsítótár-tartalékoljon programozási mintát hello ezek normál viselkedése. Ha gyorsítótárbeli hello száma nagyobb, mint várható, vizsgálja meg, amely feltölti és olvassa be az hello gyorsítótár hello alkalmazáslogikát. Ha elemek vannak eltávolítandó hello gyorsítótárból toomemory terhelés miatt ezt követően előfordulhat, hogy néhány gyorsítótárbeli, de a memóriaigény jobb metrika toomonitor lenne `Used Memory` vagy `Evicted Keys`. |
| Csatlakozott ügyfelek |kapcsolatok toohello ügyfélgyorsítótár hello megadott jelentéskészítési időszakban hello száma. Ez túl leképezi`connected_clients` a hello Redis INFO parancs. Egyszer hello [kapcsolathoz megadott korlátot](cache-configure.md#default-redis-server-configuration) elérésekor a következő kapcsolódási kísérletekkel toohello gyorsítótár sikertelen lesz. Megjegyzés: Ha nincs aktív ügyfélalkalmazást, nem kell lenniük néhány csatlakoztatott ügyfelek toointernal folyamatok és kapcsolatok miatt. |
| Eltávolított kulcsok |hello során hello hello gyorsítótárból kizárt elemek száma a megadott jelentési időköz esedékes toohello `maxmemory` korlátot. Ez túl leképezi`evicted_keys` a hello Redis INFO parancs. |
| Lejárt kulcsok |hello elemszáma lejárt hello gyorsítótárból hello megadott jelentéskészítési időszakban. Ez az érték túl leképezhető`expired_keys` a hello Redis INFO parancs. |
| Összes kulcs  | hello hello gyorsítótárban hello túli jelentési időszak során kulcsok maximális számát. Ez túl leképezi`keyspace` a hello Redis INFO parancs. A gyorsítótárak fürtözési engedélyezve van, a metrikák rendszer alapjául szolgáló hello tooa korlátozása miatt az összes kulcs hello hello shard, amelyekről kulcsok maximális száma hello hello jelentési időszak során a kulcsok maximális számát adja vissza.  |
| Beolvasása |get műveletek hello gyorsítótárból hello megadott jelentéskészítési időszakban hello száma. Ez az érték hello Redis adatai közötti értéket hello következő hello összege minden parancs: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, és `cmdstat_getrange`, és egyenértékű toohello összege találatot eredményező gyorsítótárbeli kereséseinek és a gyorsítótárbeli sikertelen keresések hello jelentési időszak során. |
| A kiszolgálóterhelés redis |mely hello Redis-kiszolgáló feldolgozását, és nem vár a tétlen üzenetek ciklusok hello százalékos aránya Ha ez a számláló azt jelenti, hogy hello Redis-kiszolgáló elérte a teljesítményének felső határát, és nem tudja feldolgozni a hello CPU 100 eléri a gyorsabban működnek. Ha nagy a Redis kiszolgálóterhelés majd látni fogja hello ügyfél időtúllépési kivétel. Ebben az esetben érdemes vertikális felskálázásával vagy az adatok particionálása be több gyorsítótárában. |
| Beállítása |a megadott jelentési időköz set műveletek toohello gyorsítótár során hello hello száma. Ez az érték hello Redis adatai közötti értéket hello következő hello összege minden parancs: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , és `cmdstat_setnx`. |
| Összes művelet |a megadott jelentési időköz hello hello hello gyorsítótár-kiszolgáló által feldolgozott parancsok teljes száma. Ez az érték túl leképezhető`total_commands_processed` a hello Redis INFO parancs. Pub/sub kizárólag az Azure Redis Cache használata esetén nem lesz metrikák hiányában a `Cache Hits`, `Cache Misses`, `Gets`, vagy `Sets`, de nem lesznek `Total Operations` hello gyorsítótár-használati pub/sub műveletek tükröző metrikák. |
| Használt memória |hello mennyisége MB-gyorsítótár hello kulcs/érték párok hello során használt a gyorsítótár-memória megadott jelentési időköz. Ez az érték túl leképezhető`used_memory` a hello Redis INFO parancs. Metaadatok vagy töredezettséget nem tartalmaz. |
| Használt memória RSS |hello mennyisége MB-ban hello során használt a gyorsítótár-memória megadott jelentéskészítési időszakban, beleértve a töredezettséget és a metaadatok. Ez az érték túl leképezhető`used_memory_rss` a hello Redis INFO parancs. |
| CPU |hello CPU-felhasználás hello Azure Redis Cache kiszolgáló százalékos hello során megadott jelentési időköz. Ez az érték leképezhető toohello operációs rendszer `\Processor(_Total)\% Processor Time` teljesítményszámláló. |
| Gyorsítótár olvasása |hello hello gyorsítótár megabájtban (MB/s) másodpercenként hello során adatsorból beolvasott adatok meghatározott jelentési időköz. Ez az érték hello hálózati kártyák hello virtuális gépet, amely hello gyorsítótár futtatja, és nem a megadott Redis-t támogató származik. **Ezt az értéket használják ezt a gyorsítótárat toohello sávszélesség felel meg. Ha a kiszolgáló oldalán hálózati sávszélesség korlátja figyelmeztetéseket tooset használni szeretne, majd létre kell hoznia azt ez `Cache Read` számláló. Lásd: [ezt a táblázatot](cache-faq.md#cache-performance) a hello figyelhető meg különböző árképzési szinteket, és méretű gyorsítótár sávszélesség korlátja.** |
| Gyorsítótár-írási |hello toohello gyorsítótár mérete (MB) hello megadott jelentéskészítési időszakban (MB/s) másodpercenként írt adatok mennyiségét. Ez az érték hello hálózati kártyák hello virtuális gépet, amely hello gyorsítótár futtatja, és nem a megadott Redis-t támogató származik. Ez az érték toohello hálózati sávszélesség toohello gyorsítótár hello ügyfélről küldött adatok felel meg. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Riasztások
Konfigurálhat tooreceive metrikák és tevékenység naplók alapján. Az Azure a figyelő lehetővé teszi egy riasztási toodo hello követően amikor elindítja a tooconfigure:

* E-mail értesítés küldése
* A webhook hívása
* Egy Azure logikai alkalmazás meghívása

tooconfigure riasztási szabályok a gyorsítótárhoz, kattintson a **riasztási szabályok** a hello **erőforrás menü**.

![Figyelés](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Konfigurálásával és riasztások használatával kapcsolatos további információkért lásd: [áttekintése a riasztások](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Tevékenységnaplók
Tevékenységi naplóit adja meg az Azure Redis Cache példányt a végrehajtott műveletek hello betekintést. Azt korábban hívták "naplófájlok" vagy "működési logs". Tevékenység-naplók segítségével meghatározhatja hello "mi, ki, és mikor" az összes írási műveleteket (PUT, POST, Törlés), az Azure Redis Cache példány végzett. 

> [!NOTE]
> Tevékenység naplói nem tartalmazzák (GET) olvasási műveletek.
>
>

tooview tevékenységi naplóit a gyorsítótárhoz, kattintson a **tevékenységi naplóit** a hello **erőforrás menü**.

Tevékenységi naplóit kapcsolatos további információkért lásd: [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).











