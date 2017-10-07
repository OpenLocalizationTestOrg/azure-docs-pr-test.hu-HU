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
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a><span data-ttu-id="0befc-104">Csatlakozás az eszköz tooyour IoT hub .NET használatával</span><span class="sxs-lookup"><span data-stu-id="0befc-104">Connect your device tooyour IoT hub using .NET</span></span>

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="0befc-105">Ez az oktatóanyag végén hello három .NET konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="0befc-105">At hello end of this tutorial, you have three .NET console apps:</span></span>

* <span data-ttu-id="0befc-106">**CreateDeviceIdentity**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect az eszközalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0befc-106">**CreateDeviceIdentity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="0befc-107">**ReadDeviceToCloudMessages**, amely megjeleníti az eszköz alkalmazás által küldött hello telemetriai.</span><span class="sxs-lookup"><span data-stu-id="0befc-107">**ReadDeviceToCloudMessages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="0befc-108">**SimulatedDevice**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és üzenetet küld a telemetriai adatok másodpercenként hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="0befc-108">**SimulatedDevice**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second by using hello MQTT protocol.</span></span>

<span data-ttu-id="0befc-109">Töltse le, vagy a klónozáshoz hello három alkalmazás a Githubról tartalmazó hello Visual Studio megoldás.</span><span class="sxs-lookup"><span data-stu-id="0befc-109">You can download or clone hello Visual Studio solution, which contains hello three apps from Github.</span></span>

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> <span data-ttu-id="0befc-110">Hello Azure IoT SDK-k toobuild használt alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez, lásd: [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="0befc-110">For information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices, and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="0befc-111">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0befc-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="0befc-112">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0befc-112">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="0befc-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="0befc-113">An active Azure account.</span></span> <span data-ttu-id="0befc-114">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="0befc-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="0befc-115">Az IoT hub most hozott létre, és hello állomásnév és az IoT-központ kapcsolati karakterláncot, hogy kell-e toocomplete hello rest oktatóanyag rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0befc-115">You have now created your IoT hub, and you have hello host name and IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="0befc-116">Az eszközről a felhőbe irányuló üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="0befc-116">Receive device-to-cloud messages</span></span>
<span data-ttu-id="0befc-117">Ebben a szakaszban egy .NET-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról.</span><span class="sxs-lookup"><span data-stu-id="0befc-117">In this section, you create a .NET console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="0befc-118">Az IoT-központ közzétesz egy [Azure Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg.</span><span class="sxs-lookup"><span data-stu-id="0befc-118">An IoT hub exposes an [Azure Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="0befc-119">egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0befc-119">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="0befc-120">Hogyan tooprocess eszköz-felhő üzenetek léptékű toolearn lásd: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="0befc-120">toolearn how tooprocess device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span> <span data-ttu-id="0befc-121">Hogyan tooprocess Eseményközpontokból származó üzenetek kapcsolatos további információkért lásd: hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="0befc-121">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span> <span data-ttu-id="0befc-122">(Ez az oktatóanyag az alkalmazandó toohello IoT Hub Event Hub-kompatibilis végpontok.)</span><span class="sxs-lookup"><span data-stu-id="0befc-122">(This tutorial is applicable toohello IoT Hub Event Hub-compatible endpoints.)</span></span>

> [!NOTE]
> <span data-ttu-id="0befc-123">hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="0befc-123">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="0befc-124">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="0befc-124">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="0befc-125">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="0befc-125">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="0befc-126">Név hello projekt **ReadDeviceToCloudMessages**.</span><span class="sxs-lookup"><span data-stu-id="0befc-126">Name hello project **ReadDeviceToCloudMessages**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10a]

2. <span data-ttu-id="0befc-128">A Megoldáskezelőben kattintson a jobb gombbal hello **ReadDeviceToCloudMessages** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="0befc-128">In Solution Explorer, right-click hello **ReadDeviceToCloudMessages** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="0befc-129">A hello **NuGet-Csomagkezelő** ablakban, keresse meg **WindowsAzure.ServiceBus**, jelölje be **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="0befc-129">In hello **NuGet Package Manager** window, search for **WindowsAzure.ServiceBus**, select **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="0befc-130">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást túl[Azure Service Bus][lnk-servicebus-nuget], elemet minden függőségével.</span><span class="sxs-lookup"><span data-stu-id="0befc-130">This procedure downloads, installs, and adds a reference too[Azure Service Bus][lnk-servicebus-nuget], with all its dependencies.</span></span> <span data-ttu-id="0befc-131">Ez a csomag lehetővé teszi, hogy a hello alkalmazás tooconnect toohello Event Hub-kompatibilis végpont az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0befc-131">This package enables hello application tooconnect toohello Event Hub-compatible endpoint on your IoT hub.</span></span>

4. <span data-ttu-id="0befc-132">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="0befc-132">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. <span data-ttu-id="0befc-133">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="0befc-133">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="0befc-134">Hello helyőrző értékét lecserélheti egy hello hello "Az IoT-központ létrehozása" szakaszban létrehozott hello hub IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0befc-134">Replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello "Create an IoT hub" section.</span></span>

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. <span data-ttu-id="0befc-135">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="0befc-135">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="0befc-136">Ez a módszer egy **EventHubReceiver** származó összes hello IoT hub eszköz-felhő példány tooreceive üzenetek fogadására partíciók.</span><span class="sxs-lookup"><span data-stu-id="0befc-136">This method uses an **EventHubReceiver** instance tooreceive messages from all hello IoT hub device-to-cloud receive partitions.</span></span> <span data-ttu-id="0befc-137">Figyelje meg, hogyan továbbítsa a `DateTime.Now` paraméter hello létrehozásakor **EventHubReceiver** objektumot, hogy miután elindult küldött üzenetek csak kap.</span><span class="sxs-lookup"><span data-stu-id="0befc-137">Notice how you pass a `DateTime.Now` parameter when you create hello **EventHubReceiver** object, so that it only receives messages sent after it starts.</span></span> <span data-ttu-id="0befc-138">Ez a szűrő akkor hasznos, egy tesztkörnyezetben, így hello aktuális készletében lévő üzenetek látható.</span><span class="sxs-lookup"><span data-stu-id="0befc-138">This filter is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="0befc-139">Éles környezetben a kód győződjön meg arról, hogy az összes köszönőüzenetei feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="0befc-139">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="0befc-140">További információkért lásd: hello oktatóanyag [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="0befc-140">For more information, see hello tutorial [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial].</span></span>

7. <span data-ttu-id="0befc-141">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="0befc-141">Finally, add hello following lines toohello **Main** method:</span></span>

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

## <a name="create-a-device-app"></a><span data-ttu-id="0befc-142">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0befc-142">Create a device app</span></span>

<span data-ttu-id="0befc-143">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0befc-143">In this section, you create a .NET console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="0befc-144">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="0befc-144">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="0befc-145">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="0befc-145">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="0befc-146">Név hello projekt **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="0befc-146">Name hello project **SimulatedDevice**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10b]

2. <span data-ttu-id="0befc-148">A Megoldáskezelőben kattintson a jobb gombbal hello **SimulatedDevice** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="0befc-148">In Solution Explorer, right-click hello **SimulatedDevice** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="0befc-149">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **Microsoft.Azure.Devices.Client**, jelölje be **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="0befc-149">In hello **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Client**, select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="0befc-150">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK NuGet-csomag] [ lnk-device-nuget] és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="0befc-150">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK NuGet package][lnk-device-nuget] and its dependencies.</span></span>

4. <span data-ttu-id="0befc-151">Adja hozzá a következő hello `using` hello hello tetején utasítás **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="0befc-151">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. <span data-ttu-id="0befc-152">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="0befc-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="0befc-153">Helyettesítő `{iot hub hostname}` hello IoT hub állomásnévvel lekért hello "IoT hub létrehozása" szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0befc-153">Substitute `{iot hub hostname}` with hello IoT hub host name you retrieved in hello "Create an IoT hub" section.</span></span> <span data-ttu-id="0befc-154">Helyettesítő `{device key}` hello eszköz kulccsal lekért hello "Egy eszközidentitás létrehozása" szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0befc-154">Substitute `{device key}` with hello device key you retrieved in hello "Create a device identity" section.</span></span>

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. <span data-ttu-id="0befc-155">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="0befc-155">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="0befc-156">Ez a metódus másodpercenként elküld egy új, az eszközről a felhőbe irányuló üzenetet.</span><span class="sxs-lookup"><span data-stu-id="0befc-156">This method sends a new device-to-cloud message every second.</span></span> <span data-ttu-id="0befc-157">üdvözlőüzenetére JSON-szerializált objektum hello eszközazonosítójú tartalmazza, és véletlenszerűen generált számok toosimulate hőmérséklet-érzékelő és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="0befc-157">hello message contains a JSON-serialized object, with hello device ID and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

7. <span data-ttu-id="0befc-158">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="0befc-158">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    <span data-ttu-id="0befc-159">Alapértelmezés szerint hello **létrehozása** metódus a .NET-keretrendszer alkalmazás létrehoz egy **DeviceClient** hello AMQP protokoll toocommunicate használ, az IoT Hub-példány.</span><span class="sxs-lookup"><span data-stu-id="0befc-159">By default, hello **Create** method in a .NET Framework app creates a **DeviceClient** instance that uses hello AMQP protocol toocommunicate with IoT Hub.</span></span> <span data-ttu-id="0befc-160">toouse hello MQTT vagy a HTTP protokollhoz, használjon hello hello felülbírálásának **létrehozása** módszer, amely lehetővé teszi a toospecify hello protokoll.</span><span class="sxs-lookup"><span data-stu-id="0befc-160">toouse hello MQTT or HTTP protocol, use hello override of hello **Create** method that enables you toospecify hello protocol.</span></span> <span data-ttu-id="0befc-161">UWP és PCL ügyfelek alapértelmezés szerint hello HTTP protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="0befc-161">UWP and PCL clients use hello HTTP protocol by default.</span></span> <span data-ttu-id="0befc-162">Ha hello HTTP protokollt használ, hello is fel kell **Microsoft.AspNet.WebApi.Client** NuGet csomag tooyour projekt tooinclude hello **System.Net.Http.Formatting** névtér.</span><span class="sxs-lookup"><span data-stu-id="0befc-162">If you use hello HTTP protocol, you should also add hello **Microsoft.AspNet.WebApi.Client** NuGet package tooyour project tooinclude hello **System.Net.Http.Formatting** namespace.</span></span>

<span data-ttu-id="0befc-163">Ez az oktatóanyag végigvezeti hello lépéseket toocreate az IoT-központ eszközalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0befc-163">This tutorial takes you through hello steps toocreate an IoT Hub device app.</span></span> <span data-ttu-id="0befc-164">Is használhatja a hello [Azure IoT-központ szolgáltatáshoz kapcsolódó] [ lnk-connected-service] Visual Studio bővítmény tooadd hello szükséges kód tooyour eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0befc-164">You can also use hello [Connected Service for Azure IoT Hub][lnk-connected-service] Visual Studio extension tooadd hello necessary code tooyour device app.</span></span>

> [!NOTE]
> <span data-ttu-id="0befc-165">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="0befc-165">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0befc-166">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="0befc-166">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="0befc-167">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="0befc-167">Run hello apps</span></span>

<span data-ttu-id="0befc-168">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="0befc-168">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="0befc-169">A Visual Studióban a Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal a megoldásra, majd kattintson a **Set StartUp projects** (Indítási projektek beállítása) parancsra.</span><span class="sxs-lookup"><span data-stu-id="0befc-169">In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**.</span></span> <span data-ttu-id="0befc-170">Válassza ki **több kezdőprojekt**, majd válassza ki **Start** hello műveletként a mindkét hello **ReadDeviceToCloudMessages** és **SimulatedDevice** projektek.</span><span class="sxs-lookup"><span data-stu-id="0befc-170">Select **Multiple startup projects**, and then select **Start** as hello action for both hello **ReadDeviceToCloudMessages** and **SimulatedDevice** projects.</span></span>

    ![Indítási projektek tulajdonságai][41]

2. <span data-ttu-id="0befc-172">Nyomja le az **F5** toostart mindkét futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0befc-172">Press **F5** toostart both apps running.</span></span> <span data-ttu-id="0befc-173">hello a konzol kimeneti hello **SimulatedDevice** alkalmazás látható köszönőüzenetei az eszközalkalmazás tooyour IoT-központ küld.</span><span class="sxs-lookup"><span data-stu-id="0befc-173">hello console output from hello **SimulatedDevice** app shows hello messages your device app sends tooyour IoT hub.</span></span> <span data-ttu-id="0befc-174">hello a konzol kimeneti hello **ReadDeviceToCloudMessages** alkalmazás, amely megkapja az IoT hub köszönőüzenetei jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0befc-174">hello console output from hello **ReadDeviceToCloudMessages** app shows hello messages that your IoT hub receives.</span></span>

    ![Az alkalmazások konzolkimenetei][42]

3. <span data-ttu-id="0befc-176">Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:</span><span class="sxs-lookup"><span data-stu-id="0befc-176">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Az Azure Portal Használat csempéje][43]

## <a name="next-steps"></a><span data-ttu-id="0befc-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0befc-178">Next steps</span></span>

<span data-ttu-id="0befc-179">Ebben az oktatóanyagban hello Azure-portálon az IoT-központ konfigurálva, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="0befc-179">In this tutorial, you configured an IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="0befc-180">Az eszköz identitása tooenable hello eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta.</span><span class="sxs-lookup"><span data-stu-id="0befc-180">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="0befc-181">Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="0befc-181">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="0befc-182">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="0befc-182">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="0befc-183">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="0befc-183">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="0befc-184">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="0befc-184">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="0befc-185">[Ismerkedés az IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="0befc-185">[Getting started with IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="0befc-186">toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="0befc-186">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

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
