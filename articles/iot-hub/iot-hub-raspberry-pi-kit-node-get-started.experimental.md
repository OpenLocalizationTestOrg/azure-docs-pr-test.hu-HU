---
title: "A felhő (Node.js) - málna Pi csatlakozzon az Azure IoT Hub Raspberry Pi |} Microsoft Docs"
description: "Csatlakozás málna Pi Azure IoT-központ málna Pi Azure felhőben történő adatküldéshez."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot raspberry pi, raspberry pi iot-központ, raspberry pi adatokat küldött a felhőben, raspberry pi felhőbe"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.openlocfilehash: 956ed5ab0ed38ddebd978b35eb54bc96567f0d57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a><span data-ttu-id="071fb-104">Raspberry Pi csatlakozni az Azure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="071fb-104">Connect Raspberry Pi to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="071fb-105">Ebben az oktatóanyagban akkor először tanulás alapjainak málna Pi Raspbian futtató használata.</span><span class="sxs-lookup"><span data-stu-id="071fb-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="071fb-106">Majd megtudhatja, hogyan kapcsolódhat zökkenőmentesen az eszközök a felhőbe [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="071fb-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="071fb-107">Windows 10 IoT minta, látogasson el a [Windows fejlesztői központ](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="071fb-107">For Windows 10 IoT Core samples, go to the [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="071fb-108">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="071fb-108">Don't have a kit yet?</span></span> <span data-ttu-id="071fb-109">Próbálja meg a [málna Pi 3 emulátor](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span><span class="sxs-lookup"><span data-stu-id="071fb-109">Try the [Raspberry Pi 3 emulator](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span></span> <span data-ttu-id="071fb-110">Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="071fb-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="071fb-111">Mit</span><span class="sxs-lookup"><span data-stu-id="071fb-111">What you do</span></span>

* <span data-ttu-id="071fb-112">A telepítő Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="071fb-112">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="071fb-113">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="071fb-113">Create an IoT hub.</span></span>
* <span data-ttu-id="071fb-114">Eszköz regisztrálása az IoT hub a pi.</span><span class="sxs-lookup"><span data-stu-id="071fb-114">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="071fb-115">Futtassa a mintaalkalmazást érzékelő adatokat küldeni az IoT hub Pi.</span><span class="sxs-lookup"><span data-stu-id="071fb-115">Run a sample application on Pi to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="071fb-116">Az IoT-központ az Ön által létrehozott málna Pi csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="071fb-116">Connect Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="071fb-117">Majd futtassa a mintaalkalmazást a hőmérséklet és a páratartalom adatokat gyűjteni BME280 érzékelő Pi.</span><span class="sxs-lookup"><span data-stu-id="071fb-117">Then you run a sample application on Pi to collect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="071fb-118">Végezetül az érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="071fb-118">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="071fb-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="071fb-119">What you learn</span></span>

* <span data-ttu-id="071fb-120">Megtudhatja, hogyan hozzon létre egy Azure IoT-központot, és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="071fb-120">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="071fb-121">Hogyan BME280 érzékelő Pi kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="071fb-121">How to connect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="071fb-122">Megtudhatja, hogyan futtatja a mintaalkalmazás Pi érzékelő adatok gyűjtéséért felelős ügyfélfeladatot.</span><span class="sxs-lookup"><span data-stu-id="071fb-122">How to collect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="071fb-123">Hogyan érzékelő adatokat küldeni az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="071fb-123">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="071fb-124">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="071fb-124">What you need</span></span>

![Mi szükséges](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="071fb-126">A Pi 2 málna vagy málna Pi 3 kártya.</span><span class="sxs-lookup"><span data-stu-id="071fb-126">The Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="071fb-127">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="071fb-127">An active Azure subscription.</span></span> <span data-ttu-id="071fb-128">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="071fb-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="071fb-129">A figyelő egy USB-billentyűzet és egér Pi-hez.</span><span class="sxs-lookup"><span data-stu-id="071fb-129">A monitor, a USB keyboard, and mouse that connect to Pi.</span></span>
* <span data-ttu-id="071fb-130">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="071fb-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="071fb-131">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="071fb-131">An Internet connection.</span></span>
* <span data-ttu-id="071fb-132">Egy 16 GB-os vagy újabb microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="071fb-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="071fb-133">Egy USB-SD adapter vagy microSD-kártyán írása a microSD-kártyán operációsrendszer-képet.</span><span class="sxs-lookup"><span data-stu-id="071fb-133">A USB-SD adapter or microSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="071fb-134">Egy 5-volt 2-amp tápegység a 6-mértékű kiszolgálóhasználat micro USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="071fb-134">A 5-volt 2-amp power supply with the 6-foot micro USB cable.</span></span>

<span data-ttu-id="071fb-135">A következő elemek nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="071fb-135">The following items are optional:</span></span>

* <span data-ttu-id="071fb-136">Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="071fb-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="071fb-137">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="071fb-137">A breadboard.</span></span>
* <span data-ttu-id="071fb-138">6/aljzat átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="071fb-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="071fb-139">Egy szórt 10 mm LED-jét.</span><span class="sxs-lookup"><span data-stu-id="071fb-139">A diffused 10-mm LED.</span></span>

  > [!NOTE] 
  <span data-ttu-id="071fb-140">Ezek az elemek nem kötelező, mert a kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="071fb-140">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="071fb-141">A telepítő Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="071fb-141">Setup Raspberry Pi</span></span>

### <a name="install-the-raspbian-operating-system-for-pi"></a><span data-ttu-id="071fb-142">A Pi a Raspbian operációs rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="071fb-142">Install the Raspbian operating system for Pi</span></span>

<span data-ttu-id="071fb-143">Készítse elő a microSD-kártyán Raspbian kép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="071fb-143">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="071fb-144">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="071fb-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="071fb-145">[Töltse le a Pixel Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (a .zip-fájlt).</span><span class="sxs-lookup"><span data-stu-id="071fb-145">[Download Raspbian Jessie with Pixel](https://www.raspberrypi.org/downloads/raspbian/) (the .zip file).</span></span>
   1. <span data-ttu-id="071fb-146">Bontsa ki a Raspbian lemezképet a számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="071fb-146">Extract the Raspbian image to a folder on your computer.</span></span>
1. <span data-ttu-id="071fb-147">A microSD-kártyán Raspbian telepítése.</span><span class="sxs-lookup"><span data-stu-id="071fb-147">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="071fb-148">[Töltse le és telepítse a Etcher SD-kártya író segédprogram](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="071fb-148">[Download and install the Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="071fb-149">Futtassa a Etcher, és az 1. lépésben válassza ki a kibontott Raspbian kép.</span><span class="sxs-lookup"><span data-stu-id="071fb-149">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="071fb-150">Jelölje ki a microSD-kártyát meghajtót.</span><span class="sxs-lookup"><span data-stu-id="071fb-150">Select the microSD card drive.</span></span> <span data-ttu-id="071fb-151">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott ki a megfelelő meghajtó.</span><span class="sxs-lookup"><span data-stu-id="071fb-151">Note that Etcher may have already selected the correct drive.</span></span>
   1. <span data-ttu-id="071fb-152">Kattintson a microSD-kártyán Raspbian telepítése Flash.</span><span class="sxs-lookup"><span data-stu-id="071fb-152">Click Flash to install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="071fb-153">Eltávolítja a számítógépről a microSD-kártyán, ha a telepítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="071fb-153">Remove the microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="071fb-154">Biztonságos a microSD-kártyán közvetlenül eltávolítani, mert Etcher automatikusan kiadása vagy leválasztja a microSD-kártyán befejezése után is.</span><span class="sxs-lookup"><span data-stu-id="071fb-154">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   1. <span data-ttu-id="071fb-155">A microSD-kártyán beszúrása Pi.</span><span class="sxs-lookup"><span data-stu-id="071fb-155">Insert the microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="071fb-156">SSH- és I2C engedélyezése</span><span class="sxs-lookup"><span data-stu-id="071fb-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="071fb-157">Pi csatlakozni a monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` felhasználónevet és `raspberry` a jelszót.</span><span class="sxs-lookup"><span data-stu-id="071fb-157">Connect Pi to the monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as the user name and `raspberry` as the password.</span></span>
1. <span data-ttu-id="071fb-158">Kattintson a Raspberry ikonra > **beállítások** > **málna Pi konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="071fb-158">Click the Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![A Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="071fb-160">Az a **felületek** lapon **I2C** és **SSH** való **engedélyezése**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="071fb-160">On the **Interfaces** tab, set **I2C** and **SSH** to **Enable**, and then click **OK**.</span></span> <span data-ttu-id="071fb-161">Ha nem rendelkezik a fizikai érzékelők és szimulált érzékelőadatait használni szeretne, ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="071fb-161">If you don't have physical sensors and want to use simulated sensor data, this step is optional.</span></span>

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="071fb-163">SSH- és I2C engedélyezéséhez található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="071fb-163">To enable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-the-sensor-to-pi"></a><span data-ttu-id="071fb-164">Csatlakozás az érzékelő Pi</span><span class="sxs-lookup"><span data-stu-id="071fb-164">Connect the sensor to Pi</span></span>

<span data-ttu-id="071fb-165">Használják a breadboard és átkötés LED és egy BME280 Pi kell kapcsolódniuk az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="071fb-165">Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows.</span></span> <span data-ttu-id="071fb-166">Ha még nem rendelkezik az érzékelő, hagyja ki ezt a szakaszt.</span><span class="sxs-lookup"><span data-stu-id="071fb-166">If you don’t have the sensor, skip this section.</span></span>

![A Pi málna és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="071fb-168">A BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="071fb-168">The BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="071fb-169">És a LED fog villogni, ha egy eszköz és a felhő közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="071fb-169">And the LED will blink if there is a communication between device and the cloud.</span></span> 

<span data-ttu-id="071fb-170">PIN-kód érzékelő használja a következő vezetékezést:</span><span class="sxs-lookup"><span data-stu-id="071fb-170">For sensor pins, use the following wiring:</span></span>

| <span data-ttu-id="071fb-171">Indítsa el a (érzékelő & LED)</span><span class="sxs-lookup"><span data-stu-id="071fb-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="071fb-172">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="071fb-172">End (Board)</span></span>            | <span data-ttu-id="071fb-173">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="071fb-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="071fb-174">VDD (PIN-kód 5 g.)</span><span class="sxs-lookup"><span data-stu-id="071fb-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="071fb-175">3.3V PWR (1. Pin)</span><span class="sxs-lookup"><span data-stu-id="071fb-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="071fb-176">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-176">White cable</span></span>   |
| <span data-ttu-id="071fb-177">GND (7G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="071fb-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="071fb-178">GND (PIN-kód 6)</span><span class="sxs-lookup"><span data-stu-id="071fb-178">GND (Pin 6)</span></span>            | <span data-ttu-id="071fb-179">Barna kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-179">Brown cable</span></span>   |
| <span data-ttu-id="071fb-180">SCK (8G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="071fb-180">SCK (Pin 8G)</span></span>             | <span data-ttu-id="071fb-181">I2C1 SDA (3 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="071fb-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="071fb-182">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-182">Orange cable</span></span>  |
| <span data-ttu-id="071fb-183">SDI (PIN-kód 10G)</span><span class="sxs-lookup"><span data-stu-id="071fb-183">SDI (Pin 10G)</span></span>            | <span data-ttu-id="071fb-184">I2C1 SCL (PIN-kód 5)</span><span class="sxs-lookup"><span data-stu-id="071fb-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="071fb-185">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-185">Red cable</span></span>     |
| <span data-ttu-id="071fb-186">LED VDD (PIN-kód 18F)</span><span class="sxs-lookup"><span data-stu-id="071fb-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="071fb-187">GPIO 24 (PIN-kód 18)</span><span class="sxs-lookup"><span data-stu-id="071fb-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="071fb-188">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-188">White cable</span></span>   |
| <span data-ttu-id="071fb-189">LED GND (PIN-kód 17F)</span><span class="sxs-lookup"><span data-stu-id="071fb-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="071fb-190">GND (PIN-kód 20)</span><span class="sxs-lookup"><span data-stu-id="071fb-190">GND (Pin 20)</span></span>           | <span data-ttu-id="071fb-191">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="071fb-191">Black cable</span></span>   |

<span data-ttu-id="071fb-192">Megjelenítéséhez kattintson ide [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="071fb-192">Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="071fb-193">Miután sikeresen csatlakozott a málna Pi BME280, meg kell például a kép alatt.</span><span class="sxs-lookup"><span data-stu-id="071fb-193">After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.</span></span>

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a><span data-ttu-id="071fb-195">A Pi csatlakoznak a hálózathoz</span><span class="sxs-lookup"><span data-stu-id="071fb-195">Connect Pi to the network</span></span>

<span data-ttu-id="071fb-196">Kapcsolja be a Pi a micro USB-kábelen és a tápegység.</span><span class="sxs-lookup"><span data-stu-id="071fb-196">Turn on Pi by using the micro USB cable and the power supply.</span></span> <span data-ttu-id="071fb-197">Az Ethernet-kábel segítségével Pi csatlakozni a vezetékes hálózatra, vagy hajtsa végre a [málna Pi alapját utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi a vezeték nélküli hálózathoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="071fb-197">Use the Ethernet cable to connect Pi to your wired network or follow the [instructions from the Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) to connect Pi to your wireless network.</span></span> <span data-ttu-id="071fb-198">Miután a Pi sikeresen csatlakozott a hálózathoz, azt kell jegyezze fel az a [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="071fb-198">After your Pi has been successfully connected to the network, you need to take a note of the [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Vezetékes hálózathoz csatlakoznak](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="071fb-200">Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="071fb-200">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="071fb-201">Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="071fb-201">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="071fb-202">A Pi mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="071fb-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a><span data-ttu-id="071fb-203">Klónozza a mintaalkalmazást, és az előfeltételként szükséges csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="071fb-203">Clone sample application and install the prerequisite packages</span></span>

1. <span data-ttu-id="071fb-204">A számítógép a következő SSH-ügyfél segítségével a málna Pi csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="071fb-204">Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.</span></span>
    - <span data-ttu-id="071fb-205">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="071fb-205">[PuTTY](http://www.putty.org/) for Windows.</span></span> <span data-ttu-id="071fb-206">A Pi SSH-kapcsolaton keresztül csatlakozzanak IP-címe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="071fb-206">You need the IP address of your Pi to connect it via SSH.</span></span>
    - <span data-ttu-id="071fb-207">A beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="071fb-207">The built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="071fb-208">Előfordulhat, hogy kell futtatnia `ssh pi@<ip address of pi>` Pi SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="071fb-208">You might need run `ssh pi@<ip address of pi>` to connect Pi via SSH.</span></span>

   > [!NOTE] 
   <span data-ttu-id="071fb-209">Az alapértelmezett felhasználónév az `pi` , és a jelszó `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="071fb-209">The default username is `pi` , and the password is `raspberry`.</span></span>

1. <span data-ttu-id="071fb-210">A Pi Node.js és NPM telepíthető.</span><span class="sxs-lookup"><span data-stu-id="071fb-210">Install Node.js and NPM to your Pi.</span></span>
   
   <span data-ttu-id="071fb-211">Először ellenőrizze a Node.js-verzió a következő paranccsal.</span><span class="sxs-lookup"><span data-stu-id="071fb-211">First you should check your Node.js version with the following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="071fb-212">Ha verziószáma kisebb, mint a 4.x-es, vagy nincs nem Node.js a Pi, majd futtassa a következő parancs futtatásával telepítse vagy frissítse a Node.js.</span><span class="sxs-lookup"><span data-stu-id="071fb-212">If the version is lower than 4.x or there is no Node.js on your Pi, then run the following command to install or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="071fb-213">Klónozza a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="071fb-213">Clone the sample application by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="071fb-214">A következő parancsot az összes csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="071fb-214">Install all packages by the following command.</span></span> <span data-ttu-id="071fb-215">Ez magában foglalja az Azure IoT-eszközök SDK, BME280 érzékelő és huzalozási Pi függvénytárak.</span><span class="sxs-lookup"><span data-stu-id="071fb-215">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="071fb-216">A hálózati kapcsolatban a telepítési folyamat denpening több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="071fb-216">It might take several minutes to finish this installation process denpening on your network connection.</span></span>

### <a name="configure-the-sample-application"></a><span data-ttu-id="071fb-217">A mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="071fb-217">Configure the sample application</span></span>

1. <span data-ttu-id="071fb-218">Nyissa meg a konfigurációs fájlban a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="071fb-218">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="071fb-220">Két elem ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="071fb-220">There are two items in this file you can configurate.</span></span> <span data-ttu-id="071fb-221">Az első egy `interval`, amely megadja, hogy két felhőbe küldött üzeneteket közötti időközt.</span><span class="sxs-lookup"><span data-stu-id="071fb-221">The first one is `interval`, which defines the time interval between two messages that send to cloud.</span></span> <span data-ttu-id="071fb-222">A második érték `simulatedData`, vagyis az, hogy szimulált érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="071fb-222">The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="071fb-223">Ha Ön **nem rendelkezik az érzékelő**, beállíthatja a `simulatedData` egy érték `true` a minta kérelem létrehozása és használata a szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="071fb-223">If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="071fb-224">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="071fb-224">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-the-sample-application"></a><span data-ttu-id="071fb-225">Futtassa a mintaalkalmazást</span><span class="sxs-lookup"><span data-stu-id="071fb-225">Run the sample application</span></span>

1. <span data-ttu-id="071fb-226">Futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="071fb-226">Run the sample application by running the following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="071fb-227">Győződjön meg arról, hogy Ön-e beillesztési az eszköz kapcsolati karakterláncát azokat a szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="071fb-227">Make sure you copy-paste the device connection string into the single quotes.</span></span>


<span data-ttu-id="071fb-228">A következő kimeneti bemutatja az érzékelő adatokat és az IoT hub küldött üzenetek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="071fb-228">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Kimeneti - érzékelő adatokat küld az IoT hub málna Pi](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="071fb-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="071fb-230">Next steps</span></span>

<span data-ttu-id="071fb-231">Egy mintaalkalmazás érzékelő adatokat gyűjteni, és küldje el az IoT hub futtatását.</span><span class="sxs-lookup"><span data-stu-id="071fb-231">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]