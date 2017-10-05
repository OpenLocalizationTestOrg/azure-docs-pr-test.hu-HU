---
title: "Azure Event Hubs dedikált kapacitás áttekintése |} Microsoft Docs"
description: "A Microsoft Azure Event Hubs dedikált kapacitás áttekintése."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: sethm;babanisa
ms.openlocfilehash: b3af61ec0923a0d9d207cee790d59aa9254a578b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Dedikált Event Hubs áttekintése

*Event Hubs dedikált* kapacitás biztosít egy bérlői központi telepítések a legnagyobb igénybevételt jelentő követelményekkel rendelkező ügyfelek esetén. Teljes léptékű Azure Event Hubs érkező is több mint 2 millió események második vagy legfeljebb 2 GB / másodperc teljes tartós tárolási és alárendelt második késés telemetriai adatok száma. Ez egyben lehetővé teszi integrált megoldások feldolgozása valós idejű és kötegelt ugyanarra a rendszerre. Az Event Hubs archív az ajánlatban szereplő csökkentheti a megoldás összetettsége azzal, hogy valós időben és kötegelt alapú folyamatok támogatja egyetlen adatfolyam.

Az alábbi táblázat összehasonlítja a rendelkezésre álló szolgáltatásszintek az Event hubs. Az Event Hubs dedikált elérhető rögzített havi ár, a Standard és alapszintű legtöbb szolgáltatását tarifacsomag használati képest. A dedikált kínál, a szolgáltatások, a Standard csomag, de nagy munkaterhelések rendelkező ügyfelek esetén a vállalati méretezési kapacitással. 

| Szolgáltatás | Basic | Standard | Dedikált |
| --- |:---:|:---:|:---:|
| Belépő események | Fizessen millió esemény | Fizessen millió esemény | Tartalmazza |
| Átviteli egység (1 MB/s érkező, kimenő 2 MB/s) | Fizessen óránként | Fizessen óránként | Tartalmazza |
| Üzenet mérete | 256 KB | 256 KB | 1 MB |
| Kiadói irányelvek | N/A | Igen | Igen |     
| Felhasználói csoportok | 1 - alapértelmezett | 20 | 20 |
| Üzenetek visszajátszása | Igen | Igen | Igen |
| Kapacitásegységek maximális száma | 20 | 20 (rugalmas 100)  | 1 CU≈200 |
| Felügyelt kapcsolatok | 100 tartalmazza | 1000 tartalmazza | 100 K tartalmazza |
| További felügyelt kapcsolatok | N/A | Igen | Igen |
| Üzenetmegőrzés | 1 ingyenes nap | 1 ingyenes nap | Legfeljebb 7 napot tartalmaz |
| Archiválás (előzetes verzió) | N/A   | Fizessen óránként | Tartalmazza |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Event Hubs dedikált kapacitás előnyei

A következő előnyöket Event Hubs dedikált használatakor érhetők el:

* A többi bérlő nem zaj üzemeltető egybérlős.
* Üzenet mérete 256 KB-os képest 1 MB-ra növeli a Standard és alapszintű.
* Minden alkalommal ismételhető teljesítménye.
* Garantált kapacitásnövelés igényeinek kielégítése érdekében.
* Méretezhető 1 és 8 kapacitásegység (CU) – közötti másodpercenként legfeljebb 2 millió érkező események biztosítása.
  * Logikai csoport Egyé az Event Hubs dedikált, ahol minden CU körülbelül 200 átviteli egységek (SO) egyenértékű biztosít a kezelésére.
* Karbantartási nulla: azt a terheléselosztás, az operációs rendszer frissítések, biztonsági javítások és particionálás kezelése.
* Rögzített, havonta árak.

Event Hubs dedikált is eltávolítja a szabványos ajánlat átviteli vonatkozó korlátozások némelyike. Átviteli egységek a Basic és Standard rétegek jogosíthatók 1000 események második vagy 1 MB / s érkező / SO és a dupla adott kimenő forgalom mennyisége. A dedikált méretezési ajánlat korlátozások nélkül legyen érkező és a kimenő forgalom esemény száma. Ezek a korlátozások csak a feldolgozási kapacitás a megvásárolt event hubs vonatkozik.

Ez a szolgáltatás a legnagyobb telemetriai felhasználói megcélzó és nagyvállalati szerződéssel rendelkező ügyfelek számára elérhető.

## <a name="how-to-onboard"></a>Hogyan bevezetni

Az Event Hubs dedikált platform és egy nagyvállalati szerződés a különböző méretű logikai csoport Egyé kínálják. Minden egyes CU körülbelül 200 átviteli egységek egyenértékű biztosít. Felfelé vagy lefelé társították az igényeknek megfelelő hozzáadásával vagy eltávolításával a logikai csoport Egyé is méretezheti a kapacitást. A dedikált terv egyedi abban, hogy egy több gyakorlati Bevezetés az Event Hubs termékért felelős csoport a rugalmas telepítési, amely az Ön számára legmegfelelőbb lekérni a fog tapasztalni. 

## <a name="next-steps"></a>Következő lépések
Forduljon a Microsoft értékesítési képviselőink vagy a Microsoft Support Event Hubs dedikált kapacitásával kapcsolatos további részletek. Is további információ az Event Hubs érhetők el a következőket:

Árazással kapcsolatos részletes információkért látogasson el a következőket:

- [Event Hubs dedikált árképzési](https://azure.microsoft.com/pricing/details/event-hubs/). Forduljon a Microsoft értékesítési képviselőink vagy a Microsoft Support Event Hubs dedikált kapacitásával kapcsolatos további részletek.
- A [Event Hubs gyakran ismételt kérdések](event-hubs-faq.md) díjszabási információkat tartalmaz, és az Event Hubs néhány gyakran feltett kérdésekre adott válaszok. 
