---
title: "aaaGet Azure IoT Hub (csomópont) használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend eszköz-felhő üzenetek tooAzure IoT Hub IoT SDK for Node.js használatával. Szimulált eszköz és a szolgáltatás alkalmazások tooregister létrehozása az eszköz, üzenetküldés és üzenetek olvasni az IoT-központ."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0747895365f2359a9c38ea1e85a5881d6efec0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a>Csatlakozás a szimulált eszköz tooyour IoT hub csomópont használatával
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén hello három Node.js konzol alkalmazások közül választhat:

* **CreateDeviceIdentity.js**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect a szimulált eszköz alkalmazás.
* **ReadDeviceToCloudMessages.js**, amely megjeleníti a szimulált eszköz alkalmazás által küldött hello telemetriai.
* **SimulatedDevice.js**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és minden második használatával hello MQTT protokoll telemetriai üzenetet küld.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.
> 
> 

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Ezzel létrehozta az IoT Hubot. Hello IoT-központ állomásnév és az IoT-központ kapcsolati karakterláncot, hogy kell-e toocomplete hello rest oktatóanyag hello rendelkezik.

## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely létrehoz egy eszközidentitás hello identitásjegyzékhez a az IoT hub létrehozása. Eszköz csak tooIoT hub is elérheti, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez. További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity]. A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.

1. Hozzon létre egy új, **createdeviceidentity** nevű üres mappát. A hello **createdeviceidentity** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **createdeviceidentity** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** szolgáltatás SDK-csomagot:
   
    ```
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **CreateDeviceIdentity.js** hello fájlban **createdeviceidentity** mappa.
4. Adja hozzá a következő hello `require` hello hello indításkor utasítása **CreateDeviceIdentity.js** fájlt:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Adja hozzá a következő kód toohello hello **CreateDeviceIdentity.js** fájlt, és hello helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc hello előző szakaszban létrehozott hello központ hello: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Adja hozzá a következő kód toocreate hello identitás rendszerleíró adatbázis az IoT hub eszköz definíció hello. Ezt a kódot eszköz hoz létre, ha hello eszköz azonosítója nem létezik a hello identitásjegyzékhez, ellenkező esetben hello meglévő eszköz kulcsa hello adja vissza:
   
    ```
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. Mentse és zárja be a **CreateDeviceIdentity.js** fájlt.
8. toorun hello **createdeviceidentity** alkalmazás, a következő parancs parancssorba hello hello createdeviceidentity mappában hello hajtható végre:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Jegyezze fel a hello **Eszközazonosító** és **eszközkulcs**. Ezek az értékek később szüksége tooIoT eszközként központ kapcsolódó alkalmazásban létrehozásakor.

> [!NOTE]
> az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja. Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol. Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon. További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a>Az eszközről a felhőbe irányuló üzenetek fogadása
Ebben a szakaszban egy Node.js-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról. Az IoT-központ mutatja egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg. egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre. Hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] az oktatóanyag bemutatja, hogyan tooprocess eszköz-felhő léptékű üzenetek. Hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag további információkat biztosítanak a tooprocess Eseményközpontokból származó üzenetek és alkalmazható toohello IoT Hub Event Hub-kompatibilis végpontok.

> [!NOTE]
> hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.
> 
> 

1. Hozzon létre egy **readdevicetocloudmessages** nevű üres mappát. A hello **readdevicetocloudmessages** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **readdevicetocloudmessages** mappa, futtassa a következő parancs tooinstall hello hello **azure-eseményközpontok** csomag:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **ReadDeviceToCloudMessages.js** hello fájlban **readdevicetocloudmessages** mappa.
4. Adja hozzá a következő hello `require` hello utasításokat elejére hello **ReadDeviceToCloudMessages.js** fájlt:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Adja hozzá a következő változó deklarációjában hello és hello helyőrző értékét lecserélheti egy hello IoT-központ kapcsolati karakterlánca:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Adja hozzá a következő kimeneti toohello konzol nyomtatása két függvények hello:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Adja hozzá a következő kód toocreate hello hello **EventHubClient**, nyissa meg a hello kapcsolat tooyour IoT-központot, és minden partíció esetében fogadó létrehozása. Ez az alkalmazás egy szűrőt használ, amikor létrehozza a fogadó úgy, hogy hello fogadó csak olvassa be küldött üzenetek tooIoT Hub után hello fogadó elindul. Ez a szűrő akkor hasznos, tesztelési környezetben, ezért az imént hello aktuális üzenetek készletét. Éles környezetben a kód győződjön meg arról, hogy az összes köszönőüzenetei feldolgozza. További információkért lásd: hello [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-process-d2c-tutorial] oktatóanyag:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Mentse és zárja be a hello **ReadDeviceToCloudMessages.js** fájlt.

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld hoz létre.

1. Hozzon létre egy **simulateddevice** nevű üres mappát. A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **SimulatedDevice.js** hello fájlban **simulateddevice** mappa.
4. Adja hozzá a következő hello `require` hello utasításokat elejére hello **SimulatedDevice.js** fájlt:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány. Cserélje le **{youriothostname}** hello IoT-központ hello nevű hello létrehozott *létrehoz egy IoT-központot* szakasz. Cserélje le **{yourdevicekey}** hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Adja hozzá a következő függvény toodisplay kimeneti hello alkalmazásból hello:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Egy visszahívás létrehozásáról és használatáról hello **setInterval** toosend üzenet tooyour IoT-központ másodpercenként működik:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
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
8. Nyissa meg a hello kapcsolat tooyour IoT-központot, és üzenetek küldésének megkezdése:
   
    ```
    client.open(connectCallback);
    ```
9. Mentse és zárja be a hello **SimulatedDevice.js** fájlt.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].
> 
> 

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. A parancssorba a hello **readdevicetocloudmessages** mappa, futtassa a következő parancs toobegin az IoT hub figyelési hello:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![NODE.js IoT-központ szolgáltatás toomonitor eszközről a felhőbe alkalmazásüzenetek][7]
2. A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toobegin telemetriai adatok tooyour IoT-központ küldése hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![NODE.js IoT-központ eszközre app toosend eszközről a felhőbe küldött üzenetek][8]
3. Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:
   
    ![Az Azure portál használata csempe ábrázoló száma küldött üzenetek tooIoT Hub][43]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta. Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg. 

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Kapcsolódás az eszközhöz][lnk-connect-device]
* [Eszközfelügyelet – első lépések][lnk-device-management]
* [Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]

toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
