---
title: "egy egyéni szabály Azure IoT Suite aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate egy egyéni szabályt az IoT Suite előre konfigurált megoldás."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 6c5bb2ca54f3f17b99ad482e727c8e9fa28d7fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="17fb8-103">Hozzon létre egy egyéni szabály hello távoli felügyeleti előkonfigurált megoldás</span><span class="sxs-lookup"><span data-stu-id="17fb8-103">Create a custom rule in hello remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="17fb8-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="17fb8-104">Introduction</span></span>

<span data-ttu-id="17fb8-105">Előre konfigurált hello megoldásokat konfigurálhat [szabályokat, amelyek indul el, ha egy telemetriai érték, egy eszköz eléri a meghatározott][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="17fb8-105">In hello preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="17fb8-106">[Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello] [ lnk-dynamic-telemetry] ismerteti, hogyan értékeket is hozzáadhat egyéni telemetriai adatokat, például a *ExternalTemperature* tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="17fb8-106">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* tooyour solution.</span></span> <span data-ttu-id="17fb8-107">Ez a cikk bemutatja, hogyan toocreate egyéni szabály dinamikus telemetriai adatokat a típusokat a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="17fb8-107">This article shows you how toocreate custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="17fb8-108">Ez az oktatóanyag egy egyszerű Node.js szimulált eszköz toogenerate dinamikus telemetriai toosend toohello előkonfigurált megoldás háttérrendszerének használja.</span><span class="sxs-lookup"><span data-stu-id="17fb8-108">This tutorial uses a simple Node.js simulated device toogenerate dynamic telemetry toosend toohello preconfigured solution back end.</span></span> <span data-ttu-id="17fb8-109">Majd hozzá az egyéni szabályok a hello **RemoteMonitoring** Visual Studio megoldás és a testreszabott háttér tooyour Azure-előfizetés telepítése.</span><span class="sxs-lookup"><span data-stu-id="17fb8-109">You then add custom rules in hello **RemoteMonitoring** Visual Studio solution and deploy this customized back end tooyour Azure subscription.</span></span>

<span data-ttu-id="17fb8-110">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="17fb8-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="17fb8-111">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="17fb8-111">An active Azure subscription.</span></span> <span data-ttu-id="17fb8-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="17fb8-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="17fb8-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="17fb8-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="17fb8-114">[NODE.js] [ lnk-node] verzió 0.12.x vagy későbbi toocreate a szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="17fb8-114">[Node.js][lnk-node] version 0.12.x or later toocreate a simulated device.</span></span>
* <span data-ttu-id="17fb8-115">A Visual Studio 2015-öt vagy a Visual Studio 2017 toomodify előre konfigurált hello megoldás háttérrendszere végén az új szabályokat.</span><span class="sxs-lookup"><span data-stu-id="17fb8-115">Visual Studio 2015 or Visual Studio 2017 toomodify hello preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="17fb8-116">Jegyezze fel az üzembe helyezéshez választott hello megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="17fb8-116">Make a note of hello solution name you chose for your deployment.</span></span> <span data-ttu-id="17fb8-117">Az oktatóanyag későbbi részében szüksége ezen megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="17fb8-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="17fb8-118">Miután ellenőrizte, hogy akkor küld le lehet állítani hello Node.js-Konzolalkalmazás **ExternalTemperature** telemetriai toohello előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="17fb8-118">You can stop hello Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry toohello preconfigured solution.</span></span> <span data-ttu-id="17fb8-119">Hello konzolablak tartsa nyitva, mert a Node.js-Konzolalkalmazás hello egyéni szabály toohello megoldás hozzáadása után ismét futtatni.</span><span class="sxs-lookup"><span data-stu-id="17fb8-119">Keep hello console window open because you run this Node.js console app again after you add hello custom rule toohello solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="17fb8-120">A szabály a tárolási helyek</span><span class="sxs-lookup"><span data-stu-id="17fb8-120">Rule storage locations</span></span>

<span data-ttu-id="17fb8-121">Két külön helyen megőrződjenek szabályokkal kapcsolatos információkat:</span><span class="sxs-lookup"><span data-stu-id="17fb8-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="17fb8-122">**DeviceRulesNormalizedTable** – Ez a táblázat a táblázat egy normalizált hello megoldás portál által definiált toohello szabályok hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="17fb8-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference toohello rules defined by hello solution portal.</span></span> <span data-ttu-id="17fb8-123">Hello megoldás portal eszköz szabályokat jeleníti meg, amikor lekérdezi a következő táblázat hello szabálydefiníciók.</span><span class="sxs-lookup"><span data-stu-id="17fb8-123">When hello solution portal displays device rules, it queries this table for hello rule definitions.</span></span>
* <span data-ttu-id="17fb8-124">**DeviceRules** blob – a blob tároló összes hello szabályai vannak megadva összes regisztrált eszközökre és a hivatkozás bemeneti toohello Azure Stream Analytics-feladatok típusúként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="17fb8-124">**DeviceRules** blob – This blob stores all hello rules defined for all registered devices and is defined as a reference input toohello Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="17fb8-125">Meglévő szabály frissítésekor, vagy adjon meg egy új szabályt hello megoldás portal, hello tábla és a blob frissített tooreflect hello módosítások.</span><span class="sxs-lookup"><span data-stu-id="17fb8-125">When you update an existing rule or define a new rule in hello solution portal, both hello table and blob are updated tooreflect hello changes.</span></span> <span data-ttu-id="17fb8-126">hello portál megjeleníti-definíció hello szabály hello tábla tárolót és hello Stream Analytics-feladatok által hivatkozott definition származik hello blob hello szabály származik.</span><span class="sxs-lookup"><span data-stu-id="17fb8-126">hello rule definition displayed in hello portal comes from hello table store, and hello rule definition referenced by hello Stream Analytics jobs comes from hello blob.</span></span> 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="17fb8-127">Hello RemoteMonitoring Visual Studio megoldás frissítése</span><span class="sxs-lookup"><span data-stu-id="17fb8-127">Update hello RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="17fb8-128">hello következő lépések bemutatják, hogyan toomodify hello RemoteMonitoring Visual Studio megoldás tooinclude hello használó új szabály **ExternalTemperature** hello szimulált eszközről küldött telemetriai:</span><span class="sxs-lookup"><span data-stu-id="17fb8-128">hello following steps show you how toomodify hello RemoteMonitoring Visual Studio solution tooinclude a new rule that uses hello **ExternalTemperature** telemetry sent from hello simulated device:</span></span>

1. <span data-ttu-id="17fb8-129">Ha Ön még nem tette meg, klónozás hello **azure iot-távoli-ellenőrző** tárház tooa megfelelő helye a helyi gépen hello Git parancs a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="17fb8-129">If you have not already done so, clone hello **azure-iot-remote-monitoring** repository tooa suitable location on your local machine using hello following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="17fb8-130">A Visual Studio, nyissa meg az hello RemoteMonitoring.sln fájl helyi másolatát hello **azure iot-távoli-ellenőrző** tárházba.</span><span class="sxs-lookup"><span data-stu-id="17fb8-130">In Visual Studio, open hello RemoteMonitoring.sln file from your local copy of hello **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="17fb8-131">Nyissa meg Infrastructure\Models\DeviceRuleBlobEntity.cs hello fájlt, és adja hozzá egy **ExternalTemperature** tulajdonság az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="17fb8-131">Open hello file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="17fb8-132">A hello ugyanazt a fájlt, adja hozzá egy **ExternalTemperatureRuleOutput** tulajdonság az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="17fb8-132">In hello same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="17fb8-133">Nyissa meg a Infrastructure\Models\DeviceRuleDataFields.cs hello fájlt, és adja hozzá a következő hello **ExternalTemperature** után hello meglévő tulajdonság **páratartalom** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="17fb8-133">Open hello file Infrastructure\Models\DeviceRuleDataFields.cs and add hello following **ExternalTemperature** property after hello existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="17fb8-134">A hello ugyanazt a fájlt, a frissítéskezelés hello **_availableDataFields** metódus tooinclude **ExternalTemperature** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="17fb8-134">In hello same file, update hello **_availableDataFields** method tooinclude **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="17fb8-135">Nyissa meg a hello fájl Infrastructure\Repository\DeviceRulesRepository.cs, és módosítsa a hello **BuildBlobEntityListFromTableRows** módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="17fb8-135">Open hello file Infrastructure\Repository\DeviceRulesRepository.cs and modify hello **BuildBlobEntityListFromTableRows** method as follows:</span></span>

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-hello-solution"></a><span data-ttu-id="17fb8-136">Építse újra, és telepítse újra a hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="17fb8-136">Rebuild and redeploy hello solution.</span></span>

<span data-ttu-id="17fb8-137">Most már telepítheti a frissített hello megoldás tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="17fb8-137">You can now deploy hello updated solution tooyour Azure subscription.</span></span>

1. <span data-ttu-id="17fb8-138">Nyisson meg egy rendszergazda jogú parancssort, és keresse meg a helyi példány hello azure iot-távoli-ellenőrző tárház toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="17fb8-138">Open an elevated command prompt and navigate toohello root of your local copy of hello azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="17fb8-139">toodeploy a frissített megoldás, futtassa a következő parancs és hello **{telepítési neve}** hello nevű előre konfigurált megoldás a központi telepítés korábban feljegyzett:</span><span class="sxs-lookup"><span data-stu-id="17fb8-139">toodeploy your updated solution, run hello following command substituting **{deployment name}** with hello name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a><span data-ttu-id="17fb8-140">Frissítés hello Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="17fb8-140">Update hello Stream Analytics job</span></span>

<span data-ttu-id="17fb8-141">Ha hello telepítés befejeződött, hello Stream Analytics toouse hello új szabály feladatdefiníciók frissítheti.</span><span class="sxs-lookup"><span data-stu-id="17fb8-141">When hello deployment is complete, you can update hello Stream Analytics job toouse hello new rule definitions.</span></span>

1. <span data-ttu-id="17fb8-142">Hello Azure-portálon keresse meg az előkonfigurált megoldás erőforrásokat tartalmazó toohello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="17fb8-142">In hello Azure portal, navigate toohello resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="17fb8-143">Ez az erőforráscsoport neve megegyezik a megadott hello van megoldás hello hello üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="17fb8-143">This resource group has hello same name you specified for hello solution during hello deployment.</span></span>

2. <span data-ttu-id="17fb8-144">Keresse meg a toohello {telepítési neve}-szabályok Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="17fb8-144">Navigate toohello {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="17fb8-145">Kattintson a **leállítása** toostop hello Stream Analytics-feladat futtatását.</span><span class="sxs-lookup"><span data-stu-id="17fb8-145">Click **Stop** toostop hello Stream Analytics job from running.</span></span> <span data-ttu-id="17fb8-146">(Meg kell várnia a folyamatos átviteli feladat toostop hello lekérdezés szerkesztése előtt hello).</span><span class="sxs-lookup"><span data-stu-id="17fb8-146">(You must wait for hello streaming job toostop before you can edit hello query).</span></span>

4. <span data-ttu-id="17fb8-147">Kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="17fb8-147">Click **Query**.</span></span> <span data-ttu-id="17fb8-148">Hello lekérdezés tooinclude hello szerkesztése **válasszon** nyilatkozata **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="17fb8-148">Edit hello query tooinclude hello **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="17fb8-149">hello alábbi példa a teljes lekérdezés hello hello új **válasszon** utasítást:</span><span class="sxs-lookup"><span data-stu-id="17fb8-149">hello following sample shows hello complete query with hello new **SELECT** statement:</span></span>

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
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. <span data-ttu-id="17fb8-150">Kattintson a **mentése** toochange hello szabály lekérdezés frissítése.</span><span class="sxs-lookup"><span data-stu-id="17fb8-150">Click **Save** toochange hello updated rule query.</span></span>

6. <span data-ttu-id="17fb8-151">Kattintson a **Start** toostart hello Stream Analytics-feladat újra futtatni.</span><span class="sxs-lookup"><span data-stu-id="17fb8-151">Click **Start** toostart hello Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-hello-dashboard"></a><span data-ttu-id="17fb8-152">Az új szabály hozzáadása a hello irányítópult</span><span class="sxs-lookup"><span data-stu-id="17fb8-152">Add your new rule in hello dashboard</span></span>

<span data-ttu-id="17fb8-153">Mostantól hozzáadhatja azok hello **ExternalTemperature** szabály tooa eszköz hello megoldás irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="17fb8-153">You can now add hello **ExternalTemperature** rule tooa device in hello solution dashboard.</span></span>

1. <span data-ttu-id="17fb8-154">Keresse meg a toohello megoldás portálon.</span><span class="sxs-lookup"><span data-stu-id="17fb8-154">Navigate toohello solution portal.</span></span>

2. <span data-ttu-id="17fb8-155">Keresse meg a toohello **eszközök** panel.</span><span class="sxs-lookup"><span data-stu-id="17fb8-155">Navigate toohello **Devices** panel.</span></span>

3. <span data-ttu-id="17fb8-156">Keresse meg a hello egyéni eszköz létrehozott által küldött **ExternalTemperature** telemetriai adatok és a hello **. eszköz részletei** panelen, kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="17fb8-156">Locate hello custom device you created that sends **ExternalTemperature** telemetry and on hello **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="17fb8-157">Válassza ki **ExternalTemperature** a **adatmező**.</span><span class="sxs-lookup"><span data-stu-id="17fb8-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="17fb8-158">Állítsa be **küszöbérték** too56.</span><span class="sxs-lookup"><span data-stu-id="17fb8-158">Set **Threshold** too56.</span></span> <span data-ttu-id="17fb8-159">Kattintson a **mentéséhez és a szabályok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="17fb8-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="17fb8-160">Térjen vissza a toohello irányítópult tooview hello riasztás előzményeit.</span><span class="sxs-lookup"><span data-stu-id="17fb8-160">Return toohello dashboard tooview hello alarm history.</span></span>

7. <span data-ttu-id="17fb8-161">Hello konzolablakban nyitva maradt, hello Node.js konzol app toobegin küldésének megkezdése **ExternalTemperature** telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="17fb8-161">In hello console window you left open, start hello Node.js console app toobegin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="17fb8-162">Figyelje meg, hogy hello **riasztási előzmények** táblázat új riasztások aktiválásáról hello új szabályt.</span><span class="sxs-lookup"><span data-stu-id="17fb8-162">Notice that hello **Alarm History** table shows new alarms when hello new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="17fb8-163">További információ</span><span class="sxs-lookup"><span data-stu-id="17fb8-163">Additional information</span></span>

<span data-ttu-id="17fb8-164">Változó hello operátor  **>**  bonyolultabb és túllép hello lépéseit az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="17fb8-164">Changing hello operator **>** is more complex and goes beyond hello steps outlined in this tutorial.</span></span> <span data-ttu-id="17fb8-165">Módosíthatja a Stream Analytics-feladat toouse hello bármilyen tetszés operátor, adott operátor hello megoldás portálon tükröző napjainkban összetettebb feladat.</span><span class="sxs-lookup"><span data-stu-id="17fb8-165">While you can change hello Stream Analytics job toouse whatever operator you like, reflecting that operator in hello solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="17fb8-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17fb8-166">Next steps</span></span>
<span data-ttu-id="17fb8-167">Most, hogy megtudhatta, hogyan toocreate egyéni szabályok, részletesebben előre konfigurált hello megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="17fb8-167">Now that you've seen how toocreate custom rules, you can learn more about hello preconfigured solutions:</span></span>

- <span data-ttu-id="17fb8-168">[Csatlakozás a Logic App tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="17fb8-168">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="17fb8-169">[Eszköz információk metaadatok hello távoli figyelési megoldást előre konfigurált][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="17fb8-169">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md