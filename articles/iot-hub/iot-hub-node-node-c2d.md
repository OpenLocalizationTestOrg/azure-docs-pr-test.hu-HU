---
title: "Az Azure IoT Hub (csomópont) felhő eszközre üzenetek |} Microsoft Docs"
description: "Hogyan küldhetők felhő-eszközre küldött üzenetek eszközökre az Azure IoT-központ az Azure IoT SDK for Node.js használatával. A szimulált eszköz alkalmazásnak, hogy a felhő-eszközre küldött üzenetek fogadására és módosíthat egy háttér-alkalmazást a felhőből eszközre küldéséhez módosítása."
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
ms.openlocfilehash: 4580bda5633f84a7c7af0dc85f3cea4951024836
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a><span data-ttu-id="4c7f4-104">Az IoT-központ (csomópont) felhő eszközre-üzenetek</span><span class="sxs-lookup"><span data-stu-id="4c7f4-104">Send cloud-to-device messages with IoT Hub (Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="4c7f4-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="4c7f4-105">Introduction</span></span>
<span data-ttu-id="4c7f4-106">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="4c7f4-107">A [Ismerkedés az IoT-központ] oktatóanyag bemutatja, hogyan létrehoz egy IoT-központot, azt egy eszközidentitás kiépítéséhez és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-107">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="4c7f4-108">Ez az oktatóanyag épül [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="4c7f4-109">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-109">It shows you how to:</span></span>

* <span data-ttu-id="4c7f4-110">A megoldás háttérrendszeréhez, az IoT-központ keresztül egyetlen eszközt felhő eszközre üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-110">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="4c7f4-111">Az eszközön a felhőből eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="4c7f4-112">A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) IoT-központot egy eszközre küldött üzenetek esetében.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="4c7f4-113">Felhő eszközre üzenetek további információt a [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-113">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="4c7f4-114">Ez az oktatóanyag végén, futtatja a Node.js-konzol két alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-114">At the end of this tutorial, you run two Node.js console apps:</span></span>

* <span data-ttu-id="4c7f4-115">**SimulatedDevice**, az alkalmazás létrehozása a módosított változatát [Ismerkedés az IoT-központ], amely csatlakozik az IoT hub és felhő eszközre üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-115">**SimulatedDevice**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="4c7f4-116">**SendCloudToDeviceMessage**, melyik felhőalapú eszközre üzenetet küld az IoT-központ szimulált eszköz alkalmazás, és a szállítási nyugtázási majd megkapja.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-116">**SendCloudToDeviceMessage**, which sends a cloud-to-device message to the simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="4c7f4-117">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="4c7f4-118">Csatlakoztassa az eszközt, az oktatóanyag kódot, és általában Azure IoT Hub kapcsolatos lépésenkénti útmutatót lásd: a [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-118">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [Azure IoT Developer Center].</span></span>
> 
> 

<span data-ttu-id="4c7f4-119">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-119">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="4c7f4-120">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-120">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="4c7f4-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-121">An active Azure account.</span></span> <span data-ttu-id="4c7f4-122">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="4c7f4-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-simulated-device-app"></a><span data-ttu-id="4c7f4-123">A szimulált eszköz alkalmazás üzeneteket fogadni</span><span class="sxs-lookup"><span data-stu-id="4c7f4-123">Receive messages in the simulated device app</span></span>
<span data-ttu-id="4c7f4-124">Ebben a szakaszban módosítsa a szimulált eszköz alkalmazás létrehozott [Ismerkedés az IoT-központ] felhő eszközre üzenetek fogadása az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-124">In this section, you modify the simulated device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="4c7f4-125">Egy szövegszerkesztőben nyissa meg a SimulatedDevice.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-125">Using a text editor, open the SimulatedDevice.js file.</span></span>
2. <span data-ttu-id="4c7f4-126">Módosítsa a **connectCallback** működnek, mint az IoT-központ által küldött üzenetek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-126">Modify the **connectCallback** function to handle messages sent from IoT Hub.</span></span> <span data-ttu-id="4c7f4-127">Ebben a példában az eszköz mindig meghívja a **teljes** függvény értesíteni az IoT-központot, feldolgozta-e az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-127">In this example, the device always invokes the **complete** function to notify IoT Hub that it has processed the message.</span></span> <span data-ttu-id="4c7f4-128">Új verziójának a **connectCallback** függvény néz ki a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-128">Your new version of the **connectCallback** function looks like the following snippet:</span></span>
   
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
        // Create a message and send it to the IoT Hub every second
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
   > <span data-ttu-id="4c7f4-129">Ha a átvitelhez MQTT vagy AMQP helyett a HTTP Protokollt használja. a **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként).</span><span class="sxs-lookup"><span data-stu-id="4c7f4-129">If you use HTTP instead of MQTT or AMQP as the transport, the **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="4c7f4-130">MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás közötti különbségekről további információkért lásd: a [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-130">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="4c7f4-131">Felhő eszközre üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="4c7f4-131">Send a cloud-to-device message</span></span>
<span data-ttu-id="4c7f4-132">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely felhő eszközre üzeneteket küld a szimulált eszköz alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-132">In this section, you create a Node.js console app that sends cloud-to-device messages to the simulated device app.</span></span> <span data-ttu-id="4c7f4-133">A hozzáadott eszköz az Eszközazonosítót van szüksége a [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-133">You need the device ID of the device you added in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="4c7f4-134">Az IoT-központ kapcsolati karakterlánc, amely megtalálható a központ is kell a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-134">You also need the IoT Hub connection string for your hub that you can find in the [Azure portal].</span></span>

1. <span data-ttu-id="4c7f4-135">Hozzon létre egy üres nevű **sendcloudtodevicemessage**.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-135">Create an empty folder called **sendcloudtodevicemessage**.</span></span> <span data-ttu-id="4c7f4-136">Az a **sendcloudtodevicemessage** mappa, hozzon létre egy package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-136">In the **sendcloudtodevicemessage** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="4c7f4-137">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-137">Accept all the defaults:</span></span>
   
    ```shell
    npm init
    ```
2. <span data-ttu-id="4c7f4-138">A parancssorba a **sendcloudtodevicemessage** mappa telepítéséhez a következő parancsot a **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-138">At your command prompt in the **sendcloudtodevicemessage** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```shell
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="4c7f4-139">Egy szövegszerkesztő használatával hozzon létre egy **SendCloudToDeviceMessage.js** fájlt a **sendcloudtodevicemessage** mappa.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-139">Using a text editor, create a **SendCloudToDeviceMessage.js** file in the **sendcloudtodevicemessage** folder.</span></span>
4. <span data-ttu-id="4c7f4-140">Adja hozzá a következő `require` elején utasítások a **SendCloudToDeviceMessage.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-140">Add the following `require` statements at the start of the **SendCloudToDeviceMessage.js** file:</span></span>
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. <span data-ttu-id="4c7f4-141">Adja hozzá a következő kódot a **SendCloudToDeviceMessage.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-141">Add the following code to **SendCloudToDeviceMessage.js** file.</span></span> <span data-ttu-id="4c7f4-142">Cserélje le a "{iot hub kapcsolati karakterlánc}" helyőrző értékét az IoT-központ kapcsolati karakterlánccal létrehozott központ a [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-142">Replace the "{iot hub connection string}" placeholder value with the IoT Hub connection string for the hub you created in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="4c7f4-143">A "{eszközazonosító}" helyőrzőt cserélje le a hozzáadott eszköz az Eszközazonosítót az [Ismerkedés az IoT-központ] oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-143">Replace the "{device id}" placeholder with the device ID of the device you added in the [Get started with IoT Hub] tutorial:</span></span>
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="4c7f4-144">Adja hozzá a következő függvény nyomtatni művelet eredményeit a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-144">Add the following function to print operation results to the console:</span></span>
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="4c7f4-145">Adja hozzá a következő függvény nyomtatni kézbesítési visszajelzés üzenetek a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-145">Add the following function to print delivery feedback messages to the console:</span></span>
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. <span data-ttu-id="4c7f4-146">Adja hozzá a következő kódot üzenetet küldeni az eszközt, és a visszajelzés üzenet kezelését, amikor az eszköz elfogadja a felhőből eszközre üzenet:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-146">Add the following code to send a message to your device and handle the feedback message when the device acknowledges the cloud-to-device message:</span></span>
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. <span data-ttu-id="4c7f4-147">Mentse és zárja be **SendCloudToDeviceMessage.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-147">Save and close **SendCloudToDeviceMessage.js** file.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="4c7f4-148">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="4c7f4-148">Run the applications</span></span>
<span data-ttu-id="4c7f4-149">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-149">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="4c7f4-150">A parancssorban a **simulateddevice** mappa, futtassa a következő parancsot, telemetriai adatokat küldhet az IoT-központot, és a felhő-eszközre küldött üzenetek figyelésére:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-150">At the command prompt in the **simulateddevice** folder, run the following command to send telemetry to IoT Hub and to listen for cloud-to-device messages:</span></span>
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![A szimulált eszköz alkalmazás futtatása][img-simulated-device]
2. <span data-ttu-id="4c7f4-152">A parancsot a parancssorba a **sendcloudtodevicemessage** mappa, a következő parancsot a felhőből eszközre küldött, és várjon, amíg a visszaigazolás-visszajelzés:</span><span class="sxs-lookup"><span data-stu-id="4c7f4-152">At a command prompt in the **sendcloudtodevicemessage** folder, run the following command to send a cloud-to-device message and wait for the acknowledgment feedback:</span></span>
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Futtassa az alkalmazásnak, hogy a felhő eszközre vonatkozó parancs küldése][img-send-command]
   
   > [!NOTE]
   > <span data-ttu-id="4c7f4-154">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-154">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="4c7f4-155">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), az MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-155">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="4c7f4-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c7f4-156">Next steps</span></span>
<span data-ttu-id="4c7f4-157">Ebben az oktatóprogramban megismerte felhő eszközre üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="4c7f4-157">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="4c7f4-158">Példák teljes végpontok közötti megoldások, amelyek használják az IoT-központot, lásd: [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-158">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="4c7f4-159">Az IoT hubbal megoldások fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="4c7f4-159">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

<span data-ttu-id="4c7f4-160">[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="4c7f4-160">[Get started with IoT Hub]: iot-hub-node-node-getstarted.md</span></span>
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
<span data-ttu-id="4c7f4-161">[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="4c7f4-161">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="4c7f4-162">[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="4c7f4-162">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
<span data-ttu-id="4c7f4-163">[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="4c7f4-163">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="4c7f4-164">[Azure-portálon]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="4c7f4-164">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="4c7f4-165">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span><span class="sxs-lookup"><span data-stu-id="4c7f4-165">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span></span>
