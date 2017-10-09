---
title: "az Azure IoT Hub (csomópont) aaaSchedule feladatok |} Microsoft Docs"
description: "Hogyan tooschedule az Azure IoT-központ feladat tooinvoke a közvetlen módszer több eszközön. Hello Azure IoT SDK-k használata Node.js tooimplement hello szimulált eszköz alkalmazások és a app toorun hello feladata."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: be293362447fbcddaa3433b66f208f22545fe0c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="52980-104">Ütemezés és a feladatok (csomópont)</span><span class="sxs-lookup"><span data-stu-id="52980-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="52980-105">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi egy háttér-alkalmazás toocreate és nyomon követése feladatok ütemezése és eszközök millióira frissítése.</span><span class="sxs-lookup"><span data-stu-id="52980-105">Azure IoT Hub is a fully managed service that enables a back-end app toocreate and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="52980-106">A következő műveletek hello feladatok használható:</span><span class="sxs-lookup"><span data-stu-id="52980-106">Jobs can be used for hello following actions:</span></span>

* <span data-ttu-id="52980-107">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="52980-107">Update desired properties</span></span>
* <span data-ttu-id="52980-108">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="52980-108">Update tags</span></span>
* <span data-ttu-id="52980-109">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="52980-109">Invoke direct methods</span></span>

<span data-ttu-id="52980-110">Fogalmilag egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi hello való összevetéssel az eszközök, eszköz két lekérdezést által definiált végrehajtás folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="52980-110">Conceptually, a job wraps one of these actions and tracks hello progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="52980-111">Például egy háttér-alkalmazás használhat egy feladat tooinvoke újraindítás metódus 10 000 eszköz, a két eszköz lekérdezés által megadott és ütemezett egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="52980-111">For example, a back-end app can use a job tooinvoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="52980-112">Az alkalmazás egyes eszközök fogadni és hello újraindítás metódus hajtható végre, majd előrehaladásának.</span><span class="sxs-lookup"><span data-stu-id="52980-112">That application can then track progress as each of those devices receive and execute hello reboot method.</span></span>

<span data-ttu-id="52980-113">További információ az egyes képességek a cikkeiben:</span><span class="sxs-lookup"><span data-stu-id="52980-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="52980-114">A két eszköz és a tulajdonságok: [Ismerkedés az eszköz twins] [ lnk-get-started-twin] és [oktatóanyag: hogyan toouse eszköz iker tulajdonságai][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="52980-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="52980-115">közvetlen módszerek: [IoT Hub fejlesztői útmutató - közvetlen módszerek] [ lnk-dev-methods] és [oktatóanyag: közvetlen módszer][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="52980-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="52980-116">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="52980-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="52980-117">Hozzon létre egy közvetlen metódus, amely lehetővé teszi a szimulált eszköz alkalmazást **lockDoor** hello megoldás háttérrendszerének által hívható.</span><span class="sxs-lookup"><span data-stu-id="52980-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by hello solution back end.</span></span>
* <span data-ttu-id="52980-118">Hozzon létre egy Node.js-Konzolalkalmazás, hogy hívások hello **lockDoor** közvetlen módszer használatával egy feladat és a frissítések hello hello szimulált eszköz alkalmazásban szükséges tulajdonságok eszköz feladat használatával.</span><span class="sxs-lookup"><span data-stu-id="52980-118">Create a Node.js console app that calls hello **lockDoor** direct method in hello simulated device app using a job and updates hello desired properties using a device job.</span></span>

<span data-ttu-id="52980-119">Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="52980-119">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="52980-120">**simDevice.js**, amelyhez csatlakoznak az IoT-központ tooyour hello eszközidentitás, és megkapja a **lockDoor** közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="52980-120">**simDevice.js**, which connects tooyour IoT hub with hello device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="52980-121">**scheduleJobService.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazás és frissítés hello eszköz iker a kívánt tulajdonságokkal feladat használatával.</span><span class="sxs-lookup"><span data-stu-id="52980-121">**scheduleJobService.js**, which calls a direct method in hello simulated device app and update hello device twin's desired properties using a job.</span></span>

<span data-ttu-id="52980-122">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="52980-122">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="52980-123">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="52980-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="52980-124">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="52980-124">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="52980-125">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="52980-125">An active Azure account.</span></span> <span data-ttu-id="52980-126">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="52980-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="52980-127">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="52980-127">Create a simulated device app</span></span>
<span data-ttu-id="52980-128">Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely tooa hello felhő, amely elindítja a szimulált eszköz újraindítás által meghívott közvetlen metódus válaszol, és használja hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása.</span><span class="sxs-lookup"><span data-stu-id="52980-128">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="52980-129">Hozzon létre egy új üres nevű **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="52980-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="52980-130">A hello **simDevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="52980-130">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="52980-131">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="52980-131">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="52980-132">A parancssorban hello **simDevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="52980-132">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="52980-133">Egy szövegszerkesztő használatával hozzon létre egy új **simDevice.js** hello fájlban **simDevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="52980-133">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>
4. <span data-ttu-id="52980-134">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **simDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="52980-134">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="52980-135">Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="52980-135">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="52980-136">Adja hozzá a következő függvény toohandle hello hello **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="52980-136">Add hello following function toohandle hello **lockDoor** method.</span></span>
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. <span data-ttu-id="52980-137">Adja hozzá a következő kód tooregister hello kezelő hello hello **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="52980-137">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. <span data-ttu-id="52980-138">Mentse és zárja be a hello **simDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="52980-138">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="52980-139">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="52980-139">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="52980-140">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="52980-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="52980-141">Ütemezett feladatok közvetlen metódus hívása, és egy eszköz iker tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="52980-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="52980-142">Ebben a szakaszban egy távoli kezdeményező Node.js-Konzolalkalmazás létrehozása **lockDoor** egy eszközön, a közvetlen módszer és a frissítés hello eszköz iker tartozó tulajdonságok használatával.</span><span class="sxs-lookup"><span data-stu-id="52980-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update hello device twin's properties.</span></span>

1. <span data-ttu-id="52980-143">Hozzon létre egy új üres nevű **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="52980-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="52980-144">A hello **scheduleJobService** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="52980-144">In hello **scheduleJobService** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="52980-145">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="52980-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="52980-146">A parancssorban hello **scheduleJobService** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="52980-146">At your command prompt in hello **scheduleJobService** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="52980-147">Egy szövegszerkesztő használatával hozzon létre egy új **scheduleJobService.js** hello fájlban **scheduleJobService** mappa.</span><span class="sxs-lookup"><span data-stu-id="52980-147">Using a text editor, create a new **scheduleJobService.js** file in hello **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="52980-148">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_gscheduleJobServiceetstarted_service.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="52980-148">Add hello following 'require' statements at hello start of hello **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="52980-149">Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:</span><span class="sxs-lookup"><span data-stu-id="52980-149">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="52980-150">Adja hozzá a következő függvény, amely lesz hello feladat végrehajtásának használt toomonitor hello hello:</span><span class="sxs-lookup"><span data-stu-id="52980-150">Add hello following function that will be used toomonitor hello execution of hello job:</span></span>
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. <span data-ttu-id="52980-151">Adja hozzá a következő kód tooschedule hello feladatot, amely hello eszköz metódust hello:</span><span class="sxs-lookup"><span data-stu-id="52980-151">Add hello following code tooschedule hello job that calls hello device method:</span></span>
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable tooprocess method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. <span data-ttu-id="52980-152">Adja hozzá a következő kód tooschedule hello feladat tooupdate hello eszköz iker hello:</span><span class="sxs-lookup"><span data-stu-id="52980-152">Add hello following code tooschedule hello job tooupdate hello device twin:</span></span>
   
    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. <span data-ttu-id="52980-153">Mentse és zárja be a hello **scheduleJobService.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="52980-153">Save and close hello **scheduleJobService.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="52980-154">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="52980-154">Run hello applications</span></span>
<span data-ttu-id="52980-155">Most már áll készen toorun hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="52980-155">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="52980-156">Parancssorba hello hello **simDevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="52980-156">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="52980-157">Parancssorba hello hello **scheduleJobService** mappa, futtassa a következő parancs tootrigger hello feladatok toolock hello ajtó és frissítés hello iker hello</span><span class="sxs-lookup"><span data-stu-id="52980-157">At hello command prompt in hello **scheduleJobService** folder, run hello following command tootrigger hello jobs toolock hello door and update hello twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="52980-158">Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.</span><span class="sxs-lookup"><span data-stu-id="52980-158">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52980-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52980-159">Next steps</span></span>
<span data-ttu-id="52980-160">Ebben az oktatóanyagban használt a egy feladat tooschedule közvetlen módszer tooa eszköz és a hello eszköz iker tulajdonságok hello frissítése.</span><span class="sxs-lookup"><span data-stu-id="52980-160">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="52980-161">Ismerkedés az IoT-központ és az eszköz felügyeleti minták például távolról keresztül hello vezeték nélkül belső vezérlőprogram frissítési toocontinue lásd:</span><span class="sxs-lookup"><span data-stu-id="52980-161">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, see:</span></span>

<span data-ttu-id="52980-162">[Oktatóanyag: Hogyan toodo a belső vezérlőprogram frissítése][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="52980-162">[Tutorial: How toodo a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="52980-163">Ismerkedés az IoT-központ toocontinue lásd [Ismerkedés az Azure IoT peremhálózati][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="52980-163">toocontinue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
