---
title: "prémium szintű Azure Redis Cache aaaHow tooconfigure adatok megőrzését"
description: "Megtudhatja, hogyan tooconfigure és az adatmegőrzés a Premium szint Azure Redis Cache példány kezelése"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: sdanie
ms.openlocfilehash: 62feb6f5522e0270487f045eb303bf852434143d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-persistence-for-a-premium-azure-redis-cache"></a>Hogyan tooconfigure adatok megőrzését egy prémium szintű Azure Redis Cache
Azure Redis Cache-gyorsítótár mérete és a funkciót, beleértve a prémium réteg szolgáltatások, például a fürtszolgáltatás, az adatmegőrzésre és a virtuális hálózat támogatásának hello választott rugalmasságot biztosítanak, amelyek különböző gyorsítótár ajánlatok rendelkezik. Ez a cikk ismerteti, hogyan tooconfigure adatmegőrzési a egy prémium szintű Azure Redis Cache-gyorsítótár példány.

Más prémium gyorsítótár funkciókról további információért lásd: [bemutatása toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Mi az adatmegőrzés?
[Redis-adatmegőrzés](https://redis.io/topics/persistence) lehetővé teszi a Redis toopersist adataihoz. Pillanatképek és hardver meghibásodása esetén is betölthet, az hello adatok biztonsági másolatát is. Ez a nagyon nagy előnyös használhatóságát Basic vagy Standard csomagra, ahol az összes hello adatok a memóriában tárolja, és esetleges adatvesztés, ha a gyorsítótár-csomópontok nem működnek meghibásodása esetén is lehet. 

Azure Redis Cache Redis adatmegőrzési használatával a következő modellek hello kínálja:

* **Rekordadatbázis adatmegőrzési** -amikor Rekordadatbázis (Redis-adatbázis) adatmegőrzési van konfigurálva, az Azure Redis Cache továbbra is fennáll, a bináris formátumú toodisk egy konfigurálható biztonsági mentési gyakoriság alapján a Redis hello Redis gyorsítótár pillanatképet. Ha egy katasztrofális esemény történik, amely letiltja a hello elsődleges és replika gyorsítótár, hello gyorsítótár újraépíti hello legutóbbi pillanatkép. További tudnivalók hello [előnyeit](https://redis.io/topics/persistence#rdb-advantages) és [hátrányai](https://redis.io/topics/persistence#rdb-disadvantages) Rekordadatbázis adatmegőrzési a.
* **AOF adatmegőrzési** -amikor AOF (hozzáfűzés egyetlen fájl) adatmegőrzési van konfigurálva, Azure Redis Cache minden írási művelet tooa napló mentett legalább egyszer másodpercenként be egy Azure Storage-fiókba menti. Ha egy katasztrofális esemény történik, amely letiltja a hello elsődleges és replika gyorsítótár, hello gyorsítótár újraépíti tárolt hello írási műveletek használatával. További tudnivalók hello [előnyeit](https://redis.io/topics/persistence#aof-advantages) és [hátrányai](https://redis.io/topics/persistence#aof-disadvantages) AOF adatmegőrzési a.

Adatmegőrzési van konfigurálva a hello **új Redis Cache** panel gyorsítótár létrehozása során, és a hello **erőforrás menü** meglévő prémium gyorsítótárazza.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

A prémium tarifacsomag kijelölése után kattintson **Redis-adatmegőrzés**.

![Redis-adatmegőrzés][redis-cache-persistence]

hello hello a következő szakaszban található lépéseket ismerteti hogyan tooconfigure Redis adatmegőrzési az új premium-gyorsítótár. Miután beállította a Redis-adatmegőrzés, kattintson a **létrehozása** az új premium gyorsítótárazza a Redis-adatmegőrzés toocreate.

## <a name="enable-redis-persistence"></a>A Redis-adatmegőrzés engedélyezése

Redis adatmegőrzési engedélyezve van a hello **Redis-adatmegőrzés** panelen válassza ki vagy **Rekordadatbázis** vagy **AOF** megőrzését. Ezen a panelen az új gyorsítótárakhoz érhető hello gyorsítótár létrehozási folyamata során hello előző szakaszban leírtak szerint. Meglévő gyorsítótárak hello **Redis-adatmegőrzés** panel elérése hello **erőforrás menü** a gyorsítótárhoz.

![Redis beállítások][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>Rekordadatbázis adatmegőrzési konfigurálása

tooenable Rekordadatbázis adatmegőrzési, kattintson a **Rekordadatbázis**. Kattintson egy korábban engedélyezett prémium gyorsítótár toodisable Rekordadatbázis adatmegőrzési **letiltott**.

![Redis Rekordadatbázis adatmegőrzési][redis-cache-rdb-persistence]

tooconfigure hello biztonsági mentési időközt, jelölje be a **biztonsági mentési gyakoriság** hello legördülő listából. Választható a **15 perc**, **30 perc**, **60 perc**, **6 óra**, **12 óra**, és **24 óra**. Ezt az időközt elindítja számbavételi után hello korábbi biztonsági mentési művelet sikeresen befejeződött, és lejárta, amikor egy új biztonsági másolat lehet kezdeményezni.

Kattintson a **Tárfiók** tooselect hello tárolási fiók toouse, majd válassza ki bármelyik hello **elsődleges kulcs** vagy **másodlagos kulcs** toouse a hello **tároló Kulcs** legördülő listán. Ki kell választania egy tárolási fiókot a hello hello gyorsítótárába, és ugyanabban a régióban és egy **prémium szintű Storage** fiók használata ajánlott, mivel a prémium szintű storage rendelkezik nagyobb átviteli sebességgel. 

> [!IMPORTANT]
> Ha újragenerálják hello kulcs adatmegőrzési fiókjához, módosítania kell a kívánt kulcsot hello hello **Biztonságitár-kulcs** legördülő listán.
> 
> 

Kattintson a **OK** toosave hello adatmegőrzési konfigurációs.

hello következő biztonsági mentés (vagy az új gyorsítótárakhoz első biztonsági mentés) hello biztonsági mentési gyakoriság időtartam után indítható.

## <a name="configure-aof-persistence"></a>AOF adatmegőrzési konfigurálása

tooenable AOF adatmegőrzési, kattintson a **AOF**. Kattintson egy korábban engedélyezett prémium gyorsítótár toodisable AOF adatmegőrzési **letiltott**.

![Redis AOF adatmegőrzési][redis-cache-aof-persistence]

tooconfigure AOF adatmegőrzési, adjon meg egy **első Tárfiók**. Ezt a tárfiókot kell hello hello gyorsítótárába, és ugyanabban a régióban és egy **prémium szintű Storage** fiók használata ajánlott, mivel a prémium szintű storage rendelkezik nagyobb átviteli sebességgel. Konfigurálhatja egy további nevű tárfiók **második Tárfiók**. Ha egy másik tárolási fiókot van konfigurálva, hello írások toohello replika gyorsítótár írt toothis második tárfiók. Minden egyes konfigurált tárfiók, válassza ki bármelyik hello **elsődleges kulcs** vagy **másodlagos kulcs** a hello toouse **Biztonságitár-kulcs** legördülő listán. 

> [!IMPORTANT]
> Ha újragenerálják hello kulcs adatmegőrzési fiókjához, módosítania kell a kívánt kulcsot hello hello **Biztonságitár-kulcs** legördülő listán.
> 
> 

AOF adatmegőrzési engedélyezve van, amikor az írási műveletek toohello gyorsítótár mentett toohello storage-fiók (vagy ha egy másik tárolási fiókot konfigurált fiókok). A végzetes hibák hello esemény, hogy le mindkét vesz hello elsődleges és replika gyorsítótár hello tárolt AOF napló használt toorebuild hello gyorsítótár.

## <a name="persistence-faq"></a>Adatmegőrzési – gyakori kérdések
hello alábbi lista válaszok toocommonly kérdések Azure Redis Cache adatmegőrzési kapcsolatban.

* [A korábban létrehozott gyorsítótáron adatmegőrzés engedélyezése](#can-i-enable-persistence-on-a-previously-created-cache)
* [Engedélyezhető a hello AOF és Rekordadatbázis adatmegőrzési ugyanannyi időt vesz igénybe?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Adatmegőrzési modellt kell választanom?](#which-persistence-model-should-i-choose)
* [Mi történik, ha szeretnék rendelkezik méretezhető tooa különböző méretét, és egy biztonsági mentési művelet skálázás hello előtt végrehajtott helyreáll?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>Rekordadatbázis adatmegőrzési
* [Módosíthatom hello Rekordadatbázis biztonsági mentés gyakoriságát, hello gyorsítótár létrehozása után?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Miért Ha egy Rekordadatbázis biztonsági mentési gyakoriság 60 perc több mint 60 perc között van biztonsági mentések?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Toohello régi Rekordadatbázis biztonsági mentések mi történik, ha egy új biztonsági másolat legyen?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF adatmegőrzési
* [Mikor kell használni egy másik tárolási fiókot?](#when-should-i-use-a-second-storage-account)
* [AOF adatmegőrzési hatása teljes, a késleltetés vagy a teljesítmény a gyorsítótár nem?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Hogyan távolíthatom hello második tárfiókot?](#how-can-i-remove-the-second-storage-account)
* [Mi az, hogy egy átdolgozás, és hogyan azt befolyásolja a gyorsítótár?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Mit kell várhatnak, amikor a gyorsítótár méretezést, AOF engedélyezve van?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Hogyan vannak rendezve AOF adataimat storage?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>A korábban létrehozott gyorsítótáron adatmegőrzés engedélyezése
Igen, a Redis-adatmegőrzés is konfigurálható gyorsítótár létrehozását, mind a meglévő prémium gyorsítótárak.

### <a name="can-i-enable-aof-and-rdb-persistence-at-hello-same-time"></a>Engedélyezhető a hello AOF és Rekordadatbázis adatmegőrzési ugyanannyi időt vesz igénybe?

Nem, engedélyezheti a csak Rekordadatbázis vagy AOF, de egyszerre csak hello ugyanannyi időt vesz igénybe.

### <a name="which-persistence-model-should-i-choose"></a>Adatmegőrzési modellt kell választanom?

AOF adatmegőrzési menti minden írási tooa naplót, amely néhány hatással van a teljesítmény, Rekordadatbázis képest menti hello alapuló biztonsági mentések adatmegőrzési konfigurált biztonsági mentési időszakban, a teljesítményre gyakorolt minimális hatás mellett. AOF adatmegőrzési akkor válassza, ha elsődleges célja toominimize adatvesztés, és a teljesítmény csökkenése kezelheti a gyorsítótárhoz. Rekordadatbázis adatmegőrzési akkor válassza, ha a gyorsítótár toomaintain optimális átviteli kívánja, de továbbra is szeretné, hogy egy olyan mechanizmus az adat-helyreállítást.

* További tudnivalók hello [előnyeit](https://redis.io/topics/persistence#rdb-advantages) és [hátrányai](https://redis.io/topics/persistence#rdb-disadvantages) Rekordadatbázis adatmegőrzési a.
* További tudnivalók hello [előnyeit](https://redis.io/topics/persistence#aof-advantages) és [hátrányai](https://redis.io/topics/persistence#aof-disadvantages) AOF adatmegőrzési a.

A teljesítményre AOF adatmegőrzési használata esetén további információkért lásd: [Does AOF adatmegőrzési hatása teljes, a késleltetés vagy a teljesítmény a gyorsítótár?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-tooa-different-size-and-a-backup-is-restored-that-was-made-before-hello-scaling-operation"></a>Mi történik, ha szeretnék rendelkezik méretezhető tooa különböző méretét, és egy biztonsági mentési művelet skálázás hello előtt végrehajtott helyreáll?

A Rekordadatbázis és a AOF megőrzéséhez:

* Ha nagyobb méretű tooa rendelkezik méretezhető, akkor ennek nincs hatása.
* Ha Ön rendelkezik méretezhető tooa kisebb méretű, és van egy egyéni [adatbázisok](cache-configure.md#databases) beállítástól, amely érték nagyobb, mint hello [adatbázisok korlát](cache-configure.md#databases) az új méretéhez ezeket az adatbázisokat az adatok visszaállítása nem. További információkért lásd: [a skálázás során érintett beállítás egyéni adatbázisok?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Ha Ön rendelkezik méretezhető tooa kisebb méretet, és nincs elegendő hely a hello hello adatok hello utolsó biztonsági mentés, kulcsok alapján ki lesz zárva: hello visszaállítási folyamat során kisebb méretű toohold, általában használatával hello [allkeys-lru](http://redis.io/topics/lru-cache) kiürítés házirend.

### <a name="can-i-change-hello-rdb-backup-frequency-after-i-create-hello-cache"></a>Módosíthatom hello Rekordadatbázis biztonsági mentés gyakoriságát, hello gyorsítótár létrehozása után?
Igen, módosíthatja a biztonsági mentési gyakorisága hello Rekordadatbázis adatmegőrzési a hello **Redis-adatmegőrzés** panelen. Útmutatásért lásd: [konfigurálása Redis adatmegőrzési](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Miért Ha egy Rekordadatbázis biztonsági mentési gyakoriság 60 perc több mint 60 perc között van biztonsági mentések?
hello Rekordadatbázis adatmegőrzési biztonsági mentési gyakoriság időköz nem indul el, amíg hello korábbi biztonsági mentési folyamat sikeresen befejeződött. Ha egy teljes biztonsági mentés 15 perc toosuccessfully tart hello biztonsági mentési gyakoriság érték 60 perc, hello következő biztonsági mentés nem elindítani hello követő 75 percen hello korábbi biztonsági mentés idején.

### <a name="what-happens-toohello-old-rdb-backups-when-a-new-backup-is-made"></a>Toohello régi Rekordadatbázis biztonsági mentések mi történik, ha egy új biztonsági másolat legyen?
Legutóbbi hello kivételével az összes Rekordadatbázis adatmegőrzési biztonsági automatikusan törlődnek. Ez a törlés nem fordulhat elő, azonnal, de a régebbi biztonsági másolatok nem határozatlan ideig maradnak meg.


### <a name="when-should-i-use-a-second-storage-account"></a>Mikor kell használni egy másik tárolási fiókot?

Ha úgy véli, magasabb, mint a hello gyorsítótáron várt műveletek van egy másik tárolási fiókot kell használatra AOF adatmegőrzési.  Másodlagos tárfiók hello beállítása lehetővé teszi a gyorsítótár nem eléri a tárolási sávszélesség korlátja.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>AOF adatmegőrzési hatása teljes, a késleltetés vagy a teljesítmény a gyorsítótár nem?

AOF adatmegőrzési befolyásolja átviteli által körülbelül 15 – 20 % Ha hello gyorsítótár maximális terhelés alatt (Processzor- és kiszolgáló egyaránt betöltése 90 %-a). Nem szabadna késési problémák hello gyorsítótár esetén ezeken a határokon. Azonban hello gyorsítótár eléri a működés felső korlátjának hamarabb rendelkező AOF engedélyezve van.

### <a name="how-can-i-remove-hello-second-storage-account"></a>Hogyan távolíthatom hello második tárfiókot?

Eltávolíthatja a hello AOF adatmegőrzési másodlagos tárfiók hello második tárfiók toobe hello azonos hello első tárolási fiók beállításával. Útmutatásért lásd: [konfigurálása AOF adatmegőrzési](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Mi az, hogy egy átdolgozás, és hogyan azt befolyásolja a gyorsítótár?

Hello AOF fájl méretének elég nagy, akkor egy átdolgozás automatikusan hello gyorsítótár várólistájára. hello átdolgozás átméretezi hello AOF fájl hello minimálisan szükséges műveletek toocreate hello aktuális adatkészlet szükséges. Újraírások, során várható tooreach teljesítménykorlátok hamarabb különösen nagy adatkészletekkel meghatározásakor. Újraírások elő kisebb gyakran hello AOF fájl nagyobb válik, de ha történik egy jelentős ideig eltarthat.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Mit kell várhatnak, amikor a gyorsítótár méretezést, AOF engedélyezve van?

Ha hello AOF fájl a skálázás hello időpontjában jelentősen nagy, majd várt hello skálázási művelet tootake hosszabb a vártnál, mivel azt fogja betöltődnek hello fájl skálázás befejeződése után.

A méretezés további információkért lásd: [mi történik, ha szeretnék rendelkezik méretezhető tooa különböző méretét, és egy biztonsági mentési művelet skálázás hello előtt végrehajtott helyreáll?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Hogyan vannak rendezve AOF adataimat storage?

A AOF-fájlokban tárolt adatok több lapblobokat tooincrease teljesítményének hello adatok toostorage elmenti egy van felosztva. hello következő táblázatban láthatók az egyes tarifacsomagok hány lapblobokat használnak:

| Premium szintű csomag | Blobok |
|--------------|-------|
| P1           | a shard / 4    |
| P2           | a shard 8    |
| P3           | a shard / 16   |
| P4           | a shard 20   |

Amikor a fürtszolgáltatás engedélyezve van, minden egyes shard hello gyorsítótárában saját rendelkezik a lapblobokat, hello előző táblázat szerint. Például három szilánkok P2 gyorsítótár az AOF fájl elosztása 24 lapblobokat (8 blobok / shard, a 3 szilánkok).

Egy átdolgozás után két készlet AOF fájlok tárolási szerepel. Újraírások hello háttérben történik, és a hozzáfűző toohello első fájlkészlet, amíg set műveletek során hello átdolgozás toohello gyorsítótár küldött hozzáfűzése toohello másik készletben. A biztonsági mentés során hiba esetén újraírások ideiglenesen tárolja, de egy átdolgozás befejeződése után azonnal törlődik.


## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan több premium toouse gyorsítótár szolgáltatásokat.

* [Bevezetés toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
