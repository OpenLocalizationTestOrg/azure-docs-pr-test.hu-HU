---
title: "aaaUse Azure IoT Hub iker tulajdonságai (.NET/.NET) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooconfigure eszközök. Hello Azure IoT-eszközök SDK a .NET tooimplement a szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs használja."
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
ms.openlocfilehash: 486436d29abfd5158c253adc5abf5935e0e1fdba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="d8b89-104">Használja a kívánt tulajdonságokkal tooconfigure eszközök</span><span class="sxs-lookup"><span data-stu-id="d8b89-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="d8b89-105">Ez az oktatóanyag végén hello hogy két .NET konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="d8b89-105">At hello end of this tutorial, you will have two .NET console apps:</span></span>

* <span data-ttu-id="d8b89-106">**SimulateDeviceConfiguration**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="d8b89-106">**SimulateDeviceConfiguration**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="d8b89-107">**SetDesiredConfigurationAndQuery**, egy háttér-alkalmazást, amely hello szükséges konfigurációs egy eszközön, és lekérdezések hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="d8b89-107">**SetDesiredConfigurationAndQuery**, a back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="d8b89-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="d8b89-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="d8b89-109">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="d8b89-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="d8b89-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d8b89-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d8b89-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8b89-111">An active Azure account.</span></span> <span data-ttu-id="d8b89-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d8b89-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="d8b89-113">Ha követte hello [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="d8b89-114">Ebben az esetben kihagyhatja toohello [létrehozás hello szimulált eszköz alkalmazásának] [ lnk-how-to-configure-createapp] szakasz.</span><span class="sxs-lookup"><span data-stu-id="d8b89-114">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="d8b89-115">Hello szimulált eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8b89-115">Create hello simulated device app</span></span>
<span data-ttu-id="d8b89-116">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések a szimulált hello konfigurációs frissítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="d8b89-116">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="d8b89-117">A Visual Studio, hozzon létre egy új Visual C# klasszikus Windows asztal projektet hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="d8b89-117">In Visual Studio, create a new Visual C# Windows Classic Desktop project by using hello **Console Application** project template.</span></span> <span data-ttu-id="d8b89-118">Név hello projekt **SimulateDeviceConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-118">Name hello project **SimulateDeviceConfiguration**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]

1. <span data-ttu-id="d8b89-120">A Megoldáskezelőben kattintson a jobb gombbal hello **SimulateDeviceConfiguration** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="d8b89-120">In Solution Explorer, right-click hello **SimulateDeviceConfiguration** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="d8b89-121">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="d8b89-122">Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d8b89-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="d8b89-123">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="d8b89-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="d8b89-125">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="d8b89-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="d8b89-126">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="d8b89-127">Hello helyőrző értékét lecserélheti egy hello eszköz kapcsolati karakterláncot, amely az előző szakaszban hello feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="d8b89-127">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. <span data-ttu-id="d8b89-128">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="d8b89-128">Add hello following method toohello **Program** class:</span></span>
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    <span data-ttu-id="d8b89-129">Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="d8b89-129">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="d8b89-130">a fent látható kód hello inicializál hello **ügyfél** objektumot, és lekéri hello eszköz iker a **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-130">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="d8b89-131">Adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-131">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="d8b89-132">Ez a módszer hello kezdőértéket telemetriai beállítja hello helyi eszközön, és majd frissítések hello eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="d8b89-132">This method sets hello initial values of telemetry on hello local device and then updates hello device twin.</span></span>

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

1. <span data-ttu-id="d8b89-133">Adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-133">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="d8b89-134">Ez egy visszahívást, amely észleli a változást az *szükséges tulajdonságok* a hello eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="d8b89-134">This is a callback which will detect a change in *desired properties* in hello device twin.</span></span>

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

    <span data-ttu-id="d8b89-135">A metódus frissítések hello jelentett hello helyi eszköz a két objektum hello konfiguráció tulajdonságainak túl kérelem és a készletek hello állapotának frissítése**függőben lévő**, majd a frissítések hello eszköz iker hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="d8b89-135">This method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="d8b89-136">Miután sikeresen frissített hello eszköz iker, hello metódus meghívásával befejezné hello konfiguráció módosításakor `CompleteConfigChange` ismertetett hello következő pontra.</span><span class="sxs-lookup"><span data-stu-id="d8b89-136">After successfully updating hello device twin, it completes hello config change by calling hello method `CompleteConfigChange` described in hello next point.</span></span>

1. <span data-ttu-id="d8b89-137">Adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-137">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="d8b89-138">Ez a módszer szimulálja egy eszköz alaphelyzetbe állítása, majd a frissítések helyi jelentett tulajdonságok hello állapotának beállításakor túl hello**sikeres** és eltávolítja hello **pendingConfig** elemet.</span><span class="sxs-lookup"><span data-stu-id="d8b89-138">This method simulates a device reset, then updates hello local reported properties setting hello status too**Success** and removes hello **pendingConfig** element.</span></span> <span data-ttu-id="d8b89-139">Majd frissíti a hello eszköz iker hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="d8b89-139">It then updates hello device twin on hello service.</span></span> 

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
                Console.WriteLine("Config change complete \nPress any key tooexit.");
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

1. <span data-ttu-id="d8b89-140">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="d8b89-140">Finally add hello following lines toohello **Main** method:</span></span>

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
   > <span data-ttu-id="d8b89-141">Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben.</span><span class="sxs-lookup"><span data-stu-id="d8b89-141">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="d8b89-142">Néhány konfigurációs frissítési folyamat előfordulhat, hogy képes tooaccommodate módosításainak konfigurációjához hello frissítés futása közben, előfordulhat, hogy rendelkeznek őket, és néhány sikerült utasítsa el azokat a hibaállapotot tooqueue.</span><span class="sxs-lookup"><span data-stu-id="d8b89-142">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="d8b89-143">Győződjön meg arról, hogy tooconsider hello kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logika hello hello konfigurációváltozás kezdeményezése előtt.</span><span class="sxs-lookup"><span data-stu-id="d8b89-143">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="d8b89-144">Hello megoldás kiépítését, majd futtassa az hello eszközalkalmazás a Visual Studio eszközből kattintva **F5**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-144">Build hello solution and then run hello device app from Visual Studio by clicking **F5**.</span></span> <span data-ttu-id="d8b89-145">Jelzi, hogy a szimulált eszköz hello eszköz iker beolvasása, hello telemetriai beállítása, és várakozik a kívánt tulajdonság módosításának köszönőüzenetei megtekintheti a hello kimeneti konzolon.</span><span class="sxs-lookup"><span data-stu-id="d8b89-145">On hello output console, you should see hello messages indicating that your simulated device is retrieving hello device twin, setting up hello telemetry, and waiting for desired property change.</span></span> <span data-ttu-id="d8b89-146">Folyamatosan futó hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d8b89-146">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="d8b89-147">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8b89-147">Create hello service app</span></span>
<span data-ttu-id="d8b89-148">Ez a szakasz során létrehoz egy .NET-Konzolalkalmazás, hogy a frissítések hello *szükségeskonfiguráció-tulajdonságok* a hello eszköz iker társított **myDeviceId** új telemetria-konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="d8b89-148">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="d8b89-149">Ezután hello eszköz twins hello IoT-központ tárolt lekérdezi és hello hello különbségének szükséges, és a jelentett hello eszköz konfigurációját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d8b89-149">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="d8b89-150">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="d8b89-150">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="d8b89-151">Név hello projekt **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-151">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="d8b89-153">A Megoldáskezelőben kattintson a jobb gombbal hello **SetDesiredConfigurationAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="d8b89-153">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="d8b89-154">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d8b89-154">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="d8b89-155">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="d8b89-155">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="d8b89-157">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="d8b89-157">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="d8b89-158">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-158">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="d8b89-159">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="d8b89-159">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="d8b89-160">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="d8b89-160">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="d8b89-161">Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="d8b89-161">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="d8b89-162">Ez a kód inicializálja hello **beállításjegyzék** objektum beolvassa az eszköz iker hello **myDeviceId**, majd frissíti a kívánt tulajdonságát egy új telemetriai configuration objektummal.</span><span class="sxs-lookup"><span data-stu-id="d8b89-162">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="d8b89-163">Ezt követően hello eszköz twins tárolt hello IoT-központ 10 másodpercenként kérdezi le, és megrendelése hello szükséges, és telemetriai konfigurációk jelentett.</span><span class="sxs-lookup"><span data-stu-id="d8b89-163">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="d8b89-164">Tekintse meg a toohello [IoT-központ lekérdezési nyelv] [ lnk-query] toolearn hogyan toogenerate gazdag jelent az eszközön.</span><span class="sxs-lookup"><span data-stu-id="d8b89-164">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="d8b89-165">Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="d8b89-165">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="d8b89-166">Használja több eszközt, és nem toodetect módosítások toogenerate felhasználók számára is elérhető jelentések lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="d8b89-166">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="d8b89-167">Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="d8b89-167">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="d8b89-168">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="d8b89-168">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="d8b89-169">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **SetDesiredConfigurationAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-169">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="d8b89-170">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d8b89-170">Build hello solution.</span></span>
1. <span data-ttu-id="d8b89-171">A **SimulateDeviceConfiguration** alkalmazást futtató eszköznek, futtatási hello service-alkalmazást a Visual Studio használatával **F5**.</span><span class="sxs-lookup"><span data-stu-id="d8b89-171">With **SimulateDeviceConfiguration** device app running, run hello service app from Visual Studio using **F5**.</span></span> <span data-ttu-id="d8b89-172">Megtekintheti az hello jelentett konfigurációs módosítást a **függőben lévő** túl**sikeres** hello új aktív a Küldés 24 óra helyett öt perces gyakoriságot.</span><span class="sxs-lookup"><span data-stu-id="d8b89-172">You should see hello reported configuration change from **Pending** too**Success** with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Eszköz sikeresen konfigurálva][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="d8b89-174">Nincs késleltetést mentése tooa perc közötti hello eszköz jelentés művelet és hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="d8b89-174">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="d8b89-175">Ez a tooenable hello lekérdezés infrastruktúra toowork nagyon nagy méretekben.</span><span class="sxs-lookup"><span data-stu-id="d8b89-175">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="d8b89-176">egy egyetlen eszközt iker tooretrieve konzisztens nézeteinek hello használata **getDeviceTwin** metódus a hello **beállításjegyzék** osztály.</span><span class="sxs-lookup"><span data-stu-id="d8b89-176">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="d8b89-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8b89-177">Next steps</span></span>
<span data-ttu-id="d8b89-178">Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* hello megoldásból háttér, és egy eszköz alkalmazás toodetect, módosítása és annak állapotát a jelentett hello reporting többlépéses frissítési folyamat szimulálása megírt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="d8b89-178">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="d8b89-179">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="d8b89-179">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="d8b89-180">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="d8b89-180">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="d8b89-181">ütemezhet, vagy hajtsa végre műveleteket a eszközök nagy mennyiségű, lásd: hello [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d8b89-181">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="d8b89-182">interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d8b89-182">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
