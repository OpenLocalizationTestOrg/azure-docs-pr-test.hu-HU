---
title: "aaaGet Azure IoT Hub eszköz twins (csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooadd címkéket, majd az IoT Hub-lekérdezést. Használhatja az Azure IoT SDK-k hello Node.js tooimplement hello szimulált eszköz alkalmazás és egy szolgáltatás-alkalmazást, amely hello címkék hozzáadása, és az IoT-központ lekérdezés hello futtatja."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: d60b8c3de85e9285e496b86e27d4ee31a0554a1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-node"></a>Ismerkedés az eszköz twins (csomópont)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Ez az oktatóanyag végén hello hogy két Node.js konzol alkalmazásokat:

* **AddTagsAndQuery.js**, a Node.js háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.
* **TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely olyan eszköz, amely tooyour IoT-központ a korábban létrehozott hello eszközidentitás szimulálja, és jelenti a kapcsolat állapotát.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.
> 
> 

toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:

* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása
Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely hely metaadatok toohello eszköz iker társított **myDeviceId**. Ezután hello eszköz twins hello IoT-központ hello található hello eszközök kiválasztása tárolja SZÁMUNKRA, és jelentik a mobilhálózat kapcsolatot, majd hello lekérdezések.

1. Hozzon létre egy új üres nevű **addtagsandqueryapp**. A hello **addtagsandqueryapp** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **addtagsandqueryapp** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:
   
    ```
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **AddTagsAndQuery.js** hello fájlban **addtagsandqueryapp** mappa.
4. Adja hozzá a következő kód toohello hello **AddTagsAndQuery.js** fájlt, és helyettesítő hello **{iot hub kapcsolati karakterlánc}** helyőrzőt hello a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánc:
   
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   
    Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból. hello előző kód először inicializálja hello **beállításjegyzék** objektumot, majd beolvassa az eszköz iker hello **myDeviceId**, és végül frissíti a címkék szükséges hello helyére vonatkozó információkat.
   
    Hello után hello frissítése címkéket az hívások hello **queryTwins** függvény.
5. Adja hozzá a következő kódot a hello végén hello **AddTagsAndQuery.js** tooimplement hello **queryTwins** függvény:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    hello előző kód két lekérdezést hajt végre: először a választ csak hello eszköz twins hello található eszközök hello **Redmond43** gépek és hello második refines hello lekérdezés tooselect csak hello csatlakozó eszközöket is keresztül mobilhálózati.
   
    Vegye figyelembe, hogy hello előző kóddal, amikor hello létrehozza **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg. Hello **lekérdezés** objektum tartalmaz egy **hasMoreResults** használható tooinvoke hello logikai tulajdonság **nextAsTwin** módszerek összes tooretrieve eredmények több alkalommal. A metódus hívása **következő** eredmények, amelyek az eszköz twins például összesítési lekérdezések eredményeit nem érhető el.
6. Futtassa az alkalmazást hello:
   
        node AddTagsAndQuery.js
   
    A hello eredményei egy eszközhöz hello lekérdezés kérése minden eszköz a mappában lévő kell megjelennie **Redmond43** nincs hello lekérdezés, amely korlátozza a hello mobilhálózati használó toodevices elkészítéséig.
   
    ![][1]

Hello a következő szakaszban egy eszköz-alkalmazást, amely jelent hello csatlakozási adatokat hoz létre, és módosításokat hello hello előző szakaszban hello lekérdezés eredménye.

## <a name="create-hello-device-app"></a>Hello eszköz alkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**, és majd a frissítések az eszköz két által jelentett tulajdonságok toocontain hello információt, hogy használatával mobilhálózathoz csatlakozik.

> [!NOTE]
> Ilyenkor eszköz twins elérhetők, csak az eszközök, tooIoT központi csatlakozás hello MQTT protokoll használatával. Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.
> 
> 

1. Hozzon létre egy új üres nevű **reportconnectivity**. A hello **reportconnectivity** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **reportconnectivity** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag :
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** hello fájlban **reportconnectivity** mappa.
4. Adja hozzá a következő kód toohello hello **ReportConnectivity.js** fájlt, és helyettesítő hello **{eszköz kapcsolati karakterlánc}** helyőrző hello eszköz kapcsolati karakterlánccal másolt hello létrehozásakor **myDeviceId** eszközidentitás:
   
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
5. Hello eszköz alkalmazás futtatása
   
        node ReportConnectivity.js
   
    Hello üzenet `twin state reported`.
6. Most, hogy hello eszköz jelentette a kapcsolati információ meg kell jelennie mindkét lekérdezések. Nyissa meg újra a hello **addtagsandqueryapp** hello mappa, és futtassa le újra:
   
        node AddTagsAndQuery.js
   
    Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.
   
    ![][3]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és a szimulált eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a. Azt is megtanulta, hogyan tooquery ezt az információt hello SQL-szerű IoT Hub lekérdezési nyelv.

A következő erőforrások toolearn hogyan használja hello számára:

* telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban
* eszközök, eszköz iker kívánt tulajdonságok használata hello konfigurálása [használata szükséges tulajdonságok tooconfigure eszközök] [ lnk-twin-how-to-configure] oktatóanyagban
* interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
