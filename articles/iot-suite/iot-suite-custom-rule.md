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
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a>Hozzon létre egy egyéni szabály hello távoli felügyeleti előkonfigurált megoldás

## <a name="introduction"></a>Bevezetés

Előre konfigurált hello megoldásokat konfigurálhat [szabályokat, amelyek indul el, ha egy telemetriai érték, egy eszköz eléri a meghatározott][lnk-builtin-rule]. [Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello] [ lnk-dynamic-telemetry] ismerteti, hogyan értékeket is hozzáadhat egyéni telemetriai adatokat, például a *ExternalTemperature* tooyour megoldás. Ez a cikk bemutatja, hogyan toocreate egyéni szabály dinamikus telemetriai adatokat a típusokat a megoldásban.

Ez az oktatóanyag egy egyszerű Node.js szimulált eszköz toogenerate dinamikus telemetriai toosend toohello előkonfigurált megoldás háttérrendszerének használja. Majd hozzá az egyéni szabályok a hello **RemoteMonitoring** Visual Studio megoldás és a testreszabott háttér tooyour Azure-előfizetés telepítése.

toocomplete ebben az oktatóanyagban szüksége:

* Aktív Azure-előfizetés. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].
* [NODE.js] [ lnk-node] verzió 0.12.x vagy későbbi toocreate a szimulált eszköz.
* A Visual Studio 2015-öt vagy a Visual Studio 2017 toomodify előre konfigurált hello megoldás háttérrendszere végén az új szabályokat.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

Jegyezze fel az üzembe helyezéshez választott hello megoldás neve. Az oktatóanyag későbbi részében szüksége ezen megoldás neve.

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

Miután ellenőrizte, hogy akkor küld le lehet állítani hello Node.js-Konzolalkalmazás **ExternalTemperature** telemetriai toohello előre konfigurált megoldás. Hello konzolablak tartsa nyitva, mert a Node.js-Konzolalkalmazás hello egyéni szabály toohello megoldás hozzáadása után ismét futtatni.

## <a name="rule-storage-locations"></a>A szabály a tárolási helyek

Két külön helyen megőrződjenek szabályokkal kapcsolatos információkat:

* **DeviceRulesNormalizedTable** – Ez a táblázat a táblázat egy normalizált hello megoldás portál által definiált toohello szabályok hivatkoznak. Hello megoldás portal eszköz szabályokat jeleníti meg, amikor lekérdezi a következő táblázat hello szabálydefiníciók.
* **DeviceRules** blob – a blob tároló összes hello szabályai vannak megadva összes regisztrált eszközökre és a hivatkozás bemeneti toohello Azure Stream Analytics-feladatok típusúként van definiálva.
 
Meglévő szabály frissítésekor, vagy adjon meg egy új szabályt hello megoldás portal, hello tábla és a blob frissített tooreflect hello módosítások. hello portál megjeleníti-definíció hello szabály hello tábla tárolót és hello Stream Analytics-feladatok által hivatkozott definition származik hello blob hello szabály származik. 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a>Hello RemoteMonitoring Visual Studio megoldás frissítése

hello következő lépések bemutatják, hogyan toomodify hello RemoteMonitoring Visual Studio megoldás tooinclude hello használó új szabály **ExternalTemperature** hello szimulált eszközről küldött telemetriai:

1. Ha Ön még nem tette meg, klónozás hello **azure iot-távoli-ellenőrző** tárház tooa megfelelő helye a helyi gépen hello Git parancs a következő használatával:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. A Visual Studio, nyissa meg az hello RemoteMonitoring.sln fájl helyi másolatát hello **azure iot-távoli-ellenőrző** tárházba.

3. Nyissa meg Infrastructure\Models\DeviceRuleBlobEntity.cs hello fájlt, és adja hozzá egy **ExternalTemperature** tulajdonság az alábbiak szerint:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. A hello ugyanazt a fájlt, adja hozzá egy **ExternalTemperatureRuleOutput** tulajdonság az alábbiak szerint:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Nyissa meg a Infrastructure\Models\DeviceRuleDataFields.cs hello fájlt, és adja hozzá a következő hello **ExternalTemperature** után hello meglévő tulajdonság **páratartalom** tulajdonság:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. A hello ugyanazt a fájlt, a frissítéskezelés hello **_availableDataFields** metódus tooinclude **ExternalTemperature** az alábbiak szerint:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Nyissa meg a hello fájl Infrastructure\Repository\DeviceRulesRepository.cs, és módosítsa a hello **BuildBlobEntityListFromTableRows** módszert az alábbiak szerint:

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

## <a name="rebuild-and-redeploy-hello-solution"></a>Építse újra, és telepítse újra a hello megoldás.

Most már telepítheti a frissített hello megoldás tooyour Azure-előfizetés.

1. Nyisson meg egy rendszergazda jogú parancssort, és keresse meg a helyi példány hello azure iot-távoli-ellenőrző tárház toohello gyökérmappájában.

2. toodeploy a frissített megoldás, futtassa a következő parancs és hello **{telepítési neve}** hello nevű előre konfigurált megoldás a központi telepítés korábban feljegyzett:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a>Frissítés hello Stream Analytics-feladat

Ha hello telepítés befejeződött, hello Stream Analytics toouse hello új szabály feladatdefiníciók frissítheti.

1. Hello Azure-portálon keresse meg az előkonfigurált megoldás erőforrásokat tartalmazó toohello erőforráscsoportot. Ez az erőforráscsoport neve megegyezik a megadott hello van megoldás hello hello üzembe helyezése során.

2. Keresse meg a toohello {telepítési neve}-szabályok Stream Analytics-feladat. 

3. Kattintson a **leállítása** toostop hello Stream Analytics-feladat futtatását. (Meg kell várnia a folyamatos átviteli feladat toostop hello lekérdezés szerkesztése előtt hello).

4. Kattintson a **lekérdezés**. Hello lekérdezés tooinclude hello szerkesztése **válasszon** nyilatkozata **ExternalTemperature**. hello alábbi példa a teljes lekérdezés hello hello új **válasszon** utasítást:

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

5. Kattintson a **mentése** toochange hello szabály lekérdezés frissítése.

6. Kattintson a **Start** toostart hello Stream Analytics-feladat újra futtatni.

## <a name="add-your-new-rule-in-hello-dashboard"></a>Az új szabály hozzáadása a hello irányítópult

Mostantól hozzáadhatja azok hello **ExternalTemperature** szabály tooa eszköz hello megoldás irányítópulton.

1. Keresse meg a toohello megoldás portálon.

2. Keresse meg a toohello **eszközök** panel.

3. Keresse meg a hello egyéni eszköz létrehozott által küldött **ExternalTemperature** telemetriai adatok és a hello **. eszköz részletei** panelen, kattintson a **szabály hozzáadása**.

4. Válassza ki **ExternalTemperature** a **adatmező**.

5. Állítsa be **küszöbérték** too56. Kattintson a **mentéséhez és a szabályok megtekintése**.

6. Térjen vissza a toohello irányítópult tooview hello riasztás előzményeit.

7. Hello konzolablakban nyitva maradt, hello Node.js konzol app toobegin küldésének megkezdése **ExternalTemperature** telemetriai adatokat.

8. Figyelje meg, hogy hello **riasztási előzmények** táblázat új riasztások aktiválásáról hello új szabályt.
 
## <a name="additional-information"></a>További információ

Változó hello operátor  **>**  bonyolultabb és túllép hello lépéseit az oktatóanyag. Módosíthatja a Stream Analytics-feladat toouse hello bármilyen tetszés operátor, adott operátor hello megoldás portálon tükröző napjainkban összetettebb feladat. 

## <a name="next-steps"></a>Következő lépések
Most, hogy megtudhatta, hogyan toocreate egyéni szabályok, részletesebben előre konfigurált hello megoldásokról:

- [Csatlakozás a Logic App tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás][lnk-logic-app]
- [Eszköz információk metaadatok hello távoli figyelési megoldást előre konfigurált][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md