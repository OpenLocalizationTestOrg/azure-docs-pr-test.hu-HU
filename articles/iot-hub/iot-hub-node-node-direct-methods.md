---
title: "Az Azure IoT Hub közvetlen módszerek (csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub közvetlen módszerek. Az Azure IoT SDK for Node.js használatával megvalósítható a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és a service-alkalmazást, amely hívja meg a közvetlen módszer."
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
ms.openlocfilehash: 83725c3ae3fd3807f2469be888e270ba078a8972
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="c7f93-104">Az IoT-eszközön a Node.js közvetlen módszerekkel</span><span class="sxs-lookup"><span data-stu-id="c7f93-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="c7f93-105">Ez az oktatóanyag végén két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="c7f93-105">At the end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="c7f93-106">**CallMethodOnDevice.js**, amely egy metódus meghívja a szimulált eszköz alkalmazás és a válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="c7f93-106">**CallMethodOnDevice.js**, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="c7f93-107">**SimulatedDevice.js**, amely a korábban létrehozott eszközidentitás az IoT hub kapcsolódik, és válaszol-e a felhő által meghívott metódus.</span><span class="sxs-lookup"><span data-stu-id="c7f93-107">**SimulatedDevice.js**, which connects to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="c7f93-108">Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="c7f93-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="c7f93-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c7f93-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="c7f93-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c7f93-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="c7f93-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c7f93-111">An active Azure account.</span></span> <span data-ttu-id="c7f93-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="c7f93-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="c7f93-113">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7f93-113">Create a simulated device app</span></span>
<span data-ttu-id="c7f93-114">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a felhő által meghívott metódus hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7f93-114">In this section, you create a Node.js console app that responds to a method called by the cloud.</span></span>

1. <span data-ttu-id="c7f93-115">Hozzon létre egy új, **simulateddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="c7f93-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="c7f93-116">A **simulateddevice** mappában hozza létre a package.json fájlt úgy, hogy beírja a következő parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="c7f93-116">In the **simulateddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="c7f93-117">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="c7f93-117">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c7f93-118">Telepítse az **azure-iot-device** eszközoldali SDK csomagot és az **azure-iot-device-mqtt** csomagot. Ehhez futtassa a parancssorban a következő parancsot a **simulateddevice** mappában:</span><span class="sxs-lookup"><span data-stu-id="c7f93-118">At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="c7f93-119">Egy szövegszerkesztővel hozzon létre egy új **SimulatedDevice.js** fájlt a **simulateddevice** mappában.</span><span class="sxs-lookup"><span data-stu-id="c7f93-119">Using a text editor, create a new **SimulatedDevice.js** file in the **simulateddevice** folder.</span></span>
4. <span data-ttu-id="c7f93-120">Adja hozzá a következő `require` utasításokat a **SimulatedDevice.js** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="c7f93-120">Add the following `require` statements at the start of the **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="c7f93-121">Adja hozzá a **connectionString** változó, és hozzon létre egy **DeviceClient** példány.</span><span class="sxs-lookup"><span data-stu-id="c7f93-121">Add a **connectionString** variable and use it to create a **DeviceClient** instance.</span></span> <span data-ttu-id="c7f93-122">Cserélje le **{eszköz kapcsolati karakterlánc}** az eszköz kapcsolattal karakterlánc Ön hozott létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="c7f93-122">Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="c7f93-123">Adja hozzá a következő függvény az eszközön a metódus végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="c7f93-123">Add the following function to implement the method on the device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="c7f93-124">Nyissa meg a kapcsolatot az IoT-központ és kezdő inicializálni a metódus figyelőt:</span><span class="sxs-lookup"><span data-stu-id="c7f93-124">Open the connection to your IoT hub and start initialize the method listener:</span></span>
   
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
8. <span data-ttu-id="c7f93-125">Mentse és zárja be a **SimulatedDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c7f93-125">Save and close the **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="c7f93-126">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="c7f93-126">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="c7f93-127">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), az MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="c7f93-127">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="c7f93-128">A metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="c7f93-128">Call a method on a device</span></span>
<span data-ttu-id="c7f93-129">Ebben a szakaszban hozzon létre egy Node.js-Konzolalkalmazás, amely egy metódus meghívja a szimulált eszköz alkalmazás és a válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="c7f93-129">In this section, you create a Node.js console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="c7f93-130">Hozzon létre egy új üres nevű **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="c7f93-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="c7f93-131">Az a **callmethodondevice** mappa, hozzon létre egy package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="c7f93-131">In the **callmethodondevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="c7f93-132">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="c7f93-132">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c7f93-133">A parancssorba a **callmethodondevice** mappa telepítéséhez a következő parancsot a **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="c7f93-133">At your command prompt in the **callmethodondevice** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="c7f93-134">Egy szövegszerkesztő használatával hozzon létre egy **CallMethodOnDevice.js** fájlt a **callmethodondevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="c7f93-134">Using a text editor, create a **CallMethodOnDevice.js** file in the **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="c7f93-135">Adja hozzá a következő `require` elején utasítások a **CallMethodOnDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="c7f93-135">Add the following `require` statements at the start of the **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="c7f93-136">Adja hozzá a következő változódeklarációt, és a helyőrző értékét cserélje le az IoT Hub kapcsolati karakterláncára:</span><span class="sxs-lookup"><span data-stu-id="c7f93-136">Add the following variable declaration and replace the placeholder value with the IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="c7f93-137">Az ügyfél számára, nyissa meg a kapcsolatot az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c7f93-137">Create the client to open the connection to your IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="c7f93-138">Adja hozzá a következő működnek, mint az eszköz metódus meghívása, és nyomtassa ki az eszköz válasza a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="c7f93-138">Add the following function to invoke the device method and print the device response to the console:</span></span>
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. <span data-ttu-id="c7f93-139">Mentse és zárja be a **CallMethodOnDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c7f93-139">Save and close the **CallMethodOnDevice.js** file.</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="c7f93-140">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="c7f93-140">Run the apps</span></span>
<span data-ttu-id="c7f93-141">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="c7f93-141">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="c7f93-142">A parancsot a parancssorba a **simulateddevice** mappa megkezdeni a figyelést az IoT Hub a metódushívások a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c7f93-142">At a command prompt in the **simulateddevice** folder, run the following command to start listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="c7f93-143">A parancsot a parancssorba a **callmethodondevice** mappa, a következő parancsot az IoT hub figyelés megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="c7f93-143">At a command prompt in the **callmethodondevice** folder, run the following command to begin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="c7f93-144">A metódus által az üzenet és az alkalmazást, amely az eszközről a válasz a metódus megjelenítési nevű nyomtasson reagálni az eszköz jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c7f93-144">You will see the device react to the method by printing out the message and the application which called the method display the response from the device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="c7f93-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7f93-145">Next steps</span></span>
<span data-ttu-id="c7f93-146">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="c7f93-146">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="c7f93-147">Ez az eszközazonosító segítségével engedélyezheti a felhő által meghívott módszerek reagálni a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c7f93-147">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="c7f93-148">Létrehozott egy alkalmazást, amely meghívja a módszerek az eszközön, és megjeleníti az eszköz válaszára is.</span><span class="sxs-lookup"><span data-stu-id="c7f93-148">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="c7f93-149">További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:</span><span class="sxs-lookup"><span data-stu-id="c7f93-149">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="c7f93-150">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="c7f93-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="c7f93-151">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="c7f93-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="c7f93-152">Megtudhatja, hogyan terjeszthető ki az IoT-megoldás és az ütemezések metódushívások több eszközön, tekintse meg a [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c7f93-152">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
