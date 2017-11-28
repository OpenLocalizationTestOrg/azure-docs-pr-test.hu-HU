---
title: "aaaGet Azure IoT Hub eszköz twins (.NET/.NET) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooadd címkéket, majd az IoT Hub-lekérdezést. Hello Azure IoT-eszközök SDK használata .NET tooimplement hello szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely hello címkék hozzáadása, és az IoT-központ lekérdezés hello futtatja."
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
ms.openlocfilehash: 7fa73ac896c44e79c6522d252cd1515bd6e7bb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="cf8a5-104">Ismerkedés az eszköz twins (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="cf8a5-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="cf8a5-105">Ez az oktatóanyag végén hello hogy ezen .NET konzol alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-105">At hello end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="cf8a5-106">**CreateDeviceIdentity**, a .NET-alkalmazások ilyenkor létrejön egy eszközidentitás és a biztonsági kulcs tooconnect a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="cf8a5-107">**AddTagsAndQuery**, egy háttér-.NET-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="cf8a5-108">**ReportConnectivity**, egy eszköz .NET-alkalmazás, amely olyan eszköz, amely tooyour IoT-központ a korábban létrehozott hello eszközidentitás szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-108">**ReportConnectivity**, a .NET device app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="cf8a5-109">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="cf8a5-110">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-110">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="cf8a5-111">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="cf8a5-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-112">An active Azure account.</span></span> <span data-ttu-id="cf8a5-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="cf8a5-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="cf8a5-114">Ha azt szeretné toocreate hello eszközidentitás programozott módon helyette, olvassa el a megfelelő szakasz hello hello [csatlakoztassa a szimulált eszköz tooyour IoT hubot .NET használatával] [ lnk-device-identity-csharp] cikk.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-114">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="cf8a5-115">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf8a5-115">Create hello service app</span></span>
<span data-ttu-id="cf8a5-116">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely hely metaadatok toohello eszköz iker társított létrehozása **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-116">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="cf8a5-117">Ezután hello eszköz twins hello IoT-központ hello található hello eszközök kiválasztása tárolja SZÁMUNKRA, és majd hello azokon, mobilhálózat kapcsolatot jelentett lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-117">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="cf8a5-118">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-118">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="cf8a5-119">Név hello projekt **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-119">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="cf8a5-121">A Megoldáskezelőben kattintson a jobb gombbal hello **AddTagsAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="cf8a5-121">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="cf8a5-122">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-122">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="cf8a5-123">Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-123">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="cf8a5-124">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-124">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="cf8a5-126">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-126">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="cf8a5-127">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-127">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="cf8a5-128">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-128">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="cf8a5-129">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-129">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="cf8a5-130">Hello **RegistryManager** osztály összes hello módszerek szükséges toointeract az eszköz twins hello szolgáltatás elérhetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-130">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="cf8a5-131">hello előző kód először inicializálja hello **registryManager** objektumot, majd beolvassa az eszköz iker hello **myDeviceId**, és végül frissíti a címkék szükséges hello helyére vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-131">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="cf8a5-132">Miután frissített, két lekérdezést hajt végre: először a választ csak hello eszköz twins hello található eszközök hello **Redmond43** gépek és hello második refines hello lekérdezés tooselect csak hello csatlakozó eszközöket is keresztül mobilhálózati.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-132">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="cf8a5-133">Vegye figyelembe, hogy hello előző kóddal, amikor hello létrehozza **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-133">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="cf8a5-134">Hello **lekérdezés** objektum tartalmaz egy **HasMoreResults** használható tooinvoke hello logikai tulajdonság **GetNextAsTwinAsync** módszerek többször tooretrieve összes eredmények.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-134">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="cf8a5-135">A metódus hívása **GetNextAsJson** eredmények, amelyek például nem eszköz twins, összesítési-lekérdezések eredményének érhető el.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="cf8a5-136">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-136">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="cf8a5-137">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **AddTagsAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-137">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="cf8a5-138">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-138">Build hello solution.</span></span>
1. <span data-ttu-id="cf8a5-139">Az alkalmazás futtatásához kattintson a jobb gombbal a hello **AddTagsAndQuery** projektet, majd válassza **Debug**, utána pedig **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-139">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="cf8a5-140">A hello eredményei egy eszközhöz hello lekérdezés kérése minden eszköz a mappában lévő kell megjelennie **Redmond43** nincs hello lekérdezés, amely korlátozza a hello mobilhálózati használó toodevices elkészítéséig.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-140">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Lekérdezés eredményei ablakban][img-addtagapp]

<span data-ttu-id="cf8a5-142">Hello a következő szakaszban létrehoz egy eszköz-alkalmazást, amely hello kapcsolati információ jelent, és módosításokat hello hello előző szakaszban hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-142">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="cf8a5-143">Hello eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf8a5-143">Create hello device app</span></span>
<span data-ttu-id="cf8a5-144">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**, majd frissíti a jelentésben szereplő tulajdonságok toocontain hello adatait, hogy használatával mobilhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-144">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="cf8a5-145">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-145">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="cf8a5-146">Név hello projekt **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-146">Name hello project **ReportConnectivity**.</span></span>
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]
    
1. <span data-ttu-id="cf8a5-148">A Megoldáskezelőben kattintson a jobb gombbal hello **ReportConnectivity** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="cf8a5-148">In Solution Explorer, right-click hello **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="cf8a5-149">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-149">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="cf8a5-150">Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-150">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="cf8a5-151">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. <span data-ttu-id="cf8a5-153">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="cf8a5-154">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="cf8a5-155">Hello helyőrző értékét lecserélheti egy hello eszköz kapcsolati karakterláncot, amely az előző szakaszban hello feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-155">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="cf8a5-156">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-156">Add hello following method toohello **Program** class:</span></span>

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
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

    <span data-ttu-id="cf8a5-157">Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-157">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="cf8a5-158">a fent látható kód hello inicializál hello **ügyfél** objektumot, és lekéri hello eszköz iker a **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-158">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="cf8a5-159">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-159">Add hello following method toohello **Program** class:</span></span>
   
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

   <span data-ttu-id="cf8a5-160">frissítések fenti kódot hello **myDeviceId**által jelentett hello kapcsolati információ tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-160">hello code above updates **myDeviceId**'s reported property with hello connectivity information.</span></span>

1. <span data-ttu-id="cf8a5-161">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-161">Finally, add hello following lines toohello **Main** method:</span></span>
   
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
       Console.WriteLine("Press Enter tooexit.");
       Console.ReadLine();

1. <span data-ttu-id="cf8a5-162">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **ReportConnectivity** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-162">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="cf8a5-163">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-163">Build hello solution.</span></span>
1. <span data-ttu-id="cf8a5-164">Az alkalmazás futtatásához kattintson a jobb gombbal a hello **ReportConnectivity** projektet, majd válassza **Debug**, utána pedig **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-164">Run this application by right-clicking on hello **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="cf8a5-165">Hello iker adatainak lekérése, és elküldi a kapcsolattípust láthatja a *tulajdonság jelentett*.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-165">You should see it getting hello twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Futtassa az alkalmazást tooreport eszközkapcsolatok][img-rundeviceapp]
    
    
1. <span data-ttu-id="cf8a5-167">Most, hogy hello eszköz jelentette a kapcsolati információ meg kell jelennie mindkét lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-167">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="cf8a5-168">Futtassa a hello .NET **AddTagsAndQuery** app toorun hello le újra.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-168">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="cf8a5-169">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Sikeresen jelentett eszközkapcsolatok][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="cf8a5-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf8a5-171">Next steps</span></span>
<span data-ttu-id="cf8a5-172">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-172">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="cf8a5-173">Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és a szimulált eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-173">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="cf8a5-174">Azt is megtanulta, hogyan tooquery ezt az információt hello SQL-szerű IoT Hub lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-174">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="cf8a5-175">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="cf8a5-175">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="cf8a5-176">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="cf8a5-176">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="cf8a5-177">eszközök, eszköz iker kívánt tulajdonságok használata hello konfigurálása [használata szükséges tulajdonságok tooconfigure eszközök] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="cf8a5-177">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="cf8a5-178">interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="cf8a5-178">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

