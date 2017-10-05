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
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="5a308-104">Az érzékelő adatokat az IoT hub használata az Azure Machine Learning előrejelzési időjárási</span><span class="sxs-lookup"><span data-stu-id="5a308-104">Weather forecast using the sensor data from your IoT hub in Azure Machine Learning</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="5a308-106">Gépi tanulás a technika, amely segít a számítógépek ismerje meg, az előrejelzési algoritmus jövőbeli történéseket, eredményeket vagy trendeket meglévő adatokból tudományos adatok.</span><span class="sxs-lookup"><span data-stu-id="5a308-106">Machine learning is a technique of data science that helps computers learn from existing data to forecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="5a308-107">Az Azure Machine Learning egy felhőalapú prediktív elemzési szolgáltatás, amely lehetővé teszi elemzési megoldásként használható prediktív modellek gyors létrehozását és üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="5a308-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible to quickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="5a308-108">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="5a308-108">What you learn</span></span>

<span data-ttu-id="5a308-109">Időjárás előrejelzés (eső esélyét) az Azure Machine Learning segítségével megtanulhatja a hőmérséklet és a páratartalom adatokat az Azure IoT hub használatával.</span><span class="sxs-lookup"><span data-stu-id="5a308-109">You learn how to use Azure Machine Learning to do weather forecast (chance of rain) using the temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="5a308-110">Az esélye, eső egy előkészített időjárási előrejelzési modell kimeneti.</span><span class="sxs-lookup"><span data-stu-id="5a308-110">The chance of rain is the output of a prepared weather prediction model.</span></span> <span data-ttu-id="5a308-111">A modell az előrejelzési algoritmus alapján a hőmérséklet és a páratartalom eső esélyét régebbi adatok épül.</span><span class="sxs-lookup"><span data-stu-id="5a308-111">The model is built upon historic data to forecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="5a308-112">Mit</span><span class="sxs-lookup"><span data-stu-id="5a308-112">What you do</span></span>

- <span data-ttu-id="5a308-113">Időjárás előrejelzési modell rendszerbe állítása webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="5a308-113">Deploy the weather prediction model as a web service.</span></span>
- <span data-ttu-id="5a308-114">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="5a308-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="5a308-115">A Stream Analytics-feladat létrehozása, és a feladat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="5a308-115">Create a Stream Analytics job and configure the job to:</span></span>
  - <span data-ttu-id="5a308-116">Hőmérséklet és a páratartalom adatokat olvasni az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5a308-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="5a308-117">Eső lehetősége a webszolgáltatás hívására.</span><span class="sxs-lookup"><span data-stu-id="5a308-117">Call the web service to get the rain chance.</span></span>
  - <span data-ttu-id="5a308-118">Az eredmény mentése az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5a308-118">Save the result to an Azure blob storage.</span></span>
- <span data-ttu-id="5a308-119">A Microsoft Azure Tártallózó segítségével megtekintheti a időjárás.</span><span class="sxs-lookup"><span data-stu-id="5a308-119">Use Microsoft Azure Storage Explorer to view the weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5a308-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="5a308-120">What you need</span></span>

- <span data-ttu-id="5a308-121">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="5a308-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="5a308-122">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5a308-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="5a308-123">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5a308-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="5a308-124">Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5a308-124">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="5a308-125">Egy Azure Machine Learning Studio-fiók.</span><span class="sxs-lookup"><span data-stu-id="5a308-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="5a308-126">([Machine Learning Studio ingyenes próbálja](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="5a308-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="5a308-127">Időjárás előrejelzési modell rendszerbe állítása egy webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5a308-127">Deploy the weather prediction model as a web service</span></span>

1. <span data-ttu-id="5a308-128">Lépjen a [időjárási előrejelzési modell lap](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="5a308-128">Go to the [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="5a308-129">Kattintson a **Megnyitás a Studióban** a Microsoft Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="5a308-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="5a308-130">![Nyissa meg az időjárási előrejelzési modell oldal a Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="5a308-130">![Open the weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="5a308-131">Kattintson a **futtatása** a lépéseket a modell ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5a308-131">Click **Run** to validate the steps in the model.</span></span> <span data-ttu-id="5a308-132">Ez a lépés befejezéséhez 2 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="5a308-132">This step might take 2 minutes to complete.</span></span>
   <span data-ttu-id="5a308-133">![Nyissa meg a időjárási előrejelzési modellt az Azure Machine Learning Studióban](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="5a308-133">![Open the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="5a308-134">Kattintson a **WEBSZOLGÁLTATÁS beállítása** > **prediktív webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="5a308-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="5a308-135">![Az Azure Machine Learning Studióban időjárási előrejelzési modell rendszerbe állítása](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="5a308-135">![Deploy the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="5a308-136">Az ábrán, húzza a **webszolgáltatás bemenetét** modul valahol közelében a **Score Model** modul.</span><span class="sxs-lookup"><span data-stu-id="5a308-136">In the diagram, drag the **Web service input** module somewhere near the **Score Model** module.</span></span>
1. <span data-ttu-id="5a308-137">Csatlakozás a **webszolgáltatás bemenetét** modult a **Score Model** modul.</span><span class="sxs-lookup"><span data-stu-id="5a308-137">Connect the **Web service input** module to the **Score Model** module.</span></span>
   <span data-ttu-id="5a308-138">![Csatlakozás az Azure Machine Learning Studióban két modulok](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="5a308-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="5a308-139">Kattintson a **futtatása** a lépéseket a modell ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5a308-139">Click **RUN** to validate the steps in the model.</span></span>
1. <span data-ttu-id="5a308-140">Kattintson a **WEBES szolgáltatás telepítése** a modell rendszerbe webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="5a308-140">Click **DEPLOY WEB SERVICE** to deploy the model as a web service.</span></span>
1. <span data-ttu-id="5a308-141">A modell az irányítópulton, töltse le a **Excel 2010 vagy korábbi munkafüzet** a **kérelem/válasz**.</span><span class="sxs-lookup"><span data-stu-id="5a308-141">On the dashboard of the model, download the **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="5a308-142">Győződjön meg arról, hogy töltse le a **Excel 2010 vagy korábbi munkafüzet** még akkor is, ha az Excel újabb verziójában futtat a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5a308-142">Ensure that you download the **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Töltse le az Excel, a kérelem-válasz végpont](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="5a308-144">Nyissa meg az Excel-munkafüzetben, jegyezze fel a **WEBES szolgáltatás URL-címe** és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="5a308-144">Open the Excel workbook, make a note of the **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="5a308-145">Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="5a308-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="5a308-146">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a308-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="5a308-147">Az a [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="5a308-147">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="5a308-148">Adja meg a feladat a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="5a308-148">Enter the following information for the job.</span></span>

   <span data-ttu-id="5a308-149">**Feladat neve**: a feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="5a308-149">**Job name**: The name of the job.</span></span> <span data-ttu-id="5a308-150">A névnek globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5a308-150">The name must be globally unique.</span></span>

   <span data-ttu-id="5a308-151">**Erőforráscsoport**: használja ugyanazt az erőforráscsoportot, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="5a308-151">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="5a308-152">**Hely**: ugyanazt a helyet használja a erőforráscsoportként működnek.</span><span class="sxs-lookup"><span data-stu-id="5a308-152">**Location**: Use the same location as your resource group.</span></span>

   <span data-ttu-id="5a308-153">**Rögzítés az irányítópulton**: ezt a beállítást, az egyszerű elérés érdekében ellenőrizze, hogy az IoT hub az irányítópultról.</span><span class="sxs-lookup"><span data-stu-id="5a308-153">**Pin to dashboard**: Check this option for easy access to your IoT hub from the dashboard.</span></span>

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="5a308-155">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a308-155">Click **Create**.</span></span>

### <a name="add-an-input-to-the-stream-analytics-job"></a><span data-ttu-id="5a308-156">A Stream Analytics-feladat bemenete hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a308-156">Add an input to the Stream Analytics job</span></span>

1. <span data-ttu-id="5a308-157">Nyissa meg a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="5a308-157">Open the Stream Analytics job.</span></span>
1. <span data-ttu-id="5a308-158">A **feladat topológia**, kattintson a **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="5a308-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="5a308-159">Az a **bemenetek** ablaktáblában kattintson **Hozzáadás**, és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="5a308-159">In the **Inputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="5a308-160">**A bemeneti alias**: a bemeneti egyedi alias.</span><span class="sxs-lookup"><span data-stu-id="5a308-160">**Input alias**: The unique alias for the input.</span></span>

   <span data-ttu-id="5a308-161">**Forrás**: válasszon **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="5a308-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="5a308-162">**Felhasználói csoport**: válassza ki a létrehozott fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="5a308-162">**Consumer group**: Select the consumer group you created.</span></span>

   ![A Stream Analytics-feladat bemenete hozzáadása az Azure-ban](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="5a308-164">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a308-164">Click **Create**.</span></span>

### <a name="add-an-output-to-the-stream-analytics-job"></a><span data-ttu-id="5a308-165">Adja hozzá egy kimeneti a Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="5a308-165">Add an output to the Stream Analytics job</span></span>

1. <span data-ttu-id="5a308-166">A **feladat topológia**, kattintson a **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="5a308-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="5a308-167">Az a **kimenetek** ablaktáblán kattintson a **Hozzáadás**, és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="5a308-167">In the **Outputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="5a308-168">**A kimeneti alias**: az egyedi alias a kimeneti oldal számára.</span><span class="sxs-lookup"><span data-stu-id="5a308-168">**Output alias**: The unique alias for the output.</span></span>

   <span data-ttu-id="5a308-169">**Gyűjtése**: válasszon **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="5a308-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="5a308-170">**A tárfiók**: A tárfiók a blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="5a308-170">**Storage account**: The storage account for your blob storage.</span></span> <span data-ttu-id="5a308-171">Hozzon létre egy tárfiókot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="5a308-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="5a308-172">**Tároló**: A tároló, a blob menteni.</span><span class="sxs-lookup"><span data-stu-id="5a308-172">**Container**: The container where the blob is saved.</span></span> <span data-ttu-id="5a308-173">Hozzon létre egy tárolót, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="5a308-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="5a308-174">**Esemény szerializálási formátum**: válasszon **CSV**.</span><span class="sxs-lookup"><span data-stu-id="5a308-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Egy kimeneti hozzáadása az Azure Stream Analytics-feladat](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="5a308-176">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a308-176">Click **Create**.</span></span>

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a><span data-ttu-id="5a308-177">A Stream Analytics-feladat üzembe helyezett webszolgáltatás hívására függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a308-177">Add a function to the Stream Analytics job to call the web service you deployed</span></span>

1. <span data-ttu-id="5a308-178">A **feladat topológia**, kattintson a **funkciók** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5a308-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="5a308-179">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="5a308-179">Enter the following information:</span></span>

   <span data-ttu-id="5a308-180">**Alias működéséhez**: Adjon meg `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="5a308-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="5a308-181">**Típus működéséhez**: válasszon **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="5a308-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="5a308-182">**Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.</span><span class="sxs-lookup"><span data-stu-id="5a308-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="5a308-183">**URL-cím**: Adja meg a WEBSZOLGÁLTATÁS URL-címe le feljegyzett az Excel-munkafüzetből.</span><span class="sxs-lookup"><span data-stu-id="5a308-183">**URL**: Enter the WEB SERVICE URL that you noted down from the Excel workbook.</span></span>

   <span data-ttu-id="5a308-184">**Kulcs**: írja be a hozzáférési kulcs le feljegyzett az Excel-munkafüzetből.</span><span class="sxs-lookup"><span data-stu-id="5a308-184">**Key**: Enter the ACCESS KEY that you noted down from the Excel workbook.</span></span>

   ![Egy függvény hozzáadása az Azure Stream Analytics-feladat](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="5a308-186">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a308-186">Click **Create**.</span></span>

### <a name="configure-the-query-of-the-stream-analytics-job"></a><span data-ttu-id="5a308-187">A lekérdezést a Stream Analytics-feladat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5a308-187">Configure the query of the Stream Analytics job</span></span>

1. <span data-ttu-id="5a308-188">A **feladat topológia**, kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="5a308-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="5a308-189">Cserélje le a meglévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="5a308-189">Replace the existing code with the following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="5a308-190">Cserélje le `[YourInputAlias]` a bemeneti áljel a feladat.</span><span class="sxs-lookup"><span data-stu-id="5a308-190">Replace `[YourInputAlias]` with the input alias of the job.</span></span>

   <span data-ttu-id="5a308-191">Cserélje le `[YourOutputAlias]` a kimeneti aliasnév a feladat.</span><span class="sxs-lookup"><span data-stu-id="5a308-191">Replace `[YourOutputAlias]` with the output alias of the job.</span></span>

1. <span data-ttu-id="5a308-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a308-192">Click **Save**.</span></span>

### <a name="run-the-stream-analytics-job"></a><span data-ttu-id="5a308-193">A Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="5a308-193">Run the Stream Analytics job</span></span>

<span data-ttu-id="5a308-194">Kattintson a Stream Analytics-feladat **Start** > **most** > **Start**.</span><span class="sxs-lookup"><span data-stu-id="5a308-194">In the Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="5a308-195">Ha a feladat sikeresen elindul, a feladat állapota a **leállítva** való **futtató**.</span><span class="sxs-lookup"><span data-stu-id="5a308-195">Once the job successfully starts, the job status changes from **Stopped** to **Running**.</span></span>

![A Stream Analytics-feladat futtatása](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a><span data-ttu-id="5a308-197">A Microsoft Azure Tártallózó segítségével megtekintheti a időjárás:</span><span class="sxs-lookup"><span data-stu-id="5a308-197">Use Microsoft Azure Storage Explorer to view the weather forecast</span></span>

<span data-ttu-id="5a308-198">Futtassa az ügyfélalkalmazás összegyűjtése és az IoT hub hőmérséklet és a páratartalom adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="5a308-198">Run the client application to start collecting and sending temperature and humidity data to your IoT hub.</span></span> <span data-ttu-id="5a308-199">Minden üzenet, amely az IoT hub fogad a Stream Analytics-feladat meghívja a időjárás webszolgáltatás eső esélyét létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5a308-199">For each message that your IoT hub receives, the Stream Analytics job calls the weather forecast web service to produce the chance of rain.</span></span> <span data-ttu-id="5a308-200">Az eredmény ezután menti az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5a308-200">The result is then saved to your Azure blob storage.</span></span> <span data-ttu-id="5a308-201">Az Azure Tártallózó olyan eszköz, amely segítségével a eredmény.</span><span class="sxs-lookup"><span data-stu-id="5a308-201">Azure Storage Explorer is a tool that you can use to view the result.</span></span>

1. <span data-ttu-id="5a308-202">[Töltse le és telepítse a Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="5a308-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="5a308-203">Nyissa meg az Azure Storage Explorert.</span><span class="sxs-lookup"><span data-stu-id="5a308-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="5a308-204">Jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="5a308-204">Sign in to your Azure account.</span></span>
1. <span data-ttu-id="5a308-205">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="5a308-205">Select your subscription.</span></span>
1. <span data-ttu-id="5a308-206">Kattintson az előfizetés > **Tárfiókok** > a tárfiók > **Blobtárolók** > a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5a308-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="5a308-207">Nyisson meg egy CSV-fájlt az eredményt.</span><span class="sxs-lookup"><span data-stu-id="5a308-207">Open a .csv file to see the result.</span></span> <span data-ttu-id="5a308-208">Az utolsó oszlopban az esélye, eső rögzíti.</span><span class="sxs-lookup"><span data-stu-id="5a308-208">The last column records the chance of rain.</span></span>

   ![Időjárás eredmény Azure Machine Learning segítségével](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="5a308-210">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="5a308-210">Summary</span></span>

<span data-ttu-id="5a308-211">Már használta sikeresen Azure Machine Learning az esélye, eső, amely az IoT hub megkapja a hőmérséklet és a páratartalom adatok alapján történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5a308-211">You’ve successfully used Azure Machine Learning to produce the chance of rain based on the temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]