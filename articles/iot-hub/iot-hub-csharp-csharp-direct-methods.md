---
title: "Közvetlen Azure IoT Hub-módszerekkel (.NET/.NET) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub közvetlen módszerek. Az Azure IoT-eszközök a .NET SDK használatával valósítja meg a szimulált eszköz alkalmazást, amely közvetlen módszer és a Azure IoT szolgáltatást a service-alkalmazást, amely hívja meg a közvetlen módszer végrehajtásához .NET SDK tartalmazza."
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
ms.openlocfilehash: 9ce1fbebb6417c10618aa182e3c1d9ddf8132fb6
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="use-direct-methods-netnet"></a><span data-ttu-id="183ce-104">Közvetlen módszerekkel (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="183ce-104">Use direct methods (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="183ce-105">Ebben az oktatóanyagban fogjuk két .NET konzol alkalmazások fejlesztéséhez:</span><span class="sxs-lookup"><span data-stu-id="183ce-105">In this tutorial, we are going to develop two .NET console apps:</span></span>

* <span data-ttu-id="183ce-106">**CallMethodOnDevice**, egy háttér-alkalmazást, amely metódus meghívja a szimulált eszköz alkalmazásban, és a válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="183ce-106">**CallMethodOnDevice**, a back-end app, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="183ce-107">**SimulateDeviceMethods**, korábban létrehozott egy konzolalkalmazás szimulálja egy eszköz csatlakoztatása az IoT hub eszköz azonosítóját és a felhő által meghívott metódus megválaszolja.</span><span class="sxs-lookup"><span data-stu-id="183ce-107">**SimulateDeviceMethods**, a console app which simulates a device connecting to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="183ce-108">Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="183ce-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="183ce-109">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="183ce-109">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="183ce-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="183ce-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="183ce-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="183ce-111">An active Azure account.</span></span> <span data-ttu-id="183ce-112">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="183ce-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="183ce-113">Ha szeretne létrehozni az eszközidentitást programozott módon helyette, olvassa el a megfelelő részt a [a szimulált eszköz csatlakoztatása az IoT hub .NET használatával] [ lnk-device-identity-csharp] cikk.</span><span class="sxs-lookup"><span data-stu-id="183ce-113">If you want to create the device identity programmatically instead, read the corresponding section in the [Connect your simulated device to your IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="183ce-114">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="183ce-114">Create a simulated device app</span></span>
<span data-ttu-id="183ce-115">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely válaszol a végfelhasználók által a megoldás háttérrendszere nevű metódust hoz létre.</span><span class="sxs-lookup"><span data-stu-id="183ce-115">In this section, you create a .NET console app that responds to a method called by the solution back end.</span></span>

1. <span data-ttu-id="183ce-116">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="183ce-116">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="183ce-117">Nevet a projektnek **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="183ce-117">Name the project **SimulateDeviceMethods**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]
    
1. <span data-ttu-id="183ce-119">A Megoldáskezelőben kattintson a jobb gombbal a **SimulateDeviceMethods** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="183ce-119">In Solution Explorer, right-click the **SimulateDeviceMethods** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="183ce-120">Az a **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="183ce-120">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="183ce-121">Válassza ki **telepítése** telepítéséhez a **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="183ce-121">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="183ce-122">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást a [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="183ce-122">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="183ce-124">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="183ce-124">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. <span data-ttu-id="183ce-125">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="183ce-125">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="183ce-126">Cserélje le a helyőrző értékét az előző szakaszban feljegyzett eszköz kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="183ce-126">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="183ce-127">Adja hozzá a következőt valósítja meg a közvetlen az eszközön:</span><span class="sxs-lookup"><span data-stu-id="183ce-127">Add the following to implement the direct method on the device:</span></span>

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written to log.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. <span data-ttu-id="183ce-128">Végül adja hozzá az alábbi kódot a **fő** módszert, nyissa meg a kapcsolatot az IoT hub és a metódus figyelő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="183ce-128">Finally, add the following code to the **Main** method to open the connection to your IoT hub and initialize the method listener:</span></span>
   
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter to exit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove the "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. <span data-ttu-id="183ce-129">A Visual Studio Solution Explorerben kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** .</span><span class="sxs-lookup"><span data-stu-id="183ce-129">In the Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**.</span></span> <span data-ttu-id="183ce-130">Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a **SimulateDeviceMethods** projekt legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="183ce-130">Select **Single startup project**, and then select the **SimulateDeviceMethods** project in the dropdown menu.</span></span>        

> [!NOTE]
> <span data-ttu-id="183ce-131">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="183ce-131">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="183ce-132">Az éles kódban, meg kell valósítania újrapróbálkozási szabályzatok (például újrapróbálkozási), az MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="183ce-132">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="183ce-133">A közvetlen metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="183ce-133">Call a direct method on a device</span></span>
<span data-ttu-id="183ce-134">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely egy metódus meghívja a szimulált eszköz alkalmazás és a válasz megjeleníti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="183ce-134">In this section, you create a .NET console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="183ce-135">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="183ce-135">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="183ce-136">A Microsoft .NET-keretrendszer 4.5.1-es vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="183ce-136">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="183ce-137">Nevet a projektnek **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="183ce-137">Name the project **CallMethodOnDevice**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createserviceapp]
2. <span data-ttu-id="183ce-139">A Megoldáskezelőben kattintson a jobb gombbal a **CallMethodOnDevice** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="183ce-139">In Solution Explorer, right-click the **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="183ce-140">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="183ce-140">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="183ce-141">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="183ce-141">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]

4. <span data-ttu-id="183ce-143">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="183ce-143">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="183ce-144">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="183ce-144">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="183ce-145">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="183ce-145">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="183ce-146">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="183ce-146">Add the following method to the **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="183ce-147">Ez a metódus meghívja a közvetlen módszer nevű `writeLine` a a `myDeviceId` eszköz.</span><span class="sxs-lookup"><span data-stu-id="183ce-147">This method invokes a direct method with name `writeLine` on the `myDeviceId` device.</span></span> <span data-ttu-id="183ce-148">Ezután ír a választ, a konzolon az eszköz által biztosított.</span><span class="sxs-lookup"><span data-stu-id="183ce-148">Then, it writes the response provided by the device on the console.</span></span> <span data-ttu-id="183ce-149">Ne feledje, hogy az eszköz válaszára időtúllépési értéket.</span><span class="sxs-lookup"><span data-stu-id="183ce-149">Note how it is possible to specify a timeout value for the device to respond.</span></span>
7. <span data-ttu-id="183ce-150">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="183ce-150">Finally, add the following lines to the **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="183ce-151">A Visual Studio Solution Explorerben kattintson a jobb gombbal a megoldás, és kattintson **indítási projektek beállítása...** .</span><span class="sxs-lookup"><span data-stu-id="183ce-151">In the Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**.</span></span> <span data-ttu-id="183ce-152">Válassza ki **egyetlen kezdőprojekt**, majd válassza ki a **CallMethodOnDevice** projekt legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="183ce-152">Select **Single startup project**, and then select the **CallMethodOnDevice** project in the dropdown menu.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="183ce-153">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="183ce-153">Run the applications</span></span>
<span data-ttu-id="183ce-154">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="183ce-154">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="183ce-155">A .NET-eszköz alkalmazás futtatása **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="183ce-155">Run the .NET device app **SimulateDeviceMethods**.</span></span> <span data-ttu-id="183ce-156">Akkor kell megkezdeni a figyelést az IoT Hub a metódushívások:</span><span class="sxs-lookup"><span data-stu-id="183ce-156">It should start listening for method calls from your IoT Hub:</span></span> 

    ![Eszköz-alkalmazás futtatása][img-deviceapprun]
1. <span data-ttu-id="183ce-158">Most, hogy az eszköz csatlakoztatva van, és várakozik a metódus meghívásához, futtassa a .NET **CallMethodOnDevice** app a szimulált eszköz alkalmazás metódus meghívására.</span><span class="sxs-lookup"><span data-stu-id="183ce-158">Now that the device is connected and waiting for method invocations, run the .NET **CallMethodOnDevice** app to invoke the method in the simulated device app.</span></span> <span data-ttu-id="183ce-159">Az eszköz válasza a konzol írt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="183ce-159">You should see the device response written in the console.</span></span>
   
    ![Szolgáltatás-alkalmazás futtatása][img-serviceapprun]
1. <span data-ttu-id="183ce-161">Az eszköz majd reagál a metódus által nyomtatás ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="183ce-161">The device then reacts to the method by printing this message:</span></span>
   
    ![Az eszközön meghívott közvetlen módszer][img-directmethodinvoked]

## <a name="next-steps"></a><span data-ttu-id="183ce-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="183ce-163">Next steps</span></span>
<span data-ttu-id="183ce-164">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="183ce-164">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="183ce-165">Ez az eszközazonosító segítségével engedélyezheti a felhő által meghívott módszerek reagálni a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="183ce-165">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="183ce-166">Létrehozott egy alkalmazást, amely meghívja a módszerek az eszközön, és megjeleníti az eszköz válaszára is.</span><span class="sxs-lookup"><span data-stu-id="183ce-166">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="183ce-167">További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:</span><span class="sxs-lookup"><span data-stu-id="183ce-167">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="183ce-168">[Ismerkedés az IoT-központ]</span><span class="sxs-lookup"><span data-stu-id="183ce-168">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="183ce-169">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="183ce-169">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="183ce-170">Megtudhatja, hogyan terjeszthető ki az IoT-megoldás és az ütemezések metódushívások több eszközön, tekintse meg a [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="183ce-170">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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

<span data-ttu-id="183ce-171">[Ismerkedés az IoT-központ]: iot-hub-node-node-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="183ce-171">[Get started with IoT Hub]: iot-hub-node-node-getstarted.md</span></span>
