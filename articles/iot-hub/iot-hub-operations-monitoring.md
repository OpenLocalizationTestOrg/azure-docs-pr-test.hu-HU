---
title: "Azure IoT Hub műveletek figyelése |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub műveletek figyelési műveletek az IoT hub valós idejű állapotának figyelésére."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b6de5c5df5f9401a41be152bfa06eb994594e83d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="0d897-103">Az IoT-központ műveletek figyelése</span><span class="sxs-lookup"><span data-stu-id="0d897-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="0d897-104">Figyelési IoT-központ műveletek lehetővé teszi az IoT hub valós idejű műveletek állapotának figyelése.</span><span class="sxs-lookup"><span data-stu-id="0d897-104">IoT Hub operations monitoring enables you to monitor the status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="0d897-105">Az IoT-központ között számos modulkategória közül műveletek nyomon követi az események.</span><span class="sxs-lookup"><span data-stu-id="0d897-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="0d897-106">Dönthet úgy is, az események küldése az egy vagy több kategóriához végpont az IoT hub feldolgozás esetén.</span><span class="sxs-lookup"><span data-stu-id="0d897-106">You can opt into sending events from one or more categories to an endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="0d897-107">Az adatok a hibák figyelése, vagy adatokat minták alapján összetettebb feldolgozás beállítása.</span><span class="sxs-lookup"><span data-stu-id="0d897-107">You can monitor the data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="0d897-108">Az IoT-központ figyeli az események hat kategóriák:</span><span class="sxs-lookup"><span data-stu-id="0d897-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="0d897-109">Eszköz identitása műveletek</span><span class="sxs-lookup"><span data-stu-id="0d897-109">Device identity operations</span></span>
* <span data-ttu-id="0d897-110">Telemetriát</span><span class="sxs-lookup"><span data-stu-id="0d897-110">Device telemetry</span></span>
* <span data-ttu-id="0d897-111">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="0d897-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="0d897-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="0d897-112">Connections</span></span>
* <span data-ttu-id="0d897-113">Fájlfeltöltések</span><span class="sxs-lookup"><span data-stu-id="0d897-113">File uploads</span></span>
* <span data-ttu-id="0d897-114">Üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="0d897-114">Message routing</span></span>

## <a name="how-to-enable-operations-monitoring"></a><span data-ttu-id="0d897-115">Figyelési műveletek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0d897-115">How to enable operations monitoring</span></span>

1. <span data-ttu-id="0d897-116">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="0d897-116">Create an IoT hub.</span></span> <span data-ttu-id="0d897-117">Az IoT-központ a létrehozásával találhat útmutatót a [Ismerkedés] [ lnk-get-started] útmutató.</span><span class="sxs-lookup"><span data-stu-id="0d897-117">You can find instructions on how to create an IoT hub in the [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="0d897-118">Az IoT hub panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="0d897-118">Open the blade of your IoT hub.</span></span> <span data-ttu-id="0d897-119">Itt kattintson **figyelési műveletek**.</span><span class="sxs-lookup"><span data-stu-id="0d897-119">From there, click **Operations monitoring**.</span></span>

    ![A portálon konfigurációs figyelési műveletek][1]

1. <span data-ttu-id="0d897-121">Válassza ki a figyelési kategóriákat szeretne figyelni, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0d897-121">Select the monitoring categories you wish to monitor, and then click **Save**.</span></span> <span data-ttu-id="0d897-122">Az események állnak rendelkezésre a felsorolt Event Hub-kompatibilis végpont a olvasását **figyelési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0d897-122">The events are available for reading from the Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="0d897-123">Az IoT-központ végpontjának nevezik `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="0d897-123">The IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Az IoT hub a figyelési műveletek konfigurálása][2]

> [!NOTE]
> <span data-ttu-id="0d897-125">Kiválasztása **részletes** figyelését a **kapcsolatok** kategória hatására az IoT-központ létrehozhat további diagnosztikai üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="0d897-125">Selecting **Verbose** monitoring for the **Connections** category causes IoT Hub to generate additional diagnostics messages.</span></span> <span data-ttu-id="0d897-126">Az összes egyéb kategóriába a **részletes** beállítása a módosításokat az IoT-központ adatok mennyiségére minden hibaüzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0d897-126">For all other categories, the **Verbose** setting changes the quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-to-use-them"></a><span data-ttu-id="0d897-127">Esemény kategóriák és a használatukat</span><span class="sxs-lookup"><span data-stu-id="0d897-127">Event categories and how to use them</span></span>

<span data-ttu-id="0d897-128">Minden egyes kategória nyomon követi a figyelési műveletek IoT-központot, és minden felügyeleti kategória interakcióba más típusú rendelkezik, amely meghatározza, milyen események kategória felépítése séma.</span><span class="sxs-lookup"><span data-stu-id="0d897-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="0d897-129">Eszköz identitása műveletek</span><span class="sxs-lookup"><span data-stu-id="0d897-129">Device identity operations</span></span>

<span data-ttu-id="0d897-130">Az identitás műveletek eszközkategória nyomon követi az létrehozása, frissíteni vagy törölni egy bejegyzést az IoT hub identitásjegyzékhez tett kísérlet során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-130">The device identity operations category tracks errors that occur when you attempt to create, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="0d897-131">Ebbe a kategóriába követési akkor hasznos, forgatókönyvek történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="0d897-131">Tracking this category is useful for provisioning scenarios.</span></span>

```json
{
    "time": "UTC timestamp",
        "operationName": "create",
        "category": "DeviceIdentityOperations",
        "level": "Error",
        "statusCode": 4XX,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "durationMs": 1234,
        "userAgent": "userAgent",
        "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a><span data-ttu-id="0d897-132">Telemetriát</span><span class="sxs-lookup"><span data-stu-id="0d897-132">Device telemetry</span></span>

<span data-ttu-id="0d897-133">Az eszközkategória telemetriai nyomon követi a telemetria-feldolgozási folyamat kapcsolódnak, és az IoT hub előforduló hibákról.</span><span class="sxs-lookup"><span data-stu-id="0d897-133">The device telemetry category tracks errors that occur at the IoT hub and are related to the telemetry pipeline.</span></span> <span data-ttu-id="0d897-134">Ebbe a kategóriába olyan hibák telemetriai események (például a szabályozás) küldésekor és fogadásakor telemetriai események (például a jogosulatlan olvasó).</span><span class="sxs-lookup"><span data-stu-id="0d897-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="0d897-135">Ez a kategória nem dolgozza fel a futó maga az eszköz által okozott hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-135">This category cannot catch errors caused by code running on the device itself.</span></span>

```json
{
        "messageSizeInBytes": 1234,
        "batching": 0,
        "protocol": "Amqp",
        "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
        "time": "UTC timestamp",
        "operationName": "ingress",
        "category": "DeviceTelemetry",
        "level": "Error",
        "statusCode": 4XX,
        "statusType": 4XX001,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "EventProcessedUtcTime": "UTC timestamp",
        "PartitionId": 1,
        "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a><span data-ttu-id="0d897-136">Felhő eszközparancsok</span><span class="sxs-lookup"><span data-stu-id="0d897-136">Cloud-to-device commands</span></span>

<span data-ttu-id="0d897-137">A felhő eszközparancsok kategória követi nyomon, hogy a felhő-eszközre küldött üzenetek feldolgozási folyamat kapcsolódnak, és az IoT hub előforduló hibákról.</span><span class="sxs-lookup"><span data-stu-id="0d897-137">The cloud-to-device commands category tracks errors that occur at the IoT hub and are related to the cloud-to-device message pipeline.</span></span> <span data-ttu-id="0d897-138">Ebbe a kategóriába tartoznak (például a jogosulatlan feladótól) felhő eszközre üzenetküldésre, (például kézbesítési száma túllépve) felhő eszközre üzenetek fogadására, és a felhő-eszközre küldött üzenetek visszajelzés fogadása (például a visszajelzés lejárt) előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="0d897-139">Ez a kategória nem dolgozza fel a hibák egy eszközről, amely nem megfelelően kezeli a felhő eszközre üzenetet, ha a felhő eszközre üzenet sikeresen lett kézbesítve.</span><span class="sxs-lookup"><span data-stu-id="0d897-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if the cloud-to-device message was delivered successfully.</span></span>

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": “UTC timestamp"
}
```

### <a name="connections"></a><span data-ttu-id="0d897-140">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="0d897-140">Connections</span></span>

<span data-ttu-id="0d897-141">A kapcsolatok kategória nyomon követi az eszközök csatlakozni, vagy válassza le az IoT-központ előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-141">The connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="0d897-142">Ebbe a kategóriába követési akkor hasznos, jogosulatlan kapcsolódási kísérletek azonosítására és nyomon követése, ha a kapcsolat azért területein gyenge hálózati eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="0d897-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a><span data-ttu-id="0d897-143">Fájlfeltöltések</span><span class="sxs-lookup"><span data-stu-id="0d897-143">File uploads</span></span>

<span data-ttu-id="0d897-144">A fájl feltöltése kategória nyomon követi a fájl feltöltése funkció kapcsolódnak, és az IoT hub előforduló hibákról.</span><span class="sxs-lookup"><span data-stu-id="0d897-144">The file upload category tracks errors that occur at the IoT hub and are related to file upload functionality.</span></span> <span data-ttu-id="0d897-145">Ez a kategória tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0d897-145">This category includes:</span></span>

* <span data-ttu-id="0d897-146">A SAS URI-azonosítóhoz, például amikor egy eszköz értesíti a központ, a feltöltött előtt lejár előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-146">Errors that occur with the SAS URI, such as when it expires before a device notifies the hub of a completed upload.</span></span>
* <span data-ttu-id="0d897-147">Nem sikerült az eszköz által jelentett feltöltések.</span><span class="sxs-lookup"><span data-stu-id="0d897-147">Failed uploads reported by the device.</span></span>
* <span data-ttu-id="0d897-148">Ha egy fájl nem található a tároló az IoT-központ értesítési üzenet létrehozása során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="0d897-149">Ez a kategória nem dolgozza fel a jelentkező hibák közvetlenül az eszközre van egy feltöltés tároló.</span><span class="sxs-lookup"><span data-stu-id="0d897-149">This category cannot catch errors that directly occur while the device is uploading a file to storage.</span></span>

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a><span data-ttu-id="0d897-150">Üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="0d897-150">Message routing</span></span>

<span data-ttu-id="0d897-151">Az üzenet útválasztási kategória nyomon követi az üzenet útvonal értékelési és végpont-állapotot az IoT-központ által érzékelt során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-151">The message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="0d897-152">Ebbe a kategóriába olyan események, például amikor egy szabály akkor ad vissza "nem definiált", ha IoT-központ állapotúként jelöli meg a végpont kézbesítetlen, és bármely más hibák végpont.</span><span class="sxs-lookup"><span data-stu-id="0d897-152">This category includes events such as when a rule evaluates to "undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="0d897-153">Ebbe a kategóriába nem tartalmazza az üzenetek maguk (például a szabályozás hibák eszköz), a "telemetriát" kategóriához tartozó jelentett bizonyos hibákat.</span><span class="sxs-lookup"><span data-stu-id="0d897-153">This category does not include specific errors about the messages themselves (such as device throttling errors), which are reported under the "device telemetry" category.</span></span>

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a><span data-ttu-id="0d897-154">események megtekintése</span><span class="sxs-lookup"><span data-stu-id="0d897-154">View events</span></span>

<span data-ttu-id="0d897-155">Használhatja a *IOT hubbal-explorer* eszköz gyors teszteléséhez, hogy az IoT hub előállító figyelési esemény.</span><span class="sxs-lookup"><span data-stu-id="0d897-155">You can use the *iothub-explorer* tool to quickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="0d897-156">Az eszköz telepítéséhez tekintse át a megjelenő utasításokat a [IOT hubbal-explorer] [ lnk-iothub-explorer] GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="0d897-156">To install the tool, see the instructions in the [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="0d897-157">Győződjön meg arról, hogy a **kapcsolatok** figyelő kategória értéke **részletes** a portálon.</span><span class="sxs-lookup"><span data-stu-id="0d897-157">Make sure the **Connections** monitoring category is set to **Verbose** in the portal.</span></span>

1. <span data-ttu-id="0d897-158">Parancsot egy parancssorba a következő parancsot a figyelési végpont olvasásakor:</span><span class="sxs-lookup"><span data-stu-id="0d897-158">At a command-prompt, run the following command to read from the monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="0d897-159">Egy másik-parancssorban futtassa a következő parancsot egy eszköz, eszköz-felhő üzenetküldésre szimulálása:</span><span class="sxs-lookup"><span data-stu-id="0d897-159">In another command-prompt, run the following command to simulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="0d897-160">A szimulált eszköz csatlakozik az IoT hub, az első parancssori látható a figyelési esemény.</span><span class="sxs-lookup"><span data-stu-id="0d897-160">The first command-prompt shows the monitoring events as the simulated device connects to your IoT hub.</span></span>

## <a name="connect-to-the-monitoring-endpoint"></a><span data-ttu-id="0d897-161">A figyelési végponthoz kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="0d897-161">Connect to the monitoring endpoint</span></span>

<span data-ttu-id="0d897-162">A figyelési az IoT hub-végpont Event Hub-kompatibilis végpont.</span><span class="sxs-lookup"><span data-stu-id="0d897-162">The monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="0d897-163">Bármely mechanizmus, amely az Event Hubs figyelési üzenetek olvasásakor a végpont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0d897-163">You can use any mechanism that works with Event Hubs to read monitoring messages from this endpoint.</span></span> <span data-ttu-id="0d897-164">Az alábbi minta létrehoz egy alapszintű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="0d897-164">The following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="0d897-165">Az Event Hubs-üzenetek feldolgozásával kapcsolatos további információkért lásd [az Event Hubs használatának első lépéseit][lnk-eventhubs-tutorial] ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="0d897-165">For more information about how to process messages from Event Hubs, see the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="0d897-166">A figyelési végponthoz kapcsolódni kell egy kapcsolati karakterláncot és a végpont neve.</span><span class="sxs-lookup"><span data-stu-id="0d897-166">To connect to the monitoring endpoint, you need a connection string and the endpoint name.</span></span> <span data-ttu-id="0d897-167">A következő lépések bemutatják a szükséges értékek keresése a portál:</span><span class="sxs-lookup"><span data-stu-id="0d897-167">The following steps show you how to find the necessary values in the portal:</span></span>

1. <span data-ttu-id="0d897-168">A portálon lépjen az IoT-központ erőforráspanelre.</span><span class="sxs-lookup"><span data-stu-id="0d897-168">In the portal, navigate to your IoT Hub resource blade.</span></span>

1. <span data-ttu-id="0d897-169">Válasszon **figyelési műveletek**, és jegyezze fel a a **Event Hub-kompatibilis neve** és **Event Hub-kompatibilis végpont** értékeket:</span><span class="sxs-lookup"><span data-stu-id="0d897-169">Choose **Operations monitoring**, and make a note of the **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Event Hub-kompatibilis végpont értékek][img-endpoints]

1. <span data-ttu-id="0d897-171">Válasszon **megosztott elérési házirendek**, majd válassza a **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="0d897-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="0d897-172">Jegyezze fel a **elsődleges kulcs** érték:</span><span class="sxs-lookup"><span data-stu-id="0d897-172">Make a note of the **Primary key** value:</span></span>

    ![Szolgáltatás megosztott elérési házirend elsődleges kulcs][img-service-key]

<span data-ttu-id="0d897-174">A következő C# kódminta származik a Visual Studio **klasszikus Windows asztal** C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0d897-174">The following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="0d897-175">A projekt már a **WindowsAzure.ServiceBus** NuGet-csomagja telepítve.</span><span class="sxs-lookup"><span data-stu-id="0d897-175">The project has the **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="0d897-176">Cserélje le a kapcsolati karakterlánc helyőrzőjét egy olyan kapcsolati karakterlánccal, amely használja a **Event Hub-kompatibilis végpont** és szolgáltatás **elsődleges kulcs** értékek, akkor azt már korábban említettük a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0d897-176">Replace the connection string placeholder with a connection string that uses the **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in the following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="0d897-177">A figyelési végpont nevét helyőrzőt cserélje le a **Event Hub-kompatibilis neve** korábban feljegyzett érték.</span><span class="sxs-lookup"><span data-stu-id="0d897-177">Replace the monitoring endpoint name placeholder with the **Event Hub-compatible name** value you noted previously.</span></span>

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="0d897-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d897-178">Next steps</span></span>
<span data-ttu-id="0d897-179">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="0d897-179">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0d897-180">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="0d897-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="0d897-181">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="0d897-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md