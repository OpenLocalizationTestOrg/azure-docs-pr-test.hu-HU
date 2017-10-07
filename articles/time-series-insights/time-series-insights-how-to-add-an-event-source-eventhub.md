---
title: "az Event Hubs esemény tooyour Azure idő adatsorozat Insights forráskörnyezettel aaaHow tooadd |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti, hogyan tooadd esemény forrás, amely csatlakoztatott tooan Eseményközpont tooyour idő adatsorozat Insights környezet"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: a98cd91deb51c829084a00c5f2a80b39d0fc511c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-event-hub-event-source"></a>Hogyan tooadd egy Eseményközpontba eseményforrás

Ez az oktatóanyag bemutatja, hogyan toouse hello Azure portál tooadd eseményforrást olvassa be az Eseményközpont tooyour idő adatsorozat Insights környezetben.

## <a name="prerequisites"></a>Előfeltételek

Az Eseményközpontok hozott létre, és írás események tooit. Az Event Hubs további információkért lásd: <https://azure.microsoft.com/services/event-hubs/>

> [A fogyasztói csoportok] Minden alkalommal adatsorozat Insights eseményforrás kell toohave saját dedikált fogyasztói csoportot, amelyek nincsenek megosztva, más fogyasztóval. Ha több olvasók felhasználásához származó események hello azonos fogyasztói csoportot, minden olvasók valószínűleg toosee hibák. Vegye figyelembe, hogy nincs-e is maximális hossza 20 felhasználói csoportot az Event Hubs egy. További információkért lásd: hello [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Válassza ki importálása

hello eseményforrás hello beállításait is meg lehet adni manuálisan, vagy egy eseményközpontot, amelyek a rendelkezésre álló tooyou hello eseményközpontokból származó választható ki.
A hello **importálási lehetőség** választó, válasszon egyet az alábbi beállítások hello:

* Adja meg az Event Hubs beállításokat manuálisan
* Az elérhető előfizetések Eseményközpontjának használata

### <a name="select-an-available-event-hub"></a>Válassza ki az elérhető Eseményközpontok

hello következő táblázat ismerteti a hello új esemény (forrás) lapot annak leírását az egyes lehetőségek lehetőséget választva a rendelkezésre álló Eseményközpontban, egy Eseményforrás:

| TULAJDONSÁG NEVE | LEÍRÁS |
| --- | --- |
| Eseményforrás neve | a forrás hello neve. Ez a név a idő adatsorozat Insights környezeten belül egyedinek kell lennie.
| Forrás | Válasszon **Eseményközpont** toocreate egy Eseményközpontba esemény forrását.
| Előfizetés-azonosító | Válassza ki, amelyben az eseményközpont létre lett hozva hello előfizetést.
| Service bus-névtér | Válassza ki, amely tartalmazza az Event Hubs hello hello Service Bus-névtér.
| Eseményközpont neve | Válassza ki az Event Hubs hello hello nevét.
| Eseményközpont házirend neve | Válassza ki a megosztott hello hozzáférési házirendet, amely hello Event Hub konfigurálása lapon is létrehozható. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. hello megosztott hozzáférési házirend a eseményforrás *kell* rendelkezik **olvasási** engedélyek.
| Event hub fogyasztói csoportot | hello fogyasztói csoportot tooread származó események hello Eseményközpontot. Nagyon fontos ajánlott toouse az eseményforrás a dedikált fogyasztói csoportot.

### <a name="provide-event-hub-settings-manually"></a>Adja meg az Event Hubs beállításokat manuálisan

hello következő táblázat ismerteti az egyes tulajdonságai hello új esemény (forrás) lapot a leírás beállításainak megadásakor manuálisan:

| TULAJDONSÁG NEVE | LEÍRÁS |
| --- | --- |
| Eseményforrás neve | a forrás hello neve. Ez a név a idő adatsorozat Insights környezeten belül egyedinek kell lennie.
| Forrás | Válasszon **Eseményközpont** toocreate egy Eseményközpontba esemény forrását.
| Előfizetés-azonosító | hello előfizetés, amelyben az eseményközpont létre lett hozva.
| Erőforráscsoport | hello előfizetés, amelyben az eseményközpont létre lett hozva.
| Service bus-névtér | A Service Bus-névtér az üzenetküldési entitások készletének tárolója. Egy új Eseményközpont létrehozásakor egy Szolgáltatásbusz-névtér is létrejött.
| Eseményközpont neve | az Eseményközpont hello neve. Az eseményközpont létrehozásakor meg nevet is adott neki
| Eseményközpont házirend neve | hello megosztott elérési házirendet, amely a hello Event Hub konfigurálása lapon is létrehozható. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. hello megosztott hozzáférési házirend a eseményforrás *kell* rendelkezik **olvasási** engedélyek.
| Event hub házirend kulcs | hello megosztott elérési kulcsot használt tooauthenticate hozzáférés toohello Service Bus-névtér. Ide írja be hello elsődleges vagy másodlagos kulcsot.
| Event hub fogyasztói csoportot | hello fogyasztói csoportot tooread származó események hello Eseményközpontot. Nagyon fontos ajánlott toouse az eseményforrás a dedikált fogyasztói csoportot.

## <a name="next-steps"></a>Következő lépések

1. Adja hozzá a adatok hozzáférési házirend tooyour környezet [Define adat-hozzáférési házirendek](time-series-insights-data-access.md)
1. Férhet hozzá a környezethez a hello [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com)
