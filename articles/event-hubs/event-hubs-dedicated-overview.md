---
title: "Azure Event Hubs dedikált kapacitás aaaOverview |} Microsoft Docs"
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
ms.openlocfilehash: 79c09975e5c0a6d4729c8f836576770abaaf322e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Dedikált Event Hubs áttekintése

*Event Hubs dedikált* kapacitás biztosít egy bérlői telepítések hello rendelkező ügyfelek esetén legnagyobb igénybevételt jelentő követelmények. Teljes léptékű Azure Event Hubs érkező is több mint 2 millió események másodpercenkénti vagy felfelé telemetriai teljesen tartós tárolási és alárendelt második késéssel too2-GB / s. Ez is lehetővé teszi, hogy integrált megoldások feldolgozásával valós idejű és a kötegelt hello ugyanarra a rendszerre. Az Event Hubs archív hello ajánlatban szereplő a megoldás hello összetettsége azzal, hogy valós időben és kötegelt alapú folyamatok támogatja egyetlen adatfolyam csökkentheti.

hello következő táblázat összehasonlítja az Event hubs hello elérhető szolgáltatási szinteket. hello Event Hubs dedikált ajánlat rögzített havi ár, a Standard és az alapszintű legtöbb szolgáltatását árképzési összehasonlított toousage. hello dedikált réteg hello szolgáltatást hello Standard csomag, de nagy munkaterhelések rendelkező ügyfelek esetén a vállalati méretezési kapacitással kínál. 

| Szolgáltatás | Basic | Standard | Dedikált |
| --- |:---:|:---:|:---:|
| Belépő események | Fizessen millió esemény | Fizessen millió esemény | Tartalmazza |
| Átviteli egység (1 MB/s érkező, kimenő 2 MB/s) | Fizessen óránként | Fizessen óránként | Tartalmazza |
| Üzenet mérete | 256 KB | 256 KB | 1 MB |
| Kiadói irányelvek | N/A | Igen | Igen |     
| Felhasználói csoportok | 1 - alapértelmezett | 20 | 20 |
| Üzenetek visszajátszása | Igen | Igen | Igen |
| Kapacitásegységek maximális száma | 20 | 20 (rugalmas too100)  | 1 CU≈200 |
| Felügyelt kapcsolatok | 100 tartalmazza | 1000 tartalmazza | 100 K tartalmazza |
| További felügyelt kapcsolatok | N/A | Igen | Igen |
| Üzenetmegőrzés | 1 ingyenes nap | 1 ingyenes nap | Akár too7 nap tartalmazza |
| Archiválás (előzetes verzió) | N/A   | Fizessen óránként | Tartalmazza |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Event Hubs dedikált kapacitás előnyei

a következő előnyöket hello Event Hubs dedikált használatakor érhetők el:

* A többi bérlő nem zaj üzemeltető egybérlős.
* Üzenet mérete nő too1 MB, mint a Standard és alapszintű too256 KB.
* Minden alkalommal ismételhető teljesítménye.
* Garantált kapacitás toomeet a kapacitásnövelés kell.
* Méretezhető 1 és 8 kapacitásegység (CU) – közötti too2 millió érkező események másodpercenkénti mentést biztosít.
  * Logikai csoport Egyé kezelésére hello az Event Hubs dedikált, ahol minden CU körülbelül 200 átviteli egységek (SO) hello egyenértékű biztosít.
* Karbantartási nulla: azt a terheléselosztás, az operációs rendszer frissítések, biztonsági javítások és particionálás kezelése.
* Rögzített, havonta árak.

Event Hubs dedikált is eltávolítja az egyes hello átviteli korlátozásai hello szabványos ajánlat. Átviteli egységek a Basic és Standard rétegek jogosíthatók too1000 események második vagy 1 MB / s érkező / SO és a dupla adott kimenő forgalom mennyisége. hello dedikált méretezési ajánlat korlátozások nélkül legyen érkező és a kimenő forgalom esemény száma. Ezek a korlátozások csak az event hubs vásárolt hello kapacitásának feldolgozása hello vonatkozik.

Ez a szolgáltatás hello legnagyobb telemetriai felhasználói megcélzó, és elérhető toocustomers egy nagyvállalati szerződéshez.

## <a name="how-tooonboard"></a>Hogyan tooonboard

hello Event Hubs dedikált platform és egy nagyvállalati szerződés a különböző méretű logikai csoport Egyé kínálják. Minden egyes CU körülbelül 200 átviteli egységek hello megfelelője itt. Méretezheti a kapacitás felfelé vagy lefelé teljes hello hónap toomeet igényeinek hozzáadásával vagy eltávolításával a logikai csoport Egyé. hello dedikált terv egyedi abban, hogy egy több gyakorlati Bevezetés az Event Hubs termék team tooget hello rugalmas telepítési, amely az Ön számára legmegfelelőbb hello fog tapasztalni. 

## <a name="next-steps"></a>Következő lépések
Forduljon a Microsoft értékesítési képviselőink vagy a Microsoft Support tooget további kapacitásával kapcsolatos részletes információkat Event Hubs dedikált. Is további információ az Event Hubs érhetők el a következő hivatkozások hello:

Részletes információk a díjszabásról látogasson el a következő hivatkozások hello:

- [Event Hubs dedikált árképzési](https://azure.microsoft.com/pricing/details/event-hubs/). Forduljon a Microsoft értékesítési képviselőink vagy a Microsoft Support tooget további kapacitásával kapcsolatos részletes információkat Event Hubs dedikált.
- Hello [Event Hubs gyakran ismételt kérdések](event-hubs-faq.md) díjszabási információkat tartalmaz, és az Event Hubs néhány gyakran feltett kérdésekre adott válaszok. 
