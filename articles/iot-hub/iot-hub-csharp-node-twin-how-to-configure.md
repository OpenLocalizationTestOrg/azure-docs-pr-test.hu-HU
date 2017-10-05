---
title: "Azure IoT Hub eszköz iker tulajdonságait (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins eszközök konfigurálásához. Az Azure IoT-eszközök SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazások és az Azure IoT szolgáltatás SDK for .NET egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs végrehajtásához."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 78b4523fa7d0c056f84214429730a5df1bcdcef7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-desired-properties-to-configure-devices"></a><span data-ttu-id="da5b2-104">Használni kívánt tulajdonságokat eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="da5b2-104">Use desired properties to configure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="da5b2-105">Ez az oktatóanyag végén hogy két konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="da5b2-105">At the end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="da5b2-106">**SimulateDeviceConfiguration.js**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="da5b2-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="da5b2-107">**SetDesiredConfigurationAndQuery**, .NET-háttér-alkalmazás, amely beállítja a kívánt konfiguráció egy eszközön, és lekérdezi a konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="da5b2-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="da5b2-108">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="da5b2-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="da5b2-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="da5b2-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="da5b2-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="da5b2-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="da5b2-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="da5b2-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="da5b2-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="da5b2-112">An active Azure account.</span></span> <span data-ttu-id="da5b2-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="da5b2-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="da5b2-114">Ha követte a [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="da5b2-114">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="da5b2-115">Ebben az esetben ugorjon a [a szimulált eszköz-alkalmazás létrehozása] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="da5b2-115">In that case, you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a><span data-ttu-id="da5b2-116">A szimulált eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="da5b2-116">Create the simulated device app</span></span>
<span data-ttu-id="da5b2-117">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések szimulált konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="da5b2-117">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="da5b2-118">Hozzon létre egy új üres nevű **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="da5b2-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="da5b2-119">Az a **simulatedeviceconfiguration** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="da5b2-119">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="da5b2-120">Fogadja el az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="da5b2-120">Accept all the defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="da5b2-121">A parancssorba a **simulatedeviceconfiguration** mappa telepítéséhez a következő parancsot a **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:</span><span class="sxs-lookup"><span data-stu-id="da5b2-121">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="da5b2-122">Egy szövegszerkesztő használatával hozzon létre egy új **SimulateDeviceConfiguration.js** fájlt a **simulatedeviceconfiguration** mappa.</span><span class="sxs-lookup"><span data-stu-id="da5b2-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="da5b2-123">Adja hozzá a következő kódot a **SimulateDeviceConfiguration.js** fájlt, és lecserélni a **{eszköz kapcsolati karakterlánc}** helyőrző az eszköz kapcsolati karakterlánccal létrehozása után másolja a **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="da5b2-123">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="da5b2-124">A **ügyfél** vezérlőnek az eszközről eszköz twins együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="da5b2-124">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="da5b2-125">Ez a kód inicializálja a **ügyfél** objektumazonosító, beolvassa az eszköz iker a **myDeviceId**, és a frissítés kezelőjét a *tulajdonságok szükséges*.</span><span class="sxs-lookup"><span data-stu-id="da5b2-125">This code initializes the **Client** object, retrieves the device twin for **myDeviceId**, and then attaches a handler for the update on *desired properties*.</span></span> <span data-ttu-id="da5b2-126">A kezelő ellenőrzi, hogy egy tényleges Helykonfiguráció-változtatási kérelem a configIds összehasonlításával, akkor hív meg, olyan módszer, amelyik elindul a konfigurációs módosítást.</span><span class="sxs-lookup"><span data-stu-id="da5b2-126">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="da5b2-127">Vegye figyelembe, hogy az egyszerűség kedvéért, ez a kód egy kódolt alapértelmezett értéket használja, a kezdeti konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="da5b2-127">Note that for the sake of simplicity, this code uses a hard-coded default for the initial configuration.</span></span> <span data-ttu-id="da5b2-128">Egy valós alkalmazás valószínűleg szeretné, hogy a konfigurálás betöltése a helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="da5b2-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="da5b2-129">Kívánt tulajdonság állapotváltozási események mindig egyszer kibocsátott eszköz csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="da5b2-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="da5b2-130">Győződjön meg arról, hogy ellenőrizze, hogy van-e egy tényleges módosítása a kívánt tulajdonságaiban bármilyen művelet végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="da5b2-130">Make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="da5b2-131">Adja hozzá a következő metódusokat előtt a `client.open()` hívása:</span><span class="sxs-lookup"><span data-stu-id="da5b2-131">Add the following methods before the `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="da5b2-132">A **initConfigChange** módszer a jelentett a helyi eszközön a két objektum tulajdonságainak frissítése a konfigurációs frissítési kérelmet az állapota, **függőben lévő**, majd frissíti az eszköz a két szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="da5b2-132">The **initConfigChange** method updates the reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="da5b2-133">Miután sikeresen frissített a eszköz iker, egy hosszú ideig tartó folyamatot, amely végrehajtása során szimulálja **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="da5b2-133">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="da5b2-134">Ez a módszer az állapot helyi jelentett tulajdonságainak frissítése **sikeres** és eltávolítása a **pendingConfig** objektum.</span><span class="sxs-lookup"><span data-stu-id="da5b2-134">This method updates the local reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="da5b2-135">Ezután frissíti az eszköz a két szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="da5b2-135">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="da5b2-136">Megjegyzés: a, hogy a sávszélességet, hogy csak a módosítani kívánt tulajdonságok megadásával tulajdonságának frissítésekor (nevű **javítás** a fenti kódban), a teljes dokumentum felülírása helyett.</span><span class="sxs-lookup"><span data-stu-id="da5b2-136">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="da5b2-137">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="da5b2-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="da5b2-138">Néhány konfigurációs frissítési folyamat közben fut-e a frissítés, néhány lehet a várólistába helyezni őket, és néhány sikerült hibát elutasítása a célként megadott konfigurációs módosítások befogadásához lehet.</span><span class="sxs-lookup"><span data-stu-id="da5b2-138">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, some might have to queue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="da5b2-139">Ügyeljen arra, hogy fontolja meg a kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logikai kezdeményezése a konfiguráció módosítása előtt.</span><span class="sxs-lookup"><span data-stu-id="da5b2-139">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="da5b2-140">Az eszköz alkalmazás futtatása:</span><span class="sxs-lookup"><span data-stu-id="da5b2-140">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="da5b2-141">Az üzenet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="da5b2-141">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="da5b2-142">Tartsa meg az alkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="da5b2-142">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="da5b2-143">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="da5b2-143">Create the service app</span></span>
<span data-ttu-id="da5b2-144">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely frissíti hoz létre a *szükséges tulajdonságok* meg az eszköz a két társított **myDeviceId** új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="da5b2-144">In this section, you will create a .NET console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="da5b2-145">Ezután lekérdezi az eszköz twins az IoT hub tárolja, és a kívánt és jelentett konfigurációkat, az eszköz közötti különbséget szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="da5b2-145">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="da5b2-146">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="da5b2-146">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="da5b2-147">Nevet a projektnek **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="da5b2-147">Name the project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="da5b2-149">A Megoldáskezelőben kattintson a jobb gombbal a **SetDesiredConfigurationAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="da5b2-149">In Solution Explorer, right-click the **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="da5b2-150">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="da5b2-150">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="da5b2-151">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="da5b2-151">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="da5b2-153">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="da5b2-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="da5b2-154">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="da5b2-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="da5b2-155">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="da5b2-155">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="da5b2-156">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="da5b2-156">Add the following method to the **Program** class:</span></span>
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    <span data-ttu-id="da5b2-157">A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatás együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="da5b2-157">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="da5b2-158">Ezzel a kóddal inicializálja a **beállításjegyzék** objektumazonosító, beolvassa az eszköz iker a **myDeviceId**, majd frissíti a kívánt tulajdonságát egy új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="da5b2-158">This code initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="da5b2-159">Ezt követően azt lekérdezi az eszköz twins tárolja az IoT hub 10 másodpercenként, és kiírja a kívánt és jelentett telemetriai konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="da5b2-159">After that, it queries the device twins stored in the IoT hub every 10 seconds, and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="da5b2-160">Tekintse meg a [IoT-központ lekérdezési nyelv] [ lnk-query] további információt az eszközök közötti hatékony jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="da5b2-160">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="da5b2-161">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="da5b2-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="da5b2-162">Lekérdezésekkel számos eszközön keresztül a felhasználók számára is elérhető jelentések létrehozásához, és nem észleli a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="da5b2-162">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="da5b2-163">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="da5b2-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="da5b2-164">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="da5b2-164">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. <span data-ttu-id="da5b2-165">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **SetDesiredConfigurationAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="da5b2-165">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="da5b2-166">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="da5b2-166">Build the solution.</span></span>
1. <span data-ttu-id="da5b2-167">A **SimulateDeviceConfiguration.js** a .NET-alkalmazás fut, futtassa a Visual Studio használatával **F5** és megjelenítheti a jelentésben szereplő konfigurációs módosítást a **sikeres** való **függőben lévő** való **sikeres** újra az új aktív küldés 24 óra helyett öt perces gyakoriságot.</span><span class="sxs-lookup"><span data-stu-id="da5b2-167">With **SimulateDeviceConfiguration.js** running, run the .NET application from Visual Studio using **F5** and you should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Eszköz sikeresen konfigurálva][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="da5b2-169">Nincs késleltetést legfeljebb egy percet, az eszköz jelentés művelet és a lekérdezési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="da5b2-169">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="da5b2-170">Ez a nagyon nagy méretekben működéséhez a lekérdezés infrastruktúra engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="da5b2-170">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="da5b2-171">Egy egyetlen eszközt iker használati konzisztens nézetek beolvasása a **getDeviceTwin** metódust a **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="da5b2-171">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="da5b2-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da5b2-172">Next steps</span></span>
<span data-ttu-id="da5b2-173">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* a megoldásban való háttér, és egy eszköz alkalmazás észleli a változást, és egy többlépéses frissítési folyamat állapotának jelentett tulajdonságai reporting szimulálása megírt.</span><span class="sxs-lookup"><span data-stu-id="da5b2-173">In this tutorial, you set a desired configuration as *desired properties* from the solution back end, and wrote a device app to detect that change and simulate a multi-step update process reporting its status through the reported properties.</span></span>

<span data-ttu-id="da5b2-174">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="da5b2-174">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="da5b2-175">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="da5b2-175">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="da5b2-176">ütemezett vagy műveleteket hajtson végre a nagy mennyiségű eszközök lásd: a [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="da5b2-176">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="da5b2-177">az interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközök szabályozásának a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="da5b2-177">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

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
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
