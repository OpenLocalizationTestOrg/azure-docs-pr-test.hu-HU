---
title: "az Azure Redis Cache georeplikáció aaaHow tooconfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooreplicate az Azure Redis Cache-példányok földrajzi régiók között."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: sdanie
ms.openlocfilehash: edcd6f202b51055d1a4e47ecaf11f9977d50aa81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-geo-replication-for-azure-redis-cache"></a>Hogyan tooconfigure georeplikáció az Azure Redis Cache

A georeplikáció lehetővé teszi a csatolás két Premium szint Azure Redis Cache példányt. Egy gyorsítótár hello elsődleges csatolt gyorsítótárat és más, mint hello másodlagos csatolt gyorsítótár hello van kijelölve. hello másodlagos csatolt gyorsítótár csak olvashatóvá válik, és adatok írásbeli toohello elsődleges gyorsítótár toohello másodlagos csatolt gyorsítótár replikálva. Ez a funkció használt tooreplicate a gyorsítótár Azure-régiók közötti lehet. Ez a cikk ismerteti a Premium szint Azure Redis Cache-példányok útmutató tooconfiguring a georeplikáció.

## <a name="geo-replication-prerequisites"></a>A georeplikáció Előfeltételek

georeplikálási tooconfigure két gyorsítótárak, a következő előfeltételek hello közötti kell teljesülniük:

- Mindkét gyorsítótárak kell [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.
- Mindkét gyorsítótárak hello kell lennie ugyanazon Azure-előfizetésből.
- hello másodlagos csatolt gyorsítótár kell lennie vagy hello azonos árképzési szint vagy hello elsődleges társított gyorsítótár-nál nagyobb tarifacsomagot.
- Ha elsődleges csatolt gyorsítótár hello fürtszolgáltatás engedélyezve van, hello másodlagos csatolt gyorsítótár kell rendelkeznie, fürtszolgáltatási hello engedélyezett száma azonos szilánkok hello elsődleges csatolt gyorsítótár.
- Mindkét gyorsítótárak kell létrehozni és futó állapotban.
- Adatmegőrzési nem engedélyezhetők vagy gyorsítótár.
- A georeplikáció ugyanazt a virtuális Hálózatot támogatott hello-gyorsítótárak közötti. A más Vnetekről-gyorsítótárak közötti georeplikáció is támogatott, mindaddig, amíg a két hello Vnetek úgy, hogy a hello Vnetek erőforrás-e képes tooreach vannak konfigurálva másik TCP-kapcsolatokon keresztül.

A georeplikáció konfigurálása után hello következő korlátozások vonatkoznak tooyour csatolt gyorsítótár pár:

- hello másodlagos csatolt gyorsítótár csak olvasható; érheti el abból, de bármilyen adat tooit nem írható. 
- Minden adat hello másodlagos csatolt gyorsítótárában hiányában korábban hello hivatkozás eltávolítása. Hello georeplikáció ezt követően távolítja el azonban, ha a hello replikált adatok marad hello másodlagos csatolt gyorsítótárában.
- Nem indítható el egy [művelet skálázás](cache-how-to-scale.md) vagy gyorsítótár vagy [hello száma szilánkok megváltoztatása](cache-how-to-premium-clustering.md) Ha hello gyorsítótár fürtszolgáltatás engedélyezve van.
- Nem engedélyezi az adatmegőrzést vagy gyorsítótár.
- Használhat [exportálása](cache-how-to-import-export-data.md#export) vagy gyorsítótárával, de csak akkor [importálási](cache-how-to-import-export-data.md#import) be elsődleges hello társított gyorsítótár.
- Csatolt gyorsítótár vagy hello tartalmazó erőforráscsoportot, amíg el nem távolítja hello georeplikációs hivatkozást nem törölhető. További információkért lásd: [miért volt hello művelet sikertelen, ha a csatolt gyorsítótár toodelete próbáltam?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Ha két gyorsítótárak hello különböző régiókban, a hálózati kilépő díjak érvényesek toohello adatok régiók toohello másodlagos csatolt gyorsítótár replikálódnak. További információkért lásd: [mennyi biztosítja költség tooreplicate az adatok Azure-régiók között?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Nincs olyan automatikus feladatátvétel toohello másodlagos csatolt gyorsítótár Ha hello elsődleges gyorsítótár (és annak replikája) leáll. Rendelés toofailover ügyfélalkalmazások toomanually eltávolítása hello georeplikációs hivatkozást kell, és pont hello ügyfél alkalmazások toohello gyorsítótár, amely korábban hello másodlagos csatolt gyorsítótár. További információkért lásd: [hogyan toohello másodlagos csatolt gyorsítótár feladatátvételét működik?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>A georeplikáció hivatkozás hozzáadása

1. két premium gyorsítótárazza a együtt georeplikáció, toolink kattintson **georeplikáció** hello erőforrás menüből szándék szerint hello elsődleges kapcsolódó hello gyorsítótár gyorsítótárba, és kattintson **gyorsítótár replikációs hivatkozás hozzáadása**a hello **georeplikáció** panelen.

    ![Hivatkozás hozzáadása](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Kattintson a kívánt hello másodlagos gyorsítótárába hello hello neve **kompatibilis gyorsítótárak** listája. Ha a kívánt gyorsítótár hello listában nem látható, ellenőrizze, hogy hello [georeplikáció Előfeltételek](#geo-replication-prerequisites) hello szükséges-e a másodlagos gyorsítótár számára. toofilter hello gyorsítótárak régiónként, kattintson a kívánt régiót hello hello térkép toodisplay csak azokat a hello gyorsítótárazza a **kompatibilis gyorsítótárak** listája.

    ![A georeplikáció kompatibilis gyorsítótárak](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Hello hello másodlagos gyorsítótár folyamat vagy nézet adatainak linking hello helyi menü használatával is kezdeményezheti.

    ![A georeplikáció a helyi menü](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Kattintson a **hivatkozás** toolink két gyorsítótárak hello együtt, és hello replikáció megkezdéséhez.

    ![Hivatkozás gyorsítótárak](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Hello replikációs folyamat előrehaladását hello megtekintheti a hello **georeplikáció** panelen.

    ![Hivatkozási állapota](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Hello linking hello állapotát is megtekintheti **áttekintése** mindkét hello elsődleges és másodlagos gyorsítótár paneljét.

    ![Gyorsítótár állapota](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Hello replikációs eljárás végrehajtása után hello **hivatkozás állapota** túl változik**sikeres**.

    ![Gyorsítótár állapota](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Hello folyamat linking, során hello elsődleges csatolt gyorsítótár használható marad, de hello másodlagos csatolt gyorsítótár nem érhető el, csak hello linking folyamat befejeződése után.

## <a name="remove-a-geo-replication-link"></a>A georeplikáció hivatkozás eltávolítása

1. tooremove hello hivatkozás között két és a leállítási georeplikáció, kattintson a **gyorsítótárak leválasztása** a hello **georeplikáció** panelen.
    
    ![Gyorsítótárak leválasztása](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Hello csatolás megszüntetése folyamat befejezése után hello másodlagos gyorsítótár mindkét olvasása és írása.

>[!NOTE]
>Hello georeplikáció hivatkozás eltávolítása után hello hello másodlagos gyorsítótár hello elsődleges csatolt gyorsítótárában marad, a replikált adatokat.
>
>

## <a name="geo-replication-faq"></a>A georeplikáció – gyakori kérdések

- [A georeplikáció használható Standard vagy a Basic szint gyorsítótárával?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Nem használható hello csatolás és a kapcsolat megszüntetése folyamat során a gyorsítótár?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Képes vagyok összeköt két kettőnél több gyorsítótárak?](#can-i-link-more-than-two-caches-together)
- [Két, különböző Azure-előfizetések a gyorsítótárak is kapcsolhat?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [A különböző méretű két gyorsítótárak is kapcsolhat?](#can-i-link-two-caches-with-different-sizes)
- [A georeplikáció használata telepítése fürtözött környezetben?](#can-i-use-geo-replication-with-clustering-enabled)
- [Használhatom a georeplikáció a gyorsítótárak a VNETEN belül?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Használhatja a PowerShell vagy Azure CLI toomanage georeplikáció?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Nem mennyibe tooreplicate az adatok Azure-régiók között?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Miért volt sikertelen a hello művelet közben toodelete a csatolt gyorsítótár?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Milyen régióban kell használni a másodlagos csatolt gyorsítótár?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Hogyan működik a toohello másodlagos csatolt gyorsítótár feladatátvételét?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>A georeplikáció használható Standard vagy a Basic szint gyorsítótárával?

A georeplikáció nem, a prémium szintű réteghez gyorsítótárainak csak érhető el.

### <a name="is-my-cache-available-for-use-during-hello-linking-or-unlinking-process"></a>Nem használható hello csatolás és a kapcsolat megszüntetése folyamat során a gyorsítótár?

- Ha két gyorsítótárak összekapcsoló a georeplikációért, hello elsődleges csatolt gyorsítótár használható marad, de hello másodlagos csatolt gyorsítótár nem érhető el, hello linking folyamat befejezéséig.
- Ha eltávolít két-gyorsítótárak közötti hello georeplikációs hivatkozást, mindkét gyorsítótárak elérhetők maradnak.

### <a name="can-i-link-more-than-two-caches-together"></a>Képes vagyok összeköt két kettőnél több gyorsítótárak?

Nem, georeplikáció használata esetén is csak összeköt két gyorsítótárát.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Két, különböző Azure-előfizetések a gyorsítótárak is kapcsolhat?

Egyetlen, mindkét gyorsítótárak hello kell lennie ugyanazon Azure-előfizetésből.

### <a name="can-i-link-two-caches-with-different-sizes"></a>A különböző méretű két gyorsítótárak is kapcsolhat?

Igen, mindaddig, amíg hello másodlagos csatolt gyorsítótár mérete nagyobb, mint hello elsődleges csatolt gyorsítótár.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>A georeplikáció használata telepítése fürtözött környezetben?

Igen, mindaddig, amíg mindkét gyorsítótárak rendelkezik hello szilánkok azonos számú.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Használhatom a georeplikáció a gyorsítótárak a VNETEN belül?

Igen, a Vnetek-gyorsítótárak a georeplikáció támogatottak. 

- A georeplikáció ugyanazt a virtuális Hálózatot támogatott hello-gyorsítótárak közötti.
- A más Vnetekről-gyorsítótárak közötti georeplikáció is támogatott, mindaddig, amíg a két hello Vnetek úgy, hogy a hello Vnetek erőforrás-e képes tooreach vannak konfigurálva másik TCP-kapcsolatokon keresztül.

### <a name="can-i-use-powershell-or-azure-cli-toomanage-geo-replication"></a>Használhatja a PowerShell vagy Azure CLI toomanage georeplikáció?

Jelenleg csak kezelheti a georeplikáció használatával hello Azure-portálon.

### <a name="how-much-does-it-cost-tooreplicate-my-data-across-azure-regions"></a>Nem mennyibe tooreplicate az adatok Azure-régiók között?

Ha-georeplikációt használ, a hello elsődleges csatolt gyorsítótárból adatai replikált toohello másodlagos társított gyorsítótár. Ha a két hello kapcsolódó gyorsítótárak vannak hello azonos Azure-régió, ingyenesek hello adatátvitelre. Ha hello két kapcsolódó gyorsítótárak különböző Azure-régiókban, hello Georeplikáció adatok átvitel kell fizetni hello sávszélesség költség replikálása adott adatok toohello más Azure-régiót. További információkért lásd: [sávszélesség díjszabás](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-hello-operation-fail-when-i-tried-toodelete-my-linked-cache"></a>Miért volt sikertelen a hello művelet közben toodelete a csatolt gyorsítótár?

Ha két gyorsítótárak egymáshoz kapcsolódó, nem törölhető vagy hello gyorsítótár egy olyan erőforráscsoport, amely tartalmazza azokat, amíg el nem távolítja hello georeplikációs hivatkozást. Kísérel meg toodelete hello tartalmazó erőforráscsoportot egyik vagy mindkét hello kapcsolódó gyorsítótárak, hello egyéb erőforrások hello erőforráscsoportban törlése, de hello erőforráscsoport marad hello `deleting` állapot és az esetleges kapcsolódó hello erőforráscsoportban gyorsítótárak hello maradni `running` állapotát. hello erőforráscsoport és hello toocomplete hello törlését kapcsolódó gyorsítótárak belül, akkor a break hello georeplikációs hivatkozást a [georeplikáció hivatkozás eltávolítása](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Milyen régióban kell használni a másodlagos csatolt gyorsítótár?

Általánosságban ajánlott a gyorsítótár tooexist a hello azonos hello alkalmazásként, aki hozzáfér az Azure-régiót. Ha az alkalmazás egy elsődleges és tartalék régió, az elsődleges és másodlagos gyorsítótár szerepel kell azokban a régiókban. Párhuzamos régiók kapcsolatos további információkért lásd: [gyakorlati tanácsok – Azure párosított régiók](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-toohello-secondary-linked-cache-work"></a>Hogyan működik a toohello másodlagos csatolt gyorsítótár feladatátvételét?

A georeplikáció eredeti kiadását hello Azure Redis Cache nem támogatja a automatikus feladatátvétel Azure-régiók között. A georeplikáció elsősorban vész-helyreállítási forgatókönyv használatos. Distater helyreállítás az ügyfelek kell elindítani a hello egész alkalmazáscsoportokat egy biztonsági mentési régióban összehangolt módon ahelyett, hogy eldönthesse, mikor egyéni alkalmazás-összetevők a saját tooswitch tootheir biztonsági mentések. Ez a különösen fontos tooRedis. Hello fő előnyei a Redis egyike, hogy a rendszer egy nagyon kis késleltetésű tárolóban. Redis által használt alkalmazás átadja tooa különböző Azure-régió, de hello számítási rétegben viszont nem, visszatérési ideje round hozzáadott hello hatással lenne egy észrevehető teljesítmény. Ezért szeretnénk tooavoid Redis hibás keresztül automatikusan miatt tootransient elérhetőségével kapcsolatos problémákat.

Jelenleg tooinitiate hello feladatátvétellel kell tooremove hello georeplikáció hello Azure-portálon hivatkozásra, és módosítsa a hello kapcsolati végpontot a Redis-kliensével hello hello elsődleges csatolt gyorsítótár (korábbi nevén csatolt) toohello másodlagos gyorsítótár. Ha két gyorsítótárak vannak hozzárendelésének megszüntetése, hello hello replika lesz szokványos írási és olvasási gyorsítótár újra, és közvetlenül a Redis-ügyfelektől érkező kéréseket fogad.


## <a name="next-steps"></a>Következő lépések

További tudnivalók hello [Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md).

