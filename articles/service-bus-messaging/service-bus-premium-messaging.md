---
title: "aaaAzure Service Bus prémium és standard szintű üzenetkezelés árképzési áttekintése |} Microsoft Docs"
description: "A Service Bus prémium és standard üzenetkezelési szintjei"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: darosa;sethm
ms.openlocfilehash: 4eea5d86d342e858f50450308fb3d96a7a80b49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>A Service Bus prémium és standard szintű üzenetkezelés szintjei

A Service Bus-üzenetkezelés, amely a várólistákhoz és témakörökhöz hasonló entitásokat is tartalmaz, a vállalati üzenetkezelési képességeket ötvözi a gazdag közzétételi/előfizetési szemantikákkal a felhőbeli skálázással. Service Bus üzenetkezelés használható hello kommunikációs gerincét, számos kifinomult felhőalapú megoldás.

Hello *prémium* Service Bus üzenetkezelés szintjének címek méretezés, a teljesítmény és a kritikus fontosságú alkalmazások rendelkezésre állásának általános ügyfélkérelmeket. Annak ellenére, hogy hello szolgáltatáskészletek csaknem azonosak, a Service Bus üzenetkezelés két szintje a tervezett tooserve más használati eseteket.

A következő táblázat hello néhány fontos eltérést emel.

| Prémium | Standard |
| --- | --- |
| Magas teljesítmény |Változó teljesítmény |
| Kiszámítható teljesítmény |Változó késés |
| Rögzített díjszabás |Használatalapú változó díjszabás |
| Képes tooscale munkaterhelés növekszik és csökken |N/A |
| Too1 MB méretű üzeneteket |Üzenet mérete too256 KB mentése |

**Service Bus prémium szintű üzenetkezelés** , hogy minden ügyfél számítási feladata elkülönítve hello CPU és memória szintű erőforrás-elkülönítést is biztosít. Ennek az erőforrás-tárolónak a neve *üzenetkezelési egység*. Legalább egy üzenetkezelési egység van lefoglalva minden prémium névtérhez. Az egyes Service Bus prémium névterekhez 1, 2 vagy 4 üzenetkezelési egység vásárolható. Egy egyetlen számítási feladat vagy entitás több üzenetkezelési egységre is kiterjedhet, és az üzenetkezelési egységek száma hello tetszés szerint módosítható lesz, bár a számlázás 24 órás vagy napi díjszabás szerint történik. hello eredménye a Service Bus-alapú megoldás kiszámítható és ismételhető teljesítménye.

Nem csak kiszámíthatóbb és nagyobb rendelkezésre állású a teljesítmény, de gyorsabb is. Service Bus prémium szintű üzenetkezelés épít, rendszerben bevezetett hello tárolóprogramot [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Prémium szintű üzenetkezelés esetén a csúcsteljesítmény sokkal gyorsabb, mint hello Standard csomagra.

## <a name="premium-messaging-technical-differences"></a>Prémium szintű üzenetkezelés – műszaki eltérések

hello következő szakaszok tárgyalják néhány különbség a prémium és standard szintű üzenetkezelési szintek között.

### <a name="partitioned-queues-and-topics"></a>Particionált üzenetsorok és témakörök

A prémium szintű üzenetkezelés támogatja a particionált üzenetsorokat és témaköröket. Ezek az entitások tulajdonképpen mindig particionáltak (és nem tilthatók le). Azonban prémium particionálva üzenetsorok és témakörök nem működnek a hello ugyanúgy, mint a Standard és az alapszintű Service Bus üzenetkezelés rétegek hello. Prémium szintű üzenetküldés nem használja az SQL-t az adattároláshoz, és már nem rendelkezik a megosztott platformokhoz társított Erőforrásverseny hello. Ennek eredményeképpen particionálás nem szükséges tooimprove teljesítményét. Emellett a hello partíciók száma megváltozott 16 partíciójáról a prémium szintű a standard szintű üzenetkezelés too2 partíciók. Két partíció biztosítja a rendelkezésre állást, és megfelelőbb szám a prémium szintű futtatókörnyezethez hello. 

Prémium szintű üzenetkezelés esetén hello mérete rendelkező entitás megadásakor [MaxSizeInMegabytes](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxsizeinmegabytes#Microsoft_ServiceBus_Messaging_QueueDescription_MaxSizeInMegabytes), hogy mérete van-e osztani egyaránt hello 2 partíció eltérően [Standard particionált entitások](service-bus-partitioning.md#standard) mely hello a teljes mérete 16 alkalommal hello megadott mérete. 

A particionálásra vonatkozó további információkat a [Partitioned queues and topics](service-bus-partitioning.md) (Particionált üzenetsorok és témakörök) című rész tartalmazza.

### <a name="express-entities"></a>Expressz entitások

Mivel a prémium szintű üzenetkezelés teljesen izolált futtatókörnyezetben fut, a prémium szintű névterek nem támogatják az expressz entitásokat. Hello expressz szolgáltatásról kapcsolatos további információkért lásd: hello [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) tulajdonság.

Ha futtatja a szabványos a hibaüzenetekkel és a kívánt tooport toohello prémium csomagban kódot, győződjön meg arról, hogy hello [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) tulajdonsága túl**hamis** (hello az alapértelmezett érték).

## <a name="get-started-with-premium-messaging"></a>Ismerkedés a prémium szintű üzenetkezeléssel

Ismerkedés a prémium szintű üzenetkezelés egyszerű pedig hello folyamata hasonló toothat, Standard szintű üzenetkezelés. Első lépésként [hozzon létre egy névteret](service-bus-create-namespace-portal.md). Győződjön meg arról, hogy a **Tarifacsomag** alatt a **Prémium** tarifacsomagot választotta ki.

![create-premium-namespace][create-premium-namespace]

[Az Azure Resource Manager-sablonok használatával is létrehozhat prémium szintű névtereket](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Következő lépések

toolearn Service Bus üzenetkezelés kapcsolatos további információkért tekintse meg a következő témakörök hello.

* [Az Azure Service Bus prémium szintű üzenetkezelés bemutatása (blogbejegyzés)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Az Azure Service Bus prémium szintű üzenetkezelés bemutatása (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [A Service Bus-üzenetkezelés áttekintése](service-bus-messaging-overview.md)
* [Hogyan toouse Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
