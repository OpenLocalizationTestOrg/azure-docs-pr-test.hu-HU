---
title: "aaaRemote előre konfigurált figyelés megoldás forgatókönyv |} Microsoft Docs"
description: "Hello Azure IoT előre konfigurált megoldás távoli megfigyelési és az architektúra leírását."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a><span data-ttu-id="bb9af-103">A távoli figyelési előre konfigurált megoldás bemutatója</span><span class="sxs-lookup"><span data-stu-id="bb9af-103">Remote monitoring preconfigured solution walkthrough</span></span>

<span data-ttu-id="bb9af-104">az IoT Suite távoli megfigyelési hello [előre konfigurált megoldás] [ lnk-preconfigured-solutions] egy-végpontok megvalósítása figyeli a távoli helyeken futó több gépek megoldás.</span><span class="sxs-lookup"><span data-stu-id="bb9af-104">hello IoT Suite remote monitoring [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end monitoring solution for multiple machines running in remote locations.</span></span> <span data-ttu-id="bb9af-105">hello megoldás kulcsfontosságú Azure szolgáltatások tooprovide hello üzleti helyzethez általános végrehajtásának egyesíti.</span><span class="sxs-lookup"><span data-stu-id="bb9af-105">hello solution combines key Azure services tooprovide a generic implementation of hello business scenario.</span></span> <span data-ttu-id="bb9af-106">Használhat hello megoldás kiindulási pontként a saját megvalósítási és [testreszabása] [ lnk-customize] azt toomeet saját speciális üzleti igényeinek.</span><span class="sxs-lookup"><span data-stu-id="bb9af-106">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="bb9af-107">Ez a cikk bemutatja, hogyan néhány fontosabb elemei hello hello távoli figyelési megoldást tooenable toounderstand, hogyan működik.</span><span class="sxs-lookup"><span data-stu-id="bb9af-107">This article walks you through some of hello key elements of hello remote monitoring solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="bb9af-108">Ezeknek az ismereteknek a birtokában:</span><span class="sxs-lookup"><span data-stu-id="bb9af-108">This knowledge helps you to:</span></span>

* <span data-ttu-id="bb9af-109">Problémamegoldás hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="bb9af-109">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="bb9af-110">Tervezze meg hogyan toocustomize toohello megoldás toomeet a saját konkrét követelmények.</span><span class="sxs-lookup"><span data-stu-id="bb9af-110">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span> 
* <span data-ttu-id="bb9af-111">Kialakíthatja saját, Azure-szolgáltatásokat használó IoT-megoldását.</span><span class="sxs-lookup"><span data-stu-id="bb9af-111">Design your own IoT solution that uses Azure services.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="bb9af-112">Logikai architektúra</span><span class="sxs-lookup"><span data-stu-id="bb9af-112">Logical architecture</span></span>

<span data-ttu-id="bb9af-113">a következő diagram hello hello logikai összetevőinek előre konfigurált hello megoldás ismerteti:</span><span class="sxs-lookup"><span data-stu-id="bb9af-113">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Logikai architektúra](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a><span data-ttu-id="bb9af-115">Szimulált eszközök</span><span class="sxs-lookup"><span data-stu-id="bb9af-115">Simulated devices</span></span>

<span data-ttu-id="bb9af-116">A megoldás előre konfigurált hello hello szimulált eszköz egy hűtőeszköz (például épület légkondicionálók vagy létesítmény vezeték nélkül kezelési egység) jelöli.</span><span class="sxs-lookup"><span data-stu-id="bb9af-116">In hello preconfigured solution, hello simulated device represents a cooling device (such as a building air conditioner or facility air handling unit).</span></span> <span data-ttu-id="bb9af-117">Előre konfigurált hello megoldás telepítésekor automatikusan is kiépítése futó négy szimulált eszköz egy [Azure webjobs-feladat][lnk-webjobs].</span><span class="sxs-lookup"><span data-stu-id="bb9af-117">When you deploy hello preconfigured solution, you also automatically provision four simulated devices that run in an [Azure WebJob][lnk-webjobs].</span></span> <span data-ttu-id="bb9af-118">Szimulált hello eszközök megkönnyítik a akkor tooexplore hello viselkedését hello megoldás nélkül hello kell toodeploy fizikai eszközöket.</span><span class="sxs-lookup"><span data-stu-id="bb9af-118">hello simulated devices make it easy for you tooexplore hello behavior of hello solution without hello need toodeploy any physical devices.</span></span> <span data-ttu-id="bb9af-119">toodeploy tényleges fizikai eszközön, lásd: hello [csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello] [ lnk-connect-rm] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="bb9af-119">toodeploy a real physical device, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

### <a name="device-to-cloud-messages"></a><span data-ttu-id="bb9af-120">Az eszközről a felhőbe irányuló üzenetek</span><span class="sxs-lookup"><span data-stu-id="bb9af-120">Device-to-cloud messages</span></span>

<span data-ttu-id="bb9af-121">Minden egyes szimulált eszköz küldhet a következő üzenet típusok tooIoT Hub hello:</span><span class="sxs-lookup"><span data-stu-id="bb9af-121">Each simulated device can send hello following message types tooIoT Hub:</span></span>

| <span data-ttu-id="bb9af-122">Üzenet</span><span class="sxs-lookup"><span data-stu-id="bb9af-122">Message</span></span> | <span data-ttu-id="bb9af-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="bb9af-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bb9af-124">Indítás</span><span class="sxs-lookup"><span data-stu-id="bb9af-124">Startup</span></span> |<span data-ttu-id="bb9af-125">Hello eszköz indításakor elküldi egy **eszközinformáció** adatokat saját magáról toohello háttér tartalmazó üzenetet.</span><span class="sxs-lookup"><span data-stu-id="bb9af-125">When hello device starts, it sends a **device-info** message containing information about itself toohello back end.</span></span> <span data-ttu-id="bb9af-126">Ezen adatok tartalmazzák a hello eszközazonosító és hello parancsok listáját, és módszerek hello eszköz támogatja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-126">This data includes hello device id and a list of hello commands and methods hello device supports.</span></span> |
| <span data-ttu-id="bb9af-127">Jelenlét</span><span class="sxs-lookup"><span data-stu-id="bb9af-127">Presence</span></span> |<span data-ttu-id="bb9af-128">Egy eszköz rendszeresen küld egy **jelenléte** tooreport üzenet, hogy hello eszköz is értelemben érzékelő hello jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="bb9af-128">A device periodically sends a **presence** message tooreport whether hello device can sense hello presence of a sensor.</span></span> |
| <span data-ttu-id="bb9af-129">Telemetria</span><span class="sxs-lookup"><span data-stu-id="bb9af-129">Telemetry</span></span> |<span data-ttu-id="bb9af-130">Egy eszköz rendszeresen küld egy **telemetriai** üzenet szimulált értékeinek hello hőmérséklet és a páratartalom hello eszközről összegyűjtött jelentéseket a szimulált érzékelők.</span><span class="sxs-lookup"><span data-stu-id="bb9af-130">A device periodically sends a **telemetry** message that reports simulated values for hello temperature and humidity collected from hello device's simulated sensors.</span></span> |

> [!NOTE]
> <span data-ttu-id="bb9af-131">hello megoldás tárolja egy Cosmos DB adatbázisban, és nem hello eszköz iker hello eszköz által támogatott parancsok hello listáját.</span><span class="sxs-lookup"><span data-stu-id="bb9af-131">hello solution stores hello list of commands supported by hello device in a Cosmos DB database and not in hello device twin.</span></span>

### <a name="properties-and-device-twins"></a><span data-ttu-id="bb9af-132">Tulajdonságok és ikereszközök</span><span class="sxs-lookup"><span data-stu-id="bb9af-132">Properties and device twins</span></span>

<span data-ttu-id="bb9af-133">hello szimulált eszközök elküldik a következő eszköz tulajdonságok toohello hello [kettős] [ lnk-device-twins] hello IoT-központ, a *tulajdonságok jelentett*.</span><span class="sxs-lookup"><span data-stu-id="bb9af-133">hello simulated devices send hello following device properties toohello [twin][lnk-device-twins] in hello IoT hub as *reported properties*.</span></span> <span data-ttu-id="bb9af-134">hello eszköz küld jelentett tulajdonságok indításkor és a válasz tooa **eszköz állapotváltoztatási** parancs vagy a metódus.</span><span class="sxs-lookup"><span data-stu-id="bb9af-134">hello device sends reported properties at startup and in response tooa **Change Device State** command or method.</span></span>

| <span data-ttu-id="bb9af-135">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bb9af-135">Property</span></span> | <span data-ttu-id="bb9af-136">Cél</span><span class="sxs-lookup"><span data-stu-id="bb9af-136">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="bb9af-137">Config.TelemetryInterval</span><span class="sxs-lookup"><span data-stu-id="bb9af-137">Config.TelemetryInterval</span></span> | <span data-ttu-id="bb9af-138">Gyakoriság (másodperc) hello eszköz küld telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="bb9af-138">Frequency (seconds) hello device sends telemetry</span></span> |
| <span data-ttu-id="bb9af-139">Config.TemperatureMeanValue</span><span class="sxs-lookup"><span data-stu-id="bb9af-139">Config.TemperatureMeanValue</span></span> | <span data-ttu-id="bb9af-140">Hello átlagos értéket a szimulált hello hőmérséklet telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="bb9af-140">Specifies hello mean value for hello simulated temperature telemetry</span></span> |
| <span data-ttu-id="bb9af-141">Device.DeviceID</span><span class="sxs-lookup"><span data-stu-id="bb9af-141">Device.DeviceID</span></span> |<span data-ttu-id="bb9af-142">Azonosító van megadva, vagy egy eszköz hello megoldásban létrehozásakor hozzárendelt</span><span class="sxs-lookup"><span data-stu-id="bb9af-142">Id that is either provided or assigned when a device is created in hello solution</span></span> |
| <span data-ttu-id="bb9af-143">Device.DeviceState</span><span class="sxs-lookup"><span data-stu-id="bb9af-143">Device.DeviceState</span></span> | <span data-ttu-id="bb9af-144">Hello eszköz által jelzett állapotát</span><span class="sxs-lookup"><span data-stu-id="bb9af-144">State reported by hello device</span></span> |
| <span data-ttu-id="bb9af-145">Device.CreatedTime</span><span class="sxs-lookup"><span data-stu-id="bb9af-145">Device.CreatedTime</span></span> |<span data-ttu-id="bb9af-146">Hello megoldás idő hello eszköz sikeresen létrehozva</span><span class="sxs-lookup"><span data-stu-id="bb9af-146">Time hello device was created in hello solution</span></span> |
| <span data-ttu-id="bb9af-147">Device.StartupTime</span><span class="sxs-lookup"><span data-stu-id="bb9af-147">Device.StartupTime</span></span> |<span data-ttu-id="bb9af-148">Idő hello eszköz indítása</span><span class="sxs-lookup"><span data-stu-id="bb9af-148">Time hello device was started</span></span> |
| <span data-ttu-id="bb9af-149">Device.LastDesiredPropertyChange</span><span class="sxs-lookup"><span data-stu-id="bb9af-149">Device.LastDesiredPropertyChange</span></span> |<span data-ttu-id="bb9af-150">hello verziószámát hello utolsó kívánt tulajdonság módosítása</span><span class="sxs-lookup"><span data-stu-id="bb9af-150">hello version number of hello last desired property change</span></span> |
| <span data-ttu-id="bb9af-151">Device.Location.Latitude</span><span class="sxs-lookup"><span data-stu-id="bb9af-151">Device.Location.Latitude</span></span> |<span data-ttu-id="bb9af-152">A földrajzi hosszúság hello eszköz helye</span><span class="sxs-lookup"><span data-stu-id="bb9af-152">Latitude location of hello device</span></span> |
| <span data-ttu-id="bb9af-153">Device.Location.Longitude</span><span class="sxs-lookup"><span data-stu-id="bb9af-153">Device.Location.Longitude</span></span> |<span data-ttu-id="bb9af-154">Hello eszköz hosszúság helye</span><span class="sxs-lookup"><span data-stu-id="bb9af-154">Longitude location of hello device</span></span> |
| <span data-ttu-id="bb9af-155">System.Manufacturer</span><span class="sxs-lookup"><span data-stu-id="bb9af-155">System.Manufacturer</span></span> |<span data-ttu-id="bb9af-156">Eszközgyártó</span><span class="sxs-lookup"><span data-stu-id="bb9af-156">Device manufacturer</span></span> |
| <span data-ttu-id="bb9af-157">System.ModelNumber</span><span class="sxs-lookup"><span data-stu-id="bb9af-157">System.ModelNumber</span></span> |<span data-ttu-id="bb9af-158">Hello eszköz modell száma</span><span class="sxs-lookup"><span data-stu-id="bb9af-158">Model number of hello device</span></span> |
| <span data-ttu-id="bb9af-159">System.SerialNumber</span><span class="sxs-lookup"><span data-stu-id="bb9af-159">System.SerialNumber</span></span> |<span data-ttu-id="bb9af-160">Hello eszköz sorozatszáma</span><span class="sxs-lookup"><span data-stu-id="bb9af-160">Serial number of hello device</span></span> |
| <span data-ttu-id="bb9af-161">System.FirmwareVersion</span><span class="sxs-lookup"><span data-stu-id="bb9af-161">System.FirmwareVersion</span></span> |<span data-ttu-id="bb9af-162">Belső vezérlőprogram hello eszköz jelenlegi verziója</span><span class="sxs-lookup"><span data-stu-id="bb9af-162">Current version of firmware on hello device</span></span> |
| <span data-ttu-id="bb9af-163">System.Platform</span><span class="sxs-lookup"><span data-stu-id="bb9af-163">System.Platform</span></span> |<span data-ttu-id="bb9af-164">Hello eszköz platform architektúrája</span><span class="sxs-lookup"><span data-stu-id="bb9af-164">Platform architecture of hello device</span></span> |
| <span data-ttu-id="bb9af-165">System.Processor</span><span class="sxs-lookup"><span data-stu-id="bb9af-165">System.Processor</span></span> |<span data-ttu-id="bb9af-166">Processzor futó hello eszköz</span><span class="sxs-lookup"><span data-stu-id="bb9af-166">Processor running hello device</span></span> |
| <span data-ttu-id="bb9af-167">System.InstalledRAM</span><span class="sxs-lookup"><span data-stu-id="bb9af-167">System.InstalledRAM</span></span> |<span data-ttu-id="bb9af-168">Hello eszközre telepített, a RAM mennyisége</span><span class="sxs-lookup"><span data-stu-id="bb9af-168">Amount of RAM installed on hello device</span></span> |

<span data-ttu-id="bb9af-169">hello szimulátor magok ezeket a tulajdonságokat, a szimulált eszköz minta értékekkel.</span><span class="sxs-lookup"><span data-stu-id="bb9af-169">hello simulator seeds these properties in simulated devices with sample values.</span></span> <span data-ttu-id="bb9af-170">Minden alkalommal, amikor hello szimulátor inicializálja a szimulált eszköz, hello eszköz jelentett tulajdonságként hello előre definiált metaadatok tooIoT központi jelentés.</span><span class="sxs-lookup"><span data-stu-id="bb9af-170">Each time hello simulator initializes a simulated device, hello device reports hello pre-defined metadata tooIoT Hub as reported properties.</span></span> <span data-ttu-id="bb9af-171">Jelentett tulajdonságok hello eszköz csak akkor lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="bb9af-171">Reported properties can only be updated by hello device.</span></span> <span data-ttu-id="bb9af-172">toochange jelentett tulajdonságot, állítsa a kívánt tulajdonság megoldás portálon.</span><span class="sxs-lookup"><span data-stu-id="bb9af-172">toochange a reported property, you set a desired property in solution portal.</span></span> <span data-ttu-id="bb9af-173">Feladata hello hello eszköz:</span><span class="sxs-lookup"><span data-stu-id="bb9af-173">It is hello responsibility of hello device to:</span></span>

1. <span data-ttu-id="bb9af-174">Rendszeres időközönként lekérdezni kívánt tulajdonságokkal hello IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="bb9af-174">Periodically retrieve desired properties from hello IoT hub.</span></span>
2. <span data-ttu-id="bb9af-175">Frissítse a konfigurálás szükséges hello tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="bb9af-175">Update its configuration with hello desired property value.</span></span>
3. <span data-ttu-id="bb9af-176">Hello új érték hátsó toohello hub küldése jelentett tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="bb9af-176">Send hello new value back toohello hub as a reported property.</span></span>

<span data-ttu-id="bb9af-177">Hello megoldás irányítópultról használható *szükséges tulajdonságok* tooset tulajdonságok hello segítségével egy eszközön [eszköz iker][lnk-device-twins].</span><span class="sxs-lookup"><span data-stu-id="bb9af-177">From hello solution dashboard, you can use *desired properties* tooset properties on a device by using hello [device twin][lnk-device-twins].</span></span> <span data-ttu-id="bb9af-178">Általában egy eszköz olvassa be az kívánt tulajdonság értéke a jelentett tulajdonságként módosíthatja a belső állapotot és a jelentés hello hello hub tooupdate.</span><span class="sxs-lookup"><span data-stu-id="bb9af-178">Typically, a device reads a desired property value from hello hub tooupdate its internal state and report hello change back as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="bb9af-179">hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** kívánt tulajdonságokkal tooupdate hello jelentett küldött vissza tulajdonságai tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="bb9af-179">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="bb9af-180">Az összes többi kívánt tulajdonság változáskérések hello szimulált eszköz figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-180">All other desired property change requests are ignored in hello simulated device.</span></span>

### <a name="methods"></a><span data-ttu-id="bb9af-181">Metódusok</span><span class="sxs-lookup"><span data-stu-id="bb9af-181">Methods</span></span>

<span data-ttu-id="bb9af-182">hello szimulált eszközöket képes kezelni a következő módszerek hello ([módszerek közvetlen][lnk-direct-methods]) hello megoldás portálon keresztül hello IoT-központ meghívott:</span><span class="sxs-lookup"><span data-stu-id="bb9af-182">hello simulated devices can handle hello following methods ([direct methods][lnk-direct-methods]) invoked from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="bb9af-183">Módszer</span><span class="sxs-lookup"><span data-stu-id="bb9af-183">Method</span></span> | <span data-ttu-id="bb9af-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="bb9af-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bb9af-185">InitiateFirmwareUpdate</span><span class="sxs-lookup"><span data-stu-id="bb9af-185">InitiateFirmwareUpdate</span></span> |<span data-ttu-id="bb9af-186">Arra utasítja a hello eszköz tooperform vezérlőprogram-frissítés</span><span class="sxs-lookup"><span data-stu-id="bb9af-186">Instructs hello device tooperform a firmware update</span></span> |
| <span data-ttu-id="bb9af-187">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="bb9af-187">Reboot</span></span> |<span data-ttu-id="bb9af-188">Arra utasítja a hello eszköz tooreboot</span><span class="sxs-lookup"><span data-stu-id="bb9af-188">Instructs hello device tooreboot</span></span> |
| <span data-ttu-id="bb9af-189">FactoryReset</span><span class="sxs-lookup"><span data-stu-id="bb9af-189">FactoryReset</span></span> |<span data-ttu-id="bb9af-190">Arra utasítja a gyári hello eszköz tooperform</span><span class="sxs-lookup"><span data-stu-id="bb9af-190">Instructs hello device tooperform a factory reset</span></span> |

<span data-ttu-id="bb9af-191">Egyes metódusok használata jelentett tulajdonságok tooreport folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="bb9af-191">Some methods use reported properties tooreport on progress.</span></span> <span data-ttu-id="bb9af-192">Például hello **InitiateFirmwareUpdate** metódus aszinkron módon hello eszközön futó hello frissítés szimulálja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-192">For example, hello **InitiateFirmwareUpdate** method simulates running hello update asynchronously on hello device.</span></span> <span data-ttu-id="bb9af-193">hello metódus azonnal visszaadja hello eszközön, miközben hello aszinkron feladat vissza toosend állapotának frissítése tovább toohello megoldás Irányítópult segítségével jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="bb9af-193">hello method returns immediately on hello device, while hello asynchronous task continues toosend status updates back toohello solution dashboard using reported properties.</span></span>

### <a name="commands"></a><span data-ttu-id="bb9af-194">Parancsok</span><span class="sxs-lookup"><span data-stu-id="bb9af-194">Commands</span></span>

<span data-ttu-id="bb9af-195">Szimulált hello eszközök kezelni tud a következő parancsokat (felhő eszközre üzenetek) hello megoldás portálon keresztül hello IoT-központ által küldött hello:</span><span class="sxs-lookup"><span data-stu-id="bb9af-195">hello simulated devices can handle hello following commands (cloud-to-device messages) sent from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="bb9af-196">Parancs</span><span class="sxs-lookup"><span data-stu-id="bb9af-196">Command</span></span> | <span data-ttu-id="bb9af-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="bb9af-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bb9af-198">PingDevice</span><span class="sxs-lookup"><span data-stu-id="bb9af-198">PingDevice</span></span> |<span data-ttu-id="bb9af-199">Elküldi a *ping* toohello eszköz toocheck életben</span><span class="sxs-lookup"><span data-stu-id="bb9af-199">Sends a *ping* toohello device toocheck it is alive</span></span> |
| <span data-ttu-id="bb9af-200">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="bb9af-200">StartTelemetry</span></span> |<span data-ttu-id="bb9af-201">Telemetriai adatok küldését indítása hello eszköz</span><span class="sxs-lookup"><span data-stu-id="bb9af-201">Starts hello device sending telemetry</span></span> |
| <span data-ttu-id="bb9af-202">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="bb9af-202">StopTelemetry</span></span> |<span data-ttu-id="bb9af-203">Leállítja a telemetriai adatok küldését a faxeszközön hello</span><span class="sxs-lookup"><span data-stu-id="bb9af-203">Stops hello device from sending telemetry</span></span> |
| <span data-ttu-id="bb9af-204">ChangeSetPointTemp</span><span class="sxs-lookup"><span data-stu-id="bb9af-204">ChangeSetPointTemp</span></span> |<span data-ttu-id="bb9af-205">Módosítások hello pont beállítása értéket körül mely hello véletlenszerű adatokat</span><span class="sxs-lookup"><span data-stu-id="bb9af-205">Changes hello set point value around which hello random data is generated</span></span> |
| <span data-ttu-id="bb9af-206">DiagnosticTelemetry</span><span class="sxs-lookup"><span data-stu-id="bb9af-206">DiagnosticTelemetry</span></span> |<span data-ttu-id="bb9af-207">Eseményindítók hello eszköz szimulátor toosend egy további telemetriai értékét (externalTemp)</span><span class="sxs-lookup"><span data-stu-id="bb9af-207">Triggers hello device simulator toosend an additional telemetry value (externalTemp)</span></span> |
| <span data-ttu-id="bb9af-208">ChangeDeviceState</span><span class="sxs-lookup"><span data-stu-id="bb9af-208">ChangeDeviceState</span></span> |<span data-ttu-id="bb9af-209">Hello eszköz egy kiterjesztett tulajdonság módosítása és hello eszköz tájékoztató üzenet küldi hello eszközről</span><span class="sxs-lookup"><span data-stu-id="bb9af-209">Changes an extended state property for hello device and sends hello device info message from hello device</span></span> |

> [!NOTE]
> <span data-ttu-id="bb9af-210">Ezen parancsok (felhőből egy eszközre irányuló üzenetek) összehasonlításáért lásd a [felhőből egy eszközre irányuló kommunikáció útmutatóját][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="bb9af-210">For a comparison of these commands (cloud-to-device messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>

## <a name="iot-hub"></a><span data-ttu-id="bb9af-211">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="bb9af-211">IoT Hub</span></span>

<span data-ttu-id="bb9af-212">Hello [IoT-központ] [ lnk-iothub] ingests hello eszközökről hello felhőbe küldött adatokat, és lehetővé teszi az elérhető toohello Azure Stream Analytics (ASA) feladatok.</span><span class="sxs-lookup"><span data-stu-id="bb9af-212">hello [IoT hub][lnk-iothub] ingests data sent from hello devices into hello cloud and makes it available toohello Azure Stream Analytics (ASA) jobs.</span></span> <span data-ttu-id="bb9af-213">Minden egyes adatfolyam ASA feladat külön IoT-központ fogyasztói csoportot tooread hello adatfolyam lévő üzenetek az eszközök használja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-213">Each stream ASA job uses a separate IoT Hub consumer group tooread hello stream of messages from your devices.</span></span>

<span data-ttu-id="bb9af-214">az IoT-központ hello megoldásban is hello:</span><span class="sxs-lookup"><span data-stu-id="bb9af-214">hello IoT hub in hello solution also:</span></span>

- <span data-ttu-id="bb9af-215">Egy tároló hello azonosítók és a hitelesítési kulcsokat tooconnect toohello portal engedélyezett összes hello eszközök identitásjegyzékhez kezeli.</span><span class="sxs-lookup"><span data-stu-id="bb9af-215">Maintains an identity registry that stores hello ids and authentication keys of all hello devices permitted tooconnect toohello portal.</span></span> <span data-ttu-id="bb9af-216">Engedélyezi, és tiltsa le az eszközöknek az üdvözlő identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="bb9af-216">You can enable and disable devices through hello identity registry.</span></span>
- <span data-ttu-id="bb9af-217">Küld parancsokat tooyour eszközök hello megoldás portal nevében.</span><span class="sxs-lookup"><span data-stu-id="bb9af-217">Sends commands tooyour devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="bb9af-218">Módszerek az eszközök nevében hello megoldás portal hív meg.</span><span class="sxs-lookup"><span data-stu-id="bb9af-218">Invokes methods on your devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="bb9af-219">Az összes regisztrált eszközhöz biztosítja a megfelelő ikereszközt.</span><span class="sxs-lookup"><span data-stu-id="bb9af-219">Maintains device twins for all registered devices.</span></span> <span data-ttu-id="bb9af-220">Egy eszköz a két eszköz által jelentett hello tulajdonságértékek tárolja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-220">A device twin stores hello property values reported by a device.</span></span> <span data-ttu-id="bb9af-221">Egy eszköz iker hello megoldás-portálon, hello eszköz tooretrieve következő csatlakozáskor beállított kívánt tulajdonságok tárolja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-221">A device twin also stores desired properties, set in hello solution portal, for hello device tooretrieve when it next connects.</span></span>
- <span data-ttu-id="bb9af-222">Ütemezések feladatok több eszközre tooset tulajdonságait, vagy több eszközön metódusok.</span><span class="sxs-lookup"><span data-stu-id="bb9af-222">Schedules jobs tooset properties for multiple devices or invoke methods on multiple devices.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="bb9af-223">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bb9af-223">Azure Stream Analytics</span></span>

<span data-ttu-id="bb9af-224">A távoli felügyeleti megoldás, hello [Azure Stream Analytics] [ lnk-asa] (ASA) kiszállítja feldolgozást vagy tárolási hello IoT hub tooother háttér-összetevők által fogadott üzenetek küldési eszköz.</span><span class="sxs-lookup"><span data-stu-id="bb9af-224">In hello remote monitoring solution, [Azure Stream Analytics][lnk-asa] (ASA) dispatches device messages received by hello IoT hub tooother back-end components for processing or storage.</span></span> <span data-ttu-id="bb9af-225">Különböző ASA feladatok köszönőüzenetei hello tartalma alapján adott funkció végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="bb9af-225">Different ASA jobs perform specific functions based on hello content of hello messages.</span></span>

<span data-ttu-id="bb9af-226">**1. feladat: Eszközinformáció** eszköz információs üzenetet hello bejövő üzenet adatfolyam-szűrők és tooan Eseményközpont végpont küldése.</span><span class="sxs-lookup"><span data-stu-id="bb9af-226">**Job 1: Device Info** filters device information messages from hello incoming message stream and sends them tooan Event Hub endpoint.</span></span> <span data-ttu-id="bb9af-227">Egy eszköz eszköz információs üzenetet küld, indításkor és a válasz tooa **SendDeviceInfo** parancsot.</span><span class="sxs-lookup"><span data-stu-id="bb9af-227">A device sends device information messages at startup and in response tooa **SendDeviceInfo** command.</span></span> <span data-ttu-id="bb9af-228">Ez a feladat használja a következő lekérdezés definíciója tooidentify hello **eszközinformáció** üzenetek:</span><span class="sxs-lookup"><span data-stu-id="bb9af-228">This job uses hello following query definition tooidentify **device-info** messages:</span></span>

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

<span data-ttu-id="bb9af-229">Ez a feladat elküldi a kimeneti tooan Event Hubs további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="bb9af-229">This job sends its output tooan Event Hub for further processing.</span></span>

<span data-ttu-id="bb9af-230">A **2. feladat: szabályok** kiértékeli a bejövő hőmérséklettel és páratartalommal kapcsolatos telemetriaértékeket az eszközönkénti küszöbértékekhez képest.</span><span class="sxs-lookup"><span data-stu-id="bb9af-230">**Job 2: Rules** evaluates incoming temperature and humidity telemetry values against per-device thresholds.</span></span> <span data-ttu-id="bb9af-231">Hello szabályok szerkesztő hello megoldás portálon elérhető küszöbértékek vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="bb9af-231">Threshold values are set in hello rules editor available in hello solution portal.</span></span> <span data-ttu-id="bb9af-232">A Blob-tárhelyek időbélyegző szerint rendezve tárolják az eszköz/érték párokat, amelyeket a rendszer **referenciaadatokként** olvas be a Stream Analyticsbe.</span><span class="sxs-lookup"><span data-stu-id="bb9af-232">Each device/value pair is stored by timestamp in a blob which Stream Analytics reads in as **Reference Data**.</span></span> <span data-ttu-id="bb9af-233">hello feladat hello beállított küszöbértéket hello eszköz szemben nem üres értéket összehasonlítja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-233">hello job compares any non-empty value against hello set threshold for hello device.</span></span> <span data-ttu-id="bb9af-234">Ha az érték meghaladja a hello ">" feltétel, hello feladat kimenete egy **riasztás** eseményt, amely azt jelzi, hogy hello küszöbérték túllépése és hello eszközök, az érték és az időbélyegző értékei biztosít.</span><span class="sxs-lookup"><span data-stu-id="bb9af-234">If it exceeds hello '>' condition, hello job outputs an **alarm** event that indicates that hello threshold is exceeded and provides hello device, value, and timestamp values.</span></span> <span data-ttu-id="bb9af-235">Ez a feladat a következő lekérdezés definíciója tooidentify telemetriai üzeneteket, amelyek elindítják a riasztót olyan esetben hello használja:</span><span class="sxs-lookup"><span data-stu-id="bb9af-235">This job uses hello following query definition tooidentify telemetry messages that should trigger an alarm:</span></span>

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

<span data-ttu-id="bb9af-236">hello feladat elküldi a kimeneti tooan Event Hubs további feldolgozásra, és menti minden riasztás tooblob tárolási részleteit a ahol hello megoldás portal hello riasztási információkat olvashatja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-236">hello job sends its output tooan Event Hub for further processing and saves details of each alert tooblob storage from where hello solution portal can read hello alert information.</span></span>

<span data-ttu-id="bb9af-237">**3. feladat: Telemetriai** hello bejövő eszköz telemetriai adatfolyam két módon működik.</span><span class="sxs-lookup"><span data-stu-id="bb9af-237">**Job 3: Telemetry** operates on hello incoming device telemetry stream in two ways.</span></span> <span data-ttu-id="bb9af-238">hello elküldi az összes telemetriai üzenet blobtárolóból hello eszközök toopersistent a hosszú távú tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="bb9af-238">hello first sends all telemetry messages from hello devices toopersistent blob storage for long-term storage.</span></span> <span data-ttu-id="bb9af-239">hello második kiszámítja átlagos, minimális és maximális nedvességtartalma keresztül egy 5 perces csúszóablak értéket, majd elküldi a tooblob tárolására.</span><span class="sxs-lookup"><span data-stu-id="bb9af-239">hello second computes average, minimum, and maximum humidity values over a five-minute sliding window and sends this data tooblob storage.</span></span> <span data-ttu-id="bb9af-240">hello megoldás portal hello telemetriai adatokat olvas a blob storage toopopulate hello diagramokat.</span><span class="sxs-lookup"><span data-stu-id="bb9af-240">hello solution portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="bb9af-241">Ez a feladat a következő lekérdezés definíciója hello használja:</span><span class="sxs-lookup"><span data-stu-id="bb9af-241">This job uses hello following query definition:</span></span>

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a><span data-ttu-id="bb9af-242">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bb9af-242">Event Hubs</span></span>

<span data-ttu-id="bb9af-243">Hello **eszközinformáció** és **szabályok** ASA feladatok kimeneti az adatok tooEvent hubok tooreliably előre a toohello **Eseményfeldolgozóhoz** hello webjobs-feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="bb9af-243">hello **device info** and **rules** ASA jobs output their data tooEvent Hubs tooreliably forward on toohello **Event Processor** running in hello WebJob.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="bb9af-244">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bb9af-244">Azure storage</span></span>

<span data-ttu-id="bb9af-245">hello megoldás hello eszközökről származó összes hello nyers, összesített és telemetriai adatok használ az Azure blob storage toopersist hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="bb9af-245">hello solution uses Azure blob storage toopersist all hello raw and summarized telemetry data from hello devices in hello solution.</span></span> <span data-ttu-id="bb9af-246">hello portal hello telemetriai adatokat olvas a blob storage toopopulate hello diagramokat.</span><span class="sxs-lookup"><span data-stu-id="bb9af-246">hello portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="bb9af-247">toodisplay riasztások hello megoldás portal hello adatokat olvas blob-tároló, hogy a rekordok telemetriai túllépte értékek hello konfigurálásakor küszöbértékeket.</span><span class="sxs-lookup"><span data-stu-id="bb9af-247">toodisplay alerts, hello solution portal reads hello data from blob storage that records when telemetry values exceeded hello configured threshold values.</span></span> <span data-ttu-id="bb9af-248">hello megoldás a blob storage toorecord hello küszöbértékek hello megoldás portál beállítása is használ.</span><span class="sxs-lookup"><span data-stu-id="bb9af-248">hello solution also uses blob storage toorecord hello threshold values you set in hello solution portal.</span></span>

## <a name="webjobs"></a><span data-ttu-id="bb9af-249">WebJobs</span><span class="sxs-lookup"><span data-stu-id="bb9af-249">WebJobs</span></span>

<span data-ttu-id="bb9af-250">Ezenkívül toohosting hello eszköz szimulátoros oktatás módszereiről, hello WebJobs hello megoldásban is állomás hello **Eseményfeldolgozóhoz** egy Azure webjobs-feladat, amely kezeli a parancs válaszok futtatása.</span><span class="sxs-lookup"><span data-stu-id="bb9af-250">In addition toohosting hello device simulators, hello WebJobs in hello solution also host hello **Event Processor** running in an Azure WebJob that handles command responses.</span></span> <span data-ttu-id="bb9af-251">A parancs válasz üzenetek tooupdate hello parancs eszközelőzmények (hello Cosmos DB adatbázisban tárolt) használja.</span><span class="sxs-lookup"><span data-stu-id="bb9af-251">It uses command response messages tooupdate hello device command history (stored in hello Cosmos DB database).</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="bb9af-252">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bb9af-252">Cosmos DB</span></span>

<span data-ttu-id="bb9af-253">hello megoldás Cosmos DB adatbázis toostore információ a hello eszközök csatlakoztatott toohello megoldást használ.</span><span class="sxs-lookup"><span data-stu-id="bb9af-253">hello solution uses a Cosmos DB database toostore information about hello devices connected toohello solution.</span></span> <span data-ttu-id="bb9af-254">Ezen információk közé tartozik a hello előzmények hello megoldás portálról toodevices küldött parancsokat és hello megoldás portal metódusokra.</span><span class="sxs-lookup"><span data-stu-id="bb9af-254">This information includes hello history of commands sent toodevices from hello solution portal and of methods invoked from hello solution portal.</span></span>

## <a name="solution-portal"></a><span data-ttu-id="bb9af-255">Megoldásportál</span><span class="sxs-lookup"><span data-stu-id="bb9af-255">Solution portal</span></span>

<span data-ttu-id="bb9af-256">hello megoldás portál egy webalkalmazást, előre konfigurált hello megoldás részeként.</span><span class="sxs-lookup"><span data-stu-id="bb9af-256">hello solution portal is a web app deployed as part of hello preconfigured solution.</span></span> <span data-ttu-id="bb9af-257">hello kulcs hello megoldás portálon oldalai hello irányítópult és hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="bb9af-257">hello key pages in hello solution portal are hello dashboard and hello device list.</span></span>

### <a name="dashboard"></a><span data-ttu-id="bb9af-258">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="bb9af-258">Dashboard</span></span>

<span data-ttu-id="bb9af-259">Ezen a lapon hello web app alkalmazásban használja a Power bi javascript vezérlők (lásd: [Power bi-látványelemek tárház](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetriai adatokat hello eszközökről.</span><span class="sxs-lookup"><span data-stu-id="bb9af-259">This page in hello web app uses PowerBI javascript controls (See [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetry data from hello devices.</span></span> <span data-ttu-id="bb9af-260">hello megoldás hello ASA telemetriai feladat toowrite hello telemetriai adatok tooblob tárolást használ.</span><span class="sxs-lookup"><span data-stu-id="bb9af-260">hello solution uses hello ASA telemetry job toowrite hello telemetry data tooblob storage.</span></span>

### <a name="device-list"></a><span data-ttu-id="bb9af-261">Eszközlista</span><span class="sxs-lookup"><span data-stu-id="bb9af-261">Device list</span></span>

<span data-ttu-id="bb9af-262">Ezen a lapon a hello megoldás portál a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="bb9af-262">From this page in hello solution portal you can:</span></span>

* <span data-ttu-id="bb9af-263">Új eszközöket építhet ki.</span><span class="sxs-lookup"><span data-stu-id="bb9af-263">Provision a new device.</span></span> <span data-ttu-id="bb9af-264">Ez a művelet beállítja a hello az eszköz egyedi azonosítója, és hello hitelesítési kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bb9af-264">This action sets hello unique device id and generates hello authentication key.</span></span> <span data-ttu-id="bb9af-265">Hello eszköz tooboth hello IoT Hub identitásjegyzékhez kapcsolatos információkat és hello Megoldásfüggő Cosmos DB adatbázis ír.</span><span class="sxs-lookup"><span data-stu-id="bb9af-265">It writes information about hello device tooboth hello IoT Hub identity registry and hello solution-specific Cosmos DB database.</span></span>
* <span data-ttu-id="bb9af-266">Felügyelheti az eszköztulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="bb9af-266">Manage device properties.</span></span> <span data-ttu-id="bb9af-267">A művelet segítségével megtekintheti a meglévő tulajdonságokat, és új tulajdonságokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="bb9af-267">This action includes viewing existing properties and updating with new properties.</span></span>
* <span data-ttu-id="bb9af-268">Parancsok tooa eszköz küldése.</span><span class="sxs-lookup"><span data-stu-id="bb9af-268">Send commands tooa device.</span></span>
* <span data-ttu-id="bb9af-269">Egy eszköz hello parancs előzményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="bb9af-269">View hello command history for a device.</span></span>
* <span data-ttu-id="bb9af-270">Engedélyezheti és letilthatja a különböző eszközöket.</span><span class="sxs-lookup"><span data-stu-id="bb9af-270">Enable and disable devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb9af-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb9af-271">Next steps</span></span>

<span data-ttu-id="bb9af-272">hello TechNet következő blogbejegyzésekben adjon meg több részletet távoli felügyeleti előkonfigurált megoldás hello:</span><span class="sxs-lookup"><span data-stu-id="bb9af-272">hello following TechNet blog posts provide more detail about hello remote monitoring preconfigured solution:</span></span>

* [<span data-ttu-id="bb9af-273">Az IoT Suite - alatt hello technikai részletek - távoli figyelése</span><span class="sxs-lookup"><span data-stu-id="bb9af-273">IoT Suite - Under hello Hood - Remote Monitoring</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [<span data-ttu-id="bb9af-274">IIoT Suite – Távoli figyelés – Élő és szimulált eszközök hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bb9af-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

<span data-ttu-id="bb9af-275">Továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="bb9af-275">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="bb9af-276">[Csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="bb9af-276">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="bb9af-277">[Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="bb9af-277">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
