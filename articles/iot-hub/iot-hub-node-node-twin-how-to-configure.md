---
title: "Azure IoT Hub eszköz iker tulajdonságait (csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins eszközök konfigurálásához. Az Azure IoT SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazás és egy szolgáltatás-alkalmazást, amely módosítja egy eszköz konfigurálásának egy eszköz iker használatával."
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
ms.openlocfilehash: 771106ce7b00a5231d9929e4b5ea34aefe693597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a><span data-ttu-id="6c4bf-104">Használja a kívánt tulajdonságait (csomópont) eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c4bf-104">Use desired properties to configure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="6c4bf-105">Ez az oktatóanyag végén meg kell két Node.js konzol alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-105">At the end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="6c4bf-106">**SimulateDeviceConfiguration.js**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="6c4bf-107">**SetDesiredConfigurationAndQuery.js**, a Node.js háttér-alkalmazás, amely beállítja a kívánt konfiguráció egy eszközön, és lekérdezi a konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="6c4bf-108">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="6c4bf-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="6c4bf-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="6c4bf-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-111">An active Azure account.</span></span> <span data-ttu-id="6c4bf-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="6c4bf-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="6c4bf-113">Ha követte a [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**; és a ugorjon[a szimulált eszköz-alkalmazás létrehozása] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-113">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a><span data-ttu-id="6c4bf-114">A szimulált eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c4bf-114">Create the simulated device app</span></span>
<span data-ttu-id="6c4bf-115">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések szimulált konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-115">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="6c4bf-116">Hozzon létre egy új üres nevű **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="6c4bf-117">Az a **simulatedeviceconfiguration** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-117">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="6c4bf-118">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="6c4bf-119">A parancssorba a **simulatedeviceconfiguration** mappa telepítéséhez a következő parancsot a **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-119">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="6c4bf-120">Egy szövegszerkesztő használatával hozzon létre egy új **SimulateDeviceConfiguration.js** fájlt a **simulatedeviceconfiguration** mappa.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="6c4bf-121">Adja hozzá a következő kódot a **SimulateDeviceConfiguration.js** fájlt, és lecserélni a **{eszköz kapcsolati karakterlánc}** helyőrző az eszköz kapcsolati karakterlánccal létrehozása után másolja a **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-121">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="6c4bf-122">A **ügyfél** vezérlőnek az eszközről eszköz twins együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-122">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="6c4bf-123">Az előző kód után állíthatja a **ügyfél** objektumazonosító, beolvassa az eszköz iker a **myDeviceId**, és csatolja a kívánt tulajdonságokkal a frissítés kezelőjét.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-123">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId**, and attaches a handler for the update on desired properties.</span></span> <span data-ttu-id="6c4bf-124">A kezelő ellenőrzi, hogy egy tényleges Helykonfiguráció-változtatási kérelem a configIds összehasonlításával, akkor hív meg, olyan módszer, amelyik elindul a konfigurációs módosítást.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-124">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="6c4bf-125">Vegye figyelembe, hogy az egyszerűség kedvéért, az előző kód egy kódolt alapértelmezett értéket használja, a kezdeti konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-125">Note that for the sake of simplicity, the previous code uses a hard-coded default for the inital configuration.</span></span> <span data-ttu-id="6c4bf-126">Egy valós alkalmazás valószínűleg szeretné, hogy a konfigurálás betöltése a helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="6c4bf-127">Kívánt tulajdonság állapotváltozási események mindig kibocsátott egyszer eszköz csatlakozáskor, ügyeljen arra, hogy ellenőrizze, hogy van-e egy tényleges módosítása a kívánt tulajdonságaiban bármilyen művelet végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-127">Desired property change events are always emitted once at device connection, make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="6c4bf-128">Adja hozzá a következő metódusokat előtt a `client.open()` hívása:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-128">Add the following methods before the `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="6c4bf-129">A **initConfigChange** metódus frissíti a helyi eszközön a két objektum jelentett tulajdonságainak a konfigurációs frissítési kérelmet az állapota, **függőben lévő**, majd a frissítések az eszköz iker a a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-129">The **initConfigChange** method updates reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="6c4bf-130">Miután sikeresen frissített a eszköz iker, egy hosszú ideig tartó folyamatot, amely végrehajtása során szimulálja **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-130">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="6c4bf-131">Ez a módszer frissíti a helyi eszközön iker jelentett tulajdonságok az állapot **sikeres** és eltávolítása a **pendingConfig** objektum.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-131">This method updates the local device twin's reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="6c4bf-132">Ezután frissíti az eszköz a két szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-132">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="6c4bf-133">Megjegyzés: a, hogy a sávszélességet, hogy csak a módosítani kívánt tulajdonságok megadásával tulajdonságának frissítésekor (nevű **javítás** a fenti kódban), a teljes dokumentum felülírása helyett.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-133">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6c4bf-134">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="6c4bf-135">Néhány konfigurációs frissítési folyamat közben fut-e a frissítés, mások lehet a várólistába helyezni őket, és mások sikerült hibát elutasítása a célként megadott konfigurációs módosítások befogadásához lehet.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-135">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, others might have to queue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="6c4bf-136">Ügyeljen arra, hogy fontolja meg a kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logikai kezdeményezése a konfiguráció módosítása előtt.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-136">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="6c4bf-137">Az eszköz alkalmazás futtatása:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-137">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="6c4bf-138">Az üzenet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-138">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="6c4bf-139">Tartsa meg az alkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-139">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="6c4bf-140">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c4bf-140">Create the service app</span></span>
<span data-ttu-id="6c4bf-141">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely frissíti hoz létre a *szükséges tulajdonságok* meg az eszköz a két társított **myDeviceId** új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-141">In this section, you will create a Node.js console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="6c4bf-142">Ezután lekérdezi az eszköz twins az IoT hub tárolja, és a kívánt és jelentett konfigurációkat, az eszköz közötti különbséget szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-142">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="6c4bf-143">Hozzon létre egy új üres nevű **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="6c4bf-144">Az a **setdesiredandqueryapp** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-144">In the **setdesiredandqueryapp** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="6c4bf-145">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="6c4bf-146">A parancssorba a **setdesiredandqueryapp** mappa telepítéséhez a következő parancsot a **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-146">At your command prompt in the **setdesiredandqueryapp** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="6c4bf-147">Egy szövegszerkesztő használatával hozzon létre egy új **SetDesiredAndQuery.js** fájlt a **addtagsandqueryapp** mappa.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in the **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="6c4bf-148">Adja hozzá a következő kódot a **SetDesiredAndQuery.js** fájlt, és lecserélni az **{iot hub kapcsolati karakterlánc}** helyőrző a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánccal:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-148">Add the following code to the **SetDesiredAndQuery.js** file, and substitute the **{iot hub connection string}** placeholder with the IoT Hub connection string you copied when you created your hub:</span></span>
   
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

    <span data-ttu-id="6c4bf-149">A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatás együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-149">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="6c4bf-150">Az előző kód után állíthatja a **beállításjegyzék** objektumazonosító, beolvassa az eszköz iker a **myDeviceId**, és frissíti a kívánt tulajdonságát egy új telemetriai konfigurációs objektuma.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-150">The previous code, after it initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="6c4bf-151">Ezt követően meghívja a **queryTwins** működéséhez esemény 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-151">After that, it calls the **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6c4bf-152">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="6c4bf-153">Lekérdezésekkel számos eszközön keresztül a felhasználók számára is elérhető jelentések létrehozásához, és nem észleli a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-153">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="6c4bf-154">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="6c4bf-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="6c4bf-155">.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-155">.</span></span>

1. <span data-ttu-id="6c4bf-156">Adja hozzá a következő kódot közvetlenül az előtt a `registry.getDeviceTwin()` meghívása megvalósításához a **queryTwins** függvény:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-156">Add the following code right before the `registry.getDeviceTwin()` invocation to implement the **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
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
   
    <span data-ttu-id="6c4bf-157">Az előző kód lekérdezéseket a eszköz twins az IoT hub tárolja, és kiírja a kívánt és jelentett telemetriai konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-157">The previous code queries the device twins stored in the IoT hub and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="6c4bf-158">Tekintse meg a [IoT-központ lekérdezési nyelv] [ lnk-query] további információt az eszközök közötti hatékony jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-158">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
2. <span data-ttu-id="6c4bf-159">A **SimulateDeviceConfiguration.js** fut, futtassa az alkalmazást az:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-159">With **SimulateDeviceConfiguration.js** running, run the application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="6c4bf-160">Módosítsa a jelentésben szereplő konfigurációs kell megjelennie **sikeres** való **függőben lévő** való **sikeres** újra az új aktív küldés 24 óra helyett öt perces gyakoriságot.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-160">You should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="6c4bf-161">Nincs késleltetést legfeljebb egy percet, az eszköz jelentés művelet és a lekérdezési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-161">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="6c4bf-162">Ez a nagyon nagy méretekben működéséhez a lekérdezés infrastruktúra engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-162">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="6c4bf-163">Egy egyetlen eszközt iker használati konzisztens nézetek beolvasása a **getDeviceTwin** metódust a **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-163">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="6c4bf-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c4bf-164">Next steps</span></span>
<span data-ttu-id="6c4bf-165">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* egy háttér-alkalmazásból, és a szimulált eszköz alkalmazások észleli a változást, és a tag állapotaként reporting többlépéses frissítési folyamat szimulálása megírt  *Tulajdonságok jelentett* számára az eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app to detect that change and simulate a multi-step update process reporting its status as *reported properties* to the device twin.</span></span>

<span data-ttu-id="6c4bf-166">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="6c4bf-166">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="6c4bf-167">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="6c4bf-167">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="6c4bf-168">ütemezett vagy műveleteket hajtson végre a nagy mennyiségű eszközök lásd: a [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-168">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="6c4bf-169">az interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközök szabályozásának a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6c4bf-169">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
