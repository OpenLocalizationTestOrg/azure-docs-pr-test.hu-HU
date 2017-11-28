---
title: "aaaGet Azure IoT Hub eszköz twins (.NET/csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooadd címkéket, majd az IoT Hub-lekérdezést. Hello Azure IoT-eszközök SDK használata Node.js tooimplement hello szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely hello címkék hozzáadása, és az IoT-központ lekérdezés hello futtatja."
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
ms.openlocfilehash: 1cec082ebddc19c9b87998a5fd0159d32b07acd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnode"></a><span data-ttu-id="4c99c-104">Ismerkedés az eszköz twins (.NET/csomópont)</span><span class="sxs-lookup"><span data-stu-id="4c99c-104">Get started with device twins (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="4c99c-105">Ez az oktatóanyag végén hello hogy a .NET- és egy Node.js-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="4c99c-105">At hello end of this tutorial, you will have a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="4c99c-106">**AddTagsAndQuery.sln**, a .NET-háttér-alkalmazás, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="4c99c-106">**AddTagsAndQuery.sln**, a .NET back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="4c99c-107">**TwinSimulatedDevice.js**, a Node.js-alkalmazás, amely olyan eszköz, amely tooyour IoT-központ a korábban létrehozott hello eszközidentitás szimulálja, és jelenti a kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="4c99c-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="4c99c-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4c99c-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="4c99c-109">toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c99c-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="4c99c-110">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4c99c-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="4c99c-111">A Node.js 0.10.x vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="4c99c-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="4c99c-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="4c99c-112">An active Azure account.</span></span> <span data-ttu-id="4c99c-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="4c99c-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="4c99c-114">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c99c-114">Create hello service app</span></span>
<span data-ttu-id="4c99c-115">Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely hely metaadatok toohello eszköz iker társított létrehozása **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-115">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="4c99c-116">Ezután hello eszköz twins hello IoT-központ hello található hello eszközök kiválasztása tárolja SZÁMUNKRA, és majd hello azokon, mobilhálózat kapcsolatot jelentett lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="4c99c-116">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="4c99c-117">A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="4c99c-117">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="4c99c-118">Név hello projekt **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-118">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. <span data-ttu-id="4c99c-120">A Megoldáskezelőben kattintson a jobb gombbal hello **AddTagsAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="4c99c-120">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="4c99c-121">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="4c99c-122">Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="4c99c-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="4c99c-123">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="4c99c-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. <span data-ttu-id="4c99c-125">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="4c99c-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="4c99c-126">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="4c99c-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="4c99c-127">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4c99c-127">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="4c99c-128">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="4c99c-128">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="4c99c-129">Hello **RegistryManager** osztály összes hello módszerek szükséges toointeract az eszköz twins hello szolgáltatás elérhetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="4c99c-129">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="4c99c-130">hello előző kód először inicializálja hello **registryManager** objektumot, majd beolvassa az eszköz iker hello **myDeviceId**, és végül frissíti a címkék szükséges hello helyére vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="4c99c-130">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="4c99c-131">Miután frissített, két lekérdezést hajt végre: először a választ csak hello eszköz twins hello található eszközök hello **Redmond43** gépek és hello második refines hello lekérdezés tooselect csak hello csatlakozó eszközöket is keresztül mobilhálózati.</span><span class="sxs-lookup"><span data-stu-id="4c99c-131">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="4c99c-132">Vegye figyelembe, hogy hello előző kóddal, amikor hello létrehozza **lekérdezés** objektumazonosító, a visszaadott dokumentumok maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4c99c-132">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="4c99c-133">Hello **lekérdezés** objektum tartalmaz egy **HasMoreResults** használható tooinvoke hello logikai tulajdonság **GetNextAsTwinAsync** módszerek többször tooretrieve összes eredmények.</span><span class="sxs-lookup"><span data-stu-id="4c99c-133">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="4c99c-134">A metódus hívása **GetNextAsJson** eredmények, amelyek például nem eszköz twins, összesítési-lekérdezések eredményének érhető el.</span><span class="sxs-lookup"><span data-stu-id="4c99c-134">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="4c99c-135">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="4c99c-135">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="4c99c-136">A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **AddTagsAndQuery** projekt **Start**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-136">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="4c99c-137">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4c99c-137">Build hello solution.</span></span>
1. <span data-ttu-id="4c99c-138">Az alkalmazás futtatásához kattintson a jobb gombbal a hello **AddTagsAndQuery** projektet, majd válassza **Debug**, utána pedig **Start új példány**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-138">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="4c99c-139">A hello eredményei egy eszközhöz hello lekérdezés kérése minden eszköz a mappában lévő kell megjelennie **Redmond43** nincs hello lekérdezés, amely korlátozza a hello mobilhálózati használó toodevices elkészítéséig.</span><span class="sxs-lookup"><span data-stu-id="4c99c-139">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Lekérdezés eredményei ablakban][img-addtagapp]

<span data-ttu-id="4c99c-141">Hello a következő szakaszban létrehoz egy eszköz-alkalmazást, amely hello kapcsolati információ jelent, és módosításokat hello hello előző szakaszban hello lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="4c99c-141">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="4c99c-142">Hello eszköz alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c99c-142">Create hello device app</span></span>
<span data-ttu-id="4c99c-143">Ebben a szakaszban egy Node.js-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**, majd frissíti a jelentésben szereplő tulajdonságok toocontain hello adatait, hogy használatával mobilhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4c99c-143">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="4c99c-144">Hozzon létre egy új üres nevű **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="4c99c-144">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="4c99c-145">A hello **reportconnectivity** mappa, hozzon létre egy új package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="4c99c-145">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="4c99c-146">Fogadja el az összes hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="4c99c-146">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="4c99c-147">A parancssorban hello **reportconnectivity** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök**, és **azure-iot-eszközök – mqtt** csomag :</span><span class="sxs-lookup"><span data-stu-id="4c99c-147">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="4c99c-148">Egy szövegszerkesztő használatával hozzon létre egy új **ReportConnectivity.js** hello fájlban **reportconnectivity** mappa.</span><span class="sxs-lookup"><span data-stu-id="4c99c-148">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
1. <span data-ttu-id="4c99c-149">Adja hozzá a következő kód toohello hello **ReportConnectivity.js** fájlt, és az eszköz kapcsolati karakterlánc egy hello létrehozása után másolja hello hello helyőrzője helyettesítse **myDeviceId** eszköz identitás:</span><span class="sxs-lookup"><span data-stu-id="4c99c-149">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello placeholder for device connection string with hello one you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="4c99c-150">Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="4c99c-150">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="4c99c-151">hello előző kód után inicializálja a hello **ügyfél** objektum beolvassa az eszköz iker hello **myDeviceId** , és frissíti a jelentett tulajdonsága hello kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="4c99c-151">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
1. <span data-ttu-id="4c99c-152">Hello eszköz alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4c99c-152">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="4c99c-153">Hello üzenet `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="4c99c-153">You should see hello message `twin state reported`.</span></span>
1. <span data-ttu-id="4c99c-154">Most, hogy hello eszköz jelentette a kapcsolati információ meg kell jelennie mindkét lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="4c99c-154">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="4c99c-155">Futtassa a hello .NET **AddTagsAndQuery** app toorun hello le újra.</span><span class="sxs-lookup"><span data-stu-id="4c99c-155">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="4c99c-156">Most **myDeviceId** mindkét lekérdezési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="4c99c-156">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][img-addtagapp2]

## <a name="next-steps"></a><span data-ttu-id="4c99c-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c99c-157">Next steps</span></span>
<span data-ttu-id="4c99c-158">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="4c99c-158">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="4c99c-159">Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és a szimulált eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a.</span><span class="sxs-lookup"><span data-stu-id="4c99c-159">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="4c99c-160">Azt is megtanulta, hogyan tooquery ezt az információt hello SQL-szerű IoT Hub lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="4c99c-160">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="4c99c-161">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="4c99c-161">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="4c99c-162">telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="4c99c-162">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="4c99c-163">eszközök, eszköz iker kívánt tulajdonságok használata hello konfigurálása [használata szükséges tulajdonságok tooconfigure eszközök] [ lnk-twin-how-to-configure] oktatóanyagban</span><span class="sxs-lookup"><span data-stu-id="4c99c-163">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="4c99c-164">interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c99c-164">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

