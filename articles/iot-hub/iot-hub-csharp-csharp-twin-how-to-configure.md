---
title: "Azure IoT Hub eszköz iker tulajdonságait (.NET/.NET) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins eszközök konfigurálásához. Az Azure IoT-eszközök a .NET SDK használatával valósítja meg a szimulált eszköz alkalmazások és az Azure IoT szolgáltatás SDK for .NET egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs végrehajtásához."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 679cda28bf3ce9fb207fe3693a3453b355f1de15
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="use-desired-properties-to-configure-devices"></a><span data-ttu-id="32e8a-104">Használni kívánt tulajdonságokat eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32e8a-104">Use desired properties to configure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="32e8a-105">Ez az oktatóanyag végén hogy két .NET konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="32e8a-105">At the end of this tutorial, you will have two .NET console apps:</span></span>

* <span data-ttu-id="32e8a-106">**SimulateDeviceConfiguration**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="32e8a-106">**SimulateDeviceConfiguration**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="32e8a-107">**SetDesiredConfigurationAndQuery**, egy háttér-alkalmazást, amely beállítja a kívánt konfiguráció egy eszközön, és lekérdezi a konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="32e8a-107">**SetDesiredConfigurationAndQuery**, a back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="32e8a-108">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="32e8a-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="32e8a-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="32e8a-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="32e8a-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="32e8a-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="32e8a-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="32e8a-111">An active Azure account.</span></span> <span data-ttu-id="32e8a-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="32e8a-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="32e8a-113">Ha követte a [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-113">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="32e8a-114">Ebben az esetben ugorjon a [a szimulált eszköz-alkalmazás létrehozása] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="32e8a-114">In that case, you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a><span data-ttu-id="32e8a-115">A szimulált eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="32e8a-115">Create the simulated device app</span></span>
<span data-ttu-id="32e8a-116">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések szimulált konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="32e8a-116">In this section, you create a .NET console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="32e8a-117">A Visual Studio, hozzon létre egy új Visual C# klasszikus Windows asztal projektet használatával a **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="32e8a-117">In Visual Studio, create a new Visual C# Windows Classic Desktop project by using the **Console Application** project template.</span></span> <span data-ttu-id="32e8a-118">Nevet a projektnek **SimulateDeviceConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-118">Name the project **SimulateDeviceConfiguration**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]

1. <span data-ttu-id="32e8a-120">A Megoldáskezelőben kattintson a jobb gombbal a **SimulateDeviceConfiguration** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="32e8a-120">In Solution Explorer, right-click the **SimulateDeviceConfiguration** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="32e8a-121">Az a **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-121">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="32e8a-122">Válassza ki **telepítése** telepítéséhez a **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="32e8a-122">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="32e8a-123">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást a [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="32e8a-123">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="32e8a-125">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="32e8a-125">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="32e8a-126">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="32e8a-126">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="32e8a-127">Cserélje le a helyőrző értékét az előző szakaszban feljegyzett eszköz kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="32e8a-127">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. <span data-ttu-id="32e8a-128">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="32e8a-128">Add the following method to the **Program** class:</span></span>
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    <span data-ttu-id="32e8a-129">A **ügyfél** vezérlőnek az eszköz twins az eszközről interaktív szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="32e8a-129">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="32e8a-130">A fent látható kód inicializálja a **ügyfél** objektumot, és majd beolvassa az eszköz iker a **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-130">The code shown above, initializes the **Client** object, and then retrieves the device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="32e8a-131">Adja hozzá a következő metódust a **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="32e8a-131">Add the following method to the **Program** class.</span></span> <span data-ttu-id="32e8a-132">Ez a módszer telemetriai adatot a kezdeti értékekre állítja be a helyi eszközön, és frissíti az eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="32e8a-132">This method sets the initial values of telemetry on the local device and then updates the device twin.</span></span>

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. <span data-ttu-id="32e8a-133">Adja hozzá a következő metódust a **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="32e8a-133">Add the following method to the **Program** class.</span></span> <span data-ttu-id="32e8a-134">Ez egy visszahívást, amely észleli a változást az *tulajdonságok szükséges* az eszköz iker a.</span><span class="sxs-lookup"><span data-stu-id="32e8a-134">This is a callback which will detect a change in *desired properties* in the device twin.</span></span>

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    <span data-ttu-id="32e8a-135">Ez a módszer a jelentett a helyi eszközön a két objektum tulajdonságainak frissítése a konfigurációs frissítési kérelmet az állapota, **függőben lévő**, majd frissíti az eszköz a két szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32e8a-135">This method updates the reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="32e8a-136">Miután sikeresen frissített a két eszköz, a metódus meghívásával befejezné a konfiguráció módosításakor `CompleteConfigChange` leírása a következő az.</span><span class="sxs-lookup"><span data-stu-id="32e8a-136">After successfully updating the device twin, it completes the config change by calling the method `CompleteConfigChange` described in the next point.</span></span>

1. <span data-ttu-id="32e8a-137">Adja hozzá a következő metódust a **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="32e8a-137">Add the following method to the **Program** class.</span></span> <span data-ttu-id="32e8a-138">Ez a módszer szimulálja egy eszköz alaphelyzetbe állítása, majd a frissítéseket a helyi jelentett az állapot tulajdonságok **sikeres** , és eltávolítja a **pendingConfig** elemet.</span><span class="sxs-lookup"><span data-stu-id="32e8a-138">This method simulates a device reset, then updates the local reported properties setting the status to **Success** and removes the **pendingConfig** element.</span></span> <span data-ttu-id="32e8a-139">Ezután frissíti az eszköz a két szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32e8a-139">It then updates the device twin on the service.</span></span> 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key to exit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. <span data-ttu-id="32e8a-140">Végül adja hozzá a következő sorokat a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="32e8a-140">Finally add the following lines to the **Main** method:</span></span>

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallback(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > <span data-ttu-id="32e8a-141">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="32e8a-141">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="32e8a-142">Néhány konfigurációs frissítési folyamat közben fut-e a frissítés, néhány lehet a várólistába helyezni őket, és néhány sikerült hibát elutasítása a célként megadott konfigurációs módosítások befogadásához lehet.</span><span class="sxs-lookup"><span data-stu-id="32e8a-142">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, some might have to queue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="32e8a-143">Ügyeljen arra, hogy fontolja meg a kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logikai kezdeményezése a konfiguráció módosítása előtt.</span><span class="sxs-lookup"><span data-stu-id="32e8a-143">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="32e8a-144">A megoldás felépítéséhez, és futtassa az eszköz alkalmazást a Visual Studio eszközből kattintva **F5**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-144">Build the solution and then run the device app from Visual Studio by clicking **F5**.</span></span> <span data-ttu-id="32e8a-145">A kimeneti konzolon megtekintheti az üzeneteket, jelezve, hogy a szimulált eszköz beolvassa az eszköz a két, telemetriai adatok beállításával és a kívánt tulajdonság módosításának várakozás.</span><span class="sxs-lookup"><span data-stu-id="32e8a-145">On the output console, you should see the messages indicating that your simulated device is retrieving the device twin, setting up the telemetry, and waiting for desired property change.</span></span> <span data-ttu-id="32e8a-146">Tartsa meg az alkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="32e8a-146">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="32e8a-147">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="32e8a-147">Create the service app</span></span>
<span data-ttu-id="32e8a-148">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely frissíti hoz létre a *szükséges tulajdonságok* meg az eszköz a két társított **myDeviceId** új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="32e8a-148">In this section, you will create a .NET console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="32e8a-149">Ezután lekérdezi az eszköz twins az IoT hub tárolja, és a kívánt és jelentett konfigurációkat, az eszköz közötti különbséget szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="32e8a-149">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="32e8a-150">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="32e8a-150">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="32e8a-151">Nevet a projektnek **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-151">Name the project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="32e8a-153">A Megoldáskezelőben kattintson a jobb gombbal a **SetDesiredConfigurationAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="32e8a-153">In Solution Explorer, right-click the **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="32e8a-154">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="32e8a-154">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="32e8a-155">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="32e8a-155">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="32e8a-157">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="32e8a-157">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="32e8a-158">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="32e8a-158">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="32e8a-159">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="32e8a-159">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="32e8a-160">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="32e8a-160">Add the following method to the **Program** class:</span></span>
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    <span data-ttu-id="32e8a-161">A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatás együttműködhet szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="32e8a-161">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="32e8a-162">Ezzel a kóddal inicializálja a **beállításjegyzék** objektumazonosító, beolvassa az eszköz iker a **myDeviceId**, majd frissíti a kívánt tulajdonságát egy új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="32e8a-162">This code initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="32e8a-163">Ezt követően azt lekérdezi az eszköz twins tárolja az IoT hub 10 másodpercenként, és kiírja a kívánt és jelentett telemetriai konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="32e8a-163">After that, it queries the device twins stored in the IoT hub every 10 seconds, and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="32e8a-164">Tekintse meg a [IoT-központ lekérdezési nyelv] [ lnk-query] további információt az eszközök közötti hatékony jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="32e8a-164">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="32e8a-165">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="32e8a-165">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="32e8a-166">Lekérdezésekkel számos eszközön keresztül a felhasználók számára is elérhető jelentések létrehozásához, és nem észleli a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="32e8a-166">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="32e8a-167">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="32e8a-167">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="32e8a-168">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="32e8a-168">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. <span data-ttu-id="32e8a-169">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **SetDesiredConfigurationAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-169">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="32e8a-170">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="32e8a-170">Build the solution.</span></span>
1. <span data-ttu-id="32e8a-171">A **SimulateDeviceConfiguration** futtató eszköznek az alkalmazás, szolgáltatás-alkalmazás futtatása a Visual Studio használatával **F5**.</span><span class="sxs-lookup"><span data-stu-id="32e8a-171">With **SimulateDeviceConfiguration** device app running, run the service app from Visual Studio using **F5**.</span></span> <span data-ttu-id="32e8a-172">Módosítsa a jelentésben szereplő konfigurációs kell megjelennie **függőben lévő** való **sikeres** az új aktív a Küldés 24 óra helyett öt perces gyakoriságot.</span><span class="sxs-lookup"><span data-stu-id="32e8a-172">You should see the reported configuration change from **Pending** to **Success** with the new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Eszköz sikeresen konfigurálva][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="32e8a-174">Nincs késleltetést legfeljebb egy percet, az eszköz jelentés művelet és a lekérdezési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="32e8a-174">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="32e8a-175">Ez a nagyon nagy méretekben működéséhez a lekérdezés infrastruktúra engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="32e8a-175">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="32e8a-176">Egy egyetlen eszközt iker használati konzisztens nézetek beolvasása a **getDeviceTwin** metódust a **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="32e8a-176">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="32e8a-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32e8a-177">Next steps</span></span>
<span data-ttu-id="32e8a-178">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* a megoldásban való háttér, és egy eszköz alkalmazás észleli a változást, és egy többlépéses frissítési folyamat állapotának jelentett tulajdonságai reporting szimulálása megírt.</span><span class="sxs-lookup"><span data-stu-id="32e8a-178">In this tutorial, you set a desired configuration as *desired properties* from the solution back end, and wrote a device app to detect that change and simulate a multi-step update process reporting its status through the reported properties.</span></span>

<span data-ttu-id="32e8a-179">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="32e8a-179">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="32e8a-180">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="32e8a-180">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="32e8a-181">ütemezett vagy műveleteket hajtson végre a nagy mennyiségű eszközök lásd: a [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="32e8a-181">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="32e8a-182">az interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközök szabályozásának a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="32e8a-182">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
