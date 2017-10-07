---
title: "az érzékelő adatokat az Azure IoT hub – webalkalmazások aaaReal idejű adatok vizuális |} Microsoft Docs"
description: "Használja a Microsoft Azure App Service toovisualize hőmérséklet és a páratartalom adatok hello érzékelő gyűjtése történik, és az Iot-központ tooyour küldött hello Web Apps szolgáltatást."
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
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="5f8ea-104">Az Azure IoT hub a valós idejű érzékelőadatok megjelenítése hello Azure App Service Web Apps szolgáltatása használatával</span><span class="sxs-lookup"><span data-stu-id="5f8ea-104">Visualize real-time sensor data from your Azure IoT hub by using hello Web Apps feature of Azure App Service</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="5f8ea-106">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="5f8ea-106">What you learn</span></span>

<span data-ttu-id="5f8ea-107">Ebben az oktatóanyagban elsajátíthatja, hogyan, amely az IoT hub fogad, amely a webes alkalmazás futtatásával toovisualize valós idejű érzékelőadatok üzemelteti a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-107">In this tutorial, you learn how toovisualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="5f8ea-108">Ha szeretné tootry toovisualize hello adatokat az IoT hub a Power BI használatával, lásd: [a Power BI toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="5f8ea-108">If you want tootry toovisualize hello data in your IoT hub by using Power BI, see [Use Power BI toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="5f8ea-109">Mit</span><span class="sxs-lookup"><span data-stu-id="5f8ea-109">What you do</span></span>

- <span data-ttu-id="5f8ea-110">A webalkalmazás létrehozása az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-110">Create a web app in hello Azure portal.</span></span>
- <span data-ttu-id="5f8ea-111">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="5f8ea-112">Hello web app tooread érzékelő adatait az IoT hub konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-112">Configure hello web app tooread sensor data from your IoT hub.</span></span>
- <span data-ttu-id="5f8ea-113">Töltse fel a webes alkalmazás toobe hello webalkalmazás üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-113">Upload a web application toobe hosted by hello web app.</span></span>
- <span data-ttu-id="5f8ea-114">Nyissa meg az IoT hub hello web app toosee valós idejű hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-114">Open hello web app toosee real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5f8ea-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="5f8ea-115">What you need</span></span>

- <span data-ttu-id="5f8ea-116">[Konfigurálja az eszközt](iot-hub-raspberry-pi-kit-node-get-started.md), amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="5f8ea-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers hello following requirements:</span></span>
  - <span data-ttu-id="5f8ea-117">Aktív Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="5f8ea-117">An active Azure subscription</span></span>
  - <span data-ttu-id="5f8ea-118">Az Iot-központ az előfizetéshez tartozó</span><span class="sxs-lookup"><span data-stu-id="5f8ea-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="5f8ea-119">Egy ügyfélalkalmazás által küldött üzenetek tooyour Iot-központ</span><span class="sxs-lookup"><span data-stu-id="5f8ea-119">A client application that sends messages tooyour Iot hub</span></span>
- [<span data-ttu-id="5f8ea-120">Töltse le a Git</span><span class="sxs-lookup"><span data-stu-id="5f8ea-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="5f8ea-121">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f8ea-121">Create a web app</span></span>

1. <span data-ttu-id="5f8ea-122">A hello [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-122">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="5f8ea-123">Adja meg a feladat egyedi nevét, ellenőrizze a hello előfizetés, adjon meg egy erőforráscsoportot és helyet, jelölje be **PIN-kód toodashboard**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-123">Enter a unique job name, verify hello subscription, specify a resource group and a location, select **Pin toodashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="5f8ea-124">Azt javasoljuk, hogy kiválassza hello helyen, ahol az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-124">We recommend that you select hello same location as that of your resource group.</span></span> <span data-ttu-id="5f8ea-125">Ezzel segítséget nyújt a feldolgozási sebesség, és csökkenti a hello adatátvitel költségeit.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-125">Doing so assists with processing speed and reduces hello cost of data transfer.</span></span>

   ![Webalkalmazás létrehozása](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a><span data-ttu-id="5f8ea-127">Hello web app tooread adatokat az IoT hub konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5f8ea-127">Configure hello web app tooread data from your IoT hub</span></span>

1. <span data-ttu-id="5f8ea-128">Nyissa meg az imént létesített hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-128">Open hello web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="5f8ea-129">Kattintson a **Alkalmazásbeállítások**, majd az **Alkalmazásbeállítások**, adja hozzá a következő kulcs/érték párok hello:</span><span class="sxs-lookup"><span data-stu-id="5f8ea-129">Click **Application settings**, and then, under **App settings**, add hello following key/value pairs:</span></span>

   | <span data-ttu-id="5f8ea-130">Kulcs</span><span class="sxs-lookup"><span data-stu-id="5f8ea-130">Key</span></span>                                   | <span data-ttu-id="5f8ea-131">Érték</span><span class="sxs-lookup"><span data-stu-id="5f8ea-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="5f8ea-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="5f8ea-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="5f8ea-133">Az IOT hubbal-explorer kapott</span><span class="sxs-lookup"><span data-stu-id="5f8ea-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="5f8ea-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="5f8ea-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="5f8ea-135">hello felhasználói csoport hozzáadása tooyour IoT-központ hello neve</span><span class="sxs-lookup"><span data-stu-id="5f8ea-135">hello name of hello consumer group that you add tooyour IoT hub</span></span>  |

   ![A kulcs/érték párok beállítások tooyour webalkalmazás hozzáadása](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="5f8ea-137">Kattintson a **Alkalmazásbeállítások**a **általános beállítások**, váltása hello **webes szoftvercsatornák** lehetőséget, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-137">Click **Application settings**, under **General settings**, toggle hello **Web sockets** option, and then click **Save**.</span></span>

   ![Webes szoftvercsatornák beállítást váltása hello](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a><span data-ttu-id="5f8ea-139">A webes alkalmazás toobe hello webalkalmazás által üzemeltetett feltöltése</span><span class="sxs-lookup"><span data-stu-id="5f8ea-139">Upload a web application toobe hosted by hello web app</span></span>

<span data-ttu-id="5f8ea-140">A Githubon hajtottunk érhető el egy webes alkalmazás, amely az IoT hub valós idejű érzékelő adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="5f8ea-141">Toodo szüksége hello web app toowork állítson be egy Git-tárházat, a Githubról hello webes alkalmazás letöltése és a hello web app toohost tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-141">All you need toodo is configure hello web app toowork with a Git repository, download hello web application from GitHub, and then upload it tooAzure for hello web app toohost.</span></span>

1. <span data-ttu-id="5f8ea-142">Hello web app alkalmazásban kattintson **központi telepítési beállítások** > **forrás választása** > **helyi Git-tárház**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-142">In hello web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![A webes alkalmazás központi telepítési toouse hello helyi Git-tárház beállítása](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="5f8ea-144">Kattintson a **üzembe helyezési hitelesítő adatok**, hozzon létre egy felhasználói nevet és jelszót toouse tooconnect toohello Git-tárházat az Azure-ban, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-144">Click **Deployment Credentials**, create a user name and password toouse tooconnect toohello Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="5f8ea-145">Kattintson a **áttekintése**, és jegyezze fel a hello értékének **Git-klón URL-címét**.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-145">Click **Overview**, and note hello value of **Git clone url**.</span></span>

   ![Hello Git Klónozás webalkalmazás URL-CÍMÉT az beszerzése](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="5f8ea-147">Nyissa meg a parancsot vagy terminálablakot a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="5f8ea-148">Töltse le a hello webalkalmazás a Githubból, és töltse fel a hello web app toohost tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-148">Download hello web app from GitHub, and upload it tooAzure for hello web app toohost.</span></span> <span data-ttu-id="5f8ea-149">toodo Igen, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="5f8ea-149">toodo so, run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="5f8ea-150">\<Git-klón URL-cím\> hello található hello Git-tárház URL-cím-hello **áttekintése** hello webalkalmazás oldalán.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-150">\<Git clone URL\> is hello URL of hello Git repository found on hello **Overview** page of hello web app.</span></span>

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="5f8ea-151">Az IoT hub hello webes alkalmazás toosee valós idejű hőmérséklet és a páratartalom adatainak megnyitása</span><span class="sxs-lookup"><span data-stu-id="5f8ea-151">Open hello web app toosee real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="5f8ea-152">A hello **áttekintése** lap webalkalmazás, kattintson a hello URL-cím tooopen hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-152">On hello **Overview** page of your web app, click hello URL tooopen hello web app.</span></span>

![A webalkalmazás hello URL-cím beszerzése](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="5f8ea-154">Meg kell jelennie az IoT hub a hello valós idejű hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-154">You should see hello real-time temperature and humidity data from your IoT hub.</span></span>

![Valós idejű hőmérséklet és a páratartalom bemutató alkalmazás weblap](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="5f8ea-156">Ellenőrizze, hogy hello mintaalkalmazás fut-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-156">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="5f8ea-157">Ha nem, üres diagram kap, olvassa el a toohello oktatóanyagok alatt [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5f8ea-157">If not, you will get a blank chart, you can refer toohello tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f8ea-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f8ea-158">Next steps</span></span>
<span data-ttu-id="5f8ea-159">Használta a webes alkalmazás toovisualize valós idejű érzékelőadatok az IoT hub sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="5f8ea-159">You've successfully used your web app toovisualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="5f8ea-160">Egy Azure IoT Hub megadásának alternatív módja toovisualize adatokat, lásd: [a Power BI toovisualize valós idejű érzékelőadatok az IoT hub a](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="5f8ea-160">For an alternative way toovisualize data from Azure IoT Hub, see [Use Power BI toovisualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
