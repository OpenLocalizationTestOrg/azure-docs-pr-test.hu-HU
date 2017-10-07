---
title: "aaaGet lépések Azure IoT Hub (.NET) |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend eszköz-felhő üzenetek tooAzure IoT Hub IoT SDK-k használata a .NET-hez. Szimulált eszköz és a szolgáltatás alkalmazások tooregister létrehozása az eszköz, üzenetküldés és üzenetek olvasni az IoT-központ."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56cf14687411898ea0fa4ebb1782e18b3930809c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a>Csatlakozás az eszköz tooyour IoT hub .NET használatával

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén hello három .NET konzol alkalmazások közül választhat:

* **CreateDeviceIdentity**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect az eszközalkalmazás.
* **ReadDeviceToCloudMessages**, amely megjeleníti az eszköz alkalmazás által küldött hello telemetriai.
* **SimulatedDevice**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és üzenetet küld a telemetriai adatok másodpercenként hello MQTT protokoll használatával.

Töltse le, vagy a klónozáshoz hello három alkalmazás a Githubról tartalmazó hello Visual Studio megoldás.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Hello Azure IoT SDK-k toobuild használt alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez, lásd: [Azure IoT SDK-k][lnk-hub-sdks].

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Az IoT hub most hozott létre, és hello állomásnév és az IoT-központ kapcsolati karakterláncot, hogy kell-e toocomplete hello rest oktatóanyag rendelkezik.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Az eszközről a felhőbe irányuló üzenetek fogadása
Ebben a szakaszban egy .NET-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról. Az IoT-központ közzétesz egy [Azure Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg. egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre. Hogyan tooprocess eszköz-felhő üzenetek léptékű toolearn lásd: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag. Hogyan tooprocess Eseményközpontokból származó üzenetek kapcsolatos további információkért lásd: hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag. (Ez az oktatóanyag az alkalmazandó toohello IoT Hub Event Hub-kompatibilis végpontok.)

> [!NOTE]
> hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **ReadDeviceToCloudMessages**.

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10a]

2. A Megoldáskezelőben kattintson a jobb gombbal hello **ReadDeviceToCloudMessages** projektre, és kattintson a **NuGet-csomagok kezelése**.

3. A hello **NuGet-Csomagkezelő** ablakban, keresse meg **WindowsAzure.ServiceBus**, jelölje be **telepítése**, és el kell fogadnia a használati feltételek hello. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást túl[Azure Service Bus][lnk-servicebus-nuget], elemet minden függőségével. Ez a csomag lehetővé teszi, hogy a hello alkalmazás tooconnect toohello Event Hub-kompatibilis végpont az IoT hub.

4. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello hello "Az IoT-központ létrehozása" szakaszban létrehozott hello hub IoT-központ kapcsolati karakterláncot.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    Ez a módszer egy **EventHubReceiver** származó összes hello IoT hub eszköz-felhő példány tooreceive üzenetek fogadására partíciók. Figyelje meg, hogyan továbbítsa a `DateTime.Now` paraméter hello létrehozásakor **EventHubReceiver** objektumot, hogy miután elindult küldött üzenetek csak kap. Ez a szűrő akkor hasznos, egy tesztkörnyezetben, így hello aktuális készletében lévő üzenetek látható. Éles környezetben a kód győződjön meg arról, hogy az összes köszönőüzenetei feldolgozza. További információkért lásd: hello oktatóanyag [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket][lnk-process-d2c-tutorial].

7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C tooexit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása

Ebben a szakaszban egy .NET-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld hoz létre.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **SimulatedDevice**.

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10b]

2. A Megoldáskezelőben kattintson a jobb gombbal hello **SimulatedDevice** projektre, és kattintson a **NuGet-csomagok kezelése**.

3. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **Microsoft.Azure.Devices.Client**, jelölje be **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK NuGet-csomag] [ lnk-device-nuget] és annak függőségeit.

4. Adja hozzá a következő hello `using` hello hello tetején utasítás **Program.cs** fájlt:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Adja hozzá a következő mezők toohello hello **Program** osztály. Helyettesítő `{iot hub hostname}` hello IoT hub állomásnévvel lekért hello "IoT hub létrehozása" szakaszban. Helyettesítő `{device key}` hello eszköz kulccsal lekért hello "Egy eszközidentitás létrehozása" szakaszban.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Ez a metódus másodpercenként elküld egy új, az eszközről a felhőbe irányuló üzenetet. üdvözlőüzenetére JSON-szerializált objektum hello eszközazonosítójú tartalmazza, és véletlenszerűen generált számok toosimulate hőmérséklet-érzékelő és a páratartalom érzékelő.

7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Alapértelmezés szerint hello **létrehozása** metódus a .NET-keretrendszer alkalmazás létrehoz egy **DeviceClient** hello AMQP protokoll toocommunicate használ, az IoT Hub-példány. toouse hello MQTT vagy a HTTP protokollhoz, használjon hello hello felülbírálásának **létrehozása** módszer, amely lehetővé teszi a toospecify hello protokoll. UWP és PCL ügyfelek alapértelmezés szerint hello HTTP protokollt használja. Ha hello HTTP protokollt használ, hello is fel kell **Microsoft.AspNet.WebApi.Client** NuGet csomag tooyour projekt tooinclude hello **System.Net.Http.Formatting** névtér.

Ez az oktatóanyag végigvezeti hello lépéseket toocreate az IoT-központ eszközalkalmazás. Is használhatja a hello [Azure IoT-központ szolgáltatáshoz kapcsolódó] [ lnk-connected-service] Visual Studio bővítmény tooadd hello szükséges kód tooyour eszköz alkalmazás.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello alkalmazásokat.

1. A Visual Studióban a Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal a megoldásra, majd kattintson a **Set StartUp projects** (Indítási projektek beállítása) parancsra. Válassza ki **több kezdőprojekt**, majd válassza ki **Start** hello műveletként a mindkét hello **ReadDeviceToCloudMessages** és **SimulatedDevice** projektek.

    ![Indítási projektek tulajdonságai][41]

2. Nyomja le az **F5** toostart mindkét futó alkalmazások. hello a konzol kimeneti hello **SimulatedDevice** alkalmazás látható köszönőüzenetei az eszközalkalmazás tooyour IoT-központ küld. hello a konzol kimeneti hello **ReadDeviceToCloudMessages** alkalmazás, amely megkapja az IoT hub köszönőüzenetei jeleníti meg.

    ![Az alkalmazások konzolkimenetei][42]

3. Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:

    ![Az Azure Portal Használat csempéje][43]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello Azure-portálon az IoT-központ konfigurálva, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitása tooenable hello eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta. Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg.

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Kapcsolódás az eszközhöz][lnk-connect-device]
* [Eszközfelügyelet – első lépések][lnk-device-management]
* [Ismerkedés az IoT Edge szolgáltatással][lnk-iot-edge]

toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
