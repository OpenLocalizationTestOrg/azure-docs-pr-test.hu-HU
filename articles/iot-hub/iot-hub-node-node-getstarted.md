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
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a><span data-ttu-id="c2e65-104">Csatlakozás a szimulált eszköz tooyour IoT hub csomópont használatával</span><span class="sxs-lookup"><span data-stu-id="c2e65-104">Connect your simulated device tooyour IoT hub using Node</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="c2e65-105">Ez az oktatóanyag végén hello három Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="c2e65-105">At hello end of this tutorial, you have three Node.js console apps:</span></span>

* <span data-ttu-id="c2e65-106">**CreateDeviceIdentity.js**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c2e65-106">**CreateDeviceIdentity.js**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="c2e65-107">**ReadDeviceToCloudMessages.js**, amely megjeleníti a szimulált eszköz alkalmazás által küldött hello telemetriai.</span><span class="sxs-lookup"><span data-stu-id="c2e65-107">**ReadDeviceToCloudMessages.js**, which displays hello telemetry sent by your simulated device app.</span></span>
* <span data-ttu-id="c2e65-108">**SimulatedDevice.js**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és minden második használatával hello MQTT protokoll telemetriai üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="c2e65-108">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e65-109">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="c2e65-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="c2e65-110">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c2e65-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c2e65-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c2e65-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="c2e65-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c2e65-112">An active Azure account.</span></span> <span data-ttu-id="c2e65-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="c2e65-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="c2e65-114">Ezzel létrehozta az IoT Hubot.</span><span class="sxs-lookup"><span data-stu-id="c2e65-114">You have now created your IoT hub.</span></span> <span data-ttu-id="c2e65-115">Hello IoT-központ állomásnév és az IoT-központ kapcsolati karakterláncot, hogy kell-e toocomplete hello rest oktatóanyag hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c2e65-115">You have hello IoT Hub host name and hello IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="c2e65-116">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2e65-116">Create a device identity</span></span>
<span data-ttu-id="c2e65-117">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely létrehoz egy eszközidentitás hello identitásjegyzékhez a az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c2e65-117">In this section, you create a Node.js console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="c2e65-118">Eszköz csak tooIoT hub is elérheti, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="c2e65-118">A device can only connect tooIoT hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="c2e65-119">További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="c2e65-119">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="c2e65-120">A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="c2e65-120">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="c2e65-121">Hozzon létre egy új, **createdeviceidentity** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="c2e65-121">Create a new empty folder called **createdeviceidentity**.</span></span> <span data-ttu-id="c2e65-122">A hello **createdeviceidentity** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c2e65-122">In hello **createdeviceidentity** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="c2e65-123">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c2e65-123">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c2e65-124">A parancssorban hello **createdeviceidentity** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** szolgáltatás SDK-csomagot:</span><span class="sxs-lookup"><span data-stu-id="c2e65-124">At your command prompt in hello **createdeviceidentity** folder, run hello following command tooinstall hello **azure-iothub** Service SDK package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="c2e65-125">Egy szövegszerkesztő használatával hozzon létre egy **CreateDeviceIdentity.js** hello fájlban **createdeviceidentity** mappa.</span><span class="sxs-lookup"><span data-stu-id="c2e65-125">Using a text editor, create a **CreateDeviceIdentity.js** file in hello **createdeviceidentity** folder.</span></span>
4. <span data-ttu-id="c2e65-126">Adja hozzá a következő hello `require` hello hello indításkor utasítása **CreateDeviceIdentity.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="c2e65-126">Add hello following `require` statement at hello start of hello **CreateDeviceIdentity.js** file:</span></span>
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. <span data-ttu-id="c2e65-127">Adja hozzá a következő kód toohello hello **CreateDeviceIdentity.js** fájlt, és hello helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc hello előző szakaszban létrehozott hello központ hello:</span><span class="sxs-lookup"><span data-stu-id="c2e65-127">Add hello following code toohello **CreateDeviceIdentity.js** file and replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello previous section:</span></span> 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="c2e65-128">Adja hozzá a következő kód toocreate hello identitás rendszerleíró adatbázis az IoT hub eszköz definíció hello.</span><span class="sxs-lookup"><span data-stu-id="c2e65-128">Add hello following code toocreate a device definition in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="c2e65-129">Ezt a kódot eszköz hoz létre, ha hello eszköz azonosítója nem létezik a hello identitásjegyzékhez, ellenkező esetben hello meglévő eszköz kulcsa hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c2e65-129">This code creates a device if hello device ID does not exist in hello identity registry, otherwise it returns hello key of hello existing device:</span></span>
   
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

7. <span data-ttu-id="c2e65-130">Mentse és zárja be a **CreateDeviceIdentity.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c2e65-130">Save and close **CreateDeviceIdentity.js** file.</span></span>
8. <span data-ttu-id="c2e65-131">toorun hello **createdeviceidentity** alkalmazás, a következő parancs parancssorba hello hello createdeviceidentity mappában hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="c2e65-131">toorun hello **createdeviceidentity** application, execute hello following command at hello command prompt in hello createdeviceidentity folder:</span></span>
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. <span data-ttu-id="c2e65-132">Jegyezze fel a hello **Eszközazonosító** és **eszközkulcs**.</span><span class="sxs-lookup"><span data-stu-id="c2e65-132">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="c2e65-133">Ezek az értékek később szüksége tooIoT eszközként központ kapcsolódó alkalmazásban létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c2e65-133">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e65-134">az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja.</span><span class="sxs-lookup"><span data-stu-id="c2e65-134">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="c2e65-135">Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol.</span><span class="sxs-lookup"><span data-stu-id="c2e65-135">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="c2e65-136">Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon.</span><span class="sxs-lookup"><span data-stu-id="c2e65-136">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="c2e65-137">További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="c2e65-137">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="c2e65-138">Az eszközről a felhőbe irányuló üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="c2e65-138">Receive device-to-cloud messages</span></span>
<span data-ttu-id="c2e65-139">Ebben a szakaszban egy Node.js-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról.</span><span class="sxs-lookup"><span data-stu-id="c2e65-139">In this section, you create a Node.js console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="c2e65-140">Az IoT-központ mutatja egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg.</span><span class="sxs-lookup"><span data-stu-id="c2e65-140">An IoT hub exposes an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="c2e65-141">egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c2e65-141">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="c2e65-142">Hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] az oktatóanyag bemutatja, hogyan tooprocess eszköz-felhő léptékű üzenetek.</span><span class="sxs-lookup"><span data-stu-id="c2e65-142">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="c2e65-143">Hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag további információkat biztosítanak a tooprocess Eseményközpontokból származó üzenetek és alkalmazható toohello IoT Hub Event Hub-kompatibilis végpontok.</span><span class="sxs-lookup"><span data-stu-id="c2e65-143">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e65-144">hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="c2e65-144">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>
> 
> 

1. <span data-ttu-id="c2e65-145">Hozzon létre egy **readdevicetocloudmessages** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="c2e65-145">Create an empty folder called **readdevicetocloudmessages**.</span></span> <span data-ttu-id="c2e65-146">A hello **readdevicetocloudmessages** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c2e65-146">In hello **readdevicetocloudmessages** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="c2e65-147">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c2e65-147">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c2e65-148">A parancssorban hello **readdevicetocloudmessages** mappa, futtassa a következő parancs tooinstall hello hello **azure-eseményközpontok** csomag:</span><span class="sxs-lookup"><span data-stu-id="c2e65-148">At your command prompt in hello **readdevicetocloudmessages** folder, run hello following command tooinstall hello **azure-event-hubs** package:</span></span>
   
    ```
    npm install azure-event-hubs --save
    ```
3. <span data-ttu-id="c2e65-149">Egy szövegszerkesztő használatával hozzon létre egy **ReadDeviceToCloudMessages.js** hello fájlban **readdevicetocloudmessages** mappa.</span><span class="sxs-lookup"><span data-stu-id="c2e65-149">Using a text editor, create a **ReadDeviceToCloudMessages.js** file in hello **readdevicetocloudmessages** folder.</span></span>
4. <span data-ttu-id="c2e65-150">Adja hozzá a következő hello `require` hello utasításokat elejére hello **ReadDeviceToCloudMessages.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="c2e65-150">Add hello following `require` statements at hello start of hello **ReadDeviceToCloudMessages.js** file:</span></span>
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. <span data-ttu-id="c2e65-151">Adja hozzá a következő változó deklarációjában hello és hello helyőrző értékét lecserélheti egy hello IoT-központ kapcsolati karakterlánca:</span><span class="sxs-lookup"><span data-stu-id="c2e65-151">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. <span data-ttu-id="c2e65-152">Adja hozzá a következő kimeneti toohello konzol nyomtatása két függvények hello:</span><span class="sxs-lookup"><span data-stu-id="c2e65-152">Add hello following two functions that print output toohello console:</span></span>
   
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
7. <span data-ttu-id="c2e65-153">Adja hozzá a következő kód toocreate hello hello **EventHubClient**, nyissa meg a hello kapcsolat tooyour IoT-központot, és minden partíció esetében fogadó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c2e65-153">Add hello following code toocreate hello **EventHubClient**, open hello connection tooyour IoT Hub, and create a receiver for each partition.</span></span> <span data-ttu-id="c2e65-154">Ez az alkalmazás egy szűrőt használ, amikor létrehozza a fogadó úgy, hogy hello fogadó csak olvassa be küldött üzenetek tooIoT Hub után hello fogadó elindul.</span><span class="sxs-lookup"><span data-stu-id="c2e65-154">This application uses a filter when it creates a receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="c2e65-155">Ez a szűrő akkor hasznos, tesztelési környezetben, ezért az imént hello aktuális üzenetek készletét.</span><span class="sxs-lookup"><span data-stu-id="c2e65-155">This filter is useful in a test environment so you see just hello current set of messages.</span></span> <span data-ttu-id="c2e65-156">Éles környezetben a kód győződjön meg arról, hogy az összes köszönőüzenetei feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="c2e65-156">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="c2e65-157">További információkért lásd: hello [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-process-d2c-tutorial] oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="c2e65-157">For more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial:</span></span>
   
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
8. <span data-ttu-id="c2e65-158">Mentse és zárja be a hello **ReadDeviceToCloudMessages.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c2e65-158">Save and close hello **ReadDeviceToCloudMessages.js** file.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="c2e65-159">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2e65-159">Create a simulated device app</span></span>
<span data-ttu-id="c2e65-160">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c2e65-160">In this section, you create a Node.js console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="c2e65-161">Hozzon létre egy **simulateddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="c2e65-161">Create an empty folder called **simulateddevice**.</span></span> <span data-ttu-id="c2e65-162">A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c2e65-162">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="c2e65-163">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c2e65-163">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c2e65-164">A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="c2e65-164">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="c2e65-165">Egy szövegszerkesztő használatával hozzon létre egy **SimulatedDevice.js** hello fájlban **simulateddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="c2e65-165">Using a text editor, create a **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="c2e65-166">Adja hozzá a következő hello `require` hello utasításokat elejére hello **SimulatedDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="c2e65-166">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. <span data-ttu-id="c2e65-167">Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="c2e65-167">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="c2e65-168">Cserélje le **{youriothostname}** hello IoT-központ hello nevű hello létrehozott *létrehoz egy IoT-központot* szakasz.</span><span class="sxs-lookup"><span data-stu-id="c2e65-168">Replace **{youriothostname}** with hello name of hello IoT hub you created hello *Create an IoT Hub* section.</span></span> <span data-ttu-id="c2e65-169">Cserélje le **{yourdevicekey}** hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="c2e65-169">Replace **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. <span data-ttu-id="c2e65-170">Adja hozzá a következő függvény toodisplay kimeneti hello alkalmazásból hello:</span><span class="sxs-lookup"><span data-stu-id="c2e65-170">Add hello following function toodisplay output from hello application:</span></span>
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="c2e65-171">Egy visszahívás létrehozásáról és használatáról hello **setInterval** toosend üzenet tooyour IoT-központ másodpercenként működik:</span><span class="sxs-lookup"><span data-stu-id="c2e65-171">Create a callback and use hello **setInterval** function toosend a message tooyour IoT hub every second:</span></span>
   
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
8. <span data-ttu-id="c2e65-172">Nyissa meg a hello kapcsolat tooyour IoT-központot, és üzenetek küldésének megkezdése:</span><span class="sxs-lookup"><span data-stu-id="c2e65-172">Open hello connection tooyour IoT Hub and start sending messages:</span></span>
   
    ```
    client.open(connectCallback);
    ```
9. <span data-ttu-id="c2e65-173">Mentse és zárja be a hello **SimulatedDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c2e65-173">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e65-174">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="c2e65-174">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="c2e65-175">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="c2e65-175">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="run-hello-apps"></a><span data-ttu-id="c2e65-176">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="c2e65-176">Run hello apps</span></span>
<span data-ttu-id="c2e65-177">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c2e65-177">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="c2e65-178">A parancssorba a hello **readdevicetocloudmessages** mappa, futtassa a következő parancs toobegin az IoT hub figyelési hello:</span><span class="sxs-lookup"><span data-stu-id="c2e65-178">At a command prompt in hello **readdevicetocloudmessages** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![NODE.js IoT-központ szolgáltatás toomonitor eszközről a felhőbe alkalmazásüzenetek][7]
2. <span data-ttu-id="c2e65-180">A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toobegin telemetriai adatok tooyour IoT-központ küldése hello:</span><span class="sxs-lookup"><span data-stu-id="c2e65-180">At a command prompt in hello **simulateddevice** folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![NODE.js IoT-központ eszközre app toosend eszközről a felhőbe küldött üzenetek][8]
3. <span data-ttu-id="c2e65-182">Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:</span><span class="sxs-lookup"><span data-stu-id="c2e65-182">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>
   
    ![Az Azure portál használata csempe ábrázoló száma küldött üzenetek tooIoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="c2e65-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2e65-184">Next steps</span></span>
<span data-ttu-id="c2e65-185">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="c2e65-185">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="c2e65-186">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta.</span><span class="sxs-lookup"><span data-stu-id="c2e65-186">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="c2e65-187">Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="c2e65-187">You also created an app that displays hello messages received by hello IoT hub.</span></span> 

<span data-ttu-id="c2e65-188">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="c2e65-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="c2e65-189">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="c2e65-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="c2e65-190">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="c2e65-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="c2e65-191">[Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="c2e65-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="c2e65-192">toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c2e65-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
