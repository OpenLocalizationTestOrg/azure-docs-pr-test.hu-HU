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
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="a64b6-104">Az IoT-eszközön a Node.js közvetlen módszerekkel</span><span class="sxs-lookup"><span data-stu-id="a64b6-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="a64b6-105">Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a64b6-105">At hello end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="a64b6-106">**CallMethodOnDevice.js**, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a64b6-106">**CallMethodOnDevice.js**, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="a64b6-107">**SimulatedDevice.js**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és hello felhő által meghívott toohello metódus válaszol.</span><span class="sxs-lookup"><span data-stu-id="a64b6-107">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="a64b6-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="a64b6-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="a64b6-109">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a64b6-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="a64b6-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a64b6-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="a64b6-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a64b6-111">An active Azure account.</span></span> <span data-ttu-id="a64b6-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="a64b6-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="a64b6-113">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a64b6-113">Create a simulated device app</span></span>
<span data-ttu-id="a64b6-114">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa metódus hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a64b6-114">In this section, you create a Node.js console app that responds tooa method called by hello cloud.</span></span>

1. <span data-ttu-id="a64b6-115">Hozzon létre egy új, **simulateddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="a64b6-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="a64b6-116">A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="a64b6-116">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a64b6-117">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a64b6-117">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a64b6-118">A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="a64b6-118">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="a64b6-119">Egy szövegszerkesztő használatával hozzon létre egy új **SimulatedDevice.js** hello fájlban **simulateddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="a64b6-119">Using a text editor, create a new **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="a64b6-120">Adja hozzá a következő hello `require` hello utasításokat elejére hello **SimulatedDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="a64b6-120">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="a64b6-121">Adja hozzá a **connectionString** változó, és toocreate használni egy **DeviceClient** példány.</span><span class="sxs-lookup"><span data-stu-id="a64b6-121">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="a64b6-122">Cserélje le **{eszköz kapcsolati karakterlánc}** hello eszköz kapcsolati karakterlánccal hozta létre a hello *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="a64b6-122">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="a64b6-123">Adja hozzá a következő függvény tooimplement hello metódus hello eszközön hello:</span><span class="sxs-lookup"><span data-stu-id="a64b6-123">Add hello following function tooimplement hello method on hello device:</span></span>
   
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
7. <span data-ttu-id="a64b6-124">Nyissa meg a hello kapcsolat tooyour IoT-központot, és inicializálási hello metódus figyelő elindítása:</span><span class="sxs-lookup"><span data-stu-id="a64b6-124">Open hello connection tooyour IoT hub and start initialize hello method listener:</span></span>
   
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
8. <span data-ttu-id="a64b6-125">Mentse és zárja be a hello **SimulatedDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="a64b6-125">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="a64b6-126">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="a64b6-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="a64b6-127">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="a64b6-127">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="a64b6-128">A metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="a64b6-128">Call a method on a device</span></span>
<span data-ttu-id="a64b6-129">Ebben a szakaszban hozzon létre egy Node.js-Konzolalkalmazás, amely egy metódust hívja be hello szimulált eszköz alkalmazásának és hello válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="a64b6-129">In this section, you create a Node.js console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="a64b6-130">Hozzon létre egy új üres nevű **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="a64b6-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="a64b6-131">A hello **callmethodondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="a64b6-131">In hello **callmethodondevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a64b6-132">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a64b6-132">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a64b6-133">A parancssorban hello **callmethodondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="a64b6-133">At your command prompt in hello **callmethodondevice** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="a64b6-134">Egy szövegszerkesztő használatával hozzon létre egy **CallMethodOnDevice.js** hello fájlban **callmethodondevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="a64b6-134">Using a text editor, create a **CallMethodOnDevice.js** file in hello **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="a64b6-135">Adja hozzá a következő hello `require` hello utasításokat elejére hello **CallMethodOnDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="a64b6-135">Add hello following `require` statements at hello start of hello **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="a64b6-136">Adja hozzá a következő változó deklarációjában hello és hello helyőrző értékét lecserélheti egy hello IoT-központ kapcsolati karakterlánca:</span><span class="sxs-lookup"><span data-stu-id="a64b6-136">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="a64b6-137">Hello ügyfél tooopen hello kapcsolat tooyour IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a64b6-137">Create hello client tooopen hello connection tooyour IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="a64b6-138">Adja hozzá a következő függvény tooinvoke hello eszköz metódus és a nyomtató hello eszköz válasz toohello konzol hello:</span><span class="sxs-lookup"><span data-stu-id="a64b6-138">Add hello following function tooinvoke hello device method and print hello device response toohello console:</span></span>
   
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
8. <span data-ttu-id="a64b6-139">Mentse és zárja be a hello **CallMethodOnDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="a64b6-139">Save and close hello **CallMethodOnDevice.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="a64b6-140">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="a64b6-140">Run hello apps</span></span>
<span data-ttu-id="a64b6-141">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="a64b6-141">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="a64b6-142">A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toostart metódushívások az IoT-központ figyel hello:</span><span class="sxs-lookup"><span data-stu-id="a64b6-142">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="a64b6-143">A parancssorba a hello **callmethodondevice** mappa, futtassa a következő parancs toobegin az IoT hub figyelési hello:</span><span class="sxs-lookup"><span data-stu-id="a64b6-143">At a command prompt in hello **callmethodondevice** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="a64b6-144">Hello eszköz reagálni toohello metódus által üdvözlőüzenetére és hello alkalmazást, amely hello metódus megjelenítési hello válasz nevű hello eszközről nyomtasson jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a64b6-144">You will see hello device react toohello method by printing out hello message and hello application which called hello method display hello response from hello device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="a64b6-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a64b6-145">Next steps</span></span>
<span data-ttu-id="a64b6-146">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="a64b6-146">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="a64b6-147">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta.</span><span class="sxs-lookup"><span data-stu-id="a64b6-147">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="a64b6-148">Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="a64b6-148">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="a64b6-149">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="a64b6-149">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="a64b6-150">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="a64b6-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="a64b6-151">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="a64b6-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="a64b6-152">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a64b6-152">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
