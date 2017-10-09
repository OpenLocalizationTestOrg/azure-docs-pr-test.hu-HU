---
title: "Azure IoT Hub aaaUse közvetlen módszerek (.NET/.NET) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. Hello Azure IoT-eszközök SDK használata .NET tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: d4fa093a99558ec6faf294c2583a14a722b9ac03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnet"></a><span data-ttu-id="5505d-104">Közvetlen módszerekkel (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="5505d-104">Use direct methods (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="5505d-105">Ebben az oktatóanyagban dolgozunk folyamatos toodevelop két .NET konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="5505d-105">In this tutorial, we are going toodevelop two .NET console apps:</span></span>

* <span data-ttu-id="5505d-106">**CallMethodOnDevice**, egy háttér-alkalmazást, mert metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5505d-106">**CallMethodOnDevice**, a back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="5505d-107">**SimulateDeviceMethods**, egy konzolalkalmazás tooyour IoT-központ összeköti a korábban létrehozott hello eszközidentitás eszköz szimulálja, és válaszolhat hello felhő által meghívott toohello metódus.</span><span class="sxs-lookup"><span data-stu-id="5505d-107">**SimulateDeviceMethods**, a console app which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="5505d-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="5505d-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="5505d-109">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="5505d-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="5505d-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5505d-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="5505d-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="5505d-111">An active Azure account.</span></span> <span data-ttu-id="5505d-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="5505d-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="5505d-113">Ha azt szeretné toocreate hello eszközidentitás programozott módon helyette, olvassa el a megfelelő szakasz hello hello [csatlakoztassa a szimulált eszköz tooyour IoT hubot .NET használatával] [ lnk-device-identity-csharp] cikk.</span><span class="sxs-lookup"><span data-stu-id="5505d-113">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="5505d-114">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5505d-114">Create a simulated device app</span></span>
<span data-ttu-id="5505d-115">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódus hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5505d-115">In this section, you create a .NET console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="5505d-116">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="5505d-116">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="5505d-117">Név hello projekt **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="5505d-117">Name hello project **SimulateDeviceMethods**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]
    
1. <span data-ttu-id="5505d-119">A Megoldáskezelőben kattintson a jobb gombbal hello **SimulateDeviceMethods** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="5505d-119">In Solution Explorer, right-click hello **SimulateDeviceMethods** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="5505d-120">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="5505d-120">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="5505d-121">Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="5505d-121">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="5505d-122">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="5505d-122">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="5505d-124">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="5505d-124">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. <span data-ttu-id="5505d-125">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="5505d-125">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="5505d-126">Hello helyőrző értékét lecserélheti egy hello eszköz kapcsolati karakterláncot, amely az előző szakaszban hello feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="5505d-126">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="5505d-127">Adja hozzá a következő tooimplement hello közvetlen módszer hello eszközön hello:</span><span class="sxs-lookup"><span data-stu-id="5505d-127">Add hello following tooimplement hello direct method on hello device:</span></span>

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. <span data-ttu-id="5505d-128">Végül adja hozzá a következő kód toohello hello **fő** metódus tooopen hello kapcsolat tooyour IoT hub és inicializálási hello metódus figyelő:</span><span class="sxs-lookup"><span data-stu-id="5505d-128">Finally, add hello following code toohello **Main** method tooopen hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
        try
        {
            Console.WriteLine("Connecting toohub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter tooexit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove hello "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. <span data-ttu-id="5505d-129">A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **SimulateDeviceMethods** projektre a hello legördülő menüre.</span><span class="sxs-lookup"><span data-stu-id="5505d-129">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **SimulateDeviceMethods** project in hello dropdown menu.</span></span>        

> [!NOTE]
> <span data-ttu-id="5505d-130">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="5505d-130">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="5505d-131">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="5505d-131">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="5505d-132">A közvetlen metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="5505d-132">Call a direct method on a device</span></span>
<span data-ttu-id="5505d-133">Ebben a szakaszban egy .NET-Konzolalkalmazás metódus meghívja a hello szimulált eszköz app, majd megjeleníti a hello válasz hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5505d-133">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="5505d-134">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="5505d-134">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="5505d-135">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5505d-135">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="5505d-136">Név hello projekt **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="5505d-136">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createserviceapp]
2. <span data-ttu-id="5505d-138">A Megoldáskezelőben kattintson a jobb gombbal hello **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="5505d-138">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="5505d-139">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="5505d-139">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="5505d-140">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="5505d-140">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]

4. <span data-ttu-id="5505d-142">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="5505d-142">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="5505d-143">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="5505d-143">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="5505d-144">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5505d-144">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="5505d-145">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="5505d-145">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="5505d-146">Ez a metódus meghívja a közvetlen módszer nevű `writeLine` a hello `myDeviceId` eszköz.</span><span class="sxs-lookup"><span data-stu-id="5505d-146">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="5505d-147">Ezután ír a hello konzolon hello eszköz által biztosított hello válasz.</span><span class="sxs-lookup"><span data-stu-id="5505d-147">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="5505d-148">Ne feledje, hogy lehetséges toospecify hello eszköz toorespond időtúllépési értéket.</span><span class="sxs-lookup"><span data-stu-id="5505d-148">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="5505d-149">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="5505d-149">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="5505d-150">A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **CallMethodOnDevice** projektre a hello legördülő menüre.</span><span class="sxs-lookup"><span data-stu-id="5505d-150">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="5505d-151">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="5505d-151">Run hello applications</span></span>
<span data-ttu-id="5505d-152">Most már áll készen toorun hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5505d-152">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="5505d-153">Hello .NET eszköz alkalmazás **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="5505d-153">Run hello .NET device app **SimulateDeviceMethods**.</span></span> <span data-ttu-id="5505d-154">Akkor kell megkezdeni a figyelést az IoT Hub a metódushívások:</span><span class="sxs-lookup"><span data-stu-id="5505d-154">It should start listening for method calls from your IoT Hub:</span></span> 

    ![Eszköz-alkalmazás futtatása][img-deviceapprun]
1. <span data-ttu-id="5505d-156">Most, hogy hello eszköz csatlakoztatva van, és metódus meghívásához várakozik, futtassa a hello .NET **CallMethodOnDevice** tooinvoke hello használata hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5505d-156">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="5505d-157">Hello eszköz válasz írása hello konzolon kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="5505d-157">You should see hello device response written in hello console.</span></span>
   
    ![Szolgáltatás-alkalmazás futtatása][img-serviceapprun]
1. <span data-ttu-id="5505d-159">hello eszköz majd reagál toohello metódus által nyomtatás ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="5505d-159">hello device then reacts toohello method by printing this message:</span></span>
   
    ![Hello eszközön meghívott közvetlen módszer][img-directmethodinvoked]

## <a name="next-steps"></a><span data-ttu-id="5505d-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5505d-161">Next steps</span></span>
<span data-ttu-id="5505d-162">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="5505d-162">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="5505d-163">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta.</span><span class="sxs-lookup"><span data-stu-id="5505d-163">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="5505d-164">Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="5505d-164">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="5505d-165">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="5505d-165">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="5505d-166">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="5505d-166">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="5505d-167">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="5505d-167">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="5505d-168">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="5505d-168">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
