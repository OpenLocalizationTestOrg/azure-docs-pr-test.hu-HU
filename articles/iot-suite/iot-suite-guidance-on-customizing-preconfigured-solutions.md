---
title: "Testreszabás előre konfigurált megoldások |} Microsoft Docs"
description: "Hogyan szabhatja testre az Azure IoT Suite előre konfigurált megoldásokat nyújt útmutatást."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: bdf4cd89d5ad0392337dfe761108608d506adf18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="customize-a-preconfigured-solution"></a><span data-ttu-id="2382a-103">Előre konfigurált megoldás testreszabása</span><span class="sxs-lookup"><span data-stu-id="2382a-103">Customize a preconfigured solution</span></span>

<span data-ttu-id="2382a-104">Az Azure IoT Suite előre konfigurált megoldásai belül a csomaggal együtt működve biztosítanak egy végpont megoldás a szolgáltatások bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="2382a-104">The preconfigured solutions provided with the Azure IoT Suite demonstrate the services within the suite working together to deliver an end-to-end solution.</span></span> <span data-ttu-id="2382a-105">A kiindulási pont, a különböző helyek, amelyeknek kiterjesztése és a megoldás az adott forgatókönyveket testreszabása vannak.</span><span class="sxs-lookup"><span data-stu-id="2382a-105">From this starting point, there are various places in which you can extend and customize the solution for specific scenarios.</span></span> <span data-ttu-id="2382a-106">A következő szakaszok ismertetik a közös testreszabási pontok.</span><span class="sxs-lookup"><span data-stu-id="2382a-106">The following sections describe these common customization points.</span></span>

## <a name="find-the-source-code"></a><span data-ttu-id="2382a-107">A Forráskód keresése</span><span class="sxs-lookup"><span data-stu-id="2382a-107">Find the source code</span></span>

<span data-ttu-id="2382a-108">Az előkonfigurált megoldás forráskódját a következő tárházak a githubon érhető el:</span><span class="sxs-lookup"><span data-stu-id="2382a-108">The source code for the preconfigured solutions is available on GitHub in the following repositories:</span></span>

* <span data-ttu-id="2382a-109">Távoli megfigyelési: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span><span class="sxs-lookup"><span data-stu-id="2382a-109">Remote Monitoring: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span></span>
* <span data-ttu-id="2382a-110">A prediktív karbantartási: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span><span class="sxs-lookup"><span data-stu-id="2382a-110">Predictive Maintenance: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span></span>
* <span data-ttu-id="2382a-111">Csatlakoztatott gyári: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span><span class="sxs-lookup"><span data-stu-id="2382a-111">Connected factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span></span>

<span data-ttu-id="2382a-112">Az előkonfigurált megoldás forráskódját bemutatása a minták és gyakorlatok összessége valósította meg a végpont funkciót egy IoT-megoldás Azure IoT Suite segítségével valósul meg.</span><span class="sxs-lookup"><span data-stu-id="2382a-112">The source code for the preconfigured solutions is provided to demonstrate the patterns and practices used to implement the end-to-end functionality of an IoT solution using Azure IoT Suite.</span></span> <span data-ttu-id="2382a-113">További információ és a GitHub-adattárak a megoldások üzembe található.</span><span class="sxs-lookup"><span data-stu-id="2382a-113">You can find more information about how to build and deploy the solutions in the GitHub repositories.</span></span>

## <a name="change-the-preconfigured-rules"></a><span data-ttu-id="2382a-114">Az előre konfigurált szabályai módosítása</span><span class="sxs-lookup"><span data-stu-id="2382a-114">Change the preconfigured rules</span></span>

<span data-ttu-id="2382a-115">A távoli felügyeleti megoldás tartalmaz három [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) feladatok eszközadatokat, telemetriai adatok és a megoldás szabálylogika kezelésére.</span><span class="sxs-lookup"><span data-stu-id="2382a-115">The remote monitoring solution includes three [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobs to handle device information, telemetry, and rules logic in the solution.</span></span>

<span data-ttu-id="2382a-116">A három stream analytics-feladatok és szintaxisát a részletesen ismerteti a [távoli megfigyelési előre konfigurált megoldás forgatókönyv](iot-suite-remote-monitoring-sample-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="2382a-116">The three stream analytics jobs and their syntax are described in depth in the [Remote monitoring preconfigured solution walkthrough](iot-suite-remote-monitoring-sample-walkthrough.md).</span></span> 

<span data-ttu-id="2382a-117">Ezeket a feladatokat, közvetlenül az alter logika szerkesztése, vagy adott logika hozzáadása a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="2382a-117">You can edit these jobs directly to alter the logic, or add logic specific to your scenario.</span></span> <span data-ttu-id="2382a-118">A Stream Analytics-feladatok az alábbiak szerint találja meg:</span><span class="sxs-lookup"><span data-stu-id="2382a-118">You can find the Stream Analytics jobs as follows:</span></span>

1. <span data-ttu-id="2382a-119">Ugrás a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2382a-119">Go to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2382a-120">Nyissa meg az erőforráscsoport neve megegyezik az IoT-megoldásból.</span><span class="sxs-lookup"><span data-stu-id="2382a-120">Navigate to the resource group with the same name as your IoT solution.</span></span> 
3. <span data-ttu-id="2382a-121">Válassza ki a módosítani kívánt Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="2382a-121">Select the Azure Stream Analytics job you'd like to modify.</span></span> 
4. <span data-ttu-id="2382a-122">A feladat leállítása kiválasztásával **leállítása** a parancsok készlete.</span><span class="sxs-lookup"><span data-stu-id="2382a-122">Stop the job by selecting **Stop** in the set of commands.</span></span> 
5. <span data-ttu-id="2382a-123">A bemenet, a lekérdezés és a kimenetek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="2382a-123">Edit the inputs, query, and outputs.</span></span>
   
    <span data-ttu-id="2382a-124">Egy egyszerű módosítása, hogy módosítsa a lekérdezést a **szabályok** használandó feladat egy **"<"** ahelyett, hogy egy **">"**.</span><span class="sxs-lookup"><span data-stu-id="2382a-124">A simple modification is to change the query for the **Rules** job to use a **"<"** instead of a **">"**.</span></span> <span data-ttu-id="2382a-125">A megoldás portálra még mindig **">"** amikor szabály szerkesztése, de figyelje meg, hogyan viselkedését tükrözött miatt a változást az alapul szolgáló feladatot.</span><span class="sxs-lookup"><span data-stu-id="2382a-125">The solution portal still shows **">"** when you edit a rule, but notice how the behavior is flipped due to the change in the underlying job.</span></span>
6. <span data-ttu-id="2382a-126">Indítsa el a feladatot</span><span class="sxs-lookup"><span data-stu-id="2382a-126">Start the job</span></span>

> [!NOTE]
> <span data-ttu-id="2382a-127">A távoli figyelési irányítópult adatokat, függ, így módosítása a feladat sikertelen lesz az irányítópult okozhat.</span><span class="sxs-lookup"><span data-stu-id="2382a-127">The remote monitoring dashboard depends on specific data, so altering the jobs can cause the dashboard to fail.</span></span>

## <a name="add-your-own-rules"></a><span data-ttu-id="2382a-128">A saját szabályok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2382a-128">Add your own rules</span></span>

<span data-ttu-id="2382a-129">Mellett az előre konfigurált Azure Stream Analytics-feladatok módosítása, az Azure-portál használatával vegyen fel új feladatokat, vagy vegyen fel új lekérdezések meglévő feladatokat.</span><span class="sxs-lookup"><span data-stu-id="2382a-129">In addition to changing the preconfigured Azure Stream Analytics jobs, you can use the Azure portal to add new jobs or add new queries to existing jobs.</span></span>

## <a name="customize-devices"></a><span data-ttu-id="2382a-130">Eszközök testreszabása</span><span class="sxs-lookup"><span data-stu-id="2382a-130">Customize devices</span></span>

<span data-ttu-id="2382a-131">Eszközök adott adott esetben a leggyakrabban használt bővítmény tevékenységek egyikét dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="2382a-131">One of the most common extension activities is working with devices specific to your scenario.</span></span> <span data-ttu-id="2382a-132">Többféleképpen eszközökre való munkához.</span><span class="sxs-lookup"><span data-stu-id="2382a-132">There are several methods for working with devices.</span></span> <span data-ttu-id="2382a-133">Ezek a metódusok lehetnek a szimulált eszköz helyzetnek megfelelő módosítása, vagy használja a [IoT eszköz SDK] [ IoT Device SDK] csatlakoztassa a fizikai eszközt a megoldás.</span><span class="sxs-lookup"><span data-stu-id="2382a-133">These methods include altering a simulated device to match your scenario, or using the [IoT Device SDK][IoT Device SDK] to connect your physical device to the solution.</span></span>

<span data-ttu-id="2382a-134">Egy részletes útmutató a Hozzáadás, eszközök, tekintse meg a [Iot Suite csatlakozó eszközök](iot-suite-connecting-devices.md) cikk és a [C SDK minta figyelési távoli](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span><span class="sxs-lookup"><span data-stu-id="2382a-134">For a step-by-step guide to adding devices, see the [Iot Suite Connecting Devices](iot-suite-connecting-devices.md) article and the [remote monitoring C SDK Sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span></span> <span data-ttu-id="2382a-135">Ez a minta tervezték a távoli felügyeleti előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="2382a-135">This sample is designed to work with the remote monitoring preconfigured solution.</span></span>

### <a name="create-your-own-simulated-device"></a><span data-ttu-id="2382a-136">A saját szimulált eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="2382a-136">Create your own simulated device</span></span>

<span data-ttu-id="2382a-137">Tartalmazza a [távoli figyelési megoldás forráskódját](https://github.com/Azure/azure-iot-remote-monitoring), .NET szimulátor van.</span><span class="sxs-lookup"><span data-stu-id="2382a-137">Included in the [remote monitoring solution source code](https://github.com/Azure/azure-iot-remote-monitoring), is a .NET simulator.</span></span> <span data-ttu-id="2382a-138">A szimulátor a megoldás részeként üzembe azt, és módosíthatja úgy, hogy a különböző metaadatokat, telemetriai adatokat küldhet és válaszolhat azokra a más parancsok és módszerek.</span><span class="sxs-lookup"><span data-stu-id="2382a-138">This simulator is the one provisioned as part of the solution and you can alter it to send different metadata, telemetry, and respond to different commands and methods.</span></span>

<span data-ttu-id="2382a-139">A távoli felügyeleti előkonfigurált megoldás előre konfigurált szimulátor bocsát ki a hőmérséklet és a páratartalom telemetriai hűtő eszköz szimulálja.</span><span class="sxs-lookup"><span data-stu-id="2382a-139">The preconfigured simulator in the remote monitoring preconfigured solution simulates a cooler device that emits temperature and humidity telemetry.</span></span> <span data-ttu-id="2382a-140">Módosíthatja a szimulátor a [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektre, ha már ágazik el a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="2382a-140">You can modify the simulator in the [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project when you've forked the GitHub repository.</span></span>

### <a name="available-locations-for-simulated-devices"></a><span data-ttu-id="2382a-141">A szimulált eszköz elérhető helyről</span><span class="sxs-lookup"><span data-stu-id="2382a-141">Available locations for simulated devices</span></span>

<span data-ttu-id="2382a-142">Alapértelmezés szerint helyek Budapest/Redmond, Washington, az Amerikai Egyesült Államok van.</span><span class="sxs-lookup"><span data-stu-id="2382a-142">The default set of locations is in Seattle/Redmond, Washington, United States of America.</span></span> <span data-ttu-id="2382a-143">Módosíthatja a következő helyeken és [SampleDeviceFactory.cs][lnk-sample-device-factory].</span><span class="sxs-lookup"><span data-stu-id="2382a-143">You can change these locations in [SampleDeviceFactory.cs][lnk-sample-device-factory].</span></span>

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a><span data-ttu-id="2382a-144">A kívánt tulajdonság frissítéskezelő hozzáadása a szimulátor</span><span class="sxs-lookup"><span data-stu-id="2382a-144">Add a desired property update handler to the simulator</span></span>

<span data-ttu-id="2382a-145">Egy eszköz számára a kívánt tulajdonság értékét a megoldás-portálon állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="2382a-145">You can set a value for a desired property for a device in the solution portal.</span></span> <span data-ttu-id="2382a-146">Az eszközt, hogy a tulajdonság kezelni felelőssége módosítsa a kérelmet, ha az eszköz olvas be a kívánt tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="2382a-146">It is the responsibility of the device to handle the property change request when the device retrieves the desired property value.</span></span> <span data-ttu-id="2382a-147">Egy tulajdonság értékének módosítása kívánt tulajdonságon keresztül támogatásához, kell hozzáadnia a kezelő számára a szimulátor.</span><span class="sxs-lookup"><span data-stu-id="2382a-147">To add support for a property value change through a desired property, you need to add a handler to the simulator.</span></span>

<span data-ttu-id="2382a-148">A szimulátor tartalmazza a kezelők a **SetPointTemp** és **TelemetryInterval** tulajdonságokhoz, frissítheti úgy, hogy a megoldás portálon értékek szükséges.</span><span class="sxs-lookup"><span data-stu-id="2382a-148">The simulator contains handlers for the **SetPointTemp** and **TelemetryInterval** properties that you can update by setting desired values in the solution portal.</span></span>

<span data-ttu-id="2382a-149">A következő példa bemutatja a kezelőt a **SetPointTemp** tulajdonság szükséges a **CoolerDevice** osztály:</span><span class="sxs-lookup"><span data-stu-id="2382a-149">The following example shows the handler for the **SetPointTemp** desired property in the **CoolerDevice** class:</span></span>

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

<span data-ttu-id="2382a-150">Ez a módszer a telemetriai adatok pont hőmérséklet frissíti, és majd visszajelzést a módosítás az IoT-központ által jelentett tulajdonság beállítását.</span><span class="sxs-lookup"><span data-stu-id="2382a-150">This method updates the telemetry point temperature and then reports the change back to IoT Hub by setting a reported property.</span></span>

<span data-ttu-id="2382a-151">A minta az előző példában a következő a saját tulajdonságokat adhat hozzá a saját kezelőit.</span><span class="sxs-lookup"><span data-stu-id="2382a-151">You can add your own handlers for your own properties by following the pattern in the preceding example.</span></span>

<span data-ttu-id="2382a-152">Is kötni kell a kívánt tulajdonságot a kezelő az alábbi példában látható módon a **CoolerDevice** konstruktor:</span><span class="sxs-lookup"><span data-stu-id="2382a-152">You must also bind the desired property to the handler as shown in the following example from the **CoolerDevice** constructor:</span></span>

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

<span data-ttu-id="2382a-153">Vegye figyelembe, hogy **SetPointTempPropertyName** egy meghatározott "Config.SetPointTemp" konstans.</span><span class="sxs-lookup"><span data-stu-id="2382a-153">Note that **SetPointTempPropertyName** is a constant defined as "Config.SetPointTemp".</span></span>

### <a name="add-support-for-a-new-method-to-the-simulator"></a><span data-ttu-id="2382a-154">A szimulátor hozzáadása egy új módszer támogatása</span><span class="sxs-lookup"><span data-stu-id="2382a-154">Add support for a new method to the simulator</span></span>

<span data-ttu-id="2382a-155">Testre szabhatja a szimulátor támogatásához új [módszer (közvetlen módszer)][lnk-direct-methods].</span><span class="sxs-lookup"><span data-stu-id="2382a-155">You can customize the simulator to add support for a new [method (direct method)][lnk-direct-methods].</span></span> <span data-ttu-id="2382a-156">Szükséges két fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="2382a-156">There are two key steps required:</span></span>

- <span data-ttu-id="2382a-157">A szimulátor értesítenie kell az IoT-központ az előre konfigurált megoldásban a metódus adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2382a-157">The simulator must notify the IoT hub in the preconfigured solution with details of the method.</span></span>
- <span data-ttu-id="2382a-158">A szimulátor tartalmaznia kell a metódus hívása kezelésére indításakor, a kódot a **eszközadatok** a solution explorer vagy egy feladat panelen.</span><span class="sxs-lookup"><span data-stu-id="2382a-158">The simulator must include code to handle the method call when you invoke it from the **Device details** panel in the solution explorer or through a job.</span></span>

<span data-ttu-id="2382a-159">A távoli megfigyelési előre konfigurált megoldás által használt *tulajdonságok jelentett* támogatott módszereket küldeni az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2382a-159">The remote monitoring preconfigured solution uses *reported properties* to send details of supported methods to IoT hub.</span></span> <span data-ttu-id="2382a-160">A megoldás háttérrendszeréhez fenntart egy listát a metódus meghívásához előzményeit együtt minden eszköz által támogatott összes módszert.</span><span class="sxs-lookup"><span data-stu-id="2382a-160">The solution back end maintains a list of all the methods supported by each device along with a history of method invocations.</span></span> <span data-ttu-id="2382a-161">Tekintse meg az eszközök, és a megoldás portálon metódusok.</span><span class="sxs-lookup"><span data-stu-id="2382a-161">You can view this information about devices and invoke methods in the solution portal.</span></span>

<span data-ttu-id="2382a-162">Az IoT hub értesíti, hogy egy eszköz támogatja-e egy metódust, az eszköz részleteit az metódust kell hozzáadnia a **SupportedMethods** csomópont jelentett tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="2382a-162">To notify the IoT hub that a device supports a method, the device must add details of the method to the **SupportedMethods** node in the reported properties:</span></span>

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

<span data-ttu-id="2382a-163">A metódus-aláírás formátuma a következő: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span><span class="sxs-lookup"><span data-stu-id="2382a-163">The method signature has the following format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span></span> <span data-ttu-id="2382a-164">Ahhoz például, hogy adja meg a **InitiateFirmwareUpdate** metódus azt várja, egy karakterlánc-paramétert nevű **FwPackageURI**, használja a következő metódus-aláírás:</span><span class="sxs-lookup"><span data-stu-id="2382a-164">For example, to specify the **InitiateFirmwareUpdate** method expects a string parameter named **FwPackageURI**, use the following method signature:</span></span>

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

<span data-ttu-id="2382a-165">A támogatott paraméter típusainak listáját lásd: a **CommandTypes** osztály az infrastruktúra-projektben.</span><span class="sxs-lookup"><span data-stu-id="2382a-165">For a list of supported parameter types, see the **CommandTypes** class in the Infrastructure project.</span></span>

<span data-ttu-id="2382a-166">Töröl egy metódust, adja meg a metódus-aláírás `null` jelentett tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="2382a-166">To delete a method, set the method signature to `null` in the reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="2382a-167">A megoldás háttérrendszeréhez csak frissíti a támogatott módszerekkel kapcsolatos információk, amikor megkapja a *eszközadatokat* üzenet az eszközön.</span><span class="sxs-lookup"><span data-stu-id="2382a-167">The solution back end only updates information about supported methods when it receives a *device information* message from the device.</span></span>

<span data-ttu-id="2382a-168">A következő példakód a a **SampleDeviceFactory** osztály a közös projekt bemutatja, hogyan vehet fel felhasználókat listájának **SupportedMethods** az eszköz által küldött jelentett tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="2382a-168">The following code sample from the **SampleDeviceFactory** class in the Common project shows how to add a method to the list of **SupportedMethods** in the reported properties sent by the device:</span></span>

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

<span data-ttu-id="2382a-169">A kódrészletet részleteit hozzáadja a **InitiateFirmwareUpdate** módszer, beleértve a megoldás portál és a szükséges metódus paraméterek részleteit megjelenítendő szöveget.</span><span class="sxs-lookup"><span data-stu-id="2382a-169">This code snippet adds details of the **InitiateFirmwareUpdate** method including text to display in the solution portal and details of the required method parameters.</span></span>

<span data-ttu-id="2382a-170">A szimulátor küld jelentett tulajdonságait, beleértve a támogatott módszerek, az IoT hubhoz a szimulátor indításakor listáját.</span><span class="sxs-lookup"><span data-stu-id="2382a-170">The simulator sends reported properties, including the list of supported methods, to IoT Hub when the simulator starts.</span></span>

<span data-ttu-id="2382a-171">Adja hozzá a szimulátor kódot az egyes módszerek, amely támogatja a kezelő.</span><span class="sxs-lookup"><span data-stu-id="2382a-171">Add a handler to the simulator code for each method it supports.</span></span> <span data-ttu-id="2382a-172">Láthatja, hogy a meglévő kezelőket a **CoolerDevice** osztály a Simulator.WebJob projektben.</span><span class="sxs-lookup"><span data-stu-id="2382a-172">You can see the existing handlers in the **CoolerDevice** class in the Simulator.WebJob project.</span></span> <span data-ttu-id="2382a-173">A következő példa bemutatja a kezelőt **InitiateFirmwareUpdate** módszert:</span><span class="sxs-lookup"><span data-stu-id="2382a-173">The following example shows the handler for **InitiateFirmwareUpdate** method:</span></span>

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

<span data-ttu-id="2382a-174">Kezelő metódusnevek kell kezdődnie `On` megadott azon metódus neve követ.</span><span class="sxs-lookup"><span data-stu-id="2382a-174">Method handler names must start with `On` followed by the name of the method.</span></span> <span data-ttu-id="2382a-175">A **methodRequest** paraméter a metódushívás átadni a megoldás háttérből paramétereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2382a-175">The **methodRequest** parameter contains any parameters passed with the method invocation from the solution back end.</span></span> <span data-ttu-id="2382a-176">A visszatérési érték típusúnak kell lennie **feladat&lt;MethodResponse&gt;**.</span><span class="sxs-lookup"><span data-stu-id="2382a-176">The return value must be of type **Task&lt;MethodResponse&gt;**.</span></span> <span data-ttu-id="2382a-177">A **BuildMethodResponse** segédprogram módszer létrehozásához nyújt segítséget a visszatérési érték.</span><span class="sxs-lookup"><span data-stu-id="2382a-177">The **BuildMethodResponse** utility method helps you create the return value.</span></span>

<span data-ttu-id="2382a-178">A metódus kezelő belül sikerült:</span><span class="sxs-lookup"><span data-stu-id="2382a-178">Inside the method handler, you could:</span></span>

- <span data-ttu-id="2382a-179">Aszinkron feladat elindítása.</span><span class="sxs-lookup"><span data-stu-id="2382a-179">Start an asynchronous task.</span></span>
- <span data-ttu-id="2382a-180">A kívánt tulajdonságokat lekérni a *eszköz iker* az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2382a-180">Retrieve desired properties from the *device twin* in IoT Hub.</span></span>
- <span data-ttu-id="2382a-181">Egy egyetlen jelentett tulajdonság használatával frissítse a **SetReportedPropertyAsync** metódust a **CoolerDevice** osztály.</span><span class="sxs-lookup"><span data-stu-id="2382a-181">Update a single reported property using the **SetReportedPropertyAsync** method in the **CoolerDevice** class.</span></span>
- <span data-ttu-id="2382a-182">Hozzon létre több jelentett tulajdonságainak frissítése egy **TwinCollection** példány és hívása a **Transport.UpdateReportedPropertiesAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="2382a-182">Update multiple reported properties by creating a **TwinCollection** instance and calling the **Transport.UpdateReportedPropertiesAsync** method.</span></span>

<span data-ttu-id="2382a-183">Az előző példában a belső vezérlőprogram frissítése a következő lépéseket végzi el:</span><span class="sxs-lookup"><span data-stu-id="2382a-183">The preceding firmware update example performs the following steps:</span></span>

- <span data-ttu-id="2382a-184">Ellenőrzi, hogy az eszköz a belső vezérlőprogram frissítési kérelem fogadására képes-e.</span><span class="sxs-lookup"><span data-stu-id="2382a-184">Checks the device is able to accept the firmware update request.</span></span>
- <span data-ttu-id="2382a-185">Aszinkron módon indít el a belső vezérlőprogram frissítési műveletet, és alaphelyzetbe állítja a telemetriai adatokat, a művelet befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="2382a-185">Asynchronously initiates the firmware update operation and resets the telemetry when the operation is complete.</span></span>
- <span data-ttu-id="2382a-186">Azonnal visszaadja a "FirmwareUpdate elfogadta" üzenetet annak jelzésére, hogy az eszköz a kérést elfogadták.</span><span class="sxs-lookup"><span data-stu-id="2382a-186">Immediately returns the "FirmwareUpdate accepted" message to indicate the request was accepted by the device.</span></span>

### <a name="build-and-use-your-own-physical-device"></a><span data-ttu-id="2382a-187">Hozza létre, és a saját (fizikai) eszköz</span><span class="sxs-lookup"><span data-stu-id="2382a-187">Build and use your own (physical) device</span></span>

<span data-ttu-id="2382a-188">A [Azure IoT SDK-k](https://github.com/Azure/azure-iot-sdks) adja meg a szalagtárak számos eszköztípus (nyelv és operációs rendszerek) csatlakozni az IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2382a-188">The [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) provide libraries for connecting numerous device types (languages and operating systems) into IoT solutions.</span></span>

## <a name="modify-dashboard-limits"></a><span data-ttu-id="2382a-189">Irányítópult korlátok módosítása</span><span class="sxs-lookup"><span data-stu-id="2382a-189">Modify dashboard limits</span></span>

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a><span data-ttu-id="2382a-190">Irányítópult legördülő megjelenő eszközök száma</span><span class="sxs-lookup"><span data-stu-id="2382a-190">Number of devices displayed in dashboard dropdown</span></span>

<span data-ttu-id="2382a-191">Az alapértelmezett érték 200.</span><span class="sxs-lookup"><span data-stu-id="2382a-191">The default is 200.</span></span> <span data-ttu-id="2382a-192">Módosíthatja ezt a számot [DashboardController.cs][lnk-dashboard-controller].</span><span class="sxs-lookup"><span data-stu-id="2382a-192">You can change this number in [DashboardController.cs][lnk-dashboard-controller].</span></span>

### <a name="number-of-pins-to-display-in-bing-map-control"></a><span data-ttu-id="2382a-193">PIN-kódok Bing térképek vezérlőben megjelenítendő száma</span><span class="sxs-lookup"><span data-stu-id="2382a-193">Number of pins to display in Bing Map control</span></span>

<span data-ttu-id="2382a-194">Az alapértelmezett érték 200.</span><span class="sxs-lookup"><span data-stu-id="2382a-194">The default is 200.</span></span> <span data-ttu-id="2382a-195">Módosíthatja ezt a számot [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span><span class="sxs-lookup"><span data-stu-id="2382a-195">You can change this number in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span></span>

### <a name="time-period-of-telemetry-graph"></a><span data-ttu-id="2382a-196">Telemetria gráf időszakban</span><span class="sxs-lookup"><span data-stu-id="2382a-196">Time period of telemetry graph</span></span>

<span data-ttu-id="2382a-197">Az alapértelmezett érték 10 perc.</span><span class="sxs-lookup"><span data-stu-id="2382a-197">The default is 10 minutes.</span></span> <span data-ttu-id="2382a-198">Ez az érték módosítható [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span><span class="sxs-lookup"><span data-stu-id="2382a-198">You can change this value in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span></span>

## <a name="manually-set-up-application-roles"></a><span data-ttu-id="2382a-199">Alkalmazási szerepköröknek manuális beállítása</span><span class="sxs-lookup"><span data-stu-id="2382a-199">Manually set up application roles</span></span>

<span data-ttu-id="2382a-200">Az alábbi eljárás ismerteti, hogyan adhat **Admin** és **ReadOnly** alkalmazási szerepköröknek előre konfigurált megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2382a-200">The following procedure describes how to add **Admin** and **ReadOnly** application roles to a preconfigured solution.</span></span> <span data-ttu-id="2382a-201">Fontos megjegyezni, hogy a azureiotsuite.com helyről már kiépített előkonfigurált megoldásokat tartalmazza a **Admin** és **ReadOnly** szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="2382a-201">Note that preconfigured solutions provisioned from the azureiotsuite.com site already include the **Admin** and **ReadOnly** roles.</span></span>

<span data-ttu-id="2382a-202">A tagjai a **ReadOnly** szerepkör az irányítópult és az eszközök listája látható, de nem jogosultak eszközök hozzáadása, módosítása eszközattribútumokon vagy parancsokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="2382a-202">Members of the **ReadOnly** role can see the dashboard and the device list, but are not allowed to add devices, change device attributes, or send commands.</span></span>  <span data-ttu-id="2382a-203">A tagjai a **Admin** szerepkör minden funkció teljes hozzáféréssel rendelkeznek a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="2382a-203">Members of the **Admin** role have full access to all the functionality in the solution.</span></span>

1. <span data-ttu-id="2382a-204">Lépjen a [a klasszikus Azure portálon][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="2382a-204">Go to the [Azure classic portal][lnk-classic-portal].</span></span>
2. <span data-ttu-id="2382a-205">Válassza ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2382a-205">Select **Active Directory**.</span></span>
3. <span data-ttu-id="2382a-206">Kattintson az AAD-bérlőt, a megoldás létesített használt nevére.</span><span class="sxs-lookup"><span data-stu-id="2382a-206">Click the name of the AAD tenant you used when you provisioned your solution.</span></span>
4. <span data-ttu-id="2382a-207">Kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2382a-207">Click **Applications**.</span></span>
5. <span data-ttu-id="2382a-208">Kattintson a nevére, az alkalmazás, amely megfelel az előkonfigurált megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="2382a-208">Click the name of the application that matches your preconfigured solution name.</span></span> <span data-ttu-id="2382a-209">Ha nem lát a listában az alkalmazást, válassza ki a **a vállalat tulajdonában lévő alkalmazások** a a **megjelenítése** legördülő menüből, majd kattintson a pipa.</span><span class="sxs-lookup"><span data-stu-id="2382a-209">If you don't see your application in the list, select **Applications my company owns** in the **Show** dropdown and click the check mark.</span></span>
6. <span data-ttu-id="2382a-210">Kattintson a lap alján **kezelése Manifest** , majd **jegyzékfájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="2382a-210">At the bottom of the page, click **Manage Manifest** and then **Download Manifest**.</span></span>
7. <span data-ttu-id="2382a-211">Ez az eljárás egy .JSON kiterjesztésű fájlt a helyi számítógépre tölti le.</span><span class="sxs-lookup"><span data-stu-id="2382a-211">This procedure downloads a .json file to your local machine.</span></span> <span data-ttu-id="2382a-212">Nyissa meg ezt a fájlt egy szövegszerkesztőben, az Ön által választott szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="2382a-212">Open this file for editing in a text editor of your choice.</span></span>
8. <span data-ttu-id="2382a-213">A harmadik sorban a .JSON kiterjesztésű fájl láthatja:</span><span class="sxs-lookup"><span data-stu-id="2382a-213">On the third line of the .json file, you can see:</span></span>

   ```json
   "appRoles" : [],
   ```
   <span data-ttu-id="2382a-214">Cserélje le ezt a sort a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="2382a-214">Replace this line with the following code:</span></span>

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. <span data-ttu-id="2382a-215">Mentse a frissített .JSON kiterjesztésű fájlt (felülírhatja a meglévő fájlt).</span><span class="sxs-lookup"><span data-stu-id="2382a-215">Save the updated .json file (you can overwrite the existing file).</span></span>
10. <span data-ttu-id="2382a-216">A klasszikus Azure portálon, a lap alján válassza **kezelése Manifest** majd **feltöltése Manifest** feltölteni a az előző lépésben mentett .JSON kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="2382a-216">In the Azure classic portal, at the bottom of the page, select **Manage Manifest** then **Upload Manifest** to upload the .json file you saved in the previous step.</span></span>
11. <span data-ttu-id="2382a-217">Ezzel hozzáadta a **Admin** és **ReadOnly** szerepkörök, amelyekkel az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2382a-217">You have now added the **Admin** and **ReadOnly** roles to your application.</span></span>
12. <span data-ttu-id="2382a-218">Hozzárendelése ezek szerepkörök valamelyikét egy felhasználónak számít a címtárában, lásd: [a azureiotsuite.com Web][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="2382a-218">To assign one of these roles to a user in your directory, see [Permissions on the azureiotsuite.com site][lnk-permissions].</span></span>

## <a name="feedback"></a><span data-ttu-id="2382a-219">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="2382a-219">Feedback</span></span>

<span data-ttu-id="2382a-220">Rendelkezik a testreszabást szeretné lásd a jelen dokumentum az érintett?</span><span class="sxs-lookup"><span data-stu-id="2382a-220">Do you have a customization you'd like to see covered in this document?</span></span> <span data-ttu-id="2382a-221">Adja hozzá a szolgáltatási javaslataikat [User Voice](https://feedback.azure.com/forums/321918-azure-iot), vagy ez a cikk megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="2382a-221">Add feature suggestions to [User Voice](https://feedback.azure.com/forums/321918-azure-iot), or comment on this article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2382a-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2382a-222">Next steps</span></span>

<span data-ttu-id="2382a-223">Az előkonfigurált megoldásokat testreszabására vonatkozó beállításokkal kapcsolatos további tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="2382a-223">To learn more about the options for customizing the preconfigured solutions, see:</span></span>

* <span data-ttu-id="2382a-224">[Logikai alkalmazás csatlakoztatása az Azure IoT Suite távoli megfigyelési előre konfigurált megoldás][lnk-logicapp]</span><span class="sxs-lookup"><span data-stu-id="2382a-224">[Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapp]</span></span>
* <span data-ttu-id="2382a-225">[Dinamikus telemetriai adatokat a távoli felügyeleti előkonfigurált megoldás][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="2382a-225">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="2382a-226">[Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="2382a-226">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo]</span></span>
* <span data-ttu-id="2382a-227">[Testre szabhatja, hogy a csatlakoztatott gyári megoldás OPC EE-kiszolgálóinak adatait jeleníti meg][lnk-cf-customize]</span><span class="sxs-lookup"><span data-stu-id="2382a-227">[Customize how the connected factory solution displays data from your OPC UA servers][lnk-cf-customize]</span></span>

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md