---
title: "Azure költségeik aaaUnderstand korlátozása |} Microsoft Docs"
description: "Ismerteti, hogyan működik Azure költségeik korlátozza, és hogyan tooremove azt"
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
ms.openlocfilehash: ed01401a07c3d0e7edebe42fb1482b7b60b1df51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-spending-limit-and-how-tooremove-it"></a>Azure költségkeret megérteni, hogyan tooremove azt

Azure költségkeret, hogy mennyit képes költeni az Azure-előfizetéshez korlátozást. Előfizetés hello próbaverziós ajánlat vagy ajánlatokat, amely tartalmazza a kreditek több hónapokban az új ügyfelek hello költségkeret alapértelmezés szerint engedélyezve van. hello költségkeret értéke 0. Nem módosítható. hello költségkeret nem érhető el, például a használatalapú fizetés előfizetések és kötelezettségvállalás tervek előfizetés esetében. Lásd: hello [teljes listáját az Azure-ajánlatok és költségkeret hello hello rendelkezésre állását](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-hello-spending-limit"></a>Mi történik, ha szeretnék elérni költségkeret hello?

A használati díjak, az ajánlatban szereplő havi díjak hello lefoglalhat eredményez, amikor központilag telepített szolgáltatások hello le vannak tiltva az adott számlázási hónap hello rest. Így például a Cloud Services telepített szolgáltatásai el lesznek távolítva az éles környezetből, és az Azure virtuális gépek le lesznek állítva és a hozzájuk tartozó erőforrások fel lesznek szabadítva. tooprevent a szolgáltatásai le van tiltva, dönthet úgy tooremove a költségkeretét. A szolgáltatások le vannak tiltva, ha a rendszergazdák csak olvasható módon a storage-fiókok és az adatbázisok hello adatok érhetők el. Hello elején hello következő számlázási hónap, ha az ajánlat kreditek is több hónap, az előfizetés nem ismét engedélyezni. Ezután telepítse újra a Felhőszolgáltatások és teljes körű hozzáférési tooyour storage-fiókok és az adatbázisok.

Miután hello ingyenes próba-előfizetés elérte a költségkeret hello, hello előfizetés újbóli engedélyezése, és hogy automatikusan [frissítési tooour szabványos használatalapú ajánlatok](billing-upgrade-azure-subscription.md) 90 napon belül.

Kattintson a költségkeret az előfizetéshez hello értesítést kapjon. Jelentkezzen be toohello [Azure Account Center](https://account.windowsazure.com), jelölje be **fiók**, majd válassza ki **előfizetések**. Elérte a költségkeret hello előfizetések szóló értesítések láthatja.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Dolog van szó, a akkor is, ha a költségkeret maximumát engedélyezve van

Egyes Azure-szolgáltatások és [piactér a későbbi szoftvervásárlások](https://azure.microsoft.com/marketplace/) is függő díj terheli hello fizetési mód (CC) alatt, még akkor is, ha a költségkeret maximumát be van állítva. Többek között a Visual studio licencek, a Azure Active Directory premium, a támogatási csomagokat és a legtöbb külső vállalati arculattal szolgáltatások hello piactéren keresztül.


## <a name="when-not-toouse-hello-spending-limit"></a>Ha nem toouse hello költségkeret maximumát

hello költségkeret sikerült akadályozza meg a központi telepítése, vagy bizonyos piactér és a Microsoft-szolgáltatások használatával. Az alábbiakban hello forgatókönyvek, ahol el kell távolítania az előfizetés a költségkeret hello.

- Első fél képek toodeploy például Oracle és szolgáltatásokat, például a Visual Studio Team Services tervezi. Ez a forgatókönyv miatt a tooexceed szinte azonnal kiadásait korlátozza, és hatására az előfizetés toobe le van tiltva.

- Olyan szolgáltatásai vannak, amelyek működése nem szakadhat meg.

- Vannak olyan szolgáltatások és erőforrások beállításai, például a virtuális IP-címek, amikor nem szeretné toolose. Ezek a beállítások is elvesznek, ha hello szolgáltatásokat és erőforrásokat fel van szabadítva.


## <a name="remove-hello-spending-limit"></a>Távolítsa el a költségkeret hello

Eltávolíthatja hello költségkeret bármikor mindaddig, amíg egy érvényes fizetési módot, az Ön előfizetéséhez rendelve van. A jóváírás több hónap rendelkező ajánlatok esetén is engedélyezheti újra hello költségkeret hello elején a következő számlázási ciklusban.

tooremove költések korlátozni, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure Account Center](https://account.windowsazure.com).

2. Válasszon egy előfizetést.

3. Ha hello előfizetés le van tiltva esedékes toohello Költségkeretről elérése, kattintson az értesítés: "Előfizetés elérte a Költségkeretét hello és le lett tiltva tooprevent díjakat." Ellenkező esetben kattintson a **költségkeret eltávolítása** a hello **ELŐFIZETÉS ÁLLAPOTÁT** területen.

4. Válasszon egy Önnek megfelelő lehetőséget.

|Beállítás|Következmény|
|-------|-----|
|A költségkeret eltávolítása meghatározatlan időre|Eltávolítja a költségkeret bekapcsolása nélkül, automatikusan hello hello elején következő számlázási időszak hello.|
|Távolítsa el az aktuális számlázási időszak hello költségkeret maximumát|Eltávolítja a költségkeret, így azt vissza a bekapcsolása automatikusan hello hello elején következő számlázási időszak hello.|

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
