---
title: "az Azure Event Hubs aaaWhat, és miért érdemes használni |} Microsoft Docs"
description: "Áttekintés és a bevezetés tooAzure Event Hubs - felhőméretű telemetriai adatfeldolgozást webhelyek, alkalmazások és eszközök"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: f6199a2e5bee8506f529b6f561234d79f9c8d465
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-event-hubs"></a>Mi az Event Hubs?

Az Azure Event Hubs egy kiválóan méretezhető adatstreamelési platform és eseményfeldolgozási szolgáltatás, amely másodpercenként több millió esemény fogadására és feldolgozására képes. Az Event Hubs képes az elosztott szoftverek és eszközök által generált események, adatok vagy telemetria feldolgozására és tárolására. Adatok küldése az eseményközpont tooan átalakíthatók és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják. A hello képességét tooprovide [közzétételi-feliratkozási képességek](https://msdn.microsoft.com/library/aa560414.aspx) alacsony késéssel és nagy méretű az Event Hubs funkcionál hello "on elsajátítják" a Big Data.

## <a name="why-use-event-hubs"></a>Miért érdemes az Event Hubs platformot használni?

Az Event Hubs esemény- és telemetriakezelési képességei különösen az alábbiakhoz hasznosak:

* alkalmazások kialakítása,
* felhasználói élmények vagy munkafolyamatok feldolgozása,
* az eszközök internetes hálózatát (IoT) érintő forgatókönyvek.

Az Event Hubs segítségével lehetségessé válik például a viselkedéskövetés a mobilalkalmazásokban, a forgalmi információk gyűjtése a webfarmokról, a játékbeli események rögzítése a konzolos játékokban, vagy telemetriaadatok gyűjtése az ipari gépekről, csatlakoztatott járművekről vagy más eszközökről.

## <a name="azure-event-hubs-overview"></a>Azure Event Hubs – áttekintés

hello mely Event Hubs megoldás architektúrák játszik szerepet hello "bejárati ajtón" megoldásarchitektúrákban, az úgynevezett egy *eseménybetöltőnek*. Egy eseménybetöltőnek összetevő vagy szolgáltatás, amely az esemény-közzétevők és az esemény fogyasztók toodecouple hello éles egy esemény-adatfolyam a hello felhasználásától között. hello alábbi ábra mutatja be ebbe az architektúrába:

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Az Event Hubs üzenetstream-kezelési képességet is biztosít, de olyan tulajdonságokkal rendelkezik, amelyek eltérnek a hagyományos vállalati üzenetkezelés jellemzőitől. Az Event Hubs képességei kimondottan a nagy mennyiségre és eseményfeldolgozási forgatókönyvekre vannak optimalizálva. Az Event Hubs így nem azonos a [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) üzenetküldési, és nem valósítja meg bizonyos hello képességeket elérhető [Service Bus üzenetkezelés](/azure/service-bus-messaging/) entitások, például a témakörök.

## <a name="event-hubs-features"></a>Event Hubs-szolgáltatások

Az Event Hubs hello a következő fő elemekből áll:

- [**Esemény gyártók-közzétevők**](event-hubs-features.md#event-publishers): által küldött adatok tooan eseményközpont entitás. Az esemény közzététele AMQP 1.0-n vagy HTTPS-en keresztül történik.
- [**Rögzítése**](event-hubs-features.md#capture): lehetővé teszi a streamelési adatok toocapture Event Hubs, és tárolja az Azure Blob storage-fiók.
- [**Partíciók**](event-hubs-features.md#partitions): lehetővé teszi, hogy minden fogyasztói tooonly egy adott részhalmazát, vagyis partícióját olvassa a hello eseményfelhasználó.
- [**SAS-tokenje**](event-hubs-features.md#sas-tokens): azonosítja, és ezzel hitelesíti a hello esemény-közzétevő.
- [**Eseményfelhasználó**](event-hubs-features.md#event-consumers): minden entitás, amely eseményadatokat olvas egy eseményközpontból. Az eseményfelhasználó az AMQP 1.0-n keresztül csatlakozik. 
- [**Felhasználói csoportok**](event-hubs-features.md#consumer-groups): biztosít minden több alkalmazás hello eseménystream külön láthassák fel, ezek a fogyasztók tooact egymástól függetlenül engedélyezése.
- [**Átviteli egységek**](event-hubs-features.md#capacity): előre megvásárolt kapacitásegységek. Egy partíció legfeljebb egy átviteli egységgel rendelkezhet.

Ezeket és más, az Event Hubs szolgáltatást technikai részleteiért lásd: hello [Event Hubs szolgáltatások – áttekintés](event-hubs-features.md). 

## <a name="next-steps"></a>Következő lépések

Az Event Hubs részletes díjszabási információi: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).

Az Event Hubs kapcsolatos további információkért látogasson el a következő hivatkozások hello:

* Bevezetés az [Event Hubs használatába oktatóanyag](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)
* [Az Event Hubsot használó mintaalkalmazások](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

