---
title: "aaaSave az IoT hub-üzenetek tooAzure adattárolás |} Microsoft Docs"
description: "Egy Azure függvény app toosave az IoT hub üzenetek tooyour Azure table storage használata. IoT hub köszönőüzenetei tartalmaz információkat, például az IoT-eszközről küldött érzékelőadatait."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT adattárolás, iot érzékelő adatokat tároló"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: be72d9ba9a781822926364351b50f58f5d96e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a><span data-ttu-id="eecda-105">Az IoT hub üzeneteket, amelyek tartalmazzák az érzékelő adatokat tooyour Azure table storage mentése</span><span class="sxs-lookup"><span data-stu-id="eecda-105">Save IoT hub messages that contain sensor data tooyour Azure table storage</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="eecda-107">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="eecda-107">What you learn</span></span>

<span data-ttu-id="eecda-108">Megtudhatja, hogyan működik az toocreate egy Azure storage-fiókot és az Azure app toostore IoT hub üzenetek a table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="eecda-108">You learn how toocreate an Azure storage account and an Azure function app toostore IoT hub messages in your table storage.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="eecda-109">Mit</span><span class="sxs-lookup"><span data-stu-id="eecda-109">What you do</span></span>

- <span data-ttu-id="eecda-110">Hozzon létre egy Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="eecda-110">Create an Azure storage account.</span></span>
- <span data-ttu-id="eecda-111">Készítse elő az IoT hub kapcsolati tooread üzenetek.</span><span class="sxs-lookup"><span data-stu-id="eecda-111">Prepare your IoT hub connection tooread messages.</span></span>
- <span data-ttu-id="eecda-112">Létrehozhat és telepíthet egy Azure függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eecda-112">Create and deploy an Azure function app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="eecda-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="eecda-113">What you need</span></span>

- <span data-ttu-id="eecda-114">[Konfigurálja az eszközt](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="eecda-114">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello following requirements:</span></span>
  - <span data-ttu-id="eecda-115">Aktív Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="eecda-115">An active Azure subscription</span></span>
  - <span data-ttu-id="eecda-116">Az IoT-központ az előfizetéshez tartozó</span><span class="sxs-lookup"><span data-stu-id="eecda-116">An IoT hub under your subscription</span></span> 
  - <span data-ttu-id="eecda-117">Egy futó alkalmazás által küldött üzenetek tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="eecda-117">A running application that sends messages tooyour IoT hub</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="eecda-118">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="eecda-118">Create an Azure storage account</span></span>

1. <span data-ttu-id="eecda-119">A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **tárolási** > **tárfiók**  >   **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="eecda-119">In hello [Azure portal](https://portal.azure.com/), click **New** > **Storage** > **Storage account** > **Create**.</span></span>

2. <span data-ttu-id="eecda-120">Adja meg hello hello tárfiók szükséges adatokat:</span><span class="sxs-lookup"><span data-stu-id="eecda-120">Enter hello necessary information for hello storage account:</span></span>

   ![Hozzon létre egy tárfiókot a hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * <span data-ttu-id="eecda-122">**Név**: hello hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="eecda-122">**Name**: hello name of hello storage account.</span></span> <span data-ttu-id="eecda-123">hello nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="eecda-123">hello name must be globally unique.</span></span>

   * <span data-ttu-id="eecda-124">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="eecda-124">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="eecda-125">**PIN-kód toodashboard**: válassza ezt a beállítást, mennyire egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="eecda-125">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

3. <span data-ttu-id="eecda-126">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-126">Click **Create**.</span></span>

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a><span data-ttu-id="eecda-127">Az IoT hub kapcsolati tooread üzenetek előkészítése</span><span class="sxs-lookup"><span data-stu-id="eecda-127">Prepare your IoT hub connection tooread messages</span></span>

<span data-ttu-id="eecda-128">Az IoT-központ közzététele egy beépített hub-kompatibilis végpont tooenable alkalmazások tooread IoT hub eseményüzenetek.</span><span class="sxs-lookup"><span data-stu-id="eecda-128">IoT hub exposes a built-in event hub-compatible endpoint tooenable applications tooread IoT hub messages.</span></span> <span data-ttu-id="eecda-129">Eközben alkalmazások a fogyasztói csoportok tooread adatokat az IoT hub használhatják.</span><span class="sxs-lookup"><span data-stu-id="eecda-129">Meanwhile, applications use consumer groups tooread data from your IoT hub.</span></span> <span data-ttu-id="eecda-130">Az IoT hub hoz létre egy Azure függvény alkalmazásadatok tooread, mielőtt hello a következő:</span><span class="sxs-lookup"><span data-stu-id="eecda-130">Before you create an Azure function app tooread data from your IoT hub, do hello following:</span></span>

- <span data-ttu-id="eecda-131">Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="eecda-131">Get hello connection string of your IoT hub endpoint.</span></span>
- <span data-ttu-id="eecda-132">Hozzon létre az IoT hub fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="eecda-132">Create a consumer group for your IoT hub.</span></span>

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a><span data-ttu-id="eecda-133">Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="eecda-133">Get hello connection string of your IoT hub endpoint</span></span>

1. <span data-ttu-id="eecda-134">Nyissa meg az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eecda-134">Open your IoT hub.</span></span>

2. <span data-ttu-id="eecda-135">A hello **IoT-központ** ablaktáblán, a **Messaging**, kattintson a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="eecda-135">On hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="eecda-136">Hello a jobb oldali ablaktáblán, a **beépített végpontok**, kattintson a **események**.</span><span class="sxs-lookup"><span data-stu-id="eecda-136">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="eecda-137">A hello **tulajdonságok** ablaktáblán, a következő értékek Megjegyzés hello:</span><span class="sxs-lookup"><span data-stu-id="eecda-137">In hello **Properties** pane, note hello following values:</span></span>
   - <span data-ttu-id="eecda-138">Event hub-kompatibilis végpont</span><span class="sxs-lookup"><span data-stu-id="eecda-138">Event hub-compatible endpoint</span></span>
   - <span data-ttu-id="eecda-139">Event hub-kompatibilis neve</span><span class="sxs-lookup"><span data-stu-id="eecda-139">Event hub-compatible name</span></span>

   ![Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása a hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. <span data-ttu-id="eecda-141">A hello **IoT-központ** ablaktáblán, a **beállítások**, kattintson a **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="eecda-141">In hello **IoT Hub** pane, under **Settings**, click **Shared access policies**.</span></span>

6. <span data-ttu-id="eecda-142">Kattintson a **iothubowner**.</span><span class="sxs-lookup"><span data-stu-id="eecda-142">Click **iothubowner**.</span></span>

7. <span data-ttu-id="eecda-143">Megjegyzés: hello **elsődleges kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="eecda-143">Note hello **Primary key** value.</span></span>

8. <span data-ttu-id="eecda-144">Az alábbiak szerint hozza létre az IoT-központ végpontjának hello kapcsolati karakterlánca:</span><span class="sxs-lookup"><span data-stu-id="eecda-144">Create hello connection string of your IoT hub endpoint as follows:</span></span>

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > <span data-ttu-id="eecda-145">Cserélje le `<Event Hub-compatible endpoint>` és `<Primary key>` hello értékekkel, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="eecda-145">Replace `<Event Hub-compatible endpoint>` and `<Primary key>` with hello values that you noted earlier.</span></span>

### <a name="create-a-consumer-group-for-your-iot-hub"></a><span data-ttu-id="eecda-146">Az IoT hub felhasználói csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="eecda-146">Create a consumer group for your IoT hub</span></span>

1. <span data-ttu-id="eecda-147">Nyissa meg az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eecda-147">Open your IoT hub.</span></span>

2. <span data-ttu-id="eecda-148">A hello **IoT-központ** ablaktáblán, a **Messaging**, kattintson a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="eecda-148">In hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="eecda-149">Hello a jobb oldali ablaktáblán, a **beépített végpontok**, kattintson a **események**.</span><span class="sxs-lookup"><span data-stu-id="eecda-149">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="eecda-150">A hello **tulajdonságok** ablaktáblán, a **fogyasztói csoportok**, és adjon meg egy nevet, majd jegyezze fel az azt.</span><span class="sxs-lookup"><span data-stu-id="eecda-150">In hello **Properties** pane, under **Consumer groups**, enter a name, and then make a note of it.</span></span>

5. <span data-ttu-id="eecda-151">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-151">Click **Save**.</span></span>

## <a name="create-and-deploy-an-azure-function-app"></a><span data-ttu-id="eecda-152">Hozzon létre és Azure függvény alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="eecda-152">Create and deploy an Azure function app</span></span>

1. <span data-ttu-id="eecda-153">A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **számítási** > **függvény App**  >   **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="eecda-153">In hello [Azure portal](https://portal.azure.com/), click **New** > **Compute** > **Function App** > **Create**.</span></span>

2. <span data-ttu-id="eecda-154">Adja meg a szükséges információkat hello hello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eecda-154">Enter hello necessary information for hello function app.</span></span>

   ![Egy függvény-alkalmazás létrehozása az Azure-portálon hello](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * <span data-ttu-id="eecda-156">**Alkalmazásnév**: hello függvény alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="eecda-156">**App name**: hello name of hello function app.</span></span> <span data-ttu-id="eecda-157">hello nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="eecda-157">hello name must be globally unique.</span></span>

   * <span data-ttu-id="eecda-158">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="eecda-158">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="eecda-159">**A Tárfiók**: hello létrehozott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="eecda-159">**Storage Account**: hello storage account that you created.</span></span>

   * <span data-ttu-id="eecda-160">**PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés toohello függvény alkalmazás hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="eecda-160">**Pin toodashboard**: Check this option for easy access toohello function app from hello dashboard.</span></span>

3. <span data-ttu-id="eecda-161">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-161">Click **Create**.</span></span>

4. <span data-ttu-id="eecda-162">Hello függvény alkalmazás létrehozása után nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="eecda-162">After hello function app has been created, open it.</span></span>

5. <span data-ttu-id="eecda-163">Hello függvény alkalmazás hozzon létre egy új függvényt hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="eecda-163">In hello function app, create a new function by doing hello following:</span></span>

   <span data-ttu-id="eecda-164">a.</span><span class="sxs-lookup"><span data-stu-id="eecda-164">a.</span></span> <span data-ttu-id="eecda-165">Kattintson a **új függvény**.</span><span class="sxs-lookup"><span data-stu-id="eecda-165">Click **New Function**.</span></span>

   <span data-ttu-id="eecda-166">b.</span><span class="sxs-lookup"><span data-stu-id="eecda-166">b.</span></span> <span data-ttu-id="eecda-167">Válassza ki **JavaScript** a **nyelvi**, és **adatfeldolgozási** a **forgatókönyv**.</span><span class="sxs-lookup"><span data-stu-id="eecda-167">Select **JavaScript** for **Language**, and **Data Processing** for **Scenario**.</span></span>

   <span data-ttu-id="eecda-168">c.</span><span class="sxs-lookup"><span data-stu-id="eecda-168">c.</span></span> <span data-ttu-id="eecda-169">Kattintson a **Ez a függvény létrehozása**, és kattintson a **új függvény**.</span><span class="sxs-lookup"><span data-stu-id="eecda-169">Click **Create this function**, and then click **New Function**.</span></span>

   <span data-ttu-id="eecda-170">d.</span><span class="sxs-lookup"><span data-stu-id="eecda-170">d.</span></span> <span data-ttu-id="eecda-171">Válassza ki **JavaScript** hello nyelvhez és **adatfeldolgozási** hello forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="eecda-171">Select **JavaScript** for hello language, and **Data Processing** for hello scenario.</span></span>

   <span data-ttu-id="eecda-172">e.</span><span class="sxs-lookup"><span data-stu-id="eecda-172">e.</span></span> <span data-ttu-id="eecda-173">Kattintson a hello **EventHubTrigger-JavaScript** sablont.</span><span class="sxs-lookup"><span data-stu-id="eecda-173">Click hello **EventHubTrigger-JavaScript** template.</span></span>

   <span data-ttu-id="eecda-174">f.</span><span class="sxs-lookup"><span data-stu-id="eecda-174">f.</span></span> <span data-ttu-id="eecda-175">Adja meg a szükséges információkat hello hello sablont.</span><span class="sxs-lookup"><span data-stu-id="eecda-175">Enter hello necessary information for hello template.</span></span>

      * <span data-ttu-id="eecda-176">**A funkció neve**: hello hello függvény neve.</span><span class="sxs-lookup"><span data-stu-id="eecda-176">**Name your function**: hello name of hello function.</span></span>

      * <span data-ttu-id="eecda-177">**Eseményközpont neveként**: hello event hub-kompatibilis nevét, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="eecda-177">**Event Hub name**: hello event hub-compatible name that you noted earlier.</span></span>

      * <span data-ttu-id="eecda-178">**Esemény hubkapcsolat**: tooadd hello kapcsolati karakterlánca hello IoT-központ végpontjának létrehozott, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="eecda-178">**Event Hub connection**: tooadd hello connection string of hello IoT hub endpoint that you created, click **New**.</span></span>

   <span data-ttu-id="eecda-179">g.</span><span class="sxs-lookup"><span data-stu-id="eecda-179">g.</span></span> <span data-ttu-id="eecda-180">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-180">Click **Create**.</span></span>

6. <span data-ttu-id="eecda-181">Adja meg egy kimeneti hello függvény hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="eecda-181">Configure an output of hello function by doing hello following:</span></span>

   <span data-ttu-id="eecda-182">a.</span><span class="sxs-lookup"><span data-stu-id="eecda-182">a.</span></span> <span data-ttu-id="eecda-183">Kattintson a **integrálni** > **új kimeneti** > **Azure Table Storage** > **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="eecda-183">Click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

      ![Adja hozzá a table storage tooyour függvény app hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   <span data-ttu-id="eecda-185">b.</span><span class="sxs-lookup"><span data-stu-id="eecda-185">b.</span></span> <span data-ttu-id="eecda-186">Adja meg a hello szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="eecda-186">Enter hello necessary information.</span></span>

      * <span data-ttu-id="eecda-187">**Tábla paraméter neve**: használata `outputTable`, amely használandó hello Azure függvény kódot.</span><span class="sxs-lookup"><span data-stu-id="eecda-187">**Table parameter name**: Use `outputTable`, which will be used in hello Azure function's code.</span></span>
      
      * <span data-ttu-id="eecda-188">**Táblanév**: használata `deviceData`.</span><span class="sxs-lookup"><span data-stu-id="eecda-188">**Table name**: Use `deviceData`.</span></span>

      * <span data-ttu-id="eecda-189">**Fiók tárolókapcsolat**: kattintson a **új**, majd válassza ki vagy adja meg a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="eecda-189">**Storage account connection**: Click **New**, and then select or enter your storage account.</span></span> <span data-ttu-id="eecda-190">Ha hello tárfiók nem jelenik meg, tekintse meg [tárolási fiókra vonatkozó követelmények](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="eecda-190">If hello storage account is not displayed, see [Storage account requirements](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span></span>
      
   <span data-ttu-id="eecda-191">c.</span><span class="sxs-lookup"><span data-stu-id="eecda-191">c.</span></span> <span data-ttu-id="eecda-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-192">Click **Save**.</span></span>

7. <span data-ttu-id="eecda-193">A **eseményindítók**, kattintson a **Azure Event Hubs (eventHubMessages)**.</span><span class="sxs-lookup"><span data-stu-id="eecda-193">Under **Triggers**, click **Azure Event Hub (eventHubMessages)**.</span></span>

8. <span data-ttu-id="eecda-194">A **Eseményközpont fogyasztói csoportot**, adjon meg hello hello fogyasztói csoportot létrehozni, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="eecda-194">Under **Event Hub consumer group**, enter hello name of hello consumer group that you created, and then click **Save**.</span></span>

9. <span data-ttu-id="eecda-195">Kattintson a bal oldali hello létrehozott hello függvény majd **fájlok megtekintése** a jobb oldali hello.</span><span class="sxs-lookup"><span data-stu-id="eecda-195">Click hello function you've created on hello left and then click **View Files** on hello right.</span></span>

10. <span data-ttu-id="eecda-196">Cserélje le a hello kód `index.js` hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="eecda-196">Replace hello code in `index.js` with hello following:</span></span>

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in hello IoT hub.
   // hello message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. <span data-ttu-id="eecda-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="eecda-197">Click **Save**.</span></span>

<span data-ttu-id="eecda-198">Most létrehozott hello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eecda-198">You have now created hello function app.</span></span> <span data-ttu-id="eecda-199">Üzenetek, amely az IoT hub megkapja a table storage-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="eecda-199">It stores messages that your IoT hub receives in your table storage.</span></span>

> [!NOTE]
> <span data-ttu-id="eecda-200">Használhatja a hello **futtatása** gomb tootest hello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eecda-200">You can use hello **Run** button tootest hello function app.</span></span> <span data-ttu-id="eecda-201">Amikor rákattint **futtatása**, hello tesztüzenet küldése tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="eecda-201">When you click **Run**, hello test message is sent tooyour IoT hub.</span></span> <span data-ttu-id="eecda-202">hello érkezésének üdvözlőüzenetére a kell hello függvény app toostart elindítható, és mentse a hello üzenet tooyour a table storage.</span><span class="sxs-lookup"><span data-stu-id="eecda-202">hello arrival of hello message should trigger hello function app toostart and then save hello message tooyour table storage.</span></span> <span data-ttu-id="eecda-203">Hello **naplók** ablaktábla hello hello folyamat részletes adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="eecda-203">hello **Logs** pane records hello details of hello process.</span></span>

## <a name="verify-your-message-in-your-table-storage"></a><span data-ttu-id="eecda-204">Ellenőrizze az üzenetet a table storage-ban</span><span class="sxs-lookup"><span data-stu-id="eecda-204">Verify your message in your table storage</span></span>

1. <span data-ttu-id="eecda-205">Az eszköz toosend üzenetek tooyour IoT hub hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="eecda-205">Run hello sample application on your device toosend messages tooyour IoT hub.</span></span>

2. <span data-ttu-id="eecda-206">[Töltse le és telepítse az Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="eecda-206">[Download and install Azure Storage Explorer](http://storageexplorer.com/).</span></span>

3. <span data-ttu-id="eecda-207">Nyissa meg a Tártallózót, kattintson **egy Azure-fiók hozzáadása** > **jelentkezzen be a**, majd jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="eecda-207">Open Storage Explorer, click **Add an Azure Account** > **Sign in**, and then sign in tooyour Azure account.</span></span>

4. <span data-ttu-id="eecda-208">Kattintson az Azure-előfizetéshez > **Tárfiókok** > a tárfiók > **táblák** > **deviceData**.</span><span class="sxs-lookup"><span data-stu-id="eecda-208">Click your Azure subscription > **Storage Accounts** > your storage account > **Tables** > **deviceData**.</span></span>

   <span data-ttu-id="eecda-209">Láthatja, hogy az eszköz tooyour IoT hub hello bejelentkezett küldött üzenetek `deviceData` tábla.</span><span class="sxs-lookup"><span data-stu-id="eecda-209">You should see messages sent from your device tooyour IoT hub logged in hello `deviceData` table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eecda-210">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eecda-210">Next steps</span></span>

<span data-ttu-id="eecda-211">Sikeresen létrehozta az Azure storage-fiókok és az Azure függvény alkalmazás, amely tárolja az üzeneteket, amely az IoT hub megkapja a table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="eecda-211">You’ve successfully created your Azure storage account and Azure function app, which stores messages that your IoT hub receives in your table storage.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
