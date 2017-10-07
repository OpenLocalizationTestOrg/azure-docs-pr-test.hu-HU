---
title: "aaaWeather előrejelzési adatokat az IoT-központ Azure Machine Learning használatával |} Microsoft Docs"
description: "Használja az Azure Machine Learning hello hőmérséklet és a páratartalom az IoT hub gyűjti össze az érzékelő adatokat alapján eső toopredict hello esélyét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "időjárás: gépi tanulás"
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 04abe97558ccfc152bae2e0d435033433c0023dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Időjárás: hello érzékelő adatokat az IoT hub használata az Azure Machine Learning

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Gépi tanulás a technika, amely segítségével a meglévő adatok tooforecast jövőbeli történéseket, eredményeket vagy trendeket megtudjuk számítógépek tudományos adatok. Az Azure Machine Learning egy felhőalapú prediktív elemzési szolgáltatás, amely megkönnyíti a lehetséges tooquickly létrehozása és üzembe prediktív modelleket elemzési megoldásként.

## <a name="what-you-learn"></a>Ismertetett témák

Megtudhatja, hogyan hello hőmérséklet és a páratartalom adatait az Azure IoT hub toouse Azure Machine Learning toodo időjárás (eső esélyét) használatával. hello esélyét eső egy hello kimenet egy előkészített időjárási előrejelzési modell. hello modell a hőmérséklet és a páratartalom alapján eső régebbi adatok tooforecast esélyét épül.

## <a name="what-you-do"></a>Mit

- Hello időjárási előrejelzési modell rendszerbe állítása webszolgáltatásként.
- Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.
- A Stream Analytics-feladat létrehozása, és a feladat hello konfigurálása:
  - Hőmérséklet és a páratartalom adatokat olvasni az IoT hub.
  - Hello web service tooget hello eső alkalommal hívja.
  - Mentse a hello eredmény tooan Azure blob Storage tárolóban.
- A Microsoft Azure Tártallózó tooview hello időjárás használja.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:
  - Aktív Azure-előfizetés.
  - Az előfizetéshez tartozó Azure IoT hub.
  - Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.
- Egy Azure Machine Learning Studio-fiók. ([Machine Learning Studio ingyenes próbálja](https://studio.azureml.net/)).

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a>Egy webszolgáltatás hello időjárási előrejelzési modell rendszerbe állítása

1. Nyissa meg toohello [időjárási előrejelzési modell lap](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Kattintson a **Megnyitás a Studióban** a Microsoft Azure Machine Learning Studióban.
   ![Megnyitás hello időjárási előrejelzési modell oldal a Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Kattintson a **futtatása** toovalidate hello lépések hello modellben. Ebben a lépésben toocomplete 2 percet is igénybe vehet.
   ![Nyissa meg hello időjárási előrejelzési modell az Azure Machine Learning Studióban](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Kattintson a **WEBSZOLGÁLTATÁS beállítása** > **prediktív webszolgáltatás**.
   ![Hello időjárási előrejelzési modell Azure Machine Learning Studio telepítése](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Hello a diagramon, húzza a hello **webszolgáltatás bemenetét** valahol közelében hello modul **Score Model** modul.
1. Csatlakozás hello **webszolgáltatás bemenetét** modul toohello **Score Model** modul.
   ![Csatlakozás az Azure Machine Learning Studióban két modulok](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Kattintson a **futtatása** toovalidate hello lépések hello modellben.
1. Kattintson a **WEBES szolgáltatás telepítése** toodeploy hello modell webszolgáltatásként.
1. Hello irányítópult hello modell, töltse le a hello **Excel 2010 vagy korábbi munkafüzet** a **kérelem/válasz**.

   > [!Note]
   > Győződjön meg arról, hogy töltse le a hello **Excel 2010 vagy korábbi munkafüzet** még akkor is, ha az Excel újabb verziójában futtat a számítógépen.

   ![Töltse le a hello Excel hello kérelem-válasz végpont](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Nyissa meg a hello Excel-munkafüzetben, jegyezze fel a hello **WEBES szolgáltatás URL-címe** és **hozzáférési kulcs**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása

### <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása

1. A hello [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.
1. Adja meg a következő információ hello feladat hello.

   **Feladat neve**: hello feladat hello nevét. hello nevének globálisan egyedinek kell lennie.

   **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   **Hely**: használata hello ugyanaz az erőforráscsoport és a helyen.

   **PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Egy bemeneti toohello Stream Analytics-feladat hozzáadása

1. Nyissa meg hello Stream Analytics-feladat.
1. A **feladat topológia**, kattintson a **bemenetek**.
1. A hello **bemenetek** ablaktáblában kattintson **Hozzáadás**, majd adja meg a következő információ hello:

   **A bemeneti alias**: hello egyedi alias hello bemeneti.

   **Forrás**: válasszon **IoT-központ**.

   **Felhasználói csoport**: Select hello fogyasztói csoportot hozott létre.

   ![Egy bemeneti toohello Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-an-output-toohello-stream-analytics-job"></a>Adja hozzá egy kimeneti toohello Stream Analytics-feladat

1. A **feladat topológia**, kattintson a **kimenetek**.
1. A hello **kimenetek** ablaktáblán kattintson a **Hozzáadás**, majd adja meg a következő információ hello:

   **A kimeneti alias**: hello egyedi alias hello kimenet.

   **Gyűjtése**: válasszon **Blob-tároló**.

   **A tárfiók**: hello tárfiók a blob Storage. Hozzon létre egy tárfiókot, vagy használjon egy meglévőt.

   **Tároló**: hello tároló hello blob menteni. Hozzon létre egy tárolót, vagy használjon egy meglévőt.

   **Esemény szerializálási formátum**: válasszon **CSV**.

   ![Egy kimeneti toohello Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a>Egy függvény toohello Stream Analytics feladat toocall hello webszolgáltatás üzembe helyezett hozzáadása

1. A **feladat topológia**, kattintson a **funkciók** > **Hozzáadás**.
1. Adja meg a következő információ hello:

   **Alias működéséhez**: Adjon meg `machinelearning`.

   **Típus működéséhez**: válasszon **Azure ML**.

   **Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.

   **URL-cím**: Adja meg a WEBSZOLGÁLTATÁS URL-címe le feljegyzett hello hello Excel-munkafüzetből.

   **Kulcs**: írja be a hozzáférési kulcs le feljegyzett hello hello Excel-munkafüzetből.

   ![Egy függvény toohello Stream Analytics-feladat hozzáadása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat lekérdezése hello konfigurálása

1. A **feladat topológia**, kattintson a **lekérdezés**.
1. Cserélje le a következő kód hello hello meglévő kódot:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Cserélje le `[YourInputAlias]` bemeneti hello aliasnév hello feladat.

   Cserélje le `[YourOutputAlias]` hello kimeneti aliasnév hello feladat.

1. Kattintson a **Save** (Mentés) gombra.

### <a name="run-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat futtatása

Hello Stream Analytics-feladat, kattintson **Start** > **most** > **Start**. Miután hello feladat sikeresen elindul, hello feladat állapota a **leállítva** túl**futtató**.

![Hello Stream Analytics-feladat futtatása](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a>Használja a Microsoft Azure Tártallózó tooview hello időjárás:

Hello ügyfél alkalmazás toostart összegyűjtése és elküldését a hőmérséklet és a páratartalom adatok tooyour IoT-központ futtatása. Minden üzenet, amely megkapja az IoT hub hello Stream Analytics-feladat hello időjárás web service tooproduce hello esélyét eső hívja. hello eredmény majd mentett tooyour Azure blob Storage tárolóban. Az Azure Tártallózó egy olyan eszköz, amelyeket felhasználhat tooview hello eredménye.

1. [Töltse le és telepítse a Microsoft Azure Tártallózó](http://storageexplorer.com/).
1. Nyissa meg az Azure Storage Explorert.
1. Bejelentkezés tooyour Azure-fiók.
1. Válassza ki előfizetését.
1. Kattintson az előfizetés > **Tárfiókok** > a tárfiók > **Blobtárolók** > a tárolóban.
1. Nyissa meg a .csv fájl toosee hello eredményt. hello utolsó oszlop rekordok hello eső esélyét.

   ![Időjárás eredmény Azure Machine Learning segítségével](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Összefoglalás

Sikeresen használta az Azure Machine Learning tooproduce hello hello hőmérséklet és a páratartalom adatot, amely megkapja az IoT hub eső esélyét.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]