---
title: "Az Azure IoT Hub (csomópont) feladatok ütemezése |} Microsoft Docs"
description: "How to Schedule a több eszközre közvetlen metódus egy Azure IoT Hub-feladat ütemezése. Az Azure IoT SDK for Node.js használatával megvalósítható a szimulált eszköz alkalmazások és a service-alkalmazást, a feladat futtatásához."
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
ms.openlocfilehash: 42e594dc6a8a8be619b5652bf8e44cf883650489
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="7e5c9-104">Ütemezés és a feladatok (csomópont)</span><span class="sxs-lookup"><span data-stu-id="7e5c9-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="7e5c9-105">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi egy háttér-alkalmazás létrehozása és nyomon követheti a feladatok ütemezése és eszközök millióira frissítése.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-105">Azure IoT Hub is a fully managed service that enables a back-end app to create and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="7e5c9-106">Feladatok is használható a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-106">Jobs can be used for the following actions:</span></span>

* <span data-ttu-id="7e5c9-107">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="7e5c9-107">Update desired properties</span></span>
* <span data-ttu-id="7e5c9-108">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="7e5c9-108">Update tags</span></span>
* <span data-ttu-id="7e5c9-109">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="7e5c9-109">Invoke direct methods</span></span>

<span data-ttu-id="7e5c9-110">Egy feladat fogalmilag, becsomagolja az alábbi műveletek egyikét, és nyomon követi a folyamatot való összevetéssel az eszközök, eszköz két lekérdezést által definiált végrehajtás.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-110">Conceptually, a job wraps one of these actions and tracks the progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="7e5c9-111">Például egy háttér-alkalmazást, egy feladat 10 000 eszközök, eszköz iker lekérdezés által megadott, és egy későbbi időpontban ütemezett újraindítás metódus használható.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-111">For example, a back-end app can use a job to invoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="7e5c9-112">Az alkalmazás egyes eszközök kap, és az újraindítás metódus végrehajtása majd előrehaladásának.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-112">That application can then track progress as each of those devices receive and execute the reboot method.</span></span>

<span data-ttu-id="7e5c9-113">További információ az egyes képességek a cikkeiben:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="7e5c9-114">A két eszköz és a tulajdonságok: [Ismerkedés az eszköz twins] [ lnk-get-started-twin] és [oktatóanyag: kettős eszköztulajdonságok használata][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="7e5c9-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How to use device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="7e5c9-115">közvetlen módszerek: [IoT Hub fejlesztői útmutató - közvetlen módszerek] [ lnk-dev-methods] és [oktatóanyag: közvetlen módszer][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="7e5c9-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="7e5c9-116">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="7e5c9-117">Hozzon létre egy közvetlen metódus, amely lehetővé teszi a szimulált eszköz alkalmazást **lockDoor** által a megoldás háttérrendszeréhez hívható.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by the solution back end.</span></span>
* <span data-ttu-id="7e5c9-118">Hozzon létre egy Node.js-Konzolalkalmazás, amely behívja a **lockDoor** közvetlen módszer a alkalmazásban szimulált eszköz egy feladat és a frissítések a kívánt eszköz feladat használatával tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-118">Create a Node.js console app that calls the **lockDoor** direct method in the simulated device app using a job and updates the desired properties using a device job.</span></span>

<span data-ttu-id="7e5c9-119">Ez az oktatóanyag végén két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-119">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="7e5c9-120">**simDevice.js**, amely az IoT hub eszköz identitással csatlakozik, és megkapja a **lockDoor** közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-120">**simDevice.js**, which connects to your IoT hub with the device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="7e5c9-121">**scheduleJobService.js**, amely közvetlen metódus meghívja a szimulált eszköz alkalmazás és a frissítés az eszköz a két feladat használatával tulajdonságok szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-121">**scheduleJobService.js**, which calls a direct method in the simulated device app and update the device twin's desired properties using a job.</span></span>

<span data-ttu-id="7e5c9-122">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-122">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="7e5c9-123">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="7e5c9-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="7e5c9-124">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan telepítse a Node.js-ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-124">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7e5c9-125">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-125">An active Azure account.</span></span> <span data-ttu-id="7e5c9-126">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="7e5c9-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="7e5c9-127">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e5c9-127">Create a simulated device app</span></span>
<span data-ttu-id="7e5c9-128">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a felhőbe, amely elindítja a szimulált eszköz újraindítás és a jelentésben szereplő tulajdonságok segítségével engedélyezése az eszköz iker lekérdezések azonosíthatja azokat az eszközöket, és ha azok utolsó újraindítása után hívja a közvetlen módszer hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-128">In this section, you create a Node.js console app that responds to a direct method called by the cloud, which triggers a simulated device reboot and uses the reported properties to enable device twin queries to identify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="7e5c9-129">Hozzon létre egy új üres nevű **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="7e5c9-130">Az a **simDevice** mappa, hozzon létre egy package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-130">In the **simDevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="7e5c9-131">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-131">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="7e5c9-132">Parancsot a parancssorba a a **simDevice** mappa telepítéséhez a következő parancsot a **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-132">At your command prompt in the **simDevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="7e5c9-133">Egy szövegszerkesztő használatával hozzon létre egy új **simDevice.js** fájlt a **simDevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-133">Using a text editor, create a new **simDevice.js** file in the **simDevice** folder.</span></span>
4. <span data-ttu-id="7e5c9-134">Adja hozzá a következő "szükséges" utasítások elején a **simDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-134">Add the following 'require' statements at the start of the **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="7e5c9-135">Adjon hozzá egy **connectionString** változót, és ezzel hozzon létre egy **Ügyfél** példányt.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-135">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="7e5c9-136">Adja hozzá a következő függvény kezelni a **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-136">Add the following function to handle the **lockDoor** method.</span></span>
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. <span data-ttu-id="7e5c9-137">Az alábbi kódot a kezelőt regisztrálni a **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-137">Add the following code to register the handler for the **lockDoor** method.</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. <span data-ttu-id="7e5c9-138">Mentse és zárja be a **simDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-138">Save and close the **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="7e5c9-139">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-139">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="7e5c9-140">Az éles kódban újrapróbálkozási házirendeket is meg kell valósítania (például egy exponenciális leállítást) a [tranziens hibakezelést][lnk-transient-faults] ismertető MSDN-cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="7e5c9-141">Ütemezett feladatok közvetlen metódus hívása, és egy eszköz iker tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="7e5c9-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="7e5c9-142">Ebben a szakaszban egy távoli kezdeményező Node.js-Konzolalkalmazás létrehozása **lockDoor** közvetlen módszert használ az eszközön, és az eszköz iker tulajdonságainak frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update the device twin's properties.</span></span>

1. <span data-ttu-id="7e5c9-143">Hozzon létre egy új üres nevű **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="7e5c9-144">Az a **scheduleJobService** mappa, hozzon létre egy package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-144">In the **scheduleJobService** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="7e5c9-145">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="7e5c9-146">A parancssorban a a **scheduleJobService** mappa telepítéséhez a következő parancsot a **azure-IOT hubbal** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-146">At your command prompt in the **scheduleJobService** folder, run the following command to install the **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="7e5c9-147">Egy szövegszerkesztő használatával hozzon létre egy új **scheduleJobService.js** fájlt a **scheduleJobService** mappa.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-147">Using a text editor, create a new **scheduleJobService.js** file in the **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="7e5c9-148">Adja hozzá a következő "szükséges" utasítások elején a **dmpatterns_gscheduleJobServiceetstarted_service.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-148">Add the following 'require' statements at the start of the **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="7e5c9-149">Adja hozzá a következő változók deklarációja, és cserélje le a helyőrző értékeket:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-149">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="7e5c9-150">Adja hozzá a következő függvény használatával figyelheti a feladat végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-150">Add the following function that will be used to monitor the execution of the job:</span></span>
   
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
7. <span data-ttu-id="7e5c9-151">Adja hozzá a ütemezni a feladatot, amely az eszköz metódust hívja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-151">Add the following code to schedule the job that calls the device method:</span></span>
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
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
8. <span data-ttu-id="7e5c9-152">Adja hozzá a következő kódot az eszköz iker frissítése feladat ütemezése:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-152">Add the following code to schedule the job to update the device twin:</span></span>
   
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
9. <span data-ttu-id="7e5c9-153">Mentse és zárja be a **scheduleJobService.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-153">Save and close the **scheduleJobService.js** file.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="7e5c9-154">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="7e5c9-154">Run the applications</span></span>
<span data-ttu-id="7e5c9-155">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-155">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="7e5c9-156">A parancssorban a **simDevice** mappa, a következő parancsot a rendszer újraindítása közvetlen módszer figyelését.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-156">At the command prompt in the **simDevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="7e5c9-157">A parancssorban a **scheduleJobService** mappa, a következő parancsot az ajtó zárolja, és frissíti a kettős feladatok elindítása</span><span class="sxs-lookup"><span data-stu-id="7e5c9-157">At the command prompt in the **scheduleJobService** folder, run the following command to trigger the jobs to lock the door and update the twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="7e5c9-158">A közvetlen módszer a konzolon eszköz válasz megjelenik.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-158">You see the device response to the direct method in the console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e5c9-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e5c9-159">Next steps</span></span>
<span data-ttu-id="7e5c9-160">Ebben az oktatóanyagban egy feladat ütemezése a közvetlen módszer egy eszköz és a két eszköz tulajdonságok frissítése használt.</span><span class="sxs-lookup"><span data-stu-id="7e5c9-160">In this tutorial, you used a job to schedule a direct method to a device and the update of the device twin's properties.</span></span>

<span data-ttu-id="7e5c9-161">A folytatáshoz, a légkondicionáló frissítést keresztül Ismerkedés az IoT-központ és az eszköz felügyeleti minták például távolról, lásd:</span><span class="sxs-lookup"><span data-stu-id="7e5c9-161">To continue getting started with IoT Hub and device management patterns such as remote over the air firmware update, see:</span></span>

<span data-ttu-id="7e5c9-162">[Oktatóanyag: Módjáról a belső vezérlőprogram frissítése][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="7e5c9-162">[Tutorial: How to do a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="7e5c9-163">Ismerkedés az IoT-központ a folytatáshoz tekintse meg a [Ismerkedés az Azure IoT peremhálózati][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="7e5c9-163">To continue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
