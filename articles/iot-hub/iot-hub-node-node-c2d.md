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
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a><span data-ttu-id="f188e-104">Az IoT-központ (csomópont) felhő eszközre-üzenetek</span><span class="sxs-lookup"><span data-stu-id="f188e-104">Send cloud-to-device messages with IoT Hub (Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="f188e-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f188e-105">Introduction</span></span>
<span data-ttu-id="f188e-106">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="f188e-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="f188e-107">Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.</span><span class="sxs-lookup"><span data-stu-id="f188e-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="f188e-108">Ez az oktatóanyag épül [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="f188e-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="f188e-109">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="f188e-109">It shows you how to:</span></span>

* <span data-ttu-id="f188e-110">A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.</span><span class="sxs-lookup"><span data-stu-id="f188e-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="f188e-111">Az eszközön a felhőből eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="f188e-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="f188e-112">A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="f188e-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="f188e-113">További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="f188e-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="f188e-114">Ez az oktatóanyag végén hello akkor futtatja a Node.js-konzol két alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="f188e-114">At hello end of this tutorial, you run two Node.js console apps:</span></span>

* <span data-ttu-id="f188e-115">**SimulatedDevice**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="f188e-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="f188e-116">**SendCloudToDeviceMessage**, amely IoT-központot egy felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazás küld, és majd megkapja a kézbesítési nyugtázási.</span><span class="sxs-lookup"><span data-stu-id="f188e-116">**SendCloudToDeviceMessage**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="f188e-117">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="f188e-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="f188e-118">Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="f188e-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>
> 
> 

<span data-ttu-id="f188e-119">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f188e-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="f188e-120">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f188e-120">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="f188e-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="f188e-121">An active Azure account.</span></span> <span data-ttu-id="f188e-122">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="f188e-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="f188e-123">A szimulált eszköz app hello üzeneteket fogadni</span><span class="sxs-lookup"><span data-stu-id="f188e-123">Receive messages in hello simulated device app</span></span>
<span data-ttu-id="f188e-124">Ebben a szakaszban létrehozott hello szimulált eszköz alkalmazásának módosítása [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.</span><span class="sxs-lookup"><span data-stu-id="f188e-124">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="f188e-125">Egy szövegszerkesztőben nyissa meg hello SimulatedDevice.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="f188e-125">Using a text editor, open hello SimulatedDevice.js file.</span></span>
2. <span data-ttu-id="f188e-126">Módosítsa a hello **connectCallback** az IoT-központ által küldött toohandle üzenetek működéséhez.</span><span class="sxs-lookup"><span data-stu-id="f188e-126">Modify hello **connectCallback** function toohandle messages sent from IoT Hub.</span></span> <span data-ttu-id="f188e-127">Ebben a példában hello eszköz mindig hív hello **teljes** toonotify, feldolgozta-e üdvözlőüzenetére IoT-Központ működéséhez.</span><span class="sxs-lookup"><span data-stu-id="f188e-127">In this example, hello device always invokes hello **complete** function toonotify IoT Hub that it has processed hello message.</span></span> <span data-ttu-id="f188e-128">Hello új verziójának **connectCallback** függvény a következő kódrészletet hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="f188e-128">Your new version of hello **connectCallback** function looks like hello following snippet:</span></span>
   
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
   > <span data-ttu-id="f188e-129">A HTTP használata helyett MQTT vagy AMQP hello átvitelhez, hello **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként).</span><span class="sxs-lookup"><span data-stu-id="f188e-129">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="f188e-130">MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="f188e-130">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="f188e-131">Felhő eszközre üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="f188e-131">Send a cloud-to-device message</span></span>
<span data-ttu-id="f188e-132">Ebben a szakaszban egy Node.js-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f188e-132">In this section, you create a Node.js console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="f188e-133">Eszközazonosító hello hozzáadott hello eszköz kell hello [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f188e-133">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="f188e-134">Akkor is kell hello IoT-központ kapcsolati karakterláncot a központ, amely az hello található [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="f188e-134">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="f188e-135">Hozzon létre egy üres nevű **sendcloudtodevicemessage**.</span><span class="sxs-lookup"><span data-stu-id="f188e-135">Create an empty folder called **sendcloudtodevicemessage**.</span></span> <span data-ttu-id="f188e-136">A hello **sendcloudtodevicemessage** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f188e-136">In hello **sendcloudtodevicemessage** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="f188e-137">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f188e-137">Accept all hello defaults:</span></span>
   
    ```shell
    npm init
    ```
2. <span data-ttu-id="f188e-138">A parancssorban hello **sendcloudtodevicemessage** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="f188e-138">At your command prompt in hello **sendcloudtodevicemessage** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```shell
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="f188e-139">Egy szövegszerkesztő használatával hozzon létre egy **SendCloudToDeviceMessage.js** hello fájlban **sendcloudtodevicemessage** mappa.</span><span class="sxs-lookup"><span data-stu-id="f188e-139">Using a text editor, create a **SendCloudToDeviceMessage.js** file in hello **sendcloudtodevicemessage** folder.</span></span>
4. <span data-ttu-id="f188e-140">Adja hozzá a következő hello `require` hello utasításokat elejére hello **SendCloudToDeviceMessage.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="f188e-140">Add hello following `require` statements at hello start of hello **SendCloudToDeviceMessage.js** file:</span></span>
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. <span data-ttu-id="f188e-141">Adja hozzá a következő kód túl hello**SendCloudToDeviceMessage.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="f188e-141">Add hello following code too**SendCloudToDeviceMessage.js** file.</span></span> <span data-ttu-id="f188e-142">Hello "{iot hub kapcsolati karakterlánc}" helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc hello létrehozott hello központ hello [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f188e-142">Replace hello "{iot hub connection string}" placeholder value with hello IoT Hub connection string for hello hub you created in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="f188e-143">Hello "{eszközazonosító}" helyőrzőt cserélje le hello Eszközazonosító hello hozzáadott hello eszköz [Ismerkedés az IoT-központ] oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="f188e-143">Replace hello "{device id}" placeholder with hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial:</span></span>
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="f188e-144">Adja hozzá a következő függvény tooprint művelet eredmények toohello konzol hello:</span><span class="sxs-lookup"><span data-stu-id="f188e-144">Add hello following function tooprint operation results toohello console:</span></span>
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="f188e-145">Adja hozzá a következő függvény tooprint kézbesítési visszajelzés üzenetek toohello konzol hello:</span><span class="sxs-lookup"><span data-stu-id="f188e-145">Add hello following function tooprint delivery feedback messages toohello console:</span></span>
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. <span data-ttu-id="f188e-146">Adja hozzá a hello alábbi code toosend egy üzenet tooyour eszközt, és kezelni a visszajelzés üdvözlőüzenetére hello eszköz elismeri üdvözlőüzenetére felhő eszközre:</span><span class="sxs-lookup"><span data-stu-id="f188e-146">Add hello following code toosend a message tooyour device and handle hello feedback message when hello device acknowledges hello cloud-to-device message:</span></span>
   
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
9. <span data-ttu-id="f188e-147">Mentse és zárja be **SendCloudToDeviceMessage.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="f188e-147">Save and close **SendCloudToDeviceMessage.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="f188e-148">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="f188e-148">Run hello applications</span></span>
<span data-ttu-id="f188e-149">Most már áll készen toorun hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f188e-149">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="f188e-150">Parancssorba hello hello **simulateddevice** mappa, futtassa a következő hello toosend telemetriai tooIoT Hub és a felhő-eszközre küldött üzenetek toolisten parancsot:</span><span class="sxs-lookup"><span data-stu-id="f188e-150">At hello command prompt in hello **simulateddevice** folder, run hello following command toosend telemetry tooIoT Hub and toolisten for cloud-to-device messages:</span></span>
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Hello szimulált eszköz alkalmazás futtatása][img-simulated-device]
2. <span data-ttu-id="f188e-152">A hello parancsot a parancssorba **sendcloudtodevicemessage** mappa, futtassa a következő parancs toosend hello felhő eszközre üzenet, és várjon, amíg hello visszaigazolás-visszajelzés:</span><span class="sxs-lookup"><span data-stu-id="f188e-152">At a command prompt in hello **sendcloudtodevicemessage** folder, run hello following command toosend a cloud-to-device message and wait for hello acknowledgment feedback:</span></span>
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Hello app toosend hello felhő eszközre parancs futtatása][img-send-command]
   
   > [!NOTE]
   > <span data-ttu-id="f188e-154">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="f188e-154">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="f188e-155">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="f188e-155">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="f188e-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f188e-156">Next steps</span></span>
<span data-ttu-id="f188e-157">Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="f188e-157">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="f188e-158">teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="f188e-158">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="f188e-159">További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="f188e-159">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

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
