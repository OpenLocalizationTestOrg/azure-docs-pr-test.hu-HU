---
title: "Közvetlen Azure IoT Hub-módszerekkel (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub közvetlen módszerek. Az Azure IoT-eszközök SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazást, amely közvetlen módszer és a Azure IoT szolgáltatást a service-alkalmazást, amely hívja meg a közvetlen módszer végrehajtásához .NET SDK tartalmazza."
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
ms.openlocfilehash: ad705789a153381e21b2ccb05d4e0c17f78671fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-direct-methods-netnode"></a><span data-ttu-id="3b77a-104">Közvetlen módszerekkel (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="3b77a-104">Use direct methods (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="3b77a-105">Ebben az oktatóanyagban fogjuk a .NET- és egy Node.js-Konzolalkalmazás fejlesztéséhez:</span><span class="sxs-lookup"><span data-stu-id="3b77a-105">In this tutorial, we are going to develop a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="3b77a-106">**CallMethodOnDevice.sln**, .NET-háttér-alkalmazás, amely metódus meghívja a szimulált eszköz alkalmazásban, és a válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="3b77a-106">**CallMethodOnDevice.sln**, a .NET back-end app, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="3b77a-107">**SimulatedDevice.js**, a Node.js-alkalmazás, amely egy eszköz csatlakozik az IoT hub, korábban létrehozott eszköz identitású szimulálja, és válaszol-e a felhő által meghívott metódus.</span><span class="sxs-lookup"><span data-stu-id="3b77a-107">**SimulatedDevice.js**, a Node.js app, which simulates a device connecting to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="3b77a-108">Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="3b77a-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="3b77a-109">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3b77a-109">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="3b77a-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3b77a-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="3b77a-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="3b77a-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="3b77a-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="3b77a-112">An active Azure account.</span></span> <span data-ttu-id="3b77a-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="3b77a-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="3b77a-114">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3b77a-114">Create a simulated device app</span></span>
<span data-ttu-id="3b77a-115">Ebben a szakaszban hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a végfelhasználók által a megoldás háttérrendszere nevű metódust.</span><span class="sxs-lookup"><span data-stu-id="3b77a-115">In this section, you create a Node.js console app that responds to a method called by the solution back end.</span></span>

1. <span data-ttu-id="3b77a-116">Hozzon létre egy új, **simulateddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="3b77a-116">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="3b77a-117">A **simulateddevice** mappában hozza létre a package.json fájlt úgy, hogy beírja a következő parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="3b77a-117">In the **simulateddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="3b77a-118">Fogadja el az összes alapértelmezett beállítást:</span><span class="sxs-lookup"><span data-stu-id="3b77a-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="3b77a-119">A parancssorba a **simulateddevice** mappa telepítéséhez a következő parancsot a **azure iot-eszközök** és **azure-iot-eszközök – mqtt** csomagok:</span><span class="sxs-lookup"><span data-stu-id="3b77a-119">At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="3b77a-120">Egy szövegszerkesztővel, a fájl létrehozása a **simulateddevice** mappa, és adjon neki nevet **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="3b77a-120">Using a text editor, create a file in the **simulateddevice** folder and name it **SimulatedDevice.js**.</span></span>
4. <span data-ttu-id="3b77a-121">Adja hozzá a következő `require` utasításokat a **SimulatedDevice.js** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="3b77a-121">Add the following `require` statements at the start of the **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="3b77a-122">Adja hozzá a **connectionString** változó, és hozzon létre egy **DeviceClient** példány.</span><span class="sxs-lookup"><span data-stu-id="3b77a-122">Add a **connectionString** variable and use it to create a **DeviceClient** instance.</span></span> <span data-ttu-id="3b77a-123">Cserélje le **{eszköz kapcsolati karakterlánc}** az eszköz kapcsolattal karakterlánc Ön hozott létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="3b77a-123">Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="3b77a-124">Adja hozzá a következő függvény megvalósításához a közvetlen módszer az eszközön:</span><span class="sxs-lookup"><span data-stu-id="3b77a-124">Add the following function to implement the direct method on the device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="3b77a-125">Nyissa meg a kapcsolatot az IoT hub, és a metódus figyelő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="3b77a-125">Open the connection to your IoT hub and initialize the method listener:</span></span>
   
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
8. <span data-ttu-id="3b77a-126">Mentse és zárja be a **SimulatedDevice.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b77a-126">Save and close the **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="3b77a-127">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="3b77a-127">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="3b77a-128">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), az MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="3b77a-128">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="3b77a-129">A közvetlen metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="3b77a-129">Call a direct method on a device</span></span>
<span data-ttu-id="3b77a-130">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely egy metódus meghívja a szimulált eszköz alkalmazás és a válasz megjeleníti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3b77a-130">In this section, you create a .NET console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="3b77a-131">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="3b77a-131">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="3b77a-132">A Microsoft .NET-keretrendszer 4.5.1-es vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="3b77a-132">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="3b77a-133">Nevet a projektnek **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="3b77a-133">Name the project **CallMethodOnDevice**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. <span data-ttu-id="3b77a-135">A Megoldáskezelőben kattintson a jobb gombbal a **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="3b77a-135">In Solution Explorer, right-click the **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="3b77a-136">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="3b77a-136">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="3b77a-137">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="3b77a-137">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]

4. <span data-ttu-id="3b77a-139">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="3b77a-139">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="3b77a-140">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="3b77a-140">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="3b77a-141">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="3b77a-141">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="3b77a-142">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="3b77a-142">Add the following method to the **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="3b77a-143">Ez a metódus meghívja a közvetlen módszer nevű `writeLine` a a `myDeviceId` eszköz.</span><span class="sxs-lookup"><span data-stu-id="3b77a-143">This method invokes a direct method with name `writeLine` on the `myDeviceId` device.</span></span> <span data-ttu-id="3b77a-144">Ezután ír a választ, a konzolon az eszköz által biztosított.</span><span class="sxs-lookup"><span data-stu-id="3b77a-144">Then, it writes the response provided by the device on the console.</span></span> <span data-ttu-id="3b77a-145">Ne feledje, hogy az eszköz válaszára időtúllépési értéket.</span><span class="sxs-lookup"><span data-stu-id="3b77a-145">Note how it is possible to specify a timeout value for the device to respond.</span></span>
7. <span data-ttu-id="3b77a-146">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="3b77a-146">Finally, add the following lines to the **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a><span data-ttu-id="3b77a-147">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="3b77a-147">Run the applications</span></span>
<span data-ttu-id="3b77a-148">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="3b77a-148">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="3b77a-149">A Visual Studio Solution Explorerben kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** .</span><span class="sxs-lookup"><span data-stu-id="3b77a-149">In the Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**.</span></span> <span data-ttu-id="3b77a-150">Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a **CallMethodOnDevice** projekt legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="3b77a-150">Select **Single startup project**, and then select the **CallMethodOnDevice** project in the dropdown menu.</span></span>

2. <span data-ttu-id="3b77a-151">A parancsot a parancssorba a **simulateddevice** mappa megkezdeni a figyelést az IoT Hub a metódushívások a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3b77a-151">At a command prompt in the **simulateddevice** folder, run the following command to start listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   <span data-ttu-id="3b77a-152">Várjon, amíg a szimulált eszköz megnyitásához:![][7]</span><span class="sxs-lookup"><span data-stu-id="3b77a-152">Wait for the simulated device to open:  ![][7]</span></span>
3. <span data-ttu-id="3b77a-153">Most, hogy az eszköz csatlakoztatva van, és várakozik a metódus meghívásához, futtassa a .NET **CallMethodOnDevice** app a szimulált eszköz alkalmazás metódus meghívására.</span><span class="sxs-lookup"><span data-stu-id="3b77a-153">Now that the device is connected and waiting for method invocations, run the .NET **CallMethodOnDevice** app to invoke the method in the simulated device app.</span></span> <span data-ttu-id="3b77a-154">Az eszköz válasza a konzol írt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="3b77a-154">You should see the device response written in the console.</span></span>
   
    ![][8]
4. <span data-ttu-id="3b77a-155">Az eszköz majd reagál a metódus által nyomtatás ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="3b77a-155">The device then reacts to the method by printing this message:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="3b77a-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b77a-156">Next steps</span></span>
<span data-ttu-id="3b77a-157">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="3b77a-157">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="3b77a-158">Ez az eszközazonosító segítségével engedélyezheti a felhő által meghívott módszerek reagálni a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3b77a-158">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="3b77a-159">Létrehozott egy alkalmazást, amely meghívja a módszerek az eszközön, és megjeleníti az eszköz válaszára is.</span><span class="sxs-lookup"><span data-stu-id="3b77a-159">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="3b77a-160">További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:</span><span class="sxs-lookup"><span data-stu-id="3b77a-160">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="3b77a-161">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="3b77a-161">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="3b77a-162">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="3b77a-162">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="3b77a-163">Megtudhatja, hogyan terjeszthető ki az IoT-megoldás és az ütemezések metódushívások több eszközön, tekintse meg a [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3b77a-163">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
<span data-ttu-id="3b77a-164">[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="3b77a-164">[Get started with IoT Hub]: iot-hub-node-node-getstarted.md</span></span>
