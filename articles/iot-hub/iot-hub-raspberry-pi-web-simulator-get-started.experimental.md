---
title: "aaaSimulated málna Pi toocloud (Node.js) - csatlakozás málna Pi webes szimulátor tooAzure IoT-központ |} Microsoft Docs"
description: "Csatlakozás málna Pi webes szimulátor tooAzure IoT-központ málna Pi toosend adatok toohello Azure felhőben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi szimulátor, az azure iot raspberry pi raspberry pi iot-központot, raspberry pi küldése adatok toocloud raspberry pi toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/7/2017
ms.author: xshi
ms.openlocfilehash: 0ebe1004e96ff4e64fdd1b05b7e35192ec42f7d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a><span data-ttu-id="6966f-104">Csatlakozás málna Pi online szimulátor tooAzure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="6966f-104">Connect Raspberry Pi online simulator tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="6966f-105">Ebben az oktatóanyagban akkor először hello használatának alapjait málna Pi online szimulátor tanulási.</span><span class="sxs-lookup"><span data-stu-id="6966f-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi online simulator.</span></span> <span data-ttu-id="6966f-106">Majd megismerheti, hogyan tooseamlessly hello Pi szimulátor toohello felhő használatával kapcsolódnak [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="6966f-106">You then learn how tooseamlessly connect hello Pi simulator toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> 

<span data-ttu-id="6966f-107">Ha fizikai eszközöket, látogasson [csatlakozás málna Pi tooAzure IoT-központ](iot-hub-raspberry-pi-kit-node-get-started.md) tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="6966f-107">If you have physical devices, visit [Connect Raspberry Pi tooAzure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) tooget started.</span></span> 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a><span data-ttu-id="6966f-108">Mit</span><span class="sxs-lookup"><span data-stu-id="6966f-108">What you do</span></span>

* <span data-ttu-id="6966f-109">Hello elsajátítását málna Pi online szimulátor.</span><span class="sxs-lookup"><span data-stu-id="6966f-109">Learn hello basics of Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="6966f-110">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6966f-110">Create an IoT hub.</span></span>
* <span data-ttu-id="6966f-111">Eszköz regisztrálása az IoT hub a pi.</span><span class="sxs-lookup"><span data-stu-id="6966f-111">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="6966f-112">Futtassa a mintaalkalmazást Pi szimulált toosend érzékelő adatokat tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="6966f-112">Run a sample application on Pi toosend simulated sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="6966f-113">Csatlakoztassa a szimulált málna Pi tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6966f-113">Connect simulated Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="6966f-114">Majd futtassa a mintaalkalmazást hello szimulátor toogenerate érzékelő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="6966f-114">Then you run a sample application with hello simulator toogenerate sensor data.</span></span> <span data-ttu-id="6966f-115">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6966f-115">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="6966f-116">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="6966f-116">What you learn</span></span>

* <span data-ttu-id="6966f-117">Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="6966f-117">How toocreate an Azure IoT hub and get your new device connection string.</span></span> <span data-ttu-id="6966f-118">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="6966f-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="6966f-119">Hogyan toowork rendelkező málna Pi online szimulátor.</span><span class="sxs-lookup"><span data-stu-id="6966f-119">How toowork with Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="6966f-120">Hogyan toosend érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6966f-120">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="overview-of-raspberry-pi-web-simulator"></a><span data-ttu-id="6966f-121">Málna Pi webes szimulátor áttekintése</span><span class="sxs-lookup"><span data-stu-id="6966f-121">Overview of Raspberry Pi web simulator</span></span>

<span data-ttu-id="6966f-122">Kattintson a hello gomb toolaunch málna Pi online szimulátor.</span><span class="sxs-lookup"><span data-stu-id="6966f-122">Click hello button toolaunch Raspberry Pi online simulator.</span></span>

> [!div class="button"]
[<span data-ttu-id="6966f-123">Indítsa el a málna Pi szimulátor</span><span class="sxs-lookup"><span data-stu-id="6966f-123">Start Raspberry Pi simulator</span></span>](https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted)

<span data-ttu-id="6966f-124">Nincsenek hello webes szimulátor háromféleképpen használhatók.</span><span class="sxs-lookup"><span data-stu-id="6966f-124">There are three areas in hello web simulator.</span></span>
* <span data-ttu-id="6966f-125">Összeállítási terület - hello alapértelmezett kapcsolatcsoport, a Pi kapcsolódik BME280 érzékelő és LED.</span><span class="sxs-lookup"><span data-stu-id="6966f-125">Assembly area - hello default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="6966f-126">hello terület zárolva van az előzetes verzióra jelenleg így testreszabási nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="6966f-126">hello area is locked in preview version so currently you cannot do customization.</span></span>
* <span data-ttu-id="6966f-127">Kódolás a terület - toocode málna Pi az online kód szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="6966f-127">Coding area - An online code editor for you toocode with Raspberry Pi.</span></span> <span data-ttu-id="6966f-128">hello alapértelmezett mintaalkalmazás BME280 érzékelő toocollect érzékelőadatait segítségével, és elküldi a tooyour Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="6966f-128">hello default sample application helps toocollect sensor data from BME280 sensor and sends tooyour Azure IoT Hub.</span></span> <span data-ttu-id="6966f-129">hello alkalmazásra az teljes mértékben kompatibilis a valódi Pi eszközök.</span><span class="sxs-lookup"><span data-stu-id="6966f-129">hello application is fully compatible with real Pi devices.</span></span> 
* <span data-ttu-id="6966f-130">Integrált Windows - hello kimeneti kódot mutatja.</span><span class="sxs-lookup"><span data-stu-id="6966f-130">Integrated console window - It shows hello output of your code.</span></span> <span data-ttu-id="6966f-131">Az ablak hello tetején nincsenek három gomb.</span><span class="sxs-lookup"><span data-stu-id="6966f-131">At hello top of this window, there are three buttons.</span></span>
   * <span data-ttu-id="6966f-132">**Futtatás** -terület kódolási hello hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="6966f-132">**Run** - Run hello application in hello coding area.</span></span>
   * <span data-ttu-id="6966f-133">**Alaphelyzetbe** -visszaállítási hello terület toohello alapértelmezett mintaalkalmazás kódolása.</span><span class="sxs-lookup"><span data-stu-id="6966f-133">**Reset** - Reset hello coding area toohello default sample application.</span></span>
   * <span data-ttu-id="6966f-134">**Modellrészek/kibontott** – a megfelelő toofold/bővíthető hello konzolablakban gomb nincs ügyféloldali hello.</span><span class="sxs-lookup"><span data-stu-id="6966f-134">**Fold/Expand** - On hello right side there is a button for you toofold/expand hello console window.</span></span>

> [!NOTE] 
<span data-ttu-id="6966f-135">hello málna Pi webes szimulátor jelenleg előzetes verzióban érhető el.</span><span class="sxs-lookup"><span data-stu-id="6966f-135">hello Raspberry Pi web simulator is now available in preview version.</span></span> <span data-ttu-id="6966f-136">Szeretnénk toohear a beszéd a hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="6966f-136">We'd like toohear your voice in hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span></span> <span data-ttu-id="6966f-137">hello forráskód a nyilvános [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="6966f-137">hello source code is public on [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span></span>

![A Pi online szimulátor áttekintése](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="6966f-139">A Pi webes szimulátor mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="6966f-139">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="6966f-140">Kódolási terület, győződjön meg arról, hello alapértelmezett mintaalkalmazás dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="6966f-140">In coding area, make sure you are working on hello default sample application.</span></span> <span data-ttu-id="6966f-141">A sor 15 hello helyőrzőt cserélje le hello Azure IoT hub eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="6966f-141">Replace hello placeholder in Line 15 with hello Azure IoT hub device connection string.</span></span>
   <span data-ttu-id="6966f-142">![Cserélje le a hello eszköz kapcsolati karakterlánc](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span><span class="sxs-lookup"><span data-stu-id="6966f-142">![Replace hello device connection string](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span></span>

2. <span data-ttu-id="6966f-143">Kattintson a **futtatása** vagy típus `npm start` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6966f-143">Click **Run** or type `npm start` toorun hello application.</span></span>


<span data-ttu-id="6966f-144">Megjelenik a következő hello kimeneti, amely mutatja hello érzékelőadatait és hello tooyour IoT-központ küldött üzenetek ![kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span><span class="sxs-lookup"><span data-stu-id="6966f-144">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub ![Output - sensor data sent from Raspberry Pi tooyour IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="6966f-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6966f-145">Next steps</span></span>

<span data-ttu-id="6966f-146">Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6966f-146">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
