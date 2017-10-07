---
title: "aaaGet Azure IoT Hub kezelés (csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse IoT Hub eszköz felügyeleti tooinitiate egy távoli eszköz újraindul. Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és a szolgáltatás alkalmazást, amely meghívja a közvetlen módszer hello hello Azure IoT SDK-t használja."
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
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 5dd1878e71231850fb95f4170b823f1e86c3ee83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-node"></a>Ismerkedés az Eszközfelügyelet (csomópont)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Ez az oktatóanyag a következőket mutatja be:

* Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.
* Létrehoz egy szimulált eszköz alkalmazást, amely tartalmazza a közvetlen módszer, amely az eszköz újraindul. Közvetlen módszerek hello felhőből hívják.
* Hozzon létre egy Node.js-Konzolalkalmazás, amely behívja hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.

Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:

**dmpatterns_getstarted_device.js**, amely kapcsolódik tooyour IoT-központ hello eszközidentitás, korábban létrehozott kap egy újraindítás közvetlen módszer, fizikai újraindítás szimulálja, és hello idő az utolsó újraindítás hello jelentéseket.

**dmpatterns_getstarted_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello választ, és megjeleníti hello frissítése jelentett tulajdonságok.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* NODE.js-verzió 0.12.x vagy újabb verzióját, <br/>  [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ez a szakasz tartalma

* Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer
* A szimulált eszköz újraindítását eseményindító
* Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása

1. Hozzon létre egy **manageddevice** nevű üres mappát.  A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_device.js** hello fájlban **manageddevice** mappa.
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
7. Nyissa meg a hello kapcsolat tooyour IoT-központ, és indítsa el a hello közvetlen módszer figyelő:
   
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

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Eseményindító közvetlen metódussal hello eszközön távoli újraindítás
Ebben a szakaszban egy Node.js-Konzolalkalmazás közvetlen metódussal eszköz távoli újraindítást kezdeményezett hoz létre. hello alkalmazás eszköz iker lekérdezések toodiscover hello legutóbbi újraindítás használ, az eszköznek.

1. Hozzon létre egy üres nevű **triggerrebootondevice**.  A hello **triggerrebootondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **triggerrebootondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:
   
    ```
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_service.js** hello fájlban **triggerrebootondevice** mappa.
4. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_service.js** fájlt:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Adja hozzá a következő függvény tooinvoke hello eszköz metódus tooreboot hello céleszköz hello:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked hello device tooreboot.");  
            }
        });
    };
    ```
7. Adja hozzá a hello következő tooquery hello eszköz funkciót, és a legutóbbi újraindítás hello beolvasása:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device tooreport last reboot time.');
        });
    };
    ```
8. Adja hozzá a következő kód toocall hello funkciók hello kiváltó hello indítsa újra a közvetlen módszer és hello a lekérdezés utolsó újraindítás időpontja:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Mentse és zárja be a hello **dmpatterns_getstarted_service.js** fájlt.

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Parancssorba hello hello **triggerrebootondevice** mappa, futtassa a következő parancs tootrigger hello távoli hello újraindul, és lekérdezi a hello eszköz iker toofind hello utolsó indítsa újra a számítógépet idő.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
