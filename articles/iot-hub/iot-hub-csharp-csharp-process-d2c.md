---
title: "aaaProcess Azure IoT Hub eszközről a felhőbe üzenetek útvonalak (.Net) |} Microsoft Docs"
description: "Hogyan tooprocess IoT-központ eszközről a felhőbe üzenetek útválasztási szabályokat és egyéni végpontokat toodispatch üzenetek tooother háttér-szolgáltatásaihoz."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: c1dd5be04ca30c65af2be466ba6c8c1858333154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="e28c1-103">Az IoT-központ eszközről a felhőbe üzenetek útvonalak (.NET) használatával</span><span class="sxs-lookup"><span data-stu-id="e28c1-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="e28c1-104">Ez az oktatóanyag épít, hello [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e28c1-104">This tutorial builds on hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="e28c1-105">hello oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="e28c1-105">hello tutorial:</span></span>

* <span data-ttu-id="e28c1-106">Bemutatja, hogyan toouse útválasztási szabályok toodispatch eszközről a felhőbe üzenetek könnyen és konfigurációs-alapú.</span><span class="sxs-lookup"><span data-stu-id="e28c1-106">Shows you how toouse routing rules toodispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="e28c1-107">Bemutatja, hogyan tooisolate interaktív üzenetek hello megoldásból azonnali beavatkozást igénylő háttér további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="e28c1-107">Illustrates how tooisolate interactive messages that require immediate action from hello solution back end for further processing.</span></span> <span data-ttu-id="e28c1-108">Például egy eszköz előfordulhat, hogy küldése riasztás, amely elindítja a jegy beszúrása egy CRM-rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="e28c1-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="e28c1-109">Adatpont üzenetek, például hőmérséklet telemetriai ellentétben be egy analytics hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="e28c1-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="e28c1-110">Ez az oktatóanyag végén hello három .NET konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="e28c1-110">At hello end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="e28c1-111">**SimulatedDevice**, hello alkalmazás hello létrehozott egy módosított verziója [Ismerkedés az IoT-központ] oktatóanyag adatpont eszközről a felhőbe üzeneteket küld másodpercenként, és interaktív eszközre felhő minden 10 üzenetek másodperc.</span><span class="sxs-lookup"><span data-stu-id="e28c1-111">**SimulatedDevice**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="e28c1-112">**ReadDeviceToCloudMessages** , hogy a jeleníti meg az eszköz alkalmazás által küldött nem kritikus telemetriai hello.</span><span class="sxs-lookup"><span data-stu-id="e28c1-112">**ReadDeviceToCloudMessages** that displays hello non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="e28c1-113">**ReadCriticalQueue** távolít el hello kritikus által küldött üzenetek az eszköz alkalmazás egy Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="e28c1-113">**ReadCriticalQueue** de-queues hello critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="e28c1-114">A várólista nincs csatolt toohello IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e28c1-114">This queue is attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="e28c1-115">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek, beleértve a C, Java és JavaScript SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="e28c1-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="e28c1-116">toolearn hogyan tooreplace ebben az oktatóanyagban szimulált eszköz hello fizikai eszköz, tekintse meg a hello [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="e28c1-116">toolearn how tooreplace hello simulated device in this tutorial with a physical device, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="e28c1-117">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e28c1-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="e28c1-118">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e28c1-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="e28c1-119">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="e28c1-119">An active Azure account.</span></span> <br/><span data-ttu-id="e28c1-120">Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="e28c1-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="e28c1-121">Néhány alapvető ismerete szükséges [Azure Storage] és [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="e28c1-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="e28c1-122">Interaktív üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="e28c1-122">Send interactive messages</span></span>

<span data-ttu-id="e28c1-123">Hello létrehozott hello eszközalkalmazás módosítása [Ismerkedés az IoT-központ] oktatóanyag toooccasionally interaktív üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="e28c1-123">Modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send interactive messages.</span></span>

<span data-ttu-id="e28c1-124">A Visual Studio, a hello **SimulatedDevice** projektre, cserélje le a hello `SendDeviceToCloudMessagesAsync` hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="e28c1-124">In Visual Studio, in hello **SimulatedDevice** project, replace hello `SendDeviceToCloudMessagesAsync` method with hello following code:</span></span>

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

<span data-ttu-id="e28c1-125">Ez a metódus véletlenszerűen ad hello tulajdonság `"level": "critical"` hello eszközét, ezzel szimulálja egy üzenetet, amely azonnali beavatkozást igényel a háttérkiszolgálók megoldás hello által küldött toomessages.</span><span class="sxs-lookup"><span data-stu-id="e28c1-125">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello solution back-end.</span></span> <span data-ttu-id="e28c1-126">hello eszközalkalmazás információt átadja a hello üzenet tulajdonságai, ahelyett, hogy hello üzenet törzsében, úgy, hogy az IoT-központ megfelelő üzenet toohello hello üzenetcél irányíthatja.</span><span class="sxs-lookup"><span data-stu-id="e28c1-126">hello device app passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="e28c1-127">Üzenet tulajdonságai tooroute üzenetek feldolgozása, Ezenfelül itt látható példa az toohello közbeni útvonalra cold-elérési utat is több, különböző esetekre is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e28c1-127">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="e28c1-128">Hello szakét az egyszerűség Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="e28c1-128">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="e28c1-129">Az éles kódban, meg kell valósítania egy újrapróbálkozási házirendje, például az exponenciális leállítási hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="e28c1-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a><span data-ttu-id="e28c1-130">Az IoT hub útvonal üzenetek tooa sorból</span><span class="sxs-lookup"><span data-stu-id="e28c1-130">Route messages tooa queue in your IoT hub</span></span>

<span data-ttu-id="e28c1-131">Ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="e28c1-131">In this section, you:</span></span>

* <span data-ttu-id="e28c1-132">Hozzon létre egy Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="e28c1-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="e28c1-133">Az IoT-központ tooyour csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="e28c1-133">Connect it tooyour IoT hub.</span></span>
* <span data-ttu-id="e28c1-134">Konfigurálja az IoT hub toosend üzenetek toohello várólista hello jelenléte ügyfélkulcscsere-tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="e28c1-134">Configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span>

<span data-ttu-id="e28c1-135">Hogyan tooprocess a Service Bus-üzenetsorok érkező üzenetek kapcsolatos további információkért lásd: [Ismerkedés a várólisták][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="e28c1-135">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="e28c1-136">Hozzon létre egy Service Bus-üzenetsorba, a [Ismerkedés a várólisták][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="e28c1-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="e28c1-137">hello várólista hello kell lennie ugyanazon előfizetésével és régiójával, az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e28c1-137">hello queue must be in hello same subscription and region as your IoT hub.</span></span> <span data-ttu-id="e28c1-138">Jegyezze fel a hello névtér és a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="e28c1-138">Make a note of hello namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e28c1-139">Service Bus-üzenetsorok és témakörök használatos az IoT-központok végpontjai nem lehet **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e28c1-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="e28c1-140">Ha ezek a lehetőségek valamelyikét engedélyezve vannak, hello végpont megjelenik **Unreachable** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e28c1-140">If either of those options are enabled, hello endpoint appears as **Unreachable** in hello Azure portal.</span></span>

2. <span data-ttu-id="e28c1-141">A hello Azure-portálon, nyissa meg az IoT hub, és kattintson a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="e28c1-141">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Az IoT-központ végpontok][30]

3. <span data-ttu-id="e28c1-143">A hello **végpontok** panelen kattintson a **hozzáadása** : hello felső tooadd a várólista tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e28c1-143">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="e28c1-144">Név hello végpont **CriticalQueue** és hello legördülő listákat tooselect **Service Bus-üzenetsorba**, Service Bus-névtér, amelyben a várólista található hello és hello a várólista neve.</span><span class="sxs-lookup"><span data-stu-id="e28c1-144">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="e28c1-145">Amikor elkészült, kattintson a **mentése** hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e28c1-145">When you are done, click **Save** at hello bottom.</span></span>
    
    ![A végpont hozzáadása][31]
    
4. <span data-ttu-id="e28c1-147">Most kattintson **útvonalak** az IoT Hub a.</span><span class="sxs-lookup"><span data-stu-id="e28c1-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="e28c1-148">Kattintson a **Hozzáadás** hello panel toocreate toohello csak várólistára meg üzeneteit útválasztási szabályainak hozzáadott hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e28c1-148">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="e28c1-149">Válassza ki **DeviceTelemetry** hello adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="e28c1-149">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="e28c1-150">Adja meg `level="critical"` hello feltételként, és válassza ki az előzőekben adott hozzá, útválasztási szabály végpont hello egyéni végpontként hello várólista.</span><span class="sxs-lookup"><span data-stu-id="e28c1-150">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="e28c1-151">Amikor elkészült, kattintson a **mentése** hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e28c1-151">When you are done, click **Save** at hello bottom.</span></span>
    
    ![Egy útvonal hozzáadása][32]
    
    <span data-ttu-id="e28c1-153">Ellenőrizze, hogy hello tartalék útvonal túl van-e állítva**ON**.</span><span class="sxs-lookup"><span data-stu-id="e28c1-153">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="e28c1-154">Ezt az értéket az IoT-központ hello alapértelmezett konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="e28c1-154">This value is hello default configuration for an IoT hub.</span></span>
    
    ![Tartalék útvonal][33]

## <a name="read-from-hello-queue-endpoint"></a><span data-ttu-id="e28c1-156">Hello várólista végpont olvasásakor</span><span class="sxs-lookup"><span data-stu-id="e28c1-156">Read from hello queue endpoint</span></span>

<span data-ttu-id="e28c1-157">Ebben a szakaszban hello várólista végpont köszönőüzenetei olvasni azt.</span><span class="sxs-lookup"><span data-stu-id="e28c1-157">In this section, you read hello messages from hello queue endpoint.</span></span>

1. <span data-ttu-id="e28c1-158">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="e28c1-158">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="e28c1-159">Név hello projekt **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="e28c1-159">Name hello project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="e28c1-160">A Megoldáskezelőben kattintson a jobb gombbal hello **ReadCriticalQueue** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="e28c1-160">In Solution Explorer, right-click hello **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="e28c1-161">Ez a művelet hatására megjelenik a hello **NuGet-Csomagkezelő** ablak.</span><span class="sxs-lookup"><span data-stu-id="e28c1-161">This operation displays hello **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="e28c1-162">Keresse meg **WindowsAzure.ServiceBus**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="e28c1-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="e28c1-163">Ez a művelet tölti le, telepíti, és hozzáad egy hivatkozást toohello Azure Service Bus elemet minden függőségével.</span><span class="sxs-lookup"><span data-stu-id="e28c1-163">This operation downloads, installs, and adds a reference toohello Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="e28c1-164">Adja hozzá a következő hello **használatával** hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="e28c1-164">Add hello following **using** statements at hello top of hello **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="e28c1-165">Végül adja hozzá a következő sorokat toohello hello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="e28c1-165">Finally, add hello following lines toohello **Main** method.</span></span> <span data-ttu-id="e28c1-166">Helyettesítse be a kapcsolati karakterlánc hello **figyelésére** hello várólista engedélyek:</span><span class="sxs-lookup"><span data-stu-id="e28c1-166">Substitute hello connection string with **Listen** permissions for hello queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C tooexit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="e28c1-167">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="e28c1-167">Run hello applications</span></span>
<span data-ttu-id="e28c1-168">Most már készen áll a toorun hello alkalmazások áll.</span><span class="sxs-lookup"><span data-stu-id="e28c1-168">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="e28c1-169">A Visual Studio, a Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza ki **indítási projektek beállítása**.</span><span class="sxs-lookup"><span data-stu-id="e28c1-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="e28c1-170">Válassza ki **több kezdőprojekt**, majd jelölje be **Start** hello műveletként a hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, és **ReadCriticalQueue** projektek.</span><span class="sxs-lookup"><span data-stu-id="e28c1-170">Select **Multiple startup projects**, then select **Start** as hello action for hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="e28c1-171">Nyomja le az **F5** toostart hello három konzol alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e28c1-171">Press **F5** toostart hello three console apps.</span></span> <span data-ttu-id="e28c1-172">Hello **ReadDeviceToCloudMessages** az alkalmazás rendelkezik hello küldött üzenetek csak nem kritikus **SimulatedDevice** az alkalmazás és hello **ReadCriticalQueue** az alkalmazás csak rendelkezik kritikus üzeneteihez.</span><span class="sxs-lookup"><span data-stu-id="e28c1-172">hello **ReadDeviceToCloudMessages** app has only non-critical messages sent from hello **SimulatedDevice** application, and hello **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Három konzol alkalmazás][50]

## <a name="next-steps"></a><span data-ttu-id="e28c1-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e28c1-174">Next steps</span></span>
<span data-ttu-id="e28c1-175">Ebben az oktatóanyagban megtanulta, hogyan tooreliably mennyi funkcióval hello üzenet útválasztási IoT hub eszköz a felhőbe küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e28c1-175">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="e28c1-176">Hello [hogyan toosend felhőből eszközre küldött üzenetek IoT-központ] [ lnk-c2d] bemutatja, hogyan toosend üzenetek tooyour eszközöket a megoldás háttérből.</span><span class="sxs-lookup"><span data-stu-id="e28c1-176">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="e28c1-177">teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="e28c1-177">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="e28c1-178">További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="e28c1-178">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="e28c1-179">toolearn üzenetet az IoT hubon útválasztási kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="e28c1-179">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[Ismerkedés az IoT-központ]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT fejlesztői központ]: https://azure.microsoft.com/develop/iot
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
