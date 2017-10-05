---
title: "Ismerkedés az Azure IoT Hub eszköz twins (.NET/.NET) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins címkéket, majd az IoT Hub-lekérdezést. Az Azure IoT-eszközök a .NET SDK használatával valósítja meg a szimulált eszköz alkalmazás és az Azure IoT szolgáltatás SDK for .NET egy szolgáltatás-alkalmazást, amely hozzáadja a címkéket és az IoT Hub-lekérdezés futtatása végrehajtásához."
services: iot-hub
documentationcenter: node
author: dsk-2015
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dkshir
ms.openlocfilehash: 6073d594117e69676b753a1e3af25fffa3583a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="18e2f-104">Ismerkedés az eszköz twins (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="18e2f-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="18e2f-105">Ez az oktatóanyag végén a konzol .NET alkalmazások lesz:</span><span class="sxs-lookup"><span data-stu-id="18e2f-105">At the end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="18e2f-106">**CreateDeviceIdentity**, ilyenkor létrejön egy eszközidentitás és a biztonsági kulcs a szimulált eszköz alkalmazás csatlakoztatása .NET-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="18e2f-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key to connect your simulated device app.</span></span>
* <span data-ttu-id="18e2f-107">**AddTagsAndQuery**, egy háttér-.NET-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="18e2f-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="18e2f-108">**ReportConnectivity**, egy eszköz .NET-alkalmazás, amely olyan eszköz, amely kapcsolódik a korábban létrehozott eszközidentitás az IoT hub szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="18e2f-108">**ReportConnectivity**, a .NET device app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="18e2f-109">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="18e2f-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="18e2f-110">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="18e2f-110">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="18e2f-111">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="18e2f-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="18e2f-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="18e2f-112">An active Azure account.</span></span> <span data-ttu-id="18e2f-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="18e2f-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="18e2f-114">Ha szeretne létrehozni az eszközidentitást programozott módon helyette, olvassa el a megfelelő részt a [a szimulált eszköz csatlakoztatása az IoT hub .NET használatával] [ lnk-device-identity-csharp] cikk.</span><span class="sxs-lookup"><span data-stu-id="18e2f-114">If you want to create the device identity programmatically instead, read the corresponding section in the [Connect your simulated device to your IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="18e2f-115">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="18e2f-115">Create the service app</span></span>
<span data-ttu-id="18e2f-116">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) a társított eszközök a két hely metaadatok hozzáadó létrehozása **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-116">In this section, you create a .NET console app (using C#) that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="18e2f-117">Ezután lekérdezi az eszköz twins tárolja az IoT hub, az eszközök az Egyesült Államok, és a gazdarendszerhez a mobilhálózat kapcsolat jelentett kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="18e2f-117">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="18e2f-118">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="18e2f-118">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="18e2f-119">Nevet a projektnek **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-119">Name the project **AddTagsAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="18e2f-121">A Megoldáskezelőben kattintson a jobb gombbal a **AddTagsAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="18e2f-121">In Solution Explorer, right-click the **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="18e2f-122">Az a **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-122">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="18e2f-123">Válassza ki **telepítése** telepítéséhez a **Microsoft.Azure.Devices** csomagot, majd fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="18e2f-123">Select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="18e2f-124">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="18e2f-124">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="18e2f-126">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="18e2f-126">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="18e2f-127">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="18e2f-127">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="18e2f-128">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="18e2f-128">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="18e2f-129">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="18e2f-129">Add the following method to the **Program** class:</span></span>
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    <span data-ttu-id="18e2f-130">A **RegistryManager** osztály eszköz twins a szolgáltatás együttműködhet szükséges összes módszert mutatja.</span><span class="sxs-lookup"><span data-stu-id="18e2f-130">The **RegistryManager** class exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="18e2f-131">Az előző kód először inicializálja a **registryManager** objektumot, majd beolvassa az eszköz iker a **myDeviceId**, és végül frissíti a címkék a kívánt helyre információkkal.</span><span class="sxs-lookup"><span data-stu-id="18e2f-131">The previous code first initializes the **registryManager** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="18e2f-132">Miután frissített, két lekérdezést hajt végre: az első csak az eszköz twins található eszközök kiválasztja a **Redmond43** gépek és a második rendszerint a lekérdezést csak azokat az eszközöket is keresztül mobilhálózati kapcsolódó kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="18e2f-132">After updating, it executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="18e2f-133">Vegye figyelembe, hogy az előző kód, amikor létrehozza a **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="18e2f-133">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="18e2f-134">A **lekérdezés** objektum tartalmaz egy **HasMoreResults** logikai tulajdonság, amely segítségével meghívni a **GetNextAsTwinAsync** módszerek több alkalommal fordult elő az összes eredmények beolvasásához.</span><span class="sxs-lookup"><span data-stu-id="18e2f-134">The **query** object contains a **HasMoreResults** boolean property that you can use to invoke the **GetNextAsTwinAsync** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="18e2f-135">A metódus hívása **GetNextAsJson** eredmények, amelyek például nem eszköz twins, összesítési-lekérdezések eredményének érhető el.</span><span class="sxs-lookup"><span data-stu-id="18e2f-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="18e2f-136">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="18e2f-136">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="18e2f-137">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **AddTagsAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-137">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="18e2f-138">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="18e2f-138">Build the solution.</span></span>
1. <span data-ttu-id="18e2f-139">Az alkalmazás futtatásához a jobb gombbal a **AddTagsAndQuery** projektet, majd válassza **Debug**, utána pedig **Start új példányt**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-139">Run this application by right-clicking on the **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="18e2f-140">Megjelenik az eredmények között egy eszközön a lekérdezés kérni a minden eszköz a mappában lévő **Redmond43** és a lekérdezés, amely korlátozza az eredmények mobilhálózati használó eszközök sem.</span><span class="sxs-lookup"><span data-stu-id="18e2f-140">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![Lekérdezés eredményei ablakban][img-addtagapp]

<span data-ttu-id="18e2f-142">A következő szakaszban hozzon létre egy eszköz alkalmazást, amely a kapcsolódási adatokat, és módosítja az előző szakaszban a lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="18e2f-142">In the next section, you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="18e2f-143">Az eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="18e2f-143">Create the device app</span></span>
<span data-ttu-id="18e2f-144">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**, majd frissíti a jelentett tulajdonságait, hogy mobilhálózat használata csatlakozik a információkat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="18e2f-144">In this section, you create a .NET console app that connects to your hub as **myDeviceId**, and then updates its reported properties to contain the information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="18e2f-145">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="18e2f-145">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="18e2f-146">Nevet a projektnek **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-146">Name the project **ReportConnectivity**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]
    
1. <span data-ttu-id="18e2f-148">A Megoldáskezelőben kattintson a jobb gombbal a **ReportConnectivity** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="18e2f-148">In Solution Explorer, right-click the **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="18e2f-149">Az a **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-149">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="18e2f-150">Válassza ki **telepítése** telepítéséhez a **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="18e2f-150">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="18e2f-151">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást a [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="18e2f-151">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="18e2f-153">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="18e2f-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="18e2f-154">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="18e2f-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="18e2f-155">Cserélje le a helyőrző értékét az előző szakaszban feljegyzett eszköz kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="18e2f-155">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="18e2f-156">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="18e2f-156">Add the following method to the **Program** class:</span></span>

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
                Console.WriteLine("Retrieving twin");
                await Client.GetTwinAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    <span data-ttu-id="18e2f-157">A **ügyfél** vezérlőnek az eszköz twins az eszközről interaktív szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="18e2f-157">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="18e2f-158">A fent látható kód inicializálja a **ügyfél** objektumot, és majd beolvassa az eszköz iker a **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-158">The code shown above, initializes the **Client** object, and then retrieves the device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="18e2f-159">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="18e2f-159">Add the following method to the **Program** class:</span></span>
   
        public static async void ReportConnectivity()
        {
            try
            {
                Console.WriteLine("Sending connectivity data as reported property");
                
                TwinCollection reportedProperties, connectivity;
                reportedProperties = new TwinCollection();
                connectivity = new TwinCollection();
                connectivity["type"] = "cellular";
                reportedProperties["connectivity"] = connectivity;
                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

   <span data-ttu-id="18e2f-160">A fenti frissítések kódot **myDeviceId**által jelentett tulajdonságot, amelyben a kapcsolati információ.</span><span class="sxs-lookup"><span data-stu-id="18e2f-160">The code above updates **myDeviceId**'s reported property with the connectivity information.</span></span>

1. <span data-ttu-id="18e2f-161">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="18e2f-161">Finally, add the following lines to the **Main** method:</span></span>
   
       try
       {
            InitClient();
            ReportConnectivity();
       }
       catch (Exception ex)
       {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
       }
       Console.WriteLine("Press Enter to exit.");
       Console.ReadLine();

1. <span data-ttu-id="18e2f-162">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **ReportConnectivity** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-162">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="18e2f-163">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="18e2f-163">Build the solution.</span></span>
1. <span data-ttu-id="18e2f-164">Az alkalmazás futtatásához a jobb gombbal a **ReportConnectivity** projektet, majd válassza **Debug**, utána pedig **Start új példányt**.</span><span class="sxs-lookup"><span data-stu-id="18e2f-164">Run this application by right-clicking on the **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="18e2f-165">A kettős adatainak lekérése, és elküldi a kapcsolattípust láthatja a *tulajdonság jelentett*.</span><span class="sxs-lookup"><span data-stu-id="18e2f-165">You should see it getting the twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Eszköz-alkalmazás futtatása jelentés kapcsolatához][img-rundeviceapp]
    
    
1. <span data-ttu-id="18e2f-167">Most, hogy az eszköz jelentette a kapcsolat adatait, akkor mindkét lekérdezések meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="18e2f-167">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="18e2f-168">Futtassa a .NET **AddTagsAndQuery** alkalmazásnak, hogy futtassa újból a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="18e2f-168">Run the .NET **AddTagsAndQuery** app to run the queries again.</span></span> <span data-ttu-id="18e2f-169">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="18e2f-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Sikeresen jelentett eszközkapcsolatok][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="18e2f-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18e2f-171">Next steps</span></span>
<span data-ttu-id="18e2f-172">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="18e2f-172">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="18e2f-173">Fel van véve eszköz metaadatait címkék egy háttér-alkalmazásból, és a szimulált eszköz alkalmazásának megírt az eszköz a két jelentés eszköz kapcsolódási adatok.</span><span class="sxs-lookup"><span data-stu-id="18e2f-173">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="18e2f-174">Megtudta, ezt az információt az SQL-szerű IoT Hub lekérdezési nyelv lekérdezése is.</span><span class="sxs-lookup"><span data-stu-id="18e2f-174">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="18e2f-175">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="18e2f-175">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="18e2f-176">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="18e2f-176">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="18e2f-177">eszköz iker kívánt tulajdonságokkal rendelkező eszközök konfigurálása a [használata szükséges eszközök tulajdonságok] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="18e2f-177">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="18e2f-178">az interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="18e2f-178">control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

