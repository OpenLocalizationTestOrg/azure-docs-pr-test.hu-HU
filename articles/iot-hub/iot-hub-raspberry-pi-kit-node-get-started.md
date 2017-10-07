---
title: "aaaRaspberry Pi toocloud (Node.js) - csatlakozás málna Pi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban málna Pi tooAzure IoT-központ málna Pi toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi raspberry pi iot hub, raspberry pi küldési adatok toocloud raspberry pi toocloud"
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57a15ab3984021a9c18ff0aa1316a4d4c6ebdec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a><span data-ttu-id="a1c52-104">Csatlakozás málna Pi tooAzure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="a1c52-104">Connect Raspberry Pi tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="a1c52-105">Ebben az oktatóanyagban akkor először tanulási hello használatának alapjait málna Pi Raspbian futtató.</span><span class="sxs-lookup"><span data-stu-id="a1c52-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="a1c52-106">Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="a1c52-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="a1c52-107">Windows 10 IoT minta, nyissa meg a toohello [Windows fejlesztői központ](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="a1c52-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="a1c52-108">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="a1c52-108">Don't have a kit yet?</span></span> <span data-ttu-id="a1c52-109">Próbálja [málna Pi online szimulátor](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a1c52-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="a1c52-110">Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="a1c52-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>


## <a name="what-you-do"></a><span data-ttu-id="a1c52-111">Mit</span><span class="sxs-lookup"><span data-stu-id="a1c52-111">What you do</span></span>

* <span data-ttu-id="a1c52-112">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-112">Create an IoT hub.</span></span>
* <span data-ttu-id="a1c52-113">Eszköz regisztrálása az IoT hub a pi.</span><span class="sxs-lookup"><span data-stu-id="a1c52-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="a1c52-114">A telepítő Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="a1c52-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="a1c52-115">Futtassa a mintaalkalmazást Pi toosend érzékelő adatokat tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="a1c52-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="a1c52-116">Csatlakozás málna Pi tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a1c52-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="a1c52-117">Akkor futtassa a mintaalkalmazást Pi toocollect hőmérséklet és a páratartalom adatok BME280 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="a1c52-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="a1c52-118">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="a1c52-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="a1c52-119">What you learn</span></span>

* <span data-ttu-id="a1c52-120">Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="a1c52-121">Hogyan tooconnect BME280 érzékelő a pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="a1c52-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="a1c52-122">Hogyan toocollect érzékelőadatait Pi mintaalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a1c52-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="a1c52-123">Hogyan toosend érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a1c52-124">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a1c52-124">What you need</span></span>

![Mi szükséges](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="a1c52-126">hello málna Pi 2 vagy málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="a1c52-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="a1c52-127">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a1c52-127">An active Azure subscription.</span></span> <span data-ttu-id="a1c52-128">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="a1c52-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="a1c52-129">A figyelő egy USB-billentyűzet és egér tooPi-hez.</span><span class="sxs-lookup"><span data-stu-id="a1c52-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="a1c52-130">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="a1c52-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="a1c52-131">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="a1c52-131">An Internet connection.</span></span>
* <span data-ttu-id="a1c52-132">Egy 16 GB-os vagy újabb microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a1c52-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="a1c52-133">Egy USB-SD adapter vagy microSD kártya tooburn hello operációsrendszer-képet hello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a1c52-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="a1c52-134">Egy 5-volt 2-amp tápegység hello 6-mértékű kiszolgálóhasználat micro USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="a1c52-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="a1c52-135">a következő elemek hello nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="a1c52-135">hello following items are optional:</span></span>

* <span data-ttu-id="a1c52-136">Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="a1c52-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="a1c52-137">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="a1c52-137">A breadboard.</span></span>
* <span data-ttu-id="a1c52-138">6/aljzat átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="a1c52-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="a1c52-139">Egy szórt 10 mm LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a1c52-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="a1c52-140">Ezek az elemek nem kötelező, mert hello kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="a1c52-140">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="a1c52-141">A telepítő Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="a1c52-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="a1c52-142">A Pi hello Raspbian operációs rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="a1c52-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="a1c52-143">Készítse elő a hello microSD-kártyán hello Raspbian lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1c52-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="a1c52-144">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="a1c52-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="a1c52-145">[Töltse le az asztallal Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip fájl).</span><span class="sxs-lookup"><span data-stu-id="a1c52-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="a1c52-146">Bontsa ki a hello Raspbian kép tooa mappát a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a1c52-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="a1c52-147">Telepítse a Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a1c52-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="a1c52-148">[Töltse le és telepítse a hello Etcher SD-kártya író segédprogram](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="a1c52-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="a1c52-149">Futtassa a Etcher és hello Raspbian kép kibontott válassza az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="a1c52-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="a1c52-150">Válassza ki a hello microSD-kártyát meghajtó.</span><span class="sxs-lookup"><span data-stu-id="a1c52-150">Select hello microSD card drive.</span></span> <span data-ttu-id="a1c52-151">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott hello megfelelő meghajtót.</span><span class="sxs-lookup"><span data-stu-id="a1c52-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="a1c52-152">Kattintson a Flash tooinstall Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a1c52-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="a1c52-153">Ha a telepítés hello microSD-kártyán eltávolítása a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="a1c52-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="a1c52-154">Ennek az oka biztonságos tooremove hello microSD-kártyán közvetlenül Etcher automatikusan kiadása vagy leválasztja hello microSD-kártyán befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="a1c52-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="a1c52-155">A Pi hello microSD-kártyán beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="a1c52-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="a1c52-156">SSH- és I2C engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a1c52-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="a1c52-157">Csatlakozás Pi toohello monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` a hello felhasználónévvel és `raspberry` hello jelszóként.</span><span class="sxs-lookup"><span data-stu-id="a1c52-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="a1c52-158">Kattintson a hello Raspberry ikon > **beállítások** > **málna Pi konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="a1c52-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![hello Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="a1c52-160">A hello **felületek** lapon **I2C** és **SSH** túl**engedélyezése**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1c52-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="a1c52-161">Ha nem rendelkezik a fizikai érzékelők, és szeretné, hogy szimulált toouse érzékelőadatait, ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="a1c52-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="a1c52-163">tooenable SSH és I2C található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="a1c52-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="a1c52-164">Csatlakozás hello érzékelő tooPi</span><span class="sxs-lookup"><span data-stu-id="a1c52-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="a1c52-165">Hello breadboard és átkötés fenyegetéseknek tooconnect LED és egy BME280 tooPi a következőképpen használhatja.</span><span class="sxs-lookup"><span data-stu-id="a1c52-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="a1c52-166">Ha még nem rendelkezik hello érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="a1c52-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![hello málna Pi és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="a1c52-168">hello BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1c52-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="a1c52-169">És hello LED fog villogni, ha egy eszköz és a hello közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="a1c52-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="a1c52-170">Érzékelő PIN-kód használja a következő vezetékezést hello:</span><span class="sxs-lookup"><span data-stu-id="a1c52-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="a1c52-171">Indítsa el a (érzékelő & LED)</span><span class="sxs-lookup"><span data-stu-id="a1c52-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="a1c52-172">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="a1c52-172">End (Board)</span></span>            | <span data-ttu-id="a1c52-173">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="a1c52-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="a1c52-174">VDD (PIN-kód 5 g.)</span><span class="sxs-lookup"><span data-stu-id="a1c52-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="a1c52-175">3.3V PWR (1. Pin)</span><span class="sxs-lookup"><span data-stu-id="a1c52-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="a1c52-176">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-176">White cable</span></span>   |
| <span data-ttu-id="a1c52-177">GND (7G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="a1c52-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="a1c52-178">GND (PIN-kód 6)</span><span class="sxs-lookup"><span data-stu-id="a1c52-178">GND (Pin 6)</span></span>            | <span data-ttu-id="a1c52-179">Barna kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-179">Brown cable</span></span>   |
| <span data-ttu-id="a1c52-180">SDI (PIN-kód 10G)</span><span class="sxs-lookup"><span data-stu-id="a1c52-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="a1c52-181">I2C1 SDA (3 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="a1c52-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="a1c52-182">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-182">Red cable</span></span>     |
| <span data-ttu-id="a1c52-183">SCK (8G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="a1c52-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="a1c52-184">I2C1 SCL (PIN-kód 5)</span><span class="sxs-lookup"><span data-stu-id="a1c52-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="a1c52-185">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-185">Orange cable</span></span>  |
| <span data-ttu-id="a1c52-186">LED VDD (PIN-kód 18F)</span><span class="sxs-lookup"><span data-stu-id="a1c52-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="a1c52-187">GPIO 24 (PIN-kód 18)</span><span class="sxs-lookup"><span data-stu-id="a1c52-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="a1c52-188">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-188">White cable</span></span>   |
| <span data-ttu-id="a1c52-189">LED GND (PIN-kód 17F)</span><span class="sxs-lookup"><span data-stu-id="a1c52-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="a1c52-190">GND (PIN-kód 20)</span><span class="sxs-lookup"><span data-stu-id="a1c52-190">GND (Pin 20)</span></span>           | <span data-ttu-id="a1c52-191">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="a1c52-191">Black cable</span></span>   |

<span data-ttu-id="a1c52-192">Kattintson a tooview [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="a1c52-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="a1c52-193">Miután sikeresen csatlakozott BME280 tooyour málna Pi, meg kell például a kép alatt.</span><span class="sxs-lookup"><span data-stu-id="a1c52-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="a1c52-195">A Pi toohello hálózat</span><span class="sxs-lookup"><span data-stu-id="a1c52-195">Connect Pi toohello network</span></span>

<span data-ttu-id="a1c52-196">Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység.</span><span class="sxs-lookup"><span data-stu-id="a1c52-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="a1c52-197">Használjon hello Ethernet kábel tooconnect Pi tooyour vezetékes hálózati, vagy hajtsa végre a hello [hello málna Pi Foundation utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour vezeték nélküli hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a1c52-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="a1c52-198">A Pi sikeresen csatlakoztatott toohello hálózati után tootake hello jegyezze fel kell [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="a1c52-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Csatlakoztatott toowired hálózati](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="a1c52-200">Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="a1c52-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="a1c52-201">Például, ha a számítógép vezeték nélküli hálózathoz csatlakoztatott tooa Pi pedig vezetékes hálózathoz csatlakoztatott tooa, akkor előfordulhat, hogy nem lásd: hello IP-cím hello devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="a1c52-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="a1c52-202">A Pi mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a1c52-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a><span data-ttu-id="a1c52-203">Klónozza a mintaalkalmazást és hello előfeltételként szükséges csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="a1c52-203">Clone sample application and install hello prerequisite packages</span></span>

1. <span data-ttu-id="a1c52-204">SSH-ügyfél követően a gazdagép számítógép tooconnect tooyour málna Pi hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="a1c52-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="a1c52-205">**Windows-felhasználók**</span><span class="sxs-lookup"><span data-stu-id="a1c52-205">**Windows Users**</span></span>
   1. <span data-ttu-id="a1c52-206">Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="a1c52-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="a1c52-207">Másolja a Pi hello gazdagép nevét (vagy IP-cím) szakasz hello IP-címét, és válassza ki az SSH hello kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="a1c52-207">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   ![A puTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="a1c52-209">**Mac és Ubuntu felhasználók**</span><span class="sxs-lookup"><span data-stu-id="a1c52-209">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="a1c52-210">Ubuntu vagy macOS hello beépített SSH-ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="a1c52-210">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="a1c52-211">Előfordulhat, hogy toorun `ssh pi@<ip address of pi>` tooconnect Pi SSH-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="a1c52-211">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="a1c52-212">hello alapértelmezett felhasználónév az `pi` , és hello jelszó `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="a1c52-212">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="a1c52-213">Telepítse a Node.js és NPM tooyour Pi.</span><span class="sxs-lookup"><span data-stu-id="a1c52-213">Install Node.js and NPM tooyour Pi.</span></span>
   
   <span data-ttu-id="a1c52-214">Először a Node.js-verzió érdemes egyeztetni a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="a1c52-214">First you should check your Node.js version with hello following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="a1c52-215">Ha hello verziószáma kisebb, mint a 4.x-es, vagy nincs nem Node.js a Pi, majd futtassa a következő parancs tooinstall hello, vagy frissítse a Node.js.</span><span class="sxs-lookup"><span data-stu-id="a1c52-215">If hello version is lower than 4.x or there is no Node.js on your Pi, then run hello following command tooinstall or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="a1c52-216">Klónozza a mintaalkalmazást hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1c52-216">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="a1c52-217">A következő parancs hello összes csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1c52-217">Install all packages by hello following command.</span></span> <span data-ttu-id="a1c52-218">Ez magában foglalja az Azure IoT-eszközök SDK, BME280 érzékelő és huzalozási Pi függvénytárak.</span><span class="sxs-lookup"><span data-stu-id="a1c52-218">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="a1c52-219">Eltarthat néhány percig toofinish a telepítési folyamat attól függően, hogy a hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-219">It might take several minutes toofinish this installation process depending on your network connection.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="a1c52-220">Hello mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a1c52-220">Configure hello sample application</span></span>

1. <span data-ttu-id="a1c52-221">Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1c52-221">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="a1c52-223">Két elem ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="a1c52-223">There are two items in this file you can configurate.</span></span> <span data-ttu-id="a1c52-224">hello először egy van `interval`, amely megadja, hogy hello időtartam (ezredmásodpercben) két toocloud küldött üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="a1c52-224">hello first one is `interval`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="a1c52-225">második hello `simulatedData`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="a1c52-225">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="a1c52-226">Ha Ön **nincs hello érzékelő**, beállíthatja hello `simulatedData` érték túl`true` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="a1c52-226">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="a1c52-227">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="a1c52-227">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="a1c52-228">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a1c52-228">Run hello sample application</span></span>

<span data-ttu-id="a1c52-229">Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1c52-229">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="a1c52-230">Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="a1c52-230">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="a1c52-231">Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-231">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="a1c52-233">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1c52-233">Next steps</span></span>

<span data-ttu-id="a1c52-234">Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c52-234">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="a1c52-235">a málna Pi egy parancssori felületen, a elküldte tooyour IoT hub vagy küldési üzenetek tooyour málna Pi toosee köszönőüzenetei lásd: hello [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="a1c52-235">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
