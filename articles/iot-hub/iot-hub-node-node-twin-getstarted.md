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
# <a name="get-started-with-device-twins-node"></a><span data-ttu-id="f2d1e-104">Ismerkedés az eszköz twins (csomópont)</span><span class="sxs-lookup"><span data-stu-id="f2d1e-104">Get started with device twins (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="f2d1e-105">Ez az oktatóanyag végén hello hogy két Node.js konzol alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="f2d1e-106">**AddTagsAndQuery.js**, a Node.js háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-106">**AddTagsAndQuery.js**, a Node.js back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="f2d1e-107">**TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely olyan eszköz, amely tooyour IoT-központ a korábban létrehozott hello eszközidentitás szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="f2d1e-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="f2d1e-109">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="f2d1e-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="f2d1e-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-111">An active Azure account.</span></span> <span data-ttu-id="f2d1e-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="f2d1e-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="f2d1e-113">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2d1e-113">Create hello service app</span></span>
<span data-ttu-id="f2d1e-114">Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely hely metaadatok toohello eszköz iker társított **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-114">In this section, you create a Node.js console app that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="f2d1e-115">Ezután hello eszköz twins hello IoT-központ hello található hello eszközök kiválasztása tárolja SZÁMUNKRA, és jelentik a mobilhálózat kapcsolatot, majd hello lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-115">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that are reporting a cellular connection.</span></span>

1. <span data-ttu-id="f2d1e-116">Hozzon létre egy új üres nevű **addtagsandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-116">Create a new empty folder called **addtagsandqueryapp**.</span></span> <span data-ttu-id="f2d1e-117">A hello **addtagsandqueryapp** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-117">In hello **addtagsandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="f2d1e-118">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="f2d1e-119">A parancssorban hello **addtagsandqueryapp** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-119">At your command prompt in hello **addtagsandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="f2d1e-120">Egy szövegszerkesztő használatával hozzon létre egy új **AddTagsAndQuery.js** hello fájlban **addtagsandqueryapp** mappa.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-120">Using a text editor, create a new **AddTagsAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="f2d1e-121">Adja hozzá a következő kód toohello hello **AddTagsAndQuery.js** fájlt, és helyettesítő hello **{iot hub kapcsolati karakterlánc}** helyőrzőt hello a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-121">Add hello following code toohello **AddTagsAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
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
   
    <span data-ttu-id="f2d1e-122">Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-122">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="f2d1e-123">hello előző kód először inicializálja hello **beállításjegyzék** objektumot, majd beolvassa az eszköz iker hello **myDeviceId**, és végül frissíti a címkék szükséges hello helyére vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-123">hello previous code first initializes hello **Registry** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="f2d1e-124">Hello után hello frissítése címkéket az hívások hello **queryTwins** függvény.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-124">After hello updating hello tags it calls hello **queryTwins** function.</span></span>
5. <span data-ttu-id="f2d1e-125">Adja hozzá a következő kódot a hello végén hello **AddTagsAndQuery.js** tooimplement hello **queryTwins** függvény:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-125">Add hello following code at hello end of  **AddTagsAndQuery.js** tooimplement hello **queryTwins** function:</span></span>
   
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
   
    <span data-ttu-id="f2d1e-126">hello előző kód két lekérdezést hajt végre: először a választ csak hello eszköz twins hello található eszközök hello **Redmond43** gépek és hello második refines hello lekérdezés tooselect csak hello csatlakozó eszközöket is keresztül mobilhálózati.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-126">hello previous code executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="f2d1e-127">Vegye figyelembe, hogy hello előző kóddal, amikor hello létrehozza **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-127">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="f2d1e-128">Hello **lekérdezés** objektum tartalmaz egy **hasMoreResults** használható tooinvoke hello logikai tulajdonság **nextAsTwin** módszerek összes tooretrieve eredmények több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-128">hello **query** object contains a **hasMoreResults** boolean property that you can use tooinvoke hello **nextAsTwin** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="f2d1e-129">A metódus hívása **következő** eredmények, amelyek az eszköz twins például összesítési lekérdezések eredményeit nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-129">A method called **next** is available for results that are not device twins for example, results of aggregation queries.</span></span>
6. <span data-ttu-id="f2d1e-130">Futtassa az alkalmazást hello:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-130">Run hello application with:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="f2d1e-131">A hello eredményei egy eszközhöz hello lekérdezés kérése minden eszköz a mappában lévő kell megjelennie **Redmond43** nincs hello lekérdezés, amely korlátozza a hello mobilhálózati használó toodevices elkészítéséig.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-131">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![][1]

<span data-ttu-id="f2d1e-132">Hello a következő szakaszban egy eszköz-alkalmazást, amely jelent hello csatlakozási adatokat hoz létre, és módosításokat hello hello előző szakaszban hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-132">In hello next section you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="f2d1e-133">Hello eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2d1e-133">Create hello device app</span></span>
<span data-ttu-id="f2d1e-134">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**, és majd a frissítések az eszköz két által jelentett tulajdonságok toocontain hello információt, hogy használatával mobilhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-134">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its device twin's reported properties toocontain hello information that it is connected using a cellular network.</span></span>

> [!NOTE]
> <span data-ttu-id="f2d1e-135">Ilyenkor eszköz twins elérhetők, csak az eszközök, tooIoT központi csatlakozás hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-135">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="f2d1e-136">Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-136">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

1. <span data-ttu-id="f2d1e-137">Hozzon létre egy új üres nevű **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-137">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="f2d1e-138">A hello **reportconnectivity** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-138">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="f2d1e-139">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-139">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="f2d1e-140">A parancssorban hello **reportconnectivity** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag :</span><span class="sxs-lookup"><span data-stu-id="f2d1e-140">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="f2d1e-141">Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** hello fájlban **reportconnectivity** mappa.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-141">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
4. <span data-ttu-id="f2d1e-142">Adja hozzá a következő kód toohello hello **ReportConnectivity.js** fájlt, és helyettesítő hello **{eszköz kapcsolati karakterlánc}** helyőrző hello eszköz kapcsolati karakterlánccal másolt hello létrehozásakor **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-142">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="f2d1e-143">Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-143">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="f2d1e-144">hello előző kód után inicializálja a hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId** , és frissíti a jelentett tulajdonsága hello kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-144">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
5. <span data-ttu-id="f2d1e-145">Hello eszköz alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="f2d1e-145">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="f2d1e-146">Hello üzenet `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-146">You should see hello message `twin state reported`.</span></span>
6. <span data-ttu-id="f2d1e-147">Most, hogy hello eszköz jelentette a kapcsolati információ meg kell jelennie mindkét lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-147">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="f2d1e-148">Nyissa meg újra a hello **addtagsandqueryapp** hello mappa, és futtassa le újra:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-148">Go back in hello **addtagsandqueryapp** folder and run hello queries again:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="f2d1e-149">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-149">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][3]

## <a name="next-steps"></a><span data-ttu-id="f2d1e-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2d1e-150">Next steps</span></span>
<span data-ttu-id="f2d1e-151">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-151">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="f2d1e-152">Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és a szimulált eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-152">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="f2d1e-153">Azt is megtanulta, hogyan tooquery ezt az információt hello SQL-szerű IoT Hub lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-153">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="f2d1e-154">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="f2d1e-154">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="f2d1e-155">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="f2d1e-155">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="f2d1e-156">eszközök, eszköz iker kívánt tulajdonságok használata hello konfigurálása [használata szükséges tulajdonságok tooconfigure eszközök] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="f2d1e-156">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="f2d1e-157">interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f2d1e-157">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
