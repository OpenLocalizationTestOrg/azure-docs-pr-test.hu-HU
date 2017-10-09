---
title: "figyelési aaaAzure IoT-központ műveletek |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub műveletek toomonitor figyelési hello műveletek az IoT hub valós idejű állapotát."
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
ms.openlocfilehash: a0b233ef2d9bd0827e19fa30fdbdd49b2b61b813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="5d283-103">Az IoT-központ műveletek figyelése</span><span class="sxs-lookup"><span data-stu-id="5d283-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="5d283-104">IoT Hub műveletek figyelését teszi lehetővé toomonitor hello állapota az IoT hub valós idejű műveletek.</span><span class="sxs-lookup"><span data-stu-id="5d283-104">IoT Hub operations monitoring enables you toomonitor hello status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="5d283-105">Az IoT-központ között számos modulkategória közül műveletek nyomon követi az események.</span><span class="sxs-lookup"><span data-stu-id="5d283-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="5d283-106">Dönthet úgy is, az események küldhetők a egy vagy több kategóriák tooan végpontját az IoT hub feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="5d283-106">You can opt into sending events from one or more categories tooan endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="5d283-107">Hello adatok hibák figyelése, vagy adatokat minták alapján összetettebb feldolgozás beállítása.</span><span class="sxs-lookup"><span data-stu-id="5d283-107">You can monitor hello data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="5d283-108">Az IoT-központ figyeli az események hat kategóriák:</span><span class="sxs-lookup"><span data-stu-id="5d283-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="5d283-109">Eszköz identitása műveletek</span><span class="sxs-lookup"><span data-stu-id="5d283-109">Device identity operations</span></span>
* <span data-ttu-id="5d283-110">Telemetriát</span><span class="sxs-lookup"><span data-stu-id="5d283-110">Device telemetry</span></span>
* <span data-ttu-id="5d283-111">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="5d283-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="5d283-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="5d283-112">Connections</span></span>
* <span data-ttu-id="5d283-113">Fájlfeltöltések</span><span class="sxs-lookup"><span data-stu-id="5d283-113">File uploads</span></span>
* <span data-ttu-id="5d283-114">Üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="5d283-114">Message routing</span></span>

## <a name="how-tooenable-operations-monitoring"></a><span data-ttu-id="5d283-115">Hogyan tooenable műveletek figyelése</span><span class="sxs-lookup"><span data-stu-id="5d283-115">How tooenable operations monitoring</span></span>

1. <span data-ttu-id="5d283-116">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="5d283-116">Create an IoT hub.</span></span> <span data-ttu-id="5d283-117">Hogyan találhat útmutatót az IoT-központ a hello toocreate [Ismerkedés] [ lnk-get-started] útmutató.</span><span class="sxs-lookup"><span data-stu-id="5d283-117">You can find instructions on how toocreate an IoT hub in hello [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="5d283-118">Az IoT hub hello panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5d283-118">Open hello blade of your IoT hub.</span></span> <span data-ttu-id="5d283-119">Itt kattintson **figyelési műveletek**.</span><span class="sxs-lookup"><span data-stu-id="5d283-119">From there, click **Operations monitoring**.</span></span>

    ![Figyelési konfiguráció hello portálon hozzáférési műveleteket][1]

1. <span data-ttu-id="5d283-121">Jelölje be hello kategóriák toomonitor kívánja, és kattintson a figyelés **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5d283-121">Select hello monitoring categories you wish toomonitor, and then click **Save**.</span></span> <span data-ttu-id="5d283-122">hello események állnak rendelkezésre a felsorolt hello Event Hub-kompatibilis végpont olvasását **figyelési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5d283-122">hello events are available for reading from hello Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="5d283-123">hello IoT-központ végpontjának neve `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="5d283-123">hello IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Az IoT hub a figyelési műveletek konfigurálása][2]

> [!NOTE]
> <span data-ttu-id="5d283-125">Kiválasztása **részletes** hello figyelésének **kapcsolatok** kategória hatására az IoT-központ toogenerate további diagnosztikai üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="5d283-125">Selecting **Verbose** monitoring for hello **Connections** category causes IoT Hub toogenerate additional diagnostics messages.</span></span> <span data-ttu-id="5d283-126">Minden más kategóriák hello **részletes** módosítások adatokat az IoT-központ mennyisége hello beállítása minden hibaüzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5d283-126">For all other categories, hello **Verbose** setting changes hello quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-toouse-them"></a><span data-ttu-id="5d283-127">Kategóriák, és hogyan toouse őket</span><span class="sxs-lookup"><span data-stu-id="5d283-127">Event categories and how toouse them</span></span>

<span data-ttu-id="5d283-128">Minden egyes kategória nyomon követi a figyelési műveletek IoT-központot, és minden felügyeleti kategória interakcióba más típusú rendelkezik, amely meghatározza, milyen események kategória felépítése séma.</span><span class="sxs-lookup"><span data-stu-id="5d283-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="5d283-129">Eszköz identitása műveletek</span><span class="sxs-lookup"><span data-stu-id="5d283-129">Device identity operations</span></span>

<span data-ttu-id="5d283-130">hello eszközkategóriát identitás műveletek nyomon követi az toocreate tett kísérlet során előforduló hibákat, frissíteni vagy törölni az IoT hub identitásjegyzékhez egy bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="5d283-130">hello device identity operations category tracks errors that occur when you attempt toocreate, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="5d283-131">Ebbe a kategóriába követési akkor hasznos, forgatókönyvek történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d283-131">Tracking this category is useful for provisioning scenarios.</span></span>

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

### <a name="device-telemetry"></a><span data-ttu-id="5d283-132">Telemetriát</span><span class="sxs-lookup"><span data-stu-id="5d283-132">Device telemetry</span></span>

<span data-ttu-id="5d283-133">hello telemetriai eszközkategóriát hello IoT-központ halasztja és a kapcsolódó toohello telemetriai csővezeték hibákról követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="5d283-133">hello device telemetry category tracks errors that occur at hello IoT hub and are related toohello telemetry pipeline.</span></span> <span data-ttu-id="5d283-134">Ebbe a kategóriába olyan hibák telemetriai események (például a szabályozás) küldésekor és fogadásakor telemetriai események (például a jogosulatlan olvasó).</span><span class="sxs-lookup"><span data-stu-id="5d283-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="5d283-135">Ez a kategória nem dolgozza fel a futó hello eszköz által okozott hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-135">This category cannot catch errors caused by code running on hello device itself.</span></span>

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

### <a name="cloud-to-device-commands"></a><span data-ttu-id="5d283-136">Felhő eszközparancsok</span><span class="sxs-lookup"><span data-stu-id="5d283-136">Cloud-to-device commands</span></span>

<span data-ttu-id="5d283-137">hello felhő eszközparancsok kategória nyomon követi a hibákat, az IoT-központ hello halasztja és a kapcsolódó toohello felhő-eszközre küldött üzenetek feldolgozási sor.</span><span class="sxs-lookup"><span data-stu-id="5d283-137">hello cloud-to-device commands category tracks errors that occur at hello IoT hub and are related toohello cloud-to-device message pipeline.</span></span> <span data-ttu-id="5d283-138">Ebbe a kategóriába tartoznak (például a jogosulatlan feladótól) felhő eszközre üzenetküldésre, (például kézbesítési száma túllépve) felhő eszközre üzenetek fogadására, és a felhő-eszközre küldött üzenetek visszajelzés fogadása (például a visszajelzés lejárt) előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="5d283-139">Ez a kategória nem dolgozza fel a hibák egy eszközről, amely nem megfelelően kezeli a felhő eszközre üzenetet, ha felhő eszközre üdvözlőüzenetére lett sikeresen kézbesítve.</span><span class="sxs-lookup"><span data-stu-id="5d283-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if hello cloud-to-device message was delivered successfully.</span></span>

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

### <a name="connections"></a><span data-ttu-id="5d283-140">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="5d283-140">Connections</span></span>

<span data-ttu-id="5d283-141">hello kapcsolatok kategória nyomon követi az eszközök csatlakozni, vagy válassza le az IoT-központ előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-141">hello connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="5d283-142">Ebbe a kategóriába követési akkor hasznos, jogosulatlan kapcsolódási kísérletek azonosítására és nyomon követése, ha a kapcsolat azért területein gyenge hálózati eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="5d283-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

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

### <a name="file-uploads"></a><span data-ttu-id="5d283-143">Fájlfeltöltések</span><span class="sxs-lookup"><span data-stu-id="5d283-143">File uploads</span></span>

<span data-ttu-id="5d283-144">hello fájl feltöltése kategória hello IoT-központ halasztja és a kapcsolódó toofile feltöltés funkció hibákról követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="5d283-144">hello file upload category tracks errors that occur at hello IoT hub and are related toofile upload functionality.</span></span> <span data-ttu-id="5d283-145">Ez a kategória tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5d283-145">This category includes:</span></span>

* <span data-ttu-id="5d283-146">Hello SAS URI-t, például amikor egy eszköz, a feltöltött hello hub értesíti előtt lejár az előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-146">Errors that occur with hello SAS URI, such as when it expires before a device notifies hello hub of a completed upload.</span></span>
* <span data-ttu-id="5d283-147">Nem sikerült hello eszköz által jelzett feltöltések.</span><span class="sxs-lookup"><span data-stu-id="5d283-147">Failed uploads reported by hello device.</span></span>
* <span data-ttu-id="5d283-148">Ha egy fájl nem található a tároló az IoT-központ értesítési üzenet létrehozása során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="5d283-149">Ez a kategória nem dolgozza fel a jelentkező hibák közvetlenül hello eszköz egy fájl toostorage tölti fel.</span><span class="sxs-lookup"><span data-stu-id="5d283-149">This category cannot catch errors that directly occur while hello device is uploading a file toostorage.</span></span>

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

### <a name="message-routing"></a><span data-ttu-id="5d283-150">Üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="5d283-150">Message routing</span></span>

<span data-ttu-id="5d283-151">hello üzenet útválasztási kategória nyomon követi az üzenet útvonal értékelési és végpont-állapotot az IoT-központ által érzékelt során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="5d283-151">hello message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="5d283-152">Ebbe a kategóriába olyan események, például amikor egy szabály akkor túl "nem definiált", ha az IoT-központ állapotúként jelöli meg a végpont kézbesítetlen, és bármely más hibák végpont.</span><span class="sxs-lookup"><span data-stu-id="5d283-152">This category includes events such as when a rule evaluates too"undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="5d283-153">Ebbe a kategóriába nem tartalmaz bizonyos hibákat köszönőüzenetei maguk (például a szabályozás hibák eszköz), a hello "telemetriát" kategória jelentett kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5d283-153">This category does not include specific errors about hello messages themselves (such as device throttling errors), which are reported under hello "device telemetry" category.</span></span>

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

## <a name="view-events"></a><span data-ttu-id="5d283-154">események megtekintése</span><span class="sxs-lookup"><span data-stu-id="5d283-154">View events</span></span>

<span data-ttu-id="5d283-155">Használhatja a hello *IOT hubbal-explorer* eszköz tooquickly tesztelje, hogy az IoT hub előállító figyelési esemény.</span><span class="sxs-lookup"><span data-stu-id="5d283-155">You can use hello *iothub-explorer* tool tooquickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="5d283-156">tooinstall hello eszköz, lásd: hello hello utasításait [IOT hubbal-explorer] [ lnk-iothub-explorer] GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="5d283-156">tooinstall hello tool, see hello instructions in hello [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="5d283-157">Győződjön meg arról, hogy hello **kapcsolatok** figyelő kategória értéke túl**részletes** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="5d283-157">Make sure hello **Connections** monitoring category is set too**Verbose** in hello portal.</span></span>

1. <span data-ttu-id="5d283-158">Parancsot egy parancssorba futtassa a parancsot tooread követően – figyelési végpontját hello hello:</span><span class="sxs-lookup"><span data-stu-id="5d283-158">At a command-prompt, run hello following command tooread from hello monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="5d283-159">Egy másik-parancssorban futtassa a következő parancs toosimulate hello eszközről a felhőbe üzeneteket küldő eszköz:</span><span class="sxs-lookup"><span data-stu-id="5d283-159">In another command-prompt, run hello following command toosimulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="5d283-160">hello szimulált eszköz csatlakozik az IoT-központ tooyour hello első parancssori látható hello figyelési esemény.</span><span class="sxs-lookup"><span data-stu-id="5d283-160">hello first command-prompt shows hello monitoring events as hello simulated device connects tooyour IoT hub.</span></span>

## <a name="connect-toohello-monitoring-endpoint"></a><span data-ttu-id="5d283-161">Figyelési végpontját toohello csatlakozás</span><span class="sxs-lookup"><span data-stu-id="5d283-161">Connect toohello monitoring endpoint</span></span>

<span data-ttu-id="5d283-162">az IoT hub-végpont figyelési hello Event Hub-kompatibilis végpont.</span><span class="sxs-lookup"><span data-stu-id="5d283-162">hello monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="5d283-163">Bármely mechanizmus, amely az Event Hubs tooread figyelési üzenetek ehhez a végponthoz is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5d283-163">You can use any mechanism that works with Event Hubs tooread monitoring messages from this endpoint.</span></span> <span data-ttu-id="5d283-164">hello alábbi minta létrehoz egy alapszintű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="5d283-164">hello following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="5d283-165">Hogyan tooprocess Eseményközpontokból származó üzenetek kapcsolatos további információkért lásd: hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="5d283-165">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="5d283-166">tooconnect toohello figyelési végpont kell egy kapcsolati karakterláncot és a hello a végpont neve.</span><span class="sxs-lookup"><span data-stu-id="5d283-166">tooconnect toohello monitoring endpoint, you need a connection string and hello endpoint name.</span></span> <span data-ttu-id="5d283-167">hello lépések bemutatják, hogyan toofind hello hello portálon szükséges értékeket:</span><span class="sxs-lookup"><span data-stu-id="5d283-167">hello following steps show you how toofind hello necessary values in hello portal:</span></span>

1. <span data-ttu-id="5d283-168">Hello portálon lépjen az IoT-központ erőforráspanelen tooyour.</span><span class="sxs-lookup"><span data-stu-id="5d283-168">In hello portal, navigate tooyour IoT Hub resource blade.</span></span>

1. <span data-ttu-id="5d283-169">Válasszon **figyelési műveletek**, és jegyezze fel a hello **Event Hub-kompatibilis neve** és **Event Hub-kompatibilis végpont** értékeket:</span><span class="sxs-lookup"><span data-stu-id="5d283-169">Choose **Operations monitoring**, and make a note of hello **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Event Hub-kompatibilis végpont értékek][img-endpoints]

1. <span data-ttu-id="5d283-171">Válasszon **megosztott elérési házirendek**, majd válassza a **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="5d283-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="5d283-172">Jegyezze fel a hello **elsődleges kulcs** érték:</span><span class="sxs-lookup"><span data-stu-id="5d283-172">Make a note of hello **Primary key** value:</span></span>

    ![Szolgáltatás megosztott elérési házirend elsődleges kulcs][img-service-key]

<span data-ttu-id="5d283-174">hello alábbi C# kódminta származik a Visual Studio **klasszikus Windows asztal** C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5d283-174">hello following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="5d283-175">hello projekt már hello **WindowsAzure.ServiceBus** NuGet-csomagja telepítve.</span><span class="sxs-lookup"><span data-stu-id="5d283-175">hello project has hello **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="5d283-176">Hello kapcsolati karakterlánc helyőrzőjét cserélje le a kapcsolati karakterlánc által használt hello **Event Hub-kompatibilis végpont** és szolgáltatás **elsődleges kulcs** látható módon hello következő korábban feljegyzett értékek Példa:</span><span class="sxs-lookup"><span data-stu-id="5d283-176">Replace hello connection string placeholder with a connection string that uses hello **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in hello following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="5d283-177">Cserélje le a végpont neve helyőrzőt hello figyelési hello **Event Hub-kompatibilis neve** korábban feljegyzett érték.</span><span class="sxs-lookup"><span data-stu-id="5d283-177">Replace hello monitoring endpoint name placeholder with hello **Event Hub-compatible name** value you noted previously.</span></span>

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key tooexit.\n");

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

## <a name="next-steps"></a><span data-ttu-id="5d283-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d283-178">Next steps</span></span>
<span data-ttu-id="5d283-179">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="5d283-179">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5d283-180">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="5d283-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="5d283-181">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="5d283-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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