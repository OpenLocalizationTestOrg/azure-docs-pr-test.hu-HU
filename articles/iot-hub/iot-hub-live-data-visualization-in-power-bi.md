---
title: "az érzékelők adatstreamének az Azure IoT Hub – Power BI aaaReal idejű adatok vizuális |} Microsoft Docs"
description: "A Power BI toovisualize hőmérséklet és a páratartalom adatokat használnak hello érzékelő gyűjtése és tooyour Azure IoT hub küldött."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "valós idejű adatok vizuális, az élő adatok vizuális érzékelő adatábrázolási"
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: d79ce757a9f2ab7a4744e8a0c523106e0f72cecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Az Azure IoT Hub Power BI használatával valós idejű érzékelőadatok megjelenítése

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Ismertetett témák

Megismerheti, hogyan toovisualize valós idejű érzékelőadatok, amely az Azure IoT hub megkapja a Power BI által. Ha azt szeretné, tootry hello adatok megjelenítése a webalkalmazásokkal az IoT hub, tekintse meg [használata Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Mit

- Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.
- Létrehozása, konfigurálása, és futtassa a Stream Analytics-feladat az adatok átvitele az IoT hub tooyour Power BI-fiókkal.
- Hozzon létre, és tegye közzé a Power BI toovisualize hello jelentésadatokat.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:
  - Aktív Azure-előfizetés.
  - Az előfizetéshez tartozó Azure IoT hub.
  - Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.
- Power BI-fiókkal. ([Próbálja ingyenesen a Power BI](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása

### <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása

1. A hello Azure-portálon, kattintson az új > az eszközök internetes hálózatát > Stream Analytics-feladat.
1. Adja meg a következő információ hello feladat hello.

   **Feladat neve**: hello feladat hello nevét. hello nevének globálisan egyedinek kell lennie.

   **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   **Hely**: használata hello ugyanaz az erőforráscsoport és a helyen.

   **PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Egy bemeneti toohello Stream Analytics-feladat hozzáadása

1. Nyissa meg hello Stream Analytics-feladat.
1. A **feladat topológia**, kattintson a **bemenetek**.
1. A hello **bemenetek** ablaktáblában kattintson **Hozzáadás**, majd adja meg a következő információ hello:

   **A bemeneti alias**: hello egyedi alias hello bemeneti.

   **Forrás**: válasszon **IoT-központ**.

   **Felhasználói csoport**: újonnan létrehozott válassza hello fogyasztói csoportot.
1. Kattintson a **Create** (Létrehozás) gombra.

   ![Egy bemeneti tooa Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a>Adja hozzá egy kimeneti toohello Stream Analytics-feladat

1. A **feladat topológia**, kattintson a **kimenetek**.
1. A hello **kimenetek** ablaktáblán kattintson a **Hozzáadás**, majd adja meg a következő információ hello:

   **A kimeneti alias**: hello egyedi alias hello kimenet.

   **Gyűjtése**: válasszon **a Power BI**.
1. Kattintson a **engedélyezés**, és jelentkezzen be a Power BI-fiókjába.
1. Követően adja meg a következő információ hello:

   **Munkaterület csoport**: válassza ki a cél csoport munkaterülettől.

   **A DataSet neve**: Adja meg a DataSet adatkészlet nevét.

   **Tábla neve**: Adjon meg egy tábla nevét.
1. Kattintson a **Create** (Létrehozás) gombra.

   ![Egy kimeneti tooa Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat lekérdezése hello konfigurálása

1. A **feladat topológia**, kattintson a **lekérdezés**.
1. Cserélje le `[YourInputAlias]` bemeneti hello aliasnév hello feladat.
1. Cserélje le `[YourOutputAlias]` hello kimeneti aliasnév hello feladat.
1. Kattintson a **Save** (Mentés) gombra.

   ![A lekérdezés tooa Stream Analytics-feladat hozzáadása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat futtatása

Hello Stream Analytics-feladat, kattintson **Start** > **most** > **Start**. Miután hello feladat sikeresen elindul, hello feladat állapota a **leállítva** túl**futtató**.

![Az Azure Stream Analytics-feladat futtatása](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a>Hozzon létre, és tegye közzé a Power BI-jelentés toovisualize hello adatokhoz

1. Ellenőrizze, hogy hello mintaalkalmazás fut-e az eszközön. Ha nem, olvassa el a toohello oktatóanyagok [beállítani az eszközét](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Jelentkezzen be tooyour [Power BI](https://powerbi.microsoft.com/en-us/) fiók.
1. Nyissa meg toohello csoport munkaterület hello kimeneti hello Stream Analytics-feladat létrehozásakor beállított.
1. Kattintson a **adatkészletek Streaming**.

   Meg kell jelenniük a felsorolt hello dataset hello Stream Analytics-feladat kimeneti hello létrehozásakor megadott.
1. A **műveletek**, kattintson a hello első ikon toocreate jelentést.

   ![Microsoft Power BI-jelentés létrehozása](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Hozzon létre egy sor diagram tooshow valós idejű hőmérséklet adott idő alatt.
   1. Hello jelentés létrehozása oldal vegye fel a vonaldiagram.
   1. A hello **mezők** ablaktáblában bontsa ki a hello kimeneti hello Stream Analytics-feladat létrehozásakor adott hello tábla.
   1. A csomóponthúzási **EventEnqueuedUtcTime** túl**tengely** a hello **képi megjelenítések** ablaktáblán.
   1. A csomóponthúzási **hőmérséklet** túl**értékek**.

      Most egy vonaldiagramot jön létre. hello x tengely dátum és idő hello UTC időzóna jeleníti meg. hello y tengely hőmérséklet-érzékelő hello jeleníti meg.

      ![A Microsoft Power BI-jelentés hőmérséklet tooa vonaldiagram hozzáadása](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Hozzon létre egy másik sor diagram tooshow valós idejű páratartalom adott idő alatt. toodo, hajtsa végre az azonos lépések fent hello és helyezze **EventEnqueuedUtcTime** hello ábrázolja és **páratartalom** hello y tengelyen.

   ![A Microsoft Power BI-jelentés páratartalom tooa vonaldiagram hozzáadása](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Kattintson a **mentése** toosave hello jelentés.
1. Kattintson a **fájl** > **tooweb közzététele**.
1. Kattintson a **hozzon létre beágyazási kódot**, és kattintson a **közzététel**.

Hello jelentés hivatkozása, amelyeket megoszthat a bárki, aki a jelentéskészítő elérése és kód részlet toointegrate hello jelentést a saját blogba vagy webhelyre van megadva.

![Microsoft Power BI-jelentés közzététele](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

A Microsoft emellett hello [Power BI mobilalkalmazásokkal](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) megtekintéséhez és interaktív a Power BI-irányítópult és jelentések használata a mobileszközön.

## <a name="next-steps"></a>Következő lépések

Az Azure IoT hub a Power BI toovisualize valós idejű érzékelőadatok sikeresen használt.
Nincs Azure IoT-központot egy másik módja toovisualize adatait. Lásd: [használata Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
