---
title: "A felhő (Python) - málna Pi csatlakozzon az Azure IoT Hub Raspberry Pi |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ málna Pi adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban málna Pi csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi, raspberry pi iot-központ, raspberry pi adatokat küldött a felhőben, raspberry pi felhőbe"
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 1b1a9dc960846cbc15ce09d0fd106e1492937439
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a><span data-ttu-id="3e789-104">Raspberry Pi csatlakozni az Azure IoT Hub (Python)</span><span class="sxs-lookup"><span data-stu-id="3e789-104">Connect Raspberry Pi to Azure IoT Hub (Python)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="3e789-105">Ebben az oktatóanyagban akkor először tanulás alapjainak málna Pi Raspbian futtató használata.</span><span class="sxs-lookup"><span data-stu-id="3e789-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="3e789-106">Majd megtudhatja, hogyan kapcsolódhat zökkenőmentesen az eszközök a felhőbe [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="3e789-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="3e789-107">Windows 10 IoT minta, látogasson el a [Windows fejlesztői központ](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="3e789-107">For Windows 10 IoT Core samples, go to the [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="3e789-108">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="3e789-108">Don't have a kit yet?</span></span> <span data-ttu-id="3e789-109">Próbálja [málna Pi online szimulátor](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3e789-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="3e789-110">Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="3e789-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="3e789-111">Mit</span><span class="sxs-lookup"><span data-stu-id="3e789-111">What you do</span></span>

* <span data-ttu-id="3e789-112">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="3e789-112">Create an IoT hub.</span></span>
* <span data-ttu-id="3e789-113">Eszköz regisztrálása az IoT hub a pi.</span><span class="sxs-lookup"><span data-stu-id="3e789-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="3e789-114">A telepítő Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="3e789-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="3e789-115">Futtassa a mintaalkalmazást érzékelő adatokat küldeni az IoT hub Pi.</span><span class="sxs-lookup"><span data-stu-id="3e789-115">Run a sample application on Pi to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="3e789-116">Az IoT-központ az Ön által létrehozott málna Pi csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3e789-116">Connect Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="3e789-117">Majd futtassa a mintaalkalmazást a hőmérséklet és a páratartalom adatokat gyűjteni BME280 érzékelő Pi.</span><span class="sxs-lookup"><span data-stu-id="3e789-117">Then you run a sample application on Pi to collect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="3e789-118">Végezetül az érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3e789-118">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="3e789-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="3e789-119">What you learn</span></span>

* <span data-ttu-id="3e789-120">Megtudhatja, hogyan hozzon létre egy Azure IoT-központot, és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3e789-120">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="3e789-121">Hogyan BME280 érzékelő Pi kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="3e789-121">How to connect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="3e789-122">Megtudhatja, hogyan futtatja a mintaalkalmazás Pi érzékelő adatok gyűjtéséért felelős ügyfélfeladatot.</span><span class="sxs-lookup"><span data-stu-id="3e789-122">How to collect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="3e789-123">Hogyan érzékelő adatokat küldeni az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3e789-123">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3e789-124">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="3e789-124">What you need</span></span>

![Mi szükséges](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="3e789-126">A Pi 2 málna vagy málna Pi 3 kártya.</span><span class="sxs-lookup"><span data-stu-id="3e789-126">The Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="3e789-127">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3e789-127">An active Azure subscription.</span></span> <span data-ttu-id="3e789-128">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3e789-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="3e789-129">A figyelő egy USB-billentyűzet és egér Pi-hez.</span><span class="sxs-lookup"><span data-stu-id="3e789-129">A monitor, a USB keyboard, and mouse that connect to Pi.</span></span>
* <span data-ttu-id="3e789-130">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="3e789-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="3e789-131">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="3e789-131">An Internet connection.</span></span>
* <span data-ttu-id="3e789-132">Egy 16 GB-os vagy újabb microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="3e789-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="3e789-133">Egy USB-SD adapter vagy microSD-kártyán írása a microSD-kártyán operációsrendszer-képet.</span><span class="sxs-lookup"><span data-stu-id="3e789-133">A USB-SD adapter or microSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="3e789-134">Egy 5-volt 2-amp tápegység a 6-mértékű kiszolgálóhasználat micro USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="3e789-134">A 5-volt 2-amp power supply with the 6-foot micro USB cable.</span></span>

<span data-ttu-id="3e789-135">A következő elemek nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="3e789-135">The following items are optional:</span></span>

* <span data-ttu-id="3e789-136">Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="3e789-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="3e789-137">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="3e789-137">A breadboard.</span></span>
* <span data-ttu-id="3e789-138">6/aljzat átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="3e789-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="3e789-139">Egy szórt 10 mm LED-jét.</span><span class="sxs-lookup"><span data-stu-id="3e789-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="3e789-140">Ezek az elemek nem kötelező, mert a kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="3e789-140">These items are optional because the code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a><span data-ttu-id="3e789-141">Málna Pi beállítása</span><span class="sxs-lookup"><span data-stu-id="3e789-141">Set up Raspberry Pi</span></span>

### <a name="install-the-raspbian-operating-system-for-pi"></a><span data-ttu-id="3e789-142">A Pi a Raspbian operációs rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="3e789-142">Install the Raspbian operating system for Pi</span></span>

<span data-ttu-id="3e789-143">Készítse elő a microSD-kártyán Raspbian kép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e789-143">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="3e789-144">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="3e789-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="3e789-145">[Töltse le az asztallal Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (a .zip-fájlt).</span><span class="sxs-lookup"><span data-stu-id="3e789-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (the .zip file).</span></span>
   1. <span data-ttu-id="3e789-146">Bontsa ki a Raspbian lemezképet a számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="3e789-146">Extract the Raspbian image to a folder on your computer.</span></span>
1. <span data-ttu-id="3e789-147">A microSD-kártyán Raspbian telepítése.</span><span class="sxs-lookup"><span data-stu-id="3e789-147">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="3e789-148">[Töltse le és telepítse a Etcher SD-kártya író segédprogram](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="3e789-148">[Download and install the Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="3e789-149">Futtassa a Etcher, és az 1. lépésben válassza ki a kibontott Raspbian kép.</span><span class="sxs-lookup"><span data-stu-id="3e789-149">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="3e789-150">Jelölje ki a microSD-kártyát meghajtót.</span><span class="sxs-lookup"><span data-stu-id="3e789-150">Select the microSD card drive.</span></span> <span data-ttu-id="3e789-151">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott ki a megfelelő meghajtó.</span><span class="sxs-lookup"><span data-stu-id="3e789-151">Note that Etcher may have already selected the correct drive.</span></span>
   1. <span data-ttu-id="3e789-152">Kattintson a microSD-kártyán Raspbian telepítése Flash.</span><span class="sxs-lookup"><span data-stu-id="3e789-152">Click Flash to install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="3e789-153">Eltávolítja a számítógépről a microSD-kártyán, ha a telepítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3e789-153">Remove the microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="3e789-154">Biztonságos a microSD-kártyán közvetlenül eltávolítani, mert Etcher automatikusan kiadása vagy leválasztja a microSD-kártyán befejezése után is.</span><span class="sxs-lookup"><span data-stu-id="3e789-154">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   1. <span data-ttu-id="3e789-155">A microSD-kártyán beszúrása Pi.</span><span class="sxs-lookup"><span data-stu-id="3e789-155">Insert the microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="3e789-156">SSH- és I2C engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3e789-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="3e789-157">Pi csatlakozni a monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` felhasználónevet és `raspberry` a jelszót.</span><span class="sxs-lookup"><span data-stu-id="3e789-157">Connect Pi to the monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as the user name and `raspberry` as the password.</span></span>
1. <span data-ttu-id="3e789-158">Kattintson a Raspberry ikonra > **beállítások** > **málna Pi konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="3e789-158">Click the Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![A Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="3e789-160">Az a **felületek** lapon **I2C** és **SSH** való **engedélyezése**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e789-160">On the **Interfaces** tab, set **I2C** and **SSH** to **Enable**, and then click **OK**.</span></span> <span data-ttu-id="3e789-161">Ha nem rendelkezik a fizikai érzékelők és szimulált érzékelőadatait használni szeretne, ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="3e789-161">If you don't have physical sensors and want to use simulated sensor data, this step is optional.</span></span>

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="3e789-163">SSH- és I2C engedélyezéséhez található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="3e789-163">To enable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-the-sensor-to-pi"></a><span data-ttu-id="3e789-164">Csatlakozás az érzékelő Pi</span><span class="sxs-lookup"><span data-stu-id="3e789-164">Connect the sensor to Pi</span></span>

<span data-ttu-id="3e789-165">Használják a breadboard és átkötés LED és egy BME280 Pi kell kapcsolódniuk az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="3e789-165">Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows.</span></span> <span data-ttu-id="3e789-166">Ha még nem rendelkezik az érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="3e789-166">If you don’t have the sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![A Pi málna és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="3e789-168">A BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="3e789-168">The BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="3e789-169">És a LED fog villogni, ha egy eszköz és a felhő közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="3e789-169">And the LED will blink if there is a communication between device and the cloud.</span></span> 

<span data-ttu-id="3e789-170">PIN-kód érzékelő használja a következő vezetékezést:</span><span class="sxs-lookup"><span data-stu-id="3e789-170">For sensor pins, use the following wiring:</span></span>

| <span data-ttu-id="3e789-171">Indítsa el a (érzékelő & LED)</span><span class="sxs-lookup"><span data-stu-id="3e789-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="3e789-172">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="3e789-172">End (Board)</span></span>            | <span data-ttu-id="3e789-173">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="3e789-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="3e789-174">VDD (PIN-kód 5 g.)</span><span class="sxs-lookup"><span data-stu-id="3e789-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="3e789-175">3.3V PWR (1. Pin)</span><span class="sxs-lookup"><span data-stu-id="3e789-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="3e789-176">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-176">White cable</span></span>   |
| <span data-ttu-id="3e789-177">GND (7G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="3e789-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="3e789-178">GND (PIN-kód 6)</span><span class="sxs-lookup"><span data-stu-id="3e789-178">GND (Pin 6)</span></span>            | <span data-ttu-id="3e789-179">Barna kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-179">Brown cable</span></span>   |
| <span data-ttu-id="3e789-180">SDI (PIN-kód 10G)</span><span class="sxs-lookup"><span data-stu-id="3e789-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="3e789-181">I2C1 SDA (3 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="3e789-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="3e789-182">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-182">Red cable</span></span>     |
| <span data-ttu-id="3e789-183">SCK (8G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="3e789-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="3e789-184">I2C1 SCL (PIN-kód 5)</span><span class="sxs-lookup"><span data-stu-id="3e789-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="3e789-185">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-185">Orange cable</span></span>  |
| <span data-ttu-id="3e789-186">LED VDD (PIN-kód 18F)</span><span class="sxs-lookup"><span data-stu-id="3e789-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="3e789-187">GPIO 24 (PIN-kód 18)</span><span class="sxs-lookup"><span data-stu-id="3e789-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="3e789-188">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-188">White cable</span></span>   |
| <span data-ttu-id="3e789-189">LED GND (PIN-kód 17F)</span><span class="sxs-lookup"><span data-stu-id="3e789-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="3e789-190">GND (PIN-kód 20)</span><span class="sxs-lookup"><span data-stu-id="3e789-190">GND (Pin 20)</span></span>           | <span data-ttu-id="3e789-191">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="3e789-191">Black cable</span></span>   |

<span data-ttu-id="3e789-192">Megjelenítéséhez kattintson ide [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="3e789-192">Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="3e789-193">Miután sikeresen csatlakozott a málna Pi BME280, meg kell például a kép alatt.</span><span class="sxs-lookup"><span data-stu-id="3e789-193">After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.</span></span>

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a><span data-ttu-id="3e789-195">A Pi csatlakoznak a hálózathoz</span><span class="sxs-lookup"><span data-stu-id="3e789-195">Connect Pi to the network</span></span>

<span data-ttu-id="3e789-196">Kapcsolja be a Pi a micro USB-kábelen és a tápegység.</span><span class="sxs-lookup"><span data-stu-id="3e789-196">Turn on Pi by using the micro USB cable and the power supply.</span></span> <span data-ttu-id="3e789-197">Az Ethernet-kábel segítségével Pi csatlakozni a vezetékes hálózatra, vagy hajtsa végre a [málna Pi alapját utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi a vezeték nélküli hálózathoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="3e789-197">Use the Ethernet cable to connect Pi to your wired network or follow the [instructions from the Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) to connect Pi to your wireless network.</span></span> <span data-ttu-id="3e789-198">Miután a Pi sikeresen csatlakozott a hálózathoz, azt kell jegyezze fel az a [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="3e789-198">After your Pi has been successfully connected to the network, you need to take a note of the [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Vezetékes hálózathoz csatlakoznak](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="3e789-200">Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="3e789-200">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="3e789-201">Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="3e789-201">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="3e789-202">A Pi mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="3e789-202">Run a sample application on Pi</span></span>

### <a name="install-the-prerequisite-packages"></a><span data-ttu-id="3e789-203">Az előfeltételként szükséges csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="3e789-203">Install the prerequisite packages</span></span>

<span data-ttu-id="3e789-204">A számítógép a következő SSH-ügyfél segítségével a málna Pi csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3e789-204">Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.</span></span>
   
   <span data-ttu-id="3e789-205">**Windows-felhasználók**</span><span class="sxs-lookup"><span data-stu-id="3e789-205">**Windows Users**</span></span>
   1. <span data-ttu-id="3e789-206">Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="3e789-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="3e789-207">Az IP-cím a tartományban a gazdagép nevét (vagy IP-cím) szakaszában másolja, és válassza ki az SSH a kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="3e789-207">Copy the IP address of your Pi into the Host name (or IP address) section and select SSH as the connection type.</span></span>
   
   
   <span data-ttu-id="3e789-208">**Mac és Ubuntu felhasználók**</span><span class="sxs-lookup"><span data-stu-id="3e789-208">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="3e789-209">A beépített SSH-ügyfél Ubuntu vagy macOS használja.</span><span class="sxs-lookup"><span data-stu-id="3e789-209">Use the built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="3e789-210">Előfordulhat, hogy futtatásához szükséges `ssh pi@<ip address of pi>` Pi SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3e789-210">You might need to run `ssh pi@<ip address of pi>` to connect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="3e789-211">Az alapértelmezett felhasználónév az `pi` , és a jelszó `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="3e789-211">The default username is `pi` , and the password is `raspberry`.</span></span>


### <a name="configure-the-sample-application"></a><span data-ttu-id="3e789-212">A mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3e789-212">Configure the sample application</span></span>

1. <span data-ttu-id="3e789-213">Klónozza a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3e789-213">Clone the sample application by running the following command:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. <span data-ttu-id="3e789-214">Nyissa meg a konfigurációs fájlban a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3e789-214">Open the config file by running the following commands:</span></span>

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   <span data-ttu-id="3e789-215">5 makrók van ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="3e789-215">There are 5 macros in this file you can configurate.</span></span> <span data-ttu-id="3e789-216">Az első egy `MESSAGE_TIMESPAN`, amely megadja, hogy az időtartam (ezredmásodpercben) két felhőbe küldött üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="3e789-216">The first one is `MESSAGE_TIMESPAN`, which defines the time interval (in milliseconds) between two messages that send to cloud.</span></span> <span data-ttu-id="3e789-217">A második érték `SIMULATED_DATA`, vagyis az, hogy szimulált érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="3e789-217">The second one `SIMULATED_DATA`, which is a Boolean value for whether to use simulated sensor data or not.</span></span> <span data-ttu-id="3e789-218">`I2C_ADDRESS`a I2C címet a BME280 érzékelő csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="3e789-218">`I2C_ADDRESS` is the I2C address which your BME280 sensor is connected.</span></span> <span data-ttu-id="3e789-219">`GPIO_PIN_ADDRESS`a GPIO cím szolgál a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="3e789-219">`GPIO_PIN_ADDRESS` is the GPIO address for your LED.</span></span> <span data-ttu-id="3e789-220">Az utolsót `BLINK_TIMESPAN`, amely meghatározni a TimeSpan érték, ha a LED ezredmásodpercben be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="3e789-220">The last one is `BLINK_TIMESPAN`, which defined the timespan when your LED is turned on in milliseconds.</span></span>

   <span data-ttu-id="3e789-221">Ha Ön **nem rendelkezik az érzékelő**, beállíthatja a `SIMULATED_DATA` egy érték `True` a minta kérelem létrehozása és használata a szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="3e789-221">If you **don't have the sensor**, set the `SIMULATED_DATA` value to `True` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="3e789-222">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="3e789-222">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-the-sample-application"></a><span data-ttu-id="3e789-223">Hozza létre, és futtassa a mintaalkalmazást</span><span class="sxs-lookup"><span data-stu-id="3e789-223">Build and run the sample application</span></span>

1. <span data-ttu-id="3e789-224">A minta-alkalmazás létrehozása a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="3e789-224">Build the sample application by running the following command.</span></span> <span data-ttu-id="3e789-225">Mivel az Azure IoT készült SDK-k Python burkolók az Azure IoT eszköz C SDK fölött, szüksége lesz a C-függvénytárak összeállításához, ha azt szeretné, vagy hozza létre a Python-könyvtárak forráskód kell.</span><span class="sxs-lookup"><span data-stu-id="3e789-225">Because the Azure IoT SDKs for Python are wrappers on top of the Azure IoT Device C SDK, you will need to compile the C libraries if you want or need to generate the Python libraries from source code.</span></span>

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   <span data-ttu-id="3e789-226">Azt is megadhatja a kívánt futtatásával verzió `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span><span class="sxs-lookup"><span data-stu-id="3e789-226">You can also specify the version you want by running `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span></span> <span data-ttu-id="3e789-227">Ha paraméter nélkül parancsfájlt futtatja, a parancsfájl automatikusan észleli a python telepített verzióját (keresési feladatütemezési 2.7 -> 3.4 -> 3.5).</span><span class="sxs-lookup"><span data-stu-id="3e789-227">If you run script without parameter, the script will automatically detect the version of python installed (Search sequence 2.7->3.4->3.5).</span></span> <span data-ttu-id="3e789-228">Győződjön meg arról, hogy a Python verzió tartja konzisztens során kialakításához és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3e789-228">Make sure your Python version keeps consistent during building and running.</span></span> 
   
   > [!NOTE] 
   <span data-ttu-id="3e789-229">Épület a Python ügyféloldali kódtár (iothub_client.so) kisebb, mint 1GB RAM-MAL rendelkező Linux rendszerű eszközökön, láthatja a build a lent látható módon iothub_client_python.cpp felépítésekor első Beragadt 98 % `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span><span class="sxs-lookup"><span data-stu-id="3e789-229">On building the Python client library (iothub_client.so) on Linux devices that have less than 1GB RAM, you may see build getting stuck at 98% while building iothub_client_python.cpp as shown below `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span></span> <span data-ttu-id="3e789-230">Ha ezt a problémát tapasztal, ellenőrizze az eszköz használatával a memória-felhasználás `free -m command` egy másik terminál ablakban ebben az időszakban.</span><span class="sxs-lookup"><span data-stu-id="3e789-230">If you run into this issue, check the memory consumption of the device using `free -m command` in another terminal window during that time.</span></span> <span data-ttu-id="3e789-231">Ha kevés a memória iothub_client_python.cpp fájl fordítása közben fut, akkor előfordulhat, hogy ideiglenesen növelje a lapozófájl több elérhető memória, a Python ügyféloldali SDK-könyvtár eszköz sikeresen létrehozásához lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="3e789-231">If you are running out of memory while compiling iothub_client_python.cpp file, you may have to temporarily increase the swap space to get more available memory to successfully build the Python client-side device SDK library.</span></span>
   
1. <span data-ttu-id="3e789-232">Futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3e789-232">Run the sample application by running the following command:</span></span>

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="3e789-233">Győződjön meg arról, hogy Ön-e beillesztési az eszköz kapcsolati karakterláncát azokat a szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="3e789-233">Make sure you copy-paste the device connection string into the single quotes.</span></span> <span data-ttu-id="3e789-234">Ha a python, 3, akkor a parancs használata és `python3 app.py '<your Azure IoT hub device connection string>'`.</span><span class="sxs-lookup"><span data-stu-id="3e789-234">And if you use the python 3, then you can use the command `python3 app.py '<your Azure IoT hub device connection string>'`.</span></span>


   <span data-ttu-id="3e789-235">A következő kimeneti bemutatja az érzékelő adatokat és az IoT hub küldött üzenetek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="3e789-235">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

   ![Kimeneti - érzékelő adatokat küld az IoT hub málna Pi](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a><span data-ttu-id="3e789-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e789-237">Next steps</span></span>

<span data-ttu-id="3e789-238">Egy mintaalkalmazás érzékelő adatokat gyűjteni, és küldje el az IoT hub futtatását.</span><span class="sxs-lookup"><span data-stu-id="3e789-238">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span> <span data-ttu-id="3e789-239">A málna Pi az IoT hub vagy küldési üzenetek a málna Pi egy parancssori felület a küldött üzenetek, olvassa el a [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="3e789-239">To see the messages that your Raspberry Pi has sent to your IoT hub or send messages to your Raspberry Pi in a command line interface, see the [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
