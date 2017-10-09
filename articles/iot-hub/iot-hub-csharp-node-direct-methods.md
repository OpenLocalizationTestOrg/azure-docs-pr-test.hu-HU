---
title: "Azure IoT Hub aaaUse közvetlen módszerek (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. Hello Azure IoT-eszközök SDK használata Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK .NET tooimplement, amely hello közvetlen metódust hívja service-alkalmazást."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a><span data-ttu-id="9e7d1-104">Közvetlen módszerekkel (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="9e7d1-104">Use direct methods (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="9e7d1-105">Ebben az oktatóanyagban fogjuk toodevelop egy .NET- és egy Node.js-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-105">In this tutorial, we are going toodevelop a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="9e7d1-106">**CallMethodOnDevice.sln**, .NET-háttér-alkalmazás, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-106">**CallMethodOnDevice.sln**, a .NET back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="9e7d1-107">**SimulatedDevice.js**, a Node.js alkalmazást, mert egy eszköz tooyour IoT-központ összeköti a korábban létrehozott hello eszközidentitás szimulál, és a hello felhő által meghívott toohello metódus válaszol.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-107">**SimulatedDevice.js**, a Node.js app, which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="9e7d1-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="9e7d1-109">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="9e7d1-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="9e7d1-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="9e7d1-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-112">An active Azure account.</span></span> <span data-ttu-id="9e7d1-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="9e7d1-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="9e7d1-114">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9e7d1-114">Create a simulated device app</span></span>
<span data-ttu-id="9e7d1-115">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódus hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-115">In this section, you create a Node.js console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="9e7d1-116">Hozzon létre egy új, **simulateddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-116">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="9e7d1-117">A hello **simulateddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-117">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="9e7d1-118">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="9e7d1-119">A parancssorban hello **simulateddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-119">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="9e7d1-120">Egy szövegszerkesztő használatával hozzon létre egy fájlt hello **simulateddevice** mappa, és adjon neki nevet **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-120">Using a text editor, create a file in hello **simulateddevice** folder and name it **SimulatedDevice.js**.</span></span>
4. <span data-ttu-id="9e7d1-121">Adja hozzá a következő hello `require` hello utasításokat elejére hello **SimulatedDevice.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-121">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="9e7d1-122">Adja hozzá a **connectionString** változó, és toocreate használni egy **DeviceClient** példány.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-122">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="9e7d1-123">Cserélje le **{eszköz kapcsolati karakterlánc}** hello eszköz kapcsolati karakterlánccal hozta létre a hello *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-123">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="9e7d1-124">Adja hozzá a következő függvény tooimplement hello közvetlen módszer hello eszközön hello:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-124">Add hello following function tooimplement hello direct method on hello device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="9e7d1-125">Nyissa meg a hello kapcsolat tooyour IoT-központot, és hello metódus figyelő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-125">Open hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. <span data-ttu-id="9e7d1-126">Mentse és zárja be a hello **SimulatedDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-126">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="9e7d1-127">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-127">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="9e7d1-128">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="9e7d1-128">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="9e7d1-129">A közvetlen metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="9e7d1-129">Call a direct method on a device</span></span>
<span data-ttu-id="9e7d1-130">Ebben a szakaszban egy .NET-Konzolalkalmazás metódus meghívja a hello szimulált eszköz app, majd megjeleníti a hello válasz hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-130">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="9e7d1-131">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-131">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="9e7d1-132">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-132">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="9e7d1-133">Név hello projekt **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-133">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. <span data-ttu-id="9e7d1-135">A Megoldáskezelőben kattintson a jobb gombbal hello **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="9e7d1-135">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="9e7d1-136">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-136">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="9e7d1-137">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-137">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]

4. <span data-ttu-id="9e7d1-139">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-139">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="9e7d1-140">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-140">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="9e7d1-141">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-141">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="9e7d1-142">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-142">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="9e7d1-143">Ez a metódus meghívja a közvetlen módszer nevű `writeLine` a hello `myDeviceId` eszköz.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-143">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="9e7d1-144">Ezután ír a hello konzolon hello eszköz által biztosított hello válasz.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-144">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="9e7d1-145">Ne feledje, hogy lehetséges toospecify hello eszköz toorespond időtúllépési értéket.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-145">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="9e7d1-146">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-146">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

## <a name="run-hello-applications"></a><span data-ttu-id="9e7d1-147">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="9e7d1-147">Run hello applications</span></span>
<span data-ttu-id="9e7d1-148">Most már áll készen toorun hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-148">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="9e7d1-149">A Visual Studio Solution Explorer hello, kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** . Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a hello **CallMethodOnDevice** projektre a hello legördülő menüre.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-149">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

2. <span data-ttu-id="9e7d1-150">A parancssorba a hello **simulateddevice** mappa, futtassa a következő parancs toostart metódushívások az IoT-központ figyel hello:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-150">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   <span data-ttu-id="9e7d1-151">Várjon, amíg hello szimulált eszköz tooopen:![][7]</span><span class="sxs-lookup"><span data-stu-id="9e7d1-151">Wait for hello simulated device tooopen:  ![][7]</span></span>
3. <span data-ttu-id="9e7d1-152">Most, hogy hello eszköz csatlakoztatva van, és metódus meghívásához várakozik, futtassa a hello .NET **CallMethodOnDevice** tooinvoke hello használata hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-152">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="9e7d1-153">Hello eszköz válasz írása hello konzolon kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-153">You should see hello device response written in hello console.</span></span>
   
    ![][8]
4. <span data-ttu-id="9e7d1-154">hello eszköz majd reagál toohello metódus által nyomtatás ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-154">hello device then reacts toohello method by printing this message:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="9e7d1-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e7d1-155">Next steps</span></span>
<span data-ttu-id="9e7d1-156">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-156">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="9e7d1-157">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-157">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="9e7d1-158">Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-158">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="9e7d1-159">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="9e7d1-159">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="9e7d1-160">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="9e7d1-160">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="9e7d1-161">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="9e7d1-161">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="9e7d1-162">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="9e7d1-162">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md
