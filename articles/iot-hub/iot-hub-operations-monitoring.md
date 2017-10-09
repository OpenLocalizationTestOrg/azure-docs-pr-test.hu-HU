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
# <a name="iot-hub-operations-monitoring"></a>Az IoT-központ műveletek figyelése

IoT Hub műveletek figyelését teszi lehetővé toomonitor hello állapota az IoT hub valós idejű műveletek. Az IoT-központ között számos modulkategória közül műveletek nyomon követi az események. Dönthet úgy is, az események küldhetők a egy vagy több kategóriák tooan végpontját az IoT hub feldolgozásra. Hello adatok hibák figyelése, vagy adatokat minták alapján összetettebb feldolgozás beállítása.

Az IoT-központ figyeli az események hat kategóriák:

* Eszköz identitása műveletek
* Telemetriát
* Felhő-eszközre küldött üzenetek
* Kapcsolatok
* Fájlfeltöltések
* Üzenet-útválasztás

## <a name="how-tooenable-operations-monitoring"></a>Hogyan tooenable műveletek figyelése

1. Létrehoz egy IoT-központot. Hogyan találhat útmutatót az IoT-központ a hello toocreate [Ismerkedés] [ lnk-get-started] útmutató.

1. Az IoT hub hello panel megnyitásához. Itt kattintson **figyelési műveletek**.

    ![Figyelési konfiguráció hello portálon hozzáférési műveleteket][1]

1. Jelölje be hello kategóriák toomonitor kívánja, és kattintson a figyelés **mentése**. hello események állnak rendelkezésre a felsorolt hello Event Hub-kompatibilis végpont olvasását **figyelési beállítások**. hello IoT-központ végpontjának neve `messages/operationsmonitoringevents`.

    ![Az IoT hub a figyelési műveletek konfigurálása][2]

> [!NOTE]
> Kiválasztása **részletes** hello figyelésének **kapcsolatok** kategória hatására az IoT-központ toogenerate további diagnosztikai üzeneteket. Minden más kategóriák hello **részletes** módosítások adatokat az IoT-központ mennyisége hello beállítása minden hibaüzenet tartalmazza.

## <a name="event-categories-and-how-toouse-them"></a>Kategóriák, és hogyan toouse őket

Minden egyes kategória nyomon követi a figyelési műveletek IoT-központot, és minden felügyeleti kategória interakcióba más típusú rendelkezik, amely meghatározza, milyen események kategória felépítése séma.

### <a name="device-identity-operations"></a>Eszköz identitása műveletek

hello eszközkategóriát identitás műveletek nyomon követi az toocreate tett kísérlet során előforduló hibákat, frissíteni vagy törölni az IoT hub identitásjegyzékhez egy bejegyzést. Ebbe a kategóriába követési akkor hasznos, forgatókönyvek történő üzembe helyezéséhez.

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

### <a name="device-telemetry"></a>Telemetriát

hello telemetriai eszközkategóriát hello IoT-központ halasztja és a kapcsolódó toohello telemetriai csővezeték hibákról követi nyomon. Ebbe a kategóriába olyan hibák telemetriai események (például a szabályozás) küldésekor és fogadásakor telemetriai események (például a jogosulatlan olvasó). Ez a kategória nem dolgozza fel a futó hello eszköz által okozott hibákat.

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

### <a name="cloud-to-device-commands"></a>Felhő eszközparancsok

hello felhő eszközparancsok kategória nyomon követi a hibákat, az IoT-központ hello halasztja és a kapcsolódó toohello felhő-eszközre küldött üzenetek feldolgozási sor. Ebbe a kategóriába tartoznak (például a jogosulatlan feladótól) felhő eszközre üzenetküldésre, (például kézbesítési száma túllépve) felhő eszközre üzenetek fogadására, és a felhő-eszközre küldött üzenetek visszajelzés fogadása (például a visszajelzés lejárt) előforduló hibákat. Ez a kategória nem dolgozza fel a hibák egy eszközről, amely nem megfelelően kezeli a felhő eszközre üzenetet, ha felhő eszközre üdvözlőüzenetére lett sikeresen kézbesítve.

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

### <a name="connections"></a>Kapcsolatok

hello kapcsolatok kategória nyomon követi az eszközök csatlakozni, vagy válassza le az IoT-központ előforduló hibákat. Ebbe a kategóriába követési akkor hasznos, jogosulatlan kapcsolódási kísérletek azonosítására és nyomon követése, ha a kapcsolat azért területein gyenge hálózati eszközökhöz.

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

### <a name="file-uploads"></a>Fájlfeltöltések

hello fájl feltöltése kategória hello IoT-központ halasztja és a kapcsolódó toofile feltöltés funkció hibákról követi nyomon. Ez a kategória tartalmazza:

* Hello SAS URI-t, például amikor egy eszköz, a feltöltött hello hub értesíti előtt lejár az előforduló hibákat.
* Nem sikerült hello eszköz által jelzett feltöltések.
* Ha egy fájl nem található a tároló az IoT-központ értesítési üzenet létrehozása során előforduló hibákat.

Ez a kategória nem dolgozza fel a jelentkező hibák közvetlenül hello eszköz egy fájl toostorage tölti fel.

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

### <a name="message-routing"></a>Üzenet-útválasztás

hello üzenet útválasztási kategória nyomon követi az üzenet útvonal értékelési és végpont-állapotot az IoT-központ által érzékelt során előforduló hibákat. Ebbe a kategóriába olyan események, például amikor egy szabály akkor túl "nem definiált", ha az IoT-központ állapotúként jelöli meg a végpont kézbesítetlen, és bármely más hibák végpont. Ebbe a kategóriába nem tartalmaz bizonyos hibákat köszönőüzenetei maguk (például a szabályozás hibák eszköz), a hello "telemetriát" kategória jelentett kapcsolatban.

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

## <a name="view-events"></a>események megtekintése

Használhatja a hello *IOT hubbal-explorer* eszköz tooquickly tesztelje, hogy az IoT hub előállító figyelési esemény. tooinstall hello eszköz, lásd: hello hello utasításait [IOT hubbal-explorer] [ lnk-iothub-explorer] GitHub-tárházban.

1. Győződjön meg arról, hogy hello **kapcsolatok** figyelő kategória értéke túl**részletes** hello portálon.

1. Parancsot egy parancssorba futtassa a parancsot tooread követően – figyelési végpontját hello hello:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Egy másik-parancssorban futtassa a következő parancs toosimulate hello eszközről a felhőbe üzeneteket küldő eszköz:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. hello szimulált eszköz csatlakozik az IoT-központ tooyour hello első parancssori látható hello figyelési esemény.

## <a name="connect-toohello-monitoring-endpoint"></a>Figyelési végpontját toohello csatlakozás

az IoT hub-végpont figyelési hello Event Hub-kompatibilis végpont. Bármely mechanizmus, amely az Event Hubs tooread figyelési üzenetek ehhez a végponthoz is használhatja. hello alábbi minta létrehoz egy alapszintű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés. Hogyan tooprocess Eseményközpontokból származó üzenetek kapcsolatos további információkért lásd: hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag.

tooconnect toohello figyelési végpont kell egy kapcsolati karakterláncot és a hello a végpont neve. hello lépések bemutatják, hogyan toofind hello hello portálon szükséges értékeket:

1. Hello portálon lépjen az IoT-központ erőforráspanelen tooyour.

1. Válasszon **figyelési műveletek**, és jegyezze fel a hello **Event Hub-kompatibilis neve** és **Event Hub-kompatibilis végpont** értékeket:

    ![Event Hub-kompatibilis végpont értékek][img-endpoints]

1. Válasszon **megosztott elérési házirendek**, majd válassza a **szolgáltatás**. Jegyezze fel a hello **elsődleges kulcs** érték:

    ![Szolgáltatás megosztott elérési házirend elsődleges kulcs][img-service-key]

hello alábbi C# kódminta származik a Visual Studio **klasszikus Windows asztal** C# konzolalkalmazást. hello projekt már hello **WindowsAzure.ServiceBus** NuGet-csomagja telepítve.

* Hello kapcsolati karakterlánc helyőrzőjét cserélje le a kapcsolati karakterlánc által használt hello **Event Hub-kompatibilis végpont** és szolgáltatás **elsődleges kulcs** látható módon hello következő korábban feljegyzett értékek Példa:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Cserélje le a végpont neve helyőrzőt hello figyelési hello **Event Hub-kompatibilis neve** korábban feljegyzett érték.

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

## <a name="next-steps"></a>Következő lépések
toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

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