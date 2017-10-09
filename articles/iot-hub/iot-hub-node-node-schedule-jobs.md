---
title: "az Azure IoT Hub (csomópont) aaaSchedule feladatok |} Microsoft Docs"
description: "Hogyan tooschedule az Azure IoT-központ feladat tooinvoke a közvetlen módszer több eszközön. Hello Azure IoT SDK-k használata Node.js tooimplement hello szimulált eszköz alkalmazások és a app toorun hello feladata."
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
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: be293362447fbcddaa3433b66f208f22545fe0c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a>Ütemezés és a feladatok (csomópont)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi egy háttér-alkalmazás toocreate és nyomon követése feladatok ütemezése és eszközök millióira frissítése.  A következő műveletek hello feladatok használható:

* Eszköz kívánt tulajdonságainak frissítése
* Címkék frissítése
* Közvetlen metódusok

Fogalmilag egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi hello való összevetéssel az eszközök, eszköz két lekérdezést által definiált végrehajtás folyamatban van.  Például egy háttér-alkalmazás használhat egy feladat tooinvoke újraindítás metódus 10 000 eszköz, a két eszköz lekérdezés által megadott és ütemezett egy későbbi időpontban.  Az alkalmazás egyes eszközök fogadni és hello újraindítás metódus hajtható végre, majd előrehaladásának.

További információ az egyes képességek a cikkeiben:

* A két eszköz és a tulajdonságok: [Ismerkedés az eszköz twins] [ lnk-get-started-twin] és [oktatóanyag: hogyan toouse eszköz iker tulajdonságai][lnk-twin-props]
* közvetlen módszerek: [IoT Hub fejlesztői útmutató - közvetlen módszerek] [ lnk-dev-methods] és [oktatóanyag: közvetlen módszer][lnk-c2d-methods]

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy közvetlen metódus, amely lehetővé teszi a szimulált eszköz alkalmazást **lockDoor** hello megoldás háttérrendszerének által hívható.
* Hozzon létre egy Node.js-Konzolalkalmazás, hogy hívások hello **lockDoor** közvetlen módszer használatával egy feladat és a frissítések hello hello szimulált eszköz alkalmazásban szükséges tulajdonságok eszköz feladat használatával.

Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:

**simDevice.js**, amelyhez csatlakoznak az IoT-központ tooyour hello eszközidentitás, és megkapja a **lockDoor** közvetlen módszer.

**scheduleJobService.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazás és frissítés hello eszköz iker a kívánt tulajdonságokkal feladat használatával.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* NODE.js-verzió 0.12.x vagy újabb verzióját, <br/>  [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely tooa hello felhő, amely elindítja a szimulált eszköz újraindítás által meghívott közvetlen metódus válaszol, és használja hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása.

1. Hozzon létre egy új üres nevű **simDevice**.  A hello **simDevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **simDevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **simDevice.js** hello fájlban **simDevice** mappa.
4. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **simDevice.js** fájlt:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Adja hozzá a következő függvény toohandle hello hello **lockDoor** metódust.
   
    ```
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
7. Adja hozzá a következő kód tooregister hello kezelő hello hello **lockDoor** metódust.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Mentse és zárja be a hello **simDevice.js** fájlt.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Ütemezett feladatok közvetlen metódus hívása, és egy eszköz iker tulajdonságainak frissítése
Ebben a szakaszban egy távoli kezdeményező Node.js-Konzolalkalmazás létrehozása **lockDoor** egy eszközön, a közvetlen módszer és a frissítés hello eszköz iker tartozó tulajdonságok használatával.

1. Hozzon létre egy új üres nevű **scheduleJobService**.  A hello **scheduleJobService** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **scheduleJobService** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy új **scheduleJobService.js** hello fájlban **scheduleJobService** mappa.
4. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_gscheduleJobServiceetstarted_service.js** fájlt:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Adja hozzá a következő függvény, amely lesz hello feladat végrehajtásának használt toomonitor hello hello:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. Adja hozzá a következő kód tooschedule hello feladatot, amely hello eszköz metódust hello:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable tooprocess method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. Adja hozzá a következő kód tooschedule hello feladat tooupdate hello eszköz iker hello:
   
    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. Mentse és zárja be a hello **scheduleJobService.js** fájlt.

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához
Most már áll készen toorun hello alkalmazások.

1. Parancssorba hello hello **simDevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.
   
    ```
    node simDevice.js
    ```
2. Parancssorba hello hello **scheduleJobService** mappa, futtassa a következő parancs tootrigger hello feladatok toolock hello ajtó és frissítés hello iker hello
   
    ```
    node scheduleJobService.js
    ```
3. Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban használt a egy feladat tooschedule közvetlen módszer tooa eszköz és a hello eszköz iker tulajdonságok hello frissítése.

Ismerkedés az IoT-központ és az eszköz felügyeleti minták például távolról keresztül hello vezeték nélkül belső vezérlőprogram frissítési toocontinue lásd:

[Oktatóanyag: Hogyan toodo a belső vezérlőprogram frissítése][lnk-fwupdate]

Ismerkedés az IoT-központ toocontinue lásd [Ismerkedés az Azure IoT peremhálózati][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
