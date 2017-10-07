---
title: "aaaAvailability és az Azure Event Hubs következetes |} Microsoft Docs"
description: "Hogyan tooprovide hello maximális mennyisége rendelkezésre állását és az Azure Event Hubs használatával egységesítése partíciót."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a8ededaae1589830da21cb8910ca694d2d628bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a>Rendelkezésre állás és a konzisztencia az Event Hubs

## <a name="overview"></a>Áttekintés
Az Azure Event Hubs használ egy [modell particionálás](event-hubs-features.md#partitions) tooimprove rendelkezésre állását és párhuzamos folyamatkezelést biztosítja egyetlen eseményközpont belül. Például ha egy eseményközpontba négy partíciót, és ezek a partíciók egyik áthelyeznek egy kiszolgáló tooanother terheléselosztási műveletet, akkor is továbbra is küldhet és fogadhat három más partíciókból. Emellett több partíciót rendelkező lehetővé teszi toohave a teljes átviteli sebesség növelése több egyidejű olvasók feldolgozni az adatokat. Particionálás és az elosztott rendszer rendelés hello következményei megértése, egyik fontos szempontja, hogy a megoldás kialakításának.

toohelp hello kompromisszum közötti rendezés és a rendelkezésre állási ismertetik, lásd: hello [CAP tétel](https://en.wikipedia.org/wiki/CAP_theorem), más néven sörgyár tartozó tétel. A tétel hello választott közötti konzisztencia, a rendelkezésre állás és a partíció képesség ismerteti.

Sörgyár tartozó tétel meghatározza, hogy konzisztencia és rendelkezésre állás az alábbiak szerint:
* Partícióazonosító tolerancia: hello azon képessége, egy adatfeldolgozási rendszer toocontinue adatok feldolgozása még akkor is, ha a partíció hiba történik.
* Rendelkezésre állás: nem hibás csomópont választ ad vissza egy ésszerű (a nem hibák vagy időtúllépés) elfogadható időn belül.
* Konzisztencia: olvasási garantáltan tooreturn hello legutóbbi írása az adott ügyfél.

## <a name="partition-tolerance"></a>Partíció tolerancia
Az Event Hubs egy particionált adatmodell épül. Konfigurálható hello partíciók száma az eseményközpont a telepítés során, de nem ez az érték később módosíthatja. Az Event Hubs partíciók kell használnia, mert van toomake rendelkezésre állás és a konzisztencia kapcsolatos döntést az alkalmazás.

## <a name="availability"></a>Rendelkezésre állás
hello legegyszerűbb módja tooget használatába Event Hubs az toouse hello alapértelmezés. Ha létrehoz egy új `EventHubClient` objektumra, és használja a hello `Send` metódust, az eseményeket a rendszer automatikusan terjeszt a partíciók az eseményközpont között. Ez a viselkedés lehetővé teszi a hello legnagyobb mennyisége idő.

Használati esetek hello maximális idő szükséges ez a modell használata ajánlott.

## <a name="consistency"></a>Konzisztencia
Bizonyos esetekben hello események sorrendje fontos lehet. Például érdemes lehet a háttér-rendszer tooprocess egy frissítési parancs előtt a Törlés parancsot. Ebben a példában, vagy hello partíciós kulcs egy olyan eseményre, vagy használhatja a `PartitionSender` objektum tooonly küldése események tooa bizonyos partíció. Ezzel biztosítja, hogy ezek az események olvasása hello partícióról, olvasott sorrendben.

Ezzel a konfigurációval vegye figyelembe, hogy ha küldi hello adott partíció toowhich nem érhető el, kapni fog egy hibaüzenetet. Összehasonlítási pontként Ha nem rendelkezik egy kapcsolat tooa egypartíciós, hello Event Hubs szolgáltatás küldi el a esemény toohello következő elérhető partíció.

Egy lehetséges megoldás tooensure idő, is Mindeközben rendelési lenne tooaggregate események alkalmazás feldolgozása az esemény részeként. Ennek legegyszerűbb módja tooaccomplish hello az toostamp az esemény egyéni feladatütemezési számú tulajdonsággal. a következő kód hello példáját mutatja be:

```csharp
// Get hello latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

Ebben a példában elküldi az esemény tooone hello rendelkezésre álló partíciók az eseményközpont, és beállítja a megfelelő sorszám hello az alkalmazásból. Ez a megoldás által a feldolgozás alkalmazás állapota toobe igényel, de lehetővé a feladók nagy valószínűséggel toobe rendelkezésre álló végpontok.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs szolgáltatás – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
