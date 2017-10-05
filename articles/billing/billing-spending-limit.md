---
title: "Azure költségkeret megértése |} Microsoft Docs"
description: "Ismerteti, hogyan működik a költségkeret Azure és annak eltávolításáról"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: genli
ms.openlocfilehash: a2743ef34bde0faabb3afd2ace27acddd59d3d70
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Azure-korlátozás és annak eltávolításáról költségeik ismertetése

Azure költségkeret, hogy mennyit képes költeni az Azure-előfizetéshez korlátozást. Új ügyfelek számára a próbaverziós ajánlat vagy ajánlatokat, amely tartalmazza a kreditek több hónap Regisztráljon a költségkeret maximumát, alapértelmezés szerint engedélyezve van. A költségkeret maximumát értéke 0. Nem módosítható. A költségkeret nem érhető el az olyan előfizetéstípusokhoz, mint a használatalapú fizetés és a kötelezettségvállalásos csomagok. Tekintse meg a [teljes listáját az Azure-ajánlatok és a költségkeret maximumát rendelkezésre állását](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Mi történik a költségkeret elérése esetén?

Ha az ajánlatban szereplő havi díjak lefoglalhat költségek a használati eredményezi, a központilag telepített szolgáltatásokat az adott számlázási hónap hátralévő le vannak tiltva. Így például a Cloud Services telepített szolgáltatásai el lesznek távolítva az éles környezetből, és az Azure virtuális gépek le lesznek állítva és a hozzájuk tartozó erőforrások fel lesznek szabadítva. A szolgáltatások letiltásának megelőzése érdekében eltávolíthatja a költségkeretet. A szolgáltatások le vannak tiltva, ha a rendszergazdák csak olvasható módon az adatokat a storage-fiókok és az adatbázisok érhetők el. A következő számlázási hónap elején ha ajánlatát kreditek tartalmaz több hónapban, az előfizetés lesz ismét engedélyezni kell. Ezután telepítse újra a Cloud Services és a storage-fiókok és az adatbázisok teljes hozzáféréssel rendelkeznek.

Után az ingyenes próba-előfizetés eléri a költségkeret maximumát, engedélyezze újra az előfizetést, és hogy automatikusan [frissítési a szabványos használatalapú fizetés ajánlatra](billing-upgrade-azure-subscription.md) 90 napon belül.

A költségkeret maximumát kattint az előfizetéshez értesítést kapjon. Jelentkezzen be a [Azure Account Center](https://account.windowsazure.com), jelölje be **fiók**, majd válassza ki **előfizetések**. Megjelenik az előfizetést, amely túllépte a költségkeretét szóló értesítések.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Dolog van szó, a akkor is, ha a költségkeret maximumát engedélyezve van

Egyes Azure-szolgáltatások és [piactér a későbbi szoftvervásárlások](https://azure.microsoft.com/marketplace/) is függő díj terheli a fizetési módot (CC) alatt, még akkor is, ha a költségkeret maximumát be van állítva. Többek között a Visual studio licencek, a prémium szintű Azure Active Directory, a támogatási csomagokat és a legtöbb külső vállalati arculattal szolgáltatások a piactéren keresztül.


## <a name="when-not-to-use-the-spending-limit"></a>Mikor nem javasolt a költségkeret használata?

A költségkeret megakadályozhatja bizonyos piactéri és Microsoft-szolgáltatások telepítését vagy használatát. A következő szituációkban javasolt a költségkeret eltávolítása az előfizetésből.

- Belső rendszerképeket tervez üzembe helyezni, mint például az Oracle, és olyan szolgáltatásokat, mint például a Visual Studio Online Team Services. Ez a forgatókönyv miatt a válaszpuffer szinte azonnal a költségkeret maximumát, és hatására az előfizetés le kell tiltani.

- Olyan szolgáltatásai vannak, amelyek működése nem szakadhat meg.

- Olyan szolgáltatásokkal és erőforrásokkal rendelkezik, amelyeknek beállításait – például a virtuális IP-címeket – nem szeretné elveszteni. Ezek a beállítások is elvesznek, ha a szolgáltatások és erőforrások fel van szabadítva.


## <a name="remove-the-spending-limit"></a>Költségkeret eltávolítása

A költségkeretet bármikor eltávolíthatja mindaddig, amíg van érvényes fizetési mód rendelve az előfizetéséhez. A több hónapon átívelő krediteket tartalmazó ajánlatok esetén a következő elszámolási időszaktól kezdődően is visszakapcsolhatja a költségkeretet.

Az alábbi lépéseket követve távolíthatja el a költségkeretet:

1. Jelentkezzen be a [Azure Account Center](https://account.windowsazure.com).

2. Válasszon egy előfizetést.

3. Ha az előfizetés a költségkeret elérése miatt van letiltva, kattintson a következő értesítésre: „Subscription reached the Spending Limit and has been disabled to prevent charges.” (A költségkeret kimerült, ezért a rendszer a további költségek elkerülése érdekében letiltotta az előfizetést.) Ellenkező esetben kattintson a **költségkeret eltávolítása** a a **ELŐFIZETÉS ÁLLAPOTÁT** területen.

4. Válasszon egy Önnek megfelelő lehetőséget.

|Beállítás|Következmény|
|-------|-----|
|A költségkeret eltávolítása meghatározatlan időre|A költségkeret eltávolítása oly módon, hogy az nem kapcsolódik vissza automatikusan a következő elszámolási időszak kezdetén.|
|A költségkeret eltávolítása az adott elszámolási időszakban|A költségkeret eltávolítása oly módon, hogy a következő elszámolási időszak kezdetén automatikusan visszakapcsolódik.|

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.
