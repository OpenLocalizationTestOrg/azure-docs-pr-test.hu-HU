---
title: dinamikus telemetriai aaaUse |} Microsoft Docs
description: "Hajtsa végre az ezen oktatóanyag toolearn hogyan toouse dinamikus telemetriai hello Azure IoT Suite távoli figyelési előre konfigurált megoldás."
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
ms.openlocfilehash: 06cb2a370b67b4950efdfa4c7d906ac92106f4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a>Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello

Dinamikus telemetriai lehetővé teszi, hogy Ön toovisualize bármely távoli felügyeleti előkonfigurált megoldás küldött telemetriai toohello. Szimulált hello eszközök, amelyek az előre konfigurált hello megoldással telepítése telemetriát hőmérséklet és a páratartalom, amely hello irányítópulton jelenítheti meg. Ha meglévő szimulált eszköz szabja testre, hozzon létre új szimulált eszköz, vagy csatlakoztassa a fizikai eszközök előre konfigurált toohello megoldás például hello külső hőmérséklet, RPM vagy szélsebesség telemetriai értékekhez küldhet. Majd jelenítheti meg a további hello irányítópult telemetriai adatokat.

Ebben az oktatóanyagban egy egyszerű Node.js szimulált-eszközt használ., hogy a dinamikus telemetriai tooexperiment könnyen módosíthatja.

toocomplete ebben az oktatóanyagban lesz szüksége:

* Aktív Azure-előfizetés. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].
* [NODE.js] [ lnk-node] verzió 0.12.x vagy újabb.

Az operációs rendszereken, például a Windows vagy Linux, ahol telepítheti a Node.js oktatóanyag hajthatja végre.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>A telemetria-típus hozzáadása

következő lépés hello tooreplace hello telemetriai hello Node.js szimulált eszköz egy új értékhalmazt állítja elő:

1. Állítsa le hello Node.js szimulált eszköz beírásával **Ctrl + C** a parancssor vagy a rendszerhéj.
2. Hello remote_monitoring.js fájl, a hello alapadatokhoz értékek hello meglévő hőmérséklet, páratartalom és külső hőmérséklet telemetriai látható. Adja meg az alapadatokhoz értékét **rpm** az alábbiak szerint:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. hello Node.js szimulált eszköz által használt hello **generateRandomIncrement** alapadatokhoz értékek hello remote_monitoring.js fájl tooadd egy véletlenszerű növekmény toohello működni. Ügyfélfuttatási hello **rpm** érték hozzáadásával egy kódsort hello meglévő randomizations után az alábbiak szerint:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Hello új rpm érték toohello JSON hasznos hello eszköz küld tooIoT Hub hozzáadása:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Futtassa a hello Node.js szimulált eszköz hello a következő parancs használatával:

    `node remote_monitoring.js`

6. Hello új RPM telemetriai típus fog megjelenni a diagram hello hello irányítópult láthatja:

![Adja hozzá a RPM toohello irányítópult][image3]

> [!NOTE]
> Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.

## <a name="customize-hello-dashboard-display"></a>Hello irányítópult megjelenítéséhez

Hello **eszközinformáció** üzenetet tartalmazhatnak metaadatok kapcsolatos hello telemetriai hello eszköz tooIoT Hub küldhet. A metaadatok hello eszköz küldi hello telemetriai típusokat adhat meg. Hello módosítása **deviceMetaData** hello remote_monitoring.js fájl tooinclude értéket egy **Telemetriai** definícióját a következő hello **parancsok** definíciója. hello következő kódrészletet látható hello **parancsok** definition (lehet, hogy tooadd egy `,` után hello **parancsok** definition):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> hello távoli felügyeleti megoldás nem toocompare hello metaadat-definíciójában hello telemetriai adatfolyam adatokat használ.


Hozzáadása egy **Telemetriai** definícióját, ahogy az előző hello kódrészletet nem változtatja meg a hello irányítópult hello viselkedését. Azonban hello metaadatok is lehetnek egy **DisplayName** toocustomize hello megjelenítési hello irányítópulton attribútum. Frissítés hello **Telemetriai** metaadat-definíciójában, ahogy az alábbi részlet hello:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

hello alábbi képernyőfelvételen látható hogyan módosítja a ezt a módosítást a hello diagram jelmagyarázatának hello irányítópult:

![Diagram jelmagyarázatának hello testreszabása][image4]

> [!NOTE]
> Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.

## <a name="filter-hello-telemetry-types"></a>Hello telemetriai típusok szűrése

Alapértelmezés szerint hello hello irányítópult minden adatsort ábrázolja hello telemetriai adatfolyamban. Használhatja a hello **eszközinformáció** metaadatok toosuppress hello adott telemetriai hello diagram megjelenítését. 

toomake hello diagram megjelenítése csak hőmérséklet és a páratartalom telemetriai, hagyja el a **ExternalTemperature** a hello **eszközinformáció** **Telemetriai** metaadat az alábbiak szerint:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Hello **külső hőmérséklet** hello diagramon már nem jelenik meg:

![Szűrő hello telemetriai adatokat hello irányítópult][image5]

Ez a módosítás csak hello diagram megjelenítési érinti. Hello **ExternalTemperature** adatértékek továbbra is tárolja, a háttérbeli feldolgozás érhető el.

> [!NOTE]
> Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.

## <a name="handle-errors"></a>Hibák kezelésének

Egy data stream toodisplay hello diagramon, a a **típus** a hello **eszközinformáció** metaadatok hello telemetriai értékek adattípusa hello egyeznie kell. Például, ha a hello metaadatok határozza meg, hogy hello **típus** páratartalom adatok az **int** és egy **dupla** hello telemetriai adatfolyam megtalálható, majd hello páratartalom telemetriai does hello diagramon nem jeleníti meg. Azonban hello **páratartalom** értékek továbbra is tárolja, a háttér-feldolgozási érhető el.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtudhatta, hogyan toouse dinamikus telemetriai, áttekintheti, hogyan hello előre konfigurált megoldásokkal kapcsolatos használja, eszköz adatai: [eszköz információk metaadatok hello távoli figyelési megoldást előre konfigurált] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
