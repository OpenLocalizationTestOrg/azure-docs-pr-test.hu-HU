---
title: "Az érzékelők adatstreamének az Azure IoT hub – Web Apps a valós idejű adatok vizuális |} Microsoft Docs"
description: "A Microsoft Azure App Service Web Apps szolgáltatása segítségével, amely az érzékelő gyűjtése történik, és az Iot hub küldött hőmérséklet és a páratartalom adatainak megjelenítése."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "valós idejű adatok vizuális, az élő adatok vizuális érzékelő adatábrázolási"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: e037f5c29cabf8e5d0d3e7ded187280a0652d5c3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="19644-104">Az Azure IoT hub a valós idejű érzékelőadatok megjelenítése az Azure App Service Web Apps szolgáltatásának használatával</span><span class="sxs-lookup"><span data-stu-id="19644-104">Visualize real-time sensor data from your Azure IoT hub by using the Web Apps feature of Azure App Service</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="19644-106">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="19644-106">What you learn</span></span>

<span data-ttu-id="19644-107">Ebben az oktatóanyagban elsajátíthatja, hogyan jelenítheti meg az IoT-központ által futtatott webalkalmazás fut a webes alkalmazás kap valós idejű érzékelőadatok.</span><span class="sxs-lookup"><span data-stu-id="19644-107">In this tutorial, you learn how to visualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="19644-108">Ha azt szeretné, próbálja meg az IoT hub adatainak megjelenítése Power BI használatával kapcsolatos tudnivalókat lásd: [használjon Power BI segítségével ábrázolhatja valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="19644-108">If you want to try to visualize the data in your IoT hub by using Power BI, see [Use Power BI to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="19644-109">Mit</span><span class="sxs-lookup"><span data-stu-id="19644-109">What you do</span></span>

- <span data-ttu-id="19644-110">Webalkalmazás létrehozása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="19644-110">Create a web app in the Azure portal.</span></span>
- <span data-ttu-id="19644-111">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="19644-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="19644-112">A webalkalmazás érzékelő adatokat olvasni az IoT hub konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="19644-112">Configure the web app to read sensor data from your IoT hub.</span></span>
- <span data-ttu-id="19644-113">Töltse fel a webalkalmazások a web app működhetnek.</span><span class="sxs-lookup"><span data-stu-id="19644-113">Upload a web application to be hosted by the web app.</span></span>
- <span data-ttu-id="19644-114">Nyissa meg a webalkalmazás az IoT hub valós idejű hőmérséklet és a páratartalom adatai.</span><span class="sxs-lookup"><span data-stu-id="19644-114">Open the web app to see real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="19644-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="19644-115">What you need</span></span>

- <span data-ttu-id="19644-116">[Konfigurálja az eszközt](iot-hub-raspberry-pi-kit-node-get-started.md), amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="19644-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers the following requirements:</span></span>
  - <span data-ttu-id="19644-117">Aktív Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="19644-117">An active Azure subscription</span></span>
  - <span data-ttu-id="19644-118">Az Iot-központ az előfizetéshez tartozó</span><span class="sxs-lookup"><span data-stu-id="19644-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="19644-119">Egy ügyfélalkalmazást, amely üzeneteket küld az Iot hub</span><span class="sxs-lookup"><span data-stu-id="19644-119">A client application that sends messages to your Iot hub</span></span>
- [<span data-ttu-id="19644-120">Töltse le a Git</span><span class="sxs-lookup"><span data-stu-id="19644-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="19644-121">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="19644-121">Create a web app</span></span>

1. <span data-ttu-id="19644-122">Az a [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19644-122">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="19644-123">Adja meg egy egyedi feladat nevét, ellenőrizze az előfizetés, adjon meg egy erőforráscsoportot és helyet, jelölje be **rögzítés az irányítópulton**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="19644-123">Enter a unique job name, verify the subscription, specify a resource group and a location, select **Pin to dashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="19644-124">Azt javasoljuk, hogy legyen, mint az erőforráscsoport válassza ki az ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="19644-124">We recommend that you select the same location as that of your resource group.</span></span> <span data-ttu-id="19644-125">Ezzel segítséget nyújt a feldolgozási sebesség, és csökkenti az adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="19644-125">Doing so assists with processing speed and reduces the cost of data transfer.</span></span>

   ![Webalkalmazás létrehozása](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a><span data-ttu-id="19644-127">A webalkalmazás adatokat olvasni az IoT hub konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19644-127">Configure the web app to read data from your IoT hub</span></span>

1. <span data-ttu-id="19644-128">Nyissa meg a webes alkalmazás imént létesített.</span><span class="sxs-lookup"><span data-stu-id="19644-128">Open the web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="19644-129">Kattintson a **Alkalmazásbeállítások**, majd az **Alkalmazásbeállítások**, adja hozzá a következő kulcs/érték párok:</span><span class="sxs-lookup"><span data-stu-id="19644-129">Click **Application settings**, and then, under **App settings**, add the following key/value pairs:</span></span>

   | <span data-ttu-id="19644-130">Kulcs</span><span class="sxs-lookup"><span data-stu-id="19644-130">Key</span></span>                                   | <span data-ttu-id="19644-131">Érték</span><span class="sxs-lookup"><span data-stu-id="19644-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="19644-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="19644-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="19644-133">Az IOT hubbal-explorer kapott</span><span class="sxs-lookup"><span data-stu-id="19644-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="19644-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="19644-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="19644-135">A fogyasztói csoportot felvenni kívánt az IoT hub nevét</span><span class="sxs-lookup"><span data-stu-id="19644-135">The name of the consumer group that you add to your IoT hub</span></span>  |

   ![A webalkalmazás a kulcs/érték párok beállítások hozzáadása](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="19644-137">Kattintson a **Alkalmazásbeállítások**a **általános beállítások**, váltása a **webes szoftvercsatornák** lehetőséget, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="19644-137">Click **Application settings**, under **General settings**, toggle the **Web sockets** option, and then click **Save**.</span></span>

   ![A webes szoftvercsatornák beállítást váltása](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a><span data-ttu-id="19644-139">A webalkalmazás üzemeltetnie webalkalmazás feltöltése</span><span class="sxs-lookup"><span data-stu-id="19644-139">Upload a web application to be hosted by the web app</span></span>

<span data-ttu-id="19644-140">A Githubon hajtottunk érhető el egy webes alkalmazás, amely az IoT hub valós idejű érzékelő adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19644-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="19644-141">Ehhez szüksége a webalkalmazás Git-tárház, töltse le a webes alkalmazás a Githubból, és töltse fel azt Azure állomásnak a webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="19644-141">All you need to do is configure the web app to work with a Git repository, download the web application from GitHub, and then upload it to Azure for the web app to host.</span></span>

1. <span data-ttu-id="19644-142">Kattintson a webalkalmazás **központi telepítési beállítások** > **forrás választása** > **helyi Git-tárház**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="19644-142">In the web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![A webes alkalmazás telepítéséhez a helyi Git-tárházon konfigurálása](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="19644-144">Kattintson a **üzembe helyezési hitelesítő adatok**, hozzon létre egy felhasználónevet és jelszót ehhez a Git-tárházat az Azure-ban, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="19644-144">Click **Deployment Credentials**, create a user name and password to use to connect to the Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="19644-145">Kattintson a **áttekintése**, és jegyezze fel a értékének **Git-klón URL-címét**.</span><span class="sxs-lookup"><span data-stu-id="19644-145">Click **Overview**, and note the value of **Git clone url**.</span></span>

   ![A webalkalmazás Git klón URL-cím beszerzése](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="19644-147">Nyissa meg a parancsot vagy terminálablakot a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19644-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="19644-148">Töltse le a webalkalmazást a Githubból, és töltse fel az Azure a webalkalmazás a gazdagépre.</span><span class="sxs-lookup"><span data-stu-id="19644-148">Download the web app from GitHub, and upload it to Azure for the web app to host.</span></span> <span data-ttu-id="19644-149">Ehhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="19644-149">To do so, run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="19644-150">\<Git-klón URL-cím\> található a Git-tárház URL-címe a **áttekintése** a webes alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="19644-150">\<Git clone URL\> is the URL of the Git repository found on the **Overview** page of the web app.</span></span>

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="19644-151">Nyissa meg a webes alkalmazást az IoT hub valós idejű hőmérséklet és a páratartalom adatai</span><span class="sxs-lookup"><span data-stu-id="19644-151">Open the web app to see real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="19644-152">Az a **áttekintése** lap webalkalmazás, kattintson a nyissa meg a webes alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="19644-152">On the **Overview** page of your web app, click the URL to open the web app.</span></span>

![A webalkalmazás URL-cím beszerzése](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="19644-154">Az IoT hub származó, meg kell jelennie a hőmérséklet és a páratartalom valós idejű adatok.</span><span class="sxs-lookup"><span data-stu-id="19644-154">You should see the real-time temperature and humidity data from your IoT hub.</span></span>

![Valós idejű hőmérséklet és a páratartalom bemutató alkalmazás weblap](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="19644-156">Ellenőrizze, hogy a mintaalkalmazás fut-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="19644-156">Ensure the sample application is running on your device.</span></span> <span data-ttu-id="19644-157">Ha nem, elérhetővé válik egy üres diagram, olvassa el az oktatóanyagok alatt [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="19644-157">If not, you will get a blank chart, you can refer to the tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="19644-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19644-158">Next steps</span></span>
<span data-ttu-id="19644-159">A webalkalmazás sikeresen használtuk az IoT hub a valós idejű érzékelőadatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="19644-159">You've successfully used your web app to visualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="19644-160">Egy Azure IoT Hub-adatok ábrázolása alternatív módja, lásd: [használjon Power BI segítségével ábrázolhatja az IoT hub a valós idejű érzékelőadatok](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="19644-160">For an alternative way to visualize data from Azure IoT Hub, see [Use Power BI to visualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
