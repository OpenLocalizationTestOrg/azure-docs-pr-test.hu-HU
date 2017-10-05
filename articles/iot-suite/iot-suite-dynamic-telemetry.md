---
title: "Használjon dinamikus telemetriai |} Microsoft Docs"
description: "Ez az oktatóanyag megtudhatja, hogyan használható az Azure IoT Suite távoli felügyeleti előkonfigurált megoldás dinamikus telemetriai."
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
ms.openlocfilehash: 0114f27f9b8ae76e1170d04ddf66e2c4bf20686a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="721d2-103">Dinamikus telemetriai adatokat a távoli felügyeleti előkonfigurált megoldás</span><span class="sxs-lookup"><span data-stu-id="721d2-103">Use dynamic telemetry with the remote monitoring preconfigured solution</span></span>

<span data-ttu-id="721d2-104">Dinamikus telemetriai lehetővé teszi bármely telemetriai adatokat küldött a távoli felügyeleti előkonfigurált megoldás megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="721d2-104">Dynamic telemetry enables you to visualize any telemetry sent to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="721d2-105">A szimulált eszköz, amely az előkonfigurált megoldás üzembe helyezéséhez telemetriát hőmérséklet és a páratartalom, amely az irányítópulton jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="721d2-105">The simulated devices that deploy with the preconfigured solution send temperature and humidity telemetry, which you can visualize on the dashboard.</span></span> <span data-ttu-id="721d2-106">Ha meglévő szimulált eszközök testreszabása, hozzon létre új szimulált eszköz, vagy fizikai eszközök csatlakoztatása az előkonfigurált megoldás az egyéb telemetriai értékek, például a külső hőmérséklet, RPM vagy szélsebesség küldhet.</span><span class="sxs-lookup"><span data-stu-id="721d2-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices to the preconfigured solution you can send other telemetry values such as the external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="721d2-107">Majd jelenítheti meg az irányítópulton a további telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="721d2-107">You can then visualize this additional telemetry on the dashboard.</span></span>

<span data-ttu-id="721d2-108">Ez az oktatóanyag egy egyszerű Node.js szimulált eszköz könnyen módosíthatja dinamikus telemetriai kísérletezhet használja.</span><span class="sxs-lookup"><span data-stu-id="721d2-108">This tutorial uses a simple Node.js simulated device that you can easily modify to experiment with dynamic telemetry.</span></span>

<span data-ttu-id="721d2-109">Az oktatóanyag elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="721d2-109">To complete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="721d2-110">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="721d2-110">An active Azure subscription.</span></span> <span data-ttu-id="721d2-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="721d2-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="721d2-112">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="721d2-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="721d2-113">[NODE.js] [ lnk-node] verzió 0.12.x vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="721d2-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="721d2-114">Az operációs rendszereken, például a Windows vagy Linux, ahol telepítheti a Node.js oktatóanyag hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="721d2-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="721d2-115">A telemetria-típus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="721d2-115">Add a telemetry type</span></span>

<span data-ttu-id="721d2-116">A következő lépés, hogy a telemetriai adatok új értékek vannak beállítva a Node.js szimulált eszköz állítja elő:</span><span class="sxs-lookup"><span data-stu-id="721d2-116">The next step is to replace the telemetry generated by the Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="721d2-117">A Node.js szimulált eszköz leállításához írja be a **Ctrl + C** a parancssor vagy a rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="721d2-117">Stop the Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="721d2-118">A remote_monitoring.js fájlban megtekintheti a meglévő hőmérséklet, a páratartalom és a külső hőmérséklet telemetriai alapadatokhoz értékeit.</span><span class="sxs-lookup"><span data-stu-id="721d2-118">In the remote_monitoring.js file, you can see the base data values for the existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="721d2-119">Adja meg az alapadatokhoz értékét **rpm** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="721d2-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="721d2-120">A Node.js szimulált eszköz használ a **generateRandomIncrement** függvény véletlenszerű növekmény hozzáadása alapszintű adatértékek remote_monitoring.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="721d2-120">The Node.js simulated device uses the **generateRandomIncrement** function in the remote_monitoring.js file to add a random increment to the base data values.</span></span> <span data-ttu-id="721d2-121">Ügyfélfuttatási a **rpm** érték hozzáadásával egy kódsort a meglévő randomizations után az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="721d2-121">Randomize the **rpm** value by adding a line of code after the existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="721d2-122">Az eszköz küld az IoT-központ a JSON-adattartalmat az új rpm érték hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="721d2-122">Add the new rpm value to the JSON payload the device sends to IoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="721d2-123">Futtassa a szimulált eszköz Node.js, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="721d2-123">Run the Node.js simulated device using the following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="721d2-124">Az új RPM telemetriai típus fog megjelenni a diagram az irányítópulton a láthatja:</span><span class="sxs-lookup"><span data-stu-id="721d2-124">Observe the new RPM telemetry type that displays on the chart in the dashboard:</span></span>

![Az irányítópult RPM hozzáadása][image3]

> [!NOTE]
> <span data-ttu-id="721d2-126">Tiltsa le, majd engedélyezze a a Node.js-eszköz szükség a **eszközök** lap állapotúként jelenik meg azonnal az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="721d2-126">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="customize-the-dashboard-display"></a><span data-ttu-id="721d2-127">Az irányítópult megjelenítéséhez</span><span class="sxs-lookup"><span data-stu-id="721d2-127">Customize the dashboard display</span></span>

<span data-ttu-id="721d2-128">A **eszközinformáció** hibaüzenet is tartalmazza az eszköz küldhet az IoT-központ telemetriai adatok metaadatait.</span><span class="sxs-lookup"><span data-stu-id="721d2-128">The **Device-Info** message can include metadata about the telemetry the device can send to IoT Hub.</span></span> <span data-ttu-id="721d2-129">A metaadatok lehet meghatározni az telemetriai adatokat küld az eszközre.</span><span class="sxs-lookup"><span data-stu-id="721d2-129">This metadata can specify the telemetry types the device sends.</span></span> <span data-ttu-id="721d2-130">Módosítsa a **deviceMetaData** felvenni a remote_monitoring.js fájlban egy **Telemetriai** definíció alábbi a **parancsok** definition.</span><span class="sxs-lookup"><span data-stu-id="721d2-130">Modify the **deviceMetaData** value in the remote_monitoring.js file to include a **Telemetry** definition following the **Commands** definition.</span></span> <span data-ttu-id="721d2-131">A következő kódrészletben látható kódot a **parancsok** definition (ne feledje hozzáadni egy `,` után a **parancsok** definition):</span><span class="sxs-lookup"><span data-stu-id="721d2-131">The following code snippet shows the **Commands** definition (be sure to add a `,` after the **Commands** definition):</span></span>

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
> <span data-ttu-id="721d2-132">A távoli felügyeleti megoldás hasonlítsa össze a metaadat-definíciójában a telemetriai adatok adatfolyam adatokat nem használja.</span><span class="sxs-lookup"><span data-stu-id="721d2-132">The remote monitoring solution uses a case-insensitive match to compare the metadata definition with data in the telemetry stream.</span></span>


<span data-ttu-id="721d2-133">Hozzáadása egy **Telemetriai** definícióját, ahogy az előző kódrészletet az nem változtatja meg az irányítópult viselkedését.</span><span class="sxs-lookup"><span data-stu-id="721d2-133">Adding a **Telemetry** definition as shown in the preceding code snippet does not change the behavior of the dashboard.</span></span> <span data-ttu-id="721d2-134">Azonban a metaadatok is lehetnek egy **DisplayName** attribútum az irányítópult megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="721d2-134">However, the metadata can also include a **DisplayName** attribute to customize the display in the dashboard.</span></span> <span data-ttu-id="721d2-135">Frissítés a **Telemetriai** metaadat-definíciójában, ahogy az a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="721d2-135">Update the **Telemetry** metadata definition as shown in the following snippet:</span></span>

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

<span data-ttu-id="721d2-136">Az alábbi képernyőfelvételen látható, hogyan Ez a változás módosítja az irányítópulton a diagram jelmagyarázatának:</span><span class="sxs-lookup"><span data-stu-id="721d2-136">The following screenshot shows how this change modifies the chart legend on the dashboard:</span></span>

![A diagram jelmagyarázatának testreszabása][image4]

> [!NOTE]
> <span data-ttu-id="721d2-138">Tiltsa le, majd engedélyezze a a Node.js-eszköz szükség a **eszközök** lap állapotúként jelenik meg azonnal az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="721d2-138">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="filter-the-telemetry-types"></a><span data-ttu-id="721d2-139">A telemetria-típusok szűrése</span><span class="sxs-lookup"><span data-stu-id="721d2-139">Filter the telemetry types</span></span>

<span data-ttu-id="721d2-140">Alapértelmezés szerint a diagram az irányítópulton látható minden adatsort a telemetria-adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="721d2-140">By default, the chart on the dashboard shows every data series in the telemetry stream.</span></span> <span data-ttu-id="721d2-141">Használhatja a **eszközinformáció** metaadatok az adott telemetriai típusok a diagram a megjelenítését.</span><span class="sxs-lookup"><span data-stu-id="721d2-141">You can use the **Device-Info** metadata to suppress the display of specific telemetry types on the chart.</span></span> 

<span data-ttu-id="721d2-142">Ahhoz, hogy a diagram megjelenítése csak a hőmérséklet és a páratartalom telemetriai, hagyja el **ExternalTemperature** a a **eszközinformáció** **Telemetriai** metaadat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="721d2-142">To make the chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from the **Device-Info** **Telemetry** metadata as follows:</span></span>

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

<span data-ttu-id="721d2-143">A **külső hőmérséklet** a diagram már nem jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="721d2-143">The **Outdoor Temperature** no longer displays on the chart:</span></span>

![Az irányítópult telemetriai adatok szűrése][image5]

<span data-ttu-id="721d2-145">Ez a módosítás csak a diagram megjelenítési érinti.</span><span class="sxs-lookup"><span data-stu-id="721d2-145">This change only affects the chart display.</span></span> <span data-ttu-id="721d2-146">A **ExternalTemperature** adatértékek továbbra is tárolja, a háttérbeli feldolgozás érhető el.</span><span class="sxs-lookup"><span data-stu-id="721d2-146">The **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="721d2-147">Tiltsa le, majd engedélyezze a a Node.js-eszköz szükség a **eszközök** lap állapotúként jelenik meg azonnal az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="721d2-147">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="721d2-148">Hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="721d2-148">Handle errors</span></span>

<span data-ttu-id="721d2-149">A diagramon megjelenítendő egy adatfolyam a **típus** a a **eszközinformáció** metaadatok meg kell egyeznie a telemetriai adatok értékeinek adatok típusát.</span><span class="sxs-lookup"><span data-stu-id="721d2-149">For a data stream to display on the chart, its **Type** in the **Device-Info** metadata must match the data type of the telemetry values.</span></span> <span data-ttu-id="721d2-150">Például, ha a metaadatok azt jelenti, hogy a **típus** páratartalom adatok az **int** és egy **dupla** páratartalom telemetriai adatok nem jelennek meg a diagramra, majd a telemetriai adatok adatfolyamban található.</span><span class="sxs-lookup"><span data-stu-id="721d2-150">For example, if the metadata specifies that the **Type** of humidity data is **int** and a **double** is found in the telemetry stream then the humidity telemetry does not display on the chart.</span></span> <span data-ttu-id="721d2-151">Azonban a **páratartalom** értékek továbbra is tárolja, a háttér-feldolgozási érhető el.</span><span class="sxs-lookup"><span data-stu-id="721d2-151">However, the **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="721d2-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="721d2-152">Next steps</span></span>

<span data-ttu-id="721d2-153">Most, hogy korábban már látott dinamikus telemetriai használata, többet is megtudhat arról, hogyan az előkonfigurált megoldásokat eszköz adatai: [eszköz információk metaadatait a távoli figyelési megoldást előre konfigurált][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="721d2-153">Now that you've seen how to use dynamic telemetry, you can learn more about how the preconfigured solutions use device information: [Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
