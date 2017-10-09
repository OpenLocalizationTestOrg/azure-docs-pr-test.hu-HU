---
title: "aaaGet Azure IoT Hub kezelés (.NET/csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz felügyeleti tooinitiate egy távoli eszköz újraindul. Hello Azure IoT-eszközök SDK használata Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
ms.openlocfilehash: ea6d50dfac1c222d7836e3bf5503c6c9b8c89dfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="ed791-104">Ismerkedés az Eszközfelügyelet (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="ed791-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="ed791-105">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="ed791-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="ed791-106">Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="ed791-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="ed791-107">Létrehoz egy szimulált eszköz alkalmazást, amely tartalmazza a közvetlen módszer, amely az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="ed791-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="ed791-108">Közvetlen módszerek hello felhőből hívják.</span><span class="sxs-lookup"><span data-stu-id="ed791-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="ed791-109">Hozzon létre egy .NET-Konzolalkalmazás, amely behívja hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ed791-109">Create a .NET console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="ed791-110">Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="ed791-110">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="ed791-111">**dmpatterns_getstarted_device.js**, amely kapcsolódik tooyour IoT-központ hello eszközidentitás, korábban létrehozott kap egy újraindítás közvetlen módszer, fizikai újraindítás szimulálja, és hello idő az utolsó újraindítás hello jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="ed791-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="ed791-112">**TriggerReboot**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello választ, és megjeleníti hello frissítése jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ed791-112">**TriggerReboot**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="ed791-113">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ed791-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="ed791-114">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ed791-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="ed791-115">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="ed791-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="ed791-116">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="ed791-116">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="ed791-117">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ed791-117">An active Azure account.</span></span> <span data-ttu-id="ed791-118">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="ed791-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="ed791-119">Eseményindító közvetlen metódussal hello eszközön távoli újraindítás</span><span class="sxs-lookup"><span data-stu-id="ed791-119">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="ed791-120">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) közvetlen metódussal eszköz távoli újraindítást kezdeményezett hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ed791-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="ed791-121">hello alkalmazás eszköz iker lekérdezések toodiscover hello legutóbbi újraindítás használ, az eszköznek.</span><span class="sxs-lookup"><span data-stu-id="ed791-121">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="ed791-122">A Visual Studio, a Visual C# klasszikus Windows asztal projekt tooa új megoldás hozzáadása hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="ed791-122">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="ed791-123">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ed791-123">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="ed791-124">Név hello projekt **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="ed791-124">Name hello project **TriggerReboot**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

2. <span data-ttu-id="ed791-126">A Megoldáskezelőben kattintson a jobb gombbal hello **TriggerReboot** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="ed791-126">In Solution Explorer, right-click hello **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="ed791-127">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="ed791-127">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="ed791-128">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="ed791-128">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
4. <span data-ttu-id="ed791-130">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="ed791-130">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="ed791-131">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="ed791-131">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="ed791-132">Hello helyőrző értékét lecserélheti egy IoT-központ kapcsolati karakterlánc az előző szakaszban hello és hello céleszköz létrehozott hello központ hello.</span><span class="sxs-lookup"><span data-stu-id="ed791-132">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section and hello target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="ed791-133">Adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="ed791-133">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="ed791-134">A kód beolvassa hello eszköz iker az eszköz- és kimenetekkel hello újraindítás hello jelentett tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ed791-134">This code gets hello device twin for hello rebooting device and outputs hello reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="ed791-135">Adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="ed791-135">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="ed791-136">Ez a kód hello újraindítás közvetlen metódussal hello eszközön kezdeményezi.</span><span class="sxs-lookup"><span data-stu-id="ed791-136">This code initiates hello reboot on hello device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="ed791-137">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ed791-137">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. <span data-ttu-id="ed791-138">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ed791-138">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="ed791-139">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed791-139">Create a simulated device app</span></span>
<span data-ttu-id="ed791-140">Ez a szakasz tartalma</span><span class="sxs-lookup"><span data-stu-id="ed791-140">In this section, you will</span></span>

* <span data-ttu-id="ed791-141">Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="ed791-141">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="ed791-142">A szimulált eszköz újraindítását eseményindító</span><span class="sxs-lookup"><span data-stu-id="ed791-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="ed791-143">Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása</span><span class="sxs-lookup"><span data-stu-id="ed791-143">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="ed791-144">Hozzon létre egy új üres nevű **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="ed791-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="ed791-145">A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="ed791-145">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="ed791-146">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="ed791-146">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="ed791-147">A parancssorban hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="ed791-147">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="ed791-148">Egy szövegszerkesztő használatával hozzon létre egy új **dmpatterns_getstarted_device.js** hello fájlban **manageddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="ed791-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="ed791-149">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_device.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="ed791-149">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="ed791-150">Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="ed791-150">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="ed791-151">Hello kapcsolati karakterláncot cserélje le a eszköz kapcsolati karakterláncát.</span><span class="sxs-lookup"><span data-stu-id="ed791-151">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="ed791-152">Adja hozzá a következő függvény tooimplement hello közvetlen módszer hello eszközön hello</span><span class="sxs-lookup"><span data-stu-id="ed791-152">Add hello following function tooimplement hello direct method on hello device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. <span data-ttu-id="ed791-153">Adja hozzá a hello következő code tooopen hello kapcsolat tooyour IoT-központot, és indítsa el a hello közvetlen módszer figyelő:</span><span class="sxs-lookup"><span data-stu-id="ed791-153">Add hello following code tooopen hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. <span data-ttu-id="ed791-154">Mentse és zárja be a hello **dmpatterns_getstarted_device.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed791-154">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="ed791-155">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="ed791-155">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="ed791-156">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="ed791-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-hello-apps"></a><span data-ttu-id="ed791-157">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="ed791-157">Run hello apps</span></span>
<span data-ttu-id="ed791-158">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ed791-158">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="ed791-159">Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="ed791-159">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="ed791-160">Futtatási hello C# Konzolalkalmazás **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="ed791-160">Run hello C# console app **TriggerReboot**.</span></span> <span data-ttu-id="ed791-161">Kattintson a jobb gombbal hello **TriggerReboot** projektre, válassza **Debug**, majd válassza ki **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="ed791-161">Right-click hello **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="ed791-162">Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.</span><span class="sxs-lookup"><span data-stu-id="ed791-162">You see hello device response toohello direct method in hello console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/