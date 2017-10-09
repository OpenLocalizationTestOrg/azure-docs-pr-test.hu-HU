---
title: "az Azure Service Fabric megbízható gyűjtemények és zárolási mód aaaTransactions |} Microsoft Docs"
description: "Az Azure Fabric megbízható állapot szolgáltatáskezelő és megbízható gyűjtemények tranzakciók és zárolása."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 340e029aa98f43ad6e46b48f687dad01f9d96f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Tranzakciók és az Azure Service Fabric megbízható gyűjtemények zárolási mód

## <a name="transaction"></a>Tranzakció
Egy tranzakció a munka egyetlen logikai egységként végrehajtott műveletek sorozata.
Egy tranzakció kell virágot hello következő ACID tulajdonságai. (lásd: https://technet.microsoft.com/en-us/library/ms190612)
* **Atomicity**: egy tranzakciót egy atomi munkaegység kell lennie. Más szóval vagy az adatok változtatás történik, vagy egyikük sem történik.
* **Konzisztencia**: befejezése után egy tranzakció konzisztens állapotban kell hagynia minden adat. Az összes belső adatszerkezetek megfelelő hello tranzakció hello végén kell lennie.
* **Elkülönítési**: párhuzamos tranzakciók által végrehajtott módosítások el kell különíteni a párhuzamos tranzakciók hello módosítások. egy művelet egy ITransaction belül használt hello elkülönítési szint hello IReliableState teljesítő hello művelet határozza meg.
* **Tartóssági**: amikor egy tranzakció véget ért, hatásai vannak, ha véglegesen hello rendszerben. hello a módosításokat továbbra is fennáll, még akkor is, a rendszer hibák hello esemény.

### <a name="isolation-levels"></a>Elkülönítési szinten
Elkülönítési szint meghatározása hello mértékben toowhich hello tranzakció legyen különítve a többi tranzakciók által végzett módosításokat.
Két, a megbízható gyűjtemények támogatott elkülönítési szinten van:

* **Ismételhető olvasás**: meghatározza, hogy utasítás nem tudja olvasni a módosított, de más tranzakciók által még nem véglegesített adatokat, és, hogy más tranzakciók módosíthatja, hogy az aktuális tranzakció hello aktuális hello csak olvasásra került adatok tranzakció befejezését. További részletekért lásd: [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
* **Pillanatkép**: Megadja, hogy egy tranzakció utasítás által beolvasott adatok hello tranzakciós úton konzisztens hello tranzakció hello kezdetén meglévő hello adatok verzióját.
  hello tranzakció fel tudja ismerni csak adatok adtainak hello hello tranzakció elindítása előtt véglegesítése volt.
  Adatok módosítások más tranzakciók hello hello aktuális tranzakció elindítása után nem látható toostatements hello aktuális tranzakció végrehajtása.
  hello hatással, mintha hello utasítás szerepel egy tranzakcióban elkövetett hello adatok pillanatképe beolvasása, adott hello tranzakció hello kezdetén verzióját.
  A pillanatképek egységesek megbízható gyűjtemények.
  További részletekért lásd: [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Megbízható gyűjtemények automatikusan válasszon hello elkülönítési szint toouse adott olvasási művelet attól függően, hogy hello művelet és hello replika szerepkörét az hello hello tranzakció létrehozása során.
Az alábbiakban az elkülönítési szint alapértelmezett megbízható szótár és várólista műveletekhez ábrázol hello tábla.

| A művelet \ szerepkör | Elsődleges | Másodlagos |
| --- |:--- |:--- |
| Egyetlen entitás olvasása |Ismételhető olvasás |Pillanatkép |
| Számbavételi, száma |Pillanatkép |Pillanatkép |

> [!NOTE]
> Gyakori példák egyetlen entitás műveletekhez `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Hello megbízható szótár és a megbízható várólista hello támogatja, olvassa el a ír.
Más szóval bármely írási tranzakción belül kell látható tooa következő olvassák toohello tartozik, amely ugyanabban a tranzakcióban.

## <a name="locks"></a>Zárolások
A megbízható gyűjtemények szigorú tranzakciók megvalósítása két fázis zárolás: a tranzakció nem zárolások hello amíg hello tranzakció véget nem ér egy megszakítás vagy egy véglegesítési szerzett.

Megbízható szótár minden egyetlen entitás műveleteihez zárolás sor szintet használja.
Megbízható várólista lefolytathassa szigorú tranzakciós FIFO tulajdonság egyidejű ki.
Megbízható várólista művelet szintű zárolások engedélyezése egy tranzakciót használ `TryPeekAsync` és/vagy `TryDequeueAsync` és egy tranzakciót `EnqueueAsync` egyszerre.
Ne feledje, hogy toopreserve FIFO, ha egy `TryPeekAsync` vagy `TryDequeueAsync` valaha is figyelembe veszi, hogy megbízható várólista hello üres, akkor azok is zárolják `EnqueueAsync`.

Az írási műveletek mindig egy kizárólagos zárolást.
Az olvasási műveleteknél a hello zárolás szolgáltatás tényezők néhány függ.
Bármely olvasási művelet történik pillanatkép-elkülönítés használatával szabad zárolás.
Alapértelmezés szerint minden ismételhető olvasása a közös zárolás vesz igénybe.
Azonban bármely olvasási művelet, amely támogatja a ismételhető Olvasás, hello felhasználói kérje meg a módosítási zárolást hello a közös zárolás helyett.
Módosítási zárolást, az aszimmetrikus zár használt tooprevent közös formája, amely akkor fordul elő, amikor több tranzakció egy későbbi időpontban lehetséges frissítések erőforrások zárolása holtpont.

hello zárolási kompatibilitási mátrix hello a következő táblázatban találhatók:

| Kérelem \ kap | None | Közös | Frissítés | Kizárólagos |
| --- |:--- |:--- |:--- |:--- |
| Közös |Ütközés |Ütközés |Ütközés |Ütközés |
| Frissítés |Ütközés |Ütközés |Ütközés |Ütközés |
| Kizárólagos |Ütközés |Ütközés |Ütközés |Ütközés |

Hello megbízható gyűjtemények API-k időtúllépési argumentumának holtpont észlelési szolgál.
Például két tranzakció (T1 és T2) próbál tooread és K1 frissítése.
Lehetséges, hogy azok toodeadlock, mert azok mindkét hello rendelkező megosztott zárolása.
Egyik vagy mindkét hello műveletek ebben az esetben, amelynek az időkorlátja.

Ebben a forgatókönyvben holtpont példája egy nagyszerű hogyan módosítási zárolást előfordulhat, hogy holtpont.

## <a name="next-steps"></a>Következő lépések
* [A Reliable Collections használata](service-fabric-work-with-reliable-collections.md)
* [Megbízható szolgáltatások értesítések](service-fabric-reliable-services-notifications.md)
* [Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)](service-fabric-reliable-services-backup-restore.md)
* [Megbízható állapot Manager konfigurálása](service-fabric-reliable-services-configuration.md)
* [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

