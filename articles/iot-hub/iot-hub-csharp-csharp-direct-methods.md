---
title: "Azure IoT Hub aaaUse közvetlen módszerek (.NET/.NET) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. Hello Azure IoT-eszközök SDK használata .NET tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: d4fa093a99558ec6faf294c2583a14a722b9ac03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnet"></a>Közvetlen módszerekkel (.NET/.NET)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Ebben az oktatóanyagban dolgozunk folyamatos toodevelop két .NET konzol alkalmazások:

* **CallMethodOnDevice**, egy háttér-alkalmazást, mert metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.
* **SimulateDeviceMethods**, egy konzolalkalmazás tooyour IoT-központ összeköti a korábban létrehozott hello eszközidentitás eszköz szimulálja, és válaszolhat hello felhő által meghívott toohello metódus.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.
> 
> 

toocomplete ebben az oktatóanyagban szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Ha azt szeretné toocreate hello eszközidentitás programozott módon helyette, olvassa el a megfelelő szakasz hello hello [csatlakoztassa a szimulált eszköz tooyour IoT hubot .NET használatával] [ lnk-device-identity-csharp] cikk.


## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban egy .NET-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódus hoz létre.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **SimulateDeviceMethods**.
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]
    
1. A Megoldáskezelőben kattintson a jobb gombbal hello **SimulateDeviceMethods** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**. Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello eszköz kapcsolati karakterláncot, amely az előző szakaszban hello feljegyzett.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Adja hozzá a következő tooimplement hello közvetlen módszer hello eszközön hello:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Végül adja hozzá a következő kód toohello hello **fő** metódus tooopen hello kapcsolat tooyour IoT hub és inicializálási hello metódus figyelő:
   
        try
        {
            Console.WriteLine("Connecting toohub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter tooexit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove hello "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **SimulateDeviceMethods** projektre a hello legördülő menüre.        

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>A közvetlen metódus hívása az eszközön
Ebben a szakaszban egy .NET-Konzolalkalmazás metódus meghívja a hello szimulált eszköz app, majd megjeleníti a hello válasz hoz létre.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **CallMethodOnDevice**.
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createserviceapp]
2. A Megoldáskezelőben kattintson a jobb gombbal hello **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .
3. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]

4. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Ez a metódus meghívja a közvetlen módszer nevű `writeLine` a hello `myDeviceId` eszköz. Ezután ír a hello konzolon hello eszköz által biztosított hello válasz. Ne feledje, hogy lehetséges toospecify hello eszköz toorespond időtúllépési értéket.
7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **CallMethodOnDevice** projektre a hello legördülő menüre.

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához
Most már áll készen toorun hello alkalmazások.

1. Hello .NET eszköz alkalmazás **SimulateDeviceMethods**. Akkor kell megkezdeni a figyelést az IoT Hub a metódushívások: 

    ![Eszköz-alkalmazás futtatása][img-deviceapprun]
1. Most, hogy hello eszköz csatlakoztatva van, és metódus meghívásához várakozik, futtassa a hello .NET **CallMethodOnDevice** tooinvoke hello használata hello szimulált eszköz alkalmazásban. Hello eszköz válasz írása hello konzolon kell megjelennie.
   
    ![Szolgáltatás-alkalmazás futtatása][img-serviceapprun]
1. hello eszköz majd reagál toohello metódus által nyomtatás ezt az üzenetet:
   
    ![Hello eszközön meghívott közvetlen módszer][img-directmethodinvoked]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta. Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről. 

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Ismerkedés az IoT-központ]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
