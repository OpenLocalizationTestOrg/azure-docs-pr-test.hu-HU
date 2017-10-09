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
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Az IoT-központ eszközről a felhőbe üzenetek útvonalak (.NET) használatával

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Ez az oktatóanyag épít, hello [Ismerkedés az IoT-központ] oktatóanyag. hello oktatóanyag:

* Bemutatja, hogyan toouse útválasztási szabályok toodispatch eszközről a felhőbe üzenetek könnyen és konfigurációs-alapú.
* Bemutatja, hogyan tooisolate interaktív üzenetek hello megoldásból azonnali beavatkozást igénylő háttér további feldolgozásra. Például egy eszköz előfordulhat, hogy küldése riasztás, amely elindítja a jegy beszúrása egy CRM-rendszerbe. Adatpont üzenetek, például hőmérséklet telemetriai ellentétben be egy analytics hírcsatorna.

Ez az oktatóanyag végén hello három .NET konzol alkalmazások futtatása:

* **SimulatedDevice**, hello alkalmazás hello létrehozott egy módosított verziója [Ismerkedés az IoT-központ] oktatóanyag adatpont eszközről a felhőbe üzeneteket küld másodpercenként, és interaktív eszközre felhő minden 10 üzenetek másodperc.
* **ReadDeviceToCloudMessages** , hogy a jeleníti meg az eszköz alkalmazás által küldött nem kritikus telemetriai hello.
* **ReadCriticalQueue** távolít el hello kritikus által küldött üzenetek az eszköz alkalmazás egy Service Bus-üzenetsorba. A várólista nincs csatolt toohello IoT-központot.

> [!NOTE]
> Az IoT-központ rendelkezik sok eszköz platformok és nyelvek, beleértve a C, Java és JavaScript SDK támogatása. toolearn hogyan tooreplace ebben az oktatóanyagban szimulált eszköz hello fizikai eszköz, tekintse meg a hello [Azure IoT fejlesztői központ].

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. <br/>Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.

Néhány alapvető ismerete szükséges [Azure Storage] és [Azure Service Bus].

## <a name="send-interactive-messages"></a>Interaktív üzenetek küldése

Hello létrehozott hello eszközalkalmazás módosítása [Ismerkedés az IoT-központ] oktatóanyag toooccasionally interaktív üzeneteket küldeni.

A Visual Studio, a hello **SimulatedDevice** projektre, cserélje le a hello `SendDeviceToCloudMessagesAsync` hello kód a következő metódust:

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

Ez a metódus véletlenszerűen ad hello tulajdonság `"level": "critical"` hello eszközét, ezzel szimulálja egy üzenetet, amely azonnali beavatkozást igényel a háttérkiszolgálók megoldás hello által küldött toomessages. hello eszközalkalmazás információt átadja a hello üzenet tulajdonságai, ahelyett, hogy hello üzenet törzsében, úgy, hogy az IoT-központ megfelelő üzenet toohello hello üzenetcél irányíthatja.

> [!NOTE]
> Üzenet tulajdonságai tooroute üzenetek feldolgozása, Ezenfelül itt látható példa az toohello közbeni útvonalra cold-elérési utat is több, különböző esetekre is használhatja.

> [!NOTE]
> Hello szakét az egyszerűség Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania egy újrapróbálkozási házirendje, például az exponenciális leállítási hello MSDN-cikkben leírtak [átmeneti hiba kezelése].

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a>Az IoT hub útvonal üzenetek tooa sorból

Ebben a szakaszban:

* Hozzon létre egy Service Bus-üzenetsorba.
* Az IoT-központ tooyour csatlakoztassa.
* Konfigurálja az IoT hub toosend üzenetek toohello várólista hello jelenléte ügyfélkulcscsere-tulajdonságok alapján.

Hogyan tooprocess a Service Bus-üzenetsorok érkező üzenetek kapcsolatos további információkért lásd: [Ismerkedés a várólisták][Service Bus queue].

1. Hozzon létre egy Service Bus-üzenetsorba, a [Ismerkedés a várólisták][Service Bus queue]. hello várólista hello kell lennie ugyanazon előfizetésével és régiójával, az IoT hub. Jegyezze fel a hello névtér és a várólista nevét.

    > [!NOTE]
    > Service Bus-üzenetsorok és témakörök használatos az IoT-központok végpontjai nem lehet **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van. Ha ezek a lehetőségek valamelyikét engedélyezve vannak, hello végpont megjelenik **Unreachable** a hello Azure-portálon.

2. A hello Azure-portálon, nyissa meg az IoT hub, és kattintson a **végpontok**.
    
    ![Az IoT-központ végpontok][30]

3. A hello **végpontok** panelen kattintson a **hozzáadása** : hello felső tooadd a várólista tooyour IoT-központot. Név hello végpont **CriticalQueue** és hello legördülő listákat tooselect **Service Bus-üzenetsorba**, Service Bus-névtér, amelyben a várólista található hello és hello a várólista neve. Amikor elkészült, kattintson a **mentése** hello lap alján.
    
    ![A végpont hozzáadása][31]
    
4. Most kattintson **útvonalak** az IoT Hub a. Kattintson a **Hozzáadás** hello panel toocreate toohello csak várólistára meg üzeneteit útválasztási szabályainak hozzáadott hello tetején. Válassza ki **DeviceTelemetry** hello adatforrásként. Adja meg `level="critical"` hello feltételként, és válassza ki az előzőekben adott hozzá, útválasztási szabály végpont hello egyéni végpontként hello várólista. Amikor elkészült, kattintson a **mentése** hello lap alján.
    
    ![Egy útvonal hozzáadása][32]
    
    Ellenőrizze, hogy hello tartalék útvonal túl van-e állítva**ON**. Ezt az értéket az IoT-központ hello alapértelmezett konfigurációjának.
    
    ![Tartalék útvonal][33]

## <a name="read-from-hello-queue-endpoint"></a>Hello várólista végpont olvasásakor

Ebben a szakaszban hello várólista végpont köszönőüzenetei olvasni azt.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása, hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Név hello projekt **ReadCriticalQueue**.

2. A Megoldáskezelőben kattintson a jobb gombbal hello **ReadCriticalQueue** projektre, és kattintson a **NuGet-csomagok kezelése**. Ez a művelet hatására megjelenik a hello **NuGet-Csomagkezelő** ablak.

3. Keresse meg **WindowsAzure.ServiceBus**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello. Ez a művelet tölti le, telepíti, és hozzáad egy hivatkozást toohello Azure Service Bus elemet minden függőségével.

4. Adja hozzá a következő hello **használatával** hello hello tetején utasítások **Program.cs** fájlt:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Végül adja hozzá a következő sorokat toohello hello **fő** metódust. Helyettesítse be a kapcsolati karakterlánc hello **figyelésére** hello várólista engedélyek:
   
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

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához
Most már készen áll a toorun hello alkalmazások áll.

1. A Visual Studio, a Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza ki **indítási projektek beállítása**. Válassza ki **több kezdőprojekt**, majd jelölje be **Start** hello műveletként a hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, és **ReadCriticalQueue** projektek.
2. Nyomja le az **F5** toostart hello három konzol alkalmazás. Hello **ReadDeviceToCloudMessages** az alkalmazás rendelkezik hello küldött üzenetek csak nem kritikus **SimulatedDevice** az alkalmazás és hello **ReadCriticalQueue** az alkalmazás csak rendelkezik kritikus üzeneteihez.
   
   ![Három konzol alkalmazás][50]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan tooreliably mennyi funkcióval hello üzenet útválasztási IoT hub eszköz a felhőbe küldött üzeneteket.

Hello [hogyan toosend felhőből eszközre küldött üzenetek IoT-központ] [ lnk-c2d] bemutatja, hogyan toosend üzenetek tooyour eszközöket a megoldás háttérből.

teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite][lnk-suite].

További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].

toolearn üzenetet az IoT hubon útválasztási kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging].

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
