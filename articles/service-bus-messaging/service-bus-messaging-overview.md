---
title: "aaaAzure Service Bus üzenetkezelésének áttekintése |} Microsoft Docs"
description: "A Service Bus-üzenetkezelés és az Azure Relay ismertetése"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: ede2904e11544d8f9428a2d657dcc77dacd95ac4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-hello-cloud"></a>Service Bus üzenetkezelés: rugalmas adattovábbítás a felhőben hello
A Microsoft Azure Service Bus egy megbízható információkézbesítési szolgáltatás. hello szolgáltatás célja toomake kommunikációs egyszerűbb. Ha két vagy több fél szeretne tooexchange információkat, a kommunikáció egyeztető szükségük. A Service Bus egy közvetítő- vagy harmadikfél-alapú kommunikációs módszer. Ez a hasonló tooa hello fizikai világ postai szolgáltatás. A postai szolgáltatások teszik nagyon könnyen toosend különböző fajta levelek és csomagok számos kézbesítési garanciával bárhol a hello world.

Hasonló toohello postai szolgáltatás levélkézbesítéséhez, a Service Bus is rugalmas hello feladó és hello címzett. hello üzenetkezelési szolgáltatás biztosítja, hogy hello információ akkor is célba akkor is, ha hello a két fél soha nem is online hello azonos időben, vagy nem érhetők el: hello pontos ugyanaz a idő. Ezzel a módszerrel üzenetküldési hasonló toosending betűk, nem közvetítőalapú kommunikáció pedig hasonló tooplacing telefonhívást (vagy egy telefonhívást használatáról toobe - hívás hívásvárakoztatás és hívóazonosítás azonosítója, amely idők telefonhívásaihoz előtt).

hello üzenet küldőjének is megkövetelheti a különböző továbbítási jellemzőt, beleértve a tranzakciók, kettős észlelés, időalapú lejárat és kötegelés. Ezeknek a mintáknak is megvannak a postai párhuzamaik: ismételt kézbesítés, kötelező aláírás, címmódosítás vagy visszahívás.

A Service Bus két különböző üzenetkezelési mintát támogat: az *Azure Relay-t* és a *Service Bus-üzenetkezelést*.

## <a name="azure-relay"></a>Azure Relay
Hello [WCF továbbító](../service-bus-relay/relay-what-is-it.md) Azure-továbbító összetevője egy központosított (de erősen elosztott terhelésű) szolgáltatás, amely számos különböző átviteli protokollt és webszolgáltatási szabványt. Ezek közé tartozik például a SOAP, a WS-* és a REST is. Hello [továbbítási szolgáltatás](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) számos különböző továbbítási kapcsolati lehetőséget biztosít, és segít a közvetlen társ-társ-kapcsolatok egyeztetésére, ha lehetséges. A Service Bus .NET-fejlesztők számára, akik használják a Windows Communication Foundation (WCF), mind a legutóbb tooperformance és a használhatóság hello van optimalizálva, és teljes hozzáféréssel tooits biztosít a továbbítási szolgáltatáshoz a SOAP és a REST felületeken keresztül. Ez lehetővé teszi bármely SOAP vagy REST programozási környezet toointegrate a Service busszal.

hello továbbítási szolgáltatás támogatja a hagyományos egyirányú üzenetküldés, kérelem/válasz, és a társ-társ üzenetkezelés. Támogatja az események terjesztését is az internetes tartományban tooenable közzétételi-előfizetési forgatókönyvek és a megnövekedett pontok közötti nagyobb hatékonyság kétirányú szoftvercsatornás kommunikáció. Hello továbbítón keresztüli üzenetcsere mintában a helyszíni szolgáltatásnak toohello továbbítási szolgáltatáshoz egy kimenő porton keresztül csatlakozik, és a kétirányú szoftvercsatornás kommunikáció kötött tooa adott szinkronizálási címhez hoz létre. hello ügyfél küldött üzenetek toohello továbbítási szolgáltatás hello szinkronizálási címhez célcsoportkezelést majd kommunikál hello a helyszíni szolgáltatással. hello továbbítási szolgáltatás fog majd "továbbítási" üzenetek toohello a helyszíni szolgáltatás hello kétirányú szoftvercsatornás már működő keresztül. hello ügyfél nem kell a közvetlen kapcsolat toohello a helyszíni szolgáltatás, se nem szükséges informatikai tooknow ahol hello szolgáltatás található, és hello helyszíni szolgáltatást nem kell bejövő portra a hello tűzfal nyissa meg.

Kezdeményezzen hello kapcsolat a helyszíni szolgáltatás és a hello továbbítási szolgáltatás között, a WCF "továbbítása" kötéskészlet használatával. Hello háttérben hello továbbítási kötéseket a tootransport kötés elemek toocreate WCF-csatornaösszetevők Service Bus hello felhőben integrálódó képezi.

WCF továbbító számos előnnyel jár, de hello kiszolgáló szükséges, és ügyfél tooboth hello azonos rendelés toosend idő, és üzeneteket fogadni, online állapotú legyen. Ez nem optimális a HTTP-stílusú kommunikáció, melyik hello kérelmek nem lehet általában hosszú élettartamú, sem az ügyfelek, amelyek csak alkalmanként csatlakoznak például böngészők, mobilalkalmazások, és így tovább. A közvetítőalapú üzenettovábbítás támogatja a leválasztott kommunikációt, és megvannak a saját előnyei; az ügyfelek és a kiszolgálók akkor csatlakozhatnak, amikor szükséges, és a műveleteket aszinkron módon hajthatják végre.

## <a name="brokered-messaging"></a>Közvetítőalapú üzenettovábbítás
Ezzel szemben toohello továbbítják a rendszer, a Service Bus üzenetküldési, vagy [közvetítőalapú üzenettovábbítás](service-bus-queues-topics-subscriptions.md) tekinthető aszinkronnak, vagy "ideiglenesen le." Az adatalkotóknak (küldőknek) és a fogyasztóknak (fogadóknak) nem rendelkezik toobe online hello ugyanannyi időt vesz igénybe. hello üzenetküldési infrastruktúra megbízhatóan tárolja az üzeneteket az "ügynök" (például egy várólista), amíg a hello fogyasztó fél készen tooreceive őket. Ez lehetővé teszi a leválasztott, akár önkéntesen; hello elosztott alkalmazás toobe hello összetevői például karbantartás céljából, vagy tooa összetevő összeomlása, az nem befolyásolja a teljes rendszer hello miatt. Ezenkívül hello fogadó alkalmazás csak lehet toocome online hello nap, például olyan készletkezelő rendszer esetén, amely csak szükséges toorun hello végén lévő hello üzleti nap bizonyos időpontjaiban.

hello hello Service Bus közvetítő alapú üzenettovábbítás infrastruktúrájának alapvető összetevői a várólisták, témakörök és előfizetések.  hello elsődleges különbség az, hogy témakörök támogatják-e a közzétételi/előfizetési képességeket, amelyek kifinomultabb tartalomalapú Útválasztás és kézbesítési logika, például küldés toomultiple címzettek nem használható. Ezek az összetevők új aszinkron üzenetkezelési forgatókönyveket tesznek lehetővé, például az átmeneti leválasztást, a közzétételt/előfizetést és a terheléselosztást. További információk ezekről az üzenetkezelési entitásokról: [Service Bus queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md) (Service Bus-üzenetsorok, -témakörök és -előfizetések).

Mivel hello WCF továbbító infrastruktúrájával hello közvetítőalapú üzenettovábbítás képessége is elérhető, a WCF és .NET-keretrendszer programozók és REST-en keresztül.

## <a name="next-steps"></a>Következő lépések
toolearn Service Bus üzenetkezelés, kapcsolatos további információkért tekintse meg a következő témakörök hello.

* [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-üzenetsorok, -témakörök és -előfizetések](service-bus-queues-topics-subscriptions.md)
* [Hogyan toouse Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md)
* [Hogyan toouse Service Bus üzenettémák és előfizetések](service-bus-dotnet-how-to-use-topics-subscriptions.md)

