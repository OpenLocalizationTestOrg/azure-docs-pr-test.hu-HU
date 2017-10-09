---
title: "az Azure IoT Hub (csomópont) aaaCloud eszközre üzenetek |} Microsoft Docs"
description: "Hogyan toosend felhő eszközre küldött üzenetek tooa eszköz regisztrációját az Azure IoT-központ hello Azure IoT SDK for Node.js használatával. A szimulált eszköz alkalmazás tooreceive felhő eszközre üzeneteket módosítja, és módosítsa a háttér-alkalmazás toosend hello felhő eszközre üzeneteket."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 1ccae0cada52193c2abb91504c086cac226e93da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Az IoT-központ (csomópont) felhő eszközre-üzenetek
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Bevezetés
Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere. Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.

Ez az oktatóanyag épül [Ismerkedés az IoT-központ]. Megmutatja, hogyan számára:

* A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.
* Az eszközön a felhőből eszközre üzeneteket fogadni.
* A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.

További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].

Ez az oktatóanyag végén hello akkor futtatja a Node.js-konzol két alkalmazásokat:

* **SimulatedDevice**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.
* **SendCloudToDeviceMessage**, amely IoT-központot egy felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazás küld, és majd megkapja a kézbesítési nyugtázási.

> [!NOTE]
> Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása. Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [Azure IoT fejlesztői központ].
> 
> 

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

## <a name="receive-messages-in-hello-simulated-device-app"></a>A szimulált eszköz app hello üzeneteket fogadni
Ebben a szakaszban létrehozott hello szimulált eszköz alkalmazásának módosítása [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.

1. Egy szövegszerkesztőben nyissa meg hello SimulatedDevice.js fájlt.
2. Módosítsa a hello **connectCallback** az IoT-központ által küldött toohandle üzenetek működéséhez. Ebben a példában hello eszköz mindig hív hello **teljes** toonotify, feldolgozta-e üdvözlőüzenetére IoT-Központ működéséhez. Hello új verziójának **connectCallback** függvény a következő kódrészletet hello néz ki:
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it toohello IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > A HTTP használata helyett MQTT vagy AMQP hello átvitelhez, hello **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként). MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Felhő eszközre üzenet küldése
Ebben a szakaszban egy Node.js-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazást hoz létre. Eszközazonosító hello hozzáadott hello eszköz kell hello [Ismerkedés az IoT-központ] oktatóanyag. Akkor is kell hello IoT-központ kapcsolati karakterláncot a központ, amely az hello található [Azure-portálon].

1. Hozzon létre egy üres nevű **sendcloudtodevicemessage**. A hello **sendcloudtodevicemessage** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```shell
    npm init
    ```
2. A parancssorban hello **sendcloudtodevicemessage** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **SendCloudToDeviceMessage.js** hello fájlban **sendcloudtodevicemessage** mappa.
4. Adja hozzá a következő hello `require` hello utasításokat elejére hello **SendCloudToDeviceMessage.js** fájlt:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Adja hozzá a következő kód túl hello**SendCloudToDeviceMessage.js** fájlt. Hello "{iot hub kapcsolati karakterlánc}" helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc hello létrehozott hello központ hello [Ismerkedés az IoT-központ] oktatóanyag. Hello "{eszközazonosító}" helyőrzőt cserélje le hello Eszközazonosító hello hozzáadott hello eszköz [Ismerkedés az IoT-központ] oktatóanyag:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. Adja hozzá a következő függvény tooprint művelet eredmények toohello konzol hello:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Adja hozzá a következő függvény tooprint kézbesítési visszajelzés üzenetek toohello konzol hello:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Adja hozzá a hello alábbi code toosend egy üzenet tooyour eszközt, és kezelni a visszajelzés üdvözlőüzenetére hello eszköz elismeri üdvözlőüzenetére felhő eszközre:
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud toodevice message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. Mentse és zárja be **SendCloudToDeviceMessage.js** fájlt.

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához
Most már áll készen toorun hello alkalmazások.

1. Parancssorba hello hello **simulateddevice** mappa, futtassa a következő hello toosend telemetriai tooIoT Hub és a felhő-eszközre küldött üzenetek toolisten parancsot:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Hello szimulált eszköz alkalmazás futtatása][img-simulated-device]
2. A hello parancsot a parancssorba **sendcloudtodevicemessage** mappa, futtassa a következő parancs toosend hello felhő eszközre üzenet, és várjon, amíg hello visszaigazolás-visszajelzés:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Hello app toosend hello felhő eszközre parancs futtatása][img-send-command]
   
   > [!NOTE]
   > Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].
   > 
   > 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására. 

teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].

További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portálon]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
