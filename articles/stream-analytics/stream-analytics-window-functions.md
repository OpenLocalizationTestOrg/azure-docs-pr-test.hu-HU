---
title: "aaaIntroduction tooStream Analytics ablak Funkciók |} Microsoft Docs"
description: "Ismerje meg, függvények hello három ablakban a Stream Analytics (átfedésmentes, hopping, a késleltetett)."
keywords: "átfedésmentes ablak késleltetett ablakban hopping ablak"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a>Bevezetés tooStream Analytics ablak Funkciók
Az adatfolyam-forgatókönyvek sok valós idejű szükséges tooperform műveleteket csak időbeli windows hello adatait is. Ablakozó függvény natív támogatása, amely helyezi át hello tű fejlesztést tesz lehetővé az összetett adatfolyam jelentésfeldolgozó feladatok szerzői Azure Stream Analytics alapfunkciója. A Stream Analytics lehetővé teszi, hogy a fejlesztők toouse [ **Átfedésmentes**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) és [ **csúszó** ](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform historikus műveleteket a streamelési adatok. Érdemes megjegyezni, hogy minden [ablak](https://msdn.microsoft.com/library/dn835019.aspx) műveletek kimeneti hello eredményeket **end** hello ablak. hello kimenet hello ablak lesz egyszeri esemény alapján hello aggregátumfüggvényt használja. hello esemény hello időbélyegzőjét hello ablak hello végéhez lesz, és az összes ablak függvényeket rögzített hosszúságú. Végül, amely minden ablak függvényt kell használni a fontos toonote egy [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) záradékban.

![Stream Analytics ablak Funkciók fogalmak](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Átfedésmentes ablak
Átfedésmentes ablak Funkciók használt toosegment történő különböző idő adatfolyam, és például az alábbi példa hello előzetesen, a művelet végrehajtására szolgál(nak). hello kulcs differentiators Átfedésmentes ablak, hogy azok ismételje meg a, nem lehetnek átfedésben és az esemény nem tartozhat toomore, mint egy átfedésmentes ablak.

![Stream Analytics ablak Funkciók átfedésmentes – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Ugróablak
Szórási ablak Funkciók Ugrás előre meghatározott időtartamon által. Így események tartozhatnak toomore, mint egy Hopping ablak eredménykészlet Átfedésmentes windows egymást átfedő is, mint azok könnyen toothink lehet. toomake Hopping ablak hello ugyanaz, mint egy egyszerűen kellene megadnia Átfedésmentes ablak hello ugrási méretének toobe hello ugyanaz, mint hello ablakméret. 

![Stream Analytics ablak működik szórási – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Csúszóablak
Mozgó ablak Funkciók Átfedésmentes és a windows, Hopping előállít egy kimeneti **csak** egy esemény bekövetkezésekor. Minden időszak lesz legalább egy eseményt, és hello ablak folyamatosan mozgatja előre egy "(epszilon). Például a Hopping Windows események toomore, mint egy késleltetett ablak is tartozhatnak.

![Bevezetés a késleltetett Stream Analytics ablak Funkciók](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Az ablak functions kapcsolatos segítség kérése
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

