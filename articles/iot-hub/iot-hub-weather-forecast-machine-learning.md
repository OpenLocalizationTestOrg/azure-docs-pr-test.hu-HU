---
title: "Időjárás előrejelzési adatokat az IoT-központ Azure Machine Learning használatával |} Microsoft Docs"
description: "Használata Azure Machine Learning eső esélyét előre jelezni az IoT hub gyűjti össze az érzékelő hőmérséklet és a páratartalom adatok alapján."
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
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Az érzékelő adatokat az IoT hub használata az Azure Machine Learning előrejelzési időjárási

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Gépi tanulás a technika, amely segít a számítógépek ismerje meg, az előrejelzési algoritmus jövőbeli történéseket, eredményeket vagy trendeket meglévő adatokból tudományos adatok. Az Azure Machine Learning egy felhőalapú prediktív elemzési szolgáltatás, amely lehetővé teszi elemzési megoldásként használható prediktív modellek gyors létrehozását és üzembe helyezését.

## <a name="what-you-learn"></a>Ismertetett témák

Időjárás előrejelzés (eső esélyét) az Azure Machine Learning segítségével megtanulhatja a hőmérséklet és a páratartalom adatokat az Azure IoT hub használatával. Az esélye, eső egy előkészített időjárási előrejelzési modell kimeneti. A modell az előrejelzési algoritmus alapján a hőmérséklet és a páratartalom eső esélyét régebbi adatok épül.

## <a name="what-you-do"></a>Mit

- Időjárás előrejelzési modell rendszerbe állítása webszolgáltatásként.
- Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.
- A Stream Analytics-feladat létrehozása, és a feladat konfigurálása:
  - Hőmérséklet és a páratartalom adatokat olvasni az IoT hub.
  - Eső lehetősége a webszolgáltatás hívására.
  - Az eredmény mentése az Azure blob Storage tárolóban.
- A Microsoft Azure Tártallózó segítségével megtekintheti a időjárás.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:
  - Aktív Azure-előfizetés.
  - Az előfizetéshez tartozó Azure IoT hub.
  - Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.
- Egy Azure Machine Learning Studio-fiók. ([Machine Learning Studio ingyenes próbálja](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Időjárás előrejelzési modell rendszerbe állítása egy webszolgáltatás

1. Lépjen a [időjárási előrejelzési modell lap](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Kattintson a **Megnyitás a Studióban** a Microsoft Azure Machine Learning Studióban.
   ![Nyissa meg az időjárási előrejelzési modell oldal a Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Kattintson a **futtatása** a lépéseket a modell ellenőrzése. Ez a lépés befejezéséhez 2 percig is eltarthat.
   ![Nyissa meg a időjárási előrejelzési modellt az Azure Machine Learning Studióban](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Kattintson a **WEBSZOLGÁLTATÁS beállítása** > **prediktív webszolgáltatás**.
   ![Az Azure Machine Learning Studióban időjárási előrejelzési modell rendszerbe állítása](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Az ábrán, húzza a **webszolgáltatás bemenetét** modul valahol közelében a **Score Model** modul.
1. Csatlakozás a **webszolgáltatás bemenetét** modult a **Score Model** modul.
   ![Csatlakozás az Azure Machine Learning Studióban két modulok](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Kattintson a **futtatása** a lépéseket a modell ellenőrzése.
1. Kattintson a **WEBES szolgáltatás telepítése** a modell rendszerbe webszolgáltatásként.
1. A modell az irányítópulton, töltse le a **Excel 2010 vagy korábbi munkafüzet** a **kérelem/válasz**.

   > [!Note]
   > Győződjön meg arról, hogy töltse le a **Excel 2010 vagy korábbi munkafüzet** még akkor is, ha az Excel újabb verziójában futtat a számítógépen.

   ![Töltse le az Excel, a kérelem-válasz végpont](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Nyissa meg az Excel-munkafüzetben, jegyezze fel a **WEBES szolgáltatás URL-címe** és **hozzáférési kulcs**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása

### <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása

1. Az a [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.
1. Adja meg a feladat a következő információkat.

   **Feladat neve**: a feladat nevét. A névnek globálisan egyedinek kell lennie.

   **Erőforráscsoport**: használja ugyanazt az erőforráscsoportot, amely az IoT hub használja.

   **Hely**: ugyanazt a helyet használja a erőforráscsoportként működnek.

   **Rögzítés az irányítópulton**: ezt a beállítást, az egyszerű elérés érdekében ellenőrizze, hogy az IoT hub az irányítópultról.

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-an-input-to-the-stream-analytics-job"></a>A Stream Analytics-feladat bemenete hozzáadása

1. Nyissa meg a Stream Analytics-feladat.
1. A **feladat topológia**, kattintson a **bemenetek**.
1. Az a **bemenetek** ablaktáblában kattintson **Hozzáadás**, és írja be a következő információkat:

   **A bemeneti alias**: a bemeneti egyedi alias.

   **Forrás**: válasszon **IoT-központ**.

   **Felhasználói csoport**: válassza ki a létrehozott fogyasztói csoportot.

   ![A Stream Analytics-feladat bemenete hozzáadása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adja hozzá egy kimeneti a Stream Analytics-feladat

1. A **feladat topológia**, kattintson a **kimenetek**.
1. Az a **kimenetek** ablaktáblán kattintson a **Hozzáadás**, és írja be a következő információkat:

   **A kimeneti alias**: az egyedi alias a kimeneti oldal számára.

   **Gyűjtése**: válasszon **Blob-tároló**.

   **A tárfiók**: A tárfiók a blob-tároló. Hozzon létre egy tárfiókot, vagy használjon egy meglévőt.

   **Tároló**: A tároló, a blob menteni. Hozzon létre egy tárolót, vagy használjon egy meglévőt.

   **Esemény szerializálási formátum**: válasszon **CSV**.

   ![Egy kimeneti hozzáadása az Azure Stream Analytics-feladat](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>A Stream Analytics-feladat üzembe helyezett webszolgáltatás hívására függvény hozzáadása

1. A **feladat topológia**, kattintson a **funkciók** > **Hozzáadás**.
1. Adja meg a következő információkat:

   **Alias működéséhez**: Adjon meg `machinelearning`.

   **Típus működéséhez**: válasszon **Azure ML**.

   **Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.

   **URL-cím**: Adja meg a WEBSZOLGÁLTATÁS URL-címe le feljegyzett az Excel-munkafüzetből.

   **Kulcs**: írja be a hozzáférési kulcs le feljegyzett az Excel-munkafüzetből.

   ![Egy függvény hozzáadása az Azure Stream Analytics-feladat](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>A lekérdezést a Stream Analytics-feladat konfigurálása

1. A **feladat topológia**, kattintson a **lekérdezés**.
1. Cserélje le a meglévő kódot az alábbira:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Cserélje le `[YourInputAlias]` a bemeneti áljel a feladat.

   Cserélje le `[YourOutputAlias]` a kimeneti aliasnév a feladat.

1. Kattintson a **Save** (Mentés) gombra.

### <a name="run-the-stream-analytics-job"></a>A Stream Analytics-feladat futtatása

Kattintson a Stream Analytics-feladat **Start** > **most** > **Start**. Ha a feladat sikeresen elindul, a feladat állapota a **leállítva** való **futtató**.

![A Stream Analytics-feladat futtatása](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>A Microsoft Azure Tártallózó segítségével megtekintheti a időjárás:

Futtassa az ügyfélalkalmazás összegyűjtése és az IoT hub hőmérséklet és a páratartalom adatokat küldi el. Minden üzenet, amely az IoT hub fogad a Stream Analytics-feladat meghívja a időjárás webszolgáltatás eső esélyét létrehozásához. Az eredmény ezután menti az Azure blob storage. Az Azure Tártallózó olyan eszköz, amely segítségével a eredmény.

1. [Töltse le és telepítse a Microsoft Azure Tártallózó](http://storageexplorer.com/).
1. Nyissa meg az Azure Storage Explorert.
1. Jelentkezzen be az Azure-fiókjával.
1. Válassza ki előfizetését.
1. Kattintson az előfizetés > **Tárfiókok** > a tárfiók > **Blobtárolók** > a tárolóban.
1. Nyisson meg egy CSV-fájlt az eredményt. Az utolsó oszlopban az esélye, eső rögzíti.

   ![Időjárás eredmény Azure Machine Learning segítségével](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Összefoglalás

Már használta sikeresen Azure Machine Learning az esélye, eső, amely az IoT hub megkapja a hőmérséklet és a páratartalom adatok alapján történő létrehozásához.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]