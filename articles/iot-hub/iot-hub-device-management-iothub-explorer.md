---
title: "Az Azure IoT-eszközök kezelése a IOT hubbal-explorer |} Microsoft Docs"
description: "Eszközzel az IOT hubbal-explorer CLI Azure IoT Hub kezeléséhez, ha kiemeli a közvetlen módszerek és a kettős kívánt tulajdonságok felügyeleti lehetőségeket."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot kezelés, az azure iot hub – eszközkezelés, eszköz felügyeleti iot, iot hub – Eszközkezelés"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: 5b7a5057bdfb5920fbb5759bed1f5561cfa1d7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="cc41a-104">Használja az IOT hubbal-explorer Azure IoT Hub – Eszközkezelés</span><span class="sxs-lookup"><span data-stu-id="cc41a-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="cc41a-106">[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) parancssori eszköz, amely a gazdagépen futtat az IoT hub beállításjegyzék eszköz identitásainak kezelése a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="cc41a-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer to manage device identities in your IoT hub registry.</span></span> <span data-ttu-id="cc41a-107">Az ismét felügyeleti beállításokkal rendelkezik, amelyek segítségével különböző feladatok végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="cc41a-107">It comes with management options that you can use to perform various tasks.</span></span>

| <span data-ttu-id="cc41a-108">Lehetőséget</span><span class="sxs-lookup"><span data-stu-id="cc41a-108">Management option</span></span>          | <span data-ttu-id="cc41a-109">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc41a-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cc41a-110">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="cc41a-110">Direct methods</span></span>             | <span data-ttu-id="cc41a-111">Egy eszköz, például indítása és leállítása üzenetküldésre, vagy az eszköz újraindul működésre tétele.</span><span class="sxs-lookup"><span data-stu-id="cc41a-111">Make a device act such as starting or stopping sending messages or rebooting the device.</span></span>                                        |
| <span data-ttu-id="cc41a-112">A két szükségeskonfiguráció-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="cc41a-112">Twin desired properties</span></span>    | <span data-ttu-id="cc41a-113">Egy eszköz üzembe egyes állapotok, például egy LED zöld vagy a telemetriai adatok küldési időköze és 30 perc.</span><span class="sxs-lookup"><span data-stu-id="cc41a-113">Put a device into certain states, such as setting an LED to green or setting the telemetry send interval to 30 minutes.</span></span>         |
| <span data-ttu-id="cc41a-114">A két jelentett tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="cc41a-114">Twin reported properties</span></span>   | <span data-ttu-id="cc41a-115">Egy eszköz jelentett állapotának beolvasására.</span><span class="sxs-lookup"><span data-stu-id="cc41a-115">Get the reported state of a device.</span></span> <span data-ttu-id="cc41a-116">Például az eszköz jelentés készít a LED most villogó-e.</span><span class="sxs-lookup"><span data-stu-id="cc41a-116">For example, the device reports the LED is blinking now.</span></span>                                    |
| <span data-ttu-id="cc41a-117">A két címkék</span><span class="sxs-lookup"><span data-stu-id="cc41a-117">Twin tags</span></span>                  | <span data-ttu-id="cc41a-118">A felhőben tárolt eszközre vonatkozó metaadatok.</span><span class="sxs-lookup"><span data-stu-id="cc41a-118">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="cc41a-119">Például a központi telepítési helye a Eladóautomata.</span><span class="sxs-lookup"><span data-stu-id="cc41a-119">For example, the deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="cc41a-120">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc41a-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="cc41a-121">Értesítések küldése eszközre.</span><span class="sxs-lookup"><span data-stu-id="cc41a-121">Send notifications to a device.</span></span> <span data-ttu-id="cc41a-122">Például "nagyon valószínű ma eső számára.</span><span class="sxs-lookup"><span data-stu-id="cc41a-122">For example, "It is very likely to rain today.</span></span> <span data-ttu-id="cc41a-123">Ne feledje az összevonó kapcsolja."</span><span class="sxs-lookup"><span data-stu-id="cc41a-123">Don't forget to bring an umbrella."</span></span>              |
| <span data-ttu-id="cc41a-124">Eszköz iker lekérdezések</span><span class="sxs-lookup"><span data-stu-id="cc41a-124">Device twin queries</span></span>        | <span data-ttu-id="cc41a-125">A lekérdezés az összes eszköz twins rendelkező tetszőleges feltételek, például az eszközök, amelyek használható azonosító beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cc41a-125">Query all device twins to retrieve those with arbitrary conditions, such as identifying the devices that are available for use.</span></span> |

<span data-ttu-id="cc41a-126">A különbségek a Magyarázat és útmutatást ezen beállítások használatával, lásd: [eszközről a felhőbe kommunikációs útmutatást](iot-hub-devguide-d2c-guidance.md) és [felhő eszközre kommunikációs útmutatást](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="cc41a-126">For more detailed explanation on the differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cc41a-127">Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják.</span><span class="sxs-lookup"><span data-stu-id="cc41a-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="cc41a-128">Az IoT-központ továbbra is fennáll, egy eszköz iker az egyes eszközök ahhoz csatlakozó ügyfélnél.</span><span class="sxs-lookup"><span data-stu-id="cc41a-128">IoT Hub persists a device twin for each device that connects to it.</span></span> <span data-ttu-id="cc41a-129">Eszköz twins kapcsolatos további információkért lásd: [Ismerkedés az eszköz twins](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="cc41a-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="cc41a-130">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="cc41a-130">What you learn</span></span>

<span data-ttu-id="cc41a-131">Megismerheti a fejlesztői gépen különböző kezelési lehetőségek az IOT hubbal-Intéző segítségével.</span><span class="sxs-lookup"><span data-stu-id="cc41a-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="cc41a-132">Mit</span><span class="sxs-lookup"><span data-stu-id="cc41a-132">What you do</span></span>

<span data-ttu-id="cc41a-133">IOT hubbal-kezelő futtatása a különböző felügyeleti beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="cc41a-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cc41a-134">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="cc41a-134">What you need</span></span>

- <span data-ttu-id="cc41a-135">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="cc41a-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="cc41a-136">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="cc41a-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="cc41a-137">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cc41a-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="cc41a-138">Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cc41a-138">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="cc41a-139">Győződjön meg arról, hogy az eszköz az ügyfélalkalmazás az oktatóanyag során fut-e.</span><span class="sxs-lookup"><span data-stu-id="cc41a-139">Make sure your device is running with the client application during this tutorial.</span></span>
- <span data-ttu-id="cc41a-140">IOT hubbal-Explorerben [telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer) a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="cc41a-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-to-your-iot-hub"></a><span data-ttu-id="cc41a-141">Az IoT hub kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="cc41a-141">Connect to your IoT hub</span></span>

<span data-ttu-id="cc41a-142">Az IoT hub csatlakoztatása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-142">Connect to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="cc41a-143">Használja az IOT hubbal-explorer közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="cc41a-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="cc41a-144">Hívása a `start` módszer használatát az eszköz alkalmazásának üzeneteket küldhet az IoT hub a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-144">Invoke the `start` method in the device app to send messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="cc41a-145">Hívása a `stop` módszer az eszköz alkalmazásban való küldésének leállításához az IoT hub üzenetek a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-145">Invoke the `stop` method in the device app to stop sending messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="cc41a-146">Használja az IOT hubbal-explorer iker a kívánt tulajdonságokkal</span><span class="sxs-lookup"><span data-stu-id="cc41a-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="cc41a-147">Állítsa be a kívánt tulajdonság intervallumát 3000 = a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-147">Set a desired property interval = 3000 by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="cc41a-148">Ez a tulajdonság az eszköz által is olvasható.</span><span class="sxs-lookup"><span data-stu-id="cc41a-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="cc41a-149">Használja az IOT hubbal-explorer iker által jelentett tulajdonságokkal</span><span class="sxs-lookup"><span data-stu-id="cc41a-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="cc41a-150">Az eszköz jelentett tulajdonságait olvassa be a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-150">Get the reported properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="cc41a-151">A tulajdonságok egyike $metadata. az eszköz megjelenítheti az utolsó $lastUpdated küld vagy üzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="cc41a-151">One of the properties is $metadata.$lastUpdated which shows the last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="cc41a-152">-Kezelővel IOT hubbal-iker tartozó címkékkel</span><span class="sxs-lookup"><span data-stu-id="cc41a-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="cc41a-153">A címkéket és az eszköz Tulajdonságok megjelenítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-153">Display the tags and properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="cc41a-154">A mező szerepkör hozzáadása = a hőmérséklet és a páratartalom az eszköz a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-154">Add a field role = temperature&humidity to the device by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="cc41a-155">IOT hubbal-explorer használata a felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc41a-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="cc41a-156">"A Hello, World" üzenetet küldeni az eszköz a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-156">Send a "Hello World" message to the device by running the following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="cc41a-157">Lásd: [IOT hubbal-Intéző segítségével az eszköz és az IoT-központ között üzeneteket küldjön és fogadjon](iot-hub-explorer-cloud-device-messaging.md) valós forgatókönyv esetén ez a parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="cc41a-157">See [Use iothub-explorer to send and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="cc41a-158">Használja az IOT hubbal-explorer eszköz twins lekérdezések</span><span class="sxs-lookup"><span data-stu-id="cc41a-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="cc41a-159">Eszközök a szerepkör címkével ellátott lekérdezése = "a hőmérséklet és a páratartalom" a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-159">Query devices with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="cc41a-160">Szerepkör címkével ellátott kivételével minden eszköz lekérdezése = "a hőmérséklet és a páratartalom" a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc41a-160">Query all devices except those with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="cc41a-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc41a-161">Next steps</span></span>

<span data-ttu-id="cc41a-162">Hogy megismerte az IOT hubbal-explorer használata a különböző felügyeleti lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="cc41a-162">You've learned how to use iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
