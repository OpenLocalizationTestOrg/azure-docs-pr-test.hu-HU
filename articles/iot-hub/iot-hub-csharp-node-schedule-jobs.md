---
title: "az Azure IoT Hub (.NET/csomópont) aaaSchedule feladatok |} Microsoft Docs"
description: "Hogyan tooschedule az Azure IoT-központ feladat tooinvoke a közvetlen módszer több eszközön. Hello Azure IoT-eszközök SDK használata Node.js tooimplement hello szimulált eszköz alkalmazásokat és hello Azure IoT szolgáltatás SDK .NET tooimplement a app toorun hello feladata."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: f6148b67129dde4580bfe9ccceafd6400fbc5976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Ütemezés és a feladatok (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub tooschedule és nyomon követése feladatok frissítő eszközök millióira használja. Feladatok használata:

* Eszköz kívánt tulajdonságainak frissítése
* Címkék frissítése
* Közvetlen metódusok

Egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi hello végrehajtási eszköz két lekérdezést által meghatározott eszközök készlete alapján. Például egy háttér-alkalmazást, egy feladat tooinvoke közvetlen módszer hello eszköz újraindulása 10 000 eszközökön használható. Adjon meg hello eszközök eszköz iker lekérdezéssel, és egy későbbi időpontban hello feladat toorun ütemezni. hello feladat nyomon követi előrehaladását, mivel minden egyes hello eszközök kapja meg, és hello újraindítás közvetlen metódus hajtható végre.

További információ az egyes képességek, toolearn lásd:

* A két eszköz és a tulajdonságok: [Ismerkedés az eszköz twins] [ lnk-get-started-twin] és [oktatóanyag: hogyan toouse eszköz iker tulajdonságai][lnk-twin-props]
* A közvetlen módszer: [IoT Hub fejlesztői útmutató - közvetlen módszerek] [ lnk-dev-methods] és [oktatóanyag: közvetlen módszerekkel][lnk-c2d-methods]

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy eszköz-alkalmazást, amely egy közvetlen a hívott metódus **lockDoor** , amely hello háttér-alkalmazás által hívható. hello eszközalkalmazás kívánt tulajdonságok módosítását is fogad hello háttér-alkalmazást.
* Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot toocall hello **lockDoor** közvetlen módszer több eszközön. Egy másik feladat küldi el a kívánt tulajdonság toomultiple eszközök frissíti.

Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:

**simDevice.js** , hogy csatlakozik az IoT-központ tooyour, hello megvalósítja **lockDoor** közvetlen metódust, és kezeli a szükséges tulajdonságok módosítását.

**ScheduleJob** használó feladatok toocall hello **lockDoor** közvetlen módszer és a frissítés hello eszköz iker szükségeskonfiguráció-tulajdonságok több eszközön.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* A Node.js 0.12.x vagy újabb verziója. hello cikk [a fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Közvetlen metódus hívása, és az eszköz iker frissítések küldése feladatok ütemezése

Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) használó feladatok toocall hello létrehozása **lockDoor** közvetlen módszer és elküldeni kívánt tulajdonság toomultiple eszközök frissíti.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **ScheduleJob**.

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

1. A Megoldáskezelőben kattintson a jobb gombbal hello **ScheduleJob** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez a lépés tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Adja hozzá a következő hello `using` utasítás Ha hello alapértelmezett utasításokban még nincs letöltve.

    ```csharp
    using System.Threading.Tasks;
    ```

1. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrzőt cserélje le az IoT-központ kapcsolati karakterlánc hello központ hello előző szakaszban létrehozott hello.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER toorun hello next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER tooexit.");
    Console.ReadLine();
    ```

1. A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **ScheduleJob** projekt **Start**. Hello megoldás felépítéséhez.

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása

Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely tooa hello felhő, amely elindítja a szimulált eszköz újraindítás által meghívott közvetlen metódus válaszol, és használja hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása.

1. Hozzon létre egy új üres nevű **simDevice**.  A hello **simDevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:

    ```cmd/sh
    npm init
    ```

1. A parancssorban hello **simDevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Egy szövegszerkesztő használatával hozzon létre egy új **simDevice.js** hello fájlban **simDevice** mappa.

1. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **simDevice.js** fájlt:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány. Győződjön meg arról, hogy tooreplace hello helyőrzőt értékek megfelelő tooyour beállítása.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Adja hozzá a következő függvény toohandle hello hello **lockDoor** metódust.

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. Adja hozzá a következő kód tooregister hello kezelő hello hello **lockDoor** metódust.

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. Mentse és zárja be a hello **simDevice.js** fájlt.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello hello **simDevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.

    ```cmd/sh
    node simDevice.js
    ```

1. Futtatási hello C# Konzolalkalmazás **ScheduleJob** kattintson a jobb gombbal a hello **ScheduleJob** projekt, jelölje be **Debug** és **Start új példány**.

1. Az eszköz és a háttér-alkalmazások hello kimenet jelenik meg.

    ![Hello alkalmazások tooschedule feladatok futtatása][img-schedulejobs]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban használt a egy feladat tooschedule közvetlen módszer tooa eszköz és a hello eszköz iker tulajdonságok hello frissítése.

keresztül hello vezeték nélkül belső vezérlőprogram frissítése, olvassa el az IoT-központ és az eszköz, például távolról felügyeleti mintákat bevezetés toocontinue [oktatóanyag: hogyan toodo a belső vezérlőprogram frissítése][lnk-fwupdate].

Ismerkedés az IoT-központ toocontinue lásd [Ismerkedés az IoT-Edge][lnk-iot-edge].

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
