---
title: "aaaCustomizing előre konfigurált megoldások |} Microsoft Docs"
description: "Hogyan toocustomize hello Azure IoT Suite előre konfigurált megoldásokat nyújt útmutatást."
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
ms.openlocfilehash: 1a8573f5ac6ed944c44459df495446f15174d513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-a-preconfigured-solution"></a><span data-ttu-id="074a7-103">Előre konfigurált megoldás testreszabása</span><span class="sxs-lookup"><span data-stu-id="074a7-103">Customize a preconfigured solution</span></span>

<span data-ttu-id="074a7-104">hello Azure IoT Suite előre konfigurált hello megoldásai hello szolgáltatások belül hello suite működik együtt toodeliver egy végpont megoldás bemutatása.</span><span class="sxs-lookup"><span data-stu-id="074a7-104">hello preconfigured solutions provided with hello Azure IoT Suite demonstrate hello services within hello suite working together toodeliver an end-to-end solution.</span></span> <span data-ttu-id="074a7-105">A kiindulási pont, a különböző helyek, amelyeknek kiterjesztése és testre szabhatja az adott forgatókönyveket hello megoldás vannak.</span><span class="sxs-lookup"><span data-stu-id="074a7-105">From this starting point, there are various places in which you can extend and customize hello solution for specific scenarios.</span></span> <span data-ttu-id="074a7-106">hello a következő szakaszok ismertetik a közös testreszabási pontok.</span><span class="sxs-lookup"><span data-stu-id="074a7-106">hello following sections describe these common customization points.</span></span>

## <a name="find-hello-source-code"></a><span data-ttu-id="074a7-107">Hello Forráskód keresése</span><span class="sxs-lookup"><span data-stu-id="074a7-107">Find hello source code</span></span>

<span data-ttu-id="074a7-108">előre konfigurált hello megoldások hello forráskódja érhető el a Githubon hello tárházak találhatók a következő:</span><span class="sxs-lookup"><span data-stu-id="074a7-108">hello source code for hello preconfigured solutions is available on GitHub in hello following repositories:</span></span>

* <span data-ttu-id="074a7-109">Távoli megfigyelési: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span><span class="sxs-lookup"><span data-stu-id="074a7-109">Remote Monitoring: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span></span>
* <span data-ttu-id="074a7-110">A prediktív karbantartási: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span><span class="sxs-lookup"><span data-stu-id="074a7-110">Predictive Maintenance: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span></span>
* <span data-ttu-id="074a7-111">Csatlakoztatott gyári: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span><span class="sxs-lookup"><span data-stu-id="074a7-111">Connected factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span></span>

<span data-ttu-id="074a7-112">előre konfigurált hello megoldások hello forráskódja kerül a toodemonstrate hello minták és gyakorlatok használt tooimplement hello végpont funkciók egy IoT-megoldás Azure IoT Suite használata.</span><span class="sxs-lookup"><span data-stu-id="074a7-112">hello source code for hello preconfigured solutions is provided toodemonstrate hello patterns and practices used tooimplement hello end-to-end functionality of an IoT solution using Azure IoT Suite.</span></span> <span data-ttu-id="074a7-113">További információ található toobuild és központi telepítése a GitHub-adattárak hello hello megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="074a7-113">You can find more information about how toobuild and deploy hello solutions in hello GitHub repositories.</span></span>

## <a name="change-hello-preconfigured-rules"></a><span data-ttu-id="074a7-114">Hello előre konfigurált szabályok módosítása</span><span class="sxs-lookup"><span data-stu-id="074a7-114">Change hello preconfigured rules</span></span>

<span data-ttu-id="074a7-115">hello távoli figyelési megoldás tartalmaz három [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) feladatok toohandle eszközadatokat, telemetriai, szabálylogika hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="074a7-115">hello remote monitoring solution includes three [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobs toohandle device information, telemetry, and rules logic in hello solution.</span></span>

<span data-ttu-id="074a7-116">hello három stream analytics-feladatok és a hello részletesen ismerteti a szintaxisát [távoli megfigyelési előre konfigurált megoldás forgatókönyv](iot-suite-remote-monitoring-sample-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="074a7-116">hello three stream analytics jobs and their syntax are described in depth in hello [Remote monitoring preconfigured solution walkthrough](iot-suite-remote-monitoring-sample-walkthrough.md).</span></span> 

<span data-ttu-id="074a7-117">Ezek a feladatok közvetlen tooalter logika hello, vagy vegye fel a logikai adott tooyour forgatókönyv szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="074a7-117">You can edit these jobs directly tooalter hello logic, or add logic specific tooyour scenario.</span></span> <span data-ttu-id="074a7-118">Található hello Stream Analytics-feladatok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="074a7-118">You can find hello Stream Analytics jobs as follows:</span></span>

1. <span data-ttu-id="074a7-119">Nyissa meg túl[Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="074a7-119">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="074a7-120">Keresse meg az erőforráscsoport toohello hello azonos nevet, amint az IoT-megoldásból.</span><span class="sxs-lookup"><span data-stu-id="074a7-120">Navigate toohello resource group with hello same name as your IoT solution.</span></span> 
3. <span data-ttu-id="074a7-121">Válassza ki azt szeretné, hogy toomodify hello Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="074a7-121">Select hello Azure Stream Analytics job you'd like toomodify.</span></span> 
4. <span data-ttu-id="074a7-122">Hello feladat leállításával kiválasztásával **leállítása** hello készletében lévő parancsok.</span><span class="sxs-lookup"><span data-stu-id="074a7-122">Stop hello job by selecting **Stop** in hello set of commands.</span></span> 
5. <span data-ttu-id="074a7-123">Hello bemenet, a lekérdezés és a kimenetek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="074a7-123">Edit hello inputs, query, and outputs.</span></span>
   
    <span data-ttu-id="074a7-124">Egy egyszerű módosítás toochange hello lekérdezést hello **szabályok** feladat toouse egy **"<"** ahelyett, hogy egy **">"**.</span><span class="sxs-lookup"><span data-stu-id="074a7-124">A simple modification is toochange hello query for hello **Rules** job toouse a **"<"** instead of a **">"**.</span></span> <span data-ttu-id="074a7-125">hello megoldás portálra még mindig **">"** amikor szabály szerkesztése, de figyelje meg, hogyan hello viselkedés tükrözött toohello változást az alapul szolgáló feladat hello miatt.</span><span class="sxs-lookup"><span data-stu-id="074a7-125">hello solution portal still shows **">"** when you edit a rule, but notice how hello behavior is flipped due toohello change in hello underlying job.</span></span>
6. <span data-ttu-id="074a7-126">Hello feladat indítása</span><span class="sxs-lookup"><span data-stu-id="074a7-126">Start hello job</span></span>

> [!NOTE]
> <span data-ttu-id="074a7-127">hello távoli figyelési irányítópult adatokat, függ, így hello irányítópult toofail hello feladatok módosítása okozhatja.</span><span class="sxs-lookup"><span data-stu-id="074a7-127">hello remote monitoring dashboard depends on specific data, so altering hello jobs can cause hello dashboard toofail.</span></span>

## <a name="add-your-own-rules"></a><span data-ttu-id="074a7-128">A saját szabályok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="074a7-128">Add your own rules</span></span>

<span data-ttu-id="074a7-129">Ezenkívül toochanging hello előre konfigurált Azure Stream Analytics-feladatok, hello Azure portál tooadd új feladatok használhatja, vagy vegyen fel új lekérdezések tooexisting feladatokat.</span><span class="sxs-lookup"><span data-stu-id="074a7-129">In addition toochanging hello preconfigured Azure Stream Analytics jobs, you can use hello Azure portal tooadd new jobs or add new queries tooexisting jobs.</span></span>

## <a name="customize-devices"></a><span data-ttu-id="074a7-130">Eszközök testreszabása</span><span class="sxs-lookup"><span data-stu-id="074a7-130">Customize devices</span></span>

<span data-ttu-id="074a7-131">Hello leggyakoribb bővítmény tevékenységek egyikét és eszközök meghatározott tooyour forgatókönyv működik.</span><span class="sxs-lookup"><span data-stu-id="074a7-131">One of hello most common extension activities is working with devices specific tooyour scenario.</span></span> <span data-ttu-id="074a7-132">Többféleképpen eszközökre való munkához.</span><span class="sxs-lookup"><span data-stu-id="074a7-132">There are several methods for working with devices.</span></span> <span data-ttu-id="074a7-133">Ezek a metódusok lehetnek a szimulált eszköz toomatch adott esetben módosítása, vagy a hello segítségével [IoT eszköz SDK] [ IoT Device SDK] tooconnect a fizikai eszköz toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="074a7-133">These methods include altering a simulated device toomatch your scenario, or using hello [IoT Device SDK][IoT Device SDK] tooconnect your physical device toohello solution.</span></span>

<span data-ttu-id="074a7-134">Egy részletes útmutató tooadding eszközök esetében lásd: hello [Iot Suite csatlakozó eszközök](iot-suite-connecting-devices.md) cikk és hello [C SDK minta figyelési távoli](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span><span class="sxs-lookup"><span data-stu-id="074a7-134">For a step-by-step guide tooadding devices, see hello [Iot Suite Connecting Devices](iot-suite-connecting-devices.md) article and hello [remote monitoring C SDK Sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span></span> <span data-ttu-id="074a7-135">Ez a minta a távoli felügyeleti előkonfigurált megoldás hello tervezett toowork.</span><span class="sxs-lookup"><span data-stu-id="074a7-135">This sample is designed toowork with hello remote monitoring preconfigured solution.</span></span>

### <a name="create-your-own-simulated-device"></a><span data-ttu-id="074a7-136">A saját szimulált eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="074a7-136">Create your own simulated device</span></span>

<span data-ttu-id="074a7-137">Hello szereplő [távoli figyelési megoldás forráskódját](https://github.com/Azure/azure-iot-remote-monitoring), .NET szimulátor van.</span><span class="sxs-lookup"><span data-stu-id="074a7-137">Included in hello [remote monitoring solution source code](https://github.com/Azure/azure-iot-remote-monitoring), is a .NET simulator.</span></span> <span data-ttu-id="074a7-138">A szimulátor egy részét hello megoldás, és módosíthatja a toosend különböző metaadatokat, telemetriai adatokat, és válaszoljon toodifferent parancsok és módszerek kiépített hello.</span><span class="sxs-lookup"><span data-stu-id="074a7-138">This simulator is hello one provisioned as part of hello solution and you can alter it toosend different metadata, telemetry, and respond toodifferent commands and methods.</span></span>

<span data-ttu-id="074a7-139">hello előre konfigurált szimulátor a távoli felügyeleti előkonfigurált megoldás hello bocsát ki a hőmérséklet és a páratartalom telemetriai hűtő eszköz szimulálja.</span><span class="sxs-lookup"><span data-stu-id="074a7-139">hello preconfigured simulator in hello remote monitoring preconfigured solution simulates a cooler device that emits temperature and humidity telemetry.</span></span> <span data-ttu-id="074a7-140">Módosíthatja a hello hello szimulátor [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektre, ha már ágazik el hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="074a7-140">You can modify hello simulator in hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project when you've forked hello GitHub repository.</span></span>

### <a name="available-locations-for-simulated-devices"></a><span data-ttu-id="074a7-141">A szimulált eszköz elérhető helyről</span><span class="sxs-lookup"><span data-stu-id="074a7-141">Available locations for simulated devices</span></span>

<span data-ttu-id="074a7-142">hello alapértelmezés szerint a helyek Budapest/Redmond, Washington, az Amerikai Egyesült Államok van.</span><span class="sxs-lookup"><span data-stu-id="074a7-142">hello default set of locations is in Seattle/Redmond, Washington, United States of America.</span></span> <span data-ttu-id="074a7-143">Módosíthatja a következő helyeken és [SampleDeviceFactory.cs][lnk-sample-device-factory].</span><span class="sxs-lookup"><span data-stu-id="074a7-143">You can change these locations in [SampleDeviceFactory.cs][lnk-sample-device-factory].</span></span>

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a><span data-ttu-id="074a7-144">Adja hozzá a kívánt tulajdonság frissítési kezelő toohello szimulátor</span><span class="sxs-lookup"><span data-stu-id="074a7-144">Add a desired property update handler toohello simulator</span></span>

<span data-ttu-id="074a7-145">Egy eszköz számára a kívánt tulajdonság értékét hello megoldás portálon állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="074a7-145">You can set a value for a desired property for a device in hello solution portal.</span></span> <span data-ttu-id="074a7-146">Feladata hello hello eszköz toohandle hello tulajdonság változáskérés amikor hello eszköz kér le a szükséges hello tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="074a7-146">It is hello responsibility of hello device toohandle hello property change request when hello device retrieves hello desired property value.</span></span> <span data-ttu-id="074a7-147">a tulajdonság értékének módosítása tooadd támogatása a kívánt tulajdonságon keresztül kell tooadd kezelő toohello szimulátor.</span><span class="sxs-lookup"><span data-stu-id="074a7-147">tooadd support for a property value change through a desired property, you need tooadd a handler toohello simulator.</span></span>

<span data-ttu-id="074a7-148">hello szimulátor tartalmaz hello kezelőkkel **SetPointTemp** és **TelemetryInterval** beállításával frissíthető tulajdonságok kívánt hello megoldás portálon értékeit.</span><span class="sxs-lookup"><span data-stu-id="074a7-148">hello simulator contains handlers for hello **SetPointTemp** and **TelemetryInterval** properties that you can update by setting desired values in hello solution portal.</span></span>

<span data-ttu-id="074a7-149">hello alábbi példa bemutatja a hello hello kezelő **SetPointTemp** hello tulajdonság szükséges **CoolerDevice** osztály:</span><span class="sxs-lookup"><span data-stu-id="074a7-149">hello following example shows hello handler for hello **SetPointTemp** desired property in hello **CoolerDevice** class:</span></span>

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

<span data-ttu-id="074a7-150">Ez a módszer hello telemetriai pont hőmérséklet és jelentések hello módosítása hátsó tooIoT központ által jelentett tulajdonság beállítása frissíti.</span><span class="sxs-lookup"><span data-stu-id="074a7-150">This method updates hello telemetry point temperature and then reports hello change back tooIoT Hub by setting a reported property.</span></span>

<span data-ttu-id="074a7-151">A saját kezelőit a saját tulajdonságokat adhat hozzá a következő hello minta a fenti példa hello által.</span><span class="sxs-lookup"><span data-stu-id="074a7-151">You can add your own handlers for your own properties by following hello pattern in hello preceding example.</span></span>

<span data-ttu-id="074a7-152">Is kötni kell hello kívánt tulajdonság toohello kezelő látható módon a következő példa hello hello **CoolerDevice** konstruktor:</span><span class="sxs-lookup"><span data-stu-id="074a7-152">You must also bind hello desired property toohello handler as shown in hello following example from hello **CoolerDevice** constructor:</span></span>

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

<span data-ttu-id="074a7-153">Vegye figyelembe, hogy **SetPointTempPropertyName** egy meghatározott "Config.SetPointTemp" konstans.</span><span class="sxs-lookup"><span data-stu-id="074a7-153">Note that **SetPointTempPropertyName** is a constant defined as "Config.SetPointTemp".</span></span>

### <a name="add-support-for-a-new-method-toohello-simulator"></a><span data-ttu-id="074a7-154">Adja hozzá egy új módszer toohello szimulátor támogatása</span><span class="sxs-lookup"><span data-stu-id="074a7-154">Add support for a new method toohello simulator</span></span>

<span data-ttu-id="074a7-155">Testre szabhatja a hello szimulátor tooadd támogatása egy új [módszer (közvetlen módszer)][lnk-direct-methods].</span><span class="sxs-lookup"><span data-stu-id="074a7-155">You can customize hello simulator tooadd support for a new [method (direct method)][lnk-direct-methods].</span></span> <span data-ttu-id="074a7-156">Szükséges két fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="074a7-156">There are two key steps required:</span></span>

- <span data-ttu-id="074a7-157">hello szimulátor értesítenie kell az IoT-központ hello előre konfigurált hello megoldásban hello metódus adatokkal.</span><span class="sxs-lookup"><span data-stu-id="074a7-157">hello simulator must notify hello IoT hub in hello preconfigured solution with details of hello method.</span></span>
- <span data-ttu-id="074a7-158">hello szimulátor kell tartalmazniuk a kód toohandle hello metódus hívása a hello meghívása **eszközadatok** hello megoldástallózó vagy egy feladat panelen.</span><span class="sxs-lookup"><span data-stu-id="074a7-158">hello simulator must include code toohandle hello method call when you invoke it from hello **Device details** panel in hello solution explorer or through a job.</span></span>

<span data-ttu-id="074a7-159">hello távoli megfigyelési előre konfigurált megoldás által használt *tulajdonságok jelentett* támogatott módszerek tooIoT hub toosend részleteit.</span><span class="sxs-lookup"><span data-stu-id="074a7-159">hello remote monitoring preconfigured solution uses *reported properties* toosend details of supported methods tooIoT hub.</span></span> <span data-ttu-id="074a7-160">hello megoldás háttérrendszerének fenntart egy listát a metódus meghívásához előzményeit együtt minden eszköz által támogatott összes hello módszert.</span><span class="sxs-lookup"><span data-stu-id="074a7-160">hello solution back end maintains a list of all hello methods supported by each device along with a history of method invocations.</span></span> <span data-ttu-id="074a7-161">Tekintse meg az eszközök és a metódusok hello megoldás portálon.</span><span class="sxs-lookup"><span data-stu-id="074a7-161">You can view this information about devices and invoke methods in hello solution portal.</span></span>

<span data-ttu-id="074a7-162">toonotify hello IoT-központot, hogy egy eszköz támogatja-e egy módszert, hello eszköz hozzá kell adnia a hello metódus toohello részleteit **SupportedMethods** hello csomópontja jelentett tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="074a7-162">toonotify hello IoT hub that a device supports a method, hello device must add details of hello method toohello **SupportedMethods** node in hello reported properties:</span></span>

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

<span data-ttu-id="074a7-163">hello metódus-aláírás rendelkezik hello a következő formátumban: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span><span class="sxs-lookup"><span data-stu-id="074a7-163">hello method signature has hello following format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span></span> <span data-ttu-id="074a7-164">Például toospecify hello **InitiateFirmwareUpdate** metódus azt várja, egy karakterlánc-paramétert nevű **FwPackageURI**, a következő metódus-aláírás hello használata:</span><span class="sxs-lookup"><span data-stu-id="074a7-164">For example, toospecify hello **InitiateFirmwareUpdate** method expects a string parameter named **FwPackageURI**, use hello following method signature:</span></span>

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

<span data-ttu-id="074a7-165">A támogatott paraméter típusainak listáját lásd: hello **CommandTypes** osztály hello infrastruktúra projektben.</span><span class="sxs-lookup"><span data-stu-id="074a7-165">For a list of supported parameter types, see hello **CommandTypes** class in hello Infrastructure project.</span></span>

<span data-ttu-id="074a7-166">egy metódus toodelete hello metódus-aláírás túl beállítása`null` hello a jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="074a7-166">toodelete a method, set hello method signature too`null` in hello reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="074a7-167">hello megoldás háttérrendszerének csak frissíti információ a támogatott módszerek fogadásakor egy *eszközadatokat* üzenet hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="074a7-167">hello solution back end only updates information about supported methods when it receives a *device information* message from hello device.</span></span>

<span data-ttu-id="074a7-168">kódminta hello a következő hello **SampleDeviceFactory** hello közös osztály hogyan projekt mutat be tooadd metódus toohello listáját a **SupportedMethods** hello a jelentett hello által küldött tulajdonságai eszköz:</span><span class="sxs-lookup"><span data-stu-id="074a7-168">hello following code sample from hello **SampleDeviceFactory** class in hello Common project shows how tooadd a method toohello list of **SupportedMethods** in hello reported properties sent by hello device:</span></span>

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

<span data-ttu-id="074a7-169">A kódrészletet hozzáadja hello részleteit **InitiateFirmwareUpdate** többek között a szöveg toodisplay a hello megoldás portal és hello részleteit metódus szükséges, a metódusok paramétereihez.</span><span class="sxs-lookup"><span data-stu-id="074a7-169">This code snippet adds details of hello **InitiateFirmwareUpdate** method including text toodisplay in hello solution portal and details of hello required method parameters.</span></span>

<span data-ttu-id="074a7-170">hello szimulátor küld jelentett tulajdonságai, beleértve a hello támogatott módszerek tooIoT Hub hello szimulátor indításakor.</span><span class="sxs-lookup"><span data-stu-id="074a7-170">hello simulator sends reported properties, including hello list of supported methods, tooIoT Hub when hello simulator starts.</span></span>

<span data-ttu-id="074a7-171">Adja hozzá az egyes módszerek, amely támogatja a kezelő toohello szimulátor kódot.</span><span class="sxs-lookup"><span data-stu-id="074a7-171">Add a handler toohello simulator code for each method it supports.</span></span> <span data-ttu-id="074a7-172">Megtekintheti a meglévő kezelőket hello hello **CoolerDevice** osztály hello Simulator.WebJob projektben.</span><span class="sxs-lookup"><span data-stu-id="074a7-172">You can see hello existing handlers in hello **CoolerDevice** class in hello Simulator.WebJob project.</span></span> <span data-ttu-id="074a7-173">hello alábbi példa bemutatja a hello kezelő **InitiateFirmwareUpdate** módszert:</span><span class="sxs-lookup"><span data-stu-id="074a7-173">hello following example shows hello handler for **InitiateFirmwareUpdate** method:</span></span>

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

<span data-ttu-id="074a7-174">Kezelő metódusnevek kell kezdődnie `On` hello metódus hello neve követ.</span><span class="sxs-lookup"><span data-stu-id="074a7-174">Method handler names must start with `On` followed by hello name of hello method.</span></span> <span data-ttu-id="074a7-175">Hello **methodRequest** paraméter hello megoldás háttérből hello metódushívás átadni paramétereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="074a7-175">hello **methodRequest** parameter contains any parameters passed with hello method invocation from hello solution back end.</span></span> <span data-ttu-id="074a7-176">hello visszatérési érték típusúnak kell lennie **feladat&lt;MethodResponse&gt;**.</span><span class="sxs-lookup"><span data-stu-id="074a7-176">hello return value must be of type **Task&lt;MethodResponse&gt;**.</span></span> <span data-ttu-id="074a7-177">Hello **BuildMethodResponse** segédprogram módszer segítségével hozhat létre a hello visszatérési érték.</span><span class="sxs-lookup"><span data-stu-id="074a7-177">hello **BuildMethodResponse** utility method helps you create hello return value.</span></span>

<span data-ttu-id="074a7-178">Belül hello metódus-kezelőjének sikerült:</span><span class="sxs-lookup"><span data-stu-id="074a7-178">Inside hello method handler, you could:</span></span>

- <span data-ttu-id="074a7-179">Aszinkron feladat elindítása.</span><span class="sxs-lookup"><span data-stu-id="074a7-179">Start an asynchronous task.</span></span>
- <span data-ttu-id="074a7-180">Lekérdezni kívánt tulajdonságokkal hello *eszköz iker* az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="074a7-180">Retrieve desired properties from hello *device twin* in IoT Hub.</span></span>
- <span data-ttu-id="074a7-181">Hello segítségével egyetlen jelentett tulajdonság frissítéséhez **SetReportedPropertyAsync** metódus a hello **CoolerDevice** osztály.</span><span class="sxs-lookup"><span data-stu-id="074a7-181">Update a single reported property using hello **SetReportedPropertyAsync** method in hello **CoolerDevice** class.</span></span>
- <span data-ttu-id="074a7-182">Hozzon létre több jelentett tulajdonságainak frissítése egy **TwinCollection** példány és a hívó hello **Transport.UpdateReportedPropertiesAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="074a7-182">Update multiple reported properties by creating a **TwinCollection** instance and calling hello **Transport.UpdateReportedPropertiesAsync** method.</span></span>

<span data-ttu-id="074a7-183">hello belső vezérlőprogram-frissítés példában hajtja végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="074a7-183">hello preceding firmware update example performs hello following steps:</span></span>

- <span data-ttu-id="074a7-184">Ellenőrzések hello eszköz képes tooaccept hello belső vezérlőprogram frissítési kérelem.</span><span class="sxs-lookup"><span data-stu-id="074a7-184">Checks hello device is able tooaccept hello firmware update request.</span></span>
- <span data-ttu-id="074a7-185">Aszinkron módon hello belső vezérlőprogram frissítési művelet elindul, és alaphelyzetbe állítja a hello telemetriai hello művelet befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="074a7-185">Asynchronously initiates hello firmware update operation and resets hello telemetry when hello operation is complete.</span></span>
- <span data-ttu-id="074a7-186">Azonnal vissza hello "FirmwareUpdate elfogadta" üzenetet tooindicate hello kérés elfogadva hello eszköz</span><span class="sxs-lookup"><span data-stu-id="074a7-186">Immediately returns hello "FirmwareUpdate accepted" message tooindicate hello request was accepted by hello device.</span></span>

### <a name="build-and-use-your-own-physical-device"></a><span data-ttu-id="074a7-187">Hozza létre, és a saját (fizikai) eszköz</span><span class="sxs-lookup"><span data-stu-id="074a7-187">Build and use your own (physical) device</span></span>

<span data-ttu-id="074a7-188">Hello [Azure IoT SDK-k](https://github.com/Azure/azure-iot-sdks) adja meg a szalagtárak számos eszköztípus (nyelv és operációs rendszerek) csatlakozni az IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="074a7-188">hello [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) provide libraries for connecting numerous device types (languages and operating systems) into IoT solutions.</span></span>

## <a name="modify-dashboard-limits"></a><span data-ttu-id="074a7-189">Irányítópult korlátok módosítása</span><span class="sxs-lookup"><span data-stu-id="074a7-189">Modify dashboard limits</span></span>

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a><span data-ttu-id="074a7-190">Irányítópult legördülő megjelenő eszközök száma</span><span class="sxs-lookup"><span data-stu-id="074a7-190">Number of devices displayed in dashboard dropdown</span></span>

<span data-ttu-id="074a7-191">hello alapértelmezett érték 200.</span><span class="sxs-lookup"><span data-stu-id="074a7-191">hello default is 200.</span></span> <span data-ttu-id="074a7-192">Módosíthatja ezt a számot [DashboardController.cs][lnk-dashboard-controller].</span><span class="sxs-lookup"><span data-stu-id="074a7-192">You can change this number in [DashboardController.cs][lnk-dashboard-controller].</span></span>

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a><span data-ttu-id="074a7-193">A Bing térképek vezérlő PIN-kódok toodisplay száma</span><span class="sxs-lookup"><span data-stu-id="074a7-193">Number of pins toodisplay in Bing Map control</span></span>

<span data-ttu-id="074a7-194">hello alapértelmezett érték 200.</span><span class="sxs-lookup"><span data-stu-id="074a7-194">hello default is 200.</span></span> <span data-ttu-id="074a7-195">Módosíthatja ezt a számot [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span><span class="sxs-lookup"><span data-stu-id="074a7-195">You can change this number in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span></span>

### <a name="time-period-of-telemetry-graph"></a><span data-ttu-id="074a7-196">Telemetria gráf időszakban</span><span class="sxs-lookup"><span data-stu-id="074a7-196">Time period of telemetry graph</span></span>

<span data-ttu-id="074a7-197">hello alapértelmezett érték 10 perc.</span><span class="sxs-lookup"><span data-stu-id="074a7-197">hello default is 10 minutes.</span></span> <span data-ttu-id="074a7-198">Ez az érték módosítható [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span><span class="sxs-lookup"><span data-stu-id="074a7-198">You can change this value in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span></span>

## <a name="manually-set-up-application-roles"></a><span data-ttu-id="074a7-199">Alkalmazási szerepköröknek manuális beállítása</span><span class="sxs-lookup"><span data-stu-id="074a7-199">Manually set up application roles</span></span>

<span data-ttu-id="074a7-200">hello alábbi eljárás ismerteti, hogyan tooadd **Admin** és **ReadOnly** alkalmazás szerepkörök tooa előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="074a7-200">hello following procedure describes how tooadd **Admin** and **ReadOnly** application roles tooa preconfigured solution.</span></span> <span data-ttu-id="074a7-201">Fontos megjegyezni, hogy már kiépített hello azureiotsuite.com helyről előkonfigurált megoldásokat tartalmazza-e a hello **Admin** és **ReadOnly** szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="074a7-201">Note that preconfigured solutions provisioned from hello azureiotsuite.com site already include hello **Admin** and **ReadOnly** roles.</span></span>

<span data-ttu-id="074a7-202">Hello tagjai **ReadOnly** szerepkör hello irányítópult és hello eszközök listája látható, de nem megengedett tooadd eszközök, eszköz-attribútumok módosítása vagy küldési parancsok.</span><span class="sxs-lookup"><span data-stu-id="074a7-202">Members of hello **ReadOnly** role can see hello dashboard and hello device list, but are not allowed tooadd devices, change device attributes, or send commands.</span></span>  <span data-ttu-id="074a7-203">Hello tagjai **Admin** szerepkör rendelkezik teljes körű hozzáférési tooall hello funkcióit a hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="074a7-203">Members of hello **Admin** role have full access tooall hello functionality in hello solution.</span></span>

1. <span data-ttu-id="074a7-204">Nyissa meg toohello [a klasszikus Azure portálon][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="074a7-204">Go toohello [Azure classic portal][lnk-classic-portal].</span></span>
2. <span data-ttu-id="074a7-205">Válassza ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="074a7-205">Select **Active Directory**.</span></span>
3. <span data-ttu-id="074a7-206">Kattintson a megoldás létesített használt hello AAD-bérlőt hello nevére.</span><span class="sxs-lookup"><span data-stu-id="074a7-206">Click hello name of hello AAD tenant you used when you provisioned your solution.</span></span>
4. <span data-ttu-id="074a7-207">Kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="074a7-207">Click **Applications**.</span></span>
5. <span data-ttu-id="074a7-208">Kattintson a hello alkalmazás, amely megfelel az előkonfigurált megoldás neve hello nevét.</span><span class="sxs-lookup"><span data-stu-id="074a7-208">Click hello name of hello application that matches your preconfigured solution name.</span></span> <span data-ttu-id="074a7-209">Ha nem látja az alkalmazás hello listán, válassza ki a **a vállalat tulajdonában lévő alkalmazások** a hello **megjelenítése** legördülő menüből, majd kattintson a pipa jelre hello.</span><span class="sxs-lookup"><span data-stu-id="074a7-209">If you don't see your application in hello list, select **Applications my company owns** in hello **Show** dropdown and click hello check mark.</span></span>
6. <span data-ttu-id="074a7-210">Hello a hello lap alján, kattintson **kezelése Manifest** , majd **jegyzékfájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="074a7-210">At hello bottom of hello page, click **Manage Manifest** and then **Download Manifest**.</span></span>
7. <span data-ttu-id="074a7-211">Ez az eljárás egy .JSON kiterjesztésű fájl tooyour helyi számítógép tölti le.</span><span class="sxs-lookup"><span data-stu-id="074a7-211">This procedure downloads a .json file tooyour local machine.</span></span> <span data-ttu-id="074a7-212">Nyissa meg ezt a fájlt egy szövegszerkesztőben, az Ön által választott szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="074a7-212">Open this file for editing in a text editor of your choice.</span></span>
8. <span data-ttu-id="074a7-213">Hello harmadik sor hello .JSON kiterjesztésű fájl láthatja:</span><span class="sxs-lookup"><span data-stu-id="074a7-213">On hello third line of hello .json file, you can see:</span></span>

   ```json
   "appRoles" : [],
   ```
   <span data-ttu-id="074a7-214">Cserélje le ezt a sort a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="074a7-214">Replace this line with hello following code:</span></span>

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access toohello application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access toodevice information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. <span data-ttu-id="074a7-215">Hello frissített .JSON kiterjesztésű fájlt (felülírhatja hello meglévő fájl) mentése.</span><span class="sxs-lookup"><span data-stu-id="074a7-215">Save hello updated .json file (you can overwrite hello existing file).</span></span>
10. <span data-ttu-id="074a7-216">Hello alján hello hello, klasszikus Azure portálon válassza **kezelése Manifest** majd **feltöltése Manifest** hello előző lépésben mentett tooupload hello .JSON kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="074a7-216">In hello Azure classic portal, at hello bottom of hello page, select **Manage Manifest** then **Upload Manifest** tooupload hello .json file you saved in hello previous step.</span></span>
11. <span data-ttu-id="074a7-217">Ezzel hozzáadta a hello **Admin** és **ReadOnly** szerepkörök tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="074a7-217">You have now added hello **Admin** and **ReadOnly** roles tooyour application.</span></span>
12. <span data-ttu-id="074a7-218">a szerepkörök tooa felhasználónak számít a címtárában, egyik tooassign lásd: [hello azureiotsuite.com hely engedélyeinek][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="074a7-218">tooassign one of these roles tooa user in your directory, see [Permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

## <a name="feedback"></a><span data-ttu-id="074a7-219">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="074a7-219">Feedback</span></span>

<span data-ttu-id="074a7-220">Rendelkezik a dokumentum által ismertetett toosee milyen testreszabást?</span><span class="sxs-lookup"><span data-stu-id="074a7-220">Do you have a customization you'd like toosee covered in this document?</span></span> <span data-ttu-id="074a7-221">Adja hozzá a szolgáltatási javaslataikat túl[User Voice](https://feedback.azure.com/forums/321918-azure-iot), vagy ez a cikk megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="074a7-221">Add feature suggestions too[User Voice](https://feedback.azure.com/forums/321918-azure-iot), or comment on this article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="074a7-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="074a7-222">Next steps</span></span>

<span data-ttu-id="074a7-223">További információ az előre konfigurált hello megoldások testreszabásának hello beállításai toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="074a7-223">toolearn more about hello options for customizing hello preconfigured solutions, see:</span></span>

* <span data-ttu-id="074a7-224">[Csatlakozás a Logic App tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás][lnk-logicapp]</span><span class="sxs-lookup"><span data-stu-id="074a7-224">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapp]</span></span>
* <span data-ttu-id="074a7-225">[Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="074a7-225">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="074a7-226">[Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="074a7-226">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>
* <span data-ttu-id="074a7-227">[Hogyan hello csatlakoztatva a OPC EE-kiszolgálóiról gyári megoldás jelenít meg adatokat testreszabása][lnk-cf-customize]</span><span class="sxs-lookup"><span data-stu-id="074a7-227">[Customize how hello connected factory solution displays data from your OPC UA servers][lnk-cf-customize]</span></span>

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