---
title: "egy esemény tooyour Azure idő adatsorozat Insights forráskörnyezettel aaaAdd |} Microsoft Docs"
description: "Ebben az oktatóanyagban csatlakoztat egy esemény tooyour idő adatsorozat Insights forráskörnyezet"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 817df5e81cb4dc3d7376914a4651aabebadbcc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Hozzon létre egy eseményforrás idő adatsorozat Insights környezetnek hello Ibiza portálon

Egy Time Series Insights-eseményforrás egy eseményközvetítőből, például az Azure Event Hubsból származik. Idő adatsorozat Insights keresztül közvetlenül tooEvent források választásával dolgozhat fel hello adatfolyam anélkül, hogy a felhasználók toowrite egyetlen sor kódot. A Time Series Insights jelenleg az Azure Event Hubs és Azure IoT Hubs forrásokat támogatja. A jövőbeli hello további eseményforrások lesz hozzáadva.

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a>Egy esemény forráskörnyezettel tooyour lépéseket tooadd

1.  Jelentkezzen be toohello [Ibiza portálon](https://portal.azure.com).
2.  Kattintson az "Összes erőforrás" hello menü hello hello Ibiza portálon a bal oldalán.
3.  Válassza ki az Azure Time Series Insights-környezetet.

  ![Hello idő adatsorozat Insights eseményforrás létrehozása](media/add-event-source/getstarted-create-event-source-1.png)

4.  Válassza az „Eseményforrások” lehetőséget, és kattintson a „+ Hozzáadás” gombra.

  ![Hozzon létre hello idő adatsorozat Insights eseményforrás - részletek](media/add-event-source/getstarted-create-event-source-2.png)

5.  Adja meg hello hello esemény forrását. Ez a név az eseményforrástól származó összes eseményhez hozzá lesz rendelve, és a lekérdezéskor elérhető.
6.  Válassza ki az eseményközpontok a hello Eseményközpont erőforrások hello az aktuális előfizetésben. Máskülönben válassza az importálási beállítást "adja meg az Event Hubs-beállítások manuális" toospecify egy eseményközpontba egy másik előfizetésben. Az eseményeket JSON formátumban kell közzétenni.
7.  Válassza ki, hogy rendelkezik-e olvasási engedéllyel a hello eseményközpont házirendet.
8.  Határozza meg az eseményközpont fogyasztói csoportját.

  > [!IMPORTANT]
  > Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet). Fogyasztói csoportot más szolgáltatások használata esetén olvassa el a művelet negatívan befolyásolja az ebben a környezetben, és más szolgáltatások hello. Ha a "$Default" fogyasztói csoportot hello használ, előfordulhat toopotential újbóli más olvasók által.

9.  Kattintson a „Létrehozás” elemre.

Hello eseményforrás létrehozása, után idő adatsorozat Insights automatikusan elindítja adatfolyam környezetébe.

## <a name="next-steps"></a>Következő lépések

* [Események küldése](time-series-insights-send-events.md) toohello eseményforrás
* A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)
