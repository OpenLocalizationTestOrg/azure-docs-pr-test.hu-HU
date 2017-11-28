---
title: "használatába aaaGet előre konfigurált megoldások |} Microsoft Docs"
description: "Hajtsa végre az ezen oktatóanyag toolearn hogyan toodeploy egy Azure IoT Suite előre konfigurált megoldás."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a><span data-ttu-id="ac1ac-103">Ismerkedés az előre konfigurált hello megoldások</span><span class="sxs-lookup"><span data-stu-id="ac1ac-103">Get started with hello preconfigured solutions</span></span>

<span data-ttu-id="ac1ac-104">Az Azure IoT Suite [előre konfigurált megoldások] [ lnk-preconfigured-solutions] több Azure IoT szolgáltatások toodeliver végpontok közötti megoldások, amelyek megvalósítják az IoT elterjedt üzleti forgatókönyvek kombinálni.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="ac1ac-105">Hello *távoli megfigyelési* előkonfigurált megoldás összekapcsolja tooand figyelők az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-105">hello *remote monitoring* preconfigured solution connects tooand monitors your devices.</span></span> <span data-ttu-id="ac1ac-106">Azáltal, hogy válaszoljon automatikusan toothat adatfolyamba való folyamatok hello megoldás tooanalyze hello adatfolyam az eszközök és tooimprove üzleti tevékenységét származó adatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-106">You can use hello solution tooanalyze hello stream of data from your devices and tooimprove business outcomes by making processes respond automatically toothat stream of data.</span></span>

<span data-ttu-id="ac1ac-107">Az oktatóanyag bemutatja, hogyan tooprovision hello távoli megfigyelési előre konfigurált a megoldás.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-107">This tutorial shows you how tooprovision hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="ac1ac-108">Azt is bemutatja, hogyan hello alapvető szolgáltatások előre konfigurált hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="ac1ac-109">Érheti el ezeket a funkciókat számos hello megoldás *irányítópult* , előre konfigurált hello megoldás részeként telepíti:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![Az előre konfigurált távoli figyelési megoldás irányítópultja][img-dashboard]

<span data-ttu-id="ac1ac-111">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ac1ac-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a><span data-ttu-id="ac1ac-114">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-114">Scenario overview</span></span>

<span data-ttu-id="ac1ac-115">Távoli felügyeleti előkonfigurált megoldás hello központi telepítésekor az erőforrásokat, amelyek lehetővé teszik a toostep keresztül távoli figyelési forgatókönyve előre feltöltve.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-115">When you deploy hello remote monitoring preconfigured solution, it is prepopulated with resources that enable you toostep through a common remote monitoring scenario.</span></span> <span data-ttu-id="ac1ac-116">Ebben a forgatókönyvben számos eszközök csatlakoztatott toohello megoldás jelentik a váratlan hőmérséklet-értékek.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-116">In this scenario, several devices connected toohello solution are reporting unexpected temperature values.</span></span> <span data-ttu-id="ac1ac-117">hello következő szakaszok bemutatják, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-117">hello following sections show you how to:</span></span>

* <span data-ttu-id="ac1ac-118">Váratlan hőmérséklet értékek küldése hello eszközök azonosítása.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-118">Identify hello devices sending unexpected temperature values.</span></span>
* <span data-ttu-id="ac1ac-119">Ezek az eszközök konfigurálásához toosend részletesebb telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-119">Configure these devices toosend more detailed telemetry.</span></span>
* <span data-ttu-id="ac1ac-120">Az eszközön a hello belső vezérlőprogram frissítése a hello probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-120">Fix hello problem by updating hello firmware on these devices.</span></span>
* <span data-ttu-id="ac1ac-121">Győződjön meg arról, hogy a művelet megoldotta az hello probléma.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-121">Verify that your action has resolved hello issue.</span></span>

<span data-ttu-id="ac1ac-122">Ebben a forgatókönyvben alapfunkciója, hogy távolról is végrehajtható, ezek a műveletek hello megoldás irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-122">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="ac1ac-123">Nem kell fizikai hozzáférés toohello eszközök.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-123">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="ac1ac-124">Nézet hello megoldás irányítópultja</span><span class="sxs-lookup"><span data-stu-id="ac1ac-124">View hello solution dashboard</span></span>

<span data-ttu-id="ac1ac-125">hello megoldás irányítópultja toomanage telepített hello megoldás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-125">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="ac1ac-126">Megtekintheti például a telemetriát, eszközöket adhat hozzá és szabályokat konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-126">For example, you can view telemetry, add devices, and configure rules.</span></span>

1. <span data-ttu-id="ac1ac-127">Hello kiépítés befejeztével, miután az előkonfigurált megoldás hello csempe jelzi **készen**, válassza a **indítsa el** tooopen a távoli felügyeleti megoldás portál új lapon.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-127">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Indítsa el az előre konfigurált hello megoldás][img-launch-solution]

1. <span data-ttu-id="ac1ac-129">Alapértelmezés szerint hello megoldás portal mutatja hello *irányítópult*.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-129">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="ac1ac-130">Navigálhat tooother területek hello megoldás portál hello bal oldalán található hello hello menü segítségével.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-130">You can navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Az előre konfigurált távoli figyelési megoldás irányítópultja][img-menu]

<span data-ttu-id="ac1ac-132">hello irányítópult a következő információ hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-132">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="ac1ac-133">Minden eszköz hello helye megjelenítő térkép toohello megoldás csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-133">A map that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="ac1ac-134">Hello megoldás első futtatásakor nincsenek 25 szimulált eszközök.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-134">When you first run hello solution, there are 25 simulated devices.</span></span> <span data-ttu-id="ac1ac-135">hello szimulált eszköz megvalósítása Azure webjobs-feladatok, és hello megoldás a hello térképen hello a Bing térképek API tooplot információkat használja.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-135">hello simulated devices are implemented as Azure WebJobs, and hello solution uses hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="ac1ac-136">Lásd: hello [gyakran ismételt kérdések] [ lnk-faq] toolearn hogyan toomake hello térkép dinamikus.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-136">See hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="ac1ac-137">A **Telemetria előzményei** panelt, amely a páratartalommal és hőmérséklettel kapcsolatos telemetriát jelenít meg a kiválasztott eszközről közel valós időben, és összesített adatokat tartalmaz, például a maximális, minimális és átlagos páratartalmat.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-137">A **Telemetry History** panel that plots humidity and temperature telemetry from a selected device in near real time and displays aggregate data such as maximum, minimum, and average humidity.</span></span>
* <span data-ttu-id="ac1ac-138">A **Riasztások előzményei** panelt, amely közelmúltbeli riasztási eseményeket jelenít meg, amikor egy telemetriaérték túllépett egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-138">An **Alarm History** panel that shows recent alarm events when a telemetry value has exceeded a threshold.</span></span> <span data-ttu-id="ac1ac-139">Saját riasztások hozzáadása toohello példákban előre konfigurált hello megoldás által létrehozott adhat meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-139">You can define your own alarms in addition toohello examples created by hello preconfigured solution.</span></span>
* <span data-ttu-id="ac1ac-140">A **Feladatok** panelt, amely az ütemezett feladatok információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-140">A **Jobs** panel that displays information about scheduled jobs.</span></span> <span data-ttu-id="ac1ac-141">A **Felügyeleti feladatok** lapon ütemezheti a saját feladatait.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-141">You can schedule your own jobs on **Management jobs** page.</span></span>

## <a name="view-alarms"></a><span data-ttu-id="ac1ac-142">Riasztások megtekintése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-142">View alarms</span></span>

<span data-ttu-id="ac1ac-143">hello riasztási Előzmények panelen láthatók öt eszköz nagyobbnak, mint a várt telemetriai értékek jelentik.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-143">hello alarm history panel shows you that five devices are reporting higher than expected telemetry values.</span></span>

![TEENDŐ riasztás előzmények hello megoldás irányítópultja][img-alarms]

> [!NOTE]
> <span data-ttu-id="ac1ac-145">Ezek a riasztások akkor jönnek létre, előre konfigurált hello megoldás szereplő szabály által.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-145">These alarms are generated by a rule that is included in hello preconfigured solution.</span></span> <span data-ttu-id="ac1ac-146">Ez a szabály riasztást küld, ha egy eszköz által küldött hello hőmérséklet értéke meghaladja a 60.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-146">This rule generates an alert when hello temperature value sent by a device exceeds 60.</span></span> <span data-ttu-id="ac1ac-147">Kiválasztásával megadhatja a saját szabályok és a műveletek [szabályok](#add-a-rule) és [műveletek](#add-an-action) hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-147">You can define your own rules and actions by choosing [Rules](#add-a-rule) and [Actions](#add-an-action) in hello left-hand menu.</span></span>

## <a name="view-devices"></a><span data-ttu-id="ac1ac-148">Eszközök megtekintése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-148">View devices</span></span>

<span data-ttu-id="ac1ac-149">Hello *eszközök* jeleníti meg az összes regisztrált hello eszközök hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-149">hello *devices* list shows all hello registered devices in hello solution.</span></span> <span data-ttu-id="ac1ac-150">Hello eszköz listából, megtekintheti és szerkesztheti az eszköz metaadatait adja hozzá vagy távolítson el eszközöket, és metódusok eszközökön.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-150">From hello device list you can view and edit device metadata, add or remove devices, and invoke methods on devices.</span></span> <span data-ttu-id="ac1ac-151">Szűrés és a rendezés hello eszközlistában eszközök hello listája.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-151">You can filter and sort hello list of devices in hello device list.</span></span> <span data-ttu-id="ac1ac-152">Testre szabhatja hello oszlopok hello eszközök listája látható.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-152">You can also customize hello columns shown in hello device list.</span></span>

1. <span data-ttu-id="ac1ac-153">Válasszon **eszközök** tooshow hello eszközök listája ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-153">Choose **Devices** tooshow hello device list for this solution.</span></span>

   ![Hello hello megoldás portálon eszköz lista megtekintése][img-devicelist]

1. <span data-ttu-id="ac1ac-155">hello eszközlista kezdetben jeleníti 25 szimulált hozta létre hello létesítésének folyamatát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-155">hello device list initially shows 25 simulated devices created by hello provisioning process.</span></span> <span data-ttu-id="ac1ac-156">Hozzáadhat további szimulált és a fizikai eszközök toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-156">You can add additional simulated and physical devices toohello solution.</span></span>

1. <span data-ttu-id="ac1ac-157">tooview hello részletei az eszköz hello eszköz listában válassza ki egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-157">tooview hello details of a device, choose a device in hello device list.</span></span>

   ![Hello hello megoldás portálon eszköz részleteinek megtekintése][img-devicedetails]

<span data-ttu-id="ac1ac-159">Hello **eszközadatok** panel hat szakaszokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-159">hello **Device Details** panel contains six sections:</span></span>

* <span data-ttu-id="ac1ac-160">Hivatkozások gyűjteményét is tartalmazza, amelyek engedélyezik, toocustomize hello eszköz ikon, hello eszköz letiltása, szabály hozzáadása, a kíván hívni egy metódust vagy a parancsot.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-160">A collection of links that enable you toocustomize hello device icon, disable hello device, add a rule, invoke a method, or send a command.</span></span> <span data-ttu-id="ac1ac-161">Ezen parancsok (egy eszközről a felhőbe irányuló üzenetek) összehasonlításáért lásd a [felhőből egy eszközre irányuló kommunikáció útmutatóját][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-161">For a comparison of commands (device-to-cloud messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>
* <span data-ttu-id="ac1ac-162">Hello **eszköz iker - címkék** szakasz tooedit előfizetéscímkék értékeit hello eszköz segítségével.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-162">hello **Device Twin - Tags** section enables you tooedit tag values for hello device.</span></span> <span data-ttu-id="ac1ac-163">Címke értékek megjelenítése hello eszközök listáját, és címke értékek toofilter hello eszközre a listában.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-163">You can display tag values in hello device list and use tag values toofilter hello device list.</span></span>
* <span data-ttu-id="ac1ac-164">Hello **eszköz iker - tulajdonságok szükséges** szakasz tooset tulajdonság értékek küldött toobe toohello eszköz segítségével.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-164">hello **Device Twin - Desired Properties** section enables you tooset property values toobe sent toohello device.</span></span>
* <span data-ttu-id="ac1ac-165">Hello **eszköz iker - tulajdonságok jelentett** szakasz hello eszközről küldött tulajdonságértékek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-165">hello **Device Twin - Reported Properties** section shows property values sent from hello device.</span></span>
* <span data-ttu-id="ac1ac-166">Hello **eszköztulajdonságok** szakasz hello identitásjegyzékhez hello eszköz például az azonosító és a hitelesítő kulcs információval is.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-166">hello **Device Properties** section shows information from hello identity registry such as hello device id and authentication keys.</span></span>
* <span data-ttu-id="ac1ac-167">Hello **legutóbbi feladatok** szakasz információkat az eszköz nemrég célzott feladatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-167">hello **Recent Jobs** section shows information about any jobs that have recently targeted this device.</span></span>

## <a name="filter-hello-device-list"></a><span data-ttu-id="ac1ac-168">Szűrő hello eszközök listája</span><span class="sxs-lookup"><span data-stu-id="ac1ac-168">Filter hello device list</span></span>

<span data-ttu-id="ac1ac-169">A szűrő toodisplay csak a nem várt hőmérséklet értékek küldenek eszközöket használhatja.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-169">You can use a filter toodisplay only those devices that are sending unexpected temperature values.</span></span> <span data-ttu-id="ac1ac-170">távoli felügyeleti előkonfigurált megoldás hello tartalmaz hello **nem megfelelő eszközök** átlagos hőmérséklet érték nagyobb, mint 60 tooshow eszközök szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-170">hello remote monitoring preconfigured solution includes hello **Unhealthy devices** filter tooshow devices with a mean temperature value greater than 60.</span></span> <span data-ttu-id="ac1ac-171">Emellett [saját szűrőket is létrehozhat](#add-a-filter).</span><span class="sxs-lookup"><span data-stu-id="ac1ac-171">You can also [create your own filters](#add-a-filter).</span></span>

1. <span data-ttu-id="ac1ac-172">Válasszon **mentett szűrő megnyitása** toodisplay rendelkezésre álló szűrők listája.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-172">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="ac1ac-173">Válassza a **nem megfelelő eszközök** tooapply hello szűrő:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-173">Then choose **Unhealthy devices** tooapply hello filter:</span></span>

    ![Szűrők hello listájának megjelenítése][img-unhealthy-filter]

1. <span data-ttu-id="ac1ac-175">hello eszközlista most jeleníti csak átlagos hőmérséklet érték nagyobb, mint 60.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-175">hello device list now shows only devices with a mean temperature value greater than 60.</span></span>

    ![Nézet hello szűrt eszköz listáját jeleníti meg a nem megfelelő eszközt][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a><span data-ttu-id="ac1ac-177">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-177">Update desired properties</span></span>

<span data-ttu-id="ac1ac-178">Sikerült tehát azonosítani néhány eszközt, amelyek javításra szorulhatnak.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-178">You have now identified a set of devices that may need remediation.</span></span> <span data-ttu-id="ac1ac-179">Azonban úgy dönt, hogy hello Adatok gyakoriságot 15 másodperc nincs-e elegendő hello probléma egyértelmű diagnosztikai.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-179">However, you decide that hello data frequency of 15 seconds is not sufficient for a clear diagnosis of hello issue.</span></span> <span data-ttu-id="ac1ac-180">Hello telemetriai gyakoriság toofive másodperc tooprovide módosításával kapcsolatos további adatok pontok toobetter hello probléma diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-180">Changing hello telemetry frequency toofive seconds tooprovide you with more data points toobetter diagnose hello issue.</span></span> <span data-ttu-id="ac1ac-181">A konfigurációs módosítás tooyour távoli eszközök hello megoldás portálról tolható ki.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-181">You can push this configuration change tooyour remote devices from hello solution portal.</span></span> <span data-ttu-id="ac1ac-182">Egyszer, hello hatás értékelje ki, és majd bérlőként hello eredmények hello módosítása, hogy.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-182">You can make hello change once, evaluate hello impact, and then act on hello results.</span></span>

<span data-ttu-id="ac1ac-183">Kövesse ezeket a lépéseket toorun egy feladatot, amely módosítja a hello **TelemetryInterval** hatással hello eszközök tulajdonság szükséges.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-183">Follow these steps toorun a job that changes hello **TelemetryInterval** desired property for hello affected devices.</span></span> <span data-ttu-id="ac1ac-184">Ha hello eszköz megkapja-e új hello **TelemetryInterval** tulajdonság értéke, módosítsa a konfigurációs toosend telemetriai minden ötödik helyett 15 másodpercenként másodperc:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-184">When hello devices receive hello new **TelemetryInterval** property value, they change their configuration toosend telemetry every five seconds instead of every 15 seconds:</span></span>

1. <span data-ttu-id="ac1ac-185">Válassza a nem megfelelő eszközök listája hello megjelennek a hello eszközök listáját, amíg **Feladatütemező**, majd **szerkesztése eszköz iker**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-185">While you are showing hello list of unhealthy devices in hello device list, choose **Job Scheduler**, then **Edit Device Twin**.</span></span>

1. <span data-ttu-id="ac1ac-186">Hello feladat hívás **módosításának telemetriai időköze**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-186">Call hello job **Change telemetry interval**.</span></span>

1. <span data-ttu-id="ac1ac-187">Hello hello értékének módosítása **kívánt tulajdonság** neve **kívánt. Config.TelemetryInterval** toofive másodperc.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-187">Change hello value of hello **Desired Property** name **desired.Config.TelemetryInterval** toofive seconds.</span></span>

1. <span data-ttu-id="ac1ac-188">Válassza az **Ütemezés** elemet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-188">Choose **Schedule**.</span></span>

    ![Hello TelemetryInterval tulajdonság toofive másodperc módosítása][img-change-interval]

1. <span data-ttu-id="ac1ac-190">Kísérheti hello hello feladat a hello **felügyeleti feladatok** hello lapjára.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-190">You can monitor hello progress of hello job on hello **Management Jobs** page in hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-191">Toochange kívánt tulajdonság értéke egy különálló eszköz, használja hello **kívánt tulajdonságokkal** hello szakasz **eszközadatok** panel egy feladat futtatása helyett.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-191">If you want toochange a desired property value for an individual device, use hello **Desired Properties** section in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="ac1ac-192">Ez a feladat beállítja hello hello értékének **TelemetryInterval** hello eszköz iker tulajdonság szükséges, az összes kijelölt hello szűrő eszközök hello.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-192">This job sets hello value of hello **TelemetryInterval** desired property in hello device twin for all hello devices selected by hello filter.</span></span> <span data-ttu-id="ac1ac-193">hello eszközök lekérje ezt az értéket a hello eszköz iker, és azok viselkedését frissítése.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-193">hello devices retrieve this value from hello device twin and update their behavior.</span></span> <span data-ttu-id="ac1ac-194">Amikor egy eszköz kéri le és dolgozza fel a kívánt tulajdonságot egy eszközt a két, hello megfelelő érték tulajdonság állítja be.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-194">When a device retrieves and processes a desired property from a device twin, it sets hello corresponding reported value property.</span></span>

## <a name="invoke-methods"></a><span data-ttu-id="ac1ac-195">Metódusok meghívása</span><span class="sxs-lookup"><span data-stu-id="ac1ac-195">Invoke methods</span></span>

<span data-ttu-id="ac1ac-196">Amíg hello feladat fut, bizonyára észrevette, a nem megfelelő eszközök hello listájában, hogy ezek az eszközök rendelkeznek-e a régi (kevesebb mint 1.6-os verzióra) belső vezérlőprogram verziója.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-196">While hello job runs, you notice in hello list of unhealthy devices that all these devices have old (less than version 1.6) firmware versions.</span></span>

![Nézet hello jelentett belsővezérlőprogram-verziónként hello nem megfelelő eszközök][img-old-firmware]

<span data-ttu-id="ac1ac-198">Lehet, hogy a belső vezérlőprogram verziójának hello okának váratlan hello hőmérséklet értékei, mivel tudható, hogy a más kifogástalan állapotú eszközök nemrég frissített tooversion 2.0 volt.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-198">This firmware version may be hello root cause of hello unexpected temperature values because you know that other healthy devices were recently updated tooversion 2.0.</span></span> <span data-ttu-id="ac1ac-199">Használhatja a hello beépített **régi belső vezérlőprogram eszközök** szűrése tooidentify azokat az eszközöket, a régi belső vezérlőprogramjának következő verziójával.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-199">You can use hello built-in **Old firmware devices** filter tooidentify any devices with old firmware versions.</span></span> <span data-ttu-id="ac1ac-200">Hello portálról távolról frissítheti az összes hello eszközök továbbra is fut a régi belsővezérlőprogram-verziók:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-200">From hello portal, you can then remotely update all hello devices still running old firmware versions:</span></span>

1. <span data-ttu-id="ac1ac-201">Válasszon **mentett szűrő megnyitása** toodisplay rendelkezésre álló szűrők listája.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-201">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="ac1ac-202">Válassza a **régi belső vezérlőprogram eszközök** tooapply hello szűrő:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-202">Then choose **Old firmware devices** tooapply hello filter:</span></span>

    ![Szűrők hello listájának megjelenítése][img-old-filter]

1. <span data-ttu-id="ac1ac-204">hello eszközök listája most csak régi belső vezérlőprogramjának következő verziójával rendelkező eszközöket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-204">hello device list now shows only devices with old firmware versions.</span></span> <span data-ttu-id="ac1ac-205">Ez a lista tartalmazza a hello öt eszköz hello által azonosított **nem megfelelő eszközök** szűrő és három további eszközöket:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-205">This list includes hello five devices identified by hello **Unhealthy devices** filter and three additional devices:</span></span>

    ![Nézet hello szűrt eszköz listáját jeleníti meg régi eszközt][img-filtered-old-list]

1. <span data-ttu-id="ac1ac-207">Válassza a **Feladatütemező**, majd a **Metódus meghívása** elemet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-207">Choose **Job Scheduler**, then **Invoke Method**.</span></span>

1. <span data-ttu-id="ac1ac-208">Állítsa be **feladatnév** túl**belső vezérlőprogram frissítési tooversion 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-208">Set **Job Name** too**Firmware update tooversion 2.0**.</span></span>

1. <span data-ttu-id="ac1ac-209">Válasszon **InitiateFirmwareUpdate** , hello **metódus**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-209">Choose **InitiateFirmwareUpdate** as hello **Method**.</span></span>

1. <span data-ttu-id="ac1ac-210">Set hello **FwPackageUri** paraméter túl**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-210">Set hello **FwPackageUri** parameter too**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span></span>

1. <span data-ttu-id="ac1ac-211">Válassza az **Ütemezés** elemet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-211">Choose **Schedule**.</span></span> <span data-ttu-id="ac1ac-212">hello alapértelmezett most hello feladat toorun szolgál.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-212">hello default is for hello job toorun now.</span></span>

    ![Feladat tooupdate hello az kijelölt hello eszközök belső vezérlőprogramjai létrehozása][img-method-update]

> [!NOTE]
> <span data-ttu-id="ac1ac-214">Ha azt szeretné, hogy egy adott eszközre vonatkozó metódus tooinvoke, **módszerek** a hello **eszközadatok** panel egy feladat futtatása helyett.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-214">If you want tooinvoke a method on an individual device, choose **Methods** in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="ac1ac-215">Ez a feladat hív meg hello **InitiateFirmwareUpdate** közvetlen módszer hello szűrő kijelölt összes hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-215">This job invokes hello **InitiateFirmwareUpdate** direct method on all hello devices selected by hello filter.</span></span> <span data-ttu-id="ac1ac-216">Eszközök azonnal tooIoT Hub válaszol, és majd aszinkron módon az hello belső vezérlőprogram frissítési folyamat elindítása.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-216">Devices respond immediately tooIoT Hub and then initiate hello firmware update process asynchronously.</span></span> <span data-ttu-id="ac1ac-217">hello eszközök ismertetik állapot hello belső vezérlőprogram frissítési folyamat keresztül jelentett tulajdonságértékek, ahogy az alábbi képernyőképek hello.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-217">hello devices provide status information about hello firmware update process through reported property values, as shown in hello following screenshots.</span></span> <span data-ttu-id="ac1ac-218">Válassza ki a hello **frissítése** ikon tooupdate hello információk hello eszköz és a feladat azokat:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-218">Choose hello **Refresh** icon tooupdate hello information in hello device and job lists:</span></span>

<span data-ttu-id="ac1ac-219">![A feladatlista hello belső vezérlőprogram frissítési lista futó megjelenítő][img-update-1]
![eszközök listáját ábrázoló belső vezérlőprogram frissítési állapot][img-update-2]
![listáját ábrázoló hello belső vezérlőprogram frissítési feladatlista teljes][img-update-3]</span><span class="sxs-lookup"><span data-stu-id="ac1ac-219">![Job list showing hello firmware update list running][img-update-1]
![Device list showing firmware update status][img-update-2]
![Job list showing hello firmware update list complete][img-update-3]</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-220">Éles környezetben feladatokat toorun kijelölt karbantartási időszak alatt ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-220">In a production environment, you can schedule jobs toorun during a designated maintenance window.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="ac1ac-221">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-221">Scenario review</span></span>

<span data-ttu-id="ac1ac-222">Ebben a forgatókönyvben egy potenciális problémát egy, a távoli eszközök hello riasztási előzmények hello irányítópult és a szűrő használata azonosított.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-222">In this scenario, you identified a potential issue with some of your remote devices using hello alarm history on hello dashboard and a filter.</span></span> <span data-ttu-id="ac1ac-223">Újból használt hello szűrési és egy feladat tooremotely konfigurálása hello eszközök tooprovide további információt toohelp hello probléma diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-223">You then used hello filter and a job tooremotely configure hello devices tooprovide more information toohelp diagnose hello issue.</span></span> <span data-ttu-id="ac1ac-224">Végezetül használt szűrő, ezért tooschedule karbantartási feladat az érintett hello eszközökön.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-224">Finally, you used a filter and a job tooschedule maintenance on hello affected devices.</span></span> <span data-ttu-id="ac1ac-225">Ha toohello irányítópult ad vissza, ellenőrizheti, hogy nincsenek-e már nem bármely adatforrásból származó eszközöket a megoldás a riasztások.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-225">If you return toohello dashboard, you can check that there are no longer any alarms coming from devices in your solution.</span></span> <span data-ttu-id="ac1ac-226">Használhatja a szűrőt, amely a belső vezérlőprogram hello tooverify naprakész a megoldás összes hello eszközön, és hogy nincsenek több nem megfelelő eszközök:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-226">You can use a filter tooverify that hello firmware is up-to-date on all hello devices in your solution and that there are no more unhealthy devices:</span></span>

![Szűrő, amely kimutatja, hogy az összes eszköz belső vezérlőprogramja naprakész][img-updated]

![Szűrő, amely kimutatja, hogy az összes eszköz kifogástalan állapotú][img-healthy]

## <a name="other-features"></a><span data-ttu-id="ac1ac-229">Egyéb jellemzők</span><span class="sxs-lookup"><span data-stu-id="ac1ac-229">Other features</span></span>

<span data-ttu-id="ac1ac-230">hello következő szakaszok ismertetik a távoli felügyeleti előkonfigurált megoldás hello néhány további szolgáltatásokat, nem ismerteti a hello előző forgatókönyv részeként.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-230">hello following sections describe some additional features of hello remote monitoring preconfigured solution that are not described as part of hello previous scenario.</span></span>

### <a name="customize-columns"></a><span data-ttu-id="ac1ac-231">Oszlopok testreszabása</span><span class="sxs-lookup"><span data-stu-id="ac1ac-231">Customize columns</span></span>

<span data-ttu-id="ac1ac-232">Testre szabhatja kiválasztásával hello eszközök listája látható hello információk **oszlop szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-232">You can customize hello information shown in hello device list by choosing **Column editor**.</span></span> <span data-ttu-id="ac1ac-233">Hozzáadhat és eltávolíthat jelentett tulajdonságot és címkeértékeket megjelenítő oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-233">You can add and remove columns that display reported property and tag values.</span></span> <span data-ttu-id="ac1ac-234">Átrendezheti és át is nevezheti az oszlopokat:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-234">You can also reorder and rename columns:</span></span>

   ![Oszlop szerkesztő adatmegőrzési hello eszközök listája][img-columneditor]

### <a name="customize-hello-device-icon"></a><span data-ttu-id="ac1ac-236">Hello eszköz ikon testreszabása</span><span class="sxs-lookup"><span data-stu-id="ac1ac-236">Customize hello device icon</span></span>

<span data-ttu-id="ac1ac-237">Testre szabhatja a hello eszköz listáját a következőtől: hello hello eszköz ikon **eszközadatok** panel az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-237">You can customize hello device icon displayed in hello device list from hello **Device Details** panel as follows:</span></span>

1. <span data-ttu-id="ac1ac-238">Válassza ki a hello ceruza ikonra tooopen hello **Szerkesztés kép** eszköz panel:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-238">Choose hello pencil icon tooopen hello **Edit image** panel for a device:</span></span>

   ![Eszközkép szerkesztőjének megnyitása][img-startimageedit]

1. <span data-ttu-id="ac1ac-240">Töltse fel az új képet vagy egy meglévő lemezképet hello használja, és válassza a **mentése**:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-240">Either upload a new image or use one of hello existing images and then choose **Save**:</span></span>

   ![Eszközkép szerkesztőjének szerkesztési folyamata][img-imageedit]

1. <span data-ttu-id="ac1ac-242">hello most kijelölt képet jeleníti meg hello **ikon** hello eszköz oszlop.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-242">hello image you selected now displays in hello **Icon** column for hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-243">hello lemezképe a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-243">hello image is stored in blob storage.</span></span> <span data-ttu-id="ac1ac-244">Hello eszköz iker egy címkét tartalmaz egy hivatkozást toohello lemezképet a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-244">A tag in hello device twin contains a link toohello image in blob storage.</span></span>

### <a name="add-a-device"></a><span data-ttu-id="ac1ac-245">Eszköz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ac1ac-245">Add a device</span></span>

<span data-ttu-id="ac1ac-246">Előre konfigurált hello megoldás telepítésekor automatikusan létesítsen 25 minta eszközök hello eszközök listája látható.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-246">When you deploy hello preconfigured solution, you automatically provision 25 sample devices that you can see in hello device list.</span></span> <span data-ttu-id="ac1ac-247">Ezek az eszközök Azure WebJobs-feladatban futó, *szimulált eszközök*.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-247">These devices are *simulated devices* running in an Azure WebJob.</span></span> <span data-ttu-id="ac1ac-248">Szimulált eszközök megkönnyítik az Ön tooexperiment előre konfigurált hello megoldással hello kell toodeploy valódi, fizikai eszközök nélkül.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-248">Simulated devices make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical devices.</span></span> <span data-ttu-id="ac1ac-249">Ha szeretné, hogy a valós toohello megoldás tooconnect, tekintse meg a hello [csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello] [ lnk-connect-rm] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-249">If you do want tooconnect a real device toohello solution, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

<span data-ttu-id="ac1ac-250">hello következő lépések bemutatják, hogyan tooadd szimulált eszköz toohello megoldást:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-250">hello following steps show you how tooadd a simulated device toohello solution:</span></span>

1. <span data-ttu-id="ac1ac-251">Lépjen vissza toohello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-251">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="ac1ac-252">tooadd egy eszközt, válassza a **+ A eszköz hozzáadása** hello bal alsó sarokban található.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-252">tooadd a device, choose **+ Add A Device** in hello bottom left corner.</span></span>

   ![Egy előre konfigurált toohello megoldás hozzáadása][img-adddevice]

1. <span data-ttu-id="ac1ac-254">Válasszon **új hozzáadása** a hello **szimulált eszköz** csempére.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-254">Choose **Add New** on hello **Simulated Device** tile.</span></span>

   ![Az új eszköz adatainak megadása az irányítópulton][img-addnew]

   <span data-ttu-id="ac1ac-256">Továbbá toocreating új szimulált eszköz, is hozzáadhat egy fizikai eszköz Ha úgy dönt, hogy toocreate egy **egyéni eszköz**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-256">In addition toocreating a new simulated device, you can also add a physical device if you choose toocreate a **Custom Device**.</span></span> <span data-ttu-id="ac1ac-257">toolearn fizikai eszközök toohello megoldás, bővebben lásd: [csatlakozni az eszköz toohello IoT Suite távoli felügyeleti előkonfigurált megoldás][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-257">toolearn more about connecting physical devices toohello solution, see [Connect your device toohello IoT Suite remote monitoring preconfigured solution][lnk-connect-rm].</span></span>

1. <span data-ttu-id="ac1ac-258">Válassza a **Meghatározom a saját eszközazonosítómat** elemet, és írjon be egy egyéni eszközazonosító nevet, például a **mydevice_01** nevet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-258">Select **Let me define my own Device ID**, and enter a unique device ID name such as **mydevice_01**.</span></span>

1. <span data-ttu-id="ac1ac-259">Válassza a **Létrehozás** elemet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-259">Choose **Create**.</span></span>

   ![Új eszköz mentése][img-definedevice]

1. <span data-ttu-id="ac1ac-261">A 3. lépésében **szimulált eszköz hozzáadása**, válassza a **végzett** tooreturn toohello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-261">In step 3 of **Add a simulated device**, choose **Done** tooreturn toohello device list.</span></span>

1. <span data-ttu-id="ac1ac-262">Megtekintheti az eszköz **futtató** hello eszköz listában.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-262">You can view your device **Running** in hello device list.</span></span>

    ![Új eszköz megtekintése az eszközlistában][img-runningnew]

1. <span data-ttu-id="ac1ac-264">Megtekintheti továbbá hello szimulált telemetriai hello Irányítópulton az új eszközről:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-264">You can also view hello simulated telemetry from your new device on hello dashboard:</span></span>

    ![Az új eszköz telemetriájának megtekintése][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a><span data-ttu-id="ac1ac-266">Eszköz letiltása és törlése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-266">Disable and delete a device</span></span>

<span data-ttu-id="ac1ac-267">Letilthatja az eszközöket, és a letiltásuk után eltávolíthatja őket:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-267">You can disable a device, and after it is disabled you can remove it:</span></span>

![Eszköz letiltása és eltávolítása][img-disable]

### <a name="add-a-rule"></a><span data-ttu-id="ac1ac-269">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ac1ac-269">Add a rule</span></span>

<span data-ttu-id="ac1ac-270">Nincsenek az előzőekben adott hozzá az új eszköz hello szabályok.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-270">There are no rules for hello new device you just added.</span></span> <span data-ttu-id="ac1ac-271">Ebben a szakaszban hozzáadhat egy szabályt, amely egy riasztás akkor váltja ki, ha új hello által jelentett hello hőmérséklet eszköz meghaladja 47 fokban megadva.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-271">In this section, you add a rule that triggers an alarm when hello temperature reported by hello new device exceeds 47 degrees.</span></span> <span data-ttu-id="ac1ac-272">Mielőtt elkezdené, figyelje meg, hogy hello telemetriai előzmények hello új eszköz hello irányítópulton látható hello eszköz hőmérséklete soha nem meghaladja 45 fokban megadva.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-272">Before you start, notice that hello telemetry history for hello new device on hello dashboard shows hello device temperature never exceeds 45 degrees.</span></span>

1. <span data-ttu-id="ac1ac-273">Lépjen vissza toohello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-273">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="ac1ac-274">tooadd szabály hello eszközt, jelölje ki az új eszköz hello **eszközök listában**, és válassza a **Hozzáadás szabály**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-274">tooadd a rule for hello device, select your new device in hello **Devices List**, and then choose **Add rule**.</span></span>

1. <span data-ttu-id="ac1ac-275">Hozzon létre egy szabályt, amely használja **hőmérséklet** hello adatmező és felhasználási **AlarmTemp** hello kimeneti, amikor hello elérte 47 fok:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-275">Create a rule that uses **Temperature** as hello data field and uses **AlarmTemp** as hello output when hello temperature exceeds 47 degrees:</span></span>

    ![Eszközszabály hozzáadása][img-adddevicerule]

1. <span data-ttu-id="ac1ac-277">toosave a módosításokat, válassza a **mentése és a szabályok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-277">toosave your changes, choose **Save and View Rules**.</span></span>

1. <span data-ttu-id="ac1ac-278">Válasszon **parancsok** hello eszköz részletező ablaktábláján hello új eszköz.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-278">Choose **Commands** in hello device details pane for hello new device.</span></span>

    ![Eszközszabály hozzáadása][img-adddevicerule2]

1. <span data-ttu-id="ac1ac-280">Válassza ki **ChangeSetPointTemp** hello parancs lista és a set **SetPointTemp** too45.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-280">Select **ChangeSetPointTemp** from hello command list and set **SetPointTemp** too45.</span></span> <span data-ttu-id="ac1ac-281">Ezután válassza a **Parancs küldése** elemet:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-281">Then choose **Send Command**:</span></span>

    ![Eszközszabály hozzáadása][img-adddevicerule3]

1. <span data-ttu-id="ac1ac-283">Lépjen vissza toohello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-283">Navigate back toohello dashboard.</span></span> <span data-ttu-id="ac1ac-284">Rövid idő múlva megjelenik egy új bejegyzést hello **riasztási előzmények** ablaktáblán, ha az új eszköz által jelentett hello hőmérséklet hello 47 fokos küszöbértéket meghaladó:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-284">After a short time, you will see a new entry in hello **Alarm History** pane when hello temperature reported by your new device exceeds hello 47-degree threshold:</span></span>

    ![Eszközszabály hozzáadása][img-adddevicerule4]

1. <span data-ttu-id="ac1ac-286">Megtekintheti és szerkesztheti a hello a szabályok **szabályok** hello irányítópult oldalán:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-286">You can review and edit all your rules on hello **Rules** page of hello dashboard:</span></span>

    ![Eszközszabályok listázása][img-rules]

1. <span data-ttu-id="ac1ac-288">Megtekintheti és szerkesztheti is lehet képernyőfelvételt készíteni a hello válasz tooa szabályban lévő hello műveleteket **műveletek** hello irányítópult oldalán:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-288">You can review and edit all hello actions that can be taken in response tooa rule on hello **Actions** page of hello dashboard:</span></span>

    ![Eszközműveletek listázása][img-actions]

> [!NOTE]
> <span data-ttu-id="ac1ac-290">E-mailt küldhet lehetséges toodefine műveleteket, vagy a válasz tooa SMS szabály, vagy egy üzleti rendszer integrálása egy [logikai alkalmazás][lnk-logic-apps].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-290">It is possible toodefine actions that can send an email message or SMS in response tooa rule or integrate with a line-of-business system through a [Logic App][lnk-logic-apps].</span></span> <span data-ttu-id="ac1ac-291">További információkért lásd: hello [Azure IoT Suite távoli megfigyelési csatlakozás logikai alkalmazás tooyour előre konfigurált megoldás][lnk-logicapptutorial].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-291">For more information, see hello [Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapptutorial].</span></span>

### <a name="manage-filters"></a><span data-ttu-id="ac1ac-292">Szűrők kezelése</span><span class="sxs-lookup"><span data-stu-id="ac1ac-292">Manage filters</span></span>

<span data-ttu-id="ac1ac-293">Hello eszközlistában hozzon létre, mentse, és töltse be újra a szűrők toodisplay eszközök csatlakoztatott tooyour hub testreszabott listáját.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-293">In hello device list, you can create, save, and reload filters toodisplay a customized list of devices connected tooyour hub.</span></span> <span data-ttu-id="ac1ac-294">toocreate szűrő:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-294">toocreate a filter:</span></span>

1. <span data-ttu-id="ac1ac-295">Válassza ki a hello Szerkesztés szűrő ikonja felett hello eszközök listája:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-295">Choose hello edit filter icon above hello list of devices:</span></span>

    ![Nyissa meg hello szűrő szerkesztő][img-editfiltericon]

1. <span data-ttu-id="ac1ac-297">A hello **szűrő szerkesztő**, vegye fel a hello mezők, operátorok és értékek toofilter hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-297">In hello **Filter editor**, add hello fields, operators, and values toofilter hello device list.</span></span> <span data-ttu-id="ac1ac-298">A szűrő több záradékok toorefine adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-298">You can add multiple clauses toorefine your filter.</span></span> <span data-ttu-id="ac1ac-299">Válasszon **szűrő** tooapply hello szűrő:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-299">Choose **Filter** tooapply hello filter:</span></span>

    ![Szűrő létrehozása][img-filtereditor]

1. <span data-ttu-id="ac1ac-301">Ebben a példában hello alapján szűri a program gyártó és a modell száma:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-301">In this example, hello list is filtered by manufacturer and model number:</span></span>

    ![Szűrt lista][img-filterelist]

1. <span data-ttu-id="ac1ac-303">toosave egy egyéni nevet, a szűrő válasszon hello **Mentés másként** ikon:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-303">toosave your filter with a custom name, choose hello **Save as** icon:</span></span>

    ![Szűrő mentése][img-savefilter]

1. <span data-ttu-id="ac1ac-305">egy korábban mentett szűrő tooreapply válasszon hello **mentett szűrő megnyitása** ikon:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-305">tooreapply a filter you saved previously, choose hello **Open saved filter** icon:</span></span>

    ![Szűrő megnyitása][img-openfilter]

<span data-ttu-id="ac1ac-307">Eszközazonosító, eszközállapot, kívánt tulajdonságok, jelentett tulajdonságok és címkék alapján hozhat létre szűrőket.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-307">You can create filters based on device id, device state, desired properties, reported properties, and tags.</span></span> <span data-ttu-id="ac1ac-308">Hozzáadja a saját egyéni címkék tooa eszköz hello **címkék** hello szakasza **eszközadatok** panelen, vagy futtassa egy feladat tooupdate címkék több eszközön.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-308">You add your own custom tags tooa device in hello **Tags** section of hello **Device Details** panel, or run a job tooupdate tags on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-309">A hello **szűrő szerkesztő**, használhatja a hello **nézet speciális** tooedit hello lekérdezés szövegének közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-309">In hello **Filter editor**, you can use hello **Advanced view** tooedit hello query text directly.</span></span>

### <a name="commands"></a><span data-ttu-id="ac1ac-310">Parancsok</span><span class="sxs-lookup"><span data-stu-id="ac1ac-310">Commands</span></span>

<span data-ttu-id="ac1ac-311">A hello **eszközadatok** panelen parancsok toohello eszköz küldhet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-311">From hello **Device Details** panel, you can send commands toohello device.</span></span> <span data-ttu-id="ac1ac-312">Indításakor egy eszközt, küldi hello információ parancsok toohello megoldás támogatja.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-312">When a device first starts, it sends information about hello commands it supports toohello solution.</span></span> <span data-ttu-id="ac1ac-313">Parancsok és módszerek hello különbségei tárgyalását lásd: [Azure IoT Hub felhő eszközre beállítások][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-313">For a discussion of hello differences between commands and methods, see [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance].</span></span>

1. <span data-ttu-id="ac1ac-314">Válasszon **parancsok** a hello **. eszköz részletei** hello kiválasztott eszköz panel:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-314">Choose **Commands** in hello **Device Details** panel for hello selected device:</span></span>

   ![Eszközparancsok az irányítópulton][img-devicecommands]

1. <span data-ttu-id="ac1ac-316">Válassza ki **PingDevice** hello listából.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-316">Select **PingDevice** from hello command list.</span></span>

1. <span data-ttu-id="ac1ac-317">Válassza a **Parancs küldése** elemet.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-317">Choose **Send Command**.</span></span>

1. <span data-ttu-id="ac1ac-318">Az előző hello hello parancs hello állapotát tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-318">You can see hello status of hello command in hello command history.</span></span>

   ![Parancsállapot az irányítópulton][img-pingcommand]

<span data-ttu-id="ac1ac-320">hello megoldás minden parancs küldi hello állapotát követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-320">hello solution tracks hello status of each command it sends.</span></span> <span data-ttu-id="ac1ac-321">Hello eredménye kezdetben **függőben lévő**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-321">Initially hello result is **Pending**.</span></span> <span data-ttu-id="ac1ac-322">Amikor hello eszköz jelzi, hogy teljesítette-e hello parancs, hello eredménye túl van-e állítva**sikeres**.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-322">When hello device reports that it has executed hello command, hello result is set too**Success**.</span></span>

## <a name="behind-hello-scenes"></a><span data-ttu-id="ac1ac-323">Hello háttérben</span><span class="sxs-lookup"><span data-stu-id="ac1ac-323">Behind hello scenes</span></span>

<span data-ttu-id="ac1ac-324">Amikor telepít egy előre beállított megoldás, hello központi telepítési folyamat több erőforrást hello kijelölt Azure-előfizetés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-324">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="ac1ac-325">Megtekintheti ezeket az erőforrásokat a hello Azure [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="ac1ac-325">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="ac1ac-326">hello központi telepítési folyamat létrehoz egy **erőforráscsoport** előkonfigurált megoldást választja hello neve alapján néven:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-326">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Hello Azure-portálon az előkonfigurált megoldás][img-portal]

<span data-ttu-id="ac1ac-328">Megtekintheti a hello-beállítások az egyes erőforrások hello az erőforrások listájához a hello erőforráscsoport lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-328">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="ac1ac-329">Előre konfigurált hello megoldás forráskódját hello is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-329">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="ac1ac-330">távoli előkonfigurált megoldás forráskódját figyelési hello van hello [azure iot-távoli-ellenőrző] [ lnk-rmgithub] GitHub-tárházban:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-330">hello remote monitoring preconfigured solution source code is in hello [azure-iot-remote-monitoring][lnk-rmgithub] GitHub repository:</span></span>

* <span data-ttu-id="ac1ac-331">Hello **DeviceAdministration** mappa hello forráskódja hello irányítópult tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-331">hello **DeviceAdministration** folder contains hello source code for hello dashboard.</span></span>
* <span data-ttu-id="ac1ac-332">Hello **szimulátor** mappa tartalmazza hello hello szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-332">hello **Simulator** folder contains hello source code for hello simulated device.</span></span>
* <span data-ttu-id="ac1ac-333">Hello **EventProcessor** mappa hello forráskódja kezelő Bejövő hello telemetriai hello háttér-folyamat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-333">hello **EventProcessor** folder contains hello source code for hello back-end process that handles hello incoming telemetry.</span></span>

<span data-ttu-id="ac1ac-334">Amikor elkészült, hogy törli előre konfigurált hello megoldás az Azure-előfizetéshez hello [azureiotsuite.com] [ lnk-azureiotsuite] hely.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-334">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="ac1ac-335">Ez a hely összes hello előre konfigurált hello megoldás létrehozása után kiépített erőforrások tooeasily törlése lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-335">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="ac1ac-336">tooensure törlése minden kapcsolódó előre konfigurált toohello megoldás, törölje a hello [azureiotsuite.com] [ lnk-azureiotsuite] helyről, és ne törölje az erőforráscsoportot hello hello portálon.</span><span class="sxs-lookup"><span data-stu-id="ac1ac-336">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site and do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac1ac-337">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac1ac-337">Next Steps</span></span>

<span data-ttu-id="ac1ac-338">Most, hogy egy előre konfigurált működő megoldást telepítése után, továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="ac1ac-338">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="ac1ac-339">[A távoli figyelési előre konfigurált megoldás bemutatója][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="ac1ac-339">[Remote monitoring preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="ac1ac-340">[Csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="ac1ac-340">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="ac1ac-341">[Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="ac1ac-341">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md