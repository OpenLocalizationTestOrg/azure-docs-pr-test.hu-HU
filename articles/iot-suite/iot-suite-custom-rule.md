---
title: "Hozzon létre egy egyéni szabály Azure IoT Suite |} Microsoft Docs"
description: "A megoldás IoT Suite előre konfigurált egy egyéni szabály létrehozásának módjáról."
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
ms.openlocfilehash: d58c27234ea05a82aaa3e8d72f70c1449980df09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="9593b-103">Hozzon létre egy egyéni szabályt a távoli felügyeleti előkonfigurált megoldás</span><span class="sxs-lookup"><span data-stu-id="9593b-103">Create a custom rule in the remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="9593b-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9593b-104">Introduction</span></span>

<span data-ttu-id="9593b-105">Az előkonfigurált megoldásokat konfigurálhat [szabályokat, amelyek indul el, ha egy telemetriai érték, egy eszköz eléri a meghatározott][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="9593b-105">In the preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="9593b-106">[Dinamikus telemetriai adatokat a távoli figyelési megoldást előre konfigurált] [ lnk-dynamic-telemetry] ismerteti, hogyan értékeket is hozzáadhat egyéni telemetriai adatokat, például a *ExternalTemperature* a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="9593b-106">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* to your solution.</span></span> <span data-ttu-id="9593b-107">Ez a cikk bemutatja, hogyan dinamikus telemetriai típusok egyéni szabály létrehozására a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="9593b-107">This article shows you how to create custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="9593b-108">Ebben az oktatóanyagban egy egyszerű Node.js szimulált eszköz használják a dinamikus telemetriai adatokat küldhet az előkonfigurált megoldás háttérrendszerének létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9593b-108">This tutorial uses a simple Node.js simulated device to generate dynamic telemetry to send to the preconfigured solution back end.</span></span> <span data-ttu-id="9593b-109">Majd adja hozzá az egyéni szabályok a **RemoteMonitoring** Visual Studio megoldás, és a testreszabott háttér telepítse az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9593b-109">You then add custom rules in the **RemoteMonitoring** Visual Studio solution and deploy this customized back end to your Azure subscription.</span></span>

<span data-ttu-id="9593b-110">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="9593b-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="9593b-111">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="9593b-111">An active Azure subscription.</span></span> <span data-ttu-id="9593b-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="9593b-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9593b-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="9593b-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="9593b-114">[NODE.js] [ lnk-node] verzió 0.12.x vagy újabb rendszert a szimulált eszköz létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9593b-114">[Node.js][lnk-node] version 0.12.x or later to create a simulated device.</span></span>
* <span data-ttu-id="9593b-115">A Visual Studio 2015-öt vagy a Visual Studio 2017 módosíthatja az előkonfigurált megoldás végén az új szabályokat.</span><span class="sxs-lookup"><span data-stu-id="9593b-115">Visual Studio 2015 or Visual Studio 2017 to modify the preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="9593b-116">Jegyezze fel az üzembe helyezéshez választott megoldás nevét.</span><span class="sxs-lookup"><span data-stu-id="9593b-116">Make a note of the solution name you chose for your deployment.</span></span> <span data-ttu-id="9593b-117">Az oktatóanyag későbbi részében szüksége ezen megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="9593b-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="9593b-118">Miután ellenőrizte, hogy akkor küld le lehet állítani a Node.js-Konzolalkalmazás **ExternalTemperature** telemetria bekapcsolásával az előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="9593b-118">You can stop the Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry to the preconfigured solution.</span></span> <span data-ttu-id="9593b-119">A konzolablakban tartsa nyitva, mert a Node.js-Konzolalkalmazás a megoldáshoz az egyéni szabály hozzáadása után újra futtatni.</span><span class="sxs-lookup"><span data-stu-id="9593b-119">Keep the console window open because you run this Node.js console app again after you add the custom rule to the solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="9593b-120">A szabály a tárolási helyek</span><span class="sxs-lookup"><span data-stu-id="9593b-120">Rule storage locations</span></span>

<span data-ttu-id="9593b-121">Két külön helyen megőrződjenek szabályokkal kapcsolatos információkat:</span><span class="sxs-lookup"><span data-stu-id="9593b-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="9593b-122">**DeviceRulesNormalizedTable** tábla – Ez a táblázat a megoldás portál által meghatározott szabályok normalizált hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9593b-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference to the rules defined by the solution portal.</span></span> <span data-ttu-id="9593b-123">A megoldás portal eszköz szabályokat jeleníti meg, amikor lekérdezi a következő táblázat a szabálydefiníciók.</span><span class="sxs-lookup"><span data-stu-id="9593b-123">When the solution portal displays device rules, it queries this table for the rule definitions.</span></span>
* <span data-ttu-id="9593b-124">**DeviceRules** blob – a blob tárolja az összes szabály definiált összes regisztrált eszközökhöz, és adjon meg az Azure Stream Analytics-feladatok hivatkozásként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="9593b-124">**DeviceRules** blob – This blob stores all the rules defined for all registered devices and is defined as a reference input to the Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="9593b-125">Meglévő szabály frissítésekor, vagy adjon meg egy új szabályt a megoldás portálon, a tábla és a blob frissülnek a módosításokkal.</span><span class="sxs-lookup"><span data-stu-id="9593b-125">When you update an existing rule or define a new rule in the solution portal, both the table and blob are updated to reflect the changes.</span></span> <span data-ttu-id="9593b-126">A tábla tároló származik a szabálydefiníciót, megjelenik a portálon, és a Stream Analytics-feladatok által hivatkozott szabálydefiníciója származik a blob.</span><span class="sxs-lookup"><span data-stu-id="9593b-126">The rule definition displayed in the portal comes from the table store, and the rule definition referenced by the Stream Analytics jobs comes from the blob.</span></span> 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="9593b-127">Frissítés a RemoteMonitoring Visual Studio megoldás</span><span class="sxs-lookup"><span data-stu-id="9593b-127">Update the RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="9593b-128">A következő lépések bemutatják a módosítása a RemoteMonitoring Visual Studio megoldás egy új szabályt, amely használja felvenni a **ExternalTemperature** a szimulált eszközről küldött telemetriai:</span><span class="sxs-lookup"><span data-stu-id="9593b-128">The following steps show you how to modify the RemoteMonitoring Visual Studio solution to include a new rule that uses the **ExternalTemperature** telemetry sent from the simulated device:</span></span>

1. <span data-ttu-id="9593b-129">Ha még nem tette meg, klónozza a **azure iot-távoli-ellenőrző** tárházat a helyi számítógépen a következő Git-paranccsal megfelelő helyre:</span><span class="sxs-lookup"><span data-stu-id="9593b-129">If you have not already done so, clone the **azure-iot-remote-monitoring** repository to a suitable location on your local machine using the following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="9593b-130">A Visual Studio, nyissa meg a RemoteMonitoring.sln fájl helyi másolatát a **azure iot-távoli-ellenőrző** tárházba.</span><span class="sxs-lookup"><span data-stu-id="9593b-130">In Visual Studio, open the RemoteMonitoring.sln file from your local copy of the **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="9593b-131">Nyissa meg a fájlt Infrastructure\Models\DeviceRuleBlobEntity.cs, és adja hozzá egy **ExternalTemperature** tulajdonság az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9593b-131">Open the file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="9593b-132">Ugyanebben a fájlban adja hozzá egy **ExternalTemperatureRuleOutput** tulajdonság az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9593b-132">In the same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="9593b-133">Nyissa meg a fájlt Infrastructure\Models\DeviceRuleDataFields.cs, és adja hozzá a következő **ExternalTemperature** követően a meglévő tulajdonság **páratartalom** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="9593b-133">Open the file Infrastructure\Models\DeviceRuleDataFields.cs and add the following **ExternalTemperature** property after the existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="9593b-134">Ugyanebben a fájlban frissítse a **_availableDataFields** felvenni metódus **ExternalTemperature** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9593b-134">In the same file, update the **_availableDataFields** method to include **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="9593b-135">Nyissa meg a fájlt Infrastructure\Repository\DeviceRulesRepository.cs, és módosítsa a **BuildBlobEntityListFromTableRows** módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9593b-135">Open the file Infrastructure\Repository\DeviceRulesRepository.cs and modify the **BuildBlobEntityListFromTableRows** method as follows:</span></span>

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

## <a name="rebuild-and-redeploy-the-solution"></a><span data-ttu-id="9593b-136">Építse újra, és a megoldás újbóli telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9593b-136">Rebuild and redeploy the solution.</span></span>

<span data-ttu-id="9593b-137">A frissített megoldás most már telepítheti az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9593b-137">You can now deploy the updated solution to your Azure subscription.</span></span>

1. <span data-ttu-id="9593b-138">Nyisson meg egy rendszergazda jogú parancssort, és keresse meg a helyi másolat készítése az azure-iot-távelérési-figyelés tárház gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="9593b-138">Open an elevated command prompt and navigate to the root of your local copy of the azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="9593b-139">A frissített megoldás telepítéséhez futtassa a következő parancs és **{telepítési neve}** nevű, a korábban feljegyzett előkonfigurált megoldás üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="9593b-139">To deploy your updated solution, run the following command substituting **{deployment name}** with the name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a><span data-ttu-id="9593b-140">Frissítés a Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="9593b-140">Update the Stream Analytics job</span></span>

<span data-ttu-id="9593b-141">Ha a telepítés befejeződött, a Stream Analytics-feladat az új szabálydefiníciók használandó frissítheti.</span><span class="sxs-lookup"><span data-stu-id="9593b-141">When the deployment is complete, you can update the Stream Analytics job to use the new rule definitions.</span></span>

1. <span data-ttu-id="9593b-142">Az Azure-portálon lépjen az előkonfigurált megoldás erőforrásokat tartalmazó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9593b-142">In the Azure portal, navigate to the resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="9593b-143">Ez az erőforráscsoport a telepítés során a megoldáshoz megadott névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9593b-143">This resource group has the same name you specified for the solution during the deployment.</span></span>

2. <span data-ttu-id="9593b-144">Keresse meg az {neve}-szabályok Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="9593b-144">Navigate to the {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="9593b-145">Kattintson a **leállítása** leállítja a Stream Analytics-feladat futtatását.</span><span class="sxs-lookup"><span data-stu-id="9593b-145">Click **Stop** to stop the Stream Analytics job from running.</span></span> <span data-ttu-id="9593b-146">(Várnia kell a folyamatos átviteli feladat leállítása a lekérdezés szerkesztése előtt).</span><span class="sxs-lookup"><span data-stu-id="9593b-146">(You must wait for the streaming job to stop before you can edit the query).</span></span>

4. <span data-ttu-id="9593b-147">Kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="9593b-147">Click **Query**.</span></span> <span data-ttu-id="9593b-148">Szerkessze a lekérdezést, hogy tartalmazzák a **válasszon** nyilatkozata **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="9593b-148">Edit the query to include the **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="9593b-149">A következő példa bemutatja a teljes lekérdezés az új **válasszon** utasítást:</span><span class="sxs-lookup"><span data-stu-id="9593b-149">The following sample shows the complete query with the new **SELECT** statement:</span></span>

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

5. <span data-ttu-id="9593b-150">Kattintson a **mentése** a frissített szabály lekérdezés módosításához.</span><span class="sxs-lookup"><span data-stu-id="9593b-150">Click **Save** to change the updated rule query.</span></span>

6. <span data-ttu-id="9593b-151">Kattintson a **Start** újra futtatni a Stream Analytics-feladat elindításához.</span><span class="sxs-lookup"><span data-stu-id="9593b-151">Click **Start** to start the Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-the-dashboard"></a><span data-ttu-id="9593b-152">Az irányítópult az új szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9593b-152">Add your new rule in the dashboard</span></span>

<span data-ttu-id="9593b-153">Mostantól hozzáadhatja a **ExternalTemperature** szabály egy eszközre, a megoldás irányítópultjának.</span><span class="sxs-lookup"><span data-stu-id="9593b-153">You can now add the **ExternalTemperature** rule to a device in the solution dashboard.</span></span>

1. <span data-ttu-id="9593b-154">Navigáljon a megoldás portálra.</span><span class="sxs-lookup"><span data-stu-id="9593b-154">Navigate to the solution portal.</span></span>

2. <span data-ttu-id="9593b-155">Keresse meg a **eszközök** panel.</span><span class="sxs-lookup"><span data-stu-id="9593b-155">Navigate to the **Devices** panel.</span></span>

3. <span data-ttu-id="9593b-156">Keresse meg az egyéni eszközt létrehozott által küldött **ExternalTemperature** telemetriai adatok és a a **eszközadatok** panelen, kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9593b-156">Locate the custom device you created that sends **ExternalTemperature** telemetry and on the **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="9593b-157">Válassza ki **ExternalTemperature** a **adatmező**.</span><span class="sxs-lookup"><span data-stu-id="9593b-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="9593b-158">Állítsa be **küszöbérték** 56.</span><span class="sxs-lookup"><span data-stu-id="9593b-158">Set **Threshold** to 56.</span></span> <span data-ttu-id="9593b-159">Kattintson a **mentéséhez és a szabályok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="9593b-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="9593b-160">Térjen vissza az irányítópulton a riasztás előzményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="9593b-160">Return to the dashboard to view the alarm history.</span></span>

7. <span data-ttu-id="9593b-161">A konzolablakban nyitva maradt, indítsa el a Node.js-Konzolalkalmazás megkezdéséhez küldése **ExternalTemperature** telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9593b-161">In the console window you left open, start the Node.js console app to begin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="9593b-162">Figyelje meg, hogy a **riasztási előzmények** táblázat új riasztások az új szabály indítását.</span><span class="sxs-lookup"><span data-stu-id="9593b-162">Notice that the **Alarm History** table shows new alarms when the new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="9593b-163">További információ</span><span class="sxs-lookup"><span data-stu-id="9593b-163">Additional information</span></span>

<span data-ttu-id="9593b-164">Az operátor módosítása  **>**  bonyolultabb és túllép ebben az oktatóanyagban leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9593b-164">Changing the operator **>** is more complex and goes beyond the steps outlined in this tutorial.</span></span> <span data-ttu-id="9593b-165">Módosíthatja a Stream Analytics-feladat bármilyen tetszés operátor használata, jelezve, hogy a megoldás portálon operátor napjainkban összetettebb feladat.</span><span class="sxs-lookup"><span data-stu-id="9593b-165">While you can change the Stream Analytics job to use whatever operator you like, reflecting that operator in the solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9593b-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9593b-166">Next steps</span></span>
<span data-ttu-id="9593b-167">Most, hogy korábban már látott egyéni szabályok létrehozásához, többet is megtudhat az előkonfigurált megoldások:</span><span class="sxs-lookup"><span data-stu-id="9593b-167">Now that you've seen how to create custom rules, you can learn more about the preconfigured solutions:</span></span>

- <span data-ttu-id="9593b-168">[Logikai alkalmazás csatlakoztatása az Azure IoT Suite távoli megfigyelési előre konfigurált megoldás][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="9593b-168">[Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="9593b-169">[Eszköz információk metaadatait a távoli figyelési megoldást előre konfigurált][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="9593b-169">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md