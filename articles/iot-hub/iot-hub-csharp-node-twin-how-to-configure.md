---
title: "aaaUse Azure IoT Hub iker tulajdonságai (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooconfigure eszközök. Hello Azure IoT-eszközök SDK a Node.js tooimplement a szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs használja."
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
ms.openlocfilehash: 840a1b2e45f4763131299577583aa89015dcdd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="2133c-104">Használja a kívánt tulajdonságokkal tooconfigure eszközök</span><span class="sxs-lookup"><span data-stu-id="2133c-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="2133c-105">Ez az oktatóanyag végén hello hogy két konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="2133c-105">At hello end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="2133c-106">**SimulateDeviceConfiguration.js**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="2133c-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="2133c-107">**SetDesiredConfigurationAndQuery**, a .NET-háttér-alkalmazás, amely hello szükséges konfigurációs egy eszközön, és lekérdezések hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="2133c-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="2133c-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="2133c-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="2133c-109">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="2133c-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="2133c-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2133c-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="2133c-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="2133c-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="2133c-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="2133c-112">An active Azure account.</span></span> <span data-ttu-id="2133c-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="2133c-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="2133c-114">Ha követte hello [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="2133c-114">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="2133c-115">Ebben az esetben kihagyhatja toohello [létrehozás hello szimulált eszköz alkalmazásának] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="2133c-115">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="2133c-116">Hello szimulált eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2133c-116">Create hello simulated device app</span></span>
<span data-ttu-id="2133c-117">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések a szimulált hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="2133c-117">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="2133c-118">Hozzon létre egy új üres nevű **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="2133c-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="2133c-119">A hello **simulatedeviceconfiguration** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="2133c-119">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="2133c-120">Fogadja el az összes hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="2133c-120">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="2133c-121">A parancssorban hello **simulatedeviceconfiguration** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt**csomagok:</span><span class="sxs-lookup"><span data-stu-id="2133c-121">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="2133c-122">Egy szövegszerkesztő használatával hozzon létre egy új **SimulateDeviceConfiguration.js** hello fájlban **simulatedeviceconfiguration** mappa.</span><span class="sxs-lookup"><span data-stu-id="2133c-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="2133c-123">Adja hozzá a következő kód toohello hello **SimulateDeviceConfiguration.js** fájlt, és helyettesítő hello **{eszköz kapcsolati karakterlánc}** helyőrzőt kimásolt mikor hello eszköz kapcsolati karakterláncot, hello létrehozott **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="2133c-123">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="2133c-124">Hello **ügyfél** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="2133c-124">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="2133c-125">Ez a kód inicializálja hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId**, és a kezelő hello frissítés a *tulajdonságok szükséges*.</span><span class="sxs-lookup"><span data-stu-id="2133c-125">This code initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and then attaches a handler for hello update on *desired properties*.</span></span> <span data-ttu-id="2133c-126">hello kezelő ellenőrzi, hogy egy tényleges Helykonfiguráció-változtatási kérelem hello configIds összehasonlításával, akkor hív meg, amely elindítja a hello konfigurációváltozás metódus.</span><span class="sxs-lookup"><span data-stu-id="2133c-126">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="2133c-127">Vegye figyelembe, hogy hello szakét az egyszerűség, ezt a kódot használja a kódolt alapértelmezett hello kezdeti konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2133c-127">Note that for hello sake of simplicity, this code uses a hard-coded default for hello initial configuration.</span></span> <span data-ttu-id="2133c-128">Egy valós alkalmazás valószínűleg szeretné, hogy a konfigurálás betöltése a helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="2133c-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="2133c-129">Kívánt tulajdonság állapotváltozási események mindig egyszer kibocsátott eszköz csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="2133c-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="2133c-130">Győződjön meg arról, hogy nincs-e egy tényleges módosítása a hello toocheck szükséges tulajdonságok bármilyen művelet végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="2133c-130">Make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="2133c-131">Adja hozzá a következő módszerek előtt hello hello `client.open()` hívása:</span><span class="sxs-lookup"><span data-stu-id="2133c-131">Add hello following methods before hello `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="2133c-132">Hello **initConfigChange** metódus frissítések hello jelentett hello helyi eszköz a két objektum hello konfiguráció tulajdonságainak túl kérelem és a készletek hello állapotának frissítése**függőben lévő**, majd a frissítések hello hello szolgáltatásban iker eszköz.</span><span class="sxs-lookup"><span data-stu-id="2133c-132">hello **initConfigChange** method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="2133c-133">Miután sikeresen frissített hello eszköz két, a hosszú ideig futó folyamat. a hello végrehajtásának leállítása szimulálja **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="2133c-133">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="2133c-134">Ez a módszer frissíti hello helyi jelentett tulajdonságok hello állapotának beállításakor túl**sikeres** és hello eltávolítása **pendingConfig** objektum.</span><span class="sxs-lookup"><span data-stu-id="2133c-134">This method updates hello local reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="2133c-135">Majd frissíti a hello eszköz iker hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="2133c-135">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="2133c-136">Megjegyzés: a toosave sávszélesség, hogy csak a hello tulajdonságok toobe módosított megadásával tulajdonságának frissítésekor (nevű **javítás** hello kód fent található), teljes dokumentum hello felülírása helyett.</span><span class="sxs-lookup"><span data-stu-id="2133c-136">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2133c-137">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="2133c-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="2133c-138">Néhány konfigurációs frissítési folyamat előfordulhat, hogy képes tooaccommodate módosításainak konfigurációjához hello frissítés futása közben, előfordulhat, hogy rendelkeznek őket, és néhány sikerült utasítsa el azokat a hibaállapotot tooqueue.</span><span class="sxs-lookup"><span data-stu-id="2133c-138">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="2133c-139">Győződjön meg arról, hogy tooconsider hello kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logika hello hello konfigurációváltozás kezdeményezése előtt.</span><span class="sxs-lookup"><span data-stu-id="2133c-139">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="2133c-140">Hello eszköz alkalmazás futtatása:</span><span class="sxs-lookup"><span data-stu-id="2133c-140">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="2133c-141">Hello üzenet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="2133c-141">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="2133c-142">Folyamatosan futó hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2133c-142">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="2133c-143">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2133c-143">Create hello service app</span></span>
<span data-ttu-id="2133c-144">Ez a szakasz során létrehoz egy .NET-Konzolalkalmazás, hogy a frissítések hello *szükségeskonfiguráció-tulajdonságok* a hello eszköz iker társított **myDeviceId** új telemetria-konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="2133c-144">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="2133c-145">Ezután hello eszköz twins hello IoT-központ tárolt lekérdezi és hello hello különbségének szükséges, és a jelentett hello eszköz konfigurációját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2133c-145">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="2133c-146">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="2133c-146">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="2133c-147">Név hello projekt **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="2133c-147">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="2133c-149">A Megoldáskezelőben kattintson a jobb gombbal hello **SetDesiredConfigurationAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="2133c-149">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="2133c-150">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="2133c-150">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="2133c-151">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="2133c-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="2133c-153">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="2133c-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="2133c-154">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="2133c-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="2133c-155">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2133c-155">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="2133c-156">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="2133c-156">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="2133c-157">Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="2133c-157">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="2133c-158">Ez a kód inicializálja hello **beállításjegyzék** objektum beolvassa az eszköz iker hello **myDeviceId**, majd frissíti a kívánt tulajdonságát egy új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="2133c-158">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="2133c-159">Ezt követően hello eszköz twins tárolt hello IoT-központ 10 másodpercenként kérdezi le, és megrendelése hello szükséges, és telemetriai konfigurációk jelentett.</span><span class="sxs-lookup"><span data-stu-id="2133c-159">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="2133c-160">Tekintse meg a toohello [IoT-központ lekérdezési nyelv] [ lnk-query] toolearn hogyan toogenerate gazdag jelent az eszközön.</span><span class="sxs-lookup"><span data-stu-id="2133c-160">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="2133c-161">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="2133c-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="2133c-162">Használja több eszközt, és nem toodetect módosítások toogenerate felhasználók számára is elérhető jelentések lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="2133c-162">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="2133c-163">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="2133c-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="2133c-164">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="2133c-164">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="2133c-165">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **SetDesiredConfigurationAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="2133c-165">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="2133c-166">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2133c-166">Build hello solution.</span></span>
1. <span data-ttu-id="2133c-167">A **SimulateDeviceConfiguration.js** hello .NET-alkalmazás fut, futtassa a Visual Studio használatával **F5** és megtekintheti az hello jelentett konfigurációs módosítást a **sikeres** túl**függőben lévő** túl**sikeres** újra hello új aktív küldés 24 óra helyett öt perces gyakoriságot.</span><span class="sxs-lookup"><span data-stu-id="2133c-167">With **SimulateDeviceConfiguration.js** running, run hello .NET application from Visual Studio using **F5** and you should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Eszköz sikeresen konfigurálva][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="2133c-169">Nincs késleltetést mentése tooa perc közötti hello eszköz jelentés művelet és hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="2133c-169">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="2133c-170">Ez a tooenable hello lekérdezés infrastruktúra toowork nagyon nagy méretekben.</span><span class="sxs-lookup"><span data-stu-id="2133c-170">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="2133c-171">egy egyetlen eszközt iker tooretrieve konzisztens nézeteinek hello használata **getDeviceTwin** metódus a hello **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="2133c-171">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="2133c-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2133c-172">Next steps</span></span>
<span data-ttu-id="2133c-173">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* hello megoldásból háttér, és egy eszköz alkalmazás toodetect, módosítása és annak állapotát a jelentett hello reporting többlépéses frissítési folyamat szimulálása megírt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="2133c-173">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="2133c-174">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="2133c-174">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="2133c-175">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="2133c-175">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="2133c-176">ütemezhet, vagy hajtsa végre műveleteket a eszközök nagy mennyiségű, lásd: hello [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2133c-176">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="2133c-177">interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2133c-177">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
