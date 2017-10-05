---
title: "Ismerkedés az Azure IoT Hub eszköz twins (.NET/csomópont) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins címkéket, majd az IoT Hub-lekérdezést. Az Azure IoT-eszközök SDK for Node.js használatával valósítja meg a szimulált eszköz alkalmazás és az Azure IoT szolgáltatás SDK for .NET egy szolgáltatás-alkalmazást, amely hozzáadja a címkéket és az IoT Hub-lekérdezés futtatása végrehajtásához."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
ms.openlocfilehash: 07797b9159c9b926e9eb47d8864c63048951931a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-device-twins-netnode"></a><span data-ttu-id="a77bc-104">Ismerkedés az eszköz twins (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="a77bc-104">Get started with device twins (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="a77bc-105">Ez az oktatóanyag végén hogy a .NET- és egy Node.js-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="a77bc-105">At the end of this tutorial, you will have a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="a77bc-106">**AddTagsAndQuery.sln**, a .NET-háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="a77bc-106">**AddTagsAndQuery.sln**, a .NET back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="a77bc-107">**TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely egy eszköz, amely összeköti az IoT hub korábban létrehozott eszköz identitású szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="a77bc-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="a77bc-108">A cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="a77bc-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="a77bc-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a77bc-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="a77bc-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a77bc-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="a77bc-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a77bc-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="a77bc-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a77bc-112">An active Azure account.</span></span> <span data-ttu-id="a77bc-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="a77bc-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a><span data-ttu-id="a77bc-114">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a77bc-114">Create the service app</span></span>
<span data-ttu-id="a77bc-115">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#) a társított eszközök a két hely metaadatok hozzáadó létrehozása **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-115">In this section, you create a .NET console app (using C#) that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="a77bc-116">Ezután lekérdezi az eszköz twins tárolja az IoT hub, az eszközök az Egyesült Államok, és a gazdarendszerhez a mobilhálózat kapcsolat jelentett kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a77bc-116">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="a77bc-117">A Visual Studióban adjon hozzá egy Visual C# nyelvű Windows klasszikus asztalialkalmazás-projektet az aktuális megoldáshoz a **Console Application** (Konzolalkalmazás) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="a77bc-117">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="a77bc-118">Nevet a projektnek **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-118">Name the project **AddTagsAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="a77bc-120">A Megoldáskezelőben kattintson a jobb gombbal a **AddTagsAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="a77bc-120">In Solution Explorer, right-click the **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="a77bc-121">Az a **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-121">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="a77bc-122">Válassza ki **telepítése** telepítéséhez a **Microsoft.Azure.Devices** csomagot, majd fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="a77bc-122">Select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="a77bc-123">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="a77bc-123">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="a77bc-125">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="a77bc-125">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="a77bc-126">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="a77bc-126">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="a77bc-127">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="a77bc-127">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="a77bc-128">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="a77bc-128">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="a77bc-129">A **RegistryManager** osztály eszköz twins a szolgáltatás együttműködhet szükséges összes módszert mutatja.</span><span class="sxs-lookup"><span data-stu-id="a77bc-129">The **RegistryManager** class exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="a77bc-130">Az előző kód először inicializálja a **registryManager** objektumot, majd beolvassa az eszköz iker a **myDeviceId**, és végül frissíti a címkék a kívánt helyre információkkal.</span><span class="sxs-lookup"><span data-stu-id="a77bc-130">The previous code first initializes the **registryManager** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="a77bc-131">Miután frissített, két lekérdezést hajt végre: az első csak az eszköz twins található eszközök kiválasztja a **Redmond43** gépek és a második rendszerint a lekérdezést csak azokat az eszközöket is keresztül mobilhálózati kapcsolódó kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="a77bc-131">After updating, it executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="a77bc-132">Vegye figyelembe, hogy az előző kód, amikor létrehozza a **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a77bc-132">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="a77bc-133">A **lekérdezés** objektum tartalmaz egy **HasMoreResults** logikai tulajdonság, amely segítségével meghívni a **GetNextAsTwinAsync** módszerek több alkalommal fordult elő az összes eredmények beolvasásához.</span><span class="sxs-lookup"><span data-stu-id="a77bc-133">The **query** object contains a **HasMoreResults** boolean property that you can use to invoke the **GetNextAsTwinAsync** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="a77bc-134">A metódus hívása **GetNextAsJson** eredmények, amelyek például nem eszköz twins, összesítési-lekérdezések eredményének érhető el.</span><span class="sxs-lookup"><span data-stu-id="a77bc-134">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="a77bc-135">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="a77bc-135">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="a77bc-136">A Solution Explorerben nyissa meg a **állítsa be indítási projektek...**  , és győződjön meg arról, hogy a **művelet** a **AddTagsAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-136">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="a77bc-137">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a77bc-137">Build the solution.</span></span>
1. <span data-ttu-id="a77bc-138">Az alkalmazás futtatásához a jobb gombbal a **AddTagsAndQuery** projektet, majd válassza **Debug**, utána pedig **Start új példányt**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-138">Run this application by right-clicking on the **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="a77bc-139">Megjelenik az eredmények között egy eszközön a lekérdezés kérni a minden eszköz a mappában lévő **Redmond43** és a lekérdezés, amely korlátozza az eredmények mobilhálózati használó eszközök sem.</span><span class="sxs-lookup"><span data-stu-id="a77bc-139">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![Lekérdezés eredményei ablakban][img-addtagapp]

<span data-ttu-id="a77bc-141">A következő szakaszban hozzon létre egy eszköz alkalmazást, amely a kapcsolódási adatokat, és módosítja az előző szakaszban a lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="a77bc-141">In the next section, you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="a77bc-142">Az eszköz-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a77bc-142">Create the device app</span></span>
<span data-ttu-id="a77bc-143">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely kapcsolódik a hub, létrehozhat **myDeviceId**, majd frissíti a jelentett tulajdonságait, hogy mobilhálózat használata csatlakozik a információkat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="a77bc-143">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, and then updates its reported properties to contain the information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="a77bc-144">Hozzon létre egy új üres nevű **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="a77bc-144">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="a77bc-145">Az a **reportconnectivity** mappa, hozzon létre egy új package.json fájlt parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="a77bc-145">In the **reportconnectivity** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="a77bc-146">Fogadja el az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a77bc-146">Accept all the defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="a77bc-147">A parancssorba a **reportconnectivity** mappa telepítéséhez a következő parancsot a **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="a77bc-147">At your command prompt in the **reportconnectivity** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="a77bc-148">Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** fájlt a **reportconnectivity** mappa.</span><span class="sxs-lookup"><span data-stu-id="a77bc-148">Using a text editor, create a new **ReportConnectivity.js** file in the **reportconnectivity** folder.</span></span>
1. <span data-ttu-id="a77bc-149">Adja hozzá a következő kódot a **ReportConnectivity.js** fájlt, és az eszköz kapcsolati karakterlánc létrehozása után másolt helyőrző helyettesítse a **myDeviceId** eszközidentitás:</span><span class="sxs-lookup"><span data-stu-id="a77bc-149">Add the following code to the **ReportConnectivity.js** file, and substitute the placeholder for device connection string with the one you copied when you created the **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    <span data-ttu-id="a77bc-150">A **ügyfél** vezérlőnek az eszköz twins az eszközről interaktív szükséges összes módszert.</span><span class="sxs-lookup"><span data-stu-id="a77bc-150">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="a77bc-151">Az előző kód után állíthatja a **ügyfél** objektumazonosító, beolvassa az eszköz iker a **myDeviceId** , és frissíti a jelentett tulajdonsága a kapcsolati információ.</span><span class="sxs-lookup"><span data-stu-id="a77bc-151">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId** and updates its reported property with the connectivity information.</span></span>
1. <span data-ttu-id="a77bc-152">Az eszköz alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a77bc-152">Run the device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="a77bc-153">Az üzenet `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="a77bc-153">You should see the message `twin state reported`.</span></span>
1. <span data-ttu-id="a77bc-154">Most, hogy az eszköz jelentette a kapcsolat adatait, akkor mindkét lekérdezések meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="a77bc-154">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="a77bc-155">Futtassa a .NET **AddTagsAndQuery** alkalmazásnak, hogy futtassa újból a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="a77bc-155">Run the .NET **AddTagsAndQuery** app to run the queries again.</span></span> <span data-ttu-id="a77bc-156">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="a77bc-156">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][img-addtagapp2]

## <a name="next-steps"></a><span data-ttu-id="a77bc-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a77bc-157">Next steps</span></span>
<span data-ttu-id="a77bc-158">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="a77bc-158">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="a77bc-159">Fel van véve eszköz metaadatait címkék egy háttér-alkalmazásból, és a szimulált eszköz alkalmazásának megírt az eszköz a két jelentés eszköz kapcsolódási adatok.</span><span class="sxs-lookup"><span data-stu-id="a77bc-159">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="a77bc-160">Megtudta, ezt az információt az SQL-szerű IoT Hub lekérdezési nyelv lekérdezése is.</span><span class="sxs-lookup"><span data-stu-id="a77bc-160">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="a77bc-161">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="a77bc-161">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="a77bc-162">telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="a77bc-162">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="a77bc-163">eszköz iker kívánt tulajdonságokkal rendelkező eszközök konfigurálása a [használata szükséges eszközök tulajdonságok] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="a77bc-163">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="a77bc-164">az interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a77bc-164">control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

