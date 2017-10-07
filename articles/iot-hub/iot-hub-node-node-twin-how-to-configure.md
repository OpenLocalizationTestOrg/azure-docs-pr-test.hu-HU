---
title: "aaaUse Azure IoT Hub iker tulajdonságai (csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooconfigure eszközök. A szimulált eszköz alkalmazás és egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs hello Azure IoT SDK-k használata Node.js tooimplement."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a>Szükségeskonfiguráció-alkalmazható tulajdonságok tooconfigure eszközöket (csomópont)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Ez az oktatóanyag végén hello hogy két Node.js konzol alkalmazásokat:

* **SimulateDeviceConfiguration.js**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotának hello.
* **SetDesiredConfigurationAndQuery.js**, a Node.js háttér-alkalmazás, amely hello szükséges konfigurációs egy eszközön, és lekérdezések hello konfigurációs frissítési folyamat.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.
> 
> 

toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:

* A Node.js 0.10.x vagy újabb verziója.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

Ha követte hello [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**; pedig kihagyhatja toohello [ Hello szimulált eszköz alkalmazás létrehozása] [ lnk-how-to-configure-createapp] szakasz.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a>Hello szimulált eszköz alkalmazás létrehozása
Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések a szimulált hello konfigurációs frissítési folyamat.

1. Hozzon létre egy új üres nevű **simulatedeviceconfiguration**. A hello **simulatedeviceconfiguration** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **simulatedeviceconfiguration** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **SimulateDeviceConfiguration.js** hello fájlban **simulatedeviceconfiguration** mappa.
4. Adja hozzá a következő kód toohello hello **SimulateDeviceConfiguration.js** fájlt, és helyettesítő hello **{eszköz kapcsolati karakterlánc}** helyőrzőt kimásolt mikor hello eszköz kapcsolati karakterláncot, hello létrehozott **myDeviceId** eszközidentitás:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Hello **ügyfél** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello eszközről. hello előző kód után inicializálja a hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId**, és csatolja a kívánt tulajdonságok hello frissítés kezelőjét. hello kezelő ellenőrzi, hogy egy tényleges Helykonfiguráció-változtatási kérelem hello configIds összehasonlításával, akkor hív meg, amely elindítja a hello konfigurációváltozás metódus.
   
    Vegye figyelembe, hogy hello szakét az egyszerűség, a hello előző kód használja a kódolt alapértelmezett hello kezdeti konfiguráció. Egy valós alkalmazás valószínűleg szeretné, hogy a konfigurálás betöltése a helyi tárterület.
   
   > [!IMPORTANT]
   > Kívánt tulajdonság állapotváltozási események mindig kibocsátott egyszer eszköz csatlakozáskor, győződjön meg arról, hogy nincs-e egy tényleges módosítása a hello toocheck szükséges tulajdonságok bármilyen művelet végrehajtása előtt.
   > 
   > 
5. Adja hozzá a következő módszerek előtt hello hello `client.open()` hívása:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Hello **initConfigChange** metódus frissítések tulajdonságok jelentett hello helyi eszköz a két objektum hello konfigurációs frissítési kérelmet, és készletek hello állapot túl**függőben lévő**, majd a frissítések hello eszköz hello szolgáltatásban iker. Miután sikeresen frissített hello eszköz két, a hosszú ideig futó folyamat. a hello végrehajtásának leállítása szimulálja **completeConfigChange**. A metódus frissítések hello helyi eszköz iker által jelentett hello állapotának beállításakor túl tulajdonságok**sikeres** és hello eltávolítása **pendingConfig** objektum. Majd frissíti a hello eszköz iker hello szolgáltatásban.
   
    Megjegyzés: a toosave sávszélesség, hogy csak a hello tulajdonságok toobe módosított megadásával tulajdonságának frissítésekor (nevű **javítás** hello kód fent található), teljes dokumentum hello felülírása helyett.
   
   > [!NOTE]
   > Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben. Néhány konfigurációs frissítési folyamat előfordulhat, hogy képes tooaccommodate módosításainak konfigurációjához hello frissítés futása közben, mások lehet őket, és mások sikerült utasítsa el azokat a hibaállapotot tooqueue. Győződjön meg arról, hogy tooconsider hello kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logika hello hello konfigurációváltozás kezdeményezése előtt.
   > 
   > 
6. Hello eszköz alkalmazás futtatása:
   
        node SimulateDeviceConfiguration.js
   
    Hello üzenet `retrieved device twin`. Folyamatosan futó hello alkalmazást.

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása
Ez a szakasz során létrehoz egy Node.js-Konzolalkalmazás, hogy a frissítések hello *szükségeskonfiguráció-tulajdonságok* a hello eszköz iker társított **myDeviceId** új telemetria-konfigurációs objektum. Ezután hello eszköz twins hello IoT-központ tárolt lekérdezi és hello hello különbségének szükséges, és a jelentett hello eszköz konfigurációját jeleníti meg.

1. Hozzon létre egy új üres nevű **setdesiredandqueryapp**. A hello **setdesiredandqueryapp** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **setdesiredandqueryapp** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **SetDesiredAndQuery.js** hello fájlban **addtagsandqueryapp** mappa.
4. Adja hozzá a következő kód toohello hello **SetDesiredAndQuery.js** fájlt, és helyettesítő hello **{iot hub kapcsolati karakterlánc}** helyőrzőt hello a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánc :
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból. hello előző kód után inicializálja a hello **beállításjegyzék** objektum beolvassa az eszköz iker hello **myDeviceId**, és frissíti a kívánt tulajdonságát egy új telemetriai konfigurációs objektuma. Ezt követően meghívja a hello **queryTwins** működéséhez esemény 10 másodperc.

    > [!IMPORTANT]
    > Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál. Használja több eszközt, és nem toodetect módosítások toogenerate felhasználók számára is elérhető jelentések lekérdezi. Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].
    > 
    >.

1. Adja hozzá a következő kódot közvetlenül előtt hello hello `registry.getDeviceTwin()` meghívása tooimplement hello **queryTwins** függvény:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    hello előző kód lekérdezések hello eszköz twins hello IoT-központ tárolja és megrendelése hello szükséges, és telemetriai konfigurációk jelentett. Tekintse meg a toohello [IoT-központ lekérdezési nyelv] [ lnk-query] toolearn hogyan toogenerate gazdag jelent az eszközön.
2. A **SimulateDeviceConfiguration.js** hello alkalmazás fut, futtassa:
   
        node SetDesiredAndQuery.js 5m
   
    Megtekintheti az hello jelentett konfigurációs módosítást a **sikeres** túl**függőben lévő** túl**sikeres** újra hello új aktív küldés gyakoriság öt perc 24 helyett. óra.
   
   > [!IMPORTANT]
   > Nincs késleltetést mentése tooa perc közötti hello eszköz jelentés művelet és hello lekérdezés eredménye. Ez a tooenable hello lekérdezés infrastruktúra toowork nagyon nagy méretekben. egy egyetlen eszközt iker tooretrieve konzisztens nézeteinek hello használata **getDeviceTwin** metódus a hello **beállításjegyzék** osztály.
   > 
   > 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* egy háttér-alkalmazásból, és a szimulált eszköz alkalmazás toodetect, módosítása és a tag állapotaként reporting többlépéses frissítési folyamat szimulálása megírt  *Tulajdonságok jelentett* toohello eszköz iker.

A következő erőforrások toolearn hogyan használja hello számára:

* telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban
* ütemezhet, vagy hajtsa végre műveleteket a eszközök nagy mennyiségű, lásd: hello [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.
* interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
