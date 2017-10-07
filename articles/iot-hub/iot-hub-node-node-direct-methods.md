---
title: "az IoT-központ aaaAzure közvetlen módszerek (csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. Hello Azure IoT SDK-k használata Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és egy szolgáltatás-alkalmazást, amely hello közvetlen metódust hívja."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12300ba451816fec1f80163b633f6b6e411d9e5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>Az IoT-eszközön a Node.js közvetlen módszerekkel
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:

* **CallMethodOnDevice.js**, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.
* **SimulatedDevice.js**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és hello felhő által meghívott toohello metódus válaszol.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.
> 
> 

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa metódus hoz létre.

1. Hozzon létre egy új, **simulateddevice** nevű üres mappát. A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **SimulatedDevice.js** hello fájlban **simulateddevice** mappa.
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
6. Adja hozzá a következő függvény tooimplement hello metódus hello eszközön hello:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Nyissa meg a hello kapcsolat tooyour IoT-központot, és inicializálási hello metódus figyelő elindítása:
   
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

## <a name="call-a-method-on-a-device"></a>A metódus hívása az eszközön
Ebben a szakaszban hozzon létre egy Node.js-Konzolalkalmazás, amely egy metódust hívja be hello szimulált eszköz alkalmazásának és hello válasz megjeleníti.

1. Hozzon létre egy új üres nevű **callmethodondevice**. A hello **callmethodondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **callmethodondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:
   
    ```
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **CallMethodOnDevice.js** hello fájlban **callmethodondevice** mappa.
4. Adja hozzá a következő hello `require` hello utasításokat elejére hello **CallMethodOnDevice.js** fájlt:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Adja hozzá a következő változó deklarációjában hello és hello helyőrző értékét lecserélheti egy hello IoT-központ kapcsolati karakterlánca:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. Hello ügyfél tooopen hello kapcsolat tooyour IoT hub létrehozása.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Adja hozzá a következő függvény tooinvoke hello eszköz metódus és a nyomtató hello eszköz válasz toohello konzol hello:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed tooinvoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Mentse és zárja be a hello **CallMethodOnDevice.js** fájlt.

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toostart metódushívások az IoT-központ figyel hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. A parancssorba a hello **callmethodondevice** mappa, futtassa a következő parancs toobegin az IoT hub figyelési hello:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Hello eszköz reagálni toohello metódus által üdvözlőüzenetére és hello alkalmazást, amely hello metódus megjelenítési hello válasz nevű hello eszközről nyomtasson jelenik meg:
   
    ![][9]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta. Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről. 

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Ismerkedés az IoT-központ]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
