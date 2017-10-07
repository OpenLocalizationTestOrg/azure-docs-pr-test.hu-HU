---
title: "aaaIntroduction toohello Azure Redis Cache prémium szintjének |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és kezelheti a Redis-adatmegőrzés Redis fürtszolgáltatás és a Premium szint Azure Redis Cache példány hálózatok támogatása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 5b58a03647fbf1198509ac6f1acd04f1b682ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-redis-cache-premium-tier"></a>Bevezetés toohello Azure Redis Cache prémium szintjének
Azure Redis Cache egy elosztott, felügyelt gyorsítótár, amely segít a felső szinten gyors hozzáférést tooyour adatok megadásával jól méretezhető és rugalmas alkalmazásait. 

új, prémium szintű egy vállalati készen áll a réteget, amely tartalmazza az összes hello Standard szintű funkciók és további, például a jobb teljesítmény, a nagyobb munkaterhelésekhez, a vész-helyreállítási, exportálni, és fokozott biztonsági hello. Olvasási toolearn hello prémium gyorsítótár réteg hello további szolgáltatásokról további továbbra is.

## <a name="better-performance-compared-toostandard-or-basic-tier"></a>Jobb teljesítmény képest tooStandard vagy az alapszintű csomag
**Jobb teljesítmény Standard vagy alapszintű rétegben.** Hardver, amely gyorsabb processzorral rendelkezik, és jobb teljesítményt képest toohello alapszintű vagy Standard csomagra lehetővé gyorsítótárakat a hello prémium csomagban vannak telepítve. Prémium szintű réteghez gyorsítótárainak rendelkezik nagyobb átviteli teljesítményt és kisebb késések fordulnak elő. 

**Hello adatátviteli sebességét azonos méretű gyorsítótár értéke magasabb a prémium szintű, összehasonlított tooStandard rétegként.** Hello átviteli sebességgel például egy 53 GB P4 (prémium) gyorsítótár 250 KB-os kérelmek / másodperc, a C6 összehasonlított too150K (normál).

Méret, átviteli sebesség és a prémium szintű gyorsítótárak sávszélesség kapcsolatos további információkért lásd: [Azure Redis Cache – gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis-adatmegőrzés
hello prémium csomagban lehetővé teszi toopersist hello gyorsítótárazott adatokat az Azure Storage-fiók. A Basic vagy Standard-gyorsítótár összes hello adatok csak memóriában van tárolva. Alapul szolgáló infrastruktúra esetén nincs problémák lehetséges adatvesztést is lehet. Azt javasoljuk, hogy hello Redis adatok megőrzését a funkciójával hello Premium szint tooincrease rugalmassági adatvesztés ellen. Azure Redis Cache Rekordadatbázis és AOF (hamarosan elérhető) lehetőséget kínál a [Redis-adatmegőrzés](http://redis.io/topics/persistence). 

Útmutatás az adatmegőrzés konfigurálásához: [hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis-fürt
Ha szeretné toocreate gyorsítótárak 53 GB-nál nagyobb, vagy tooshard adatok több Redis-csomópont között, a fürtözés, a hello prémium csomagban elérhető Redis is használhatja. Minden csomópont elsődleges vagy replika gyorsítótár párból áll a magas rendelkezésre állású Azure kezeli. 

**A redis-fürtszolgáltatás lehetővé teszi maximális méretezés és teljesítmény.** Átviteli sebesség lineárisan növeli, mivel a szilánkok (csomópontok) hello fürt hello értéknek a növelésével. EG. Ha a 10 szilánkok P4 fürt létrehozásakor, akkor hello elérhető átviteli sebesség 250 KB-os * 10 = 2,5 millió kérések száma másodpercenként. Tekintse meg a hello [Azure Redis Cache – gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) méretét, az átviteli sebesség és a sávszélesség a prémium szintű gyorsítótárak kapcsolatos további részletekért.

Fürtszolgáltatás, használatába tooget lásd [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Nagyobb biztonság és elszigeteltség
Hello Basic vagy Standard szint létrehozott gyorsítótárak érhetők el a nyilvános interneten hello. Hozzáférés toohello gyorsítótár korlátozódik hello hozzáférési kulcs alapján. A hello prémium csomagban további biztosítható, hogy csak a megadott hálózatban lévő ügyfelek hozzáférhet hello gyorsítótár. Telepítheti a Redis Cache-ben egy [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). Használhatja a vnet összes hello szolgáltatás, például alhálózatok, hozzáférés-vezérlési házirendeket és egyéb szolgáltatások toofurther korlátozása a hozzáférés tooRedis.

További információkért lásd: [hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/Export
Importálási/exportálási egy Azure Redis Cache-adatok felügyeleti művelet, így tooimport adatok Azure Redis Cache vagy Azure Redis Cache exportálási adatokat importálni, és a prémium szintű gyorsítótár tooa oldalakra vonatkozó blob az Azure Redis Cache-adatbázis (Rekordadatbázis) pillanatkép exportálása Storage-fiók. Ez lehetővé teszi a különböző Azure Redis Cache példányai között toomigrate vagy hello gyorsítótár adatokkal használat előtt.

Importálás használt toobring Redis kompatibilis Rekordadatbázis (oka) t futtató kiszolgálóról, a Redis bármely felhő és a környezet, beleértve a Redis futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások is lehet. Adatok importálása egy egyszerűen toocreate egy előre megadott adatokkal gyorsítótár. Hello importálási folyamat során az Azure Redis Cache hello Rekordadatbázis fájlok az Azure storage betölti a memóriába, és, majd a kívánt hello kulcsok hello gyorsítótárába.

Exportálás lehetővé teszi tooexport hello adataihoz az Azure Redis Cache tooRedis kompatibilis Rekordadatbázis (oka) t. Ez a szolgáltatás toomove adatokat egy Azure Redis Cache példány tooanother vagy tooanother Redis-kiszolgáló is használhatja. VM állomások hello Azure Redis Cache server-példány, és hello fájl kijelölve tárfiók feltöltött toohello hello hello exportálás során ideiglenes fájl jön létre. Hello az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel hello ideiglenes fájl törlődik.

További információkért lásd: [hogyan tooimport adatimportáláshoz és exportál adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Újraindítás
hello prémium csomagban tooreboot lehetővé teszi a gyorsítótár igény egy vagy több csomópontja. Ez lehetővé teszi, hogy tootest hello eseményben rugalmasságot az alkalmazás a hibák. A következő csomópont hello újraindíthatja.

* A gyorsítótár fő csomópont
* Az alárendelt csomópont a gyorsítótár
* A gyorsítótár fő és az alárendelt csomópont
* A prémium szintű gyorsítótár fürtözési használatakor újraindíthatja hello master, az alárendelt vagy mindkét csomópont az egyes szilánkok hello gyorsítótárban

További információkért lásd: [újraindítás](cache-administration.md#reboot) és [újraindítás GYIK](cache-administration.md#reboot-faq).

>[!NOTE]
>Újraindítás funkció engedélyezve van minden Azure Redis Cache-csomagban.
>
>

## <a name="schedule-updates"></a>Frissítések ütemezése
hello ütemezett frissítések funkció lehetővé teszi a gyorsítótár karbantartási időszak toodesignate. Hello karbantartási időszak megadása esetén a Redis-kiszolgáló változásainak végrehajtott ezen időszak alatt történjen. toodesignate karbantartási időszak, válassza ki a kívánt hello nap, és adja meg a hello karbantartási időszak kezdő időpontja minden nap. Vegye figyelembe, hogy hello karbantartási ablak időpontja UTC szerint. 

További információkért lásd: [frissítések ütemezése](cache-administration.md#schedule-updates) és [frissítések ütemezése gyakran ismételt kérdések](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Csak Redis hello ütemezett karbantartási időszak alatt végrehajtott frissítések kiszolgáló. hello karbantartási időszak nem érvényes tooAzure frissítéseket, vagy toohello virtuális gép operációs rendszer frissítése.
> 
> 

## <a name="geo-replication"></a>Georeplikáció

**A georeplikáció** lehetővé teszi a csatolás két Premium szint Azure Redis Cache példányt. Egy gyorsítótár hello elsődleges csatolt gyorsítótárat és más, mint hello másodlagos csatolt gyorsítótár hello van kijelölve. hello másodlagos csatolt gyorsítótár csak olvashatóvá válik, és adatok írásbeli toohello elsődleges gyorsítótár toohello másodlagos csatolt gyorsítótár replikálva. Ez a funkció használt tooreplicate a gyorsítótár Azure-régiók közötti lehet.

További információkért lásd: [hogyan tooconfigure georeplikáció az Azure Redis Cache](cache-how-to-geo-replication.md).


## <a name="tooscale-toohello-premium-tier"></a>tooscale toohello prémium csomagban
tooscale toohello prémium csomagban, egyszerűen válassza ki valamelyik hello prémium csomag szükséges hello **tarifacsomag módosítása** panelen. A gyorsítótár toohello prémium csomagban PowerShell és a parancssori felület használatával is méretezheti. Részletes útmutatásért lásd: [hogyan tooScale Azure Redis Cache-gyorsítótár](cache-how-to-scale.md) és [hogyan tooautomate egy skálázási műveletet](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Következő lépések
Gyorsítótár létrehozásához, és vizsgálja meg hello új prémium szint szolgáltatások.

* [Hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md)
* [Hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache](cache-how-to-premium-vnet.md)
* [Hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md)
* [Hogyan tooimport adatimportáláshoz és exportál adatokat az Azure Redis Cache](cache-how-to-import-export-data.md)
* [Hogyan tooadminister Azure Redis Cache-gyorsítótár](cache-administration.md)

