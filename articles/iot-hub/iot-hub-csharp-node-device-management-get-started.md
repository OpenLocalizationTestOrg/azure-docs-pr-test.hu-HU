---
title: "aaaGet Azure IoT Hub kezelés (.NET/csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz felügyeleti tooinitiate egy távoli eszköz újraindul. Hello Azure IoT-eszközök SDK használata Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
ms.openlocfilehash: ea6d50dfac1c222d7836e3bf5503c6c9b8c89dfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-netnode"></a>Ismerkedés az Eszközfelügyelet (.NET/csomópont)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Ez az oktatóanyag a következőket mutatja be:

* Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.
* Létrehoz egy szimulált eszköz alkalmazást, amely tartalmazza a közvetlen módszer, amely az eszköz újraindul. Közvetlen módszerek hello felhőből hívják.
* Hozzon létre egy .NET-Konzolalkalmazás, amely behívja hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.

Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:

**dmpatterns_getstarted_device.js**, amely kapcsolódik tooyour IoT-központ hello eszközidentitás, korábban létrehozott kap egy újraindítás közvetlen módszer, fizikai újraindítás szimulálja, és hello idő az utolsó újraindítás hello jelentéseket.

**TriggerReboot**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello választ, és megjeleníti hello frissítése jelentett tulajdonságok.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* NODE.js-verzió 0.12.x vagy újabb verzióját, <br/>  [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Eseményindító közvetlen metódussal hello eszközön távoli újraindítás
Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) közvetlen metódussal eszköz távoli újraindítást kezdeményezett hoz létre. hello alkalmazás eszköz iker lekérdezések toodiscover hello legutóbbi újraindítás használ, az eszköznek.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt tooa új megoldás hozzáadása hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **TriggerReboot**.

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

2. A Megoldáskezelőben kattintson a jobb gombbal hello **TriggerReboot** projektre, és kattintson a **NuGet-csomagok kezelése**.
3. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
4. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc az előző szakaszban hello és hello céleszköz létrehozott hello központ hello.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. Adja hozzá a következő metódus toohello hello **Program** osztály.  A kód beolvassa hello eszköz iker az eszköz- és kimenetekkel hello újraindítás hello jelentett tulajdonságait.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Adja hozzá a következő metódus toohello hello **Program** osztály.  Ez a kód hello újraindítás közvetlen metódussal hello eszközön kezdeményezi.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. Hello megoldás felépítéséhez.

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ez a szakasz tartalma

* Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer
* A szimulált eszköz újraindítását eseményindító
* Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása

1. Hozzon létre egy új üres nevű **manageddevice**.  A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **dmpatterns_getstarted_device.js** hello fájlban **manageddevice** mappa.
4. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_device.js** fájlt:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.  Hello kapcsolati karakterláncot cserélje le a eszköz kapcsolati karakterláncát.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Adja hozzá a következő függvény tooimplement hello közvetlen módszer hello eszközön hello
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Adja hozzá a hello következő code tooopen hello kapcsolat tooyour IoT-központot, és indítsa el a hello közvetlen módszer figyelő:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Mentse és zárja be a hello **dmpatterns_getstarted_device.js** fájlt.
   
> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].


## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Futtatási hello C# Konzolalkalmazás **TriggerReboot**. Kattintson a jobb gombbal hello **TriggerReboot** projektre, válassza **Debug**, majd válassza ki **Start új példány**.

3. Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/