---
title: "Ismerkedés az Azure IoT Hub kezelés (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub kezelés távoli eszköz újraindítás kezdeményezése. Az Azure IoT-eszközök SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazást, amely közvetlen módszer és a Azure IoT szolgáltatást a service-alkalmazást, amely hívja meg a közvetlen módszer végrehajtásához .NET SDK tartalmazza."
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
ms.openlocfilehash: d97fc5493570985f94c23032c870628d6a089dcd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="3eff4-104">Ismerkedés az Eszközfelügyelet (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="3eff4-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="3eff4-105">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="3eff4-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="3eff4-106">Az Azure-portál használatával létrehoz egy IoT-központot, és az IoT hub hozzon létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="3eff4-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="3eff4-107">Létrehoz egy szimulált eszköz alkalmazást, amely tartalmazza a közvetlen módszer, amely az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="3eff4-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="3eff4-108">Közvetlen módszerek a felhőből hívják.</span><span class="sxs-lookup"><span data-stu-id="3eff4-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="3eff4-109">A .NET-Konzolalkalmazás, amely behívja az újraindítás közvetlen módszer a szimulált eszköz alkalmazás keresztül az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3eff4-109">Create a .NET console app that calls the reboot direct method in the simulated device app through your IoT hub.</span></span>

<span data-ttu-id="3eff4-110">Ez az oktatóanyag végén akkor egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="3eff4-110">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="3eff4-111">**dmpatterns_getstarted_device.js**, amely kapcsolódik az IoT hub korábban létrehozott eszköz identitású kap egy újraindítás közvetlen módszer, szimulálja a fizikai számítógép újraindítása, és jelentést az idő az utolsó újraindítás.</span><span class="sxs-lookup"><span data-stu-id="3eff4-111">**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.</span></span>

<span data-ttu-id="3eff4-112">**TriggerReboot**, amely közvetlen metódus meghívja a szimulált eszköz alkalmazás jeleníti meg a választ, és jeleníti meg a frissített jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="3eff4-112">**TriggerReboot**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.</span></span>

<span data-ttu-id="3eff4-113">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3eff4-113">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="3eff4-114">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3eff4-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="3eff4-115">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="3eff4-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="3eff4-116">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan telepítse a Node.js-ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="3eff4-116">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="3eff4-117">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="3eff4-117">An active Azure account.</span></span> <span data-ttu-id="3eff4-118">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="3eff4-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="3eff4-119">Az eszközön, a közvetlen módszer használatával távoli újraindítás eseményindító</span><span class="sxs-lookup"><span data-stu-id="3eff4-119">Trigger a remote reboot on the device using a direct method</span></span>
<span data-ttu-id="3eff4-120">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) közvetlen metódussal eszköz távoli újraindítást kezdeményezett hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3eff4-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="3eff4-121">Az alkalmazás számára az eszköz legutóbbi újraindítás eszköz iker lekérdezések használ.</span><span class="sxs-lookup"><span data-stu-id="3eff4-121">The app uses device twin queries to discover the last reboot time for that device.</span></span>

1. <span data-ttu-id="3eff4-122">A Visual Studióban adjon hozzá egy Visual C# Windows klasszikus asztalialkalmazás-projektet az új megoldáshoz a **Console App (.NET Framework)** (Konzolalkalmazás (.NET-keretrendszer)) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="3eff4-122">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="3eff4-123">A Microsoft .NET-keretrendszer 4.5.1-es vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="3eff4-123">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="3eff4-124">Nevet a projektnek **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="3eff4-124">Name the project **TriggerReboot**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

2. <span data-ttu-id="3eff4-126">A Megoldáskezelőben kattintson a jobb gombbal a **TriggerReboot** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3eff4-126">In Solution Explorer, right-click the **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="3eff4-127">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="3eff4-127">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="3eff4-128">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="3eff4-128">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
4. <span data-ttu-id="3eff4-130">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="3eff4-130">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="3eff4-131">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="3eff4-131">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="3eff4-132">Cserélje le a helyőrző értékét az előző szakaszban és a céleszköz létrehozott központ IoT-központ kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="3eff4-132">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section and the target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="3eff4-133">Adja hozzá a következő metódust a **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="3eff4-133">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="3eff4-134">Ezt a kódot az eszköz iker lekérdezi az újraindítás eszköz, és kiírja a jelentésben szereplő tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="3eff4-134">This code gets the device twin for the rebooting device and outputs the reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="3eff4-135">Adja hozzá a következő metódust a **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="3eff4-135">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="3eff4-136">Ez a kód indít el az újraindítást követően az eszközön, a közvetlen módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="3eff4-136">This code initiates the reboot on the device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="3eff4-137">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="3eff4-137">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. <span data-ttu-id="3eff4-138">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3eff4-138">Build the solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="3eff4-139">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3eff4-139">Create a simulated device app</span></span>
<span data-ttu-id="3eff4-140">Ez a szakasz tartalma</span><span class="sxs-lookup"><span data-stu-id="3eff4-140">In this section, you will</span></span>

* <span data-ttu-id="3eff4-141">Egy Node.js-konzolalkalmazást hoz létre, amely a felhő által meghívott közvetlen metódusra válaszol</span><span class="sxs-lookup"><span data-stu-id="3eff4-141">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="3eff4-142">A szimulált eszköz újraindítását eseményindító</span><span class="sxs-lookup"><span data-stu-id="3eff4-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="3eff4-143">A jelentésben szereplő tulajdonságok használatával engedélyezhető az eszköz iker lekérdezések azonosíthatja azokat az eszközöket, és ha tartanak újraindítása után</span><span class="sxs-lookup"><span data-stu-id="3eff4-143">Use the reported properties to enable device twin queries to identify devices and when they last rebooted</span></span>

1. <span data-ttu-id="3eff4-144">Hozzon létre egy új üres nevű **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="3eff4-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="3eff4-145">A **manageddevice** mappában hozzon létre egy package.json fájlt úgy, hogy beírja a következő parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="3eff4-145">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="3eff4-146">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="3eff4-146">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="3eff4-147">Parancsot a parancssorba a a **manageddevice** mappa telepítéséhez a következő parancsot a **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="3eff4-147">At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="3eff4-148">Egy szövegszerkesztő használatával hozzon létre egy új **dmpatterns_getstarted_device.js** fájlt a **manageddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="3eff4-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in the **manageddevice** folder.</span></span>
4. <span data-ttu-id="3eff4-149">Adja hozzá a következő "szükséges" utasítások elején a **dmpatterns_getstarted_device.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="3eff4-149">Add the following 'require' statements at the start of the **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="3eff4-150">Adjon hozzá egy **connectionString** változót, és ezzel hozzon létre egy **Ügyfél** példányt.</span><span class="sxs-lookup"><span data-stu-id="3eff4-150">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  <span data-ttu-id="3eff4-151">Cserélje le a kapcsolati karakterlánc az eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3eff4-151">Replace the connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="3eff4-152">Az eszközön a közvetlen módszer végrehajtásához a következő függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3eff4-152">Add the following function to implement the direct method on the device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
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
7. <span data-ttu-id="3eff4-153">Adja hozzá a következő kódot az IoT hub a kapcsolat megnyitásához, és indítsa el a közvetlen módszer figyelő:</span><span class="sxs-lookup"><span data-stu-id="3eff4-153">Add the following code to open the connection to your IoT hub and start the direct method listener:</span></span>
   
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
8. <span data-ttu-id="3eff4-154">Mentse és zárja be a **dmpatterns_getstarted_device.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3eff4-154">Save and close the **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="3eff4-155">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="3eff4-155">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="3eff4-156">Az éles kódban újrapróbálkozási házirendeket is meg kell valósítania (például egy exponenciális leállítást) a [tranziens hibakezelést][lnk-transient-faults] ismertető MSDN-cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="3eff4-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-the-apps"></a><span data-ttu-id="3eff4-157">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="3eff4-157">Run the apps</span></span>
<span data-ttu-id="3eff4-158">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="3eff4-158">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="3eff4-159">A parancssorban a **manageddevice** mappa, a következő parancsot a rendszer újraindítása közvetlen módszer figyelését.</span><span class="sxs-lookup"><span data-stu-id="3eff4-159">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="3eff4-160">Futtassa a C# Konzolalkalmazás **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="3eff4-160">Run the C# console app **TriggerReboot**.</span></span> <span data-ttu-id="3eff4-161">Kattintson a jobb gombbal a **TriggerReboot** projektre, válassza **Debug**, majd válassza ki **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="3eff4-161">Right-click the **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="3eff4-162">A közvetlen módszer a konzolon eszköz válasz megjelenik.</span><span class="sxs-lookup"><span data-stu-id="3eff4-162">You see the device response to the direct method in the console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/