---
title: "Az Azure Event rács fogalmak"
description: "Azure Event rács és a fogalmakat ismerteti. Határozza meg az esemény rács több kulcsfontosságú összetevők."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/10/2017
ms.author: babanisa
ms.openlocfilehash: 83b9b2c7bb4134e1d9bdf857449bfb85884333d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="concepts-in-azure-event-grid"></a>Az Azure Event rácsban fogalmak

A fő Azure esemény rácsban a következők:

## <a name="events"></a>Események

Az esemény teljes leíró adatokat valami, és ismételje meg a rendszer a legkevesebb jelenti.  Minden eseményhez tartozik az általános információkat, például: az esemény forrását időt vett igénybe az esemény, érvényes és egyedi azonosítója.  Minden esemény is rendelkezik, amely csak az adott eseményt vonatkozó információkkal. Egy új fájl létrehozását az Azure Storage esemény például a fájl, például a lastTimeModified érték részleteit tartalmazza. Vagy eseménnyel kapcsolatos egy virtuális gép újraindul a virtuális gépet, és újraindítás okának nevét tartalmazza.

## <a name="event-sourcespublishers"></a>Esemény források-közzétevők

Egy eseményforrás, ahol az esemény akkor fordul elő. Például az Azure Storage az eseményforrás a létrehozott események BLOB. Az Azure virtuális gép háló a forrás virtuális gép események. Eseményforrások felelősek esemény rácshoz események közzététele.

## <a name="topics"></a>Témakörök

Közzétevők események kategorizálása témakörökre. A témakör egy végpontot, ahol a közzétevő küld eseményeket tartalmaz. Bizonyos típusú eseményekről válaszolni előfizetők döntse el, melyik témakörök előfizetni. Témakörök is, hogy előfizetők felderíthesse az események felhasználásához megfelelően hogyan adja meg az esemény séma.

Rendszer témakörök Azure szolgáltatás által biztosított beépített témakörök szolgálnak. Egyéni témakörök alkalmazás és a külső témakörök szolgálnak.

## <a name="event-subscriptions"></a>Esemény-előfizetések

Előfizetés arra utasítja a témakör az eseményeket az előfizető olyan fogadás iránt érdeklődik esemény rács.  Előfizetés is tartalmazza a hogyan események kézbesítési az előfizető.

## <a name="event-handlers"></a>Az eseménykezelők

Esemény rács szempontból eseménykezelő az a hely, ahol az esemény küldése. A kezelő bontja feldolgozni az eseményt néhány további művelet.  Esemény rács több előfizető-típusokat támogatja. Attól függően, hogy milyen típusú előfizető esemény rács különböző mechanizmusok garantálja a kézbesítési esemény következik.  A HTTP-webhook eseménykezelők, az eseményt a rendszer ismét megkísérli mindaddig, amíg a kezelő egy állapot kódot ad vissza `200 – OK`. Az Azure Storage Üzenetsorába az események ismétlődnek, amíg a várólista-szolgáltatás nem tudja sikeresen feldolgozni az üzenetet leküldéses a várólistán.

## <a name="filters"></a>Szűrők

Amikor előfizet egy témakörbe, szűrheti a végpontnak küldött eseményeket. Esemény típusa, vagy a tulajdonos minta szerint szűrheti. További információkért lásd: [esemény rács előfizetés séma](subscription-creation-schema.md).

## <a name="security"></a>Biztonság

Esemény témakörök előfizetés, és a témakörök közzététele biztonságot nyújt. Ha az előfizetés, az erőforrás vagy a megfelelő engedélyekkel kell rendelkeznie. Közzétételekor, rendelkeznie kell egy SAS-jogkivonat vagy a témakör kulcsos hitelesítéséhez. További információkért lásd: [esemény rács biztonsági és hitelesítési](security-authentication.md).

## <a name="failed-delivery"></a>Sikertelen kézbesítés

Esemény rács nem tudják megerősíteni, hogy egy eseményt az előfizető végpont megkapta-e, ha ez az esemény redelivers. További információkért lásd: [esemény rács üzenetkézbesítést, és próbálkozzon újra](delivery-and-retry.md).

## <a name="next-steps"></a>Következő lépések

* Esemény rácshoz ismertetőért lásd: [esemény rács](overview.md).
* Ha gyorsan esemény rács segítségével, lásd: [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).