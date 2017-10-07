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
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="95b1d-104">Időjárás: hello érzékelő adatokat az IoT hub használata az Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="95b1d-104">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="95b1d-106">Gépi tanulás a technika, amely segítségével a meglévő adatok tooforecast jövőbeli történéseket, eredményeket vagy trendeket megtudjuk számítógépek tudományos adatok.</span><span class="sxs-lookup"><span data-stu-id="95b1d-106">Machine learning is a technique of data science that helps computers learn from existing data tooforecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="95b1d-107">Az Azure Machine Learning egy felhőalapú prediktív elemzési szolgáltatás, amely megkönnyíti a lehetséges tooquickly létrehozása és üzembe prediktív modelleket elemzési megoldásként.</span><span class="sxs-lookup"><span data-stu-id="95b1d-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible tooquickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="95b1d-108">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="95b1d-108">What you learn</span></span>

<span data-ttu-id="95b1d-109">Megtudhatja, hogyan hello hőmérséklet és a páratartalom adatait az Azure IoT hub toouse Azure Machine Learning toodo időjárás (eső esélyét) használatával.</span><span class="sxs-lookup"><span data-stu-id="95b1d-109">You learn how toouse Azure Machine Learning toodo weather forecast (chance of rain) using hello temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="95b1d-110">hello esélyét eső egy hello kimenet egy előkészített időjárási előrejelzési modell.</span><span class="sxs-lookup"><span data-stu-id="95b1d-110">hello chance of rain is hello output of a prepared weather prediction model.</span></span> <span data-ttu-id="95b1d-111">hello modell a hőmérséklet és a páratartalom alapján eső régebbi adatok tooforecast esélyét épül.</span><span class="sxs-lookup"><span data-stu-id="95b1d-111">hello model is built upon historic data tooforecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="95b1d-112">Mit</span><span class="sxs-lookup"><span data-stu-id="95b1d-112">What you do</span></span>

- <span data-ttu-id="95b1d-113">Hello időjárási előrejelzési modell rendszerbe állítása webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="95b1d-113">Deploy hello weather prediction model as a web service.</span></span>
- <span data-ttu-id="95b1d-114">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="95b1d-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="95b1d-115">A Stream Analytics-feladat létrehozása, és a feladat hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="95b1d-115">Create a Stream Analytics job and configure hello job to:</span></span>
  - <span data-ttu-id="95b1d-116">Hőmérséklet és a páratartalom adatokat olvasni az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="95b1d-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="95b1d-117">Hello web service tooget hello eső alkalommal hívja.</span><span class="sxs-lookup"><span data-stu-id="95b1d-117">Call hello web service tooget hello rain chance.</span></span>
  - <span data-ttu-id="95b1d-118">Mentse a hello eredmény tooan Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="95b1d-118">Save hello result tooan Azure blob storage.</span></span>
- <span data-ttu-id="95b1d-119">A Microsoft Azure Tártallózó tooview hello időjárás használja.</span><span class="sxs-lookup"><span data-stu-id="95b1d-119">Use Microsoft Azure Storage Explorer tooview hello weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="95b1d-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="95b1d-120">What you need</span></span>

- <span data-ttu-id="95b1d-121">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="95b1d-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="95b1d-122">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="95b1d-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="95b1d-123">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="95b1d-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="95b1d-124">Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="95b1d-124">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="95b1d-125">Egy Azure Machine Learning Studio-fiók.</span><span class="sxs-lookup"><span data-stu-id="95b1d-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="95b1d-126">([Machine Learning Studio ingyenes próbálja](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="95b1d-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="95b1d-127">Egy webszolgáltatás hello időjárási előrejelzési modell rendszerbe állítása</span><span class="sxs-lookup"><span data-stu-id="95b1d-127">Deploy hello weather prediction model as a web service</span></span>

1. <span data-ttu-id="95b1d-128">Nyissa meg toohello [időjárási előrejelzési modell lap](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="95b1d-128">Go toohello [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="95b1d-129">Kattintson a **Megnyitás a Studióban** a Microsoft Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="95b1d-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="95b1d-130">![Megnyitás hello időjárási előrejelzési modell oldal a Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="95b1d-130">![Open hello weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="95b1d-131">Kattintson a **futtatása** toovalidate hello lépések hello modellben.</span><span class="sxs-lookup"><span data-stu-id="95b1d-131">Click **Run** toovalidate hello steps in hello model.</span></span> <span data-ttu-id="95b1d-132">Ebben a lépésben toocomplete 2 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="95b1d-132">This step might take 2 minutes toocomplete.</span></span>
   <span data-ttu-id="95b1d-133">![Nyissa meg hello időjárási előrejelzési modell az Azure Machine Learning Studióban](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="95b1d-133">![Open hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="95b1d-134">Kattintson a **WEBSZOLGÁLTATÁS beállítása** > **prediktív webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="95b1d-135">![Hello időjárási előrejelzési modell Azure Machine Learning Studio telepítése](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="95b1d-135">![Deploy hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="95b1d-136">Hello a diagramon, húzza a hello **webszolgáltatás bemenetét** valahol közelében hello modul **Score Model** modul.</span><span class="sxs-lookup"><span data-stu-id="95b1d-136">In hello diagram, drag hello **Web service input** module somewhere near hello **Score Model** module.</span></span>
1. <span data-ttu-id="95b1d-137">Csatlakozás hello **webszolgáltatás bemenetét** modul toohello **Score Model** modul.</span><span class="sxs-lookup"><span data-stu-id="95b1d-137">Connect hello **Web service input** module toohello **Score Model** module.</span></span>
   <span data-ttu-id="95b1d-138">![Csatlakozás az Azure Machine Learning Studióban két modulok](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="95b1d-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="95b1d-139">Kattintson a **futtatása** toovalidate hello lépések hello modellben.</span><span class="sxs-lookup"><span data-stu-id="95b1d-139">Click **RUN** toovalidate hello steps in hello model.</span></span>
1. <span data-ttu-id="95b1d-140">Kattintson a **WEBES szolgáltatás telepítése** toodeploy hello modell webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="95b1d-140">Click **DEPLOY WEB SERVICE** toodeploy hello model as a web service.</span></span>
1. <span data-ttu-id="95b1d-141">Hello irányítópult hello modell, töltse le a hello **Excel 2010 vagy korábbi munkafüzet** a **kérelem/válasz**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-141">On hello dashboard of hello model, download hello **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="95b1d-142">Győződjön meg arról, hogy töltse le a hello **Excel 2010 vagy korábbi munkafüzet** még akkor is, ha az Excel újabb verziójában futtat a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="95b1d-142">Ensure that you download hello **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Töltse le a hello Excel hello kérelem-válasz végpont](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="95b1d-144">Nyissa meg a hello Excel-munkafüzetben, jegyezze fel a hello **WEBES szolgáltatás URL-címe** és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-144">Open hello Excel workbook, make a note of hello **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="95b1d-145">Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="95b1d-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="95b1d-146">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="95b1d-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="95b1d-147">A hello [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-147">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="95b1d-148">Adja meg a következő információ hello feladat hello.</span><span class="sxs-lookup"><span data-stu-id="95b1d-148">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="95b1d-149">**Feladat neve**: hello feladat hello nevét.</span><span class="sxs-lookup"><span data-stu-id="95b1d-149">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="95b1d-150">hello nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="95b1d-150">hello name must be globally unique.</span></span>

   <span data-ttu-id="95b1d-151">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="95b1d-151">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="95b1d-152">**Hely**: használata hello ugyanaz az erőforráscsoport és a helyen.</span><span class="sxs-lookup"><span data-stu-id="95b1d-152">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="95b1d-153">**PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="95b1d-153">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="95b1d-155">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="95b1d-155">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="95b1d-156">Egy bemeneti toohello Stream Analytics-feladat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="95b1d-156">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="95b1d-157">Nyissa meg hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="95b1d-157">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="95b1d-158">A **feladat topológia**, kattintson a **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="95b1d-159">A hello **bemenetek** ablaktáblában kattintson **Hozzáadás**, majd adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="95b1d-159">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="95b1d-160">**A bemeneti alias**: hello egyedi alias hello bemeneti.</span><span class="sxs-lookup"><span data-stu-id="95b1d-160">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="95b1d-161">**Forrás**: válasszon **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="95b1d-162">**Felhasználói csoport**: Select hello fogyasztói csoportot hozott létre.</span><span class="sxs-lookup"><span data-stu-id="95b1d-162">**Consumer group**: Select hello consumer group you created.</span></span>

   ![Egy bemeneti toohello Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="95b1d-164">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="95b1d-164">Click **Create**.</span></span>

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="95b1d-165">Adja hozzá egy kimeneti toohello Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="95b1d-165">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="95b1d-166">A **feladat topológia**, kattintson a **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="95b1d-167">A hello **kimenetek** ablaktáblán kattintson a **Hozzáadás**, majd adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="95b1d-167">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="95b1d-168">**A kimeneti alias**: hello egyedi alias hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="95b1d-168">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="95b1d-169">**Gyűjtése**: válasszon **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="95b1d-170">**A tárfiók**: hello tárfiók a blob Storage.</span><span class="sxs-lookup"><span data-stu-id="95b1d-170">**Storage account**: hello storage account for your blob storage.</span></span> <span data-ttu-id="95b1d-171">Hozzon létre egy tárfiókot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="95b1d-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="95b1d-172">**Tároló**: hello tároló hello blob menteni.</span><span class="sxs-lookup"><span data-stu-id="95b1d-172">**Container**: hello container where hello blob is saved.</span></span> <span data-ttu-id="95b1d-173">Hozzon létre egy tárolót, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="95b1d-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="95b1d-174">**Esemény szerializálási formátum**: válasszon **CSV**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Egy kimeneti toohello Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="95b1d-176">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="95b1d-176">Click **Create**.</span></span>

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a><span data-ttu-id="95b1d-177">Egy függvény toohello Stream Analytics feladat toocall hello webszolgáltatás üzembe helyezett hozzáadása</span><span class="sxs-lookup"><span data-stu-id="95b1d-177">Add a function toohello Stream Analytics job toocall hello web service you deployed</span></span>

1. <span data-ttu-id="95b1d-178">A **feladat topológia**, kattintson a **funkciók** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="95b1d-179">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="95b1d-179">Enter hello following information:</span></span>

   <span data-ttu-id="95b1d-180">**Alias működéséhez**: Adjon meg `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="95b1d-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="95b1d-181">**Típus működéséhez**: válasszon **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="95b1d-182">**Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="95b1d-183">**URL-cím**: Adja meg a WEBSZOLGÁLTATÁS URL-címe le feljegyzett hello hello Excel-munkafüzetből.</span><span class="sxs-lookup"><span data-stu-id="95b1d-183">**URL**: Enter hello WEB SERVICE URL that you noted down from hello Excel workbook.</span></span>

   <span data-ttu-id="95b1d-184">**Kulcs**: írja be a hozzáférési kulcs le feljegyzett hello hello Excel-munkafüzetből.</span><span class="sxs-lookup"><span data-stu-id="95b1d-184">**Key**: Enter hello ACCESS KEY that you noted down from hello Excel workbook.</span></span>

   ![Egy függvény toohello Stream Analytics-feladat hozzáadása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="95b1d-186">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="95b1d-186">Click **Create**.</span></span>

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="95b1d-187">Hello Stream Analytics-feladat lekérdezése hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95b1d-187">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="95b1d-188">A **feladat topológia**, kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="95b1d-189">Cserélje le a következő kód hello hello meglévő kódot:</span><span class="sxs-lookup"><span data-stu-id="95b1d-189">Replace hello existing code with hello following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="95b1d-190">Cserélje le `[YourInputAlias]` bemeneti hello aliasnév hello feladat.</span><span class="sxs-lookup"><span data-stu-id="95b1d-190">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>

   <span data-ttu-id="95b1d-191">Cserélje le `[YourOutputAlias]` hello kimeneti aliasnév hello feladat.</span><span class="sxs-lookup"><span data-stu-id="95b1d-191">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>

1. <span data-ttu-id="95b1d-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="95b1d-192">Click **Save**.</span></span>

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="95b1d-193">Hello Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="95b1d-193">Run hello Stream Analytics job</span></span>

<span data-ttu-id="95b1d-194">Hello Stream Analytics-feladat, kattintson **Start** > **most** > **Start**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-194">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="95b1d-195">Miután hello feladat sikeresen elindul, hello feladat állapota a **leállítva** túl**futtató**.</span><span class="sxs-lookup"><span data-stu-id="95b1d-195">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Hello Stream Analytics-feladat futtatása](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a><span data-ttu-id="95b1d-197">Használja a Microsoft Azure Tártallózó tooview hello időjárás:</span><span class="sxs-lookup"><span data-stu-id="95b1d-197">Use Microsoft Azure Storage Explorer tooview hello weather forecast</span></span>

<span data-ttu-id="95b1d-198">Hello ügyfél alkalmazás toostart összegyűjtése és elküldését a hőmérséklet és a páratartalom adatok tooyour IoT-központ futtatása.</span><span class="sxs-lookup"><span data-stu-id="95b1d-198">Run hello client application toostart collecting and sending temperature and humidity data tooyour IoT hub.</span></span> <span data-ttu-id="95b1d-199">Minden üzenet, amely megkapja az IoT hub hello Stream Analytics-feladat hello időjárás web service tooproduce hello esélyét eső hívja.</span><span class="sxs-lookup"><span data-stu-id="95b1d-199">For each message that your IoT hub receives, hello Stream Analytics job calls hello weather forecast web service tooproduce hello chance of rain.</span></span> <span data-ttu-id="95b1d-200">hello eredmény majd mentett tooyour Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="95b1d-200">hello result is then saved tooyour Azure blob storage.</span></span> <span data-ttu-id="95b1d-201">Az Azure Tártallózó egy olyan eszköz, amelyeket felhasználhat tooview hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="95b1d-201">Azure Storage Explorer is a tool that you can use tooview hello result.</span></span>

1. <span data-ttu-id="95b1d-202">[Töltse le és telepítse a Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="95b1d-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="95b1d-203">Nyissa meg az Azure Storage Explorert.</span><span class="sxs-lookup"><span data-stu-id="95b1d-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="95b1d-204">Bejelentkezés tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="95b1d-204">Sign in tooyour Azure account.</span></span>
1. <span data-ttu-id="95b1d-205">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="95b1d-205">Select your subscription.</span></span>
1. <span data-ttu-id="95b1d-206">Kattintson az előfizetés > **Tárfiókok** > a tárfiók > **Blobtárolók** > a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="95b1d-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="95b1d-207">Nyissa meg a .csv fájl toosee hello eredményt.</span><span class="sxs-lookup"><span data-stu-id="95b1d-207">Open a .csv file toosee hello result.</span></span> <span data-ttu-id="95b1d-208">hello utolsó oszlop rekordok hello eső esélyét.</span><span class="sxs-lookup"><span data-stu-id="95b1d-208">hello last column records hello chance of rain.</span></span>

   ![Időjárás eredmény Azure Machine Learning segítségével](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="95b1d-210">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="95b1d-210">Summary</span></span>

<span data-ttu-id="95b1d-211">Sikeresen használta az Azure Machine Learning tooproduce hello hello hőmérséklet és a páratartalom adatot, amely megkapja az IoT hub eső esélyét.</span><span class="sxs-lookup"><span data-stu-id="95b1d-211">You’ve successfully used Azure Machine Learning tooproduce hello chance of rain based on hello temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]