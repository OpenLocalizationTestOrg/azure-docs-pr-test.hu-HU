---
title: "az Azure IoT Hub (.NET/csomópont) aaaSchedule feladatok |} Microsoft Docs"
description: "Hogyan tooschedule az Azure IoT-központ feladat tooinvoke a közvetlen módszer több eszközön. Hello Azure IoT-eszközök SDK használata Node.js tooimplement hello szimulált eszköz alkalmazásokat és hello Azure IoT szolgáltatás SDK .NET tooimplement a app toorun hello feladata."
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
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: f6148b67129dde4580bfe9ccceafd6400fbc5976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a><span data-ttu-id="7bee5-104">Ütemezés és a feladatok (.NET/Node.js)</span><span class="sxs-lookup"><span data-stu-id="7bee5-104">Schedule and broadcast jobs (.NET/Node.js)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="7bee5-105">Azure IoT Hub tooschedule és nyomon követése feladatok frissítő eszközök millióira használja.</span><span class="sxs-lookup"><span data-stu-id="7bee5-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="7bee5-106">Feladatok használata:</span><span class="sxs-lookup"><span data-stu-id="7bee5-106">Use jobs to:</span></span>

* <span data-ttu-id="7bee5-107">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="7bee5-107">Update desired properties</span></span>
* <span data-ttu-id="7bee5-108">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="7bee5-108">Update tags</span></span>
* <span data-ttu-id="7bee5-109">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="7bee5-109">Invoke direct methods</span></span>

<span data-ttu-id="7bee5-110">Egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi hello végrehajtási eszköz két lekérdezést által meghatározott eszközök készlete alapján.</span><span class="sxs-lookup"><span data-stu-id="7bee5-110">A job wraps one of these actions and tracks hello execution against a set of devices that is defined by a device twin query.</span></span> <span data-ttu-id="7bee5-111">Például egy háttér-alkalmazást, egy feladat tooinvoke közvetlen módszer hello eszköz újraindulása 10 000 eszközökön használható.</span><span class="sxs-lookup"><span data-stu-id="7bee5-111">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="7bee5-112">Adjon meg hello eszközök eszköz iker lekérdezéssel, és egy későbbi időpontban hello feladat toorun ütemezni.</span><span class="sxs-lookup"><span data-stu-id="7bee5-112">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="7bee5-113">hello feladat nyomon követi előrehaladását, mivel minden egyes hello eszközök kapja meg, és hello újraindítás közvetlen metódus hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="7bee5-113">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="7bee5-114">További információ az egyes képességek, toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="7bee5-114">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="7bee5-115">A két eszköz és a tulajdonságok: [Ismerkedés az eszköz twins] [ lnk-get-started-twin] és [oktatóanyag: hogyan toouse eszköz iker tulajdonságai][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="7bee5-115">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="7bee5-116">A közvetlen módszer: [IoT Hub fejlesztői útmutató - közvetlen módszerek] [ lnk-dev-methods] és [oktatóanyag: közvetlen módszerekkel][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="7bee5-116">Direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: Use direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="7bee5-117">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="7bee5-117">This tutorial shows you how to:</span></span>

* <span data-ttu-id="7bee5-118">Hozzon létre egy eszköz-alkalmazást, amely egy közvetlen a hívott metódus **lockDoor** , amely hello háttér-alkalmazás által hívható.</span><span class="sxs-lookup"><span data-stu-id="7bee5-118">Create a device app that implements a direct method called **lockDoor** that can be called by hello back-end app.</span></span> <span data-ttu-id="7bee5-119">hello eszközalkalmazás kívánt tulajdonságok módosítását is fogad hello háttér-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7bee5-119">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="7bee5-120">Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot toocall hello **lockDoor** közvetlen módszer több eszközön.</span><span class="sxs-lookup"><span data-stu-id="7bee5-120">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="7bee5-121">Egy másik feladat küldi el a kívánt tulajdonság toomultiple eszközök frissíti.</span><span class="sxs-lookup"><span data-stu-id="7bee5-121">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="7bee5-122">Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="7bee5-122">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="7bee5-123">**simDevice.js** , hogy csatlakozik az IoT-központ tooyour, hello megvalósítja **lockDoor** közvetlen metódust, és kezeli a szükséges tulajdonságok módosítását.</span><span class="sxs-lookup"><span data-stu-id="7bee5-123">**simDevice.js** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="7bee5-124">**ScheduleJob** használó feladatok toocall hello **lockDoor** közvetlen módszer és a frissítés hello eszköz iker szükségeskonfiguráció-tulajdonságok több eszközön.</span><span class="sxs-lookup"><span data-stu-id="7bee5-124">**ScheduleJob** that uses jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

<span data-ttu-id="7bee5-125">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7bee5-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="7bee5-126">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7bee5-126">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="7bee5-127">A Node.js 0.12.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="7bee5-127">Node.js version 0.12.x or later.</span></span> <span data-ttu-id="7bee5-128">hello cikk [a fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="7bee5-128">hello article [Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7bee5-129">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7bee5-129">An active Azure account.</span></span> <span data-ttu-id="7bee5-130">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="7bee5-130">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a><span data-ttu-id="7bee5-131">Közvetlen metódus hívása, és az eszköz iker frissítések küldése feladatok ütemezése</span><span class="sxs-lookup"><span data-stu-id="7bee5-131">Schedule jobs for calling a direct method and sending device twin updates</span></span>

<span data-ttu-id="7bee5-132">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) használó feladatok toocall hello létrehozása **lockDoor** közvetlen módszer és elküldeni kívánt tulajdonság toomultiple eszközök frissíti.</span><span class="sxs-lookup"><span data-stu-id="7bee5-132">In this section, you create a .NET console app (using C#) that uses jobs toocall hello **lockDoor** direct method and send desired property updates toomultiple devices.</span></span>

1. <span data-ttu-id="7bee5-133">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="7bee5-133">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="7bee5-134">Név hello projekt **ScheduleJob**.</span><span class="sxs-lookup"><span data-stu-id="7bee5-134">Name hello project **ScheduleJob**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

1. <span data-ttu-id="7bee5-136">A Megoldáskezelőben kattintson a jobb gombbal hello **ScheduleJob** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="7bee5-136">In Solution Explorer, right-click hello **ScheduleJob** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="7bee5-137">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="7bee5-137">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="7bee5-138">Ez a lépés tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="7bee5-138">This step downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="7bee5-140">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="7bee5-140">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. <span data-ttu-id="7bee5-141">Adja hozzá a következő hello `using` utasítás Ha hello alapértelmezett utasításokban még nincs letöltve.</span><span class="sxs-lookup"><span data-stu-id="7bee5-141">Add hello following `using` statement if not already present in hello default statements.</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="7bee5-142">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="7bee5-142">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="7bee5-143">Hello helyőrzőt cserélje le az IoT-központ kapcsolati karakterlánc hello központ hello előző szakaszban létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="7bee5-143">Replace hello placeholder with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. <span data-ttu-id="7bee5-144">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="7bee5-144">Add hello following method toohello **Program** class:</span></span>

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. <span data-ttu-id="7bee5-145">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="7bee5-145">Add hello following method toohello **Program** class:</span></span>

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. <span data-ttu-id="7bee5-146">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="7bee5-146">Add hello following method toohello **Program** class:</span></span>

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. <span data-ttu-id="7bee5-147">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7bee5-147">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER toorun hello next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER tooexit.");
    Console.ReadLine();
    ```

1. <span data-ttu-id="7bee5-148">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **ScheduleJob** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="7bee5-148">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ScheduleJob** project is **Start**.</span></span> <span data-ttu-id="7bee5-149">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7bee5-149">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="7bee5-150">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bee5-150">Create a simulated device app</span></span>

<span data-ttu-id="7bee5-151">Ebben a szakaszban hoz létre egy Node.js-Konzolalkalmazás, amely tooa hello felhő, amely elindítja a szimulált eszköz újraindítás által meghívott közvetlen metódus válaszol, és használja hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása.</span><span class="sxs-lookup"><span data-stu-id="7bee5-151">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="7bee5-152">Hozzon létre egy új üres nevű **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="7bee5-152">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="7bee5-153">A hello **simDevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="7bee5-153">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="7bee5-154">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="7bee5-154">Accept all hello defaults:</span></span>

    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="7bee5-155">A parancssorban hello **simDevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:</span><span class="sxs-lookup"><span data-stu-id="7bee5-155">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="7bee5-156">Egy szövegszerkesztő használatával hozzon létre egy új **simDevice.js** hello fájlban **simDevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="7bee5-156">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>

1. <span data-ttu-id="7bee5-157">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **simDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="7bee5-157">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. <span data-ttu-id="7bee5-158">Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="7bee5-158">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="7bee5-159">Győződjön meg arról, hogy tooreplace hello helyőrzőt értékek megfelelő tooyour beállítása.</span><span class="sxs-lookup"><span data-stu-id="7bee5-159">Make sure tooreplace hello placeholders with values appropriate tooyour setup.</span></span>

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="7bee5-160">Adja hozzá a következő függvény toohandle hello hello **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="7bee5-160">Add hello following function toohandle hello **lockDoor** method.</span></span>

    ```nodejs
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

1. <span data-ttu-id="7bee5-161">Adja hozzá a következő kód tooregister hello kezelő hello hello **lockDoor** metódust.</span><span class="sxs-lookup"><span data-stu-id="7bee5-161">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. <span data-ttu-id="7bee5-162">Mentse és zárja be a hello **simDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7bee5-162">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="7bee5-163">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="7bee5-163">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="7bee5-164">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="7bee5-164">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="7bee5-165">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="7bee5-165">Run hello apps</span></span>

<span data-ttu-id="7bee5-166">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="7bee5-166">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="7bee5-167">Parancssorba hello hello **simDevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="7bee5-167">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>

    ```cmd/sh
    node simDevice.js
    ```

1. <span data-ttu-id="7bee5-168">Futtatási hello C# Konzolalkalmazás **ScheduleJob** kattintson a jobb gombbal a hello **ScheduleJob** projekt, jelölje be **Debug** és **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="7bee5-168">Run hello C# console app **ScheduleJob** by right-clicking on hello **ScheduleJob** project, then selecting **Debug** and **Start new instance**.</span></span>

1. <span data-ttu-id="7bee5-169">Az eszköz és a háttér-alkalmazások hello kimenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7bee5-169">You see hello output from both device and back-end apps.</span></span>

    ![Hello alkalmazások tooschedule feladatok futtatása][img-schedulejobs]

## <a name="next-steps"></a><span data-ttu-id="7bee5-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bee5-171">Next steps</span></span>

<span data-ttu-id="7bee5-172">Ebben az oktatóanyagban használt a egy feladat tooschedule közvetlen módszer tooa eszköz és a hello eszköz iker tulajdonságok hello frissítése.</span><span class="sxs-lookup"><span data-stu-id="7bee5-172">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="7bee5-173">keresztül hello vezeték nélkül belső vezérlőprogram frissítése, olvassa el az IoT-központ és az eszköz, például távolról felügyeleti mintákat bevezetés toocontinue [oktatóanyag: hogyan toodo a belső vezérlőprogram frissítése][lnk-fwupdate].</span><span class="sxs-lookup"><span data-stu-id="7bee5-173">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, read [Tutorial: How toodo a firmware update][lnk-fwupdate].</span></span>

<span data-ttu-id="7bee5-174">Ismerkedés az IoT-központ toocontinue lásd [Ismerkedés az IoT-Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="7bee5-174">toocontinue getting started with IoT Hub, see [Getting started with IoT Edge][lnk-iot-edge].</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
