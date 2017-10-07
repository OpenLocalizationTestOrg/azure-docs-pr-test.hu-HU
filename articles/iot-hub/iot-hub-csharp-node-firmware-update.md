---
title: "az Azure IoT Hub (.NET/csomópont) aaaDevice vezérlőprogram-frissítés |} Microsoft Docs"
description: "Hogyan toouse kezelés az Azure IoT Hub tooinitiate egy eszköz belső vezérlőprogram frissítése. Hello Azure IoT-eszközök SDK a Node.js tooimplement a szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement service-alkalmazást, amely elindítja a hello belső vezérlőprogram frissítési használja."
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
ms.openlocfilehash: d48899651bcd5d67e577c971be0f9453c8f21592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a><span data-ttu-id="c3971-104">Használja az eszköz felügyeleti tooinitiate eszköz vezérlőprogram-frissítés (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="c3971-104">Use device management tooinitiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="c3971-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c3971-105">Introduction</span></span>
<span data-ttu-id="c3971-106">A hello [Ismerkedés az eszközkezelés] [ lnk-dm-getstarted] az oktatóanyagban megtudhatta, hogyan toouse hello [eszköz iker] [ lnk-devtwin] és [közvetlen módszerek] [ lnk-c2dmethod] primitívek tooremotely eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="c3971-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="c3971-107">A oktatóanyag használja az ugyanazon az IoT-központ primitívek hello, és bemutatja, hogyan toodo egy-végpontok szimulált vezérlőprogram-frissítés.</span><span class="sxs-lookup"><span data-stu-id="c3971-107">This tutorial uses hello same IoT Hub primitives and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="c3971-108">Ebben a mintában hello belső vezérlőprogram-frissítés végrehajtása során használatos hello [málna Pi eszköz megvalósítási minta][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="c3971-108">This pattern is used in hello firmware update implementation for hello [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="c3971-109">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="c3971-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="c3971-110">Hozzon létre egy .NET-Konzolalkalmazás, amely behívja hello firmwareUpdate közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c3971-110">Create a .NET console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="c3971-111">Hozzon létre egy szimulált eszköz alkalmazást, amely egy **firmwareUpdate** közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="c3971-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="c3971-112">Ez a módszer indít el egy többlépcsős folyamat, amely toodownload hello belső vezérlőprogram kép vár, hello belső vezérlőprogram lemezképet letölti és végül a hello belső vezérlőprogram kép vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c3971-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="c3971-113">Minden egyes hello frissítés szakaszában hello eszköz által használt hello jelentett tulajdonságok tooreport folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="c3971-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="c3971-114">Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="c3971-114">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="c3971-115">**dmpatterns_fwupdate_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello válasz, és rendszeres időközönként (minden 500ms) frissítése megjeleníti hello jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="c3971-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="c3971-116">**TriggerFWUpdate**, tooyour IoT-központ köti össze a korábban létrehozott hello eszközidentitás firmwareUpdate közvetlen metódus kap, egy több államot folyamat toosimulate keresztül futtat, a belső vezérlőprogram frissítési többek között: hello lemezkép letöltése, vár hello új lemezképet, és végül alkalmazása hello lemezkép letöltése.</span><span class="sxs-lookup"><span data-stu-id="c3971-116">**TriggerFWUpdate**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="c3971-117">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c3971-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c3971-118">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c3971-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="c3971-119">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="c3971-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="c3971-120">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="c3971-120">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="c3971-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c3971-121">An active Azure account.</span></span> <span data-ttu-id="c3971-122">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="c3971-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="c3971-123">Hajtsa végre a hello [Ismerkedés az eszközkezelés](iot-hub-csharp-node-device-management-get-started.md) cikk toocreate az IoT hub és az IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c3971-123">Follow hello [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="c3971-124">Indítás, közvetlen metódussal hello eszközön távoli vezérlőprogram-frissítés</span><span class="sxs-lookup"><span data-stu-id="c3971-124">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="c3971-125">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely indít el egy távoli belső vezérlőprogram frissítése egy eszközön hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c3971-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="c3971-126">hello alkalmazása használja-e a közvetlen módszer tooinitiate hello frissítése és által használt eszköz iker lekérdezések tooperiodically hello aktív belső vezérlőprogram frissítése hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c3971-126">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="c3971-127">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="c3971-127">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="c3971-128">Név hello projekt **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="c3971-128">Name hello project **TriggerFWUpdate**.</span></span>

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

1. <span data-ttu-id="c3971-130">A Megoldáskezelőben kattintson a jobb gombbal hello **TriggerFWUpdate** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="c3971-130">In Solution Explorer, right-click hello **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="c3971-131">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="c3971-131">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="c3971-132">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="c3971-132">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="c3971-134">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="c3971-134">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="c3971-135">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="c3971-135">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="c3971-136">Cserélje le a helyőrző értékeket több hello IoT-központ kapcsolati karakterlánc hello központ hello előző szakaszban létrehozott hello és hello az eszköz azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c3971-136">Replace hello multiple placeholder values with hello IoT Hub connection string for hello hub that you created in hello previous section and hello Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="c3971-137">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="c3971-137">Add hello following method toohello **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="c3971-138">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="c3971-138">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="c3971-139">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="c3971-139">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. <span data-ttu-id="c3971-140">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **TriggerFWUpdate** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="c3971-140">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="c3971-141">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3971-141">Build hello solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="c3971-142">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="c3971-142">Run hello apps</span></span>
<span data-ttu-id="c3971-143">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c3971-143">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="c3971-144">Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="c3971-144">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="c3971-145">A Visual Studióban, kattintson a jobb gombbal a hello **TriggerFWUpdate** projectRun toohello C# konzolalkalmazást, jelölje be **Debug** és **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="c3971-145">In Visual Studio, right-click on hello **TriggerFWUpdate** projectRun toohello C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="c3971-146">Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.</span><span class="sxs-lookup"><span data-stu-id="c3971-146">You see hello device response toohello direct method in hello console.</span></span>

    ![Belső vezérlőprogram frissítése sikeres volt][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="c3971-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3971-148">Next steps</span></span>
<span data-ttu-id="c3971-149">Ebben az oktatóanyagban egy közvetlen módszer tootrigger távoli használt belső vezérlőprogram frissítése egy eszközön, és a használt hello jelentett tulajdonságok toofollow hello hello belső vezérlőprogram frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="c3971-149">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="c3971-150">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c3971-150">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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