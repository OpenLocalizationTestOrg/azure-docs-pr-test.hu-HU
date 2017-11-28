---
title: "Azure IoT Hub eszközről a felhőbe üzenetek használatával útvonalak (.Net) |} Microsoft Docs"
description: "Az IoT-központ eszközről a felhőbe üzenetek feldolgozásához átirányítani más háttérszolgáltatások üzenetek útválasztási szabályokat és az egyéni végpontokat használatával hogyan."
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
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="1abc6-103">Az IoT-központ eszközről a felhőbe üzenetek útvonalak (.NET) használatával</span><span class="sxs-lookup"><span data-stu-id="1abc6-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="1abc6-104">Ez az oktatóanyag épít, a [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1abc6-104">This tutorial builds on the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="1abc6-105">Az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="1abc6-105">The tutorial:</span></span>

* <span data-ttu-id="1abc6-106">Bemutatja, hogyan könnyen és konfigurációalapú az eszközről a felhőbe üzenetek mennyi útválasztási szabályok használatával.</span><span class="sxs-lookup"><span data-stu-id="1abc6-106">Shows you how to use routing rules to dispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="1abc6-107">Bemutatja, hogyan interaktív üzenetek további feldolgozásra a megoldás háttérből azonnali beavatkozást igénylő elkülönítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1abc6-107">Illustrates how to isolate interactive messages that require immediate action from the solution back end for further processing.</span></span> <span data-ttu-id="1abc6-108">Például egy eszköz előfordulhat, hogy küldése riasztás, amely elindítja a jegy beszúrása egy CRM-rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="1abc6-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="1abc6-109">Adatpont üzenetek, például hőmérséklet telemetriai ellentétben be egy analytics hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="1abc6-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="1abc6-110">Ez az oktatóanyag végén három .NET konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="1abc6-110">At the end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="1abc6-111">**SimulatedDevice**, az alkalmazás létrehozása a módosított változatát a [Ismerkedés az IoT-központ] oktatóanyag adatpont eszközről a felhőbe üzeneteket küld másodpercenként, és interaktív eszközre felhő 10 másodpercenként küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="1abc6-111">**SimulatedDevice**, a modified version of the app created in the [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="1abc6-112">**ReadDeviceToCloudMessages** , amely a nem kritikus az eszköz alkalmazás által küldött telemetriai jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1abc6-112">**ReadDeviceToCloudMessages** that displays the non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="1abc6-113">**ReadCriticalQueue** távolít el az eszköz-alkalmazás által a Service Bus-üzenetsorba küldött a kritikus üzeneteihez.</span><span class="sxs-lookup"><span data-stu-id="1abc6-113">**ReadCriticalQueue** de-queues the critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="1abc6-114">A várólista az IoT hub van csatolva.</span><span class="sxs-lookup"><span data-stu-id="1abc6-114">This queue is attached to the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="1abc6-115">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek, beleértve a C, Java és JavaScript SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="1abc6-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="1abc6-116">Lecseréli a szimulált eszköz ebben az oktatóanyagban egy fizikai eszköz, lásd: a [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="1abc6-116">To learn how to replace the simulated device in this tutorial with a physical device, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="1abc6-117">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1abc6-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="1abc6-118">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1abc6-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="1abc6-119">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1abc6-119">An active Azure account.</span></span> <br/><span data-ttu-id="1abc6-120">Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="1abc6-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="1abc6-121">Néhány alapvető ismerete szükséges [Azure Storage] és [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="1abc6-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="1abc6-122">Interaktív üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="1abc6-122">Send interactive messages</span></span>

<span data-ttu-id="1abc6-123">Az eszköz alkalmazás létrehozott módosítása a [Ismerkedés az IoT-központ] interaktív küldéséhez alkalmanként oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1abc6-123">Modify the device app you created in the [Get started with IoT Hub] tutorial to occasionally send interactive messages.</span></span>

<span data-ttu-id="1abc6-124">A Visual Studio a a **SimulatedDevice** projektre, cserélje le a `SendDeviceToCloudMessagesAsync` metódus a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="1abc6-124">In Visual Studio, in the **SimulatedDevice** project, replace the `SendDeviceToCloudMessagesAsync` method with the following code:</span></span>

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

<span data-ttu-id="1abc6-125">Ez a módszer véletlenszerűen hozzáadja a tulajdonság `"level": "critical"` az eszköz által küldött üzenetek, amely szimulálja egy üzenetet, amely szerint a megoldás háttér-azonnali beavatkozást igényel.</span><span class="sxs-lookup"><span data-stu-id="1abc6-125">This method randomly adds the property `"level": "critical"` to messages sent by the device, which simulates a message that requires immediate action by the solution back-end.</span></span> <span data-ttu-id="1abc6-126">Az eszköz alkalmazás az információt továbbítja az üzenet tulajdonságai ahelyett, hogy az üzenet törzsében úgy, hogy az IoT-központ irányítani tudja a megfelelő üzenet célra az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="1abc6-126">The device app passes this information in the message properties, instead of in the message body, so that IoT Hub can route the message to the proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="1abc6-127">Több, különböző esetekre, beleértve a cold-path feldolgozási mellett az itt bemutatott példában közbeni elérési üzenettulajdonságok üzenetek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1abc6-127">You can use message properties to route messages for various scenarios including cold-path processing, in addition to the hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="1abc6-128">Az egyszerűség kedvéért ez az oktatóanyag nem valósítja meg az összes újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="1abc6-128">For the sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1abc6-129">Az éles kódban, meg kell valósítania egy újrapróbálkozási házirendje, például az exponenciális leállítási, az MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="1abc6-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in the MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a><span data-ttu-id="1abc6-130">Az IoT hub a várólistához üzenetek</span><span class="sxs-lookup"><span data-stu-id="1abc6-130">Route messages to a queue in your IoT hub</span></span>

<span data-ttu-id="1abc6-131">Ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="1abc6-131">In this section, you:</span></span>

* <span data-ttu-id="1abc6-132">Hozzon létre egy Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="1abc6-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="1abc6-133">Csatlakoztassa az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1abc6-133">Connect it to your IoT hub.</span></span>
* <span data-ttu-id="1abc6-134">Az IoT hub, az üzenetek küldése az üzenetsorba, annak alapján, hogy az üzenet egy tulajdonságának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1abc6-134">Configure your IoT hub to send messages to the queue based on the presence of a property on the message.</span></span>

<span data-ttu-id="1abc6-135">A Service Bus-üzenetsorok folyamat üzenetek módjáról további információkért lásd: [Ismerkedés a várólisták][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="1abc6-135">For more information about how to process messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="1abc6-136">Hozzon létre egy Service Bus-üzenetsorba, a [Ismerkedés a várólisták][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="1abc6-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="1abc6-137">A várólista az IoT hub, az előfizetés és a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1abc6-137">The queue must be in the same subscription and region as your IoT hub.</span></span> <span data-ttu-id="1abc6-138">Jegyezze fel az a névtér és a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="1abc6-138">Make a note of the namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1abc6-139">Service Bus-üzenetsorok és témakörök használatos az IoT-központok végpontjai nem lehet **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="1abc6-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="1abc6-140">Ha ezek a lehetőségek valamelyikét engedélyezve vannak, a végpont megjelenik **Unreachable** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="1abc6-140">If either of those options are enabled, the endpoint appears as **Unreachable** in the Azure portal.</span></span>

2. <span data-ttu-id="1abc6-141">Az Azure portálon, nyissa meg az IoT hub, és kattintson **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="1abc6-141">In the Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Az IoT-központ végpontok][30]

3. <span data-ttu-id="1abc6-143">Az a **végpontok** panelen kattintson **Hozzáadás** a sor hozzáadása az IoT hub tetején.</span><span class="sxs-lookup"><span data-stu-id="1abc6-143">In the **Endpoints** blade, click **Add** at the top to add your queue to your IoT hub.</span></span> <span data-ttu-id="1abc6-144">A végpont neve **CriticalQueue** majd válassza ki a legördülő listákat **Service Bus-üzenetsorba**, ahol a várólista van a Service Bus-névtér és a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="1abc6-144">Name the endpoint **CriticalQueue** and use the drop-downs to select **Service Bus queue**, the Service Bus namespace in which your queue resides, and the name of your queue.</span></span> <span data-ttu-id="1abc6-145">Amikor elkészült, kattintson a **mentése** alján.</span><span class="sxs-lookup"><span data-stu-id="1abc6-145">When you are done, click **Save** at the bottom.</span></span>
    
    ![A végpont hozzáadása][31]
    
4. <span data-ttu-id="1abc6-147">Most kattintson **útvonalak** az IoT Hub a.</span><span class="sxs-lookup"><span data-stu-id="1abc6-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="1abc6-148">Kattintson a **Hozzáadás** útválasztási szabály, amely továbbítja az üzeneteket a várólista létrehozása a panel tetején az előzőekben adott hozzá.</span><span class="sxs-lookup"><span data-stu-id="1abc6-148">Click **Add** at the top of the blade to create a routing rule that routes messages to the queue you just added.</span></span> <span data-ttu-id="1abc6-149">Válassza ki **DeviceTelemetry** a adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="1abc6-149">Select **DeviceTelemetry** as the source of data.</span></span> <span data-ttu-id="1abc6-150">Adja meg `level="critical"` feltételt, és válassza ki a várólista-útválasztási szabály végpontjának egyéni végpontként hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="1abc6-150">Enter `level="critical"` as the condition, and choose the queue you just added as a custom endpoint as the routing rule endpoint.</span></span> <span data-ttu-id="1abc6-151">Amikor elkészült, kattintson a **mentése** alján.</span><span class="sxs-lookup"><span data-stu-id="1abc6-151">When you are done, click **Save** at the bottom.</span></span>
    
    ![Egy útvonal hozzáadása][32]
    
    <span data-ttu-id="1abc6-153">Győződjön meg arról, hogy a tartalék útvonal értéke **ON**.</span><span class="sxs-lookup"><span data-stu-id="1abc6-153">Make sure the fallback route is set to **ON**.</span></span> <span data-ttu-id="1abc6-154">Ez az érték az alapértelmezett konfigurációjának megadása az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="1abc6-154">This value is the default configuration for an IoT hub.</span></span>
    
    ![Tartalék útvonal][33]

## <a name="read-from-the-queue-endpoint"></a><span data-ttu-id="1abc6-156">A várólista végpontról olvasása</span><span class="sxs-lookup"><span data-stu-id="1abc6-156">Read from the queue endpoint</span></span>

<span data-ttu-id="1abc6-157">Ebben a szakaszban az üzenetek a várólista végpontról olvasni.</span><span class="sxs-lookup"><span data-stu-id="1abc6-157">In this section, you read the messages from the queue endpoint.</span></span>

1. <span data-ttu-id="1abc6-158">A Visual Studióban adjon hozzá egy Visual C# Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console App (.NET Framework)** (Konzolalkalmazás (.NET-keretrendszer)) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="1abc6-158">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="1abc6-159">Nevet a projektnek **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="1abc6-159">Name the project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="1abc6-160">A Megoldáskezelőben kattintson a jobb gombbal a **ReadCriticalQueue** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="1abc6-160">In Solution Explorer, right-click the **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="1abc6-161">Ez a művelet hatására megjelenik a **NuGet-Csomagkezelő** ablak.</span><span class="sxs-lookup"><span data-stu-id="1abc6-161">This operation displays the **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="1abc6-162">Keresse meg **WindowsAzure.ServiceBus**, kattintson a **telepítése**, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="1abc6-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept the terms of use.</span></span> <span data-ttu-id="1abc6-163">Ez a művelet tölti le, telepíti, és hozzáad egy hivatkozást az Azure Service Bus elemet minden függőségével.</span><span class="sxs-lookup"><span data-stu-id="1abc6-163">This operation downloads, installs, and adds a reference to the Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="1abc6-164">Adja hozzá a következő **használatával** tetején lévő utasítások a **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="1abc6-164">Add the following **using** statements at the top of the **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="1abc6-165">Végül adja hozzá a következő sorokat a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="1abc6-165">Finally, add the following lines to the **Main** method.</span></span> <span data-ttu-id="1abc6-166">Helyettesítse be a kapcsolati karakterlánc **figyelésére** a várólistához tartozó engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="1abc6-166">Substitute the connection string with **Listen** permissions for the queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
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

## <a name="run-the-applications"></a><span data-ttu-id="1abc6-167">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="1abc6-167">Run the applications</span></span>
<span data-ttu-id="1abc6-168">Készen áll arra, hogy futtassa az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="1abc6-168">Now you are ready to run the applications.</span></span>

1. <span data-ttu-id="1abc6-169">A Visual Studio, a Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza ki **indítási projektek beállítása**.</span><span class="sxs-lookup"><span data-stu-id="1abc6-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="1abc6-170">Válassza ki **több kezdőprojekt**, majd jelölje be **Start** művelet a **ReadDeviceToCloudMessages**, **SimulatedDevice**, és **ReadCriticalQueue** projektek.</span><span class="sxs-lookup"><span data-stu-id="1abc6-170">Select **Multiple startup projects**, then select **Start** as the action for the **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="1abc6-171">Nyomja le az **F5** elindítani a három konzol alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="1abc6-171">Press **F5** to start the three console apps.</span></span> <span data-ttu-id="1abc6-172">A **ReadDeviceToCloudMessages** az alkalmazás által küldött üzenetek csak nem kritikus rendelkezik a **SimulatedDevice** alkalmazás, és a **ReadCriticalQueue** alkalmazás még csak a kritikus üzeneteihez.</span><span class="sxs-lookup"><span data-stu-id="1abc6-172">The **ReadDeviceToCloudMessages** app has only non-critical messages sent from the **SimulatedDevice** application, and the **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Három konzol alkalmazás][50]

## <a name="next-steps"></a><span data-ttu-id="1abc6-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1abc6-174">Next steps</span></span>
<span data-ttu-id="1abc6-175">Ebben az oktatóanyagban megtudta, hogyan megbízhatóan átirányítani az eszköz a felhőbe küldött üzeneteket az üzenetet az IoT-központ útválasztási funkcióra használatával.</span><span class="sxs-lookup"><span data-stu-id="1abc6-175">In this tutorial, you learned how to reliably dispatch device-to-cloud messages by using the message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="1abc6-176">A [IoT hubbal felhő eszközre üzenetek küldése] [ lnk-c2d] bemutatja, hogyan üzenetek küldése az eszközöket a megoldás háttérből.</span><span class="sxs-lookup"><span data-stu-id="1abc6-176">The [How to send cloud-to-device messages with IoT Hub][lnk-c2d] shows you how to send messages to your devices from your solution back end.</span></span>

<span data-ttu-id="1abc6-177">Példák teljes végpontok közötti megoldások, amelyek használják az IoT-központot, lásd: [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="1abc6-177">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="1abc6-178">Az IoT hubbal megoldások fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="1abc6-178">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<span data-ttu-id="1abc6-179">Az üzenetet az IoT hubon útválasztási kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="1abc6-179">To learn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
<span data-ttu-id="1abc6-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span><span class="sxs-lookup"><span data-stu-id="1abc6-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span></span>
<span data-ttu-id="1abc6-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span><span class="sxs-lookup"><span data-stu-id="1abc6-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span></span>
<span data-ttu-id="1abc6-182">[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="1abc6-182">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="1abc6-183">[Ismerkedés az IoT-központ]: iot-hub-csharp-csharp-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="1abc6-183">[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md</span></span>
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
<span data-ttu-id="1abc6-184">[Azure IoT fejlesztői központ]: https://azure.microsoft.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="1abc6-184">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span></span>
<span data-ttu-id="1abc6-185">[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="1abc6-185">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
