---
title: "aaaUse Azure IoT Hub iker tulajdonságai (csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooconfigure eszközök. A szimulált eszköz alkalmazás és egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs hello Azure IoT SDK-k használata Node.js tooimplement."
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
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a><span data-ttu-id="c0836-104">Szükségeskonfiguráció-alkalmazható tulajdonságok tooconfigure eszközöket (csomópont)</span><span class="sxs-lookup"><span data-stu-id="c0836-104">Use desired properties tooconfigure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="c0836-105">Ez az oktatóanyag végén hello hogy két Node.js konzol alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="c0836-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="c0836-106">**SimulateDeviceConfiguration.js**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="c0836-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="c0836-107">**SetDesiredConfigurationAndQuery.js**, a Node.js háttér-alkalmazás, amely hello szükséges konfigurációs egy eszközön, és lekérdezések hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="c0836-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="c0836-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c0836-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="c0836-109">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c0836-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="c0836-110">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c0836-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="c0836-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c0836-111">An active Azure account.</span></span> <span data-ttu-id="c0836-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="c0836-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="c0836-113">Ha követte hello [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**; pedig kihagyhatja toohello [ Hello szimulált eszköz alkalmazás létrehozása] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="c0836-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="c0836-114">Hello szimulált eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0836-114">Create hello simulated device app</span></span>
<span data-ttu-id="c0836-115">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések a szimulált hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="c0836-115">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="c0836-116">Hozzon létre egy új üres nevű **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c0836-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="c0836-117">A hello **simulatedeviceconfiguration** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c0836-117">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="c0836-118">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c0836-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c0836-119">A parancssorban hello **simulatedeviceconfiguration** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="c0836-119">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="c0836-120">Egy szövegszerkesztő használatával hozzon létre egy új **SimulateDeviceConfiguration.js** hello fájlban **simulatedeviceconfiguration** mappa.</span><span class="sxs-lookup"><span data-stu-id="c0836-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="c0836-121">Adja hozzá a következő kód toohello hello **SimulateDeviceConfiguration.js** fájlt, és helyettesítő hello **{eszköz kapcsolati karakterlánc}** helyőrzőt kimásolt mikor hello eszköz kapcsolati karakterláncot, hello létrehozott **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="c0836-121">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="c0836-122">Hello **ügyfél** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="c0836-122">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="c0836-123">hello előző kód után inicializálja a hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId**, és csatolja a kívánt tulajdonságok hello frissítés kezelőjét.</span><span class="sxs-lookup"><span data-stu-id="c0836-123">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and attaches a handler for hello update on desired properties.</span></span> <span data-ttu-id="c0836-124">hello kezelő ellenőrzi, hogy egy tényleges Helykonfiguráció-változtatási kérelem hello configIds összehasonlításával, akkor hív meg, amely elindítja a hello konfigurációváltozás metódus.</span><span class="sxs-lookup"><span data-stu-id="c0836-124">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="c0836-125">Vegye figyelembe, hogy hello szakét az egyszerűség, a hello előző kód használja a kódolt alapértelmezett hello kezdeti konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c0836-125">Note that for hello sake of simplicity, hello previous code uses a hard-coded default for hello inital configuration.</span></span> <span data-ttu-id="c0836-126">Egy valós alkalmazás valószínűleg szeretné, hogy a konfigurálás betöltése a helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="c0836-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="c0836-127">Kívánt tulajdonság állapotváltozási események mindig kibocsátott egyszer eszköz csatlakozáskor, győződjön meg arról, hogy nincs-e egy tényleges módosítása a hello toocheck szükséges tulajdonságok bármilyen művelet végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="c0836-127">Desired property change events are always emitted once at device connection, make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="c0836-128">Adja hozzá a következő módszerek előtt hello hello `client.open()` hívása:</span><span class="sxs-lookup"><span data-stu-id="c0836-128">Add hello following methods before hello `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="c0836-129">Hello **initConfigChange** metódus frissítések tulajdonságok jelentett hello helyi eszköz a két objektum hello konfigurációs frissítési kérelmet, és készletek hello állapot túl**függőben lévő**, majd a frissítések hello eszköz hello szolgáltatásban iker.</span><span class="sxs-lookup"><span data-stu-id="c0836-129">hello **initConfigChange** method updates reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="c0836-130">Miután sikeresen frissített hello eszköz két, a hosszú ideig futó folyamat. a hello végrehajtásának leállítása szimulálja **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="c0836-130">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="c0836-131">A metódus frissítések hello helyi eszköz iker által jelentett hello állapotának beállításakor túl tulajdonságok**sikeres** és hello eltávolítása **pendingConfig** objektum.</span><span class="sxs-lookup"><span data-stu-id="c0836-131">This method updates hello local device twin's reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="c0836-132">Majd frissíti a hello eszköz iker hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="c0836-132">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="c0836-133">Megjegyzés: a toosave sávszélesség, hogy csak a hello tulajdonságok toobe módosított megadásával tulajdonságának frissítésekor (nevű **javítás** hello kód fent található), teljes dokumentum hello felülírása helyett.</span><span class="sxs-lookup"><span data-stu-id="c0836-133">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c0836-134">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="c0836-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="c0836-135">Néhány konfigurációs frissítési folyamat előfordulhat, hogy képes tooaccommodate módosításainak konfigurációjához hello frissítés futása közben, mások lehet őket, és mások sikerült utasítsa el azokat a hibaállapotot tooqueue.</span><span class="sxs-lookup"><span data-stu-id="c0836-135">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, others might have tooqueue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="c0836-136">Győződjön meg arról, hogy tooconsider hello kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logika hello hello konfigurációváltozás kezdeményezése előtt.</span><span class="sxs-lookup"><span data-stu-id="c0836-136">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="c0836-137">Hello eszköz alkalmazás futtatása:</span><span class="sxs-lookup"><span data-stu-id="c0836-137">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="c0836-138">Hello üzenet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="c0836-138">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="c0836-139">Folyamatosan futó hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c0836-139">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="c0836-140">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0836-140">Create hello service app</span></span>
<span data-ttu-id="c0836-141">Ez a szakasz során létrehoz egy Node.js-Konzolalkalmazás, hogy a frissítések hello *szükségeskonfiguráció-tulajdonságok* a hello eszköz iker társított **myDeviceId** új telemetria-konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="c0836-141">In this section, you will create a Node.js console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="c0836-142">Ezután hello eszköz twins hello IoT-központ tárolt lekérdezi és hello hello különbségének szükséges, és a jelentett hello eszköz konfigurációját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c0836-142">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="c0836-143">Hozzon létre egy új üres nevű **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="c0836-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="c0836-144">A hello **setdesiredandqueryapp** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c0836-144">In hello **setdesiredandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="c0836-145">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c0836-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="c0836-146">A parancssorban hello **setdesiredandqueryapp** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** csomag:</span><span class="sxs-lookup"><span data-stu-id="c0836-146">At your command prompt in hello **setdesiredandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="c0836-147">Egy szövegszerkesztő használatával hozzon létre egy új **SetDesiredAndQuery.js** hello fájlban **addtagsandqueryapp** mappa.</span><span class="sxs-lookup"><span data-stu-id="c0836-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="c0836-148">Adja hozzá a következő kód toohello hello **SetDesiredAndQuery.js** fájlt, és helyettesítő hello **{iot hub kapcsolati karakterlánc}** helyőrzőt hello a hub létrehozása után másolja az IoT-központ kapcsolati karakterlánc :</span><span class="sxs-lookup"><span data-stu-id="c0836-148">Add hello following code toohello **SetDesiredAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
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

    <span data-ttu-id="c0836-149">Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="c0836-149">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="c0836-150">hello előző kód után inicializálja a hello **beállításjegyzék** objektum beolvassa az eszköz iker hello **myDeviceId**, és frissíti a kívánt tulajdonságát egy új telemetriai konfigurációs objektuma.</span><span class="sxs-lookup"><span data-stu-id="c0836-150">hello previous code, after it initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="c0836-151">Ezt követően meghívja a hello **queryTwins** működéséhez esemény 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="c0836-151">After that, it calls hello **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c0836-152">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="c0836-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="c0836-153">Használja több eszközt, és nem toodetect módosítások toogenerate felhasználók számára is elérhető jelentések lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="c0836-153">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="c0836-154">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="c0836-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="c0836-155">.</span><span class="sxs-lookup"><span data-stu-id="c0836-155">.</span></span>

1. <span data-ttu-id="c0836-156">Adja hozzá a következő kódot közvetlenül előtt hello hello `registry.getDeviceTwin()` meghívása tooimplement hello **queryTwins** függvény:</span><span class="sxs-lookup"><span data-stu-id="c0836-156">Add hello following code right before hello `registry.getDeviceTwin()` invocation tooimplement hello **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
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
   
    <span data-ttu-id="c0836-157">hello előző kód lekérdezések hello eszköz twins hello IoT-központ tárolja és megrendelése hello szükséges, és telemetriai konfigurációk jelentett.</span><span class="sxs-lookup"><span data-stu-id="c0836-157">hello previous code queries hello device twins stored in hello IoT hub and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="c0836-158">Tekintse meg a toohello [IoT-központ lekérdezési nyelv] [ lnk-query] toolearn hogyan toogenerate gazdag jelent az eszközön.</span><span class="sxs-lookup"><span data-stu-id="c0836-158">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
2. <span data-ttu-id="c0836-159">A **SimulateDeviceConfiguration.js** hello alkalmazás fut, futtassa:</span><span class="sxs-lookup"><span data-stu-id="c0836-159">With **SimulateDeviceConfiguration.js** running, run hello application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="c0836-160">Megtekintheti az hello jelentett konfigurációs módosítást a **sikeres** túl**függőben lévő** túl**sikeres** újra hello új aktív küldés gyakoriság öt perc 24 helyett. óra.</span><span class="sxs-lookup"><span data-stu-id="c0836-160">You should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="c0836-161">Nincs késleltetést mentése tooa perc közötti hello eszköz jelentés művelet és hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="c0836-161">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="c0836-162">Ez a tooenable hello lekérdezés infrastruktúra toowork nagyon nagy méretekben.</span><span class="sxs-lookup"><span data-stu-id="c0836-162">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="c0836-163">egy egyetlen eszközt iker tooretrieve konzisztens nézeteinek hello használata **getDeviceTwin** metódus a hello **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="c0836-163">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="c0836-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0836-164">Next steps</span></span>
<span data-ttu-id="c0836-165">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* egy háttér-alkalmazásból, és a szimulált eszköz alkalmazás toodetect, módosítása és a tag állapotaként reporting többlépéses frissítési folyamat szimulálása megírt  *Tulajdonságok jelentett* toohello eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="c0836-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app toodetect that change and simulate a multi-step update process reporting its status as *reported properties* toohello device twin.</span></span>

<span data-ttu-id="c0836-166">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="c0836-166">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="c0836-167">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="c0836-167">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="c0836-168">ütemezhet, vagy hajtsa végre műveleteket a eszközök nagy mennyiségű, lásd: hello [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c0836-168">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="c0836-169">interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c0836-169">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
