---
title: "az IoT-eszközök kezelése a IOT hubbal-explorer aaaAzure |} Microsoft Docs"
description: "Eszközzel hello IOT hubbal-explorer CLI Azure IoT Hub kezeléséhez, ha kiemeli hello közvetlen módszerek és hello iker kívánt tulajdonságokkal kezelési lehetőségek."
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
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="e40cf-104">Használja az IOT hubbal-explorer Azure IoT Hub – Eszközkezelés</span><span class="sxs-lookup"><span data-stu-id="e40cf-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="e40cf-106">[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) parancssori eszköz, amely futtatja a fogadó számítógép toomanage eszköz azonosítói az IoT hub beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="e40cf-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer toomanage device identities in your IoT hub registry.</span></span> <span data-ttu-id="e40cf-107">Az ismét tooperform használt beállítások a különböző feladatok.</span><span class="sxs-lookup"><span data-stu-id="e40cf-107">It comes with management options that you can use tooperform various tasks.</span></span>

| <span data-ttu-id="e40cf-108">Lehetőséget</span><span class="sxs-lookup"><span data-stu-id="e40cf-108">Management option</span></span>          | <span data-ttu-id="e40cf-109">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="e40cf-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e40cf-110">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="e40cf-110">Direct methods</span></span>             | <span data-ttu-id="e40cf-111">Egy eszköz, például indítása és leállítása üzenetküldésre vagy hello eszköz újraindítása működésre tétele.</span><span class="sxs-lookup"><span data-stu-id="e40cf-111">Make a device act such as starting or stopping sending messages or rebooting hello device.</span></span>                                        |
| <span data-ttu-id="e40cf-112">A két szükségeskonfiguráció-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="e40cf-112">Twin desired properties</span></span>    | <span data-ttu-id="e40cf-113">Egy eszköz bocsátott egyes állapotok, például egy LED toogreen beállítást, vagy beállítás hello telemetriai adatok küldése időköz too30 perc.</span><span class="sxs-lookup"><span data-stu-id="e40cf-113">Put a device into certain states, such as setting an LED toogreen or setting hello telemetry send interval too30 minutes.</span></span>         |
| <span data-ttu-id="e40cf-114">A két jelentett tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e40cf-114">Twin reported properties</span></span>   | <span data-ttu-id="e40cf-115">Első hello jelentett eszközök állapotát.</span><span class="sxs-lookup"><span data-stu-id="e40cf-115">Get hello reported state of a device.</span></span> <span data-ttu-id="e40cf-116">Például a hello eszköz jelentés hello LED most villogó-e.</span><span class="sxs-lookup"><span data-stu-id="e40cf-116">For example, hello device reports hello LED is blinking now.</span></span>                                    |
| <span data-ttu-id="e40cf-117">A két címkék</span><span class="sxs-lookup"><span data-stu-id="e40cf-117">Twin tags</span></span>                  | <span data-ttu-id="e40cf-118">Hello felhőben tárolt az eszközre vonatkozó metaadatok.</span><span class="sxs-lookup"><span data-stu-id="e40cf-118">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="e40cf-119">Például hello egy Eladóautomata telepítési helyét.</span><span class="sxs-lookup"><span data-stu-id="e40cf-119">For example, hello deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="e40cf-120">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="e40cf-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="e40cf-121">Értesítések tooa eszköz küldése.</span><span class="sxs-lookup"><span data-stu-id="e40cf-121">Send notifications tooa device.</span></span> <span data-ttu-id="e40cf-122">Például "akkor nagyon valószínű toorain ma.</span><span class="sxs-lookup"><span data-stu-id="e40cf-122">For example, "It is very likely toorain today.</span></span> <span data-ttu-id="e40cf-123">Ne feledje toobring egy összevonó."</span><span class="sxs-lookup"><span data-stu-id="e40cf-123">Don't forget toobring an umbrella."</span></span>              |
| <span data-ttu-id="e40cf-124">Eszköz iker lekérdezések</span><span class="sxs-lookup"><span data-stu-id="e40cf-124">Device twin queries</span></span>        | <span data-ttu-id="e40cf-125">A lekérdezés az összes eszköz twins tooretrieve rendelkező tetszőleges feltételek, például használható hello eszközök azonosításához.</span><span class="sxs-lookup"><span data-stu-id="e40cf-125">Query all device twins tooretrieve those with arbitrary conditions, such as identifying hello devices that are available for use.</span></span> |

<span data-ttu-id="e40cf-126">Részletesebb hello különbségek a Magyarázat és útmutatást ezen beállítások használatával, lásd: [eszközről a felhőbe kommunikációs útmutatást](iot-hub-devguide-d2c-guidance.md) és [felhő eszközre kommunikációs útmutatást](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="e40cf-126">For more detailed explanation on hello differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e40cf-127">Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják.</span><span class="sxs-lookup"><span data-stu-id="e40cf-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="e40cf-128">Az IoT-központ továbbra is fennáll, egy eszköz iker tooit csatlakozó eszközökről.</span><span class="sxs-lookup"><span data-stu-id="e40cf-128">IoT Hub persists a device twin for each device that connects tooit.</span></span> <span data-ttu-id="e40cf-129">Eszköz twins kapcsolatos további információkért lásd: [Ismerkedés az eszköz twins](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="e40cf-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="e40cf-130">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="e40cf-130">What you learn</span></span>

<span data-ttu-id="e40cf-131">Megismerheti a fejlesztői gépen különböző kezelési lehetőségek az IOT hubbal-Intéző segítségével.</span><span class="sxs-lookup"><span data-stu-id="e40cf-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="e40cf-132">Mit</span><span class="sxs-lookup"><span data-stu-id="e40cf-132">What you do</span></span>

<span data-ttu-id="e40cf-133">IOT hubbal-kezelő futtatása a különböző felügyeleti beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="e40cf-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e40cf-134">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e40cf-134">What you need</span></span>

- <span data-ttu-id="e40cf-135">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="e40cf-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="e40cf-136">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e40cf-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="e40cf-137">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e40cf-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="e40cf-138">Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e40cf-138">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="e40cf-139">Győződjön meg arról, hogy az eszköz hello ügyfélalkalmazás Ez az oktatóanyag során fut-e.</span><span class="sxs-lookup"><span data-stu-id="e40cf-139">Make sure your device is running with hello client application during this tutorial.</span></span>
- <span data-ttu-id="e40cf-140">IOT hubbal-Explorerben [telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer) a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="e40cf-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-tooyour-iot-hub"></a><span data-ttu-id="e40cf-141">Csatlakozás tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="e40cf-141">Connect tooyour IoT hub</span></span>

<span data-ttu-id="e40cf-142">Csatlakozás az IoT-központ tooyour hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-142">Connect tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="e40cf-143">Használja az IOT hubbal-explorer közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="e40cf-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="e40cf-144">Hello meghívása `start` metódus a hello eszköz alkalmazás toosend üzenetek tooyour IoT-központ hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-144">Invoke hello `start` method in hello device app toosend messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="e40cf-145">Hello meghívása `stop` hello eszköz alkalmazás toostop küldésekor metódus tooyour IoT-központ üzenetek hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-145">Invoke hello `stop` method in hello device app toostop sending messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="e40cf-146">Használja az IOT hubbal-explorer iker a kívánt tulajdonságokkal</span><span class="sxs-lookup"><span data-stu-id="e40cf-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="e40cf-147">Állítsa be a kívánt tulajdonság intervallumát 3000 = hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-147">Set a desired property interval = 3000 by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="e40cf-148">Ez a tulajdonság az eszköz által is olvasható.</span><span class="sxs-lookup"><span data-stu-id="e40cf-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="e40cf-149">Használja az IOT hubbal-explorer iker által jelentett tulajdonságokkal</span><span class="sxs-lookup"><span data-stu-id="e40cf-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="e40cf-150">Első hello hello eszköz futtatásával hello jelentett tulajdonságait a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e40cf-150">Get hello reported properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="e40cf-151">Hello tulajdonságok egyike $metadata. melyik látható hello utolsó idő az eszköz $lastUpdated küld vagy üzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="e40cf-151">One of hello properties is $metadata.$lastUpdated which shows hello last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="e40cf-152">-Kezelővel IOT hubbal-iker tartozó címkékkel</span><span class="sxs-lookup"><span data-stu-id="e40cf-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="e40cf-153">Megjeleníti a hello címkék és hello eszköz tulajdonságait hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-153">Display hello tags and properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="e40cf-154">A mező szerepkör hozzáadása = a hőmérséklet és a páratartalom toohello eszköz hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-154">Add a field role = temperature&humidity toohello device by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="e40cf-155">IOT hubbal-explorer használata a felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="e40cf-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="e40cf-156">A "Hello World" üzenet toohello eszköz küldése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-156">Send a "Hello World" message toohello device by running hello following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="e40cf-157">Lásd: [IOT hubbal-explorer toosend használja, és az eszköz és az IoT-központ között üzeneteket fogadni](iot-hub-explorer-cloud-device-messaging.md) valós forgatókönyv esetén ez a parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="e40cf-157">See [Use iothub-explorer toosend and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="e40cf-158">Használja az IOT hubbal-explorer eszköz twins lekérdezések</span><span class="sxs-lookup"><span data-stu-id="e40cf-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="e40cf-159">Eszközök a szerepkör címkével ellátott lekérdezése = "a hőmérséklet és a páratartalom" hello, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-159">Query devices with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="e40cf-160">Szerepkör címkével ellátott kivételével minden eszköz lekérdezése = "a hőmérséklet és a páratartalom" hello, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e40cf-160">Query all devices except those with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="e40cf-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e40cf-161">Next steps</span></span>

<span data-ttu-id="e40cf-162">Hogy megismerte hogyan toouse IOT hubbal-explorer, a különböző felügyeleti beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="e40cf-162">You've learned how toouse iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
