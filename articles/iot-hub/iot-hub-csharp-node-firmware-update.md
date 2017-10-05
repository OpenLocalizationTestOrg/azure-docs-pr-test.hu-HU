---
title: "Eszköz belső vezérlőprogram frissítése az Azure IoT Hub (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan használható az eszközkezelés Azure IoT hub eszköz vezérlőprogram-frissítés kezdeményezése. Az Azure IoT-eszközök SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazások és az Azure IoT szolgáltatás SDK for .NET egy service-alkalmazást, amely elindítja a belső vezérlőprogram-frissítés végrehajtásához."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2017
ms.author: juanpere
ms.openlocfilehash: c2192328a152e955d182c4a07b391c98a5960964
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a><span data-ttu-id="d1b74-104">Eszközkezelés használja indításához eszköz vezérlőprogram-frissítés (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="d1b74-104">Use device management to initiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="d1b74-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="d1b74-105">Introduction</span></span>
<span data-ttu-id="d1b74-106">Az a [Ismerkedés az eszközkezelés] [ lnk-dm-getstarted] oktatóanyag, megtudhatta, hogyan használható a [eszköz iker] [ lnk-devtwin] és [közvetlen módszerek ] [ lnk-c2dmethod] primitívek távolról az eszköz újraindítását.</span><span class="sxs-lookup"><span data-stu-id="d1b74-106">In the [Get started with device management][lnk-dm-getstarted] tutorial, you saw how to use the [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives to remotely reboot a device.</span></span> <span data-ttu-id="d1b74-107">Ez az oktatóanyag az ugyanazon az IoT-központ primitívek használ, és bemutatja, hogyan hajtsa végre egy végpontok közötti szimulált belső vezérlőprogram-frissítés.</span><span class="sxs-lookup"><span data-stu-id="d1b74-107">This tutorial uses the same IoT Hub primitives and shows you how to do an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="d1b74-108">A belső vezérlőprogram frissítési implementációja szerepel ebben a mintában a [málna Pi eszköz megvalósítási minta][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="d1b74-108">This pattern is used in the firmware update implementation for the [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="d1b74-109">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="d1b74-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="d1b74-110">A .NET-Konzolalkalmazás, amely behívja a firmwareUpdate közvetlen módszer a szimulált eszköz alkalmazás keresztül az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d1b74-110">Create a .NET console app that calls the firmwareUpdate direct method in the simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="d1b74-111">Hozzon létre egy szimulált eszköz alkalmazást, amely egy **firmwareUpdate** közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="d1b74-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="d1b74-112">Ez a módszer indít el egy többlépcsős folyamat, amely megvárja-e a belső vezérlőprogram lemezképet letölti, letölti a belső vezérlőprogram lemezképet és a belső vezérlőprogram kép végül vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d1b74-112">This method initiates a multi-stage process that waits to download the firmware image, downloads the firmware image, and finally applies the firmware image.</span></span> <span data-ttu-id="d1b74-113">A frissítés egyes szakasza alatt az eszköz használatával a jelentésben szereplő tulajdonságok előrehaladásról.</span><span class="sxs-lookup"><span data-stu-id="d1b74-113">During each stage of the update, the device uses the reported properties to report on progress.</span></span>

<span data-ttu-id="d1b74-114">Ez az oktatóanyag végén akkor egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="d1b74-114">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="d1b74-115">**dmpatterns_fwupdate_service.js**, amely közvetlen metódus meghívja a szimulált eszköz alkalmazás jeleníti meg a választ, és rendszeres időközönként (minden 500ms) jeleníti meg a frissített jelenteni tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="d1b74-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in the simulated device app, displays the response, and periodically (every 500ms) displays the updated reported properties.</span></span>

<span data-ttu-id="d1b74-116">**TriggerFWUpdate**, amely kapcsolódik az IoT hub, korábban létrehozott eszköz identitású kap egy firmwareUpdate közvetlen módszer esetén az több államot folyamatot, a belső vezérlőprogram frissítési például szimulálásához: Várakozás a lemezkép letöltése az új lemezkép letöltése, és végül a kép alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="d1b74-116">**TriggerFWUpdate**, which connects to your IoT hub with the device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process to simulate a firmware update including: waiting for the image download, downloading the new image, and finally applying the image.</span></span>

<span data-ttu-id="d1b74-117">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="d1b74-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="d1b74-118">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d1b74-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d1b74-119">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="d1b74-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="d1b74-120">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan telepítse a Node.js-ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="d1b74-120">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="d1b74-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d1b74-121">An active Azure account.</span></span> <span data-ttu-id="d1b74-122">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="d1b74-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="d1b74-123">Kövesse a [Ismerkedés az eszközkezelés](iot-hub-csharp-node-device-management-get-started.md) cikk az IoT hub létrehozása, és az IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="d1b74-123">Follow the [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article to create your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a><span data-ttu-id="d1b74-124">Indítás, az eszközön, a közvetlen módszer használatával távoli vezérlőprogram-frissítés</span><span class="sxs-lookup"><span data-stu-id="d1b74-124">Trigger a remote firmware update on the device using a direct method</span></span>
<span data-ttu-id="d1b74-125">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely indít el egy távoli belső vezérlőprogram frissítése egy eszközön hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1b74-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="d1b74-126">Az alkalmazás közvetlen módszer előtt használja a frissítés, és az eszköz iker lekérdezések rendszeres időközönként a aktív belső vezérlőprogram frissítése a állapot lekérdezése céljából.</span><span class="sxs-lookup"><span data-stu-id="d1b74-126">The app uses a direct method to initiate the update and uses device twin queries to periodically get the status of the active firmware update.</span></span>

1. <span data-ttu-id="d1b74-127">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="d1b74-127">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="d1b74-128">Nevet a projektnek **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="d1b74-128">Name the project **TriggerFWUpdate**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

1. <span data-ttu-id="d1b74-130">A Megoldáskezelőben kattintson a jobb gombbal a **TriggerFWUpdate** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="d1b74-130">In Solution Explorer, right-click the **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="d1b74-131">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d1b74-131">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="d1b74-132">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="d1b74-132">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="d1b74-134">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="d1b74-134">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="d1b74-135">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="d1b74-135">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="d1b74-136">Cserélje le a több helyőrző értékeket az előző szakaszban és az eszköz azonosítója a központ IoT-központ kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="d1b74-136">Replace the multiple placeholder values with the IoT Hub connection string for the hub that you created in the previous section and the Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="d1b74-137">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="d1b74-137">Add the following method to the **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="d1b74-138">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="d1b74-138">Add the following method to the **Program** class:</span></span>

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. <span data-ttu-id="d1b74-139">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="d1b74-139">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. <span data-ttu-id="d1b74-140">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **TriggerFWUpdate** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="d1b74-140">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="d1b74-141">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1b74-141">Build the solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a><span data-ttu-id="d1b74-142">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="d1b74-142">Run the apps</span></span>
<span data-ttu-id="d1b74-143">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="d1b74-143">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="d1b74-144">A parancssorban a **manageddevice** mappa, a következő parancsot a rendszer újraindítása közvetlen módszer figyelését.</span><span class="sxs-lookup"><span data-stu-id="d1b74-144">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="d1b74-145">A Visual Studióban, kattintson a jobb gombbal a a **TriggerFWUpdate** projectRun a C# konzolalkalmazást, hogy válasszon **Debug** és **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="d1b74-145">In Visual Studio, right-click on the **TriggerFWUpdate** projectRun to the C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="d1b74-146">A közvetlen módszer a konzolon eszköz válasz megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d1b74-146">You see the device response to the direct method in the console.</span></span>

    ![Belső vezérlőprogram frissítése sikeres volt][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="d1b74-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1b74-148">Next steps</span></span>
<span data-ttu-id="d1b74-149">Ez az oktatóanyag elindítása egy távoli belső vezérlőprogram frissítése egy eszközön a közvetlen módszer használatával, és a jelentésben szereplő tulajdonságok segítségével nyomon követheti a belső vezérlőprogram frissítése.</span><span class="sxs-lookup"><span data-stu-id="d1b74-149">In this tutorial, you used a direct method to trigger a remote firmware update on a device and used the reported properties to follow the progress of the firmware update.</span></span>

<span data-ttu-id="d1b74-150">Megtudhatja, hogyan terjeszthető ki az IoT-megoldás és az ütemezések metódushívások több eszközön, tekintse meg a [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d1b74-150">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/