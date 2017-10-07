---
title: "aaaGet Azure IoT Hub eszköz twins (.NET/csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooadd címkéket, majd az IoT Hub-lekérdezést. Hello Azure IoT-eszközök SDK használata Node.js tooimplement hello szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely hello címkék hozzáadása, és az IoT-központ lekérdezés hello futtatja."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
ms.openlocfilehash: 1cec082ebddc19c9b87998a5fd0159d32b07acd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnode"></a>Ismerkedés az eszköz twins (.NET/csomópont)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Ez az oktatóanyag végén hello hogy a .NET- és egy Node.js-Konzolalkalmazás:

* **AddTagsAndQuery.sln**, a .NET-háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.
* **TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely olyan eszköz, amely tooyour IoT-központ a korábban létrehozott hello eszközidentitás szimulálja, és jelenti a kapcsolat állapotát.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.
> 
> 

toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása
Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely hely metaadatok toohello eszköz iker társított létrehozása **myDeviceId**. Ezután hello eszköz twins hello IoT-központ hello található hello eszközök kiválasztása tárolja SZÁMUNKRA, és majd hello azokon, mobilhálózat kapcsolatot jelentett lekérdezések.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **AddTagsAndQuery**.
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. A Megoldáskezelőben kattintson a jobb gombbal hello **AddTagsAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices**. Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
1. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    Hello **RegistryManager** osztály összes hello módszerek szükséges toointeract az eszköz twins hello szolgáltatás elérhetővé teszi. hello előző kód először inicializálja hello **registryManager** objektumot, majd beolvassa az eszköz iker hello **myDeviceId**, és végül frissíti a címkék szükséges hello helyére vonatkozó információkat.
   
    Miután frissített, két lekérdezést hajt végre: először a választ csak hello eszköz twins hello található eszközök hello **Redmond43** gépek és hello második refines hello lekérdezés tooselect csak hello csatlakozó eszközöket is keresztül mobilhálózati.
   
    Vegye figyelembe, hogy hello előző kóddal, amikor hello létrehozza **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg. Hello **lekérdezés** objektum tartalmaz egy **HasMoreResults** használható tooinvoke hello logikai tulajdonság **GetNextAsTwinAsync** módszerek többször tooretrieve összes eredmények. A metódus hívása **GetNextAsJson** eredmények, amelyek például nem eszköz twins, összesítési-lekérdezések eredményének érhető el.
1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **AddTagsAndQuery** projekt **Start**. Hello megoldás felépítéséhez.
1. Az alkalmazás futtatásához kattintson a jobb gombbal a hello **AddTagsAndQuery** projektet, majd válassza **Debug**, utána pedig **Start új példány**. A hello eredményei egy eszközhöz hello lekérdezés kérése minden eszköz a mappában lévő kell megjelennie **Redmond43** nincs hello lekérdezés, amely korlátozza a hello mobilhálózati használó toodevices elkészítéséig.
   
    ![Lekérdezés eredményei ablakban][img-addtagapp]

Hello a következő szakaszban létrehoz egy eszköz-alkalmazást, amely hello kapcsolati információ jelent, és módosításokat hello hello előző szakaszban hello lekérdezés eredménye.

## <a name="create-hello-device-app"></a>Hello eszköz alkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**, majd frissíti a jelentésben szereplő tulajdonságok toocontain hello adatait, hogy használatával mobilhálózathoz csatlakozik.

1. Hozzon létre egy új üres nevű **reportconnectivity**. A hello **reportconnectivity** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett értéket.
   
    ```
    npm init
    ```
1. A parancssorban hello **reportconnectivity** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag :
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** hello fájlban **reportconnectivity** mappa.
1. Adja hozzá a következő kód toohello hello **ReportConnectivity.js** fájlt, és az eszköz kapcsolati karakterlánc egy hello létrehozása után másolja hello hello helyőrzője helyettesítse **myDeviceId** eszköz identitás:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről. hello előző kód után inicializálja a hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId** , és frissíti a jelentett tulajdonsága hello kapcsolódási információt.
1. Hello eszköz alkalmazás futtatása
   
        node ReportConnectivity.js
   
    Hello üzenet `twin state reported`.
1. Most, hogy hello eszköz jelentette a kapcsolati információ meg kell jelennie mindkét lekérdezések. Futtassa a hello .NET **AddTagsAndQuery** app toorun hello le újra. Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.
   
    ![][img-addtagapp2]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és a szimulált eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a. Azt is megtanulta, hogyan tooquery ezt az információt hello SQL-szerű IoT Hub lekérdezési nyelv.

A következő erőforrások toolearn hogyan használja hello számára:

* telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban
* eszközök, eszköz iker kívánt tulajdonságok használata hello konfigurálása [használata szükséges tulajdonságok tooconfigure eszközök] [ lnk-twin-how-to-configure] oktatóanyagban
* interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

