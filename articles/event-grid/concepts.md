---
title: "aaaAzure esemény rács fogalmak"
description: "Azure Event rács és a fogalmakat ismerteti. Határozza meg az esemény rács több kulcsfontosságú összetevők."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/10/2017
ms.author: babanisa
ms.openlocfilehash: bb86381330fd2d6d2769167ec1953f0d5c28af85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="concepts-in-azure-event-grid"></a>Az Azure Event rácsban fogalmak

hello Azure esemény rács fő jellemzői a következők:

## <a name="events"></a>Események

Egy esemény egy hello adatmennyiséget teljesen leíró valami hello rendszerben történt.  Minden eseményhez tartozik az általános információkat, például: hello esemény, hello esemény forrását tartott érvényes és egyedi azonosítója.  Minden esemény is rendelkezik, amely csak a megfelelő toohello adott esemény információkkal. Egy esemény létrehozása az Azure Storage új fájlokról például hello fájl, például hello lastTimeModified érték adatait tartalmazza. Vagy egy virtuális gép újraindul kapcsolatos esemény hello virtuális gépet, és újraindítás okának hello hello nevét tartalmazza.

## <a name="event-sourcespublishers"></a>Esemény források-közzétevők

Egy eseményforrás, ahol hello esemény történik. Például az Azure Storage hello eseményforrás létrehozott események BLOB. Az Azure virtuális gép háló hello forrás virtuális gép események. Eseményforrások események tooEvent rács közzétételi felelősek.

## <a name="topics"></a>Témakörök

Közzétevők események kategorizálása témakörökre. hello témakör tartalmaz egy végpontot, ahol hello publisher eseményeket küldi. toorespond toocertain típusú eseményeket, előfizetők döntse el, melyik témakörök toosubscribe számára. Témakörök is, hogy előfizetők felderíthesse hogyan a tooconsume hello események megfelelően adja meg az esemény séma.

Rendszer témakörök Azure szolgáltatás által biztosított beépített témakörök szolgálnak. Egyéni témakörök alkalmazás és a külső témakörök szolgálnak.

## <a name="event-subscriptions"></a>Esemény-előfizetések

Előfizetés arra utasítja a témakör az eseményeket az előfizető olyan fogadás iránt érdeklődik esemény rács.  Előfizetés is tartalmazza a hogyan események kézbesítési toohello előfizető.

## <a name="event-handlers"></a>Az eseménykezelők

Egy esemény rács szempontjából eseménykezelő hello hely, ahol hello esemény érkezik. hello-kezelő bontja néhány további művelet tooprocess hello esemény.  Esemény rács több előfizető-típusokat támogatja. Előfizető hello típusától függően esemény rács különböző mechanizmusok tooguarantee hello kézbesítési hello esemény következik.  A HTTP webhook eseménykezelők hello esemény van végrehajtásáig hello kezelő adja vissza egy állapotkódját `200 – OK`. Az Azure Storage Üzenetsorába hello események ismétlődnek, amíg hello Queue szolgáltatás nem tud toosuccessfully hello üzenet leküldéses hello várólistán.

## <a name="filters"></a>Szűrők

Amikor előfizetés tooa témakör, végezhet toohello végpont küldött hello eseményeket. Esemény típusa, vagy a tulajdonos minta szerint szűrheti. További információkért lásd: [esemény rács előfizetés séma](subscription-creation-schema.md).

## <a name="security"></a>Biztonság

Esemény tootopics előfizetés, és a témakörök közzététele biztonságot nyújt. Ha az előfizetés, hello erőforrás vagy a témakör a megfelelő engedélyekkel kell rendelkeznie. Közzétételekor, rendelkeznie kell egy SAS-jogkivonat vagy hello témakör kulcsos hitelesítéséhez. További információkért lásd: [esemény rács biztonsági és hitelesítési](security-authentication.md).

## <a name="failed-delivery"></a>Sikertelen kézbesítés

Esemény rács nem tudják megerősíteni, hogy az esemény hello előfizető végpont megkapta-e, ha azt redelivers hello esemény. További információkért lásd: [esemény rács üzenetkézbesítést, és próbálkozzon újra](delivery-and-retry.md).

## <a name="next-steps"></a>Következő lépések

* Egy bevezető tooEvent rács, lásd: [esemény rács](overview.md).
* tooquickly esemény rács használatának megkezdésében című [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).
