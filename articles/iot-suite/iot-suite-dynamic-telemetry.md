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
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="33e9a-103">Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello</span><span class="sxs-lookup"><span data-stu-id="33e9a-103">Use dynamic telemetry with hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="33e9a-104">Dinamikus telemetriai lehetővé teszi, hogy Ön toovisualize bármely távoli felügyeleti előkonfigurált megoldás küldött telemetriai toohello.</span><span class="sxs-lookup"><span data-stu-id="33e9a-104">Dynamic telemetry enables you toovisualize any telemetry sent toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="33e9a-105">Szimulált hello eszközök, amelyek az előre konfigurált hello megoldással telepítése telemetriát hőmérséklet és a páratartalom, amely hello irányítópulton jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="33e9a-105">hello simulated devices that deploy with hello preconfigured solution send temperature and humidity telemetry, which you can visualize on hello dashboard.</span></span> <span data-ttu-id="33e9a-106">Ha meglévő szimulált eszköz szabja testre, hozzon létre új szimulált eszköz, vagy csatlakoztassa a fizikai eszközök előre konfigurált toohello megoldás például hello külső hőmérséklet, RPM vagy szélsebesség telemetriai értékekhez küldhet.</span><span class="sxs-lookup"><span data-stu-id="33e9a-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices toohello preconfigured solution you can send other telemetry values such as hello external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="33e9a-107">Majd jelenítheti meg a további hello irányítópult telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="33e9a-107">You can then visualize this additional telemetry on hello dashboard.</span></span>

<span data-ttu-id="33e9a-108">Ebben az oktatóanyagban egy egyszerű Node.js szimulált-eszközt használ., hogy a dinamikus telemetriai tooexperiment könnyen módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="33e9a-108">This tutorial uses a simple Node.js simulated device that you can easily modify tooexperiment with dynamic telemetry.</span></span>

<span data-ttu-id="33e9a-109">toocomplete ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="33e9a-109">toocomplete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="33e9a-110">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="33e9a-110">An active Azure subscription.</span></span> <span data-ttu-id="33e9a-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="33e9a-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="33e9a-112">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="33e9a-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="33e9a-113">[NODE.js] [ lnk-node] verzió 0.12.x vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="33e9a-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="33e9a-114">Az operációs rendszereken, például a Windows vagy Linux, ahol telepítheti a Node.js oktatóanyag hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="33e9a-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="33e9a-115">A telemetria-típus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="33e9a-115">Add a telemetry type</span></span>

<span data-ttu-id="33e9a-116">következő lépés hello tooreplace hello telemetriai hello Node.js szimulált eszköz egy új értékhalmazt állítja elő:</span><span class="sxs-lookup"><span data-stu-id="33e9a-116">hello next step is tooreplace hello telemetry generated by hello Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="33e9a-117">Állítsa le hello Node.js szimulált eszköz beírásával **Ctrl + C** a parancssor vagy a rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="33e9a-117">Stop hello Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="33e9a-118">Hello remote_monitoring.js fájl, a hello alapadatokhoz értékek hello meglévő hőmérséklet, páratartalom és külső hőmérséklet telemetriai látható.</span><span class="sxs-lookup"><span data-stu-id="33e9a-118">In hello remote_monitoring.js file, you can see hello base data values for hello existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="33e9a-119">Adja meg az alapadatokhoz értékét **rpm** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="33e9a-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="33e9a-120">hello Node.js szimulált eszköz által használt hello **generateRandomIncrement** alapadatokhoz értékek hello remote_monitoring.js fájl tooadd egy véletlenszerű növekmény toohello működni.</span><span class="sxs-lookup"><span data-stu-id="33e9a-120">hello Node.js simulated device uses hello **generateRandomIncrement** function in hello remote_monitoring.js file tooadd a random increment toohello base data values.</span></span> <span data-ttu-id="33e9a-121">Ügyfélfuttatási hello **rpm** érték hozzáadásával egy kódsort hello meglévő randomizations után az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="33e9a-121">Randomize hello **rpm** value by adding a line of code after hello existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="33e9a-122">Hello új rpm érték toohello JSON hasznos hello eszköz küld tooIoT Hub hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="33e9a-122">Add hello new rpm value toohello JSON payload hello device sends tooIoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="33e9a-123">Futtassa a hello Node.js szimulált eszköz hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="33e9a-123">Run hello Node.js simulated device using hello following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="33e9a-124">Hello új RPM telemetriai típus fog megjelenni a diagram hello hello irányítópult láthatja:</span><span class="sxs-lookup"><span data-stu-id="33e9a-124">Observe hello new RPM telemetry type that displays on hello chart in hello dashboard:</span></span>

![Adja hozzá a RPM toohello irányítópult][image3]

> [!NOTE]
> <span data-ttu-id="33e9a-126">Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.</span><span class="sxs-lookup"><span data-stu-id="33e9a-126">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="customize-hello-dashboard-display"></a><span data-ttu-id="33e9a-127">Hello irányítópult megjelenítéséhez</span><span class="sxs-lookup"><span data-stu-id="33e9a-127">Customize hello dashboard display</span></span>

<span data-ttu-id="33e9a-128">Hello **eszközinformáció** üzenetet tartalmazhatnak metaadatok kapcsolatos hello telemetriai hello eszköz tooIoT Hub küldhet.</span><span class="sxs-lookup"><span data-stu-id="33e9a-128">hello **Device-Info** message can include metadata about hello telemetry hello device can send tooIoT Hub.</span></span> <span data-ttu-id="33e9a-129">A metaadatok hello eszköz küldi hello telemetriai típusokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="33e9a-129">This metadata can specify hello telemetry types hello device sends.</span></span> <span data-ttu-id="33e9a-130">Hello módosítása **deviceMetaData** hello remote_monitoring.js fájl tooinclude értéket egy **Telemetriai** definícióját a következő hello **parancsok** definíciója.</span><span class="sxs-lookup"><span data-stu-id="33e9a-130">Modify hello **deviceMetaData** value in hello remote_monitoring.js file tooinclude a **Telemetry** definition following hello **Commands** definition.</span></span> <span data-ttu-id="33e9a-131">hello következő kódrészletet látható hello **parancsok** definition (lehet, hogy tooadd egy `,` után hello **parancsok** definition):</span><span class="sxs-lookup"><span data-stu-id="33e9a-131">hello following code snippet shows hello **Commands** definition (be sure tooadd a `,` after hello **Commands** definition):</span></span>

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
> <span data-ttu-id="33e9a-132">hello távoli felügyeleti megoldás nem toocompare hello metaadat-definíciójában hello telemetriai adatfolyam adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="33e9a-132">hello remote monitoring solution uses a case-insensitive match toocompare hello metadata definition with data in hello telemetry stream.</span></span>


<span data-ttu-id="33e9a-133">Hozzáadása egy **Telemetriai** definícióját, ahogy az előző hello kódrészletet nem változtatja meg a hello irányítópult hello viselkedését.</span><span class="sxs-lookup"><span data-stu-id="33e9a-133">Adding a **Telemetry** definition as shown in hello preceding code snippet does not change hello behavior of hello dashboard.</span></span> <span data-ttu-id="33e9a-134">Azonban hello metaadatok is lehetnek egy **DisplayName** toocustomize hello megjelenítési hello irányítópulton attribútum.</span><span class="sxs-lookup"><span data-stu-id="33e9a-134">However, hello metadata can also include a **DisplayName** attribute toocustomize hello display in hello dashboard.</span></span> <span data-ttu-id="33e9a-135">Frissítés hello **Telemetriai** metaadat-definíciójában, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="33e9a-135">Update hello **Telemetry** metadata definition as shown in hello following snippet:</span></span>

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

<span data-ttu-id="33e9a-136">hello alábbi képernyőfelvételen látható hogyan módosítja a ezt a módosítást a hello diagram jelmagyarázatának hello irányítópult:</span><span class="sxs-lookup"><span data-stu-id="33e9a-136">hello following screenshot shows how this change modifies hello chart legend on hello dashboard:</span></span>

![Diagram jelmagyarázatának hello testreszabása][image4]

> [!NOTE]
> <span data-ttu-id="33e9a-138">Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.</span><span class="sxs-lookup"><span data-stu-id="33e9a-138">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="filter-hello-telemetry-types"></a><span data-ttu-id="33e9a-139">Hello telemetriai típusok szűrése</span><span class="sxs-lookup"><span data-stu-id="33e9a-139">Filter hello telemetry types</span></span>

<span data-ttu-id="33e9a-140">Alapértelmezés szerint hello hello irányítópult minden adatsort ábrázolja hello telemetriai adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="33e9a-140">By default, hello chart on hello dashboard shows every data series in hello telemetry stream.</span></span> <span data-ttu-id="33e9a-141">Használhatja a hello **eszközinformáció** metaadatok toosuppress hello adott telemetriai hello diagram megjelenítését.</span><span class="sxs-lookup"><span data-stu-id="33e9a-141">You can use hello **Device-Info** metadata toosuppress hello display of specific telemetry types on hello chart.</span></span> 

<span data-ttu-id="33e9a-142">toomake hello diagram megjelenítése csak hőmérséklet és a páratartalom telemetriai, hagyja el a **ExternalTemperature** a hello **eszközinformáció** **Telemetriai** metaadat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="33e9a-142">toomake hello chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from hello **Device-Info** **Telemetry** metadata as follows:</span></span>

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

<span data-ttu-id="33e9a-143">Hello **külső hőmérséklet** hello diagramon már nem jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="33e9a-143">hello **Outdoor Temperature** no longer displays on hello chart:</span></span>

![Szűrő hello telemetriai adatokat hello irányítópult][image5]

<span data-ttu-id="33e9a-145">Ez a módosítás csak hello diagram megjelenítési érinti.</span><span class="sxs-lookup"><span data-stu-id="33e9a-145">This change only affects hello chart display.</span></span> <span data-ttu-id="33e9a-146">Hello **ExternalTemperature** adatértékek továbbra is tárolja, a háttérbeli feldolgozás érhető el.</span><span class="sxs-lookup"><span data-stu-id="33e9a-146">hello **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="33e9a-147">Előfordulhat, hogy toodisable kell, és engedélyeznie kell a Node.js-eszközön hello hello **eszközök** hello irányítópult toosee hello változás azonnal lapján.</span><span class="sxs-lookup"><span data-stu-id="33e9a-147">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="33e9a-148">Hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="33e9a-148">Handle errors</span></span>

<span data-ttu-id="33e9a-149">Egy data stream toodisplay hello diagramon, a a **típus** a hello **eszközinformáció** metaadatok hello telemetriai értékek adattípusa hello egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="33e9a-149">For a data stream toodisplay on hello chart, its **Type** in hello **Device-Info** metadata must match hello data type of hello telemetry values.</span></span> <span data-ttu-id="33e9a-150">Például, ha a hello metaadatok határozza meg, hogy hello **típus** páratartalom adatok az **int** és egy **dupla** hello telemetriai adatfolyam megtalálható, majd hello páratartalom telemetriai does hello diagramon nem jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="33e9a-150">For example, if hello metadata specifies that hello **Type** of humidity data is **int** and a **double** is found in hello telemetry stream then hello humidity telemetry does not display on hello chart.</span></span> <span data-ttu-id="33e9a-151">Azonban hello **páratartalom** értékek továbbra is tárolja, a háttér-feldolgozási érhető el.</span><span class="sxs-lookup"><span data-stu-id="33e9a-151">However, hello **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33e9a-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33e9a-152">Next steps</span></span>

<span data-ttu-id="33e9a-153">Most, hogy megtudhatta, hogyan toouse dinamikus telemetriai, áttekintheti, hogyan hello előre konfigurált megoldásokkal kapcsolatos használja, eszköz adatai: [eszköz információk metaadatok hello távoli figyelési megoldást előre konfigurált] [ lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="33e9a-153">Now that you've seen how toouse dynamic telemetry, you can learn more about how hello preconfigured solutions use device information: [Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
