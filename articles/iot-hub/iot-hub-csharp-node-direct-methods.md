---
title: "Azure IoT Hub aaaUse közvetlen módszerek (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. Hello Azure IoT-eszközök SDK használata Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a>Közvetlen módszerekkel (.NET/csomópont)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Ebben az oktatóanyagban fogjuk toodevelop egy .NET- és egy Node.js-Konzolalkalmazás:

* **CallMethodOnDevice.sln**, .NET-háttér-alkalmazás, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.
* **SimulatedDevice.js**, a Node.js alkalmazást, mert egy eszköz tooyour IoT-központ összeköti a korábban létrehozott hello eszközidentitás szimulál, és a hello felhő által meghívott toohello metódus válaszol.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.
> 
> 

toocomplete ebben az oktatóanyagban szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódus hoz létre.

1. Hozzon létre egy új, **simulateddevice** nevű üres mappát. A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy fájlt hello **simulateddevice** mappa, és adjon neki nevet **SimulatedDevice.js**.
4. Adja hozzá a következő hello `require` hello utasításokat elejére hello **SimulatedDevice.js** fájlt:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Adja hozzá a **connectionString** változó, és toocreate használni egy **DeviceClient** példány. Cserélje le **{eszköz kapcsolati karakterlánc}** hello eszköz kapcsolati karakterlánccal hozta létre a hello *hozzon létre egy eszközidentitás* szakasz:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Adja hozzá a következő függvény tooimplement hello közvetlen módszer hello eszközön hello:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Nyissa meg a hello kapcsolat tooyour IoT-központot, és hello metódus figyelő inicializálása:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Mentse és zárja be a hello **SimulatedDevice.js** fájlt.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>A közvetlen metódus hívása az eszközön
Ebben a szakaszban egy .NET-Konzolalkalmazás metódus meghívja a hello szimulált eszköz app, majd megjeleníti a hello válasz hoz létre.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **CallMethodOnDevice**.
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. A Megoldáskezelőben kattintson a jobb gombbal hello **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .
3. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]

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

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához
Most már áll készen toorun hello alkalmazások.

1. A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **CallMethodOnDevice** projektre a hello legördülő menüre.

2. A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toostart metódushívások az IoT-központ figyel hello:
   
    ```
    node SimulatedDevice.js
    ```
   Várjon, amíg hello szimulált eszköz tooopen:![][7]
3. Most, hogy hello eszköz csatlakoztatva van, és metódus meghívásához várakozik, futtassa a hello .NET **CallMethodOnDevice** tooinvoke hello használata hello szimulált eszköz alkalmazásban. Hello eszköz válasz írása hello konzolon kell megjelennie.
   
    ![][8]
4. hello eszköz majd reagál toohello metódus által nyomtatás ezt az üzenetet:
   
    ![][9]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta. Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről. 

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Ismerkedés az IoT-központ]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
