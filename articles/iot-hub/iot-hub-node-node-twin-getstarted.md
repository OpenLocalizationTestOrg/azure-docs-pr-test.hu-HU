---
title: "Ismerkedés az Azure IoT Hub eszköz twins (csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins címkéket, majd az IoT Hub-lekérdezést. Az Azure IoT SDK for Node.js használatával megvalósítható a szimulált eszköz alkalmazást és egy szolgáltatás-alkalmazást, amely hozzáadja a címkéket és az IoT Hub-lekérdezés futtatása."
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
ms.openlocfilehash: 633c9fd4f8a1d017d93148f8c2e860ccba14238c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-device-twins-node"></a><span data-ttu-id="d5afc-104">Ismerkedés az eszköz twins (csomópont)</span><span class="sxs-lookup"><span data-stu-id="d5afc-104">Get started with device twins (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="d5afc-105">Ez az oktatóanyag végén meg kell két Node.js konzol alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="d5afc-105">At the end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="d5afc-106">**AddTagsAndQuery.js**, a Node.js háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="d5afc-106">**AddTagsAndQuery.js**, a Node.js back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="d5afc-107">**TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely egy eszköz, amely összeköti az IoT hub korábban létrehozott eszköz identitású szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="d5afc-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="d5afc-108">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="d5afc-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="d5afc-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="d5afc-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="d5afc-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="d5afc-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="d5afc-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d5afc-111">An active Azure account.</span></span> <span data-ttu-id="d5afc-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="d5afc-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a><span data-ttu-id="d5afc-113">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5afc-113">Create the service app</span></span>
<span data-ttu-id="d5afc-114">Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, a társított eszközök a két hely metaadatok hozzáadó **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="d5afc-114">In this section, you create a Node.js console app that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="d5afc-115">Ezután lekérdezi az eszköz twins tárolja az IoT hub, az eszközök az Egyesült Államok, és a mobilhálózat kapcsolat jelentő megfelelően kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="d5afc-115">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that are reporting a cellular connection.</span></span>

1. <span data-ttu-id="d5afc-116">Hozzon létre egy új üres nevű **addtagsandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="d5afc-116">Create a new empty folder called **addtagsandqueryapp**.</span></span> <span data-ttu-id="d5afc-117">Az a **addtagsandqueryapp** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="d5afc-117">In the **addtagsandqueryapp** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="d5afc-118">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="d5afc-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="d5afc-119">A parancssorba a **addtagsandqueryapp** mappa telepítéséhez a következő parancsot a **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="d5afc-119">At your command prompt in the **addtagsandqueryapp** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="d5afc-120">Egy szövegszerkesztő használatával hozzon létre egy új **AddTagsAndQuery.js** fájlt a **addtagsandqueryapp** mappa.</span><span class="sxs-lookup"><span data-stu-id="d5afc-120">Using a text editor, create a new **AddTagsAndQuery.js** file in the **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="d5afc-121">Adja hozzá a következő kódot a **AddTagsAndQuery.js** fájlt, és lecserélni az **{iot hub kapcsolati karakterlánc}** helyőrző a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánccal:</span><span class="sxs-lookup"><span data-stu-id="d5afc-121">Add the following code to the **AddTagsAndQuery.js** file, and substitute the **{iot hub connection string}** placeholder with the IoT Hub connection string you copied when you created your hub:</span></span>
   
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
   
    <span data-ttu-id="d5afc-122">A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatás együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="d5afc-122">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="d5afc-123">Az előző kód először inicializálja a **beállításjegyzék** objektumot, majd beolvassa az eszköz iker a **myDeviceId**, és végül frissíti a címkék a kívánt helyre információkkal.</span><span class="sxs-lookup"><span data-stu-id="d5afc-123">The previous code first initializes the **Registry** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="d5afc-124">A címkék frissítése után hívja a **queryTwins** függvény.</span><span class="sxs-lookup"><span data-stu-id="d5afc-124">After the updating the tags it calls the **queryTwins** function.</span></span>
5. <span data-ttu-id="d5afc-125">Adja hozzá a következő kódot végén **AddTagsAndQuery.js** megvalósításához a **queryTwins** függvény:</span><span class="sxs-lookup"><span data-stu-id="d5afc-125">Add the following code at the end of  **AddTagsAndQuery.js** to implement the **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    <span data-ttu-id="d5afc-126">Az előző kód két lekérdezést hajt végre: az első csak az eszköz twins található eszközök kiválasztja a **Redmond43** gépek és a második rendszerint a lekérdezést csak azokat az eszközöket is keresztül mobilhálózati kapcsolódó kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="d5afc-126">The previous code executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="d5afc-127">Vegye figyelembe, hogy az előző kód, amikor létrehozza a **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d5afc-127">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="d5afc-128">A **lekérdezés** objektum tartalmaz egy **hasMoreResults** logikai tulajdonság, amely segítségével meghívni a **nextAsTwin** módszerek több alkalommal fordult elő az összes eredmények beolvasásához.</span><span class="sxs-lookup"><span data-stu-id="d5afc-128">The **query** object contains a **hasMoreResults** boolean property that you can use to invoke the **nextAsTwin** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="d5afc-129">A metódus hívása **következő** eredmények, amelyek az eszköz twins például összesítési lekérdezések eredményeit nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d5afc-129">A method called **next** is available for results that are not device twins for example, results of aggregation queries.</span></span>
6. <span data-ttu-id="d5afc-130">Futtassa az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="d5afc-130">Run the application with:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="d5afc-131">Megjelenik az eredmények között egy eszközön a lekérdezés kérni a minden eszköz a mappában lévő **Redmond43** és a lekérdezés, amely korlátozza az eredmények mobilhálózati használó eszközök sem.</span><span class="sxs-lookup"><span data-stu-id="d5afc-131">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![][1]

<span data-ttu-id="d5afc-132">A következő szakaszban létrehoz egy eszköz-alkalmazást, amely a kapcsolódási adatokat, és módosítja az előző szakaszban a lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="d5afc-132">In the next section you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="d5afc-133">Az eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5afc-133">Create the device app</span></span>
<span data-ttu-id="d5afc-134">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**, és az eszköz iker által jelentett tulajdonságok a információkat, hogy mobilhálózat használata csatlakozik tartalmazzák majd a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="d5afc-134">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, and then updates its device twin's reported properties to contain the information that it is connected using a cellular network.</span></span>

> [!NOTE]
> <span data-ttu-id="d5afc-135">Ilyenkor eszköz twins elérhetők csak a MQTT protokollal IoT-központ csatlakozó eszközökről.</span><span class="sxs-lookup"><span data-stu-id="d5afc-135">At this time, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="d5afc-136">Tekintse meg a [MQTT támogatási] [ lnk-devguide-mqtt] miként a meglévő eszköz alkalmazásának módját MQTT használatára.</span><span class="sxs-lookup"><span data-stu-id="d5afc-136">Please refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>
> 
> 

1. <span data-ttu-id="d5afc-137">Hozzon létre egy új üres nevű **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="d5afc-137">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="d5afc-138">Az a **reportconnectivity** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="d5afc-138">In the **reportconnectivity** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="d5afc-139">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="d5afc-139">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="d5afc-140">A parancssorba a **reportconnectivity** mappa telepítéséhez a következő parancsot a **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="d5afc-140">At your command prompt in the **reportconnectivity** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="d5afc-141">Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** fájlt a **reportconnectivity** mappa.</span><span class="sxs-lookup"><span data-stu-id="d5afc-141">Using a text editor, create a new **ReportConnectivity.js** file in the **reportconnectivity** folder.</span></span>
4. <span data-ttu-id="d5afc-142">Adja hozzá a következő kódot a **ReportConnectivity.js** fájlt, és lecserélni a **{eszköz kapcsolati karakterlánc}** helyőrző a létrehozásautánmásoljaeszközkapcsolatikarakterlánccalrendelkező**myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="d5afc-142">Add the following code to the **ReportConnectivity.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="d5afc-143">A **ügyfél** vezérlőnek az eszköz twins az eszközről interaktív szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="d5afc-143">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="d5afc-144">Az előző kód után állíthatja a **ügyfél** objektumazonosító, beolvassa az eszköz iker a **myDeviceId** , és frissíti a jelentett tulajdonsága a kapcsolati információ.</span><span class="sxs-lookup"><span data-stu-id="d5afc-144">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId** and updates its reported property with the connectivity information.</span></span>
5. <span data-ttu-id="d5afc-145">Az eszköz alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="d5afc-145">Run the device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="d5afc-146">Az üzenet `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="d5afc-146">You should see the message `twin state reported`.</span></span>
6. <span data-ttu-id="d5afc-147">Most, hogy az eszköz jelentette a kapcsolat adatait, akkor mindkét lekérdezések meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="d5afc-147">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="d5afc-148">Lépjen vissza a **addtagsandqueryapp** mappa, és futtassa újból a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="d5afc-148">Go back in the **addtagsandqueryapp** folder and run the queries again:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="d5afc-149">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="d5afc-149">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][3]

## <a name="next-steps"></a><span data-ttu-id="d5afc-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5afc-150">Next steps</span></span>
<span data-ttu-id="d5afc-151">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="d5afc-151">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="d5afc-152">Fel van véve eszköz metaadatait címkék egy háttér-alkalmazásból, és a szimulált eszköz alkalmazásának megírt az eszköz a két jelentés eszköz kapcsolódási adatok.</span><span class="sxs-lookup"><span data-stu-id="d5afc-152">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="d5afc-153">Megtudta, ezt az információt az SQL-szerű IoT Hub lekérdezési nyelv lekérdezése is.</span><span class="sxs-lookup"><span data-stu-id="d5afc-153">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="d5afc-154">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="d5afc-154">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="d5afc-155">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="d5afc-155">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="d5afc-156">eszköz iker kívánt tulajdonságokkal rendelkező eszközök konfigurálása a [használata szükséges eszközök tulajdonságok] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="d5afc-156">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="d5afc-157">az interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközök szabályozásának a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d5afc-157">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
