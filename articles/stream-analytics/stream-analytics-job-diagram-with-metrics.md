---
title: "aaa Azure Stream Analytics adatvezérelt hibakeresést keresztül hello feladat diagram |} Microsoft Docs"
description: "A Stream Analytics-feladat hibaelhárítása hello feladat ábra és metrikák használatával."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a>Adatalapú hibakeresést keresztül hello feladat diagramja

hello feladat ábra: hello **figyelés** panel az Azure-portálon hello segítségével jelenítheti meg a feladat folyamat. Azt illusztrálja, bemenetek, kimenetek és lekérdezések lépéseit. Egyes lépéseihez szükséges hello feladat ábra tooexamine hello metrikák is használhat, toomore gyorsan különítse el a probléma forrásának hello összefüggő problémák megoldásakor.

## <a name="using-hello-job-diagram"></a>Hello feladat ábra használatával

Az Azure-portálon hello alatt a Stream Analytics-feladatot, miközben **támogatási + hibaelhárítás**, jelölje be **feladat ábra**:

![A metrika - hely feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

A lekérdezés-szerkesztő ablakban jelölje ki minden egyes lekérdezés lépés toosee hello megfelelő szakaszához. Hello lépés metrika diagramot az alsó ablaktáblában hello lapon jelenik meg.

![A metrika - alapvető feladat feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Válassza ki a toosee hello partíciók hello Azure Event Hubs bemeneti, **...** A helyi menü megjelenik. Hello bemeneti egyesülés is látható.

![A metrika - feladat ábra bontsa ki a partíció](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

toosee hello metrika diagramot csak egy olyan partíciót, jelölje be hello partíció csomópont. hello metrikák hello lap hello alján jelennek meg.

![A metrika - további metrikák feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

toosee hello metrikák diagramot egyesülés, jelölje be hello egyesülés csomópont. a következő diagram hello látható, hogy egyetlen esemény sem volt eldobni módosul.

![A metrika - feladat ábra rács](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

toosee hello részletei hello metrika értékét, és az idő, pont toohello diagram.

![Diagram metrikákat a sikertelen feladat - vigye](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Hibaelhárítása mérőszámok segítségével

Hello **QueryLastProcessedTime** metrika azt jelzi, ha egy adott lépésre adatokat kapott. Hello topológia megtekintésével is dolgozhat visszafelé hello kimeneti processzor toosee mely lépés nem kapja az adatokat. Ha a lépés nem sikerül adatokat, nyissa meg azt megelőzően toohello lekérdezés lépést. Ellenőrizze, hogy hello előző lekérdezés lépést tartalmaz egy olyan időkeretet, és hogy elég idő telt meg toooutput adatokat. (Megjegyzés: Ez idő windows illesztett toohello óra.)
 
Ha hello előző lekérdezés lépés egy bemeneti processzor, akkor hello bemeneti metrikák toohelp válasz hello célzott kérdések a következő. Segít meghatározni, hogy egy feladat van adat lekérésekor a bemeneti forrásból. Ha hello lekérdezés particionálása, vizsgálja meg az egyes partíciók.
 
### <a name="how-much-data-is-being-read"></a>Mennyi adatot olvasása?

*   **InputEventsSourcesTotal** hello olvasható adatok egységek száma. Például hello számú blobot.
*   **InputEventsTotal** események olvasása hello száma. Ez a metrika partíciónként érhető el.
*   **InputEventsInBytesTotal** hello olvasott bájtok száma.
*   **InputEventsLastArrivalTime** minden fogadott események a várólistában levő időpontja frissül.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Az idő soron? Ha tényleges események olvasható, előfordulhat, hogy nem adható ki absztrakt.

*   **InputEventsLastPunctuationTime** azt jelzi, ha egy absztrakt bocsátotta tookeep idő áthelyezése előre. Ha nem jelenik meg absztrakt, adatfolyama is blokkolnánk a hozzáférését.
 
### <a name="are-there-any-errors-in-hello-input"></a>Vannak-e hibák hello bemeneti?

*   **InputEventsEventDataNullTotal** null adattal rendelkező események száma.
*   **InputEventsSerializerErrorsTotal** nem deszerializálhatók megfelelően események száma.
*   **InputEventsDegradedTotal** a szerializálás megszüntetése nem problémába ütközött események száma.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Események eldobott vagy igazítva?

*   **InputEventsEarlyTotal** hello eseményeket, amelyek egy alkalmazás időbélyeg előtt hello magas vízjel száma.
*   **InputEventsLateTotal** hello eseményeket, amelyek egy alkalmazás időbélyeg után hello magas vízjel száma.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** hello számú események eldobott hello feladat kezdési időpont előtt.
 
### <a name="are-we-falling-behind-in-reading-data"></a>Amelyek azt alá tartozó az adatok olvasása közben?

*   **InputEventsSourcesBackloggedTotal** jelzi, hogy hány több üzenetet kell toobe Event Hubs és Azure IoT Hub bemeneti című témakörben találhat.


## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [Bevezetés tooStream elemzés](stream-analytics-introduction.md)
* [A Stream Analytics használatába](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-feladatok méretezése](stream-analytics-scale-jobs.md)
* [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [A Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)
