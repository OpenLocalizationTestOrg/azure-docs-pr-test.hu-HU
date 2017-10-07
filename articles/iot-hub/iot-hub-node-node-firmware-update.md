---
title: "az Azure IoT Hub (csomópont) aaaDevice belső vezérlőprogram frissítése |} Microsoft Docs"
description: "Hogyan toouse kezelés az Azure IoT Hub tooinitiate egy eszköz belső vezérlőprogram frissítése. A szimulált eszköz alkalmazásának és a service-alkalmazást, amely elindítja a hello belső vezérlőprogram frissítési hello Azure IoT SDK-k használata Node.js tooimplement."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: juanpere
ms.openlocfilehash: 99d4b369e7aba334bf713e0c657e6e5d227fb691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a>Használja az eszköz felügyeleti tooinitiate eszköz vezérlőprogram-frissítés (csomópontok /)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Bevezetés
A hello [Ismerkedés az eszközkezelés] [ lnk-dm-getstarted] az oktatóanyagban megtudhatta, hogyan toouse hello [eszköz iker] [ lnk-devtwin] és [közvetlen módszerek] [ lnk-c2dmethod] primitívek tooremotely eszköz újraindul. A oktatóanyag használja az ugyanazon az IoT-központ primitívek hello és nyújt útmutatást, és bemutatja, hogyan toodo egy-végpontok szimulált vezérlőprogram-frissítés.  Ebben a mintában hello Intel Edison eszköz minta hello belső vezérlőprogram frissítési végrehajtására szolgál.

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy Node.js-Konzolalkalmazás, amely behívja hello firmwareUpdate közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.
* Hozzon létre egy szimulált eszköz alkalmazást, amely egy **firmwareUpdate** közvetlen módszer. Ez a módszer indít el egy többlépcsős folyamat, amely toodownload hello belső vezérlőprogram kép vár, hello belső vezérlőprogram lemezképet letölti és végül a hello belső vezérlőprogram kép vonatkozik. Minden egyes hello frissítés szakaszában hello eszköz által használt hello jelentett tulajdonságok tooreport folyamatban van.

Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:

**dmpatterns_fwupdate_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello válasz, és rendszeres időközönként (minden 500ms) frissítése megjeleníti hello jelentett tulajdonságok.

**dmpatterns_fwupdate_device.js**, tooyour IoT-központ köti össze a korábban létrehozott hello eszközidentitás firmwareUpdate közvetlen metódus kap, egy több államot folyamat toosimulate keresztül futtat, a belső vezérlőprogram frissítési többek között: hello vár Kép letöltése, hello új lemezkép letöltése, és végül a hello kép telepítése.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* NODE.js-verzió 0.12.x vagy újabb verzióját, <br/>  [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

Hajtsa végre a hello [Ismerkedés az eszközkezelés](iot-hub-node-node-device-management-get-started.md) cikk toocreate az IoT hub és az IoT-központ kapcsolati karakterláncot.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Indítás, közvetlen metódussal hello eszközön távoli vezérlőprogram-frissítés
Ebben a szakaszban egy eszköz távoli vezérlőprogram-frissítés kezdeményező Node.js-Konzolalkalmazás létrehozása. hello alkalmazása használja-e a közvetlen módszer tooinitiate hello frissítése és által használt eszköz iker lekérdezések tooperiodically hello aktív belső vezérlőprogram frissítése hello állapotának beolvasása.

1. Hozzon létre egy üres nevű **triggerfwupdateondevice**.  A hello **triggerfwupdateondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.  Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```
2. A parancssorban hello **triggerfwupdateondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-iot-központ** és **azure-iot-eszközök – mqtt** eszköz SDK-csomagot:
   
    ```
    npm install azure-iothub --save
    ```
3. Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_service.js** hello fájlban **triggerfwupdateondevice** mappa.
4. Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_service.js** fájlt:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Adja hozzá a következő hello toofind funkciót, és hello firmwareUpdate hello értékének megjelenítésére jelentett tulajdonság.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Adja hozzá a következő függvény tooinvoke hello firmwareUpdate metódus tooreboot hello céleszköz hello:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start hello firmware update on hello device: ' + err.message)
        } 
      });
    };
    ```
8. Végezetül Hozzáadás hello alábbi toocode toostart hello belső vezérlőprogram frissítési sorrend működik, és rendszeres időközönként megjelenjen hello jelentett tulajdonságok:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Mentse és zárja be a hello **dmpatterns_fwupdate_service.js** fájlt.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Parancssorba hello hello **triggerfwupdateondevice** mappa, futtassa a következő parancs tootrigger hello távoli hello újraindul, és lekérdezi a hello eszköz iker toofind hello utolsó indítsa újra a számítógépet idő.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy közvetlen módszer tootrigger távoli használt belső vezérlőprogram frissítése egy eszközön, és a használt hello jelentett tulajdonságok toofollow hello hello belső vezérlőprogram frissítése folyamatban van.

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
